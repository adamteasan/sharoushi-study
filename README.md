<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>社労士試験 学習管理システム Pro</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Hiragino Sans', 'Hiragino Kaku Gothic ProN', 'Noto Sans JP', Arial, sans-serif;
    }
    
    body {
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      min-height: 100vh;
      padding: 20px;
    }
    
    .main-container {
      max-width: 1600px;
      margin: 0 auto;
    }
    
    .tabs {
      display: flex;
      gap: 10px;
      margin-bottom: 20px;
      background: rgba(255, 255, 255, 0.2);
      padding: 10px;
      border-radius: 15px;
      backdrop-filter: blur(10px);
    }
    
    .tab-btn {
      flex: 1;
      padding: 15px;
      background: rgba(255, 255, 255, 0.3);
      border: none;
      border-radius: 10px;
      color: white;
      font-size: 18px;
      font-weight: bold;
      cursor: pointer;
      transition: all 0.3s;
    }
    
    .tab-btn.active {
      background: white;
      color: #667eea;
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
      transform: translateY(-2px);
    }
    
    .tab-btn:hover:not(.active) {
      background: rgba(255, 255, 255, 0.4);
    }
    
    .tab-content {
      display: none;
      animation: fadeIn 0.3s;
    }
    
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(10px); }
      to { opacity: 1; transform: translateY(0); }
    }
    
    .tab-content.active {
      display: block;
    }
    
    /* タイマーセクション */
    .timer-container {
      max-width: 500px;
      margin: 0 auto;
      background: white;
      border-radius: 20px;
      padding: 30px;
      box-shadow: 0 20px 60px rgba(0,0,0,0.3);
    }
    
    h1 {
      text-align: center;
      color: #667eea;
      margin-bottom: 10px;
      font-size: 28px;
    }
    
    .date-display {
      text-align: center;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      color: white;
      padding: 12px;
      border-radius: 10px;
      margin-bottom: 25px;
      font-size: 16px;
      font-weight: bold;
    }
    
    .subject-select {
      margin-bottom: 25px;
    }
    
    label {
      display: block;
      margin-bottom: 10px;
      font-weight: bold;
      color: #555;
      font-size: 16px;
    }
    
    select, input[type="date"], input[type="text"] {
      width: 100%;
      padding: 14px;
      font-size: 17px;
      border: 2px solid #ddd;
      border-radius: 10px;
      background: white;
      cursor: pointer;
      transition: border-color 0.3s;
    }
    
    input[type="text"] {
      text-align: center;
      font-family: 'Courier New', monospace;
    }
    
    select:focus, input:focus {
      outline: none;
      border-color: #667eea;
    }
    
    .timer-display {
      text-align: center;
      font-size: 72px;
      font-weight: bold;
      color: #667eea;
      margin: 40px 0;
      font-family: 'Courier New', monospace;
      text-shadow: 2px 2px 4px rgba(0,0,0,0.1);
    }
    
    .controls {
      display: flex;
      gap: 12px;
      margin-bottom: 25px;
    }
    
    button {
      padding: 16px;
      font-size: 17px;
      font-weight: bold;
      border: none;
      border-radius: 12px;
      cursor: pointer;
      transition: all 0.3s;
      box-shadow: 0 4px 12px rgba(0,0,0,0.15);
    }
    
    .controls button {
      flex: 1;
    }
    
    .start-btn {
      background: #10b981;
      color: white;
    }
    
    .start-btn:hover:not(:disabled) {
      background: #059669;
      transform: translateY(-2px);
    }
    
    .start-btn:disabled {
      background: #d1d5db;
      cursor: not-allowed;
      transform: none;
    }
    
    .stop-btn {
      background: #ef4444;
      color: white;
    }
    
    .stop-btn:hover:not(:disabled) {
      background: #dc2626;
      transform: translateY(-2px);
    }
    
    .stop-btn:disabled {
      background: #d1d5db;
      cursor: not-allowed;
      transform: none;
    }
    
    .reset-btn {
      background: #6b7280;
      color: white;
    }
    
    .reset-btn:hover {
      background: #4b5563;
      transform: translateY(-2px);
    }
    
    .elapsed-info {
      text-align: center;
      color: #666;
      margin-bottom: 20px;
      font-size: 16px;
      min-height: 24px;
    }
    
    .record-btn {
      width: 100%;
      padding: 18px;
      background: #667eea;
      color: white;
      font-size: 19px;
      font-weight: bold;
    }
    
    .record-btn:hover:not(:disabled) {
      background: #5568d3;
      transform: translateY(-2px);
    }
    
    .record-btn:disabled {
      background: #d1d5db;
      cursor: not-allowed;
      transform: none;
    }
    
    .message {
      text-align: center;
      margin-top: 15px;
      padding: 12px;
      border-radius: 10px;
      font-weight: bold;
      display: none;
    }
    
    .message.success {
      background: #d1fae5;
      color: #065f46;
      display: block;
    }
    
    .message.error {
      background: #fee2e2;
      color: #991b1b;
      display: block;
    }
    
    .helper-text {
      font-size: 13px;
      color: #666;
      margin-top: 5px;
      text-align: center;
    }
    
    /* スプレッドシートセクション */
    .sheet-container {
      background: white;
      border-radius: 20px;
      padding: 30px;
      box-shadow: 0 20px 60px rgba(0,0,0,0.3);
    }
    
    .sheet-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 25px;
      flex-wrap: wrap;
      gap: 15px;
    }
    
    .total-display {
      background: linear-gradient(135deg, #203864 0%, #1e3a5f 100%);
      color: white;
      padding: 20px;
      border-radius: 12px;
      text-align: center;
      margin-bottom: 20px;
    }
    
    .total-label {
      font-size: 16px;
      opacity: 0.9;
      margin-bottom: 8px;
    }
    
    .total-time {
      font-size: 48px;
      font-weight: bold;
      font-family: 'Courier New', monospace;
    }
    
    .sheet-wrapper {
      overflow-x: auto;
      border-radius: 12px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.1);
      max-height: 70vh;
      overflow-y: auto;
    }
    
    .sheet-table {
      width: 100%;
      border-collapse: collapse;
      background: white;
      font-size: 14px;
    }
    
    .sheet-table thead {
      position: sticky;
      top: 0;
      z-index: 10;
    }
    
    .sheet-table th {
      background: #4472C4;
      color: white;
      padding: 12px 8px;
      text-align: center;
      font-weight: bold;
      font-size: 12px;
      border: 1px solid #365a9e;
      min-width: 80px;
    }
    
    .sheet-table th.subject-col {
      min-width: 90px;
      background: #70AD47;
      border: 1px solid #5a8c38;
    }
    
    .sheet-table th.total-col {
      min-width: 80px;
      background: #FFC000;
      border: 1px solid #d9a200;
    }
    
    .sheet-table td {
      padding: 8px;
      text-align: center;
      border: 1px solid #e0e0e0;
    }
    
    .sheet-table .date-cell {
      font-weight: bold;
    }
    
    .sheet-table .weekday-sat {
      color: #0000FF;
    }
    
    .sheet-table .weekday-sun {
      color: #FF0000;
    }
    
    .sheet-table .shift-early {
      background: #87CEEB;
    }
    
    .sheet-table .shift-rest {
      background: #F4B084;
    }
    
    .sheet-table .shift-late {
      background: #FFB6C1;
    }
    
    .sheet-table input {
      width: 100%;
      border: none;
      background: transparent;
      text-align: center;
      font-family: 'Courier New', monospace;
      padding: 4px;
      font-size: 14px;
    }
    
    .sheet-table input:focus {
      outline: 2px solid #667eea;
      background: #fff;
      border-radius: 4px;
    }
    
    /* 勉強時間がある場合、濃い背景色 */
    .sheet-table input.has-value {
      background: #2d5a9e !important;
      color: white;
      font-weight: bold;
    }
    
    .sheet-table .shift-early input.has-value {
      background: #1e90ff !important;
    }
    
    .sheet-table .shift-rest input.has-value {
      background: #d2691e !important;
    }
    
    .sheet-table .shift-late input.has-value {
      background: #c71585 !important;
    }
    
    .sheet-table .total-cell {
      background: #FFF2CC;
      font-weight: bold;
      font-family: 'Courier New', monospace;
    }
    
    .sheet-table .summary-row {
      background: #E7E6E6;
      font-weight: bold;
    }
    
    /* 記録ログセクション */
    .log-container {
      background: white;
      border-radius: 20px;
      padding: 30px;
      box-shadow: 0 20px 60px rgba(0,0,0,0.3);
    }
    
    .log-table-wrapper {
      overflow-x: auto;
      border-radius: 12px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.1);
      max-height: 70vh;
      overflow-y: auto;
    }
    
    .log-table {
      width: 100%;
      border-collapse: collapse;
      background: white;
    }
    
    .log-table thead {
      background: #667eea;
      color: white;
      position: sticky;
      top: 0;
      z-index: 5;
    }
    
    .log-table th {
      padding: 15px;
      text-align: left;
      font-weight: bold;
      font-size: 15px;
    }
    
    .log-table td {
      padding: 12px 15px;
      border-bottom: 1px solid #e5e7eb;
    }
    
    .log-table tbody tr:hover {
      background: #f9fafb;
    }
    
    .log-actions {
      display: flex;
      gap: 8px;
    }
    
    .edit-btn, .delete-btn {
      padding: 6px 12px;
      font-size: 13px;
      border-radius: 6px;
      cursor: pointer;
    }
    
    .edit-btn {
      background: #3b82f6;
      color: white;
    }
    
    .edit-btn:hover {
      background: #2563eb;
    }
    
    .delete-btn {
      background: #ef4444;
      color: white;
    }
    
    .delete-btn:hover {
      background: #dc2626;
    }
    
    /* モーダル */
    .modal {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.5);
      z-index: 1000;
      justify-content: center;
      align-items: center;
    }
    
    .modal.active {
      display: flex;
    }
    
    .modal-content {
      background: white;
      border-radius: 20px;
      padding: 30px;
      max-width: 500px;
      width: 90%;
    }
    
    .modal-title {
      font-size: 24px;
      font-weight: bold;
      color: #667eea;
      margin-bottom: 20px;
    }
    
    .modal-buttons {
      display: flex;
      gap: 10px;
      margin-top: 20px;
    }
    
    .modal-buttons button {
      flex: 1;
    }
    
    /* 統計分析セクション */
    .stats-container {
      background: white;
      border-radius: 20px;
      padding: 30px;
      box-shadow: 0 20px 60px rgba(0,0,0,0.3);
    }
    
    .stats-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 15px;
      margin-bottom: 30px;
    }
    
    .stat-card {
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      color: white;
      padding: 20px;
      border-radius: 12px;
      text-align: center;
      transition: transform 0.3s;
    }
    
    .stat-card:hover {
      transform: translateY(-5px);
    }
    
    .stat-label {
      font-size: 14px;
      opacity: 0.9;
      margin-bottom: 8px;
    }
    
    .stat-value {
      font-size: 32px;
      font-weight: bold;
      font-family: 'Courier New', monospace;
    }
    
    .goal-cards {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
      gap: 15px;
      margin-bottom: 30px;
    }
    
    .goal-card {
      background: white;
      border-radius: 12px;
      padding: 20px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.1);
      border-left: 5px solid #667eea;
    }
    
    .goal-title {
      font-size: 18px;
      font-weight: bold;
      color: #374151;
      margin-bottom: 15px;
    }
    
    .goal-stat {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 10px;
    }
    
    .goal-label {
      color: #6b7280;
      font-size: 14px;
    }
    
    .goal-value {
      font-size: 20px;
      font-weight: bold;
      color: #667eea;
      font-family: 'Courier New', monospace;
    }
    
    .status-badge {
      display: inline-block;
      padding: 4px 12px;
      border-radius: 20px;
      font-size: 12px;
      font-weight: bold;
      margin-top: 10px;
    }
    
    .status-ahead {
      background: #d1fae5;
      color: #065f46;
    }
    
    .status-behind {
      background: #fee2e2;
      color: #991b1b;
    }
    
    .status-ontrack {
      background: #dbeafe;
      color: #1e40af;
    }
    
    .chart-section {
      background: #f9fafb;
      border-radius: 12px;
      padding: 25px;
      margin-bottom: 25px;
    }
    
    .chart-title {
      font-size: 20px;
      font-weight: bold;
      color: #374151;
      margin-bottom: 20px;
      display: flex;
      align-items: center;
      gap: 10px;
    }
    
    .chart-canvas {
      width: 100%;
      height: 300px;
      background: white;
      border-radius: 8px;
      padding: 15px;
    }
    
    .subject-breakdown {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
      gap: 15px;
      margin-top: 20px;
    }
    
    .subject-item {
      background: white;
      border-radius: 8px;
      padding: 15px;
      border-left: 4px solid #667eea;
    }
    
    .subject-name {
      font-weight: bold;
      color: #374151;
      margin-bottom: 8px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    
    .subject-target {
      font-size: 12px;
      color: #6b7280;
      background: #f3f4f6;
      padding: 2px 8px;
      border-radius: 12px;
    }
    
    .subject-stats {
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    
    .subject-time {
      font-size: 24px;
      font-weight: bold;
      color: #667eea;
      font-family: 'Courier New', monospace;
    }
    
    .subject-percentage {
      font-size: 14px;
      color: #6b7280;
    }
    
    .progress-bar {
      width: 100%;
      height: 8px;
      background: #e5e7eb;
      border-radius: 4px;
      margin-top: 8px;
      overflow: hidden;
    }
    
    .progress-fill {
      height: 100%;
      background: linear-gradient(90deg, #667eea, #764ba2);
      border-radius: 4px;
      transition: width 0.5s ease;
    }
    
    .insights-section {
      background: linear-gradient(135deg, #f0f9ff 0%, #e0f2fe 100%);
      border-radius: 12px;
      padding: 25px;
      border-left: 5px solid #0ea5e9;
    }
    
    .insight-item {
      display: flex;
      align-items: flex-start;
      gap: 15px;
      margin-bottom: 15px;
      padding: 15px;
      background: white;
      border-radius: 8px;
    }
    
    .insight-icon {
      font-size: 24px;
      flex-shrink: 0;
    }
    
    .insight-content h3 {
      color: #0c4a6e;
      font-size: 16px;
      margin-bottom: 5px;
    }
    
    .insight-content p {
      color: #475569;
      font-size: 14px;
      line-height: 1.5;
    }
    
    .controls-row {
      display: flex;
      gap: 10px;
      margin-bottom: 20px;
      flex-wrap: wrap;
    }
    
    .export-btn, .clear-btn, .sync-btn {
      padding: 12px 20px;
      font-size: 15px;
      font-weight: bold;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      transition: all 0.3s;
    }
    
    .export-btn {
      background: #8b5cf6;
      color: white;
    }
    
    .export-btn:hover {
      background: #7c3aed;
      transform: translateY(-2px);
    }
    
    .sync-btn {
      background: #10b981;
      color: white;
    }
    
    .sync-btn:hover {
      background: #059669;
      transform: translateY(-2px);
    }
    
    .trend-indicator {
      display: inline-flex;
      align-items: center;
      gap: 5px;
      padding: 4px 12px;
      border-radius: 20px;
      font-size: 12px;
      font-weight: bold;
      margin-left: 10px;
    }
    
    .trend-up {
      background: #d1fae5;
      color: #065f46;
    }
    
    .trend-down {
      background: #fee2e2;
      color: #991b1b;
    }
    
    .trend-neutral {
      background: #e5e7eb;
      color: #374151;
    }
    
    @media (max-width: 768px) {
      .timer-display {
        font-size: 56px;
      }
      
      .stat-value {
        font-size: 24px;
      }
      
      .sheet-table {
        font-size: 12px;
      }
      
      .total-time {
        font-size: 36px;
      }
      
      .chart-canvas {
        height: 200px;
      }
    }

    
    /* 🎮 RPGアニメーション */
    @keyframes pulse {
      0%, 100% { transform: scale(1); }
      50% { transform: scale(1.05); }
    }
    
    @keyframes slideInRight {
      from {
        opacity: 0;
        transform: translateX(400px);
      }
      to {
        opacity: 1;
        transform: translateX(0);
      }
    }
    
    @keyframes slideOutRight {
      from {
        opacity: 1;
        transform: translateX(0);
      }
      to {
        opacity: 0;
        transform: translateX(400px);
      }
    }
    
    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }
    
    @keyframes scaleIn {
      from {
        opacity: 0;
        transform: scale(0.5);
      }
      to {
        opacity: 1;
        transform: scale(1);
      }
    }
    
    @keyframes spin {
      from { transform: rotate(0deg); }
      to { transform: rotate(360deg); }
    }
    
    #levelUpMessage {
      animation: pulse 1s infinite;
    }

    
    /* 装備タブ */
    .filter-btn {
      padding: 8px 16px;
      margin-right: 8px;
      background: #e5e7eb;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      font-size: 14px;
      font-weight: 600;
    }
    
    .filter-btn.active {
      background: #667eea;
      color: white;
    }
    
    .equipment-card {
      background: white;
      border: 2px solid #e5e7eb;
      border-radius: 12px;
      padding: 16px;
      cursor: pointer;
      transition: all 0.2s;
    }
    
    .equipment-card:hover {
      transform: translateY(-4px);
      box-shadow: 0 4px 12px rgba(0,0,0,0.1);
    }
    
    .equipment-card.Common {
      border-color: #64748b;
    }
    
    .equipment-card.Rare {
      border-color: #3b82f6;
    }
    
    .equipment-card.Epic {
      border-color: #a855f7;
    }
    
    .equipment-card.Legendary {
      border-color: #f59e0b;
      background: linear-gradient(135deg, #fffbeb 0%, #fef3c7 100%);
    }
  </style>
</head>
<body>
  <div class="main-container">
    <!-- タブ切り替え -->
    <div class="tabs">
      <button class="tab-btn active" onclick="switchTab('timer', this)">⏱️ タイマー</button>
      <button class="tab-btn" onclick="switchTab('sheet', this)">📋 スプレッドシート</button>
      <button class="tab-btn" onclick="switchTab('equipment', this)">🗡️ 装備</button>
      <button class="tab-btn" onclick="switchTab('skills', this)">🌳 スキル</button>
      <button class="tab-btn" onclick="switchTab('shop', this)">🎁 ショップ</button>
      <button class="tab-btn" onclick="switchTab('analytics', this)">📈 統計</button>
      <button class="tab-btn" onclick="switchTab('log', this)">📝 記録ログ</button>
      <button class="tab-btn" onclick="switchTab('stats', this)">📈 高度な分析</button>
    </div>
    
    <!-- タイマータブ -->
    <div id="timer-tab" class="tab-content active">
      <!-- 🎮 RPGステータス表示 -->
      <div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 24px; border-radius: 20px; margin-bottom: 30px; box-shadow: 0 10px 30px rgba(102, 126, 234, 0.3);">
        <div style="display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 20px; gap: 20px;">
          <div id="characterSprite" style="width: 80px; height: 80px; font-size: 64px;">🧒</div>
          <div style="flex: 1; color: white;">
            <div style="font-size: 28px; font-weight: 900; text-shadow: 0 2px 4px rgba(0,0,0,0.3); margin-bottom: 12px;">
              Lv.<span id="rpgLevel">1</span>
            </div>
            <div style="background: rgba(0,0,0,0.2); border-radius: 12px; padding: 4px; margin-bottom: 12px;">
              <div style="position: relative; background: rgba(255,255,255,0.15); border-radius: 10px; height: 28px; overflow: hidden;">
                <div id="rpgXpBar" style="width: 0%; height: 100%; background: linear-gradient(90deg, #10b981 0%, #059669 100%); border-radius: 10px; transition: width 0.5s ease;"></div>
                <div style="position: absolute; top: 0; left: 0; right: 0; bottom: 0; display: flex; align-items: center; justify-content: center;">
                  <span id="rpgXpText" style="color: white; font-weight: 800; font-size: 13px; text-shadow: 0 1px 3px rgba(0,0,0,0.5);">0 / 100 XP</span>
                </div>
              </div>
            </div>
            <div style="display: flex; gap: 16px; font-weight: 700; font-size: 16px; flex-wrap: wrap;">
              <div>💰 <span id="rpgCoins">0</span></div>
              <div>💎 <span id="rpgGems">0</span></div>
              <div>🔥 <span id="rpgStreak">0</span>日</div>
            </div>
            <div style="display: flex; gap: 16px; font-weight: 700; font-size: 14px; margin-top: 8px; opacity: 0.9; flex-wrap: wrap;">
              <div>⚔️ ATK: <span id="rpgAtk">100</span></div>
              <div>🛡️ DEF: <span id="rpgDef">5</span></div>
            </div>
          </div>
        </div>
        <div id="levelUpMessage" style="display: none; padding: 16px; background: linear-gradient(135deg, #fbbf24 0%, #f59e0b 100%); border-radius: 12px; text-align: center; color: white; font-weight: 800; font-size: 18px;">
          ✨ レベルアップ！✨
        </div>
        
        <!-- 集中力ゲージ -->
        <div style="margin-top: 16px; background: rgba(0,0,0,0.15); border-radius: 12px; padding: 16px;">
          <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 8px;">
            <span style="color: white; font-weight: 700; font-size: 14px;">❤️ 集中力</span>
            <span id="focusCount" style="color: white; font-weight: 800; font-size: 14px;">5/5</span>
          </div>
          <div style="display: flex; gap: 8px; margin-bottom: 8px;" id="focusHearts">
            <span style="font-size: 24px;">❤️</span>
            <span style="font-size: 24px;">❤️</span>
            <span style="font-size: 24px;">❤️</span>
            <span style="font-size: 24px;">❤️</span>
            <span style="font-size: 24px;">❤️</span>
          </div>
          <div id="focusInfo" style="font-size: 11px; color: rgba(255,255,255,0.7); line-height: 1.4;">
            集中力ボーナス: +50% 報酬<br>
            30分で1回復、日またぎで全回復
          </div>
        </div>
        
        <!-- デイリーガチャ -->
        <div style="margin-top: 16px; background: rgba(0,0,0,0.15); border-radius: 12px; padding: 16px;">
          <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px;">
            <span style="color: white; font-weight: 700; font-size: 14px;">🎲 デイリーガチャ</span>
            <span id="gachaAvailable" style="color: white; font-weight: 800; font-size: 14px;">1回</span>
          </div>
          <button id="gachaBtn" onclick="rollDailyGacha()" style="width: 100%; padding: 12px; background: linear-gradient(135deg, #f59e0b 0%, #d97706 100%); color: white; border: none; border-radius: 10px; font-weight: 700; font-size: 16px; cursor: pointer;">
            🎁 ガチャを回す (100コイン)
          </button>
          <div id="gachaTimer" style="margin-top: 8px; font-size: 12px; color: rgba(255,255,255,0.8); text-align: center;"></div>
        </div>
        
        <!-- デイリールーレット -->
        <div style="margin-top: 16px; background: rgba(0,0,0,0.15); border-radius: 12px; padding: 16px;">
          <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px;">
            <span style="color: white; font-weight: 700; font-size: 14px;">🎰 デイリールーレット</span>
            <span id="rouletteStatus" style="color: white; font-weight: 800; font-size: 14px;">利用可能</span>
          </div>
          <button id="rouletteBtn" onclick="rollDailyRoulette()" style="width: 100%; padding: 12px; background: linear-gradient(135deg, #f59e0b 0%, #d97706 100%); color: white; border: none; border-radius: 10px; font-weight: 700; font-size: 16px; cursor: pointer;">
            🎰 ルーレットを回す（無料）
          </button>
          <div id="rouletteTimer" style="margin-top: 8px; font-size: 12px; color: rgba(255,255,255,0.8); text-align: center;"></div>
        </div>
        
        <!-- 📦 宝箱 -->
        <div style="margin-top: 16px; background: rgba(0,0,0,0.15); border-radius: 12px; padding: 16px;">
          <div style="color: white; font-weight: 700; font-size: 14px; margin-bottom: 12px;">📦 宝箱</div>
          <div id="chestContainer"></div>
        </div>
        
        <!-- 🎯 デイリークエスト -->
        <div style="margin-top: 16px; background: rgba(0,0,0,0.15); border-radius: 12px; padding: 16px;">
          <div style="color: white; font-weight: 700; font-size: 14px; margin-bottom: 12px;">🎯 デイリークエスト</div>
          <div id="questContainer"></div>
        </div>
        
        <!-- 👾 ボス挑戦 -->
        <div style="margin-top: 16px; background: rgba(0,0,0,0.15); border-radius: 12px; padding: 16px;">
          <div style="color: white; font-weight: 700; font-size: 14px; margin-bottom: 12px;">👾 ボス挑戦</div>
          <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 8px;">
            <button onclick="fightWeeklyBoss()" style="padding: 12px; background: linear-gradient(135deg, #f59e0b, #d97706); color: white; border: none; border-radius: 10px; font-weight: 700; cursor: pointer; font-size: 14px;">
              ⚔️ 週ボス
            </button>
            <button onclick="fightMonthlyBoss()" style="padding: 12px; background: linear-gradient(135deg, #a855f7, #9333ea); color: white; border: none; border-radius: 10px; font-weight: 700; cursor: pointer; font-size: 14px;">
              👑 月ボス
            </button>
          </div>
        </div>
      </div>

      <div class="timer-container">
        <h1>⏱️ 学習タイマー</h1>
        
        <div class="date-display" id="dateDisplay">📅 読み込み中...</div>
        
        <div class="subject-select">
          <label for="recordDate">記録する日付</label>
          <input type="date" id="recordDate">
        </div>
        
        <div class="subject-select">
          <label for="subject">科目を選択</label>
          <select id="subject">
            <option value="">科目を選択してください</option>
            <option value="労働基準法">労働基準法</option>
            <option value="労働安全衛生法">労働安全衛生法</option>
            <option value="労働者災害補償保険法">労働者災害補償保険法</option>
            <option value="雇用保険法">雇用保険法</option>
            <option value="労働保険徴収法">労働保険徴収法</option>
            <option value="健康保険法">健康保険法</option>
            <option value="国民年金法">国民年金法</option>
            <option value="厚生年金保険法">厚生年金保険法</option>
            <option value="社会保険に関する一般常識">社会保険に関する一般常識</option>
            <option value="労働に関する一般常識">労働に関する一般常識</option>
            <option value="その他">その他</option>
          </select>
        </div>
        
        <div class="timer-display" id="timer">00:00:00</div>
        
        <div class="subject-select">
          <label for="manualTime">または時間を直接入力</label>
          <input type="text" id="manualTime" placeholder="例: 1:30 (1時間30分)">
          <p class="helper-text">形式: 時:分 (例: 2:15 = 2時間15分)</p>
        </div>
        
        <div class="elapsed-info" id="elapsedInfo"></div>
        
        <div class="controls">
          <button class="start-btn" id="startBtn" onclick="startTimer()">開始</button>
          <button class="stop-btn" id="stopBtn" onclick="stopTimer()" disabled>停止</button>
          <button class="reset-btn" id="resetBtn" onclick="resetTimer()">リセット</button>
        </div>
        
        <button class="record-btn" id="recordBtn" onclick="recordTime()">📝 記録する</button>
        <div class="message" id="message"></div>
      </div>
    </div>
    
    <!-- スプレッドシートタブ -->
    <div id="sheet-tab" class="tab-content">
      <div class="sheet-container">
        <div class="sheet-header">
          <h1>📋 学習記録シート</h1>
          <div class="controls-row">
            <button class="sync-btn" onclick="syncFromTimer()">🔄 タイマーから同期</button>
            <button class="export-btn" onclick="exportSheet()">💾 横型CSV</button>
            <button class="export-btn" onclick="exportOriginalFormat()">📊 縦型CSV</button>
          </div>
        </div>
        
        <div class="total-display">
          <div class="total-label">総学習時間</div>
          <div class="total-time" id="sheetTotalTime">00:00</div>
        </div>
        
        <div class="sheet-wrapper">
          <table class="sheet-table" id="sheetTable">
            <thead>
              <tr id="sheetHeader">
                <th class="date-col">日付</th>
                <th class="weekday-col">曜日</th>
                <th class="remaining-col">残り日数</th>
                <th class="subject-col">労働基準法</th>
                <th class="subject-col">労働安全衛生法</th>
                <th class="subject-col">労災保険法</th>
                <th class="subject-col">雇用保険法</th>
                <th class="subject-col">労働保険徴収法</th>
                <th class="subject-col">健康保険法</th>
                <th class="subject-col">国民年金法</th>
                <th class="subject-col">厚生年金保険法</th>
                <th class="subject-col">社保一般常識</th>
                <th class="subject-col">労働一般常識</th>
                <th class="subject-col">その他</th>
                <th class="total-col">当日合計</th>
              </tr>
              <tr class="summary-row">
                <td colspan="3">科目別合計</td>
                <td id="sum-0">00:00</td>
                <td id="sum-1">00:00</td>
                <td id="sum-2">00:00</td>
                <td id="sum-3">00:00</td>
                <td id="sum-4">00:00</td>
                <td id="sum-5">00:00</td>
                <td id="sum-6">00:00</td>
                <td id="sum-7">00:00</td>
                <td id="sum-8">00:00</td>
                <td id="sum-9">00:00</td>
                <td id="sum-10">00:00</td>
                <td id="sum-total" class="total-cell">00:00</td>
              </tr>
            </thead>
            <tbody id="sheetBody">
            </tbody>
          </table>
        </div>
      </div>
    </div>
    
    <!-- 記録ログタブ -->
    <div id="log-tab" class="tab-content">
      <div class="log-container">
        <h1>📝 記録ログ</h1>
        
        <div class="log-table-wrapper">
          <table class="log-table">
            <thead>
              <tr>
                <th>日付</th>
                <th>記録時刻</th>
                <th>科目</th>
                <th>学習時間</th>
                <th>操作</th>
              </tr>
            </thead>
            <tbody id="logTableBody">
            </tbody>
          </table>
        </div>
      </div>
    </div>
    
    <!-- 高度な分析タブ -->
    <div id="stats-tab" class="tab-content">
      <div class="stats-container">
        <h1>📊 高度な学習分析</h1>
        
        <!-- 主要統計カード -->
        <div class="stats-grid">
          <div class="stat-card">
            <div class="stat-label">総学習時間</div>
            <div class="stat-value" id="totalAllTime">00:00</div>
          </div>
          <div class="stat-card">
            <div class="stat-label">今週の学習時間</div>
            <div class="stat-value" id="weekTime">00:00</div>
          </div>
          <div class="stat-card">
            <div class="stat-label">今月の学習時間</div>
            <div class="stat-value" id="monthTime">00:00</div>
          </div>
          <div class="stat-card">
            <div class="stat-label">学習日数</div>
            <div class="stat-value"><span id="studyDays">0</span>日</div>
          </div>
          <div class="stat-card">
            <div class="stat-label">早番平均</div>
            <div class="stat-value" id="earlyAvg">00:00</div>
          </div>
          <div class="stat-card">
            <div class="stat-label">遅番平均</div>
            <div class="stat-value" id="lateAvg">00:00</div>
          </div>
          <div class="stat-card">
            <div class="stat-label">休日平均</div>
            <div class="stat-value" id="restAvg">00:00</div>
          </div>
          <div class="stat-card">
            <div class="stat-label">試験まで</div>
            <div class="stat-value"><span id="daysToExam">0</span>日</div>
          </div>
        </div>
        
        <!-- 目標達成カード -->
        <div class="goal-cards">
          <div class="goal-card">
            <div class="goal-title">🎯 800時間目標</div>
            <div class="goal-stat">
              <span class="goal-label">達成率</span>
              <span class="goal-value" id="goal800Progress">0%</span>
            </div>
            <div style="margin: 15px 0; padding: 15px; background: #f9fafb; border-radius: 8px;">
              <div style="font-size: 13px; font-weight: bold; color: #374151; margin-bottom: 10px;">必要ペース</div>
              <div style="display: grid; gap: 8px;">
                <div style="display: flex; justify-content: space-between; font-size: 13px;">
                  <span style="color: #6b7280;">早番:</span>
                  <span id="goal800EarlyPace" style="font-family: 'Courier New', monospace; font-weight: bold; color: #667eea;">00:00</span>
                </div>
                <div style="display: flex; justify-content: space-between; font-size: 13px;">
                  <span style="color: #6b7280;">遅番:</span>
                  <span id="goal800LatePace" style="font-family: 'Courier New', monospace; font-weight: bold; color: #667eea;">00:00</span>
                </div>
                <div style="display: flex; justify-content: space-between; font-size: 13px;">
                  <span style="color: #6b7280;">休日:</span>
                  <span id="goal800RestPace" style="font-family: 'Courier New', monospace; font-weight: bold; color: #667eea;">00:00</span>
                </div>
              </div>
            </div>
            <div id="goal800Status"></div>
          </div>
          
          <div class="goal-card">
            <div class="goal-title">🎯 1000時間目標</div>
            <div class="goal-stat">
              <span class="goal-label">達成率</span>
              <span class="goal-value" id="goal1000Progress">0%</span>
            </div>
            <div style="margin: 15px 0; padding: 15px; background: #f9fafb; border-radius: 8px;">
              <div style="font-size: 13px; font-weight: bold; color: #374151; margin-bottom: 10px;">必要ペース</div>
              <div style="display: grid; gap: 8px;">
                <div style="display: flex; justify-content: space-between; font-size: 13px;">
                  <span style="color: #6b7280;">早番:</span>
                  <span id="goal1000EarlyPace" style="font-family: 'Courier New', monospace; font-weight: bold; color: #667eea;">00:00</span>
                </div>
                <div style="display: flex; justify-content: space-between; font-size: 13px;">
                  <span style="color: #6b7280;">遅番:</span>
                  <span id="goal1000LatePace" style="font-family: 'Courier New', monospace; font-weight: bold; color: #667eea;">00:00</span>
                </div>
                <div style="display: flex; justify-content: space-between; font-size: 13px;">
                  <span style="color: #6b7280;">休日:</span>
                  <span id="goal1000RestPace" style="font-family: 'Courier New', monospace; font-weight: bold; color: #667eea;">00:00</span>
                </div>
              </div>
            </div>
            <div id="goal1000Status"></div>
          </div>
          
          <div class="goal-card">
            <div class="goal-title">🎯 1200時間目標</div>
            <div class="goal-stat">
              <span class="goal-label">達成率</span>
              <span class="goal-value" id="goal1200Progress">0%</span>
            </div>
            <div style="margin: 15px 0; padding: 15px; background: #f9fafb; border-radius: 8px;">
              <div style="font-size: 13px; font-weight: bold; color: #374151; margin-bottom: 10px;">必要ペース</div>
              <div style="display: grid; gap: 8px;">
                <div style="display: flex; justify-content: space-between; font-size: 13px;">
                  <span style="color: #6b7280;">早番:</span>
                  <span id="goal1200EarlyPace" style="font-family: 'Courier New', monospace; font-weight: bold; color: #667eea;">00:00</span>
                </div>
                <div style="display: flex; justify-content: space-between; font-size: 13px;">
                  <span style="color: #6b7280;">遅番:</span>
                  <span id="goal1200LatePace" style="font-family: 'Courier New', monospace; font-weight: bold; color: #667eea;">00:00</span>
                </div>
                <div style="display: flex; justify-content: space-between; font-size: 13px;">
                  <span style="color: #6b7280;">休日:</span>
                  <span id="goal1200RestPace" style="font-family: 'Courier New', monospace; font-weight: bold; color: #667eea;">00:00</span>
                </div>
              </div>
            </div>
            <div id="goal1200Status"></div>
          </div>
        </div>
        
        <!-- 科目別内訳 -->
        <div class="chart-section">
          <div class="chart-title">
            <span>📚</span>
            <span>科目別学習時間</span>
          </div>
          <div id="subjectBreakdown" class="subject-breakdown"></div>
        </div>
        
        <!-- 学習トレンド -->
        <div class="chart-section">
          <div class="chart-title">
            <span>📈</span>
            <span>学習トレンド (直近30日)</span>
            <span id="trendIndicator"></span>
          </div>
          <div class="chart-canvas">
            <canvas id="trendChart"></canvas>
          </div>
        </div>
        
        <!-- 曜日別・科目別分析 -->
        <div class="chart-section">
          <div class="chart-title">
            <span>📅</span>
            <span>シフト別・科目別学習パターン</span>
          </div>
          <div style="display: flex; gap: 10px; margin-bottom: 20px; justify-content: center;">
            <button class="filter-btn" id="shiftBtn1Week" onclick="updateShiftChartPeriod('week')" style="padding: 8px 16px; background: #6366f1; color: white; border: none; border-radius: 8px; font-weight: 700; cursor: pointer;">直近1週間</button>
            <button class="filter-btn" id="shiftBtn1Month" onclick="updateShiftChartPeriod('month')" style="padding: 8px 16px; background: #d1d5db; color: #374151; border: none; border-radius: 8px; font-weight: 700; cursor: pointer;">直近1ヶ月</button>
            <button class="filter-btn active" id="shiftBtnAll" onclick="updateShiftChartPeriod('all')" style="padding: 8px 16px; background: #d1d5db; color: #374151; border: none; border-radius: 8px; font-weight: 700; cursor: pointer;">全期間</button>
          </div>
          <div class="chart-canvas">
            <canvas id="shiftChart"></canvas>
          </div>
        </div>
        
        <!-- インサイト -->
        <div class="insights-section">
          <div class="chart-title">
            <span>💡</span>
            <span>学習インサイト</span>
          </div>
          <div id="insights"></div>
        </div>
      </div>
    </div>

    <!-- 装備タブ -->
    <div id="equipment-tab" class="tab-content">
      <div class="stats-container">
        <h1>🗡️ 装備管理</h1>
        
        <!-- 現在の装備 -->
        <div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 16px; margin-bottom: 20px; color: white;">
          <h3 style="margin-top: 0;">⚔️ 装備中</h3>
          
          <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 16px; margin-top: 16px;">
            <!-- 武器スロット -->
            <div style="background: rgba(255,255,255,0.1); padding: 16px; border-radius: 12px;">
              <div style="font-size: 14px; opacity: 0.8; margin-bottom: 8px;">🗡️ 武器</div>
              <div id="equippedWeapon" style="font-size: 16px; font-weight: 700; min-height: 40px;">なし</div>
              <button onclick="unequipItem('weapon')" style="margin-top: 8px; padding: 8px 16px; background: rgba(255,255,255,0.2); color: white; border: none; border-radius: 8px; cursor: pointer; font-size: 12px;">外す</button>
            </div>
            
            <!-- 防具スロット -->
            <div style="background: rgba(255,255,255,0.1); padding: 16px; border-radius: 12px;">
              <div style="font-size: 14px; opacity: 0.8; margin-bottom: 8px;">🛡️ 防具</div>
              <div id="equippedArmor" style="font-size: 16px; font-weight: 700; min-height: 40px;">なし</div>
              <button onclick="unequipItem('armor')" style="margin-top: 8px; padding: 8px 16px; background: rgba(255,255,255,0.2); color: white; border: none; border-radius: 8px; cursor: pointer; font-size: 12px;">外す</button>
            </div>
            
            <!-- アクセサリースロット -->
            <div style="background: rgba(255,255,255,0.1); padding: 16px; border-radius: 12px;">
              <div style="font-size: 14px; opacity: 0.8; margin-bottom: 8px;">💍 アクセ</div>
              <div id="equippedAccessory" style="font-size: 16px; font-weight: 700; min-height: 40px;">なし</div>
              <button onclick="unequipItem('accessory')" style="margin-top: 8px; padding: 8px 16px; background: rgba(255,255,255,0.2); color: white; border: none; border-radius: 8px; cursor: pointer; font-size: 12px;">外す</button>
            </div>
          </div>
        </div>
        
        <!-- インベントリ -->
        <div>
          <h3>🎒 インベントリ</h3>
          <div style="margin-bottom: 12px;">
            <button onclick="filterInventory('all')" class="filter-btn active" id="filter-all">全て</button>
            <button onclick="filterInventory('weapon')" class="filter-btn" id="filter-weapon">🗡️ 武器</button>
            <button onclick="filterInventory('armor')" class="filter-btn" id="filter-armor">🛡️ 防具</button>
            <button onclick="filterInventory('accessory')" class="filter-btn" id="filter-accessory">💍 アクセ</button>
          </div>
          <div id="inventoryList" style="display: grid; grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); gap: 12px;">
            <!-- JavaScriptで動的生成 -->
          </div>
        </div>
      </div>
    </div>

    <!-- スキルツリータブ -->
    <div id="skills-tab" class="tab-content">
      <div class="stats-container">
        <h1>🌳 スキルツリー</h1>
        
        <div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 16px; margin-bottom: 20px; color: white; text-align: center;">
          <div style="font-size: 18px; font-weight: 700; margin-bottom: 8px;">利用可能なスキルポイント</div>
          <div style="font-size: 48px; font-weight: 900;" id="skillPointsDisplay">0</div>
          <div style="font-size: 12px; opacity: 0.8; margin-top: 8px;">レベルアップで1SP獲得</div>
          <button onclick="resetSkills()" style="margin-top: 12px; padding: 8px 20px; background: rgba(255,255,255,0.2); color: white; border: none; border-radius: 8px; cursor: pointer; font-weight: 700;">
            🔄 リセット (50ジェム)
          </button>
        </div>
        
        <!-- 攻撃系スキル -->
        <div style="margin-bottom: 20px;">
          <h3 style="color: #ef4444; margin-bottom: 12px;">⚔️ 攻撃系スキル</h3>
          <div id="attackSkills" style="display: grid; grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); gap: 12px;">
            <!-- JavaScriptで動的生成 -->
          </div>
        </div>
        
        <!-- 防御系スキル -->
        <div style="margin-bottom: 20px;">
          <h3 style="color: #3b82f6; margin-bottom: 12px;">🛡️ 防御系スキル</h3>
          <div id="defenseSkills" style="display: grid; grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); gap: 12px;">
            <!-- JavaScriptで動的生成 -->
          </div>
        </div>
        
        <!-- サポート系スキル -->
        <div style="margin-bottom: 20px;">
          <h3 style="color: #10b981; margin-bottom: 12px;">✨ サポート系スキル</h3>
          <div id="supportSkills" style="display: grid; grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); gap: 12px;">
            <!-- JavaScriptで動的生成 -->
          </div>
        </div>
      </div>
    </div>

    <!-- ショップタブ -->
    <div id="shop-tab" class="tab-content">
      <div class="stats-container">
        <h1>🎁 ショップ</h1>
        
        <div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 16px; margin-bottom: 20px; color: white; text-align: center;">
          <div style="display: flex; justify-content: center; gap: 40px;">
            <div>
              <div style="font-size: 14px; opacity: 0.9;">所持コイン</div>
              <div style="font-size: 32px; font-weight: 900;">💰 <span id="shopCoins">0</span></div>
            </div>
            <div>
              <div style="font-size: 14px; opacity: 0.9;">所持ジェム</div>
              <div style="font-size: 32px; font-weight: 900;">💎 <span id="shopGems">0</span></div>
            </div>
          </div>
        </div>
        
        <div id="shopContainer">
          <!-- JavaScriptで動的生成 -->
        </div>
      </div>
    </div>

    <!-- 統計タブ -->
    <div id="analytics-tab" class="tab-content">
      <div class="stats-container">
        <h1>📈 学習統計</h1>
        
        <div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 16px; margin-bottom: 20px; color: white;">
          <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(150px, 1fr)); gap: 16px; text-align: center;">
            <div>
              <div style="font-size: 14px; opacity: 0.9;">総学習時間</div>
              <div style="font-size: 28px; font-weight: 900;" id="totalStudyTime">0時間</div>
            </div>
            <div>
              <div style="font-size: 14px; opacity: 0.9;">最長ストリーク</div>
              <div style="font-size: 28px; font-weight: 900;" id="maxStreak">0日</div>
            </div>
            <div>
              <div style="font-size: 14px; opacity: 0.9;">記録した日数</div>
              <div style="font-size: 28px; font-weight: 900;" id="studyDays">0日</div>
            </div>
            <div>
              <div style="font-size: 14px; opacity: 0.9;">平均時間/日</div>
              <div style="font-size: 28px; font-weight: 900;" id="avgTimePerDay">0分</div>
            </div>
          </div>
        </div>
        
        <div style="background: white; padding: 20px; border-radius: 16px; margin-bottom: 20px;">
          <h3 style="margin-bottom: 16px;">📊 週間学習時間</h3>
          <div id="weeklyChart" style="height: 200px;">
            <canvas id="weeklyCanvas" width="600" height="200"></canvas>
          </div>
        </div>
        
        <div style="background: white; padding: 20px; border-radius: 16px; margin-bottom: 20px;">
          <h3 style="margin-bottom: 16px;">🥧 科目別学習時間</h3>
          <div id="subjectChart" style="height: 300px; display: flex; justify-content: center; align-items: center;">
            <canvas id="subjectCanvas" width="300" height="300"></canvas>
          </div>
        </div>
      </div>
    </div>
  </div>
  
  <!-- 編集モーダル -->
  <div id="editModal" class="modal">
    <div class="modal-content">
      <div class="modal-title">記録を編集</div>
      <div class="subject-select">
        <label for="editDate">日付</label>
        <input type="date" id="editDate">
      </div>
      <div class="subject-select">
        <label for="editSubject">科目</label>
        <select id="editSubject">
          <option value="労働基準法">労働基準法</option>
          <option value="労働安全衛生法">労働安全衛生法</option>
          <option value="労働者災害補償保険法">労働者災害補償保険法</option>
          <option value="雇用保険法">雇用保険法</option>
          <option value="労働保険徴収法">労働保険徴収法</option>
          <option value="健康保険法">健康保険法</option>
          <option value="国民年金法">国民年金法</option>
          <option value="厚生年金保険法">厚生年金保険法</option>
          <option value="社会保険に関する一般常識">社会保険に関する一般常識</option>
          <option value="労働に関する一般常識">労働に関する一般常識</option>
          <option value="その他">その他</option>
        </select>
      </div>
      <div class="subject-select">
        <label for="editTime">学習時間</label>
        <input type="text" id="editTime" placeholder="例: 1:30">
      </div>
      <div class="modal-buttons">
        <button class="start-btn" onclick="saveEdit()">保存</button>
        <button class="reset-btn" onclick="closeEditModal()">キャンセル</button>
      </div>
    </div>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
  <script>

    // 🎮 日付処理関数（UTC罠回避）
    function toDateKeyLocal(date = new Date()) {
      const y = date.getFullYear();
      const m = String(date.getMonth() + 1).padStart(2, '0');
      const d = String(date.getDate()).padStart(2, '0');
      return `${y}-${m}-${d}`;
    }
    
    function fromDateKeyLocal(dateKey) {
      return new Date(`${dateKey}T00:00:00`);
    }
    
    // グローバル変数
    let startTime = null;
    let elapsedTime = 0;
    let timerInterval = null;
    let isPaused = false;
    let pausedTime = 0;
    let studyRecords = [];

    // 🎮 RPGデータ
    let playerData = {
      level: 1,
      xp: 0,
      totalXP: 0,
      coins: 0,
      gems: 0,
      streak: 0,
      lastStudyDate: null,
      streakProtection: 0,
      defeatedBosses: [],
      achievements: [],
      lastGachaDate: null,
      inventory: [],
      
      // v1.0 新要素
      atk: 100,
      def: 5,
      focus: 5,
      lastFocusUpdate: null,
      lastLoginDate: null,
      lastRouletteDate: null,
      totalStudyMinutes: 0,
      
      // v2.0: 装備システム
      equippedWeapon: null,
      equippedArmor: null,
      equippedAccessory: null,
      
      // v3.0: 新機能
      skillPoints: 0,
      learnedSkills: [],
      chests: [],
      defeatedEnemies: [],
      weekBossDefeatedDate: null,
      monthBossDefeatedDate: null,
      dailyQuests: [],
      lastQuestDate: null,
      
      // v3.5: ショップシステム
      shopRefreshDate: null,
      purchasedItems: []
    };
    
    // 科目ボスデータ

    // 🗡️ v2.0: 装備データベース
    const equipmentDatabase = {
      weapons: [
        { id: 'w001', name: '木の剣', rarity: 'Common', atk: 10, def: 0, effects: {} },
        { id: 'w002', name: '鉄の剣', rarity: 'Common', atk: 20, def: 0, effects: {} },
        { id: 'w003', name: '鋼の剣', rarity: 'Rare', atk: 40, def: 0, effects: {} },
        { id: 'w004', name: 'ミスリル剣', rarity: 'Rare', atk: 60, def: 0, effects: {} },
        { id: 'w005', name: 'ドラゴンスレイヤー', rarity: 'Epic', atk: 100, def: 0, effects: {} },
        { id: 'w006', name: '魔法剣', rarity: 'Epic', atk: 120, def: 0, effects: {} },
        { id: 'w007', name: 'エクスカリバー', rarity: 'Legendary', atk: 200, def: 0, effects: {} },
        { id: 'w008', name: '神剣グラム', rarity: 'Legendary', atk: 300, def: 0, effects: {} }
      ],
      
      armors: [
        { id: 'a001', name: '革の鎧', rarity: 'Common', atk: 0, def: 20, effects: {} },
        { id: 'a002', name: '鋼の鎧', rarity: 'Rare', atk: 10, def: 50, effects: {} },
        { id: 'a003', name: 'プラチナアーマー', rarity: 'Epic', atk: 30, def: 120, effects: {} },
        { id: 'a004', name: 'ドラゴンアーマー', rarity: 'Legendary', atk: 60, def: 250, effects: {} },
        { id: 'a005', name: '神の鎧', rarity: 'Legendary', atk: 100, def: 500, effects: {} }
      ],
      
      accessories: [
        { id: 'c001', name: '木の鉛筆', rarity: 'Common', atk: 5, def: 0, effects: {} },
        { id: 'c002', name: '幸運のペン', rarity: 'Rare', atk: 15, def: 0, effects: {} },
        { id: 'c003', name: '集中のメガネ', rarity: 'Rare', atk: 0, def: 0, effects: { xpBonus: 0.10 } },
        { id: 'c004', name: '時空の腕時計', rarity: 'Epic', atk: 30, def: 0, effects: { xpBonus: 0.15, coinBonus: 0.15 } },
        { id: 'c005', name: '知恵のお守り', rarity: 'Epic', atk: 40, def: 0, effects: { xpBonus: 0.20 } },
        { id: 'c006', name: 'エターナルリング', rarity: 'Legendary', atk: 80, def: 0, effects: { xpBonus: 0.30 } },
        { id: 'c007', name: '神の指輪', rarity: 'Legendary', atk: 150, def: 200, effects: { xpBonus: 0.50 } }
      ]
    };
    
    function getEnhancementBonus(level) {
      return 1 + ((level || 0) * 0.1);
    }
    

    
    // 🌳 v3.0: スキルツリー
    const skillDatabase = {
      attack: [
        { id: 'atk1', name: 'パワーアップI', cost: 1, maxLevel: 5, effect: { atk: 10 }, requires: [] },
        { id: 'atk2', name: 'パワーアップII', cost: 2, maxLevel: 5, effect: { atk: 20 }, requires: ['atk1'] },
        { id: 'atk3', name: 'パワーアップIII', cost: 3, maxLevel: 5, effect: { atk: 40 }, requires: ['atk2'] },
        { id: 'crit1', name: 'クリティカルI', cost: 2, maxLevel: 3, effect: { critRate: 0.05 }, requires: ['atk1'] }
      ],
      defense: [
        { id: 'def1', name: 'タフネスI', cost: 1, maxLevel: 5, effect: { def: 20 }, requires: [] },
        { id: 'def2', name: 'タフネスII', cost: 2, maxLevel: 5, effect: { def: 40 }, requires: ['def1'] },
        { id: 'regen', name: '集中力回復', cost: 3, maxLevel: 3, effect: { focusRegen: 0.1 }, requires: ['def1'] }
      ],
      support: [
        { id: 'xp1', name: 'XPブーストI', cost: 1, maxLevel: 5, effect: { xpBonus: 0.05 }, requires: [] },
        { id: 'xp2', name: 'XPブーストII', cost: 2, maxLevel: 5, effect: { xpBonus: 0.10 }, requires: ['xp1'] },
        { id: 'coin1', name: 'コインブーストI', cost: 1, maxLevel: 5, effect: { coinBonus: 0.05 }, requires: [] },
        { id: 'coin2', name: 'コインブーストII', cost: 2, maxLevel: 5, effect: { coinBonus: 0.10 }, requires: ['coin1'] }
      ]
    };
    
    function learnSkill(skillId) {
      const skill = findSkillById(skillId);
      if (!skill) return;
      
      if (!playerData.learnedSkills) playerData.learnedSkills = [];
      
      const learned = playerData.learnedSkills.find(s => s.id === skillId);
      const currentLevel = learned ? learned.level : 0;
      
      if (currentLevel >= skill.maxLevel) {
        showMessage('❌ スキルは最大レベルです', 'error');
        return;
      }
      
      for (let reqId of skill.requires) {
        const reqSkill = playerData.learnedSkills.find(s => s.id === reqId);
        if (!reqSkill || reqSkill.level === 0) {
          showMessage('❌ 前提スキルが必要です', 'error');
          return;
        }
      }
      
      if (playerData.skillPoints < skill.cost) {
        showMessage('❌ スキルポイントが足りません（必要: ' + skill.cost + '）', 'error');
        return;
      }
      
      playerData.skillPoints -= skill.cost;
      
      if (learned) {
        learned.level++;
      } else {
        playerData.learnedSkills.push({ id: skillId, level: 1 });
      }
      
      saveRPGData();
      updateRPGDisplay();
      updateSkillTreeDisplay();
      showMessage('✨ ' + skill.name + ' を習得しました！', 'success');
    }
    
    function resetSkills() {
      if (!confirm('スキルをリセットしますか？（コスト: 50ジェム）')) return;
      
      if (playerData.gems < 50) {
        showMessage('❌ ジェムが足りません（必要: 50）', 'error');
        return;
      }
      
      if (!playerData.learnedSkills) playerData.learnedSkills = [];
      
      let totalSP = 0;
      playerData.learnedSkills.forEach(learned => {
        const skill = findSkillById(learned.id);
        if (skill) {
          totalSP += skill.cost * learned.level;
        }
      });
      
      playerData.gems -= 50;
      playerData.skillPoints += totalSP;
      playerData.learnedSkills = [];
      
      saveRPGData();
      updateRPGDisplay();
      updateSkillTreeDisplay();
      showMessage('✅ スキルをリセットしました（' + totalSP + ' SPを返却）', 'success');
    }
    
    function findSkillById(id) {
      for (let skill of skillDatabase.attack) if (skill.id === id) return skill;
      for (let skill of skillDatabase.defense) if (skill.id === id) return skill;
      for (let skill of skillDatabase.support) if (skill.id === id) return skill;
      return null;
    }
    
    function getSkillBonus() {
      let bonus = { atk: 0, def: 0, xpBonus: 0, coinBonus: 0, critRate: 0, focusRegen: 0 };
      
      if (!playerData.learnedSkills) playerData.learnedSkills = [];
      
      playerData.learnedSkills.forEach(learned => {
        const skill = findSkillById(learned.id);
        if (skill && skill.effect) {
          for (let key in skill.effect) {
            bonus[key] = (bonus[key] || 0) + skill.effect[key] * learned.level;
          }
        }
      });
      
      return bonus;
    }
    
    function updateSkillTreeDisplay() {
      const pointsEl = document.getElementById('skillPointsDisplay');
      if (pointsEl) pointsEl.textContent = playerData.skillPoints || 0;
      
      updateSkillCategory('attack', 'attackSkills', '#ef4444');
      updateSkillCategory('defense', 'defenseSkills', '#3b82f6');
      updateSkillCategory('support', 'supportSkills', '#10b981');
    }
    
    function updateSkillCategory(category, containerId, color) {
      const container = document.getElementById(containerId);
      if (!container) return;
      
      // 配列の初期化を保証
      if (!playerData.learnedSkills) playerData.learnedSkills = [];
      
      const skills = skillDatabase[category];
      let html = '';
      
      skills.forEach(skill => {
        const learned = playerData.learnedSkills.find(s => s.id === skill.id);
        const currentLevel = learned ? learned.level : 0;
        const canLearn = currentLevel < skill.maxLevel && playerData.skillPoints >= skill.cost;
        
        let requirementsMet = true;
        let requirementText = '';
        if (skill.requires.length > 0) {
          skill.requires.forEach(reqId => {
            const reqSkill = findSkillById(reqId);
            const reqLearned = playerData.learnedSkills.find(s => s.id === reqId);
            if (!reqLearned || reqLearned.level === 0) {
              requirementsMet = false;
              requirementText = '前提: ' + reqSkill.name;
            }
          });
        }
        
        const effectText = Object.entries(skill.effect).map(([k, v]) => {
          if (k === 'atk') return 'ATK +' + v;
          if (k === 'def') return 'DEF +' + v;
          if (k === 'xpBonus') return 'XP +' + (v * 100) + '%';
          if (k === 'coinBonus') return 'コイン +' + (v * 100) + '%';
          if (k === 'critRate') return 'クリティカル +' + (v * 100) + '%';
          if (k === 'focusRegen') return '集中力回復 +' + (v * 100) + '%';
          return '';
        }).join(', ');
        
        html += '<div style="background: white; border: 2px solid ' + (currentLevel > 0 ? color : '#e5e7eb') + '; padding: 16px; border-radius: 12px;">';
        html += '<div style="display: flex; justify-content: space-between; margin-bottom: 8px;">';
        html += '<div><div style="font-size: 16px; font-weight: 700; color: #374151;">' + skill.name + (currentLevel > 0 ? ' Lv.' + currentLevel : '') + '</div>';
        html += '<div style="font-size: 12px; color: #6b7280;">' + effectText + '</div></div>';
        html += '<div style="font-size: 12px; font-weight: 700; color: ' + color + ';">' + skill.cost + ' SP</div></div>';
        
        if (requirementText) {
          html += '<div style="font-size: 11px; color: #f59e0b; margin-bottom: 8px;">' + requirementText + '</div>';
        }
        
        html += '<div style="background: #f3f4f6; height: 6px; border-radius: 3px; margin-bottom: 8px;">';
        html += '<div style="background: ' + color + '; height: 100%; width: ' + ((currentLevel / skill.maxLevel) * 100) + '%; border-radius: 3px;"></div></div>';
        
        html += '<div style="display: flex; justify-content: space-between; align-items: center;">';
        html += '<div style="font-size: 11px; color: #9ca3af;">' + currentLevel + '/' + skill.maxLevel + '</div>';
        if (canLearn && requirementsMet) {
          html += '<button onclick="learnSkill(\'' + skill.id + '\')" style="padding: 6px 12px; background: ' + color + '; color: white; border: none; border-radius: 6px; font-size: 12px; font-weight: 700; cursor: pointer;">習得</button>';
        }
        html += '</div></div>';
      });
      
      container.innerHTML = html;
    }

    
    // 📦 v3.0: 宝箱システム
    const chestTypes = {
      wood: { name: '木の宝箱', time: 60, rewards: { coins: 100, xp: 50 }, dropRate: 0.2, icon: '📦' },
      silver: { name: '銀の宝箱', time: 240, rewards: { coins: 500, xp: 200, gems: 1 }, dropRate: 0.1, icon: '🎁' },
      gold: { name: '金の宝箱', time: 480, rewards: { coins: 2000, xp: 1000, gems: 5 }, dropRate: 0.03, icon: '💎' }
    };
    
    function rollChestDrop(minutes) {
      if (minutes < 30) return null;
      
      const random = Math.random();
      
      if (random < chestTypes.gold.dropRate) return 'gold';
      if (random < chestTypes.gold.dropRate + chestTypes.silver.dropRate) return 'silver';
      if (random < chestTypes.gold.dropRate + chestTypes.silver.dropRate + chestTypes.wood.dropRate) return 'wood';
      
      return null;
    }
    
    function addChest(type) {
      if (!playerData.chests) playerData.chests = [];
      
      if (playerData.chests.length >= 4) {
        showMessage('❌ 宝箱は最大4個までです', 'error');
        return;
      }
      
      const now = new Date();
      const chestData = chestTypes[type];
      const unlockTime = new Date(now.getTime() + chestData.time * 60000);
      
      playerData.chests.push({
        type: type,
        startTime: now.toISOString(),
        unlockTime: unlockTime.toISOString()
      });
      
      saveRPGData();
      updateChestDisplay();
      
      showMessage('📦 ' + chestData.name + 'を獲得！', 'success');
    }
    
    function openChest(index) {
      if (!playerData.chests || !playerData.chests[index]) return;
      
      const chest = playerData.chests[index];
      const chestData = chestTypes[chest.type];
      const now = new Date();
      const unlockTime = new Date(chest.unlockTime);
      
      if (now < unlockTime) {
        const remainingMinutes = Math.ceil((unlockTime - now) / 60000);
        const gemCost = Math.ceil(remainingMinutes / 10);
        const coinCost = gemCost * 100;
        
        const choice = confirm('即開封しますか？\n\nジェム: ' + gemCost + '個\nまたは\nコイン: ' + coinCost + '個\n\n[OK] = ジェム使用 / [キャンセル] = コイン使用');
        
        if (choice) {
          // ジェム使用
          if (playerData.gems < gemCost) {
            showMessage('❌ ジェムが足りません（必要: ' + gemCost + '）', 'error');
            return;
          }
          playerData.gems -= gemCost;
        } else {
          // コイン使用
          if (playerData.coins < coinCost) {
            showMessage('❌ コインが足りません（必要: ' + coinCost + '）', 'error');
            return;
          }
          playerData.coins -= coinCost;
        }
      }
      
      let rewardMsg = '🎉 ' + chestData.name + 'を開封！\n\n';
      if (chestData.rewards.coins) {
        playerData.coins += chestData.rewards.coins;
        rewardMsg += '💰 +' + chestData.rewards.coins + ' コイン\n';
      }
      if (chestData.rewards.xp) {
        addXP(chestData.rewards.xp);
        rewardMsg += '⭐ +' + chestData.rewards.xp + ' XP\n';
      }
      if (chestData.rewards.gems) {
        playerData.gems += chestData.rewards.gems;
        rewardMsg += '💎 +' + chestData.rewards.gems + ' ジェム\n';
      }
      
      playerData.chests.splice(index, 1);
      
      saveRPGData();
      updateRPGDisplay();
      updateChestDisplay();
      
      showMessage(rewardMsg, 'success');
    }
    
    function formatTime(minutes) {
      if (minutes < 60) return minutes + '分';
      const hours = Math.floor(minutes / 60);
      const mins = minutes % 60;
      return hours + '時間' + (mins > 0 ? mins + '分' : '');
    }
    
    function updateChestDisplay() {
      const chestContainer = document.getElementById('chestContainer');
      if (!chestContainer) return;
      
      if (!playerData.chests) playerData.chests = [];
      
      let html = '';
      
      for (let i = 0; i < 4; i++) {
        if (i < playerData.chests.length) {
          const chest = playerData.chests[i];
          const chestData = chestTypes[chest.type];
          const now = new Date();
          const unlockTime = new Date(chest.unlockTime);
          const isReady = now >= unlockTime;
          const remaining = Math.max(0, Math.ceil((unlockTime - now) / 60000));
          
          html += '<div onclick="openChest(' + i + ')" style="background: ' + (isReady ? 'linear-gradient(135deg, #10b981, #059669)' : '#f3f4f6') + '; padding: 16px; border-radius: 12px; cursor: pointer; text-align: center;">';
          html += '<div style="font-size: 32px;">' + chestData.icon + '</div>';
          html += '<div style="font-size: 12px; font-weight: 700; margin-top: 8px; color: ' + (isReady ? 'white' : '#374151') + ';">' + chestData.name + '</div>';
          html += '<div style="font-size: 10px; margin-top: 4px; color: ' + (isReady ? 'white' : '#6b7280') + ';">';
          html += (isReady ? '✅ 開封可能' : '🕐 ' + formatTime(remaining));
          html += '</div></div>';
        } else {
          html += '<div style="background: #e5e7eb; padding: 16px; border-radius: 12px; text-align: center; opacity: 0.3;">';
          html += '<div style="font-size: 32px;">📦</div>';
          html += '<div style="font-size: 12px; margin-top: 8px;">空き</div>';
          html += '</div>';
        }
      }
      
      chestContainer.innerHTML = html;
    }

    
    // 🎯 v3.0: デイリークエスト
    const questTemplates = [
      { id: 'study30', name: '30分勉強する', reward: { xp: 100, coins: 50 }, target: 30 },
      { id: 'study60', name: '60分勉強する', reward: { xp: 200, coins: 100 }, target: 60 },
      { id: 'gacha1', name: 'ガチャを1回引く', reward: { xp: 50, coins: 50 }, target: 1 },
      { id: 'roulette1', name: 'ルーレットを回す', reward: { xp: 50, gems: 1 }, target: 1 },
      { id: 'equip1', name: '装備を1つ装着する', reward: { coins: 100 }, target: 1 },
      { id: 'login', name: 'ログインボーナス', reward: { coins: 50, gems: 1 }, target: 1 }
    ];
    
    function generateDailyQuests() {
      const today = toDateKeyLocal(new Date());
      
      if (playerData.lastQuestDate === today && playerData.dailyQuests && playerData.dailyQuests.length > 0) {
        return;
      }
      
      const shuffled = questTemplates.sort(() => Math.random() - 0.5);
      const selected = shuffled.slice(0, 3);
      
      playerData.dailyQuests = selected.map(template => ({
        id: template.id,
        name: template.name,
        completed: false,
        reward: template.reward,
        progress: 0,
        target: template.target
      }));
      
      playerData.lastQuestDate = today;
      saveRPGData();
    }
    
    function updateQuestProgress(questId, amount) {
      if (!playerData.dailyQuests) return;
      
      const quest = playerData.dailyQuests.find(q => q.id === questId);
      if (!quest || quest.completed) return;
      
      quest.progress += amount;
      
      if (quest.progress >= quest.target) {
        quest.completed = true;
        
        let rewardMsg = '🎯 クエスト達成: ' + quest.name + '\n\n';
        if (quest.reward.xp) {
          addXP(quest.reward.xp);
          rewardMsg += '⭐ +' + quest.reward.xp + ' XP\n';
        }
        if (quest.reward.coins) {
          playerData.coins += quest.reward.coins;
          rewardMsg += '💰 +' + quest.reward.coins + ' コイン\n';
        }
        if (quest.reward.gems) {
          playerData.gems += quest.reward.gems;
          rewardMsg += '💎 +' + quest.reward.gems + ' ジェム\n';
        }
        
        const allCompleted = playerData.dailyQuests.every(q => q.completed);
        if (allCompleted) {
          playerData.gems += 5;
          rewardMsg += '\n🎊 全クエスト達成ボーナス！\n💎 +5 ジェム';
        }
        
        saveRPGData();
        updateRPGDisplay();
        updateQuestDisplay();
        
        showMessage(rewardMsg, 'success');
      } else {
        saveRPGData();
        updateQuestDisplay();
      }
    }
    
    function updateQuestDisplay() {
      const questContainer = document.getElementById('questContainer');
      if (!questContainer) return;
      
      if (!playerData.dailyQuests || playerData.dailyQuests.length === 0) {
        questContainer.innerHTML = '<div style="text-align: center; padding: 20px; color: rgba(255,255,255,0.7); font-size: 12px;">クエストを生成中...</div>';
        return;
      }
      
      let html = '';
      
      playerData.dailyQuests.forEach(quest => {
        const progress = Math.min(100, (quest.progress / quest.target) * 100);
        const isCompleted = quest.completed;
        
        html += '<div style="background: ' + (isCompleted ? 'linear-gradient(135deg, #10b981, #059669)' : 'white') + '; border: 2px solid ' + (isCompleted ? '#10b981' : '#e5e7eb') + '; padding: 16px; border-radius: 12px; margin-bottom: 12px;">';
        html += '<div style="display: flex; justify-content: space-between; margin-bottom: 8px;">';
        html += '<div style="font-size: 16px; font-weight: 700; color: ' + (isCompleted ? 'white' : '#374151') + ';">';
        html += (isCompleted ? '✅' : '⏳') + ' ' + quest.name + '</div>';
        html += '<div style="font-size: 14px; color: ' + (isCompleted ? 'white' : '#6b7280') + ';">' + quest.progress + '/' + quest.target + '</div>';
        html += '</div>';
        html += '<div style="background: ' + (isCompleted ? 'rgba(255,255,255,0.3)' : '#f3f4f6') + '; height: 8px; border-radius: 4px; overflow: hidden;">';
        html += '<div style="background: ' + (isCompleted ? 'white' : '#667eea') + '; height: 100%; width: ' + progress + '%;"></div>';
        html += '</div>';
        html += '<div style="font-size: 12px; margin-top: 8px; color: ' + (isCompleted ? 'white' : '#6b7280') + ';">';
        const rewards = [];
        if (quest.reward.xp) rewards.push('⭐' + quest.reward.xp + ' XP');
        if (quest.reward.coins) rewards.push('💰' + quest.reward.coins);
        if (quest.reward.gems) rewards.push('💎' + quest.reward.gems);
        html += rewards.join(' ');
        html += '</div></div>';
      });
      
      questContainer.innerHTML = html;
    }

    
    // 👾 v3.0: ボスシステム
    const weeklyBosses = [
      { name: '労働基準法の守護者', hp: 5000, atk: 200, rewards: { xp: 1000, coins: 500, gems: 10 }, icon: '⚔️' },
      { name: '社会保険の番人', hp: 6000, atk: 250, rewards: { xp: 1200, coins: 600, gems: 12 }, icon: '🛡️' },
      { name: '労災保険の精霊', hp: 7000, atk: 300, rewards: { xp: 1500, coins: 700, gems: 15 }, icon: '💊' }
    ];
    
    const monthlyBoss = {
      name: '社労士試験の化身',
      hp: 50000,
      atk: 500,
      rewards: { xp: 10000, coins: 5000, gems: 100 },
      icon: '👑'
    };
    
    function canFightWeeklyBoss() {
      if (!playerData.weekBossDefeatedDate) return true;
      
      const lastDefeat = new Date(playerData.weekBossDefeatedDate);
      const now = new Date();
      const daysSince = Math.floor((now - lastDefeat) / (1000 * 60 * 60 * 24));
      
      return daysSince >= 7;
    }
    
    function canFightMonthlyBoss() {
      if (!playerData.monthBossDefeatedDate) return true;
      
      const lastDefeat = new Date(playerData.monthBossDefeatedDate);
      const now = new Date();
      
      return lastDefeat.getMonth() !== now.getMonth() || lastDefeat.getFullYear() !== now.getFullYear();
    }
    
    function fightWeeklyBoss() {
      if (!canFightWeeklyBoss()) {
        showMessage('❌ 週ボスは週1回しか挑戦できません', 'error');
        return;
      }
      
      const boss = weeklyBosses[Math.floor(Math.random() * weeklyBosses.length)];
      
      const playerATK = calculateATK();
      const turnsToWin = Math.ceil(boss.hp / playerATK);
      const damage = boss.atk * turnsToWin;
      const playerDEF = calculateDEF();
      
      if (damage > playerDEF * 2) {
        showMessage('❌ 戦力不足です！装備とスキルを強化してください\n必要DEF: ' + Math.ceil(damage / 2), 'error');
        return;
      }
      
      playerData.weekBossDefeatedDate = new Date().toISOString();
      addXP(boss.rewards.xp);
      playerData.coins += boss.rewards.coins;
      playerData.gems += boss.rewards.gems;
      
      saveRPGData();
      updateRPGDisplay();
      
      showMessage('🎉 ' + boss.icon + ' ' + boss.name + 'を撃破！\n\n⭐ +' + boss.rewards.xp + ' XP\n💰 +' + boss.rewards.coins + ' コイン\n💎 +' + boss.rewards.gems + ' ジェム', 'success');
    }
    
    function fightMonthlyBoss() {
      if (!canFightMonthlyBoss()) {
        showMessage('❌ 月ボスは月1回しか挑戦できません', 'error');
        return;
      }
      
      const boss = monthlyBoss;
      
      const playerATK = calculateATK();
      const turnsToWin = Math.ceil(boss.hp / playerATK);
      const damage = boss.atk * turnsToWin;
      const playerDEF = calculateDEF();
      
      if (damage > playerDEF * 3) {
        showMessage('❌ 戦力不足です！装備とスキルを強化してください\n必要DEF: ' + Math.ceil(damage / 3), 'error');
        return;
      }
      
      playerData.monthBossDefeatedDate = new Date().toISOString();
      addXP(boss.rewards.xp);
      playerData.coins += boss.rewards.coins;
      playerData.gems += boss.rewards.gems;
      
      saveRPGData();
      updateRPGDisplay();
      
      showMessage('🎊 ' + boss.icon + ' ' + boss.name + 'を撃破！\n\n⭐ +' + boss.rewards.xp + ' XP\n💰 +' + boss.rewards.coins + ' コイン\n💎 +' + boss.rewards.gems + ' ジェム', 'success');
    }

    
    // 🎁 v3.5: ショップシステム
    const shopItems = {
      equipment: [
        { id: 'shop_epic_weapon', name: 'エピック武器ガチャ', price: 100, currency: 'gems', type: 'equipment', rarity: 'Epic' },
        { id: 'shop_legendary_weapon', name: 'レジェンド武器ガチャ', price: 500, currency: 'gems', type: 'equipment', rarity: 'Legendary' }
      ],
      consumables: [
        { id: 'xp_boost_small', name: 'XPブースト（小）', price: 500, currency: 'coins', effect: { type: 'xp', value: 500 } },
        { id: 'xp_boost_large', name: 'XPブースト（大）', price: 2000, currency: 'coins', effect: { type: 'xp', value: 2000 } },
        { id: 'coin_boost', name: 'コインブースト', price: 1000, currency: 'coins', effect: { type: 'coins', value: 1000 } },
        { id: 'focus_restore', name: '集中力全回復', price: 300, currency: 'coins', effect: { type: 'focus', value: 5 } },
        { id: 'skill_point', name: 'スキルポイント', price: 1000, currency: 'coins', effect: { type: 'sp', value: 1 } }
      ]
    };
    
    function refreshShop() {
      const today = toDateKeyLocal(new Date());
      
      if (playerData.shopRefreshDate === today) {
        return; // 今日は既に更新済み
      }
      
      playerData.shopRefreshDate = today;
      saveRPGData();
    }
    
    function buyShopItem(itemId) {
      let item = null;
      
      // アイテムを検索
      for (let category in shopItems) {
        const found = shopItems[category].find(i => i.id === itemId);
        if (found) {
          item = found;
          break;
        }
      }
      
      if (!item) return;
      
      // 通貨チェック
      if (item.currency === 'gems') {
        if (playerData.gems < item.price) {
          showMessage('❌ ジェムが足りません（必要: ' + item.price + '）', 'error');
          return;
        }
      } else if (item.currency === 'coins') {
        if (playerData.coins < item.price) {
          showMessage('❌ コインが足りません（必要: ' + item.price + '）', 'error');
          return;
        }
      }
      
      // 購入処理
      if (item.currency === 'gems') {
        playerData.gems -= item.price;
      } else {
        playerData.coins -= item.price;
      }
      
      // アイテム効果適用
      if (item.type === 'equipment') {
        // 装備ガチャ
        let equipment;
        const allEquipment = [
          ...equipmentDatabase.weapons.filter(e => e.rarity === item.rarity),
          ...equipmentDatabase.armors.filter(e => e.rarity === item.rarity),
          ...equipmentDatabase.accessories.filter(e => e.rarity === item.rarity)
        ];
        
        if (allEquipment.length > 0) {
          equipment = allEquipment[Math.floor(Math.random() * allEquipment.length)];
          const newEquipment = { ...equipment, level: 0 };
          playerData.inventory.push(newEquipment);
          
          showMessage('🎉 ' + item.name + 'を購入！\n\n' + equipment.rarity + ' ' + equipment.name + ' を獲得！', 'success');
        }
      } else if (item.effect) {
        // 消費アイテム
        if (item.effect.type === 'xp') {
          addXP(item.effect.value);
          showMessage('✨ ' + item.name + 'を使用！\n\n⭐ +' + item.effect.value + ' XP', 'success');
        } else if (item.effect.type === 'coins') {
          playerData.coins += item.effect.value;
          showMessage('✨ ' + item.name + 'を使用！\n\n💰 +' + item.effect.value + ' コイン', 'success');
        } else if (item.effect.type === 'focus') {
          playerData.focus = 5;
          showMessage('✨ ' + item.name + 'を使用！\n\n❤️ 集中力全回復', 'success');
        } else if (item.effect.type === 'sp') {
          playerData.skillPoints += item.effect.value;
          showMessage('✨ ' + item.name + 'を購入！\n\n🌟 +' + item.effect.value + ' スキルポイント', 'success');
        }
      }
      
      saveRPGData();
      updateRPGDisplay();
      updateShopDisplay();
    }
    

    
    // 📈 v3.5: 統計・分析システム
    function updateShopCoins() {
      const coinsEl = document.getElementById('shopCoins');
      const gemsEl = document.getElementById('shopGems');
      if (coinsEl) coinsEl.textContent = playerData.coins || 0;
      if (gemsEl) gemsEl.textContent = playerData.gems || 0;
    }
    
    function updateAnalyticsDisplay() {
      calculateAnalytics();
      drawWeeklyChart();
      drawSubjectChart();
    }
    
    function calculateAnalytics() {
      let totalMinutes = 0;
      let studyDays = 0;
      let maxStreak = 0;
      
      // 総学習時間と記録日数を計算
      for (let dateKey in studyRecords) {
        let dayTotal = 0;
        for (let subject in studyRecords[dateKey]) {
          const minutes = studyRecords[dateKey][subject];
          totalMinutes += minutes;
          dayTotal += minutes;
        }
        if (dayTotal > 0) studyDays++;
      }
      
      // 最長ストリーク計算
      const dates = Object.keys(studyRecords).sort();
      let currentStreak = 0;
      let lastDate = null;
      
      dates.forEach(dateKey => {
        let dayTotal = 0;
        for (let subject in studyRecords[dateKey]) {
          dayTotal += studyRecords[dateKey][subject];
        }
        
        if (dayTotal > 0) {
          if (lastDate) {
            const lastD = new Date(lastDate);
            const currentD = new Date(dateKey);
            const diffDays = Math.floor((currentD - lastD) / (1000 * 60 * 60 * 24));
            
            if (diffDays === 1) {
              currentStreak++;
            } else {
              currentStreak = 1;
            }
          } else {
            currentStreak = 1;
          }
          
          if (currentStreak > maxStreak) {
            maxStreak = currentStreak;
          }
          
          lastDate = dateKey;
        }
      });
      
      // 表示更新
      const totalHours = Math.floor(totalMinutes / 60);
      const totalMins = totalMinutes % 60;
      document.getElementById('totalStudyTime').textContent = totalHours + '時間' + (totalMins > 0 ? totalMins + '分' : '');
      document.getElementById('maxStreak').textContent = maxStreak + '日';
      document.getElementById('studyDays').textContent = studyDays + '日';
      
      const avgMinutes = studyDays > 0 ? Math.floor(totalMinutes / studyDays) : 0;
      document.getElementById('avgTimePerDay').textContent = avgMinutes + '分';
    }
    
    function drawWeeklyChart() {
      const canvas = document.getElementById('weeklyCanvas');
      if (!canvas) return;
      
      const ctx = canvas.getContext('2d');
      const width = canvas.width;
      const height = canvas.height;
      
      // クリア
      ctx.clearRect(0, 0, width, height);
      
      // 過去7日間のデータを取得
      const today = new Date();
      const weekData = [];
      const labels = ['日', '月', '火', '水', '木', '金', '土'];
      
      for (let i = 6; i >= 0; i--) {
        const date = new Date(today);
        date.setDate(date.getDate() - i);
        const dateKey = toDateKeyLocal(date);
        
        let dayTotal = 0;
        if (studyRecords[dateKey]) {
          for (let subject in studyRecords[dateKey]) {
            dayTotal += studyRecords[dateKey][subject];
          }
        }
        weekData.push(dayTotal);
      }
      
      // 最大値
      const maxMinutes = Math.max(...weekData, 60);
      
      // バーグラフ描画
      const barWidth = width / 7 - 10;
      const barSpacing = 10;
      
      weekData.forEach((minutes, i) => {
        const barHeight = (minutes / maxMinutes) * (height - 40);
        const x = i * (barWidth + barSpacing) + barSpacing;
        const y = height - barHeight - 20;
        
        // バー
        ctx.fillStyle = '#667eea';
        ctx.fillRect(x, y, barWidth, barHeight);
        
        // ラベル
        ctx.fillStyle = '#374151';
        ctx.font = '12px sans-serif';
        ctx.textAlign = 'center';
        ctx.fillText(labels[(today.getDay() - 6 + i + 7) % 7], x + barWidth / 2, height - 5);
        
        // 時間表示
        if (minutes > 0) {
          const hours = Math.floor(minutes / 60);
          const mins = minutes % 60;
          const timeText = hours > 0 ? hours + 'h' + (mins > 0 ? mins + 'm' : '') : mins + 'm';
          ctx.fillStyle = '#ffffff';
          ctx.font = 'bold 11px sans-serif';
          ctx.fillText(timeText, x + barWidth / 2, y - 5);
        }
      });
    }
    
    function drawSubjectChart() {
      const canvas = document.getElementById('subjectCanvas');
      if (!canvas) return;
      
      const ctx = canvas.getContext('2d');
      const width = canvas.width;
      const height = canvas.height;
      const centerX = width / 2;
      const centerY = height / 2;
      const radius = Math.min(width, height) / 2 - 20;
      
      // クリア
      ctx.clearRect(0, 0, width, height);
      
      // 科目別合計を計算
      const subjectTotals = {};
      for (let dateKey in studyRecords) {
        for (let subject in studyRecords[dateKey]) {
          if (!subjectTotals[subject]) subjectTotals[subject] = 0;
          subjectTotals[subject] += studyRecords[dateKey][subject];
        }
      }
      
      // ソート
      const sortedSubjects = Object.entries(subjectTotals).sort((a, b) => b[1] - a[1]);
      
      if (sortedSubjects.length === 0) {
        ctx.fillStyle = '#9ca3af';
        ctx.font = '16px sans-serif';
        ctx.textAlign = 'center';
        ctx.fillText('データがありません', centerX, centerY);
        return;
      }
      
      const total = sortedSubjects.reduce((sum, [, minutes]) => sum + minutes, 0);
      
      // 色
      const colors = ['#ef4444', '#f59e0b', '#10b981', '#3b82f6', '#8b5cf6', '#ec4899', '#14b8a6', '#f97316', '#84cc16', '#6366f1'];
      
      // 円グラフ描画
      let startAngle = -Math.PI / 2;
      
      sortedSubjects.forEach(([subject, minutes], i) => {
        const sliceAngle = (minutes / total) * 2 * Math.PI;
        
        ctx.fillStyle = colors[i % colors.length];
        ctx.beginPath();
        ctx.moveTo(centerX, centerY);
        ctx.arc(centerX, centerY, radius, startAngle, startAngle + sliceAngle);
        ctx.closePath();
        ctx.fill();
        
        // パーセンテージ表示
        if (sliceAngle > 0.1) {
          const midAngle = startAngle + sliceAngle / 2;
          const textX = centerX + Math.cos(midAngle) * radius * 0.7;
          const textY = centerY + Math.sin(midAngle) * radius * 0.7;
          
          ctx.fillStyle = '#ffffff';
          ctx.font = 'bold 12px sans-serif';
          ctx.textAlign = 'center';
          ctx.fillText(Math.round((minutes / total) * 100) + '%', textX, textY);
        }
        
        startAngle += sliceAngle;
      });
      
      // 凡例
      let legendY = 10;
      sortedSubjects.forEach(([subject, minutes], i) => {
        if (i < 10) {
          ctx.fillStyle = colors[i % colors.length];
          ctx.fillRect(width + 20, legendY, 15, 15);
          
          ctx.fillStyle = '#374151';
          ctx.font = '12px sans-serif';
          ctx.textAlign = 'left';
          const hours = Math.floor(minutes / 60);
          const mins = minutes % 60;
          const timeText = hours > 0 ? hours + 'h' + mins + 'm' : mins + 'm';
          ctx.fillText(subject.substring(0, 10) + ': ' + timeText, width + 40, legendY + 12);
          
          legendY += 25;
        }
      });
    }
    function updateShopDisplay() {
      const shopContainer = document.getElementById('shopContainer');
      if (!shopContainer) return;
      
      let html = '';
      
      // 装備ガチャ
      html += '<div style="margin-bottom: 20px;">';
      html += '<h3 style="color: #a855f7; margin-bottom: 12px;">⚔️ 装備ガチャ</h3>';
      html += '<div style="display: grid; grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); gap: 12px;">';
      
      shopItems.equipment.forEach(item => {
        html += '<div style="background: white; border: 2px solid #a855f7; padding: 16px; border-radius: 12px;">';
        html += '<div style="font-size: 16px; font-weight: 700; color: #374151; margin-bottom: 8px;">' + item.name + '</div>';
        html += '<div style="font-size: 14px; color: #6b7280; margin-bottom: 12px;">' + item.rarity + '装備確定</div>';
        html += '<button onclick="buyShopItem(\'' + item.id + '\')" style="width: 100%; padding: 10px; background: linear-gradient(135deg, #a855f7, #9333ea); color: white; border: none; border-radius: 8px; font-weight: 700; cursor: pointer;">';
        html += '💎 ' + item.price + ' ジェム';
        html += '</button></div>';
      });
      
      html += '</div></div>';
      
      // 消費アイテム
      html += '<div style="margin-bottom: 20px;">';
      html += '<h3 style="color: #10b981; margin-bottom: 12px;">✨ 消費アイテム</h3>';
      html += '<div style="display: grid; grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); gap: 12px;">';
      
      shopItems.consumables.forEach(item => {
        html += '<div style="background: white; border: 2px solid #10b981; padding: 16px; border-radius: 12px;">';
        html += '<div style="font-size: 16px; font-weight: 700; color: #374151; margin-bottom: 8px;">' + item.name + '</div>';
        
        let effectText = '';
        if (item.effect.type === 'xp') effectText = '⭐ +' + item.effect.value + ' XP';
        else if (item.effect.type === 'coins') effectText = '💰 +' + item.effect.value + ' コイン';
        else if (item.effect.type === 'focus') effectText = '❤️ 集中力全回復';
        else if (item.effect.type === 'sp') effectText = '🌟 +' + item.effect.value + ' SP';
        
        html += '<div style="font-size: 14px; color: #6b7280; margin-bottom: 12px;">' + effectText + '</div>';
        html += '<button onclick="buyShopItem(\'' + item.id + '\')" style="width: 100%; padding: 10px; background: linear-gradient(135deg, #10b981, #059669); color: white; border: none; border-radius: 8px; font-weight: 700; cursor: pointer;">';
        html += '💰 ' + item.price + ' コイン';
        html += '</button></div>';
      });
      
      html += '</div></div>';
      
      shopContainer.innerHTML = html;
    }
    function getEquipmentBonus() {
      let atkBonus = 0;
      let defBonus = 0;
      let xpBonus = 0;
      let coinBonus = 0;
      
      if (playerData.equippedWeapon) {
        const level = playerData.equippedWeapon.level || 0;
        const bonus = getEnhancementBonus(level);
        atkBonus += playerData.equippedWeapon.atk * bonus;
        defBonus += playerData.equippedWeapon.def * bonus;
      }
      
      if (playerData.equippedArmor) {
        const level = playerData.equippedArmor.level || 0;
        const bonus = getEnhancementBonus(level);
        atkBonus += playerData.equippedArmor.atk * bonus;
        defBonus += playerData.equippedArmor.def * bonus;
      }
      
      if (playerData.equippedAccessory) {
        const level = playerData.equippedAccessory.level || 0;
        const bonus = getEnhancementBonus(level);
        atkBonus += playerData.equippedAccessory.atk * bonus;
        defBonus += playerData.equippedAccessory.def * bonus;
        if (playerData.equippedAccessory.effects.xpBonus) {
          xpBonus += playerData.equippedAccessory.effects.xpBonus;
        }
        if (playerData.equippedAccessory.effects.coinBonus) {
          coinBonus += playerData.equippedAccessory.effects.coinBonus;
        }
      }
      
      return { atk: Math.floor(atkBonus), def: Math.floor(defBonus), xpBonus, coinBonus };
    }
    
    function findEquipmentById(id) {
      for (let w of equipmentDatabase.weapons) if (w.id === id) return w;
      for (let a of equipmentDatabase.armors) if (a.id === id) return a;
      for (let c of equipmentDatabase.accessories) if (c.id === id) return c;
      return null;
    }
    
    function equipItem(equipment, slot) {
      if (slot === 'weapon') playerData.equippedWeapon = equipment;
      else if (slot === 'armor') playerData.equippedArmor = equipment;
      else if (slot === 'accessory') playerData.equippedAccessory = equipment;
      
      saveRPGData();
      updateRPGDisplay();
      updateQuestProgress('equip1', 1);
      showMessage(equipment.name + 'を装備しました！', 'success');
    }
    
    function unequipItem(slot) {
      if (slot === 'weapon') playerData.equippedWeapon = null;
      else if (slot === 'armor') playerData.equippedArmor = null;
      else if (slot === 'accessory') playerData.equippedAccessory = null;
      
      saveRPGData();
      updateRPGDisplay();
      showMessage('装備を外しました', 'info');
    }

    const subjectBosses = {
      '労働基準法': { name: '労働基準法ドラゴン', hp: 120, difficulty: 2, icon: '🐉' },
      '労働安全衛生法': { name: '労働安全衛生法ガーディアン', hp: 80, difficulty: 1, icon: '🛡️' },
      '労働者災害補償保険法': { name: '労災保険法ヒーラー', hp: 120, difficulty: 3, icon: '💊' },
      '雇用保険法': { name: '雇用保険法マーチャント', hp: 120, difficulty: 3, icon: '💼' },
      '労働保険徴収法': { name: '労働保険徴収法トレジャー', hp: 70, difficulty: 2, icon: '💰' },
      '健康保険法': { name: '健康保険法ドクター', hp: 130, difficulty: 4, icon: '🏥' },
      '国民年金法': { name: '国民年金法エルダー', hp: 150, difficulty: 5, icon: '👴' },
      '厚生年金保険法': { name: '厚生年金法タイクーン', hp: 150, difficulty: 5, icon: '🏢' },
      '社会保険に関する一般常識': { name: '社保一般常識ライブラリアン', hp: 80, difficulty: 3, icon: '📚' },
      '労働に関する一般常識': { name: '労働一般常識ジャッジ', hp: 80, difficulty: 3, icon: '⚖️' },
      'その他': { name: 'その他ダンジョンマスター', hp: 999, difficulty: 0, icon: '🎲' }
    };
    
    // レベルアップ必要XP
    function getRequiredXP(level) {
      if (level <= 10) return 100 * level;
      if (level <= 20) return 200 * level;
      if (level <= 30) return 300 * level;
      if (level <= 50) return 500 * level;
      if (level <= 70) return 700 * level;
      if (level <= 100) return 1000 * level;
      if (level <= 120) return 1200 * level;
      return 1500 * level;
    }

    let sheetData = {};
    let trendChartInstance = null;
    let weekdayChartInstance = null;
    let editingIndex = -1;
    
    const subjects = [
      '労働基準法',
      '労働安全衛生法',
      '労働者災害補償保険法',
      '雇用保険法',
      '労働保険徴収法',
      '健康保険法',
      '国民年金法',
      '厚生年金保険法',
      '社会保険に関する一般常識',
      '労働に関する一般常識',
      'その他'
    ];
    
    // 科目別推定勉強時間（時間）
    const subjectTargets = {
      '労働基準法': 120,
      '労働安全衛生法': 80,
      '労働者災害補償保険法': 120,
      '雇用保険法': 120,
      '労働保険徴収法': 70,
      '健康保険法': 130,
      '国民年金法': 150,
      '厚生年金保険法': 150,
      '社会保険に関する一般常識': 80,
      '労働に関する一般常識': 80,
      'その他': 0
    };
    
    // 科目別の色
    const subjectColors = [
      '#667eea', '#f59e0b', '#10b981', '#ef4444', '#8b5cf6',
      '#ec4899', '#06b6d4', '#f97316', '#14b8a6', '#6366f1', '#84cc16'
    ];
    
    // 初期化
    window.onload = function() {
      updateDateDisplay();
      loadRecords();
      loadSheetData();
      generateSheetRows();
      updateSheetSums();
      updateLogTable();
      
      // 🎮 RPG初期化
      updateRPGDisplay();
      checkAchievements();
      updateBossBattle();
      
      // v1.0: オフライン報酬表示
      setTimeout(() => {
        const offlineRewards = calculateOfflineRewards();
        if (offlineRewards) {
          showOfflineRewards(offlineRewards);
        }
      }, 500);
      
      // v2.1: 装備表示初期化
      updateEquipmentDisplay();
      updateInventoryDisplay();
      
      // v3.0: 初期化
      generateDailyQuests();
      updateQuestDisplay();
      updateChestDisplay();
      updateSkillTreeDisplay();
      updateQuestProgress('login', 1);
      
      // v3.5: ショップ更新
      refreshShop();
    };
    
    // データ読み込み・保存
    function loadRecords() {
      const saved = localStorage.getItem('studyRecords');
      if (saved) {
        studyRecords = JSON.parse(saved);
      }
      loadRPGData();
    }

    // 🎮 RPGデータ保存・読み込み
    function saveRPGData() {
      localStorage.setItem('rpgData', JSON.stringify(playerData));
    }
    
    function loadRPGData() {
      const saved = localStorage.getItem('rpgData');
      if (saved) {
        playerData = JSON.parse(saved);
      }
      // 配列の初期化を保証
      if (!playerData.achievements) playerData.achievements = [];
      if (!playerData.defeatedBosses) playerData.defeatedBosses = [];
      if (!playerData.inventory) playerData.inventory = [];
      
      // v3.0: 新しいフィールドの初期化
      if (!playerData.learnedSkills) playerData.learnedSkills = [];
      if (!playerData.chests) playerData.chests = [];
      if (!playerData.defeatedEnemies) playerData.defeatedEnemies = [];
      if (!playerData.dailyQuests) playerData.dailyQuests = [];
      if (playerData.skillPoints === undefined) playerData.skillPoints = 0;
      
      // v3.5: ショップ初期化
      if (!playerData.purchasedItems) playerData.purchasedItems = [];
      
      updateRPGDisplay();
    }
    
    // 🎮 RPG表示更新

    // ⚔️ ATK/DEF計算

    // ❤️ 集中力システム（ハイブリッド）

    // 💰 オフライン報酬

    // 🎰 デイリールーレット
    function rollDailyRoulette() {
      const today = new Date();
      const todayKey = toDateKeyLocal(today);
      
      if (playerData.lastRouletteDate === todayKey) {
        showMessage('❌ 今日のルーレットは既に回しました', 'error');
        return;
      }
      
      const random = Math.random();
      let reward;
      
      if (random < 0.01) {
        reward = { type: 'gems', amount: 50, name: '🌟 大当たり！ジェム×50', color: '#f59e0b' };
      } else if (random < 0.05) {
        reward = { type: 'gems', amount: 5, name: '💎 ジェム×5', color: '#a855f7' };
      } else if (random < 0.15) {
        reward = { type: 'gems', amount: 1, name: '💎 ジェム×1', color: '#3b82f6' };
      } else if (random < 0.40) {
        reward = { type: 'xp', amount: 200, name: '⭐ XP×200', color: '#10b981' };
      } else {
        reward = { type: 'coins', amount: 100, name: '💰 コイン×100', color: '#64748b' };
      }
      
      // 報酬付与
      if (reward.type === 'gems') playerData.gems += reward.amount;
      else if (reward.type === 'xp') addXP(reward.amount);
      else if (reward.type === 'coins') playerData.coins += reward.amount;
      
      playerData.lastRouletteDate = todayKey;
      saveRPGData();
      updateRPGDisplay();
      
      updateQuestProgress('roulette1', 1);
      
      showRouletteResult(reward);
    }
    
    function showRouletteResult(reward) {
      const overlay = document.createElement('div');
      overlay.style.cssText = `
        position: fixed; top: 0; left: 0; width: 100%; height: 100%;
        background: rgba(0,0,0,0.9); z-index: 10000;
        display: flex; align-items: center; justify-content: center;
        animation: fadeIn 0.3s;
      `;
      
      overlay.innerHTML = `
        <div style="background: ${reward.color}; padding: 50px; border-radius: 20px; text-align: center; color: white; animation: scaleIn 0.5s;">
          <div style="font-size: 64px; margin-bottom: 20px;">🎰</div>
          <div style="font-size: 32px; font-weight: 900; margin-bottom: 20px;">デイリールーレット</div>
          <div style="font-size: 28px; font-weight: 700; margin-bottom: 30px;">${reward.name}</div>
          <button onclick="this.parentElement.parentElement.remove()" style="padding: 15px 50px; background: white; color: ${reward.color}; border: none; border-radius: 12px; font-size: 20px; font-weight: 700; cursor: pointer;">やった！</button>
        </div>
      `;
      
      document.body.appendChild(overlay);
    }
    

    // ❤️ 集中力表示更新
    function updateFocusDisplay() {
      const focusCountEl = document.getElementById('focusCount');
      const focusHeartsEl = document.getElementById('focusHearts');
      const focusInfoEl = document.getElementById('focusInfo');
      
      if (focusCountEl) focusCountEl.textContent = `${playerData.focus}/5`;
      
      if (focusHeartsEl) {
        let heartsHTML = '';
        for (let i = 0; i < 5; i++) {
          if (i < playerData.focus) {
            heartsHTML += '<span style="font-size: 24px;">❤️</span>';
          } else {
            heartsHTML += '<span style="font-size: 24px;">♡</span>';
          }
        }
        focusHeartsEl.innerHTML = heartsHTML;
      }
      
      if (focusInfoEl) {
        if (playerData.focus > 0) {
          focusInfoEl.innerHTML = '集中力ボーナス: +50% 報酬<br>30分で1回復、日またぎで全回復';
        } else {
          // 次回復時刻計算
          if (playerData.lastFocusUpdate) {
            const lastUpdate = new Date(playerData.lastFocusUpdate);
            const nextRecovery = new Date(lastUpdate.getTime() + 30 * 60 * 1000);
            const now = new Date();
            const minutesLeft = Math.max(0, Math.ceil((nextRecovery - now) / (1000 * 60)));
            focusInfoEl.innerHTML = `⚠️ 集中力0（ボーナスなし）<br>次回復: ${minutesLeft}分後`;
          } else {
            focusInfoEl.innerHTML = '⚠️ 集中力0（ボーナスなし）';
          }
        }
      }
    }
    
    // 🎰 ルーレットUI更新
    function updateRouletteUI() {
      const today = new Date();
      const todayKey = toDateKeyLocal(today);
      const rouletteBtn = document.getElementById('rouletteBtn');
      const rouletteStatus = document.getElementById('rouletteStatus');
      
      if (!rouletteBtn) return;
      
      if (playerData.lastRouletteDate === todayKey) {
        rouletteBtn.disabled = true;
        rouletteBtn.style.opacity = '0.5';
        rouletteBtn.style.cursor = 'not-allowed';
        if (rouletteStatus) rouletteStatus.textContent = '本日済み';
      } else {
        rouletteBtn.disabled = false;
        rouletteBtn.style.opacity = '1';
        rouletteBtn.style.cursor = 'pointer';
        if (rouletteStatus) rouletteStatus.textContent = '利用可能';
      }
    }

    
    // 🎒 インベントリ管理
    let currentFilter = 'all';
    
    function updateInventoryDisplay() {
      const inventoryList = document.getElementById('inventoryList');
      if (!inventoryList) return;
      
      if (!playerData.inventory || playerData.inventory.length === 0) {
        inventoryList.innerHTML = '<div style="grid-column: 1/-1; text-align: center; padding: 40px; color: #9ca3af;">インベントリが空です</div>';
        return;
      }
      
      let html = '';
      
      playerData.inventory.forEach((item, index) => {
        if (!item.id) return;
        
        const slot = item.id.startsWith('w') ? 'weapon' : item.id.startsWith('a') ? 'armor' : 'accessory';
        if (currentFilter !== 'all' && slot !== currentFilter) return;
        
        const slotIcon = item.id.startsWith('w') ? '🗡️' : item.id.startsWith('a') ? '🛡️' : '💍';
        const rarityColor = { 'Common': '#64748b', 'Rare': '#3b82f6', 'Epic': '#a855f7', 'Legendary': '#f59e0b' }[item.rarity] || '#64748b';
        const level = item.level || 0;
        const enhanceBonus = getEnhancementBonus(level);
        
        html += `
          <div class="equipment-card ${item.rarity}" onclick="showEquipmentDetail(${index})">
            <div style="font-size: 12px; color: ${rarityColor}; font-weight: 700; margin-bottom: 4px;">${item.rarity}</div>
            <div style="font-size: 24px; margin-bottom: 8px;">${slotIcon}</div>
            <div style="font-size: 16px; font-weight: 700; margin-bottom: 8px;">${item.name}</div>
            <div style="font-size: 14px; color: #6b7280;">
              ⚔️ ${Math.floor(item.atk * enhanceBonus)}<br>
              🛡️ ${Math.floor(item.def * enhanceBonus)}
            </div>
            ${level > 0 ? `<div style="margin-top: 8px; font-size: 12px; color: #10b981; font-weight: 700;">+${level}</div>` : ''}
          </div>
        `;
      });
      
      inventoryList.innerHTML = html || '<div style="grid-column: 1/-1; text-align: center; padding: 40px; color: #9ca3af;">該当する装備がありません</div>';
    }
    
    function filterInventory(type) {
      currentFilter = type;
      
      document.querySelectorAll('.filter-btn').forEach(btn => btn.classList.remove('active'));
      document.getElementById('filter-' + type).classList.add('active');
      
      updateInventoryDisplay();
    }
    
    function showEquipmentDetail(index) {
      const item = playerData.inventory[index];
      if (!item || !item.id) return;
      
      const slotIcon = item.id.startsWith('w') ? '🗡️' : item.id.startsWith('a') ? '🛡️' : '💍';
      const slot = item.id.startsWith('w') ? 'weapon' : item.id.startsWith('a') ? 'armor' : 'accessory';
      const rarityColor = { 'Common': '#64748b', 'Rare': '#3b82f6', 'Epic': '#a855f7', 'Legendary': '#f59e0b' }[item.rarity] || '#64748b';
      const level = item.level || 0;
      const currentAtk = Math.floor(item.atk * getEnhancementBonus(level));
      const currentDef = Math.floor(item.def * getEnhancementBonus(level));
      const nextAtk = Math.floor(item.atk * getEnhancementBonus(level + 1));
      const nextDef = Math.floor(item.def * getEnhancementBonus(level + 1));
      const enhanceCost = (level + 1) * 100;
      
      const overlay = document.createElement('div');
      overlay.style.cssText = 'position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.8); z-index: 10000; display: flex; align-items: center; justify-content: center; animation: fadeIn 0.3s;';
      
      overlay.innerHTML = `
        <div style="background: white; padding: 30px; border-radius: 20px; max-width: 500px; animation: scaleIn 0.3s;">
          <div style="font-size: 14px; color: ${rarityColor}; font-weight: 700; margin-bottom: 8px;">${item.rarity}</div>
          <div style="font-size: 48px; margin-bottom: 12px;">${slotIcon}</div>
          <div style="font-size: 28px; font-weight: 900; margin-bottom: 20px;">${item.name} ${level > 0 ? '+' + level : ''}</div>
          
          <div style="background: #f3f4f6; padding: 16px; border-radius: 12px; margin-bottom: 20px;">
            <div style="font-size: 18px; font-weight: 700; margin-bottom: 12px;">現在のステータス</div>
            <div style="font-size: 16px; color: #374151;">
              ⚔️ ATK: ${currentAtk}<br>
              🛡️ DEF: ${currentDef}
            </div>
            ${level < 10 ? `
              <div style="margin-top: 12px; padding-top: 12px; border-top: 1px solid #e5e7eb;">
                <div style="font-size: 14px; color: #6b7280; margin-bottom: 8px;">強化後（Lv.${level + 1}）</div>
                <div style="font-size: 14px; color: #10b981;">
                  ⚔️ ATK: ${nextAtk} <span style="color: #059669;">(+${nextAtk - currentAtk})</span><br>
                  🛡️ DEF: ${nextDef} <span style="color: #059669;">(+${nextDef - currentDef})</span>
                </div>
              </div>
            ` : '<div style="margin-top: 12px; color: #f59e0b; font-weight: 700;">✨ 最大強化レベル</div>'}
          </div>
          
          <div style="display: flex; gap: 12px;">
            <button onclick="equipItemFromInventory(${index}, '${slot}'); this.parentElement.parentElement.parentElement.remove();" style="flex: 1; padding: 12px; background: #667eea; color: white; border: none; border-radius: 10px; font-size: 16px; font-weight: 700; cursor: pointer;">
              装備する
            </button>
            ${level < 10 ? `
              <button onclick="enhanceEquipment(${index}); this.parentElement.parentElement.parentElement.remove();" style="flex: 1; padding: 12px; background: #10b981; color: white; border: none; border-radius: 10px; font-size: 16px; font-weight: 700; cursor: pointer;">
                強化 (💰${enhanceCost})
              </button>
            ` : ''}
            <button onclick="this.parentElement.parentElement.parentElement.remove()" style="padding: 12px 20px; background: #e5e7eb; color: #374151; border: none; border-radius: 10px; font-size: 16px; font-weight: 700; cursor: pointer;">
              閉じる
            </button>
          </div>
        </div>
      `;
      
      document.body.appendChild(overlay);
    }
    
    function equipItemFromInventory(index, slot) {
      const item = playerData.inventory[index];
      if (!item) return;
      
      equipItem(item, slot);
      updateEquipmentDisplay();
      updateInventoryDisplay();
    }
    
    function enhanceEquipment(index) {
      const item = playerData.inventory[index];
      if (!item) return;
      
      const level = item.level || 0;
      if (level >= 10) {
        showMessage('❌ これ以上強化できません', 'error');
        return;
      }
      
      const cost = (level + 1) * 100;
      if (playerData.coins < cost) {
        showMessage('❌ コインが足りません（必要: ' + cost + '）', 'error');
        return;
      }
      
      playerData.coins -= cost;
      item.level = level + 1;
      
      saveRPGData();
      updateRPGDisplay();
      updateInventoryDisplay();
      updateEquipmentDisplay();
      
      showMessage('✨ ' + item.name + ' を Lv.' + item.level + ' に強化しました！', 'success');
    }
    
    function updateEquipmentDisplay() {
      const weaponEl = document.getElementById('equippedWeapon');
      const armorEl = document.getElementById('equippedArmor');
      const accessoryEl = document.getElementById('equippedAccessory');
      
      if (weaponEl) {
        if (playerData.equippedWeapon) {
          const level = playerData.equippedWeapon.level || 0;
          const atk = Math.floor(playerData.equippedWeapon.atk * getEnhancementBonus(level));
          weaponEl.innerHTML = playerData.equippedWeapon.name + (level > 0 ? ' +' + level : '') + '<br><span style="font-size: 12px; opacity: 0.8;">⚔️' + atk + '</span>';
        } else {
          weaponEl.textContent = 'なし';
        }
      }
      
      if (armorEl) {
        if (playerData.equippedArmor) {
          const level = playerData.equippedArmor.level || 0;
          const def = Math.floor(playerData.equippedArmor.def * getEnhancementBonus(level));
          armorEl.innerHTML = playerData.equippedArmor.name + (level > 0 ? ' +' + level : '') + '<br><span style="font-size: 12px; opacity: 0.8;">🛡️' + def + '</span>';
        } else {
          armorEl.textContent = 'なし';
        }
      }
      
      if (accessoryEl) {
        if (playerData.equippedAccessory) {
          const level = playerData.equippedAccessory.level || 0;
          const atk = Math.floor(playerData.equippedAccessory.atk * getEnhancementBonus(level));
          accessoryEl.innerHTML = playerData.equippedAccessory.name + (level > 0 ? ' +' + level : '') + '<br><span style="font-size: 12px; opacity: 0.8;">⚔️' + atk + '</span>';
        } else {
          accessoryEl.textContent = 'なし';
        }
      }
    }
    function canRollRoulette() {
      const today = new Date();
      const todayKey = toDateKeyLocal(today);
      return playerData.lastRouletteDate !== todayKey;
    }
    function calculateOfflineRewards() {
      const now = new Date();
      
      if (!playerData.lastLoginDate) {
        playerData.lastLoginDate = now.toISOString();
        return null;
      }
      
      const lastLogin = new Date(playerData.lastLoginDate);
      const hoursPassed = Math.min(24, (now - lastLogin) / (1000 * 60 * 60));
      
      if (hoursPassed < 1) return null;
      
      // 基本オフライン報酬（1時間あたり）
      const coinsPerHour = 50 + (playerData.level * 10);
      const xpPerHour = 20 + (playerData.level * 5);
      
      const rewards = {
        hours: Math.floor(hoursPassed),
        totalCoins: Math.floor(coinsPerHour * hoursPassed),
        totalXP: Math.floor(xpPerHour * hoursPassed)
      };
      
      playerData.lastLoginDate = now.toISOString();
      
      return rewards;
    }
    
    function showOfflineRewards(rewards) {
      if (!rewards || rewards.hours < 1) return;
      
      const overlay = document.createElement('div');
      overlay.style.cssText = `
        position: fixed; top: 0; left: 0; width: 100%; height: 100%;
        background: rgba(0,0,0,0.9); z-index: 10000;
        display: flex; align-items: center; justify-content: center;
        animation: fadeIn 0.5s;
      `;
      
      overlay.innerHTML = `
        <div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 40px; border-radius: 20px; text-align: center; color: white; max-width: 500px; animation: scaleIn 0.5s;">
          <div style="font-size: 32px; font-weight: 900; margin-bottom: 20px;">🎉 おかえりなさい！</div>
          <div style="font-size: 18px; margin-bottom: 30px;">${rewards.hours}時間ぶりのログインです</div>
          
          <div style="background: rgba(0,0,0,0.2); padding: 20px; border-radius: 12px; margin-bottom: 20px;">
            <div style="font-size: 20px; font-weight: 700; margin-bottom: 15px;">💰 オフライン報酬</div>
            <div style="font-size: 28px; font-weight: 900; margin: 10px 0;">💰 ${rewards.totalCoins} コイン</div>
            <div style="font-size: 28px; font-weight: 900; margin: 10px 0;">⭐ ${rewards.totalXP} XP</div>
          </div>
          
          <button onclick="this.parentElement.parentElement.remove(); applyOfflineRewards(${rewards.totalCoins}, ${rewards.totalXP})" style="padding: 15px 50px; background: white; color: #667eea; border: none; border-radius: 12px; font-size: 20px; font-weight: 700; cursor: pointer;">受け取る</button>
        </div>
      `;
      
      document.body.appendChild(overlay);
    }
    
    function applyOfflineRewards(coins, xp) {
      playerData.coins += coins;
      addXP(xp);
      saveRPGData();
      updateRPGDisplay();
    }
    function updateFocus() {
      const now = new Date();
      
      if (!playerData.lastFocusUpdate) {
        playerData.lastFocusUpdate = now.toISOString();
        playerData.focus = 5;
        return;
      }
      
      const lastUpdate = new Date(playerData.lastFocusUpdate);
      
      // 日をまたいだ場合は全回復
      if (toDateKeyLocal(now) !== toDateKeyLocal(lastUpdate)) {
        playerData.focus = 5;
        playerData.lastFocusUpdate = now.toISOString();
        return;
      }
      
      // 経過時間で回復（30分で1回復）
      const minutesPassed = Math.floor((now - lastUpdate) / (1000 * 60));
      const focusRecovered = Math.floor(minutesPassed / 30);
      
      if (focusRecovered > 0) {
        playerData.focus = Math.min(5, playerData.focus + focusRecovered);
        playerData.lastFocusUpdate = now.toISOString();
      }
    }
    
    function consumeFocus(minutes) {
      const focusUsed = Math.floor(minutes / 30);
      playerData.focus = Math.max(0, playerData.focus - focusUsed);
      playerData.lastFocusUpdate = new Date().toISOString();
    }
    
    function getFocusBonus() {
      return playerData.focus > 0 ? 1.5 : 1.0;
    }
    function calculateATK() {
      let levelMultiplier = 1.0;
      
      if (playerData.level <= 10) levelMultiplier = 1.0;
      else if (playerData.level <= 20) levelMultiplier = 1.2;
      else if (playerData.level <= 30) levelMultiplier = 1.5;
      else if (playerData.level <= 40) levelMultiplier = 1.8;
      else if (playerData.level <= 50) levelMultiplier = 2.0;
      else if (playerData.level <= 60) levelMultiplier = 2.3;
      else if (playerData.level <= 70) levelMultiplier = 2.6;
      else if (playerData.level <= 80) levelMultiplier = 3.0;
      else if (playerData.level <= 90) levelMultiplier = 3.5;
      else if (playerData.level <= 100) levelMultiplier = 4.0;
      else if (playerData.level <= 110) levelMultiplier = 4.5;
      else if (playerData.level <= 120) levelMultiplier = 5.0;
      else if (playerData.level <= 130) levelMultiplier = 6.0;
      else if (playerData.level <= 140) levelMultiplier = 7.0;
      else levelMultiplier = 8.0;
      
      const baseATK = Math.floor(100 * levelMultiplier);
      const equipBonus = getEquipmentBonus();
      const skillBonus = getSkillBonus();
      playerData.atk = baseATK + equipBonus.atk + skillBonus.atk;
      return playerData.atk;
    }
    
    function calculateDEF() {
      const baseDEF = playerData.level * 5;
      const equipBonus = getEquipmentBonus();
      const skillBonus = getSkillBonus();
      playerData.def = baseDEF + equipBonus.def + skillBonus.def;
      return playerData.def;
    }
    function updateRPGDisplay() {
      try {
        calculateATK();
        calculateDEF();
        updateFocus();
        
        const levelEl = document.getElementById('rpgLevel');
        const coinsEl = document.getElementById('rpgCoins');
        const gemsEl = document.getElementById('rpgGems');
        const streakEl = document.getElementById('rpgStreak');
        const xpBarEl = document.getElementById('rpgXpBar');
        const xpTextEl = document.getElementById('rpgXpText');
        
        if (levelEl) levelEl.textContent = playerData.level || 1;
        if (coinsEl) coinsEl.textContent = playerData.coins || 0;
        if (gemsEl) gemsEl.textContent = playerData.gems || 0;
        if (streakEl) streakEl.textContent = playerData.streak || 0;
        
        // ATK/DEF表示
        const atkEl = document.getElementById('rpgAtk');
        const defEl = document.getElementById('rpgDef');
        if (atkEl) atkEl.textContent = playerData.atk || 100;
        if (defEl) defEl.textContent = playerData.def || 5;
        
        // 集中力表示
        updateFocusDisplay();
        updateRouletteUI();
        
        const requiredXP = getRequiredXP(playerData.level || 1);
        const progress = ((playerData.xp || 0) / requiredXP * 100).toFixed(1);
        
        if (xpBarEl) xpBarEl.style.width = progress + '%';
        if (xpTextEl) xpTextEl.textContent = `${playerData.xp || 0} / ${requiredXP} XP`;
        
        updateCharacterSprite();
        updateGachaUI();
      } catch (error) {
        console.error('RPG表示エラー:', error);
      }
    }
    
    // 🎮 XP追加
    function addXP(amount) {
      const oldLevel = playerData.level;
      playerData.xp += amount;
      playerData.totalXP += amount;
      
      let leveledUp = false;
      while (playerData.xp >= getRequiredXP(playerData.level)) {
        playerData.xp -= getRequiredXP(playerData.level);
        playerData.level++;
        leveledUp = true;
      }
      
      if (leveledUp) {
        showLevelUpAnimation(oldLevel, playerData.level);
        playSound('level_up');
      }
      
      saveRPGData();
      updateRPGDisplay();
    }
    
    // 🎮 報酬計算
    function calculateRewards(minutes) {
      const baseXP = minutes * 1;
      const baseCoins = minutes * 1;
      let bonusXP = 0;
      let bonusCoins = 0;
      const bonuses = [];
      
      if (minutes >= 30) {
        bonusXP += 50;
        bonuses.push({ type: '30分達成', xp: 50 });
      }
      if (minutes >= 60) {
        bonusXP += 100;
        bonusCoins += 50;
        bonuses.push({ type: '1時間達成', xp: 100, coins: 50 });
      }
      if (minutes >= 120) {
        bonusXP += 200;
        bonusCoins += 100;
        playerData.gems += 1;
        bonuses.push({ type: 'クリティカル', xp: 200, coins: 100, gems: 1 });
      }
      if (minutes >= 180) {
        bonusXP += 500;
        bonusCoins += 300;
        playerData.gems += 2;
        bonuses.push({ type: '超クリティカル', xp: 500, coins: 300, gems: 2 });
      }
      
      const random = Math.random();
      if (random < 0.01) {
        const jackpot = 500 + Math.floor(Math.random() * 500);
        bonusXP += jackpot;
        bonusCoins += jackpot;
        playerData.gems += 5;
        bonuses.push({ type: '🌟 JACKPOT', xp: jackpot, coins: jackpot, gems: 5 });
        showJackpotAnimation();
      } else if (random < 0.05) {
        const ssr = 200 + Math.floor(Math.random() * 200);
        bonusXP += ssr;
        playerData.gems += 3;
        bonuses.push({ type: '✨ SSR', xp: ssr, gems: 3 });
      } else if (random < 0.15) {
        const rare = 100 + Math.floor(Math.random() * 100);
        bonusXP += rare;
        playerData.gems += 1;
        bonuses.push({ type: '💎 レア', xp: rare, gems: 1 });
      } else if (random < 0.35) {
        const lucky = 30 + Math.floor(Math.random() * 50);
        bonusXP += lucky;
        bonuses.push({ type: '🍀 ラッキー', xp: lucky });
      }
      
      if (playerData.streak >= 7) {
        const streakBonus = Math.floor(baseXP * 0.1);
        bonusXP += streakBonus;
        bonuses.push({ type: `🔥 ${playerData.streak}日連続`, xp: streakBonus });
      }
      
      // 集中力ボーナス
      const focusBonus = getFocusBonus();
      const focusMultiplier = focusBonus - 1.0;
      
      if (focusMultiplier > 0) {
        const focusBonusXP = Math.floor((baseXP + bonusXP) * focusMultiplier);
        bonusXP += focusBonusXP;
        bonuses.push({ type: '❤️ 集中力ボーナス', xp: focusBonusXP });
      }
      
      // コンボ倍率（ストリーク）
      let comboMultiplier = 1.0;
      if (playerData.streak >= 7) {
        comboMultiplier = 1.0 + Math.min(0.5, playerData.streak / 70 * 0.5);
        const comboBonusXP = Math.floor((baseXP + bonusXP) * (comboMultiplier - 1.0));
        if (comboBonusXP > 0) {
          bonusXP += comboBonusXP;
          bonuses.push({ type: `🔥 ${playerData.streak}日コンボ`, xp: comboBonusXP });
        }
      }
      
      return {
        baseXP,
        baseCoins,
        bonusXP,
        bonusCoins,
        totalXP: baseXP + bonusXP,
        totalCoins: baseCoins + bonusCoins,
        bonuses,
        focusBonus,
        comboMultiplier
      };
    }
    
    // 🎮 ストリーク更新
    function updateStreak(date) {
      const today = fromDateKeyLocal(date);
      const todayKey = toDateKeyLocal(today);
      
      if (playerData.lastStudyDate) {
        const lastDate = fromDateKeyLocal(playerData.lastStudyDate);
        const daysDiff = Math.floor((today - lastDate) / (1000 * 60 * 60 * 24));
        
        if (daysDiff === 1) {
          playerData.streak++;
          checkStreakBonus(playerData.streak);
        } else if (daysDiff === 0) {
          // 同日
        } else {
          if (playerData.streakProtection > 0) {
            playerData.streakProtection--;
            showMessage(`🛡️ ストリーク保護使用（残り${playerData.streakProtection}）`, 'info');
          } else {
            playerData.streak = 1;
          }
        }
      } else {
        playerData.streak = 1;
      }
      
      playerData.lastStudyDate = todayKey;
      saveRPGData();
      updateRPGDisplay();
    }
    
    // 🎮 ストリークボーナス
    function checkStreakBonus(streak) {
      let bonus = null;
      if (streak === 7) {
        bonus = { xp: 500, coins: 500, gems: 1, message: '🔥 1週間連続' };
      } else if (streak === 30) {
        bonus = { xp: 3000, coins: 3000, gems: 10, message: '🔥🔥 1ヶ月連続' };
        playerData.streakProtection++;
      } else if (streak === 100) {
        bonus = { xp: 10000, coins: 10000, gems: 50, message: '👑 100日連続' };
        playerData.streakProtection += 5;
      }
      
      if (bonus) {
        playerData.coins += bonus.coins;
        if (bonus.gems) playerData.gems += bonus.gems;
        addXP(bonus.xp);
        showStreakBonusAnimation(bonus);
      }
    }

    // 🎮 アニメーション
    function showLevelUpAnimation(oldLevel, newLevel) {
      const message = document.getElementById('levelUpMessage');
      if (message) {
        message.style.display = 'block';
        message.innerHTML = `✨ レベルアップ！✨<br>Lv.${oldLevel} → Lv.${newLevel}`;
        setTimeout(() => { message.style.display = 'none'; }, 3000);
      }
    }
    
    function showJackpotAnimation() {
      const overlay = document.createElement('div');
      overlay.style.cssText = `
        position: fixed; top: 0; left: 0; width: 100%; height: 100%;
        background: linear-gradient(135deg, #fbbf24 0%, #f59e0b 100%);
        z-index: 10000; display: flex; align-items: center; justify-content: center;
        animation: fadeIn 0.3s;
      `;
      overlay.innerHTML = `
        <div style="text-align: center; color: white; animation: scaleIn 0.5s;">
          <div style="font-size: 80px; margin-bottom: 20px; animation: spin 2s linear infinite;">🌟</div>
          <div style="font-size: 64px; font-weight: 900;">JACKPOT!</div>
        </div>
      `;
      document.body.appendChild(overlay);
      setTimeout(() => overlay.remove(), 2000);
    }
    
    function showStreakBonusAnimation(bonus) {
      const overlay = document.createElement('div');
      overlay.style.cssText = `
        position: fixed; top: 0; left: 0; width: 100%; height: 100%;
        background: rgba(0,0,0,0.8); z-index: 10000;
        display: flex; align-items: center; justify-content: center;
        animation: fadeIn 0.3s;
      `;
      overlay.innerHTML = `
        <div style="background: linear-gradient(135deg, #f59e0b 0%, #d97706 100%); padding: 40px; border-radius: 20px; text-align: center; color: white; animation: scaleIn 0.5s; max-width: 500px;">
          <div style="font-size: 48px; margin-bottom: 20px;">🎉</div>
          <div style="font-size: 32px; font-weight: 900; margin-bottom: 20px;">${bonus.message}</div>
          <div style="font-size: 24px; margin-bottom: 10px;">報酬:</div>
          <div style="font-size: 20px; font-weight: 700;">
            ⭐ +${bonus.xp} XP<br>
            💰 +${bonus.coins} コイン
            ${bonus.gems ? `<br>💎 +${bonus.gems} ジェム` : ''}
          </div>
          <button onclick="this.parentElement.parentElement.remove()" style="margin-top: 30px; padding: 15px 40px; background: white; color: #d97706; border: none; border-radius: 12px; font-size: 18px; font-weight: 700; cursor: pointer;">受け取る</button>
        </div>
      `;
      document.body.appendChild(overlay);
    }
    
    function updateCharacterSprite() {
      const sprite = document.getElementById('characterSprite');
      if (!sprite) return;
      const level = playerData.level;
      sprite.textContent = level >= 100 ? '💎' : level >= 80 ? '👑' : level >= 60 ? '🦸' : level >= 40 ? '🧙' : level >= 20 ? '⚔️' : level >= 10 ? '🛡️' : '🧒';
    }
    
    function playSound(type) {
      // サウンド省略（Web Audio API）
    }

    // 🎲 デイリーガチャ
    const gachaItems = [
      { name: 'XPブースト', rarity: 'N', effect: 'xp', value: 100 },
      { name: 'コインパック', rarity: 'N', effect: 'coins', value: 100 },
      { name: 'XPブーストLV2', rarity: 'R', effect: 'xp', value: 300 },
      { name: 'メガXPブースト', rarity: 'SSR', effect: 'xp', value: 1000 },
      { name: '伝説のXPブースト', rarity: 'UR', effect: 'xp', value: 5000 }
    ];
    
    function rollDailyGacha() {
      const today = new Date();
      const todayKey = toDateKeyLocal(today);
      
      if (playerData.lastGachaDate === todayKey) {
        showMessage('❌ 今日のガチャは既に引きました', 'error');
        return;
      }
      
      if (playerData.coins < 100) {
        showMessage('❌ コインが足りません（必要: 100）', 'error');
        return;
      }
      
      playerData.coins -= 100;
      
      const random = Math.random();
      let result;
      
      // 50%で装備、50%でアイテム
      if (Math.random() < 0.5) {
        // 装備ガチャ
        result = rollEquipmentGacha(random);
      } else {
        // アイテムガチャ（従来）
        let item;
        if (random < 0.01) item = gachaItems[4]; // UR 1%
        else if (random < 0.10) item = gachaItems[3]; // SSR 9%
        else if (random < 0.40) item = gachaItems[2]; // R 30%
        else item = gachaItems[Math.floor(Math.random() * 2)]; // N 60%
        
        if (item.effect === 'xp') addXP(item.value);
        else if (item.effect === 'coins') playerData.coins += item.value;
        
        result = { type: 'item', ...item };
      }
      
      playerData.lastGachaDate = todayKey;
      saveRPGData();
      updateRPGDisplay();
      
      updateQuestProgress('gacha1', 1);
      
      showGachaResult(result);
    }
    

    
    function rollEquipmentGacha(random) {
      let equipment;
      let rarity;
      
      if (random < 0.01) {
        rarity = 'Legendary';
        const legendaries = [
          ...equipmentDatabase.weapons.filter(e => e.rarity === 'Legendary'),
          ...equipmentDatabase.armors.filter(e => e.rarity === 'Legendary'),
          ...equipmentDatabase.accessories.filter(e => e.rarity === 'Legendary')
        ];
        equipment = legendaries[Math.floor(Math.random() * legendaries.length)];
      } else if (random < 0.10) {
        rarity = 'Epic';
        const epics = [
          ...equipmentDatabase.weapons.filter(e => e.rarity === 'Epic'),
          ...equipmentDatabase.armors.filter(e => e.rarity === 'Epic'),
          ...equipmentDatabase.accessories.filter(e => e.rarity === 'Epic')
        ];
        equipment = epics[Math.floor(Math.random() * epics.length)];
      } else if (random < 0.40) {
        rarity = 'Rare';
        const rares = [
          ...equipmentDatabase.weapons.filter(e => e.rarity === 'Rare'),
          ...equipmentDatabase.armors.filter(e => e.rarity === 'Rare'),
          ...equipmentDatabase.accessories.filter(e => e.rarity === 'Rare')
        ];
        equipment = rares[Math.floor(Math.random() * rares.length)];
      } else {
        rarity = 'Common';
        const commons = [
          ...equipmentDatabase.weapons.filter(e => e.rarity === 'Common'),
          ...equipmentDatabase.armors.filter(e => e.rarity === 'Common'),
          ...equipmentDatabase.accessories.filter(e => e.rarity === 'Common')
        ];
        equipment = commons[Math.floor(Math.random() * commons.length)];
      }
      
      // インベントリに追加
      if (!playerData.inventory) playerData.inventory = [];
      playerData.inventory.push({
        ...equipment,
        level: 0,
        obtainedDate: toDateKeyLocal(new Date())
      });
      
      return { type: 'equipment', ...equipment };
    }
    function showGachaResult(result) {
      const bgColors = { 'Common': '#64748b', 'Rare': '#3b82f6', 'Epic': '#a855f7', 'Legendary': '#f59e0b', 'N': '#64748b', 'R': '#3b82f6', 'SSR': '#a855f7', 'UR': '#f59e0b' };
      const overlay = document.createElement('div');
      overlay.style.cssText = 'position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.9); z-index: 10000; display: flex; align-items: center; justify-content: center; animation: fadeIn 0.3s;';
      
      let contentHTML;
      if (result.type === 'equipment') {
        const slotIcon = result.id.startsWith('w') ? '🗡️' : result.id.startsWith('a') ? '🛡️' : '💍';
        contentHTML = `
          <div style="background: ${bgColors[result.rarity]}; padding: 50px; border-radius: 20px; text-align: center; color: white; animation: scaleIn 0.5s; max-width: 500px;">
            <div style="font-size: 24px; font-weight: 700; margin-bottom: 10px;">${result.rarity}</div>
            <div style="font-size: 48px; margin-bottom: 10px;">${slotIcon}</div>
            <div style="font-size: 36px; font-weight: 900; margin-bottom: 20px;">${result.name}</div>
            <div style="font-size: 20px; font-weight: 700;">
              ⚔️ ATK +${result.atk}<br>
              🛡️ DEF +${result.def}
            </div>
            <button onclick="this.parentElement.parentElement.remove()" style="margin-top: 30px; padding: 15px 50px; background: white; color: ${bgColors[result.rarity]}; border: none; border-radius: 12px; font-size: 20px; font-weight: 700; cursor: pointer;">インベントリへ</button>
          </div>
        `;
      } else {
        contentHTML = `
          <div style="background: ${bgColors[result.rarity]}; padding: 50px; border-radius: 20px; text-align: center; color: white; animation: scaleIn 0.5s; max-width: 500px;">
            <div style="font-size: 24px; font-weight: 700; margin-bottom: 10px;">${result.rarity}</div>
            <div style="font-size: 36px; font-weight: 900; margin-bottom: 30px;">${result.name}</div>
            <div style="font-size: 28px; font-weight: 800;">
              ${result.effect === 'xp' ? `⭐ +${result.value} XP` : `💰 +${result.value} コイン`}
            </div>
            <button onclick="this.parentElement.parentElement.remove()" style="margin-top: 30px; padding: 15px 50px; background: white; color: ${bgColors[result.rarity]}; border: none; border-radius: 12px; font-size: 20px; font-weight: 700; cursor: pointer;">閉じる</button>
          </div>
        `;
      }
      
      overlay.innerHTML = contentHTML;
      document.body.appendChild(overlay);
    }
    
    function updateGachaUI() {
      const today = new Date();
      const todayKey = toDateKeyLocal(today);
      const gachaBtn = document.getElementById('gachaBtn');
      const gachaAvailable = document.getElementById('gachaAvailable');
      const gachaTimer = document.getElementById('gachaTimer');
      
      if (!gachaBtn) return;
      if (!playerData.inventory) playerData.inventory = [];
      
      if (playerData.lastGachaDate === todayKey) {
        gachaBtn.disabled = true;
        gachaBtn.style.opacity = '0.5';
        gachaBtn.style.cursor = 'not-allowed';
        if (gachaAvailable) gachaAvailable.textContent = '0回';
        if (gachaTimer) gachaTimer.textContent = '明日また来てね';
      } else {
        gachaBtn.disabled = false;
        gachaBtn.style.opacity = '1';
        gachaBtn.style.cursor = 'pointer';
        if (gachaAvailable) gachaAvailable.textContent = '1回';
        if (gachaTimer) gachaTimer.textContent = '利用可能！';
      }
    }
    
    // 🏆 実績システム
    const achievements = [
      { id: 'study_1h', name: '初学者', desc: '1時間達成', icon: '⭐', check: (m) => m >= 60 },
      { id: 'study_10h', name: '継続者', desc: '10時間達成', icon: '⭐', check: (m) => m >= 600 },
      { id: 'study_50h', name: '努力家', desc: '50時間達成', icon: '🏅', check: (m) => m >= 3000 },
      { id: 'study_100h', name: '勤勉家', desc: '100時間達成', icon: '🏅', check: (m) => m >= 6000 },
      { id: 'study_500h', name: 'マスター', desc: '500時間達成', icon: '💎', check: (m) => m >= 30000 },
      { id: 'study_1000h', name: '伝説', desc: '1000時間達成', icon: '👑', check: (m) => m >= 60000 },
      { id: 'streak_7', name: '1週間連続', desc: '7日連続学習', icon: '🔥', check: (m) => playerData.streak >= 7 },
      { id: 'streak_30', name: '1ヶ月連続', desc: '30日連続学習', icon: '🔥', check: (m) => playerData.streak >= 30 },
      { id: 'level_10', name: 'レベル10', desc: 'Lv10到達', icon: '📈', check: (m) => playerData.level >= 10 },
      { id: 'level_50', name: 'レベル50', desc: 'Lv50到達', icon: '📈', check: (m) => playerData.level >= 50 },
      { id: 'level_100', name: 'レベル100', desc: 'Lv100到達', icon: '📈', check: (m) => playerData.level >= 100 }
    ];
    
    function checkAchievements() {
      if (!playerData.achievements) playerData.achievements = [];
      const totalMinutes = studyRecords.reduce((sum, r) => sum + r.minutes, 0);
      achievements.forEach(achievement => {
        if (!playerData.achievements.includes(achievement.id) && achievement.check(totalMinutes)) {
          playerData.achievements.push(achievement.id);
          saveRPGData();
          showAchievementUnlock(achievement);
        }
      });
    }
    
    function showAchievementUnlock(achievement) {
      const notification = document.createElement('div');
      notification.style.cssText = `
        position: fixed; top: 80px; right: 20px; background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        color: white; padding: 20px; border-radius: 16px; box-shadow: 0 10px 30px rgba(0,0,0,0.3);
        z-index: 10000; animation: slideInRight 0.5s, slideOutRight 0.5s 3s; min-width: 300px;
      `;
      notification.innerHTML = `
        <div style="font-size: 14px; opacity: 0.9; margin-bottom: 8px;">🏆 実績解除！</div>
        <div style="display: flex; align-items: center; gap: 12px;">
          <div style="font-size: 36px;">${achievement.icon}</div>
          <div>
            <div style="font-size: 20px; font-weight: 900;">${achievement.name}</div>
            <div style="font-size: 14px; opacity: 0.9;">${achievement.desc}</div>
          </div>
        </div>
      `;
      document.body.appendChild(notification);
      setTimeout(() => notification.remove(), 3500);
    }
    
    // ⚔️ ボス戦システム
    function updateBossBattle() {
      const subject = document.getElementById('subject').value;
      if (!subject || !subjectBosses[subject]) return;
      if (!playerData.defeatedBosses) playerData.defeatedBosses = [];
      
      const boss = subjectBosses[subject];
      const subjectMinutes = studyRecords.filter(r => r.subject === subject).reduce((sum, r) => sum + r.minutes, 0);
      const subjectHours = subjectMinutes / 60;
      const currentHP = Math.max(0, boss.hp - subjectHours);
      
      if (currentHP <= 0 && !playerData.defeatedBosses.includes(subject)) {
        playerData.defeatedBosses.push(subject);
        saveRPGData();
        showBossDefeatAnimation(boss);
      }
    }
    
    function showBossDefeatAnimation(boss) {
      const rewards = { xp: boss.hp * 100, coins: boss.hp * 50, gems: boss.difficulty * 3 };
      playerData.coins += rewards.coins;
      playerData.gems += rewards.gems;
      addXP(rewards.xp);
      
      const overlay = document.createElement('div');
      overlay.style.cssText = `position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.9); z-index: 10000; display: flex; align-items: center; justify-content: center; animation: fadeIn 0.5s;`;
      overlay.innerHTML = `
        <div style="background: linear-gradient(135deg, #10b981 0%, #059669 100%); padding: 50px; border-radius: 20px; text-align: center; color: white; animation: scaleIn 0.5s; max-width: 600px;">
          <div style="font-size: 80px; margin-bottom: 20px;">${boss.icon}</div>
          <div style="font-size: 48px; font-weight: 900; margin-bottom: 20px;">ボス撃破！</div>
          <div style="font-size: 28px; margin-bottom: 30px;">${boss.name}</div>
          <div style="font-size: 20px; font-weight: 700; line-height: 1.8;">
            ⭐ +${rewards.xp} XP<br>💰 +${rewards.coins} コイン<br>💎 +${rewards.gems} ジェム
          </div>
          <button onclick="this.parentElement.parentElement.remove()" style="margin-top: 30px; padding: 15px 50px; background: white; color: #059669; border: none; border-radius: 12px; font-size: 20px; font-weight: 700; cursor: pointer;">やったぜ！</button>
        </div>
      `;
      document.body.appendChild(overlay);
    }



    
    function saveRecords() {
      localStorage.setItem('studyRecords', JSON.stringify(studyRecords));
    }
    
    function loadSheetData() {
      const saved = localStorage.getItem('sheetData');
      if (saved) {
        sheetData = JSON.parse(saved);
      }
    }
    
    function saveSheetData() {
      localStorage.setItem('sheetData', JSON.stringify(sheetData));
    }
    
    // 日付関連
    function updateDateDisplay() {
      const today = new Date();
      const dateStr = today.getFullYear() + '/' + 
                      String(today.getMonth() + 1).padStart(2, '0') + '/' + 
                      String(today.getDate()).padStart(2, '0');
      const weekdays = ['日', '月', '火', '水', '木', '金', '土'];
      const weekday = weekdays[today.getDay()];
      
      document.getElementById('dateDisplay').textContent = `📅 ${dateStr} (${weekday})`;
      
      const todayISO = toDateKeyLocal(today);
      document.getElementById('recordDate').value = todayISO;
    }
    
    function getShiftType(date) {
      const shiftStart = new Date('2026-02-06T00:00:00');
      const restStart = new Date('2026-02-10T00:00:00');
      const lateStart = new Date('2026-02-12T00:00:00');
      
      if (date < shiftStart) return 'late';
      if (date < restStart) return 'early';
      if (date < lateStart) return 'rest';
      
      const daysSince = Math.floor((date - lateStart) / (1000 * 60 * 60 * 24));
      const position = daysSince % 12;
      
      if (position < 4) return 'late';
      if (position < 6) return 'rest';
      if (position < 10) return 'early';
      return 'rest';
    }
    
    // タブ切り替え
    function switchTab(tab, btn) {
      document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));
      if (btn) btn.classList.add('active');
      
      document.querySelectorAll('.tab-content').forEach(content => content.classList.remove('active'));
      const target = document.getElementById(tab + '-tab');
      if (target) target.classList.add('active');
      
      if (tab === 'stats') updateAdvancedStats();
      if (tab === 'sheet') updateSheetSums();
      if (tab === 'equipment') { updateEquipmentDisplay(); updateInventoryDisplay(); }
      if (tab === 'skills') updateSkillTreeDisplay();
      if (tab === 'shop') { updateShopDisplay(); updateShopCoins(); }
      if (tab === 'analytics') updateAnalyticsDisplay();
      if (tab === 'log') updateLogTable();
    }
    
    // タイマー機能
    function startTimer() {
      const subject = document.getElementById('subject').value;
      if (!subject) {
        showMessage('科目を選択してください', 'error');
        return;
      }
      
      if (isPaused) {
        startTime = Date.now() - pausedTime;
        isPaused = false;
      } else {
        startTime = Date.now() - elapsedTime;
      }
      
      timerInterval = setInterval(updateTimer, 100);
      
      document.getElementById('startBtn').disabled = true;
      document.getElementById('stopBtn').disabled = false;
      document.getElementById('subject').disabled = true;
      hideMessage();
    }
    
    function stopTimer() {
      clearInterval(timerInterval);
      pausedTime = Date.now() - startTime;
      isPaused = true;
      
      document.getElementById('startBtn').disabled = false;
      document.getElementById('stopBtn').disabled = true;
      
      updateElapsedInfo();
    }
    
    function resetTimer() {
      clearInterval(timerInterval);
      startTime = null;
      elapsedTime = 0;
      pausedTime = 0;
      isPaused = false;
      
      document.getElementById('timer').textContent = '00:00:00';
      document.getElementById('startBtn').disabled = false;
      document.getElementById('stopBtn').disabled = true;
      document.getElementById('subject').disabled = false;
      document.getElementById('elapsedInfo').textContent = '';
      hideMessage();
    }
    
    function updateTimer() {
      elapsedTime = Date.now() - startTime;
      
      const hours = Math.floor(elapsedTime / 3600000);
      const minutes = Math.floor((elapsedTime % 3600000) / 60000);
      const seconds = Math.floor((elapsedTime % 60000) / 1000);
      
      document.getElementById('timer').textContent = 
        pad(hours) + ':' + pad(minutes) + ':' + pad(seconds);
    }
    
    function updateElapsedInfo() {
      const minutes = Math.floor(elapsedTime / 60000);
      const hours = (minutes / 60).toFixed(2);
      document.getElementById('elapsedInfo').textContent = 
        `経過時間: ${minutes}分 (${hours}時間)`;
    }
    
    function pad(num) {
      return num.toString().padStart(2, '0');
    }
    
    function recordTime() {
      const subject = document.getElementById('subject').value;
      
      if (!subject) {
        showMessage('科目を選択してください', 'error');
        return;
      }
      
      const manualTimeInput = document.getElementById('manualTime').value.trim();
      let minutes = 0;
      
      if (manualTimeInput) {
        minutes = parseManualTime(manualTimeInput);
        if (minutes === 0) {
          showMessage('正しい時間形式で入力してください (例: 1:30)', 'error');
          return;
        }
      } else {
        minutes = Math.floor(elapsedTime / 60000);
        if (minutes === 0) {
          showMessage('1分以上の学習時間を記録してください、または時間を直接入力してください', 'error');
          return;
        }
      }
      
      const selectedDate = document.getElementById('recordDate').value;
      if (!selectedDate) {
        showMessage('日付を選択してください', 'error');
        return;
      }
      
      const recordDate = new Date(selectedDate + 'T00:00:00');
      const now = new Date();
      const dateKey = selectedDate;
      
      const record = {
        date: dateKey,
        time: now.toTimeString().split(' ')[0].substring(0, 5),
        subject: subject,
        minutes: minutes,
        weekday: ['日', '月', '火', '水', '木', '金', '土'][recordDate.getDay()]
      };
      
      studyRecords.push(record);
      saveRecords();
      
      if (!sheetData[dateKey]) sheetData[dateKey] = {};
      if (!sheetData[dateKey][subject]) sheetData[dateKey][subject] = 0;
      sheetData[dateKey][subject] += minutes;
      saveSheetData();
      updateSheetRow(dateKey);
      updateSheetSums();
      updateLogTable();
      
      const source = manualTimeInput ? '(手動入力)' : '';
      
      // 🎮 RPG報酬処理
      const rewards = calculateRewards(minutes);
      playerData.coins += rewards.totalCoins;
      addXP(rewards.totalXP);
      updateStreak(dateKey);
      checkAchievements();
      updateBossBattle();
      consumeFocus(minutes);
      
      // 宝箱ドロップ判定
      const chestType = rollChestDrop(minutes);
      if (chestType) {
        addChest(chestType);
      }
      
      // クエスト進捗更新
      if (minutes >= 30) updateQuestProgress('study30', minutes);
      if (minutes >= 60) updateQuestProgress('study60', minutes);
      
      // 報酬メッセージ作成
      let rewardMsg = `✅ ${dateKey} ${subject} ${minutes}分を記録！

🎮 報酬:
⭐ +${rewards.totalXP} XP
💰 +${rewards.totalCoins} コイン

📊 倍率:
${rewards.focusBonus > 1 ? `❤️ 集中力: ×${rewards.focusBonus.toFixed(1)}
` : ''}${rewards.comboMultiplier > 1 ? `🔥 コンボ: ×${rewards.comboMultiplier.toFixed(2)}
` : ''}`;
      
      if (rewards.bonuses.length > 0) {
        rewardMsg += `
🎁 ボーナス:
`;
        rewards.bonuses.forEach(b => {
          rewardMsg += `• ${b.type}`;
          if (b.xp) rewardMsg += ` (+${b.xp} XP)`;
          if (b.coins) rewardMsg += ` (+${b.coins} コイン)`;
          if (b.gems) rewardMsg += ` (+${b.gems} 💎)`;
          rewardMsg += `
`;
        });
      }
      
      showMessage(rewardMsg + `
${source}`, 'success');
      setTimeout(() => {
        resetTimer();
        document.getElementById('manualTime').value = '';
      }, 2000);
    }
    
    function parseManualTime(input) {
      const parts = input.split(':');
      if (parts.length !== 2) return 0;
      
      const hours = parseInt(parts[0]);
      const mins = parseInt(parts[1]);
      
      if (isNaN(hours) || isNaN(mins) || hours < 0 || mins < 0 || mins >= 60) {
        return 0;
      }
      
      return hours * 60 + mins;
    }
    
    // スプレッドシート機能
    function generateSheetRows() {
      const tbody = document.getElementById('sheetBody');
      if (!tbody) return;
      
      const today = new Date();
      today.setHours(0, 0, 0, 0);
      const todayKey = toDateKeyLocal(today);
      const startDate = new Date('2026-02-04T00:00:00');
      const examDate = new Date('2026-08-23T00:00:00');
      const weekdays = ['日', '月', '火', '水', '木', '金', '土'];
      
      let html = '';
      let currentDate = new Date(startDate);
      
      // 全ての日付を生成（開始日から試験日まで）
      while (currentDate <= examDate) {
        const dateKey = toDateKeyLocal(currentDate);
        const weekday = weekdays[currentDate.getDay()];
        const remaining = Math.floor((examDate - currentDate) / (1000 * 60 * 60 * 24));
        const shiftType = getShiftType(currentDate);
        const shiftClass = `shift-${shiftType}`;
        
        let weekdayClass = '';
        if (weekday === '土') weekdayClass = 'weekday-sat';
        if (weekday === '日') weekdayClass = 'weekday-sun';
        
        // 今日の行を強調
        const isToday = dateKey === todayKey;
        
        html += `<tr data-date="${dateKey}" id="row-${dateKey}">`;
        html += `<td class="date-cell ${shiftClass}" style="${isToday ? 'background: rgba(255, 215, 0, 0.3) !important; font-weight: 900; border-left: 5px solid #f59e0b;' : ''}">${dateKey}${isToday ? ' 📍今日' : ''}</td>`;
        html += `<td class="${shiftClass} ${weekdayClass}" style="${isToday ? 'background: rgba(255, 215, 0, 0.3) !important; font-weight: 900;' : ''}">${weekday}</td>`;
        html += `<td class="${shiftClass}" style="${isToday ? 'background: rgba(255, 215, 0, 0.3) !important; font-weight: 900;' : ''}">${remaining}</td>`;
        
        subjects.forEach((subject, idx) => {
          const value = sheetData[dateKey]?.[subject] || '';
          const timeStr = value ? minutesToTimeString(value) : '';
          const hasValueClass = value ? 'has-value' : '';
          html += `<td class="${shiftClass}" style="${isToday ? 'background: rgba(255, 215, 0, 0.2) !important;' : ''}"><input type="text" class="${hasValueClass}" data-date="${dateKey}" data-subject="${subject}" value="${timeStr}" onchange="updateSheetCell(this)" placeholder="00:00"></td>`;
        });
        
        html += `<td class="total-cell ${shiftClass}" id="total-${dateKey}" style="${isToday ? 'background: rgba(255, 215, 0, 0.3) !important; font-weight: 900;' : ''}">00:00</td>`;
        html += `</tr>`;
        
        currentDate.setDate(currentDate.getDate() + 1);
      }
      
      tbody.innerHTML = html;
      updateAllTotals();
      
      // スクロール位置を当日の行に調整
      setTimeout(() => {
        const todayRow = document.getElementById(`row-${todayKey}`);
        if (todayRow) {
          todayRow.scrollIntoView({ behavior: 'smooth', block: 'start' });
        }
      }, 100);
    }
    
    function updateSheetCell(input) {
      const date = input.dataset.date;
      const subject = input.dataset.subject;
      const value = input.value.trim();
      
      if (!value) {
        input.classList.remove('has-value');
        if (sheetData[date]) delete sheetData[date][subject];
        saveSheetData();
        updateSheetRow(date);
        updateSheetSums();
        return;
      }
      
      const minutes = timeStringToMinutes(value);
      if (minutes > 0) {
        if (!sheetData[date]) sheetData[date] = {};
        sheetData[date][subject] = minutes;
        input.value = minutesToTimeString(minutes);
        input.classList.add('has-value');
        saveSheetData();
        updateSheetRow(date);
        updateSheetSums();
      } else {
        input.value = '';
        input.classList.remove('has-value');
      }
    }
    
    function updateSheetRow(dateKey) {
      const row = document.querySelector(`tr[data-date="${dateKey}"]`);
      if (!row) return;
      
      subjects.forEach(subject => {
        const input = row.querySelector(`input[data-subject="${subject}"]`);
        const value = sheetData[dateKey]?.[subject] || 0;
        input.value = value > 0 ? minutesToTimeString(value) : '';
        if (value > 0) {
          input.classList.add('has-value');
        } else {
          input.classList.remove('has-value');
        }
      });
      
      updateRowTotal(dateKey);
    }
    
    function updateRowTotal(dateKey) {
      let total = 0;
      subjects.forEach(subject => {
        total += sheetData[dateKey]?.[subject] || 0;
      });
      
      const cell = document.getElementById(`total-${dateKey}`);
      if (cell) cell.textContent = minutesToTimeString(total);
    }
    
    function updateAllTotals() {
      const dates = Object.keys(sheetData);
      dates.forEach(date => updateRowTotal(date));
    }
    
    function updateSheetSums() {
      const sums = new Array(11).fill(0);
      let grandTotal = 0;
      
      Object.values(sheetData).forEach(dayData => {
        subjects.forEach((subject, idx) => {
          const value = dayData[subject] || 0;
          sums[idx] += value;
          grandTotal += value;
        });
      });
      
      subjects.forEach((subject, idx) => {
        const cell = document.getElementById(`sum-${idx}`);
        if (cell) cell.textContent = minutesToTimeString(sums[idx]);
      });
      
      const totalCell = document.getElementById('sum-total');
      if (totalCell) totalCell.textContent = minutesToTimeString(grandTotal);
      
      const totalDisplay = document.getElementById('sheetTotalTime');
      if (totalDisplay) totalDisplay.textContent = minutesToTimeString(grandTotal);
    }
    
    function syncFromTimer() {
      const tempData = {};
      studyRecords.forEach(record => {
        if (!tempData[record.date]) tempData[record.date] = {};
        if (!tempData[record.date][record.subject]) tempData[record.date][record.subject] = 0;
        tempData[record.date][record.subject] += record.minutes;
      });
      
      sheetData = tempData;
      saveSheetData();
      
      document.querySelectorAll('#sheetBody input').forEach(input => {
        const date = input.dataset.date;
        const subject = input.dataset.subject;
        const value = sheetData[date]?.[subject] || 0;
        input.value = value > 0 ? minutesToTimeString(value) : '';
        if (value > 0) {
          input.classList.add('has-value');
        } else {
          input.classList.remove('has-value');
        }
      });
      
      updateAllTotals();
      updateSheetSums();
      
      alert('✅ タイマーの記録をシートに同期しました');
    }
    
    // 記録ログ機能
    function updateLogTable() {
      const tbody = document.getElementById('logTableBody');
      
      if (studyRecords.length === 0) {
        tbody.innerHTML = '<tr><td colspan="5" style="text-align: center; padding: 40px; color: #9ca3af;">まだ記録がありません</td></tr>';
        return;
      }
      
      const sorted = [...studyRecords].reverse();
      let html = '';
      
      sorted.forEach((record, index) => {
        const actualIndex = studyRecords.length - 1 - index;
        html += `
          <tr>
            <td>${record.date}</td>
            <td>${record.time}</td>
            <td style="font-weight: bold;">${record.subject}</td>
            <td style="font-family: 'Courier New', monospace; color: #667eea; font-weight: bold;">${minutesToTimeString(record.minutes)}</td>
            <td>
              <div class="log-actions">
                <button class="edit-btn" onclick="openEditModal(${actualIndex})">編集</button>
                <button class="delete-btn" onclick="deleteRecord(${actualIndex})">削除</button>
              </div>
            </td>
          </tr>
        `;
      });
      
      tbody.innerHTML = html;
    }
    
    function openEditModal(index) {
      editingIndex = index;
      const record = studyRecords[index];
      
      document.getElementById('editDate').value = record.date;
      document.getElementById('editSubject').value = record.subject;
      document.getElementById('editTime').value = minutesToTimeString(record.minutes);
      
      document.getElementById('editModal').classList.add('active');
    }
    
    function closeEditModal() {
      document.getElementById('editModal').classList.remove('active');
      editingIndex = -1;
    }
    
    function saveEdit() {
      if (editingIndex === -1) return;
      
      const oldRecord = studyRecords[editingIndex];
      const newDate = document.getElementById('editDate').value;
      const newSubject = document.getElementById('editSubject').value;
      const newTime = document.getElementById('editTime').value;
      const newMinutes = timeStringToMinutes(newTime);
      
      if (newMinutes === 0) {
        alert('正しい時間形式で入力してください');
        return;
      }
      
      // シートデータから古い記録を削除
      if (sheetData[oldRecord.date] && sheetData[oldRecord.date][oldRecord.subject]) {
        sheetData[oldRecord.date][oldRecord.subject] -= oldRecord.minutes;
        if (sheetData[oldRecord.date][oldRecord.subject] <= 0) {
          delete sheetData[oldRecord.date][oldRecord.subject];
        }
      }
      
      // 🎮 RPG: XP差分を計算
      const oldRewards = calculateRewards(oldRecord.minutes);
      const newRewards = calculateRewards(newMinutes);
      const xpDiff = newRewards.totalXP - oldRewards.totalXP;
      const coinDiff = newRewards.totalCoins - oldRewards.totalCoins;
      
      playerData.xp += xpDiff;
      playerData.totalXP += xpDiff;
      playerData.coins += coinDiff;
      
      // レベル調整
      if (xpDiff < 0) {
        while (playerData.level > 1 && playerData.xp < 0) {
          playerData.level--;
          const requiredXP = getRequiredXP(playerData.level);
          playerData.xp += requiredXP;
        }
        if (playerData.xp < 0) playerData.xp = 0;
      } else {
        while (playerData.xp >= getRequiredXP(playerData.level)) {
          playerData.xp -= getRequiredXP(playerData.level);
          playerData.level++;
        }
      }
      
      if (playerData.coins < 0) playerData.coins = 0;
      
      saveRPGData();
      updateRPGDisplay();
      
      // 記録を更新
      studyRecords[editingIndex] = {
        date: newDate,
        time: oldRecord.time,
        subject: newSubject,
        minutes: newMinutes,
        weekday: ['日', '月', '火', '水', '木', '金', '土'][fromDateKeyLocal(newDate).getDay()]
      };
      
      // シートデータに新しい記録を追加
      if (!sheetData[newDate]) sheetData[newDate] = {};
      if (!sheetData[newDate][newSubject]) sheetData[newDate][newSubject] = 0;
      sheetData[newDate][newSubject] += newMinutes;
      
      saveRecords();
      saveSheetData();
      updateSheetRow(oldRecord.date);
      updateSheetRow(newDate);
      updateSheetSums();
      updateLogTable();
      closeEditModal();
      
      alert('✅ 記録を更新しました（XP/コインも調整されました）');
    }
    
    function deleteRecord(index) {
      if (!confirm('この記録を削除しますか？')) return;
      
      const record = studyRecords[index];
      
      // 🎮 RPG: 削除した分のXPを減らす
      const lostRewards = calculateRewards(record.minutes);
      playerData.xp -= lostRewards.totalXP;
      playerData.totalXP -= lostRewards.totalXP;
      playerData.coins -= lostRewards.totalCoins;
      
      // レベルダウンチェック
      while (playerData.level > 1 && playerData.xp < 0) {
        playerData.level--;
        const requiredXP = getRequiredXP(playerData.level);
        playerData.xp += requiredXP;
      }
      if (playerData.xp < 0) playerData.xp = 0;
      if (playerData.coins < 0) playerData.coins = 0;
      
      saveRPGData();
      updateRPGDisplay();
      
      // シートデータから削除
      if (sheetData[record.date] && sheetData[record.date][record.subject]) {
        sheetData[record.date][record.subject] -= record.minutes;
        if (sheetData[record.date][record.subject] <= 0) {
          delete sheetData[record.date][record.subject];
        }
      }
      
      studyRecords.splice(index, 1);
      saveRecords();
      saveSheetData();
      updateSheetRow(record.date);
      updateSheetSums();
      updateLogTable();
      
      alert('✅ 記録を削除しました（XP/コインも調整されました）');
    }
    
    // 高度な統計分析
    function updateAdvancedStats() {
      const totalMinutes = studyRecords.reduce((sum, r) => sum + r.minutes, 0);
      document.getElementById('totalAllTime').textContent = minutesToTimeString(totalMinutes);
      
      // 今週の学習時間
      const today = new Date();
      const weekStart = new Date(today);
      weekStart.setDate(today.getDate() - today.getDay());
      weekStart.setHours(0, 0, 0, 0);
      const weekMinutes = studyRecords
        .filter(r => new Date(r.date) >= weekStart)
        .reduce((sum, r) => sum + r.minutes, 0);
      document.getElementById('weekTime').textContent = minutesToTimeString(weekMinutes);
      
      // 今月の学習時間
      const monthStart = new Date(today.getFullYear(), today.getMonth(), 1);
      const monthMinutes = studyRecords
        .filter(r => new Date(r.date) >= monthStart)
        .reduce((sum, r) => sum + r.minutes, 0);
      document.getElementById('monthTime').textContent = minutesToTimeString(monthMinutes);
      
      // 学習日数
      const uniqueDates = [...new Set(studyRecords.map(r => r.date))];
      document.getElementById('studyDays').textContent = uniqueDates.length;
      
      // 早番・遅番・休日の平均
      const shiftStats = { early: { total: 0, count: 0 }, late: { total: 0, count: 0 }, rest: { total: 0, count: 0 } };
      
      studyRecords.forEach(record => {
        const date = new Date(record.date + 'T00:00:00');
        const shiftType = getShiftType(date);
        shiftStats[shiftType].total += record.minutes;
      });
      
      uniqueDates.forEach(dateStr => {
        const date = new Date(dateStr + 'T00:00:00');
        const shiftType = getShiftType(date);
        const dayMinutes = studyRecords
          .filter(r => r.date === dateStr)
          .reduce((sum, r) => sum + r.minutes, 0);
        if (dayMinutes > 0) {
          shiftStats[shiftType].count++;
        }
      });
      
      const earlyAvg = shiftStats.early.count > 0 ? shiftStats.early.total / shiftStats.early.count : 0;
      const lateAvg = shiftStats.late.count > 0 ? shiftStats.late.total / shiftStats.late.count : 0;
      const restAvg = shiftStats.rest.count > 0 ? shiftStats.rest.total / shiftStats.rest.count : 0;
      
      document.getElementById('earlyAvg').textContent = minutesToTimeString(earlyAvg);
      document.getElementById('lateAvg').textContent = minutesToTimeString(lateAvg);
      document.getElementById('restAvg').textContent = minutesToTimeString(restAvg);
      
      // 試験まで
      const examDate = new Date('2026-08-23T00:00:00');
      const daysToExam = Math.ceil((examDate - today) / (1000 * 60 * 60 * 24));
      document.getElementById('daysToExam').textContent = daysToExam;
      
      // 残り期間の早番・遅番・休日の日数を計算
      const remainingDays = { early: 0, late: 0, rest: 0 };
      let currentDate = new Date(today);
      currentDate.setDate(currentDate.getDate() + 1);
      
      while (currentDate <= examDate) {
        const shiftType = getShiftType(currentDate);
        remainingDays[shiftType]++;
        currentDate.setDate(currentDate.getDate() + 1);
      }
      
      // 目標達成カード更新
      updateGoalCards(totalMinutes, remainingDays, earlyAvg, lateAvg, restAvg);
      
      // 科目別内訳
      updateSubjectBreakdown(totalMinutes);
      
      // トレンドチャート
      updateTrendChart();
      
      // 曜日別・科目別チャート
      updateShiftChart();
      
      // インサイト
      generateInsights(totalMinutes, earlyAvg, lateAvg, restAvg, remainingDays, shiftStats);
    }
    
    function updateGoalCards(totalMinutes, remainingDays, earlyAvg, lateAvg, restAvg) {
      const goals = [800, 1000, 1200];
      
      goals.forEach(goal => {
        const goalMinutes = goal * 60;
        const progress = Math.min(100, (totalMinutes / goalMinutes * 100)).toFixed(1);
        const remainingMinutes = Math.max(0, goalMinutes - totalMinutes);
        
        const progressEl = document.getElementById(`goal${goal}Progress`);
        if (progressEl) progressEl.textContent = progress + '%';
        
        // 早番・遅番・休日別の必要ペース
        // 早番・遅番は最大3時間、残りを休日で調整
        
        let earlyRequired = 0;
        let lateRequired = 0;
        let restRequired = 0;
        
        if (remainingMinutes > 0) {
          const maxWorkdayMinutes = 180; // 3時間
          const earlyDays = remainingDays.early || 0;
          const lateDays = remainingDays.late || 0;
          const restDays = remainingDays.rest || 0;
          
          // 早番・遅番で最大限勉強した場合の合計
          const maxWorkdayTotal = (earlyDays + lateDays) * maxWorkdayMinutes;
          
          // 休日で調整が必要な時間
          const restNeed = remainingMinutes - maxWorkdayTotal;
          
          if (restNeed > 0 && restDays > 0) {
            // 早番・遅番は3時間、休日で残りを調整
            earlyRequired = maxWorkdayMinutes;
            lateRequired = maxWorkdayMinutes;
            restRequired = restNeed / restDays;
          } else if (restNeed <= 0) {
            // 早番・遅番だけで達成可能
            const workdayTotal = earlyDays + lateDays;
            if (workdayTotal > 0) {
              const perWorkday = remainingMinutes / workdayTotal;
              earlyRequired = perWorkday;
              lateRequired = perWorkday;
              restRequired = 0;
            }
          } else {
            // 休日がない場合は均等配分
            const totalDays = earlyDays + lateDays + restDays;
            if (totalDays > 0) {
              const perDay = remainingMinutes / totalDays;
              earlyRequired = perDay;
              lateRequired = perDay;
              restRequired = perDay;
            }
          }
        }
        
        const earlyPaceEl = document.getElementById(`goal${goal}EarlyPace`);
        const latePaceEl = document.getElementById(`goal${goal}LatePace`);
        const restPaceEl = document.getElementById(`goal${goal}RestPace`);
        
        if (earlyPaceEl) earlyPaceEl.textContent = minutesToTimeString(earlyRequired);
        if (latePaceEl) latePaceEl.textContent = minutesToTimeString(lateRequired);
        if (restPaceEl) restPaceEl.textContent = minutesToTimeString(restRequired);
        
        // ステータス判定
        const statusEl = document.getElementById(`goal${goal}Status`);
        if (!statusEl) return;
        
        let statusHTML = '';
        
        if (totalMinutes >= goalMinutes) {
          const excess = totalMinutes - goalMinutes;
          statusHTML = `<span class="status-badge status-ahead">✅ 達成済み（${minutesToTimeString(excess)}超過）</span>`;
        } else {
          const shortage = goalMinutes - totalMinutes;
          
          // 経過日数のシフトパターンを計算
          const today = new Date();
          const startDate = new Date('2026-02-06T00:00:00');
          const daysPassed = Math.max(0, Math.floor((today - startDate) / (1000 * 60 * 60 * 24)));
          
          let passedDays = { early: 0, late: 0, rest: 0 };
          let checkDate = new Date(startDate);
          
          for (let i = 0; i < daysPassed; i++) {
            const shiftType = getShiftType(checkDate);
            passedDays[shiftType]++;
            checkDate.setDate(checkDate.getDate() + 1);
          }
          
          // この目標を達成するための必要ペース
          const maxWorkdayMinutes = 180; // 3時間
          const earlyDays = remainingDays.early || 0;
          const lateDays = remainingDays.late || 0;
          const restDays = remainingDays.rest || 0;
          
          let thisEarlyRequired = 0;
          let thisLateRequired = 0;
          let thisRestRequired = 0;
          
          const maxWorkdayTotal = (earlyDays + lateDays) * maxWorkdayMinutes;
          const restNeed = remainingMinutes - maxWorkdayTotal;
          
          if (restNeed > 0 && restDays > 0) {
            thisEarlyRequired = maxWorkdayMinutes;
            thisLateRequired = maxWorkdayMinutes;
            thisRestRequired = restNeed / restDays;
          } else if (restNeed <= 0) {
            const workdayTotal = earlyDays + lateDays;
            if (workdayTotal > 0) {
              const perWorkday = remainingMinutes / workdayTotal;
              thisEarlyRequired = perWorkday;
              thisLateRequired = perWorkday;
              thisRestRequired = 0;
            }
          }
          
          // 現在の平均ペースでこれまで勉強すべきだった時間（当日含む）
          const todayShift = getShiftType(today);
          const todayRequired = todayShift === 'early' ? thisEarlyRequired : 
                               todayShift === 'late' ? thisLateRequired : thisRestRequired;
          
          const shouldHaveStudied = thisEarlyRequired * passedDays.early + 
                                    thisLateRequired * passedDays.late + 
                                    thisRestRequired * passedDays.rest +
                                    todayRequired; // 当日分を追加
          
          // 実際の学習時間との差
          const progressDifference = totalMinutes - shouldHaveStudied;
          
          let progressHTML = '';
          if (progressDifference > 0) {
            progressHTML = `
              <div style="margin-top: 12px; padding: 12px; background: linear-gradient(135deg, #d1fae5 0%, #a7f3d0 100%); border-radius: 10px; border: 2px solid #10b981;">
                <div style="font-size: 13px; color: #065f46; font-weight: 700; margin-bottom: 4px;">📈 進捗状況</div>
                <div style="font-size: 16px; color: #065f46; font-weight: 800;">
                  必要ペースより <span style="font-size: 20px;">${minutesToTimeString(progressDifference)}</span> 進んでいます！
                </div>
                <div style="font-size: 12px; color: #047857; margin-top: 6px;">
                  このペースなら${goal}時間達成可能です
                </div>
              </div>
            `;
          } else if (progressDifference < 0) {
            progressHTML = `
              <div style="margin-top: 12px; padding: 12px; background: linear-gradient(135deg, #fee2e2 0%, #fecaca 100%); border-radius: 10px; border: 2px solid #ef4444;">
                <div style="font-size: 13px; color: #991b1b; font-weight: 700; margin-bottom: 4px;">⚠️ 進捗状況</div>
                <div style="font-size: 16px; color: #991b1b; font-weight: 800;">
                  必要ペースより <span style="font-size: 20px;">${minutesToTimeString(Math.abs(progressDifference))}</span> 遅れています
                </div>
                <div style="font-size: 12px; color: #7f1d1d; margin-top: 6px;">
                  ペースアップが必要です
                </div>
              </div>
            `;
          } else {
            progressHTML = `
              <div style="margin-top: 12px; padding: 12px; background: linear-gradient(135deg, #dbeafe 0%, #bfdbfe 100%); border-radius: 10px; border: 2px solid #3b82f6;">
                <div style="font-size: 13px; color: #1e40af; font-weight: 700; margin-bottom: 4px;">✨ 進捗状況</div>
                <div style="font-size: 16px; color: #1e40af; font-weight: 800;">
                  必要ペース通りです！
                </div>
                <div style="font-size: 12px; color: #1e3a8a; margin-top: 6px;">
                  この調子で頑張りましょう
                </div>
              </div>
            `;
          }
          
          statusHTML = `
            <span class="status-badge status-ontrack">📊 進行中</span>
            <div style="margin-top: 10px; font-size: 13px; color: #6b7280; font-weight: 600;">
              残り必要時間: ${minutesToTimeString(shortage)} (${(shortage/60).toFixed(0)}時間)
            </div>
            ${progressHTML}
          `;
        }
        
        statusEl.innerHTML = statusHTML;
      });
    }
    
    function updateSubjectBreakdown(totalMinutes) {
      const subjectTotals = {};
      let subjectSumMinutes = 0;
      
      subjects.forEach(subject => {
        const minutes = studyRecords
          .filter(r => r.subject === subject)
          .reduce((sum, r) => sum + r.minutes, 0);
        subjectTotals[subject] = minutes;
        if (subjectTargets[subject] > 0) {  // 「その他」を除く
          subjectSumMinutes += minutes;
        }
      });
      
      const container = document.getElementById('subjectBreakdown');
      container.innerHTML = '';
      
      subjects.forEach(subject => {
        const minutes = subjectTotals[subject] || 0;
        const hours = (minutes / 60).toFixed(1);
        const targetHours = subjectTargets[subject];
        
        // パーセンテージは目標時間に対する達成率
        const targetProgress = targetHours > 0 ? ((minutes / 60) / targetHours * 100).toFixed(1) : 0;
        
        const item = document.createElement('div');
        item.className = 'subject-item';
        item.innerHTML = `
          <div class="subject-name">
            <span>${subject}</span>
            ${targetHours > 0 ? `<span class="subject-target">目標${targetHours}h</span>` : ''}
          </div>
          <div class="subject-stats">
            <div class="subject-time">${hours}h</div>
            ${targetHours > 0 ? `<div class="subject-percentage">${targetProgress}%</div>` : '<div class="subject-percentage">-</div>'}
          </div>
          <div class="progress-bar">
            <div class="progress-fill" style="width: ${Math.min(100, targetProgress)}%"></div>
          </div>
        `;
        container.appendChild(item);
      });
    }
    
    function updateTrendChart() {
      const canvas = document.getElementById('trendChart');
      const ctx = canvas.getContext('2d');
      
      const today = new Date();
      const labels = [];
      const data = [];
      
      for (let i = 29; i >= 0; i--) {
        const date = new Date(today);
        date.setDate(today.getDate() - i);
        const dateKey = toDateKeyLocal(date);
        
        labels.push((date.getMonth() + 1) + '/' + date.getDate());
        
        const dayMinutes = studyRecords
          .filter(r => r.date === dateKey)
          .reduce((sum, r) => sum + r.minutes, 0);
        
        data.push((dayMinutes / 60).toFixed(2));
      }
      
      const recent7 = data.slice(-7).reduce((a, b) => parseFloat(a) + parseFloat(b), 0) / 7;
      const previous7 = data.slice(-14, -7).reduce((a, b) => parseFloat(a) + parseFloat(b), 0) / 7;
      const change = previous7 > 0 ? ((recent7 - previous7) / previous7 * 100).toFixed(1) : 0;
      
      const trendEl = document.getElementById('trendIndicator');
      if (change > 5) {
        trendEl.innerHTML = `<span class="trend-indicator trend-up">↗ ${change}% 増加</span>`;
      } else if (change < -5) {
        trendEl.innerHTML = `<span class="trend-indicator trend-down">↘ ${Math.abs(change)}% 減少</span>`;
      } else {
        trendEl.innerHTML = `<span class="trend-indicator trend-neutral">→ 横ばい</span>`;
      }
      
      if (trendChartInstance) {
        trendChartInstance.destroy();
      }
      
      trendChartInstance = new Chart(ctx, {
        type: 'line',
        data: {
          labels: labels,
          datasets: [{
            label: '学習時間 (時間)',
            data: data,
            borderColor: '#667eea',
            backgroundColor: 'rgba(102, 126, 234, 0.1)',
            tension: 0.4,
            fill: true
          }]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          plugins: {
            legend: {
              display: false
            }
          },
          scales: {
            y: {
              beginAtZero: true,
              ticks: {
                callback: function(value) {
                  return value + 'h';
                }
              }
            }
          }
        }
      });
    }
    
    let weekdayChartPeriod = 'all';
    let currentReportType = 'today';
    
    // レポート切り替え
    function switchReport(type) {
      currentReportType = type;
      
      // ボタンのスタイル更新
      document.querySelectorAll('#report-tab .filter-btn').forEach(btn => {
        btn.style.background = '#d1d5db';
        btn.style.color = '#374151';
      });
      
      const activeBtn = type === 'today' ? 'reportBtnToday' : type === 'week' ? 'reportBtnWeek' : 'reportBtnMonth';
      const btn = document.getElementById(activeBtn);
      if (btn) {
        btn.style.background = 'linear-gradient(135deg, #6366f1 0%, #8b5cf6 100%)';
        btn.style.color = 'white';
      }
      
      updateReport();
    }
    
    // レポート更新
    function updateReport() {
      const container = document.getElementById('reportContent');
      if (!container) return;
      
      const today = new Date();
      let startDate, endDate, title, period;
      
      if (currentReportType === 'today') {
        startDate = new Date(today);
        startDate.setHours(0, 0, 0, 0);
        endDate = new Date(today);
        endDate.setHours(23, 59, 59, 999);
        title = '今日の学習レポート';
        period = toDateKeyLocal(today);
      } else if (currentReportType === 'week') {
        startDate = new Date(today);
        startDate.setDate(today.getDate() - today.getDay());
        startDate.setHours(0, 0, 0, 0);
        endDate = new Date(today);
        endDate.setHours(23, 59, 59, 999);
        title = '今週の学習レポート';
        period = `${toDateKeyLocal(startDate)} 〜 ${toDateKeyLocal(endDate)}`;
      } else {
        startDate = new Date(today.getFullYear(), today.getMonth(), 1);
        endDate = new Date(today.getFullYear(), today.getMonth() + 1, 0);
        endDate.setHours(23, 59, 59, 999);
        title = '今月の学習レポート';
        period = `${today.getFullYear()}年${today.getMonth() + 1}月`;
      }
      
      // データ集計
      const filteredRecords = studyRecords.filter(r => {
        const recordDate = new Date(r.date + 'T00:00:00');
        return recordDate >= startDate && recordDate <= endDate;
      });
      
      const totalMinutes = filteredRecords.reduce((sum, r) => sum + r.minutes, 0);
      const uniqueDates = [...new Set(filteredRecords.map(r => r.date))].length;
      const avgMinutes = uniqueDates > 0 ? totalMinutes / uniqueDates : 0;
      
      // 科目別集計
      const subjectTotals = {};
      subjects.forEach(subject => {
        subjectTotals[subject] = filteredRecords
          .filter(r => r.subject === subject)
          .reduce((sum, r) => sum + r.minutes, 0);
      });
      
      // 最も勉強した科目
      const sortedSubjects = Object.entries(subjectTotals)
        .filter(([_, minutes]) => minutes > 0)
        .sort((a, b) => b[1] - a[1]);
      
      const topSubject = sortedSubjects.length > 0 ? sortedSubjects[0] : null;
      
      // 日別データ（週間・月間用）
      let dailyData = '';
      if (currentReportType !== 'today') {
        const dateMap = {};
        let currentDay = new Date(startDate);
        
        while (currentDay <= endDate) {
          const dateKey = toDateKeyLocal(currentDay);
          const dayMinutes = filteredRecords
            .filter(r => r.date === dateKey)
            .reduce((sum, r) => sum + r.minutes, 0);
          
          dateMap[dateKey] = dayMinutes;
          currentDay.setDate(currentDay.getDate() + 1);
        }
        
        const dates = Object.keys(dateMap).sort();
        const maxMinutes = Math.max(...Object.values(dateMap), 1);
        
        dailyData = `
          <div class="chart-section" style="margin-top: 30px;">
            <div class="chart-title">
              <span>📊</span>
              <span>日別学習時間</span>
            </div>
            <div style="display: grid; gap: 8px;">
              ${dates.map(date => {
                const minutes = dateMap[date];
                const percentage = (minutes / maxMinutes * 100).toFixed(0);
                const dateObj = new Date(date + 'T00:00:00');
                const weekdays = ['日', '月', '火', '水', '木', '金', '土'];
                const weekday = weekdays[dateObj.getDay()];
                
                return `
                  <div style="display: flex; align-items: center; gap: 12px;">
                    <div style="min-width: 100px; font-size: 13px; font-weight: 600; color: #374151;">
                      ${date.substring(5)} (${weekday})
                    </div>
                    <div style="flex: 1; background: #e5e7eb; height: 32px; border-radius: 8px; overflow: hidden; position: relative;">
                      <div style="width: ${percentage}%; height: 100%; background: linear-gradient(90deg, #6366f1, #8b5cf6); border-radius: 8px; display: flex; align-items: center; justify-content: flex-end; padding-right: 12px;">
                        ${minutes > 0 ? `<span style="font-size: 12px; font-weight: 700; color: white;">${minutesToTimeString(minutes)}</span>` : ''}
                      </div>
                    </div>
                  </div>
                `;
              }).join('')}
            </div>
          </div>
        `;
      }
      
      container.innerHTML = `
        <div style="text-align: center; margin-bottom: 30px;">
          <h2 style="font-size: 28px; font-weight: 800; color: #1f2937; margin-bottom: 8px;">${title}</h2>
          <p style="font-size: 14px; color: #6b7280; font-weight: 600;">${period}</p>
        </div>
        
        <!-- サマリーカード -->
        <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 16px; margin-bottom: 30px;">
          <div style="background: linear-gradient(135deg, #6366f1 0%, #8b5cf6 100%); color: white; padding: 24px; border-radius: 16px; box-shadow: 0 8px 24px rgba(99, 102, 241, 0.3);">
            <div style="font-size: 13px; opacity: 0.9; margin-bottom: 8px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.5px;">総学習時間</div>
            <div style="font-size: 36px; font-weight: 800; font-family: 'Courier New', monospace;">${minutesToTimeString(totalMinutes)}</div>
          </div>
          
          <div style="background: linear-gradient(135deg, #10b981 0%, #059669 100%); color: white; padding: 24px; border-radius: 16px; box-shadow: 0 8px 24px rgba(16, 185, 129, 0.3);">
            <div style="font-size: 13px; opacity: 0.9; margin-bottom: 8px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.5px;">学習日数</div>
            <div style="font-size: 36px; font-weight: 800;">${uniqueDates}日</div>
          </div>
          
          <div style="background: linear-gradient(135deg, #f59e0b 0%, #d97706 100%); color: white; padding: 24px; border-radius: 16px; box-shadow: 0 8px 24px rgba(245, 158, 11, 0.3);">
            <div style="font-size: 13px; opacity: 0.9; margin-bottom: 8px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.5px;">1日平均</div>
            <div style="font-size: 36px; font-weight: 800; font-family: 'Courier New', monospace;">${minutesToTimeString(avgMinutes)}</div>
          </div>
          
          ${topSubject ? `
          <div style="background: linear-gradient(135deg, #ec4899 0%, #db2777 100%); color: white; padding: 24px; border-radius: 16px; box-shadow: 0 8px 24px rgba(236, 72, 153, 0.3);">
            <div style="font-size: 13px; opacity: 0.9; margin-bottom: 8px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.5px;">最も勉強した科目</div>
            <div style="font-size: 18px; font-weight: 800; margin-bottom: 4px;">${topSubject[0]}</div>
            <div style="font-size: 24px; font-weight: 700; font-family: 'Courier New', monospace;">${minutesToTimeString(topSubject[1])}</div>
          </div>
          ` : ''}
        </div>
        
        <!-- 科目別内訳 -->
        <div class="chart-section">
          <div class="chart-title">
            <span>📚</span>
            <span>科目別学習時間</span>
          </div>
          <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 12px;">
            ${sortedSubjects.map(([subject, minutes]) => {
              const percentage = totalMinutes > 0 ? (minutes / totalMinutes * 100).toFixed(1) : 0;
              const hours = (minutes / 60).toFixed(1);
              
              return `
                <div style="background: white; border-radius: 12px; padding: 16px; border-left: 5px solid #6366f1; box-shadow: 0 2px 8px rgba(0,0,0,0.06);">
                  <div style="font-weight: 800; color: #1f2937; margin-bottom: 8px; font-size: 14px;">${subject}</div>
                  <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 8px;">
                    <div style="font-size: 24px; font-weight: 800; background: linear-gradient(135deg, #6366f1 0%, #8b5cf6 100%); -webkit-background-clip: text; -webkit-text-fill-color: transparent; background-clip: text;">${hours}h</div>
                    <div style="font-size: 14px; color: #6b7280; font-weight: 700;">${percentage}%</div>
                  </div>
                  <div class="progress-bar" style="background: #e5e7eb; height: 8px; border-radius: 4px; overflow: hidden;">
                    <div style="width: ${percentage}%; height: 100%; background: linear-gradient(90deg, #6366f1, #8b5cf6); border-radius: 4px;"></div>
                  </div>
                </div>
              `;
            }).join('')}
          </div>
        </div>
        
        ${dailyData}
        
        ${filteredRecords.length === 0 ? `
          <div style="text-align: center; padding: 60px 20px; background: linear-gradient(135deg, #f9fafb 0%, #f3f4f6 100%); border-radius: 16px; margin-top: 30px;">
            <div style="font-size: 48px; margin-bottom: 16px;">📚</div>
            <div style="font-size: 18px; font-weight: 700; color: #6b7280; margin-bottom: 8px;">この期間の学習記録がありません</div>
            <div style="font-size: 14px; color: #9ca3af;">タイマーで学習を記録しましょう！</div>
          </div>
        ` : ''}
        
        <div style="margin-top: 30px; text-align: center;">
          <button onclick="exportReportAsImage()" style="padding: 14px 28px; background: linear-gradient(135deg, #8b5cf6 0%, #7c3aed 100%); color: white; border: none; border-radius: 12px; font-weight: 700; font-size: 15px; cursor: pointer; box-shadow: 0 6px 20px rgba(139, 92, 246, 0.3); transition: all 0.3s;">
            📸 レポートを画像として保存
          </button>
        </div>
      `;
    }
    
    // レポート画像エクスポート（簡易版）
    function exportReportAsImage() {
      alert('📸 レポートのスクリーンショットを撮影してください！\n\nWindows: Win + Shift + S\nmacOS: Cmd + Shift + 4\n\n※ 本格的な画像エクスポート機能は次のバージョンで実装予定です');
    }
    
    function updateShiftChartPeriod(period) {
      weekdayChartPeriod = period;
      
      // ボタンのスタイル更新
      document.querySelectorAll('.filter-btn').forEach(btn => {
        btn.style.background = '#d1d5db';
        btn.style.color = '#374151';
      });
      
      const activeBtn = period === 'week' ? 'shiftBtn1Week' : period === 'month' ? 'shiftBtn1Month' : 'shiftBtnAll';
      const btn = document.getElementById(activeBtn);
      if (btn) {
        btn.style.background = '#6366f1';
        btn.style.color = 'white';
      }
      
      updateShiftChart();
    }
    
    function updateShiftChart() {
      const canvas = document.getElementById('shiftChart');
      if (!canvas) return;
      
      if (window.shiftChartInstance) {
        window.shiftChartInstance.destroy();
      }
      
      const ctx = canvas.getContext('2d');
      
      // 期間フィルター
      let filteredRecords = studyRecords;
      if (window.shiftChartPeriod === 'week') {
        const weekAgo = new Date();
        weekAgo.setDate(weekAgo.getDate() - 7);
        weekAgo.setHours(0, 0, 0, 0);
        filteredRecords = studyRecords.filter(r => fromDateKeyLocal(r.date) >= weekAgo);
      } else if (window.shiftChartPeriod === 'month') {
        const monthAgo = new Date();
        monthAgo.setDate(monthAgo.getDate() - 30);
        monthAgo.setHours(0, 0, 0, 0);
        filteredRecords = studyRecords.filter(r => fromDateKeyLocal(r.date) >= monthAgo);
      }
      
      // シフト別・科目別データ集計
      const subjects = [...new Set(filteredRecords.map(r => r.subject))];
      const shifts = ['早番', '遅番', '休日'];
      
      const data = {};
      subjects.forEach(subject => {
        data[subject] = { early: 0, late: 0, rest: 0 };
        
        filteredRecords.filter(r => r.subject === subject).forEach(record => {
          const date = fromDateKeyLocal(record.date);
          const shiftType = getShiftType(date);
          
          if (shiftType === 'early') data[subject].early += record.minutes;
          else if (shiftType === 'late') data[subject].late += record.minutes;
          else if (shiftType === 'rest') data[subject].rest += record.minutes;
        });
      });
      
      // Chart.jsデータセット作成
      const colors = [
        '#ef4444', '#f59e0b', '#10b981', '#3b82f6', '#8b5cf6',
        '#ec4899', '#f97316', '#14b8a6', '#6366f1', '#a855f7'
      ];
      
      const datasets = subjects.map((subject, idx) => ({
        label: subject,
        data: [data[subject].early, data[subject].late, data[subject].rest],
        backgroundColor: colors[idx % colors.length],
        borderColor: colors[idx % colors.length],
        borderWidth: 2
      }));
      
      window.shiftChartInstance = new Chart(ctx, {
        type: 'bar',
        data: {
          labels: shifts,
          datasets: datasets
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          plugins: {
            legend: {
              position: 'top',
              labels: { font: { size: 12 } }
            },
            tooltip: {
              callbacks: {
                label: function(context) {
                  const minutes = context.parsed.y;
                  return context.dataset.label + ': ' + minutesToTimeString(minutes);
                }
              }
            }
          },
          scales: {
            x: {
              stacked: true,
              title: { display: true, text: 'シフト' }
            },
            y: {
              stacked: true,
              beginAtZero: true,
              title: { display: true, text: '学習時間（分）' },
              ticks: {
                callback: function(value) {
                  return minutesToTimeString(value);
                }
              }
            }
          }
        }
      });
    }
    
    function generateInsights(totalMinutes, earlyAvg, lateAvg, restAvg, remainingDays, shiftStats) {
      const container = document.getElementById('insights');
      container.innerHTML = '';
      
      const insights = [];
      
      // 目標達成可能性（1000時間基準）
      const goalMinutes = 1000 * 60;
      const remainingMinutes = goalMinutes - totalMinutes;
      const totalWeightedDays = remainingDays.early + remainingDays.late + remainingDays.rest * 2;
      const baseRequired = totalWeightedDays > 0 ? remainingMinutes / totalWeightedDays : 0;
      
      if (remainingMinutes > 0) {
        const earlyRequired = baseRequired;
        const lateRequired = baseRequired;
        const restRequired = baseRequired * 2;
        
        const earlyOk = earlyRequired <= earlyAvg * 1.1;
        const lateOk = lateRequired <= lateAvg * 1.1;
        const restOk = restRequired <= restAvg * 1.1;
        
        if (earlyOk && lateOk && restOk) {
          insights.push({
            icon: '🎯',
            title: '1000時間目標達成は十分可能です！',
            text: `現在のペースを維持すれば、目標達成が可能です。早番${minutesToTimeString(earlyRequired)}、遅番${minutesToTimeString(lateRequired)}、休日${minutesToTimeString(restRequired)}の学習を継続しましょう。`
          });
        } else {
          const shortages = [];
          if (!earlyOk) shortages.push(`早番は+${Math.round(earlyRequired - earlyAvg)}分`);
          if (!lateOk) shortages.push(`遅番は+${Math.round(lateRequired - lateAvg)}分`);
          if (!restOk) shortages.push(`休日は+${Math.round(restRequired - restAvg)}分`);
          
          insights.push({
            icon: '⚠️',
            title: 'ペースアップが必要です',
            text: `目標達成には${shortages.join('、')}の増加が必要です。特に時間を確保しやすい休日を活用しましょう。`
          });
        }
      } else {
        insights.push({
          icon: '🎉',
          title: '1000時間目標達成おめでとうございます！',
          text: `素晴らしい努力です！このペースを維持して、さらに上の目標や復習に時間を使いましょう。試験本番まで気を抜かずに頑張ってください。`
        });
      }
      
      // シフトバランスのアドバイス
      if (shiftStats.rest.count > 0 && shiftStats.early.count > 0) {
        const restToEarlyRatio = restAvg / earlyAvg;
        if (restToEarlyRatio < 1.3) {
          insights.push({
            icon: '📅',
            title: '休日の学習時間を増やしましょう',
            text: `休日は勤務日より時間を確保しやすいはずです。現在、休日の平均${minutesToTimeString(restAvg)}は早番${minutesToTimeString(earlyAvg)}の${restToEarlyRatio.toFixed(1)}倍です。休日は早番の2倍程度（${minutesToTimeString(earlyAvg * 2)}）を目標にすると効率的です。`
          });
        } else if (restToEarlyRatio > 3) {
          insights.push({
            icon: '⚡',
            title: '平日の学習習慣を強化しましょう',
            text: `休日にしっかり勉強できていますね！平日も少しずつ時間を増やすと、さらに安定したペースになります。通勤時間や昼休みなどのスキマ時間も活用してみましょう。`
          });
        }
      }
      
      // 科目バランスのアドバイス
      const subjectTotals = {};
      subjects.forEach(subject => {
        if (subjectTargets[subject] > 0) {
          const minutes = studyRecords
            .filter(r => r.subject === subject)
            .reduce((sum, r) => sum + r.minutes, 0);
          subjectTotals[subject] = {
            minutes: minutes,
            progress: (minutes / 60) / subjectTargets[subject]
          };
        }
      });
      
      const sortedByProgress = Object.entries(subjectTotals)
        .sort((a, b) => a[1].progress - b[1].progress);
      
      if (sortedByProgress.length > 0) {
        const weakest = sortedByProgress[0];
        const strongest = sortedByProgress[sortedByProgress.length - 1];
        
        if (weakest[1].progress < 0.3 && strongest[1].progress > 0.6) {
          const weakProgress = (weakest[1].progress * 100).toFixed(0);
          const remaining = (subjectTargets[weakest[0]] - weakest[1].minutes / 60).toFixed(0);
          
          insights.push({
            icon: '📖',
            title: `${weakest[0]}に重点を置きましょう`,
            text: `${weakest[0]}の進捗が${weakProgress}%と遅れています。目標まで残り約${remaining}時間です。得意科目とのバランスを取りながら、苦手科目を優先的に学習することをおすすめします。`
          });
        } else if (Object.values(subjectTotals).every(s => s.progress > 0.2 && s.progress < 0.8)) {
          insights.push({
            icon: '👏',
            title: '科目バランスが素晴らしいです',
            text: `全科目をバランスよく学習できています。この調子で各科目を着実に進めていきましょう。どの科目も満遍なく力をつけることが合格への近道です。`
          });
        }
      }
      
      // 学習習慣のアドバイス
      const today = new Date();
      const last7Days = [];
      for (let i = 0; i < 7; i++) {
        const date = new Date(today);
        date.setDate(today.getDate() - i);
        last7Days.push(toDateKeyLocal(date));
      }
      
      const studiedDays = last7Days.filter(date => 
        studyRecords.some(r => r.date === date)
      ).length;
      
      const last30Days = [];
      for (let i = 0; i < 30; i++) {
        const date = new Date(today);
        date.setDate(today.getDate() - i);
        last30Days.push(toDateKeyLocal(date));
      }
      
      const studiedDays30 = last30Days.filter(date => 
        studyRecords.some(r => r.date === date)
      ).length;
      
      if (studiedDays >= 6) {
        insights.push({
          icon: '🔥',
          title: '素晴らしい学習習慣です！',
          text: `この1週間で${studiedDays}日学習しました。ほぼ毎日勉強する習慣が身についていますね。継続は力なり。この調子で合格まで走り抜けましょう！`
        });
      } else if (studiedDays >= 4) {
        insights.push({
          icon: '💪',
          title: '良いペースです！',
          text: `この1週間で${studiedDays}日学習しました。週5日以上を目標に、さらに習慣化を進めましょう。休日だけでなく、平日も少しずつ学習する習慣をつけると効果的です。`
        });
      } else if (studiedDays > 0) {
        insights.push({
          icon: '📚',
          title: '学習習慣を強化しましょう',
          text: `この1週間の学習日数は${studiedDays}日です。理想は週5日以上です。毎日少しずつでも良いので、コンスタントに学習する習慣をつけましょう。15分でも続けることが大切です。`
        });
      }
      
      // 継続性のアドバイス
      if (studiedDays30 > 0) {
        const continuityRate = (studiedDays30 / 30 * 100).toFixed(0);
        if (continuityRate > 80) {
          insights.push({
            icon: '🌟',
            title: '驚異的な継続力です！',
            text: `この30日間で${studiedDays30}日学習（継続率${continuityRate}%）。この継続力があれば合格は間違いありません。体調管理にも気をつけて、最後まで走り抜けましょう！`
          });
        }
      }
      
      // 試験までの残り時間に応じたアドバイス
      const examDate = new Date('2026-08-23T00:00:00');
      const daysToExam = Math.ceil((examDate - today) / (1000 * 60 * 60 * 24));
      
      if (daysToExam <= 30) {
        insights.push({
          icon: '⏰',
          title: '試験直前期に入りました',
          text: `試験まで残り${daysToExam}日です。新しい論点よりも、これまで学習した内容の復習と定着に重点を置きましょう。過去問を繰り返し解いて、本番の感覚を養うことが重要です。`
        });
      } else if (daysToExam <= 60) {
        insights.push({
          icon: '📝',
          title: '仕上げの時期です',
          text: `試験まで残り${daysToExam}日です。基礎知識の定着を確認しながら、過去問演習を増やしていきましょう。苦手分野の克服と得意分野の確実な得点が鍵です。`
        });
      }
      
      insights.forEach(insight => {
        const item = document.createElement('div');
        item.className = 'insight-item';
        item.innerHTML = `
          <div class="insight-icon">${insight.icon}</div>
          <div class="insight-content">
            <h3>${insight.title}</h3>
            <p>${insight.text}</p>
          </div>
        `;
        container.appendChild(item);
      });
    }
    
    // エクスポート
    function exportSheet() {
      let csv = '日付,曜日,残り日数,' + subjects.join(',') + ',当日合計\n';
      
      const startDate = new Date('2026-02-04T00:00:00');
      const examDate = new Date('2026-08-23T00:00:00');
      const weekdays = ['日', '月', '火', '水', '木', '金', '土'];
      
      let currentDate = new Date(startDate);
      while (currentDate <= examDate) {
        const dateKey = toDateKeyLocal(currentDate);
        const weekday = weekdays[currentDate.getDay()];
        const remaining = Math.floor((examDate - currentDate) / (1000 * 60 * 60 * 24));
        
        let row = `${dateKey},${weekday},${remaining}`;
        let dailyTotal = 0;
        
        subjects.forEach(subject => {
          const minutes = sheetData[dateKey]?.[subject] || 0;
          row += `,${minutesToTimeString(minutes)}`;
          dailyTotal += minutes;
        });
        
        row += `,${minutesToTimeString(dailyTotal)}\n`;
        csv += row;
        
        currentDate.setDate(currentDate.getDate() + 1);
      }
      
      downloadCSV(csv, '社労士試験_学習記録_横型.csv');
    }
    
    function exportOriginalFormat() {
      let csv = '日付,曜日,残り日数,科目,勉強時間\n';
      
      const startDate = new Date('2026-02-04T00:00:00');
      const examDate = new Date('2026-08-23T00:00:00');
      const weekdays = ['日', '月', '火', '水', '木', '金', '土'];
      
      let currentDate = new Date(startDate);
      while (currentDate <= examDate) {
        const dateKey = toDateKeyLocal(currentDate);
        const weekday = weekdays[currentDate.getDay()];
        const remaining = Math.floor((examDate - currentDate) / (1000 * 60 * 60 * 24));
        
        const dayData = sheetData[dateKey] || {};
        const hasData = Object.values(dayData).some(v => v > 0);
        
        if (hasData) {
          subjects.forEach(subject => {
            const minutes = dayData[subject] || 0;
            if (minutes > 0) {
              csv += `${dateKey},${weekday},${remaining},${subject},${minutesToTimeString(minutes)}\n`;
            }
          });
        } else {
          csv += `${dateKey},${weekday},${remaining},,\n`;
        }
        
        currentDate.setDate(currentDate.getDate() + 1);
      }
      
      downloadCSV(csv, '社労士試験_学習記録_縦型.csv');
    }
    
    function downloadCSV(csv, filename) {
      const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
      const link = document.createElement('a');
      const url = URL.createObjectURL(blob);
      
      link.setAttribute('href', url);
      link.setAttribute('download', filename);
      link.style.visibility = 'hidden';
      
      document.body.appendChild(link);
      link.click();
      document.body.removeChild(link);
    }
    
    // ユーティリティ
    function timeStringToMinutes(str) {
      const parts = str.split(':');
      if (parts.length !== 2) return 0;
      const hours = parseInt(parts[0]) || 0;
      const minutes = parseInt(parts[1]) || 0;
      return hours * 60 + minutes;
    }
    
    function minutesToTimeString(minutes) {
      const hours = Math.floor(minutes / 60);
      const mins = Math.floor(minutes % 60);
      return `${pad(hours)}:${pad(mins)}`;
    }
    
    function showMessage(msg, type) {
      const msgEl = document.getElementById('message');
      msgEl.textContent = msg;
      msgEl.className = 'message ' + type;
    }
    
    function hideMessage() {
      document.getElementById('message').className = 'message';
    }
  </script>
</body>
</html>
