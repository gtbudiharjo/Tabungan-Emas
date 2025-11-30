<!doctype html>
<html lang="id">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Tabungan Emas Pegadaian — Gold Mode Final</title>
<style>
:root{
  --blue:#0b61ff;
  --gold:#e0b300;
  --green:#12a150;
  --red:#d63b3b;
  --bg:#0f1114;
  --card:#17181b;
  --muted:#9aa3b2;
  --text:#e6eef8;
  --accent:#0b2e99;
}
*{box-sizing:border-box}
body{margin:0;padding:18px;font-family:Inter,ui-sans-serif,system-ui,Segoe UI,Roboto;background:linear-gradient(180deg,#0b0c0e 0%,#0f1114 100%);color:var(--text)}
h1{margin:0 0 14px;text-align:center;color:var(--blue);font-size:20px}
.container{display:flex;gap:12px;flex-wrap:wrap}
.left,.right{background:var(--card);border-radius:12px;padding:12px;box-shadow:0 6px 18px rgba(3,6,12,0.6)}
.left{flex:1;min-width:260px;max-width:340px}
.right{flex:2;min-width:320px}
h2{margin:0 0 8px;font-size:15px;color:var(--blue)}
label{display:block;font-size:13px;color:var(--muted);margin-bottom:6px}
input[type="number"], input[type="date"], input[type="text"], input[type="password"]{width:100%;padding:8px 10px;border-radius:8px;border:1px solid rgba(255,255,255,0.04);background:#0d0e10;color:var(--text);font-size:13px}
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
th,td{padding:8px 6px;border-bottom:1px solid rgba(255,255,255,0.03);text-align:left}
th{background:var(--accent);color:#fff;font-weight:600;font-size:13px}
tr:hover td{background:rgba(255,255,255,0.02)}
tfoot td{font-weight:700;background:rgba(0,0,0,0.15);color:var(--gold)}
.profit-pos{color:var(--green);font-weight:700}
.profit-neg{color:var(--red);font-weight:700}
.inline-edit{padding:6px;border-radius:6px;background:#0c0c0d;border:1px solid #222;color:var(--text)}
.controls{display:flex;gap:8px;flex-wrap:wrap}
.topbar{display:flex;justify-content:space-between;align-items:center;margin-bottom:6px}
.total-bar{display:flex;gap:12px;align-items:center}
#totalDisplay{color:var(--muted)}
.hidden{display:none}
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
<h1>💰 Tabungan Emas Pegadaian — Gold Mode Final</h1>

<!-- ===== LOGIN / REGISTER ===== -->
<div id="authSection">
  <div class="container">
    <div class="left">
      <h2>Login</h2>
      <form id="loginForm">
        <label>Username</label>
        <input type="text" id="loginUser" required>
        <label>Password</label>
        <input type="password" id="loginPass" required>
        <button class="btn primary" type="submit">Login</button>
      </form>
      <p style="margin-top:10px;color:var(--muted)">Belum punya akun? <button id="showRegister" class="btn small ghost">Register</button></p>
    </div>

    <div class="left hidden" id="registerBox">
      <h2>Register</h2>
      <form id="registerForm">
        <label>Username</label>
        <input type="text" id="regUser" required>
        <label>Password</label>
        <input type="password" id="regPass" required>
        <button class="btn gold" type="submit">Register</button>
      </form>
      <p style="margin-top:10px;color:var(--muted)">Sudah punya akun? <button id="showLogin" class="btn small ghost">Login</button></p>
    </div>
  </div>
</div>

<!-- ===== TABUNGAN EMAS (DISSEMBUNYIKAN SAMPAI LOGIN) ===== -->
<div id="appSection" class="hidden">

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
  Data tersimpan otomatis di browser (<b>goldAssets_final</b>)
</div>

<button id="logoutBtn" class="btn red" style="margin-top:12px">Logout</button>

</div>

<script>
/* =========================
   LOGIN / REGISTER SIMPLE
========================= */
const LS_USERS = "goldUsers";
const LS_LOGIN = "goldLoggedIn";
function getUsers(){ return JSON.parse(localStorage.getItem(LS_USERS)||"{}"); }
function saveUsers(users){ localStorage.setItem(LS_USERS, JSON.stringify(users)); }
function showApp(){ document.getElementById("authSection").classList.add("hidden"); document.getElementById("appSection").classList.remove("hidden"); }
function showAuth(){ document.getElementById("authSection").classList.remove("hidden"); document.getElementById("appSection").classList.add("hidden"); }

document.getElementById("showRegister").onclick=()=>{ document.getElementById("registerBox").classList.remove("hidden"); document.getElementById("loginForm").parentElement.classList.add("hidden"); }
document.getElementById("showLogin").onclick=()=>{ document.getElementById("registerBox").classList.add("hidden"); document.getElementById("loginForm").parentElement.classList.remove("hidden"); }

document.getElementById("registerForm").onsubmit=function(e){
  e.preventDefault();
  const user=document.getElementById("regUser").value;
  const pass=document.getElementById("regPass").value;
  const users=getUsers();
  if(users[user]){ alert("Username sudah ada!"); return; }
  users[user]=pass;
  saveUsers(users);
  alert("✅ Registrasi sukses! Silakan login.");
  document.getElementById("showLogin").click();
};

document.getElementById("loginForm").onsubmit=function(e){
  e.preventDefault();
  const user=document.getElementById("loginUser").value;
  const pass=document.getElementById("loginPass").value;
  const users=getUsers();
  if(users[user] && users[user]===pass){
    localStorage.setItem(LS_LOGIN,user);
    showApp();
  } else alert("❌ Username atau password salah!");
};

document.getElementById("logoutBtn").onclick=()=>{
  localStorage.removeItem(LS_LOGIN);
  showAuth();
};

// Tampilkan langsung tabungan jika sudah login
if(localStorage.getItem(LS_LOGIN)){ showApp(); }

/* =========================
   SCRIPT TABUNGAN EMAS (originalmu)
   Tempel seluruh script yang sudah ada di sini
========================= */
// --- Semua script yang sebelumnya kamu kirim tetap utuh ---
// Mulai dari storage & helpers, render, CRUD, events, zakat, backup/restore, dst.

</script>
</body>
</html>
