<!DOCTYPE html>
<html>
<head>
  <title>Live Machine Dashboard</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #0f172a;
      color: white;
      margin: 0;
      padding: 0;
    }

    header {
      background: #020617;
      padding: 15px;
      text-align: center;
      font-size: 22px;
      font-weight: bold;
      border-bottom: 1px solid #1e293b;
    }

    .container {
      padding: 20px;
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
      gap: 20px;
    }

    .card {
      background: #1e293b;
      padding: 20px;
      border-radius: 12px;
      box-shadow: 0 4px 20px rgba(0,0,0,0.3);
      text-align: center;
      transition: 0.3s;
    }

    .card:hover {
      transform: scale(1.05);
    }

    .value {
      font-size: 32px;
      margin-top: 10px;
      font-weight: bold;
    }

    .label {
      color: #94a3b8;
    }

    .status {
      font-size: 20px;
      padding: 10px;
      border-radius: 8px;
      margin-top: 10px;
      display: inline-block;
    }

    .running { background: #16a34a; }
    .idle { background: #facc15; color: black; }
    .fault { background: #dc2626; }

    .back-btn {
      margin: 20px;
      padding: 10px 20px;
      background: #2563eb;
      border: none;
      border-radius: 8px;
      color: white;
      cursor: pointer;
    }

    .back-btn:hover {
      background: #1d4ed8;
    }
  </style>
</head>

<body>

<header id="title">Machine Dashboard</header>

<div class="container">
  <div class="card">
    <div class="label">Temperature</div>
    <div class="value" id="temp">--</div>
  </div>

  <div class="card">
    <div class="label">RPM</div>
    <div class="value" id="rpm">--</div>
  </div>

  <div class="card">
    <div class="label">Voltage</div>
    <div class="value" id="voltage">--</div>
  </div>

  <div class="card">
    <div class="label">Status</div>
    <div id="status" class="status">--</div>
  </div>
</div>

<center>
  <button class="back-btn" onclick="goBack()">⬅ Back</button>
</center>

<!-- Firebase -->
<script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-app.js"></script>
<script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-database.js"></script>

<script>
  // Get Machine ID
  const params = new URLSearchParams(window.location.search);
  const machineId = params.get("id") || "Machine1";

  document.getElementById("title").innerText = machineId + " Dashboard";

  // Firebase Config
  var firebaseConfig = {
    apiKey: "AIzaSyAtUjNOxehowo3hBGQZ2Ln3-XgG7-LFTFo",
    databaseURL: "https://qr-machine-project-default-rtdb.asia-southeast1.firebasedatabase.app"
  };

  firebase.initializeApp(firebaseConfig);

  var db = firebase.database().ref(machineId);

  db.on("value", function(snapshot) {
    var data = snapshot.val();

    if (data) {
      document.getElementById("temp").innerText = data.temp + " °C";
      document.getElementById("rpm").innerText = data.rpm;
      document.getElementById("voltage").innerText = data.voltage + " V";

      let statusEl = document.getElementById("status");
      statusEl.innerText = data.status;

      // Dynamic color
      statusEl.className = "status";
      if (data.status.toLowerCase() === "running") {
        statusEl.classList.add("running");
      } else if (data.status.toLowerCase() === "idle") {
        statusEl.classList.add("idle");
      } else {
        statusEl.classList.add("fault");
      }
    }
  });

  function goBack() {
    window.location.href = "index.html";
  }
</script>

</body>
</html>
