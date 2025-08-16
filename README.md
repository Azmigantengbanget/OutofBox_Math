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



  { "en": "Jika Anda memiliki 10 apel dan Anda memberikan 2 kepada teman Anda, berapa banyak apel yang Anda miliki?", "id": "Tetap 10, Anda hanya memberikannya." },
  { "en": "Angka apa yang jika dibagi 2, 3, 4, 5 sisanya 1?", "id": "Enam puluh satu (61)." },
  { "en": "Berapa kali Anda bisa mengurangi 10 dari 100?", "id": "Hanya satu kali saja." },
  { "en": "Sebuah jam kehilangan 10 menit setiap jam. Jika jam menunjukkan jam 3 sore, jam berapa sebenarnya ketika jam itu menunjukkan jam 4 sore?", "id": "Sebenarnya jam 4:10 sore." },
  { "en": "Urutan berikutnya dari 1, 11, 21, 1211, 111221, ...?", "id": "312211." },
  { "en": "Seorang pria berumur dua kali lipat dari anaknya. 20 tahun yang lalu, usianya tiga kali lipat. Berapa umur mereka sekarang?", "id": "Ayah 80 tahun, anak 40 tahun." },
  { "en": "Berapa jumlah sudut dalam sebuah bintang berujung lima?", "id": "180 derajat." },
  { "en": "Jika 3 kucing menangkap 3 tikus dalam 3 menit, berapa lama 100 kucing menangkap 100 tikus?", "id": "Tiga menit." },
  { "en": "Ada 8 jeruk di keranjang, dan Anda mengambil 3. Berapa banyak jeruk yang Anda miliki?", "id": "Tiga jeruk yang Anda ambil." },
  { "en": "Angka romawi apa yang tidak bisa ditulis?", "id": "Nol." },
  { "en": "Bagaimana cara mendapatkan 1000 dengan delapan angka 8?", "id": "888 + 88 + 8 + 8 + 8." },
  { "en": "Mana yang lebih berat, satu pon bulu atau satu pon emas?", "id": "Keduanya sama berat." },
  { "en": "Sebuah buku memiliki halaman 1-200. Berapa kali angka '1' muncul?", "id": "140 kali." },
  { "en": "Jika ayam setengah bertelur 1,5 telur dalam 1,5 hari, berapa telur yang dihasilkan satu ayam dalam 6 hari?", "id": "Enam telur." },
  { "en": "Berapa banyak sisi yang dimiliki lingkaran?", "id": "Dua, sisi dalam dan luar." },
  { "en": "Angka berapa yang menjadi setengah dari seperempat dari sepersepuluh dari 400?", "id": "Lima." },
  { "en": "Jika Anda menulis semua angka dari 300 hingga 400, berapa kali Anda menulis angka 3?", "id": "120 kali." },
  { "en": "Seorang petani memiliki 17 domba. Semuanya mati kecuali 9. Berapa yang tersisa?", "id": "Sembilan domba." },
  { "en": "Apa satu-satunya bilangan yang namanya memiliki jumlah huruf yang sama dengan nilainya?", "id": "Empat." },
  { "en": "Kereta listrik bergerak ke utara. Ke arah mana asapnya berhembus?", "id": "Kereta listrik tidak punya asap." },
  { "en": "Berapa banyak '9' yang ada antara 1 dan 100?", "id": "Dua puluh." },
  { "en": "Bagaimana cara membuat angka tujuh menjadi genap?", "id": "Hapus huruf 'S' dari 'SEVEN'." },
  { "en": "5 saudara laki-laki masing-masing punya 1 saudara perempuan. Berapa total saudara kandung?", "id": "Enam saudara kandung." },
  { "en": "Apa tiga bilangan bulat positif berbeda yang jumlah dan hasil kalinya sama?", "id": "Satu, dua, dan tiga." },
  { "en": "Setelah melempar koin 10 kali kepala, berapa probabilitas lemparan berikutnya ekor?", "id": "Lima puluh persen." },
  { "en": "Seorang pria berkata, 'Ayah orang di potret ini adalah anak ayah saya.' Potret siapa itu?", "id": "Potret anaknya." },
  { "en": "6 apel untuk 6 orang, tapi satu apel tersisa di keranjang. Bagaimana?", "id": "Orang terakhir mengambil keranjang berisi apel." },
  { "en": "Berapa jumlah dari 1+2+3+...+100?", "id": "5050." },
  { "en": "Seorang anak laki-laki berusia 4 tahun. Kakak perempuannya tiga kali lebih tua. Kapan usia kakaknya dua kali lebih tua?", "id": "Saat kakaknya berumur 16 tahun." },
  { "en": "Botol dan gabus berharga $1,10. Botol $1 lebih mahal dari gabus. Berapa harga gabus?", "id": "$0,05 atau lima sen." },
  { "en": "Berapa 1/2 dari 2/3 dari 3/4 ... dari 9/10 dari 1000?", "id": "Seratus." },
  { "en": "Bagaimana cara mengukur 4 liter air hanya dengan ember 3L dan 5L?", "id": "Isi 5L, tuang ke 3L, sisa 2L." },
  { "en": "Berapa banyak bulan yang memiliki 28 hari?", "id": "Semua dua belas bulan." },
  { "en": "Ada dua ayah dan dua anak, tetapi hanya ada tiga orang. Bagaimana?", "id": "Kakek, ayah, dan anak." },
  { "en": "Mana yang benar: 'sembilan tambah lima adalah tiga belas' atau 'sembilan tambah lima sama dengan tiga belas'?", "id": "Keduanya salah, hasilnya 14." },
  { "en": "Dokter memberi 3 pil, diminum setiap setengah jam. Berapa lama pil habis?", "id": "Satu jam." },
  { "en": "Beberapa bulan punya 31 hari, yang lain 30. Berapa yang punya 28 hari?", "id": "Semua bulan memilikinya." },
  { "en": "Petak bunga lili menutupi danau dalam 48 hari, ukurannya berlipat ganda setiap hari. Kapan danau setengah tertutup?", "id": "Pada hari ke-47." },
  { "en": "Berapa digit terakhir dari 7 pangkat 100?", "id": "Satu." },
  { "en": "8 orang butuh 10 jam membangun tembok. Berapa lama waktu yang dibutuhkan 4 orang?", "id": "Tidak ada, tembok sudah jadi." },
  { "en": "Dua orang bermain 5 pertandingan catur, masing-masing menang jumlah yang sama, tidak ada seri. Bagaimana?", "id": "Mereka tidak saling melawan." },
  { "en": "Angka berapa yang tidak memiliki angka Romawi?", "id": "Nol." },
  { "en": "Jika kemarin adalah besok, maka hari ini adalah Jumat. Hari apa ini?", "id": "Rabu." },
  { "en": "Berapa banyak ujung yang dimiliki oleh 4 kubus?", "id": "Tiga puluh dua ujung." },
  { "en": "Jika Anda memiliki semangkuk dengan enam apel dan Anda mengambil empat, berapa banyak yang Anda miliki?", "id": "Empat apel." },
  { "en": "Saya bilangan ganjil. Ambil satu huruf, saya menjadi genap. Angka berapakah saya?", "id": "Seven (tujuh)." },
  { "en": "Ayah Tom punya lima putra: 10, 20, 30, 40, ... Siapa nama putra kelima?", "id": "Tom." },
  { "en": "Urutan berikutnya dari O, T, T, F, F, S, S, ...?", "id": "E (inisial nama angka)." },
  { "en": "Jika pesawat jatuh di perbatasan AS dan Kanada, di mana penyintas dikuburkan?", "id": "Penyintas tidak dikuburkan." },
  { "en": "Bagaimana Anda melempar telur ke lantai beton tanpa memecahkannya?", "id": "Lantai beton tidak mudah pecah." },
  { "en": "Apa yang memiliki mata tetapi tidak bisa melihat?", "id": "Jarum." },
  { "en": "Jika 1 telur butuh 1 menit untuk direbus, berapa lama untuk 5 telur?", "id": "Satu menit jika bersamaan." },
  { "en": "Berapa jumlah total kotak di papan catur?", "id": "204 kotak." },
  { "en": "Apa yang selalu datang tetapi tidak pernah tiba?", "id": "Besok." },
  { "en": "Bilangan prima genap satu-satunya?", "id": "Dua." },
  { "en": "Jika usia saya dibalik, itu usia anak saya. Tahun lalu, saya dua kali usianya. Berapa usia kami?", "id": "73 dan 37." },
  { "en": "Bagaimana cara menulis 4 di antara 5?", "id": "F(IV)E." },
  { "en": "Di ruangan ada 4 sudut, di setiap sudut ada 1 kucing, di depan setiap kucing ada 3 kucing. Berapa banyak kucing?", "id": "Empat kucing." },
  { "en": "Berapa banyak lubang dalam satu permen polo?", "id": "Satu lubang." },
  { "en": "Angka berapa yang jika dikalikan angka lain hasilnya selalu sama?", "id": "Nol." },
  { "en": "Saya punya dua koin senilai 30 sen, salah satunya bukan nikel. Koin apa itu?", "id": "Koin 25 sen dan nikel." },
  { "en": "Kapan 11 + 3 = 2?", "id": "Pada jam." },
  { "en": "Berapa banyak hewan dari setiap jenis yang dibawa Musa ke bahtera?", "id": "Tidak ada, itu bahtera Nuh." },
  { "en": "Jika ada 12 ikan dan setengahnya tenggelam, berapa yang tersisa?", "id": "Dua belas, ikan tidak tenggelam." },
  { "en": "Ada 10 burung di pohon. Pemburu menembak satu. Berapa yang tersisa?", "id": "Tidak ada, yang lain terbang." },
  { "en": "Apa yang dimulai dengan T, diakhiri dengan T, dan memiliki T di dalamnya?", "id": "Teapot (teko)." },
  { "en": "Berapa banyak huruf dalam 'alfabet'?", "id": "Tujuh huruf." },
  { "en": "Apa yang bisa Anda pegang di tangan kiri tetapi tidak di tangan kanan?", "id": "Tangan kanan Anda." },
  { "en": "Jika 2 orang 2 hari menggali 2 lubang, berapa lama 1 orang menggali 1 lubang?", "id": "Dua hari." },
  { "en": "Apa yang punya leher tanpa kepala, dan tubuh tanpa kaki?", "id": "Botol." },
  { "en": "Berapa banyak sisi pada sebuah rumah?", "id": "Dua, sisi dalam dan luar." },
  { "en": "Anda menyetir bus. Tiga orang turun, dua naik. Lima turun, empat naik. Siapa nama sopirnya?", "id": "Nama Anda sendiri." },
  { "en": "Berapa banyak ulang tahun yang dimiliki rata-rata orang?", "id": "Satu setiap tahun." },
  { "en": "Apa yang harus ditambahkan ke 111 agar menjadi 6?", "id": "Tidak ada, balikkan angkanya jadi 9." },
  { "en": "Mobil balap melaju 100 lap dalam 2 jam. Berapa kecepatan rata-ratanya?", "id": "Tergantung panjang satu lap." },
  { "en": "Berapa nilai dari (x-a)(x-b)...(x-z)?", "id": "Nol, karena ada faktor (x-x)." },
  { "en": "Apa yang naik tapi tidak pernah turun?", "id": "Usia Anda." },
  { "en": "Seorang pria berlayar dari timur ke barat mengelilingi dunia. Berapa kali kapalnya menabrak daratan?", "id": "Tidak ada, dia berlayar." },
  { "en": "Siput memanjat tiang 10 kaki, naik 3 kaki siang hari, turun 2 kaki malam hari. Berapa hari?", "id": "Delapan hari." },
  { "en": "Apa yang penuh lubang tapi masih menampung air?", "id": "Spons." },
  { "en": "Berapa banyak tanah dalam lubang berukuran 3x2x6 meter?", "id": "Tidak ada tanah dalam lubang." },
  { "en": "Seorang pria punya 20 sapi. Semuanya mati kecuali 11. Berapa yang tersisa?", "id": "Sebelas sapi." },
  { "en": "Apa yang ada di tengah-tengah 'dunia'?", "id": "Huruf 'n'." },
  { "en": "Jika Anda menjatuhkan topi kuning ke Laut Merah, topi itu menjadi apa?", "id": "Menjadi basah." },
  { "en": "Dua gadis lahir dari ibu yang sama, pada waktu yang sama, tapi bukan kembar. Bagaimana?", "id": "Mereka kembar tiga atau lebih." },
  { "en": "Kapan Anda bisa menambahkan 2 ke 11 dan mendapatkan 1?", "id": "Di jam (11 + 2 = 1)." },
  { "en": "Seorang pria berjalan dalam hujan tanpa payung, tapi rambutnya tidak basah. Bagaimana?", "id": "Dia botak." },
  { "en": "Apa yang memiliki 13 hati, tetapi tidak ada organ lain?", "id": "Satu set kartu remi." },
  { "en": "Berapa banyak jari yang Anda miliki di dua tangan?", "id": "Delapan jari dan dua ibu jari." },
  { "en": "Seorang pria mendorong mobilnya ke hotel dan kehilangan kekayaannya. Apa yang terjadi?", "id": "Dia sedang bermain Monopoli." },
  { "en": "Bagaimana cara membuat delapan segitiga dengan empat batang korek api?", "id": "Bentuk piramida tiga dimensi." },
  { "en": "Urutan berikutnya dari J, F, M, A, M, J, J, ...?", "id": "A (inisial nama bulan)." },
  { "en": "Berapa banyak hewan dari setiap jenis kelamin yang dibawa Adam ke bahtera?", "id": "Tidak ada, Adam tidak punya bahtera." },
  { "en": "Apa yang bisa Anda pecahkan bahkan tanpa menyentuhnya?", "id": "Sebuah janji." },
  { "en": "Berapa langkah memasukkan gajah ke dalam lemari es?", "id": "Tiga langkah." },
  { "en": "Berapa langkah memasukkan jerapah ke dalam lemari es?", "id": "Empat langkah." },
  { "en": "Raja hutan mengadakan pertemuan. Semua hewan datang kecuali satu. Hewan apa?", "id": "Jerapah, masih di lemari es." },
  { "en": "Anda harus menyeberangi sungai penuh buaya. Bagaimana Anda melakukannya?", "id": "Berenang saja, buaya sedang rapat." },
  { "en": "Jika 2 adalah perusahaan dan 3 adalah kerumunan, apa itu 4 dan 5?", "id": "Sembilan." },
  { "en": "Apa angka selanjutnya dalam pola ini: 1, 4, 9, 16, 25, ...?", "id": "36 (bilangan kuadrat)." },
  { "en": "Saya memiliki kota tanpa orang, hutan tanpa pohon, dan air tanpa ikan. Apa saya?", "id": "Sebuah peta." },
  { "en": "Angka apa yang nilainya tidak berubah jika dibalik?", "id": "Enam puluh sembilan (69) dan delapan puluh delapan (88)." },
  { "en": "Apa yang bisa diukur tapi tidak punya panjang, lebar, atau tinggi?", "id": "Suhu." },
  { "en": "Jika 7 orang bertemu dan masing-masing berjabat tangan sekali, berapa total jabat tangan?", "id": "Dua puluh satu kali." },
  { "en": "Sebuah balapan, Anda menyalip orang di posisi kedua. Posisi Anda sekarang?", "id": "Posisi kedua." },
  { "en": "Urutan berikutnya dari 2, 3, 5, 7, 11, ...?", "id": "13 (urutan bilangan prima)." },
  { "en": "Bagaimana cara membuat persamaan 5+5+5=550 benar dengan satu garis?", "id": "Ubah tanda '+' pertama menjadi '4'." },
  { "en": "Berapa banyak sisi yang dimiliki sebuah tabung?", "id": "Tiga sisi." },
  { "en": "Seorang pria meninggal karena usia tua pada ulang tahunnya yang ke-25. Bagaimana bisa?", "id": "Dia lahir pada 29 Februari." },
  { "en": "Berapa hasil kali semua angka di keypad telepon?", "id": "Nol (karena ada angka 0)." },
  { "en": "Jika dibutuhkan 6 pria 1 jam untuk menggali 1 lubang, berapa lama 1 pria menggali setengah lubang?", "id": "Tidak ada yang namanya setengah lubang." },
  { "en": "Kapan 99 lebih besar dari 100?", "id": "Saat menggunakan microwave (99 detik > 1 menit)." },
  { "en": "Mana yang datang lebih dulu, ayam atau telur?", "id": "Telur, dinosaurus bertelur sebelum ada ayam." },
  { "en": "Bagaimana cara menjatuhkan jarum dari ketinggian 1 meter tanpa menyentuh lantai?", "id": "Jatuhkan dari ketinggian lebih dari 1 meter." },
  { "en": "Apa yang memiliki 4 mata (i) tetapi tidak bisa melihat?", "id": "Mississippi." },
  { "en": "Jika dibutuhkan 12 tukang 12 jam untuk membangun rumah, berapa lama 6 tukang membangunnya?", "id": "Tidak ada, rumahnya sudah dibangun." },
  { "en": "Jika 1=5, 2=10, 3=15, 4=20, maka 5=...?", "id": "1 (karena pada awalnya 1=5)." },
  { "en": "Berapa banyak kotak pada papan catur 3x3?", "id": "Empat belas kotak." },
  { "en": "Mengapa angka 6 takut pada angka 7?", "id": "Karena seven eight nine (7 8 9)." },
  { "en": "Angka mana yang tidak bisa dibagi dua untuk menghasilkan bilangan bulat?", "id": "Semua bilangan ganjil." },
  { "en": "Bagaimana cara membagi 188 menjadi dua bagian yang sama persis?", "id": "Potong secara horizontal, menjadi 100 di atas 100." },
  { "en": "Jika sebuah bola memantul kembali ke setengah ketinggian jatuhnya, berapa total jarak yang ditempuh setelah dijatuhkan dari 10m dan berhenti?", "id": "30 meter." },
  { "en": "Berapa banyak angka 0 di akhir dari 100 faktorial (100!)?", "id": "Dua puluh empat angka nol." },
  { "en": "Seorang pria berusia 40 tahun. Berapa banyak ulang tahun yang telah ia lewati?", "id": "Empat puluh ulang tahun." },
  { "en": "Apa yang memiliki jari dan ibu jari, tetapi tidak hidup?", "id": "Sarung tangan." },
  { "en": "Apa angka selanjutnya dalam urutan ini: 1, 1, 2, 3, 5, 8, ...?", "id": "13 (deret Fibonacci)." },
  { "en": "Jika dibutuhkan 8 menit untuk memasak 1 jagung, berapa lama untuk 4 jagung?", "id": "Delapan menit jika dimasak bersamaan." },
  { "en": "Jika Anda menulis semua angka dari 1 sampai 99, berapa kali Anda menulis angka 7?", "id": "Dua puluh kali." },
  { "en": "Apa yang ada di depan wanita dan di belakang sapi?", "id": "Huruf 'W'." },
  { "en": "Berapa banyak binatang yang saya bawa jika saya membawa 2 anjing, 3 kucing, dan 1 kuda?", "id": "Satu hewan (kuda)." },
  { "en": "Jika Anda memiliki 20 permen dan teman Anda meminta 3, berapa yang tersisa?", "id": "20 permen, Anda belum memberikannya." },
  { "en": "Seberapa jauh Anda bisa berjalan ke dalam hutan?", "id": "Setengah jalan, lalu Anda berjalan keluar." },
  { "en": "Kapan 2 + 11 bisa sama dengan 1?", "id": "Di jam." },
  { "en": "Saya punya dua koin berjumlah 55 sen. Salah satunya bukan koin 50 sen. Koin apa itu?", "id": "Koin 50 sen dan 5 sen." },
  { "en": "Berapa jumlah minimum kaus kaki yang harus Anda ambil dari laci berisi 5 kaus kaki biru dan 8 kaus kaki merah untuk mendapatkan sepasang?", "id": "Tiga kaus kaki." },
  { "en": "Angka tiga digit apa yang digit keduanya empat kali lebih besar dari digit ketiga dan digit pertama tiga kurang dari digit kedua?", "id": "141." },
  { "en": "Jika ada 4 anak dan 4 apel, bagaimana Anda bisa membagi apel secara merata sehingga 1 apel tersisa di keranjang?", "id": "Berikan keranjang dengan apel terakhirnya." },
  { "en": "Jika Anda mengalikan angka ini dengan angka lain, jawabannya akan selalu sama. Angka berapa ini?", "id": "Nol." },
  { "en": "Apa yang terjadi hanya sekali dalam semenit, dua kali dalam sekejap, tetapi tidak pernah dalam seribu tahun?", "id": "Huruf 'M'." },
  { "en": "Bilangan apa yang bertambah nilainya jika setengahnya dibalik?", "id": "Angka 8." },
  { "en": "Ada 30 sapi di ladang, dan 28 ayam. Berapa banyak yang tidak?", "id": "Sepuluh (dari 'twenty-eight')." },
  { "en": "Apa yang memiliki lebih banyak kunci tetapi tidak bisa membuka satu pintu pun?", "id": "Piano." },
  { "en": "Gunakan empat angka 9 untuk membuat 100.", "id": "99 + 9/9." },
  { "en": "Berapa banyak potongan yang bisa Anda dapatkan dari sebuah kue dengan 3 potongan lurus?", "id": "Tujuh potongan." },
  { "en": "Jika hari sebelum kemarin adalah 21, maka hari setelah besok adalah?", "id": "25." },
  { "en": "Saya adalah bilangan tiga digit. Digit puluhan saya adalah 5 lebih dari digit satuan saya. Digit ratusan saya 8 kurang dari digit puluhan saya. Siapakah saya?", "id": "194." },
  { "en": "Seorang pria bertaruh $100 dengan temannya bahwa dia bisa memprediksi skor pertandingan sepak bola sebelum dimulai. Dia menang. Bagaimana?", "id": "Skor sebelum pertandingan selalu 0-0." },
  { "en": "Apa itu pusat gravitasi?", "id": "Huruf V." },
  { "en": "Angka berapa yang jika Anda ambil setengahnya, tidak ada yang tersisa?", "id": "Angka 8 (setengahnya adalah 0)." },
  { "en": "Jika Z=26 dan Y=25, maka A+B+C=?", "id": "Enam (1+2+3)." },
  { "en": "Jika seorang pria dapat memakan satu pizza dalam 15 menit, berapa lama waktu yang dibutuhkan tiga pria untuk memakan tiga pizza?", "id": "Lima belas menit." },
  { "en": "Apa yang lebih berguna saat rusak?", "id": "Telur." },
  { "en": "Gunakan empat angka 7 untuk membuat 100.", "id": "(7/.7) * (7/.7) = 100." },
  { "en": "Jika Anda membutuhkan 1 jam untuk mengecat pagar, mengapa 2 orang butuh 2 jam?", "id": "Mereka mengobrol." },
  { "en": "Bagaimana Anda bisa mendapatkan 23 menggunakan hanya angka 2?", "id": "22 + 2/2." },
  { "en": "Jika ada 5 apel dan Anda mengambil 3, berapa yang Anda miliki?", "id": "Tiga apel." },
  { "en": "Keluarga Smith memiliki 4 anak perempuan. Masing-masing memiliki seorang saudara laki-laki. Berapa jumlah anak?", "id": "Lima anak." },
  { "en": "Angka apa yang terletak di antara 7 dan 8?", "id": "Tanda koma atau desimal." },
  { "en": "Jika dibutuhkan 10 orang 10 hari untuk membangun 10 rumah, berapa lama 1 orang membangun 1 rumah?", "id": "Sepuluh hari." },
  { "en": "Jika ada 12 perangko dalam selusin, berapa banyak perangko 2 sen dalam selusin?", "id": "Dua belas." },
  { "en": "Seorang pria membangun rumah persegi dengan semua sisi menghadap ke selatan. Seekor beruang lewat. Apa warna beruang itu?", "id": "Putih, rumahnya di Kutub Utara." },
  { "en": "Jika dua adalah pasangan dan tiga adalah trio, apa itu empat dan lima?", "id": "Sembilan." },
  { "en": "Saya memiliki 6 koin senilai 86 sen. Koin apa yang saya miliki?", "id": "1 koin 50, 1 koin 25, 2 koin 5, 2 koin 1." },
  { "en": "Apa pertanyaan yang tidak bisa Anda jawab 'ya'?", "id": "Apakah kamu sudah tidur?" },
  { "en": "Bagaimana cara membuat 7 menjadi genap?", "id": "Hilangkan huruf S." },
  { "en": "Jika Anda memiliki satu korek api di ruangan gelap dengan lilin, lampu minyak, dan tungku kayu, apa yang Anda nyalakan dulu?", "id": "Korek api." },
  { "en": "Di mana Jumat datang sebelum Kamis?", "id": "Di dalam kamus." },
  { "en": "Seorang anak menjatuhkan mainannya di atas kopi. Bagaimana mainannya tidak basah?", "id": "Itu adalah biji kopi." },
  { "en": "Apa yang naik dan turun tapi tidak pernah bergerak?", "id": "Tangga." },
  { "en": "Berapa banyak lubang di kemeja ini: lubang leher, dua lengan, dan lubang bawah, ditambah dua lubang di depan dan satu di belakang?", "id": "Delapan lubang (termasuk yang tembus)." },
  { "en": "Jika seorang penjaga malam meninggal di siang hari, apakah dia mendapat pensiun?", "id": "Tidak, orang mati tidak mendapat pensiun." },
  { "en": "Saya adalah bilangan prima. Jika Anda menambahkan 's' pada saya, saya menjadi jamak. Siapakah saya?", "id": "One (S-one)." },
  { "en": "Apa yang memiliki 88 kunci tetapi tidak dapat membuka satu pintu pun?", "id": "Piano." },
  { "en": "Berapa banyak telur yang bisa Anda letakkan di keranjang kosong?", "id": "Satu telur, setelah itu keranjang tidak kosong." },
  { "en": "Bagaimana cara agar 29-1=30?", "id": "Gunakan angka Romawi: XXIX - I = XXX." },
  { "en": "Jika Anda memiliki 5 ikan di akuarium dan 2 mati, berapa banyak yang tersisa?", "id": "Lima, yang mati masih di sana." },
  { "en": "Berapa banyak angka dari 1-100 yang tidak mengandung huruf 'A'?", "id": "Nol. (One, Two, Three, ...)" },
  { "en": "Jika dibutuhkan 3 menit untuk merebus satu telur, berapa lama untuk tiga telur?", "id": "Tiga menit jika bersamaan." },
  { "en": "Mana yang lebih berat: satu ton batu bata atau satu ton bulu?", "id": "Keduanya sama berat." },
  { "en": "Berapa banyak hadiah yang diberikan dalam lagu 'The Twelve Days of Christmas'?", "id": "364 hadiah." },
  { "en": "Seorang pria membeli anjing seharga $60, menjualnya seharga $70, membelinya kembali seharga $80, dan menjualnya seharga $90. Berapa untungnya?", "id": "$20." },
  { "en": "Apa tiga bilangan bulat positif yang memberikan hasil yang sama ketika dikalikan dan dijumlahkan?", "id": "Satu, dua, dan tiga." },
  { "en": "Berapa banyak hewan dari setiap jenis yang dibawa Nuh ke dalam bahtera?", "id": "Dua dari setiap jenis." },
  { "en": "Jika ada 7 anjing di halaman, berapa banyak kaki yang ada?", "id": "Dua puluh delapan kaki." },
  { "en": "Berapa banyak 'F' dalam kalimat: 'Finished files are the result of years of scientific study combined with the experience of years'?", "id": "Enam." },
  { "en": "Seorang pria berusia dua kali lipat dari putrinya. Total usia mereka adalah 66. Berapa usia mereka?", "id": "Pria 44, putri 22." },
  { "en": "Apa yang memiliki satu kepala, satu kaki, dan empat kaki?", "id": "Tempat tidur." },
  { "en": "Jika Anda menulis 1 sampai 10, angka mana yang akan Anda lewatkan?", "id": "Tidak ada." },
  { "en": "Jika sebuah kapal memiliki tangga dengan 10 anak tangga, dan air naik 2 anak tangga, berapa banyak yang tersisa?", "id": "Sepuluh, kapal ikut naik." },
  { "en": "Apa yang lebih besar dari Tuhan, lebih jahat dari iblis, orang miskin memilikinya, orang kaya membutuhkannya, dan jika Anda memakannya, Anda akan mati?", "id": "Tidak ada." },
  { "en": "Berapa banyak digit dalam sistem bilangan biner?", "id": "Dua (0 dan 1)." },
  { "en": "Berapa banyak bulan dalam setahun yang memiliki 31 hari?", "id": "Tujuh bulan." },
  { "en": "Apa yang memiliki banyak gigi tetapi tidak bisa makan?", "id": "Sisir." },
  { "en": "Jika seekor gagak di pagi hari, apa yang di malam hari?", "id": "Huruf 'G'." },
  { "en": "Seorang pria menggambarkan putrinya, 'Mereka semua pirang, tapi dua; semua berambut cokelat, tapi dua; dan semua berambut merah, tapi dua.' Berapa banyak putrinya?", "id": "Tiga (satu pirang, satu cokelat, satu merah)." },
  { "en": "Jika sebuah jam berdentang 6 kali dalam 5 detik, berapa lama untuk berdentang 12 kali?", "id": "Sebelas detik." },
  { "en": "Seorang pria melihat perahu penuh orang. Namun tidak ada satu orang pun di perahu itu. Bagaimana?", "id": "Semua orang di perahu sudah menikah." },
  { "en": "Jika dibutuhkan 5 mesin 5 menit untuk membuat 5 widget, berapa lama 100 mesin membuat 100 widget?", "id": "Lima menit." },
  { "en": "Berapa banyak sisi pada sebuah pensil heksagonal yang belum diasah?", "id": "Delapan sisi." },
  { "en": "Apa yang bisa berjalan tapi tidak punya kaki?", "id": "Waktu." },
  { "en": "Berapa nilai dari 1 + 1 - 1 + 1 - 1 + ... selamanya?", "id": "Tidak terdefinisi atau ambigu." },
  { "en": "Anda memiliki 12 bola, satu berbeda berat. Berapa penimbangan minimal untuk menemukannya?", "id": "Tiga kali penimbangan dengan timbangan seimbang." },
  { "en": "Jika sebuah buku halaman 8 dan 9 bersebelahan, buku itu dijilid bagaimana?", "id": "Buku tidak dijilid dengan benar." },
  { "en": "Jam berapa yang menunjukkan waktu yang benar hanya dua kali sehari?", "id": "Jam yang rusak atau mati." },
  { "en": "Bagaimana cara memotong kue menjadi 8 potong hanya dengan 3 kali potongan?", "id": "Dua potongan vertikal, satu potongan horizontal." },
  { "en": "Jika Anda menjumlahkan umur saya dengan 2, membaginya dengan 3, lalu menguranginya dengan 1, hasilnya 5. Berapa umur saya?", "id": "Enam belas tahun." },
  { "en": "Apa angka selanjutnya: 7, 10, 8, 11, 9, 12, ...?", "id": "10 (polanya +3, -2)." },
  { "en": "Berapa banyak segitiga sama sisi yang bisa dibuat dari 6 batang korek api?", "id": "Empat, dalam bentuk piramida tiga dimensi." },
  { "en": "Jika 10 mesin membuat 10 mobil dalam 10 hari, berapa lama 5 mesin membuat 5 mobil?", "id": "Sepuluh hari." },
  { "en": "Ayah Mary punya 5 putri: Nana, Nene, Nini, Nono. Siapa nama putri kelima?", "id": "Mary." },
  { "en": "Apa yang memiliki banyak kata tetapi tidak pernah berbicara?", "id": "Buku." },
  { "en": "Berapa banyak cara 3 orang bisa duduk di 3 kursi?", "id": "Enam cara (3 faktorial)." },
  { "en": "Jika sebuah tim menang 3 kali dan kalah 1 kali, berapa persentase kemenangannya?", "id": "Tujuh puluh lima persen." },
  { "en": "Berapa banyak lubang yang dimiliki sebuah kaos?", "id": "Empat lubang (leher, lengan, bawah)." },
  { "en": "Apa yang memiliki jari-jari tetapi tidak bisa bertepuk tangan?", "id": "Roda." },
  { "en": "Jika Anda menggandakan saya dan menambahkan 4, hasilnya 10. Siapakah saya?", "id": "Angka tiga." },
  { "en": "Sebuah toko mengurangi harga sebesar 20%, lalu menaikkannya lagi 20%. Apakah harganya sama?", "id": "Tidak, harganya lebih rendah dari semula." },
  { "en": "Apa yang memiliki nilai tetapi tidak memiliki ukuran?", "id": "Angka." },
  { "en": "Bagaimana cara menulis 31 menggunakan hanya angka 3 sebanyak lima kali?", "id": "33 - 3 + 3/3." },
  { "en": "Jika dibutuhkan 1 orang 1 jam untuk mengecat 1 dinding, berapa lama 2 orang mengecat 2 dinding?", "id": "Satu jam." },
  { "en": "Apa angka yang selalu berbohong?", "id": "Angka delapan (lying on its side)." },
  { "en": "Jika Anda menambahkan nomor rumah Anda dengan 5, dikalikan 2, ditambah 10, dibagi 2, lalu dikurangi nomor rumah Anda, hasilnya?", "id": "Sepuluh." },
  { "en": "Di sebuah ruangan ada 2 anjing, 4 kucing, dan 5 manusia. Berapa banyak kaki di lantai?", "id": "Dua, karena hewan punya cakar." },
  { "en": "Apa itu probabilitas melempar dadu dan mendapatkan angka 7?", "id": "Nol." },
  { "en": "Gunakan empat angka 2 untuk membuat 25.", "id": "22 + 2 + 2/2 (tidak bisa)." },
  { "en": "Berapa banyak permukaan yang dimiliki sebuah bola Mobius?", "id": "Satu permukaan." },
  { "en": "Dua angka yang jika dikalikan hasilnya 16 dan jika dijumlahkan hasilnya 10?", "id": "Delapan dan dua." },
  { "en": "Berapa banyak kubus 1x1x1 yang ada dalam kubus 3x3x3?", "id": "Dua puluh tujuh." },
  { "en": "Apa yang terjadi sekali dalam setahun, dua kali dalam seminggu, tapi tidak pernah dalam sehari?", "id": "Huruf 'E'." },
  { "en": "Jika 4 apel berharga 80 sen, berapa harga 6 apel?", "id": "120 sen atau $1,20." },
  { "en": "Jika besok adalah hari Minggu, hari apa kemarin lusa?", "id": "Kamis." },
  { "en": "Apa yang punya cabang tapi tidak punya daun atau buah?", "id": "Bank." },
  { "en": "Jika Anda membutuhkan 8 potong roti untuk 4 orang, berapa potong untuk 3 orang?", "id": "Enam potong roti." },
  { "en": "Berapa jumlah sudut dalam sebuah segi delapan (oktagon)?", "id": "1080 derajat." },
  { "en": "Jika dibutuhkan 3 jam untuk menempuh 180 mil, berapa kecepatan rata-ratanya?", "id": "Enam puluh mil per jam." },
  { "en": "Saya adalah bayangan dari angka 3. Siapakah saya?", "id": "Huruf E." },
  { "en": "Bagaimana cara mengukur 6 menit dengan jam pasir 4 menit dan 7 menit?", "id": "Balik keduanya, saat 4 menit habis balik lagi." },
  { "en": "Berapa banyak angka 2 yang Anda perlukan untuk membuat 222?", "id": "Tiga angka 2." },
  { "en": "Sebuah kereta menempuh 1 mil dalam 1 menit 15 detik. Berapa kecepatannya?", "id": "48 mil per jam." },
  { "en": "Jika Anda memiliki 3 kaus kaki hijau, 4 biru, dan 5 merah dalam kegelapan, berapa minimal yang harus diambil untuk sepasang?", "id": "Empat kaus kaki." },
  { "en": "Apa yang memiliki dasar di atas?", "id": "Kaki Anda." },
  { "en": "Jika seorang dokter memberi Anda 5 pil untuk diminum satu setiap jam, kapan pil terakhir diminum?", "id": "Empat jam setelah yang pertama." },
  { "en": "Berapa banyak sisi pada sebuah koin?", "id": "Tiga (depan, belakang, samping)." },
  { "en": "Apa yang mengikuti anjing kemanapun ia pergi?", "id": "Ekornya." },
  { "en": "Berapa banyak angka dalam PI yang Anda ketahui?", "id": "3.14159..." },
  { "en": "Berapa jumlah dari semua angka pada dadu standar?", "id": "Dua puluh satu." },
  { "en": "Jika 3+3=8, 4+4=10, maka 5+5=...?", "id": "12 (polanya +2)." },
  { "en": "Berapa banyak cara untuk menyusun huruf 'APPLE'?", "id": "Enam puluh cara." },
  { "en": "Berapa banyak angka antara 1 dan 100 yang bisa dibagi 7?", "id": "Empat belas angka." },
  { "en": "Jika sebuah persegi memiliki luas 64, berapa kelilingnya?", "id": "Tiga puluh dua." },
  { "en": "Apa benda matematika yang bisa Anda sajikan untuk pencuci mulut?", "id": "Pi (pie)." },
  { "en": "Jika Anda memiliki 23 orang di sebuah ruangan, berapa kemungkinan dua orang memiliki tanggal lahir yang sama?", "id": "Lebih dari lima puluh persen." },
  { "en": "Berapa banyak tanda 'stop' yang Anda lewati jika Anda berkendara 25 mil dengan kecepatan 25 mph di kota?", "id": "Tergantung pada rutenya." },
  { "en": "Apa yang memiliki satu mata tetapi tidak bisa melihat?", "id": "Badai." },
  { "en": "Bagaimana cara membuat 4 segitiga sama sisi dengan 3 batang korek api?", "id": "Tidak bisa, butuh minimal 6." },
  { "en": "Jika seorang pria memakan 100 anggur dalam 5 hari, setiap hari makan 6 lebih banyak dari hari sebelumnya, berapa banyak yang ia makan pada hari pertama?", "id": "Delapan anggur." },
  { "en": "Berapa banyak digit yang ada dalam 1 juta?", "id": "Tujuh digit." },
  { "en": "Apa yang selalu bertambah tetapi tidak pernah berkurang?", "id": "Usia." },
  { "en": "Berapa banyak 0 dalam satu miliar?", "id": "Sembilan." },
  { "en": "Jika 5 ekor sapi memakan 5 hektar rumput dalam 5 hari, berapa lama 1 ekor sapi memakan 1 hektar?", "id": "Lima hari." },
  { "en": "Apa itu segitiga Pascal?", "id": "Susunan bilangan segitiga." },
  { "en": "Jika Anda memiliki 10 lilin yang menyala dan angin mematikan 3, berapa yang tersisa?", "id": "Tiga, yang lainnya terbakar habis." },
  { "en": "Apa yang memiliki 6 wajah tetapi tidak memakai riasan?", "id": "Dadu." },
  { "en": "Berapa banyak detik dalam satu hari?", "id": "86.400 detik." },
  { "en": "Jika Anda melipat selembar kertas 42 kali, setinggi apa itu?", "id": "Akan mencapai bulan." },
  { "en": "Apa itu bilangan sempurna?", "id": "Jumlah pembaginya sama dengan dirinya sendiri (6, 28)." },
  { "en": "Apa yang memiliki awal, tengah, dan akhir, tetapi hanya memiliki satu huruf?", "id": "Envelope (amplop)." },
  { "en": "Seorang pria memiliki 10 koin senilai $1,15. Dia tidak bisa memberi kembalian untuk satu dolar. Koin apa yang dia miliki?", "id": "Satu koin 50 sen, tiga koin 20 sen, enam koin 1 sen." },
  { "en": "Berapa banyak sudut yang dimiliki sebuah lingkaran?", "id": "Nol." },
  { "en": "Jika dibutuhkan 7 hari untuk membuat 1 mobil, berapa mobil yang bisa dibuat dalam 28 hari?", "id": "Empat mobil." },
  { "en": "Apa angka selanjutnya: 1, 2, 4, 8, 16, ...?", "id": "32 (dikalikan dua)." },
  { "en": "Jika Anda memotong tali menjadi 10 bagian, berapa banyak potongan yang Anda buat?", "id": "Sembilan potongan." },
  { "en": "Berapa banyak cara berbeda untuk mendapatkan 4 dari dua dadu?", "id": "Tiga cara (1+3, 2+2, 3+1)." },
  { "en": "Apa yang memiliki kepala dan ekor tetapi tidak memiliki tubuh?", "id": "Koin." },
  { "en": "Berapa nilai dari sudut dalam sebuah bujur sangkar?", "id": "Sembilan puluh derajat." },
  { "en": "Jika Anda membeli 3 item seharga 45 sen, berapa kembalian dari satu dolar?", "id": "Lima puluh lima sen." },
  { "en": "Berapa banyak nol yang Anda tulis untuk sepuluh ribu?", "id": "Empat nol." },
  { "en": "Jika ada 15 bola di dalam kotak, 5 merah, 5 biru, 5 hijau. Berapa minimal yang diambil untuk memastikan Anda mendapat satu dari setiap warna?", "id": "Sebelas bola." },
  { "en": "Apa yang bisa mengisi ruangan tapi tidak memakan tempat?", "id": "Cahaya." },
  { "en": "Berapa nilai dari 10 dibagi setengah ditambah 5?", "id": "Dua puluh lima." },
  { "en": "Jika 2 tukang kayu bisa membuat 2 meja dalam 2 hari, berapa lama 1 tukang kayu membuat 1 meja?", "id": "Dua hari." },
  { "en": "Berapa hasil dari 3x3x3x3?", "id": "Delapan puluh satu." },
  { "en": "Apa yang selalu di depan Anda tetapi tidak bisa dilihat?", "id": "Masa depan." },
  { "en": "Berapa banyak hewan dari setiap jenis yang dibawa Cleopatra ke dalam bahtera?", "id": "Cleopatra tidak punya bahtera." },
  { "en": "Jika sebuah lingkaran memiliki diameter 10, berapa jari-jarinya?", "id": "Lima." },
  { "en": "Angka berapa yang jika dibalik tetap sama?", "id": "69, 96, 88." },
  { "en": "Jika Anda memiliki 25 koin dan menghabiskan 11, berapa yang tersisa?", "id": "Empat belas koin." },
  { "en": "Bagaimana cara membuat 1000 dengan delapan angka 7?", "id": "777 + 77 + 7 + 7 + 7 + 7 + 7 + 1 = 1000. Tidak mungkin." },
  { "en": "Apa yang terjadi jika Anda melempar batu hijau ke Laut Merah?", "id": "Batu itu menjadi basah dan tenggelam." },
  { "en": "Dua bilangan prima yang jika dijumlahkan hasilnya 25?", "id": "2 dan 23." },
  { "en": "Berapa banyak sisi pada sebuah snowflake?", "id": "Enam." },
  { "en": "Jika 1/2 dari 5 adalah 3, maka 1/3 dari 10 adalah?", "id": "Empat." },
  { "en": "Berapa banyak kali Anda bisa melipat selembar kertas menjadi dua?", "id": "Sekitar tujuh kali secara fisik." },
  { "en": "Apa yang memiliki leher tetapi tidak memiliki kepala?", "id": "Gitar." },
  { "en": "Jika dibutuhkan 1 menit untuk berlari 1 putaran, berapa lama untuk 10 putaran?", "id": "Sepuluh menit." },
  { "en": "Sebuah buku memiliki 100 halaman. Berapa banyak lembar kertas yang dimilikinya?", "id": "Lima puluh lembar." },
  { "en": "Berapa nilai x jika 2x + 4 = 10?", "id": "Tiga." },
  { "en": "Apa yang memiliki 12 wajah dan 30 tepi?", "id": "Dodecahedron." },
  { "en": "Jika ada 3 apel dan Anda mengambil 2, berapa banyak yang Anda miliki?", "id": "Dua apel." },
  { "en": "Berapa banyak titik di dua dadu?", "id": "Empat puluh dua." },
  { "en": "Jika Anda berada di sebuah ruangan dengan 100 orang, dan setiap orang menjabat tangan setiap orang lain sekali, berapa banyak jabat tangan yang terjadi?", "id": "4950 jabat tangan." },
  { "en": "Apa angka selanjutnya dalam urutan ini: 1, 5, 9, 13, ...?", "id": "17 (ditambah empat setiap kali)." },
  { "en": "Bagaimana cara membuat angka 30 dengan tiga angka yang sama?", "id": "6 x 5 = 30 (bukan tiga angka sama)." },
  { "en": "Sebuah kelas berisi 26 siswa. Jika ada 4 lebih banyak perempuan daripada laki-laki, berapa banyak perempuan?", "id": "15 perempuan." },
  { "en": "Berapa banyak cara untuk menyusun ulang huruf 'BOOK'?", "id": "Dua belas cara." },
  { "en": "Jika sebuah lingkaran dibagi oleh 4 garis lurus, berapa jumlah maksimum wilayah yang terbentuk?", "id": "Sebelas wilayah." },
  { "en": "Gunakan angka 1, 2, 3, 4 untuk membuat 24.", "id": "(4-3+1)*2 tidak bisa. Coba lagi." },
  { "en": "Saya adalah angka antara 20 dan 30. Saya adalah kelipatan 3 dan 4. Siapakah saya?", "id": "Dua puluh empat." },
  { "en": "Berapa banyak jari kaki pada 8 orang?", "id": "Delapan puluh jari kaki." },
  { "en": "Jika 1 ayam bertelur 1 telur sehari, berapa telur dari 10 ayam dalam 10 hari?", "id": "Seratus telur." },
  { "en": "Berapa banyak angka ganjil antara 0 dan 20?", "id": "Sepuluh angka ganjil." },
  { "en": "Apa yang bisa Anda letakkan antara 7 dan 8 untuk membuatnya lebih besar dari 7 tapi lebih kecil dari 8?", "id": "Tanda koma (7,8)." },
  { "en": "Urutan berikutnya: 1, 4, 27, 256, ...?", "id": "3125 (n pangkat n)." },
  { "en": "Seorang petani memiliki 19 domba, semua kecuali 7 mati. Berapa banyak yang hidup?", "id": "Tujuh domba." },
  { "en": "Berapa banyak ujung pada 5 setengah batang korek api?", "id": "Dua belas ujung." },
  { "en": "Jika sebuah pertandingan sepak bola berakhir 5-3, berapa total gol yang dicetak?", "id": "Delapan gol." },
  { "en": "Bagaimana cara mendapatkan 24 menggunakan tiga angka 8?", "id": "8 + 8 + 8." },
  { "en": "Berapa banyak hewan dari setiap jenis yang dibawa Ratu Elizabeth ke bahtera?", "id": "Dia tidak punya bahtera." },
  { "en": "Jika sebuah segitiga memiliki sisi 3, 4, dan 5, jenis segitiga apa itu?", "id": "Segitiga siku-siku." },
  { "en": "Apa angka satu digit yang paling sering muncul antara 1 dan 1000?", "id": "Angka satu." },
  { "en": "Jika dibutuhkan 4 orang 4 hari untuk membangun 4 perahu, berapa lama 1 orang membangun 1 perahu?", "id": "Empat hari." },
  { "en": "Berapa banyak angka 9 yang Anda lewati saat menghitung dari 1 sampai 100?", "id": "Dua puluh angka sembilan." },
  { "en": "Jika Anda memiliki keranjang berisi 10 apel dan 10 teman, bagaimana Anda memberi setiap teman satu apel dan menyisakan satu di keranjang?", "id": "Berikan keranjang dengan apel terakhirnya." },
  { "en": "Berapa nilai dari sudut ketiga jika dua sudut segitiga adalah 60 derajat?", "id": "Enam puluh derajat." },
  { "en": "Bagaimana cara membuat persamaan ini benar: 81 x 9 = 801?", "id": "Balikkan: 108 = 6 x 18." },
  { "en": "Apa yang memiliki 3 kaki tetapi tidak bisa berjalan?", "id": "Meteran." },
  { "en": "Jika ada 60 menit dalam satu jam, berapa menit dalam 2 setengah jam?", "id": "150 menit." },
  { "en": "Urutan berikutnya: 3, 3, 5, 4, 4, 3, 5, ...?", "id": "5 (jumlah huruf dalam angka)." },
  { "en": "Jika sebuah mobil berjalan dengan kecepatan 60 km/jam, berapa jauh perjalanannya dalam 2 jam 30 menit?", "id": "150 kilometer." },
  { "en": "Berapa banyak angka prima antara 10 dan 20?", "id": "Empat (11, 13, 17, 19)." },
  { "en": "Apa yang memiliki banyak masalah tetapi tidak ada solusi?", "id": "Buku matematika." },
  { "en": "Jika seekor kuda diikat dengan tali 5 meter, area merumputnya berapa?", "id": "Lingkaran dengan jari-jari 5 meter." },
  { "en": "Berapa jumlah total hari dalam setahun kabisat?", "id": "366 hari." },
  { "en": "Jika dibutuhkan 20 langkah untuk menaiki separuh menara, berapa total langkahnya?", "id": "Empat puluh langkah." },
  { "en": "Angka apa yang bisa Anda bagi dengan 2, 3, 4, 5, dan 6 dengan sisa 1?", "id": "Enam puluh satu." },
  { "en": "Apa yang memiliki leher tanpa kepala, dan punggung tanpa tulang?", "id": "Kemeja." },
  { "en": "Jika Anda mengambil 3 apel dari 5 apel, berapa yang tersisa?", "id": "Lima apel, Anda hanya memegangnya." },
  { "en": "Berapa banyak cara berbeda untuk membayar 10 sen?", "id": "Empat cara." },
  { "en": "Berapa banyak lubang pada sebuah sedotan?", "id": "Satu lubang panjang." },
  { "en": "Jika 1=3, 2=3, 3=5, 4=4, 5=4, maka 6=...?", "id": "3 (jumlah huruf dalam kata)." },
  { "en": "Berapa jumlah dari 10 bilangan ganjil pertama?", "id": "Seratus." },
  { "en": "Jika sebuah persegi panjang memiliki panjang 10 dan lebar 5, berapa luasnya?", "id": "Lima puluh." },
  { "en": "Berapa banyak sisi yang dimiliki oleh sebuah dodecahedron?", "id": "Dua belas sisi." },
  { "en": "Berapa kali jam dan menit tumpang tindih dalam 24 jam?", "id": "Dua puluh dua kali." },
  { "en": "Apa yang memiliki kota, toko, dan jalan tetapi tidak ada orang?", "id": "Peta." },
  { "en": "Gunakan tiga angka 5 untuk membuat 31.", "id": "5! / 5 + 5 (tidak bisa)." },
  { "en": "Jika ada 8 jeruk di tas dan Anda mengambil 2, berapa banyak yang Anda miliki?", "id": "Dua jeruk." },
  { "en": "Berapa banyak angka yang bisa Anda bentuk menggunakan 2, 5, dan 8?", "id": "Enam angka berbeda." },
  { "en": "Jika Anda berada di urutan ke-3 dari depan dan belakang dalam sebuah antrian, berapa banyak orang di sana?", "id": "Lima orang." },
  { "en": "Apa yang memiliki 4 jari dan 1 ibu jari tetapi bukan tangan?", "id": "Sarung tangan." },
  { "en": "Bagaimana cara menulis 20 tanpa menggunakan angka 2 atau 0?", "id": "Gunakan angka Romawi: XX." },
  { "en": "Berapa banyak sisi pada sebuah belah ketupat?", "id": "Empat sisi." },
  { "en": "Jika kemarin adalah hari Rabu, hari apa setelah besok?", "id": "Jumat." },
  { "en": "Berapa hasil dari 100 x 0?", "id": "Nol." },
  { "en": "Jika sebuah tim memainkan 10 pertandingan dan memenangkan 6, berapa persen kekalahannya?", "id": "Empat puluh persen." },
  { "en": "Angka apa yang jika dikalikan sendiri hasilnya adalah kuadrat sempurna?", "id": "Semua angka." },
  { "en": "Berapa banyak sudut tumpul dalam sebuah segitiga sama kaki?", "id": "Maksimal satu." },
  { "en": "Jika 2 apel dan 3 jeruk berharga 7 dolar, dan 3 apel dan 2 jeruk berharga 8 dolar, berapa harga 1 apel?", "id": "Dua dolar." },
  { "en": "Apa yang menjadi lebih besar semakin banyak Anda ambil?", "id": "Lubang." },
  { "en": "Berapa banyak angka 5 yang muncul dari 1 hingga 100?", "id": "Dua puluh." },
  { "en": "Jika dibutuhkan 1 menit untuk menelepon 1 teman, berapa lama untuk 5 teman secara berurutan?", "id": "Lima menit." },
  { "en": "Apa angka selanjutnya: 2, 5, 10, 17, 26, ...?", "id": "37 (n^2 + 1)." },
  { "en": "Berapa banyak segitiga dalam sebuah bintang Daud?", "id": "Delapan segitiga." },
  { "en": "Jika Anda memiliki 7 kaus kaki tunggal, berapa banyak pasangan yang bisa Anda buat?", "id": "Nol pasangan." },
  { "en": "Berapa banyak huruf dalam alfabet Yunani?", "id": "Dua puluh empat." },
  { "en": "Berapa banyak angka yang ada di jam analog?", "id": "Dua belas angka." },
  { "en": "Apa yang bisa Anda hitung, tetapi tidak bisa Anda lihat?", "id": "Suara Anda." },
  { "en": "Jika Anda membutuhkan 18 telur untuk 3 kue, berapa telur untuk 1 kue?", "id": "Enam telur." },
  { "en": "Berapa banyak angka nol dalam seratus ribu?", "id": "Lima nol." },
  { "en": "Jika sebuah persegi dilipat diagonal, bentuk apa yang dihasilkan?", "id": "Segitiga." },
  { "en": "Apa yang memiliki akar yang tidak bisa dilihat?", "id": "Bilangan." },
  { "en": "Seorang anak memiliki 10 koin senilai 25 sen. Berapa total uangnya?", "id": "Dua ratus lima puluh sen atau $2,50." },
  { "en": "Berapa banyak cara untuk memilih 2 orang dari 4 orang?", "id": "Enam cara." },
  { "en": "Jika Anda memiliki 2 ember, 3 liter dan 5 liter, bagaimana Anda mengukur tepat 1 liter?", "id": "Isi 3L, tuang ke 5L. Ulangi." },
  { "en": "Berapa banyak angka genap antara 1 dan 11?", "id": "Lima angka genap." },
  { "en": "Apa yang memiliki 2 tangan tetapi tidak bisa bertepuk tangan?", "id": "Jam." },
  { "en": "Jika Anda memiliki 100 bola, 99% merah. Berapa bola merah yang harus dibuang agar menjadi 98% merah?", "id": "Lima puluh bola." },
  { "en": "Berapa banyak sudut siku-siku dalam sebuah persegi panjang?", "id": "Empat sudut." },
  { "en": "Jika sebuah buku berharga $15 ditambah setengah harganya, berapa harganya?", "id": "Tiga puluh dolar." },
  { "en": "Angka apa yang jika dibagi dua adalah nol?", "id": "Angka 8." },
  { "en": "Berapa banyak angka yang ada pada sebuah kalkulator standar?", "id": "Sepuluh (0-9)." },
  { "en": "Jika Anda menulis angka 1 sampai 50, berapa kali Anda menulis angka 4?", "id": "Lima belas kali." },
  { "en": "Apa yang memiliki volume tetapi tidak ada massa?", "id": "Suara." },
  { "en": "Jika 6 kucing bisa menangkap 6 tikus dalam 6 menit, berapa kucing yang dibutuhkan untuk menangkap 100 tikus dalam 100 menit?", "id": "Enam kucing." },
  { "en": "Berapa banyak bulan yang memiliki 30 hari?", "id": "Empat bulan." },
  { "en": "Sebuah batang baja dipotong 10 kali. Berapa banyak potongan yang dihasilkan?", "id": "Sebelas potongan." },
  { "en": "Apa yang memiliki dua titik akhir tetapi tidak ada awal atau akhir?", "id": "Garis." },
  { "en": "Jika seorang pria berjarak 30 kaki dari rumahnya dan berjalan 10 kaki ke arahnya, berapa jauh dia sekarang?", "id": "Dua puluh kaki." },
  { "en": "Berapa banyak huruf 'x' dalam 'multiplication'?", "id": "Nol." },
  { "en": "Jika sebuah tim mencetak 2 gol per pertandingan, berapa gol dalam 5 pertandingan?", "id": "Sepuluh gol." },
  { "en": "Angka berapa yang jika Anda tambahkan ke dirinya sendiri, kalikan dengan 4, lalu bagi dengan 8, Anda mendapatkan angka itu kembali?", "id": "Angka apa pun." },
  { "en": "Berapa banyak sisi pada prisma segitiga?", "id": "Lima sisi." },
  { "en": "Apa yang bisa Anda bagi tetapi tidak bisa Anda lihat?", "id": "Waktu." },
  { "en": "Jika sebuah kue dipotong menjadi 4, lalu setiap potongan dipotong menjadi 2, berapa total potongan?", "id": "Delapan potongan." },
  { "en": "Berapa banyak angka 3 digit yang bisa Anda buat menggunakan 1, 2, 3 tanpa pengulangan?", "id": "Enam angka." },
  { "en": "Jika Anda memiliki 26 orang dalam perlombaan, dan Anda mengalahkan orang terakhir, posisi berapa Anda?", "id": "Posisi ke dua puluh lima." },
  { "en": "Apa yang bisa Anda tambah tetapi tidak bisa Anda kurangi?", "id": "Usia Anda." },
  { "en": "Jika Anda memiliki 20 item dan mengelompokkannya dalam 5, berapa banyak grup yang Anda miliki?", "id": "Empat grup." },
  { "en": "Berapa banyak angka dari 1 sampai 20 yang habis dibagi 3?", "id": "Enam angka." },
  { "en": "Apa itu bilangan rasional?", "id": "Bilangan yang bisa dinyatakan sebagai pecahan." },
  { "en": "Apa angka selanjutnya: 1, 1, 2, 4, 3, 9, 4, ...?", "id": "16 (urutan angka dan kuadratnya)." },
  { "en": "Berapa banyak digit pada bilangan 10 pangkat 100 (Googol)?", "id": "101 digit." },
  { "en": "Jika sebuah bus berangkat dengan 6 penumpang. Di perhentian pertama, 4 turun, 8 naik. Di perhentian kedua, setengahnya turun. Berapa penumpang tersisa?", "id": "Lima penumpang." },
  { "en": "Gunakan empat angka 4 untuk membuat 48.", "id": "(4+4) * (4+4) - 4 = 60. Tidak bisa." },
  { "en": "Jika butuh 2 menit untuk menggoreng satu sisi pancake, berapa waktu minimum untuk 3 pancake?", "id": "Tiga menit." },
  { "en": "Sebuah jam kehilangan 20 detik setiap menit. Berapa lama sampai jam itu kehilangan 24 jam penuh?", "id": "72 jam." },
  { "en": "Jika dibutuhkan 9 tukang 2 hari untuk membangun gudang, berapa lama 3 tukang membangunnya?", "id": "Enam hari." },
  { "en": "Apa tiga bilangan bulat yang hasil jumlah dan kalinya sama?", "id": "Satu, dua, dan tiga." },
  { "en": "Sebuah bakteri berlipat ganda setiap menit. Jika cangkir penuh pada jam 12:00, kapan cangkir setengah penuh?", "id": "Pukul 11:59." },
  { "en": "Jika 10 kucing di atas atap dan satu melompat, berapa yang tersisa?", "id": "Tidak ada, mereka 'copy cats'." },
  { "en": "Berapa jumlah sudut dalam sebuah heksagon?", "id": "720 derajat." },
  { "en": "Apa yang bisa Anda bagi di antara dua orang, tetapi hanya satu yang akan mendapatkannya?", "id": "Sebuah rahasia." },
  { "en": "Jika Anda memiliki 25 kuda dan 5 lintasan balap, berapa balapan minimum untuk menemukan 3 kuda tercepat?", "id": "Tujuh balapan." },
  { "en": "Apa yang menjadi lebih basah semakin kering?", "id": "Handuk." },
  { "en": "Berapa banyak angka 6 yang Anda lihat dari 1 sampai 100?", "id": "Dua puluh." },
  { "en": "Jika harga sebuah barang $18 setelah diskon 10%, berapa harga aslinya?", "id": "Dua puluh dolar." },
  { "en": "Angka berapa yang jika dibaca dari depan atau belakang sama?", "id": "Palindrom, seperti 181." },
  { "en": "Berapa banyak hewan dari setiap jenis yang dibawa Columbus ke kapalnya?", "id": "Tidak ada, dia tidak punya bahtera." },
  { "en": "Sebuah toko menjual 8 apel seharga $1. Berapa harga 12 apel?", "id": "Satu setengah dolar ($1.50)." },
  { "en": "Jika dua hari yang lalu adalah hari setelah Senin, hari apa besok?", "id": "Jumat." },
  { "en": "Apa yang memiliki akar kuadrat negatif?", "id": "Bilangan imajiner." },
  { "en": "Berapa banyak cara untuk mewarnai 3 negara di peta dengan 2 warna tanpa ada yang bersebelahan?", "id": "Nol, tidak mungkin." },
  { "en": "Jika dibutuhkan 10 jam untuk 100 pekerja membangun 1 rumah, berapa lama 1 pekerja membangun 1 rumah?", "id": "Seribu jam." },
  { "en": "Apa angka romawi untuk 500?", "id": "D." },
  { "en": "Jika Anda menjumlahkan sisi-sisi dadu yang berlawanan, berapa hasilnya?", "id": "Tujuh." },
  { "en": "Bagaimana cara menempatkan 10 koin ke dalam 3 gelas sehingga setiap gelas berisi jumlah ganjil?", "id": "Tempatkan 7, 1, dan 2 (tidak mungkin)." },
  { "en": "Jika seekor burung terbang 10 mil ke selatan, lalu 10 mil ke timur, lalu 10 mil ke utara, ia berada di tempat yang sama. Di mana dia?", "id": "Di Kutub Utara." },
  { "en": "Berapa banyak angka 0 di akhir dari 10! (10 faktorial)?", "id": "Dua angka nol." },
  { "en": "Seorang pria berusia 30 tahun. Putranya berusia 3 tahun. Kapan usia pria itu tiga kali lipat usia putranya?", "id": "Dalam 10.5 tahun (tidak mungkin)." },
  { "en": "Apa yang memiliki 21 titik tetapi tidak bisa melihat?", "id": "Dadu." },
  { "en": "Jika Anda memiliki tali sepanjang 30 meter dan memotongnya 2 meter setiap hari, kapan potongan terakhir?", "id": "Pada hari ke empat belas." },
  { "en": "Berapa banyak segitiga yang bisa dibentuk dari 5 titik di lingkaran?", "id": "Sepuluh segitiga." },
  { "en": "Apa yang memiliki probabilitas 1?", "id": "Sesuatu yang pasti terjadi." },
  { "en": "Berapa banyak angka genap prima yang ada?", "id": "Satu, yaitu angka dua." },
  { "en": "Jika dibutuhkan 3 galon cat untuk menutupi sebuah ruangan, berapa yang dibutuhkan untuk ruangan berukuran dua kali lipat?", "id": "Dua belas galon." },
  { "en": "Apa yang memiliki keliling tak terhingga tetapi luas terbatas?", "id": "Koch snowflake (fraktal)." },
  { "en": "Berapa banyak angka dari 1 sampai 100 yang memiliki angka 0?", "id": "Sebelas." },
  { "en": "Jika Anda melempar dua koin, berapa probabilitas keduanya kepala?", "id": "Satu dari empat (25%)." },
  { "en": "Seorang pria bertaruh bahwa kudanya akan menang. Kuda itu seri. Apakah dia menang?", "id": "Tidak, dia tidak menang." },
  { "en": "Apa itu bilangan 'transenden'?", "id": "Bilangan yang bukan akar polinomial (contoh: pi)." },
  { "en": "Jika sebuah kereta berbelok ke kanan, roda mana yang paling sedikit berputar?", "id": "Roda bagian dalam." },
  { "en": "Berapa banyak bujur sangkar kecil di dalam bujur sangkar 4x4?", "id": "Enam belas." },
  { "en": "Jika 5+6=11 dan 7+8=15, mengapa 9+10=7?", "id": "Karena itu jam 7 pagi." },
  { "en": "Apa itu Paradoks Zeno?", "id": "Untuk mencapai tujuan, Anda harus menempuh setengah jaraknya." },
  { "en": "Gunakan angka 2, 3, 4, 5 untuk membuat 26.", "id": "5 * (4 + 3/2) tidak bisa." },
  { "en": "Jika Anda punya 10 ikan, 5 tenggelam, 2 berenang pergi, 3 mati. Berapa yang tersisa?", "id": "Sepuluh, mereka semua masih di sana." },
  { "en": "Apa perbedaan antara 6 dan setengah lusin?", "id": "Tidak ada perbedaan." },
  { "en": "Jika sebuah kubus dicat merah lalu dipotong menjadi 27 kubus kecil, berapa yang tidak dicat sama sekali?", "id": "Satu kubus." },
  { "en": "Apa angka selanjutnya: 1, 2, 6, 24, 120, ...?", "id": "720 (faktorial)." },
  { "en": "Berapa banyak sisi pada sebuah bola?", "id": "Dua: dalam dan luar." },
  { "en": "Jika Anda mengalikan semua angka ganjil antara 1 dan 10, berapa hasilnya?", "id": "945." },
  { "en": "Berapa banyak cara untuk mendapatkan jumlah 5 dari dua dadu?", "id": "Empat cara." },
  { "en": "Apa itu konjektur Goldbach?", "id": "Setiap bilangan genap > 2 adalah jumlah dua prima." },
  { "en": "Berapa banyak detik dalam satu jam?", "id": "3600 detik." },
  { "en": "Jika sebuah lingkaran memiliki keliling 10π, berapa luasnya?", "id": "25π." },
  { "en": "Berapa banyak garis yang bisa Anda gambar melalui 3 titik yang tidak segaris?", "id": "Tiga garis." },
  { "en": "Berapa nilai dari 0/0?", "id": "Tidak terdefinisi." },
  { "en": "Jika sebuah pita sepanjang 10 meter dipotong menjadi 5 bagian, berapa banyak potongan yang dibuat?", "id": "Empat potongan." },
  { "en": "Apa yang memiliki 8 sudut dan 12 sisi?", "id": "Kubus." },
  { "en": "Jika Anda menghitung mundur dari 100, angka berapa yang datang sebelum 90?", "id": "Sembilan puluh satu." },
  { "en": "Apa yang memiliki awal tak terhingga dan akhir tak terhingga?", "id": "Garis lurus." },
  { "en": "Berapa banyak angka 3 yang ada antara 1 dan 50?", "id": "Lima belas." },
  { "en": "Jika dibutuhkan 4 hari bagi 4 ayam untuk menetaskan 4 telur, berapa lama 1 ayam menetaskan 1 telur?", "id": "Empat hari." },
  { "en": "Berapa banyak angka 2 digit yang ada?", "id": "Sembilan puluh." },
  { "en": "Jika sebuah segitiga memiliki sudut 90 dan 45 derajat, berapa sudut ketiganya?", "id": "Empat puluh lima derajat." },
  { "en": "Apa yang memiliki leher panjang tapi tidak ada kepala?", "id": "Jerapah di buku anatomi." },
  { "en": "Berapa banyak angka 7 yang Anda temukan dalam satu set kartu remi?", "id": "Empat." },
  { "en": "Jika Anda memiliki 15 permen dan membaginya di antara 3 teman, berapa yang didapat masing-masing?", "id": "Lima permen." },
  { "en": "Apa itu bilangan irasional?", "id": "Bilangan yang tidak bisa jadi pecahan sederhana." },
  { "en": "Berapa banyak sisi pada sebuah piramida persegi?", "id": "Lima sisi." },
  { "en": "Jika sebuah mobil melaju 120 km dalam 2 jam, berapa kecepatannya?", "id": "60 kilometer per jam." },
  { "en": "Bagaimana cara menulis 49 dalam angka Romawi?", "id": "XLIX." },
  { "en": "Jika Anda memiliki 3 koin, berapa banyak jumlah uang yang berbeda yang bisa Anda buat?", "id": "Tujuh." },
  { "en": "Apa yang memiliki volume tak terhingga?", "id": "Tanduk Gabriel." },
  { "en": "Jika sebuah kotak berisi 24 kaleng, dan Anda mengambil sepertiganya, berapa yang tersisa?", "id": "Enam belas kaleng." },
  { "en": "Angka apa yang jika ditambah 5 atau dikalikan 5 hasilnya sama?", "id": "1.25." },
  { "en": "Berapa banyak nol dalam satu triliun?", "id": "Dua belas." },
  { "en": "Jika seekor katak berada di dasar sumur 30 kaki dan melompat 3 kaki setiap hari dan jatuh 2 kaki setiap malam, kapan ia keluar?", "id": "Pada hari ke dua puluh delapan." },
  { "en": "Berapa banyak jari dan ibu jari pada 10 pasang sarung tangan?", "id": "Seratus." },
  { "en": "Apa itu Teorema Pythagoras?", "id": "a² + b² = c²." },
  { "en": "Jika Anda membutuhkan 1 galon untuk mengecat 100 kaki persegi, berapa yang dibutuhkan untuk 250 kaki persegi?", "id": "2.5 galon." },
  { "en": "Berapa banyak angka dari 1 sampai 1000 yang merupakan kuadrat sempurna?", "id": "Tiga puluh satu." },
  { "en": "Apa yang memiliki 4 sisi sama dan 4 sudut siku-siku?", "id": "Bujur sangkar." },
  { "en": "Jika Anda memutar dadu, angka mana yang tidak pernah Anda lihat?", "id": "Sisi bawah." },
  { "en": "Berapa jumlah dari 5 bilangan genap pertama?", "id": "Tiga puluh." },
  { "en": "Jika sebuah ruangan berukuran 10x12 kaki, berapa luasnya?", "id": "120 kaki persegi." },
  { "en": "Apa angka selanjutnya: 5, 8, 13, 21, ...?", "id": "34 (deret Fibonacci)." },
  { "en": "Berapa banyak garis simetri pada sebuah segi lima beraturan?", "id": "Lima." },
  { "en": "Jika Anda memiliki 20 dolar dan membelanjakan 5, berapa persen yang tersisa?", "id": "Tujuh puluh lima persen." },
  { "en": "Apa itu bilangan komposit?", "id": "Bilangan yang bukan prima." },
  { "en": "Jika ada 7 orang di sebuah pesta, dan semua orang bersulang sekali, berapa banyak dentingan gelas yang terjadi?", "id": "Dua puluh satu." },
  { "en": "Berapa banyak angka 1 yang Anda tulis saat menulis 1 sampai 100?", "id": "Dua puluh satu." },
  { "en": "Apa yang memiliki 2 basis dan tidak ada simpul?", "id": "Silinder." },
  { "en": "Jika sebuah pizza dipotong 8 potong, dan Anda makan 3, berapa persen yang tersisa?", "id": "62.5 persen." },
  { "en": "Berapa jumlah dari 1 sampai 20?", "id": "Dua ratus sepuluh." },
  { "en": "Apa yang memiliki probabilitas nol?", "id": "Sesuatu yang tidak mungkin terjadi." },
  { "en": "Berapa banyak angka 2 digit yang bisa dibentuk dari angka 1, 2, 3, 4?", "id": "Enam belas." },
  { "en": "Jika sebuah kubus memiliki volume 27 cm³, berapa panjang sisinya?", "id": "Tiga sentimeter." },
  { "en": "Apa yang bisa Anda hitung menggunakan jari tetapi tidak bisa Anda sentuh?", "id": "Denyut nadi Anda." },
  { "en": "Berapa banyak angka 1 yang dibutuhkan untuk menulis semua nomor halaman dari 1 sampai 199?", "id": "Seratus empat puluh." },
  { "en": "Jika 4 orang bisa mengecat 4 ruangan dalam 4 jam, berapa lama 2 orang mengecat 2 ruangan?", "id": "Empat jam." },
  { "en": "Urutan berikutnya: 1, 1, 2, 3, 5, 8, ...?", "id": "13 (deret Fibonacci)." },
  { "en": "Seorang pria memiliki 15 koin, totalnya $1. Koin apa yang dimilikinya?", "id": "10 nikel, 5 dime (tidak mungkin)." },
  { "en": "Jika 12 + 3 = 3, kapan ini benar?", "id": "Pada jam (12 siang + 3 jam = 3 sore)." },
  { "en": "Berapa banyak cara untuk menyusun ulang huruf 'LEVEL'?", "id": "Tiga puluh cara." },
  { "en": "Jika sebuah pizza memiliki jari-jari 'z' dan ketebalan 'a', apa volumenya?", "id": "Pi * z * z * a." },
  { "en": "Mengapa angka Romawi X adalah sepuluh?", "id": "Dua V (5) digabungkan." },
  { "en": "Jika Anda membutuhkan 7 menit dan 11 menit jam pasir untuk mengukur 15 menit, bagaimana caranya?", "id": "Mulai keduanya, balik 7 saat habis." },
  { "en": "Apa angka selanjutnya: 8, 6, 7, 5, 6, 4, ...?", "id": "5 (polanya -2, +1)." },
  { "en": "Berapa banyak angka 0 dalam seratus triliun?", "id": "Empat belas." },
  { "en": "Jika Anda memiliki 26 huruf, tetapi kehilangan E, T, A, berapa yang tersisa?", "id": "Dua puluh tiga huruf." },
  { "en": "Berapa jumlah angka pada roda roulette?", "id": "666." },
  { "en": "Jika Anda memotong sepotong keju 3 kali, berapa potongan maksimum yang bisa Anda dapatkan?", "id": "Delapan potongan." },
  { "en": "Apa yang memiliki wajah dan dua tangan tetapi tidak ada lengan atau kaki?", "id": "Jam." },
  { "en": "Sebuah mobil menempuh jarak 180 mil. Separuh perjalanan dengan kecepatan 30 mph, separuh lagi 60 mph. Berapa kecepatan rata-ratanya?", "id": "40 mil per jam." },
  { "en": "Jika ada 10 pena dalam satu kotak, berapa pena dalam 10 kotak?", "id": "Seratus pena." },
  { "en": "Apa yang memiliki 1000 mata tetapi tidak bisa melihat?", "id": "Jalan setapak kerikil." },
  { "en": "Berapa banyak bilangan prima antara 20 dan 30?", "id": "Dua (23, 29)." },
  { "en": "Jika sebuah persegi dilipat menjadi dua, bentuk apa yang Anda dapatkan?", "id": "Persegi panjang." },
  { "en": "Bagaimana cara menulis 100 dengan enam angka 9?", "id": "99 + 99/99." },
  { "en": "Jika sebuah tim menang 7 dari 10 pertandingan, berapa persen kemenangannya?", "id": "Tujuh puluh persen." },
  { "en": "Angka berapa yang tidak memiliki perwakilan dalam angka Romawi?", "id": "Nol." },
  { "en": "Jika ada 5 saudara perempuan di sebuah ruangan dan masing-masing sedang membaca buku, berapa banyak orang di ruangan itu?", "id": "Lima orang." },
  { "en": "Berapa banyak tepi yang dimiliki oleh sebuah icosahedron?", "id": "Tiga puluh tepi." },
  { "en": "Gunakan empat angka 5 untuk membuat 100.", "id": "(5 x 5 x 5) - (5 x 5)." },
  { "en": "Jika Anda menggulirkan dua dadu, berapa probabilitas mendapatkan jumlah 12?", "id": "1 dari 36." },
  { "en": "Apa itu bilangan 'amicable'?", "id": "Dua bilangan yang jumlah pembaginya saling sama." },
  { "en": "Berapa banyak angka dari 1 sampai 100 yang merupakan kelipatan 5?", "id": "Dua puluh." },
  { "en": "Jika 3 orang bisa membangun 3 pagar dalam 3 hari, berapa lama 1 orang membangun 1 pagar?", "id": "Tiga hari." },
  { "en": "Berapa banyak garis simetri pada sebuah bintang berujung lima?", "id": "Lima." },
  { "en": "Jika dibutuhkan 1 galon untuk menempuh 25 mil, berapa galon untuk 100 mil?", "id": "Empat galon." },
  { "en": "Apa yang bisa Anda kalikan dengan 6 untuk mendapatkan 3?", "id": "Setengah." },
  { "en": "Berapa banyak angka 4 digit yang bisa Anda buat menggunakan 1, 1, 2, 2?", "id": "Enam angka." },
  { "en": "Jika sebuah persegi panjang memiliki keliling 20 dan panjang 6, berapa lebarnya?", "id": "Empat." },
  { "en": "Berapa banyak angka 8 yang Anda temukan saat menghitung dari 1 sampai 100?", "id": "Dua puluh." },
  { "en": "Apa yang memiliki satu ujung tapi tidak ada awal atau akhir?", "id": "Pelangi." },
  { "en": "Jika Anda membutuhkan 15 menit untuk berjalan 1 mil, berapa lama untuk 3 mil?", "id": "Empat puluh lima menit." },
  { "en": "Berapa banyak angka 2 yang Anda butuhkan untuk membuat 24?", "id": "Tiga (22+2)." },
  { "en": "Jika 1/3 dari suatu angka adalah 5, berapakah angka itu?", "id": "Lima belas." },
  { "en": "Berapa banyak angka 3 digit yang merupakan kelipatan 10?", "id": "Sembilan puluh." },
  { "en": "Apa itu 'pembuktian dengan kontradiksi'?", "id": "Membuktikan sesuatu benar dengan menunjukkan kebalikannya salah." },
  { "en": "Jika sebuah kereta meninggalkan stasiun pada pukul 8 pagi dan berjalan 50 mph, di mana kereta itu pada pukul 10 pagi?", "id": "100 mil dari stasiun." },
  { "en": "Berapa banyak angka dari 1 sampai 500 yang habis dibagi 3 dan 5?", "id": "Tiga puluh tiga." },
  { "en": "Jika Anda memiliki 17 apel dan semua kecuali 9 dimakan, berapa yang tersisa?", "id": "Sembilan." },
  { "en": "Berapa banyak nol dalam satu desiliun?", "id": "Tiga puluh tiga." },
  { "en": "Apa yang memiliki 12 huruf dan merupakan alat matematika?", "id": "Kalkulator." },
  { "en": "Jika Anda melipat selembar kertas menjadi dua 10 kali, setebal apa itu?", "id": "Lebih tebal dari buku." },
  { "en": "Berapa banyak angka 5 digit yang bisa dibuat dari 1, 2, 3, 4, 5?", "id": "Seratus dua puluh." },
  { "en": "Jika ada 20 siswa dan 1/4 dari mereka laki-laki, berapa banyak perempuan?", "id": "Lima belas perempuan." },
  { "en": "Apa itu 'Saringan Eratosthenes'?", "id": "Cara kuno untuk menemukan bilangan prima." },
  { "en": "Jika Anda memiliki 30 koin perak, dan teman Anda memiliki 20 koin emas, siapa yang memiliki lebih banyak koin?", "id": "Anda." },
  { "en": "Berapa banyak cara untuk mendapatkan jumlah 7 dari dua dadu?", "id": "Enam cara." },
  { "en": "Jika sebuah buku memiliki 250 halaman, berapa kali angka 2 muncul?", "id": "Delapan puluh enam kali." },
  { "en": "Berapa banyak detik dalam seminggu?", "id": "604.800 detik." },
  { "en": "Jika Anda memiliki 2 celana dan 3 kemeja, berapa banyak pakaian yang bisa Anda buat?", "id": "Enam pakaian." },
  { "en": "Apa yang memiliki probabilitas lebih dari 1?", "id": "Tidak ada." },
  { "en": "Berapa banyak angka 7 yang ada di antara 1 dan 111?", "id": "Dua puluh dua." },
  { "en": "Jika sebuah pesta memiliki 10 orang dan setiap orang berjabat tangan dengan yang lain, berapa banyak jabat tangan?", "id": "Empat puluh lima." },
  { "en": "Apa yang memiliki 32 panel dan digunakan dalam olahraga?", "id": "Bola sepak." },
  { "en": "Jika dibutuhkan 5 menit untuk berlari satu putaran, berapa putaran dalam satu jam?", "id": "Dua belas putaran." },
  { "en": "Berapa banyak angka romawi yang dibutuhkan untuk menulis 888?", "id": "Sepuluh (DCCCLXXXVIII)." },
  { "en": "Apa yang memiliki 64 kotak dan 32 buah?", "id": "Papan catur." },
  { "en": "Jika Anda memiliki 16 kaus kaki di laci, 8 hitam dan 8 putih. Berapa minimal yang harus diambil dalam gelap untuk mendapatkan sepasang?", "id": "Tiga kaus kaki." },
  { "en": "Berapa banyak sisi pada sebuah trapesium?", "id": "Empat sisi." },
  { "en": "Jika Anda mengalikan semua angka pada sebuah dadu, berapa hasilnya?", "id": "720." },
  { "en": "Apa yang bisa dipecahkan tetapi tidak pernah diperbaiki?", "id": "Rekor." },
  { "en": "Jika 3 jeruk berharga 60 sen, berapa harga selusin?", "id": "Dua ratus empat puluh sen ($2.40)." },
  { "en": "Berapa banyak angka dari 1 sampai 100 yang merupakan bilangan prima?", "id": "Dua puluh lima." },
  { "en": "Jika sebuah jam berbunyi setiap jam, berapa kali dalam sehari?", "id": "156 kali." },
  { "en": "Apa yang memiliki 13 sekop di dalamnya?", "id": "Satu set kartu." },
  { "en": "Berapa banyak sudut dalam sebuah kubus?", "id": "Dua puluh empat." },
  { "en": "Jika Anda membutuhkan 1 telur untuk setiap 2 cangkir tepung, berapa telur untuk 8 cangkir?", "id": "Empat telur." },
  { "en": "Apa yang selalu negatif?", "id": "Ruang kosong." },
  { "en": "Berapa banyak angka dari 1 sampai 100 yang bisa dibagi 9?", "id": "Sebelas." },
  { "en": "Jika Anda memutar koin 3 kali, berapa probabilitas mendapatkan semua kepala?", "id": "Satu dari delapan." },
  { "en": "Berapa banyak angka dalam satu lusin tukang roti?", "id": "Tiga belas." },
  { "en": "Jika sebuah ruangan memiliki 4 dinding, dan setiap dinding memiliki 4 jendela, berapa total jendela?", "id": "Enam belas jendela." },
  { "en": "Apa itu 'mean', 'median', dan 'mode'?", "id": "Ukuran tendensi sentral dalam statistik." },
  { "en": "Berapa banyak kartu dalam satu set standar?", "id": "Lima puluh dua." },
  { "en": "Jika dibutuhkan 3 orang 1 hari untuk menggali 1 lubang, berapa lama 1 orang menggali 3 lubang?", "id": "Sembilan hari." },
  { "en": "Apa itu bilangan Fibonacci?", "id": "Setiap angka adalah jumlah dari dua sebelumnya." },
  { "en": "Jika sebuah buku memiliki 10 bab, dan setiap bab memiliki 20 halaman, berapa total halaman?", "id": "Dua ratus halaman." },
  { "en": "Berapa banyak angka 0 yang ada di antara 1 dan 100?", "id": "Sebelas." },
  { "en": "Jika Anda memiliki 20 domba, dan 8 melompat pagar, berapa banyak yang tidak?", "id": "Dua belas." },
  { "en": "Apa yang memiliki 2 pasang sisi sejajar?", "id": "Jajar genjang." },
  { "en": "Berapa banyak angka yang ada di kalender?", "id": "Tiga puluh satu." },
  { "en": "Jika 5 apel berharga $1, berapa harga 20 apel?", "id": "Empat dolar." },
  { "en": "Apa itu 'Golden Ratio'?", "id": "Rasio matematis sekitar 1.618." },
  { "en": "Jika sebuah balapan panjangnya 10 km, berapa mil itu?", "id": "Sekitar 6.2 mil." },
  { "en": "Berapa banyak angka 3 digit yang kurang dari 200?", "id": "Seratus." },
  { "en": "Jika sebuah pesta dimulai pukul 7:30 malam dan berlangsung 4 jam, kapan selesai?", "id": "Pukul 11:30 malam." },
  { "en": "Apa yang bisa Anda bagi tetapi tidak bisa Anda ambil?", "id": "Kesalahan." },
  { "en": "Berapa banyak sudut lancip dalam sebuah segitiga siku-siku?", "id": "Dua." },
  { "en": "Jika Anda memiliki 40 permen dan makan 30, berapa persen yang Anda makan?", "id": "Tujuh puluh lima persen." },
  { "en": "Berapa banyak angka 0 pada akhir dari 50! (lima puluh faktorial)?", "id": "Dua belas angka nol." },
  { "en": "Jika sebuah belah ketupat memiliki 4 sisi, berapa banyak diagonalnya?", "id": "Dua diagonal." },
  { "en": "Apa yang bisa dihitung tetapi tidak memiliki akhir?", "id": "Angka." },
  { "en": "Berapa banyak bulan dalam satu abad?", "id": "Seribu dua ratus bulan." },
  { "en": "Jika 10% dari suatu angka adalah 7, berapakah angka itu?", "id": "Tujuh puluh." },
  { "en": "Urutan berikutnya: 1, 3, 6, 10, 15, ...?", "id": "21 (bilangan triangular)." },
  { "en": "Berapa banyak cara untuk menyusun 4 orang dalam satu baris?", "id": "Dua puluh empat cara (4 faktorial)." },
  { "en": "Jika sebuah persegi memiliki keliling 36, berapa luasnya?", "id": "Delapan puluh satu." },
  { "en": "Berapa banyak sisi pada sebuah tetrahedron?", "id": "Empat sisi." },
  { "en": "Jika sebuah jam dinding jatuh, kemungkinan angka mana yang akan pecah?", "id": "Tidak ada, angka biasanya dicat." },
  { "en": "Berapa banyak angka genap antara 1 dan 101?", "id": "Lima puluh." },
  { "en": "Jika Anda berinvestasi $100, naik 10% lalu turun 10%, berapa uang Anda sekarang?", "id": "$99, kurang dari semula." },
  { "en": "Berapa banyak angka 2 yang Anda tulis dari 1 sampai 100?", "id": "Dua puluh." },
  { "en": "Gunakan delapan angka 8 untuk membuat 100.", "id": "(888-88)/8 = 100." },
  { "en": "Jika 20 adalah XX dan 30 adalah XXX, apa itu 40?", "id": "XL." },
  { "en": "Berapa banyak rusuk yang dimiliki sebuah kubus?", "id": "Dua belas rusuk." },
  { "en": "Jika Anda menggandakan angka dan menguranginya dengan setengahnya, apa yang Anda dapatkan?", "id": "1.5 kali angka asli." },
  { "en": "Berapa banyak angka 3 digit yang bisa dibuat dari 0, 1, 2?", "id": "Empat (102, 120, 201, 210)." },
  { "en": "Jika sebuah buku di rak adalah yang ke-4 dari kiri dan ke-6 dari kanan, berapa banyak buku di rak?", "id": "Sembilan buku." },
  { "en": "Apa yang memiliki 18 lubang dan diikuti oleh banyak orang?", "id": "Lapangan golf." },
  { "en": "Berapa banyak detik dalam bulan Februari tahun kabisat?", "id": "2.505.600 detik." },
  { "en": "Jika Anda memiliki 30 permen, dan Anda memakan 2/3 nya, berapa yang tersisa?", "id": "Sepuluh permen." },
  { "en": "Apa yang memiliki satu suara tetapi banyak gema?", "id": "Matematika." },
  { "en": "Jika ada 10 apel dan Anda mengambil 4, berapa banyak yang Anda miliki?", "id": "Empat apel." },
  { "en": "Berapa banyak angka yang ada di papan dart?", "id": "Dua puluh." },
  { "en": "Jika sebuah tim mencetak rata-rata 3 gol dalam 5 pertandingan, berapa total golnya?", "id": "Lima belas gol." },
  { "en": "Berapa banyak angka 0 yang ada di antara 1 dan 200?", "id": "Tiga puluh satu." },
  { "en": "Apa yang memiliki banyak cabang tetapi tidak ada akar?", "id": "Diagram pohon." },
  { "en": "Jika 5 orang bisa membangun 1 rumah dalam 20 hari, berapa hari yang dibutuhkan 10 orang?", "id": "Sepuluh hari." },
  { "en": "Berapa jumlah dari 20 bilangan bulat positif pertama?", "id": "Dua ratus sepuluh." },
  { "en": "Berapa banyak angka prima satu digit?", "id": "Empat (2, 3, 5, 7)." },
  { "en": "Jika sebuah lingkaran memiliki diameter 14, berapa kelilingnya?", "id": "Empat belas pi." },
  { "en": "Jika A=1, B=2, C=3... apa kata dari 3-1-20?", "id": "CAT." },
  { "en": "Berapa banyak cara 5 buku bisa diatur di rak?", "id": "Seratus dua puluh cara." },
  { "en": "Jika Anda memiliki 2 koin senilai 15 sen, dan salah satunya bukan nikel, koin apa itu?", "id": "Satu dime dan satu nikel." },
  { "en": "Berapa banyak angka 1 yang ada di antara 1 dan 99?", "id": "Dua puluh." },
  { "en": "Berapa banyak hewan dari setiap jenis yang dibawa Alexander Agung ke dalam bahtera?", "id": "Dia tidak punya bahtera." },
  { "en": "Jika sebuah pesta memiliki 12 pasangan, berapa banyak orang di sana?", "id": "Dua puluh empat orang." },
  { "en": "Berapa banyak sisi pada sebuah heptahedron?", "id": "Tujuh sisi." },
  { "en": "Jika Anda membutuhkan 2 butir telur untuk satu kue, berapa kue yang bisa Anda buat dengan 10 butir telur?", "id": "Lima kue." },
  { "en": "Apa yang memiliki 13 berlian tetapi tidak kaya?", "id": "Satu set kartu remi." },
  { "en": "Berapa banyak angka 2 digit yang habis dibagi 5?", "id": "Delapan belas." },
  { "en": "Jika sebuah jam menunjukkan pukul 3:15, berapa sudut antara jarum jam dan menit?", "id": "7.5 derajat." },
  { "en": "Apa yang naik tapi tidak pernah bergerak?", "id": "Grafik." },
  { "en": "Berapa banyak angka 3 digit yang merupakan palindrom?", "id": "Sembilan puluh." },
  { "en": "Jika Anda memutar dadu, apa probabilitas mendapatkan bilangan genap?", "id": "Setengah atau 50%." },
  { "en": "Seorang anak memiliki 5 apel dan memberikan 3. Berapa persen yang tersisa?", "id": "Empat puluh persen." },
  { "en": "Apa yang memiliki 2 ujung runcing dan 1 bagian tengah?", "id": "Pensil." },
  { "en": "Berapa banyak angka 6 yang muncul dari 1 sampai 66?", "id": "Empat belas." },
  { "en": "Jika sebuah toko memberikan diskon 25% untuk barang seharga $20, berapa harga barunya?", "id": "Lima belas dolar." },
  { "en": "Berapa banyak angka yang ada di plat nomor standar?", "id": "Tergantung negara atau wilayah." },
  { "en": "Jika dibutuhkan 30 hari untuk mengisi botol, dan jumlahnya berlipat ganda setiap hari, kapan botol 1/4 penuh?", "id": "Pada hari ke dua puluh delapan." },
  { "en": "Apa itu bilangan 'aneh'?", "id": "Bilangan yang berlimpah tetapi tidak semi-sempurna." },
  { "en": "Berapa banyak angka prima kembar antara 1 dan 100?", "id": "Delapan pasang." },
  { "en": "Jika sebuah balok memiliki panjang 5, lebar 4, dan tinggi 3, berapa volumenya?", "id": "Enam puluh." },
  { "en": "Berapa banyak angka 9 yang Anda perlukan untuk membuat 100?", "id": "Dua (99 + 9/9)." },
  { "en": "Jika Anda memiliki 22 apel dan membaginya di antara 4 teman, berapa sisa apelnya?", "id": "Dua apel." },
  { "en": "Apa yang memiliki 100 kaki tetapi tidak bisa berjalan?", "id": "Lima puluh pasang sepatu." },
  { "en": "Berapa banyak angka dari 1 sampai 1000 yang memiliki setidaknya satu angka 1?", "id": "Dua ratus tujuh puluh satu." },
  { "en": "Jika sebuah sepeda motor berjalan 200 km dengan 5 liter bensin, berapa km per liter?", "id": "40 kilometer per liter." },
  { "en": "Apa yang memiliki 26 kunci tetapi tidak bisa membuka pintu?", "id": "Keyboard." },
  { "en": "Berapa banyak angka 3 digit yang habis dibagi 7?", "id": "Seratus dua puluh delapan." },
  { "en": "Jika 10 orang dapat mengecat 60 rumah dalam 120 hari, berapa hari yang dibutuhkan 5 orang untuk mengecat 30 rumah?", "id": "Seratus dua puluh hari." },
  { "en": "Berapa banyak angka yang ada pada termometer standar?", "id": "Tergantung pada skalanya." },
  { "en": "Jika Anda membutuhkan 1 jam untuk membaca 50 halaman, berapa lama untuk 200 halaman?", "id": "Empat jam." },
  { "en": "Apa yang memiliki 4 kuadran tetapi hanya satu dunia?", "id": "Grafik Kartesius." },
  { "en": "Berapa banyak angka dari 1 sampai 100 yang merupakan kuadrat sempurna?", "id": "Sepuluh." },
  { "en": "Jika sebuah pizza dipotong menjadi 12, dan Anda makan 1/4, berapa potong yang Anda makan?", "id": "Tiga potong." },
  { "en": "Berapa banyak cara untuk memilih 3 rasa es krim dari 5?", "id": "Sepuluh cara." },
  { "en": "Apa yang memiliki 2 mata tetapi tidak bisa melihat?", "id": "Dadu." },
  { "en": "Jika sebuah film berdurasi 90 menit, berapa jam itu?", "id": "Satu setengah jam." },
  { "en": "Berapa banyak angka 0 yang ada di akhir dari 20!?", "id": "Empat angka nol." },
  { "en": "Jika Anda memiliki 18 kaus kaki dan 1/3nya berlubang, berapa banyak yang bagus?", "id": "Dua belas kaus kaki." },
  { "en": "Apa yang memiliki 6 sisi tetapi bukan kubus?", "id": "Prisma heksagonal." },
  { "en": "Berapa banyak angka dari 1 sampai 100 yang habis dibagi 2 dan 3?", "id": "Enam belas." },
  { "en": "Jika sebuah kereta berjalan 75 mph, berapa lama untuk menempuh 300 mil?", "id": "Empat jam." },
  { "en": "Apa yang bisa diukur dalam 'kaki' tetapi tidak punya kaki?", "id": "Jarak." },
  { "en": "Berapa banyak angka 3 digit yang jumlah digitnya adalah 3?", "id": "Enam." },
  { "en": "Jika 100 kucing memakan 100 tikus dalam 100 hari, berapa lama 1 kucing memakan 1 tikus?", "id": "Seratus hari." },
  { "en": "Berapa banyak sisi pada sebuah nonagon?", "id": "Sembilan sisi." },
  { "en": "Jika Anda memiliki 20 koin dan 1/4nya adalah nikel, berapa banyak nikel yang Anda miliki?", "id": "Lima nikel." },
  { "en": "Apa yang memiliki 88 tombol tetapi tidak bisa mengetik?", "id": "Piano." },
  { "en": "Berapa banyak angka 4 digit yang bisa dibentuk dari 1, 2, 3, 4 tanpa pengulangan?", "id": "Dua puluh empat." },
  { "en": "Jika sebuah persegi panjang memiliki luas 48 dan panjang 8, berapa lebarnya?", "id": "Enam." },
  { "en": "Apa yang memiliki 2 tangan, 12 angka, dan 1 wajah?", "id": "Jam." },
  { "en": "Berapa banyak angka dari 1 sampai 100 yang tidak mengandung angka 7?", "id": "Delapan puluh." },
  { "en": "Jika sebuah resep membutuhkan 2 cangkir gula untuk 24 kue, berapa yang dibutuhkan untuk 12 kue?", "id": "Satu cangkir." },
  { "en": "Berapa banyak cara untuk duduk 6 orang di meja bundar?", "id": "Seratus dua puluh cara." },
  { "en": "Apa yang bisa dihitung dalam 'derajat' tetapi tidak panas?", "id": "Sudut." },
  { "en": "Jika Anda memiliki 11 apel di keranjang dan Anda mengambil 11, berapa yang tersisa?", "id": "Sebelas, Anda yang mengambilnya." },
  { "en": "Berapa banyak angka 0 yang ada dalam satu googolplex?", "id": "Satu googol." },
  { "en": "Jika sebuah tim memainkan 20 pertandingan dan menang 15, berapa rasio menang-kalahnya?", "id": "Tiga banding satu." },
  { "en": "Apa yang memiliki 16 ons dalam satu pon tetapi tidak berat?", "id": "Pertanyaan." },
  { "en": "Berapa banyak angka prima yang kurang dari 10?", "id": "Empat." },
  { "en": "Jika sebuah mobil menggunakan 1 liter bensin untuk 12 km, berapa yang dibutuhkan untuk 60 km?", "id": "Lima liter." },
  { "en": "Apa yang memiliki 14 pon dalam satu batu tetapi tidak bisa dilempar?", "id": "Satuan berat." },
  { "en": "Berapa banyak angka dari 1 sampai 100 yang merupakan kelipatan 8?", "id": "Dua belas." },
  { "en": "Jika Anda menggulirkan dadu 6 sisi, apa probabilitas tidak mendapatkan 6?", "id": "Lima dari enam." },
  { "en": "Berapa banyak cara untuk mengatur ulang huruf 'MISSISSIPPI'?", "id": "34.650 cara." },
  { "en": "Berapa banyak angka dari 1 sampai 1000 yang merupakan pangkat tiga sempurna?", "id": "Sepuluh." },
  { "en": "Jika sebuah buku memiliki 300 halaman, berapa total digit yang digunakan untuk penomoran?", "id": "792 digit." },
  { "en": "Apa yang bisa Anda kalikan dengan 7 untuk mendapatkan jawaban antara 40 dan 50?", "id": "Angka enam atau tujuh." },
  { "en": "Jika 10 mesin membutuhkan 10 menit untuk membuat 10 gadget, berapa lama 1 mesin membuat 1 gadget?", "id": "Sepuluh menit." },
  { "en": "Urutan berikutnya: 2, 6, 12, 20, 30, ...?", "id": "42 (n*(n+1))." },
  { "en": "Berapa banyak simpul (vertices) yang dimiliki oleh sebuah icosahedron?", "id": "Dua belas simpul." },
  { "en": "Jika Anda memiliki 27 bola tenis dan 3 kotak, bagaimana Anda menempatkan jumlah ganjil di setiap kotak?", "id": "Letakkan 25, 1, dan 1." },
  { "en": "Berapa banyak angka 4 digit yang semua digitnya ganjil?", "id": "625 angka." },
  { "en": "Jika ada 50 negara bagian di AS, berapa banyak yang berbatasan dengan Kanada?", "id": "Tiga belas negara bagian." },
  { "en": "Berapa banyak nol dalam satu kuadriliun?", "id": "Lima belas." },
  { "en": "Jika dibutuhkan 12 orang 9 hari untuk menyelesaikan pekerjaan, berapa lama 8 orang menyelesaikannya?", "id": "13.5 hari." },
  { "en": "Apa yang memiliki 4 sisi yang sama tetapi bukan bujur sangkar?", "id": "Belah ketupat." },
  { "en": "Jika Anda membutuhkan 8 ons tepung untuk 12 kue, berapa ons untuk 9 kue?", "id": "Enam ons." },
  { "en": "Berapa banyak angka antara 1 dan 100 yang jumlah digitnya 8?", "id": "Sepuluh angka." },
  { "en": "Jika sebuah jam berdetak sekali setiap detik, berapa kali dalam satu jam?", "id": "3600 kali." },
  { "en": "Apa itu bilangan 'vampir'?", "id": "Bilangan yang dapat difaktorkan dari digitnya sendiri." },
  { "en": "Jika Anda menulis 1 sampai 100, berapa kali Anda menulis angka genap?", "id": "Tergantung pada bagaimana Anda menghitungnya." },
  { "en": "Berapa banyak diagonal dalam sebuah oktagon (segi delapan)?", "id": "Dua puluh diagonal." },
  { "en": "Jika sebuah kolam diisi oleh 2 pipa (satu 2 jam, satu 3 jam), berapa lama jika keduanya dibuka?", "id": "1.2 jam atau 72 menit." },
  { "en": "Berapa banyak angka 5 yang ada antara 100 dan 200?", "id": "Dua puluh." },
  { "en": "Jika sebuah tim memenangkan 60% dari 30 pertandingan, berapa banyak yang mereka menangkan?", "id": "Delapan belas pertandingan." },
  { "en": "Berapa banyak segitiga yang ada di logo Mitsubishi?", "id": "Tiga belah ketupat, atau enam segitiga." },
  { "en": "Jika Anda memiliki 2 ember, 4 liter dan 9 liter, bagaimana Anda mengukur 6 liter?", "id": "Isi 9L, tuang ke 4L dua kali." },
  { "en": "Apa yang memiliki 366 hari sekali setiap 4 tahun?", "id": "Tahun kabisat." },
  { "en": "Berapa banyak angka 2 digit yang merupakan bilangan prima?", "id": "Dua puluh satu." },
  { "en": "Jika sebuah roda berputar 10 kali per menit, berapa banyak putaran dalam 2.5 jam?", "id": "1500 putaran." },
  { "en": "Apa itu 'Masalah Empat Warna'?", "id": "Peta dapat diwarnai dengan empat warna." },
  { "en": "Jika sebuah persegi dilukis di dalam lingkaran, mana yang lebih besar luasnya?", "id": "Lingkaran." },
  { "en": "Berapa banyak angka 0 yang Anda tulis untuk satu juta?", "id": "Enam." },
  { "en": "Berapa banyak cara untuk mendapatkan jumlah 10 dari dua dadu?", "id": "Tiga cara." },
  { "en": "Jika Anda memiliki 14 kaus kaki di laci, 7 pasang, berapa minimal yang harus ditarik untuk mendapatkan sepasang?", "id": "Delapan kaus kaki." },
  { "en": "Berapa banyak sisi pada sebuah dekagon?", "id": "Sepuluh sisi." },
  { "en": "Jika Anda membutuhkan 3 tomat untuk 1 saus, berapa yang dibutuhkan untuk 5 saus?", "id": "Lima belas tomat." },
  { "en": "Apa yang memiliki 1600 kotak di dalamnya?", "id": "Papan catur 3D." },
  { "en": "Jika Anda menambahkan semua angka dari -10 hingga 10, berapa hasilnya?", "id": "Nol." },
  { "en": "Berapa banyak angka dari 1 sampai 100 yang habis dibagi 4?", "id": "Dua puluh lima." },
  { "en": "Jika sebuah kereta berjarak 100 mil dan berjalan 50 mph, berapa lama untuk sampai?", "id": "Dua jam." },
  { "en": "Apa itu 'Pita Mobius'?", "id": "Permukaan satu sisi dan satu batas." },
  { "en": "Jika 1 dari 5 orang menyukai apel, berapa persen yang tidak?", "id": "Delapan puluh persen." },
  { "en": "Berapa banyak angka 3 digit yang dapat dibuat dari 7, 8, 9?", "id": "Enam angka." },
  { "en": "Jika Anda memiliki 100 koin, dan satu lebih ringan, berapa penimbangan minimal untuk menemukannya?", "id": "Lima kali." },
  { "en": "Berapa banyak angka yang ada di kalender Advent?", "id": "Dua puluh empat." },
  { "en": "Jika 2x + 3 = 11, berapa nilai x?", "id": "Empat." },
  { "en": "Berapa banyak angka prima yang genap?", "id": "Satu, yaitu angka 2." },
  { "en": "Jika sebuah balapan adalah 5000 meter, berapa kilometer itu?", "id": "Lima kilometer." },
  { "en": "Apa yang memiliki 8 planet dan 1 matahari?", "id": "Tata surya kita." },
  { "en": "Jika Anda memiliki 20 pertanyaan dan menjawab 15 dengan benar, berapa skor Anda?", "id": "Tujuh puluh lima persen." },
  { "en": "Berapa banyak angka dari 1 sampai 100 yang merupakan kelipatan 10?", "id": "Sepuluh." },
  { "en": "Jika sebuah kubus dicat di luar dan dipotong menjadi 64 kubus kecil, berapa yang dicat di satu sisi?", "id": "Dua puluh empat." },
  { "en": "Apa itu 'Identitas Euler'?", "id": "e^(iπ) + 1 = 0." },
  { "en": "Jika ada 30 siswa, dan rasio laki-laki ke perempuan adalah 2:3, berapa banyak perempuan?", "id": "Delapan belas perempuan." },
  { "en": "Berapa banyak cara untuk menyusun ulang huruf 'GOOGLE'?", "id": "Seratus delapan puluh cara." },
  { "en": "Jika Anda memutar koin dan dadu, berapa banyak hasil yang mungkin?", "id": "Dua belas." },
  { "en": "Berapa banyak angka 0 yang ada di akhir dari 1000!?", "id": "Dua ratus empat puluh sembilan." },
  { "en": "Jika sebuah segitiga memiliki sisi 5, 12, dan 13, apa jenisnya?", "id": "Segitiga siku-siku." },
  { "en": "Berapa banyak angka dari 1 sampai 100 yang tidak mengandung angka 1?", "id": "Delapan puluh." },
  { "en": "Jika sebuah film dimulai pukul 2:45 dan berdurasi 110 menit, kapan selesai?", "id": "Pukul 4:35." },
  { "en": "Apa yang bisa diukur dalam 'byte' tetapi tidak bisa dimakan?", "id": "Data komputer." },
  { "en": "Berapa banyak angka 3 digit yang jumlah digitnya 25?", "id": "Enam (799, 889, 898, 979, 988, 997)." },
  { "en": "Jika dibutuhkan 60 menit untuk memasak kalkun 10 pon, berapa lama untuk 20 pon?", "id": "Tergantung resepnya." },
  { "en": "Berapa banyak sisi pada sebuah undekagon?", "id": "Sebelas sisi." },
  { "en": "Jika Anda memiliki 10 pasang sepatu, berapa banyak sepatu individu yang Anda miliki?", "id": "Dua puluh sepatu." },
  { "en": "Apa yang memiliki 52 kartu dan 4 jenis?", "id": "Satu set kartu remi." },
  { "en": "Berapa banyak angka 4 digit yang merupakan palindrom?", "id": "Sembilan puluh." },
  { "en": "Jika sebuah mobil melaju 360 mil dengan 12 galon, berapa mil per galonnya?", "id": "Tiga puluh mil per galon." },
  { "en": "Apa yang memiliki 24 tulang rusuk dan 1 tulang dada?", "id": "Rangka manusia." },
  { "en": "Berapa banyak angka dari 1 sampai 100 yang merupakan bilangan kubik?", "id": "Empat (1, 8, 27, 64)." },
  { "en": "Jika resep membutuhkan 1/2 cangkir gula, berapa yang dibutuhkan untuk resep ganda?", "id": "Satu cangkir." },
  { "en": "Berapa banyak cara untuk memilih presiden dan wakil presiden dari 5 orang?", "id": "Dua puluh cara." },
  { "en": "Apa yang bisa dihitung dalam 'bar' tetapi tidak bisa diminum?", "id": "Tekanan atau musik." },
  { "en": "Jika Anda memiliki 100 kotak, dan Anda memberi nomor 1 sampai 100, berapa banyak angka 8 yang Anda tulis?", "id": "Dua puluh." },
  { "en": "Berapa banyak angka 0 yang ada dalam satu sextillion?", "id": "Dua puluh satu." },
  { "en": "Jika sebuah tim kalah 5 dari 25 pertandingan, berapa persen kemenangannya?", "id": "Delapan puluh persen." },
  { "en": "Apa yang memiliki 7 warna tetapi tidak bisa dipakai?", "id": "Pelangi." },
  { "en": "Berapa banyak angka prima yang ada antara 40 dan 50?", "id": "Tiga (41, 43, 47)." },
  { "en": "Jika sebuah pesawat terbang 1200 mil dalam 3 jam, berapa kecepatannya?", "id": "400 mil per jam." },
  { "en": "Apa yang memiliki 360 derajat tetapi tidak panas?", "id": "Lingkaran." },
  { "en": "Berapa banyak angka dari 1 sampai 200 yang habis dibagi 5?", "id": "Empat puluh." },
  { "en": "Jika Anda menggulirkan dua dadu, apa probabilitas mendapatkan 'snake eyes' (dua angka 1)?", "id": "Satu dari tiga puluh enam." },
  { "en": "Berapa banyak cara untuk menyusun ulang huruf 'BANANA'?", "id": "Enam puluh cara." },
  { "en": "Apa yang memiliki 6 kaki tetapi tidak bisa berjalan?", "id": "Penggaris." },
  { "en": "Jika Anda memiliki 12 kue dan 4 teman, berapa banyak yang didapat setiap teman?", "id": "Tiga kue." },
  { "en": "Berapa banyak angka 0 yang ada di akhir dari 30!?", "id": "Tujuh angka nol." },
  { "en": "Jika sebuah tim mencetak 120 poin dalam 4 kuarter, berapa rata-rata poin per kuarter?", "id": "Tiga puluh poin." },
  { "en": "Apa yang memiliki 10 baris dan 9 kolom?", "id": "Papan Sudoku." },
  { "en": "Berapa banyak angka dari 1 sampai 100 yang memiliki digit 2?", "id": "Sembilan belas." },
  { "en": "Jika sebuah resep untuk 6 orang membutuhkan 2 telur, berapa yang dibutuhkan untuk 3 orang?", "id": "Satu telur." },
  { "en": "Berapa banyak cara untuk memilih tim 3 orang dari 5 orang?", "id": "Sepuluh cara." },
  { "en": "Apa yang bisa dihitung dalam 'derajat' tetapi bukan suhu?", "id": "Sebuah diploma atau sudut." },
  { "en": "Jika Anda memiliki 20 koin di saku dan kehilangan 3, berapa yang tersisa?", "id": "Tujuh belas koin." },
  { "en": "Berapa banyak angka 0 yang ada dalam satu miliar?", "id": "Sembilan." },
  { "en": "Jika sebuah toko buka dari jam 9 pagi sampai 5 sore, berapa jam buka?", "id": "Delapan jam." },
  { "en": "Apa yang memiliki 8 sisi dan 8 sudut?", "id": "Oktagon." },
  { "en": "Berapa banyak angka prima yang ada antara 50 dan 60?", "id": "Dua (53, 59)." },
  { "en": "Jika sebuah mobil melaju 65 mph selama 4 jam, berapa jauh perjalanannya?", "id": "260 mil." },
  { "en": "Apa yang memiliki 4 musim tetapi tidak ada cuaca?", "id": "Kalender." },
  { "en": "Berapa banyak angka dari 1 sampai 100 yang merupakan kelipatan 6?", "id": "Enam belas." },
  { "en": "Jika Anda memutar roda dengan 8 bagian, apa probabilitas mendarat di satu bagian?", "id": "Satu dari delapan." }


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
