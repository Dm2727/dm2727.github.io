<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Educational Gambling Awareness Slots</title>
<style>
  body {
    background:#050510; color:#eee; font-family:Arial, sans-serif;
    padding:20px; text-align:center;
  }
  h1 { color:#ffd54f; margin-bottom:5px; text-shadow:0 0 8px #ff6; }
  h2 { color:#ffca28; }

  .machine {
    background:radial-gradient(circle at top, #333 0, #111 60%);
    padding:20px; border-radius:12px;
    max-width:750px; margin:20px auto;
    box-shadow:0 0 25px rgba(0,0,0,0.8);
    border:1px solid #444;
  }
  .reels {
    display:flex; justify-content:space-between;
    margin:20px 0;
  }
  .reel {
    background:#222; padding:15px 20px; border-radius:10px;
    font-size:2.7rem; width:100px;
    box-shadow:0 0 10px rgba(0,0,0,0.7);
  }
  .reel.near-miss {
    box-shadow:0 0 15px #ff9800;
    border:1px solid #ffb74d;
  }
  button {
    background:#ff7043; color:#fff; border:none;
    padding:12px 25px; border-radius:6px;
    font-size:1rem; cursor:pointer;
  }
  button:hover { background:#ff8a65; }

  .stats { margin-top:15px; font-size:0.95rem; }
  .loss { color:#ff5252; font-weight:bold; }
  .win { color:#81c784; font-weight:bold; }

  .info, .panel {
    margin-top:25px; background:#1e1e1e; padding:15px;
    border-radius:8px; max-width:750px;
    margin-left:auto; margin-right:auto;
    text-align:left;
  }
  .info { border-left:4px solid #ffca28; }
  .panel { border-left:4px solid #64b5f6; }

  .info-title { color:#ffca28; font-weight:bold; margin-bottom:8px; }
  .panel-title { color:#64b5f6; font-weight:bold; margin-bottom:8px; }

  .calc-box {
    background:#121212; padding:20px; border-radius:10px;
    border-left:4px solid #ff5252; max-width:750px;
    margin:40px auto; text-align:left;
  }
  input {
    width:100%; padding:8px; margin:8px 0;
    border-radius:5px; border:none;
    background:#222; color:#eee;
  }

  .toggle-row {
    margin-top:10px; font-size:0.9rem; display:flex;
    align-items:center; gap:8px;
  }

  .buy-instead {
    margin-top:15px; font-size:0.95rem;
  }
  .buy-item {
    margin:4px 0;
  }
  .buy-hit {
    color:#81c784; font-weight:bold;
  }

  .near-miss-text {
    margin-top:10px; font-size:0.9rem; color:#ffb74d;
  }

  .rigging-info {
    margin-top:10px; font-size:0.9rem; color:#ccc;
  }
</style>
</head>
<body>

<h1>Educational Slot Machine & Gambling Awareness</h1>
<p style="color:#ccc;">This is not real gambling. It shows how the odds, near‑misses, and losses work in your head and your wallet.</p>

<div class="machine">
  <div class="reels">
    <div class="reel" id="r1">🍒</div>
    <div class="reel" id="r2">🍋</div>
    <div class="reel" id="r3">🟥</div>
    <div class="reel" id="r4">7️⃣</div>
    <div class="reel" id="r5">❌</div>
  </div>

  <button id="spin">Spin</button>

  <div class="stats">
    Spins: <span id="spins">0</span><br>
    Real‑world cost so far: <span id="realCost" class="loss">£0.00</span><br>
    Wagered (credits): <span id="wager">0</span><br>
    Won (credits): <span id="won">0</span><br>
    Net (credits): <span id="net" class="loss">0</span><br>
    Near‑misses: <span id="nearMissCount">0</span>
  </div>

  <div class="toggle-row" style="justify-content:center;">
    <input type="checkbox" id="riggedMode">
    <label for="riggedMode">Show “rigged” casino mode (educational demo of house edge)</label>
  </div>

  <div id="nearMissText" class="near-miss-text"></div>
</div>

<div class="info">
  <div class="info-title">Is it catchy for you?</div>
  So you’ve been into one of these — hard to escape, right?
  The colours, the rhythm, the “almost win” moments…
  They’re designed to keep you spinning.
  <div style="margin-top:8px; color:#ffab91;">
    Even with fake credits, notice how easy it is to keep clicking. In a real casino, that’s real money and real time.
  </div>
</div>

<div class="panel">
  <div class="panel-title">Near‑miss psychology</div>
  A “near miss” is when the reels stop just one symbol away from a big win.
  Your brain reacts almost like you actually won — even though you lost.
  This demo makes near‑misses happen more often than pure chance, to show how powerful that feeling can be.
</div>

<!-- Gambling Awareness Calculator -->
<div class="calc-box">
  <h2>Gambling Awareness Calculator</h2>

  <label>How much do you usually gamble per day (£)?</label>
  <input id="dailySpend" type="number" value="20" min="0">

  <label>How many days per week do you gamble?</label>
  <input id="daysPerWeek" type="number" value="3" min="0" max="7">

  <label>Average return (slots usually 85–95%)</label>
  <input id="returnRate" type="number" value="90" min="50" max="100">

  <button onclick="calcLosses()">Calculate</button>

  <div id="results" style="margin-top:20px; font-size:1rem; line-height:1.5;"></div>

  <div class="buy-instead" id="buyInstead"></div>
</div>

<div class="panel">
  <div class="panel-title">How casinos “rig” the game (legally)</div>
  Casinos don’t usually cheat spin‑by‑spin — they design the math so that, over time, they always win.
  <div class="rigging-info" id="riggingInfo">
    In normal mode, this demo pays back about 90% of what you bet (10% house edge).  
    In “rigged” demo mode, the payouts are reduced further and near‑misses are more common, to show how the edge can grow.
  </div>
</div>

<script>
/* SLOT MACHINE */
const symbols = ["🍒","7️⃣","🟥","🍋","❌"];
const reels = ["r1","r2","r3","r4","r5"].map(id => document.getElementById(id));

let spins = 0, wager = 0, won = 0, nearMissCount = 0;
const costPerSpin = 10; // credits
const realMoneyPerSpin = 0.20; // 20p per spin

const nearMissTextEl = document.getElementById("nearMissText");
const riggedModeEl = document.getElementById("riggedMode");

function clearNearMissHighlight() {
  reels.forEach(r => r.classList.remove("near-miss"));
  nearMissTextEl.textContent = "";
}

function makeRandomResult() {
  return reels.map(() => symbols[Math.floor(Math.random()*symbols.length)]);
}

// Create a near‑miss pattern: 4 of a kind and one ❌ or one off‑symbol
function makeNearMissResult() {
  const winSymbol = symbols[Math.floor(Math.random()*4)]; // not ❌
  const result = new Array(5).fill(winSymbol);
  const missIndex = Math.floor(Math.random()*5);
  // either ❌ or a different non‑win symbol
  const otherChoices = symbols.filter(s => s !== winSymbol);
  result[missIndex] = otherChoices[Math.floor(Math.random()*otherChoices.length)];
  return result;
}

document.getElementById("spin").addEventListener("click", () => {
  clearNearMissHighlight();
  spins++;
  wager += costPerSpin;

  const rigged = riggedModeEl.checked;

  let result;
  let isNearMiss = false;

  // Chance of forcing a near‑miss (higher in rigged demo mode)
  const nearMissChance = rigged ? 0.35 : 0.18;
  if (Math.random() < nearMissChance) {
    result = makeNearMissResult();
    isNearMiss = true;
  } else {
    result = makeRandomResult();
  }

  result.forEach((s, i) => {
    reels[i].textContent = s;
  });

  let payout = 0;

  if (!result.includes("❌")) {
    const counts = {};
    result.forEach(s => counts[s] = (counts[s]||0)+1);
    const max = Math.max(...Object.values(counts));

    if (max === 5) payout = costPerSpin * 50;
    else if (max === 4) payout = costPerSpin * 10;
    else if (max === 3) payout = costPerSpin * 3;
  }

  // Apply house edge: stronger reduction in rigged demo mode
  const reduction = rigged ? 0.4 : 0.6;
  payout = Math.floor(payout * reduction);
  won += payout;

  const net = won - wager;
  const realCost = spins * realMoneyPerSpin;

  document.getElementById("spins").textContent = spins;
  document.getElementById("wager").textContent = wager;
  document.getElementById("won").textContent = won;
  document.getElementById("net").textContent = net;
  document.getElementById("realCost").textContent = "£" + realCost.toFixed(2);

  if (net >= 0) {
    document.getElementById("net").classList.remove("loss");
    document.getElementById("net").classList.add("win");
  } else {
    document.getElementById("net").classList.add("loss");
    document.getElementById("net").classList.remove("win");
  }

  if (isNearMiss && payout === 0) {
    nearMissCount++;
    document.getElementById("nearMissCount").textContent = nearMissCount;
    reels.forEach(r => r.classList.add("near-miss"));
    nearMissTextEl.textContent =
      "NEAR MISS: Your brain reacts like you almost won, even though this is still a loss.";
  }
});

/* LOSS CALCULATOR + WHAT YOU COULD BUY INSTEAD */
function calcLosses() {
  const daily = Math.max(0, parseFloat(document.getElementById("dailySpend").value) || 0);
  const days = Math.max(0, Math.min(7, parseFloat(document.getElementById("daysPerWeek").value) || 0));
  const returnRateRaw = parseFloat(document.getElementById("returnRate").value) || 0;
  const returnRate = Math.max(0, Math.min(100, returnRateRaw)) / 100;

  const weeklySpend = daily * days;
  const yearlySpend = weeklySpend * 52;

  const expectedReturn = yearlySpend * returnRate;
  const expectedLoss = yearlySpend - expectedReturn;

  const savingsIfQuit = expectedLoss;
  const monthsOfBills = (savingsIfQuit / 150).toFixed(1);

  document.getElementById("results").innerHTML = `
    <div><strong>Weekly spend:</strong> £${weeklySpend.toFixed(2)}</div>
    <div><strong>Yearly spend:</strong> £${yearlySpend.toFixed(2)}</div>
    <div><strong>Expected yearly loss:</strong> <span style="color:#ff5252;">£${expectedLoss.toFixed(2)}</span></div>
    <div><strong>Money saved if you quit today:</strong> <span style="color:#81c784;">£${savingsIfQuit.toFixed(2)}</span></div>
    <div style="margin-top:10px; color:#ccc;">
      That’s enough to cover about <strong>${monthsOfBills}</strong> months of basic living costs.
    </div>
  `;

  updateBuyInstead(savingsIfQuit);
}

function updateBuyInstead(amount) {
  const el = document.getElementById("buyInstead");
  if (amount <= 0) {
    el.textContent = "";
    return;
  }

  const items = [
    { cost: 50,  text: "a week of groceries 🛒" },
    { cost: 200, text: "a budget smartphone 📱" },
    { cost: 600, text: "a short holiday or weekend away ✈️" },
    { cost: 1200, text: "a month of rent or bills 🏠" },
    { cost: 5000, text: "a decent used car 🚗" }
  ];

  let html = "<div><strong>What you could buy instead if you quit:</strong></div>";
  items.forEach(item => {
    if (amount >= item.cost) {
      html += `<div class="buy-item buy-hit">• £${item.cost} ≈ ${item.text}</div>`;
    } else {
      html += `<div class="buy-item">• £${item.cost} ≈ ${item.text}</div>`;
    }
  });

  el.innerHTML = html;
}
</script>

</body>
</html>
