<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8" />
  <title>Joyeux anniversaire Alexia !</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <style>
    :root {
      --bg-smart: #1a0a12;
      --bg-ocean: #040813;
      --accent: #BD1343;      /* rose Alexia */
      --accent-light: #ffb3c6;
      --text: #ffffff;
      --sad: #4a4f63;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      color: var(--text);
      overflow: hidden;
      background: radial-gradient(circle at center, #3d1a2a, var(--bg-smart));
      transition: background 0.6s ease;
    }

    /* ===== ÉCRAN DE DÉPART ===== */
    .start-screen {
      text-align: center;
      transition: opacity 0.6s ease, transform 0.6s ease;
    }

    .start-screen.hidden {
      opacity: 0;
      pointer-events: none;
      transform: scale(0.95);
    }

    .btn-row {
      display: flex;
      gap: 16px;
      justify-content: center;
    }

    .choice-btn {
      border: none;
      padding: 16px 28px;
      font-size: 1.1rem;
      font-weight: 600;
      border-radius: 999px;
      cursor: pointer;
      transition: transform 0.15s, box-shadow 0.15s, background 0.2s;
      color: white;
      min-width: 120px;
    }

    .choice-btn.smart {
      background: linear-gradient(135deg, var(--accent), #e91e63);
      box-shadow: 0 6px 20px rgba(189, 19, 67, 0.5);
    }

    .choice-btn.smart:hover {
      transform: translateY(-2px) scale(1.03);
      box-shadow: 0 10px 28px rgba(189, 19, 67, 0.7);
    }

    .choice-btn.ocean {
      background: linear-gradient(135deg, #1f3b5b, #0b1827);
      box-shadow: 0 6px 20px rgba(7, 21, 40, 0.7);
    }

    .choice-btn.ocean:hover {
      transform: translateY(-2px) scale(1.03);
      box-shadow: 0 10px 28px rgba(3, 12, 26, 0.9);
    }

    .hint {
      margin-top: 16px;
      font-size: 0.9rem;
      opacity: 0.75;
    }

    /* ===== BOUTON RETOUR ===== */
    .back-btn {
      position: fixed;
      top: 16px;
      left: 16px;
      padding: 8px 14px;
      border-radius: 999px;
      border: none;
      font-size: 0.9rem;
      font-weight: 500;
      background: rgba(0, 0, 0, 0.4);
      color: #fff;
      cursor: pointer;
      display: none;
      backdrop-filter: blur(6px);
      z-index: 20;
    }

    .back-btn.visible {
      display: inline-flex;
      align-items: center;
      gap: 6px;
    }

    /* ===== CONTENU PRINCIPAL ===== */
    .main-content {
      position: absolute;
      text-align: center;
      padding: 24px;
      max-width: 560px;
      opacity: 0;
      pointer-events: none;
      transform: scale(0.9);
      transition: opacity 0.4s ease, transform 0.4s ease;
      z-index: 5;
    }

    .main-content.visible {
      opacity: 1;
      pointer-events: auto;
      transform: scale(1);
    }

    h1 {
      font-size: clamp(2.6rem, 8vw, 3.6rem);
      letter-spacing: 0.12em;
      text-transform: uppercase;
      margin-bottom: 16px;
      text-shadow: 0 0 20px rgba(0,0,0,0.6);
      animation: explodeIn 0.9s ease-out forwards;
      opacity: 0;
    }

    .name {
      font-size: clamp(1.8rem, 5.5vw, 2.6rem);
      margin-bottom: 18px;
      font-weight: 700;
      opacity: 0;
      animation: fadeInUp 1.1s ease-out 0.3s forwards;
    }

    p {
      font-size: 1rem;
      line-height: 1.7;
      margin-bottom: 24px;
      opacity: 0;
      animation: fadeIn 1s ease-out 0.6s forwards;
    }

    footer {
      opacity: 0;
      font-size: 0.8rem;
      margin-top: 8px;
      animation: fadeIn 1.2s ease-out 1s forwards;
    }

    /* Style Smart / Ocean */
    .main-content.smart h1 { color: var(--accent-light); }
    .main-content.smart .name { color: var(--accent); }

    .main-content.ocean h1 { color: #9fb3ff; }
    .main-content.ocean .name { color: #c3d4ff; }

    /* ===== COEUR (Smart) ===== */
    .heart {
      width: 80px;
      height: 80px;
      position: relative;
      display: inline-block;
      margin-bottom: 24px;
      animation: beat 1.2s infinite ease-in-out;
    }

    .heart::before,
    .heart::after {
      content: "";
      position: absolute;
      width: 40px;
      height: 60px;
      background: var(--accent);
      border-radius: 40px 40px 0 0;
      top: 20px;
    }

    .heart::before {
      left: 20px;
      transform: rotate(-45deg);
      transform-origin: bottom right;
    }

    .heart::after {
      right: 20px;
      transform: rotate(45deg);
      transform-origin: bottom left;
    }

    /* ===== FEUX D'ARTIFICE (Smart) ===== */
    #fireworksCanvas {
      position: fixed;
      inset: 0;
      pointer-events: none;
      z-index: 3;
    }

    /* ===== PLUIE (Ocean) ===== */
    #rainCanvas {
      position: fixed;
      inset: 0;
      pointer-events: none;
      z-index: 2;
    }

    /* ===== CACA EMOJI (Ocean) ===== */
    .confetti-sad {
      pointer-events: none;
      z-index: 4;
      font-size: 48px;
    }

    .confetti-sad span {
      position: fixed;
      top: -15vh;
      width: 50px;
      height: 50px;
      text-align: center;
      line-height: 50px;
      animation: fall 5s linear infinite;
      display: none;
    }

    .confetti-sad.visible span {
      display: block;
    }

    .confetti-sad span:nth-child(odd)  { color: #9aa4c4; } /* gouttes */
    .confetti-sad span:nth-child(even) { color: #d4a574; } /* caca */

    /* positions */
    .confetti-sad span:nth-child(1)  { left: 5%;  animation-delay: 0s; }
    .confetti-sad span:nth-child(2)  { left: 15%; animation-delay: 0.4s; }
    .confetti-sad span:nth-child(3)  { left: 25%; animation-delay: 0.8s; }
    .confetti-sad span:nth-child(4)  { left: 35%; animation-delay: 1.2s; }
    .confetti-sad span:nth-child(5)  { left: 45%; animation-delay: 1.6s; }
    .confetti-sad span:nth-child(6)  { left: 55%; animation-delay: 2s; }
    .confetti-sad span:nth-child(7)  { left: 65%; animation-delay: 2.4s; }
    .confetti-sad span:nth-child(8)  { left: 75%; animation-delay: 2.8s; }
    .confetti-sad span:nth-child(9)  { left: 85%; animation-delay: 3.2s; }
    .confetti-sad span:nth-child(10) { left: 50%; animation-delay: 3.6s; }
    .confetti-sad span:nth-child(11) { left: 10%; animation-delay: 0.2s; }
    .confetti-sad span:nth-child(12) { left: 90%; animation-delay: 1s; }

    /* ===== ANIMATIONS ===== */
    @keyframes explodeIn {
      0%   { transform: scale(0.4) translateY(40px); opacity: 0; }
      40%  { transform: scale(1.2) translateY(-8px); opacity: 1; }
      70%  { transform: scale(0.95) translateY(4px); }
      100% { transform: scale(1) translateY(0); }
    }

    @keyframes fadeInUp {
      0%   { opacity: 0; transform: translateY(20px); }
      100% { opacity: 1; transform: translateY(0); }
    }

    @keyframes fadeIn {
      0%   { opacity: 0; }
      100% { opacity: 1; }
    }

    @keyframes beat {
      0%, 100% { transform: scale(1); }
      25%      { transform: scale(1.2); }
      50%      { transform: scale(0.95); }
      75%      { transform: scale(1.1); }
    }

    @keyframes fall {
      0%   { transform: translateY(-10vh) rotate(0deg); }
      100% { transform: translateY(120vh) rotate(360deg); }
    }

    @media (max-width: 480px) {
      .main-content { padding: 20px; max-width: 100%; }
      p { font-size: 0.95rem; }
      .btn-row {
        flex-direction: column;
      }
      .choice-btn {
        width: 100%;
      }
    }
  </style>
</head>
<body>

  <!-- CANVAS FEUX D'ARTIFICE -->
  <canvas id="fireworksCanvas"></canvas>

  <!-- CANVAS PLUIE -->
  <canvas id="rainCanvas"></canvas>

  <!-- CACA + GOUTTES EMOJI -->
  <div class="confetti-sad" id="confettiSad">
    <span>💧</span><span>💩</span><span>💧</span><span>💩</span><span>💧</span>
    <span>💩</span><span>💧</span><span>💩</span><span>💧</span><span>💩</span>
    <span>💧</span><span>💩</span>
  </div>

  <!-- Bouton retour -->
  <button class="back-btn" id="backBtn">← Retour au menu</button>

  <!-- Écran de départ -->
  <div class="start-screen" id="startScreen">
    <div class="btn-row">
      <button class="choice-btn smart" id="btnSmart">Smart</button>
      <button class="choice-btn ocean" id="btnOcean">Ocean</button>
    </div>
    <p class="hint">Choisis ta version d'anniversaire… 😏</p>
  </div>

  <!-- Contenu principal -->
  <div class="main-content" id="mainContent">
    <h1 id="mainTitle"></h1>
    <div class="name" id="mainName"></div>
    <div class="heart" id="heart"></div>
    <p id="mainText"></p>
    <footer>Créé juste pour toi. Oui, toi, Alexia.</footer>
  </div>

  <script>
    const startScreen     = document.getElementById('startScreen');
    const btnSmart        = document.getElementById('btnSmart');
    const btnOcean        = document.getElementById('btnOcean');

    const mainContent     = document.getElementById('mainContent');
    const mainTitle       = document.getElementById('mainTitle');
    const mainName        = document.getElementById('mainName');
    const mainText        = document.getElementById('mainText');
    const heart           = document.getElementById('heart');

    const confettiSad     = document.getElementById('confettiSad');
    const backBtn         = document.getElementById('backBtn');

    const fireworksCanvas = document.getElementById('fireworksCanvas');
    const fireworksCtx    = fireworksCanvas.getContext('2d');

    const rainCanvas      = document.getElementById('rainCanvas');
    const rainCtx         = rainCanvas.getContext('2d');

    function resizeCanvas() {
      fireworksCanvas.width  = window.innerWidth;
      fireworksCanvas.height = window.innerHeight;
      rainCanvas.width       = window.innerWidth;
      rainCanvas.height      = window.innerHeight;
    }
    resizeCanvas();
    window.addEventListener('resize', resizeCanvas);

    let fireworksActive = false;
    let rainActive      = false;
    let particles       = [];
    let raindrops       = [];

    /* ===== FEUX D'ARTIFICE ===== */
    function createFirework(x, y, color) {
      const count = 40;
      for (let i = 0; i < count; i++) {
        const angle = (Math.PI * 2 * i) / count;
        const speed = 2 + Math.random() * 2.5;
        particles.push({
          x, y,
          vx: Math.cos(angle) * speed,
          vy: Math.sin(angle) * speed,
          alpha: 1,
          color
        });
      }
    }

    function updateFireworks() {
      fireworksCtx.fillStyle = 'rgba(0, 0, 0, 0.3)';
      fireworksCtx.fillRect(0, 0, fireworksCanvas.width, fireworksCanvas.height);

      particles.forEach((p, index) => {
        p.x += p.vx;
        p.y += p.vy;
        p.vy += 0.02;
        p.alpha -= 0.012;

        if (p.alpha <= 0) {
          particles.splice(index, 1);
          return;
        }

        fireworksCtx.save();
        fireworksCtx.globalAlpha = p.alpha;
        fireworksCtx.fillStyle = p.color;
        fireworksCtx.beginPath();
        fireworksCtx.arc(p.x, p.y, 3, 0, Math.PI * 2);
        fireworksCtx.fill();
        fireworksCtx.restore();
      });

      if (fireworksActive && particles.length < 60) {
        const cx = Math.random() * fireworksCanvas.width * 0.6 + fireworksCanvas.width * 0.2;
        const cy = Math.random() * fireworksCanvas.height * 0.5 + fireworksCanvas.height * 0.15;
        const colors = ['#ff9ff3', '#feca57', '#BD1343', '#ff6b6b', '#48dbfb'];
        const color = colors[Math.floor(Math.random() * colors.length)];
        createFirework(cx, cy, color);
      }
    }

    /* ===== PLUIE ===== */
    function initRain() {
      raindrops = [];
      for (let i = 0; i < 100; i++) {
        raindrops.push({
          x: Math.random() * rainCanvas.width,
          y: Math.random() * rainCanvas.height,
          speed: 4 + Math.random() * 3,
          length: 15 + Math.random() * 10
        });
      }
    }

    function updateRain() {
      rainCtx.fillStyle = 'rgba(10, 20, 60, 0.5)';
      rainCtx.fillRect(0, 0, rainCanvas.width, rainCanvas.height);

      rainCtx.strokeStyle = '#4a6fa5';
      rainCtx.lineWidth = 1.5;
      rainCtx.lineCap = 'round';

      raindrops.forEach(drop => {
        rainCtx.beginPath();
        rainCtx.moveTo(drop.x, drop.y);
        rainCtx.lineTo(drop.x, drop.y + drop.length);
        rainCtx.stroke();

        drop.y += drop.speed;

        if (drop.y > rainCanvas.height) {
          drop.y = -drop.length;
          drop.x = Math.random() * rainCanvas.width;
        }
      });
    }

    /* ===== BOUCLE PRINCIPALE ===== */
    function loop() {
      requestAnimationFrame(loop);

      if (fireworksActive) {
        updateFireworks();
      } else {
        fireworksCtx.clearRect(0, 0, fireworksCanvas.width, fireworksCanvas.height);
      }

      if (rainActive) {
        updateRain();
      } else {
        rainCtx.clearRect(0, 0, rainCanvas.width, rainCanvas.height);
      }
    }
    loop();

    function resetToMenu() {
      mainContent.classList.remove('visible', 'smart', 'ocean');
      startScreen.classList.remove('hidden');
      backBtn.classList.remove('visible');

      fireworksActive = false;
      rainActive      = false;
      fireworksCtx.clearRect(0, 0, fireworksCanvas.width, fireworksCanvas.height);
      rainCtx.clearRect(0, 0, rainCanvas.width, rainCanvas.height);

      confettiSad.classList.remove('visible');

      document.body.style.background =
        'radial-gradient(circle at center, #3d1a2a, ' +
        getComputedStyle(document.documentElement).getPropertyValue('--bg-smart') + ')';
    }

    function showSmart() {
      document.body.style.background =
        'radial-gradient(circle at center, #3d1a2a, ' +
        getComputedStyle(document.documentElement).getPropertyValue('--bg-smart') + ')';

      mainContent.classList.remove('ocean');
      mainContent.classList.add('smart');

      mainTitle.textContent = 'Happy Smart Birthday';
      mainName.textContent  = 'Alexia 💖';
      mainText.textContent  = "Que cette journée soit remplie de Smart sourires, et surtout de Smart apéro. ✨";

      heart.style.display = 'inline-block';
      confettiSad.classList.remove('visible');
      rainActive = false;
      rainCtx.clearRect(0, 0, rainCanvas.width, rainCanvas.height);

      startScreen.classList.add('hidden');
      backBtn.classList.add('visible');

      fireworksActive = true;

      setTimeout(() => {
        mainContent.classList.add('visible');
      }, 250);
    }

    function showOcean() {
      document.body.style.background =
        'radial-gradient(circle at center, #020716, ' +
        getComputedStyle(document.documentElement).getPropertyValue('--bg-ocean') + ')';

      mainContent.classList.remove('smart');
      mainContent.classList.add('ocean');

      mainTitle.textContent = 'Happy Ocean Birthday';
      mainName.textContent  = 'Alexia 🌧️';
      mainText.textContent  = "En espérant que cette journée ne sera pas pour toi un gros Ocean Caca. ✨";

      heart.style.display = 'none';
      confettiSad.classList.add('visible');
      initRain();
      rainActive = true;

      startScreen.classList.add('hidden');
      backBtn.classList.add('visible');

      fireworksActive = false;
      fireworksCtx.clearRect(0, 0, fireworksCanvas.width, fireworksCanvas.height);

      setTimeout(() => {
        mainContent.classList.add('visible');
      }, 250);
    }

    btnSmart.addEventListener('click', showSmart);
    btnOcean.addEventListener('click', showOcean);
    backBtn.addEventListener('click', resetToMenu);
  </script>
</body>
</html>
