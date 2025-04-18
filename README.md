<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Advanced Math Challenge</title>
  <style>
    * {
      box-sizing: border-box;
    }
    body {
      font-family: 'Courier New', monospace;
      background: linear-gradient(135deg, #1e3c72, #2a5298);
      color: white;
      text-align: center;
      padding: 20px;
      margin: 0;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
    }
    .container {
      background-color: rgba(0, 0, 0, 0.7);
      border-radius: 15px;
      padding: 20px;
      max-width: 600px;
      margin: auto;
      box-shadow: 0 0 20px rgba(0, 0, 0, 0.5);
    }
    h1 {
      color: #4fc3f7;
      border-bottom: 2px solid #4fc3f7;
      padding-bottom: 10px;
    }
    .question {
      font-size: 1.8em;
      margin: 20px 0;
      color: #f48fb1;
    }
    input {
      padding: 10px;
      font-size: 1.2em;
      width: 150px;
      text-align: center;
      margin: 10px;
      border: 2px solid #4fc3f7;
      border-radius: 5px;
      background-color: rgba(255, 255, 255, 0.1);
      color: white;
    }
    button {
      background-color: #4fc3f7;
      color: black;
      border: none;
      padding: 10px 20px;
      margin: 5px;
      border-radius: 5px;
      cursor: pointer;
      font-weight: bold;
      transition: all 0.3s;
    }
    button:hover {
      background-color: #f48fb1;
      transform: scale(1.05);
    }
    .stats {
      margin-top: 20px;
      padding: 15px;
      background-color: rgba(0, 0, 0, 0.3);
      border-radius: 10px;
      text-align: left;
    }
    .correct { color: #81c784; }
    .incorrect { color: #ff8a65; }
    .streak { color: #ffd54f; }
    .timer { color: #4fc3f7; }
    .difficulty-selector {
      margin: 15px 0;
      display: flex;
      justify-content: center;
      flex-wrap: wrap;
      gap: 10px;
    }
    .difficulty-btn {
      padding: 8px 15px;
      background-color: #2a5298;
      border-radius: 5px;
    }
    .difficulty-btn.active {
      background-color: #4fc3f7;
      color: black;
    }
    footer {
      background: rgba(0, 0, 0, 0.3);
      padding: 15px;
      text-align: center;
      font-size: 0.9em;
      margin-top: auto;
    }
    footer a {
      color: #4fc3f7;
      text-decoration: none;
      font-weight: bold;
    }
    footer a:hover {
      text-decoration: underline;
      color: #f48fb1;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>ADVANCED MATH CHALLENGE</h1>
    <p>Solve problems with ones, tens, and hundreds!</p>
    <div class="difficulty-selector">
      <button id="onesBtn" class="difficulty-btn" onclick="setDifficulty('ones')">Ones (1–9)</button>
      <button id="tensBtn" class="difficulty-btn active" onclick="setDifficulty('tens')">Tens (10–90)</button>
      <button id="hundredsBtn" class="difficulty-btn" onclick="setDifficulty('hundreds')">Hundreds (100–900)</button>
      <button id="mixedBtn" class="difficulty-btn" onclick="setDifficulty('mixed')">Mixed</button>
    </div>
    <div class="question" id="question"></div>
    <input type="text" id="answer" placeholder="Your answer">
    <div>
      <button onclick="checkAnswer()">Submit</button>
      <button onclick="newQuestion()">New</button>
      <button onclick="leaveQuestion()">Skip</button>
      <button onclick="showStats()">Stats</button>
      <button onclick="toggleTimer()" id="timerBtn">Enable Timer</button>
    </div>
    <div class="stats" id="stats" style="display:none;">
      <h3>Statistics</h3>
      <p id="correct">Correct: 0</p>
      <p id="total">Total: 0</p>
      <p id="accuracy">Accuracy: 0%</p>
      <p id="streak">Current streak: 0</p>
      <p id="maxStreak">Max streak: 0</p>
      <p id="timeElapsed">Time elapsed: 0 seconds</p>
      <p id="speed">Speed: 0 questions/minute</p>
      <p id="leftQuestions">Questions skipped: 0</p>
    </div>
  </div>

  <footer>
    Follow me on Instagram: 
    <a href="https://www.instagram.com/codex._.heoster?igsh=YzljYTk1ODg3Zg==" target="_blank">
      @codex._.heoster
    </a>
  </footer>

  <script>
    const state = {
      correct: 0,
      total: 0,
      streak: 0,
      maxStreak: 0,
      timedMode: false,
      startTime: null,
      questionStart: null,
      currentAnswer: null,
      difficulty: 'tens',
      leftQuestions: 0
    };

    const questionEl = document.getElementById('question');
    const answerEl = document.getElementById('answer');
    const statsEl = document.getElementById('stats');
    const timerBtn = document.getElementById('timerBtn');

    document.addEventListener('DOMContentLoaded', () => {
      newQuestion();
      answerEl.addEventListener('keypress', (e) => {
        if (e.key === 'Enter') checkAnswer();
      });
    });

    function setDifficulty(level) {
      state.difficulty = level;
      document.querySelectorAll('.difficulty-btn').forEach(btn => btn.classList.remove('active'));
      document.getElementById(`${level}Btn`).classList.add('active');
      newQuestion();
    }

    function generateNumber() {
      const ranges = {
        ones: [1, 9],
        tens: [10, 90, 10],
        hundreds: [100, 900, 100],
        mixed: [1, 900]
      };
      const [min, max, step] = ranges[state.difficulty];
      if (state.difficulty === 'mixed') {
        const rand = Math.random();
        if (rand < 0.3) return Math.floor(Math.random() * 9 + 1) * 100;
        if (rand < 0.8) return Math.floor(Math.random() * 9 + 1) * 10;
        return Math.floor(Math.random() * 9) + 1;
      }
      return step ? Math.floor(Math.random() * ((max - min) / step + 1)) * step + min :
                    Math.floor(Math.random() * (max - min + 1)) + min;
    }

    function newQuestion() {
      const numOps = Math.random() > 0.3 ? 2 : 3;
      let expr = `${generateNumber()}`;
      let result = eval(expr);

      for (let i = 0; i < numOps; i++) {
        const op = Math.random() > 0.5 ? '+' : '-';
        const val = generateNumber();
        expr += ` ${op} ${val}`;
        result = eval(expr);
      }

      questionEl.textContent = `${expr} = ?`;
      state.currentAnswer = Math.round(result);
      state.questionStart = Date.now();
      answerEl.value = '';
      answerEl.focus();
    }

    function checkAnswer() {
      const userInput = answerEl.value.trim();
      if (!userInput.match(/^-?\d+$/)) {
        alert('Please enter a valid integer');
        return;
      }

      const userAnswer = parseInt(userInput);
      state.total++;
      const isCorrect = userAnswer === state.currentAnswer;

      if (isCorrect) {
        state.correct++;
        state.streak++;
        state.maxStreak = Math.max(state.maxStreak, state.streak);
        questionEl.style.color = '#81c784';
      } else {
        state.streak = 0;
        questionEl.style.color = '#ff8a65';
        alert(`Incorrect. Correct answer: ${state.currentAnswer}`);
      }

      setTimeout(() => questionEl.style.color = '#f48fb1', 600);

      if (state.total % 3 === 0) showStats();
      newQuestion();
    }

    function leaveQuestion() {
      state.leftQuestions++;
      alert(`Correct answer was: ${state.currentAnswer}`);
      newQuestion();
    }

    function toggleTimer() {
      state.timedMode = !state.timedMode;
      timerBtn.textContent = state.timedMode ? 'Disable Timer' : 'Enable Timer';
      if (state.timedMode && !state.startTime) {
        state.startTime = Date.now();
      }
    }

    function showStats() {
      const elapsed = state.startTime ? (Date.now() - state.startTime) / 1000 : 0;
      const accuracy = state.total > 0 ? ((state.correct / state.total) * 100).toFixed(1) : 0;
      const speed = elapsed ? ((state.total / elapsed) * 60).toFixed(1) : 0;

      document.getElementById('correct').textContent = `Correct: ${state.correct}`;
      document.getElementById('total').textContent = `Total: ${state.total}`;
      document.getElementById('accuracy').textContent = `Accuracy: ${accuracy}%`;
      document.getElementById('streak').textContent = `Current streak: ${state.streak}`;
      document.getElementById('maxStreak').textContent = `Max streak: ${state.maxStreak}`;
      document.getElementById('timeElapsed').textContent = `Time elapsed: ${elapsed.toFixed(1)} seconds`;
      document.getElementById('speed').textContent = `Speed: ${speed} questions/minute`;
      document.getElementById('leftQuestions').textContent = `Questions skipped: ${state.leftQuestions}`;

      statsEl.style.display = 'block';
    }
  </script>
</body>
</html>
