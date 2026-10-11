[Uploading lagu-kita.mp3…]()
let currentPage = 0;
const pages = document.querySelectorAll('.page');

// Navigasi Pindah Halaman
function showPage(index) {
    pages.forEach((page, i) => {
        page.classList.remove('active');
        if (i === index) {
            page.classList.add('active');
        }
    });
    currentPage = index;
    window.scrollTo({ top: 0, behavior: 'smooth' });
}

function nextPage() {
    if (currentPage < pages.length - 1) {
        showPage(currentPage + 1);
    }
}

function prevPage() {
    if (currentPage > 0) {
        showPage(currentPage - 1);
    }
}

function goToPage(index) {
    showPage(index);
}

// Fitur Tombol Ganti Mode (HP / Komputer)
const modeToggleBtn = document.getElementById('modeToggle');
const bodyElement = document.body;

if (modeToggleBtn) {
    modeToggleBtn.addEventListener('click', () => {
        if (bodyElement.classList.contains('desktop-mode')) {
            bodyElement.classList.remove('desktop-mode');
            bodyElement.classList.add('mobile-mode');
            modeToggleBtn.innerText = "💻 Mode Komputer";
        } else {
            bodyElement.classList.remove('mobile-mode');
            bodyElement.classList.add('desktop-mode');
            modeToggleBtn.innerText = "📱 Mode HP";
        }
    });
}

// Fitur Tiup Lilin Meriah & Konfeti Perayaan
const blowBtn = document.getElementById('blowBtn');
const flame = document.getElementById('flame');
const surpriseText = document.getElementById('blowSurprise');

if (blowBtn) {
    blowBtn.addEventListener('click', () => {
        flame.classList.add('off');
        blowBtn.innerText = "✨ Lilin Berhasil Ditiup! ✨";
        blowBtn.style.background = "#2ed573";
        blowBtn.disabled = true;
        
        if (surpriseText) {
            surpriseText.classList.remove('hidden');
        }
        
        launchConfetti();
    });
}

// Sistem Efek Konfeti Canvas
function launchConfetti() {
    const canvas = document.getElementById('confettiCanvas');
    const ctx = canvas.getContext('2d');
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;

    const particles = [];
    const colors = ['#ff4757', '#ffa502', '#2ed573', '#1e90ff', '#3742fa', '#ed4245'];

    for (let i = 0; i < 100; i++) {
        particles.push({
            x: canvas.width / 2,
            y: canvas.height / 2,
            vx: (Math.random() - 0.5) * 10,
            vy: (Math.random() - 0.7) * 10,
            size: Math.random() * 7 + 3,
            color: colors[Math.floor(Math.random() * colors.length)],
            gravity: 0.15,
            opacity: 1
        });
    }

    function animate() {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        let activeParticles = 0;

        particles.forEach(p => {
            if (p.opacity > 0) {
                p.x += p.vx;
                p.y += p.vy;
                p.vy += p.gravity;
                p.opacity -= 0.01;

                ctx.save();
                ctx.globalAlpha = Math.max(p.opacity, 0);
                ctx.fillStyle = p.color;
                ctx.beginPath();
                ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2);
                ctx.fill();
                ctx.restore();

                activeParticles++;
            }
        });

        if (activeParticles > 0) {
            requestAnimationFrame(animate);
        }
    }

    animate();
}

// Efek Partikel Hati Melayang Latar Belakang
function createHeart() {
    const container = document.getElementById('heartsContainer');
    if (!container) return;
    
    const heart = document.createElement('div');
    heart.classList.add('heart');
    heart.innerHTML = '❤️';
    heart.style.left = Math.random() * 100 + 'vw';
    heart.style.animationDuration = Math.random() * 2 + 3 + 's';
    heart.style.fontSize = Math.random() * 12 + 10 + 'px';
    
    container.appendChild(heart);
    
    setTimeout(() => {
        heart.remove();
    }, 5000);
}

setInterval(createHeart, 500);

// Pemutar Musik MP3
const musicBtn = document.getElementById('musicToggle');
const bgMusic = document.getElementById('bgMusic');
let isPlaying = false;

if (musicBtn && bgMusic) {
    musicBtn.addEventListener('click', () => {
        if (!isPlaying) {
            bgMusic.play().then(() => {
                isPlaying = true;
                musicBtn.innerText = "⏸️ Matikan Musik";
            }).catch(error => {
                console.log("Gagal memutar audio:", error);
                alert("Pastikan Anda sudah menaruh file musik mp3 dengan nama 'lagu-kita.mp3' di folder yang sama!");
            });
        } else {
            bgMusic.pause();
            isPlaying = false;
            musicBtn.innerText = "🎵 Putar Musik";
        }
    });
    bgMusic.src = "lagu-kita.mp3";

}
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Poppins', sans-serif;
    background: linear-gradient(135deg, #ff9a9e 0%, #fecfef 99%, #fecfef 100%);
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    overflow-x: hidden;
    color: #4a4a4a;
    transition: background 0.3s ease;
}

/* Canvas Konfeti */
#confettiCanvas {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    pointer-events: none;
    z-index: 99;
}

/* Tombol Kontrol Pojok Kanan Atas */
.top-controls {
    position: fixed;
    top: 15px;
    right: 15px;
    display: flex;
    gap: 8px;
    z-index: 100;
}

.control-btn {
    background: rgba(255, 255, 255, 0.9);
    border: none;
    padding: 8px 14px;
    border-radius: 20px;
    cursor: pointer;
    font-size: 0.82rem;
    font-weight: 600;
    box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    transition: all 0.2s;
}

.control-btn:hover {
    background: #ffffff;
    transform: translateY(-2px);
}

/* Screen Wrapper untuk Mode Komputer vs HP */
.screen-wrapper {
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px;
}

/* === MODE KOMPUTER (Default) === */
body.desktop-mode .screen-wrapper {
    max-width: 650px;
}

body.desktop-mode .content-box {
    border-radius: 24px;
    padding: 40px 35px;
}

/* === MODE HP (Mockup Smartphone di Komputer) === */
body.mobile-mode {
    background: #2f3542; /* Latar belakang gelap elegan saat mode HP di komputer */
}

body.mobile-mode .screen-wrapper {
    max-width: 390px;
    height: 820px;
    background: #000;
    border-radius: 40px;
    padding: 15px;
    box-shadow: 0 25px 50px rgba(0,0,0,0.4), inset 0 0 10px rgba(255,255,255,0.2);
    border: 6px solid #485460;
    overflow-y: auto;
    margin: auto;
}

body.mobile-mode .slider-container {
    padding: 0;
}

body.mobile-mode .content-box {
    border-radius: 25px;
    padding: 25px 18px;
    background: rgba(255, 255, 255, 0.96);
}

/* Sembunyikan scrollbar bawaan di mode HP agar rapi */
body.mobile-mode .screen-wrapper::-webkit-scrollbar {
    display: none;
}

/* Container Per-Halaman */
.slider-container {
    width: 100%;
    position: relative;
    z-index: 10;
}

.page {
    display: none;
    opacity: 0;
    transition: opacity 0.4s ease-in-out;
}

.page.active {
    display: block;
    opacity: 1;
}

.content-box {
    backdrop-filter: blur(10px);
    box-shadow: 0 15px 35px rgba(0,0,0,0.1);
    text-align: center;
}

/* Typografi */
.tag {
    background: #ffe3e3;
    color: #ff4757;
    font-size: 0.82rem;
    padding: 5px 14px;
    border-radius: 20px;
    font-weight: 600;
}

.title {
    font-size: 1.7rem;
    color: #2f3542;
    margin: 12px 0 8px;
}

.handwriting {
    font-family: 'Sacramento', cursive;
    font-size: 2.3rem;
    color: #ff4757;
    margin-bottom: 12px;
}

.subtitle {
    font-size: 0.9rem;
    color: #747d8c;
    margin-bottom: 15px;
}

.letter-body {
    font-size: 0.95rem;
    line-height: 1.6;
    color: #2f3542;
    text-align: left;
    margin: 15px 0;
}

.letter-body p {
    margin-bottom: 10px;
}

/* Tombol Navigasi */
.btn {
    border: none;
    padding: 11px 22px;
    border-radius: 30px;
    font-size: 0.9rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s ease;
    margin: 4px;
}

.btn-next {
    background: #ff4757;
    color: white;
}

.btn-next:hover {
    background: #ff6b81;
    transform: translateY(-2px);
}

.btn-prev {
    background: #f1f2f6;
    color: #747d8c;
}

.btn-secondary {
    background: #ff758c;
    color: white;
}

.nav-buttons {
    display: flex;
    justify-content: space-between;
    margin-top: 20px;
}

/* Visual Kue & Lilin */
.cake-wrapper {
    margin: 25px 0;
    display: flex;
    justify-content: center;
}

.cake {
    width: 110px;
    height: 65px;
    background: #ff758c;
    border-radius: 10px 10px 0 0;
    position: relative;
    box-shadow: inset 0 8px 0 #ff8fa3;
}

.candle {
    width: 12px;
    height: 32px;
    background: #f1c40f;
    position: absolute;
    top: -32px;
    left: 50%;
    transform: translateX(-50%);
    border-radius: 3px;
}

.flame {
    width: 13px;
    height: 18px;
    background: #ff9f43;
    border-radius: 50% 50% 20% 20%;
    position: absolute;
    top: -16px;
    left: 50%;
    transform: translateX(-50%);
    box-shadow: 0 0 10px #ff9f43;
    animation: flicker 0.6s infinite alternate;
}

.flame.off {
    display: none;
}

@keyframes flicker {
    from { opacity: 0.8; transform: translateX(-50%) scale(0.9); }
    to { opacity: 1; transform: translateX(-50%) scale(1.1); }
}

.surprise-text {
    margin: 12px 0;
    font-size: 1rem;
    font-weight: 600;
    color: #ff4757;
    animation: popIn 0.5s ease-out forwards;
}

.surprise-text.hidden {
    display: none;
}

@keyframes popIn {
    0% { transform: scale(0.5); opacity: 0; }
    100% { transform: scale(1); opacity: 1; }
}

/* Halaman Foto Polaroid */
.solo-photos-grid {
    display: flex;
    flex-direction: column;
    gap: 15px;
    margin: 15px 0;
}

.polaroid-card {
    background: #ffffff;
    padding: 10px 10px 15px 10px;
    border-radius: 8px;
    box-shadow: 0 6px 15px rgba(0,0,0,0.06);
}

.photo-frame img {
    width: 100%;
    height: 200px;
    object-fit: cover;
    border-radius: 5px;
}

.polaroid-caption {
    font-family: 'Sacramento', cursive;
    font-size: 1.5rem;
    color: #4a4a4a;
    margin-top: 8px;
}

/* Galeri Foto Berdua */
.gallery-grid {
    display: grid;
    grid-template-columns: 1fr;
    gap: 12px;
    margin: 15px 0;
}

.photo-card {
    background: #fff;
    padding: 8px;
    border-radius: 12px;
    box-shadow: 0 4px 12px rgba(0,0,0,0.05);
}

.img-placeholder img {
    width: 100%;
    height: 170px;
    object-fit: cover;
    border-radius: 8px;
}

.caption {
    font-size: 0.8rem;
    color: #57606f;
    margin-top: 6px;
}

/* Cards Alasan (Flip Cards) */
.reasons-container {
    display: flex;
    flex-direction: column;
    gap: 10px;
    margin: 15px 0;
}

.reason-card {
    height: 70px;
    perspective: 1000px;
    cursor: pointer;
}

.card-inner {
    position: relative;
    width: 100%;
    height: 100%;
    transition: transform 0.6s;
    transform-style: preserve-3d;
}

.reason-card.flipped .card-inner {
    transform: rotateY(180deg);
}

.card-front, .card-back {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
    border-radius: 10px;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 10px;
    font-size: 0.85rem;
    text-align: center;
}

.card-front {
    background: #ffeaa7;
    color: #d63031;
    font-weight: 600;
}

.card-back {
    background: #ff7675;
    color: white;
    transform: rotateY(180deg);
}

/* Partikel Hati Latar Belakang */
.hearts-container {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    z-index: 0;
}

.heart {
    position: absolute;
    color: rgba(255, 71, 87, 0.35);
    animation: floatUp 4s linear infinite;
}

@keyframes floatUp {
    0% { transform: translateY(100vh) scale(0); opacity: 1; }
    100% { transform: translateY(-10vh) scale(1); opacity: 0; }
}
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Selamat Ulang Tahun, Sayang! ❤️</title>
    <link rel="stylesheet" href="style.css">
    <!-- Font Google -->
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&family=Sacramento&display=swap" rel="stylesheet">
</head>
<body class="desktop-mode">

    <!-- Tombol Kontrol Atas (Musik & Mode Tampilan) -->
    <div class="top-controls">
        <button id="musicToggle" class="control-btn">🎵 Putar Musik</button>
        <button id="modeToggle" class="control-btn">📱 Mode HP</button>
        <audio id="bgMusic" src="lagu-kita.mp3" loop></audio>
    </div>



    <!-- Partikel Hati & Konfeti -->
    <div class="hearts-container" id="heartsContainer"></div>
    <canvas id="confettiCanvas"></canvas>

    <!-- Wrapper Utama (Bisa berubah ukuran via tombol mode) -->
    <div class="screen-wrapper" id="screenWrapper">
        <div class="slider-container">
            
            <!-- HALAMAN 1: Sambutan & Tiup Lilin Meriah -->
            <section class="page active" id="page1">
                <div class="content-box">
                    <span class="tag">Hari Spesial Kamu 🎉</span>
                    <h1 class="title">Selamat Ulang Tahun!</h1>
                    <p class="subtitle">Tiup lilinnya dan buat harapan terbaik kamu hari ini ya✨</p>
                    
                    <div class="cake-wrapper">
                        <div class="cake">
                            <div class="candle">
                                <div class="flame" id="flame"></div>
                            </div>
                        </div>
                    </div>

                    <div class="action-area">
                        <button id="blowBtn" class="btn btn-secondary">💨 Tiup Lilinnya!</button>
                    </div>

                    <div id="blowSurprise" class="surprise-text hidden">
                        ✨ Make a wish! Semoga semua impianmu terwujud! 🎂🎉
                    </div>
                    
                    <button class="btn btn-next" onclick="nextPage()">Lanjut ke Halaman Berikutnya ➔</button>
                </div>
            </section>

            <!-- HALAMAN 2: Ucapan Manusiawi & Hangat -->
            <section class="page" id="page2">
                <div class="content-box">
                    <h2 class="handwriting">Untuk Kamu yang Paling Berarti...</h2>
                    <div class="letter-body">
                        <p>Selamat bertambah usia ya!</p>
                        <p>Terima kasih sudah bertahan sampai hari ini, melewati banyak hal baik yang bikin kamu tersenyum maupun hari-hari berat yang mungkin bikin kamu capek.</p>
                        <p>Aku bersyukur banget bisa ada di samping kamu, ngeliat kamu tumbuh, dan melangkah bareng kamu. Semoga di usia yang baru ini, langkah kamu terasa lebih ringan, hati kamu makin tenang, dan hal-hal baik selalu menemukan jalannya ke kamu.</p>
                        <p>Jangan pernah ragu sama diri kamu sendiri ya, karena buat aku, kamu selalu mengagumkan dengan cara kamu sendiri. ❤️</p>
                    </div>
                    <div class="nav-buttons">
                        <button class="btn btn-prev" onclick="prevPage()">⬅ Kembali</button>
                        <button class="btn btn-next" onclick="nextPage()">Lanjut ➔</button>
                    </div>
                </div>
            </section>

            <!-- HALAMAN 3: Halaman Khusus Foto Dia (3 Foto) -->
            <section class="page" id="page3">
                <div class="content-box">
                    <h2>Semua Tentang Kamu 🌸</h2>
                    <p class="subtitle">Beberapa potret favorit aku dari kamu yang selalu bikin terpana.</p>

                    <div class="solo-photos-grid">
                        <div class="polaroid-card">
                            <div class="photo-frame">
                                <img src="foto1.jpg" alt="Foto Pacar 1" onerror="this.src='https://images.unsplash.com/photo-1534528741775-53994a69daeb?q=80&w=500'">
                            </div>
                            <p class="polaroid-caption">Senyum tercantik di dunia 😊</p>
                        </div>

                        <div class="polaroid-card">
                            <div class="photo-frame">
                                <img src="foto2.jpg" alt="Foto Pacar 2" onerror="this.src='https://images.unsplash.com/photo-1517841905240-472988babdf9?q=80&w=500'">
                            </div>
                            <p class="polaroid-caption">Selalu manis di setiap suasana ✨</p>
                        </div>

                        <div class="polaroid-card">
                            <div class="photo-frame">
                                <img src="foto3.jpg" alt="Foto Pacar 3" onerror="this.src='https://images.unsplash.com/photo-1524504388940-b1c1722653e1?q=80&w=500'">
                            </div>
                            <p class="polaroid-caption">Favorit aku sepanjang masa ❤️</p>
                        </div>
                    </div>

                    <div class="nav-buttons">
                        <button class="btn btn-prev" onclick="prevPage()">⬅ Kembali</button>
                        <button class="btn btn-next" onclick="nextPage()">Lanjut ➔</button>
                    </div>
                </div>
            </section>

            <!-- HALAMAN 4: Galeri Kenangan Berdua -->
            <section class="page" id="page4">
                <div class="content-box">
                    <h2>Sedikit Cerita Tentang Kita 📸</h2>
                    <p class="subtitle">Setiap momen sama kamu selalu punya tempat tersendiri di hati aku.</p>

                    <div class="gallery-grid">
                        <div class="photo-card">
                            <div class="img-placeholder">
                                <img src="berdua1.jpg" alt="Foto Berdua 1" onerror="this.src='https://images.unsplash.com/photo-1518199266791-5375a83190b7?q=80&w=500'">
                            </div>
                            <p class="caption">Senyum kamu yang selalu bikin suasana jadi hangat.</p>
                        </div>

                        <div class="photo-card">
                            <div class="img-placeholder">
                                <img src="berdua2.jpg" alt="Foto Berdua 2" onerror="this.src='https://images.unsplash.com/photo-1522673607200-164d1b6ce486?q=80&w=500'">
                            </div>
                            <p class="caption">Momen receh dan tenang yang selalu aku tunggu setiap harinya.</p>
                        </div>
                    </div>

                    <div class="nav-buttons">
                        <button class="btn btn-prev" onclick="prevPage()">⬅ Kembali</button>
                        <button class="btn btn-next" onclick="nextPage()">Lanjut ➔</button>
                    </div>
                </div>
            </section>

            <!-- HALAMAN 5: Alasan Mengapa Aku Mencintaimu -->
            <section class="page" id="page5">
                <div class="content-box">
                    <h2>Alasan Kenapa Aku Sayang Kamu ✨</h2>
                    <p class="subtitle">Klik setiap kartu untuk melihatnya satu per satu:</p>

                    <div class="reasons-container">
                        <div class="reason-card" onclick="this.classList.toggle('flipped')">
                            <div class="card-inner">
                                <div class="card-front">1. Cara Kamu Perhatian 💖</div>
                                <div class="card-back">Hal-hal kecil yang kamu lakukan selalu sukses bikin aku merasa dihargai dan dicintai.</div>
                            </div>
                        </div>

                        <div class="reason-card" onclick="this.classList.toggle('flipped')">
                            <div class="card-inner">
                                <div class="card-front">2. Ketabahan Kamu 🌟</div>
                                <div class="card-back">Melihat kamu berjuang dan pantang menyerah selalu bikin aku makin kagum sama kamu.</div>
                            </div>
                        </div>

                        <div class="reason-card" onclick="this.classList.toggle('flipped')">
                            <div class="card-inner">
                                <div class="card-front">3. Rumah Buat Aku 🏡</div>
                                <div class="card-back">Cuma sama kamu aku bisa jadi diri sendiri tanpa perlu takut dinilai apa-apa.</div>
                            </div>
                        </div>
                    </div>

                    <div class="nav-buttons">
                        <button class="btn btn-prev" onclick="prevPage()">⬅ Kembali</button>
                        <button class="btn btn-next" onclick="nextPage()">Lanjut ➔</button>
                    </div>
                </div>
            </section>

            <!-- HALAMAN 6: Penutup & Harapan -->
            <section class="page" id="page6">
                <div class="content-box">
                    <h1 class="handwriting">Sekali lagi, Selamat Ulang Tahun!</h1>
                    <p class="letter-body" style="text-align: center;">
                        Makasih ya sudah hadir di dunia ini dan jadi bagian terindah dalam hidup aku.<br><br>
                        Aku selalu ada di sini buat kamu, di hari baik maupun hari berat.<br><br>
                        <strong>I love you so much! ❤️</strong>
                    </p>
                    <div class="nav-buttons" style="justify-content: center; gap: 10px;">
                        <button class="btn btn-prev" onclick="prevPage()">⬅ Kembali</button>
                        <button class="btn btn-secondary" onclick="goToPage(0)">🔄 Ulangi Dari Awal</button>
                    </div>
                </div>
            </section>

        </div>
    </div>

    <script src="script.js"></script>
</body>
</html>
