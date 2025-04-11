<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>ClickyMood</title>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap" rel="stylesheet">
  <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-auth-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-firestore-compat.js"></script>
  <style>
    body {
      font-family: 'Poppins', sans-serif;
      background: linear-gradient(135deg, #c8e6c9, #b3e5fc);
      background-size: 300% 300%;
      animation: gradientBG 15s ease infinite;
      text-align: center;
      margin: 0;
      padding: 40px;
      position: relative;
      overflow-x: hidden;
    }
    @keyframes gradientBG {
      0% { background-position: 0% 50%; }
      50% { background-position: 100% 50%; }
      100% { background-position: 0% 50%; }
    }
    .floating {
      position: absolute;
      width: 50px;
      animation: floatDown 10s linear infinite;
      opacity: 0.4;
    }
    @keyframes floatDown {
      0% { transform: translateY(-100px) rotate(0deg); }
      100% { transform: translateY(120vh) rotate(360deg); }
    }
    .logo-text {
      font-size: 2.8rem;
      font-weight: bold;
      color: #2e7d32;
      margin-bottom: 10px;
      text-shadow: 2px 2px #fff;
    }
    input, button {
      padding: 10px;
      margin: 10px;
      font-size: 16px;
      border-radius: 6px;
      border: none;
    }
    button {
      cursor: pointer;
      background: darkorange;
      color: white;
      transition: background 0.3s;
    }
    button:hover {
      background: #e65100;
    }
    .section {
      max-width: 700px;
      margin: 30px auto;
      background: #ffffffdd;
      padding: 30px;
      border-radius: 12px;
      box-shadow: 0 0 12px rgba(0,0,0,0.1);
    }
    #progress-bar {
      height: 25px;
      width: 0%;
      background: linear-gradient(to right, orange, darkorange);
      border-radius: 20px;
      transition: 0.3s;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 20px;
    }
    th, td {
      padding: 12px;
      border-bottom: 1px solid #eee;
    }
    th {
      background: orange;
      color: white;
    }
  </style>
</head>
<body>

<!-- Floating Icons -->
<img src="https://cdn-icons-png.flaticon.com/512/1170/1170576.png" class="floating" style="left:10%;" />
<img src="https://cdn-icons-png.flaticon.com/512/877/877931.png" class="floating" style="left:50%;" />
<img src="https://cdn-icons-png.flaticon.com/512/3135/3135715.png" class="floating" style="left:80%;" />

<!-- Brand Name -->
<div class="logo-text">ClickyMood</div>
<p>Visit 100 get up to 100$</h1>
<p>You will be eligible for the announced prize only if you successfully hit the number 100 from visiting our partner sites.</p>
<p>You have to visit and open minimum 15 sec our partner's site for a successfully single count</p>
<div class="section" id="auth-section">
  <h2>Sign Up / Log In</h2>
  <input type="email" id="email" placeholder="Email"><br>
  <input type="text" id="username" placeholder="Username"><br>
  <input type="password" id="password" placeholder="Password"><br>
  <button onclick="signUp()">Sign Up</button>
  <button onclick="logInUser()">Log In</button>
</div>

<div class="section" id="app-section" style="display:none;">
  <h2>Welcome, <span id="user-name"></span>!</h2>
  <button onclick="logOut()">Log Out</button>
  <br><br>
  <button id="visit-btn">🚀 Visit & Earn</button>
  <h3 id="counter-display">Visits: 0</h3>
  <p id="bonus-msg" style="color:green;"></p>
  <p id="streak-msg" style="color:orangered;"></p>

  <div style="margin: 20px auto; max-width: 400px; background: #eee; border-radius: 20px;">
    <div id="progress-bar"></div>
  </div>
  <p id="progress-text">Level 0 of 100</p>

  <!-- 🎁 Mystery Box Reward -->
  <div class="section" style="margin-top:40px;">
    <h2 style="color:#d2691e; font-size:28px;">🎁 Mystery Box Reward at Level 100</h2>
    <p style="font-size:18px;">Complete <strong>Level 100</strong> and unlock a surprise reward!</p>
    <div style="text-align:left; max-width:700px; margin:0 auto;">
      <ul style="font-size:17px; list-style: none; padding:0;">
        <li style="margin:10px 0;">🎁 <strong>Gift Cards</strong> – Amazon, Play Store</li>
        <li style="margin:10px 0;">🎬 <strong>Premium Memberships</strong> – Netflix, Spotify, YouTube Premium</li>
        <li style="margin:10px 0;">💻 <strong>Online Service Credits</strong> – Canva Pro, etc.</li>
        <li style="margin:10px 0;">💰 <strong>Cryptocurrency</strong> – USDT, BTC, ETH</li>
        <li style="margin:10px 0;">📱 <strong>Mobile Top-Up / Airtime</strong></li>
      </ul>
    </div>
    <p style="font-size:16px; margin-top:20px;">🔐 <strong>Note:</strong> Rewards are randomly selected and delivered after reaching Level 100. Everyone gets something real!</p>
  </div>

  <!-- 🏆 Leaderboard -->
  <h3>🏆 Top Contributors</h3>
  <table>
    <thead>
      <tr><th>Rank</th><th>User</th><th>Visits</th><th>Reward</th></tr>
    </thead>
    <tbody id="leaderboard"></tbody>
  </table>
</div>

<script>
  const firebaseConfig = {
    apiKey: "AIzaSyBwj01X0Mga3-9LjG2rCW6ymfMM6wXzCZA",
    authDomain: "visit100.firebaseapp.com",
    projectId: "visit100",
    storageBucket: "visit100.firebasestorage.app",
    messagingSenderId: "210904913407",
    appId: "1:210904913407:web:361490386a647e624383ca",
    measurementId: "G-ZYQNPHXVYF"
  };

  firebase.initializeApp(firebaseConfig);
  const auth = firebase.auth();
  const db = firebase.firestore();

  let currentUser = null;
  let streak = 0;
  const urls = [
    "https://m.ok.ru/dk?st.cmd=userProfile",
    "https://www.youtube.com",
    "https://www.gmail.com",
    "https://www.google.com"
  ];

  function signUp() {
    const email = document.getElementById('email').value;
    const username = document.getElementById('username').value;
    const password = document.getElementById('password').value;
    if (!email || !password || !username) return alert("All fields required");

    auth.createUserWithEmailAndPassword(email, password)
      .then(cred => {
        return db.collection("users").doc(cred.user.uid).set({
          email,
          username,
          visitCount: 0,
          reward: `$${Math.floor(Math.random() * 21 + 80)}`
        });
      })
      .catch(err => alert(err.message));
  }

  function logInUser() {
    const email = document.getElementById('email').value;
    const password = document.getElementById('password').value;
    auth.signInWithEmailAndPassword(email, password)
      .catch(err => alert(err.message));
  }

  function logOut() {
    auth.signOut();
  }

  function updateUI(userDoc) {
    document.getElementById('auth-section').style.display = 'none';
    document.getElementById('app-section').style.display = 'block';
    document.getElementById('user-name').innerText = userDoc.username;
    document.getElementById('counter-display').innerText = `Visits: ${userDoc.visitCount}`;
    updateProgressBar(userDoc.visitCount);
    loadLeaderboard();
  }

  auth.onAuthStateChanged(user => {
    if (user) {
      currentUser = user;
      db.collection("users").doc(user.uid).get().then(doc => {
        updateUI(doc.data());
      });
    } else {
      document.getElementById('auth-section').style.display = 'block';
      document.getElementById('app-section').style.display = 'none';
    }
  });

  document.getElementById('visit-btn').addEventListener('click', () => {
    const randomUrl = urls[Math.floor(Math.random() * urls.length)];
    window.open(randomUrl, "_blank");

    streak++;
    setTimeout(() => {
      let userRef = db.collection("users").doc(currentUser.uid);
      userRef.get().then(doc => {
        let data = doc.data();
        let newCount = data.visitCount + 1;
        let bonus = "";

        if (streak >= 3) {
          newCount += 2;
          streak = 0;
          bonus = "🔥 STREAK BONUS: +2!";
        }

        const today = new Date().toDateString();
        if (data.lastBonus !== today) {
          newCount += 5;
          bonus = "🎉 DAILY BONUS: +5!";
          userRef.update({ lastBonus: today });
        }

        userRef.update({ visitCount: newCount }).then(() => {
          document.getElementById('counter-display').innerText = `Visits: ${newCount}`;
          document.getElementById('bonus-msg').innerText = bonus;
          updateProgressBar(newCount);
          loadLeaderboard();
        });
      });
    }, 15000);
  });

  function updateProgressBar(count) {
    const progress = Math.min(count, 100);
    document.getElementById('progress-bar').style.width = `${progress}%`;
    document.getElementById('progress-text').innerText = `Level ${progress} of 100`;
  }

  function loadLeaderboard() {
    db.collection("users").orderBy("visitCount", "desc").limit(5).get().then(snapshot => {
      const tbody = document.getElementById('leaderboard');
      tbody.innerHTML = "";
      let rank = 1;
      snapshot.forEach(doc => {
        const data = doc.data();
        const row = `<tr>
          <td>${rank++}</td>
          <td>${data.username}</td>
          <td>${data.visitCount}</td>
          <td>${data.reward}</td>
        </tr>`;
        tbody.innerHTML += row;
      });
    });
  }
</script>

</body>
</html>
