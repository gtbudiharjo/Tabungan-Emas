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
  --muted:#6b7280;
  --text:#111;
  --accent:#e5e7eb;
}
*{box-sizing:border-box}
body{margin:0;padding:18px;font-family:Inter,ui-sans-serif,system-ui,Segoe UI,Roboto;background:var(--bg);color:var(--text)}
h1{margin:0 0 14px;text-align:center;color:var(--blue);font-size:20px}
.container{display:flex;gap:12px;flex-wrap:wrap}
.left,.right{background:var(--card);border-radius:12px;padding:12px;box-shadow:0 6px 18px rgba(0,0,0,0.1)}
.left{flex:1;min-width:260px;max-width:340px}
.right{flex:2;min-width:320px}
h2{margin:0 0 8px;font-size:15px;color:var(--blue)}
label{display:block;font-size:13px;color:var(--muted);margin-bottom:6px}
input[type="number"], input[type="date"], input[type="text"], input[type="password"]{width:100%;padding:8px 10px;border-radius:8px;border:1px solid var(--accent);background:#f3f4f6;color:var(--text);font-size:13px}
.row{display:flex;gap:8px;flex-wrap:wrap}
.row>div{flex:1;min-width:100px}
.btn{display:inline-block;padding:7px 10px;border-radius:8px;border:none;cursor:pointer;font-size:13px}
.btn.primary{background:var(--blue);color:#fff}
.btn.gold{background:var(--gold);color:#000;font-weight:600}
.btn.ghost{background:transparent;border:1px solid var(--blue);color:var(--blue)}
.btn.small{padding:5px 8px;font-size:12px;border-radius:7px}
.total{font-size:13px;color:var(--text);margin-top:8px}
.muted{color:var(--muted);font-size:12px}
table{width:100%;border-collapse:collapse;margin-top:10px;font-size:13px,table-layout:auto}
th,td{padding:8px 6px;border-bottom:1px solid var(--accent);text-align:left;word-break:break-word}
th{background:var(--accent);color:#111;font-weight:600;font-size:13px}
.profit-pos{color:var(--green);font-weight:700}
.profit-neg{color:var(--red);font-weight:700}
.inline-edit{padding:6px;border-radius:6px;background:#f3f4f6;border:1px solid #d1d5db;color:var(--text)}
.controls{display:flex;gap:8px;flex-wrap:wrap}
.topbar{display:flex;justify-content:space-between;align-items:center;margin-bottom:6px}
.total-bar{display:flex;gap:12px;align-items:center}
#totalDisplay{color:var(--muted)}
#showOld.active { background:var(--blue);color:#fff;border-color:transparent }
#showAll.active { background:transparent;color:var(--blue);border-color:var(--blue) }
#modal{display:none;position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.6);justify-content:center;align-items:center;z-index:999}
#modalContent{background:var(--card);padding:20px;border-radius:12px;width:300px;max-width:90%}
#modalContent h3{margin-top:0;color:var(--blue)}
#modalContent input{margin-bottom:10px}
.modal-buttons{display:flex;justify-content:flex-end;gap:8px}
.login-box{max-width:350px;margin:50px auto;padding:20px;background:var(--card);border-radius:12px;box-shadow:0 4px 12px rgba(0,0,0,0.1)}
.login-box h2{text-align:center;margin-bottom:12px;color:var(--blue)}
.switch-login{font-size:12px;text-align:center;margin-top:8px;color:var(--muted);cursor:pointer}
</style>
</head>
<body>
<div id="loginContainer" class="login-box">
  <h2>💰 Tabungan Emas</h2>
  <input id="username" type="text" placeholder="Username" />
  <input id="password" type="password" placeholder="Password" />
  <button id="loginBtn" class="btn primary" style="width:100%;margin-top:10px">Login</button>
  <div class="switch-login" id="showRegister">Belum punya akun? Register</div>
</div>

<div id="mainApp" style="display:none">
<h1>💰 Tabungan Emas — Light Mode</h1>
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
<div style="text-align:center;margin-top:12px;color:var(--muted)">Data tersimpan otomatis di browser (<b>goldAssets_final</b>)</div>
</div>

<script>
// =================== LOGIN REGISTER ===================
let currentUser=null;
const users=JSON.parse(localStorage.getItem('users')||'[]');
const loginContainer=$('loginContainer');
const mainApp=$('mainApp');
function $(id){return document.getElementById(id)}

$('loginBtn').onclick=()=>{
  const u=$('username').value.trim();
  const p=$('password').value.trim();
  const user=users.find(x=>x.username===u && x.password===p);
  if(user){currentUser=u;loginContainer.style.display='none';mainApp.style.display='block'; render(); renderLeftTotals();}
  else alert('Login gagal');
};
$('showRegister').onclick=()=>{
  const u=prompt('Username baru:');
  if(!u) return;
  if(users.find(x=>x.username===u)){alert('Username sudah ada'); return;}
  const p=prompt('Password baru:');
  if(!p) return;
  users.push({username:u,password:p});
  localStorage.setItem('users',JSON.stringify(users));
  alert('Akun berhasil dibuat');
};

// =================== TABUNGAN EMAS ===================
const LS_KEY='goldAssets_final';
let assets=JSON.parse(localStorage.getItem(LS_KEY)||'[]');
let filterMode='all';

function save(){ localStorage.setItem(LS_KEY,JSON.stringify(assets)); }
function formatRp(n){ return 'Rp'+Math.round(n||0).toString().replace(/\B(?=(\d{3})+(?!\d))/g,'.'); }
function isOld(d){ const dt=new Date(d); return !isNaN(dt) && (new Date()-dt)>=365*24*60*60*1000; }

function renderLeftTotals(){
  const currentPrice=+$('currentPrice').value||0;
  let totalGram=0,totalBuy=0,totalNow=0,totalProfit=0;
  assets.forEach(a=>{
    const soldGram=assets.filter(x=>x.type==='sell' && x.parentId===a.id).reduce((s,x)=>s+x.grams,0);
    const rem=(a.grams||0)-soldGram;
    const nowVal=rem*currentPrice;
    const profit=nowVal-rem*(a.priceBuy||0);
    totalGram+=rem;
    totalBuy+=rem*(a.priceBuy||0);
    totalNow+=nowVal;
    totalProfit+=profit;
  });
  $('leftGramTotal').textContent=`Saldo Emas: ${totalGram.toFixed(5)} g`;
  $('leftProfitTotal').innerHTML=totalProfit>=0?`<span class='profit-pos'>Laba: ${formatRp(totalProfit)}</span>`:`<span class='profit-neg'>Rugi: ${formatRp(totalProfit)}</span>`;
  $('leftTotalAsset').textContent=`💰 Total Aset (harga sekarang): ${formatRp(totalNow)}`;
  $('totalDisplay').textContent=`${totalGram.toFixed(5)} g • ${formatRp(totalNow)}`;
  $('profitTotal').textContent=`💹 Keuntungan: ${formatRp(totalProfit)}`;
  $('valueTotal').textContent=`💰 Beli: ${formatRp(totalBuy)} | Nilai Sekarang: ${formatRp(totalNow)}`;
}

function render(){
  const tbody=document.querySelector('#table tbody'); tbody.innerHTML='';
  const currentPrice=+$('currentPrice').value||0;
  assets.filter(a=>a.type==='buy').forEach((a,i)=>{
    const soldGram=assets.filter(x=>x.type==='sell' && x.parentId===a.id).reduce((s,x)=>s+x.grams,0);
    const rem=a.grams-soldGram;
    const nowVal=rem*currentPrice;
    const profit=nowVal-rem*a.priceBuy;
    const tr=document.createElement('tr');
    tr.innerHTML=`<td>${i+1}</td><td>${rem.toFixed(5)}</td><td>${a.date}</td><td>${formatRp(a.priceBuy)}</td><td>${formatRp(rem*a.priceBuy)}</td><td>${formatRp(nowVal)}</td><td>${formatRp(profit)}</td><td><button class='btn small ghost editBtn'>✏️</button><button class='btn small ghost sellBtn'>💰 Jual</button><button class='btn small ghost' onclick="delAsset('${a.id}')">❌</button></td>`;
    tbody.appendChild(tr);
  });
  renderLeftTotals();
}

function delAsset(id){if(confirm('Hapus transaksi?')){assets=assets.filter(a=>a.id!==id && a.parentId!==id);save();render();}}
$('assetForm').onsubmit=function(e){e.preventDefault();assets.push({id:Date.now().toString(36),grams:+$('grams').value,priceBuy:+$('priceBuy').value,date:$('date').value,type:'buy'});save();render();this.reset();};
$('currentPrice').oninput=()=>render();
$('clearAll').onclick=()=>{if(confirm('Hapus semua data?')){assets=[];save();render();}};
</script>
</body>
</html>
