<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Blood Donation Drive - Futuristic UI</title>

<script src="https://cdn.tailwindcss.com"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>

<style>
body {
  background: black;
  color: white;
  overflow-x: hidden;
}

/* GRID BACKGROUND */
body::before {
  content: "";
  position: fixed;
  inset: 0;
  background-image:
    linear-gradient(rgba(255,0,60,0.06) 1px, transparent 1px),
    linear-gradient(90deg, rgba(255,0,60,0.06) 1px, transparent 1px);
  background-size: 40px 40px;
  pointer-events: none;
}

/* NEON TEXT */
.neon {
  text-shadow: 0 0 6px #ff0040, 0 0 20px #ff0040;
  color: #ff2b5c;
}

/* FADE IN */
.fade {
  opacity: 0;
  transform: translateY(30px);
  transition: 0.8s ease;
}
.fade.show {
  opacity: 1;
  transform: translateY(0);
}

/* FLOAT BUTTON */
.floating {
  position: fixed;
  bottom: 20px;
  right: 20px;
  background: #ff0040;
  color: white;
  padding: 14px 18px;
  border-radius: 999px;
  box-shadow: 0 0 20px #ff0040;
  cursor: pointer;
  animation: pulse 1.5s infinite;
}

@keyframes pulse {
  0%,100% { transform: scale(1); }
  50% { transform: scale(1.08); }
}
</style>
</head>

<body>

<!-- FLOAT BUTTON -->
<div class="floating" onclick="document.getElementById('event').scrollIntoView({behavior:'smooth'})">
  🩸 Donate
</div>

<!-- HERO -->
<section class="text-center py-20 px-6 fade show">

  <h1 class="text-4xl md:text-6xl font-extrabold neon">
    🩸 Blood Donation Drive
  </h1>

  <p class="mt-4 text-gray-300 max-w-2xl mx-auto text-lg">
    Your blood could be someone’s second chance at life. One donation can help save up to three lives.
  </p>

</section>

<!-- ELIGIBILITY -->
<section class="max-w-5xl mx-auto px-6 py-12 fade">

  <h2 class="text-2xl font-bold neon mb-6">Eligibility Check</h2>

  <ul class="space-y-2 text-gray-300">
    <li>✔ Age: 16–65 years old</li>
    <li>✔ Minors may need parental consent</li>
    <li>✔ Weight: At least 50kg (110 lbs)</li>
    <li>✔ Must be in good general health</li>
    <li>✔ Bring a valid ID</li>
  </ul>

  <h3 class="mt-6 text-xl font-semibold text-red-400">Before Donating</h3>
  <ul class="space-y-2 text-gray-300 mt-2">
    <li>💤 Get a good night’s sleep</li>
    <li>🥗 Eat a light meal</li>
    <li>💧 Stay hydrated</li>
    <li>❤️ Stay calm</li>
  </ul>

</section>

<!-- BENEFICIARIES -->
<section class="bg-black border-t border-b border-red-900 py-14 px-6 fade">

  <h2 class="text-3xl text-center neon mb-10">Who Does Your Donation Help?</h2>

  <div class="grid md:grid-cols-3 gap-6 max-w-6xl mx-auto">

    <div class="border border-red-900 p-6 rounded-2xl">
      <h3 class="font-bold">👩‍🍼 Mothers in Labor</h3>
      <p class="text-gray-400">Helps during childbirth complications.</p>
    </div>

    <div class="border border-red-900 p-6 rounded-2xl">
      <h3 class="font-bold">🏥 Surgery Patients</h3>
      <p class="text-gray-400">Supports emergency operations.</p>
    </div>

    <div class="border border-red-900 p-6 rounded-2xl">
      <h3 class="font-bold">🧒 Children with Illnesses</h3>
      <p class="text-gray-400">Requires life-saving transfusions.</p>
    </div>

  </div>

</section>

<!-- BENEFITS -->
<section class="max-w-5xl mx-auto px-6 py-12 fade">

  <h2 class="text-2xl neon mb-6">Donor Benefits</h2>

  <div class="grid md:grid-cols-2 gap-6">

    <div class="border border-red-900 p-6 rounded-2xl">
      <h3 class="font-bold text-red-400">Health Benefits</h3>
      <ul class="text-gray-300 mt-2 space-y-2">
        <li>🩺 Blood pressure screening</li>
        <li>❤️ Hemoglobin check</li>
        <li>🩸 Blood typing</li>
      </ul>
    </div>

    <div class="border border-red-900 p-6 rounded-2xl">
      <h3 class="font-bold text-red-400">Appreciation</h3>
      <ul class="text-gray-300 mt-2 space-y-2">
        <li>🍪 Snacks</li>
        <li>🎁 Certificates</li>
        <li>🎟 Tokens</li>
      </ul>
    </div>

  </div>
</section>

<!-- EVENT -->
<section id="event" class="text-center py-14 fade">

  <h2 class="text-3xl neon mb-6">Event Details</h2>

  <p>📅 May 20, 2026</p>
  <p>🕒 12:00 PM - 3:00 PM</p>
  <p>📍 National University - Lipa</p>

  <button class="mt-6 bg-red-600 px-6 py-3 rounded-xl font-bold">
    Register Now
  </button>

</section>

<!-- QR -->
<section class="text-center py-12 fade">

  <h2 class="neon text-2xl mb-4">Scan to Register</h2>
  <div id="qrcode" class="flex justify-center"></div>

</section>

<!-- SCRIPT -->
<script>
// scroll animation
const f = document.querySelectorAll('.fade');
const observer = new IntersectionObserver(entries=>{
  entries.forEach(e=>{
    if(e.isIntersecting) e.target.classList.add('show');
  });
});
f.forEach(el=>observer.observe(el));

// QR
new QRCode(document.getElementById("qrcode"), {
  text: "https://forms.gle/example",
  width: 180,
  height: 180,
  colorDark: "#ff0040",
  colorLight: "#000"
});
</script>

</body>
</html>
