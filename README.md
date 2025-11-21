<html lang="id">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<style>
:root{
  --blue:#0a56d8;
  --gold:#d4a600;
  --green:#0f8c45;
  --red:#d43c3c;
  --bg:#f8f9fc;          /* Lebih terang */
  --card:#ffffff;        /* Kotak putih */
  --muted:#5f6a7a;       /* Lebih soft */
  --text:#1d1f23;        /* Teks lebih gelap */
  --accent:#dce6ff;      /* Header tabel lebih terang */
}

*{box-sizing:border-box}

body{
  margin:0;padding:16px;
  font-family:Inter,ui-sans-serif,system-ui,Segoe UI,Roboto;
  background:#f4f6fc;             /* Sangat terang */
  color:var(--text);
  font-size:14px;
}

h1{
  margin:0 0 10px;
  text-align:center;
  color:var(--blue);
  font-size:18px;                 /* lebih kecil */
}

.container{
  display:flex;gap:10px;flex-wrap:wrap;
}

.left,.right{
  background:var(--card);
  border-radius:10px;
  padding:10px;
  box-shadow:0 2px 8px rgba(0,0,0,0.08); /* shadow ringan */
}

.left{flex:1;min-width:230px;max-width:300px}
.right{flex:5;min-width:600px}

h2{
  margin:0 0 6px;
  font-size:12px !important;
  color:var(--blue);
}

label{
  display:block;
  font-size:12px;
  color:var(--muted);
  margin-bottom:4px;
}

input[type="number"],
input[type="date"],
input[type="text"],
input[type="password"]{
  width:100%;
  padding:6px 8px;
  border-radius:6px;
  border:1px solid #d4d7dd;
  background:#ffffff;
  color:var(--text);
  font-size:12px;
}

.row{display:flex;gap:6px;flex-wrap:wrap}
.row>div{flex:1;min-width:90px}

.btn{
  display:inline-block;
  padding:6px 8px;
  border-radius:6px;
  border:none;
  cursor:pointer;
  font-size:12px;
}

.btn.primary{background:var(--blue);color:#fff}
.btn.gold{background:var(--gold);color:#000;font-weight:600}
.btn.ghost{background:#fff;border:1px solid var(--blue);color:var(--blue)}
.btn.small{padding:4px 6px;font-size:11px;border-radius:5px}

.total{font-size:12px;color:var(--text);margin-top:6px}
.muted{color:var(--muted);font-size:11px}

table{
  width:100%;
  border-collapse:collapse;
  margin-top:8px;
  font-size:12px;
}

th,td{
  padding:6px 5px;
  border-bottom:1px solid #e4e6eb;
  text-align:left;
}

th{
  background:var(--accent);
  color:#1a1a1a;
  font-weight:600;
  font-size:12px;
}

tr:hover td{
  background:#f0f2f7;
}

tfoot td{
  font-weight:700;
  background:#f2f2f2;
  color:var(--gold);
}

.profit-pos{color:var(--green);font-weight:600}
.profit-neg{color:var(--red);font-weight:600}

.inline-edit{
  padding:5px;
  border-radius:5px;
  background:#ffffff;
  border:1px solid #ccc;
  color:var(--text);
  font-size:12px;
}

.controls{display:flex;gap:6px;flex-wrap:wrap}

.topbar{
  display:flex;justify-content:space-between;
  align-items:center;margin-bottom:6px;
}

.total-bar{display:flex;gap:10px;align-items:center}
#totalDisplay{color:var(--muted);font-size:12px}

@media(max-width:860px){
  .container{flex-direction:column}
}

/* Login Overlay */
#loginOverlay,#registerOverlay{
  position:fixed;top:0;left:0;width:100%;height:100%;
  background:rgba(255,255,255,0.9);
  display:flex;justify-content:center;align-items:center;
  z-index:9999;
}
</style>
</head>
<body>

<!-- LOGIN OVERLAY -->
<div id="loginOverlay">
  <div style="background:var(--card);padding:20px;border-radius:12px;width:280px;text-align:center">
    <h2 style="margin-bottom:12px;color:var(--blue)">🔒 Login</h2>
    <input id="loginUser" type="text" placeholder="Username"><br><br>
    <input id="loginPass" type="password" placeholder="Password"><br><br>
    <button id="loginBtn" class="btn primary" style="width:100%">Login</button>
    <p style="margin-top:8px;color:var(--muted);font-size:12px">Belum punya akun? <span id="showRegister" style="color:var(--gold);cursor:pointer">Daftar</span></p>
  </div>
</div>

<!-- REGISTER OVERLAY -->
<div id="registerOverlay" style="display:none">
  <div style="background:var(--card);padding:20px;border-radius:12px;width:280px;text-align:center">
    <h2 style="margin-bottom:12px;color:var(--blue)">📝 Daftar</h2>
    <input id="regUser" type="text" placeholder="Username"><br><br>
    <input id="regPass" type="password" placeholder="Password"><br><br>
    <button id="registerBtn" class="btn gold" style="width:100%">Daftar</button>
    <p style="margin-top:8px;color:var(--muted);font-size:12px">Sudah punya akun? <span id="showLogin" style="color:var(--blue);cursor:pointer">Login</span></p>
  </div>
</div>

<h1>💰 Tabungan Emas</h1>

<div class="container">
  <div class="left">
    <!-- Harga Sekarang -->
    <div class="card">
      <h2>Harga Sekarang / gr</h2>
      <input id="currentPrice" type="number" placeholder="Contoh: 999000" step="0.00001" />
    </div>
    <!-- Tambah Aset -->
    <div class="card" style="margin-top:12px">
      <h2>Tambah Emas</h2>
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
    <!-- Admin Manual -->
    <div class="card" style="margin-top:12px">
      <h2>Potong Admin </h2>
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
    <!-- Jual Aset -->
    <div class="card" style="margin-top:12px">
      <h2>Jual Emas</h2>
      <form id="sellForm">
        <div class="row">
          <div><label>Gram</label><input id="sellGram" type="number" step="0.00001" required></div>
          <div><label>Harga Jual/gr</label><input id="sellPrice" type="number" step="0.00001" required></div>
          <div><label>Tanggal Jual</label><input id="sellDate" type="date" required></div>
        </div>
        <div style="margin-top:10px">
          <button class="btn gold" type="submit">💸 Jual</button>
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
          <button id="logoutBtn" class="btn small ghost" style="margin-left:8px">🚪 Logout</button>
        </div>
      </div>

      <div style="margin:8px 0" class="controls">
        <button id="showAll" class="btn small ghost">🌍 Semua</button>
        <button id="showOld" class="btn small ghost">⏳ ≥ 1 Tahun</button>
        <button id="backupJSON" class="btn small gold">📦 Backup JSON</button>
        <input type="file" id="restoreFile" accept=".json" style="display:none">
        <button id="restoreJSON" class="btn small ghost">♻️ Restore</button>
      </div>

      <table id="table">
        <thead>
          <tr><th>No</th><th>Gram</th><th>Tanggal</th><th>Harga Beli/Jual</th><th>Nilai Beli</th><th>Nilai Sekarang</th><th>Laba</th><th>Aksi</th></tr>
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

<div style="text-align:center;margin-top:12px;color:var(--muted)">Data tersimpan otomatis di browser (<b>goldAssets_v14</b>)</div>
<script>
// ===== LOGIN & REGISTER =====
const USER_KEY = "goldAssetsUser_v1"; 
const LS_KEY = "goldAssets_v14";

function hash(s){ return btoa(s); }

function checkLogin(){
  const loggedIn = sessionStorage.getItem("loggedIn");
  if(!loggedIn){
    document.getElementById("loginOverlay").style.display = "flex";
    document.getElementById("registerOverlay").style.display = "none";
    document.querySelector(".container").style.display = "none";
    return false;
  } else {
    document.getElementById("loginOverlay").style.display = "none";
    document.getElementById("registerOverlay").style.display = "none";
    document.querySelector(".container").style.display = "flex";
    return true;
  }
}

// Toggle login/register
document.getElementById("showRegister").onclick = ()=>{ 
  document.getElementById("loginOverlay").style.display="none";
  document.getElementById("registerOverlay").style.display="flex";
};
document.getElementById("showLogin").onclick = ()=>{ 
  document.getElementById("loginOverlay").style.display="flex";
  document.getElementById("registerOverlay").style.display="none";
};

// Register
document.getElementById("registerBtn").onclick = ()=>{
  const u = document.getElementById("regUser").value.trim();
  const p = document.getElementById("regPass").value;
  if(!u || !p) return alert("Lengkapi username & password");
  const userData = { username: u, password: hash(p) };
  localStorage.setItem(USER_KEY, JSON.stringify(userData));
  alert("Akun berhasil dibuat, silakan login!");
  document.getElementById("registerOverlay").style.display="none";
  document.getElementById("loginOverlay").style.display="flex";
};

// Login
document.getElementById("loginBtn").onclick = ()=>{
  const u = document.getElementById("loginUser").value.trim();
  const p = document.getElementById("loginPass").value;
  const stored = JSON.parse(localStorage.getItem(USER_KEY) || "{}");
  if(u === stored.username && hash(p) === stored.password){
    sessionStorage.setItem("loggedIn", "1");
    checkLogin();
  } else {
    alert("Username atau password salah!");
  }
};

// Logout
document.getElementById("logoutBtn").onclick = function(){
  sessionStorage.removeItem("loggedIn");
  location.reload();
};

// ===== SCRIPT TABUNGAN EMAS =====
let assets = JSON.parse(localStorage.getItem(LS_KEY) || "[]");
let filterMode = "all";
const $ = id => document.getElementById(id);
const tbody = document.querySelector("#table tbody");

function save(){ localStorage.setItem(LS_KEY, JSON.stringify(assets)); }
function formatRp(n){ return "Rp"+Math.round(n||0).toString().replace(/\B(?=(\d{3})+(?!\d))/g,"."); }
function isOld(d){ return (new Date() - new Date(d)) >= 365*24*60*60*1000; }
function getSaldoEmas(){ return assets.reduce((s,a)=>s + (Number(a.grams)||0), 0); }

function renderLeftTotals(){
  const currentPrice = +$("currentPrice").value || 0;
  const totalGram = getSaldoEmas();
  const totalBuy = assets.reduce((s,a)=>s + (Number(a.grams)||0) * (Number(a.priceBuy)||0), 0);
  const totalValue = totalGram * currentPrice;
  const totalProfit = totalValue - totalBuy;

  $("leftGramTotal").textContent = "Saldo Emas: " + totalGram.toFixed(5) + " g";
  if(currentPrice > 0){
    $("leftProfitTotal").innerHTML = (totalProfit >= 0 ? `<span class="profit-pos">Laba: ${formatRp(totalProfit)}</span>` : `<span class="profit-neg">Rugi: ${formatRp(totalProfit)}</span>`);
    $("leftTotalAsset").textContent = "💰 Total Aset (harga sekarang): " + formatRp(totalValue);
  } else {
    $("leftProfitTotal").textContent = "";
    $("leftTotalAsset").textContent = "";
  }
}

function render(){
  tbody.innerHTML = "";
  const currentPrice = +$("currentPrice").value || 0;
  let totalBuy = 0, totalNow = 0, totalProfit = 0;

  let filtered = (filterMode === "old") ? assets.filter(a=>isOld(a.date)) : assets.slice();
  filtered.sort((a,b)=> new Date(a.date) - new Date(b.date));

  filtered.forEach((a,i)=>{
    const isSell = a.grams < 0;
    const absGram = Math.abs(Number(a.grams || 0));
    const price = Number(a.priceBuy || 0);
    const beli = absGram * price;
    let nowValue = 0;
	if(a.type !== "admin"){
	nowValue = absGram * (currentPrice > 0 ? currentPrice : price);
	}
    const now = isSell ? -nowValue : nowValue;
    let laba = isSell ? -beli : nowValue - beli;

    totalBuy += isSell ? -beli : beli;
    totalNow += now;
    totalProfit += laba;

    const tr = document.createElement("tr");
    tr.dataset.id = a.id;

    if(a.type==="admin") tr.style.color="var(--red)";
    else if(isSell) tr.style.color='var(--red)';
    else if(laba>0) tr.style.color='var(--green)';
    else tr.style.color='var(--text)';

    const m=["Jan","Feb","Mar","Apr","Mei","Jun","Jul","Agu","Sep","Okt","Nov","Des"];
    const d = new Date(a.date || Date.now());
    const ds = `${d.getDate()} ${m[d.getMonth()]} ${d.getFullYear()}`;

    const label = a.type==="admin" ? "Admin" : (isSell ? "Jual" : "Beli");

    tr.innerHTML = `
      <td>${i+1}</td>
      <td class="grams">${Number(a.grams).toFixed(5)}</td>
      <td class="date">${ds}</td>
      <td class="priceBuy">${formatRp(price)}</td>
      <td>${formatRp(isSell ? -beli : beli)}</td>
      <td>${now < 0 ? '-' + formatRp(Math.abs(now)) : formatRp(now)}</td>
      <td>${laba < 0 ? '-' + formatRp(Math.abs(laba)) : formatRp(laba)}</td>
      <td>${label} 
        <button class="btn small ghost editBtn">✏️</button>
        <button class="btn small ghost" onclick="delAsset('${a.id}')">🗑</button>
      </td>
    `;
    tbody.appendChild(tr);

    // Edit
    tr.querySelector(".editBtn").onclick = ()=>{
      const grams = prompt("Gram:", a.grams);
      const priceB = prompt("Harga Beli/gr:", a.priceBuy);
      if(grams && priceB){
        a.grams = parseFloat(grams);
        a.priceBuy = parseFloat(priceB);
        save();
        render();
        renderLeftTotals();
      }
    };
  });

  $("footGram").textContent = getSaldoEmas().toFixed(5);
  $("footBuy").textContent = formatRp(totalBuy);
  $("footNow").textContent = formatRp(totalNow);
  $("footProfit").textContent = formatRp(totalProfit);

  $("totalDisplay").textContent = `${getSaldoEmas().toFixed(5)} g • ${formatRp(totalNow)}`;
  $("profitTotal").textContent = totalProfit >=0 ? "Laba Total: "+formatRp(totalProfit) : "Rugi Total: "+formatRp(totalProfit);
  $("valueTotal").textContent = "Nilai Total: "+formatRp(totalNow);
  renderLeftTotals();
}

// Forms
$("assetForm").onsubmit = function(e){
  e.preventDefault();
  const g = parseFloat($("grams").value);
  const p = parseFloat($("priceBuy").value);
  const d = $("date").value;
  if(isNaN(g)||isNaN(p)||!d) return alert("Lengkapi data");
  assets.push({ id: Date.now().toString(), grams:g, priceBuy:p, date:d, type:"buy" });
  save(); render(); $("assetForm").reset();
};
$("sellForm").onsubmit = function(e){
  e.preventDefault();
  const g = parseFloat($("sellGram").value);
  const p = parseFloat($("sellPrice").value);
  const d = $("sellDate").value;
  if(isNaN(g)||isNaN(p)||!d) return alert("Lengkapi data");
  assets.push({ id: Date.now().toString(), grams:-g, priceBuy:p, date:d, type:"sell" });
  save(); render(); $("sellForm").reset();
};
$("adminForm").onsubmit = function(e){
  e.preventDefault();
  const g = parseFloat($("adminGram").value);
  const d = $("adminDate").value;
  if(isNaN(g)||!d) return alert("Lengkapi data");
  assets.push({ id: Date.now().toString(), grams:-g, priceBuy:0, date:d, type:"admin" });
  save(); render(); $("adminForm").reset();
};

// Controls
$("clearAll").onclick = ()=>{
  if(confirm("Hapus semua aset?")){ assets=[]; save(); render(); }
};
$("showAll").onclick = ()=>{ filterMode="all"; render(); };
$("showOld").onclick = ()=>{ filterMode="old"; render(); };
$("currentPrice").oninput = render;

// Zakat
$("zakatBtn").onclick = function(){
  const totalGram = getSaldoEmas();
  const nishab = 85; // gram
  if(totalGram>=nishab){
    const zakat = totalGram*0.025;
    alert("Zakat Wajib: "+zakat.toFixed(5)+" g");
  } else {
    alert("Belum wajib zakat (nishab 85g)");
  }
};

// Backup / Restore
$("backupJSON").onclick = ()=>{ 
  const blob = new Blob([JSON.stringify(assets,null,2)],{type:"application/json"});
  const a = document.createElement("a"); a.href = URL.createObjectURL(blob); a.download="goldAssetsBackup.json"; a.click();
};
$("restoreJSON").onclick = ()=>{$("restoreFile").click();};
$("restoreFile").onchange = function(e){
  const file = e.target.files[0];
  if(!file) return;
  const reader = new FileReader();
  reader.onload = ()=>{ 
    try{ assets = JSON.parse(reader.result); save(); render(); alert("Restore berhasil"); } 
    catch{ alert("File tidak valid"); }
  };
  reader.readAsText(file);
};

// Delete single asset
function delAsset(id){ 
  if(confirm("Hapus aset ini?")){ assets=assets.filter(a=>a.id!==id); save(); render(); }
}

checkLogin(); 
render();
</script>
</body>
</html>
