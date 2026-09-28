<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Click the Circle Game</title>

  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: #111827;
      color: white;
      text-align: center;
    }

    h1 {
      margin: 20px 0 10px;
    }

    #info {
      font-size: 20px;
      margin-bottom: 15px;
    }

    #game {
      width: 90vw;
      max-width: 800px;
      height: 500px;
      margin: auto;
      background: #1f2937;
      border: 3px solid #374151;
      border-radius: 15px;
      position: relative;
      overflow: hidden;
    }

    #circle {
      width: 50px;
      height: 50px;
      background: #22c55e;
      border-radius: 50%;
      position: absolute;
      cursor: pointer;
      display: none;
    }

    button {
      padding: 12px 25px;
      font-size: 18px;
      border: none;
      border-radius: 8px;
      background: #3b82f6;
      color: white;
      cursor: pointer;
    }

    button:hover {
      background: #2563eb;
    }
  </style>
</head>

<body>

  <h1>🎯 Click the Circle!</h1>

  <div id="info">
    Score: <span id="score">0</span> |
    Time: <span id="time">30</span>
  </div>

  <button onclick="startGame()">Start Game</button>

  <div id="game">
    <div id="circle" onclick="hitCircle()"></div>
  </div>

  <script>
    let score = 0;
    let time = 30;
    let timer;
    let playing = false;

    const circle = document.getElementById("circle");
    const game = document.getElementById("game");
    const scoreText = document.getElementById("score");
    const timeText = document.getElementById("time");

    function startGame() {
      score = 0;
      time = 30;
      playing = true;

      scoreText.textContent = score;
      timeText.textContent = time;

      circle.style.display = "block";
      moveCircle();

      clearInterval(timer);

      timer = setInterval(() => {
        time--;
        timeText.textContent = time;

        if (time <= 0) {
          endGame();
        }
      }, 1000);
    }

    function hitCircle() {
      if (!playing) return;

      score++;
      scoreText.textContent = score;

      moveCircle();
    }

    function moveCircle() {
      const maxX = game.clientWidth - circle.offsetWidth;
      const maxY = game.clientHeight - circle.offsetHeight;

      const x = Math.random() * maxX;
      const y = Math.random() * maxY;

      circle.style.left = x + "px";
      circle.style.top = y + "px";
    }

    function endGame() {
      playing = false;
      clearInterval(timer);

      circle.style.display = "none";

      alert("Game Over! Your score: " + score);
    }
  </script>

</body>
</html>
