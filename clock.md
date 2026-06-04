CLOCKEEY
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Clock</title>
<style>
  body {
    font-family: monospace;
    background: #0d1117;
    color: #c9d1d9;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    font-size: 24px;
  }
</style>
</head>
<body>
<div id="clock"></div>

<script>
function pad(n, s) { return String(n).padStart(s, "0"); }

function tick() {
  const d = new Date();
  const t =
    d.getFullYear() + "/" +
    pad(d.getMonth() + 1, 2) + "/" +
    pad(d.getDate(), 2) + " " +
    pad(d.getHours(), 2) + ":" +
    pad(d.getMinutes(), 2) + ":" +
    pad(d.getSeconds(), 2) + "." +
    pad(d.getMilliseconds(), 3);

  document.getElementById("clock").textContent = t;
}

setInterval(tick, 10);
tick();
</script>
</body>
</html> 

<div style="width:200px;height:200px;border:8px solid #30363d;border-radius:50%;position:relative;margin:20px auto;background:#0d1117;">
  <div id="hour" style="position:absolute;width:6px;height:50px;background:#c9d1d9;top:50px;left:97px;transform-origin:bottom;"></div>
  <div id="minute" style="position:absolute;width:4px;height:70px;background:#58a6ff;top:30px;left:98px;transform-origin:bottom;"></div>
  <div id="second" style="position:absolute;width:2px;height:80px;background:#ff7b72;top:20px;left:99px;transform-origin:bottom;"></div>
</div>

<script>
function rotate() {
  const now = new Date();
  const sec = now.getSeconds() * 6;
  const min = now.getMinutes() * 6 + sec / 60;
  const hr  = ((now.getHours() % 12) * 30) + min / 12;

  document.getElementById("hour").style.transform = "rotate(" + hr + "deg)";
  document.getElementById("minute").style.transform = "rotate(" + min + "deg)";
  document.getElementById("second").style.transform = "rotate(" + sec + "deg)";
}
setInterval(rotate, 1000);
rotate();
</script>
