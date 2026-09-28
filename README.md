<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Dodge the Blocks</title>

  <style>
    body {
      margin: 0;
      background: #111;
      color: white;
      font-family: Arial, sans-serif;
      text-align: center;
    }

    h1 {
      margin: 15px;
    }

    #score {
      font-size: 22px;
      margin-bottom: 10px;
    }

    #game {
      width: 400px;
      height: 600px;
      background: #222;
      border: 3px solid white;
      margin: auto;
      position: relative;
      overflow: hidden;
    }

    #player {
      width: 50px;
      height: 50px;
      background: #00aaff;
      position: absolute;
      bottom: 20px;
      left: 175px;
      border-radius: 8px;
    }

    .enemy {
      width: 50px;
      height: 50px;
      background: red;
      position: absolute;
      top: -50px;
      border-radius: 8px;
    }

    button {
      margin: 15px;
      padding: 10px 25px;
      font-size: 18px;
      cursor: pointer;
      border: none;
      border-radius: 8px;
      background: #00aaff;
      color: white;
    }
  </style>
</head>

<body>

  <h1>🚗 Dodge the Blocks</h1>
  <div id="score">Score: 0</div>

  <button onclick="startGame()">Start Game</button>

  <div id="game">
    <div id="player"></div>
  </div>

  <script>
    const player = document.getElementById("player");
    const game = document.getElementById("game");
    const scoreText = document.getElementById("score");

    let playerX = 175;
    let score = 0;
    let gameRunning = false;
    let enemies = [];

    document.addEventListener("keydown", function(event) {
      if (!gameRunning) return;

      if (event.key === "ArrowLeft" && playerX > 0) {
        playerX -= 25;
      }

      if (event.key === "ArrowRight" && playerX < 350) {
        playerX += 25;
      }

      player.style.left = playerX + "px";
    });

    function startGame() {
      score = 0;
      playerX = 175;
      gameRunning = true;

      scoreText.textContent = "Score: 0";
      player.style.left = playerX + "px";

      enemies.forEach(enemy => enemy.remove());
      enemies = [];

      gameLoop();
    }

    function createEnemy() {
      if (!gameRunning) return;

      const enemy = document.createElement("div");
      enemy.classList.add("enemy");

      enemy.style.left = Math.random() * 350 + "px";
      enemy.style.top = "-50px";

      game.appendChild(enemy);
      enemies.push(enemy);
    }

    function gameLoop() {
      if (!gameRunning) return;

      enemies.forEach((enemy, index) => {
        let y = parseInt(enemy.style.top);
        y += 5;

        enemy.style.top = y + "px";

        const enemyX = parseInt(enemy.style.left);

        // Collision detection
        if (
          y + 50 > 530 &&
          y < 580 &&
          enemyX < playerX + 50 &&
          enemyX + 50 > playerX
        ) {
          gameOver();
        }

        // Enemy passed player
        if (y > 600) {
          enemy.remove();
          enemies.splice(index, 1);

          score++;
          scoreText.textContent = "Score: " + score;
        }
      });

      requestAnimationFrame(gameLoop);
    }

    function gameOver() {
      gameRunning = false;

      alert("💥 Game Over!\nYour score: " + score);
    }

    // Create enemies regularly
    setInterval(() => {
      if (gameRunning) {
        createEnemy();
      }
    }, 800);
  </script>

</body>
</html>
