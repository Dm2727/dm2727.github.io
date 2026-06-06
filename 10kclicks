ads that redirect to other ads (simulated)
ONLY FOR EDUCATIONAL ONLY


<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>10,000 Clicks: The Ad Web Simulator</title>

<style>
    body {
        font-family: Arial, sans-serif;
        background: #f0f0f0;
        margin: 0;
        padding: 20px;
    }

    .container {
        max-width: 800px;
        margin: auto;
        background: white;
        padding: 25px;
        border-radius: 12px;
        box-shadow: 0 0 15px rgba(0,0,0,0.15);
    }

    h1 {
        margin-top: 0;
    }

    #website {
        font-size: 2em;
        margin: 20px 0;
    }

    #description {
        min-height: 60px;
        color: #444;
    }

    button {
        padding: 12px 20px;
        font-size: 18px;
        cursor: pointer;
        margin-top: 15px;
    }

    #history {
        margin-top: 25px;
        max-height: 250px;
        overflow-y: auto;
        border: 1px solid #ddd;
        padding: 10px;
        background: #fafafa;
    }

    .entry {
        margin-bottom: 5px;
    }

    .milestone {
        font-weight: bold;
        color: darkred;
    }
</style>
</head>
<body>

<div class="container">

    <h1>🖱 10,000 Clicks: Ad Web Simulator</h1>

    <p>
        You may only navigate by clicking advertisements.
    </p>

    <p><strong>Clicks:</strong> <span id="clicks">0</span></p>

    <div id="website">📰 Major News Article</div>

    <div id="description">
        You begin on a reputable news website.
    </div>

    <button onclick="clickAd()">Click Advertisement</button>

    <div id="history"></div>

</div>

<script>

let clicks = 0;

const stages = [

{
    max: 20,
    sites: [
        ["📰 Major News", "A large news publisher."],
        ["🏟 Sports Network", "Live scores and sports coverage."],
        ["💼 Business News", "Market headlines."],
        ["🛒 Major Retailer", "Shop today's deals."],
        ["📱 Tech Brand", "Latest gadgets."]
    ]
},

{
    max: 100,
    sites: [
        ["🍳 Recipe Blog", "Grandma's secret recipes."],
        ["✈ Travel Blog", "10 places to visit."],
        ["📷 Lifestyle Blog", "Influencer recommendations."],
        ["🐶 Pet Advice", "How to train your dog."],
        ["🎬 Entertainment News", "Celebrity updates."]
    ]
},

{
    max: 300,
    sites: [
        ["📋 Top 10 List", "You won't believe #7."],
        ["🧠 Personality Quiz", "Which potato are you?"],
        ["😂 Meme Site", "Internet humor."],
        ["😲 Viral Stories", "Amazing transformations."],
        ["📖 Slideshow", "Click next to continue."]
    ]
},

{
    max: 700,
    sites: [
        ["💰 Insurance Eligibility", "See if you qualify."],
        ["🏠 Mortgage Quotes", "Compare lenders."],
        ["🚗 Car Insurance", "Save money today."],
        ["🎓 College Match", "Find your school."],
        ["☎ Expert Consultation", "Speak with an advisor."]
    ]
},

{
    max: 1500,
    sites: [
        ["📢 Ad Farm", "Ads surrounding thin content."],
        ["➡ Continue Page", "One more click..."],
        ["🎁 Sweepstakes", "Claim your reward."],
        ["📱 App Promotion", "Install now!"],
        ["🎯 Sponsored Discovery", "Recommended for you."]
    ]
},

{
    max: 3000,
    sites: [
        ["⚠ Fake Update", "Update your browser."],
        ["💸 Miracle Offer", "Lose weight instantly."],
        ["🪙 Crypto Riches", "Become wealthy."],
        ["💻 Tech Support", "Call immediately."],
        ["💳 Subscription Trap", "Free trial!"]
    ]
},

{
    max: Infinity,
    sites: [
        ["🎰 Online Casino", "Play now."],
        ["⚽ Sports Betting", "Bet today."],
        ["🪙 Crypto Casino", "Provably fair."],
        ["❤️ Dating Affiliate", "Meet singles."],
        ["🔄 Redirect Loop", "More ads await."],
        ["💰 Passive Income Funnel", "Earn while sleeping."],
        ["📈 Forex Affiliate", "Trade currencies."],
        ["🎲 Sweepstakes Casino", "Free spins."]
    ]
}

];

function clickAd() {

    clicks++;

    document.getElementById("clicks").textContent = clicks;

    let stage;

    for (const s of stages) {
        if (clicks <= s.max) {
            stage = s;
            break;
        }
    }

    const site =
        stage.sites[
            Math.floor(Math.random() * stage.sites.length)
        ];

    document.getElementById("website").textContent = site[0];
    document.getElementById("description").textContent = site[1];

    addHistory(site[0]);

    randomEvents();
}

function addHistory(siteName) {

    const history = document.getElementById("history");

    const div = document.createElement("div");

    div.className = "entry";

    div.textContent = "#" + clicks + " → " + siteName;

    history.prepend(div);

    while (history.children.length > 30) {
        history.removeChild(history.lastChild);
    }
}

function randomEvents() {

    if (clicks === 100) milestone(
        "You barely remember the original news article."
    );

    if (clicks === 500) milestone(
        "You realize many sites exist mainly to show more ads."
    );

    if (clicks === 1000) milestone(
        "You are trapped in the lead-generation ecosystem."
    );

    if (clicks === 3000) milestone(
        "You've entered the attention sink."
    );

    if (clicks === 10000) milestone(
        "Congratulations. The internet has become one giant ad loop."
    );

    /* Rare escape */

    if (clicks > 500 && Math.random() < 0.01) {

        document.getElementById("website").textContent =
            "📰 Major News";

        document.getElementById("description").textContent =
            "Against all odds, you escaped back to the surface web.";

        milestone("You escaped!");
    }
}

function milestone(text) {

    const history = document.getElementById("history");

    const div = document.createElement("div");

    div.className = "entry milestone";

    div.textContent = "★ " + text;

    history.prepend(div);
}

</script>

</body>
</html>
