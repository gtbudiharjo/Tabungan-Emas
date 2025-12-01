<Semangat Ya Nabungnya html>
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
  --bg:#f5f5f5;       /* Background utama terang */
  --card:#ffffff;      /* Card putih */
  --muted:#6b7280;     /* Warna teks sekunder lebih gelap */
  --text:#111827;      /* Warna teks utama gelap */
  --accent:#dbeafe;    /* Accent untuk header tabel, lebih terang */
}
*{box-sizing:border-box}
body{margin:0;padding:18px;font-family:Inter,ui-sans-serif,system-ui,Segoe UI,Roboto;background:linear-gradient(180deg,#ffffff 0%,#f5f5f5 100%);color:var(--text)}
h1{margin:0 0 14px;text-align:center;color:var(--blue);font-size:20px}
.container{display:flex;gap:12px;flex-wrap:wrap}
.left,.right{background:var(--card);border-radius:12px;padding:12px;box-shadow:0 6px 18px rgba(0,0,0,0.1)}
.left{flex:1;min-width:260px;max-width:340px}
.right{flex:2;min-width:320px}
h2{margin:0 0 8px;font-size:15px;color:var(--blue)}
label{display:block;font-size:13px;color:var(--muted);margin-bottom:6px}
input[type="number"], input[type="date"]{width:100%;padding:8px 10px;border-radius:8px;border:1px solid rgba(0,0,0,0.1);background:#fdfdfd;color:var(--text);font-size:13px}
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
th{background:var(--accent);color:var(--text);font-weight:600;font-size:13px}
tr:hover td{background:rgba(0,0,0,0.03)}
tfoot td{font-weight:700;background:rgba(0,0,0,0.05);color:var(--gold)}
.profit-pos{color:var(--green);font-weight:700}
.profit-neg{color:var(--red);font-weight:700}
.inline-edit{padding:6px;border-radius:6px;background:#f3f4f6;border:1px solid #d1d5db;color:var(--text)}
.controls{display:flex;gap:8px;flex-wrap:wrap}
.topbar{display:flex;justify-content:space-between;align-items:center;margin-bottom:6px}
.total-bar{display:flex;gap:12px;align-items:center}
#totalDisplay{color:var(--muted)}
@media(max-width:860px){.container{flex-direction:column}}
#showOld.active { background:#0b61ff;color:#fff;border-color:transparent }
#showAll.active { background:transparent;color:var(--blue);border-color:var(--blue) }
/* Modal */
#modal{display:none;position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.2);justify-content:center;align-items:center;z-index:999}
#modalContent{background:var(--card);padding:20px;border-radius:12px;width:300px;max-width:90%}
#modalContent h3{margin-top:0;color:var(--blue)}
#modalContent input{margin-bottom:10px}
.modal-buttons{display:flex;justify-content:flex-end;gap:8px}
</style>
</head>
<body>

<!-- LOGIN / REGISTER -->
<div id="authBox">
  <h2>Login</h2>
  <form id="loginForm">
    <label>Username</label>
    <input type="text" id="loginUser" required>
    <label>Password</label>
    <input type="password" id="loginPass" required>
    <button class="btn primary" type="submit">Login</button>
  </form>
  <small id="toggleRegister">Belum punya akun? Register</small>
</div>

<!-- MAIN APP -->
<div id="mainApp" style="display:none">
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
    <button id="logoutBtn" class="btn ghost small" style="margin-top:12px;width:100%">🚪 Logout</button>
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
</div>

<script>
/* =========================
   GLOBAL VARS
========================= */
let currentUser = null;
const USERS_KEY = "goldUsers_final";
let users = JSON.parse(localStorage.getItem(USERS_KEY) || "[]");
const LS_KEY="goldAssets_final";
let assets = JSON.parse(localStorage.getItem(LS_KEY)||"[]");
let filterMode="all";

const authBox = document.getElementById("authBox");
const mainApp = document.getElementById("mainApp");
const loginForm = document.getElementById("loginForm");
const toggleRegister = document.getElementById("toggleRegister");
const tbody = document.querySelector("#table tbody");

const $=id=>document.getElementById(id);

/* =========================
   LOGIN / REGISTER
========================= */
let isRegister=false;

toggleRegister.onclick = ()=>{
  isRegister=!isRegister;
  authBox.querySelector("h2").textContent = isRegister ? "Register" : "Login";
  loginForm.querySelector("button").textContent = isRegister ? "Register" : "Login";
  toggleRegister.textContent = isRegister ? "Sudah punya akun? Login" : "Belum punya akun? Register";
  loginForm.reset();
};

loginForm.onsubmit = function(e){
  e.preventDefault();
  const username = $("loginUser").value.trim();
  const password = $("loginPass").value.trim();
  if(!username||!password) return alert("Username dan password harus diisi!");
  if(isRegister){
    if(users.find(u=>u.username===username)) return alert("❌ Username sudah terdaftar!");
    users.push({username,password});
    localStorage.setItem(USERS_KEY,JSON.stringify(users));
    alert("✅ Registrasi berhasil! Silahkan login.");
    toggleRegister.click();
  }else{
    const user = users.find(u=>u.username===username&&u.password===password);
    if(user){
      currentUser=user.username;
      authBox.style.display="none";
      mainApp.style.display="block";
      render(); renderLeftTotals();
    }else alert("❌ Username / password salah!");
  }
};

$("logoutBtn").onclick = ()=>{
  currentUser=null;
  authBox.style.display="block";
  mainApp.style.display="none";
  loginForm.reset();
};

/* =========================
   HELPERS
========================= */
function save(){ localStorage.setItem(LS_KEY,JSON.stringify(assets)); }
function formatRp(n){ return "Rp"+Math.round(n||0).toString().replace(/\B(?=(\d{3})+(?!\d))/g,"."); }
function isOld(d){ const dt=new Date(d); return !isNaN(dt) && (new Date()-dt)>=365*24*60*60*1000; }
function normalizeItem(item){ return { id: item.id||Date.now().toString(36)+Math.random().toString(36).slice(2,6), grams:Number(item.grams)||0, priceBuy:Number(item.priceBuy)||0, date:item.date||new Date().toISOString().slice(0,10), type:item.type?String(item.type):(Number(item.grams)<0?"admin":"buy"), parentId:item.parentId||null }; }

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
      const nowVal = rem * currentPrice; // gunakan harga sekarang, selalu
      const profit = nowVal - rem * a.priceBuy;
      totalGram += rem;
      totalBuy += rem * a.priceBuy;
      totalNow += nowVal;
      totalProfit += profit;
    } else if(a.type === "admin"){
      totalGram += a.grams; // admin biasanya minus
      totalNow += a.grams * currentPrice; // gunakan harga sekarang
      totalBuy += 30000; // atau bisa tetap 0, terserah logika beli admin
      totalProfit += (a.grams * currentPrice) - 30000; // agar laba/rugi konsisten
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
  tbody.innerHTML=""; // reset
  const currentPrice = +$("currentPrice").value || 0;
  let totalGram=0,totalBuy=0,totalNow=0,totalProfit=0;

  // Filter
  let filtered = assets.filter(a=>a.type==="buy"||a.type==="admin");
  if(filterMode==="old"){
    filtered = filtered.filter(a=>{
      const soldGram = assets.filter(x=>x.type==="sell" && x.parentId===a.id)
                             .reduce((s,x)=>s+x.grams,0);
      const rem = a.grams - soldGram;
      return a.type==="admin" || (isOld(a.date) && rem>0);
    });
  }

  filtered.sort((a,b)=> new Date(a.date)-new Date(b.date));

  filtered.forEach((a,i)=>{
    if(a.type==="admin"){
      const trAdmin=document.createElement("tr");
      trAdmin.dataset.id=a.id;
      trAdmin.style.color="var(--red)";
      trAdmin.innerHTML=`
        <td>-</td>
        <td>${a.grams.toFixed(5)}</td>
        <td>${a.date}</td>
        <td>${formatRp(30000)}</td>
        <td>${formatRp(30000)}</td>
        <td>${formatRp(30000)}</td>
        <td>${formatRp(-30000)}</td>
        <td>
          Admin Fee
          <button class="btn small ghost editAdmin">✏️</button>
          <button class="btn small ghost delAdmin">❌</button>
        </td>
      `;
      tbody.appendChild(trAdmin);
      totalGram += a.grams;
      totalBuy += 30000;
      totalProfit -= 30000;
      return;
    }

    // Buy
    const sold = assets.filter(x=>x.type==="sell" && x.parentId===a.id);
    const soldGram = sold.reduce((s,x)=>s+x.grams,0);
    const rem = a.grams - soldGram;
    const nowVal = rem*(currentPrice||a.priceBuy);
    const profit = nowVal - rem*a.priceBuy;

    const tr=document.createElement("tr");
    tr.dataset.id=a.id;
    tr.style.color=(soldGram>0?"var(--red)":profit>0?"var(--green)":profit<0?"var(--red)":"var(--text)");
    if(soldGram>0) tr.style.textDecoration="line-through";
    tr.innerHTML=`
      <td>${i+1}</td>
      <td>${a.grams.toFixed(5)}</td>
      <td>${a.date}</td>
      <td>${formatRp(a.priceBuy)}</td>
      <td>${formatRp(a.grams*a.priceBuy)}</td>
      <td>${formatRp(nowVal)}</td>
      <td>${formatRp(profit)}</td>
      <td>
        <button class="btn small ghost editBtn">✏️ Edit</button>
        <button class="btn small ghost sellBtn">💰 Jual</button>
        <button class="btn small ghost" onclick="delAsset('${a.id}')">❌</button>
      </td>
    `;
    tbody.appendChild(tr);

    // Historical sell
    sold.forEach(s=>{
      const trSell=document.createElement("tr");
      trSell.dataset.id=s.id;
      trSell.style.color="var(--blue)";
      trSell.innerHTML=`
        <td>${i+1}</td>
        <td>${s.grams.toFixed(5)}</td>
        <td>${s.date}</td>
        <td>${formatRp(s.priceBuy)}</td>
        <td>${formatRp(s.grams*s.priceBuy)}</td>
        <td>-</td>
        <td>-</td>
        <td>Historical Sell</td>
      `;
      tbody.appendChild(trSell);
    });

    // Remaining buy
    if(rem>0 && soldGram>0){
      const trRemain=document.createElement("tr");
      trRemain.dataset.id=a.id+"_remain";
      trRemain.style.color=(profit>=0?"var(--green)":"var(--red)");
      trRemain.innerHTML=`
        <td>${i+1}</td>
        <td>${rem.toFixed(5)}</td>
        <td>${a.date}</td>
        <td>${formatRp(a.priceBuy)}</td>
        <td>${formatRp(rem*a.priceBuy)}</td>
        <td>${formatRp(nowVal)}</td>
        <td>${formatRp(profit)}</td>
        <td>
          <button class="btn small ghost editBtn">✏️ Edit</button>
          <button class="btn small ghost sellBtn">💰 Jual</button>
        </td>
      `;
      tbody.appendChild(trRemain);
    }

    totalGram += rem;
    totalBuy += rem*a.priceBuy;
    totalNow += nowVal;
    totalProfit += profit;
  });

  // ===== HITUNG TOTAL SEKARANG AKURAT =====
const totalNowAccurate = totalGram * (currentPrice || 0);
const totalProfitAccurate = totalNowAccurate - totalBuy;

// ===== UPDATE FOOTER =====
$("footGram").textContent = totalGram.toFixed(5);                 // Total gram tersisa
$("footBuy").textContent = formatRp(totalBuy);                    // Total beli
$("footNow").textContent = formatRp(totalNowAccurate);            // Nilai sekarang akurat
$("footProfit").textContent = formatRp(totalProfitAccurate);      // Profit akurat
$("totalDisplay").textContent = `${totalGram.toFixed(5)} g • ${formatRp(totalNowAccurate)}`;
$("profitTotal").textContent = `💹 Keuntungan: ${formatRp(totalProfitAccurate)}`;
$("valueTotal").textContent = `💰 Beli: ${formatRp(totalBuy)} | Nilai Sekarang: ${formatRp(totalNowAccurate)}`;

}

/* =========================
   CRUD & Events
========================= */
function delAsset(id){
  if(confirm("Hapus transaksi?")){
    assets = assets.filter(a=>a.id!==id && a.parentId!==id);
    save(); render(); renderLeftTotals();
  }
}

tbody.addEventListener("click", e => {
  const tr = e.target.closest("tr");
  if (!tr) return;
  let id = tr.dataset.id;
  let a = assets.find(x => x.id === id);

  // Jika baris _remain
  if (!a && id.endsWith("_remain")) {
    const parentId = id.replace("_remain","");
    a = assets.find(x => x.id === parentId);
    id = parentId;
  }
  if (!a) return;

  /* =====================
        EDIT DATA BUY
  ===================== */
  if (e.target.classList.contains("editBtn") && a.type==="buy") {
    const newGram = +prompt("Ubah gram:", a.grams);
    const newPrice = +prompt("Ubah harga beli/gr:", a.priceBuy);
    const newDate = prompt("Ubah tanggal (YYYY-MM-DD):", a.date);

    if (!newGram || !newPrice || !newDate) return;

    // Cek apakah sudah ada penjualan → tidak boleh kurang dari total terjual
    const soldGram = assets.filter(x=>x.type==="sell" && x.parentId===a.id)
                           .reduce((s,x)=>s+x.grams,0);

    if (newGram < soldGram) {
      alert("❌ Gram baru tidak boleh lebih kecil dari total gram yang sudah dijual!");
      return;
    }

    a.grams = newGram;
    a.priceBuy = newPrice;
    a.date = newDate;

    save(); render(); renderLeftTotals();
  }

  /* =====================
      EDIT DATA ADMIN
  ===================== */
  if (e.target.classList.contains("editAdmin") && a.type==="admin") {
    const g = +prompt("Ubah gram admin (angka minus):", a.grams);
    const d = prompt("Tanggal admin (YYYY-MM-DD):", a.date);
    if (!g || !d) return;
    a.grams = g > 0 ? -g : g;
    a.date = d;
    save(); render(); renderLeftTotals();
  }

  /* =====================
           SELL
  ===================== */
  if (e.target.classList.contains("sellBtn")) {
    const rem = a.grams - assets.filter(x=>x.type==="sell" && x.parentId===a.id)
                                .reduce((s,x)=>s+x.grams,0);
    const grams = +prompt("Gram dijual (tersisa "+rem.toFixed(5)+" g):", rem.toFixed(5));
    const price = +prompt("Harga jual/gr:", a.priceBuy);
    const date = prompt("Tanggal jual (YYYY-MM-DD):", new Date().toISOString().slice(0,10));

    if(grams>0 && grams<=rem && price && date){
      assets.push({id:Date.now().toString(36), type:"sell", grams, priceBuy:price, date, parentId:a.id});
      save(); render(); renderLeftTotals();
    }
  }
});

$("assetForm").onsubmit = function(e){
  e.preventDefault();
  const grams= +$("grams").value;
  const price= +$("priceBuy").value;
  const date= $("date").value;
  assets.push({id:Date.now().toString(36), grams, priceBuy:price, date, type:"buy"});
  save(); render(); renderLeftTotals();
  this.reset();
};

$("adminForm").onsubmit = function(e){
  e.preventDefault();
  const grams = -Math.abs(+$("adminGram").value);
  const date = $("adminDate").value;
  assets.push({id:Date.now().toString(36), grams, priceBuy:30000, date, type:"admin"});
  save(); render(); renderLeftTotals();
  this.reset();
};

$("clearAll").onclick = ()=>{ if(confirm("Hapus semua data?")){ assets=[]; save(); render(); renderLeftTotals(); } };
$("currentPrice").oninput = ()=>{ render(); renderLeftTotals(); };

/* =========================
   Backup & Restore
========================= */
$("backupJSON").onclick=()=>{
  const blob = new Blob([JSON.stringify(assets)],{type:"application/json"});
  const a = document.createElement("a");
  a.href=URL.createObjectURL(blob);
  a.download="Pegadaian.json";
  a.click();
};
$("restoreJSON").onclick = ()=> $("restoreFile").click();
$("restoreFile").onchange=function(e){
  const f=e.target.files[0];
  if(!f) return;
  const r=new FileReader();
  r.onload=ev=>{
    try{
      const parsed = JSON.parse(ev.target.result);
      if(Array.isArray(parsed)){
        const incoming = parsed.map(normalizeItem);
        assets = incoming;
        save(); render(); renderLeftTotals();
        alert("✅ Data berhasil dipulihkan tanpa dobel.");
      } else alert("File JSON tidak berisi array transaksi.");
    }catch(err){ console.error(err); alert("Gagal membaca JSON. Periksa format file."); }
  };
  r.readAsText(f);
};

/* =========================
   Sortir ≥1 Tahun
========================= */
$("showOld").onclick = ()=>{
  filterMode = "old";
  $("showOld").classList.add("active");
  $("showAll").classList.remove("active");
  render();
};
$("showAll").onclick = ()=>{
  filterMode = "all";
  $("showAll").classList.add("active");
  $("showOld").classList.remove("active");
  render();
};

/* =========================
   Zakat
========================= */
$("zakatBtn").onclick = function() {
  const currentPrice = +$("currentPrice").value || 0;
  if(currentPrice <= 0) return alert("Masukkan harga emas saat ini!");

  // Ambil total gram setahun dari footer tabel
  const totalGramOld = +$("footGram").textContent || 0;
  const totalNow = +$("footNow").textContent.replace(/[^0-9]/g,"") || 0; // angka di footer tanpa Rp/format

  const nishab = 85 * currentPrice;

  if(totalNow < nishab){
    alert(`⏳ Belum wajib zakat\nSaldo gram (≥1 tahun): ${totalGramOld.toFixed(5)} g\nNilai aset sekarang: ${formatRp(totalNow)}\nNishab: ${formatRp(nishab)}`);
  } else {
    const zakat = totalNow * 0.025;
    alert(`🎉 Sudah wajib zakat\nSaldo gram (≥1 tahun): ${totalGramOld.toFixed(5)} g\nNilai aset sekarang: ${formatRp(totalNow)}\nNishab: ${formatRp(nishab)}\nZakat 2,5% = ${formatRp(zakat)}`);
  }
};

/* =========================
   Init
========================= */
render(); renderLeftTotals();
</script>
</body>
</html>
