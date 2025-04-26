<!DOCTYPE html>
<html lang="tr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Kaydırılabilir Tek Sayfa Site + Oyun</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      scroll-behavior: smooth;
    }
    header {
      background-color: #4CAF50;
      color: white;
      padding: 20px;
      text-align: center;
      position: sticky;
      top: 0;
      z-index: 1000;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    header h1 {
      margin: 0;
      font-size: 24px;
    }
    header button {
      padding: 10px 20px;
      background-color: white;
      color: #4CAF50;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-weight: bold;
    }
    section {
      padding: 100px 20px;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
    }
    .section-1 { background-color: #f2f2f2; }
    .section-2 { background-color: #e6e6e6; }
    .section-3 { background-color: #cccccc; }
    footer {
      background-color: #333;
      color: white;
      text-align: center;
      padding: 20px;
    }
    .modal {
      display: none;
      position: fixed;
      z-index: 2000;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      overflow: auto;
      background-color: rgba(0,0,0,0.5);
    }
    .modal-content {
      background-color: #fff;
      margin: 10% auto;
      padding: 20px;
      border: 1px solid #888;
      width: 300px;
      text-align: center;
      border-radius: 10px;
    }
    .modal-content input {
      width: 90%;
      padding: 10px;
      margin: 10px 0;
    }
    .tab-buttons {
      display: flex;
      justify-content: space-around;
      margin-bottom: 10px;
    }
    .tab-buttons button {
      flex: 1;
      padding: 10px;
      cursor: pointer;
      background-color: #eee;
      border: none;
      font-weight: bold;
    }
    .tab-buttons button.active {
      background-color: #4CAF50;
      color: white;
    }
    .tab-content {
      display: none;
    }
    .tab-content.active {
      display: block;
    }
    .status-message {
      margin-top: 20px;
      font-size: 18px;
      color: green;
    }
    .source-link {
      margin-top: 20px;
      font-size: 16px;
      color: #4CAF50;
      cursor: pointer;
    }
    canvas {
      background: #cceeff;
      display: block;
      margin: 20px auto;
      border: 2px solid #000;
    }
  </style>
</head>
<body>

  <header>
    <h1>Benim Sitem</h1>
    <button id="signInBtn">Giriş/Kayıt</button>
  </header>

  <div id="myModal" class="modal">
    <div class="modal-content">
      <span class="close">&times;</span>
      <div class="tab-buttons">
        <button class="tablink active" data-tab="giris">Giriş Yap</button>
        <button class="tablink" data-tab="kayit">Kayıt Ol</button>
      </div>
      <div id="giris" class="tab-content active">
        <input type="text" id="loginUser" placeholder="Kullanıcı Adı">
        <input type="password" id="loginPass" placeholder="Şifre">
        <br><br>
        <button onclick="girisYap()">Giriş Yap</button>
      </div>
      <div id="kayit" class="tab-content">
        <input type="text" id="registerName" placeholder="Ad Soyad">
        <input type="text" id="registerEmail" placeholder="E-posta">
        <input type="password" id="registerPass" placeholder="Şifre">
        <br><br>
        <button onclick="kayitOl()">Kayıt Ol</button>
      </div>
    </div>
  </div>

  <section class="section-1">
    <h2>Hoşgeldiniz!</h2>
    <p>Bu bir demo sitedir. Aşağı kaydırarak daha fazla içerik keşfedin!</p>
    <div id="statusMessage" class="status-message"></div>
    <div id="sourceLink" class="source-link"></div>
  </section>

  <section class="section-2">
    <h2>Hakkımızda</h2>
    <p>Biz yaratıcı fikirlerle dolu bir ekibiz. Her zaman yeniliği hedefliyoruz.</p>
  </section>

  <section class="section-3">
    <h2>Mimi Blok Atlama Oyunu</h2>
    <canvas id="gameCanvas" width="500" height="300"></canvas>
    <br>
    <button onclick="location.reload()">Yeniden Başlat</button>
  </section>

  <footer>
    &copy; 2025 Benim Sitem. Tüm hakları saklıdır.
  </footer>

<script>
  const modal = document.getElementById("myModal");
  const btn = document.getElementById("signInBtn");
  const span = document.getElementsByClassName("close")[0];
  const tabButtons = document.querySelectorAll(".tablink");
  const tabContents = document.querySelectorAll(".tab-content");
  const statusMessage = document.getElementById("statusMessage");
  const sourceLink = document.getElementById("sourceLink");

  btn.onclick = () => modal.style.display = "block";
  span.onclick = () => modal.style.display = "none";
  window.onclick = (event) => { if (event.target == modal) modal.style.display = "none"; };

  tabButtons.forEach(button => {
    button.addEventListener("click", () => {
      tabButtons.forEach(btn => btn.classList.remove("active"));
      tabContents.forEach(content => content.classList.remove("active"));
      button.classList.add("active");
      document.getElementById(button.getAttribute("data-tab")).classList.add("active");
    });
  });

  function kayitOl() {
    const name = document.getElementById("registerName").value;
    const email = document.getElementById("registerEmail").value;
    const pass = document.getElementById("registerPass").value;

    if (name && email && pass) {
      alert(`Tebrikler ${name}, başarıyla kayıt oldunuz! 🎉`);
      modal.style.display = "none";
    } else {
      alert("Lütfen tüm bilgileri doldurun!");
    }
  }

  function girisYap() {
    const user = document.getElementById("loginUser").value;
    const pass = document.getElementById("loginPass").value;

    if (user && pass) {
      // Sayfada giriş yapıldığını gösterecek mesaj
      statusMessage.textContent = `Hoşgeldin ${user}! Başarıyla giriş yaptın. 🎉`;
      modal.style.display = "none";
      // Açık kaynak kodu bağlantısını göster
      sourceLink.innerHTML = 'Açık kaynak kodlarına buradan ulaşabilirsiniz: <a href="https://github.com/your-repository-link" target="_blank">GitHub Repository</a>';
    } else {
      alert("Lütfen kullanıcı adı ve şifre girin!");
    }
  }

  const canvas = document.getElementById("gameCanvas");
  const ctx = canvas.getContext("2d");

  let score = 0;
  let gameOver = false;

  let mimi = {
    x: 50,
    y: 200,
    width: 30,
    height: 30,
    velocityY: 0,
    jumpStrength: -10,
    gravity: 0.5,
    grounded: false,
    jumps: 0,
    maxJumps: 2
  };

  let block = {
    x: 500,
    y: 200,
    width: 30,
    height: 30,
    speed: 3,
    color: "#333"
  };

  function drawMimi() {
    ctx.fillStyle = "#ff66cc";
    ctx.fillRect(mimi.x, mimi.y, mimi.width, mimi.height);
  }

  function drawBlock() {
    ctx.fillStyle = block.color;
    ctx.fillRect(block.x, block.y, block.width, block.height);
  }

  function drawGround() {
    ctx.fillStyle = "#888";
    ctx.fillRect(0, 230, canvas.width, 70);
  }

  function drawScore() {
    ctx.fillStyle = "#000";
    ctx.font = "16px Arial";
    ctx.fillText("Puan: " + score, 10, 20);
  }

  function checkCollision() {
    if (
      mimi.x < block.x + block.width &&
      mimi.x + mimi.width > block.x &&
      mimi.y < block.y + block.height &&
      mimi.y + mimi.height > block.y
    ) {
      gameOver = true;
    }
  }

  function jump() {
    if (mimi.jumps < mimi.maxJumps) {
      mimi.velocityY = mimi.jumpStrength;
      mimi.jumps++;
    }
  }

  function update() {
    if (gameOver) {
      ctx.fillStyle = "red";
      ctx.font = "30px Arial";
      ctx.fillText("Oyun Bitti!", 150, 150);
      return;
    }

    ctx.clearRect(0, 0, canvas.width, canvas.height);
    drawGround();

    mimi.velocityY += mimi.gravity;
    mimi.y += mimi.velocityY;

    if (mimi.y + mimi.height > 230) {
      mimi.y = 230 - mimi.height;
      mimi.velocityY = 0;
      mimi.grounded = true;
      mimi.jumps = 0;
    } else {
      mimi.grounded = false;
    }

    block.x -= block.speed;
    if (block.x + block.width < 0) {
      block.x = canvas.width;
      score++;
      block.color = Math.random() < 0.1 ? "#ff0000" : "#333";
    }

    drawMimi();
    drawBlock();
    drawScore();
    checkCollision();

    requestAnimationFrame(update);
  }

  document.addEventListener("keydown", function (e) {
    if (e.code === "Space") jump();
  });

  canvas.addEventListener("touchstart", function () {
    jump();
  });

  canvas.addEventListener("click", function () {
    jump();
  });

  update();
</script>

</body>
</html>
