<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Oman Gold Rate Today</title>
  <meta name="description" content="Daily gold rates in Oman. Live gold price per gram for 24K, 22K, 21K, 18K in OMR." />

  <style>
    :root {
      --gold: #c9a227;
      --dark: #111;
      --light: #f7f7f7;
    }

    body {
      margin: 0;
      font-family: Arial, Helvetica, sans-serif;
      background: var(--light);
      color: #222;
    }

    header {
      background: var(--dark);
      color: #fff;
      padding: 16px;
      text-align: center;
    }

    header h1 {
      margin: 0;
      font-size: 1.6rem;
      color: var(--gold);
    }

    header p {
      margin: 6px 0 0;
      font-size: 0.9rem;
      opacity: 0.9;
    }

    main {
      max-width: 900px;
      margin: 20px auto;
      padding: 0 12px;
    }

    .card {
      background: #fff;
      border-radius: 8px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.08);
      margin-bottom: 20px;
      overflow: hidden;
    }

    .card h2 {
      margin: 0;
      padding: 14px;
      background: var(--gold);
      color: #000;
      font-size: 1.1rem;
    }

    table {
      width: 100%;
      border-collapse: collapse;
    }

    th, td {
      padding: 12px;
      border-bottom: 1px solid #eee;
      text-align: center;
    }

    th {
      background: #fafafa;
      font-weight: bold;
    }

    tr:last-child td {
      border-bottom: none;
    }

    .updated {
      padding: 10px;
      font-size: 0.85rem;
      text-align: right;
      color: #555;
    }

    footer {
      background: #111;
      color: #ccc;
      text-align: center;
      padding: 14px;
      font-size: 0.85rem;
    }

    @media (max-width: 600px) {
      header h1 {
        font-size: 1.3rem;
      }
    }
  </style>
</head>

<body>

  <header>
    <h1 id="title">Oman Gold Rate Today</h1>
    <p id="subtitle">Latest gold price per gram in Oman (OMR)</p>
    <button onclick="setLang('en')">English</button>
    <button onclick="setLang('ar')">العربية</button>
  </header>

  <main>

    <div class="card">
      <h2>Today's Gold Rates</h2>
      <table>
        <thead>
          <tr>
            <th>Gold Purity</th>
            <th>Price (OMR / gram)</th>
          </tr>
        </thead>
        <tbody id="goldRates">
          <tr><td>24 Karat</td><td>--</td></tr>
          <tr><td>22 Karat</td><td>--</td></tr>
          <tr><td>21 Karat</td><td>--</td></tr>
          <tr><td>18 Karat</td><td>--</td></tr>
        </tbody>
      </table>
      <div class="updated" id="lastUpdated">Last updated: --</div>
    </div>

    <div class="card">
      <h2>Gold Rate Information</h2>
      <table>
        <tr><td>Currency</td><td>Omani Rial (OMR)</td></tr>
        <tr><td>Market</td><td>Oman</td></tr>
        <tr><td>Price Type</td><td>Per Gram</td></tr>
      </table>
    </div>

  </main>

  <footer>
    © <span id="year"></span> Oman Gold Rate | Informational website
  </footer>

  <script>
    const translations = {
      en: {
        title: 'Oman Gold Rate Today',
        subtitle: 'Latest gold price per gram in Oman (OMR)',
        updated: 'Last updated:'
      },
      ar: {
        title: 'سعر الذهب اليوم في عمان',
        subtitle: 'آخر سعر للذهب بالجرام في سلطنة عمان',
        updated: 'آخر تحديث:'
      }
    };

    function setLang(lang) {
      document.documentElement.lang = lang;
      document.body.dir = lang === 'ar' ? 'rtl' : 'ltr';
      document.getElementById('title').textContent = translations[lang].title;
      document.getElementById('subtitle').textContent = translations[lang].subtitle;
    }

    setLang('en');

    async function loadGoldRates() {
      try {
        const response = await fetch('https://www.goldapi.io/api/XAU/OMR', {
          headers: {
            'x-access-token': 'YOUR_API_KEY',
            'Content-Type': 'application/json'
          }
        });

        const data = await response.json();
        const price24 = data.price / 31.1035;
        const price22 = price24 * (22 / 24);
        const price21 = price24 * (21 / 24);
        const price18 = price24 * (18 / 24);

        const rows = document.querySelectorAll('#goldRates tr');
        rows[0].children[1].textContent = price24.toFixed(3);
        rows[1].children[1].textContent = price22.toFixed(3);
        rows[2].children[1].textContent = price21.toFixed(3);
        rows[3].children[1].textContent = price18.toFixed(3);

        document.getElementById('lastUpdated').textContent =
          (document.documentElement.lang === 'ar'
            ? translations.ar.updated
            : translations.en.updated) + ' ' + new Date().toLocaleString();
      } catch (e) {
        document.getElementById('lastUpdated').textContent = 'Error loading data';
      }
    }

    loadGoldRates();

    // Auto refresh every 15 minutes (900000 ms)
    setInterval(loadGoldRates, 900000);
    document.getElementById('year').textContent = new Date().getFullYear();
  </script>
</body>
</html>
