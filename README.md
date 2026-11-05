<!doctype html>
<html lang="id">
<head>
<meta charset="utf-8"><meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1">
<meta name="theme-color" content="#123b35">
<link rel="manifest" href="manifest.webmanifest"><title>Stok & Gaji Toko</title>
<style>
:root{--green:#123b35;--mint:#e8f3ef;--bg:#f4f7f6;--ink:#172522;--muted:#66736f;--red:#b42318}
*{box-sizing:border-box}body{margin:0;background:var(--bg);color:var(--ink);font:15px system-ui,-apple-system,Segoe UI,sans-serif}
header{background:var(--green);color:white;padding:20px 16px 18px;border-radius:0 0 22px 22px}
header h1{font-size:21px;margin:0 0 4px}header p{margin:0;color:#d3e5df;font-size:13px}
main{padding:14px 14px 92px;max-width:720px;margin:auto}
.nav{display:grid;grid-template-columns:repeat(3,1fr);gap:7px;margin:14px 0}
.nav button{border:0;border-radius:12px;padding:11px 4px;background:white;color:var(--green);font-weight:650}
.nav button.active{background:var(--green);color:white}
.panel{display:none}.panel.active{display:block}
.cards{display:grid;grid-template-columns:repeat(2,1fr);gap:10px}
.card,.box{background:white;border-radius:15px;padding:14px;box-shadow:0 2px 10px #163c3210}
.card small{color:var(--muted)}.card strong{display:block;font-size:21px;margin-top:6px}
h2{font-size:18px;margin:16px 0 10px}.box{margin:10px 0}
label{display:block;font-size:13px;color:var(--muted);margin:10px 0 5px}
input,select{width:100%;padding:12px;border:1px solid #d9e2de;border-radius:10px;background:white;font:inherit;color:var(--ink)}
button.primary{width:100%;border:0;background:var(--green);color:white;padding:13px;border-radius:11px;font-weight:700;margin-top:12px}
button.secondary{border:1px solid #cbd8d3;background:white;color:var(--green);padding:9px 11px;border-radius:9px;font-weight:600}
button.danger{border:0;background:#fff0ef;color:var(--red);padding:8px 10px;border-radius:8px}
.row{display:flex;justify-content:space-between;gap:10px;align-items:center}
.item{padding:11px 0;border-bottom:1px solid #edf1ef}.item:last-child{border:0}
.muted{color:var(--muted);font-size:12px}.pill{font-size:11px;padding:4px 7px;border-radius:20px;background:var(--mint);color:var(--green)}
.empty{text-align:center;color:var(--muted);padding:20px 8px}
.actions{display:flex;gap:8px;flex-wrap:wrap;margin-top:10px}
hr{border:0;border-top:1px solid #edf1ef;margin:12px 0}
</style></head>
<body>
<header><h1>Manajemen Toko</h1><p>Stok barang masuk, keluar & penggajian harian</p></header>
<main>
<div class="nav">
<button class="active" data-tab="home">Beranda</button><button data-tab="stock">Stok</button><button data-tab="trans">Transaksi</button>
<button data-tab="staff">Karyawan</button><button data-tab="pay">Gaji Harian</button><button data-tab="report">Laporan</button>
</div>
<section id="home" class="panel active">
<div class="cards">
<div class="card"><small>Jenis barang</small><strong id="countProducts">0</strong></div>
<div class="card"><small>Total unit stok</small><strong id="totalStock">0</strong></div>
<div class="card"><small>Omzet tercatat</small><strong id="revenue">Rp0</strong></div>
<div class="card"><small>Laba kotor perkiraan</small><strong id="profit">Rp0</strong></div>
</div>
<h2>Ringkasan</h2><div class="box"><div class="row"><span>Total gaji dibayarkan</span><b id="paidWages">Rp0</b></div><hr><div class="row"><span>Barang stok menipis (≤ 5)</span><b id="lowStock">0</b></div></div>
<div class="box"><b>Mulai gunakan aplikasi</b><p class="muted">Tambahkan produk di menu Stok, lalu catat barang masuk atau keluar melalui menu Transaksi.</p></div>
</section>
<section id="stock" class="panel">
<h2>Tambah barang</h2><div class="box">
<label>Nama barang</label><input id="pname" placeholder="Contoh: Air mineral">
<label>Kode barang (opsional)</label><input id="pcode" placeholder="SKU-001">
<label>Stok awal</label><input id="pqty" type="number" min="0" value="0">
<label>Harga modal per unit (Rp)</label><input id="pcost" type="number" min="0" placeholder="Contoh: 3000">
<label>Harga jual per unit (Rp)</label><input id="psell" type="number" min="0" placeholder="Contoh: 5000">
<button class="primary" onclick="addProduct()">Simpan Barang</button></div>
<h2>Daftar barang</h2><div class="box" id="productList"></div>
</section>
<section id="trans" class="panel">
<h2>Catat barang masuk / keluar</h2><div class="box">
<label>Barang</label><select id="tproduct"></select>
<label>Jenis transaksi</label><select id="ttype"><option value="in">Barang masuk</option><option value="out">Barang keluar / penjualan</option></select>
<label>Jumlah unit</label><input id="tqty" type="number" min="1" value="1">
<label>Harga jual per unit (untuk penjualan, opsional)</label><input id="tprice" type="number" min="0" placeholder="Kosongkan untuk memakai harga jual barang">
<label>Catatan</label><input id="tnote" placeholder="Contoh: Pembelian dari pemasok">
<button class="primary" onclick="addTransaction()">Simpan Transaksi</button></div>
<h2>Riwayat transaksi</h2><div class="box" id="transactionList"></div>
</section>
<section id="staff" class="panel">
<h2>Tambah karyawan</h2><div class="box">
<label>Nama karyawan</label><input id="sname" placeholder="Nama lengkap">
<label>Jabatan (opsional)</label><input id="srole" placeholder="Kasir / penjaga toko">
<label>Upah per hari (Rp)</label><input id="swage" type="number" min="0" placeholder="Contoh: 100000">
<button class="primary" onclick="addStaff()">Simpan Karyawan</button></div>
<h2>Daftar karyawan</h2><div class="box" id="staffList"></div>
</section>
<section id="pay" class="panel">
<h2>Hitung gaji harian</h2><div class="box">
<label>Karyawan</label><select id="payStaff"></select>
<label>Tanggal pembayaran</label><input id="payDate" type="date">
<label>Jumlah hari kerja</label><input id="days" type="number" min="0" value="1">
<label>Upah per hari (Rp)</label><input id="dailyWage" type="number" min="0" placeholder="Otomatis dari data karyawan">
<label>Bonus (Rp)</label><input id="bonus" type="number" min="0" value="0">
<label>Potongan (Rp)</label><input id="deduct" type="number" min="0" value="0">
<div class="box" style="background:#e8f3ef;margin-top:12px"><div class="row"><b>Total gaji</b><strong id="salaryTotal">Rp0</strong></div><small class="muted">Hari kerja × upah harian + bonus − potongan</small></div>
<button class="primary" onclick="paySalary()">Simpan Pembayaran Gaji</button></div>
<h2>Riwayat pembayaran</h2><div class="box" id="payList"></div>
</section>
<section id="report" class="panel">
<h2>Laporan toko</h2><div class="box" id="reportSummary"></div>
<div class="actions"><button class="secondary" onclick="exportCSV()">Ekspor CSV</button><button class="secondary" onclick="backup()">Cadangkan Data (JSON)</button><button class="secondary" onclick="document.getElementById('restoreFile').click()">Pulihkan Cadangan</button><input id="restoreFile" type="file" accept="application/json" style="display:none" onchange="restore(event)"></div>
<p class="muted">Simpan cadangan secara berkala. Data hanya tersimpan di perangkat ini.</p>
</section>
</main>
<script>
const $=id=>document.getElementById(id), money=n=>'Rp'+Number(n||0).toLocaleString('id-ID');
let db={products:[],transactions:[],staff:[],payments:[]};
try{db=Object.assign(db,JSON.parse(localStorage.getItem('tokoku_db')||'{}'))}catch(e){}
const save=()=>{localStorage.setItem('tokoku_db',JSON.stringify(db));render()};
const esc=s=>String(s??'').replace(/[&<>"']/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
document.querySelectorAll('.nav button').forEach(b=>b.onclick=()=>{document.querySelectorAll('.nav button').forEach(x=>x.classList.toggle('active',x===b));document.querySelectorAll('.panel').forEach(x=>x.classList.toggle('active',x.id===b.dataset.tab));render()});
function addProduct(){let name=$('pname').value.trim(),qty=+$('pqty').value,cost=+$('pcost').value,sell=+$('psell').value;if(!name||qty<0||cost<0||sell<0)return alert('Lengkapi nama barang dan angka yang valid.');db.products.push({id:Date.now(),name,code:$('pcode').value,cost,sell,qty});['pname','pcode','pcost','psell'].forEach(i=>$(i).value='');$('pqty').value=0;save()}
function addTransaction(){let p=db.products.find(x=>x.id==$('tproduct').value),type=$('ttype').value,qty=+$('tqty').value;if(!p)return alert('Tambahkan barang terlebih dahulu.');if(qty<1)return alert('Jumlah harus lebih dari 0.');if(type==='out'&&qty>p.qty)return alert('Stok tidak mencukupi.');let price=+$('tprice').value||p.sell;p.qty+=type==='in'?qty:-qty;db.transactions.unshift({date:new Date().toISOString(),product:p.name,type,qty,price,cost:p.cost,note:$('tnote').value});$('tqty').value=1;$('tprice').value='';$('tnote').value='';save()}
function addStaff(){let name=$('sname').value.trim(),wage=+$('swage').value;if(!name||wage<0)return alert('Isi nama dan upah harian.');db.staff.push({id:Date.now(),name,role:$('srole').value,wage});$('sname').value='';$('srole').value='';$('swage').value='';save()}
function paySalary(){let s=db.staff.find(x=>x.id==$('payStaff').value),days=+$('days').value,wage=+$('dailyWage').value||s?.wage||0,bonus=+$('bonus').value,deduct=+$('deduct').value;if(!s)return alert('Tambahkan karyawan terlebih dahulu.');if(days<0||wage<0||bonus<0||deduct<0)return alert('Periksa angka gaji.');let total=days*wage+bonus-deduct;if(total<0)return alert('Potongan tidak boleh melebihi total gaji.');db.payments.unshift({date:$('payDate').value||new Date().toISOString().slice(0,10),name:s.name,days,wage,bonus,deduct,total});$('bonus').value=0;$('deduct').value=0;save()}
function removeProduct(id){if(!confirm('Hapus barang ini? Riwayat transaksi tetap tersimpan.'))return;db.products=db.products.filter(p=>p.id!==id);save()}
function removeStaff(id){if(!confirm('Hapus data karyawan ini? Riwayat gaji tetap tersimpan.'))return;db.staff=db.staff.filter(s=>s.id!==id);save()}
function render(){
$('countProducts').textContent=db.products.length;$('totalStock').textContent=db.products.reduce((a,p)=>a+p.qty,0);
let sales=db.transactions.filter(t=>t.type==='out');let rev=sales.reduce((a,t)=>a+t.qty*t.price,0),profit=sales.reduce((a,t)=>a+t.qty*(t.price-t.cost),0);
$('revenue').textContent=money(rev);$('profit').textContent=money(profit);$('paidWages').textContent=money(db.payments.reduce((a,p)=>a+p.total,0));$('lowStock').textContent=db.products.filter(p=>p.qty<=5).length;
$('productList').innerHTML=db.products.length?db.products.map(p=>`<div class="item row"><div><b>${esc(p.name)}</b><div class="muted">${esc(p.code)} · Modal ${money(p.cost)} · Jual ${money(p.sell)}</div><span class="pill">Stok: ${p.qty}</span></div><button class="danger" onclick="removeProduct(${p.id})">Hapus</button></div>`).join(''):'<div class="empty">Belum ada barang.</div>';
$('tproduct').innerHTML=db.products.map(p=>`<option value="${p.id}">${esc(p.name)} (stok ${p.qty})</option>`).join('')||'<option value="">Belum ada barang</option>';
$('staffList').innerHTML=db.staff.length?db.staff.map(s=>`<div class="item row"><div><b>${esc(s.name)}</b><div class="muted">${esc(s.role)} · ${money(s.wage)}/hari</div></div><button class="danger" onclick="removeStaff(${s.id})">Hapus</button></div>`).join(''):'<div class="empty">Belum ada karyawan.</div>';
$('payStaff').innerHTML=db.staff.map(s=>`<option value="${s.id}">${esc(s.name)}</option>`).join('')||'<option value="">Belum ada karyawan</option>';
let selected=db.staff.find(s=>s.id==$('payStaff').value)||db.staff[0];if(selected&&!$('dailyWage').dataset.edited)$('dailyWage').value=selected.wage;
$('transactionList').innerHTML=db.transactions.length?db.transactions.slice(0,60).map(t=>`<div class="item"><div class="row"><b>${esc(t.product)}</b><span class="pill">${t.type==='in'?'Masuk':'Keluar'}</span></div><div class="muted">${new Date(t.date).toLocaleString('id-ID')} · ${t.qty} unit ${t.type==='out'?'· '+money(t.price)+' / unit':''}</div><div class="muted">${esc(t.note)}</div></div>`).join(''):'<div class="empty">Belum ada transaksi.</div>';
$('payList').innerHTML=db.payments.length?db.payments.map(p=>`<div class="item"><div class="row"><b>${esc(p.name)}</b><b>${money(p.total)}</b></div><div class="muted">${p.date} · ${p.days} hari × ${money(p.wage)} + bonus ${money(p.bonus)} − potongan ${money(p.deduct)}</div></div>`).join(''):'<div class="empty">Belum ada pembayaran gaji.</div>';
$('reportSummary').innerHTML=`<div class="row"><span>Total barang</span><b>${db.products.length}</b></div><hr><div class="row"><span>Total unit tersisa</span><b>${db.products.reduce((a,p)=>a+p.qty,0)}</b></div><hr><div class="row"><span>Omzet penjualan</span><b>${money(rev)}</b></div><hr><div class="row"><span>Perkiraan laba kotor</span><b>${money(profit)}</b></div><hr><div class="row"><span>Total gaji tercatat</span><b>${money(db.payments.reduce((a,p)=>a+p.total,0))}</b></div>`;
updateSalary();
}
function updateSalary(){let s=db.staff.find(x=>x.id==$('payStaff').value)||db.staff[0];let wage=+$('dailyWage').value||s?.wage||0;$('salaryTotal').textContent=money(+$('days').value*wage + +$('bonus').value - +$('deduct').value)}
['days','dailyWage','bonus','deduct'].forEach(id=>$(id).addEventListener('input',updateSalary));
$('payStaff').addEventListener('change',()=>{let s=db.staff.find(x=>x.id==$('payStaff').value);$('dailyWage').value=s?.wage||0;$('dailyWage').dataset.edited='';updateSalary()});
$('dailyWage').addEventListener('input',()=>{$('dailyWage').dataset.edited='1'});
function backup(){let blob=new Blob([JSON.stringify(db,null,2)],{type:'application/json'}),a=document.createElement('a');a.href=URL.createObjectURL(blob);a.download='cadangan-data-toko.json';a.click();URL.revokeObjectURL(a.href)}
function restore(e){let f=e.target.files[0];if(!f)return;let r=new FileReader();r.onload=()=>{try{let d=JSON.parse(r.result);if(!Array.isArray(d.products)||!Array.isArray(d.staff))throw 0;db=d;save();alert('Cadangan berhasil dipulihkan.')}catch(_){alert('File cadangan tidak valid.')}};r.readAsText(f)}
function exportCSV(){let rows=[['Jenis','Tanggal','Nama','Jumlah/Hari','Nilai','Catatan']];db.transactions.forEach(t=>rows.push(['Transaksi '+(t.type==='in'?'Masuk':'Keluar'),t.date,t.product,t.qty,t.qty*t.price,t.note]));db.payments.forEach(p=>rows.push(['Gaji',p.date,p.name,p.days,p.total,'Upah harian '+p.wage]));let csv=rows.map(r=>r.map(x=>'"'+String(x??'').replace(/"/g,'""')+'"').join(',')).join('\\n');let a=document.createElement('a');a.href=URL.createObjectURL(new Blob([csv],{type:'text/csv'}));a.download='laporan-toko.csv';a.click();URL.revokeObjectURL(a.href)}
$('payDate').value=new Date().toISOString().slice(0,10);render();
if ('serviceWorker' in navigator) { window.addEventListener('load', () => navigator.serviceWorker.register('./sw.js').catch(()=>{})); }
</script></body></html>
