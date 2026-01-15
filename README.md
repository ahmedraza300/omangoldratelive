<!Raza>
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
  --card:#fff;
  --text:#222;
}
body {
  margin:0;
  font-family: Arial,sans-serif;
  background: var(--light);
  color: var(--text);
}
header {
  background: var(--dark);
  color: #fff;
  padding: 16px;
  text-align:center;
}
header h1 {
  margin:0;
  font-size:1.6rem;
  color: var(--gold);
}
header button {
  margin:0 5px;
  padding:6px 12px;
  border:none;
  border-radius:4px;
  cursor:pointer;
  background:#222;
  color:#fff;
}
main {
  max-width:900px;
  margin:20px auto;
  padding:0 12px;
}
.card {
  background: var(--card);
  border-radius:10px;
  box-shadow:0 4px 12px rgba(0,0,0,0.12);
  padding:12px;
  overflow:hidden;
  margin-bottom:20px;
}
.card h2 {
  margin:0;
  padding:14px;
  background: var(--gold);
  color:#000;
  font-size:1.2rem;
  text-align:center;
  border-radius:8px 8px 0 0;
}
table {
  width:100%;
  border-collapse:collapse;
  text-align:center;
}
th, td {
  padding:12px;
  border-bottom:1px solid #eee;
}
th {
  background:#fafafa;
  font-weight:bold;
}
.updated {
  padding:10px;
  font-size:0.85rem;
  text-align:right;
  color:#555;
}
footer {
  background: var(--dark);
  color:#ccc;
  text-align:center;
  padding:14px;
  font-size:0.85rem;
}
@media(max-width:600px){
  header h1{font-size:1.3rem;}
  header button{padding:4px 8px;font-size:0.85rem;}
  th, td{font-size:0.85rem;}
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
<h2 id="ratesTitle">Today's Gold Rates</h2>
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
<div class="updated" id="lastUpdated">Last updated: --</div>
</div>
</main>

<footer>© <span id="year"></span> Oman Gold Rate Live</footer>

<script>
// Translations
const translations = {
en:{title:"Oman Gold Rate Live", ratesTitle:"Today's Gold Rates", purityHeader:"Gold Purity", buyHeader:"Buy Price (OMR/g)", sellHeader:"Sell Price (OMR/g)", updatedText:"Last updated:"},
ar:{title:"سعر الذهب اليوم في عمان", ratesTitle:"أسعار الذهب اليوم", purityHeader:"عيار الذهب", buyHeader:"سعر الشراء (ر.ع/جرام)", sellHeader:"سعر البيع (ر.ع/جرام)", updatedText:"آخر تحديث:"}
};

function setLang(lang){
  document.documentElement.lang = lang;
  document.body.dir = lang==='ar'?'rtl':'ltr';
  document.getElementById('title').textContent = translations[lang].title;
  document.getElementById('ratesTitle').textContent = translations[lang].ratesTitle;
  document.querySelector('th:nth-child(1)').textContent = translations[lang].purityHeader;
  document.querySelector('th:nth-child(2)').textContent = translations[lang].buyHeader;
  document.querySelector('th:nth-child(3)').textContent = translations[lang].sellHeader;
  loadRates();
}

async function loadRates(){
  try{
    // Public proxy fetch for live XAU to OMR price
    const res = await fetch("https://api.allorigins.win/raw?url=https://www.goldapi.io/api/XAU/OMR");
    const data = await res.json();
    if(!data.price) return;

    const pricePerGram = data.price / 31.1035; // price per gram

    const rates = [
      {karat:24,mult:1},
      {karat:22,mult:22/24},
      {karat:21,mult:21/24},
      {karat:18,mult:18/24}
    ];

    rates.forEach((r,i)=>{
      const buy = (pricePerGram*r.mult).toFixed(3);
      const sell = (pricePerGram*r.mult*1.02).toFixed(3);
      document.getElementById('ratesBody').rows[i].cells[1].textContent = buy;
      document.getElementById('ratesBody').rows[i].cells[2].textContent = sell;
    });

    // 8g Gini from 21K
    const giniPrice = (pricePerGram*(21/24)*8).toFixed(3);
    document.getElementById('ratesBody').rows[4].cells[1].textContent = giniPrice;
    document.getElementById('ratesBody').rows[4].cells[2].textContent = giniPrice;

    document.getElementById('lastUpdated').textContent = (document.documentElement.lang==='ar'?translations.ar.updatedText:translations.en.updatedText)+' '+new Date().toLocaleString();

  } catch(e){console.error("Error fetching gold rates:", e);}
}

// Initialize
setLang('en');
loadRates();
setInterval(loadRates,900000); // every 15 minutes
document.getElementById('year').textContent = new Date().getFullYear();
</script>

</body>
</html>

