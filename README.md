<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
  <title>RadarCam | Crowdfunding · Track Any Item with Radar Fusion</title>
  <!-- Tailwind + Font Awesome + Google Fonts -->
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
  <link href="https://fonts.googleapis.com/css2?family=Inter:opsz,wght@14..32,300;14..32,400;14..32,600;14..32,700;14..32,800&display=swap" rel="stylesheet">
  <style>
    * { font-family: 'Inter', sans-serif; }
    /* custom progress bar smoothness */
    .progress-fill {
      transition: width 0.35s cubic-bezier(0.2, 0.9, 0.4, 1.1);
    }
    /* radar canvas container styling */
    .radar-container {
      background: radial-gradient(circle at 30% 20%, #0b1a2e, #030a12);
      border-radius: 1.5rem;
      box-shadow: 0 20px 35px -12px rgba(0,0,0,0.4);
    }
    .radar-canvas {
      width: 100%;
      height: auto;
      background: #05141f;
      border-radius: 1rem;
      display: block;
      margin: 0 auto;
      box-shadow: inset 0 0 18px rgba(0,255,255,0.2), 0 6px 12px rgba(0,0,0,0.3);
    }
    .glow-text {
      text-shadow: 0 0 6px rgba(0,212,255,0.6);
    }
    .pledge-card {
      transition: transform 0.2s ease, box-shadow 0.2s;
    }
    .pledge-card:hover {
      transform: translateY(-4px);
      box-shadow: 0 20px 30px -12px rgba(0,0,0,0.25);
    }
    button:active {
      transform: scale(0.97);
    }
    @keyframes pulse-ring {
      0% { box-shadow: 0 0 0 0 rgba(0, 255, 200, 0.5); }
      80% { box-shadow: 0 0 0 12px rgba(0, 255, 200, 0); }
      100% { box-shadow: 0 0 0 0 rgba(0, 255, 200, 0); }
    }
    .badge-radar {
      animation: pulse-ring 1.8s infinite;
    }
  </style>
</head>
<body class="bg-gradient-to-br from-slate-900 via-slate-800 to-gray-900 text-white">

  <!-- HEADER / NAVBAR -->
  <nav class="backdrop-blur-md bg-black/40 border-b border-white/10 sticky top-0 z-20">
    <div class="max-w-7xl mx-auto px-5 py-4 flex justify-between items-center flex-wrap gap-3">
      <div class="flex items-center gap-2">
        <i class="fas fa-satellite-dish text-cyan-400 text-3xl"></i>
        <span class="font-black text-2xl tracking-tight bg-gradient-to-r from-cyan-300 to-blue-400 bg-clip-text text-transparent">Radar<span class="text-white">Cam</span></span>
        <span class="ml-2 text-xs bg-cyan-900/60 text-cyan-200 px-2 py-1 rounded-full border border-cyan-500/40">Fusion Tracking™</span>
      </div>
      <div class="flex gap-4 text-sm font-medium">
        <a href="#campaign" class="hover:text-cyan-300 transition">Campaign</a>
        <a href="#radar-demo" class="hover:text-cyan-300 transition">Live Radar</a>
        <a href="#rewards" class="hover:text-cyan-300 transition">Rewards</a>
      </div>
    </div>
  </nav>

  <main class="max-w-7xl mx-auto px-5 py-8 md:py-12">
    <!-- Hero + grid section -->
    <div class="flex flex-wrap lg:flex-nowrap gap-8 mb-16" id="campaign">
      <!-- LEFT COLUMN : CAMPAIGN DETAILS & FUNDING -->
      <div class="w-full lg:w-1/2 space-y-6">
        <div class="space-y-3">
          <span class="inline-flex items-center gap-1 bg-cyan-950/60 text-cyan-300 text-xs font-semibold px-3 py-1 rounded-full border border-cyan-500/50"><i class="fas fa-radar text-xs"></i> Crowdfunding · Live now</span>
          <h1 class="text-4xl md:text-5xl font-extrabold leading-tight bg-gradient-to-r from-white via-cyan-100 to-blue-300 bg-clip-text text-transparent">RadarCam <br> Never lose anything again.</h1>
          <p class="text-gray-300 text-lg">Revolutionary <span class="font-bold text-cyan-300">radar + optical camera</span> fusion that tracks keys, wallet, pets, luggage & valuables with <span class="font-semibold">centimeter precision</span>. See through walls? no — but tracks items behind obstacles via mmWave radar.</p>
          <div class="flex flex-wrap gap-3 pt-2">
            <div class="flex items-center gap-1 bg-white/5 rounded-full px-3 py-1"><i class="fas fa-microchip text-cyan-400"></i><span class="text-sm">60GHz Radar</span></div>
            <div class="flex items-center gap-1 bg-white/5 rounded-full px-3 py-1"><i class="fas fa-video text-cyan-400"></i><span class="text-sm">4K Night Vision</span></div>
            <div class="flex items-center gap-1 bg-white/5 rounded-full px-3 py-1"><i class="fas fa-battery-full text-cyan-400"></i><span class="text-sm">14-day battery</span></div>
          </div>
        </div>

        <!-- FUNDING PROGRESS CARD -->
        <div class="bg-slate-800/70 backdrop-blur-sm rounded-2xl border border-slate-700 p-6 shadow-xl">
          <div class="flex justify-between items-end flex-wrap gap-2">
            <div>
              <p class="text-gray-400 text-sm uppercase tracking-wide">Total raised</p>
              <p class="text-4xl font-bold"><span id="raisedAmount">0</span> <span class="text-xl font-normal text-gray-300">USD</span></p>
            </div>
            <div class="text-right">
              <p class="text-gray-400 text-sm">Goal</p>
              <p class="text-2xl font-semibold">$100,000 USD</p>
            </div>
          </div>
          <div class="mt-4 relative">
            <div class="w-full h-4 bg-gray-700 rounded-full overflow-hidden">
              <div id="progressFill" class="progress-fill h-full bg-gradient-to-r from-cyan-500 to-blue-500 rounded-full" style="width: 0%;"></div>
            </div>
            <div class="flex justify-between mt-2 text-sm">
              <span id="percentFunded" class="text-cyan-300 font-bold">0%</span>
              <span class="text-gray-400"><i class="fas fa-users mr-1"></i> <span id="backerCount">0</span> backers</span>
            </div>
          </div>
          <div class="mt-5 flex gap-3 text-sm">
            <div class="bg-slate-900/70 rounded-lg px-4 py-2 flex-1 text-center"><i class="fas fa-map-marker-alt text-cyan-400"></i> <span class="block text-lg font-bold">50m</span><span class="text-gray-400">tracking range</span></div>
            <div class="bg-slate-900/70 rounded-lg px-4 py-2 flex-1 text-center"><i class="fas fa-weight-hanging text-cyan-400"></i> <span class="block text-lg font-bold">20+ items</span><span class="text-gray-400">simultaneously</span></div>
          </div>
        </div>

        <!-- REWARD TIERS (section) -->
        <div id="rewards" class="space-y-4 pt-4">
          <h2 class="text-2xl font-bold flex items-center gap-2"><i class="fas fa-gift text-cyan-400"></i> Pledge · Choose your reward</h2>
          <div class="grid gap-4">
            <!-- Early Bird -->
            <div class="pledge-card bg-gradient-to-r from-slate-800 to-slate-800/70 rounded-xl border border-cyan-800/40 p-5 hover:border-cyan-500/60 transition">
              <div class="flex justify-between items-start flex-wrap">
                <div><h3 class="text-xl font-bold">🔭 Early Bird RadarCam</h3><p class="text-gray-300 text-sm">One RadarCam unit + magnetic mount + USB-C cable.</p></div>
                <span class="text-2xl font-black text-cyan-300">$49</span>
              </div>
              <button data-amount="49" class="pledge-btn mt-4 w-full bg-cyan-600 hover:bg-cyan-500 transition font-semibold py-2 rounded-lg flex items-center justify-center gap-2"><i class="fas fa-hand-holding-heart"></i> Pledge $49</button>
            </div>
            <!-- Standard Pack -->
            <div class="pledge-card bg-gradient-to-r from-slate-800 to-slate-800/70 rounded-xl border border-blue-800/40 p-5 hover:border-blue-500/60 transition">
              <div class="flex justify-between items-start flex-wrap">
                <div><h3 class="text-xl font-bold">⚡ RadarCam Pro Bundle</h3><p class="text-gray-300 text-sm">RadarCam + Wall mount + Protective case + Sticker pack.</p></div>
                <span class="text-2xl font-black text-cyan-300">$79</span>
              </div>
              <button data-amount="79" class="pledge-btn mt-4 w-full bg-cyan-600 hover:bg-cyan-500 transition font-semibold py-2 rounded-lg flex items-center justify-center gap-2"><i class="fas fa-hand-holding-heart"></i> Pledge $79</button>
            </div>
            <!-- Family pack -->
            <div class="pledge-card bg-gradient-to-r from-slate-800 to-slate-800/70 rounded-xl border border-purple-800/40 p-5 hover:border-purple-500/60 transition">
              <div class="flex justify-between items-start flex-wrap">
                <div><h3 class="text-xl font-bold">👨‍👩‍👧‍👦 Family Guardian (3 units)</h3><p class="text-gray-300 text-sm">Three RadarCams – ideal for home, car, office. Multi-item dashboard.</p></div>
                <span class="text-2xl font-black text-cyan-300">$199</span>
              </div>
              <button data-amount="199" class="pledge-btn mt-4 w-full bg-cyan-600 hover:bg-cyan-500 transition font-semibold py-2 rounded-lg flex items-center justify-center gap-2"><i class="fas fa-hand-holding-heart"></i> Pledge $199</button>
            </div>
          </div>
          <!-- Custom pledge -->
          <div class="bg-slate-800/50 rounded-xl p-5 border border-slate-700">
            <label class="block font-semibold mb-2">✨ Custom amount (any support)</label>
            <div class="flex flex-wrap gap-3 items-center">
              <div class="relative flex-grow">
                <span class="absolute left-3 top-1/2 -translate-y-1/2 text-gray-400">$</span>
                <input type="number" id="customAmountInput" placeholder="10" min="1" step="1" class="w-full pl-7 pr-3 py-2 bg-slate-900 rounded-lg border border-slate-600 focus:border-cyan-400 focus:ring-1 focus:ring-cyan-400 outline-none text-white">
              </div>
              <button id="customPledgeBtn" class="bg-slate-700 hover:bg-cyan-700 transition px-6 py-2 rounded-lg font-semibold"><i class="fas fa-hand-peace mr-1"></i> Pledge</button>
            </div>
            <p class="text-xs text-gray-400 mt-2">Any amount helps us mass-produce the radar technology. You get our eternal gratitude + updates.</p>
          </div>
        </div>
        <div class="text-xs text-gray-400 border-t border-slate-700/50 pt-4 mt-2"><i class="fas fa-shield-alt"></i> Demo simulation · No real transaction. Every pledge increases total raised & backer count to showcase community support.</div>
      </div>

      <!-- RIGHT COLUMN : RADAR CAMERA LIVE TRACKING DEMO (effectively tracking items) -->
      <div class="w-full lg:w-1/2" id="radar-demo">
        <div class="radar-container p-4 md:p-5">
          <div class="flex justify-between items-center mb-2">
            <h2 class="text-xl font-bold flex items-center gap-2"><i class="fas fa-satellite-dish text-cyan-400"></i> Live Radar View<small class="text-xs text-cyan-300 ml-2"> real-time item tracking</small></h2>
            <span class="badge-radar inline-flex items-center gap-1 text-cyan-300 text-xs bg-cyan-950/50 px-2 py-1 rounded-full"><i class="fas fa-circle text-[8px]"></i> scanning active</span>
          </div>
          <div class="flex justify-center">
            <canvas id="radarCanvas" width="400" height="400" class="radar-canvas w-full max-w-[400px] aspect-square"></canvas>
          </div>
          <!-- track list of items update -->
          <div class="mt-4 grid grid-cols-2 gap-2 text-sm bg-black/30 rounded-xl p-3">
            <div class="font-semibold flex items-center gap-2"><i class="fas fa-location-dot text-cyan-400"></i> Tracked objects:</div>
            <div class="text-right text-cyan-300 text-xs">updated live</div>
            <div id="trackedItemsList" class="col-span-2 space-y-1 text-gray-200 text-xs md:text-sm font-mono"></div>
          </div>
          <p class="text-[11px] text-gray-400 mt-3 text-center"><i class="fas fa-waveform"></i> mmWave Radar + AI fusion: distance & angle precision. Items move in real demo to simulate tracking efficiency.</p>
        </div>
        <div class="mt-5 bg-slate-800/40 rounded-2xl p-4 text-center border border-slate-700/60">
          <i class="fas fa-check-circle text-green-400 text-xl mr-1"></i> 
          <span class="font-medium">Effectively tracks keys, wallet, AirTag-like items, pets — even behind sofa cushions.</span>
        </div>
      </div>
    </div>

    <!-- tech & innovation section -->
    <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mt-10 pt-6 border-t border-slate-700/40">
      <div class="flex flex-col items-center text-center gap-2"><i class="fas fa-chart-line text-3xl text-cyan-400"></i><h3 class="font-bold">Radar Imaging</h3><p class="text-sm text-gray-300">60GHz radar generates 3D point cloud; tracks items with sub-10cm error.</p></div>
      <div class="flex flex-col items-center text-center gap-2"><i class="fas fa-robot text-3xl text-cyan-400"></i><h3 class="font-bold">AI Object Recognition</h3><p class="text-sm text-gray-300">Camera identifies "keys", "remote", "wallet" and fuses with radar location.</p></div>
      <div class="flex flex-col items-center text-center gap-2"><i class="fas fa-cloud-upload-alt text-3xl text-cyan-400"></i><h3 class="font-bold">Cloud Dashboard</h3><p class="text-sm text-gray-300">Find items via mobile app — last known positions & geofence alerts.</p></div>
    </div>

    <!-- footer -->
    <footer class="mt-16 text-center text-gray-500 text-sm border-t border-slate-800/60 pt-8">
      <p>© 2025 RadarCam · Crowdfunding concept demo. Radar tracking simulation illustrates item position tracking effectively.</p>
    </footer>
  </main>

  <script>
    // ---------- CROWDFUNDING GLOBAL STATE ----------
    const GOAL = 100000;
    let currentRaised = 52750;   // initial raised: 52.75% funded
    let backers = 832;            // backer count
    
    // DOM elements
    const raisedSpan = document.getElementById('raisedAmount');
    const backerSpan = document.getElementById('backerCount');
    const progressFillDiv = document.getElementById('progressFill');
    const percentSpan = document.getElementById('percentFunded');
    
    function formatNumber(num) {
      return num.toLocaleString('en-US');
    }
    
    function updateFundingUI() {
      raisedSpan.innerText = formatNumber(currentRaised);
      backerSpan.innerText = formatNumber(backers);
      let percent = (currentRaised / GOAL) * 100;
      percent = Math.min(percent, 200); // cap width representation (max visual)
      const cappedWidth = Math.min(percent, 100);
      progressFillDiv.style.width = `${cappedWidth}%`;
      percentSpan.innerText = `${Math.floor(percent)}%`;
      // extra effect: if funded over 100% show special message (optional)
      if (currentRaised >= GOAL) {
        progressFillDiv.classList.add('bg-gradient-to-r', 'from-yellow-400', 'to-orange-400');
        if(!document.getElementById('goalMsg')) {
          const msgDiv = document.createElement('div');
          msgDiv.id = 'goalMsg';
          msgDiv.className = 'text-green-300 text-sm mt-2 font-semibold flex items-center gap-1';
          msgDiv.innerHTML = '<i class="fas fa-trophy"></i> GOAL REACHED! Stretch goals unlocked: extended battery upgrade!';
          document.querySelector('#campaign .bg-slate-800\\/70')?.appendChild(msgDiv);
        }
      } else {
        const existing = document.getElementById('goalMsg');
        if(existing) existing.remove();
        progressFillDiv.classList.remove('bg-gradient-to-r', 'from-yellow-400', 'to-orange-400');
      }
    }
    
    // PLEDGE function (simulate)
    function addPledge(amount) {
      if (isNaN(amount) || amount <= 0) {
        alert("Please enter a valid pledge amount (≥ $1).");
        return false;
      }
      currentRaised += amount;
      backers += 1;
      updateFundingUI();
      // success visual message (toast-like)
      showToast(`🎉 Thank you for pledging $${amount}! You’re now part of the RadarCam community.`);
      return true;
    }
    
    // Simple toast notification
    function showToast(msg) {
      let toast = document.createElement('div');
      toast.className = 'fixed bottom-5 left-1/2 transform -translate-x-1/2 bg-gray-900 border border-cyan-500 text-white px-5 py-3 rounded-xl shadow-2xl z-50 flex items-center gap-2 backdrop-blur-md text-sm font-medium animate-bounce';
      toast.innerHTML = `<i class="fas fa-check-circle text-cyan-400"></i> ${msg}`;
      document.body.appendChild(toast);
      setTimeout(() => { toast.style.opacity = '0'; setTimeout(() => toast.remove(), 400); }, 2800);
    }
    
    // event binding for pledge buttons (reward tiers)
    document.querySelectorAll('.pledge-btn').forEach(btn => {
      btn.addEventListener('click', (e) => {
        const amount = parseInt(btn.getAttribute('data-amount'));
        if (amount && amount > 0) {
          addPledge(amount);
        }
      });
    });
    
    // custom pledge button
    const customBtn = document.getElementById('customPledgeBtn');
    const customInput = document.getElementById('customAmountInput');
    customBtn.addEventListener('click', () => {
      let val = parseFloat(customInput.value);
      if (isNaN(val) || val < 1) {
        alert("Please enter an amount at least $1");
        return;
      }
      val = Math.floor(val); // clean integer
      addPledge(val);
      customInput.value = '';
    });
    
    // initial set UI
    updateFundingUI();
    
    // ------------------- RADAR SIMULATION: effective item tracking demo -------------------
    const canvas = document.getElementById('radarCanvas');
    const ctx = canvas.getContext('2d');
    let width = 400, height = 400;
    canvas.width = width; canvas.height = height;
    
    // tracked items (radar targets) with names, polar coordinates (angle rad, distance px from center)
    // center at (200,200), max radius 170px (safe from canvas border)
    let items = [
      { name: "🔑 Keys", angle: 0.8, distance: 85, color: "#FFD966" },
      { name: "👛 Wallet", angle: 4.2, distance: 120, color: "#A5D6FF" },
      { name: "📱 Phone", angle: 2.9, distance: 60, color: "#C0E0FF" },
      { name: "🎮 Remote", angle: 5.6, distance: 145, color: "#FFB347" },
      { name: "🐕 Pet tag", angle: 3.4, distance: 40, color: "#FFA07A" }
    ];
    
    // random move each item gently to simulate real-time tracking (effective location changes)
    function updateItemsMovement() {
      for (let item of items) {
        // slightly change angle (±0.05 rad) and distance (±3 to 7 px)
        let deltaAngle = (Math.random() - 0.5) * 0.12;
        let deltaDist = (Math.random() - 0.5) * 9;
        item.angle += deltaAngle;
        item.distance += deltaDist;
        // boundaries: angle loop 0-2PI
        if (item.angle < 0) item.angle += Math.PI * 2;
        if (item.angle >= Math.PI * 2) item.angle -= Math.PI * 2;
        // keep distance inside 30–165 px (visible)
        item.distance = Math.min(165, Math.max(30, item.distance));
      }
    }
    
    let sweepAngle = 0;
    let animationId = null;
    
    function drawRadar() {
      if (!ctx) return;
      ctx.clearRect(0, 0, width, height);
      
      // background & radar rings
      ctx.save();
      ctx.shadowBlur = 0;
      // rings
      const centerX = width/2, centerY = height/2;
      const maxRadius = 170;
      ctx.strokeStyle = "rgba(0, 210, 255, 0.5)";
      ctx.lineWidth = 1.2;
      for (let r = 1; r <= 3; r++) {
        ctx.beginPath();
        ctx.arc(centerX, centerY, maxRadius * (r/3), 0, Math.PI*2);
        ctx.stroke();
      }
      // crosshair lines
      ctx.beginPath();
      ctx.moveTo(centerX - maxRadius, centerY);
      ctx.lineTo(centerX + maxRadius, centerY);
      ctx.moveTo(centerX, centerY - maxRadius);
      ctx.lineTo(centerX, centerY + maxRadius);
      ctx.stroke();
      // subtle grid circles
      ctx.beginPath();
      ctx.arc(centerX, centerY, maxRadius*0.66, 0, Math.PI*2);
      ctx.strokeStyle = "rgba(0, 180, 220, 0.3)";
      ctx.stroke();
      
      // draw each item (blip)
      for (let item of items) {
        let x = centerX + item.distance * Math.cos(item.angle);
        let y = centerY + item.distance * Math.sin(item.angle);
        // shadow glow
        ctx.beginPath();
        ctx.arc(x, y, 8, 0, 2 * Math.PI);
        ctx.fillStyle = item.color + "66";
        ctx.fill();
        ctx.beginPath();
        ctx.arc(x, y, 5, 0, 2 * Math.PI);
        ctx.fillStyle = item.color;
        ctx.fill();
        ctx.beginPath();
        ctx.arc(x, y, 2, 0, 2 * Math.PI);
        ctx.fillStyle = "white";
        ctx.fill();
        // small label nearby (optional)
        ctx.font = "bold 10px 'Inter'";
        ctx.fillStyle = "#E0F2FE";
        ctx.shadowBlur = 2;
        ctx.fillText(item.name, x + 6, y - 4);
      }
      
      // draw scanning sweep line (camera/radar rotation)
      let sweepX = centerX + maxRadius * Math.cos(sweepAngle);
      let sweepY = centerY + maxRadius * Math.sin(sweepAngle);
      ctx.beginPath();
      ctx.moveTo(centerX, centerY);
      ctx.lineTo(sweepX, sweepY);
      ctx.strokeStyle = "#00FFFF";
      ctx.lineWidth = 2.2;
      ctx.shadowBlur = 6;
      ctx.stroke();
      // semi-transparent sweep gradient
      let gradient = ctx.createLinearGradient(centerX, centerY, sweepX, sweepY);
      gradient.addColorStop(0, "rgba(0,255,200,0.2)");
      gradient.addColorStop(1, "rgba(0,255,200,0)");
      ctx.beginPath();
      ctx.moveTo(centerX, centerY);
      ctx.lineTo(sweepX, sweepY);
      ctx.lineTo(sweepX + (sweepX-centerX)*0.2, sweepY + (sweepY-centerY)*0.2);
      ctx.fillStyle = gradient;
      ctx.fill();
      
      // small center indicator
      ctx.beginPath();
      ctx.arc(centerX, centerY, 5, 0, 2*Math.PI);
      ctx.fillStyle = "#0ff";
      ctx.fill();
      ctx.restore();
      
      // update tracking textual info panel (items positions approximate)
      updateTrackedListPanel();
    }
    
    function updateTrackedListPanel() {
      const container = document.getElementById('trackedItemsList');
      if (!container) return;
      let html = '';
      for (let item of items) {
        // convert distance (px) to approximate real meters: max 170px = 50m actual range
        let meters = ((item.distance / 170) * 48).toFixed(1);
        let degrees = ((item.angle * 180 / Math.PI) % 360).toFixed(0);
        html += `<div class="flex justify-between text-xs border-b border-cyan-900/40 pb-1"><span><i class="fas fa-circle" style="color:${item.color}; font-size:8px;"></i> ${item.name}</span><span class="text-cyan-300">${meters}m  @ ${degrees}°</span></div>`;
      }
      container.innerHTML = html || '<div class="text-gray-400">Tracking...</div>';
    }
    
    // animate sweep angle and redraw radar
    let lastMoveUpdate = 0;
    function animateRadar(timestamp) {
      sweepAngle += 0.02;
      if (sweepAngle > Math.PI * 2) sweepAngle -= Math.PI * 2;
      
      // update items movement every 0.7s (simulate real-time location changes)
      if (!lastMoveUpdate || timestamp - lastMoveUpdate > 700) {
        updateItemsMovement();
        lastMoveUpdate = timestamp;
      }
      
      drawRadar();
      requestAnimationFrame(animateRadar);
    }
    
    // Init radar + start animation
    requestAnimationFrame(animateRadar);
    
    // to make canvas responsive, update if needed, but fixed dimensions work fine
    
    // Add small tooltip: effective tracking demonstration interactively
    const radarBox = document.querySelector('.radar-container');
    if(radarBox) {
      radarBox.addEventListener('mouseenter', () => {
        // just for fun, slight pulse style
      });
    }
    
    // add extra initial text: items are moving in realtime to simulate radar tracking capability
    console.log("Radar active - live item tracking simulation");
    
    // Additional note: provide manual initial demonstration
    setInterval(() => {
      // ensure item list textual updates happen smoothly
      updateTrackedListPanel();
    }, 500);
  </script>
</body>
</html>
