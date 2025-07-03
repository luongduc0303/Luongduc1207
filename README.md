<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
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
    }

    h1 {
      margin-top: 80px;
      font-size: 2.5em;
      color: #d6336c;
    }

    .button-container {
      margin-top: 50px;
      position: relative;
      height: 200px;
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
    }

    button:hover {
      background-color: #ff3366;
    }

    #noBtn {
      position: absolute;
    }

    #message {
      display: none;
      font-size: 24px;
      color: #c2185b;
      margin-top: 40px;
      max-width: 90%;
      margin-left: auto;
      margin-right: auto;
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
  </style>
</head>
<body>
  <h1>💖 Tớ có điều muốn nói... 💖</h1>
  <div class="button-container">
    <button onclick="acceptLove()">Tớ cũng thích cậu 💖</button>
    <button id="noBtn" onmouseover="moveButton()">Xin lỗi, mình chỉ muốn làm bạn 😢</button>
  </div>
  <div id="message"></div>

  <script>
    function acceptLove() {
      const msg = document.getElementById('message');
      msg.style.display = 'block';
      msg.classList.add('typing');
      msg.innerText = "Từ giờ phút này, cậu là người đặc biệt nhất đối với tớ rồi 💘 Mình hãy cùng nhau tạo nên những kỷ niệm thật đẹp nhé! 💑✨";

      // Hiệu ứng tim bay nhanh hơn
      startHeartRain(100);

      // Bắn pháo hoa
      for (let i = 0; i < 30; i++) {
        setTimeout(() => {
          launchFirework();
        }, i * 150);
      }
    }

    function moveButton() {
      const button = document.getElementById('noBtn');
      const container = document.querySelector('.button-container');

      const containerWidth = container.offsetWidth;
      const containerHeight = container.offsetHeight;

      const newLeft = Math.random() * (containerWidth - 150);
      const newTop = Math.random() * (containerHeight - 60);

      button.style.left = `${newLeft}px`;
      button.style.top = `${newTop}px`;
    }

    function startHeartRain(intervalTime) {
      setInterval(() => {
        const heart = document.createElement("div");
        heart.classList.add("heart");
        heart.style.left = Math.random() * 100 + "vw";
        heart.style.fontSize = Math.random() * 20 + 10 + "px";
        heart.innerText = "❤️";
        document.body.appendChild(heart);

        setTimeout(() => {
          heart.remove();
        }, 5000);
      }, intervalTime);
    }

    function launchFirework() {
      const fw = document.createElement("div");
      fw.classList.add("firework");
      fw.style.left = Math.random() * window.innerWidth + "px";
      fw.style.top = Math.random() * window.innerHeight + "px";
      document.body.appendChild(fw);
      setTimeout(() => {
        fw.remove();
      }, 1000);
    }

    // Tim bay mặc định nhẹ nhàng
    startHeartRain(300);
  </script>
</body>
</html>

