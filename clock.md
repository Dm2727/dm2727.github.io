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
