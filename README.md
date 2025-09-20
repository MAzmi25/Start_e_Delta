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


  { "en": "Apa Itu Hubungan Bintang?", "id": "Koneksi Tiga Fase Dengan Titik Netral." },
  { "en": "Apa Itu Hubungan Delta?", "id": "Koneksi Tiga Fase Tanpa Titik Netral." },
  { "en": "Apa Nama Lain Hubungan Bintang?", "id": "Hubungan Wye Atau Y." },
  { "en": "Apa Nama Lain Hubungan Delta?", "id": "Hubungan Segitiga Atau Mesh." },
  { "en": "Berapa Kawat Pada Hubungan Bintang?", "id": "Empat Kawat Termasuk Netral." },
  { "en": "Berapa Kawat Pada Hubungan Delta?", "id": "Tiga Kawat Tanpa Netral." },
  { "en": "Apa Fungsi Titik Netral?", "id": "Sebagai Referensi Tegangan Nol." },
  { "en": "Bagaimana Arus Fasa Dan Arus Saluran Pada Bintang?", "id": "Arus Fasa Sama Dengan Arus Saluran." },
  { "en": "Bagaimana Tegangan Fasa Dan Tegangan Saluran Pada Bintang?", "id": "Tegangan Saluran Akar Tiga Tegangan Fasa." },
  { "en": "Bagaimana Arus Fasa Dan Arus Saluran Pada Delta?", "id": "Arus Saluran Akar Tiga Arus Fasa." },
  { "en": "Bagaimana Tegangan Fasa Dan Tegangan Saluran Pada Delta?", "id": "Tegangan Fasa Sama Dengan Tegangan Saluran." },
  { "en": "Di Mana Hubungan Bintang Biasa Digunakan?", "id": "Pada Sistem Distribusi Tenaga Listrik." },
  { "en": "Di Mana Hubungan Delta Biasa Digunakan?", "id": "Pada Sistem Transmisi Tenaga Listrik." },
  { "en": "Apa Keuntungan Hubungan Bintang?", "id": "Menyediakan Tegangan Fasa Dan Netral." },
  { "en": "Apa Kerugian Hubungan Bintang?", "id": "Membutuhkan Konduktor Netral Tambahan." },
  { "en": "Apa Keuntungan Hubungan Delta?", "id": "Lebih Andal Jika Satu Fasa Terbuka." },
  { "en": "Apa Kerugian Hubungan Delta?", "id": "Tidak Memiliki Titik Netral." },
  { "en": "Bagaimana Simbol Hubungan Bintang?", "id": "Seperti Huruf Y." },
  { "en": "Bagaimana Simbol Hubungan Delta?", "id": "Seperti Simbol Segitiga." },
  { "en": "Apa Yang Dimaksud Arus Fasa?", "id": "Arus Yang Mengalir Melalui Beban Fasa." },
  { "en": "Apa Yang Dimaksud Arus Saluran?", "id": "Arus Yang Mengalir Melalui Saluran Transmisi." },
  { "en": "Apa Yang Dimaksud Tegangan Fasa?", "id": "Tegangan Antara Fasa Dan Netral." },
  { "en": "Apa Yang Dimaksud Tegangan Saluran?", "id": "Tegangan Antara Dua Fasa." },
  { "en": "Berapa Sudut Fasa Antara Tegangan Fasa?", "id": "Seratus Dua Puluh Derajat." },
  { "en": "Berapa Sudut Fasa Antara Tegangan Saluran?", "id": "Seratus Dua Puluh Derajat." },
  { "en": "Apakah Hubungan Bintang Cocok Untuk Beban Tidak Seimbang?", "id": "Ya Sangat Cocok Sekali." },
  { "en": "Apakah Hubungan Delta Cocok Untuk Beban Tidak Seimbang?", "id": "Tidak Terlalu Cocok." },
  { "en": "Bagaimana Daya Tiga Fasa Dihitung?", "id": "Tiga Kali Daya Satu Fasa." },
  { "en": "Apa Rumus Daya Pada Hubungan Bintang?", "id": "Akar Tiga Kali Vl Kali Il Cos Teta." },
  { "en": "Apa Rumus Daya Pada Hubungan Delta?", "id": "Akar Tiga Kali Vl Kali Il Cos Teta." },
  { "en": "Apa Satuan Daya Aktif?", "id": "Watt." },
  { "en": "Apa Satuan Daya Reaktif?", "id": "Var." },
  { "en": "Apa Satuan Daya Semu?", "id": "Va." },
  { "en": "Apa Itu Faktor Daya?", "id": "Rasio Daya Aktif Dan Daya Semu." },
  { "en": "Bagaimana Cara Memperbaiki Faktor Daya?", "id": "Menggunakan Kapasitor Bank." },
  { "en": "Apa Akibat Faktor Daya Rendah?", "id": "Menyebabkan Kerugian Daya Yang Besar." },
  { "en": "Apa Manfaat Faktor Daya Tinggi?", "id": "Meningkatkan Efisiensi Sistem Tenaga Listrik." },
  { "en": "Apa Itu Beban Seimbang?", "id": "Beban Dengan Impedansi Sama Setiap Fasa." },
  { "en": "Apa Itu Beban Tidak Seimbang?", "id": "Beban Dengan Impedansi Berbeda Setiap Fasa." },
  { "en": "Apa Peran Transformator?", "id": "Mengubah Tingkat Tegangan Listrik." },
  { "en": "Bagaimana Transformator Tiga Fasa Dihubungkan?", "id": "Bintang-Bintang Bintang-Delta Delta-Bintang Delta-Delta." },
  { "en": "Apa Keuntungan Transformator Hubungan Bintang-Bintang?", "id": "Memiliki Titik Netral Pada Kedua Sisi." },
  { "en": "Apa Kerugian Transformator Hubungan Bintang-Bintang?", "id": "Rentan Terhadap Harmonisa Ketiga." },
  { "en": "Apa Keuntungan Transformator Hubungan Bintang-Delta?", "id": "Menghilangkan Harmonisa Ketiga." },
  { "en": "Apa Kerugian Transformator Hubungan Bintang-Delta?", "id": "Pergeseran Fasa Tiga Puluh Derajat." },
  { "en": "Apa Keuntungan Transformator Hubungan Delta-Bintang?", "id": "Cocok Untuk Distribusi Tegangan Rendah." },
  { "en": "Apa Kerugian Transformator Hubungan Delta-Bintang?", "id": "Pergeseran Fasa Tiga Puluh Derajat." },
  { "en": "Apa Keuntungan Transformator Hubungan Delta-Delta?", "id": "Dapat Beroperasi Dengan Satu Fasa Terbuka." },
  { "en": "Apa Kerugian Transformator Hubungan Delta-Delta?", "id": "Tidak Memiliki Titik Netral." },
  { "en": "Apa Itu Harmonisa?", "id": "Frekuensi Kelipatan Dari Frekuensi Fundamental." },
  { "en": "Bagaimana Harmonisa Mempengaruhi Sistem Tenaga Listrik?", "id": "Menyebabkan Pemanasan Berlebih Dan Distorsi." },
  { "en": "Apa Itu Urutan Fasa?", "id": "Urutan Tegangan Mencapai Nilai Puncak." },
  { "en": "Apa Saja Urutan Fasa Yang Umum?", "id": "Abc Dan Acb." },
  { "en": "Mengapa Urutan Fasa Penting?", "id": "Mempengaruhi Arah Putaran Motor Tiga Fasa." },
  { "en": "Bagaimana Cara Menentukan Urutan Fasa?", "id": "Menggunakan Phase Sequence Indicator." },
  { "en": "Apa Itu Motor Induksi Tiga Fasa?", "id": "Motor Listrik Yang Dijalankan Dengan Listrik Tiga Fasa." },
  { "en": "Bagaimana Hubungan Stator Motor Induksi Tiga Fasa?", "id": "Bisa Dihubungkan Bintang Atau Delta." },
  { "en": "Kapan Stator Motor Dihubungkan Bintang?", "id": "Saat Starting Untuk Mengurangi Arus." },
  { "en": "Kapan Stator Motor Dihubungkan Delta?", "id": "Saat Berjalan Normal Untuk Daya Penuh." },
  { "en": "Apa Itu Starting Bintang-Delta?", "id": "Metode Starting Motor Untuk Mengurangi Arus." },
  { "en": "Apa Keuntungan Starting Bintang-Delta?", "id": "Mengurangi Arus Starting Secara Signifikan." },
  { "en": "Apa Kerugian Starting Bintang-Delta?", "id": "Torsi Starting Juga Berkurang." },
  { "en": "Apa Itu Generator Tiga Fasa?", "id": "Mesin Yang Membangkitkan Listrik Tiga Fasa." },
  { "en": "Bagaimana Belitan Generator Tiga Fasa Dihubungkan?", "id": "Biasanya Dihubungkan Bintang." },
  { "en": "Mengapa Belitan Generator Dihubungkan Bintang?", "id": "Untuk Mendapatkan Titik Netral." },
  { "en": "Apa Fungsi Sistem Proteksi?", "id": "Melindungi Peralatan Dari Gangguan Listrik." },
  { "en": "Bagaimana Sistem Proteksi Bekerja?", "id": "Mendeteksi Gangguan Dan Memutuskan Rangkaian." },
  { "en": "Apa Itu Gangguan Hubung Singkat?", "id": "Koneksi Arus Listrik Dengan Resistansi Rendah." },
  { "en": "Apa Akibat Hubung Singkat?", "id": "Arus Sangat Besar Dan Berbahaya." },
  { "en": "Apa Itu Gangguan Tanah?", "id": "Hubungan Fasa Ke Tanah." },
  { "en": "Bagaimana Mendeteksi Gangguan Tanah?", "id": "Menggunakan Relai Arus Sisa." },
  { "en": "Apa Itu Sistem Pentanahan?", "id": "Menghubungkan Peralatan Listrik Ke Tanah." },
  { "en": "Apa Tujuan Sistem Pentanahan?", "id": "Untuk Keselamatan Dan Stabilitas Sistem." },
  { "en": "Jenis Sistem Pentanahan Apa Yang Ada?", "id": "Tt It Tn-S Tn-C Tn-C-S." },
  { "en": "Apa Perbedaan Antara Sistem Tt Dan Tn?", "id": "Cara Menghubungkan Titik Netral Ke Tanah." },
  { "en": "Apa Itu Tegangan Sentuh?", "id": "Tegangan Antara Bodi Peralatan Dan Tanah." },
  { "en": "Apa Itu Tegangan Langkah?", "id": "Tegangan Antara Dua Kaki Di Atas Tanah." },
  { "en": "Bagaimana Mengurangi Risiko Tegangan Sentuh Dan Langkah?", "id": "Dengan Sistem Pentanahan Yang Baik." },
  { "en": "Apa Itu Isolasi Listrik?", "id": "Bahan Yang Tidak Menghantarkan Listrik." },
  { "en": "Apa Fungsi Isolasi Listrik?", "id": "Mencegah Kontak Langsung Dengan Konduktor." },
  { "en": "Apa Itu Kelas Isolasi?", "id": "Tingkat Ketahanan Isolasi Terhadap Suhu." },
  { "en": "Bagaimana Mengukur Tahanan Isolasi?", "id": "Menggunakan Insulation Tester Atau Megger." },
  { "en": "Apa Standar Warna Kabel Fasa?", "id": "Merah Kuning Hitam." },
  { "en": "Apa Standar Warna Kabel Netral?", "id": "Biru." },
  { "en": "Apa Standar Warna Kabel Tanah?", "id": "Hijau-Kuning." },
  { "en": "Apa Itu Kapasitansi Saluran Transmisi?", "id": "Kemampuan Saluran Menyimpan Muatan Listrik." },
  { "en": "Apa Itu Induktansi Saluran Transmisi?", "id": "Sifat Saluran Melawan Perubahan Arus." },
  { "en": "Apa Itu Impedansi Saluran Transmisi?", "id": "Hambatan Total Saluran Terhadap Arus Bolak-Balik." },
  { "en": "Bagaimana Impedansi Dihitung?", "id": "Kombinasi Resistansi Dan Reaktansi." },
  { "en": "Apa Itu Reaktansi Induktif?", "id": "Hambatan Akibat Induktansi." },
  { "en": "Apa Itu Reaktansi Kapasitif?", "id": "Hambatan Akibat Kapasitansi." },
  { "en": "Apa Itu Efek Kulit?", "id": "Arus Cenderung Mengalir Di Permukaan Konduktor." },
  { "en": "Apa Itu Efek Jarak?", "id": "Medan Magnet Dari Satu Konduktor Mempengaruhi Yang Lain." },
  { "en": "Apa Itu Korona?", "id": "Pelepasan Listrik Di Sekitar Konduktor Tegangan Tinggi." },
  { "en": "Bagaimana Mengurangi Korona?", "id": "Menggunakan Konduktor Dengan Diameter Lebih Besar." },
  { "en": "Apa Tujuan Konversi Bintang-Delta?", "id": "Untuk Menyederhanakan Analisis Rangkaian Listrik." },
  { "en": "Apa Rumus Konversi Delta Ke Bintang?", "id": "Produk Resistor Berdekatan Dibagi Total Resistor." },
  { "en": "Apa Rumus Konversi Bintang Ke Delta?", "id": "Jumlah Produk Berpasangan Dibagi Resistor Berlawanan." },
  { "en": "Apakah Konversi Ini Berlaku Untuk Impedansi?", "id": "Ya Berlaku Untuk Resistor Dan Impedansi." },
  { "en": "Apa Itu Metode Dua Wattmeter?", "id": "Mengukur Daya Total Dengan Dua Wattmeter." },
  { "en": "Di Mana Kumparan Arus Wattmeter Dihubungkan?", "id": "Secara Seri Dengan Dua Saluran." },
  { "en": "Di Mana Kumparan Tegangan Wattmeter Dihubungkan?", "id": "Antara Saluran Dan Saluran Ketiga." },
  { "en": "Bagaimana Daya Total Dihitung Dengan Metode Ini?", "id": "Jumlah Pembacaan Kedua Wattmeter (W1 + W2)." },
  { "en": "Apa Yang Ditunjukkan Jika W1 Sama Dengan W2?", "id": "Faktor Daya Adalah Satu (Beban Resistif)." },
  { "en": "Apa Yang Ditunjukkan Jika W1 Atau W2 Nol?", "id": "Faktor Daya Adalah Setengah (0.5)." },
  { "en": "Apa Yang Ditunjukkan Jika W1 Sama Dengan Negatif W2?", "id": "Faktor Daya Adalah Nol (Beban Reaktif Murni)." },
  { "en": "Apa Itu Metode Satu Wattmeter?", "id": "Mengukur Daya Pada Beban Seimbang." },
  { "en": "Apa Syarat Menggunakan Metode Satu Wattmeter?", "id": "Beban Harus Benar-Benar Seimbang Sempurna." },
  { "en": "Apa Yang Terjadi Jika Satu Fasa Terbuka Di Delta?", "id": "Sistem Beroperasi Sebagai Open-Delta (V-V)." },
  { "en": "Berapa Kapasitas Daya Konfigurasi Open-Delta?", "id": "Sekitar 57.7% Dari Kapasitas Penuh." },
  { "en": "Apa Yang Terjadi Jika Netral Terbuka Di Bintang?", "id": "Tegangan Fasa Menjadi Tidak Seimbang." },
  { "en": "Apa Itu Arus Sirkulasi?", "id": "Arus Yang Mengalir Dalam Loop Delta Tertutup." },
  { "en": "Apa Penyebab Arus Sirkulasi Di Delta?", "id": "Ketidakseimbangan Tegangan Atau Harmonisa Ketiga." },
  { "en": "Mengapa Hubungan Delta Menekan Harmonisa Ketiga?", "id": "Karena Harmonisa Ketiga Terperangkap Dalam Loop." },
  { "en": "Apakah Hubungan Bintang Memiliki Arus Sirkulasi?", "id": "Tidak Karena Tidak Ada Jalur Tertutup." },
  { "en": "Tegangan Apa Yang Lebih Tinggi Pada Hubungan Bintang?", "id": "Tegangan Saluran Lebih Tinggi Dari Fasa." },
  { "en": "Arus Apa Yang Lebih Tinggi Pada Hubungan Delta?", "id": "Arus Saluran Lebih Tinggi Dari Fasa." },
  { "en": "Mengapa Isolasi Lebih Rendah Dibutuhkan Pada Bintang?", "id": "Karena Tegangan Fasa Lebih Rendah." },
  { "en": "Mengapa Ukuran Konduktor Lebih Besar Dibutuhkan Pada Delta?", "id": "Karena Arus Fasa Lebih Rendah." },
  { "en": "Apa Itu Pergeseran Titik Bintang?", "id": "Titik Netral Bergeser Akibat Beban Tak Seimbang." },
  { "en": "Apa Dampak Pergeseran Titik Bintang?", "id": "Menyebabkan Tegangan Fasa Tidak Sama." },
  { "en": "Bagaimana Hubungan Delta Menangani Beban Tunggal?", "id": "Beban Dapat Dihubungkan Antara Dua Fasa." },
  { "en": "Bagaimana Hubungan Bintang Menangani Beban Tunggal?", "id": "Beban Dihubungkan Antara Fasa Dan Netral." },
  { "en": "Manakah Yang Lebih Baik Untuk Transmisi Jarak Jauh?", "id": "Hubungan Delta Untuk Tegangan Tinggi." },
  { "en": "Manakah Yang Lebih Aman Untuk Pengguna Akhir?", "id": "Hubungan Bintang Dengan Pentanahan Netral." },
  { "en": "Apa Itu Sistem Tiga Fasa Empat Kawat?", "id": "Sistem Bintang Dengan Kawat Netral." },
  { "en": "Apa Itu Sistem Tiga Fasa Tiga Kawat?", "id": "Sistem Delta Atau Bintang Tanpa Netral." },
  { "en": "Apa Hubungan Antara Daya Kompleks Fasa Dan Total?", "id": "Daya Total Adalah Tiga Kali Daya Fasa." },
  { "en": "Bagaimana Menghitung Impedansi Ekuivalen Bintang?", "id": "Impedansi Delta Dibagi Tiga." },
  { "en": "Bagaimana Menghitung Impedansi Ekuivalen Delta?", "id": "Tiga Kali Impedansi Bintang." },
  { "en": "Kapan Konversi Impedansi Ini Berlaku?", "id": "Hanya Untuk Beban Seimbang Sempurna." },
  { "en": "Apa Fungsi Utama Starter Bintang-Delta?", "id": "Membatasi Arus Lonjakan Saat Motor Start." },
  { "en": "Berapa Torsi Awal Pada Koneksi Bintang?", "id": "Sepertiga Dari Torsi Pada Koneksi Delta." },
  { "en": "Berapa Arus Awal Pada Koneksi Bintang?", "id": "Sepertiga Dari Arus Pada Koneksi Delta." },
  { "en": "Manakah Yang Memberikan Tegangan Ganda?", "id": "Hubungan Bintang (Fasa-Netral Dan Fasa-Fasa)." },
  { "en": "Apakah Hubungan Delta Memberikan Tegangan Ganda?", "id": "Tidak Hanya Satu Tingkat Tegangan." },
  { "en": "Manakah Yang Lebih Umum Untuk Pemanas Industri?", "id": "Hubungan Delta Untuk Daya Lebih Tinggi." },
  { "en": "Manakah Yang Lebih Umum Untuk Penerangan Komersial?", "id": "Hubungan Bintang Untuk Fleksibilitas Tegangan." },
  { "en": "Apa Itu Tegangan Urutan Nol?", "id": "Komponen Tegangan Yang Sama Di Setiap Fasa." },
  { "en": "Apakah Tegangan Urutan Nol Ada Di Delta?", "id": "Tidak Ada Jalur Untuk Arus Urutan Nol." },
  { "en": "Di Mana Arus Urutan Nol Mengalir?", "id": "Mengalir Melalui Netral Pada Hubungan Bintang." },
  { "en": "Apa Itu Komponen Simetris?", "id": "Urutan Positif Negatif Dan Nol." },
  { "en": "Untuk Apa Komponen Simetris Digunakan?", "id": "Untuk Menganalisis Sistem Tidak Seimbang." },
  { "en": "Apa Urutan Positif Itu?", "id": "Satu Set Fasor Tiga Fasa Seimbang." },
  { "en": "Apa Urutan Negatif Itu?", "id": "Memiliki Urutan Fasa Yang Berlawanan." },
  { "en": "Apa Urutan Nol Itu?", "id": "Tiga Fasor Dengan Magnitudo Dan Fasa Sama." },
  { "en": "Manakah Yang Membutuhkan Lebih Sedikit Putaran Belitan?", "id": "Hubungan Bintang Untuk Tegangan Fasa Rendah." },
  { "en": "Manakah Yang Rentan Terhadap Kerusakan Isolasi?", "id": "Hubungan Delta Karena Tegangan Tinggi." },
  { "en": "Apa Itu Sistem Open-Wye Open-Delta?", "id": "Transformator Untuk Melayani Beban Tiga Fasa." },
  { "en": "Bagaimana Arus Netral Pada Beban Seimbang Bintang?", "id": "Arus Netral Adalah Nol." },
  { "en": "Kapan Arus Mengalir Di Kawat Netral?", "id": "Saat Beban Tiga Fasa Tidak Seimbang." },
  { "en": "Apakah Mungkin Membuat Hubungan Delta Dari Sumber Bintang?", "id": "Ya Menggunakan Transformator Delta-Bintang." },
  { "en": "Apakah Mungkin Membuat Hubungan Bintang Dari Sumber Delta?", "id": "Ya Menggunakan Transformator Bintang-Delta." },
  { "en": "Apa Itu Grounded Bintang?", "id": "Titik Netral Dihubungkan Ke Tanah." },
  { "en": "Apa Keuntungan Grounded Bintang?", "id": "Membatasi Tegangan Lebih Saat Terjadi Gangguan." },
  { "en": "Apa Itu Ungrounded Delta?", "id": "Tidak Ada Koneksi Langsung Ke Tanah." },
  { "en": "Apa Keuntungan Ungrounded Delta?", "id": "Dapat Terus Beroperasi Saat Gangguan Fasa-Tanah." },
  { "en": "Apa Itu Arus Fasa Pada Beban Bintang?", "id": "Tegangan Fasa Dibagi Impedansi Fasa." },
  { "en": "Apa Itu Arus Fasa Pada Beban Delta?", "id": "Tegangan Saluran Dibagi Impedansi Fasa." },
  { "en": "Bagaimana Hubungan Daya Dan Impedansi?", "id": "Daya Berbanding Terbalik Dengan Impedansi." },
  { "en": "Manakah Yang Menghasilkan Medan Magnet Berputar?", "id": "Keduanya Menghasilkan Medan Magnet Berputar." },
  { "en": "Bagaimana Arah Medan Magnet Ditentukan?", "id": "Oleh Urutan Fasa Sistem." },
  { "en": "Apa Itu Efisiensi Transmisi?", "id": "Rasio Daya Diterima Dan Daya Dikirim." },
  { "en": "Bagaimana Bintang-Delta Meningkatkan Efisiensi?", "id": "Mengurangi Kerugian Tembaga Saat Starting." },
  { "en": "Manakah Yang Lebih Stabil Terhadap Gangguan?", "id": "Sistem Bintang Yang Ditanahkan (Grounded)." },
  { "en": "Apa Itu Ketidakseimbangan Tegangan?", "id": "Magnitudo Tegangan Antar Fasa Tidak Sama." },
  { "en": "Apa Dampak Ketidakseimbangan Tegangan Pada Motor?", "id": "Menyebabkan Pemanasan Berlebih Dan Getaran." },
  { "en": "Manakah Koneksi Yang Lebih Mudah Diparalelkan?", "id": "Koneksi Delta Lebih Mudah Diparalelkan." },
  { "en": "Apa Itu Relai Diferensial?", "id": "Alat Proteksi Untuk Transformator." },
  { "en": "Bagaimana Relai Diferensial Bekerja?", "id": "Membandingkan Arus Masuk Dan Keluar." },
  { "en": "Manakah Yang Membutuhkan Proteksi Lebih Kompleks?", "id": "Hubungan Delta Karena Sifatnya Yang Ungrounded." },
  { "en": "Apa Itu Resistansi Pentanahan?", "id": "Resistansi Antara Elektroda Tanah Dan Bumi." },
  { "en": "Berapa Nilai Ideal Resistansi Pentanahan?", "id": "Nilainya Harus Sekecil Mungkin." },
  { "en": "Apa Itu Tegangan Fasa-Ke-Fasa?", "id": "Sama Dengan Tegangan Saluran." },
  { "en": "Apa Itu Tegangan Fasa-Ke-Netral?", "id": "Sama Dengan Tegangan Fasa." },
  { "en": "Manakah Yang Digunakan Pada Sistem HVDC?", "id": "Tidak Keduanya HVDC Adalah Arus Searah." },
  { "en": "Apakah Bintang Dan Delta Relevan Untuk Arus Searah?", "id": "Tidak Hanya Relevan Untuk Arus Bolak-Balik." },
  { "en": "Apa Itu Jaringan Listrik?", "id": "Sistem Interkoneksi Pembangkit Transmisi Distribusi." },
  { "en": "Bagaimana Bintang Dan Delta Berperan Dalam Jaringan?", "id": "Sebagai Konfigurasi Utama Di Setiap Tahap." },
  { "en": "Apa Itu Fasor?", "id": "Representasi Grafis Bilangan Kompleks." },
  { "en": "Bagaimana Fasor Digunakan Dalam Analisis Tiga Fasa?", "id": "Untuk Menggambarkan Magnitudo Dan Fasa Tegangan." },
  { "en": "Manakah Yang Memiliki Keandalan Operasional Lebih Tinggi?", "id": "Hubungan Delta Karena Sifat Redundansinya." },
  { "en": "Apa Itu Bank Transformator?", "id": "Tiga Transformator Fasa Tunggal Dihubungkan Bersama." },
  { "en": "Apa Keuntungan Bank Transformator Dibanding Unit Tiga Fasa?", "id": "Lebih Mudah Mengganti Unit Yang Rusak." },
  { "en": "Manakah Yang Memiliki Biaya Awal Lebih Rendah?", "id": "Hubungan Bintang Karena Isolasi Lebih Rendah." },
  { "en": "Manakah Yang Memiliki Biaya Perawatan Lebih Tinggi?", "id": "Hubungan Delta Jika Terjadi Kerusakan." },
  { "en": "Apa Yang Dimaksud Dengan Sudut Daya?", "id": "Sudut Antara Tegangan Dan Arus." },
  { "en": "Bagaimana Sudut Daya Mempengaruhi Pembacaan Wattmeter?", "id": "Mempengaruhi Magnitudo Dan Tanda Pembacaan." },
  { "en": "Manakah Yang Lebih Baik Untuk Beban Berfluktuasi?", "id": "Hubungan Delta Lebih Stabil." },
  { "en": "Apa Itu Sistem Kelistrikan Polifasa?", "id": "Sistem Dengan Lebih Dari Satu Fasa." },
  { "en": "Mengapa Sistem Tiga Fasa Paling Umum?", "id": "Karena Paling Efisien Dan Ekonomis." },
  { "en": "Apa Peran Nikola Tesla Dalam Sistem Tiga Fasa?", "id": "Dia Adalah Penemu Sistem Tiga Fasa." },
  { "en": "Apa Itu Koneksi Bintang Interkoneksi?", "id": "Juga Dikenal Sebagai Transformator Zigzag." },
  { "en": "Apa Tujuan Utama Transformator Zigzag?", "id": "Untuk Menyediakan Jalur Pentanahan Pada Delta." },
  { "en": "Bagaimana Belitan Pada Trafo Zigzag Dihubungkan?", "id": "Setiap Fasa Dibagi Menjadi Dua Bagian." },
  { "en": "Apakah Trafo Zigzag Mahal?", "id": "Ya Lebih Mahal Dari Trafo Konvensional." },
  { "en": "Apa Itu Sistem High-Leg Delta?", "id": "Sistem Delta Dengan Center-Tap Ditanahkan." },
  { "en": "Berapa Tegangan Fasa-Ke-Tanah Pada High-Leg?", "id": "Lebih Tinggi Dari Dua Fasa Lainnya." },
  { "en": "Bagaimana Mengidentifikasi Kabel High-Leg?", "id": "Biasanya Ditandai Dengan Warna Oranye." },
  { "en": "Di Mana Sistem High-Leg Delta Digunakan?", "id": "Pada Bangunan Komersial Lama Di Amerika." },
  { "en": "Apa Risiko Sistem High-Leg Delta?", "id": "Kesalahan Koneksi Dapat Merusak Peralatan." },
  { "en": "Manakah Yang Menghasilkan Torsi Awal Lebih Halus?", "id": "Starter Bintang-Delta Mengurangi Sentakan Awal." },
  { "en": "Bagaimana Ammeter Dihubungkan Dalam Sirkuit Tiga Fasa?", "id": "Selalu Dihubungkan Secara Seri." },
  { "en": "Bagaimana Voltmeter Dihubungkan Untuk Mengukur Tegangan Fasa?", "id": "Antara Fasa Dan Titik Netral." },
  { "en": "Bagaimana Voltmeter Dihubungkan Untuk Mengukur Tegangan Saluran?", "id": "Antara Dua Saluran Fasa Yang Berbeda." },
  { "en": "Apa Itu Power Factor Meter?", "id": "Alat Untuk Mengukur Faktor Daya Listrik." },
  { "en": "Manakah Yang Lebih Rentan Terhadap Ferroresonance?", "id": "Sistem Bintang Yang Tidak Ditanahkan." },
  { "en": "Apa Itu Ferroresonance?", "id": "Osilasi Tegangan Tinggi Berbahaya." },
  { "en": "Manakah Yang Umum Digunakan Untuk Pembangkit Listrik Tenaga Angin?", "id": "Generator Biasanya Terhubung Bintang." },
  { "en": "Manakah Yang Umum Digunakan Untuk Tungku Busur Listrik?", "id": "Biasanya Menggunakan Koneksi Delta." },
  { "en": "Apa Itu Arus Gangguan?", "id": "Arus Yang Mengalir Saat Terjadi Hubung Singkat." },
  { "en": "Manakah Yang Memiliki Arus Gangguan Fasa-Ke-Tanah Lebih Besar?", "id": "Sistem Bintang Yang Ditanahkan Solid." },
  { "en": "Manakah Yang Memiliki Arus Gangguan Fasa-Ke-Fasa Lebih Besar?", "id": "Besarnya Hampir Sama Untuk Keduanya." },
  { "en": "Apa Itu Sistem Pentanahan Impedansi Tinggi?", "id": "Netral Ditanahkan Melalui Impedansi Tinggi." },
  { "en": "Apa Tujuan Pentanahan Impedansi Tinggi?", "id": "Membatasi Arus Gangguan Tanah." },
  { "en": "Koneksi Apa Yang Cocok Untuk Pentanahan Impedansi Tinggi?", "id": "Hanya Cocok Untuk Sistem Bintang." },
  { "en": "Apa Itu Busur Api (Arc Flash)?", "id": "Ledakan Energi Listrik Berbahaya." },
  { "en": "Bagaimana Koneksi Bintang-Delta Mempengaruhi Busur Api?", "id": "Dapat Mengurangi Energi Busur Api." },
  { "en": "Bagaimana Cara Menghubungkan Beban Fasa Tunggal Ke Sistem Delta?", "id": "Di Antara Dua Fasa Mana Saja." },
  { "en": "Bagaimana Tegangan Beban Tersebut?", "id": "Sama Dengan Tegangan Saluran Sistem." },
  { "en": "Bagaimana Cara Menghubungkan Beban Fasa Tunggal Ke Sistem Bintang?", "id": "Antara Satu Fasa Dan Netral." },
  { "en": "Bagaimana Tegangan Beban Tersebut Di Sistem Bintang?", "id": "Sama Dengan Tegangan Fasa Sistem." },
  { "en": "Apa Istilah Lain Untuk Open-Delta?", "id": "Koneksi V-V." },
  { "en": "Kapan Koneksi Open-Delta Digunakan?", "id": "Sebagai Solusi Darurat Atau Sementara." },
  { "en": "Apa Itu Faktor Pemanfaatan Transformator?", "id": "Rasio Daya Aktual Dan Kapasitas Terpasang." },
  { "en": "Berapa Faktor Pemanfaatan Pada Konfigurasi Open-Delta?", "id": "Sekitar Delapan Puluh Enam Koma Enam Persen." },
  { "en": "Manakah Yang Lebih Sederhana Dalam Konstruksi Fisik?", "id": "Koneksi Delta Memiliki Koneksi Yang Lebih Sedikit." },
  { "en": "Manakah Yang Membutuhkan Ruang Fisik Lebih Besar?", "id": "Tergantung Pada Desain Transformator." },
  { "en": "Apa Itu Diagram Fasor?", "id": "Representasi Vektor Dari Kuantitas AC." },
  { "en": "Bagaimana Fasor Tegangan Saluran Dan Fasa Di Bintang?", "id": "Tegangan Saluran Mendahului Fasa 30 Derajat." },
  { "en": "Bagaimana Fasor Arus Saluran Dan Fasa Di Delta?", "id": "Arus Saluran Tertinggal Fasa 30 Derajat." },
  { "en": "Apakah Koneksi Bintang Selalu Memiliki Netral?", "id": "Tidak Netral Bisa Dibiarkan Mengambang." },
  { "en": "Apa Konsekuensi Netral Mengambang?", "id": "Tegangan Bisa Menjadi Sangat Tidak Stabil." },
  { "en": "Manakah Yang Lebih Baik Untuk Starting Motor Beban Berat?", "id": "Metode Starting Selain Bintang-Delta." },
  { "en": "Manakah Yang Lebih Tahan Terhadap Petir?", "id": "Sistem Bintang Yang Ditanahkan Dengan Baik." },
  { "en": "Apa Itu Gelombang Berjalan (Traveling Wave)?", "id": "Lonjakan Tegangan Akibat Sambaran Petir." },
  { "en": "Bagaimana Koneksi Delta Mempengaruhi Gelombang Berjalan?", "id": "Bisa Menyebabkan Tegangan Lebih Tinggi." },
  { "en": "Apa Itu Isolator?", "id": "Komponen Untuk Mengisolasi Konduktor Dari Penopang." },
  { "en": "Apakah Kebutuhan Isolator Berbeda?", "id": "Ya Berdasarkan Tingkat Tegangan Sistem." },
  { "en": "Apa Hubungan Antara Daya Dan Torsi Motor?", "id": "Daya Sebanding Dengan Torsi Kali Kecepatan." },
  { "en": "Bagaimana Koneksi Delta Meningkatkan Torsi?", "id": "Karena Tegangan Penuh Diterapkan Pada Belitan." },
  { "en": "Manakah Yang Lebih Efisien Dalam Distribusi Daya?", "id": "Sistem Tiga Fasa Lebih Efisien." },
  { "en": "Mengapa Sistem Tiga Fasa Lebih Efisien?", "id": "Membutuhkan Lebih Sedikit Tembaga Untuk Daya Sama." },
  { "en": "Apa Itu Jatuh Tegangan (Voltage Drop)?", "id": "Penurunan Tegangan Di Sepanjang Saluran." },
  { "en": "Faktor Apa Yang Mempengaruhi Jatuh Tegangan?", "id": "Panjang Arus Dan Impedansi Saluran." },
  { "en": "Bagaimana Mengurangi Jatuh Tegangan?", "id": "Menaikkan Tegangan Atau Ukuran Konduktor." },
  { "en": "Manakah Yang Memberikan Kualitas Daya Lebih Baik?", "id": "Sistem Seimbang Dengan Koneksi Yang Tepat." },
  { "en": "Apa Itu Kedip Tegangan (Voltage Sag)?", "id": "Penurunan Tegangan Jangka Pendek." },
  { "en": "Apa Itu Bengkak Tegangan (Voltage Swell)?", "id": "Kenaikan Tegangan Jangka Pendek." },
  { "en": "Bagaimana Sistem Pentanahan Mempengaruhi Kedip Tegangan?", "id": "Pentanahan Solid Dapat Memperburuk Kedip Tegangan." },
  { "en": "Manakah Yang Lebih Rentan Terhadap Kebisingan (Noise)?", "id": "Tergantung Pada Desain Dan Pentanahan Sistem." },
  { "en": "Apa Itu Slip Motor Induksi?", "id": "Perbedaan Antara Kecepatan Sinkron Dan Aktual." },
  { "en": "Apakah Koneksi Stator Mempengaruhi Slip?", "id": "Tidak Slip Terutama Tergantung Pada Beban." },
  { "en": "Apa Itu Kecepatan Sinkron?", "id": "Kecepatan Putaran Medan Magnet Stator." },
  { "en": "Bagaimana Kecepatan Sinkron Dihitung?", "id": "Seratus Dua Puluh Kali Frekuensi Dibagi Jumlah Kutub." },
  { "en": "Apa Itu Kutub (Pole) Motor?", "id": "Kutub Magnetik Yang Dihasilkan Oleh Belitan Stator." },
  { "en": "Apa Itu Ototransformator?", "id": "Transformator Dengan Hanya Satu Belitan." },
  { "en": "Apakah Ototransformator Bisa Dihubungkan Bintang Atau Delta?", "id": "Ya Bisa Dikonfigurasi Sebagai Keduanya." },
  { "en": "Apa Itu Rugi Tembaga (Copper Loss)?", "id": "Kerugian Daya Akibat Resistansi Belitan." },
  { "en": "Apa Itu Rugi Inti (Core Loss)?", "id": "Kerugian Daya Akibat Histeresis Dan Arus Eddy." },
  { "en": "Manakah Yang Memiliki Rugi Tembaga Lebih Rendah Saat Start?", "id": "Koneksi Bintang Karena Arus Lebih Rendah." },
  { "en": "Manakah Yang Digunakan Dalam Pengelasan Tiga Fasa?", "id": "Biasanya Menggunakan Sumber Daya Terhubung Delta." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Rangkaian Yang Mengubah AC Menjadi DC." },
  { "en": "Bagaimana Penyearah Tiga Fasa Dikonfigurasi?", "id": "Bisa Menggunakan Sumber Bintang Atau Delta." },
  { "en": "Manakah Yang Menghasilkan Riak DC Lebih Rendah?", "id": "Penyearah Tiga Fasa Dibanding Fasa Tunggal." },
  { "en": "Apa Itu Inverter?", "id": "Rangkaian Yang Mengubah DC Menjadi AC." },
  { "en": "Apakah Inverter Tiga Fasa Menghasilkan Koneksi Bintang Atau Delta?", "id": "Outputnya Dapat Disintesis Sebagai Keduanya." },
  { "en": "Apa Itu Variable Frequency Drive (VFD)?", "id": "Alat Untuk Mengontrol Kecepatan Motor AC." },
  { "en": "Bagaimana VFD Bekerja?", "id": "Mengubah Frekuensi Dan Tegangan Catu Daya." },
  { "en": "Manakah Koneksi Motor Yang Cocok Untuk VFD?", "id": "Koneksi Delta Biasanya Lebih Disukai." },
  { "en": "Apa Itu Soft Starter?", "id": "Alat Elektronik Untuk Mengurangi Arus Start." },
  { "en": "Apa Perbedaan Soft Starter Dan Starter Bintang-Delta?", "id": "Soft Starter Memberikan Akselerasi Lebih Halus." },
  { "en": "Manakah Yang Lebih Mahal Soft Starter Atau Bintang-Delta?", "id": "Soft Starter Umumnya Lebih Mahal." },
  { "en": "Apa Itu Sistem Tenaga Listrik Darurat?", "id": "Sumber Daya Cadangan Saat Listrik Utama Padam." },
  { "en": "Bagaimana Genset Darurat Biasanya Dikonfigurasi?", "id": "Keluaran Generator Biasanya Dihubungkan Bintang." },
  { "en": "Mengapa Genset Dihubungkan Bintang?", "id": "Untuk Menyediakan Netral Bagi Beban Fasa Tunggal." },
  { "en": "Apa Itu Saklar Transfer Otomatis (ATS)?", "id": "Saklar Yang Memindahkan Beban Ke Sumber Darurat." },
  { "en": "Manakah Yang Lebih Mudah Dikoordinasikan Proteksinya?", "id": "Sistem Bintang Yang Ditanahkan." },
  { "en": "Apa Itu Koordinasi Proteksi?", "id": "Memastikan Alat Proteksi Terdekat Gangguan Bekerja Dahulu." },
  { "en": "Apa Itu Relai Arus Lebih (Overcurrent Relay)?", "id": "Relai Yang Bekerja Saat Arus Melebihi Batas." },
  { "en": "Apa Itu Sekering (Fuse)?", "id": "Perangkat Proteksi Sekali Pakai." },
  { "en": "Apa Itu Pemutus Sirkuit (Circuit Breaker)?", "id": "Saklar Otomatis Yang Dapat Direset." },
  { "en": "Apakah Pemilihan Proteksi Tergantung Koneksi?", "id": "Ya Karakteristik Gangguan Mempengaruhi Pemilihan." },
  { "en": "Apa Itu Kualitas Daya (Power Quality)?", "id": "Ukuran Seberapa Baik Sistem Mendukung Beban." },
  { "en": "Manakah Yang Lebih Mungkin Menyebabkan Masalah Kualitas Daya?", "id": "Beban Tidak Seimbang Pada Sistem Bintang." },
  { "en": "Apa Itu Transien?", "id": "Lonjakan Atau Penurunan Tegangan Jangka Sangat Pendek." },
  { "en": "Bagaimana Transien Terjadi?", "id": "Akibat Switching Petir Atau Gangguan." },
  { "en": "Manakah Yang Lebih Baik Dalam Meredam Transien?", "id": "Karakteristiknya Berbeda Tergantung Jenis Transien." },
  { "en": "Apa Itu Belitan Tersier?", "id": "Belitan Ketiga Pada Transformator." },
  { "en": "Apa Bentuk Koneksi Belitan Tersier?", "id": "Hampir Selalu Dihubungkan Secara Delta." },
  { "en": "Apa Fungsi Utama Belitan Tersier?", "id": "Menekan Harmonisa Dan Menstabilkan Tegangan Netral." },
  { "en": "Apakah Belitan Tersier Selalu Dibebani?", "id": "Tidak Seringkali Hanya Untuk Sirkulasi Harmonisa." },
  { "en": "Bagaimana Harmonisa Kelima Berperilaku?", "id": "Seperti Komponen Urutan Negatif." },
  { "en": "Bagaimana Harmonisa Ketujuh Berperilaku?", "id": "Seperti Komponen Urutan Positif." },
  { "en": "Harmonisa Mana Yang Disebut Triplen?", "id": "Kelipatan Tiga (3 6 9 Dst)." },
  { "en": "Bagaimana Perilaku Harmonisa Triplen?", "id": "Seperti Komponen Urutan Nol." },
  { "en": "Manakah Koneksi Yang Menghilangkan Harmonisa Triplen Dari Saluran?", "id": "Koneksi Delta." },
  { "en": "Apa Yang Terjadi Pada Harmonisa Triplen Di Bintang?", "id": "Mengalir Di Kawat Netral Jika Ada." },
  { "en": "Apa Itu Distorsi Harmonik Total (THD)?", "id": "Ukuran Total Distorsi Harmonik." },
  { "en": "Manakah Yang Cenderung Memiliki THD Tegangan Lebih Rendah?", "id": "Sistem Dengan Koneksi Delta." },
  { "en": "Apa Itu Filter Harmonik?", "id": "Perangkat Untuk Mengurangi Atau Menghilangkan Harmonisa." },
  { "en": "Manakah Yang Membutuhkan Rating Arus Switchgear Lebih Tinggi?", "id": "Tergantung Pada Lokasi Dan Konfigurasi Sistem." },
  { "en": "Manakah Yang Membutuhkan Rating Tegangan Isolasi Busbar Lebih Tinggi?", "id": "Koneksi Delta Untuk Tegangan Yang Sama." },
  { "en": "Apa Itu Stabilitas Sudut Rotor?", "id": "Kemampuan Generator Sinkron Untuk Tetap Sinkron." },
  { "en": "Bagaimana Koneksi Transformator Mempengaruhi Stabilitas?", "id": "Mempengaruhi Impedansi Transfer Antar Generator." },
  { "en": "Manakah Yang Umumnya Memberikan Batas Stabilitas Lebih Tinggi?", "id": "Tergantung Pada Desain Sistem Secara Keseluruhan." },
  { "en": "Apa Itu Tap Changer?", "id": "Mekanisme Untuk Mengubah Rasio Belitan Trafo." },
  { "en": "Di Sisi Mana Tap Changer Biasanya Dipasang?", "id": "Biasanya Di Sisi Tegangan Tinggi." },
  { "en": "Apakah Operasi Tap Changer Mempengaruhi Koneksi?", "id": "Tidak Mengubah Jenis Koneksi Bintang Delta." },
  { "en": "Manakah Yang Lebih Ekonomis Dari Segi Biaya Isolasi?", "id": "Hubungan Bintang Karena Tegangan Fasa Rendah." },
  { "en": "Manakah Yang Lebih Ekonomis Dari Segi Biaya Konduktor?", "id": "Tergantung Pada Tegangan Dan Arus Desain." },
  { "en": "Apa Itu Biaya Siklus Hidup (Life Cycle Cost)?", "id": "Total Biaya Selama Umur Operasi Peralatan." },
  { "en": "Apa Itu Grup Vektor Transformator?", "id": "Menunjukkan Pergeseran Fasa Antara Belitan." },
  { "en": "Grup Vektor Apa Yang Umum Untuk Bintang-Bintang?", "id": "Yy0 (Nol Derajat Pergeseran Fasa)." },
  { "en": "Grup Vektor Apa Yang Umum Untuk Delta-Bintang?", "id": "Dy11 (Minus Tiga Puluh Derajat Pergeseran)." },
  { "en": "Mengapa Grup Vektor Penting?", "id": "Sangat Penting Untuk Operasi Paralel Transformator." },
  { "en": "Apa Syarat Paralel Transformator?", "id": "Tegangan Rasio Polaritas Dan Grup Vektor Sama." },
  { "en": "Apa Yang Terjadi Jika Grup Vektor Berbeda Diparalelkan?", "id": "Akan Terjadi Arus Sirkulasi Yang Besar." },
  { "en": "Berapa Pergeseran Fasa Pada Koneksi Zigzag?", "id": "Nol Derajat Sama Seperti Koneksi Bintang." },
  { "en": "Manakah Yang Lebih Efisien Untuk Beban Tidak Seimbang?", "id": "Hubungan Bintang Dengan Netral Yang Solid." },
  { "en": "Apa Itu Ketidakseimbangan Arus?", "id": "Magnitudo Arus Antar Fasa Tidak Sama." },
  { "en": "Apa Dampak Ketidakseimbangan Arus?", "id": "Menghasilkan Komponen Urutan Negatif." },
  { "en": "Komponen Urutan Negatif Berbahaya Untuk Apa?", "id": "Sangat Berbahaya Untuk Motor Dan Generator." },
  { "en": "Bagaimana Komponen Urutan Negatif Memanaskan Motor?", "id": "Menghasilkan Medan Magnet Berlawanan Arah." },
  { "en": "Manakah Yang Bisa Menyembunyikan Ketidakseimbangan Beban?", "id": "Koneksi Delta Dapat Menyembunyikan Dari Sumber." },
  { "en": "Bagaimana Menghitung Daya Reaktif Dengan Dua Wattmeter?", "id": "Akar Tiga Kali Selisih Pembacaan (W1-W2)." },
  { "en": "Bagaimana Menghitung Sudut Faktor Daya Dari Metode Ini?", "id": "Tan Invers Dari Formula Daya Reaktif." },
  { "en": "Manakah Yang Lebih Mudah Untuk Diuji Di Pabrik?", "id": "Tidak Ada Perbedaan Signifikan Dalam Pengujian." },
  { "en": "Apa Itu Uji Hubung Singkat Transformator?", "id": "Untuk Menentukan Impedansi Dan Rugi Tembaga." },
  { "en": "Apa Itu Uji Beban Nol Transformator?", "id": "Untuk Menentukan Rugi Inti Dan Arus Eksitasi." },
  { "en": "Apakah Prosedur Uji Berbeda Untuk Bintang Dan Delta?", "id": "Prinsipnya Sama Tapi Koneksi Pengukuran Berbeda." },
  { "en": "Manakah Yang Lebih Disukai Untuk Sistem Distribusi Perkotaan?", "id": "Sistem Bintang Empat Kawat." },
  { "en": "Mengapa Sistem Bintang Dipilih Untuk Perkotaan?", "id": "Melayani Beban Fasa Tiga Dan Fasa Tunggal." },
  { "en": "Manakah Yang Lebih Disukai Untuk Area Industri Berat?", "id": "Sistem Delta Untuk Beban Motor Besar." },
  { "en": "Apa Itu Pusat Beban (Load Center)?", "id": "Titik Di Mana Sebagian Besar Beban Terkonsentrasi." },
  { "en": "Apa Itu Gardu Induk (Substation)?", "id": "Fasilitas Untuk Mengubah Dan Mendistribusikan Listrik." },
  { "en": "Konfigurasi Apa Yang Umum Di Gardu Induk Distribusi?", "id": "Transformator Step-Down Delta-Bintang." },
  { "en": "Mengapa Delta-Bintang Digunakan Di Gardu Distribusi?", "id": "Menurunkan Tegangan Transmisi Dan Memberi Netral." },
  { "en": "Manakah Yang Memiliki Keamanan Personel Lebih Baik?", "id": "Sistem Bintang Yang Ditanahkan Dengan Benar." },
  { "en": "Bagaimana Pentanahan Netral Meningkatkan Keamanan?", "id": "Memicu Perangkat Proteksi Saat Terjadi Gangguan." },
  { "en": "Apa Itu Gangguan Fasa Ganda Ke Tanah?", "id": "Dua Fasa Terhubung Singkat Ke Tanah." },
  { "en": "Manakah Yang Lebih Sulit Dideteksi Gangguan Resistansi Tinggi?", "id": "Pada Sistem Delta Yang Tidak Ditanahkan." },
  { "en": "Apa Itu Relai Arah (Directional Relay)?", "id": "Relai Yang Bekerja Berdasarkan Arah Arus Gangguan." },
  { "en": "Di Mana Relai Arah Digunakan?", "id": "Pada Jaringan Kompleks Seperti Jaringan Cincin." },
  { "en": "Apa Itu Pembangkit Terdistribusi (Distributed Generation)?", "id": "Pembangkit Listrik Skala Kecil Dekat Beban." },
  { "en": "Bagaimana Pembangkit Terdistribusi Mempengaruhi Sistem?", "id": "Dapat Mempengaruhi Aliran Daya Dan Proteksi." },
  { "en": "Manakah Yang Lebih Mudah Diintegrasikan Dengan Pembangkit Terdistribusi?", "id": "Keduanya Menimbulkan Tantangan Desain Yang Berbeda." },
  { "en": "Apa Itu Pulau Listrik (Islanding)?", "id": "Pembangkit Terdistribusi Terus Memberi Daya Saat Jaringan Padam." },
  { "en": "Mengapa Islanding Berbahaya?", "id": "Berisiko Bagi Pekerja Dan Peralatan." },
  { "en": "Apa Itu Jaringan Cerdas (Smart Grid)?", "id": "Jaringan Listrik Dengan Kontrol Dan Komunikasi Digital." },
  { "en": "Apakah Konsep Bintang-Delta Masih Relevan Di Jaringan Cerdas?", "id": "Ya Konsep Fundamental Tetap Sama." },
  { "en": "Apa Itu FACTS (Flexible AC Transmission Systems)?", "id": "Perangkat Elektronika Daya Untuk Mengontrol Aliran." },
  { "en": "Bagaimana FACTS Berinteraksi Dengan Koneksi Bintang-Delta?", "id": "Membantu Mengontrol Stabilitas Dan Kualitas Daya." },
  { "en": "Manakah Yang Lebih Tahan Terhadap Pergeseran Fasa Tegangan?", "id": "Hubungan Delta Sedikit Lebih Kaku (Stiff)." },
  { "en": "Apa Itu Kekakuan (Stiffness) Sistem Tenaga?", "id": "Kemampuan Sistem Menahan Perubahan Tegangan." },
  { "en": "Apa Itu Jatuh Daya (Power Swing)?", "id": "Osilasi Daya Antara Dua Area Jaringan." },
  { "en": "Manakah Yang Lebih Baik Dalam Meredam Jatuh Daya?", "id": "Tergantung Pada Impedansi Sistem." },
  { "en": "Apa Itu Analisis Aliran Daya (Power Flow Analysis)?", "id": "Studi Untuk Menentukan Kondisi Operasi Jaringan." },
  { "en": "Bagaimana Beban Dimodelkan Dalam Analisis Aliran Daya?", "id": "Bisa Sebagai Bintang Atau Delta." },
  { "en": "Apa Itu Bus Slack/Swing?", "id": "Bus Referensi Dalam Analisis Aliran Daya." },
  { "en": "Apa Itu Bus Beban (PQ Bus)?", "id": "Bus Di Mana Daya Aktif Dan Reaktif Diketahui." },
  { "en": "Apa Itu Bus Tegangan Terkendali (PV Bus)?", "id": "Bus Di Mana Daya Aktif Dan Tegangan Diketahui." },
  { "en": "Manakah Yang Menghasilkan Torsi Pulsasi Lebih Rendah?", "id": "Keduanya Menghasilkan Torsi Konstan." },
  { "en": "Mengapa Daya Tiga Fasa Konstan?", "id": "Jumlah Daya Sesaat Setiap Fasa Selalu Konstan." },
  { "en": "Apa Keuntungan Daya Konstan?", "id": "Operasi Motor Dan Generator Lebih Halus." },
  { "en": "Bagaimana Menghitung Arus Netral Pada Beban Tak Seimbang?", "id": "Jumlah Vektor Dari Ketiga Arus Fasa." },
  { "en": "Berapa Besar Maksimum Arus Netral?", "id": "Bisa Sebesar Arus Fasa Tertinggi." },
  { "en": "Mengapa Netral Tidak Boleh Disatukan Dengan Ground?", "id": "Untuk Mencegah Arus Kembali Melalui Jalur Yang Salah." },
  { "en": "Apa Itu NEC (National Electrical Code)?", "id": "Standar Keamanan Instalasi Listrik Di AS." },
  { "en": "Apa Itu IEC (International Electrotechnical Commission)?", "id": "Organisasi Standar Elektroteknik Internasional." },
  { "en": "Apakah Standar Untuk Bintang-Delta Berbeda?", "id": "Ya Ada Perbedaan Antara Standar NEC Dan IEC." },
  { "en": "Manakah Yang Lebih Umum Di Eropa?", "id": "Sistem Bintang Empat Kawat Sangat Umum." },
  { "en": "Manakah Yang Lebih Umum Di Amerika Utara?", "id": "Kombinasi Sistem Bintang Dan Delta Digunakan." },
  { "en": "Apa Itu Sudut Torsi (Torque Angle)?", "id": "Sudut Antara Medan Magnet Rotor Dan Stator." },
  { "en": "Bagaimana Sudut Torsi Berhubungan Dengan Beban?", "id": "Sudut Torsi Meningkat Seiring Dengan Peningkatan Beban." },
  { "en": "Apa Yang Terjadi Jika Sudut Torsi Terlalu Besar?", "id": "Generator Atau Motor Kehilangan Sinkronisasi." },
  { "en": "Manakah Yang Lebih Tahan Terhadap Beban Berlebih Jangka Pendek?", "id": "Hubungan Delta Pada Motor." },
  { "en": "Apa Itu Kapasitas Termal?", "id": "Kemampuan Peralatan Menahan Panas." },
  { "en": "Bagaimana Arus Berlebih Mempengaruhi Kapasitas Termal?", "id": "Pemanasan Berlebih Dapat Merusak Isolasi." },
  { "en": "Manakah Yang Lebih Mudah Dipelihara?", "id": "Keduanya Membutuhkan Program Pemeliharaan Rutin." },
  { "en": "Apa Saja Kegiatan Pemeliharaan Transformator?", "id": "Pengujian Minyak Pengecekan Suhu Dan Pembersihan." },
  { "en": "Manakah Yang Memiliki Umur Pakai Lebih Lama?", "id": "Tidak Ada Perbedaan Umur Pakai Yang Melekat." },
  { "en": "Apa Faktor Utama Yang Mempengaruhi Umur Peralatan Listrik?", "id": "Suhu Operasi Dan Kualitas Pemeliharaan." },
  { "en": "Apa Itu Arus Inrush Transformator?", "id": "Lonjakan Arus Saat Transformator Pertama Kali Dinyalakan." },
  { "en": "Faktor Apa Yang Mempengaruhi Besarnya Arus Inrush?", "id": "Fluks Sisa Dan Sudut Tegangan." },
  { "en": "Bagaimana Koneksi Bintang-Delta Mempengaruhi Arus Inrush?", "id": "Karakteristiknya Berbeda Tetapi Keduanya Mengalaminya." },
  { "en": "Bagaimana Mengurangi Arus Inrush?", "id": "Menggunakan Soft Starter Atau Resistor Pre-Insertion." },
  { "en": "Apa Itu Pentanahan Resonansi?", "id": "Pentanahan Netral Bintang Melalui Reaktor." },
  { "en": "Apa Nama Lain Pentanahan Resonansi?", "id": "Pentanahan Kumparan Petersen (Petersen Coil)." },
  { "en": "Apa Tujuan Pentanahan Resonansi?", "id": "Mengurangi Arus Gangguan Tanah Hingga Nol." },
  { "en": "Apa Kerugian Pentanahan Resonansi?", "id": "Sulit Mendeteksi Gangguan Resistansi Tinggi." },
  { "en": "Apa Itu Pentanahan Solid?", "id": "Menghubungkan Netral Bintang Langsung Ke Tanah." },
  { "en": "Apa Keuntungan Pentanahan Solid?", "id": "Mudah Mendeteksi Gangguan Dan Stabil." },
  { "en": "Apa Kerugian Pentanahan Solid?", "id": "Menghasilkan Arus Gangguan Tanah Sangat Tinggi." },
  { "en": "Bagaimana Energi Diukur Pada Sistem Bintang Empat Kawat?", "id": "Menggunakan Meter Energi Tiga Elemen." },
  { "en": "Bagaimana Energi Diukur Pada Sistem Delta Tiga Kawat?", "id": "Menggunakan Meter Energi Dua Elemen." },
  { "en": "Apa Itu Teorema Blondel?", "id": "Prinsip Dasar Pengukuran Daya Polifasa." },
  { "en": "Berapa Jumlah Elemen Wattmeter Yang Dibutuhkan?", "id": "Sama Dengan Jumlah Kawat Dikurangi Satu." },
  { "en": "Apa Itu Transformator Instrumen?", "id": "CT Dan PT Untuk Pengukuran Dan Proteksi." },
  { "en": "Apa Itu Transformator Arus (CT)?", "id": "Menurunkan Arus Untuk Diukur Oleh Meter." },
  { "en": "Apa Itu Transformator Potensial (PT)?", "id": "Menurunkan Tegangan Untuk Diukur Oleh Meter." },
  { "en": "Bagaimana CT Dihubungkan Untuk Proteksi Diferensial Trafo Delta-Bintang?", "id": "CT Sisi Delta Dihubungkan Bintang." },
  { "en": "Mengapa Koneksi CT Harus Dibalik?", "id": "Untuk Mengkompensasi Pergeseran Fasa 30 Derajat." },
  { "en": "Apa Itu Saturasi CT?", "id": "Ketidakmampuan CT Mereproduksi Arus Tinggi." },
  { "en": "Apa Akibat Saturasi CT?", "id": "Dapat Menyebabkan Kegagalan Operasi Relai." },
  { "en": "Manakah Yang Lebih Rentan Terhadap Tegangan Lebih Transien?", "id": "Sistem Delta Yang Tidak Ditanahkan." },
  { "en": "Apa Itu Tegangan Lebih Switching?", "id": "Tegangan Tinggi Akibat Operasi Pemutusan Rangkaian." },
  { "en": "Apa Itu Arester Surja (Surge Arrester)?", "id": "Perangkat Pelindung Terhadap Tegangan Lebih." },
  { "en": "Di Mana Arester Surja Dipasang?", "id": "Secara Paralel Antara Fasa Dan Tanah." },
  { "en": "Apakah Kebutuhan Arester Berbeda Untuk Bintang Dan Delta?", "id": "Ya Rating Tegangan Disesuaikan." },
  { "en": "Apa Itu Penyearah Enam Pulsa?", "id": "Penyearah Tiga Fasa Gelombang Penuh Dasar." },
  { "en": "Apa Itu Penyearah Dua Belas Pulsa?", "id": "Menggunakan Dua Jembatan Enam Pulsa." },
  { "en": "Bagaimana Penyearah Dua Belas Pulsa Dibuat?", "id": "Menggunakan Trafo Bintang-Bintang Dan Bintang-Delta." },
  { "en": "Apa Keuntungan Penyearah Dua Belas Pulsa?", "id": "Mengurangi Distorsi Harmonik Arus Secara Signifikan." },
  { "en": "Harmonisa Mana Yang Dihilangkan Oleh Penyearah 12 Pulsa?", "id": "Harmonisa Ke-5 Dan Ke-7." },
  { "en": "Manakah Yang Lebih Disukai Untuk Aplikasi VFD Besar?", "id": "Input Dengan Penyearah 12 Atau 18 Pulsa." },
  { "en": "Apa Itu Common Mode Voltage Pada VFD?", "id": "Tegangan Rata-Rata Output VFD Terhadap Tanah." },
  { "en": "Bagaimana Common Mode Voltage Mempengaruhi Motor?", "id": "Dapat Merusak Bantalan (Bearing) Motor." },
  { "en": "Apakah Koneksi Motor Mempengaruhi Common Mode Voltage?", "id": "Tidak Terutama Disebabkan Oleh Switching VFD." },
  { "en": "Manakah Yang Lebih Kaku Terhadap Variasi Beban?", "id": "Sistem Delta Cenderung Sedikit Lebih Kaku." },
  { "en": "Apa Itu Regulasi Tegangan?", "id": "Perubahan Tegangan Dari Beban Nol Ke Beban Penuh." },
  { "en": "Manakah Yang Umumnya Memiliki Regulasi Tegangan Lebih Baik?", "id": "Tergantung Pada Impedansi Sumber Dan Saluran." },
  { "en": "Apa Itu Bank Kapasitor?", "id": "Kumpulan Kapasitor Untuk Memperbaiki Faktor Daya." },
  { "en": "Bagaimana Bank Kapasitor Tiga Fasa Dihubungkan?", "id": "Bisa Dihubungkan Bintang Atau Delta." },
  { "en": "Apa Keuntungan Bank Kapasitor Koneksi Delta?", "id": "Tegangan Lebih Rendah Kapasitansi Lebih Tinggi." },
  { "en": "Apa Keuntungan Bank Kapasitor Koneksi Bintang?", "id": "Memiliki Netral Untuk Proteksi Ketidakseimbangan." },
  { "en": "Apa Itu Fenomena Resonansi Paralel?", "id": "Resonansi Antara Kapasitor Dan Induktansi Sistem." },
  { "en": "Kapan Resonansi Paralel Bisa Terjadi?", "id": "Saat Bank Kapasitor Dihubungkan Ke Sistem." },
  { "en": "Apa Akibat Resonansi Paralel?", "id": "Amplifikasi Tegangan Dan Arus Harmonik." },
  { "en": "Manakah Yang Lebih Kompleks Untuk Dimodelkan Secara Matematis?", "id": "Sistem Tidak Seimbang Dan Sistem Ditanahkan." },
  { "en": "Apa Itu Matriks Impedansi Bus (Zbus)?", "id": "Metode Untuk Menganalisis Sistem Tenaga." },
  { "en": "Apa Itu Matriks Admitansi Bus (Ybus)?", "id": "Metode Yang Lebih Umum Digunakan." },
  { "en": "Apakah Model Bintang-Delta Penting Dalam Perhitungan Ini?", "id": "Ya Sangat Penting Untuk Membangun Matriks." },
  { "en": "Manakah Yang Lebih Tahan Terhadap Gangguan Asimetris?", "id": "Sistem Bintang Yang Ditanahkan Solid." },
  { "en": "Apa Contoh Gangguan Asimetris?", "id": "Gangguan Satu Fasa Ke Tanah." },
  { "en": "Apa Contoh Gangguan Simetris?", "id": "Gangguan Tiga Fasa." },
  { "en": "Manakah Yang Menyebabkan Gangguan Paling Parah?", "id": "Gangguan Tiga Fasa Biasanya Paling Parah." },
  { "en": "Apa Itu Kemampuan Breaking Pemutus Sirkuit?", "id": "Arus Gangguan Maksimum Yang Dapat Diputuskan." },
  { "en": "Bagaimana Arus Gangguan Dihitung?", "id": "Berdasarkan Tegangan Dan Impedansi Thevenin." },
  { "en": "Manakah Yang Digunakan Pada Sistem Kelistrikan Pesawat?", "id": "Sistem Tiga Fasa 400 Hz." },
  { "en": "Bagaimana Generator Pesawat Biasanya Dikonfigurasi?", "id": "Biasanya Dikonfigurasi Bintang." },
  { "en": "Manakah Yang Digunakan Pada Sistem Kelistrikan Kapal Laut?", "id": "Menggunakan Sistem Tiga Fasa 60 Hz." },
  { "en": "Bagaimana Sistem Distribusi Kapal Dikonfigurasi?", "id": "Sering Menggunakan Sistem Delta Tidak Ditanahkan." },
  { "en": "Mengapa Sistem Delta Dipilih Untuk Kapal?", "id": "Untuk Keandalan Operasional Yang Lebih Tinggi." },
  { "en": "Apa Itu Sistem Tenaga Listrik Satu Fasa Tiga Kawat?", "id": "Umum Digunakan Untuk Distribusi Residensial." },
  { "en": "Bagaimana Hubungannya Dengan Sistem Tiga Fasa?", "id": "Berasal Dari Satu Fasa Transformator Distribusi." },
  { "en": "Apa Itu Faktor Keragaman (Diversity Factor)?", "id": "Rasio Jumlah Permintaan Puncak Dan Puncak Sistem." },
  { "en": "Apa Itu Faktor Beban (Load Factor)?", "id": "Rasio Beban Rata-Rata Dan Beban Puncak." },
  { "en": "Manakah Yang Menawarkan Penghematan Energi Lebih Besar?", "id": "Bukan Koneksinya Tetapi Efisiensi Peralatan." },
  { "en": "Apa Itu Standar IEEE 519?", "id": "Standar Batasan Distorsi Harmonik." },
  { "en": "Bagaimana Cara Memenuhi Standar IEEE 519?", "id": "Menggunakan Filter Atau Penyearah Multi-Pulsa." },
  { "en": "Manakah Yang Lebih Mudah Menyebabkan Interferensi Elektromagnetik (EMI)?", "id": "Keduanya Dapat Menjadi Sumber EMI." },
  { "en": "Apa Itu EMC (Electromagnetic Compatibility)?", "id": "Kemampuan Peralatan Berfungsi Tanpa Menyebabkan EMI." },
  { "en": "Apa Itu Pemeliharaan Prediktif?", "id": "Memprediksi Kapan Kerusakan Akan Terjadi." },
  { "en": "Contoh Pemeliharaan Prediktif Untuk Transformator?", "id": "Analisis Gas Terlarut (Dissolved Gas Analysis)." },
  { "en": "Manakah Yang Lebih Sulit Diselesaikan Masalahnya (Troubleshoot)?", "id": "Sistem Delta Tidak Ditanahkan." },
  { "en": "Mengapa Sistem Delta Sulit Di-Troubleshoot?", "id": "Lokasi Gangguan Tanah Sulit Ditemukan." },
  { "en": "Apa Itu Sistem SCADA?", "id": "Sistem Kontrol Pengawasan Dan Akuisisi Data." },
  { "en": "Bagaimana SCADA Memantau Sistem Bintang-Delta?", "id": "Melalui Sensor Tegangan Arus Dan Suhu." },
  { "en": "Manakah Yang Lebih Cepat Usang (Obsolete)?", "id": "Bukan Koneksinya Tapi Teknologi Peralatannya." },
  { "en": "Apa Itu Cadangan Berputar (Spinning Reserve)?", "id": "Kapasitas Pembangkit Ekstra Yang Sinkron." },
  { "en": "Manakah Yang Lebih Cepat Merespon Perubahan Beban?", "id": "Tergantung Pada Kontrol Generator Bukan Koneksi." },
  { "en": "Apa Itu Black Start?", "id": "Memulihkan Jaringan Tanpa Daya Eksternal." },
  { "en": "Peran Apa Yang Dimainkan Koneksi Dalam Black Start?", "id": "Koneksi Peralatan Harus Dikelola Dengan Hati-Hati." },
  { "en": "Apa Itu Rugi-Rugi Dielektrik?", "id": "Kerugian Energi Pada Material Isolasi." },
  { "en": "Manakah Yang Memiliki Rugi-Rugi Dielektrik Lebih Tinggi?", "id": "Sistem Dengan Tegangan Operasi Lebih Tinggi." },
  { "en": "Apa Itu Uji Tangen Delta?", "id": "Mengukur Kualitas Isolasi Peralatan Listrik." },
  { "en": "Manakah Yang Membutuhkan Ruang Gardu Lebih Besar?", "id": "Tergantung Pada Tegangan Dan Konfigurasi Fisik." },
  { "en": "Apa Itu Jarak Bebas (Clearance) Listrik?", "id": "Jarak Minimum Antar Komponen Bertegangan." },
  { "en": "Manakah Yang Membutuhkan Jarak Bebas Lebih Besar?", "id": "Sistem Dengan Tingkat Tegangan Lebih Tinggi." },
  { "en": "Apa Itu Kabel Berisolasi Polimer?", "id": "Kabel Modern Yang Menggunakan Isolasi Sintetis." },
  { "en": "Apa Itu Kabel Berisolasi Kertas Minyak?", "id": "Teknologi Kabel Yang Lebih Tua." },
  { "en": "Manakah Yang Lebih Tahan Terhadap Kelembaban?", "id": "Kabel Dengan Isolasi Polimer Modern." },
  { "en": "Apa Itu Umur Isolasi?", "id": "Durasi Waktu Isolasi Dapat Berfungsi Efektif." },
  { "en": "Bagaimana Suhu Mempengaruhi Umur Isolasi?", "id": "Suhu Tinggi Mempercepat Penuaan Isolasi." },
  { "en": "Manakah Yang Lebih Baik Untuk Lingkungan Panas?", "id": "Peralatan Dengan Kelas Isolasi Suhu Tinggi." },
  { "en": "Apa Itu Sistem Pendingin Transformator?", "id": "Sistem Untuk Menghilangkan Panas Dari Transformator." },
  { "en": "Contoh Sistem Pendingin?", "id": "ONAN ONAF OFAF." },
  { "en": "Apakah Sistem Pendingin Bergantung Pada Koneksi?", "id": "Tidak Bergantung Pada Total Kerugian Daya." },
  { "en": "Apa Arti Terminal U1 V1 W1 Pada Motor?", "id": "Awal Mula Dari Tiga Belitan Fasa." },
  { "en": "Apa Arti Terminal U2 V2 W2 Pada Motor?", "id": "Ujung Akhir Dari Tiga Belitan Fasa." },
  { "en": "Bagaimana Cara Fisik Menghubungkan Motor Dalam Bintang?", "id": "Satukan Terminal U2 V2 W2." },
  { "en": "Bagaimana Cara Fisik Menghubungkan Motor Dalam Delta?", "id": "Hubungkan U2 Ke V1 V2 Ke W1 W2 Ke U1." },
  { "en": "Apa Fungsi Plat Penghubung (Link) Pada Terminal Motor?", "id": "Untuk Mengkonfigurasi Sambungan Bintang Atau Delta." },
  { "en": "Berapa Jumlah Plat Penghubung Untuk Sambungan Bintang?", "id": "Satu Plat Menghubungkan Tiga Terminal." },
  { "en": "Berapa Jumlah Plat Penghubung Untuk Sambungan Delta?", "id": "Tiga Plat Menghubungkan Terminal Secara Berpasangan." },
  { "en": "Apa Itu Motor Tegangan Ganda (Dual Voltage)?", "id": "Motor Yang Dapat Beroperasi Pada Dua Tegangan." },
  { "en": "Bagaimana Koneksi Motor Untuk Tegangan Lebih Rendah?", "id": "Biasanya Dihubungkan Secara Delta." },
  { "en": "Bagaimana Koneksi Motor Untuk Tegangan Lebih Tinggi?", "id": "Biasanya Dihubungkan Secara Bintang." },
  { "en": "Apa Itu Faktor Ketidakseimbangan Tegangan (VUF)?", "id": "Ukuran Persentase Ketidakseimbangan Tegangan." },
  { "en": "Berapa Batas VUF Yang Direkomendasikan Untuk Motor?", "id": "Idealnya Di Bawah Satu Persen." },
  { "en": "Apa Dampak VUF Tinggi Pada Motor?", "id": "Pemanasan Berlebih Dan Penurunan Umur Pakai." },
  { "en": "Apa Itu Faktor Ketidakseimbangan Arus (CUF)?", "id": "Ukuran Persentase Ketidakseimbangan Arus." },
  { "en": "Bagaimana Hubungan Antara VUF Dan CUF?", "id": "CUF Biasanya Enam Hingga Sepuluh Kali VUF." },
  { "en": "Manakah Yang Lebih Sensitif Terhadap Ketidakseimbangan?", "id": "Motor Induksi Sangat Sensitif." },
  { "en": "Bagaimana Koneksi Delta Menangani Arus Tidak Seimbang?", "id": "Arus Sirkulasi Dapat Terjadi Dalam Belitan." },
  { "en": "Apa Itu Relai Urutan Negatif?", "id": "Relai Yang Melindungi Dari Ketidakseimbangan Fasa." },
  { "en": "Mengapa Sulit Memverifikasi Sirkuit Mati Pada Sistem Delta?", "id": "Karena Tidak Ada Referensi Tegangan Nol (Tanah)." },
  { "en": "Apa Itu Tegangan Hantu (Phantom Voltage)?", "id": "Tegangan Terukur Pada Sirkuit Terbuka." },
  { "en": "Manakah Yang Lebih Rentan Menunjukkan Tegangan Hantu?", "id": "Sistem Delta Yang Tidak Ditanahkan." },
  { "en": "Apa Itu Backfeed?", "id": "Aliran Energi Listrik Ke Arah Berlawanan." },
  { "en": "Bagaimana Koneksi Delta Dapat Menyebabkan Backfeed Berbahaya?", "id": "Melalui Transformator Bahkan Saat Satu Sisi Terbuka." },
  { "en": "Manakah Yang Lebih Mudah Dikoordinasikan Dengan Proteksi Cadangan?", "id": "Sistem Bintang Yang Ditanahkan." },
  { "en": "Apa Itu Proteksi Cadangan (Backup Protection)?", "id": "Sistem Proteksi Lapis Kedua." },
  { "en": "Apa Itu Sistem Tenaga Tak Terinterupsi (UPS)?", "id": "Menyediakan Daya Instan Saat Listrik Padam." },
  { "en": "Bagaimana Output UPS Tiga Fasa Biasanya Dikonfigurasi?", "id": "Biasanya Dikonfigurasi Sebagai Sistem Bintang." },
  { "en": "Apa Itu Bypass Statis Pada UPS?", "id": "Jalur Alternatif Melalui Saklar Statis." },
  { "en": "Apa Itu Relai Impedansi?", "id": "Relai Proteksi Yang Mengukur Impedansi Saluran." },
  { "en": "Di Mana Relai Impedansi Umumnya Digunakan?", "id": "Pada Saluran Transmisi Jarak Menengah Dan Jauh." },
  { "en": "Apakah Pengaturan Relai Impedansi Bergantung Pada Koneksi Beban?", "id": "Tidak Bergantung Pada Impedansi Saluran." },
  { "en": "Apa Itu Sistem Distribusi Radial?", "id": "Satu Jalur Dari Gardu Induk Ke Beban." },
  { "en": "Apa Itu Sistem Distribusi Cincin (Ring)?", "id": "Beban Diberi Daya Dari Dua Arah." },
  { "en": "Manakah Koneksi Yang Fleksibel Untuk Kedua Sistem Distribusi?", "id": "Keduanya Dapat Diterapkan Sesuai Kebutuhan." },
  { "en": "Manakah Yang Lebih Tahan Terhadap Penurunan Tegangan Sesaat?", "id": "Karakteristiknya Lebih Bergantung Pada Sumber." },
  { "en": "Apa Itu Kekebalan (Immunity) Peralatan?", "id": "Kemampuan Peralatan Beroperasi Saat Ada Gangguan." },
  { "en": "Apa Itu Derating Peralatan?", "id": "Mengurangi Kapasitas Operasi Akibat Kondisi Buruk." },
  { "en": "Kapan Motor Perlu Di-Derating?", "id": "Saat Beroperasi Dengan Ketidakseimbangan Tegangan Tinggi." },
  { "en": "Apa Itu Pemanasan Dielektrik?", "id": "Proses Pemanasan Material Isolatator Listrik." },
  { "en": "Manakah Yang Digunakan Dalam Pemanasan Induksi?", "id": "Menggunakan Prinsip Medan Elektromagnetik." },
  { "en": "Apa Itu Rugi-Rugi Arus Eddy?", "id": "Arus Sirkulasi Dalam Inti Magnetik." },
  { "en": "Bagaimana Mengurangi Rugi-Rugi Arus Eddy?", "id": "Menggunakan Inti Berlapis (Laminasi)." },
  { "en": "Manakah Yang Menghasilkan Fluks Magnetik Lebih Merata?", "id": "Keduanya Dirancang Untuk Fluks Yang Merata." },
  { "en": "Apa Itu Titik Bintang Buatan (Artificial Star Point)?", "id": "Dibuat Menggunakan Jaringan Impedansi." },
  { "en": "Kapan Titik Bintang Buatan Digunakan?", "id": "Saat Netral Dibutuhkan Dari Sistem Delta." },
  { "en": "Manakah Yang Lebih Mudah Diadaptasi Untuk Masa Depan?", "id": "Sistem Bintang Empat Kawat Lebih Fleksibel." },
  { "en": "Apa Itu Konduktor PEN?", "id": "Konduktor Gabungan Fungsi Netral Dan Proteksi." },
  { "en": "Di Sistem Mana Konduktor PEN Ditemukan?", "id": "Pada Sistem Pentanahan TN-C." },
  { "en": "Apa Risiko Menggunakan Konduktor PEN?", "id": "Kehilangan Koneksi Dapat Membuat Sasis Bertegangan." },
  { "en": "Apa Itu Konduktor PE?", "id": "Konduktor Proteksi (Pentanahan) Terpisah." },
  { "en": "Di Sistem Mana Konduktor PE Ditemukan?", "id": "Pada Sistem Pentanahan TN-S." },
  { "en": "Manakah Sistem Yang Lebih Aman TN-C Atau TN-S?", "id": "Sistem TN-S Dianggap Lebih Aman." },
  { "en": "Apa Itu Arus Bocor (Leakage Current)?", "id": "Arus Yang Mengalir Ke Tanah." },
  { "en": "Bagaimana Arus Bocor Dideteksi?", "id": "Menggunakan Perangkat Proteksi Arus Sisa (RCD)." },
  { "en": "Apakah RCD Bekerja Pada Sistem Delta Tidak Ditanahkan?", "id": "Tidak Karena Tidak Ada Jalur Kembali." },
  { "en": "Apa Itu Sistem IT?", "id": "Sistem Dengan Titik Netral Terisolasi." },
  { "en": "Apa Keuntungan Sistem IT?", "id": "Keandalan Tinggi Pada Gangguan Tanah Pertama." },
  { "en": "Di Mana Sistem IT Biasa Digunakan?", "id": "Rumah Sakit Industri Dan Kapal Laut." },
  { "en": "Bagaimana Nasib Gangguan Tanah Kedua Pada Sistem IT?", "id": "Menyebabkan Hubung Singkat Fasa-Ke-Fasa." },
  { "en": "Apa Itu Monitor Isolasi (IMD)?", "id": "Perangkat Untuk Memantau Resistansi Isolasi." },
  { "en": "Di Sistem Mana IMD Wajib Digunakan?", "id": "Pada Sistem IT." },
  { "en": "Manakah Yang Lebih Baik Untuk Drive Servo Presisi?", "id": "Kualitas Daya Lebih Penting Daripada Koneksi." },
  { "en": "Apa Itu Torsi Cogging?", "id": "Pulsasi Torsi Akibat Interaksi Magnet." },
  { "en": "Apakah Koneksi Stator Mempengaruhi Torsi Cogging?", "id": "Tidak Terutama Terkait Desain Motor." },
  { "en": "Apa Itu Torsi Riak (Torque Ripple)?", "id": "Pulsasi Torsi Saat Motor Beroperasi." },
  { "en": "Bagaimana Harmonisa Mempengaruhi Torsi Riak?", "id": "Harmonisa Dapat Meningkatkan Torsi Riak." },
  { "en": "Manakah Yang Menghasilkan Getaran Lebih Sedikit?", "id": "Sistem Seimbang Dengan Kualitas Daya Baik." },
  { "en": "Apa Itu Analisis Tanda Tangan Listrik (ESA)?", "id": "Metode Mendiagnosis Masalah Motor." },
  { "en": "Bagaimana ESA Bekerja?", "id": "Menganalisis Spektrum Arus Dan Tegangan Motor." },
  { "en": "Apakah ESA Dapat Membedakan Koneksi Bintang Dan Delta?", "id": "Ya Karakteristiknya Akan Sedikit Berbeda." },
  { "en": "Manakah Yang Lebih Sulit Didinginkan Secara Efektif?", "id": "Tergantung Pada Desain Dan Ukuran Mesin." },
  { "en": "Apa Itu Kepadatan Fluks Magnetik?", "id": "Ukuran Kekuatan Medan Magnet." },
  { "en": "Bagaimana Tegangan Berlebih Mempengaruhi Kepadatan Fluks?", "id": "Tegangan Berlebih Meningkatkan Kepadatan Fluks." },
  { "en": "Apa Akibat Kepadatan Fluks Terlalu Tinggi?", "id": "Saturasi Inti Dan Pemanasan Berlebih." },
  { "en": "Manakah Yang Lebih Toleran Terhadap Tegangan Berlebih?", "id": "Tergantung Pada Desain Margin Saturasi." },
  { "en": "Apa Itu Autotransformer Starter?", "id": "Menggunakan Autotransformer Untuk Mengurangi Tegangan Start." },
  { "en": "Apa Keuntungan Autotransformer Starter Dibanding Bintang-Delta?", "id": "Memberikan Pengaturan Torsi Awal Yang Fleksibel." },
  { "en": "Manakah Yang Lebih Mahal Autotransformer Atau Bintang-Delta?", "id": "Autotransformer Starter Umumnya Lebih Mahal." },
  { "en": "Apa Itu Pengasutan Direct On Line (DOL)?", "id": "Menghubungkan Motor Langsung Ke Jaringan." },
  { "en": "Kapan Pengasutan DOL Digunakan?", "id": "Untuk Motor Kecil Di Mana Arus Start Tidak Masalah." },
  { "en": "Apa Arus Start Motor DOL?", "id": "Bisa Lima Hingga Delapan Kali Arus Nominal." },
  { "en": "Manakah Yang Membutuhkan Kabel Daya Lebih Besar?", "id": "Metode Start DOL Karena Arus Tinggi." },
  { "en": "Apa Itu Impedansi Urutan Nol?", "id": "Impedansi Sistem Terhadap Arus Urutan Nol." },
  { "en": "Bagaimana Impedansi Urutan Nol Pada Trafo Bintang Ditanahkan?", "id": "Relatif Rendah." },
  { "en": "Bagaimana Impedansi Urutan Nol Pada Trafo Delta?", "id": "Sangat Tinggi Atau Tak Terhingga." },
  { "en": "Mengapa Impedansi Urutan Nol Penting?", "id": "Penting Untuk Perhitungan Arus Gangguan Tanah." },
  { "en": "Manakah Yang Lebih Tahan Terhadap Korosi?", "id": "Material Dan Lingkungan Lebih Menentukan." },
  { "en": "Apa Itu Sistem Scott-T?", "id": "Koneksi Dua Trafo Untuk Konversi Fasa." },
  { "en": "Apa Fungsi Utama Koneksi Scott-T?", "id": "Mengubah Tiga Fasa Menjadi Dua Fasa." },
  { "en": "Di Mana Koneksi Scott-T Digunakan?", "id": "Untuk Menyuplai Beban Dua Fasa Khusus." },
  { "en": "Apakah Koneksi Bintang-Delta Dapat Dibalik Putarannya?", "id": "Ya Dengan Menukar Dua Fasa Mana Saja." },
  { "en": "Apakah Membalik Putaran Mempengaruhi Jenis Koneksi?", "id": "Tidak Hanya Mengubah Urutan Fasa." },
  { "en": "Manakah Yang Lebih Umum Dalam Aplikasi Rumah Tangga?", "id": "Tidak Keduanya Sistem Rumah Tangga Fasa Tunggal." },
  { "en": "Manakah Yang Lebih Berat Untuk Daya Yang Sama?", "id": "Perbedaan Beratnya Biasanya Tidak Signifikan." },
  { "en": "Apa Itu Efek Ferranti?", "id": "Tegangan Ujung Penerima Lebih Tinggi Dari Pengirim." },
  { "en": "Kapan Efek Ferranti Terjadi?", "id": "Pada Saluran Transmisi Panjang Tanpa Beban." },
  { "en": "Apakah Koneksi Bintang-Delta Mempengaruhi Efek Ferranti?", "id": "Tidak Terutama Disebabkan Oleh Kapasitansi Saluran." },
  { "en": "Apa Itu Sistem Per-Unit?", "id": "Normalisasi Kuantitas Listrik Dengan Nilai Dasar." },
  { "en": "Mengapa Sistem Per-Unit Digunakan?", "id": "Menyederhanakan Perhitungan Dan Analisis Sistem." },
  { "en": "Bagaimana Transformator Bintang-Delta Dimodelkan Dalam Sistem Per-Unit?", "id": "Modelnya Memperhitungkan Pergeseran Fasa." },
  { "en": "Apa Itu Jaringan Urutan Positif?", "id": "Representasi Rangkaian Untuk Komponen Urutan Positif." },
  { "en": "Apa Itu Jaringan Urutan Negatif?", "id": "Representasi Rangkaian Untuk Komponen Urutan Negatif." },
  { "en": "Apa Itu Jaringan Urutan Nol?", "id": "Representasi Rangkaian Untuk Komponen Urutan Nol." },
  { "en": "Bagaimana Jaringan Urutan Dihubungkan Untuk Gangguan Satu Fasa?", "id": "Ketiga Jaringan Dihubungkan Secara Seri." },
  { "en": "Bagaimana Netral Dimodelkan Dalam Jaringan Urutan Nol?", "id": "Melalui Tiga Kali Impedansi Pentanahan." },
  { "en": "Apakah Koneksi Delta Memiliki Jalur Dalam Jaringan Urutan Nol?", "id": "Tidak Jalurnya Terbuka." },
  { "en": "Manakah Yang Memiliki Impedansi Urutan Positif Dan Negatif Sama?", "id": "Peralatan Statis Seperti Transformator Dan Saluran." },
  { "en": "Di Mana Impedansi Urutan Positif Dan Negatif Berbeda?", "id": "Pada Mesin Berputar Seperti Generator." },
  { "en": "Apa Itu Papan Nama (Nameplate) Motor?", "id": "Plat Informasi Spesifikasi Teknis Motor." },
  { "en": "Informasi Koneksi Apa Yang Ada Di Papan Nama Motor?", "id": "Diagram Koneksi Untuk Tegangan Berbeda." },
  { "en": "Apa Arti 'Service Factor' Pada Papan Nama?", "id": "Kapasitas Beban Berlebih Berkelanjutan Yang Diizinkan." },
  { "en": "Manakah Yang Lebih Tahan Terhadap Kenaikan Suhu Sekitar?", "id": "Motor Dengan Kelas Isolasi Yang Lebih Tinggi." },
  { "en": "Apa Itu Uji Rotasi Fasa?", "id": "Memverifikasi Urutan Fasa Sebelum Menghubungkan Motor." },
  { "en": "Apa Yang Terjadi Jika Urutan Fasa Motor Terbalik?", "id": "Motor Akan Berputar Ke Arah Sebaliknya." },
  { "en": "Manakah Koneksi Yang Lebih Mudah Diterapkan Pengereman Dinamis?", "id": "Keduanya Dapat Diterapkan Dengan Metode Yang Sesuai." },
  { "en": "Apa Itu Pengereman Dinamis (Dynamic Braking)?", "id": "Mengubah Energi Kinetik Motor Menjadi Panas." },
  { "en": "Manakah Yang Lebih Cenderung Menghasilkan Kebisingan Akustik?", "id": "Kebisingan Terutama Dari Konstruksi Bukan Koneksi." },
  { "en": "Apa Penyebab Utama Kebisingan Pada Transformator?", "id": "Magnetostriksi Pada Inti Besi." },
  { "en": "Apa Itu Magnetostriksi?", "id": "Perubahan Dimensi Material Akibat Medan Magnet." },
  { "en": "Manakah Yang Membutuhkan Pondasi Lebih Kuat?", "id": "Tergantung Berat Total Peralatan." },
  { "en": "Apa Itu Analisis Gangguan Transien?", "id": "Studi Perilaku Sistem Segera Setelah Gangguan." },
  { "en": "Apa Itu Komponen Subtransien, Transien, Dan Mantap?", "id": "Tahapan Arus Gangguan Dari Generator Sinkron." },
  { "en": "Manakah Yang Mempengaruhi Respons Subtransien?", "id": "Terutama Desain Internal Generator." },
  { "en": "Apa Itu Relai Jarak (Distance Relay)?", "id": "Nama Lain Untuk Relai Impedansi." },
  { "en": "Manakah Yang Membutuhkan Pengaturan Relai Arus Sisa?", "id": "Proteksi Gangguan Tanah Pada Sistem Bintang." },
  { "en": "Apa Itu Relai Tegangan Lebih (Overvoltage Relay)?", "id": "Melindungi Peralatan Dari Tegangan Berlebih." },
  { "en": "Apa Itu Relai Tegangan Kurang (Undervoltage Relay)?", "id": "Melindungi Motor Dari Tegangan Terlalu Rendah." },
  { "en": "Manakah Yang Lebih Kritis Terhadap Variasi Frekuensi?", "id": "Keduanya Kritis Karena Mempengaruhi Kecepatan Sinkron." },
  { "en": "Apa Itu Pelepasan Beban (Load Shedding)?", "id": "Memutuskan Beban Untuk Mencegah Keruntuhan Sistem." },
  { "en": "Bagaimana Bintang-Delta Berhubungan Dengan Skema Pelepasan Beban?", "id": "Sebagai Bagian Dari Beban Yang Dikontrol." },
  { "en": "Apa Itu Pusat Kendali Motor (MCC)?", "id": "Rakitan Starter Motor Dan Pemutus Sirkuit." },
  { "en": "Bagaimana MCC Menangani Berbagai Jenis Starter?", "id": "Menyediakan Kompartemen Untuk Starter DOL Bintang-Delta Dll." },
  { "en": "Manakah Yang Lebih Sering Ditemukan Di Jaringan Transmisi?", "id": "Koneksi Delta Pada Sisi Tegangan Tinggi." },
  { "en": "Manakah Yang Lebih Sering Ditemukan Di Jaringan Distribusi Sekunder?", "id": "Koneksi Bintang Empat Kawat." },
  { "en": "Apa Itu Titik Kopling Umum (PCC)?", "id": "Titik Di Mana Beban Terhubung Ke Jaringan." },
  { "en": "Di Mana Kualitas Daya Biasanya Diukur?", "id": "Di Titik Kopling Umum (PCC)." },
  { "en": "Manakah Yang Lebih Tahan Terhadap Beban Harmonik?", "id": "Koneksi Delta Dapat Mengisolasi Harmonisa Triplen." },
  { "en": "Apa Itu Beban Non-Linear?", "id": "Beban Yang Menghasilkan Arus Harmonik." },
  { "en": "Contoh Beban Non-Linear?", "id": "Komputer VFD Dan Penyearah." },
  { "en": "Bagaimana Arus Harmonik Memanaskan Netral Di Sistem Bintang?", "id": "Arus Harmonisa Triplen Menumpuk Di Netral." },
  { "en": "Apa Itu K-Factor Transformator?", "id": "Kemampuan Trafo Menangani Beban Harmonik." },
  { "en": "Apakah Trafo Bintang Dan Delta Memiliki K-Factor Berbeda?", "id": "Desainnya Yang Menentukan Bukan Hanya Koneksi." },
  { "en": "Manakah Yang Menghasilkan Efisiensi Energi Lebih Tinggi?", "id": "Efisiensi Tergantung Desain Bukan Tipe Koneksi." },
  { "en": "Apa Itu Standar NEMA?", "id": "Standar Asosiasi Produsen Listrik Nasional AS." },
  { "en": "Apa Yang Ditetapkan Standar NEMA Untuk Motor?", "id": "Ukuran Kinerja Dan Karakteristik Operasi." },
  { "en": "Manakah Yang Lebih Mudah Dipasang?", "id": "Kompleksitas Pemasangan Keduanya Hampir Sebanding." },
  { "en": "Apa Saja Pertimbangan Keselamatan Saat Memasang Trafo?", "id": "Pentanahan Jarak Bebas Dan Ventilasi." },
  { "en": "Manakah Yang Membutuhkan Ventilasi Lebih Baik?", "id": "Peralatan Dengan Total Kerugian Daya Lebih Besar." },
  { "en": "Apa Itu Kelas Akurasi CT?", "id": "Ukuran Seberapa Akurat CT Mereproduksi Arus." },
  { "en": "Manakah Yang Membutuhkan CT Dengan Akurasi Lebih Tinggi?", "id": "Sistem Pengukuran Pendapatan (Billing Metering)." },
  { "en": "Apa Itu Beban (Burden) CT?", "id": "Total Impedansi Yang Terhubung Ke Sekunder CT." },
  { "en": "Apa Yang Terjadi Jika Beban CT Terlalu Tinggi?", "id": "Akurasi CT Akan Menurun Drastis." },
  { "en": "Mengapa Sekunder CT Tidak Boleh Terbuka?", "id": "Menghasilkan Tegangan Sangat Tinggi Yang Berbahaya." },
  { "en": "Manakah Yang Lebih Sesuai Untuk Penggerak Regeneratif?", "id": "Keduanya Dapat Digunakan Dengan Desain Yang Tepat." },
  { "en": "Apa Itu Penggerak Regeneratif?", "id": "Drive Yang Dapat Mengembalikan Energi Ke Jaringan." },
  { "en": "Di Mana Penggerak Regeneratif Digunakan?", "id": "Lift Crane Dan Kendaraan Listrik." },
  { "en": "Manakah Yang Lebih Mudah Dalam Deteksi Awal Kegagalan?", "id": "Sistem Bintang Ditanahkan Untuk Gangguan Tanah." },
  { "en": "Apa Itu Termografi Inframerah?", "id": "Menggunakan Kamera Termal Untuk Mendeteksi Titik Panas." },
  { "en": "Apa Yang Dapat Dideteksi Termografi Pada Koneksi?", "id": "Koneksi Longgar Atau Korosi." },
  { "en": "Manakah Yang Memerlukan Pengujian Pemeliharaan Lebih Sering?", "id": "Jadwal Pemeliharaan Ditentukan Oleh Kekritisan." },
  { "en": "Apa Itu Uji Resistansi Kontak?", "id": "Mengukur Resistansi Pada Sambungan Listrik." },
  { "en": "Mengapa Resistansi Kontak Rendah Itu Penting?", "id": "Resistansi Tinggi Menyebabkan Pemanasan Dan Kerugian." },
  { "en": "Manakah Yang Lebih Baik Untuk Sistem Cadangan Baterai?", "id": "Inverter Dari Baterai Biasanya Menghasilkan Output Bintang." },
  { "en": "Apa Itu Faktor Puncak (Crest Factor)?", "id": "Rasio Nilai Puncak Dan Nilai RMS." },
  { "en": "Bagaimana Harmonisa Mempengaruhi Faktor Puncak Arus?", "id": "Harmonisa Dapat Meningkatkan Faktor Puncak." },
  { "en": "Peralatan Apa Yang Sensitif Terhadap Faktor Puncak?", "id": "Beberapa Jenis Pemutus Sirkuit Dan Sekering." },
  { "en": "Manakah Yang Lebih Cocok Untuk Aplikasi Frekuensi Tinggi?", "id": "Desain Fisik Lebih Penting Dari Koneksi." },
  { "en": "Apa Itu Efek Kedekatan (Proximity Effect)?", "id": "Distribusi Arus Yang Tidak Merata Akibat Konduktor Berdekatan." },
  { "en": "Manakah Yang Lebih Dipengaruhi Efek Kedekatan?", "id": "Desain Fisik Belitan Menentukan Dampaknya." },
  { "en": "Siapa Yang Pertama Kali Menerapkan Transmisi Tiga Fasa?", "id": "Mikhail Dolivo-Dobrovolsky Pada Tahun 1891." },
  { "en": "Apa Itu Perang Arus (War of Currents)?", "id": "Persaingan Antara Sistem AC Tesla Dan DC Edison." },
  { "en": "Sistem Mana Yang Menang Dalam Perang Arus?", "id": "Sistem Arus Bolak-Balik (AC)." },
  { "en": "Mengapa Sistem AC Menang?", "id": "Karena Kemudahan Transformasi Tegangan." },
  { "en": "Manakah Yang Lebih Mudah Diintegrasikan Dengan Energi Terbarukan?", "id": "Keduanya Memerlukan Antarmuka Elektronika Daya." },
  { "en": "Apa Itu Antarmuka Elektronika Daya?", "id": "Inverter Dan Konverter Untuk Menyesuaikan Listrik." },
  { "en": "Manakah Yang Lebih Toleran Terhadap Variasi Tegangan Input?", "id": "Toleransi Ditentukan Oleh Desain Peralatan." },
  { "en": "Apa Itu Motor Sinkron?", "id": "Motor Di Mana Kecepatan Rotor Sinkron Dengan Frekuensi." },
  { "en": "Bagaimana Stator Motor Sinkron Biasanya Dihubungkan?", "id": "Biasanya Dihubungkan Bintang." },
  { "en": "Apa Fungsi Belitan Peredam (Damper Winding)?", "id": "Meredam Osilasi Dan Membantu Saat Start." },
  { "en": "Manakah Yang Lebih Baik Untuk Aplikasi Daya Besar?", "id": "Keduanya Digunakan Tergantung Filosofi Desain." },
  { "en": "Apa Itu Sistem Tenaga Fleksibel?", "id": "Sistem Yang Dapat Beradaptasi Dengan Perubahan." },
  { "en": "Manakah Yang Menawarkan Fleksibilitas Desain Lebih Besar?", "id": "Setiap Koneksi Memiliki Kelebihan Desainnya Sendiri." },
  { "en": "Apa Itu Analisis Keandalan?", "id": "Studi Untuk Menilai Kemungkinan Kegagalan Sistem." },
  { "en": "Manakah Yang Memiliki Indeks Keandalan Lebih Tinggi?", "id": "Sistem Delta Untuk Gangguan Tanah Tunggal." },
  { "en": "Apa Itu MTBF (Mean Time Between Failures)?", "id": "Waktu Rata-Rata Antara Kegagalan." },
  { "en": "Apa Itu MTTR (Mean Time To Repair)?", "id": "Waktu Rata-Rata Untuk Memperbaiki Kegagalan." },
  { "en": "Manakah Yang Memiliki MTTR Lebih Rendah?", "id": "Gangguan Pada Sistem Bintang Ditanahkan Lebih Cepat Ditemukan." },
  { "en": "Apa Itu Analisis Risiko?", "id": "Proses Mengidentifikasi Dan Mengevaluasi Risiko." },
  { "en": "Manakah Yang Menimbulkan Risiko Keamanan Lebih Tinggi?", "id": "Keduanya Aman Jika Dirancang Dan Dipelihara Benar." },
  { "en": "Apa Itu Studi Sirkuit Pendek?", "id": "Analisis Untuk Menentukan Arus Gangguan." },
  { "en": "Mengapa Studi Sirkuit Pendek Penting?", "id": "Untuk Memilih Rating Peralatan Proteksi." },
  { "en": "Manakah Yang Lebih Rumit Dihitung Arus Gangguannya?", "id": "Gangguan Asimetris Pada Sistem Tak Seimbang." },
  { "en": "Hukum Kirchhoff Apa Yang Berlaku Pada Titik Netral Bintang?", "id": "Hukum Arus Kirchhoff (KCL)." },
  { "en": "Hukum Kirchhoff Apa Yang Berlaku Pada Loop Delta Tertutup?", "id": "Hukum Tegangan Kirchhoff (KVL)." },
  { "en": "Apa Itu Transformator Pentanahan (Grounding Transformer)?", "id": "Menyediakan Titik Netral Buatan." },
  { "en": "Untuk Sistem Apa Transformator Pentanahan Digunakan?", "id": "Untuk Sistem Delta Atau Bintang Tidak Ditanahkan." },
  { "en": "Bagaimana Konfigurasi Umum Transformator Pentanahan?", "id": "Bintang Terhubung Dengan Interkoneksi Zigzag." },
  { "en": "Manakah Yang Dapat Menyebabkan Interferensi Induktif Pada Jalur Komunikasi?", "id": "Arus Urutan Nol Dari Sistem Bintang." },
  { "en": "Mengapa Arus Urutan Nol Menyebabkan Interferensi?", "id": "Karena Fluks Magnetiknya Tidak Saling Menghilangkan." },
  { "en": "Apa Itu Arus Bantalan (Bearing Current) Motor?", "id": "Arus Liar Yang Mengalir Melalui Bantalan." },
  { "en": "Apa Penyebab Utama Arus Bantalan?", "id": "Asimetri Fluks Dan Tegangan Common-Mode VFD." },
  { "en": "Manakah Yang Lebih Rentan Terhadap Kerusakan Akibat Petir?", "id": "Tergantung Pada Efektivitas Sistem Proteksi Surja." },
  { "en": "Apa Itu Pelindung Surja (Surge Protection Device - SPD)?", "id": "Nama Modern Untuk Arester Surja." },
  { "en": "Manakah Yang Lebih Baik Untuk Aplikasi Pembangkit Mikrohidro?", "id": "Generator Asinkron Atau Sinkron Terhubung Bintang." },
  { "en": "Manakah Yang Lebih Sesuai Untuk Panel Surya Skala Besar?", "id": "Inverter Menghasilkan Output Tiga Fasa Bintang." },
  { "en": "Apa Itu Kondensor Sinkron (Synchronous Condenser)?", "id": "Motor Sinkron Tanpa Beban Untuk Koreksi Daya." },
  { "en": "Bagaimana Kondensor Sinkron Biasanya Dihubungkan?", "id": "Biasanya Dihubungkan Bintang." },
  { "en": "Apa Itu Uji Polarisasi Indeks (PI Test)?", "id": "Uji Lanjutan Untuk Mengevaluasi Kualitas Isolasi." },
  { "en": "Berapa Nilai PI Yang Dianggap Baik?", "id": "Nilai Di Atas Dua Dianggap Baik." },
  { "en": "Manakah Yang Memerlukan Analisis Stabilitas Lebih Rinci?", "id": "Sistem Besar Dengan Banyak Generator." },
  { "en": "Apa Itu Redaman (Damping) Pada Sistem Tenaga?", "id": "Kemampuan Sistem Meredam Osilasi." },
  { "en": "Bagaimana Koneksi Mempengaruhi Redaman Sistem?", "id": "Secara Tidak Langsung Melalui Perubahan Impedansi." },
  { "en": "Manakah Yang Lebih Mudah Ditingkatkan Kapasitasnya?", "id": "Peningkatan Kapasitas Memerlukan Desain Ulang." },
  { "en": "Apa Itu Kabel Bawah Tanah?", "id": "Kabel Listrik Yang Ditanam Di Dalam Tanah." },
  { "en": "Apa Perbedaan Utama Kabel Bawah Tanah Dan Udara?", "id": "Kabel Bawah Tanah Memiliki Kapasitansi Tinggi." },
  { "en": "Bagaimana Kapasitansi Tinggi Mempengaruhi Sistem?", "id": "Menghasilkan Arus Pengisian (Charging Current)." },
  { "en": "Manakah Yang Lebih Cocok Untuk Jaringan Kabel Bawah Tanah?", "id": "Keduanya Digunakan Dengan Pertimbangan Desain." },
  { "en": "Apa Itu Reaktor Shunt?", "id": "Induktor Untuk Mengkompensasi Arus Pengisian." },
  { "en": "Bagaimana Reaktor Shunt Biasanya Dihubungkan?", "id": "Dihubungkan Bintang Dengan Titik Netral Ditanahkan." },
  { "en": "Apa Itu Kapasitor Seri?", "id": "Kapasitor Dihubungkan Seri Dengan Saluran." },
  { "en": "Apa Fungsi Kapasitor Seri?", "id": "Meningkatkan Kapasitas Transfer Daya Saluran." },
  { "en": "Manakah Yang Lebih Sering Menggunakan Kompensasi Seri?", "id": "Saluran Transmisi Tegangan Ekstra Tinggi." },
  { "en": "Apa Itu Uji Hi-Pot (High Potential)?", "id": "Uji Ketahanan Tegangan Tinggi Pada Isolasi." },
  { "en": "Manakah Yang Membutuhkan Tegangan Uji Hi-Pot Lebih Tinggi?", "id": "Peralatan Dengan Rating Tegangan Lebih Tinggi." },
  { "en": "Apa Itu Partial Discharge (PD)?", "id": "Pelepasan Listrik Kecil Pada Cacat Isolasi." },
  { "en": "Apa Akibat Dari Partial Discharge?", "id": "Dapat Merusak Dan Menurunkan Kualitas Isolasi." },
  { "en": "Manakah Yang Lebih Mudah Dideteksi PD-nya?", "id": "Teknik Deteksi Berlaku Untuk Semua Peralatan." },
  { "en": "Apa Itu Standar ANSI?", "id": "Standar Dari American National Standards Institute." },
  { "en": "Manakah Yang Lebih Disukai Oleh Standar ANSI?", "id": "Standar Tidak Memihak Tapi Memberi Panduan." },
  { "en": "Apa Itu Faktor Amplifikasi Harmonik?", "id": "Peningkatan Level Harmonik Akibat Resonansi." },
  { "en": "Manakah Yang Lebih Rentan Terhadap Amplifikasi Harmonik?", "id": "Sistem Dengan Bank Kapasitor." },
  { "en": "Apa Itu Beban Fasa Tunggal Ke Fasa Tunggal?", "id": "Beban Dihubungkan Antara Dua Fasa." },
  { "en": "Di Sistem Mana Beban Ini Umum?", "id": "Umum Pada Sistem Delta." },
  { "en": "Manakah Yang Memberikan Keseimbangan Beban Alami Lebih Baik?", "id": "Beban Tiga Fasa Murni." },
  { "en": "Apa Itu Rugi-Rugi Stray Load?", "id": "Kerugian Tambahan Akibat Fluks Bocor." },
  { "en": "Manakah Yang Memiliki Rugi-Rugi Stray Load Lebih Besar?", "id": "Tergantung Pada Desain Dan Konstruksi Mesin." },
  { "en": "Apa Itu Analisis Modal?", "id": "Studi Frekuensi Osilasi Alami Sistem." },
  { "en": "Manakah Yang Memiliki Frekuensi Osilasi Lebih Rendah?", "id": "Tergantung Pada Inersia Dan Impedansi Sistem." },
  { "en": "Apa Itu Monitoring Online?", "id": "Pemantauan Kondisi Peralatan Secara Real-Time." },
  { "en": "Manakah Yang Lebih Penting Untuk Dipantau Secara Online?", "id": "Peralatan Kritis Tanpa Memandang Koneksi." },
  { "en": "Apa Itu Sistem Otomasi Gardu Induk (SAS)?", "id": "Penggunaan Komputer Untuk Mengontrol Gardu Induk." },
  { "en": "Bagaimana SAS Berinteraksi Dengan Koneksi Bintang-Delta?", "id": "Memantau Dan Mengontrol Peralatan Yang Terhubung." },
  { "en": "Manakah Yang Membutuhkan Sistem Kontrol Lebih Canggih?", "id": "Tingkat Kecanggihan Ditentukan Oleh Aplikasi." },
  { "en": "Apa Itu PMU (Phasor Measurement Unit)?", "id": "Perangkat Yang Mengukur Fasor Tegangan Dan Arus." },
  { "en": "Apa Kegunaan PMU?", "id": "Memantau Stabilitas Jaringan Secara Real-Time." },
  { "en": "Manakah Yang Lebih Mudah Diukur Oleh PMU?", "id": "Keduanya Dapat Diukur Dengan Koneksi Yang Tepat." },
  { "en": "Apa Itu WAMS (Wide Area Monitoring Systems)?", "id": "Sistem Pemantauan Menggunakan Data Dari Banyak PMU." },
  { "en": "Apa Itu Keandalan (Reliability) Sistem Tenaga?", "id": "Kemampuan Menyediakan Listrik Tanpa Interupsi." },
  { "en": "Manakah Yang Memberikan Kontribusi Lebih Pada Keandalan?", "id": "Konfigurasi Jaringan Seperti Sistem Cincin." },
  { "en": "Apa Itu Ketahanan (Resilience) Sistem Tenaga?", "id": "Kemampuan Pulih Cepat Dari Gangguan Besar." },
  { "en": "Manakah Yang Lebih Tahan Banting (Resilient)?", "id": "Tergantung Pada Redundansi Dan Skema Pemulihan." },
  { "en": "Apa Itu Interkoneksi Asinkron?", "id": "Menghubungkan Dua Jaringan AC Melalui Konverter DC." },
  { "en": "Manakah Yang Digunakan Dalam Interkoneksi HVDC?", "id": "AC Dikonversi Ke DC Lalu Kembali Ke AC." },
  { "en": "Apa Itu Arus Magnetisasi?", "id": "Arus Yang Dibutuhkan Untuk Menciptakan Fluks Magnetik." },
  { "en": "Manakah Yang Membutuhkan Arus Magnetisasi Lebih Besar?", "id": "Tergantung Pada Desain Inti Transformator." },
  { "en": "Apa Itu Fluks Bocor (Leakage Flux)?", "id": "Fluks Magnetik Yang Tidak Terhubung Dengan Belitan Lain." },
  { "en": "Apa Efek Dari Fluks Bocor?", "id": "Menyebabkan Reaktansi Bocor Dan Jatuh Tegangan." },
  { "en": "Manakah Yang Memiliki Reaktansi Bocor Lebih Rendah?", "id": "Tergantung Pada Pengaturan Fisik Belitan." },
  { "en": "Apa Itu Uji Rasio Belitan (TTR)?", "id": "Memverifikasi Rasio Tegangan Transformator." },
  { "en": "Mengapa TTR Penting?", "id": "Memastikan Transformator Bekerja Sesuai Spesifikasi." },
  { "en": "Manakah Yang Lebih Sulit Diuji TTR-nya?", "id": "Trafo Dengan Grup Vektor Kompleks." },
  { "en": "Apa Itu Uji Tahanan Belitan (Winding Resistance)?", "id": "Mengukur Resistansi DC Dari Belitan." },
  { "en": "Informasi Apa Yang Diberikan Uji Tahanan Belitan?", "id": "Menunjukkan Kesehatan Koneksi Internal." },
  { "en": "Manakah Yang Lebih Penting Diuji Tahanan Belitannya?", "id": "Keduanya Sama Penting Untuk Diuji." },
  { "en": "Apa Itu Faktor Disipasi (Tan Delta)?", "id": "Ukuran Kerugian Dielektrik Pada Isolasi." },
  { "en": "Nilai Tan Delta Yang Baik Itu Bagaimana?", "id": "Nilai Yang Sangat Rendah." },
  { "en": "Manakah Yang Cenderung Memiliki Nilai Tan Delta Lebih Tinggi?", "id": "Peralatan Tua Atau Terkontaminasi." },
  { "en": "Apa Itu Koordinasi Isolasi?", "id": "Mendesain Kekuatan Isolasi Sesuai Tegangan." },
  { "en": "Manakah Yang Membutuhkan Koordinasi Isolasi Lebih Hati-Hati?", "id": "Sistem Tegangan Sangat Tinggi." },
  { "en": "Apa Itu BIL (Basic Insulation Level)?", "id": "Kemampuan Isolasi Menahan Tegangan Impuls Petir." },
  { "en": "Manakah Yang Membutuhkan BIL Lebih Tinggi?", "id": "Peralatan Di Lokasi Rawan Petir." },
  { "en": "Apa Itu SIL (Switching Insulation Level)?", "id": "Kemampuan Isolasi Menahan Tegangan Impuls Switching." },
  { "en": "Manakah Yang Lebih Penting BIL Atau SIL?", "id": "Keduanya Penting Tergantung Tingkat Tegangan." },
  { "en": "Apa Itu Penuaan (Aging) Isolasi?", "id": "Penurunan Kualitas Isolasi Seiring Waktu." },
  { "en": "Faktor Apa Yang Mempercepat Penuaan Isolasi?", "id": "Panas Kelembaban Dan Stres Listrik." },
  { "en": "Manakah Yang Lebih Awet Jika Dirawat Dengan Baik?", "id": "Keduanya Dapat Bertahan Sangat Lama." },
  { "en": "Apa Itu Analisis Biaya-Manfaat?", "id": "Membandingkan Biaya Dan Manfaat Suatu Pilihan." },
  { "en": "Manakah Yang Menawarkan Biaya-Manfaat Terbaik?", "id": "Sangat Tergantung Pada Kebutuhan Spesifik Aplikasi." },
  { "en": "Apa Itu Optimisasi Desain?", "id": "Proses Menemukan Desain Terbaik." },
  { "en": "Apakah Bintang Atau Delta Selalu Menjadi Pilihan Optimal?", "id": "Tidak Pilihan Optimal Tergantung Banyak Faktor." },
  { "en": "Apa Peran Insinyur Listrik?", "id": "Menganalisis Dan Memilih Solusi Teknik Terbaik." },
  { "en": "Apakah Konsep Ini Akan Digantikan Teknologi Baru?", "id": "Tidak Ini Adalah Prinsip Dasar Fundamental." },
  { "en": "Apa Langkah Terakhir Setelah Semua Koneksi Selesai?", "id": "Pengujian Menyeluruh Sebelum Dioperasikan." },
  { "en": "Apa Itu Komisioning (Commissioning)?", "id": "Proses Menguji Dan Menyiapkan Sistem Baru." },
  { "en": "Apakah Prosedur Komisioning Berbeda Untuk Bintang Dan Delta?", "id": "Beberapa Uji Spesifik Mungkin Berbeda." }



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
