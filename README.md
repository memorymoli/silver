<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <!-- 測試階段防爬蟲收錄 -->
  <meta name="robots" content="noindex, nofollow">
  <!-- 強制淺色護眼模式 -->
  <meta name="color-scheme" content="light only">
  <title>美茉莉 - 銀髮備忘錄</title>
  <style>
    :root {
      color-scheme: light only;
    }
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: -apple-system, BlinkMacSystemFont, "PingFang TC", "Microsoft JhengHei", "Noto Sans TC", sans-serif;
    }

    /* 暖燕麥米白護眼底色 */
    body {
      background-color: #F5F1EB !important;
      color: #0F172A !important;
      display: flex;
      flex-direction: column;
      align-items: center;
      min-height: 100vh;
      padding: 10px 10px 32px 10px;
    }

    /* 頂部看板：天氣、日期、時間滿版特大字且絕不出框 */
    .dashboard-card {
      width: 100%;
      max-width: 520px;
      background-color: #1E293B;
      border: 3px solid #334155;
      border-radius: 20px;
      padding: 14px 12px;
      margin-bottom: 14px;
      box-shadow: 0 6px 18px rgba(0,0,0,0.12);
      display: flex;
      flex-direction: column;
      gap: 8px;
      overflow: hidden;
    }
    .weather-row {
      display: flex;
      justify-content: space-between;
      align-items: center;
      border-bottom: 2px solid #334155;
      padding-bottom: 8px;
      gap: 4px;
      width: 100%;
    }
    .weather-info {
      display: flex;
      align-items: center;
      gap: 6px;
      font-size: clamp(32px, 9.2vw, 44px);
      font-weight: 900;
      color: #38BDF8;
      white-space: nowrap;
      line-height: 1.1;
    }
    .weather-temp {
      font-size: clamp(36px, 10.5vw, 48px);
      font-weight: 900;
      color: #FFFFFF;
      white-space: nowrap;
      line-height: 1.1;
    }
    .datetime-col {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 4px;
      width: 100%;
    }
    .date-text {
      font-size: clamp(28px, 8.2vw, 38px);
      color: #E2E8F0;
      font-weight: 900;
      white-space: nowrap;
      line-height: 1.15;
    }
    .time-text {
      font-size: clamp(42px, 12.2vw, 56px);
      font-weight: 900;
      color: #F8FAFC;
      letter-spacing: 1px;
      white-space: nowrap;
      line-height: 1.1;
    }

    /* 主副標題設計 */
    header { text-align: center; margin-bottom: 14px; width: 100%; max-width: 520px; }
    h1 {
      font-size: clamp(46px, 13.5vw, 60px);
      color: #10B981;
      letter-spacing: 2px;
      font-weight: 900;
      line-height: 1.12;
      text-shadow: 0 2px 8px rgba(16, 185, 129, 0.25);
    }
    .sub-title {
      font-size: clamp(34px, 10vw, 46px);
      color: #DC2626;
      font-weight: 900;
      letter-spacing: 3px;
      margin-top: 4px;
      text-shadow: 0 2px 6px rgba(220, 38, 38, 0.18);
    }

    /* 區塊卡片共通樣式 */
    .app-card {
      width: 100%;
      max-width: 520px;
      background-color: #FFFFFF;
      border: 3px solid #E2E8F0;
      border-radius: 22px;
      padding: 16px 12px;
      box-shadow: 0 8px 24px rgba(0,0,0,0.06);
      margin-bottom: 18px;
      overflow: hidden;
    }
    .section-label {
      font-size: clamp(32px, 9.2vw, 40px);
      color: #0F172A;
      margin-bottom: 14px;
      font-weight: 900;
      text-align: center;
      line-height: 1.2;
    }

    /* 2 欄按鈕格線 */
    .category-grid-2x {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 12px;
      margin-bottom: 16px;
    }

    /* 備忘錄四大類：按下先播提示音再自動錄音 */
    .memo-box {
      border: 4px solid transparent;
      font-size: clamp(28px, 8.2vw, 36px);
      font-weight: 900;
      padding: 16px 4px;
      border-radius: 18px;
      cursor: pointer;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 6px;
      text-align: center;
      min-height: 125px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.06);
      -webkit-tap-highlight-color: transparent;
      transition: all 0.2s ease;
      word-break: keep-all;
    }
    .memo-box span.icon { font-size: clamp(42px, 11vw, 52px); line-height: 1; }

    .cat-daily { background-color: #F3E8FF; border-color: #E9D5FF; color: #7E22CE; }
    .cat-hosp { background-color: #E0F2FE; border-color: #BAE6FD; color: #0369A1; }
    .cat-fit { background-color: #DCFCE7; border-color: #BBF7D0; color: #15803D; }
    .cat-med { background-color: #FFE4E6; border-color: #FDA4AF; color: #BE123C; }

    .memo-box.active {
      outline: 5px solid #0F172A;
      outline-offset: -2px;
      transform: scale(1.02);
      box-shadow: 0 6px 18px rgba(15, 23, 42, 0.25);
    }

    .memo-box.recording-card {
      background-color: #FEE2E2 !important;
      border-color: #DC2626 !important;
      color: #B91C1C !important;
      animation: pulseCard 1.2s infinite;
    }
    @keyframes pulseCard {
      0% { box-shadow: 0 0 0 0 rgba(220, 38, 38, 0.7); }
      70% { box-shadow: 0 0 0 14px rgba(220, 38, 38, 0); }
      100% { box-shadow: 0 0 0 0 rgba(220, 38, 38, 0); }
    }

    /* 錄音狀態大字提示橫幅 */
    .mic-status-banner {
      display: none;
      background-color: #FEF2F2;
      border: 3px solid #EF4444;
      border-radius: 16px;
      padding: 12px 10px;
      margin-bottom: 14px;
      text-align: center;
    }
    .mic-status-text {
      font-size: clamp(22px, 6.2vw, 27px);
      color: #DC2626;
      font-weight: 900;
      line-height: 1.35;
      margin-bottom: 8px;
    }
    .stop-rec-btn {
      background-color: #DC2626;
      color: #FFFFFF;
      border: none;
      font-size: clamp(22px, 6vw, 26px);
      font-weight: 900;
      padding: 10px 24px;
      border-radius: 12px;
      cursor: pointer;
    }

    /* 純打字輸入區 */
    .sub-input-label {
      font-size: clamp(26px, 7.5vw, 32px);
      color: #1E293B;
      margin-top: 4px;
      margin-bottom: 10px;
      font-weight: 900;
    }
    .text-input-full {
      width: 100%;
      background-color: #F8FAFC;
      border: 3px solid #CBD5E1;
      border-radius: 14px;
      color: #0F172A;
      font-size: clamp(24px, 6.8vw, 30px);
      font-weight: 900;
      padding: 16px 12px;
      outline: none;
      margin-bottom: 16px;
    }
    .text-input-full:focus { border-color: #0284C7; background-color: #FFFFFF; }

    /* 特大字手動鬧鐘按鈕 */
    .alarm-trigger-btn {
      width: 100%;
      background-color: #F8FAFC;
      border: 3px solid #64748B;
      border-radius: 16px;
      padding: 16px 12px;
      margin-bottom: 18px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      cursor: pointer;
      gap: 8px;
    }
    .alarm-trigger-label {
      font-size: clamp(24px, 6.8vw, 30px);
      color: #1E293B;
      font-weight: 900;
      white-space: nowrap;
    }
    .alarm-trigger-value {
      font-size: clamp(26px, 7.4vw, 34px);
      color: #0284C7;
      font-weight: 900;
      white-space: nowrap;
    }

    .add-btn {
      width: 100%;
      background: linear-gradient(145deg, #0284C7, #0369A1);
      border: none;
      color: #FFFFFF;
      font-size: clamp(26px, 7.5vw, 34px);
      font-weight: 900;
      padding: 18px 8px;
      border-radius: 16px;
      cursor: pointer;
      box-shadow: 0 6px 16px rgba(2, 132, 199, 0.35);
      white-space: nowrap;
    }

    /* 銀髮生活區按鈕：字體放到最大，長輩專用超大字級 */
    .life-box {
      border: 4px solid transparent;
      font-size: clamp(28px, 8.2vw, 36px);
      font-weight: 900;
      padding: 18px 6px;
      border-radius: 18px;
      cursor: pointer;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 6px;
      text-decoration: none;
      text-align: center;
      min-height: 135px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.06);
      -webkit-tap-highlight-color: transparent;
      transition: all 0.2s ease;
      line-height: 1.2;
    }
    .life-box span.icon { font-size: clamp(42px, 11.5vw, 52px); line-height: 1; }
    .life-sub-text {
      font-size: clamp(20px, 5.8vw, 25px);
      font-weight: 900;
      margin-top: 2px;
      display: block;
    }

    /* 單放一排滿版特大卡片（居家無障礙空間諮詢專用） */
    .full-span-box {
      grid-column: span 2;
      background-color: #F8FAFC;
      border: 4px solid #94A3B8;
      color: #0F172A;
      padding: 18px 10px;
    }
    .full-span-title {
      font-size: clamp(26px, 7.6vw, 34px);
      font-weight: 900;
      line-height: 1.2;
      margin-bottom: 12px;
      white-space: nowrap;
    }
    .sub-link-row-2 {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 10px;
      width: 100%;
    }
    .sub-link-btn-big {
      display: block;
      width: 100%;
      color: #FFFFFF;
      font-size: clamp(24px, 6.8vw, 30px);
      font-weight: 900;
      padding: 14px 4px;
      border-radius: 14px;
      text-decoration: none;
      text-align: center;
      box-shadow: 0 4px 10px rgba(0,0,0,0.18);
      white-space: nowrap;
    }

    .cat-guide { background-color: #FCE7F3; border-color: #FBCFE8; color: #BE185D; }
    .cat-mind { background-color: #CCFBF1; border-color: #99F6E4; color: #0F766E; }
    .cat-nou { background-color: #E0E7FF; border-color: #C7D2FE; color: #3730A3; }
    .link-box { background-color: #F8FAFC; border: 3px solid #CBD5E1; color: #1E293B; }
    .cat-fraud { background-color: #FEE2E2; border-color: #FCA5A5; color: #991B1B; }
    .cat-feedback { background-color: #FEF3C7; border-color: #FCD34D; color: #B45309; }
    .cat-sing { background-color: #FFF7ED; border-color: #FED7AA; color: #C2410C; }
    .cat-laugh { background-color: #FEF9C3; border-color: #FDE047; color: #A16207; }

    /* 備忘清單區塊 */
    .list-container {
      width: 100%;
      max-width: 520px;
      display: flex;
      flex-direction: column;
      gap: 14px;
      margin-bottom: 22px;
    }
    .list-header {
      font-size: clamp(28px, 8.2vw, 36px);
      color: #0F172A;
      border-bottom: 3px solid #CBD5E1;
      padding-bottom: 8px;
      font-weight: 900;
    }
    .note-card {
      background-color: #FFFFFF;
      border: 3px solid #E2E8F0;
      border-radius: 18px;
      padding: 16px 12px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 10px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.06);
      overflow: hidden;
    }
    .cat-tag {
      font-size: clamp(20px, 5.6vw, 24px);
      padding: 5px 12px;
      border-radius: 8px;
      font-weight: 900;
      display: inline-block;
      margin-bottom: 8px;
    }
    .tag-吃藥 { background-color: #FFE4E6; color: #BE123C; }
    .tag-回診 { background-color: #E0F2FE; color: #0369A1; }
    .tag-運動健身 { background-color: #DCFCE7; color: #15803D; }
    .tag-一般日常 { background-color: #F3E8FF; color: #7E22CE; }

    .note-text {
      font-size: clamp(26px, 7.4vw, 34px);
      color: #0F172A;
      font-weight: 900;
      line-height: 1.3;
      word-break: break-word;
    }
    .note-alarm {
      font-size: clamp(22px, 6.2vw, 28px);
      color: #0284C7;
      margin-top: 6px;
      font-weight: 900;
    }
    
    .card-actions {
      display: flex;
      flex-direction: column;
      gap: 8px;
      min-width: 118px;
      flex-shrink: 0;
    }
    .btn-speak, .btn-line, .btn-del {
      padding: 10px 6px;
      border-radius: 10px;
      font-size: clamp(19px, 5.2vw, 22px);
      font-weight: 900;
      cursor: pointer;
      text-align: center;
      white-space: nowrap;
    }
    .btn-speak { background: #F1F5F9; border: 2px solid #CBD5E1; color: #1E293B; }
    .btn-line { background: #06C755; border: 2px solid #05A848; color: #FFFFFF; text-decoration: none; display: block; }
    .btn-del { background: #FFF1F2; border: 2px solid #FECDD3; color: #E11D48; }

    /* 免責聲明區塊 */
    .disclaimer-box {
      width: 100%;
      max-width: 520px;
      background-color: #EAE5DC;
      border: 2px solid #D6D0C4;
      border-radius: 16px;
      padding: 16px 14px;
      margin-top: 6px;
      color: #334155;
      font-size: clamp(18px, 5vw, 21px);
      line-height: 1.55;
      font-weight: 600;
    }
    .disclaimer-title {
      font-size: clamp(21px, 6vw, 25px);
      font-weight: 900;
      color: #0F172A;
      margin-bottom: 8px;
    }

    /* 通用彈出視窗樣式 */
    .modal-overlay {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(15, 23, 42, 0.82);
      z-index: 2000;
      justify-content: center;
      align-items: center;
      padding: 12px;
    }
    .modal-card {
      background-color: #FFFFFF;
      width: 100%;
      max-width: 500px;
      max-height: 90vh;
      overflow-y: auto;
      border-radius: 24px;
      padding: 20px 14px;
      border: 4px solid #0284C7;
      box-shadow: 0 15px 35px rgba(0,0,0,0.4);
    }
    .modal-title {
      font-size: clamp(30px, 8.5vw, 38px);
      font-weight: 900;
      color: #0F172A;
      text-align: center;
      margin-bottom: 14px;
    }

    /* 特大號數字鬧鐘選擇網格 */
    .clock-grid-4 {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 10px;
      margin-bottom: 16px;
    }
    .clock-grid-3 {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 10px;
      margin-bottom: 16px;
    }
    .num-pick-btn {
      background-color: #F1F5F9;
      border: 3px solid #CBD5E1;
      border-radius: 14px;
      color: #0F172A;
      font-size: clamp(28px, 8vw, 36px);
      font-weight: 900;
      padding: 14px 4px;
      text-align: center;
      cursor: pointer;
    }
    .num-pick-btn:active, .num-pick-btn.active-num {
      background-color: #0284C7;
      border-color: #0369A1;
      color: #FFFFFF;
    }
    .no-alarm-btn {
      width: 100%;
      background-color: #FFF1F2;
      border: 3px solid #FDA4AF;
      color: #BE123C;
      font-size: clamp(24px, 6.8vw, 30px);
      font-weight: 900;
      padding: 14px;
      border-radius: 14px;
      cursor: pointer;
      margin-bottom: 12px;
    }

    /* 互動教學與建議回饋彈窗 */
    .play-guide-btn {
      width: 100%;
      background: linear-gradient(145deg, #10B981, #059669);
      color: #FFFFFF;
      border: none;
      font-size: clamp(24px, 6.8vw, 30px);
      font-weight: 900;
      padding: 16px 8px;
      border-radius: 14px;
      cursor: pointer;
      margin-bottom: 16px;
    }
    .feedback-textarea {
      width: 100%;
      min-height: 130px;
      background-color: #F8FAFC;
      border: 3px solid #CBD5E1;
      border-radius: 16px;
      padding: 14px;
      font-size: clamp(24px, 6.8vw, 28px);
      font-weight: 800;
      color: #0F172A;
      margin-bottom: 14px;
      outline: none;
    }
    .send-line-feedback-btn {
      display: block;
      width: 100%;
      background-color: #06C755;
      color: #FFFFFF;
      border: none;
      font-size: clamp(24px, 6.8vw, 30px);
      font-weight: 900;
      padding: 16px 8px;
      border-radius: 14px;
      text-align: center;
      text-decoration: none;
      cursor: pointer;
      margin-bottom: 10px;
    }
    .step-item {
      background-color: #F8FAFC;
      border: 2px solid #E2E8F0;
      border-radius: 16px;
      padding: 14px 12px;
      margin-bottom: 12px;
    }
    .step-num {
      font-size: clamp(24px, 6.8vw, 28px);
      font-weight: 900;
      color: #0284C7;
      margin-bottom: 4px;
    }
    .step-desc {
      font-size: clamp(21px, 6vw, 25px);
      color: #1E293B;
      font-weight: 800;
      line-height: 1.4;
    }
    .close-modal-btn {
      width: 100%;
      background-color: #334155;
      color: #FFFFFF;
      border: none;
      font-size: clamp(24px, 6.8vw, 30px);
      font-weight: 900;
      padding: 16px 8px;
      border-radius: 14px;
      cursor: pointer;
      margin-top: 6px;
    }
  </style>
</head>
<body>

  <!-- 最上層：天氣、日期、時間各占一行放到最大且不出框 -->
  <div class="dashboard-card">
    <div class="weather-row">
      <div class="weather-info">
        <span id="weather-icon">⛅</span>
        <span id="weather-desc">載入中</span>
      </div>
      <div class="weather-temp" id="weather-temp">--°C</div>
    </div>
    <div class="datetime-col">
      <div class="date-text" id="current-date">--月--日 星期-</div>
      <div class="time-text" id="current-time">00:00:00</div>
    </div>
  </div>

  <header>
    <h1>美茉莉</h1>
    <div class="sub-title">銀髮備忘錄</div>
  </header>

  <!-- 區塊一：點選要記的事情 -->
  <div class="app-card">
    <div class="section-label">點選要記的事情</div>

    <div class="category-grid-2x">
      <button class="memo-box cat-daily active" id="btn-cat-daily" onclick="selectAndRecord('一般日常', this)">
        <span class="icon">📝</span>
        <span>一般日常</span>
      </button>

      <button class="memo-box cat-hosp" id="btn-cat-hosp" onclick="selectAndRecord('回診', this)">
        <span class="icon">🏥</span>
        <span>回診</span>
      </button>

      <button class="memo-box cat-fit" id="btn-cat-fit" onclick="selectAndRecord('運動健身', this)">
        <span class="icon">🏃</span>
        <span>運動健身</span>
      </button>

      <button class="memo-box cat-med" id="btn-cat-med" onclick="selectAndRecord('吃藥', this)">
        <span class="icon">💊</span>
        <span>吃藥</span>
      </button>
    </div>

    <!-- 錄音進行中提示橫幅 -->
    <div class="mic-status-banner" id="mic-banner">
      <div class="mic-status-text" id="mic-tip">🔴 正在準備錄音...</div>
      <button type="button" class="stop-rec-btn" onclick="stopRecord()">⏹️ 講完了（停止錄音）</button>
    </div>

    <!-- 打字輸入框 -->
    <div class="sub-input-label">打字輸入：</div>
    <input type="text" id="note-input" class="text-input-full" placeholder="點上方按鈕直接講，或在此打字">

    <!-- 特大字手動鬧鐘按鈕 -->
    <button type="button" class="alarm-trigger-btn" onclick="openClockModal()">
      <span class="alarm-trigger-label">⏰ 提醒時間：</span>
      <span class="alarm-trigger-value" id="alarm-display">不設鬧鐘 ▾</span>
    </button>

    <button class="add-btn" onclick="addNote()">＋ 記下來並設定提醒</button>
  </div>

  <!-- 區塊二：記下的行程清單 -->
  <div class="list-container">
    <div class="list-header">
      📋 記下的行程清單
    </div>
    <div id="notes-list"></div>
  </div>

  <!-- 區塊三：銀髮生活 -->
  <div class="app-card">
    <div class="section-label">銀髮生活</div>

    <div class="category-grid-2x">
      <!-- 第一排（最上面）：使用教學、心靈有方 -->
      <button class="life-box cat-guide" onclick="openGuideModal()">
        <span class="icon">📖</span>
        <span>使用教學</span>
      </button>

      <a href="https://m.youtube.com/@fongyuanacademy" target="_blank" rel="noopener noreferrer" class="life-box cat-mind">
        <span class="icon">🧘</span>
        <span>心靈有方</span>
      </a>

      <!-- 第二排（單放滿版大卡片）：居家無障礙空間諮詢 -->
      <div class="life-box full-span-box">
        <div class="full-span-title">🛠️ 居家無障礙空間諮詢</div>
        <div class="sub-link-row-2">
          <a href="https://maps.app.goo.gl/tiNDgm5CQbmy8PhT7?g_st=al" target="_blank" rel="noopener noreferrer" class="sub-link-btn-big" style="background:#0284C7;">📍 地圖導航</a>
          <a href="https://www.facebook.com/share/1DvCdVuFQJ/" target="_blank" rel="noopener noreferrer" class="sub-link-btn-big" style="background:#1877F2;">📘 臉書專頁</a>
        </div>
      </div>

      <!-- 第三排：終身學習、老當益壯 -->
      <a href="https://www.nou.edu.tw" target="_blank" rel="noopener noreferrer" class="life-box cat-nou">
        <span class="icon">🎓</span>
        <span>終身學習<span class="life-sub-text">(空中大學)</span></span>
      </a>

      <a href="https://45plus.wda.gov.tw" target="_blank" rel="noopener noreferrer" class="life-box link-box">
        <span class="icon">💼</span>
        <span>老當益壯<span class="life-sub-text">(中高齡就業)</span></span>
      </a>

      <!-- 第四排：防詐騙提醒、備忘錄改善建議 -->
      <a href="https://165.npa.gov.tw" target="_blank" rel="noopener noreferrer" class="life-box cat-fraud">
        <span class="icon">🛡️</span>
        <span>防詐騙提醒<span class="life-sub-text">(165 官網)</span></span>
      </a>

      <button class="life-box cat-feedback" onclick="openFeedbackModal()">
        <span class="icon">💬</span>
        <span style="line-height: 1.15;">備忘錄<br>改善建議</span>
      </button>

      <!-- 第五排：每日一唱、每日一笑 -->
      <a href="https://www.youtube.com/results?search_query=%E5%9C%8B%E8%AA%9E%E8%80%81%E6%AD%8C+%E5%8D%A1%E6%8B%89OK+%E4%BC%B4%E5%94%B1" target="_blank" rel="noopener noreferrer" class="life-box cat-sing">
        <span class="icon">🎤</span>
        <span>每日一唱</span>
      </a>

      <a href="https://www.youtube.com/results?search_query=%E7%9B%B8%E8%81%B2%E7%B2%BE%E9%81%B8+%E5%B9%BD%E9%BB%98%E5%96%9C%E5%8A%87+%E9%80%97%E7%AC%91%E7%9F%AD%E7%89%87" target="_blank" rel="noopener noreferrer" class="life-box cat-laugh">
        <span class="icon">😄</span>
        <span>每日一笑</span>
      </a>
    </div>
  </div>

  <!-- 免責聲明與隱私宣告區塊 -->
  <div class="disclaimer-box">
    <div class="disclaimer-title">⚖️ 服務免責與隱私聲明</div>
    <p>1. <strong>醫療與用藥免責</strong>：本網站僅提供日常生活備忘輔助，用藥劑量、時間及回診事宜，請務必遵照專業醫師與藥師指示。</p>
    <p>2. <strong>提醒功能說明</strong>：網頁鬧鐘提醒可能受限於手機瀏覽器背景休眠或省電模式影響，重要事項建議同步設定手機內建鬧鐘。</p>
    <p>3. <strong>零個資留存安全</strong>：您的所有輸入與備忘紀錄僅儲存於您個人的手機瀏覽器內部，本站不設資料庫、絕不蒐集或上傳任何個人隱私資料。</p>
  </div>

  <!-- 特大號數字鬧鐘設定彈窗 -->
  <div class="modal-overlay" id="clock-modal" onclick="if(event.target === this) closeClockModal()">
    <div class="modal-card">
      <div class="modal-title" id="clock-modal-title">⏰ 請點選「幾點」</div>
      
      <div id="hour-step-box">
        <div class="clock-grid-4" id="hour-grid"></div>
        <button type="button" class="no-alarm-btn" onclick="clearAlarmSetting()">❌ 不設鬧鐘</button>
      </div>

      <div id="minute-step-box" style="display:none;">
        <div class="clock-grid-3" id="minute-grid"></div>
      </div>

      <button type="button" class="close-modal-btn" onclick="closeClockModal()">取消關閉</button>
    </div>
  </div>

  <!-- 備忘錄改善建議彈窗 -->
  <div class="modal-overlay" id="feedback-modal" onclick="if(event.target === this) closeFeedbackModal()">
    <div class="modal-card" style="border-color: #D97706;">
      <div class="modal-title">💬 備忘錄改善建議</div>
      <p style="font-size: 22px; color: #334155; font-weight: 800; margin-bottom: 12px; line-height: 1.4;">
        請寫下您覺得哪裡不好用、或希望增加什麼功能：
      </p>
      <textarea id="feedback-input" class="feedback-textarea" placeholder="例如：字可以再大一點、希望增加什麼功能..."></textarea>
      <button type="button" class="send-line-feedback-btn" onclick="sendFeedbackViaLine()">📲 透過 LINE 傳送建議</button>
      <button type="button" class="close-modal-btn" onclick="closeFeedbackModal()">關閉視窗</button>
    </div>
  </div>

  <!-- 互動式有聲圖文教學視窗 -->
  <div class="modal-overlay" id="guide-modal" onclick="if(event.target === this) closeGuideModal()">
    <div class="modal-card" style="border-color: #10B981;">
      <div class="modal-title">📖 美茉莉使用教學</div>
      <button class="play-guide-btn" onclick="playFullVoiceGuide()">🔊 點這裡，語音念給您聽</button>

      <div class="step-item">
        <div class="step-num">第一步：點按鈕直接開始說話</div>
        <div class="step-desc">直接點一下【一般日常】、【回診】、【運動健身】或【吃藥】，就會馬上開始錄音！</div>
      </div>

      <div class="step-item">
        <div class="step-num">第二步：說出時間自動設鬧鐘</div>
        <div class="step-desc">只要說出例如「下午三點吃降血壓藥」，系統會自動幫您設定好 15:00 的鬧鐘！</div>
      </div>

      <div class="step-item">
        <div class="step-num">第三步：手動選大字鬧鐘並記下</div>
        <div class="step-desc">也可以點【⏰ 提醒時間】用超大數字挑選幾點幾分，最後按藍色按鈕記下來。</div>
      </div>

      <div class="step-item">
        <div class="step-num">第四步：傳給兒女或念出來</div>
        <div class="step-desc">在清單中按【🔊 念出】可再聽一次，按【📲 LINE兒女】可直接傳給家人看。</div>
      </div>

      <button class="close-modal-btn" onclick="closeGuideModal()">我知道了，關閉教學</button>
    </div>
  </div>

  <script>
    const STORAGE_KEY = 'senior_care_notes_v24';
    let currentCategory = '一般日常';
    let selectedHour = '';
    let selectedMinute = '00';
    let isRecording = false;
    let recognition = null;
    let notifiedIds = new Set();
    let energeticGirlVoice = null;
    let recordTimer = null;
    let countdownSec = 15;
    let savedPhrases = [];

    function escapeHtml(str) {
      if (!str) return '';
      return String(str)
        .replace(/&/g, '&amp;')
        .replace(/</g, '&lt;')
        .replace(/>/g, '&gt;')
        .replace(/"/g, '&quot;')
        .replace(/'/g, '&#39;');
    }

    // 1. 挑選有活力朝氣的年輕女孩聲線
    function initVoices() {
      if ('speechSynthesis' in window) {
        const voices = window.speechSynthesis.getVoices();
        energeticGirlVoice = voices.find(v => 
          (v.lang === 'zh-TW' || v.lang.includes('TW')) && 
          (v.name.includes('HsiaoChen') || v.name.includes('Hsiao-Chen') || v.name.includes('Yating') || v.name.includes('Xiaoxiao') || v.name.includes('Female'))
        ) || voices.find(v => v.lang === 'zh-TW') || voices.find(v => v.lang.includes('zh'));
      }
    }
    if ('speechSynthesis' in window) {
      window.speechSynthesis.onvoiceschanged = initVoices;
      initVoices();
    }

    // 支援播報結束回呼的語音朗讀函式
    function speak(text, onEndCallback = null) {
      if ('speechSynthesis' in window) {
        window.speechSynthesis.cancel();
        const utter = new SpeechSynthesisUtterance(text);
        if (energeticGirlVoice) utter.voice = energeticGirlVoice;
        utter.lang = 'zh-TW';
        utter.rate = 1.08;
        utter.pitch = 1.32;
        if (onEndCallback) {
          utter.onend = onEndCallback;
          utter.onerror = onEndCallback;
        }
        window.speechSynthesis.speak(utter);
      } else if (onEndCallback) {
        onEndCallback();
      }
    }

    // 2. 初始化特大號數字鬧鐘選擇網格
    function buildBigClockGrid() {
      const hourGrid = document.getElementById('hour-grid');
      let hHtml = '';
      for (let h = 0; h < 24; h++) {
        const hh = String(h).padStart(2, '0');
        hHtml += `<button type="button" class="num-pick-btn" onclick="pickHour('${hh}')">${hh}點</button>`;
      }
      hourGrid.innerHTML = hHtml;

      const minGrid = document.getElementById('minute-grid');
      let mHtml = '';
      for (let m = 0; m < 60; m += 5) {
        const mm = String(m).padStart(2, '0');
        mHtml += `<button type="button" class="num-pick-btn" onclick="pickMinute('${mm}')">${mm}分</button>`;
      }
      minGrid.innerHTML = mHtml;
    }
    buildBigClockGrid();

    function openClockModal() {
      document.getElementById('clock-modal-title').innerText = '⏰ 請點選「幾點」';
      document.getElementById('hour-step-box').style.display = 'block';
      document.getElementById('minute-step-box').style.display = 'none';
      document.getElementById('clock-modal').style.display = 'flex';
    }

    function pickHour(hh) {
      selectedHour = hh;
      document.getElementById('clock-modal-title').innerText = `⏰ 已選 ${hh} 點，請點選「幾分」`;
      document.getElementById('hour-step-box').style.display = 'none';
      document.getElementById('minute-step-box').style.display = 'block';
    }

    function pickMinute(mm) {
      selectedMinute = mm;
      updateAlarmDisplay();
      closeClockModal();
      speak(`已設定 ${selectedHour} 點 ${selectedMinute} 分`);
    }

    function clearAlarmSetting() {
      selectedHour = '';
      selectedMinute = '00';
      updateAlarmDisplay();
      closeClockModal();
    }

    function closeClockModal() {
      document.getElementById('clock-modal').style.display = 'none';
    }

    function updateAlarmDisplay() {
      const el = document.getElementById('alarm-display');
      if (!selectedHour) {
        el.innerText = '不設鬧鐘 ▾';
        el.style.color = '#0284C7';
      } else {
        el.innerText = `${selectedHour}:${selectedMinute} ▾`;
        el.style.color = '#DC2626';
      }
    }

    // 改善建議彈窗控制
    function openFeedbackModal() {
      document.getElementById('feedback-modal').style.display = 'flex';
      speak("請輸入您對美茉莉銀髮備忘錄的改善建議。");
    }

    function closeFeedbackModal() {
      document.getElementById('feedback-modal').style.display = 'none';
    }

    function sendFeedbackViaLine() {
      const text = document.getElementById('feedback-input').value.trim();
      if (!text) {
        alert("請先輸入建議內容喔！");
        return;
      }
      const msg = encodeURIComponent(`【美茉莉銀髮備忘錄 - 改善建議】\n建議內容：${text}`);
      window.open(`https://line.me/R/msg/text/?${msg}`, '_blank', 'noopener,noreferrer');
      document.getElementById('feedback-input').value = '';
      closeFeedbackModal();
    }

    // 3. 自然語意時間解析器
    function parseChineseNumber(str) {
      if (!str) return NaN;
      if (/^\d+$/.test(str)) return parseInt(str, 10);
      const map = { '零':0, '〇':0, '一':1, '二':2, '兩':2, '三':3, '四':4, '五':5, '六':6, '七':7, '八':8, '九':9 };
      if (str === '十') return 10;
      if (str.length === 2 && str[0] === '十') return 10 + (map[str[1]] || 0);
      if (str.length === 2 && str[1] === '十') return (map[str[0]] || 0) * 10;
      if (str.length === 3 && str[1] === '十') return (map[str[0]] || 0) * 10 + (map[str[2]] || 0);
      return map[str] !== undefined ? map[str] : NaN;
    }

    function autoExtractTimeFromSpeech(text) {
      const regex = /(凌晨|早上|上午|中午|下午|傍晚|晚上|夜間)?\s*([0-9]{1,2}|[零〇一二兩三四五六七八九十]{1,3})\s*[點時]\s*(半|一刻|三刻|[0-9]{1,2}|[零〇一二兩三四五六七八九十]{1,3})?/;
      const match = text.match(regex);
      if (!match) return;

      const period = match[1] || '';
      let hour = parseChineseNumber(match[2]);
      const minStr = match[3] || '';

      if (isNaN(hour) || hour < 0 || hour > 24) return;

      if ((period === '下午' || period === '傍晚' || period === '晚上' || period === '夜間') && hour < 12) {
        hour += 12;
      } else if (period === '中午' && hour >= 1 && hour <= 5) {
        hour += 12;
      } else if ((period === '凌晨' || period === '早上' || period === '上午') && hour === 12) {
        hour = 0;
      }

      if (hour === 24) hour = 0;

      let minute = 0;
      if (minStr === '半') minute = 30;
      else if (minStr === '一刻') minute = 15;
      else if (minStr === '三刻') minute = 45;
      else if (minStr) {
        const parsedM = parseChineseNumber(minStr);
        if (!isNaN(parsedM) && parsedM >= 0 && parsedM < 60) {
          minute = parsedM;
        }
      }

      selectedHour = String(hour).padStart(2, '0');
      selectedMinute = String(minute).padStart(2, '0');
      updateAlarmDisplay();
    }

    // 4. 點選分類按鈕：先念出「XX提醒錄音開始」，提示音結束後自動啟動麥克風
    function selectAndRecord(cat, btn) {
      if (isRecording && currentCategory === cat) {
        stopRecord();
        return;
      }

      if (isRecording) {
        stopRecord();
      }

      currentCategory = cat;
      document.querySelectorAll('.memo-box').forEach(b => {
        b.classList.remove('active', 'recording-card');
      });
      btn.classList.add('active', 'recording-card');

      const input = document.getElementById('note-input');
      if (cat === '吃藥') input.placeholder = '例如：下午三點吃降血壓藥';
      else if (cat === '回診') input.placeholder = '例如：上午九點心臟科回診';
      else if (cat === '運動健身') input.placeholder = '例如：晚上七點公園散步';
      else if (cat === '一般日常') input.placeholder = '例如：下午四點買水果';

      const banner = document.getElementById('mic-banner');
      const tip = document.getElementById('mic-tip');
      banner.style.display = 'block';
      tip.innerText = `🔊 ${cat}提醒錄音開始...`;

      let micStarted = false;
      const launchMic = () => {
        if (micStarted) return;
        micStarted = true;
        startDirectRecord(cat, btn);
      };

      // 念出「吃藥提醒錄音開始」，念完瞬間開啟麥克風（加上 2.4 秒保險定時器防部分瀏覽器未觸發 onend）
      speak(`${cat}提醒錄音開始`, launchMic);
      setTimeout(launchMic, 2400);
    }

    function createRecognitionInstance() {
      const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
      if (!SpeechRecognition) return null;

      const rec = new SpeechRecognition();
      rec.continuous = false;
      rec.interimResults = false;
      rec.lang = 'zh-TW';

      rec.onresult = (e) => {
        if (e.results && e.results[0] && e.results[0][0]) {
          const newText = e.results[0][0].transcript.trim();
          if (newText) {
            const lastPhrase = savedPhrases[savedPhrases.length - 1] || '';
            if (newText !== lastPhrase && !lastPhrase.endsWith(newText)) {
              if (lastPhrase && newText.startsWith(lastPhrase)) {
                savedPhrases[savedPhrases.length - 1] = newText;
              } else {
                savedPhrases.push(newText);
              }
            }
            const fullText = savedPhrases.join('，');
            document.getElementById('note-input').value = fullText;
            autoExtractTimeFromSpeech(fullText);
          }
        }
      };

      rec.onend = () => {
        if (isRecording && countdownSec > 0) {
          try {
            rec.start();
          } catch (err) {
            cleanupRecordUI();
          }
        } else {
          cleanupRecordUI();
        }
      };

      rec.onerror = (e) => {
        if (e.error === 'no-speech' && isRecording && countdownSec > 0) {
          return;
        }
        cleanupRecordUI();
        if (e.error !== 'aborted') {
          document.getElementById('mic-banner').style.display = 'block';
          document.getElementById('mic-tip').innerText = '沒聽清楚，請再點一次上方按鈕重說。';
        }
      };

      return rec;
    }

    function startDirectRecord(cat, btn) {
      const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
      if (!SpeechRecognition) return;

      isRecording = true;
      savedPhrases = [];
      document.getElementById('note-input').value = '';
      btn.classList.add('recording-card');

      const banner = document.getElementById('mic-banner');
      const tip = document.getElementById('mic-tip');
      banner.style.display = 'block';

      countdownSec = 15;
      tip.innerText = `🔴 請說出內容與上下午時間（剩 ${countdownSec} 秒）`;

      clearInterval(recordTimer);
      recordTimer = setInterval(() => {
        countdownSec--;
        if (countdownSec > 0 && isRecording) {
          tip.innerText = `🔴 請說出內容與上下午時間（剩 ${countdownSec} 秒）`;
        } else {
          stopRecord();
        }
      }, 1000);

      recognition = createRecognitionInstance();
      try {
        recognition.start();
      } catch (e) {
        cleanupRecordUI();
      }
    }

    function stopRecord() {
      isRecording = false;
      if (recognition) {
        try { recognition.abort(); } catch (e) {}
      }
      cleanupRecordUI();
    }

    function cleanupRecordUI() {
      isRecording = false;
      clearInterval(recordTimer);
      document.querySelectorAll('.memo-box').forEach(b => b.classList.remove('recording-card'));
      document.getElementById('mic-banner').style.display = 'none';
    }

    // 5. 儲存與新增備忘錄
    function addNote() {
      if (isRecording) stopRecord();

      const input = document.getElementById('note-input');
      const text = input.value.trim();

      if (!text) {
        speak("請先點上方按鈕說話，或打字輸入內容喔！");
        return;
      }

      if (!selectedHour) {
        autoExtractTimeFromSpeech(text);
      }

      const alarmStr = selectedHour ? `${selectedHour}:${selectedMinute}` : '';
      const notes = getNotes();
      const newNote = {
        id: Date.now(),
        category: currentCategory,
        text: text,
        time: alarmStr,
        createdDate: new Date().toLocaleDateString('zh-TW', { month: 'numeric', day: 'numeric' })
      };

      notes.unshift(newNote);
      localStorage.setItem(STORAGE_KEY, JSON.stringify(notes));

      input.value = '';
      clearAlarmSetting();
      render();

      const timeConfirm = alarmStr ? `，並設定 ${alarmStr} 提醒` : '';
      const confirmMsg = `已經幫您記好啦！${newNote.category}，${newNote.text}${timeConfirm}`;
      speak(confirmMsg);
    }

    function getNotes() {
      return JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]');
    }

    function deleteNote(id) {
      const notes = getNotes().filter(n => n.id !== id);
      localStorage.setItem(STORAGE_KEY, JSON.stringify(notes));
      render();
    }

    // 6. 渲染備忘錄清單
    function render() {
      const listEl = document.getElementById('notes-list');
      const notes = getNotes();

      if (notes.length === 0) {
        listEl.innerHTML = '<div style="color:#64748B; text-align:center; margin:16px 0; font-size:clamp(24px, 6.8vw, 30px); font-weight:900;">目前沒有任何備忘事項</div>';
        return;
      }

      listEl.innerHTML = notes.map(n => {
        const safeText = escapeHtml(n.text);
        const safeCategory = escapeHtml(n.category);
        const safeTime = escapeHtml(n.time);
        const safeDate = escapeHtml(n.createdDate);

        const lineMsg = encodeURIComponent(`【美茉莉備忘提醒】\n事項：${n.category} - ${n.text}\n時間：${n.time ? n.time : n.createdDate}`);
        const lineUrl = `https://line.me/R/msg/text/?${lineMsg}`;

        return `
          <div class="note-card">
            <div style="flex:1; min-width:0;">
              <span class="cat-tag tag-${safeCategory}">${safeCategory}</span>
              <div class="note-text">${safeText}</div>
              <div class="note-alarm">${safeTime ? '⏰ 提醒：' + safeTime : '📅 記於：' + safeDate}</div>
            </div>
            <div class="card-actions">
              <button class="btn-speak" onclick="speak('${safeCategory}提醒：${safeText}')">🔊 念出</button>
              <a href="${lineUrl}" target="_blank" rel="noopener noreferrer" class="btn-line">📲 LINE兒女</a>
              <button class="btn-del" onclick="deleteNote(${n.id})">刪除</button>
            </div>
          </div>
        `;
      }).join('');
    }

    // 開啟與關閉有聲教學視窗
    function openGuideModal() {
      document.getElementById('guide-modal').style.display = 'flex';
      playFullVoiceGuide();
    }

    function closeGuideModal() {
      document.getElementById('guide-modal').style.display = 'none';
      if ('speechSynthesis' in window) {
        window.speechSynthesis.cancel();
      }
    }

    function playFullVoiceGuide() {
      const fullText = "歡迎使用美茉莉銀髮備忘錄！第一步，直接點選上方要記的事情，例如一般日常、回診、運動健身或吃藥，聽到提示音後就會馬上開始錄音！第二步，只要說出例如下午三點吃降血壓藥，系統就會自動幫您設定好鬧鐘！第三步，按藍色大按鈕記下來就完成囉！";
      speak(fullText);
    }

    // 7. 即時時鐘與鬧鐘
    function updateClock() {
      const now = new Date();
      const weekdays = ['星期日', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六'];
      const m = now.getMonth() + 1;
      const d = now.getDate();
      const day = weekdays[now.getDay()];

      const hh = String(now.getHours()).padStart(2, '0');
      const mm = String(now.getMinutes()).padStart(2, '0');
      const ss = String(now.getSeconds()).padStart(2, '0');

      document.getElementById('current-date').innerText = `${m}月${d}日 ${day}`;
      document.getElementById('current-time').innerText = `${hh}:${mm}:${ss}`;

      const currentHHMM = `${hh}:${mm}`;
      const notes = getNotes();
      notes.forEach(note => {
        if (note.time === currentHHMM) {
          const alarmKey = `${note.id}-${currentHHMM}`;
          if (!notifiedIds.has(alarmKey)) {
            notifiedIds.add(alarmKey);
            const msg = `時間到了！提醒您，${note.category}：${note.text}`;
            speak(msg);
            alert(`【美茉莉提醒您】\n\n類別：${note.category}\n內容：${note.text}`);
          }
        }
      });
    }
    setInterval(updateClock, 1000);
    updateClock();

    // 8. 即時天氣
    function fetchWeather(lat = 25.04, lon = 121.28) {
      fetch(`https://api.open-meteo.com/v1/forecast?latitude=${lat}&longitude=${lon}&current_weather=true`)
        .then(res => res.json())
        .then(data => {
          if (data && data.current_weather) {
            const temp = Math.round(data.current_weather.temperature);
            const code = data.current_weather.weathercode;
            const w = getWeatherText(code);
            document.getElementById('weather-temp').innerText = `${temp}°C`;
            document.getElementById('weather-desc').innerText = w.text;
            document.getElementById('weather-icon').innerText = w.icon;
          }
        })
        .catch(() => {
          document.getElementById('weather-desc').innerText = '多雲時晴';
          document.getElementById('weather-temp').innerText = '舒適';
        });
    }

    function getWeatherText(code) {
      if (code === 0) return { text: '晴天', icon: '☀️' };
      if ([1, 2, 3].includes(code)) return { text: '多雲晴', icon: '⛅' };
      if ([45, 48].includes(code)) return { text: '起霧', icon: '🌫️' };
      if ([51, 53, 55, 61, 63, 65, 80, 81, 82].includes(code)) return { text: '陰有雨', icon: '🌧️' };
      if ([95, 96, 99].includes(code)) return { text: '雷陣雨', icon: '⛈️' };
      return { text: '陰天', icon: '☁️' };
    }

    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(
        pos => fetchWeather(pos.coords.latitude, pos.coords.longitude),
        () => fetchWeather()
      );
    } else {
      fetchWeather();
    }

    render();
  </script>
</body>
</html>
