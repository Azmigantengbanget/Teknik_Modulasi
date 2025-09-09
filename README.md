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


    { "en": "Apa Proses Menumpangkan Sinyal Informasi?", "id": "Modulasi (Modulation)." },
    { "en": "Apa Sinyal Pembawa Frekuensi Tinggi?", "id": "Gelombang Pembawa (Carrier Wave)." },
    { "en": "Apa Sinyal Informasi Asli?", "id": "Sinyal Baseband Atau Modulasi." },
    { "en": "Kenapa Modulasi Dibutuhkan?", "id": "Untuk Transmisi Sinyal Jarak Jauh." },
    { "en": "Apa Alat Untuk Melakukan Modulasi?", "id": "Modulator." },
    { "en": "Apa Proses Memisahkan Sinyal Informasi?", "id": "Demodulasi (Demodulation)." },
    { "en": "Apa Alat Untuk Melakukan Demodulasi?", "id": "Demodulator." },
    { "en": "Apa Parameter Gelombang Yang Dimodulasi?", "id": "Amplitudo, Frekuensi, Atau Fasa." },
    { "en": "Apa Jenis Modulasi Analog?", "id": "AM (Amplitude Modulation), FM (Frequency Modulation), PM (Phase Modulation)." },
    { "en": "Apa Jenis Modulasi Digital?", "id": "ASK (Amplitude-Shift Keying), FSK (Frequency-Shift Keying), PSK (Phase-Shift Keying)." },
    { "en": "Modulasi Yang Mengubah Amplitudo Pembawa?", "id": "AM (Amplitude Modulation)." },
    { "en": "Apa Bentuk Sinyal AM (Amplitude Modulation)?", "id": "Sampul (Envelope) Menyerupai Sinyal Informasi." },
    { "en": "Apa Frekuensi Di Atas Frekuensi Pembawa?", "id": "Pita Sisi Atas (Upper Sideband)." },
    { "en": "Apa Frekuensi Di Bawah Frekuensi Pembawa?", "id": "Pita Sisi Bawah (Lower Sideband)." },
    { "en": "Apa Ukuran Kedalaman Modulasi AM (Amplitude Modulation)?", "id": "Indeks Modulasi (m)." },
    { "en": "Apa Yang Terjadi Jika Indeks Modulasi > 1?", "id": "Modulasi Berlebih (Overmodulation)." },
    { "en": "Berapa Bandwidth Sinyal AM (Amplitude Modulation)?", "id": "Dua Kali Frekuensi Modulasi Maksimum." },
    { "en": "Apa Jenis AM Dengan Pembawa Ditekan?", "id": "DSB-SC (Double Sideband Suppressed Carrier)." },
    { "en": "Apa Jenis AM Dengan Satu Pita Sisi?", "id": "SSB (Single Sideband)." },
    { "en": "Apa Keuntungan SSB (Single Sideband) Dibanding AM?", "id": "Bandwidth Sempit, Daya Efisien." },
    { "en": "Apa Demodulator AM (Amplitude Modulation) Paling Sederhana?", "id": "Detektor Sampul (Envelope Detector)." },
    { "en": "Modulasi Yang Mengubah Frekuensi Pembawa?", "id": "FM (Frequency Modulation)." },
    { "en": "Apa Keuntungan Utama FM (Frequency Modulation) Dibanding AM?", "id": "Lebih Tahan Terhadap Derau (Noise)." },
    { "en": "Apa Pergeseran Frekuensi Maksimum Dalam FM (Frequency Modulation)?", "id": "Deviasi Frekuensi (Frequency Deviation)." },
    { "en": "Apa Rasio Deviasi Frekuensi Per Frekuensi Modulasi?", "id": "Indeks Modulasi FM (Frequency Modulation) (β)." },
    { "en": "Bagaimana Aturan Menghitung Bandwidth FM (Frequency Modulation)?", "id": "Aturan Carson (Carson's Rule)." },
    { "en": "Apa Sinyal FM Dengan Indeks Modulasi Kecil?", "id": "NFM (Narrowband Frequency Modulation)." },
    { "en": "Apa Sinyal FM Dengan Indeks Modulasi Besar?", "id": "WFM (Wideband Frequency Modulation)." },
    { "en": "Apa Demodulator Umum Untuk Sinyal FM (Frequency Modulation)?", "id": "Diskriminator Frekuensi." },
    { "en": "Apa Demodulator FM (Frequency Modulation) Kinerja Tinggi?", "id": "Demodulator PLL (Phase-Locked Loop)." },
    { "en": "Modulasi Yang Mengubah Fasa Pembawa?", "id": "PM (Phase Modulation)." },
    { "en": "Apa Hubungan PM (Phase Modulation) Dan FM (Frequency Modulation)?", "id": "FM Adalah Integral Dari PM." },
    { "en": "Apa Itu Modulasi Sudut (Angle Modulation)?", "id": "Gabungan Modulasi FM Dan PM." },
    { "en": "Teknik Menaikkan Frekuensi Tinggi Sebelum Transmisi?", "id": "Pre-emphasis." },
    { "en": "Teknik Mengembalikan Respon Frekuensi Di Penerima?", "id": "De-emphasis." },
    { "en": "Apa Tujuan Pre-emphasis Dan De-emphasis?", "id": "Meningkatkan Kinerja Derau (SNR)." },
    { "en": "Apa Modulasi Digital Yang Mengubah Amplitudo?", "id": "ASK (Amplitude-Shift Keying)." },
    { "en": "Apa Bentuk ASK (Amplitude-Shift Keying) Paling Sederhana?", "id": "OOK (On-Off Keying)." },
    { "en": "Apa Modulasi Digital Yang Mengubah Frekuensi?", "id": "FSK (Frequency-Shift Keying)." },
    { "en": "Apa Modulasi Digital Yang Mengubah Fasa?", "id": "PSK (Phase-Shift Keying)." },
    { "en": "Apa PSK (Phase-Shift Keying) Yang Menggunakan Dua Fasa?", "id": "BPSK (Binary Phase-Shift Keying)." },
    { "en": "Berapa Pergeseran Fasa Pada BPSK (Binary Phase-Shift Keying)?", "id": "180 Derajat." },
    { "en": "Apa PSK (Phase-Shift Keying) Yang Menggunakan Empat Fasa?", "id": "QPSK (Quadrature Phase-Shift Keying)." },
    { "en": "Berapa Pergeseran Fasa Pada QPSK (Quadrature Phase-Shift Keying)?", "id": "90 Derajat." },
    { "en": "Apa Representasi Grafis Modulasi Digital?", "id": "Diagram Konstelasi." },
    { "en": "Apa Titik Pada Diagram Konstelasi?", "id": "Titik Simbol." },
    { "en": "Apa Jarak Antara Titik Konstelasi?", "id": "Jarak Euclidean." },
    { "en": "Apa Modulasi Yang Mengubah Amplitudo Dan Fasa?", "id": "QAM (Quadrature Amplitude Modulation)." },
    { "en": "Apa Keuntungan QAM (Quadrature Amplitude Modulation)?", "id": "Efisiensi Spektral Sangat Tinggi." },
    { "en": "Contoh QAM (Quadrature Amplitude Modulation)?", "id": "16-QAM, 64-QAM." },
    { "en": "Apa Itu Laju Simbol (Symbol Rate)?", "id": "Jumlah Simbol Yang Dikirim Per Detik." },
    { "en": "Apa Satuan Laju Simbol?", "id": "Baud." },
    { "en": "Apa Itu Laju Bit (Bit Rate)?", "id": "Jumlah Bit Yang Dikirim Per Detik." },
    { "en": "Bagaimana Hubungan Laju Bit Dan Baud?", "id": "Laju Bit = Baud x Bit Per Simbol." },
    { "en": "Apa Itu Bandwidth?", "id": "Rentang Frekuensi Yang Digunakan Sinyal." },
    { "en": "Apa Itu Efisiensi Spektral?", "id": "Rasio Laju Bit Per Bandwidth." },
    { "en": "Apa Satuan Efisiensi Spektral?", "id": "Bit Per Detik Per Hertz." },
    { "en": "Apa Itu Deteksi Koheren?", "id": "Demodulasi Menggunakan Referensi Fasa Pembawa." },
    { "en": "Apa Itu Deteksi Non-Koheren?", "id": "Demodulasi Tanpa Referensi Fasa Pembawa." },
    { "en": "Apa Masalah Fading Dalam Komunikasi?", "id": "Fluktuasi Kekuatan Sinyal." },
    { "en": "Apa Efek Doppler Dalam Komunikasi?", "id": "Pergeseran Frekuensi Akibat Gerakan." },
    { "en": "Apa Itu Multiplexing?", "id": "Mengirim Banyak Sinyal Lewat Satu Kanal." },
    { "en": "Multiplexing Berbasis Pembagian Frekuensi?", "id": "FDM (Frequency-Division Multiplexing)." },
    { "en": "Multiplexing Berbasis Pembagian Waktu?", "id": "TDM (Time-Division Multiplexing)." },
    { "en": "Apa Itu Kanal Komunikasi?", "id": "Medium Transmisi Sinyal." },
    { "en": "Apa Itu Derau (Noise)?", "id": "Sinyal Acak Yang Tidak Diinginkan." },
    { "en": "Apa Model Derau Paling Umum?", "id": "AWGN (Additive White Gaussian Noise)." },
    { "en": "Apa Itu Interferensi?", "id": "Gangguan Dari Sinyal Lain." },
    { "en": "Apa Itu Distorsi?", "id": "Perubahan Bentuk Gelombang Sinyal." },
    { "en": "Apa Itu Atenuasi (Attenuation)?", "id": "Pelemahan Kekuatan Sinyal." },
    { "en": "Apa Itu Diagram Mata (Eye Diagram)?", "id": "Alat Visualisasi Kualitas Sinyal Digital." },
    { "en": "Apa Yang Diindikasikan Mata Terbuka Lebar?", "id": "Sinyal Berkualitas Baik." },
    { "en": "Apa Itu Interferensi Antar Simbol (ISI)?", "id": "Simbol Mengganggu Simbol Berikutnya." },
    { "en": "Apa Penyebab ISI (Inter-Symbol Interference)?", "id": "Distorsi Kanal (Multipath Fading)." },
    { "en": "Apa Itu Equalizer?", "id": "Filter Untuk Mengurangi Efek ISI." },
    { "en": "Modulasi Yang Tahan Terhadap Fading?", "id": "Spread Spectrum." },
    { "en": "Teknik Menyebar Sinyal Ke Frekuensi Luas?", "id": "Spread Spectrum." },
    { "en": "Contoh Spread Spectrum?", "id": "DSSS (Direct-Sequence Spread Spectrum) dan FHSS (Frequency-Hopping Spread Spectrum)." },
    { "en": "Apa Itu Kode Chip Dalam DSSS (Direct-Sequence Spread Spectrum)?", "id": "Kode Penyebar Sinyal." },
    { "en": "Apa Itu Pola Lompatan Dalam FHSS (Frequency-Hopping Spread Spectrum)?", "id": "Urutan Frekuensi Yang Digunakan." },
    { "en": "Apa Itu Metode Akses Ganda?", "id": "Berbagi Kanal Oleh Banyak Pengguna." },
    { "en": "Akses Ganda Berbasis Frekuensi?", "id": "FDMA (Frequency-Division Multiple Access)." },
    { "en": "Akses Ganda Berbasis Waktu?", "id": "TDMA (Time-Division Multiple Access)." },
    { "en": "Akses Ganda Berbasis Kode?", "id": "CDMA (Code-Division Multiple Access)." },
    { "en": "Teknologi Mana Yang Menggunakan CDMA (Code-Division Multiple Access)?", "id": "Sistem Seluler Generasi Awal (3G)." },
    { "en": "Modulasi Multi-pembawa?", "id": "OFDM (Orthogonal Frequency-Division Multiplexing)." },
    { "en": "Apa Keuntungan OFDM (Orthogonal Frequency-Division Multiplexing)?", "id": "Sangat Tahan Terhadap Multipath Fading." },
    { "en": "Di Mana OFDM (Orthogonal Frequency-Division Multiplexing) Digunakan?", "id": "Wi-Fi, 4G LTE, 5G NR." },
    { "en": "Apa Itu Awalan Siklik (Cyclic Prefix)?", "id": "Melindungi Simbol OFDM Dari ISI." },
    { "en": "Apa Rasio Daya Puncak Per Rata-rata?", "id": "PAPR (Peak-to-Average Power Ratio)." },
    { "en": "Modulasi Mana Yang Punya PAPR (Peak-to-Average Power Ratio) Tinggi?", "id": "OFDM (Orthogonal Frequency-Division Multiplexing)." },
    { "en": "Apa Itu Sinkronisasi?", "id": "Menyelaraskan Penerima Dengan Pemancar." },
    { "en": "Apa Itu Pemulihan Pembawa (Carrier Recovery)?", "id": "Mengekstrak Frekuensi Dan Fasa Pembawa." },
    { "en": "Apa Itu Pemulihan Clock (Clock Recovery)?", "id": "Mengekstrak Waktu Simbol." },
    { "en": "Apa Itu Kapasitas Kanal?", "id": "Laju Data Maksimum Teoritis Kanal." },
    { "en": "Siapa Yang Merumuskan Kapasitas Kanal?", "id": "Claude Shannon." },
    { "en": "Apa Itu Pengkodean Kanal (Channel Coding)?", "id": "Menambahkan Redundansi Untuk Koreksi Error." },
    { "en": "Apa Itu Pengkodean Sumber (Source Coding)?", "id": "Mengompres Data Sebelum Transmisi." },
    { "en": "Apa Itu Modulator Seimbang (Balanced Modulator)?", "id": "Modulator Yang Menekan Sinyal Pembawa." },
    { "en": "Apa Hasil Dari Modulator Seimbang?", "id": "Sinyal DSB-SC (Double Sideband Suppressed Carrier)." },
    { "en": "Bagaimana Cara Kerja Detektor Sinkron?", "id": "Mengalikan Sinyal Dengan Pembawa Lokal." },
    { "en": "Apa Nama Lain Detektor Sinkron?", "id": "Detektor Koheren." },
    { "en": "Apa Kesulitan Utama Deteksi Koheren?", "id": "Perlu Sinkronisasi Fasa Yang Tepat." },
    { "en": "Apa Filter Yang Digunakan Setelah Detektor Sinkron?", "id": "Filter Lolos Bawah (Low-Pass Filter)." },
    { "en": "Apa Itu Modulasi Pita Sisi Vestigial (Vestigial Sideband Modulation)?", "id": "VSB (Vestigial Sideband Modulation)." },
    { "en": "Di Mana VSB (Vestigial Sideband Modulation) Digunakan?", "id": "Transmisi Sinyal TV Analog." },
    { "en": "Apa Keuntungan VSB (Vestigial Sideband Modulation)?", "id": "Menghemat Bandwidth Dibandingkan AM." },
    { "en": "Apa Itu Penerima Superheterodyne?", "id": "Penerima Yang Mengubah Frekuensi Sinyal." },
    { "en": "Apa Frekuensi Hasil Konversi Di Penerima?", "id": "IF (Intermediate Frequency)." },
    { "en": "Apa Keuntungan Arsitektur Superheterodyne?", "id": "Selektivitas Dan Sensitivitas Tinggi." },
    { "en": "Apa Itu Frekuensi Cermin (Image Frequency)?", "id": "Frekuensi Yang Tidak Diinginkan." },
    { "en": "Bagaimana Cara Menekan Frekuensi Cermin?", "id": "Menggunakan Filter RF (Radio Frequency) Di Awal." },
    { "en": "Apa Fungsi Osilator Lokal (Local Oscillator)?", "id": "Menghasilkan Sinyal Untuk Proses Pencampuran." },
    { "en": "Apa Fungsi Mixer Dalam Penerima?", "id": "Mengalikan Sinyal RF Dan LO." },
    { "en": "Modulasi Yang Membutuhkan Bandwidth Paling Lebar?", "id": "WFM (Wideband Frequency Modulation)." },
    { "en": "Modulasi Yang Membutuhkan Bandwidth Paling Sempit?", "id": "SSB (Single Sideband)." },
    { "en": "Apa Modulasi Yang Paling Tahan Derau?", "id": "FM (Frequency Modulation)." },
    { "en": "Apa Modulasi Yang Paling Rentan Derau?", "id": "AM (Amplitude Modulation)." },
    { "en": "Apa Itu DPSK (Differential Phase-Shift Keying)?", "id": "Fasa Direferensikan Ke Simbol Sebelumnya." },
    { "en": "Apa Keuntungan DPSK (Differential Phase-Shift Keying)?", "id": "Tidak Memerlukan Deteksi Koheren." },
    { "en": "Apa Itu MSK (Minimum-Shift Keying)?", "id": "Jenis FSK (Frequency-Shift Keying) Fase Kontinu." },
    { "en": "Apa Keuntungan MSK (Minimum-Shift Keying)?", "id": "Efisiensi Spektral Baik." },
    { "en": "Apa Itu GMSK (Gaussian Minimum-Shift Keying)?", "id": "MSK Dengan Filter Gaussian." },
    { "en": "Di Mana GMSK (Gaussian Minimum-Shift Keying) Digunakan?", "id": "Sistem Seluler GSM (Global System for Mobile Communications)." },
    { "en": "Apa Itu Modulasi Bertingkat (M-ary Modulation)?", "id": "Mengirim Lebih Dari Satu Bit Per Simbol." },
    { "en": "Contoh Modulasi M-ary?", "id": "QPSK (Quadrature Phase-Shift Keying), 8-PSK, 16-QAM." },
    { "en": "Apa Itu 8-PSK (8-Phase Shift Keying)?", "id": "PSK Yang Menggunakan Delapan Fasa." },
    { "en": "Berapa Bit Per Simbol Pada 8-PSK?", "id": "3 Bit Per Simbol." },
    { "en": "Apa Itu 16-QAM (16-Quadrature Amplitude Modulation)?", "id": "QAM Dengan 16 Titik Konstelasi." },
    { "en": "Berapa Bit Per Simbol Pada 16-QAM?", "id": "4 Bit Per Simbol." },
    { "en": "Apa Konsekuensi Modulasi Orde Tinggi?", "id": "Lebih Rentan Terhadap Derau." },
    { "en": "Apa Itu Modulasi Trellis-Coded (Trellis-Coded Modulation)?", "id": "TCM (Trellis-Coded Modulation)." },
    { "en": "Apa Fungsi TCM (Trellis-Coded Modulation)?", "id": "Menggabungkan Modulasi Dengan Pengkodean Kanal." },
    { "en": "Apa Itu Pembentukan Pulsa (Pulse Shaping)?", "id": "Membentuk Gelombang Simbol Digital." },
    { "en": "Apa Tujuan Pembentukan Pulsa?", "id": "Mengurangi Interferensi Antar Simbol (ISI)." },
    { "en": "Contoh Filter Pembentukan Pulsa?", "id": "Filter Raised-Cosine." },
    { "en": "Apa Itu Faktor Roll-off?", "id": "Parameter Filter Raised-Cosine." },
    { "en": "Apa Itu Bandwidth Nyquist?", "id": "Bandwidth Minimum Teoritis Tanpa ISI." },
    { "en": "Apa Itu Modulasi Adaptif?", "id": "Mengubah Skema Modulasi Sesuai Kondisi." },
    { "en": "Kapan Modulasi Orde Rendah Digunakan?", "id": "Saat Kondisi Kanal Buruk (SNR Rendah)." },
    { "en": "Kapan Modulasi Orde Tinggi Digunakan?", "id": "Saat Kondisi Kanal Baik (SNR Tinggi)." },
    { "en": "Apa Itu Sinyal In-phase (I)?", "id": "Komponen Sinyal Yang Sefasa Pembawa." },
    { "en": "Apa Itu Sinyal Quadrature (Q)?", "id": "Komponen Sinyal Beda Fasa 90°." },
    { "en": "Apa Itu Modulator I/Q?", "id": "Modulator Menggunakan Sinyal I Dan Q." },
    { "en": "Apa Itu Demodulator I/Q?", "id": "Demodulator Menggunakan Sinyal I Dan Q." },
    { "en": "Apa Itu Ketidakseimbangan I/Q (I/Q Imbalance)?", "id": "Kesalahan Amplitudo Atau Fasa I/Q." },
    { "en": "Apa Akibat Ketidakseimbangan I/Q?", "id": "Menurunkan Kinerja Modulasi." },
    { "en": "Apa Itu Kebocoran LO (LO Leakage)?", "id": "Sinyal Osilator Lokal Bocor Ke Output." },
    { "en": "Apa Itu Penekanan Pita Sisi (Sideband Suppression)?", "id": "Ukuran Kualitas Modulator SSB." },
    { "en": "Apa Itu Modulasi Denyut Nadi (Pulse Modulation)?", "id": "Informasi Dalam Parameter Pulsa." },
    { "en": "Apa Itu PAM (Pulse-Amplitude Modulation)?", "id": "Informasi Dalam Amplitudo Pulsa." },
    { "en": "Apa Itu PWM (Pulse-Width Modulation)?", "id": "Informasi Dalam Lebar Pulsa." },
    { "en": "Apa Itu PPM (Pulse-Position Modulation)?", "id": "Informasi Dalam Posisi Pulsa." },
    { "en": "Apa Itu PCM (Pulse-Code Modulation)?", "id": "Representasi Digital Sinyal Analog." },
    { "en": "Apa Tiga Langkah Dasar PCM (Pulse-Code Modulation)?", "id": "Sampling, Kuantisasi, Pengkodean." },
    { "en": "Apa Itu Kuantisasi (Quantization)?", "id": "Pembulatan Nilai Sampel Ke Level Terdekat." },
    { "en": "Apa Itu Derau Kuantisasi (Quantization Noise)?", "id": "Kesalahan Akibat Proses Kuantisasi." },
    { "en": "Bagaimana Cara Mengurangi Derau Kuantisasi?", "id": "Meningkatkan Jumlah Level Kuantisasi." },
    { "en": "Apa Itu Pengkodean (Encoding)?", "id": "Memberi Kode Biner Pada Level Kuantisasi." },
    { "en": "Apa Itu Companding?", "id": "Mengompres Sinyal Sebelum Kuantisasi." },
    { "en": "Apa Tujuan Companding?", "id": "Meningkatkan SNR Untuk Sinyal Amplitudo Rendah." },
    { "en": "Contoh Hukum Companding?", "id": "μ-Law (Amerika Utara) Dan A-Law (Eropa)." },
    { "en": "Apa Itu Modulasi Delta?", "id": "Hanya Mengirim Perbedaan Antar Sampel." },
    { "en": "Apa Masalah Dalam Modulasi Delta?", "id": "Slope Overload Dan Granular Noise." },
    { "en": "Apa Itu DPCM (Differential Pulse-Code Modulation)?", "id": "PCM Yang Mengkodekan Selisih." },
    { "en": "Apa Itu ADPCM (Adaptive Differential Pulse-Code Modulation)?", "id": "DPCM Dengan Ukuran Langkah Adaptif." },
    { "en": "Apa Itu Kode Baris (Line Coding)?", "id": "Representasi Sinyal Digital Dalam Baseband." },
    { "en": "Contoh Kode Baris?", "id": "NRZ (Non-Return-to-Zero), RZ (Return-to-Zero), Manchester." },
    { "en": "Apa Keuntungan Kode Manchester?", "id": "Memiliki Komponen Clock Terintegrasi." },
    { "en": "Apa Itu Simbol?", "id": "Bentuk Gelombang Unik Yang Mewakili Data." },
    { "en": "Modulasi Mana Yang Paling Sederhana?", "id": "ASK (Amplitude-Shift Keying) / OOK (On-Off Keying)." },
    { "en": "Apa Perbedaan Antara Baud Dan bps?", "id": "Baud Adalah Laju Simbol, bps Laju Bit." },
    { "en": "Kapan Baud Sama Dengan bps?", "id": "Saat Setiap Simbol Mewakili Satu Bit." },
    { "en": "Modulasi Apa Yang Baud = bps?", "id": "BPSK (Binary Phase-Shift Keying)." },
    { "en": "Apa Itu Modulasi Koheren?", "id": "Membutuhkan Pemulihan Fasa Pembawa." },
    { "en": "Apa Itu Modulasi Non-Koheren?", "id": "Tidak Membutuhkan Pemulihan Fasa." },
    { "en": "Demodulator Mana Yang Lebih Sederhana?", "id": "Demodulator Non-Koheren." },
    { "en": "Demodulator Mana Yang Kinerjanya Lebih Baik?", "id": "Demodulator Koheren." },
    { "en": "Apa Itu BER (Bit Error Rate)?", "id": "Rasio Bit Error Per Total Bit." },
    { "en": "Apa Itu Kurva Air Terjun (Waterfall Curve)?", "id": "Grafik BER vs. SNR." },
    { "en": "Apa Itu Batas Shannon?", "id": "Batas Kapasitas Kanal Teoritis." },
    { "en": "Apa Itu Pengkodean Konvolusi?", "id": "Jenis Kode Koreksi Error." },
    { "en": "Apa Itu Dekoder Viterbi?", "id": "Dekoder Untuk Kode Konvolusi." },
    { "en": "Apa Itu Kode Turbo?", "id": "Kode Koreksi Error Kinerja Tinggi." },
    { "en": "Apa Itu Kode LDPC (Low-Density Parity-Check)?", "id": "Kode Koreksi Error Mendekati Batas Shannon." },
    { "en": "Apa Itu Interleaving?", "id": "Mengacak Urutan Bit Untuk Melawan Burst Error." },
    { "en": "Apa Itu Burst Error?", "id": "Error Yang Terjadi Secara Berkelompok." },
    { "en": "Apa Itu Fading Multipath?", "id": "Sinyal Datang Dari Banyak Lintasan." },
    { "en": "Apa Itu Delay Spread?", "id": "Perbedaan Waktu Tiba Antar Lintasan." },
    { "en": "Apa Akibat Delay Spread?", "id": "Interferensi Antar Simbol (ISI)." },
    { "en": "Apa Itu Rake Receiver?", "id": "Penerima Untuk Melawan Efek Multipath." },
    { "en": "Di Mana Rake Receiver Digunakan?", "id": "Sistem CDMA (Code-Division Multiple Access)." },
    { "en": "Apa Itu Diversity?", "id": "Teknik Menggunakan Banyak Jalur Komunikasi." },
    { "en": "Apa Itu Space Diversity?", "id": "Menggunakan Beberapa Antena Terpisah." },
    { "en": "Apa Itu Frequency Diversity?", "id": "Mengirim Sinyal Pada Frekuensi Berbeda." },
    { "en": "Apa Itu Time Diversity?", "id": "Mengirim Ulang Sinyal Pada Waktu Berbeda." },
    { "en": "Apa Itu Modulasi Digital Paling Dasar?", "id": "OOK (On-Off Keying)." },
    { "en": "Apa Itu Modulasi Kunci Geser Amplitudo?", "id": "ASK (Amplitude-Shift Keying)." },
    { "en": "Apa Itu Modulasi Kunci Geser Frekuensi?", "id": "FSK (Frequency-Shift Keying)." },
    { "en": "Apa Itu Modulasi Kunci Geser Fasa?", "id": "PSK (Phase-Shift Keying)." },
    { "en": "Apa Efek Derau Pada Amplitudo Sinyal?", "id": "Mengubah Amplitudo Secara Acak." },
    { "en": "Modulasi Mana Yang Paling Terpengaruh Derau Amplitudo?", "id": "AM (Amplitude Modulation) dan ASK (Amplitude-Shift Keying)." },
    { "en": "Apa Itu Modulasi Amplitudo Kuadratur?", "id": "QAM (Quadrature Amplitude Modulation)." },
    { "en": "Apa Yang Berubah Pada Sinyal QAM (Quadrature Amplitude Modulation)?", "id": "Amplitudo Dan Fasa Sekaligus." },
    { "en": "Apa Keunggulan Utama QAM (Quadrature Amplitude Modulation)?", "id": "Sangat Efisien Dalam Penggunaan Bandwidth." },
    { "en": "Bagaimana Simbol Ditampilkan Dalam Modulasi Digital?", "id": "Diagram Konstelasi." },
    { "en": "Berapa Bit Per Simbol Untuk 256-QAM?", "id": "8 Bit Per Simbol." },
    { "en": "Apa Itu Gray Coding Dalam Konstelasi?", "id": "Simbol Berdekatan Hanya Beda Satu Bit." },
    { "en": "Apa Tujuan Gray Coding?", "id": "Mengurangi Bit Error Rate (BER)." },
    { "en": "Apa Itu Pemulihan Fasa Pembawa?", "id": "Carrier Recovery." },
    { "en": "Kenapa Carrier Recovery Dibutuhkan?", "id": "Untuk Demodulasi Koheren (PSK/QAM)." },
    { "en": "Apa Sirkuit Untuk Pemulihan Fasa Pembawa?", "id": "PLL (Phase-Locked Loop)." },
    { "en": "Apa Itu Kesalahan Fasa (Phase Error)?", "id": "Perbedaan Fasa Pembawa Lokal-Diterima." },
    { "en": "Apa Akibat Kesalahan Fasa?", "id": "Rotasi Konstelasi Dan Bit Error." },
    { "en": "Apa Itu Filter Pencocok (Matched Filter)?", "id": "Filter Optimal Untuk Mendeteksi Sinyal." },
    { "en": "Apa Fungsi Matched Filter Di Penerima?", "id": "Memaksimalkan SNR (Signal-to-Noise Ratio)." },
    { "en": "Apa Itu Proses Korelasi Silang?", "id": "Mengukur Kemiripan Antara Dua Sinyal." },
    { "en": "Apa Itu Orthogonality (Ortogonalitas)?", "id": "Dua Sinyal Tidak Saling Mengganggu." },
    { "en": "Modulasi Mana Yang Menggunakan Sinyal Ortogonal?", "id": "OFDM (Orthogonal Frequency-Division Multiplexing) Dan QAM (Quadrature Amplitude Modulation)." },
    { "en": "Apa Itu Sinyal I (In-phase)?", "id": "Komponen Sinyal Sumbu Horizontal." },
    { "en": "Apa Itu Sinyal Q (Quadrature)?", "id": "Komponen Sinyal Sumbu Vertikal." },
    { "en": "Apa Itu Baseband Kompleks?", "id": "Representasi Sinyal Menggunakan I Dan Q." },
    { "en": "Apa Itu Pengambilan Sampel (Sampling)?", "id": "Mengubah Sinyal Kontinu Ke Diskrit." },
    { "en": "Apa Itu Teorema Nyquist-Shannon?", "id": "Frekuensi Sampling Minimal Dua Kali." },
    { "en": "Apa Itu Aliasing?", "id": "Distorsi Akibat Frekuensi Sampling Rendah." },
    { "en": "Bagaimana Mencegah Aliasing?", "id": "Menggunakan Filter Anti-Aliasing." },
    { "en": "Apa Bentuk Filter Anti-Aliasing?", "id": "Filter Lolos Bawah (Low-Pass Filter)." },
    { "en": "Apa Itu Spektrum Sinyal?", "id": "Representasi Sinyal Dalam Domain Frekuensi." },
    { "en": "Apa Alat Untuk Melihat Spektrum?", "id": "Spectrum Analyzer." },
    { "en": "Apa Itu Transformasi Fourier?", "id": "Mengubah Sinyal Dari Domain Waktu-Frekuensi." },
    { "en": "Apa Itu Sinyal Periodik?", "id": "Sinyal Yang Berulang Secara Teratur." },
    { "en": "Apa Komponen Spektrum Sinyal Periodik?", "id": "Frekuensi Fundamental Dan Harmonik." },
    { "en": "Apa Itu Sinyal Pita Dasar (Baseband)?", "id": "Sinyal Berpusat Di Sekitar Frekuensi Nol." },
    { "en": "Apa Itu Sinyal Pita Lolos (Passband)?", "id": "Sinyal Berpusat Di Frekuensi Pembawa." },
    { "en": "Apa Itu Konversi Naik (Up-conversion)?", "id": "Menggeser Sinyal Baseband Ke Passband." },
    { "en": "Apa Itu Konversi Turun (Down-conversion)?", "id": "Menggeser Sinyal Passband Ke Baseband." },
    { "en": "Apa Komponen Untuk Konversi Frekuensi?", "id": "Mixer Dan Osilator Lokal." },
    { "en": "Apa Itu Modulasi Digital Linear?", "id": "Amplitudo Sinyal Proporsional Data Baseband." },
    { "en": "Contoh Modulasi Linear?", "id": "PSK (Phase-Shift Keying) Dan QAM (Quadrature Amplitude Modulation)." },
    { "en": "Apa Itu Modulasi Amplop Konstan?", "id": "Amplitudo Sinyal Selalu Konstan." },
    { "en": "Contoh Modulasi Amplop Konstan?", "id": "FSK (Frequency-Shift Keying) Dan GMSK (Gaussian Minimum-Shift Keying)." },
    { "en": "Apa Keuntungan Amplop Konstan?", "id": "Dapat Menggunakan Penguat Non-Linear." },
    { "en": "Apa Itu Penguat Non-Linear?", "id": "Penguat Efisien Tapi Dapat Merusak Amplitudo." },
    { "en": "Apa Itu Distorsi AM-ke-AM?", "id": "Perubahan Amplitudo Akibat Non-Linearitas." },
    { "en": "Apa Itu Distorsi AM-ke-PM?", "id": "Perubahan Fasa Akibat Perubahan Amplitudo." },
    { "en": "Apa Itu Back-off Daya?", "id": "Mengurangi Daya Untuk Operasi Linear." },
    { "en": "Apa Itu Linearisasi Penguat (Amplifier Linearization)?", "id": "Teknik Untuk Mengurangi Distorsi." },
    { "en": "Contoh Teknik Linearisasi?", "id": "Pre-distorsi Digital (Digital Pre-Distortion)." },
    { "en": "Apa Itu Penyiaran (Broadcasting)?", "id": "Transmisi Dari Satu Titik Ke Banyak." },
    { "en": "Apa Itu Unicasting?", "id": "Transmisi Dari Satu Titik Ke Satu Titik." },
    { "en": "Apa Itu Multicasting?", "id": "Transmisi Satu Titik Ke Grup Tertentu." },
    { "en": "Apa Itu Dupleks (Duplex)?", "id": "Komunikasi Dua Arah." },
    { "en": "Apa Itu Simplex?", "id": "Komunikasi Satu Arah Saja." },
    { "en": "Apa Itu Half-Duplex?", "id": "Dua Arah Tapi Secara Bergantian." },
    { "en": "Apa Itu Full-Duplex?", "id": "Dua Arah Secara Bersamaan." },
    { "en": "Bagaimana Mencapai Full-Duplex?", "id": "Menggunakan Frekuensi Atau Waktu Berbeda." },
    { "en": "Apa Itu FDD (Frequency-Division Duplex)?", "id": "Dupleks Dengan Frekuensi Terpisah." },
    { "en": "Apa Itu TDD (Time-Division Duplex)?", "id": "Dupleks Dengan Slot Waktu Terpisah." },
    { "en": "Apa Itu Modem?", "id": "Singkatan Dari Modulator-Demodulator." },
    { "en": "Apa Itu Transceiver?", "id": "Gabungan Dari Transmitter Dan Receiver." },
    { "en": "Apa Itu Transponder?", "id": "Transceiver Yang Merespon Otomatis." },
    { "en": "Apa Itu Sinyal Analog?", "id": "Sinyal Dengan Nilai Kontinu." },
    { "en": "Apa Itu Sinyal Digital?", "id": "Sinyal Dengan Nilai Diskret." },
    { "en": "Apa Itu Bit?", "id": "Unit Dasar Informasi Digital." },
    { "en": "Apa Itu Byte?", "id": "Kumpulan Delapan Bit." },
    { "en": "Apa Itu Probabilitas Error Simbol?", "id": "SER (Symbol Error Rate)." },
    { "en": "Mana Yang Biasanya Lebih Tinggi, SER atau BER?", "id": "SER (Symbol Error Rate)." },
    { "en": "Apa Itu Kanal AWGN (Additive White Gaussian Noise)?", "id": "Kanal Ideal Dengan Derau Gaussian." },
    { "en": "Apa Itu Kanal Fading Rayleigh?", "id": "Model Kanal Tanpa Jalur Langsung." },
    { "en": "Apa Itu Kanal Fading Rician?", "id": "Model Kanal Dengan Jalur Langsung." },
    { "en": "Apa Itu Koherensi Waktu (Coherence Time)?", "id": "Durasi Kanal Dianggap Konstan." },
    { "en": "Apa Itu Koherensi Bandwidth (Coherence Bandwidth)?", "id": "Rentang Frekuensi Dengan Fading Serupa." },
    { "en": "Apa Itu Fading Datar (Flat Fading)?", "id": "Fading Mempengaruhi Semua Frekuensi Sama." },
    { "en": "Apa Itu Fading Selektif Frekuensi?", "id": "Fading Mempengaruhi Frekuensi Berbeda." },
    { "en": "Apa Itu Doppler Spread?", "id": "Penyebaran Spektrum Akibat Efek Doppler." },
    { "en": "Apa Itu Fading Cepat (Fast Fading)?", "id": "Kanal Berubah Lebih Cepat Dari Sinyal." },
    { "en": "Apa Itu Fading Lambat (Slow Fading)?", "id": "Kanal Berubah Lebih Lambat Dari Sinyal." },
    { "en": "Apa Itu CSI (Channel State Information)?", "id": "Informasi Karakteristik Kanal Komunikasi." },
    { "en": "Bagaimana Penerima Memperoleh CSI (Channel State Information)?", "id": "Menggunakan Sinyal Pilot Atau Referensi." },
    { "en": "Apa Itu Estimasi Kanal?", "id": "Proses Memperkirakan Respons Kanal." },
    { "en": "Apa Itu Scrambling?", "id": "Mengacak Urutan Data Digital." },
    { "en": "Apa Tujuan Scrambling?", "id": "Menghindari Urutan Bit Berulang Panjang." },
    { "en": "Apa Itu Diferensial Encoding?", "id": "Mengkodekan Informasi Dalam Perubahan." },
    { "en": "Apa Itu Modulasi Orde Lebih Tinggi (Higher-Order Modulation)?", "id": "Modulasi Dengan Banyak Titik Konstelasi." },
    { "en": "Apa Itu API (Application Programming Interface) Untuk Radio?", "id": "SDR (Software-Defined Radio)." },
    { "en": "Apa Keuntungan SDR (Software-Defined Radio)?", "id": "Sangat Fleksibel Dan Dapat Diprogram." },
    { "en": "Apa Radio Yang Dapat Beradaptasi?", "id": "Radio Kognitif (Cognitive Radio)." },
    { "en": "Apa Fungsi Radio Kognitif?", "id": "Mendeteksi Dan Menggunakan Spektrum Kosong." },
    { "en": "Apa Itu Deteksi Energi?", "id": "Metode Sederhana Mendeteksi Kehadiran Sinyal." },
    { "en": "Apa Itu Modulasi Denyut Amplitudo?", "id": "PAM (Pulse-Amplitude Modulation)." },
    { "en": "Apa Itu Modulasi Denyut Lebar?", "id": "PWM (Pulse-Width Modulation)." },
    { "en": "Apa Itu Modulasi Denyut Posisi?", "id": "PPM (Pulse-Position Modulation)." },
    { "en": "Di Mana PWM (Pulse-Width Modulation) Umum Digunakan?", "id": "Kontrol Motor Dan Catu Daya." },
    { "en": "Apa Keuntungan PPM (Pulse-Position Modulation)?", "id": "Efisiensi Daya Tinggi." },
    { "en": "Apa Itu Modulasi Kode Denyut?", "id": "PCM (Pulse-Code Modulation)." },
    { "en": "Apa Representasi Digital Sinyal Analog?", "id": "PCM (Pulse-Code Modulation)." },
    { "en": "Modulasi Mana Yang Paling Dasar Untuk Audio Digital?", "id": "PCM (Pulse-Code Modulation)." },
    { "en": "Apa Itu Modulasi Delta?", "id": "Mengirimkan Perbedaan Antar Sampel (1-bit)." },
    { "en": "Apa Itu DPCM (Differential Pulse-Code Modulation)?", "id": "PCM Yang Mengkodekan Selisih Nilai." },
    { "en": "Apa Itu ADPCM (Adaptive Differential Pulse-Code Modulation)?", "id": "DPCM Dengan Langkah Kuantisasi Adaptif." },
    { "en": "Apa Itu Sinyal Jam (Clock Signal)?", "id": "Sinyal Sinkronisasi Untuk Sirkuit Digital." },
    { "en": "Apa Itu Tepi Naik (Rising Edge)?", "id": "Transisi Sinyal Dari Rendah Ke Tinggi." },
    { "en": "Apa Itu Tepi Turun (Falling Edge)?", "id": "Transisi Sinyal Dari Tinggi Ke Rendah." },
    { "en": "Apa Itu Jitter?", "id": "Variasi Waktu Pada Tepi Sinyal." },
    { "en": "Apa Akibat Jitter Pada Modulasi?", "id": "Meningkatkan Bit Error Rate (BER)." },
    { "en": "Apa Itu Sinyal Pembawa Termodulasi?", "id": "Sinyal Passband." },
    { "en": "Apa Modulasi Yang Menghasilkan Sinyal Passband?", "id": "AM (Amplitude Modulation), FM (Frequency Modulation), PSK (Phase-Shift Keying), Dll." },
    { "en": "Apa Itu Modulasi Baseband?", "id": "Mengirim Sinyal Digital Tanpa Pembawa RF." },
    { "en": "Contoh Modulasi Baseband?", "id": "Kode Garis (Line Coding) Seperti Manchester." },
    { "en": "Apa Itu Filter Kosinus Naik?", "id": "Filter Pembentuk Pulsa (Pulse Shaping)." },
    { "en": "Apa Itu Kriteria Nyquist Untuk ISI (Inter-Symbol Interference)?", "id": "Syarat Untuk Transmisi Bebas ISI." },
    { "en": "Apa Itu Rolloff Factor?", "id": "Parameter Yang Menentukan Kelebihan Bandwidth." },
    { "en": "Apa Itu Modulasi Dengan Efisiensi Daya Baik?", "id": "FSK (Frequency-Shift Keying), GMSK (Gaussian Minimum-Shift Keying)." },
    { "en": "Apa Itu Modulasi Dengan Efisiensi Spektral Baik?", "id": "QAM (Quadrature Amplitude Modulation) Orde Tinggi." },
    { "en": "Mana Yang Lebih Penting, Efisiensi Daya Atau Spektral?", "id": "Tergantung Pada Aplikasi Sistem." },
    { "en": "Aplikasi Yang Butuh Efisiensi Daya?", "id": "Komunikasi Satelit Dan Perangkat Baterai." },
    { "en": "Aplikasi Yang Butuh Efisiensi Spektral?", "id": "Jaringan Seluler Dan Wi-Fi." },
    { "en": "Apa Itu Kode Hamming?", "id": "Jenis Kode Koreksi Error Sederhana." },
    { "en": "Apa Itu Kode Reed-Solomon?", "id": "Kode Koreksi Error Untuk Burst Error." },
    { "en": "Apa Itu Pengkodean Blok?", "id": "Mengkodekan Blok Bit Data Tetap." },
    { "en": "Apa Itu Pengkodean Konvolusional?", "id": "Mengkodekan Aliran Bit Secara Berkelanjutan." },
    { "en": "Apa Itu Panjang Kendala (Constraint Length)?", "id": "Parameter Dalam Kode Konvolusional." },
    { "en": "Apa Itu Dekoder Hard-Decision?", "id": "Dekoder Yang Bekerja Dengan Bit 0/1." },
    { "en": "Apa Itu Dekoder Soft-Decision?", "id": "Dekoder Menggunakan Informasi Probabilitas." },
    { "en": "Dekoder Mana Yang Lebih Baik?", "id": "Dekoder Soft-Decision." },
    { "en": "Apa Itu Gain Pengkodean (Coding Gain)?", "id": "Peningkatan Kinerja SNR Akibat Pengkodean." },
    { "en": "Apa Itu Laju Kode (Code Rate)?", "id": "Rasio Bit Informasi Per Bit Total." },
    { "en": "Apa Itu Puncturing Dalam Pengkodean?", "id": "Menghapus Beberapa Bit Paritas." },
    { "en": "Apa Itu Modulasi Coded?", "id": "Menggabungkan Modulasi Dengan Pengkodean." },
    { "en": "Apa Itu Bit Paritas?", "id": "Bit Tambahan Untuk Deteksi Error." },
    { "en": "Apa Itu CRC (Cyclic Redundancy Check)?", "id": "Kode Untuk Deteksi Error." },
    { "en": "Apakah CRC (Cyclic Redundancy Check) Bisa Memperbaiki Error?", "id": "Tidak, Hanya Mendeteksi Error." },
    { "en": "Apa Itu ARQ (Automatic Repeat Request)?", "id": "Meminta Transmisi Ulang Jika Ada Error." },
    { "en": "Apa Itu FEC (Forward Error Correction)?", "id": "Memperbaiki Error Di Penerima." },
    { "en": "Apa Itu HARQ (Hybrid Automatic Repeat Request)?", "id": "Gabungan ARQ Dan FEC." },
    { "en": "Apa Itu FDD (Frequency Division Duplex)?", "id": "Uplink Dan Downlink Frekuensi Berbeda." },
    { "en": "Apa Itu TDD (Time Division Duplex)?", "id": "Uplink Dan Downlink Waktu Berbeda." },
    { "en": "Apa Itu Modulasi Vektor?", "id": "Representasi Sinyal Sebagai Vektor I/Q." },
    { "en": "Apa Itu Kesalahan Vektor Magnitudo?", "id": "EVM (Error Vector Magnitude)." },
    { "en": "Apa Yang Diukur EVM (Error Vector Magnitude)?", "id": "Kualitas Akurasi Sinyal Modulasi." },
    { "en": "Apa Penyebab EVM (Error Vector Magnitude) Yang Buruk?", "id": "Derau, Distorsi, Ketidakseimbangan I/Q." },
    { "en": "Apa Itu Predistorsi Digital?", "id": "DPD (Digital Pre-Distortion)." },
    { "en": "Apa Fungsi DPD (Digital Pre-Distortion)?", "id": "Mengkompensasi Non-Linearitas Penguat Daya." },
    { "en": "Apa Itu Crest Factor Reduction?", "id": "CFR (Crest Factor Reduction)." },
    { "en": "Apa Fungsi CFR (Crest Factor Reduction)?", "id": "Mengurangi PAPR (Peak-to-Average Power Ratio) Sinyal OFDM." },
    { "en": "Apa Itu Filter Interpolasi?", "id": "Filter Untuk Meningkatkan Laju Sampel." },
    { "en": "Apa Itu Filter Desimasi?", "id": "Filter Untuk Menurunkan Laju Sampel." },
    { "en": "Apa Itu Sistem Multi-laju (Multirate System)?", "id": "Sistem Dengan Laju Sampel Berbeda." },
    { "en": "Apa Itu Bank Filter (Filter Bank)?", "id": "Memisahkan Sinyal Menjadi Sub-band." },
    { "en": "Apa Itu Transformasi Wavelet?", "id": "Analisis Sinyal Menggunakan Fungsi Wavelet." },
    { "en": "Apa Itu Spektrum Tersebar (Spread Spectrum)?", "id": "Menyebarkan Energi Sinyal Ke Bandwidth Luas." },
    { "en": "Apa Itu DSSS (Direct-Sequence Spread Spectrum)?", "id": "Menyebarkan Sinyal Dengan Kode Chip." },
    { "en": "Apa Itu FHSS (Frequency-Hopping Spread Spectrum)?", "id": "Mengubah Frekuensi Transmisi Secara Cepat." },
    { "en": "Apa Itu Penguatan Proses (Processing Gain)?", "id": "Peningkatan SNR Akibat Spread Spectrum." },
    { "en": "Apa Itu Akuisisi Kode?", "id": "Proses Sinkronisasi Kode Di Penerima." },
    { "en": "Apa Itu Pelacakan Kode (Code Tracking)?", "id": "Menjaga Sinkronisasi Kode." },
    { "en": "Apa Itu Sinyal Pilot?", "id": "Sinyal Referensi Untuk Sinkronisasi/Estimasi." },
    { "en": "Apa Itu Sinkronisasi Frame?", "id": "Menemukan Awal Dari Sebuah Frame Data." },
    { "en": "Apa Itu Sinkronisasi Simbol?", "id": "Menemukan Waktu Optimal Untuk Sampling." },
    { "en": "Apa Itu Detektor Korelasi?", "id": "Penerima Optimal Untuk Sinyal DSSS." },
    { "en": "Apa Itu Urutan-M (M-Sequence)?", "id": "Jenis Kode Pseudo-acak Populer." },
    { "en": "Apa Itu Kode Gold?", "id": "Kode Dengan Sifat Korelasi Silang Baik." },
    { "en": "Apa Itu Kode Walsh-Hadamard?", "id": "Kode Ortogonal Yang Digunakan Di CDMA." },
    { "en": "Apa Itu Interferensi Multi-pengguna?", "id": "Gangguan Antar Pengguna Dalam Sistem CDMA." },
    { "en": "Bagaimana Sifat Ortogonal Mengurangi Interferensi?", "id": "Sinyal Pengguna Lain Terlihat Seperti Derau." },
    { "en": "Apa Itu Modulasi Tunda Diferensial?", "id": "DQPSK (Differential Quadrature Phase-Shift Keying)." },
    { "en": "Apa Itu OQPSK (Offset Quadrature Phase-Shift Keying)?", "id": "QPSK Dengan Pergeseran Waktu Sinyal Q." },
    { "en": "Apa Keuntungan OQPSK (Offset Quadrature Phase-Shift Keying)?", "id": "Mengurangi Perubahan Fasa Mendadak." },
    { "en": "Apa Itu π/4-QPSK?", "id": "Varian QPSK Dengan Pergeseran Fasa Tambahan." },
    { "en": "Apa Itu CPM (Continuous-Phase Modulation)?", "id": "Modulasi Dengan Fasa Yang Selalu Kontinu." },
    { "en": "Apa Itu Modulasi Amplitudo Sisa?", "id": "AM (Amplitude Modulation) Dengan Sedikit Pembawa." },
    { "en": "Apa Itu SSB-FC (Single Sideband Full Carrier)?", "id": "SSB Dengan Pembawa Penuh." },
    { "en": "Apa Itu ISB (Independent Sideband)?", "id": "Mengirim Informasi Berbeda Di USB/LSB." },
    { "en": "Apa Itu Modulasi Stereo FM?", "id": "Mengirim Sinyal L+R Dan L-R." },
    { "en": "Apa Itu Sinyal Pilot Stereo?", "id": "Sinyal 19 kHz Untuk Demodulasi Stereo." },
    { "en": "Apa Itu SCA (Subsidiary Communications Authority)?", "id": "Sub-pembawa Tambahan Pada Sinyal FM." },
    { "en": "Apa Itu Modulasi Digital Pada Pembawa Analog?", "id": "Digunakan Pada Modem Telepon." },
    { "en": "Apa Itu Sistem Radio Trunking?", "id": "Berbagi Sekelompok Kanal Frekuensi." },
    { "en": "Apa Itu Simulcast?", "id": "Menyiarkan Sinyal Sama Dari Banyak Pemancar." },
    { "en": "Apa Itu Modulasi Cerdas (Smart Modulation)?", "id": "Nama Lain Untuk Modulasi Adaptif." },
    { "en": "Apa Itu Modulasi Hierarkis?", "id": "Menanamkan Aliran Data Prioritas Berbeda." },
    { "en": "Apa Itu Watermarking Digital?", "id": "Menyematkan Informasi Tersembunyi Dalam Sinyal." },
    { "en": "Apa Itu Steganografi?", "id": "Seni Menyembunyikan Pesan." },
    { "en": "Apa Itu Modulasi Optik?", "id": "Memodulasi Sinar Cahaya." },
    { "en": "Bagaimana Cara Memodulasi Cahaya?", "id": "Mengubah Intensitas, Frekuensi, Atau Fasa." },
    { "en": "Apa Itu Modulator Eksternal?", "id": "Perangkat Yang Memodulasi Cahaya Di Luar Laser." },
    { "en": "Apa Itu Modulator Langsung (Direct Modulation)?", "id": "Memodulasi Laser Dengan Mengubah Arus." },
    { "en": "Apa Itu Chirp Frekuensi?", "id": "Perubahan Frekuensi Yang Tidak Diinginkan." },
    { "en": "Apa Penyebab Chirp Frekuensi?", "id": "Modulasi Langsung Pada Laser Semikonduktor." },
    { "en": "Apa Itu Modulator Mach-Zehnder?", "id": "Modulator Optik Berbasis Interferensi." },
    { "en": "Apa Itu Modulator Elektro-Optik?", "id": "Mengubah Sifat Optik Dengan Medan Listrik." },
    { "en": "Apa Itu Modulator Akusto-Optik?", "id": "Mengubah Sifat Optik Dengan Gelombang Suara." },
    { "en": "Apa Itu Modulasi Spasial Cahaya?", "id": "SLM (Spatial Light Modulator)." },
    { "en": "Di Mana SLM (Spatial Light Modulator) Digunakan?", "id": "Proyektor Digital Dan Holografi." },
    { "en": "Apa Itu Deteksi Langsung (Direct Detection)?", "id": "Penerima Optik Yang Merespon Intensitas." },
    { "en": "Apa Itu Deteksi Koheren Optik?", "id": "Mendeteksi Amplitudo Dan Fasa Cahaya." },
    { "en": "Apa Itu Heterodyne Optik?", "id": "Pencampuran Sinyal Optik Dengan Laser Lokal." },
    { "en": "Apa Itu Homodyne Optik?", "id": "Heterodyne Dengan Frekuensi Sama Persis." },
    { "en": "Apa Itu Modulasi Polaritas Kembali-ke-Nol?", "id": "RZ (Return-to-Zero) Signaling." },
    { "en": "Apa Itu Modulasi Polaritas Tidak-Kembali-ke-Nol?", "id": "NRZ (Non-Return-to-Zero) Signaling." },
    { "en": "Sinyal Mana Yang Selalu Kembali Ke Nol Antar Bit?", "id": "RZ (Return-to-Zero)." },
    { "en": "Apa Itu Modulasi Kunci Geser Fasa Kuadratur?", "id": "QPSK (Quadrature Phase-Shift Keying)." },
    { "en": "Apa Itu Modulasi Kunci Geser Amplitudo Kuadratur?", "id":'QAM (Quadrature Amplitude Modulation).'},
    { "en": "Apa Itu Modulasi Kunci Geser Frekuensi Minimum?", "id": "MSK (Minimum-Shift Keying)." },
    { "en": "Apa Itu Modulasi Kode Denyut Diferensial?", "id": "DPCM (Differential Pulse-Code Modulation)." },
    { "en": "Apa Itu Modulasi Dengan Kode Trellis?", "id": "TCM (Trellis-Coded Modulation)." },
    { "en": "Apa Itu Modulasi Multi-pembawa Kode-Divisi?", "id": "MC-CDMA (Multi-Carrier Code-Division Multiple Access)." },
    { "en": "Apa Itu Modulasi Single-Carrier FDMA?", "id": "SC-FDMA (Single-Carrier Frequency-Division Multiple Access)." },
    { "en": "Di Mana SC-FDMA Digunakan?", "id": "Uplink Pada Jaringan 4G LTE." },
    { "en": "Kenapa SC-FDMA Digunakan Di Uplink?", "id": "Memiliki PAPR (Peak-to-Average Power Ratio) Lebih Rendah." },
    { "en": "Apa Itu Modulasi Filter Bank Multi-Carrier?", "id": "FBMC (Filter Bank Multi-Carrier)." },
    { "en": "Apa Keuntungan FBMC (Filter Bank Multi-Carrier)?", "id": "Penekanan Side Lobe Sangat Baik." },
    { "en": "Apa Itu Modulasi Universal Filtered Multi-Carrier?", "id": "UFMC (Universal Filtered Multi-Carrier)." },
    { "en": "Apa Itu Generalized Frequency Division Multiplexing?", "id": "GFDM (Generalized Frequency Division Multiplexing)." },
    { "en": "Apa Itu Modulasi Spasial (Spatial Modulation)?", "id": "Informasi Dikodekan Dalam Indeks Antena." },
    { "en": "Apa Itu Sistem MIMO (Multiple-Input Multiple-Output)?", "id": "Menggunakan Banyak Antena Di Pemancar/Penerima." },
    { "en": "Apa Itu Multiplexing Spasial?", "id": "Mengirim Aliran Data Berbeda Lewat Antena." },
    { "en": "Apa Itu Pengkodean Ruang-Waktu?", "id": "STC (Space-Time Coding)." },
    { "en": "Apa Itu Pengkodean Blok Ruang-Waktu?", "id": "STBC (Space-Time Block Coding)." },
    { "en": "Contoh STBC (Space-Time Block Coding)?", "id": "Kode Alamouti." },
    { "en": "Apa Itu Beamforming?", "id": "Mengarahkan Energi Sinyal Ke Arah Tertentu." },
    { "en": "Apa Itu Sistem Massive MIMO?", "id": "Sistem MIMO Dengan Jumlah Antena Sangat Banyak." },
    { "en": "Apa Itu Modulasi Index?", "id": "Ukuran Seberapa Banyak Pembawa Dimodulasi." },
    { "en": "Apa Itu Modulasi Dalam (Deep Modulation)?", "id": "Modulasi Dengan Indeks Yang Tinggi." },
    { "en": "Apa Itu Deviasi Fasa Puncak?", "id": "Pergeseran Fasa Maksimum Dalam PM." },
    { "en": "Apa Itu Pita Sisi Ganda (Double Sideband)?", "id": "Mengandung Pita Sisi Atas Dan Bawah." },
    { "en": "Apa Itu Demodulasi Produk?", "id": "Nama Lain Untuk Deteksi Sinkron." },
    { "en": "Apa Itu Costas Loop?", "id": "Sirkuit PLL Untuk Pemulihan Pembawa DSB-SC." },
    { "en": "Apa Itu Squaring Loop?", "id": "Sirkuit Lain Untuk Pemulihan Pembawa." },
    { "en": "Apa Itu Foster-Seeley Discriminator?", "id": "Jenis Diskriminator Frekuensi Untuk FM." },
    { "en": "Apa Itu Ratio Detector?", "id": "Demodulator FM Yang Tahan Derau Amplitudo." },
    { "en": "Apa Itu Quadrature Detector?", "id": "Demodulator FM Berbasis Pergeseran Fasa." },
    { "en": "Apa Itu Zero-Crossing Detector?", "id": "Menghitung Laju Lintasan Nol Sinyal FM." },
    { "en": "Apa Itu Modulasi Armstrong?", "id": "Metode Tidak Langsung Menghasilkan Sinyal FM." },
    { "en": "Apa Itu Modulasi Langsung (Direct Modulation)?", "id": "Langsung Mengubah Frekuensi Osilator." },
    { "en": "Apa Itu Slope Detector?", "id": "Demodulator AM Sederhana Menggunakan Filter." },
    { "en": "Apa Itu Modulasi Digital Koheren?", "id": "PSK (Phase-Shift Keying), QAM (Quadrature Amplitude Modulation)." },
    { "en": "Apa Itu Modulasi Digital Non-Koheren?", "id": "ASK (Amplitude-Shift Keying), FSK (Frequency-Shift Keying), DPSK (Differential Phase-Shift Keying)." },
    { "en": "Apa Itu FSK (Frequency-Shift Keying) Fase Kontinu?", "id": "CP-FSK (Continuous-Phase Frequency-Shift Keying)." },
    { "en": "Apa Itu Sunde's FSK?", "id": "FSK Dengan Indeks Modulasi 0.5." },
    { "en": "Apa Itu Modulasi Partial Response?", "id": "Memperkenalkan ISI Terkontrol Untuk Efisiensi." },
    { "en": "Contoh Modulasi Partial Response?", "id": "Modulasi Duo-biner." },
    { "en": "Apa Itu Koding Diferensial?", "id": "Mengatasi Ambiguitas Fasa 180 Derajat." },
    { "en": "Apa Itu Modulasi Pita Suara (Voiceband Modulation)?", "id": "Modulasi Untuk Kanal Telepon." },
    { "en": "Apa Itu Standar Modem V.32?", "id": "Menggunakan Modulasi 16-QAM." },
    { "en": "Apa Itu Standar Modem V.90?", "id": "Menggunakan PCM (Pulse-Code Modulation) Di Sisi Jaringan." },
    { "en": "Apa Itu Modulasi Gelombang Mikro?", "id": "Modulasi Pada Frekuensi Gigahertz." },
    { "en": "Apa Itu Modulasi Satelit?", "id": "Modulasi Untuk Komunikasi Via Satelit." },
    { "en": "Apa Itu SCPC (Single Channel Per Carrier)?", "id": "Setiap Kanal Diberi Satu Pembawa." },
    { "en": "Apa Itu MCPC (Multiple Channels Per Carrier)?", "id": "Banyak Kanal Dalam Satu Pembawa." },
    { "en": "Apa Itu Modulasi Visible Light Communication (VLC)?", "id": "Memodulasi Cahaya Tampak (Seperti LED)." },
    { "en": "Apa Itu Komunikasi Akustik Bawah Air?", "id": "Menggunakan Gelombang Suara Di Air." },
    { "en": "Tantangan Modulasi Bawah Air?", "id": "Multipath Parah Dan Efek Doppler." },
    { "en": "Apa Itu Modulasi Power-Line Communication (PLC)?", "id": "Menggunakan Jaringan Listrik Sebagai Media." },
    { "en": "Tantangan Modulasi PLC (Power-Line Communication)?", "id": "Derau Impulsif Dan Atenuasi Tinggi." },
    { "en": "Apa Itu Modulasi Near-Field Communication (NFC)?", "id": "ASK (Amplitude-Shift Keying) Dengan Modifikasi." },
    { "en": "Apa Itu Modulasi Bluetooth?", "id": "GFSK (Gaussian Frequency-Shift Keying) Atau DPSK (Differential Phase-Shift Keying)." },
    { "en": "Apa Itu Modulasi Wi-Fi?", "id": "OFDM (Orthogonal Frequency-Division Multiplexing) Dengan QAM (Quadrature Amplitude Modulation)." },
    { "en": "Apa Itu Modulasi Seluler (4G/5G)?", "id": "OFDMA (Orthogonal Frequency-Division Multiple Access)." },
    { "en": "Apa Perbedaan OFDM Dan OFDMA?", "id": "OFDMA Memberi Sub-pembawa Ke Pengguna Berbeda." },
    { "en": "Apa Itu Modulasi Siaran Radio Digital (DAB)?", "id": "COFDM (Coded Orthogonal Frequency-Division Multiplexing)." },
    { "en": "Apa Itu Modulasi Siaran TV Digital (DVB-T)?", "id": "COFDM (Coded Orthogonal Frequency-Division Multiplexing)." },
    { "en": "Apa Itu Kompresi Data?", "id": "Proses Mengurangi Ukuran Data." },
    { "en": "Apa Itu Kompresi Lossless?", "id": "Data Asli Dapat Dipulihkan Sempurna." },
    { "en": "Apa Itu Kompresi Lossy?", "id": "Beberapa Informasi Dihilangkan Secara Permanen." },
    { "en": "Contoh Kompresi Lossless?", "id": "ZIP, PNG, FLAC." },
    { "en": "Contoh Kompresi Lossy?", "id": "JPEG, MP3, MPEG." },
    { "en": "Apa Itu Kode Huffman?", "id": "Teknik Pengkodean Entropi." },
    { "en": "Apa Itu Enkripsi?", "id": "Mengamankan Data Dari Akses Tidak Sah." },
    { "en": "Apa Itu Kriptografi?", "id": "Ilmu Tentang Komunikasi Aman." },
    { "en": "Apa Itu Kunci Simetris?", "id": "Kunci Sama Untuk Enkripsi Dan Dekripsi." },
    { "en": "Apa Itu Kunci Asimetris (Kunci Publik)?", "id": "Kunci Berbeda Untuk Enkripsi/Dekripsi." },
    { "en": "Apa Itu AES (Advanced Encryption Standard)?", "id": "Standar Enkripsi Simetris." },
    { "en": "Apa Itu RSA (Rivest-Shamir-Adleman)?", "id": "Algoritma Kriptografi Kunci Publik." },
    { "en": "Apa Itu Tanda Tangan Digital?", "id": "Memastikan Keaslian Dan Integritas." },
    { "en": "Apa Itu Fungsi Hash?", "id": "Membuat Sidik Jari Digital Ukuran Tetap." },
    { "en": "Contoh Fungsi Hash?", "id": "SHA-256 (Secure Hash Algorithm 256-bit)." },
    { "en": "Apa Itu Sertifikat Digital?", "id": "Mengikat Kunci Publik Ke Identitas." },
    { "en": "Apa Itu Modulasi Kunci Geser Amplitudo-Fasa?", "id": "APSK (Amplitude Phase-Shift Keying)." },
    { "en": "Apa Keuntungan APSK (Amplitude Phase-Shift Keying)?", "id": "Efisiensi Daya Lebih Baik Dari QAM." },
    { "en": "Di Mana APSK (Amplitude Phase-Shift Keying) Digunakan?", "id": "Penyiaran Satelit (DVB-S2)." },
    { "en": "Apa Itu Probabilitas Kesalahan Bit?", "id": "BER (Bit Error Rate)." },
    { "en": "Apa Itu Probabilitas Kesalahan Simbol?", "id": "SER (Symbol Error Rate)." },
    { "en": "Apa Itu Koding Diferensial Dalam PSK?", "id": "Mengatasi Ambiguitas Fasa Pembawa." },
    { "en": "Apa Itu Kanal Linear?", "id": "Output Adalah Superposisi Respons Input." },
    { "en": "Apa Itu Kanal Non-Linear?", "id": "Output Tidak Proporsional Terhadap Input." },
    { "en": "Apa Itu Modulasi Dengan Memori?", "id": "Simbol Saat Ini Tergantung Simbol Sebelumnya." },
    { "en": "Apa Itu Modulasi Tanpa Memori?", "id": "Setiap Simbol Independen." },
    { "en": "Apa Itu Modulasi Biner?", "id": "Setiap Simbol Mewakili Satu Bit." },
    { "en": "Apa Itu Modulasi Non-Biner (M-ary)?", "id": "Simbol Mewakili Lebih Dari Satu Bit." },
    { "en": "Apa Itu Bandwidth Daya Ekuivalen?", "id": "Ukuran Bandwidth Sinyal Acak." },
    { "en": "Apa Itu Proses Siklostasioner?", "id": "Statistiknya Bervariasi Secara Periodik." },
    { "en": "Apa Itu Filter Akar Kosinus Naik?", "id": "Filter Pembentuk Pulsa Terpisah." },
    { "en": "Di Mana Filter Akar Kosinus Naik Ditempatkan?", "id": "Di Pemancar Dan Penerima." },
    { "en": "Apa Efek Gabungan Dua Filter Akar Kosinus Naik?", "id": "Filter Kosinus Naik (Bebas ISI)." },
    { "en": "Apa Itu Kepadatan Spektral Daya?", "id": "PSD (Power Spectral Density)." },
    { "en": "Apa Fungsi PSD (Power Spectral Density)?", "id": "Mendeskripsikan Distribusi Daya Per Frekuensi." },
    { "en": "Bagaimana Spektrum Sinyal AM (Amplitude Modulation)?", "id": "Pembawa Dengan Dua Pita Sisi." },
    { "en": "Bagaimana Spektrum Sinyal FM (Frequency Modulation)?", "id": "Terdiri Dari Banyak Pita Sisi." },
    { "en": "Apa Itu Fungsi Bessel?", "id": "Menentukan Amplitudo Pita Sisi FM." },
    { "en": "Apa Itu Spektrum Sinyal ASK (Amplitude-Shift Keying)?", "id": "Berbentuk Seperti Fungsi Sinc Kuadrat." },
    { "en": "Apa Itu Spektrum Sinyal PSK (Phase-Shift Keying)?", "id": "Berbentuk Seperti Fungsi Sinc Kuadrat." },
    { "en": "Apa Itu Spektrum Sinyal FSK (Frequency-Shift Keying)?", "id": "Dua Puncak Pada Frekuensi Simbol." },
    { "en": "Apa Itu Bandwidth Null-to-Null?", "id": "Lebar Lobus Utama Spektrum." },
    { "en": "Apa Itu Bandwidth -3dB?", "id": "Lebar Pita Saat Daya Turun Setengah." },
    { "en": "Apa Itu Bandwidth Terokupasi (Occupied Bandwidth)?", "id": "Bandwidth Yang Mengandung 99% Daya." },
    { "en": "Apa Itu Emisi Di Luar Pita (Out-of-Band Emission)?", "id": "Daya Yang Dipancarkan Di Luar Kanal." },
    { "en": "Apa Itu Topeng Spektral (Spectral Mask)?", "id": "Batas Maksimum Emisi Di Luar Pita." },
    { "en": "Apa Itu Modulasi Dengan Amplop Konstan?", "id": "Amplitudo Sinyal Tidak Pernah Berubah." },
    { "en": "Apa Itu Modulasi Dengan Amplop Tidak Konstan?", "id": "Amplitudo Sinyal Bervariasi." },
    { "en": "Apa Itu Jarak Minimum Konstelasi?", "id": "Menentukan Ketahanan Terhadap Derau." },
    { "en": "Apa Itu Batas Union (Union Bound)?", "id": "Aproksimasi Probabilitas Error Simbol." },
    { "en": "Apa Itu Maximum Likelihood (ML) Detection?", "id": "Aturan Keputusan Optimal Di Penerima." },
    { "en": "Apa Itu Modulasi Pita Ultra-Lebar?", "id": "UWB (Ultra-Wideband)." },
    { "en": "Apa Karakteristik UWB (Ultra-Wideband)?", "id": "Menggunakan Pulsa Sangat Pendek." },
    { "en": "Apa Itu Modulasi Posisi Denyut (Pulse Position Modulation)?", "id": "PPM (Pulse-Position Modulation)." },
    { "en": "Apa Itu Modulasi Amplitudo Denyut (Pulse Amplitude Modulation)?", "id": "PAM (Pulse-Amplitude Modulation)." },
    { "en": "Apa Beda PAM (Pulse Amplitude Modulation) Dan ASK (Amplitude-Shift Keying)?", "id": "PAM Adalah Versi Analognya." },
    { "en": "Apa Itu Modulasi Kepadatan Denyut (Pulse Density Modulation)?", "id": "PDM (Pulse-Density Modulation)." },
    { "en": "Di Mana PDM (Pulse-Density Modulation) Digunakan?", "id": "Format Audio Super Audio CD (SACD)." },
    { "en": "Apa Itu Direct Sequence-Code Division Multiple Access?", "id": "DS-CDMA." },
    { "en": "Apa Itu Frequency Hopping-Code Division Multiple Access?", "id": "FH-CDMA." },
    { "en": "Apa Itu Time Hopping-CDMA?", "id": "TH-CDMA." },
    { "en": "Apa Itu Walsh Code?", "id": "Kode Ortogonal Untuk Kanalisasi CDMA." },
    { "en": "Apa Itu Kode Pseudo-Noise (PN Code)?", "id": "Kode Untuk Menyebarkan Sinyal." },
    { "en": "Apa Itu Near-Far Problem?", "id": "Masalah Pengguna Dekat Mengalahkan Jauh." },
    { "en": "Bagaimana Mengatasi Near-Far Problem?", "id": "Menggunakan Kontrol Daya (Power Control)." },
    { "en": "Apa Itu Soft Handoff?", "id": "Terhubung Ke Dua BTS Sekaligus." },
    { "en": "Sistem Mana Yang Menggunakan Soft Handoff?", "id": "CDMA (Code-Division Multiple Access)." },
    { "en": "Apa Itu Hard Handoff?", "id": "Memutus Koneksi Lama Sebelum Baru." },
    { "en": "Apa Itu Modulasi Optik Intensitas?", "id": "IM/DD (Intensity Modulation/Direct Detection)." },
    { "en": "Apa Itu Modulasi Koheren Optik?", "id": "Menggunakan Amplitudo, Fasa, Polarisasi Cahaya." },
    { "en": "Apa Itu Modulasi DP-QPSK?", "id": "Dual-Polarization Quadrature Phase-Shift Keying." },
    { "en": "Di Mana DP-QPSK Digunakan?", "id": "Sistem Serat Optik 100G." },
    { "en": "Apa Itu Modulasi Akustik?", "id": "Memodulasi Gelombang Suara." },
    { "en": "Apa Itu Modem Akustik?", "id": "Mengirim Data Melalui Suara." },
    { "en": "Apa Itu Modulasi Amplitudo Vestigial?", "id": "VSB (Vestigial Sideband Modulation)." },
    { "en": "Apa Itu Batas Gray?", "id": "Probabilitas Error Simbol Untuk Gray Coding." },
    { "en": "Apa Itu Modulasi Simbol Kecepatan Variabel?", "id": "VCM (Variable Coding and Modulation)." },
    { "en": "Apa Itu Modulasi Simbol Adaptif?", "id": "ACM (Adaptive Coding and Modulation)." },
    { "en": "Apa Itu Throughput?", "id": "Laju Transfer Data Efektif Aktual." },
    { "en": "Apa Itu Goodput?", "id": "Throughput Lapisan Aplikasi." },
    { "en": "Apa Itu Latency?", "id": "Waktu Tunda Total." },
    { "en": "Apa Itu Bandwidth Propagasi?", "id": "Ukuran Laju Data Maksimum Kanal." },
    { "en": "Apa Itu Shannon-Hartley Theorem?", "id": "Menghitung Kapasitas Kanal Dengan Derau." },
    { "en": "Apa Itu Derau Termal?", "id": "Derau Akibat Gerakan Acak Elektron." },
    { "en": "Apa Itu Derau Shot?", "id": "Derau Akibat Sifat Diskret Muatan." },
    { "en": "Apa Itu Derau Fasa?", "id": "Fluktuasi Acak Fasa Osilator." },
    { "en": "Apa Itu Derau Amplitudo?", "id": "Fluktuasi Acak Amplitudo Osilator." },
    { "en": "Apa Itu Sinyal Siklostasioner?", "id": "Sinyal Dengan Statistik Periodik." },
    { "en": "Apa Itu Synchronous Data Link Control?", "id": "SDLC (Synchronous Data Link Control)." },
    { "en": "Apa Itu High-Level Data Link Control?", "id": "HDLC (High-Level Data Link Control)." },
    { "en": "Apa Itu Sinyal Analog Komposit?", "id": "Sinyal Terdiri Dari Banyak Frekuensi." },
    { "en": "Apa Itu Harmonik?", "id": "Frekuensi Kelipatan Dari Frekuensi Fundamental." },
    { "en": "Apa Itu Timbre?", "id": "Kualitas Suara Yang Ditentukan Harmonik." },
    { "en": "Apa Itu Modulasi Lebar Celah?", "id": "GWM (Gap-Width Modulation)." },
    { "en": "Apa Itu Telegrafi?", "id": "Komunikasi Teks Jarak Jauh." },
    { "en": "Apa Itu Kode Morse?", "id": "Metode Pengkodean Karakter." },
    { "en": "Apa Itu Teletypewriter?", "id": "TTY (Teletypewriter)." },
    { "en": "Apa Itu Kode Baudot?", "id": "Kode Karakter Lima Bit." },
    { "en": "Apa Itu Kode ASCII (American Standard Code for Information Interchange)?", "id": "Standar Pengkodean Karakter." },
    { "en": "Apa Itu Komunikasi Baseband?", "id": "Transmisi Sinyal Digital Tanpa Modulasi." },
    { "en": "Apa Itu Komunikasi Broadband?", "id": "Transmisi Dengan Modulasi Ke Frekuensi Tinggi." },
    { "en": "Apa Itu Pembawa (Carrier)?", "id": "Gelombang Frekuensi Tinggi Yang Dimodulasi." },
    { "en": "Apa Itu Sampul (Envelope)?", "id": "Variasi Amplitudo Sinyal AM." },
    { "en": "Apa Itu Modulasi Silang?", "id": "Modulasi Satu Pembawa Oleh Sinyal Lain." },
    { "en": "Apa Itu Intermodulasi?", "id": "Pencampuran Sinyal Menghasilkan Frekuensi Baru." },
    { "en": "Apa Itu Modulasi Terdistorsi?", "id": "Sinyal Yang Bentuknya Berubah." },
    { "en": "Apa Itu Penguat Linear?", "id": "Menguatkan Tanpa Mengubah Bentuk Sinyal." },
    { "en": "Apa Itu Titik Intersep Orde Ketiga?", "id": "IP3 (Third-Order Intercept Point)." },
    { "en": "Apa Yang Diukur IP3 (Third-Order Intercept Point)?", "id": "Linearitas Penguat Atau Mixer." },
    { "en": "Apa Itu Titik Kompresi 1-dB?", "id": "P1dB (1-dB Compression Point)." },
    { "en": "Apa Yang Diukur P1dB?", "id": "Batas Daya Output Linear." },
    { "en": "Apa Itu Modulasi Digital Cerdas?", "id": "Mengubah Skema Berdasarkan Kondisi Kanal." },
    { "en": "Apa Itu Radio Perangkat Lunak?", "id": "SDR (Software-Defined Radio)." },
    { "en": "Apa Itu Modulasi Trellis?", "id": "Menggabungkan Pengkodean Dan Modulasi." },
    { "en": "Apa Itu Kode Blok?", "id": "Mengkodekan Blok Data Ukuran Tetap." },
    { "en": "Apa Itu Diagram Trellis?", "id": "Representasi Grafis Kode Konvolusional." },
    { "en": "Apa Itu Algoritma Viterbi?", "id": "Algoritma Dekoding Untuk Kode Konvolusi." },
    { "en": "Apa Itu Kanal Simetris Biner?", "id": "BSC (Binary Symmetric Channel)." },
    { "en": "Apa Itu Kanal Penghapusan Biner?", "id": "BEC (Binary Erasure Channel)." },
    { "en": "Apa Itu Pengkodean Entropi?", "id": "Kompresi Data Lossless." },
    { "en": "Apa Itu Panjang Kode Rata-rata?", "id": "Ukuran Efisiensi Kode Sumber." },
    { "en": "Apa Itu Ketidaksamaan Kraft?", "id": "Syarat Untuk Kode Prefiks." },
    { "en": "Apa Itu Kode Prefiks?", "id": "Tidak Ada Kode Yang Merupakan Awalan Kode Lain." },
    { "en": "Apa Itu Pengkodean Aritmetik?", "id": "Metode Kompresi Data." },
    { "en": "Apa Itu Lempel-Ziv-Welch?", "id": "LZW (Lempel-Ziv-Welch) Compression." },
    { "en": "Apa Itu Multipleksing Statistik?", "id": "Berbagi Kanal Berdasarkan Permintaan." },
    { "en": "Apa Itu Sirkuit Switching?", "id": "Membuat Koneksi Khusus Antar Pengguna." },
    { "en": "Apa Itu Paket Switching?", "id": "Data Dibagi Menjadi Paket-paket." },
    { "en": "Apa Itu Datagram?", "id": "Paket Independen Dalam Jaringan." },
    { "en": "Apa Itu Sirkuit Virtual?", "id": "Jalur Tetap Dalam Jaringan Paket." },
    { "en": "Apa Itu Jaringan Seluler?", "id": "Jaringan Radio Terdiri Dari Sel-sel." },
    { "en": "Apa Itu Stasiun Pangkalan?", "id": "BTS (Base Transceiver Station)." },
    { "en": "Apa Itu Kontroler Stasiun Pangkalan?", "id": "BSC (Base Station Controller)." },
    { "en": "Apa Itu Pusat Switching Seluler?", "id": "MSC (Mobile Switching Center)." },
    { "en": "Apa Itu Handoff (Handover)?", "id": "Proses Berpindah Antar Sel." },
    { "en": "Apa Itu Sel Makro (Macrocell)?", "id": "Sel Besar Untuk Area Luas." },
    { "en": "Apa Itu Sel Mikro (Microcell)?", "id": "Sel Kecil Untuk Area Padat." },
    { "en": "Apa Itu Sel Piko (Picocell)?", "id": "Sel Sangat Kecil (Dalam Ruangan)." },
    { "en": "Apa Itu Sel Femto (Femtocell)?", "id": "Sel Pribadi Untuk Rumah/Kantor." },
    { "en": "Apa Itu Efek Pinggir Sel?", "id": "Kinerja Buruk Di Tepi Sel." },
    { "en": "Apa Itu Pernapasan Sel (Cell Breathing)?", "id": "Ukuran Sel Berubah Berdasarkan Beban." },
    { "en": "Di Sistem Mana Cell Breathing Terjadi?", "id": "CDMA (Code-Division Multiple Access)." },
    { "en": "Apa Itu Sektor Antena?", "id": "Membagi Sel Menjadi Beberapa Sektor." },
    { "en": "Apa Itu Kemiringan Antena (Antenna Tilt)?", "id": "Mengatur Sudut Vertikal Antena." },
    { "en": "Apa Itu Jaringan Ad Hoc?", "id": "Jaringan Nirkabel Tanpa Infrastruktur." },
    { "en": "Apa Itu Jaringan Sensor Nirkabel?", "id": "WSN (Wireless Sensor Network)." },
    { "en": "Apa Itu Protokol Routing?", "id": "Aturan Untuk Menemukan Jalur Jaringan." },
    { "en": "Apa Itu Topologi Jaringan?", "id": "Tata Letak Fisik Atau Logis Jaringan." },
    { "en": "Apa Itu Jaringan Mesh?", "id": "Setiap Node Terhubung Ke Banyak Node." },
    { "en": "Apa Itu Modulasi Analog Kuantum?", "id": "QAM (Quantum Amplitude Modulation)." },
    { "en": "Apa Itu Modulasi Untuk Komunikasi Optik Koheren?", "id": "DP-QPSK (Dual-Polarization Quadrature Phase-Shift Keying)." },
    { "en": "Apa Itu Modulasi Untuk Blu-ray?", "id": "17PP (Parity-Preserving Punctured) Code." },
    { "en": "Apa Itu Jaringan Akses Radio?", "id": "RAN (Radio Access Network)." },
    { "en": "Apa Itu Jaringan Inti?", "id": "Core Network." },
    { "en": "Apa Itu Backhaul?", "id": "Koneksi Dari RAN Ke Core Network." },
    { "en": "Apa Itu Fronthaul?", "id": "Koneksi Dalam Arsitektur C-RAN." },
    { "en": "Apa Itu C-RAN (Cloud-Radio Access Network)?", "id": "RAN Dengan Pemrosesan Terpusat." },
    { "en": "Apa Itu Small Cell?", "id": "Istilah Umum Untuk Piko/Femtocell." },
    { "en": "Apa Itu Carrier Aggregation?", "id": "Menggabungkan Beberapa Pembawa Frekuensi." },
    { "en": "Apa Tujuan Carrier Aggregation?", "id": "Meningkatkan Laju Data Puncak." },
    { "en": "Apa Itu Lisensi Spektrum?", "id": "Hak Eksklusif Menggunakan Frekuensi." },
    { "en": "Apa Itu Spektrum Tanpa Lisensi?", "id": "Frekuensi Yang Dapat Digunakan Bersama." },
    { "en": "Contoh Spektrum Tanpa Lisensi?", "id": "ISM (Industrial, Scientific, and Medical) Band." },
    { "en": "Apa Itu Analisis Tautan (Link Budget)?", "id": "Perhitungan Daya Dalam Link Komunikasi." },
    { "en": "Apa Itu EIRP (Effective Isotropic Radiated Power)?", "id": "Daya Pancar Efektif Dari Antena." },
    { "en": "Apa Itu G/T?", "id": "Angka Kebaikan (Figure of Merit) Sistem Penerima." },
    { "en": "Apa Itu Kehilangan Ruang Bebas (Free Space Path Loss)?", "id": "Pelemahan Sinyal Di Ruang Hampa." },
    { "en": "Apa Itu Margin Fading?", "id": "Daya Cadangan Untuk Mengatasi Fading." },
    { "en": "Apa Itu Interferensi Kanal Sebelah?", "id": "ACI (Adjacent-Channel Interference)." },
    { "en": "Apa Itu Interferensi Co-Kanal?", "id": "CCI (Co-Channel Interference)." },
    { "en": "Apa Itu Modulasi Gaussian Frequency Shift Keying?", "id": "GFSK (Gaussian Frequency-Shift Keying)." },
    { "en": "Di Mana GFSK Digunakan?", "id": "Bluetooth." },
    { "en": "Apa Itu Teknik Akses Ganda Pembagian Ortogonal?", "id": "OFDMA (Orthogonal Frequency Division Multiple Access)." },
    { "en": "Apa Itu Unit Sumber Daya (Resource Unit)?", "id": "Unit Alokasi Dalam OFDMA." },
    { "en": "Apa Itu Modulasi Tampilan Kristal Cair?", "id": "Mengubah Polarisasi Cahaya." },
    { "en": "Apa Itu Sinyal Kendali Jarak Jauh?", "id": "Modulasi Kode Pulsa Inframerah." },
    { "en": "Apa Itu Modulasi Dalam Serat Optik?", "id": "Modulasi Intensitas Cahaya." },
    { "en": "Apa Itu Chirping Dalam Laser?", "id": "Perubahan Frekuensi Saat Modulasi." },
    { "en": "Apa Itu Self-Phase Modulation?", "id": "SPM (Self-Phase Modulation)." },
    { "en": "Apa Itu Cross-Phase Modulation?", "id": "XPM (Cross-Phase Modulation)." },
    { "en": "Apa Itu Four-Wave Mixing?", "id": "FWM (Four-Wave Mixing)." },
    { "en": "Apa Itu Modulasi Duobiner?", "id": "Jenis Modulasi Respon Parsial." },
    { "en": "Apa Itu Modulasi Kode Lapis (Layered Coding Modulation)?", "id": "LCM (Layered Coding Modulation)." },
    { "en": "Apa Itu Modulasi Buta (Blind Modulation)?", "id": "Identifikasi Modulasi Tanpa Informasi Awal." },
    { "en": "Apa Itu Klasifikasi Modulasi Otomatis?", "id": "AMC (Automatic Modulation Classification)." },
    { "en": "Apa Itu Demodulasi Berbasis Model?", "id": "Menggunakan Model Kanal Untuk Demodulasi." },
    { "en": "Apa Itu Synchronous AM?", "id": "Demodulasi AM Menggunakan Pembawa Lokal." },
    { "en": "Apa Itu Homodyne Receiver?", "id": "Penerima Dengan IF (Intermediate Frequency) Nol." },
    { "en": "Apa Kelemahan Penerima Homodyne?", "id": "Masalah DC Offset Dan Kebocoran LO." },
    { "en": "Apa Itu Heterodyne Receiver?", "id": "Penerima Dengan IF (Intermediate Frequency) Non-Nol." },
    { "en": "Apa Itu Arsitektur Weaver?", "id": "Modulator SSB (Single Sideband) Berbasis Kuadratur." },
    { "en": "Apa Itu Modulasi Serrodyne?", "id": "Modulasi Fasa Menggunakan Gelombang Gigi Gergaji." },
    { "en": "Apa Itu Modulasi Optik Keseimbangan?", "id": "Menggunakan Dua Modulator Mach-Zehnder." },
    { "en": "Apa Itu Modulasi Sigma-Delta?", "id": "Teknik Oversampling Untuk ADC/DAC." },
    { "en": "Apa Itu Noise Shaping?", "id": "Membentuk Spektrum Derau Kuantisasi." },
    { "en": "Apa Itu Dither?", "id": "Menambahkan Derau Untuk Mengurangi Distorsi." },
    { "en": "Apa Itu Modulasi Digital Video Broadcasting?", "id": "DVB (Digital Video Broadcasting)." },
    { "en": "Apa Itu Advanced Television Systems Committee?", "id": "ATSC (Advanced Television Systems Committee)." },
    { "en": "Apa Itu Integrated Services Digital Broadcasting?", "id": "ISDB (Integrated Services Digital Broadcasting)." },
    { "en": "Apa Itu Digital Multimedia Broadcasting?", "id": "DMB (Digital Multimedia Broadcasting)." },
    { "en": "Apa Itu CDMA2000?", "id": "Standar Seluler 3G." },
    { "en": "Apa Itu WCDMA (Wideband Code Division Multiple Access)?", "id": "Teknologi Akses Radio 3G UMTS." },
    { "en": "Apa Itu HSPA (High-Speed Packet Access)?", "id": "Peningkatan Dari Jaringan 3G." },
    { "en": "Apa Itu LTE (Long-Term Evolution)?", "id": "Standar Seluler Generasi Keempat (4G)." },
    { "en": "Apa Itu NR (New Radio)?", "id": "Standar Seluler Generasi Kelima (5G)." },
    { "en": "Apa Itu WiMAX (Worldwide Interoperability for Microwave Access)?", "id": "Standar Komunikasi Nirkabel Broadband." },
    { "en": "Apa Itu LoRa (Long Range)?", "id": "Teknik Modulasi Spread Spectrum." },
    { "en": "Apa Itu Sigfox?", "id": "Teknologi Jaringan LPWAN Lainnya." },
    { "en": "Apa Itu NB-IoT (Narrowband-Internet of Things)?", "id": "Standar LPWAN (Low-Power Wide-Area Network) Berbasis Seluler." },
    { "en": "Apa Itu LTE-M (Long-Term Evolution for Machines)?", "id": "Standar LPWAN Lainnya Untuk Perangkat IoT." },
    { "en": "Apa Itu Modulasi Chirp Spread Spectrum?", "id": "CSS (Chirp Spread Spectrum)." },
    { "en": "Teknologi Mana Yang Menggunakan CSS (Chirp Spread Spectrum)?", "id": "LoRa (Long Range)." },
    { "en": "Apa Itu Faktor Penyebaran (Spreading Factor)?", "id": "Parameter Yang Mengontrol Laju Data/Jangkauan." },
    { "en": "Apa Itu Modulasi Adaptif Dalam Wi-Fi?", "id": "Secara Otomatis Memilih Laju Data Terbaik." },
    { "en": "Apa Itu Skema MCS (Modulation and Coding Scheme)?", "id": "Kombinasi Modulasi Dan Laju Kode." },
    { "en": "Apa Itu Modulasi Untuk Komunikasi Kabel Listrik?", "id": "BPSK (Binary Phase-Shift Keying) Dengan OFDM (Orthogonal Frequency-Division Multiplexing)." },
    { "en": "Apa Itu Modulasi Untuk Kode QR (Quick Response)?", "id": "Modulasi Spasial (Pola Hitam Putih)." },
    { "en": "Apa Itu Modulasi Untuk Pita Magnetik?", "id": "MFM (Modified Frequency Modulation)." },
    { "en": "Apa Itu Group Coded Recording?", "id": "GCR (Group Coded Recording)." },
    { "en": "Apa Itu Modulasi Untuk Hard Disk Drive?", "id": "PRML (Partial Response Maximum Likelihood)." },
    { "en": "Apa Itu Modulasi Untuk CD (Compact Disc)?", "id": "EFM (Eight-to-Fourteen Modulation)." },
    { "en": "Apa Itu Modulasi Untuk DVD (Digital Versatile Disc)?", "id": "EFMPlus." },
    { "en": "Apa Itu Modulasi Untuk Blu-ray Disc?", "id": "17PP (Parity-Preserving Punctured) Code." },
    { "en": "Apa Itu Modulasi Dalam Memori Flash?", "id": "Penyimpanan Level Tegangan (Multi-Level Cell)." },
    { "en": "Apa Itu MLC (Multi-Level Cell)?", "id": "Menyimpan Lebih Dari Satu Bit Per Sel." },
    { "en": "Apa Itu TLC (Triple-Level Cell)?", "id": "Menyimpan Tiga Bit Per Sel Memori." },
    { "en": "Apa Itu QLC (Quad-Level Cell)?", "id": "Menyimpan Empat Bit Per Sel Memori." },
    { "en": "Apa Itu Modulasi Untuk Remote Control TV?", "id": "Modulasi Pulsa Inframerah." },
    { "en": "Apa Itu Kode Manchester Diferensial?", "id": "Transisi Di Tengah Selalu Ada." },
    { "en": "Di Mana Kode Manchester Digunakan?", "id": "Jaringan Ethernet Awal (10BASE-T)." },
    { "en": "Apa Itu Pengkodean 4B/5B?", "id": "Mengkodekan 4 Bit Menjadi 5 Bit." },
    { "en": "Apa Tujuan Pengkodean 4B/5B?", "id": "Menjamin Transisi Sinyal Yang Cukup." },
    { "en": "Apa Itu Pengkodean 8B/10B?", "id": "Mengkodekan 8 Bit Menjadi 10 Bit." },
    { "en": "Apa Itu DC Balance?", "id": "Menjaga Rata-rata Level DC Sinyal." },
    { "en": "Apa Itu Modulasi PAM-5 (Pulse-Amplitude Modulation 5-level)?", "id": "Digunakan Dalam Gigabit Ethernet." },
    { "en": "Apa Itu Modulasi PAM-16 (Pulse-Amplitude Modulation 16-level)?", "id": "Digunakan Dalam Ethernet Kecepatan Sangat Tinggi." },
    { "en": "Apa Itu Modulasi Untuk DSL (Digital Subscriber Line)?", "id": "DMT (Discrete Multi-Tone)." },
    { "en": "Apa Itu DMT (Discrete Multi-Tone)?", "id": "Pada Dasarnya Adalah OFDM (Orthogonal Frequency-Division Multiplexing)." },
    { "en": "Apa Itu Bit Loading?", "id": "Mengalokasikan Bit Ke Sub-pembawa Berbeda." },
    { "en": "Apa Itu Modulasi Untuk Komunikasi Satelit Modern?", "id": "APSK (Amplitude Phase-Shift Keying) Orde Tinggi." },
    { "en": "Apa Itu Modulasi Untuk Penyiaran Radio Digital?", "id": "COFDM (Coded Orthogonal Frequency-Division Multiplexing)." },
    { "en": "Apa Beda COFDM Dan OFDM?", "id": "COFDM Secara Spesifik Menyebutkan Pengkodean." },
    { "en": "Apa Itu SFN (Single-Frequency Network)?", "id": "Jaringan Pemancar Dengan Frekuensi Sama." },
    { "en": "Bagaimana SFN (Single-Frequency Network) Bekerja?", "id": "OFDM Mengubah Interferensi Menjadi Sinyal Positif." },
    { "en": "Apa Itu Transmodulator?", "id": "Mengubah Satu Jenis Modulasi Ke Lainnya." },
    { "en": "Contoh Transmodulator?", "id": "QAM (Quadrature Amplitude Modulation) Ke COFDM (Coded Orthogonal Frequency-Division Multiplexing)." },
    { "en": "Apa Itu Modulasi Untuk Pembaca Kartu Magnetik?", "id": "FSK (Frequency-Shift Keying) (FSK F2F)." },
    { "en": "Apa Itu Modulasi Optik Polarisasi?", "id": "PolSK (Polarization-Shift Keying)." },
    { "en": "Apa Itu Modulasi Mode Orbital Angular Momentum?", "id": "OAM (Orbital Angular Momentum) Modulation." },
    { "en": "Apa Itu Modulasi Gelombang Kontinu?", "id": "CW (Continuous Wave)." },
    { "en": "Apa Informasi Dalam Sinyal CW (Continuous Wave)?", "id": "Ada Atau Tidaknya Sinyal (On-Off)." },
    { "en": "Di Mana CW (Continuous Wave) Digunakan?", "id": "Telegrafi Kode Morse Dan Radar." },
    { "en": "Apa Itu Modulasi Untuk Telepon Analog?", "id": "Modulasi Amplitudo (Sinyal Suara)." },
    { "en": "Apa Itu Sinyal DTMF (Dual-Tone Multi-Frequency)?", "id": "Modulasi Dua Nada Untuk Panggilan." },
    { "en": "Apa Itu Modulasi Untuk Sistem Pager?", "id": "FSK (Frequency-Shift Keying) (POCSAG/FLEX)." },
    { "en": "Apa Itu Modulasi Untuk AIS (Automatic Identification System)?", "id": "GMSK (Gaussian Minimum-Shift Keying)." },
    { "en": "Apa Itu Modulasi Untuk ADS-B (Automatic Dependent Surveillance–Broadcast)?", "id": "PPM (Pulse-Position Modulation)." },
    { "en": "Apa Itu Modulasi Untuk RFID (Radio-Frequency Identification)?", "id": "ASK (Amplitude-Shift Keying) Atau PSK (Phase-Shift Keying)." },
    { "en": "Apa Itu Backscatter Modulation?", "id": "Tag Pasif Memantulkan Dan Memodulasi Sinyal." },
    { "en": "Apa Itu Modulasi Untuk Komunikasi Cahaya Tampak?", "id": "VLC (Visible Light Communication)." },
    { "en": "Contoh Modulasi VLC (Visible Light Communication)?", "id": "VPPM (Variable Pulse Position Modulation)." },
    { "en": "Apa Itu Sistem Seluler Awal (1G)?", "id": "Menggunakan Modulasi FM (Frequency Modulation) Analog." },
    { "en": "Apa Itu Sistem Seluler 2G?", "id": "Menggunakan Modulasi Digital (GMSK/QPSK)." },
    { "en": "Apa Itu Sistem Seluler 3G?", "id": "Menggunakan CDMA (Code-Division Multiple Access) Dan QPSK (Quadrature Phase-Shift Keying)." },
    { "en": "Apa Itu Sistem Seluler 4G?", "id": "Menggunakan OFDMA (Orthogonal Frequency Division Multiple Access) Dan QAM (Quadrature Amplitude Modulation)." },
    { "en": "Apa Itu Sistem Seluler 5G?", "id": "OFDM (Orthogonal Frequency-Division Multiplexing) Fleksibel Dan QAM (Quadrature Amplitude Modulation) Tinggi." },
    { "en": "Apa Itu Modulasi Untuk Wi-Fi (802.11a/g/n/ac/ax)?", "id": "OFDM (Orthogonal Frequency-Division Multiplexing) Dengan BPSK/QPSK/QAM." },
    { "en": "Apa Itu Modulasi Untuk Bluetooth Classic?", "id": "GFSK (Gaussian Frequency-Shift Keying)." },
    { "en": "Apa Itu Modulasi Untuk Bluetooth Low Energy (BLE)?", "id": "GFSK (Gaussian Frequency-Shift Keying)." },
    { "en": "Apa Itu Modulasi Untuk Zigbee?", "id": "OQPSK (Offset Quadrature Phase-Shift Keying)." },
    { "en": "Apa Itu Modulasi Untuk Z-Wave?", "id": "FSK (Frequency-Shift Keying) Atau GFSK (Gaussian Frequency-Shift Keying)." },
    { "en": "Apa Itu Modulasi Untuk Kartu Kredit Nirkontak?", "id": "ASK (Amplitude-Shift Keying) 10%." },
    { "en": "Apa Itu Modulasi Untuk Siaran Radio AM?", "id": "AM (Amplitude Modulation)." },
    { "en": "Apa Itu Modulasi Untuk Siaran Radio FM?", "id": "FM (Frequency Modulation) Pita Lebar." },
    { "en": "Apa Itu Modulasi Untuk Sinyal Audio TV Analog?", "id": "FM (Frequency Modulation)." },
    { "en": "Apa Itu Modulasi Untuk Sinyal Video TV Analog?", "id": "VSB (Vestigial Sideband Modulation)." },
    { "en": "Apa Itu Modulasi Untuk Sinyal Warna TV Analog?", "id": "QAM (Quadrature Amplitude Modulation) (NTSC/PAL)." },
    { "en": "Apa Itu Modulasi Untuk Teletext?", "id": "Modulasi Digital Disisipkan Dalam Sinyal TV." },
    { "en": "Apa Itu Modulasi Untuk RDS (Radio Data System)?", "id": "Sub-pembawa PSK (Phase-Shift Keying) Pada Siaran FM." },
    { "en": "Apa Itu Modulasi Untuk Siaran Radio HD?", "id": "COFDM (Coded Orthogonal Frequency-Division Multiplexing)." },
    { "en": "Apa Itu Modulasi Untuk Siaran Radio Satelit?", "id": "CDM (Coded Division Multiplexing) Atau COFDM (Coded Orthogonal Frequency-Division Multiplexing)." },
    { "en": "Apa Itu Modulasi Dalam Sistem Radar?", "id": "Pulsa Termodulasi (Chirp, Kode Barker)." },
    { "en": "Apa Itu Chirp Modulation?", "id": "Frekuensi Sinyal Berubah Secara Linear." },
    { "en": "Apa Itu Kode Barker?", "id": "Kode Biner Dengan Sifat Autokorelasi Baik." },
    { "en": "Apa Itu Modulasi Untuk GPS (Global Positioning System)?", "id": "BPSK (Binary Phase-Shift Keying) Dengan DSSS (Direct-Sequence Spread Spectrum)." },
    { "en": "Apa Itu Kode C/A (Coarse/Acquisition)?", "id": "Kode Penyebar Untuk Sinyal GPS Sipil." },
    { "en": "Apa Itu Kode P(Y) (Precise)?", "id": "Kode Terenkripsi Untuk Sinyal GPS Militer." },
    { "en": "Apa Itu BOC (Binary Offset Carrier) Modulation?", "id": "Modulasi Untuk Sinyal GPS Modern." },
    { "en": "Apa Itu Modulasi Gelombang Gravitasi?", "id": "Perubahan Amplitudo Ruang-Waktu." },
    { "en": "Apa Itu Pemrosesan Sinyal In-phase/Quadrature?", "id": "I/Q Processing." },
    { "en": "Apa Itu Konstelasi Tidak Seragam?", "id": "Jarak Antar Titik Simbol Tidak Sama." },
    { "en": "Apa Itu Pembentukan Probabilistik (Probabilistic Shaping)?", "id": "Menggunakan Titik Konstelasi Lebih Sering." },
    { "en": "Apa Itu Faster-Than-Nyquist (FTN) Signaling?", "id": "Mengirim Simbol Lebih Cepat Dari Batas Nyquist." },
    { "en": "Apa Itu Modulasi Untuk Komunikasi Kuantum?", "id": "CV-QKD (Continuous-Variable Quantum Key Distribution)." },
    { "en": "Apa Itu Modulasi Untuk Resonansi Magnetik Nuklir?", "id": "NMR (Nuclear Magnetic Resonance) Spectroscopy." },
    { "en": "Apa Itu Modulasi Spasial?", "id": "Informasi Dalam Posisi Atau Arah." },
    { "en": "Apa Itu Modulasi Dalam Jaringan Saraf?", "id": "Perubahan Laju Penembakan Neuron." },
    { "en": "Apa Itu Modulasi Tunda (Delay Modulation)?", "id": "Informasi Dalam Interval Waktu Antar Pulsa." },
    { "en": "Apa Itu Modulasi Spektral-Fasa?", "id": "SPEED (Spectral Phase-Encoded Energy Dispersal)." },
    { "en": "Apa Itu Modulasi Frekuensi Offset Minimum?", "id": "MSK (Minimum-Shift Keying)." },
    { "en": "Apa Itu Modulasi Biner Fasa Kontinu?", "id": "CPFSK (Continuous-Phase Frequency-Shift Keying)." },
    { "en": "Apa Itu Kunci Geser Fasa Ortogonal?", "id": "OQPSK (Offset Quadrature Phase-Shift Keying)." },
    { "en": "Apa Itu Modulasi Teralis?", "id": "TCM (Trellis Coded Modulation)." },
    { "en": "Apa Itu Modulasi Kode Pulsa Adaptif?", "id": "ADPCM (Adaptive Differential Pulse-Code Modulation)." },
    { "en": "Apa Itu Modulasi Amplitudo Pulsa?", "id": "PAM (Pulse-Amplitude Modulation)." },
    { "en": "Apa Itu Modulasi Lebar Pulsa?", "id": "PWM (Pulse-Width Modulation)." },
    { "en": "Apa Itu Modulasi Posisi Pulsa?", "id": "PPM (Pulse-Position Modulation)." },
    { "en": "Apa Itu Modulasi Kode Pulsa?", "id": "PCM (Pulse-Code Modulation)." },
    { "en": "Apa Itu Modulasi Amplitudo Sisa?", "id": "VSB (Vestigial Sideband Modulation)." },
    { "en": "Apa Itu Modulasi Sisi Tunggal?", "id": "SSB (Single Sideband)." },
    { "en": "Apa Itu Radio Perangkat Lunak?", "id": "SDR (Software-Defined Radio)." },
    { "en": "Apa Itu Rasio Kesalahan Bit?", "id": "BER (Bit Error Rate)." },
    { "en": "Apa Itu Rasio Sinyal-ke-Derau?", "id": "SNR (Signal-to-Noise Ratio)." },
    { "en": "Apa Itu Modulasi Biner?", "id": "Mengirim Satu Bit Per Simbol." },
    { "en": "Apa Itu Modulasi M-ary?", "id": "Mengirim Beberapa Bit Per Simbol." },
    { "en": "Apa Itu Pembentukan Pulsa?", "id": "Membatasi Spektrum Sinyal Baseband." },
    { "en": "Apa Itu Filter Kosinus Terangkat?", "id": "Filter Pembentuk Pulsa Umum." },
    { "en": "Apa Itu Diagram Mata?", "id": "Visualisasi Kualitas Sinyal Digital." },
    { "en": "Apa Itu Jitter Fase?", "id": "Variasi Cepat Fasa Sinyal." },
    { "en": "Apa Itu Osilator Terkendali Tegangan?", "id": "VCO (Voltage-Controlled Oscillator)." },
    { "en": "Apa Itu Loop Terkunci Fasa?", "id": "PLL (Phase-Locked Loop)." },
    { "en": "Apa Itu Detektor Fasa?", "id": "Membandingkan Fasa Dua Sinyal." },
    { "en": "Apa Itu Filter Loop?", "id": "Filter Lolos Bawah Dalam PLL." },
    { "en": "Apa Itu Rentang Kunci (Lock Range)?", "id": "Rentang Frekuensi PLL Dapat Tetap Terkunci." },
    { "en": "Apa Itu Rentang Tangkap (Capture Range)?", "id": "Rentang Frekuensi PLL Dapat Mulai Mengunci." },
    { "en": "Apa Itu Modulasi Sudut?", "id": "Termasuk FM Dan PM." },
    { "en": "Apa Itu Pita Sisi Atas?", "id": "USB (Upper Sideband)." },
    { "en": "Apa Itu Pita Sisi Bawah?", "id": "LSB (Lower Sideband)." },
    { "en": "Apa Itu Modulasi Dengan Pembawa Penuh?", "id": "AM (Amplitude Modulation) Konvensional." },
    { "en": "Apa Itu Modulasi Dengan Pembawa Tertekan?", "id": "DSB-SC (Double Sideband Suppressed Carrier)." },
    { "en": "Apa Itu Detektor Produk?", "id": "Demodulator Untuk Sinyal SSB/DSB-SC." },
    { "en": "Apa Itu Sinyal Kuadratur?", "id": "Dua Sinyal Beda Fasa 90 Derajat." },
    { "en": "Apa Itu Modulasi Frekuensi Sempit?", "id": "NFM (Narrowband FM)." },
    { "en": "Apa Itu Modulasi Frekuensi Lebar?", "id": "WFM (Wideband FM)." },
    { "en": "Apa Itu Penguat Derau Rendah?", "id": "LNA (Low-Noise Amplifier)." },
    { "en": "Apa Itu Penguat Daya?", "id": "PA (Power Amplifier)." },
    { "en": "Apa Itu Modulasi Untuk Siaran Suara Digital?", "id": "DAB (Digital Audio Broadcasting)." },
    { "en": "Apa Itu Modulasi Untuk Siaran Radio Satelit?", "id": "SDARS (Satellite Digital Audio Radio Service)." },
    { "en": "Apa Itu Modulasi Untuk Komunikasi Optik?", "id": "Modulasi Intensitas." },
    { "en": "Apa Itu Kunci Geser On-Off?", "id": "OOK (On-Off Keying)." },
    { "en": "Di Mana OOK (On-Off Keying) Digunakan?", "id": "Komunikasi Serat Optik Dan Inframerah." },
    { "en": "Apa Itu Deteksi Energi?", "id": "Mendeteksi Kehadiran Sinyal Tanpa Fasa." },
    { "en": "Apa Itu Modulasi Dalam Kode Batang?", "id": "Modulasi Lebar Garis." },
    { "en": "Apa Itu Modulasi Dalam Pita Magnetik?", "id": "Saturasi Magnetik." },
    { "en": "Apa Itu Modulasi Untuk Wi-Fi Awal (802.11b)?", "id": "DSSS (Direct-Sequence Spread Spectrum) Dengan CCK." },
    { "en": "Apa Itu Kode Barker?", "id": "Kode Dengan Sifat Autokorelasi Baik." },
    { "en": "Apa Itu Spreading Code?", "id": "Kode Yang Digunakan Untuk Menyebarkan Sinyal." },
    { "en": "Apa Itu Despreading?", "id": "Proses Mengembalikan Sinyal Tersebar." },
    { "en": "Apa Itu Interferensi Jamming?", "id": "Interferensi Sengaja Dari Sumber Lain." },
    { "en": "Apa Itu Anti-Jamming?", "id": "Kemampuan Melawan Interferensi Jamming." },
    { "en": "Teknik Apa Yang Punya Sifat Anti-Jamming?", "id": "Spread Spectrum (DSSS/FHSS)." },
    { "en": "Apa Itu Intersepsi Peluang Rendah?", "id": "LPI (Low Probability of Intercept)." },
    { "en": "Teknik Apa Yang Punya Sifat LPI?", "id": "Spread Spectrum." },
    { "en": "Apa Itu Deteksi Peluang Rendah?", "id": "LPD (Low Probability of Detection)." },
    { "en": "Apa Itu Multipath?", "id": "Sinyal Diterima Melalui Banyak Lintasan." },
    { "en": "Apa Itu Kanal AWGN?", "id": "Kanal Dengan Derau Putih Gaussian Aditif." },
    { "en": "Apa Itu Kanal Rayleigh Fading?", "id": "Kanal Multipath Tanpa Sinyal Langsung." },
    { "en": "Apa Itu Kanal Rician Fading?", "id": "Kanal Multipath Dengan Sinyal Langsung." },
    { "en": "Apa Itu K-Factor?", "id": "Rasio Daya Sinyal Langsung Per Pantulan." },
    { "en": "Apa Itu Equalization?", "id": "Mengkompensasi Distorsi Kanal." },
    { "en": "Apa Itu Equalizer Linear?", "id": "Filter Linear Untuk Memperbaiki Sinyal." },
    { "en": "Apa Itu Equalizer Umpan Balik Keputusan?", "id": "DFE (Decision Feedback Equalizer)." },
    { "en": "Apa Itu Modulasi Yang Dikodekan?", "id": "Coded Modulation." },
    { "en": "Apa Itu Bentuk Gelombang (Waveform)?", "id": "Bentuk Sinyal Sebagai Fungsi Waktu." },
    { "en": "Apa Itu Periode?", "id": "Waktu Untuk Satu Siklus Lengkap." },
    { "en": "Apa Itu Panjang Gelombang?", "id": "Jarak Spasial Satu Siklus Gelombang." },
    { "en": "Apa Itu Frekuensi?", "id": "Jumlah Siklus Per Detik." },
    { "en": "Apa Itu Fasa?", "id": "Posisi Sinyal Dalam Siklusnya." },
    { "en": "Apa Itu Amplitudo?", "id": "Kekuatan Atau Magnitudo Maksimum Sinyal." },
    { "en": "Apa Itu Sinyal Deterministik?", "id": "Sinyal Yang Dapat Diprediksi." },
    { "en": "Apa Itu Sinyal Stokastik (Acak)?", "id": "Sinyal Yang Tidak Dapat Diprediksi." },
    { "en": "Apa Itu Proses Stasioner?", "id": "Proses Acak Dengan Statistik Konstan." },
    { "en": "Apa Itu Proses Ergodik?", "id": "Rata-rata Waktu Sama Dengan Rata-rata Ensemble." },
    { "en": "Apa Itu Korelasi Otomatis?", "id": "Korelasi Sinyal Dengan Versi Tertundanya." },
    { "en": "Apa Itu Korelasi Silang?", "id": "Korelasi Antara Dua Sinyal Berbeda." },
    { "en": "Apa Itu Konvolusi?", "id": "Operasi Matematis Merepresentasikan Efek Filter." },
    { "en": "Apa Itu Respon Impuls?", "id": "Output Filter Terhadap Input Impuls." },
    { "en": "Apa Itu Fungsi Transfer?", "id": "Representasi Filter Dalam Domain Frekuensi." },
    { "en": "Apa Itu Modulasi Digital Koheren?", "id": "Demodulasi Membutuhkan Referensi Fasa." },
    { "en": "Apa Itu Modulasi Digital Non-Koheren?", "id": "Demodulasi Tidak Butuh Referensi Fasa." },
    { "en": "Apa Itu Modulasi Kuadratur?", "id": "Menggunakan Dua Pembawa Ortogonal." },
    { "en": "Apa Itu Modulasi Pita Samping Tunggal?", "id": "SSB (Single Sideband)." },
    { "en": "Apa Itu Modulasi Frekuensi Fasa Kontinu?", "id": "CPFSK (Continuous Phase Frequency Shift Keying)." },
    { "en": "Apa Itu Kode Konvolusi?", "id": "Jenis Kode Koreksi Error." },
    { "en": "Apa Itu Kode Blok Linear?", "id": "Jenis Kode Koreksi Error Lainnya." },
    { "en": "Apa Itu Jarak Hamming?", "id": "Jumlah Posisi Bit Yang Berbeda." },
    { "en": "Apa Itu Bobot Hamming?", "id": "Jumlah Bit '1' Dalam Kata Kode." },
    { "en": "Apa Itu Kode Siklik?", "id": "Sub-kelas Kode Blok Linear." },
    { "en": "Apa Itu Kode BCH?", "id": "Jenis Kode Siklik Kuat." },
    { "en": "Apa Itu Pengkodean Diferensial?", "id": "Mengkodekan Perbedaan Antara Simbol." },
    { "en": "Apa Itu Dekode Maximum Likelihood?", "id": "Mencari Urutan Paling Mungkin." },
    { "en": "Apa Itu Algoritma Berlecamp-Massey?", "id": "Algoritma Dekoding Untuk Kode BCH/RS." },
    { "en": "Apa Itu Dekode Soft-Input Soft-Output?", "id": "SISO (Soft-Input Soft-Output) Decoder." },
    { "en": "Apa Itu Kode Polar?", "id": "Kode Yang Terbukti Mencapai Kapasitas Kanal." },
    { "en": "Apa Itu Kode Fountain?", "id": "Kode Erasure Tanpa Laju." },
    { "en": "Apa Itu Modulasi Spasial Umum?", "id": "GSM (Generalized Spatial Modulation)." },
    { "en": "Apa Itu Modulasi Cincin?", "id": "Ring Modulation." },
    { "en": "Apa Itu Modulasi Kunci Geser Fasa Ternary?", "id": "TPSK (Ternary Phase Shift Keying)." },
    { "en": "Apa Itu Modulasi Kunci Geser Fasa Biner?", "id": "BPSK (Binary Phase-Shift Keying)." },
    { "en": "Apa Itu Modulasi Kunci Geser Frekuensi Gaussian?", "id": "GFSK (Gaussian Frequency-Shift Keying)." },
    { "en": "Apa Itu Modulasi Akses Ganda Pembagian Frekuensi Ortogonal?", "id": "OFDMA (Orthogonal Frequency Division Multiple Access)." },
    { "en": "Apa Itu Modulasi Pembawa Tunggal Pembagian Frekuensi?", "id": "SC-FDMA (Single-Carrier Frequency-Division Multiple Access)." },
    { "en": "Apa Itu Modulasi Radio Digital?", "id": "DRM (Digital Radio Mondiale)." },
    { "en": "Apa Itu Modulasi Pita Samping Ganda?", "id": "DSB (Double Sideband)." },
    { "en": "Apa Itu Modulasi Amplitudo Dan Fasa?", "id": "APM (Amplitude and Phase Modulation)." },
    { "en": "Apa Itu Modulasi Kunci Geser Fasa Amplitudo?", "id": "APSK (Amplitude Phase-Shift Keying)." },
    { "en": "Apa Itu Pengkodean Prediktif Linear?", "id": "LPC (Linear Predictive Coding)." },
    { "en": "Apa Itu Modulasi Eksitasi Pulsa Kode?", "id": "CELP (Code-Excited Linear Prediction)." },
    { "en": "Apa Itu Sinyal Campuran Fasa-Amplitudo?", "id": "AM-PM (Amplitude Modulation to Phase Modulation)." },
    { "en": "Apa Itu Modulasi Dengan Laju Adaptif?", "id": "ARM (Adaptive Rate Modulation)." },
    { "en": "Apa Itu Modulasi Jalur Balik?", "id": "Backscatter Modulation." },
    { "en": "Apa Itu Modulasi Kunci Geser Frekuensi Ortogonal?", "id": "OFSK (Orthogonal Frequency-Shift Keying)." },
    { "en": "Apa Itu Modulasi Kunci Geser Fasa Offset?", "id": "OQPSK (Offset Quadrature Phase-Shift Keying)." },
    { "en": "Apa Itu Modulasi Kunci Fasa Minimum?", "id": "MSK (Minimum-Shift Keying)." },
    { "en": "Apa Itu Modulasi Denyut-Waktu?", "id": "PTM (Pulse-Time Modulation)." },
    { "en": "Apa Itu Modulasi Durasi-Denyut?", "id": "PDM (Pulse-Duration Modulation)." },
    { "en": "Apa Itu Modulasi Frekuensi-Denyut?", "id": "PFM (Pulse-Frequency Modulation)." },
    { "en": "Apa Itu Modulasi Vektor 8-PSK?", "id": "8-PSK (8-Phase Shift Keying)." },
    { "en": "Apa Itu Modulasi Kunci Fasa Diferensial?", "id": "DPSK (Differential Phase Shift Keying)." },
    { "en": "Apa Itu Modulasi Fasa Kontinu?", "id": "CPM (Continuous Phase Modulation)." },
    { "en": "Apa Itu Sistem Komunikasi Nirkabel?", "id": "WCS (Wireless Communication System)." },
    { "en": "Apa Itu Sistem Satelit Bergerak?", "id": "MSS (Mobile Satellite System)." },
    { "en": "Apa Itu Sistem Penyiaran Satelit?", "id": "BSS (Broadcast Satellite System)." },
    { "en": "Apa Itu Sistem Layanan Satelit Tetap?", "id": "FSS (Fixed-Service Satellite)." },
    { "en": "Apa Itu Stasiun Bumi?", "id": "ES (Earth Station)." },
    { "en": "Apa Itu Terminal Apertur Sangat Kecil?", "id": "VSAT (Very Small Aperture Terminal)." },
    { "en": "Apa Itu Jaringan Area Luas Daya Rendah?", "id": "LPWAN (Low-Power Wide-Area Network)." },
    { "en": "Apa Itu Komunikasi Nirkabel Jarak Pendek?", "id": "SRD (Short-Range Device)." },
    { "en": "Apa Itu Jaringan Area Tubuh?", "id": "BAN (Body Area Network)." },
    { "en": "Apa Itu Jaringan Area Pribadi?", "id": "PAN (Personal Area Network)." },
    { "en": "Apa Itu Jaringan Area Lokal?", "id": "LAN (Local Area Network)." },
    { "en": "Apa Itu Jaringan Area Metropolitan?", "id": "MAN (Metropolitan Area Network)." },
    { "en": "Apa Itu Jaringan Area Luas?", "id": "WAN (Wide Area Network)." },
    { "en": "Apa Itu Sistem Global Untuk Komunikasi Seluler?", "id": "GSM (Global System for Mobile Communications)." },
    { "en": "Apa Itu Layanan Radio Paket Umum?", "id": "GPRS (General Packet Radio Service)." },
    { "en": "Apa Itu Peningkatan Laju Data Untuk Evolusi GSM?", "id": "EDGE (Enhanced Data rates for GSM Evolution)." },
    { "en": "Apa Itu Sistem Telekomunikasi Seluler Universal?", "id": "UMTS (Universal Mobile Telecommunications System)." },
    { "en": "Apa Itu Akses Paket Kecepatan Tinggi?", "id": "HSPA (High-Speed Packet Access)." },
    { "en": "Apa Itu Evolusi Jangka Panjang?", "id": "LTE (Long-Term Evolution)." },
    { "en": "Apa Itu Arsitektur Jaringan Terdistribusi?", "id": "DAN (Distributed Antenna Network)." },
    { "en": "Apa Itu Radio Kognitif?", "id": "CR (Cognitive Radio)." },
    { "en": "Apa Itu Modulasi Optik?", "id": "Mengubah Sifat Gelombang Cahaya." },
    { "en": "Apa Itu Modulasi Intensitas?", "id": "IM (Intensity Modulation)." },
    { "en": "Apa Itu Demodulasi Deteksi Langsung?", "id": "DD (Direct Detection)." },
    { "en": "Apa Itu Modulasi Polarisasi?", "id": "PolM (Polarization Modulation)." },
    { "en": "Apa Itu Multiplexing Divisi Panjang Gelombang?", "id": "WDM (Wavelength-Division Multiplexing)." },
    { "en": "Apa Itu Multiplexing Divisi Panjang Gelombang Padat?", "id": "DWDM (Dense Wavelength-Division Multiplexing)." },
    { "en": "Apa Itu Multiplexing Divisi Panjang Gelombang Kasar?", "id": "CWDM (Coarse Wavelength-Division Multiplexing)." },
    { "en": "Apa Itu Komunikasi Optik Ruang Bebas?", "id": "FSO (Free-Space Optical) Communication." },
    { "en": "Apa Itu Li-Fi (Light Fidelity)?", "id": "Komunikasi Nirkabel Berbasis Cahaya." },
    { "en": "Apa Itu Modulasi Cahaya Tampak?", "id": "VLC (Visible Light Communication)." },
    { "en": "Apa Itu Penguat Optik?", "id": "OA (Optical Amplifier)." },
    { "en": "Apa Itu Penguat Serat Doping Erbium?", "id": "EDFA (Erbium-Doped Fiber Amplifier)." },
    { "en": "Apa Itu Penguat Optik Semikonduktor?", "id": "SOA (Semiconductor Optical Amplifier)." },
    { "en": "Apa Itu Kompensasi Dispersi?", "id": "DC (Dispersion Compensation)." },
    { "en": "Apa Itu Serat Kompensasi Dispersi?", "id": "DCF (Dispersion-Compensating Fiber)." },
    { "en": "Apa Itu Kisi Serat Bragg?", "id": "FBG (Fiber Bragg Grating)." },
    { "en": "Apa Itu Modulasi Akustik-Optik?", "id": "AOM (Acousto-Optic Modulator)." },
    { "en": "Apa Itu Modulasi Elektro-Optik?", "id": "EOM (Electro-Optic Modulator)." },
    { "en": "Apa Itu Modulasi Serapan-Elektro?", "id": "EAM (Electro-Absorption Modulator)." },
    { "en": "Apa Itu Modulator Mach-Zehnder?", "id": "MZM (Mach-Zehnder Modulator)." },
    { "en": "Apa Itu Multiplexing Divisi Polarisasi?", "id": "PDM (Polarization-Division Multiplexing)." },
    { "en": "Apa Itu Transceiver Optik?", "id": "Penerima Dan Pemancar Optik." },
    { "en": "Apa Itu Jaringan Optik Pasif?", "id": "PON (Passive Optical Network)." },
    { "en": "Apa Itu Jaringan Optik Aktif?", "id": "AON (Active Optical Network)." },
    { "en": "Apa Itu Terminal Jaringan Optik?", "id": "ONT (Optical Network Terminal)." },
    { "en": "Apa Itu Terminal Jalur Optik?", "id": "OLT (Optical Line Terminal)." },
    { "en": "Apa Itu Jaringan Transportasi Optik?", "id": "OTN (Optical Transport Network)." },
    { "en": "Apa Itu Hirarki Digital Sinkron?", "id": "SDH (Synchronous Digital Hierarchy)." },
    { "en": "Apa Itu Jaringan Optik Sinkron?", "id": "SONET (Synchronous Optical Network)." },
    { "en": "Apa Itu Sinyal Pembawa Optik?", "id": "OC (Optical Carrier) Signal." },
    { "en": "Apa Itu Modulasi Time-Correlated Single Photon Counting?", "id": "TCSPC (Time-Correlated Single Photon Counting)." },
    { "en": "Apa Itu Modulasi Lebar Garis?", "id": "LWM (Linewidth Modulation)." },
    { "en": "Apa Itu Modulasi Frekuensi Chirp?", "id": "CFM (Chirped Frequency Modulation)." },
    { "en": "Apa Itu Deteksi Interferometrik?", "id": "Metode Demodulasi Fasa Optik." },
    { "en": "Apa Itu Modulasi Bipolar?", "id": "Menggunakan Level Tegangan Positif Dan Negatif." },
    { "en": "Apa Itu Modulasi Unipolar?", "id": "Menggunakan Level Tegangan Nol Dan Positif." },
    { "en": "Apa Itu Kode Kembali-ke-Nol (Return-to-Zero)?", "id": "RZ (Return-to-Zero)." },
    { "en": "Apa Itu Kode Tidak-Kembali-ke-Nol (Non-Return-to-Zero)?", "id": "NRZ (Non-Return-to-Zero)." },
    { "en": "Apa Itu NRZI (Non-Return-to-Zero Inverted)?", "id": "Inversi Sinyal Untuk Bit '1'." },
    { "en": "Apa Itu Kode Bipolar AMI (Alternate Mark Inversion)?", "id": "Level Tegangan Bergantian Untuk Bit '1'." },
    { "en": "Apa Itu Kode Pseudoternary?", "id": "Bit '0' Aktif, Bit '1' Nol." },
    { "en": "Apa Itu Kode Manchester?", "id": "Transisi Di Tengah Bit." },
    { "en": "Apa Itu Kode Manchester Diferensial?", "id": "Informasi Dalam Ada/Tidaknya Transisi." },
    { "en": "Apa Itu Kode Blok?", "id": "Mengganti Blok m-bit Dengan Blok n-bit." },
    { "en": "Apa Itu Kode 3B/4B?", "id": "Mengkodekan 3 Bit Menjadi 4 Bit." },
    { "en": "Apa Itu Kode 5B/6B?", "id": "Mengkodekan 5 Bit Menjadi 6 Bit." },
    { "en": "Apa Itu Kode 64B/66B?", "id": "Mengkodekan 64 Bit Menjadi 66 Bit." },
    { "en": "Apa Itu Scrambler?", "id": "Mengacak Urutan Bit." },
    { "en": "Apa Itu Descrambler?", "id": "Mengembalikan Urutan Bit Asli." },
    { "en": "Apa Itu Modulasi Audio?", "id": "Memodulasi Sinyal Pembawa Dengan Suara." },
    { "en": "Apa Itu Modulasi Video?", "id": "Memodulasi Sinyal Pembawa Dengan Gambar." },
    { "en": "Apa Itu Modulasi Data?", "id": "Memodulasi Sinyal Pembawa Dengan Data Digital." },
    { "en": "Apa Itu Modulasi Satelit Siaran Langsung?", "id": "DBS (Direct Broadcast Satellite)." },
    { "en": "Apa Itu Modulasi Kunci Geser Fasa Amplitudo?", "id": "APSK (Amplitude Phase-Shift Keying)." },
    { "en": "Apa Itu Sistem Komunikasi Data?", "id": "DCS (Data Communication System)." },
    { "en": "Apa Itu Terminal Data?", "id": "DTE (Data Terminal Equipment)." },
    { "en": "Apa Itu Peralatan Komunikasi Data?", "id": "DCE (Data Communication Equipment)." },
    { "en": "Apa Itu Modem Pita Suara?", "id": "Mengirim Data Melalui Saluran Telepon." },
    { "en": "Apa Itu Antarmuka Serial RS-232?", "id": "Standar Komunikasi Serial." },
    { "en": "Apa Itu Sinyal Siap Kirim?", "id": "RTS (Request to Send)." },
    { "en": "Apa Itu Sinyal Jelas Untuk Mengirim?", "id": "CTS (Clear to Send)." },
    { "en": "Apa Itu Sinyal Data Diterima?", "id": "RXD (Receive Data)." },
    { "en": "Apa Itu Sinyal Data Dipancarkan?", "id": "TXD (Transmit Data)." },
    { "en": "Apa Itu Sinyal Pembawa Terdeteksi?", "id": "DCD (Data Carrier Detect)." },
    { "en": "Apa Itu Pengkodean Huffman?", "id": "Metode Kompresi Statistik." },
    { "en": "Apa Itu Pengkodean Shannon-Fano?", "id": "Metode Kompresi Entropi Lainnya." },
    { "en": "Apa Itu Efek Memori Kanal?", "id": "Output Saat Ini Tergantung Input Sebelumnya." },
    { "en": "Apa Itu Kanal Tanpa Memori?", "id": "Output Saat Ini Hanya Tergantung Input Saat Ini." },
    { "en": "Apa Itu Selektivitas Kanal Sebelah?", "id": "ACS (Adjacent-Channel Selectivity)." },
    { "en": "Apa Itu Penolakan Kanal Sebelah?", "id": "ACR (Adjacent-Channel Rejection)." },
    { "en": "Apa Itu Titik Intersep?", "id": "IP (Intercept Point)." },
    { "en": "Apa Itu Derau Intermodulasi?", "id": "Produk Distorsi Akibat Non-Linearitas." },
    { "en": "Apa Itu Penguatan Konversi?", "id": "Conversion Gain." },
    { "en": "Apa Itu Kerugian Konversi?", "id": "Conversion Loss." },
    { "en": "Apa Itu Isolasi Port?", "id": "Ukuran Kebocoran Antar Port Mixer." },
    { "en": "Apa Itu Modulasi Digital Pada Pembawa Kuadratur?", "id": "QAM (Quadrature Amplitude Modulation)." },
    { "en": "Apa Itu Modulasi Denyut Digital?", "id": "Mengubah Aliran Bit Menjadi Pulsa." },
    { "en": "Apa Itu Sinkronisasi Bit?", "id": "Menentukan Momen Tepat Untuk Sampling." },
    { "en": "Apa Itu Sinkronisasi Kata?", "id": "Menentukan Batas Antar Kata Kode." },
    { "en": "Apa Itu Sinyal Sinkronisasi Unik?", "id": "UW (Unique Word)." },
    { "en": "Apa Itu Preamble?", "id": "Urutan Bit Di Awal Frame." },
    { "en": "Apa Fungsi Preamble?", "id": "Sinkronisasi Waktu Dan Frekuensi." },
    { "en": "Apa Itu Header?", "id": "Informasi Kontrol Di Awal Paket." },
    { "en": "Apa Itu Payload?", "id": "Data Aktual Dalam Sebuah Paket." },
    { "en": "Apa Itu Trailer?", "id": "Informasi Tambahan Di Akhir Paket." },
    { "en": "Apa Itu Deteksi Urutan?", "id": "Menemukan Urutan Bit Tertentu." },
    { "en": "Apa Itu Kanal Pita Terbatas?", "id": "Kanal Dengan Bandwidth Terbatas." },
    { "en": "Apa Itu Kanal Terdistorsi?", "id": "Kanal Yang Mengubah Bentuk Sinyal." },
    { "en": "Apa Itu Equalizer Adaptif?", "id": "Equalizer Yang Dapat Menyesuaikan Diri." },
    { "en": "Apa Itu Algoritma LMS (Least Mean Squares)?", "id": "Algoritma Adaptif Untuk Equalizer." },
    { "en": "Apa Itu Urutan Pelatihan?", "id": "Sinyal Dikenal Untuk Mengatur Equalizer." },
    { "en": "Apa Itu Equalizer Buta?", "id": "Bekerja Tanpa Urutan Pelatihan." },
    { "en": "Apa Itu Keputusan Umpan Balik?", "id": "Menggunakan Simbol Terdahulu Untuk Koreksi." },
    { "en": "Apa Itu Modulasi Kunci Geser Fasa Ganda?", "id": "2PSK (Binary Phase Shift Keying)." },
    { "en": "Apa Itu Modulasi Kunci Geser Fasa Empat?", "id": "4PSK (Quadrature Phase Shift Keying)." },
    { "en": "Apa Itu Modulasi Kunci Geser Frekuensi Ganda?", "id": "2FSK (Binary Frequency Shift Keying)." },
    { "en": "Apa Itu Modulasi Untuk Sistem RFID?", "id": "ASK (Amplitude-Shift Keying) atau PSK (Phase-Shift Keying)." },
    { "en": "Apa Itu Komunikasi Optik Nirkabel?", "id": "OWC (Optical Wireless Communication)." },
    { "en": "Apa Itu Modulasi Dengan Laju Sampel Pecahan?", "id": "FSR (Fractional-Sample Rate) Modulation." },
    { "en": "Apa Itu Modulasi Dengan Tunda Waktu?", "id": "TDM (Time-Delay Modulation)." },
    { "en": "Apa Itu Modulasi Spektrum Chirp?", "id": "CSS (Chirp Spread Spectrum)." },
    { "en": "Apa Itu Modulasi Dalam Sistem PLC (Power Line Communication)?", "id": "OFDM (Orthogonal Frequency-Division Multiplexing)." },
    { "en": "Apa Itu Sistem Komunikasi Troposcatter?", "id": "Menggunakan Hamburan Sinyal Di Troposfer." },
    { "en": "Apa Itu Sistem Komunikasi Meteor Burst?", "id": "Menggunakan Jejak Meteor Untuk Komunikasi." },
    { "en": "Apa Itu Komunikasi Frekuensi Sangat Rendah?", "id": "VLF (Very Low Frequency)." },
    { "en": "Di Mana VLF (Very Low Frequency) Digunakan?", "id": "Komunikasi Dengan Kapal Selam." },
    { "en": "Apa Itu Komunikasi Gelombang Milimeter?", "id": "Komunikasi Di Atas 30 GHz." },
    { "en": "Apa Keuntungan Gelombang Milimeter?", "id": "Bandwidth Sangat Lebar." },
    { "en": "Apa Kerugian Gelombang Milimeter?", "id": "Rentan Terhadap Hujan Dan Hambatan." },
    { "en": "Apa Itu Jaringan Nirkabel Kepadatan Tinggi?", "id": "HDWN (High-Density Wireless Network)." },
    { "en": "Apa Itu Perangkat-ke-Perangkat?", "id": "D2D (Device-to-Device) Communication." },
    { "en": "Apa Itu Jaringan Yang Berpusat Pada Pengguna?", "id": "User-Centric Network." },
    { "en": "Apa Itu Komunikasi Molekuler?", "id": "Menggunakan Molekul Untuk Mengirim Informasi." },
    { "en": "Apa Itu Teori Informasi?", "id": "Studi Matematis Kuantifikasi Informasi." },
    { "en": "Apa Itu Entropi?", "id": "Ukuran Ketidakpastian Atau Informasi." },
    { "en": "Apa Itu Informasi Timbal Balik?", "id": "Ukuran Ketergantungan Antar Variabel." },
    { "en": "Apa Itu Redundansi?", "id": "Informasi Berlebih Dalam Pesan." },
    { "en": "Apa Itu Kriptanalisis?", "id": "Seni Memecahkan Kode Terenkripsi." },
    { "en": "Apa Itu Serangan Brute-Force?", "id": "Mencoba Semua Kemungkinan Kunci." },
    { "en": "Apa Itu Serangan Man-in-the-Middle?", "id": "MITM (Man-in-the-Middle) Attack." },
    { "en": "Apa Itu Jaringan Area Sensor?", "id": "SAN (Sensor Area Network)." },
    { "en": "Apa Itu Modulasi Dalam Jaringan Otomotif?", "id": "CAN (Controller Area Network) Bus Signaling." },
    { "en": "Apa Itu Protokol FlexRay?", "id": "Protokol Komunikasi Otomotif Kecepatan Tinggi." },
    { "en": "Apa Itu Jaringan MOST (Media Oriented Systems Transport)?", "id": "Jaringan Serat Optik Untuk Multimedia Mobil." },
    { "en": "Apa Itu Modulasi Dalam Sistem Avionik?", "id": "ARINC (Aeronautical Radio, Incorporated) 429." },
    { "en": "Apa Itu Modulasi Kode Manchester Biphase-L?", "id": "Standar Pengkodean Digital." },
    { "en": "Apa Itu Modulasi Untuk Kartu Cerdas Nirkontak?", "id": "ISO/IEC 14443." },
    { "en": "Apa Itu Modulasi Untuk Pembayaran Seluler?", "id": "NFC (Near Field Communication)." },
    { "en": "Apa Itu Modulasi Untuk Penyiaran Audio Digital?", "id": "DAB (Digital Audio Broadcasting)." },
    { "en": "Apa Itu Modulasi Untuk Penyiaran Video Digital?", "id": "DVB (Digital Video Broadcasting)." },
    { "en": "Apa Itu Pemrosesan Sinyal Digital?", "id": "DSP (Digital Signal Processing)." },
    { "en": "Apa Itu Konvolusi?", "id": "Operasi Matematis Dasar Dalam DSP." },
    { "en": "Apa Itu Korelasi?", "id": "Ukuran Kemiripan Antara Dua Sinyal." },
    { "en": "Apa Itu Filter Respons Impuls Terbatas?", "id": "FIR (Finite Impulse Response) Filter." },
    { "en": "Apa Itu Filter Respons Impuls Tak Terbatas?", "id": "IIR (Infinite Impulse Response) Filter." },
    { "en": "Filter Mana Yang Selalu Stabil?", "id": "FIR (Finite Impulse Response) Filter." },
    { "en": "Filter Mana Yang Lebih Efisien Secara Komputasi?", "id": "IIR (Infinite Impulse Response) Filter." },
    { "en": "Apa Itu Transformasi Fourier Cepat?", "id": "FFT (Fast Fourier Transform)." },
    { "en": "Apa Itu Transformasi Z?", "id": "Alat Analisis Untuk Sinyal Diskrit." },
    { "en": "Apa Itu Pole Dan Zero?", "id": "Akar Dari Fungsi Transfer." },
    { "en": "Apa Itu Efek Jendela?", "id": "Mengurangi Kebocoran Spektral Dalam FFT." },
    { "en": "Contoh Fungsi Jendela?", "id": "Hamming, Hanning, Blackman." },
    { "en": "Apa Itu Modulasi Dalam Sistem Audio Digital?", "id": "PCM (Pulse-Code Modulation), DSD (Direct Stream Digital)." },
    { "en": "Apa Itu Direct Stream Digital?", "id": "DSD (Direct Stream Digital)." },
    { "en": "Di Mana DSD (Direct Stream Digital) Digunakan?", "id": "Super Audio CD (SACD)." },
    { "en": "Apa Itu Modulasi Sigma-Delta Satu Bit?", "id": "Dasar Dari DSD (Direct Stream Digital)." },
    { "en": "Apa Itu Komunikasi Full-Duplex In-Band?", "id": "Mengirim Dan Menerima Frekuensi Sama." },
    { "en": "Apa Itu Pembatalan Interferensi Diri?", "id": "SIC (Self-Interference Cancellation)." },
    { "en": "Apa Itu Modulasi Untuk Gelombang Terahertz?", "id": "Komunikasi THz." },
    { "en": "Apa Itu Jaringan Terdefinisi Perangkat Lunak?", "id": "SDN (Software-Defined Networking)." },
    { "en": "Apa Itu Virtualisasi Fungsi Jaringan?", "id": "NFV (Network Functions Virtualization)." },
    { "en": "Apa Itu Modulasi Untuk Internet of Things (IoT)?", "id": "LPWAN (Low-Power Wide-Area Network) Modulation (LoRa, NB-IoT)." }


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
