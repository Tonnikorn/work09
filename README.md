<!doctype html>
<html lang="th">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>บันทึกยอดขายพาร์ทไทม์</title>
<link href="https://fonts.googleapis.com/css2?family=Mitr:wght@300;400;600&display=swap" rel="stylesheet">
<style>
:root {
  --bg: #fff0f6;
  --card: #ffffffdd;
  --text: #333;
  --accent: #ff66a3;
  --accent-hover: #ff3385;
  --border: #f5c6db;
  --th-bg: #ffd6e8;
}
body.dark {
  --bg: #1e1e2f;
  --card: #2c2c3a;
  --text: #f4f4f9;
  --accent: #ff82b5;
  --accent-hover: #ff4d94;
  --border: #4a4a60;
  --th-bg: #3b3b54;
}

body {
  font-family: "Mitr", sans-serif;
  background: var(--bg);
  color: var(--text);
  padding: 20px;
  transition: background 0.3s, color 0.3s;
}
h1 {
  text-align: center;
  color: var(--accent);
  font-size: 1.9em;
  margin-bottom: 20px;
  text-shadow: 0 2px 4px rgba(0,0,0,0.1);
}
.card {
  background: var(--card);
  border-radius: 22px;
  padding: 20px;
  box-shadow: 0 4px 12px rgba(255,102,163,0.15);
  max-width: 620px;
  margin: 15px auto;
  transition: 0.3s;
}
.card:hover { transform: translateY(-3px); box-shadow: 0 6px 14px rgba(255,102,163,0.2); }

label { display: block; margin-top: 10px; font-weight: 600; }

input {
  width: 100%;
  padding: 10px;
  border-radius: 12px;
  border: 1px solid var(--border);
  background: #fff;
  font-size: 1em;
  transition: 0.2s;
}
body.dark input {
  background: #3a3a4b;
  color: #fff;
}
input:focus {
  border-color: var(--accent);
  outline: none;
  box-shadow: 0 0 4px var(--accent);
}

button {
  margin: 8px 5px;
  padding: 10px 18px;
  border: none;
  border-radius: 14px;
  background: var(--accent);
  color: white;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s ease;
}
button:hover {
  background: var(--accent-hover);
  transform: scale(1.05);
  box-shadow: 0 0 10px rgba(255,102,163,0.4);
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 15px;
  animation: fadeIn 0.5s ease;
  border-radius: 10px;
  overflow: hidden;
}
@keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
th, td {
  border: 1px solid var(--border);
  text-align: center;
  padding: 6px;
}
th {
  background: var(--th-bg);
  color: var(--accent);
}

.location-btns {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin-top: 8px;
}
.location-btns button {
  flex: 1 1 30%;
  background: #ffeaf3;
  color: #333;
  font-size: 0.9em;
  transition: 0.2s;
}
.location-btns button.active {
  background: var(--accent);
  color: white;
  transform: scale(1.05);
}
body.dark .location-btns button {
  background: #3a3a4b;
  color: #eee;
}
#totalDisplay {
  text-align: center;
  font-size: 1.2em;
  color: var(--accent);
  margin-top: 10px;
}

/* ปุ่มลบ */
.delete-btn {
  background: #ffb3c6;
  border: none;
  border-radius: 10px;
  padding: 5px 10px;
  cursor: pointer;
  font-size: 0.9em;
  color: #fff;
  font-weight: 600;
  transition: 0.2s;
}
.delete-btn:hover {
  background: #ff4d6d;
  transform: scale(1.1);
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
  animation: fadeIn 0.3s ease;
  z-index: 100;
}
.popup-content {
  background: var(--card);
  border-radius: 20px;
  padding: 25px;
  max-width: 400px;
  text-align: center;
  box-shadow: 0 4px 15px rgba(0,0,0,0.2);
  animation: pop 0.3s ease;
}
@keyframes pop { from { transform: scale(0.8); opacity: 0; } to { transform: scale(1); opacity: 1; } }
.popup-content h2 { color: var(--accent); }
.close-btn {
  background: #ccc;
  color: black;
  margin-top: 10px;
}
body.dark .close-btn { background: #555; color: #eee; }
.close-btn:hover { background: #bbb; }

/* Loading Popup */
#loadingPopup {
  display: none;
  position: fixed;
  top: 0; left: 0;
  width: 100%; height: 100%;
  background: rgba(0,0,0,0.4);
  z-index: 200;
  justify-content: center;
  align-items: center;
}
.loading-box {
  background: var(--card);
  border-radius: 18px;
  padding: 25px 30px;
  text-align: center;
  box-shadow: 0 4px 20px rgba(0,0,0,0.3);
  animation: pop 0.3s ease;
}
.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #ddd;
  border-top: 4px solid var(--accent);
  border-radius: 50%;
  margin: 0 auto 15px;
  animation: spin 1s linear infinite;
}
@keyframes spin { from { transform: rotate(0deg);} to { transform: rotate(360deg);} }

/* ปุ่มโหมดมืด */
.theme-toggle {
  position: fixed;
  top: 15px;
  right: 15px;
  background: var(--accent);
  color: white;
  border: none;
  border-radius: 50%;
  width: 45px;
  height: 45px;
  cursor: pointer;
  box-shadow: 0 4px 8px rgba(0,0,0,0.15);
  transition: 0.3s;
  font-size: 1.2em;
}
.theme-toggle:hover {
  transform: rotate(20deg) scale(1.1);
  background: var(--accent-hover);
}
</style>
</head>
<body>

<button class="theme-toggle" onclick="toggleTheme()">🌙</button>

<h1>🍹 บันทึกยอดขายพาร์ทไทม์💕</h1>

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

  <div style="text-align:center;">
    <button onclick="saveData()">💾 บันทึกข้อมูล</button>
    <button onclick="showTotal()">💰 ดูยอดสะสม</button>
    <button onclick="showSlip()">🧾 สลิปเบิกเงิน</button>
    <button onclick="resetWeek()">✂️ รีเซ็ตสัปดาห์</button>
  </div>
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
        <th>ลบ</th>
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

<!-- Popup Loading -->
<div id="loadingPopup" class="popup">
  <div class="loading-box">
    <div class="spinner"></div>
    <div id="loadingText">กำลังทำงาน กรุณารอสักครู่...</div>
  </div>
</div>

<script>
const SCRIPT_URL = '[https://script.google.com/macros/s/AKfycbxp-D6R3wPq9mlwKnbvBi17_t9Fk0FY5ypURIgwm7PCo1CRKdY1VeSXFNGJAs_iP4VZ4w/exec](https://script.google.com/macros/s/AKfycbxp-D6R3wPq9mlwKnbvBi17_t9Fk0FY5ypURIgwm7PCo1CRKdY1VeSXFNGJAs_iP4VZ4w/exec)';
let selectedLocation = '';
let isDark = false;

// ฟังก์ชันแสดง/ซ่อน popup โหลด
function showLoading(msg="กำลังทำงาน กรุณารอสักครู่...") {
  document.getElementById('loadingText').textContent = msg;
  document.getElementById('loadingPopup').style.display = 'flex';
}
function hideLoading() {
  document.getElementById('loadingPopup').style.display = 'none';
}

// โหมดมืด
function toggleTheme() {
  isDark = !isDark;
  document.body.classList.toggle('dark', isDark);
  document.querySelector('.theme-toggle').textContent = isDark ? '☀️' : '🌙';
  localStorage.setItem('theme', isDark ? 'dark' : 'light');
}
if (localStorage.getItem('theme') === 'dark') {
  document.body.classList.add('dark');
  document.querySelector('.theme-toggle').textContent = '☀️';
  isDark = true;
}

function selectLocation(btn, loc) {
  document.querySelectorAll('.location-btns button').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  selectedLocation = loc;
}

// ✅ บันทึกข้อมูล
async function saveData() {
  const hours = document.getElementById('hours').value.trim();
  const cups = document.getElementById('cups').value.trim();
  if (!selectedLocation) return alert('กรุณาเลือกสถานที่ก่อนบันทึก');
  if (!hours || !cups) return alert('กรอกข้อมูลให้ครบก่อนบันทึก');
  showLoading('💾 กำลังบันทึกข้อมูล...');

  const resCheck = await fetch(SCRIPT_URL);
  const existing = await resCheck.json();
  const today = new Date().toLocaleDateString('th-TH');
  const duplicate = existing.find(r => r.date === today && r.location === selectedLocation);

  if (duplicate) {
    hideLoading();
    if (confirm(`ข้อมูลวันที่ ${today} (${selectedLocation}) มีอยู่แล้ว ต้องการลบข้อมูลเดิมหรือไม่?`)) {
      await deleteData(today, selectedLocation);
      alert('🗑️ ลบข้อมูลเดิมเรียบร้อยแล้ว');
    } else return;
    showLoading('💾 กำลังบันทึกข้อมูลใหม่...');
  }

  const res = await fetch(SCRIPT_URL, {
    method: 'POST',
    body: JSON.stringify({ hours, cups, location: selectedLocation }),
  });
  const data = await res.json();
  hideLoading();

  if (data.status === 'success') {
    alert('✅ บันทึกเรียบร้อย');
    document.getElementById('hours').value = '';
    document.getElementById('cups').value = '';
    loadTable();
  } else alert('เกิดข้อผิดพลาด: ' + data.message);
}

async function loadTable() {
  showLoading('📊 กำลังโหลดข้อมูล...');
  const res = await fetch(SCRIPT_URL);
  const result = await res.json();
  hideLoading();
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
      <td><button class="delete-btn" onclick="confirmDelete('${r.date}','${r.location}')">🗑️</button></td>
    `;
    tbody.appendChild(row);
  });
}

function confirmDelete(date, location) {
  if (confirm(`แน่ใจหรือไม่ว่าต้องการลบข้อมูลวันที่ ${date} (${location}) ?`)) {
    deleteData(date, location);
  }
}

async function deleteData(date, location) {
  showLoading('🗑️ กำลังลบข้อมูล...');
  const res = await fetch(SCRIPT_URL, {
    method: 'POST',
    body: JSON.stringify({ action: 'delete', date, location }),
  });
  const data = await res.json();
  hideLoading();
  if (data.status === 'deleted') {
    alert('🗑️ ลบข้อมูลเรียบร้อย');
    loadTable();
  }
}

async function showTotal() {
  showLoading('💰 กำลังคำนวณยอดสะสม...');
  const res = await fetch(SCRIPT_URL);
  const result = await res.json();
  hideLoading();
  const total = result.reduce((sum, r) => sum + Number(r.total || 0), 0);
  document.getElementById('totalDisplay').textContent = `💰 ยอดสะสมทั้งหมด: ${total.toLocaleString()} บาท`;
}

async function showSlip() {
  showLoading('🧾 กำลังสร้างสลิป...');
  const res = await fetch(SCRIPT_URL);
  const result = await res.json();
  hideLoading();
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

async function resetWeek() {
  if (!confirm('แน่ใจหรือไม่ว่าจะรีเซ็ตข้อมูลสัปดาห์นี้?')) return;
  showLoading('✂️ กำลังรีเซ็ตข้อมูล...');
  const res = await fetch(SCRIPT_URL, {
    method: 'POST',
    body: JSON.stringify({ action: 'resetWeek' }),
  });
  const data = await res.json();
  hideLoading();
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
