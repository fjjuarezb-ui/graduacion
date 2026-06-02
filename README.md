[index-2.html](https://github.com/user-attachments/files/28494225/index-2.html)
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Control de Botellas</title>
<style>
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif;background:#f0f4f0;min-height:100vh;color:#1a1a1a}
.container{max-width:480px;margin:0 auto;padding:1.25rem}
.hidden{display:none!important}

/* SCAN */
.scan-card{background:#fff;border-radius:16px;padding:2rem 1.5rem;text-align:center;box-shadow:0 2px 12px rgba(0,0,0,.08);margin-top:2rem}
.icon-circle{width:80px;height:80px;border-radius:50%;background:#E1F5EE;display:flex;align-items:center;justify-content:center;margin:0 auto 1rem;font-size:38px}
.scan-title{font-size:22px;font-weight:700;margin-bottom:4px}
.scan-sub{font-size:14px;color:#666;margin-bottom:1.25rem}
.badge{display:inline-block;padding:5px 16px;border-radius:8px;font-size:13px;font-weight:600;margin-bottom:1.25rem}
.badge-ok{background:#E1F5EE;color:#085041}
.badge-used{background:#E6F1FB;color:#042C53}
.badge-exp{background:#FAECE7;color:#4A1B0C}
.btn-open{background:#0F6E56;color:#fff;border:none;border-radius:10px;padding:14px 0;font-size:16px;font-weight:700;cursor:pointer;width:100%;margin-top:4px}
.msg-small{font-size:13px;color:#666;margin-top:10px}

/* EXPIRED */
.exp-box{background:#FAECE7;border:1px solid #F0997B;border-radius:12px;padding:2rem;text-align:center;margin-top:2rem}
.exp-box h2{color:#4A1B0C;font-size:20px;margin-bottom:8px}
.exp-box p{color:#993C1D;font-size:14px}

/* DASHBOARD */
.dash-header{background:#0F6E56;color:#fff;border-radius:12px;padding:1.25rem 1.5rem;margin-bottom:1rem}
.dash-header h1{font-size:18px;font-weight:700}
.dash-header p{font-size:13px;opacity:.8;margin-top:2px}
.metrics{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:1rem}
.metric{background:#fff;border-radius:10px;padding:1rem;box-shadow:0 1px 4px rgba(0,0,0,.06)}
.metric-label{font-size:12px;color:#666;margin-bottom:4px}
.metric-value{font-size:28px;font-weight:700}
.green{color:#0F6E56}
.prog-bg{background:#ddd;border-radius:99px;height:8px;margin-bottom:4px}
.prog-fill{background:#0F6E56;border-radius:99px;height:8px;transition:width .5s}
.prog-label{font-size:12px;color:#666;text-align:right;margin-bottom:1rem}
.tabs{display:flex;background:#fff;border-radius:10px;padding:4px;box-shadow:0 1px 4px rgba(0,0,0,.06);margin-bottom:1rem}
.tab{flex:1;padding:8px;text-align:center;font-size:13px;cursor:pointer;border-radius:7px;border:none;background:none;color:#666;font-weight:600}
.tab.active{background:#0F6E56;color:#fff}
.bottles-grid{display:grid;grid-template-columns:repeat(5,1fr);gap:6px;margin-bottom:1rem}
.bcell{aspect-ratio:1;border-radius:8px;border:1px solid #ddd;display:flex;flex-direction:column;align-items:center;justify-content:center;font-size:10px;color:#aaa;background:#fff}
.bcell.open{background:#E1F5EE;border-color:#0F6E56;color:#085041;font-weight:700}
.bcell-icon{font-size:16px;line-height:1.2}
.log-list{list-style:none;display:flex;flex-direction:column;gap:8px}
.log-item{background:#fff;border-radius:10px;padding:12px 14px;display:flex;align-items:center;gap:12px;box-shadow:0 1px 4px rgba(0,0,0,.06)}
.log-dot{width:36px;height:36px;border-radius:50%;background:#E1F5EE;display:flex;align-items:center;justify-content:center;font-size:18px;flex-shrink:0}
.log-name{font-weight:600;font-size:14px}
.log-time{font-size:12px;color:#666}
.empty{text-align:center;color:#aaa;font-size:14px;padding:2rem 0}
.reset-btn{margin-top:1.5rem;background:none;border:1px solid #ddd;border-radius:8px;padding:10px;font-size:12px;color:#aaa;cursor:pointer;width:100%}
.pop{animation:pop .35s ease}
@keyframes pop{0%{transform:scale(.8);opacity:0}60%{transform:scale(1.1)}100%{transform:scale(1);opacity:1}}
</style>
</head>
<body>

<div id="screen-scan" class="container hidden">
  <div class="scan-card">
    <div class="icon-circle" id="si-icon">🍾</div>
    <div class="scan-title">Botella <span id="si-num">#?</span></div>
    <div class="scan-sub">Graduación &middot; <span id="si-date"></span></div>
    <div><span class="badge badge-ok" id="si-badge">Disponible</span></div>
    <div id="si-action"></div>
  </div>
</div>

<div id="screen-exp" class="container hidden">
  <div class="exp-box">
    <div style="font-size:40px;margin-bottom:12px">⏰</div>
    <h2>QR Expirado</h2>
    <p>Este código ya no es válido.<br>El evento de 7 días ha concluido.</p>
  </div>
</div>

<div id="screen-dash" class="container hidden">
  <div class="dash-header">
    <h1>🎓 Control de Botellas</h1>
    <p id="dash-date"></p>
  </div>
  <div class="metrics">
    <div class="metric"><div class="metric-label">Total botellas</div><div class="metric-value" id="m-total">50</div></div>
    <div class="metric"><div class="metric-label">Abiertas</div><div class="metric-value green" id="m-open">0</div></div>
    <div class="metric"><div class="metric-label">Disponibles</div><div class="metric-value" id="m-avail">50</div></div>
    <div class="metric"><div class="metric-label">Consumo</div><div class="metric-value" id="m-pct">0%</div></div>
  </div>
  <div class="prog-bg"><div class="prog-fill" id="prog" style="width:0%"></div></div>
  <div class="prog-label" id="prog-lbl">0 de 50 abiertas</div>
  <div class="tabs">
    <button class="tab active" id="tab-btn-grid" onclick="showTab('grid')">Mapa</button>
    <button class="tab" id="tab-btn-log" onclick="showTab('log')">Registro</button>
  </div>
  <div id="tab-grid">
    <div class="bottles-grid" id="bottles-grid"></div>
  </div>
  <div id="tab-log" class="hidden">
    <ul class="log-list" id="log-list"></ul>
  </div>
  <button class="reset-btn" onclick="resetAll()">↺ Reiniciar datos del evento</button>
</div>

<script>
var TOTAL = 50;
var DAYS = 7;
var KEY = 'grad_v3';

function loadState() {
  try {
    var raw = localStorage.getItem(KEY);
    if (raw) return JSON.parse(raw);
  } catch(e) {}
  return null;
}

function saveState(s) {
  try { localStorage.setItem(KEY, JSON.stringify(s)); } catch(e) {}
}

var state = loadState();
if (!state || !state.created) {
  state = { created: Date.now(), opened: {}, log: [] };
  saveState(state);
}

function fmtDate(ts) {
  try {
    return new Date(ts).toLocaleString('es-HN', {day:'2-digit',month:'short',hour:'2-digit',minute:'2-digit'});
  } catch(e) { return new Date(ts).toLocaleString(); }
}

function todayStr() {
  try {
    return new Date().toLocaleDateString('es-HN', {day:'2-digit',month:'long',year:'numeric'});
  } catch(e) { return new Date().toLocaleDateString(); }
}

function show(id) {
  var ids = ['screen-scan','screen-exp','screen-dash'];
  for (var i=0; i<ids.length; i++) {
    var el = document.getElementById(ids[i]);
    if (el) el.className = 'container' + (ids[i]===id ? '' : ' hidden');
  }
}

function showTab(t) {
  document.getElementById('tab-grid').className = t==='grid' ? '' : 'hidden';
  document.getElementById('tab-log').className = t==='log' ? '' : 'hidden';
  document.getElementById('tab-btn-grid').className = 'tab' + (t==='grid' ? ' active' : '');
  document.getElementById('tab-btn-log').className = 'tab' + (t==='log' ? ' active' : '');
}

function doOpen(n) {
  state.opened[n] = Date.now();
  state.log.push({ b: n, ts: Date.now() });
  saveState(state);
  document.getElementById('si-icon').textContent = '✅';
  document.getElementById('si-icon').style.background = '#0F6E56';
  var badge = document.getElementById('si-badge');
  badge.className = 'badge badge-ok pop';
  badge.textContent = '¡Registrada!';
  var t = new Date().toLocaleTimeString('es-HN',{hour:'2-digit',minute:'2-digit'});
  document.getElementById('si-action').innerHTML = '<p class="msg-small">Registrada a las ' + t + '</p>';
}

function refreshDash() {
  var n = Object.keys(state.opened).length;
  var pct = Math.round(n / TOTAL * 100);
  document.getElementById('m-open').textContent = n;
  document.getElementById('m-avail').textContent = TOTAL - n;
  document.getElementById('m-pct').textContent = pct + '%';
  document.getElementById('prog').style.width = pct + '%';
  document.getElementById('prog-lbl').textContent = n + ' de ' + TOTAL + ' abiertas';

  var grid = document.getElementById('bottles-grid');
  var html = '';
  for (var i=1; i<=TOTAL; i++) {
    var open = !!state.opened[i];
    html += '<div class="bcell' + (open?' open':'') + '">';
    html += '<div class="bcell-icon">' + (open?'🍾':'·') + '</div>';
    html += '<div>' + i + '</div></div>';
  }
  grid.innerHTML = html;

  var list = document.getElementById('log-list');
  if (!state.log || !state.log.length) {
    list.innerHTML = '<li class="empty">Sin registros aún.</li>';
  } else {
    var items = '';
    var reversed = state.log.slice().reverse();
    for (var j=0; j<reversed.length; j++) {
      var e = reversed[j];
      items += '<li class="log-item"><div class="log-dot">🍾</div>';
      items += '<div><div class="log-name">Botella #' + e.b + '</div>';
      items += '<div class="log-time">' + fmtDate(e.ts) + '</div></div></li>';
    }
    list.innerHTML = items;
  }
}

function resetAll() {
  if (confirm('¿Reiniciar todos los registros? No se puede deshacer.')) {
    state = { created: Date.now(), opened: {}, log: [] };
    saveState(state);
    refreshDash();
  }
}

function route() {
  var p = new URLSearchParams(window.location.search);
  var b = parseInt(p.get('b'));
  var t = parseInt(p.get('t'));

  if (b && t) {
    show('screen-scan');
    document.getElementById('si-num').textContent = '#' + b;
    document.getElementById('si-date').textContent = todayStr();

    if ((Date.now() - t) > DAYS * 86400000) {
      show('screen-exp');
      return;
    }

    var badge = document.getElementById('si-badge');
    var action = document.getElementById('si-action');

    if (state.opened[b]) {
      badge.className = 'badge badge-used';
      badge.textContent = 'Ya registrada';
      action.innerHTML = '<p class="msg-small">Abierta el ' + fmtDate(state.opened[b]) + '</p>';
    } else {
      badge.className = 'badge badge-ok';
      badge.textContent = 'Disponible';
      action.innerHTML = '<button class="btn-open" onclick="doOpen(' + b + ')">Registrar apertura</button>';
    }
    return;
  }

  show('screen-dash');
  document.getElementById('dash-date').textContent = todayStr();
  refreshDash();
}

route();
</script>
</body>
</html>
