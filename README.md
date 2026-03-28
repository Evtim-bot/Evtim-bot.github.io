<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Darts Checkout Trainer</title>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@700;900&family=Share+Tech+Mono&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg: #0a0a0f;
    --panel: #12121a;
    --border: #1e1e2e;
    --accent: #00ffcc;
    --accent2: #ff4466;
    --accent3: #ffd700;
    --text: #e0e0f0;
    --muted: #555577;
  }

  body {
    font-family: 'Share Tech Mono', monospace;
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    background-image:
      radial-gradient(ellipse at 20% 10%, rgba(0,255,204,0.05) 0%, transparent 50%),
      radial-gradient(ellipse at 80% 90%, rgba(255,68,102,0.05) 0%, transparent 50%);
  }

  /* ── TOP NAV ── */
  .top-nav {
    width: 100%;
    background: var(--panel);
    border-bottom: 1px solid var(--border);
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 24px;
    height: 56px;
    position: sticky;
    top: 0;
    z-index: 100;
    box-shadow: 0 2px 20px rgba(0,0,0,0.4);
  }

  .nav-brand {
    font-family: 'Orbitron', sans-serif;
    font-size: 0.85rem;
    color: var(--accent);
    letter-spacing: 0.15em;
    text-shadow: 0 0 12px rgba(0,255,204,0.4);
    white-space: nowrap;
  }

  .nav-tabs { display: flex; gap: 4px; }

  .nav-tab {
    font-family: 'Orbitron', sans-serif;
    font-size: 0.65rem;
    letter-spacing: 0.1em;
    padding: 7px 18px;
    border-radius: 6px;
    border: 1px solid transparent;
    cursor: pointer;
    background: transparent;
    color: var(--muted);
    transition: all 0.18s;
  }
  .nav-tab:hover { color: var(--text); border-color: var(--border); }
  .nav-tab.active {
    background: rgba(0,255,204,0.1);
    border-color: var(--accent);
    color: var(--accent);
    text-shadow: 0 0 8px rgba(0,255,204,0.3);
  }

  /* ── PAGES ── */
  .page { display: none; width: 100%; }
  .page.active { display: flex; flex-direction: column; align-items: center; }

  /* ── GAME PAGE ── */
  #page-game { padding: 30px 20px 60px; }

  h1 {
    font-family: 'Orbitron', sans-serif;
    font-size: clamp(1.4rem, 4vw, 2.2rem);
    color: var(--accent);
    letter-spacing: 0.1em;
    text-shadow: 0 0 20px rgba(0,255,204,0.4);
    margin-bottom: 6px;
  }

  .subtitle {
    color: var(--muted);
    font-size: 0.8rem;
    letter-spacing: 0.2em;
    text-transform: uppercase;
    margin-bottom: 30px;
  }

  .scoreboard { display: flex; gap: 20px; margin-bottom: 28px; }

  .score-box {
    background: var(--panel);
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 10px 24px;
    text-align: center;
  }
  .score-box .label {
    font-size: 0.65rem; letter-spacing: 0.2em;
    color: var(--muted); text-transform: uppercase; margin-bottom: 4px;
  }
  .score-box .val {
    font-family: 'Orbitron', sans-serif;
    font-size: 1.6rem; color: var(--accent3);
    text-shadow: 0 0 10px rgba(255,215,0,0.3);
  }

  .target-card {
    background: var(--panel);
    border: 1px solid var(--accent);
    border-radius: 12px;
    padding: 24px 48px;
    margin-bottom: 28px;
    text-align: center;
    box-shadow: 0 0 30px rgba(0,255,204,0.1), inset 0 0 30px rgba(0,255,204,0.03);
    min-width: 260px;
  }
  .target-label {
    font-size: 0.7rem; letter-spacing: 0.3em;
    color: var(--muted); text-transform: uppercase; margin-bottom: 8px;
  }
  .target-num {
    font-family: 'Orbitron', sans-serif;
    font-size: clamp(2.5rem, 8vw, 4rem);
    color: #fff; text-shadow: 0 0 30px rgba(255,255,255,0.3); line-height: 1;
  }

  .input-row {
    display: flex; gap: 10px; margin-bottom: 16px;
    width: 100%; max-width: 480px;
  }

  input {
    flex: 1; padding: 12px 16px;
    font-family: 'Share Tech Mono', monospace; font-size: 1rem;
    background: var(--panel); border: 1px solid var(--border);
    border-radius: 8px; color: var(--text); outline: none;
    transition: border-color 0.2s; letter-spacing: 0.05em;
  }
  input:focus { border-color: var(--accent); box-shadow: 0 0 0 2px rgba(0,255,204,0.1); }
  input::placeholder { color: var(--muted); }

  button {
    padding: 12px 22px; font-family: 'Orbitron', sans-serif;
    font-size: 0.75rem; letter-spacing: 0.1em;
    border-radius: 8px; border: none; cursor: pointer; transition: all 0.15s;
  }
  .btn-submit { background: var(--accent); color: #000; font-weight: 700; }
  .btn-submit:hover { background: #00e6b8; box-shadow: 0 0 16px rgba(0,255,204,0.4); }
  .btn-restart {
    background: transparent; border: 1px solid var(--accent2);
    color: var(--accent2); font-size: 0.7rem; margin-top: 8px;
  }
  .btn-restart:hover { background: rgba(255,68,102,0.1); }

  .result { min-height: 48px; text-align: center; font-size: 0.9rem; margin-bottom: 8px; }
  .correct { color: #00ff88; text-shadow: 0 0 8px rgba(0,255,136,0.4); }
  .wrong { color: var(--accent2); }
  .answer-reveal { color: var(--accent3); margin-top: 4px; font-size: 0.85rem; }
  .hidden { display: none !important; }

  .leaderboard { margin-top: 36px; width: 100%; max-width: 340px; }
  .lb-title {
    font-family: 'Orbitron', sans-serif; font-size: 0.8rem;
    letter-spacing: 0.2em; color: var(--muted); text-transform: uppercase;
    text-align: center; margin-bottom: 12px;
    border-bottom: 1px solid var(--border); padding-bottom: 8px;
  }
  #leaderboard { list-style: none; counter-reset: lb; }
  #leaderboard li {
    counter-increment: lb; display: flex; align-items: center;
    gap: 12px; padding: 6px 12px; border-radius: 6px; transition: background 0.15s;
  }
  #leaderboard li::before {
    content: counter(lb); font-family: 'Orbitron', sans-serif;
    font-size: 0.7rem; color: var(--muted); width: 18px; text-align: right;
  }
  #leaderboard li:nth-child(1) { color: var(--accent3); }
  #leaderboard li:nth-child(2) { color: #c0c0c0; }
  #leaderboard li:nth-child(3) { color: #cd7f32; }
  #leaderboard li:hover { background: var(--panel); }
  .lb-score { font-family: 'Orbitron', sans-serif; font-size: 1rem; }
  .lb-bar { flex: 1; height: 3px; background: var(--border); border-radius: 2px; overflow: hidden; }
  .lb-fill { height: 100%; background: var(--accent); border-radius: 2px; transition: width 0.4s; }

  @keyframes pop {
    0%   { transform: scale(1); }
    50%  { transform: scale(1.08); }
    100% { transform: scale(1); }
  }
  .pop { animation: pop 0.25s ease; }

  /* ── TABLE PAGE ── */
  #page-table { padding: 28px 16px 60px; max-width: 980px; width: 100%; }

  .table-header { text-align: center; margin-bottom: 24px; }
  .table-header h2 {
    font-family: 'Orbitron', sans-serif;
    font-size: clamp(1.2rem, 3vw, 1.8rem);
    color: var(--accent); letter-spacing: 0.1em;
    text-shadow: 0 0 16px rgba(0,255,204,0.3); margin-bottom: 6px;
  }
  .table-header p { color: var(--muted); font-size: 0.75rem; letter-spacing: 0.15em; text-transform: uppercase; }

  .legend {
    display: flex; gap: 16px; justify-content: center;
    margin-bottom: 20px; flex-wrap: wrap;
  }
  .legend-item { display: flex; align-items: center; gap: 6px; font-size: 0.7rem; color: var(--muted); letter-spacing: 0.1em; }
  .legend-dot { width: 10px; height: 10px; border-radius: 50%; }

  .table-search-wrap { display: flex; justify-content: center; margin-bottom: 20px; }
  .table-search {
    background: var(--panel); border: 1px solid var(--border);
    border-radius: 8px; padding: 10px 16px;
    font-family: 'Share Tech Mono', monospace; font-size: 0.9rem;
    color: var(--text); outline: none; width: 100%; max-width: 280px;
    transition: border-color 0.2s;
  }
  .table-search:focus { border-color: var(--accent); }
  .table-search::placeholder { color: var(--muted); }

  .section-title {
    font-family: 'Orbitron', sans-serif; font-size: 0.7rem;
    letter-spacing: 0.25em; color: var(--muted); text-transform: uppercase;
    margin: 24px 0 10px; padding-left: 8px;
    border-left: 2px solid var(--accent);
  }

  .checkout-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(210px, 1fr));
    gap: 5px;
  }

  .checkout-row {
    display: flex; align-items: stretch;
    background: var(--panel); border: 1px solid var(--border);
    border-radius: 7px; overflow: hidden;
    transition: border-color 0.15s, box-shadow 0.15s;
  }
  .checkout-row:hover {
    border-color: var(--accent);
    box-shadow: 0 0 10px rgba(0,255,204,0.08);
  }

  .co-score {
    font-family: 'Orbitron', sans-serif; font-size: 0.85rem; font-weight: 700;
    color: #fff; background: rgba(255,255,255,0.05);
    padding: 8px 10px; min-width: 46px; text-align: center;
    border-right: 1px solid var(--border);
    display: flex; align-items: center; justify-content: center;
  }

  .co-darts { padding: 7px 10px; font-size: 0.72rem; line-height: 1.6; flex: 1; }

  .co-dart {
    display: inline-block; padding: 1px 5px;
    border-radius: 3px; margin: 1px 2px 1px 0;
    font-size: 0.7rem; font-weight: bold; letter-spacing: 0.03em;
  }
  .dart-treble { background: rgba(255,68,102,0.2); color: #ff8899; border: 1px solid rgba(255,68,102,0.3); }
  .dart-double { background: rgba(0,255,204,0.12); color: var(--accent); border: 1px solid rgba(0,255,204,0.25); }
  .dart-bull   { background: rgba(255,215,0,0.15); color: var(--accent3); border: 1px solid rgba(255,215,0,0.25); }
  .dart-single { background: rgba(255,255,255,0.06); color: #aaa; border: 1px solid rgba(255,255,255,0.1); }

  .co-alt {
    font-size: 0.65rem; color: var(--muted);
    margin-top: 4px; padding-top: 4px;
    border-top: 1px dashed var(--border);
  }
  .co-alt-label { color: var(--muted); font-size: 0.6rem; margin-right: 3px; }
</style>
</head>
<body>

<!-- TOP NAV -->
<nav class="top-nav">
  <div class="nav-brand">🎯 DARTS CHECKOUT</div>
  <div class="nav-tabs">
    <button class="nav-tab active" id="tab-game" onclick="switchTab('game')">▶ TRAINER</button>
    <button class="nav-tab" id="tab-table" onclick="switchTab('table')">📋 TABLE</button>
  </div>
</nav>

<!-- ══ GAME PAGE ══ -->
<div id="page-game" class="page active">
  <h1>CHECKOUT TRAINER</h1>
  <div class="subtitle">Type the correct checkout route</div>

  <div class="scoreboard">
    <div class="score-box">
      <div class="label">Streak</div>
      <div class="val" id="score">0</div>
    </div>
    <div class="score-box">
      <div class="label">Best</div>
      <div class="val" id="best">0</div>
    </div>
  </div>

  <div class="target-card">
    <div class="target-label">Checkout</div>
    <div class="target-num" id="target">—</div>
  </div>

  <div class="input-row">
    <input id="answer" placeholder="e.g. T20 T20 Bull" autocomplete="off" />
    <button class="btn-submit" onclick="submitAnswer()">GO</button>
  </div>

  <div class="result" id="result"></div>
  <button id="restartBtn" class="btn-restart hidden" onclick="restart()">▶ New Run</button>

  <div class="leaderboard">
    <div class="lb-title">Top 10 Scores</div>
    <ol id="leaderboard"></ol>
  </div>
</div>

<!-- ══ TABLE PAGE ══ -->
<div id="page-table" class="page">
  <div class="table-header">
    <h2>CHECKOUT TABLE</h2>
    <p>All standard routes · hover to highlight</p>
  </div>

  <div class="legend">
    <div class="legend-item"><div class="legend-dot" style="background:#ff8899"></div>Treble</div>
    <div class="legend-item"><div class="legend-dot" style="background:#00ffcc"></div>Double</div>
    <div class="legend-item"><div class="legend-dot" style="background:#ffd700"></div>Bull / 25</div>
    <div class="legend-item"><div class="legend-dot" style="background:#888"></div>Single</div>
  </div>

  <div class="table-search-wrap">
    <input class="table-search" id="tableSearch" placeholder="Search score… e.g. 170" oninput="filterTable()" />
  </div>

  <div id="tableContent"></div>
</div>

<script>
/* ── DATA ───────────────────────────────────────────────────────── */
const checkouts = {
  170: ["T20 T20 Bull"],
  167: ["T20 T19 Bull"],
  164: ["T20 T18 Bull"],
  161: ["T20 T17 Bull"],
  160: ["T20 T20 D20"],
  158: ["T20 T20 D19"],
  157: ["T20 T19 D20"],
  156: ["T20 T20 D18"],
  155: ["T20 T19 D19"],
  154: ["T20 T18 D20"],
  153: ["T20 T19 D18"],
  152: ["T20 T20 D16"],
  151: ["T20 T17 D20"],
  150: ["T20 T18 D18"],
  149: ["T20 T19 D16"],
  148: ["T20 T20 D14"],
  147: ["T20 T17 D18"],
  146: ["T20 T18 D16"],
  145: ["T20 T19 D14","T20 T15 D20"],
  144: ["T20 T20 D12"],
  143: ["T20 T17 D16"],
  142: ["T20 T14 D20"],
  141: ["T20 T19 D12"],
  140: ["T20 T20 D10"],
  139: ["T20 T13 D20","T19 T14 D20"],
  138: ["T20 T18 D12"],
  137: ["T20 T15 D16","T20 T19 D10"],
  136: ["T20 T20 D8"],
  135: ["25 T20 Bull"],
  134: ["T20 T14 D16"],
  133: ["T20 T19 D8"],
  132: ["25 T19 Bull"],
  131: ["T20 T13 D16"],
  130: ["T20 T20 D5"],
  129: ["T19 T12 D18","19 T20 Bull"],
  128: ["T18 T14 D16","18 T20 Bull"],
  127: ["T20 T17 D8","T20 T17 Bull"],
  126: ["T19 T19 D6","T19 T19 Bull"],
  125: ["25 T20 D20","T18 T13 D16"],
  124: ["T20 T14 Bull","T20 T16 D8"],
  123: ["T19 T16 Bull","T19 T16 D9"],
  122: ["T18 T20 D4","T18 18 Bull"],
  121: ["T20 T11 Bull","T20 T11 D14"],
  120: ["T20 20 D20"],
  119: ["T19 T12 Bull","T19 T12 D13"],
  118: ["T20 18 D20"],
  117: ["T20 17 D20"],
  116: ["T19 19 D20"],
  115: ["T19 18 D20"],
  114: ["T20 14 D20"],
  113: ["T19 16 D20"],
  112: ["T20 20 D16"],
  111: ["T19 14 D20"],
  110: ["T20 10 D20"],
  109: ["T20 17 D16","T20 9 D20"],
  108: ["T20 16 D16"],
  107: ["T19 10 D20", "T20 15 D16"],
  106: ["T20 6 D20"],
  105: ["T19 16 D16", "T20 13 D16"],
  104: ["T16 16 D20"],
  103: ["T19 6 D20"],
  102: ["T20 10 D16", "T16 14 D20"],
  101: ["T20 9 D16"],
  100: ["T20 D20"],
  99:  ["T19 10 D16"],
  98:  ["T20 D19"],
  97:  ["T19 D20"],
  96:  ["T20 D18"],
  95:  ["T19 D19"],
  94:  ["T18 D20"],
  93:  ["T19 D18"],
  92:  ["T20 D16"],
  91:  ["T17 D20"],
  90:  ["T20 D15"],
  89:  ["T19 D16"],
  88:  ["T20 D14"],
  87:  ["T17 D18"],
  86:  ["T18 D16"],
  85:  ["T15 D20"],
  84:  ["T20 D12"],
  83:  ["T17 D16"],
  82:  ["Bull D16", "T14 D20"],
  81:  ["T19 D12"],
  80:  ["T20 D10"],
  79:  ["T19 D11"],
  78:  ["T18 D12"],
  77:  ["T19 D10"],
  76:  ["T20 D8"],
  75:  ["T17 D12"],
  74:  ["T14 D16"],
  73:  ["T19 D8"],
  72:  ["T16 D12"],
  71:  ["T13 D16"],
  70:  ["T18 D8"],
  69:  ["T19 D6"],
  68:  ["T16 D10","T20 D4"],
  67:  ["T9 D20", "T17 D8"],
  66:  ["T10 D18", "T14 D12"],
  65:  ["T11 D16", "T19 D4"],
  64:  ["T16 D8"],
  63:  ["T17 D6", "T13 D12"],
  62:  ["T10 D16"],
  61:  ["T15 D8"],
  60:  ["20 D20"],
  59:  ["19 D20"],
  58:  ["18 D20"],
  57:  ["17 D20"],
  56:  ["16 D20"],
  55:  ["15 D20"],
  54:  ["14 D20"],
  53:  ["13 D20"],
  52:  ["12 D20"],
  51:  ["11 D20"],
  50:  ["10 D20","18 D16","Bull"],
  49:  ["9 D20","17 D16"],
  48:  ["16 D16","8 D20"],
  47:  ["15 D16","7 D20"],
  46:  ["6 D20","14 D16"],
  45:  ["13 D16","5 D20"],
  44:  ["12 D16","4 D20"],
  43:  ["11 D16","3 D20"],
  42:  ["10 D16","2 D20"],
  41:  ["9 D16","1 D20"],
  40:  ["D20"],
  38:  ["D19"],
  36:  ["D18"],
  34:  ["D17"],
  32:  ["D16"],
  30:  ["D15"],
  28:  ["D14"],
  26:  ["D13"],
  24:  ["D12"],
  22:  ["D11"],
  20:  ["D10"],
  18:  ["D9"],
  16:  ["D8"],
  14:  ["D7"],
  12:  ["D6"],
  10:  ["D5"],
  8:   ["D4"],
  6:   ["D3"],
  4:   ["D2"],
  2:   ["D1"]
};

/* ── TAB SWITCHING ──────────────────────────────────────────────── */
function switchTab(tab) {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.nav-tab').forEach(t => t.classList.remove('active'));
  document.getElementById('page-' + tab).classList.add('active');
  document.getElementById('tab-' + tab).classList.add('active');
  if (tab === 'game') document.getElementById('answer').focus();
}

/* ── DART RENDERER ──────────────────────────────────────────────── */
function dartClass(token) {
  const t = token.toUpperCase();
  if (t === 'BULL') return 'dart-bull';
  if (t === '25')   return 'dart-bull';
  if (t.startsWith('T')) return 'dart-treble';
  if (t.startsWith('D')) return 'dart-double';
  return 'dart-single';
}

function renderRoute(route) {
  return route.split(' ').map(d =>
    `<span class="co-dart ${dartClass(d)}">${d}</span>`
  ).join('');
}

/* ── TABLE BUILDER ──────────────────────────────────────────────── */
const sections = [
  { label: '3-Dart Finishes  (101 – 170)', min: 101, max: 170 },
  { label: '2-Dart Finishes  (41 – 100)',  min: 41,  max: 100 },
  { label: '1-Dart Finishes  (2 – 40)',    min: 2,   max: 40  },
];

function buildTable(filter = '') {
  const container = document.getElementById('tableContent');
  container.innerHTML = '';
  const allKeys = Object.keys(checkouts).map(Number).sort((a, b) => b - a);

  sections.forEach(sec => {
    const keys = allKeys.filter(k => {
      if (k < sec.min || k > sec.max) return false;
      return !filter || String(k).includes(filter);
    });
    if (!keys.length) return;

    const title = document.createElement('div');
    title.className = 'section-title';
    title.textContent = sec.label;
    container.appendChild(title);

    const grid = document.createElement('div');
    grid.className = 'checkout-grid';

    keys.forEach(k => {
      const routes = checkouts[k];
      const row = document.createElement('div');
      row.className = 'checkout-row';

      const scoreEl = document.createElement('div');
      scoreEl.className = 'co-score';
      scoreEl.textContent = k;

      const dartsEl = document.createElement('div');
      dartsEl.className = 'co-darts';
      dartsEl.innerHTML = renderRoute(routes[0]);

      if (routes.length > 1) {
        routes.slice(1).forEach(alt => {
          const altDiv = document.createElement('div');
          altDiv.className = 'co-alt';
          altDiv.innerHTML = `<span class="co-alt-label">or</span>` + renderRoute(alt);
          dartsEl.appendChild(altDiv);
        });
      }

      row.appendChild(scoreEl);
      row.appendChild(dartsEl);
      grid.appendChild(row);
    });

    container.appendChild(grid);
  });

  if (!container.innerHTML.trim()) {
    container.innerHTML = `<p style="text-align:center;color:var(--muted);padding:40px 0">No checkout found for "${filter}"</p>`;
  }
}

function filterTable() {
  buildTable(document.getElementById('tableSearch').value.trim());
}

/* ── GAME LOGIC ─────────────────────────────────────────────────── */
let currentTarget;
let score = 0;

function getBestScore() {
  return (JSON.parse(localStorage.getItem("dartScores") || "[]")[0]) || 0;
}
function updateBestDisplay() {
  document.getElementById("best").textContent = getBestScore();
}
function getRandomCheckout() {
  const keys = Object.keys(checkouts);
  return parseInt(keys[Math.floor(Math.random() * keys.length)]);
}
function nextRound() {
  currentTarget = getRandomCheckout();
  const el = document.getElementById("target");
  el.textContent = currentTarget;
  el.classList.remove("pop");
  void el.offsetWidth;
  el.classList.add("pop");
  document.getElementById("answer").value = "";
  document.getElementById("result").innerHTML = "";
  document.getElementById("answer").focus();
}
function normalize(str) {
  return str.toUpperCase().replace(/\s+/g, ' ').trim();
}
function submitAnswer() {
  const input = normalize(document.getElementById("answer").value);
  if (!input) return;
  const correct = checkouts[currentTarget];
  if (correct.some(r => normalize(r) === input)) {
    score++;
    const s = document.getElementById("score");
    s.textContent = score;
    s.classList.remove("pop"); void s.offsetWidth; s.classList.add("pop");
    document.getElementById("result").innerHTML = `<span class="correct">✓ Correct!</span>`;
    setTimeout(nextRound, 500);
  } else {
    document.getElementById("result").innerHTML =
      `<span class="wrong">✗ Wrong!</span><br>
       <span class="answer-reveal">→ ${correct.join("  OR  ")}</span>`;
    saveScore(score);
    score = 0;
    document.getElementById("restartBtn").classList.remove("hidden");
  }
}
function restart() {
  document.getElementById("score").textContent = "0";
  document.getElementById("restartBtn").classList.add("hidden");
  document.getElementById("result").innerHTML = "";
  nextRound();
}
function saveScore(n) {
  if (n === 0) return;
  let s = JSON.parse(localStorage.getItem("dartScores") || "[]");
  s.push(n); s.sort((a,b)=>b-a); s=s.slice(0,10);
  localStorage.setItem("dartScores", JSON.stringify(s));
  displayScores(); updateBestDisplay();
}
function displayScores() {
  const s = JSON.parse(localStorage.getItem("dartScores") || "[]");
  const max = s[0] || 1;
  const list = document.getElementById("leaderboard");
  list.innerHTML = "";
  s.forEach(v => {
    const li = document.createElement("li");
    li.innerHTML = `<span class="lb-score">${v}</span>
      <div class="lb-bar"><div class="lb-fill" style="width:${Math.round(v/max*100)}%"></div></div>`;
    list.appendChild(li);
  });
}

document.getElementById("answer").addEventListener("keydown", e => {
  if (e.key === "Enter") submitAnswer();
});

/* ── INIT ── */
buildTable();
displayScores();
updateBestDisplay();
nextRound();
</script>
</body>
</html>
