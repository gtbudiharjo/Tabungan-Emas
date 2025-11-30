<html lang="id">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Tabungan Emas Pegadaian — Light Mode</title>
<style>
:root{
  --blue:#0b61ff;
  --gold:#e0b300;
  --green:#12a150;
  --red:#d63b3b;
  --bg:#f9f9f9;
  --card:#fff;
  --muted:#555;
  --text:#222;
  --accent:#e0e0e0;
}
body{margin:0;padding:18px;font-family:Inter,ui-sans-serif,system-ui,Segoe UI,Roboto;background:var(--bg);color:var(--text)}
h1{margin:0 0 14px;text-align:center;color:var(--blue);font-size:20px}
.container{display:flex;gap:12px;flex-wrap:wrap}
.left,.right{background:var(--card);border-radius:12px;padding:12px;box-shadow:0 2px 6px rgba(0,0,0,0.1)}
.left{flex:1;min-width:260px;max-width:340px}
.right{flex:2;min-width:320px}
h2{margin:0 0 8px;font-size:15px;color:var(--blue)}
label{display:block;font-size:13px;color:var(--muted);margin-bottom:6px}
input[type="number"], input[type="date"], input[type="text"], input[type="password"]{width:100%;padding:6px 8px;border-radius:6px;border:1px solid #ccc;background:#fff;color:var(--text);font-size:13px}
.row{display:flex;gap:8px;flex-wrap:wrap}
.row>div{flex:1;min-width:100px}
.btn{display:inline-block;padding:6px 10px;border-radius:6px;border:none;cursor:pointer;font-size:13px}
.btn.primary{background:var(--blue);color:#fff}
.btn.gold{background:var(--gold);color:#000;font-weight:600}
.btn.ghost{background:transparent;border:1px solid var(--blue);color:var(--blue)}
.btn.small{padding:4px 8px;font-size:12px;border-radius:6px}
.total{font-size:13px;color:var(--text);margin-top:8px}
.muted{color:var(--muted);font-size:12px}
table{width:100%;border-collapse:collapse;margin-top:10px;font-size:13px;table-layout:fixed;word-wrap:break-word;}
th,td{padding:6px 4px;border:1px solid var(--accent);text-align:center;overflow-wrap:break-word;}
th{background:var(--blue);color:#fff;font-weight:600;font-size:13px}
.profit-pos{color:var(--green);font-weight:700}
.profit-neg{color:var(--red);font-weight:700}
.inline-edit{padding:6px;border-radius:6px;background:#f0f0f0;border:1px solid #ccc;color:var(--text)}
.controls{display:flex;gap:8px;flex-wrap:wrap}
.topbar{display:flex;justify-content:space-between;align-items:center;margin-bottom:6px}
.total-bar{display:flex;gap:12px;align-items:center}
#totalDisplay{color:var(--muted)}
.auth-box{padding:20px;background:#fff;border:1px solid #ccc;max-width:300px;margin:20px auto;border-radius:8px;text-align:center}
.auth-box input{margin-bottom:6px;padding:6px;font-size:14px}
.auth-box button{width:100%;padding:6px;font-size:14px;margin-bottom:6px}
.auth-box small{display:block;margin-top:6px;color:var(--blue);cursor:pointer;text-decoration:underline}
@media(max-width:860px){.container{flex-direction:column}}
</style>
</head>
<body>

<h1>💰 Tabungan Emas Pegadaian — Light Mode</h1>

<!-- LOGIN / REGISTER -->
<div id="authContainer" class="auth-box">
  <h2>Login</h2>
  <form id="loginForm">
    <input type="text" id="loginUser" placeholder="Username" required />
    <input type="password" id="loginPass" placeholder="Password" required />
    <button type="submit" class="btn primary">Login</button>
  </form>
  <small id="showRegister">Belum punya akun? Register</small>

  <div id="registerFormContainer" style="display:none;margin-top:10px">
    <form id="registerForm">
      <input type="text" id="regUser" placeholder="Username" required />
      <input type="password" id="regPass" placeholder="Password" required />
      <button type="submit" class="btn gold">Register</button>
    </form>
  </div>
</div>

<!-- ASET EMAS -->
<div class="container" id="mainApp" style="display:none;">

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
          <button id="logoutBtn" class="btn ghost small">🚪 Logout</button>
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

<script>
// ================= LOGIN / REGISTER =================
const LS_USERS = "goldUsers_final";
let currentUser = null;
function saveUsers(users){ localStorage.setItem(LS_USERS, JSON.stringify(users)); }
function getUsers(){ return JSON.parse(localStorage.getItem(LS_USERS)||"[]"); }

const showRegisterLink = document.getElementById("showRegister");
const registerContainer = document.getElementById("registerFormContainer");
showRegisterLink.onclick = ()=>{ registerContainer.style.display = registerContainer.style.display==="none"?"block":"none"; };

// Register
document.getElementById("registerForm").onsubmit = function(e){
  e.preventDefault();
  const user = document.getElementById("regUser").value.trim();
  const pass = document.getElementById("regPass").value.trim();
  if(!user||!pass) return alert("Isi username & password!");
  const users = getUsers();
  if(users.find(u=>u.user===user)) return alert("Username sudah ada!");
  users.push({user, pass});
  saveUsers(users);
  alert("✅ Registrasi berhasil! Silakan login.");
  this.reset(); registerContainer.style.display="none";
};

// Login
document.getElementById("loginForm").onsubmit = function(e){
  e.preventDefault();
  const user = document.getElementById("loginUser").value.trim();
  const pass = document.getElementById("loginPass").value.trim();
  const users = getUsers();
  const match = users.find(u=>u.user===user && u.pass===pass);
  if(match){
    currentUser = user;
    document.getElementById("authContainer").style.display="none";
    document.getElementById("mainApp").style.display="flex";
    render(); renderLeftTotals();
  } else alert("❌ Username / password salah!");
};

document.getElementById("logoutBtn").onclick = ()=>{
  currentUser = null;
  document.getElementById("authContainer").style.display="block";
  document.getElementById("mainApp").style.display="none";
};

// ================= STORAGE & HELPERS EMAS =================
const LS_KEY="goldAssets_final";
let assets = JSON.parse(localStorage.getItem(LS_KEY)||"[]");
let filterMode="all";
const $=id=>document.getElementById(id);
const tbody = document.querySelector("#table tbody");
function save(){ localStorage.setItem(LS_KEY,JSON.stringify(assets)); }
function formatRp(n){ return "Rp"+Math.round(n||0).toString().replace(/\B(?=(\d{3})+(?!\d))/g,"."); }
function isOld(d){ const dt=new Date(d); return !isNaN(dt) && (new Date()-dt)>=365*24*60*60*1000; }
function normalizeItem(item){
  return { id: item.id || Date.now().toString(36)+Math.random().toString(36).slice(2,6),
           grams: Number(item.grams)||0,
           priceBuy: Number(item.priceBuy)||0,
           date: item.date||new Date().toISOString().slice(0,10),
           type: item.type ? String(item.type) : (Number(item.grams)<0?"admin":"buy"),
           parentId: item.parentId||null };
}

// ================= RENDER TOTALS =================
function renderLeftTotals(){
  const currentPrice = +$("currentPrice").value || 0;
  let totalGram = 0, totalBuy = 0, totalNow = 0, totalProfit = 0;
  assets.forEach(a => {
    if(a.type==="buy"){
      const soldGram = assets.filter(x=>x.type==="sell" && x.parentId===a.id).reduce((s,x)=>s+x.grams,0);
      const rem = a.grams - soldGram;
      const nowVal = rem*currentPrice;
      const profit = nowVal - rem*a.priceBuy;
      totalGram += rem; totalBuy += rem*a.priceBuy; totalNow += nowVal; totalProfit += profit;
    } else if(a.type==="admin"){
      totalGram += a.grams; totalNow += a.grams*currentPrice; totalBuy += 30000; totalProfit += a.grams*currentPrice-30000;
    }
  });
  $("leftGramTotal").textContent=`Saldo Emas: ${totalGram.toFixed(5)} g`;
  $("leftProfitTotal").innerHTML = totalProfit>=0? `<span class="profit-pos">Laba: ${formatRp(totalProfit)}</span>`:`<span class="profit-neg">Rugi: ${formatRp(totalProfit)}</span>`;
  $("leftTotalAsset").textContent=`💰 Total Aset (harga sekarang): ${formatRp(totalNow)}`;
}

// ================= RENDER TABLE =================
function render(){
  tbody.innerHTML=""; const currentPrice = +$("currentPrice").value || 0;
  let totalGram=0,totalBuy=0,totalNow=0,totalProfit=0;
  let filtered = assets.filter(a=>a.type==="buy"||a.type==="admin");
  if(filterMode==="old"){ filtered = filtered.filter(a=>{ const soldGram = assets.filter(x=>x.type==="sell" && x.parentId===a.id).reduce((s,x)=>s+x.grams,0); const rem = a.grams - soldGram; return a.type==="admin" || (isOld(a.date) && rem>0); }); }
  filtered.sort((a,b)=> new Date(a.date)-new Date(b.date));
  filtered.forEach((a,i)=>{
    if(a.type==="admin"){ const trAdmin=document.createElement("tr"); trAdmin.dataset.id=a.id; trAdmin.style.color="var(--red)";
      trAdmin.innerHTML=`<td>-</td><td>${a.grams.toFixed(5)}</td><td>${a.date}</td><td>${formatRp(30000)}</td><td>${formatRp(30000)}</td><td>${formatRp(30000)}</td><td>${formatRp(-30000)}</td><td>Admin Fee <button class="btn small ghost editAdmin">✏️</button> <button class="btn small ghost delAdmin">❌</button></td>`;
      tbody.appendChild(trAdmin); totalGram+=a.grams; totalBuy+=30000; totalProfit-=30000; return; }
    const sold = assets.filter(x=>x.type==="sell" && x.parentId===a.id);
    const soldGram = sold.reduce((s,x)=>s+x.grams,0); const rem = a.grams - soldGram;
    const nowVal = rem*(currentPrice||a.priceBuy); const profit = nowVal - rem*a.priceBuy;
    const tr=document.createElement("tr"); tr.dataset.id=a.id; tr.style.color=(soldGram>0?"var(--red)":profit>0?"var(--green)":profit<0?"var(--red)":"var(--text)");
    if(soldGram>0) tr.style.textDecoration="line-through";
    tr.innerHTML=`<td>${i+1}</td><td>${a.grams.toFixed(5)}</td><td>${a.date}</td><td>${formatRp(a.priceBuy)}</td><td>${formatRp(a.grams*a.priceBuy)}</td><td>${formatRp(nowVal)}</td><td>${formatRp(profit)}</td><td><button class="btn small ghost editBtn">✏️ Edit</button><button class="btn small ghost sellBtn">💰 Jual</button><button class="btn small ghost" onclick="delAsset('${a.id}')">❌</button></td>`;
    tbody.appendChild(tr);
    sold.forEach(s=>{ const trSell=document.createElement("tr"); trSell.dataset.id=s.id; trSell.style.color="var(--blue)"; trSell.innerHTML=`<td>${i+1}</td><td>${s.grams.toFixed(5)}</td><td>${s.date}</td><td>${formatRp(s.priceBuy)}</td><td>${formatRp(s.grams*s.priceBuy)}</td><td>-</td><td>-</td><td>Historical Sell</td>`; tbody.appendChild(trSell); });
    if(rem>0 && soldGram>0){ const trRemain=document.createElement("tr"); trRemain.dataset.id=a.id+"_remain"; trRemain.style.color=(profit>=0?"var(--green)":"var(--red)"); trRemain.innerHTML=`<td>${i+1}</td><td>${rem.toFixed(5)}</td><td>${a.date}</td><td>${formatRp(a.priceBuy)}</td><td>${formatRp(rem*a.priceBuy)}</td><td>${formatRp(nowVal)}</td><td>${formatRp(profit)}</td><td><button class="btn small ghost editBtn">✏️ Edit</button><button class="btn small ghost sellBtn">💰 Jual</button></td>`; tbody.appendChild(trRemain);}
    totalGram+=rem; totalBuy+=rem*a.priceBuy; totalNow+=nowVal; totalProfit+=profit;
  });
  const totalNowAccurate = totalGram*(currentPrice||0); const totalProfitAccurate=totalNowAccurate-totalBuy;
  $("footGram").textContent = totalGram.toFixed(5); $("footBuy").textContent=formatRp(totalBuy);
  $("footNow").textContent=formatRp(totalNowAccurate); $("footProfit").textContent=formatRp(totalProfitAccurate);
  $("totalDisplay").textContent=`${totalGram.toFixed(5)} g • ${formatRp(totalNowAccurate)}`;
  $("profitTotal").textContent=`💹 Keuntungan: ${formatRp(totalProfitAccurate)}`;
  $("valueTotal").textContent=`💰 Beli: ${formatRp(totalBuy)} | Nilai Sekarang: ${formatRp(totalNowAccurate)}`;
}

// ================= CRUD & EVENTS EMAS =================
function delAsset(id){ if(confirm("Hapus transaksi?")){ assets=assets.filter(a=>a.id!==id && a.parentId!==id); save(); render(); renderLeftTotals(); } }

tbody.addEventListener("click", e=>{
  const tr=e.target.closest("tr"); if(!tr) return; let id=tr.dataset.id; let a=assets.find(x=>x.id===id);
  if(!a && id.endsWith("_remain")){ const parentId=id.replace("_remain",""); a=assets.find(x=>x.id===parentId); id=parentId; } if(!a) return;
  if(e.target.classList.contains("editBtn") && a.type==="buy"){ const newGram=+prompt("Ubah gram:",a.grams); const newPrice=+prompt("Ubah harga beli/gr:",a.priceBuy); const newDate=prompt("Ubah tanggal (YYYY-MM-DD):",a.date);
    if(!newGram||!newPrice||!newDate) return; const soldGram=assets.filter(x=>x.type==="sell"&&x.parentId===a.id).reduce((s,x)=>s+x.grams,0); if(newGram<soldGram){ alert("❌ Gram baru tidak boleh lebih kecil dari total gram yang sudah dijual!"); return; }
    a.grams=newGram; a.priceBuy=newPrice; a.date=newDate; save(); render(); renderLeftTotals(); }
  if(e.target.classList.contains("editAdmin") && a.type==="admin"){ const g=+prompt("Ubah gram admin (angka minus):",a.grams); const d=prompt("Tanggal admin (YYYY-MM-DD):",a.date); if(!g||!d) return; a.grams=g>0?-g:g; a.date=d; save(); render(); renderLeftTotals(); }
  if(e.target.classList.contains("sellBtn")){ const rem=a.grams-assets.filter(x=>x.type==="sell"&&x.parentId===a.id).reduce((s,x)=>s+x.grams,0); const grams=+prompt("Gram dijual (tersisa "+rem.toFixed(5)+" g):",rem.toFixed(5)); const price=+prompt("Harga jual/gr:",a.priceBuy); const date=prompt("Tanggal jual (YYYY-MM-DD):",new Date().toISOString().slice(0,10));
    if(grams>0 && grams<=rem && price && date){ assets.push({id:Date.now().toString(36), type:"sell", grams, priceBuy:price, date, parentId:a.id}); save(); render(); renderLeftTotals(); } }
});

$("assetForm").onsubmit=function(e){ e.preventDefault(); const grams=+$("grams").value; const price=+$("priceBuy").value; const date=$("date").value; assets.push({id:Date.now().toString(36), grams, priceBuy:price, date, type:"buy"}); save(); render(); renderLeftTotals(); this.reset(); };
$("adminForm").onsubmit=function(e){ e.preventDefault(); const grams=-Math.abs(+$("adminGram").value); const date=$("adminDate").value; assets.push({id:Date.now().toString(36), grams, priceBuy:30000, date, type:"admin"}); save(); render(); renderLeftTotals(); this.reset(); };
$("clearAll").onclick=()=>{ if(confirm("Hapus semua data?")){ assets=[]; save(); render(); renderLeftTotals(); } };
$("currentPrice").oninput=()=>{ render(); renderLeftTotals(); };

// ================= BACKUP & RESTORE =================
$("backupJSON").onclick=()=>{ const blob=new Blob([JSON.stringify(assets)],{type:"application/json"}); const a=document.createElement("a"); a.href=URL.createObjectURL(blob); a.download="Pegadaian.json"; a.click(); };
$("restoreJSON").onclick=()=>$("restoreFile").click();
$("restoreFile").onchange=function(e){ const f=e.target.files[0]; if(!f) return; const r=new FileReader(); r.onload=ev=>{ try{ const parsed=JSON.parse(ev.target.result); if(Array.isArray(parsed)){ assets=parsed.map(normalizeItem); save(); render(); renderLeftTotals(); alert("✅ Data berhasil dipulihkan."); }else alert("File JSON tidak berisi array."); }catch(err){ console.error(err); alert("Gagal membaca JSON."); } }; r.readAsText(f); };

// ================= SORTIR ≥1 TAHUN =================
$("showOld").onclick=()=>{ filterMode="old"; $("showOld").classList.add("active"); $("showAll").classList.remove("active"); render(); };
$("showAll").onclick=()=>{ filterMode="all"; $("showAll").classList.add("active"); $("showOld").classList.remove("active"); render(); };

// ================= ZAKAT =================
$("zakatBtn").onclick=function(){
  const currentPrice=+$("currentPrice").value||0; if(currentPrice<=0) return alert("Masukkan harga emas saat ini!");
  const totalGramOld=+$("footGram").textContent||0;
  const totalNow=+$("footNow").textContent.replace(/[^0-9]/g,"")||0;
  const nishab=85*currentPrice;
  if(totalNow<nishab){ alert(`⏳ Belum wajib zakat\nSaldo gram (≥1 tahun): ${totalGramOld.toFixed(5)} g\nNilai aset sekarang: ${formatRp(totalNow)}\nNishab: ${formatRp(nishab)}`); }
  else{ const zakat=totalNow*0.025; alert(`🎉 Sudah wajib zakat\nSaldo gram (≥1 tahun): ${totalGramOld.toFixed(5)} g\nNilai aset sekarang: ${formatRp(totalNow)}\nNishab: ${formatRp(nishab)}\nZakat 2,5% = ${formatRp(zakat)}`); }
};

// ================= INIT =================
render(); renderLeftTotals();
</script>
</body>
</html>
