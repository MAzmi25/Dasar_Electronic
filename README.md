<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


  { "en": "Apa Itu Arus Lemah?", "id": "Arus Listrik Dengan Daya Relatif Kecil." },
  { "en": "Apa Itu Arus Listrik?", "id": "Aliran Muatan Listrik Dalam Suatu Rangkaian." },
  { "en": "Apa Satuan Arus Listrik?", "id": "Ampere (A)." },
  { "en": "Apa Itu Tegangan Listrik?", "id": "Beda Potensial Antara Dua Titik." },
  { "en": "Apa Satuan Tegangan Listrik?", "id": "Volt (V)." },
  { "en": "Apa Itu Resistansi?", "id": "Hambatan Terhadap Aliran Arus Listrik." },
  { "en": "Apa Satuan Resistansi?", "id": "Ohm (Ω)." },
  { "en": "Apa Itu Hukum Ohm?", "id": "Hubungan Antara Tegangan, Arus, Dan Resistansi." },
  { "en": "Apa Itu Daya Listrik?", "id": "Laju Energi Listrik Yang Digunakan." },
  { "en": "Apa Satuan Daya Listrik?", "id": "Watt (W)." },
  { "en": "Apa Itu Energi Listrik?", "id": "Daya Yang Digunakan Selama Waktu Tertentu." },
  { "en": "Apa Satuan Energi Listrik?", "id": "Joule (J) Atau Watt-Hour (Wh)." },
  { "en": "Apa Itu Konduktor?", "id": "Bahan Yang Mudah Menghantarkan Listrik." },
  { "en": "Apa Itu Isolator?", "id": "Bahan Yang Sulit Menghantarkan Listrik." },
  { "en": "Apa Itu Semikonduktor?", "id": "Bahan Di Antara Konduktor Dan Isolator." },
  { "en": "Apa Itu Rangkaian Seri?", "id": "Komponen Terhubung Dalam Satu Jalur." },
  { "en": "Resistansi Total Rangkaian Seri?", "id": "Jumlah Dari Semua Resistor." },
  { "en": "Apa Itu Rangkaian Paralel?", "id": "Komponen Terhubung Dalam Jalur Bercabang." },
  { "en": "Resistansi Total Rangkaian Paralel?", "id": "Lebih Kecil Dari Resistor Terkecil." },
  { "en": "Apa Itu Hukum Arus Kirchhoff (KCL)?", "id": "Arus Masuk Simpul Sama Dengan Arus Keluar." },
  { "en": "Apa Itu Hukum Tegangan Kirchhoff (KVL)?", "id": "Jumlah Tegangan Dalam Loop Tertutup Adalah Nol." },
  { "en": "Apa Itu Simpul (Node)?", "id": "Titik Pertemuan Tiga Atau Lebih Komponen." },
  { "en": "Apa Itu Loop?", "id": "Jalur Tertutup Dalam Suatu Rangkaian." },
  { "en": "Apa Itu Arus Searah (DC)?", "id": "Direct Current, Arus Mengalir Satu Arah." },
  { "en": "Apa Itu Arus Bolak-Balik (AC)?", "id": "Alternating Current, Arah Arus Berubah Periodik." },
  { "en": "Apa Itu Resistor?", "id": "Komponen Pasif Yang Menghambat Arus." },
  { "en": "Apa Fungsi Resistor?", "id": "Membatasi Arus, Membagi Tegangan." },
  { "en": "Apa Itu Kode Warna Resistor?", "id": "Menentukan Nilai Resistansi Dan Toleransi." },
  { "en": "Apa Itu Toleransi Resistor?", "id": "Batas Penyimpangan Nilai Resistansi." },
  { "en": "Apa Itu Potensiometer?", "id": "Resistor Variabel Dengan Tiga Terminal." },
  { "en": "Apa Itu Kapasitor?", "id": "Komponen Pasif Yang Menyimpan Energi Listrik." },
  { "en": "Bagaimana Kapasitor Menyimpan Energi?", "id": "Dalam Bentuk Medan Listrik." },
  { "en": "Apa Itu Kapasitansi?", "id": "Kemampuan Kapasitor Menyimpan Muatan." },
  { "en": "Apa Satuan Kapasitansi?", "id": "Farad (F)." },
  { "en": "Kapasitor Seri?", "id": "Total Kapasitansi Lebih Kecil." },
  { "en": "Kapasitor Paralel?", "id": "Total Kapasitansi Adalah Jumlahnya." },
  { "en": "Apa Itu Induktor?", "id": "Komponen Pasif Yang Menyimpan Energi Magnetik." },
  { "en": "Bagaimana Induktor Menyimpan Energi?", "id": "Dalam Bentuk Medan Magnet." },
  { "en": "Apa Itu Induktansi?", "id": "Kemampuan Induktor Menghasilkan GGL Induksi." },
  { "en": "Apa Satuan Induktansi?", "id": "Henry (H)." },
  { "en": "Induktor Seri?", "id": "Total Induktansi Adalah Jumlahnya." },
  { "en": "Induktor Paralel?", "id": "Total Induktansi Lebih Kecil." },
  { "en": "Apa Itu Impedansi (Z)?", "id": "Hambatan Total Rangkaian AC." },
  { "en": "Apa Itu Reaktansi (X)?", "id": "Hambatan Akibat Kapasitor Atau Induktor." },
  { "en": "Apa Itu Reaktansi Kapasitif (Xc)?", "id": "Hambatan Kapasitor Terhadap Arus AC." },
  { "en": "Apa Itu Reaktansi Induktif (Xl)?", "id": "Hambatan Induktor Terhadap Arus AC." },
  { "en": "Apa Itu Rangkaian RC?", "id": "Rangkaian Yang Terdiri Dari Resistor Kapasitor." },
  { "en": "Apa Itu Rangkaian RL?", "id": "Rangkaian Yang Terdiri Dari Resistor Induktor." },
  { "en": "Apa Itu Rangkaian RLC?", "id": "Rangkaian Dengan Resistor, Induktor, Kapasitor." },
  { "en": "Apa Itu Konstanta Waktu?", "id": "Ukuran Kecepatan Respon Rangkaian RC RL." },
  { "en": "Apa Itu Frekuensi?", "id": "Jumlah Siklus Per Detik." },
  { "en": "Apa Satuan Frekuensi?", "id": "Hertz (Hz)." },
  { "en": "Apa Itu Periode?", "id": "Waktu Untuk Satu Siklus Penuh." },
  { "en": "Apa Itu Amplitudo?", "id": "Nilai Puncak Dari Suatu Gelombang." },
  { "en": "Apa Itu Fasa?", "id": "Posisi Relatif Gelombang Dalam Waktu." },
  { "en": "Apa Itu Nilai RMS?", "id": "Root Mean Square, Nilai Efektif AC." },
  { "en": "Apa Itu Teorema Superposisi?", "id": "Analisis Rangkaian Dengan Satu Sumber Aktif." },
  { "en": "Apa Itu Teorema Thevenin?", "id": "Menyederhanakan Rangkaian Menjadi Sumber Tegangan Ekuivalen." },
  { "en": "Apa Itu Teorema Norton?", "id": "Menyederhanakan Rangkaian Menjadi Sumber Arus Ekuivalen." },
  { "en": "Apa Itu Transformasi Sumber?", "id": "Mengubah Antara Sumber Thevenin Dan Norton." },
  { "en": "Apa Itu Ground (Tanah)?", "id": "Titik Referensi Tegangan Nol." },
  { "en": "Apa Itu Sirkuit Terbuka (Open Circuit)?", "id": "Jalur Terputus, Arus Tidak Mengalir." },
  { "en": "Apa Itu Hubung Singkat (Short Circuit)?", "id": "Jalur Resistansi Rendah, Arus Sangat Besar." },
  { "en": "Apa Itu Saklar?", "id": "Komponen Untuk Membuka Dan Menutup Rangkaian." },
  { "en": "Apa Itu Sekering (Fuse)?", "id": "Melindungi Rangkaian Dari Arus Berlebih." },
  { "en": "Apa Itu Circuit Breaker?", "id": "Saklar Pengaman Yang Dapat Direset." },
  { "en": "Apa Itu Sumber Tegangan Ideal?", "id": "Memberikan Tegangan Konstan Berapapun Arusnya." },
  { "en": "Apa Itu Sumber Arus Ideal?", "id": "Memberikan Arus Konstan Berapapun Tegangannya." },
  { "en": "Apa Itu Sumber Dependen?", "id": "Nilainya Bergantung Pada Tegangan Arus Lain." },
  { "en": "Apa Itu Multimeter?", "id": "Alat Ukur Listrik Serbaguna." },
  { "en": "Apa Itu Voltmeter?", "id": "Alat Untuk Mengukur Tegangan." },
  { "en": "Bagaimana Memasang Voltmeter?", "id": "Dipasang Secara Paralel Dengan Komponen." },
  { "en": "Apa Itu Amperemeter?", "id": "Alat Untuk Mengukur Arus." },
  { "en": "Bagaimana Memasang Amperemeter?", "id": "Dipasang Secara Seri Dalam Rangkaian." },
  { "en": "Apa Itu Ohmmeter?", "id": "Alat Untuk Mengukur Resistansi." },
  { "en": "Apa Itu Osiloskop?", "id": "Menampilkan Bentuk Gelombang Sinyal Listrik." },
  { "en": "Apa Itu Function Generator?", "id": "Menghasilkan Berbagai Bentuk Gelombang Sinyal." },
  { "en": "Apa Itu Power Supply?", "id": "Menyediakan Sumber Daya DC Untuk Rangkaian." },
  { "en": "Apa Itu PCB (Printed Circuit Board)?", "id": "Papan Sirkuit Cetak." },
  { "en": "Apa Fungsi PCB?", "id": "Menghubungkan Komponen Elektronik Secara Fisik." },
  { "en": "Apa Itu Breadboard?", "id": "Papan Untuk Merangkai Prototipe Sirkuit." },
  { "en": "Apa Itu Solder?", "id": "Logam Cair Untuk Menyambung Komponen." },
  { "en": "Apa Itu Desoldering?", "id": "Proses Melepas Komponen Yang Disolder." },
  { "en": "Apa Itu Konduktansi?", "id": "Kebalikan Dari Resistansi." },
  { "en": "Apa Itu Admitansi?", "id": "Kebalikan Dari Impedansi." },
  { "en": "Apa Itu Susceptansi?", "id": "Bagian Imajiner Dari Admitansi." },
  { "en": "Apa Itu Resonansi?", "id": "Saat Reaktansi Induktif Sama Dengan Kapasitif." },
  { "en": "Apa Itu Bandwidth?", "id": "Rentang Frekuensi Operasi Suatu Sistem." },
  { "en": "Apa Itu Faktor Kualitas (Q)?", "id": "Ukuran Selektivitas Rangkaian Resonansi." },
  { "en": "Apa Itu Filter?", "id": "Rangkaian Yang Melewatkan Frekuensi Tertentu." },
  { "en": "Apa Itu Filter Low-Pass?", "id": "Melewatkan Frekuensi Rendah." },
  { "en": "Apa Itu Filter High-Pass?", "id": "Melewatkan Frekuensi Tinggi." },
  { "en": "Apa Itu Filter Band-Pass?", "id": "Melewatkan Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Filter Band-Stop?", "id": "Meredam Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Semikonduktor?", "id": "Material Antara Konduktor Dan Isolator." },
  { "en": "Contoh Material Semikonduktor?", "id": "Silikon (Si), Germanium (Ge)." },
  { "en": "Apa Itu Doping?", "id": "Menambahkan Atom Pengotor Ke Semikonduktor." },
  { "en": "Tujuan Doping?", "id": "Mengubah Sifat Konduktivitas Semikonduktor." },
  { "en": "Apa Itu Semikonduktor Intrinsik?", "id": "Semikonduktor Murni Tanpa Doping." },
  { "en": "Apa Itu Semikonduktor Ekstrinsik?", "id": "Semikonduktor Yang Telah Didoping." },
  { "en": "Apa Itu Semikonduktor Tipe-N?", "id": "Pembawa Muatan Mayoritasnya Adalah Elektron." },
  { "en": "Apa Itu Semikonduktor Tipe-P?", "id": "Pembawa Muatan Mayoritasnya Adalah Hole." },
  { "en": "Apa Itu Hole?", "id": "Kekosongan Elektron Yang Bertindak Muatan Positif." },
  { "en": "Apa Itu Sambungan P-N (P-N Junction)?", "id": "Batas Antara Material Tipe-P Tipe-N." },
  { "en": "Apa Itu Dioda?", "id": "Komponen Yang Terbuat Dari Sambungan P-N." },
  { "en": "Fungsi Utama Dioda?", "id": "Hanya Melewatkan Arus Dalam Satu Arah." },
  { "en": "Apa Itu Anoda?", "id": "Terminal Positif Dioda (Sisi P)." },
  { "en": "Apa Itu Katoda?", "id": "Terminal Negatif Dioda (Sisi N)." },
  { "en": "Apa Itu Daerah Deplesi?", "id": "Area Di Sekitar Sambungan Tanpa Pembawa Muatan." },
  { "en": "Apa Itu Bias Maju (Forward Bias)?", "id": "Memberi Tegangan Positif Ke Anoda." },
  { "en": "Efek Bias Maju?", "id": "Dioda Menghantar Arus." },
  { "en": "Apa Itu Bias Mundur (Reverse Bias)?", "id": "Memberi Tegangan Negatif Ke Anoda." },
  { "en": "Efek Bias Mundur?", "id": "Dioda Menghambat Arus." },
  { "en": "Apa Itu Tegangan Cut-in (Knee Voltage)?", "id": "Tegangan Minimum Agar Dioda Menghantar." },
  { "en": "Tegangan Cut-in Untuk Silikon?", "id": "Sekitar 0.7 Volt." },
  { "en": "Tegangan Cut-in Untuk Germanium?", "id": "Sekitar 0.3 Volt." },
  { "en": "Apa Itu Arus Bocor (Leakage Current)?", "id": "Arus Sangat Kecil Saat Bias Mundur." },
  { "en": "Apa Itu Tegangan Breakdown?", "id": "Tegangan Mundur Maksimum Sebelum Dioda Rusak." },
  { "en": "Apa Itu Penyearahan (Rectification)?", "id": "Proses Mengubah AC Menjadi DC." },
  { "en": "Apa Itu Penyearah Setengah Gelombang?", "id": "Hanya Menggunakan Setengah Siklus AC." },
  { "en": "Apa Itu Penyearah Gelombang Penuh?", "id": "Menggunakan Seluruh Siklus AC." },
  { "en": "Apa Itu Penyearah Jembatan?", "id": "Jenis Penyearah Gelombang Penuh Paling Umum." },
  { "en": "Berapa Dioda Pada Penyearah Jembatan?", "id": "Empat Buah Dioda." },
  { "en": "Apa Itu Ripple?", "id": "Sisa Variasi AC Pada Output DC." },
  { "en": "Bagaimana Mengurangi Ripple?", "id": "Menggunakan Kapasitor Filter." },
  { "en": "Apa Itu Dioda Zener?", "id": "Dioda Khusus Yang Bekerja Di Daerah Breakdown." },
  { "en": "Fungsi Dioda Zener?", "id": "Sebagai Regulator Tegangan." },
  { "en": "Apa Itu Regulator Tegangan?", "id": "Menjaga Tegangan Output Tetap Konstan." },
  { "en": "Apa Itu LED (Light Emitting Diode)?", "id": "Dioda Yang Memancarkan Cahaya Saat Dialiri Arus." },
  { "en": "Bagaimana LED Bekerja?", "id": "Rekombinasi Elektron Dan Hole Melepas Energi Cahaya." },
  { "en": "Apa Itu Fotodioda?", "id": "Dioda Yang Peka Terhadap Cahaya." },
  { "en": "Bagaimana Fotodioda Bekerja?", "id": "Cahaya Menghasilkan Pasangan Elektron-Hole." },
  { "en": "Apa Itu Sel Surya (Solar Cell)?", "id": "Dioda P-N Area Besar Untuk Mengubah Cahaya." },
  { "en": "Apa Itu Dioda Varactor (Varicap)?", "id": "Kapasitansi Dioda Yang Dapat Diatur Tegangan." },
  { "en": "Aplikasi Dioda Varactor?", "id": "Tuning Rangkaian Radio." },
  { "en": "Apa Itu Dioda Schottky?", "id": "Sambungan Metal-Semikonduktor." },
  { "en": "Keuntungan Dioda Schottky?", "id": "Tegangan Maju Rendah, Switching Cepat." },
  { "en": "Aplikasi Dioda Schottky?", "id": "Catu Daya Switching, Rangkaian Frekuensi Tinggi." },
  { "en": "Apa Itu Dioda Tunnel?", "id": "Dioda Dengan Efek Terowongan Kuantum." },
  { "en": "Karakteristik Dioda Tunnel?", "id": "Memiliki Daerah Resistansi Negatif." },
  { "en": "Apa Itu Dioda PIN?", "id": "Dioda Dengan Lapisan Intrinsik Di Tengah." },
  { "en": "Aplikasi Dioda PIN?", "id": "Saklar Frekuensi Tinggi, Attenuator." },
  { "en": "Apa Itu Optocoupler (Opto-isolator)?", "id": "Mengisolasi Dua Rangkaian Menggunakan Cahaya." },
  { "en": "Komponen Optocoupler?", "id": "LED Dan Fototransistor Dalam Satu Paket." },
  { "en": "Apa Itu Clipper (Limiter)?", "id": "Rangkaian Dioda Untuk Memotong Bagian Sinyal." },
  { "en": "Apa Itu Clamper (DC Restorer)?", "id": "Rangkaian Dioda Untuk Menggeser Level DC Sinyal." },
  { "en": "Apa Itu Pengganda Tegangan?", "id": "Rangkaian Dioda-Kapasitor Untuk Menaikkan Tegangan DC." },
  { "en": "Apa Itu Transistor?", "id": "Komponen Semikonduktor Aktif." },
  { "en": "Fungsi Utama Transistor?", "id": "Sebagai Saklar Atau Penguat (Amplifier)." },
  { "en": "Dua Jenis Utama Transistor?", "id": "BJT Dan FET." },
  { "en": "Apa Itu BJT?", "id": "Bipolar Junction Transistor." },
  { "en": "Struktur BJT?", "id": "Dua Sambungan P-N (NPN Atau PNP)." },
  { "en": "Tiga Terminal BJT?", "id": "Emitor, Basis, Kolektor." },
  { "en": "BJT Adalah Perangkat Terkendali Apa?", "id": "Terkendali Arus (Current-Controlled)." },
  { "en": "Arus Basis BJT?", "id": "Mengontrol Arus Yang Jauh Lebih Besar." },
  { "en": "Apa Itu FET?", "id": "Field-Effect Transistor." },
  { "en": "FET Adalah Perangkat Terkendali Apa?", "id": "Terkendali Tegangan (Voltage-Controlled)." },
  { "en": "Keuntungan FET Dibanding BJT?", "id": "Impedansi Input Sangat Tinggi." },
  { "en": "Dua Jenis Utama FET?", "id": "JFET Dan MOSFET." },
  { "en": "Apa Itu JFET?", "id": "Junction Field-Effect Transistor." },
  { "en": "Apa Itu MOSFET?", "id": "Metal-Oxide-Semiconductor Field-Effect Transistor." },
  { "en": "Tiga Terminal FET?", "id": "Source, Gate, Drain." },
  { "en": "Apa Itu Sirkuit Terpadu (IC)?", "id": "Integrated Circuit." },
  { "en": "Apa Itu IC?", "id": "Jutaan Transistor Dan Komponen Lainnya." },
  { "en": "Apa Itu Skala Integrasi?", "id": "Menunjukkan Jumlah Komponen Per Chip." },
  { "en": "Contoh Skala Integrasi?", "id": "SSI, MSI, LSI, VLSI." },
  { "en": "Apa Itu VLSI?", "id": "Very Large Scale Integration." },
  { "en": "Apa Itu Hukum Moore?", "id": "Jumlah Transistor Pada Chip Berlipat Ganda." },
  { "en": "Apa Itu Fabrikasi Semikonduktor?", "id": "Proses Pembuatan Sirkuit Terpadu." },
  { "en": "Apa Itu Wafer Silikon?", "id": "Bahan Dasar Untuk Membuat Chip IC." },
  { "en": "Apa Itu Fotolitografi?", "id": "Proses Transfer Pola Ke Wafer." },
  { "en": "Apa Itu Etsa (Etching)?", "id": "Menghilangkan Material Secara Selektif." },
  { "en": "Apa Itu Ruang Bersih (Cleanroom)?", "id": "Lingkungan Manufaktur Bebas Debu." },
  { "en": "Apa Itu Penyearah Silikon Terkendali (SCR)?", "id": "Silicon Controlled Rectifier, Jenis Thyristor." },
  { "en": "Aplikasi SCR?", "id": "Kontrol Daya AC, Dimmer Lampu." },
  { "en": "Apa Itu TRIAC?", "id": "TRIode for Alternating Current." },
  { "en": "Fungsi TRIAC?", "id": "Sebagai Saklar AC Dua Arah." },
  { "en": "Apa Itu DIAC?", "id": "DIode for Alternating Current." },
  { "en": "Fungsi DIAC?", "id": "Sebagai Pemicu Untuk TRIAC." },
  { "en": "Apa Itu Thyristor?", "id": "Keluarga Perangkat Semikonduktor Untuk Switching Daya." },
  { "en": "Apa Itu UJT?", "id": "Unijunction Transistor." },
  { "en": "Aplikasi UJT?", "id": "Rangkaian Pemicu (Trigger) Untuk SCR." },
  { "en": "Apa Itu Heat Sink?", "id": "Logam Pendingin Untuk Komponen Semikonduktor." },
  { "en": "Mengapa Heat Sink Diperlukan?", "id": "Untuk Mencegah Komponen Dari Panas Berlebih." },
  { "en": "Apa Itu Disipasi Daya?", "id": "Energi Listrik Yang Berubah Menjadi Panas." },
  { "en": "Apa Itu ESD (Electrostatic Discharge)?", "id": "Pelepasan Listrik Statis Yang Merusak Komponen." },
  { "en": "Bagaimana Mencegah ESD?", "id": "Menggunakan Gelang Antistatis, Alas Antistatis." },
  { "en": "Apa Itu BJT?", "id": "Bipolar Junction Transistor." },
  { "en": "Dua Tipe BJT?", "id": "NPN Dan PNP." },
  { "en": "Tiga Terminal BJT?", "id": "Basis (Base), Kolektor (Collector), Emitor (Emitter)." },
  { "en": "Bagaimana BJT Bekerja?", "id": "Arus Basis Kecil Mengontrol Arus Kolektor Besar." },
  { "en": "Apa Itu Penguatan Arus (Beta/hFE)?", "id": "Rasio Arus Kolektor Terhadap Arus Basis." },
  { "en": "Tiga Daerah Operasi BJT?", "id": "Cut-off, Aktif, Jenuh (Saturasi)." },
  { "en": "Daerah Cut-off?", "id": "Transistor Mati, Tidak Ada Arus Mengalir." },
  { "en": "Daerah Aktif?", "id": "Transistor Bekerja Sebagai Penguat." },
  { "en": "Daerah Jenuh (Saturasi)?", "id": "Transistor Sepenuhnya On, Bekerja Sebagai Saklar." },
  { "en": "Apa Itu Bias Transistor?", "id": "Memberi Tegangan DC Untuk Menentukan Titik Kerja." },
  { "en": "Apa Itu Titik Kerja (Q-point)?", "id": "Titik Operasi Transistor Di Daerah Aktif." },
  { "en": "Mengapa Bias Stabil Penting?", "id": "Agar Titik Kerja Tidak Bergeser." },
  { "en": "Rangkaian Bias Pembagi Tegangan?", "id": "Metode Pembiasan BJT Paling Umum." },
  { "en": "Apa Itu Garis Beban (Load Line)?", "id": "Grafik Hubungan Arus Dan Tegangan Kolektor." },
  { "en": "Apa Itu Penguat (Amplifier)?", "id": "Rangkaian Untuk Meningkatkan Kekuatan Sinyal." },
  { "en": "Apa Itu Penguatan Tegangan (Av)?", "id": "Rasio Tegangan Output Terhadap Input." },
  { "en": "Apa Itu Penguatan Arus (Ai)?", "id": "Rasio Arus Output Terhadap Input." },
  { "en": "Apa Itu Penguatan Daya (Ap)?", "id": "Rasio Daya Output Terhadap Input." },
  { "en": "Apa Itu Impedansi Input?", "id": "Hambatan Yang Dilihat Sumber Sinyal." },
  { "en": "Apa Itu Impedansi Output?", "id": "Hambatan Internal Dari Penguat." },
  { "en": "Tiga Konfigurasi Penguat BJT?", "id": "Common-Emitter, Common-Collector, Common-Base." },
  { "en": "Penguat Common-Emitter (CE)?", "id": "Penguatan Tegangan Dan Arus Tinggi." },
  { "en": "Pergeseran Fasa Penguat CE?", "id": "180 Derajat." },
  { "en": "Penguat Common-Collector (CC)?", "id": "Penguatan Tegangan Hampir Satu." },
  { "en": "Nama Lain Penguat CC?", "id": "Emitter Follower." },
  { "en": "Karakteristik Penguat CC?", "id": "Impedansi Input Tinggi, Output Rendah." },
  { "en": "Aplikasi Penguat CC?", "id": "Sebagai Buffer Atau Penyesuai Impedansi." },
  { "en": "Penguat Common-Base (CB)?", "id": "Penguatan Arus Hampir Satu." },
  { "en": "Karakteristik Penguat CB?", "id": "Impedansi Input Rendah, Output Tinggi." },
  { "en": "Aplikasi Penguat CB?", "id": "Penguat Frekuensi Tinggi." },
  { "en": "Apa Itu Model Sinyal Kecil?", "id": "Model Linear Transistor Untuk Analisis AC." },
  { "en": "Contoh Model Sinyal Kecil?", "id": "Model Hybrid-pi, Model r_e." },
  { "en": "Apa Itu FET?", "id": "Field-Effect Transistor." },
  { "en": "Apa Itu MOSFET?", "id": "Metal-Oxide-Semiconductor FET." },
  { "en": "Tiga Terminal MOSFET?", "id": "Gate, Drain, Source." },
  { "en": "Dua Tipe MOSFET?", "id": "Depletion Dan Enhancement." },
  { "en": "Apa Itu MOSFET Tipe Enhancement?", "id": "Normalnya Off, Membutuhkan Tegangan Untuk On." },
  { "en": "Apa Itu MOSFET Tipe Depletion?", "id": "Normalnya On, Membutuhkan Tegangan Untuk Off." },
  { "en": "MOSFET N-Channel vs P-Channel?", "id": "Berdasarkan Jenis Pembawa Muatan." },
  { "en": "Keuntungan MOSFET?", "id": "Impedansi Gate Sangat Tinggi, Disipasi Daya Rendah." },
  { "en": "Kelemahan MOSFET?", "id": "Rentan Terhadap Kerusakan Akibat Listrik Statis." },
  { "en": "Daerah Operasi MOSFET?", "id": "Cut-off, Triode (Ohmik), Saturasi." },
  { "en": "Daerah Triode MOSFET?", "id": "Bekerja Seperti Resistor Variabel." },
  { "en": "Daerah Saturasi MOSFET?", "id": "Bekerja Sebagai Penguat Atau Sumber Arus." },
  { "en": "Apa Itu Transkonduktansi (gm)?", "id": "Ukuran Penguatan Dari FET." },
  { "en": "Tiga Konfigurasi Penguat FET?", "id": "Common-Source, Common-Drain, Common-Gate." },
  { "en": "Penguat Common-Source (CS)?", "id": "Mirip Dengan Common-Emitter BJT." },
  { "en": "Penguat Common-Drain (CD)?", "id": "Mirip Dengan Common-Collector BJT." },
  { "en": "Nama Lain Penguat CD?", "id": "Source Follower." },
  { "en": "Penguat Common-Gate (CG)?", "id": "Mirip Dengan Common-Base BJT." },
  { "en": "Apa Itu JFET?", "id": "Junction Field-Effect Transistor." },
  { "en": "Apa Itu Tegangan Pinch-off?", "id": "Tegangan Yang Membuat Kanal JFET Tertutup." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Mengambil Sebagian Output Kembali Ke Input." },
  { "en": "Apa Itu Umpan Balik Negatif?", "id": "Mengurangi Gain Total." },
  { "en": "Keuntungan Umpan Balik Negatif?", "id": "Meningkatkan Stabilitas, Mengurangi Distorsi." },
  { "en": "Apa Itu Umpan Balik Positif?", "id": "Meningkatkan Gain Total." },
  { "en": "Aplikasi Umpan Balik Positif?", "id": "Osilator, Rangkaian Schmitt Trigger." },
  { "en": "Apa Itu Osilator?", "id": "Rangkaian Yang Menghasilkan Sinyal Periodik." },
  { "en": "Syarat Osilasi Barkhausen?", "id": "Gain Loop Satu, Pergeseran Fasa Nol." },
  { "en": "Jenis Osilator?", "id": "RC (Phase-Shift, Wien Bridge), LC (Colpitts, Hartley)." },
  { "en": "Apa Itu Osilator Kristal?", "id": "Osilator Dengan Stabilitas Frekuensi Sangat Tinggi." },
  { "en": "Apa Itu Penguat Diferensial?", "id": "Menguatkan Perbedaan Antara Dua Sinyal Input." },
  { "en": "Apa Itu Common-Mode Signal?", "id": "Sinyal Yang Sama Pada Kedua Input." },
  { "en": "Apa Itu Differential-Mode Signal?", "id": "Perbedaan Sinyal Antara Dua Input." },
  { "en": "Apa Itu CMRR?", "id": "Common-Mode Rejection Ratio." },
  { "en": "CMRR Yang Baik Bernilai?", "id": "Sangat Tinggi." },
  { "en": "Apa Itu Sumber Arus Konstan?", "id": "Rangkaian Yang Memberikan Arus Stabil." },
  { "en": "Apa Itu Cermin Arus (Current Mirror)?", "id": "Menyalin Arus Dari Satu Cabang Ke Cabang Lain." },
  { "en": "Apa Itu Beban Aktif?", "id": "Menggunakan Transistor Sebagai Beban Penguat." },
  { "en": "Keuntungan Beban Aktif?", "id": "Mendapatkan Penguatan Tegangan Yang Sangat Tinggi." },
  { "en": "Apa Itu Penguat Multitingkat (Multistage)?", "id": "Menggabungkan Beberapa Tingkat Penguat Secara Kaskade." },
  { "en": "Tujuan Penguat Multitingkat?", "id": "Untuk Mendapatkan Penguatan Total Yang Besar." },
  { "en": "Metode Kopling Antar Tingkat?", "id": "Kopling RC, Kopling Trafo, Kopling Langsung." },
  { "en": "Apa Itu Respons Frekuensi?", "id": "Bagaimana Gain Penguat Berubah Dengan Frekuensi." },
  { "en": "Apa Itu Frekuensi Cutoff Bawah?", "id": "Frekuensi Dimana Gain Turun 3 dB." },
  { "en": "Apa Itu Frekuensi Cutoff Atas?", "id": "Frekuensi Dimana Gain Juga Turun 3 dB." },
  { "en": "Apa Itu Bandwidth Penguat?", "id": "Rentang Frekuensi Antara Titik Cutoff." },
  { "en": "Penyebab Cutoff Bawah?", "id": "Kapasitor Kopling Dan Bypass." },
  { "en": "Penyebab Cutoff Atas?", "id": "Kapasitansi Internal Transistor." },
  { "en": "Apa Itu Efek Miller?", "id": "Meningkatkan Kapasitansi Input Akibat Penguatan." },
  { "en": "Apa Itu Penguat Daya?", "id": "Menguatkan Daya Sinyal Untuk Menggerakkan Beban." },
  { "en": "Contoh Beban Penguat Daya?", "id": "Loudspeaker, Antena." },
  { "en": "Apa Itu Penguat Kelas A?", "id": "Transistor Selalu Aktif." },
  { "en": "Karakteristik Kelas A?", "id": "Linearitas Baik, Efisiensi Sangat Rendah." },
  { "en": "Apa Itu Penguat Kelas B?", "id": "Setiap Transistor Aktif Selama Setengah Siklus." },
  { "en": "Karakteristik Kelas B?", "id": "Efisiensi Lebih Baik, Ada Crossover Distortion." },
  { "en": "Apa Itu Penguat Kelas AB?", "id": "Kompromi Antara Kelas A Dan Kelas B." },
  { "en": "Karakteristik Kelas AB?", "id": "Menghilangkan Crossover Distortion, Efisiensi Baik." },
  { "en": "Apa Itu Penguat Kelas C?", "id": "Transistor Aktif Kurang Dari Setengah Siklus." },
  { "en": "Aplikasi Kelas C?", "id": "Penguat Frekuensi Radio." },
  { "en": "Apa Itu Penguat Kelas D?", "id": "Penguat Switching." },
  { "en": "Karakteristik Kelas D?", "id": "Efisiensi Sangat Tinggi (Di Atas 90%)." },
  { "en": "Apa Itu Distorsi?", "id": "Perubahan Bentuk Sinyal Yang Tidak Diinginkan." },
  { "en": "Apa Itu Noise?", "id": "Sinyal Acak Tak Diinginkan Yang Mengganggu." },
  { "en": "Apa Itu Op-Amp?", "id": "Operational Amplifier." },
  { "en": "Apa Fungsi Op-Amp?", "id": "Penguat Diferensial Gain Sangat Tinggi." },
  { "en": "Simbol Op-Amp?", "id": "Segitiga Dengan Dua Input Dan Satu Output." },
  { "en": "Dua Input Op-Amp?", "id": "Inverting (-) Dan Non-inverting (+)." },
  { "en": "Apa Itu Op-Amp Ideal?", "id": "Model Sederhana Op-Amp Dengan Karakteristik Sempurna." },
  { "en": "Karakteristik Op-Amp Ideal?", "id": "Gain Tak Hingga, Impedansi Input Tak Hingga." },
  { "en": "Impedansi Output Op-Amp Ideal?", "id": "Nol." },
  { "en": "Bandwidth Op-Amp Ideal?", "id": "Tak Hingga." },
  { "en": "Apa Itu Golden Rules Op-Amp?", "id": "Dua Aturan Untuk Analisis Rangkaian." },
  { "en": "Golden Rule Pertama?", "id": "Tidak Ada Arus Mengalir Ke Terminal Input." },
  { "en": "Golden Rule Kedua?", "id": "Op-Amp Menjaga Tegangan Input Sama." },
  { "en": "Apa Itu Virtual Ground?", "id": "Titik Yang Tegangannya Nol Tapi Tidak Terhubung." },
  { "en": "Apa Itu Konfigurasi Loop Tertutup?", "id": "Menggunakan Umpan Balik Negatif." },
  { "en": "Mengapa Umpan Balik Negatif Penting?", "id": "Untuk Mengontrol Gain Dan Menstabilkan Op-Amp." },
  { "en": "Apa Itu Penguat Inverting?", "id": "Sinyal Masuk Ke Input Inverting." },
  { "en": "Gain Penguat Inverting?", "id": "Ditentukan Oleh Rasio Resistor Umpan Balik." },
  { "en": "Fasa Output Penguat Inverting?", "id": "Terbalik 180 Derajat Dari Input." },
  { "en": "Apa Itu Penguat Non-inverting?", "id": "Sinyal Masuk Ke Input Non-inverting." },
  { "en": "Gain Penguat Non-inverting?", "id": "Selalu Lebih Besar Atau Sama Dengan Satu." },
  { "en": "Fasa Output Penguat Non-inverting?", "id": "Sefasa Dengan Input." },
  { "en": "Apa Itu Voltage Follower?", "id": "Penguat Non-inverting Dengan Gain Satu." },
  { "en": "Aplikasi Voltage Follower?", "id": "Sebagai Buffer Atau Isolator." },
  { "en": "Apa Itu Summing Amplifier?", "id": "Menjumlahkan Beberapa Sinyal Tegangan." },
  { "en": "Apa Itu Difference Amplifier?", "id": "Menguatkan Perbedaan Antara Dua Sinyal." },
  { "en": "Apa Itu Integrator?", "id": "Outputnya Adalah Integral Dari Sinyal Input." },
  { "en": "Implementasi Integrator?", "id": "Menggunakan Kapasitor Di Jalur Umpan Balik." },
  { "en": "Apa Itu Differentiator?", "id": "Outputnya Adalah Turunan Dari Sinyal Input." },
  { "en": "Implementasi Differentiator?", "id": "Menggunakan Kapasitor Di Jalur Input." },
  { "en": "Masalah Differentiator Praktis?", "id": "Sangat Rentan Terhadap Noise." },
  { "en": "Apa Itu Komparator (Comparator)?", "id": "Membandingkan Dua Tegangan Input." },
  { "en": "Output Komparator?", "id": "Tegangan Positif Atau Negatif Maksimum." },
  { "en": "Apa Itu Schmitt Trigger?", "id": "Komparator Dengan Histeresis." },
  { "en": "Tujuan Histeresis?", "id": "Mencegah Osilasi Akibat Noise." },
  { "en": "Apa Itu Filter Aktif?", "id": "Filter Yang Menggunakan Op-Amp." },
  { "en": "Keuntungan Filter Aktif?", "id": "Dapat Memiliki Gain, Mudah Didesain." },
  { "en": "Filter Aktif Low-Pass Orde Pertama?", "id": "Menggunakan Rangkaian RC Dan Op-Amp." },
  { "en": "Filter Aktif High-Pass Orde Pertama?", "id": "Menggunakan Rangkaian CR Dan Op-Amp." },
  { "en": "Filter Sallen-Key?", "id": "Topologi Populer Untuk Filter Aktif Orde Dua." },
  { "en": "Apa Itu Osilator?", "id": "Menghasilkan Sinyal Periodik Tanpa Input." },
  { "en": "Apa Itu Osilator Jembatan Wien?", "id": "Osilator Op-Amp Yang Menghasilkan Gelombang Sinus." },
  { "en": "Apa Itu Astable Multivibrator?", "id": "Osilator Op-Amp Yang Menghasilkan Gelombang Kotak." },
  { "en": "Apa Itu Monostable Multivibrator?", "id": "Menghasilkan Satu Pulsa Saat Dipicu." },
  { "en": "Apa Itu Penyearah Presisi?", "id": "Menyearahkan Sinyal Kecil Menggunakan Op-Amp." },
  { "en": "Apa Itu Logarithmic Amplifier?", "id": "Outputnya Adalah Logaritma Dari Input." },
  { "en": "Apa Itu Anti-Logarithmic Amplifier?", "id": "Outputnya Adalah Eksponensial Dari Input." },
  { "en": "Apa Itu Konverter Arus-ke-Tegangan?", "id": "Mengubah Sinyal Arus Menjadi Sinyal Tegangan." },
  { "en": "Nama Lain Konverter Arus-ke-Tegangan?", "id": "Transimpedance Amplifier." },
  { "en": "Apa Itu Konverter Tegangan-ke-Arus?", "id": "Mengubah Sinyal Tegangan Menjadi Sinyal Arus." },
  { "en": "Nama Lain Konverter Tegangan-ke-Arus?", "id": "Transconductance Amplifier." },
  { "en": "Karakteristik Op-Amp Nyata (Non-Ideal)?", "id": "Gain Terhingga, Impedansi Terhingga, Dll." },
  { "en": "Apa Itu Open-Loop Gain?", "id": "Gain Op-Amp Tanpa Umpan Balik." },
  { "en": "Nilai Open-Loop Gain Nyata?", "id": "Sangat Besar, Tapi Terhingga." },
  { "en": "Apa Itu Gain-Bandwidth Product (GBW)?", "id": "Hasil Kali Gain Dan Bandwidth Adalah Konstan." },
  { "en": "Apa Itu Slew Rate?", "id": "Laju Perubahan Tegangan Output Maksimum." },
  { "en": "Efek Slew Rate Terbatas?", "id": "Dapat Mendistorsi Sinyal Frekuensi Tinggi." },
  { "en": "Apa Itu Tegangan Offset Input?", "id": "Tegangan Diferensial Agar Output Nol." },
  { "en": "Apa Itu Arus Bias Input?", "id": "Arus DC Kecil Yang Mengalir Ke Input." },
  { "en": "Apa Itu Arus Offset Input?", "id": "Perbedaan Antara Dua Arus Bias Input." },
  { "en": "Apa Itu CMRR?", "id": "Common-Mode Rejection Ratio." },
  { "en": "Fungsi CMRR?", "id": "Kemampuan Menolak Sinyal Yang Sama Di Kedua Input." },
  { "en": "Apa Itu PSRR?", "id": "Power Supply Rejection Ratio." },
  { "en": "Fungsi PSRR?", "id": "Kemampuan Menolak Noise Dari Catu Daya." },
  { "en": "Apa Itu Noise Op-Amp?", "id": "Op-Amp Menghasilkan Noise Internal." },
  { "en": "Apa Itu Catu Daya Ganda (Dual Supply)?", "id": "Op-Amp Membutuhkan Catu Daya Positif Negatif." },
  { "en": "Apa Itu Op-Amp Catu Daya Tunggal?", "id": "Dapat Beroperasi Dengan Satu Sumber Tegangan." },
  { "en": "Apa Itu Rail-to-Rail Op-Amp?", "id": "Outputnya Dapat Mencapai Tegangan Catu Daya." },
  { "en": "Apa Itu Instrumentasi Amplifier?", "id": "Penguat Diferensial Presisi Tinggi." },
  { "en": "Karakteristik Instrumentasi Amplifier?", "id": "CMRR Sangat Tinggi, Gain Mudah Diatur." },
  { "en": "Aplikasi Instrumentasi Amplifier?", "id": "Pengkondisian Sinyal Dari Sensor." },
  { "en": "Apa Itu Isolation Amplifier?", "id": "Memberikan Isolasi Listrik Antara Input Output." },
  { "en": "Kapan Isolation Amplifier Dibutuhkan?", "id": "Untuk Keamanan Dan Mengeliminasi Ground Loop." },
  { "en": "Apa Itu Sample and Hold Amplifier?", "id": "Mencuplik Sinyal Dan Menahannya Konstan." },
  { "en": "Aplikasi Sample and Hold?", "id": "Digunakan Dalam Konverter Analog-ke-Digital." },
  { "en": "Apa Itu Active Clamp Circuit?", "id": "Rangkaian Op-Amp Untuk Membatasi Tegangan." },
  { "en": "Apa Itu Window Comparator?", "id": "Mendeteksi Apakah Sinyal Berada Dalam Rentang." },
  { "en": "Apa Itu Peak Detector?", "id": "Menemukan Dan Menyimpan Nilai Puncak Sinyal." },
  { "en": "Apa Itu Zero Crossing Detector?", "id": "Mendeteksi Saat Sinyal Melewati Nol." },
  { "en": "Apa Itu Gyrator?", "id": "Rangkaian Op-Amp Yang Meniru Induktor." },
  { "en": "Apa Itu Negative Impedance Converter?", "id": "Rangkaian Yang Menghasilkan Resistansi Negatif." },
  { "en": "Apa Itu All-Pass Filter?", "id": "Menggeser Fasa Tanpa Mengubah Amplitudo." },
  { "en": "Apa Itu Respon Step Dari Integrator?", "id": "Menghasilkan Sinyal Ramp." },
  { "en": "Apa Itu Respon Step Dari Differentiator?", "id": "Menghasilkan Sinyal Impuls." },
  { "en": "Apa Itu Rangkaian Howland Current Source?", "id": "Sumber Arus Terkendali Tegangan." },
  { "en": "Apa Itu Kompensasi Frekuensi?", "id": "Menjamin Stabilitas Op-Amp Dalam Loop Tertutup." },
  { "en": "Apa Itu Phase Margin?", "id": "Ukuran Stabilitas Rangkaian Umpan Balik." },
  { "en": "Apa Itu Gain Margin?", "id": "Ukuran Stabilitas Lainnya." },
  { "en": "Apa Itu Osilasi?", "id": "Terjadi Jika Rangkaian Umpan Balik Tidak Stabil." },
  { "en": "Apa Itu Loop Gain?", "id": "Gain Total Di Sepanjang Loop Umpan Balik." },
  { "en": "Apa Itu Analisis Bode?", "id": "Digunakan Untuk Menganalisis Stabilitas Penguat." },
  { "en": "Apa Itu Op-Amp Daya?", "id": "Mampu Menangani Arus Output Yang Besar." },
  { "en": "Apa Itu Op-Amp Presisi?", "id": "Offset Dan Drift Sangat Rendah." },
  { "en": "Apa Itu Op-Amp Kecepatan Tinggi?", "id": "Memiliki GBW Dan Slew Rate Tinggi." },
  { "en": "Apa Itu Op-Amp Arus Umpan Balik?", "id": "Arsitektur Berbeda Untuk Performa Kecepatan Tinggi." },
  { "en": "Apa Itu Chopper-Stabilized Amplifier?", "id": "Teknik Untuk Mengurangi Offset Dan Drift." },
  { "en": "Apa Itu Auto-Zero Amplifier?", "id": "Teknik Lain Untuk Koreksi Offset." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Op-Amp Adalah Komponen Kunci Di Dalamnya." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Op-Amp Sering Digunakan Di Tahap Output." },
  { "en": "Apa Itu Current Feedback Amplifier?", "id": "Jenis Op-Amp Dengan Bandwidth Hampir Konstan." },
  { "en": "Apa Itu Elektronika Digital?", "id": "Elektronika Berbasis Sinyal Diskrit (0 Dan 1)." },
  { "en": "Apa Itu Sinyal Biner?", "id": "Sinyal Yang Hanya Memiliki Dua Level." },
  { "en": "Apa Representasi '1'?", "id": "Tegangan Tinggi (High)." },
  { "en": "Apa Representasi '0'?", "id": "Tegangan Rendah (Low)." },
  { "en": "Apa Itu Bit?", "id": "Binary Digit, Unit Informasi Dasar." },
  { "en": "Apa Itu Byte?", "id": "Sekelompok 8 Bit." },
  { "en": "Apa Itu Sistem Bilangan Biner?", "id": "Sistem Bilangan Berbasis Dua." },
  { "en": "Apa Itu Sistem Bilangan Heksadesimal?", "id": "Sistem Bilangan Berbasis Enam Belas." },
  { "en": "Apa Itu Aljabar Boolean?", "id": "Matematika Untuk Menganalisis Dan Menyederhanakan Logika." },
  { "en": "Variabel Dalam Aljabar Boolean?", "id": "Hanya Dapat Bernilai Benar (1) Atau Salah (0)." },
  { "en": "Tiga Operasi Dasar Boolean?", "id": "AND, OR, NOT." },
  { "en": "Operasi AND?", "id": "Output 1 Jika Semua Input 1." },
  { "en": "Operasi OR?", "id": "Output 1 Jika Salah Satu Input 1." },
  { "en": "Operasi NOT?", "id": "Membalik Nilai Input (Inverter)." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Implementasi Fisik Dari Operasi Boolean." },
  { "en": "Apa Itu Tabel Kebenaran?", "id": "Tabel Yang Menunjukkan Output Untuk Semua Input." },
  { "en": "Apa Itu Gerbang NAND?", "id": "Not-AND, Kebalikan Dari Gerbang AND." },
  { "en": "Apa Itu Gerbang NOR?", "id": "Not-OR, Kebalikan Dari Gerbang OR." },
  { "en": "Apa Itu Gerbang XOR?", "id": "Exclusive-OR." },
  { "en": "Output Gerbang XOR?", "id": "Output 1 Jika Input Ganjil." },
  { "en": "Apa Itu Gerbang XNOR?", "id": "Exclusive-NOR." },
  { "en": "Output Gerbang XNOR?", "id": "Output 1 Jika Inputnya Sama." },
  { "en": "Apa Itu Gerbang Universal?", "id": "Gerbang Yang Dapat Membentuk Semua Gerbang Lain." },
  { "en": "Gerbang Apa Saja Yang Universal?", "id": "NAND Dan NOR." },
  { "en": "Apa Itu Hukum De Morgan?", "id": "Aturan Untuk Mengubah Antara AND/OR Dan NAND/NOR." },
  { "en": "Apa Itu Sirkuit Kombinasional?", "id": "Outputnya Hanya Bergantung Pada Input Saat Ini." },
  { "en": "Karakteristik Sirkuit Kombinasional?", "id": "Tidak Memiliki Memori." },
  { "en": "Apa Itu Sirkuit Sekuensial?", "id": "Outputnya Bergantung Pada Input Dan Keadaan Sebelumnya." },
  { "en": "Apa Itu Half Adder?", "id": "Menjumlahkan Dua Bit Tunggal." },
  { "en": "Output Half Adder?", "id": "Sum (Jumlah) Dan Carry (Bawaan)." },
  { "en": "Apa Itu Full Adder?", "id": "Menjumlahkan Tiga Bit (Dua Bit Carry-in)." },
  { "en": "Apa Itu Ripple-Carry Adder?", "id": "Menjumlahkan Bilangan Biner Dengan Rangkaian Full Adder." },
  { "en": "Apa Itu Half Subtractor?", "id": "Mengurangkan Dua Bit Tunggal." },
  { "en": "Output Half Subtractor?", "id": "Difference (Selisih) Dan Borrow (Pinjaman)." },
  { "en": "Apa Itu Full Subtractor?", "id": "Mengurangkan Tiga Bit." },
  { "en": "Apa Itu Komparator?", "id": "Membandingkan Dua Bilangan Biner." },
  { "en": "Output Komparator?", "id": "A > B, A = B, A < B." },
  { "en": "Apa Itu Encoder?", "id": "Mengubah Input Aktif Menjadi Kode Biner." },
  { "en": "Apa Itu Decoder?", "id": "Mengubah Kode Biner Menjadi Output Aktif." },
  { "en": "Aplikasi Decoder?", "id": "Pemilihan Memori, Anoda 7-Segmen." },
  { "en": "Apa Itu Decoder BCD ke 7-Segmen?", "id": "Mengubah Kode Biner Desimal Ke Tampilan." },
  { "en": "Apa Itu Multiplexer (MUX)?", "id": "Memilih Satu Dari Banyak Input." },
  { "en": "Nama Lain MUX?", "id": "Data Selector." },
  { "en": "Apa Itu Demultiplexer (DEMUX)?", "id": "Mengirim Satu Input Ke Salah Satu Output." },
  { "en": "Nama Lain DEMUX?", "id": "Data Distributor." },
  { "en": "Apa Itu Parity Generator/Checker?", "id": "Digunakan Untuk Deteksi Kesalahan Sederhana." },
  { "en": "Apa Itu Paritas Ganjil?", "id": "Total Bit 1 Adalah Ganjil." },
  { "en": "Apa Itu Paritas Genap?", "id": "Total Bit 1 Adalah Genap." },
  { "en": "Apa Itu Karnaugh Map (K-Map)?", "id": "Metode Grafis Untuk Menyederhanakan Ekspresi Logika." },
  { "en": "Apa Itu Metode Quine-McCluskey?", "id": "Metode Tabular Untuk Penyederhanaan Logika." },
  { "en": "Apa Itu Sum of Products (SOP)?", "id": "Bentuk Ekspresi Logika (Penjumlahan Dari Perkalian)." },
  { "en": "Apa Itu Product of Sums (POS)?", "id": "Bentuk Ekspresi Logika (Perkalian Dari Penjumlahan)." },
  { "en": "Apa Itu Minterm?", "id": "Suku Perkalian Dalam Bentuk SOP Kanonik." },
  { "en": "Apa Itu Maxterm?", "id": "Suku Penjumlahan Dalam Bentuk POS Kanonik." },
  { "en": "Apa Itu Hazard?", "id": "Output Palsu Sesaat Akibat Tundaan Gerbang." },
  { "en": "Apa Itu Static Hazard?", "id": "Output Berubah Sesaat Saat Seharusnya Tetap." },
  { "en": "Apa Itu Dynamic Hazard?", "id": "Output Berubah Beberapa Kali Saat Seharusnya Sekali." },
  { "en": "Apa Itu Race Condition?", "id": "Kondisi Dimana Urutan Sinyal Mempengaruhi Output." },
  { "en": "Apa Itu Keluarga Logika?", "id": "Klasifikasi Sirkuit Digital Berdasarkan Teknologinya." },
  { "en": "Apa Itu TTL (Transistor-Transistor Logic)?", "id": "Keluarga Logika Berbasis BJT." },
  { "en": "Apa Itu CMOS (Complementary MOS)?", "id": "Keluarga Logika Berbasis MOSFET." },
  { "en": "Keuntungan CMOS?", "id": "Konsumsi Daya Sangat Rendah." },
  { "en": "Apa Itu ECL (Emitter-Coupled Logic)?", "id": "Keluarga Logika Paling Cepat." },
  { "en": "Apa Itu Level Tegangan Logika?", "id": "Rentang Tegangan Untuk High Dan Low." },
  { "en": "Apa Itu Noise Margin?", "id": "Toleransi Terhadap Gangguan Noise." },
  { "en": "Apa Itu Propagation Delay?", "id": "Waktu Tunda Sinyal Melewati Gerbang." },
  { "en": "Apa Itu Fan-Out?", "id": "Jumlah Input Gerbang Yang Bisa Digerakkan." },
  { "en": "Apa Itu Open-Collector Output?", "id": "Output TTL Yang Membutuhkan Resistor Pull-up." },
  { "en": "Apa Itu Tri-State Logic?", "id": "Output Dengan Tiga Keadaan: High, Low, Hi-Z." },
  { "en": "Apa Itu Keadaan Hi-Z (Impedansi Tinggi)?", "id": "Output Terputus Secara Elektrik." },
  { "en": "Tujuan Tri-State Logic?", "id": "Untuk Berbagi Bus Data." },
  { "en": "Apa Itu Arithmetic Logic Unit (ALU)?", "id": "Bagian CPU Yang Melakukan Operasi Aritmatika Logika." },
  { "en": "Apa Itu Look-Ahead Carry Adder?", "id": "Penjumlah Biner Berkecepatan Tinggi." },
  { "en": "Apa Itu Bus?", "id": "Sekumpulan Kabel Paralel Untuk Mentransfer Data." },
  { "en": "Jenis Bus?", "id": "Bus Alamat, Bus Data, Bus Kontrol." },
  { "en": "Apa Itu Programmable Logic Device (PLD)?", "id": "IC Yang Logikanya Dapat Diprogram." },
  { "en": "Apa Itu PAL (Programmable Array Logic)?", "id": "Jenis PLD Sederhana." },
  { "en": "Apa Itu GAL (Generic Array Logic)?", "id": "Versi PAL Yang Dapat Diprogram Ulang." },
  { "en": "Apa Itu CPLD (Complex PLD)?", "id": "Berisi Beberapa Blok Logika Seperti PAL." },
  { "en": "Apa Itu FPGA (Field-Programmable Gate Array)?", "id": "PLD Paling Fleksibel Dan Kompleks." },
  { "en": "Apa Itu Look-Up Table (LUT)?", "id": "Blok Logika Dasar Dalam FPGA." },
  { "en": "Apa Itu Hardware Description Language (HDL)?", "id": "Bahasa Untuk Mendeskripsikan Sirkuit Digital." },
  { "en": "Contoh HDL?", "id": "VHDL Dan Verilog." },
  { "en": "Apa Itu Sintesis?", "id": "Proses Menerjemahkan Kode HDL Menjadi Gerbang." },
  { "en": "Apa Itu Simulasi?", "id": "Menguji Fungsionalitas Desain Di Komputer." },
  { "en": "Apa Itu Gray Code?", "id": "Kode Dimana Hanya Satu Bit Berubah." },
  { "en": "Aplikasi Gray Code?", "id": "Encoder Posisi Untuk Menghindari Error." },
  { "en": "Apa Itu Kode BCD?", "id": "Binary-Coded Decimal." },
  { "en": "Apa Itu Kode Excess-3?", "id": "Kode BCD Ditambah Tiga." },
  { "en": "Apa Itu Kode ASCII?", "id": "Kode Standar Untuk Karakter Teks." },
  { "en": "Apa Itu Two's Complement?", "id": "Metode Representasi Bilangan Biner Bertanda." },
  { "en": "Apa Itu Sirkuit Sekuensial?", "id": "Sirkuit Dengan Memori." },
  { "en": "Output Sirkuit Sekuensial Bergantung Pada Apa?", "id": "Input Saat Ini Dan Keadaan Sebelumnya." },
  { "en": "Apa Elemen Dasar Sirkuit Sekuensial?", "id": "Flip-Flop Atau Latch." },
  { "en": "Sirkuit Sekuensial vs Kombinasional?", "id": "Sekuensial Memiliki Memori, Kombinasional Tidak." },
  { "en": "Dua Tipe Sirkuit Sekuensial?", "id": "Sinkron Dan Asinkron." },
  { "en": "Apa Itu Sirkuit Sinkron?", "id": "Perubahan Keadaan Dikontrol Oleh Sinyal Clock." },
  { "en": "Apa Itu Sirkuit Asinkron?", "id": "Perubahan Keadaan Terjadi Langsung Akibat Perubahan Input." },
  { "en": "Apa Itu Sinyal Clock?", "id": "Sinyal Periodik Untuk Sinkronisasi." },
  { "en": "Apa Itu Latch?", "id": "Elemen Memori Biner Yang Transparan." },
  { "en": "Apa Itu Latch SR?", "id": "Terdiri Dari Dua Gerbang NAND Atau NOR." },
  { "en": "Input Latch SR?", "id": "Set (S) Dan Reset (R)." },
  { "en": "Keadaan Terlarang Latch SR?", "id": "Saat S Dan R Aktif Bersamaan." },
  { "en": "Apa Itu Latch D (Data)?", "id": "Menyimpan Nilai Input D." },
  { "en": "Apa Itu Flip-Flop?", "id": "Elemen Memori Biner Yang Dipicu Tepi (Edge-Triggered)." },
  { "en": "Perbedaan Latch Dan Flip-Flop?", "id": "Latch Level-Triggered, Flip-Flop Edge-Triggered." },
  { "en": "Apa Itu Edge-Triggered?", "id": "Perubahan Terjadi Pada Tepi Naik Atau Turun Clock." },
  { "en": "Apa Itu Flip-Flop SR?", "id": "Versi Sinkron Dari Latch SR." },
  { "en": "Apa Itu Flip-Flop JK?", "id": "Versi Perbaikan Dari Flip-Flop SR." },
  { "en": "Keuntungan Flip-Flop JK?", "id": "Tidak Ada Keadaan Terlarang." },
  { "en": "Mode Toggle Flip-Flop JK?", "id": "Saat J Dan K Sama-sama High." },
  { "en": "Apa Itu Flip-Flop D (Data)?", "id": "Menyimpan Nilai D Saat Clock Aktif." },
  { "en": "Aplikasi Flip-Flop D?", "id": "Register, Memori." },
  { "en": "Apa Itu Flip-Flop T (Toggle)?", "id": "Membalik (Toggle) Keadaan Outputnya." },
  { "en": "Input Asinkron Flip-Flop?", "id": "Preset Dan Clear." },
  { "en": "Fungsi Preset?", "id": "Memaksa Output Ke 1." },
  { "en": "Fungsi Clear?", "id": "Memaksa Output Ke 0." },
  { "en": "Apa Itu Register?", "id": "Sekelompok Flip-Flop Untuk Menyimpan Data." },
  { "en": "Apa Itu Register Geser (Shift Register)?", "id": "Menggeser Data Satu Posisi Per Clock." },
  { "en": "Aplikasi Register Geser?", "id": "Konversi Serial-Paralel, Pembangkit Urutan." },
  { "en": "Apa Itu Serial-In, Serial-Out (SISO)?", "id": "Data Masuk Dan Keluar Secara Serial." },
  { "en": "Apa Itu Serial-In, Parallel-Out (SIPO)?", "id": "Data Masuk Serial, Keluar Paralel." },
  { "en": "Apa Itu Parallel-In, Serial-Out (PISO)?", "id": "Data Masuk Paralel, Keluar Serial." },
  { "en": "Apa Itu Parallel-In, Parallel-Out (PIPO)?", "id": "Data Masuk Dan Keluar Paralel." },
  { "en": "Apa Itu Universal Shift Register?", "id": "Dapat Melakukan Semua Mode Operasi." },
  { "en": "Apa Itu Ring Counter?", "id": "Register Geser Dengan Output Terhubung Ke Input." },
  { "en": "Apa Itu Johnson Counter?", "id": "Modifikasi Dari Ring Counter." },
  { "en": "Apa Itu Counter (Pencacah)?", "id": "Sirkuit Sekuensial Untuk Menghitung Pulsa." },
  { "en": "Apa Itu Counter Asinkron?", "id": "Flip-Flop Tidak Dipicu Clock Yang Sama." },
  { "en": "Nama Lain Counter Asinkron?", "id": "Ripple Counter." },
  { "en": "Kekurangan Ripple Counter?", "id": "Ada Tundaan Propagasi." },
  { "en": "Apa Itu Counter Sinkron?", "id": "Semua Flip-Flop Dipicu Clock Yang Sama." },
  { "en": "Keuntungan Counter Sinkron?", "id": "Lebih Cepat Dan Tanpa Masalah Tundaan." },
  { "en": "Apa Itu Up Counter?", "id": "Menghitung Naik." },
  { "en": "Apa Itu Down Counter?", "id": "Menghitung Turun." },
  { "en": "Apa Itu Up/Down Counter?", "id": "Dapat Menghitung Naik Atau Turun." },
  { "en": "Apa Itu Decade Counter?", "id": "Menghitung Dari 0 Hingga 9." },
  { "en": "Apa Itu Modulus Counter?", "id": "Jumlah Keadaan Yang Dimiliki Counter." },
  { "en": "Bagaimana Mendesain Counter Sinkron?", "id": "Menggunakan Tabel Eksitasi Flip-Flop." },
  { "en": "Apa Itu Tabel Eksitasi?", "id": "Menunjukkan Input Yang Dibutuhkan Untuk Transisi." },
  { "en": "Apa Itu Mesin Keadaan (State Machine)?", "id": "Model Matematis Untuk Sirkuit Sekuensial." },
  { "en": "Apa Itu Finite State Machine (FSM)?", "id": "Mesin Keadaan Dengan Jumlah Keadaan Terbatas." },
  { "en": "Apa Itu State (Keadaan)?", "id": "Informasi Yang Tersimpan Dalam Memori." },
  { "en": "Apa Itu Diagram Keadaan (State Diagram)?", "id": "Representasi Grafis Dari FSM." },
  { "en": "Apa Itu Tabel Keadaan (State Table)?", "id": "Representasi Tabular Dari FSM." },
  { "en": "Apa Itu Mesin Moore?", "id": "Output Hanya Bergantung Pada Keadaan Saat Ini." },
  { "en": "Apa Itu Mesin Mealy?", "id": "Output Bergantung Pada Keadaan Dan Input." },
  { "en": "Langkah Desain Sirkuit Sekuensial?", "id": "Diagram, Tabel, Penentuan Flip-Flop, Persamaan." },
  { "en": "Apa Itu Reduksi Keadaan?", "id": "Menyederhanakan FSM Dengan Menghapus Keadaan Redundan." },
  { "en": "Apa Itu Penugasan Keadaan (State Assignment)?", "id": "Memberi Kode Biner Untuk Setiap Keadaan." },
  { "en": "Apa Itu Setup Time?", "id": "Waktu Data Harus Stabil Sebelum Clock." },
  { "en": "Apa Itu Hold Time?", "id": "Waktu Data Harus Stabil Setelah Clock." },
  { "en": "Pelanggaran Setup/Hold Time Menyebabkan?", "id": "Metastabilitas." },
  { "en": "Apa Itu Metastabilitas?", "id": "Keadaan Tak Tentu Antara 0 Dan 1." },
  { "en": "Bagaimana Mengatasi Metastabilitas?", "id": "Menggunakan Sinkronizer." },
  { "en": "Apa Itu Sinkronizer?", "id": "Sirkuit Untuk Menyelaraskan Sinyal Asinkron." },
  { "en": "Apa Itu Clock Skew?", "id": "Perbedaan Waktu Tiba Sinyal Clock." },
  { "en": "Apa Itu Clock Jitter?", "id": "Variasi Periodik Dari Sinyal Clock." },
  { "en": "Apa Itu Memory?", "id": "Perangkat Untuk Menyimpan Informasi Digital." },
  { "en": "Apa Itu RAM (Random Access Memory)?", "id": "Memori Yang Dapat Dibaca Dan Ditulis." },
  { "en": "Apa Itu ROM (Read-Only Memory)?", "id": "Memori Yang Hanya Dapat Dibaca." },
  { "en": "Apa Itu SRAM (Static RAM)?", "id": "Menggunakan Latch Sebagai Sel Memori." },
  { "en": "Karakteristik SRAM?", "id": "Cepat, Mahal, Volatile." },
  { "en": "Apa Itu DRAM (Dynamic RAM)?", "id": "Menggunakan Kapasitor Sebagai Sel Memori." },
  { "en": "Karakteristik DRAM?", "id": "Padat, Murah, Perlu Refresh." },
  { "en": "Apa Itu Volatile Memory?", "id": "Data Hilang Saat Daya Dimatikan." },
  { "en": "Apa Itu Non-Volatile Memory?", "id": "Data Tetap Ada Tanpa Daya." },
  { "en": "Contoh Non-Volatile Memory?", "id": "ROM, Flash Memory." },
  { "en": "Apa Itu PROM (Programmable ROM)?", "id": "Dapat Diprogram Satu Kali." },
  { "en": "Apa Itu EPROM (Erasable PROM)?", "id": "Dapat Dihapus Dengan Sinar UV." },
  { "en": "Apa Itu EEPROM (Electrically Erasable PROM)?", "id": "Dapat Dihapus Secara Elektrik." },
  { "en": "Apa Itu Flash Memory?", "id": "Jenis EEPROM Yang Cepat Dan Populer." },
  { "en": "Apa Itu Dekoder Alamat?", "id": "Memilih Lokasi Memori Yang Akan Diakses." },
  { "en": "Apa Itu Hirarki Memori?", "id": "Pengorganisasian Memori Berdasarkan Kecepatan Dan Ukuran." },
  { "en": "Urutan Hirarki Memori?", "id": "Register, Cache, RAM, Penyimpanan Sekunder." },
  { "en": "Apa Itu Analisis Timing?", "id": "Menganalisis Waktu Tunda Dalam Sirkuit Digital." },
  { "en": "Apa Itu Critical Path?", "id": "Jalur Dengan Tundaan Terpanjang Dalam Sirkuit." },
  { "en": "Apa Itu Pipelining?", "id": "Teknik Untuk Meningkatkan Throughput Sirkuit." },
  { "en": "Apa Itu Sequence Detector?", "id": "FSM Yang Mengenali Urutan Bit Tertentu." },
  { "en": "Apa Itu One-Hot Encoding?", "id": "Metode Penugasan Keadaan." },
  { "en": "Keuntungan One-Hot Encoding?", "id": "Logika Dekoder Yang Sederhana." },
  { "en": "Apa Itu Timing Diagram?", "id": "Grafik Yang Menunjukkan Hubungan Waktu Sinyal." },
  { "en": "Apa Itu Glitch?", "id": "Pulsa Palsu Sesaat Pada Output." },
  { "en": "Penyebab Glitch?", "id": "Hazard Dalam Sirkuit Kombinasional." },
  { "en": "Apa Itu Mikroprosesor?", "id": "Unit Pemroses Pusat (CPU) Dalam Satu Chip." },
  { "en": "Apa Itu Mikrokontroler?", "id": "Komputer Kecil Dalam Satu Chip." },
  { "en": "Perbedaan Utama Mikroprosesor Dan Mikrokontroler?", "id": "Mikrokontroler Memiliki RAM, ROM, I/O." },
  { "en": "Komponen Utama Mikrokontroler?", "id": "CPU, Memori, Timer, I/O Port." },
  { "en": "Apa Itu CPU (Central Processing Unit)?", "id": "Otak Yang Menjalankan Instruksi." },
  { "en": "Apa Itu ALU (Arithmetic Logic Unit)?", "id": "Bagian CPU Untuk Operasi Aritmatika Logika." },
  { "en": "Apa Itu Register?", "id": "Lokasi Penyimpanan Sangat Cepat Di Dalam CPU." },
  { "en": "Apa Itu Program Counter (PC)?", "id": "Menyimpan Alamat Instruksi Berikutnya." },
  { "en": "Apa Itu Instruction Register (IR)?", "id": "Menyimpan Instruksi Yang Sedang Dijalankan." },
  { "en": "Apa Itu Accumulator?", "id": "Register Untuk Menyimpan Hasil Operasi ALU." },
  { "en": "Apa Itu Memori?", "id": "Tempat Menyimpan Program Dan Data." },
  { "en": "Apa Itu RAM (Random Access Memory)?", "id": "Memori Volatile Untuk Data Sementara." },
  { "en": "Apa Itu ROM (Read-Only Memory)?", "id": "Memori Non-Volatile Untuk Menyimpan Program." },
  { "en": "Apa Itu Flash Memory?", "id": "Jenis ROM Yang Dapat Diprogram Ulang." },
  { "en": "Apa Itu I/O Port?", "id": "Input/Output Port." },
  { "en": "Fungsi I/O Port?", "id": "Menghubungkan Mikrokontroler Dengan Dunia Luar." },
  { "en": "Apa Itu GPIO?", "id": "General Purpose Input/Output." },
  { "en": "Apa Itu Arsitektur Komputer?", "id": "Desain Konseptual Dan Struktur Operasional." },
  { "en": "Apa Itu Arsitektur Von Neumann?", "id": "Menggunakan Memori Yang Sama Untuk Program Data." },
  { "en": "Apa Itu Arsitektur Harvard?", "id": "Memisahkan Memori Program Dan Data." },
  { "en": "Keuntungan Arsitektur Harvard?", "id": "Akses Program Dan Data Bisa Bersamaan." },
  { "en": "Apa Itu Bus?", "id": "Jalur Komunikasi Internal." },
  { "en": "Apa Itu Bus Alamat?", "id": "Membawa Informasi Lokasi Memori." },
  { "en": "Apa Itu Bus Data?", "id": "Membawa Data Antara CPU Dan Memori." },
  { "en": "Apa Itu Bus Kontrol?", "id": "Membawa Sinyal-Sinyal Pengaturan Waktu." },
  { "en": "Apa Itu Siklus Instruksi?", "id": "Fetch, Decode, Execute." },
  { "en": "Tahap Fetch?", "id": "Mengambil Instruksi Dari Memori." },
  { "en": "Tahap Decode?", "id": "Menerjemahkan Instruksi." },
  { "en": "Tahap Execute?", "id": "Menjalankan Instruksi." },
  { "en": "Apa Itu Set Instruksi?", "id": "Kumpulan Semua Instruksi Yang Dimengerti CPU." },
  { "en": "Apa Itu CISC (Complex Instruction Set Computer)?", "id": "Memiliki Set Instruksi Yang Kompleks." },
  { "en": "Apa Itu RISC (Reduced Instruction Set Computer)?", "id": "Memiliki Set Instruksi Yang Sederhana." },
  { "en": "Apa Itu Bahasa Assembly?", "id": "Bahasa Pemrograman Tingkat Rendah." },
  { "en": "Apa Itu Bahasa Mesin?", "id": "Bahasa Biner Yang Langsung Dimengerti CPU." },
  { "en": "Apa Itu Compiler?", "id": "Menerjemahkan Bahasa Tingkat Tinggi Ke Mesin." },
  { "en": "Apa Itu Assembler?", "id": "Menerjemahkan Bahasa Assembly Ke Mesin." },
  { "en": "Apa Itu Osilator?", "id": "Menyediakan Sinyal Clock Untuk Mikrokontroler." },
  { "en": "Apa Itu Kristal Kuarsa?", "id": "Komponen Untuk Membuat Osilator Stabil." },
  { "en": "Apa Itu Timer/Counter?", "id": "Periferal Untuk Mengukur Waktu Menghitung Event." },
  { "en": "Apa Itu Watchdog Timer?", "id": "Mereset Sistem Jika Program Macet." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Mengubah Sinyal Analog Menjadi Digital." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Mengubah Sinyal Digital Menjadi Analog." },
  { "en": "Apa Itu PWM (Pulse Width Modulation)?", "id": "Teknik Menghasilkan Sinyal Analog Semu." },
  { "en": "Apa Itu Komunikasi Serial?", "id": "Mengirim Data Satu Bit Sekaligus." },
  { "en": "Apa Itu UART?", "id": "Universal Asynchronous Receiver/Transmitter." },
  { "en": "Apa Itu SPI (Serial Peripheral Interface)?", "id": "Protokol Komunikasi Serial Sinkron." },
  { "en": "Apa Itu I2C (Inter-Integrated Circuit)?", "id": "Protokol Komunikasi Serial Dua Kabel." },
  { "en": "Apa Itu Interrupt?", "id": "Sinyal Yang Menginterupsi Jalannya Program." },
  { "en": "Tujuan Interrupt?", "id": "Untuk Merespon Event Penting Secara Cepat." },
  { "en": "Apa Itu Interrupt Service Routine (ISR)?", "id": "Kode Yang Dijalankan Saat Terjadi Interrupt." },
  { "en": "Apa Itu Power-On Reset?", "id": "Sirkuit Yang Mereset Mikrokontroler Saat Dinyalakan." },
  { "en": "Apa Itu Brown-Out Detector?", "id": "Mereset Jika Tegangan Catu Daya Turun." },
  { "en": "Apa Itu Sistem Tertanam (Embedded System)?", "id": "Sistem Komputer Untuk Tugas Spesifik." },
  { "en": "Contoh Sistem Tertanam?", "id": "Microwave, Mesin Cuci, Router." },
  { "en": "Apa Itu Real-Time Operating System (RTOS)?", "id": "Sistem Operasi Untuk Aplikasi Real-Time." },
  { "en": "Karakteristik RTOS?", "id": "Deterministik Dan Respons Cepat." },
  { "en": "Apa Itu Cross-Compiler?", "id": "Compiler Yang Berjalan Di Satu Komputer." },
  { "en": "Apa Itu Debugger?", "id": "Alat Untuk Mencari Kesalahan Dalam Program." },
  { "en": "Apa Itu In-Circuit Debugger (ICD)?", "id": "Debugger Yang Terhubung Langsung Ke Mikrokontroler." },
  { "en": "Apa Itu Simulator?", "id": "Menjalankan Dan Menguji Kode Tanpa Perangkat Keras." },
  { "en": "Apa Itu Emulator?", "id": "Meniru Perilaku Perangkat Keras Di Perangkat Lunak." },
  { "en": "Apa Itu Programmer (Device Programmer)?", "id": "Alat Untuk Memasukkan Program Ke Mikrokontroler." },
  { "en": "Apa Itu ICSP (In-Circuit Serial Programming)?", "id": "Memprogram Chip Tanpa Melepasnya Dari Rangkaian." },
  { "en": "Apa Itu Bootloader?", "id": "Program Kecil Untuk Memuat Program Utama." },
  { "en": "Contoh Mikrokontroler Populer?", "id": "Arduino (AVR), PIC, STM32 (ARM)." },
  { "en": "Apa Itu Arduino?", "id": "Platform Prototyping Berbasis Mikrokontroler." },
  { "en": "Apa Itu Raspberry Pi?", "id": "Komputer Papan Tunggal, Bukan Mikrokontroler." },
  { "en": "Apa Itu DMA (Direct Memory Access)?", "id": "Transfer Data Langsung Tanpa Melibatkan CPU." },
  { "en": "Apa Itu EEPROM (Electrically Erasable PROM)?", "id": "Memori Non-Volatile Untuk Menyimpan Pengaturan." },
  { "en": "Apa Itu Komparator Analog?", "id": "Periferal Untuk Membandingkan Dua Tegangan Analog." },
  { "en": "Apa Itu Sleep Mode?", "id": "Mode Konsumsi Daya Rendah." },
  { "en": "Apa Itu Pin Multiplexing?", "id": "Satu Pin Fisik Memiliki Beberapa Fungsi." },
  { "en": "Apa Itu Pull-up Resistor?", "id": "Memberi Nilai High Default Pada Input." },
  { "en": "Apa Itu Pull-down Resistor?", "id": "Memberi Nilai Low Default Pada Input." },
  { "en": "Apa Itu Open-Drain Output?", "id": "Output Yang Hanya Bisa Menarik Ke Low." },
  { "en": "Apa Itu Firmware?", "id": "Perangkat Lunak Yang Tertanam Dalam Perangkat Keras." },
  { "en": "Apa Itu Bit Banging?", "id": "Implementasi Protokol Serial Dengan Perangkat Lunak." },
  { "en": "Apa Itu Data Sheet?", "id": "Dokumen Teknis Detail Tentang Sebuah Chip." },
  { "en": "Apa Itu Errata?", "id": "Dokumen Koreksi Atau Masalah Yang Ditemukan." },
  { "en": "Apa Itu Application Note?", "id": "Dokumen Yang Menjelaskan Aplikasi Tertentu." },
  { "en": "Apa Itu Development Board?", "id": "Papan Sirkuit Untuk Memulai Pengembangan." },
  { "en": "Apa Itu IDE (Integrated Development Environment)?", "id": "Perangkat Lunak Untuk Menulis Dan Menguji Kode." },
  { "en": "Apa Itu Library?", "id": "Kumpulan Kode Yang Telah Ditulis Sebelumnya." },
  { "en": "Apa Itu API (Application Programming Interface)?", "id": "Antarmuka Untuk Berinteraksi Dengan Perangkat Lunak." },
  { "en": "Apa Itu Stack?", "id": "Area Memori Untuk Variabel Lokal Panggilan Fungsi." },
  { "en": "Apa Itu Heap?", "id": "Area Memori Untuk Alokasi Dinamis." },
  { "en": "Apa Itu Pointer?", "id": "Variabel Yang Menyimpan Alamat Memori." },
  { "en": "Apa Itu Volatile Keyword?", "id": "Memberitahu Compiler Bahwa Variabel Bisa Berubah." },
  { "en": "Apa Itu Sinyal?", "id": "Representasi Informasi Dari Suatu Fenomena." },
  { "en": "Apa Itu Sistem?", "id": "Proses Yang Memodifikasi Sinyal." },
  { "en": "Sinyal Waktu Kontinu?", "id": "Sinyal Yang Didefinisikan Di Setiap Waktu." },
  { "en": "Sinyal Waktu Diskrit?", "id": "Sinyal Yang Didefinisikan Di Waktu Tertentu." },
  { "en": "Sinyal Analog?", "id": "Sinyal Dengan Amplitudo Kontinu." },
  { "en": "Sinyal Digital?", "id": "Sinyal Dengan Amplitudo Diskrit." },
  { "en": "Apa Itu Sampling?", "id": "Proses Mengubah Sinyal Waktu Kontinu Ke Diskrit." },
  { "en": "Apa Itu Kuantisasi?", "id": "Proses Mengubah Amplitudo Kontinu Ke Diskrit." },
  { "en": "Apa Itu Teorema Sampling Nyquist?", "id": "Frekuensi Sampling Harus Dua Kali Frekuensi Sinyal." },
  { "en": "Apa Itu Aliasing?", "id": "Distorsi Akibat Laju Sampling Terlalu Rendah." },
  { "en": "Sinyal Periodik?", "id": "Sinyal Yang Berulang Dalam Interval Waktu." },
  { "en": "Sinyal Aperiodik?", "id": "Sinyal Yang Tidak Berulang." },
  { "en": "Sinyal Genap (Even)?", "id": "Simetris Terhadap Sumbu Vertikal." },
  { "en": "Sinyal Ganjil (Odd)?", "id": "Simetris Terhadap Titik Asal." },
  { "en": "Sinyal Deterministik?", "id": "Dapat Dideskripsikan Secara Matematis." },
  { "en": "Sinyal Acak (Random)?", "id": "Tidak Dapat Diprediksi Secara Tepat." },
  { "en": "Apa Itu Noise?", "id": "Sinyal Acak Yang Tidak Diinginkan." },
  { "en": "Apa Itu Sinyal Impuls Satuan?", "id": "Sinyal Sangat Singkat Dengan Luas Satu." },
  { "en": "Apa Itu Sinyal Step Satuan?", "id": "Sinyal Yang Bernilai Satu Setelah t=0." },
  { "en": "Apa Itu Sistem LTI?", "id": "Linear Time-Invariant." },
  { "en": "Apa Itu Linearitas?", "id": "Memenuhi Prinsip Superposisi." },
  { "en": "Apa Itu Time-Invariance?", "id": "Respon Tidak Berubah Terhadap Pergeseran Waktu." },
  { "en": "Apa Itu Respons Impuls?", "id": "Output Sistem LTI Saat Diberi Input Impuls." },
  { "en": "Apa Itu Konvolusi?", "id": "Operasi Matematis Untuk Mencari Output Sistem LTI." },
  { "en": "Apa Itu Sistem Kausal?", "id": "Output Tidak Bergantung Pada Input Masa Depan." },
  { "en": "Apa Itu Sistem Stabil?", "id": "Input Terbatas Menghasilkan Output Terbatas (BIBO)." },
  { "en": "Apa Itu Domain Waktu?", "id": "Representasi Sinyal Sebagai Fungsi Waktu." },
  { "en": "Apa Itu Domain Frekuensi?", "id": "Representasi Sinyal Sebagai Fungsi Frekuensi." },
  { "en": "Apa Itu Transformasi Fourier?", "id": "Mengubah Sinyal Antara Domain Waktu Frekuensi." },
  { "en": "Apa Itu Spektrum Frekuensi?", "id": "Plot Amplitudo Dan Fasa Terhadap Frekuensi." },
  { "en": "Apa Itu Deret Fourier?", "id": "Untuk Merepresentasikan Sinyal Periodik." },
  { "en": "Apa Itu Transformasi Fourier Diskrit (DFT)?", "id": "Transformasi Fourier Untuk Sinyal Diskrit." },
  { "en": "Apa Itu Fast Fourier Transform (FFT)?", "id": "Algoritma Cepat Untuk Menghitung DFT." },
  { "en": "Apa Itu Transformasi Laplace?", "id": "Generalisasi Transformasi Fourier." },
  { "en": "Apa Itu Bidang-s?", "id": "Domain Frekuensi Kompleks." },
  { "en": "Apa Itu Transformasi-Z?", "id": "Analogi Laplace Untuk Sinyal Waktu Diskrit." },
  { "en": "Apa Itu Bidang-z?", "id": "Domain Frekuensi Kompleks Untuk Sinyal Diskrit." },
  { "en": "Apa Itu Fungsi Transfer?", "id": "Rasio Output Terhadap Input Di Domain Frekuensi." },
  { "en": "Apa Itu Pole?", "id": "Akar Penyebut Fungsi Transfer." },
  { "en": "Apa Itu Zero?", "id": "Akar Pembilang Fungsi Transfer." },
  { "en": "Hubungan Pole Dengan Stabilitas?", "id": "Pole Menentukan Stabilitas Sistem." },
  { "en": "Apa Itu Respons Frekuensi?", "id": "Bagaimana Sistem Merespon Terhadap Input Sinusoidal." },
  { "en": "Apa Itu Plot Bode?", "id": "Grafik Respons Frekuensi (Magnitudo Dan Fasa)." },
  { "en": "Apa Itu Filter?", "id": "Sistem Yang Memodifikasi Spektrum Frekuensi Sinyal." },
  { "en": "Apa Itu Filter Low-Pass?", "id": "Melewatkan Frekuensi Rendah." },
  { "en": "Apa Itu Filter High-Pass?", "id": "Melewatkan Frekuensi Tinggi." },
  { "en": "Apa Itu Filter Band-Pass?", "id": "Melewatkan Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Frekuensi Cutoff?", "id": "Frekuensi Batas Operasi Filter." },
  { "en": "Apa Itu Filter Ideal?", "id": "Memiliki Transisi Yang Sangat Tajam." },
  { "en": "Apakah Filter Ideal Dapat Dibuat?", "id": "Tidak, Secara Praktis Tidak Mungkin." },
  { "en": "Apa Itu Filter Butterworth?", "id": "Memiliki Respons Yang Sangat Datar." },
  { "en": "Apa Itu Filter Chebyshev?", "id": "Memiliki Transisi Tajam Dengan Ripple." },
  { "en": "Apa Itu Filter Bessel?", "id": "Memiliki Respon Fasa Yang Linear." },
  { "en": "Apa Itu Filter Digital?", "id": "Filter Yang Diimplementasikan Secara Digital." },
  { "en": "Apa Itu Filter FIR?", "id": "Finite Impulse Response." },
  { "en": "Keuntungan Filter FIR?", "id": "Selalu Stabil, Fasa Linear Mudah Dicapai." },
  { "en": "Apa Itu Filter IIR?", "id": "Infinite Impulse Response." },
  { "en": "Keuntungan Filter IIR?", "id": "Lebih Efisien Secara Komputasi." },
  { "en": "Apa Itu Windowing?", "id": "Teknik Yang Digunakan Dalam Desain Filter FIR." },
  { "en": "Apa Itu Modulasi?", "id": "Menumpangkan Sinyal Informasi Ke Sinyal Pembawa." },
  { "en": "Apa Itu Sinyal Pembawa (Carrier)?", "id": "Sinyal Frekuensi Tinggi." },
  { "en": "Apa Itu Modulasi Amplitudo (AM)?", "id": "Amplitudo Pembawa Diubah." },
  { "en": "Apa Itu Modulasi Frekuensi (FM)?", "id": "Frekuensi Pembawa Diubah." },
  { "en": "Apa Itu Modulasi Fasa (PM)?", "id": "Fasa Pembawa Diubah." },
  { "en": "Apa Itu Demodulasi?", "id": "Proses Mendapatkan Kembali Sinyal Informasi." },
  { "en": "Apa Itu Bandwidth?", "id": "Rentang Frekuensi Yang Ditempati Sinyal." },
  { "en": "Apa Itu Sistem Komunikasi?", "id": "Pemancar, Kanal, Penerima." },
  { "en": "Apa Itu Kanal?", "id": "Medium Transmisi Sinyal." },
  { "en": "Apa Itu SNR?", "id": "Signal-to-Noise Ratio." },
  { "en": "Apa Itu Korelasi?", "id": "Ukuran Kemiripan Antara Dua Sinyal." },
  { "en": "Apa Itu Autokorelasi?", "id": "Korelasi Sinyal Dengan Dirinya Sendiri." },
  { "en": "Apa Itu Cross-Korelasi?", "id": "Korelasi Antara Dua Sinyal Berbeda." },
  { "en": "Apa Itu Sistem Invertible?", "id": "Sistem Yang Inputnya Dapat Dipulihkan." },
  { "en": "Apa Itu Dekonvolusi?", "id": "Proses Membalik Efek Konvolusi." },
  { "en": "Apa Itu Hilbert Transform?", "id": "Menggeser Fasa Semua Komponen Sinyal." },
  { "en": "Apa Itu Sinyal Analitik?", "id": "Sinyal Bernilai Kompleks." },
  { "en": "Apa Itu Digital Signal Processor (DSP)?", "id": "Mikroprosesor Khusus Untuk Pemrosesan Sinyal." },
  { "en": "Aplikasi DSP?", "id": "Audio, Video, Telekomunikasi." },
  { "en": "Apa Itu Downsampling?", "id": "Mengurangi Laju Sampling Sinyal." },
  { "en": "Apa Itu Upsampling?", "id": "Meningkatkan Laju Sampling Sinyal." },
  { "en": "Apa Itu Interpolasi?", "id": "Membuat Sampel Baru Di Antara Sampel Yang Ada." },
  { "en": "Apa Itu Desimasi?", "id": "Proses Downsampling Setelah Filtering." },
  { "en": "Apa Itu Sistem Multirate?", "id": "Sistem Yang Menggunakan Beberapa Laju Sampling." },
  { "en": "Apa Itu Wavelet Transform?", "id": "Analisis Sinyal Dengan Resolusi Waktu-Frekuensi." },
  { "en": "Apa Itu Sistem Diskrit?", "id": "Sistem Yang Memproses Sinyal Diskrit." },
  { "en": "Apa Itu Diagram Blok?", "id": "Representasi Grafis Dari Sistem." },
  { "en": "Apa Itu Signal Flow Graph?", "id": "Representasi Grafis Lainnya Untuk Sistem." },
  { "en": "Apa Itu Energi Sinyal?", "id": "Ukuran Kekuatan Sinyal." },
  { "en": "Apa Itu Daya Sinyal?", "id": "Energi Rata-rata Per Satuan Waktu." },
  { "en": "Apa Itu White Noise?", "id": "Noise Dengan Spektrum Daya Yang Datar." },
  { "en": "Apa Itu Pink Noise?", "id": "Spektrum Daya Berkurang Dengan Frekuensi." },
  { "en": "Apa Itu Power Spectral Density (PSD)?", "id": "Distribusi Daya Sinyal Terhadap Frekuensi." },
  { "en": "Apa Itu Efek Gibbs?", "id": "Osilasi Dekat Diskontinuitas Dalam Deret Fourier." },
  { "en": "Apa Itu Sistem Komunikasi?", "id": "Mengirim Informasi Dari Sumber Ke Tujuan." },
  { "en": "Elemen Dasar Sistem Komunikasi?", "id": "Pemancar, Kanal, Penerima." },
  { "en": "Apa Itu Sinyal Informasi?", "id": "Sinyal Yang Mengandung Pesan (Suara, Gambar)." },
  { "en": "Nama Lain Sinyal Informasi?", "id": "Sinyal Baseband Atau Sinyal Modulasi." },
  { "en": "Apa Itu Modulasi?", "id": "Menumpangkan Sinyal Informasi Ke Sinyal Pembawa." },
  { "en": "Mengapa Modulasi Diperlukan?", "id": "Untuk Transmisi Efisien Dan Multiplexing." },
  { "en": "Apa Itu Sinyal Pembawa (Carrier)?", "id": "Gelombang Frekuensi Tinggi." },
  { "en": "Apa Itu Modulasi Analog?", "id": "Parameter Pembawa Diubah Secara Kontinu." },
  { "en": "Jenis Modulasi Analog?", "id": "AM, FM, PM." },
  { "en": "Apa Itu Modulasi Amplitudo (AM)?", "id": "Amplitudo Pembawa Bervariasi Sesuai Sinyal Informasi." },
  { "en": "Apa Itu Envelope (Amplop)?", "id": "Bentuk Sinyal Termodulasi AM." },
  { "en": "Apa Itu Indeks Modulasi AM?", "id": "Ukuran Seberapa Dalam Amplitudo Dimodulasi." },
  { "en": "Apa Itu Overmodulation?", "id": "Indeks Modulasi Lebih Dari 100%." },
  { "en": "Apa Akibat Overmodulation?", "id": "Menyebabkan Distorsi Pada Sinyal." },
  { "en": "Spektrum Sinyal AM?", "id": "Pembawa, Upper Sideband, Lower Sideband." },
  { "en": "Apa Itu Sideband?", "id": "Pita Frekuensi Yang Mengandung Informasi." },
  { "en": "Bandwidth Sinyal AM?", "id": "Dua Kali Frekuensi Sinyal Informasi." },
  { "en": "Daya Sinyal AM?", "id": "Terdiri Dari Daya Pembawa Dan Sideband." },
  { "en": "Efisiensi Modulasi AM?", "id": "Persentase Daya Di Sideband." },
  { "en": "Apa Itu DSB-SC?", "id": "Double Sideband Suppressed Carrier." },
  { "en": "Keuntungan DSB-SC?", "id": "Lebih Efisien Daya Dibandingkan AM." },
  { "en": "Apa Itu SSB?", "id": "Single Sideband." },
  { "en": "Keuntungan SSB?", "id": "Lebih Efisien Bandwidth Dan Daya." },
  { "en": "Apa Itu VSB?", "id": "Vestigial Sideband." },
  { "en": "Aplikasi VSB?", "id": "Transmisi Sinyal Video Televisi Analog." },
  { "en": "Apa Itu Demodulasi AM?", "id": "Proses Mendeteksi Sinyal Informasi Kembali." },
  { "en": "Apa Itu Detektor Amplop?", "id": "Sirkuit Sederhana Untuk Demodulasi AM." },
  { "en": "Apa Itu Modulasi Frekuensi (FM)?", "id": "Frekuensi Pembawa Bervariasi Sesuai Sinyal Informasi." },
  { "en": "Amplitudo Sinyal FM?", "id": "Selalu Konstan." },
  { "en": "Keuntungan FM Dibanding AM?", "id": "Lebih Tahan Terhadap Noise." },
  { "en": "Apa Itu Deviasi Frekuensi?", "id": "Pergeseran Frekuensi Maksimum Dari Pembawa." },
  { "en": "Apa Itu Indeks Modulasi FM?", "id": "Rasio Deviasi Frekuensi Terhadap Frekuensi Informasi." },
  { "en": "Apa Itu Narrowband FM (NBFM)?", "id": "FM Dengan Indeks Modulasi Kecil." },
  { "en": "Apa Itu Wideband FM (WBFM)?", "id": "FM Dengan Indeks Modulasi Besar." },
  { "en": "Bandwidth Sinyal FM?", "id": "Diperkirakan Dengan Aturan Carson." },
  { "en": "Apa Itu Modulasi Fasa (PM)?", "id": "Fasa Pembawa Bervariasi Sesuai Sinyal Informasi." },
  { "en": "Hubungan FM Dan PM?", "id": "Saling Berkaitan Erat." },
  { "en": "Apa Itu Pemancar (Transmitter)?", "id": "Menghasilkan Dan Memancarkan Sinyal Radio." },
  { "en": "Blok Diagram Pemancar AM?", "id": "Osilator, Modulator, Penguat Daya." },
  { "en": "Apa Itu Penerima (Receiver)?", "id": "Menangkap Dan Memproses Sinyal Radio." },
  { "en": "Tiga Karakteristik Penerima?", "id": "Sensitivitas, Selektivitas, Fidelitas." },
  { "en": "Apa Itu Sensitivitas?", "id": "Kemampuan Menerima Sinyal Yang Lemah." },
  { "en": "Apa Itu Selektivitas?", "id": "Kemampuan Memilih Sinyal Yang Diinginkan." },
  { "en": "Apa Itu Fidelitas?", "id": "Kemampuan Mereproduksi Sinyal Informasi Asli." },
  { "en": "Apa Itu Penerima Superheterodyne?", "id": "Arsitektur Penerima Radio Paling Umum." },
  { "en": "Prinsip Superheterodyne?", "id": "Mengubah Frekuensi Radio Menjadi Frekuensi Antara." },
  { "en": "Apa Itu Frekuensi Antara (IF)?", "id": "Intermediate Frequency." },
  { "en": "Keuntungan Superheterodyne?", "id": "Selektivitas Dan Penguatan Yang Baik." },
  { "en": "Apa Itu Mixer?", "id": "Sirkuit Untuk Mencampur Dua Frekuensi." },
  { "en": "Apa Itu Osilator Lokal?", "id": "Menghasilkan Sinyal Untuk Proses Pencampuran." },
  { "en": "Apa Itu Frekuensi Bayangan (Image Frequency)?", "id": "Frekuensi Tak Diinginkan Yang Dapat Diterima." },
  { "en": "Apa Itu Automatic Gain Control (AGC)?", "id": "Menjaga Level Output Tetap Konstan." },
  { "en": "Apa Itu Demodulator FM?", "id": "Sirkuit Untuk Mendeteksi Sinyal FM." },
  { "en": "Contoh Demodulator FM?", "id": "Slope Detector, Discriminator, Phase-Locked Loop (PLL)." },
  { "en": "Apa Itu Phase-Locked Loop (PLL)?", "id": "Sirkuit Umpan Balik Untuk Sinkronisasi Fasa." },
  { "en": "Apa Itu Pre-emphasis?", "id": "Meningkatkan Amplitudo Frekuensi Tinggi Sebelum Transmisi." },
  { "en": "Apa Itu De-emphasis?", "id": "Melemahkan Amplitudo Frekuensi Tinggi Setelah Penerimaan." },
  { "en": "Tujuan Pre-emphasis Dan De-emphasis?", "id": "Meningkatkan Kualitas Sinyal (SNR) Di FM." },
  { "en": "Apa Itu Siaran Stereo FM?", "id": "Mengirim Sinyal Audio Kiri Dan Kanan." },
  { "en": "Bagaimana Stereo FM Bekerja?", "id": "Menggunakan Sinyal Multipleks." },
  { "en": "Apa Itu Sinyal Televisi Analog?", "id": "Mengandung Informasi Gambar (Video) Dan Suara (Audio)." },
  { "en": "Modulasi Sinyal Video?", "id": "Menggunakan Modulasi Amplitudo (AM-VSB)." },
  { "en": "Modulasi Sinyal Audio?", "id": "Menggunakan Modulasi Frekuensi (FM)." },
  { "en": "Apa Itu Multiplexing?", "id": "Mengirim Beberapa Sinyal Melalui Satu Kanal." },
  { "en": "Apa Itu FDM?", "id": "Frequency Division Multiplexing." },
  { "en": "Prinsip FDM?", "id": "Setiap Sinyal Mendapat Pita Frekuensi Berbeda." },
  { "en": "Apa Itu TDM?", "id": "Time Division Multiplexing." },
  { "en": "Prinsip TDM?", "id": "Setiap Sinyal Mendapat Slot Waktu Berbeda." },
  { "en": "Apa Itu Kanal Komunikasi?", "id": "Medium Fisik Untuk Transmisi Sinyal." },
  { "en": "Contoh Kanal?", "id": "Kabel, Udara Bebas, Serat Optik." },
  { "en": "Apa Itu Noise?", "id": "Sinyal Acak Yang Mengganggu Komunikasi." },
  { "en": "Jenis Noise?", "id": "Noise Termal, Shot Noise, Interferensi." },
  { "en": "Apa Itu Noise Termal (Johnson Noise)?", "id": "Dihasilkan Oleh Agitasi Termal Elektron." },
  { "en": "Apa Itu SNR (Signal-to-Noise Ratio)?", "id": "Rasio Daya Sinyal Terhadap Daya Noise." },
  { "en": "SNR Tinggi Berarti?", "id": "Kualitas Sinyal Baik." },
  { "en": "Apa Itu Noise Figure?", "id": "Ukuran Seberapa Banyak Noise Ditambahkan Perangkat." },
  { "en": "Apa Itu Antena?", "id": "Mengubah Sinyal Listrik Menjadi Gelombang Elektromagnetik." },
  { "en": "Apa Itu Propagasi Gelombang Radio?", "id": "Cara Gelombang Radio Merambat." },
  { "en": "Apa Itu Ground Wave?", "id": "Gelombang Yang Mengikuti Kelengkungan Bumi." },
  { "en": "Apa Itu Sky Wave?", "id": "Gelombang Yang Dipantulkan Oleh Ionosfer." },
  { "en": "Apa Itu Line-of-Sight (LOS) Propagation?", "id": "Perambatan Garis Lurus." },
  { "en": "Apa Itu Fading?", "id": "Fluktuasi Kekuatan Sinyal Yang Diterima." },
  { "en": "Apa Itu Multipath Fading?", "id": "Akibat Interferensi Sinyal Dari Banyak Lintasan." },
  { "en": "Apa Itu Sinyal Baseband?", "id": "Sinyal Informasi Dalam Rentang Frekuensi Aslinya." },
  { "en": "Apa Itu Sinyal Passband?", "id": "Sinyal Baseband Yang Telah Dimodulasi." },
  { "en": "Apa Itu Komunikasi Telepon?", "id": "Contoh Sistem Komunikasi Analog." },
  { "en": "Apa Itu Local Loop?", "id": "Kabel Dari Sentral Telepon Ke Pelanggan." },
  { "en": "Apa Itu Switching?", "id": "Menghubungkan Penelepon Dengan Pihak Yang Dituju." },
  { "en": "Apa Itu Bandwidth Kanal Suara?", "id": "Sekitar 4 kHz." },
  { "en": "Apa Itu Penguat (Amplifier)?", "id": "Meningkatkan Kekuatan Sinyal." },
  { "en": "Apa Itu Attenuasi?", "id": "Pelemahan Sinyal Seiring Jarak." },
  { "en": "Apa Itu Komunikasi Digital?", "id": "Transmisi Informasi Dalam Bentuk Biner." },
  { "en": "Keuntungan Komunikasi Digital?", "id": "Tahan Noise, Mudah Diproses, Keamanan Baik." },
  { "en": "Tiga Langkah Dasar Komunikasi Digital?", "id": "Sampling, Kuantisasi, Pengkodean." },
  { "en": "Apa Itu Pulse Code Modulation (PCM)?", "id": "Metode Standar Mengubah Analog Ke Digital." },
  { "en": "Apa Itu Sampling?", "id": "Mengambil Sampel Sinyal Analog Secara Periodik." },
  { "en": "Apa Itu Teorema Sampling?", "id": "Laju Sampling Harus > 2 Kali Frekuensi Maksimum." },
  { "en": "Apa Itu Kuantisasi?", "id": "Membulatkan Nilai Sampel Ke Level Terdekat." },
  { "en": "Apa Itu Level Kuantisasi?", "id": "Jumlah Level Diskrit Yang Digunakan." },
  { "en": "Apa Itu Error Kuantisasi?", "id": "Kesalahan Akibat Pembulatan." },
  { "en": "Nama Lain Error Kuantisasi?", "id": "Quantization Noise." },
  { "en": "Apa Itu Pengkodean (Encoding)?", "id": "Memberi Kode Biner Untuk Setiap Level Kuantisasi." },
  { "en": "Apa Itu Bit Rate?", "id": "Jumlah Bit Yang Dikirim Per Detik." },
  { "en": "Apa Itu Line Coding?", "id": "Representasi Data Biner Sebagai Sinyal Baseband." },
  { "en": "Contoh Line Coding?", "id": "NRZ, RZ, Manchester." },
  { "en": "Apa Itu NRZ (Non-Return-to-Zero)?", "id": "Level Konstan Untuk Setiap Bit." },
  { "en": "Apa Itu Kode Manchester?", "id": "Setiap Bit Memiliki Transisi Di Tengah." },
  { "en": "Apa Itu Modulasi Digital?", "id": "Menumpangkan Sinyal Digital Ke Sinyal Pembawa." },
  { "en": "Apa Itu Amplitude Shift Keying (ASK)?", "id": "Data Direpresentasikan Oleh Amplitudo Berbeda." },
  { "en": "Apa Itu Frequency Shift Keying (FSK)?", "id": "Data Direpresentasikan Oleh Frekuensi Berbeda." },
  { "en": "Apa Itu Phase Shift Keying (PSK)?", "id": "Data Direpresentasikan Oleh Fasa Berbeda." },
  { "en": "Apa Itu BPSK?", "id": "Binary PSK, Menggunakan Dua Fasa." },
  { "en": "Apa Itu QPSK?", "id": "Quadrature PSK, Menggunakan Empat Fasa." },
  { "en": "Apa Itu Simbol?", "id": "Satu Bentuk Gelombang Yang Merepresentasikan Bit." },
  { "en": "Berapa Bit Per Simbol QPSK?", "id": "Dua Bit Per Simbol." },
  { "en": "Apa Itu Diagram Konstelasi?", "id": "Visualisasi Titik Simbol Modulasi Digital." },
  { "en": "Apa Itu QAM?", "id": "Quadrature Amplitude Modulation." },
  { "en": "Bagaimana QAM Bekerja?", "id": "Menggabungkan Variasi Amplitudo Dan Fasa." },
  { "en": "Apa Itu Bandwidth Efisiensi?", "id": "Rasio Bit Rate Terhadap Bandwidth." },
  { "en": "Apa Itu Probabilitas Error Bit (BER)?", "id": "Bit Error Rate." },
  { "en": "Apa Itu Eb/N0?", "id": "Rasio Energi Per Bit Terhadap Kerapatan Noise." },
  { "en": "Apa Itu Matched Filter?", "id": "Filter Optimal Untuk Mendeteksi Sinyal." },
  { "en": "Apa Itu Inter-Symbol Interference (ISI)?", "id": "Gangguan Antar Simbol Akibat Distorsi Kanal." },
  { "en": "Apa Itu Kriteria Nyquist Untuk Zero ISI?", "id": "Syarat Untuk Menghilangkan ISI." },
  { "en": "Apa Itu Eye Diagram (Diagram Mata)?", "id": "Alat Visual Untuk Menganalisis Kualitas Sinyal." },
  { "en": "Eye Opening Lebar Berarti?", "id": "Kualitas Sinyal Baik, Noise Rendah." },
  { "en": "Apa Itu Sinkronisasi?", "id": "Penting Dalam Komunikasi Digital." },
  { "en": "Jenis Sinkronisasi?", "id": "Sinkronisasi Carrier, Simbol, Frame." },
  { "en": "Apa Itu Delta Modulation (DM)?", "id": "Metode Sederhana Mengubah Sinyal Analog Digital." },
  { "en": "Apa Itu Differential PCM (DPCM)?", "id": "Mengkodekan Selisih Antar Sampel." },
  { "en": "Apa Itu Adaptive DPCM?", "id": "DPCM Dengan Ukuran Langkah Yang Beradaptasi." },
  { "en": "Apa Itu Time Division Multiplexing (TDM)?", "id": "Membagi Kanal Berdasarkan Waktu." },
  { "en": "Contoh TDM?", "id": "Sistem Telepon Digital (T1/E1)." },
  { "en": "Apa Itu Framing?", "id": "Menambahkan Bit Untuk Identifikasi Frame Data." },
  { "en": "Apa Itu Teori Informasi?", "id": "Studi Matematis Tentang Komunikasi." },
  { "en": "Siapa Bapak Teori Informasi?", "id": "Claude Shannon." },
  { "en": "Apa Itu Entropi?", "id": "Ukuran Rata-rata Informasi Atau Ketidakpastian." },
  { "en": "Apa Itu Source Coding?", "id": "Kompresi Data." },
  { "en": "Apa Itu Teorema Source Coding?", "id": "Batas Kompresi Adalah Entropi." },
  { "en": "Apa Itu Kode Huffman?", "id": "Algoritma Kompresi Data Lossless." },
  { "en": "Apa Itu Channel Coding?", "id": "Menambahkan Redundansi Untuk Melawan Error." },
  { "en": "Apa Itu Teorema Channel Coding?", "id": "Transmisi Bebas Error Dimungkinkan Di Bawah Kapasitas." },
  { "en": "Apa Itu Kapasitas Kanal?", "id": "Laju Data Maksimum Yang Andal." },
  { "en": "Apa Itu Hukum Shannon-Hartley?", "id": "Kapasitas Kanal Dengan Noise Gaussian." },
  { "en": "Apa Itu Error Detection?", "id": "Mendeteksi Adanya Kesalahan Transmisi." },
  { "en": "Apa Itu Error Correction?", "id": "Mendeteksi Dan Memperbaiki Kesalahan." },
  { "en": "Apa Itu Parity Check?", "id": "Metode Deteksi Error Paling Sederhana." },
  { "en": "Apa Itu Checksum?", "id": "Metode Deteksi Error Lainnya." },
  { "en": "Apa Itu CRC (Cyclic Redundancy Check)?", "id": "Metode Deteksi Error Yang Kuat." },
  { "en": "Apa Itu Forward Error Correction (FEC)?", "id": "Penerima Memperbaiki Error Tanpa Transmisi Ulang." },
  { "en": "Apa Itu Automatic Repeat Request (ARQ)?", "id": "Meminta Transmisi Ulang Jika Ada Error." },
  { "en": "Apa Itu Block Code?", "id": "Jenis Kode Koreksi Error." },
  { "en": "Apa Itu Kode Hamming?", "id": "Contoh Block Code Sederhana." },
  { "en": "Apa Itu Jarak Hamming?", "id": "Jumlah Posisi Bit Yang Berbeda." },
  { "en": "Apa Itu Convolutional Code?", "id": "Jenis Kode FEC Lainnya." },
  { "en": "Apa Itu Algoritma Viterbi?", "id": "Algoritma Umum Untuk Dekode Kode Konvolusional." },
  { "en": "Apa Itu Spread Spectrum?", "id": "Teknik Menyebarkan Sinyal Ke Bandwidth Lebar." },
  { "en": "Tujuan Spread Spectrum?", "id": "Tahan Terhadap Interferensi Dan Penyadapan." },
  { "en": "Apa Itu DSSS (Direct Sequence Spread Spectrum)?", "id": "Mengalikan Sinyal Dengan Kode Penyebar." },
  { "en": "Apa Itu FHSS (Frequency Hopping Spread Spectrum)?", "id": "Mengubah Frekuensi Transmisi Secara Cepat." },
  { "en": "Apa Itu CDMA (Code Division Multiple Access)?", "id": "Banyak Pengguna Berbagi Frekuensi Menggunakan Kode." },
  { "en": "Apa Itu OFDM (Orthogonal Frequency Division Multiplexing)?", "id": "Membagi Data Ke Banyak Sub-carrier." },
  { "en": "Keuntungan OFDM?", "id": "Sangat Tahan Terhadap Multipath Fading." },
  { "en": "Aplikasi OFDM?", "id": "Wi-Fi, 4G/5G, Siaran TV Digital." },
  { "en": "Apa Itu Equalization?", "id": "Proses Mengkompensasi Distorsi Kanal." },
  { "en": "Apa Itu Modem?", "id": "Modulator-Demodulator." },
  { "en": "Apa Itu Jaringan Komputer?", "id": "Kumpulan Komputer Yang Saling Terhubung." },
  { "en": "Apa Itu Protokol?", "id": "Aturan Yang Mengatur Komunikasi." },
  { "en": "Apa Itu Model OSI?", "id": "Model Referensi 7 Lapis." },
  { "en": "Apa Itu TCP/IP?", "id": "Suite Protokol Utama Internet." },
  { "en": "Apa Itu Ethernet?", "id": "Teknologi Jaringan Area Lokal (LAN) Dominan." },
  { "en": "Apa Itu Router?", "id": "Menghubungkan Jaringan Dan Meneruskan Paket." },
  { "en": "Apa Itu Alamat IP?", "id": "Alamat Unik Perangkat Di Jaringan." },
  { "en": "Apa Itu Paket?", "id": "Unit Data Dalam Komunikasi Jaringan." },
  { "en": "Apa Itu Komunikasi Serat Optik?", "id": "Menggunakan Pulsa Cahaya Untuk Mengirim Data." },
  { "en": "Keuntungan Serat Optik?", "id": "Bandwidth Sangat Besar, Tahan Interferensi." },
  { "en": "Apa Itu Komunikasi Nirkabel?", "id": "Komunikasi Tanpa Menggunakan Kabel." },
  { "en": "Contoh Komunikasi Nirkabel?", "id": "Wi-Fi, Bluetooth, Seluler." },
  { "en": "Apa Itu Komunikasi Seluler?", "id": "Jaringan Radio Terdistribusi (Sel)." },
  { "en": "Apa Itu Handoff?", "id": "Peralihan Koneksi Antara Sel." },
  { "en": "Apa Itu Komunikasi Satelit?", "id": "Menggunakan Satelit Sebagai Repeater." },
  { "en": "Apa Itu Latensi?", "id": "Waktu Tunda Dalam Komunikasi." },
  { "en": "Apa Itu Arsitektur Komputer?", "id": "Desain Tingkat Tinggi Dari Sistem Komputer." },
  { "en": "Apa Itu Organisasi Komputer?", "id": "Implementasi Dari Arsitektur." },
  { "en": "Apa Itu Pipelining?", "id": "Teknik Mengeksekusi Beberapa Instruksi Secara Tumpang Tindih." },
  { "en": "Tujuan Pipelining?", "id": "Meningkatkan Throughput Atau Kinerja CPU." },
  { "en": "Apa Itu Pipeline Hazard?", "id": "Masalah Yang Mengganggu Aliran Pipeline." },
  { "en": "Jenis Hazard?", "id": "Struktural, Data, Kontrol." },
  { "en": "Apa Itu Data Hazard?", "id": "Instruksi Bergantung Pada Hasil Instruksi Sebelumnya." },
  { "en": "Apa Itu Control Hazard?", "id": "Masalah Akibat Instruksi Percabangan (Branch)." },
  { "en": "Apa Itu Branch Prediction?", "id": "Menebak Arah Percabangan Untuk Menjaga Pipeline." },
  { "en": "Apa Itu Arsitektur Superscalar?", "id": "CPU Dengan Beberapa Pipeline Eksekusi." },
  { "en": "Apa Itu Out-of-Order Execution?", "id": "Mengeksekusi Instruksi Tidak Sesuai Urutan Program." },
  { "en": "Apa Itu Hirarki Memori?", "id": "Pengorganisasian Memori Bertingkat." },
  { "en": "Tujuan Hirarki Memori?", "id": "Menyeimbangkan Kecepatan, Ukuran, Dan Biaya." },
  { "en": "Level Hirarki Memori?", "id": "Register, Cache, Memori Utama, Penyimpanan Sekunder." },
  { "en": "Apa Itu Cache Memory?", "id": "Memori Kecil Dan Cepat." },
  { "en": "Fungsi Cache?", "id": "Menyimpan Data Yang Sering Diakses." },
  { "en": "Apa Itu Prinsip Lokalitas?", "id": "Program Cenderung Mengakses Data Di Dekatnya." },
  { "en": "Apa Itu Cache Hit?", "id": "Data Yang Dicari Ditemukan Di Cache." },
  { "en": "Apa Itu Cache Miss?", "id": "Data Yang Dicari Tidak Ditemukan Di Cache." },
  { "en": "Apa Itu Memori Virtual?", "id": "Teknik Menggunakan Disk Sebagai Perpanjangan RAM." },
  { "en": "Apa Itu Paging?", "id": "Membagi Memori Menjadi Blok Berukuran Tetap." },
  { "en": "Apa Itu Page Fault?", "id": "Terjadi Saat Halaman Yang Dibutuhkan Tidak Di RAM." },
  { "en": "Apa Itu Memory Management Unit (MMU)?", "id": "Perangkat Keras Untuk Mengelola Memori Virtual." },
  { "en": "Apa Itu Komputer Paralel?", "id": "Menggunakan Beberapa Prosesor Secara Bersamaan." },
  { "en": "Apa Itu Multiprocessor?", "id": "Sistem Komputer Dengan Lebih Dari Satu CPU." },
  { "en": "Apa Itu Multicore Processor?", "id": "Chip Tunggal Dengan Beberapa Core CPU." },
  { "en": "Apa Itu Hukum Amdahl?", "id": "Membatasi Peningkatan Kinerja Dari Paralelisasi." },
  { "en": "Apa Itu SIMD?", "id": "Single Instruction, Multiple Data." },
  { "en": "Apa Itu MIMD?", "id": "Multiple Instruction, Multiple Data." },
  { "en": "Apa Itu GPU (Graphics Processing Unit)?", "id": "Prosesor Khusus Dengan Banyak Core." },
  { "en": "Aplikasi GPU?", "id": "Grafis, Komputasi Ilmiah, Machine Learning." },
  { "en": "Apa Itu ASIC (Application-Specific Integrated Circuit)?", "id": "Chip Yang Didesain Untuk Tugas Sangat Spesifik." },
  { "en": "Apa Itu FPGA (Field-Programmable Gate Array)?", "id": "Chip Yang Logikanya Dapat Dikonfigurasi Ulang." },
  { "en": "Apa Itu System on a Chip (SoC)?", "id": "Mengintegrasikan Semua Komponen Sistem Dalam Satu Chip." },
  { "en": "Apa Itu DSP (Digital Signal Processor)?", "id": "Mikroprosesor Yang Dioptimalkan Untuk Pemrosesan Sinyal." },
  { "en": "Arsitektur DSP?", "id": "Biasanya Harvard Termodifikasi." },
  { "en": "Apa Itu MAC (Multiply-Accumulate) Unit?", "id": "Operasi Kunci Dalam Algoritma DSP." },
  { "en": "Apa Itu Bus Arbitration?", "id": "Proses Mengatur Siapa Yang Boleh Menggunakan Bus." },
  { "en": "Apa Itu DMA (Direct Memory Access)?", "id": "Memungkinkan Periferal Mengakses Memori Langsung." },
  { "en": "Apa Itu Fault Tolerance?", "id": "Kemampuan Sistem Untuk Tetap Bekerja." },
  { "en": "Apa Itu Redundansi?", "id": "Menggunakan Komponen Cadangan Untuk Keandalan." },
  { "en": "Apa Itu Kode Koreksi Error (ECC)?", "id": "Error-Correcting Code." },
  { "en": "Fungsi ECC Memory?", "id": "Mendeteksi Dan Memperbaiki Kesalahan Memori." },
  { "en": "Apa Itu Verilog?", "id": "Bahasa Deskripsi Perangkat Keras (HDL)." },
  { "en": "Apa Itu VHDL?", "id": "Bahasa Deskripsi Perangkat Keras Lainnya." },
  { "en": "Apa Itu Sintesis Logika?", "id": "Mengubah Kode HDL Menjadi Desain Gerbang." },
  { "en": "Apa Itu Place and Route?", "id": "Menempatkan Dan Menghubungkan Gerbang Di Chip." },
  { "en": "Apa Itu Static Timing Analysis (STA)?", "id": "Menganalisis Waktu Tunda Sirkuit Tanpa Simulasi." },
  { "en": "Apa Itu Desain Sinkron?", "id": "Desain Digital Yang Mengandalkan Sinyal Clock." },
  { "en": "Apa Itu Clock Domain Crossing (CDC)?", "id": "Mentransfer Data Antara Bagian Dengan Clock Berbeda." },
  { "en": "Apa Itu Antarmuka (Interface)?", "id": "Batas Antara Dua Sistem Atau Komponen." },
  { "en": "Contoh Antarmuka?", "id": "USB, SATA, PCIe." },
  { "en": "Apa Itu USB (Universal Serial Bus)?", "id": "Standar Antarmuka Serial Populer." },
  { "en": "Apa Itu PCIe (Peripheral Component Interconnect Express)?", "id": "Antarmuka Bus Berkecepatan Tinggi." },
  { "en": "Apa Itu Firmware?", "id": "Perangkat Lunak Tingkat Rendah." },
  { "en": "Apa Itu BIOS/UEFI?", "id": "Firmware Yang Menginisialisasi Komputer Saat Boot." },
  { "en": "Apa Itu Sistem Operasi?", "id": "Perangkat Lunak Yang Mengelola Sumber Daya Komputer." },
  { "en": "Apa Itu Kernel?", "id": "Inti Dari Sistem Operasi." },
  { "en": "Apa Itu Device Driver?", "id": "Perangkat Lunak Yang Mengontrol Perangkat Keras." },
  { "en": "Apa Itu Jaringan Sensor?", "id": "Jaringan Terdistribusi Dari Sensor Otonom." },
  { "en": "Apa Itu Komputasi Tertanam?", "id": "Komputasi Yang Menjadi Bagian Dari Sistem Lebih Besar." },
  { "en": "Apa Itu Desain Daya Rendah?", "id": "Teknik Untuk Mengurangi Konsumsi Energi." },
  { "en": "Apa Itu Clock Gating?", "id": "Mematikan Clock Ke Bagian Yang Tidak Digunakan." },
  { "en": "Apa Itu Power Gating?", "id": "Mematikan Catu Daya Ke Bagian Sirkuit." },
  { "en": "Apa Itu Dynamic Voltage and Frequency Scaling (DVFS)?", "id": "Menyesuaikan Tegangan Dan Frekuensi." },
  { "en": "Apa Itu Verifikasi Formal?", "id": "Menggunakan Metode Matematis Untuk Membuktikan Kebenaran." },
  { "en": "Apa Itu Model Checking?", "id": "Teknik Verifikasi Formal Otomatis." },
  { "en": "Apa Itu Testbench?", "id": "Kode Untuk Memverifikasi Fungsionalitas Desain." },
  { "en": "Apa Itu Code Coverage?", "id": "Metrik Seberapa Banyak Kode Yang Diuji." },
  { "en": "Apa Itu Design for Test (DFT)?", "id": "Mendesain Chip Agar Mudah Diuji." },
  { "en": "Apa Itu Scan Chain?", "id": "Teknik DFT Untuk Mengakses Flip-Flop Internal." },
  { "en": "Apa Itu Built-In Self-Test (BIST)?", "id": "Sirkuit Yang Dapat Menguji Dirinya Sendiri." },
  { "en": "Apa Itu JTAG?", "id": "Joint Test Action Group, Standar Untuk Pengujian." },
  { "en": "Apa Itu Analog-to-Digital Converter (ADC)?", "id": "Mengubah Sinyal Analog Ke Digital." },
  { "en": "Apa Itu Flash ADC?", "id": "ADC Paling Cepat." },
  { "en": "Apa Itu Successive Approximation ADC?", "id": "ADC Yang Umum Digunakan." },
  { "en": "Apa Itu Sigma-Delta ADC?", "id": "ADC Dengan Resolusi Sangat Tinggi." },
  { "en": "Apa Itu Resolusi ADC?", "id": "Jumlah Bit Dalam Hasil Konversi." },
  { "en": "Apa Itu Laju Sampling?", "id": "Seberapa Sering Konversi Dilakukan." },
  { "en": "Apa Itu Digital-to-Analog Converter (DAC)?", "id": "Mengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Itu R-2R Ladder DAC?", "id": "Arsitektur DAC Yang Umum." },
  { "en": "Apa Itu Phase-Locked Loop (PLL)?", "id": "Sirkuit Umpan Balik Untuk Menghasilkan Clock." },
  { "en": "Aplikasi PLL?", "id": "Sintesis Frekuensi, Pemulihan Clock." },
  { "en": "Apa Itu Delay-Locked Loop (DLL)?", "id": "Mirip PLL, Tapi Mengontrol Tundaan." },
  { "en": "Apa Itu Sirkuit Mixed-Signal?", "id": "Sirkuit Yang Menggabungkan Analog Dan Digital." },
  { "en": "Apa Itu Emisi Elektromagnetik?", "id": "Sirkuit Digital Dapat Menjadi Sumber Interferensi." },
  { "en": "Apa Itu Power Integrity?", "id": "Menjaga Tegangan Catu Daya Tetap Bersih." },
  { "en": "Apa Itu Signal Integrity?", "id": "Menjaga Kualitas Sinyal Listrik." },
  { "en": "Apa Itu Refleksi Sinyal?", "id": "Terjadi Akibat Ketidakcocokan Impedansi." },
  { "en": "Apa Itu Crosstalk?", "id": "Interferensi Antara Jalur Sinyal Berdekatan." },
  { "en": "Apa Itu Terminasi?", "id": "Menggunakan Resistor Untuk Mencegah Refleksi." },
  { "en": "Apa Itu Instrumentasi?", "id": "Ilmu Dan Teknologi Pengukuran." },
  { "en": "Apa Itu Pengukuran?", "id": "Proses Mendapatkan Nilai Kuantitatif." },
  { "en": "Apa Itu Instrumen?", "id": "Alat Yang Digunakan Untuk Mengukur." },
  { "en": "Apa Itu Akurasi?", "id": "Kedekatan Pengukuran Dengan Nilai Sebenarnya." },
  { "en": "Apa Itu Presisi?", "id": "Kedekatan Pengukuran Berulang Satu Sama Lain." },
  { "en": "Apa Itu Resolusi?", "id": "Perubahan Terkecil Yang Dapat Diukur." },
  { "en": "Apa Itu Sensitivitas?", "id": "Rasio Perubahan Output Terhadap Input." },
  { "en": "Apa Itu Error (Kesalahan)?", "id": "Perbedaan Antara Nilai Terukur Dan Sebenarnya." },
  { "en": "Apa Itu Error Sistematis?", "id": "Error Yang Konsisten Dan Dapat Diprediksi." },
  { "en": "Apa Itu Error Acak?", "id": "Fluktuasi Yang Tidak Dapat Diprediksi." },
  { "en": "Apa Itu Kalibrasi?", "id": "Membandingkan Instrumen Dengan Standar Yang Diketahui." },
  { "en": "Apa Itu Standar Pengukuran?", "id": "Referensi Yang Diterima Secara Universal." },
  { "en": "Apa Itu Multimeter Digital (DMM)?", "id": "Alat Ukur Elektronik Serbaguna." },
  { "en": "Fungsi DMM?", "id": "Mengukur Tegangan, Arus, Resistansi." },
  { "en": "Apa Itu Impedansi Input Voltmeter?", "id": "Harus Sangat Tinggi." },
  { "en": "Mengapa Impedansi Input Voltmeter Tinggi?", "id": "Agar Tidak Membebani Rangkaian Yang Diukur." },
  { "en": "Apa Itu Resistansi Shunt Amperemeter?", "id": "Harus Sangat Rendah." },
  { "en": "Mengapa Resistansi Shunt Amperemeter Rendah?", "id": "Agar Tidak Mengubah Arus Yang Diukur." },
  { "en": "Apa Itu Pengukuran AC True RMS?", "id": "Mengukur Nilai Efektif Bentuk Gelombang Apapun." },
  { "en": "Apa Itu Osiloskop?", "id": "Memvisualisasikan Sinyal Tegangan Terhadap Waktu." },
  { "en": "Apa Itu Sumbu Vertikal Osiloskop?", "id": "Merepresentasikan Tegangan (Volt/Div)." },
  { "en": "Apa Itu Sumbu Horisontal Osiloskop?", "id": "Merepresentasikan Waktu (Time/Div)." },
  { "en": "Apa Itu Triggering?", "id": "Menstabilkan Tampilan Bentuk Gelombang Berulang." },
  { "en": "Apa Itu Bandwidth Osiloskop?", "id": "Frekuensi Maksimum Yang Dapat Diukur Akurat." },
  { "en": "Apa Itu Laju Sampling?", "id": "Seberapa Cepat Osiloskop Mengambil Sampel." },
  { "en": "Apa Itu Osiloskop Digital (DSO)?", "id": "Mendigitalkan Dan Menyimpan Bentuk Gelombang." },
  { "en": "Apa Itu Osiloskop Analog?", "id": "Menggunakan Tabung Sinar Katoda (CRT)." },
  { "en": "Apa Itu Probe Osiloskop?", "id": "Menghubungkan Osiloskop Ke Rangkaian." },
  { "en": "Apa Itu Atenuasi Probe (10x)?", "id": "Mengurangi Beban Pada Rangkaian." },
  { "en": "Apa Itu Function Generator?", "id": "Menghasilkan Sinyal Uji (Sinus, Kotak, Segitiga)." },
  { "en": "Apa Itu Arbitrary Waveform Generator (AWG)?", "id": "Dapat Menghasilkan Bentuk Gelombang Apapun." },
  { "en": "Apa Itu Power Supply?", "id": "Menyediakan Tegangan Dan Arus DC Stabil." },
  { "en": "Apa Itu Electronic Load?", "id": "Beban Terprogram Untuk Menguji Catu Daya." },
  { "en": "Apa Itu Frequency Counter?", "id": "Mengukur Frekuensi Sinyal Dengan Sangat Akurat." },
  { "en": "Apa Itu Spectrum Analyzer?", "id": "Menampilkan Komponen Frekuensi Sinyal." },
  { "en": "Apa Itu Domain Frekuensi?", "id": "Sinyal Dilihat Sebagai Kumpulan Frekuensi." },
  { "en": "Apa Itu Logic Analyzer?", "id": "Menganalisis Banyak Sinyal Digital Secara Bersamaan." },
  { "en": "Perbedaan Osiloskop Dan Logic Analyzer?", "id": "Osiloskop Untuk Analog, Logic Analyzer Untuk Digital." },
  { "en": "Apa Itu LCR Meter?", "id": "Mengukur Induktansi, Kapasitansi, Dan Resistansi." },
  { "en": "Apa Itu Jembatan Wheatstone?", "id": "Sirkuit Klasik Untuk Pengukuran Resistansi Presisi." },
  { "en": "Apa Itu Sensor?", "id": "Mengubah Besaran Fisik Menjadi Sinyal Listrik." },
  { "en": "Apa Itu Transduser?", "id": "Perangkat Yang Mengubah Energi." },
  { "en": "Apa Itu Pengkondisian Sinyal?", "id": "Memproses Sinyal Sensor Sebelum Diukur." },
  { "en": "Tujuan Pengkondisian Sinyal?", "id": "Amplifikasi, Filtering, Isolasi." },
  { "en": "Apa Itu Instrumentasi Amplifier?", "id": "Penguat Diferensial Presisi Untuk Sensor." },
  { "en": "Apa Itu Strain Gauge?", "id": "Sensor Untuk Mengukur Regangan Mekanis." },
  { "en": "Apa Itu Load Cell?", "id": "Mengukur Gaya Atau Berat." },
  { "en": "Apa Itu Termokopel?", "id": "Sensor Suhu Yang Menghasilkan Tegangan." },
  { "en": "Apa Itu Cold Junction Compensation?", "id": "Diperlukan Untuk Pengukuran Termokopel Akurat." },
  { "en": "Apa Itu RTD (Resistance Temperature Detector)?", "id": "Resistansi Berubah Secara Linear Dengan Suhu." },
  { "en": "Apa Itu Termistor?", "id": "Resistor Sangat Sensitif Terhadap Suhu." },
  { "en": "Apa Itu LVDT (Linear Variable Differential Transformer)?", "id": "Sensor Perpindahan Linear Yang Sangat Akurat." },
  { "en": "Apa Itu Sistem Akuisisi Data (DAQ)?", "id": "Mengumpulkan Dan Merekam Data Dari Sensor." },
  { "en": "Apa Itu Noise?", "id": "Sinyal Tak Diinginkan Yang Mengganggu Pengukuran." },
  { "en": "Apa Itu Noise Termal?", "id": "Dihasilkan Oleh Gerakan Acak Elektron." },
  { "en": "Apa Itu Interferensi Elektromagnetik (EMI)?", "id": "Gangguan Dari Sumber Elektromagnetik Eksternal." },
  { "en": "Apa Itu Shielding?", "id": "Menggunakan Konduktor Untuk Memblokir EMI." },
  { "en": "Apa Itu Grounding?", "id": "Menyediakan Titik Referensi Bersama." },
  { "en": "Apa Itu Ground Loop?", "id": "Masalah Noise Akibat Beberapa Jalur Ground." },
  { "en": "Apa Itu Kabel Twisted Pair?", "id": "Mengurangi Interferensi Magnetik." },
  { "en": "Apa Itu Kabel Koaksial?", "id": "Kabel Dengan Pelindung Untuk Sinyal Frekuensi Tinggi." },
  { "en": "Apa Itu Pengukuran Diferensial?", "id": "Mengukur Perbedaan Antara Dua Sinyal." },
  { "en": "Manfaat Pengukuran Diferensial?", "id": "Sangat Baik Dalam Menolak Common-Mode Noise." },
  { "en": "Apa Itu GPIB/IEEE-488?", "id": "Standar Bus Untuk Mengontrol Instrumen." },
  { "en": "Apa Itu VXI/PXI?", "id": "Standar Instrumentasi Modular." },
  { "en": "Apa Itu LabVIEW?", "id": "Lingkungan Pemrograman Grafis Untuk Instrumentasi." },
  { "en": "Apa Itu Virtual Instrument?", "id": "Instrumen Berbasis Perangkat Lunak Dan DAQ." },
  { "en": "Apa Itu Network Analyzer?", "id": "Mengukur Karakteristik Jaringan (Filter, Amplifier)." },
  { "en": "Apa Itu S-parameter?", "id": "Parameter Yang Mendeskripsikan Jaringan Frekuensi Tinggi." },
  { "en": "Apa Itu Power Meter?", "id": "Mengukur Daya Sinyal Frekuensi Radio." },
  { "en": "Apa Itu Data Logger?", "id": "Merekam Data Pengukuran Selama Periode Waktu." },
  { "en": "Apa Itu Uji Kompatibilitas Elektromagnetik (EMC)?", "id": "Menguji Emisi Dan Kekebalan Perangkat." },
  { "en": "Apa Itu Antena?", "id": "Transduser Antara Gelombang Terpandu Dan Ruang Bebas." },
  { "en": "Apa Itu Time-Domain Reflectometry (TDR)?", "id": "Teknik Untuk Menemukan Kesalahan Pada Kabel." },
  { "en": "Bagaimana TDR Bekerja?", "id": "Mengirim Pulsa Dan Menganalisis Pantulannya." },
  { "en": "Apa Itu Pengukuran Empat Kawat?", "id": "Metode Untuk Mengukur Resistansi Rendah Akurat." },
  { "en": "Tujuan Pengukuran Empat Kawat?", "id": "Mengeliminasi Error Akibat Resistansi Kabel." },
  { "en": "Apa Itu Standar Kalibrasi Primer?", "id": "Standar Paling Akurat Di Tingkat Nasional." },
  { "en": "Apa Itu Ketertelusuran (Traceability)?", "id": "Menghubungkan Pengukuran Ke Standar Nasional." },
  { "en": "Apa Itu Ketidakpastian Pengukuran?", "id": "Rentang Dimana Nilai Sebenarnya Mungkin Berada." },
  { "en": "Apa Itu Sistem Satuan Internasional (SI)?", "id": "Sistem Satuan Standar Global." },
  { "en": "Apa Itu Otomasi Pengujian?", "id": "Menggunakan Komputer Untuk Mengontrol Instrumen." },
  { "en": "Apa Itu Linearitas Instrumen?", "id": "Seberapa Lurus Respon Instrumen." },
  { "en": "Apa Itu Histeresis?", "id": "Perbedaan Pembacaan Saat Naik Dan Turun." },
  { "en": "Apa Itu Drift?", "id": "Perubahan Pembacaan Instrumen Seiring Waktu." },
  { "en": "Apa Itu Loading Effect?", "id": "Instrumen Mengubah Perilaku Rangkaian Yang Diukur." },
  { "en": "Bagaimana Mengurangi Loading Effect?", "id": "Menggunakan Instrumen Dengan Impedansi Tepat." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Komponen Kunci Dalam Instrumen Digital." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Komponen Kunci Dalam Generator Sinyal." },
  { "en": "Apa Itu Pengambilan Sampel (Sampling)?", "id": "Kunci Untuk Mengubah Sinyal Analog Ke Digital." },
  { "en": "Apa Itu Aliasing?", "id": "Error Akibat Laju Sampling Terlalu Lambat." },
  { "en": "Apa Itu Telekomunikasi?", "id": "Komunikasi Jarak Jauh Menggunakan Teknologi." },
  { "en": "Apa Itu Sinyal?", "id": "Representasi Fisik Dari Informasi." },
  { "en": "Apa Itu Transmisi?", "id": "Proses Pengiriman Sinyal." },
  { "en": "Apa Itu Kanal?", "id": "Medium Fisik Tempat Sinyal Merambat." },
  { "en": "Apa Itu Pemancar (Transmitter)?", "id": "Mengubah Informasi Menjadi Sinyal Yang Sesuai." },
  { "en": "Apa Itu Penerima (Receiver)?", "id": "Mengubah Sinyal Kembali Menjadi Informasi." },
  { "en": "Apa Itu Modulasi?", "id": "Menumpangkan Informasi Ke Gelombang Pembawa." },
  { "en": "Apa Itu Gelombang Pembawa (Carrier)?", "id": "Gelombang Frekuensi Tinggi." },
  { "en": "Apa Itu Demodulasi?", "id": "Memisahkan Informasi Dari Gelombang Pembawa." },
  { "en": "Apa Itu Sistem Komunikasi Analog?", "id": "Mengirimkan Sinyal Analog." },
  { "en": "Apa Itu Sistem Komunikasi Digital?", "id": "Mengirimkan Informasi Dalam Bentuk Digital." },
  { "en": "Apa Itu Modulasi Amplitudo (AM)?", "id": "Mengubah Amplitudo Pembawa." },
  { "en": "Apa Itu Modulasi Frekuensi (FM)?", "id": "Mengubah Frekuensi Pembawa." },
  { "en": "Apa Itu Spektrum Frekuensi?", "id": "Rentang Frekuensi Yang Digunakan." },
  { "en": "Apa Itu Alokasi Spektrum?", "id": "Pembagian Spektrum Frekuensi Oleh Regulator." },
  { "en": "Apa Itu Bandwidth?", "id": "Lebar Pita Frekuensi Yang Dibutuhkan Sinyal." },
  { "en": "Apa Itu Noise?", "id": "Gangguan Acak Yang Merusak Sinyal." },
  { "en": "Apa Itu SNR?", "id": "Signal-to-Noise Ratio (Rasio Sinyal-ke-Noise)." },
  { "en": "Apa Itu Atenuasi?", "id": "Pelemahan Kekuatan Sinyal Seiring Jarak." },
  { "en": "Apa Itu Distorsi?", "id": "Perubahan Bentuk Sinyal Yang Tidak Diinginkan." },
  { "en": "Apa Itu Repeater?", "id": "Menguatkan Dan Mengirim Ulang Sinyal." },
  { "en": "Apa Itu Multiplexing?", "id": "Mengirim Banyak Sinyal Melalui Satu Kanal." },
  { "en": "Apa Itu FDM (Frequency Division Multiplexing)?", "id": "Membagi Kanal Berdasarkan Frekuensi." },
  { "en": "Apa Itu TDM (Time Division Multiplexing)?", "id": "Membagi Kanal Berdasarkan Waktu." },
  { "en": "Apa Itu Antena?", "id": "Mengubah Sinyal Listrik Menjadi Gelombang Elektromagnetik." },
  { "en": "Apa Itu Propagasi Gelombang Radio?", "id": "Cara Gelombang Radio Merambat Di Atmosfer." },
  { "en": "Apa Itu Ground Wave?", "id": "Merambat Mengikuti Permukaan Bumi." },
  { "en": "Apa Itu Sky Wave?", "id": "Dipantulkan Oleh Lapisan Ionosfer." },
  { "en": "Apa Itu Line-of-Sight?", "id": "Transmisi Garis Lurus." },
  { "en": "Apa Itu Sistem Telepon?", "id": "Jaringan Untuk Komunikasi Suara." },
  { "en": "Apa Itu Sentral Telepon (Switch)?", "id": "Menghubungkan Panggilan Antar Pelanggan." },
  { "en": "Apa Itu PSTN (Public Switched Telephone Network)?", "id": "Jaringan Telepon Kabel Global." },
  { "en": "Apa Itu Sistem Seluler?", "id": "Komunikasi Nirkabel Menggunakan Jaringan Sel." },
  { "en": "Apa Itu Sel?", "id": "Area Geografis Yang Dilayani Satu Base Station." },
  { "en": "Apa Itu Base Station?", "id": "Menara Dengan Antena Untuk Komunikasi Seluler." },
  { "en": "Apa Itu Handoff?", "id": "Proses Berpindah Antara Sel Saat Bergerak." },
  { "en": "Generasi Komunikasi Seluler?", "id": "1G, 2G, 3G, 4G, 5G." },
  { "en": "Apa Itu Komunikasi Satelit?", "id": "Menggunakan Satelit Di Orbit Sebagai Repeater." },
  { "en": "Apa Itu Orbit Geostasioner (GEO)?", "id": "Satelit Tampak Diam Dari Bumi." },
  { "en": "Apa Itu Uplink?", "id": "Sinyal Dari Bumi Ke Satelit." },
  { "en": "Apa Itu Downlink?", "id": "Sinyal Dari Satelit Ke Bumi." },
  { "en": "Apa Itu Transponder?", "id": "Penerima Dan Pemancar Di Satelit." },
  { "en": "Aplikasi Satelit?", "id": "Siaran TV, GPS, Komunikasi Jarak Jauh." },
  { "en": "Apa Itu GPS (Global Positioning System)?", "id": "Sistem Navigasi Global Berbasis Satelit." },
  { "en": "Apa Itu Komunikasi Serat Optik?", "id": "Mengirim Data Sebagai Pulsa Cahaya." },
  { "en": "Keuntungan Serat Optik?", "id": "Kapasitas Sangat Besar, Tahan Interferensi." },
  { "en": "Prinsip Serat Optik?", "id": "Pemantulan Internal Total." },
  { "en": "Apa Itu Jaringan Komputer?", "id": "Interkoneksi Perangkat Untuk Berbagi Sumber Daya." },
  { "en": "Apa Itu LAN (Local Area Network)?", "id": "Jaringan Dalam Area Terbatas (Gedung)." },
  { "en": "Apa Itu WAN (Wide Area Network)?", "id": "Jaringan Yang Mencakup Area Geografis Luas." },
  { "en": "Apa Itu Internet?", "id": "Jaringan Global Dari Jaringan Komputer." },
  { "en": "Apa Itu Protokol?", "id": "Sekumpulan Aturan Untuk Komunikasi Data." },
  { "en": "Apa Itu TCP/IP?", "id": "Suite Protokol Dasar Untuk Internet." },
  { "en": "Apa Itu Alamat IP?", "id": "Alamat Unik Perangkat Di Jaringan." },
  { "en": "Apa Itu Router?", "id": "Perangkat Yang Meneruskan Data Antar Jaringan." },
  { "en": "Apa Itu Modem?", "id": "MOdulator-DEModulator." },
  { "en": "Fungsi Modem?", "id": "Menghubungkan Jaringan Digital Ke Saluran Analog." },
  { "en": "Apa Itu Wi-Fi?", "id": "Teknologi Jaringan Area Lokal Nirkabel." },
  { "en": "Apa Itu Bluetooth?", "id": "Teknologi Nirkabel Jarak Pendek." },
  { "en": "Apa Itu Siaran Radio?", "id": "Transmisi Sinyal Ke Banyak Penerima." },
  { "en": "Apa Itu Siaran Televisi?", "id": "Transmisi Gambar Dan Suara." },
  { "en": "Apa Itu Televisi Digital?", "id": "Menggunakan Sinyal Digital Untuk Siaran." },
  { "en": "Apa Itu Kompresi Data?", "id": "Mengurangi Ukuran Data Untuk Transmisi." },
  { "en": "Apa Itu Kompresi Lossless?", "id": "Data Asli Dapat Dipulihkan Sempurna." },
  { "en": "Apa Itu Kompresi Lossy?", "id": "Beberapa Informasi Dihilangkan Untuk Ukuran Kecil." },
  { "en": "Contoh Kompresi Lossy?", "id": "JPEG, MP3." },
  { "en": "Apa Itu Pengkodean Kanal (Channel Coding)?", "id": "Menambahkan Bit Redundansi Untuk Melawan Error." },
  { "en": "Apa Itu Parity Bit?", "id": "Metode Deteksi Error Sederhana." },
  { "en": "Apa Itu Teori Informasi?", "id": "Studi Matematis Tentang Kuantifikasi Informasi." },
  { "en": "Apa Itu Bit?", "id": "Unit Dasar Informasi." },
  { "en": "Apa Itu Bit Rate?", "id": "Jumlah Bit Yang Dikirim Per Detik." },
  { "en": "Apa Itu Baud Rate?", "id": "Jumlah Perubahan Sinyal Per Detik." },
  { "en": "Apa Itu Kabel Twisted Pair?", "id": "Kabel Tembaga Umum Untuk Telepon LAN." },
  { "en": "Apa Itu Kabel Koaksial?", "id": "Digunakan Untuk TV Kabel Dan Jaringan Awal." },
  { "en": "Apa Itu Fading?", "id": "Variasi Kekuatan Sinyal." },
  { "en": "Apa Itu Sistem Radar?", "id": "Menggunakan Gelombang Radio Untuk Mendeteksi Objek." },
  { "en": "Apa Itu Sistem Sonar?", "id": "Menggunakan Gelombang Suara Di Bawah Air." },
  { "en": "Apa Itu Baseband Signal?", "id": "Sinyal Informasi Sebelum Modulasi." },
  { "en": "Apa Itu Passband Signal?", "id": "Sinyal Setelah Dimodulasi Ke Frekuensi Tinggi." },
  { "en": "Apa Itu Duplex?", "id": "Komunikasi Dua Arah." },
  { "en": "Apa Itu Half-Duplex?", "id": "Dua Arah, Tapi Bergantian." },
  { "en": "Apa Itu Full-Duplex?", "id": "Dua Arah, Secara Bersamaan." },
  { "en": "Apa Itu Simplex?", "id": "Komunikasi Hanya Satu Arah." },
  { "en": "Contoh Simplex?", "id": "Siaran Radio Atau TV." },
  { "en": "Apa Itu Jaringan Ad Hoc?", "id": "Jaringan Nirkabel Tanpa Infrastruktur Terpusat." },
  { "en": "Apa Itu Jaringan Sensor?", "id": "Jaringan Terdistribusi Dari Sensor Nirkabel." },
  { "en": "Apa Itu Internet of Things (IoT)?", "id": "Jaringan Perangkat Sehari-hari Yang Terhubung." },
  { "en": "Apa Itu Penguat (Amplifier)?", "id": "Meningkatkan Kekuatan Sinyal." },
  { "en": "Apa Itu Filter?", "id": "Menghilangkan Komponen Frekuensi Yang Tidak Diinginkan." },
  { "en": "Apa Itu Osilator?", "id": "Menghasilkan Sinyal Pembawa (Carrier)." },
  { "en": "Apa Itu Mixer?", "id": "Mengalikan Dua Sinyal (Untuk Modulasi)." },
  { "en": "Apa Itu Propagasi?", "id": "Perambatan Gelombang Melalui Medium." },
  { "en": "Apa Itu Interferensi?", "id": "Gangguan Dari Sinyal Lain." },
  { "en": "Apa Itu Optoelektronika?", "id": "Studi Perangkat Yang Berinteraksi Dengan Cahaya." },
  { "en": "Apa Itu Fotonika?", "id": "Ilmu Dan Teknologi Foton (Cahaya)." },
  { "en": "Apa Itu Foton?", "id": "Partikel Kuantum Dari Cahaya." },
  { "en": "Apa Itu Sifat Ganda Cahaya?", "id": "Cahaya Bersifat Gelombang Dan Partikel." },
  { "en": "Apa Itu Spektrum Elektromagnetik?", "id": "Rentang Gelombang Termasuk Cahaya Tampak." },
  { "en": "Apa Itu Cahaya Tampak?", "id": "Cahaya Yang Dapat Dilihat Mata Manusia." },
  { "en": "Apa Itu Inframerah (Infrared)?", "id": "Radiasi Elektromagnetik Dengan Panjang Gelombang Lebih." },
  { "en": "Apa Itu Ultraviolet (UV)?", "id": "Radiasi Dengan Panjang Gelombang Lebih Pendek." },
  { "en": "Apa Itu LED?", "id": "Light Emitting Diode." },
  { "en": "Bagaimana LED Bekerja?", "id": "Dioda Semikonduktor Yang Memancarkan Cahaya." },
  { "en": "Keuntungan LED?", "id": "Efisien, Awet, Ukuran Kecil." },
  { "en": "Apa Itu OLED?", "id": "Organic Light Emitting Diode." },
  { "en": "Aplikasi OLED?", "id": "Layar Ponsel Dan TV." },
  { "en": "Apa Itu Dioda Laser?", "id": "Sumber Cahaya Laser Berbasis Semikonduktor." },
  { "en": "Apa Itu Laser?", "id": "Light Amplification by Stimulated Emission of Radiation." },
  { "en": "Sifat Cahaya Laser?", "id": "Monokromatik, Koheren, Searah." },
  { "en": "Apa Itu Emisi Spontan?", "id": "Atom Memancarkan Foton Secara Acak." },
  { "en": "Apa Itu Emisi Terstimulasi?", "id": "Foton Memicu Emisi Foton Identik Lainnya." },
  { "en": "Apa Itu Inversi Populasi?", "id": "Kondisi Yang Diperlukan Untuk Terjadinya Lasing." },
  { "en": "Aplikasi Laser?", "id": "Komunikasi, Pembedahan, Pemotongan Logam." },
  { "en": "Apa Itu Fotodetektor?", "id": "Perangkat Yang Mendeteksi Cahaya." },
  { "en": "Apa Itu Fotodioda?", "id": "Dioda Yang Menghasilkan Arus Saat Terkena Cahaya." },
  { "en": "Apa Itu Fototransistor?", "id": "Transistor Yang Dikontrol Oleh Cahaya." },
  { "en": "Apa Itu Photoresistor (LDR)?", "id": "Light Dependent Resistor." },
  { "en": "Bagaimana LDR Bekerja?", "id": "Resistansinya Berkurang Saat Cahaya Terang." },
  { "en": "Apa Itu Sel Surya?", "id": "Fotodioda Area Luas Untuk Menghasilkan Daya." },
  { "en": "Nama Lain Sel Surya?", "id": "Sel Fotovoltaik (PV Cell)." },
  { "en": "Apa Itu Efek Fotovoltaik?", "id": "Pembangkitan Tegangan Di Sambungan P-N." },
  { "en": "Apa Itu CCD (Charge-Coupled Device)?", "id": "Sensor Gambar Elektronik." },
  { "en": "Apa Itu Sensor CMOS?", "id": "Teknologi Sensor Gambar Lainnya." },
  { "en": "Aplikasi Sensor Gambar?", "id": "Kamera Digital, Ponsel." },
  { "en": "Apa Itu Serat Optik?", "id": "Pemandu Cahaya Tipis Yang Fleksibel." },
  { "en": "Bahan Serat Optik?", "id": "Kaca Atau Plastik." },
  { "en": "Prinsip Kerja Serat Optik?", "id": "Pemantulan Internal Total." },
  { "en": "Struktur Serat Optik?", "id": "Inti (Core) Dan Selubung (Cladding)." },
  { "en": "Apa Itu Komunikasi Serat Optik?", "id": "Transmisi Data Menggunakan Cahaya Dalam Serat." },
  { "en": "Keuntungan Komunikasi Serat Optik?", "id": "Kapasitas Besar, Keamanan, Tahan Interferensi." },
  { "en": "Apa Itu Atenuasi?", "id": "Pelemahan Sinyal Cahaya Dalam Serat." },
  { "en": "Apa Itu Dispersi?", "id": "Penyebaran Pulsa Cahaya Seiring Jarak." },
  { "en": "Jenis Dispersi?", "id": "Kromatik Dan Modal." },
  { "en": "Apa Itu Wavelength Division Multiplexing (WDM)?", "id": "Mengirim Banyak Sinyal Cahaya Berbeda Warna." },
  { "en": "Apa Itu Penguat Serat Optik?", "id": "Menguatkan Sinyal Cahaya Langsung." },
  { "en": "Contoh Penguat Serat Optik?", "id": "EDFA (Erbium-Doped Fiber Amplifier)." },
  { "en": "Apa Itu Sensor Serat Optik?", "id": "Menggunakan Serat Optik Untuk Pengukuran." },
  { "en": "Apa Itu Tampilan (Display)?", "id": "Perangkat Output Visual." },
  { "en": "Apa Itu LCD?", "id": "Liquid Crystal Display." },
  { "en": "Bagaimana LCD Bekerja?", "id": "Kristal Cair Mengontrol Lewatnya Cahaya." },
  { "en": "Apa Itu Backlight?", "id": "Sumber Cahaya Di Belakang Panel LCD." },
  { "en": "Apa Itu Tampilan 7-Segmen?", "id": "Tampilan Sederhana Untuk Angka." },
  { "en": "Apa Itu Optocoupler?", "id": "Mengisolasi Rangkaian Menggunakan Pasangan LED-Fotodetektor." },
  { "en": "Apa Itu Solid State Relay (SSR)?", "id": "Relai Yang Menggunakan Optocoupler." },
  { "en": "Apa Itu Fotolitografi?", "id": "Proses Menggunakan Cahaya Untuk Membuat Pola IC." },
  { "en": "Apa Itu Optik Terintegrasi?", "id": "Membuat Komponen Optik Pada Sebuah Chip." },
  { "en": "Apa Itu Pemandu Gelombang (Waveguide)?", "id": "Struktur Yang Memandu Perambatan Cahaya." },
  { "en": "Apa Itu Interferometri?", "id": "Teknik Menggunakan Interferensi Cahaya Untuk Pengukuran." },
  { "en": "Apa Itu Holografi?", "id": "Merekam Dan Merekonstruksi Gambar Tiga Dimensi." },
  { "en": "Apa Itu Spektroskopi?", "id": "Mempelajari Interaksi Cahaya Dan Materi." },
  { "en": "Apa Itu Luminositas?", "id": "Ukuran Kecerahan Cahaya Yang Dirasakan." },
  { "en": "Apa Itu Indeks Bias?", "id": "Ukuran Seberapa Cepat Cahaya Merambat." },
  { "en": "Apa Itu Pemantulan?", "id": "Cahaya Memantul Dari Permukaan." },
  { "en": "Apa Itu Pembiasan?", "id": "Cahaya Berbelok Saat Pindah Medium." },
  { "en": "Apa Itu Difraksi?", "id": "Penyebaran Cahaya Saat Melewati Celah." },
  { "en": "Apa Itu Interferensi?", "id": "Penggabungan Gelombang Cahaya." },
  { "en": "Apa Itu Koherensi?", "id": "Sifat Gelombang Cahaya Dengan Fasa Konstan." },
  { "en": "Apa Itu Lensa?", "id": "Komponen Optik Untuk Memfokuskan Cahaya." },
  { "en": "Apa Itu Cermin?", "id": "Komponen Optik Untuk Memantulkan Cahaya." },
  { "en": "Apa Itu Prisma?", "id": "Komponen Optik Untuk Mendispersi Cahaya." },
  { "en": "Apa Itu Filter Optik?", "id": "Melewatkan Panjang Gelombang Cahaya Tertentu." },
  { "en": "Apa Itu Polarisator?", "id": "Melewatkan Polarisasi Cahaya Tertentu." },
  { "en": "Apa Itu Sumber Cahaya Pijar?", "id": "Menghasilkan Cahaya Dengan Memanaskan Filamen." },
  { "en": "Apa Itu Lampu Fluorescent?", "id": "Menggunakan Gas Dan Lapisan Fosfor." },
  { "en": "Apa Itu Efisiensi Luminus?", "id": "Rasio Cahaya Tampak Terhadap Total Daya." },
  { "en": "Apa Itu Temperatur Warna?", "id": "Mendeskripsikan Tampilan Warna Cahaya." },
  { "en": "Apa Itu Color Rendering Index (CRI)?", "id": "Kemampuan Sumber Cahaya Menampilkan Warna." },
  { "en": "Apa Itu Proyektor?", "id": "Menampilkan Gambar Ke Permukaan Besar." },
  { "en": "Apa Itu Barcode Scanner?", "id": "Membaca Kode Batang Menggunakan Laser Atau LED." },
  { "en": "Apa Itu Kamera?", "id": "Merekam Gambar Menggunakan Lensa Dan Sensor." },
  { "en": "Apa Itu Mikroskop?", "id": "Melihat Objek Yang Sangat Kecil." },
  { "en": "Apa Itu Teleskop?", "id": "Melihat Objek Yang Sangat Jauh." },
  { "en": "Apa Itu LIDAR?", "id": "Light Detection and Ranging." },
  { "en": "Bagaimana LIDAR Bekerja?", "id": "Mengukur Jarak Menggunakan Pulsa Laser." },
  { "en": "Apa Itu Remote Control Inframerah?", "id": "Mengirim Sinyal Menggunakan LED Inframerah." },
  { "en": "Apa Itu Pencitraan Termal?", "id": "Memvisualisasikan Panas Menggunakan Radiasi Inframerah." },
  { "en": "Apa Itu Optik Non-Linear?", "id": "Studi Interaksi Cahaya Intens Dengan Materi." },
  { "en": "Apa Itu Pembangkitan Harmonik Kedua?", "id": "Menggandakan Frekuensi Cahaya." },
  { "en": "Apa Itu Laser Cutting?", "id": "Menggunakan Laser Berdaya Tinggi Untuk Memotong." },
  { "en": "Apa Itu Pengelasan Laser?", "id": "Menggunakan Laser Untuk Menyambung Material." },
  { "en": "Apa Itu Pembedahan Laser?", "id": "Penggunaan Laser Dalam Prosedur Medis." },
  { "en": "Apa Itu Fotokimia?", "id": "Studi Reaksi Kimia Yang Dipicu Cahaya." },
  { "en": "Apa Itu Fotosintesis?", "id": "Proses Biologis Menggunakan Energi Cahaya." },
  { "en": "Apa Itu Sirkuit Terpadu (IC)?", "id": "Sirkuit Elektronik Lengkap Dalam Satu Chip." },
  { "en": "Nama Lain IC?", "id": "Chip, Microchip." },
  { "en": "Bahan Dasar IC?", "id": "Silikon (Silicon)." },
  { "en": "Mengapa Silikon Digunakan?", "id": "Merupakan Semikonduktor Yang Melimpah." },
  { "en": "Apa Itu Fabrikasi IC?", "id": "Proses Manufaktur Sirkuit Terpadu." },
  { "en": "Apa Itu Wafer?", "id": "Kepingan Tipis Silikon." },
  { "en": "Apa Itu Die (Jamak: Dice)?", "id": "Satu Chip Individual Di Atas Wafer." },
  { "en": "Apa Itu Fotolitografi?", "id": "Proses Kunci Untuk Mencetak Pola Sirkuit." },
  { "en": "Apa Itu Photoresist?", "id": "Bahan Peka Cahaya Yang Digunakan." },
  { "en": "Apa Itu Mask?", "id": "Template Pola Sirkuit." },
  { "en": "Apa Itu Etsa (Etching)?", "id": "Menghilangkan Material Yang Tidak Diinginkan." },
  { "en": "Apa Itu Deposisi?", "id": "Menambahkan Lapisan Tipis Material." },
  { "en": "Apa Itu Doping?", "id": "Memasukkan Atom Pengotor Ke Silikon." },
  { "en": "Apa Itu Implantasi Ion?", "id": "Metode Doping Dengan Menembakkan Ion." },
  { "en": "Apa Itu Ruang Bersih (Cleanroom)?", "id": "Lingkungan Manufaktur Yang Sangat Bersih." },
  { "en": "Mengapa Cleanroom Dibutuhkan?", "id": "Partikel Debu Dapat Merusak Sirkuit." },
  { "en": "Apa Itu Skala Integrasi?", "id": "Ukuran Kepadatan Komponen Pada Chip." },
  { "en": "Apa Itu SSI (Small-Scale Integration)?", "id": "Kurang Dari 100 Transistor." },
  { "en": "Apa Itu MSI (Medium-Scale Integration)?", "id": "100 Hingga Ribuan Transistor." },
  { "en": "Apa Itu LSI (Large-Scale Integration)?", "id": "Ribuan Hingga Ratusan Ribu Transistor." },
  { "en": "Apa Itu VLSI (Very Large-Scale Integration)?", "id": "Lebih Dari Ratusan Ribu Transistor." },
  { "en": "Apa Itu Hukum Moore?", "id": "Prediksi Kepadatan Transistor Berlipat Ganda." },
  { "en": "Apa Itu Teknologi Proses (Node)?", "id": "Ukuran Fitur Terkecil Di Chip (Contoh: 7nm)." },
  { "en": "Apa Itu IC Analog?", "id": "Memproses Sinyal Kontinu." },
  { "en": "Contoh IC Analog?", "id": "Op-Amp, Regulator Tegangan." },
  { "en": "Apa Itu IC Digital?", "id": "Memproses Sinyal Biner." },
  { "en": "Contoh IC Digital?", "id": "Mikroprosesor, Memori, Gerbang Logika." },
  { "en": "Apa Itu IC Mixed-Signal?", "id": "Menggabungkan Sirkuit Analog Dan Digital." },
  { "en": "Contoh IC Mixed-Signal?", "id": "ADC, DAC, SoC." },
  { "en": "Apa Itu SoC (System on a Chip)?", "id": "Sistem Lengkap Dalam Satu Chip." },
  { "en": "Apa Itu ASIC (Application-Specific IC)?", "id": "IC Yang Dirancang Untuk Tugas Khusus." },
  { "en": "Apa Itu FPGA (Field-Programmable Gate Array)?", "id": "IC Digital Yang Dapat Diprogram Ulang." },
  { "en": "Apa Itu Kemasan IC (Packaging)?", "id": "Wadah Yang Melindungi Chip." },
  { "en": "Fungsi Kemasan IC?", "id": "Melindungi, Menghubungkan, Membuang Panas." },
  { "en": "Apa Itu Pin?", "id": "Kaki-kaki Logam Untuk Koneksi Eksternal." },
  { "en": "Apa Itu DIP (Dual In-line Package)?", "id": "Jenis Kemasan Klasik Dengan Dua Baris Pin." },
  { "en": "Apa Itu SMT (Surface-Mount Technology)?", "id": "Komponen Dipasang Di Permukaan PCB." },
  { "en": "Apa Itu BGA (Ball Grid Array)?", "id": "Kemasan Menggunakan Bola Solder Di Bawahnya." },
  { "en": "Apa Itu CMOS?", "id": "Complementary Metal-Oxide-Semiconductor." },
  { "en": "Mengapa CMOS Populer?", "id": "Konsumsi Daya Statis Sangat Rendah." },
  { "en": "Struktur Inverter CMOS?", "id": "Terdiri Dari PMOS Dan NMOS." },
  { "en": "Apa Itu PMOS?", "id": "MOSFET Tipe P." },
  { "en": "Apa Itu NMOS?", "id": "MOSFET Tipe N." },
  { "en": "Apa Itu Latch-up?", "id": "Masalah Hubung Singkat Pada Sirkuit CMOS." },
  { "en": "Apa Itu Yield?", "id": "Persentase Chip Fungsional Per Wafer." },
  { "en": "Apa Itu Pengujian IC?", "id": "Memastikan Setiap Chip Bekerja Dengan Benar." },
  { "en": "Apa Itu Desain IC?", "id": "Proses Merancang Sirkuit Di Dalam Chip." },
  { "en": "Apa Itu Tata Letak (Layout)?", "id": "Gambar Geometris Fisik Dari Sirkuit." },
  { "en": "Apa Itu EDA (Electronic Design Automation)?", "id": "Perangkat Lunak Untuk Mendesain IC." },
  { "en": "Apa Itu Verifikasi Desain?", "id": "Memastikan Desain Sesuai Spesifikasi." },
  { "en": "Apa Itu Simulasi Sirkuit?", "id": "Menggunakan Komputer Untuk Menganalisis Perilaku Sirkuit." },
  { "en": "Apa Itu SPICE?", "id": "Program Simulasi Sirkuit Populer." },
  { "en": "Apa Itu Library Sel Standar?", "id": "Kumpulan Gerbang Logika Yang Telah Didesain." },
  { "en": "Apa Itu Power Dissipation?", "id": "Energi Yang Berubah Menjadi Panas." },
  { "en": "Apa Itu Daya Statis?", "id": "Daya Yang Dikonsumsi Saat Sirkuit Diam." },
  { "en": "Apa Itu Daya Dinamis?", "id": "Daya Yang Dikonsumsi Saat Sirkuit Beralih." },
  { "en": "Apa Itu Clock Distribution Network?", "id": "Jaringan Untuk Mendistribusikan Sinyal Clock." },
  { "en": "Apa Itu Clock Skew?", "id": "Perbedaan Waktu Tiba Clock Di Titik Berbeda." },
  { "en": "Apa Itu Power Grid?", "id": "Jaringan Jalur Logam Untuk Catu Daya." },
  { "en": "Apa Itu Interconnect?", "id": "Kabel Logam Yang Menghubungkan Komponen." },
  { "en": "Apa Itu Parasitic Capacitance?", "id": "Kapasitansi Tak Diinginkan Akibat Tata Letak." },
  { "en": "Apa Itu Crosstalk?", "id": "Interferensi Antara Jalur Sinyal Berdekatan." },
  { "en": "Apa Itu Signal Integrity?", "id": "Menjaga Kualitas Sinyal Listrik." },
  { "en": "Apa Itu Power Integrity?", "id": "Menjaga Tegangan Catu Daya Tetap Stabil." },
  { "en": "Apa Itu Decoupling Capacitor?", "id": "Menstabilkan Catu Daya Lokal." },
  { "en": "Apa Itu ESD Protection?", "id": "Electrostatic Discharge Protection." },
  { "en": "Fungsi ESD Protection?", "id": "Melindungi Pin IC Dari Listrik Statis." },
  { "en": "Apa Itu Bonding Wire?", "id": "Kawat Emas Halus Yang Menghubungkan Die Ke Pin." },
  { "en": "Apa Itu Flip-Chip?", "id": "Metode Pemasangan Die Terbalik." },
  { "en": "Apa Itu 3D IC?", "id": "Menumpuk Beberapa Die Secara Vertikal." },
  { "en": "Apa Itu Through-Silicon Via (TSV)?", "id": "Koneksi Vertikal Melalui Die Silikon." },
  { "en": "Apa Itu IP Core?", "id": "Intellectual Property, Blok Desain Yang Dapat Digunakan Ulang." },
  { "en": "Apa Itu Foundry?", "id": "Pabrik Yang Memproduksi Chip Untuk Perusahaan Lain." },
  { "en": "Apa Itu Fabless Company?", "id": "Perusahaan Yang Mendesain Chip Tapi Tidak Memproduksi." },
  { "en": "Apa Itu IDM (Integrated Device Manufacturer)?", "id": "Perusahaan Yang Mendesain Dan Memproduksi Chip." },
  { "en": "Apa Itu FinFET?", "id": "Arsitektur Transistor Tiga Dimensi." },
  { "en": "Apa Itu Gate-All-Around (GAA)?", "id": "Arsitektur Transistor Generasi Berikutnya." },
  { "en": "Apa Itu Memristor?", "id": "Komponen Elektronik Keempat Yang Hipotetis." },
  { "en": "Apa Itu Komputasi Neuromorfik?", "id": "Desain Chip Yang Meniru Otak." },
  { "en": "Apa Itu Fotonika Silikon?", "id": "Mengintegrasikan Komponen Optik Pada Chip Silikon." },
  { "en": "Apa Itu RFIC?", "id": "Radio-Frequency Integrated Circuit." },
  { "en": "Aplikasi RFIC?", "id": "Ponsel, Wi-Fi, Radar." },
  { "en": "Apa Itu MMIC?", "id": "Monolithic Microwave Integrated Circuit." },
  { "en": "Apa Itu Power Management IC (PMIC)?", "id": "Mengelola Semua Fungsi Daya." },
  { "en": "Apa Itu Sensor IC?", "id": "Mengintegrasikan Sensor Dan Sirkuit Pemrosesan." },
  { "en": "Contoh Sensor IC?", "id": "Akselerometer, Giroskop, Sensor Gambar." },
  { "en": "Apa Itu Reliability?", "id": "Kemampuan Chip Beroperasi Andal Seiring Waktu." },
  { "en": "Apa Itu Electromigration?", "id": "Perpindahan Atom Logam Akibat Arus Tinggi." },
  { "en": "Apa Itu Hot Carrier Injection?", "id": "Mekanisme Degradasi Transistor." },
  { "en": "Apa Itu Soft Error?", "id": "Error Sementara Akibat Partikel Kosmik." },
  { "en": "Apa Itu Teori Rangkaian?", "id": "Fondasi Matematis Untuk Analisis Sirkuit." },
  { "en": "Apa Itu Teori Medan Elektromagnetik?", "id": "Fondasi Untuk Memahami Gelombang Dan Antena." },
  { "en": "Apa Itu Analisis Sinyal?", "id": "Memahami Dan Memproses Sinyal." },
  { "en": "Apa Itu Sistem Kontrol?", "id": "Mengatur Perilaku Sistem Lain." },
  { "en": "Apa Itu Sistem Digital?", "id": "Sistem Berbasis Logika Biner." },
  { "en": "Apa Itu Sistem Analog?", "id": "Sistem Berbasis Sinyal Kontinu." },
  { "en": "Apa Itu Sistem Tenaga?", "id": "Berurusan Dengan Pembangkitan Dan Distribusi Daya." },
  { "en": "Arus Kuat vs Arus Lemah?", "id": "Arus Kuat Untuk Daya, Arus Lemah Informasi." },
  { "en": "Apa Itu Rekayasa Komputer?", "id": "Desain Perangkat Keras Dan Lunak Komputer." },
  { "en": "Apa Itu Rekayasa Telekomunikasi?", "id": "Desain Sistem Komunikasi." },
  { "en": "Apa Itu Rekayasa Elektronika?", "id": "Desain Sirkuit Dan Perangkat Elektronik." },
  { "en": "Apa Itu Standar Teknik?", "id": "Dokumen Yang Menetapkan Spesifikasi Teknis." },
  { "en": "Apa Itu IEEE?", "id": "Institute of Electrical and Electronics Engineers." },
  { "en": "Apa Itu IEC?", "id": "International Electrotechnical Commission." },
  { "en": "Apa Itu Keselamatan Listrik?", "id": "Praktik Untuk Mencegah Kecelakaan Listrik." },
  { "en": "Bahaya Utama Listrik?", "id": "Sengatan Listrik, Kebakaran, Busur Api." },
  { "en": "Apa Itu Grounding?", "id": "Menyediakan Jalur Aman Untuk Arus Bocor." },
  { "en": "Apa Itu Isolasi?", "id": "Mencegah Kontak Dengan Bagian Bertegangan." },
  { "en": "Apa Itu Etika Profesi?", "id": "Prinsip Moral Bagi Para Insinyur." },
  { "en": "Apa Itu Pembelajaran Seumur Hidup?", "id": "Penting Mengingat Teknologi Cepat Berubah." },
  { "en": "Apa Itu Simulasi?", "id": "Menggunakan Model Komputer Untuk Menganalisis Sistem." },
  { "en": "Apa Itu Prototyping?", "id": "Membuat Versi Awal Dari Suatu Desain." },
  { "en": "Apa Itu Troubleshooting?", "id": "Proses Mencari Dan Memperbaiki Masalah." },
  { "en": "Langkah Dasar Troubleshooting?", "id": "Observasi, Analisis, Pengujian, Perbaikan." },
  { "en": "Apa Itu Dokumentasi Teknik?", "id": "Data Sheet, Skema, Manual." },
  { "en": "Pentingnya Dokumentasi?", "id": "Untuk Pemahaman, Perawatan, Dan Perbaikan." },
  { "en": "Apa Itu Skematik?", "id": "Diagram Simbolik Dari Suatu Rangkaian." },
  { "en": "Apa Itu Tata Letak PCB?", "id": "Desain Fisik Papan Sirkuit Cetak." },
  { "en": "Apa Itu Bill of Materials (BOM)?", "id": "Daftar Semua Komponen Yang Dibutuhkan." },
  { "en": "Apa Itu Project Management?", "id": "Merencanakan, Melaksanakan, Dan Menyelesaikan Proyek." },
  { "en": "Apa Itu Kerja Tim?", "id": "Sangat Penting Dalam Proyek Rekayasa Kompleks." },
  { "en": "Apa Itu Komunikasi Teknis?", "id": "Menyampaikan Informasi Teknis Secara Jelas." },
  { "en": "Apa Itu Inovasi?", "id": "Menciptakan Solusi Baru Yang Bermanfaat." },
  { "en": "Apa Itu Riset Dan Pengembangan (R&D)?", "id": "Aktivitas Untuk Menghasilkan Inovasi." },
  { "en": "Apa Itu Paten?", "id": "Hak Eksklusif Atas Suatu Penemuan." },
  { "en": "Apa Itu Hak Cipta?", "id": "Melindungi Karya Orisinal Seperti Perangkat Lunak." },
  { "en": "Apa Itu Kewirausahaan Teknologi?", "id": "Membangun Bisnis Berbasis Teknologi." },
  { "en": "Apa Itu Gelombang Mikro?", "id": "Gelombang Elektromagnetik Frekuensi Sangat Tinggi." },
  { "en": "Aplikasi Gelombang Mikro?", "id": "Radar, Komunikasi Satelit, Oven." },
  { "en": "Apa Itu Antena?", "id": "Transduser Antara Sirkuit Dan Ruang Bebas." },
  { "en": "Apa Itu Gain Antena?", "id": "Ukuran Kemampuan Antena Memfokuskan Energi." },
  { "en": "Apa Itu Polarisasi Antena?", "id": "Orientasi Medan Listrik Yang Dipancarkan." },
  { "en": "Apa Itu Pola Radiasi?", "id": "Grafik Kekuatan Sinyal Antena." },
  { "en": "Apa Itu Mekatronika?", "id": "Integrasi Mekanika, Elektronika, Dan Komputer." },
  { "en": "Apa Itu Robotika?", "id": "Desain Dan Aplikasi Robot." },
  { "en": "Apa Itu Sistem Tertanam?", "id": "Komputer Khusus Di Dalam Produk." },
  { "en": "Apa Itu Pemrosesan Citra?", "id": "Menganalisis Dan Memanipulasi Gambar Digital." },
  { "en": "Apa Itu Visi Komputer?", "id": "Memberi Komputer Kemampuan Untuk 'Melihat'." },
  { "en": "Apa Itu Kecerdasan Buatan (AI)?", "id": "Mesin Yang Menunjukkan Kecerdasan." },
  { "en": "Apa Itu Machine Learning?", "id": "Sistem Belajar Dari Data Tanpa Diprogram." },
  { "en": "Apa Itu Jaringan Saraf?", "id": "Model AI Yang Terinspirasi Otak." },
  { "en": "Apa Itu Sistem Fuzzy?", "id": "Sistem Logika Yang Menangani Ketidakpastian." },
  { "en": "Apa Itu Instrumentasi Virtual?", "id": "Menggunakan PC Untuk Meniru Instrumen." },
  { "en": "Apa Itu LabVIEW?", "id": "Bahasa Pemrograman Grafis Untuk Instrumentasi." },
  { "en": "Apa Itu MATLAB?", "id": "Lingkungan Komputasi Numerik." },
  { "en": "Apa Itu Simulink?", "id": "Alat Simulasi Grafis Di MATLAB." },
  { "en": "Apa Itu CAD (Computer-Aided Design)?", "id": "Desain Berbantuan Komputer." },
  { "en": "Contoh Perangkat Lunak CAD?", "id": "AutoCAD, SolidWorks, Eagle." },
  { "en": "Apa Itu Fotonik?", "id": "Teknologi Berbasis Foton (Cahaya)." },
  { "en": "Apa Itu Optoelektronika?", "id": "Studi Perangkat Yang Mengubah Cahaya Listrik." },
  { "en": "Apa Itu Kuantum?", "id": "Unit Energi Atau Materi Terkecil." },
  { "en": "Apa Itu Fisika Modern?", "id": "Fisika Relativitas Dan Kuantum." },
  { "en": "Apa Itu Nanoteknologi?", "id": "Rekayasa Pada Skala Nanometer." },
  { "en": "Apa Itu Bahan Cerdas?", "id": "Material Yang Sifatnya Dapat Diubah." },
  { "en": "Apa Itu Biomimetik?", "id": "Meniru Desain Dan Proses Dari Alam." },
  { "en": "Apa Itu Rekayasa Biomedis?", "id": "Penerapan Teknik Dalam Bidang Medis." },
  { "en": "Contoh Rekayasa Biomedis?", "id": "Pencitraan Medis (MRI), Alat Pacu Jantung." },
  { "en": "Apa Itu Termodinamika?", "id": "Studi Tentang Panas Dan Energi." },
  { "en": "Pentingnya Termodinamika?", "id": "Manajemen Panas Pada Perangkat Elektronik." },
  { "en": "Apa Itu Mekanika Fluida?", "id": "Studi Tentang Perilaku Cairan Dan Gas." },
  { "en": "Apa Itu Kompatibilitas Elektromagnetik (EMC)?", "id": "Kemampuan Perangkat Bekerja Tanpa Interferensi." },
  { "en": "Apa Itu Interferensi Elektromagnetik (EMI)?", "id": "Gangguan Elektromagnetik Yang Tidak Diinginkan." },
  { "en": "Apa Itu Decibel (dB)?", "id": "Satuan Logaritmik Untuk Rasio." },
  { "en": "Apa Itu Diagram Smith?", "id": "Alat Grafis Untuk Analisis Frekuensi Tinggi." },
  { "en": "Apa Itu Parameter S?", "id": "Mendeskripsikan Perilaku Jaringan Frekuensi Tinggi." },
  { "en": "Apa Itu Gelombang Berdiri?", "id": "Interferensi Gelombang Datang Dan Pantul." },
  { "en": "Apa Itu VSWR (Voltage Standing Wave Ratio)?", "id": "Ukuran Ketidakcocokan Impedansi." },
  { "en": "Apa Itu Pencocokan Impedansi?", "id": "Membuat Impedansi Beban Sama Dengan Sumber." },
  { "en": "Tujuan Pencocokan Impedansi?", "id": "Untuk Transfer Daya Maksimal." },
  { "en": "Apa Itu Sistem Linear?", "id": "Output Sebanding Dengan Input." },
  { "en": "Apa Itu Sistem Non-Linear?", "id": "Output Tidak Sebanding Dengan Input." },
  { "en": "Apa Itu Distorsi Harmonik?", "id": "Munculnya Frekuensi Kelipatan Akibat Non-Linearitas." },
  { "en": "Apa Itu Intermodulasi?", "id": "Pencampuran Sinyal Akibat Non-Linearitas." },
  { "en": "Apa Itu Titik Kompresi 1-dB?", "id": "Ukuran Linearitas Penguat." },
  { "en": "Apa Itu Angka Bising (Noise Figure)?", "id": "Ukuran Noise Yang Ditambahkan Perangkat." },
  { "en": "Apa Itu Suhu Bising?", "id": "Representasi Noise Dalam Satuan Suhu." },
  { "en": "Apa Itu Rumus Friis?", "id": "Menghitung Angka Bising Total Sistem Kaskade." },
  { "en": "Apa Itu Analisis Fourier?", "id": "Menguraikan Sinyal Menjadi Komponen Frekuensinya." },
  { "en": "Apa Itu Transformasi Laplace?", "id": "Alat Matematis Untuk Analisis Rangkaian." },
  { "en": "Apa Itu Transformasi Z?", "id": "Alat Matematis Untuk Sistem Diskrit." },
  { "en": "Apa Itu Probabilitas?", "id": "Studi Tentang Ketidakpastian." },
  { "en": "Pentingnya Probabilitas?", "id": "Analisis Noise, Teori Informasi." },
  { "en": "Apa Itu Variabel Acak?", "id": "Variabel Yang Nilainya Adalah Hasil Acak." },
  { "en": "Apa Itu Proses Stokastik?", "id": "Sinyal Acak Yang Berevolusi Seiring Waktu." },
  { "en": "Apa Itu Kalkulus?", "id": "Matematika Tentang Perubahan." },
  { "en": "Apa Itu Persamaan Diferensial?", "id": "Mendeskripsikan Perilaku Dinamis Sistem." },
  { "en": "Apa Itu Aljabar Linear?", "id": "Studi Vektor, Ruang Vektor, Matriks." },
  { "en": "Pentingnya Aljabar Linear?", "id": "Analisis Rangkaian, Teori Kontrol." }


        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
