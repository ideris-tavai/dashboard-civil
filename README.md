<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Dashboard Konstruksi</title>

<style>
*{
    margin:0;
    padding:0;
    box-sizing:border-box;
    font-family:Arial, sans-serif;
}

body{
    background:#f4f6f9;
}

header{
    background:#1f4e79;
    color:white;
    padding:15px;
    text-align:center;
}

.container{
    width:95%;
    margin:auto;
    margin-top:20px;
}

.cards{
    display:grid;
    grid-template-columns:repeat(auto-fit,minmax(250px,1fr));
    gap:15px;
}

.card{
    background:white;
    padding:20px;
    border-radius:10px;
    box-shadow:0 2px 5px rgba(0,0,0,0.1);
}

.card h3{
    color:#1f4e79;
}

.section{
    background:white;
    padding:20px;
    margin-top:20px;
    border-radius:10px;
    box-shadow:0 2px 5px rgba(0,0,0,0.1);
}

input, select{
    width:100%;
    padding:10px;
    margin-top:5px;
    margin-bottom:10px;
}

button{
    background:#1f4e79;
    color:white;
    border:none;
    padding:10px 15px;
    cursor:pointer;
    border-radius:5px;
}

button:hover{
    background:#163c5f;
}

table{
    width:100%;
    border-collapse:collapse;
    margin-top:10px;
}

table th, table td{
    border:1px solid #ddd;
    padding:8px;
    text-align:center;
}

th{
    background:#1f4e79;
    color:white;
}

.progress{
    color:green;
    font-weight:bold;
}
</style>
</head>
<body>

<header>
    <h1>Dashboard Manajemen Konstruksi</h1>
</header>

<div class="container">

    <div class="cards">
        <div class="card">
            <h3>Total Pekerjaan</h3>
            <h2 id="totalPekerjaan">0</h2>
        </div>

        <div class="card">
            <h3>Rata-rata Progres</h3>
            <h2 id="avgProgress">0%</h2>
        </div>

        <div class="card">
            <h3>Data Pemeliharaan</h3>
            <h2 id="totalMaintenance">0</h2>
        </div>
    </div>

    <!-- Pelaksanaan -->
    <div class="section">
        <h2>Pelaksanaan Pekerjaan</h2>

        <input type="date" id="tanggal">

        <input type="text" id="pekerjaan"
        placeholder="Nama Pekerjaan">

        <input type="number" id="target"
        placeholder="Target Volume">

        <input type="number" id="realisasi"
        placeholder="Realisasi Volume">

        <button onclick="tambahPekerjaan()">
            Simpan
        </button>

        <table>
            <thead>
                <tr>
                    <th>Tanggal</th>
                    <th>Pekerjaan</th>
                    <th>Target</th>
                    <th>Realisasi</th>
                    <th>Progres</th>
                </tr>
            </thead>
            <tbody id="tabelPekerjaan"></tbody>
        </table>
    </div>

    <!-- Pengawasan -->
    <div class="section">
        <h2>Checklist Pengawasan</h2>

        <label>
            <input type="checkbox" class="check">
            APD Lengkap
        </label><br>

        <label>
            <input type="checkbox" class="check">
            Material Sesuai
        </label><br>

        <label>
            <input type="checkbox" class="check">
            Area Kerja Aman
        </label><br>

        <label>
            <input type="checkbox" class="check">
            Gambar Kerja Tersedia
        </label><br><br>

        <button onclick="hitungPengawasan()">
            Hitung Nilai
        </button>

        <h3 id="nilaiPengawasan"></h3>
    </div>

    <!-- Pemeliharaan -->
    <div class="section">
        <h2>Pemeliharaan</h2>

        <input type="text"
        id="aset"
        placeholder="Nama Aset">

        <select id="kondisi">
            <option>Baik</option>
            <option>Rusak Ringan</option>
            <option>Rusak Sedang</option>
            <option>Rusak Berat</option>
        </select>

        <input type="text"
        id="tindakan"
        placeholder="Tindakan Perbaikan">

        <button onclick="tambahMaintenance()">
            Simpan
        </button>

        <table>
            <thead>
                <tr>
                    <th>Aset</th>
                    <th>Kondisi</th>
                    <th>Tindakan</th>
                </tr>
            </thead>
            <tbody id="maintenanceTable"></tbody>
        </table>
    </div>

</div>

<script>

let totalProgress = 0;
let totalData = 0;
let maintenanceCount = 0;

function tambahPekerjaan(){

    let tanggal =
    document.getElementById("tanggal").value;

    let pekerjaan =
    document.getElementById("pekerjaan").value;

    let target =
    parseFloat(document.getElementById("target").value);

    let realisasi =
    parseFloat(document.getElementById("realisasi").value);

    if(!target || !realisasi){
        alert("Isi data dengan benar");
        return;
    }

    let progress =
    ((realisasi/target)*100).toFixed(1);

    let row = `
    <tr>
        <td>${tanggal}</td>
        <td>${pekerjaan}</td>
        <td>${target}</td>
        <td>${realisasi}</td>
        <td class="progress">${progress}%</td>
    </tr>`;

    document
    .getElementById("tabelPekerjaan")
    .innerHTML += row;

    totalProgress += parseFloat(progress);
    totalData++;

    document.getElementById("totalPekerjaan")
    .innerText = totalData;

    document.getElementById("avgProgress")
    .innerText =
    (totalProgress/totalData).toFixed(1)+"%";
}

function hitungPengawasan(){

    let checks =
    document.querySelectorAll(".check");

    let benar = 0;

    checks.forEach(c=>{
        if(c.checked) benar++;
    });

    let nilai =
    ((benar/checks.length)*100).toFixed(1);

    document.getElementById("nilaiPengawasan")
    .innerHTML =
    "Nilai Kepatuhan : "+nilai+"%";
}

function tambahMaintenance(){

    let aset =
    document.getElementById("aset").value;

    let kondisi =
    document.getElementById("kondisi").value;

    let tindakan =
    document.getElementById("tindakan").value;

    let row = `
    <tr>
      <td>${aset}</td>
      <td>${kondisi}</td>
      <td>${tindakan}</td>
    </tr>`;

    document
    .getElementById("maintenanceTable")
    .innerHTML += row;

    maintenanceCount++;

    document
    .getElementById("totalMaintenance")
    .innerText = maintenanceCount;
}

</script>

</body>
</html>
