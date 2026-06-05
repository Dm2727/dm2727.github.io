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
      align-items: center;
      padding: 20px;
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

    #hour { height: 4px; width: 30%; background: #58a6ff; }
    #minute { height: 3px; width: 40%; background: #8b949e; }
    #second { height: 2px; width: 45%; background: #ff7b72; }
    #ms { height: 1px; width: 48%; background: #00ff9d; opacity: 0.8; }

    .center-dot {
      width: 14px;
      height: 14px;
      background: #c9d1d9;
      border-radius: 50%;
      position: absolute;
      top: calc(50% - 7px);
      left: calc(50% - 7px);
    }

    /* YEAR CALENDAR */
    #yearCalendar {
      width: 90%;
      max-width: 900px;
      height: 300px;
      overflow-y: scroll;
      background: #111820;
      border: 2px solid #30363d;
      padding: 20px;
      border-radius: 12px;
      color: #c9d1d9;
    }

    .month { margin-bottom: 25px; }
    .month h2 { margin: 0 0 10px 0; color: #58a6ff; }

    .days {
      display: grid;
      grid-template-columns: repeat(7, 1fr);
      gap: 6px;
    }

    .day {
      padding: 8px;
      background: #0d1117;
      border: 1px solid #30363d;
      border-radius: 6px;
      text-align: center;
      cursor: pointer;
    }

    .day:hover { background: #1a2330; }

    .hasNote {
      background: #003b2f !important;
      border-color: #00ff9d;
    }

    /* NOTE PANEL */
    #notePanel {
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background: #111820;
      padding: 20px;
      border: 2px solid #30363d;
      border-radius: 12px;
      width: 300px;
      color: #c9d1d9;
      display: none;
    }

    #noteText {
      width: 100%;
      height: 120px;
      background: #0d1117;
      border: 1px solid #30363d;
      color: #c9d1d9;
      border-radius: 6px;
      padding: 8px;
    }

    button {
      margin-top: 10px;
      padding: 8px 12px;
      background: #30363d;
      color: #c9d1d9;
      border: none;
      border-radius: 6px;
      cursor: pointer;
    }

    button:hover {
      background: #3f4954;
    }
  </style>
</head>
<body>

  <div id="digital">Loading...</div>
  <div id="calendar">Loading date...</div>

  <div class="clock">
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

    <div id="hour" class="hand"></div>
    <div id="minute" class="hand"></div>
    <div id="second" class="hand"></div>
    <div id="ms" class="hand"></div>

    <div class="center-dot"></div>
  </div>

  <div id="yearCalendar"></div>

  <div id="notePanel">
    <h3 id="noteDate"></h3>
    <textarea id="noteText" placeholder="Write your note here..."></textarea>
    <button onclick="saveNote()">Save Note</button>
    <button onclick="closeNote()">Close</button>
  </div>

  <script>
    function pad(n, s) { return String(n).padStart(s, "0"); }

    const days = ["Monday","Tuesday","Wednesday","Thursday","Friday","Saturday","Sunday"];
    const months = ["January","February","March","April","May","June","July","August","September","October","November","December"];

    function getDayMTWTFSS(d) { return days[(d.getDay() + 6) % 7]; }

    function updateDigitalAndCalendar(now) {
      const t =
        now.getFullYear() + "/" +
        pad(now.getMonth() + 1, 2) + "/" +
        pad(now.getDate(), 2) + " " +
        pad(now.getHours(), 2) + ":" +
        pad(now.getMinutes(), 2) + ":" +
        pad(now.getSeconds(), 2) + ":" +
        pad(now.getMilliseconds(), 3);

      document.getElementById("digital").textContent = t;

      const calendarText =
        getDayMTWTFSS(now) + ", " +
        months[now.getMonth()] + " " +
        now.getDate() + ", " +
        now.getFullYear();

      document.getElementById("calendar").textContent = calendarText;
    }

    function updateAnalog(now) {
      const ms = now.getMilliseconds();
      const sec = now.getSeconds() + ms / 1000;
      const min = now.getMinutes() + sec / 60;
      const hr  = (now.getHours() % 12) + min / 60;

      document.getElementById("hour").style.transform = `rotate(${hr * 30}deg)`;
      document.getElementById("minute").style.transform = `rotate(${min * 6}deg)`;
      document.getElementById("second").style.transform = `rotate(${sec * 6}deg)`;
      document.getElementById("ms").style.transform = `rotate(${ms * 0.36}deg)`;
    }

    function tick() {
      const now = new Date();
      updateDigitalAndCalendar(now);
      updateAnalog(now);
      requestAnimationFrame(tick);
    }

    tick();

    /* YEAR CALENDAR + NOTES */
    function buildYearCalendar(year) {
      const container = document.getElementById("yearCalendar");
      container.innerHTML = "";

      for (let m = 0; m < 12; m++) {
        const monthDiv = document.createElement("div");
        monthDiv.className = "month";

        const title = document.createElement("h2");
        title.textContent = months[m] + " " + year;
        monthDiv.appendChild(title);

        const daysGrid = document.createElement("div");
        daysGrid.className = "days";

        const firstDay = new Date(year, m, 1).getDay();
        const offset = (firstDay + 6) % 7;

        for (let i = 0; i < offset; i++) {
          const empty = document.createElement("div");
          daysGrid.appendChild(empty);
        }

        const daysInMonth = new Date(year, m + 1, 0).getDate();

        for (let d = 1; d <= daysInMonth; d++) {
          const dayDiv = document.createElement("div");
          dayDiv.className = "day";
          dayDiv.textContent = d;

          const key = `${year}-${m+1}-${d}`;
          if (localStorage.getItem("note-" + key)) {
            dayDiv.classList.add("hasNote");
          }

          dayDiv.onclick = () => openNote(year, m, d);
          daysGrid.appendChild(dayDiv);
        }

        monthDiv.appendChild(daysGrid);
        container.appendChild(monthDiv);
      }
    }

    function openNote(year, month, day) {
      const key = `${year}-${month+1}-${day}`;
      document.getElementById("noteDate").textContent = key;
      document.getElementById("noteText").value =
        localStorage.getItem("note-" + key) || "";
      document.getElementById("notePanel").style.display = "block";
    }

    function saveNote() {
      const key = document.getElementById("noteDate").textContent;
      const text = document.getElementById("noteText").value;

      if (text.trim() === "") {
        localStorage.removeItem("note-" + key);
      } else {
        localStorage.setItem("note-" + key, text);
      }

      buildYearCalendar(new Date().getFullYear());
      closeNote();
    }

    function closeNote() {
      document.getElementById("notePanel").style.display = "none";
    }

    buildYearCalendar(new Date().getFullYear());
  </script>

</body>
</html>
