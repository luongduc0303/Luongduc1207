<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Web tỏ tình + Minigame 🌸</title>
<style>
  body {
    margin: 0; padding: 0;
    background: #ffe6f0;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    text-align: center;
    user-select: none;
    overflow-x: hidden;
  }
  h1 {
    color: #d6336c;
    margin-top: 20px;
  }
  #gameArea, #easyGameArea {
    position: relative;
    margin: 20px auto;
    width: 320px;
    height: 400px;
    background: #fff0f6;
    border: 3px dashed #ff6699;
    border-radius: 15px;
    overflow: hidden;
  }
  .flower, .star {
    position: absolute;
    cursor: pointer;
    user-select: none;
    will-change: transform;
  }
  .flower {
    width: 40px;
    height: 40px;
    background: url('https://i.imgur.com/uaQX49N.png') no-repeat center/contain;
  }
  .star {
    width: 30px;
    height: 30px;
    background: url('https://i.imgur.com/QZJ7TyX.png') no-repeat center/contain;
  }
  #scoreboard, #scoreboardEasy {
    font-size: 22px;
    margin-top: 10px;
    color: #d6336c;
  }
  #timeLeft, #timeLeftEasy {
    font-size: 20px;
    margin-top: 5px;
    color: #c2185b;
  }
  #postGameQuestion {
    display: none;
    margin-top: 20px;
  }
  #postGameQuestion button {
    margin: 10px;
    padding: 12px 25px;
    font-size: 18px;
    border-radius: 8px;
    border: none;
    cursor: pointer;
    background-color: #ff6699;
    color: white;
    transition: background-color 0.3s ease;
  }
  #postGameQuestion button:hover {
    background-color: #ff3366;
  }
  #nuzzleMessage {
    display: none;
    font-size: 24px;
    color: #d6336c;
    margin-top: 20px;
  }
  #chatBox {
    display: none;
    margin-top: 20px;
    font-size: 18px;
    color: #d6336c;
  }
  /* Chữ rơi */
  .falling-char {
    position: fixed;
    top: 0;
    font-size: 26px;
    color: #c2185b;
    font-weight: 700;
    user-select: none;
    pointer-events: none;
    text-shadow: 1px 1px 2px #880033;
    will-change: transform, opacity;
    animation-fill-mode: forwards;
    animation-timing-function: ease-in;
    opacity: 0;
    z-index: 9999;
  }
  @keyframes fallDown {
    0% {
      opacity: 0;
      transform: translateY(-50px);
    }
    20% {
      opacity: 1;
    }
    100% {
      opacity: 1;
      transform: translateY(80vh);
    }
  }
  /* Emoji khóc rơi */
  .falling-emoji {
    position: fixed;
    top: 0;
    font-size: 32px;
    user-select: none;
    pointer-events: none;
    opacity: 0.85;
    will-change: transform, opacity;
    animation-fill-mode: forwards;
    animation-timing-function: linear;
    z-index: 9998;
  }
  @keyframes emojiFall {
    0% {
      opacity: 0.85;
      transform: translateY(-40px) rotate(0deg);
    }
    100% {
      opacity: 0;
      transform: translateY(100vh) rotate(720deg);
    }
  }
  /* Nút xin lỗi dịch chuyển */
  #sorryBtn {
    margin: 15px;
    padding: 15px 30px;
    font-size: 18px;
    border-radius: 12px;
    border: none;
    background-color: #d6336c;
    color: white;
    cursor: pointer;
    position: relative;
    user-select: none;
    transition: background-color 0.3s ease;
  }
  #sorryBtn:hover {
    background-color: #ff3366;
  }
  #agreeBtn {
    margin: 15px;
    padding: 15px 30px;
    font-size: 18px;
    border-radius: 12px;
    border: none;
    background-color: #ff6699;
    color: white;
    cursor: pointer;
    user-select: none;
    transition: background-color 0.3s ease;
  }
  #agreeBtn:hover {
    background-color: #ff3366cc;
  }
  #initialQuestion {
    margin-top: 40px;
  }
  /* Video pixel buồn */
  #sadVideoContainer {
    display: none;
    margin-top: 20px;
  }
  #sadVideoContainer video {
    width: 320px;
    border-radius: 15px;
    border: 3px solid #d6336c;
  }
  #apologyMsg {
    color: #c2185b;
    font-size: 20px;
    margin-top: 20px;
    max-width: 320px;
    margin-left: auto;
    margin-right: auto;
    font-style: italic;
    user-select: none;
  }
</style>
</head>
<body>

<h1>🌸 Web tỏ tình + Minigame 🌸</h1>

<!-- Câu hỏi ban đầu -->
<div id="initialQuestion">
  <button id="agreeBtn">Đồng ý</button>
  <button id="sorryBtn">Xin lỗi, mình chỉ muốn làm bạn</button>
</div>

<!-- Video pixel buồn -->
<div id="sadVideoContainer">
  <video src="https://interactive-examples.mdn.mozilla.net/media/cc0-videos/flower.webm" autoplay loop muted playsinline></video>
  <p style="color:#d6336c; font-style: italic; margin-top:10px;">Xin lỗi đã làm phiền...</p>
</div>

<!-- Game chính -->
<div id="gameArea"></div>
<div id="scoreboard">Điểm: 0</div>
<div id="timeLeft">Thời gian: 15 giây</div>

<!-- Tin nhắn xin lỗi + câu hỏi sau game -->
<div id="apologyMsg"></div>

<div id="postGameQuestion">
  <p>Bạn muốn...</p>
  <button id="chatBtn">Nhắn tin với tớ 💬</button>
  <button id="stayBtn">Ở lại :3</button>
</div>

<div id="nuzzleMessage">Tớ nựng cậu một xíu 🥰</div>

<div id="chatBox">
  <p>Nhắn tin với tớ nhé! ❤️</p>
  <p>(Sẽ dẫn cậu sang Facebook.)</p>
</div>

<!-- Audio chill -->
<audio id="chillAudio" loop preload="auto">
  <source src="https://cdn.pixabay.com/download/audio/2022/03/24/audio_8c79eaf882.mp3?filename=relaxing-music-ambient-11818.mp3" type="audio/mpeg">
  Your browser does not support the audio element.
</audio>

<script>
  const agreeBtn = document.getElementById('agreeBtn');
  const sorryBtn = document.getElementById('sorryBtn');
  const initialQuestion = document.getElementById('initialQuestion');
  const sadVideoContainer = document.getElementById('sadVideoContainer');
  const apologyMsg = document.getElementById('apologyMsg');
  const postGameQuestion = document.getElementById('postGameQuestion');
  const chatBtn = document.getElementById('chatBtn');
  const stayBtn = document.getElementById('stayBtn');
  const nuzzleMessage = document.getElementById('nuzzleMessage');
  const chatBox = document.getElementById('chatBox');
  const chillAudio = document.getElementById('chillAudio');

  const gameArea = document.getElementById('gameArea');
  const scoreboard = document.getElementById('scoreboard');
  const timeLeftDisplay = document.getElementById('timeLeft');

  let sorryClicks = 0;
  const maxSorryClicks = 5;
  let score = 0;
  let timeLeft = 15;
  let gameInterval;
  let countdownInterval;

  // Mảng các chữ rơi: "Thương cậu nhìu lắm"
  const fallingText = "Thương cậu nhìu lắm";

  // Các emoji buồn dùng cho hiệu ứng rơi
  const emojis = ['😭', '😢', '🥺'];

  // Khi click "Xin lỗi"
  sorryBtn.addEventListener('click', () => {
    sorryClicks++;
    // Dịch chuyển vị trí nút theo chu kỳ, mượt hơn
    const x = Math.random() * (window.innerWidth - sorryBtn.offsetWidth);
    const y = Math.random() * (window.innerHeight - sorryBtn.offsetHeight - 100) + 50;
    sorryBtn.style.position = 'fixed';
    sorryBtn.style.left = x + 'px';
    sorryBtn.style.top = y + 'px';

    // Tạo chữ rơi mỗi lần click
    createFallingChars();

    // Tạo emoji buồn rơi
    createFallingEmoji();

    if (sorryClicks >= maxSorryClicks) {
      // Hiện video pixel buồn, ẩn nút
      initialQuestion.style.display = 'none';
      sadVideoContainer.style.display = 'block';
      apologyMsg.textContent = "Mình hiểu rồi... Mình xin lỗi nếu làm phiền cậu nha.";
      apologyMsg.style.display = 'block';
      // Tắt hiệu ứng nút
      sorryBtn.style.display = 'none';
      agreeBtn.style.display = 'none';
    }
  });

  // Khi click "Đồng ý"
  agreeBtn.addEventListener('click', () => {
    initialQuestion.style.display = 'none';
    startGame();
  });

  // Tạo hiệu ứng chữ rơi mượt
  function createFallingChars() {
    for(let i = 0; i < fallingText.length; i++) {
      const span = document.createElement('span');
      span.textContent = fallingText[i];
      span.className = 'falling-char';
      // Random vị trí ngang
      span.style.left = (Math.random() * window.innerWidth) + 'px';
      span.style.top = '-30px';
      span.style.animation = `fallDown 4s ease forwards`;
      span.style.animationDelay = (i * 0.1) + 's';
      document.body.appendChild(span);

      // Xóa sau khi animation kết thúc (4s)
      setTimeout(() => {
        span.remove();
      }, 4000);
    }
  }

  // Tạo emoji rơi buồn mượt
  function createFallingEmoji() {
    const emoji = emojis[Math.floor(Math.random() * emojis.length)];
    const span = document.createElement('span');
    span.textContent = emoji;
    span.className = 'falling-emoji';
    span.style.left = (Math.random() * window.innerWidth) + 'px';
    span.style.top = '-40px';
    span.style.animation = 'emojiFall 5s linear forwards';
    document.body.appendChild(span);
    setTimeout(() => {
      span.remove();
    }, 5000);
  }

  // Bắt đầu minigame
  function startGame() {
    score = 0;
    timeLeft = 15;
    scoreboard.textContent = "Điểm: 0";
    timeLeftDisplay.textContent = `Thời gian: ${timeLeft} giây`;
    gameArea.style.display = 'block';
    scoreboard.style.display = 'block';
    timeLeftDisplay.style.display = 'block';
    apologyMsg.style.display = 'none';
    postGameQuestion.style.display = 'none';
    nuzzleMessage.style.display = 'none';
    chatBox.style.display = 'none';

    chillAudio.currentTime = 0;
    chillAudio.play();

    spawnFlower();

    countdownInterval = setInterval(() => {
      timeLeft--;
      timeLeftDisplay.textContent = `Thời gian: ${timeLeft} giây`;
      if (timeLeft <= 0) {
        endGame();
      }
    }, 1000);
  }

  // Kết thúc game
  function endGame() {
    clearInterval(countdownInterval);
    chillAudio.pause();

    gameArea.innerHTML = '';
    gameArea.style.display = 'none';
    scoreboard.style.display = 'none';
    timeLeftDisplay.style.display = 'none';

    if(score >= 5) {
      // Nếu điểm cao, hỏi thêm
      postGameQuestion.style.display = 'block';
    } else {
      apologyMsg.textContent = "Chơi được vậy rồi, tớ nựng cậu một xíu 🥰";
      apologyMsg.style.display = 'block';
      nuzzleMessage.style.display = 'block';
    }
  }

  // Tạo bông hoa rơi, người chơi click để cộng điểm
  function spawnFlower() {
    const flower = document.createElement('div');
    flower.className = 'flower';

    // Bắt đầu từ trên cùng, vị trí ngẫu nhiên ngang
    flower.style.left = Math.random() * (gameArea.clientWidth - 40) + 'px';
    flower.style.top = '-50px';

    gameArea.appendChild(flower);

    // Tạo animation rơi mượt
    let position = -50;
    const speed = 1 + Math.random() * 2; // vận tốc rơi

    function fall() {
      position += speed;
      flower.style.top = position + 'px';

      if(position > gameArea.clientHeight) {
        // Nếu rơi ra ngoài thì xóa và tạo hoa mới
        flower.remove();
        if(timeLeft > 0) spawnFlower();
      } else {
        requestAnimationFrame(fall);
      }
    }
    requestAnimationFrame(fall);

    flower.addEventListener('click', () => {
      score++;
      scoreboard.textContent = `Điểm: ${score}`;
      flower.remove();
      if(timeLeft > 0) spawnFlower();
    });
  }

  // Xử lý nút sau game
  chatBtn.addEventListener('click', () => {
    postGameQuestion.style.display = 'none';
    chatBox.style.display = 'block';
    // Dẫn link Facebook (ví dụ)
    window.open('https://www.facebook.com/messages/t/', '_blank');
  });

  stayBtn.addEventListener('click', () => {
    postGameQuestion.style.display = 'none';
    nuzzleMessage.style.display = 'block';
  });

</script>
</body>
</html>
