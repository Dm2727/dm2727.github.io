<!DOCTYPE html>
<html lang="en">
<head>
    https://fonts.googleapis.com
https://fonts.gstatic.com
https://fonts.googleapis.com/css2?family=Open+Sans:wght@300;400;500;600;700&display=swap
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>rng</title>

<style>
body{
    font-family:Montserrat,sans-serif;
    background:#1a1a1a;
    color:white;
    text-align:center;
    padding:20px;
}

h1{
    color:#00ccff;
}

#entries{
    max-width:700px;
    margin:20px auto;
}

.entry{
    display:flex;
    justify-content:center;
    gap:10px;
    margin:10px 0;
}

.name,.odds{
    padding:8px;
    border:none;
    border-radius:5px;
}

.name{
    width:250px;
}

.odds{
    width:90px;
    text-align:center;
    background:#ddd;
}

button{
    padding:10px 15px;
    border:none;
    border-radius:5px;
    cursor:pointer;
    color:white;
}

.add{
    background:#28a745;
}

.roll{
    background:#007bff;
}

.reset{
    background:#ff9800;
}

.remove{
    background:#dc3545;
}

#result{
    margin-top:25px;
    font-size:28px;
    color:gold;
    font-weight:bold;
}
</style>
</head>
<body>

<h1>rng 1.0.2</h1>

<div id="entries"></div>

<button class="add" onclick="addEntry()">Add Name</button>
<button class="roll" onclick="roll()">Pick Random</button>
<button class="reset" onclick="resetList()">Reset</button>

<div id="result">unpicked</div>

<script>
const entriesDiv = document.getElementById("entries");

function updateOdds() {
    const entries = document.querySelectorAll(".entry");

    if (entries.length === 0) return;

    const chance = (100 / entries.length).toFixed(2);

    entries.forEach(entry => {
        entry.querySelector(".odds").value = chance + "%";
    });
}

function addEntry(name = "") {
    const div = document.createElement("div");
    div.className = "entry";

    div.innerHTML = `
        <label>Name:</label>
        <input class="name" placeholder="Enter name" value="${name}">
        <input class="odds" type="text" readonly>
        <button class="remove">X</button>
    `;

    div.querySelector(".remove").onclick = () => {
        div.remove();
        updateOdds();

        if (document.querySelectorAll(".entry").length === 0) {
            document.getElementById("result").textContent = "No names added";
        }
    };

    entriesDiv.appendChild(div);
    updateOdds();
}

function resetList() {
    entriesDiv.innerHTML = "";
    addEntry();
    document.getElementById("result").textContent = "Reset complete";
}

function roll() {
    const items = [...document.querySelectorAll(".entry")];

    if (items.length === 0) {
        document.getElementById("result").textContent =
            "1+ name";
        return;
    }

    const winner = items[Math.floor(Math.random() * items.length)];
    const name = winner.querySelector(".name").value.trim() || "Unnamed";

    document.getElementById("result").textContent =
        "picked: " + name;
}

addEntry();
</script>

</body>
</html>
