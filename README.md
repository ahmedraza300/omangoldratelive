<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Oman Gold Rate Live</title>
  <meta name="description" content="Live gold prices in Oman. 24K, 22K, 21K gold rates updated automatically.">
  <style>
    :root { --gold: #c9a227; --dark: #111; --light: #f7f7f7; }
    body { margin:0; font-family:Arial,sans-serif; background:var(--light); color:#222; }
    header { background:var(--dark); color:#fff; padding:16px; text-align:center; }
    header h1 { margin:0; font-size:1.6rem; color:var(--gold); }
    header p { margin:6px 0 0; font-size:0.9rem; opacity:0.9; }
    header button { margin:0 5px; padding:5px 10px; border:none; border-radius:4px; cursor:pointer; }
    main { max-width:900px; margin:20px auto; padding:0 12px; text-align:center; }
    footer { background:#111; color:#ccc; text-align:center; padding:14px; font-size:0.85rem; }
    @media(max-width:600px){header h1{font-size:1.3rem;}}
  </style>
</head>
<body>
  <header>
    <h1 id="title">Oman Gold Rate Live</h1>
    <p id="subtitle">Latest gold price per gram in Oman (OMR)</p>
    <button onclick="setLang('en')">English</button>
    <button onclick="setLang('ar')">العربية</button>
  </header>
  <main>
    <!-- Official GoldPriceOne Widget Script -->
    <div id="goldprice-widget" data-coins="XAU24,XAU22,XAU21" data-currency="OMR" data-theme="auto"></div>
    <script src="https://widget.goldpriceone.app/goldpriceone-widget.js"></script>
  </main>
  <footer>© <span id="year"></span> Oman Gold Rate Live</footer>
  <script>
    const translations = {
      en: { title:'Oman Gold Rate Live', subtitle:'Latest gold price per gram in Oman (OMR)' },
      ar: { title:'سعر الذهب اليوم في عمان', subtitle:'آخر سعر للذهب بالجرام في سلطنة عمان' }
    };

    function setLang(lang){
      document.documentElement.lang = lang;
      document.body.dir = lang==='ar'?'rtl':'ltr';
      document.getElementById('title').textContent = translations[lang].title;
      document.getElementById('subtitle').textContent = translations[lang].subtitle;
    }

    setLang('en');
    document.getElementById('year').textContent = new Date().getFullYear();
  </script>
</body>
</html>
