<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Ở đây có mỗi hai ta😊</title>
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
    display: none;
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

<h1>Ở đây có mỗi hai ta😊</h1>

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
      apologyMsg.textContent = "Mình hiểu rồi... Mình xin lỗi nếu làm bạn buồn :(";
      apologyMsg.style.display = 'block';
      chillAudio.pause();
    }
  });

  // Tạo chữ rơi animation
  function createFallingChars() {
    for(let i=0; i<fallingText.length; i++) {
      const span = document.createElement('span');
      span.textContent = fallingText[i];
      span.classList.add('falling-char');
      span.style.left = Math.random() * (window.innerWidth - 20) + 'px';
      span.style.animation = `fallDown 3s ease forwards`;
      span.style.animationDelay = (i * 0.2) + 's';
      document.body.appendChild(span);

      setTimeout(() => {
        span.remove();
      }, 3500);
    }
  }

  // Tạo emoji khóc rơi
  function createFallingEmoji() {
    const emoji = document.createElement('div');
    emoji.textContent = emojis[Math.floor(Math.random() * emojis.length)];
    emoji.classList.add('falling-emoji');
    emoji.style.left = Math.random() * (window.innerWidth - 30) + 'px';
    emoji.style.animation = `emojiFall 4s linear forwards`;
    document.body.appendChild(emoji);
    setTimeout(() => emoji.remove(), 4000);
  }

  // Khi click đồng ý, bắt đầu game
  agreeBtn.addEventListener('click', () => {
    initialQuestion.style.display = 'none';
    chillAudio.play();
    startGame();
  });

  // Bắt đầu game mini nhặt hoa (đơn giản)
  function startGame() {
    score = 0;
    timeLeft = 15;
    scoreboard.textContent = "Điểm: " + score;
    timeLeftDisplay.textContent = "Thời gian: " + timeLeft + " giây";
    gameArea.style.display = 'block';
    postGameQuestion.style.display = 'none';
    apologyMsg.style.display = 'none';
    nuzzleMessage.style.display = 'none';
    chatBox.style.display = 'none';

    spawnFlower();

    countdownInterval = setInterval(() => {
      timeLeft--;
      timeLeftDisplay.textContent = "Thời gian: " + timeLeft + " giây";
      if(timeLeft <= 0) {
        clearInterval(countdownInterval);
        gameArea.style.display = 'none';
        postGameQuestion.style.display = 'block';
      }
    }, 1000);
  }

  // Tạo hoa ngẫu nhiên trong vùng game
  function spawnFlower() {
    if(timeLeft <= 0) return;

    const flower = document.createElement('div');
    flower.classList.add('flower');
    const x = Math.random() * (gameArea.clientWidth - 40);
    const y = Math.random() * (gameArea.clientHeight - 40);
    flower.style.left = x + 'px';
    flower.style.top = y + 'px';

    flower.addEventListener('click', () => {
      score++;
      scoreboard.textContent = "Điểm: " + score;
      flower.remove();
      spawnFlower();
    });

    gameArea.appendChild(flower);
  }

  // Xử lý nút chat và ở lại
  chatBtn.addEventListener('click', () => {
    window.location.href = "https://www.facebook.com/luongduc1234";
  });

  stayBtn.addEventListener('click', () => {
    postGameQuestion.style.display = 'none';
    nuzzleMessage.style.display = 'block';

    setTimeout(() => {
      nuzzleMessage.style.display = 'none';
      startEasyGame();
    }, 2500);
  });

  // Minigame dễ
  function startEasyGame() {
    score = 0;
    timeLeft = 10;
    scoreboard.textContent = "Điểm: " + score;
    timeLeftDisplay.textContent = "Thời gian: " + timeLeft + " giây";
    gameArea.style.display = 'block';

    // Thay đổi hoa thành sao cho khác game 1
    gameArea.innerHTML = ''; // reset

    spawnStar();

    countdownInterval = setInterval(() => {
      timeLeft--;
      timeLeftDisplay.textContent = "Thời gian: " + timeLeft + " giây";
      if(timeLeft <= 0) {
        clearInterval(countdownInterval);
        gameArea.style.display = 'none';
        showFinalMessage();
      }
    }, 1000);
  }

  // Tạo sao trong game dễ
  function spawnStar() {
    if(timeLeft <= 0) return;

    const star = document.createElement('div');
    star.classList.add('star');
    const x = Math.random() * (gameArea.clientWidth - 30);
    const y = Math.random() * (gameArea.clientHeight - 30);
    star.style.left = x + 'px';
    star.style.top = y + 'px';

    star.addEventListener('click', () => {
      score++;
      scoreboard.textContent = "Điểm: " + score;
      star.remove();
      spawnStar();
    });

    gameArea.appendChild(star);
  }

  // Tin nhắn cuối cùng với hiệu ứng chữ rơi và emoji khóc
  function showFinalMessage() {
    apologyMsg.textContent = '';
    apologyMsg.style.display = 'block';
    const message = "vk oi anh xin loi: <a hk de vo buon nx nha";

    // Tạo từng chữ rơi xuống
    let delay = 0;
    for(let i=0; i<message.length; i++) {
      setTimeout(() => {
        const span = document.createElement('span');
        span.textContent = message[i];
        span.classList.add('falling-char');
        span.style.left = Math.random() * (window.innerWidth - 20) + 'px';
        span.style.animation = `fallDown 4s ease forwards`;
        document.body.appendChild(span);
        setTimeout(() => span.remove(), 4500);
      }, delay);
      delay += 150;
    }

    // Tạo emoji khóc rơi nhiều lần
    let emojiCount = 0;
    const emojiInterval = setInterval(() => {
      createFallingEmoji();
      emojiCount++;
      if(emojiCount > 15) clearInterval(emojiInterval);
    }, 200);
  }
</script>

</body>
</html>
