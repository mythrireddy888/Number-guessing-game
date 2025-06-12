# Number-guessing-game
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Innovative Number Guessing Game</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@600&display=swap');

    body {
      margin: 0;
      background: linear-gradient(135deg, #667eea, #764ba2);
      font-family: 'Poppins', sans-serif;
      color: #eee;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      min-height: 100vh;
      padding: 2rem;
      user-select: none;
    }

    h1 {
      font-size: 3rem;
      margin-bottom: 0.5rem;
      text-shadow: 0 3px 8px rgba(0,0,0,0.4);
    }

    .instructions {
      font-size: 1.1rem;
      margin-bottom: 2rem;
      color: #d0d0ff;
      text-align: center;
      max-width: 360px;
      user-select:none;
    }

    .game-container {
      background: rgba(255, 255, 255, 0.1);
      border-radius: 16px;
      padding: 2rem 2.5rem;
      width: 320px;
      box-shadow: 0 8px 20px rgba(0,0,0,0.3);
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    input[type="number"] {
      width: 100%;
      padding: 0.9rem 1rem;
      font-size: 1.25rem;
      border: none;
      border-radius: 10px;
      text-align: center;
      font-weight: 600;
      outline: none;
      box-shadow:
        inset 3px 3px 6px #6066e6,
        inset -3px -3px 6px #a0a6ff;
      margin-bottom: 1.3rem;
      transition: box-shadow 0.3s ease;
      color: #222;
    }

    input[type="number"]:focus {
      box-shadow:
        0 0 12px 3px #acb6ff,
        inset 3px 3px 6px #6066e6,
        inset -3px -3px 6px #a0a6ff;
    }

    button {
      background: #ffb347;
      border: none;
      border-radius: 14px;
      padding: 0.85rem 2rem;
      font-size: 1.2rem;
      font-weight: 700;
      cursor: pointer;
      color: #222;
      box-shadow: 0 6px 12px #e69f33;
      transition: background 0.3s ease, box-shadow 0.3s ease;
      user-select: none;
    }

    button:hover {
      background: #ffcf6b;
      box-shadow: 0 8px 18px #f1be50;
    }

    button:active {
      background: #e6a637;
      box-shadow: none;
      transform: scale(0.96);
    }

    .feedback {
      margin: 1.2rem 0 1.6rem 0;
      font-size: 1.3rem;
      height: 2.2rem;
      font-weight: 600;
      text-align: center;
      user-select:none;
      transition: color 0.3s ease;
    }

    .feedback.too-high {
      color: #ff5252;
    }
    .feedback.too-low {
      color: #42a5f5;
    }
    .feedback.correct {
      color: #64dd17;
      font-size: 1.6rem;
      text-shadow: 0 0 15px #64dd17;
    }

    .attempts-bar-container {
      width: 100%;
      height: 18px;
      background: linear-gradient(145deg, #5252ff, #2c2c8c);
      border-radius: 12px;
      overflow: hidden;
      margin-bottom: 1.5rem;
      box-shadow:
        inset 3px 3px 8px #4041c7,
        inset -3px -3px 8px #6466e1;
      user-select:none;
    }

    .attempts-bar {
      height: 100%;
      background: linear-gradient(145deg, #64dd17, #4caf50);
      width: 100%;
      transition: width 0.5s ease;
      border-radius: 12px 0 0 12px;
    }

    .game-over {
      font-weight: 700;
      color: #ff5252;
      text-align: center;
      margin-bottom: 1.2rem;
      user-select:none;
    }

    .play-again-btn {
      margin-top: 0.6rem;
      background: #64dd17;
      color: #222;
      box-shadow: 0 8px 18px #4caf50;
    }

    /* Subtle glowing circle animation behind input */
    .glow-container {
      position: relative;
      width: 100%;
      margin-bottom: 1rem;
    }
    .glow-circle {
      position: absolute;
      top: 50%;
      left: 50%;
      width: 280px;
      height: 280px;
      background: radial-gradient(circle at center, #667eea 0%, transparent 70%);
      filter: blur(35px);
      opacity: 0.4;
      transform: translate(-50%, -50%);
      pointer-events: none;
      animation: pulseGlow 4s infinite;
      border-radius: 50%;
      z-index: -1;
    }
    @keyframes pulseGlow {
      0%, 100% { opacity: 0.4; }
      50% { opacity: 0.7; }
    }

    /* Responsive */
    @media (max-width: 400px) {
      .game-container {
        width: 100%;
      }
      input[type="number"] {
        font-size: 1.1rem;
      }
      h1 {
        font-size: 2.5rem;
      }
      button {
        font-size: 1.1rem;
        padding: 0.75rem 1.6rem;
      }
    }
  </style>
</head>
<body>
  <h1>Number Guessing Game</h1>
  <p class="instructions">Guess the secret number between <strong>1</strong> and <strong>100</strong>.<br>You have <strong>7</strong> attempts. Good luck!</p>
  <div class="game-container" role="main" aria-live="polite" aria-atomic="true">
    <div class="glow-container">
      <input id="guessInput" type="number" min="1" max="100" placeholder="Enter your guess" aria-label="Enter your guess between 1 and 100" autocomplete="off" />
      <div class="glow-circle"></div>
    </div>
    <button id="submitBtn">Submit Guess</button>
    <div class="feedback" id="feedbackMsg" aria-live="assertive"></div>
    <div class="attempts-bar-container" role="progressbar" aria-valuemin="0" aria-valuemax="7" aria-valuenow="7" aria-label="Attempts left progress bar">
      <div class="attempts-bar" id="attemptsBar"></div>
    </div>
    <div id="gameOverMsg" class="game-over" role="alert" aria-live="assertive" style="display:none;"></div>
    <button id="playAgainBtn" class="play-again-btn" style="display:none;">Play Again</button>
  </div>

  <script>
    (() => {
      const maxAttempts = 7;
      let secretNumber = 0;
      let attemptsLeft = maxAttempts;
      let gameActive = true;

      const guessInput = document.getElementById('guessInput');
      const submitBtn = document.getElementById('submitBtn');
      const feedbackMsg = document.getElementById('feedbackMsg');
      const attemptsBar = document.getElementById('attemptsBar');
      const attemptsBarContainer = attemptsBar.parentElement;
      const gameOverMsg = document.getElementById('gameOverMsg');
      const playAgainBtn = document.getElementById('playAgainBtn');

      function newGame() {
        secretNumber = Math.floor(Math.random() * 100) + 1;
        attemptsLeft = maxAttempts;
        gameActive = true;
        feedbackMsg.textContent = '';
        gameOverMsg.style.display = 'none';
        playAgainBtn.style.display = 'none';
        updateAttemptsBar();
        guessInput.value = '';
        guessInput.disabled = false;
        submitBtn.disabled = false;
        guessInput.focus();
      }

      function updateAttemptsBar() {
        const widthPercent = (attemptsLeft / maxAttempts) * 100;
        attemptsBar.style.width = widthPercent + '%';
        attemptsBarContainer.setAttribute('aria-valuenow', attemptsLeft);
        // Change color by attempts left:
        if(attemptsLeft > 4) {
          attemptsBar.style.background = 'linear-gradient(145deg, #64dd17, #4caf50)';
        } else if(attemptsLeft > 2) {
          attemptsBar.style.background = 'linear-gradient(145deg, #ffb300, #ff6f00)';
        } else {
          attemptsBar.style.background = 'linear-gradient(145deg, #d50000, #b71c1c)';
        }
      }

      function playSound(type) {
        // Simple beep with Web Audio API for UX enhancement
        if(typeof AudioContext === 'undefined') return; // fallback no sound

        const ctx = new (window.AudioContext || window.webkitAudioContext)();
        const o = ctx.createOscillator();
        const g = ctx.createGain();

        o.connect(g);
        g.connect(ctx.destination);

        if(type === 'correct') {
          o.frequency.value = 880;
          g.gain.value = 0.15;
        } else if(type === 'wrong') {
          o.frequency.value = 440;
          g.gain.value = 0.12;
        } else if(type === 'gameover') {
          o.frequency.value = 220;
          g.gain.value = 0.1;
        }

        o.start(0);
        setTimeout(() => {
          o.stop();
          ctx.close();
        }, 150);
      }

      submitBtn.addEventListener('click', () => {
        if(!gameActive) return;
        const guess = parseInt(guessInput.value, 10);
        if(isNaN(guess) || guess < 1 || guess > 100) {
          feedbackMsg.textContent = 'Please enter a number between 1 and 100.';
          feedbackMsg.className = 'feedback';
          guessInput.focus();
          return;
        }

        attemptsLeft--;
        updateAttemptsBar();

        if(guess === secretNumber) {
          // Correct guess
          feedbackMsg.textContent = '🎉 Correct! You guessed the number!';
          feedbackMsg.className = 'feedback correct';
          playSound('correct');
          gameActive = false;
          guessInput.disabled = true;
          submitBtn.disabled = true;
          gameOverMsg.style.display = 'block';
          gameOverMsg.textContent = It took you ${maxAttempts - attemptsLeft} attempt${(maxAttempts - attemptsLeft) !== 1 ? 's' : ''}!;
          playAgainBtn.style.display = 'inline-block';
        } else if(attemptsLeft === 0) {
          // Out of attempts, lost
          feedbackMsg.textContent = '😢 Game Over! You ran out of attempts.';
          feedbackMsg.className = 'feedback';
          playSound('gameover');
          gameActive = false;
          guessInput.disabled = true;
          submitBtn.disabled = true;
          gameOverMsg.style.display = 'block';
          gameOverMsg.textContent = The number was ${secretNumber}. Try again!;
          playAgainBtn.style.display = 'inline-block';
        } else {
          // Wrong guess feedback
          if(guess > secretNumber) {
            feedbackMsg.textContent = '⬇ Too high! Try a lower number.';
            feedbackMsg.className = 'feedback too-high';
          } else {
            feedbackMsg.textContent = '⬆ Too low! Try a higher number.';
            feedbackMsg.className = 'feedback too-low';
          }
          playSound('wrong');
          guessInput.value = '';
          guessInput.focus();
        }
      });

      guessInput.addEventListener('keydown', (e) => {
        if(e.key === 'Enter') {
          e.preventDefault();
          submitBtn.click();
        }
      });

      playAgainBtn.addEventListener('click', () => {
        newGame();
      });

      newGame();
    })();
  </script>
</body>
</html>
