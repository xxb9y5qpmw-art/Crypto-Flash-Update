# Crypto-Flash-Update# 🚨 Crypto Price Alert App

A simple web-based Crypto Price Alert application that notifies users when a selected cryptocurrency reaches a target price using live market data.

Built with **HTML, CSS, and JavaScript**, and powered by the **CoinGecko API**.

---

## 🔥 Features

- 📊 Live crypto prices (Bitcoin, Ethereum, Solana)
- 🚨 Price alerts when a target is reached
- 🔔 Browser notifications
- 📈 Live price display
- ⏱ Auto-refresh every 30 seconds
- 🌙 Clean dark-mode UI
- 💻 No backend required

---

## 🛠️ Technologies Used

- HTML5
- CSS3
- JavaScript (ES6)
- CoinGecko Public API

---

## 🚀 Live Demo
(Enable GitHub Pages to add your link here)


---

# 🚀 2️⃣ ADVANCED FEATURES (UPGRADED APP)

### ✅ New Features Added
✔ Live price display  
✔ Browser notifications  
✔ Auto-updating UI  
✔ Error handling  
✔ Professional UX

---

## 🔁 Updated `index.html`
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Crypto Price Alert 🚨</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>

  <h1>Crypto Price Alert 🚨</h1>

  <select id="crypto">
    <option value="bitcoin">Bitcoin</option>
    <option value="ethereum">Ethereum</option>
    <option value="solana">Solana</option>
  </select>

  <input type="number" id="price" placeholder="Target price (USD)">

  <button onclick="setAlert()">Set Alert</button>

  <h3 id="livePrice">Current Price: --</h3>
  <p id="status"></p>

  <script src="script.js"></script>
</body>
</html>
body {
  font-family: Arial, sans-serif;
  background: #020617;
  color: white;
  text-align: center;
  padding: 40px;
}

input, select, button {
  padding: 12px;
  margin: 10px;
  border-radius: 6px;
  border: none;
  font-size: 16px;
}

button {
  background: #22c55e;
  font-weight: bold;
  cursor: pointer;
}

button:hover {
  background: #16a34a;
}

#livePrice {
  margin-top: 20px;
  color: #38bdf8;
}

let alertSet = false;
let targetPrice = 0;
let selectedCrypto = "";

if ("Notification" in window) {
  Notification.requestPermission();
}

async function setAlert() {
  selectedCrypto = document.getElementById("crypto").value;
  targetPrice = Number(document.getElementById("price").value);

  if (!targetPrice || targetPrice <= 0) {
    alert("Enter a valid price");
    return;
  }

  alertSet = true;
  document.getElementById("status").textContent =
    `Alert set for ${selectedCrypto.toUpperCase()} at $${targetPrice}`;

  checkPrice();
}

async function checkPrice() {
  const url = `https://api.coingecko.com/api/v3/simple/price?ids=${selectedCrypto}&vs_currencies=usd`;

  try {
    const response = await fetch(url);
    const data = await response.json();
    const currentPrice = data[selectedCrypto].usd;

    document.getElementById("livePrice").textContent =
      `Current Price: $${currentPrice}`;

    if (alertSet && currentPrice >= targetPrice) {
      notifyUser(currentPrice);
      alertSet = false;
      document.getElementById("status").textContent = "Alert triggered!";
    }

    setTimeout(checkPrice, 30000);

  } catch (error) {
    console.error("Price fetch error:", error);
  }
}

function notifyUser(price) {
  if (Notification.permission === "granted") {
    new Notification("🚨 Crypto Alert", {
      body: `${selectedCrypto.toUpperCase()} hit $${price}`
    });
  } else {
    alert(`${selectedCrypto.toUpperCase()} hit $${price}`);
  }
}

checkPrice();
{
  "name": "Crypto Price Alert",
  "short_name": "CryptoAlert",
  "start_url": ".",
  "display": "standalone",
  "background_color": "#020617",
  "theme_color": "#22c55e",
  "icons": [
    {
      "src": "icon.png",
      "sizes": "192x192",
      "type": "image/png"
    }
  ]
}
<link rel="manifest" href="manifest.json">
<meta name="theme-color" content="#22c55e">
npm install @capacitor/core @capacitor/cli
npx cap init
npm install @capacitor/android
npx cap add android
npx cap open android
<h1>📊 Crypto Dashboard</h1>

<div id="dashboard"></div>

<input type="number" id="price" placeholder="Alert price">
<button onclick="setAlert()">Set Alert</button>

<p id="status"></p>
#dashboard {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 15px;
  margin-top: 30px;
}

.card {
  background: #0f172a;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 0 10px #000;
}

.price {
  font-size: 22px;
  color: #38bdf8;
}

.positive {
  color: #22c55e;
}

.negative {
  color: #ef4444;
}
const coins = ["bitcoin", "ethereum", "solana"];
let selectedCrypto = "bitcoin";
let targetPrice = 0;
let alertSet = false;

async function loadDashboard() {
  const url = `https://api.coingecko.com/api/v3/simple/price?ids=${coins.join(",")}&vs_currencies=usd&include_24hr_change=true`;
  const res = await fetch(url);
  const data = await res.json();

  const dashboard = document.getElementById("dashboard");
  dashboard.innerHTML = "";

  coins.forEach(coin => {
    const price = data[coin].usd;
    const change = data[coin].usd_24h_change.toFixed(2);
    const cls = change >= 0 ? "positive" : "negative";

    dashboard.innerHTML += `
      <div class="card" onclick="selectCoin('${coin}')">
        <h3>${coin.toUpperCase()}</h3>
        <p class="price">$${price}</p>
        <p class="${cls}">${change}% (24h)</p>
      </div>
    `;
  });
}

function selectCoin(coin) {
  selectedCrypto = coin;
  document.getElementById("status").textContent =
    `Selected ${coin.toUpperCase()}`;
}

function setAlert() {
  targetPrice = Number(document.getElementById("price").value);
  if (!targetPrice) return alert("Enter a valid price");

  alertSet = true;
  checkAlert();
}

async function checkAlert() {let chart;

async function loadChart(coin) {
  const url = `https://api.coingecko.com/api/v3/coins/${coin}/market_chart?vs_currency=usd&days=1`;
  const res = await fetch(url);
  const data = await res.json();

  const prices = data.prices.map(p => p[1]);
  const labels = data.prices.map((_, i) => i);

  if (chart) chart.destroy();

  chart = new Chart(document.getElementById("chart"), {
    type: "line",
    data: {
      labels,
      datasets: [{
        label: coin.toUpperCase(),
        data: prices,
        borderWidth: 2,
        fill: false
      }]
    }
  });
}
<canvas id="chart"></canvas>

  if (!alertSet) return;

  const url = `https://api.coingecko.com/api/v3/simple/price?ids=${selectedCrypto}&vs_currencies=usd`;
  const res = await fetch(url);
  const data = await res.json();

  if (data[selectedCrypto].usd >= targetPrice) {
    alert(`🚨 ${selectedCrypto.toUpperCase()} hit $${targetPrice}`);
    alertSet = false;
  } else {
    setTimeout(checkAlert, 30000);
  }
}

loadDashboard();
setInterval(loadDashboard, 30000);
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-auth-compat.js"></script>
const firebaseConfig = {
  apiKey: "YOUR_KEY",
  authDomain: "YOUR_DOMAIN",
  projectId: "YOUR_PROJECT_ID",
};

firebase.initializeApp(firebaseConfig);
const auth = firebase.auth();
function signup(email, password) {
  auth.createUserWithEmailAndPassword(email, password)
    .then(() => showApp())
    .catch(err => alert(err.message));
}

function login(email, password) {
  auth.signInWithEmailAndPassword(email, password)
    .then(() => showApp())
    .catch(err => alert(err.message));
}

function logout() {
  auth.signOut();
}

auth.onAuthStateChanged(user => {
  if (user) showApp();
});
<div id="login">
  <input id="email" placeholder="Email">
  <input id="password" type="password" placeholder="Password">
  <button onclick="login(email.value, password.value)">Login</button>
  <button onclick="signup(email.value, password.value)">Sign Up</button>
</div>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>loadChart("bitcoin");
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }

    match /alerts/{alertId} {
      allow read, write: if request.auth != null
        && request.auth.uid == resource.data.userId;
    }
  }
}
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-firestore-compat.js"></script>
const db = firebase.firestore();
auth.onAuthStateChanged(async user => {
  if (!user) return;

  const userRef = db.collection("users").doc(user.uid);
  const doc = await userRef.get();

  if (!doc.exists) {
    await userRef.set({
      email: user.email,
      createdAt: firebase.firestore.FieldValue.serverTimestamp(),
      plan: "free"
    });
  }

  showApp();
});
async function saveAlert(coin, price) {
  const user = auth.currentUser;
  if (!user) return;

  await db.collection("alerts").add({
    userId: user.uid,
    coin,
    targetPrice: price,
    active: true,
    createdAt: firebase.firestore.FieldValue.serverTimestamp()
  });

  document.getElementById("status").textContent = "Alert saved ☁️";
}
saveAlert(selectedCrypto, targetPrice);
async function loadAlerts() {
  const user = auth.currentUser;
  if (!user) return;

  const snapshot = await db
    .collection("alerts")
    .where("userId", "==", user.uid)
    .where("active", "==", true)
    .get();

  snapshot.forEach(doc => {
    const alert = doc.data();
    console.log(alert.coin, alert.targetPrice);
  });
}
async function triggerAlert(alertId) {
  await db.collection("alerts").doc(alertId).update({
    active: false,
    triggeredAt: firebase.firestore.FieldValue.serverTimestamp()
  });
}
users/
  └── uid
      ├── email
      ├── plan
      └── createdAt

alerts/
  └── alertId
      ├── userId
      ├── coin
      ├── targetPrice
      ├── active
      ├── createdAt

alerts/
  └── alertId
      ├── userId
      ├── coin
      ├── type            // "price" OR "percent"
      ├── targetPrice     // for price alerts
      ├── percentChange   // for smart alerts (e.g. 5)
      ├── basePrice       // price when alert was created
      ├── direction       // "up", "down", or "both"
      ├── active
      ├── createdAt
<select id="alertType">
  <option value="price">Price Alert</option>
  <option value="percent">Percent Change Alert</option>
</select>

<input type="number" id="percent" placeholder="Percent change (e.g. 5)">
<select id="direction">
  <option value="both">Up or Down</option>
  <option value="up">Up Only</option>
  <option value="down">Down Only</option>
</select>
async function saveSmartAlert(coin, percent, direction) {
  const user = auth.currentUser;
  if (!user) return;

  // Get current price to use as baseline
  const priceRes = await fetch(
    `https://api.coingecko.com/api/v3/simple/price?ids=${coin}&vs_currencies=usd`
  );
  const priceData = await priceRes.json();
  const basePrice = priceData[coin].usd;

  await db.collection("alerts").add({
    userId: user.uid,
    coin,
    type: "percent",
    percentChange: percent,
    direction,
    basePrice,
    active: true,
    createdAt: firebase.firestore.FieldValue.serverTimestamp()
  });

  document.getElementById("status").textContent =
    `Smart alert set for ${coin.toUpperCase()} (${percent}%)`;
}
async function checkSmartAlerts() {
  const user = auth.currentUser;
  if (!user) return;

  const snapshot = await db
    .collection("alerts")
    .where("userId", "==", user.uid)
    .where("active", "==", true)
    .where("type", "==", "percent")
    .get();

  for (const doc of snapshot.docs) {
    const alert = doc.data();

    const res = await fetch(
      `https://api.coingecko.com/api/v3/simple/price?ids=${alert.coin}&vs_currencies=usd`
    );
    const data = await res.json();
    const currentPrice = data[alert.coin].usd;

    const percentMove =
      ((currentPrice - alert.basePrice) / alert.basePrice) * 100;

    const hitUp =
      percentMove >= alert.percentChange && alert.direction !== "down";

    const hitDown =
      percentMove <= -alert.percentChange && alert.direction !== "up";

    if (hitUp || hitDown) {
      triggerSmartAlert(doc.id, alert.coin, percentMove, currentPrice);
    }
  }
}
async function triggerSmartAlert(alertId, coin, percentMove, price) {
  await db.collection("alerts").doc(alertId).update({
    active: false,
    triggeredAt: firebase.firestore.FieldValue.serverTimestamp()
  });

  const message =
    `${coin.toUpperCase()} moved ${percentMove.toFixed(2)}% to $${price}`;

  if (Notification.permission === "granted") {
    new Notification("🚨 Smart Crypto Alert", { body: message });
  } else {
    alert(message);
  }
}
setInterval(checkSmartAlerts, 60000); // every 60 seconds
watchlists/
  └── docId
      ├── userId
      ├── coin
      ├── addedAt
async function addToWatchlist(coin) {
  const user = auth.currentUser;
  if (!user) return;

  await db.collection("watchlists").add({
    userId: user.uid,
    coin,
    addedAt: firebase.firestore.FieldValue.serverTimestamp()
  });

  alert(`${coin.toUpperCase()} added to watchlist ⭐`);
}
async function loadWatchlist() {
  const user = auth.currentUser;
  if (!user) return;

  const snapshot = await db
    .collection("watchlists")
    .where("userId", "==", user.uid)
    .get();

  const coins = snapshot.docs.map(doc => doc.data().coin);
  loadWatchlistAnalytics(coins);
}
async function loadWatchlistAnalytics(coins) {
  if (coins.length === 0) return;

  const url = `https://api.coingecko.com/api/v3/simple/price?ids=${coins.join(",")}&vs_currencies=usd&include_24hr_change=true`;
  const res = await fetch(url);
  const data = await res.json();

  const container = document.getElementById("watchlist");
  container.innerHTML = "";

  coins.forEach(coin => {
    const price = data[coin].usd;
    const change = data[coin].usd_24h_change.toFixed(2);
    const cls = change >= 0 ? "positive" : "negative";

    container.innerHTML += `
      <div class="card">
        <h3>${coin.toUpperCase()}</h3>
        <p>$${price}</p>
        <p class="${cls}">${change}% (24h)</p>
      </div>
    `;
  });
}
<h2>⭐ Watchlist</h2>
<div id="watchlist"></div>
async function dailySummary() {
  const user = auth.currentUser;
  if (!user) return;

  const snapshot = await db
    .collection("watchlists")
    .where("userId", "==", user.uid)
    .get();

  if (snapshot.empty) return;

  const coins = snapshot.docs.map(doc => doc.data().coin);

  const url = `https://api.coingecko.com/api/v3/simple/price?ids=${coins.join(",")}&vs_currencies=usd&include_24hr_change=true`;
  const res = await fetch(url);
  const data = await res.json();

  let topGainer = null;
  let biggestDrop = null;

  coins.forEach(coin => {
    const change = data[coin].usd_24h_change;

    if (!topGainer || change > topGainer.change)
      topGainer = { coin, change };

    if (!biggestDrop || change < biggestDrop.change)
      biggestDrop = { coin, change };
  });

  sendDailyNotification(topGainer, biggestDrop);
}
function sendDailyNotification(top, drop) {
  const message =
    `📊 Daily Crypto Summary\n` +
    `🚀 Top Gainer: ${top.coin.toUpperCase()} (${top.change.toFixed(2)}%)\n` +
    `📉 Biggest Drop: ${drop.coin.toUpperCase()} (${drop.change.toFixed(2)}%)`;

  if (Notification.permission === "granted") {
    new Notification("Daily Crypto Summary", { body: message });
  }
}
const lastRun = localStorage.getItem("lastSummary");

if (!lastRun || Date.now() - lastRun > 86400000) {
  dailySummary();
  localStorage.setItem("lastSummary", Date.now());
}
portfolios/
  └── docId
      ├── userId
      ├── coin
      ├── amount        // e.g. 0.25 BTC
      ├── addedAt
async function addHolding(coin, amount) {
  const user = auth.currentUser;
  if (!user) return;

  await db.collection("portfolios").add({
    userId: user.uid,
    coin,
    amount: Number(amount),
    addedAt: firebase.firestore.FieldValue.serverTimestamp()
  });

  alert("Holding added to portfolio 📊");
}
async function loadPortfolio() {
  const user = auth.currentUser;
  if (!user) return;

  const snapshot = await db
    .collection("portfolios")
    .where("userId", "==", user.uid)
    .get();

  const holdings = snapshot.docs.map(doc => doc.data());
  calculatePortfolioValue(holdings);
}
async function calculatePortfolioValue(holdings) {
  const coins = holdings.map(h => h.coin);

  const url = `https://api.coingecko.com/api/v3/simple/price?ids=${coins.join(",")}&vs_currencies=usd&include_24hr_change=true`;
  const res = await fetch(url);
  const prices = await res.json();

  let totalValue = 0;
  const container = document.getElementById("portfolio");
  container.innerHTML = "";

  holdings.forEach(h => {
    const price = prices[h.coin].usd;
    const value = price * h.amount;
    totalValue += value;

    container.innerHTML += `
      <div class="card">
        <h3>${h.coin.toUpperCase()}</h3>
        <p>Holdings: ${h.amount}</p>
        <p>Value: $${value.toFixed(2)}</p>
      </div>
    `;
  });

  document.getElementById("totalValue").textContent =
    `Total Portfolio Value: $${totalValue.toFixed(2)}`;
}
<h2>📊 Portfolio</h2>
<div id="portfolio"></div>
<h3 id="totalValue"></h3>
function buildInsightData(holdings, prices) {
  return holdings.map(h => ({
    coin: h.coin,
    value: prices[h.coin].usd * h.amount,
    change24h: prices[h.coin].usd_24h_change
  }));
}
function generateInsights(data) {
  const total = data.reduce((sum, d) => sum + d.value, 0);

  const insights = [];

  data.forEach(d => {
    const percent = ((d.value / total) * 100).toFixed(1);

    if (percent > 50) {
      insights.push(
        `${d.coin.toUpperCase()} makes up ${percent}% of your portfolio, meaning your value is highly influenced by this asset.`
      );
    }

    if (Math.abs(d.change24h) > 8) {
      insights.push(
        `${d.coin.toUpperCase()} showed high volatility today (${d.change24h.toFixed(2)}%).`
      );
    }
  });

  if (insights.length === 0) {
    insights.push("Your portfolio is relatively balanced with no extreme movements today.");
  }

  displayInsights(insights);
}
function displayInsights(insights) {
  const container = document.getElementById("insights");
  container.innerHTML = "";

  insights.forEach(text => {
    container.innerHTML += `<p>🤖 ${text}</p>`;
  });
}
const insightData = buildInsightData(holdings, prices);
generateInsights(insightData);
<h2>🤖 AI Insights</h2>
<div id="insights"></div>
function buildDailySummaryData(holdings, prices) {
  let totalValue = 0;
  let topCoin = null;
  let best = null;
  let worst = null;
  let volatilityCount = 0;

  holdings.forEach(h => {
    const price = prices[h.coin].usd;
    const change = prices[h.coin].usd_24h_change;
    const value = price * h.amount;
    totalValue += value;

    if (!topCoin || value > topCoin.value) {
      topCoin = { coin: h.coin, value };
    }

    if (!best || change > best.change) {
      best = { coin: h.coin, change };
    }

    if (!worst || change < worst.change) {
      worst = { coin: h.coin, change };
    }

    if (Math.abs(change) > 7) volatilityCount++;
  });

  return {
    totalValue,
    topCoin,
    best,
    worst,
    volatilityLevel:
      volatilityCount === 0 ? "low" :
      volatilityCount <= 2 ? "moderate" : "high"
  };
}
function generateDailySummary(data) {
  const { totalValue, topCoin, best, worst, volatilityLevel } = data;

  let summary = `📊 Daily Crypto Summary\n\n`;

  summary += `Your portfolio is currently valued at $${totalValue.toFixed(2)}. `;
  summary += `${topCoin.coin.toUpperCase()} represents your largest holding today.\n\n`;

  if (best.change > 0) {
    summary += `🚀 ${best.coin.toUpperCase()} was the strongest performer, rising ${best.change.toFixed(2)}% over the past 24 hours. `;
  }

  if (worst.change < 0) {
    summary += `${worst.coin.toUpperCase()} experienced the largest decline at ${worst.change.toFixed(2)}%.\n\n`;
  }

  summary += `📈 Market activity showed ${volatilityLevel} volatility across your tracked assets.`;

  return summary;
}
<h2>🧠 Daily AI Report</h2>
<div id="dailyReport"></div>
function displayDailyReport(text) {
  document.getElementById("dailyReport").textContent = text;
}
async function runDailyAIReport(holdings, prices) {
  const lastRun = localStorage.getItem("lastAIReport");

  if (!lastRun || Date.now() - lastRun > 86400000) {
    const data = buildDailySummaryData(holdings, prices);
    const report = generateDailySummary(data);

    displayDailyReport(report);

    if (Notification.permission === "granted") {
      new Notification("🧠 Daily Crypto Report", {
        body: report.slice(0, 200) + "..."
      });
    }

    localStorage.setItem("lastAIReport", Date.now());
  }
}
async function saveDailyReport(text) {
  const user = auth.currentUser;
  if (!user) return;

  await db.collection("dailyReports").add({
    userId: user.uid,
    text,
    createdAt: firebase.firestore.FieldValue.serverTimestamp()
  });
}

