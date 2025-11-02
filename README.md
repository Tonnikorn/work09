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
  --toast-bg: #ff66a3;
}
body { font-family:"Mitr",sans-serif; background: var(--bg); color: var(--text); padding: 20px; transition:0.3s; }
h1 { text-align:center; color:var(--accent); margin-bottom:20px; font-size:1.9em; animation: bounce 1s infinite alternate; }
@keyframes bounce {
  0% { transform: translateY(0); }
  100% { transform: translateY(-5px); }
}
.card { background: var(--card); border-radius:22px; padding:20px; max-width:620px; margin:15px auto; box-shadow:0 8px 20px rgba(255,102,163,0.2); transition:0.3s; }
.card:hover { transform: translateY(-3px); box-shadow:0 12px 25px rgba(255,102,163,0.3); }
label { display:block; margin-top:10px; font-weight:600; }
input { width:100%; padding:12px; border-radius:12px; border:1px solid var(--border); margin-top:5px; font-size:1em; }
button { margin:8px 5px; padding:10px 18px; border:none; border-radius:14px; background:var(--accent); color:white; cursor:pointer; transition:0.2s; font-weight:600; }
button:hover { background: var(--accent-hover); transform:scale(1.08) rotate(-1deg); }
table { width:100%; border-collapse:collapse; margin-top:15px; font-size:0.95em; }
th,td { border:1px solid var(--border); text-align:center; padding:6px; }
th { background: var(--th-bg); color:var(--accent); }
tr:nth-child(even){ background:rgba(255,255,255,0.4); }
.time-btns, .location-btns{ display:flex; flex-wrap:wrap; gap:6px; margin-top:8px; }
.time-btns button, .location-btns button{ flex:1 1 30%; background:#ffeaf3; color:#333; font-size:0.9em; border-radius:12px; padding:8px 0; cursor:pointer; transition:0.3s; }
.time-btns button.active, .location-btns button.active{ background:var(--accent); color:white; transform:scale(1.05); }
#loadingOverlay{
  display:none;
  position:fixed;
  top:0; left:0;
  width:100%; height:100%;
  background:rgba(0,0,0,0.4);
  color:white;
  font-size:1.5em;
  font-weight:600;
  display:flex;
  align-items:center;
  justify-content:center;
  z-index:9999;
  text-align:center;
  padding:20px;
  border-radius:12px;
  animation: fadein 0.3s ease-in-out;
}
@keyframes fadein { from {opacity:0;} to {opacity:1;} }

/* Toast */
#toast{
  visibility:hidden;
  min-width:200px;
  max-width:300px;
  background:var(--toast-bg);
  color:white;
  text-align:center;
  border-radius:14px;
  padding:12px 20px;
  position:fixed;
  z-index:10000;
  left:50%;
  bottom:30px;
  transform:translateX(-50%);
  font-weight:600;
  box-shadow:0 4px 12px rgba(0,0,0,0.2);
  transition:0.5s ease-in-out;
}
#toast.show{
  visibility:visible;
  animation: slideup 0.5s, fadeout 0.5s 2.5s;
}
@keyframes slideup{ from{bottom:0;opacity:0;} to{bottom:30px;opacity:1;} }
@keyframes fadeout{ from{opacity:1;} to{opacity:0;} }
</style>
</head>
<body>

<h1>🍹 บันทึกยอดขายพาร์ทไทม์ 💕</h1>

<div class="card">
  <label>จำนวนชั่วโมง:</label>
  <input id="hours" type="number" placeholder="เช่น 5" min="0">
  <label>จำนวนแก้ว:</label>
  <input id="cups" type="number" placeholder="เช่น 60" min="0">

  <label>เลือกสถานที่:</label>
  <div class="location-btns">
    <button type="button" onclick="selectLocation(this,'ซอยวุ่นวาย')">ซอยวุ่นวาย</button>
    <button type="button" onclick="selectLocation(this,'หน้ามอ')">หน้ามอ</button>
    <button type="button" onclick="selectLocation(this,'ลาเดอมา')">ลาเดอมา</button>
    <button type="button" onclick="selectLocation(this,'ท่าขอนยาง')">ท่าขอนยาง</button>
    <button type="button" onclick="selectLocation(this,'ขามเรียง')">ขามเรียง</button>
  </div>

  <label>เลือกช่วงเวลา:</label>
  <div class="time-btns">
    <button type="button" onclick="selectTime(this,35)">เต็มวัน (35 บาท/ชม)</button>
    <button type="button" onclick="selectTime(this,30)">พาร์ทไทม์ (30 บาท/ชม)</button>
  </div>

  <div style="text-align:center; margin-top:12px;">
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
        <th>สถานที่</th>
        <th>rate/ชม</th>
        <th>ค่าแรง</th>
        <th>คอมมิชชั่น</th>
        <th>รวม</th>
        <th>ลบ</th>
      </tr>
    </thead>
    <tbody></tbody>
  </table>
  <div id="totalDisplay" style="text-align:center; margin-top:12px; font-weight:600; color:var(--accent)"></div>
</div>

<div id="loadingOverlay">กำลังประมวลผล...</div>
<div id="toast"></div>

<script>
const SCRIPT_URL = 'https://script.google.com/macros/s/AKfycbyrhB-ID8D3bKzOhbkd-eGAJOE8bG_wb75lpoZueGQurqAv5eQ31pC2J7SOR58TbfaOLw/exec';
let selectedLocation='';
let selectedRate=0;

// เลือกสถานที่
function selectLocation(btn, loc){
  document.querySelectorAll('.location-btns button').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
  selectedLocation=loc;
}

// เลือกเวลา
function selectTime(btn, rate){
  document.querySelectorAll('.time-btns button').forEach(b=>b.classList.remove('active'));
  btn.classList.add('active');
  selectedRate=rate;
}

// Toast popup
function showToast(msg){
  const toast=document.getElementById('toast');
  toast.innerText=msg;
  toast.className='show';
  setTimeout(()=>{ toast.className=''; },3000);
}

// Loading overlay
function showLoading(msg='กำลังประมวลผล...'){
  const overlay = document.getElementById('loadingOverlay');
  overlay.innerText = msg;
  overlay.style.display = 'flex';
}
function hideLoading(){
  document.getElementById('loadingOverlay').style.display = 'none';
}

// Save data
async function saveData(){
  const hours = document.getElementById('hours').value.trim();
  const cups = document.getElementById('cups').value.trim();
  if(!selectedLocation){ showToast('⚠️ กรุณาเลือกสถานที่'); return;}
  if(!selectedRate){ showToast('⚠️ กรุณาเลือกช่วงเวลา'); return;}
  if(!hours || !cups){ showToast('⚠️ กรอกข้อมูลให้ครบ'); return;}

  showLoading('💾 กำลังบันทึกข้อมูล...');
  try{
    const res = await fetch(SCRIPT_URL,{
      method:'POST',
      body: JSON.stringify({hours,cups,location:selectedLocation,rate:selectedRate})
    });
    const data = await res.json();
    if(data.status==='success'){
      showToast('✅ บันทึกสำเร็จ');
      document.getElementById('hours').value='';
      document.getElementById('cups').value='';
      selectedRate=0;
      document.querySelectorAll('.time-btns button').forEach(b=>b.classList.remove('active'));
      await loadTable(true);
    }else showToast('❌ '+data.message);
  } finally{
    hideLoading();
  }
}

// Load table
let dataCache=[];
async function loadTable(force=false){
  if(!force && dataCache.length){ renderTable(dataCache); return;}
  showLoading('📊 กำลังโหลดข้อมูล...');
  try{
    const res = await fetch(SCRIPT_URL);
    dataCache = await res.json();
    renderTable(dataCache);
  } catch(e){
    showToast('❌ โหลดข้อมูลล้มเหลว');
  } finally{
    hideLoading();
  }
}

function renderTable(result){
  const tbody = document.querySelector('#dataTable tbody');
  tbody.innerHTML='';
  let total=0;
  result.forEach(r=>{
    total+=Number(r.total||0);
    const row=document.createElement('tr');
    row.innerHTML=`
      <td>${r.date}</td>
      <td>${r.hours}</td>
      <td>${r.cups}</td>
      <td>${r.location}</td>
      <td>${r.rate}</td>
      <td>${r.wage}</td>
      <td>${r.commission}</td>
      <td>${r.total}</td>
      <td><button onclick="confirmDelete('${r.date}','${r.location}')">🗑️</button></td>
    `;
    tbody.appendChild(row);
  });
  document.getElementById('totalDisplay').innerText=`💰 ยอดรวมทั้งหมด: ${total.toLocaleString()} บาท`;
}

// Delete
async function confirmDelete(date,location){
  if(!confirm(`ลบข้อมูลวันที่ ${date} (${location}) ?`)) return;
  showLoading('🗑️ กำลังลบข้อมูล...');
  try{
    const res=await fetch(SCRIPT_URL,{method:'POST',body:JSON.stringify({action:'delete',date,location})});
    const data=await res.json();
    if(data.status==='deleted'){ showToast('🗑️ ลบข้อมูลแล้ว'); loadTable(true); }
  } finally{
    hideLoading();
  }
}

// Total & Slip
function showTotal(){ 
  showLoading('💰 กำลังคำนวณยอดสะสม...');
  setTimeout(()=>{
    const total=dataCache.reduce((sum,r)=>sum+Number(r.total||0),0);
    showToast(`💰 ยอดสะสม: ${total.toLocaleString()} บาท`);
    hideLoading();
  },300);
}

function showSlip(){
  let slipText = `🧾 สลิปเบิกเงิน\nชื่อ: ป๋อมแป๋ม\nวันที่: ${new Date().toLocaleDateString('th-TH')}\n\n`;
  slipText += "รายละเอียด:\n";
  dataCache.forEach(r=>{
    slipText+=`${r.date} | ${r.location} | ${r.hours} ชม | rate ${r.rate} | แก้ว ${r.cups} | รวม ${r.total}\n`;
  });
  const total=dataCache.reduce((sum,r)=>sum+Number(r.total||0),0);
  slipText += `\n💰 ยอดรวมทั้งหมด: ${total.toLocaleString()} บาท\nขอบคุณสำหรับความตั้งใจในการทำงาน 💕`;
  showToast('🧾 สลิปสร้างเสร็จแล้ว'); 
  console.log(slipText); // ส่งไป console แทน alert
}

// Reset all
async function resetAll(){
  if(!confirm('⚠️ ต้องการเริ่มใหม่ทั้งหมดใช่ไหม?')) return;
  showLoading('🧹 กำลังล้างข้อมูลทั้งหมด...');
  try{
    const res=await fetch(SCRIPT_URL,{method:'POST',body:JSON.stringify({action:'resetAll'})});
    const data=await res.json();
    if(data.status==='all_reset'){ showToast('🧹 ล้างข้อมูลแล้ว'); dataCache=[]; loadTable(true); }
  } finally{
    hideLoading();
  }
}

loadTable();
</script>
</body>
</html>
