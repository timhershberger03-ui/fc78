<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1" />
  <title>Doyle FC78 — Jobs + Archive + Species Totals + Backup</title>
  <style>
    :root{ --pad:12px; --r:14px; --b:#ddd; --bg:#f7f7f7; --txt:#111; }
    body{ font-family:system-ui,-apple-system,Segoe UI,Roboto,Arial,sans-serif; margin:0; background:#fff; color:var(--txt); }
    header{
      position:sticky; top:0; z-index:30;
      background:#fff; border-bottom:1px solid #eee;
      padding:10px 12px;
      display:flex; align-items:center; justify-content:space-between; gap:10px;
    }
    header h1{ font-size:16px; margin:0; }
    header .hdrBtns{ display:flex; gap:10px; }
    header button{
      width:auto; padding:10px 12px; border-radius:12px;
      border:1px solid #111; background:#111; color:#fff; font-weight:900; font-size:14px;
    }

    .wrap{ padding:12px; padding-bottom:150px; max-width:760px; margin:0 auto; }
    .card{ border:1px solid var(--b); border-radius:var(--r); padding:var(--pad); }
    label{ display:block; font-weight:900; font-size:13px; margin:10px 0 6px; }
    input, select, textarea{
      width:100%; font-size:16px; padding:10px 12px;
      border-radius:12px; border:1px solid #ccc; box-sizing:border-box;
    }
    textarea{ min-height: 180px; font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", "Courier New", monospace; }
    .mini{ font-size:12px; color:#666; margin-top:6px; line-height:1.25; }
    .grid2{ display:grid; grid-template-columns:1fr 1fr; gap:10px; }
    @media (max-width:380px){ .grid2{ grid-template-columns:1fr; } }
    .warn{ color:#b00020; font-weight:950; }

    .out{ background:var(--bg); border-radius:var(--r); padding:10px 12px; margin-top:10px; }
    .pillRow{ display:flex; gap:8px; flex-wrap:wrap; margin-top:10px; }
    .pill{ background:#f2f2f2; padding:8px 10px; border-radius:999px; font-weight:950; font-size:13px; }

    .list{ margin-top:12px; display:flex; flex-direction:column; gap:10px; }
    .tree{ border:1px solid #eee; border-radius:var(--r); padding:10px 12px; }
    .treeTop{ display:flex; justify-content:space-between; gap:10px; align-items:baseline; }
    .treeTop b{ font-size:15px; }
    .treeGrid{
      margin-top:8px;
      display:grid; grid-template-columns:1fr 1fr; gap:6px 10px;
      font-size:13px;
    }
    .treeGrid div{ color:#333; }
    .treeGrid span{ color:#666; }
    .treeActions{ margin-top:10px; display:flex; gap:10px; flex-wrap:wrap; }
    .treeActions button{
      flex:1; min-width:120px;
      padding:10px 12px; border-radius:12px;
      border:1px solid #ccc; background:#fff; color:#111; font-weight:900;
    }

    .totalsBar{
      position:fixed; left:0; right:0; bottom:0; z-index:40;
      background:#fff; border-top:1px solid #eee;
      padding:10px 12px;
    }
    .totRow{ display:flex; gap:10px; flex-wrap:wrap; align-items:center; justify-content:space-between; }

    .drawerBackdrop{ position:fixed; inset:0; background:rgba(0,0,0,.35); display:none; z-index:60; }
    .drawer{
      position:fixed; left:0; right:0; bottom:0;
      background:#fff; border-top-left-radius:18px; border-top-right-radius:18px;
      max-height:85vh; overflow:auto;
      transform:translateY(110%); transition:transform .2s ease;
      z-index:70; padding:12px;
    }
    .drawer.open{ transform:translateY(0); }
    .drawerBackdrop.open{ display:block; }
    .drawerHeader{ display:flex; justify-content:space-between; align-items:center; gap:10px; }
    .drawerHeader h2{ margin:0; font-size:16px; }
    .drawerHeader button{ width:auto; padding:10px 12px; border-radius:12px; border:1px solid #ccc; background:#fff; font-weight:900; }

    .hr{ border:none; border-top:1px solid #eee; margin:12px 0; }

    .btnRow3{ display:grid; grid-template-columns:1fr 1fr 1fr; gap:10px; margin-top:10px; }
    .btnRow2{ display:grid; grid-template-columns:1fr 1fr; gap:10px; margin-top:10px; }
    @media (max-width:520px){ .btnRow3{ grid-template-columns:1fr; } .btnRow2{ grid-template-columns:1fr; } }
    .btn{
      padding:10px 12px; border-radius:12px; border:1px solid #111;
      background:#111; color:#fff; font-weight:950;
    }
    .btn.secondary{ background:#fff; color:#111; border-color:#ccc; }
    .btn.danger{ background:#b00020; border-color:#b00020; }

    table{ width:100%; border-collapse:collapse; }
    th,td{ text-align:left; padding:8px 6px; border-bottom:1px solid #eee; font-size:13px; }
    th{ font-size:12px; color:#444; text-transform:uppercase; letter-spacing:.04em; }
  </style>
</head>
<body>
<header>
  <h1>Doyle FC78 — Phone</h1>
  <div class="hdrBtns">
    <button id="settingsBtn" type="button">⚙️ Settings</button>
  </div>
</header>

<div class="wrap">
  <div class="card">

    <div class="grid2">
      <div>
        <label for="species">Species</label>
        <select id="species">
          <option value="Walnut" selected>Walnut</option>
          <option value="Whiteoak">Whiteoak</option>
          <option value="Maple">Maple</option>
          <option value="Redoak">Redoak</option>
          <option value="Pinoak">Pinoak</option>
          <option value="Cherry">Cherry</option>
        </select>
      </div>
      <div>
        <label>Job</label>
        <div class="out mini" id="jobTag">—</div>
        <div class="mini" id="jobSawLine">Saw $/BF (job): —</div>
      </div>
    </div>

    <div class="grid2">
      <div>
        <label for="dbh">DBH (in)</label>
        <input id="dbh" type="number" inputmode="numeric" min="10" max="40" placeholder="22">
        <div class="mini">2 digits → jumps to total ft</div>
      </div>
      <div>
        <label for="totFt">Total ft</label>
        <input id="totFt" type="number" inputmode="numeric" min="0" max="200" placeholder="80">
        <div class="mini">then jumps to veneer ft</div>
      </div>
    </div>

    <div class="grid2">
      <div>
        <label for="venFt">Veneer ft (00–99)</label>
        <input id="venFt" type="text" inputmode="numeric" maxlength="2" placeholder="00">
        <div class="mini">2 digits required (00–99)</div>
      </div>
      <div>
        <label for="vRate">Veneer $/BF (always resets)</label>
        <input id="vRate" type="text" inputmode="decimal" placeholder="00.00" value="00.00">
        <div class="mini">Auto-add happens after veneer price</div>
      </div>
    </div>

    <div id="msg" class="mini"></div>

    <div class="pillRow" id="speciesPills"></div>

    <div id="list" class="list"></div>
  </div>
</div>

<div class="totalsBar">
  <div class="totRow">
    <div class="pill">Trees: <span id="tc">0</span></div>
    <div class="pill">V BF: <span id="vbfT">0</span></div>
    <div class="pill">S BF: <span id="sbfT">0</span></div>
    <div class="pill">BF: <span id="bfT">0</span></div>
    <div class="pill">$: <span id="dolT">$0.00</span></div>
  </div>
</div>

<!-- SETTINGS DRAWER -->
<div id="backdrop" class="drawerBackdrop"></div>
<div id="drawer" class="drawer" aria-hidden="true">
  <div class="drawerHeader">
    <h2>Jobs</h2>
    <button id="closeDrawer" type="button">Close</button>
  </div>

  <hr class="hr">

  <div class="grid2">
    <div>
      <label for="jobSel">Job</label>
      <select id="jobSel"></select>
      <div class="mini">
        <label style="display:flex; gap:8px; align-items:center; font-weight:900; margin-top:8px;">
          <input id="showArchived" type="checkbox" style="width:auto; transform:scale(1.1);">
          Show archived jobs
        </label>
      </div>
    </div>

    <div>
      <label for="jobSawRate">Saw $/BF (job default)</label>
      <input id="jobSawRate" type="number" inputmode="decimal" step="0.01" min="0" placeholder="0.00">
      <div class="mini">This applies to every tree in this job.</div>
    </div>
  </div>

  <div class="grid2">
    <div>
      <label for="newJobName">New job name</label>
      <input id="newJobName" type="text" placeholder="Smith Walnut – Tract A">
    </div>
    <div>
      <label>Archive</label>
      <button id="toggleArchive" class="btn secondary" type="button">Archive this job</button>
      <div class="mini">Archived jobs are hidden unless “Show archived” is on.</div>
    </div>
  </div>

  <div class="btnRow3">
    <button id="createJob" class="btn" type="button">Create Job</button>
    <button id="renameJob" class="btn secondary" type="button">Rename</button>
    <button id="deleteJob" class="btn danger" type="button">Delete</button>
  </div>

  <div class="btnRow2">
    <button id="exportCsv" class="btn secondary" type="button">Export CSV (this job)</button>
    <button id="clearJobTrees" class="btn secondary" type="button">Clear Trees (this job)</button>
  </div>

  <hr class="hr">

  <div>
    <h3 style="margin:0 0 8px; font-size:14px;">Totals by Species (this job)</h3>
    <div class="out" id="speciesTableWrap"></div>
  </div>

  <hr class="hr">

  <div>
    <h3 style="margin:0 0 8px; font-size:14px;">Backup / Restore</h3>
    <div class="btnRow2">
      <button id="backupJson" class="btn secondary" type="button">Backup (JSON)</button>
      <button id="openRestore" class="btn secondary" type="button">Restore (JSON)</button>
    </div>
    <div class="mini">Backup saves ALL jobs (including archived) and all trees.</div>
  </div>

  <div class="out mini">
    Entry flow: DBH → Total → Veneer ft (2 digits) → Veneer $/BF (auto-add).<br>
    Veneer $/BF resets to 00.00 after each tree. Saw $/BF is set per job.
  </div>
</div>

<!-- RESTORE DRAWER -->
<div id="rBackdrop" class="drawerBackdrop"></div>
<div id="rDrawer" class="drawer" aria-hidden="true">
  <div class="drawerHeader">
    <h2>Restore JSON</h2>
    <button id="closeRestore" type="button">Close</button>
  </div>
  <hr class="hr">
  <div class="out mini">
    Paste your backup JSON below, then tap Restore. This will replace your current data.
  </div>
  <label for="restoreText">Backup JSON</label>
  <textarea id="restoreText" placeholder='{"currentJobId":"...","jobs":{...}}'></textarea>
  <div class="btnRow2">
    <button id="doRestore" class="btn" type="button">Restore</button>
    <button id="clearRestoreBox" class="btn secondary" type="button">Clear</button>
  </div>
  <div class="mini warn" id="restoreMsg"></div>
</div>

<script>
  // ---------- STORAGE ----------
  const STORAGE_KEY = "fc78_jobs_archive_species_backup_v2";
  function nowId(){ return "job_" + Math.random().toString(16).slice(2) + "_" + Date.now(); }
  function loadState(){ try{ const r=localStorage.getItem(STORAGE_KEY); return r?JSON.parse(r):null; }catch{ return null; } }
  function saveState(){ localStorage.setItem(STORAGE_KEY, JSON.stringify(state)); }

  // ---------- FC78 TABLE ----------
  const T = {
    10:{1:14,1.5:17,2:20,2.5:21,3:22,3.5:null,4:null,4.5:null,5:null,5.5:null,6:null},
    11:{1:22,1.5:27,2:32,2.5:35,3:38,3.5:null,4:null,4.5:null,5:null,5.5:null,6:null},
    12:{1:29,1.5:36,2:43,2.5:48,3:53,3.5:54,4:56,4.5:null,5:null,5.5:null,6:null},
    13:{1:38,1.5:48,2:59,2.5:66,3:73,3.5:76,4:80,4.5:null,5:null,5.5:null,6:null},
    14:{1:48,1.5:62,2:75,2.5:84,3:93,3.5:98,4:103,4.5:null,5:null,5.5:null,6:null},
    15:{1:60,1.5:78,2:96,2.5:108,3:121,3.5:128,4:136,4.5:null,5:null,5.5:null,6:null},
    16:{1:72,1.5:94,2:116,2.5:132,3:149,3.5:160,4:170,4.5:null,5:null,5.5:null,6:null},
    17:{1:86,1.5:113,2:140,2.5:161,3:182,3.5:196,4:209,4.5:null,5:null,5.5:null,6:null},
    18:{1:100,1.5:132,2:164,2.5:190,3:215,3.5:232,4:248,4.5:null,5:null,5.5:null,6:null},
    19:{1:118,1.5:156,2:195,2.5:225,3:256,3.5:276,4:297,4.5:null,5:null,5.5:null,6:null},
    20:{1:135,1.5:180,2:225,2.5:261,3:297,3.5:322,4:346,4.5:364,5:383,5.5:null,6:null},
    21:{1:154,1.5:207,2:260,2.5:302,3:344,3.5:374,4:404,4.5:428,5:452,5.5:null,6:null},
    22:{1:174,1.5:234,2:295,2.5:344,3:392,3.5:427,4:462,4.5:492,5:521,5.5:null,6:null},
    23:{1:195,1.5:264,2:332,2.5:388,3:444,3.5:483,4:522,4.5:558,5:594,5.5:null,6:null},
    24:{1:216,1.5:293,2:370,2.5:433,3:496,3.5:539,4:582,4.5:625,5:668,5.5:null,6:null},
    25:{1:241,1.5:328,2:414,2.5:486,3:558,3.5:609,4:660,4.5:709,5:758,5.5:null,6:null},
    26:{1:266,1.5:362,2:459,2.5:539,3:619,3.5:678,4:737,4.5:793,5:849,5.5:null,6:null},
    27:{1:292,1.5:398,2:505,2.5:594,3:684,3.5:749,4:814,4.5:877,5:940,5.5:null,6:null},
    28:{1:317,1.5:434,2:551,2.5:651,3:750,3.5:820,4:890,4.5:961,5:1032,5.5:1096,6:1161},
    29:{1:346,1.5:475,2:604,2.5:714,3:824,3.5:902,4:980,4.5:1061,5:1142,5.5:1218,6:1294},
    30:{1:376,1.5:517,2:658,2.5:778,3:898,3.5:984,4:1069,4.5:1160,5:1251,5.5:1339,6:1427},
    31:{1:408,1.5:562,2:717,2.5:850,3:983,3.5:1080,4:1176,4.5:1273,5:1370,5.5:1470,6:1570},
    32:{1:441,1.5:608,2:776,2.5:922,3:1068,3.5:1176,4:1283,4.5:1386,5:1488,5.5:1600,6:1712},
    33:{1:474,1.5:654,2:835,2.5:994,3:1152,3.5:1268,4:1385,4.5:1497,5:1609,5.5:1734,6:1858},
    34:{1:506,1.5:700,2:894,2.5:1064,3:1235,3.5:1361,4:1487,4.5:1608,5:1730,5.5:1866,6:2003},
    35:{1:544,1.5:754,2:964,2.5:1149,3:1334,3.5:1472,4:1610,4.5:1743,5:1876,5.5:2020,6:2163},
    36:{1:581,1.5:808,2:1035,2.5:1234,3:1434,3.5:1583,4:1732,4.5:1878,5:2023,5.5:2173,6:2323},
    37:{1:618,1.5:860,2:1102,2.5:1318,3:1534,3.5:1694,4:1854,4.5:2013,5:2172,5.5:2332,6:2492},
    38:{1:655,1.5:912,2:1170,2.5:1402,3:1635,3.5:1805,4:1975,4.5:2148,5:2322,5.5:2491,6:2660},
    39:{1:698,1.5:974,2:1250,2.5:1498,3:1746,3.5:1932,4:2118,4.5:2298,5:2479,5.5:2662,6:2844},
    40:{1:740,1.5:1035,2:1330,2.5:1594,3:1858,3.5:2059,4:2260,4.5:2448,5:2636,5.5:2832,6:3027}
  };

  // ---------- ELEMENTS ----------
  const speciesEl = document.getElementById("species");
  const jobTag = document.getElementById("jobTag");
  const jobSawLine = document.getElementById("jobSawLine");

  const dbh = document.getElementById("dbh");
  const totFt = document.getElementById("totFt");
  const venFt = document.getElementById("venFt");
  const vRate = document.getElementById("vRate");

  const msg = document.getElementById("msg");
  const list = document.getElementById("list");

  const tc = document.getElementById("tc");
  const vbfT = document.getElementById("vbfT");
  const sbfT = document.getElementById("sbfT");
  const bfT = document.getElementById("bfT");
  const dolT = document.getElementById("dolT");

  const speciesPills = document.getElementById("speciesPills");

  // drawers
  const settingsBtn = document.getElementById("settingsBtn");
  const backdrop = document.getElementById("backdrop");
  const drawer = document.getElementById("drawer");
  const closeDrawer = document.getElementById("closeDrawer");

  const jobSel = document.getElementById("jobSel");
  const showArchived = document.getElementById("showArchived");
  const newJobName = document.getElementById("newJobName");
  const createJob = document.getElementById("createJob");
  const renameJob = document.getElementById("renameJob");
  const deleteJob = document.getElementById("deleteJob");
  const toggleArchive = document.getElementById("toggleArchive");
  const exportCsv = document.getElementById("exportCsv");
  const clearJobTrees = document.getElementById("clearJobTrees");
  const jobSawRate = document.getElementById("jobSawRate");

  const speciesTableWrap = document.getElementById("speciesTableWrap");
  const backupJson = document.getElementById("backupJson");
  const openRestore = document.getElementById("openRestore");

  // restore drawer
  const rBackdrop = document.getElementById("rBackdrop");
  const rDrawer = document.getElementById("rDrawer");
  const closeRestore = document.getElementById("closeRestore");
  const restoreText = document.getElementById("restoreText");
  const doRestore = document.getElementById("doRestore");
  const clearRestoreBox = document.getElementById("clearRestoreBox");
  const restoreMsg = document.getElementById("restoreMsg");

  // ---------- STATE ----------
  let state = loadState();
  if(!state || !state.jobs || !state.currentJobId){
    const id = nowId();
    state = {
      currentJobId: id,
      jobs: {
        [id]: { name: "Job 1", archived: false, sawRateJob: 0.00, rows: [] }
      }
    };
    saveState();
  }

  // ---------- HELPERS ----------
  const clamp = (v,a,b)=>Math.max(a,Math.min(b,v));
  const money = x => `$${(Number(x)||0).toFixed(2)}`;
  const digitsOnly = s => String(s||"").replace(/[^0-9]/g,"");

  function currentJob(){ return state.jobs[state.currentJobId]; }

  // ---- money format: always 2 digits before decimal, 2 after (00.00 to 99.99)
  function parseMoney2Digits(text){
    let s = String(text||"").trim();
    if(s==="") return {ok:false, val:0, fmt:"00.00"};
    s = s.replace(/[^0-9.]/g,"");
    if(!s.includes(".")) s = s + ".00";
    const parts = s.split(".");
    let whole = (parts[0]||"").replace(/\D/g,"").slice(0,2);
    let frac  = (parts[1]||"").replace(/\D/g,"").slice(0,2);
    whole = whole.padStart(2,"0").slice(-2);
    frac  = frac.padEnd(2,"0").slice(0,2);
    const val = Number(`${whole}.${frac}`);
    return { ok:Number.isFinite(val), val, fmt:`${whole}.${frac}` };
  }

  function enforce2DigitInt(el, max){
    let d = digitsOnly(el.value).slice(0,2);
    if(d.length===0){ el.value=""; return; }
    let n = Number(d);
    if(!Number.isFinite(n)) n = 0;
    n = Math.max(0, Math.min(max, n));
    el.value = String(n);
  }
  function pad2(el){
    const d = digitsOnly(el.value);
    if(d.length===0) return;
    el.value = d.padStart(2,"0").slice(-2);
  }

  // ---------- FC78 MATH ----------
  function getTableValue(dbhInt, logs){
    const row = T[dbhInt];
    if(!row) return null;
    return (row[logs] ?? null);
  }

  // Interpolate between whole-inch DBHs
  function bfAtLogs(dbhRaw, logs){
    if(logs<=0) return 0;
    const d = clamp(dbhRaw,10,40);
    const lo = Math.floor(d), hi = Math.ceil(d);
    if(lo===hi) return getTableValue(lo,logs);
    const vLo = getTableValue(lo,logs), vHi = getTableValue(hi,logs);
    if(vLo==null || vHi==null) return null;
    const t = (d-lo)/(hi-lo);
    return vLo + (vHi - vLo)*t;
  }

  // Cumulative BF at any feet (continuous) using 0.5-log interpolation
  function cumulativeBF_atFeet(dbhRaw, ft){
    if(!Number.isFinite(ft) || ft<=0) return 0;
    const ftC = clamp(ft,0,96);

    if(ftC < 16){
      const one = bfAtLogs(dbhRaw,1);
      if(one==null) return null;
      return one*(ftC/16);
    }

    const logsRaw = ftC/16;
    if(logsRaw>=6) return bfAtLogs(dbhRaw,6);
    if(logsRaw<=1) return bfAtLogs(dbhRaw,1);

    let lo = Math.floor(logsRaw*2)/2;
    let hi = lo + 0.5;
    lo = clamp(lo,1,6); hi = clamp(hi,1,6);

    const vLo = bfAtLogs(dbhRaw,lo);
    const vHi = bfAtLogs(dbhRaw,hi);
    if(vLo==null || vHi==null) return null;

    const t = (logsRaw-lo)/(hi-lo);
    return vLo + (vHi - vLo)*t;
  }

  // ---------- DRAWERS ----------
  function openSettings(){
    backdrop.classList.add("open");
    drawer.classList.add("open");
  }
  function closeSettings(){
    backdrop.classList.remove("open");
    drawer.classList.remove("open");
  }
  settingsBtn.onclick = openSettings;
  closeDrawer.onclick = closeSettings;
  backdrop.onclick = closeSettings;

  function openRestoreDrawer(){
    rBackdrop.classList.add("open");
    rDrawer.classList.add("open");
    restoreMsg.textContent = "";
  }
  function closeRestoreDrawer(){
    rBackdrop.classList.remove("open");
    rDrawer.classList.remove("open");
  }
  openRestore.onclick = openRestoreDrawer;
  closeRestore.onclick = closeRestoreDrawer;
  rBackdrop.onclick = closeRestoreDrawer;

  // ---------- JOBS ----------
  function ensureJobFields(job){
    if(typeof job.archived !== "boolean") job.archived = false;
    if(typeof job.sawRateJob !== "number" || !Number.isFinite(job.sawRateJob) || job.sawRateJob<0) job.sawRateJob = 0.00;
    if(!Array.isArray(job.rows)) job.rows = [];
  }

  function renderJobSelect(){
    jobSel.innerHTML = "";
    const ids = Object.keys(state.jobs);

    const filtered = ids
      .map(id => ({id, job: state.jobs[id]}))
      .filter(x => showArchived.checked ? true : !x.job.archived)
      .sort((a,b)=>(a.job.name||"").localeCompare(b.job.name||""));

    // If current job is archived and showArchived is off, temporarily include it so you can still see it.
    const cur = state.jobs[state.currentJobId];
    if(cur && cur.archived && !showArchived.checked){
      filtered.unshift({id: state.currentJobId, job: cur});
    }

    for(const item of filtered){
      const o = document.createElement("option");
      o.value = item.id;
      o.textContent = (item.job.archived ? "🗄️ " : "") + (item.job.name||item.id);
      if(item.id===state.currentJobId) o.selected = true;
      jobSel.appendChild(o);
    }
  }

  function loadJobIntoUI(){
    const job = currentJob();
    ensureJobFields(job);

    jobTag.textContent = job.name || "—";
    jobSawLine.textContent = `Saw $/BF (job): ${job.sawRateJob.toFixed(2)}`;
    jobSawRate.value = job.sawRateJob.toFixed(2);

    toggleArchive.textContent = job.archived ? "Unarchive this job" : "Archive this job";

    msg.textContent = "";
    renderList();
    renderSpeciesTable();
    dbh.focus();
  }

  showArchived.onchange = () => {
    renderJobSelect();
  };

  jobSel.onchange = () => {
    state.currentJobId = jobSel.value;
    saveState();
    loadJobIntoUI();
  };

  createJob.onclick = () => {
    const name = (newJobName.value||"").trim() || `Job ${Object.keys(state.jobs).length+1}`;
    const id = nowId();
    state.jobs[id] = { name, archived:false, sawRateJob: 0.00, rows: [] };
    state.currentJobId = id;
    newJobName.value = "";
    saveState();
    renderJobSelect();
    loadJobIntoUI();
  };

  renameJob.onclick = () => {
    const job = currentJob();
    const name = prompt("Rename job to:", job.name||"");
    if(name==null) return;
    const t = name.trim(); if(!t) return;
    job.name = t;
    saveState();
    renderJobSelect();
    loadJobIntoUI();
  };

  deleteJob.onclick = () => {
    const job = currentJob();
    if(!confirm(`Delete job "${job.name}"?`)) return;
    delete state.jobs[state.currentJobId];
    const remaining = Object.keys(state.jobs);
    if(remaining.length===0){
      const id = nowId();
      state.jobs[id] = { name:"Job 1", archived:false, sawRateJob:0.00, rows:[] };
      state.currentJobId = id;
    }else{
      state.currentJobId = remaining[0];
    }
    saveState();
    renderJobSelect();
    loadJobIntoUI();
  };

  toggleArchive.onclick = () => {
    const job = currentJob();
    job.archived = !job.archived;
    saveState();
    renderJobSelect();
    loadJobIntoUI();
  };

  clearJobTrees.onclick = () => {
    const job = currentJob();
    if(!confirm(`Clear ALL trees from "${job.name}"?`)) return;
    job.rows = [];
    saveState();
    renderList();
    renderSpeciesTable();
  };

  // job saw rate
  jobSawRate.addEventListener("input", () => {
    const job = currentJob();
    let n = Number(jobSawRate.value);
    if(!Number.isFinite(n) || n<0) n = 0;
    job.sawRateJob = n;
    saveState();
    jobSawLine.textContent = `Saw $/BF (job): ${job.sawRateJob.toFixed(2)}`;
    // Update display totals; existing rows keep their stored saw rate
    renderList();
    renderSpeciesTable();
  });

  // ---------- BACKUP / RESTORE ----------
  backupJson.onclick = () => {
    const blob = new Blob([JSON.stringify(state, null, 2)], {type:"application/json;charset=utf-8"});
    const url = URL.createObjectURL(blob);
    const a = document.createElement("a");
    a.href = url;
    a.download = `fc78_backup_${new Date().toISOString().slice(0,10)}.json`;
    document.body.appendChild(a); a.click(); a.remove();
    URL.revokeObjectURL(url);
  };

  clearRestoreBox.onclick = () => {
    restoreText.value = "";
    restoreMsg.textContent = "";
  };

  doRestore.onclick = () => {
    restoreMsg.textContent = "";
    let obj = null;
    try{
      obj = JSON.parse(restoreText.value);
    }catch(e){
      restoreMsg.textContent = "That JSON is not valid.";
      return;
    }
    if(!obj || typeof obj !== "object" || !obj.jobs || typeof obj.jobs !== "object" || !obj.currentJobId){
      restoreMsg.textContent = "Backup format looks wrong (missing jobs/currentJobId).";
      return;
    }
    for(const id of Object.keys(obj.jobs)){
      const j = obj.jobs[id];
      if(!j || typeof j !== "object") continue;
      if(typeof j.name !== "string") j.name = String(j.name||"Job");
      if(typeof j.archived !== "boolean") j.archived = false;
      if(typeof j.sawRateJob !== "number" || !Number.isFinite(j.sawRateJob) || j.sawRateJob<0) j.sawRateJob = 0.00;
      if(!Array.isArray(j.rows)) j.rows = [];
    }
    if(!obj.jobs[obj.currentJobId]){
      obj.currentJobId = Object.keys(obj.jobs)[0] || nowId();
      if(!obj.jobs[obj.currentJobId]){
        obj.jobs[obj.currentJobId] = { name:"Job 1", archived:false, sawRateJob:0.00, rows:[] };
      }
    }

    state = obj;
    saveState();
    renderJobSelect();
    loadJobIntoUI();
    closeRestoreDrawer();
  };

  // ---------- EXPORT CSV ----------
  function csvEscape(s){
    const x = String(s??"");
    if (/[",\n]/.test(x)) return `"${x.replace(/"/g,'""')}"`;
    return x;
  }

  exportCsv.onclick = () => {
    const job = currentJob();
    const header=["Job","Index","Species","DBH","TotalFt","VeneerFt","SawFt","VeneerBF","SawBF","TotalBF","VeneerRate","SawRate(Job)","Dollars"];
    const lines=[header.join(",")];

    job.rows.forEach((r,idx)=>{
      lines.push([
        csvEscape(job.name), idx+1, csvEscape(r.species), csvEscape(r.dbhDisp),
        r.totFt, r.venFt, r.sawFt,
        r.vbf, r.sbf, r.tbf,
        (Number(r.vRate)||0).toFixed(2),
        (Number(r.sRate)||0).toFixed(2),
        (Number(r.dollars)||0).toFixed(2)
      ].join(","));
    });

    const blob=new Blob([lines.join("\n")],{type:"text/csv;charset=utf-8"});
    const url=URL.createObjectURL(blob);
    const a=document.createElement("a");
    a.href=url;
    a.download=`${(job.name||"job").replace(/[^a-z0-9_\- ]/gi,"").trim()||"job"}_fc78.csv`;
    document.body.appendChild(a); a.click(); a.remove(); URL.revokeObjectURL(url);
  };

  // ---------- FAST ENTRY (2 DIGITS veneer ft + veneer price) ----------
  dbh.addEventListener("input",()=>{
    const d = digitsOnly(dbh.value);
    const n = Number(d);
    if(d.length>=2 && n>=10){ totFt.focus(); totFt.select(); }
  });

  totFt.addEventListener("input",()=>{
    const d = digitsOnly(totFt.value);
    const n = Number(d);
    if((d.length===2 && n>=10 && n<=99) || d.length>=3){
      venFt.focus(); venFt.select();
    }
  });

  venFt.addEventListener("input",()=>{
    enforce2DigitInt(venFt, 99);
    const d = digitsOnly(venFt.value);
    if(d.length===2 || d==="0"){
      pad2(venFt);
      vRate.focus(); vRate.select();
    }
  });
  venFt.addEventListener("blur",()=> pad2(venFt));

  // Veneer rate: format on blur; AUTO-ADD after typing stops (debounced). No auto-jump.
  vRate.addEventListener("blur",()=>{
    const p = parseMoney2Digits(vRate.value);
    if(p.ok) vRate.value = p.fmt;
  });

  let addTimer = null;
  vRate.addEventListener("input",()=>{
    if(addTimer) clearTimeout(addTimer);
    addTimer = setTimeout(()=>{
      const p = parseMoney2Digits(vRate.value);
      if(!p.ok) return;
      addTreeNow();
    }, 380);
  });

  // ---------- ADD TREE ----------
  function addTreeNow(){
    msg.textContent = "";

    const job = currentJob();
    ensureJobFields(job);

    const dbhRaw = Number(dbh.value);
    const total = Number(totFt.value);

    const venDigits = digitsOnly(venFt.value);
    const ven = venDigits==="" ? NaN : Number(venDigits);

    const vParsed = parseMoney2Digits(vRate.value);
    const vR = vParsed.ok ? vParsed.val : NaN;

    const sR = Number(job.sawRateJob)||0;

    if(!Number.isFinite(dbhRaw) || !Number.isFinite(total) || total<=0){
      msg.innerHTML = `<span class="warn">Enter DBH and total feet.</span>`; return;
    }
    if(!Number.isFinite(ven) || ven<0 || ven>99){
      msg.innerHTML = `<span class="warn">Veneer ft must be 00–99.</span>`; return;
    }
    if(!Number.isFinite(vR) || vR<0 || vR>99.99){
      msg.innerHTML = `<span class="warn">Veneer $/BF must be 00.00–99.99.</span>`; return;
    }

    const venUse = Math.min(ven,total);
    const saw = Math.max(0,total-venUse);

    const totCum = cumulativeBF_atFeet(dbhRaw,total);
    const venCum = (venUse<=0)?0:cumulativeBF_atFeet(dbhRaw,venUse);

    if(totCum==null || venCum==null){
      msg.innerHTML = `<span class="warn">Missing table value for that DBH/height.</span>`; return;
    }

    const tbf = Math.round(totCum);
    const vbf = Math.round(venCum);
    const sbf = Math.max(0, tbf - vbf);

    const vDollars = vbf*vR;
    const sDollars = sbf*sR;
    const dollars = vDollars + sDollars;

    job.rows.push({
      species: speciesEl.value,
      dbhDisp: `${dbhRaw.toFixed(1)}"`,
      totFt: Math.round(total),
      venFt: Math.round(venUse),
      sawFt: Math.round(saw),
      vbf, sbf, tbf,
      vRate: vR,
      sRate: sR,
      vDollars,
      sDollars,
      dollars
    });

    saveState();
    renderList();
    renderSpeciesTable();

    // reset entry fields:
    dbh.value = "";
    totFt.value = "";
    venFt.value = "";
    vRate.value = "00.00"; // ALWAYS resets
    dbh.focus();
  }

  // ---------- AGGREGATION (SPECIES TOTALS) ----------
  function aggregateBySpecies(job){
    const map = new Map();
    for(const r of job.rows){
      const sp = r.species || "Unknown";
      if(!map.has(sp)){
        map.set(sp, {species:sp, trees:0, vbf:0, sbf:0, tbf:0, vDol:0, sDol:0, dollars:0});
      }
      const a = map.get(sp);
      a.trees += 1;
      a.vbf += Number(r.vbf)||0;
      a.sbf += Number(r.sbf)||0;
      a.tbf += Number(r.tbf)||0;

      const vD = (Number(r.vbf)||0) * (Number(r.vRate)||0);
      const sD = (Number(r.sbf)||0) * (Number(r.sRate)||0);
      a.vDol += vD;
      a.sDol += sD;
      a.dollars += (vD+sD);
    }
    return Array.from(map.values()).sort((a,b)=>a.species.localeCompare(b.species));
  }

  function renderSpeciesTable(){
    const job = currentJob();
    ensureJobFields(job);
    const rows = aggregateBySpecies(job);

    // pills (top of main page)
    speciesPills.innerHTML = "";
    if(rows.length===0){
      speciesPills.innerHTML = `<span class="mini">No species totals yet.</span>`;
    }else{
      for(const r of rows){
        const pill = document.createElement("div");
        pill.className = "pill";
        pill.textContent = `${r.species}: ${r.tbf} BF`;
        speciesPills.appendChild(pill);
      }
    }

    // table (in settings)
    if(rows.length===0){
      speciesTableWrap.innerHTML = `<span class="mini">No trees yet.</span>`;
      return;
    }
    let html = `<table>
      <thead><tr>
        <th>Species</th><th>Trees</th><th>V BF</th><th>S BF</th><th>Total BF</th><th>V $</th><th>S $</th><th>Total $</th>
      </tr></thead><tbody>`;
    for(const r of rows){
      html += `<tr>
        <td>${r.species}</td>
        <td>${r.trees}</td>
        <td>${r.vbf}</td>
        <td>${r.sbf}</td>
        <td>${r.tbf}</td>
        <td>${money(r.vDol)}</td>
        <td>${money(r.sDol)}</td>
        <td>${money(r.dollars)}</td>
      </tr>`;
    }
    html += `</tbody></table>`;
    speciesTableWrap.innerHTML = html;
  }

  // ---------- RENDER LIST / TOTALS ----------
  function renderList(){
    const job = currentJob();
    ensureJobFields(job);

    list.innerHTML = "";
    let vSum=0, sSum=0, bSum=0, dSum=0;

    job.rows.forEach((r,i)=>{
      const vD = (Number(r.vbf)||0)*(Number(r.vRate)||0);
      const sD = (Number(r.sbf)||0)*(Number(r.sRate)||0);
      r.vDollars = vD;
      r.sDollars = sD;
      r.dollars = vD + sD;

      vSum += Number(r.vbf)||0;
      sSum += Number(r.sbf)||0;
      bSum += Number(r.tbf)||0;
      dSum += Number(r.dollars)||0;

      const div = document.createElement("div");
      div.className = "tree";
      div.innerHTML = `
        <div class="treeTop">
          <b>#${i+1} • ${r.species} • ${r.dbhDisp}</b>
          <b>${r.tbf} BF</b>
        </div>
        <div class="treeGrid">
          <div><span>Total</span> ${r.totFt} ft</div>
          <div><span>Veneer</span> ${r.venFt} ft</div>
          <div><span>Saw</span> ${r.sawFt} ft</div>
          <div><span>Tree $</span> ${money(r.dollars)}</div>
          <div><span>V BF</span> ${r.vbf}</div>
          <div><span>S BF</span> ${r.sbf}</div>
          <div><span>V $</span> ${money(r.vDollars)}</div>
          <div><span>S $</span> ${money(r.sDollars)}</div>
          <div><span>V $/BF</span> ${(Number(r.vRate)||0).toFixed(2)}</div>
          <div><span>S $/BF</span> ${(Number(r.sRate)||0).toFixed(2)}</div>
        </div>
        <div class="treeActions">
          <button type="button" data-edit="${i}">Edit Veneer $</button>
          <button type="button" data-del="${i}">Delete</button>
        </div>
      `;
      list.appendChild(div);
    });

    tc.textContent = String(job.rows.length);
    vbfT.textContent = String(vSum);
    sbfT.textContent = String(sSum);
    bfT.textContent  = String(bSum);
    dolT.textContent = money(dSum);

    list.querySelectorAll("button[data-del]").forEach(btn=>{
      btn.addEventListener("click",()=>{
        const i = Number(btn.dataset.del);
        job.rows.splice(i,1);
        saveState();
        renderList();
        renderSpeciesTable();
      });
    });

    list.querySelectorAll("button[data-edit]").forEach(btn=>{
      btn.addEventListener("click",()=>{
        const i = Number(btn.dataset.edit);
        const r = job.rows[i];
        const v = prompt(`Tree #${i+1} Veneer $/BF (00.00):`, (Number(r.vRate)||0).toFixed(2));
        if(v==null) return;
        const vp = parseMoney2Digits(v);
        if(!vp.ok || vp.val<0 || vp.val>99.99) return;
        r.vRate = vp.val;
        saveState();
        renderList();
        renderSpeciesTable();
      });
    });

    saveState();
  }

  // ---------- INIT ----------
  for(const id of Object.keys(state.jobs)){
    const j = state.jobs[id];
    if(!j) continue;
    ensureJobFields(j);
  }
  renderJobSelect();
  loadJobIntoUI();
</script>
</body>
</html>
