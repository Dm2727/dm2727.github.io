ads that redirect to other ads (simulated)
ONLY FOR EDUCATIONAL ONLY


<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>10,000 Click Descent</title>
<style>
  body { font-family: Arial, sans-serif; background:#111; color:#eee; margin:0; padding:20px; }
  .container { max-width:900px; margin:auto; background:#1b1b1b; padding:25px; border-radius:12px; box-shadow:0 0 20px rgba(0,0,0,0.6); }
  h1 { margin-top:0; }
  #website { font-size:1.8em; margin:20px 0 10px; }
  #description { min-height:50px; color:#ccc; }
  button { padding:12px 22px; font-size:18px; cursor:pointer; margin-top:15px; border-radius:6px; border:none; background:#ffcc00; color:#000; }
  button:hover { background:#ffd944; }
  #history { margin-top:25px; max-height:260px; overflow-y:auto; border:1px solid #333; padding:10px; background:#151515; font-size:13px; }
  .entry { margin-bottom:4px; }
  .meta { margin-top:10px; font-size:14px; color:#aaa; }
</style>
</head>
<body>

<div class="container">
  <h1>🖱 10,000 Click Descent</h1>
  <p>You only move forward by clicking.</p>

  <p><strong>Clicks:</strong> <span id="clicks">0</span></p>

  <div id="website">📰 Major News Article</div>
  <div id="description">You begin on a reputable news website.</div>

  <button onclick="clickAd()">Click</button>

  <div id="history"></div>
</div>

<script>
let clicks = 0;

/* -------------------------
   TIER 1 — 50 (normal web)
------------------------- */
const tier1 = [
"New Flagship Phone Launch","Market Outlook 2026","Electric SUV Spotlight","Career Growth Guide",
"Home Renovation Trends","Easy Weeknight Meals","Travel Guide: New York","Improve Your Phone Photography",
"Morning Habits That Boost Energy","Best Wireless Earbuds of the Year","Credit Card Comparison 2026",
"Fitness Trends to Watch","Interior Design Inspiration","Beginner’s Investing Guide","Skincare Essentials",
"Seasonal Shopping Highlights","Books Everyone Is Talking About","Films Coming Out This Month",
"Gaming Releases 2026","Running Shoe Roundup","Sleep Improvement Tips","Smart Home Starter Guide",
"Travel Essentials Checklist","Wine Regions to Explore","Music Trends Emerging","Best Burgers in London",
"Streaming Picks","Kitchen Tools That Actually Help","Indoor Plants That Thrive","Productivity Software Guide",
"Budgeting Tips for 2026","Brain‑Boosting Activities","DIY Home Fixes","Winter Fashion Essentials",
"Blue‑Light Glasses Review","Cycling Routes Near You","Healthy Smoothie Ideas","Subscription Boxes Reviewed",
"Podcasts Everyone Loves","Cleaning Hacks That Work","Office Chair Comparison","Mobile App Recommendations",
"Clothing Care Tips","Online Safety Basics","Weekend Getaway Ideas","Baking Recipes to Try",
"Back‑to‑School Essentials","Health Trends 2026","Local Events Near You","Top Rated Restaurants Nearby"
];

/* -------------------------
   TIER 2 — 50 (SEO mush)
------------------------- */
const tier2 = [
"Dog Training Life Hacks 2026","7 Meals Anyone Can Make","12 Everyday Mistakes People Don’t Realise They Make",
"Personality Quiz","Travel Secrets Cabin Crew Mention","Relatable Work‑From‑Home Moments","25 Life Hacks People Swear By",
"Skincare Routines That Actually Work","Cleaning Tricks You Haven’t Tried","Herbal Drinks People Are Trying",
"Laundry Tips You Should Know","Hidden Phone Features","Budget Home Decor Ideas","Stress‑Relief Techniques",
"Fast Food Items Ranked","Photo Editing Tricks","Kitchen Hacks From Chefs","Plants That Clean the Air",
"Clothing Repair Basics","Games You Can Play in 5 Minutes","Books That Are Easy to Finish","Wellness Trends People Are Trying",
"Desserts Anyone Can Make","Sleep Tips That Actually Help","Packing Tips Frequent Travellers Use",
"Bathroom Cleaning Shortcuts","Puzzles That Improve Focus","Smoothie Recipes Trending","Clothing Items Everyone Should Own",
"DIY Fixes for Common Problems","Shows People Are Binge‑Watching","Music Playlists for Any Mood","Weekend Trip Ideas",
"Pizza Toppings Ranked","Household Items With Surprising Uses","Subscription Boxes Reviewed","Haircare Tips for 2026",
"Decluttering Tips That Work","Kitchen Gadgets People Love","Ice Cube Tray Hacks","Cleaning Products Ranked",
"Baking Tips for Beginners","Wellness Challenges Trending","Sewing Basics Anyone Can Learn","Skincare Ingredients Explained",
"Drinks That Boost Energy","Local Spots People Recommend","Household Myths Tested","Quick Cleaning Routines"
];

/* -------------------------
   TIER 3 — 50 (comparison tools)
------------------------- */
const tier3 = [
"Check Your Eligibility for Lower Rates","Home Finance Offers Near You","Drivers in Hampton Are Paying More",
"Flexible Online Courses","Speak With a Specialist","Review Updated Offers","Explore Investment Tools",
"Career Assessment Tool","Local Service Finder","Quick Eligibility Check","Request a Callback",
"Market Comparison Dashboard","Property Value Estimator","Vehicle Cost Calculator","Rewards Programme Finder",
"Delivery Savings Tool","Monthly Cost Breakdown","Financial Snapshot","Beginner’s Learning Centre",
"Consultation Request Form","Quick Quote Tool","Local Offers Updated","Compare Plans Side‑by‑Side",
"Personalised Results","Schedule a Short Call","Review Your Options","Explore Savings Opportunities",
"Compare Providers Instantly","Updated Listings Near You","Request More Information","Explore Tools and Resources",
"Quick Review Tool","Updated Comparison Results","Explore Offers in Your Area","Connect With a Representative",
"Instant Estimate Tool","Review Updated Plans","Explore Local Opportunities","Request a Follow‑Up",
"Quick Overview Tool","Updated Market Insights","Explore Personalised Tools","Book a Short Session",
"Review Your Matches","Compare Local Providers","Explore New Options","Request a Quick Chat",
"Instant Overview","Updated Recommendations"
];

/* -------------------------
   TIER 4 — 225 (fake virus + fake winnings)
------------------------- */
const tier4 = [
"CriticalAlert_your_system_is_at_risk","CriticalAlert_fake_virus_detected","CriticalAlert_click_to_secure_device",
"CriticalAlert_fake_hack_attempt","CriticalAlert_immediate_action_required","CriticalAlert_fake_ransom_notice",
"CriticalAlert_fake_security_breach","CriticalAlert_fake_malware_found","CriticalAlert_fake_trojan_warning",
"CriticalAlert_fake_spyware_detected",

"SystemPopup_your_computer_is_not_safe","SystemPopup_fake_scan_in_progress","SystemPopup_fake_cleanup_required",
"SystemPopup_fake_protection_disabled","SystemPopup_fake_firewall_off","SystemPopup_fake_update_needed",
"SystemPopup_fake_repair_tool","SystemPopup_fake_threat_blocked","SystemPopup_fake_security_popup",
"SystemPopup_fake_urgent_fix",

"AntivirusCheck_fake_virus_scan","AntivirusCheck_fake_threat_report","AntivirusCheck_fake_infection_log",
"AntivirusCheck_fake_cleanup_tool","AntivirusCheck_fake_shield_disabled","AntivirusCheck_fake_protection_low",
"AntivirusCheck_fake_scan_required","AntivirusCheck_fake_remove_now","AntivirusCheck_fake_risk_detected",
"AntivirusCheck_fake_critical_issue",

"MalwareWarning_fake_hacked_message","MalwareWarning_fake_remote_access","MalwareWarning_fake_data_compromise",
"MalwareWarning_fake_encryption_notice","MalwareWarning_fake_system_lock","MalwareWarning_fake_ransom_popup",
"MalwareWarning_fake_security_risk","MalwareWarning_fake_payload_detected","MalwareWarning_fake_unauthorised_access",
"MalwareWarning_fake_system_corrupt",

"PrizeCenter_fake_you_have_won","PrizeCenter_fake_claim_reward","PrizeCenter_fake_bonus_waiting",
"PrizeCenter_fake_spin_to_win","PrizeCenter_fake_jackpot_ready","PrizeCenter_fake_reward_pending",
"PrizeCenter_fake_click_to_claim","PrizeCenter_fake_final_step","PrizeCenter_fake_reward_unlocked",
"PrizeCenter_fake_offer_available",

"RewardHub_fake_click_here_now","RewardHub_fake_special_bonus","RewardHub_fake_daily_prize",
"RewardHub_fake_instant_win","RewardHub_fake_gift_waiting","RewardHub_fake_reward_screen",
"RewardHub_fake_claim_button","RewardHub_fake_limited_offer","RewardHub_fake_unlock_reward",
"RewardHub_fake_continue_to_win",

"WinScreen_fake_you_are_selected","WinScreen_fake_congratulations_user","WinScreen_fake_final_confirmation",
"WinScreen_fake_reward_ready","WinScreen_fake_press_ok_to_continue","WinScreen_fake_you_are_the_winner",
"WinScreen_fake_claim_in_10_seconds","WinScreen_fake_click_to_open","WinScreen_fake_bonus_unlocked",
"WinScreen_fake_reward_popup",

"SecurityScan_fake_system_failure","SecurityScan_fake_integrity_error","SecurityScan_fake_scan_result",
"SecurityScan_fake_threat_summary","SecurityScan_fake_critical_status","SecurityScan_fake_warning_screen",
"SecurityScan_fake_diagnostics","SecurityScan_fake_health_report","SecurityScan_fake_system_alert",
"SecurityScan_fake_protection_report",

"FirewallNotice_fake_intrusion_blocked","FirewallNotice_fake_port_attack","FirewallNotice_fake_network_risk",
"FirewallNotice_fake_connection_warning","FirewallNotice_fake_access_attempt","FirewallNotice_fake_remote_probe",
"FirewallNotice_fake_suspicious_traffic","FirewallNotice_fake_network_alert","FirewallNotice_fake_blocked_request",
"FirewallNotice_fake_security_event",

"ClickHere_fake_fix_now","ClickHere_fake_secure_device","ClickHere_fake_unlock_reward",
"ClickHere_fake_continue","ClickHere_fake_open_now","ClickHere_fake_verify_identity",
"ClickHere_fake_confirm_action","ClickHere_fake_start_cleanup","ClickHere_fake_start_scan",
"ClickHere_fake_claim_bonus",

"Jackpot_fake_spin_now","Jackpot_fake_bonus_round","Jackpot_fake_reward_drop","Jackpot_fake_lucky_user",
"Jackpot_fake_final_spin","Jackpot_fake_big_win","Jackpot_fake_claim_screen","Jackpot_fake_reward_popup",
"Jackpot_fake_click_to_spin","Jackpot_fake_instant_prize",

"ThreatMonitor_fake_hack_detected","ThreatMonitor_fake_data_leak","ThreatMonitor_fake_suspicious_login",
"ThreatMonitor_fake_password_compromise","ThreatMonitor_fake_security_flag","ThreatMonitor_fake_risk_alert",
"ThreatMonitor_fake_threat_log","ThreatMonitor_fake_event_detected","ThreatMonitor_fake_warning_popup",
"ThreatMonitor_fake_scan_required",

"SystemError_fake_critical_failure","SystemError_fake_blue_screen","SystemError_fake_kernel_issue",
"SystemError_fake_memory_corrupt","SystemError_fake_process_crash","SystemError_fake_system_halt",
"SystemError_fake_restart_required","SystemError_fake_repair_needed","SystemError_fake_system_lock",
"SystemError_fake_urgent_restart"
];

/* -------------------------
   TIER 5 — 750 (same theme, more spammy)
------------------------- */
const tier5 = [];
const tier5Prefixes = [
  "YourComputerIsHacked","FakeAntivirus","FakeMalwareScan","CriticalSystemAlert",
  "UrgentSecurityWarning","ThreatDetected","SystemCompromised","FakeFirewall",
  "FakeProtectionSuite","FakeCleanupTool","PrizeCenter","RewardHub","WinnerPage",
  "BonusPortal","LuckyWheel","JackpotZone","ClaimReward","InstantWin","DailyPrize",
  "ClickHereNow","UrgentFix","SecurityBreach","SystemLock","FakeRansomNotice"
];
const tier5Suffixes = [
  "clickhere","fixnow","securedevice","unlockreward","claimnow",
  "youvewon","finalstep","continue","openhere","verify",
  "scanrequired","cleanupneeded","threatfound","riskdetected","warning",
  "bonuspage","rewardcenter","spinagain","checkprize","viewreward"
];
for (let i = 0; i < 750; i++) {
  const pre = tier5Prefixes[i % tier5Prefixes.length];
  const suf = tier5Suffixes[i % tier5Suffixes.length];
  const id  = Math.random().toString(36).slice(2, 8);
  tier5.push(pre + "_" + id + "_" + suf);
}

/* -------------------------
   TIER 6 — 1250 (deep‑web ghost pages)
------------------------- */
const tier6 = [];
const tier6Prefixes = [
  "Abyss","DeepAbyss","Ghost","Shadow","Lost","Broken","Archive","Mirror",
  "Void","Null","Fragment","Unknown","Untitled","OldForum","Thread"
];
const tier6Suffixes = [
  "null","undefined","corrupt","voidfile","empty","missing","fragment",
  "glitch","looped","endless","unknown0","unknown1","unknown2",
  "untitled0","final0"
];
for (let i = 0; i < 1250; i++) {
  const pre = tier6Prefixes[i % tier6Prefixes.length];
  const suf = tier6Suffixes[i % tier6Suffixes.length];
  const id  = Math.random().toString(36).slice(2, 8);
  tier6.push(pre + "_" + id + "_" + suf);
}

/* -------------------------
   PICK TIER BY CLICKS
------------------------- */
function pickTier() {
  if (clicks < 250) return tier1;
  if (clicks < 500) return tier2;
  if (clicks < 750) return tier3;
  if (clicks < 1000) return tier4;
  if (clicks < 3000) return tier5;
  return tier6;
}

/* -------------------------
   CLICK HANDLER
------------------------- */
function clickAd() {
  clicks++;
  document.getElementById("clicks").textContent = clicks;

  const tier = pickTier();
  const choice = tier[Math.floor(Math.random() * tier.length)];

  document.getElementById("website").textContent = choice;
  document.getElementById("description").textContent = "Exploring: " + choice;

  addHistory(choice);
}

/* -------------------------
   HISTORY
------------------------- */
function addHistory(text) {
  const h = document.getElementById("history");
  const div = document.createElement("div");
  div.className = "entry";
  div.textContent = "#" + clicks + " → " + text;
  h.prepend(div);
  if (h.children.length > 60) h.removeChild(h.lastChild);
}
</script>

</body>
</html>
