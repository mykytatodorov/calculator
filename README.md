# calculator
<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MT. Kalkulator</title>
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@300;400;500;600;700&family=Bebas+Neue&display=swap" rel="stylesheet">
<style>
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
:root{
  --red:#e8273a;
  --dark:#111;
  --light:#f5f4f0;
  --gray:#888;
  --border:#e0e0e0;
  --font:'Space Grotesk',sans-serif;
}
body{
  font-family:var(--font);
  background:var(--light);
  min-height:100vh;
  display:flex;align-items:center;justify-content:center;
  position:relative;overflow:hidden;
}
body::before{
  content:'';position:fixed;inset:0;
  background-image:linear-gradient(rgba(0,0,0,0.045) 1px,transparent 1px),
                   linear-gradient(90deg,rgba(0,0,0,0.045) 1px,transparent 1px);
  background-size:52px 52px;pointer-events:none;
}

/* ——— WRAPPER ——— */
.calc-wrapper{
  position:relative;z-index:1;
  display:flex;flex-direction:column;align-items:center;gap:20px;
}
.calc-tag{
  font-size:0.62rem;letter-spacing:4px;text-transform:uppercase;
  color:var(--gray);font-weight:600;
  display:flex;align-items:center;gap:8px;
}
.calc-tag::before{content:'';width:24px;height:1px;background:var(--red)}
.calc-tag::after{content:'';width:24px;height:1px;background:var(--red)}

/* ——— CALCULATOR BODY ——— */
.calc{
  width:340px;
  background:#fff;
  border:1px solid var(--border);
  box-shadow:0 8px 48px rgba(0,0,0,0.08),0 2px 8px rgba(0,0,0,0.04);
  overflow:hidden;
}

/* Header bar */
.calc-header{
  background:var(--dark);
  padding:14px 20px;
  display:flex;justify-content:space-between;align-items:center;
}
.calc-logo{
  font-family:'Bebas Neue',sans-serif;
  font-size:1rem;letter-spacing:3px;color:#fff;
}
.calc-logo span{color:var(--red)}
.calc-mode{
  font-size:0.58rem;letter-spacing:2px;text-transform:uppercase;
  color:rgba(255,255,255,0.4);font-weight:600;
}

/* Display */
.calc-display{
  background:#fff;
  padding:20px 24px 16px;
  border-bottom:1px solid var(--border);
  min-height:100px;
  display:flex;flex-direction:column;justify-content:flex-end;align-items:flex-end;
  position:relative;
  overflow:hidden;
}
.display-history{
  font-size:0.78rem;color:var(--gray);
  min-height:20px;margin-bottom:6px;
  letter-spacing:0.5px;word-break:break-all;text-align:right;
  transition:opacity 0.2s;
}
.display-main{
  font-size:2.6rem;font-weight:300;color:var(--dark);
  line-height:1;text-align:right;word-break:break-all;
  transition:transform 0.1s ease, font-size 0.2s;
  letter-spacing:-1px;
}
.display-main.big{font-size:2.6rem}
.display-main.med{font-size:2rem}
.display-main.small{font-size:1.5rem}
.display-main.pulse{animation:pulse 0.12s ease}
@keyframes pulse{0%{transform:scale(1)}50%{transform:scale(0.97)}100%{transform:scale(1)}}

/* corner accent */
.calc-display::after{
  content:'';position:absolute;
  bottom:0;left:0;width:3px;height:40px;
  background:var(--red);
}

/* Buttons grid */
.calc-buttons{
  display:grid;
  grid-template-columns:repeat(4,1fr);
  gap:1px;
  background:var(--border);
}
.btn{
  background:#fff;
  border:none;
  padding:0;
  height:72px;
  font-family:var(--font);
  font-size:1.05rem;
  font-weight:500;
  color:var(--dark);
  cursor:pointer;
  transition:background 0.12s,color 0.12s,transform 0.08s;
  position:relative;
  overflow:hidden;
  letter-spacing:0;
}
.btn:active{transform:scale(0.94)}
.btn::after{
  content:'';position:absolute;inset:0;
  background:var(--dark);opacity:0;transition:opacity 0.15s;
}
.btn:hover::after{opacity:0.04}

/* Button types */
.btn-op{
  background:#fafafa;
  color:var(--dark);
  font-weight:600;font-size:1.1rem;
}
.btn-op:hover{background:#f0f0f0}

.btn-eq{
  background:var(--red);
  color:#fff;
  font-size:1.3rem;font-weight:700;
  grid-row:span 2;
  height:145px;
}
.btn-eq:hover{background:#c01e2e}
.btn-eq::after{background:#fff}
.btn-eq:active{transform:scale(0.96)}

.btn-clear{
  background:var(--dark);color:#fff;
  font-size:0.75rem;font-weight:700;letter-spacing:2px;text-transform:uppercase;
}
.btn-clear:hover{background:#222}
.btn-clear::after{background:#fff}

.btn-sign,.btn-pct{
  background:#fafafa;font-size:0.9rem;color:var(--gray);font-weight:600;
}

.btn-zero{grid-column:span 2}

/* bottom bar */
.calc-footer{
  padding:10px 20px;
  display:flex;justify-content:space-between;align-items:center;
  border-top:1px solid var(--border);
}
.calc-footer-label{font-size:0.58rem;letter-spacing:2px;color:var(--gray);text-transform:uppercase;font-weight:600}

/* ripple */
.ripple{
  position:absolute;border-radius:50%;
  background:rgba(0,0,0,0.08);
  transform:scale(0);animation:rippleAnim 0.4s linear;pointer-events:none;
}
@keyframes rippleAnim{to{transform:scale(4);opacity:0}}

/* ——— HISTORY PANEL ——— */
.history-panel{
  width:340px;background:#fff;border:1px solid var(--border);
  max-height:0;overflow:hidden;transition:max-height 0.4s ease;
}
.history-panel.open{max-height:220px}
.history-header{
  padding:12px 20px;border-bottom:1px solid var(--border);
  display:flex;justify-content:space-between;align-items:center;
}
.history-title{font-size:0.62rem;letter-spacing:3px;text-transform:uppercase;font-weight:700;color:var(--dark)}
.history-clear{font-size:0.6rem;letter-spacing:1px;color:var(--red);cursor:pointer;font-weight:600;border:none;background:none;font-family:var(--font)}
.history-list{padding:8px 0;max-height:170px;overflow-y:auto;scrollbar-width:thin}
.history-item{
  padding:8px 20px;display:flex;justify-content:space-between;
  font-size:0.82rem;cursor:pointer;transition:background 0.15s;
}
.history-item:hover{background:var(--light)}
.history-expr{color:var(--gray)}
.history-res{font-weight:700;color:var(--dark)}
.history-empty{padding:20px;text-align:center;font-size:0.78rem;color:var(--gray)}

/* toggle history btn */
.toggle-history{
  font-size:0.6rem;letter-spacing:2px;text-transform:uppercase;font-weight:700;
  color:var(--gray);border:none;background:none;font-family:var(--font);cursor:pointer;
  display:flex;align-items:center;gap:6px;transition:color 0.2s;
}
.toggle-history:hover{color:var(--dark)}
.toggle-history .arr{transition:transform 0.3s}
.toggle-history.open .arr{transform:rotate(180deg)}

@media(max-width:400px){
  .calc,.history-panel{width:calc(100vw - 40px)}
  .btn{height:64px}
  .btn-eq{height:129px}
}
</style>
</head>
<body>

<div class="calc-wrapper">
  <div class="calc-tag">MT. Portfolio // Kalkulator</div>

  <div class="calc">
    <div class="calc-header">
      <div class="calc-logo">MT<span>.</span></div>
      <div class="calc-mode">Standard</div>
    </div>

    <div class="calc-display">
      <div class="display-history" id="history">&#8203;</div>
      <div class="display-main big" id="display">0</div>
    </div>

    <div class="calc-buttons">
      <!-- Row 1 -->
      <button class="btn btn-clear" onclick="clearAll()" onmousedown="ripple(event)">AC</button>
      <button class="btn btn-sign" onclick="toggleSign()" onmousedown="ripple(event)">+/−</button>
      <button class="btn btn-pct" onclick="percent()" onmousedown="ripple(event)">%</button>
      <button class="btn btn-op" onclick="setOp('/')" onmousedown="ripple(event)" id="op-/">÷</button>
      <!-- Row 2 -->
      <button class="btn" onclick="input('7')" onmousedown="ripple(event)">7</button>
      <button class="btn" onclick="input('8')" onmousedown="ripple(event)">8</button>
      <button class="btn" onclick="input('9')" onmousedown="ripple(event)">9</button>
      <button class="btn btn-op" onclick="setOp('*')" onmousedown="ripple(event)" id="op-*">×</button>
      <!-- Row 3 -->
      <button class="btn" onclick="input('4')" onmousedown="ripple(event)">4</button>
      <button class="btn" onclick="input('5')" onmousedown="ripple(event)">5</button>
      <button class="btn" onclick="input('6')" onmousedown="ripple(event)">6</button>
      <button class="btn btn-op" onclick="setOp('-')" onmousedown="ripple(event)" id="op--">−</button>
      <!-- Row 4 -->
      <button class="btn" onclick="input('1')" onmousedown="ripple(event)">1</button>
      <button class="btn" onclick="input('2')" onmousedown="ripple(event)">2</button>
      <button class="btn" onclick="input('3')" onmousedown="ripple(event)">3</button>
      <button class="btn btn-op" onclick="setOp('+')" onmousedown="ripple(event)" id="op-+">+</button>
      <!-- Row 5 -->
      <button class="btn btn-zero" onclick="input('0')" onmousedown="ripple(event)">0</button>
      <button class="btn" onclick="inputDot()" onmousedown="ripple(event)">,</button>
      <button class="btn btn-eq" onclick="calculate()" onmousedown="ripple(event)">=</button>
    </div>

    <div class="calc-footer">
      <div class="calc-footer-label">v2025.06</div>
      <button class="toggle-history" id="hist-toggle" onclick="toggleHistory()">
        Verlauf <span class="arr">▲</span>
      </button>
    </div>
  </div>

  <div class="history-panel" id="hist-panel">
    <div class="history-header">
      <span class="history-title">Verlauf</span>
      <button class="history-clear" onclick="clearHistory()">Löschen</button>
    </div>
    <div class="history-list" id="hist-list">
      <div class="history-empty" id="hist-empty">Noch keine Berechnungen</div>
    </div>
  </div>
</div>

<script>
let current = '0';
let prev = null;
let operator = null;
let newInput = true;
let history = [];

const display = document.getElementById('display');
const histDisplay = document.getElementById('history');

function updateDisplay(val) {
  let str = String(val);
  // german comma
  let shown = str.replace('.', ',');
  // limit length
  if (shown.replace('-','').replace(',','').length > 10) {
    let num = parseFloat(str);
    shown = num.toPrecision(8).replace('.',',').replace(/\.?0+e/, 'e');
  }
  display.textContent = shown;
  // size
  display.className = 'display-main';
  const len = shown.length;
  if(len <= 9) display.classList.add('big');
  else if(len <= 13) display.classList.add('med');
  else display.classList.add('small');
  // pulse
  display.classList.add('pulse');
  setTimeout(() => display.classList.remove('pulse'), 120);
}

function input(digit) {
  if(newInput) { current = digit; newInput = false; }
  else {
    if(current === '0' && digit !== '.') current = digit;
    else if(current.length < 12) current += digit;
  }
  updateDisplay(current);
}

function inputDot() {
  if(newInput) { current = '0.'; newInput = false; }
  else if(!current.includes('.')) current += '.';
  updateDisplay(current);
}

function setOp(op) {
  if(!newInput && prev !== null && operator) calculate(true);
  prev = parseFloat(current);
  operator = op;
  newInput = true;
  const opSymbols = {'/':'÷','*':'×','-':'−','+':'+'};
  histDisplay.textContent = current.replace('.',',') + ' ' + opSymbols[op];
  // highlight op button
  document.querySelectorAll('.btn-op').forEach(b => b.style.background = '');
  const btn = document.getElementById('op-' + op);
  if(btn) btn.style.background = '#f0f0f0';
}

function calculate(intermediate) {
  if(operator === null || prev === null) return;
  const a = prev;
  const b = parseFloat(current);
  const opSymbols = {'/':'÷','*':'×','-':'−','+':'+'};
  let result;
  switch(operator) {
    case '+': result = a + b; break;
    case '-': result = a - b; break;
    case '*': result = a * b; break;
    case '/': result = b === 0 ? 'Fehler' : a / b; break;
  }
  if(!intermediate) {
    const expr = String(a).replace('.',',') + ' ' + opSymbols[operator] + ' ' + String(b).replace('.',',');
    const res = result === 'Fehler' ? 'Fehler' : String(result).replace('.',',');
    histDisplay.textContent = expr + ' =';
    addToHistory(expr, res);
    operator = null;
    prev = null;
    document.querySelectorAll('.btn-op').forEach(b => b.style.background = '');
  }
  current = result === 'Fehler' ? 'Fehler' : String(result);
  updateDisplay(current.replace('.',','));
  newInput = true;
}

function clearAll() {
  current = '0'; prev = null; operator = null; newInput = true;
  histDisplay.innerHTML = '&#8203;';
  updateDisplay('0');
  document.querySelectorAll('.btn-op').forEach(b => b.style.background = '');
}

function toggleSign() {
  if(current === '0' || current === 'Fehler') return;
  current = current.startsWith('-') ? current.slice(1) : '-' + current;
  updateDisplay(current);
}

function percent() {
  if(current === 'Fehler') return;
  current = String(parseFloat(current) / 100);
  updateDisplay(current);
  newInput = true;
}

// History
function addToHistory(expr, res) {
  history.unshift({expr, res});
  if(history.length > 20) history.pop();
  renderHistory();
}

function renderHistory() {
  const list = document.getElementById('hist-list');
  const empty = document.getElementById('hist-empty');
  if(history.length === 0) {
    empty.style.display = 'block';
    list.querySelectorAll('.history-item').forEach(el => el.remove());
    return;
  }
  empty.style.display = 'none';
  list.querySelectorAll('.history-item').forEach(el => el.remove());
  history.forEach(item => {
    const div = document.createElement('div');
    div.className = 'history-item';
    div.innerHTML = `<span class="history-expr">${item.expr}</span><span class="history-res">${item.res}</span>`;
    div.onclick = () => {
      current = item.res.replace(',','.');
      updateDisplay(item.res);
      newInput = true;
    };
    list.appendChild(div);
  });
}

function clearHistory() {
  history = [];
  renderHistory();
}

function toggleHistory() {
  const panel = document.getElementById('hist-panel');
  const btn = document.getElementById('hist-toggle');
  panel.classList.toggle('open');
  btn.classList.toggle('open');
}

// Ripple effect
function ripple(e) {
  const btn = e.currentTarget;
  const r = document.createElement('span');
  r.className = 'ripple';
  const size = Math.max(btn.offsetWidth, btn.offsetHeight);
  r.style.cssText = `width:${size}px;height:${size}px;left:${e.offsetX - size/2}px;top:${e.offsetY - size/2}px`;
  btn.appendChild(r);
  setTimeout(() => r.remove(), 400);
}

// Keyboard support
document.addEventListener('keydown', e => {
  if(e.key >= '0' && e.key <= '9') input(e.key);
  else if(e.key === '.') inputDot();
  else if(e.key === ',') inputDot();
  else if(e.key === '+') setOp('+');
  else if(e.key === '-') setOp('-');
  else if(e.key === '*') setOp('*');
  else if(e.key === '/') { e.preventDefault(); setOp('/'); }
  else if(e.key === 'Enter' || e.key === '=') calculate();
  else if(e.key === 'Escape') clearAll();
  else if(e.key === 'Backspace') {
    if(current.length > 1) { current = current.slice(0,-1); updateDisplay(current); }
    else { current = '0'; updateDisplay('0'); }
  }
});
</script>
</body>
</html>

