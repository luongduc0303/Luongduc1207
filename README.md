<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Gửi Nga 💖</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="container">
    <h1>Gửi Nga 💌</h1>
    <p id="message">Anh có điều này muốn nói với em...</p>

    <div class="buttons">
      <button onclick="showMessage(0)">💬 Câu 1</button>
      <button onclick="showMessage(1)">💬 Câu 2</button>
      <button onclick="showMessage(2)">💬 Câu 3</button>
      <button onclick="showMessage(3)">💬 Câu 4</button>
      <button onclick="showMessage(4)">💬 Câu 5</button>
    </div>

    <div class="final-choice">
      <button onclick="accept()">💖 Em đồng ý</button>
      <button onclick="think()">🤔 Em cần suy nghĩ</button>
    </div>
  </div>

  <script src="script.js"></script>
</body>
</html>
