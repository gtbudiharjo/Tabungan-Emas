<html lang="id">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Aset Emas — Light Mode</title>

<style>
:root{
  --blue:#0b61ff;
  --gold:#e0b300;
  --green:#12a150;
  --red:#d63b3b;
  --bg:#ffffff;
  --card:#f4f6f9;
  --muted:#6f7480;
  --text:#111;
  --accent:#cbd5e1;
}

*{box-sizing:border-box}

body{
  margin:0;
  padding:18px;
  font-family:Inter,ui-sans-serif,system-ui,Segoe UI,Roboto;
  background:#ffffff;
  color:var(--text);
}

h1{
  margin:0 0 14px;
  text-align:center;
  color:var(--blue);
  font-size:20px;
}

.container{
  display:flex;
  gap:12px;
  flex-wrap:wrap;
}

.left,.right{
  background:var(--card);
  border-radius:12px;
  padding:12px;
  box-shadow:0 2px 8px rgba(0,0,0,0.1);
}

.left{flex:1;min-width:260px;max-width:340px}
.right{flex:2;min-width:320px;overflow-x:auto}

h2{
  margin:0 0 8px;
  font-size:15px;
  color:var(--blue);
}

label{
  display:block;
  font-size:13px;
  color:var(--muted);
  margin-bottom:6px;
}

input[type="number"],
input[type="date"],
input[type="text"],
input[type="password"]{
  width:100%;
  padding:7px 9px;
  border-radius:6px;
  border:1px solid #d0d5dd;
  background:#fff;
  color:var(--text);
  font-size:13px;
}

.row{display:flex;gap:8px;flex-wrap:wrap}
.row>div{flex:1;min-width:100px}

.btn{
  display:inline-block;
  padding:6px 9px;
  border-radius:8px;
  border:none;
  cursor:pointer;
  font-size:12px;
}

.btn.primary{background:var(--blue);color:#fff}
.btn.gold{background:var(--gold);color:#000;font-weight:600}
.btn.ghost{background:transparent;border:1px solid var(--blue);color:var(--blue)}

.total{font-size:13px;color:var(--text);margin-top:8px}
.muted{color:var(--muted);font-size:12px}

/* Tabel anti-scroll */
table{
  width:100%;
  max-width:100%;
  border-collapse:collapse;
  margin-top:10px;
  font-size:12px;
  table-layout:fixed;
}

th,td{
  padding:8px 6px;
  border-bottom:1px solid #e5e7eb;
  text-align:left;

  /* FIX anti scroll */
  white-space:normal;
  word-break:break-word;
}

th{
  background:#e2e8f0;
  color:#111;
  font-weight:600;
}

tr:hover td{background:#f1f5f9}

tfoot td{
  font-weight:700;
  background:#f0f0f0;
  color:var(--blue);
}

.profit-pos{color:var(--green);font-weight:700}
.profit-neg{color:var(--red);font-weight:700}

.controls{display:flex;gap:8px;flex-wrap:wrap}
.topbar{display:flex;justify-content:space-between;align-items:center;margin-bottom:6px}
.total-bar{display:flex;gap:12px;align-items:center}

@media(max-width:860px){
  .container{flex-direction:column}
}

/* Login/Register Overlay */
#loginOverlay,#registerOverlay{
  position:fixed;
  top:0;left:0;
  width:100%;height:100%;
  background:rgba(240,240,240,0.95);
  display:flex;
  justify-content:center;
  align-items:center;
  z-index:9999;
}

/* Form card */
.overlay-box{
  background:white;
  padding:18px;
  border-radius:12px;
  width:260px;
  box-shadow:0 4px 12px rgba(0,0,0,0.15);
  text-align:center;
}
</style>
</head>

<body>

<h1>Manajemen Aset Emas — Light Mode</h1>

<div class="container">

  <!-- Left Panel -->
  <div class="left">
    <h2>Input Pembelian</h2>

    <label>Tanggal</label>
    <input type="date">

    <label>Harga/Gram</label>
    <input type="number">

    <label>Jumlah Gram</label>
    <input type="number">

    <button class="btn primary" style="margin-top:10px;width:100%">Tambah</button>

    <p class="total">Total aset: <b>0 gram</b></p>
  </div>

  <!-- Right Panel -->
  <div class="right">
    <h2>Riwayat Transaksi</h2>

    <table>
      <thead>
        <tr>
          <th>Tanggal</th>
          <th>Gram</th>
          <th>Harga Beli</th>
          <th>Total</th>
          <th>Aksi</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>2025-01-12</td>
          <td>1.20</td>
          <td>1.200.000</td>
          <td>1.440.000</td>
          <td><button class="btn ghost small">Hapus</button></td>
        </tr>
      </tbody>
    </table>
  </div>

</div>

</body>
</html>
<!doctype html>
<html lang="id">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Aset Emas — Light Mode (Fix 100%)</title>

<style>
:root{
  --blue:#0b61ff;
  --gold:#e0b300;
  --green:#12a150;
  --red:#d63b3b;
  --bg:#ffffff;
  --card:#f4f6f9;
  --muted:#6f7480;
  --text:#111;
  --accent:#cbd5e1;
}

*{box-sizing:border-box}

body{
  margin:0;
  padding:18px;
  font-family:Inter,ui-sans-serif,system-ui,Segoe UI,Roboto;
  background:#ffffff;
  color:var(--text);
}

h1{
  margin:0 0 14px;
  text-align:center;
  color:var(--blue);
  font-size:20px;
}

.container{
  display:flex;
  gap:12px;
  flex-wrap:wrap;
}

.left,
.right{
  background:var(--card);
  border-radius:12px;
  padding:12px;
  box-shadow:0 2px 8px rgba(0,0,0,0.1);
}

/* FIX: hanya satu deklarasi .right */
.left{
  flex:1;
  min-width:260px;
  max-width:340px;
}

.right{
  flex:3;
  min-width:320px;
  overflow-x:hidden; /* FIX anti horizontal scroll */
}

h2{
  margin:0 0 8px;
  font-size:15px;
  color:var(--blue);
}

label{
  display:block;
  font-size:13px;
  color:var(--muted);
  margin-bottom:6px;
}

input[type="number"],
input[type="date"],
input[type="text"],
input[type="password"]{
  width:100%;
  padding:7px 9px;
  border-radius:6px;
  border:1px solid #d0d5dd;
  background:#fff;
  color:var(--text);
  font-size:13px;
}

.row{display:flex;gap:8px;flex-wrap:wrap}
.row>div{flex:1;min-width:100px}

.btn{
  display:inline-block;
  padding:6px 9px;
  border-radius:8px;
  border:none;
  cursor:pointer;
  font-size:12px;
}

.btn.primary{background:var(--blue);color:#fff}
.btn.gold{background:var(--gold);color:#000;font-weight:600}
.btn.ghost{background:transparent;border:1px solid var(--blue);color:var(--blue)}

.total{font-size:13px;color:var(--text);margin-top:8px}
.muted{color:var(--muted);font-size:12px}

/* tabel anti-scroll */
table{
  width:100%;
  border-collapse:collapse;
  margin-top:10px;
  font-size:12px;
  table-layout:fixed; /* FIX */
}

th,td{
  padding:8px 6px;
  border-bottom:1px solid #e5e7eb;
  text-align:left;

  /* FIX paling penting */
  white-space:normal;
  word-break:break-word;
}

th{
  background:#e2e8f0;
  color:#111;
  font-weight:600;
}

tr:hover td{background:#f1f5f9}

tfoot td{
  font-weight:700;
  background:#f0f0f0;
  color:var(--blue);
}

.profit-pos{color:var(--green);font-weight:700}
.profit-neg{color:var(--red);font-weight:700}

.controls{display:flex;gap:8px;flex-wrap:wrap}
.topbar{display:flex;justify-content:space-between;align-items:center;margin-bottom:6px}
.total-bar{display:flex;gap:12px;align-items:center}

@media(max-width:860px){
  .container{flex-direction:column}
}

/* overlay login */
#loginOverlay,#registerOverlay{
  position:fixed;
  top:0;left:0;
  width:100%;height:100%;
  background:rgba(240,240,240,0.95);
  display:flex;
  justify-content:center;
  align-items:center;
  z-index:9999;
}

.overlay-box{
  background:white;
  padding:18px;
  border-radius:12px;
  width:260px;
  box-shadow:0 4px 12px rgba(0,0,0,0.15);
  text-align:center;
}
</style>
</head>

<body>

<h1>Manajemen Aset Emas — Light Mode</h1>

<div class="container">

  <!-- Left Panel -->
  <div class="left">
    <h2>Input Pembelian</h2>

    <label>Tanggal</label>
    <input type="date">

    <label>Harga/Gram</label>
    <input type="number">

    <label>Jumlah Gram</label>
    <input type="number">

    <button class="btn primary" style="margin-top:10px;width:100%">Tambah</button>

    <p class="total">Total aset: <b>0 gram</b></p>
  </div>

  <!-- Right Panel -->
  <div class="right">
    <h2>Riwayat Transaksi</h2>

    <table>
      <thead>
        <tr>
          <th>Tanggal</th>
          <th>Gram</th>
          <th>Harga Beli</th>
          <th>Total</th>
          <th>Aksi</th>
        </tr>
      </thead>

      <tbody>
        <tr>
          <td>2025-01-12</td>
          <td>1.20</td>
          <td>1.200.000</td>
          <td>1.440.000</td>
          <td><button class="btn ghost small">Hapus</button></td>
        </tr>

        <tr>
          <td>2025-01-15</td>
          <td>0.75</td>
          <td>1.210.000</td>
          <td>907.500</td>
          <td><button class="btn ghost small">Hapus</button></td>
        </tr>
      </tbody>
    </table>

  </div>

</div>

</body>
</html>
