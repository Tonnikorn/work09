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
  --toast-bg: #fff;
}
body.dark {
  --bg: #1e1e2f;
  --card: #2c2c3a;
  --text: #f4f4f9;
  --accent: #ff82b5;
  --accent-hover: #ff4d94;
  --border: #4a4a60;
  --th-bg: #3b3b54;
  --toast-bg: #2e2e3f;
}

body {
  font-family: "Mitr", sans-serif;
  background: var(--bg);
  color: var(--text);
  padding: 20px;
  transition: background 0.3s, color 0.3s;
  overflow-x: hidden;
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
tr:nth-child(even) { background: rgba(255,255,255,0.4); }
body.dark tr:nth-child(even) { background: rgba(60,60,80,0.4); }

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
  font-size: 1.1em;
  color: var(--accent);
  margin-top: 12px;
  font-weight: 600;
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
  z-index: 300;
}
.theme-toggle:hover {
  transform: rotate(20deg) scale(1.1);
  background: var(--accent-hover);
}

/* Toast Notification */
#toast {
  position: fixed;
  bottom: 20px;
  right: 20px;
  background: var(--toast-bg);
  color: var(--text);
  border-radius: 10px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.2);
  padding: 12px 18px;
  font-weight: 500;
  display: none;
  z-index: 500;
}
</style>
</head>
<body>

<button class="theme-toggle" onclick="toggleTheme()">🌙</button>
<h1>🍹 บันทึกยอดขายพาร์ทไทม์💕</h1>

<div class="card">
  <label>จำนวนชั่วโมง:</label>
  <input id="hours" type="number" placeholder="เช่น 5" min="0">

  <label>จำนวนแก้ว:</label>
  <input id="cups" type="number" placeholder="เช่น 60" min="0">

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
    <button onclick="resetAll()">🧹 เริ่มใหม่ทั้งหมด</button>
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

<!-- Toast -->
<div id="toast"></div>

<script>
const SCRIPT_URL = "https://script.google.com/macros/s/AKfycbxp-D6R3wPq9mlwKnbvBi17_t9Fk0FY5ypURIgwm7PCo1CRKdY1VeSXFNGJAs_iP4VZ4w/exec";
let selectedLocation = '';
let isDark = localStorage.getItem('theme') === 'dark';
document.body.classList.toggle('dark', isDark);
document.querySelector('.theme-toggle').textContent = isDark ? '☀️' : '🌙';

// ----- Loading Popup -----
function showLoading(msg="กำลังทำงาน กรุณารอสักครู่...") {
  document.getElementById('loadingText').textContent = msg;
  document.getElementById('loadingPopup').style.display = 'flex';
}
function hideLoading() { document.getElementById('loadingPopup').style.display = 'none'; }

// ----- Toast -----
function showToast(msg) {
  const toast = document.getElementById('toast');
  toast.textContent = msg;
  toast.style.display = 'block';
  setTimeout(() => toast.style.display = 'none', 2000);
}

// ----- Theme -----
function toggleTheme() {
  isDark = !isDark;
  document.body.classList.toggle('dark', isDark);
  document.querySelector('.theme-toggle').textContent = isDark ? '☀️' : '🌙';
  localStorage.setItem('theme', isDark ? 'dark' : 'light');
}

// ----- Location -----
function selectLocation(btn, loc) {
  document.querySelectorAll('.location-btns button').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  selectedLocation = loc;
}

// ----- Save Data -----
async function saveData() {
  const hours = document.getElementById('hours').value.trim();
  const cups = document.getElementById('cups').value.trim();
  if (!selectedLocation) return showToast('⚠️ กรุณาเลือกสถานที่');
  if (!hours || !cups) return showToast('⚠️ กรอกข้อมูลให้ครบ');
  showLoading('💾 กำลังบันทึกข้อมูล...');

  const res = await fetch(SCRIPT_URL, {
    method: 'POST',
    body: JSON.stringify({ hours, cups, location: selectedLocation }),
  });
  const data = await res.json();
  hideLoading();

  if (data.status === 'success') {
    showToast('✅ บันทึกสำเร็จ');
    document.getElementById('hours').value = '';
    document.getElementById('cups').value = '';
    loadTable(true);
  } else showToast('❌ ' + data.message);
}

// ----- Load Table -----
let dataCache = [];
async function loadTable(force=false) {
  if (!force && dataCache.length) return renderTable(dataCache);
  showLoading('📊 กำลังโหลดข้อมูล...');
  const res = await fetch(SCRIPT_URL);
  dataCache = await res.json();
  hideLoading();
  renderTable(dataCache);
}

function renderTable(result) {
  const tbody = document.querySelector('#dataTable tbody');
  tbody.innerHTML = '';
  let total = 0;
  result.forEach(r => {
    const row = document.createElement('tr');
    total += Number(r.total || 0);
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
  document.getElementById('totalDisplay').innerHTML = `💰 ยอดรวมทั้งหมด: ${total.toLocaleString()} บาท`;
}

async function confirmDelete(date, location) {
  if (!confirm(`ลบข้อมูลวันที่ ${date} (${location}) ?`)) return;
  showLoading('🗑️ กำลังลบข้อมูล...');
  const res = await fetch(SCRIPT_URL, {
    method: 'POST',
    body: JSON.stringify({ action: 'delete', date, location }),
  });
  const data = await res.json();
  hideLoading();
  if (data.status === 'deleted') {
    showToast('🗑️ ลบข้อมูลแล้ว');
    loadTable(true);
  }
}

// ----- Total & Slip -----
async function showTotal() {
  const total = dataCache.reduce((sum, r) => sum + Number(r.total || 0), 0);
  showToast(`💰 ยอดสะสม: ${total.toLocaleString()} บาท`);
}
async function showSlip() {
  const total = dataCache.reduce((sum, r) => sum + Number(r.total || 0), 0);
  const slipHTML = `
    <p><strong>ชื่อ:</strong> ป๋อมแป๋ม</p>
    <p><strong>วันที่:</strong> ${new Date().toLocaleDateString('th-TH')}</p>
    <p><strong>ยอดรวมที่เบิกได้:</strong> ${total.toLocaleString()} บาท</p>
    <p>ขอบคุณสำหรับความตั้งใจในการทำงาน 💕</p>
  `;
  document.getElementById('slipDetails').innerHTML = slipHTML;
  document.getElementById('slipPopup').style.display = 'flex';
}
function closeSlip() { document.getElementById('slipPopup').style.display = 'none'; }

// ----- Reset All -----
async function resetAll() {
  if (!confirm('⚠️ ต้องการเริ่มใหม่ทั้งหมดใช่ไหม?')) return;
  showLoading('🧹 กำลังล้างข้อมูลทั้งหมด...');
  const res = await fetch(SCRIPT_URL, {
    method: 'POST',
    body: JSON.stringify({ action: 'resetAll' }),
  });
  const data = await res.json();
  hideLoading();
  if (data.status === 'all_reset') {
    showToast('🧹 ล้างข้อมูลแล้ว');
    dataCache = [];
    loadTable(true);
  }
}

loadTable();
</script>
</body>
</html>
