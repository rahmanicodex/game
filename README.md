
<html lang="fa">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>بازی پیدا کردن کلمات</title>
  <style>
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      direction: rtl;
      background: #f0f8ff;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      min-height: 100vh;
      margin: 0;
      padding: 20px;
    }
    .container {
      background: white;
      border-radius: 15px;
      box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
      padding: 30px;
      width: 90%;
      max-width: 600px;
      text-align: center;
    }
    h1 {
      color: #2c3e50;
      margin-bottom: 20px;
      font-size: 28px;
    }
    .game-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 20px;
      flex-wrap: wrap;
    }
    .game-info {
      display: flex;
      gap: 15px;
    }
    .info-box {
      background: #e3f2fd;
      padding: 8px 15px;
      border-radius: 8px;
      font-weight: bold;
      color: #1976d2;
    }
    #word {
      font-size: 24px;
      margin: 15px 0;
      font-weight: bold;
      color: #d32f2f;
      min-height: 36px;
    }
    .grid {
      display: grid;
      grid-template-columns: repeat(6, 1fr);
      gap: 8px;
      margin: 20px auto;
    }
    .cell {
      aspect-ratio: 1/1;
      background: white;
      display: flex;
      align-items: center;
      justify-content: center;
      border: 2px solid #bbdefb;
      font-size: 22px;
      cursor: pointer;
      transition: all 0.3s;
      border-radius: 8px;
      font-weight: bold;
      color: #0d47a1;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }
    .cell:hover {
      transform: scale(1.05);
      box-shadow: 0 4px 8px rgba(0,0,0,0.2);
    }
    .cell.selected {
      background: #bbdefb;
      border-color: #1976d2;
      transform: scale(1.05);
    }
    .cell.correct {
      background: #c8e6c9;
      border-color: #388e3c;
      animation: bounce 0.5s;
    }
    #message {
      margin-top: 20px;
      font-size: 18px;
      min-height: 27px;
    }
    .success {
      color: #388e3c;
      font-weight: bold;
    }
    .error {
      color: #d32f2f;
    }
    .controls {
      margin-top: 25px;
      display: flex;
      justify-content: center;
      gap: 15px;
      flex-wrap: wrap;
    }
    button {
      background: #1976d2;
      color: white;
      border: none;
      padding: 10px 20px;
      border-radius: 8px;
      cursor: pointer;
      font-size: 16px;
      transition: all 0.3s;
    }
    button:hover {
      background: #1565c0;
      transform: translateY(-2px);
      box-shadow: 0 4px 8px rgba(0,0,0,0.2);
    }
    .difficulty-selector {
      margin-bottom: 20px;
    }
    .difficulty-btn {
      background: #e0e0e0;
      color: #424242;
      margin: 0 5px;
    }
    .difficulty-btn.active {
      background: #1976d2;
      color: white;
    }
    .hint-btn {
      background: #ffa000;
    }
    .hint-btn:hover {
      background: #ff8f00;
    }
    @keyframes bounce {
      0%, 100% { transform: translateY(0); }
      50% { transform: translateY(-10px); }
    }
    @keyframes shake {
      0%, 100% { transform: translateX(0); }
      20%, 60% { transform: translateX(-5px); }
      40%, 80% { transform: translateX(5px); }
    }
    .shake {
      animation: shake 0.5s;
    }
    .progress-container {
      width: 100%;
      height: 10px;
      background: #e0e0e0;
      border-radius: 5px;
      margin-top: 15px;
      overflow: hidden;
    }
    .progress-bar {
      height: 100%;
      background: #4caf50;
      width: 100%;
      transition: width 0.3s;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>بازی پیدا کردن کلمات فارسی</h1>
    
    <div class="difficulty-selector">
      <button class="difficulty-btn active" data-level="easy">آسان</button>
      <button class="difficulty-btn" data-level="medium">متوسط</button>
      <button class="difficulty-btn" data-level="hard">سخت</button>
    </div>
    
    <div class="game-header">
      <div class="game-info">
        <div class="info-box">امتیاز: <span id="score">0</span></div>
        <div class="info-box">زمان: <span id="time">60</span></div>
      </div>
      <button class="hint-btn" id="hint-btn">راهنمایی (<span id="hint-count">3</span>)</button>
    </div>
    
    <div class="progress-container">
      <div class="progress-bar" id="progress-bar"></div>
    </div>
    
    <div id="word"></div>
    <div class="grid" id="grid"></div>
    <div id="message"></div>
    
    <div class="controls">
      <button id="new-game-btn">بازی جدید</button>
      <button id="check-btn">بررسی کلمه</button>
    </div>
  </div>

  <script>
    // سطوح دشواری
    const difficultySettings = {
      easy: {
        gridSize: 6,
        words: ["مکتب", "کتاب", "قلم", "هوش", "درس", "نور", "باغ", "خانه"],
        time: 90,
        hintCount: 5
      },
      medium: {
        gridSize: 8,
        words: ["مدرسه", "دانشجو", "معلم", "تحصیل", "علم", "کتابخانه", "دفتر", "قلم"],
        time: 120,
        hintCount: 3
      },
      hard: {
        gridSize: 10,
        words: ["دانشگاه", "آموزشگاه", "کتابفروشی", "خوشنویسی", "پژوهش", "کتابخوانی", "مطالعه", "تحقیق"],
        time: 150,
        hintCount: 2
      }
    };

    // متغیرهای بازی
    let currentDifficulty = 'easy';
    let selectedWord = '';
    let gridSize = 6;
    let grid = document.getElementById("grid");
    let message = document.getElementById("message");
    let wordDiv = document.getElementById("word");
    let selectedCells = [];
    let score = 0;
    let timeLeft = 60;
    let timer;
    let hintCount = 3;
    let wordPositions = [];
    let usedHints = [];

    // عناصر DOM
    const scoreElement = document.getElementById("score");
    const timeElement = document.getElementById("time");
    const hintCountElement = document.getElementById("hint-count");
    const progressBar = document.getElementById("progress-bar");
    const newGameBtn = document.getElementById("new-game-btn");
    const checkBtn = document.getElementById("check-btn");
    const hintBtn = document.getElementById("hint-btn");
    const difficultyBtns = document.querySelectorAll(".difficulty-btn");

    // شروع بازی
    function initGame() {
      const settings = difficultySettings[currentDifficulty];
      gridSize = settings.gridSize;
      timeLeft = settings.time;
      hintCount = settings.hintCount;
      usedHints = [];
      
      selectedWord = settings.words[Math.floor(Math.random() * settings.words.length)];
      
      grid.innerHTML = '';
      grid.style.gridTemplateColumns = `repeat(${gridSize}, 1fr)`;
      
      wordDiv.textContent = "کلمه هدف: " + selectedWord;
      message.textContent = '';
      scoreElement.textContent = score;
      timeElement.textContent = timeLeft;
      hintCountElement.textContent = hintCount;
      progressBar.style.width = '100%';
      
      generateGrid();
      startTimer();
    }

    // تولید جدول حروف
    function generateGrid() {
      const fullGrid = new Array(gridSize * gridSize).fill('');
      wordPositions = [];

      const directions = ["horizontal", "vertical", "diagonal"];
      const direction = directions[Math.floor(Math.random() * directions.length)];

      let startRow, startCol;
      let rowInc = 0, colInc = 0;

      switch (direction) {
        case "horizontal":
          startRow = Math.floor(Math.random() * gridSize);
          startCol = Math.floor(Math.random() * (gridSize - selectedWord.length));
          rowInc = 0;
          colInc = 1;
          break;
        case "vertical":
          startRow = Math.floor(Math.random() * (gridSize - selectedWord.length));
          startCol = Math.floor(Math.random() * gridSize);
          rowInc = 1;
          colInc = 0;
          break;
        case "diagonal":
          startRow = Math.floor(Math.random() * (gridSize - selectedWord.length));
          startCol = Math.floor(Math.random() * (gridSize - selectedWord.length));
          rowInc = 1;
          colInc = 1;
          break;
      }

      for (let i = 0; i < selectedWord.length; i++) {
        const row = startRow + i * rowInc;
        const col = startCol + i * colInc;
        const index = row * gridSize + col;
        fullGrid[index] = selectedWord[i];
        wordPositions.push(index);
      }

      const letters = "ابپتثجچحخدذرزژسشصضطظعغفقکگلمنوهی";
      for (let i = 0; i < fullGrid.length; i++) {
        if (!fullGrid[i]) {
          fullGrid[i] = letters[Math.floor(Math.random() * letters.length)];
        }
        const cell = document.createElement("div");
        cell.classList.add("cell");
        cell.textContent = fullGrid[i];
        cell.dataset.index = i;
        cell.addEventListener("click", () => selectCell(cell));
        grid.appendChild(cell);
      }
    }

    // انتخاب سلول
    function selectCell(cell) {
      const index = parseInt(cell.dataset.index);
      
      const cellIndex = selectedCells.indexOf(index);
      if (cellIndex > -1) {
        selectedCells.splice(cellIndex, 1);
        cell.classList.remove("selected");
        return;
      }
      
      if (selectedCells.length >= selectedWord.length) {
        message.textContent = "شما نمی‌توانید حروف بیشتری انتخاب کنید!";
        message.className = "error";
        cell.classList.add("shake");
        setTimeout(() => cell.classList.remove("shake"), 500);
        return;
      }
      
      selectedCells.push(index);
      cell.classList.add("selected");
      checkOrder();
    }

    // بررسی ترتیب انتخاب سلول‌ها
    function checkOrder() {
      if (selectedCells.length !== selectedWord.length) return;
      
      const isConsecutive = checkConsecutive(selectedCells);
      
      if (!isConsecutive) {
        message.textContent = "حروف باید به ترتیب و در یک راستا انتخاب شوند!";
        message.className = "error";
        clearSelection();
      } else {
        checkWord();
      }
    }

    // بررسی چسبندگی و ترتیب انتخاب سلول‌ها (بهبود یافته)
    function checkConsecutive(cells) {
      if (cells.length < 2) return true;
      cells.sort((a, b) => a - b);
      
      let isHorizontal = true;
      for (let i = 1; i < cells.length; i++) {
        if (cells[i] - cells[i - 1] !== 1) {
          isHorizontal = false;
          break;
        }
      }
      if (isHorizontal) return true;
      
      let isVertical = true;
      for (let i = 1; i < cells.length; i++) {
        if (cells[i] - cells[i - 1] !== gridSize) {
          isVertical = false;
          break;
        }
      }
      if (isVertical) return true;

      // بررسی قطری پایین-راست
      let isDiagonal1 = true;
      for (let i = 1; i < cells.length; i++) {
        if (cells[i] - cells[i - 1] !== gridSize + 1) {
          isDiagonal1 = false;
          break;
        }
      }
      if (isDiagonal1) return true;

      // بررسی قطری بالا-چپ (معکوس)
      let isDiagonal2 = true;
      for (let i = 1; i < cells.length; i++) {
        if (cells[i - 1] - cells[i] !== gridSize + 1) {
          isDiagonal2 = false;
          break;
        }
      }
      if (isDiagonal2) return true;

      // بررسی افقی معکوس
      let isHorizontalRev = true;
      for (let i = 1; i < cells.length; i++) {
        if (cells[i - 1] - cells[i] !== 1) {
          isHorizontalRev = false;
          break;
        }
      }
      if (isHorizontalRev) return true;

      // بررسی عمودی معکوس
      let isVerticalRev = true;
      for (let i = 1; i < cells.length; i++) {
        if (cells[i - 1] - cells[i] !== gridSize) {
          isVerticalRev = false;
          break;
        }
      }
      if (isVerticalRev) return true;

      // بررسی قطری پایین-چپ (معکوس جهت دیگر)
      let isDiagonal3 = true;
      for (let i = 1; i < cells.length; i++) {
        if (cells[i] - cells[i - 1] !== gridSize - 1) {
          isDiagonal3 = false;
          break;
        }
      }
      if (isDiagonal3) return true;

      // بررسی قطری بالا-راست (معکوس جهت دیگر)
      let isDiagonal4 = true;
      for (let i = 1; i < cells.length; i++) {
        if (cells[i - 1] - cells[i] !== gridSize - 1) {
          isDiagonal4 = false;
          break;
        }
      }
      if (isDiagonal4) return true;

      return false;
    }

    // بررسی تطابق کلمه انتخاب شده
    function checkWord() {
      const letters = selectedCells.map(i => grid.children[i].textContent).join('');
      const reverseLetters = letters.split('').reverse().join('');
      if (letters === selectedWord || reverseLetters === selectedWord) {
        message.textContent = "آفرین! کلمه درست است.";
        message.className = "success";
        score += selectedWord.length * 10;
        scoreElement.textContent = score;
        animateCorrect();
        setTimeout(() => {
          clearSelection();
          startNewRound();
        }, 1500);
      } else {
        message.textContent = "کلمه اشتباه است. دوباره تلاش کنید.";
        message.className = "error";
        animateWrong();
        clearSelection();
      }
    }

    // پاک کردن انتخاب‌ها
    function clearSelection() {
      selectedCells.forEach(i => grid.children[i].classList.remove("selected"));
      selectedCells = [];
    }

    // شروع دور جدید با کلمه جدید
    function startNewRound() {
      const settings = difficultySettings[currentDifficulty];
      selectedWord = settings.words[Math.floor(Math.random() * settings.words.length)];
      wordDiv.textContent = "کلمه هدف: " + selectedWord;
      message.textContent = '';
      grid.innerHTML = '';
      generateGrid();
    }

    // انیمیشن صحیح بودن کلمه
    function animateCorrect() {
      selectedCells.forEach(i => {
        grid.children[i].classList.add("correct");
      });
    }

    // انیمیشن اشتباه بودن کلمه
    function animateWrong() {
      selectedCells.forEach(i => {
        grid.children[i].classList.add("shake");
      });
      setTimeout(() => {
        selectedCells.forEach(i => {
          grid.children[i].classList.remove("shake");
        });
      }, 500);
    }

    // شروع تایمر
    function startTimer() {
      clearInterval(timer);
      timer = setInterval(() => {
        timeLeft--;
        timeElement.textContent = timeLeft;
        progressBar.style.width = (timeLeft / difficultySettings[currentDifficulty].time) * 100 + '%';
        if (timeLeft <= 0) {
          clearInterval(timer);
          message.textContent = "زمان بازی تمام شد!";
          message.className = "error";
          disableGame();
        }
      }, 1000);
    }

    // غیرفعال کردن بازی پس از پایان زمان
    function disableGame() {
      for (let i = 0; i < grid.children.length; i++) {
        grid.children[i].removeEventListener("click", () => selectCell(grid.children[i]));
        grid.children[i].classList.add("disabled");
        grid.children[i].style.cursor = 'not-allowed';
      }
    }

    // راهنمایی نمایش حرف بعدی
    function showHint() {
      if (hintCount <= 0) {
        message.textContent = "راهنمایی‌ها تمام شده‌اند.";
        message.className = "error";
        return;
      }

      let nextHintIndex = 0;
      while (usedHints.includes(nextHintIndex) && nextHintIndex < selectedWord.length) {
        nextHintIndex++;
      }
      if (nextHintIndex >= selectedWord.length) {
        message.textContent = "همه حروف کلمه نشان داده شده است.";
        message.className = "error";
        return;
      }

      const index = wordPositions[nextHintIndex];
      const cell = grid.children[index];
      cell.classList.add("correct");
      usedHints.push(nextHintIndex);
      hintCount--;
      hintCountElement.textContent = hintCount;
      message.textContent = `حرف شماره ${nextHintIndex + 1} نشان داده شد.`;
      message.className = "success";

      setTimeout(() => {
        cell.classList.remove("correct");
      }, 1500);
    }

    // تغییر سطح دشواری
    difficultyBtns.forEach(btn => {
      btn.addEventListener("click", () => {
        difficultyBtns.forEach(b => b.classList.remove("active"));
        btn.classList.add("active");
        currentDifficulty = btn.dataset.level;
        score = 0;
        initGame();
      });
    });

    // رویدادها
    newGameBtn.addEventListener("click", () => {
      score = 0;
      initGame();
    });
    checkBtn.addEventListener("click", () => {
      if (selectedCells.length !== selectedWord.length) {
        message.textContent = "لطفاً تمام حروف را انتخاب کنید.";
        message.className = "error";
        return;
      }
      checkOrder();
    });
    hintBtn.addEventListener("click", showHint);

    // شروع اولیه بازی
    initGame();
  </script>
</body>
</html>
