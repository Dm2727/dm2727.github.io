<script>
  function pad(n, s) {
    return String(n).padStart(s, "0");
  }

  // MTWTFSS order
  const days = ["Monday","Tuesday","Wednesday","Thursday","Friday","Saturday","Sunday"];
  const months = ["January","February","March","April","May","June","July","August","September","October","November","December"];

  // Convert JS Sunday=0 to Monday=0
  function getDayMTWTFSS(d) {
    return days[(d.getDay() + 6) % 7];
  }

  // Day of year
  function getDayOfYear(d) {
    const start = new Date(d.getFullYear(), 0, 1);
    const diff = d - start;
    return Math.floor(diff / 86400000) + 1;
  }

  // ISO week number (Monday-based)
  function getISOWeek(d) {
    const date = new Date(d.getTime());
    date.setHours(0,0,0,0);

    // Thursday of this week determines the year
    date.setDate(date.getDate() + 3 - ((date.getDay() + 6) % 7));

    const week1 = new Date(date.getFullYear(), 0, 4);
    return 1 + Math.round(((date - week1) / 86400000 - 3 + ((week1.getDay() + 6) % 7)) / 7);
  }

  // Quarter (1–4)
  function getQuarter(d) {
    return Math.floor(d.getMonth() / 3) + 1;
  }

  function updateDigitalAndCalendar(now) {
    const t =
      now.getFullYear() + "/" +
      pad(now.getMonth() + 1, 2) + "/" +
      pad(now.getDate(), 2) + " " +
      pad(now.getHours(), 2) + ":" +
      pad(now.getMinutes(), 2) + ":" +
      pad(now.getSeconds(), 2) + "." +
      pad(now.getMilliseconds(), 3);

    document.getElementById("digital").textContent = t;

    const dayName = getDayMTWTFSS(now);
    const monthName = months[now.getMonth()];
    const dayOfYear = getDayOfYear(now);
    const weekNum = getISOWeek(now);
    const quarter = getQuarter(now);

    const calendarText =
      `${dayName}, ${monthName} ${now.getDate()}, ${now.getFullYear()}  
       Day ${dayOfYear} • Week ${weekNum} • Q${quarter}`;

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
</script>
