<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Ultimate Math Challenge</title>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap" rel="stylesheet">
  <style>
    body {
      font-family: 'Poppins', sans-serif;
      background: linear-gradient(135deg, #0f172a, #1e293b);
      color: #fff;
      margin: 0;
      padding: 0;
      display: flex;
      min-height: 100vh;
      align-items: center;
      justify-content: center;
    }
    .container {
      max-width: 500px;
      background: #1e293b;
      padding: 30px;
      border-radius: 16px;
      box-shadow: 0 8px 24px rgba(0,0,0,0.4);
    }
    h1 {
      text-align: center;
      font-size: 1.8rem;
      margin-bottom: 10px;
    }
    p {
      text-align: center;
      font-size: 0.95rem;
      color: #94a3b8;
      margin-bottom: 20px;
    }
    .selectors {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 10px;
      margin-bottom: 15px;
    }
    .selectors button {
      padding: 8px 14px;
      border: none;
      background: #334155;
      color: #fff;
      border-radius: 8px;
      cursor: pointer;
    }
    .selectors button.active {
      background: #4f46e5;
    }
    .question {
      font-size: 1.4rem;
      text-align: center;
      margin-bottom: 15px;
    }
    input[type="text"] {
      width: 100%;
      padding: 10px;
      font-size: 1rem;
      border-radius: 8px;
      border: none;
      margin-bottom: 15px;
    }
    .btn-group {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      justify-content: center;
    }
    .btn-group button {
      padding: 10px 16px;
      border: none;
      border-radius: 8px;
      background: #4f46e5;
      color: #fff;
      font-weight: bold;
      cursor: pointer;
    }
    .stats {
      margin-top: 25px;
      background: #0f172a;
      padding: 15px;
      border-radius: 12px;
      display: none;
    }
    .stats h3 {
      margin-top: 0;
      text-align: center;
    }
    .stats p {
      font-size: 0.9rem;
      margin: 6px 0;
      color: #cbd5e1;
    }
    .timer {
      text-align: center;
      margin: 10px 0;
      font-size: 1.1rem;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Ultimate Math Challenge</h1>
    <p>Fast & Smart Math Game. Commands: <code>quit</code>, <code>new</code>, <code>stats</code></p>

    <div class="selectors">
      <button onclick="setDifficulty('ones')" id="onesBtn">Ones</button>
      <button onclick="setDifficulty('tens')" id="tensBtn" class="active">Tens</button>
      <button onclick="setDifficulty('hundreds')" id="hundredsBtn">Hundreds</button>
      <button onclick="setDifficulty('mixed')" id="mixedBtn">Mixed</button>
    </div>
    <div class="selectors">
      <button onclick="setOperation('add')" id="addBtn" class="active">+</button>
      <button onclick="setOperation('sub')" id="subBtn">-</button>
      <button onclick="setOperation('mul')" id="mulBtn">×</button>
      <button onclick="startTimerMode()">60s Mode</button>
    </div>

    <div class="timer" id="timer">Time: ∞</div>
    <div class="question" id="question">Loading...</div>
    <input type="text" id="answer" placeholder="Type your answer..." onkeypress="handleKey(event)">

    <div class="btn-group">
      <button onclick="checkAnswer()">Submit</button>
      <button onclick="newQuestion()">New</button>
      <button onclick="skipQuestion()">Skip</button>
      <button onclick="toggleStats()">Stats</button>
      <button onclick="resetStats()">Reset</button>
    </div>

    <div class="stats" id="stats">
      <h3>Stats</h3>
      <p id="statCorrect">Correct: 0</p>
      <p id="statTotal">Total Attempts: 0</p>
      <p id="statAccuracy">Accuracy: 0%</p>
      <p id="statStreak">Streak: 0</p>
      <p id="statMaxStreak">Max Streak: 0</p>
      <p id="statSkipped">Skipped: 0</p>
    </div>
  </div>

  <audio id="correctSound" src="https://cdn.pixabay.com/download/audio/2022/03/15/audio_78e1642f78.mp3?filename=correct-answer-2-96147.mp3"></audio>
  <audio id="wrongSound" src="https://cdn.pixabay.com/download/audio/2022/03/15/audio_44ef83a299.mp3?filename=wrong-answer-126516.mp3"></audio>

  <script>
    let num1, num2, answer, difficulty = 'tens', operation = 'add';
    let total = +localStorage.getItem("total") || 0;
    let correct = +localStorage.getItem("correct") || 0;
    let streak = +localStorage.getItem("streak") || 0;
    let maxStreak = +localStorage.getItem("maxStreak") || 0;
    let skipped = +localStorage.getItem("skipped") || 0;
    let timer = null, timerCount = null;

    const qEl = document.getElementById("question");
    const aEl = document.getElementById("answer");

    function setDifficulty(diff) {
      difficulty = diff;
      document.querySelectorAll('.selectors button').forEach(btn => btn.classList.remove('active'));
      document.getElementById(diff + "Btn").classList.add('active');
      newQuestion();
    }

    function setOperation(op) {
      operation = op;
      document.getElementById("addBtn").classList.remove("active");
      document.getElementById("subBtn").classList.remove("active");
      document.getElementById("mulBtn").classList.remove("active");
      document.getElementById(op + "Btn").classList.add("active");
      newQuestion();
    }

    function generateRandom(range) {
      return Math.floor(Math.random() * range);
    }

    function newQuestion() {
      let range = difficulty === 'hundreds' ? 1000 : difficulty === 'tens' ? 100 : 10;
      if (difficulty === 'mixed') {
        const options = [10, 100, 1000];
        range = options[Math.floor(Math.random() * options.length)];
      }
      num1 = generateRandom(range);
      num2 = generateRandom(range);

      if (operation === 'sub' && num2 > num1) [num1, num2] = [num2, num1];
      if (operation === 'add') answer = num1 + num2;
      else if (operation === 'sub') answer = num1 - num2;
      else answer = num1 * num2;

      let symbol = operation === 'add' ? '+' : operation === 'sub' ? '-' : '×';
      qEl.textContent = `${num1} ${symbol} ${num2} = ?`;
      aEl.value = '';
      aEl.focus();
    }

    function checkAnswer() {
      const input = aEl.value.trim().toLowerCase();
      if (input === 'quit') return alert("Thanks for playing!");
      if (input === 'new') return newQuestion();
      if (input === 'stats') return toggleStats(true);

      total++;
      if (parseInt(input) === answer) {
        correct++;
        streak++;
        maxStreak = Math.max(streak, maxStreak);
        document.getElementById("correctSound").play();
        newQuestion();
      } else {
        document.getElementById("wrongSound").play();
        alert(`Wrong! Correct answer: ${answer}`);
        streak = 0;
        newQuestion();
      }
      saveStats();
      updateStats();
    }

    function skipQuestion() {
      skipped++;
      streak = 0;
      saveStats();
      newQuestion();
      updateStats();
    }

    function handleKey(e) {
      if (e.key === "Enter") checkAnswer();
    }

    function toggleStats(force = false) {
      const box = document.getElementById("stats");
      if (force || box.style.display === "none") {
        box.style.display = "block";
        updateStats();
      } else {
        box.style.display = "none";
      }
    }

    function updateStats() {
      const acc = total > 0 ? ((correct / total) * 100).toFixed(1) : 0;
      document.getElementById("statCorrect").textContent = `Correct: ${correct}`;
      document.getElementById("statTotal").textContent = `Total Attempts: ${total}`;
      document.getElementById("statAccuracy").textContent = `Accuracy: ${acc}%`;
      document.getElementById("statStreak").textContent = `Streak: ${streak}`;
      document.getElementById("statMaxStreak").textContent = `Max Streak: ${maxStreak}`;
      document.getElementById("statSkipped").textContent = `Skipped: ${skipped}`;
    }

    function saveStats() {
      localStorage.setItem("total", total);
      localStorage.setItem("correct", correct);
      localStorage.setItem("streak", streak);
      localStorage.setItem("maxStreak", maxStreak);
      localStorage.setItem("skipped", skipped);
    }

    function resetStats() {
      if (!confirm("Reset all stats?")) return;
      total = correct = streak = maxStreak = skipped = 0;
      saveStats();
      updateStats();
    }

    function startTimerMode() {
      clearInterval(timer);
      timerCount = 60;
      document.getElementById("timer").textContent = `Time: ${timerCount}s`;
      timer = setInterval(() => {
        timerCount--;
        document.getElementById("timer").textContent = `Time: ${timerCount}s`;
        if (timerCount <= 0) {
          clearInterval(timer);
          alert(`Time's up! You got ${correct} correct out of ${total}.`);
        }
      }, 1000);
      newQuestion();
    }

    // Init
    updateStats();
    newQuestion();
  </script>
</body>
</html>
