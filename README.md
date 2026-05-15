<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Neon Medical HUD - Blood Donation</title>

<script src="https://cdn.tailwindcss.com"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>

<style>
body {
  background: radial-gradient(circle at top, #0a0a0a, #000);
  color: white;
  overflow-x: hidden;
}

/* GRID HUD */
body::before {
  content: "";
  position: fixed;
  inset: 0;
  background-image:
    linear-gradient(rgba(0,255,150,0.05) 1px, transparent 1px),
    linear-gradient(90deg, rgba(0,255,150,0.05) 1px, transparent 1px);
  background-size: 40px 40px;
  pointer-events: none;
}

/* SCANLINE */
body::after {
  content: "";
  position: fixed;
  inset: 0;
  background: repeating-linear-gradient(
    to bottom,
    rgba(255,255,255,0.03),
    rgba(255,255,255,0.03) 1px,
    transparent 2px,
    transparent 4px
  );
  pointer-events: none;
  animation: scan 6s linear infinite;
}

@keyframes scan {
  0% { transform: translateY(-100%); }
  100% { transform: translateY(100%); }
}

/* NEON TEXT */
.neon {
  color: #00ff99;
  text-shadow: 0 0 5px #00ff99, 0 0 20px #00ff99;
}

/* GLITCH TITLE */
.glitch {
  position: relative;
  font-weight: 800;
}

.glitch::before,
.glitch::after {
  content: "BLOOD DONATION SYSTEM";
  position: absolute;
  left: 0;
  width: 100%;
  overflow: hidden;
}

.glitch::before {
  color: red;
  animation: glitch 1s infinite linear alternate-reverse;
}

.glitch::after {
  color: cyan;
  animation: glitch 1.3s infinite linear alternate-reverse;
}

@keyframes glitch {
  0% { transform: translate(0); }
  20% { transform: translate(-2px,2px); }
  40% { transform: translate(2px,-2px); }
  60% { transform: translate(-1px,1px); }
  100% { transform: translate(0); }
}

/* ECG LINE */
.ecg {
  height: 2px;
  background: #00ff99;
  box-shadow: 0 0 15px #00ff99;
  animation: pulse 1.2s infinite linear;
}

@keyframes pulse {
  0% { transform: scaleX(1); opacity: 0.3; }
  50% { transform: scaleX(1.05); opacity: 1; }
  100% { transform: scaleX(1); opacity: 0.3; }
}

/* FLOAT BUTTON */
.floating {
  position: fixed;
  bottom: 20px;
  right: 20px;
  background: #00ff99;
  color: black;
  font-weight: bold;
  padding: 14px 18px;
  border-radius: 999px;
  box-shadow: 0 0 25px #00ff99;
  cursor: pointer;
  animation: float 2s infinite;
}

@keyframes float {
  0%,100% { transform: translateY(0); }
  50% { transform: translateY(-8px); }
}

/* FADE IN */
.fade {
  opacity: 0;
  transform: translateY(40px);
  transition: 1s ease;
}

.fade.show {
  opacity: 1;
  transform: translateY(0);
}

/* DATA CARD */
.card {
  background: rgba(0,0,0,0.7);
  border: 1px solid rgba(0,255,150,0.3);
  backdrop-filter: blur(10px);
  box-shadow: 0 0 20px rgba(0,255,150,0.1);
}
</style>
</head>

<body>

<!-- FLOAT BUTTON -->
<div class="floating" onclick="document.getElementById('register').scrollIntoView({behavior:'smooth'})">
  ⚡ DONATE
</div>

<!-- HERO -->
<section class="text-center py-24 px-6">

  <h1 class="text-4xl md:text-6xl glitch neon">
    BLOOD DONATION SYSTEM
  </h1>

  <div class="ecg my-8"></div>

  <p class="text-gray-300 max-w-2xl mx-auto">
    NEURAL MEDICAL INTERFACE ACTIVE — DONATION NETWORK ONLINE
  </p>
</section>

<!-- STATS -->
<section class="grid md:grid-cols-3 gap-6 max-w-6xl mx-auto px-6">

  <div class="card p-6 rounded-2xl text-center fade">
    <h2 class="text-3xl neon" id="donors">0</h2>
    <p class="text-gray-400">Active Donors</p>
  </div>

  <div class="card p-6 rounded-2xl text-center fade">
    <h2 class="text-3xl neon" id="lives">0</h2>
    <p class="text-gray-400">Lives Impacted</p>
  </div>

  <div class="card p-6 rounded-2xl text-center fade">
    <h2 class="text-3xl neon" id="sessions">0</h2>
    <p class="text-gray-400">Active Sessions</p>
  </div>

</section>

<!-- INFO -->
<section class="max-w-5xl mx-auto px-6 py-20 space-y-10">

  <div class="card p-8 rounded-2xl fade">
    <h2 class="text-2xl neon mb-4">SYSTEM ELIGIBILITY CHECK</h2>
    <ul class="text-gray-300 space-y-2">
      <li>✔ AGE: 16–65</li>
      <li>✔ WEIGHT: ≥50KG</li>
      <li>✔ HEALTH STATUS: STABLE</li>
    </ul>
  </div>

  <div class="card p-8 rounded-2xl fade">
    <h2 class="text-2xl neon mb-4">MISSION TARGETS</h2>
    <p class="text-gray-400">
      Mothers in labor • Surgical emergencies • Pediatric transfusions
    </p>
  </div>

</section>

<!-- QR -->
<section id="register" class="text-center py-20 fade">

  <h2 class="text-3xl neon mb-6">SCAN TO REGISTER</h2>

  <div id="qrcode" class="flex justify-center"></div>

</section>

<!-- SCRIPT -->
<script>

// scroll animation
const f = document.querySelectorAll('.fade');
const observer = new IntersectionObserver(e=>{
  e.forEach(i=>{
    if(i.isIntersecting) i.target.classList.add('show');
  });
});
f.forEach(x=>observer.observe(x));

// QR
new QRCode(document.getElementById("qrcode"), {
  text: "https://forms.gle/example",
  width: 180,
  height: 180,
  colorDark: "#00ff99",
  colorLight: "#000"
});

// animated counters
function count(id, end, speed){
  let el = document.getElementById(id);
  let i = 0;
  let interval = setInterval(()=>{
    i++;
    el.innerText = i;
    if(i >= end) clearInterval(interval);
  }, speed);
}

count("donors", 120, 10);
count("lives", 340, 8);
count("sessions", 55, 30);

</script>

</body>
</html>
