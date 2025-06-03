
<html lang="fa">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>بازی تست هوش</title>
  <link href="https://cdn.jsdelivr.net/gh/rastikerdar/vazir-font@v30.1.0/dist/font-face.css" rel="stylesheet">
  <style>
    html, body {
      overflow: hidden;
      height: 100%;
      width: 100%;
      position: fixed;
    }

    :root {
      --primary: #4361ee;
      --primary-light: #4895ef;
      --secondary: #3f37c9;
      --success: #4cc9f0;
      --danger: #f72585;
      --warning: #f8961e;
      --info: #43aa8b;
      --light: #f8f9fa;
      --dark: #212529;
      --cell-gradient: linear-gradient(145deg, #ffffff 0%, #f8f9fa 100%);
      --shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
      --shadow-hover: 0 10px 15px rgba(0, 0, 0, 0.1);
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      font-family: Vazir, 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      direction: rtl;
      background: 
        linear-gradient(135deg, rgba(67, 97, 238, 0.2) 0%, rgba(58, 12, 163, 0.2) 100%),
        url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100" viewBox="0 0 100 100"><path fill="%234361ee" fill-opacity="0.1" d="M30,10L50,30L70,10L90,30L70,50L90,70L70,90L50,70L30,90L10,70L30,50L10,30L30,10Z" transform="rotate(45 50 50)"/></svg>');
      background-attachment: fixed;
      background-size: cover;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      min-height: 100vh;
      padding: 20px;
      color: var(--dark);
    }

    .container {
      background: rgba(255, 255, 255, 0.9);
      border-radius: 20px;
      box-shadow: var(--shadow-hover);
      padding: 30px;
      width: 95%;
      max-width: 800px;
      max-height: 95vh;
      overflow-y: auto;
      text-align: center;
      position: relative;
      transition: all 0.3s ease;
      backdrop-filter: blur(5px);
    }

    .container::before {
      content: '';
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 10px;
      background: var(--primary);
    }

    h1 {
      color: var(--primary);
      margin-bottom: 20px;
      font-size: 2.2rem;
      font-weight: 700;
      position: relative;
      padding-bottom: 10px;
    }

    h1::after {
      content: '';
      position: absolute;
      bottom: 0;
      right: 0;
      width: 100px;
      height: 4px;
      background: var(--primary-light);
      border-radius: 2px;
    }

    .welcome-message {
      background: rgba(67, 97, 238, 0.1);
      padding: 15px;
      border-radius: 12px;
      margin-bottom: 25px;
      color: var(--primary);
      font-weight: 500;
      border-right: 4px solid var(--primary);
      animation: fadeIn 0.8s ease;
    }

    .creator-credit {
      margin-top: 25px;
      font-size: 0.9rem;
      color: #666;
      opacity: 0.8;
      transition: all 0.3s;
    }

    .creator-credit:hover {
      opacity: 1;
      color: var(--primary);
    }

    .game-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 25px;
      flex-wrap: wrap;
      gap: 15px;
    }

    .game-info {
      display: flex;
      gap: 15px;
      flex-wrap: wrap;
      justify-content: center;
    }

    .info-box {
      background: rgba(67, 97, 238, 0.1);
      padding: 10px 20px;
      border-radius: 10px;
      font-weight: bold;
      color: var(--primary);
      display: flex;
      align-items: center;
      gap: 8px;
      box-shadow: var(--shadow);
      transition: all 0.3s;
    }

    .info-box:hover {
      transform: translateY(-3px);
      box-shadow: var(--shadow-hover);
    }

    .info-box i {
      font-size: 1.2rem;
    }

    #word {
      font-size: 1.8rem;
      margin: 20px 0;
      font-weight: bold;
      color: var(--secondary);
      min-height: 50px;
      display: flex;
      align-items: center;
      justify-content: center;
      background: rgba(67, 97, 238, 0.05);
      padding: 15px;
      border-radius: 12px;
      border: 2px dashed rgba(67, 97, 238, 0.2);
    }

    .grid {
      display: grid;
      gap: 10px;
      margin: 25px auto;
      perspective: 1000px;
    }

    .cell {
      aspect-ratio: 1/1;
      background: var(--cell-gradient);
      display: flex;
      align-items: center;
      justify-content: center;
      border: 2px solid rgba(67, 97, 238, 0.2);
      font-size: 1.8rem;
      cursor: pointer;
      transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
      border-radius: 12px;
      font-weight: bold;
      color: var(--primary);
      box-shadow: var(--shadow);
      transform-style: preserve-3d;
    }

    .cell:hover {
      transform: translateY(-5px) scale(1.05);
      box-shadow: var(--shadow-hover);
      border-color: var(--primary-light);
    }

    .cell.selected {
      background: rgba(72, 149, 239, 0.2);
      border-color: var(--primary);
      transform: translateY(-5px) scale(1.05);
      color: var(--secondary);
    }

    .cell.correct {
      background: rgba(76, 201, 240, 0.2);
      border-color: var(--success);
      color: #0a9396;
      animation: flipIn 0.6s ease forwards;
    }

    .cell.locked {
      opacity: 0.8;
      pointer-events: none;
      transform: rotateY(180deg);
    }

    #message {
      margin-top: 25px;
      font-size: 1.1rem;
      min-height: 30px;
      padding: 12px;
      border-radius: 8px;
      transition: all 0.3s;
    }

    .success {
      background: rgba(67, 201, 143, 0.2);
      color: #1b9aaa;
      border-right: 4px solid var(--info);
    }

    .error {
      background: rgba(247, 37, 133, 0.1);
      color: var(--danger);
      border-right: 4px solid var(--danger);
    }

    .controls {
      margin-top: 30px;
      display: flex;
      justify-content: center;
      gap: 20px;
      flex-wrap: wrap;
    }

    button {
      background: var(--primary);
      color: white;
      border: none;
      padding: 12px 25px;
      border-radius: 12px;
      cursor: pointer;
      font-size: 1rem;
      font-weight: 500;
      transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
      box-shadow: var(--shadow);
      display: flex;
      align-items: center;
      gap: 8px;
      min-width: 150px;
      justify-content: center;
    }

    button:hover {
      background: var(--secondary);
      transform: translateY(-3px);
      box-shadow: var(--shadow-hover);
    }

    button:active {
      transform: translateY(1px);
    }

    button:disabled {
      background: #adb5bd;
      cursor: not-allowed;
      transform: none !important;
    }

    .progress-container {
      width: 100%;
      height: 12px;
      background: #e9ecef;
      border-radius: 6px;
      margin-top: 20px;
      overflow: hidden;
      box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.1);
    }

    .progress-bar {
      height: 100%;
      background: var(--primary);
      transition: width 0.5s ease;
      border-radius: 6px;
      position: relative;
      overflow: hidden;
    }

    .progress-bar::after {
      content: '';
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background: linear-gradient(
        90deg,
        rgba(255, 255, 255, 0) 0%,
        rgba(255, 255, 255, 0.3) 50%,
        rgba(255, 255, 255, 0) 100%
      );
      animation: shimmer 2s infinite;
    }

    .level-complete {
      font-size: 1.8rem;
      color: var(--primary);
      font-weight: bold;
      margin: 25px 0;
      animation: pulse 1.5s infinite, fadeIn 0.8s ease;
      background: rgba(67, 97, 238, 0.1);
      padding: 15px;
      border-radius: 12px;
      display: inline-block;
    }

    /* انیمیشن‌ها */
    @keyframes flipIn {
      0% { transform: rotateY(0) scale(1); }
      50% { transform: rotateY(90deg) scale(1.1); }
      100% { transform: rotateY(0) scale(1); }
    }

    @keyframes bounce {
      0%, 100% { transform: translateY(0); }
      50% { transform: translateY(-15px); }
    }

    @keyframes shake {
      0%, 100% { transform: translateX(0); }
      20%, 60% { transform: translateX(-5px); }
      40%, 80% { transform: translateX(5px); }
    }

    @keyframes pulse {
      0%, 100% { transform: scale(1); opacity: 1; }
      50% { transform: scale(1.05); opacity: 0.9; }
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(20px); }
      to { opacity: 1; transform: translateY(0); }
    }

    @keyframes shimmer {
      0% { transform: translateX(-100%); }
      100% { transform: translateX(100%); }
    }

    /* رسپانسیو */
    @media (max-width: 768px) {
      .container {
        padding: 20px;
      }
      
      h1 {
        font-size: 1.8rem;
      }
      
      .cell {
        font-size: 1.5rem;
      }
      
      #word {
        font-size: 1.5rem;
      }
      
      button {
        padding: 10px 20px;
        min-width: 120px;
      }
    }

    @media (max-width: 480px) {
      .game-header {
        flex-direction: column;
      }
      
      .game-info {
        width: 100%;
        justify-content: space-between;
      }
      
      .info-box {
        flex-grow: 1;
        justify-content: center;
      }
      
      .controls {
        gap: 10px;
      }
      
      button {
        width: 100%;
      }
    }
  </style>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
</head>
<body>
  <div class="container">
    <h1>بازی تست هوش</h1>
    
    <div class="welcome-message">
      <i class="fas fa-gamepad"></i> به بازی تست هوش خوش آمدید! <br>مهدی رحمانی
    </div>
    
    <div class="game-header">
      <div class="game-info">
        <div class="info-box">
          <i class="fas fa-layer-group"></i>
          مرحله: <span id="current-level">1</span>/<span id="total-levels">5</span>
        </div>
        <div class="info-box">
          <i class="fas fa-star"></i>
          امتیاز: <span id="score">0</span>
        </div>
      </div>
    </div>
    
    <div class="progress-container">
      <div class="progress-bar" id="progress-bar"></div>
    </div>
    
    <div id="word"></div>
    <div class="grid" id="grid"></div>
    <div id="message"></div>
    <div id="level-complete" class="level-complete" style="display: none;">
      <i class="fas fa-trophy"></i> مرحله با موفقیت تکمیل شد!
    </div>
    
    <div class="controls">
      <button id="next-level-btn" disabled>
        <i class="fas fa-arrow-left"></i> مرحله بعد
      </button>
      <button id="hint-btn">
        <i class="fas fa-lightbulb"></i> راهنمایی (<span id="hint-count">3</span>)
      </button>
    </div>
    
    <div class="creator-credit">
      <i class="fas fa-code"></i> طراحی و توسعه توسط مهدی رحمانی
    </div>
  </div>

  <script>
    // تنظیمات بازی
    const gameSettings = {
      levels: [
        {
          size: 6,
          words: ["مکتب", "کتاب", "قلم", "دفتر", "مدرسه", "نقاش", "مداد", "پاککن", "خطکش", "گچ"],
          time: 90,
          hints: 3
        },
        {
          size: 7,
          words: ["دانشجو", "معلم", "تحصیل", "علم", "کتابخانه", "دانشگاه", "آموزگار", "مدرسه", "کلاس", "تخته"],
          time: 120,
          hints: 3
        },
        {
          size: 8,
          words: ["دانشگاه", "آموزش", "پژوهش", "مطالعه", "تحقیق", "کتابفروشی", "خوشنویسی", "کتابخوانی", "دانشآموز", "کتابداری"],
          time: 150,
          hints: 2
        },
        {
          size: 9,
          words: ["کتابفروشی", "خوشنویسی", "کتابخوانی", "دانشآموز", "کتابداری", "دیکته", "انشا", "ریاضی", "علوم", "تاریخ"],
          time: 180,
          hints: 2
        },
        {
          size: 10,
          words: ["دایرهالمعارف", "کتابشناسی", "تحصیلات", "آموزشگاه", "کتابگردی", "کتابخانه", "دانشکده", "پروفسور", "دکترا", "لیسانس"],
          time: 210,
          hints: 1
        }
      ],
      letters: "ابپتثجچحخدذرزژسشصضطظعغفقکگلمنوهی"
    };

    // متغیرهای بازی
    let currentLevel = 0;
    let selectedWord = '';
    let gridSize = 5;
    let score = 0;
    let hintCount = 3;
    let selectedCells = [];
    let wordPositions = [];
    let timer;
    let timeLeft = 90;
    let usedWords = []; // لیست کلمات استفاده شده

    // عناصر DOM
    const grid = document.getElementById("grid");
    const message = document.getElementById("message");
    const wordDiv = document.getElementById("word");
    const scoreElement = document.getElementById("score");
    const currentLevelElement = document.getElementById("current-level");
    const totalLevelsElement = document.getElementById("total-levels");
    const hintCountElement = document.getElementById("hint-count");
    const progressBar = document.getElementById("progress-bar");
    const nextLevelBtn = document.getElementById("next-level-btn");
    const hintBtn = document.getElementById("hint-btn");
    const levelCompleteDiv = document.getElementById("level-complete");

    // مقداردهی اولیه
    totalLevelsElement.textContent = gameSettings.levels.length;
    initLevel();

    // شروع مرحله جدید
    function initLevel() {
      // ریست متغیرها
      selectedCells = [];
      wordPositions = [];
      grid.innerHTML = '';
      message.textContent = '';
      message.className = '';
      levelCompleteDiv.style.display = 'none';
      nextLevelBtn.disabled = true;
      
      // تنظیمات مرحله فعلی
      const level = gameSettings.levels[currentLevel];
      gridSize = level.size;
      timeLeft = level.time;
      hintCount = level.hints;
      
      // انتخاب کلمه تصادفی بدون تکرار
      let availableWords = level.words.filter(word => !usedWords.includes(word));
      
      if (availableWords.length === 0) {
        // اگر همه کلمات استفاده شده‌اند، لیست را ریست کنیم
        usedWords = [];
        availableWords = [...level.words];
      }
      
      selectedWord = availableWords[Math.floor(Math.random() * availableWords.length)];
      usedWords.push(selectedWord);
      
      // به‌روزرسانی نمایش
      currentLevelElement.textContent = currentLevel + 1;
      wordDiv.textContent = "کلمه هدف: " + selectedWord;
      hintCountElement.textContent = hintCount;
      progressBar.style.width = '100%';
      progressBar.style.backgroundColor = '';
      
      // تنظیم گرید
      grid.style.gridTemplateColumns = `repeat(${gridSize}, 1fr)`;
      
      // تولید جدول
      generateGrid();
      
      // شروع تایمر
      startTimer();
    }

    // تولید جدول حروف
    function generateGrid() {
      const fullGrid = new Array(gridSize * gridSize).fill('');

      // انتخاب مسیر تصادفی
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

      // قرار دادن کلمه در جدول
      for (let i = 0; i < selectedWord.length; i++) {
        const row = startRow + i * rowInc;
        const col = startCol + i * colInc;
        const index = row * gridSize + col;
        fullGrid[index] = selectedWord[i];
        wordPositions.push(index);
      }

      // پر کردن باقی جدول
      for (let i = 0; i < fullGrid.length; i++) {
        if (!fullGrid[i]) {
          fullGrid[i] = gameSettings.letters[Math.floor(Math.random() * gameSettings.letters.length)];
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
      
      // اگر سلول قبلاً انتخاب شده باشد
      const cellIndex = selectedCells.indexOf(index);
      if (cellIndex > -1) {
        selectedCells.splice(cellIndex, 1);
        cell.classList.remove("selected");
        message.textContent = "";
        message.className = "";
        return;
      }
      
      // اگر سلول قفل شده باشد
      if (cell.classList.contains("locked")) {
        message.textContent = "این حرف در مرحله قبلی استفاده شده!";
        message.className = "error";
        cell.classList.add("shake");
        setTimeout(() => cell.classList.remove("shake"), 500);
        return;
      }
      
      // اضافه کردن سلول جدید
      selectedCells.push(index);
      cell.classList.add("selected");
      
      // اگر تعداد حروف کافی انتخاب شده باشد
      if (selectedCells.length === selectedWord.length) {
        checkWord();
      }
    }

    // بررسی کلمه
    function checkWord() {
      // بررسی ترتیب انتخاب
      const isConsecutive = checkConsecutive(selectedCells);
      if (!isConsecutive) {
        message.textContent = "حروف باید به ترتیب و در یک راستا انتخاب شوند!";
        message.className = "error";
        resetSelection();
        return;
      }
      
      // بررسی تطابق کلمه
      const selectedLetters = selectedCells.map(i => grid.children[i].textContent).join("");
      if (selectedLetters === selectedWord) {
        // کلمه صحیح
        score += selectedWord.length * (currentLevel + 1) * 5;
        scoreElement.textContent = score;
        
        // علامت گذاری سلول‌های صحیح
        selectedCells.forEach(i => {
          grid.children[i].classList.remove("selected");
          grid.children[i].classList.add("correct", "locked");
        });
        
        // نمایش پیام موفقیت
        message.textContent = "آفرین! کلمه درست پیدا شد!";
        message.className = "success";
        levelCompleteDiv.style.display = 'block';
        
        // فعال کردن دکمه مرحله بعد
        nextLevelBtn.disabled = false;
        
        // توقف تایمر
        clearInterval(timer);
      } else {
        // کلمه نادرست
        message.textContent = "کلمه انتخاب شده صحیح نیست!";
        message.className = "error";
        resetSelection();
      }
    }

    // بررسی ترتیب انتخاب
    function checkConsecutive(cells) {
      if (cells.length <= 1) return true;
      
      cells.sort((a, b) => a - b);
      
      const directions = [
        { row: 0, col: 1 },   // افقی
        { row: 1, col: 0 },   // عمودی
        { row: 1, col: 1 },   // قطری
        { row: 1, col: -1 }   // قطری معکوس
      ];
      
      for (const dir of directions) {
        let isValid = true;
        for (let i = 1; i < cells.length; i++) {
          const prevRow = Math.floor(cells[i-1] / gridSize);
          const prevCol = cells[i-1] % gridSize;
          const expectedIndex = (prevRow + dir.row) * gridSize + (prevCol + dir.col);
          
          if (cells[i] !== expectedIndex) {
            isValid = false;
            break;
          }
        }
        if (isValid) return true;
      }
      
      return false;
    }

    // ریست انتخاب‌ها
    function resetSelection() {
      selectedCells.forEach(i => {
        grid.children[i].classList.remove("selected");
      });
      selectedCells = [];
    }

    // شروع تایمر
    function startTimer() {
      clearInterval(timer);
      timeLeft = gameSettings.levels[currentLevel].time;
      updateProgressBar();
      
      timer = setInterval(() => {
        timeLeft--;
        updateProgressBar();
        
        if (timeLeft <= 0) {
          clearInterval(timer);
          message.textContent = "زمان به پایان رسید!";
          message.className = "error";
          nextLevelBtn.disabled = false;
        }
      }, 1000);
    }

    // به‌روزرسانی نوار پیشرفت
    function updateProgressBar() {
      const percent = (timeLeft / gameSettings.levels[currentLevel].time) * 100;
      progressBar.style.width = `${percent}%`;
      
      if (percent < 30) {
        progressBar.style.backgroundColor = '#f72585';
      } else if (percent < 60) {
        progressBar.style.backgroundColor = '#f8961e';
      } else {
        progressBar.style.backgroundColor = '#4361ee';
      }
    }

    // راهنمایی
    function showHint() {
      if (hintCount <= 0) {
        message.textContent = "راهنمایی بیشتری ندارید!";
        message.className = "error";
        return;
      }
      
      // پیدا کردن حرفی که هنوز راهنمایی نشده
      let hintIndex;
      for (let i = 0; i < wordPositions.length; i++) {
        const cell = grid.children[wordPositions[i]];
        if (!cell.classList.contains("correct") && !cell.classList.contains("selected")) {
          hintIndex = wordPositions[i];
          break;
        }
      }
      
      if (hintIndex === undefined) return;
      
      hintCount--;
      hintCountElement.textContent = hintCount;
      
      // نمایش راهنمایی
      const cell = grid.children[hintIndex];
      cell.classList.add("selected");
      setTimeout(() => {
        cell.classList.remove("selected");
      }, 1000);
    }

    // رویدادها
    nextLevelBtn.addEventListener("click", () => {
      currentLevel++;
      if (currentLevel < gameSettings.levels.length) {
        initLevel();
      } else {
        // پایان بازی
        message.textContent = `تبریک! بازی به پایان رسید. امتیاز نهایی شما: ${score}`;
        message.className = "success";
        nextLevelBtn.disabled = true;
        grid.innerHTML = '';
        wordDiv.textContent = '';
        levelCompleteDiv.style.display = 'none';
      }
    });
    
    hintBtn.addEventListener("click", showHint);
  </script>
</body>
</html>
