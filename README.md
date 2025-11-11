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


  {
    "en": "Apa Itu Jembatan Wheatstone?",
    "id": "Pengukur Hambatan Presisi Tinggi."
  },
  {
    "en": "Kapan Jembatan Wheatstone Seimbang?",
    "id": "Saat Arus Galvanometer Nol."
  },
  {
    "en": "Apa Itu Galvanometer?",
    "id": "Alat Deteksi Arus Sangat Kecil."
  },
  {
    "en": "Apa Itu Megger (Mega Ohm Meter)?",
    "id": "Alat Ukur Resistansi Isolasi."
  },
  {
    "en": "Mengapa Megger (Mega Ohm Meter) Menggunakan Tegangan Tinggi?",
    "id": "Menguji Kebocoran Arus Isolasi."
  },
  {
    "en": "Apa Itu Earth Tester?",
    "id": "Alat Ukur Resistansi Pentanahan."
  },
  {
    "en": "Apa Itu Tachometer?",
    "id": "Alat Ukur Kecepatan Putaran (RPM)."
  },
  {
    "en": "Apa Kepanjangan RPM (Revolutions Per Minute)?",
    "id": "Revolutions Per Minute."
  },
  {
    "en": "Apa Itu Lux Meter?",
    "id": "Alat Ukur Intensitas Cahaya."
  },
  {
    "en": "Apa Itu Sound Level Meter?",
    "id": "Alat Ukur Intensitas Suara (Desibel)."
  },
  {
    "en": "Apa Itu Kesalahan (Error) Sistematis?",
    "id": "Kesalahan Akibat Alat Ukur (Offset)."
  },
  {
    "en": "Apa Itu Kesalahan (Error) Acak (Random)?",
    "id": "Kesalahan Akibat Faktor Lingkungan."
  },
  {
    "en": "Apa Itu Bahan Feromagnetik?",
    "id": "Bahan Mudah Ditarik Magnet."
  },
  {
    "en": "Contoh Bahan Feromagnetik?",
    "id": "Besi, Baja, Kobalt, Nikel."
  },
  {
    "en": "Apa Itu Bahan Paramagnetik?",
    "id": "Bahan Sedikit Ditarik Magnet."
  },
  {
    "en": "Contoh Bahan Paramagnetik?",
    "id": "Aluminium, Platinum."
  },
  {
    "en": "Apa Itu Bahan Diamagnetik?",
    "id": "Bahan Sedikit Ditolak Magnet."
  },
  {
    "en": "Contoh Bahan Diamagnetik?",
    "id": "Tembaga, Emas, Air."
  },
  {
    "en": "Apa Itu Retentivitas Magnetik?",
    "id": "Kemampuan Menyimpan Sisa Magnet."
  },
  {
    "en": "Apa Itu Koersivitas Magnetik?",
    "id": "Kekuatan Medan Penghilang Magnet."
  },
  {
    "en": "Apa Itu Magnet Permanen?",
    "id": "Bahan Retentivitas Tinggi."
  },
  {
    "en": "Apa Itu Magnet Lunak (Soft Magnet)?",
    "id": "Bahan Mudah Dimagnetkan Dan Hilang."
  },
  {
    "en": "Aplikasi Magnet Lunak?",
    "id": "Inti Transformator, Elektromagnet."
  },
  {
    "en": "Apa Itu Rangkaian Kombinasional?",
    "id": "Output Tergantung Input Saat Ini."
  },
  {
    "en": "Contoh Rangkaian Kombinasional?",
    "id": "Encoder, Decoder, MUX, Adder."
  },
  {
    "en": "Apa Itu Rangkaian Sekuensial?",
    "id": "Output Tergantung Input Dan State."
  },
  {
    "en": "Contoh Rangkaian Sekuensial?",
    "id": "Flip-Flop, Counter, Register."
  },
  {
    "en": "Apa Kepanjangan RAM (Random Access Memory) Statik (SRAM)?",
    "id": "Static Random Access Memory."
  },
  {
    "en": "Apa Struktur Sel Memori SRAM?",
    "id": "Menggunakan Flip-Flop (Latch)."
  },
  {
    "en": "Apa Kepanjangan RAM (Random Access Memory) Dinamik (DRAM)?",
    "id": "Dynamic Random Access Memory."
  },
  {
    "en": "Apa Struktur Sel Memori DRAM?",
    "id": "Kapasitor Dan Transistor."
  },
  {
    "en": "Kekurangan DRAM (Dynamic RAM)?",
    "id": "Harus Di-Refresh Secara Periodik."
  },
  {
    "en": "Mana Lebih Cepat (SRAM/DRAM)?",
    "id": "SRAM (Static RAM)."
  },
  {
    "en": "Mana Lebih Padat (SRAM/DRAM)?",
    "id": "DRAM (Dynamic RAM)."
  },
  {
    "en": "Apa Itu Seven Segment Display?",
    "id": "Penampil Angka Digital Tujuh Segmen."
  },
  {
    "en": "Apa Tipe Seven Segment Common Anode?",
    "id": "Anoda Terhubung Bersama (Ke VCC)."
  },
  {
    "en": "Apa Tipe Seven Segment Common Cathode?",
    "id": "Katoda Terhubung Bersama (Ke Ground)."
  },
  {
    "en": "Apa Kepanjangan LCD (Liquid Crystal Display)?",
    "id": "Liquid Crystal Display."
  },
  {
    "en": "Apa Itu Pixel (Picture Element)?",
    "id": "Titik Terkecil Pada Layar."
  },
  {
    "en": "Apa Itu Thyristor?",
    "id": "Keluarga Komponen Saklar Semikonduktor."
  },
  {
    "en": "Anggota Keluarga Thyristor?",
    "id": "SCR, TRIAC, DIAC, GTO."
  },
  {
    "en": "Apa Kepanjangan GTO (Gate Turn-Off Thyristor)?",
    "id": "Gate Turn-Off Thyristor."
  },
  {
    "en": "Keunggulan GTO (Gate Turn-Off Thyristor)?",
    "id": "Bisa Dimatikan Melalui Gate."
  },
  {
    "en": "Apa Kepanjangan UJT (Unijunction Transistor)?",
    "id": "Unijunction Transistor."
  },
  {
    "en": "Aplikasi UJT (Unijunction Transistor)?",
    "id": "Osilator Relaksasi, Pemicu SCR."
  },
  {
    "en": "Apa Itu Dioda Laser?",
    "id": "Dioda Penghasil Cahaya Laser."
  },
  {
    "en": "Apa Itu Efek Kulit (Skin Effect)?",
    "id": "Arus AC Mengalir Di Permukaan Konduktor."
  },
  {
    "en": "Kapan Skin Effect (Efek Kulit) Signifikan?",
    "id": "Pada Frekuensi Sangat Tinggi."
  },
  {
    "en": "Dampak Skin Effect (Efek Kulit)?",
    "id": "Meningkatkan Resistansi Efektif Kabel."
  },
  {
    "en": "Apa Itu Efek Corona (Corona Discharge)?",
    "id": "Pelepasan Listrik Di Sekitar Konduktor."
  },
  {
    "en": "Kapan Efek Corona Terjadi?",
    "id": "Pada Saluran Transmisi Tegangan Tinggi."
  },
  {
    "en": "Tanda-Tanda Efek Corona?",
    "id": "Cahaya Ungu, Suara Mendesis."
  },
  {
    "en": "Dampak Negatif Efek Corona?",
    "id": "Kehilangan Daya, Interferensi Radio."
  },
  {
    "en": "Apa Itu Isolator (Insulator) Listrik?",
    "id": "Penyekat Bahan Konduktif."
  },
  {
    "en": "Bahan Isolator Jaringan Transmisi?",
    "id": "Keramik Atau Gelas (Porselen)."
  },
  {
    "en": "Apa Itu Isolator Tipe Pin (Pin Insulator)?",
    "id": "Isolator Untuk Jaringan Distribusi."
  },
  {
    "en": "Apa Itu Isolator Tipe Gantung (Suspension Insulator)?",
    "id": "Rangkaian Piringan Isolator (SUTET)."
  },
  {
    "en": "Apa Itu Konduktivitas (Conductivity)?",
    "id": "Kemampuan Material Menghantar Listrik."
  },
  {
    "en": "Konduktivitas Adalah Kebalikan Dari Apa?",
    "id": "Resistivitas (Hambatan Jenis)."
  },
  {
    "en": "Logam Konduktivitas Terbaik?",
    "id": "Perak (Silver)."
  },
  {
    "en": "Mengapa Kabel Banyak Menggunakan Tembaga?",
    "id": "Konduktivitas Baik, Harga Wajar."
  },
  {
    "en": "Mengapa SUTET Menggunakan Aluminium?",
    "id": "Ringan Dan Cukup Konduktif."
  },
  {
    "en": "Apa Kepanjangan ACSR (Aluminium Conductor Steel Reinforced)?",
    "id": "Aluminium Conductor Steel Reinforced."
  },
  {
    "en": "Fungsi Inti Baja (Steel) Pada Kabel ACSR?",
    "id": "Kekuatan Mekanis (Menahan Beban Tarik)."
  },
  {
    "en": "Apa Itu Modulasi Digital?",
    "id": "Penumpangan Sinyal Digital Ke Carrier."
  },
  {
    "en": "Apa Kepanjangan ASK (Amplitude Shift Keying)?",
    "id": "Amplitude Shift Keying."
  },
  {
    "en": "Apa Yang Berubah Pada Sinyal ASK?",
    "id": "Amplitudo (Bit 1 Amplitudo, Bit 0 Nol)."
  },
  {
    "en": "Apa Kepanjangan FSK (Frequency Shift Keying)?",
    "id": "Frequency Shift Keying."
  },
  {
    "en": "Apa Yang Berubah Pada Sinyal FSK?",
    "id": "Frekuensi (Dua Frekuensi Berbeda)."
  },
  {
    "en": "Apa Kepanjangan PSK (Phase Shift Keying)?",
    "id": "Phase Shift Keying."
  },
  {
    "en": "Apa Yang Berubah Pada Sinyal PSK?",
    "id": "Fasa Sinyal."
  },
  {
    "en": "Apa Kepanjangan QAM (Quadrature Amplitude Modulation)?",
    "id": "Quadrature Amplitude Modulation."
  },
  {
    "en": "Apa Itu QAM (Quadrature Amplitude Modulation)?",
    "id": "Kombinasi Modulasi ASK Dan PSK."
  },
  {
    "en": "Apa Itu Spektrum Elektromagnetik?",
    "id": "Rentang Semua Frekuensi Radiasi EM."
  },
  {
    "en": "Urutan Spektrum EM (Frekuensi Terendah)?",
    "id": "Radio, Mikro, Infra Merah."
  },
  {
    "en": "Urutan Spektrum EM (Frekuensi Tertinggi)?",
    "id": "Ultra Violet, Sinar-X, Sinar Gamma."
  },
  {
    "en": "Apa Itu Serat Optik (Fiber Optic)?",
    "id": "Media Transmisi Menggunakan Cahaya."
  },
  {
    "en": "Apa Prinsip Kerja Serat Optik?",
    "id": "Pemantulan Sempurna Internal."
  },
  {
    "en": "Apa Keunggulan Serat Optik?",
    "id": "Bandwidth Besar, Kebal Interferensi EMI."
  },
  {
    "en": "Apa Itu Core (Inti) Serat Optik?",
    "id": "Bagian Dalam Tempat Cahaya Merambat."
  },
  {
    "en": "Apa Itu Cladding (Selubung) Serat Optik?",
    "id": "Lapisan Luar Core, Memantulkan Cahaya."
  },
  {
    "en": "Apa Itu Fungsi Alih (Transfer Function)?",
    "id": "Rasio Output Terhadap Input (Domain-S)."
  },
  {
    "en": "Apa Itu Domain-S (Laplace)?",
    "id": "Metode Analisis Sistem Kontrol."
  },
  {
    "en": "Apa Itu Bode Plot?",
    "id": "Grafik Respon Frekuensi (Gain Dan Fasa)."
  },
  {
    "en": "Apa Itu Stabilitas Sistem Kontrol?",
    "id": "Kemampuan Sistem Kembali Seimbang."
  },
  {
    "en": "Apa Itu Sistem Orde Pertama?",
    "id": "Sistem Deskripsi Satu Persamaan Diferensial."
  },
  {
    "en": "Apa Itu Sistem Orde Kedua?",
    "id": "Sistem Deskripsi Dua Persamaan Diferensial."
  },
  {
    "en": "Apa Itu Damping Ratio (Rasio Redaman)?",
    "id": "Menentukan Sifat Osilasi Sistem."
  },
  {
    "en": "Apa Itu Sistem Underdamped?",
    "id": "Mencapai Setpoint Dengan Osilasi."
  },
  {
    "en": "Apa Itu Sistem Overdamped?",
    "id": "Mencapai Setpoint Lambat Tanpa Osilasi."
  },
  {
    "en": "Apa Itu Sistem Critically Damped?",
    "id": "Mencapai Setpoint Tercepat Tanpa Osilasi."
  },
  {
    "en": "Apa Itu Setpoint (SP)?",
    "id": "Nilai Acuan Yang Diinginkan Sistem."
  },
  {
    "en": "Apa Itu Error (Kesalahan) Sistem Kontrol?",
    "id": "Selisih Antara Setpoint Dan Output."
  },
  {
    "en": "Apa Itu Steady State Error?",
    "id": "Error Saat Sistem Stabil (Waktu Tak Terhingga)."
  },
  {
    "en": "Apa Itu Komparator (Comparator) Op-Amp?",
    "id": "Membandingkan Dua Tegangan Input."
  },
  {
    "en": "Output Komparator Op-Amp?",
    "id": "Positif Maksimum Atau Negatif Maksimum."
  },
  {
    "en": "Apa Itu LCR (Inductance Capacitance Resistance) Meter?",
    "id": "Alat Ukur L, C, Dan R."
  },
  {
    "en": "Apa Itu Spectrum Analyzer?",
    "id": "Mengukur Sinyal Domain Frekuensi."
  },
  {
    "en": "Apa Itu Logic Analyzer?",
    "id": "Menganalisis Sinyal Digital Multi-Kanal."
  },
  {
    "en": "Apa IC (Integrated Circuit) Timer Paling Populer?",
    "id": "IC 555."
  },
  {
    "en": "Sebutkan Mode Operasi IC 555?",
    "id": "Astable, Monostable, Bistable."
  },
  {
    "en": "Apa Itu Mode Astable (555)?",
    "id": "Osilator (Pembangkit Pulsa Terus Menerus)."
  },
  {
    "en": "Apa Itu Mode Monostable (555)?",
    "id": "Timer Satu Kali (One-Shot Pulse)."
  },
  {
    "en": "Apa Kepanjangan SSR (Solid State Relay)?",
    "id": "Solid State Relay."
  },
  {
    "en": "Keunggulan SSR (Solid State Relay) Dibanding Relai Mekanis?",
    "id": "Tanpa Bagian Bergerak, Cepat."
  },
  {
    "en": "Komponen Switching Utama SSR (Solid State Relay) AC?",
    "id": "TRIAC Atau SCR."
  },
  {
    "en": "Apa Kepanjangan MCCB (Molded Case Circuit Breaker)?",
    "id": "Molded Case Circuit Breaker."
  },
  {
    "en": "Perbedaan Utama MCB Dan MCCB?",
    "id": "MCCB Untuk Arus Lebih Besar."
  },
  {
    "en": "Apa Kepanjangan ACB (Air Circuit Breaker)?",
    "id": "Air Circuit Breaker."
  },
  {
    "en": "Kapan ACB (Air Circuit Breaker) Digunakan?",
    "id": "Distribusi Utama (Arus Sangat Besar)."
  },
  {
    "en": "Apa Itu Relai Proteksi?",
    "id": "Alat Pengaman Sistem Tenaga Listrik."
  },
  {
    "en": "Apa Itu Proteksi Arus Lebih?",
    "id": "Melindungi Dari Arus Berlebih/Hubung Singkat."
  },
  {
    "en": "Apa Itu Relai Diferensial?",
    "id": "Membandingkan Arus Masuk Dan Keluar."
  },
  {
    "en": "Aplikasi Relai Diferensial?",
    "id": "Proteksi Transformator, Generator, Busbar."
  },
  {
    "en": "Apa Itu Relai Jarak (Distance Relay)?",
    "id": "Proteksi Saluran Transmisi (Impedansi)."
  },
  {
    "en": "Siapa Penemu Aljabar Boolean?",
    "id": "George Boole."
  },
  {
    "en": "Apa Bunyi Hukum De Morgan?",
    "id": "Inversi Penjumlahan Adalah Perkalian Inversi."
  },
  {
    "en": "Apa Itu Peta Karnaugh (K-Map)?",
    "id": "Metode Penyederhanaan Fungsi Logika."
  },
  {
    "en": "Tujuan Utama Peta Karnaugh (K-Map)?",
    "id": "Minimalisasi Jumlah Gerbang Logika."
  },
  {
    "en": "Apa Itu Minterm?",
    "id": "Ekspresi SOP (Sum of Products)."
  },
  {
    "en": "Apa Itu Maxterm?",
    "id": "Ekspresi POS (Product of Sums)."
  },
  {
    "en": "Apa Kepanjangan SOP (Sum of Products)?",
    "id": "Sum of Products (Jumlah Hasil Kali)."
  },
  {
    "en": "Apa Kepanjangan POS (Product of Sums)?",
    "id": "Product of Sums (Hasil Kali Jumlah)."
  },
  {
    "en": "Apa Itu Analisis Fourier?",
    "id": "Menguraikan Sinyal Menjadi Sinusoidal."
  },
  {
    "en": "Apa Itu Deret Fourier?",
    "id": "Representasi Sinyal Periodik."
  },
  {
    "en": "Apa Itu Transformasi Fourier?",
    "id": "Representasi Sinyal Non-Periodik."
  },
  {
    "en": "Apa Itu Sinyal Fundamental?",
    "id": "Frekuensi Terendah Dalam Sinyal."
  },
  {
    "en": "Apa Itu Bandwidth Sinyal?",
    "id": "Rentang Frekuensi Yang Ditempati Sinyal."
  },
  {
    "en": "Apa Itu Decibel (dB)?",
    "id": "Satuan Rasio Logaritmik."
  },
  {
    "en": "Apa Arti Satuan dBm?",
    "id": "Level Daya Relatif Terhadap 1 Miliwatt."
  },
  {
    "en": "Apa Arti Satuan dBW?",
    "id": "Level Daya Relatif Terhadap 1 Watt."
  },
  {
    "en": "Apa Itu Motor Induksi Tiga Fasa?",
    "id": "Motor AC Digerakkan Tiga Fasa."
  },
  {
    "en": "Apa Itu Medan Putar (Rotating Field)?",
    "id": "Medan Magnet Berputar Stator."
  },
  {
    "en": "Apa Itu Kecepatan Sinkron (Ns)?",
    "id": "Kecepatan Medan Putar Stator."
  },
  {
    "en": "Apa Rumus Kecepatan Sinkron (Ns)?",
    "id": "Ns = 120 * f / P."
  },
  {
    "en": "Apa Arti 'P' Dalam Rumus Ns?",
    "id": "Jumlah Kutub (Pole) Motor."
  },
  {
    "en": "Bagaimana Kecepatan Rotor (Nr) Motor Induksi?",
    "id": "Selalu Lebih Lambat Dari Ns."
  },
  {
    "en": "Apa Itu Slip (s) Motor Induksi?",
    "id": "Perbedaan Persentase Antara Ns Dan Nr."
  },
  {
    "en": "Apa Rumus Slip (s)?",
    "id": "s = (Ns - Nr) / Ns."
  },
  {
    "en": "Kapan Nilai Slip (s) = 1 (Maksimum)?",
    "id": "Saat Rotor Diam (Start)."
  },
  {
    "en": "Kapan Nilai Slip (s) = 0 (Minimum)?",
    "id": "Rotor Berputar Secepat Ns (Teoritis)."
  },
  {
    "en": "Sebutkan Tipe Rotor Motor Induksi?",
    "id": "Sangkar Tupai Dan Rotor Lilit."
  },
  {
    "en": "Apa Itu Rotor Sangkar Tupai (Squirrel Cage)?",
    "id": "Batang Konduktor Dihubung Singkat."
  },
  {
    "en": "Apa Itu Rotor Lilit (Wound Rotor)?",
    "id": "Rotor Dengan Lilitan Dan Slip Ring."
  },
  {
    "en": "Mengapa Arus Starting Motor Induksi Tinggi?",
    "id": "Karena Slip = 1 (GGL Balik Nol)."
  },
  {
    "en": "Apa Itu Metode Starting Star-Delta?",
    "id": "Mengurangi Tegangan Start Motor."
  },
  {
    "en": "Tujuan Utama Starting Star-Delta?",
    "id": "Mengurangi Arus Lonjakan Awal."
  },
  {
    "en": "Apa Itu Efek Piezoelektrik?",
    "id": "Material Hasilkan Listrik Saat Ditekan."
  },
  {
    "en": "Apa Contoh Bahan Piezoelektrik?",
    "id": "Kuarsa (Quartz), Keramik PZT."
  },
  {
    "en": "Aplikasi Efek Piezoelektrik?",
    "id": "Sensor Tekanan, Osilator Kristal."
  },
  {
    "en": "Apa Itu Superkonduktor?",
    "id": "Material Hambatan Listrik Nol."
  },
  {
    "en": "Kapan Superkonduktor Bekerja?",
    "id": "Di Bawah Suhu Kritis (Tc)."
  },
  {
    "en": "Apa Itu Efek Meissner?",
    "id": "Penolakan Sempurna Medan Magnet."
  },
  {
    "en": "Apa Itu Waveguide (Pemandu Gelombang)?",
    "id": "Pipa Logam Pemandu Gelombang Mikro."
  },
  {
    "en": "Kapan Waveguide (Pemandu Gelombang) Digunakan?",
    "id": "Frekuensi Sangat Tinggi (Microwave)."
  },
  {
    "en": "Apa Itu Multiplexing (Multipleksi)?",
    "id": "Menggabungkan Banyak Sinyal Satu Saluran."
  },
  {
    "en": "Apa Kepanjangan FDM (Frequency Division Multiplexing)?",
    "id": "Frequency Division Multiplexing."
  },
  {
    "en": "Prinsip FDM (Frequency Division Multiplexing)?",
    "id": "Membagi Pita Frekuensi."
  },
  {
    "en": "Apa Kepanjangan TDM (Time Division Multiplexing)?",
    "id": "Time Division Multiplexing."
  },
  {
    "en": "Prinsip TDM (Time Division Multiplexing)?",
    "id": "Membagi Slot Waktu (Bergantian)."
  },
  {
    "en": "Apa Itu Noise Termal (Johnson-Nyquist)?",
    "id": "Noise Dihasilkan Gerakan Termal Elektron."
  },
  {
    "en": "Apa Itu Dioda IMPATT?",
    "id": "Dioda Osilator Microwave."
  },
  {
    "en": "Apa Itu Sirkulator (Circulator) RF?",
    "id": "Perangkat RF Tiga Port (Satu Arah)."
  },
  {
    "en": "Apa Itu Isolator (Isolator) RF?",
    "id": "Perangkat RF Dua Port (Satu Arah)."
  },
  {
    "en": "Apa Itu Rangkaian Schmitt Trigger?",
    "id": "Komparator Dengan Histeresis."
  },
  {
    "en": "Apa Fungsi Histeresis Schmitt Trigger?",
    "id": "Mencegah Noise Transisi Sinyal."
  },
  {
    "en": "Apa Itu Memristor?",
    "id": "Komponen Pasif Keempat (Resistor Memori)."
  },
  {
    "en": "Apa Kepanjangan PLD (Programmable Logic Device)?",
    "id": "Programmable Logic Device."
  },
  {
    "en": "Apa Kepanjangan CPLD (Complex Programmable Logic Device)?",
    "id": "Complex Programmable Logic Device."
  },
  {
    "en": "Apa Kepanjangan FPGA (Field Programmable Gate Array)?",
    "id": "Field Programmable Gate Array."
  },
  {
    "en": "Perbedaan FPGA Dan Mikrokontroler?",
    "id": "FPGA (Hardware), Mikrokontroler (Software)."
  },
  {
    "en": "Bahasa Pemrograman FPGA?",
    "id": "VHDL Atau Verilog."
  },
  {
    "en": "Apa Kepanjangan VHDL (VHSIC Hardware Description Language)?",
    "id": "VHSIC Hardware Description Language."
  },
  {
    "en": "Apa Itu Jaringan Distribusi Radial?",
    "id": "Sistem Pasokan Satu Arah."
  },
  {
    "en": "Apa Itu Jaringan Distribusi Ring (Cincin)?",
    "id": "Sistem Pasokan Loop Tertutup."
  },
  {
    "en": "Keuntungan Jaringan Ring?",
    "id": "Keandalan Tinggi (Dua Arah Pasokan)."
  },
  {
    "en": "Apa Itu Jaringan Distribusi Spindle?",
    "id": "Gabungan Radial Dan Ring."
  },
  {
    "en": "Apa Itu Recloser (Penutup Balik Otomatis)?",
    "id": "Circuit Breaker Reset Otomatis."
  },
  {
    "en": "Fungsi Recloser (Penutup Balik Otomatis)?",
    "id": "Mengatasi Gangguan Temporer Jaringan."
  },
  {
    "en": "Apa Itu Kapasitas Baterai?",
    "id": "Jumlah Muatan Yang Tersimpan."
  },
  {
    "en": "Apa Satuan Kapasitas Baterai?",
    "id": "Ampere-Hour (Ah) Atau Milliampere-Hour (mAh)."
  },
  {
    "en": "Apa Itu C-Rate Baterai?",
    "id": "Laju Pengisian/Pengosongan Baterai."
  },
  {
    "en": "Apa Arti 1C Pada Baterai?",
    "id": "Mengisi/Mengosongkan Penuh Dalam 1 Jam."
  },
  {
    "en": "Apa Itu Self-Discharge Baterai?",
    "id": "Kehilangan Muatan Saat Tidak Digunakan."
  },
  {
    "en": "Apa Kepanjangan BMS (Battery Management System)?",
    "id": "Battery Management System."
  },
  {
    "en": "Fungsi BMS (Battery Management System)?",
    "id": "Melindungi Baterai (Overcharge, Over-Discharge)."
  },
  {
    "en": "Apa Elektrolit Baterai Aki (Lead-Acid)?",
    "id": "Asam Sulfat (H2SO4)."
  },
  {
    "en": "Apa Kepanjangan Baterai Li-Ion?",
    "id": "Lithium-Ion."
  },
  {
    "en": "Apa Keunggulan Baterai Li-Ion?",
    "id": "Kepadatan Energi Tinggi, Ringan."
  },
  {
    "en": "Apa Kepanjangan Baterai NiCd?",
    "id": "Nickel-Cadmium."
  },
  {
    "en": "Kekurangan Baterai NiCd?",
    "id": "Efek Memori (Memory Effect)."
  },
  {
    "en": "Apa Kepanjangan Baterai NiMH?",
    "id": "Nickel-Metal Hydride."
  },
  {
    "en": "Apa Itu Tang Ampere (Clamp Meter)?",
    "id": "Amperemeter Tanpa Memutus Sirkuit."
  },
  {
    "en": "Prinsip Kerja Tang Ampere?",
    "id": "Induksi Magnetik (Efek Hall)."
  },
  {
    "en": "Apa Itu Konverter Jembatan-H (H-Bridge)?",
    "id": "Sirkuit Kontrol Arah Putar Motor DC."
  },
  {
    "en": "Mengapa Disebut Jembatan-H (H-Bridge)?",
    "id": "Skematik Berbentuk Huruf H."
  },
  {
    "en": "Apa Kepanjangan TTL (Transistor-Transistor Logic)?",
    "id": "Transistor-Transistor Logic."
  },
  {
    "en": "Berapa Tegangan Kerja Logika TTL (Transistor-Transistor Logic)?",
    "id": "Lima Volt (5V)."
  },
  {
    "en": "Apa Kepanjangan CMOS (Complementary Metal Oxide Semiconductor)?",
    "id": "Complementary Metal Oxide Semiconductor."
  },
  {
    "en": "Apa Keunggulan Utama CMOS (Complementary Metal Oxide Semiconductor)?",
    "id": "Konsumsi Daya Sangat Rendah."
  },
  {
    "en": "Apa Kelemahan CMOS (Complementary Metal Oxide Semiconductor)?",
    "id": "Rentan Terhadap Listrik Statis (ESD)."
  },
  {
    "en": "Apa Kepanjangan ESD (Electrostatic Discharge)?",
    "id": "Electrostatic Discharge."
  },
  {
    "en": "Apa Itu Fan-Out Gerbang Logika?",
    "id": "Batas Beban Output Gerbang."
  },
  {
    "en": "Apa Itu Waktu Propagasi (Propagation Delay)?",
    "id": "Waktu Tunda Output Gerbang Logika."
  },
  {
    "en": "Apa Itu Noise Margin (Margin Kebisingan)?",
    "id": "Kekebalan Gerbang Logika Terhadap Noise."
  },
  {
    "en": "Apa Itu Output Tri-State?",
    "id": "Logika 1, 0, Atau High-Z."
  },
  {
    "en": "Apa Status High-Z (Impedansi Tinggi)?",
    "id": "Kondisi Output Terputus (Mengambang)."
  },
  {
    "en": "Apa Itu Sinyal Periodik?",
    "id": "Sinyal Yang Berulang Konstan."
  },
  {
    "en": "Apa Itu Sinyal Energi?",
    "id": "Energi Terbatas, Daya Rata-Rata Nol."
  },
  {
    "en": "Apa Itu Sinyal Daya?",
    "id": "Daya Terbatas, Energi Tak Terbatas."
  },
  {
    "en": "Apa Itu Sistem LTI (Linear Time-Invariant)?",
    "id": "Sistem Linear Dan Time-Invariant."
  },
  {
    "en": "Apa Itu Konvolusi (Convolution)?",
    "id": "Operasi Matematis Dasar Sistem LTI."
  },
  {
    "en": "Apa Itu Respon Impuls (Impulse Response)?",
    "id": "Output Sistem Terhadap Input Impuls."
  },
  {
    "en": "Apa Itu Motor Universal?",
    "id": "Motor Dapat Beroperasi Di AC/DC."
  },
  {
    "en": "Aplikasi Motor Universal?",
    "id": "Blender, Bor Tangan, Vacuum Cleaner."
  },
  {
    "en": "Apa Itu Motor Fasa Belah (Split-Phase)?",
    "id": "Motor Induksi Satu Fasa."
  },
  {
    "en": "Fungsi Kumparan Bantu (Auxiliary) Split-Phase?",
    "id": "Menghasilkan Medan Putar Saat Start."
  },
  {
    "en": "Apa Itu Motor Kapasitor (Capacitor Motor)?",
    "id": "Motor Fasa Belah Dengan Kapasitor."
  },
  {
    "en": "Apa Fungsi Kapasitor Start (Capacitor Start)?",
    "id": "Meningkatkan Torsi Awal Motor."
  },
  {
    "en": "Apa Fungsi Kapasitor Run (Capacitor Run)?",
    "id": "Memperbaiki Efisiensi Saat Beroperasi."
  },
  {
    "en": "Apa Itu Motor Shaded-Pole?",
    "id": "Motor Induksi Satu Fasa Daya Kecil."
  },
  {
    "en": "Aplikasi Motor Shaded-Pole?",
    "id": "Kipas Angin Meja Kecil."
  },
  {
    "en": "Apa Itu Saluran Transmisi (Transmission Line)?",
    "id": "Media Perambatan Energi Elektromagnetik."
  },
  {
    "en": "Apa Itu Penyesuaian Impedansi (Impedance Matching)?",
    "id": "Membuat Impedansi Beban = Sumber."
  },
  {
    "en": "Tujuan Penyesuaian Impedansi?",
    "id": "Transfer Daya Maksimal, Tanpa Pantulan."
  },
  {
    "en": "Apa Itu Gelombang Pantul (Reflected Wave)?",
    "id": "Gelombang Kembali Ke Sumber."
  },
  {
    "en": "Apa Itu Gelombang Berdiri (Standing Wave)?",
    "id": "Interferensi Gelombang Datang Dan Pantul."
  },
  {
    "en": "Apa Kepanjangan SWR (Standing Wave Ratio)?",
    "id": "Standing Wave Ratio."
  },
  {
    "en": "Apa Itu VSWR (Voltage Standing Wave Ratio)?",
    "id": "Rasio Tegangan Maksimum Minimum."
  },
  {
    "en": "Berapa Nilai VSWR (Voltage Standing Wave Ratio) Ideal?",
    "id": "Satu Banding Satu (1:1)."
  },
  {
    "en": "Apa Itu Smith Chart?",
    "id": "Grafik Analisis Saluran Transmisi RF."
  },
  {
    "en": "Apa Itu Generator Sinkron?",
    "id": "Mesin Pengubah Energi Mekanik Ke Listrik AC."
  },
  {
    "en": "Nama Lain Generator Sinkron?",
    "id": "Alternator."
  },
  {
    "en": "Apa Itu Eksitasi (Excitation) Alternator?",
    "id": "Pemberian Arus DC Ke Kumparan Rotor."
  },
  {
    "en": "Fungsi Eksitasi (Excitation)?",
    "id": "Membangkitkan Medan Magnet Rotor."
  },
  {
    "en": "Apa Kepanjangan AVR (Automatic Voltage Regulator)?",
    "id": "Automatic Voltage Regulator."
  },
  {
    "en": "Fungsi AVR (Automatic Voltage Regulator) Pada Alternator?",
    "id": "Menjaga Kestabilan Tegangan Output."
  },
  {
    "en": "Apa Itu Sinkronisasi (Paralleling) Generator?",
    "id": "Proses Paralel Generator Ke Jaringan."
  },
  {
    "en": "Sebutkan Syarat Sinkronisasi Generator?",
    "id": "Tegangan, Frekuensi, Urutan Fasa Sama."
  },
  {
    "en": "Apa Itu Motor Sinkron?",
    "id": "Motor AC Kecepatan Konstan (Sinkron)."
  },
  {
    "en": "Apa Itu Kondensor Sinkron (Synchronous Condenser)?",
    "id": "Motor Sinkron Tanpa Beban Mekanis."
  },
  {
    "en": "Apa Fungsi Kondensor Sinkron?",
    "id": "Perbaikan Faktor Daya Jaringan."
  },
  {
    "en": "Apa Itu Kurva V (V-Curves) Motor Sinkron?",
    "id": "Grafik Arus Jangkar Vs Arus Medan."
  },
  {
    "en": "Apa Kepanjangan SCADA (Supervisory Control And Data Acquisition)?",
    "id": "Supervisory Control And Data Acquisition."
  },
  {
    "en": "Apa Fungsi Utama SCADA (Supervisory Control And Data Acquisition)?",
    "id": "Monitoring Dan Kontrol Sistem Jarak Jauh."
  },
  {
    "en": "Apa Kepanjangan HMI (Human Machine Interface)?",
    "id": "Human Machine Interface."
  },
  {
    "en": "Apa Itu HMI (Human Machine Interface)?",
    "id": "Tampilan Grafis Operator SCADA."
  },
  {
    "en": "Apa Kepanjangan RTU (Remote Terminal Unit)?",
    "id": "Remote Terminal Unit."
  },
  {
    "en": "Fungsi RTU (Remote Terminal Unit) Dalam SCADA?",
    "id": "Unit Akuisisi Data Di Lokasi Terpencil."
  },
  {
    "en": "Apa Itu Studi Aliran Daya (Load Flow)?",
    "id": "Analisis Kondisi Operasi Jaringan Listrik."
  },
  {
    "en": "Tujuan Studi Aliran Daya?",
    "id": "Mengetahui Tegangan, Arus, Daya Sistem."
  },
  {
    "en": "Apa Itu Bus (Rel) Dalam Sistem Tenaga?",
    "id": "Titik Simpul Pertemuan Jaringan."
  },
  {
    "en": "Apa Itu Bus Slack (Slack Bus)?",
    "id": "Bus Referensi Tegangan Dan Sudut Fasa."
  },
  {
    "en": "Apa Itu Bus Beban (Load Bus / PQ Bus)?",
    "id": "Bus P (Daya Aktif) Dan Q (Reaktif) Diketahui."
  },
  {
    "en": "Apa Itu Bus Generator (Generator Bus / PV Bus)?",
    "id": "Bus P (Daya Aktif) Dan V (Tegangan) Diketahui."
  },
  {
    "en": "Apa Itu Lampu Pijar (Incandescent)?",
    "id": "Cahaya Dari Filamen Yang Dipanaskan."
  },
  {
    "en": "Apa Bahan Filamen Lampu Pijar?",
    "id": "Tungsten (Wolfram)."
  },
  {
    "en": "Kelemahan Lampu Pijar?",
    "id": "Sangat Boros Energi (Efisien Rendah)."
  },
  {
    "en": "Apa Kepanjangan Lampu TL (Fluorescent)?",
    "id": "Tubular Lamp."
  },
  {
    "en": "Prinsip Kerja Lampu TL (Fluorescent)?",
    "id": "Pendaran Lapisan Fosfor Oleh UV."
  },
  {
    "en": "Apa Fungsi Ballast Pada Lampu TL?",
    "id": "Membatasi Arus Listrik."
  },
  {
    "en": "Apa Fungsi Starter Pada Lampu TL?",
    "id": "Pemicu Awal (Menutup Membuka Sirkuit)."
  },
  {
    "en": "Apa Kepanjangan CFL (Compact Fluorescent Lamp)?",
    "id": "Compact Fluorescent Lamp."
  },
  {
    "en": "Nama Umum Lampu CFL (Compact Fluorescent Lamp)?",
    "id": "Lampu Hemat Energi (LHE)."
  },
  {
    "en": "Apa Kepanjangan Lampu HID (High Intensity Discharge)?",
    "id": "High Intensity Discharge."
  },
  {
    "en": "Contoh Lampu HID (High Intensity Discharge)?",
    "id": "Merkuri, Sodium (Natrium)."
  },
  {
    "en": "Apa Itu Efisiensi Luminus (Luminous Efficacy)?",
    "id": "Tingkat Kecerahan Cahaya Per Watt."
  },
  {
    "en": "Apa Satuan Efisiensi Luminus?",
    "id": "Lumen Per Watt (Lm/W)."
  },
  {
    "en": "Apa Itu Lumen (lm)?",
    "id": "Satuan Intensitas (Fluks) Cahaya."
  },
  {
    "en": "Apa Itu Temperatur Warna (Color Temperature)?",
    "id": "Ukuran Kualitas Warna Cahaya."
  },
  {
    "en": "Apa Satuan Temperatur Warna?",
    "id": "Kelvin (K)."
  },
  {
    "en": "Temperatur Warna Hangat (Warm White)?",
    "id": "Sekitar 2700K - 3000K."
  },
  {
    "en": "Temperatur Warna Dingin (Cool White)?",
    "id": "Sekitar 5000K - 6500K."
  },
  {
    "en": "Apa Kepanjangan CRI (Color Rendering Index)?",
    "id": "Color Rendering Index."
  },
  {
    "en": "Apa Fungsi CRI (Color Rendering Index)?",
    "id": "Mengukur Keakuratan Warna Objek."
  },
  {
    "en": "Apa Itu Transformator Satu Fasa?",
    "id": "Trafo Sistem AC Satu Fasa."
  },
  {
    "en": "Apa Itu Rasio Lilitan (Turns Ratio) Trafo?",
    "id": "Perbandingan Lilitan Primer Dan Sekunder."
  },
  {
    "en": "Apa Itu Transformator Ideal?",
    "id": "Transformator Tanpa Kerugian Daya."
  },
  {
    "en": "Apa Itu Kerugian Inti (Core Loss) Trafo?",
    "id": "Kerugian Histeresis Dan Arus Eddy."
  },
  {
    "en": "Apa Itu Kerugian Tembaga (Copper Loss) Trafo?",
    "id": "Kerugian Panas (I Kuadrat R) Lilitan."
  },
  {
    "en": "Apa Itu Uji Beban Nol (Open Circuit Test) Trafo?",
    "id": "Mengukur Kerugian Inti Besi."
  },
  {
    "en": "Apa Itu Uji Hubung Singkat (Short Circuit Test) Trafo?",
    "id": "Mengukur Kerugian Tembaga."
  },
  {
    "en": "Apa Itu Autotransformator (Autotransformer)?",
    "id": "Transformator Hanya Memiliki Satu Lilitan."
  },
  {
"en": "Apa Itu Tap Changer Trafo?",
"id": "Alat Pengubah Rasio Lilitan Trafo."
},
{
"en": "Apa Fungsi Minyak Transformator?",
"id": "Pendingin Dan Isolasi."
},
{
"en": "Apa Itu Relai Buchholz?",
"id": "Proteksi Internal Trafo Berbasis Gas."
},
{
"en": "Apa Itu Gel Silika (Silica Gel) Trafo?",
"id": "Penyerap Kelembaban Udara Pernapasan."
},
{
"en": "Apa Itu Konservator Trafo?",
"id": "Tangki Ekspansi Cadangan Minyak."
},
{
"en": "Apa Itu Bushing Transformator?",
"id": "Isolator Terminal Sambungan Trafo."
},
{
"en": "Apa Itu Dioda Flyback (Freewheeling Diode)?",
"id": "Perendam Lonjakan Tegangan Induktif."
},
{
"en": "Dimana Dioda Flyback Dipasang?",
"id": "Paralel Terbalik Dengan Beban Induktif."
},
{
"en": "Apa Itu Tang Crimping (Crimping Tool)?",
"id": "Alat Pemasang Konektor Kabel (Skun)."
},
{
"en": "Apa Itu Kabel Skun (Cable Lug)?",
"id": "Terminal Penyambung Ujung Kabel."
},
  {
    "en": "Apa Kepanjangan IP (Ingress Protection) Rating?",
    "id": "Ingress Protection (Rating Perlindungan)."
  },
  {
    "en": "Apa Arti Digit Pertama IP (Ingress Protection) Rating?",
    "id": "Perlindungan Terhadap Benda Padat."
  },
  {
    "en": "Apa Arti Digit Kedua IP (Ingress Protection) Rating?",
    "id": "Perlindungan Terhadap Benda Cair (Air)."
  },
  {
    "en": "Apa Arti Rating IP68?",
    "id": "Tahan Debu Total, Tahan Terendam Air."
  },
  {
    "en": "Apa Itu Kelas Isolasi (Insulation Class) Termal?",
    "id": "Batas Suhu Operasi Material Isolasi."
  },
  {
    "en": "Apa Batas Suhu Kelas Isolasi A?",
    "id": "Seratus Lima Derajat Celcius."
  },
  {
    "en": "Apa Batas Suhu Kelas Isolasi F?",
    "id": "Seratus Lima Puluh Lima Derajat Celcius."
  },
  {
    "en": "Apa Batas Suhu Kelas Isolasi H?",
    "id": "Seratus Delapan Puluh Derajat Celcius."
  },
  {
    "en": "Apa Itu Sistem Pentanahan TT?",
    "id": "Netral Sumber Dan Bodi Terpisah."
  },
  {
    "en": "Apa Itu Sistem Pentanahan TN (Terra Neutral)?",
    "id": "Netral Sumber Dan Bodi Terhubung."
  },
  {
    "en": "Apa Itu Sistem Pentanahan TN-C (Terra Neutral-Combined)?",
    "id": "Kabel Netral Dan Ground Gabung (PEN)."
  },
  {
    "en": "Apa Itu Sistem Pentanahan TN-S (Terra Neutral-Separated)?",
    "id": "Kabel Netral Dan Ground Terpisah."
  },
  {
    "en": "Apa Itu Sistem Pentanahan TN-C-S (Terra Neutral-Combined-Separated)?",
    "id": "Gabungan TN-C (PLN) Dan TN-S (Rumah)."
  },
  {
    "en": "Apa Itu Sistem Pentanahan IT (Isolated Terra)?",
    "id": "Sumber Listrik Terisolasi Dari Bumi."
  },
  {
    "en": "Apa Itu Elektroda Bumi (Earth Electrode)?",
    "id": "Konduktor Yang Tertanam Di Tanah."
  },
  {
    "en": "Apa Itu Bus Sistem (System Bus)?",
    "id": "Jalur Komunikasi Internal Komputer."
  },
  {
    "en": "Apa Tiga Tipe Bus Sistem?",
    "id": "Bus Alamat, Data, Dan Kontrol."
  },
  {
    "en": "Apa Fungsi Bus Alamat (Address Bus)?",
    "id": "Membawa Informasi Lokasi Memori."
  },
  {
    "en": "Apa Sifat Arah Bus Alamat?",
    "id": "Satu Arah (Unidirectional) Dari CPU."
  },
  {
    "en": "Apa Fungsi Bus Data (Data Bus)?",
    "id": "Membawa Data Antar Komponen."
  },
  {
    "en": "Apa Sifat Arah Bus Data?",
    "id": "Dua Arah (Bidirectional)."
  },
  {
    "en": "Apa Fungsi Bus Kontrol (Control Bus)?",
    "id": "Membawa Sinyal Kontrol Dan Timing."
  },
  {
    "en": "Apa Kepanjangan PROM (Programmable Read Only Memory)?",
    "id": "Programmable Read Only Memory."
  },
  {
    "en": "Bisakah PROM (Programmable ROM) Diprogram Ulang?",
    "id": "Tidak, Hanya Sekali Program."
  },
  {
    "en": "Apa Kepanjangan EPROM (Erasable Programmable ROM)?",
    "id": "Erasable Programmable ROM."
  },
  {
    "en": "Bagaimana Cara Menghapus Memori EPROM?",
    "id": "Menggunakan Sinar Ultra Violet (UV)."
  },
  {
    "en": "Apa Kepanjangan EEPROM (Electrically Erasable PROM)?",
    "id": "Electrically Erasable Programmable ROM."
  },
  {
    "en": "Bagaimana Cara Menghapus Memori EEPROM?",
    "id": "Secara Elektrik (Tanpa Sinar UV)."
  },
  {
    "en": "Apa Itu Memori Flash (Flash Memory)?",
    "id": "Tipe Khusus Dari EEPROM."
  },
  {
    "en": "Apa Kepanjangan SDRAM (Synchronous Dynamic RAM)?",
    "id": "Synchronous Dynamic RAM."
  },
  {
    "en": "Apa Itu Komunikasi Mode Simplex?",
    "id": "Komunikasi Satu Arah Saja."
  },
  {
    "en": "Apa Contoh Komunikasi Simplex?",
    "id": "Siaran Radio Atau Televisi."
  },
  {
    "en": "Apa Itu Komunikasi Mode Half-Duplex?",
    "id": "Dua Arah, Tetapi Bergantian."
  },
  {
    "en": "Apa Contoh Komunikasi Half-Duplex?",
    "id": "Walkie-Talkie (Handy Talky)."
  },
  {
    "en": "Apa Itu Komunikasi Mode Full-Duplex?",
    "id": "Dua Arah, Secara Bersamaan."
  },
  {
    "en": "Apa Contoh Komunikasi Full-Duplex?",
    "id": "Telepon Atau Ponsel."
  },
  {
    "en": "Apa Itu Bit Rate?",
    "id": "Jumlah Bit Yang Ditransfer Per Detik."
  },
  {
    "en": "Apa Satuan Bit Rate?",
    "id": "Bps (Bits Per Second)."
  },
  {
    "en": "Apa Itu Baud Rate?",
    "id": "Jumlah Perubahan Simbol Per Detik."
  },
  {
    "en": "Apa Itu Efek Hall (Hall Effect)?",
    "id": "Tegangan Listrik Timbul Akibat Medan Magnet."
  },
  {
    "en": "Apa Itu Sensor Efek Hall?",
    "id": "Sensor Pendeteksi Medan Magnet."
  },
  {
    "en": "Aplikasi Sensor Efek Hall?",
    "id": "Sensor Kecepatan, Deteksi Posisi."
  },
  {
    "en": "Apa Itu Efek Peltier?",
    "id": "Transfer Panas Akibat Aliran Arus."
  },
  {
    "en": "Apa Itu Elemen Peltier?",
    "id": "Pendingin Termoelektrik (TEC)."
  },
  {
    "en": "Aplikasi Elemen Peltier?",
    "id": "Pendingin CPU, Kulkas Mini Portabel."
  },
  {
    "en": "Apa Kebalikan Dari Efek Peltier?",
    "id": "Efek Seebeck (Dasar Termokopel)."
  },
  {
    "en": "Apa Itu Superkapasitor (Ultracapacitor)?",
    "id": "Kapasitor Densitas Energi Sangat Tinggi."
  },
  {
    "en": "Apa Kepanjangan EDLC (Electric Double-Layer Capacitor)?",
    "id": "Electric Double-Layer Capacitor."
  },
  {
    "en": "Perbedaan Utama Kapasitor Dan Superkapasitor?",
    "id": "Superkapasitor Menyimpan Energi Jauh Lebih Besar."
  },
  {
    "en": "Keunggulan Superkapasitor?",
    "id": "Siklus Cas-Isi Ulang Sangat Cepat."
  },
  {
    "en": "Apa Itu Sekring (Fuse) Tipe Cepat (Fast-Blow)?",
    "id": "Melindungi Komponen Elektronik Sensitif."
  },
  {
    "en": "Apa Itu Sekring (Fuse) Tipe Lambat (Slow-Blow)?",
    "id": "Menahan Arus Lonjakan Awal (Inrush)."
  },
  {
    "en": "Di Mana Sekring (Fuse) Lambat Digunakan?",
    "id": "Rangkaian Motor Atau Catu Daya."
  },
  {
    "en": "Apa Itu Arus Inrush (Inrush Current)?",
    "id": "Lonjakan Arus Tinggi Saat Alat Menyala."
  },
  {
    "en": "Apa Itu Kesalahan Paralaks (Parallax Error)?",
    "id": "Kesalahan Baca Alat Ukur Analog."
  },
  {
    "en": "Penyebab Kesalahan Paralaks?",
    "id": "Posisi Mata Tidak Tegak Lurus Jarum."
  },
  {
    "en": "Apa Itu Kesalahan Pemuatan (Loading Error)?",
    "id": "Efek Alat Ukur Mengubah Rangkaian."
  },
  {
    "en": "Kapan Loading Error Voltmeter Terjadi?",
    "id": "Impedansi Voltmeter Terlalu Rendah."
  },
  {
    "en": "Kapan Loading Error Amperemeter Terjadi?",
    "id": "Impedansi Amperemeter Terlalu Tinggi."
  },
  {
    "en": "Apa Itu Switching (Mode) Lunak (Soft Switching)?",
    "id": "Switching Saat Tegangan Atau Arus Nol."
  },
  {
    "en": "Keuntungan Switching (Mode) Lunak?",
    "id": "Kerugian Daya Rendah, Efisiensi Tinggi."
  },
  {
    "en": "Apa Kepanjangan ZVS (Zero Voltage Switching)?",
    "id": "Zero Voltage Switching."
  },
  {
    "en": "Apa Kepanjangan ZCS (Zero Current Switching)?",
    "id": "Zero Current Switching."
  },
  {
    "en": "Apa Itu Switching (Mode) Keras (Hard Switching)?",
    "id": "Switching Saat Tegangan Dan Arus Ada."
  },
  {
    "en": "Kekurangan Switching (Mode) Keras?",
    "id": "Kerugian Daya Tinggi, Menghasilkan EMI."
  },
  {
    "en": "Apa Kepanjangan SMPS (Switch Mode Power Supply)?",
    "id": "Switch Mode Power Supply."
  },
  {
    "en": "Keunggulan SMPS (Switch Mode Power Supply)?",
    "id": "Efisien, Ringan, Ukuran Kecil."
  },
  {
    "en": "Kekurangan SMPS (Switch Mode Power Supply)?",
    "id": "Noise Output (Ripple) Lebih Tinggi."
  },
  {
    "en": "Apa Itu Rangkaian Resonansi Tangki (Tank Circuit)?",
    "id": "Kombinasi Paralel Induktor Dan Kapasitor."
  },
  {
    "en": "Fungsi Rangkaian Tangki (Tank Circuit)?",
    "id": "Osilator Atau Filter Band-Pass."
  },
  {
    "en": "Apa Itu Faktor Kualitas (Q Factor)?",
    "id": "Ukuran Kualitas Rangkaian Resonansi."
  },
  {
    "en": "Apa Arti Q Factor Tinggi?",
    "id": "Resonansi Tajam (Selektivitas Baik)."
  },
  {
    "en": "Apa Itu Respon Transien (Transient Response)?",
    "id": "Respon Rangkaian Terhadap Perubahan Input."
  },
  {
    "en": "Apa Itu Respon Steady-State?",
    "id": "Perilaku Rangkaian Setelah Waktu Lama."
  },
  {
    "en": "Apa Itu Konstanta Waktu (Time Constant) RC?",
    "id": "Ukuran Waktu Pengisian Kapasitor."
  },
  {
    "en": "Apa Rumus Konstanta Waktu RC (Tau)?",
    "id": "Tau = R * C."
  },
  {
    "en": "Apa Itu Konstanta Waktu (Time Constant) RL?",
    "id": "Ukuran Waktu Kenaikan Arus Induktor."
  },
  {
    "en": "Apa Rumus Konstanta Waktu RL (Tau)?",
    "id": "Tau = L / R."
  },
  {
    "en": "Apa Itu Panjang Gelombang (Wavelength)?",
    "id": "Jarak Fisik Satu Siklus Gelombang."
  },
  {
    "en": "Apa Simbol Panjang Gelombang?",
    "id": "Lambda (λ)."
  },
  {
    "en": "Apa Hubungan Panjang Gelombang Dan Frekuensi?",
    "id": "Berbanding Terbalik."
  },
  {
    "en": "Apa Itu Propagasi Gelombang Tanah (Ground Wave)?",
    "id": "Gelombang Radio Mengikuti Permukaan Bumi."
  },
  {
    "en": "Apa Itu Propagasi Gelombang Langit (Skywave)?",
    "id": "Gelombang Radio Dipantulkan Ionosfer."
  },
  {
    "en": "Frekuensi Apa Yang Dipantulkan Ionosfer?",
    "id": "HF (High Frequency) (3-30 MHz)."
  },
  {
    "en": "Apa Itu Propagasi Garis Pandang (Line-Of-Sight)?",
    "id": "Gelombang Merambat Lurus (VHF/UHF)."
  },
  {
    "en": "Apa Kepanjangan TVS (Transient Voltage Suppressor) Diode?",
    "id": "Transient Voltage Suppressor Diode."
  },
  {
    "en": "Fungsi Dioda TVS (Transient Voltage Suppressor)?",
    "id": "Melindungi Rangkaian Dari Lonjakan Transien."
  },
  {
    "en": "Apa Kepanjangan GDT (Gas Discharge Tube)?",
    "id": "Gas Discharge Tube."
  },
  {
    "en": "Fungsi GDT (Gas Discharge Tube)?",
    "id": "Perlindungan Lonjakan Tegangan Sangat Tinggi."
  },
  {
    "en": "Apa Itu Sekring (Fuse) PTC (Positive Temperature Coefficient) Yang Dapat Direset?",
    "id": "Sekring Pulih Otomatis Setelah Dingin."
  },
  {
    "en": "Nama Populer Sekring (Fuse) PTC (Positive Temperature Coefficient)?",
    "id": "Polyfuse Atau Polyswitch."
  },
  {
    "en": "Apa Itu Pre-Amplifier (Pre-Amp)?",
    "id": "Penguat Sinyal Input Yang Lemah."
  },
  {
    "en": "Apa Itu Power Amplifier (Penguat Daya)?",
    "id": "Penguat Sinyal Untuk Menggerakkan Beban."
  },
  {
    "en": "Apa Itu Crossover Audio?",
    "id": "Filter Pembagi Frekuensi Suara."
  },
  {
    "en": "Apa Itu Woofer?",
    "id": "Speaker Khusus Suara Frekuensi Rendah."
  },
  {
    "en": "Apa Itu Tweeter?",
    "id": "Speaker Khusus Suara Frekuensi Tinggi."
  },
  {
    "en": "Apa Itu Midrange Speaker?",
    "id": "Speaker Khusus Suara Frekuensi Menengah."
  },
  {
    "en": "Apa Kepanjangan Saklar DPDT (Double Pole Double Throw)?",
    "id": "Double Pole Double Throw."
  },
  {
    "en": "Apa Itu Nilai RMS (Root Mean Square)?",
    "id": "Nilai Efektif Sinyal AC."
  },
  {
    "en": "Apa Rumus RMS Gelombang Sinus?",
    "id": "Vpeak Dibagi Akar Dua."
  },
  {
    "en": "Apa Itu Nilai Puncak (Peak Value)?",
    "id": "Amplitudo Maksimum Gelombang."
  },
  {
    "en": "Apa Itu Nilai Puncak-Ke-Puncak (Peak-To-Peak)?",
    "id": "Jarak Antara Puncak Positif Negatif."
  },
  {
    "en": "Apa Itu Faktor Puncak (Crest Factor)?",
    "id": "Rasio Nilai Puncak Terhadap RMS."
  },
  {
    "en": "Apa Itu Sinyal Gelombang Kotak (Square Wave)?",
    "id": "Sinyal Digital Dua Level (Tinggi Rendah)."
  },
  {
    "en": "Apa Itu Sinyal Gelombang Segitiga (Triangle Wave)?",
    "id": "Sinyal Naik Turun Secara Linear."
  },
  {
    "en": "Apa Itu Sinyal Gigi Gergaji (Sawtooth Wave)?",
    "id": "Naik Linear, Turun Seketika."
  },
  {
    "en": "Apa Kepanjangan UART (Universal Asynchronous Receiver-Transmitter)?",
    "id": "Universal Asynchronous Receiver-Transmitter."
  },
  {
    "en": "Apa Fungsi UART (Universal Asynchronous Receiver-Transmitter)?",
    "id": "Komunikasi Serial Asinkron."
  },
  {
    "en": "Apa Itu Komunikasi Sinkron?",
    "id": "Komunikasi Menggunakan Sinyal Clock."
  },
  {
    "en": "Apa Itu Komunikasi Asinkron?",
    "id": "Komunikasi Tanpa Sinyal Clock Bersama."
  },
  {
    "en": "Apa Kepanjangan SPI (Serial Peripheral Interface)?",
    "id": "Serial Peripheral Interface."
  },
  {
    "en": "Apa Saja Jalur Utama SPI?",
    "id": "MISO, MOSI, SCK, CS."
  },
  {
    "en": "Apa Kepanjangan I2C (Inter-Integrated Circuit)?",
    "id": "Inter-Integrated Circuit."
  },
  {
    "en": "Ada Berapa Kabel Protokol I2C?",
    "id": "Dua Kabel (SDA Dan SCL)."
  },
  {
    "en": "Apa Kepanjangan SDA (Serial Data)?",
    "id": "Serial Data Line."
  },
  {
    "en": "Apa Kepanjangan SCL (Serial Clock)?",
    "id": "Serial Clock Line."
  },
  {
    "en": "Apa Kepanjangan IGBT (Insulated Gate Bipolar Transistor)?",
    "id": "Insulated Gate Bipolar Transistor."
  },
  {
    "en": "Apa Keunggulan IGBT (Insulated Gate Bipolar Transistor)?",
    "id": "Gabungan Kecepatan MOSFET Daya BJT."
  },
  {
    "en": "Aplikasi Umum IGBT (Insulated Gate Bipolar Transistor)?",
    "id": "Inverter Daya Tinggi, Mesin Las."
  },
  {
    "en": "Apa Itu Root Locus (Kedudukan Akar)?",
    "id": "Grafik Lokasi Pole Loop Tertutup."
  },
  {
    "en": "Apa Itu Kriteria Stabilitas Nyquist?",
    "id": "Analisis Stabilitas Domain Frekuensi."
  },
  {
    "en": "Apa Itu Gain Margin (GM)?",
    "id": "Ukuran Kestabilan Relatif (Gain)."
  },
  {
    "en": "Apa Itu Phase Margin (PM)?",
    "id": "Ukuran Kestabilan Relatif (Fasa)."
  },
  {
    "en": "Apa Itu Pole (Kutub) Sistem Kontrol?",
    "id": "Akar Penyebut Fungsi Alih."
  },
  {
    "en": "Apa Itu Zero (Nol) Sistem Kontrol?",
    "id": "Akar Pembilang Fungsi Alih."
  },
  {
    "en": "Apa Itu Mesin Keadaan (State Machine)?",
    "id": "Model Perilaku Sistem Sekuensial."
  },
  {
    "en": "Apa Itu Mesin Moore (Moore Machine)?",
    "id": "Output Tergantung State Saat Ini."
  },
  {
    "en": "Apa Itu Mesin Mealy (Mealy Machine)?",
    "id": "Output Tergantung State Dan Input."
  },
  {
    "en": "Apa Itu Kode Gray (Gray Code)?",
    "id": "Kode Biner Perubahan Satu Bit."
  },
  {
    "en": "Keuntungan Kode Gray (Gray Code)?",
    "id": "Mengurangi Error Transisi Posisi."
  },
  {
    "en": "Apa Itu Parity Bit (Bit Paritas)?",
    "id": "Bit Tambahan Untuk Pengecekan Error."
  },
  {
    "en": "Apa Itu Paritas Genap (Even Parity)?",
    "id": "Jumlah Total Bit 1 Adalah Genap."
  },
  {
    "en": "Apa Itu Paritas Ganjil (Odd Parity)?",
    "id": "Jumlah Total Bit 1 Adalah Ganjil."
  },
  {
    "en": "Apa Itu Teorema Sampling Nyquist?",
    "id": "Fs Harus Lebih Besar 2 Fmax."
  },
  {
    "en": "Apa Itu Frekuensi Nyquist?",
    "id": "Setengah Dari Frekuensi Sampling."
  },
  {
    "en": "Apa Itu Aliasing (Sinyal)?",
    "id": "Distorsi Sinyal Akibat Sampling Lambat."
  },
  {
    "en": "Bagaimana Cara Mencegah Aliasing?",
    "id": "Filter Anti-Aliasing (LPF) Sebelum ADC."
  },
  {
    "en": "Apa Itu Kuantisasi (Quantization)?",
    "id": "Pembulatan Nilai Sampel Ke Level Diskret."
  },
  {
    "en": "Apa Itu Error Kuantisasi (Quantization Error)?",
    "id": "Selisih Nilai Asli Dan Kuantisasi."
  },
  {
    "en": "Apa Kepanjangan PCM (Pulse Code Modulation)?",
    "id": "Pulse Code Modulation."
  },
  {
    "en": "Tiga Proses Utama PCM (Pulse Code Modulation)?",
    "id": "Sampling, Kuantisasi, Dan Koding."
  },
  {
    "en": "Apa Kepanjangan NEMA (National Electrical Manufacturers Association)?",
    "id": "National Electrical Manufacturers Association."
  },
  {
    "en": "Apa Kepanjangan IEC (International Electrotechnical Commission)?",
    "id": "International Electrotechnical Commission."
  },
  {
    "en": "Apa Kepanjangan IEEE (Institute of Electrical and Electronics Engineers)?",
    "id": "Institute of Electrical and Electronics Engineers."
  },
  {
    "en": "Apa Kepanjangan SNI (Standar Nasional Indonesia)?",
    "id": "Standar Nasional Indonesia."
  },
  {
    "en": "Apa Kepanjangan PUIL (Persyaratan Umum Instalasi Listrik)?",
    "id": "Persyaratan Umum Instalasi Listrik."
  },
  {
    "en": "Apa Itu Arc Flash (Busur Api Listrik)?",
    "id": "Ledakan Energi Akibat Hubung Singkat."
  },
  {
    "en": "Apa Bahaya Utama Arc Flash?",
    "id": "Panas Ekstrem, Cahaya Silau, Gelombang Kejut."
  },
  {
    "en": "Apa Kepanjangan APD (Alat Pelindung Diri)?",
    "id": "Alat Pelindung Diri."
  },
  {
    "en": "Apa Itu Pakaian Tahan Api (FR Clothing)?",
    "id": "Pakaian Perlindungan Arc Flash."
  },
  {
    "en": "Apa Kepanjangan AWG (American Wire Gauge)?",
    "id": "American Wire Gauge."
  },
  {
    "en": "Bagaimana Hubungan Nilai AWG Dan Diameter Kabel?",
    "id": "AWG Kecil Berarti Diameter Besar."
  },
  {
    "en": "Apa Itu Konektor BNC (Bayonet Neill-Concelman)?",
    "id": "Konektor Koaksial Tipe Kunci Bayonet."
  },
  {
    "en": "Aplikasi Konektor BNC (Bayonet Neill-Concelman)?",
    "id": "Osiloskop, Video Analog."
  },
  {
    "en": "Apa Itu Konektor SMA (SubMiniature Version A)?",
    "id": "Konektor Koaksial Ulir Frekuensi Tinggi."
  },
  {
    "en": "Apa Itu Konektor RJ45 (Registered Jack 45)?",
    "id": "Konektor Standar Jaringan Ethernet."
  },
  {
    "en": "Apa Itu Konektor RJ11 (Registered Jack 11)?",
    "id": "Konektor Standar Saluran Telepon."
  },
  {
    "en": "Apa Itu Kabel UTP (Unshielded Twisted Pair)?",
    "id": "Kabel Terpilin Tanpa Pelindung."
  },
  {
    "en": "Apa Itu Kabel STP (Shielded Twisted Pair)?",
    "id": "Kabel Terpilin Dengan Pelindung Foil."
  },
  {
    "en": "Fungsi Pilinan (Twisted) Pada Kabel UTP?",
    "id": "Mengurangi Interferensi Crosstalk Dan EMI."
  },
  {
    "en": "Apa Itu Common Mode Choke?",
    "id": "Induktor Filter Noise Common Mode."
  },
  {
    "en": "Apa Itu Differential Mode Noise?",
    "id": "Noise Antara Dua Jalur Sinyal."
  },
  {
    "en": "Apa Itu Common Mode Noise?",
    "id": "Noise Yang Sama Pada Dua Jalur."
  },
  {
    "en": "Apa Itu Topologi Flyback SMPS?",
    "id": "Konverter DC-DC Isolasi Satu Saklar."
  },
  {
    "en": "Apa Itu Topologi Forward SMPS?",
    "id": "Konverter DC-DC Isolasi (Trafo Biasa)."
  },
  {
    "en": "Apa Itu Sensor Proximity Induktif?",
    "id": "Sensor Non-Kontak Pendeteksi Logam."
  },
  {
    "en": "Apa Itu Sensor Proximity Kapasitif?",
    "id": "Sensor Non-Kontak Deteksi Beragam Material."
  },
  {
    "en": "Apa Itu Sensor Ultrasonik?",
    "id": "Mengukur Jarak Menggunakan Gelombang Suara."
  },
  {
    "en": "Apa Kepanjangan PIR (Passive Infrared) Sensor?",
    "id": "Passive Infrared Sensor."
  },
  {
    "en": "Apa Yang Dideteksi Sensor PIR (Passive Infrared)?",
    "id": "Radiasi Panas Inframerah (Gerakan)."
  },
  {
    "en": "Apa Itu Sensor Rotary Encoder?",
    "id": "Sensor Pengukur Posisi Sudut Putaran."
  },
  {
    "en": "Apa Dua Tipe Rotary Encoder?",
    "id": "Inkremental Dan Absolut."
  },
  {
    "en": "Apa Itu Encoder Inkremental?",
    "id": "Menghasilkan Pulsa Relatif Terhadap Gerakan."
  },
  {
    "en": "Apa Itu Encoder Absolut?",
    "id": "Memberikan Posisi Sudut Absolut (Unik)."
  },
  {
    "en": "Apa Itu Load Cell (Sel Beban)?",
    "id": "Transduser Gaya Menjadi Sinyal Listrik."
  },
  {
    "en": "Apa Komponen Pengindra Utama Load Cell?",
    "id": "Strain Gauge."
  },
  {
    "en": "Apa Itu Energi Terbarukan (Renewable Energy)?",
    "id": "Sumber Energi Alam Tak Terbatas."
  },
  {
    "en": "Sebutkan Contoh Energi Terbarukan?",
    "id": "Surya, Angin, Air, Panas Bumi."
  },
  {
    "en": "Apa Itu Smart Grid (Jaringan Cerdas)?",
    "id": "Jaringan Listrik Modern Terkomputerisasi."
  },
  {
    "en": "Apa Itu Mikrogid (Microgrid)?",
    "id": "Jaringan Listrik Lokal Skala Kecil."
  },
  {
    "en": "Apa Itu Pembangkit Listrik Distributed Generation (DG)?",
    "id": "Pembangkit Listrik Tersebar (Kecil)."
  },
  {
    "en": "Apa Itu Net Metering (Pengukuran Bersih)?",
    "id": "Sistem Ekspor-Impor Listrik PLTS."
  },
  {
    "en": "Apa Itu Beban Puncak (Peak Load)?",
    "id": "Permintaan Listrik Tertinggi Sistem."
  },
  {
    "en": "Apa Itu Beban Dasar (Base Load)?",
    "id": "Permintaan Listrik Minimum Konstan."
  },
  {
    "en": "Apa Itu Pembangkit Beban Puncak (Peaker Plant)?",
    "id": "Pembangkit Cadangan Saat Beban Puncak."
  },
  {
    "en": "Apa Itu Pembangkit Beban Dasar (Base Load Plant)?",
    "id": "Pembangkit Operasi Kontinu (PLTU, PLTN)."
  },
  {
    "en": "Apa Itu Faktor Beban (Load Factor)?",
    "id": "Rasio Energi Aktual Terhadap Maksimum."
  },
  {
    "en": "Apa Itu Faktor Ketersediaan (Availability Factor)?",
    "id": "Waktu Kesiapan Pembangkit Beroperasi."
  },
  {
    "en": "Apa Itu Faktor Kapasitas (Capacity Factor)?",
    "id_": "Rasio Output Aktual Terhadap Maksimum."
  },
  {
    "en": "Apa Itu Kapasitansi Parasitik?",
    "id": "Kapasitansi Tak Diinginkan Dalam Rangkaian."
  },
  {
    "en": "Apa Itu Induktansi Parasitik?",
    "id": "Induktansi Tak Diinginkan Dalam Rangkaian."
  },
  {
    "en": "Apa Itu Dioda Bridge (Jembatan Dioda)?",
    "id": "Empat Dioda Penyearah Gelombang Penuh."
  },
  {
    "en": "Apa Itu Kapasitor Bypass (Decoupling)?",
    "id": "Filter Noise Frekuensi Tinggi Di Jalur VCC."
  },
  {
    "en": "Apa Itu Penguat (Amplifier) Kelas A?",
    "id": "Titik Kerja Di Tengah (Bias DC)."
  },
  {
    "en": "Berapa Sudut Konduksi Penguat Kelas A?",
    "id": "Penuh Tiga Ratus Enam Puluh Derajat."
  },
  {
    "en": "Kelemahan Utama Penguat Kelas A?",
    "id": "Efisiensi Sangat Rendah (Boros Panas)."
  },
  {
    "en": "Apa Itu Penguat (Amplifier) Kelas B?",
    "id": "Titik Kerja Di Titik Cut-Off."
  },
  {
    "en": "Berapa Sudut Konduksi Penguat Kelas B?",
    "id": "Seratus Delapan Puluh Derajat."
  },
  {
    "en": "Apa Itu Distorsi Crossover (Crossover Distortion)?",
    "id": "Cacat Sinyal Saat Pergantian Transistor."
  },
  {
    "en": "Penguat Kelas Apa Penyebab Crossover Distortion?",
    "id": "Kelas B."
  },
  {
    "en": "Apa Itu Penguat (Amplifier) Kelas AB?",
    "id": "Gabungan Kelas A Dan B."
  },
  {
    "en": "Tujuan Utama Penguat Kelas AB?",
    "id": "Menghilangkan Distorsi Crossover."
  },
  {
    "en": "Apa Itu Penguat (Amplifier) Kelas C?",
    "id": "Titik Kerja Di Bawah Cut-Off."
  },
  {
    "en": "Aplikasi Utama Penguat Kelas C?",
    "id": "Penguat Frekuensi Radio (RF) Tuned."
  },
  {
    "en": "Apa Itu Penguat (Amplifier) Kelas D?",
    "id": "Penguat Switching (Menggunakan PWM)."
  },
  {
    "en": "Keunggulan Utama Penguat Kelas D?",
    "id": "Efisiensi Sangat Tinggi."
  },
  {
    "en": "Apa Itu Seri Logika 74xx?",
    "id": "Keluarga IC Digital Berbasis TTL."
  },
  {
    "en": "Apa Itu Seri Logika 40xx?",
    "id": "Keluarga IC Digital Berbasis CMOS."
  },
  {
    "en": "Apa Kepanjangan LSTTL (Low-Power Schottky TTL)?",
    "id": "Low-Power Schottky TTL."
  },
  {
    "en": "Apa Kepanjangan HC (High-Speed CMOS)?",
    "id": "High-Speed CMOS."
  },
  {
    "en": "Apa Itu Standar RS232 (Recommended Standard 232)?",
    "id": "Standar Komunikasi Serial (Port COM)."
  },
  {
    "en": "Apa Kepanjangan RS485 (Recommended Standard 485)?",
    "id": "Recommended Standard 485."
  },
  {
    "en": "Apa Keunggulan RS485?",
    "id": "Jarak Jauh Dan Multi-Drop."
  },
  {
    "en": "Apa Itu Sinyal Diferensial (Differential Signaling)?",
    "id": "Sinyal Dikirim Lewat Dua Kabel (A+, B-)."
  },
  {
    "en": "Keuntungan Sinyal Diferensial?",
    "id": "Sangat Kebal Terhadap Noise (EMI)."
  },
  {
    "en": "Apa Kepanjangan CAN (Controller Area Network) Bus?",
    "id": "Controller Area Network."
  },
  {
    "en": "Dimana CAN (Controller Area Network) Bus Umum Digunakan?",
    "id": "Otomotif (Mobil) Dan Industri."
  },
  {
    "en": "Apa Itu Analisis Ruang Keadaan (State-Space)?",
    "id": "Model Sistem Kontrol Berbasis Matriks."
  },
  {
    "en": "Apa Itu Variabel Keadaan (State Variables)?",
    "id": "Set Variabel Minimal Deskripsi Sistem."
  },
  {
    "en": "Apa Itu Keterkendalian (Controllability) Sistem?",
    "id": "Kemampuan Input Mengontrol Keadaan Internal."
  },
  {
    "en": "Apa Itu Keteramatan (Observability) Sistem?",
    "id": "Kemampuan Output Mengukur Keadaan Internal."
  },
  {
    "en": "Apa Itu Jembatan Maxwell (Maxwell Bridge)?",
    "id": "Mengukur Induktansi (Faktor Q Rendah)."
  },
  {
    "en": "Apa Itu Jembatan Hay (Hay Bridge)?",
    "id": "Mengukur Induktansi (Faktor Q Tinggi)."
  },
  {
    "en": "Apa Itu Jembatan Schering (Schering Bridge)?",
    "id": "Mengukur Kapasitansi Dan Faktor Disipasi."
  },
  {
    "en": "Apa Itu Jembatan Kelvin (Kelvin Bridge)?",
    "id": "Mengukur Nilai Resistansi Sangat Rendah."
  },
  {
    "en": "Apa Itu Stabilitas Sudut Rotor?",
    "id": "Kemampuan Generator Tetap Sinkron."
  },
  {
    "en": "Apa Itu Stabilitas Transien (Transient Stability)?",
    "id": "Respon Sistem Terhadap Gangguan Besar."
  },
  {
    "en": "Apa Itu Stabilitas Keadaan Tetap (Steady-State Stability)?",
    "id": "Respon Sistem Terhadap Gangguan Kecil."
  },
  {
    "en": "Apa Itu Inersia Sistem Tenaga?",
    "id": "Energi Kinetik Rotasi (Menahan Perubahan F)."
  },
  {
    "en": "Apa Itu Black Start (Start Hitam)?",
    "id": "Menghidupkan Pembangkit Tanpa Jaringan."
  },
  {
    "en": "Apa Itu Governor (Pengatur Kecepatan) Turbin?",
    "id": "Mengontrol Kecepatan Putaran Generator."
  },
  {
    "en": "Fungsi Governor (Pengatur Kecepatan)?",
    "id": "Mengatur Frekuensi Sistem (Daya Aktif)."
  },
  {
    "en": "Fungsi AVR (Automatic Voltage Regulator)?",
    "id": "Mengatur Tegangan Sistem (Daya Reaktif)."
  },
  {
    "en": "Apa Itu Studi Hubung Singkat?",
    "id": "Menganalisis Arus Gangguan Maksimum."
  },
  {
    "en": "Tujuan Studi Hubung Singkat?",
    "id": "Menentukan Rating Proteksi (MCB, Sekring)."
  },
  {
    "en": "Apa Itu Koordinasi Proteksi?",
    "id": "Pengaturan Urutan Kerja Alat Proteksi."
  },
  {
    "en": "Apa Itu Bahan FR-4 (Flame Retardant 4)?",
    "id": "Material Papan PCB Paling Umum."
  },
  {
    "en": "Bahan FR-4 (Flame Retardant 4) Terbuat Dari Apa?",
    "id": "Fiberglass (Serat Kaca) Dan Epoxy."
  },
  {
    "en": "Apa Itu Mylar?",
    "id": "Nama Dagang Film Poliester (Isolator)."
  },
  {
    "en": "Apa Itu Mika (Mica)?",
    "id": "Bahan Isolator Tahan Panas Tinggi."
  },
  {
    "en": "Aplikasi Mika (Mica)?",
    "id": "Isolasi Transistor Daya, Kapasitor RF."
  },
  {
    "en": "Apa Itu Bakelit (Bakelite)?",
    "id": "Plastik Termoset Kuno (Isolator)."
  },
  {
    "en": "Apa Itu Tembaga Bebas Oksigen (OFC)?",
    "id": "Tembaga Murni Konduktivitas Tinggi."
  },
  {
    "en": "Aplikasi OFC (Oxygen-Free Copper)?",
    "id": "Kabel Audio Kualitas Tinggi."
  },
  {
    "en": "Apa Itu Atenuator (Attenuator)?",
    "id": "Rangkaian Pelemahan Sinyal (Mengurangi Daya)."
  },
  {
    "en": "Apa Itu Mixer (Pencampur) Frekuensi?",
    "id": "Pengali Sinyal (Menghasilkan F1+F2, F1-F2)."
  },
  {
    "en": "Apa Itu Penerima Superheterodyne?",
    "id": "Prinsip Penerima Radio (Menggunakan IF)."
  },
  {
    "en": "Apa Itu Frekuensi IF (Intermediate Frequency)?",
    "id": "Frekuensi Antara Hasil Mixing."
  },
  {
    "en": "Apa Kepanjangan PLL (Phase Locked Loop)?",
    "id": "Phase Locked Loop."
  },
  {
    "en": "Apa Fungsi PLL (Phase Locked Loop)?",
    "id": "Penstabil Frekuensi, Sintesis Frekuensi."
  },
  {
    "en": "Apa Itu Resonator Rongga (Cavity Resonator)?",
    "id": "Resonator Frekuensi Sangat Tinggi (Microwave)."
  },
  {
    "en": "Apa Itu Magnetron?",
    "id": "Tabung Pembangkit Gelombang Mikro."
  },
  {
    "en": "Aplikasi Magnetron?",
    "id": "Oven Microwave Dan Radar."
  },
  {
    "en": "Apa Itu Klystron?",
    "id": "Tabung Penguat Sinyal Microwave."
  },
  {
    "en": "Apa Kepanjangan TWT (Travelling Wave Tube)?",
    "id": "Travelling Wave Tube."
  },
  {
    "en": "Aplikasi TWT (Travelling Wave Tube)?",
    "id": "Penguat Daya Satelit Komunikasi."
  },
  {
    "en": "Apa Kepanjangan BPSK (Binary Phase Shift Keying)?",
    "id": "Binary Phase Shift Keying."
  },
  {
    "en": "Berapa Fasa Sinyal BPSK?",
    "id": "Dua Fasa (0 Dan 180 Derajat)."
  },
  {
    "en": "Apa Kepanjangan QPSK (Quadrature Phase Shift Keying)?",
    "id": "Quadrature Phase Shift Keying."
  },
  {
    "en": "Berapa Fasa Sinyal QPSK?",
    "id": "Empat Fasa (Selisih 90 Derajat)."
  },
  {
    "en": "Apa Itu Diagram Konstelasi (Constellation Diagram)?",
    "id": "Grafik Titik Simbol Modulasi Digital."
  },
  {
    "en": "Apa Kepanjangan BER (Bit Error Rate)?",
    "id": "Bit Error Rate."
  },
  {
    "en": "Apa Arti BER (Bit Error Rate) Rendah?",
    "id": "Kualitas Sinyal Transmisi Baik."
  },
  {
    "en": "Apa Itu Kode Saluran (Line Coding)?",
    "id": "Metode Pengkodean Bit Digital."
  },
  {
    "en": "Apa Itu Kode Manchester?",
    "id": "Transisi Sinyal Di Tengah Bit."
  },
  {
    "en": "Keuntungan Kode Manchester?",
    "id": "Sinkronisasi Mandiri (Self-Sync)."
  },
  {
    "en": "Apa Itu Filter SAW (Surface Acoustic Wave)?",
    "id": "Filter RF Presisi (Piezoelektrik)."
  },
  {
    "en": "Apa Itu Dioda SiC (Silicon Carbide)?",
    "id": "Dioda Daya (Tegangan Tinggi, Cepat)."
  },
  {
    "en": "Apa Itu Transistor GaN (Gallium Nitride)?",
    "id": "Transistor (Switching Cepat, Efisien)."
  },
  {
    "en": "Keunggulan GaN (Gallium Nitride) Dibanding Silikon?",
    "id": "Lebih Cepat, Efisien, Ukuran Kecil."
  },
  {
    "en": "Apa Itu Cermin Arus (Current Mirror)?",
    "id": "Rangkaian Menyalin (Menduplikasi) Arus."
  },
  {
    "en": "Apa Itu Pasangan Diferensial (Differential Pair)?",
    "id": "Inti Input Penguat Operasional (Op-Amp)."
  },
  {
    "en": "Apa Kepanjangan CMRR (Common Mode Rejection Ratio)?",
    "id": "Common Mode Rejection Ratio."
  },
  {
    "en": "Apa Arti CMRR (Common Mode Rejection Ratio) Tinggi?",
    "id": "Kemampuan Baik Menolak Noise."
  },
  {
    "en": "Apa Itu Resistor Pull-Up?",
    "id": "Menghubungkan Jalur Ke Tegangan VCC."
  },
  {
    "en": "Apa Itu Resistor Pull-Down?",
    "id": "Menghubungkan Jalur Ke Ground (GND)."
  },
  {
    "en": "Fungsi Resistor Pull-Up/Pull-Down?",
    "id": "Memastikan Level Logika Input Jelas."
  },
  {
    "en": "Apa Itu Input Mengambang (Floating Input)?",
    "id": "Input Tidak Terhubung (Status Tidak Jelas)."
  },
  {
    "en": "Apa Itu Tang Potong (Diagonal Cutter)?",
    "id": "Alat Pemotong Kawat Elektronika."
  },
  {
    "en": "Apa Itu Tang Pengupas Kabel (Wire Stripper)?",
    "id": "Alat Mengupas Isolasi Kabel."
  },
  {
    "en": "Apa Itu Obeng Presisi (Precision Screwdriver)?",
    "id": "Obeng Set Kecil."
  },
  {
    "en": "Apa Itu Gelang Antistatis (Anti-Static Wrist Strap)?",
    "id": "Mencegah Kerusakan ESD."
  },
  {
    "en": "Apa Itu Matras Antistatis (Anti-Static Mat)?",
    "id": "Alas Kerja Aman ESD."
  },
  {
    "en": "Apa Itu Sensor pH?",
    "id": "Mengukur Tingkat Keasaman (Asam/Basa)."
  },
  {
    "en": "Apa Itu Sensor Turbiditas (Turbidity Sensor)?",
    "id": "Mengukur Kekeruhan Air."
  },
  {
    "en": "Apa Itu Sensor Giroskop (Gyroscope)?",
    "id": "Mengukur Kecepatan Sudut (Rotasi)."
  },
  {
    "en": "Apa Itu Akselerometer (Accelerometer)?",
    "id": "Mengukur Percepatan Linear (Gravitasi)."
  },
  {
    "en": "Apa Kepanjangan IMU (Inertial Measurement Unit)?",
    "id": "Inertial Measurement Unit."
  },
  {
    "en": "Apa Isi Dari IMU (Inertial Measurement Unit)?",
    "id": "Giroskop Dan Akselerometer."
  },
  {
    "en": "Apa Kepanjangan RoHS (Restriction of Hazardous Substances)?",
    "id": "Direktif Pembatasan Bahan Berbahaya."
  },
  {
    "en": "Apa Itu Tanda CE (Conformité Européenne)?",
    "id": "Tanda Kesesuaian Produk Eropa."
  },
  {
    "en": "Apa Itu Sertifikasi UL (Underwriters Laboratories)?",
    "id": "Sertifikasi Keamanan Produk (AS)."
  },
  {
    "en": "Apa Kepanjangan NEC (National Electrical Code)?",
    "id": "Standar Instalasi Listrik (AS)."
  },
  {
    "en": "Apa Itu Sistem Per-Unit (PU)?",
    "id": "Metode Normalisasi Analisis Sistem Tenaga."
  },
  {
    "en": "Apa Tujuan Sistem Per-Unit (PU)?",
    "id": "Menyederhanakan Perhitungan Transformator."
  },
  {
    "en": "Apa Itu Nilai Dasar (Base Value) PU?",
    "id": "Nilai Referensi (Daya, Tegangan)."
  },
  {
    "en": "Apa Itu Analisis Gangguan (Fault Analysis)?",
    "id": "Studi Aliran Arus Hubung Singkat."
  },
  {
    "en": "Apa Itu Gangguan Simetris (Symmetrical Fault)?",
    "id": "Gangguan Tiga Fasa Seimbang."
  },
  {
    "en": "Apa Itu Gangguan Asimetris (Unsymmetrical Fault)?",
    "id": "Gangguan Tak Seimbang (Satu Fasa)."
  },
  {
    "en": "Apa Itu Gangguan Fasa-Tanah (Line-to-Ground/LG)?",
    "id": "Hubung Singkat Satu Fasa Ke Tanah."
  },
  {
    "en": "Apa Itu Gangguan Fasa-Fasa (Line-to-Line/LL)?",
    "id": "Hubung Singkat Antar Dua Fasa."
  },
  {
    "en": "Apa Itu Gangguan Dua Fasa-Tanah (LLG)?",
    "id": "Hubung Singkat Dua Fasa Ke Tanah."
  },
  {
    "en": "Apa Itu Komponen Simetris (Symmetrical Components)?",
    "id": "Metode Analisis Gangguan Asimetris."
  },
  {
    "en": "Apa Itu Komponen Urutan Positif?",
    "id": "Sistem Seimbang Urutan Normal."
  },
  {
    "en": "Apa Itu Komponen Urutan Negatif?",
    "id": "Sistem Seimbang Urutan Terbalik."
  },
  {
    "en": "Apa Itu Komponen Urutan Nol?",
    "id": "Tiga Vektor Sefasa."
  },
  {
    "en": "Penyebab Arus Urutan Negatif?",
    "id": "Beban Tak Seimbang, Gangguan LL."
  },
  {
    "en": "Penyebab Arus Urutan Nol?",
    "id": "Hanya Saat Gangguan Ke Tanah (LG)."
  },
  {
    "en": "Apa Itu Matriks Impedansi Bus (Zbus)?",
    "id": "Matriks Impedansi Jaringan Sistem Tenaga."
  },
  {
    "en": "Apa Itu Setup Time (Waktu Pengaturan)?",
    "id": "Data Stabil Sebelum Clock Tiba."
  },
  {
    "en": "Apa Itu Hold Time (Waktu Penahanan)?",
    "id": "Data Stabil Setelah Clock Tiba."
  },
  {
    "en": "Apa Akibat Pelanggaran Setup/Hold Time?",
    "id": "Metastabilitas (Output Tidak Jelas)."
  },
  {
    "en": "Apa Itu Metastabilitas (Metastability)?",
    "id": "Kondisi Output Ragu-Ragu (Bukan 0/1)."
  },
  {
    "en": "Apa Itu Clock Skew (Kemiringan Clock)?",
    "id": "Perbedaan Waktu Tiba Sinyal Clock."
  },
  {
    "en": "Apa Itu Clock Jitter (Getaran Clock)?",
    "id": "Variasi Periode Sinyal Clock."
  },
  {
    "en": "Apa Itu Generator Paritas (Parity Generator)?",
    "id": "Rangkaian Pembuat Bit Paritas."
  },
  {
    "en": "Apa Itu Pemeriksa Paritas (Parity Checker)?",
    "id": "Rangkaian Pengecek Kesalahan Paritas."
  },
  {
    "en": "Apa Itu Penjumlah Look-Ahead Carry?",
    "id": "Penjumlah Biner Cepat (Paralel)."
  },
  {
    "en": "Apa Itu Penjumlah Ripple Carry?",
    "id": "Penjumlah Biner (Carry Merambat)."
  },
  {
    "en": "Apa Itu Koefisien Suhu Hambatan?",
    "id": "Perubahan Resistansi Terhadap Suhu."
  },
  {
    "en": "Apa Itu Kekuatan Dielektrik (Dielectric Strength)?",
    "id": "Tegangan Tembus Maksimum Isolator."
  },
  {
    "en": "Apa Itu Generator Fungsi (Function Generator)?",
    "id": "Alat Pembangkit Sinyal (Sinus, Kotak)."
  },
  {
    "en": "Apa Kepanjangan AWG (Arbitrary Waveform Generator)?",
    "id": "Arbitrary Waveform Generator."
  },
  {
    "en": "Apa Itu Probe Logika (Logic Probe)?",
    "id": "Alat Uji Cepat Sinyal Digital."
  },
  {
    "en": "Apa Indikasi Probe Logika?",
    "id": "Tinggi, Rendah, Pulsa, Mengambang."
  },
  {
    "en": "Apa Itu Probe Arus (Current Probe)?",
    "id": "Probe Osiloskop Pengukur Arus."
  },
  {
    "en": "Apa Itu Probe Diferensial (Differential Probe)?",
    "id": "Probe Pengukur Beda Tegangan."
  },
  {
    "en": "Apa Itu Probe Tegangan Tinggi (HV Probe)?",
    "id": "Probe Pengukur Tegangan Ribuan Volt."
  },
  {
    "en": "Apa Hukum Gauss Listrik?",
    "id": "Fluks Listrik Sebanding Muatan Dilingkupi."
  },
  {
    "en": "Apa Hukum Gauss Magnetisme?",
    "id": "Total Fluks Magnetik Tertutup Nol."
  },
  {
    "en": "Apa Hukum Induksi Faraday?",
    "id": "GGL Induksi Akibat Perubahan Fluks Magnetik."
  },
  {
    "en": "Apa Hukum Ampere-Maxwell?",
    "id": "Medan Magnet Dihasilkan Arus Listrik."
  },
  {
    "en": "Apa Itu Persamaan Maxwell?",
    "id": "Empat Hukum Dasar Elektromagnetisme."
  },
  {
    "en": "Apa Itu Vektor Poynting?",
    "id": "Arah Dan Laju Energi Gelombang EM."
  },
  {
    "en": "Apa Itu Dwikutub Listrik (Electric Dipole)?",
    "id": "Sepasang Muatan Sama Berlawanan."
  },
  {
    "en": "Apa Itu Spektrum RF (Radio Frequency)?",
    "id": "Rentang Frekuensi Gelombang Radio."
  },
  {
    "en": "Apa Pita LF (Low Frequency)?",
    "id": "Tiga Puluh KHz - 300 KHz."
  },
  {
    "en": "Apa Pita MF (Medium Frequency)?",
    "id": "Tiga Ratus KHz - 3 MHz."
  },
  {
    "en": "Apa Pita HF (High Frequency)?",
    "id": "Tiga MHz - 30 MHz."
  },
  {
    "en": "Apa Pita VHF (Very High Frequency)?",
    "id": "Tiga Puluh MHz - 300 MHz."
  },
  {
    "en": "Apa Pita UHF (Ultra High Frequency)?",
    "id": "Tiga Ratus MHz - 3 GHz."
  },
  {
    "en": "Apa Pita SHF (Super High Frequency)?",
    "id": "Tiga GHz - 30 GHz."
  },
  {
    "en": "Apa Pita EHF (Extremely High Frequency)?",
    "id": "Tiga Puluh GHz - 300 GHz."
  },
  {
    "en": "Apa Itu Antena Isotropik?",
    "id": "Antena Ideal, Memancar Segala Arah."
  },
  {
    "en": "Apa Itu Beamwidth (Lebar Pancaran) Antena?",
    "id": "Sudut Konsentrasi Pancaran Utama."
  },
  {
    "en": "Apa Itu Rasio Depan-Belakang Antena?",
    "id": "Rasio Gain Arah Depan Vs Belakang."
  },
  {
    "en": "Apa Itu Lobi Samping (Side Lobes) Antena?",
    "id": "Pancaran Samping Yang Tidak Diinginkan."
  },
  {
    "en": "Apa Itu Antena Yagi-Uda?",
    "id": "Antena TV Terarah (Direksional)."
  },
  {
    "en": "Apa Itu Antena Parabola (Parabolic Antenna)?",
    "id": "Antena Reflektor (Satelit, Radar)."
  },
  {
    "en": "Apa Itu Antena Tambalan (Patch Antenna)?",
    "id": "Antena Datar Pada Papan PCB."
  },
  {
    "en": "Apa Itu Perangkap Gelombang (Wave Trap)?",
    "id": "Filter Penolak Frekuensi Tertentu."
  },
  {
    "en": "Apa Itu Duplekser (Duplexer)?",
    "id": "Mengirim Menerima Satu Antena Bersamaan."
  },
  {
    "en": "Apa Itu Diplexer?",
    "id": "Memisah/Menggabung Sinyal Beda Frekuensi."
  },
  {
    "en": "Apa Itu Dualitas Jaringan (Network Duality)?",
    "id": "Keterkaitan Konsep Rangkaian (Seri/Paralel)."
  },
  {
    "en": "Apa Dual Dari Sumber Tegangan?",
    "id": "Sumber Arus."
  },
  {
    "en": "Apa Dual Dari Induktor?",
    "id": "Kapasitor."
  },
  {
    "en": "Apa Dual Dari Rangkaian Seri?",
    "id": "Rangkaian Paralel."
  },
  {
    "en": "Apa Dual Dari KCL (Kirchhoff's Current Law)?",
    "id": "KVL (Kirchhoff's Voltage Law)."
  },
  {
    "en": "Apa Itu Topologi Jaringan (Network Topology)?",
    "id": "Struktur Hubungan Komponen Rangkaian."
  },
  {
    "en": "Apa Itu Cabang (Branch) Rangkaian?",
    "id": "Elemen Tunggal (Misal Resistor)."
  },
  {
    "en": "Apa Itu Simpul (Node) Rangkaian?",
    "id": "Titik Pertemuan Antar Cabang."
  },
  {
    "en": "Apa Itu Graf (Graph) Rangkaian?",
    "id": "Representasi Grafis Topologi."
  },
  {
    "en": "Apa Itu Pohon (Tree) Rangkaian?",
    "id": "Sub-Graf Terhubung Tanpa Loop."
  },
  {
    "en": "Apa Itu Jaringan Dua Pintu (Two-Port)?",
    "id": "Rangkaian Listrik Punya Input Output."
  },
  {
    "en": "Apa Itu Parameter-Z (Impedansi)?",
    "id": "Parameter Jaringan (V Fungsi I)."
  },
  {
    "en": "Apa Itu Parameter-Y (Admitansi)?",
    "id": "Parameter Jaringan (I Fungsi V)."
  },
  {
    "en": "Apa Itu Parameter-H (Hybrid)?",
    "id": "Parameter Jaringan (Model Transistor)."
  },
  {
    "en": "Apa Itu Parameter-T (Transmisi)?",
    "id": "Parameter Rangkaian Kaskade (Cascade)."
  },
  {
    "en": "Apa Itu Transformasi Delta-Wye (Δ-Y)?",
    "id": "Konversi Jaringan Segitiga Ke Bintang."
  },
  {
    "en": "Apa Itu Transformasi Wye-Delta (Y-Δ)?",
    "id": "Konversi Jaringan Bintang Ke Segitiga."
  },
  {
    "en": "Apa Itu Supernode (Simpul Super)?",
    "id": "Analisis Simpul Gabungan Sumber Tegangan."
  },
  {
    "en": "Apa Itu Supermesh (Simpal Super)?",
    "id": "Analisis Mesh Gabungan Sumber Arus."
  },
  {
    "en": "Apa Itu Kondisi Awal (Initial Condition)?",
    "id": "Nilai Arus/Tegangan Pada t=0."
  },
  {
    "en": "Sifat Induktor Pada t=0+ (Transient)?",
    "id": "Rangkaian Terbuka (Open Circuit)."
  },
  {
    "en": "Sifat Kapasitor Pada t=0+ (Transient)?",
    "id": "Hubung Singkat (Short Circuit)."
  },
  {
    "en": "Sifat Induktor Pada DC Steady State?",
    "id": "Hubung Singkat (Short Circuit)."
  },
  {
    "en": "Sifat Kapasitor Pada DC Steady State?",
    "id": "Rangkaian Terbuka (Open Circuit)."
  },
  {
    "en": "Apa Kepanjangan LVDT (Linear Variable Differential Transformer)?",
    "id": "Linear Variable Differential Transformer."
  },
  {
    "en": "Apa Fungsi LVDT (Linear Variable Differential Transformer)?",
    "id": "Sensor Pengukur Posisi Linear."
  },
  {
    "en": "Apa Kepanjangan RTD (Resistance Temperature Detector)?",
    "id": "Resistance Temperature Detector."
  },
  {
    "en": "Bahan Utama Sensor RTD (Resistance Temperature Detector)?",
    "id": "Platinum (Platina)."
  },
  {
    "en": "Apa Itu Thyratron?",
    "id": "Tabung Gas Saklar Daya Tinggi (Kuno)."
  },
  {
    "en": "Apa Itu Ignitron?",
    "id": "Penyearah Merkuri Arus Sangat Besar."
  },
  {
    "en": "Apa Itu Tabung Hampa (Vacuum Tube)?",
    "id": "Komponen Aktif Elektronik Generasi Awal."
  },
  {
    "en": "Apa Kepanjangan CRT (Cathode Ray Tube)?",
    "id": "Cathode Ray Tube (Tabung Gambar)."
  },
  {
    "en": "Apa Kepanjangan FPAA (Field-Programmable Analog Array)?",
    "id": "Field-Programmable Analog Array."
  },
  {
    "en": "Apa Itu Wafer Silikon?",
    "id": "Lempengan Bahan Dasar Semikonduktor."
  },
  {
    "en": "Apa Itu Proses Litografi (Lithography)?",
    "id": "Proses Pencetakan Pola Sirkuit."
  },
  {
    "en": "Apa Itu Proses Etching (Pengetsaan)?",
    "id": "Penghilangan Selektif Lapisan Material."
  },
  {
    "en": "Apa Itu Deposisi (Deposition)?",
    "id": "Penambahan Lapisan Tipis Material."
  },
  {
    "en": "Apa Itu Implantasi Ion (Ion Implantation)?",
    "id": "Proses Doping Menggunakan Ion."
  },
  {
    "en": "Apa Itu Ruang Bersih (Cleanroom)?",
    "id": "Ruang Produksi IC Bebas Debu."
  },
  {
    "en": "Apa Itu Die (Semikonduktor)?",
    "id": "Satu Potongan Chip Dari Wafer."
  },
  {
    "en": "Apa Itu Pengemasan IC (IC Packaging)?",
    "id": "Proses Melindungi Die Chip."
  },
  {
    "en": "Apa Kepanjangan DIP (Dual In-line Package)?",
    "id": "Dual In-line Package."
  },
  {
    "en": "Apa Kepanjangan SOIC (Small Outline Integrated Circuit)?",
    "id": "Small Outline Integrated Circuit."
  },
  {
    "en": "Apa Kepanjangan QFP (Quad Flat Package)?",
    "id": "Quad Flat Package."
  },
  {
    "en": "Apa Kepanjangan BGA (Ball Grid Array)?",
    "id": "Ball Grid Array."
  },
  {
    "en": "Apa Itu Wire Bonding (Ikatan Kawat)?",
    "id": "Menyambung Die Ke Pin Kemasan."
  },
  {
    "en": "Apa Itu Rangkaian Snubber?",
    "id": "Rangkaian Peredam Lonjakan Tegangan."
  },
  {
    "en": "Apa Itu Konverter Cuk?",
    "id": "Konverter DC-DC Tipe Buck-Boost."
  },
  {
    "en": "Apa Itu Konverter SEPIC (Single-Ended Primary-Inductor Converter)?",
    "id": "Konverter DC-DC (Tegangan Output Fleksibel)."
  },
  {
    "en": "Apa Itu Konverter Resonansi?",
    "id": "Konverter Menggunakan Prinsip Resonansi LC."
  },
  {
    "en": "Apa Itu Inverter Multi-Level?",
    "id": "Inverter Output Tegangan Bertingkat."
  },
  {
    "en": "Keuntungan Inverter Multi-Level?",
    "id": "Mengurangi Distorsi Harmonisa (THD)."
  },
  {
    "en": "Apa Itu Siklokonverter (Cycloconverter)?",
    "id": "Konverter AC Ke AC (Frekuensi Beda)."
  },
  {
    "en": "Apa Itu Filter Aktif (Active Filter)?",
    "id": "Filter Menggunakan Komponen Aktif (Op-Amp)."
  },
  {
    "en": "Apa Itu Filter Pasif (Passive Filter)?",
    "id": "Filter Hanya Komponen Pasif (RLC)."
  },
  {
    "en": "Keuntungan Filter Aktif?",
    "id": "Bisa Memberikan Penguatan (Gain)."
  },
  {
    "en": "Kelemahan Filter Aktif?",
    "id": "Memerlukan Catu Daya Eksternal."
  },
  {
    "en": "Apa Itu Filter Butterworth?",
    "id": "Filter Respon Datar Maksimal (Maximally Flat)."
  },
  {
    "en": "Apa Itu Filter Chebyshev?",
    "id": "Filter Respon Curam (Ada Riak)."
  },
  {
    "en": "Apa Itu Filter Bessel?",
    "id": "Filter Tundaan Grup Konstan (Fasa Linear)."
  },
  {
    "en": "Apa Itu Tundaan Grup (Group Delay)?",
    "id": "Ukuran Distorsi Fasa Sinyal."
  },
  {
    "en": "Apa Itu Transformasi-Z (Z-Transform)?",
    "id": "Analisis Sistem Waktu Diskret (Digital)."
  },
  {
    "en": "Transformasi-Z (Z-Transform) Adalah Padanan Dari Transformasi Apa?",
    "id": "Transformasi Laplace (Domain-S)."
  },
  {
    "en": "Apa Itu Diagram Keadaan (State Diagram)?",
    "id": "Representasi Grafis Mesin Keadaan (FSM)."
  },
  {
    "en": "Apa Itu Tabel Keadaan (State Table)?",
    "id": "Representasi Tabel Mesin Keadaan (FSM)."
  },
  {
    "en": "Apa Itu Observer (Pengamat) Sistem Kontrol?",
    "id": "Estimator Variabel Keadaan Internal."
  },
  {
    "en": "Apa Itu Histeresis (Hysteresis) Sensor?",
    "id": "Perbedaan Output Saat Naik Dan Turun."
  },
  {
    "en": "Apa Itu Linearitas (Linearity) Sensor?",
    "id": "Kedekatan Respon Sensor Garis Lurus."
  },
  {
    "en": "Apa Itu Waktu Respon (Response Time) Sensor?",
    "id": "Waktu Sensor Merespon Perubahan."
  },
  {
    "en": "Apa Itu Rentang Dinamis (Dynamic Range)?",
    "id": "Rasio Sinyal Terbesar Terkecil."
  },
  {
    "en": "Apa Itu Q Meter (Quality Factor Meter)?",
    "id": "Alat Ukur Faktor Kualitas (Q) Rangkaian."
  },
  {
    "en": "Apa Itu Distortion Meter (Pengukur Distorsi)?",
    "id": "Mengukur Distorsi Harmonisa Sinyal."
  },
  {
    "en": "Apa Itu Kode Hamming (Hamming Code)?",
    "id": "Kode Deteksi Dan Koreksi Error."
  },
  {
    "en": "Apa Itu Jarak Hamming (Hamming Distance)?",
    "id": "Jumlah Posisi Bit Yang Berbeda."
  },
  {
    "en": "Apa Kepanjangan CRC (Cyclic Redundancy Check)?",
    "id": "Cyclic Redundancy Check (Deteksi Error)."
  },
  {
    "en": "Apa Itu Checksum (Hitungan Cek)?",
    "id": "Metode Sederhana Deteksi Error."
  },
  {
    "en": "Apa Itu Parameter-S (Scattering Parameters)?",
    "id": "Parameter Jaringan Frekuensi Tinggi (RF)."
  },
  {
    "en": "Apa Arti S11 (Parameter-S)?",
    "id": "Koefisien Pantulan Input (Return Loss)."
  },
  {
    "en": "Apa Arti S21 (Parameter-S)?",
    "id": "Penguatan (Gain) Transmisi Maju."
  },
  {
    "en": "Apa Arti S12 (Parameter-S)?",
    "id": "Isolasi (Isolation) Transmisi Mundur."
  },
  {
    "en": "Apa Arti S22 (Parameter-S)?",
    "id": "Koefisien Pantulan Output (Return Loss)."
  },
  {
    "en": "Apa Itu Noise Figure (NF)?",
    "id": "Ukuran Degradasi SNR (Signal-to-Noise Ratio) Perangkat."
  },
  {
    "en": "Apa Arti Noise Figure (NF) Rendah?",
    "id": "Kualitas Baik, Noise Tambahan Kecil."
  },
  {
    "en": "Apa Kepanjangan LNA (Low Noise Amplifier)?",
    "id": "Low Noise Amplifier."
  },
  {
    "en": "Dimana Posisi LNA (Low Noise Amplifier) Dipasang?",
    "id": "Tahap Paling Awal Penerima (Dekat Antena)."
  },
  {
    "en": "Apa Itu Intermodulasi (Intermodulation Distortion/IMD)?",
    "id": "Produk Sinyal Palsu Akibat Non-Linearitas."
  },
  {
    "en": "Apa Itu Titik Kompresi 1dB (P1dB)?",
    "id": "Titik Awal Saturasi Penguat."
  },
  {
    "en": "Apa Itu Titik IP3 (Third-Order Intercept Point)?",
    "id": "Ukuran Linearitas Perangkat RF."
  },
  {
    "en": "Apa Kepanjangan PSS (Power System Stabilizer)?",
    "id": "Power System Stabilizer."
  },
  {
    "en": "Apa Fungsi PSS (Power System Stabilizer)?",
    "id": "Meredam Osilasi Daya Sistem Tenaga."
  },
  {
    "en": "Apa Kepanjangan FACTS (Flexible AC Transmission Systems)?",
    "id": "Flexible AC Transmission Systems."
  },
  {
    "en": "Apa Tujuan Utama FACTS (Flexible AC Transmission Systems)?",
    "id": "Mengoptimalkan Aliran Daya Transmisi."
  },
  {
    "en": "Apa Kepanjangan SVC (Static VAR Compensator)?",
    "id": "Static VAR Compensator."
  },
  {
    "en": "Apa Fungsi SVC (Static VAR Compensator)?",
    "id": "Kontrol Cepat Daya Reaktif Jaringan."
  },
  {
    "en": "Apa Kepanjangan TCSC (Thyristor Controlled Series Capacitor)?",
    "id": "Thyristor Controlled Series Capacitor."
  },
  {
    "en": "Apa Fungsi TCSC (Thyristor Controlled Series Capacitor)?",
    "id": "Mengatur Impedansi Kompensasi Seri."
  },
  {
    "en": "Apa Itu Kompensasi Seri (Saluran Transmisi)?",
    "id": "Memasang Kapasitor Seri Saluran."
  },
  {
    "en": "Apa Itu Kompensasi Shunt (Saluran Transmisi)?",
    "id": "Memasang Reaktor/Kapasitor Paralel."
  },
  {
    "en": "Apa Itu Ayunan Daya (Power Swing)?",
    "id": "Osilasi Daya Antar Generator."
  },
  {
    "en": "Apa Itu Konduktor Berkas (Bundled Conductor)?",
    "id": "Dua/Lebih Konduktor Per Fasa SUTET."
  },
  {
    "en": "Tujuan Konduktor Berkas (Bundled Conductor)?",
    "id": "Mengurangi Efek Corona Dan Induktansi."
  },
  {
    "en": "Apa Itu Transposisi Saluran Transmisi?",
    "id": "Pertukaran Posisi Fisik Kabel Fasa."
  },
  {
    "en": "Tujuan Transposisi Saluran Transmisi?",
    "id": "Menyeimbangkan Impedansi Tiga Fasa."
  },
  {
    "en": "Apa Itu Arus Magnetisasi (Magnetizing Current)?",
    "id": "Arus Pembentuk Fluks Inti Trafo."
  },
  {
    "en": "Apa Itu Efisiensi Trafo Sepanjang Hari (All-Day)?",
    "id": "Efisiensi Trafo Distribusi (Siklus 24 Jam)."
  },
  {
    "en": "Apa Itu Beban Seimbang (Balanced Load)?",
    "id": "Impedansi Ketiga Fasa Sama Persis."
  },
  {
    "en": "Apa Itu Beban Tak Seimbang (Unbalanced Load)?",
    "id": "Impedansi Ketiga Fasa Tidak Sama."
  },
  {
    "en": "Dampak Beban Tak Seimbang?",
    "id": "Muncul Arus Di Kawat Netral."
  },
  {
    "en": "Apa Itu Motor Histeresis (Hysteresis Motor)?",
    "id": "Motor Sinkron Kecil (Start Sendiri)."
  },
  {
    "en": "Aplikasi Motor Histeresis?",
    "id": "Jam Listrik, Timer Mekanis Presisi."
  },
  {
    "en": "Apa Itu Motor Reluktansi (Reluctance Motor)?",
    "id": "Motor Sinkron (Rotor Tanpa Lilitan)."
  },
  {
    "en": "Apa Itu Pengontrol Tegangan AC (AC Voltage Controller)?",
    "id": "Mengatur Tegangan RMS AC (Pakai TRIAC)."
  },
  {
    "en": "Apa Itu Efek Miller (Miller Effect)?",
    "id": "Peningkatan Kapasitansi Input Penguat."
  },
  {
    "en": "Apa Itu Rangkaian Darlington Pair?",
    "id": "Dua Transistor (BJT) Gabungan Gain Tinggi."
  },
  {
    "en": "Apa Simbol Darlington Pair?",
    "id": "Transistor Dengan Beta Sangat Tinggi."
  },
  {
    "en": "Apa Itu Rangkaian Sziklai Pair?",
    "id": "Pasangan Komplementer (NPN Dan PNP)."
  },
  {
    "en": "Apa Itu Rangkaian Cascode?",
    "id": "Penguat Dua Tahap (CE Diikuti CB)."
  },
  {
    "en": "Keuntungan Penguat Cascode?",
    "id": "Bandwidth Lebar, Isolasi Balik Tinggi."
  },
  {
    "en": "Apa Itu Penguat Common Emitter (CE)?",
    "id": "Penguat BJT (Gain Tegangan Tinggi)."
  },
  {
    "en": "Apa Itu Penguat Common Collector (CC)?",
    "id": "Penguat BJT (Pengikut Emitor)."
  },
  {
    "en": "Apa Fungsi Penguat Common Collector (CC)?",
    "id": "Penyangga Impedansi (Gain Arus)."
  },
  {
    "en": "Apa Itu Penguat Common Base (CB)?",
    "id": "Penguat BJT (Non-Inverting)."
  },
  {
    "en": "Apa Itu Penguat Common Source (CS) FET?",
    "id": "Penguat FET (Serupa Common Emitter)."
  },
  {
    "en": "Apa Itu Penguat Common Drain (CD) FET?",
    "id": "Penguat FET (Pengikut Source)."
  },
  {
    "en": "Apa Itu Penguat Common Gate (CG) FET?",
    "id": "Penguat FET (Impedansi Input Rendah)."
  },
  {
    "en": "Apa Itu Garis Beban (Load Line) DC?",
    "id": "Grafik Titik Operasi Transistor."
  },
  {
    "en": "Apa Itu Titik-Q (Quiescent Point)?",
    "id": "Titik Operasi DC Transistor."
  },
  {
    "en": "Apa Itu Pembiasan (Biasing) Transistor?",
    "id": "Pengaturan Titik-Q (Titik Kerja DC)."
  },
  {
    "en": "Apa Itu Rangkaian Pembagi Tegangan (Voltage Divider) Bias?",
    "id": "Metode Pembiasan Transistor Paling Stabil."
  },
  {
    "en": "Apa Itu Pelarian Termal (Thermal Runaway)?",
    "id": "Kondisi Transistor Panas Rusak."
  },
  {
    "en": "Apa Itu Dioda Varactor?",
    "id": "Dioda Kapasitansi Variabel (Varicap)."
  },
  {
    "en": "Apa Itu Dioda Step Recovery (SRD)?",
    "id": "Dioda Pembangkit Harmonisa Cepat."
  },
  {
    "en": "Apa Kepanjangan DSP (Digital Signal Processing)?",
    "id": "Digital Signal Processing."
  },
  {
    "en": "Apa Itu Sinyal Waktu Kontinu?",
    "id": "Sinyal Didefinisikan Setiap Saat."
  },
  {
    "en": "Apa Itu Sinyal Waktu Diskret?",
    "id": "Sinyal Didefinisikan Interval Tertentu."
  },
  {
    "en": "Apa Kepanjangan DFT (Discrete Fourier Transform)?",
    "id": "Discrete Fourier Transform."
  },
  {
    "en": "Apa Fungsi DFT (Discrete Fourier Transform)?",
    "id": "Analisis Frekuensi Sinyal Diskret."
  },
  {
    "en": "Apa Kepanjangan FFT (Fast Fourier Transform)?",
    "id": "Fast Fourier Transform."
  },
  {
    "en": "Keuntungan FFT (Fast Fourier Transform)?",
    "id": "Algoritma Perhitungan DFT Cepat."
  },
  {
    "en": "Apa Kepanjangan FIR (Finite Impulse Response) Filter?",
    "id": "Finite Impulse Response."
  },
  {
    "en": "Karakteristik Filter FIR (Finite Impulse Response)?",
    "id": "Selalu Stabil, Fasa Linear."
  },
  {
    "en": "Apa Kepanjangan IIR (Infinite Impulse Response) Filter?",
    "id": "Infinite Impulse Response."
  },
  {
    "en": "Karakteristik Filter IIR (Infinite Impulse Response)?",
    "id": "Efisien, Fasa Non-Linear."
  },
  {
    "en": "Apa Itu Jendela (Windowing) Dalam DSP?",
    "id": "Mengurangi Kebocoran Spektral."
  },
  {
    "en": "Apa Itu Downsampling (Decimation)?",
    "id": "Mengurangi Laju Sampling Sinyal."
  },
  {
    "en": "Apa Itu Upsampling (Interpolation)?",
    "id": "Meningkatkan Laju Sampling Sinyal."
  },
  {
    "en": "Apa Kepanjangan EMC (Electromagnetic Compatibility)?",
    "id": "Electromagnetic Compatibility."
  },
  {
    "en": "Apa Dua Aspek Utama EMC?",
    "id": "Emisi Dan Kekebalan (Susceptibility)."
  },
  {
    "en": "Apa Itu Emisi Elektromagnetik (EMI)?",
    "id": "Pancaran Noise Yang Tidak Diinginkan."
  },
  {
    "en": "Apa Itu Kekebalan (Susceptibility) Elektromagnetik (EMS)?",
    "id": "Ketahanan Perangkat Terhadap Noise."
  },
  {
    "en": "Apa Itu Emisi Terkonduksi (Conducted Emission)?",
    "id": "Noise Merambat Lewat Kabel."
  },
  {
    "en": "Apa Itu Emisi Teradiasi (Radiated Emission)?",
    "id": "Noise Merambat Lewat Udara."
  },
  {
    "en": "Apa Itu Kopling (Coupling) Kapasitif?",
    "id": "Transfer Noise Melalui Medan Listrik."
  },
  {
    "en": "Apa Itu Kopling (Coupling) Induktif?",
    "id": "Transfer Noise Melalui Medan Magnet."
  },
  {
    "en": "Apa Itu Sangkar Faraday (Faraday Cage)?",
    "id": "Pelindung Mencegah Radiasi EMI."
  },
  {
    "en": "Apa Kepanjangan ECG (Electrocardiogram)?",
    "id": "Rekaman Sinyal Listrik Jantung."
  },
  {
    "en": "Apa Kepanjangan EEG (Electroencephalogram)?",
    "id": "Rekaman Sinyal Listrik Otak."
  },
  {
    "en": "Apa Kepanjangan EMG (Electromyogram)?",
    "id": "Rekaman Sinyal Listrik Otot."
  },
  {
    "en": "Apa Itu Biopotensial (Biopotential)?",
    "id": "Sinyal Listrik Dalam Sel Hidup."
  },
  {
    "en": "Apa Itu Sistem Kontrol Non-Linear?",
    "id": "Sistem Tidak Mematuhi Superposisi."
  },
  {
    "en": "Apa Itu Logika Fuzzy (Fuzzy Logic)?",
    "id": "Logika Berbasis Derajat Kebenaran."
  },
  {
    "en": "Apa Itu Jaringan Saraf Tiruan (ANN)?",
    "id": "Model Komputasi Mirip Otak Manusia."
  },
  {
    "en": "Apa Itu Sistem Kontrol Adaptif?",
    "id": "Kontroler Menyesuaikan Parameter Otomatis."
  },
  {
    "en": "Apa Kepanjangan DOF (Degrees of Freedom) Robot?",
    "id": "Degrees of Freedom (Derajat Kebebasan)."
  },
  {
    "en": "Apa Itu Kinematika (Kinematics) Robot?",
    "id": "Ilmu Gerakan Robot (Geometri)."
  },
  {
    "en": "Apa Itu Kinematika Maju (Forward Kinematics)?",
    "id": "Menghitung Posisi End-Effector."
  },
  {
    "en": "Apa Itu Kinematika Balik (Inverse Kinematics)?",
    "id": "Menghitung Sudut Sendi (Joint)."
  },
  {
    "en": "Apa Itu End-Effector Robot?",
    "id": "Alat Kerja Di Ujung Lengan Robot."
  },
  {
    "en": "Apa Itu Sendi (Joint) Robot?",
    "id": "Bagian Bergerak Penghubung Link."
  },
  {
    "en": "Apa Itu Catu Daya Linear?",
    "id": "Menggunakan Trafo Dan Regulator Linear."
  },
  {
    "en": "Kelemahan Catu Daya Linear?",
    "id": "Efisiensi Rendah, Berat, Panas."
  },
  {
    "en": "Keunggulan Catu Daya Linear?",
    "id": "Noise Output (Ripple) Sangat Rendah."
  },
  {
    "en": "Apa Itu Regulator Linear?",
    "id": "IC Penstabil Tegangan DC (Contoh 7805)."
  },
  {
    "en": "Apa Kepanjangan LDO (Low Dropout Regulator)?",
    "id": "Low Dropout Regulator."
  },
  {
    "en": "Keunggulan LDO (Low Dropout Regulator)?",
    "id": "Beda Tegangan Input Output Kecil."
  },
  {
    "en": "Apa Itu Tegangan Referensi (Voltage Reference)?",
    "id": "Sumber Tegangan Stabil Presisi."
  },
  {
    "en": "Apa Kepanjangan PIV (Peak Inverse Voltage) Dioda?",
    "id": "Peak Inverse Voltage."
  },
  {
    "en": "Apa Arti PIV (Peak Inverse Voltage)?",
    "id": "Batas Tegangan Mundur Maksimum Dioda."
  },
  {
    "en": "Apa Itu Arus Maju (Forward Current) Dioda?",
    "id": "Batas Arus Maksimum Saat Forward Bias."
  },
  {
    "en": "Apa Itu Arus Lonjakan (Surge Current) Dioda?",
    "id": "Batas Arus Puncak Sesaat."
  },
  {
    "en": "Apa Topologi Half-Bridge SMPS?",
    "id": "Konverter Jembatan (Dua Saklar)."
  },
  {
    "en": "Apa Topologi Full-Bridge SMPS?",
    "id": "Konverter Jembatan (Empat Saklar)."
  },
  {
    "en": "Apa Itu Kontrol Mode Arus (Current Mode)?",
    "id": "Kontrol SMPS Berbasis Arus Induktor."
  },
  {
    "en": "Apa Itu Kontrol Mode Tegangan (Voltage Mode)?",
    "id": "Kontrol SMPS Berbasis Tegangan Output."
  },
  {
    "en": "Apa Itu Dioda Bodi (Body Diode) MOSFET?",
    "id": "Dioda Parasitik Internal MOSFET."
  },
  {
    "en": "Apa Itu Kapasitansi Miller (Miller Capacitance)?",
    "id": "Kapasitansi Cgd (Gate-Drain) MOSFET."
  },
  {
    "en": "Apa Itu Efek Tembok Awal (Early Effect)?",
    "id": "Efek Modulasi Lebar Basis BJT."
  },
  {
    "en": "Apa Itu Model Ebers-Moll?",
    "id": "Model Matematis Transistor BJT."
  },
  {
    "en": "Apa Itu Model Hybrid-Pi?",
    "id": "Model Rangkaian Sinyal Kecil BJT."
  },
  {
    "en": "Apa Itu Analisis Sinyal Kecil (Small Signal)?",
    "id": "Analisis Respon AC (Linearisasi)."
  },
  {
    "en": "Apa Itu Analisis Sinyal Besar (Large Signal)?",
    "id": "Analisis Respon DC (Non-Linear)."
  },
  {
    "en": "Apa Itu Resistor Film Tebal (Thick Film)?",
    "id": "Resistor Dibuat Teknik Cetak Pasta."
  },
  {
    "en": "Apa Itu Resistor Film Tipis (Thin Film)?",
    "id": "Resistor Dibuat Teknik Deposisi Uap."
  },
  {
    "en": "Apa Itu Resistor Kawat Lilit (Wirewound)?",
    "id": "Resistor Kawat Hambatan Dililitkan."
  },
  {
    "en": "Karakteristik Resistor Kawat Lilit?",
    "id": "Daya Tinggi, Punya Induktansi."
  },
  {
    "en": "Apa Itu Kapasitor Variabel (Varco)?",
    "id": "Kapasitor Nilainya Bisa Diatur (Tuning)."
  },
  {
    "en": "Apa Itu Kapasitor Film (Film Capacitor)?",
    "id": "Kapasitor Dielektrik Film Plastik."
  },
  {
    "en": "Karakteristik Kapasitor Film?",
    "id": "Stabil, Toleransi Baik, Non-Polar."
  },
  {
    "en": "Apa Itu Kapasitor Kopling (Coupling)?",
    "id": "Melewatkan Sinyal AC, Memblokir DC."
  },
  {
    "en": "Apa Itu Kapasitor Smoothing (Penghalus)?",
    "id": "Meratakan Riak Tegangan Hasil Penyearah."
  },
  {
    "en": "Apa Itu Kapasitor Bulk (Bulk Capacitor)?",
    "id": "Kapasitor Penyimpanan Energi Utama."
  },
  {
    "en": "Apa Itu Osilator Clapp?",
    "id": "Varian Osilator Colpitts (Lebih Stabil)."
  },
  {
    "en": "Apa Itu Osilator Pierce?",
    "id": "Osilator Kristal (Menggunakan Inverter Logika)."
  },
  {
    "en": "Apa Itu Rangkaian Diferensiator (Differentiator)?",
    "id": "Output Adalah Turunan (Derivative) Input."
  },
  {
    "en": "Apa Itu Rangkaian Integrator (Integrator)?",
    "id": "Output Adalah Integral Input."
  },
  {
    "en": "Kapan Integrator Op-Amp Menjadi Saturasi?",
    "id": "Saat Diberi Input DC."
  },
  {
    "en": "Apa Itu Rangkaian Penjumlah (Summing Amplifier)?",
    "id": "Output Adalah Jumlah Input Tertimbang."
  },
  {
    "en": "Apa Itu Penguat Diferensial (Differential Amplifier)?",
    "id": "Menguatkan Selisih Dua Tegangan Input."
  },
  {
    "en": "Apa Itu Penguat Instrumentasi (In-Amp)?",
    "id": "Penguat Diferensial Presisi Tinggi."
  },
  {
    "en": "Karakteristik Penguat Instrumentasi?",
    "id": "CMRR Tinggi, Impedansi Input Tinggi."
  },
  {
    "en": "Apa Itu Efek Termoelektrik?",
    "id": "Konversi Beda Suhu Ke Tegangan."
  },
  {
    "en": "Apa Itu Efek Thomson?",
    "id": "Pemanasan/Pendinginan Konduktor Berarus."
  },
  {
    "en": "Apa Itu Efek Volta?",
    "id": "Beda Potensial Kontak Dua Logam."
  },
  {
    "en": "Apa Itu Magnetoresistansi (Magnetoresistance)?",
    "id": "Perubahan Hambatan Akibat Medan Magnet."
  },
  {
    "en": "Apa Kepanjangan GMR (Giant Magnetoresistance)?",
    "id": "Giant Magnetoresistance."
  },
  {
    "en": "Aplikasi GMR (Giant Magnetoresistance)?",
    "id": "Sensor Hard Disk Drive (HDD)."
  },
  {
    "en": "Apa Itu Efek Wiegand?",
    "id": "Denyut Tegangan Akibat Perubahan Magnet."
  },
  {
    "en": "Apa Itu Magnetostriksi (Magnetostriction)?",
    "id": "Perubahan Bentuk Material Akibat Magnet."
  },
  {
    "en": "Apa Itu Generator Van De Graaff?",
    "id": "Penghasil Tegangan Tinggi Statis."
  },
  {
    "en": "Apa Itu Kumparan Tesla (Tesla Coil)?",
    "id": "Resonator Trafo Penghasil Tegangan Tinggi AC."
  },
  {
    "en": "Apa Itu Resistor Bleeder?",
    "id": "Resistor Pembuang Muatan Kapasitor."
  },
  {
    "en": "Fungsi Resistor Bleeder?",
    "id": "Keamanan (Mencegah Sengatan Sisa)."
  },
  {
    "en": "Apa Itu Band Gap Semikonduktor?",
    "id": "Energi Pemisah Pita Valensi Konduksi."
  },
  {
    "en": "Apa Satuan Band Gap?",
    "id": "Elektron-Volt (eV)."
  },
  {
    "en": "Apa Itu Pita Valensi (Valence Band)?",
    "id": "Pita Energi Terluar Elektron Terikat."
  },
  {
    "en": "Apa Itu Pita Konduksi (Conduction Band)?",
    "id": "Pita Energi Elektron Bebas Bergerak."
  },
  {
    "en": "Bagaimana Band Gap Konduktor?",
    "id": "Saling Tumpah Tindih (Nol)."
  },
  {
    "en": "Bagaimana Band Gap Isolator?",
    "id": "Sangat Lebar."
  },
  {
    "en": "Bagaimana Band Gap Semikonduktor?",
    "id": "Cukup Sempit (Bisa Dilewati)."
  },
  {
    "en": "Apa Itu Analisis Simpul (Nodal Analysis)?",
    "id": "Metode Analisis Rangkaian Berbasis KCL."
  },
  {
    "en": "Apa Variabel Utama Analisis Simpul?",
    "id": "Tegangan Simpul (Node Voltage)."
  },
  {
    "en": "Apa Itu Simpul Referensi (Reference Node)?",
    "id": "Simpul Patokan (Nol Volt/Ground)."
  },
  {
    "en": "Apa Itu Simpul Non-Referensi?",
    "id": "Simpul Yang Tegangan Dicari."
  },
  {
    "en": "Apa Itu Analisis Simpal (Mesh Analysis)?",
    "id": "Metode Analisis Rangkaian Berbasis KVL."
  },
  {
    "en": "Apa Variabel Utama Analisis Simpal?",
    "id": "Arus Simpal (Mesh Current)."
  },
  {
    "en": "Apa Itu Arus Cabang (Branch Current)?",
    "id": "Arus Aktual Yang Mengalir."
  },
  {
    "en": "Analisis Simpal (Mesh) Hanya Untuk Rangkaian Apa?",
    "id": "Rangkaian Planar (Datar)."
  },
  {
    "en": "Apa Itu Rangkaian Planar?",
    "id": "Dapat Digambar Tanpa Persilangan Kabel."
  },
  {
    "en": "Apa Itu Gaya Lorentz (Lorentz Force)?",
    "id": "Gaya Muatan Bergerak Medan EM."
  },
  {
    "en": "Apa Itu Permitivitas (Permittivity)?",
    "id": "Kemampuan Bahan Menyimpan Energi Listrik."
  },
  {
    "en": "Apa Simbol Permitivitas?",
    "id": "Epsilon (ε)."
  },
  {
    "en": "Apa Itu Permitivitas Relatif (εr)?",
    "id": "Rasio Permitivitas Bahan Terhadap Vakum."
  },
  {
    "en": "Apa Nama Lain Permitivitas Relatif?",
    "id": "Konstanta Dielektrik (k)."
  },
  {
    "en": "Apa Itu Suseptibilitas (Susceptibility) Magnetik?",
    "id": "Ukuran Magnetisasi Material."
  },
  {
    "en": "Apa Itu Suseptibilitas (Susceptibility) Listrik?",
    "id": "Ukuran Polarisasi Dielektrik."
  },
  {
    "en": "Apa Itu Kondisi Batas (Boundary Conditions) EM?",
    "id": "Perilaku Medan Di Batas Material."
  },
  {
    "en": "Apa Itu Rugi Dielektrik (Dielectric Loss)?",
    "id": "Energi Hilang (Panas) Pada Dielektrik."
  },
  {
    "en": "Apa Itu Tangen Rugi (Loss Tangent / Tan δ)?",
    "id": "Ukuran Kualitas Rugi Dielektrik."
  },
  {
    "en": "Apa Itu Polarisasi Dielektrik?",
    "id": "Pergeseran Muatan Material Akibat Medan."
  },
  {
    "en": "Apa Itu Bahan Piroelektrik (Pyroelectric)?",
    "id": "Hasilkan Tegangan Akibat Perubahan Suhu."
  },
  {
    "en": "Apa Kepanjangan SPWM (Sinusoidal Pulse Width Modulation)?",
    "id": "Sinusoidal Pulse Width Modulation."
  },
  {
    "en": "Dimana SPWM (Sinusoidal PWM) Digunakan?",
    "id": "Inverter (Membentuk Gelombang Sinus)."
  },
  {
    "en": "Apa Itu Indeks Modulasi Amplitudo (Ma)?",
    "id": "Rasio Amplitudo Referensi Carrier (SPWM)."
  },
  {
    "en": "Apa Itu Indeks Modulasi Frekuensi (Mf)?",
    "id": "Rasio Frekuensi Carrier Referensi (SPWM)."
  },
  {
    "en": "Apa Itu Rangkaian Crowbar (Crowbar Circuit)?",
    "id": "Rangkaian Proteksi Tegangan Berlebih."
  },
  {
    "en": "Bagaimana Rangkaian Crowbar Bekerja?",
    "id": "Hubung Singkat Paksa (Picu SCR)."
  },
  {
    "en": "Apa Itu Korelasi (Correlation) Sinyal?",
    "id": "Mengukur Kemiripan Dua Sinyal."
  },
  {
    "en": "Apa Itu Autokorelasi (Autocorrelation)?",
    "id": "Korelasi Sinyal Dengan Dirinya Sendiri."
  },
  {
    "en": "Apa Fungsi Autokorelasi?",
    "id": "Mendeteksi Periodisitas Dalam Sinyal."
  },
  {
    "en": "Apa Itu Kros-Korelasi (Cross-Correlation)?",
    "id": "Korelasi Antara Dua Sinyal Berbeda."
  },
  {
    "en": "Apa Fungsi Kros-Korelasi?",
    "id": "Menemukan Waktu Tunda (Delay) Sinyal."
  },
  {
    "en": "Apa Itu Sinyal Deterministik?",
    "id": "Sinyal Dapat Diprediksi Matematis."
  },
  {
    "en": "Apa Itu Sinyal Stokastik (Acak)?",
    "id": "Sinyal Tidak Dapat Diprediksi (Noise)."
  },
  {
    "en": "Apa Itu Kompensator (Compensator) Sistem Kontrol?",
    "id": "Rangkaian Perbaikan Respon Sistem."
  },
  {
    "en": "Apa Itu Kompensator Lag (Lag Compensator)?",
    "id": "Memperbaiki Error Steady-State."
  },
  {
    "en": "Apa Itu Kompensator Lead (Lead Compensator)?",
    "id": "Mempercepat Respon Transien Sistem."
  },
  {
    "en": "Apa Itu Kompensator Lead-Lag (Lead-Lag Compensator)?",
    "id": "Gabungan Keuntungan Lead Dan Lag."
  },
  {
    "en": "Apa Itu Regulator Tipe Shunt?",
    "id": "Regulator Terpasang Paralel Beban."
  },
  {
    "en": "Contoh Regulator Tipe Shunt?",
    "id": "Dioda Zener."
  },
  {
    "en": "Apa Itu Regulator Tipe Seri?",
    "id": "Regulator Terpasang Seri Beban."
  },
  {
    "en": "Contoh Regulator Tipe Seri?",
    "id": "IC 7805, LM317."
  },
  {
    "en": "Apa Itu IC (Integrated Circuit) LM317?",
    "id": "IC Regulator Tegangan Variabel Positif."
  },
  {
    "en": "Apa Itu IC (Integrated Circuit) LM337?",
    "id": "IC Regulator Tegangan Variabel Negatif."
  },
  {
    "en": "Apa Itu IC (Integrated Circuit) LM7805?",
    "id": "IC Regulator Tetap 5 Volt Positif."
  },
  {
    "en": "Apa Itu IC (Integrated Circuit) LM7905?",
    "id": "IC Regulator Tetap 5 Volt Negatif."
  },
  {
    "en": "Apa Itu Tegangan Offset Input Op-Amp?",
    "id": "Beda Tegangan Input (Output Nol)."
  },
  {
    "en": "Apa Itu Arus Bias Input Op-Amp?",
    "id": "Arus DC Yang Dibutuhkan Input Op-Amp."
  },
  {
    "en": "Apa Itu Tegangan Offset Output Op-Amp?",
    "id": "Tegangan Output Saat Input Nol."
  },
  {
    "en": "Apa Kepanjangan GBWP (Gain Bandwidth Product) Op-Amp?",
    "id": "Gain Bandwidth Product."
  },
  {
    "en": "Apa Arti GBWP (Gain Bandwidth Product)?",
    "id": "Perkalian Gain Dan Bandwidth Konstan."
  },
  {
    "en": "Apa Itu Ground Loop (Loop Pentanahan)?",
    "id": "Noise Akibat Beda Potensial Ground."
  },
  {
    "en": "Bagaimana Mengatasi Ground Loop (Audio)?",
    "id": "Isolasi Ground (Contoh: DI Box)."
  },
  {
    "en": "Apa Itu Kabel Litz (Litz Wire)?",
    "id": "Kabel Serabut Terisolasi Individual."
  },
  {
    "en": "Apa Fungsi Kabel Litz (Litz Wire)?",
    "id": "Mengurangi Efek Kulit (Skin Effect)."
  },
  {
    "en": "Apa Itu Pembumian (Earthing)?",
    "id": "Koneksi Ke Massa Bumi (Keamanan)."
  },
  {
    "en": "Apa Itu Pentanahan (Grounding)?",
    "id": "Titik Referensi Nol Volt Rangkaian."
  },
  {
    "en": "Apa Perbedaan Earthing Dan Grounding?",
    "id": "Earthing (Keamanan), Grounding (Referensi)."
  },
  {
    "en": "Apa Itu Turbin Uap (Steam Turbine)?",
    "id": "Mengubah Energi Uap Menjadi Putaran."
  },
  {
    "en": "Apa Itu Turbin Gas (Gas Turbine)?",
    "id": "Mengubah Energi Gas Panas Menjadi Putaran."
  },
  {
    "en": "Apa Itu Turbin Air (Water Turbine)?",
    "id": "Mengubah Energi Air Jatuh Menjadi Putaran."
  },
  {
    "en": "Apa Contoh Turbin Air Tipe Impuls?",
    "id": "Turbin Pelton."
  },
  {
    "en": "Apa Contoh Turbin Air Tipe Reaksi?",
    "id": "Turbin Francis, Turbin Kaplan."
  },
  {
    "en": "Apa Itu Boiler (Ketel Uap)?",
    "id": "Pemanas Air Menjadi Uap (PLTU)."
  },
  {
    "en": "Apa Itu Kondensor (Pembangkit Listrik)?",
    "id": "Mengubah Uap Kembali Menjadi Air."
  },
  {
    "en": "Apa Itu Siklus Rankine (Rankine Cycle)?",
    "id": "Siklus Termodinamika Dasar PLTU."
  },
  {
    "en": "Apa Itu Siklus Brayton (Brayton Cycle)?",
    "id": "Siklus Termodinamika Dasar Turbin Gas."
  },
  {
    "en": "Apa Itu Pembangkit Siklus Gabungan (Combined Cycle)?",
    "id": "Gabungan Siklus Brayton Dan Rankine."
  },
  {
    "en": "Keuntungan Pembangkit Siklus Gabungan?",
    "id": "Efisiensi Termal Sangat Tinggi."
  },
  {
    "en": "Apa Itu Reaktor Fisi Nuklir?",
    "id": "Tempat Reaksi Fisi Berantai (PLTN)."
  },
  {
    "en": "Apa Itu Batang Kendali (Control Rods) PLTN?",
    "id": "Pengatur Laju Reaksi Fisi."
  },
  {
    "en": "Apa Bahan Batang Kendali (Control Rods)?",
    "id": "Penyerap Neutron (Boron, Kadmium)."
  },
  {
    "en": "Apa Itu Moderator (PLTN)?",
    "id": "Memperlambat Laju Neutron Cepat."
  },
  {
    "en": "Apa Contoh Bahan Moderator (PLTN)?",
    "id": "Air Berat, Air Ringan, Grafit."
  },
  {
    "en": "Apa Itu Pendingin (Coolant) PLTN?",
    "id": "Fluida Penyerap Panas Reaktor."
  },
  {
    "en": "Apa Itu Penukar Panas (Heat Exchanger)?",
    "id": "Memindahkan Panas Antar Dua Fluida."
  },
  {
    "en": "Apa Itu Reaktor Air Didih (BWR)?",
    "id": "Air Mendidih Langsung Dalam Reaktor."
  },
  {
    "en": "Apa Itu Reaktor Air Tekan (PWR)?",
    "id": "Air Tekanan Tinggi (Tidak Mendidih)."
  },
  {
    "en": "Apa Itu Pengayaan Uranium (Uranium Enrichment)?",
    "id": "Meningkatkan Konsentrasi Isotop U-235."
  },
  {
    "en": "Apa Itu Trafo Tiga Lilitan (Tertiary Winding)?",
    "id": "Trafo Tiga Sisi (Primer, Sekunder, Tersier)."
  },
  {
    "en": "Fungsi Lilitan Tersier (Tertiary Winding) Trafo?",
    "id": "Menekan Harmonisa, Pasokan Beban Tambahan."
  },
  {
    "en": "Apa Itu Saturasi Magnetik?",
    "id": "Kondisi Inti Magnet Jenuh Fluks."
  },
  {
    "en": "Dampak Saturasi Magnetik Trafo?",
    "id": "Arus Magnetisasi Sangat Tinggi."
  },
  {
    "en": "Apa Itu Arus Inrush Trafo?",
    "id": "Lonjakan Arus Saat Trafo Di-Energize."
  },
  {
    "en": "Apa Itu Reluktansi (Reluctance) Magnetik?",
    "id": "Hambatan Terhadap Aliran Fluks Magnetik."
  },
  {
    "en": "Apa Itu Magnetomotif (MMF / Gaya Gerak Magnet)?",
    "id": "Gaya Pembangkit Fluks Magnetik."
  },
  {
    "en": "Apa Satuan MMF (Gaya Gerak Magnet)?",
    "id": "Amper-Lilitan (Ampere-Turns)."
  },
  {
    "en": "Apa Itu Koersivitas (Coercivity)?",
    "id": "Kekuatan Medan Penghilang Sisa Magnet."
  },
  {
    "en": "Apa Itu Retentivitas (Retentivity)?",
    "id": "Ukuran Sisa Magnetisme Material."
  },
  {
    "en": "Apa Itu Magnet Keras (Hard Magnet)?",
    "id": "Koersivitas Tinggi (Magnet Permanen)."
  },
  {
    "en": "Apa Itu Magnet Lunak (Soft Magnet)?",
    "id": "Koersivitas Rendah (Inti Trafo)."
  },
  {
    "en": "Apa Itu Laminasi (Lamination) Inti Besi?",
    "id": "Lapisan Tipis Inti Besi."
  },
  {
    "en": "Tujuan Laminasi (Lamination) Inti Besi?",
    "id": "Mengurangi Kerugian Arus Eddy."
  },
  {
    "en": "Apa Itu Penguat Umpan Balik (Feedback Amplifier)?",
    "id": "Sebagian Sinyal Output Kembali Ke Input."
  },
  {
    "en": "Apa Itu Umpan Balik Positif (Positive Feedback)?",
    "id": "Memperkuat Sinyal (Osilasi)."
  },
  {
    "en": "Apa Itu Umpan Balik Negatif (Negative Feedback)?",
    "id": "Menstabilkan Sinyal (Mengurangi Gain)."
  },
  {
    "en": "Keuntungan Umpan Balik Negatif?",
    "id": "Meningkatkan Stabilitas, Mengurangi Distorsi."
  },
  {
    "en": "Apa Itu Konektor DB9 (D-Subminiature 9-Pin)?",
    "id": "Konektor Standar Port Serial RS232."
  },
  {
    "en": "Apa Itu Ruang Kerja (Workspace) Robot?",
    "id": "Area Jangkauan Maksimal End-Effector."
  },
  {
    "en": "Apa Itu Sendi (Joint) Revolute Robot?",
    "id": "Sendi Putar (Seperti Siku)."
  },
  {
    "en": "Apa Itu Sendi (Joint) Prismatik Robot?",
    "id": "Sendi Geser (Gerakan Linear)."
  },
  {
    "en": "Apa Itu Robot SCARA (Selective Compliance Assembly Robot Arm)?",
    "id": "Robot Cepat Untuk Perakitan."
  },
  {
    "en": "Apa Itu Robot Kartesian (Cartesian Robot)?",
    "id": "Robot Bergerak Sumbu X-Y-Z."
  },
  {
    "en": "Apa Itu Robot Artikulasi (Articulated Robot)?",
    "id": "Robot Lengan (Mirip Lengan Manusia)."
  },
  {
    "en": "Apa Itu Robot Paralel (Parallel Robot)?",
    "id": "Mekanisme Loop Tertutup (Contoh: Delta)."
  },
  {
    "en": "Apa Itu Memori Volatil (Volatile Memory)?",
    "id": "Data Hilang Saat Daya Mati."
  },
  {
    "en": "Contoh Memori Volatil?",
    "id": "RAM (SRAM, DRAM)."
  },
  {
    "en": "Apa Itu Memori Non-Volatil (Non-Volatile Memory)?",
    "id": "Data Tetap Saat Daya Mati."
  },
  {
    "en": "Contoh Memori Non-Volatil?",
    "id": "ROM, EPROM, Flash Memory, HDD."
  },
  {
    "en": "Apa Itu Cache (Memori Cepat)?",
    "id": "Memori Cepat Antara CPU Dan RAM."
  },
  {
    "en": "Apa Tujuan Cache?",
    "id": "Mempercepat Akses Data CPU."
  },
  {
    "en": "Apa Itu Arsitektur Von Neumann?",
    "id": "Data Dan Program Disimpan Bersama."
  },
  {
    "en": "Apa Itu Arsitektur Harvard?",
    "id": "Data Dan Program Disimpan Terpisah."
  },
  {
    "en": "Keuntungan Arsitektur Harvard?",
    "id": "Akses Data Program Bersamaan."
  },
  {
    "en": "Apa Itu Siklus Instruksi (Instruction Cycle) CPU?",
    "id": "Fetch, Decode, Execute."
  },
  {
    "en": "Apa Itu Fase Fetch (Ambil) CPU?",
    "id": "Mengambil Instruksi Dari Memori."
  },
  {
    "en": "Apa Itu Fase Decode (Urai) CPU?",
    "id": "Menerjemahkan Instruksi."
  },
  {
    "en": "Apa Itu Fase Execute (Eksekusi) CPU?",
    "id": "Menjalankan Perintah Instruksi."
  },
  {
    "en": "Apa Itu Register CPU?",
    "id": "Unit Memori Kecil Cepat Di CPU."
  },
  {
    "en": "Apa Itu Program Counter (PC) CPU?",
    "id": "Register Alamat Instruksi Berikutnya."
  },
  {
    "en": "Apa Itu Instruction Register (IR) CPU?",
    "id": "Register Penyimpan Instruksi Saat Ini."
  },
  {
    "en": "Apa Itu Pipelining (CPU)?",
    "id": "Teknik Eksekusi Instruksi Tumpang Tindih."
  },
  {
    "en": "Tujuan Pipelining (CPU)?",
    "id": "Meningkatkan Throughput (Kinerja) CPU."
  },
  {
    "en": "Apa Itu Propagasi Fading (Pelemahan)?",
    "id": "Fluktuasi Kekuatan Sinyal Radio."
  },
  {
    "en": "Apa Itu Propagasi Multipath (Lintasan Ganda)?",
    "id": "Sinyal Tiba Lewat Banyak Jalur."
  },
  {
    "en": "Dampak Propagasi Multipath?",
    "id": "Interferensi (Konstruktif Atau Destruktif)."
  },
  {
    "en": "Apa Itu Fading Cepat (Fast Fading)?",
    "id": "Perubahan Cepat (Akibat Gerakan)."
  },
  {
    "en": "Apa Itu Fading Lambat (Slow Fading)?",
    "id": "Perubahan Lambat (Akibat Rintangan)."
  },
  {
    "en": "Apa Itu Zona Fresnel (Fresnel Zone)?",
    "id": "Area Elips Antara Pemancar Penerima."
  },
  {
    "en": "Mengapa Zona Fresnel (Fresnel Zone) Harus Bebas Hambatan?",
    "id": "Menghindari Pelemahan Sinyal."
  },
  {
    "en": "Apa Itu Atenuasi Hujan (Rain Attenuation)?",
    "id": "Pelemahan Sinyal Akibat Air Hujan."
  },
  {
    "en": "Frekuensi Mana Yang Rentan Atenuasi Hujan?",
    "id": "Frekuensi Microwave (Di Atas 10 GHz)."
  },
  {
    "en": "Apa Itu Noise Atmosfer?",
    "id": "Gangguan Sinyal Akibat Aktivitas Atmosfer."
  },
  {
    "en": "Apa Itu Noise Kosmik (Cosmic Noise)?",
    "id": "Gangguan Sinyal Dari Luar Angkasa."
  },
  {
    "en": "Apa Itu Noise Man-Made (Buatan Manusia)?",
    "id": "Gangguan Dari Peralatan Listrik."
  },
  {
    "en": "Apa Itu Sistem Proteksi Petir Eksternal?",
    "id": "Melindungi Struktur Fisik Bangunan."
  },
  {
    "en": "Komponen Utama Proteksi Petir Eksternal?",
    "id": "Air Terminal, Konduktor, Pentanahan."
  },
  {
    "en": "Apa Itu Air Terminal (Finial)?",
    "id": "Ujung Tombak Penangkal Petir."
  },
  {
    "en": "Apa Itu Konduktor Penyalur (Down Conductor)?",
    "id": "Kabel Penyalur Arus Petir Ke Tanah."
  },
  {
    "en": "Apa Itu Sistem Pentanahan (Earthing System) Petir?",
    "id": "Menyebarkan Arus Petir Ke Tanah."
  },
  {
    "en": "Apa Itu Metode Sangkar Faraday (Proteksi Petir)?",
    "id": "Sistem Konduktor Menyelubungi Gedung."
  },
  {
    "en": "Apa Itu Sistem Proteksi Petir Internal?",
    "id": "Melindungi Peralatan Elektronik."
  },
  {
    "en": "Komponen Utama Proteksi Petir Internal?",
    "id": "SPD (Surge Protection Device)."
  },
  {
    "en": "Apa Itu Ikatan Ekuipotensial (Equipotential Bonding)?",
    "id": "Menyamakan Potensial Semua Konduktor."
  },
  {
    "en": "Tujuan Ikatan Ekuipotensial?",
    "id": "Mencegah Percikan Api Akibat Beda Tegangan."
  },
  {
    "en": "Apa Itu Tegangan Langkah (Step Potential)?",
    "id": "Beda Tegangan Antara Dua Kaki."
  },
  {
    "en": "Apa Itu Tegangan Sentuh (Touch Potential)?",
    "id": "Beda Tegangan Antara Tangan Kaki."
  },
  {
    "en": "Bahaya Tegangan Langkah Dan Sentuh?",
    "id": "Terjadi Saat Sambaran Petir Dekat."
  },
  {
    "en": "Apa Itu Menara (Tower) Transmisi Tipe Lattice?",
    "id": "Menara Rangka Besi Siku (SUTET)."
  },
  {
    "en": "Apa Itu Menara (Tower) Transmisi Tipe Monopole?",
    "id": "Menara Satu Tiang (Area Perkotaan)."
  },
  {
    "en": "Apa Itu Menara (Tower) Transmisi Tipe Guyed?",
    "id": "Menara Diperkuat Kabel Kawat Baja."
  },
  {
    "en": "Apa Fungsi Kawat Tanah (Ground Wire) Di SUTET?",
    "id": "Melindungi Kawat Fasa Dari Petir."
  },
  {
    "en": "Dimana Posisi Kawat Tanah (Ground Wire)?",
    "id": "Di Bagian Paling Atas Menara."
  },
  {
    "en": "Apa Kepanjangan OPGW (Optical Ground Wire)?",
    "id": "Optical Ground Wire."
  },
  {
    "en": "Fungsi Ganda OPGW (Optical Ground Wire)?",
    "id": "Kawat Tanah Dan Serat Optik."
  },
  {
    "en": "Apa Itu Isolator Tipe Post (Post Insulator)?",
    "id": "Isolator Penyangga (Tegak Lurus)."
  },
  {
    "en": "Apa Itu Isolator Tipe Strain (Strain Insulator)?",
    "id": "Isolator Penahan Tarikan (Horizontal)."
  },
  {
    "en": "Apa Itu Jarak Rambat (Creepage Distance) Isolator?",
    "id": "Jarak Permukaan Isolator (Cegah Arus Bocor)."
  },
  {
    "en": "Apa Itu Flashover Isolator?",
    "id": "Loncatan Api Listrik Di Permukaan Isolator."
  },
  {
    "en": "Apa Itu Beban Mekanis (Mechanical Load) Jaringan?",
    "id": "Beban Akibat Angin, Es, Berat Kabel."
  },
  {
    "en": "Apa Itu Andongan (Sag) Kabel Transmisi?",
    "id": "Lengkungan Kabel Antar Menara."
  },
  {
    "en": "Mengapa Andongan (Sag) Kabel Penting Diperhitungkan?",
    "id": "Jarak Aman (Clearance) Ke Tanah."
  },
  {
    "en": "Apa Itu Efek Ferranti (Ferranti Effect)?",
    "id": "Tegangan Ujung Saluran Lebih Tinggi."
  },
  {
    "en": "Kapan Efek Ferranti (Ferranti Effect) Terjadi?",
    "id": "Saluran Panjang Tanpa Beban."
  },
  {
    "en": "Penyebab Efek Ferranti?",
    "id": "Kapasitansi Shunt Saluran Transmisi."
  },
  {
    "en": "Bagaimana Mengatasi Efek Ferranti?",
    "id": "Memasang Reaktor Shunt."
  },
  {
    "en": "Apa Itu Reaktor Shunt (Shunt Reactor)?",
    "id": "Induktor Paralel Menyerap Daya Reaktif."
  },
  {
    "en": "Apa Itu Kapasitor Seri (Series Capacitor)?",
    "id": "Mengurangi Reaktansi Induktif Saluran."
  },
  {
    "en": "Tujuan Kapasitor Seri?",
    "id": "Meningkatkan Kemampuan Transfer Daya."
  },
  {
    "en": "Apa Itu Kapasitor Shunt (Shunt Capacitor)?",
    "id": "Menyuplai Daya Reaktif (Perbaikan PF)."
  },
  {
    "en": "Perbedaan Termistor Dan RTD (Resistance Temperature Detector)?",
    "id": "Termistor (Semikonduktor), RTD (Logam)."
  },
  {
    "en": "Mana Lebih Sensitif (Termistor/RTD)?",
    "id": "Termistor (Perubahan Hambatan Besar)."
  },
  {
    "en": "Mana Lebih Linear (Termistor/RTD)?",
    "id": "RTD (Resistance Temperature Detector)."
  },
  {
    "en": "Apa Itu Buzzer Piezoelektrik?",
    "id": "Pembangkit Suara Getaran Material Piezo."
  },
  {
    "en": "Apa Itu Pengeras Suara (Speaker) Dinamis?",
    "id": "Menggunakan Kumparan Bergerak (Voice Coil)."
  },
  {
    "en": "Apa Itu Mikrofon Kondensor?",
    "id": "Mikrofon Berbasis Kapasitor."
  },
  {
    "en": "Apakah Mikrofon Kondensor Butuh Daya?",
    "id": "Ya (Phantom Power)."
  },
  {
    "en": "Apa Itu Mikrofon Dinamis?",
    "id": "Mikrofon Berbasis Induksi (Kebalikan Speaker)."
  },
  {
    "en": "Apakah Mikrofon Dinamis Butuh Daya?",
    "id": "Tidak."
  },
  {
    "en": "Apa Itu Phantom Power?",
    "id": "Daya DC Dikirim Lewat Kabel Audio."
  },
  {
    "en": "Apa Itu Kabel Audio Seimbang (Balanced)?",
    "id": "Tiga Konduktor (Sinyal +, Sinyal -, Ground)."
  },
  {
    "en": "Keuntungan Kabel Audio Seimbang?",
    "id": "Menolak Noise (Interferensi)."
  },
  {
    "en": "Apa Itu Kabel Audio Tidak Seimbang (Unbalanced)?",
    "id": "Dua Konduktor (Sinyal, Ground)."
  },
  {
    "en": "Contoh Konektor Audio Seimbang?",
    "id": "XLR, TRS (Tip-Ring-Sleeve)."
  },
  {
    "en": "Contoh Konektor Audio Tidak Seimbang?",
    "id": "TS (Tip-Sleeve), RCA."
  },
  {
    "en": "Apa Kepanjangan DI (Direct Injection) Box?",
    "id": "Direct Injection Box."
  },
  {
    "en": "Fungsi DI (Direct Injection) Box?",
    "id": "Mengubah Sinyal Unbalanced Ke Balanced."
  },
  {
    "en": "Apa Itu Ground Lift Switch?",
    "id": "Saklar Pemutus Ground (Atasi Ground Loop)."
  },
  {
    "en": "Apa Itu Penguat Operasional Ideal?",
    "id": "Gain Tak Terhingga, Impedansi Input Tak Terhingga."
  },
  {
    "en": "Apa Itu Virtual Ground (Tanah Maya)?",
    "id": "Titik Input Inverting Op-Amp (0 Volt)."
  },
  {
    "en": "Kapan Virtual Ground (Tanah Maya) Terjadi?",
    "id": "Pada Konfigurasi Umpan Balik Negatif."
  },
  {
    "en": "Apa Itu Beban (Load) Listrik?",
    "id": "Komponen Konsumsi Energi Listrik."
  },
  {
    "en": "Apa Itu Beban Linear?",
    "id": "Hambatan Konstan (Resistor)."
  },
  {
    "en": "Apa Itu Beban Non-Linear?",
    "id": "Hambatan Berubah (Dioda, SMPS)."
  },
  {
    "en": "Apa Itu Latching Current (Arus Pengunci) SCR?",
    "id": "Arus Anoda Minimum Agar SCR Tetap ON."
  },
  {
    "en": "Apa Itu Holding Current (Arus Penahan) SCR?",
    "id": "Arus Anoda Minimum Menahan SCR ON."
  },
  {
    "en": "Mana Lebih Besar, Latching Atau Holding Current?",
    "id": "Latching Current (Arus Pengunci)."
  },
  {
    "en": "Apa Itu Komutasi (Commutation) Thyristor?",
    "id": "Proses Mematikan (Turn-Off) Thyristor."
  },
  {
    "en": "Apa Itu Komutasi Alami (Natural Commutation)?",
    "id": "Mematikan SCR Menggunakan Sinyal AC."
  },
  {
    "en": "Apa Itu Komutasi Paksa (Forced Commutation)?",
    "id": "Mematikan SCR Menggunakan Rangkaian Eksternal."
  },
  {
    "en": "Apa Itu Proteksi DV/DT (Perubahan Tegangan)?",
    "id": "Mencegah SCR ON Akibat Lonjakan Tegangan."
  },
  {
    "en": "Apa Itu Proteksi DI/DT (Perubahan Arus)?",
    "id": "Mencegah Kerusakan SCR Akibat Lonjakan Arus."
  },
  {
    "en": "Apa Fungsi Rangkaian Snubber?",
    "id": "Melindungi Saklar Elektronik Dari Lonjakan."
  },
  {
    "en": "Apa Itu Sekring (Fuse) Termal?",
    "id": "Sekring Putus Berdasarkan Suhu Berlebih."
  },
  {
    "en": "Apa Itu Resistansi Isolasi (Insulation Resistance)?",
    "id": "Hambatan Bahan Isolator (Sangat Tinggi)."
  },
  {
    "en": "Apa Itu Kegagalan Tembus (Dielectric Breakdown)?",
    "id": "Isolator Gagal Menahan Tegangan."
  },
  {
    "en": "Apa Itu Penindasan Busur Api (Arc Suppression)?",
    "id": "Metode Memadamkan Busur Api Listrik."
  },
  {
    "en": "Apa Itu Media Pemadam (Quenching) CB?",
    "id": "Bahan Pemadam Busur Api (Udara, Minyak)."
  },
  {
    "en": "Apa Kepanjangan VCB (Vacuum Circuit Breaker)?",
    "id": "Vacuum Circuit Breaker."
  },
  {
    "en": "Apa Kepanjangan SF6 (Sulfur Hexafluoride) Circuit Breaker?",
    "id": "Sulfur Hexafluoride Circuit Breaker."
  },
  {
    "en": "Keunggulan Gas SF6 (Sulfur Hexafluoride)?",
    "id": "Isolator Sangat Kuat, Pemadam Busur Api."
  },
  {
    "en": "Apa Itu Saklar Pemisah (Disconnector)?",
    "id": "Saklar Pemutus Tanpa Beban."
  },
  {
    "en": "Bisakah Pemisah (Disconnector) Memutus Arus Beban?",
    "id": "Tidak (Akan Menimbulkan Busur Api)."
  },
  {
    "en": "Apa Itu Saklar Pemutus Beban (Load Break Switch)?",
    "id": "Saklar Mampu Memutus Arus Beban."
  },
  {
    "en": "Apa Nama Lain Saklar Pemisah (Disconnector)?",
    "id": "PMS (Pemisah) Atau Isolator."
  },
  {
    "en": "Apa Kepanjangan RMU (Ring Main Unit)?",
    "id": "Ring Main Unit."
  },
  {
    "en": "Fungsi RMU (Ring Main Unit)?",
    "id": "Titik Sambung Distribusi Jaringan Cincin."
  },
  {
    "en": "Apa Itu Gardu Pilar (Feeder Pillar)?",
    "id": "Panel Pembagi Jaringan Tegangan Rendah."
  },
  {
    "en": "Apa Kepanjangan NER (Neutral Earthing Resistor)?",
    "id": "Neutral Earthing Resistor."
  },
  {
    "en": "Fungsi NER (Neutral Earthing Resistor)?",
    "id": "Membatasi Arus Gangguan Fasa-Tanah."
  },
  {
    "en": "Apa Itu Pentanahan Solid (Solidly Earthed)?",
    "id": "Titik Netral Langsung Ke Tanah."
  },
  {
    "en": "Apa Itu Pentanahan Resonansi (Petersen Coil)?",
    "id": "Pentanahan Netral Melalui Reaktor."
  },
  {
    "en": "Fungsi Kumparan Petersen (Petersen Coil)?",
    "id": "Menetralisir Arus Gangguan Fasa-Tanah."
  },
  {
    "en": "Apa Itu Diagram Satu Garis (SLD)?",
    "id": "Representasi Sederhana Sistem Tiga Fasa."
  },
  {
    "en": "Apa Itu Fasor (Phasor)?",
    "id": "Vektor Representasi Gelombang Sinus AC."
  },
  {
    "en": "Apa Itu Diagram Fasor (Phasor Diagram)?",
    "id": "Grafik Vektor Hubungan Fasa Tegangan Arus."
  },
  {
    "en": "Apa Itu Nilai Rata-Rata (Average Value) Sinusoida?",
    "id": "Nol (Satu Siklus Penuh)."
  },
  {
    "en": "Nilai Rata-Rata (Average) Setengah Gelombang?",
    "id": "0.637 Kali Nilai Puncak."
  },
  {
    "en": "Apa Itu Faktor Bentuk (Form Factor)?",
    "id": "Rasio Nilai RMS Terhadap Rata-Rata."
  },
  {
    "en": "Berapa Faktor Bentuk (Form Factor) Sinusoida?",
    "id": "Satu Koma Satu Satu (1.11)."
  },
  {
    "en": "Apa Itu Sinyal Searah (Unipolar)?",
    "id": "Sinyal Hanya Punya Satu Polaritas."
  },
  {
    "en": "Apa Itu Sinyal Bolak-Balik (Bipolar)?",
    "id": "Sinyal Punya Polaritas Positif Negatif."
  },
  {
    "en": "Apa Itu Pergeseran Fasa (Phase Shift)?",
    "id": "Perbedaan Sudut Fasa Dua Sinyal."
  },
  {
    "en": "Apa Arti Fasa Mendahului (Leading)?",
    "id": "Sinyal Mencapai Puncak Lebih Cepat."
  },
  {
    "en": "Apa Arti Fasa Tertinggal (Lagging)?",
    "id": "Sinyal Mencapai Puncak Lebih Lambat."
  },
  {
    "en": "Beban Apa Penyebab Fasa Mendahului?",
    "id": "Beban Kapasitif (Arus Mendahului Tegangan)."
  },
  {
    "en": "Beban Apa Penyebab Fasa Tertinggal?",
    "id": "Beban Induktif (Arus Tertinggal Tegangan)."
  },
  {
    "en": "Apa Kepanjangan PMU (Phasor Measurement Unit)?",
    "id": "Phasor Measurement Unit."
  },
  {
    "en": "Apa Itu Synchrophasor?",
    "id": "Data Fasor Sinkron Waktu (GPS)."
  },
  {
    "en": "Apa Kepanjangan WAMS (Wide Area Monitoring System)?",
    "id": "Wide Area Monitoring System."
  },
  {
    "en": "Apa Itu Konduktivitas Termal?",
    "id": "Kemampuan Material Menghantar Panas."
  },
  {
    "en": "Apa Itu Resistansi Termal?",
    "id": "Kemampuan Material Menghambat Panas."
  },
  {
    "en": "Apa Kepanjangan CTE (Coefficient of Thermal Expansion)?",
    "id": "Koefisien Muai Panas."
  },
  {
    "en": "Apa Kepanjangan TEC (Thermoelectric Cooler)?",
    "id": "Thermoelectric Cooler (Elemen Peltier)."
  },
  {
    "en": "Apa Kepanjangan TEG (Thermoelectric Generator)?",
    "id": "Thermoelectric Generator (Efek Seebeck)."
  },
  {
    "en": "Apa Kepanjangan CTR (Current Transfer Ratio)?",
    "id": "Current Transfer Ratio."
  },
  {
    "en": "Apa Arti CTR (Current Transfer Ratio) Optocoupler?",
    "id": "Rasio Arus Output (Transistor) Input (LED)."
  },
  {
    "en": "Apa Itu Isolasi Galvatis (Galvanic Isolation)?",
    "id": "Pemisahan Rangkaian Tanpa Koneksi Listrik."
  },
  {
    "en": "Mengapa Perlu Isolasi Galvatis?",
    "id": "Keamanan Dan Mencegah Ground Loop."
  },
  {
    "en": "Apa Itu Transformator Isolasi (Safety Transformer)?",
    "id": "Trafo Rasio 1:1 Untuk Isolasi."
  },
  {
    "en": "Apa Itu Kapasitor Kelas Y (Class-Y)?",
    "id": "Kapasitor Keamanan (Line Ke Ground)."
  },
  {
    "en": "Apa Itu Kapasitor Kelas X (Class-X)?",
    "id": "Kapasitor Keamanan (Line Ke Line)."
  },
  {
    "en": "Fungsi Kapasitor Kelas Y?",
    "id": "Menekan Noise Common-Mode."
  },
  {
    "en": "Fungsi Kapasitor Kelas X?",
    "id": "Menekan Noise Differential-Mode."
  },
  {
    "en": "Apa Itu Filter Saluran (Line Filter) EMI?",
    "id": "Filter Mencegah Noise Masuk/Keluar Jala-Jala."
  },
  {
    "en": "Apa Itu Kapasitor Feedthrough?",
    "id": "Kapasitor Filter EMI Frekuensi Tinggi."
  },
  {
    "en": "Apa Itu Gasket EMI (EMI Gasket)?",
    "id": "Segel Konduktif Untuk Shielding EMI."
  },
  {
    "en": "Apa Itu Ruang Anechoic (Anechoic Chamber)?",
    "id": "Ruang Bebas Gema (Uji EMC/Audio)."
  },
  {
    "en": "Apa Itu Field Strength Meter?",
    "id": "Alat Ukur Kuat Medan Elektromagnetik."
  },
  {
    "en": "Apa Kepanjangan VNA (Vector Network Analyzer)?",
    "id": "Vector Network Analyzer."
  },
  {
    "en": "Apa Yang Diukur VNA (Vector Network Analyzer)?",
    "id": "Parameter-S (Karakteristik RF)."
  },
  {
    "en": "Apa Kepanjangan TDR (Time Domain Reflectometer)?",
    "id": "Time Domain Reflectometer."
  },
  {
    "en": "Fungsi TDR (Time Domain Reflectometer)?",
    "id": "Mendeteksi Lokasi Kerusakan Kabel."
  },
  {
    "en": "Apa Itu Hipot (High Potential) Tester?",
    "id": "Alat Uji Tegangan Tinggi (Isolasi)."
  },
  {
    "en": "Tujuan Uji Hipot (High Potential)?",
    "id": "Memastikan Kekuatan Dielektrik Isolasi."
  },
  {
    "en": "Apa Itu Uji Kontinuitas (Continuity Test)?",
    "id": "Memeriksa Sambungan Kabel (Tidak Putus)."
  },
  {
    "en": "Apa Itu Pengukuran Kelvin (4-Kawat)?",
    "id": "Metode Ukur Resistansi Sangat Rendah."
  },
  {
    "en": "Tujuan Pengukuran Kelvin?",
    "id": "Menghilangkan Error Resistansi Kabel Probe."
  },
  {
    "en": "Apa Itu Kumparan Rogowski (Rogowski Coil)?",
    "id": "Sensor Arus AC Fleksibel."
  },
  {
    "en": "Perbedaan Kumparan Rogowski Dan CT?",
    "id": "Rogowski (Inti Udara), CT (Inti Besi)."
  },
  {
    "en": "Apa Itu Saklar Putar (Rotary Switch)?",
    "id": "Saklar Pemilih Multi-Posisi."
  },
  {
    "en": "Apa Kepanjangan Saklar DIP (Dual In-line Package)?",
    "id": "Dual In-line Package Switch."
  },
  {
    "en": "Fungsi Saklar DIP (Dual In-line Package)?",
    "id": "Pengaturan Konfigurasi Pada PCB."
  },
  {
    "en": "Apa Itu Saklar Taktil (Tactile Switch)?",
    "id": "Tombol Tekan Kecil (Ada Umpan Balik Klik)."
  },
  {
    "en": "Apa Itu Saklar Membran (Membrane Switch)?",
    "id": "Saklar Datar Fleksibel (Keypad)."
  },
  {
    "en": "Apa Itu Saklar Buluh (Reed Switch)?",
    "id": "Saklar Kontak Logam Dalam Tabung Kaca."
  },
  {
    "en": "Bagaimana Saklar Buluh (Reed Switch) Aktif?",
    "id": "Oleh Medan Magnet Eksternal."
  },
  {
    "en": "Apa Itu Saklar Raksa (Mercury Switch)?",
    "id": "Saklar Tetesan Raksa (Sensor Kemiringan)."
  },
  {
    "en": "Apa Itu Saklar Mikro (Microswitch)?",
    "id": "Saklar Pemicu Cepat (Snap-Action)."
  },
  {
    "en": "Apa Itu Deringan Saklar (Switch Bouncing)?",
    "id": "Kontak Saklar Memantul (Getaran)."
  },
  {
    "en": "Apa Akibat Deringan Saklar (Switch Bouncing)?",
    "id": "Sinyal Input Digital Terbaca Ganda."
  },
  {
    "en": "Apa Itu Debouncing (Anti-Dering)?",
    "id": "Metode Menghilangkan Efek Bouncing."
  },
  {
    "en": "Apa Contoh Hardware Debouncing?",
    "id": "Menggunakan Rangkaian RC (Resistor-Capacitor)."
  },
  {
    "en": "Apa Contoh Software Debouncing?",
    "id": "Memberi Jeda Waktu (Delay) Setelah Input."
  },
  {
    "en": "Apa Itu Buffer (Penyangga) Logika?",
    "id": "Gerbang Logika (Output = Input)."
  },
  {
    "en": "Fungsi Buffer (Penyangga) Logika?",
    "id": "Menguatkan Arus (Meningkatkan Fan-Out)."
  },
  {
    "en": "Apa Itu Buffer Tri-State?",
    "id": "Buffer Dengan Output High-Z."
  },
  {
    "en": "Fungsi Buffer Tri-State?",
    "id": "Mengizinkan Banyak Perangkat Berbagi Bus."
  },
  {
    "en": "Apa Itu Trace (Jalur) PCB?",
    "id": "Jalur Tembaga Konduktif Pada PCB."
  },
  {
    "en": "Apa Itu Pad (Bantalan) PCB?",
    "id": "Titik Solder Kaki Komponen."
  },
  {
    "en": "Apa Itu Via (Lubang Tembus) PCB?",
    "id": "Lubang Penghubung Antar Lapisan PCB."
  },
  {
    "en": "Apa Itu Via Tipe Through-Hole?",
    "id": "Lubang Menembus Semua Lapisan."
  },
  {
    "en": "Apa Itu Via Tipe Blind (Buta)?",
    "id": "Lubang Lapisan Luar Ke Dalam."
  },
  {
    "en": "Apa Itu Via Tipe Buried (Terkubur)?",
    "id": "Lubang Antar Lapisan Dalam."
  },
  {
    "en": "Apa Itu Solder Mask (Masker Solder)?",
    "id": "Lapisan Pelindung Jalur (Hijau/Biru)."
  },
  {
    "en": "Fungsi Solder Mask (Masker Solder)?",
    "id": "Mencegah Hubung Singkat Saat Solder."
  },
  {
    "en": "Apa Itu Silkscreen (Sablon) PCB?",
    "id": "Teks Putih Penanda Komponen."
  },
  {
    "en": "Apa Itu Annular Ring (Cincin Anular)?",
    "id": "Area Tembaga Sekeliling Lubang Via."
  },
  {
    "en": "Apa Itu Lapisan (Layer) PCB?",
    "id": "Tumpukan Papan Konduktif."
  },
  {
    "en": "Apa Itu PCB Single-Layer?",
    "id": "Satu Lapisan Jalur Tembaga."
  },
  {
    "en": "Apa Itu PCB Double-Layer?",
    "id": "Dua Lapisan Jalur Tembaga."
  },
  {
    "en": "Apa Itu PCB Multi-Layer?",
    "id": "Lebih Dari Dua Lapisan Tembaga."
  },
  {
    "en": "Apa Itu Ground Plane (Bidang Pentanahan)?",
    "id": "Lapisan Tembaga Solid Untuk Ground."
  },
  {
    "en": "Keuntungan Ground Plane (Bidang Pentanahan)?",
    "id": "Mengurangi Noise, Impedansi Rendah."
  },
  {
    "en": "Apa Aksi Kontrol Proportional (P)?",
    "id": "Output Sebanding Nilai Error."
  },
  {
    "en": "Apa Itu Gain Proportional (Kp)?",
    "id": "Penguatan Aksi Proportional."
  },
  {
    "en": "Efek Gain Kp Terlalu Tinggi?",
    "id": "Respon Cepat, Osilasi, Tidak Stabil."
  },
  {
    "en": "Efek Gain Kp Terlalu Rendah?",
    "id": "Respon Lambat, Error Besar."
  },
  {
    "en": "Apa Aksi Kontrol Integral (I)?",
    "id": "Output Berdasar Akumulasi Error."
  },
  {
    "en": "Apa Fungsi Aksi Integral (I)?",
    "id": "Menghilangkan Error Steady-State."
  },
  {
    "en": "Apa Itu Gain Integral (Ki)?",
    "id": "Penguatan Aksi Integral."
  },
  {
    "en": "Apa Itu Integral Windup?",
    "id": "Akumulasi Error Berlebih Saat Saturasi."
  },
  {
    "en": "Cara Mencegah Integral Windup?",
    "id": "Anti-Windup, Clamping Integral."
  },
  {
    "en": "Apa Aksi Kontrol Derivative (D)?",
    "id": "Output Berdasar Laju Perubahan Error."
  },
  {
    "en": "Apa Fungsi Aksi Derivative (D)?",
    "id": "Meredam Osilasi, Memprediksi Error."
  },
  {
    "en": "Apa Itu Gain Derivative (Kd)?",
    "id": "Penguatan Aksi Derivative."
  },
  {
    "en": "Kelemahan Aksi Derivative (D)?",
    "id": "Sangat Sensitif Terhadap Noise Input."
  },
  {
    "en": "Apa Itu Filter Derivative?",
    "id": "Filter LPF Meredam Noise Aksi D."
  },
  {
    "en": "Apa Itu Kontrol ON-OFF (Bang-Bang)?",
    "id": "Kontrol Dua Posisi (Aktif/Mati)."
  },
  {
    "en": "Contoh Kontrol ON-OFF?",
    "id": "Termostat AC, Setrika Listrik."
  },
  {
    "en": "Apa Kepanjangan ESR (Equivalent Series Resistance)?",
    "id": "Equivalent Series Resistance."
  },
  {
    "en": "Apa Itu ESR (Equivalent Series Resistance) Kapasitor?",
    "id": "Hambatan Internal Non-Ideal Kapasitor."
  },
  {
    "en": "Mengapa ESR (Equivalent Series Resistance) Kapasitor Penting?",
    "id": "Menentukan Disipasi Panas, Efisiensi."
  },
  {
    "en": "Apa Kepanjangan ESL (Equivalent Series Inductance)?",
    "id": "Equivalent Series Inductance."
  },
  {
    "en": "Apa Itu ESL (Equivalent Series Inductance) Kapasitor?",
    "id": "Induktansi Parasitik Internal Kapasitor."
  },
  {
    "en": "Kapan ESL (Equivalent Series Inductance) Berpengaruh?",
    "id": "Pada Frekuensi Sangat Tinggi (RF)."
  },
  {
    "en": "Apa Itu Induktansi Kebocoran (Leakage Inductance)?",
    "id": "Fluks Magnetik Trafo Tidak Terkopling."
  },
  {
    "en": "Apa Itu Induktansi Mutual (Mutual Inductance)?",
    "id": "Induksi GGL Antar Dua Kumparan."
  },
  {
    "en": "Apa Itu Faktor Kopling (Coupling Factor) (k)?",
    "id": "Ukuran Kopling Magnetik Trafo."
  },
  {
    "en": "Apa Nilai k Untuk Trafo Ideal?",
    "id": "Satu (Kopling Sempurna)."
  },
  {
    "en": "Apa Kepanjangan TCR (Temperature Coefficient of Resistance)?",
    "id": "Temperature Coefficient of Resistance."
  },
  {
    "en": "Apa Arti TCR (Temperature Coefficient) Positif (PTC)?",
    "id": "Hambatan Naik Saat Suhu Naik."
  },
  {
    "en": "Apa Arti TCR (Temperature Coefficient) Negatif (NTC)?",
    "id": "Hambatan Turun Saat Suhu Naik."
  },
  {
    "en": "Apa Itu Resistor Nol Ohm (Zero Ohm)?",
    "id": "Resistor Hambatan Nol (Jumper)."
  },
  {
    "en": "Fungsi Resistor Nol Ohm (Zero Ohm)?",
    "id": "Jumper Pada PCB Otomatis."
  },
  {
    "en": "Apa Itu Jaringan Resistor (Resistor Network)?",
    "id": "Beberapa Resistor Dalam Satu Kemasan."
  },
  {
    "en": "Apa Kepanjangan SPL (Sound Pressure Level)?",
    "id": "Sound Pressure Level."
  },
  {
    "en": "Apa Satuan SPL (Sound Pressure Level)?",
    "id": "Desibel (dB SPL)."
  },
  {
    "en": "Apa Ambang Batas Pendengaran Manusia (SPL)?",
    "id": "Nol dB SPL."
  },
  {
    "en": "Apa Ambang Batas Rasa Sakit (SPL)?",
    "id": "Sekitar 120-130 dB SPL."
  },
  {
    "en": "Apa Itu Impedansi Input (Audio)?",
    "id": "Hambatan Input Perangkat Audio."
  },
  {
    "en": "Apa Itu Impedansi Output (Audio)?",
    "id": "Hambatan Output Perangkat Audio."
  },
  {
    "en": "Apa Itu Penjembatan Impedansi (Impedance Bridging)?",
    "id": "Impedansi Input Jauh Lebih Tinggi Output."
  },
  {
    "en": "Mengapa Perlu Impedansi Bridging (Audio)?",
    "id": "Mencegah Kehilangan Sinyal Tegangan."
  },
  {
    "en": "Apa Itu Pencocokan Impedansi (Impedance Matching) Audio?",
    "id": "Impedansi Input Sama Dengan Output."
  },
  {
    "en": "Kapan Pencocokan Impedansi (Audio) Digunakan?",
    "id": "Transfer Daya Maksimal (Speaker)."
  },
  {
    "en": "Apa Itu Equalizer (EQ) Grafis?",
    "id": "EQ Pengatur Frekuensi Slider."
  },
  {
    "en": "Apa Itu Equalizer (EQ) Parametrik?",
    "id": "EQ Pengatur Frekuensi, Gain, Q."
  },
  {
    "en": "Apa Itu Faktor Q (Quality Factor) EQ?",
    "id": "Lebar Pita (Bandwidth) Filter EQ."
  },
  {
    "en": "Apa Itu Filter High-Pass (Audio)?",
    "id": "Memotong Frekuensi Rendah (Bass)."
  },
  {
    "en": "Apa Itu Filter Low-Pass (Audio)?",
    "id": "Memotong Frekuensi Tinggi (Treble)."
  },
  {
    "en": "Apa Itu Filter Notch (Pelemah)?",
    "id": "Memotong Satu Frekuensi Sangat Sempit."
  },
  {
    "en": "Apa Itu Sinyal Mic Level?",
    "id": "Sinyal Tegangan Sangat Rendah (Mikrofon)."
  },
  {
    "en": "Apa Itu Sinyal Line Level?",
    "id": "Sinyal Standar Antar Perangkat Audio."
  },
  {
    "en": "Apa Itu Sinyal Instrument Level?",
    "id": "Sinyal Output Gitar/Bass Listrik."
  },
  {
    "en": "Apa Itu Penguat Headroom (Audio)?",
    "id": "Ruang Sinyal Sebelum Terjadi Kliping."
  },
  {
    "en": "Apa Itu Kliping (Clipping) Sinyal Audio?",
    "id": "Distorsi Pemotongan Puncak Sinyal."
  },
  {
    "en": "Apa Kepanjangan MLCC (Multi-Layer Ceramic Capacitor)?",
    "id": "Multi-Layer Ceramic Capacitor."
  },
  {
    "en": "Apa Keunggulan MLCC (Multi-Layer Ceramic Capacitor)?",
    "id": "Ukuran Kecil, Kapasitansi Tinggi."
  },
  {
    "en": "Apa Itu Efek Mikrofonik (Microphonic Effect)?",
    "id": "Komponen Hasilkan Noise Akibat Getaran."
  },
  {
    "en": "Kapasitor Apa Yang Rentan Efek Mikrofonik?",
    "id": "Kapasitor Keramik (MLCC)."
  },
  {
    "en": "Apa Itu Arus Gelap (Dark Current)?",
    "id": "Arus Bocor Fotodioda Saat Gelap."
  },
  {
    "en": "Apa Itu Responsivitas Fotodioda?",
    "id": "Rasio Arus Foto Terhadap Daya Cahaya."
  },
  {
    "en": "Apa Itu Dioda Avalanche (Longsoran)?",
    "id": "Dioda Tembus Terkontrol (Efek Longsoran)."
  },
  {
    "en": "Perbedaan Zener Dan Avalanche Breakdown?",
    "id": "Zener (Medan Kuat), Avalanche (Tumbukan)."
  },
  {
    "en": "Apa Kepanjangan APD (Avalanche Photodiode)?",
    "id": "Avalanche Photodiode."
  },
  {
    "en": "Fungsi APD (Avalanche Photodiode)?",
    "id": "Fotodioda Sangat Sensitif (Ada Gain Internal)."
  },
  {
    "en": "Apa Itu Koherensi (Coherence) Cahaya?",
    "id": "Sifat Gelombang Cahaya Sefasa."
  },
  {
    "en": "Apa Itu Emisi Spontan (Spontaneous Emission)?",
    "id": "Pancaran Foton Acak (Seperti LED)."
  },
  {
    "en": "Apa Itu Emisi Terstimulasi (Stimulated Emission)?",
    "id": "Pancaran Foton Terpicu (Dasar Laser)."
  },
  {
    "en": "Apa Itu Injeksi Pembawa (Carrier Injection)?",
    "id": "Proses Pembangkitan Elektron-Hole."
  },
  {
    "en": "Apa Itu Catu Daya Dual (Dual Supply)?",
    "id": "Catu Daya Tegangan Positif Negatif."
  },
  {
    "en": "Kapan Catu Daya Dual Dibutuhkan?",
    "id": "Operasi Op-Amp, Sinyal AC Audio."
  },
  {
    "en": "Apa Itu Virtual Ground (Op-Amp) Rail-Splitter?",
    "id": "Membuat Ground Maya Dari Catu Tunggal."
  },
  {
    "en": "Apa Itu Rangkaian Clamper (Penjepit)?",
    "id": "Menggeser Level DC Sinyal AC."
  },
  {
    "en": "Apa Itu Rangkaian Clipper (Pemotong)?",
    "id": "Memotong Bagian Sinyal Di Atas/Bawah Level."
  },
  {
    "en": "Apa Itu Dioda Clamper Positif?",
    "id": "Menggeser Sinyal Ke Atas Level Nol."
  },
  {
    "en": "Apa Itu Dioda Clipper Paralel?",
    "id": "Memotong Amplitudo Sinyal."
  },
  {
    "en": "Apa Itu Osilator Relaksasi (Relaxation Oscillator)?",
    "id": "Osilator Berbasis Pengisian Kapasitor."
  },
  {
    "en": "Contoh Osilator Relaksasi?",
    "id": "IC 555 Mode Astable, UJT."
  },
  {
    "en": "Apa Itu Analisis Titik Operasi (DC Analysis)?",
    "id": "Analisis Rangkaian Pada Kondisi DC."
  },
  {
    "en": "Apa Itu Analisis Sapuan DC (DC Sweep)?",
    "id": "Analisis Respon Terhadap Perubahan DC."
  },
  {
    "en": "Apa Itu Analisis Transien (Transient Analysis)?",
    "id": "Analisis Respon Rangkaian Terhadap Waktu."
  },
  {
    "en": "Apa Itu Analisis AC (AC Analysis)?",
    "id": "Analisis Respon Rangkaian Terhadap Frekuensi."
  },
  {
    "en": "Apa Itu Analisis Noise (Noise Analysis)?",
    "id": "Analisis Noise Internal Rangkaian."
  },
  {
    "en": "Apa Itu Logika Kombinasional Asinkron?",
    "id": "Output Berubah Langsung (Tanpa Clock)."
  },
  {
    "en": "Apa Itu Logika Sekuensial Asinkron?",
    "id": "Perubahan State Tergantung Input."
  },
  {
    "en": "Apa Itu Logika Sekuensial Sinkron?",
    "id": "Perubahan State Disinkronkan Clock."
  },
  {
    "en": "Apa Itu Bahaya (Hazard) Logika Statis?",
    "id": "Glitch Sesaat Pada Output Statis."
  },
  {
    "en": "Apa Itu Bahaya (Hazard) Statis-1?",
    "id": "Output 1 Menjadi 0 Sesaat."
  },
  {
    "en": "Apa Itu Bahaya (Hazard) Statis-0?",
    "id": "Output 0 Menjadi 1 Sesaat."
  },
  {
    "en": "Apa Itu Bahaya (Hazard) Logika Dinamis?",
    "id": "Glitch Sesaat Saat Transisi Output."
  },
  {
    "en": "Bagaimana Mencegah Bahaya (Hazard) Logika?",
    "id": "Menambahkan Term Redundan (K-Map)."
  },
  {
    "en": "Apa Itu Kondisi Berpacu (Race Condition)?",
    "id": "Output Tergantung Urutan Sinyal Tiba."
  },
  {
    "en": "Apa Itu Siklus (Cycle) Kritis (Critical Race)?",
    "id": "Race Condition Penyebab State Akhir Salah."
  },
  {
    "en": "Apa Arsitektur PLA (Programmable Logic Array)?",
    "id": "Gerbang AND Dan OR Terprogram."
  },
  {
    "en": "Apa Arsitektur PAL (Programmable Array Logic)?",
    "id": "Gerbang AND Terprogram, OR Tetap."
  },
  {
    "en": "Apa Arsitektur GAL (Generic Array Logic)?",
    "id": "Varian PAL Yang Dapat Dihapus (EEPROM)."
  },
  {
    "en": "Apa Kepanjangan CPLD (Complex Programmable Logic Device)?",
    "id": "Complex Programmable Logic Device."
  },
  {
    "en": "Apa Kepanjangan FPGA (Field Programmable Gate Array)?",
    "id": "Field Programmable Gate Array."
  },
  {
    "en": "Apa Blok Bangunan Dasar FPGA?",
    "id": "Logic Element (LE) Atau CLB."
  },
  {
    "en": "Apa Kepanjangan CLB (Configurable Logic Block)?",
    "id": "Configurable Logic Block."
  },
  {
    "en": "Apa Isi CLB (Configurable Logic Block)?",
    "id": "LUT (Look-Up Table) Dan Flip-Flop."
  },
  {
    "en": "Apa Kepanjangan LUT (Look-Up Table)?",
    "id": "Look-Up Table (Tabel Pencarian)."
  },
  {
    "en": "Apa Fungsi LUT (Look-Up Table)?",
    "id": "Implementasi Fungsi Logika Kombinasional."
  },
  {
    "en": "Apa Kepanjangan VHDL (VHSIC Hardware Description Language)?",
    "id": "VHSIC Hardware Description Language."
  },
  {
    "en": "Apa Itu Verilog?",
    "id": "Bahasa Deskripsi Hardware (Lainnya)."
  },
  {
    "en": "Apa Itu Sintesis (Synthesis) HDL?",
    "id": "Mengubah Kode HDL Menjadi Rangkaian Logika."
  },
  {
    "en": "Apa Itu Simulasi (Simulation) HDL?",
    "id": "Verifikasi Fungsionalitas Desain Digital."
  },
  {
    "en": "Apa Itu Kerugian Konduksi (Conduction Loss)?",
    "id": "Kerugian Daya Saat Saklar ON."
  },
  {
    "en": "Apa Itu Kerugian Switching (Switching Loss)?",
    "id": "Kerugian Daya Saat Transisi ON-OFF."
  },
  {
    "en": "Kapan Kerugian Switching (Switching Loss) Dominan?",
    "id": "Pada Frekuensi Switching Tinggi."
  },
  {
    "en": "Apa Itu Switching (Mode) Keras (Hard Switching)?",
    "id": "Saklar Beralih Saat Tegangan Arus Tinggi."
  },
  {
    "en": "Apa Itu Switching (Mode) Lunak (Soft Switching)?",
    "id": "Saklar Beralih Saat Tegangan/Arus Nol."
  },
  {
    "en": "Apa Kepanjangan ZVS (Zero Voltage Switching)?",
    "id": "Zero Voltage Switching (Tegangan Nol)."
  },
  {
    "en": "Apa Kepanjangan ZCS (Zero Current Switching)?",
    "id": "Zero Current Switching (Arus Nol)."
  },
  {
    "en": "Keuntungan Switching (Mode) Lunak (Soft Switching)?",
    "id": "Mengurangi Kerugian Switching, Efisiensi Tinggi."
  },
  {
    "en": "Apa Itu Topologi Push-Pull SMPS?",
    "id": "Konverter Isolasi Dua Saklar."
  },
  {
    "en": "Apa Itu Topologi Jembatan (Bridge) Penuh?",
    "id": "Konverter Isolasi Empat Saklar."
  },
  {
    "en": "Apa Itu Topologi Jembatan (Bridge) Setengah?",
    "id": "Konverter Isolasi Dua Saklar, Dua Kapasitor."
  },
  {
    "en": "Apa Kepanjangan V/f (Voltage/Frequency) Kontrol Motor?",
    "id": "Voltage Per Hertz Control."
  },
  {
    "en": "Apa Prinsip Kontrol V/f (Voltage/Frequency)?",
    "id": "Menjaga Rasio Tegangan Frekuensi Konstan."
  },
  {
    "en": "Apa Kepanjangan FOC (Field Oriented Control)?",
    "id": "Field Oriented Control (Kontrol Vektor)."
  },
  {
    "en": "Apa Keunggulan FOC (Field Oriented Control)?",
    "id": "Respon Torsi Cepat Presisi Tinggi."
  },
  {
    "en": "Apa Kepanjangan DTC (Direct Torque Control)?",
    "id": "Direct Torque Control."
  },
  {
    "en": "Apa Itu Kontrol Sinyal Analog?",
    "id": "Kontrol Berbasis Sinyal Kontinu."
  },
  {
    "en": "Apa Itu Kontrol Sinyal Digital?",
    "id": "Kontrol Berbasis Sinyal Diskret."
  },
  {
    "en": "Apa Kepanjangan PLC (Programmable Logic Controller)?",
    "id": "Programmable Logic Controller."
  },
  {
    "en": "Apa Fungsi PLC (Programmable Logic Controller)?",
    "id": "Kontroler Otomasi Industri Terprogram."
  },
  {
    "en": "Apa Bahasa Pemrograman PLC Utama?",
    "id": "Ladder Logic (Logika Tangga)."
  },
  {
    "en": "Apa Kepanjangan DCS (Distributed Control System)?",
    "id": "Distributed Control System."
  },
  {
    "en": "Kapan DCS (Distributed Control System) Digunakan?",
    "id": "Kontrol Proses Skala Besar (Pabrik)."
  },
  {
    "en": "Apa Kepanjangan DAS (Data Acquisition System)?",
    "id": "Data Acquisition System (Akuisisi Data)."
  },
  {
    "en": "Komponen Utama DAS (Data Acquisition System)?",
    "id": "Sensor, ADC, Komputer."
  },
  {
    "en": "Apa Itu Resolusi ADC (Analog to Digital Converter)?",
    "id": "Jumlah Bit Output Digital (Presisi)."
  },
  {
    "en": "Apa Itu Laju Sampling (Sample Rate) ADC?",
    "id": "Seberapa Cepat Konversi Dilakukan."
  },
  {
    "en": "Apa Itu Rangkaian Sample and Hold (S/H)?",
    "id": "Menahan Tegangan Analog Tetap Saat Konversi."
  },
  {
    "en": "Apa Itu Sinyal Modulasi PAM (Pulse Amplitude Modulation)?",
    "id": "Amplitudo Pulsa Bervariasi (Informasi)."
  },
  {
    "en": "Apa Itu Sinyal Modulasi PPM (Pulse Position Modulation)?",
    "id": "Posisi Pulsa Bervariasi (Informasi)."
  },
  {
    "en": "Apa Itu Sinyal Modulasi PDM (Pulse Density Modulation)?",
    "id": "Kepadatan Pulsa Bervariasi (Informasi)."
  },
  {
    "en": "Apa Itu Resistansi Radiasi Antena?",
    "id": "Resistansi Ekuivalen Pemancar Daya Antena."
  },
  {
    "en": "Apa Itu Efisiensi Antena?",
    "id": "Rasio Daya Radiasi Terhadap Input."
  },
  {
    "en": "Apa Itu Resistansi Rugi (Loss Resistance) Antena?",
    "id": "Kerugian Ohm (Panas) Pada Antena."
  },
  {
    "en": "Apa Itu Lebar Pancaran (Beamwidth) Setengah Daya?",
    "id": "Sudut Pancaran (Turun -3dB)."
  },
  {
    "en": "Apa Itu Null (Nol) Pola Radiasi?",
    "id": "Arah Tanpa Pancaran Sinyal."
  },
  {
    "en": "Apa Itu Balun (Balanced-to-Unbalanced)?",
    "id": "Penghubung Saluran Seimbang Ke Tak Seimbang."
  },
  {
    "en": "Kapan Balun (Balanced-to-Unbalanced) Dibutuhkan?",
    "id": "Menghubungkan Kabel Coax Ke Antena Dipole."
  },
  {
    "en": "Apa Itu Antena Loop (Loop Antenna)?",
    "id": "Antena Berbentuk Kumparan (Loop)."
  },
  {
    "en": "Apa Itu Antena Loop Kecil?",
    "id": "Sensitif Terhadap Medan Magnet (AM)."
  },
  {
    "en": "Apa Itu Antena Heliks (Helical Antenna)?",
    "id": "Antena Berbentuk Spiral (Polarisasi Sirkular)."
  },
  {
    "en": "Apa Itu Polarisasi Sirkular Antena?",
    "id": "Vektor Medan Listrik Berputar."
  },
  {
    "en": "Apa Itu Rangkaian Catu Daya Tak Diatur?",
    "id": "Catu Daya Tanpa Regulator (Trafo, Dioda)."
  },
  {
    "en": "Apa Itu Riak (Ripple) Tegangan?",
    "id": "Fluktuasi AC Sisa Pada Output DC."
  },
  {
    "en": "Bagaimana Mengurangi Riak (Ripple) Tegangan?",
    "id": "Meningkatkan Nilai Kapasitor Filter."
  },
  {
    "en": "Apa Itu Faktor Regulasi Beban (Load Regulation)?",
    "id": "Perubahan Tegangan Output Akibat Beban."
  },
  {
    "en": "Apa Itu Faktor Regulasi Saluran (Line Regulation)?",
    "id": "Perubahan Output Akibat Input Berubah."
  },
  {
    "en": "Apa Itu IC Regulator Tegangan Tetap?",
    "id": "Menghasilkan Tegangan Output Konstan (78xx)."
  },
  {
    "en": "Apa Itu IC Regulator Tegangan Variabel?",
    "id": "Tegangan Output Dapat Diatur (LM317)."
  },
  {
    "en": "Apa Itu Dioda Schottky?",
    "id": "Dioda Sambungan Logam-Semikonduktor."
  },
  {
    "en": "Apa Keunggulan Dioda Schottky?",
    "id": "Tegangan Maju Rendah, Switching Cepat."
  },
  {
    "en": "Aplikasi Dioda Schottky?",
    "id": "Catu Daya Switching (SMPS), Penyearah."
  },
  {
    "en": "Apa Itu Dioda Varactor (Varicap)?",
    "id": "Dioda Sebagai Kapasitor Terkendali Tegangan."
  },
  {
    "en": "Aplikasi Dioda Varactor?",
    "id": "Rangkaian Tuning (VCO, PLL), Filter."
  },
  {
    "en": "Apa Itu Dioda Terowongan (Tunnel Diode)?",
    "id": "Dioda Dengan Resistansi Diferensial Negatif."
  },
  {
    "en": "Aplikasi Dioda Terowongan?",
    "id": "Osilator Frekuensi Sangat Tinggi."
  },
  {
    "en": "Apa Itu Dioda IMPATT?",
    "id": "Dioda Osilator Microwave (Efek Avalanche)."
  },
  {
    "en": "Apa Itu Dioda Gunn?",
    "id": "Dioda Osilator Microwave (Efek Gunn)."
  },
  {
    "en": "Apa Itu Dioda PIN?",
    "id": "Dioda (P-Intrinsic-N)."
  },
  {
    "en": "Aplikasi Dioda PIN?",
    "id": "Saklar RF, Atenuator, Fotodetektor."
  },
  {
    "en": "Apa Itu Fotodetektor?",
    "id": "Sensor Pengubah Cahaya Menjadi Listrik."
  },
  {
    "en": "Apa Itu Sel Surya (Photovoltaic Cell)?",
    "id": "Fotodioda Mode Photovoltaic."
  },
  {
    "en": "Apa Mode Operasi Fotodioda?",
    "id": "Photoconductive Dan Photovoltaic."
  },
  {
    "en": "Apa Itu Mode Photoconductive?",
    "id": "Fotodioda Diberi Reverse Bias (Respon Cepat)."
  },
  {
    "en": "Apa Itu Mode Photovoltaic?",
    "id": "Fotodioda Hasilkan Tegangan (Tanpa Bias)."
  },
  {
    "en": "Apa Itu Kapasitor Elektrolit (Elco)?",
    "id": "Kapasitor Polar Nilai Kapasitansi Tinggi."
  },
  {
    "en": "Apa Itu Kapasitor Tantalum?",
    "id": "Kapasitor Elektrolit Ukuran Kecil."
  },
  {
    "en": "Apa Itu Kapasitor Film Poliester (Mylar)?",
    "id": "Kapasitor Non-Polar Umum."
  },
  {
    "en": "Apa Itu Kapasitor Film Polipropilena?",
    "id": "Kapasitor Presisi (Audio, Filter)."
  },
  {
    "en": "Apa Itu Kapasitor Keramik Tipe 1 (NP0)?",
    "id": "Sangat Stabil Terhadap Suhu."
  },
  {
    "en": "Apa Itu Kapasitor Keramik Tipe 2 (X7R)?",
    "id": "Kapasitansi Tinggi, Kurang Stabil."
  },
  {
    "en": "Apa Itu Kode Tiga Digit Kapasitor SMD?",
    "id": "Dua Digit Nilai, Satu Digit Multiplier."
  },
  {
    "en": "Apa Itu Kode EIA (Electronic Industries Alliance) Resistor SMD?",
    "id": "Kode Tiga Atau Empat Digit."
  },
  {
    "en": "Apa Itu Penyearah Setengah Gelombang (Half-Wave)?",
    "id": "Rangkaian Penyearah Satu Dioda."
  },
  {
    "en": "Apa Itu Penyearah Jembatan (Bridge Rectifier)?",
    "id": "Rangkaian Penyearah Empat Dioda."
  },
  {
    "en": "Keuntungan Penyearah Jembatan (Bridge)?",
    "id": "Efisiensi Ganda, Riak (Ripple) Lebih Kecil."
  },
  {
    "en": "Apa Itu Arus Drift (Drift Current)?",
    "id": "Aliran Muatan Akibat Medan Listrik."
  },
  {
    "en": "Apa Itu Arus Difusi (Diffusion Current)?",
    "id": "Aliran Muatan Akibat Gradien Konsentrasi."
  },
  {
    "en": "Apa Itu Mobilitas (Mobility) Pembawa Muatan?",
    "id": "Kemudahan Pembawa Muatan Bergerak."
  },
  {
    "en": "Mana Mobilitas Lebih Tinggi, Elektron Atau Hole?",
    "id": "Elektron."
  },
  {
    "en": "Apa Itu Kecepatan Drift (Drift Velocity)?",
    "id": "Kecepatan Rata-Rata Muatan (Medan Listrik)."
  },
  {
    "en": "Apa Itu Level Energi Fermi (Fermi Level)?",
    "id": "Level Energi Probabilitas Setengah."
  },
  {
    "en": "Apa Itu Rekombinasi (Recombination) Langsung?",
    "id": "Elektron Lubang Bertemu Lepas Foton."
  },
  {
    "en": "Apa Itu Rekombinasi (Recombination) Tidak Langsung?",
    "id": "Rekombinasi Melalui Perangkap (Trap)."
  },
  {
    "en": "Apa Itu Generasi (Generation) Pembawa Muatan?",
    "id": "Penciptaan Pasangan Elektron Lubang."
  },
  {
    "en": "Apa Itu Waktu Hidup (Lifetime) Pembawa Muatan?",
    "id": "Waktu Rata-Rata Sebelum Rekombinasi."
  },
  {
    "en": "Apa Itu Rangkaian Gate Drive?",
    "id": "Rangkaian Pengemudi Gerbang (Gate) Transistor."
  },
  {
    "en": "Mengapa MOSFET (Metal Oxide Semiconductor FET) Perlu Gate Drive?",
    "id": "Mengisi Kapasitansi Gerbang."
  },
  {
    "en": "Apa Itu Kapasitansi Input (Ciss) MOSFET?",
    "id": "Total Kapasitansi Gerbang (Cgs + Cgd)."
  },
  {
    "en": "Apa Itu Kapasitansi Miller (Cgd) MOSFET?",
    "id": "Kapasitansi Antara Gerbang (Gate) Dan Drain."
  },
  {
    "en": "Apa Itu Miller Plateau (Dataran Miller)?",
    "id": "Tegangan Gerbang Datar Saat Switching."
  },
  {
    "en": "Apa Itu Driver Sisi Rendah (Low-Side Driver)?",
    "id": "Driver Saklar Yang Terhubung Ground."
  },
  {
    "en": "Apa Itu Driver Sisi Tinggi (High-Side Driver)?",
    "id": "Driver Saklar Yang Terhubung Beban."
  },
  {
    "en": "Apa Itu Rangkaian Bootstrap (Bootstrap Circuit)?",
    "id": "Catu Daya Mengambang Driver High-Side."
  },
  {
    "en": "Apa Itu Level Shifter (Penggeser Level)?",
    "id": "Mengubah Level Tegangan Sinyal Kontrol."
  },
  {
    "en": "Apa Itu Waktu Mati (Dead-Time)?",
    "id": "Jeda Waktu Antara Dua Saklar Bridge."
  },
  {
    "en": "Mengapa Perlu Waktu Mati (Dead-Time)?",
    "id": "Mencegah Shoot-Through."
  },
  {
    "en": "Apa Itu Shoot-Through (Arus Tembak)?",
    "id": "Kondisi Dua Saklar Bridge ON Bersamaan."
  },
  {
    "en": "Apa Akibat Shoot-Through?",
    "id": "Hubung Singkat Catu Daya, Kerusakan."
  },
  {
    "en": "Apa Itu Waktu Naik (Rise Time)?",
    "id": "Waktu Sinyal Naik (10% Ke 90%)."
  },
  {
    "en": "Apa Itu Waktu Turun (Fall Time)?",
    "id": "Waktu Sinyal Turun (90% Ke 10%)."
  },
  {
    "en": "Apa Itu Respon Transien (Transient Response)?",
    "id": "Respon Sesaat Rangkaian."
  },
  {
    "en": "Apa Itu Respon Keadaan Tunak (Steady-State)?",
    "id": "Respon Rangkaian Setelah Waktu Lama."
  },
  {
    "en": "Apa Itu Respon Alami (Natural Response)?",
    "id": "Respon Rangkaian Tanpa Sumber Eksternal."
  },
  {
    "en": "Apa Itu Respon Paksa (Forced Response)?",
    "id": "Respon Rangkaian Akibat Sumber Eksternal."
  },
  {
    "en": "Apa Itu Respon Lengkap (Complete Response)?",
    "id": "Jumlah Respon Alami Dan Paksa."
  },
  {
    "en": "Apa Itu Fungsi Step (Tangga) Unit?",
    "id": "Sinyal Nol (t<0), Satu (t>0)."
  },
  {
    "en": "Apa Itu Respon Step (Step Response)?",
    "id": "Respon Rangkaian Terhadap Input Step."
  },
  {
    "en": "Apa Itu Fungsi Impuls (Impulse Function) Unit?",
    "id": "Pulsa Durasi Singkat Luas Satu."
  },
  {
    "en": "Apa Itu Respon Impuls (Impulse Response)?",
    "id": "Respon Rangkaian Terhadap Input Impuls."
  },
  {
    "en": "Apa Itu Rangkaian Orde Pertama (First-Order)?",
    "id": "Rangkaian Satu Komponen Penyimpan Energi."
  },
  {
    "en": "Contoh Rangkaian Orde Pertama?",
    "id": "Rangkaian RC Atau RL."
  },
  {
    "en": "Apa Itu Rangkaian Orde Kedua (Second-Order)?",
    "id": "Rangkaian Dua Komponen Penyimpan Energi."
  },
  {
    "en": "Contoh Rangkaian Orde Kedua?",
    "id": "Rangkaian RLC."
  },
  {
    "en": "Apa Itu Respon Overdamped (Redaman Berlebih)?",
    "id": "Respon Lambat Tanpa Osilasi."
  },
  {
    "en": "Apa Itu Respon Critically Damped (Redaman Kritis)?",
    "id": "Respon Tercepat Tanpa Osilasi."
  },
  {
    "en": "Apa Itu Respon Underdamped (Kurang Redam)?",
    "id": "Respon Cepat Dengan Osilasi."
  },
  {
    "en": "Apa Itu Frekuensi Alami (Natural Frequency) (ω0)?",
    "id": "Frekuensi Osilasi Rangkaian LC Ideal."
  },
  {
    "en": "Apa Itu Faktor Redaman (Damping Factor) (α)?",
    "id": "Ukuran Redaman Rangkaian RLC."
  },
  {
    "en": "Apa Itu Bilangan Kompleks (Complex Numbers)?",
    "id": "Bilangan (A + jB)."
  },
  {
    "en": "Apa Itu Bagian Real (Bilangan Kompleks)?",
    "id": "Bagian (A) Sumbu Horizontal."
  },
  {
    "en": "Apa Itu Bagian Imajiner (Bilangan Kompleks)?",
    "id": "Bagian (jB) Sumbu Vertikal."
  },
  {
    "en": "Apa Itu Operator 'j' (J Operator)?",
    "id": "Akar Kuadrat Dari Minus Satu."
  },
  {
    "en": "Mengapa Teknik Elektro Menggunakan 'j' Bukan 'i'? ",
    "id": "Simbol 'i' Dipakai Untuk Arus Listrik."
  },
  {
    "en": "Apa Itu Bentuk Polar (Bilangan Kompleks)?",
    "id": "Representasi Magnitudo Dan Sudut (Fasor)."
  },
  {
    "en": "Apa Itu Bentuk Rectangular (Bilangan Kompleks)?",
    "id": "Representasi Real Dan Imajiner (A + jB)."
  },
  {
    "en": "Apa Itu Konjugasi Kompleks (Complex Conjugate)?",
    "id": "Mengubah Tanda Bagian Imajiner (A - jB)."
  },
  {
    "en": "Apa Itu Admitansi (Y)?",
    "id": "Kebalikan Dari Impedansi (Y = 1/Z)."
  },
  {
    "en": "Apa Satuan Admitansi (Y)?",
    "id": "Siemens (S) Atau Mho."
  },
  {
    "en": "Apa Itu Suseptansi (B)?",
    "id": "Bagian Imajiner Dari Admitansi."
  },
  {
    "en": "Apa Itu Konduktansi (G)?",
    "id": "Bagian Real Dari Admitansi."
  },
  {
    "en": "Apa Itu Segitiga Impedansi (Impedance Triangle)?",
    "id": "Hubungan Grafis Antara R, X, Z."
  },
  {
    "en": "Apa Rumus Teorema Pythagoras Impedansi?",
    "id": "Z Kuadrat = R Kuadrat + X Kuadrat."
  },
  {
    "en": "Apa Itu Sensor Tekanan Piezoresistif?",
    "id": "Hambatan Berubah Akibat Tekanan."
  },
  {
    "en": "Apa Itu Sensor Tekanan Kapasitif?",
    "id": "Kapasitansi Berubah Akibat Tekanan."
  },
  {
    "en": "Apa Itu Sensor Tekanan Strain Gauge?",
    "id": "Regangan Material Mengubah Hambatan."
  },
  {
    "en": "Apa Itu Tekanan Absolut (Absolute Pressure)?",
    "id": "Tekanan Referensi Vakum Sempurna."
  },
  {
    "en": "Apa Itu Tekanan Gauge (Gauge Pressure)?",
    "id": "Tekanan Referensi Tekanan Atmosfer."
  },
  {
    "en": "Apa Itu Tekanan Diferensial (Differential Pressure)?",
    "id": "Selisih Antara Dua Titik Tekanan."
  },
  {
    "en": "Apa Itu Sensor Aliran (Flow Sensor) Thermal?",
    "id": "Mengukur Aliran Berbasis Transfer Panas."
  },
  {
    "en": "Apa Itu Sensor Aliran (Flow Sensor) Turbin?",
    "id": "Mengukur Aliran Berbasis Putaran Turbin."
  },
  {
    "en": "Apa Itu Sensor Aliran (Flow Sensor) Ultrasonik?",
    "id": "Mengukur Aliran Berbasis Waktu Tempuh Suara."
  },
  {
    "en": "Apa Itu Sensor Aliran (Flow Sensor) Elektromagnetik?",
    "id": "Mengukur Aliran Fluida Konduktif."
  },
  {
    "en": "Apa Itu Sensor Level (Level Sensor) Kapasitif?",
    "id": "Mengukur Level Berbasis Perubahan Kapasitansi."
  },
  {
    "en": "Apa Itu Sensor Level (Level Sensor) Ultrasonik?",
    "id": "Mengukur Level Berbasis Pantulan Suara."
  },
  {
    "en": "Apa Itu Sensor Level (Level Sensor) Pelampung (Float)?",
    "id": "Sensor Level Mekanis (Kontak Magnetik)."
  },
  {
    "en": "Apa Bahan Anoda Baterai Aki (Lead-Acid)?",
    "id": "Timbal (Pb)."
  },
  {
    "en": "Apa Bahan Katoda Baterai Aki (Lead-Acid)?",
    "id": "Timbal Dioksida (PbO2)."
  },
  {
    "en": "Apa Bahan Elektrolit Baterai Aki?",
    "id": "Asam Sulfat (H2SO4)."
  },
  {
    "en": "Apa Bahan Anoda Baterai Li-Ion?",
    "id": "Grafit (Karbon)."
  },
  {
    "en": "Apa Bahan Katoda Baterai Li-Ion?",
    "id": "Oksida Logam Lithium."
  },
  {
    "en": "Apa Bahan Elektrolit Baterai Li-Ion?",
    "id": "Garam Lithium Pelarut Organik."
  },
  {
    "en": "Apa Kepanjangan Baterai LFP (Lithium Ferro Phosphate)?",
    "id": "Lithium Ferro Phosphate."
  },
  {
    "en": "Keunggulan Baterai LFP (Lithium Ferro Phosphate)?",
    "id": "Siklus Hidup Panjang, Lebih Aman."
  },
  {
    "en": "Apa Kepanjangan Baterai NMC (Nickel Manganese Cobalt)?",
    "id": "Nickel Manganese Cobalt."
  },
  {
    "en": "Keunggulan Baterai NMC (Nickel Manganese Cobalt)?",
    "id": "Kepadatan Energi Sangat Tinggi."
  },
  {
    "en": "Apa Itu Kepadatan Energi (Energy Density)?",
    "id": "Energi Tersimpan Per Volume Atau Berat."
  },
  {
    "en": "Apa Itu Kepadatan Daya (Power Density)?",
    "id": "Kemampuan Memberikan Arus Besar Cepat."
  },
  {
    "en": "Apa Itu Ventilasi (Venting) Baterai?",
    "id": "Katup Pelepas Tekanan Gas Internal."
  },
  {
    "en": "Apa Itu Pelarian Termal (Thermal Runaway) Baterai?",
    "id": "Kondisi Overheat Gagal Kendali (Bahaya)."
  },
  {
    "en": "Apa Kepanjangan NEMA (National Electrical Manufacturers Association)?",
    "id": "National Electrical Manufacturers Association."
  },
  {
    "en": "Apa Rating NEMA Tipe 1?",
    "id": "Penggunaan Dalam Ruangan (Indoor)."
  },
  {
    "en": "Apa Rating NEMA Tipe 3R?",
    "id": "Tahan Hujan (Outdoor)."
  },
  {
    "en": "Apa Rating NEMA Tipe 4?",
    "id": "Tahan Air (Water-Tight)."
  },
  {
    "en": "Apa Rating NEMA Tipe 4X?",
    "id": "Tahan Air Dan Korosi."
  },
  {
    "en": "Apa Rating NEMA Tipe 12?",
    "id": "Tahan Debu (Indoor)."
  },
  {
    "en": "Apa Rating NEMA Tipe 7?",
    "id": "Lokasi Berbahaya (Explosion-Proof Kelas 1)."
  },
  {
    "en": "Apa Rating NEMA Tipe 9?",
    "id": "Lokasi Berbahaya (Dust-Ignition-Proof Kelas 2)."
  },
  {
    "en": "Apa Itu Impedansi Gelombang (Wave Impedance) Ruang Hampa?",
    "id": "Tiga Ratus Tujuh Puluh Tujuh Ohm."
  },
  {
    "en": "Apa Itu Konstanta Propagasi (Propagation Constant)?",
    "id": "Ukuran Perubahan Amplitudo Fasa."
  },
  {
    "en": "Apa Itu Konstanta Atenuasi (Attenuation Constant)?",
    "id": "Bagian Real Konstanta Propagasi."
  },
  {
    "en": "Apa Itu Konstanta Fasa (Phase Constant)?",
    "id": "Bagian Imajiner Konstanta Propagasi."
  },
  {
    "en": "Apa Itu Saluran Transmisi Tak Rugi (Lossless Line)?",
    "id": "Saluran Transmisi Ideal (R=0, G=0)."
  },
  {
    "en": "Apa Itu Saluran Transmisi Distorsi Rendah (Low-Distortion)?",
    "id": "Kondisi R/L = G/C."
  },
  {
    "en": "Apa Itu Medan Listrik (E)?",
    "id": "Kekuatan Gaya Per Satuan Muatan."
  },
  {
    "en": "Apa Satuan Medan Listrik (E)?",
    "id": "Volt Per Meter (V/m)."
  },
  {
    "en": "Apa Itu Kerapatan Fluks Listrik (D)?",
    "id": "Ukuran Fluks Listrik Per Area."
  },
  {
    "en": "Apa Satuan Kerapatan Fluks Listrik (D)?",
    "id": "Coulomb Per Meter Persegi (C/m²)."
  },
  {
    "en": "Apa Hubungan Antara D Dan E?",
    "id": "D = Epsilon * E."
  },
  {
    "en": "Apa Itu Garis Gaya Listrik?",
    "id": "Representasi Arah Medan Listrik."
  },
  {
    "en": "Apa Itu Potensial Listrik (V)?",
    "id": "Energi Potensial Per Satuan Muatan."
  },
  {
    "en": "Apa Itu Permukaan Ekuipotensial?",
    "id": "Permukaan Titik Potensial Sama."
  },
  {
    "en": "Rumus Kapasitansi Pelat Sejajar?",
    "id": "C = Epsilon * A / d."
  },
  {
    "en": "Rumus Energi Tersimpan Kapasitor?",
    "id": "W = 1/2 * C * V²."
  },
  {
    "en": "Rumus Energi Tersimpan Induktor?",
    "id": "W = 1/2 * L * I²."
  },
  {
    "en": "Apa Itu Sirkuit Magnetik (Magnetic Circuit)?",
    "id": "Jalur Tertutup Fluks Magnetik."
  },
  {
    "en": "Apa Rumus GGM (Gaya Gerak Magnet)?",
    "id": "MMF = N * I (Lilitan Kali Arus)."
  },
  {
    "en": "Apa Itu Intensitas Medan Magnet (H)?",
    "id": "GGM (Gaya Gerak Magnet) Per Satuan Panjang."
  },
  {
    "en": "Apa Satuan Intensitas Medan Magnet (H)?",
    "id": "Amper Lilitan Per Meter (A-t/m)."
  },
  {
    "en": "Apa Hubungan Antara B Dan H?",
    "id": "B = Mu * H (Permeabilitas)."
  },
  {
    "en": "Apa Itu Permeabilitas Vakum (μ0)?",
    "id": "Konstanta Magnetik Ruang Hampa."
  },
  {
    "en": "Apa Itu Permeabilitas Relatif (μr)?",
    "id": "Rasio Permeabilitas Bahan Terhadap Vakum."
  },
  {
    "en": "Apa Itu Hukum Hopkinson (Hopkinson's Law)?",
    "id": "Fluks = GGM / Reluktansi."
  },
  {
    "en": "Apa Itu Celah Udara (Air Gap)?",
    "id": "Kesenjangan Udara Dalam Sirkuit Magnetik."
  },
  {
    "en": "Apa Itu Fringing (Efek Pinggiran)?",
    "id": "Fluks Magnetik Melebar Di Celah Udara."
  },
  {
    "en": "Apa Itu Soldering (Penyolderan) Gelombang (Wave)?",
    "id": "Penyolderan Komponen Through-Hole Massal."
  },
  {
    "en": "Apa Itu Soldering (Penyolderan) Reflow?",
    "id": "Penyolderan Komponen SMD (Surface Mount Device) Oven."
  },
  {
    "en": "Apa Itu Pasta Solder (Solder Paste)?",
    "id": "Campuran Timah Dan Fluks (SMD)."
  },
  {
    "en": "Apa Itu Stensil (Stencil) PCB?",
    "id": "Cetakan Pola Pasta Solder."
  },
  {
    "en": "Apa Itu Uji In-Circuit (In-Circuit Test/ICT)?",
    "id": "Pengujian Komponen PCB (Printed Circuit Board) Terpasang."
  },
  {
    "en": "Apa Itu Flying Probe Test (FPT)?",
    "id": "Metode Uji PCB (Printed Circuit Board) Tanpa Dudukan."
  },
  {
    "en": "Apa Itu Uji Fungsional (Functional Test)?",
    "id": "Pengujian Fungsi Keseluruhan Rangkaian."
  },
  {
    "en": "Apa Kepanjangan AOI (Automatic Optical Inspection)?",
    "id": "Automatic Optical Inspection."
  },
  {
    "en": "Fungsi AOI (Automatic Optical Inspection)?",
    "id": "Inspeksi Visual PCB (Printed Circuit Board) Otomatis."
  },
  {
    "en": "Apa Itu Rework (Pengerjaan Ulang) Elektronik?",
    "id": "Proses Perbaikan Komponen PCB (Printed Circuit Board)."
  },
  {
    "en": "Apa Itu Motor Linear (Linear Motor)?",
    "id": "Motor Menghasilkan Gerakan Lurus (Translasi)."
  },
  {
    "en": "Apa Itu Aktuator Solenoida (Solenoid)?",
    "id": "Aktuator Elektromagnetik (Gerak Linear)."
  },
  {
    "en": "Apa Itu Solenoida Tipe Dorong (Push)?",
    "id": "Mendorong Batang Logam Saat Aktif."
  },
  {
    "en": "Apa Itu Solenoida Tipe Tarik (Pull)?",
    "id": "Menarik Batang Logam Saat Aktif."
  },
  {
    "en": "Apa Itu Solenoida Latching (Pengunci)?",
    "id": "Tetap Di Posisi (On/Off) Tanpa Daya."
  },
  {
    "en": "Apa Kepanjangan MEMS (Micro-Electro-Mechanical Systems)?",
    "id": "Micro-Electro-Mechanical Systems."
  },
  {
    "en": "Contoh Komponen MEMS (Micro-Electro-Mechanical Systems)?",
    "id": "Akselerometer, Giroskop, Mikrofon IC."
  },
  {
    "en": "Apa Itu Osilator Pergeseran Fasa (Phase-Shift)?",
    "id": "Osilator Sinusoidal Menggunakan RC (Resistor-Capacitor)."
  },
  {
    "en": "Apa Itu Multivibrator Astabil (Astable)?",
    "id": "Osilator Pembangkit Gelombang Kotak."
  },
  {
    "en": "Apa Itu Multivibrator Monostabil (Monostable)?",
    "id": "Rangkaian Penunda Waktu (One-Shot)."
  },
  {
    "en": "Apa Itu Multivibrator Bistabil (Bistable)?",
    "id": "Rangkaian Flip-Flop (Dua Keadaan Stabil)."
  },
  {
    "en": "Apa Itu Pengganda Tegangan (Voltage Multiplier)?",
    "id": "Rangkaian Penyearah Penaik Tegangan DC."
  },
  {
    "en": "Apa Itu Rangkaian Cockcroft-Walton?",
    "id": "Pengganda Tegangan (Generator Tegangan Tinggi)."
  },
  {
    "en": "Apa Itu Detektor Selubung (Envelope Detector)?",
    "id": "Rangkaian Demodulasi Sinyal AM."
  },
  {
    "en": "Apa Itu Detektor Puncak (Peak Detector)?",
    "id": "Rangkaian Penyimpan Tegangan Puncak Maksimum."
  },
  {
    "en": "Apa Kepanjangan CISC (Complex Instruction Set Computer)?",
    "id": "Complex Instruction Set Computer."
  },
  {
    "en": "Apa Kepanjangan RISC (Reduced Instruction Set Computer)?",
    "id": "Reduced Instruction Set Computer."
  },
  {
    "en": "Perbedaan Utama CISC (Complex) Dan RISC (Reduced)?",
    "id": "Kompleksitas Set Instruksi."
  },
  {
    "en": "Apa Itu Stack Pointer (SP)?",
    "id": "Register Alamat Puncak Tumpukan (Stack)."
  },
  {
    "en": "Apa Itu Tumpukan (Stack) Memori?",
    "id": "Area Memori Penyimpanan Data LIFO."
  },
  {
    "en": "Apa Kepanjangan LIFO (Last-In First-Out)?",
    "id": "Last-In First-Out."
  },
  {
    "en": "Apa Kepanjangan FIFO (First-In First-Out)?",
    "id": "First-In First-Out."
  },
  {
    "en": "Apa Itu Antrian (Queue)?",
    "id": "Struktur Data FIFO (First-In First-Out)."
  },
  {
    "en": "Apa Kepanjangan DMA (Direct Memory Access)?",
    "id": "Direct Memory Access."
  },
  {
    "en": "Keuntungan DMA (Direct Memory Access)?",
    "id": "Transfer Data Cepat, CPU (Central Processing Unit) Bebas."
  },
  {
    "en": "Apa Itu Interupsi (Interrupt)?",
    "id": "Sinyal Asinkron Meminta Perhatian CPU (Central Processing Unit)."
  },
  {
    "en": "Apa Kepanjangan ISR (Interrupt Service Routine)?",
    "id": "Interrupt Service Routine."
  },
  {
    "en": "Apa Itu Watchdog Timer (WDT)?",
    "id": "Timer Pencegah Sistem Macet (Hang)."
  },
  {
    "en": "Bagaimana Watchdog Timer (WDT) Bekerja?",
    "id": "Harus Direset Periodik Oleh Software."
  },
  {
    "en": "Apa Itu Kestabilan (Stabilitas) Sistem Tenaga?",
    "id": "Kemampuan Sistem Pulih Dari Gangguan."
  },
  {
    "en": "Apa Itu Kestabilan (Stabilitas) Tegangan?",
    "id": "Kemampuan Sistem Menjaga Tegangan Stabil."
  },
  {
    "en": "Apa Itu Keruntuhan Tegangan (Voltage Collapse)?",
    "id": "Penurunan Tegangan Drastis Tak Terkendali."
  },
  {
    "en": "Apa Itu Bank Kapasitor (Capacitor Bank)?",
    "id": "Kumpulan Kapasitor Paralel."
  },
  {
    "en": "Fungsi Bank Kapasitor (Shunt)?",
    "id": "Menyuplai Daya Reaktif (Perbaikan PF)."
  },
  {
    "en": "Fungsi Reaktor Shunt?",
    "id": "Menyerap Daya Reaktif (Kontrol Tegangan)."
  },
  {
    "en": "Apa Itu Kompensasi Daya Reaktif?",
    "id": "Pengaturan Aliran Daya Reaktif (VAR)."
  },
  {
    "en": "Apa Itu Kontrol Frekuensi Beban (LFC)?",
    "id": "Menjaga Keseimbangan Daya-Frekuensi."
  },
  {
    "en": "Apa Kepanjangan AGC (Automatic Generation Control)?",
    "id": "Automatic Generation Control."
  },
  {
    "en": "Fungsi AGC (Automatic Generation Control)?",
    "id": "Menyesuaikan Output Generator Otomatis."
  },
  {
    "en": "Apa Itu Cadangan Berputar (Spinning Reserve)?",
    "id": "Kapasitas Generator Siap (Sinkron)."
  },
  {
    "en": "Apa Itu Cadangan Dingin (Cold Reserve)?",
    "id": "Kapasitas Pembangkit (Offline) Siap Start."
  },
  {
    "en": "Apa Itu Cadangan Panas (Hot Reserve)?",
    "id": "Kapasitas Pembangkit (Online) Beban Rendah."
  },
  {
    "en": "Apa Itu Sinyal Kausal (Causal)?",
    "id": "Output Tergantung Input Sekarang Masa Lalu."
  },
  {
    "en": "Apa Itu Sinyal Non-Kausal?",
    "id": "Output Tergantung Input Masa Depan."
  },
  {
    "en": "Apa Itu Sistem Linear (Linearity)?",
    "id": "Memenuhi Prinsip Superposisi."
  },
  {
    "en": "Apa Itu Sistem Invariansi Waktu (Time-Invariance)?",
    "id": "Respon Sistem Tidak Berubah Waktu."
  },
  {
    "en": "Apa Itu Sinyal Ortogonal (Orthogonal)?",
    "id": "Sinyal Saling Bebas (Korelasi Nol)."
  },
  {
    "en": "Apa Itu Daya Tembus (Penetration Depth) EM?",
    "id": "Ukuran Penetrasi Gelombang EM Konduktor."
  },
  {
    "en": "Apa Itu Disipasi Daya (Power Dissipation)?",
    "id": "Energi Listrik Berubah Menjadi Panas."
  },
  {
    "en": "Apa Itu Resistansi Efektif (Effective Resistance) AC?",
    "id": "Hambatan Total Rangkaian AC (Termasuk Rugi)."
  },
  {
    "en": "Apa Itu Konversi Energi Elektromekanik?",
    "id": "Interaksi Antara Sistem Listrik Mekanik."
  },
  {
    "en": "Apa Itu Resonator Keramik?",
    "id": "Osilator Stabil (Lebih Murah Dari Kristal)."
  },
  {
    "en": "Apa Itu Kabel Pita (Ribbon Cable)?",
    "id": "Kabel Datar Paralel (Contoh IDE/PATA)."
  },
  {
    "en": "Apa Kepanjangan FFC (Flexible Flat Cable)?",
    "id": "Flexible Flat Cable."
  },
  {
    "en": "Apa Kepanjangan FPC (Flexible Printed Circuit)?",
    "id": "Flexible Printed Circuit."
  },
  {
    "en": "Apa Kepanjangan ZIF (Zero Insertion Force)?",
    "id": "Zero Insertion Force."
  },
  {
    "en": "Fungsi Konektor ZIF (Zero Insertion Force)?",
    "id": "Menghubungkan Kabel FFC/FPC (Tanpa Gaya)."
  },
  {
    "en": "Apa Itu Hot-Swap (Tukar Panas)?",
    "id": "Mencabut Pasang Komponen Saat Berdaya."
  },
  {
    "en": "Apa Itu Cold-Swap (Tukar Dingin)?",
    "id": "Mencabut Pasang Komponen (Daya Mati)."
  },
  {
    "en": "Apa Itu Impedansi Input Tinggi (High-Z)?",
    "id": "Kondisi Terminal Input (Mengambang)."
  },
  {
    "en": "Apa Itu Driver Gerbang (Gate Driver) Totem-Pole?",
    "id": "Konfigurasi BJT (Bipolar Junction Transistor) Driver Cepat."
  },
  {
    "en": "Apa Itu Penjepit (Clamp) Tegangan?",
    "id": "Rangkaian Pembatas Level Tegangan."
  },
  {
    "en": "Apa Itu Dioda Penjepit (Clamping Diode)?",
    "id": "Dioda Pelindung Lonjakan Tegangan Input."
  },
  {
    "en": "Apa Itu Rangkaian RC (Resistor-Capacitor) Snubber?",
    "id": "Rangkaian RC (Resistor-Capacitor) Meredam Osilasi."
  },
  {
    "en": "Apa Itu Rangkaian RCD (Resistor-Capacitor-Diode) Snubber?",
    "id": "Snubber Pembuang Energi (Lebih Efisien)."
  },
  {
    "en": "Apa Itu Proteksi Kelas I (Class I)?",
    "id": "Peralatan Wajib Dihubungkan Ke Ground."
  },
  {
    "en": "Apa Itu Proteksi Kelas II (Class II)?",
    "id": "Peralatan Dengan Isolasi Ganda."
  },
  {
    "en": "Apa Simbol Proteksi Kelas II (Class II)?",
    "id": "Kotak Di Dalam Kotak."
  },
  {
    "en": "Apa Itu Proteksi Kelas III (Class III)?",
    "id": "Menggunakan Tegangan Ekstra Rendah (SELV)."
  },
  {
    "en": "Apa Kepanjangan SELV (Safety Extra Low Voltage)?",
    "id": "Safety Extra Low Voltage."
  },
  {
    "en": "Apa Itu Pemicu (Trigger) Osiloskop?",
    "id": "Sinkronisasi Titik Awal Tampilan."
  },
  {
    "en": "Apa Itu Pemicu Tepi (Edge Trigger)?",
    "id": "Pemicu Pada Tepi Naik Atau Turun."
  },
  {
    "en": "Apa Itu Pemicu Lebar Pulsa (Pulse Width)?",
    "id": "Pemicu Berdasar Durasi Lebar Pulsa."
  },
  {
    "en": "Apa Itu Mode Pemicu Otomatis (Auto)?",
    "id": "Tampilan Muncul Walau Tanpa Pemicu."
  },
  {
    "en": "Apa Itu Mode Pemicu Normal (Normal)?",
    "id": "Menunggu Kondisi Pemicu Terpenuhi."
  },
  {
    "en": "Apa Kepanjangan PQA (Power Quality Analyzer)?",
    "id": "Power Quality Analyzer."
  },
  {
    "en": "Apa Fungsi PQA (Power Quality Analyzer)?",
    "id": "Merekam Menganalisis Kualitas Daya."
  },
  {
    "en": "Apa Itu Distorsi Notching (Takik)?",
    "id": "Cekungan Tegangan Akibat Penyearah Daya."
  },
  {
    "en": "Apa Itu Ketidakseimbangan (Unbalance) Tegangan?",
    "id": "Magnitudo Fasa Tiga Fasa Berbeda."
  },
  {
    "en": "Apa Akibat Ketidakseimbangan (Unbalance) Tegangan?",
    "id": "Motor Panas, Arus Netral Tinggi."
  },
  {
    "en": "Apa Kepanjangan STA (Static Timing Analysis)?",
    "id": "Static Timing Analysis."
  },
  {
    "en": "Apa Itu Jalur Kritis (Critical Path) Digital?",
    "id": "Jalur Tunda Terpanjang Rangkaian."
  },
  {
    "en": "Apa Itu Batas Waktu Setup (Setup Slack)?",
    "id": "Selisih Waktu Tiba Data Setup."
  },
  {
    "en": "Apa Itu Batas Waktu Hold (Hold Slack)?",
    "id": "Selisih Waktu Tiba Data Hold."
  },
  {
    "en": "Apa Itu Relai (Relay) Elektromekanis (EMR)?",
    "id": "Saklar Mekanis Dikontrol Elektromagnet."
  },
  {
    "en": "Apa Itu Kontak NO (Normally Open) Relai?",
    "id": "Normal Terbuka, Tertutup Saat Aktif."
  },
  {
    "en": "Apa Itu Kontak NC (Normally Closed) Relai?",
    "id": "Normal Tertutup, Terbuka Saat Aktif."
  },
  {
    "en": "Apa Itu Kontak CO (Change-Over) Relai?",
    "id": "Kontak Tukar (SPDT)."
  },
  {
    "en": "Apa Itu Relai (Relay) Latching (Pengunci)?",
    "id": "Mempertahankan Status Tanpa Daya Koil."
  },
  {
    "en": "Apa Itu Relai (Relay) Koil Ganda Latching?",
    "id": "Satu Koil Set, Satu Koil Reset."
  },
  {
    "en": "Apa Itu Relai (Relay) Koil Tunggal Latching?",
    "id": "Polaritas Terbalik Untuk Set/Reset."
  },
  {
    "en": "Apa Itu Waktu Operasi (Operate Time) Relai?",
    "id": "Waktu Tunda Relai Menjadi Aktif."
  },
  {
    "en": "Apa Itu Waktu Lepas (Release Time) Relai?",
    "id": "Waktu Tunda Relai Non-Aktif."
  },
  {
    "en": "Apa Itu Dioda Peredam (Snubber) Koil Relai?",
    "id": "Meredam GGL (Gaya Gerak Listrik) Induksi Balik."
  },
  {
    "en": "Apa Itu Kurva Histeresis (Hysteresis Loop)?",
    "id": "Grafik B-H Material Magnetik."
  },
  {
    "en": "Apa Itu Titik Curie (Curie Temperature)?",
    "id": "Suhu Feromagnetik Kehilangan Sifat Magnet."
  },
  {
    "en": "Apa Itu Bahan Magnetik Keras (Hard)?",
    "id": "Koersivitas Tinggi (Magnet Permanen)."
  },
  {
    "en": "Apa Itu Bahan Magnetik Lunak (Soft)?",
    "id": "Koersivitas Rendah (Inti Trafo)."
  },
  {
    "en": "Apa Itu AlNiCo (Aluminium Nickel Cobalt)?",
    "id": "Bahan Magnet Permanen Keras."
  },
  {
    "en": "Apa Itu Magnet Neodimium (Neodymium)?",
    "id": "Bahan Magnet Permanen Terkuat."
  },
  {
    "en": "Apa Itu Ferit (Ferrite)?",
    "id": "Keramik Magnetik (Inti Frekuensi Tinggi)."
  },
  {
    "en": "Apa Itu Mu-Metal (Mu-Metal)?",
    "id": "Paduan Permeabilitas Tinggi (Pelindung Magnet)."
  },
  {
    "en": "Apa Itu Kontrol Mode Histeresis (Hysteresis)?",
    "id": "Kontrol Catu Daya Switching Cepat."
  },
  {
    "en": "Prinsip Kontrol Histeresis (Hysteresis)?",
    "id": "Output Beralih Antara Dua Batas Atas Bawah."
  },
  {
    "en": "Keuntungan Kontrol Histeresis (Hysteresis)?",
    "id": "Respon Sangat Cepat, Tanpa Kompensasi Loop."
  },
  {
    "en": "Kerugian Kontrol Histeresis (Hysteresis)?",
    "id": "Frekuensi Switching Bervariasi (Ripple)."
  },
  {
    "en": "Apa Itu Topologi Zeta (Zeta Converter)?",
    "id": "Konverter DC-DC (Non-Inverting Buck-Boost)."
  },
  {
    "en": "Apa Itu Penyearah Terkendali Penuh?",
    "id": "Penyearah Menggunakan Enam SCR (Silicon Controlled Rectifier)."
  },
  {
    "en": "Apa Itu Penyearah Setengah Terkendali?",
    "id": "Penyearah Campuran Dioda Dan SCR (Silicon Controlled Rectifier)."
  },
  {
    "en": "Apa Itu Sudut Penyulutan (Firing Angle) (α)?",
    "id": "Sudut Tunda Pemicu SCR (Silicon Controlled Rectifier)."
  },
  {
    "en": "Apa Itu Sudut Padam (Extinction Angle) (β)?",
    "id": "Sudut Saat Arus Thyristor Menjadi Nol."
  },
  {
    "en": "Apa Itu Mode Inversi (Inverting Mode) Penyearah?",
    "id": "Penyearah Berfungsi Sebagai Inverter."
  },
  {
    "en": "Apa Itu Pembanding (Comparator) Digital?",
    "id": "Membandingkan Dua Nilai Biner."
  },
  {
    "en": "Apa Tiga Output Pembanding (Comparator) Digital?",
    "id": "A Lebih Besar B, A Lebih Kecil B, A Sama Dengan B."
  },
  {
    "en": "Apa Itu Priority Encoder (Encoder Prioritas)?",
    "id": "Encoder Input Prioritas Tertinggi."
  },
  {
    "en": "Apa Itu Rangkaian Barrel Shifter?",
    "id": "Menggeser Data Multi-Bit Seketika."
  },
  {
    "en": "Apa Itu Adder (Penjumlah) BCD (Binary Coded Decimal)?",
    "id": "Penjumlah Khusus Bilangan BCD (Binary Coded Decimal)."
  },
  {
    "en": "Apa Itu Generator Paritas (Parity Generator)?",
    "id": "Rangkaian Logika Pembangkit Bit Paritas."
  },
  {
    "en": "Apa Itu Pemeriksa Paritas (Parity Checker)?",
    "id": "Rangkaian Logika Pengecek Bit Paritas."
  },
  {
    "en": "Apa Itu Rangkaian Dekoder Alamat?",
    "id": "Memilih Chip Memori/Periferal."
  },
  {
    "en": "Apa Itu Pembangkit Listrik Tenaga Panas Bumi (PLTP)?",
    "id": "Pembangkit Energi Panas Uap Bumi."
  },
  {
    "en": "Apa Itu PLTS (Pembangkit Listrik Tenaga Surya) Thermal?",
    "id": "Panas Matahari Memanaskan Fluida."
  },
  {
    "en": "Apa Itu Pembangkit Listrik Tenaga Bayu (PLTB)?",
    "id": "Pembangkit Energi Gerak Angin."
  },
  {
    "en": "Apa Itu Pembangkit Listrik Tenaga Pasang Surut?",
    "id": "Energi Gerakan Pasang Surut Air Laut."
  },
  {
    "en": "Apa Kepanjangan OTEC (Ocean Thermal Energy Conversion)?",
    "id": "Konversi Energi Panas Laut."
  },
  {
    "en": "Apa Prinsip OTEC (Ocean Thermal Energy Conversion)?",
    "id": "Beda Suhu Air Laut Permukaan Dalam."
  },
  {
    "en": "Apa Itu Pembangkit Listrik Tenaga Biomassa?",
    "id": "Energi Pembakaran Bahan Organik."
  },
  {
    "en": "Apa Itu Pembangkit Listrik Tenaga Biogas?",
    "id": "Energi Gas Metana Fermentasi."
  },
  {
    "en": "Apa Itu Efek Proksimitas (Proximity Effect)?",
    "id": "Distribusi Arus Akibat Konduktor Dekat."
  },
  {
    "en": "Apa Akibat Efek Proksimitas (Proximity Effect)?",
    "id": "Meningkatkan Hambatan Efektif Lilitan."
  },
  {
    "en": "Apa Itu Arus Sirkulasi (Circulating Current)?",
    "id": "Arus Liar Antar Trafo Paralel."
  },
  {
    "en": "Apa Itu Beban Dummy (Dummy Load)?",
    "id": "Resistor Pengganti Beban Aktual Uji Coba."
  },
  {
    "en": "Fungsi Beban Dummy (Dummy Load) RF (Radio Frequency)?",
    "id": "Menyerap Daya RF (Radio Frequency) Tanpa Radiasi."
  },
  {
    "en": "Apa Itu Induktor Mode-Common (Common Mode)?",
    "id": "Induktor Meredam Noise Common-Mode."
  },
  {
    "en": "Apa Itu Induktor Mode-Diferensial?",
    "id": "Induktor Meredam Noise Diferensial."
  },
  {
    "en": "Apa Itu Torsi Cogging (Cogging Torque)?",
    "id": "Torsi Getar Motor (Tanpa Arus)."
  },
  {
    "en": "Apa Itu Torsi Ripple (Riak Torsi)?",
    "id": "Fluktuasi Torsi Saat Motor Berputar."
  },
  {
    "en": "Apa Itu Linearitas (Linearity) Penguat?",
    "id": "Kemampuan Reproduksi Sinyal Tanpa Distorsi."
  },
  {
    "en": "Apa Itu Titik Saturasi (Saturation Point)?",
    "id": "Batas Operasi Maksimum (Transistor/Inti)."
  },
  {
    "en": "Apa Itu Titik Cut-Off (Titik Putus)?",
    "id": "Batas Operasi Minimum (Transistor)."
  },
  {
    "en": "Apa Kepanjangan TCR (Temperature Coefficient of Resistance)?",
    "id": "Temperature Coefficient of Resistance."
  },
  {
    "en": "Apa Kepanjangan Rth (Thermal Resistance)?",
    "id": "Thermal Resistance (Resistansi Termal)."
  },
  {
    "en": "Apa Satuan Resistansi Termal (Rth)?",
    "id": "Derajat Celcius Per Watt (°C/W)."
  },
  {
    "en": "Apa Itu Suhu Junction (Tj)?",
    "id": "Suhu Operasi Inti Semikonduktor."
  },
  {
    "en": "Apa Itu Suhu Sekitar (Ta)?",
    "id": "Suhu Lingkungan (Ambient)."
  },
  {
    "en": "Apa Itu Suhu Casing (Tc)?",
    "id": "Suhu Permukaan Kemasan Komponen."
  },
  {
    "en": "Apa Itu Rth-JC (Junction-To-Case)?",
    "id": "Resistansi Termal (Inti Ke Kemasan)."
  },
  {
    "en": "Apa Itu Rth-CS (Case-To-Sink)?",
    "id": "Resistansi Termal (Kemasan Ke Pendingin)."
  },
  {
    "en": "Apa Itu Rth-SA (Sink-To-Ambient)?",
    "id": "Resistansi Termal (Pendingin Ke Udara)."
  },
  {
    "en": "Apa Itu Derating (Penurunan Rating)?",
    "id": "Pengurangan Batas Operasi (Suhu Tinggi)."
  },
  {
    "en": "Apa Kepanjangan SOA (Safe Operating Area)?",
    "id": "Safe Operating Area."
  },
  {
    "en": "Apa Itu SOA (Safe Operating Area)?",
    "id": "Batas Aman Tegangan Arus Komponen."
  },
  {
    "en": "Apa Itu Kegagalan Kaskade (Cascading Failure)?",
    "id": "Kegagalan Satu Bagian Picu Lainnya."
  },
  {
    "en": "Apa Itu Kegagalan Mode Common (Common-Mode Failure)?",
    "id": "Kegagalan Multi Komponen Akibat Satu Penyebab."
  },
  {
    "en": "Apa Itu Redundansi (Redundancy)?",
    "id": "Komponen Cadangan Peningkat Keandalan."
  },
  {
    "en": "Apa Itu Hot Standby (Redundansi Panas)?",
    "id": "Cadangan Aktif, Siap Ambil Alih."
  },
  {
    "en": "Apa Itu Cold Standby (Redundansi Dingin)?",
    "id": "Cadangan Mati, Perlu Waktu Aktif."
  },
  {
    "en": "Apa Kepanjangan MTBF (Mean Time Between Failures)?",
    "id": "Mean Time Between Failures."
  },
  {
    "en": "Apa Kepanjangan MTTF (Mean Time To Failure)?",
    "id": "Mean Time To Failure."
  },
  {
    "en": "Perbedaan MTBF (Mean Time Between Failures) Dan MTTF (Mean Time To Failure)?",
    "id": "MTBF (Bisa Diperbaiki), MTTF (Tidak)."
  },
  {
    "en": "Apa Kepanjangan MTTR (Mean Time To Repair)?",
    "id": "Mean Time To Repair."
  },
  {
    "en": "Apa Itu Ketersediaan (Availability)?",
    "id": "Rasio Waktu Operasi Total."
  },
  {
    "en": "Rumus Ketersediaan (Availability)?",
    "id": "MTBF / (MTBF + MTTR)."
  },
  {
    "en": "Apa Itu ADC (Analog to Digital Converter) Tipe Flash?",
    "id": "ADC (Analog to Digital Converter) Paralel Komparator."
  },
  {
    "en": "Keunggulan Flash ADC (Analog to Digital Converter)?",
    "id": "Konversi Sangat Cepat."
  },
  {
    "en": "Kelemahan Flash ADC (Analog to Digital Converter)?",
    "id": "Kompleks, Komponen Sangat Banyak."
  },
  {
    "en": "Apa Kepanjangan SAR (Successive Approximation Register) ADC?",
    "id": "Successive Approximation Register ADC."
  },
  {
    "en": "Prinsip Kerja SAR (Successive Approximation Register) ADC?",
    "id": "Penebakan Biner Berurutan (Digital)."
  },
  {
    "en": "Apa Itu ADC (Analog to Digital Converter) Tipe Sigma-Delta?",
    "id": "ADC (Analog to Digital Converter) Resolusi Tinggi (Oversampling)."
  },
  {
    "en": "Apa Itu ADC (Analog to Digital Converter) Tipe Dual-Slope?",
    "id": "ADC (Analog to Digital Converter) Integrasi (Akurat, Lambat)."
  },
  {
    "en": "Apa Itu DAC (Digital to Analog Converter) Tipe R-2R Ladder?",
    "id": "DAC (Digital to Analog Converter) Jaringan Resistor R-2R."
  },
  {
    "en": "Apa Itu DAC (Digital to Analog Converter) Tipe Binary-Weighted?",
    "id": "DAC (Digital to Analog Converter) Resistor Pembobot Biner."
  },
  {
    "en": "Apa Kepanjangan DNL (Differential Non-Linearity)?",
    "id": "Differential Non-Linearity."
  },
  {
    "en": "Apa Arti DNL (Differential Non-Linearity)?",
    "id": "Kesalahan Lebar Langkah Konversi."
  },
  {
    "en": "Apa Kepanjangan INL (Integral Non-Linearity)?",
    "id": "Integral Non-Linearity."
  },
  {
    "en": "Apa Arti INL (Integral Non-Linearity)?",
    "id": "Kesalahan Akumulatif Linearitas ADC/DAC."
  },
  {
    "en": "Apa Itu Monotonisitas (Monotonicity) DAC?",
    "id": "Output Selalu Naik (Input Naik)."
  },
  {
    "en": "Apa Itu Pelepasan Muatan Sebagian (Partial Discharge)?",
    "id": "Pelepasan Listrik Kecil Di Isolasi."
  },
  {
    "en": "Apa Itu Uji Tan Delta (Faktor Disipasi)?",
    "id": "Mengukur Kualitas Rugi Dielektrik Isolasi."
  },
  {
    "en": "Apa Itu Uji Resistansi Lilitan (Winding Resistance)?",
    "id": "Menguji Integritas Sambungan Lilitan."
  },
  {
    "en": "Apa Itu Uji Rasio Lilitan (Turns Ratio Test)?",
    "id": "Memverifikasi Rasio Lilitan Primer Sekunder."
  },
  {
    "en": "Apa Itu Dispatch (Pengiriman) Ekonomi (Economic Dispatch)?",
    "id": "Alokasi Pembangkit Termurah (Optimal)."
  },
  {
    "en": "Apa Itu Unit Commitment (Komitmen Unit)?",
    "id": "Penjadwalan Start/Stop Unit Pembangkit."
  },
  {
    "en": "Apa Itu Biaya Marginal (Marginal Cost) Pembangkit?",
    "id": "Biaya Hasilkan Satu MWh Tambahan."
  },
  {
    "en": "Apa Itu Blackout (Padam Total)?",
    "id": "Hilangnya Pasokan Listrik Area Luas."
  },
  {
    "en": "Apa Itu Brownout (Padam Sebagian)?",
    "id": "Penurunan Tegangan Jaringan Sengaja."
  },
  {
    "en": "Apa Itu Penjatuhan Beban (Load Shedding)?",
    "id": "Pemadaman Paksa (Cegah Blackout Total)."
  },
  {
    "en": "Apa Itu Efek Fotokonduktif?",
    "id": "Hambatan Berubah Akibat Cahaya."
  },
  {
    "en": "Contoh Perangkat Fotokonduktif?",
    "id": "LDR (Light Dependent Resistor)."
  },
  {
    "en": "Apa Itu Efek Fotovoltaik?",
    "id": "Pembangkitan Tegangan Akibat Cahaya."
  },
  {
    "en": "Contoh Perangkat Fotovoltaik?",
    "id": "Sel Surya (Solar Cell)."
  },
  {
    "en": "Apa Itu Efisiensi Kuantum (Quantum Efficiency)?",
    "id": "Rasio Elektron Terhadap Foton Masuk."
  },
  {
    "en": "Apa Itu Laser Gas (Gas Laser)?",
    "id": "Laser Media Penguat Gas (HeNe)."
  },
  {
    "en": "Apa Itu Laser Zat Padat (Solid-State Laser)?",
    "id": "Laser Media Penguat Kristal (Nd:YAG)."
  },
  {
    "en": "Apa Itu Pemompaan (Pumping) Optik?",
    "id": "Proses Pemberian Energi Media Laser."
  },
  {
    "en": "Apa Itu Inversi Populasi (Population Inversion)?",
    "id": "Kondisi Syarat Terjadinya Lasing."
  },
  {
    "en": "Apa Itu Resonator Optik (Optical Resonator)?",
    "id": "Dua Cermin Pembentuk Sinar Laser."
  },
  {
    "en": "Apa Itu Serat Optik Mode Tunggal (Single-Mode)?",
    "id": "Serat Optik Satu Jalur Cahaya."
  },
  {
    "en": "Apa Itu Serat Optik Mode Ganda (Multi-Mode)?",
    "id": "Serat Optik Banyak Jalur Cahaya."
  },
  {
    "en": "Keunggulan Serat Optik Mode Tunggal?",
    "id": "Jarak Jauh, Bandwidth Sangat Tinggi."
  },
  {
    "en": "Keunggulan Serat Optik Mode Ganda?",
    "id": "Mudah Disambung, Harga Murah."
  },
  {
    "en": "Apa Itu Dispersi (Dispersion) Serat Optik?",
    "id": "Pelebaran Pulsa Sinyal Cahaya."
  },
  {
    "en": "Apa Itu Dispersi Kromatik (Chromatic Dispersion)?",
    "id": "Beda Kecepatan Panjang Gelombang."
  },
  {
    "en": "Apa Itu Dispersi Modus (Modal Dispersion)?",
    "id": "Beda Waktu Tempuh Modus (Multi-Mode)."
  },
  {
    "en": "Apa Itu Atenuasi (Attenuation) Serat Optik?",
    "id": "Pelemahan Intensitas Cahaya."
  },
  {
    "en": "Apa Itu Bahan Ferroelektrik?",
    "id": "Material Polarisasi Spontan (Bisa Dibalik)."
  },
  {
    "en": "Apa Itu Bahan Piroelektrik?",
    "id": "Polarisasi Berubah Akibat Suhu."
  },
  {
    "en": "Apa Itu Elektret (Electret)?",
    "id": "Material Dielektrik Muatan Listrik Permanen."
  },
  {
    "en": "Aplikasi Mikrofon Elektret (Electret)?",
    "id": "Mikrofon Kondensor Murah (Kompak)."
  },
  {
    "en": "Apa Itu Rangkaian RC (Resistor-Capacitor) Differentiator?",
    "id": "Filter High-Pass (HPF) Sederhana."
  },
  {
    "en": "Apa Itu Rangkaian RC (Resistor-Capacitor) Integrator?",
    "id": "Filter Low-Pass (LPF) Sederhana."
  },
  {
    "en": "Apa Itu Waktu Tunda Propagasi (Propagation Delay)?",
    "id": "Waktu Sinyal Merambat (Input Ke Output)."
  },
  {
    "en": "Apa Itu Fan-in Gerbang Logika?",
    "id": "Jumlah Input Maksimum Gerbang."
  },
  {
    "en": "Apa Itu Fan-out Gerbang Logika?",
    "id": "Batas Beban Output Gerbang."
  },
  {
    "en": "Apa Itu Output Open-Collector?",
    "id": "Output Transistor BJT (Kolektor Terbuka)."
  },
  {
    "en": "Apa Itu Output Open-Drain?",
    "id": "Output Transistor MOSFET (Drain Terbuka)."
  },
  {
    "en": "Apa Yang Dibutuhkan Output Open-Collector?",
    "id": "Resistor Pull-Up Eksternal."
  },
  {
    "en": "Apa Itu Rangkaian Wired-AND?",
    "id": "Gabungan Output Open-Collector (Logika AND)."
  },
  {
    "en": "Apa Kepanjangan RDS(on) (Resistansi On) MOSFET?",
    "id": "Resistansi Drain-Source Saat ON."
  },
  {
    "en": "Apa Itu Muatan Gerbang (Qg) MOSFET?",
    "id": "Total Muatan Mengisi Gerbang (ON)."
  },
  {
    "en": "Apa Itu Uji Pembagian (Binning) LED?",
    "id": "Sortir LED Berdasar Warna Kecerahan."
  },
  {
    "en": "Apa Itu Droop (Layu) LED?",
    "id": "Penurunan Efisiensi LED Arus Tinggi."
  },
  {
    "en": "Apa Itu Driver (Pengemudi) LED Arus Konstan?",
    "id": "Catu Daya Menjaga Arus LED Stabil."
  },
  {
    "en": "Apa Itu Driver (Pengemudi) LED Tegangan Konstan?",
    "id": "Catu Daya Menjaga Tegangan Stabil."
  },
  {
    "en": "Apa Itu Jarak Bebas (Clearance) PCB?",
    "id": "Jarak Terpendek Lewat Udara."
  },
  {
    "en": "Apa Itu Jarak Rambat (Creepage) PCB?",
    "id": "Jarak Terpendek Lewat Permukaan."
  },
  {
    "en": "Mengapa Jarak Rambat (Creepage) PCB Penting?",
    "id": "Mencegah Arus Bocor Akibat Kontaminasi."
  },
  {
    "en": "Apa Itu Pengujian Flying Probe?",
    "id": "Uji PCB (Printed Circuit Board) Otomatis Pakai Probe Bergerak."
  },
  {
    "en": "Apa Itu Pengujian Bed-of-Nails?",
    "id": "Uji PCB (Printed Circuit Board) Pakai Titik Pin Tetap."
  },
  {
    "en": "Apa Kepanjangan JTAG (Joint Test Action Group)?",
    "id": "Joint Test Action Group (Standar Uji)."
  },
  {
    "en": "Apa Itu Rantai Pindai (Scan Chain) JTAG?",
    "id": "Jalur Serial Tes Antar IC (Integrated Circuit)."
  },
  {
    "en": "Apa Kepanjangan BIST (Built-In Self-Test)?",
    "id": "Built-In Self-Test (Uji Diri Bawaan)."
  },
  {
    "en": "Apa Itu Model T (Model-T) Atenuator?",
    "id": "Atenuator Resistor Bentuk T."
  },
  {
    "en": "Apa Itu Model Pi (Model-Pi) Atenuator?",
    "id": "Atenuator Resistor Bentuk Pi (π)."
  },
  {
    "en": "Apa Itu Jembatan Hibrida (Hybrid Bridge)?",
    "id": "Sirkuit Pembagi/Penggabung Sinyal RF."
  },
  {
    "en": "Apa Itu Sirkulator (Circulator) RF?",
    "id": "Perangkat Tiga Port (Aliran Satu Arah)."
  },
  {
    "en": "Apa Itu Isolator (Isolator) RF?",
    "id": "Perangkat Dua Port (Aliran Satu Arah)."
  },
  {
    "en": "Fungsi Isolator (Isolator) RF?",
    "id": "Melindungi Pemancar Dari Pantulan Daya."
  },
  {
    "en": "Apa Itu Noise Putih (White Noise)?",
    "id": "Noise Kepadatan Spektral Datar."
  },
  {
    "en": "Apa Itu Noise Merah Jambu (Pink Noise)?",
    "id": "Noise Kepadatan Spektral Turun (1/f)."
  },
  {
    "en": "Apa Itu Noise Biru (Blue Noise)?",
    "id": "Kepadatan Spektral Naik (Proporsional f)."
  },
  {
    "en": "Apa Itu Noise Tembakan (Shot Noise)?",
    "id": "Noise Akibat Sifat Diskret Muatan Listrik."
  },
  {
    "en": "Apa Itu Noise Kedipan (Flicker Noise)?",
    "id": "Noise Frekuensi Rendah (1/f)."
  },
  {
    "en": "Apa Itu Suhu Noise (Noise Temperature)?",
    "id": "Representasi Daya Noise Dalam Kelvin."
  },
  {
    "en": "Apa Itu Angka Kebaikan (Figure of Merit)?",
    "id": "Parameter Kinerja Perbandingan Sistem."
  },
  {
    "en": "Apa Itu Transformasi Bintang-Delta (Star-Delta)?",
    "id": "Konversi Jaringan Tiga Fasa."
  },
  {
    "en": "Kapan Transformasi Bintang-Delta Digunakan?",
    "id": "Analisis Rangkaian, Starting Motor."
  },
  {
    "en": "Apa Itu Daya Kompleks (Complex Power) (S)?",
    "id": "Kombinasi Daya Aktif (P) Reaktif (Q)."
  },
  {
    "en": "Apa Rumus Daya Kompleks (S)?",
    "id": "S = P + jQ."
  },
  {
    "en": "Apa Itu Segitiga Daya (Power Triangle)?",
    "id": "Hubungan Grafis Vektor P, Q, S."
  },
  {
    "en": "Apa Itu Metode Dua Wattmeter?",
    "id": "Mengukur Daya Tiga Fasa Dua Wattmeter."
  },
  {
    "en": "Syarat Metode Dua Wattmeter?",
    "id": "Sistem Tiga Kabel (Tanpa Netral)."
  },
  {
    "en": "Apa Itu Sistem Tiga Fasa Empat Kawat?",
    "id": "Tiga Fasa Dengan Kabel Netral."
  },
  {
    "en": "Apa Itu Sistem Tiga Fasa Tiga Kawat?",
    "id": "Tiga Fasa Tanpa Kabel Netral (Delta)."
  },
  {
    "en": "Apa Itu Urutan Fasa (Phase Sequence)?",
    "id": "Urutan Puncak Tegangan (ABC Atau ACB)."
  },
  {
    "en": "Mengapa Urutan Fasa (Phase Sequence) Penting?",
    "id": "Menentukan Arah Putaran Motor Tiga Fasa."
  },
  {
    "en": "Apa Itu Indikator Urutan Fasa (Phase Sequence)?",
    "id": "Alat Uji Urutan Fasa (RST/ABC)."
  },
  {
    "en": "Berapa Arus Netral (Beban Seimbang)?",
    "id": "Nol Ampere (Idealnya)."
  },
  {
    "en": "Apa Itu Osiloskop Penyimpanan Digital (DSO)?",
    "id": "Osiloskop Menyimpan Data Digital (Memori)."
  },
  {
    "en": "Apa Kepanjangan MSO (Mixed Signal Oscilloscope)?",
    "id": "Mixed Signal Oscilloscope."
  },
  {
    "en": "Apa Kelebihan MSO (Mixed Signal Oscilloscope)?",
    "id": "Mengukur Sinyal Analog Dan Digital."
  },
  {
    "en": "Apa Itu Bandwidth (Lebar Pita) Osiloskop?",
    "id": "Batas Frekuensi Ukur Osiloskop."
  },
  {
    "en": "Apa Itu Laju Sampling (Sample Rate) Osiloskop?",
    "id": "Kecepatan ADC (Analog to Digital Converter) Merekam Sinyal."
  },
  {
    "en": "Apa Itu Kedalaman Memori (Memory Depth) Osiloskop?",
    "id": "Jumlah Titik Data Disimpan."
  },
  {
    "en": "Apa Itu Interpolasi (Interpolation) Osiloskop?",
    "id": "Estimasi Data Antar Titik Sampel."
  },
  {
    "en": "Apa Itu Probe Osiloskop Pasif 1X?",
    "id": "Probe Tanpa Pelemahan Sinyal."
  },
  {
    "en": "Apa Itu Probe Osiloskop Pasif 10X?",
    "id": "Probe Melemahkan Sinyal 10 Kali."
  },
  {
    "en": "Keuntungan Probe 10X?",
    "id": "Impedansi Input Tinggi (Beban Rendah)."
  },
  {
    "en": "Apa Itu Kompensasi Probe (Probe Compensation)?",
    "id": "Kalibrasi Probe (Menghindari Distorsi)."
  },
  {
    "en": "Apa Itu Sinyal Kalibrasi Probe?",
    "id": "Gelombang Kotak (Biasanya 1 KHz)."
  },
  {
    "en": "Apa Itu Kopling DC (DC Coupling) Osiloskop?",
    "id": "Menampilkan Komponen AC Dan DC Sinyal."
  },
  {
    "en": "Apa Itu Kopling AC (AC Coupling) Osiloskop?",
    "id": "Hanya Menampilkan Komponen AC Sinyal."
  },
  {
    "en": "Apa Itu Kopling Ground (GND) Osiloskop?",
    "id": "Menampilkan Referensi Nol Volt."
  },
  {
    "en": "Apa Itu Mode X-Y Osiloskop?",
    "id": "Menampilkan Grafik Sinyal X Vs Sinyal Y."
  },
  {
    "en": "Apa Itu Gambar Lissajous (Lissajous Figure)?",
    "id": "Pola Mode X-Y (Ukur Fasa/Frekuensi)."
  },
  {
    "en": "Apa Itu Analisis FFT (Fast Fourier Transform) Osiloskop?",
    "id": "Menampilkan Spektrum Frekuensi Sinyal."
  },
  {
    "en": "Apa Itu Persistensi (Persistence) Tampilan Osiloskop?",
    "id": "Jejak Sinyal Lama Di Layar."
  },
  {
    "en": "Apa Itu Diagram Mata (Eye Diagram)?",
    "id": "Tampilan Uji Kualitas Sinyal Digital."
  },
  {
    "en": "Apa Itu Sistem Tenaga Tak Seimbang?",
    "id": "Beban Atau Sumber Tiga Fasa Berbeda."
  },
  {
    "en": "Apa Itu Faktor Ketidakseimbangan (Unbalance Factor)?",
    "id": "Ukuran Persentase Ketidakseimbangan."
  },
  {
    "en": "Apa Itu Komponen Urutan Nol (Zero Sequence)?",
    "id": "Komponen Sefasa (Gangguan Tanah)."
  },
  {
    "en": "Apa Itu Komponen Urutan Positif (Positive Sequence)?",
    "id": "Komponen Urutan Fasa Normal (ABC)."
  },
  {
    "en": "Apa Itu Komponen Urutan Negatif (Negative Sequence)?",
    "id": "Komponen Urutan Fasa Terbalik (ACB)."
  },
  {
    "en": "Dampak Komponen Urutan Negatif (Motor)?",
    "id": "Medan Putar Berlawanan, Panas Berlebih."
  },
  {
    "en": "Dampak Komponen Urutan Nol?",
    "id": "Arus Tinggi Di Netral."
  },
  {
    "en": "Apa Itu Jaringan Urutan Nol (Zero Sequence Network)?",
    "id": "Model Rangkaian Analisis Urutan Nol."
  },
  {
    "en": "Apa Itu Jaringan Urutan Positif (Positive Sequence)?",
    "id": "Model Rangkaian Analisis Urutan Positif."
  },
  {
    "en": "Apa Itu Jaringan Urutan Negatif (Negative Sequence)?",
    "id": "Model Rangkaian Analisis Urutan Negatif."
  },
  {
    "en": "Impedansi Urutan Nol Generator?",
    "id": "Tergantung Konfigurasi Pentanahan Netral."
  },
  {
    "en": "Impedansi Urutan Nol Trafo?",
    "id": "Tergantung Konfigurasi Belitan (Y/Δ)."
  },
  {
    "en": "Apa Itu Sistem Pendingin (Cooling) ONAN?",
    "id": "Oil Natural Air Natural (Trafo)."
  },
  {
    "en": "Apa Itu Sistem Pendingin (Cooling) ONAF?",
    "id": "Oil Natural Air Forced (Trafo)."
  },
  {
    "en": "Apa Itu Sistem Pendingin (Cooling) OFAF?",
    "id": "Oil Forced Air Forced (Trafo)."
  },
  {
    "en": "Apa Itu Pengering Udara (Breather) Trafo?",
    "id": "Bernapas (Menyaring Udara Masuk)."
  },
  {
    "en": "Isi Pengering Udara (Breather) Trafo?",
    "id": "Gel Silika (Silica Gel)."
  },
  {
    "en": "Fungsi Gel Silika (Silica Gel)?",
    "id": "Menyerap Kelembaban Udara."
  },
  {
    "en": "Warna Gel Silika (Silica Gel) Kering?",
    "id": "Biru (Terkadang Oranye)."
  },
  {
    "en": "Warna Gel Silika (Silica Gel) Jenuh?",
    "id": "Merah Muda (Pink)."
  },
  {
    "en": "Apa Itu Konservator (Conservator Tank) Trafo?",
    "id": "Tangki Cadangan Pemuaian Minyak."
  },
  {
    "en": "Apa Itu Pipa Ledakan (Explosion Vent) Trafo?",
    "id": "Katup Pelepas Tekanan Darurat."
  },
  {
    "en": "Apa Itu Uji Impuls (Impulse Test) Trafo?",
    "id": "Menguji Ketahanan Trafo Terhadap Petir."
  },
  {
    "en": "Apa Itu Uji SFRA (Sweep Frequency Response Analysis)?",
    "id": "Uji Deteksi Kerusakan Mekanis Lilitan."
  },
  {
    "en": "Apa Itu Uji DGA (Dissolved Gas Analysis)?",
    "id": "Analisis Gas Terlarut Minyak Trafo."
  },
  {
    "en": "Tujuan Uji DGA (Dissolved Gas Analysis)?",
    "id": "Mendeteksi Kegagalan Internal Trafo."
  },
  {
    "en": "Apa Itu Gas Kunci (Key Gas) Uji DGA?",
    "id": "Hidrogen, Metana, Asetilena."
  },
  {
    "en": "Apa Indikasi Gas Asetilena (C2H2)?",
    "id": "Busur Api (Arcing) Suhu Sangat Tinggi."
  },
  {
    "en": "Apa Itu Pemeliharaan Preventif (Preventive)?",
    "id": "Pemeliharaan Terjadwal Mencegah Kerusakan."
  },
  {
    "en": "Apa Itu Pemeliharaan Prediktif (Predictive)?",
    "id": "Pemeliharaan Berdasar Kondisi Aktual."
  },
  {
    "en": "Apa Itu Pemeliharaan Korektif (Corrective)?",
    "id": "Pemeliharaan Setelah Terjadi Kerusakan."
  },
  {
    "en": "Apa Itu Termografi Inframerah (Infrared)?",
    "id": "Inspeksi Suhu (Deteksi Titik Panas)."
  },
  {
    "en": "Apa Itu Titik Panas (Hot Spot) Listrik?",
    "id": "Area Resistansi Tinggi (Koneksi Longgar)."
  },
  {
    "en": "Apa Itu Isolasi Padat (Solid Insulation)?",
    "id": "Contoh: PVC, Karet, Keramik, Mika."
  },
  {
    "en": "Apa Itu Isolasi Cair (Liquid Insulation)?",
    "id": "Contoh: Minyak Trafo."
  },
  {
    "en": "Apa Itu Isolasi Gas (Gaseous Insulation)?",
    "id": "Contoh: Udara, SF6 (Sulfur Hexafluoride)."
  },
  {
    "en": "Apa Itu Keseimbangan Beban (Load Balancing)?",
    "id": "Pembagian Beban Merata Antar Fasa."
  },
  {
    "en": "Apa Itu Manajemen Sisi Permintaan (DSM)?",
    "id": "Mengelola Pola Konsumsi Energi Listrik."
  },
  {
    "en": "Apa Itu Audit Energi (Energy Audit)?",
    "id": "Evaluasi Penggunaan Energi (Identifikasi Hemat)."
  },
  {
    "en": "Apa Itu Konservasi Energi (Energy Conservation)?",
    "id": "Tindakan Mengurangi Pemborosan Energi."
  },
  {
    "en": "Apa Itu Efisiensi Energi (Energy Efficiency)?",
    "id": "Menggunakan Lebih Sedikit Energi (Output Sama)."
  },
  {
    "en": "Apa Itu Regulasi Tegangan (Voltage Regulation)?",
    "id": "Perubahan Tegangan (Beban Nol Penuh)."
  },
  {
    "en": "Apa Itu Trafo CT (Current Transformer) Tipe Jendela?",
    "id": "CT (Current Transformer) Tanpa Lilitan Primer (Kabel Lewat)."
  },
  {
    "en": "Apa Itu Trafo CT (Current Transformer) Tipe Bar (Batang)?",
    "id": "CT (Current Transformer) Dengan Batang Konduktor Primer."
  },
  {
    "en": "Apa Itu Trafo CT (Current Transformer) Tipe Lilit (Wound)?",
    "id": "CT (Current Transformer) Lilitan Primer Sekunder Lengkap."
  },
  {
    "en": "Apa Itu Beban (Burden) Trafo CT?",
    "id": "Total Beban Impedansi Sisi Sekunder."
  },
  {
    "en": "Apa Itu Kelas Akurasi CT (Current Transformer)?",
    "id": "Tingkat Ketelitian Pengukuran CT (Current Transformer)."
  },
  {
    "en": "Apa Itu CT (Current Transformer) Pengukuran (Metering)?",
    "id": "Akurasi Tinggi Rentang Normal."
  },
  {
    "en": "Apa Itu CT (Current Transformer) Proteksi (Protection)?",
    "id": "Tidak Saturasi Saat Arus Gangguan."
  },
  {
    "en": "Apa Itu Saturasi CT (Current Transformer)?",
    "id": "Kondisi Inti Jenuh (Output Error)."
  },
  {
    "en": "Bahaya Sisi Sekunder CT (Current Transformer) Terbuka?",
    "id": "Menghasilkan Tegangan Sangat Tinggi."
  },
  {
    "en": "Apa Itu Trafo PT (Potential Transformer)?",
    "id": "Trafo Penurun Tegangan (Pengukuran)."
  },
  {
    "en": "Nama Lain Trafo PT (Potential Transformer)?",
    "id": "Trafo Tegangan (VT - Voltage Transformer)."
  },
  {
    "en": "Apa Itu Trafo CVT (Capacitive Voltage Transformer)?",
    "id": "Trafo Tegangan Pembagi Kapasitif."
  },
  {
    "en": "Kapan CVT (Capacitive Voltage Transformer) Digunakan?",
    "id": "Sistem Transmisi Tegangan Ekstra Tinggi."
  },
  {
    "en": "Apa Itu Varian Tipe 74HC (High-Speed CMOS)?",
    "id": "Logika CMOS (Complementary Metal Oxide Semiconductor) Cepat (Tegangan 5V)."
  },
  {
    "en": "Apa Itu Varian Tipe 74HCT (High-Speed CMOS TTL)?",
    "id": "Kompatibel Level Input TTL (Transistor-Transistor Logic)."
  },
  {
    "en": "Apa Itu Varian Tipe 74LS (Low-Power Schottky)?",
    "id": "TTL (Transistor-Transistor Logic) Cepat Daya Rendah."
  },
  {
    "en": "Apa Itu Varian Tipe 74F (Fast)?",
    "id": "TTL (Transistor-Transistor Logic) Sangat Cepat."
  },
  {
    "en": "Apa Itu Varian Tipe 74ALS (Advanced Low-Power Schottky)?",
    "id": "Peningkatan Performa Dari 74LS."
  },
  {
    "en": "Apa Itu Hambatan Dinamis (Dynamic Resistance) Dioda?",
    "id": "Hambatan Internal Dioda (Saat Forward)."
  },
  {
    "en": "Apa Itu Kapasitansi Transisi (Transition) Dioda?",
    "id": "Kapasitansi Daerah Deplesi (Reverse Bias)."
  },
  {
    "en": "Apa Itu Kapasitansi Difusi (Diffusion) Dioda?",
    "id": "Kapasitansi Akibat Injeksi Muatan (Forward)."
  },
  {
    "en": "Apa Itu Waktu Pemulihan Mundur (Reverse Recovery Time)?",
    "id": "Waktu Dioda Berhenti Konduksi Mundur."
  },
  {
    "en": "Apa Itu Dioda Pemulihan Cepat (Fast Recovery)?",
    "id": "Dioda Waktu Pemulihan Sangat Singkat."
  },
  {
    "en": "Apa Itu Dioda Pemulihan Lunak (Soft Recovery)?",
    "id": "Dioda Pemulihan Bertahap (Noise Rendah)."
  },
  {
    "en": "Apa Itu Sinyal Elektroluminesensi (Electroluminescence)?",
    "id": "Emisi Cahaya Akibat Medan Listrik."
  },
  {
    "en": "Apa Kepanjangan OLED (Organic Light Emitting Diode)?",
    "id": "Organic Light Emitting Diode."
  },
  {
    "en": "Apa Kepanjangan AMOLED (Active Matrix OLED)?",
    "id": "Active Matrix Organic Light Emitting Diode."
  },
  {
    "en": "Apa Kepanjangan PMOLED (Passive Matrix OLED)?",
    "id": "Passive Matrix Organic Light Emitting Diode."
  },
  {
    "en": "Apa Kepanjangan TFT (Thin Film Transistor)?",
    "id": "Thin Film Transistor."
  },
  {
    "en": "Fungsi TFT (Thin Film Transistor) Pada Layar?",
    "id": "Saklar Individual Setiap Piksel."
  },
  {
    "en": "Apa Itu Tampilan Multiplexing (Display Multiplexing)?",
    "id": "Teknik Mengemudi Layar (Hemat Pin)."
  },
  {
    "en": "Apa Itu Tampilan Matrix (Dot Matrix)?",
    "id": "Tampilan Titik Susunan Baris Kolom."
  },
  {
    "en": "Apa Itu Pengontrol (Controller) LCD (Liquid Crystal Display)?",
    "id": "IC (Integrated Circuit) Pengendali Tampilan LCD (Liquid Crystal Display)."
  }


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
