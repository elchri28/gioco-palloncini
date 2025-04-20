# gioco-palloncini
scoppia tutti i palloncini
<!DOCTYPE html>
<html lang="it">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Scoppia i Palloncini - Game Over</title>
  <style>
    body {
      margin: 0;
      background: linear-gradient(#87CEEB, #ffffff);
      overflow: hidden;
      font-family: Arial, sans-serif;
    }

    #scoreboard {
      position: absolute;
      top: 10px;
      left: 10px;
      background: rgba(255,255,255,0.8);
      padding: 10px 20px;
      border-radius: 10px;
      font-size: 20px;
      font-weight: bold;
      z-index: 10;
    }

    .balloon {
      position: absolute;
      width: 50px;
      height: 70px;
      background-color: red;
      border-radius: 50% 50% 50% 50% / 60% 60% 40% 40%;
      cursor: pointer;
      animation: float 8s linear forwards;
    }

    .string {
      position: absolute;
      width: 2px;
      height: 30px;
      background-color: #333;
      top: 70px;
      left: 24px;
    }

    @keyframes float {
      from { bottom: -100px; }
      to { bottom: 110%; }
    }

    #gameOverScreen {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.85);
      color: white;
      font-size: 36px;
      text-align: center;
      padding-top: 30vh;
      display: none;
      z-index: 20;
    }

    /* MEDIA QUERIES per dispositivi mobili */
    @media (max-width: 768px) {
      #scoreboard {
        font-size: 18px;
        padding: 8px 15px;
      }

      .balloon {
        width: 40px;
        height: 55px;
      }

      .string {
        width: 2px;
        height: 25px;
      }
    }

    @media (max-width: 480px) {
      #scoreboard {
        font-size: 16px;
        padding: 6px 12px;
      }

      .balloon {
        width: 35px;
        height: 50px;
      }

      .string {
        width: 1px;
        height: 20px;
      }

      #gameOverScreen {
        font-size: 28px;
        padding-top: 40vh;
      }
    }
  </style>
</head>
<body>

  <div id="scoreboard">🎯 Punteggio: <span id="score">0</span></div>
  <div id="gameOverScreen">💀 GAME OVER<br>Punteggio finale: <span id="finalScore">0</span></div>

  <script>
    const scoreEl = document.getElementById('score');
    const finalScoreEl = document.getElementById('finalScore');
    const gameOverScreen = document.getElementById('gameOverScreen');
    let score = 0;
    let isGameOver = false;

    function gameOver() {
      isGameOver = true;
      finalScoreEl.textContent = score;
      gameOverScreen.style.display = 'block';
    }

    function createBalloon() {
      if (isGameOver) return;

      const balloon = document.createElement('div');
      balloon.classList.add('balloon');

      const string = document.createElement('div');
      string.classList.add('string');
      balloon.appendChild(string);

      // Posizione casuale orizzontale
      balloon.style.left = Math.random() * (window.innerWidth - 60) + "px";

      // Durata random (5-9s)
      const duration = 5 + Math.random() * 4;
      balloon.style.animationDuration = `${duration}s`;

      document.body.appendChild(balloon);

      // Timeout per Game Over se non cliccato
      const failTimeout = setTimeout(() => {
        if (!balloon.classList.contains('popped')) {
          gameOver();
        }
      }, duration * 1000);

      balloon.addEventListener('click', () => {
        if (isGameOver) return;
        score++;
        scoreEl.textContent = score;
        balloon.classList.add('popped');
        clearTimeout(failTimeout);
        balloon.remove();
        setTimeout(createBalloon, 1000); // Ne crea uno nuovo
      });
    }

    // Avvia il gioco con 5 palloncini iniziali
    for (let i = 0; i < 5; i++) {
      setTimeout(createBalloon, i * 1000);
    }
  </script>

</body>
</html>

