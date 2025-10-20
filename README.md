<!doctype html>
<html lang="th">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>บันทึกยอดขายพาร์ทไทม์ — ป๋อมแป๋ม</title>
<link href="https://fonts.googleapis.com/css2?family=Mitr:wght@300;400;600&display=swap" rel="stylesheet">
<style>
  body {
    font-family: "Mitr", sans-serif;
    background: #fff8fb;
    color: #333;
    padding: 20px;
  }
  h1 {
    text-align: center;
    color: #d63384;
  }
  .card {
    background: #fff;
    border-radius: 20px;
    padding: 20px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    max-width: 550px;
    margin: 15px auto;
  }
  label {
    display: block;
    margin-top: 10px;
    font-weight: 600;
  }
  input {
    width: 100%;
    padding: 8px;
    border-radius: 8px;
    border: 1px solid #ccc;
  }
  button {
    margin: 6px 4px;
    padding: 10px 16px;
    border: none;
    border-radius: 10px;
    background: #ff66a3;
    color: white;
    font-weight: 600;
    cursor: pointer;
    transition: 0.2s;
  }
  button:hover {
    background: #ff3385;
    transform: translateY(-1px);
  }
  table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 15px;
  }
  th, td {
    border: 1px solid #ddd;
    text-align: center;
    padding: 6px;
  }
  th {
    background: #ffb3d9;
  }
  .location-btns button {
    background: #ffd6e8;
    color: #333;
  }
  .location-btns button.active {
    background: #ff66a3;
    color: white;
  }
  #totalDisplay {
    text-align: center;
    font-size: 1.2em;
    color: #d63384;
    margin-top: 10px;
  }
  /* Popup slip */
  .popup {
    display: none;
    position: fixed;
    top: 0; left: 0;
    width: 100%; height: 100%;
    background: rgba(0,0,0,0.4);
    justify-content: center;
    align-items: center;
  }
  .popup-content {
    background: white;
    border-radius: 20px;
    padding: 20px;
    max-width: 400px;
    text-align: center;
    box-shadow: 0 4px 15px rgba(0,0,0,0.2);
  }
  .popup-content h2 {
    color: #d63384;
  }
  .close-btn {
    background: #ccc;
    color: black;
    margin-top: 10px;
  }
  .close-btn:hover {
    background: #bbb;
  }
</style>
</head>
<body>

<h1>🍹 บันทึกยอดขายพาร์ทไทม์ — ป๋อมแป๋ม 💕</h1>

<div class="card">
  <label>จำนวนชั่วโมง:</label>
  <input id="hours" type="number" placeholder="เช่น 5">

  <label>จำนวนแก้ว:</label>
  <input id="cups" type="number" placeholder="เช่น 60">

  <label>เลือกสถานที่:</label>
  <div class="location-btns">
    <button type="button" onclick="selectLocation(this, 'ซอยวุ่นวาย')">ซอยวุ่นวาย</button>
    <button type="button" onclick="selectLocation(this, 'หน้ามอ')">หน้ามอ</button>
    <button type="button" onclick="selectLocation(this, 'ลาเดอมา')">ลาเดอมา</button>
    <button type="button" onclick="selectLocation(this, 'ท่าขอนยาง')">ท่าขอนยาง</button>
    <button type="button" onclick="selectLocation(this, 'ขามเรียง')">ขามเรียง</button>
  </div>

  <button onclick="saveData()">💾 บันทึกข้อมูล</button>
  <button onclick="showTotal()">💰 ดูยอดสะสม</button>
  <button onclick="showSlip()">🧾 ทำสลิปเบิกเงิน</button>
  <button onclick="resetWeek()">✂️ รีเซ็ตสัปดาห์</button>
</div>

<div class="card">
  <h3>📊 ตารางสรุปยอด</h3>
  <table id="dataTable">
    <thead>
      <tr>
        <th>วันที่</th>
        <th>ชั่วโมง</th>
        <th>แก้ว</th>
        <th>ค่าแรง</th>
        <th>คอมมิชชั่น</th>
        <th>รวม</th>
        <th>สถานที่</th>
      </tr>
    </thead>
    <tbody></tbody>
  </table>
  <div id="totalDisplay"></div>
</div>

<!-- Popup slip -->
<div id="slipPopup" class="popup">
  <div class="popup-content">
    <h2>🧾 สลิปเบิกเงิน</h2>
    <div id="slipDetails"></div>
    <button class="close-btn" onclick="closeSlip()">ปิด</button>
  </div>
</div>

<script>
const SCRIPT_URL = 'https://script.google.com/macros/s/AKfycbyPIlxSTPMQ7vm7EAGzz4RWgC67-0jNlhZACLnJqEfjwvNh0hz_GWn2BZKKwORRq7nfbw/exec';
let selectedLocation = '';

function selectLocation(btn, loc) {
  document.querySelectorAll('.location-btns button').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  selectedLocation = loc;
}

// ✅ บันทึกข้อมูล
async function saveData() {
  const hours = document.getElementById('hours').value;
  const cups = document.getElementById('cups').value;
  if (!selectedLocation) return alert('กรุณาเลือกสถานที่ก่อนบันทึก');
  if (!hours || !cups) return alert('กรอกข้อมูลให้ครบก่อนบันทึก');

  const res = await fetch(SCRIPT_URL, {
    method: 'POST',
    body: JSON.stringify({ hours, cups, location: selectedLocation }),
  });
  const data = await res.json();

  if (data.status === 'success') {
    alert('✅ บันทึกเรียบร้อย');
    document.getElementById('hours').value = '';
    document.getElementById('cups').value = '';
    loadTable();
  } else {
    alert('เกิดข้อผิดพลาด: ' + data.message);
  }
}

// ✅ โหลดข้อมูลทั้งหมด
async function loadTable() {
  const res = await fetch(SCRIPT_URL);
  const result = await res.json();
  const tbody = document.querySelector('#dataTable tbody');
  tbody.innerHTML = '';

  result.forEach(r => {
    const row = document.createElement('tr');
    row.innerHTML = `
      <td>${r.date}</td>
      <td>${r.hours}</td>
      <td>${r.cups}</td>
      <td>${r.wage}</td>
      <td>${r.commission}</td>
      <td>${r.total}</td>
      <td>${r.location || '-'}</td>
    `;
    tbody.appendChild(row);
  });
}

// ✅ แสดงยอดสะสมทั้งหมด
async function showTotal() {
  const res = await fetch(SCRIPT_URL);
  const result = await res.json();
  const total = result.reduce((sum, r) => sum + Number(r.total || 0), 0);
  document.getElementById('totalDisplay').textContent = `💰 ยอดสะสมทั้งหมด: ${total.toLocaleString()} บาท`;
}

// ✅ สลิปเบิกเงิน (Popup)
async function showSlip() {
  const res = await fetch(SCRIPT_URL);
  const result = await res.json();
  const total = result.reduce((sum, r) => sum + Number(r.total || 0), 0);
  const slipHTML = `
    <p><strong>ชื่อ:</strong> ป๋อมแป๋ม</p>
    <p><strong>วันที่:</strong> ${new Date().toLocaleDateString('th-TH')}</p>
    <p><strong>ยอดรวมที่เบิกได้:</strong> ${total.toLocaleString()} บาท</p>
    <p>ขอบคุณสำหรับความตั้งใจในการทำงาน 💕</p>
  `;
  document.getElementById('slipDetails').innerHTML = slipHTML;
  document.getElementById('slipPopup').style.display = 'flex';
}

function closeSlip() {
  document.getElementById('slipPopup').style.display = 'none';
}

// ✅ รีเซ็ตสัปดาห์
async function resetWeek() {
  if (!confirm('แน่ใจหรือไม่ว่าจะรีเซ็ตข้อมูลสัปดาห์นี้?')) return;
  const res = await fetch(SCRIPT_URL, {
    method: 'POST',
    body: JSON.stringify({ action: 'resetWeek' }),
  });
  const data = await res.json();
  if (data.status === 'week_reset') {
    alert('✂️ รีเซ็ตข้อมูลเรียบร้อย');
    loadTable();
    document.getElementById('totalDisplay').textContent = '';
  }
}

loadTable();
</script>
</body>
</html>
