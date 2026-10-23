<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>成語大冒險 Idiom Adventure</title>
  <!-- Tailwind CSS CDN -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- FontAwesome Icons -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <!-- Canvas Confetti CDN -->
  <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@400;500;700;900&display=swap');
    
    body {
      font-family: 'Noto Sans TC', sans-serif;
      background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
      min-height: 100vh;
    }

    /* Glassmorphism Card Style */
    .glass-card {
      background: rgba(255, 255, 255, 0.9);
      backdrop-filter: blur(12px);
      -webkit-backdrop-filter: blur(12px);
      border: 1px solid rgba(255, 255, 255, 0.6);
    }

    /* Floating Animation */
    @keyframes float {
      0%, 100% { transform: translateY(0px) rotate(0deg); }
      50% { transform: translateY(-8px) rotate(1deg); }
    }
    .animate-float {
      animation: float 4s ease-in-out infinite;
    }

    /* Custom Scrollbar */
    ::-webkit-scrollbar {
      width: 6px;
    }
    ::-webkit-scrollbar-track {
      background: rgba(0,0,0,0.05);
    }
    ::-webkit-scrollbar-thumb {
      background: rgba(99, 102, 241, 0.3);
      border-radius: 3px;
    }
  </style>
</head>
<body class="flex flex-col min-h-screen text-slate-800 antialiased selection:bg-indigo-500 selection:text-white">

  <!-- Header / Navigation Bar -->
  <header class="w-full bg-white/70 backdrop-blur-md border-b border-white/40 sticky top-0 z-40 px-4 py-3 shadow-sm">
    <div class="max-w-4xl mx-auto flex items-center justify-between">
      <div class="flex items-center space-x-3 cursor-pointer" onclick="showHomeScreen()">
        <div class="bg-indigo-600 text-white p-2.5 rounded-2xl shadow-lg shadow-indigo-200">
          <i class="fa-solid fa-graduation-cap text-xl"></i>
        </div>
        <div>
          <h1 class="text-xl font-black bg-gradient-to-r from-indigo-600 to-purple-600 bg-clip-text text-transparent">
            成語大冒險
          </h1>
          <p class="text-xs text-slate-500 font-medium hidden sm:block">成語智慧大闖關</p>
        </div>
      </div>

      <!-- Stats Bar (Hidden on Intro Screen) -->
      <div id="statsBar" class="hidden flex items-center space-x-3 sm:space-x-6 bg-slate-100/80 px-4 py-1.5 rounded-full border border-slate-200/60">
        <div class="flex items-center space-x-1.5">
          <i class="fa-solid fa-user text-indigo-500 text-xs"></i>
          <span id="playerDisplay" class="font-bold text-slate-700 text-xs sm:text-sm">---</span>
        </div>
        <div class="h-4 w-px bg-slate-300"></div>
        <div class="flex items-center space-x-1.5">
          <i class="fa-solid fa-trophy text-amber-500 text-xs"></i>
          <span id="scoreDisplay" class="font-black text-indigo-600 text-sm sm:text-base">0</span>
        </div>
        <div class="h-4 w-px bg-slate-300"></div>
        <div class="flex items-center space-x-1.5">
          <i class="fa-solid fa-clock text-indigo-500 text-xs"></i>
          <span id="timerDisplay" class="font-mono font-bold text-slate-700 text-xs sm:text-base">00:00</span>
        </div>
      </div>

      <!-- Action Buttons -->
      <div class="flex items-center gap-2">
        <button onclick="openLeaderboardModal()" class="text-slate-600 hover:text-indigo-600 bg-white hover:bg-slate-50 p-2 sm:px-3 sm:py-1.5 rounded-xl border border-slate-200 text-xs sm:text-sm font-bold transition flex items-center gap-1.5 shadow-sm">
          <i class="fa-solid fa-ranking-star text-amber-500"></i>
          <span class="hidden sm:inline">排行榜</span>
        </button>
        <button onclick="openIdiomListModal()" class="text-slate-600 hover:text-indigo-600 bg-white hover:bg-slate-50 p-2 sm:px-3 sm:py-1.5 rounded-xl border border-slate-200 text-xs sm:text-sm font-bold transition flex items-center gap-1.5 shadow-sm">
          <i class="fa-solid fa-book-open text-indigo-500"></i>
          <span class="hidden sm:inline">成語圖鑑</span>
        </button>
      </div>
    </div>
  </header>

  <!-- Main Container -->
  <main class="flex-grow flex items-center justify-center p-3 sm:p-6 max-w-4xl w-full mx-auto">

    <!-- Screen 1: Login & Welcome Screen -->
    <div id="homeScreen" class="w-full text-center space-y-6 sm:space-y-8 animate-fade-in my-auto">
      <div class="inline-block relative">
        <span class="px-4 py-1.5 rounded-full bg-indigo-100 text-indigo-700 text-xs sm:text-sm font-bold tracking-wide uppercase shadow-inner">
          <i class="fa-solid fa-star text-amber-400 mr-1"></i> 全能闖關考驗
        </span>
        <h2 class="text-3xl sm:text-5xl font-black text-slate-800 mt-3 tracking-tight">
          成語大冒險挑戰賽
        </h2>
        <p class="text-sm sm:text-base text-slate-600 max-w-md mx-auto mt-2">
          挑戰包含「喜出望外」、「目瞪口呆」等 10 個核心成語，完成三大關卡登頂排行榜！
        </p>
      </div>

      <!-- Student ID / Name Input Login Box -->
      <div class="glass-card max-w-md mx-auto p-6 sm:p-8 rounded-3xl shadow-xl text-left border border-white space-y-4">
        <div>
          <label for="studentInput" class="block text-sm font-bold text-slate-700 mb-1.5">
            <i class="fa-solid fa-id-card text-indigo-500 mr-1"></i> 請輸入學號或姓名：
          </label>
          <input type="text" id="studentInput" placeholder="例如：S12345 或 張小明" class="w-full px-4 py-3 rounded-2xl border border-slate-200 focus:outline-none focus:ring-2 focus:ring-indigo-500 text-slate-800 font-bold placeholder:text-slate-300 transition">
          <p id="inputErrorMsg" class="hidden text-xs text-rose-500 font-bold mt-1.5 flex items-center gap-1">
            <i class="fa-solid fa-circle-exclamation"></i> 請輸入學號或姓名才能開始遊戲！
          </p>
        </div>

        <button onclick="startFullGame()" class="w-full bg-gradient-to-r from-indigo-600 to-purple-600 hover:from-indigo-700 hover:to-purple-700 text-white font-black py-3.5 rounded-2xl shadow-lg shadow-indigo-200 hover:shadow-indigo-300 transition active:scale-95 flex items-center justify-center gap-2 text-base">
          <span>開始冒險闖關</span>
          <i class="fa-solid fa-arrow-right"></i>
        </button>
      </div>

      <!-- Mode Progression Overview -->
      <div class="grid grid-cols-1 sm:grid-cols-3 gap-3 max-w-3xl mx-auto text-left opacity-90">
        <div class="bg-white/80 p-4 rounded-2xl border border-slate-200 shadow-sm flex items-center space-x-3">
          <div class="w-10 h-10 rounded-xl bg-indigo-50 text-indigo-600 flex items-center justify-center font-bold text-lg">1</div>
          <div>
            <h4 class="text-xs font-bold text-slate-800">第一關：情境選詞</h4>
            <p class="text-[11px] text-slate-500">語境理解與文意填空</p>
          </div>
        </div>
        <div class="bg-white/80 p-4 rounded-2xl border border-slate-200 shadow-sm flex items-center space-x-3">
          <div class="w-10 h-10 rounded-xl bg-purple-50 text-purple-600 flex items-center justify-center font-bold text-lg">2</div>
          <div>
            <h4 class="text-xs font-bold text-slate-800">第二關：Emoji 猜謎</h4>
            <p class="text-[11px] text-slate-500">趣味圖形聯想考驗</p>
          </div>
        </div>
        <div class="bg-white/80 p-4 rounded-2xl border border-slate-200 shadow-sm flex items-center space-x-3">
          <div class="w-10 h-10 rounded-xl bg-emerald-50 text-emerald-600 flex items-center justify-center font-bold text-lg">3</div>
          <div>
            <h4 class="text-xs font-bold text-slate-800">第三關：文字拼圖</h4>
            <p class="text-[11px] text-slate-500">成語拆解與組裝組合</p>
          </div>
        </div>
      </div>
    </div>

    <!-- Screen 2: Game Playing Area -->
    <div id="gameScreen" class="hidden w-full max-w-2xl mx-auto space-y-4">
      
      <!-- Progress Bar & Question Count -->
      <div class="flex items-center justify-between text-xs sm:text-sm font-bold text-slate-500 mb-1">
        <span id="stageTitleTag" class="text-indigo-600 bg-indigo-50 px-3 py-1 rounded-full">第一關：情境選詞</span>
        <span id="questionProgress">關卡 1/3 - 題目 1/10</span>
      </div>
      <div class="w-full bg-slate-200/80 rounded-full h-2.5 overflow-hidden">
        <div id="progressBar" class="bg-gradient-to-r from-indigo-500 to-purple-500 h-2.5 rounded-full transition-all duration-300" style="width: 3%"></div>
      </div>

      <!-- Question Card Container -->
      <div class="glass-card p-6 sm:p-8 rounded-3xl shadow-xl relative min-h-[320px] flex flex-col justify-between">
        
        <!-- Question Content -->
        <div id="questionArea" class="space-y-4 my-auto">
          <!-- Dynamic Content Inserted Here -->
        </div>

        <!-- Answer Feedback Banner -->
        <div id="feedbackBox" class="hidden mt-6 p-4 rounded-2xl border flex flex-col space-y-2 animate-fade-in">
          <div class="flex items-center space-x-2 font-bold text-base">
            <span id="feedbackIcon"></span>
            <span id="feedbackTitle"></span>
          </div>
          <p id="feedbackText" class="text-xs sm:text-sm text-slate-600 leading-relaxed"></p>
        </div>

        <!-- Action / Next Button Container -->
        <div class="mt-6 flex justify-end">
          <button id="nextBtn" onclick="nextQuestion()" class="hidden bg-indigo-600 hover:bg-indigo-700 text-white font-bold px-6 py-2.5 rounded-2xl shadow-lg shadow-indigo-200 hover:shadow-indigo-300 transition active:scale-95 flex items-center gap-2 text-sm sm:text-base">
            <span>下一題</span> <i class="fa-solid fa-arrow-right"></i>
          </button>
        </div>

      </div>
    </div>

    <!-- Screen 3: Result Summary & Leaderboard Screen -->
    <div id="resultScreen" class="hidden w-full max-w-xl mx-auto text-center space-y-6 animate-fade-in my-auto">
      <div class="glass-card p-6 sm:p-8 rounded-3xl shadow-2xl space-y-6 relative overflow-hidden border border-white">
        
        <!-- Trophy Graphic -->
        <div class="w-20 h-20 bg-amber-100 text-amber-500 rounded-full flex items-center justify-center text-4xl mx-auto shadow-inner border-4 border-amber-200 animate-bounce">
          <i class="fa-solid fa-award"></i>
        </div>

        <div>
          <h2 class="text-2xl font-black text-slate-800">冒險挑戰完成！</h2>
          <p class="text-xs text-slate-500 mt-1">玩家：<span id="finalPlayerName" class="font-bold text-indigo-600">---</span></p>
        </div>

        <!-- Score Breakdown Box -->
        <div class="bg-slate-50 p-4 rounded-2xl border border-slate-100 grid grid-cols-2 gap-4">
          <div class="text-center border-r border-slate-200 pr-2">
            <p class="text-xs text-slate-400 font-bold">總得分</p>
            <p id="finalScore" class="text-3xl font-black text-indigo-600 mt-1">0</p>
          </div>
          <div class="text-center pl-2">
            <p class="text-xs text-slate-400 font-bold">總耗時</p>
            <p id="finalTime" class="text-3xl font-black text-slate-700 mt-1">00:00</p>
          </div>
        </div>

        <!-- Accuracy Evaluation Comment -->
        <div id="performanceBadge" class="p-3 bg-indigo-50 text-indigo-700 rounded-xl font-bold text-xs">
          🌟 表現優異！
        </div>

        <!-- Action Buttons -->
        <div class="space-y-3 pt-2">
          <button onclick="openLeaderboardModal()" class="w-full bg-amber-500 hover:bg-amber-600 text-white font-bold py-3 rounded-2xl shadow-lg shadow-amber-200 transition active:scale-95 flex items-center justify-center gap-2">
            <i class="fa-solid fa-ranking-star"></i> 查看班級排行榜
          </button>
          <button onclick="showHomeScreen()" class="w-full bg-slate-100 hover:bg-slate-200 text-slate-700 font-bold py-3 rounded-2xl transition active:scale-95 flex items-center justify-center gap-2">
            <i class="fa-solid fa-rotate-right"></i> 再次挑戰 / 更換學號
          </button>
        </div>

      </div>
    </div>

  </main>

  <!-- Leaderboard Modal (排行榜彈窗) -->
  <div id="leaderboardModal" class="fixed inset-0 bg-slate-900/50 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
    <div class="bg-white w-full max-w-lg rounded-3xl shadow-2xl overflow-hidden flex flex-col max-h-[85vh] animate-fade-in">
      <div class="px-6 py-4 bg-gradient-to-r from-amber-500 to-indigo-600 text-white flex items-center justify-between">
        <h3 class="font-black text-lg flex items-center gap-2">
          <i class="fa-solid fa-trophy text-amber-200"></i> 班級英雄排行榜 Top 10
        </h3>
        <button onclick="closeLeaderboardModal()" class="text-white/80 hover:text-white p-1 rounded-lg text-xl transition">
          <i class="fa-solid fa-xmark"></i>
        </button>
      </div>
      
      <!-- Leaderboard List -->
      <div class="p-4 overflow-y-auto space-y-2 flex-grow" id="leaderboardList">
        <!-- Dynamically filled -->
      </div>

      <div class="p-4 bg-slate-50 border-t flex items-center justify-between">
        <button onclick="confirmClearLeaderboard()" class="text-xs font-bold text-rose-500 hover:text-rose-700 hover:bg-rose-50 px-3 py-2 rounded-xl transition flex items-center gap-1">
          <i class="fa-solid fa-trash-can"></i> 重置排行榜 (管理者)
        </button>
        <button onclick="closeLeaderboardModal()" class="bg-slate-800 text-white px-5 py-2 rounded-xl text-xs font-bold hover:bg-slate-700 transition">
          關閉
        </button>
      </div>
    </div>
  </div>

  <!-- Idioms Reference Modal (成語圖鑑彈窗) -->
  <div id="idiomModal" class="fixed inset-0 bg-slate-900/50 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
    <div class="bg-white w-full max-w-2xl rounded-3xl shadow-2xl overflow-hidden flex flex-col max-h-[85vh] animate-fade-in">
      <div class="px-6 py-4 bg-indigo-600 text-white flex items-center justify-between">
        <h3 class="font-black text-lg flex items-center gap-2">
          <i class="fa-solid fa-book"></i> 10 個核心成語學習圖鑑
        </h3>
        <button onclick="closeIdiomListModal()" class="text-white/80 hover:text-white p-1 rounded-lg text-xl transition">
          <i class="fa-solid fa-xmark"></i>
        </button>
      </div>
      
      <div class="p-6 overflow-y-auto space-y-4" id="idiomModalList">
        <!-- Dynamically populated idiom items -->
      </div>

      <div class="p-4 bg-slate-50 border-t text-center">
        <button onclick="closeIdiomListModal()" class="bg-slate-800 text-white px-6 py-2 rounded-xl text-xs font-bold hover:bg-slate-700 transition">
          關閉圖鑑
        </button>
      </div>
    </div>
  </div>

  <!-- Web Audio Synthesizer for SFX -->
  <script>
    const AudioContext = window.AudioContext || window.webkitAudioContext;
    let audioCtx = null;

    function initAudio() {
      if (!audioCtx) {
        audioCtx = new AudioContext();
      }
    }

    function playSound(type) {
      try {
        initAudio();
        if (!audioCtx) return;

        const osc = audioCtx.createOscillator();
        const gain = audioCtx.createGain();
        osc.connect(gain);
        gain.connect(audioCtx.destination);

        const now = audioCtx.currentTime;

        if (type === 'correct') {
          osc.type = 'sine';
          osc.frequency.setValueAtTime(659.25, now);
          osc.frequency.exponentialRampToValueAtTime(880, now + 0.15);
          gain.gain.setValueAtTime(0.3, now);
          gain.gain.exponentialRampToValueAtTime(0.01, now + 0.3);
          osc.start(now);
          osc.stop(now + 0.3);
        } else if (type === 'wrong') {
          osc.type = 'sawtooth';
          osc.frequency.setValueAtTime(130.81, now);
          osc.frequency.exponentialRampToValueAtTime(98.00, now + 0.2);
          gain.gain.setValueAtTime(0.25, now);
          gain.gain.exponentialRampToValueAtTime(0.01, now + 0.25);
          osc.start(now);
          osc.stop(now + 0.25);
        } else if (type === 'click') {
          osc.type = 'triangle';
          osc.frequency.setValueAtTime(400, now);
          gain.gain.setValueAtTime(0.1, now);
          gain.gain.exponentialRampToValueAtTime(0.01, now + 0.05);
          osc.start(now);
          osc.stop(now + 0.05);
        } else if (type === 'win') {
          [523.25, 659.25, 783.99, 1046.50].forEach((freq, idx) => {
            const o = audioCtx.createOscillator();
            const g = audioCtx.createGain();
            o.connect(g);
            g.connect(audioCtx.destination);
            o.frequency.setValueAtTime(freq, now + idx * 0.1);
            g.gain.setValueAtTime(0.2, now + idx * 0.1);
            g.gain.exponentialRampToValueAtTime(0.01, now + idx * 0.1 + 0.25);
            o.start(now + idx * 0.1);
            o.stop(now + idx * 0.1 + 0.25);
          });
        }
      } catch (e) {
        console.log('Audio error:', e);
      }
    }
  </script>

  <!-- Game Logic & Leaderboard Script -->
  <script>
    // 10 Core Idioms Database
    const IDIOMS_DB = [
      {
        id: 1,
        word: "喜出望外",
        pinyin: "xǐ chū wàng wài",
        meaning: "指遇到了意想不到的好事而感到特別高興。",
        example: "原本以為這次考試會不及格，沒想到竟拿到滿分，真讓人喜出望外。",
        mode1_q: "小明參加抽獎活動，竟然幸運抽中了頭獎，讓他覺得（　　　　）。",
        mode1_options: ["喜出望外", "目瞪口呆", "得意忘形", "垂涎三尺"],
        mode2_emoji: "🥳🎁🎉",
        mode2_clue: "意想不到的好事降臨，高興得超出預期！",
        mode2_options: ["喜出望外", "心曠神怡", "生氣勃勃", "美輪美奐"]
      },
      {
        id: 2,
        word: "不計其數",
        pinyin: "bù jì qí shù",
        meaning: "無法計算數目。形容數量極多。",
        example: "夜空中閃爍著不計其數的繁星，非常美麗。",
        mode1_q: "圖書館裡收藏的書籍多達幾十萬冊，簡直是（　　　　）。",
        mode1_options: ["不計其數", "不翼而飛", "美輪美奐", "怒髮衝冠"],
        mode2_emoji: "✨🌌🔢❌",
        mode2_clue: "數量超級多，根本數不過來！",
        mode2_options: ["不計其數", "垂涎三尺", "目瞪口呆", "得意忘形"]
      },
      {
        id: 3,
        word: "得意忘形",
        pinyin: "dé yì wàng xíng",
        meaning: "形容高興得失去了常態。",
        example: "雖然取得了第一名，但我們不能得意忘形，還是要保持謙虛。",
        mode1_q: "他因為贏了比賽就（　　　　），結果在第二局比賽中輕敵輸掉了。",
        mode1_options: ["得意忘形", "喜出望外", "心曠神怡", "生氣勃勃"],
        mode2_emoji: "🏆🤪🕺",
        mode2_clue: "因為成功而太高興，結果高興到連自己的言行舉止都忘了分寸。",
        mode2_options: ["得意忘形", "怒髮衝冠", "不翼而飛", "美輪美奐"]
      },
      {
        id: 4,
        word: "不翼而飛",
        pinyin: "bù yì ér fēi",
        meaning: "沒有翅膀卻飛走了。比喻物品突然無故遺失。",
        example: "我剛才放在桌上的手錶，怎麼一下子就不翼而飛了？",
        mode1_q: "明明剛把鑰匙放在玄關，轉個身卻（　　　　），到處都找不到。",
        mode1_options: ["不翼而飛", "不計其數", "目瞪口呆", "垂涎三尺"],
        mode2_emoji: "💸🕊️❓",
        mode2_clue: "沒有翅膀卻消失得無影無蹤，東西憑空不見了！",
        mode2_options: ["不翼而飛", "喜出望外", "得意忘形", "生氣勃勃"]
      },
      {
        id: 5,
        word: "心曠神怡",
        pinyin: "xīn kuàng shén yí",
        meaning: "心情開朗，精神愉悅。",
        example: "站在山頂遠眺廣闊的大海，令人心曠神怡。",
        mode1_q: "走進綠意盎然的清晨森林，吸入新鮮空氣，頓時感到（　　　　）。",
        mode1_options: ["心曠神怡", "怒髮衝冠", "目瞪口呆", "不計其數"],
        mode2_emoji: "🏞️😌🍃",
        mode2_clue: "視野開闊，景色優美，讓人心情極度舒暢快樂。",
        mode2_options: ["心曠神怡", "喜出望外", "美輪美奐", "垂涎三尺"]
      },
      {
        id: 6,
        word: "怒髮衝冠",
        pinyin: "nù fà chōng guān",
        meaning: "盛怒的樣子。頭髮直豎把帽子都頂起來了。",
        example: "聽到對方無理取鬧的謊言，他氣得怒髮衝冠。",
        mode1_q: "看到小偷居然膽敢當街搶劫老太太的皮包，路人們都（　　　　）。",
        mode1_options: ["怒髮衝冠", "得意忘形", "垂涎三尺", "生氣勃勃"],
        mode2_emoji: "😡👨‍🦱⚡",
        mode2_clue: "憤怒到了極點，連頭髮都氣得豎了起來！",
        mode2_options: ["怒髮衝冠", "目瞪口呆", "不翼而飛", "不計其數"]
      },
      {
        id: 7,
        word: "生氣勃勃",
        pinyin: "shēng qì bó bó",
        meaning: "形容充滿生命力與活力。",
        example: "春天到了，花園裡的植物抽出了新芽，一片生氣勃勃。",
        mode1_q: "校園裡的樹木都換上了綠衣，學童們歡快奔跑，展現出（　　　　）的氣息。",
        mode1_options: ["生氣勃勃", "美輪美奐", "心曠神怡", "喜出望外"],
        mode2_emoji: "🌱🌱🏃‍♂️",
        mode2_clue: "充滿朝氣與生命力，精神充沛！",
        mode2_options: ["生氣勃勃", "得意忘形", "垂涎三尺", "目瞪口呆"]
      },
      {
        id: 8,
        word: "垂涎三尺",
        pinyin: "chuí xián sān chǐ",
        meaning: "形容嘴饞想吃，或極想獲得某物。",
        example: "媽媽剛烤好的香草泡芙散發著濃郁香味，令人垂涎三尺。",
        mode1_q: "桌上擺滿了令人口水直流的精緻美食，弟弟看得（　　　　）。",
        mode1_options: ["垂涎三尺", "不翼而飛", "怒髮衝冠", "美輪美奐"],
        mode2_emoji: "🤤🍗🍰",
        mode2_clue: "看到極度美味的食物，口水流得好長好長！",
        mode2_options: ["垂涎三尺", "心曠神怡", "喜出望外", "不計其數"]
      },
      {
        id: 9,
        word: "美輪美奐",
        pinyin: "měi lún měi huàn",
        meaning: "形容房屋高大華美，或裝飾極為精美。",
        example: "這座新落成的歌劇院建築考究，真是美輪美奐。",
        mode1_q: "皇宮裡的壁畫與雕刻工藝細緻，整體顯得（　　　　）、氣派非凡。",
        mode1_options: ["美輪美奐", "生氣勃勃", "得意忘形", "不翼而飛"],
        mode2_emoji: "🏰✨👑",
        mode2_clue: "建築物高大雄偉、裝飾華麗絕美！",
        mode2_options: ["美輪美奐", "目瞪口呆", "怒髮衝冠", "心曠神怡"]
      },
      {
        id: 10,
        word: "目瞪口呆",
        pinyin: "mù dèng kǒu dāi",
        meaning: "因受驚或吃驚而發愣的樣子。",
        example: "看到魔術師將龐大的大象憑空變不見，台下觀眾全都目瞪口呆。",
        mode1_q: "突如其來的意外變化，讓在場的所有人都（　　　　），說不出話來。",
        mode1_options: ["目瞪口呆", "喜出望外", "垂涎三尺", "心曠神怡"],
        mode2_emoji: "😲😳👀",
        mode2_clue: "太過吃驚或震驚，眼睛張大、嘴巴張開愣在原地。",
        mode2_options: ["目瞪口呆", "得意忘形", "生氣勃勃", "不計其數"]
      }
    ];

    // Game State Variables
    let studentId = "";
    let currentStage = 1; // 1, 2, 3
    let currentQuestionIndex = 0; // 0 to 9 in each stage
    let fullQuestionList = [];
    let totalScore = 0;
    let secondsElapsed = 0;
    let timerInterval = null;
    let puzzleSelectedChars = [];

    // Helper: Shuffle array
    function shuffleArray(arr) {
      const array = [...arr];
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
      return array;
    }

    // Start Full Game with Student Login
    function startFullGame() {
      const input = document.getElementById('studentInput');
      const errorMsg = document.getElementById('inputErrorMsg');
      const val = input.value.trim();

      if (!val) {
        errorMsg.classList.remove('hidden');
        input.focus();
        return;
      }

      errorMsg.classList.add('hidden');
      studentId = val;
      playSound('click');

      // Reset Game Variables
      totalScore = 0;
      currentStage = 1;
      currentQuestionIndex = 0;
      secondsElapsed = 0;

      // Prepare randomized questions for 3 stages
      fullQuestionList = shuffleArray(IDIOMS_DB);

      // DOM Updates
      document.getElementById('homeScreen').classList.add('hidden');
      document.getElementById('resultScreen').classList.add('hidden');
      document.getElementById('gameScreen').classList.remove('hidden');
      document.getElementById('statsBar').classList.remove('hidden');

      document.getElementById('playerDisplay').innerText = studentId;
      document.getElementById('scoreDisplay').innerText = totalScore;

      // Start Timer
      if (timerInterval) clearInterval(timerInterval);
      timerInterval = setInterval(updateTimer, 1000);

      renderQuestion();
    }

    // Update Timer Display
    function updateTimer() {
      secondsElapsed++;
      const mins = String(Math.floor(secondsElapsed / 60)).padStart(2, '0');
      const secs = String(secondsElapsed % 60).padStart(2, '0');
      document.getElementById('timerDisplay').innerText = `${mins}:${secs}`;
    }

    // Render Current Question based on currentStage and currentQuestionIndex
    function renderQuestion() {
      const qData = fullQuestionList[currentQuestionIndex];
      const qArea = document.getElementById('questionArea');
      const feedbackBox = document.getElementById('feedbackBox');
      const nextBtn = document.getElementById('nextBtn');

      feedbackBox.classList.add('hidden');
      nextBtn.classList.add('hidden');
      qArea.innerHTML = '';

      // Total questions = 3 stages * 10 = 30
      const totalOverallQuestions = 30;
      const currentOverallIndex = (currentStage - 1) * 10 + currentQuestionIndex + 1;
      const progressPercent = (currentOverallIndex / totalOverallQuestions) * 100;

      document.getElementById('progressBar').style.width = `${progressPercent}%`;
      document.getElementById('questionProgress').innerText = `關卡 ${currentStage}/3 - 題目 ${currentQuestionIndex + 1}/10`;

      const stageTitles = {
        1: "第一關：情境與文意選詞",
        2: "第二關：Emoji 線索猜謎",
        3: "第三關：文字拼圖組成語"
      };
      document.getElementById('stageTitleTag').innerText = stageTitles[currentStage];

      if (currentStage === 1) {
        renderMode1(qData);
      } else if (currentStage === 2) {
        renderMode2(qData);
      } else if (currentStage === 3) {
        renderMode3(qData);
      }
    }

    // Render Mode 1
    function renderMode1(qData) {
      const qArea = document.getElementById('questionArea');
      const shuffledOptions = shuffleArray(qData.mode1_options);

      qArea.innerHTML = `
        <div class="space-y-3 text-center sm:text-left">
          <span class="text-xs font-bold text-indigo-500 uppercase tracking-wider bg-indigo-50 px-3 py-1 rounded-full">情境文意填空</span>
          <p class="text-lg sm:text-xl font-bold text-slate-800 leading-relaxed pt-2">
            ${qData.mode1_q}
          </p>
        </div>
        <div class="grid grid-cols-1 sm:grid-cols-2 gap-3 pt-4">
          ${shuffledOptions.map(opt => `
            <button onclick="checkAnswer('${opt}', '${qData.word}')" class="option-btn text-left p-4 rounded-2xl bg-white border border-slate-200 hover:border-indigo-500 hover:bg-indigo-50/50 font-bold text-slate-700 shadow-sm transition active:scale-95 flex items-center justify-between">
              <span>${opt}</span>
              <i class="fa-regular fa-circle text-slate-300"></i>
            </button>
          `).join('')}
        </div>
      `;
    }

    // Render Mode 2
    function renderMode2(qData) {
      const qArea = document.getElementById('questionArea');
      const shuffledOptions = shuffleArray(qData.mode2_options);

      qArea.innerHTML = `
        <div class="text-center space-y-3">
          <div class="text-5xl sm:text-6xl animate-float py-2">
            ${qData.mode2_emoji}
          </div>
          <p class="text-xs sm:text-sm font-bold text-purple-600 bg-purple-50 inline-block px-4 py-1.5 rounded-full">
            💡 線索提示：${qData.mode2_clue}
          </p>
          <p class="text-base sm:text-lg font-bold text-slate-800 pt-2">
            請根據以上的表情與線索，猜出對應的成語：
          </p>
        </div>
        <div class="grid grid-cols-1 sm:grid-cols-2 gap-3 pt-4">
          ${shuffledOptions.map(opt => `
            <button onclick="checkAnswer('${opt}', '${qData.word}')" class="option-btn text-center p-4 rounded-2xl bg-white border border-slate-200 hover:border-purple-500 hover:bg-purple-50/50 font-bold text-slate-700 shadow-sm transition active:scale-95">
              ${opt}
            </button>
          `).join('')}
        </div>
      `;
    }

    // Render Mode 3
    function renderMode3(qData) {
      puzzleSelectedChars = [];
      const qArea = document.getElementById('questionArea');

      const correctChars = qData.word.split('');
      const otherChars = IDIOMS_DB
        .filter(i => i.word !== qData.word)
        .flatMap(i => i.word.split(''));
      const extraChars = shuffleArray(otherChars).slice(0, 4);

      const allTiles = shuffleArray([...correctChars, ...extraChars]);

      qArea.innerHTML = `
        <div class="text-center space-y-2">
          <span class="text-xs font-bold text-emerald-600 bg-emerald-50 px-3 py-1 rounded-full">成語拼圖拆解</span>
          <p class="text-xs sm:text-sm text-slate-500">點選單字方塊，組合成正確的成語：</p>
          <p class="text-xs text-slate-400 italic">提示釋義：${qData.meaning}</p>
        </div>

        <div class="flex justify-center gap-2 sm:gap-3 my-4" id="puzzleSlots">
          <div class="slot-box w-12 h-12 sm:w-14 sm:h-14 rounded-2xl border-2 border-dashed border-slate-300 flex items-center justify-center text-xl sm:text-2xl font-black text-indigo-600 bg-slate-50 shadow-inner"></div>
          <div class="slot-box w-12 h-12 sm:w-14 sm:h-14 rounded-2xl border-2 border-dashed border-slate-300 flex items-center justify-center text-xl sm:text-2xl font-black text-indigo-600 bg-slate-50 shadow-inner"></div>
          <div class="slot-box w-12 h-12 sm:w-14 sm:h-14 rounded-2xl border-2 border-dashed border-slate-300 flex items-center justify-center text-xl sm:text-2xl font-black text-indigo-600 bg-slate-50 shadow-inner"></div>
          <div class="slot-box w-12 h-12 sm:w-14 sm:h-14 rounded-2xl border-2 border-dashed border-slate-300 flex items-center justify-center text-xl sm:text-2xl font-black text-indigo-600 bg-slate-50 shadow-inner"></div>
        </div>

        <div class="flex flex-wrap justify-center gap-2 max-w-md mx-auto pt-2" id="tilesPool">
          ${allTiles.map((char, idx) => `
            <button id="tile-${idx}" onclick="selectPuzzleTile('${char}', 'tile-${idx}', '${qData.word}')" class="tile-btn w-11 h-11 sm:w-12 sm:h-12 rounded-xl bg-white border border-slate-200 text-slate-800 font-black text-lg shadow hover:border-emerald-500 hover:text-emerald-600 active:scale-90 transition">
              ${char}
            </button>
          `).join('')}
        </div>

        <div class="text-center pt-2">
          <button onclick="resetPuzzleSlots()" class="text-xs font-bold text-slate-400 hover:text-slate-600 underline">
            <i class="fa-solid fa-arrow-rotate-left"></i> 清除重選
          </button>
        </div>
      `;
    }

    // Select Puzzle Tile in Mode 3
    function selectPuzzleTile(char, tileId, targetWord) {
      if (puzzleSelectedChars.length >= 4) return;

      playSound('click');
      puzzleSelectedChars.push({ char, tileId });

      const btn = document.getElementById(tileId);
      btn.disabled = true;
      btn.classList.add('opacity-30', 'cursor-not-allowed');

      const slots = document.querySelectorAll('.slot-box');
      const nextSlotIndex = puzzleSelectedChars.length - 1;
      slots[nextSlotIndex].innerText = char;
      slots[nextSlotIndex].classList.remove('border-dashed');
      slots[nextSlotIndex].classList.add('border-solid', 'border-indigo-500', 'bg-indigo-50');

      if (puzzleSelectedChars.length === 4) {
        const assembledWord = puzzleSelectedChars.map(item => item.char).join('');
        checkAnswer(assembledWord, targetWord);
      }
    }

    // Reset Puzzle Slots
    function resetPuzzleSlots() {
      playSound('click');
      puzzleSelectedChars.forEach(item => {
        const btn = document.getElementById(item.tileId);
        if (btn) {
          btn.disabled = false;
          btn.classList.remove('opacity-30', 'cursor-not-allowed');
        }
      });
      puzzleSelectedChars = [];

      const slots = document.querySelectorAll('.slot-box');
      slots.forEach(slot => {
        slot.innerText = '';
        slot.classList.remove('border-solid', 'border-indigo-500', 'bg-indigo-50');
        slot.classList.add('border-dashed');
      });
    }

    // Check Answer
    function checkAnswer(selectedAnswer, correctAnswer) {
      const isCorrect = (selectedAnswer === correctAnswer);
      const qData = fullQuestionList[currentQuestionIndex];

      const buttons = document.querySelectorAll('.option-btn, .tile-btn');
      buttons.forEach(btn => {
        btn.disabled = true;
        btn.classList.add('pointer-events-none');
      });

      const feedbackBox = document.getElementById('feedbackBox');
      const feedbackIcon = document.getElementById('feedbackIcon');
      const feedbackTitle = document.getElementById('feedbackTitle');
      const feedbackText = document.getElementById('feedbackText');
      const nextBtn = document.getElementById('nextBtn');

      feedbackBox.classList.remove('hidden');

      if (isCorrect) {
        playSound('correct');
        totalScore += 10;
        document.getElementById('scoreDisplay').innerText = totalScore;

        feedbackBox.className = "mt-6 p-4 rounded-2xl border bg-emerald-50 border-emerald-200 text-emerald-900 animate-fade-in";
        feedbackIcon.className = "fa-solid fa-circle-check text-emerald-500 text-xl";
        feedbackTitle.innerText = "答對了！太棒了！";

        confetti({
          particleCount: 20,
          spread: 50,
          origin: { y: 0.7 }
        });
      } else {
        playSound('wrong');
        feedbackBox.className = "mt-6 p-4 rounded-2xl border bg-rose-50 border-rose-200 text-rose-900 animate-fade-in";
        feedbackIcon.className = "fa-solid fa-circle-xmark text-rose-500 text-xl";
        feedbackTitle.innerText = `答錯了！正確答案是：【${correctAnswer}】`;
      }

      feedbackText.innerHTML = `
        <span class="font-bold">【${qData.word}】</span> (${qData.pinyin})<br/>
        <span class="font-semibold">釋義：</span>${qData.meaning}<br/>
        <span class="font-semibold">例句：</span>${qData.example}
      `;

      nextBtn.classList.remove('hidden');
    }

    // Next Question or Next Stage
    function nextQuestion() {
      playSound('click');
      currentQuestionIndex++;

      if (currentQuestionIndex < 10) {
        renderQuestion();
      } else {
        // Stage completed
        if (currentStage < 3) {
          currentStage++;
          currentQuestionIndex = 0;
          fullQuestionList = shuffleArray(IDIOMS_DB); // Reshuffle for next stage
          renderQuestion();
        } else {
          finishGame();
        }
      }
    }

    // Finish Game & Leaderboard Save
    function finishGame() {
      if (timerInterval) clearInterval(timerInterval);
      playSound('win');

      document.getElementById('gameScreen').classList.add('hidden');
      document.getElementById('resultScreen').classList.remove('hidden');

      document.getElementById('finalPlayerName').innerText = studentId;
      document.getElementById('finalScore').innerText = totalScore;

      const mins = String(Math.floor(secondsElapsed / 60)).padStart(2, '0');
      const secs = String(secondsElapsed % 60).padStart(2, '0');
      const formattedTime = `${mins}:${secs}`;
      document.getElementById('finalTime').innerText = formattedTime;

      // Save Record to LocalStorage Leaderboard
      saveLeaderboardRecord(studentId, totalScore, secondsElapsed);

      // Badge logic
      const badge = document.getElementById('performanceBadge');
      if (totalScore >= 270) {
        badge.innerText = "🎉 完美通關！你是班級成語大師！";
        badge.className = "p-3 bg-amber-100 text-amber-800 rounded-xl font-bold text-xs sm:text-sm";
      } else if (totalScore >= 200) {
        badge.innerText = "🌟 表現優異！成語實力非常扎實！";
        badge.className = "p-3 bg-indigo-50 text-indigo-700 rounded-xl font-bold text-xs sm:text-sm";
      } else {
        badge.innerText = "💪 繼續加油！多複習成語圖鑑再接再厲！";
        badge.className = "p-3 bg-slate-100 text-slate-700 rounded-xl font-bold text-xs sm:text-sm";
      }

      confetti({
        particleCount: 100,
        spread: 100,
        origin: { y: 0.5 }
      });
    }

    // Save record to LocalStorage
    function saveLeaderboardRecord(name, score, timeInSec) {
      const records = JSON.parse(localStorage.getItem('idiom_adventure_leaderboard') || '[]');
      
      const now = new Date();
      const dateStr = `${now.getMonth()+1}/${now.getDate()} ${String(now.getHours()).padStart(2,'0')}:${String(now.getMinutes()).padStart(2,'0')}`;

      records.push({
        id: Date.now(),
        name: name,
        score: score,
        time: timeInSec,
        date: dateStr
      });

      // Sort logic: Score high to low, then Time low to high
      records.sort((a, b) => {
        if (b.score !== a.score) {
          return b.score - a.score;
        }
        return a.time - b.time;
      });

      localStorage.setItem('idiom_adventure_leaderboard', JSON.stringify(records));
    }

    // Open Leaderboard Modal
    function openLeaderboardModal() {
      playSound('click');
      const records = JSON.parse(localStorage.getItem('idiom_adventure_leaderboard') || '[]');
      const listContainer = document.getElementById('leaderboardList');

      if (records.length === 0) {
        listContainer.innerHTML = `
          <div class="text-center py-12 text-slate-400">
            <i class="fa-solid fa-trophy text-4xl mb-2 opacity-30"></i>
            <p class="text-sm font-bold">目前尚無排行榜記錄</p>
            <p class="text-xs">快成為第一個完成關卡的人吧！</p>
          </div>
        `;
      } else {
        // Show Top 10
        const top10 = records.slice(0, 10);
        listContainer.innerHTML = top10.map((item, idx) => {
          const mins = String(Math.floor(item.time / 60)).padStart(2, '0');
          const secs = String(item.time % 60).padStart(2, '0');
          
          let rankBadge = `<span class="w-7 h-7 rounded-full bg-slate-100 text-slate-600 font-bold text-xs flex items-center justify-center">${idx + 1}</span>`;
          if (idx === 0) rankBadge = `<span class="w-7 h-7 rounded-full bg-amber-400 text-white font-black text-xs flex items-center justify-center shadow-md">🥇</span>`;
          if (idx === 1) rankBadge = `<span class="w-7 h-7 rounded-full bg-slate-300 text-slate-700 font-black text-xs flex items-center justify-center shadow-md">🥈</span>`;
          if (idx === 2) rankBadge = `<span class="w-7 h-7 rounded-full bg-amber-700 text-white font-black text-xs flex items-center justify-center shadow-md">🥉</span>`;

          return `
            <div class="flex items-center justify-between p-3 rounded-2xl ${idx < 3 ? 'bg-amber-50/60 border border-amber-200/60' : 'bg-slate-50 border border-slate-100'}">
              <div class="flex items-center space-x-3">
                ${rankBadge}
                <div>
                  <h4 class="font-bold text-slate-800 text-sm">${item.name}</h4>
                  <p class="text-[10px] text-slate-400">${item.date}</p>
                </div>
              </div>
              <div class="text-right">
                <p class="font-black text-indigo-600 text-base">${item.score} <span class="text-xs font-normal text-slate-400">分</span></p>
                <p class="text-xs font-mono text-slate-500"><i class="fa-solid fa-clock text-[10px]"></i> ${mins}:${secs}</p>
              </div>
            </div>
          `;
        }).join('');
      }

      document.getElementById('leaderboardModal').classList.remove('hidden');
    }

    function closeLeaderboardModal() {
      playSound('click');
      document.getElementById('leaderboardModal').classList.add('hidden');
    }

    // Confirm Clear Leaderboard
    function confirmClearLeaderboard() {
      playSound('click');
      if (confirm("確定要清除所有排行榜數據嗎？（通常用於新班級上課前重置）")) {
        localStorage.removeItem('idiom_adventure_leaderboard');
        openLeaderboardModal(); // Refresh modal
      }
    }

    // Return to Home
    function showHomeScreen() {
      playSound('click');
      if (timerInterval) clearInterval(timerInterval);
      document.getElementById('gameScreen').classList.add('hidden');
      document.getElementById('resultScreen').classList.add('hidden');
      document.getElementById('statsBar').classList.add('hidden');
      document.getElementById('homeScreen').classList.remove('hidden');
    }

    // Idiom Modal Controls
    function openIdiomListModal() {
      playSound('click');
      const modalList = document.getElementById('idiomModalList');
      modalList.innerHTML = IDIOMS_DB.map((item, index) => `
        <div class="p-4 rounded-2xl bg-slate-50 border border-slate-200/80 hover:border-indigo-300 transition">
          <div class="flex items-center justify-between">
            <span class="text-base font-black text-indigo-600">${index + 1}. ${item.word}</span>
            <span class="text-xs font-mono text-slate-400">${item.pinyin}</span>
          </div>
          <p class="text-xs sm:text-sm font-medium text-slate-700 mt-1"><span class="font-bold text-slate-500">釋義：</span>${item.meaning}</p>
          <p class="text-xs text-slate-500 mt-1 bg-white p-2.5 rounded-xl border border-slate-100"><span class="font-bold text-indigo-500">例句：</span>${item.example}</p>
        </div>
      `).join('');

      document.getElementById('idiomModal').classList.remove('hidden');
    }

    function closeIdiomListModal() {
      playSound('click');
      document.getElementById('idiomModal').classList.add('hidden');
    }
  </script>
</body>
</html>
