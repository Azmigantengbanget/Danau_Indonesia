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


    { "en": "Simalungun, Toba, Karo, Dairi, Humbang Hasundutan (Sumatera Utara)?", "id": "Danau Toba." },
    { "en": "Agam (Sumatera Barat)?", "id": "Danau Maninjau." },
    { "en": "Solok & Tanah Datar (Sumatera Barat)?", "id": "Danau Singkarak." },
    { "en": "Poso (Sulawesi Tengah)?", "id": "Danau Poso." },
    { "en": "Luwu Timur (Sulawesi Selatan)?", "id": "Danau Towuti." },
    { "en": "Jayapura (Papua)?", "id": "Danau Sentani." },
    { "en": "Wajo (Sulawesi Selatan)?", "id": "Danau Tempe." },
    { "en": "Paniai (Papua Tengah)?", "id": "Danau Paniai." },
    { "en": "Kapuas Hulu (Kalimantan Barat)?", "id": "Danau Sentarum." },
    { "en": "Kerinci (Jambi)?", "id": "Danau Kerinci." },
    { "en": "Lampung Barat (Lampung) & Ogan Komering Ulu Selatan (Sumatera Selatan)?", "id": "Danau Ranau." },
    { "en": "Bangli (Bali)?", "id": "Danau Batur." },
    { "en": "Tabanan (Bali)?", "id": "Danau Bratan." },
    { "en": "Ende (Nusa Tenggara Timur)?", "id": "Danau Kelimutu." },
    { "en": "Semarang (Jawa Tengah)?", "id": "Danau Rawa Pening." },
    { "en": "Lumajang (Jawa Timur)?", "id": "Danau Ranu Kumbolo." },
    { "en": "Gorontalo (Gorontalo)?", "id": "Danau Limboto." },
    { "en": "Lombok Tengah (Nusa Tenggara Barat)?", "id": "Danau Segara Anak." },
    { "en": "Buol (Sulawesi Tengah)?", "id": "Danau Tondano." },
    { "en": "Kampar (Riau)?", "id": "Danau PLTA Koto Panjang." },
    { "en": "Solok Selatan (Sumatera Barat)?", "id": "Danau Bontak." },
    { "en": "Bandung Barat (Jawa Barat)?", "id": "Waduk Saguling." },
    { "en": "Purwakarta (Jawa Barat)?", "id": "Waduk Jatiluhur." },
    { "en": "Bandung (Jawa Barat)?", "id": "Situ Patenggang." },
    { "en": "Garut (Jawa Barat)?", "id": "Situ Bagendit." },
    { "en": "Magetan (Jawa Timur)?", "id": "Telaga Sarangan." },
    { "en": "Wonogiri (Jawa Tengah)?", "id": "Waduk Gajah Mungkur." },
    { "en": "Kulon Progo (DI Yogyakarta)?", "id": "Waduk Sermo." },
    { "en": "Buleleng (Bali)?", "id": "Danau Buyan." },
    { "en": "Gowa (Sulawesi Selatan)?", "id": "Danau Mawang." },
    { "en": "Mamberamo Raya (Papua)?", "id": "Danau Rombebai." },
    { "en": "Mimika (Papua Tengah)?", "id": "Danau Habbema." },
    { "en": "Banggai Kepulauan (Sulawesi Tengah)?", "id": "Danau Paisu Pok." },
    { "en": "Berau (Kalimantan Timur)?", "id": "Danau Labuan Cermin." },
    { "en": "Gunungkidul (DI Yogyakarta)?", "id": "Telaga Biru Semin." },
    { "en": "Aceh Tengah (Aceh)?", "id": "Danau Laut Tawar." },
    { "en": "Alahan Panjang (Sumatera Barat)?", "id": "Danau Diatas dan Dibawah." },
    { "en": "Bengkulu Utara (Bengkulu)?", "id": "Danau Dendam Tak Sudah." },
    { "en": "Belitung Timur (Kepulauan Bangka Belitung)?", "id": "Danau Kaolin." },
    { "en": "Depok (Jawa Barat)?", "id": "Danau Kenanga." },
    { "en": "Kutai Kartanegara (Kalimantan Timur)?", "id": "Danau Semayang." },
    { "en": "Barito Utara (Kalimantan Tengah)?", "id": "Danau Teluk Jokin." },
    { "en": "Sigi (Sulawesi Tengah)?", "id": "Danau Lindu." },
    { "en": "Muna (Sulawesi Tenggara)?", "id": "Danau Napabale." },
    { "en": "Lombok Timur (Nusa Tenggara Barat)?", "id": "Danau Biru." },
    { "en": "Sumba Tengah (Nusa Tenggara Timur)?", "id": "Danau Weekuri." },
    { "en": "Kaimana (Papua Barat)?", "id": "Danau Kamaka." },
    { "en": "Jember (Jawa Timur)?", "id": "Danau Taman Hidup." },
    { "en": "Probolinggo (Jawa Timur)?", "id": "Danau Ranu Regulo." },
    { "en": "Klaten (Jawa Tengah)?", "id": "Rawa Jombor." },
    { "en": "Boyolali (Jawa Tengah)?", "id": "Waduk Cengklik." },
    { "en": "Bima (Nusa Tenggara Barat)?", "id": "Danau Satonda." },
    { "en": "Sorong (Papua Barat Daya)?", "id": "Danau Uter." },
    { "en": "Teluk Bintuni (Papua Barat)?", "id": "Danau Ayamaru." },
    { "en": "Halmahera Utara (Maluku Utara)?", "id": "Danau Galela." },
    { "en": "Pulau Morotai (Maluku Utara)?", "id": "Danau Raja." },
    { "en": "Parigi Moutong (Sulawesi Tengah)?", "id": "Danau Tambing." },
    { "en": "Hulu Sungai Selatan (Kalimantan Selatan)?", "id": "Danau Bangkau." },
    { "en": "OKU Timur (Sumatera Selatan)?", "id": "Danau Beko." },
    { "en": "Tangerang (Banten)?", "id": "Danau Cisoka." },
    { "en": "Wonosobo (Jawa Tengah)?", "id": "Telaga Warna Dieng." },
    { "en": "Bolaang Mongondow Timur (Sulawesi Utara)?", "id": "Danau Moat." },
    { "en": "Seruyan (Kalimantan Tengah)?", "id": "Danau Sembuluh." },
    { "en": "Siak (Riau)?", "id": "Danau Zamrud." },
    { "en": "Tasikmalaya (Jawa Barat)?", "id": "Situ Gede." },
    { "en": "Merangin (Jambi)?", "id": "Danau Pauh." },
    { "en": "Cirebon (Jawa Barat)?", "id": "Situ Sedong." },
    { "en": "Buru (Maluku)?", "id": "Danau Rana." },
    { "en": "Mamuju (Sulawesi Barat)?", "id": "Danau Tadush." },
    { "en": "Sumbawa (Nusa Tenggara Barat)?", "id": "Danau Moyo." },
    { "en": "Pasuruan (Jawa Timur)?", "id": "Danau Ranu Grati." },
    { "en": "Sumedang (Jawa Barat)?", "id": "Waduk Jatigede." },
    { "en": "Gayo Lues (Aceh)?", "id": "Danau Ise-Ise." },
    { "en": "Pesisir Barat (Lampung)?", "id": "Danau Belibis." },
    { "en": "Malinau (Kalimantan Utara)?", "id": "Danau Laban." },
    { "en": "Donggala (Sulawesi Tengah)?", "id": "Danau Rano." },
    { "en": "Kolaka Utara (Sulawesi Tenggara)?", "id": "Danau Biru." },
    { "en": "Samosir (Sumatera Utara)?", "id": "Danau Sidihoni." },
    { "en": "Tapanuli Utara (Sumatera Utara)?", "id": "Danau Aek Natonang." },
    { "en": "Banjarnegara (Jawa Tengah)?", "id": "Telaga Merdada." },
    { "en": "Sukabumi (Jawa Barat)?", "id": "Situ Gunung." },
    { "en": "Pacitan (Jawa Timur)?", "id": "Telaga Guyang." },
    { "en": "Situbondo (Jawa Timur)?", "id": "Waduk Bajulmati." },
    { "en": "Bantul (DI Yogyakarta)?", "id": "Telaga Selopamioro." },
    { "en": "Manggarai (Nusa Tenggara Timur)?", "id": "Danau Sano Nggoang." },
    { "en": "Pohuwato (Gorontalo)?", "id": "Danau Popaya." },
    { "en": "Tidore Kepulauan (Maluku Utara)?", "id": "Danau Tugulufa." },
    { "en": "Manokwari (Papua Barat)?", "id": "Danau Anggi." },
    { "en": "Agam (Sumatera Barat)?", "id": "Danau Tarusan Kamang." },
    { "en": "Luwu (Sulawesi Selatan)?", "id": "Danau Sidenreng." },
    { "en": "Tanah Bumbu (Kalimantan Selatan)?", "id": "Danau Seran." },
    { "en": "Malang (Jawa Timur)?", "id": "Waduk Selorejo." },
    { "en": "Rembang (Jawa Tengah)?", "id": "Waduk Panohan." },
    { "en": "Palopo (Sulawesi Selatan)?", "id": "Danau Matano." },
    { "en": "Boven Digoel (Papua Selatan)?", "id": "Danau Mbuta." },
    { "en": "Lebong (Bengkulu)?", "id": "Danau Tes." },
    { "en": "Majalengka (Jawa Barat)?", "id": "Situ Sangiang." },
    { "en": "Rote Ndao (Nusa Tenggara Timur)?", "id": "Danau Laut Mati." },
    { "en": "Tegal (Jawa Tengah)?", "id": "Waduk Cacaban." },
    { "en": "Kutai Timur (Kalimantan Timur)?", "id": "Danau Telaga Bidadari." },
    { "en": "Toli-Toli (Sulawesi Tengah)?", "id": "Danau Tuinan." },
    { "en": "Enrekang (Sulawesi Selatan)?", "id": "Danau Pemanikan." },
    { "en": "Kepulauan Sula (Maluku Utara)?", "id": "Danau Kabau." },
    { "en": "Fakfak (Papua Barat)?", "id": "Danau Laheria." },
    { "en": "Karanganyar (Jawa Tengah)?", "id": "Telaga Madirda." },
    { "en": "Kebumen (Jawa Tengah)?", "id": "Waduk Sempor." },
    { "en": "Aceh Singkil (Aceh)?", "id": "Danau Anak Laut." },
    { "en": "Lampung Timur (Lampung)?", "id": "Danau Way Jepara." },
    { "en": "Konawe (Sulawesi Tenggara)?", "id": "Danau Wawotobi." },
    { "en": "Sikka (Nusa Tenggara Timur)?", "id": "Danau Koli." },
    { "en": "Pulau Wetar (Maluku Barat Daya)?", "id": "Danau Tihu." },
    { "en": "Tambrauw (Papua Barat Daya)?", "id": "Danau Framu." },
    { "en": "Asahan (Sumatera Utara)?", "id": "Danau Teratai." },
    { "en": "Musi Banyuasin (Sumatera Selatan)?", "id": "Danau Ulak Lia." },
    { "en": "Ciamis (Jawa Barat)?", "id": "Situ Lengkong Panjalu." },
    { "en": "Lamongan (Jawa Timur)?", "id": "Waduk Gondang." },
    { "en": "Sidoarjo (Jawa Timur)?", "id": "Telaga Wahyu." },
    { "en": "Pekalongan (Jawa Tengah)?", "id": "Telaga Sigebyar." },
    { "en": "Sidenreng Rappang (Sulawesi Selatan)?", "id": "Danau Sidenreng." },
    { "en": "Tanggamus (Lampung)?", "id": "Danau Hijau." },
    { "en": "Banjar (Kalimantan Selatan)?", "id": "Danau Biru Pengaron." },
    { "en": "Minahasa (Sulawesi Utara)?", "id": "Danau Linow." },
    { "en": "Jayawijaya (Papua Pegunungan)?", "id": "Danau Lembah Baliem." },
    { "en": "Brebes (Jawa Tengah)?", "id": "Waduk Malahayu." },
    { "en": "Grobogan (Jawa Tengah)?", "id": "Waduk Kedung Ombo." },
    { "en": "Kuningan (Jawa Barat)?", "id": "Telaga Remis." },
    { "en": "Bogor (Jawa Barat)?", "id": "Situ Cikaret." },
    { "en": "Palangkaraya (Kalimantan Tengah)?", "id": "Danau Tahai." },
    { "en": "Landak (Kalimantan Barat)?", "id": "Danau Laet." },
    { "en": "Tana Toraja (Sulawesi Selatan)?", "id": "Danau Makale." },
    { "en": "Kepahiang (Bengkulu)?", "id": "Danau Suro." },
    { "en": "Muaro Jambi (Jambi)?", "id": "Danau Tangkas." },
    { "en": "Padang Pariaman (Sumatera Barat)?", "id": "Danau Hijau." },
    { "en": "Rejang Lebong (Bengkulu)?", "id": "Danau Mas Harun Bastari." },
    { "en": "Madiun (Jawa Timur)?", "id": "Waduk Bening Widas." },
    { "en": "Tulungagung (Jawa Timur)?", "id": "Waduk Wonorejo." },
    { "en": "Pemalang (Jawa Tengah)?", "id": "Telaga Sidringo." },
    { "en": "Cianjur (Jawa Barat)?", "id": "Situ Cibeureum." },
    { "en": "Singkawang (Kalimantan Barat)?", "id": "Danau Biru Singkawang." },
    { "en": "Kotawaringin Timur (Kalimantan Tengah)?", "id": "Danau Ganting." },
    { "en": "Bolaang Mongondow (Sulawesi Utara)?", "id": "Danau Tondok." },
    { "en": "Kepulauan Talaud (Sulawesi Utara)?", "id": "Danau Baranusa." },
    { "en": "Lembata (Nusa Tenggara Timur)?", "id": "Danau Asmara." },
    { "en": "Aceh Jaya (Aceh)?", "id": "Danau Laut Nie." },
    { "en": "Langkat (Sumatera Utara)?", "id": "Danau Paya Mabar." },
    { "en": "Indragiri Hulu (Riau)?", "id": "Danau Menduyan." },
    { "en": "Nganjuk (Jawa Timur)?", "id": "Waduk Perning." },
    { "en": "Sragen (Jawa Tengah)?", "id": "Waduk Kembangan." },
    { "en": "Indramayu (Jawa Barat)?", "id": "Situ Bolang." },
    { "en": "Subang (Jawa Barat)?", "id": "Situ Cigayonggong." },
    { "en": "Tomohon (Sulawesi Utara)?", "id": "Danau Tampusu." },
    { "en": "Kepulauan Anambas (Kepulauan Riau)?", "id": "Danau Biru (Anambas)." },
    { "en": "Ketapang (Kalimantan Barat)?", "id": "Danau Belibis (Ketapang)." },
    { "en": "Polewali Mandar (Sulawesi Barat)?", "id": "Rawa Bangun." },
    { "en": "Soppeng (Sulawesi Selatan)?", "id": "Danau Ompo." },
    { "en": "Pidie (Aceh)?", "id": "Waduk Rajui." },
    { "en": "Lima Puluh Kota (Sumatera Barat)?", "id": "Danau Ayia." },
    { "en": "Pasaman Barat (Sumatera Barat)?", "id": "Danau Laut Tinggal." },
    { "en": "Blitar (Jawa Timur)?", "id": "Telaga Rambut Monte." },
    { "en": "Purworejo (Jawa Tengah)?", "id": "Waduk Wadaslintang." },
    { "en": "Karimun (Kepulauan Riau)?", "id": "Telaga Bidadari." },
    { "en": "Sintang (Kalimantan Barat)?", "id": "Danau Jemelak." },
    { "en": "Maros (Sulawesi Selatan)?", "id": "Danau Kassi Kebo." },
    { "en": "Boalemo (Gorontalo)?", "id": "Danau Limboto (sisi barat)." },
    { "en": "Pegunungan Arfak (Papua Barat)?", "id": "Danau Gigantic." },
    { "en": "Deiyai (Papua Tengah)?", "id": "Danau Tigi." }



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
