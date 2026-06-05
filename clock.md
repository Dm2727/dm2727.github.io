<!DOCTYPE html>
<html>
<head>
  <title>CLOCKEEY</title>
  <style>
    body {
      font-family: "Arial Rounded MT Bold", "Arial Rounded MT", Arial, sans-serif;
      background: #0d1117;
      color: #c9d1d9;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      height: 100vh;
      gap: 25px;
    }

    #digital {
      font-size: 26px;
      letter-spacing: 1px;
    }

    #calendar {
      font-size: 20px;
      color: #8b949e;
    }

    .clock {
      width: 280px;
      height: 280px;
      border: 6px solid #30363d;
      border-radius: 50%;
      position: relative;
      background: #111820;
      box-shadow: 0 0 12px #000;
    }

    .number {
      position: absolute;
      font-size: 20px;
      font-weight: bold;
      color: #c9d1d9;
      transform: translate(-50%, -50%);
    }

    .hand {
      position: absolute;
      width: 50%;
      height: 2px;
      background: #c9d1d9;
      top: 50%;
      left: 50%;
      transform-origin: 0% 50%;
    }

    #hour {
      height: 4px;
      width: 30%;
      background: #58a6ff;
    }

    #minute {
      height: 3px;
      width: 40%;
      background: #8b949e;
    }

    #second {
      height: 2px;
      width: 45%;
      background: #ff7b72;
    }

    #ms {
      height: 1px;
      width: 48%;
      background: #00ff9d;
      opacity: 0.8;
    }

    .center-dot {
      width: 14px;
      height: 14px;
      background: #c9d1d9;
      border-radius: 50%;
      position: absolute;
      top: calc(50% - 7px);
      left: calc(50% - 7px);
    }
  </style>
</head>
<body>
  <div id="digital">Loading...</div>
  <div id="calendar">Loading date...</div>

  <div class="clock">
    <!-- Numbers -->
    <div class="number" style="top: 20px; left: 140px;">12</div>
    <div class="number" style="top: 55px; left: 220px;">1</div>
    <div class="number" style="top: 110px; left: 260px;">2</div>
    <div class="number" style="top: 170px; left: 270px;">3</div>
    <div class="number" style="top: 230px; left: 260px;">4</div>
    <div class="number" style="top: 275px; left: 220px;">5</div>
    <div class="number" style="top: 300px; left: 140px;">6</div>
    <div class="number" style="top: 275px; left: 60px;">7</div>
    <div class="number" style="top: 230px; left: 20px;">8</div>
    <div class="number" style="top: 170px; left: 10px;">9</div>
    <div class="number" style="top: 110px; left: 20px;">10</div>
    <div class="number" style="top: 55px; left: 60px;">11</div>

    <!-- Hands -->
    <div id="hour" class="hand"></div>
    <div id="minute" class="hand"></div>
    <div id="second" class="hand"></div>
    <div id="ms" class="hand"></div>

    <div class="center-dot"></div>
  </div>

  <script>
    function pad(n, s) {
      return String(n).padStart(s, "0");
    }

    function updateDigitalAndCalendar() {
      const d = new Date();
      const t =
        d.getFullYear() + "/" +
        pad(d.getMonth() + 1, 2) + "/" +
        pad(d.getDate(), 2) + " " +
        pad(d.getHours(), 2) + ":" +
        pad(d.getMinutes(), 2) + ":" +
        pad(d.getSeconds(), 2) + "." +
        pad(d.getMilliseconds(), 3);

      document.getElementById("digital").textContent = t;

      const days = ["Monday","Tuesday","Wednesday","Thursday","Friday","Saturday","Sunday];
      const months = ["January","February","March","April","May","June","July","August","September","October","November","December"];

      const calendarText =
        days[d.getDay()] + ", " +
        months[d.getMonth()] + " " +
        d.getDate() + ", " +
        d.getFullYear();

      document.getElementById("calendar").textContent = calendarText;
    }

    function updateAnalog() {
      const now = new Date();

      const msDeg = now.getMilliseconds() * 0.36; // 1000 ms = 360°
      const secDeg = now.getSeconds() * 6 + msDeg / 60;
      const minDeg = now.getMinutes() * 6 + secDeg / 60;
      const hrDeg  = (now.getHours() % 12) * 30 + minDeg / 12;

      document.getElementById("hour").style.transform = `rotate(${hrDeg}deg)`;
      document.getElementById("minute").style.transform = `rotate(${minDeg}deg)`;
      document.getElementById("second").style.transform = `rotate(${secDeg}deg)`;
      document.getElementById("ms").style.transform = `rotate(${msDeg}deg)`;
    }

    setInterval(() => {
      updateDigitalAndCalendar();
      updateAnalog();
    }, 10);

    updateDigitalAndCalendar();
    updateAnalog();
  </script>
</body>
</html>
