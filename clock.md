<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>clock 3.0</title>

  <!-- Montserrat font -->
  <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;600&display=swap" rel="stylesheet">

  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      background: #0f0c29;
      color: #ffffff;
      font-family: "Montserrat", sans-serif;
    }

    .clock {
      padding: 30px 40px;
      border-radius: 16px;
      background: rgba(255, 255, 255, 0.06);
      border: 1px solid rgba(255, 255, 255, 0.15);
      backdrop-filter: blur(10px);
      box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5);
      font-size: 2rem;
      letter-spacing: 0.1em;
    }
  </style>
</head>
<body>
  <div class="clock" id="clock">0000/00/00 00:00:00.000</div>

  <script>
    function pad(num, size) {
      return num.toString().padStart(size, "0");
    }

    function updateClock() {
      const now = new Date();

      const year  = now.getFullYear();
      const month = pad(now.getMonth() + 1, 2); // 0-based
      const day   = pad(now.getDate(), 2);

      const hours   = pad(now.getHours(), 2);
      const minutes = pad(now.getMinutes(), 2);
      const seconds = pad(now.getSeconds(), 2);

      const ms = pad(now.getMilliseconds(), 3); // msmsms (3 digits)

      const formatted =
        `${year}/${month}/${day} ${hours}:${minutes}:${seconds}.${ms}`;

      document.getElementById("clock").textContent = formatted;
    }

    // Update every 10ms for a smooth millisecond display
    setInterval(updateClock, 10);
    updateClock();
  </script>
</body>
</html>
