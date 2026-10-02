# Mon-site-d-cole-
<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Carnet de notes</title>
<style>
/* ===== Variables : mode cahier (clair) et mode ardoise (sombre) ===== */
:root{
  --paper:#F2F6FB; --grid:#D5E1F0; --sheet:#FFFFFF; --ink:#1B3A8C; --red:#C0392B; --green:#1F7A57;
  --text:#262B36; --mute:#5C6677; --rule:#B8C6DC; --hi:#FFE680; --hi-text:#1B3A8C; --soft:#FFFBE0;
  --bad-bg:#FDECEA; --ok-bg:#E7F6EE; --hover:#F7FAFF;
  --serif:"Palatino Linotype",Palatino,"Book Antiqua",Georgia,serif;
  --sans:system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;
  color-scheme:light;
}
:root[data-theme="dark"]{
  --paper:#0F1E27; --grid:#183039; --sheet:#162833; --ink:#A8C8FF; --red:#FF9082; --green:#72D9A9;
  --text:#E9EFF3; --mute:#9DB1BE; --rule:#2F4D5E; --hi:#6A5A10; --hi-text:#FFF3B8; --soft:#22343A;
  --bad-bg:#3A2224; --ok-bg:#16352B; --hover:#1C3341;
  color-scheme:dark;
}

/* ===== Base ===== */
*{box-sizing:border-box}
html{-webkit-text-size-adjust:100%}
body{
  margin:0; color:var(--text); line-height:1.45; font-family:var(--sans);
  background-color:var(--paper);
  background-image:linear-gradient(var(--grid) 1px,transparent 1px),linear-gradient(90deg,var(--grid) 1px,transparent 1px);
  background-size:24px 24px;
  transition:background-color .3s;
}
#app{
  position:relative; max-width:800px; margin:0 auto; min-height:100vh;
  background:var(--sheet); padding:20px 16px 48px 34px;
  border-left:1px solid var(--rule); border-right:1px solid var(--rule);
  transition:background-color .3s;
}
#app::before{content:""; position:absolute; top:0; bottom:0; left:18px; width:2px; background:var(--red); opacity:.55}
h1,h2,h3{font-family:var(--serif); margin:0; color:var(--ink)}
h1{font-size:2rem; line-height:1.1}
h2{font-size:1.4rem; margin:22px 0 8px}
h3{font-size:1.05rem; margin:20px 0 6px}
button,input,select,textarea{font:inherit; color:inherit}
:focus-visible{outline:3px solid var(--ink); outline-offset:2px}
.hint{color:var(--mute); font-size:.9rem}
.err{color:var(--red); min-height:1.2em; margin:4px 0}
.sub-coef{color:var(--mute); font-size:.8rem; font-weight:400; font-family:var(--sans)}

/* ===== En-tête ===== */
.top{display:flex; justify-content:space-between; align-items:flex-start; gap:10px}
.school{margin:4px 0 12px; color:var(--mute)}
.print-head{display:none}

/* ===== Boutons et champs ===== */
.tabs,.seg,.chips{display:flex; gap:6px; flex-wrap:wrap; margin:10px 0}
.tabs button,.seg button,.chips button{
  border:1.5px solid var(--ink); background:var(--sheet); color:var(--ink);
  padding:8px 14px; border-radius:6px; cursor:pointer; min-height:40px;
  transition:background-color .15s, transform .1s;
}
.tabs button:hover,.seg button:hover,.chips button:hover{background:var(--hover)}
.tabs button:active,.seg button:active,.chips button:active{transform:scale(.97)}
.tabs button.on,.seg button.on,.chips button.on{background:var(--hi); color:var(--hi-text); font-weight:700}

.btn{
  border:1.5px solid var(--ink); background:var(--ink); color:var(--sheet);
  padding:9px 16px; border-radius:6px; cursor:pointer; min-height:42px;
  transition:opacity .15s, transform .1s;
}
.btn:hover{opacity:.88}
.btn:active{transform:scale(.97)}
.btn.alt{background:var(--sheet); color:var(--ink)}
.btn.danger{background:var(--sheet); color:var(--red); border-color:var(--red)}
.btn.small{padding:4px 10px; min-height:32px; font-size:.9rem}
.row{display:flex; gap:8px; flex-wrap:wrap; align-items:center; margin:8px 0}
.top-actions{display:flex; justify-content:space-between; align-items:center; flex-wrap:wrap; gap:8px}
input[type=text],input[type=password],input[type=number],select,textarea{
  border:1px solid var(--rule); border-radius:5px; padding:9px 10px; background:var(--sheet); font-size:16px; max-width:100%;
  transition:border-color .15s;
}
input:focus,select:focus{border-color:var(--ink)}
label{display:block; margin:12px 0 4px; font-weight:600}
.full{width:100%}

/* ===== Page d'accueil ===== */
.door{
  display:block; width:100%; text-align:left; margin:12px 0; padding:18px 16px; cursor:pointer;
  background:var(--sheet); border:2px solid var(--ink); border-radius:8px; color:var(--text);
  transition:background-color .15s, transform .15s;
}
.door:hover{background:var(--soft); transform:translateX(4px)}
.door .t{display:block; font-family:var(--serif); font-size:1.3rem; color:var(--ink); margin-bottom:4px}

/* ===== Statistiques ===== */
.stats{display:grid; grid-template-columns:repeat(auto-fit,minmax(130px,1fr)); gap:10px; margin:12px 0}
.stat{border:1.5px solid var(--rule); border-radius:8px; padding:10px 12px; background:var(--sheet)}
.stat b{display:block; font-family:var(--serif); font-size:1.6rem; color:var(--ink); line-height:1.2}
.stat span{color:var(--mute); font-size:.85rem}

/* ===== Tableaux de notes ===== */
.ledger{width:100%; border-collapse:collapse; margin:8px 0; font-size:.95rem}
.ledger th{text-align:left; font-weight:600; color:var(--mute); padding:6px 4px; border-bottom:2px solid var(--ink); font-size:.85rem}
.ledger td{padding:8px 4px; border-bottom:1px solid var(--grid); vertical-align:middle}
.ledger tbody tr:nth-child(even){background:var(--hover)}
.ledger .n{text-align:center; white-space:nowrap}
.ledger th.n{text-align:center}
.mark{font-family:var(--serif); font-style:italic; font-size:1.05rem}
.avg{
  display:inline-block; min-width:2.9em; text-align:center; padding:1px 7px; font-weight:700;
  border:2px solid currentColor; border-radius:50%/60%; font-family:var(--serif);
}
.avg.ok{color:var(--green)} .avg.ko{color:var(--red)} .avg.none{color:var(--mute); border-color:var(--grid); font-weight:400}
input.g{width:62px; text-align:center; padding:8px 4px}
input.g.bad{border-color:var(--red); background:var(--bad-bg)}

/* ===== Listes d'élèves ===== */
.student{
  display:flex; width:100%; align-items:center; gap:10px; text-align:left;
  background:var(--sheet); border:0; border-bottom:1px solid var(--grid); padding:12px 4px; cursor:pointer; min-height:48px;
  transition:background-color .15s, padding-left .15s;
}
.student:hover{background:var(--hover); padding-left:10px}
.student .name{flex:1; font-weight:600}
.student .rk{color:var(--mute); font-size:.9rem; min-width:3.2em; text-align:right}
.class-line{margin:2px 0 6px}

.summary{display:flex; gap:18px; flex-wrap:wrap; align-items:center; margin:16px 0; padding:12px; background:var(--soft); border:1px dashed var(--rule)}
.summary b{font-family:var(--serif)}
.empty{padding:16px; border:1px dashed var(--rule); color:var(--mute); margin:12px 0}
.list-item{display:flex; justify-content:space-between; align-items:center; gap:8px; padding:6px 0; border-bottom:1px solid var(--grid)}
.status{color:var(--mute); font-size:.85rem; margin-top:24px; min-height:1.2em}

/* ===== Graphique de comparaison ===== */
.bars{margin:8px 0}
.bar-row{margin:12px 0}
.bar-name{display:block; font-weight:600; font-size:.9rem; margin-bottom:2px}
.bar-line{display:flex; align-items:center; gap:8px; margin:3px 0}
.bar-line b{width:3.2em; text-align:right; font-size:.85rem; font-family:var(--serif)}
.track{position:relative; flex:1; height:12px; background:var(--grid); border-radius:3px; overflow:hidden}
.track::after{content:""; position:absolute; left:50%; top:0; bottom:0; border-left:2px dashed var(--red); opacity:.6}
.track i{display:block; height:100%; width:var(--w); border-radius:3px; animation:grow .7s ease-out both}
.track.me i{background:var(--ink)}
.track.cl i{background:var(--mute); opacity:.55}
.legend{display:flex; gap:14px; flex-wrap:wrap; align-items:center; color:var(--mute); font-size:.85rem}
.sw{display:inline-block; width:14px; height:10px; border-radius:2px; margin-right:4px; vertical-align:middle}
.sw.me{background:var(--ink)} .sw.cl{background:var(--mute); opacity:.55}
.sw.line{background:none; border-top:2px dashed var(--red); height:0; width:16px}

/* ===== Notifications ===== */
#toasts{position:fixed; left:0; right:0; bottom:16px; display:flex; flex-direction:column; align-items:center; gap:8px; pointer-events:none; z-index:50}
.toast{
  background:var(--ink); color:var(--sheet); padding:10px 16px; border-radius:6px; max-width:90vw;
  box-shadow:0 2px 10px rgba(0,0,0,.25); animation:pop .25s ease-out both;
}
.toast.ok{background:var(--green); color:#fff}
.toast.err{background:var(--red); color:#fff}
.toast.out{opacity:0; transition:opacity .4s}

/* ===== Animations (une seule entrée de page, et le graphique) ===== */
@keyframes enter{from{opacity:0; transform:translateY(8px)} to{opacity:1; transform:none}}
@keyframes grow{from{width:0}}
@keyframes pop{from{opacity:0; transform:translateY(10px)} to{opacity:1; transform:none}}
main.enter{animation:enter .28s ease-out}
@media (prefers-reduced-motion:reduce){
  *,*::before,*::after{animation:none !important; transition:none !important}
}

/* ===== Mobile ===== */
@media (max-width:480px){
  #app{padding-left:30px; padding-right:10px}
  #app::before{left:14px}
  h1{font-size:1.7rem}
  .ledger{font-size:.88rem}
  input.g{width:54px}
}

/* ===== Impression du bulletin ===== */
@media print{
  body{background:#fff; color:#000}
  #app{max-width:none; border:0; padding:0; background:#fff}
  #app::before,.tabs,.theme,.no-print,.status,#toasts,.seg button:not(.on){display:none !important}
  .print-head{display:block; margin-bottom:12px}
  .print-head h2{margin:0}
  .top{display:none}
  .track i{animation:none}
  .ledger tbody tr:nth-child(even){background:none}
}

</style>
</head>
<body>
<div id="app"><p>Chargement…</p></div>
<div id="toasts" role="status" aria-live="polite"></div>
<script>
(() => {
'use strict';

/* =====================================================================
   1. STOCKAGE
   Trois modes, choisis automatiquement :
   - "shared" : stockage partagé de l'aperçu Claude
   - "local"  : localStorage du navigateur (données sur CET appareil)
   - "memory" : mémoire temporaire si rien d'autre n'est disponible
   Pour brancher un vrai serveur, remplacez load() et save() par des fetch().
   ===================================================================== */
const Store = (() => {
  const KEY = 'school-data';
  let mode = 'memory';
  const mem = {};
  if (window.storage && typeof window.storage.get === 'function') mode = 'shared';
  else {
    try { localStorage.setItem('__t', '1'); localStorage.removeItem('__t'); mode = 'local'; } catch (e) {}
  }
  return {
    mode,
    async load() {
      try {
        if (mode === 'shared') { const r = await window.storage.get(KEY, true); return r ? JSON.parse(r.value) : null; }
        if (mode === 'local') { const s = localStorage.getItem(KEY); return s ? JSON.parse(s) : null; }
        return mem[KEY] || null;
      } catch (e) { return null; }
    },
    async save(d) {
      try {
        if (mode === 'shared') return !!(await window.storage.set(KEY, JSON.stringify(d), true));
        if (mode === 'local') { localStorage.setItem(KEY, JSON.stringify(d)); return true; }
        mem[KEY] = d; return true;
      } catch (e) { return false; }
    }
  };
})();

const Pref = {
  get() { try { return localStorage.getItem('school-theme'); } catch (e) { return null; } },
  set(v) { try { localStorage.setItem('school-theme', v); } catch (e) {} }
};

/* =====================================================================
   2. OUTILS
   ===================================================================== */
const $ = s => document.querySelector(s);
const uid = () => Math.random().toString(36).slice(2, 9);
const esc = s => String(s).replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
const TERMS = [1, 2, 3];

function toast(msg, kind) {
  const box = $('#toasts');
  const el = document.createElement('div');
  el.className = 'toast ' + (kind || '');
  el.textContent = msg;
  box.appendChild(el);
  setTimeout(() => el.classList.add('out'), 2600);
  setTimeout(() => el.remove(), 3100);
}

function applyTheme(t) {
  document.documentElement.dataset.theme = t;
  Pref.set(t);
}

/* =====================================================================
   3. DONNÉES ET CALCULS
   ===================================================================== */
let data = null;
let route = '/';
let lastRoute = null;
let draft = null;
let lastClass = '';
const view = {term: 1, classId: 'all', q: '', sort: 'alpha', dq: '', tClass: null, tSubject: null, unlocked: false, status: ''};

function emptyData(pin, schoolName) {
  return {schoolName: schoolName || 'Nom de l’établissement', pin: pin || '1234', classes: [], subjects: [], students: [], grades: {}};
}

function seed() {
  const d = emptyData('1234', 'Collège Les Palmiers, Bingerville');
  d.subjects = [['Français', 3], ['Mathématiques', 3], ['Anglais', 2], ['Histoire-Géographie', 2], ['SVT', 2], ['EPS', 1]]
    .map(([name, coef]) => ({id: uid(), name, coef}));
  const groups = {
    '6ème A': ['Koffi Aya', 'Traoré Ibrahim', 'Yao Marie-Ange', 'Bamba Séraphin', 'N’Guessan Lydie'],
    '5ème B': ['Kouadio Jean', 'Diallo Fatou', 'Konan Éric', 'Coulibaly Awa'],
    'Tle D': ['Ouattara Moussa', 'Assi Grâce', 'Kouamé Sylvie']
  };
  Object.entries(groups).forEach(([cname, names]) => {
    const c = {id: uid(), name: cname};
    d.classes.push(c);
    names.forEach(n => {
      const s = {id: uid(), classId: c.id, name: n};
      d.students.push(s);
      const level = 8 + Math.random() * 7;
      d.subjects.forEach(sub => {
        d.grades[`${s.id}|${sub.id}|1`] = [0, 1, 2].map(() =>
          Math.round(Math.max(3, Math.min(20, level + (Math.random() * 8 - 4))) * 2) / 2);
      });
    });
  });
  return d;
}

const mean = a => {
  const v = a.filter(x => x !== null && x !== undefined && !isNaN(x));
  return v.length ? v.reduce((s, x) => s + x, 0) / v.length : null;
};
const getG = (sid, sub, t) => data.grades[`${sid}|${sub}|${t}`] || [null, null, null];
const subAvg = (sid, sub, t) => mean(getG(sid, sub, t));
function genAvg(sid, t) {
  let n = 0, d = 0;
  data.subjects.forEach(s => { const m = subAvg(sid, s.id, t); if (m !== null) { n += m * s.coef; d += s.coef; } });
  return d ? n / d : null;
}
const classStudents = cid => data.students.filter(s => s.classId === cid);
const classSubAvg = (cid, sub, t) => mean(classStudents(cid).map(s => subAvg(s.id, sub, t)));
const classGenAvg = (cid, t) => mean(classStudents(cid).map(s => genAvg(s.id, t)));
const className = id => (data.classes.find(c => c.id === id) || {name: ''}).name;

function rank(st, t) {
  const mates = classStudents(st.classId).map(s => ({id: s.id, a: genAvg(s.id, t)}))
    .filter(x => x.a !== null).sort((a, b) => b.a - a.a);
  const i = mates.findIndex(x => x.id === st.id);
  return i < 0 ? null : {r: i + 1, n: mates.length};
}
function stats(students, t) {
  const avgs = students.map(s => ({s, a: genAvg(s.id, t)})).filter(x => x.a !== null);
  if (!avgs.length) return {n: 0, avg: null, pass: null, best: null};
  const best = avgs.reduce((m, x) => x.a > m.a ? x : m);
  return {
    n: avgs.length,
    avg: mean(avgs.map(x => x.a)),
    pass: Math.round(avgs.filter(x => x.a >= 10).length / avgs.length * 100),
    best
  };
}
const loadGrades = (sid, t) => {
  const o = {};
  data.subjects.forEach(s => { o[s.id] = getG(sid, s.id, t).slice(); });
  return o;
};

const rkLabel = r => r ? `${r.r === 1 ? '1er' : r.r + 'e'}/${r.n}` : '';
const fmt = x => x === null ? '–' : (Math.round(x * 100) / 100).toFixed(2).replace('.', ',');
const fmtMark = x => (x === null || x === undefined) ? '–' : String(Math.round(x * 100) / 100).replace('.', ',');
const appr = a => a === null ? '' : a >= 16 ? 'Excellent' : a >= 14 ? 'Très bien' : a >= 12 ? 'Bien' : a >= 10 ? 'Passable' : a >= 8 ? 'Insuffisant' : 'Faible';
const badge = x => x === null ? '<span class="avg none">–</span>' : `<span class="avg ${x >= 10 ? 'ok' : 'ko'}">${fmt(x)}</span>`;

/* ---------- Sauvegarde ---------- */
let saveTimer;
function setStatus() { const el = $('#status'); if (el) el.textContent = view.status; }
function scheduleSave() {
  view.status = 'Enregistrement…'; setStatus();
  clearTimeout(saveTimer);
  saveTimer = setTimeout(async () => {
    const ok = await Store.save(data);
    view.status = ok ? 'Enregistré' : 'Échec de l’enregistrement';
    if (!ok) toast('Les données n’ont pas pu être enregistrées.', 'err');
    setStatus();
  }, 500);
}

/* =====================================================================
   4. NAVIGATION
   ===================================================================== */
function navigate(p) {
  route = p;
  try { if (location.hash !== '#' + p) location.hash = p; } catch (e) {}
  render();
  window.scrollTo(0, 0);
}
window.addEventListener('hashchange', () => {
  let h = '/';
  try { h = location.hash.slice(1) || '/'; } catch (e) {}
  if (h !== route) { route = h; render(); }
});

const termBar = () => `<div class="seg" role="group" aria-label="Trimestre">${
  TERMS.map(t => `<button data-a="term" data-v="${t}" class="${view.term === t ? 'on' : ''}">Trimestre ${t}</button>`).join('')
}</div>`;

function render() {
  const app = $('#app');
  if (!data) { app.innerHTML = '<p>Chargement…</p>'; return; }
  const inP = route.startsWith('/parents'), inT = route.startsWith('/prof');
  const body = inP ? (route.startsWith('/parents/eleve/') ? bulletinPage() : parentsPage()) : inT ? profPage() : homePage();
  const animate = route !== lastRoute;
  lastRoute = route;
  const dark = document.documentElement.dataset.theme === 'dark';
  app.innerHTML = `
    <div class="top">
      <header>
        <h1>Carnet de notes</h1>
        <p class="school">${esc(data.schoolName)}</p>
      </header>
      <button class="btn alt small theme" data-a="theme">${dark ? 'Mode cahier' : 'Mode ardoise'}</button>
    </div>
    <nav class="tabs" aria-label="Pages du site">
      <button data-go="/" class="${!inP && !inT ? 'on' : ''}">Accueil</button>
      <button data-go="/parents" class="${inP ? 'on' : ''}">Parents</button>
      <button data-go="/prof" class="${inT ? 'on' : ''}">Professeurs</button>
    </nav>
    <main class="${animate ? 'enter' : ''}">${body}</main>
    <p id="status" class="status" aria-live="polite">${esc(view.status)}</p>`;
}

/* =====================================================================
   5. PAGES
   ===================================================================== */
function homePage() {
  return `
    <h2>Bienvenue</h2>
    <button class="door" data-go="/parents">
      <span class="t">Espace parents</span>
      Voir les notes et les moyennes des élèves de toutes les classes.
    </button>
    <button class="door" data-go="/prof">
      <span class="t">Espace professeurs</span>
      Ajouter un élève avec sa classe, saisir ses notes et suivre les moyennes.
    </button>`;
}

/* ---------- Parents ---------- */
function parentsPage() {
  return `
    <div class="top-actions">
      <h2 style="margin:0">Notes des élèves</h2>
      <button class="btn alt small" data-a="refresh">Actualiser</button>
    </div>
    ${termBar()}
    <div class="chips" role="group" aria-label="Classe">
      <button data-a="cls" data-v="all" class="${view.classId === 'all' ? 'on' : ''}">Toutes les classes</button>
      ${data.classes.map(c => `<button data-a="cls" data-v="${c.id}" class="${view.classId === c.id ? 'on' : ''}">${esc(c.name)}</button>`).join('')}
    </div>
    <div class="row">
      <input type="text" class="full" data-f="q" placeholder="Chercher un élève" value="${esc(view.q)}" aria-label="Chercher un élève" style="flex:1; min-width:180px">
      <select data-f="sort" aria-label="Trier">
        <option value="alpha" ${view.sort === 'alpha' ? 'selected' : ''}>Ordre alphabétique</option>
        <option value="rank" ${view.sort === 'rank' ? 'selected' : ''}>Du premier au dernier</option>
      </select>
    </div>
    <div id="plist">${parentList()}</div>`;
}

function sortStudents(list, t) {
  const l = list.slice();
  if (view.sort === 'rank') {
    l.sort((a, b) => (genAvg(b.id, t) ?? -1) - (genAvg(a.id, t) ?? -1));
  } else {
    l.sort((a, b) => a.name.localeCompare(b.name, 'fr'));
  }
  return l;
}

function parentList() {
  const q = view.q.trim().toLowerCase();
  let html = '';
  data.classes.filter(c => view.classId === 'all' || c.id === view.classId).forEach(c => {
    const sts = sortStudents(classStudents(c.id).filter(s => !q || s.name.toLowerCase().includes(q)), view.term);
    if (!sts.length) return;
    html += `<h3>${esc(c.name)}</h3>` + sts.map(s => `
      <button class="student" data-go="/parents/eleve/${s.id}">
        <span class="name">${esc(s.name)}</span>
        <span class="rk">${rkLabel(rank(s, view.term))}</span>
        ${badge(genAvg(s.id, view.term))}
      </button>`).join('');
  });
  return html || '<div class="empty">Aucun élève trouvé. Les professeurs n’ont peut-être pas encore ajouté d’élèves.</div>';
}

function chartHTML(st, t) {
  const w = x => x === null ? 0 : Math.round(x / 20 * 100);
  const rows = data.subjects.map(sub => {
    const m = subAvg(st.id, sub.id, t), c = classSubAvg(st.classId, sub.id, t);
    if (m === null) return '';
    return `<div class="bar-row">
      <span class="bar-name">${esc(sub.name)}</span>
      <div class="bar-line"><span class="track me"><i style="--w:${w(m)}%"></i></span><b>${fmt(m)}</b></div>
      <div class="bar-line"><span class="track cl"><i style="--w:${w(c)}%"></i></span><b>${fmt(c)}</b></div>
    </div>`;
  }).join('');
  if (!rows) return '';
  return `<h3>Comparaison avec la classe</h3>
    <div class="bars">${rows}</div>
    <p class="legend"><span><i class="sw me"></i>Élève</span><span><i class="sw cl"></i>Classe</span><span><i class="sw line"></i>Moyenne de 10</span></p>`;
}

function bulletinPage() {
  const st = data.students.find(s => s.id === route.split('/').pop());
  if (!st) return '<div class="empty">Cet élève n’existe plus.</div><button class="btn alt" data-go="/parents">Retour à la liste</button>';
  const t = view.term, a = genAvg(st.id, t);
  const rows = data.subjects.map(sub => {
    const g = getG(st.id, sub.id, t);
    return `<tr>
      <td>${esc(sub.name)} <span class="sub-coef">coef. ${sub.coef}</span></td>
      ${g.map(x => `<td class="n mark">${fmtMark(x)}</td>`).join('')}
      <td class="n">${badge(subAvg(st.id, sub.id, t))}</td>
      <td class="n mark">${fmt(classSubAvg(st.classId, sub.id, t))}</td>
    </tr>`;
  }).join('');
  return `
    <div class="top-actions no-print">
      <button class="btn alt small" data-go="/parents">Retour à la liste</button>
      <button class="btn small" data-a="print">Imprimer le bulletin</button>
    </div>
    <div class="print-head"><h2>${esc(data.schoolName)}</h2><p>Bulletin de notes, trimestre ${t}</p></div>
    <h2>${esc(st.name)}</h2>
    <p class="hint" style="margin-top:0">${esc(className(st.classId))}</p>
    <div class="no-print">${termBar()}</div>
    ${data.subjects.length ? `<table class="ledger">
      <thead><tr><th>Matière</th><th class="n">Dev. 1</th><th class="n">Dev. 2</th><th class="n">Compo.</th><th class="n">Moy.</th><th class="n">Classe</th></tr></thead>
      <tbody>${rows}</tbody></table>` : '<div class="empty">Aucune matière enregistrée.</div>'}
    <div class="summary">
      <span>Moyenne générale ${badge(a)}</span>
      <span>Rang <b>${rkLabel(rank(st, t)) || '–'}</b></span>
      <span>Appréciation <b>${appr(a) || '–'}</b></span>
    </div>
    ${chartHTML(st, t)}`;
}

/* ---------- Professeurs ---------- */
function profPage() {
  if (!view.unlocked) return loginPage();
  if (route === '/prof/notes') return notesPage();
  if (route === '/prof/reglages') return reglagesPage();
  if (route.startsWith('/prof/eleve/')) {
    if (!draft || draft.key !== route) initDraft();
    if (draft) return formPage();
  }
  return dashPage();
}

function loginPage() {
  return `<h2>Espace des professeurs</h2>
    <p>Entrez le code pour saisir les notes.</p>
    <div class="row">
      <input type="password" id="pin" inputmode="numeric" placeholder="Code" autocomplete="off" aria-label="Code">
      <button class="btn" data-a="unlock">Entrer</button>
    </div>
    <p id="pinerr" class="err" aria-live="polite"></p>
    ${data.pin === '1234' ? '<p class="hint">Code par défaut : 1234. Changez-le dans Réglages.</p>' : ''}`;
}

function dashPage() {
  const t = view.term;
  const all = stats(data.students, t);
  return `
    <h2>Tableau de bord</h2>
    <div class="row">
      <button class="btn" data-go="/prof/eleve/nouveau">Ajouter un élève</button>
      <button class="btn alt" data-go="/prof/notes">Notes par classe</button>
      <button class="btn alt" data-go="/prof/reglages">Réglages</button>
    </div>
    ${termBar()}
    <div class="stats">
      <div class="stat"><b>${data.students.length}</b><span>élèves</span></div>
      <div class="stat"><b>${data.classes.length}</b><span>classes</span></div>
      <div class="stat"><b>${fmt(all.avg)}</b><span>moyenne de l’établissement</span></div>
      <div class="stat"><b>${all.pass === null ? '–' : all.pass + ' %'}</b><span>élèves à 10 ou plus</span></div>
    </div>
    <div class="row">
      <input type="text" data-f="dq" placeholder="Chercher un élève" value="${esc(view.dq)}" aria-label="Chercher un élève" style="flex:1; min-width:180px">
      <button class="btn alt small" data-a="csv">Exporter en CSV</button>
    </div>
    <div id="dlist">${dashList()}</div>`;
}

function dashList() {
  const t = view.term, q = view.dq.trim().toLowerCase();
  if (!data.classes.length) return '<div class="empty">Aucun élève pour l’instant. Ajoutez le premier élève avec sa classe et ses notes.</div>';
  const blocks = data.classes.map(c => {
    const all = classStudents(c.id);
    const sts = sortStudents(all.filter(s => !q || s.name.toLowerCase().includes(q)), t);
    if (q && !sts.length) return '';
    const st = stats(all, t);
    return `<h3>${esc(c.name)} <span class="sub-coef">${all.length} élève${all.length > 1 ? 's' : ''}</span> ${badge(classGenAvg(c.id, t))}</h3>
      ${st.n ? `<p class="hint class-line">Réussite : ${st.pass} %. Premier : ${esc(st.best.s.name)} (${fmt(st.best.a)}).</p>` : ''}` +
      (sts.map(s => `<button class="student" data-go="/prof/eleve/${s.id}">
        <span class="name">${esc(s.name)}</span>
        <span class="rk">${rkLabel(rank(s, t))}</span>
        ${badge(genAvg(s.id, t))}
      </button>`).join('') || '<p class="hint">Aucun élève dans cette classe.</p>');
  }).join('');
  return `<p class="hint">Touchez un élève pour modifier ses notes.</p>` + (blocks || '<div class="empty">Aucun élève trouvé.</div>');
}

/* ---------- Formulaire élève ---------- */
function initDraft() {
  const id = route.split('/').pop();
  if (id === 'nouveau') {
    draft = {key: route, id: null, name: '', cls: lastClass, term: view.term, grades: {}};
    return;
  }
  const st = data.students.find(s => s.id === id);
  if (!st) { draft = null; return; }
  draft = {key: route, id: st.id, name: st.name, cls: className(st.classId), term: view.term, grades: loadGrades(st.id, view.term)};
}

function formGen() {
  let n = 0, d = 0;
  data.subjects.forEach(s => { const m = mean(draft.grades[s.id] || []); if (m !== null) { n += m * s.coef; d += s.coef; } });
  return d ? n / d : null;
}

function formPage() {
  const edit = !!draft.id;
  const rows = data.subjects.map(s => {
    const g = draft.grades[s.id] || [null, null, null];
    return `<tr>
      <td>${esc(s.name)} <span class="sub-coef">coef. ${s.coef}</span></td>
      ${[0, 1, 2].map(i => `<td class="n"><input class="g" type="number" inputmode="decimal" min="0" max="20" step="0.25"
        data-f="dgrade" data-sub="${s.id}" data-i="${i}" value="${g[i] === null || g[i] === undefined ? '' : g[i]}"
        aria-label="${esc(s.name)} ${['devoir 1', 'devoir 2', 'composition'][i]}"></td>`).join('')}
      <td class="n" id="fa-${s.id}">${badge(mean(g))}</td>
    </tr>`;
  }).join('');
  return `
    <button class="btn alt small" data-go="/prof">Retour au tableau de bord</button>
    <h2>${edit ? 'Modifier l’élève' : 'Ajouter un élève'}</h2>
    <label for="dname">Nom de l’élève</label>
    <input type="text" id="dname" class="full" data-f="dname" value="${esc(draft.name)}" autocomplete="off" placeholder="Ex. : Koffi Aya">
    <label for="dcls">Classe</label>
    <input type="text" id="dcls" class="full" data-f="dcls" list="classlist" value="${esc(draft.cls)}" autocomplete="off" placeholder="Ex. : 6ème A">
    <datalist id="classlist">${data.classes.map(c => `<option value="${esc(c.name)}"></option>`).join('')}</datalist>
    <p class="hint">Choisissez une classe existante ou tapez un nouveau nom : la classe sera créée.</p>
    <h3>Notes du trimestre</h3>
    ${termBar()}
    ${data.subjects.length ? `<table class="ledger">
      <thead><tr><th>Matière</th><th class="n">Dev. 1</th><th class="n">Dev. 2</th><th class="n">Compo.</th><th class="n">Moy.</th></tr></thead>
      <tbody>${rows}</tbody></table>
      <div class="summary"><span>Moyenne générale <span id="fgen">${badge(formGen())}</span></span></div>
      <p class="hint">Astuce : la touche Entrée passe à la note suivante.</p>`
      : `<div class="empty">Aucune matière n’est enregistrée. Ajoutez-en dans <button class="btn alt small" data-go="/prof/reglages">Réglages</button> avant de saisir des notes.</div>`}
    <p id="formerr" class="err" aria-live="polite"></p>
    <div class="row">
      <button class="btn" data-a="savedraft">Enregistrer l’élève</button>
      ${edit ? `<button class="btn danger" data-a="del" data-t="student" data-id="${draft.id}">Supprimer l’élève</button>` : ''}
    </div>`;
}

function updateFormAvgs() {
  data.subjects.forEach(s => {
    const el = $('#fa-' + s.id);
    if (el) el.innerHTML = badge(mean(draft.grades[s.id] || []));
  });
  const g = $('#fgen');
  if (g) g.innerHTML = badge(formGen());
}
function formError(msg) { const el = $('#formerr'); if (el) el.textContent = msg; }

function saveDraft() {
  const name = draft.name.trim(), cls = draft.cls.trim();
  if (!name || !cls) { formError('Saisissez le nom de l’élève et sa classe.'); return; }
  if (document.querySelector('input.g.bad')) { formError('Corrigez les notes en rouge : elles doivent être comprises entre 0 et 20.'); return; }
  let c = data.classes.find(x => x.name.toLowerCase() === cls.toLowerCase());
  if (!draft.id && c && classStudents(c.id).some(s => s.name.toLowerCase() === name.toLowerCase())) {
    formError('Cet élève existe déjà dans cette classe. Ouvrez sa fiche depuis le tableau de bord pour modifier ses notes.');
    return;
  }
  if (!c) { c = {id: uid(), name: cls}; data.classes.push(c); }
  let st = draft.id ? data.students.find(s => s.id === draft.id) : null;
  if (st) { st.name = name; st.classId = c.id; }
  else { st = {id: uid(), classId: c.id, name}; data.students.push(st); }
  data.subjects.forEach(sub => {
    const arr = draft.grades[sub.id] || [null, null, null];
    const k = `${st.id}|${sub.id}|${draft.term}`;
    if (arr.every(x => x === null || x === undefined)) delete data.grades[k];
    else data.grades[k] = arr.slice();
  });
  const wasNew = !draft.id;
  lastClass = c.name;
  view.term = draft.term;
  draft = null;
  scheduleSave();
  if (wasNew) {
    toast(`${name} est enregistré(e) en ${c.name}.`, 'ok');
    render(); window.scrollTo(0, 0);
    const first = $('#dname'); if (first) first.focus();
  } else {
    toast(`Les notes de ${name} sont enregistrées.`, 'ok');
    navigate('/prof');
  }
}

/* ---------- Notes par classe ---------- */
function ensureSel() {
  if (!data.classes.find(c => c.id === view.tClass)) view.tClass = data.classes[0] ? data.classes[0].id : null;
  if (!data.subjects.find(s => s.id === view.tSubject)) view.tSubject = data.subjects[0] ? data.subjects[0].id : null;
}
function notesPage() {
  ensureSel();
  const back = '<button class="btn alt small" data-go="/prof">Retour au tableau de bord</button>';
  if (!view.tClass || !view.tSubject) {
    return back + '<div class="empty">Ajoutez d’abord un élève avec sa classe, et au moins une matière dans Réglages.</div>';
  }
  const rows = classStudents(view.tClass).map(s => {
    const g = getG(s.id, view.tSubject, view.term);
    return `<tr>
      <td>${esc(s.name)}</td>
      ${[0, 1, 2].map(i => `<td class="n"><input class="g" type="number" inputmode="decimal" min="0" max="20" step="0.25"
        data-f="grade" data-sid="${s.id}" data-i="${i}" value="${g[i] === null ? '' : g[i]}" aria-label="${esc(s.name)} ${['devoir 1', 'devoir 2', 'composition'][i]}"></td>`).join('')}
      <td class="n" id="a-${s.id}">${badge(subAvg(s.id, view.tSubject, view.term))}</td>
    </tr>`;
  }).join('');
  return `
    ${back}
    <h2>Notes par classe</h2>
    <div class="row">
      <select data-f="tclass" aria-label="Classe">${data.classes.map(c => `<option value="${c.id}" ${c.id === view.tClass ? 'selected' : ''}>${esc(c.name)}</option>`).join('')}</select>
      <select data-f="tsubject" aria-label="Matière">${data.subjects.map(s => `<option value="${s.id}" ${s.id === view.tSubject ? 'selected' : ''}>${esc(s.name)}</option>`).join('')}</select>
    </div>
    ${termBar()}
    <table class="ledger">
      <thead><tr><th>Élève</th><th class="n">Dev. 1</th><th class="n">Dev. 2</th><th class="n">Compo.</th><th class="n">Moy.</th></tr></thead>
      <tbody>${rows}</tbody></table>
    <div class="summary"><span>Moyenne de la classe <span id="cavg">${badge(classSubAvg(view.tClass, view.tSubject, view.term))}</span></span></div>
    <p class="hint">Notes sur 20. Les moyennes se calculent automatiquement. Les parents voient les changements après avoir actualisé la page.</p>`;
}
function updateAvgs() {
  classStudents(view.tClass).forEach(s => {
    const el = $('#a-' + s.id);
    if (el) el.innerHTML = badge(subAvg(s.id, view.tSubject, view.term));
  });
  const c = $('#cavg');
  if (c) c.innerHTML = badge(classSubAvg(view.tClass, view.tSubject, view.term));
}

/* ---------- Réglages ---------- */
function reglagesPage() {
  const modes = {
    shared: 'Stockage partagé : tous les visiteurs voient les mêmes données.',
    local: 'Stockage sur cet appareil : les parents ne verront pas ces notes depuis leur propre téléphone. Pour partager les données, il faut brancher un serveur (voir Store dans app.js).',
    memory: 'Stockage temporaire : les données disparaîtront à la fermeture de la page.'
  };
  return `
    <button class="btn alt small" data-go="/prof">Retour au tableau de bord</button>
    <h2>Réglages</h2>
    <p class="hint">${modes[Store.mode]}</p>
    <label for="schoolname">Nom de l’établissement</label>
    <input type="text" id="schoolname" class="full" data-f="school" value="${esc(data.schoolName)}">

    <h3>Matières</h3>
    <div class="row">
      <input type="text" id="newsub" placeholder="Ex. : Physique-Chimie" aria-label="Nom de la matière">
      <input type="number" id="newcoef" min="1" max="10" step="1" value="2" style="width:80px" aria-label="Coefficient">
      <button class="btn" data-a="addsubject">Ajouter la matière</button>
    </div>
    ${data.subjects.map(s => `<div class="list-item"><span>${esc(s.name)} <span class="sub-coef">coef. ${s.coef}</span></span>
      <button class="btn danger small" data-a="del" data-t="subject" data-id="${s.id}">Supprimer</button></div>`).join('') || '<p class="hint">Aucune matière.</p>'}

    <h3>Classes</h3>
    ${data.classes.map(c => `<div class="list-item"><span>${esc(c.name)} <span class="sub-coef">${classStudents(c.id).length} élève(s)</span></span>
      <button class="btn danger small" data-a="del" data-t="class" data-id="${c.id}">Supprimer</button></div>`).join('') || '<p class="hint">Aucune classe. Elles se créent en ajoutant un élève.</p>'}

    <h3>Code d’accès des professeurs</h3>
    <div class="row">
      <input type="password" id="newpin" inputmode="numeric" autocomplete="off" placeholder="4 caractères ou plus" aria-label="Nouveau code">
      <button class="btn" data-a="setpin">Changer le code</button>
    </div>
    <p id="pinmsg" class="err" aria-live="polite"></p>

    <h3>Données</h3>
    <div class="row">
      <button class="btn danger" data-a="del" data-t="all">Tout effacer</button>
      <button class="btn alt" data-a="demo">Charger l’exemple</button>
      <button class="btn alt" data-a="lock">Verrouiller</button>
    </div>
    <p class="hint">« Tout effacer » et « Charger l’exemple » remplacent toutes les données. Supprimer une classe ou une matière supprime aussi les notes liées.</p>`;
}

/* ---------- Export CSV ---------- */
function exportCSV() {
  const t = view.term;
  const num = x => x === null ? '' : String(Math.round(x * 100) / 100).replace('.', ',');
  const rows = [['Classe', 'Élève', ...data.subjects.map(s => s.name), 'Moyenne générale', 'Rang']];
  data.classes.forEach(c => classStudents(c.id).forEach(s => {
    rows.push([c.name, s.name, ...data.subjects.map(sub => num(subAvg(s.id, sub.id, t))), num(genAvg(s.id, t)), rkLabel(rank(s, t))]);
  }));
  const csv = '\uFEFF' + rows.map(r => r.map(x => `"${String(x).replace(/"/g, '""')}"`).join(';')).join('\r\n');
  const url = URL.createObjectURL(new Blob([csv], {type: 'text/csv;charset=utf-8'}));
  const a = document.createElement('a');
  a.href = url; a.download = `notes-trimestre-${t}.csv`;
  document.body.appendChild(a); a.click(); a.remove();
  setTimeout(() => URL.revokeObjectURL(url), 1000);
  toast('Fichier CSV préparé.', 'ok');
}

/* =====================================================================
   6. ÉVÉNEMENTS
   ===================================================================== */
function removeStudent(id) {
  data.students = data.students.filter(s => s.id !== id);
  Object.keys(data.grades).forEach(k => { if (k.startsWith(id + '|')) delete data.grades[k]; });
}

const app = $('#app');

app.addEventListener('click', async e => {
  const g = e.target.closest('[data-go]');
  if (g) { navigate(g.dataset.go); return; }
  const b = e.target.closest('button[data-a]');
  if (!b) return;
  const a = b.dataset.a, v = b.dataset.v, id = b.dataset.id;

  if (a === 'del') {
    if (!b.dataset.armed) {
      b.dataset.armed = '1'; b.dataset.label = b.textContent; b.textContent = 'Confirmer ?';
      setTimeout(() => { if (b.isConnected) { delete b.dataset.armed; b.textContent = b.dataset.label; } }, 3000);
      return;
    }
    const t = b.dataset.t;
    if (t === 'student') removeStudent(id);
    if (t === 'class') { classStudents(id).forEach(s => removeStudent(s.id)); data.classes = data.classes.filter(c => c.id !== id); }
    if (t === 'subject') { data.subjects = data.subjects.filter(s => s.id !== id); Object.keys(data.grades).forEach(k => { if (k.split('|')[1] === id) delete data.grades[k]; }); }
    if (t === 'all') data = emptyData(data.pin, data.schoolName);
    draft = null; view.classId = 'all';
    scheduleSave();
    if (t === 'student' && route.startsWith('/prof/eleve/')) { toast('Élève supprimé.'); navigate('/prof'); return; }
    render(); return;
  }

  switch (a) {
    case 'theme':
      applyTheme(document.documentElement.dataset.theme === 'dark' ? 'light' : 'dark');
      break;
    case 'print': window.print(); return;
    case 'csv': exportCSV(); return;
    case 'term':
      view.term = +v;
      if (draft) { draft.term = view.term; if (draft.id) draft.grades = loadGrades(draft.id, view.term); }
      break;
    case 'cls': view.classId = v; break;
    case 'lock': view.unlocked = false; draft = null; navigate('/'); return;
    case 'demo': data = seed(); draft = null; view.classId = 'all'; scheduleSave(); toast('Données d’exemple chargées.'); break;
    case 'refresh': {
      const d = await Store.load();
      if (d) { data = d; toast('Notes mises à jour.', 'ok'); } else toast('Impossible d’actualiser.', 'err');
      break;
    }
    case 'unlock':
      if ($('#pin').value === data.pin) view.unlocked = true;
      else { $('#pinerr').textContent = 'Code incorrect. Vérifiez et réessayez.'; return; }
      break;
    case 'addsubject': {
      const name = $('#newsub').value.trim();
      const coef = Math.max(1, parseInt($('#newcoef').value) || 1);
      if (!name) return;
      data.subjects.push({id: uid(), name, coef}); scheduleSave(); toast(`Matière « ${name} » ajoutée.`, 'ok'); break;
    }
    case 'setpin': {
      const p = $('#newpin').value.trim();
      if (p.length < 4) { $('#pinmsg').textContent = 'Le code doit contenir au moins 4 caractères.'; return; }
      data.pin = p; scheduleSave(); toast('Code modifié.', 'ok'); break;
    }
    case 'savedraft': saveDraft(); return;
  }
  render();
});

app.addEventListener('input', e => {
  const el = e.target, f = el.dataset.f;
  if (f === 'q') { view.q = el.value; $('#plist').innerHTML = parentList(); return; }
  if (f === 'dq') { view.dq = el.value; $('#dlist').innerHTML = dashList(); return; }
  if (f === 'dname') { draft.name = el.value; return; }
  if (f === 'dcls') { draft.cls = el.value; return; }
  if (f === 'dgrade' || f === 'grade') {
    const i = +el.dataset.i, raw = el.value.trim();
    const v = raw === '' ? null : parseFloat(raw.replace(',', '.'));
    if (v !== null && (isNaN(v) || v < 0 || v > 20)) { el.classList.add('bad'); return; }
    el.classList.remove('bad');
    if (f === 'dgrade') {
      const arr = draft.grades[el.dataset.sub] || (draft.grades[el.dataset.sub] = [null, null, null]);
      arr[i] = v; updateFormAvgs();
    } else {
      const k = `${el.dataset.sid}|${view.tSubject}|${view.term}`;
      const arr = data.grades[k] || (data.grades[k] = [null, null, null]);
      arr[i] = v;
      if (arr.every(x => x === null)) delete data.grades[k];
      updateAvgs(); scheduleSave();
    }
  }
});

app.addEventListener('change', e => {
  const f = e.target.dataset.f;
  if (f === 'tclass') { view.tClass = e.target.value; render(); }
  if (f === 'tsubject') { view.tSubject = e.target.value; render(); }
  if (f === 'sort') { view.sort = e.target.value; $('#plist').innerHTML = parentList(); }
  if (f === 'school') { data.schoolName = e.target.value.trim() || data.schoolName; scheduleSave(); render(); }
});

app.addEventListener('keydown', e => {
  if (e.key !== 'Enter') return;
  const el = e.target;
  if (el.id === 'pin') { const b = document.querySelector('[data-a="unlock"]'); if (b) b.click(); return; }
  if (el.classList && el.classList.contains('g')) {
    e.preventDefault();
    const all = [...document.querySelectorAll('input.g')];
    const next = all[all.indexOf(el) + 1];
    if (next) { next.focus(); next.select(); }
  }
});

/* =====================================================================
   7. DÉMARRAGE
   ===================================================================== */
(async function init() {
  const saved = Pref.get();
  applyTheme(saved || (window.matchMedia && matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light'));
  data = await Store.load();
  if (!data) { data = seed(); await Store.save(data); }
  try { route = location.hash.slice(1) || '/'; } catch (e) { route = '/'; }
  render();
})();

})();

</script>
</body>
</html>
