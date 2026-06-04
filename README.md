[index.html](https://github.com/user-attachments/files/28601330/index.html)
# Caballos<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Dashboard — Registro de Caballos</title>
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=DM+Sans:ital,wght@0,300;0,400;0,500;0,600;1,400&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<style>
  :root {
    --bg: #0d1117;
    --surface: #161b22;
    --surface2: #1c2430;
    --border: #30363d;
    --accent: #f0a500;
    --accent2: #e05c2a;
    --text: #e6edf3;
    --muted: #7d8590;
    --green: #3fb950;
    --blue: #58a6ff;
    --red: #ff7b72;
  }
  * { margin:0; padding:0; box-sizing:border-box; }
  body { background:var(--bg); color:var(--text); font-family:'DM Sans',sans-serif; min-height:100vh; }

  /* HEADER */
  .header {
    background: linear-gradient(135deg,#0d1117,#1a2332);
    border-bottom: 2px solid var(--accent);
    padding: 18px 28px;
    display: flex; align-items:center; justify-content:space-between;
    position: sticky; top:0; z-index:200;
  }
  .header-left { display:flex; align-items:center; gap:14px; }
  .logo { width:40px; height:40px; background:var(--accent); border-radius:8px; display:flex; align-items:center; justify-content:center; font-size:20px; }
  .header h1 { font-family:'Bebas Neue',sans-serif; font-size:26px; letter-spacing:2px; }
  .header h1 span { color:var(--accent); }
  .header-sub { color:var(--muted); font-size:12px; }
  .btn { padding:8px 18px; border-radius:8px; font-family:'DM Sans',sans-serif; font-size:13px; font-weight:600; cursor:pointer; border:none; transition:all .2s; letter-spacing:.5px; }
  .btn-accent { background:var(--accent); color:#000; }
  .btn-accent:hover { background:#ffc233; }
  .btn-ghost { background:transparent; color:var(--muted); border:1px solid var(--border); }
  .btn-ghost:hover { color:var(--text); border-color:var(--text); }
  .btn-red { background:transparent; color:var(--red); border:1px solid var(--red); }
  .btn-red:hover { background:var(--red); color:#000; }
  .btn-green { background:var(--green); color:#000; }
  .btn-green:hover { filter:brightness(1.1); }
  .btn-sm { padding:5px 12px; font-size:12px; }

  /* MAIN */
  .main { padding:24px 28px; max-width:1400px; margin:0 auto; }

  /* KPIS */
  .kpis { display:grid; grid-template-columns:repeat(4,1fr); gap:14px; margin-bottom:24px; }
  .kpi { background:var(--surface); border:1px solid var(--border); border-radius:12px; padding:18px 20px; position:relative; overflow:hidden; cursor:default; animation:fadeUp .4s ease both; }
  .kpi::before { content:''; position:absolute; top:0; left:0; right:0; height:3px; }
  .kpi.gold::before { background:var(--accent); }
  .kpi.orange::before { background:var(--accent2); }
  .kpi.green::before { background:var(--green); }
  .kpi.blue::before { background:var(--blue); }
  .kpi-label { font-size:10px; font-weight:600; letter-spacing:1.5px; text-transform:uppercase; color:var(--muted); margin-bottom:8px; }
  .kpi-value { font-family:'Bebas Neue',sans-serif; font-size:40px; line-height:1; margin-bottom:4px; }
  .kpi.gold .kpi-value { color:var(--accent); }
  .kpi.orange .kpi-value { color:var(--accent2); }
  .kpi.green .kpi-value { color:var(--green); }
  .kpi.blue .kpi-value { color:var(--blue); }
  .kpi-sub { font-size:11px; color:var(--muted); }
  .kpi-icon { position:absolute; right:14px; top:50%; transform:translateY(-50%); font-size:32px; opacity:.1; }

  /* CHARTS */
  .charts-row { display:grid; grid-template-columns:1fr 1fr; gap:18px; margin-bottom:22px; }
  .chart-card { background:var(--surface); border:1px solid var(--border); border-radius:12px; padding:20px; animation:fadeUp .5s ease both; }
  .section-title { font-size:11px; font-weight:600; letter-spacing:1.5px; text-transform:uppercase; color:var(--muted); margin-bottom:16px; display:flex; align-items:center; gap:8px; }
  .section-title::before { content:''; width:3px; height:13px; background:var(--accent); border-radius:2px; }

  /* TABLE CARD */
  .table-card { background:var(--surface); border:1px solid var(--border); border-radius:12px; overflow:hidden; margin-bottom:24px; animation:fadeUp .6s ease both; }
  .table-toolbar { padding:16px 20px; display:flex; align-items:center; justify-content:space-between; border-bottom:1px solid var(--border); gap:12px; flex-wrap:wrap; }
  .toolbar-left { display:flex; align-items:center; gap:10px; }
  .search-box { background:var(--surface2); border:1px solid var(--border); border-radius:8px; padding:7px 14px; color:var(--text); font-family:'DM Sans',sans-serif; font-size:13px; outline:none; width:190px; transition:border-color .2s; }
  .search-box:focus { border-color:var(--accent); }
  .search-box::placeholder { color:var(--muted); }
  table { width:100%; border-collapse:collapse; }
  thead th { background:var(--surface2); padding:11px 16px; font-size:10px; font-weight:600; letter-spacing:1px; text-transform:uppercase; color:var(--muted); text-align:left; cursor:pointer; user-select:none; white-space:nowrap; transition:color .2s; }
  thead th:hover, thead th.sorted { color:var(--accent); }
  thead th.actions-col { cursor:default; }
  tbody tr { border-top:1px solid var(--border); transition:background .15s; }
  tbody tr:hover { background:var(--surface2); }
  tbody td { padding:11px 16px; font-size:13px; }
  td.name-cell { font-weight:500; }
  td.num-cell { text-align:center; font-weight:600; }
  td.val-cell { text-align:right; font-weight:600; color:var(--accent); }
  td.actions-cell { text-align:center; white-space:nowrap; }
  .bar-wrap { display:flex; align-items:center; gap:7px; }
  .bar-track { flex:1; height:5px; background:var(--border); border-radius:3px; overflow:hidden; min-width:50px; }
  .bar-fill { height:100%; border-radius:3px; background:var(--accent); }
  tfoot td { background:var(--surface2); border-top:2px solid var(--accent) !important; font-weight:700; font-size:13px; }
  tfoot .val-cell { color:var(--green); font-size:15px; }

  /* DOTS */
  .dot { display:inline-block; width:8px; height:8px; border-radius:50%; margin-right:6px; flex-shrink:0; }

  /* FILTERS */
  .filters { display:flex; gap:8px; margin-bottom:18px; flex-wrap:wrap; }
  .pill { padding:5px 14px; border-radius:20px; font-size:11px; font-weight:600; cursor:pointer; border:1px solid var(--border); background:var(--surface); color:var(--muted); transition:all .2s; letter-spacing:.5px; }
  .pill:hover,.pill.active { background:var(--accent); color:#000; border-color:var(--accent); }

  /* MODAL */
  .modal-overlay { display:none; position:fixed; inset:0; background:rgba(0,0,0,.7); z-index:1000; align-items:center; justify-content:center; backdrop-filter:blur(4px); }
  .modal-overlay.open { display:flex; }
  .modal { background:var(--surface); border:1px solid var(--border); border-radius:16px; padding:28px 32px; width:420px; max-width:95vw; animation:scaleIn .2s ease; }
  @keyframes scaleIn { from { transform:scale(.95); opacity:0; } to { transform:scale(1); opacity:1; } }
  .modal h2 { font-family:'Bebas Neue',sans-serif; font-size:24px; letter-spacing:2px; margin-bottom:22px; color:var(--accent); }
  .form-grid { display:grid; grid-template-columns:1fr 1fr; gap:14px; margin-bottom:22px; }
  .form-grid .full { grid-column:1/-1; }
  .form-group label { display:block; font-size:11px; font-weight:600; letter-spacing:1px; text-transform:uppercase; color:var(--muted); margin-bottom:6px; }
  .form-group input { width:100%; background:var(--surface2); border:1px solid var(--border); border-radius:8px; padding:9px 13px; color:var(--text); font-family:'DM Sans',sans-serif; font-size:14px; outline:none; transition:border-color .2s; }
  .form-group input:focus { border-color:var(--accent); }
  .modal-actions { display:flex; gap:10px; justify-content:flex-end; }
  .modal-del { margin-right:auto; }

  /* TOAST */
  .toast { position:fixed; bottom:28px; right:28px; background:var(--surface); border:1px solid var(--border); border-left:3px solid var(--green); border-radius:10px; padding:13px 20px; font-size:13px; font-weight:500; z-index:2000; transform:translateY(20px); opacity:0; transition:all .3s; pointer-events:none; }
  .toast.show { transform:translateY(0); opacity:1; }
  .toast.warn { border-left-color:var(--accent); }
  .toast.err { border-left-color:var(--red); }

  /* ANIMATIONS */
  @keyframes fadeUp { from { opacity:0; transform:translateY(14px); } to { opacity:1; transform:translateY(0); } }
  .kpi:nth-child(1){animation-delay:.05s} .kpi:nth-child(2){animation-delay:.1s} .kpi:nth-child(3){animation-delay:.15s} .kpi:nth-child(4){animation-delay:.2s}

  @media(max-width:900px){
    .kpis{grid-template-columns:repeat(2,1fr);}
    .charts-row{grid-template-columns:1fr;}
    .main{padding:14px;}
    .header{padding:12px 14px;}
  }
</style>
</head>
<body>

<div class="header">
  <div class="header-left">
    <div class="logo">🐴</div>
    <div>
      <h1>REGISTRO <span>CABALLOS</span></h1>
      <div class="header-sub">Panel de control — Participantes y pagos</div>
    </div>
  </div>
  <button class="btn btn-accent" onclick="openModal()">+ Agregar Participante</button>
</div>

<div class="main">

  <div class="kpis">
    <div class="kpi gold"><div class="kpi-label">Total Caballos</div><div class="kpi-value" id="kpi-total">—</div><div class="kpi-sub">Animales registrados</div><div class="kpi-icon">🐴</div></div>
    <div class="kpi orange"><div class="kpi-label">Participantes</div><div class="kpi-value" id="kpi-part">—</div><div class="kpi-sub">Propietarios activos</div><div class="kpi-icon">👤</div></div>
    <div class="kpi green"><div class="kpi-label">Valor Total</div><div class="kpi-value" id="kpi-valor">—</div><div class="kpi-sub">Pesos colombianos</div><div class="kpi-icon">💰</div></div>
    <div class="kpi blue"><div class="kpi-label">Promedio / Caballo</div><div class="kpi-value" id="kpi-avg">—</div><div class="kpi-sub">Por animal</div><div class="kpi-icon">📊</div></div>
  </div>

  <div class="filters">
    <div class="pill active" onclick="setFilter('all',this)">Todos</div>
    <div class="pill" onclick="setFilter('pesebrera',this)">🏠 Pesebrera</div>
    <div class="pill" onclick="setFilter('potrero',this)">🌿 Potrero</div>
    <div class="pill" onclick="setFilter('propio',this)">⭐ Propio</div>
    <div class="pill" onclick="setFilter('pagado',this)">💚 Con Pago</div>
  </div>

  <div class="charts-row">
    <div class="chart-card">
      <div class="section-title">Distribución por Tipo de Caballo</div>
      <canvas id="chartDonut" height="210"></canvas>
    </div>
    <div class="chart-card">
      <div class="section-title">Valor Pagado por Participante</div>
      <canvas id="chartBar" height="210"></canvas>
    </div>
  </div>

  <div class="table-card">
    <div class="table-toolbar">
      <div class="toolbar-left">
        <div class="section-title" style="margin:0">Detalle de Participantes</div>
        <input class="search-box" id="searchInput" type="text" placeholder="🔍 Buscar..." oninput="render()">
      </div>
      <button class="btn btn-ghost btn-sm" onclick="exportCSV()">⬇ Exportar CSV</button>
    </div>
    <table>
      <thead>
        <tr>
          <th onclick="sortBy('nombre')">Nombre</th>
          <th onclick="sortBy('cantidad')" style="text-align:center">Cantidad</th>
          <th onclick="sortBy('pesebrera')" style="text-align:center">Pesebrera</th>
          <th onclick="sortBy('potrero')" style="text-align:center">Potrero</th>
          <th onclick="sortBy('propio')" style="text-align:center">Propio</th>
          <th onclick="sortBy('valor')" style="text-align:right">Valor Pagado</th>
          <th style="text-align:left">Dist.</th>
          <th class="actions-col" style="text-align:center">Acciones</th>
        </tr>
      </thead>
      <tbody id="tableBody"></tbody>
      <tfoot id="tableFoot"></tfoot>
    </table>
  </div>
</div>

<!-- MODAL -->
<div class="modal-overlay" id="modalOverlay" onclick="closeOnBg(event)">
  <div class="modal">
    <h2 id="modalTitle">Nuevo Participante</h2>
    <div class="form-grid">
      <div class="form-group full">
        <label>Nombre</label>
        <input type="text" id="fNombre" placeholder="Ej: CARLOS MARIO">
      </div>
      <div class="form-group">
        <label>Cantidad Total</label>
        <input type="number" id="fCantidad" min="0" placeholder="0">
      </div>
      <div class="form-group">
        <label>Valor Pagado ($)</label>
        <input type="number" id="fValor" min="0" placeholder="0">
      </div>
      <div class="form-group">
        <label>Pesebrera</label>
        <input type="number" id="fPesebrera" min="0" placeholder="0">
      </div>
      <div class="form-group">
        <label>Potrero</label>
        <input type="number" id="fPotrero" min="0" placeholder="0">
      </div>
      <div class="form-group">
        <label>Propio</label>
        <input type="number" id="fPropio" min="0" placeholder="0">
      </div>
    </div>
    <div class="modal-actions">
      <button class="btn btn-red btn-sm modal-del" id="btnDelete" onclick="deleteRow()" style="display:none">🗑 Eliminar</button>
      <button class="btn btn-ghost" onclick="closeModal()">Cancelar</button>
      <button class="btn btn-green" onclick="saveRow()">Guardar</button>
    </div>
  </div>
</div>

<!-- TOAST -->
<div class="toast" id="toast"></div>

<script>
// ── DATA ──────────────────────────────────────────────
let DATA = [
  { id:1, nombre:"LORENA ORLAS",   cantidad:1, pesebrera:0, potrero:1, propio:0, valor:75000 },
  { id:2, nombre:"ANA MILENA",     cantidad:6, pesebrera:0, potrero:6, propio:0, valor:450000 },
  { id:3, nombre:"LEANDRO VARELA", cantidad:2, pesebrera:1, potrero:1, propio:0, valor:175000 },
  { id:4, nombre:"CAMI ACOSTA",    cantidad:1, pesebrera:0, potrero:1, propio:0, valor:75000 },
  { id:5, nombre:"JOHANA MEZA",    cantidad:1, pesebrera:0, potrero:1, propio:0, valor:75000 },
  { id:6, nombre:"ELIANA PEREZ",   cantidad:1, pesebrera:0, potrero:1, propio:0, valor:75000 },
  { id:7, nombre:"MONO",           cantidad:2, pesebrera:0, potrero:0, propio:2, valor:0 },
];
let nextId = 8;
let filterMode = 'all';
let sortKey = null;
let sortAsc = true;
let editingId = null;

const COLORS = ['#f0a500','#e05c2a','#3fb950','#58a6ff','#bc8cff','#ff7b72','#79c0ff','#ffa657','#d2a8ff'];
const fmt = v => v === 0 ? '$ -' : '$ ' + v.toLocaleString('es-CO');

// ── FILTER / SORT ─────────────────────────────────────
function getFiltered() {
  let d = [...DATA];
  if (filterMode==='pesebrera') d = d.filter(r=>r.pesebrera>0);
  else if (filterMode==='potrero') d = d.filter(r=>r.potrero>0);
  else if (filterMode==='propio') d = d.filter(r=>r.propio>0);
  else if (filterMode==='pagado') d = d.filter(r=>r.valor>0);
  const q = document.getElementById('searchInput').value.toLowerCase();
  if (q) d = d.filter(r=>r.nombre.toLowerCase().includes(q));
  if (sortKey) d.sort((a,b)=>{
    let va=a[sortKey], vb=b[sortKey];
    if(typeof va==='string') return sortAsc?va.localeCompare(vb):vb.localeCompare(va);
    return sortAsc?va-vb:vb-va;
  });
  return d;
}

function setFilter(type, el) {
  document.querySelectorAll('.pill').forEach(p=>p.classList.remove('active'));
  el.classList.add('active');
  filterMode = type;
  render();
}

function sortBy(key) {
  if (sortKey===key) sortAsc=!sortAsc;
  else { sortKey=key; sortAsc=true; }
  document.querySelectorAll('thead th').forEach(t=>t.classList.remove('sorted'));
  const keys=['nombre','cantidad','pesebrera','potrero','propio','valor'];
  const i = keys.indexOf(key);
  if(i>=0) document.querySelectorAll('thead th')[i].classList.add('sorted');
  render();
}

// ── RENDER ────────────────────────────────────────────
function render() {
  const rows = getFiltered();
  const maxCant = Math.max(...DATA.map(d=>d.cantidad), 1);
  const totalAll = DATA.reduce((s,d)=>s+d.cantidad,0);
  const body = document.getElementById('tableBody');
  const foot = document.getElementById('tableFoot');

  body.innerHTML = rows.length === 0
    ? '<tr><td colspan="8" style="text-align:center;padding:36px;color:var(--muted)">Sin resultados</td></tr>'
    : rows.map((d,i)=>{
      const color = COLORS[DATA.indexOf(d) % COLORS.length];
      const pct = totalAll > 0 ? ((d.cantidad/totalAll)*100).toFixed(0) : 0;
      return `<tr>
        <td class="name-cell"><span class="dot" style="background:${color}"></span>${d.nombre}</td>
        <td class="num-cell">${d.cantidad}</td>
        <td class="num-cell">${d.pesebrera||'-'}</td>
        <td class="num-cell">${d.potrero||'-'}</td>
        <td class="num-cell">${d.propio||'-'}</td>
        <td class="val-cell">${fmt(d.valor)}</td>
        <td><div class="bar-wrap"><div class="bar-track"><div class="bar-fill" style="width:${(d.cantidad/maxCant)*100}%"></div></div><span style="font-size:10px;color:var(--muted)">${pct}%</span></div></td>
        <td class="actions-cell">
          <button class="btn btn-ghost btn-sm" onclick="openModal(${d.id})" style="margin-right:4px">✏️</button>
        </td>
      </tr>`;
    }).join('');

  const totC = rows.reduce((s,d)=>s+d.cantidad,0);
  const totV = rows.reduce((s,d)=>s+d.valor,0);
  const totPes = rows.reduce((s,d)=>s+d.pesebrera,0);
  const totPot = rows.reduce((s,d)=>s+d.potrero,0);
  const totPro = rows.reduce((s,d)=>s+d.propio,0);
  foot.innerHTML = `<tr>
    <td style="font-weight:700">TOTAL (${rows.length})</td>
    <td class="num-cell">${totC}</td>
    <td class="num-cell">${totPes||'-'}</td>
    <td class="num-cell">${totPot}</td>
    <td class="num-cell">${totPro||'-'}</td>
    <td class="val-cell">${fmt(totV)}</td>
    <td></td><td></td>
  </tr>`;

  updateKPIs();
  updateCharts();
}

// ── KPIs ──────────────────────────────────────────────
function updateKPIs() {
  const rows = getFiltered();
  const total = rows.reduce((s,d)=>s+d.cantidad,0);
  const valor = rows.reduce((s,d)=>s+d.valor,0);
  const avg = total > 0 ? valor/total : 0;
  document.getElementById('kpi-total').textContent = total;
  document.getElementById('kpi-part').textContent = rows.length;
  document.getElementById('kpi-valor').textContent = '$'+(valor>=1000000?(valor/1000000).toFixed(1)+'M':(valor/1000).toFixed(0)+'K');
  document.getElementById('kpi-avg').textContent = '$'+Math.round(avg/1000)+'K';
}

// ── CHARTS ────────────────────────────────────────────
let donutChart, barChart;
function updateCharts() {
  const rows = getFiltered();
  const totPes = rows.reduce((s,d)=>s+d.pesebrera,0);
  const totPot = rows.reduce((s,d)=>s+d.potrero,0);
  const totPro = rows.reduce((s,d)=>s+d.propio,0);

  if (donutChart) {
    donutChart.data.datasets[0].data = [totPes, totPot, totPro];
    donutChart.update();
  } else {
    donutChart = new Chart(document.getElementById('chartDonut'), {
      type:'doughnut',
      data:{ labels:['Pesebrera','Potrero','Propio'], datasets:[{ data:[totPes,totPot,totPro], backgroundColor:['#f0a500','#e05c2a','#58a6ff'], borderColor:'#161b22', borderWidth:3, hoverOffset:8 }] },
      options:{ responsive:true, plugins:{ legend:{ position:'bottom', labels:{ color:'#7d8590', padding:14, font:{family:'DM Sans',size:11} } }, tooltip:{ callbacks:{ label:ctx=>` ${ctx.label}: ${ctx.parsed} caballos` } } }, cutout:'65%' }
    });
  }

  const labels = rows.map(d=>d.nombre.split(' ')[0]);
  const vals = rows.map(d=>d.valor);
  const bgColors = rows.map(d=>d.valor>0?'#f0a500':'#30363d');

  if (barChart) {
    barChart.data.labels = labels;
    barChart.data.datasets[0].data = vals;
    barChart.data.datasets[0].backgroundColor = bgColors;
    barChart.update();
  } else {
    barChart = new Chart(document.getElementById('chartBar'), {
      type:'bar',
      data:{ labels, datasets:[{ label:'Valor Pagado', data:vals, backgroundColor:bgColors, borderRadius:6, borderSkipped:false }] },
      options:{ responsive:true, plugins:{ legend:{display:false}, tooltip:{ callbacks:{ label:ctx=>` $ ${ctx.parsed.y.toLocaleString('es-CO')}` } } }, scales:{ x:{ ticks:{color:'#7d8590',font:{size:10}}, grid:{color:'#21262d'} }, y:{ ticks:{ color:'#7d8590',font:{size:10}, callback:v=>'$'+(v/1000)+'K' }, grid:{color:'#21262d'} } } }
    });
  }
}

// ── MODAL ─────────────────────────────────────────────
function openModal(id = null) {
  editingId = id;
  const m = document.getElementById('modalOverlay');
  const btnDel = document.getElementById('btnDelete');
  document.getElementById('modalTitle').textContent = id ? 'Editar Participante' : 'Nuevo Participante';
  btnDel.style.display = id ? 'block' : 'none';

  if (id) {
    const row = DATA.find(d=>d.id===id);
    if (!row) return;
    document.getElementById('fNombre').value = row.nombre;
    document.getElementById('fCantidad').value = row.cantidad;
    document.getElementById('fPesebrera').value = row.pesebrera;
    document.getElementById('fPotrero').value = row.potrero;
    document.getElementById('fPropio').value = row.propio;
    document.getElementById('fValor').value = row.valor;
  } else {
    ['fNombre','fCantidad','fPesebrera','fPotrero','fPropio','fValor'].forEach(id=>document.getElementById(id).value='');
  }
  m.classList.add('open');
  setTimeout(()=>document.getElementById('fNombre').focus(), 100);
}

function closeModal() {
  document.getElementById('modalOverlay').classList.remove('open');
  editingId = null;
}

function closeOnBg(e) {
  if (e.target === document.getElementById('modalOverlay')) closeModal();
}

function saveRow() {
  const nombre = document.getElementById('fNombre').value.trim().toUpperCase();
  if (!nombre) { showToast('El nombre es obligatorio','warn'); return; }
  const cantidad = parseInt(document.getElementById('fCantidad').value)||0;
  const pesebrera = parseInt(document.getElementById('fPesebrera').value)||0;
  const potrero = parseInt(document.getElementById('fPotrero').value)||0;
  const propio = parseInt(document.getElementById('fPropio').value)||0;
  const valor = parseFloat(document.getElementById('fValor').value)||0;

  if (editingId) {
    const i = DATA.findIndex(d=>d.id===editingId);
    DATA[i] = { id:editingId, nombre, cantidad, pesebrera, potrero, propio, valor };
    showToast('✅ Participante actualizado');
  } else {
    DATA.push({ id:nextId++, nombre, cantidad, pesebrera, potrero, propio, valor });
    showToast('✅ Participante agregado');
  }
  closeModal();
  render();
}

function deleteRow() {
  if (!editingId) return;
  const row = DATA.find(d=>d.id===editingId);
  if (!confirm(`¿Eliminar a ${row.nombre}?`)) return;
  DATA = DATA.filter(d=>d.id!==editingId);
  closeModal();
  render();
  showToast('🗑 Participante eliminado','warn');
}

// ── EXPORT CSV ────────────────────────────────────────
function exportCSV() {
  const header = 'Nombre,Cantidad,Pesebrera,Potrero,Propio,Valor Pagado';
  const rows = DATA.map(d=>`${d.nombre},${d.cantidad},${d.pesebrera},${d.potrero},${d.propio},${d.valor}`);
  const total = DATA.reduce((s,d)=>s+d.cantidad,0);
  const totalV = DATA.reduce((s,d)=>s+d.valor,0);
  rows.push(`TOTAL,${total},,,,${totalV}`);
  const blob = new Blob([[header,...rows].join('\n')], {type:'text/csv;charset=utf-8;'});
  const a = document.createElement('a'); a.href = URL.createObjectURL(blob); a.download = 'caballos.csv'; a.click();
  showToast('⬇ CSV exportado');
}

// ── TOAST ─────────────────────────────────────────────
let toastTimer;
function showToast(msg, type='') {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.className = 'toast show ' + type;
  clearTimeout(toastTimer);
  toastTimer = setTimeout(()=>t.classList.remove('show'), 2800);
}

// ── INIT ──────────────────────────────────────────────
document.addEventListener('keydown', e=>{ if(e.key==='Escape') closeModal(); });
render();
</script>
</body>
</html>
