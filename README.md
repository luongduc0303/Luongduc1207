<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8" />
  <title>Tớ có điều muốn nói...</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      background: #ffe6f0;
      font-family: 'Segoe UI', sans-serif;
      overflow: hidden;
      text-align: center;
      height: 100vh;
      position: relative;
      user-select: none;
    }

    h1 {
      margin-top: 30px;
      font-size: 2.5em;
      color: #d6336c;
    }

    .button-container {
      margin-top: 20px;
      position: relative;
      height: 200px;
      width: 300px;
      margin-left: auto;
      margin-right: auto;
    }

    button {
      margin: 10px;
      padding: 15px 30px;
      font-size: 18px;
      background-color: #ff6699;
      color: white;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      transition: 0.3s;
      position: relative;
      user-select: none;
    }

    button:hover {
      background-color: #ff3366;
    }

    #noBtn {
      position: absolute;
      top: 80px;
      left: 10px;
      width: 280px;
    }

    #acceptBtn {
      position: relative;
      z-index: 2;
      width: 280px;
    }

    #message {
      display: none;
      font-size: 24px;
      color: #c2185b;
      margin-top: 40px;
      max-width: 90%;
      margin-left: auto;
      margin-right: auto;
      min-height: 60px;
      user-select: text;
    }

    .typing {
      border-right: 2px solid #c2185b;
      white-space: nowrap;
      overflow: hidden;
      animation: typing 4s steps(40, end), blink 0.75s step-end infinite;
    }

    @keyframes typing {
      from { width: 0; }
      to { width: 100%; }
    }

    @keyframes blink {
      50% { border-color: transparent; }
    }

    .heart {
      position: absolute;
      color: #ff4d88;
      font-size: 20px;
      animation: floatDown 5s linear infinite;
      user-select: none;
      pointer-events: none;
      z-index: 1;
    }

    @keyframes floatDown {
      0% {
        transform: translateY(-50px) scale(1);
        opacity: 1;
      }
      100% {
        transform: translateY(100vh) scale(0.5);
        opacity: 0;
      }
    }

    .firework {
      position: absolute;
      width: 10px;
      height: 10px;
      background: radial-gradient(circle, #ff66cc 0%, transparent 70%);
      border-radius: 50%;
      animation: explode 1s ease-out;
      pointer-events: none;
      z-index: 3;
    }

    @keyframes explode {
      0% {
        transform: scale(0.2);
        opacity: 1;
      }
      100% {
        transform: scale(5);
        opacity: 0;
      }
    }

    /* Trò chơi bấm tim */
    #gameArea {
      position: relative;
      width: 100%;
      max-width: 320px;
      height: 300px;
      margin: 30px auto 10px;
      border: 2px dashed #ff6699;
      border-radius: 15px;
      background: #fff0f6;
      overflow: hidden;
      display: none;
      user-select: none;
      touch-action: manipulation;
    }

    #heartBtn {
      position: absolute;
      font-size: 40px;
      cursor: pointer;
      user-select: none;
      transition: transform 0.2s;
      user-select:none;
    }

    #gameInfo {
      margin-top: 15px;
      font-size: 20px;
      color: #d6336c;
      min-height: 24px;
      max-width: 320px;
      margin-left: auto;
      margin-right: auto;
      user-select: none;
    }

    /* Video pixel buồn */
    #pixelSadContainer {
      margin: 30px auto;
      max-width: 340px;
      background: #222;
      border-radius: 10px;
      padding: 15px;
      box-shadow: 0 0 10px #aaa;
      color: #ddd;
      font-style: italic;
      font-size: 18px;
      display: none;
      user-select: text;
    }

    #pixelSadContainer p {
      margin: 8px 0 0 0;
    }

    canvas {
      display: block;
      margin: 0 auto;
      image-rendering: pixelated;
      border-radius: 6px;
      background: #000;
    }

  </style>
</head>
<body>
  <h1>💖 Tớ có điều muốn nói... 💖</h1>

  <div class="button-container">
    <button id="acceptBtn" title="Bấm đồng ý để nghe lời tỏ tình">Tớ cũng thích cậu 💖</button>
    <button id="noBtn" title="Xin lỗi, mình chỉ muốn làm bạn">Xin lỗi, mình chỉ muốn làm bạn 😢</button>
  </div>

  <div id="message"></div>

  <!-- Khu vực trò chơi -->
  <div id="gameArea">
    <div id="heartBtn" title="Bấm nhanh trái tim này!">❤️</div>
  </div>
  <div id="gameInfo"></div>

  <!-- Video pixel buồn -->
  <div id="pixelSadContainer">
    <canvas id="pixelSadVideo" width="320" height="180"></canvas>
    <p>Xin lỗi nếu tớ làm phiền cậu quá nhiều... 😔</p>
    <p>Cảm ơn vì đã chịu nghe tớ nói.</p>
    <p>Mình vẫn mong được làm bạn, dù chỉ là bạn thôi.</p>
  </div>

  <!-- Audio sources -->
  <audio id="bgMusic" loop>
    <source src="https://cdn.jsdelivr.net/gh/jackrobertscott/chill-background-music@main/chill-loop.mp3" type="audio/mpeg" />
  </audio>

  <audio id="clickSound" src="https://actions.google.com/sounds/v1/ui/click.ogg"></audio>
  <audio id="heartSound" src="https://actions.google.com/sounds/v1/alarms/beep_short.ogg"></audio>
  <audio id="fireworkSound" src="https://actions.google.com/sounds/v1/fireworks/fireworks_explosion.ogg"></audio>

  <script>
    const acceptBtn = document.getElementById('acceptBtn');
    const noBtn = document.getElementById('noBtn');
    const msg = document.getElementById('message');
    const gameArea = document.getElementById('gameArea');
    const heartBtn = document.getElementById('heartBtn');
    const gameInfo = document.getElementById('gameInfo');
    const pixelSadContainer = document.getElementById('pixelSadContainer');
    const pixelSadVideo = document.getElementById('pixelSadVideo');
    const ctx = pixelSadVideo.getContext('2d');

    const bgMusic = document.getElementById('bgMusic');
    const clickSound = document.getElementById('clickSound');
    const heartSound = document.getElementById('heartSound');
    const fireworkSound = document.getElementById('fireworkSound');

    let refuseCount = 0;
    const maxRefuse = 5;

    // Nút từ chối né tránh
    function moveButton() {
      const button = noBtn;
      const container = document.querySelector('.button-container');

      const containerWidth = container.offsetWidth;
      const containerHeight = container.offsetHeight;

      const newLeft = Math.random() * (containerWidth - button.offsetWidth);
      const newTop = Math.random() * (containerHeight - button.offsetHeight);

      button.style.left = `${newLeft}px`;
      button.style.top = `${newTop}px`;
    }

    noBtn.onmouseover = () => {
      moveButton();
    };

    noBtn.onclick = () => {
      refuseCount++;
      clickSound.play();

      if (refuseCount >= maxRefuse) {
        showSadPixelVideo();
      }
    };

    acceptBtn.onclick = () => {
      clickSound.play();
      startLoveSequence();
    };

    function startLoveSequence() {
      // Ẩn nút từ chối né tránh
      noBtn.style.display = 'none';
      acceptBtn.disabled = true;
      acceptBtn.style.cursor = 'default';

      // Phát nhạc nền (nếu chưa phát)
      if (bgMusic.paused) {
        bgMusic.play().catch(() => {
          // Nếu trình duyệt chặn autoplay, sẽ chơi sau khi có tương tác
          console.log("Nhạc nền sẽ phát khi người dùng tương tác");
        });
      }

      msg.style.display = 'block';
      msg.classList.add('typing');
      msg.style.color = '#c2185b';
      msg.style.fontSize = '24px';
      msg.textContent = "Từ giờ phút này, cậu là người đặc biệt nhất đối với tớ rồi 💘 Mình hãy cùng nhau tạo nên những kỷ niệm thật đẹp nhé! 💑✨";

      startHeartRain(100);
      startFireworks(30);

      startGame();
    }

    // Hiệu ứng tim bay
    function startHeartRain(intervalTime) {
      const heartInterval = setInterval(() => {
        const heart = document.createElement("div");
        heart.classList.add("heart");
        heart.style.left = Math.random() * 100 + "vw";
        heart.style.fontSize = (10 + Math.random() * 20) + "px";
        heart.innerText = "❤️";
        document.body.appendChild(heart);

        setTimeout(() => {
          heart.remove();
        }, 5000);
      }, intervalTime);
      return heartInterval;
    }

    // Pháo hoa
    function launchFirework() {
      const fw = document.createElement("div");
      fw.classList.add("firework");
      fw.style.left = Math.random() * window.innerWidth + "px";
      fw.style.top = Math.random() * window.innerHeight + "px";
      document.body.appendChild(fw);

      fireworkSound.currentTime = 0;
      fireworkSound.play();

      setTimeout(() => {
        fw.remove();
      }, 1000);
    }

    function startFireworks(count) {
      for (let i = 0; i < count; i++) {
        setTimeout(() => {
          launchFirework();
        }, i * 150);
      }
    }

    // Trò chơi mini: bấm nhanh trái tim
    let score = 0;
    const gameTime = 15; // giây
    let timerInterval;

    function startGame() {
      score = 0;
      gameInfo.textContent = `Thời gian còn lại: ${gameTime} giây - Điểm: ${score}`;
      gameArea.style.display = 'block';
      heartBtn.style.display = 'block';

      moveHeart();

      heartBtn.onclick = heartClicked;

      let timeLeft = gameTime;
      timerInterval = setInterval(() => {
        timeLeft--;
        gameInfo.textContent = `Thời gian còn lại: ${timeLeft} giây - Điểm: ${score}`;

        if (timeLeft <= 0) {
          clearInterval(timerInterval);
          endGame();
        }
      }, 1000);
    }

    function moveHeart() {
      const areaWidth = gameArea.clientWidth - heartBtn.offsetWidth;
      const areaHeight = gameArea.clientHeight - heartBtn.offsetHeight;

      const x = Math.random() * areaWidth;
      const y = Math.random() * areaHeight;

      heartBtn.style.left = `${x}px`;
      heartBtn.style.top = `${y}px`;

      heartBtn.style.transform = 'scale(1)';
    }

    function heartClicked() {
      score++;
      heartSound.currentTime = 0;
      heartSound.play();

      // Hiệu ứng nhấn nhỏ lại rồi to lại
      heartBtn.style.transform = 'scale(0.8)';
      setTimeout(() => {
        moveHeart();
      }, 200);
    }

    function endGame() {
      heartBtn.style.display = 'none';
      gameInfo.textContent = `🎉 Bạn đã bấm được ${score} trái tim! ${score > 20 ? "Tuyệt vời quá đi! 💖" : "Cố gắng hơn lần sau nhé!"}`;
    }

    // Hiển thị video pixel buồn
    function showSadPixelVideo() {
      noBtn.style.display = 'none';
      acceptBtn.style.display = 'none';
      msg.style.display = 'none';

      pixelSadContainer.style.display = 'block';

      startPixelSadVideo();
    }

    // Animation pixel buồn trên canvas
    function startPixelSadVideo() {
      const w = pixelSadVideo.width;
      const h = pixelSadVideo.height;

      let frame = 0;

      function drawFace() {
        ctx.clearRect(0, 0, w, h);

        // Nền đen tím đậm có chớp nhẹ
        ctx.fillStyle = `rgba(30, 0, 40, ${0.5 + 0.5 * Math.sin(frame * 0.1)})`;
        ctx.fillRect(0, 0, w, h);

        // Mặt pixel đơn giản: vuông trắng
        ctx.fillStyle = '#fff';
        ctx.fillRect(w/2 - 40, h/2 - 40, 80, 80);

        // Mắt pixel: 2 chấm đen
        ctx.fillStyle = '#000';
        ctx.fillRect(w/2 - 25, h/2 - 10, 15, 15);
        ctx.fillRect(w/2 + 10, h/2 - 10, 15, 15);

        // Miệng pixel buồn (hình chữ U ngược)
        ctx.strokeStyle = '#000';
        ctx.lineWidth = 6;
        ctx.beginPath();
        ctx.moveTo(w/2 - 20, h/2 + 30);
        ctx.quadraticCurveTo(w/2, h/2 + 10, w/2 + 20, h/2 + 30);
        ctx.stroke();

        frame++;
      }

      function loop() {
        drawFace();
        requestAnimationFrame(loop);
      }

      loop();
    }

  </script>
</body>
</html>
