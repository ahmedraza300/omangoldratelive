<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Oman Gold Rate Live</title>
<style>
:root {
  --gold:#c9a227;
  --dark:#111;
  --light:#f7f7f7;
}
body { margin:0; font-family: Arial,sans-serif; background: var(--light); color:#222;}
header {background: var(--dark); color:#fff; padding:16px; text-align:center;}
header h1 {margin:0;font-size:1.7rem;color: var(--gold); font-weight:bold;}
header button {margin:0 5px; padding:6px 12px; border:none; border-radius:4px; cursor:pointer; background:#222; color:#fff; font-weight:bold;}
main { max-width:960px; margin:20px auto; padding:0 12px;}
.card { background:#fff; border-radius:12px; box-shadow:0 6px 16px rgba(0,0,0,0.18); overflow:hidden; margin-bottom:24px; padding:12px;}
.card h2 {margin:0; padding:16px; background: var(--gold); color:#000; text-align:center; font-size:1.25rem; border-radius:12px 12px 0 0; font-weight:bold;}
iframe { width:100%; height:350px; border:none;}
.last-updated { text-align:right; padding:8px 0; font-size:0.9rem; color:#555;}
table { width:100%; border-collapse:collapse; text-align:center; margin-top:8px;}
th, td { padding:12px; border-bottom:1px solid #eee;}
th { background:#fafafa; font-weight:bold;}
footer { background: var(--dark); color:#ccc; text-align:center; padding:16px; font-size:0.85rem;}
@media(max-width:600px){
header h1{font-size:1.4rem;}
header button{padding:4px 8px;font-size:0.85rem;}
th, td{font-size:0.85rem;}
.last-updated{font-size:0.8rem;}
}
</style>
</head>
<body>

<header>
<h1 id="title">Oman Gold Rate Live</h1><br>
<button onclick="setLang('en')">English</button>
<button onclick="setLang('ar')">العربية</button>
</header>

<main>
<div class="card">
<h2 id="widgetTitle">Live Gold Prices</h2>
<iframe src="https://widget.goldpriceone.app/goldpriceone-widget.html?currency=OMR&lang=en"></iframe>

<div class="last-updated" id="lastUpdated">Last Updated: -- mins ago</div>

<table>
<thead>
<tr>
<th id="purityHeader">Gold Purity</th>
<th id="buyHeader">Buy Price (OMR/g)</th>
<th id="sellHeader">Sell Price (OMR/g)</th>
</tr>
</thead>
<tbody id="ratesBody">
<tr><td>24K</td><td>--</td><td>--</td></tr>
<tr><td>22K</td><td>--</td><td>--</td></tr>
<tr><td>21K</td><td>--</td><td>--</td></tr>
<tr><td>18K</td><td>--</td><td>--</td></tr>
<tr><td>8g Gini (21K)</td><td colspan="2">--</td></tr>
</tbody>
</table>
</div>
</main>

<footer>© <span id="year"></span> Oman Gold Rate Live</footer>

<script>
const translations = {
en:{title:"Oman Gold Rate Live", widgetTitle:"Live Gold Prices", purityHeader:"Gold Purity", buyHeader:"Buy Price (OMR/g)", sellHeader:"Sell Price (OMR/g)", lastUpdated:"Last Updated"},
ar:{title:"سعر الذهب اليوم في عمان", widgetTitle:"أسعار الذهب اليوم", purityHeader:"عيار الذهب", buyHeader:"سعر الشراء (ر.ع/جرام)", sellHeader:"سعر البيع (ر.ع/جرام)", lastUpdated:"آخر تحديث"}
};

function setLang(lang){
  document.documentElement.lang = lang;
  document.body.dir = lang==='ar'?'rtl':'ltr';
  document.getElementById('title').textContent = translations[lang].title;
  document.getElementById('widgetTitle').textContent = translations[lang].widgetTitle;
  document.getElementById('purityHeader').textContent = translations[lang].purityHeader;
  document.getElementById('buyHeader').textContent = translations[lang].buyHeader;
  document.getElementById('sellHeader').textContent = translations[lang].sellHeader;
  updateLastUpdated();
}

// Update last updated time every minute
function updateLastUpdated(){
  const updatedTime = new Date(localStorage.getItem('lastUpdated') || Date.now());
  function tick(){
    const minsAgo = Math.floor((Date.now() - updatedTime)/60000);
    document.getElementById('lastUpdated').textContent = (document.documentElement.lang==='ar'?translations.ar.lastUpdated:translations.en.lastUpdated) + ": " + minsAgo + " mins ago";
  }
  tick();
  setInterval(tick,60000);
}

// Function to fetch live prices from Metal API using your key via free proxy
async function fetchLivePrices(){
  try{
    const apiKey = "a09fa9c3db43507a67d991ea9c39c871"; // your key
    const proxy = "https://api.allorigins.win/get?url=";
    const url = encodeURIComponent("https://metalpriceapi.com/api/XAU/OMR?api_key="+apiKey);
    const res = await fetch(proxy+url);
    const text = await res.json();
    const data = JSON.parse(text.contents);

    const price24 = parseFloat(data['XAU']['OMR']['24K']);
    const price22 = parseFloat(data['XAU']['OMR']['22K']);
    const price21 = parseFloat(data['XAU']['OMR']['21K']);
    const price18 = parseFloat(data['XAU']['OMR']['18K']);

    const sell = (p)=> (p*1.02).toFixed(2);

    // Update table
    const rows = document.getElementById('ratesBody').rows;
    rows[0].cells[1].textContent = price24.toFixed(2);
    rows[0].cells[2].textContent = sell(price24);
    rows[1].cells[1].textContent = price22.toFixed(2);
    rows[1].cells[2].textContent = sell(price22);
    rows[2].cells[1].textContent = price21.toFixed(2);
    rows[2].cells[2].textContent = sell(price21);
    rows[3].cells[1].textContent = price18.toFixed(2);
    rows[3].cells[2].textContent = sell(price18);
    rows[4].cells[1].colSpan = 2;
    rows[4].cells[1].textContent = (price21*8).toFixed(2); // 8g Gini

    localStorage.setItem('lastUpdated', Date.now());
  } catch(e){
    console.error("Failed to fetch live prices:",e);
  }
}

fetchLivePrices();
setLang('en');
document.getElementById('year').textContent = new Date().getFullYear();

// Auto refresh every 15 minutes
setInterval(fetchLivePrices,15*60*1000);

</script>
</body>
</html>

