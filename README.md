<html lang="id">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Tabungan Emas Pegadaian — Gold Mode Final</title>
<style>
/* ====== STYLE LOGIN ====== */
body{font-family:Inter,ui-sans-serif,system-ui;background:#f4f4f4;margin:0;padding:0}
.hidden{display:none}
.login-box, .main-box{max-width:1100px;margin:20px auto;padding:20px;border-radius:12px;background:#fff;box-shadow:0 4px 15px rgba(0,0,0,0.2)}
.login-box h2, .main-box h1{text-align:center;color:#0b61ff}
input{width:100%;padding:8px;margin:8px 0;border-radius:8px;border:1px solid #ccc;font-size:14px}
button{cursor:pointer}
button.logout{width:auto;padding:6px 12px;margin:0;background:#d63b3b;color:#fff;border:none;border-radius:8px;font-size:13px}
.error{color:red;font-size:13px;text-align:center;margin-top:6px}

/* ====== STYLE APLIKASI EMAS ====== */
:root{
  --blue:#0b61ff;
  --gold:#e0b300;
  --green:#12a150;
  --red:#d63b3b;
  --bg:#ffffff;
  --card:#f4f4f8;
  --muted:#555;
  --text:#222;
  --accent:#dde6f3;
}
*{box-sizing:border-box}
body.app{padding:18px;background:var(--bg);color:var(--text)}
h1{margin:0 0 14px;text-align:center;color:var(--blue);font-size:20px}
.container{display:flex;gap:12px;flex-wrap:wrap}
.left,.right{background:var(--card);border-radius:12px;padding:12px;box-shadow:0 6px 18px rgba(0,0,0,0.06)}
.left{flex:1;min-width:260px;max-width:340px}
.right{flex:2;min-width:320px}
h2{margin:0 0 8px;font-size:15px;color:var(--blue)}
label{display:block;font-size:13px;color:var(--muted);margin-bottom:6px}
input[type="number"], input[type="date"]{width:100%;padding:8px 10px;border-radius:8px;border:1px solid rgba(0,0,0,0.1);background:#fff;color:var(--text);font-size:13px}
.row{display:flex;gap:8px;flex-wrap:wrap}
.row>div{flex:1;min-width:100px}
.btn{display:inline-block;padding:7px 10px;border-radius:8px;border:none;cursor:pointer;font-size:13px}
.btn.primary{background:var(--blue);color:#fff}
.btn.gold{background:var(--gold);color:#000;font-weight:600}
.btn.ghost{background:transparent;border:1px solid var(--blue);color:var(--blue)}
.btn.small{padding:5px 8px;font-size:12px;border-radius:7px}
.total{font-size:13px;color:var(--text);margin-top:8px}
.muted{color:var(--muted);font-size:12px}
table{width:100%;border-collapse:collapse;margin-top:10px;font-size:13px}
th,td{padding:8px 6px;border-bottom:1px solid rgba(0,0,0,0.05);text-align:left}
th{background:var(--accent);color:#000;font-weight:600;font-size:13px}
tr:hover td{background:rgba(0,0,0,0.02)}
tfoot td{font-weight:700;background:rgba(200,180,0,0.1);color:var(--gold)}
.profit-pos{color:var(--green);font-weight:700}
.profit-neg{color:var(--red);font-weight:700}
.inline-edit{padding:6px;border-radius:6px;background:#eee;border:1px solid #ccc;color:var(--text)}
.controls{display:flex;gap:8px;flex-wrap:wrap}
.topbar{display:flex;justify-content:space-between;align-items:center;margin-bottom:6px}
.total-bar{display:flex;gap:12px;align-items:center}
#totalDisplay{color:var(--muted)}
@media(max-width:860px){.container{flex-direction:column}}
#showOld.active { background:#0b2e99;color:#fff;border-color:transparent }
#showAll.active { background:transparent;color:#0b61ff;border-color:var(--blue) }
/* Modal */
#modal{display:none;position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.6);justify-content:center;align-items:center;z-index:999}
#modalContent{background:var(--card);padding:20px;border-radius:12px;width:300px;max-width:90%}
#modalContent h3{margin-top:0;color:var(--blue)}
#modalContent input{margin-bottom:10px}
.modal-buttons{display:flex;justify-content:flex-end;gap:8px}
</style>
</head>
<body>

<!-- LOGIN / REGISTER -->
<div id="auth" class="login-box">
  <h2>Login / Register</h2>
  <input type="text" id="username" placeholder="Username" />
  <input type="password" id="password" placeholder="Password" />
  <button id="loginBtn">Login / Register</button>
  <div id="authError" class="error"></div>
</div>

<!-- MAIN APP -->
<div id="app" class="main-box hidden app">
  <div style="text-align:right;margin-bottom:10px">
    <button id="logoutBtn" class="logout">Logout</button>
  </div>

  <h1>💰 Tabungan Emas Pegadaian — Gold Mode Final</h1>

  <div class="container">
    <div class="left">
      <div class="card">
        <h2>Harga Sekarang / gr</h2>
        <input id="currentPrice" type="number" placeholder="Contoh: 999000" step="0.00001" />
      </div>
      <div class="card" style="margin-top:12px">
        <h2>Tambah Aset</h2>
        <form id="assetForm">
          <div class="row">
            <div><label>Gram</label><input id="grams" type="number" step="0.00001" required></div>
            <div><label>Harga Beli/gr</label><input id="priceBuy" type="number" step="0.00001" required></div>
            <div><label>Tanggal</label><input id="date" type="date" required></div>
          </div>
          <div style="margin-top:10px;display:flex;gap:8px;flex-wrap:wrap">
            <button class="btn primary" type="submit">💾 Simpan</button>
            <button id="clearAll" class="btn ghost small" type="button">🗑 Hapus Semua</button>
            <button id="zakatBtn" class="btn gold small" type="button">📿 Hitung Zakat</button>
          </div>
          <div class="total" id="leftGramTotal"></div>
          <div class="total" id="leftProfitTotal"></div>
          <div class="muted" id="leftTotalAsset"></div>
        </form>
      </div>
      <div class="card" style="margin-top:12px">
        <h2>Potong Admin</h2>
        <form id="adminForm">
          <div class="row">
            <div><label>Gram Admin</label><input id="adminGram" type="number" step="0.00001" required></div>
            <div><label>Tanggal Admin</label><input id="adminDate" type="date" required></div>
          </div>
          <div style="margin-top:10px">
            <button class="btn gold small" type="submit">🛠 Terapkan Admin</button>
          </div>
        </form>
      </div>
    </div>

    <div class="right">
      <div class="card">
        <div class="topbar">
          <h2 style="margin:0">Daftar Aset</h2>
          <div class="total-bar">
            <div id="totalDisplay" class="muted">0 g • Rp0</div>
          </div>
        </div>

        <div style="margin:8px 0" class="controls">
          <button id="showAll" class="btn small ghost active">🌍 Semua</button>
          <button id="showOld" class="btn small ghost">⏳ ≥ 1 Tahun</button>
          <button id="backupJSON" class="btn small gold">📦 Backup JSON</button>
          <input type="file" id="restoreFile" accept=".json" style="display:none">
          <button id="restoreJSON" class="btn small ghost">♻️ Restore</button>
        </div>

        <table id="table">
          <thead>
            <tr>
              <th>No</th><th>Gram</th><th>Tanggal</th><th>Harga / gr</th>
              <th>Nilai</th><th>Nilai Sekarang</th><th>Laba</th><th>Aksi</th>
            </tr>
          </thead>
          <tbody></tbody>
          <tfoot>
            <tr>
              <td><b>Total</b></td>
              <td id="footGram">0</td>
              <td colspan="2"></td>
              <td id="footBuy">Rp0</td>
              <td id="footNow">Rp0</td>
              <td id="footProfit">Rp0</td>
              <td></td>
            </tr>
          </tfoot>
        </table>

        <div class="total" id="profitTotal" style="margin-top:10px"></div>
        <div class="total muted" id="valueTotal"></div>
      </div>
    </div>

  </div>

  <div style="text-align:center;margin-top:12px;color:var(--muted)">
    Data tersimpan otomatis di browser (<b>per user</b>)
  </div>

</div>

<script>
// =========================
// SIMPLE AUTH
// =========================
const authBox = document.getElementById('auth');
const appBox = document.getElementById('app');
const loginBtn = document.getElementById('loginBtn');
const logoutBtn = document.getElementById('logoutBtn');
const usernameInput = document.getElementById('username');
const passwordInput = document.getElementById('password');
const authError = document.getElementById('authError');

const LS_USERS = 'goldUsers';
let currentUser = localStorage.getItem('goldCurrentUser') || null;

function getUsers(){ return JSON.parse(localStorage.getItem(LS_USERS)||'{}'); }
function saveUsers(users){ localStorage.setItem(LS_USERS, JSON.stringify(users)); }

loginBtn.onclick = function(){
  const u = usernameInput.value.trim();
  const p = passwordInput.value.trim();
  if(!u || !p){ authError.textContent='Isi username & password!'; return; }
  
  let users = getUsers();
  if(users[u]){
    if(users[u] !== p){ authError.textContent='Password salah!'; return; }
  } else {
    users[u] = p; saveUsers(users);
  }
  currentUser = u; localStorage.setItem('goldCurrentUser', u);
  showApp();
}

logoutBtn.onclick = function(){
  localStorage.removeItem('goldCurrentUser'); currentUser=null; showAuth();
}

function showApp(){ authBox.classList.add('hidden'); appBox.classList.remove('hidden'); loadUserAssets(); }
function showAuth(){ authBox.classList.remove('hidden'); appBox.classList.add('hidden'); }

// =========================
// LOAD USER ASSETS
// =========================
let LS_KEY='goldAssets_final';
let assets=[];
function loadUserAssets(){
  LS_KEY = 'goldAssets_'+currentUser;
  assets = JSON.parse(localStorage.getItem(LS_KEY)||"[]");
  render(); renderLeftTotals();
}

// =========================
// SCRIPT EMAS LENGKAP
// =========================
const $=id=>document.getElementById(id);
const tbody = document.querySelector("#table tbody");

function save(){ localStorage.setItem(LS_KEY,JSON.stringify(assets)); }
function formatRp(n){ return "Rp"+Math.round(n||0).toString().replace(/\B(?=(\d{3})+(?!\d))/g,"."); }
function isOld(d){ const dt=new Date(d); return !isNaN(dt) && (new Date()-dt)>=365*24*60*60*1000; }
function normalizeItem(item){
  return {
    id: item.id || Date.now().toString(36)+Math.random().toString(36).slice(2,6),
    grams: Number(item.grams)||0,
    priceBuy: Number(item.priceBuy)||0,
    date: item.date||new Date().toISOString().slice(0,10),
    type: item.type ? String(item.type) : (Number(item.grams)<0?"admin":"buy"),
    parentId: item.parentId||null
  };
}

/* =========================
   Render Left Totals
========================= */
function renderLeftTotals(){
  const currentPrice = +$("currentPrice").value || 0;
  let totalGram = 0, totalBuy = 0, totalNow = 0, totalProfit = 0;

  assets.forEach(a => {
    if(a.type === "buy"){
      const soldGram = assets.filter(x => x.type === "sell" && x.parentId === a.id)
                             .reduce((s,x)=>s + x.grams,0);
      const rem = a.grams - soldGram;
      const nowVal = rem * currentPrice; 
      const profit = nowVal - rem * a.priceBuy;
      totalGram += rem;
      totalBuy += rem * a.priceBuy;
      totalNow += nowVal;
      totalProfit += profit;
    } else if(a.type === "admin"){
      totalGram += a.grams; 
      totalNow += a.grams * currentPrice; 
      totalBuy += 30000; 
      totalProfit += (a.grams * currentPrice) - 30000; 
    }
  });

  $("leftGramTotal").textContent=`Saldo Emas: ${totalGram.toFixed(5)} g`;
  $("leftProfitTotal").innerHTML = totalProfit>=0 ? `<span class="profit-pos">Laba: ${formatRp(totalProfit)}</span>` : `<span class="profit-neg">Rugi: ${formatRp(totalProfit)}</span>`;
  $("leftTotalAsset").textContent=`💰 Total Aset (harga sekarang): ${formatRp(totalNow)}`;
}

/* =========================
   Render Table
========================= */
function render(){
  tbody.innerHTML=""; 
  const currentPrice = +$("currentPrice").value || 0;
  let totalGram=0,totalBuy=0,totalNow=0,totalProfit=0;

  let filtered = assets.filter(a=>a.type==="buy"||a.type==="admin");
  if(filterMode==="old"){
    filtered = filtered.filter(a=>{ const soldGram = assets.filter(x=>x.type==="sell" && x.parentId===a.id)
                             .reduce((s,x)=>s+x.grams,0); const rem = a.grams - soldGram; return a.type==="admin" || (isOld(a.date) && rem>0);});
  }
  filtered.sort((a,b)=> new Date(a.date)-new Date(b.date));

  filtered.forEach((a,i)=>{
    if(a.type==="admin"){
      const trAdmin=document.createElement("tr");
      trAdmin.dataset.id=a.id;
      trAdmin.style.color="var(--red)";
      trAdmin.innerHTML=`<td>-</td><td>${a.grams.toFixed(5)}</td><td>${a.date}</td><td>${formatRp(30000)}</td><td>${formatRp(30000)}</td><td>${formatRp(30000)}</td><td>${formatRp(-30000)}</td><td>Admin Fee <button class="btn small ghost editAdmin">✏️</button> <button class="btn small ghost delAdmin">❌</button></td>`;
      tbody.appendChild(trAdmin);
      totalGram += a.grams; totalBuy += 30000; totalProfit -= 30000; return;
    }
    const sold = assets.filter(x=>x.type==="sell" && x.parentId===a.id);
    const soldGram = sold.reduce((s,x)=>s+x.grams,0);
    const rem = a.grams - soldGram;
    const nowVal = rem*(currentPrice||a.priceBuy);
    const profit = nowVal - rem*a.priceBuy;

    const tr=document.createElement("tr");
    tr.dataset.id=a.id;
    tr.style.color=(soldGram>0?"var(--red)":profit>0?"var(--green)":profit<0?"var(--red)":"var(--text)");
    if(soldGram>0) tr.style.textDecoration="line-through";
    tr.innerHTML=`<td>${i+1}</td><td>${a.grams.toFixed(5)}</td><td>${a.date}</td><td>${formatRp(a.priceBuy)}</td><td>${formatRp(a.grams*a.priceBuy)}</td><td>${formatRp(nowVal)}</td><td>${formatRp(profit)}</td><td><button class="btn small ghost editBtn">✏️ Edit</button> <button class="btn small ghost sellBtn">💰 Jual</button> <button class="btn small ghost" onclick="delAsset('${a.id}')">❌</button></td>`;
    tbody.appendChild(tr);
    sold.forEach(s=>{
      const trSell=document.createElement("tr");
      trSell.dataset.id=s.id;
      trSell.style.color="var(--blue)";
      trSell.innerHTML=`<td>${i+1}</td><td>${s.grams.toFixed(5)}</td><td>${s.date}</td><td>${formatRp(s.priceBuy)}</td><td>${formatRp(s.grams*s.priceBuy)}</td><td>${formatRp(s.grams*currentPrice)}</td><td>${formatRp((s.grams*currentPrice)-(s.grams*s.priceBuy))}</td><td>Jual</td>`;
      tbody.appendChild(trSell);
    });

    totalGram += rem;
    totalBuy += rem*a.priceBuy;
    totalNow += nowVal;
    totalProfit += profit;
  });

  $("footGram").textContent=totalGram.toFixed(5);
  $("footBuy").textContent=formatRp(totalBuy);
  $("footNow").textContent=formatRp(totalNow);
  $("footProfit").textContent=formatRp(totalProfit);
  $("profitTotal").innerHTML=totalProfit>=0?`<span class="profit-pos">Total Laba: ${formatRp(totalProfit)}</span>`:`<span class="profit-neg">Total Rugi: ${formatRp(totalProfit)}</span>`;
  $("valueTotal").textContent=`Total Nilai Sekarang: ${formatRp(totalNow)}`;
}

/* =========================
   ADD ASSET
========================= */
$("assetForm").onsubmit=function(e){
  e.preventDefault();
  const grams=+$("grams").value;
  const priceBuy=+$("priceBuy").value;
  const date=$("date").value;
  assets.push(normalizeItem({grams,priceBuy,date}));
  save(); render(); renderLeftTotals();
  this.reset();
}

/* =========================
   ADMIN FORM
========================= */
$("adminForm").onsubmit=function(e){
  e.preventDefault();
  const grams=+$("adminGram").value;
  const date=$("adminDate").value;
  assets.push(normalizeItem({grams,date,type:"admin"}));
  save(); render(); renderLeftTotals();
  this.reset();
}

/* =========================
   CLEAR ALL
========================= */
$("clearAll").onclick=function(){ if(confirm("Hapus semua data?")){ assets=[]; save(); render(); renderLeftTotals();} }

/* =========================
   ZAKAT
========================= */
$("zakatBtn").onclick=function(){
  const currentPrice=+$("currentPrice").value;
  if(!currentPrice){ alert("Isi harga sekarang dulu!"); return; }
  const totalGram = assets.filter(a=>a.type==="buy").reduce((s,a)=>s+a.grams,0);
  const zakat=totalGram*0.025*currentPrice;
  alert(`Zakat 2.5% dari saldo emas: ${formatRp(zakat)}`);
}

/* =========================
   FILTER MODES
========================= */
let filterMode="all";
$("showAll").onclick=function(){ filterMode="all"; this.classList.add("active"); $("showOld").classList.remove("active"); render(); }
$("showOld").onclick=function(){ filterMode="old"; this.classList.add("active"); $("showAll").classList.remove("active"); render(); }

/* =========================
   BACKUP / RESTORE
========================= */
$("backupJSON").onclick=function(){
  const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(assets));
  const a=document.createElement("a");
  a.setAttribute("href",dataStr);
  a.setAttribute("download","gold_assets_"+currentUser+".json");
  document.body.appendChild(a); a.click(); a.remove();
}
$("restoreJSON").onclick=function(){$("restoreFile").click();}
$("restoreFile").onchange=function(e){
  const f=e.target.files[0]; const reader=new FileReader();
  reader.onload=function(ev){ try{ assets=JSON.parse(ev.target.result); save(); render(); renderLeftTotals(); }catch(err){alert("File salah!")} }
  reader.readAsText(f);
}

/* =========================
   DELETE ASSET
========================= */
function delAsset(id){
  if(confirm("Hapus aset ini?")){
    assets = assets.filter(a=>a.id!==id && a.parentId!==id);
    save(); render(); renderLeftTotals();
  }
}

/* =========================
   CURRENT PRICE CHANGE
========================= */
$("currentPrice").oninput=function(){ render(); renderLeftTotals(); }

/* =========================
   INITIAL SHOW
========================= */
if(currentUser){ showApp(); } else { showAuth(); }
</script>
</body>
</html>
