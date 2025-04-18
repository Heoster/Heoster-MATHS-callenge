<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Neon Math Lab</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css"/>
  <style>
    :root {
      --primary: #00f7ff;
      --secondary: #ff00e4;
      --bg-dark: #0a0a14;
      --bg-light: #1a1a2e;
      --text: #e2e2e2;
      --correct: #00ff88;
      --wrong: #ff3860;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
      background: var(--bg-dark);
      color: var(--text);
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      overflow-x: hidden;
    }

    .container {
      max-width: 700px;
      margin: 2rem auto;
      background: var(--bg-light);
      padding: 2.5rem;
      border-radius: 1rem;
      box-shadow: 0 0 2rem rgba(0, 0, 0, 0.3);
      text-align: center;
      position: relative;
      overflow: hidden;
      z-index: 1;
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
        rgba(var(--primary), 0.1) 50%,
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
      font-size: 2.5rem;
      margin-bottom: 1rem;
      background: linear-gradient(90deg, var(--primary), var(--secondary));
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
      font-weight: 800;
    }

    .difficulty-selector {
      display: flex;
      justify-content: center;
      gap: 1rem;
      margin-bottom: 1.5rem;
    }

    .difficulty-btn {
      padding: 0.5rem 1rem;
      border-radius: 2rem;
      border: none;
      background: rgba(255, 255, 255, 0.1);
      color: var(--text);
      cursor: pointer;
      transition: all 0.3s ease;
      font-weight: 600;
    }

    .difficulty-btn.active {
      background: var(--primary);
      color: var(--bg-dark);
      box-shadow: 0 0 1rem rgba(0, 247, 255, 0.5);
    }

    .question-container {
      min-height: 120px;
      display: flex;
      align-items: center;
      justify-content: center;
      margin: 2rem 0;
      position: relative;
    }

    .question {
      font-size: 2.5rem;
      font-weight: 700;
      background: linear-gradient(90deg, var(--primary), var(--secondary));
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
    }

    .input-container {
      position: relative;
      margin: 2rem auto;
      max-width: 300px;
    }

    input {
      width: 100%;
      padding: 1rem 1.5rem;
      font-size: 1.2rem;
      border-radius: 0.5rem;
      border: 2px solid rgba(255, 255, 255, 0.2);
      background: rgba(0, 0, 0, 0.3);
      color: white;
      text-align: center;
      transition: all 0.3s ease;
    }

    input:focus {
      outline: none;
      border-color: var(--primary);
      box-shadow: 0 0 1rem rgba(0, 247, 255, 0.3);
    }

    .submit-btn {
      background: linear-gradient(45deg, var(--primary), var(--secondary));
      border: none;
      padding: 1rem 2.5rem;
      color: white;
      font-size: 1.1rem;
      font-weight: 600;
      border-radius: 0.5rem;
      margin-top: 1rem;
      cursor: pointer;
      transition: all 0.3s ease;
      position: relative;
      overflow: hidden;
    }

    .submit-btn::after {
      content: '';
      position: absolute;
      top: -50%;
      left: -60%;
      width: 200%;
      height: 200%;
      background: linear-gradient(
        to right,
        rgba(255, 255, 255, 0) 0%,
        rgba(255, 255, 255, 0.1) 50%,
        rgba(255, 255, 255, 0) 100%
      );
      transform: rotate(30deg);
      transition: all 0.3s;
    }

    .submit-btn:hover {
      transform: translateY(-3px);
      box-shadow: 0 1rem 2rem rgba(0, 0, 0, 0.3);
    }

    .submit-btn:hover::after {
      left: 100%;
    }

    .stats {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 1rem;
      margin-top: 2rem;
      text-align: center;
    }

    .stat-card {
      background: rgba(0, 0, 0, 0.3);
      padding: 1rem;
      border-radius: 0.5rem;
      border-left: 3px solid var(--primary);
    }

    .stat-value {
      font-size: 1.5rem;
      font-weight: 700;
      margin-top: 0.5rem;
      background: linear-gradient(90deg, var(--primary), var(--secondary));
      -webkit-background-clip: text;
      background-clip: text;
      color: transparent;
    }

    footer {
      margin-top: auto;
      padding: 1.5rem;
      text-align: center;
      font-size: 0.9rem;
      background: rgba(0, 0, 0, 0.3);
      color: var(--text);
    }

    footer a {
      color: var(--primary);
      text-decoration: none;
      font-weight: 600;
      transition: all 0.3s ease;
    }

    footer a:hover {
      color: var(--secondary);
      text-shadow: 0 0 0.5rem rgba(0, 247, 255, 0.5);
    }

    /* Animations */
    .pulse {
      animation: pulse 1.5s infinite;
    }

    @keyframes pulse {
      0% { transform: scale(1); }
      50% { transform: scale(1.05); }
      100% { transform: scale(1); }
    }

    .shake {
      animation: shake 0.5s cubic-bezier(.36,.07,.19,.97) both;
    }

    @keyframes shake {
      10%, 90% { transform: translateX(-2px); }
      20%, 80% { transform: translateX(4px); }
      30%, 50%, 70% { transform: translateX(-8px); }
      40%, 60% { transform: translateX(8px); }
    }

    /* Floating particles */
    .particles {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: -1;
      overflow: hidden;
    }

    .particle {
      position: absolute;
      background: rgba(255, 255, 255, 0.5);
      border-radius: 50%;
      filter: blur(1px);
      animation: float linear infinite;
    }

    @keyframes float {
      0% { transform: translateY(0) rotate(0deg); opacity: 1; }
      100% { transform: translateY(-100vh) rotate(360deg); opacity: 0; }
    }

    /* Responsive */
    @media (max-width: 768px) {
      .container {
        margin: 1rem;
        padding: 1.5rem;
      }

      h1 {
        font-size: 2rem;
      }

      .question {
        font-size: 2rem;
      }

      .stats {
        grid-template-columns: 1fr;
      }
    }
  </style>
</head>
<body>
  <div class="particles" id="particles"></div>

  <div class="container animate__animated animate__fadeIn">
    <h1>Heoster Math Lab</h1>
    
    <div class="difficulty-selector">
      <button class="difficulty-btn active" data-difficulty="easy">Easy</button>
      <button class="difficulty-btn" data-difficulty="medium">Medium</button>
      <button class="difficulty-btn" data-difficulty="hard">Hard</button>
    </div>
    
    <div class="question-container">
      <p class="question" id="question">5 + 7 = ?</p>
    </div>
    
    <div class="input-container">
      <input type="text" id="answer" placeholder="Your answer..." autocomplete="off" />
    </div>
    
    <button class="submit-btn pulse" id="submitBtn">SOLVE</button>
    
    <div class="stats">
      <div class="stat-card">
        <p>Correct</p>
        <p class="stat-value" id="correct">0</p>
      </div>
      <div class="stat-card">
        <p>Total</p>
        <p class="stat-value" id="total">0</p>
      </div>
      <div class="stat-card">
        <p>Accuracy</p>
        <p class="stat-value" id="accuracy">0%</p>
      </div>
      <div class="stat-card">
        <p>Streak</p>
        <p class="stat-value" id="streak">0</p>
      </div>
    </div>
  </div>

  <footer>
    Heoster Math Lab © 2025| 
    <a href="https://www.instagram.com/codex._.heoster?igsh=YzljYTk1ODg3Zg==" target="_blank">
      Follow on Instagram
    </a>
  </footer>

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
    const particlesContainer = document.getElementById('particles');

    // Game state
    let stats = {
      correct: 0,
      total: 0,
      streak: 0
    };
    
    let currentAnswer = 0;
    let difficulty = 'easy';
    let isAnimating = false;

    // Initialize
    createParticles();
    generateQuestion();
    answerEl.focus();

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
        answerEl.classList.add('shake');
        setTimeout(() => answerEl.classList.remove('shake'), 500);
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
      } else {
        // Wrong answer
        stats.streak = 0;
        questionEl.style.color = 'var(--wrong)';
        questionEl.classList.add('animate__animated', 'animate__shakeX');
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
    }

    function createParticles() {
      const particleCount = 30;
      
      for (let i = 0; i < particleCount; i++) {
        const particle = document.createElement('div');
        particle.classList.add('particle');
        
        // Random properties
        const size = Math.random() * 5 + 1;
        const posX = Math.random() * window.innerWidth;
        const posY = Math.random() * window.innerHeight;
        const duration = Math.random() * 20 + 10;
        const delay = Math.random() * 5;
        
        particle.style.width = `${size}px`;
        particle.style.height = `${size}px`;
        particle.style.left = `${posX}px`;
        particle.style.top = `${posY}px`;
        particle.style.animationDuration = `${duration}s`;
        particle.style.animationDelay = `${delay}s`;
        
        particlesContainer.appendChild(particle);
      }
    }
  </script>
</body>
</html>
