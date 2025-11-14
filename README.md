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


  { "en": "Apa Akibat RUU (Rancangan Undang-Undang) Tidak Ditandatangani Presiden?", "id": "Tetap Sah Menjadi UU (30 Hari)." },
  { "en": "Apa Itu Perppu (Peraturan Pemerintah Pengganti Undang-Undang)?", "id": "Peraturan Pemerintah Pengganti Undang-Undang." },
  { "en": "Kapan Presiden Mengeluarkan Perppu?", "id": "Dalam Hal Ihwal Kegentingan Memaksa." },
  { "en": "Siapa Yang Menyetujui Perppu?", "id": "Harus Disetujui DPR (Dewan Perwakilan Rakyat)." },
  { "en": "Apa Status Perppu Jika Ditolak DPR?", "id": "Harus Dicabut (Tidak Berlaku)." },
  { "en": "Apa Itu Peraturan Pemerintah (PP)?", "id": "Peraturan Melaksanakan Perintah UU." },
  { "en": "Apa Itu Peraturan Presiden (Perpres)?", "id": "Peraturan Melaksanakan Perintah PP UU." },
  { "en": "Apa Itu Instruksi Presiden (Inpres)?", "id": "Perintah Presiden Kepada Bawahan (Internal)." },
  { "en": "Apa Kepanjangan Inpres?", "id": "Instruksi Presiden." },
  { "en": "Apa Itu Keputusan Presiden (Keppres)?", "id": "Keputusan Presiden Bersifat Konkret (Pengangkatan)." },
  { "en": "Apa Kepanjangan Keppres?", "id": "Keputusan Presiden." },
  { "en": "Apa Perbedaan Perpres Dan Keppres?", "id": "Perpres Mengatur, Keppres Menetapkan." },
  { "en": "Apa Itu Peraturan Daerah (Perda)?", "id": "Peraturan Dibuat Pemda Dan DPRD." },
  { "en": "Apa Itu Peraturan Kepala Daerah (Perkada)?", "id": "Peraturan Gubernur Atau Bupati/Walikota." },
  { "en": "Apa Itu Desentralisasi?", "id": "Penyerahan Wewenang Pusat Ke Daerah." },
  { "en": "Apa Lawan Desentralisasi?", "id": "Sentralisasi (Pemusatan Kekuasaan)." },
  { "en": "Apa Itu Tugas Pembantuan?", "id": "Penugasan Pusat Ke Daerah (Dana Pusat)." },
  { "en": "Apa Itu Otonomi Daerah?", "id": "Kewenangan Daerah Mengatur Urusan Sendiri." },
  { "en": "Apa Tujuan Otonomi Daerah?", "id": "Kesejahteraan Rakyat, Pelayanan Publik." },
  { "en": "Apa Itu Daerah Otonom?", "id": "Provinsi, Kabupaten, Kota." },
  { "en": "Apa Urusan Absolut Pemerintah Pusat?", "id": "Moneter, Yustisi, Pertahanan, Agama." },
  { "en": "Apa Urusan Konkuren?", "id": "Urusan Dibagi Pusat Dan Daerah." },
  { "en": "Apa Contoh Urusan Konkuren?", "id": "Pendidikan, Kesehatan, Infrastruktur." },
  { "en": "Apa Itu Rumah Sakit Umum Pusat (RSUP)?", "id": "Rumah Sakit Milik Kementerian Kesehatan." },
  { "en": "Apa Contoh RSUP (Rumah Sakit Umum Pusat)?", "id": "RSCM (Rumah Sakit Cipto Mangunkusumo) Jakarta." },
  { "en": "Apa Kepanjangan RSCM?", "id": "Rumah Sakit Cipto Mangunkusumo." },
  { "en": "Apa Itu Balai Latihan Kerja (BLK)?", "id": "Lembaga Pelatihan Keterampilan Kerja." },
  { "en": "Apa Kepanjangan BLK?", "id": "Balai Latihan Kerja." },
  { "en": "BLK (Balai Latihan Kerja) Di Bawah Kementerian Apa?", "id": "Kementerian Ketenagakerjaan." },
  { "en": "Apa Itu Wawasan Kebangsaan?", "id": "Cara Pandang Diri Lingkungan Bangsa." },
  { "en": "Apa Empat Konsensus Dasar Bangsa?", "id": "Pancasila, UUD 1945, NKRI, Bhinneka Tunggal Ika." },
  { "en": "Apa Itu Cinta Tanah Air?", "id": "Rasa Bangga Memiliki Indonesia." },
  { "en": "Apa Contoh Sikap Cinta Tanah Air?", "id": "Menggunakan Produk Dalam Negeri." },
  { "en": "Apa Itu Sadar Berbangsa Bernegara?", "id": "Sadar Hak Kewajiban Warga Negara." },
  { "en": "Apa Contoh Sadar Berbangsa Bernegara?", "id": "Disiplin Berlalu Lintas, Membayar Pajak." },
  { "en": "Apa Itu Setia Pada Pancasila?", "id": "Mengamalkan Nilai-Nilai Pancasila." },
  { "en": "Apa Itu Rela Berkorban?", "id": "Bersedia Mengorbankan Kepentingan Pribadi." },
  { "en": "Apa Itu Kemampuan Awal Bela Negara?", "id": "Kesiapsiagaan Jasmani Mental." },
  { "en": "Apa Itu Nasionalisme?", "id": "Paham Kebangsaan (Cinta Tanah Air)." },
  { "en": "Apa Itu Patriotisme?", "id": "Sikap Rela Berkorban (Jiwa Pahlawan)." },
  { "en": "Apa Itu Chauvinisme?", "id": "Nasionalisme Sempit (Berlebihan, Merendahkan)." },
  { "en": "Apa Itu Sukuisme (Etnosentrisme)?", "id": "Mengagungkan Suku Sendiri." },
  { "en": "Apa Itu Primordialisme?", "id": "Sikap Memegang Teguh Identitas Asal." },
  { "en": "Apa Itu Separatisme?", "id": "Gerakan Politik Memisahkan Diri." },
  { "en": "Apa Itu Disintegrasi?", "id": "Perpecahan Bangsa." },
  { "en": "Apa Itu Reintegrasi?", "id": "Proses Penyatuan Kembali." },
  { "en": "Apa Itu Integrasi Nasional?", "id": "Penyatuan Perbedaan Dalam Bangsa." },
  { "en": "Apa Faktor Pendorong Integrasi Nasional?", "id": "Pancasila, Sumpah Pemuda, Semboyan Bhinneka." },
  { "en": "Apa Faktor Penghambat Integrasi Nasional?", "id": "Keberagaman, Wilayah Luas, Etnosentrisme." },
  { "en": "Apa Itu Semboyan Bhinneka Tunggal Ika?", "id": "Berbeda-Beda Tetapi Tetap Satu Jua." },
  { "en": "Darimana Semboyan Bhinneka Tunggal Ika?", "id": "Kitab Sutasoma." },
  { "en": "Semboyan Itu Ditulis Di Lambang Apa?", "id": "Pita Cengkeraman Garuda Pancasila." },
  { "en": "Apa Itu Toleransi?", "id": "Sikap Menghargai Perbedaan." },
  { "en": "Apa Itu Gotong Royong?", "id": "Bekerja Bersama (Inti Pancasila)." },
  { "en": "Apa Itu Musyawarah Mufakat?", "id": "Pengambilan Keputusan Bersama." },
  { "en": "Musyawarah Mufakat Sesuai Sila Keberapa?", "id": "Sila Keempat Pancasila." },
  { "en": "Apa Itu Aklamasi?", "id": "Pernyataan Setuju Lisan Seluruh Peserta." },
  { "en": "Apa Itu Voting (Suara Terbanyak)?", "id": "Pemungutan Suara (Jika Mufakat Gagal)." },
  { "en": "Apa Itu Demokrasi Pancasila?", "id": "Demokrasi Berdasar Musyawarah (Sila Keempat)." },
  { "en": "Apa Pulau Terbesar Milik Indonesia?", "id": "Pulau Kalimantan (Bagian Indonesia)." },
  { "en": "Apa Pulau Terpadat Penduduknya Di Indonesia?", "id": "Pulau Jawa." },
  { "en": "Apa Pulau Terluar Sisi Barat Indonesia?", "id": "Pulau Benggala (Di Aceh)." },
  { "en": "Apa Pulau Terluar Sisi Timur Indonesia?", "id": "Pulau Liki (Di Papua)." },
  { "en": "Apa Pulau Terluar Sisi Utara Indonesia?", "id": "Pulau Miangas (Sulawesi Utara)." },
  { "en": "Apa Pulau Terluar Sisi Selatan Indonesia?", "id": "Pulau Rote (Nusa Tenggara Timur)." },
  { "en": "Apa Gunung Tertinggi Di Indonesia?", "id": "Puncak Jaya (Di Papua)." },
  { "en": "Apa Danau Terbesar Di Indonesia?", "id": "Danau Toba (Sumatera Utara)." },
  { "en": "Apa Laut Internal Terluas Indonesia?", "id": "Laut Jawa." },
  { "en": "Apa Selat Paling Strategis Di Indonesia?", "id": "Selat Malaka." },
  { "en": "Berapa Perkiraan Jumlah Pulau Di Indonesia?", "id": "Tujuh Belas Ribu Lebih Pulau." },
  { "en": "Apa Garis Imajiner Melintasi Indonesia?", "id": "Garis Khatulistiwa (Ekuator)." },
  { "en": "Apa Tiga Zona Waktu Indonesia?", "id": "WIB, WITA, Dan WIT." },
  { "en": "Apa Kepanjangan WIB (Waktu Indonesia Barat)?", "id": "Waktu Indonesia Barat." },
  { "en": "Apa Kepanjangan WITA (Waktu Indonesia Tengah)?", "id": "Waktu Indonesia Tengah." },
  { "en": "Apa Kepanjangan WIT (Waktu Indonesia Timur)?", "id": "Waktu Indonesia Timur." },
  { "en": "Apa Acuan Waktu WIB (Waktu Indonesia Barat)?", "id": "GMT (Greenwich Mean Time) Tambah Tujuh Jam." },
  { "en": "Apa Acuan Waktu WITA (Waktu Indonesia Tengah)?", "id": "GMT (Greenwich Mean Time) Tambah Delapan Jam." },
  { "en": "Apa Acuan Waktu WIT (Waktu Indonesia Timur)?", "id": "GMT (Greenwich Mean Time) Tambah Sembilan Jam." },
  { "en": "Apa Iklim Negara Indonesia?", "id": "Iklim Tropis Basah." },
  { "en": "Ada Berapa Musim Di Indonesia?", "id": "Dua Musim (Hujan Kering)." },
  { "en": "Angin Apa Yang Membawa Musim Hujan?", "id": "Angin Muson Barat." },
  { "en": "Angin Apa Yang Membawa Musim Kering?", "id": "Angin Muson Timur." },
  { "en": "Berapa Perkiraan Jumlah Penduduk Indonesia?", "id": "Lebih Dari 270 Juta Jiwa." },
  { "en": "Indonesia Peringkat Berapa Terpadat Di Dunia?", "id": "Peringkat Keempat." },
  { "en": "Apa Agama Mayoritas Penduduk Indonesia?", "id": "Agama Islam." },
  { "en": "Apa Kota Terbesar Di Indonesia?", "id": "DKI (Daerah Khusus Ibukota) Jakarta." },
  { "en": "Apa Bahasa Daerah Terbanyak Digunakan?", "id": "Bahasa Jawa." },
  { "en": "Berapa Perkiraan Jumlah Bahasa Daerah?", "id": "Lebih Dari Tujuh Ratus Bahasa." },
  { "en": "Apa Itu Bahasa Ibu?", "id": "Bahasa Pertama Dikuasai Manusia." },
  { "en": "Apa Itu Angka Kelahiran Kasar?", "id": "Jumlah Kelahiran Per Seribu Penduduk." },
  { "en": "Apa Itu Angka Kematian Kasar?", "id": "Jumlah Kematian Per Seribu Penduduk." },
  { "en": "Apa Itu Piramida Penduduk?", "id": "Grafik Komposisi Usia Penduduk." },
  { "en": "Bentuk Piramida Penduduk Indonesia Saat Ini?", "id": "Piramida Ekspansif (Muda Banyak)." },
  { "en": "Apa Itu Bonus Demografi?", "id": "Penduduk Usia Produktif Lebih Dominan." },
  { "en": "Berapa Rentang Usia Produktif?", "id": "Usia 15 Sampai 64 Tahun." },
  { "en": "Apa Tantangan Bonus Demografi?", "id": "Kualitas SDM (Sumber Daya Manusia) Lapangan Kerja." },
  { "en": "Apa Itu Kepadatan Penduduk?", "id": "Jumlah Penduduk Per Kilometer Persegi." },
  { "en": "Provinsi Mana Terpadat Penduduknya?", "id": "Provinsi Jawa Barat." },
  { "en": "Provinsi Mana Paling Sedikit Penduduknya?", "id": "Provinsi Papua Barat Daya." },
  { "en": "Apa Itu Indeks Pembangunan Manusia (IPM)?", "id": "Ukuran Kualitas Hidup (Pendidikan, Kesehatan)." },
  { "en": "Apa Kepanjangan IPM (Indeks Pembangunan Manusia)?", "id": "Indeks Pembangunan Manusia." },
  { "en": "Siapa Yang Menghitung IPM (Indeks Pembangunan Manusia) Indonesia?", "id": "Badan Pusat Statistik (BPS)." },
  { "en": "Kapan Hari Sumpah Pemuda Diperingati?", "id": "Tanggal Dua Puluh Delapan Oktober." },
  { "en": "Kapan Hari Kebangkitan Nasional Diperingati?", "id": "Tanggal Dua Puluh Mei (Budi Utomo)." },
  { "en": "Kapan Hari Pendidikan Nasional Diperingati?", "id": "Tanggal Dua Mei (Ki Hadjar Dewantara)." },
  { "en": "Kapan Hari Pahlawan Diperingati?", "id": "Tanggal Sepuluh November." },
  { "en": "Kapan Hari Proklamasi Kemerdekaan Indonesia?", "id": "Tanggal Tujuh Belas Agustus 1945." },
  { "en": "Kapan Hari Bela Negara Diperingati?", "id": "Tanggal Sembilan Belas Desember." },
  { "en": "Kapan Hari Konstitusi Diperingati?", "id": "Tanggal Delapan Belas Agustus." },
  { "en": "Kapan Hari Ibu Di Indonesia Diperingati?", "id": "Tanggal Dua Puluh Dua Desember." },
  { "en": "Kapan Hari Buruh Internasional Diperingati?", "id": "Tanggal Satu Mei (May Day)." },
  { "en": "Kapan Hari Tani Nasional Diperingati?", "id": "Tanggal Dua Puluh Empat September." },
  { "en": "Kapan Hari Koperasi Nasional Diperingati?", "id": "Tanggal Dua Belas Juli." },
  { "en": "Apa Itu Cuti Bersama?", "id": "Hari Libur Tambahan (Keputusan Pemerintah)." },
  { "en": "Apa Itu Hari Libur Nasional?", "id": "Hari Libur Resmi Ditetapkan Negara." },
  { "en": "Siapa Yang Menetapkan Hari Libur Nasional?", "id": "Presiden (Melalui Keputusan Presiden)." },
  { "en": "Apa Itu Satyalancana?", "id": "Tanda Kehormatan PNS (Pegawai Negeri Sipil) TNI Polri." },
  { "en": "Apa Itu Bintang Mahaputera?", "id": "Tanda Kehormatan Sipil Tertinggi." },
  { "en": "Siapa Yang Menganugerahkan Tanda Kehormatan?", "id": "Presiden Republik Indonesia." },
  { "en": "Apa Itu Gelar Pahlawan Nasional?", "id": "Gelar Kehormatan WNI Berjasa." },
  { "en": "Siapa Yang Menetapkan Gelar Pahlawan Nasional?", "id": "Presiden (Melalui Keputusan Presiden)." },
  { "en": "Apa Tanda Kehormatan Tertinggi Di Indonesia?", "id": "Bintang Republik Indonesia." },
  { "en": "Apa Itu Bintang Gerilya?", "id": "Tanda Kehormatan Pejuang Kemerdekaan." },
  { "en": "Apa Itu Bintang Jasa?", "id": "Tanda Kehormatan Atas Jasa Besar." },
  { "en": "Apa Itu Penghargaan Kalpataru?", "id": "Penghargaan Bidang Lingkungan Hidup." },
  { "en": "Siapa Pemberi Penghargaan Kalpataru?", "id": "Kementerian Lingkungan Hidup Kehutanan." },
  { "en": "Apa Itu Penghargaan Adipura?", "id": "Penghargaan Kota Terbersih Terhijau." },
  { "en": "Siapa Pemberi Penghargaan Adipura?", "id": "Kementerian Lingkungan Hidup Kehutanan." },
  { "en": "Apa Itu Penghargaan Adiwiyata?", "id": "Penghargaan Sekolah Berbudaya Lingkungan." },
  { "en": "Apa Itu Parasamya Purnakarya Nugraha?", "id": "Penghargaan Tertinggi Pembangunan Daerah." },
  { "en": "Apa Itu Penghargaan Upakarti?", "id": "Penghargaan Bidang Industri Kecil Menengah." },
  { "en": "Apa Itu Komisi Pemilihan Umum (KPU)?", "id": "Lembaga Penyelenggara Pemilihan Umum." },
  { "en": "Apa Sifat KPU (Komisi Pemilihan Umum)?", "id": "Nasional, Tetap, Dan Mandiri." },
  { "en": "Berapa Jumlah Anggota KPU (Komisi Pemilihan Umum) Pusat?", "id": "Tujuh Orang." },
  { "en": "Siapa Yang Memilih Anggota KPU (Komisi Pemilihan Umum)?", "id": "DPR (Dewan Perwakilan Rakyat) Atas Usulan Presiden." },
  { "en": "Apa Itu KPU (Komisi Pemilihan Umum) Daerah (KPUD)?", "id": "KPU (Komisi Pemilihan Umum) Di Provinsi Kabupaten/Kota." },
  { "en": "Apa Itu Badan Pengawas Pemilu (Bawaslu)?", "id": "Lembaga Pengawas Penyelenggaraan Pemilu." },
  { "en": "Apa Sifat Bawaslu (Badan Pengawas Pemilu)?", "id": "Tetap Dan Mandiri." },
  { "en": "Apa Tugas Bawaslu (Badan Pengawas Pemilu)?", "id": "Mengawasi Tahapan, Menindak Pelanggaran." },
  { "en": "Apa Itu Gakkumdu (Sentra Penegakan Hukum Terpadu)?", "id": "Tim Penanganan Pidana Pemilu." },
  { "en": "Apa Kepanjangan Gakkumdu?", "id": "Sentra Penegakan Hukum Terpadu." },
  { "en": "Siapa Anggota Gakkumdu (Sentra Penegakan Hukum Terpadu)?", "id": "Bawaslu, Kepolisian, Dan Kejaksaan." },
  { "en": "Apa Itu Dewan Kehormatan Penyelenggara Pemilu (DKPP)?", "id": "Lembaga Penjaga Kode Etik Pemilu." },
  { "en": "Siapa Yang Diadili DKPP (Dewan Kehormatan Penyelenggara Pemilu)?", "id": "Anggota KPU (Komisi Pemilihan Umum) Dan Bawaslu (Badan Pengawas Pemilu)." },
  { "en": "Apa Sanksi DKPP (Dewan Kehormatan Penyelenggara Pemilu)?", "id": "Teguran Sampai Pemberhentian Tetap." },
  { "en": "Apa Itu Pengadaan Barang Jasa Pemerintah?", "id": "Kegiatan Pengadaan Kebutuhan Pemerintah." },
  { "en": "Apa Lembaga Kebijakan Pengadaan Barang Jasa (LKPP)?", "id": "Lembaga Kebijakan Pengadaan Barang Jasa." },
  { "en": "Apa Kepanjangan LKPP (Lembaga Kebijakan Pengadaan Barang Jasa)?", "id": "Lembaga Kebijakan Pengadaan Barang Jasa Pemerintah." },
  { "en": "Apa Itu E-Katalog?", "id": "Sistem Informasi Belanja Online Pemerintah." },
  { "en": "Apa Itu Tender (Lelang)?", "id": "Metode Pemilihan Penyedia Barang Jasa." },
  { "en": "Apa Itu Harga Perkiraan Sendiri (HPS)?", "id": "Perkiraan Harga Barang Jasa Pengadaan." },
  { "en": "Apa Kepanjangan HPS (Harga Perkiraan Sendiri)?", "id": "Harga Perkiraan Sendiri." },
  { "en": "Apa Itu Unit Kerja Pengadaan Barang Jasa (UKPBJ)?", "id": "Unit Kerja Pelaksana Pengadaan." },
  { "en": "Apa Kepanjangan UKPBJ?", "id": "Unit Kerja Pengadaan Barang Jasa." },
  { "en": "Apa Itu Pejabat Pembuat Komitmen (PPK)?", "id": "Pejabat Penetap Kontrak Pengadaan." },
  { "en": "Apa Kepanjangan PPK (Pejabat Pembuat Komitmen)?", "id": "Pejabat Pembuat Komitmen." },
  { "en": "Apa Itu Pejabat Pengadaan (PP)?", "id": "Pejabat Pelaksana Pengadaan Langsung." },
  { "en": "Apa Itu Sanggah (Tender)?", "id": "Protes Peserta Lelang Atas Hasil." },
  { "en": "Apa Itu Sistem Merit ASN (Aparatur Sipil Negara)?", "id": "Kebijakan Manajemen ASN (Kualifikasi Kompetensi)." },
  { "en": "Apa Itu Kualifikasi ASN (Aparatur Sipil Negara)?", "id": "Syarat Pendidikan Formal Jabatan." },
  { "en": "Apa Itu Kompetensi ASN (Aparatur Sipil Negara)?", "id": "Kemampuan Kerja (Teknis, Manajerial)." },
  { "en": "Apa Itu Kinerja ASN (Aparatur Sipil Negara)?", "id": "Hasil Kerja Pegawai Negeri Sipil." },
  { "en": "Apa Itu Seleksi Calon ASN (CASN)?", "id": "Proses Perekrutan Aparatur Sipil Negara." },
  { "en": "Apa Kepanjangan CASN?", "id": "Calon Aparatur Sipil Negara." },
  { "en": "Apa Itu Tes Wawasan Kebangsaan (TWK)?", "id": "Tes Wawasan Kebangsaan (Seleksi CASN)." },
  { "en": "Apa Kepanjangan TWK?", "id": "Tes Wawasan Kebangsaan." },
  { "en": "Apa Itu Tes Intelegensia Umum (TIU)?", "id": "Tes Kemampuan Dasar (Seleksi CASN)." },
  { "en": "Apa Kepanjangan TIU?", "id": "Tes Intelegensia Umum." },
  { "en": "Apa Itu Tes Karakteristik Pribadi (TKP)?", "id": "Tes Karakter Pribadi (Seleksi CASN)." },
  { "en": "Apa Kepanjangan TKP (Tes Karakteristik Pribadi)?", "id": "Tes Karakteristik Pribadi." },
  { "en": "Apa Itu Pangkat Dan Golongan PNS?", "id": "Kedudukan Jenjang Kepangkatan PNS." },
  { "en": "Apa Golongan Terendah PNS (Pegawai Negeri Sipil)?", "id": "Golongan I (Satu)." },
  { "en": "Apa Golongan Tertinggi PNS (Pegawai Negeri Sipil)?", "id": "Golongan IV (Empat)." },
  { "en": "Apa Itu Jabatan Pimpinan Tinggi (JPT)?", "id": "Kelompok Jabatan Tinggi (Eselon)." },
  { "en": "Apa Kepanjangan JPT?", "id": "Jabatan Pimpinan Tinggi." },
  { "en": "Apa Itu Jabatan Fungsional (JF)?", "id": "Jabatan Berbasis Keahlian Keterampilan." },
  { "en": "Apa Kepanjangan JF (Jabatan Fungsional)?", "id": "Jabatan Fungsional." },
  { "en": "Apa Itu Sumpah Janji PNS?", "id": "Sumpah Setia Kepada Pancasila UUD." },
  { "en": "Apa Itu Disiplin PNS (Pegawai Negeri Sipil)?", "id": "Kepatuhan PNS (Pegawai Negeri Sipil) Terhadap Aturan." },
  { "en": "Apa Sanksi Hukuman Disiplin Ringan?", "id": "Teguran Lisan Atau Teguran Tertulis." },
  { "en": "Apa Sanksi Hukuman Disiplin Sedang?", "id": "Penundaan Kenaikan Pangkat Tunjangan." },
  { "en": "Apa Sanksi Hukuman Disiplin Berat?", "id": "Pemberhentian Jabatan Atau PTDH." },
  { "en": "Apa Itu Pemberhentian Tidak Dengan Hormat (PTDH)?", "id": "Pemecatan PNS (Pegawai Negeri Sipil)." },
  { "en": "Apa Kepanjangan PTDH?", "id": "Pemberhentian Tidak Dengan Hormat." },
  { "en": "Apa Alasan PTDH (Pemberhentian Tidak Dengan Hormat) PNS?", "id": "Pidana Berat, Makar, Korupsi." },
  { "en": "Apa Itu Netralitas ASN (Aparatur Sipil Negara)?", "id": "ASN (Aparatur Sipil Negara) Dilarang Berpolitik Praktis." },
  { "en": "Apa Itu Korps Pegawai Republik Indonesia (Korpri)?", "id": "Wadah Pegawai Republik Indonesia." },
  { "en": "Apa Kepanjangan Korpri?", "id": "Korps Pegawai Republik Indonesia." },
  { "en": "Apa Seragam Korpri (Korps Pegawai Republik Indonesia)?", "id": "Batik Biru Korpri." },
  { "en": "Apa Itu Protokol Kenegaraan?", "id": "Tata Acara Resmi Kenegaraan." },
  { "en": "Siapa Penanggung Jawab Protokol Negara?", "id": "Kementerian Sekretariat Negara (Setneg)." },
  { "en": "Apa Itu Tata Tempat?", "id": "Urutan Duduk Pejabat Negara." },
  { "en": "Siapa Pejabat Urutan Pertama Tata Tempat?", "id": "Presiden Republik Indonesia." },
  { "en": "Siapa Pejabat Urutan Kedua Tata Tempat?", "id": "Wakil Presiden Republik Indonesia." },
  { "en": "Apa Itu Tata Upacara Bendera?", "id": "Aturan Upacara Bendera Resmi." },
  { "en": "Kapan Bendera Setengah Tiang Dikibarkan?", "id": "Saat Hari Berkabung Nasional." },
  { "en": "Apa Itu Tanda Jasa?", "id": "Penghargaan Negara (Selain Bintang)." },
  { "en": "Apa Itu Tanda Kehormatan?", "id": "Penghargaan Negara (Bintang Jasa)." },
  { "en": "Apa Nama Istana Kepresidenan Di Jakarta?", "id": "Istana Merdeka Dan Istana Negara." },
  { "en": "Apa Nama Istana Kepresidenan Di Bogor?", "id": "Istana Bogor." },
  { "en": "Apa Nama Istana Kepresidenan Di Jawa Barat?", "id": "Istana Cipanas." },
  { "en": "Apa Nama Istana Kepresidenan Di Yogyakarta?", "id": "Gedung Agung." },
  { "en": "Apa Nama Istana Kepresidenan Di Bali?", "id": "Istana Tampaksiring." },
  { "en": "Apa Nama Mobil Dinas Presiden?", "id": "Indonesia 1 (Satu) Atau RI 1." },
  { "en": "Apa Nama Mobil Dinas Wakil Presiden?", "id": "Indonesia 2 (Dua) Atau RI 2." },
  { "en": "Lagu Indonesia Raya Terdiri Berapa Stanza?", "id": "Tiga Stanza." },
  { "en": "Apa Ukuran Standar Bendera Negara?", "id": "Dua Berbanding Tiga (Lebar Panjang)." },
  { "en": "Apa Bahasa Resmi Negara Indonesia?", "id": "Bahasa Indonesia." },
  { "en": "Apa Dasar Hukum Bahasa Negara?", "id": "Pasal 36 UUD 1945." },
  { "en": "Apa Lembaga Pembina Bahasa Indonesia?", "id": "Badan Pengembangan Pembinaan Bahasa." },
  { "en": "Badan Bahasa Berada Di Bawah Kementerian Apa?", "id": "Kementerian Pendidikan Dan Kebudayaan." },
  { "en": "Apa Itu Uji Kemahiran Berbahasa Indonesia (UKBI)?", "id": "Tes Standar Kemahiran Bahasa Indonesia." },
  { "en": "Apa Kepanjangan UKBI?", "id": "Uji Kemahiran Berbahasa Indonesia." },
  { "en": "Apa Itu Bahasa Indonesia Yang Baik Benar?", "id": "Sesuai Konteks Sesuai Kaidah." },
  { "en": "Apa Itu Peraturan Kepala Desa (Perkades)?", "id": "Peraturan Pelaksana Perdes (Peraturan Desa)." },
  { "en": "Apa Itu Sensus Ekonomi?", "id": "Pendataan Usaha Ekonomi (Sepuluh Tahun)." },
  { "en": "Apa Itu Sensus Pertanian?", "id": "Pendataan Sektor Pertanian (Sepuluh Tahun)." },
  { "en": "Apa Itu Inflasi Volatile Food?", "id": "Inflasi Harga Pangan Bergejolak." },
  { "en": "Siapa Yang Mengendalikan Inflasi Daerah?", "id": "Tim Pengendali Inflasi Daerah (TPID)." },
  { "en": "Apa Kepanjangan TPID?", "id": "Tim Pengendali Inflasi Daerah." },
  { "en": "Apa Itu Operasi Pasar?", "id": "Intervensi Pemerintah Stabilisasi Harga Pangan." },
  { "en": "Apa Itu Harga Eceran Tertinggi (HET)?", "id": "Batas Harga Jual Tertinggi Eceran." },
  { "en": "Apa Kepanjangan HET (Harga Eceran Tertinggi)?", "id": "Harga Eceran Tertinggi." },
  { "en": "Apa Itu Domestic Market Obligation (DMO)?", "id": "Kewajiban Pasokan Kebutuhan Dalam Negeri." },
  { "en": "Apa Kepanjangan DMO?", "id": "Domestic Market Obligation." },
  { "en": "Apa Contoh DMO (Domestic Market Obligation)?", "id": "Kewajiban Produsen Batu Bara (PLN)." },
  { "en": "Apa Itu Hari Libur Fakultatif?", "id": "Hari Libur Lokal (Daerah Tertentu)." },
  { "en": "Apa Contoh Hari Libur Fakultatif?", "id": "Hari Raya Nyepi (Di Bali)." },
  { "en": "Apa Itu Hari Kejepit Nasional (Harpitnas)?", "id": "Hari Kerja Antara Libur (Tidak Resmi)." },
  { "en": "Apa Kepanjangan Harpitnas?", "id": "Hari Kejepit Nasional." },
  { "en": "Apa Itu Anggaran Berbasis Kinerja?", "id": "Anggaran Berdasar Kinerja Output." },
  { "en": "Apa Itu Daftar Isian Pelaksanaan Anggaran (DIPA)?", "id": "Dokumen Pelaksanaan Anggaran Kementerian." },
  { "en": "Apa Kepanjangan DIPA?", "id": "Daftar Isian Pelaksanaan Anggaran." },
  { "en": "Siapa Yang Mengesahkan DIPA (Daftar Isian Pelaksanaan Anggaran)?", "id": "Kementerian Keuangan." },
  { "en": "Apa Itu Hari Jadi TNI (Tentara Nasional Indonesia)?", "id": "Tanggal Lima Oktober." },
  { "en": "Apa Itu Hari Bhayangkara?", "id": "Hari Ulang Tahun Polri (Satu Juli)." },
  { "en": "Apa Itu Hari Kemerdekaan?", "id": "Tujuh Belas Agustus." },
  { "en": "Apa Itu Arsip Nasional Republik Indonesia (ANRI)?", "id": "Lembaga Kearsipan Nasional." },
  { "en": "Apa Tugas ANRI (Arsip Nasional Republik Indonesia)?", "id": "Melaksanakan Tugas Kearsipan Nasional." },
  { "en": "Apa Itu Arsip Statis?", "id": "Arsip Bernilai Sejarah (Disimpan ANRI)." },
  { "en": "Apa Itu Arsip Dinamis?", "id": "Arsip Aktif Digunakan (Di Instansi)." },
  { "en": "Apa Itu Perpustakaan Nasional (Perpusnas)?", "id": "Lembaga Perpustakaan Nasional Indonesia." },
  { "en": "Apa Tugas Perpusnas (Perpustakaan Nasional)?", "id": "Menyimpan Melestarikan Koleksi Karya Nasional." },
  { "en": "Apa Itu Karya Cetak Karya Rekam (KCKR)?", "id": "Karya Wajib Diserahkan Ke Perpusnas." },
  { "en": "Apa Kepanjangan KCKR?", "id": "Karya Cetak Karya Rekam." },
  { "en": "Apa Itu Radio Republik Indonesia (RRI)?", "id": "Lembaga Penyiaran Publik Radio Nasional." },
  { "en": "Apa Kepanjangan RRI (Radio Republik Indonesia)?", "id": "Radio Republik Indonesia." },
  { "en": "Apa Status RRI (Radio Republik Indonesia)?", "id": "Lembaga Penyiaran Publik (Independen)." },
  { "en": "Apa Semboyan RRI (Radio Republik Indonesia)?", "id": "Sekali Di Udara, Tetap Di Udara." },
  { "en": "Apa Itu Televisi Republik Indonesia (TVRI)?", "id": "Lembaga Penyiaran Publik Televisi Nasional." },
  { "en": "Apa Kepanjangan TVRI?", "id": "Televisi Republik Indonesia." },
  { "en": "Apa Status TVRI (Televisi Republik Indonesia)?", "id": "Lembaga Penyiaran Publik (Independen)." },
  { "en": "Apa Semboyan TVRI (Televisi Republik Indonesia)?", "id": "Media Pemersatu Bangsa." },
  { "en": "Apa Itu Lembaga Kantor Berita Nasional (LKBN) Antara?", "id": "Kantor Berita Resmi Milik Negara." },
  { "en": "Apa Kepanjangan LKBN Antara?", "id": "Lembaga Kantor Berita Nasional Antara." },
  { "en": "Apa Tugas LKBN (Lembaga Kantor Berita Nasional) Antara?", "id": "Meliput Menyebarkan Berita Nasional Internasional." },
  { "en": "Apa Itu Komisi Penyiaran Indonesia (KPI)?", "id": "Lembaga Pengawas Penyiaran." },
  { "en": "Apa Kepanjangan KPI (Komisi Penyiaran Indonesia)?", "id": "Komisi Penyiaran Indonesia." },
  { "en": "Apa Tugas KPI (Komisi Penyiaran Indonesia)?", "id": "Mengawasi Isi Siaran TV Radio." },
  { "en": "Apa Itu Pedoman Perilaku Penyiaran (P3)?", "id": "Pedoman Perilaku Lembaga Penyiaran." },
  { "en": "Apa Itu Standar Program Siaran (SPS)?", "id": "Standar Isi Siaran (KPI)." },
  { "en": "Apa Kepanjangan P3SPS?", "id": "Pedoman Perilaku Penyiaran Standar Program Siaran." },
  { "en": "Apa Itu Komisi Informasi Pusat (KIP)?", "id": "Lembaga Urusan Keterbukaan Informasi Publik." },
  { "en": "Apa Kepanjangan KIP (Komisi Informasi Pusat)?", "id": "Komisi Informasi Pusat." },
  { "en": "Apa Tugas KIP (Komisi Informasi Pusat)?", "id": "Menyelesaikan Sengketa Informasi Publik." },
  { "en": "Apa Itu UU KIP (Undang-Undang Keterbukaan Informasi Publik)?", "id": "UU (Undang-Undang) Menjamin Hak Publik Atas Informasi." },
  { "en": "Apa Kepanjangan UU KIP?", "id": "Undang-Undang Keterbukaan Informasi Publik." },
  { "en": "Apa Itu Informasi Publik?", "id": "Informasi Dihasilkan Badan Publik." },
  { "en": "Apa Itu Informasi Dikecualikan?", "id": "Informasi Publik Yang Tidak Boleh Dibuka." },
  { "en": "Apa Contoh Informasi Dikecualikan?", "id": "Rahasia Negara, Data Intelijen." },
  { "en": "Apa Itu Pejabat Pengelola Informasi Dokumentasi (PPID)?", "id": "Pejabat Pengelola Informasi Instansi." },
  { "en": "Apa Kepanjangan PPID?", "id": "Pejabat Pengelola Informasi Dan Dokumentasi." },
  { "en": "Apa Itu Dewan Pers?", "id": "Lembaga Independen Mengawasi Etika Pers." },
  { "en": "Apa Tugas Dewan Pers?", "id": "Melindungi Kemerdekaan Pers, Menjaga Kode Etik." },
  { "en": "Apa Itu Kode Etik Jurnalistik (KEJ)?", "id": "Pedoman Etika Wartawan." },
  { "en": "Apa Kepanjangan KEJ?", "id": "Kode Etik Jurnalistik." },
  { "en": "Apa Itu Hak Tolak Wartawan?", "id": "Hak Wartawan Menolak Ungkap Identitas Narasumber." },
  { "en": "Apa Itu Hak Koreksi?", "id": "Hak Membetulkan Berita Yang Salah." },
  { "en": "Apa Itu Ratifikasi?", "id": "Pengesahan Perjanjian Internasional Menjadi Hukum Nasional." },
  { "en": "Siapa Yang Melakukan Ratifikasi?", "id": "Presiden Dengan Persetujuan DPR." },
  { "en": "Apa Bentuk Hukum Ratifikasi?", "id": "Melalui Undang-Undang Atau Peraturan Presiden." },
  { "en": "Apa Itu Perjanjian Multilateral?", "id": "Perjanjian Antara Banyak Negara." },
  { "en": "Apa Itu Memorandum Of Understanding (MoU)?", "id": "Nota Kesepahaman (Perjanjian Awal)." },
  { "en": "Apa Kepanjangan MoU?", "id": "Memorandum Of Understanding." },
  { "en": "Apa Itu Ekstradisi?", "id": "Penyerahan Pelaku Kejahatan Antar Negara." },
  { "en": "Apa Tujuan Perjanjian Ekstradisi?", "id": "Memudahkan Penangkapan Pelaku Kejahatan Lintas Negara." },
  { "en": "Apa Itu Duta Besar (Dubes)?", "id": "Perwakilan Diplomatik Tertinggi Negara." },
  { "en": "Apa Kepanjangan Dubes?", "id": "Duta Besar." },
  { "en": "Apa Itu Konsul Jenderal (Konjen)?", "id": "Perwakilan Konsuler Di Kota Besar." },
  { "en": "Apa Kepanjangan Konjen?", "id": "Konsul Jenderal." },
  { "en": "Apa Tugas Konsul?", "id": "Pelayanan WNI (Warga Negara Indonesia), Ekonomi, Sosial Budaya." },
  { "en": "Apa Itu Atase?", "id": "Pejabat Perwakilan Bidang Khusus (Militer, Dagang)." },
  { "en": "Apa Itu Atase Pertahanan (Athan)?", "id": "Atase Bidang Pertahanan Militer." },
  { "en": "Apa Itu Kedutaan Besar Republik Indonesia (KBRI)?", "id": "Kantor Perwakilan Diplomatik Indonesia." },
  { "en": "Apa Itu Konsulat Jenderal Republik Indonesia (KJRI)?", "id": "Kantor Perwakilan Konsuler Indonesia." },
  { "en": "Apa Itu Paspor?", "id": "Dokumen Perjalanan Resmi Warga Negara." },
  { "en": "Apa Warna Paspor Biasa Indonesia?", "id": "Warna Hijau." },
  { "en": "Apa Itu Paspor Diplomatik?", "id": "Paspor Untuk Tugas Diplomatik (Warna Hitam)." },
  { "en": "Apa Itu Paspor Dinas?", "id": "Paspor Untuk Tugas Dinas Resmi (Warna Biru)." },
  { "en": "Apa Itu Visa?", "id": "Izin Tinggal Masuk Negara Asing." },
  { "en": "Apa Itu Visa On Arrival (VoA)?", "id": "Visa Diurus Saat Tiba Di Negara Tujuan." },
  { "en": "Apa Itu Bebas Visa?", "id": "Kunjungan Antar Negara Tanpa Visa." },
  { "en": "Apa Itu Warga Negara Indonesia (WNI) Overstay?", "id": "WNI (Warga Negara Indonesia) Melebihi Izin Tinggal Di Luar Negeri." },
  { "en": "Apa Itu Deportasi?", "id": "Pengusiran Paksa Warga Negara Asing." },
  { "en": "Apa Itu Imigrasi?", "id": "Lembaga Pengatur Lalu Lintas Orang." },
  { "en": "Imigrasi Di Bawah Kementerian Apa?", "id": "Kementerian Hukum Dan HAM (Kemenkumham)." },
  { "en": "Apa Itu Tempat Pemeriksaan Imigrasi (TPI)?", "id": "Tempat Pemeriksaan (Bandara, Pelabuhan, PLBN)." },
  { "en": "Apa Kepanjangan TPI (Tempat Pemeriksaan Imigrasi)?", "id": "Tempat Pemeriksaan Imigrasi." },
  { "en": "Apa Itu Daftar Cekal?", "id": "Daftar Larangan Bepergian Ke Luar Negeri." },
  { "en": "Apa Itu Daftar Tangkal?", "id": "Daftar Larangan Masuk Ke Indonesia." },
  { "en": "Apa Itu Diaspora Indonesia?", "id": "Masyarakat Keturunan Indonesia Di Luar Negeri." },
  { "en": "Apa Itu Pemilu Luar Negeri?", "id": "Pemilu Bagi WNI (Warga Negara Indonesia) Di Luar Negeri." },
  { "en": "Siapa Penyelenggara Pemilu Luar Negeri?", "id": "PPLN (Panitia Pemilihan Luar Negeri)." },
  { "en": "Apa Kepanjangan PPLN?", "id": "Panitia Pemilihan Luar Negeri." },
  { "en": "Bagaimana Metode Pemilu Luar Negeri?", "id": "Mencoblos Di TPS (Tempat Pemungutan Suara) Luar Negeri, Pos." },
  { "en": "Apa Itu Ajudan Presiden?", "id": "Perwira TNI/Polri Pendamping Presiden." },
  { "en": "Apa Itu Sekretaris Pribadi Presiden (Sespri)?", "id": "Asisten Pribadi Urusan Administrasi." },
  { "en": "Apa Kepanjangan Sespri?", "id": "Sekretaris Pribadi." },
  { "en": "Apa Itu Pengadilan Niaga?", "id": "Pengadilan Khusus Perkara Kepailitan HAKI." },
  { "en": "Apa Itu Kepailitan?", "id": "Situasi Berhenti Membayar Utang (Bangkrut)." },
  { "en": "Apa Itu Penundaan Kewajiban Pembayaran Utang (PKPU)?", "id": "Penundaan Pembayaran Utang (Restrukturisasi)." },
  { "en": "Apa Kepanjangan PKPU (Penundaan Kewajiban Pembayaran Utang)?", "id": "Penundaan Kewajiban Pembayaran Utang." },
  { "en": "Apa Itu Kurator?", "id": "Pihak Pengurus Harta Pailit." },
  { "en": "Apa Itu Pengadilan Pajak?", "id": "Pengadilan Khusus Sengketa Pajak." },
  { "en": "Apa Itu Sengketa Pajak?", "id": "Perselisihan Antara Wajib Pajak Petugas Pajak." },
  { "en": "Apa Itu Pengadilan Hubungan Industrial (PHI)?", "id": "Pengadilan Khusus Sengketa Perburuhan." },
  { "en": "Apa Itu Sengketa Perburuhan?", "id": "Perselisihan Pekerja Dan Pengusaha." },
  { "en": "Apa Itu Mediasi Hubungan Industrial?", "id": "Perundingan Sengketa Buruh (Mediator)." },
  { "en": "Apa Itu Bipartit?", "id": "Perundingan Antara Pekerja Dan Pengusaha." },
  { "en": "Apa Itu Tripartit?", "id": "Perundingan (Pekerja, Pengusaha, Pemerintah)." },
  { "en": "Apa Itu Lembaga Kerja Sama (LKS) Tripartit?", "id": "Forum Komunikasi Hubungan Industrial." },
  { "en": "Apa Kepanjangan LKS (Lembaga Kerja Sama) Tripartit?", "id": "Lembaga Kerja Sama Tripartit." },
  { "en": "Apa Itu Pengadilan Tindak Pidana Korupsi (Tipikor)?", "id": "Pengadilan Khusus Perkara Korupsi." },
  { "en": "Pengadilan Tipikor Berada Di Lingkungan Apa?", "id": "Peradilan Umum." },
  { "en": "Apa Itu Pengadilan Hak Asasi Manusia (HAM)?", "id": "Pengadilan Khusus Pelanggaran HAM (Hak Asasi Manusia) Berat." },
  { "en": "Apa Itu Pengadilan Militer?", "id": "Pengadilan Khusus Anggota TNI." },
  { "en": "Apa Itu Oditur Militer?", "id": "Jaksa Penuntut Umum Di Peradilan Militer." },
  { "en": "Apa Itu Mahkamah Militer?", "id": "Pengadilan Tingkat Pertama Militer." },
  { "en": "Apa Itu Pengadilan Militer Tinggi?", "id": "Pengadilan Tingkat Banding Militer." },
  { "en": "Apa Itu Pengadilan Militer Utama?", "id": "Pengadilan Tingkat Kasasi Militer (MA)." },
  { "en": "Apa Itu Polisi Militer (PM)?", "id": "Polisi Penegak Hukum Lingkungan TNI." },
  { "en": "Apa Kepanjangan PM (Polisi Militer)?", "id": "Polisi Militer." },
  { "en": "Apa Itu APBN (Anggaran Pendapatan Belanja Negara)?", "id": "Rencana Keuangan Tahunan Negara." },
  { "en": "Apa Fungsi Alokasi APBN?", "id": "Mengalokasikan Anggaran Sektor Pembangunan." },
  { "en": "Apa Fungsi Distribusi APBN?", "id": "Pemerataan Pendapatan (Subsidi, Pajak Progresif)." },
  { "en": "Apa Fungsi Stabilisasi APBN?", "id": "Menjaga Keseimbangan Ekonomi (Inflasi)." },
  { "en": "Apa Sumber Pendapatan Negara Terbesar?", "id": "Penerimaan Pajak." },
  { "en": "Apa Itu Pajak Penghasilan (PPh)?", "id": "Pajak Atas Penghasilan Warga Negara." },
  { "en": "Apa Itu Penerimaan Negara Bukan Pajak (PNBP)?", "id": "Pendapatan Negara Di Luar Pajak." },
  { "en": "Apa Contoh PNBP (Penerimaan Negara Bukan Pajak)?", "id": "Keuntungan BUMN, Royalti Sumber Daya Alam." },
  { "en": "Apa Itu Belanja Pemerintah Pusat?", "id": "Belanja Kementerian Lembaga Negara." },
  { "en": "Apa Itu Transfer Ke Daerah (TKD)?", "id": "Anggaran APBN (Anggaran Pendapatan Belanja Negara) Untuk Daerah." },
  { "en": "Apa Kepanjangan TKD (Transfer Ke Daerah)?", "id": "Transfer Ke Daerah." },
  { "en": "Apa Komponen TKD (Transfer Ke Daerah)?", "id": "Dana Perimbangan Dan Dana Insentif." },
  { "en": "Apa Itu Defisit Anggaran?", "id": "Belanja Negara Lebih Besar Dari Pendapatan." },
  { "en": "Bagaimana Menutup Defisit Anggaran?", "id": "Melalui Pembiayaan (Utang, Surat Berharga)." },
  { "en": "Apa Itu Surplus Anggaran?", "id": "Pendapatan Negara Lebih Besar Dari Belanja." },
  { "en": "Apa Itu Surat Utang Negara (SUN)?", "id": "Surat Pengakuan Utang Negara." },
  { "en": "Apa Kepanjangan SUN (Surat Utang Negara)?", "id": "Surat Utang Negara." },
  { "en": "Apa Itu Surat Berharga Syariah Negara (SBSN)?", "id": "Surat Utang Berprinsip Syariah." },
  { "en": "Apa Kepanjangan SBSN?", "id": "Surat Berharga Syariah Negara." },
  { "en": "Apa Nama Lain SBSN (Surat Berharga Syariah Negara)?", "id": "Sukuk Negara." },
  { "en": "Apa Itu Utang Luar Negeri Pemerintah?", "id": "Pinjaman Pemerintah Dari Pihak Asing." },
  { "en": "Apa Itu Rasio Utang Terhadap PDB?", "id": "Perbandingan Total Utang Dengan PDB." },
  { "en": "Apa Batas Aman Rasio Utang?", "id": "Enam Puluh Persen Dari PDB (UU Keuangan)." },
  { "en": "Apa Itu Laporan Keuangan Pemerintah Pusat (LKPP)?", "id": "Laporan Pertanggungjawaban Keuangan Presiden." },
  { "en": "Siapa Yang Memeriksa LKPP (Laporan Keuangan Pemerintah Pusat)?", "id": "Badan Pemeriksa Keuangan (BPK)." },
  { "en": "Apa Opini Audit BPK (Badan Pemeriksa Keuangan)?", "id": "Pernyataan Wajar Laporan Keuangan." },
  { "en": "Apa Kepanjangan WTP (Wajar Tanpa Pengecualian)?", "id": "Wajar Tanpa Pengecualian." },
  { "en": "Apa Itu Barang Milik Negara (BMN)?", "id": "Aset Tetap Milik Negara." },
  { "en": "Apa Kepanjangan BMN (Barang Milik Negara)?", "id": "Barang Milik Negara." },
  { "en": "Apa Itu Barang Milik Daerah (BMD)?", "id": "Aset Tetap Milik Pemerintah Daerah." },
  { "en": "Apa Kepanjangan BMD (Barang Milik Daerah)?", "id": "Barang Milik Daerah." },
  { "en": "Siapa Pengelola BMN (Barang Milik Negara)?", "id": "Kementerian Keuangan (DJKN)." },
  { "en": "Apa Kepanjangan DJKN (Direktorat Jenderal Kekayaan Negara)?", "id": "Direktorat Jenderal Kekayaan Negara." },
  { "en": "Apa Itu Aset Negara?", "id": "Semua Kekayaan Milik Negara." },
  { "en": "Apa Itu Piutang Negara?", "id": "Tagihan Negara Kepada Pihak Lain." },
  { "en": "Apa Itu Lelang Negara?", "id": "Penjualan Barang Milik Negara Di Muka Umum." },
  { "en": "Apa Itu Inflasi Inti?", "id": "Inflasi Di Luar Harga Bergejolak." },
  { "en": "Apa Itu Inflasi Harga Bergejolak (Volatile Food)?", "id": "Inflasi Harga Pangan (Cabai, Bawang)." },
  { "en": "Apa Itu Inflasi Harga Diatur Pemerintah (Administered Price)?", "id": "Inflasi Harga (BBM, Listrik, Rokok)." },
  { "en": "Apa Itu Indeks Keyakinan Konsumen (IKK)?", "id": "Indikator Optimisme Konsumen Terhadap Ekonomi." },
  { "en": "Apa Kepanjangan IKK (Indeks Keyakinan Konsumen)?", "id": "Indeks Keyakinan Konsumen." },
  { "en": "Apa Itu Purchasing Managers Index (PMI)?", "id": "Indikator Aktivitas Sektor Manufaktur." },
  { "en": "Apa Kepanjangan PMI (Purchasing Managers Index)?", "id": "Purchasing Managers Index." },
  { "en": "Apa Arti PMI (Purchasing Managers Index) Di Atas 50?", "id": "Sektor Manufaktur Sedang Berekspansi." },
  { "en": "Apa Arti PMI (Purchasing Managers Index) Di Bawah 50?", "id": "Sektor Manufaktur Sedang Kontraksi." },
  { "en": "Apa Itu Neraca Pembayaran Indonesia (NPI)?", "id": "Catatan Transaksi Ekonomi Indonesia Dunia." },
  { "en": "Apa Kepanjangan NPI (Neraca Pembayaran Indonesia)?", "id": "Neraca Pembayaran Indonesia." },
  { "en": "Apa Komponen Neraca Pembayaran Indonesia (NPI)?", "id": "Transaksi Berjalan Dan Transaksi Modal." },
  { "en": "Apa Itu Transaksi Berjalan (Current Account)?", "id": "Transaksi Ekspor Impor Jasa." },
  { "en": "Apa Itu Defisit Transaksi Berjalan?", "id": "Impor Lebih Besar Dari Ekspor." },
  { "en": "Apa Itu Surplus Transaksi Berjalan?", "id": "Ekspor Lebih Besar Dari Impor." },
  { "en": "Apa Itu Transaksi Modal Finansial?", "id": "Transaksi Investasi (Asing, Portofolio)." },
  { "en": "Apa Itu Cadangan Devisa?", "id": "Simpanan Valuta Asing Bank Sentral." },
  { "en": "Siapa Yang Mengelola Cadangan Devisa?", "id": "Bank Indonesia." },
  { "en": "Apa Fungsi Cadangan Devisa?", "id": "Menstabilkan Rupiah, Membayar Utang Luar Negeri." },
  { "en": "Apa Itu Gini Ratio (Rasio Gini)?", "id": "Indikator Kesenjangan Distribusi Pendapatan." },
  { "en": "Berapa Rentang Angka Rasio Gini?", "id": "Nol Sampai Satu." },
  { "en": "Apa Arti Rasio Gini Mendekati Nol?", "id": "Kesenjangan Pendapatan Rendah (Merata)." },
  { "en": "Apa Arti Rasio Gini Mendekati Satu?", "id": "Kesenjangan Pendapatan Tinggi (Timpang)." },
  { "en": "Siapa Yang Menetapkan Garis Kemiskinan?", "id": "Badan Pusat Statistik (BPS)." },
  { "en": "Apa Itu Penduduk Miskin?", "id": "Penduduk Berpendapatan Di Bawah Garis Kemiskinan." },
  { "en": "Apa Itu Kemiskinan Ekstrem?", "id": "Kondisi Kemiskinan Paling Parah." },
  { "en": "Apa Target Kemiskinan Ekstrem Indonesia?", "id": "Nol Persen Pada Tahun 2024." },
  { "en": "Apa Itu Tingkat Pengangguran Terbuka (TPT)?", "id": "Persentase Pengangguran Terhadap Angkatan Kerja." },
  { "en": "Apa Kepanjangan TPT (Tingkat Pengangguran Terbuka)?", "id": "Tingkat Pengangguran Terbuka." },
  { "en": "Apa Itu Pengangguran Friksional?", "id": "Pengangguran Sementara (Pindah Kerja)." },
  { "en": "Apa Itu Pengangguran Struktural?", "id": "Pengangguran Akibat Perubahan Struktur Ekonomi." },
  { "en": "Apa Itu Pengangguran Siklikal?", "id": "Pengangguran Akibat Resesi Ekonomi." },
  { "en": "Apa Itu Pengangguran Musiman?", "id": "Pengangguran Akibat Perubahan Musim (Petani)." },
  { "en": "Apa Itu Angkatan Kerja?", "id": "Penduduk Usia Produktif (Bekerja Mencari Kerja)." },
  { "en": "Apa Itu Bukan Angkatan Kerja?", "id": "Usia Produktif Tidak Bekerja (Sekolah, IRT)." },
  { "en": "Apa Kepanjangan IRT (Ibu Rumah Tangga)?", "id": "Ibu Rumah Tangga." },
  { "en": "Apa Itu Tingkat Partisipasi Angkatan Kerja (TPAK)?", "id": "Persentase Angkatan Kerja (Usia Produktif)." },
  { "en": "Apa Kepanjangan TPAK?", "id": "Tingkat Partisipasi Angkatan Kerja." },
  { "en": "Apa Itu Sektor Formal?", "id": "Pekerjaan Terdaftar Resmi (Gaji Tetap)." },
  { "en": "Apa Itu Sektor Informal?", "id": "Pekerjaan Tidak Terdaftar (Wiraswasta, Buruh Lepas)." },
  { "en": "Apa Itu Pekerja Penuh?", "id": "Bekerja Minimal 35 Jam Seminggu." },
  { "en": "Apa Itu Pekerja Setengah Menganggur?", "id": "Bekerja Kurang Dari 35 Jam Seminggu." },
  { "en": "Apa Itu Revolusi Hijau?", "id": "Peningkatan Produksi Pertanian (Bibit Unggul)." },
  { "en": "Apa Program Revolusi Hijau Orde Baru?", "id": "Panca Usaha Tani." },
  { "en": "Apa Isi Panca Usaha Tani?", "id": "Bibit Unggul, Pupuk, Irigasi, Hama, Olah Tanah." },
  { "en": "Apa Itu Intensifikasi Pertanian?", "id": "Peningkatan Produksi Di Lahan Sama." },
  { "en": "Apa Itu Ekstensifikasi Pertanian?", "id": "Peningkatan Produksi (Membuka Lahan Baru)." },
  { "en": "Apa Itu Diversifikasi Pertanian?", "id": "Penganekaragaman Jenis Tanaman." },
  { "en": "Apa Itu Rehabilitasi Pertanian?", "id": "Pemulihan Lahan Pertanian Kritis." },
  { "en": "Apa Itu Sektor Agraris?", "id": "Sektor Ekonomi Berbasis Pertanian." },
  { "en": "Apa Itu Sektor Industri?", "id": "Sektor Ekonomi Berbasis Pengolahan." },
  { "en": "Apa Itu Sektor Jasa?", "id": "Sektor Ekonomi Berbasis Pelayanan." },
  { "en": "Apa Itu Transformasi Ekonomi?", "id": "Pergeseran Sektor (Agraris Ke Industri/Jasa)." },
  { "en": "Apa Itu Moratorium?", "id": "Penundaan Atau Penghentian Sementara." },
  { "en": "Apa Contoh Moratorium?", "id": "Moratorium Izin Pembukaan Lahan Sawit." },
  { "en": "Apa Itu Perhutanan Sosial?", "id": "Pemberian Akses Legal Masyarakat (Hutan)." },
  { "en": "Apa Tujuan Perhutanan Sosial?", "id": "Kesejahteraan Masyarakat Sekitar Hutan." },
  { "en": "Apa Itu Konflik Agraria?", "id": "Sengketa Kepemilikan Lahan." },
  { "en": "Apa Itu Reforma Agraria?", "id": "Penataan Ulang Kepemilikan Lahan." },
  { "en": "Apa Itu Redistribusi Aset?", "id": "Pembagian Lahan Negara Ke Rakyat." },
  { "en": "Apa Itu Legalisasi Aset?", "id": "Pemberian Sertifikat Tanah (Prona/PTSL)." },
  { "en": "Apa Kepanjangan Prona?", "id": "Proyek Operasi Nasional Agraria." },
  { "en": "Apa Kepanjangan PTSL?", "id": "Pendaftaran Tanah Sistematis Lengkap." },
  { "en": "Apa Itu Badan Pertanahan Nasional (BPN)?", "id": "Lembaga Urusan Pertanahan." },
  { "en": "Apa Kepanjangan BPN (Badan Pertanahan Nasional)?", "id": "Badan Pertanahan Nasional." },
  { "en": "BPN (Badan Pertanahan Nasional) Berada Di Bawah Kementerian Apa?", "id": "Kementerian Agraria Tata Ruang (ATR)." },
  { "en": "Apa Itu Tata Ruang?", "id": "Pengaturan Pemanfaatan Ruang Wilayah." },
  { "en": "Apa Itu Rencana Tata Ruang Wilayah (RTRW)?", "id": "Rencana Pola Pemanfaatan Ruang Wilayah." },
  { "en": "Apa Kepanjangan RTRW?", "id": "Rencana Tata Ruang Wilayah." },
  { "en": "Apa Itu Zona Hijau?", "id": "Kawasan Lindung (Tidak Boleh Dibangun)." },
  { "en": "Apa Itu Zona Kuning?", "id": "Kawasan Peruntukan Pemukiman." },
  { "en": "Apa Itu Zona Merah?", "id": "Kawasan Rawan Bencana (Larangan)." },
  { "en": "Apa Itu Izin Mendirikan Bangunan (IMB)?", "id": "Izin Mendirikan Bangunan (Dulu)." },
  { "en": "Apa Kepanjangan IMB (Izin Mendirikan Bangunan)?", "id": "Izin Mendirikan Bangunan." },
  { "en": "Apa Pengganti IMB (Izin Mendirikan Bangunan) Saat Ini?", "id": "Persetujuan Bangunan Gedung (PBG)." },
  { "en": "Apa Kepanjangan PBG (Persetujuan Bangunan Gedung)?", "id": "Persetujuan Bangunan Gedung." },
  { "en": "Apa Itu Sertifikat Laik Fungsi (SLF)?", "id": "Sertifikat Kelayakan Bangunan Gedung." },
  { "en": "Apa Kepanjangan SLF (Sertifikat Laik Fungsi)?", "id": "Sertifikat Laik Fungsi." },
  { "en": "Apa Itu Koefisien Dasar Bangunan (KDB)?", "id": "Batas Luas Lantai Dasar Bangunan." },
  { "en": "Apa Kepanjangan KDB (Koefisien Dasar Bangunan)?", "id": "Koefisien Dasar Bangunan." },
  { "en": "Apa Itu Koefisien Lantai Bangunan (KLB)?", "id": "Batas Jumlah Luas Lantai Bangunan." },
  { "en": "Apa Kepanjangan KLB (Koefisien Lantai Bangunan)?", "id": "Koefisien Lantai Bangunan." },
  { "en": "Apa Itu Garis Sempadan Bangunan (GSB)?", "id": "Batas Minimal Bangunan Dari Jalan." },
  { "en": "Apa Kepanjangan GSB (Garis Sempadan Bangunan)?", "id": "Garis Sempadan Bangunan." },
  { "en": "Apa Itu Ruang Terbuka Hijau (RTH)?", "id": "Kawasan Hijau Perkotaan (Taman)." },
  { "en": "Apa Kepanjangan RTH (Ruang Terbuka Hijau)?", "id": "Ruang Terbuka Hijau." },
  { "en": "Berapa Persentase Minimal RTH (Ruang Terbuka Hijau) Perkotaan?", "id": "Tiga Puluh Persen." },
  { "en": "Apa Itu RTH (Ruang Terbuka Hijau) Publik?", "id": "RTH (Ruang Terbuka Hijau) Milik Pemerintah (Dua Puluh Persen)." },
  { "en": "Apa Itu RTH (Ruang Terbuka Hijau) Privat?", "id": "RTH (Ruang Terbuka Hijau) Milik Swasta (Sepuluh Persen)." },
  { "en": "Apa Itu Jalur Hijau?", "id": "Kawasan Hijau Pemisah Jalan." },
  { "en": "Apa Itu Trotoar?", "id": "Jalur Pejalan Kaki." },
  { "en": "Apa Hak Pejalan Kaki?", "id": "Hak Atas Trotoar Yang Aman." },
  { "en": "Apa Itu Pembangunan Infrastruktur?", "id": "Pembangunan Fasilitas Fisik Publik." },
  { "en": "Apa Contoh Infrastruktur?", "id": "Jalan, Jembatan, Pelabuhan, Listrik." },
  { "en": "Apa Kepanjangan PSN (Proyek Strategis Nasional)?", "id": "Proyek Strategis Nasional." },
  { "en": "Apa Itu Kerja Sama Pemerintah Badan Usaha (KPBU)?", "id": "Kerja Sama Pemerintah Swasta (Infrastruktur)." },
  { "en": "Apa Kepanjangan KPBU?", "id": "Kerja Sama Pemerintah Badan Usaha." },
  { "en": "Apa Itu Pengadaan Tanah?", "id": "Proses Pembebasan Lahan Proyek." },
  { "en": "Apa Itu Konsinyasi (Penitipan Ganti Rugi)?", "id": "Penitipan Ganti Rugi Di Pengadilan." },
  { "en": "Kapan Konsinyasi Dilakukan?", "id": "Jika Warga Menolak Nilai Ganti Rugi." },
  { "en": "Apa Itu Juru Ukur Tanah?", "id": "Petugas Pengukur Batas Tanah (BPN)." },
  { "en": "Apa Itu Akta Jual Beli (AJB)?", "id": "Dokumen Jual Beli Tanah." },
  { "en": "Apa Kepanjangan AJB (Akta Jual Beli)?", "id": "Akta Jual Beli." },
  { "en": "Siapa Yang Membuat AJB (Akta Jual Beli)?", "id": "Pejabat Pembuat Akta Tanah (PPAT)." },
  { "en": "Apa Kepanjangan PPAT (Pejabat Pembuat Akta Tanah)?", "id": "Pejabat Pembuat Akta Tanah." },
  { "en": "Apa Itu Balik Nama Sertifikat?", "id": "Proses Perubahan Nama Pemilik Tanah." },
  { "en": "Apa Itu Hak Tanggungan?", "id": "Jaminan Utang Atas Tanah." },
  { "en": "Apa Itu Surat Kuasa?", "id": "Pelimpahan Wewenang Kepada Orang Lain." },
  { "en": "Apa Itu Surat Perjanjian?", "id": "Dokumen Kesepakatan Antar Pihak." },
  { "en": "Apa Itu Perjanjian Kerja?", "id": "Kesepakatan Antara Pekerja Dan Pengusaha." },
  { "en": "Apa Isi Perjanjian Kerja?", "id": "Hak, Kewajiban, Upah, Jangka Waktu." },
  { "en": "Apa Itu Tunjangan Hari Raya (THR)?", "id": "Pendapatan Non-Upah Wajib (Jelang Hari Raya)." },
  { "en": "Apa Kepanjangan THR (Tunjangan Hari Raya)?", "id": "Tunjangan Hari Raya." },
  { "en": "Kapan THR (Tunjangan Hari Raya) Wajib Dibayar?", "id": "Paling Lambat 7 Hari Sebelum Hari Raya." },
  { "en": "Apa Itu Upah Lembur?", "id": "Upah Kerja Di Luar Jam Kerja." },
  { "en": "Apa Itu Cuti Tahunan?", "id": "Hak Istirahat Pekerja (12 Hari)." },
  { "en": "Apa Itu Cuti Melahirkan?", "id": "Hak Istirahat Pekerja Perempuan Melahirkan." },
  { "en": "Apa Itu Cuti Haid?", "id": "Hak Istirahat Pekerja Perempuan (Haid)." },
  { "en": "Apa Itu Tenaga Kerja Wanita (TKW)?", "id": "Tenaga Kerja Wanita (Di Luar Negeri)." },
  { "en": "Apa Kepanjangan TKW (Tenaga Kerja Wanita)?", "id": "Tenaga Kerja Wanita." },
  { "en": "Apa Itu Pekerja Rumah Tangga (PRT)?", "id": "Pekerja Sektor Rumah Tangga." },
  { "en": "Apa Kepanjangan PRT (Pekerja Rumah Tangga)?", "id": "Pekerja Rumah Tangga." },
  { "en": "Apa Itu Disabilitas?", "id": "Keterbatasan Fisik Mental Jangka Panjang." },
  { "en": "Apa Hak Penyandang Disabilitas?", "id": "Hak Kesetaraan (Pekerjaan, Pendidikan, Aksesibilitas)." },
  { "en": "Apa Itu Aksesibilitas?", "id": "Kemudahan Fasilitas Bagi Penyandang Disabilitas." },
  { "en": "Apa Contoh Aksesibilitas?", "id": "Ram (Jalur Landai), Lift Khusus." },
  { "en": "Apa Itu Bahasa Isyarat Indonesia (Bisindo)?", "id": "Bahasa Isyarat Komunitas Tuli Indonesia." },
  { "en": "Apa Kepanjangan Bisindo?", "id": "Bahasa Isyarat Indonesia." },
  { "en": "Apa Itu Sistem Isyarat Bahasa Indonesia (SIBI)?", "id": "Sistem Isyarat (Bukan Bahasa) Formal." },
  { "en": "Apa Kepanjangan SIBI?", "id": "Sistem Isyarat Bahasa Indonesia." },
  { "en": "Apa Itu Huruf Braille?", "id": "Sistem Tulisan Sentuh (Tunanetra)." },
  { "en": "Apa Itu Sekolah Luar Biasa (SLB)?", "id": "Sekolah Khusus Anak Berkebutuhan Khusus." },
  { "en": "Apa Kepanjangan SLB (Sekolah Luar Biasa)?", "id": "Sekolah Luar Biasa." },
  { "en": "Apa Itu Pendidikan Inklusi?", "id": "Sistem Pendidikan (Anak Berkebutuhan Khusus Belajar Bersama)." },
  { "en": "Apa Itu Komisi Nasional Disabilitas (KND)?", "id": "Lembaga Pengawas Pemenuhan Hak Disabilitas." },
  { "en": "Apa Kepanjangan KND (Komisi Nasional Disabilitas)?", "id": "Komisi Nasional Disabilitas." },
  { "en": "Apa Itu Lansia (Lanjut Usia)?", "id": "Penduduk Berusia 60 Tahun Ke Atas." },
  { "en": "Apa Itu Jaminan Sosial Lansia?", "id": "Program Perlindungan Sosial Lanjut Usia." },
  { "en": "Apa Itu Pos Binaan Terpadu (Posbindu)?", "id": "Pos Pembinaan Kesehatan (Lansia, PTM)." },
  { "en": "Apa Kepanjangan Posbindu?", "id": "Pos Binaan Terpadu." },
  { "en": "Apa Itu Anak Yatim Piatu?", "id": "Anak Tanpa Ayah Dan Ibu." },
  { "en": "Apa Itu Panti Asuhan?", "id": "Lembaga Kesejahteraan Sosial Anak." },
  { "en": "Apa Itu Panti Jompo?", "id": "Lembaga Kesejahteraan Sosial Lansia." },
  { "en": "Apa Itu Adopsi Anak?", "id": "Pengangkatan Anak Secara Sah." },
  { "en": "Apa Itu Akta Adopsi?", "id": "Bukti Sah Pengangkatan Anak." },
  { "en": "Apa Itu Kawin Kontrak?", "id": "Perkawinan Jangka Waktu Tertentu (Dilarang)." },
  { "en": "Apa Itu Kawin Lari?", "id": "Perkawinan Tanpa Izin Orang Tua." },
  { "en": "Apa Itu Wali Nikah?", "id": "Pihak Laki-laki (Ayah) Menikahkan Perempuan." },
  { "en": "Apa Itu Mahar (Mas Kawin)?", "id": "Pemberian Wajib Calon Suami Ke Istri." },
  { "en": "Apa Itu Ijab Kabul?", "id": "Prosesi Akad Nikah (Serah Terima)." },
  { "en": "Apa Itu Talak?", "id": "Ucapan Cerai Suami Kepada Istri." },
  { "en": "Apa Itu Gugat Cerai?", "id": "Gugatan Cerai Istri Kepada Suami." },
  { "en": "Apa Itu Masa Iddah?", "id": "Masa Tunggu Wanita Pasca Cerai/Meninggal." },
  { "en": "Apa Itu Rujuk?", "id": "Kembalinya Suami Istri (Pasca Talak)." },
  { "en": "Apa Itu KUA (Kantor Urusan Agama)?", "id": "Kantor Urusan Agama (Pencatatan Nikah Islam)." },
  { "en": "Apa Itu Catatan Sipil?", "id": "Pencatatan Peristiwa Penting (Lahir, Mati, Nikah)." },
  { "en": "Apa Itu Sumpah Jabatan?", "id": "Sumpah Janji Pejabat Negara." },
  { "en": "Apa Itu Pelantikan?", "id": "Pengangkatan Resmi Pejabat." },
  { "en": "Apa Itu Serah Terima Jabatan (Sertijab)?", "id": "Upacara Serah Terima Jabatan." },
  { "en": "Apa Kepanjangan Sertijab?", "id": "Serah Terima Jabatan." },
  { "en": "Apa Itu Mutasi Jabatan?", "id": "Perpindahan Jabatan (Rotasi, Promosi)." },
  { "en": "Apa Itu Promosi Jabatan?", "id": "Kenaikan Jabatan Ke Jenjang Lebih Tinggi." },
  { "en": "Apa Itu Demosi Jabatan?", "id": "Penurunan Jabatan." },
  { "en": "Apa Itu Non-Job?", "id": "Pencopotan Jabatan (Tanpa Jabatan Baru)." },
  { "en": "Apa Itu Pelaksana Tugas (Plt)?", "id": "Pejabat Sementara (Jabatan Sama/Lebih Tinggi)." },
  { "en": "Apa Kepanjangan Plt (Pelaksana Tugas)?", "id": "Pelaksana Tugas." },
  { "en": "Apa Itu Pelaksana Harian (Plh)?", "id": "Pejabat Sementara (Jabatan Sama/Lebih Rendah)." },
  { "en": "Apa Kepanjangan Plh (Pelaksana Harian)?", "id": "Pelaksana Harian." },
  { "en": "Apa Itu Pejabat (Pj) Kepala Daerah?", "id": "Pejabat Mengisi Kekosongan Kepala Daerah." },
  { "en": "Apa Kepanjangan Pj (Pejabat)?", "id": "Pejabat." },
  { "en": "Siapa Yang Mengisi Pj (Pejabat) Gubernur?", "id": "Pejabat Pimpinan Tinggi Madya Pusat/Provinsi." },
  { "en": "Siapa Yang Mengisi Pj (Pejabat) Bupati/Wali Kota?", "id": "Pejabat Pimpinan Tinggi Pratama Pusat/Provinsi." },
  { "en": "Siapa Yang Melantik Pj (Pejabat) Gubernur?", "id": "Menteri Dalam Negeri (Mendagri)." },
  { "en": "Siapa Yang Melantik Pj (Pejabat) Bupati/Wali Kota?", "id": "Gubernur." },
  { "en": "Apa Itu Pilkada Serentak?", "id": "Pemilihan Kepala Daerah Serentak Nasional." },
  { "en": "Kapan Pilkada Serentak Nasional Dilaksanakan?", "id": "Bulan November 2024." },
  { "en": "Apa Itu Aparatur Sipil Negara (ASN)?", "id": "Pegawai Negeri Sipil Dan PPPK." },
  { "en": "Apa Kepanjangan PPPK (Pegawai Pemerintah Perjanjian Kerja)?", "id": "Pegawai Pemerintah Perjanjian Kerja." },
  { "en": "Apa Perbedaan PNS (Pegawai Negeri Sipil) Dan PPPK?", "id": "PNS (Pegawai Negeri Sipil) Punya Jaminan Pensiun." },
  { "en": "Apa Itu Digitalisasi ASN (Aparatur Sipil Negara)?", "id": "Transformasi ASN (Aparatur Sipil Negara) Menuju Digital." },
  { "en": "Apa Itu Core Values ASN (Aparatur Sipil Negara)?", "id": "Nilai-Nilai Dasar ASN (Aparatur Sipil Negara)." },
  { "en": "Apa Core Values ASN (Aparatur Sipil Negara) Saat Ini?", "id": "BerAKHLAK." },
  { "en": "Apa Kepanjangan BerAKHLAK?", "id": "Berorientasi Pelayanan, Akuntabel, Kompeten." },
  { "en": "Lanjutan Kepanjangan BerAKHLAK?", "id": "Harmonis, Loyal, Adaptif, Kolaboratif." },
  { "en": "Apa Motto ASN (Aparatur Sipil Negara) Saat Ini?", "id": "Bangga Melayani Bangsa." },
  { "en": "Apa Itu Tunjangan Kinerja (Tukin)?", "id": "Tunjangan Berbasis Kinerja ASN." },
  { "en": "Apa Kepanjangan Tukin (Tunjangan Kinerja)?", "id": "Tunjangan Kinerja." },
  { "en": "Apa Itu Tambahan Penghasilan Pegawai (TPP)?", "id": "Tunjangan Kinerja ASN (Aparatur Sipil Negara) Daerah." },
  { "en": "Apa Kepanjangan TPP (Tambahan Penghasilan Pegawai)?", "id": "Tambahan Penghasilan Pegawai." },
  { "en": "Apa Itu Sasaran Kinerja Pegawai (SKP)?", "id": "Kontrak Kinerja Tahunan ASN." },
  { "en": "Apa Kepanjangan SKP (Sasaran Kinerja Pegawai)?", "id": "Sasaran Kinerja Pegawai." },
  { "en": "Apa Itu Gaji Ketiga Belas?", "id": "Penghasilan Tambahan ASN (Tahun Ajaran Baru)." },
  { "en": "Apa Itu Hari Libur Nasional?", "id": "Hari Libur Resmi (Tanggal Merah)." },
  { "en": "Apa Itu Cuti Bersama?", "id": "Hari Libur Tambahan (Jelang Hari Raya)." },
  { "en": "Siapa Yang Menetapkan Cuti Bersama?", "id": "Pemerintah (Surat Keputusan Bersama Tiga Menteri)." },
  { "en": "Apa Itu Tiga Menteri SKB (Surat Keputusan Bersama) Cuti?", "id": "Menteri Agama, Ketenagakerjaan, PANRB." },
  { "en": "Apa Itu Hari Kejepit Nasional (Harpitnas)?", "id": "Hari Kerja Di Antara Libur (Tidak Resmi)." },
  { "en": "Apa Itu Cuti PNS (Pegawai Negeri Sipil)?", "id": "Hak Libur PNS (Pegawai Negeri Sipil) (Tahunan, Sakit)." },
  { "en": "Apa Itu Cuti Tahunan?", "id": "Hak Cuti 12 Hari Kerja." },
  { "en": "Apa Itu Cuti Besar?", "id": "Hak Cuti (PNS 5 Tahun Kerja)." },
  { "en": "Apa Itu Cuti Sakit?", "id": "Hak Cuti Karena Sakit." },
  { "en": "Apa Itu Cuti Melahirkan?", "id": "Hak Cuti Melahirkan (Tiga Bulan)." },
  { "en": "Apa Itu Cuti Alasan Penting?", "id": "Hak Cuti (Keluarga Sakit Keras/Meninggal)." },
  { "en": "Apa Itu Cuti Di Luar Tanggungan Negara (CLTN)?", "id": "Cuti Tanpa Gaji (Alasan Pribadi)." },
  { "en": "Apa Kepanjangan CLTN?", "id": "Cuti Di Luar Tanggungan Negara." },
  { "en": "Apa Itu Idul Fitri?", "id": "Hari Raya Umat Islam (Pasca Ramadhan)." },
  { "en": "Apa Itu Idul Adha?", "id": "Hari Raya Kurban Umat Islam." },
  { "en": "Apa Itu Natal?", "id": "Hari Raya Umat Kristen (Kelahiran Yesus)." },
  { "en": "Apa Itu Nyepi?", "id": "Hari Raya Umat Hindu (Tahun Baru Saka)." },
  { "en": "Apa Itu Waisak?", "id": "Hari Raya Umat Buddha (Trisuci)." },
  { "en": "Apa Itu Imlek?", "id": "Tahun Baru Umat Khonghucu." },
  { "en": "Apa Itu Kenaikan Isa Almasih?", "id": "Hari Raya Umat Kristen (Yesus Naik)." },
  { "en": "Apa Itu Wafat Isa Almasih?", "id": "Hari Raya Umat Kristen (Jumat Agung)." },
  { "en": "Apa Itu Maulid Nabi Muhammad?", "id": "Peringatan Kelahiran Nabi Muhammad SAW." },
  { "en": "Apa Itu Isra Miraj?", "id": "Peringatan Perjalanan Nabi Muhammad SAW." },
  { "en": "Apa Itu Tahun Baru Islam?", "id": "Peringatan Satu Muharram." },
  { "en": "Apa Itu Galungan?", "id": "Hari Raya Umat Hindu (Kemenangan Dharma)." },
  { "en": "Apa Itu Kuningan?", "id": "Hari Raya Umat Hindu (Setelah Galungan)." },
  { "en": "Apa Makna Sumpah Pemuda?", "id": "Satu Tanah Air, Bangsa, Bahasa." },
  { "en": "Apa Itu Cultuurstelsel?", "id": "Sistem Tanam Paksa Era Kolonial Belanda." },
  { "en": "Apa Nama Kabinet Pertama Setelah Proklamasi?", "id": "Kabinet Presidensial." },
  { "en": "Siapa Perdana Menteri Pertama Era Parlementer?", "id": "Sutan Sjahrir." },
  { "en": "Apa Maklumat Yang Mengubah Sistem Pemerintahan (1945)?", "id": "Maklumat Wakil Presiden Nomor X." },
  { "en": "Apa Maklumat Yang Mendorong Lahirnya Partai Politik?", "id": "Maklumat Pemerintah Tanggal 3 November 1945." },
  { "en": "Siapa Pemimpin Agresi Militer Belanda I?", "id": "Letnan Jenderal Simon Spoor." },
  { "en": "Apa Nama Perundingan Yang Menghasilkan KMB (Konferensi Meja Bundar)?", "id": "Perjanjian Roem-Royen." },
  { "en": "Siapa Delegasi Indonesia Di Perjanjian Roem-Royen?", "id": "Mohammad Roem." },
  { "en": "Siapa Delegasi Belanda Di Perjanjian Roem-Royen?", "id": "Herman Van Roijen." },
  { "en": "Siapa Penengah Perjanjian Roem-Royen?", "id": "Merle Cochran (Dari UNCI)." },
  { "en": "Apa Kepanjangan UNCI (United Nations Commission For Indonesia)?", "id": "United Nations Commission For Indonesia." },
  { "en": "Apa Nama Organisasi Pengganti KTN (Komisi Tiga Negara)?", "id": "UNCI (United Nations Commission For Indonesia)." },
  { "en": "Apa Kepanjangan KTN (Komisi Tiga Negara)?", "id": "Komisi Tiga Negara." },
  { "en": "Siapa Anggota KTN (Komisi Tiga Negara)?", "id": "Australia, Belgia, Amerika Serikat." },
  { "en": "Negara Apa Pilihan Indonesia Di KTN?", "id": "Australia (Richard Kirby)." },
  { "en": "Negara Apa Pilihan Belanda Di KTN?", "id": "Belgia (Paul Van Zeeland)." },
  { "en": "Siapa Pimpinan TKR (Tentara Keamanan Rakyat) Pertama?", "id": "Soeprijadi (Tidak Pernah Muncul)." },
  { "en": "Siapa Pimpinan TKR (Tentara Keamanan Rakyat) Pengganti?", "id": "Jenderal Soedirman." },
  { "en": "Bagaimana Jenderal Soedirman Dipilih?", "id": "Melalui Konferensi TKR Yogyakarta." },
  { "en": "Apa Perang Pertama Yang Dipimpin Soedirman?", "id": "Pertempuran Ambarawa." },
  { "en": "Kapan Jenderal Soedirman Dilantik Presiden?", "id": "Tanggal 18 Desember 1945." },
  { "en": "Apa Pangkat Soedirman Saat Dilantik?", "id": "Jenderal Besar." },
  { "en": "Apa Itu Pahlawan Nasional?", "id": "Gelar Kehormatan WNI Berjasa." },
  { "en": "Siapa Yang Memberi Gelar Pahlawan Nasional?", "id": "Presiden Republik Indonesia." },
  { "en": "Apa Nama Dewan Pemberi Pertimbangan Pahlawan Nasional?", "id": "Dewan Gelar, Tanda Jasa, Kehormatan." },
  { "en": "Siapa Pahlawan Wanita Dari Aceh?", "id": "Cut Nyak Dhien, Cut Meutia." },
  { "en": "Siapa Pahlawan Wanita Dari Maluku?", "id": "Martha Christina Tiahahu." },
  { "en": "Siapa Pahlawan Wanita Dari Jawa Tengah?", "id": "R.A. (Raden Ajeng) Kartini." },
  { "en": "Apa Kepanjangan R.A. (Raden Ajeng)?", "id": "Raden Ajeng." },
  { "en": "Siapa Pahlawan Wanita Dari Jawa Barat?", "id": "Dewi Sartika." },
  { "en": "Siapa Pahlawan Proklamator Indonesia?", "id": "Soekarno Dan Mohammad Hatta." },
  { "en": "Siapa Pahlawan Bapak Pendidikan Nasional?", "id": "Ki Hadjar Dewantara." },
  { "en": "Apa Nama Asli Ki Hadjar Dewantara?", "id": "Raden Mas Soewardi Soerjaningrat." },
  { "en": "Apa Nama Sekolah Didirikan Ki Hadjar Dewantara?", "id": "Taman Siswa." },
  { "en": "Apa Semboyan Pendidikan Ki Hadjar Dewantara?", "id": "Tut Wuri Handayani." },
  { "en": "Siapa Pahlawan Penulis Lagu Indonesia Raya?", "id": "W.R. (Wage Rudolf) Supratman." },
  { "en": "Siapa Pahlawan Penjahit Bendera Pusaka?", "id": "Fatmawati Soekarno." },
  { "en": "Siapa Pahlawan Perumus Pancasila (Pidato)?", "id": "Mohammad Yamin, Soepomo, Soekarno." },
  { "en": "Siapa Pahlawan Perumus Teks Proklamasi?", "id": "Soekarno, Hatta, Achmad Soebardjo." },
  { "en": "Siapa Pahlawan Pengetik Teks Proklamasi?", "id": "Sayuti Melik." },
  { "en": "Siapa Pahlawan Pengibar Bendera Pusaka?", "id": "Latief Hendraningrat Dan Suhud." },
  { "en": "Siapa Pahlawan Yang Diculik Ke Rengasdengklok?", "id": "Soekarno Dan Hatta." },
  { "en": "Siapa Tokoh Golongan Tua?", "id": "Soekarno, Hatta, Achmad Soebardjo." },
  { "en": "Siapa Tokoh Golongan Muda?", "id": "Sukarni, Chaerul Saleh, Wikana." },
  { "en": "Siapa Pahlawan Raja Yogyakarta?", "id": "Sultan Hamengkubuwono IX." },
  { "en": "Apa Peran Sultan Hamengkubuwono IX?", "id": "Menyatakan Yogyakarta Bagian RI." },
  { "en": "Siapa Pahlawan Dari Papua?", "id": "Frans Kaisiepo, Silas Papare." },
  { "en": "Frans Kaisiepo Mengusulkan Nama Apa?", "id": "Nama 'Irian' (Ikut Republik Indonesia Anti-Nederland)." },
  { "en": "Apa Jasa Silas Papare?", "id": "Mendirikan Partai Kemerdekaan Indonesia Irian." },
  { "en": "Siapa Pahlawan Julukan Ayam Jantan Dari Timur?", "id": "Sultan Hasanuddin (Makassar)." },
  { "en": "Siapa Pahlawan Pemimpin Perang Padri?", "id": "Tuanku Imam Bonjol (Sumatera Barat)." },
  { "en": "Siapa Pahlawan Pemimpin Perang Diponegoro?", "id": "Pangeran Diponegoro (Jawa)." },
  { "en": "Siapa Pahlawan Pemimpin Perang Bali?", "id": "I Gusti Ketut Jelantik." },
  { "en": "Apa Nama Perang Bali?", "id": "Perang Puputan Jagaraga." },
  { "en": "Siapa Pahlawan Pemimpin Perang Banjar?", "id": "Pangeran Antasari (Kalimantan Selatan)." },
  { "en": "Siapa Pahlawan Pemimpin Perang Aceh?", "id": "Teuku Umar, Cut Nyak Dhien." },
  { "en": "Apa Taktik Perang Teuku Umar?", "id": "Taktik Berpura-pura Menyerah." },
  { "en": "Siapa Pahlawan Pemimpin Perang Maluku?", "id": "Kapitan Pattimura (Thomas Matulessy)." },
  { "en": "Apa Itu Hari Kebangkitan Teknologi Nasional (Hakteknas)?", "id": "Hari Peringatan Kemajuan Teknologi." },
  { "en": "Apa Kepanjangan Hakteknas?", "id": "Hari Kebangkitan Teknologi Nasional." },
  { "en": "Kapan Hakteknas (Hari Kebangkitan Teknologi Nasional) Diperingati?", "id": "Tanggal Sepuluh Agustus." },
  { "en": "Peristiwa Apa Dibalik Hakteknas?", "id": "Penerbangan Pesawat N-250 Gatotkaca (1995)." },
  { "en": "Siapa Tokoh Dibalik Pesawat N-250?", "id": "B.J. (Bacharuddin Jusuf) Habibie." },
  { "en": "Apa Perusahaan Pembuat Pesawat N-250?", "id": "Industri Pesawat Terbang Nusantara (IPTN)." },
  { "en": "Apa Nama IPTN (Industri Pesawat Terbang Nusantara) Saat Ini?", "id": "PT. (Perseroan Terbatas) Dirgantara Indonesia." },
  { "en": "Apa Itu Badan Riset Inovasi Nasional (BRIN)?", "id": "Lembaga Penelitian Pengembangan Nasional." },
  { "en": "Lembaga Apa Yang Dilebur Ke BRIN?", "id": "LIPI, LAPAN, BATAN, BPPT." },
  { "en": "Apa Kepanjangan LAPAN (Lembaga Penerbangan Antariksa Nasional)?", "id": "Lembaga Penerbangan Antariksa Nasional." },
  { "en": "Apa Kepanjangan BATAN (Badan Tenaga Nuklir Nasional)?", "id": "Badan Tenaga Nuklir Nasional." },
  { "en": "Apa Kepanjangan BPPT (Badan Pengkajian Penerapan Teknologi)?", "id": "Badan Pengkajian Penerapan Teknologi." },
  { "en": "Apa Itu Program Langit Biru?", "id": "Program Pengurangan Polusi Udara." },
  { "en": "Apa Itu Uji Emisi Kendaraan?", "id": "Pengukuran Gas Buang Kendaraan." },
  { "en": "Apa Itu Bahan Bakar Ramah Lingkungan?", "id": "Bahan Bakar Rendah Emisi (Euro 4)." },
  { "en": "Apa Itu Indeks Kualitas Udara (AQI)?", "id": "Indikator Pengukuran Polusi Udara." },
  { "en": "Apa Kepanjangan AQI (Air Quality Index)?", "id": "Air Quality Index." },
  { "en": "Apa Itu PM 2.5 (Particulate Matter 2.5)?", "id": "Partikel Polusi Udara Sangat Kecil." },
  { "en": "Apa Bahaya PM 2.5 (Particulate Matter 2.5)?", "id": "Masuk Paru-paru, Gangguan Pernapasan." },
  { "en": "Apa Itu ISPU (Indeks Standar Pencemar Udara)?", "id": "Indeks Pencemaran Udara Indonesia." },
  { "en": "Apa Kepanjangan ISPU?", "id": "Indeks Standar Pencemar Udara." },
  { "en": "Apa Kategori ISPU (Indeks Standar Pencemar Udara) Baik?", "id": "Nol Sampai Lima Puluh." },
  { "en": "Apa Kategori ISPU (Indeks Standar Pencemar Udara) Tidak Sehat?", "id": "Seratus Satu Sampai Seratus Sembilan Puluh Sembilan." },
  { "en": "Apa Itu Gerakan Ciliwung Bersih?", "id": "Gerakan Membersihkan Sungai Ciliwung." },
  { "en": "Apa Itu Bank Sampah?", "id": "Tempat Pengelolaan Sampah (Sistem Tabungan)." },
  { "en": "Apa Itu Prinsip 3R (Reduce, Reuse, Recycle)?", "id": "Reduce (Kurangi), Reuse (Guna Ulang), Recycle (Daur Ulang)." },
  { "en": "Apa Itu Reduce?", "id": "Mengurangi Penggunaan Sampah (Plastik)." },
  { "en": "Apa Itu Reuse?", "id": "Menggunakan Kembali Barang Bekas." },
  { "en": "Apa Itu Recycle?", "id": "Mendaur Ulang Sampah Menjadi Barang Baru." },
  { "en": "Apa Itu Sampah Organik?", "id": "Sampah Mudah Terurai (Sisa Makanan)." },
  { "en": "Apa Itu Sampah Anorganik?", "id": "Sampah Sulit Terurai (Plastik, Kaca)." },
  { "en": "Apa Itu Kompos?", "id": "Pupuk Hasil Penguraian Sampah Organik." },
  { "en": "Apa Itu Biopori?", "id": "Lubang Resapan Air (Resapan Kompos)." },
  { "en": "Apa Itu Tempat Pembuangan Akhir (TPA)?", "id": "Lokasi Pembuangan Akhir Sampah." },
  { "en": "Apa Masalah TPA (Tempat Pembuangan Akhir) Di Indonesia?", "id": "Kelebihan Kapasitas (Overload)." },
  { "en": "Apa Itu Pengelolaan Sampah Menjadi Energi (PSEL)?", "id": "Pembangkit Listrik Tenaga Sampah (PLTSa)." },
  { "en": "Apa Kepanjangan PSEL?", "id": "Pengelolaan Sampah Menjadi Energi Listrik." },
  { "en": "Apa Kepanjangan PLTSa?", "id": "Pembangkit Listrik Tenaga Sampah." },
  { "en": "Apa Itu Ekonomi Sirkular?", "id": "Model Ekonomi (Minim Sampah, Reuse)." },
  { "en": "Apa Itu Nol Sampah (Zero Waste)?", "id": "Gaya Hidup Minim Produksi Sampah." },
  { "en": "Apa Itu Kantong Plastik Sekali Pakai?", "id": "Kantong Plastik Dilarang Di Beberapa Daerah." },
  { "en": "Apa Pengganti Kantong Plastik Sekali Pakai?", "id": "Tas Belanja Guna Ulang (Reusable Bag)." },
  { "en": "Apa Itu Sedotan Plastik?", "id": "Sedotan Dilarang Di Beberapa Restoran." },
  { "en": "Apa Itu Styrofoam?", "id": "Bahan Kemasan Makanan Tidak Ramah Lingkungan." },
  { "en": "Apa Itu Limbah B3 (Bahan Berbahaya Beracun)?", "id": "Limbah Mengandung Zat Berbahaya." },
  { "en": "Apa Kepanjangan B3 (Bahan Berbahaya Beracun)?", "id": "Bahan Berbahaya Dan Beracun." },
  { "en": "Apa Contoh Limbah B3 (Bahan Berbahaya Beracun) Rumah Tangga?", "id": "Baterai Bekas, Lampu Neon Bekas." },
  { "en": "Apa Itu Limbah Medis?", "id": "Limbah Berasal Fasilitas Kesehatan." },
  { "en": "Mengapa Limbah Medis Berbahaya?", "id": "Mengandung Bahan Infeksius." },
  { "en": "Apa Itu Incinerator?", "id": "Alat Pembakar Sampah Suhu Tinggi." },
  { "en": "Apa Itu Landfill?", "id": "Metode Penimbunan Sampah Di Tanah." },
  { "en": "Apa Itu Emisi Karbon?", "id": "Gas Rumah Kaca (CO2) Ke Atmosfer." },
  { "en": "Apa Itu Jejak Karbon (Carbon Footprint)?", "id": "Total Emisi Karbon Aktivitas Individu." },
  { "en": "Bagaimana Cara Mengurangi Jejak Karbon?", "id": "Hemat Energi, Transportasi Umum." },
  { "en": "Apa Itu Energi Baru Terbarukan (EBT)?", "id": "Energi Ramah Lingkungan (Non-Fosil)." },
  { "en": "Apa Kepanjangan EBT (Energi Baru Terbarukan)?", "id": "Energi Baru Terbarukan." },
  { "en": "Apa Contoh Energi Baru Terbarukan (EBT)?", "id": "Tenaga Surya, Air, Angin, Panas Bumi." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Surya (PLTS)?", "id": "Energi Listrik Dari Matahari." },
  { "en": "Apa Kepanjangan PLTS (Pembangkit Listrik Tenaga Surya)?", "id": "Pembangkit Listrik Tenaga Surya." },
  { "en": "Apa Itu Panel Surya?", "id": "Alat Mengubah Cahaya Matahari Listrik." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Air (PLTA)?", "id": "Energi Listrik Dari Aliran Air." },
  { "en": "Apa Kepanjangan PLTA (Pembangkit Listrik Tenaga Air)?", "id": "Pembangkit Listrik Tenaga Air." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Bayu (PLTB)?", "id": "Energi Listrik Dari Angin." },
  { "en": "Apa Kepanjangan PLTB (Pembangkit Listrik Tenaga Bayu)?", "id": "Pembangkit Listrik Tenaga Bayu." },
  { "en": "Dimana Lokasi PLTB (Pembangkit Listrik Tenaga Bayu) Terbesar Indonesia?", "id": "Sidrap (Sulawesi Selatan)." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Panas Bumi (PLTP)?", "id": "Energi Listrik Dari Panas Bumi." },
  { "en": "Apa Kepanjangan PLTP (Pembangkit Listrik Tenaga Panas Bumi)?", "id": "Pembangkit Listrik Tenaga Panas Bumi." },
  { "en": "Apa Itu Geotermal?", "id": "Energi Panas Bumi." },
  { "en": "Apa Keunggulan Energi Panas Bumi?", "id": "Ramah Lingkungan, Stabil 24 Jam." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Nuklir (PLTN)?", "id": "Energi Listrik Dari Reaksi Nuklir." },
  { "en": "Apa Kepanjangan PLTN (Pembangkit Listrik Tenaga Nuklir)?", "id": "Pembangkit Listrik Tenaga Nuklir." },
  { "en": "Apa Keunggulan PLTN (Pembangkit Listrik Tenaga Nuklir)?", "id": "Kapasitas Besar, Emisi Karbon Rendah." },
  { "en": "Apa Risiko PLTN (Pembangkit Listrik Tenaga Nuklir)?", "id": "Risiko Radiasi, Limbah Radioaktif." },
  { "en": "Apa Lembaga Nuklir Indonesia?", "id": "BRIN (Badan Riset Inovasi Nasional) (Ex-Batan)." },
  { "en": "Apa Lembaga Pengawas Nuklir Indonesia?", "id": "Bapeten (Badan Pengawas Tenaga Nuklir)." },
  { "en": "Apa Itu Bioenergi?", "id": "Energi Dari Bahan Organik (Biomassa)." },
  { "en": "Apa Itu Biodiesel?", "id": "Bahan Bakar Diesel (Minyak Nabati)." },
  { "en": "Apa Itu Bioetanol?", "id": "Bahan Bakar Bensin (Tetes Tebu)." },
  { "en": "Apa Itu Bauran Energi Nasional?", "id": "Komposisi Penggunaan Berbagai Sumber Energi." },
  { "en": "Apa Target Bauran EBT (Energi Baru Terbarukan) Indonesia?", "id": "Dua Puluh Tiga Persen (Tahun 2025)." },
  { "en": "Apa Itu Kendaraan Listrik (Electric Vehicle)?", "id": "Kendaraan Berbasis Baterai Listrik." },
  { "en": "Apa Keunggulan Kendaraan Listrik?", "id": "Bebas Emisi Gas Buang." },
  { "en": "Apa Tantangan Kendaraan Listrik?", "id": "Harga Mahal, Infrastruktur Pengisian." },
  { "en": "Apa Itu Stasiun Pengisian Kendaraan Listrik Umum (SPKLU)?", "id": "Tempat Pengisian Baterai Mobil Listrik." },
  { "en": "Apa Itu Stasiun Penukaran Baterai Kendaraan Listrik Umum (SPBKLU)?", "id": "Tempat Tukar Baterai Motor Listrik." },
  { "en": "Apa Kepanjangan SPBKLU?", "id": "Stasiun Penukaran Baterai Kendaraan Listrik Umum." },
  { "en": "Apa Itu Subsidi Kendaraan Listrik?", "id": "Bantuan Pemerintah Pembelian Kendaraan Listrik." },
  { "en": "Apa Itu Industri Baterai Listrik?", "id": "Industri Pengolahan Nikel Menjadi Baterai." },
  { "en": "Indonesia Pemasok Nikel Terbesar Dunia?", "id": "Ya, Indonesia Pemasok Terbesar Dunia." },
  { "en": "Apa Itu Konservasi Energi?", "id": "Upaya Penghematan Penggunaan Energi." },
  { "en": "Apa Contoh Konservasi Energi?", "id": "Mematikan Lampu, Menggunakan Transportasi Umum." },
  { "en": "Apa Itu Label Hemat Energi?", "id": "Tanda Efisiensi Energi (Produk Elektronik)." },
  { "en": "Apa Itu Lampu LED (Light Emitting Diode)?", "id": "Lampu Hemat Energi." },
  { "en": "Apa Kepanjangan LED (Light Emitting Diode)?", "id": "Light Emitting Diode." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Uap (PLTU)?", "id": "Pembangkit Listrik (Bahan Bakar Batu Bara)." },
  { "en": "Apa Kepanjangan PLTU (Pembangkit Listrik Tenaga Uap)?", "id": "Pembangkit Listrik Tenaga Uap." },
  { "en": "Apa Dampak Negatif PLTU (Pembangkit Listrik Tenaga Uap)?", "id": "Polusi Udara (Emisi Karbon)." },
  { "en": "Apa Itu Pensiun Dini PLTU (Pembangkit Listrik Tenaga Uap)?", "id": "Rencana Penghentian Operasi PLTU (Transisi Energi)." },
  { "en": "Apa Itu Co-Firing PLTU?", "id": "Campuran Batu Bara Dengan Biomassa." },
  { "en": "Apa Itu Just Energy Transition Partnership (JETP)?", "id": "Kemitraan Pendanaan Transisi Energi." },
  { "en": "Apa Kepanjangan JETP?", "id": "Just Energy Transition Partnership." },
  { "en": "Apa Itu Perubahan Iklim (Climate Change)?", "id": "Perubahan Pola Cuaca Global." },
  { "en": "Apa Penyebab Utama Perubahan Iklim?", "id": "Pemanasan Global Akibat Emisi Karbon." },
  { "en": "Apa Itu Pemanasan Global?", "id": "Peningkatan Suhu Rata-rata Atmosfer Bumi." },
  { "en": "Apa Itu Gas Rumah Kaca?", "id": "Gas Penahan Panas (CO2, Metana)." },
  { "en": "Apa Itu Efek Rumah Kaca?", "id": "Proses Terperangkapnya Panas Di Atmosfer." },
  { "en": "Apa Dampak Perubahan Iklim?", "id": "Kenaikan Permukaan Air Laut, Cuaca Ekstrem." },
  { "en": "Apa Itu Banjir Rob?", "id": "Banjir Akibat Pasang Air Laut." },
  { "en": "Apa Itu Abrasi?", "id": "Pengikisan Wilayah Pantai Oleh Ombak." },
  { "en": "Apa Itu Hutan Mangrove (Bakau)?", "id": "Hutan Di Wilayah Pesisir." },
  { "en": "Apa Fungsi Hutan Mangrove?", "id": "Mencegah Abrasi, Menahan Tsunami." },
  { "en": "Apa Itu Terumbu Karang?", "id": "Ekosistem Bawah Laut (Rumah Ikan)." },
  { "en": "Apa Itu Pemutihan Karang (Coral Bleaching)?", "id": "Kematian Terumbu Karang (Suhu Naik)." },
  { "en": "Apa Itu El Nino?", "id": "Fenomena Pemanasan Suhu Laut Pasifik." },
  { "en": "Apa Dampak El Nino Di Indonesia?", "id": "Kekeringan Panjang (Musim Kemarau)." },
  { "en": "Apa Itu La Nina?", "id": "Fenomena Pendinginan Suhu Laut Pasifik." },
  { "en": "Apa Dampak La Nina Di Indonesia?", "id": "Curah Hujan Tinggi (Musim Hujan)." },
  { "en": "Apa Itu Badan Meteorologi Klimatologi Geofisika (BMKG)?", "id": "Lembaga Pemantau Cuaca Iklim Gempa." },
  { "en": "Apa Kepanjangan BMKG?", "id": "Badan Meteorologi Klimatologi Geofisika." },
  { "en": "Apa Itu Prakiraan Cuaca?", "id": "Prediksi Kondisi Cuaca Harian." },
  { "en": "Apa Itu Peringatan Dini Cuaca?", "id": "Informasi Potensi Cuaca Ekstrem." },
  { "en": "Apa Itu Gempa Bumi?", "id": "Getaran Permukaan Bumi." },
  { "en": "Apa Itu Gempa Tektonik?", "id": "Gempa Akibat Pergeseran Lempeng Bumi." },
  { "en": "Apa Itu Gempa Vulkanik?", "id": "Gempa Akibat Aktivitas Gunung Berapi." },
  { "en": "Apa Itu Skala Richter (SR)?", "id": "Satuan Ukuran Kekuatan Gempa (Magnitudo)." },
  { "en": "Apa Itu Skala Mercalli (MMI)?", "id": "Satuan Ukuran Dampak Getaran Gempa." },
  { "en": "Apa Kepanjangan MMI (Modified Mercalli Intensity)?", "id": "Modified Mercalli Intensity." },
  { "en": "Apa Itu Sesar (Patahan)?", "id": "Zona Retakan Lempeng Bumi." },
  { "en": "Apa Itu Cincin Api Pasifik (Ring Of Fire)?", "id": "Jalur Rawan Gempa Gunung Berapi." },
  { "en": "Apa Itu Tsunami?", "id": "Gelombang Laut Besar Akibat Gempa." },
  { "en": "Apa Itu Sirene Peringatan Tsunami?", "id": "Alarm Tanda Peringatan Tsunami." },
  { "en": "Apa Itu Jalur Evakuasi?", "id": "Jalur Aman Menuju Titik Kumpul." },
  { "en": "Apa Itu Titik Kumpul?", "id": "Lokasi Aman Berkumpul Saat Bencana." },
  { "en": "Apa Itu Mitigasi Bencana?", "id": "Upaya Pengurangan Risiko Bencana." },
  { "en": "Apa Itu Bangunan Tahan Gempa?", "id": "Struktur Bangunan Dirancang Tahan Gempa." },
  { "en": "Apa Itu Gunung Berapi?", "id": "Gunung Berapi Aktif (Magma)." },
  { "en": "Apa Status Gunung Berapi (Level 1)?", "id": "Level I (Normal)." },
  { "en": "Apa Status Gunung Berapi (Level 2)?", "id": "Level II (Waspada)." },
  { "en": "Apa Status Gunung Berapi (Level 3)?", "id": "Level III (Siaga)." },
  { "en": "Apa Status Gunung Berapi (Level 4)?", "id": "Level IV (Awas)." },
  { "en": "Siapa Yang Mengawasi Gunung Berapi?", "id": "PVMBG (Pusat Vulkanologi Mitigasi Bencana Geologi)." },
  { "en": "Apa Kepanjangan PVMBG?", "id": "Pusat Vulkanologi Mitigasi Bencana Geologi." },
  { "en": "Apa Itu Magma?", "id": "Batuan Cair Panas Perut Bumi." },
  { "en": "Apa Itu Lava?", "id": "Magma Yang Keluar Ke Permukaan." },
  { "en": "Apa Itu Lahar?", "id": "Lumpur Vulkanik (Aliran Magma Air)." },
  { "en": "Apa Itu Awan Panas (Wedhus Gembel)?", "id": "Gas Debu Vulkanik Suhu Tinggi." },
  { "en": "Apa Itu Hujan Abu Vulkanik?", "id": "Debu Vulkanik Jatuh Dari Udara." },
  { "en": "Apa Gunung Merapi?", "id": "Gunung Berapi Aktif (Jawa Tengah/DIY)." },
  { "en": "Apa Gunung Sinabung?", "id": "Gunung Berapi Aktif (Sumatera Utara)." },
  { "en": "Apa Gunung Semeru?", "id": "Gunung Tertinggi Di Pulau Jawa." },
  { "en": "Apa Gunung Kerinci?", "id": "Gunung Tertinggi Di Sumatera." },
  { "en": "Apa Gunung Rinjani?", "id": "Gunung Berapi Di Lombok (NTB)." },
  { "en": "Apa Gunung Bromo?", "id": "Gunung Berapi Tujuan Wisata (Jawa Timur)." },
  { "en": "Apa Itu Kaldera?", "id": "Kawah Sangat Besar (Bekas Letusan)." },
  { "en": "Apa Contoh Kaldera Terkenal?", "id": "Kaldera Toba (Danau Toba)." },
  { "en": "Apa Itu Sesar Aktif?", "id": "Patahan Lempeng Bumi Aktif Bergerak." },
  { "en": "Apa Itu Sesar Lembang?", "id": "Patahan Aktif Di Dekat Bandung." },
  { "en": "Apa Itu Sesar Opak?", "id": "Patahan Aktif Di Dekat Yogyakarta." },
  { "en": "Apa Itu Sesar Semangko (Sumatera)?", "id": "Patahan Aktif Sepanjang Pulau Sumatera." },
  { "en": "Apa Itu Tanah Longsor?", "id": "Pergerakan Tanah (Lereng Curam)." },
  { "en": "Apa Penyebab Tanah Longsor?", "id": "Curah Hujan Tinggi, Kelerengan, Erosi." },
  { "en": "Apa Itu Banjir?", "id": "Luapan Air Berlebihan Merendam Daratan." },
  { "en": "Apa Penyebab Banjir?", "id": "Hujan Deras, Drainase Buruk, Sungai Dangkal." },
  { "en": "Apa Itu Banjir Bandang?", "id": "Banjir Besar Cepat (Dari Hulu)." },
  { "en": "Apa Itu Drainase?", "id": "Sistem Saluran Pembuangan Air." },
  { "en": "Apa Itu Daerah Aliran Sungai (DAS)?", "id": "Wilayah Aliran Air Ke Sungai." },
  { "en": "Apa Kepanjangan DAS (Daerah Aliran Sungai)?", "id": "Daerah Aliran Sungai." },
  { "en": "Apa Itu Normalisasi Sungai?", "id": "Pelurusan Pelebaran Sungai (Atasi Banjir)." },
  { "en": "Apa Itu Naturalisasi Sungai?", "id": "Pengembalian Fungsi Sungai (Lebih Alami)." },
  { "en": "Apa Itu Sumur Resapan?", "id": "Sumur Resapan Air Hujan." },
  { "en": "Apa Fungsi Sumur Resapan?", "id": "Mengurangi Limpasan Air, Menambah Air Tanah." },
  { "en": "Apa Itu Polder?", "id": "Sistem Pengendalian Banjir (Pompa Waduk)." },
  { "en": "Apa Itu Waduk?", "id": "Danau Buatan Penampung Air." },
  { "en": "Apa Fungsi Waduk?", "id": "Pengendali Banjir, Irigasi, PLTA (Pembangkit Listrik Tenaga Air)." },
  { "en": "Apa Itu Bendung?", "id": "Bangunan Peninggi Muka Air (Irigasi)." },
  { "en": "Apa Perbedaan Bendungan Dan Bendung?", "id": "Bendungan Menampung Air, Bendung Meninggikan Air." },
  { "en": "Apa Itu Embung?", "id": "Waduk Kecil Penampung Air Hujan." },
  { "en": "Apa Itu Kekeringan?", "id": "Kekurangan Pasokan Air." },
  { "en": "Apa Itu Kekeringan Meteorologis?", "id": "Curah Hujan Di Bawah Normal." },
  { "en": "Apa Itu Kekeringan Hidrologis?", "id": "Sumber Air (Sungai Danau) Kering." },
  { "en": "Apa Itu Kekeringan Agronomis?", "id": "Kekurangan Air Untuk Tanaman." },
  { "en": "Apa Itu El Nino?", "id": "Fenomena Pemanasan Laut Pasifik (Kering)." },
  { "en": "Apa Itu La Nina?", "id": "Fenomena Pendinginan Laut Pasifik (Basah)." },
  { "en": "Apa Itu Badan Restorasi Gambut (BRG)?", "id": "Lembaga Pemulihan Ekosistem Gambut." },
  { "en": "Apa Kepanjangan BRG?", "id": "Badan Restorasi Gambut." },
  { "en": "Apa Itu Lahan Gambut?", "id": "Lahan Basah (Timbunan Bahan Organik)." },
  { "en": "Mengapa Lahan Gambut Harus Dijaga?", "id": "Penyimpan Karbon, Rawan Terbakar." },
  { "en": "Apa Penyebab Utama Karhutla (Kebakaran Hutan Lahan)?", "id": "Pembukaan Lahan (Pembakaran Sengaja)." },
  { "en": "Apa Dampak Karhutla (Kebakaran Hutan Lahan)?", "id": "Bencana Asap, Kerusakan Lingkungan." },
  { "en": "Apa Itu Bencana Asap?", "id": "Polusi Udara Akibat Karhutla (Kebakaran Hutan Lahan)." },
  { "en": "Apa Itu ISPA (Infeksi Saluran Pernapasan Akut)?", "id": "Penyakit Akibat Bencana Asap." },
  { "en": "Apa Kepanjangan ISPA?", "id": "Infeksi Saluran Pernapasan Akut." },
  { "en": "Apa Itu Hotspot (Titik Panas)?", "id": "Indikasi Titik Kebakaran (Pantauan Satelit)." },
  { "en": "Apa Itu Water Bombing?", "id": "Pemadaman Api (Helikopter)." },
  { "en": "Apa Itu Teknologi Modifikasi Cuaca (TMC)?", "id": "Upaya Membuat Hujan Buatan." },
  { "en": "Apa Kepanjangan TMC (Teknologi Modifikasi Cuaca)?", "id": "Teknologi Modifikasi Cuaca." },
  { "en": "Apa Itu Moratorium Izin Lahan Gambut?", "id": "Penundaan Izin Pembukaan Lahan Gambut." },
  { "en": "Apa Itu Moratorium Izin Kelapa Sawit?", "id": "Penundaan Izin Perkebunan Kelapa Sawit." },
  { "en": "Apa Itu Indonesia Sustainable Palm Oil (ISPO)?", "id": "Sertifikasi Sawit Berkelanjutan Indonesia." },
  { "en": "Apa Kepanjangan ISPO?", "id": "Indonesia Sustainable Palm Oil." },
  { "en": "Apa Itu Roundtable Sustainable Palm Oil (RSPO)?", "id": "Sertifikasi Sawit Berkelanjutan Global." },
  { "en": "Apa Kepanjangan RSPO?", "id": "Roundtable Sustainable Palm Oil." },
  { "en": "Apa Itu Kampanye Hitam Sawit?", "id": "Kampanye Negatif Terhadap Industri Sawit." },
  { "en": "Apa Itu Produk Turunan Kelapa Sawit?", "id": "Minyak Goreng, Margarin, Kosmetik, Biodiesel." },
  { "en": "Apa Itu CPO (Crude Palm Oil)?", "id": "Minyak Sawit Mentah." },
  { "en": "Apa Itu Batu Bara?", "id": "Sumber Energi Fosil." },
  { "en": "Apa Pemanfaatan Utama Batu Bara?", "id": "Bahan Bakar PLTU (Pembangkit Listrik Tenaga Uap)." },
  { "en": "Apa Itu Domestic Market Obligation (DMO)?", "id": "Kewajiban Pasokan Domestik (Batu Bara)." },
  { "en": "Apa Kepanjangan DMO (Domestic Market Obligation)?", "id": "Domestic Market Obligation." },
  { "en": "Apa Itu Gas Alam?", "id": "Sumber Energi Fosil Gas." },
  { "en": "Apa Itu Gas Alam Cair (LNG)?", "id": "Gas Alam Dicairkan (Transportasi)." },
  { "en": "Apa Kepanjangan LNG (Liquefied Natural Gas)?", "id": "Liquefied Natural Gas." },
  { "en": "Apa Itu Jaringan Gas Kota (Jargas)?", "id": "Jaringan Pipa Gas Ke Rumah Tangga." },
  { "en": "Apa Kepanjangan Jargas?", "id": "Jaringan Gas Kota." },
  { "en": "Apa Itu Program Konversi Minyak Tanah Gas?", "id": "Penggantian Minyak Tanah Ke LPG." },
  { "en": "Apa Itu LPG (Liquefied Petroleum Gas)?", "id": "Gas Perminyakan Cair (Memasak)." },
  { "en": "Apa Itu Tabung LPG (Liquefied Petroleum Gas) 3 Kg?", "id": "Tabung Gas Bersubsidi (Melon)." },
  { "en": "Apa Itu Tabung LPG (Liquefied Petroleum Gas) Non-Subsidi?", "id": "Tabung Gas (Biru, Pink)." },
  { "en": "Apa Itu Zona Ekonomi Eksklusif (ZEE)?", "id": "Batas Wilayah Laut Ekonomi (200 Mil)." },
  { "en": "Apa Itu Natuna Utara?", "id": "Wilayah ZEE (Zona Ekonomi Eksklusif) Indonesia (Sengketa Klaim)." },
  { "en": "Apa Itu Nine Dash Line (Sembilan Garis Putus)?", "id": "Klaim Sepihak Tiongkok Laut Cina Selatan." },
  { "en": "Apa Sikap Indonesia Terhadap Nine Dash Line?", "id": "Menolak, Tidak Mengakui (Berdasarkan UNCLOS)." },
  { "en": "Apa Kebijakan Penenggelaman Kapal?", "id": "Hukuman Kapal Pencuri Ikan (Era Susi)." },
  { "en": "Apa Itu Kementerian Kelautan Perikanan (KKP)?", "id": "Kementerian Urusan Laut Perikanan." },
  { "en": "Apa Kepanjangan KKP (Kementerian Kelautan Perikanan)?", "id": "Kementerian Kelautan Perikanan." },
  { "en": "Apa Itu Peta?", "id": "Gambaran Permukaan Bumi." },
  { "en": "Apa Itu Atlas?", "id": "Kumpulan Peta Dibukukan." },
  { "en": "Apa Itu Globe?", "id": "Tiruan Bola Bumi." },
  { "en": "Apa Itu Garis Lintang?", "id": "Garis Khayal Horizontal Bumi." },
  { "en": "Apa Itu Garis Bujur?", "id": "Garis Khayal Vertikal Bumi." },
  { "en": "Apa Garis Lintang Nol Derajat?", "id": "Garis Khatulistiwa (Ekuator)." },
  { "en": "Apa Garis Bujur Nol Derajat?", "id": "Garis Meridian Greenwich." },
  { "en": "Apa Itu Letak Geografis Indonesia?", "id": "Antara Dua Benua Dua Samudra." },
  { "en": "Apa Dua Benua Itu?", "id": "Benua Asia Dan Australia." },
  { "en": "Apa Dua Samudra Itu?", "id": "Samudra Hindia Dan Pasifik." },
  { "en": "Apa Keuntungan Letak Geografis Indonesia?", "id": "Jalur Perdagangan Dunia Strategis." },
  { "en": "Apa Kerugian Letak Geografis Indonesia?", "id": "Rawan Penyelundupan Bencana." },
  { "en": "Apa Itu Letak Astronomis Indonesia?", "id": "Antara 6 LU - 11 LS." },
  { "en": "Apa Kepanjangan LU (Lintang Utara)?", "id": "Lintang Utara." },
  { "en": "Apa Kepanjangan LS (Lintang Selatan)?", "id": "Lintang Selatan." },
  { "en": "Apa Akibat Letak Astronomis Indonesia?", "id": "Memiliki Iklim Tropis." },
  { "en": "Apa Itu Letak Geologis Indonesia?", "id": "Pertemuan Tiga Lempeng Tektonik." },
  { "en": "Apa Tiga Lempeng Tektonik Itu?", "id": "Lempeng Eurasia, Indo-Australia, Pasifik." },
  { "en": "Apa Akibat Letak Geologis Indonesia?", "id": "Rawan Gempa Gunung Berapi." },
  { "en": "Apa Itu Cincin Api Pasifik?", "id": "Jalur Rawan Gempa Vulkanik." },
  { "en": "Apa Itu Flora?", "id": "Dunia Tumbuhan." },
  { "en": "Apa Itu Fauna?", "id": "Dunia Hewan (Satwa)." },
  { "en": "Apa Garis Pemisah Fauna Indonesia Barat Tengah?", "id": "Garis Wallace." },
  { "en": "Apa Garis Pemisah Fauna Indonesia Tengah Timur?", "id": "Garis Weber." },
  { "en": "Apa Tipe Fauna Indonesia Barat?", "id": "Tipe Asiatis (Gajah, Harimau)." },
  { "en": "Apa Tipe Fauna Indonesia Timur?", "id": "Tipe Australis (Kanguru, Kuskus)." },
  { "en": "Apa Tipe Fauna Indonesia Tengah?", "id": "Tipe Peralihan (Anoa, Komodo)." },
  { "en": "Apa Itu Komodo?", "id": "Kadal Raksasa Endemik Indonesia." },
  { "en": "Dimana Habitat Asli Komodo?", "id": "Taman Nasional Komodo (NTT)." },
  { "en": "Apa Itu Anoa?", "id": "Hewan Endemik Sulawesi (Mirip Kerbau)." },
  { "en": "Apa Itu Burung Cenderawasih?", "id": "Burung Endemik Papua." },
  { "en": "Apa Itu Badak Bercula Satu?", "id": "Hewan Endemik Ujung Kulon (Jawa)." },
  { "en": "Apa Itu Orang Utan?", "id": "Kera Besar Endemik Kalimantan Sumatera." },
  { "en": "Apa Itu Harimau Sumatera?", "id": "Harimau Endemik Pulau Sumatera." },
  { "en": "Apa Itu Bunga Bangkai Raksasa?", "id": "Amorphophallus Titanum (Endemik Sumatera)." },
  { "en": "Apa Itu Bunga Padma Raksasa?", "id": "Rafflesia Arnoldii (Puspa Langka)." },
  { "en": "Apa Itu Konservasi?", "id": "Upaya Pelestarian Perlindungan." },
  { "en": "Apa Itu Konservasi In Situ?", "id": "Pelestarian Di Habitat Asli." },
  { "en": "Apa Contoh Konservasi In Situ?", "id": "Cagar Alam, Taman Nasional." },
  { "en": "Apa Itu Konservasi Ex Situ?", "id": "Pelestarian Di Luar Habitat Asli." },
  { "en": "Apa Contoh Konservasi Ex Situ?", "id": "Kebun Binatang, Kebun Raya." },
  { "en": "Apa Itu Cagar Alam?", "id": "Kawasan Konservasi (Ilmu Pengetahuan)." },
  { "en": "Apa Itu Suaka Margasatwa?", "id": "Kawasan Konservasi (Satwa Langka)." },
  { "en": "Apa Itu Taman Nasional?", "id": "Kawasan Konservasi (Ekosistem, Wisata)." },
  { "en": "Apa Itu Taman Hutan Raya (Tahura)?", "id": "Kawasan Konservasi Koleksi Tumbuhan Satwa." },
  { "en": "Apa Kepanjangan Tahura?", "id": "Taman Hutan Raya." },
  { "en": "Apa Itu Taman Wisata Alam (TWA)?", "id": "Kawasan Konservasi (Pariwisata Rekreasi)." },
  { "en": "Apa Kepanjangan TWA (Taman Wisata Alam)?", "id": "Taman Wisata Alam." },
  { "en": "Apa Itu Kebun Binatang?", "id": "Tempat Konservasi Ex Situ Satwa." },
  { "en": "Apa Itu Kebun Raya?", "id": "Tempat Konservasi Ex Situ Tumbuhan." },
  { "en": "Apa Kebun Raya Tertua Indonesia?", "id": "Kebun Raya Bogor." },
  { "en": "Siapa Pendiri Kebun Raya Bogor?", "id": "Stamford Raffles (Istri)." },
  { "en": "Apa Itu Pelabuhan Udara?", "id": "Bandar Udara (Bandara)." },
  { "en": "Apa Bandara Internasional Di Jakarta?", "id": "Soekarno-Hatta (Cengkareng)." },
  { "en": "Apa Bandara Internasional Di Bali?", "id": "I Gusti Ngurah Rai (Denpasar)." },
  { "en": "Apa Bandara Internasional Di Surabaya?", "id": "Juanda (Sidoarjo)." },
  { "en": "Apa Bandara Internasional Di Medan?", "id": "Kualanamu (Deli Serdang)." },
  { "en": "Apa Bandara Internasional Di Yogyakarta?", "id": "Yogyakarta International Airport (YIA)." },
  { "en": "Apa Kepanjangan YIA (Yogyakarta International Airport)?", "id": "Yogyakarta International Airport." },
  { "en": "Apa Bandara Internasional Di Makassar?", "id": "Sultan Hasanuddin." },
  { "en": "Apa Maskapai Penerbangan Nasional Indonesia?", "id": "Garuda Indonesia." },
  { "en": "Apa Anak Usaha Garuda Indonesia (Low Cost)?", "id": "Citilink." },
  { "en": "Apa Itu Maskapai Berbiaya Murah (LCC)?", "id": "Maskapai Penerbangan Tarif Murah." },
  { "en": "Apa Kepanjangan LCC (Low Cost Carrier)?", "id": "Low Cost Carrier." },
  { "en": "Apa Itu Pelabuhan Laut?", "id": "Tempat Berlabuh Kapal Laut." },
  { "en": "Apa Pelabuhan Peti Kemas Utama Jakarta?", "id": "Pelabuhan Tanjung Priok." },
  { "en": "Apa Pelabuhan Penyeberangan Jawa-Sumatera?", "id": "Merak (Jawa) Dan Bakauheni (Sumatera)." },
  { "en": "Apa Pelabuhan Penyeberangan Jawa-Bali?", "id": "Ketapang (Jawa) Dan Gilimanuk (Bali)." },
  { "en": "Apa Itu PT. (Perseroan Terbatas) Pelni?", "id": "BUMN (Badan Usaha Milik Negara) Operator Kapal Penumpang." },
  { "en": "Apa Itu PT. (Perseroan Terbatas) ASDP?", "id": "BUMN (Badan Usaha Milik Negara) Operator Kapal Feri." },
  { "en": "Apa Kepanjangan ASDP?", "id": "Angkutan Sungai, Danau, Penyeberangan." },
  { "en": "Apa Itu Industri Galangan Kapal?", "id": "Industri Pembuatan Perbaikan Kapal." },
  { "en": "Apa BUMN (Badan Usaha Milik Negara) Galangan Kapal?", "id": "PT. (Perseroan Terbatas) PAL Indonesia." },
  { "en": "Apa Kepanjangan PAL (Penataran Angkatan Laut)?", "id": "Penataran Angkatan Laut." },
  { "en": "Apa Itu Kereta Api Indonesia (KAI)?", "id": "BUMN (Badan Usaha Milik Negara) Operator Kereta Api." },
  { "en": "Apa Itu Kereta Rel Listrik (KRL) Commuter Line?", "id": "Kereta Api Listrik Jabodetabek." },
  { "en": "Apa Itu Kereta Cepat Jakarta-Bandung?", "id": "Whoosh." },
  { "en": "Apa Kereta Api Jarak Jauh (Eksekutif)?", "id": "Argo Bromo, Argo Lawu." },
  { "en": "Apa Itu Moda Raya Terpadu (MRT)?", "id": "Sistem Kereta Api Cepat Perkotaan." },
  { "en": "Apa Kepanjangan MRT (Moda Raya Terpadu)?", "id": "Moda Raya Terpadu." },
  { "en": "Apa Itu Lintas Rel Terpadu (LRT)?", "id": "Sistem Kereta Api Ringan Perkotaan." },
  { "en": "Apa Kepanjangan LRT (Lintas Rel Terpadu)?", "id": "Lintas Rel Terpadu." },
  { "en": "Apa Itu Jalan Tol Trans Jawa?", "id": "Jalan Tol Sambung (Merak - Probolinggo)." },
  { "en": "Apa Itu Jalan Tol Trans Sumatera?", "id": "Proyek Jalan Tol Sambung (Sumatera)." },
  { "en": "Siapa Operator Jalan Tol Terbesar?", "id": "PT. (Perseroan Terbatas) Jasa Marga (BUMN)." },
  { "en": "Apa Itu Kartu Tol Elektronik (E-Toll)?", "id": "Kartu Pembayaran Tol Non Tunai." },
  { "en": "Apa Itu TransJakarta?", "id": "Sistem Transportasi Bus Cepat (Busway)." },
  { "en": "Apa Itu Halte?", "id": "Tempat Pemberhentian Bus." },
  { "en": "Apa Itu Damri?", "id": "BUMN (Badan Usaha Milik Negara) Operator Bus (Antar Kota, Bandara)." },
  { "en": "Apa Kepanjangan Damri?", "id": "Djawatan Angkoetan Motor Repoeblik Indonesia." },
  { "en": "Apa Itu Ojek Online (Ojol)?", "id": "Transportasi Motor Berbasis Aplikasi." },
  { "en": "Apa Itu Taksi Online?", "id": "Taksi Berbasis Aplikasi." },
  { "en": "Apa Itu Kir Kendaraan?", "id": "Uji Kelayakan Kendaraan Angkutan Umum." },
  { "en": "Siapa Yang Melakukan Uji Kir?", "id": "Dinas Perhubungan (Dishub)." },
  { "en": "Apa Kepanjangan Dishub?", "id": "Dinas Perhubungan." },
  { "en": "Apa Itu Rambu Lalu Lintas?", "id": "Papan Tanda Pengatur Lalu Lintas." },
  { "en": "Apa Arti Rambu Warna Kuning?", "id": "Rambu Peringatan (Hati-hati)." },
  { "en": "Apa Arti Rambu Warna Merah?", "id": "Rambu Larangan (Dilarang)." },
  { "en": "Apa Arti Rambu Warna Biru?", "id": "Rambu Perintah Wajib." },
  { "en": "Apa Arti Rambu Warna Hijau?", "id": "Rambu Petunjuk Arah." },
  { "en": "Apa Itu Ganjil Genap?", "id": "Aturan Pembatasan Kendaraan (Nomor Plat)." },
  { "en": "Dimana Aturan Ganjil Genap Berlaku?", "id": "Di Wilayah Jakarta (Jalan Tertentu)." },
  { "en": "Apa Itu Car Free Day (CFD)?", "id": "Hari Bebas Kendaraan Bermotor." },
  { "en": "Apa Kepanjangan CFD (Car Free Day)?", "id": "Car Free Day." },
  { "en": "Kapan CFD (Car Free Day) Jakarta Biasanya?", "id": "Setiap Hari Minggu Pagi." },
  { "en": "Apa Itu Sepeda Motor?", "id": "Kendaraan Roda Dua Bermesin." },
  { "en": "Apa Itu Helm?", "id": "Pelindung Kepala Wajib Pengendara Motor." },
  { "en": "Apa Itu Sabuk Pengaman?", "id": "Pelindung Wajib Pengemudi Mobil." },
  { "en": "Apa Itu SIM (Surat Izin Mengemudi) C?", "id": "Izin Mengemudi Sepeda Motor." },
  { "en": "Apa Itu SIM (Surat Izin Mengemudi) A?", "id": "Izin Mengemudi Mobil Pribadi." },
  { "en": "Apa Itu SIM (Surat Izin Mengemudi) B1?", "id": "Izin Mengemudi Mobil Barang/Bus." },
  { "en": "Apa Itu STNK (Surat Tanda Nomor Kendaraan)?", "id": "Surat Tanda Nomor Kendaraan." },
  { "en": "Apa Itu BPKB (Buku Pemilik Kendaraan Bermotor)?", "id": "Buku Pemilik Kendaraan Bermotor." },
  { "en": "Apa Itu Plat Nomor?", "id": "Tanda Nomor Registrasi Kendaraan." },
  { "en": "Apa Arti Plat Nomor (B)?", "id": "Kendaraan Wilayah Jakarta." },
  { "en": "Apa Arti Plat Nomor (D)?", "id": "Kendaraan Wilayah Bandung." },
  { "en": "Apa Arti Plat Nomor (L)?", "id": "Kendaraan Wilayah Surabaya." },
  { "en": "Apa Arti Plat Nomor (KB)?", "id": "Kendaraan Wilayah Kalimantan Barat." },
  { "en": "Apa Arti Plat Nomor (KT)?", "id": "Kendaraan Wilayah Kalimantan Timur." },
  { "en": "Apa Arti Plat Nomor (DA)?", "id": "Kendaraan Wilayah Kalimantan Selatan." },
  { "en": "Apa Arti Plat Nomor (DE)?", "id": "Kendaraan Wilayah Maluku." },
  { "en": "Apa Arti Plat Nomor (DG)?", "id": "Kendaraan Wilayah Maluku Utara." },
  { "en": "Apa Arti Plat Nomor (DS)?", "id": "Kendaraan Wilayah Papua (Sebelum Pemekaran)." },
  { "en": "Apa Arti Plat Nomor (DK)?", "id": "Kendaraan Wilayah Bali." },
  { "en": "Apa Arti Plat Nomor (BK)?", "id": "Kendaraan Wilayah Sumatera Utara." },
  { "en": "Apa Arti Plat Nomor (BL)?", "id": "Kendaraan Wilayah Aceh." },
  { "en": "Apa Arti Plat Nomor (BM)?", "id": "Kendaraan Wilayah Riau." },
  { "en": "Apa Arti Plat Nomor (BA)?", "id": "Kendaraan Wilayah Sumatera Barat." },
  { "en": "Apa Arti Plat Nomor (BG)?", "id": "Kendaraan Wilayah Sumatera Selatan." },
  { "en": "Apa Arti Plat Nomor (DD)?", "id": "Kendaraan Wilayah Sulawesi Selatan (Makassar)." },
  { "en": "Apa Arti Plat Nomor (DB)?", "id": "Kendaraan Wilayah Sulawesi Utara (Manado)." },
  { "en": "Apa Arti Plat Nomor (H)?", "id": "Kendaraan Wilayah Semarang." },
  { "en": "Apa Arti Plat Nomor (AB)?", "id": "Kendaraan Wilayah Yogyakarta." },
  { "en": "Apa Arti Plat Nomor (EA)?", "id": "Kendaraan Wilayah Nusa Tenggara Barat (Sumbawa)." },
  { "en": "Apa Arti Plat Nomor (ED)?", "id": "Kendaraan Wilayah Nusa Tenggara Timur (Sumba)." },
  { "en": "Apa Arti Plat Nomor Putih?", "id": "Kendaraan Milik Pribadi." },
  { "en": "Apa Arti Plat Nomor Kuning?", "id": "Kendaraan Angkutan Umum." },
  { "en": "Apa Arti Plat Nomor Merah?", "id": "Kendaraan Dinas Pemerintah (Instansi)." },
  { "en": "Apa Arti Plat Nomor Hijau?", "id": "Kendaraan Kawasan Perdagangan Bebas." },
  { "en": "Apa Itu Kantor Pos?", "id": "Tempat Layanan Pos (Kirim Surat Paket)." },
  { "en": "Apa BUMN (Badan Usaha Milik Negara) Layanan Pos?", "id": "PT. (Perseroan Terbatas) Pos Indonesia." },
  { "en": "Apa Itu Kode Pos?", "id": "Kode Area Alamat Pengiriman." },
  { "en": "Apa Itu Perangko?", "id": "Label Bukti Pembayaran Jasa Pos." },
  { "en": "Apa Itu Filateli?", "id": "Hobi Mengumpulkan Perangko." },
  { "en": "Apa Itu Materai (Meterai)?", "id": "Label Pajak Atas Dokumen Resmi." },
  { "en": "Apa Itu E-Meterai?", "id": "Meterai Elektronik (Dokumen Digital)." },
  { "en": "Apa Itu Telekomunikasi?", "id": "Komunikasi Jarak Jauh (Telepon, Internet)." },
  { "en": "Apa BUMN (Badan Usaha Milik Negara) Telekomunikasi Terbesar?", "id": "PT. (Perseroan Terbatas) Telkom Indonesia." },
  { "en": "Apa Produk Telkom Indonesia?", "id": "IndiHome (Internet), Telkomsel (Seluler)." },
  { "en": "Apa Itu Telkomsel?", "id": "Operator Seluler Terbesar Indonesia." },
  { "en": "Apa Itu Kartu SIM (Subscriber Identity Module)?", "id": "Kartu Identitas Pelanggan Seluler." },
  { "en": "Apa Kepanjangan SIM (Subscriber Identity Module)?", "id": "Subscriber Identity Module." },
  { "en": "Apa Itu Jaringan 4G (Fourth Generation)?", "id": "Jaringan Internet Seluler Generasi Keempat." },
  { "en": "Apa Kepanjangan 4G (Fourth Generation)?", "id": "Fourth Generation." },
  { "en": "Apa Itu Jaringan 5G (Fifth Generation)?", "id": "Jaringan Internet Seluler Generasi Kelima." },
  { "en": "Apa Keunggulan 5G (Fifth Generation)?", "id": "Lebih Cepat, Latensi Rendah." },
  { "en": "Apa Itu Kuota Internet?", "id": "Batas Penggunaan Data Internet." },
  { "en": "Apa Itu WiFi (Wireless Fidelity)?", "id": "Jaringan Internet Nirkabel (Tanpa Kabel)." },
  { "en": "Apa Kepanjangan WiFi (Wireless Fidelity)?", "id": "Wireless Fidelity." },
  { "en": "Apa Itu Fiber Optik?", "id": "Kabel Serat Kaca (Internet Cepat)." },
  { "en": "Apa Proyek Palapa Ring?", "id": "Proyek Jaringan Fiber Optik Nasional." },
  { "en": "Apa Tujuan Proyek Palapa Ring?", "id": "Pemerataan Akses Internet Seluruh Indonesia." },
  { "en": "Apa Itu Blank Spot?", "id": "Area Tanpa Sinyal Seluler." },
  { "en": "Apa Itu Menara BTS (Base Transceiver Station)?", "id": "Menara Pemancar Sinyal Seluler." },
  { "en": "Apa Kepanjangan BTS (Base Transceiver Station)?", "id": "Base Transceiver Station." },
  { "en": "Apa Itu Badan Aksesibilitas Telekomunikasi Informasi (BAKTI)?", "id": "Badan Layanan Akses Internet (Area 3T)." },
  { "en": "Apa Kepanjangan BAKTI?", "id": "Badan Aksesibilitas Telekomunikasi Informasi." },
  { "en": "BAKTI (Badan Aksesibilitas Telekomunikasi Informasi) Di Bawah Kementerian Apa?", "id": "Kementerian Komunikasi Dan Informatika (Kominfo)." },
  { "en": "Apa Itu Satelit SATRIA-1?", "id": "Satelit Internet Indonesia." },
  { "en": "Apa Fungsi Satelit SATRIA-1?", "id": "Pemerataan Internet (Area Terpencil)." },
  { "en": "Apa Itu Kementerian Komunikasi Dan Informatika (Kominfo)?", "id": "Kementerian Urusan Komunikasi Informatika." },
  { "en": "Apa Tugas Kominfo (Kementerian Komunikasi Dan Informatika)?", "id": "Regulasi Penyiaran, Internet, Frekuensi." },
  { "en": "Apa Itu Frekuensi Radio?", "id": "Spektrum Gelombang Elektromagnetik." },
  { "en": "Siapa Yang Mengatur Frekuensi Radio?", "id": "Kementerian Komunikasi Dan Informatika (Kominfo)." },
  { "en": "Apa Itu TV Digital?", "id": "Siaran Televisi Digital (Gambar Jernih)." },
  { "en": "Apa Itu Analog Switch Off (ASO)?", "id": "Penghentian Siaran TV Analog." },
  { "en": "Apa Kepanjangan ASO (Analog Switch Off)?", "id": "Analog Switch Off." },
  { "en": "Apa Perangkat Penerima TV Digital?", "id": "Set Top Box (STB)." },
  { "en": "Apa Kepanjangan STB (Set Top Box)?", "id": "Set Top Box." },
  { "en": "Apa Itu Rating Televisi?", "id": "Ukuran Jumlah Penonton Acara TV." },
  { "en": "Apa Itu Lembaga Sensor Film (LSF)?", "id": "Lembaga Penyensoran Film." },
  { "en": "Apa Itu Iklan Layanan Masyarakat (ILM)?", "id": "Iklan Sosialisasi Program Pemerintah." },
  { "en": "Apa Kepanjangan ILM?", "id": "Iklan Layanan Masyarakat." },
  { "en": "Apa Itu E-Sport?", "id": "Olahraga Elektronik (Game Kompetitif)." },
  { "en": "Apakah E-Sport Diakui Di Indonesia?", "id": "Ya, Cabang Olahraga Prestasi (KONI)." },
  { "en": "Apa Itu Developer Game?", "id": "Pengembang Perangkat Lunak Game." },
  { "en": "Apa Itu Influencer?", "id": "Orang Berpengaruh Di Media Sosial." },
  { "en": "Apa Itu Selebgram?", "id": "Selebriti Instagram." },
  { "en": "Apa Itu YouTuber?", "id": "Kreator Konten Di YouTube." },
  { "en": "Apa Itu Buzzer?", "id": "Pihak Pendorong Isu Media Sosial." },
  { "en": "Apa Itu Cyber Security?", "id": "Keamanan Siber." },
  { "en": "Apa Itu Hacker?", "id": "Peretas Sistem Komputer." },
  { "en": "Apa Itu Cracker?", "id": "Peretas Sistem (Tujuan Jahat)." },
  { "en": "Apa Itu Ransomware?", "id": "Virus Enkripsi Data (Minta Tebusan)." },
  { "en": "Apa Itu Pinjaman Online (Pinjol)?", "id": "Pinjaman Uang Berbasis Aplikasi." },
  { "en": "Apa Itu Investasi Bodong?", "id": "Investasi Ilegal (Menipu)." },
  { "en": "Apa Satgas Pengawas Pinjol Ilegal?", "id": "Satgas Waspada Investasi (SWI)." },
  { "en": "Apa Kepanjangan SWI (Satgas Waspada Investasi)?", "id": "Satgas Waspada Investasi." },
  { "en": "Apa Itu Otoritas Jasa Keuangan (OJK)?", "id": "Lembaga Pengawas Jasa Keuangan." },
  { "en": "Siapa Yang Diawasi OJK (Otoritas Jasa Keuangan)?", "id": "Bank, Asuransi, Pasar Modal, Fintech." },
  { "en": "Apa Itu UU (Undang-Undang) PPSK (Pengembangan Penguatan Sektor Keuangan)?", "id": "UU (Undang-Undang) Reformasi Sektor Keuangan." },
  { "en": "Apa Kepanjangan PPSK (Pengembangan Penguatan Sektor Keuangan)?", "id": "Pengembangan Dan Penguatan Sektor Keuangan." },
  { "en": "Apa Itu Komite Stabilitas Sistem Keuangan (KSSK)?", "id": "Komite Pencegahan Krisis Keuangan." },
  { "en": "Apa Kepanjangan KSSK?", "id": "Komite Stabilitas Sistem Keuangan." },
  { "en": "Siapa Anggota KSSK (Komite Stabilitas Sistem Keuangan)?", "id": "Kemenkeu, Bank Indonesia, OJK, LPS." },
  { "en": "Apa Itu Lembaga Penjamin Simpanan (LPS)?", "id": "Lembaga Penjamin Simpanan Nasabah Bank." },
  { "en": "Apa Batas Maksimal Penjaminan LPS?", "id": "Dua Miliar Rupiah Per Nasabah." },
  { "en": "Apa Itu Kesejahteraan Sosial?", "id": "Kondisi Terpenuhinya Kebutuhan Masyarakat." },
  { "en": "Apa Kepanjangan Kemensos?", "id": "Kementerian Sosial." },
  { "en": "Apa Itu Data Terpadu Kesejahteraan Sosial (DTKS)?", "id": "Basis Data Penerima Bantuan Sosial." },
  { "en": "Apa Itu Program Keluarga Harapan (PKH)?", "id": "Bantuan Tunai Bersyarat (Keluarga Miskin)." },
  { "en": "Apa Itu Bantuan Pangan Non Tunai (BPNT)?", "id": "Bantuan Sembako (Non Tunai)." },
  { "en": "Apa Itu Taruna Siaga Bencana (Tagana)?", "id": "Relawan Penanggulangan Bencana Kemensos." },
  { "en": "Apa Kepanjangan Tagana?", "id": "Taruna Siaga Bencana." },
  { "en": "Apa Itu Penyandang Masalah Kesejahteraan Sosial (PMKS)?", "id": "Individu Keluarga Bermasalah Sosial." },
  { "en": "Apa Kepanjangan PMKS?", "id": "Penyandang Masalah Kesejahteraan Sosial." },
  { "en": "Apa Contoh PMKS (Penyandang Masalah Kesejahteraan Sosial)?", "id": "Fakir Miskin, Anak Terlantar." },
  { "en": "Apa Itu Fakir Miskin?", "id": "Orang Tidak Mampu Memenuhi Kebutuhan." },
  { "en": "Apa Amanat Pasal 34 Ayat 1 UUD?", "id": "Fakir Miskin Anak Terlantar Dipelihara Negara." },
  { "en": "Apa Itu Anak Terlantar?", "id": "Anak Tanpa Pengasuhan Orang Tua." },
  { "en": "Apa Itu Panti Sosial Tresna Werdha?", "id": "Panti Jompo (Lanjut Usia)." },
  { "en": "Apa Itu Gelandangan?", "id": "Orang Hidup Tanpa Tempat Tinggal." },
  { "en": "Apa Itu Pengemis?", "id": "Orang Meminta-minta Di Muka Umum." },
  { "en": "Apa Itu Anak Jalanan (Anjal)?", "id": "Anak Beraktivitas Ekonomi Di Jalanan." },
  { "en": "Apa Kepanjangan Anjal?", "id": "Anak Jalanan." },
  { "en": "Apa Itu Pekerja Seks Komersial (PSK)?", "id": "Pekerja Seks Komersial." },
  { "en": "Apa Kepanjangan PSK (Pekerja Seks Komersial)?", "id": "Pekerja Seks Komersial." },
  { "en": "Apa Itu Lokalisasi?", "id": "Area Tempat Prostitusi." },
  { "en": "Apa Kebijakan Pemerintah Terhadap Lokalisasi?", "id": "Penutupan Lokalisasi (Rehabilitasi Sosial)." },
  { "en": "Apa Itu Rehabilitasi Sosial?", "id": "Pemulihan Fungsi Sosial (Mantan PSK)." },
  { "en": "Apa Itu Korban Tindak Kekerasan (KTM)?", "id": "Korban Tindak Kekerasan." },
  { "en": "Apa Itu Korban Bencana?", "id": "Orang Terdampak Bencana Alam Sosial." },
  { "en": "Apa Itu Bencana Sosial?", "id": "Bencana Akibat Manusia (Konflik Sosial)." },
  { "en": "Apa Itu Konflik Sosial?", "id": "Pertentangan Antar Kelompok Masyarakat." },
  { "en": "Apa Itu Komunitas Adat Terpencil (KAT)?", "id": "Komunitas Adat Terpencil." },
  { "en": "Apa Kepanjangan KAT (Komunitas Adat Terpencil)?", "id": "Komunitas Adat Terpencil." },
  { "en": "Apa Itu Program Pemberdayaan KAT?", "id": "Program Pemberdayaan Komunitas Adat Terpencil." },
  { "en": "Apa Itu Penyandang Disabilitas?", "id": "Orang Dengan Keterbatasan Fisik Mental." },
  { "en": "Apa Itu Tunanetra?", "id": "Penyandang Disabilitas Penglihatan." },
  { "en": "Apa Itu Tunarungu?", "id": "Penyandang Disabilitas Pendengaran." },
  { "en": "Apa Itu Tunawicara?", "id": "Penyandang Disabilitas Komunikasi (Bicara)." },
  { "en": "Apa Itu Tunagrahita?", "id": "Penyandang Disabilitas Intelektual." },
  { "en": "Apa Itu Tunadaksa?", "id": "Penyandang Disabilitas Fisik (Tubuh)." },
  { "en": "Apa Itu Autisme?", "id": "Spektrum Gangguan Perkembangan Saraf." },
  { "en": "Apa Itu Orang Dengan Gangguan Jiwa (ODGJ)?", "id": "Orang Dengan Gangguan Jiwa." },
  { "en": "Apa Kepanjangan ODGJ?", "id": "Orang Dengan Gangguan Jiwa." },
  { "en": "Apa Itu Pemasungan?", "id": "Tindakan Mengikat ODGJ (Dilarang)." },
  { "en": "Apa Program Indonesia Bebas Pasung?", "id": "Program Pembebasan ODGJ Dari Pemasungan." },
  { "en": "Apa Itu Kelompok Rentan?", "id": "Kelompok Rawan Terdampak Masalah Sosial." },
  { "en": "Siapa Yang Termasuk Kelompok Rentan?", "id": "Lansia, Anak, Disabilitas, Wanita Hamil." },
  { "en": "Apa Itu Pemberdayaan Perempuan?", "id": "Upaya Peningkatan Peran Perempuan." },
  { "en": "Apa Itu Kesetaraan Gender?", "id": "Kesetaraan Hak Peran Pria Wanita." },
  { "en": "Apa Itu Emansipasi?", "id": "Perjuangan Kesetaraan Hak." },
  { "en": "Siapa Tokoh Emansipasi Wanita Indonesia?", "id": "R.A. (Raden Ajeng) Kartini." },
  { "en": "Apa Itu Kementerian PPPA?", "id": "Kementerian Pemberdayaan Perempuan Perlindungan Anak." },
  { "en": "Apa Kepanjangan PPPA?", "id": "Pemberdayaan Perempuan Perlindungan Anak." },
  { "en": "Apa Itu Kekerasan Terhadap Perempuan?", "id": "Tindak Kekerasan Berbasis Gender." },
  { "en": "Apa Itu Kekerasan Seksual?", "id": "Tindak Pidana Kekerasan Seksual." },
  { "en": "Apa Itu UU TPKS (Undang-Undang Tindak Pidana Kekerasan Seksual)?", "id": "UU (Undang-Undang) Tindak Pidana Kekerasan Seksual." },
  { "en": "Apa Kepanjangan UU TPKS?", "id": "Undang-Undang Tindak Pidana Kekerasan Seksual." },
  { "en": "Apa Itu Pelecehan Seksual?", "id": "Tindakan Bernuansa Seksual Tidak Diinginkan." },
  { "en": "Apa Itu Perlindungan Anak?", "id": "Upaya Melindungi Hak-Hak Anak." },
  { "en": "Apa Itu Komisi Perlindungan Anak Indonesia (KPAI)?", "id": "Lembaga Pengawas Perlindungan Anak." },
  { "en": "Apa Itu Hak Anak?", "id": "Hak Hidup, Tumbuh Kembang, Perlindungan." },
  { "en": "Apa Itu Pekerja Anak?", "id": "Anak Di Bawah Umur Bekerja." },
  { "en": "Mengapa Pekerja Anak Dilarang?", "id": "Mengganggu Pendidikan Kesehatan Anak." },
  { "en": "Apa Itu Pernikahan Dini?", "id": "Pernikahan Di Bawah Usia Minimal." },
  { "en": "Berapa Batas Usia Minimal Menikah?", "id": "Sembilan Belas Tahun." },
  { "en": "Apa Dampak Pernikahan Dini?", "id": "Stunting, KDRT, Putus Sekolah." },
  { "en": "Apa Itu Dispensasi Kawin?", "id": "Izin Pengadilan Menikah Dini." },
  { "en": "Apa Itu Akta Kelahiran?", "id": "Bukti Sah Kelahiran Anak." },
  { "en": "Apa Hak Anak Atas Akta Kelahiran?", "id": "Hak Atas Identitas." },
  { "en": "Apa Itu Kota Layak Anak (KLA)?", "id": "Program Kota Peduli Hak Anak." },
  { "en": "Apa Kepanjangan KLA?", "id": "Kota Layak Anak." },
  { "en": "Apa Itu Forum Anak Nasional?", "id": "Wadah Partisipasi Anak Indonesia." },
  { "en": "Apa Itu Pusat Pelayanan Terpadu Pemberdayaan Perempuan Anak (P2TP2A)?", "id": "Layanan Korban Kekerasan." },
  { "en": "Apa Kepanjangan P2TP2A?", "id": "Pusat Pelayanan Terpadu Pemberdayaan Perempuan Anak." },
  { "en": "Apa Tugas Kemenpora (Kementerian Pemuda Dan Olahraga)?", "id": "Mengurus Urusan Pemuda Dan Olahraga." },
  { "en": "Apa Itu Pekan Olahraga Nasional (PON)?", "id": "Pesta Olahraga Nasional (Empat Tahunan)." },
  { "en": "Apa Kepanjangan PON (Pekan Olahraga Nasional)?", "id": "Pekan Olahraga Nasional." },
  { "en": "Dimana PON (Pekan Olahraga Nasional) 2024 Diadakan?", "id": "Aceh Dan Sumatera Utara." },
  { "en": "Apa Itu Pekan Paralimpiade Nasional (Peparnas)?", "id": "Pesta Olahraga Atlet Disabilitas." },
  { "en": "Apa Kepanjangan Peparnas?", "id": "Pekan Paralimpiade Nasional." },
  { "en": "Apa Itu SEA Games?", "id": "Pesta Olahraga Asia Tenggara." },
  { "en": "Apa Kepanjangan SEA Games?", "id": "Southeast Asian Games." },
  { "en": "Apa Itu Asian Games?", "id": "Pesta Olahraga Tingkat Asia." },
  { "en": "Kapan Indonesia Tuan Rumah Asian Games?", "id": "Tahun 1962 Dan 2018." },
  { "en": "Apa Itu Olimpiade?", "id": "Pesta Olahraga Tingkat Dunia." },
  { "en": "Cabang Andalan Indonesia Di Olimpiade?", "id": "Bulu Tangkis, Angkat Besi." },
  { "en": "Siapa Atlet Emas Olimpiade Bulu Tangkis Pertama?", "id": "Susi Susanti Dan Alan Budikusuma." },
  { "en": "Apa Itu Indonesia Masters?", "id": "Turnamen Bulu Tangkis Internasional." },
  { "en": "Apa Induk Organisasi Sepak Bola Indonesia?", "id": "Persatuan Sepak Bola Seluruh Indonesia (PSSI)." },
  { "en": "Apa Induk Organisasi Bulu Tangkis Indonesia?", "id": "Persatuan Bulu Tangkis Seluruh Indonesia (PBSI)." },
  { "en": "Apa Itu Liga 1 Indonesia?", "id": "Kompetisi Sepak Bola Profesional Tertinggi." },
  { "en": "Apa Itu Pusat Pelatihan Atlet Nasional?", "id": "Pelatnas (Pemusatan Latihan Nasional)." },
  { "en": "Apa Kepanjangan Pelatnas?", "id": "Pemusatan Latihan Nasional." },
  { "en": "Apa Itu Sistem Keolahragaan Nasional (SKN)?", "id": "Sistem Penyelenggaraan Keolahragaan Nasional." },
  { "en": "Apa Kepanjangan SKN (Sistem Keolahragaan Nasional)?", "id": "Sistem Keolahragaan Nasional." },
  { "en": "Apa Itu Hari Olahraga Nasional (Haornas)?", "id": "Peringatan Hari Olahraga Nasional." },
  { "en": "Kapan Haornas (Hari Olahraga Nasional) Diperingati?", "id": "Tanggal Sembilan September." },
  { "en": "Apa Kepanjangan Haornas?", "id": "Hari Olahraga Nasional." },
  { "en": "Apa Itu Penyiaran Publik?", "id": "Penyiaran Demi Kepentingan Publik." },
  { "en": "Apa Lembaga Penyiaran Publik (LPP) Indonesia?", "id": "RRI (Radio Republik Indonesia) Dan TVRI (Televisi Republik Indonesia)." },
  { "en": "Apa Kepanjangan LPP (Lembaga Penyiaran Publik)?", "id": "Lembaga Penyiaran Publik." },
  { "en": "RRI (Radio Republik Indonesia) TVRI (Televisi Republik Indonesia) Bertanggung Jawab Kepada Siapa?", "id": "Presiden Dan DPR (Dewan Perwakilan Rakyat)." },
  { "en": "Apa Itu Lembaga Penyiaran Swasta (LPS)?", "id": "TV (Televisi) Radio Komersial Milik Swasta." },
  { "en": "Apa Kepanjangan LPS (Lembaga Penyiaran Swasta)?", "id": "Lembaga Penyiaran Swasta." },
  { "en": "Apa Itu Lembaga Penyiaran Komunitas (LPK)?", "id": "Penyiaran Milik Komunitas (Non-Profit)." },
  { "en": "Apa Kepanjangan LPK (Lembaga Penyiaran Komunitas)?", "id": "Lembaga Penyiaran Komunitas." },
  { "en": "Apa Itu Lembaga Penyiaran Berlangganan (LPB)?", "id": "Penyiaran Berbayar (TV Kabel)." },
  { "en": "Apa Kepanjangan LPB (Lembaga Penyiaran Berlangganan)?", "id": "Lembaga Penyiaran Berlangganan." },
  { "en": "Siapa Pengawas Penyiaran Di Indonesia?", "id": "Komisi Penyiaran Indonesia (KPI)." },
  { "en": "Apa Sanksi Pelanggaran Siaran?", "id": "Teguran, Penghentian Siaran, Denda." },
  { "en": "Apa Itu Siaran Iklan?", "id": "Siaran Promosi Barang Jasa." },
  { "en": "Apa Itu Iklan Layanan Masyarakat (ILM)?", "id": "Iklan Non-Komersial (Sosial)." },
  { "en": "Apa Itu Siaran Pemilu?", "id": "Siaran Khusus Pemilu (Kampanye, Debat)." },
  { "en": "Apa Itu Hak Siar?", "id": "Izin Menyiarkan Acara (Olahraga)." },
  { "en": "Apa Itu Migrasi TV (Televisi) Analog Ke Digital?", "id": "Peralihan Teknologi Siaran TV." },
  { "en": "Apa Nama Migrasi TV (Televisi) Digital?", "id": "Analog Switch Off (ASO)." },
  { "en": "Apa Perangkat Penerima Siaran Digital?", "id": "Set Top Box (STB)." },
  { "en": "Apa Keuntungan Siaran Digital?", "id": "Gambar Jernih, Suara Bersih." },
  { "en": "Apa Itu Frekuensi Radio?", "id": "Alokasi Gelombang Radio (Publik)." },
  { "en": "Siapa Pengatur Alokasi Frekuensi?", "id": "Kementerian Kominfo (Kementerian Komunikasi Informatika)." },
  { "en": "Apa Itu Balai Monitor (Balmon)?", "id": "Unit Pengawas Spektrum Frekuensi." },
  { "en": "Apa Itu Radio Amatir?", "id": "Hobi Komunikasi Radio (ORARI)." },
  { "en": "Apa Kepanjangan ORARI?", "id": "Organisasi Amatir Radio Indonesia." },
  { "en": "Apa Itu Radio Antar Penduduk Indonesia (RAPI)?", "id": "Organisasi Komunikasi Radio Antar Penduduk." },
  { "en": "Apa Kepanjangan RAPI?", "id": "Radio Antar Penduduk Indonesia." },
  { "en": "Apa Itu Sinyal Darurat (SOS)?", "id": "Sinyal Minta Bantuan Internasional." },
  { "en": "Apa Kepanjangan SOS (Save Our Souls)?", "id": "Save Our Souls." },
  { "en": "Apa Itu Kantor Berita?", "id": "Lembaga Pengumpul Penyebar Berita." },
  { "en": "Apa Kantor Berita Nasional Indonesia?", "id": "LKBN (Lembaga Kantor Berita Nasional) Antara." },
  { "en": "Apa Status LKBN (Lembaga Kantor Berita Nasional) Antara?", "id": "BUMN (Badan Usaha Milik Negara) Perum." },
  { "en": "Apa Itu Jurnalisme Warga?", "id": "Aktivitas Jurnalistik Oleh Warga Biasa." },
  { "en": "Apa Itu Media Sosial?", "id": "Platform Interaksi Online (Facebook, X)." },
  { "en": "Apa Itu Literasi Media?", "id": "Kemampuan Menganalisis Pesan Media." },
  { "en": "Apa Itu Kedaulatan Digital?", "id": "Kedaulatan Negara Di Ruang Siber." },
  { "en": "Apa Itu Perlindungan Data Pribadi?", "id": "Hak Kontrol Atas Data Pribadi." },
  { "en": "Apa UU (Undang-Undang) Perlindungan Data Pribadi?", "id": "UU PDP (Undang-Undang Perlindungan Data Pribadi)." },
  { "en": "Apa Lembaga Pengawas PDP (Perlindungan Data Pribadi)?", "id": "Otoritas Perlindungan Data Pribadi." },
  { "en": "Apa Itu Kebocoran Data?", "id": "Peristiwa Data Pribadi Tersebar." },
  { "en": "Apa Itu Kejahatan Siber?", "id": "Kejahatan Di Dunia Maya." },
  { "en": "Apa Itu Peretasan (Hacking)?", "id": "Menyusup Sistem Komputer Tanpa Izin." },
  { "en": "Apa Itu Penipuan Online (Scam)?", "id": "Penipuan Menggunakan Internet." },
  { "en": "Apa Itu Judi Online?", "id": "Perjudian Melalui Internet (Ilegal)." },
  { "en": "Apa Satgas Pemberantasan Judi Online?", "id": "Satuan Tugas Khusus (Pemerintah)." },
  { "en": "Apa Itu Tindak Pidana Pencucian Uang (TPPU)?", "id": "Kejahatan Menyamarkan Uang Haram." },
  { "en": "Siapa Pengawas Transaksi Keuangan?", "id": "PPATK (Pusat Pelaporan Analisis Transaksi Keuangan)." },
  { "en": "Apa Itu Transaksi Keuangan Mencurigakan (TKM)?", "id": "Transaksi Tidak Wajar (Indikasi TPPU)." },
  { "en": "Apa Kepanjangan TKM (Transaksi Keuangan Mencurigakan)?", "id": "Transaksi Keuangan Mencurigakan." },
  { "en": "Apa Itu Penyedia Jasa Keuangan (PJK)?", "id": "Bank Dan Lembaga Keuangan." },
  { "en": "Apa Kepanjangan PJK (Penyedia Jasa Keuangan)?", "id": "Penyedia Jasa Keuangan." },
  { "en": "Apa Itu Prinsip Kenali Pengguna Jasa (KYC)?", "id": "Prinsip Bank Kenali Identitas Nasabah." },
  { "en": "Apa Kepanjangan KYC (Know Your Customer)?", "id": "Know Your Customer." },
  { "en": "Apa Itu Pembekuan Aset?", "id": "Penghentian Sementara Transaksi Aset." },
  { "en": "Apa Itu Perampasan Aset?", "id": "Pengambilan Aset Hasil Kejahatan Negara." },
  { "en": "Apa Itu RUU (Rancangan Undang-Undang) Perampasan Aset?", "id": "RUU (Rancangan Undang-Undang) Perampasan Aset Koruptor." },
  { "en": "Apa Itu Ekstradisi?", "id": "Penyerahan Buronan Ke Negara Asal." },
  { "en": "Apa Itu Bantuan Hukum Timbal Balik (MLA)?", "id": "Kerja Sama Hukum Lintas Negara." },
  { "en": "Apa Kepanjangan MLA (Mutual Legal Assistance)?", "id": "Mutual Legal Assistance." },
  { "en": "Apa Itu Interpol (Organisasi Polisi Kriminal Internasional)?", "id": "Organisasi Polisi Kriminal Internasional." },
  { "en": "Apa Kepanjangan Interpol?", "id": "International Criminal Police Organization." },
  { "en": "Apa Itu Red Notice Interpol?", "id": "Permintaan Penangkapan Buronan Internasional." },
  { "en": "Apa Peran Polri (Kepolisian Negara Republik Indonesia) Di Interpol?", "id": "National Central Bureau (NCB) Interpol Jakarta." },
  { "en": "Apa Kepanjangan NCB (National Central Bureau)?", "id": "National Central Bureau." },
  { "en": "Apa Itu Kejahatan Transnasional?", "id": "Kejahatan Lintas Batas Negara." },
  { "en": "Apa Contoh Kejahatan Transnasional?", "id": "Terorisme, Perdagangan Narkoba, TPPO." },
  { "en": "Apa Itu ASEANAPOL?", "id": "Kerja Sama Kepolisian Negara ASEAN." },
  { "en": "Apa Kepanjangan ASEANAPOL?", "id": "ASEAN Chiefs Of National Police." },
  { "en": "Apa Itu Kejahatan Kerah Putih (White Collar Crime)?", "id": "Kejahatan Finansial (Korupsi, Penipuan)." },
  { "en": "Apa Itu Kejahatan Kerah Biru (Blue Collar Crime)?", "id": "Kejahatan Konvensional (Pencurian)." },
  { "en": "Apa Itu Premanisme?", "id": "Aksi Pemerasan Kekerasan." },
  { "en": "Apa Itu Geng Motor?", "id": "Kelompok Pengendara Motor (Sering Kriminal)." },
  { "en": "Apa Itu Sistem Peradilan Pidana Terpadu (SPPT)?", "id": "Sistem Koordinasi Aparat Hukum Pidana." },
  { "en": "Apa Kepanjangan SPPT (Sistem Peradilan Pidana Terpadu)?", "id": "Sistem Peradilan Pidana Terpadu." },
  { "en": "Apa Itu E-Berpadu (Elektronik Berkas Pidana Terpadu)?", "id": "Aplikasi Elektronik Berkas Pidana Terpadu." },
  { "en": "Apa Itu Mahkamah Pelayaran?", "id": "Pengadilan Khusus Kecelakaan Kapal." },
  { "en": "Apa Itu KNKT (Komite Nasional Keselamatan Transportasi)?", "id": "Komite Nasional Keselamatan Transportasi." },
  { "en": "Apa Tugas KNKT (Komite Nasional Keselamatan Transportasi)?", "id": "Investigasi Kecelakaan Transportasi." },
  { "en": "Apa Sifat Investigasi KNKT (Komite Nasional Keselamatan Transportasi)?", "id": "Bukan Pro-Yustisia (Bukan Penyelidikan Pidana)." },
  { "en": "Apa Itu Kotak Hitam (Black Box)?", "id": "Perekam Data Penerbangan." },
  { "en": "Apa Fungsi Kotak Hitam?", "id": "Menganalisis Penyebab Kecelakaan Pesawat." },
  { "en": "Apa Itu Budi Pekerti?", "id": "Moral, Watak, Atau Perilaku Baik." },
  { "en": "Apa Itu Tata Krama?", "id": "Aturan Sopan Santun Dalam Pergaulan." },
  { "en": "Apa Itu Etika?", "id": "Ilmu Tentang Apa Yang Baik Buruk." },
  { "en": "Apa Itu Moral?", "id": "Ajaran Baik Buruk Perbuatan." },
  { "en": "Apa Itu Akhlak?", "id": "Budi Pekerti Atau Kelakuan." },
  { "en": "Apa Itu Sifat Malu?", "id": "Perasaan Tidak Nyaman Akibat Celaan." },
  { "en": "Apa Itu Sifat Jujur?", "id": "Berkata Benar, Tidak Curang." },
  { "en": "Apa Itu Sifat Disiplin?", "id": "Taat Patuh Pada Aturan Nilai." },
  { "en": "Apa Itu Sifat Tanggung Jawab?", "id": "Siap Menanggung Akibat Perbuatan." },
  { "en": "Apa Itu Adat Istiadat?", "id": "Kebiasaan Turun Temurun." },
  { "en": "Apa Itu Hukum Adat?", "id": "Hukum Tidak Tertulis Berlaku." },
  { "en": "Apa Itu Musyawarah Desa?", "id": "Pengambilan Keputusan Tertinggi Desa." },
  { "en": "Apa Itu Lembaga Adat?", "id": "Lembaga Pelaksana Hukum Adat." },
  { "en": "Apa Itu Hak Ulayat?", "id": "Hak Kolektif Masyarakat Adat (Tanah)." },
  { "en": "Apa Itu Masyarakat Hukum Adat (MHA)?", "id": "Kelompok Masyarakat Hukum Adat." },
  { "en": "Apa Kepanjangan MHA (Masyarakat Hukum Adat)?", "id": "Masyarakat Hukum Adat." },
  { "en": "Apa Syarat Pengakuan MHA (Masyarakat Hukum Adat)?", "id": "Masih Hidup, Sesuai Prinsip NKRI." },
  { "en": "Apa Itu Kearifan Lokal?", "id": "Kebijaksanaan Lokal (Budaya)." },
  { "en": "Apa Contoh Kearifan Lokal?", "id": "Sistem Subak (Bali), Sasi (Maluku)." },
  { "en": "Apa Itu Subak?", "id": "Sistem Irigasi Sawah Bali." },
  { "en": "Apa Itu Sasi?", "id": "Larangan Panen Sumber Daya Alam." },
  { "en": "Apa Itu Suku Baduy?", "id": "Suku Adat Di Banten." },
  { "en": "Apa Itu Suku Anak Dalam?", "id": "Suku Adat Di Jambi." },
  { "en": "Apa Itu Suku Kajang?", "id": "Suku Adat Di Sulawesi Selatan." },
  { "en": "Apa Itu Pakaian Adat Suku Kajang?", "id": "Pakaian Serba Hitam." },
  { "en": "Apa Itu Cagar Budaya?", "id": "Warisan Budaya Benda (Situs, Candi)." },
  { "en": "Siapa Yang Menetapkan Cagar Budaya?", "id": "Pemerintah (Daerah Atau Pusat)." },
  { "en": "Apa Itu Balai Pelestarian Cagar Budaya (BPCB)?", "id": "Unit Pelaksana Teknis Pelestarian Cagar Budaya." },
  { "en": "Apa Kepanjangan BPCB?", "id": "Balai Pelestarian Cagar Budaya." },
  { "en": "Apa Itu Revitalisasi Cagar Budaya?", "id": "Upaya Menghidupkan Kembali Cagar Budaya." },
  { "en": "Apa Itu Museum?", "id": "Lembaga Penyimpan Merawat Benda Sejarah." },
  { "en": "Apa Museum Nasional Indonesia?", "id": "Museum Gajah (Jakarta)." },
  { "en": "Apa Itu Museum Geologi Bandung?", "id": "Museum Koleksi Fosil Geologi." },
  { "en": "Apa Itu Museum Tsunami Aceh?", "id": "Museum Peringatan Tsunami Aceh 2004." },
  { "en": "Apa Itu Naskah Kuno?", "id": "Manuskrip Tulisan Tangan Kuno." },
  { "en": "Apa Itu Aksara?", "id": "Sistem Tulisan." },
  { "en": "Apa Aksara Kuno Indonesia?", "id": "Aksara Pallawa, Kawi, Sunda Kuno." },
  { "en": "Apa Aksara Asli Indonesia?", "id": "Aksara Lontara (Bugis), Hanacaraka (Jawa)." },
  { "en": "Apa Itu Prasasti?", "id": "Piagam Tertulis Di Batu Logam." },
  { "en": "Prasasti Menggunakan Huruf Apa?", "id": "Huruf Pallawa, Bahasa Sanskerta." },
  { "en": "Apa Prasasti Tertua Indonesia?", "id": "Prasasti Yupa (Kutai)." },
  { "en": "Apa Kerajaan Hindu Tertua Di Indonesia?", "id": "Kerajaan Kutai Martapura." },
  { "en": "Apa Kerajaan Buddha Terbesar Indonesia?", "id": "Kerajaan Sriwijaya." },
  { "en": "Dimana Pusat Kerajaan Sriwijaya?", "id": "Di Palembang (Sumatera Selatan)." },
  { "en": "Apa Peninggalan Kerajaan Sriwijaya?", "id": "Candi Muara Takus, Prasasti Kedukan Bukit." },
  { "en": "Apa Kerajaan Hindu-Buddha Terakhir Terbesar?", "id": "Kerajaan Majapahit." },
  { "en": "Dimana Pusat Kerajaan Majapahit?", "id": "Di Trowulan, Jawa Timur." },
  { "en": "Siapa Pendiri Kerajaan Majapahit?", "id": "Raden Wijaya." },
  { "en": "Siapa Raja Terbesar Majapahit?", "id": "Hayam Wuruk." },
  { "en": "Siapa Patih Terkenal Majapahit?", "id": "Gajah Mada." },
  { "en": "Apa Sumpah Terkenal Gajah Mada?", "id": "Sumpah Palapa (Menyatukan Nusantara)." },
  { "en": "Apa Kitab Peninggalan Majapahit?", "id": "Kitab Negarakertagama Dan Sutasoma." },
  { "en": "Siapa Penulis Kitab Negarakertagama?", "id": "Mpu Prapanca." },
  { "en": "Apa Isi Kitab Negarakertagama?", "id": "Kisah Perjalanan Hayam Wuruk." },
  { "en": "Siapa Penulis Kitab Sutasoma?", "id": "Mpu Tantular." },
  { "en": "Apa Isi Terkenal Kitab Sutasoma?", "id": "Semboyan Bhinneka Tunggal Ika." },
  { "en": "Apa Candi Peninggalan Kerajaan Buddha?", "id": "Candi Borobudur." },
  { "en": "Candi Borobudur Peninggalan Dinasti Apa?", "id": "Dinasti Syailendra (Mataram Kuno)." },
  { "en": "Apa Candi Peninggalan Kerajaan Hindu?", "id": "Candi Prambanan." },
  { "en": "Candi Prambanan Peninggalan Dinasti Apa?", "id": "Dinasti Sanjaya (Mataram Kuno)." },
  { "en": "Apa Kerajaan Islam Pertama Di Jawa?", "id": "Kerajaan Demak." },
  { "en": "Siapa Pendiri Kerajaan Demak?", "id": "Raden Patah." },
  { "en": "Siapa Wali Songo?", "id": "Sembilan Wali Penyebar Islam Di Jawa." },
  { "en": "Sebutkan Anggota Wali Songo?", "id": "Sunan Gresik, Sunan Ampel, Sunan Bonang." },
  { "en": "Sebutkan Anggota Wali Songo Lainnya?", "id": "Sunan Drajat, Sunan Kudus, Sunan Giri." },
  { "en": "Sebutkan Tiga Wali Songo Terakhir?", "id": "Sunan Kalijaga, Sunan Muria, Sunan Gunung Jati." },
  { "en": "Apa Media Dakwah Sunan Kalijaga?", "id": "Melalui Kesenian (Wayang, Gamelan)." },
  { "en": "Siapa Sultan Terkenal Kerajaan Aceh?", "id": "Sultan Iskandar Muda." },
  { "en": "Siapa Sultan Terkenal Kerajaan Ternate?", "id": "Sultan Baabullah." },
  { "en": "Siapa Sultan Terkenal Kerajaan Gowa-Tallo?", "id": "Sultan Hasanuddin." },
  { "en": "Apa Perjanjian Antara Sultan Hasanuddin VOC?", "id": "Perjanjian Bongaya." },
  { "en": "Apa Isi Perjanjian Bongaya?", "id": "VOC (Vereenigde Oostindische Compagnie) Monopoli Perdagangan Makassar." },
  { "en": "Siapa Sultan Terkenal Mataram Islam?", "id": "Sultan Agung." },
  { "en": "Sultan Agung Menyerang VOC (Vereenigde Oostindische Compagnie) Dimana?", "id": "Menyerang Batavia (Dua Kali Gagal)." },
  { "en": "Apa Perjanjian Pemecah Mataram Islam?", "id": "Perjanjian Giyanti (1755)." },
  { "en": "Perjanjian Giyanti Membagi Mataram Menjadi?", "id": "Kasunanan Surakarta Kasultanan Yogyakarta." },
  { "en": "Apa Perang Diponegoro?", "id": "Perang Jawa Melawan Belanda (1825-1830)." },
  { "en": "Apa Penyebab Perang Diponegoro?", "id": "Pemasangan Patok Jalan Tol (Makam Leluhur)." },
  { "en": "Apa Taktik Perang Pangeran Diponegoro?", "id": "Perang Gerilya." },
  { "en": "Apa Taktik Belanda Melawan Diponegoro?", "id": "Benteng Stelsel (Sistem Benteng)." },
  { "en": "Bagaimana Akhir Perang Diponegoro?", "id": "Pangeran Diponegoro Ditipu Ditangkap." },
  { "en": "Apa Perang Padri?", "id": "Perang Saudara (Padri Adat) Belanda." },
  { "en": "Siapa Pemimpin Kaum Padri?", "id": "Tuanku Imam Bonjol." },
  { "en": "Apa Perang Aceh?", "id": "Perang Rakyat Aceh Melawan Belanda." },
  { "en": "Siapa Pahlawan Perang Aceh?", "id": "Teuku Umar, Cut Nyak Dhien." },
  { "en": "Siapa Tokoh Belanda Penakluk Aceh?", "id": "Snouck Hurgronje (Pura-pura Masuk Islam)." },
  { "en": "Apa Perang Banjar?", "id": "Perang Rakyat Banjar Melawan Belanda." },
  { "en": "Siapa Pemimpin Perang Banjar?", "id": "Pangeran Antasari." },
  { "en": "Apa Perang Puputan Di Bali?", "id": "Perang Habis-habisan Melawan Belanda." },
  { "en": "Siapa Raja Bali Terkenal (Perang Puputan)?", "id": "I Gusti Ngurah Rai (Margarana)." },
  { "en": "Siapa Pemimpin Perlawanan Rakyat Maluku?", "id": "Kapitan Pattimura (Thomas Matulessy)." },
  { "en": "Kapan Pattimura Melawan Belanda?", "id": "Tahun 1817." },
  { "en": "Siapa Pahlawan Wanita Maluku (Pattimura)?", "id": "Martha Christina Tiahahu." },
  { "en": "Apa Perlawanan Sisingamangaraja XII?", "id": "Perang Batak Melawan Belanda." },
  { "en": "Darimana Asal Sisingamangaraja XII?", "id": "Dari Tapanuli, Sumatera Utara." },
  { "en": "Apa Itu Sarekat Islam (SI)?", "id": "Organisasi Pergerakan Nasional (Dagang Islam)." },
  { "en": "Siapa Pendiri Sarekat Dagang Islam (SDI)?", "id": "Haji Samanhudi (Di Solo)." },
  { "en": "Siapa Pemimpin Sarekat Islam (SI) Terkenal?", "id": "H.O.S. (Haji Oemar Said) Cokroaminoto." },
  { "en": "Apa Itu Budi Utomo?", "id": "Organisasi Pergerakan Nasional Pertama." },
  { "en": "Kapan Budi Utomo Berdiri?", "id": "Dua Puluh Mei 1908." },
  { "en": "Siapa Pendiri Budi Utomo?", "id": "Dr. Soetomo Dan Mahasiswa Stovia." },
  { "en": "Siapa Penggagas Budi Utomo?", "id": "Dr. Wahidin Soedirohoesodo." },
  { "en": "Tanggal Berdirinya Budi Utomo Diperingati Hari Apa?", "id": "Hari Kebangkitan Nasional." },
  { "en": "Apa Itu Indische Partij (IP)?", "id": "Partai Politik Pertama (Tiga Serangkai)." },
  { "en": "Siapa Tiga Serangkai Pendiri IP?", "id": "Douwes Dekker, Cipto Mangunkusumo, Ki Hadjar." },
  { "en": "Apa Nama Lain Douwes Dekker?", "id": "Danudirja Setiabudi." },
  { "en": "Apa Itu Muhammadiyah?", "id": "Organisasi Islam Modernis (Pendidikan Sosial)." },
  { "en": "Kapan Muhammadiyah Berdiri?", "id": "Tahun 1912 Di Yogyakarta." },
  { "en": "Apa Itu Nahdlatul Ulama (NU)?", "id": "Organisasi Islam Tradisionalis." },
  { "en": "Apa Kepanjangan NU (Nahdlatul Ulama)?", "id": "Nahdlatul Ulama." },
  { "en": "Siapa Pendiri NU (Nahdlatul Ulama)?", "id": "K.H. (Kiai Haji) Hasyim Asy'ari." },
  { "en": "Kapan NU (Nahdlatul Ulama) Berdiri?", "id": "Tahun 1926 Di Surabaya." },
  { "en": "Apa Itu Taman Siswa?", "id": "Organisasi Pendidikan Nasional." },
  { "en": "Siapa Pendiri Taman Siswa?", "id": "Ki Hadjar Dewantara." },
  { "en": "Apa Semboyan Pendidikan Taman Siswa?", "id": "Tut Wuri Handayani." },
  { "en": "Apa Itu Partai Komunis Indonesia (PKI)?", "id": "Partai Berideologi Komunis." },
  { "en": "Kapan Pemberontakan PKI (Partai Komunis Indonesia) Pertama?", "id": "Tahun 1926-1927." },
  { "en": "Kapan Pemberontakan PKI (Partai Komunis Indonesia) Kedua?", "id": "Tahun 1948 (Madiun)." },
  { "en": "Apa Itu Partai Nasional Indonesia (PNI)?", "id": "Partai Pergerakan Nasional (Soekarno)." },
  { "en": "Siapa Pendiri PNI (Partai Nasional Indonesia)?", "id": "Ir. Soekarno (Tahun 1927)." },
  { "en": "Apa Pledoi (Pembelaan) Soekarno Yang Terkenal?", "id": "Indonesia Menggugat." },
  { "en": "Apa Itu Sumpah Pemuda?", "id": "Ikrar Pemuda Indonesia." },
  { "en": "Kapan Sumpah Pemuda Diikrarkan?", "id": "Dua Puluh Delapan Oktober 1928." },
  { "en": "Dimana Kongres Pemuda II Diadakan?", "id": "Di Batavia (Jakarta)." },
  { "en": "Apa Hasil Kongres Pemuda II?", "id": "Sumpah Pemuda (Satu Nusa Bangsa Bahasa)." },
  { "en": "Apa Lagu Wajib Saat Kongres Pemuda II?", "id": "Lagu Indonesia Raya." },
  { "en": "Siapa Pencipta Lagu Indonesia Raya?", "id": "W.R. (Wage Rudolf) Supratman." },
  { "en": "Apa Organisasi Pemuda Pelopor Kongres?", "id": "PPPI (Perhimpunan Pelajar Pelajar Indonesia)." },
  { "en": "Apa Kepanjangan PPPI?", "id": "Perhimpunan Pelajar Pelajar Indonesia." },
  { "en": "Siapa Ketua Kongres Pemuda II?", "id": "Soegondo Djojopoespito." },
  { "en": "Siapa Penulis Rumusan Teks Sumpah Pemuda?", "id": "Mohammad Yamin." },
  { "en": "Apa Itu BPUPKI (Badan Penyelidik Usaha-Usaha Persiapan Kemerdekaan Indonesia)?", "id": "Badan Persiapan Kemerdekaan Jepang." },
  { "en": "Apa Nama Jepang BPUPKI?", "id": "Dokuritsu Junbi Cosakai." },
  { "en": "Siapa Ketua BPUPKI (Badan Penyelidik Usaha-Usaha Persiapan Kemerdekaan Indonesia)?", "id": "Dr. Radjiman Wedyodiningrat." },
  { "en": "Apa Agenda Sidang Pertama BPUPKI?", "id": "Merumuskan Dasar Negara." },
  { "en": "Kapan Soekarno Berpidato Lahirnya Pancasila?", "id": "Tanggal Satu Juni 1945." },
  { "en": "Tanggal Satu Juni Diperingati Hari Apa?", "id": "Hari Lahir Pancasila." },
  { "en": "Apa Usulan Nama Dasar Negara Soekarno?", "id": "Pancasila." },
  { "en": "Apa Usulan Trisila Soekarno?", "id": "Sosio-Nasionalisme, Sosio-Demokrasi, Ketuhanan." },
  { "en": "Apa Usulan Ekasila Soekarno?", "id": "Gotong Royong." },
  { "en": "Apa Itu Panitia Sembilan?", "id": "Panitia Perumus Piagam Jakarta." },
  { "en": "Apa Hasil Panitia Sembilan?", "id": "Piagam Jakarta (Jakarta Charter)." },
  { "en": "Apa Perbedaan Piagam Jakarta Pancasila?", "id": "Kalimat Sila Pertama." },
  { "en": "Apa Agenda Sidang Kedua BPUPKI?", "id": "Membahas Rancangan UUD (Undang-Undang Dasar)." },
  { "en": "Apa Badan Pengganti BPUPKI (Badan Penyelidik Usaha-Usaha Persiapan Kemerdekaan Indonesia)?", "id": "PPKI (Panitia Persiapan Kemerdekaan Indonesia)." },
  { "en": "Apa Nama Jepang PPKI?", "id": "Dokuritsu Junbi Inkai." },
  { "en": "Kapan Jepang Menyerah Pada Sekutu?", "id": "Tanggal Lima Belas Agustus 1945." },
  { "en": "Apa Peristiwa Rengasdengklok?", "id": "Penculikan Soekarno Hatta Oleh Pemuda." },
  { "en": "Apa Tujuan Peristiwa Rengasdengklok?", "id": "Mendesak Proklamasi Kemerdekaan." },
  { "en": "Dimana Naskah Proklamasi Dirumuskan?", "id": "Di Rumah Laksamana Maeda." },
  { "en": "Siapa Yang Menulis Naskah Proklamasi?", "id": "Ir. Soekarno." },
  { "en": "Siapa Yang Mengetik Naskah Proklamasi?", "id": "Sayuti Melik." },
  { "en": "Siapa Penandatangan Naskah Proklamasi?", "id": "Soekarno Dan Hatta." },
  { "en": "Atas Nama Siapa Proklamasi Ditandatangani?", "id": "Atas Nama Bangsa Indonesia." },
  { "en": "Kapan Proklamasi Kemerdekaan Indonesia?", "id": "Tujuh Belas Agustus 1945." },
  { "en": "Dimana Proklamasi Dibacakan?", "id": "Jalan Pegangsaan Timur 56 Jakarta." },
  { "en": "Siapa Yang Menjahit Bendera Pusaka?", "id": "Ibu Fatmawati." },
  { "en": "Siapa Pengibar Bendera Pusaka Pertama?", "id": "Latief Hendraningrat Dan Suhud." },
  { "en": "Apa Acara Pertama Setelah Proklamasi?", "id": "Sidang PPKI (Panitia Persiapan Kemerdekaan Indonesia)." },
  { "en": "Kapan Sidang PPKI (Panitia Persiapan Kemerdekaan Indonesia) Pertama?", "id": "Tanggal Delapan Belas Agustus 1945." },
  { "en": "Apa Hasil Sidang PPKI (Panitia Persiapan Kemerdekaan Indonesia) Pertama?", "id": "Mengesahkan UUD 1945." },
  { "en": "Apa Hasil Kedua Sidang PPKI (Panitia Persiapan Kemerdekaan Indonesia) Pertama?", "id": "Memilih Presiden Dan Wakil Presiden." },
  { "en": "Siapa Presiden Pertama?", "id": "Ir. Soekarno." },
  { "en": "Siapa Wakil Presiden Pertama?", "id": "Drs. Mohammad Hatta." },
  { "en": "Apa Hasil Ketiga Sidang PPKI (Panitia Persiapan Kemerdekaan Indonesia) Pertama?", "id": "Membentuk Komite Nasional." },
  { "en": "Apa Perubahan Besar UUD 1945?", "id": "Penghapusan Tujuh Kata Sila Pertama." },
  { "en": "Apa Bunyi Sila Pertama Yang Disahkan?", "id": "Ketuhanan Yang Maha Esa." },
  { "en": "Mengapa Tujuh Kata Dihapus?", "id": "Demi Persatuan (Keberatan Indonesia Timur)." },
  { "en": "Apa Hasil Sidang PPKI (Panitia Persiapan Kemerdekaan Indonesia) Kedua?", "id": "Pembagian Provinsi (Delapan Provinsi)." },
  { "en": "Apa Hasil Sidang PPKI (Panitia Persiapan Kemerdekaan Indonesia) Ketiga?", "id": "Membentuk Badan Keamanan Rakyat (BKR)." },
  { "en": "Apa Nama Cikal Bakal TNI?", "id": "Badan Keamanan Rakyat (BKR)." },
  { "en": "Apa Nama Lain TKR (Tentara Keamanan Rakyat)?", "id": "Tentara Keamanan Rakyat." },
  { "en": "Apa Nama Lain TRI (Tentara Republik Indonesia)?", "id": "Tentara Republik Indonesia." },
  { "en": "Apa Nama Akhir Tentara Indonesia?", "id": "Tentara Nasional Indonesia (TNI)." },
  { "en": "Kapan TNI (Tentara Nasional Indonesia) Berdiri?", "id": "Tiga Juni 1947." },
  { "en": "Siapa Yang Datang Ke Indonesia Setelah Kemerdekaan?", "id": "Sekutu (Inggris) Dan NICA Belanda." },
  { "en": "Apa Tujuan NICA (Netherlands Indies Civil Administration)?", "id": "Ingin Menjajah Indonesia Kembali." },
  { "en": "Apa Peristiwa Perlawanan Di Surabaya?", "id": "Pertempuran 10 November 1945." },
  { "en": "Siapa Tokoh Penyemangat Perlawanan Surabaya?", "id": "Bung Tomo." },
  { "en": "Siapa Pemimpin Pasukan Inggris Yang Tewas?", "id": "Brigadir Jenderal Mallaby." },
  { "en": "Tanggal 10 November Diperingati Hari Apa?", "id": "Hari Pahlawan." },
  { "en": "Apa Peristiwa Perlawanan Di Ambarawa?", "id": "Pertempuran Ambarawa (Palagan Ambarawa)." },
  { "en": "Apa Taktik Perang Kolonel Soedirman?", "id": "Taktik Supit Urang." },
  { "en": "Apa Peristiwa Di Bandung Tahun 1946?", "id": "Peristiwa Bandung Lautan Api." },
  { "en": "Mengapa Bandung Dibakar Oleh Pejuang?", "id": "Agar Tidak Dipakai Oleh Sekutu." },
  { "en": "Apa Perlawanan Di Medan?", "id": "Pertempuran Medan Area." },
  { "en": "Apa Perlawanan Di Bali?", "id": "Perang Puputan Margarana." },
  { "en": "Apa Arti Puputan?", "id": "Perang Habis-habisan." },
  { "en": "Apa Perundingan Pertama Indonesia Belanda?", "id": "Perundingan Linggarjati (1946)." },
  { "en": "Apa Isi Perjanjian Linggarjati?", "id": "Belanda Akui RI (Jawa, Madura, Sumatera)." },
  { "en": "Apa Pelanggaran Belanda Atas Linggarjati?", "id": "Agresi Militer Belanda I (1947)." },
  { "en": "Apa Perundingan Setelah Agresi Militer I?", "id": "Perjanjian Renville (1948)." },
  { "en": "Dimana Perjanjian Renville Dilaksanakan?", "id": "Di Atas Kapal USS Renville." },
  { "en": "Apa Hasil Perjanjian Renville?", "id": "Wilayah RI Semakin Sempit (Garis Van Mook)." },
  { "en": "Apa Pelanggaran Belanda Atas Renville?", "id": "Agresi Militer Belanda II (1948)." },
  { "en": "Apa Sasaran Agresi Militer Belanda II?", "id": "Menangkap Ibu Kota Yogyakarta." },
  { "en": "Siapa Pemimpin RI Yang Ditawan?", "id": "Soekarno, Hatta, Dan Sjahrir." },
  { "en": "Apa Itu Serangan Umum 1 Maret 1949?", "id": "Serangan TNI (Tentara Nasional Indonesia) Ke Yogyakarta." },
  { "en": "Siapa Pemimpin Serangan Umum 1 Maret?", "id": "Letkol Soeharto." },
  { "en": "Apa Perundingan Setelah Serangan Umum?", "id": "Perjanjian Roem-Royen." },
  { "en": "Apa Perundingan Akhir Perang Kemerdekaan?", "id": "Konferensi Meja Bundar (KMB)." },
  { "en": "Apa Bentuk Negara Indonesia (1949-1950)?", "id": "Republik Indonesia Serikat (RIS)." },
  { "en": "Apa Konstitusi RIS (Republik Indonesia Serikat)?", "id": "Konstitusi RIS 1949." },
  { "en": "Apa Konstitusi Yang Berlaku (1950-1959)?", "id": "Undang-Undang Dasar Sementara (UUDS) 1950." },
  { "en": "Apa Ciri Era Demokrasi Parlementer?", "id": "Sering Ganti Kabinet." },
  { "en": "Apa Pemilu Pertama Di Indonesia?", "id": "Pemilu 1955." },
  { "en": "Pemilu 1955 Memilih Siapa?", "id": "Anggota DPR (Dewan Perwakilan Rakyat) Dan Konstituante." },
  { "en": "Apa Tugas Badan Konstituante?", "id": "Menyusun Undang-Undang Dasar Baru." },
  { "en": "Mengapa Konstituante Dianggap Gagal?", "id": "Gagal Mencapai Kata Sepakat (UUD Baru)." },
  { "en": "Apa Tindakan Presiden Soekarno?", "id": "Mengeluarkan Dekrit Presiden." },
  { "en": "Apa Isi Dekrit Presiden 5 Juli 1959?", "id": "Pembubaran Konstituante, Kembali Ke UUD 1945." },
  { "en": "Dekrit Presiden Memulai Era Apa?", "id": "Era Demokrasi Terpimpin." },
  { "en": "Apa Ciri Demokrasi Terpimpin?", "id": "Kekuasaan Presiden Sangat Kuat." },
  { "en": "Apa Itu Trikora (Tri Komando Rakyat)?", "id": "Komando Pembebasan Irian Barat." },
  { "en": "Apa Isi Trikora (Tri Komando Rakyat)?", "id": "Gagalkan Negara Papua, Kibarkan Sang Merah Putih." },
  { "en": "Apa Hasil Pepera (Penentuan Pendapat Rakyat)?", "id": "Irian Barat Bergabung Dengan Indonesia." },
  { "en": "Apa Itu Dwikora (Dwi Komando Rakyat)?", "id": "Komando Konfrontasi Malaysia." },
  { "en": "Mengapa Indonesia Konfrontasi Dengan Malaysia?", "id": "Menolak Pembentukan Negara Federasi Malaysia." },
  { "en": "Kapan Indonesia Keluar Dari PBB?", "id": "Tahun 1965." },
  { "en": "Mengapa Indonesia Keluar Dari PBB?", "id": "Protes Malaysia Masuk Dewan Keamanan PBB." },
  { "en": "Apa Peristiwa Akhir Demokrasi Terpimpin?", "id": "G30S PKI (Tahun 1965)." },
  { "en": "Apa Kepanjangan G30S PKI?", "id": "Gerakan Tiga Puluh September Partai Komunis Indonesia." },
  { "en": "Siapa Korban G30S PKI?", "id": "Tujuh Pahlawan Revolusi (Jenderal TNI)." },
  { "en": "Dimana Jenazah Pahlawan Revolusi Ditemukan?", "id": "Di Lubang Buaya, Jakarta." },
  { "en": "Siapa Jenderal Yang Selamat?", "id": "Jenderal A.H. (Abdul Haris) Nasution." },
  { "en": "Apa Tuntutan Rakyat Setelah G30S?", "id": "Tritura (Tiga Tuntutan Rakyat)." },
  { "en": "Apa Surat Perintah 11 Maret 1966?", "id": "Supersemar (Surat Perintah Sebelas Maret)." },
  { "en": "Siapa Yang Menerima Supersemar?", "id": "Jenderal Soeharto." },
  { "en": "Supersemar Menandai Awal Era Apa?", "id": "Era Orde Baru." },
  { "en": "Apa Era Pemerintahan Soeharto?", "id": "Orde Baru." },
  { "en": "Apa Fokus Pemerintahan Orde Baru?", "id": "Pembangunan Ekonomi (Trilogi Pembangunan)." },
  { "en": "Apa Itu Trilogi Pembangunan?", "id": "Stabilitas, Pertumbuhan, Dan Pemerataan." },
  { "en": "Apa Itu Rencana Pembangunan Orde Baru?", "id": "Repelita (Rencana Pembangunan Lima Tahun)." },
  { "en": "Apa Itu Fusi Partai Politik 1973?", "id": "Penyederhanaan Partai Menjadi Tiga." },
  { "en": "Apa Tiga Partai Pemilu Orde Baru?", "id": "PPP (Partai Persatuan Pembangunan), Golkar, PDI (Partai Demokrasi Indonesia)." },
  { "en": "Apa Itu Asas Tunggal Pancasila?", "id": "Semua Partai Ormas Wajib Berasas Pancasila." },
  { "en": "Apa Itu Penataran P4?", "id": "Program Pedoman Penghayatan Pengamalan Pancasila." },
  { "en": "Apa Itu Dwi Fungsi ABRI?", "id": "Peran Ganda ABRI (Pertahanan Dan Sospol)." },
  { "en": "Kapan ASEAN (Association Of Southeast Asian Nations) Didirikan?", "id": "Tahun 1967 (Deklarasi Bangkok)." },
  { "en": "Apa Program Pengendalian Penduduk Orde Baru?", "id": "Program Keluarga Berencana (KB)." },
  { "en": "Apa Penyebab Krisis Ekonomi 1998?", "id": "Krisis Moneter Asia." },
  { "en": "Apa Peristiwa Tragedi Trisakti?", "id": "Penembakan Mahasiswa Trisakti (Mei 1998)." },
  { "en": "Siapa Pengganti Presiden Soeharto?", "id": "B.J. (Bacharuddin Jusuf) Habibie." },
  { "en": "Apa Itu Kementerian ATR BPN?", "id": "Kementerian Agraria Tata Ruang." },
  { "en": "Apa Tugas Kementerian ATR BPN?", "id": "Mengurus Pertanahan Dan Tata Ruang." },
  { "en": "Apa Itu Tata Ruang?", "id": "Pengaturan Pemanfaatan Lahan Wilayah." },
  { "en": "Apa Itu Sertifikat Tanah?", "id": "Bukti Hukum Sah Kepemilikan Tanah." },
  { "en": "Apa Itu NJOP (Nilai Jual Objek Pajak)?", "id": "Nilai Jual Objek Pajak." },
  { "en": "Apa Kepanjangan NJOP (Nilai Jual Objek Pajak)?", "id": "Nilai Jual Objek Pajak." },
  { "en": "Apa Tugas Kementerian PUPR (Pekerjaan Umum Perumahan Rakyat)?", "id": "Mengurus Infrastruktur Jalan Air Perumahan." },
  { "en": "Apa Itu Jalan Nasional?", "id": "Jalan Utama Penghubung Antar Provinsi." },
  { "en": "Siapa Pengelola Jalan Nasional?", "id": "Pemerintah Pusat (Kementerian PUPR)." },
  { "en": "Apa Itu Jalan Provinsi?", "id": "Jalan Penghubung Ibu Kota Provinsi Kabupaten." },
  { "en": "Siapa Pengelola Jalan Provinsi?", "id": "Pemerintah Provinsi." },
  { "en": "Apa Itu Jalan Kabupaten?", "id": "Jalan Penghubung Antar Kecamatan." },
  { "en": "Siapa Pengelola Jalan Kabupaten?", "id": "Pemerintah Kabupaten." },
  { "en": "Apa Itu Otorita IKN (Ibu Kota Negara)?", "id": "Badan Otorita Ibu Kota Nusantara." },
  { "en": "Siapa Pimpinan Otorita IKN (Ibu Kota Negara)?", "id": "Kepala Otorita IKN." },
  { "en": "Apa Status Otorita IKN (Ibu Kota Negara)?", "id": "Setingkat Kementerian." },
  { "en": "Apa Nama Istana Presiden Di IKN?", "id": "Istana Garuda." },
  { "en": "Apa Itu Rumah Tapak Jabatan Menteri?", "id": "Rumah Dinas Menteri Di IKN." },
  { "en": "Apa Sektor Kementerian Perhubungan (Kemenhub)?", "id": "Sektor Darat, Laut, Udara, Kereta Api." },
  { "en": "Apa Itu Peringatan Dini Bencana?", "id": "Informasi Potensi Terjadinya Bencana." },
  { "en": "Apa Itu Bencana Non-Alam?", "id": "Bencana Akibat Gagal Teknologi Wabah." },
  { "en": "Apa Itu Pengungsian?", "id": "Tempat Penampungan Sementara Korban Bencana." },
  { "en": "Apa Itu Bantuan Logistik Bencana?", "id": "Bantuan Pangan Sandang Obat." },
  { "en": "Apa Itu Relawan Bencana?", "id": "Sukarelawan Penanganan Bencana." },
  { "en": "Apa Tugas PMI (Palang Merah Indonesia)?", "id": "Bantuan Kemanusiaan Dan Donor Darah." },
  { "en": "Apa Itu Donor Darah Sukarela?", "id": "Menyumbangkan Darah Secara Sukarela." },
  { "en": "Apa Itu Unit Donor Darah (UDD)?", "id": "Unit Pengelola Transfusi Darah." },
  { "en": "Apa Kepanjangan UDD (Unit Donor Darah)?", "id": "Unit Donor Darah." },
  { "en": "Apa Itu Golongan Darah A?", "id": "Golongan Darah Tipe A." },
  { "en": "Apa Itu Golongan Darah B?", "id": "Golongan Darah Tipe B." },
  { "en": "Apa Itu Golongan Darah AB?", "id": "Golongan Darah Tipe AB." },
  { "en": "Apa Itu Golongan Darah O?", "id": "Golongan Darah Tipe O." },
  { "en": "Apa Itu Rhesus (Rh) Darah?", "id": "Jenis Protein Darah (Positif Negatif)." },
  { "en": "Apa Itu Transfusi Darah?", "id": "Proses Pemindahan Darah Ke Resipien." },
  { "en": "Siapa Resipien Universal?", "id": "Golongan Darah AB Positif." },
  { "en": "Siapa Donor Universal?", "id": "Golongan Darah O Negatif." },
  { "en": "Apa Kegiatan Utama Posyandu?", "id": "Timbang Bayi, Imunisasi, Penyuluhan Gizi." },
  { "en": "Apa Itu Bidan Desa?", "id": "Bidan Ditempatkan Di Desa." },
  { "en": "Apa Itu Puskesmas Pembantu (Pustu)?", "id": "Jaringan Pelayanan Puskesmas." },
  { "en": "Apa Kepanjangan Pustu?", "id": "Puskesmas Pembantu." },
  { "en": "Apa Penyebab Utama Stunting?", "id": "Kekurangan Gizi Kronis Jangka Panjang." },
  { "en": "Apa Itu Gizi Buruk?", "id": "Kondisi Serius Kekurangan Gizi." },
  { "en": "Apa Itu Pos Gizi?", "id": "Program Pemulihan Gizi Balita." },
  { "en": "Apa Itu Makanan Pendamping ASI (MPASI)?", "id": "Makanan Tambahan Bayi (Setelah 6 Bulan)." },
  { "en": "Apa Kepanjangan MPASI?", "id": "Makanan Pendamping ASI." },
  { "en": "Apa Itu ASI (Air Susu Ibu) Eksklusif?", "id": "Hanya Diberi ASI (Air Susu Ibu) 6 Bulan." },
  { "en": "Apa Kepanjangan ASI (Air Susu Ibu)?", "id": "Air Susu Ibu." },
  { "en": "Apa Itu Bank Darah?", "id": "Tempat Penyimpanan Darah Hasil Donor." },
  { "en": "Apa Itu Bulan Imunisasi Anak Nasional (BIAN)?", "id": "Program Kejar Imunisasi Anak." },
  { "en": "Apa Kepanjangan BIAN?", "id": "Bulan Imunisasi Anak Nasional." },
  { "en": "Apa Itu Kartu Menuju Sehat (KMS)?", "id": "Grafik Pemantauan Tumbuh Kembang Anak." },
  { "en": "Apa Kepanjangan KMS (Kartu Menuju Sehat)?", "id": "Kartu Menuju Sehat." },
  { "en": "Apa Itu Jumantik?", "id": "Juru Pemantau Jentik Nyamuk." },
  { "en": "Apa Fungsi Jumantik?", "id": "Mencegah Demam Berdarah Dengue (DBD)." },
  { "en": "Apa Itu Pemberantasan Sarang Nyamuk (PSN)?", "id": "Kegiatan Memberantas Sarang Nyamuk (3M)." },
  { "en": "Apa Kepanjangan PSN (Pemberantasan Sarang Nyamuk)?", "id": "Pemberantasan Sarang Nyamuk." },
  { "en": "Apa Itu 3M Plus?", "id": "Menutup, Menguras, Mendaur Ulang Plus." },
  { "en": "Apa Itu Abate?", "id": "Bubuk Pembunuh Jentik Nyamuk." },
  { "en": "Apa Itu Fogging?", "id": "Pengasapan Membunuh Nyamuk Dewasa." },
  { "en": "Apa Itu Penyakit Zoonosis?", "id": "Penyakit Hewan Menular Ke Manusia." },
  { "en": "Apa Contoh Zoonosis?", "id": "Rabies, Flu Burung, Leptospirosis." },
  { "en": "Apa Itu Rabies?", "id": "Penyakit Anjing Gila." },
  { "en": "Apa Itu Flu Burung (H5N1)?", "id": "Flu Disebabkan Virus Unggas." },
  { "en": "Apa Itu Karantina Hewan?", "id": "Pengawasan Hewan Mencegah Penyakit." },
  { "en": "Apa Itu Karantina Tumbuhan?", "id": "Pengawasan Tumbuhan Mencegah Hama." },
  { "en": "Apa Itu Kementerian Pertanian (Kementan)?", "id": "Kementerian Urusan Pertanian." },
  { "en": "Apa Tugas Kementan (Kementerian Pertanian)?", "id": "Meningkatkan Produksi Pangan Nasional." },
  { "en": "Apa Itu Swasembada Pangan?", "id": "Mencukupi Kebutuhan Pangan Sendiri." },
  { "en": "Apa Komoditas Pangan Pokok Indonesia?", "id": "Beras (Padi)." },
  { "en": "Apa Itu Diversifikasi Pangan?", "id": "Penganekaragaman Pangan (Non-Beras)." },
  { "en": "Apa Itu Food Estate?", "id": "Pengembangan Pangan Skala Besar." },
  { "en": "Apa Itu Sektor Peternakan?", "id": "Usaha Pemeliharaan Hewan Ternak." },
  { "en": "Apa Itu Sektor Perkebunan?", "id": "Usaha Tanaman Perkebunan (Sawit, Karet)." },
  { "en": "Apa Itu Sektor Hortikultura?", "id": "Usaha Budidaya Buah Sayur." },
  { "en": "Apa Itu Penyuluh Pertanian Lapangan (PPL)?", "id": "Petugas Penyuluh Pertanian." },
  { "en": "Apa Kepanjangan PPL (Penyuluh Pertanian Lapangan)?", "id": "Penyuluh Pertanian Lapangan." },
  { "en": "Apa Itu Kelompok Tani (Poktan)?", "id": "Kelompok Para Petani Di Desa." },
  { "en": "Apa Kepanjangan Poktan?", "id": "Kelompok Tani." },
  { "en": "Apa Itu Gabungan Kelompok Tani (Gapoktan)?", "id": "Gabungan Kelompok Tani." },
  { "en": "Apa Kepanjangan Gapoktan?", "id": "Gabungan Kelompok Tani." },
  { "en": "Apa Itu Pupuk Bersubsidi?", "id": "Pupuk Dibantu Harganya Oleh Pemerintah." },
  { "en": "Apa Jenis Pupuk Bersubsidi?", "id": "Urea Dan NPK (Phonska)." },
  { "en": "Apa Kepanjangan NPK (Nitrogen, Fosfor, Kalium)?", "id": "Nitrogen, Fosfor, Kalium." },
  { "en": "Apa Itu Pupuk Organik?", "id": "Pupuk Dari Bahan Organik (Kompos)." },
  { "en": "Apa Itu Pupuk Anorganik?", "id": "Pupuk Buatan Pabrik (Kimia)." },
  { "en": "Apa Itu Hama Tanaman?", "id": "Organisme Pengganggu Tanaman." },
  { "en": "Apa Contoh Hama Tanaman?", "id": "Wereng, Tikus, Ulat." },
  { "en": "Apa Itu Pestisida?", "id": "Zat Pembasmi Hama." },
  { "en": "Apa Itu Insektisida?", "id": "Pestisida Pembasmi Serangga." },
  { "en": "Apa Itu Herbisida?", "id": "Pestisida Pembasmi Gulma (Rumput)." },
  { "en": "Apa Itu Fungisida?", "id": "Pestisida Pembasmi Jamur." },
  { "en": "Apa Itu Bibit Unggul?", "id": "Bibit Tanaman Kualitas Terbaik." },
  { "en": "Apa Itu Irigasi?", "id": "Sistem Pengairan Sawah." },
  { "en": "Apa Itu Irigasi Tetes?", "id": "Sistem Irigasi Hemat Air (Tetes)." },
  { "en": "Apa Itu Sawah Tadah Hujan?", "id": "Sawah Mengandalkan Air Hujan." },
  { "en": "Apa Itu Musim Tanam?", "id": "Waktu Menanam Padi." },
  { "en": "Apa Itu Musim Panen?", "id": "Waktu Memanen Hasil Pertanian." },
  { "en": "Apa Itu Lumbung Padi?", "id": "Tempat Penyimpanan Padi." },
  { "en": "Daerah Apa Julukan Lumbung Padi Nasional?", "id": "Jawa Barat, Jawa Timur, Jawa Tengah." },
  { "en": "Apa Itu Badan Pangan Nasional (Bapanas)?", "id": "Lembaga Urusan Ketahanan Pangan." },
  { "en": "Apa Itu Badan Urusan Logistik (Bulog)?", "id": "BUMN (Badan Usaha Milik Negara) Logistik Pangan." },
  { "en": "Tugas Bulog (Badan Urusan Logistik)?", "id": "Menjaga Cadangan Beras Pemerintah (CBP)." },
  { "en": "Apa Kepanjangan CBP (Cadangan Beras Pemerintah)?", "id": "Cadangan Beras Pemerintah." },
  { "en": "Apa Itu Harga Eceran Tertinggi (HET) Beras?", "id": "Batas Harga Jual Beras Eceran." },
  { "en": "Siapa Yang Menetapkan HET (Harga Eceran Tertinggi) Beras?", "id": "Badan Pangan Nasional (Bapanas)." },
  { "en": "Apa Itu Operasi Pasar (OP) Beras?", "id": "Intervensi Stabilisasi Harga Beras (Bulog)." },
  { "en": "Apa Kepanjangan OP (Operasi Pasar)?", "id": "Operasi Pasar." },
  { "en": "Apa Itu Program SPHP (Stabilisasi Pasokan Harga Pangan)?", "id": "Stabilisasi Pasokan Harga Pangan." },
  { "en": "Apa Kepanjangan SPHP?", "id": "Stabilisasi Pasokan Harga Pangan." },
  { "en": "Apa Itu Impor Beras?", "id": "Memasukkan Beras Dari Luar Negeri." },
  { "en": "Apa Itu Sektor Perikanan?", "id": "Usaha Penangkapan Budidaya Ikan." },
  { "en": "Apa Itu Perikanan Tangkap?", "id": "Usaha Menangkap Ikan Di Laut." },
  { "en": "Apa Itu Perikanan Budidaya (Akuakultur)?", "id": "Usaha Pemeliharaan Ikan (Tambak, Keramba)." },
  { "en": "Apa Komoditas Ekspor Perikanan Utama?", "id": "Udang, Tuna, Cakalang, Rumput Laut." },
  { "en": "Apa Itu Illegal Fishing?", "id": "Penangkapan Ikan Ilegal." },
  { "en": "Apa Itu Destructive Fishing?", "id": "Penangkapan Ikan Merusak (Bom, Racun)." },
  { "en": "Apa Dampak Destructive Fishing?", "id": "Merusak Ekosistem Terumbu Karang." },
  { "en": "Apa Itu Pukat Harimau (Trawl)?", "id": "Alat Tangkap Ikan Merusak (Dilarang)." },
  { "en": "Apa Itu Cantrang?", "id": "Alat Tangkap Ikan (Mirip Pukat)." },
  { "en": "Apa Itu Surat Izin Penangkapan Ikan (SIPI)?", "id": "Izin Berlayar Menangkap Ikan." },
  { "en": "Apa Kepanjangan SIPI (Surat Izin Penangkapan Ikan)?", "id": "Surat Izin Penangkapan Ikan." },
  { "en": "Apa Itu Wilayah Pengelolaan Perikanan (WPP)?", "id": "Zona Wilayah Pengelolaan Perikanan." },
  { "en": "Apa Kepanjangan WPP (Wilayah Pengelolaan Perikanan)?", "id": "Wilayah Pengelolaan Perikanan." },
  { "en": "Apa Itu Budidaya Rumput Laut?", "id": "Usaha Budidaya Rumput Laut." },
  { "en": "Apa Itu Tambak Udang?", "id": "Kolam Budidaya Udang." },
  { "en": "Apa Itu Keramba Jaring Apung (KJA)?", "id": "Keramba Budidaya Ikan (Danau, Laut)." },
  { "en": "Apa Kepanjangan KJA (Keramba Jaring Apung)?", "id": "Keramba Jaring Apung." },
  { "en": "Apa Itu Garam?", "id": "Hasil Penguapan Air Laut." },
  { "en": "Apa Itu Petani Garam?", "id": "Petani Pembuat Garam." },
  { "en": "Apa Itu Garam Industri?", "id": "Garam Kebutuhan Industri (Kadar NaCl Tinggi)." },
  { "en": "Apa Itu Garam Konsumsi?", "id": "Garam Beryodium Untuk Masak." },
  { "en": "Mengapa Garam Konsumsi Harus Beryodium?", "id": "Mencegah Gangguan Akibat Kekurangan Yodium (GAKY)." },
  { "en": "Apa Kepanjangan GAKY?", "id": "Gangguan Akibat Kekurangan Yodium." },
  { "en": "Apa Penyakit Akibat Kekurangan Yodium?", "id": "Penyakit Gondok." },
  { "en": "Apa Itu Sektor Kehutanan?", "id": "Usaha Pengelolaan Hasil Hutan." },
  { "en": "Apa Hasil Hutan Kayu?", "id": "Kayu (Jati, Meranti, Sengon)." },
  { "en": "Apa Hasil Hutan Bukan Kayu (HHBK)?", "id": "Rotan, Madu, Damar, Getah." },
  { "en": "Apa Kepanjangan HHBK?", "id": "Hasil Hutan Bukan Kayu." },
  { "en": "Apa Itu Hak Pengusahaan Hutan (HPH)?", "id": "Izin Pemanfaatan Hasil Hutan Kayu." },
  { "en": "Apa Kepanjangan HPH (Hak Pengusahaan Hutan)?", "id": "Hak Pengusahaan Hutan." },
  { "en": "Apa Itu Hutan Tanaman Industri (HTI)?", "id": "Hutan Ditanam Untuk Industri (Pulp)." },
  { "en": "Apa Kepanjangan HTI (Hutan Tanaman Industri)?", "id": "Hutan Tanaman Industri." },
  { "en": "Apa Itu Industri Pulp Kertas?", "id": "Industri Bubur Kayu Kertas." },
  { "en": "Apa Itu Hutan Lindung?", "id": "Kawasan Hutan (Fungsi Tata Air)." },
  { "en": "Apa Itu Hutan Produksi?", "id": "Kawasan Hutan (Fungsi Produksi Kayu)." },
  { "en": "Apa Itu Hutan Konservasi?", "id": "Kawasan Hutan (Fungsi Pelestarian)." },
  { "en": "Apa Itu Reboisasi?", "id": "Penanaman Kembali Hutan Gundul." },
  { "en": "Apa Itu Penghijauan?", "id": "Penanaman Lahan Kosong (Luar Hutan)." },
  { "en": "Apa Itu Wanatani (Agroforestry)?", "id": "Sistem Tanam (Pohon Dan Tanaman Pertanian)." },
  { "en": "Apa Itu Perhutanan Sosial?", "id": "Izin Kelola Hutan Oleh Masyarakat." },
  { "en": "Apa Itu Illegal Logging?", "id": "Penebangan Hutan Secara Liar." },
  { "en": "Apa Itu Deforestasi?", "id": "Penggundulan Hutan Skala Besar." },
  { "en": "Apa Itu Kementerian LHK (Lingkungan Hidup Kehutanan)?", "id": "Kementerian Urusan Lingkungan Hidup Kehutanan." },
  { "en": "Apa Kepanjangan KLHK?", "id": "Kementerian Lingkungan Hidup Kehutanan." },
  { "en": "Apa Itu Perum Perhutani?", "id": "BUMN (Badan Usaha Milik Negara) Pengelola Hutan Jawa." },
  { "en": "Apa Itu Polisi Kehutanan (Polhut)?", "id": "Polisi Khusus Perlindungan Hutan." },
  { "en": "Apa Kepanjangan Polhut?", "id": "Polisi Kehutanan." },
  { "en": "Apa Itu Manggala Agni?", "id": "Tim Pemadam Kebakaran Hutan (KLHK)." },
  { "en": "Apa Itu Sektor Pertambangan?", "id": "Usaha Penggalian Bahan Tambang." },
  { "en": "Apa Itu Tambang Batu Bara?", "id": "Penambangan Batu Bara." },
  { "en": "Apa Itu Tambang Emas?", "id": "Penambangan Emas." },
  { "en": "Apa Tambang Emas Terbesar Indonesia?", "id": "Tambang Grasberg (Papua)." },
  { "en": "Siapa Operator Tambang Grasberg?", "id": "PT. (Perseroan Terbatas) Freeport Indonesia." },
  { "en": "Apa Itu Tambang Nikel?", "id": "Penambangan Nikel (Bahan Baku Baterai)." },
  { "en": "Dimana Pusat Tambang Nikel Indonesia?", "id": "Sulawesi Tengah Dan Tenggara." },
  { "en": "Apa Itu Tambang Timah?", "id": "Penambangan Timah." },
  { "en": "Dimana Pusat Tambang Timah Indonesia?", "id": "Bangka Belitung." },
  { "en": "Apa Itu Tambang Minyak Gas (Migas)?", "id": "Penambangan Minyak Bumi Gas Alam." },
  { "en": "Apa Itu Sektor Hulu Migas?", "id": "Eksplorasi Dan Produksi (Pengeboran)." },
  { "en": "Apa Itu Sektor Hilir Migas?", "id": "Pengolahan Dan Penjualan (BBM)." },
  { "en": "Siapa Regulator Hulu Migas?", "id": "SKK (Satuan Kerja Khusus) Migas." },
  { "en": "Siapa Regulator Hilir Migas?", "id": "BPH (Badan Pengatur Hilir) Migas." },
  { "en": "Apa Itu Izin Usaha Pertambangan (IUP)?", "id": "Izin Usaha Pertambangan." },
  { "en": "Apa Kepanjangan IUP (Izin Usaha Pertambangan)?", "id": "Izin Usaha Pertambangan." },
  { "en": "Apa Itu Izin Usaha Pertambangan Khusus (IUPK)?", "id": "Izin Usaha Pertambangan Khusus." },
  { "en": "Apa Kepanjangan IUPK (Izin Usaha Pertambangan Khusus)?", "id": "Izin Usaha Pertambangan Khusus." },
  { "en": "Apa Itu Smelter?", "id": "Pabrik Pengolahan Pemurnian Mineral." },
  { "en": "Apa Itu Hilirisasi Tambang?", "id": "Pengolahan Bahan Tambang Mentah." },
  { "en": "Apa Tujuan Hilirisasi Tambang?", "id": "Meningkatkan Nilai Tambah Ekspor." },
  { "en": "Apa Itu Pertambangan Tanpa Izin (PETI)?", "id": "Penambangan Ilegal." },
  { "en": "Apa Kepanjangan PETI?", "id": "Pertambangan Tanpa Izin." },
  { "en": "Apa Dampak PETI (Pertambangan Tanpa Izin)?", "id": "Kerusakan Lingkungan, Kerugian Negara." },
  { "en": "Apa Itu Reklamasi Tambang?", "id": "Upaya Perbaikan Lahan Bekas Tambang." },
  { "en": "Apa Itu Jaminan Reklamasi (Jamrek)?", "id": "Jaminan Dana Reklamasi Perusahaan." },
  { "en": "Apa Itu Sektor Industri Manufaktur?", "id": "Industri Pengolahan Bahan Baku." },
  { "en": "Apa Itu Industri Tekstil Produk Tekstil (TPT)?", "id": "Industri Tekstil Pakaian Jadi." },
  { "en": "Apa Kepanjangan TPT (Tekstil Produk Tekstil)?", "id": "Tekstil Dan Produk Tekstil." },
  { "en": "Apa Itu Industri Makanan Minuman (Mamin)?", "id": "Industri Makanan Dan Minuman." },
  { "en": "Apa Kepanjangan Mamin?", "id": "Makanan Minuman." },
  { "en": "Apa Itu Industri Otomotif?", "id": "Industri Kendaraan Bermotor." },
  { "en": "Apa Itu Industri Farmasi?", "id": "Industri Obat-obatan." },
  { "en": "Apa Itu Industri Kimia?", "id": "Industri Pengolahan Bahan Kimia." },
  { "en": "Apa Itu Industri Semen?", "id": "Industri Pembuatan Semen." },
  { "en": "Apa Itu Industri Pupuk?", "id": "Industri Pembuatan Pupuk." },
  { "en": "Apa Itu Industri Elektronik?", "id": "Industri Barang Elektronik." },
  { "en": "Apa Itu Industri Galangan Kapal?", "id": "Industri Pembuatan Kapal." },
  { "en": "Apa Itu Industri Alat Berat?", "id": "Industri Pembuatan Mesin Konstruksi." },
  { "en": "Apa Itu Industri Kecil Menengah (IKM)?", "id": "Industri Skala Kecil Menengah." },
  { "en": "Apa Kepanjangan IKM (Industri Kecil Menengah)?", "id": "Industri Kecil Menengah." },
    { "en": "Apa Itu Usaha Mikro Kecil Menengah (UMKM)?", "id": "Usaha Ekonomi Skala Mikro Kecil Menengah." },
  { "en": "Apa Kepanjangan UMKM (Usaha Mikro Kecil Menengah)?", "id": "Usaha Mikro Kecil Menengah." },
  { "en": "Apa Kriteria Usaha Mikro?", "id": "Omzet Maksimal Dua Miliar." },
  { "en": "Apa Kriteria Usaha Kecil?", "id": "Omzet Maksimal Lima Belas Miliar." },
  { "en": "Apa Kriteria Usaha Menengah?", "id": "Omzet Maksimal Lima Puluh Miliar." },
  { "en": "Mengapa UMKM (Usaha Mikro Kecil Menengah) Penting?", "id": "Menyerap Tenaga Kerja, Tahan Krisis." },
  { "en": "Apa Kementerian Pembina UMKM (Usaha Mikro Kecil Menengah)?", "id": "Kementerian Koperasi Dan UKM (Usaha Kecil Menengah)." },
  { "en": "Apa Itu Nomor Induk Berusaha (NIB)?", "id": "Identitas Wajib Pelaku Usaha." },
  { "en": "Apa Kepanjangan NIB (Nomor Induk Berusaha)?", "id": "Nomor Induk Berusaha." },
  { "en": "Dimana Mengurus NIB (Nomor Induk Berusaha)?", "id": "Melalui Sistem OSS (Online Single Submission)." },
  { "en": "Apa Itu Izin Usaha Mikro Kecil (IUMK)?", "id": "Izin Usaha Skala Mikro Kecil." },
  { "en": "Apa Kepanjangan IUMK?", "id": "Izin Usaha Mikro Kecil." },
  { "en": "Apa Itu Kredit Usaha Rakyat (KUR)?", "id": "Kredit Modal Kerja Bersubsidi." },
  { "en": "Apa Kepanjangan KUR (Kredit Usaha Rakyat)?", "id": "Kredit Usaha Rakyat." },
  { "en": "Siapa Penyalur KUR (Kredit Usaha Rakyat)?", "id": "Bank Himbara (Himpunan Bank Milik Negara) Dan Bank Daerah." },
  { "en": "Apa Itu Program Mekaar?", "id": "Program Pembiayaan Ultra Mikro (PNM)." },
  { "en": "Apa Kepanjangan Mekaar?", "id": "Membina Ekonomi Keluarga Sejahtera." },
  { "en": "Siapa Pelaksana Program Mekaar?", "id": "PT. (Perseroan Terbatas) Permodalan Nasional Madani (PNM)." },
  { "en": "Apa Kepanjangan PNM (Permodalan Nasional Madani)?", "id": "Permodalan Nasional Madani." },
  { "en": "Apa Sasaran Program Mekaar?", "id": "Perempuan Pra-Sejahtera Pelaku Usaha." },
  { "en": "Apa Itu Ultra Mikro (UMi)?", "id": "Segmen Usaha Di Bawah Mikro." },
  { "en": "Apa Kepanjangan UMi (Ultra Mikro)?", "id": "Ultra Mikro." },
  { "en": "Apa Itu Warung Tegal (Warteg)?", "id": "Warung Nasi Khas Tegal." },
  { "en": "Apa Kepanjangan Warteg?", "id": "Warung Tegal." },
  { "en": "Apa Itu Pedagang Kaki Lima (PKL)?", "id": "Pedagang Berjualan Di Trotoar." },
  { "en": "Apa Kepanjangan PKL (Pedagang Kaki Lima)?", "id": "Pedagang Kaki Lima." },
  { "en": "Apa Itu Pasar Tradisional?", "id": "Pasar Tempat Tawar Menawar." },
  { "en": "Apa Itu Pasar Modern?", "id": "Pasar Swalayan (Harga Pas)." },
  { "en": "Apa Contoh Pasar Modern?", "id": "Supermarket, Minimarket, Hypermarket." },
  { "en": "Apa Itu E-Commerce?", "id": "Perdagangan Online." },
  { "en": "Apa Contoh E-Commerce Indonesia?", "id": "Tokopedia, Shopee, Bukalapak." },
  { "en": "Apa Itu Mitra Ojol?", "id": "Pengemudi Ojek Online (Status Mitra)." },
  { "en": "Apa Itu Aplikator?", "id": "Perusahaan Penyedia Aplikasi (Gojek, Grab)." },
  { "en": "Apa Itu E-Wallet (Dompet Digital)?", "id": "Uang Elektronik Berbasis Aplikasi." },
  { "en": "Apa Contoh E-Wallet?", "id": "GoPay, OVO, Dana, LinkAja." },
  { "en": "Apa Itu QRIS (Quick Response Code Indonesian Standard)?", "id": "Standar QR (Quick Response) Code Pembayaran Nasional." },
  { "en": "Siapa Yang Mengatur QRIS?", "id": "Bank Indonesia." },
  { "en": "Apa Itu Top Up Saldo?", "id": "Pengisian Saldo Dompet Digital." },
  { "en": "Apa Itu Cash On Delivery (COD)?", "id": "Bayar Di Tempat (Saat Barang Tiba)." },
  { "en": "Apa Kepanjangan COD (Cash On Delivery)?", "id": "Cash On Delivery." },
  { "en": "Apa Itu Marketplace?", "id": "Platform Jual Beli Online (Pasar Online)." },
  { "en": "Apa Itu Social Commerce?", "id": "Jual Beli Lewat Media Sosial." },
  { "en": "Apa Contoh Social Commerce?", "id": "TikTok Shop, Instagram Shopping." },
  { "en": "Apa Itu Afiliator?", "id": "Pihak Mempromosikan Produk (Dapat Komisi)." },
  { "en": "Apa Itu UMKM (Usaha Mikro Kecil Menengah) Go Digital?", "id": "Transformasi UMKM (Usaha Mikro Kecil Menengah) Ke Digital." },
  { "en": "Apa Itu Gerakan Nasional Bangga Buatan Indonesia (Gernas BBI)?", "id": "Gerakan Cinta Produk Lokal." },
  { "en": "Apa Itu Hari UMKM (Usaha Mikro Kecil Menengah) Nasional?", "id": "Tanggal Dua Belas Agustus." },
  { "en": "Apa Itu Ekonomi Kreatif?", "id": "Ekonomi Berbasis Kreativitas." },
  { "en": "Apa Kementerian Ekonomi Kreatif?", "id": "Kemenparekraf (Kementerian Pariwisata Ekonomi Kreatif)." },
  { "en": "Sebutkan Subsektor Ekonomi Kreatif?", "id": "Kuliner, Fashion, Kriya, Film, Musik." },
  { "en": "Apa Itu Kuliner?", "id": "Bidang Usaha Makanan Minuman." },
  { "en": "Apa Itu Fashion?", "id": "Bidang Usaha Pakaian Mode." },
  { "en": "Apa Itu Kriya?", "id": "Kerajinan Tangan (Batik, Tenun)." },
  { "en": "Apa Itu Tenun Ikat?", "id": "Kain Tenun Khas Indonesia." },
  { "en": "Apa Itu Songket?", "id": "Kain Tenun Khas (Palembang, Minangkabau)." },
  { "en": "Apa Itu Ulos?", "id": "Kain Tenun Khas Batak." },
  { "en": "Apa Itu Sektor Film Animasi Video?", "id": "Subsektor Ekonomi Kreatif." },
  { "en": "Apa Itu Sektor Musik?", "id": "Subsektor Industri Musik." },
  { "en": "Apa Itu Sektor Aplikasi Game?", "id": "Subsektor Pengembangan Aplikasi Game." },
  { "en": "Apa Itu E-Sports?", "id": "Olahraga Elektronik (Game Kompetitif)." },
  { "en": "Apa Itu Sektor Penerbitan?", "id": "Industri Penerbitan Buku." },
  { "en": "Apa Itu Sektor Periklanan?", "id": "Industri Jasa Iklan Kreatif." },
  { "en": "Apa Itu Sektor Arsitektur?", "id": "Industri Jasa Desain Bangunan." },
  { "en": "Apa Itu Sektor Desain Interior?", "id": "Industri Jasa Desain Interior Ruangan." },
  { "en": "Apa Itu Sektor Desain Komunikasi Visual (DKV)?", "id": "Desain Grafis, Branding." },
  { "en": "Apa Kepanjangan DKV (Desain Komunikasi Visual)?", "id": "Desain Komunikasi Visual." },
  { "en": "Apa Itu Sektor Fotografi?", "id": "Industri Jasa Fotografi." },
  { "en": "Apa Itu Sektor Seni Pertunjukan?", "id": "Teater, Tari, Pertunjukan Musik." },
  { "en": "Apa Itu Sektor Seni Rupa?", "id": "Lukisan, Patung, Galeri Seni." },
  { "en": "Apa Itu Sektor Televisi Radio?", "id": "Industri Penyiaran Televisi Radio." },
  { "en": "Apa Itu Kekayaan Intelektual (KI)?", "id": "Hak Atas Karya Intelektual." },
  { "en": "Apa Kepanjangan HAKI (Hak Atas Kekayaan Intelektual)?", "id": "Hak Atas Kekayaan Intelektual." },
  { "en": "Apa Itu Hak Cipta?", "id": "Hak Eksklusif Pencipta (Lagu, Buku, Film)." },
  { "en": "Apa Itu Hak Paten?", "id": "Hak Eksklusif Penemu (Invensi Teknologi)." },
  { "en": "Apa Itu Hak Merek?", "id": "Hak Eksklusif Tanda Dagang (Logo, Nama)." },
  { "en": "Apa Itu Indikasi Geografis?", "id": "Tanda Asal Daerah Produk (Kopi Gayo)." },
  { "en": "Dimana Mendaftar Kekayaan Intelektual?", "id": "DJKI (Direktorat Jenderal Kekayaan Intelektual) Kemenkumham." },
  { "en": "Apa Kepanjangan DJKI?", "id": "Direktorat Jenderal Kekayaan Intelektual." },
  { "en": "Apa Itu Royalti?", "id": "Imbalan Penggunaan Hak Cipta Paten." },
  { "en": "Apa Itu Pembajakan?", "id": "Penggunaan Karya Cipta Tanpa Izin." },
  { "en": "Apa Itu Pemalsuan Merek?", "id": "Penggunaan Merek Terdaftar Tanpa Izin." },
  { "en": "Apa Itu Dewan Kerajinan Nasional (Dekranas)?", "id": "Organisasi Pembina Kerajinan Nasional." },
  { "en": "Apa Kepanjangan Dekranas?", "id": "Dewan Kerajinan Nasional." },
  { "en": "Siapa Ketua Umum Dekranas?", "id": "Pasangan Pimpinan Negara (Ibu Negara)." },
  { "en": "Apa Itu Kamar Dagang Industri Indonesia (KADIN)?", "id": "Organisasi Pengusaha Indonesia." },
  { "en": "Apa Kepanjangan KADIN?", "id": "Kamar Dagang Dan Industri Indonesia." },
  { "en": "Apa Itu Himpunan Pengusaha Muda Indonesia (HIPMI)?", "id": "Organisasi Pengusaha Muda." },
  { "en": "Apa Kepanjangan HIPMI?", "id": "Himpunan Pengusaha Muda Indonesia." },
  { "en": "Apa Itu Asosiasi Pengusaha Indonesia (APINDO)?", "id": "Asosiasi Pengusaha Indonesia." },
  { "en": "Apa Kepanjangan APINDO?", "id": "Asosiasi Pengusaha Indonesia." }


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
