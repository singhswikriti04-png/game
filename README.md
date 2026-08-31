<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Brand Match: The Marketing Gamble</title>
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      user-select: none;
    }

    body {
      background: linear-gradient(135deg, #0f172a, #1e293b);
      color: #f8fafc;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      overflow: hidden;
    }

    /* Dashboard Header */
    .dashboard {
      width: 90%;
      max-width: 420px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      background: rgba(255, 255, 255, 0.05);
      backdrop-filter: blur(10px);
      border: 1px solid rgba(255, 255, 255, 0.1);
      padding: 12px 20px;
      border-radius: 16px;
      margin-bottom: 20px;
      box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
    }

    .stat-box {
      text-align: center;
    }

    .stat-label {
      font-size: 0.75rem;
      color: #94a3b8;
      text-transform: uppercase;
      letter-spacing: 1px;
    }

    .stat-value {
      font-size: 1.25rem;
      font-weight: 700;
      color: #38bdf8;
    }

    .stat-value.money {
      color: #4ade80;
    }

    .stat-value.streak {
      color: #fbbf24;
    }

    /* Card Container */
    .card-container {
      position: relative;
      width: 320px;
      height: 420px;
      perspective: 1000px;
    }

    .card {
      position: absolute;
      width: 100%;
      height: 100%;
      background: rgba(30, 41, 59, 0.9);
      border: 2px solid rgba(255, 255, 255, 0.15);
      border-radius: 24px;
      padding: 24px;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      box-shadow: 0 20px 40px rgba(0, 0, 0, 0.5);
      cursor: grab;
      transition: transform 0.1s ease, opacity 0.3s ease;
      touch-action: none;
    }

    .card:active {
      cursor: grabbing;
    }

    .card-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .brand-badge {
      background: #3b82f6;
      color: white;
      font-size: 0.75rem;
      font-weight: 700;
      padding: 4px 10px;
      border-radius: 12px;
      text-transform: uppercase;
    }

    .brand-payout {
      color: #4ade80;
      font-weight: 700;
      font-size: 0.9rem;
    }

    .brand-logo {
      font-size: 3rem;
      text-align: center;
      margin: 10px 0;
    }

    .brand-name {
      font-size: 1.5rem;
      font-weight: 800;
      text-align: center;
      color: #ffffff;
      margin-bottom: 8px;
    }

    .brand-desc {
      font-size: 0.85rem;
      color: #cbd5e1;
      text-align: center;
      line-height: 1.4;
    }

    .stake-info {
      background: rgba(0, 0, 0, 0.2);
      border-radius: 12px;
      padding: 10px;
      text-align: center;
      font-size: 0.85rem;
      color: #94a3b8;
    }

    .stake-info span {
      color: #f43f5e;
      font-weight: 700;
    }

    /* Swipe Indicators */
    .indicator {
      position: absolute;
      top: 20px;
      padding: 6px 16px;
      border-radius: 8px;
      font-weight: 800;
      font-size: 1.2rem;
      opacity: 0;
      pointer-events: none;
      transition: opacity 0.1s ease;
      z-index: 10;
    }

    .indicator.pass {
      right: 20px;
      border: 3px solid #ef4444;
      color: #ef4444;
      transform: rotate(15deg);
    }

    .indicator.match {
      left: 20px;
      border: 3px solid #22c55e;
      color: #22c55e;
      transform: rotate(-15deg);
    }

    /* Controls */
    .controls {
      display: flex;
      gap: 20px;
      margin-top: 20px;
    }

    .btn-circle {
      width: 60px;
      height: 60px;
      border-radius: 50%;
      border: none;
      font-size: 1.5rem;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      transition: transform 0.2s ease, background-color 0.2s ease;
      box-shadow: 0 10px 20px rgba(0, 0, 0, 0.3);
    }

    .btn-circle:hover {
      transform: scale(1.1);
    }

    .btn-pass {
      background: #334155;
      color: #ef4444;
      border: 2px solid #ef4444;
    }

    .btn-match {
      background: #334155;
      color: #22c55e;
      border: 2px solid #22c55e;
    }

    /* Modal / Quiz Overlay */
    .modal-overlay {
      position: fixed;
      top: 0;
      left: 0;
      width: 100vw;
      height: 100vh;
      background: rgba(15, 23, 42, 0.85);
      backdrop-filter: blur(8px);
      display: flex;
      align-items: center;
      justify-content: center;
      opacity: 0;
      pointer-events: none;
      transition: opacity 0.3s ease;
      z-index: 100;
    }

    .modal-overlay.active {
      opacity: 1;
      pointer-events: auto;
    }

    .quiz-card {
      width: 90%;
      max-width: 400px;
      background: #1e293b;
      border: 2px solid #38bdf8;
      border-radius: 20px;
      padding: 24px;
      box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.5);
      transform: scale(0.9);
      transition: transform 0.3s ease;
    }

    .modal-overlay.active .quiz-card {
      transform: scale(1);
    }

    .quiz-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 16px;
    }

    .quiz-title {
      font-size: 1.1rem;
      font-weight: 700;
      color: #38bdf8;
    }

    .timer {
      background: #ef4444;
      color: white;
      font-weight: 700;
      padding: 4px 10px;
      border-radius: 10px;
      font-size: 0.85rem;
    }

    .question {
      font-size: 0.95rem;
      line-height: 1.5;
      margin-bottom: 20px;
      color: #f1f5f9;
    }

    .options {
      display: flex;
      flex-direction: column;
      gap: 10px;
    }

    .option-btn {
      background: #334155;
      border: 1px solid rgba(255, 255, 255, 0.1);
      color: #f8fafc;
      padding: 12px;
      border-radius: 12px;
      font-size: 0.85rem;
      text-align: left;
      cursor: pointer;
      transition: background 0.2s ease, border-color 0.2s ease;
    }

    .option-btn:hover {
      background: #475569;
      border-color: #38bdf8;
    }

    /* Result Banner */
    .result-screen {
      position: fixed;
      top: 0;
      left: 0;
      width: 100vw;
      height: 100vh;
      background: rgba(15, 23, 42, 0.95);
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      opacity: 0;
      pointer-events: none;
      transition: opacity 0.3s ease;
      z-index: 200;
      padding: 20px;
      text-align: center;
    }

    .result-screen.active {
      opacity: 1;
      pointer-events: auto;
    }

    .result-icon {
      font-size: 4rem;
      margin-bottom: 10px;
    }

    .result-title {
      font-size: 2rem;
      font-weight: 800;
      margin-bottom: 10px;
    }

    .result-desc {
      font-size: 1rem;
      color: #94a3b8;
      margin-bottom: 24px;
      max-width: 300px;
    }

    .btn-action {
      background: #38bdf8;
      color: #0f172a;
      font-weight: 700;
      border: none;
      padding: 12px 32px;
      border-radius: 12px;
      font-size: 1rem;
      cursor: pointer;
      box-shadow: 0 10px 20px rgba(56, 189, 248, 0.3);
    }
  </style>
</head>
<body>

  <!-- Top Dashboard -->
  <div class="dashboard">
    <div class="stat-box">
      <div class="stat-label">Bankroll</div>
      <div class="stat-value money" id="bankroll">$1,000</div>
    </div>
    <div class="stat-box">
      <div class="stat-label">Streak</div>
      <div class="stat-value streak" id="streak">0x</div>
    </div>
    <div class="stat-box">
      <div class="stat-label">Round</div>
      <div class="stat-value" id="round">1/5</div>
    </div>
  </div>

  <!-- Main Interactive Card Deck -->
  <div class="card-container" id="cardContainer">
    <div class="indicator pass" id="passIndicator">PASS</div>
    <div class="indicator match" id="matchIndicator">MATCH</div>
    <div class="card" id="activeCard">
      <div class="card-header">
        <span class="brand-badge" id="brandTier">Emerging</span>
        <span class="brand-payout" id="brandPayout">Payout: 2x</span>
      </div>
      <div class="brand-logo" id="brandLogo">👟</div>
      <div class="brand-name" id="brandName">AeroKick</div>
      <div class="brand-desc" id="brandDesc">Sustainable Gen-Z sneakers targeting urban street style enthusiasts.</div>
      <div class="stake-info">Required Stake: <span id="brandStake">$200</span></div>
    </div>
  </div>

  <!-- Manual Controls -->
  <div class="controls">
    <button class="btn-circle btn-pass" id="passBtn">✕</button>
    <button class="btn-circle btn-match" id="matchBtn">♥</button>
  </div>

  <!-- Brand Quiz Modal -->
  <div class="modal-overlay" id="quizModal">
    <div class="quiz-card">
      <div class="quiz-header">
        <span class="quiz-title" id="quizBrandTitle">Brand Compatibility Test</span>
        <span class="timer" id="timer">10s</span>
      </div>
      <div class="question" id="quizQuestion">Loading question...</div>
      <div class="options" id="quizOptions"></div>
    </div>
  </div>

  <!-- Game Result / Game Over Modal -->
  <div class="result-screen" id="resultScreen">
    <div class="result-icon" id="resultIcon">🏆</div>
    <div class="result-title" id="resultTitle">Campaign Finished!</div>
    <div class="result-desc" id="resultDesc">You ended with $1,500 in brand sponsorships.</div>
    <button class="btn-action" id="restartBtn">Play Again</button>
  </div>

  <script>
    // Sound FX using Web Audio API
    const AudioContext = window.AudioContext || window.webkitAudioContext;
    const audioCtx = new AudioContext();

    function playSound(freq, type, duration) {
      if (audioCtx.state === 'suspended') audioCtx.resume();
      const osc = audioCtx.createOscillator();
      const gain = audioCtx.createGain();
      osc.type = type;
      osc.frequency.value = freq;
      osc.connect(gain);
      gain.connect(audioCtx.destination);
      osc.start();
      gain.gain.exponentialRampToValueAtTime(0.00001, audioCtx.currentTime + duration);
      osc.stop(audioCtx.currentTime + duration);
    }

    function soundSuccess() { playSound(587.33, 'triangle', 0.3); setTimeout(() => playSound(880, 'sine', 0.4), 100); }
    function soundFail() { playSound(150, 'sawtooth', 0.4); }
    function soundSwipe() { playSound(300, 'sine', 0.1); }

    // Game Data & State
    const deck = [
      {
        name: "AeroKick",
        logo: "👟",
        tier: "Emerging",
        desc: "Sustainable Gen-Z sneakers targeting urban street style enthusiasts.",
        stake: 200,
        payoutMultiplier: 2,
        question: "Which marketing channel yields the highest conversion for Gen-Z sneakerheads?",
        options: ["Print Newspapers", "TikTok Creator Collaborations", "Radio Jingles", "Cold Email Blasts"],
        answer: 1
      },
      {
        name: "ZenDrink",
        logo: "🍵",
        tier: "Mid-Market",
        desc: "Zero-sugar adaptogenic sparkling tea boosting mental focus.",
        stake: 350,
        payoutMultiplier: 2.5,
        question: "What primary positioning hook targets busy corporate young professionals?",
        options: ["Cheapest price in town", "Sugar rush energy", "Clean focus without caffeine jitters", "Bulk wholesale discount"],
        answer: 2
      },
      {
        name: "LuxoWatch",
        logo: "⌚",
        tier: "Luxury",
        desc: "High-end mechanical watches built with aerospace-grade titanium.",
        stake: 500,
        payoutMultiplier: 3.5,
        question: "Luxury brand strategies rely heavily on which economic/psychological principle?",
        options: ["Scarcity and Perceived Exclusivity", "Mass Discounting", "Buy-1-Get-1-Free Deals", "Broad Commercial TV Ads"],
        answer: 0
      },
      {
        name: "EcoBite",
        logo: "🥗",
        tier: "Emerging",
        desc: "Plant-based meal kits delivered in 100% compostable packaging.",
        stake: 250,
        payoutMultiplier: 2,
        question: "Which key metric best measures customer loyalty for recurring meal subscriptions?",
        options: ["Impression Share", "Customer Retention/LTV", "Bounce Rate", "Hashtag Usage"],
        answer: 1
      },
      {
        name: "NovaTech",
        logo: "🎧",
        tier: "Enterprise",
        desc: "Noise-canceling headphones with spatial audio AI processing.",
        stake: 600,
        payoutMultiplier: 4,
        question: "What is the primary objective of a pre-launch 'Teaser Campaign'?",
        options: ["Immediate Liquidation", "Build Anticipation & Email Waitlists", "Customer Support", "Partner Acquisition"],
        answer: 1
      }
    ];

    let currentCardIndex = 0;
    let bankroll = 1000;
    let streak = 0;
    let timerInterval = null;
    let timeLeft = 10;

    // DOM Elements
    const bankrollEl = document.getElementById('bankroll');
    const streakEl = document.getElementById('streak');
    const roundEl = document.getElementById('round');
    const activeCard = document.getElementById('activeCard');
    const passIndicator = document.getElementById('passIndicator');
    const matchIndicator = document.getElementById('matchIndicator');
    
    const brandTier = document.getElementById('brandTier');
    const brandPayout = document.getElementById('brandPayout');
    const brandLogo = document.getElementById('brandLogo');
    const brandName = document.getElementById('brandName');
    const brandDesc = document.getElementById('brandDesc');
    const brandStake = document.getElementById('brandStake');

    const quizModal = document.getElementById('quizModal');
    const quizQuestion = document.getElementById('quizQuestion');
    const quizOptions = document.getElementById('quizOptions');
    const quizBrandTitle = document.getElementById('quizBrandTitle');
    const timerEl = document.getElementById('timer');

    const resultScreen = document.getElementById('resultScreen');
    const resultIcon = document.getElementById('resultIcon');
    const resultTitle = document.getElementById('resultTitle');
    const resultDesc = document.getElementById('resultDesc');
    const restartBtn = document.getElementById('restartBtn');

    // Render Current Card
    function loadCard() {
      if (currentCardIndex >= deck.length || bankroll <= 0) {
        endGame();
        return;
      }
      const data = deck[currentCardIndex];
      brandTier.textContent = data.tier;
      brandPayout.textContent = `Payout: ${data.payoutMultiplier}x`;
      brandLogo.textContent = data.logo;
      brandName.textContent = data.name;
      brandDesc.textContent = data.desc;
      brandStake.textContent = `$${data.stake}`;

      roundEl.textContent = `${currentCardIndex + 1}/${deck.length}`;
      resetCardPosition();
    }

    function resetCardPosition() {
      activeCard.style.transform = 'translate(0px, 0px) rotate(0deg)';
      activeCard.style.opacity = '1';
      passIndicator.style.opacity = '0';
      matchIndicator.style.opacity = '0';
    }

    // Touch / Mouse Drag Logic
    let startX = 0, startY = 0, currentX = 0, currentY = 0, isDragging = false;

    activeCard.addEventListener('pointerdown', (e) => {
      isDragging = true;
      startX = e.clientX;
      startY = e.clientY;
      activeCard.style.transition = 'none';
    });

    document.addEventListener('pointermove', (e) => {
      if (!isDragging) return;
      currentX = e.clientX - startX;
      currentY = e.clientY - startY;
      const rotate = currentX * 0.08;
      activeCard.style.transform = `translate(${currentX}px, ${currentY}px) rotate(${rotate}deg)`;

      if (currentX > 50) {
        matchIndicator.style.opacity = Math.min(currentX / 100, 1);
        passIndicator.style.opacity = '0';
      } else if (currentX < -50) {
        passIndicator.style.opacity = Math.min(Math.abs(currentX) / 100, 1);
        matchIndicator.style.opacity = '0';
      } else {
        matchIndicator.style.opacity = '0';
        passIndicator.style.opacity = '0';
      }
    });

    document.addEventListener('pointerup', () => {
      if (!isDragging) return;
      isDragging = false;
      activeCard.style.transition = 'transform 0.3s ease, opacity 0.3s ease';

      if (currentX > 120) {
        handleSwipeRight();
      } else if (currentX < -120) {
        handleSwipeLeft();
      } else {
        resetCardPosition();
      }
    });

    // Buttons
    document.getElementById('passBtn').addEventListener('click', () => {
      activeCard.style.transition = 'transform 0.4s ease, opacity 0.4s ease';
      activeCard.style.transform = 'translate(-300px, 50px) rotate(-30deg)';
      activeCard.style.opacity = '0';
      setTimeout(handleSwipeLeft, 200);
    });

    document.getElementById('matchBtn').addEventListener('click', () => {
      activeCard.style.transition = 'transform 0.4s ease, opacity 0.4s ease';
      activeCard.style.transform = 'translate(300px, 50px) rotate(30deg)';
      activeCard.style.opacity = '0';
      setTimeout(handleSwipeRight, 200);
    });

    function handleSwipeLeft() {
      soundSwipe();
      currentCardIndex++;
      loadCard();
    }

    function handleSwipeRight() {
      soundSwipe();
      const data = deck[currentCardIndex];
      if (bankroll < data.stake) {
        alert("Not enough bankroll to invest in this brand!");
        resetCardPosition();
        return;
      }
      openQuiz(data);
    }

    // Quiz Modal Logic
    function openQuiz(brandData) {
      quizBrandTitle.textContent = `${brandData.name} Compatibility Test`;
      quizQuestion.textContent = brandData.question;
      quizOptions.innerHTML = '';

      brandData.options.forEach((opt, index) => {
        const btn = document.createElement('button');
        btn.className = 'option-btn';
        btn.textContent = `${String.fromCharCode(65 + index)}. ${opt}`;
        btn.onclick = () => submitAnswer(index === brandData.answer);
        quizOptions.appendChild(btn);
      });

      quizModal.classList.add('active');
      startTimer();
    }

    function startTimer() {
      timeLeft = 10;
      timerEl.textContent = `${timeLeft}s`;
      clearInterval(timerInterval);
      timerInterval = setInterval(() => {
        timeLeft--;
        timerEl.textContent = `${timeLeft}s`;
        if (timeLeft <= 0) {
          clearInterval(timerInterval);
          submitAnswer(false);
        }
      }, 1000);
    }

    function submitAnswer(isCorrect) {
      clearInterval(timerInterval);
      quizModal.classList.remove('active');
      const data = deck[currentCardIndex];

      if (isCorrect) {
        soundSuccess();
        streak++;
        const streakBonus = streak > 1 ? 0.2 * streak : 0;
        const profit = Math.round(data.stake * (data.payoutMultiplier + streakBonus));
        bankroll += profit;
        alert(`IT'S A MATCH! 🎯\nYou passed the test and earned $${profit}!`);
      } else {
        soundFail();
        streak = 0;
        bankroll -= data.stake;
        alert(`GHOSTED! 💔\nWrong answer. You lost your $${data.stake} stake.`);
      }

      updateDashboard();
      currentCardIndex++;
      loadCard();
    }

    function updateDashboard() {
      bankrollEl.textContent = `$${bankroll}`;
      streakEl.textContent = `${streak}x`;
    }

    function endGame() {
      resultScreen.classList.add('active');
      if (bankroll > 1000) {
        resultIcon.textContent = "🏆";
        resultTitle.textContent = "Campaign Victory!";
        resultDesc.textContent = `You started with $1,000 and finished with $${bankroll}! Exceptional marketing strategy.`;
      } else {
        resultIcon.textContent = "💸";
        resultTitle.textContent = "Bankrupt!";
        resultDesc.textContent = `You finished with $${bankroll}. Remember to weigh your brand risks carefully next time!`;
      }
    }

    restartBtn.addEventListener('click', () => {
      bankroll = 1000;
      streak = 0;
      currentCardIndex = 0;
      updateDashboard();
      resultScreen.classList.remove('active');
      loadCard();
    });

    // Initialize Game
    loadCard();
  </script>
</body>
</html>
