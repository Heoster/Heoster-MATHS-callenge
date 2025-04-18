<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Heoster's Math Lab Pro</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css"/>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"/>
  <style>
    :root {
      --primary: #6a11cb;
      --secondary: #2575fc;
      --bg-dark: #121212;
      --bg-light: #1e1e1e;
      --text: #f5f5f5;
      --correct: #4caf50;
      --wrong: #f44336;
      --highlight: #ffc107;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      font-family: 'Poppins', -apple-system, BlinkMacSystemFont, sans-serif;
      background: linear-gradient(135deg, var(--bg-dark) 0%, #2c3e50 100%);
      color: var(--text);
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      overflow-x: hidden;
      line-height: 1.6;
    }

    .container {
      max-width: 800px;
      margin: 2rem auto;
      background: rgba(30, 30, 30, 0.85);
      padding: 2.5rem;
      border-radius: 16px;
      box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
      text-align: center;
      position: relative;
      overflow: hidden;
      z-index: 1;
      backdrop-filter: blur(10px);
      border: 1px solid rgba(255, 255, 255, 0.1);
    }

    .container::before {
      content: '';
      position: absolute;
      top: -50%;
      left: -50%;
      width: 200%;
      height: 200%;
      background: linear-gradient(
        to bottom right,
        transparent 0%,
        rgba(106, 17, 203, 0.1) 50%,
        transparent 100%
      );
      animation: rotate 20s linear infinite;
      z-index: -1;
    }

    @keyframes rotate {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
    }

    h1 {
      font-size: 2.8rem;
      margin-bottom: 1.5rem;
      background: linear-gradient(90deg, var(--primary), var(--secondary));
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
      font-weight: 800;
      text-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
    }

    .difficulty-selector {
      display: flex;
      justify-content: center;
      gap: 1rem;
      margin-bottom: 2rem;
      flex-wrap: wrap;
    }

    .difficulty-btn {
      padding: 0.75rem 1.5rem;
      border-radius: 50px;
      border: none;
      background: rgba(255, 255, 255, 0.1);
      color: var(--text);
      cursor: pointer;
      transition: all 0.3s ease;
      font-weight: 600;
      font-size: 1rem;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .difficulty-btn i {
      font-size: 1.2rem;
    }

    .difficulty-btn.active {
      background: linear-gradient(135deg, var(--primary), var(--secondary));
      color: white;
      box-shadow: 0 4px 15px rgba(106, 17, 203, 0.4);
      transform: translateY(-2px);
    }

    .question-container {
      min-height: 140px;
      display: flex;
      align-items: center;
      justify-content: center;
      margin: 2.5rem 0;
      position: relative;
    }

    .question {
      font-size: 3rem;
      font-weight: 700;
      background: linear-gradient(90deg, var(--highlight), var(--secondary));
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
      text-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
    }

    .input-container {
      position: relative;
      margin: 2.5rem auto;
      max-width: 350px;
    }

    input {
      width: 100%;
      padding: 1.2rem 1.8rem;
      font-size: 1.5rem;
      border-radius: 12px;
      border: 2px solid rgba(255, 255, 255, 0.2);
      background: rgba(0, 0, 0, 0.3);
      color: white;
      text-align: center;
      transition: all 0.3s ease;
      font-weight: 600;
    }

    input:focus {
      outline: none;
      border-color: var(--primary);
      box-shadow: 0 0 20px rgba(106, 17, 203, 0.4);
      transform: scale(1.02);
    }

    .submit-btn {
      background: linear-gradient(135deg, var(--primary), var(--secondary));
      border: none;
      padding: 1.2rem 3rem;
      color: white;
      font-size: 1.2rem;
      font-weight: 600;
      border-radius: 12px;
      margin-top: 1.5rem;
      cursor: pointer;
      transition: all 0.3s ease;
      position: relative;
      overflow: hidden;
      box-shadow: 0 4px 15px rgba(106, 17, 203, 0.3);
    }

    .submit-btn:hover {
      transform: translateY(-3px) scale(1.03);
      box-shadow: 0 8px 25px rgba(106, 17, 203, 0.4);
    }

    .submit-btn:active {
      transform: translateY(1px);
    }

    .stats {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 1.5rem;
      margin-top: 3rem;
      text-align: center;
    }

    .stat-card {
      background: rgba(0, 0, 0, 0.3);
      padding: 1.5rem 1rem;
      border-radius: 12px;
      border-top: 3px solid var(--primary);
      transition: all 0.3s ease;
    }

    .stat-card:hover {
      transform: translateY(-5px);
      box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
    }

    .stat-value {
      font-size: 1.8rem;
      font-weight: 700;
      margin-top: 0.5rem;
      background: linear-gradient(90deg, var(--primary), var(--secondary));
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
    }

    .settings-bar {
      position: absolute;
      top: 20px;
      right: 20px;
      display: flex;
      gap: 10px;
    }

    .settings-btn {
      background: rgba(255, 255, 255, 0.1);
      border: none;
      width: 40px;
      height: 40px;
      border-radius: 50%;
      color: var(--text);
      cursor: pointer;
      transition: all 0.3s ease;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 1.2rem;
    }

    .settings-btn:hover {
      background: var(--primary);
      transform: rotate(15deg) scale(1.1);
    }

    footer {
      margin-top: auto;
      padding: 1.5rem;
      text-align: center;
      font-size: 1rem;
      background: rgba(0, 0, 0, 0.3);
      color: var(--text);
      backdrop-filter: blur(5px);
    }

    footer a {
      color: var(--highlight);
      text-decoration: none;
      font-weight: 600;
      transition: all 0.3s ease;
      display: inline-flex;
      align-items: center;
      gap: 5px;
    }

    footer a:hover {
      color: var(--secondary);
      text-shadow: 0 0 10px rgba(37, 117, 252, 0.5);
    }

    /* Animations */
    @keyframes pulse {
      0% { transform: scale(1); }
      50% { transform: scale(1.05); }
      100% { transform: scale(1); }
    }

    @keyframes shake {
      10%, 90% { transform: translateX(-2px); }
      20%, 80% { transform: translateX(4px); }
      30%, 50%, 70% { transform: translateX(-8px); }
      40%, 60% { transform: translateX(8px); }
    }

    /* Confetti */
    .confetti {
      position: fixed;
      width: 15px;
      height: 15px;
      background-color: var(--highlight);
      opacity: 0;
      z-index: 1000;
      animation: confetti-fall 3s ease-in-out forwards;
    }

    @keyframes confetti-fall {
      0% {
        transform: translateY(-100px) rotate(0deg);
        opacity: 1;
      }
      100% {
        transform: translateY(100vh) rotate(360deg);
        opacity: 0;
      }
    }

    /* Progress ring */
    .progress-ring {
      position: relative;
      width: 100px;
      height: 100px;
      margin: 20px auto;
    }

    .progress-ring__circle {
      transform: rotate(-90deg);
      transform-origin: 50% 50%;
      stroke-dasharray: 251;
      stroke-dashoffset: 251;
      transition: stroke-dashoffset 0.5s;
      stroke: var(--primary);
      stroke-width: 8;
      fill: transparent;
    }

    /* Responsive */
    @media (max-width: 768px) {
      .container {
        margin: 1rem;
        padding: 1.5rem;
      }

      h1 {
        font-size: 2.2rem;
      }

      .question {
        font-size: 2.2rem;
      }

      .stats {
        grid-template-columns: repeat(2, 1fr);
      }

      .difficulty-btn {
        padding: 0.6rem 1.2rem;
        font-size: 0.9rem;
      }
    }

    @media (max-width: 480px) {
      .question {
        font-size: 1.8rem;
      }

      input {
        font-size: 1.2rem;
        padding: 1rem;
      }

      .submit-btn {
        padding: 1rem 2rem;
        font-size: 1rem;
      }

      .stat-card {
        padding: 1rem 0.5rem;
      }

      .stat-value {
        font-size: 1.5rem;
      }
    }
  </style>
</head>
<body>
  <div class="container animate__animated animate__fadeIn">
    <div class="settings-bar">
      <button class="settings-btn" id="soundToggle" title="Toggle sound">
        <i class="fas fa-volume-up"></i>
      </button>
      <button class="settings-btn" id="timerToggle" title="Timer mode">
        <i class="fas fa-stopwatch"></i>
      </button>
    </div>
    
    <h1>Heoster's Math Lab Pro</h1>
    
    <div class="difficulty-selector">
      <button class="difficulty-btn active" data-difficulty="easy">
        <i class="fas fa-smile"></i> Easy
      </button>
      <button class="difficulty-btn" data-difficulty="medium">
        <i class="fas fa-meh"></i> Medium
      </button>
      <button class="difficulty-btn" data-difficulty="hard">
        <i class="fas fa-frown"></i> Hard
      </button>
    </div>
    
    <div class="timer-display" id="timerDisplay" style="display: none;">
      <svg class="progress-ring" width="100" height="100">
        <circle class="progress-ring__circle" r="40" cx="50" cy="50"/>
      </svg>
      <div id="timerValue">30</div>
    </div>
    
    <div class="question-container">
      <p class="question" id="question">5 + 7 = ?</p>
    </div>
    
    <div class="input-container">
      <input type="text" id="answer" placeholder="Your answer..." autocomplete="off" />
    </div>
    
    <button class="submit-btn" id="submitBtn">
      <i class="fas fa-calculator"></i> SOLVE
    </button>
    
    <div class="stats">
      <div class="stat-card">
        <p><i class="fas fa-check-circle"></i> Correct</p>
        <p class="stat-value" id="correct">0</p>
      </div>
      <div class="stat-card">
        <p><i class="fas fa-list-ol"></i> Total</p>
        <p class="stat-value" id="total">0</p>
      </div>
      <div class="stat-card">
        <p><i class="fas fa-bullseye"></i> Accuracy</p>
        <p class="stat-value" id="accuracy">0%</p>
      </div>
      <div class="stat-card">
        <p><i class="fas fa-fire"></i> Streak</p>
        <p class="stat-value" id="streak">0</p>
      </div>
    </div>
  </div>

  <footer>
    <a href="https://www.instagram.com/codex._.heoster?igsh=YzljYTk1ODg3Zg==" target="_blank">
      <i class="fab fa-instagram"></i> Follow on Instagram
    </a> | Heoster Math Lab Pro © 2025
  </footer>

  <!-- Audio Elements -->
  <audio id="correctSound" src="https://assets.mixkit.co/sfx/preview/mixkit-correct-answer-tone-2870.mp3" preload="auto"></audio>
  <audio id="wrongSound" src="https://assets.mixkit.co/sfx/preview/mixkit-wrong-answer-fail-notification-946.mp3" preload="auto"></audio>

  <script>
    // DOM Elements
    const questionEl = document.getElementById('question');
    const answerEl = document.getElementById('answer');
    const correctEl = document.getElementById('correct');
    const totalEl = document.getElementById('total');
    const accuracyEl = document.getElementById('accuracy');
    const streakEl = document.getElementById('streak');
    const submitBtn = document.getElementById('submitBtn');
    const difficultyBtns = document.querySelectorAll('.difficulty-btn');
    const soundToggle = document.getElementById('soundToggle');
    const timerToggle = document.getElementById('timerToggle');
    const timerDisplay = document.getElementById('timerDisplay');
    const timerValue = document.getElementById('timerValue');
    const progressCircle = document.querySelector('.progress-ring__circle');
    const correctSound = document.getElementById('correctSound');
    const wrongSound = document.getElementById('wrongSound');

    // Game state
    let stats = {
      correct: 0,
      total: 0,
      streak: 0,
      highScore: 0
    };
    
    let currentAnswer = 0;
    let difficulty = 'easy';
    let isAnimating = false;
    let soundEnabled = true;
    let timerEnabled = false;
    let timeLeft = 30;
    let timerInterval;
    let circumference = 2 * Math.PI * 40;

    // Initialize
    generateQuestion();
    answerEl.focus();
    loadStats();

    // Event Listeners
    submitBtn.addEventListener('click', checkAnswer);
    answerEl.addEventListener('keypress', (e) => {
      if (e.key === 'Enter') checkAnswer();
    });

    difficultyBtns.forEach(btn => {
      btn.addEventListener('click', () => {
        difficultyBtns.forEach(b => b.classList.remove('active'));
        btn.classList.add('active');
        difficulty = btn.dataset.difficulty;
        generateQuestion();
      });
    });

    soundToggle.addEventListener('click', toggleSound);
    timerToggle.addEventListener('click', toggleTimer);

    // Functions
    function generateQuestion() {
      if (isAnimating) return;
      
      let a, b;
      const op = Math.random() > 0.5 ? '+' : '-';
      
      switch(difficulty) {
        case 'easy':
          a = Math.floor(Math.random() * 20) + 1;
          b = Math.floor(Math.random() * 20) + 1;
          break;
        case 'medium':
          a = Math.floor(Math.random() * 100) + 10;
          b = Math.floor(Math.random() * 100) + 10;
          break;
        case 'hard':
          a = Math.floor(Math.random() * 500) + 100;
          b = Math.floor(Math.random() * 500) + 100;
          break;
      }
      
      currentAnswer = op === '+' ? a + b : a - b;
      
      // Animation
      isAnimating = true;
      questionEl.classList.add('animate__animated', 'animate__fadeOut');
      
      setTimeout(() => {
        questionEl.textContent = `${a} ${op} ${b} = ?`;
        questionEl.classList.remove('animate__fadeOut');
        questionEl.classList.add('animate__fadeIn');
        
        setTimeout(() => {
          questionEl.classList.remove('animate__fadeIn');
          isAnimating = false;
        }, 500);
      }, 300);
    }

    function checkAnswer() {
      if (isAnimating) return;
      
      const userInput = answerEl.value.trim();
      
      if (!userInput.match(/^-?\d+$/)) {
        answerEl.classList.add('animate__animated', 'animate__shakeX');
        setTimeout(() => answerEl.classList.remove('animate__shakeX'), 500);
        return;
      }

      const userAns = parseInt(userInput);
      stats.total++;
      
      if (userAns === currentAnswer) {
        // Correct answer
        stats.correct++;
        stats.streak++;
        questionEl.style.color = 'var(--correct)';
        questionEl.classList.add('animate__animated', 'animate__tada');
        
        if (soundEnabled) {
          correctSound.currentTime = 0;
          correctSound.play();
        }
        
        if (stats.streak > stats.highScore) {
          stats.highScore = stats.streak;
        }
        
        if (stats.streak % 5 === 0) {
          createConfetti();
        }
      } else {
        // Wrong answer
        stats.streak = 0;
        questionEl.style.color = 'var(--wrong)';
        questionEl.classList.add('animate__animated', 'animate__shakeX');
        
        if (soundEnabled) {
          wrongSound.currentTime = 0;
          wrongSound.play();
        }
      }

      updateStats();
      
      setTimeout(() => {
        questionEl.style.color = '';
        questionEl.classList.remove('animate__tada', 'animate__shakeX');
        answerEl.value = '';
        generateQuestion();
        answerEl.focus();
      }, 1000);
    }

    function updateStats() {
      correctEl.textContent = stats.correct;
      totalEl.textContent = stats.total;
      accuracyEl.textContent = stats.total > 0 
        ? `${Math.round((stats.correct / stats.total) * 100)}%` 
        : '0%';
      streakEl.textContent = stats.streak;
      
      // Pulse animation for streak
      if (stats.streak > 3) {
        streakEl.classList.add('animate__animated', 'animate__pulse');
        setTimeout(() => {
          streakEl.classList.remove('animate__pulse');
        }, 1000);
      }
      
      saveStats();
    }

    function toggleSound() {
      soundEnabled = !soundEnabled;
      soundToggle.innerHTML = soundEnabled ? '<i class="fas fa-volume-up"></i>' : '<i class="fas fa-volume-mute"></i>';
      soundToggle.title = soundEnabled ? 'Sound: ON' : 'Sound: OFF';
    }

    function toggleTimer() {
      timerEnabled = !timerEnabled;
      timerDisplay.style.display = timerEnabled ? 'block' : 'none';
      timerToggle.innerHTML = timerEnabled ? '<i class="fas fa-stopwatch-20"></i>' : '<i class="fas fa-stopwatch"></i>';
      timerToggle.title = timerEnabled ? 'Timer: ON' : 'Timer: OFF';
      
      if (timerEnabled) {
        startTimer();
      } else {
        clearInterval(timerInterval);
      }
    }

    function startTimer() {
      clearInterval(timerInterval);
      timeLeft = 30;
      updateTimerDisplay();
      
      timerInterval = setInterval(() => {
        timeLeft--;
        updateTimerDisplay();
        
        if (timeLeft <= 0) {
          clearInterval(timerInterval);
          alert(`Time's up! You solved ${stats.correct} problems with ${Math.round((stats.correct/stats.total)*100)}% accuracy!`);
          timerEnabled = false;
          timerDisplay.style.display = 'none';
          timerToggle.innerHTML = '<i class="fas fa-stopwatch"></i>';
        }
      }, 1000);
    }

    function updateTimerDisplay() {
      timerValue.textContent = timeLeft;
      const offset = circumference - (timeLeft / 30) * circumference;
      progressCircle.style.strokeDashoffset = offset;
    }

    function createConfetti() {
      for (let i = 0; i < 50; i++) {
        const confetti = document.createElement('div');
        confetti.classList.add('confetti');
        confetti.style.left = `${Math.random() * 100}vw`;
        confetti.style.backgroundColor = `hsl(${Math.random() * 360}, 100%, 50%)`;
        confetti.style.width = `${Math.random() * 10 + 5}px`;
        confetti.style.height = `${Math.random() * 10 + 5}px`;
        confetti.style.animationDuration = `${Math.random() * 2 + 1}s`;
        confetti.style.animationDelay = `${Math.random() * 0.5}s`;
        document.body.appendChild(confetti);
        
        setTimeout(() => confetti.remove(), 3000);
      }
    }

    function saveStats() {
      localStorage.setItem('mathLabStats', JSON.stringify(stats));
    }

    function loadStats() {
      const savedStats = localStorage.getItem('mathLabStats');
      if (savedStats) {
        stats = JSON.parse(savedStats);
        updateStats();
      }
    }
  </script>
</body>
</html>
