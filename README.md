<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Interaction — experiments</title>
<meta name="description" content="A running collection of small interaction and type experiments.">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display&family=Bebas+Neue&family=Pacifico&family=Press+Start+2P&family=Abril+Fatface&family=Righteous&family=Permanent+Marker&family=Lobster&family=Oswald:wght@700&family=Anton&family=Caveat:wght@700&family=Shadows+Into+Light&family=Special+Elite&family=Monoton&family=Bungee&family=Zilla+Slab:wght@700&family=Cormorant+Garamond:wght@600&family=Archivo+Black&family=Rubik+Mono+One&family=VT323&family=Orbitron:wght@700&family=Sacramento&family=Amatic+SC:wght@700&family=Fredoka:wght@600&family=Kalam:wght@700&family=Indie+Flower&family=Josefin+Sans:wght@600&family=Raleway:wght@800&family=Merriweather:wght@900&family=Rock+Salt&display=swap" rel="stylesheet">
<style>
  * { box-sizing: border-box; }

  html, body {
    margin: 0;
    padding: 0;
    width: 100%;
    height: 100%;
    background: #ffffff;
    color: #000000;
    overflow: hidden;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
  }

  /* the page itself doesn't scroll — #app does */
  #app {
    height: 100%;
    overflow-y: auto;
    overflow-x: hidden;
    -webkit-overflow-scrolling: touch;
  }

  .section {
    position: relative;
    width: 100%;
    height: 100vh;
    overflow: hidden;
  }

  /* chrome shared by every section */
  .label {
    position: absolute;
    bottom: 28px;
    left: 28px;
    font-size: 12px;
    color: #adadad;
  }

  .hint {
    position: absolute;
    bottom: 28px;
    right: 28px;
    font-size: 12px;
    color: #adadad;
  }

  @media (max-width: 480px) {
    .hint { display: none; }
  }

  .scroll-cue {
    position: absolute;
    bottom: 30px;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 8px;
    font-size: 12px;
    color: #adadad;
  }
  .scroll-cue .arrow {
    width: 1px;
    height: 22px;
    background: #cfcfcf;
    animation: scrollcue 1.6s ease-in-out infinite;
    transform-origin: top center;
  }
  @keyframes scrollcue {
    0%, 100% { transform: scaleY(1); opacity: 0.55; }
    50%      { transform: scaleY(0.5); opacity: 1; }
  }
  @media (prefers-reduced-motion: reduce) {
    .scroll-cue .arrow { animation: none; }
  }

  /* ---------- section 1: font instability ---------- */
  #section-font {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  #section-font .word {
    display: flex;
    align-items: baseline;
    justify-content: center;
    flex-wrap: nowrap;
    color: #000000;
    font-size: clamp(2rem, 11vw, 8.5rem);
    line-height: 1;
    padding: 0 5vw;
    text-align: center;
  }

  #section-font .letter {
    display: inline-block;
    transform-origin: 50% 100%;
    cursor: pointer;
    user-select: none;
    transition: opacity 0.15s ease;
  }
  #section-font .letter:hover { opacity: 0.7; }
  #section-font .letter.pop { animation: pop 0.32s cubic-bezier(0.34, 1.56, 0.64, 1); }

  @keyframes pop {
    0%   { transform: scale(0.8); opacity: 0.5; }
    60%  { transform: scale(1.08); opacity: 1; }
    100% { transform: scale(1); }
  }

  @media (prefers-reduced-motion: reduce) {
    #section-font .letter.pop { animation: none; }
  }

  /* ---------- section 2: 3d cube(s) ---------- */
  #section-cube {
    user-select: none;
    -webkit-user-select: none;
  }

  .cube-field {
    position: absolute;
    inset: 0;
  }

  .cube-instance {
    --size: clamp(140px, 32vmin, 220px);
    position: absolute;
    transform: translate(-50%, -50%);
    perspective: 900px;
    cursor: grab;
    touch-action: none;
  }
  .cube-instance.dragging { cursor: grabbing; }
  .cube-instance.moving { cursor: move; }

  .cube-instance.selected .face::after {
    content: '';
    position: absolute;
    inset: 0;
    background: rgba(0, 122, 255, 0.35);
    pointer-events: none;
  }

  .cube-instance .cube {
    position: relative;
    width: var(--size);
    height: var(--size);
    transform-style: preserve-3d;
    will-change: transform;
    -webkit-user-drag: none;
  }

  .cube-instance .face {
    position: absolute;
    width: var(--size);
    height: var(--size);
    border: 1.5px solid #000000;
    box-sizing: border-box;
    backface-visibility: hidden;
    background-size: cover;
    background-position: center;
  }

  .cube-instance .face.top    { background-color: #ffffff; transform: rotateX(90deg)  translateZ(calc(var(--size) / 2)); }
  .cube-instance .face.front  { background-color: #f7f7f7; transform: translateZ(calc(var(--size) / 2)); }
  .cube-instance .face.right  { background-color: #f0f0f0; transform: rotateY(90deg)  translateZ(calc(var(--size) / 2)); }
  .cube-instance .face.left   { background-color: #f0f0f0; transform: rotateY(-90deg) translateZ(calc(var(--size) / 2)); }
  .cube-instance .face.back   { background-color: #e6e6e6; transform: rotateY(180deg) translateZ(calc(var(--size) / 2)); }
  .cube-instance .face.bottom { background-color: #d6d6d6; transform: rotateX(-90deg) translateZ(calc(var(--size) / 2)); }

  .add-cube {
    position: absolute;
    bottom: 24px;
    left: 50%;
    transform: translateX(-50%);
    width: 40px;
    height: 40px;
    padding: 0;
    border-radius: 50%;
    border: 1.5px solid #000000;
    background: #ffffff;
    color: #000000;
    font-size: 20px;
    font-family: inherit;
    line-height: 1;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    transition: background-color 0.15s ease, color 0.15s ease, transform 0.1s ease;
  }
  .add-cube:hover:not(:disabled) { background: #000000; color: #ffffff; }
  .add-cube:active:not(:disabled) { transform: translateX(-50%) scale(0.9); }
  .add-cube:disabled { opacity: 0.3; cursor: not-allowed; }

  .cube-panel {
    position: absolute;
    top: 0;
    right: 0;
    height: 100%;
    width: 220px;
    max-width: 72vw;
    box-sizing: border-box;
    background: #ffffff;
    border-left: 1px solid #e4e4e4;
    display: flex;
    align-items: center;
    padding: 0 22px;
    transform: translateX(100%);
    transition: transform 0.22s ease;
  }
  .cube-panel.open { transform: translateX(0); }

  .cube-panel-inner {
    width: 100%;
    display: flex;
    flex-direction: column;
    gap: 16px;
  }

  .panel-field {
    display: flex;
    flex-direction: column;
    gap: 8px;
  }
  .panel-label {
    display: flex;
    justify-content: space-between;
    font-size: 12px;
    color: #8a8a8a;
  }
  .panel-field input[type="range"] {
    width: 100%;
    accent-color: #000000;
  }

  .panel-btn {
    width: 100%;
    padding: 10px 14px;
    border: 1.5px solid #000000;
    background: #ffffff;
    color: #000000;
    font-family: inherit;
    font-size: 13px;
    cursor: pointer;
    transition: background-color 0.15s ease, color 0.15s ease;
  }
  .panel-btn:hover { background: #000000; color: #ffffff; }
  .panel-btn.delete { border-color: #b3b3b3; color: #8a8a8a; }
  .panel-btn.delete:hover { background: #8a8a8a; color: #ffffff; border-color: #8a8a8a; }

  /* ---------- canvas-based sections ---------- */
  .section canvas {
    display: block;
    width: 100%;
    height: 100%;
  }

  /* ---------- section 5: eye ---------- */
  #section-eye {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .eye {
    position: relative;
    width: min(48vw, 48vh, 440px);
    height: min(48vw, 48vh, 440px);
    border-radius: 50%;
    background: radial-gradient(circle at 34% 30%, #ffffff, #f1f1f1 45%, #dcdcdc 72%, #b8b8b8 100%);
    box-shadow: inset 0 0 3px rgba(0,0,0,0.2), inset 0 0 50px rgba(0,0,0,0.16), 0 20px 60px rgba(0,0,0,0.10);
    overflow: hidden;
    transform-origin: center;
  }
  .eye.blink { animation: blink 0.18s ease-in-out; }
  @keyframes blink {
    0%, 100% { transform: scaleY(1); }
    50%      { transform: scaleY(0.06); }
  }
  @media (prefers-reduced-motion: reduce) {
    .eye.blink { animation: none; }
  }

  .iris {
    position: absolute;
    left: 50%;
    top: 50%;
    width: 44%;
    height: 44%;
    border-radius: 50%;
    background-color: #3a3a3a;
    background-image: url('https://d8j0ntlcm91z4.cloudfront.net/user_3IPmyp1DwBDR70XB5hask3d7SVA/hf_20260913_220751_227bd771-5119-41f7-b88d-1b18f1be9c23.png');
    background-size: cover;
    background-position: center;
    box-shadow: 0 0 0 2px rgba(0,0,0,0.35) inset, 0 0 14px rgba(0,0,0,0.3);
    transform: translate(-50%, -50%);
  }

  .pupil {
    position: absolute;
    left: 50%;
    top: 50%;
    width: 34%;
    height: 34%;
    border-radius: 50%;
    background: #000000;
    transform: translate(-50%, -50%);
  }

  .highlight {
    position: absolute;
    left: 22%;
    top: 18%;
    width: 26%;
    height: 26%;
    border-radius: 50%;
    background: rgba(255, 255, 255, 0.9);
    filter: blur(2px);
  }
</style>
</head>
<body>
<div id="app">

  <section class="section" id="section-font">
    <div class="word" id="word" aria-label="INTERACTION"></div>
    <span class="label">Experiment 01: font instability</span>
    <span class="hint">Click a letter</span>
    <div class="scroll-cue" aria-hidden="true">
      <span>Scroll</span>
      <span class="arrow"></span>
    </div>
  </section>

  <section class="section" id="section-cube">
    <div class="cube-field" id="cubeField"></div>
    <button class="add-cube" type="button" id="addCubeBtn" aria-label="Add a cube">+</button>
    <aside class="cube-panel" id="cubePanel">
      <div class="cube-panel-inner">
        <div class="panel-field">
          <label class="panel-label" for="cubeSizeInput"><span>Size</span><span id="cubeSizeValue">160</span></label>
          <input type="range" id="cubeSizeInput" min="60" max="640" step="4">
        </div>
        <button class="panel-btn" type="button" id="cubeUploadBtn">Upload image</button>
        <input type="file" id="cubeImageInput" accept="image/*" hidden>
        <button class="panel-btn delete" type="button" id="cubeDeleteBtn">Delete cube</button>
      </div>
    </aside>
    <span class="label">Experiment 02: 3d cube</span>
    <span class="hint">Click to select, drag to rotate, middle-click to move</span>
  </section>

  <section class="section" id="section-flock">
    <canvas id="flockCanvas"></canvas>
    <span class="label">Experiment 03: flocking</span>
    <span class="hint">Move your cursor to scatter them</span>
  </section>

  <section class="section" id="section-magnet">
    <canvas id="magnetCanvas"></canvas>
    <span class="label">Experiment 04: magnetic field</span>
    <span class="hint">Move your cursor across the grid</span>
  </section>

  <section class="section" id="section-eye">
    <div class="eye" id="eye">
      <div class="iris" id="iris">
        <div class="pupil"></div>
        <div class="highlight"></div>
      </div>
    </div>
    <span class="label">Experiment 05: eye</span>
    <span class="hint">It watches your cursor</span>
  </section>

</div>

<script>
  /* =========================================================
     Experiment 01 — font instability
  ========================================================= */
  const WORD = "INTERACTION";

  const FONTS = [
    "Playfair Display", "Bebas Neue", "Pacifico", "Press Start 2P",
    "Abril Fatface", "Righteous", "Permanent Marker", "Lobster",
    "Oswald", "Anton", "Caveat", "Shadows Into Light",
    "Special Elite", "Monoton", "Bungee", "Zilla Slab",
    "Cormorant Garamond", "Archivo Black", "Rubik Mono One", "VT323",
    "Orbitron", "Sacramento", "Amatic SC", "Fredoka",
    "Kalam", "Indie Flower", "Josefin Sans", "Raleway",
    "Merriweather", "Rock Salt"
  ];

  const fontReduceMotion = window.matchMedia("(prefers-reduced-motion: reduce)").matches;
  const FONT_MIN_DELAY = fontReduceMotion ? 1500 : 500;
  const FONT_MAX_DELAY = fontReduceMotion ? 4000 : 3000;

  function randomFont(exclude) {
    let next;
    do {
      next = FONTS[Math.floor(Math.random() * FONTS.length)];
    } while (next === exclude && FONTS.length > 1);
    return next;
  }

  function randomFontDelay() {
    return FONT_MIN_DELAY + Math.random() * (FONT_MAX_DELAY - FONT_MIN_DELAY);
  }

  function applyNewFont(state) {
    const newFont = randomFont(state.currentFont);
    state.currentFont = newFont;
    state.el.style.fontFamily = `'${newFont}', sans-serif`;
    state.el.classList.remove('pop');
    void state.el.offsetWidth; // reflow to restart the animation
    state.el.classList.add('pop');
    scheduleNextFont(state, false);
  }

  function scheduleNextFont(state, isFirst) {
    const delay = isFirst ? Math.random() * 1200 : randomFontDelay();
    state.timeoutId = setTimeout(() => applyNewFont(state), delay);
  }

  function buildFontWord() {
    const container = document.getElementById('word');
    [...WORD].forEach((ch) => {
      const span = document.createElement('span');
      span.className = 'letter';
      span.textContent = ch;
      span.style.fontFamily = "sans-serif";
      container.appendChild(span);

      const state = { el: span, currentFont: null, timeoutId: null };
      span.addEventListener('click', () => {
        clearTimeout(state.timeoutId);
        applyNewFont(state);
      });

      scheduleNextFont(state, true);
    });
  }

  if (document.fonts && document.fonts.load) {
    Promise.all(FONTS.map((f) => document.fonts.load(`1em "${f}"`).catch(() => {})))
      .then(buildFontWord, buildFontWord);
  } else {
    buildFontWord();
  }

  /* =========================================================
     Experiment 02 — 3d cube(s)
  ========================================================= */
  const cubeFieldEl = document.getElementById('cubeField');
  const addCubeBtn = document.getElementById('addCubeBtn');
  const cubePanelEl = document.getElementById('cubePanel');
  const cubeUploadBtn = document.getElementById('cubeUploadBtn');
  const cubeDeleteBtn = document.getElementById('cubeDeleteBtn');
  const cubeImageInput = document.getElementById('cubeImageInput');
  const cubeSizeInput = document.getElementById('cubeSizeInput');
  const cubeSizeValue = document.getElementById('cubeSizeValue');
  const cubeReduceMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

  const CUBE_SENSITIVITY = 0.35;
  const CUBE_FRICTION = 0.94;
  const CUBE_CLICK_THRESHOLD = 5; // px of movement still counted as a click, not a drag
  const MAX_CUBES = 20;
  const CUBE_SAFE_MARGIN = { top: 0.16, bottom: 0.24, left: 0.10, right: 0.10 };
  const CUBE_MIN_R = 50;  // closest a new cube can spawn to the cube it clusters around, in px
  const CUBE_MAX_R = 260; // furthest it can spawn from that same cube, in px

  let cubeInstances = [];
  let cubeRAF = null;
  let selectedCube = null;

  function syncSizeSlider(state) {
    const currentSize = Math.round(parseFloat(getComputedStyle(state.cubeDiv).width)) || 160;
    cubeSizeInput.value = currentSize;
    cubeSizeValue.textContent = currentSize;
  }

  function selectCube(state) {
    if (selectedCube) selectedCube.wrap.classList.remove('selected');
    selectedCube = state;
    state.wrap.classList.add('selected');
    cubePanelEl.classList.add('open');
    syncSizeSlider(state);
  }

  function deselectCube() {
    if (selectedCube) selectedCube.wrap.classList.remove('selected');
    selectedCube = null;
    cubePanelEl.classList.remove('open');
  }

  function applyCubeImage(state, url) {
    state.wrap.querySelectorAll('.face').forEach((face) => {
      face.style.backgroundImage = `url('${url}')`;
      face.style.backgroundBlendMode = 'multiply';
    });
  }

  cubeSizeInput.addEventListener('input', () => {
    if (!selectedCube) return;
    selectedCube.wrap.style.setProperty('--size', cubeSizeInput.value + 'px');
    cubeSizeValue.textContent = cubeSizeInput.value;
  });

  cubeUploadBtn.addEventListener('click', () => {
    if (selectedCube) cubeImageInput.click();
  });

  cubeImageInput.addEventListener('change', () => {
    const file = cubeImageInput.files && cubeImageInput.files[0];
    const targetCube = selectedCube;
    if (file && targetCube) {
      const reader = new FileReader();
      reader.onload = () => applyCubeImage(targetCube, reader.result);
      reader.readAsDataURL(file);
    }
    cubeImageInput.value = '';
  });

  cubeDeleteBtn.addEventListener('click', () => {
    if (!selectedCube) return;
    const idx = cubeInstances.indexOf(selectedCube);
    if (idx !== -1) cubeInstances.splice(idx, 1);
    selectedCube.wrap.remove();
    deselectCube();
    addCubeBtn.disabled = cubeInstances.length >= MAX_CUBES;
  });

  // clicking empty space (not a cube, not the panel) deselects
  document.getElementById('section-cube').addEventListener('click', (e) => {
    if (e.target === cubeFieldEl || e.target.id === 'section-cube') deselectCube();
  });

  function randomCubeSpot() {
    const rect = cubeFieldEl.getBoundingClientRect();
    const w = rect.width, h = rect.height;
    const minX = CUBE_SAFE_MARGIN.left * w;
    const maxX = (1 - CUBE_SAFE_MARGIN.right) * w;
    const minY = CUBE_SAFE_MARGIN.top * h;
    const maxY = (1 - CUBE_SAFE_MARGIN.bottom) * h;

    const anchor = cubeInstances[Math.floor(Math.random() * cubeInstances.length)];

    // scale the radius to whatever room is actually available, so a small
    // window can't force every attempt to overshoot the same edge
    const maxR = Math.max(1, Math.min(CUBE_MAX_R, (maxX - minX) / 2, (maxY - minY) / 2));
    const minR = Math.min(CUBE_MIN_R, maxR * 0.6);
    const r = minR + Math.random() * (maxR - minR);
    const angle = Math.random() * Math.PI * 2;

    const x = Math.min(maxX, Math.max(minX, anchor.cx + Math.cos(angle) * r));
    const y = Math.min(maxY, Math.max(minY, anchor.cy + Math.sin(angle) * r));

    return { xPct: x / w, yPct: y / h, x, y };
  }

  function createCubeInstance(isHero) {
    const wrap = document.createElement('div');
    wrap.className = 'cube-instance' + (isHero ? ' hero' : '');

    let xPct, yPct, x, y;
    if (isHero) {
      const rect = cubeFieldEl.getBoundingClientRect();
      xPct = 0.5; yPct = 0.5; x = rect.width * 0.5; y = rect.height * 0.5;
    } else {
      const spot = randomCubeSpot();
      xPct = spot.xPct; yPct = spot.yPct; x = spot.x; y = spot.y;
    }
    wrap.style.left = (xPct * 100) + '%';
    wrap.style.top = (yPct * 100) + '%';

    const cubeDiv = document.createElement('div');
    cubeDiv.className = 'cube';
    ['front', 'back', 'right', 'left', 'top', 'bottom'].forEach((f) => {
      const face = document.createElement('div');
      face.className = 'face ' + f;
      cubeDiv.appendChild(face);
    });
    wrap.appendChild(cubeDiv);
    cubeFieldEl.appendChild(wrap);

    const state = {
      wrap, cubeDiv, cx: x, cy: y,
      rotX: isHero ? -18 : -18 + (Math.random() * 40 - 20),
      rotY: isHero ? -28 : -28 + (Math.random() * 60 - 30),
      velX: 0, velY: 0,
      dragging: false, moving: false, pointerId: null,
      lastX: 0, lastY: 0, lastT: 0,
      downX: 0, downY: 0, moved: false,
      idleSpeed: cubeReduceMotion ? 0 : (0.05 + Math.random() * 0.08) * (Math.random() < 0.5 ? -1 : 1),
    };

    wrap.addEventListener('pointerdown', (e) => {
      const isMiddleButton = e.pointerType === 'mouse' && e.button === 1;
      const isLeftButton = e.pointerType !== 'mouse' || e.button === 0;
      if (!isMiddleButton && !isLeftButton) return; // ignore right-click etc.

      e.preventDefault(); // stops text-selection / native drag / middle-click autoscroll
      state.pointerId = e.pointerId;
      wrap.setPointerCapture(state.pointerId);
      state.lastX = e.clientX;
      state.lastY = e.clientY;
      state.downX = e.clientX;
      state.downY = e.clientY;
      state.moved = false;

      if (isMiddleButton) {
        state.moving = true;
        wrap.classList.add('moving');
      } else {
        state.dragging = true;
        wrap.classList.add('dragging');
        state.lastT = performance.now();
        state.velX = 0;
        state.velY = 0;
      }
    });

    wrap.addEventListener('pointermove', (e) => {
      if (e.pointerId !== state.pointerId) return;

      if (state.moving) {
        const dx = e.clientX - state.lastX;
        const dy = e.clientY - state.lastY;
        state.lastX = e.clientX;
        state.lastY = e.clientY;

        const rect = cubeFieldEl.getBoundingClientRect();
        state.cx = Math.min(rect.width, Math.max(0, state.cx + dx));
        state.cy = Math.min(rect.height, Math.max(0, state.cy + dy));
        wrap.style.left = (state.cx / rect.width * 100) + '%';
        wrap.style.top = (state.cy / rect.height * 100) + '%';
        return;
      }

      if (!state.dragging) return;

      if (!state.moved) {
        const totalDist = Math.hypot(e.clientX - state.downX, e.clientY - state.downY);
        if (totalDist > CUBE_CLICK_THRESHOLD) state.moved = true;
      }

      const dx = e.clientX - state.lastX;
      const dy = e.clientY - state.lastY;
      const now = performance.now();
      const dt = Math.max(now - state.lastT, 1);

      state.rotY += dx * CUBE_SENSITIVITY;
      state.rotX -= dy * CUBE_SENSITIVITY;

      state.velY = (dx * CUBE_SENSITIVITY) / dt * 16;
      state.velX = (-dy * CUBE_SENSITIVITY) / dt * 16;

      state.lastX = e.clientX;
      state.lastY = e.clientY;
      state.lastT = now;
    });

    function endCubeDrag(e) {
      if (state.pointerId !== null && e.pointerId !== state.pointerId) return;
      const wasClick = state.dragging && !state.moved;
      state.dragging = false;
      state.moving = false;
      state.pointerId = null;
      wrap.classList.remove('dragging', 'moving');
      if (wasClick) selectCube(state);
    }
    wrap.addEventListener('pointerup', endCubeDrag);
    wrap.addEventListener('pointercancel', endCubeDrag);

    cubeInstances.push(state);
    addCubeBtn.disabled = cubeInstances.length >= MAX_CUBES;
    return state;
  }

  function cubeLoop() {
    cubeInstances.forEach((s) => {
      if (!s.dragging) {
        s.velX *= CUBE_FRICTION;
        s.velY *= CUBE_FRICTION;
        s.rotX += s.velX;
        s.rotY += s.velY + s.idleSpeed;
      }
      s.cubeDiv.style.transform = `rotateX(${s.rotX}deg) rotateY(${s.rotY}deg)`;
    });
    cubeRAF = requestAnimationFrame(cubeLoop);
  }

  addCubeBtn.addEventListener('click', () => {
    if (cubeInstances.length >= MAX_CUBES) return;
    createCubeInstance(false);
  });

  // both experiments start immediately — this is now one continuous
  // scrolling page, not separate views to mount/unmount
  createCubeInstance(true);
  requestAnimationFrame(cubeLoop);

  /* =========================================================
     Experiment 03 — flocking
  ========================================================= */
  (function () {
    const canvas = document.getElementById('flockCanvas');
    const section = document.getElementById('section-flock');
    const ctx = canvas.getContext('2d');
    const reduceMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

    let width = 0, height = 0;
    let boids = [];
    const pointer = { x: -9999, y: -9999, active: false };

    const MAX_SPEED = reduceMotion ? 0.9 : 2.1;
    const FLEE_SPEED_BOOST = reduceMotion ? 0.6 : 3.5; // extra top speed available right under the cursor
    const NEIGHBOR_RADIUS = 70;
    const SEPARATION_RADIUS = 26;
    const FLEE_RADIUS = 130;
    const LINE_RADIUS = 85;
    const W_SEPARATION = 1.6;
    const W_ALIGNMENT = 1.0;
    const W_COHESION = 0.7;
    const W_FLEE = reduceMotion ? 2.5 : 7;
    const FRICTION = 0.985;

    function resize() {
      const rect = section.getBoundingClientRect();
      width = rect.width;
      height = rect.height;
      const dpr = Math.min(window.devicePixelRatio || 1, 2);
      canvas.width = Math.round(width * dpr);
      canvas.height = Math.round(height * dpr);
      canvas.style.width = width + 'px';
      canvas.style.height = height + 'px';
      ctx.setTransform(dpr, 0, 0, dpr, 0, 0);
    }

    function makeBoid() {
      const angle = Math.random() * Math.PI * 2;
      return {
        x: Math.random() * width,
        y: Math.random() * height,
        vx: Math.cos(angle) * 0.6,
        vy: Math.sin(angle) * 0.6,
      };
    }

    function seedBoids() {
      const count = Math.max(50, Math.min(170, Math.round((width * height) / 9000)));
      boids = Array.from({ length: count }, makeBoid);
    }

    function wrapDelta(d, size) {
      if (d > size / 2) return d - size;
      if (d < -size / 2) return d + size;
      return d;
    }

    function step() {
      for (let i = 0; i < boids.length; i++) {
        const b = boids[i];
        let sepX = 0, sepY = 0;
        let aliX = 0, aliY = 0, aliCount = 0;
        let cohX = 0, cohY = 0, cohCount = 0;

        for (let j = 0; j < boids.length; j++) {
          if (i === j) continue;
          const o = boids[j];
          const dx = wrapDelta(o.x - b.x, width);
          const dy = wrapDelta(o.y - b.y, height);
          const dist = Math.hypot(dx, dy);
          if (dist < NEIGHBOR_RADIUS && dist > 0.0001) {
            if (dist < SEPARATION_RADIUS) {
              const f = (SEPARATION_RADIUS - dist) / SEPARATION_RADIUS;
              sepX -= (dx / dist) * f;
              sepY -= (dy / dist) * f;
            }
            aliX += o.vx; aliY += o.vy; aliCount++;
            cohX += dx; cohY += dy; cohCount++;
          }
        }

        if (aliCount > 0) { aliX /= aliCount; aliY /= aliCount; }
        if (cohCount > 0) { cohX /= cohCount; cohY /= cohCount; }

        let fleeX = 0, fleeY = 0;
        let speedBoost = 0;
        if (pointer.active) {
          const dx = b.x - pointer.x;
          const dy = b.y - pointer.y;
          const dist = Math.hypot(dx, dy);
          if (dist < FLEE_RADIUS && dist > 0.0001) {
            const proximity = 1 - dist / FLEE_RADIUS; // 0 = at the edge, 1 = right at the cursor
            const f = proximity * proximity;
            fleeX = (dx / dist) * f;
            fleeY = (dy / dist) * f;
            speedBoost = proximity * FLEE_SPEED_BOOST;
          }
        }

        b.vx += sepX * W_SEPARATION * 0.05
              + aliX * W_ALIGNMENT * 0.02
              + cohX * W_COHESION * 0.0008
              + fleeX * W_FLEE * 0.08
              + (Math.random() - 0.5) * 0.02;
        b.vy += sepY * W_SEPARATION * 0.05
              + aliY * W_ALIGNMENT * 0.02
              + cohY * W_COHESION * 0.0008
              + fleeY * W_FLEE * 0.08
              + (Math.random() - 0.5) * 0.02;

        b.vx *= FRICTION;
        b.vy *= FRICTION;

        const effectiveMaxSpeed = MAX_SPEED + speedBoost;
        const speed = Math.hypot(b.vx, b.vy);
        if (speed > effectiveMaxSpeed) {
          b.vx = (b.vx / speed) * effectiveMaxSpeed;
          b.vy = (b.vy / speed) * effectiveMaxSpeed;
        } else if (speed < 0.3) {
          const angle = speed > 0.0001 ? Math.atan2(b.vy, b.vx) : Math.random() * Math.PI * 2;
          b.vx += Math.cos(angle) * 0.02;
          b.vy += Math.sin(angle) * 0.02;
        }

        b.x = (b.x + b.vx + width) % width;
        b.y = (b.y + b.vy + height) % height;
      }
    }

    function draw() {
      ctx.clearRect(0, 0, width, height);

      ctx.lineWidth = 1;
      for (let i = 0; i < boids.length; i++) {
        for (let j = i + 1; j < boids.length; j++) {
          const a = boids[i], b = boids[j];
          const dx = a.x - b.x, dy = a.y - b.y;
          const dist = Math.hypot(dx, dy);
          if (dist < LINE_RADIUS) {
            ctx.strokeStyle = `rgba(0,0,0,${0.12 * (1 - dist / LINE_RADIUS)})`;
            ctx.beginPath();
            ctx.moveTo(a.x, a.y);
            ctx.lineTo(b.x, b.y);
            ctx.stroke();
          }
        }
      }

      ctx.fillStyle = '#000000';
      boids.forEach((b) => {
        const angle = Math.atan2(b.vy, b.vx);
        ctx.save();
        ctx.translate(b.x, b.y);
        ctx.rotate(angle);
        ctx.beginPath();
        ctx.moveTo(7, 0);
        ctx.lineTo(-5, 4);
        ctx.lineTo(-5, -4);
        ctx.closePath();
        ctx.fill();
        ctx.restore();
      });
    }

    function loop() {
      step();
      draw();
      requestAnimationFrame(loop);
    }

    function setPointerFromEvent(e) {
      const rect = canvas.getBoundingClientRect();
      pointer.x = e.clientX - rect.left;
      pointer.y = e.clientY - rect.top;
      pointer.active = true;
    }

    section.addEventListener('pointermove', setPointerFromEvent);
    section.addEventListener('pointerdown', setPointerFromEvent);
    section.addEventListener('pointerleave', () => { pointer.active = false; });

    let resizeTimer = null;
    window.addEventListener('resize', () => {
      clearTimeout(resizeTimer);
      resizeTimer = setTimeout(() => {
        resize();
        seedBoids();
      }, 150);
    });

    resize();
    seedBoids();
    requestAnimationFrame(loop);
  })();

  /* =========================================================
     Experiment 04 — magnetic field
  ========================================================= */
  (function () {
    const canvas = document.getElementById('magnetCanvas');
    const section = document.getElementById('section-magnet');
    const ctx = canvas.getContext('2d');
    const reduceMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

    let width = 0, height = 0;
    let dots = [];
    const pointer = { x: -9999, y: -9999, active: false };

    const SPACING = 42;
    const ATTRACT_RADIUS = 340;
    const MAX_PULL = reduceMotion ? 14 : 36;
    const SPRING_K = 0.16;
    const DAMPING = 0.78;

    function resize() {
      const rect = section.getBoundingClientRect();
      width = rect.width;
      height = rect.height;
      const dpr = Math.min(window.devicePixelRatio || 1, 2);
      canvas.width = Math.round(width * dpr);
      canvas.height = Math.round(height * dpr);
      canvas.style.width = width + 'px';
      canvas.style.height = height + 'px';
      ctx.setTransform(dpr, 0, 0, dpr, 0, 0);
    }

    function buildGrid() {
      dots = [];
      const marginX = ((width % SPACING) / 2) + SPACING / 2;
      const marginY = ((height % SPACING) / 2) + SPACING / 2;
      for (let gy = marginY; gy < height; gy += SPACING) {
        for (let gx = marginX; gx < width; gx += SPACING) {
          dots.push({ hx: gx, hy: gy, x: gx, y: gy, vx: 0, vy: 0 });
        }
      }
    }

    function step() {
      dots.forEach((d) => {
        let targetX = d.hx, targetY = d.hy;

        if (pointer.active) {
          const dx = pointer.x - d.hx;
          const dy = pointer.y - d.hy;
          const dist = Math.hypot(dx, dy);
          if (dist < ATTRACT_RADIUS && dist > 0.0001) {
            const pull = Math.pow(1 - dist / ATTRACT_RADIUS, 1.4) * MAX_PULL;
            targetX = d.hx + (dx / dist) * pull;
            targetY = d.hy + (dy / dist) * pull;
          }
        }

        const ax = (targetX - d.x) * SPRING_K - d.vx * DAMPING;
        const ay = (targetY - d.y) * SPRING_K - d.vy * DAMPING;
        d.vx += ax;
        d.vy += ay;
        d.x += d.vx;
        d.y += d.vy;
      });
    }

    function draw() {
      ctx.clearRect(0, 0, width, height);
      dots.forEach((d) => {
        const dispX = d.x - d.hx;
        const dispY = d.y - d.hy;
        const disp = Math.hypot(dispX, dispY);
        const pulledFraction = Math.min(1, disp / MAX_PULL);
        const radius = 2 + pulledFraction * 2.6;

        if (disp > 1) {
          ctx.strokeStyle = `rgba(0,0,0,${0.15 + pulledFraction * 0.1})`;
          ctx.lineWidth = 1;
          ctx.beginPath();
          ctx.moveTo(d.hx, d.hy);
          ctx.lineTo(d.x, d.y);
          ctx.stroke();
        }

        ctx.fillStyle = '#000000';
        ctx.beginPath();
        ctx.arc(d.x, d.y, radius, 0, Math.PI * 2);
        ctx.fill();
      });
    }

    function loop() {
      step();
      draw();
      requestAnimationFrame(loop);
    }

    function setPointerFromEvent(e) {
      const rect = canvas.getBoundingClientRect();
      pointer.x = e.clientX - rect.left;
      pointer.y = e.clientY - rect.top;
      pointer.active = true;
    }

    section.addEventListener('pointermove', setPointerFromEvent);
    section.addEventListener('pointerdown', setPointerFromEvent);
    section.addEventListener('pointerleave', () => { pointer.active = false; });

    let resizeTimer = null;
    window.addEventListener('resize', () => {
      clearTimeout(resizeTimer);
      resizeTimer = setTimeout(() => {
        resize();
        buildGrid();
      }, 150);
    });

    resize();
    buildGrid();
    requestAnimationFrame(loop);
  })();

  /* =========================================================
     Experiment 05 — eye
     A shaded sphere with a Higgsfield-generated iris that
     drifts toward the cursor, like a giant eye watching you.
  ========================================================= */
  (function () {
    const section = document.getElementById('section-eye');
    const eyeEl = document.getElementById('eye');
    const irisEl = document.getElementById('iris');
    const reduceMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

    const MAX_OFFSET_FRACTION = 0.30; // how far the iris can drift, as a fraction of the eye's own width
    const EASE = reduceMotion ? 1 : 0.22;

    let targetX = 0, targetY = 0;
    let curX = 0, curY = 0;
    const pointer = { active: false, x: 0, y: 0 };

    function updateTarget() {
      if (!pointer.active) { targetX = 0; targetY = 0; return; }
      const rect = eyeEl.getBoundingClientRect();
      const cx = rect.left + rect.width / 2;
      const cy = rect.top + rect.height / 2;
      const dx = pointer.x - cx;
      const dy = pointer.y - cy;
      const dist = Math.hypot(dx, dy) || 1;
      const maxOffset = rect.width * MAX_OFFSET_FRACTION;
      const pull = Math.min(1, dist / (rect.width * 1.2)); // ramps up to full drift as the cursor moves further off
      const mag = maxOffset * (0.35 + 0.65 * pull);
      targetX = (dx / dist) * mag;
      targetY = (dy / dist) * mag;
    }

    function loop() {
      updateTarget();
      curX += (targetX - curX) * EASE;
      curY += (targetY - curY) * EASE;
      irisEl.style.transform = `translate(-50%, -50%) translate(${curX}px, ${curY}px)`;
      requestAnimationFrame(loop);
    }

    function setPointerFromEvent(e) {
      pointer.x = e.clientX;
      pointer.y = e.clientY;
      pointer.active = true;
    }

    section.addEventListener('pointermove', setPointerFromEvent);
    section.addEventListener('pointerdown', setPointerFromEvent);
    section.addEventListener('pointerleave', () => { pointer.active = false; });

    // an occasional blink keeps it feeling alive rather than a static prop
    function scheduleBlink() {
      const delay = 2600 + Math.random() * 4200;
      setTimeout(() => {
        if (!reduceMotion) {
          eyeEl.classList.add('blink');
          eyeEl.addEventListener('animationend', function handler() {
            eyeEl.classList.remove('blink');
            eyeEl.removeEventListener('animationend', handler);
          });
        }
        scheduleBlink();
      }, delay);
    }
    scheduleBlink();

    requestAnimationFrame(loop);
  })();
</script>
</body>
</html>
