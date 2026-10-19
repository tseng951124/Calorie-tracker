# Calorie-tracker
Check your daily consumption 
<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>NutriTrack 專業飲食與運動紀錄</title>
  <style>
    :root {
      --primary: #10B981;
      --primary-dark: #059669;
      --accent: #F59E0B;
      --bg: #F3F4F6;
      --card-bg: #FFFFFF;
      --text: #1F2937;
      --text-light: #6B7280;
      --border: #E5E7EB;
      --danger: #EF4444;
      --exercise: #3B82F6;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: 'PingFang TC', 'Microsoft JhengHei', system-ui, -apple-system, sans-serif;
    }

    body {
      background-color: var(--bg);
      color: var(--text);
      line-height: 1.5;
      padding: 20px 12px;
    }

    .container {
      max-width: 650px;
      margin: 0 auto;
    }

    header {
      text-align: center;
      margin-bottom: 20px;
    }

    header h1 {
      font-size: 1.8rem;
      color: var(--primary-dark);
      margin-bottom: 4px;
    }

    header p {
      color: var(--text-light);
      font-size: 0.9rem;
    }

    .card {
      background: var(--card-bg);
      border-radius: 16px;
      padding: 20px;
      margin-bottom: 20px;
      box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.05), 0 2px 4px -1px rgba(0, 0, 0, 0.03);
    }

    /* Dashboard */
    .dashboard {
      text-align: center;
    }

    .stats-grid {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 8px;
      margin: 16px 0;
    }

    @media (max-width: 600px) {
      .stats-grid {
        grid-template-columns: 1fr 1fr;
      }
    }

    .stat-box {
      background: #F9FAFB;
      padding: 10px;
      border-radius: 12px;
      border: 1px solid var(--border);
    }

    .stat-box .label {
      font-size: 0.75rem;
      color: var(--text-light);
    }

    .stat-box .value {
      font-size: 1.1rem;
      font-weight: bold;
      margin-top: 4px;
      color: var(--text);
    }

    .progress-bar-bg {
      background-color: #E5E7EB;
      height: 12px;
      border-radius: 6px;
      overflow: hidden;
      margin-bottom: 12px;
    }

    .progress-bar-fill {
      background-color: var(--primary);
      height: 100%;
      width: 0%;
      transition: width 0.4s ease, background-color 0.3s ease;
    }

    /* Target Goal Selector & TDEE toggle */
    .goal-controls {
      display: flex;
      flex-direction: column;
      gap: 10px;
      margin-top: 14px;
      padding-top: 14px;
      border-top: 1px solid var(--border);
      text-align: left;
    }

    .goal-select-group {
      display: flex;
      justify-content: space-between;
      align-items: center;
      font-size: 0.85rem;
      font-weight: 600;
      color: var(--text-light);
    }

    .goal-select-group select {
      padding: 6px 10px;
      border: 1px solid var(--border);
      border-radius: 8px;
      font-size: 0.9rem;
      background: white;
    }

    .tdee-toggle-btn {
      background: #EFF6FF;
      color: var(--exercise);
      border: 1px solid #BFDBFE;
      padding: 8px 12px;
      border-radius: 8px;
      font-size: 0.85rem;
      font-weight: 600;
      cursor: pointer;
      text-align: center;
      transition: all 0.2s;
    }

    .tdee-toggle-btn:hover {
      background: #DBEAFE;
    }

    /* TDEE Calculator Modal/Panel */
    .tdee-panel {
      background: #F8FAFC;
      border: 1px solid #CBD5E1;
      border-radius: 12px;
      padding: 16px;
      margin-top: 12px;
      display: none;
      text-align: left;
    }

    .tdee-panel.active {
      display: block;
    }

    .tdee-panel h4 {
      margin-bottom: 10px;
      color: var(--text);
      font-size: 0.95rem;
    }

    .form-row {
      display: flex;
      gap: 8px;
      margin-bottom: 8px;
    }

    .form-group {
      display: flex;
      flex-direction: column;
      gap: 4px;
      flex: 1;
    }

    label {
      font-size: 0.8rem;
      font-weight: 600;
      color: var(--text-light);
    }

    input, select {
      padding: 8px 10px;
      border: 1px solid var(--border);
      border-radius: 8px;
      font-size: 0.9rem;
      outline: none;
      background: white;
    }

    input:focus, select:focus {
      border-color: var(--primary);
      box-shadow: 0 0 0 2px rgba(16, 185, 129, 0.2);
    }

    .btn-action {
      background: var(--exercise);
      color: white;
      border: none;
      padding: 8px 12px;
      border-radius: 8px;
      font-size: 0.85rem;
      font-weight: bold;
      cursor: pointer;
      width: 100%;
      margin-top: 8px;
    }

    .btn-action:hover {
      background: #2563EB;
    }

    /* Quick Tags */
    .quick-tags {
      display: flex;
      flex-wrap: wrap;
      gap: 6px;
      margin-top: 8px;
    }

    .tag-btn {
      background: #ECFDF5;
      color: var(--primary-dark);
      border: 1px solid #A7F3D0;
      padding: 5px 10px;
      border-radius: 16px;
      font-size: 0.8rem;
      cursor: pointer;
      transition: all 0.2s;
    }

    .tag-btn.exercise-tag {
      background: #EFF6FF;
      color: var(--exercise);
      border-color: #BFDBFE;
    }

    .tag-btn:hover {
      filter: brightness(0.95);
    }

    /* Tabs for Form */
    .tab-header {
      display: flex;
      gap: 8px;
      margin-bottom: 14px;
    }

    .tab-btn {
      flex: 1;
      padding: 8px;
      border: 1px solid var(--border);
      background: #F9FAFB;
      border-radius: 8px;
      font-size: 0.9rem;
      font-weight: 600;
      cursor: pointer;
      color: var(--text-light);
    }

    .tab-btn.active {
      background: var(--primary);
      color: white;
      border-color: var(--primary);
    }

    .tab-btn.exercise-tab-btn.active {
      background: var(--exercise);
      border-color: var(--exercise);
    }

    .form-section {
      display: none;
    }

    .form-section.active {
      display: flex;
      flex-direction: column;
      gap: 10px;
    }

    .btn-submit {
      background: var(--primary);
      color: white;
      border: none;
      padding: 10px;
      border-radius: 8px;
      font-size: 0.95rem;
      font-weight: bold;
      cursor: pointer;
      transition: background 0.2s;
      margin-top: 4px;
    }

    .btn-submit.exercise-submit {
      background: var(--exercise);
    }
    .btn-submit.exercise-submit:hover {
      background: #2563EB;
    }
    .btn-submit:hover {
      background: var(--primary-dark);
    }

    /* Record List */
    .list-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 12px;
    }

    .record-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 10px 0;
      border-bottom: 1px solid var(--border);
    }

    .record-item:last-child {
      border-bottom: none;
    }

    .record-info .badge {
      font-size: 0.7rem;
      padding: 2px 6px;
      border-radius: 4px;
      margin-right: 6px;
    }

    .badge-food { background: #E0F2FE; color: #0369A1; }
    .badge-exercise { background: #DBEAFE; color: #1E40AF; }

    .record-info .name {
      font-weight: 600;
      font-size: 0.95rem;
    }

    .record-right {
      display: flex;
      align-items: center;
      gap: 10px;
    }

    .cal-val {
      font-weight: bold;
    }
    .cal-val.positive { color: var(--primary-dark); }
    .cal-val.negative { color: var(--exercise); }

    .btn-delete {
      background: none;
      border: none;
      color: var(--text-light);
      font-size: 1.1rem;
      cursor: pointer;
      padding: 2px 6px;
      border-radius: 4px;
    }

    .btn-delete:hover {
      color: var(--danger);
      background: #FEE2E2;
    }

    .empty-msg {
      text-align: center;
      color: var(--text-light);
      padding: 16px 0;
      font-size: 0.85rem;
    }
  </style>
</head>
<body>

  <div class="container">
    <header>
      <h1>🥗 NutriTrack 體態與熱量管理</h1>
      <p>精準掌握飲食攝取與運動消耗，達成你的健康目標</p>
    </header>

    <!-- 儀表板 -->
    <div class="card dashboard">
      <div class="stats-grid">
        <div class="stat-box">
          <div class="label">每日目標</div>
          <div class="value" id="targetCalDisplay">2000</div>
        </div>
        <div class="stat-box">
          <div class="label">已攝取</div>
          <div class="value" id="consumedCalDisplay">0</div>
        </div>
        <div class="stat-box">
          <div class="label">運動消耗</div>
          <div class="value" id="exerciseCalDisplay">0</div>
        </div>
        <div class="stat-box">
          <div class="label">剩餘額度</div>
          <div class="value" id="remainingCalDisplay">2000</div>
        </div>
      </div>

      <div class="progress-bar-bg">
        <div class="progress-bar-fill" id="progressBar"></div>
      </div>

      <!-- 目標與 TDEE 控制 -->
      <div class="goal-controls">
        <div class="goal-select-group">
          <label for="goalType">🎯 體態目標：</label>
          <select id="goalType" onchange="updateGoalType()">
            <option value="lose">🔥 減重 (-500 kcal)</option>
            <option value="maintain" selected>⚖️ 維持體態 (TDEE)</option>
            <option value="gain">💪 增重 (+300 kcal)</option>
          </select>
        </div>

        <button class="tdee-toggle-btn" onclick="toggleTdeePanel()">📐 計算我的 TDEE（每日總消耗熱量）</button>

        <!-- TDEE 計算表單 -->
        <div class="tdee-panel" id="tdeePanel">
          <h4>計算你的基礎代謝與 TDEE</h4>
          <div class="form-row">
            <div class="form-group">
              <label>性別</label>
              <select id="tdeeGender">
                <option value="male">男性</option>
                <option value="female">女性</option>
              </select>
            </div>
            <div class="form-group">
              <label>年齡 (歲)</label>
              <input type="number" id="tdeeAge" value="20" min="10" max="100">
            </div>
          </div>
          <div class="form-row">
            <div class="form-group">
              <label>身高 (cm)</label>
              <input type="number" id="tdeeHeight" value="165" min="100" max="220">
            </div>
            <div class="form-group">
              <label>體重 (kg)</label>
              <input type="number" id="tdeeWeight" value="55" min="30" max="200">
            </div>
          </div>
          <div class="form-group" style="margin-bottom: 8px;">
            <label>活動量等級</label>
            <select id="tdeeActivity">
              <option value="1.2">久坐 / 幾乎不運動 (1.2)</option>
              <option value="1.375">輕度運動：每周 1-3 天 (1.375)</option>
              <option value="1.55" selected>中度運動：每周 3-5 天 (1.55)</option>
              <option value="1.725">高度運動：每周 6-7 天 (1.725)</option>
            </select>
          </div>
          <button class="btn-action" onclick="calculateTdee()">計算並套用為 TDEE 基準</button>
        </div>
      </div>
    </div>

    <!-- 新增紀錄表單 (切換飲食 / 運動) -->
    <div class="card">
      <div class="tab-header">
        <button class="tab-btn active" id="tabFoodBtn" onclick="switchTab('food')">🥗 新增飲食紀錄</button>
        <button class="tab-btn exercise-tab-btn" id="tabExerciseBtn" onclick="switchTab('exercise')">🏃‍♂️ 新增運動消耗</button>
      </div>

      <!-- 飲食表單 -->
      <form id="foodForm" class="form-section active">
        <label>⚡ 快速帶入常見食物：</label>
        <div class="quick-tags">
          <button type="button" class="tag-btn" onclick="quickAddFood('早餐', '美式咖啡', 15)">☕ 冰美式 (15)</button>
          <button type="button" class="tag-btn" onclick="quickAddFood('午餐', '健康雞胸肉餐盒', 550)">🥗 雞胸肉餐盒 (550)</button>
          <button type="button" class="tag-btn" onclick="quickAddFood('晚餐', '台式便當', 750)">🍱 排骨便當 (750)</button>
          <button type="button" class="tag-btn" onclick="quickAddFood('點心', '珍珠奶茶', 450)">🧋 珍奶 (450)</button>
        </div>

        <div style="display: flex; gap: 8px; margin-top: 6px;">
          <div class="form-group" style="flex: 1;">
            <label for="mealType">餐別</label>
            <select id="mealType">
              <option value="早餐">早餐</option>
              <option value="午餐" selected>午餐</option>
              <option value="晚餐">晚餐</option>
              <option value="點心">點心</option>
            </select>
          </div>
          <div class="form-group" style="flex: 2;">
            <label for="foodName">食物名稱</label>
            <input type="text" id="foodName" placeholder="例：沙拉、香蕉" required>
          </div>
        </div>

        <div class="form-group">
          <label for="foodCalories">攝取熱量 (kcal)</label>
          <input type="number" id="foodCalories" placeholder="例：300" min="0" required>
        </div>

        <button type="submit" class="btn-submit">＋ 記錄飲食</button>
      </form>

      <!-- 運動表單 -->
      <form id="exerciseForm" class="form-section">
        <label>⚡ 快速帶入常見運動：</label>
        <div class="quick-tags">
          <button type="button" class="tag-btn exercise-tag" onclick="quickAddExercise('羽球 (1小時)', 400)">🏸 羽球 1小時 (-400)</button>
          <button type="button" class="tag-btn exercise-tag" onclick="quickAddExercise('跑步 (30分鐘)', 300)">🏃 跑步 30分 (-300)</button>
          <button type="button" class="tag-btn exercise-tag" onclick="quickAddExercise('重訓 / 健身 (1小時)', 350)">🏋️‍♂️ 重訓 1小時 (-350)</button>
          <button type="button" class="tag-btn exercise-tag" onclick="quickAddExercise('快走 (45分鐘)', 220)">🚶‍♀️ 快走 45分 (-220)</button>
        </div>

        <div class="form-group" style="margin-top: 6px;">
          <label for="exerciseName">運動項目</label>
          <input type="text" id="exerciseName" placeholder="例：游泳、瑜珈" required>
        </div>

        <div class="form-group">
          <label for="exerciseCalories">消耗熱量 (kcal)</label>
          <input type="number" id="exerciseCalories" placeholder="例：350" min="0" required>
        </div>

        <button type="submit" class="btn-submit exercise-submit">🔥 記錄運動消耗</button>
      </form>
    </div>

    <!-- 今日紀錄明細清單 -->
    <div class="card">
      <div class="list-header">
        <h3>📝 今日活動總覽明細</h3>
        <button onclick="clearAll()" style="background:none; border:none; color:var(--danger); cursor:pointer; font-size:0.8rem;">清空全部</button>
      </div>
      <div id="recordList">
        <!-- 動態渲染紀錄 -->
      </div>
    </div>
  </div>

  <script>
    // 狀態管理
    let baseTdee = parseInt(localStorage.getItem('baseTdee')) || 2000;
    let goalType = localStorage.getItem('goalType') || 'maintain';
    let records = JSON.parse(localStorage.getItem('records')) || [];

    // DOM 元素
    const targetCalDisplay = document.getElementById('targetCalDisplay');
    const consumedCalDisplay = document.getElementById('consumedCalDisplay');
    const exerciseCalDisplay = document.getElementById('exerciseCalDisplay');
    const remainingCalDisplay = document.getElementById('remainingCalDisplay');
    const progressBar = document.getElementById('progressBar');
    const goalSelect = document.getElementById('goalType');
    const tdeePanel = document.getElementById('tdeePanel');
    const recordList = document.getElementById('recordList');

    goalSelect.value = goalType;

    // 計算當前目標總熱量 (TDEE 基礎 + 目標調整)
    function getTargetCalories() {
      if (goalType === 'lose') return baseTdee - 500;
      if (goalType === 'gain') return baseTdee + 300;
      return baseTdee; // maintain
    }

    // 初始化
    function init() {
      render();
    }

    // 渲染 UI
    function render() {
      const targetCalories = getTargetCalories();
      
      // 計算總攝取與總消耗
      const totalConsumed = records
        .filter(item => item.type === 'food')
        .reduce((sum, item) => sum + item.calories, 0);

      const totalExercise = records
        .filter(item => item.type === 'exercise')
        .reduce((sum, item) => sum + item.calories, 0);

      // 剩餘額度 = 目標 - 攝取 + 運動消耗
      const remaining = targetCalories - totalConsumed + totalExercise;
      // 進度條以「攝取熱量佔目標熱量」的比例來呈現
      const percentage = Math.min(Math.round((totalConsumed / targetCalories) * 100), 100);

      // 更新數值顯示
      targetCalDisplay.textContent = targetCalories;
      consumedCalDisplay.textContent = totalConsumed;
      exerciseCalDisplay.textContent = totalExercise;
      remainingCalDisplay.textContent = remaining;

      // 進度條樣式
      progressBar.style.width = `${percentage}%`;
      if (totalConsumed > targetCalories) {
        progressBar.style.backgroundColor = 'var(--danger)';
      } else {
        progressBar.style.backgroundColor = 'var(--primary)';
      }

      // 渲染列表
      if (records.length === 0) {
        recordList.innerHTML = `<div class="empty-msg">今天還沒有新增任何飲食或運動紀錄喔！</div>`;
      } else {
        recordList.innerHTML = records.map((item, index) => {
          if (item.type === 'food') {
            return `
              <div class="record-item">
                <div class="record-info">
                  <span class="badge badge-food">${item.meal}</span>
                  <span class="name">${escapeHtml(item.name)}</span>
                </div>
                <div class="record-right">
                  <span class="cal-val positive">+${item.calories} kcal</span>
                  <button class="btn-delete" onclick="deleteItem(${index})" title="刪除">✕</button>
                </div>
              </div>
            `;
          } else {
            return `
              <div class="record-item">
                <div class="record-info">
                  <span class="badge badge-exercise">運動</span>
                  <span class="name">${escapeHtml(item.name)}</span>
                </div>
                <div class="record-right">
                  <span class="cal-val negative">-${item.calories} kcal</span>
                  <button class="btn-delete" onclick="deleteItem(${index})" title="刪除">✕</button>
                </div>
              </div>
            `;
          }
        }).join('');
      }

      // 儲存至 LocalStorage
      localStorage.setItem('baseTdee', baseTdee);
      localStorage.setItem('goalType', goalType);
      localStorage.setItem('records', JSON.stringify(records));
    }

    // 切換分頁 (飲食 / 運動)
    function switchTab(tab) {
      const foodBtn = document.getElementById('tabFoodBtn');
      const exerciseBtn = document.getElementById('tabExerciseBtn');
      const foodForm = document.getElementById('foodForm');
      const exerciseForm = document.getElementById('exerciseForm');

      if (tab === 'food') {
        foodBtn.classList.add('active');
        exerciseBtn.classList.remove('active');
        foodForm.classList.add('active');
        exerciseForm.classList.remove('active');
      } else {
        exerciseBtn.classList.add('active');
        foodBtn.classList.remove('active');
        exerciseForm.classList.add('active');
        foodForm.classList.remove('active');
      }
    }

    // 切換 TDEE 面板顯示
    function toggleTdeePanel() {
      tdeePanel.classList.toggle('active');
    }

    // 計算 TDEE (使用 Mifflin-St Jeor 公式)
    function calculateTdee() {
      const gender = document.getElementById('tdeeGender').value;
      const age = parseInt(document.getElementById('tdeeAge').value);
      const height = parseInt(document.getElementById('tdeeHeight').value);
      const weight = parseInt(document.getElementById('tdeeWeight').value);
      const activity = parseFloat(document.getElementById('tdeeActivity').value);

// BMR 計算公式
      let bmr = (10 * weight) + (6.25 * height) - (5 * age);
      if (gender === 'male') {
        bmr += 5;
      } else {
        bmr -= 161;
      }

      baseTdee = Math.round(bmr * activity);
      tdeePanel.classList.remove('active');
      alert(`計算成功！你的預估 TDEE 為 ${baseTdee} 大卡。已自動更新基準！`);
      render();
    }

    // 切換目標類型
    function updateGoalType() {
      goalType = goalSelect.value;
      render();
    }

    // 提交飲食表單
    document.getElementById('foodForm').addEventListener('submit', (e) => {
      e.preventDefault();
      const meal = document.getElementById('mealType').value;
      const name = document.getElementById('foodName').value.trim();
      const calories = parseInt(document.getElementById('foodCalories').value);

      if (name && !isNaN(calories)) {
        records.push({ type: 'food', meal, name, calories });
        document.getElementById('foodName').value = '';
        document.getElementById('foodCalories').value = '';
        render();
      }
    });

    // 提交運動表單
    document.getElementById('exerciseForm').addEventListener('submit', (e) => {
      e.preventDefault();
      const name = document.getElementById('exerciseName').value.trim();
      const calories = parseInt(document.getElementById('exerciseCalories').value);

      if (name && !isNaN(calories)) {
        records.push({ type: 'exercise', name, calories });
        document.getElementById('exerciseName').value = '';
        document.getElementById('exerciseCalories').value = '';
        render();
      }
    });

    // 快速新增飲食
    function quickAddFood(meal, name, calories) {
      records.push({ type: 'food', meal, name, calories });
      render();
    }

    // 快速新增運動
    function quickAddExercise(name, calories) {
      records.push({ type: 'exercise', name, calories });
      render();
    }

    // 刪除單項
    function deleteItem(index) {
      records.splice(index, 1);
      render();
    }

    // 清空全部
    function clearAll() {
      if (confirm('確定要清空今天的所有紀錄嗎？')) {
        records = [];
        render();
      }
    }

    // HTML跳脫防護
    function escapeHtml(str) {
      return str.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;").replace(/"/g, "&quot;").replace(/'/g, "&#039;");
    }

    // 啟動
    init();
  </script>
</body>
</html>
