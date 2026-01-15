<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Oman Gold Rate Live</title>
<style>
:root { --gold:#c9a227; --dark:#111; --light:#f7f7f7; }
body { margin:0; font-family:Arial,sans-serif; background:var(--light); color:#222; }
header { background:var(--dark); color:#fff; padding:16px; text-align:center; }
header h1 { margin:0; font-size:1.6rem; color:var(--gold); }
main { max-width:900px; margin:20px auto; padding:0 12px; }
.card { background:#fff; border-radius:8px; box-shadow:0 2px 8px rgba(0,0,0,0.08); margin-bottom:16px; overflow:hidden; text-align:center;}
.card h2 { margin:0; padding:14px; background:var(--gold); color:#000; font-size:1.1rem; }
iframe { width:100%; height:300px; border:none; }
footer { background:#111; color:#ccc; text-align:center; padding:14px; font-size:0.85rem; }
</style>
</head>
<body>

<header>
<h1>Oman Gold Rate Live</h1>
</header>

<main>
<div class="card">
<h2>Today's Gold Rates</h2>
<!-- Live Gold Price Widget -->
<iframe src="https://www.goldprice.org/widget/live-gold-price.html?lang=en&currency=OMR"></iframe>
</div>
</main>

<footer>© <span id="year"></span> Oman Gold Rate Live</footer>

<script>
document.getElementById('year').textContent = new Date().getFullYear();
</script>

</body>
</html>

