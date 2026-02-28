<!DOCTYPE html>
<html lang="bn">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
  <title>For Mohona</title>
  <style>
    /* ===== THEME & VARIABLES ===== */
    :root {
      --bg-top: #ffe9f0;
      --bg-bottom: #fde0e7;
      --glass-bg: rgba(255, 255, 255, 0.5);
      --glass-border: rgba(255, 255, 255, 0.8);
      --text-main: #4a3b3c;
      --text-muted: #7a6366;
      --accent: #e88fa0;
      --accent-hover: #d67a8b;
      --shadow: rgba(232, 143, 160, 0.2);
    }

    * { margin: 0; padding: 0; box-sizing: border-box; }

    body {
      font-family: system-ui, -apple-system, sans-serif;
      color: var(--text-main);
      background: linear-gradient(135deg, var(--bg-top), var(--bg-bottom));
      min-height: 100vh;
      overflow: hidden;
      display: flex;
      align-items: center;
      justify-content: center;
      transition: background 1s ease;
      -webkit-tap-highlight-color: transparent;
      user-select: none;
    }

    /* ===== UI COMPONENTS ===== */
    .app-container {
      position: relative;
      width: 100%;
      max-width: 400px;
      height: 85vh;
      max-height: 700px;
      padding: 20px;
    }

    .glass-card {
      position: absolute;
      top: 20px; left: 20px; right: 20px; bottom: 20px;
      background: var(--glass-bg);
      backdrop-filter: blur(20px);
      -webkit-backdrop-filter: blur(20px);
      border: 1px solid var(--glass-border);
      border-radius: 30px;
      box-shadow: 0 20px 40px var(--shadow), inset 0 0 0 1px rgba(255,255,255,0.5);
      padding: 30px 25px;
      display: flex;
      flex-direction: column;
      justify-content: center;
      opacity: 0;
      pointer-events: none;
      transform: translateY(30px) scale(0.95);
      transition: all 0.6s cubic-bezier(0.25, 1, 0.5, 1);
    }

    .glass-card.active {
      opacity: 1;
      pointer-events: auto;
      transform: translateY(0) scale(1);
      z-index: 10;
    }

    .glass-card.prev {
      transform: translateY(-30px) scale(0.95);
    }

    /* ===== TYPOGRAPHY ===== */
    h1 { font-size: 1.6rem; font-weight: 600; margin-bottom: 15px; color: #b0465c; }
    p { font-size: 1.15rem; line-height: 1.6; margin-bottom: 15px; font-weight: 400; }
    .small-text { font-size: 0.9rem; color: var(--text-muted); margin-top: 10px; }
    
    .highlight {
      background: rgba(255, 255, 255, 0.6);
      padding: 15px;
      border-radius: 15px;
      border: 1px solid rgba(255,255,255,0.8);
      margin: 20px 0;
      font-weight: 500;
    }

    /* ===== BUTTONS & CONTROLS ===== */
    .btn {
      background: rgba(255, 255, 255, 0.8);
      border: 1px solid #fff;
      padding: 14px 28px;
      border-radius: 50px;
      font-size: 1.1rem;
      font-weight: 500;
      color: var(--text-main);
      cursor: pointer;
      box-shadow: 0 4px 15px rgba(0,0,0,0.05);
      transition: all 0.3s ease;
      margin-top: auto;
      align-self: center;
    }
    .btn:active { transform: scale(0.95); }
    .btn.primary { background: var(--accent); color: white; border: none; box-shadow: 0 8px 20px var(--shadow); }

    /* Progress Dots */
    .progress-bar {
      position: absolute;
      top: 30px; left: 0; width: 100%;
      display: flex; justify-content: center; gap: 8px; z-index: 50;
    }
    .dot { width: 8px; height: 8px; border-radius: 50%; background: rgba(0,0,0,0.1); transition: 0.3s; }
    .dot.active { background: var(--accent); transform: scale(1.2); }

    /* Hold to unlock button */
    .hold-btn-wrapper { position: relative; width: 120px; height: 120px; margin: 40px auto 0; }
    .hold-btn {
      width: 100%; height: 100%; border-radius: 50%;
      background: var(--glass-bg); border: 2px solid var(--accent);
      display: flex; align-items: center; justify-content: center;
      font-size: 1rem; color: var(--accent); cursor: pointer;
      z-index: 2; position: relative; transition: 0.2s;
    }
    .hold-ring {
      position: absolute; top: 0; left: 0; width: 100%; height: 100%;
      border-radius: 50%; background: var(--accent);
      transform: scale(0); opacity: 0.5; transform-origin: center; z-index: 1;
    }

    /* Interactive Bubbles */
    .bubbles-container { display: flex; flex-direction: column; gap: 10px; margin-top: 20px; }
    .reveal-bubble {
      background: rgba(255,255,255,0.7); border: 1px solid white;
      padding: 15px; border-radius: 15px; text-align: left;
      cursor: pointer; transition: 0.3s; position: relative; overflow: hidden;
    }
    .reveal-bubble.revealed { background: rgba(232, 143, 160, 0.1); color: #b0465c; }
    .reveal-bubble::after {
      content: 'Tap to read 🌸'; position: absolute; top: 0; left: 0; width: 100%; height: 100%;
      background: rgba(255,255,255,0.9); display: flex; align-items: center; justify-content: center;
      transition: 0.4s; font-size: 0.95rem; color: var(--text-muted);
    }
    .reveal-bubble.revealed::after { opacity: 0; pointer-events: none; }

    /* Decision Area */
    .decision-area { position: relative; height: 150px; width: 100%; margin-top: 30px; }
    .btn-yes, .btn-no { position: absolute; width: 120px; top: 20px; }
    .btn-yes { left: 10%; background: var(--accent); color: white; border: none; }
    .btn-no { right: 10%; }

    /* Tools */
    #musicToggle {
      position: fixed; top: 20px; right: 20px; width: 40px; height: 40px;
      border-radius: 50%; background: var(--glass-bg); border: 1px solid var(--glass-border);
      display: flex; align-items: center; justify-content: center; z-index: 100; cursor: pointer;
    }
    
    .particle {
      position: absolute; pointer-events: none; border-radius: 50%;
      background: var(--accent); animation: pop 1s cubic-bezier(0.1, 0.8, 0.3, 1) forwards;
    }
    @keyframes pop {
      0% { transform: translate(0,0) scale(1); opacity: 1; }
      100% { transform: translate(var(--tx), var(--ty)) scale(0); opacity: 0; }
    }
  </style>
</head>
<body>

  <audio id="bgMusic" loop>
    <source src="https://cdn.pixabay.com/download/audio/2022/05/16/audio_03d92ec3de.mp3?filename=romantic-piano-111000.mp3" type="audio/mpeg">
  </audio>

  <div id="musicToggle">🔇</div>

  <div class="progress-bar" id="progressDots"></div>

  <div class="app-container">
    
    <div class="glass-card active" id="page0">
      <h1 id="greetingText">শুভ দিন,</h1>
      <p>মোহনা...</p>
      <p class="small-text">কিছু কথা বলার ছিল। একটু সময় হবে?</p>
      
      <div class="hold-btn-wrapper">
        <div class="hold-ring" id="holdRing"></div>
        <div class="hold-btn" id="holdBtn">হোল্ড করো</div>
      </div>
      <p class="small-text" style="text-align: center; margin-top: 20px;">শুরু করতে চেপে ধরো</p>
    </div>

    <div class="glass-card" id="page1">
      <h1>জানো...</h1>
      <p>গত কয়েকটা দিন আমার একটু অন্যরকম কাটছে। খুব সাধারণ জিনিসগুলোও কেমন যেন স্পেশাল মনে হচ্ছে।</p>
      <p>অনেকক্ষণ ভেবে দেখলাম, কারণটা হয়তো তুমি। তোমার আশেপাশে থাকলে একটা অদ্ভুত শান্তি কাজ করে।</p>
      <button class="btn" onclick="nextPage()">পরের পাতা</button>
    </div>

    <div class="glass-card" id="page2">
      <p>তোমার কিছু ব্যাপার আমার খুব ভালো লাগে। এই যেমন...</p>
      
      <div class="bubbles-container">
        <div class="reveal-bubble" onclick="reveal(this)">১. তোমার স্নিগ্ধ হাসিটা, যেটা মন ভালো করে দেয়।</div>
        <div class="reveal-bubble" onclick="reveal(this)">২. তোমার কথা বলার শান্ত ধরন।</div>
        <div class="reveal-bubble" onclick="reveal(this)">৩. যেকোনো পরিস্থিতিতে তোমার ম্যাচিউরিটি।</div>
      </div>
      
      <p class="small-text" style="text-align: center; margin-top: 15px;" id="bubbleHint">সবগুলো ট্যাপ করো</p>
      <button class="btn primary" id="btnPage2" style="display: none; margin-top: 15px;" onclick="nextPage()">হুম, তারপর?</button>
    </div>

    <div class="glass-card" id="page3">
      <h1>সত্যি বলতে,</h1>
      <p>আমি আমাদের এই চেনা-জানার কোনো ভারী নাম দিতে চাই না। কোনো নাটকীয় প্রতিশ্রুতিও দিতে চাই না।</p>
      <p>শুধু মনে হলো, এই ফিলিংগুলো লুকিয়ে রাখার চেয়ে তোমাকে বলাটা বেশি জরুরি। তুমি স্পেশাল, আর আমি চাই তুমি সেটা জানো।</p>
      <button class="btn" onclick="nextPage()">সামনে চলো</button>
    </div>

    <div class="glass-card" id="page4">
      <p>তাই ভাবছিলাম...</p>
      <div class="highlight">
        সামনে একটা সুন্দর বিকেল কি একসাথে কাটানো যায়? এক কাপ চা, আর অনেক গল্প...
      </div>
      
      <div class="decision-area" id="decisionArea">
        <button class="btn btn-yes" onclick="nextPage()">হ্যাঁ, যায় ☕</button>
        <button class="btn btn-no" id="btnNo">না</button>
      </div>
    </div>

    <div class="glass-card" id="page5">
      <h1>অনেক ধন্যবাদ 🌸</h1>
      <p>আমার খুব ভালো লাগছে। কবে, কোথায় দেখা হচ্ছে—সেটা না হয় তুমিই ঠিক করো।</p>
      <p>আমি ওয়েট করব।</p>
      <div style="text-align: center; font-size: 3rem; margin-top: 20px;">🤍</div>
    </div>

  </div>

  <script>
    // --- State & Core Logic ---
    let currentPage = 0;
    const totalPages = 6;
    let revealedCount = 0;
    
    // Elements
    const cards = Array.from({length: totalPages}, (_, i) => document.getElementById(`page${i}`));
    const dotsContainer = document.getElementById('progressDots');
    const bgColors = [
      ['#ffe9f0', '#fde0e7'], // 0
      ['#eef2ff', '#e0e7ff'], // 1
      ['#fdf4ff', '#fae8ff'], // 2
      ['#f0fdf4', '#dcfce7'], // 3
      ['#fffbeb', '#fef3c7'], // 4
      ['#fff1f2', '#ffe4e6']  // 5
    ];

    // Initialize UI
    function init() {
      // Create Dots
      for(let i=0; i<totalPages; i++){
        let d = document.createElement('div');
        d.className = 'dot' + (i === 0 ? ' active' : '');
        dotsContainer.appendChild(d);
      }
      
      // Dynamic Greeting based on Time
      const hour = new Date().getHours();
      let greet = 'শুভ দিন,';
      if(hour >= 5 && hour < 12) greet = 'শুভ সকাল,';
      else if(hour >= 12 && hour < 17) greet = 'শুভ দুপুর,';
      else if(hour >= 17 && hour < 20) greet = 'শুভ বিকেল,';
      else greet = 'শুভ সন্ধ্যা,';
      document.getElementById('greetingText').innerText = greet;
    }

    // Navigation Logic
    function nextPage() {
      if (currentPage >= totalPages - 1) return;
      
      cards[currentPage].classList.remove('active');
      cards[currentPage].classList.add('prev');
      
      currentPage++;
      
      cards[currentPage].classList.remove('prev');
      cards[currentPage].classList.add('active');
      
      // Update Dots
      const dots = document.querySelectorAll('.dot');
      dots.forEach((d, i) => d.className = i <= currentPage ? 'dot active' : 'dot');
      
      // Update BG
      document.body.style.background = `linear-gradient(135deg, ${bgColors[currentPage][0]}, ${bgColors[currentPage][1]})`;

      // Trigger effects on specific pages
      if(currentPage === 5) startHearts();
    }

    // --- Interaction Logic 1: Hold to Unlock ---
    const holdBtn = document.getElementById('holdBtn');
    const holdRing = document.getElementById('holdRing');
    let holdTimer;
    let holdScale = 0;

    const startHold = (e) => {
      e.preventDefault();
      holdRing.style.transition = 'transform 1.5s linear';
      holdRing.style.transform = 'scale(1.5)';
      holdTimer = setTimeout(() => {
        createParticles(e.touches ? e.touches[0].clientX : e.clientX, e.touches ? e.touches[0].clientY : e.clientY);
        nextPage();
      }, 1500);
    };

    const endHold = () => {
      clearTimeout(holdTimer);
      holdRing.style.transition = 'transform 0.3s ease';
      holdRing.style.transform = 'scale(0)';
    };

    holdBtn.addEventListener('mousedown', startHold);
    holdBtn.addEventListener('touchstart', startHold, {passive: false});
    holdBtn.addEventListener('mouseup', endHold);
    holdBtn.addEventListener('mouseleave', endHold);
    holdBtn.addEventListener('touchend', endHold);

    // --- Interaction Logic 2: Reveal Bubbles ---
    window.reveal = function(el) {
      if(!el.classList.contains('revealed')){
        el.classList.add('revealed');
        revealedCount++;
        
        // Particle effect on tap
        const rect = el.getBoundingClientRect();
        createParticles(rect.left + rect.width/2, rect.top + rect.height/2, 5);

        if(revealedCount === 3) {
          document.getElementById('bubbleHint').style.display = 'none';
          const btn = document.getElementById('btnPage2');
          btn.style.display = 'block';
          btn.style.animation = 'pop 0.5s ease backwards';
        }
      }
    };

    // --- Interaction Logic 3: Smart Evading No Button ---
    const noBtn = document.getElementById('btnNo');
    const decisionArea = document.getElementById('decisionArea');

    const dodge = (e) => {
      e.preventDefault();
      const areaRect = decisionArea.getBoundingClientRect();
      const btnRect = noBtn.getBoundingClientRect();
      
      const maxX = areaRect.width - btnRect.width;
      const maxY = areaRect.height - btnRect.height;
      
      const newX = Math.random() * maxX;
      const newY = Math.random() * maxY;
      
      noBtn.style.left = `${newX}px`;
      noBtn.style.top = `${newY}px`;
      noBtn.style.right = 'auto'; // Reset right anchor
    };

    noBtn.addEventListener('mouseover', dodge);
    noBtn.addEventListener('touchstart', dodge, {passive: false});

    // --- Engine Logic: Particles ---
    function createParticles(x, y, count = 15) {
      for (let i = 0; i < count; i++) {
        const p = document.createElement('div');
        p.className = 'particle';
        p.style.left = x + 'px';
        p.style.top = y + 'px';
        p.style.width = Math.random() * 8 + 4 + 'px';
        p.style.height = p.style.width;
        
        const angle = Math.random() * Math.PI * 2;
        const velocity = Math.random() * 60 + 20;
        p.style.setProperty('--tx', Math.cos(angle) * velocity + 'px');
        p.style.setProperty('--ty', Math.sin(angle) * velocity + 'px');
        
        document.body.appendChild(p);
        setTimeout(() => p.remove(), 1000);
      }
    }

    // --- Engine Logic: Final Hearts ---
    function startHearts() {
      setInterval(() => {
        const h = document.createElement('div');
        h.innerHTML = '🤍';
        h.style.position = 'absolute';
        h.style.left = Math.random() * 100 + 'vw';
        h.style.top = '100vh';
        h.style.fontSize = Math.random() * 1.5 + 1 + 'rem';
        h.style.opacity = '0.7';
        h.style.pointerEvents = 'none';
        h.style.transition = 'transform 4s linear, opacity 4s linear';
        document.body.appendChild(h);
        
        setTimeout(() => {
          h.style.transform = `translateY(-120vh) rotate(${Math.random()*360}deg)`;
          h.style.opacity = '0';
        }, 50);
        
        setTimeout(() => h.remove(), 4050);
      }, 400);
    }

    // --- Audio Logic ---
    const musicToggle = document.getElementById('musicToggle');
    const bgMusic = document.getElementById('bgMusic');
    let isPlaying = false;
    
    musicToggle.addEventListener('click', () => {
      if(isPlaying) {
        bgMusic.pause();
        musicToggle.innerText = '🔇';
      } else {
        bgMusic.play();
        musicToggle.innerText = '🎵';
      }
      isPlaying = !isPlaying;
    });

    // Boot
    init();
  </script>
</body>
</html>
