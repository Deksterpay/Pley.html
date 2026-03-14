# Pley.html
<!DOCTYPE html>
<html lang="uk">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ІГРИ</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Bebas+Neue&display=swap');

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg: #0a0a0a;
    --fg: #f0f0f0;
    --accent: #ffffff;
    --dim: #444;
    --mid: #888;
    --border: #2a2a2a;
  }

  html, body {
    background: var(--bg);
    color: var(--fg);
    font-family: 'Share Tech Mono', monospace;
    min-height: 100vh;
    overflow-x: hidden;
  }

  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background: repeating-linear-gradient(
      0deg, transparent, transparent 2px,
      rgba(255,255,255,0.013) 2px, rgba(255,255,255,0.013) 4px
    );
    pointer-events: none;
    z-index: 9999;
  }

  /* ══ SCREENS ══ */
  .screen { display: none; }
  .screen.active { display: block; animation: fadeIn 0.32s ease forwards; }
  #screen-menu.active { display: flex; }

  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(14px); }
    to   { opacity: 1; transform: translateY(0); }
  }

  /* ══ MENU ══ */
  #screen-menu {
    min-height: 100vh;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 40px 20px;
  }

  .menu-logo {
    font-family: 'Bebas Neue', sans-serif;
    font-size: clamp(72px, 18vw, 140px);
    letter-spacing: 0.12em;
    line-height: 1;
    color: var(--accent);
    text-align: center;
    margin-bottom: 6px;
  }

  .menu-sub {
    font-size: 10px;
    letter-spacing: 0.45em;
    color: var(--mid);
    text-transform: uppercase;
    margin-bottom: 64px;
  }

  .menu-cards {
    display: flex;
    gap: 20px;
    flex-wrap: wrap;
    justify-content: center;
  }

  .menu-card {
    width: 220px;
    border: 1px solid var(--border);
    padding: 32px 26px 26px;
    cursor: pointer;
    background: transparent;
    color: var(--fg);
    text-align: left;
    position: relative;
    overflow: hidden;
    transition: border-color 0.2s;
  }

  .menu-card::before {
    content: '';
    position: absolute;
    inset: 0;
    background: var(--fg);
    transform: scaleX(0);
    transform-origin: left;
    transition: transform 0.22s ease;
    z-index: 0;
  }
  .menu-card:hover { border-color: var(--fg); }
  .menu-card:hover::before { transform: scaleX(1); }
  .menu-card:hover .c-num,
  .menu-card:hover .c-title,
  .menu-card:hover .c-desc,
  .menu-card:hover .c-arrow { color: var(--bg); }

  .c-num {
    font-size: 10px; letter-spacing: 0.4em; color: var(--dim);
    margin-bottom: 10px; position: relative; z-index: 1; transition: color 0.2s;
    text-transform: uppercase;
  }
  .c-title {
    font-family: 'Bebas Neue', sans-serif; font-size: 36px;
    letter-spacing: 0.06em; line-height: 1.1; margin-bottom: 14px;
    position: relative; z-index: 1; transition: color 0.2s;
  }
  .c-desc {
    font-size: 10px; color: var(--mid); letter-spacing: 0.06em;
    line-height: 1.8; position: relative; z-index: 1; transition: color 0.2s;
  }
  .c-arrow {
    position: absolute; bottom: 16px; right: 16px; font-size: 18px;
    color: var(--dim); z-index: 1; transition: color 0.2s, transform 0.2s;
  }
  .menu-card:hover .c-arrow { transform: translateX(5px); }

  /* ══ GAME SHELL ══ */
  .game-screen {
    max-width: 540px;
    margin: 0 auto;
    padding: 44px 28px 60px;
  }

  .topbar {
    display: flex;
    align-items: center;
    gap: 18px;
    margin-bottom: 36px;
    padding-bottom: 18px;
    border-bottom: 1px solid var(--border);
  }

  .back-btn {
    background: transparent;
    border: 1px solid var(--border);
    color: var(--mid);
    font-family: 'Share Tech Mono', monospace;
    font-size: 11px;
    letter-spacing: 0.2em;
    padding: 7px 14px;
    cursor: pointer;
    text-transform: uppercase;
    transition: all 0.15s;
    flex-shrink: 0;
  }
  .back-btn:hover { border-color: var(--fg); color: var(--fg); }

  .game-title-wrap .g-label {
    font-size: 10px; letter-spacing: 0.35em; color: var(--dim);
    text-transform: uppercase; margin-bottom: 2px;
  }
  .game-title-wrap .g-title {
    font-family: 'Bebas Neue', sans-serif;
    font-size: clamp(28px, 6vw, 42px);
    letter-spacing: 0.08em; color: var(--accent); line-height: 1;
  }

  /* ══ GUESS ══ */
  #guess-status {
    font-size: 13px; color: var(--mid);
    min-height: 20px; margin-bottom: 24px; letter-spacing: 0.05em;
  }

  .range-display {
    display: flex; justify-content: space-between;
    font-size: 11px; color: var(--dim); margin-bottom: 4px; letter-spacing: 0.05em;
  }
  .range-bar {
    width: 100%; height: 2px; background: var(--border);
    margin-bottom: 28px; position: relative;
  }
  .range-fill {
    position: absolute; height: 100%; background: var(--fg);
    transition: left 0.3s, width 0.3s;
  }

  .guess-input-row { display: flex; gap: 10px; margin-bottom: 16px; }

  input[type="number"] {
    flex: 1; background: transparent; border: 1px solid var(--dim);
    color: var(--fg); font-family: 'Share Tech Mono', monospace;
    font-size: 20px; padding: 10px 14px; outline: none;
    transition: border-color 0.2s; -moz-appearance: textfield;
  }
  input[type="number"]::-webkit-outer-spin-button,
  input[type="number"]::-webkit-inner-spin-button { -webkit-appearance: none; }
  input[type="number"]:focus { border-color: var(--fg); }

  .btn {
    background: transparent; border: 1px solid var(--dim); color: var(--fg);
    font-family: 'Share Tech Mono', monospace; font-size: 12px;
    letter-spacing: 0.2em; text-transform: uppercase;
    padding: 10px 20px; cursor: pointer;
    transition: background 0.15s, border-color 0.15s, color 0.15s;
  }
  .btn:hover { background: var(--fg); color: var(--bg); border-color: var(--fg); }
  .btn-full { width: 100%; }

  .attempts-counter { font-size: 11px; color: var(--mid); letter-spacing: 0.1em; }

  .hint {
    font-family: 'Bebas Neue', sans-serif; font-size: 32px;
    letter-spacing: 0.1em; margin: 12px 0; min-height: 40px;
  }
  .hint.higher { color: #ccc; }
  .hint.lower  { color: #aaa; }
  .hint.win    { color: #fff; }

  .history-row {
    display: flex; flex-wrap: wrap; gap: 6px;
    margin-top: 18px; max-height: 72px; overflow-y: auto;
  }
  .history-chip {
    font-size: 10px; padding: 2px 7px;
    border: 1px solid var(--border); color: var(--mid); letter-spacing: 0.05em;
  }
  .history-chip.too-low  { border-color: #333; color: #666; }
  .history-chip.too-high { border-color: #555; color: #999; }

  /* ══ SNAKE ══ */
  .snake-scorebar {
    display: flex; justify-content: space-between; margin-bottom: 14px;
  }
  .score-block .s-label {
    font-size: 10px; color: var(--dim); letter-spacing: 0.25em;
    text-transform: uppercase; margin-bottom: 2px;
  }
  .score-block .s-val {
    font-family: 'Bebas Neue', sans-serif; font-size: 28px;
    letter-spacing: 0.1em; color: var(--accent);
  }

  .snake-overlay { position: relative; margin-bottom: 12px; }

  #snake-canvas {
    display: block; border: 1px solid var(--border);
    width: 100%; image-rendering: pixelated;
  }

  #snake-msg {
    position: absolute; inset: 0; display: flex; flex-direction: column;
    align-items: center; justify-content: center;
    background: rgba(10,10,10,0.88);
    font-family: 'Bebas Neue', sans-serif; font-size: 22px;
    letter-spacing: 0.15em; text-align: center; gap: 8px;
  }
  #snake-msg.hidden { display: none; }
  #snake-msg small {
    font-family: 'Share Tech Mono', monospace;
    font-size: 11px; color: var(--mid); letter-spacing: 0.2em;
  }

  /* D-PAD */
  .dpad {
    display: grid;
    grid-template-columns: repeat(3, 50px);
    grid-template-rows: repeat(3, 50px);
    gap: 5px;
    margin: 12px auto;
    width: fit-content;
  }
  .dpad-btn {
    background: transparent; border: 1px solid var(--dim); color: var(--fg);
    font-size: 17px; cursor: pointer; display: flex;
    align-items: center; justify-content: center;
    transition: background 0.1s, color 0.1s;
    user-select: none; -webkit-tap-highlight-color: transparent; border-radius: 2px;
  }
  .dpad-btn:active, .dpad-btn.pressed { background: var(--fg); color: var(--bg); border-color: var(--fg); }
  .dpad-center { border-color: var(--border) !important; cursor: default; pointer-events: none; color: var(--border); }

  .snake-controls-row {
    display: flex; justify-content: space-between; align-items: center; margin-top: 6px;
  }
  .speed-selector { display: flex; gap: 5px; align-items: center; }
  .speed-selector span {
    font-size: 10px; color: var(--dim); letter-spacing: 0.2em;
    text-transform: uppercase; margin-right: 4px;
  }
  .speed-btn {
    background: transparent; border: 1px solid var(--border); color: var(--dim);
    font-family: 'Share Tech Mono', monospace; font-size: 10px;
    letter-spacing: 0.15em; padding: 4px 10px; cursor: pointer;
    transition: all 0.15s; text-transform: uppercase;
  }
  .speed-btn.active, .speed-btn:hover { background: var(--fg); color: var(--bg); border-color: var(--fg); }

  /* misc */
  ::-webkit-scrollbar { width: 4px; }
  ::-webkit-scrollbar-track { background: var(--bg); }
  ::-webkit-scrollbar-thumb { background: var(--border); }

  @keyframes flicker {
    0%,100%{opacity:1} 50%{opacity:0.5}
  }
  .flicker { animation: flicker 0.4s steps(1) 3; }
</style>
</head>
<body>

<!-- ══════════ MENU ══════════ -->
<div id="screen-menu" class="screen active">
  <div class="menu-logo">ІГРИ</div>
  <div class="menu-sub">Оберіть гру</div>
  <div class="menu-cards">

    <button class="menu-card" onclick="showScreen('guess')">
      <div class="c-num">Гра 01</div>
      <div class="c-title">Вгадай<br>число</div>
      <div class="c-desc">Число від 1 до 1000<br>Підказки більше / менше<br>Лічильник спроб</div>
      <div class="c-arrow">→</div>
    </button>

    <button class="menu-card" onclick="showScreen('snake')">
      <div class="c-num">Гра 02</div>
      <div class="c-title">Змійка</div>
      <div class="c-desc">Керування стрілками<br>D-pad для мобільних<br>Три рівні швидкості</div>
      <div class="c-arrow">→</div>
    </button>

  </div>
</div>

<!-- ══════════ GUESS ══════════ -->
<div id="screen-guess" class="screen">
  <div class="game-screen">
    <div class="topbar">
      <button class="back-btn" onclick="showScreen('menu')">← Меню</button>
      <div class="game-title-wrap">
        <div class="g-label">Гра 01</div>
        <div class="g-title">Вгадай число</div>
      </div>
    </div>

    <div id="guess-status">Я загадав число від 1 до 1000.</div>
    <div class="range-display"><span id="range-lo">1</span><span id="range-hi">1000</span></div>
    <div class="range-bar"><div class="range-fill" id="range-fill" style="left:0%;width:100%"></div></div>

    <div class="guess-input-row">
      <input type="number" id="guess-input" min="1" max="1000" placeholder="???">
      <button class="btn" id="guess-btn">Вгадати</button>
    </div>

    <div class="hint" id="guess-hint"></div>
    <div class="attempts-counter">Спроб: <span id="guess-attempts">0</span></div>
    <div class="history-row" id="guess-history"></div>

    <div style="margin-top:24px">
      <button class="btn btn-full" id="guess-new-btn">↺ Нова гра</button>
    </div>
  </div>
</div>

<!-- ══════════ SNAKE ══════════ -->
<div id="screen-snake" class="screen">
  <div class="game-screen">
    <div class="topbar">
      <button class="back-btn" onclick="snakePause(); showScreen('menu')">← Меню</button>
      <div class="game-title-wrap">
        <div class="g-label">Гра 02</div>
        <div class="g-title">Змійка</div>
      </div>
    </div>

    <div class="snake-scorebar">
      <div class="score-block">
        <div class="s-label">Рахунок</div>
        <div class="s-val" id="snake-score">000</div>
      </div>
      <div class="score-block" style="text-align:right">
        <div class="s-label">Рекорд</div>
        <div class="s-val" id="snake-best">000</div>
      </div>
    </div>

    <div class="snake-overlay">
      <canvas id="snake-canvas" width="400" height="400"></canvas>
      <div id="snake-msg">
        <div>НАТИСНИ СТАРТ</div>
        <small>СТРІЛКИ АБО D-PAD</small>
      </div>
    </div>

    <div class="dpad">
      <div></div>
      <button class="dpad-btn" data-dir="up">▲</button>
      <div></div>
      <button class="dpad-btn" data-dir="left">◀</button>
      <div class="dpad-btn dpad-center">✛</div>
      <button class="dpad-btn" data-dir="right">▶</button>
      <div></div>
      <button class="dpad-btn" data-dir="down">▼</button>
      <div></div>
    </div>

    <div class="snake-controls-row">
      <button class="btn" id="snake-start-btn">▶ Старт</button>
      <div class="speed-selector">
        <span>Швидкість</span>
        <button class="speed-btn active" data-speed="150">×1</button>
        <button class="speed-btn" data-speed="90">×2</button>
        <button class="speed-btn" data-speed="50">×3</button>
      </div>
    </div>
  </div>
</div>

<script>
/* ═══ ROUTER ═══ */
function showScreen(name) {
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
  const el = document.getElementById('screen-' + name);
  if (el) el.classList.add('active');
}

/* ═══════════════════════════════
   GUESS THE NUMBER
═══════════════════════════════ */
(function() {
  let secret, attempts, lo, hi, gameOver;

  const statusEl   = document.getElementById('guess-status');
  const inputEl    = document.getElementById('guess-input');
  const btnGuess   = document.getElementById('guess-btn');
  const btnNew     = document.getElementById('guess-new-btn');
  const hintEl     = document.getElementById('guess-hint');
  const attemptsEl = document.getElementById('guess-attempts');
  const historyEl  = document.getElementById('guess-history');
  const rangeLo    = document.getElementById('range-lo');
  const rangeHi    = document.getElementById('range-hi');
  const rangeFill  = document.getElementById('range-fill');

  function newGame() {
    secret = Math.floor(Math.random()*1000)+1;
    attempts = 0; lo = 1; hi = 1000; gameOver = false;
    statusEl.textContent   = 'Я загадав число від 1 до 1000.';
    hintEl.textContent     = ''; hintEl.className = 'hint';
    attemptsEl.textContent = '0';
    historyEl.innerHTML    = '';
    inputEl.value          = ''; inputEl.disabled = false; btnGuess.disabled = false;
    updateBar(1,1000); inputEl.focus();
  }

  function updateBar(l,h) {
    rangeLo.textContent = l; rangeHi.textContent = h;
    rangeFill.style.left  = ((l-1)/999*100)+'%';
    rangeFill.style.width = ((h-l)/999*100)+'%';
  }

  function guess() {
    if (gameOver) return;
    const val = parseInt(inputEl.value);
    if (isNaN(val)||val<1||val>1000) { statusEl.textContent='Введи число від 1 до 1000.'; return; }
    attempts++; attemptsEl.textContent = attempts;
    const chip = document.createElement('div');
    chip.className = 'history-chip'; chip.textContent = val;

    if (val===secret) {
      hintEl.textContent='ВГАДАВ!'; hintEl.className='hint win flicker';
      statusEl.textContent=`Число ${secret}. Вгадано за ${attempts} ${plural(attempts)}.`;
      chip.style.borderColor='#fff'; chip.style.color='#fff';
      gameOver=true; inputEl.disabled=true; btnGuess.disabled=true;
    } else if (val<secret) {
      hintEl.textContent='↑ БІЛЬШЕ'; hintEl.className='hint higher';
      statusEl.textContent=`${val} — замало.`;
      lo=Math.max(lo,val+1); chip.className+=' too-low'; updateBar(lo,hi);
    } else {
      hintEl.textContent='↓ МЕНШЕ'; hintEl.className='hint lower';
      statusEl.textContent=`${val} — забагато.`;
      hi=Math.min(hi,val-1); chip.className+=' too-high'; updateBar(lo,hi);
    }
    historyEl.appendChild(chip); historyEl.scrollLeft=historyEl.scrollWidth;
    inputEl.value=''; inputEl.focus();
  }

  function plural(n) { return n===1?'спробу':n<5?'спроби':'спроб'; }

  btnGuess.addEventListener('click', guess);
  inputEl.addEventListener('keydown', e => { if(e.key==='Enter') guess(); });
  btnNew.addEventListener('click', newGame);
  newGame();
})();


/* ═══════════════════════════════
   SNAKE
═══════════════════════════════ */
(function() {
  const canvas   = document.getElementById('snake-canvas');
  const ctx      = canvas.getContext('2d');
  const scoreEl  = document.getElementById('snake-score');
  const bestEl   = document.getElementById('snake-best');
  const msgEl    = document.getElementById('snake-msg');
  const startBtn = document.getElementById('snake-start-btn');
  const speedBtns = document.querySelectorAll('.speed-btn');

  const COLS=20, ROWS=20, CELL=canvas.width/COLS;
  let snake, dir, nextDir, food, score, best=0, loop, speed=150, running=false;

  const fmt = n => String(n).padStart(3,'0');

  function initSnake() {
    snake=[{x:10,y:10},{x:9,y:10},{x:8,y:10}];
    dir={x:1,y:0}; nextDir={x:1,y:0};
    score=0; scoreEl.textContent=fmt(0); placeFood();
  }

  function placeFood() {
    let p;
    do { p={x:Math.floor(Math.random()*COLS), y:Math.floor(Math.random()*ROWS)}; }
    while (snake.some(s=>s.x===p.x&&s.y===p.y));
    food=p;
  }

  function draw() {
    ctx.fillStyle='#0a0a0a'; ctx.fillRect(0,0,canvas.width,canvas.height);
    ctx.strokeStyle='#141414'; ctx.lineWidth=0.5;
    for(let c=0;c<COLS;c++) for(let r=0;r<ROWS;r++) ctx.strokeRect(c*CELL,r*CELL,CELL,CELL);

    const blink=Math.floor(Date.now()/400)%2===0;
    ctx.strokeStyle=blink?'#fff':'#888'; ctx.lineWidth=2;
    ctx.strokeRect(food.x*CELL+3,food.y*CELL+3,CELL-6,CELL-6);
    ctx.strokeStyle=blink?'#888':'#555'; ctx.lineWidth=1;
    ctx.strokeRect(food.x*CELL+5,food.y*CELL+5,CELL-10,CELL-10);

    snake.forEach((seg,i)=>{
      const shade=Math.round(255*(1-(i/snake.length)*0.5));
      ctx.fillStyle=`rgb(${shade},${shade},${shade})`;
      const pad=i===0?0:2;
      ctx.fillRect(seg.x*CELL+pad,seg.y*CELL+pad,CELL-pad*2,CELL-pad*2);
    });

    const h=snake[0]; ctx.fillStyle='#000'; const es=2;
    if      (dir.x===1)  {ctx.fillRect(h.x*CELL+CELL-5,h.y*CELL+4,es,es);        ctx.fillRect(h.x*CELL+CELL-5,h.y*CELL+CELL-6,es,es);}
    else if (dir.x===-1) {ctx.fillRect(h.x*CELL+3,h.y*CELL+4,es,es);             ctx.fillRect(h.x*CELL+3,h.y*CELL+CELL-6,es,es);}
    else if (dir.y===1)  {ctx.fillRect(h.x*CELL+4,h.y*CELL+CELL-5,es,es);        ctx.fillRect(h.x*CELL+CELL-6,h.y*CELL+CELL-5,es,es);}
    else                 {ctx.fillRect(h.x*CELL+4,h.y*CELL+3,es,es);             ctx.fillRect(h.x*CELL+CELL-6,h.y*CELL+3,es,es);}
  }

  function step() {
    dir=nextDir;
    const head={x:snake[0].x+dir.x, y:snake[0].y+dir.y};
    if(head.x<0||head.x>=COLS||head.y<0||head.y>=ROWS) return endGame();
    if(snake.some(s=>s.x===head.x&&s.y===head.y)) return endGame();
    snake.unshift(head);
    if(head.x===food.x&&head.y===food.y){
      score++; scoreEl.textContent=fmt(score);
      if(score>best){best=score; bestEl.textContent=fmt(best);}
      placeFood();
    } else { snake.pop(); }
    draw();
  }

  function endGame() {
    clearInterval(loop); running=false;
    ctx.fillStyle='rgba(255,255,255,0.07)'; ctx.fillRect(0,0,canvas.width,canvas.height);
    msgEl.innerHTML=`<div>GAME OVER</div><div style="font-size:16px;color:#888">Рахунок: ${fmt(score)}</div><small>НАТИСНИ СТАРТ ЩОБ ЗНОВУ</small>`;
    msgEl.classList.remove('hidden'); startBtn.textContent='▶ Старт';
  }

  function startGame() {
    clearInterval(loop); msgEl.classList.add('hidden');
    initSnake(); draw(); loop=setInterval(step,speed);
    running=true; startBtn.textContent='■ Стоп';
  }

  window.snakePause = function() {
    if(!running) return;
    clearInterval(loop); running=false; startBtn.textContent='▶ Старт';
    msgEl.innerHTML='<div>ПАУЗА</div><small>НАТИСНИ СТАРТ ЩОБ ПРОДОВЖИТИ</small>';
    msgEl.classList.remove('hidden');
  };

  startBtn.addEventListener('click', ()=>{ running?snakePause():startGame(); });

  speedBtns.forEach(btn=>{
    btn.addEventListener('click',()=>{
      speedBtns.forEach(b=>b.classList.remove('active')); btn.classList.add('active');
      speed=parseInt(btn.dataset.speed);
      if(running){clearInterval(loop); loop=setInterval(step,speed);}
    });
  });

  document.addEventListener('keydown', e=>{
    const map={ArrowUp:{x:0,y:-1},ArrowDown:{x:0,y:1},ArrowLeft:{x:-1,y:0},ArrowRight:{x:1,y:0}};
    if(!map[e.key]) return; e.preventDefault();
    const d=map[e.key];
    if(d.x===-dir.x&&d.y===-dir.y) return;
    nextDir=d; if(!running) startGame();
  });

  let tx,ty;
  canvas.addEventListener('touchstart',e=>{tx=e.touches[0].clientX;ty=e.touches[0].clientY;},{passive:true});
  canvas.addEventListener('touchend',e=>{
    const dx=e.changedTouches[0].clientX-tx, dy=e.changedTouches[0].clientY-ty;
    let d=Math.abs(dx)>Math.abs(dy)?(dx>0?{x:1,y:0}:{x:-1,y:0}):(dy>0?{x:0,y:1}:{x:0,y:-1});
    if(d.x===-dir.x&&d.y===-dir.y) return;
    nextDir=d; if(!running) startGame();
  },{passive:true});

  const dirMap={up:{x:0,y:-1},down:{x:0,y:1},left:{x:-1,y:0},right:{x:1,y:0}};
  document.querySelectorAll('.dpad-btn[data-dir]').forEach(btn=>{
    function press(){
      const d=dirMap[btn.dataset.dir];
      if(!d||(d.x===-dir.x&&d.y===-dir.y)) return;
      nextDir=d; btn.classList.add('pressed'); if(!running) startGame();
    }
    function release(){ btn.classList.remove('pressed'); }
    btn.addEventListener('mousedown',press);
    btn.addEventListener('touchstart',e=>{e.preventDefault();press();},{passive:false});
    btn.addEventListener('mouseup',release);
    btn.addEventListener('mouseleave',release);
    btn.addEventListener('touchend',release);
  });

  initSnake(); draw();
  setInterval(()=>{ if(!running) draw(); },400);
})();
</script>
</body>
</html>
