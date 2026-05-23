# Pratt's Place Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build `pratt.html` — a standalone page for Pratt with a forest-green Zelda theme, flip interest cards, and a Triforce Dungeon mini-game featuring Link, 2 Keese, and a Bokoblin.

**Architecture:** Single self-contained HTML file (inline CSS + JS) mirroring the pattern of `index.html`. The dungeon game uses a Canvas element with a tile-based maze, smooth pixel movement, and a simple game loop via `requestAnimationFrame`.

**Tech Stack:** Vanilla HTML/CSS/JS, Canvas 2D API, Google Fonts (Orbitron + Rajdhani)

---

## File Map

- **Create:** `pratt.html` — full page, self-contained
- **Modify:** `index.html` — add "PRATT'S PLACE →" nav link in header

---

### Task 1: pratt.html — page shell, CSS variables, header

**Files:**
- Create: `pratt.html`

- [ ] **Step 1: Create pratt.html with full shell**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Pratt's Place 🐱</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@700&family=Rajdhani:wght@400;600&display=swap');

    :root {
      --z-green: #2d7a2d;
      --z-gold: #c8a400;
      --dark: #0a180a;
      --light: #e8f5e8;
    }

    * { margin: 0; padding: 0; box-sizing: border-box; }

    body {
      background: var(--dark);
      color: var(--light);
      font-family: 'Rajdhani', sans-serif;
      font-size: 18px;
      overflow-x: hidden;
    }

    /* ── HEADER ── */
    header {
      background: linear-gradient(135deg, #1a3d1a 60%, #0d260d);
      text-align: center;
      padding: 60px 20px 40px;
      position: relative;
      overflow: hidden;
    }
    header::before {
      content: '🐾';
      font-size: 200px;
      position: absolute;
      opacity: 0.07;
      top: -20px; left: -20px;
      transform: rotate(-15deg);
    }
    .nav-link {
      display: inline-block;
      margin-bottom: 16px;
      color: var(--z-gold);
      font-family: 'Orbitron', sans-serif;
      font-size: 0.7rem;
      letter-spacing: 2px;
      text-decoration: none;
      opacity: 0.7;
      transition: opacity 0.2s;
    }
    .nav-link:hover { opacity: 1; }
    header h1 {
      font-family: 'Orbitron', sans-serif;
      font-size: clamp(2rem, 6vw, 4rem);
      color: var(--z-gold);
      text-shadow: 0 0 30px rgba(200,164,0,0.5);
      letter-spacing: 4px;
    }
    header p {
      margin-top: 10px;
      font-size: 1.2rem;
      color: #a8d4a8;
    }

    footer {
      text-align: center;
      padding: 40px 20px;
      color: #2a4a2a;
      font-size: 0.9rem;
    }
    footer span { color: var(--z-gold); }
  </style>
</head>
<body>

<header>
  <a class="nav-link" href="index.html">← RIDGE'S WORLD</a>
  <h1>PRATT'S PLACE</h1>
  <p>Cat Whisperer. Zelda Master. Legend in Progress.</p>
</header>

<footer>
  Pratt's corner of the internet &nbsp;|&nbsp; <span>🐱 🗡️ 🔺</span>
</footer>

<script>
  // game code goes here
</script>
</body>
</html>
```

- [ ] **Step 2: Open pratt.html in browser and verify header renders with green theme and nav link**

---

### Task 2: Interest cards

**Files:**
- Modify: `pratt.html` (add between header and footer)

- [ ] **Step 1: Add card CSS inside `<style>`**

```css
    /* ── CARDS ── */
    .interests {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 20px;
      padding: 50px 40px;
      max-width: 1100px;
      margin: 0 auto;
    }
    .card-wrapper {
      flex: 0 1 220px;
      height: 240px;
      perspective: 1000px;
      cursor: pointer;
    }
    .card {
      width: 100%;
      height: 100%;
      position: relative;
      transform-style: preserve-3d;
      transition: transform 0.8s cubic-bezier(0.4, 0, 0.2, 1);
      border-radius: 16px;
    }
    .card.flipped { transform: rotateY(180deg); }
    .card-front, .card-back {
      position: absolute;
      inset: 0;
      border-radius: 16px;
      backface-visibility: hidden;
      -webkit-backface-visibility: hidden;
    }
    .card-front {
      background: #0d260d;
      border: 2px solid var(--z-green);
      padding: 28px 20px;
      text-align: center;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      transition: border-color 0.2s;
    }
    .card-wrapper:hover .card-front { border-color: var(--z-gold); }
    .card-back {
      transform: rotateY(180deg);
      overflow: hidden;
      border: 2px solid var(--z-gold);
      background: #0d260d;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 4rem;
    }
    .card-back img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      border-radius: 14px;
    }
    .card .icon { font-size: 3rem; margin-bottom: 12px; }
    .card h3 {
      font-family: 'Orbitron', sans-serif;
      font-size: 0.85rem;
      color: var(--z-gold);
      letter-spacing: 2px;
      margin-bottom: 8px;
    }
    .card p { color: #a8d4a8; font-size: 0.95rem; }
```

- [ ] **Step 2: Add cards HTML between header and footer**

```html
<div class="interests">
  <div class="card-wrapper" onclick="flipCard(this)">
    <div class="card">
      <div class="card-front">
        <div class="icon">🐱</div>
        <h3>CATS</h3>
        <p>The original legends. Soft. Mysterious. Superior.</p>
      </div>
      <div class="card-back">🐾</div>
    </div>
  </div>
  <div class="card-wrapper" onclick="flipCard(this)">
    <div class="card">
      <div class="card-front">
        <div class="icon">🗡️</div>
        <h3>ZELDA</h3>
        <p>It's dangerous to go alone. Take this.</p>
      </div>
      <div class="card-back">🔺</div>
    </div>
  </div>
</div>
```

- [ ] **Step 3: Add flipCard JS inside the `<script>` tag**

```js
function flipCard(wrapper) {
  const card = wrapper.querySelector('.card');
  if (card.dataset.flipping) return;
  card.dataset.flipping = '1';
  card.classList.add('flipped');
  setTimeout(() => {
    card.classList.remove('flipped');
    setTimeout(() => { delete card.dataset.flipping; }, 800);
  }, 5000);
}
```

- [ ] **Step 4: Verify cards flip and flip back in browser**

---

### Task 3: Game section HTML + canvas

**Files:**
- Modify: `pratt.html`

- [ ] **Step 1: Add game section CSS inside `<style>`**

```css
    /* ── GAME ── */
    .game-section {
      max-width: 760px;
      margin: 0 auto;
      padding: 50px 20px;
      text-align: center;
    }
    .game-section h2 {
      font-family: 'Orbitron', sans-serif;
      color: var(--z-gold);
      font-size: 1.5rem;
      margin-bottom: 6px;
      letter-spacing: 3px;
    }
    .game-section .subtitle {
      color: #6a9a6a;
      margin-bottom: 24px;
      font-size: 1rem;
    }
    #gameCanvas {
      display: block;
      margin: 0 auto;
      border: 3px solid var(--z-gold);
      border-radius: 12px;
      background: #060e06;
    }
    .game-controls {
      margin-top: 14px;
      color: #6a9a6a;
      font-size: 0.95rem;
    }
    #startBtn {
      display: inline-block;
      margin-top: 20px;
      padding: 14px 40px;
      background: var(--z-gold);
      color: var(--dark);
      font-family: 'Orbitron', sans-serif;
      font-size: 1rem;
      font-weight: 700;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      letter-spacing: 2px;
      transition: transform 0.15s, box-shadow 0.15s;
    }
    #startBtn:hover {
      transform: scale(1.05);
      box-shadow: 0 0 20px rgba(200,164,0,0.5);
    }
```

- [ ] **Step 2: Add game HTML between interests div and footer**

```html
<div class="game-section">
  <h2>TRIFORCE DUNGEON</h2>
  <p class="subtitle">Navigate the dungeon. Avoid the enemies. Find the Triforce.</p>
  <canvas id="gameCanvas" width="720" height="468"></canvas>
  <div class="game-controls">Arrow keys to move</div>
  <br>
  <button id="startBtn">ENTER DUNGEON</button>
</div>
```

- [ ] **Step 3: Verify game section layout renders in browser (canvas shows as dark rectangle)**

---

### Task 4: Maze data + tile rendering

**Files:**
- Modify: `pratt.html` — add inside `<script>` after flipCard function

- [ ] **Step 1: Add maze constants and tile map**

```js
const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');
const startBtn = document.getElementById('startBtn');

const TILE = 36;
const COLS = 20;
const ROWS = 13;
const W = TILE * COLS; // 720
const H = TILE * ROWS; // 468

// 0=floor, 1=wall, 2=player start, 3=triforce
const MAP = [
  [1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1],
  [1,2,0,0,1,0,0,0,0,1,0,0,0,1,0,0,0,0,0,1],
  [1,1,1,0,1,0,1,1,0,1,0,1,0,1,0,1,1,1,0,1],
  [1,0,0,0,0,0,1,0,0,0,0,1,0,0,0,1,0,0,0,1],
  [1,0,1,1,1,1,1,0,1,1,1,1,1,1,0,1,0,1,1,1],
  [1,0,0,0,0,0,0,0,0,0,0,0,0,1,0,0,0,1,0,1],
  [1,1,1,0,1,1,1,1,0,1,0,1,0,0,0,1,1,1,0,1],
  [1,0,0,0,1,0,0,0,0,1,0,1,1,1,1,1,0,0,0,1],
  [1,0,1,1,1,0,1,0,1,1,0,0,0,0,0,0,0,1,0,1],
  [1,0,1,0,0,0,1,0,0,0,0,1,1,0,1,1,0,1,0,1],
  [1,0,1,0,1,1,1,1,0,1,0,1,0,0,0,1,0,0,0,1],
  [1,0,0,0,0,0,0,0,0,1,0,0,0,1,0,0,0,1,3,1],
  [1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1],
];
```

- [ ] **Step 2: Add tile drawing helpers**

```js
function tileAt(col, row) {
  if (row < 0 || row >= ROWS || col < 0 || col >= COLS) return 1;
  return MAP[row][col];
}

function isWall(px, py) {
  // check all 4 corners of a 20x20 entity centered at px,py
  const r = 10;
  return (
    tileAt(Math.floor((px-r)/TILE), Math.floor((py-r)/TILE)) === 1 ||
    tileAt(Math.floor((px+r)/TILE), Math.floor((py-r)/TILE)) === 1 ||
    tileAt(Math.floor((px-r)/TILE), Math.floor((py+r)/TILE)) === 1 ||
    tileAt(Math.floor((px+r)/TILE), Math.floor((py+r)/TILE)) === 1
  );
}

function drawTiles() {
  for (let r = 0; r < ROWS; r++) {
    for (let c = 0; c < COLS; c++) {
      const t = MAP[r][c];
      const x = c * TILE, y = r * TILE;
      if (t === 1) {
        // Stone wall
        ctx.fillStyle = '#1a3a1a';
        ctx.fillRect(x, y, TILE, TILE);
        ctx.fillStyle = '#0d260d';
        ctx.fillRect(x+1, y+1, TILE-2, TILE-2);
        ctx.strokeStyle = '#2d5a2d';
        ctx.lineWidth = 0.5;
        ctx.strokeRect(x+1, y+1, TILE-2, TILE-2);
      } else {
        // Floor
        ctx.fillStyle = '#111a11';
        ctx.fillRect(x, y, TILE, TILE);
        // subtle grid
        ctx.strokeStyle = '#1a2a1a';
        ctx.lineWidth = 0.5;
        ctx.strokeRect(x, y, TILE, TILE);
      }
    }
  }
}

function drawTriforce(cx, cy) {
  const s = 12;
  ctx.fillStyle = '#c8a400';
  ctx.shadowColor = '#c8a400';
  ctx.shadowBlur = 12;
  // top triangle
  ctx.beginPath();
  ctx.moveTo(cx, cy - s);
  ctx.lineTo(cx - s, cy + s * 0.4);
  ctx.lineTo(cx + s, cy + s * 0.4);
  ctx.closePath();
  ctx.fill();
  // bottom-left
  ctx.beginPath();
  ctx.moveTo(cx - s * 1.1, cy + s * 0.5);
  ctx.lineTo(cx - s * 2.1, cy + s * 1.8);
  ctx.lineTo(cx, cy + s * 1.8);
  ctx.closePath();
  ctx.fill();
  // bottom-right
  ctx.beginPath();
  ctx.moveTo(cx + s * 1.1, cy + s * 0.5);
  ctx.lineTo(cx, cy + s * 1.8);
  ctx.lineTo(cx + s * 2.1, cy + s * 1.8);
  ctx.closePath();
  ctx.fill();
  ctx.shadowBlur = 0;
}
```

- [ ] **Step 3: Add a temporary draw call to test tile rendering — add after the helper functions:**

```js
drawTiles();
// find triforce tile and draw it
for (let r = 0; r < ROWS; r++)
  for (let c = 0; c < COLS; c++)
    if (MAP[r][c] === 3)
      drawTriforce(c*TILE + TILE/2, r*TILE + TILE/2);
```

- [ ] **Step 4: Open in browser and verify maze renders — green walls, dark floors, gold Triforce in bottom-right area**

- [ ] **Step 5: Remove the temporary draw call (lines added in Step 3) — game loop will handle drawing**

---

### Task 5: Link — sprite, state, movement

**Files:**
- Modify: `pratt.html` — add inside `<script>`

- [ ] **Step 1: Add Link drawing function**

```js
function drawLink(x, y, dir) {
  ctx.save();
  ctx.translate(x, y);

  // Tunic (green body)
  ctx.fillStyle = '#2d8a2d';
  ctx.beginPath();
  ctx.roundRect(-9, -4, 18, 20, 3);
  ctx.fill();

  // Belt
  ctx.fillStyle = '#8B4513';
  ctx.fillRect(-9, 10, 18, 3);

  // Skin (neck + hands)
  ctx.fillStyle = '#f5c5a0';
  ctx.fillRect(-4, -14, 8, 12);

  // Head
  ctx.fillStyle = '#f5c5a0';
  ctx.beginPath();
  ctx.ellipse(0, -20, 9, 10, 0, 0, Math.PI * 2);
  ctx.fill();

  // Hat (green pointy)
  ctx.fillStyle = '#2d8a2d';
  ctx.beginPath();
  ctx.moveTo(-9, -22);
  ctx.lineTo(0, -38);
  ctx.lineTo(9, -22);
  ctx.closePath();
  ctx.fill();
  ctx.beginPath();
  ctx.ellipse(0, -22, 9, 4, 0, 0, Math.PI * 2);
  ctx.fill();

  // Eyes
  ctx.fillStyle = '#333';
  ctx.beginPath(); ctx.arc(-3, -21, 1.5, 0, Math.PI*2); ctx.fill();
  ctx.beginPath(); ctx.arc(3, -21, 1.5, 0, Math.PI*2); ctx.fill();

  // Sword (to the right, or rotated by dir)
  ctx.fillStyle = '#aaa';
  if (dir === 'right') {
    ctx.fillRect(9, -8, 16, 3);
    ctx.fillStyle = '#8B4513';
    ctx.fillRect(6, -10, 4, 7);
  } else if (dir === 'left') {
    ctx.fillRect(-25, -8, 16, 3);
    ctx.fillStyle = '#8B4513';
    ctx.fillRect(-10, -10, 4, 7);
  } else if (dir === 'up') {
    ctx.fillRect(-1.5, -38, 3, 16);
    ctx.fillStyle = '#8B4513';
    ctx.fillRect(-5, -24, 10, 4);
  } else {
    ctx.fillRect(-1.5, -4, 3, 16);
    ctx.fillStyle = '#8B4513';
    ctx.fillRect(-5, 6, 10, 4);
  }

  ctx.restore();
}
```

- [ ] **Step 2: Add player state and input**

```js
// Find start tile
let startCol = 1, startRow = 1;
for (let r = 0; r < ROWS; r++)
  for (let c = 0; c < COLS; c++)
    if (MAP[r][c] === 2) { startCol = c; startRow = r; }

const player = {
  x: startCol * TILE + TILE/2,
  y: startRow * TILE + TILE/2,
  speed: 3,
  dir: 'right',
  lives: 3,
  invincible: 0,  // frames of invincibility after hit
};

const keys = {};
document.addEventListener('keydown', e => {
  keys[e.key] = true;
  if (['ArrowUp','ArrowDown','ArrowLeft','ArrowRight'].includes(e.key))
    e.preventDefault();
});
document.addEventListener('keyup', e => { keys[e.key] = false; });

function movePlayer() {
  let dx = 0, dy = 0;
  if (keys['ArrowLeft'])  { dx = -player.speed; player.dir = 'left'; }
  if (keys['ArrowRight']) { dx =  player.speed; player.dir = 'right'; }
  if (keys['ArrowUp'])    { dy = -player.speed; player.dir = 'up'; }
  if (keys['ArrowDown'])  { dy =  player.speed; player.dir = 'down'; }

  const nx = player.x + dx;
  const ny = player.y + dy;

  if (!isWall(nx, player.y)) player.x = nx;
  if (!isWall(player.x, ny)) player.y = ny;

  if (player.invincible > 0) player.invincible--;
}
```

- [ ] **Step 3: Verify by adding a temp call in browser console — open dev tools, type `drawLink(360, 234, 'right')` after calling `drawTiles()` manually. Should see Link with sword**

---

### Task 6: Keese (bat) enemies

**Files:**
- Modify: `pratt.html`

- [ ] **Step 1: Add Keese drawing function**

```js
function drawKeese(x, y, frame) {
  ctx.save();
  ctx.translate(x, y);
  const flap = Math.sin(frame * 0.2) > 0;

  // Body
  ctx.fillStyle = '#2a0a3a';
  ctx.beginPath();
  ctx.ellipse(0, 0, 8, 6, 0, 0, Math.PI * 2);
  ctx.fill();

  // Wings
  ctx.fillStyle = '#3d1a50';
  if (flap) {
    ctx.beginPath(); ctx.ellipse(-14, -4, 10, 5, -0.3, 0, Math.PI*2); ctx.fill();
    ctx.beginPath(); ctx.ellipse(14, -4, 10, 5, 0.3, 0, Math.PI*2); ctx.fill();
  } else {
    ctx.beginPath(); ctx.ellipse(-10, 2, 10, 4, 0.3, 0, Math.PI*2); ctx.fill();
    ctx.beginPath(); ctx.ellipse(10, 2, 10, 4, -0.3, 0, Math.PI*2); ctx.fill();
  }

  // Eyes
  ctx.fillStyle = '#ff2222';
  ctx.beginPath(); ctx.arc(-3, -1, 2, 0, Math.PI*2); ctx.fill();
  ctx.beginPath(); ctx.arc(3, -1, 2, 0, Math.PI*2); ctx.fill();

  ctx.restore();
}
```

- [ ] **Step 2: Add Keese state and AI**

```js
// Place Keese on open floor tiles away from start
const keese = [
  { x: 5*TILE+TILE/2, y: 5*TILE+TILE/2, vx: 1.5, vy: 0, timer: 0 },
  { x: 14*TILE+TILE/2, y: 3*TILE+TILE/2, vx: 0, vy: 1.5, timer: 40 },
];

const DIRS = [{x:1.5,y:0},{x:-1.5,y:0},{x:0,y:1.5},{x:0,y:-1.5}];

function updateKeese() {
  for (const k of keese) {
    k.timer--;
    if (k.timer <= 0 || isWall(k.x + k.vx, k.y + k.vy)) {
      // pick a random valid direction
      const shuffled = [...DIRS].sort(() => Math.random() - 0.5);
      for (const d of shuffled) {
        if (!isWall(k.x + d.x * 8, k.y + d.y * 8)) {
          k.vx = d.x; k.vy = d.y;
          break;
        }
      }
      k.timer = 30 + Math.floor(Math.random() * 40);
    }
    if (!isWall(k.x + k.vx, k.y)) k.x += k.vx;
    if (!isWall(k.x, k.y + k.vy)) k.y += k.vy;
  }
}
```

---

### Task 7: Bokoblin enemy

**Files:**
- Modify: `pratt.html`

- [ ] **Step 1: Add Bokoblin drawing function**

```js
function drawBokoblin(x, y) {
  ctx.save();
  ctx.translate(x, y);

  // Body (red)
  ctx.fillStyle = '#aa1111';
  ctx.beginPath();
  ctx.roundRect(-11, -6, 22, 22, 4);
  ctx.fill();

  // Belly
  ctx.fillStyle = '#cc3333';
  ctx.beginPath();
  ctx.ellipse(0, 4, 7, 8, 0, 0, Math.PI * 2);
  ctx.fill();

  // Head
  ctx.fillStyle = '#aa1111';
  ctx.beginPath();
  ctx.ellipse(0, -16, 12, 11, 0, 0, Math.PI * 2);
  ctx.fill();

  // Snout
  ctx.fillStyle = '#cc2222';
  ctx.beginPath();
  ctx.ellipse(0, -12, 7, 5, 0, 0, Math.PI * 2);
  ctx.fill();

  // Eyes (yellow, beady)
  ctx.fillStyle = '#ffee00';
  ctx.beginPath(); ctx.arc(-5, -18, 3, 0, Math.PI*2); ctx.fill();
  ctx.beginPath(); ctx.arc(5, -18, 3, 0, Math.PI*2); ctx.fill();
  ctx.fillStyle = '#000';
  ctx.beginPath(); ctx.arc(-5, -18, 1.5, 0, Math.PI*2); ctx.fill();
  ctx.beginPath(); ctx.arc(5, -18, 1.5, 0, Math.PI*2); ctx.fill();

  // Horns
  ctx.fillStyle = '#882222';
  ctx.beginPath(); ctx.moveTo(-8,-24); ctx.lineTo(-5,-32); ctx.lineTo(-2,-24); ctx.closePath(); ctx.fill();
  ctx.beginPath(); ctx.moveTo(2,-24); ctx.lineTo(5,-32); ctx.lineTo(8,-24); ctx.closePath(); ctx.fill();

  // Club (left arm)
  ctx.fillStyle = '#8B4513';
  ctx.fillRect(-18, -4, 6, 16);
  ctx.fillStyle = '#5a2d0c';
  ctx.beginPath();
  ctx.ellipse(-15, -6, 7, 6, 0, 0, Math.PI*2);
  ctx.fill();

  ctx.restore();
}
```

- [ ] **Step 2: Add Bokoblin state and AI**

```js
const bokoblin = {
  x: 10*TILE+TILE/2,
  y: 8*TILE+TILE/2,
  vx: 0,
  vy: 2,
  speed: 2,
  chargeSpeed: 3.5,
  timer: 0,
  charging: false,
};

function updateBokoblin() {
  const dx = player.x - bokoblin.x;
  const dy = player.y - bokoblin.y;
  const dist = Math.sqrt(dx*dx + dy*dy);
  const CHARGE_RANGE = TILE * 3;

  if (dist < CHARGE_RANGE) {
    // Charge toward player
    bokoblin.charging = true;
    const spd = bokoblin.chargeSpeed;
    const nx = bokoblin.x + (dx / dist) * spd;
    const ny = bokoblin.y + (dy / dist) * spd;
    if (!isWall(nx, bokoblin.y)) bokoblin.x = nx;
    if (!isWall(bokoblin.x, ny)) bokoblin.y = ny;
  } else {
    // Patrol — bounce off walls
    bokoblin.charging = false;
    bokoblin.timer--;
    if (bokoblin.timer <= 0 || isWall(bokoblin.x + bokoblin.vx, bokoblin.y + bokoblin.vy)) {
      const shuffled = [...DIRS.map(d => ({x: d.x/1.5*bokoblin.speed, y: d.y/1.5*bokoblin.speed}))].sort(() => Math.random() - 0.5);
      for (const d of shuffled) {
        if (!isWall(bokoblin.x + d.x * 8, bokoblin.y + d.y * 8)) {
          bokoblin.vx = d.x; bokoblin.vy = d.y;
          break;
        }
      }
      bokoblin.timer = 40 + Math.floor(Math.random() * 50);
    }
    if (!isWall(bokoblin.x + bokoblin.vx, bokoblin.y)) bokoblin.x += bokoblin.vx;
    if (!isWall(bokoblin.x, bokoblin.y + bokoblin.vy)) bokoblin.y += bokoblin.vy;
  }
}
```

---

### Task 8: Game loop, HUD, collision, win/lose

**Files:**
- Modify: `pratt.html`

- [ ] **Step 1: Add collision detection and HUD drawing**

```js
function checkEnemyCollisions() {
  if (player.invincible > 0) return;
  const all = [...keese, bokoblin];
  for (const e of all) {
    const dx = player.x - e.x;
    const dy = player.y - e.y;
    if (Math.sqrt(dx*dx + dy*dy) < 20) {
      player.lives--;
      player.invincible = 90; // ~1.5s at 60fps
      flashPage();
    }
  }
}

function checkTriforce() {
  for (let r = 0; r < ROWS; r++)
    for (let c = 0; c < COLS; c++)
      if (MAP[r][c] === 3) {
        const tx = c*TILE + TILE/2;
        const ty = r*TILE + TILE/2;
        const dx = player.x - tx;
        const dy = player.y - ty;
        if (Math.sqrt(dx*dx + dy*dy) < TILE * 0.7) return true;
      }
  return false;
}

function drawHUD() {
  ctx.fillStyle = '#c8a400';
  ctx.font = 'bold 14px Orbitron, sans-serif';
  ctx.textAlign = 'left';
  const hearts = '❤️'.repeat(Math.max(0, player.lives));
  ctx.fillText(hearts, 10, H - 10);
}

const flashEl = document.createElement('div');
Object.assign(flashEl.style, {
  position: 'fixed', inset: '0', pointerEvents: 'none',
  background: 'rgba(255,30,30,0)', zIndex: '9999',
  transition: 'background 0.05s ease-in'
});
document.body.appendChild(flashEl);

function flashPage() {
  flashEl.style.transition = 'none';
  flashEl.style.background = 'rgba(255,30,30,0.55)';
  setTimeout(() => {
    flashEl.style.transition = 'background 0.35s ease-out';
    flashEl.style.background = 'rgba(255,30,30,0)';
  }, 60);
}
```

- [ ] **Step 2: Add game state and main loop**

```js
let gameState = 'idle';
let frame = 0;

function resetGame() {
  player.x = startCol * TILE + TILE/2;
  player.y = startRow * TILE + TILE/2;
  player.dir = 'right';
  player.lives = 3;
  player.invincible = 0;
  keese[0].x = 5*TILE+TILE/2; keese[0].y = 5*TILE+TILE/2; keese[0].vx = 1.5; keese[0].vy = 0;
  keese[1].x = 14*TILE+TILE/2; keese[1].y = 3*TILE+TILE/2; keese[1].vx = 0; keese[1].vy = 1.5;
  bokoblin.x = 10*TILE+TILE/2; bokoblin.y = 8*TILE+TILE/2;
  bokoblin.charging = false;
  frame = 0;
  gameState = 'playing';
  startBtn.textContent = 'RESTART';
}

startBtn.addEventListener('click', resetGame);

function loop() {
  requestAnimationFrame(loop);
  ctx.clearRect(0, 0, W, H);
  frame++;

  drawTiles();

  // Draw triforce
  for (let r = 0; r < ROWS; r++)
    for (let c = 0; c < COLS; c++)
      if (MAP[r][c] === 3)
        drawTriforce(c*TILE + TILE/2, r*TILE + TILE/2);

  if (gameState === 'idle') {
    ctx.fillStyle = 'rgba(0,0,0,0.5)';
    ctx.fillRect(0, 0, W, H);
    ctx.fillStyle = '#c8a400';
    ctx.font = 'bold 20px Orbitron, sans-serif';
    ctx.textAlign = 'center';
    ctx.fillText('ENTER THE DUNGEON', W/2, H/2 - 10);
    ctx.fillStyle = '#6a9a6a';
    ctx.font = '15px Rajdhani, sans-serif';
    ctx.fillText('Use arrow keys — reach the Triforce!', W/2, H/2 + 20);
    return;
  }

  if (gameState === 'playing') {
    movePlayer();
    updateKeese();
    updateBokoblin();
    checkEnemyCollisions();

    if (checkTriforce()) gameState = 'win';
    if (player.lives <= 0) gameState = 'dead';
  }

  // Draw enemies
  for (const k of keese) drawKeese(k.x, k.y, frame);
  drawBokoblin(bokoblin.x, bokoblin.y);

  // Draw Link (flicker when invincible)
  if (player.invincible === 0 || frame % 6 < 3) {
    drawLink(player.x, player.y, player.dir);
  }

  drawHUD();

  if (gameState === 'win') {
    ctx.fillStyle = 'rgba(0,0,0,0.7)';
    ctx.fillRect(0, 0, W, H);
    ctx.fillStyle = '#c8a400';
    ctx.shadowColor = '#c8a400';
    ctx.shadowBlur = 20;
    ctx.font = 'bold 28px Orbitron, sans-serif';
    ctx.textAlign = 'center';
    ctx.fillText('YOU FOUND THE TRIFORCE', W/2, H/2 - 20);
    ctx.shadowBlur = 0;
    ctx.fillStyle = '#a8d4a8';
    ctx.font = '18px Rajdhani, sans-serif';
    ctx.fillText('Courage, Wisdom, Power — all yours.', W/2, H/2 + 16);
  }

  if (gameState === 'dead') {
    ctx.fillStyle = 'rgba(0,0,0,0.75)';
    ctx.fillRect(0, 0, W, H);
    ctx.fillStyle = '#ff4444';
    ctx.font = 'bold 28px Orbitron, sans-serif';
    ctx.textAlign = 'center';
    ctx.fillText('GAME OVER', W/2, H/2 - 20);
    ctx.fillStyle = '#6a9a6a';
    ctx.font = '18px Rajdhani, sans-serif';
    ctx.fillText('Ganon wins... this time.', W/2, H/2 + 16);
  }
}

loop();
```

- [ ] **Step 3: Open in browser, click ENTER DUNGEON, verify:**
  - Link spawns top-left area, moves with arrow keys
  - Keese bats patrol and bounce off walls
  - Bokoblin patrols and charges when Link gets close
  - Hitting an enemy flashes red and costs a heart
  - Reaching the Triforce shows win screen
  - Running out of hearts shows game over screen
  - Restart button resets properly

---

### Task 9: Add nav link to index.html + commit everything

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add nav link CSS to index.html `<style>` block**

```css
    .nav-link {
      display: inline-block;
      margin-top: 12px;
      color: #FFA300;
      font-family: 'Orbitron', sans-serif;
      font-size: 0.7rem;
      letter-spacing: 2px;
      text-decoration: none;
      opacity: 0.7;
      transition: opacity 0.2s;
    }
    .nav-link:hover { opacity: 1; }
```

- [ ] **Step 2: Add link inside `<header>` in index.html, after the `<p>` subtitle**

```html
  <a class="nav-link" href="pratt.html">PRATT'S PLACE →</a>
```

- [ ] **Step 3: Commit and push everything**

```bash
git add pratt.html index.html docs/
git commit -m "Add Pratt's Place — Zelda dungeon game, flip cards, nav links"
git push
```

- [ ] **Step 4: Verify nav links work both ways — Ridge's page links to Pratt's, Pratt's links back**
