
<html lang="id">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Tabungan Emas — Light Mode</title>
<style>
:root{
  --blue:#0b61ff;
  --gold:#d4a017;
  --green:#0a8a4b;
  --red:#d63b3b;
  --bg:#fafafa;
  --card:#ffffff;
  --muted:#777;
  --border:#e5e5e5;
  --tableHeader:#f1f1f1;
  --buy:#e9f7ef;
  --sell:#fdecea;
  --admin:#f7f7f7;
}
body{margin:0;padding:0;font-family:Arial, sans-serif;background:var(--bg);color:#222;}
h1,h2,h3{color:#333;}
.container{display:flex;gap:16px;padding:16px;}
.left,.right{background:var(--card);padding:16px;border-radius:12px;border:1px solid var(--border);color:#222;}
.card{background:var(--card);padding:16px;border-radius:12px;border:1px solid var(--border);}
input,select{background:#fff;color:#222;border:1px solid var(--border);padding:6px 8px;border-radius:6px;}
button{padding:8px 14px;border-radius:8px;border:none;cursor:pointer;}
.btn{background:var(--blue);color:#fff;}
.btn.red{background:var(--red);} .btn.green{background:var(--green);} .btn.ghost{background:#fff;color:#333;border:1px solid var(--border);} 
table{width:100%;border-collapse:collapse;background:var(--card);color:#222;}
th{background:var(--tableHeader);border-bottom:2px solid var(--border);padding:10px;}
td{padding:8px;border-bottom:1px solid var(--border);}
.buy-row{background:#e9f7ef!important;} .sell-row{background:#fdecea!important;} .admin-row{background:#f7f7f7!important;} 
.topbar{display:flex;justify-content:space-between;align-items:center;margin-bottom:12px;}
.total-bar{display:flex;align-items:center;gap:8px;}
.muted{color:var(--muted);} 
#loginOverlay,#registerOverlay{position:fixed;top:0;left:0;width:100%;height:100%;display:flex;align-items:center;justify-content:center;background:rgba(255,255,255,0.92);backdrop-filter:blur(5px);z-index:999;} 
.loginBox{background:#fff;padding:24px;border-radius:12px;min-width:260px;border:1px solid var(--border);box-shadow:0 0 12px rgba(0,0,0,0.05);} 
</style>
</head>
<body>
<div id="loginOverlay">
  <div class="loginBox">
    <h2>Login</h2>
    <input id="loginUser" placeholder="Username"><br><br>
    <input id="loginPass" type="password" placeholder="Password"><br><br>
    <button class="btn" id="loginBtn">Login</button>
    <p class="muted">Belum punya akun? <a href="#" id="showRegister">Daftar</a></p>
  </div>
</div>

<div id="registerOverlay" style="display:none;">
  <div class="loginBox">
    <h2>Register</h2>
    <input id="regUser" placeholder="Username"><br><br>
    <input id="regPass" type="password" placeholder="Password"><br><br>
    <button class="btn green" id="registerBtn">Daftar</button>
    <p class="muted">Sudah punya akun? <a href="#" id="showLogin">Login</a></p>
  </div>
</div>

<h1 style="padding-left:16px;">💰 Tabungan Emas — Light Mode</h1>
<div class="container">
<div class="left" style="flex:1;">
  <h3>Input Pembelian</h3>
  <form id="assetForm">
    <input id="grams" type="number" step="0.00001" placeholder="Gram"><br><br>
    <input id="priceBuy" type="number" placeholder="Harga Beli/gr"><br><br>
    <input id="date" type="date"><br><br>
    <button class="btn green" type="submit">Tambah</button>
  </form>
  <hr>
  <h3>Input Penjualan</h3>
  <form id="sellForm">
    <input id="sellGram" type="number" step="0.00001" placeholder="Gram Jual"><br><br>
    <input id="sellPrice" type="number" placeholder="Harga Jual/gr"><br><br>
    <input id="sellDate" type="date"><br><br>
    <button class="btn red" type="submit">Jual</button>
  </form>
  <hr>
  <h3>Potongan Admin (Mengurangi Gram)</h3>
  <form id="adminForm">
    <input id="adminGram" type="number" step="0.00001" placeholder="Gram Dipotong"><br><br>
    <input id="adminDate" type="date"><br><br>
    <button class="btn" type="submit">Input Potongan</button>
  </form>
  <hr>
  <h3>Harga Pasar</h3>
  <input id="currentPrice" type="number" placeholder="Harga Pasar/gr"><br><br>
  <button class="btn" id="zakatBtn">Hitung Zakat</button>
  <br><br>
  <button class="btn ghost" id="backupJSON">Backup JSON</button>
  <button class="btn ghost" id="restoreJSON">Restore JSON</button>
  <input type="file" id="restoreFile" style="display:none;">
</div>

<div class="right" style="flex:2;">
  <div class="topbar">
    <h2>Daftar Aset</h2>
    <div class="total-bar">
      <div id="totalDisplay" class="muted">0 g • Rp0</div>
      <button id="logoutBtn" class="btn ghost">🚪 Logout</button>
    </div>
  </div>
  <div class="card">
    <button id="showAll" class="btn ghost">Tampilkan Semua</button>
    <button id="showOld" class="btn ghost">Tampilkan ≥ 1 Tahun</button>
    <button id="clearAll" class="btn red" style="float:right;">Hapus Semua</button>
    <br><br>
    <table>
      <thead>
        <tr>
          <th>Tipe</th><th>Gram</th><th>Harga</th><th>Tanggal</th><th>Aksi</th>
        </tr>
      </thead>
      <tbody id="assetBody"></tbody>
      <tfoot>
        <tr>
          <td>Total</td>
          <td id="footGram">0</td>
          <td id="footBuy">0</td>
          <td id="footNow">0</td>
          <td id="footProfit">0</td>
        </tr>
      </tfoot>
    </table>
    <br>
    <div id="profitTotal"></div>
    <div id="valueTotal"></div>
  </div>
</div>
</div>

<script>
const USER_KEY="goldUser_v1";
const LS_KEY="goldAssets_v1";
let assets=[];
let filterMode="all";
function $(id){return document.getElementById(id);} 
function hash(s){return btoa(s);} 
function formatRp(n){return "Rp"+Number(n).toLocaleString("id-ID");}

// LOGIN SYSTEM
function checkLogin(){
  const u=sessionStorage.getItem("loggedIn");
  if(!u){$("loginOverlay").style.display="flex";$("registerOverlay").style.display="none";}
  else{$("loginOverlay").style.display="none";$("registerOverlay").style.display="none";}
}
$("showRegister").onclick=()=>{$("loginOverlay").style.display="none";$("registerOverlay").style.display="flex";};
$("showLogin").onclick=()=>{$("registerOverlay").style.display="none";$("loginOverlay").style.display="flex";};

$("registerBtn").onclick=()=>{
  const u=$("regUser").value.trim();
  const p=$("regPass").value.trim();
  if(!u||!p)return alert("Isi semua data");
  localStorage.setItem(USER_KEY,JSON.stringify({user:u,pass:hash(p)}));
  alert("Registrasi berhasil");
  $("registerOverlay").style.display="none";$("loginOverlay").style.display="flex";
};

$("loginBtn").onclick=()=>{
  const u=$("loginUser").value.trim();
  const p=$("loginPass").value.trim();
  const data=JSON.parse(localStorage.getItem(USER_KEY)||"{}");
  if(!data.user)return alert("Belum ada akun. Register dulu.");
  if(u===data.user && hash(p)===data.pass){sessionStorage.setItem("loggedIn",u);checkLogin();}
  else alert("Login salah");
};

$("logoutBtn").onclick=()=>{sessionStorage.removeItem("loggedIn");location.reload();};

// LOAD & SAVE
function save(){localStorage.setItem(LS_KEY,JSON.stringify(assets));}
function load(){assets=JSON.parse(localStorage.getItem(LS_KEY)||"[]");}

load();

function getSaldoEmas(){return assets.reduce((a,b)=>a+b.grams,0);} 

function renderLeftTotals(){
  const totalNow=getSaldoEmas()*Number($("currentPrice").value||0);
  const totalBuy=assets.reduce((a,b)=>a+(b.grams>0?b.grams*b.priceBuy:0),0);
  const profit=totalNow-totalBuy;
  $("profitTotal").textContent="Laba/Rugi: "+formatRp(profit);
  $("valueTotal").textContent="Nilai Total: "+formatRp(totalNow);
}

function render(){
  const tbody=$("assetBody");
  tbody.innerHTML="";
  const now=new Date();
  let list=assets;
  if(filterMode==="old"){
    list=assets.filter(a=>{
      const d=new Date(a.date);
      const diff=(now-d)/1000/60/60/24/365;
      return diff>=1;
    });
  }

  let totalBuy=0;
  let totalNow=0;
  let totalProfit=0;
  const cp=Number($("currentPrice").value||0);

  list.forEach(a=>{
    const tr=document.createElement("tr");
    if(a.type==="buy") tr.className="buy-row";
    if(a.type==="sell") tr.className="sell-row";
    if(a.type==="admin") tr.className="admin-row";

    if(a.grams>0) totalBuy+=a.grams*a.priceBuy;
    totalNow+=a.grams*cp;

    tr.innerHTML=`
      <td>${a.type}</td>
      <td>${a.grams}</td>
      <td>${formatRp(a.priceBuy)}</td>
      <td>${a.date}</td>
      <td>
        <button class="btn ghost editBtn">✏</button>
        <button class="btn red" onclick="delAsset('${a.id}')">🗑</button>
      </td>`;

    tbody.appendChild(tr);

    tr.querySelector('.editBtn').onclick=()=>{
      const g=prompt("Gram:",a.grams);
      const p=prompt("Harga Beli/gr:",a.priceBuy);
      if(g&&p){a.grams=parseFloat(g);a.priceBuy=parseFloat(p);save();render();renderLeftTotals();}
    };
  });

  totalProfit=totalNow-totalBuy;

  $("footGram").textContent=getSaldoEmas().toFixed(5);
  $("footBuy").textContent=formatRp(totalBuy);
  $("footNow").textContent=formatRp(totalNow);
  $("footProfit").textContent=formatRp(totalProfit);

  $("totalDisplay").textContent=`${getSaldoEmas().toFixed(5)} g • ${formatRp(totalNow)}`;
  renderLeftTotals();
}

// FORMS
$("assetForm").onsubmit=e=>{
  e.preventDefault();
  const g=parseFloat($("grams").value);
  const p=parseFloat($("priceBuy").value);
  const d=$("date").value;
  if(isNaN(g)||isNaN(p)||!d)return alert("Lengkapi data");
  assets.push({id:Date.now()+"",grams:g,priceBuy:p,date:d,type:"buy"});
  save();render();e.target.reset();
};

$("sellForm").onsubmit=e=>{
  e.preventDefault();
  const g=parseFloat($("sellGram").value);
  const p=parseFloat($("sellPrice").value);
  const d=$("sellDate").value;
  if(isNaN(g)||isNaN(p)||!d)return alert("Lengkapi data");
  assets.push({id:Date.now()+"",grams:-g,priceBuy:p,date:d,type:"sell"});
  save();render();e.target.reset();
};

$("adminForm").onsubmit=e=>{
  e.preventDefault();
  const g=parseFloat($("adminGram").value);
  const d=$("adminDate").value;
  if(isNaN(g)||!d)return alert("Lengkapi data");
  assets.push({id:Date.now()+"",grams:-g,priceBuy:0,date:d,type:"admin"});
  save();render();e.target.reset();
};

$("clearAll").onclick=()=>{if(confirm("Hapus semua?")){assets=[];save();render();}};
$("showAll").onclick=()=>{filterMode="all";render();};
$("showOld").onclick=()=>{filterMode="old";render();};
$("currentPrice").oninput=render;

// ZAKAT
$("zakatBtn").onclick=()=>{
  const totalGram=getSaldoEmas();
  if(totalGram>=85) alert("Zakat Wajib: "+(totalGram*0.025).toFixed(5)+" g");
  else alert("Belum wajib zakat (nishab 85g)");
};

// BACKUP
$("backupJSON").onclick=()=>{
  const blob=new Blob([JSON.stringify(assets,null,2)],{type:"application/json"});
  const a=document.createElement("a");
  a.href=URL.createObjectURL(blob);
  a.download="backup_gold.json";
  a.click();
};

$("restoreJSON").onclick=()=>{$("restoreFile").click();};
$("restoreFile").onchange=e=>{
  const file=e.target.files[0]; if(!file)return;
  const reader=new FileReader();
  reader.onload=()=>{try{assets=JSON.parse(reader.result);save();render();alert("Restore OK");}catch{alert("File rusak");}}
  reader.readAsText(file);
};

function delAsset(id){
  if(confirm("Hapus aset ini?")){
    assets=assets.filter(a=>a.id!==id);
    save();render();
  }
}

checkLogin();
render();
</script>
</body>

</html>
