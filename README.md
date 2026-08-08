<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Pengingat & Target Tabungan</title>
  <style>
    * { box-sizing: border-box; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
    body { background-color: #f4f7fe; display: flex; justify-content: center; padding: 20px; }
    .card { background: #ffffff; padding: 25px; border-radius: 12px; box-shadow: 0 4px 15px rgba(0,0,0,0.1); width: 100%; max-width: 450px; }
    h2 { color: #2b3674; margin-bottom: 20px; text-align: center; }
    .form-group { margin-bottom: 15px; }
    label { display: block; font-weight: 600; color: #a3edda; color: #4318ff; margin-bottom: 5px; font-size: 14px; }
    input { width: 100%; padding: 10px; border: 1px solid #e0e5f2; border-radius: 8px; outline: none; }
    button { width: 100%; padding: 12px; background: #4318ff; color: white; border: none; border-radius: 8px; font-weight: bold; cursor: pointer; margin-top: 10px; }
    button:hover { background: #3311db; }
    .result { margin-top: 25px; border-top: 2px dashed #e0e5f2; padding-top: 15px; }
    .progress-bar { width: 100%; background: #eff4fb; border-radius: 10px; height: 15px; overflow: hidden; margin: 10px 0; }
    .progress { width: 0%; height: 100%; background: #05cd99; transition: width 0.3s; }
    .info-box { background: #f4f7fe; padding: 10px; border-radius: 8px; margin-top: 10px; font-size: 14px; color: #2b3674; }
  </style>
</head>
<body>

<div class="card">
  <h2>🎯 Target Tabungan</h2>
  
  <div class="form-group">
    <label for="goalName">Nama Target</label>
    <input type="text" id="goalName" placeholder="Contoh: Beli Laptop Baru">
  </div>
  
  <div class="form-group">
    <label for="targetAmount">Target Jumlah (Rp)</label>
    <input type="number" id="targetAmount" placeholder="10000000">
  </div>

  <div class="form-group">
    <label for="currentAmount">Uang Saat Ini (Rp)</label>
    <input type="number" id="currentAmount" placeholder="0">
  </div>

  <div class="form-group">
    <label for="days">Target Waktu (Hari)</label>
    <input type="number" id="days" placeholder="30">
  </div>

  <button onclick="calculateGoal()">Hitung Target & Aktifkan Pengingat</button>

  <div class="result" id="resultBox" style="display: none;">
    <h3 id="resGoalName" style="color: #2b3674; margin: 0;"></h3>
    <div class="progress-bar">
      <div class="progress" id="progressBar"></div>
    </div>
    <p id="progressText" style="font-size: 12px; color: #a3edda; color: #707ebe; text-align: right; margin: 0;"></p>
    
    <div class="info-box">
      <p>💡 **Rekomendasi Tabungan:**</p>
      <ul>
        <li>Harian: <b id="dailySavings">Rp 0</b></li>
        <li>Mingguan: <b id="weeklySavings">Rp 0</b></li>
      </ul>
    </div>
  </div>
</div>

<script>
  function calculateGoal() {
    const name = document.getElementById('goalName').value;
    const target = parseFloat(document.getElementById('targetAmount').value) || 0;
    const current = parseFloat(document.getElementById('currentAmount').value) || 0;
    const days = parseInt(document.getElementById('days').value) || 1;

    if (!name || target <= 0 || days <= 0) {
      alert("Harap isi semua data dengan benar!");
      return;
    }

    const remaining = Math.max(0, target - current);
    const daily = Math.ceil(remaining / days);
    const weekly = Math.ceil(remaining / (days / 7));

    const percent = Math.min(100, Math.round((current / target) * 100));

    // Update UI
    document.getElementById('resultBox').style.display = 'block';
    document.getElementById('resGoalName').innerText = name;
    document.getElementById('progressBar').style.width = percent + '%';
    document.getElementById('progressText').innerText = `Terkumpul: ${percent}% (Rp ${current.toLocaleString('id-ID')} / Rp ${target.toLocaleString('id-ID')})`;
    
    document.getElementById('dailySavings').innerText = `Rp ${daily.toLocaleString('id-ID')} / hari`;
    document.getElementById('weeklySavings').innerText = `Rp ${weekly.toLocaleString('id-ID')} / minggu`;

    // Minta Izin Notifikasi Browser
    requestNotificationPermission();
  }

  function requestNotificationPermission() {
    if ("Notification" in window) {
      Notification.requestPermission().then(permission => {
        if (permission === "granted") {
          new Notification("Pengingat Tabungan Aktif! 🔔", {
            body: "Jangan lupa sisihkan tabunganmu hari ini ya!",
          });
        }
      });
    }
  }
</script>

</body>
</html>
