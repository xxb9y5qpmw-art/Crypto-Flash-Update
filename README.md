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


