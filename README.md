<!DOCTYPE http>
<http lang="zh-CN">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>雅思冲刺系统 v3 · 5.0→7.0+（离线版）</title>
<style>
  * { margin:0; padding:0; box-sizing:border-box; }
  body { font-family:-apple-system,'PingFang SC','Microsoft YaHei',sans-serif; background:#0f172a; color:#e2e8f0; min-height:100vh; }
  .app { max-width:900px; margin:0 auto; padding:16px; }
  .header { display:flex; justify-content:space-between; align-items:center; margin-bottom:16px; }
  .header h1 { font-size:20px; font-weight:700; }
  .header h1 span { color:#38bdf8; }
  .streak { background:linear-gradient(135deg,#f97316,#ef4444); padding:8px 16px; border-radius:20px; font-size:14px; font-weight:600; }
  .goal-banner { background:linear-gradient(135deg,#1e3a8a,#0ea5e9); border-radius:16px; padding:16px 20px; margin-bottom:16px; display:flex; justify-content:space-between; align-items:center; flex-wrap:wrap; gap:12px; }
  .goal-banner .now { font-size:14px; opacity:.85; }
  .goal-banner .target { font-size:24px; font-weight:800; color:#fef08a; }
  .goal-banner .days { text-align:right; }
  .goal-banner .days b { font-size:20px; display:block; }
  .tabs { display:flex; gap:5px; margin-bottom:16px; flex-wrap:wrap; }
  .tab { flex:1; min-width:56px; padding:9px 4px; border:none; border-radius:12px; background:#1e293b; color:#94a3b8; font-size:12px; font-weight:600; cursor:pointer; }
  .tab.active { background:#38bdf8; color:#0f172a; }
  .tab .dot { display:inline-block; width:7px; height:7px; border-radius:50%; margin-left:4px; vertical-align:middle; }
  .panel { display:none; }
  .panel.active { display:block; }
  .card { background:#1e293b; border-radius:16px; padding:18px; margin-bottom:14px; }
  .card h3 { font-size:15px; margin-bottom:4px; }
  .card .sub { font-size:12px; color:#94a3b8; margin-bottom:12px; }
  .subtabs { display:flex; gap:8px; margin-bottom:14px; }
  .subtab { padding:8px 18px; border-radius:12px; background:#0f172a; color:#94a3b8; font-size:13px; font-weight:600; cursor:pointer; border:1px solid #334155; }
  .subtab.active { background:#38bdf8; color:#0f172a; border-color:#38bdf8; }
  .subpanel { display:none; } .subpanel.active { display:block; }
  .task { display:flex; align-items:center; gap:10px; padding:10px 12px; border-radius:10px; margin-bottom:8px; background:#0f172a; cursor:pointer; }
  .task.done { opacity:.55; }
  .task.done .task-name { text-decoration:line-through; }
  .checkbox { width:22px; height:22px; border:2px solid #475569; border-radius:7px; flex-shrink:0; display:flex; align-items:center; justify-content:center; font-size:13px; color:#0f172a; }
  .task.done .checkbox, .check-item.done .checkbox { background:#22c55e; border-color:#22c55e; }
  .task-name { flex:1; font-size:14px; }
  .task-mins { font-size:12px; color:#64748b; }
  .add-row { display:flex; gap:8px; margin-top:8px; }
  .add-row input { flex:1; background:#0f172a; border:1px solid #334155; border-radius:10px; padding:9px 12px; color:#e2e8f0; font-size:13px; outline:none; }
  .add-row input:focus { border-color:#38bdf8; }
  .add-row button { background:#38bdf8; color:#0f172a; border:none; border-radius:10px; padding:0 16px; font-weight:700; cursor:pointer; }
  .draw-area { text-align:center; padding:8px 0 4px; }
  .topic-card { background:#0f172a; border:1px dashed #475569; border-radius:12px; padding:18px; margin:12px 0; min-height:70px; display:flex; flex-direction:column; justify-content:center; }
  .topic-card .q { font-size:15px; font-weight:600; margin-bottom:4px; }
  .topic-card .hint { font-size:12px; color:#94a3b8; }
  .btn { background:linear-gradient(135deg,#38bdf8,#0ea5e9); border:none; color:#0f172a; padding:10px 24px; border-radius:12px; font-size:14px; font-weight:700; cursor:pointer; }
  .btn:disabled { opacity:.5; }
  .btn.secondary { background:#334155; color:#e2e8f0; }
  .timer { display:flex; align-items:center; justify-content:center; gap:14px; margin-top:8px; }
  .timer .time { font-size:30px; font-weight:800; font-variant-numeric:tabular-nums; color:#38bdf8; min-width:90px; text-align:center; }
  .timer button { width:42px; height:42px; border-radius:50%; border:none; font-size:16px; cursor:pointer; }
  .check-item { display:flex; gap:10px; align-items:flex-start; padding:8px 10px; border-radius:10px; background:#0f172a; margin-bottom:8px; font-size:13px; line-height:1.5; }
  .check-item.done { opacity:.55; }
  .stats-grid { display:grid; grid-template-columns:repeat(4,1fr); gap:10px; margin-bottom:14px; }
  .stat { background:#1e293b; border-radius:14px; padding:14px 8px; text-align:center; }
  .stat .num { font-size:22px; font-weight:800; color:#38bdf8; }
  .stat .lbl { font-size:11px; color:#94a3b8; margin-top:2px; }
  .chart-wrap { display:flex; align-items:flex-end; gap:6px; height:120px; padding:10px 4px 0; }
  .bar { flex:1; background:linear-gradient(180deg,#38bdf8,#0ea5e9); border-radius:5px 5px 0 0; position:relative; min-height:4px; }
  .bar .val { position:absolute; top:-18px; left:50%; transform:translateX(-50%); font-size:10px; color:#94a3b8; }
  .bar .date { position:absolute; bottom:-18px; left:50%; transform:translateX(-50%); font-size:9px; color:#64748b; white-space:nowrap; }
  .chart-labels { display:flex; justify-content:space-between; font-size:11px; color:#64748b; margin-top:26px; }
  .skill-row { display:flex; align-items:center; gap:10px; margin-bottom:12px; }
  .skill-row .lbl { width:32px; font-size:13px; font-weight:600; }
  .skill-bar { flex:1; height:10px; background:#0f172a; border-radius:6px; overflow:hidden; }
  .skill-fill { height:100%; border-radius:6px; }
  .mock-form { display:flex; gap:8px; flex-wrap:wrap; align-items:center; }
  .mock-form input { width:64px; background:#0f172a; border:1px solid #334155; border-radius:10px; padding:9px; color:#e2e8f0; font-size:14px; text-align:center; outline:none; }
  .mock-form .skill-lbl { font-size:13px; font-weight:600; width:32px; }
  .mock-history { font-size:13px; }
  .mock-history .row { display:flex; justify-content:space-between; padding:9px 4px; border-bottom:1px solid #0f172a; }
  .mock-history .row:last-child { border-bottom:none; }
  .review-q { font-size:13px; font-weight:600; margin:12px 0 6px; color:#94a3b8; }
  .review-q:first-of-type { margin-top:4px; }
  .review-q textarea { width:100%; background:#0f172a; border:1px solid #334155; border-radius:10px; padding:10px 12px; color:#e2e8f0; font-size:13px; font-family:inherit; min-height:56px; resize:vertical; outline:none; margin-top:6px; }
  .review-q textarea:focus { border-color:#38bdf8; }
  .mood-row { display:flex; gap:8px; }
  .mood { flex:1; padding:9px; text-align:center; border-radius:10px; background:#0f172a; cursor:pointer; font-size:12px; color:#94a3b8; border:2px solid transparent; }
  .mood.sel { border-color:#38bdf8; color:#e2e8f0; }
  .save-btn { width:100%; background:linear-gradient(135deg,#22c55e,#16a34a); border:none; color:#fff; padding:14px; border-radius:14px; font-size:15px; font-weight:700; cursor:pointer; margin-top:4px; }
  .toast { position:fixed; bottom:24px; left:50%; transform:translateX(-50%) translateY(80px); background:#22c55e; color:#fff; padding:10px 22px; border-radius:24px; font-size:14px; font-weight:600; transition:transform .3s; z-index:99; }
  .toast.show { transform:translateX(-50%) translateY(0); }
  .empty { text-align:center; color:#64748b; font-size:13px; padding:20px 0; }
  .reset-link { display:block; text-align:center; font-size:11px; color:#475569; margin-top:20px; cursor:pointer; }
  .legend { display:flex; gap:14px; flex-wrap:wrap; font-size:12px; color:#94a3b8; margin-bottom:8px; }
  .legend i { display:inline-block; width:14px; height:3px; border-radius:2px; margin-right:5px; vertical-align:middle; }
  .weak-row { background:#0f172a; border-radius:12px; padding:12px; margin-bottom:10px; }
  .weak-top { display:flex; align-items:center; gap:10px; margin-bottom:8px; }
  .weak-top .wt { flex:1; font-size:13.5px; font-weight:600; }
  .status-dot { width:12px; height:12px; border-radius:50%; flex-shrink:0; background:#334155; }
  .weak-ctl { display:flex; gap:8px; }
  .weak-ctl select { background:#1e293b; color:#e2e8f0; border:1px solid #334155; border-radius:8px; padding:7px 8px; font-size:12px; outline:none; }
  .weak-ctl input { flex:1; background:#1e293b; border:1px solid #334155; border-radius:8px; padding:7px 10px; color:#e2e8f0; font-size:12px; outline:none; }
  .weak-hist { display:flex; gap:4px; margin-top:8px; align-items:center; }
  .seg { width:16px; height:8px; border-radius:4px; background:#334155; }
  .seg.s0 { background:#f43f5e; } .seg.s1 { background:#fbbf24; } .seg.s2 { background:#22c55e; }
  .weak-hist .hl { font-size:11px; color:#64748b; margin-left:auto; }
  .voc-grid { display:grid; grid-template-columns:repeat(4,1fr); gap:10px; margin-bottom:14px; }
  .voc-stat { background:#1e293b; border-radius:14px; padding:13px 6px; text-align:center; }
  .voc-stat .num { font-size:20px; font-weight:800; }
  .voc-stat .lbl { font-size:11px; color:#94a3b8; margin-top:2px; }
  .set-chip { display:inline-block; background:#0f172a; border:1px solid #334155; border-radius:16px; padding:5px 12px; font-size:12px; margin:0 6px 8px 0; cursor:pointer; color:#94a3b8; }
  .set-chip.sel { border-color:#a78bfa; color:#a78bfa; }
  .book-chip { display:inline-block; background:#0f172a; border:1px solid #334155; border-radius:16px; padding:5px 12px; font-size:12.5px; margin:0 6px 8px 0; cursor:pointer; color:#94a3b8; }
  .book-chip.sel { border-color:#34d399; color:#34d399; font-weight:700; }
  .book-chip .del { margin-left:6px; color:#f43f5e; font-weight:700; }
  .word-card { background:#0f172a; border-radius:12px; padding:10px 14px; margin-bottom:6px; display:flex; align-items:baseline; gap:10px; }
  .word-card .en { font-size:15px; font-weight:800; color:#38bdf8; min-width:130px; }
  .word-card .pos { font-size:11.5px; color:#a78bfa; min-width:52px; }
  .word-card .cn { font-size:13px; color:#cbd5e1; flex:1; line-height:1.5; }
  .quiz-card { background:#0f172a; border-radius:16px; padding:24px 18px; text-align:center; }
  .quiz-dir { display:inline-block; font-size:12px; padding:4px 12px; border-radius:12px; background:#1e293b; color:#38bdf8; margin-bottom:14px; }
  .quiz-q { font-size:19px; font-weight:700; margin-bottom:16px; line-height:1.5; }
  .quiz-q small { display:block; font-size:12px; color:#64748b; font-weight:400; margin-top:6px; }
  .quiz-input { width:100%; max-width:340px; background:#1e293b; border:1px solid #334155; border-radius:12px; padding:12px 14px; color:#e2e8f0; font-size:16px; text-align:center; outline:none; margin-bottom:12px; }
  .quiz-input:focus { border-color:#38bdf8; }
  .opt { display:block; width:100%; max-width:440px; margin:0 auto 8px; background:#1e293b; border:1px solid #334155; border-radius:12px; padding:11px 12px; color:#e2e8f0; font-size:13.5px; cursor:pointer; text-align:left; line-height:1.5; }
  .opt:hover { border-color:#38bdf8; }
  .opt.correct { border-color:#22c55e; background:#052e16; }
  .opt.wrongpick { border-color:#f43f5e; background:#450a0a; }
  .quiz-fb { margin:12px auto; max-width:480px; text-align:left; background:#1e293b; border-radius:12px; padding:12px 14px; font-size:13px; line-height:1.7; display:none; }
  .quiz-fb b { color:#fbbf24; }
  .quiz-fb .rare b { color:#a78bfa; }
  .cam-book { background:#1e293b; border-radius:12px; margin-bottom:8px; }
  .cam-book > summary { padding:12px 14px; cursor:pointer; font-size:14px; font-weight:700; list-style:none; }
  .cam-test { background:#0f172a; border-radius:10px; margin:6px 8px; }
  .cam-test > summary { padding:9px 12px; cursor:pointer; font-size:13px; font-weight:600; list-style:none; color:#94a3b8; }
  details > summary::-webkit-details-marker { display:none; }
  summary::before { content:'▸ '; color:#38bdf8; }
  details[open] > summary::before { content:'▾ '; }
  .cam-body { padding:6px 12px 14px; }
  .cam-row { display:grid; grid-template-columns:42px 138px 62px 62px 1fr; gap:8px; margin-bottom:8px; align-items:center; }
  .cam-row .sl { font-size:13px; font-weight:600; }
  .cam-row input { background:#1e293b; border:1px solid #334155; border-radius:8px; padding:7px 9px; color:#e2e8f0; font-size:12.5px; outline:none; width:100%; }
  .cam-row input:focus { border-color:#38bdf8; }
  .cam-meta { font-size:11px; color:#64748b; font-weight:400; margin-left:8px; }
  .cam-summary-line { font-size:13px; line-height:1.9; }
  .cam-summary-line b { color:#38bdf8; }
  .cam-summary-line .warn { color:#f43f5e; font-weight:700; }
  .import-area textarea { width:100%; background:#0f172a; border:1px solid #334155; border-radius:10px; padding:10px 12px; color:#e2e8f0; font-size:12.5px; font-family:inherit; min-height:90px; resize:vertical; outline:none; }
  .import-area textarea:focus { border-color:#38bdf8; }
</style>
<base target="_blank">
</head>
<body>
<div class="app">
  <div class="header">
    <h1>雅思冲刺系统 <span>5.0 → 7.0+</span></h1>
    <div class="streak">🔥 连续 <b id="streakNum">0</b> 天</div>
  </div>
  <div class="goal-banner">
    <div>
      <div class="now">当前预估 5.0 · 基础曾达 6.5</div>
      <div class="target">目标 7.0 - 7.5</div>
    </div>
    <div class="days"><b id="totalDone">0</b><span style="font-size:12px;color:#bae6fd;">累计打卡天数</span></div>
  </div>

  <div class="tabs">
    <button class="tab active" data-tab="today">📅 打卡<span class="dot" id="dotToday" style="background:#f43f5e;"></span></button>
    <button class="tab" data-tab="speak">🎤 口语</button>
    <button class="tab" data-tab="write">✍️ 写作</button>
    <button class="tab" data-tab="stats">📊 数据</button>
    <button class="tab" data-tab="weak">🎯 弱点</button>
    <button class="tab" data-tab="voc">📖 单词</button>
    <button class="tab" data-tab="cam">📘 剑雅</button>
    <button class="tab" data-tab="review">📝 复盘</button>
  </div>

  <div class="panel active" id="panel-today">
    <div class="card">
      <h3>今日任务 <span id="todayProgress" style="margin-left:auto;font-size:13px;color:#38bdf8;"></span></h3>
      <div class="sub">原则：输入:输出 = 4:6，听说写时间 ≥ 50%</div>
      <div id="taskList"></div>
      <div class="add-row">
        <input id="newTaskName" placeholder="添加自定义任务，如：精听剑雅15 T2 S3">
        <input id="newTaskMins" placeholder="分钟" style="flex:0 0 60px;">
        <button onclick="addTask()">+</button>
      </div>
    </div>
    <div class="card">
      <h3>⏱ 专注计时</h3>
      <div class="timer">
        <button style="background:#334155;color:#e2e8f0;" onclick="adjustTime(-300)">−5</button>
        <div class="time" id="timerDisplay">25:00</div>
        <button style="background:#334155;color:#e2e8f0;" onclick="adjustTime(300)">+5</button>
        <button style="background:#22c55e;color:#fff;" id="startBtn" onclick="toggleTimer()">▶</button>
        <button style="background:#f43f5e;color:#fff;" onclick="resetTimer()">↺</button>
      </div>
    </div>
    <button class="save-btn" onclick="finishDay()">✅ 完成今日打卡</button>
  </div>

  <div class="panel" id="panel-speak">
    <div class="card">
      <h3>🎲 Part 1 随机抽题 <span style="font-size:11px;color:#64748b;font-weight:400;">内置16题 · 源自雅思口语公开高频话题家族</span></h3>
      <div class="sub">每日3题 · 每题录音60秒必须开口</div>
      <div class="draw-area">
        <div class="topic-card" id="topicCard"><div class="q">点击下方按钮抽题</div><div class="hint">抽题后开始录音，60秒不停顿地说</div></div>
        <button class="btn" onclick="drawTopic()">🎲 抽一题</button>
        <button class="btn secondary" id="recordBtn" onclick="mockRecord()" disabled>⏺ 开始录音</button>
      </div>
    </div>
    <div class="card">
      <h3>📼 录音记录</h3>
      <div id="recordList"><div class="empty">还没有录音，先抽题开始吧</div></div>
    </div>
  </div>

  <div class="panel" id="panel-write">
    <div class="card">
      <h3>✍️ Task 2 自查清单</h3>
      <div class="sub">写完一篇作文后逐项检查 · 全绿才算完成一篇</div>
      <div id="checklist"></div>
      <div style="display:flex;gap:8px;margin-top:10px;">
        <button class="btn" onclick="resetChecklist()">↺ 重置清单</button>
        <button class="btn secondary" onclick="logEssay()">✔ 记录本篇完成</button>
      </div>
    </div>
    <div class="card">
      <h3>📚 句型升级记录 · 已掌握 <b id="sentenceCount" style="color:#38bdf8;">0</b> 种</h3>
      <div id="sentenceList" class="empty">例：I think → It is widely believed that...</div>
      <div class="add-row">
        <input id="newSentence" placeholder="记录一个新掌握的高分句型...">
        <button onclick="addSentence()">+</button>
      </div>
    </div>
  </div>

  <div class="panel" id="panel-stats">
    <div class="subtabs">
      <div class="subtab active" data-sub="daily">📊 日常数据分析</div>
      <div class="subtab" data-sub="mock">🎯 模考数据分析</div>
    </div>

    <div class="subpanel active" id="sub-daily">
      <div class="stats-grid">
        <div class="stat"><div class="num" id="statDays">0</div><div class="lbl">打卡天数</div></div>
        <div class="stat"><div class="num" id="statHours">0</div><div class="lbl">累计小时</div></div>
        <div class="stat"><div class="num" id="statTasks">0</div><div class="lbl">完成任务</div></div>
        <div class="stat"><div class="num" id="statEssays">0</div><div class="lbl">完成作文</div></div>
      </div>
      <div class="card">
        <h3>📈 最近14天有效学习时长（分钟）</h3>
        <div class="chart-wrap" id="chart"></div>
        <div class="chart-labels"><span>← 更早</span><span>今天 →</span></div>
      </div>
      <div class="card">
        <h3>🎯 四项技能时间分布</h3>
        <div id="skillBars"></div>
      </div>
    </div>

    <div class="subpanel" id="sub-mock">
      <div class="card">
        <h3>📉 模考分数趋势曲线 <span style="font-size:11px;color:#64748b;font-weight:400;">记录2次以上模考后自动生成</span></h3>
        <div class="legend">
          <span><i style="background:#38bdf8;"></i>听力</span>
          <span><i style="background:#34d399;"></i>阅读</span>
          <span><i style="background:#fb923c;"></i>写作</span>
          <span><i style="background:#f472b6;"></i>口语</span>
          <span><i style="background:#e2e8f0;"></i>平均分</span>
        </div>
        <div id="trendChart"><div class="empty">还没有模考记录，先去下方记录一次模考成绩</div></div>
      </div>
      <div class="card">
        <h3>📝 模考成绩记录</h3>
        <div class="sub">每月1-2次全真限时模考 · 用于校准剑雅练习与真实考试的差距</div>
        <div class="mock-form">
          <span class="skill-lbl">听力</span><input id="mL" type="number" step="0.5" min="0" max="9">
          <span class="skill-lbl">阅读</span><input id="mR" type="number" step="0.5" min="0" max="9">
          <span class="skill-lbl">写作</span><input id="mW" type="number" step="0.5" min="0" max="9">
          <span class="skill-lbl">口语</span><input id="mS" type="number" step="0.5" min="0" max="9">
          <button class="btn" style="padding:9px 18px;" onclick="addMock()">记录</button>
        </div>
        <div class="mock-history" id="mockHistory" style="margin-top:12px;"><div class="empty">还没有模考记录</div></div>
      </div>
      <div class="card">
        <h3>📘 剑雅真题练习汇总 <span style="font-size:11px;color:#64748b;font-weight:400;">自动抓取【剑雅】模块数据 · 红色为均分不足6.0的科目</span></h3>
        <div id="camSummary"><div class="empty">还没有剑雅练习记录</div></div>
      </div>
    </div>
  </div>

  <div class="panel" id="panel-weak">
    <div class="card">
      <h3>🎯 弱点清单追踪</h3>
      <div class="sub">每周复盘时更新 · 🔴未改善 🟡部分改善 🟢已改善 · 支持从复盘页一键同步 · 右侧色带为最近8次轨迹</div>
      <div id="weakList"></div>
      <div class="add-row">
        <input id="newWeak" placeholder="添加自定义弱点，如：写作时间总不够用">
        <button onclick="addWeak()">+</button>
      </div>
    </div>
  </div>

  <div class="panel" id="panel-voc">
    <div class="card">
      <h3>📚 我的词书 <span style="font-size:11px;color:#64748b;font-weight:400;">点击词书查看全部单词（仅单词+词性+常见释义）</span></h3>
      <div id="bookChips"></div>
      <div style="display:flex;gap:10px;align-items:center;flex-wrap:wrap;margin-top:4px;">
        <span style="font-size:12.5px;color:#94a3b8;">每日记忆量</span>
        <select id="dailyCount" onchange="setCount(this.value)" style="background:#0f172a;color:#e2e8f0;border:1px solid #334155;border-radius:8px;padding:7px 10px;font-size:13px;outline:none;">
          <option value="10">10 词</option>
          <option value="15">15 词</option>
          <option value="20" selected>20 词</option>
          <option value="30">30 词</option>
          <option value="50">50 词</option>
        </select>
        <span id="bookInfo" style="font-size:12px;color:#64748b;"></span>
      </div>
    </div>
    <div class="card import-area">
      <h3>📥 导入新词书</h3>
      <div class="sub">粘贴单词表，每行一条：单词 词性 常见释义（如：abandon vt. 放弃；抛弃）· 可同时保留多本词书随时切换</div>
      <div class="add-row" style="margin-top:0;margin-bottom:8px;">
        <input id="bookName" placeholder="词书名称，如：雅思词汇真经（不填则自动命名）">
      </div>
      <textarea id="bookText" placeholder="abandon vt. 放弃；抛弃&#10;ability n. 能力；才能&#10;absorb vt. 吸收；使全神贯注&#10;……"></textarea>
      <button class="btn" style="margin-top:10px;" onclick="importBook()">📥 解析并导入</button>
    </div>
    <div class="voc-grid" style="margin-top:14px;">
      <div class="voc-stat"><div class="num" style="color:#38bdf8;" id="vocTotal">0</div><div class="lbl">本书词量</div></div>
      <div class="voc-stat"><div class="num" style="color:#fbbf24;" id="vocNew">0</div><div class="lbl">今日新词</div></div>
      <div class="voc-stat"><div class="num" style="color:#f472b6;" id="vocDue">0</div><div class="lbl">到期复习</div></div>
      <div class="voc-stat"><div class="num" style="color:#22c55e;" id="vocMaster">0</div><div class="lbl">已掌握</div></div>
    </div>
    <div class="card">
      <h3>🧠 艾宾浩斯记忆队列 <span style="font-size:11px;color:#64748b;font-weight:400;">间隔：当天→1→2→4→7→15→30天 · 中译英/英译中交替</span></h3>
      <button class="btn" style="width:100%;" id="quizStartBtn" onclick="startQuiz()">▶ 开始今日学习</button>
    </div>
    <div id="quizArea"></div>
    <div class="card">
      <h3>📖 词库浏览 <span style="font-size:11px;color:#64748b;font-weight:400;">点击语义组标签筛选</span></h3>
      <div id="setChips"></div>
      <div id="wordBrowser"></div>
    </div>
  </div>

  <div class="panel" id="panel-cam">
    <div class="card">
      <h3>📘 剑雅真题复盘 <span style="font-size:11px;color:#64748b;font-weight:400;">剑4–剑20 · 每本4套A类习题 · 点开填写：日期 / 时长 / 分数 / 复盘</span></h3>
      <div class="sub">数据会被【周复盘】自动抓取分析，并汇入【数据→模考数据分析】</div>
      <div id="camList"></div>
    </div>
  </div>

  <div class="panel" id="panel-review">
    <div class="card">
      <h3>🔍 本周自动分析</h3>
      <div class="sub">自动抓取【剑雅真题-复盘】中自上次复盘以来的数据</div>
      <div id="autoAnalysis"><div class="empty">点击生成本周分析</div></div>
      <div style="display:flex;gap:8px;margin-top:10px;flex-wrap:wrap;">
        <button class="btn" onclick="genAnalysis()">⚡ 生成本周分析</button>
        <button class="btn secondary" id="btnFillRv3" onclick="fillRv3()" disabled>写入复盘第3题</button>
        <button class="btn secondary" id="btnSyncWeak" onclick="syncWeak()" disabled>同步到弱点模块</button>
      </div>
    </div>
    <div class="card">
      <h3>📝 本周复盘</h3>
      <div class="review-q">1. 本周口语流利度有改善吗？（填充词/停顿情况）</div>
      <textarea id="rv1"></textarea>
      <div class="review-q">2. 写作论证质量如何？（立场/例证/句式多样性）</div>
      <textarea id="rv2"></textarea>
      <div class="review-q">3. 听力/阅读错题主要集中在什么类型？</div>
      <textarea id="rv3" placeholder="可点上方「写入复盘第3题」自动填入剑雅数据分析"></textarea>
      <div class="review-q">4. 下周重心调整</div>
      <textarea id="rv4"></textarea>
      <div class="review-q">本周整体状态</div>
      <div class="mood-row" id="moodRow">
        <div class="mood" data-v="😖">😖 很吃力</div>
        <div class="mood" data-v="😐">😐 一般</div>
        <div class="mood" data-v="🙂">🙂 顺利</div>
        <div class="mood" data-v="🔥">🔥 状态极佳</div>
      </div>
      <button class="save-btn" style="margin-top:14px;" onclick="saveReview()">保存本周复盘</button>
    </div>
    <div class="card">
      <h3>📖 历史复盘</h3>
      <div id="reviewHistory"><div class="empty">还没有复盘记录</div></div>
    </div>
  </div>

  <span class="reset-link" onclick="resetAll()">清除全部数据（重新开始）</span>
</div>

<div class="toast" id="toast">已保存 ✓</div>

<script>
(function(){
  const KEY='ielts_system_v3';
  const today=new Date().toISOString().slice(0,10);
  const yesterday=new Date(Date.now()-864e5).toISOString().slice(0,10);
  const IV=[0,1,2,4,7,15,30];

  const defaultTasks=[
    {id:'t1',name:'词汇激活：复习核心词50个 + 自己造句',mins:25,skill:'vocab'},
    {id:'t2',name:'听力精听：剑雅真题 Section 3/4 听写+跟读',mins:40,skill:'listen'},
    {id:'t3',name:'口语：Part 1 抽3题录音 + 回听自评',mins:30,skill:'speak'},
    {id:'t4',name:'写作：Task 2 一篇 + 自查清单',mins:45,skill:'write'},
    {id:'t5',name:'阅读：限时一篇 + 同义替换整理',mins:30,skill:'read'}
  ];

  const topics=[
    {q:"Do you like living in your hometown?",hint:"家乡话题 · 试试对比过去和现在"},
    {q:"Do you prefer to study alone or with others?",hint:"学习偏好 · 给一个具体例子"},
    {q:"How often do you use your phone?",hint:"科技话题 · 说一个具体场景"},
    {q:"Do you like cooking? Why or why not?",hint:"生活习惯 · 用2-3个理由展开"},
    {q:"What kind of music do you like?",hint:"爱好话题 · 描述一次具体经历"},
    {q:"Do you think it is important to learn English?",hint:"语言学习 · 注意用高分搭配"},
    {q:"What do you usually do on weekends?",hint:"日常活动 · 避免只列清单，要有细节"},
    {q:"Do you prefer reading paper books or e-books?",hint:"对比题 · 明确表态+理由"},
    {q:"What is your favourite season?",hint:"季节话题 · 加入感官描述"},
    {q:"Do you like to take photos?",hint:"爱好话题 · 说一次拍照的经历"},
    {q:"How do you usually get to work or school?",hint:"交通话题 · 可以谈谈环保角度"},
    {q:"Do you think watching TV is a waste of time?",hint:"观点题 · 立场要鲜明"},
    {q:"What skills would you like to learn?",hint:"能力话题 · 和未来目标联系起来"},
    {q:"Do you prefer eating at home or eating out?",hint:"饮食话题 · 对比两者优缺点"},
    {q:"Is your neighbourhood a good place to live?",hint:"居住环境 · 举例说明"},
    {q:"Do you like your name? Why?",hint:"个人话题 · 讲故事更容易说满60秒"}
  ];

  const DEFAULT_BOOK={id:'default',name:'内置·高频同义替换70词',words:[
    {en:'important',pos:'adj.',cn:'重要的，有重大影响的',rare:'僻：（人）有社会地位的；自命不凡的（贬，罕用）',theme:'重要的'},
    {en:'crucial',pos:'adj.',cn:'至关重要的，决定性的',rare:'僻：十字形的；十字路口的（古义）',theme:'重要的'},
    {en:'vital',pos:'adj.',cn:'极其重要的，必不可少的',rare:'僻：生命的，维持生命所需的（vital organs 生命器官）',theme:'重要的'},
    {en:'pivotal',pos:'adj.',cn:'关键的，起枢纽作用的',rare:'僻：枢轴的；像枢轴一样转动的',theme:'重要的'},
    {en:'paramount',pos:'adj.',cn:'首要的，至高无上的',rare:'僻：n. 最高统治者，元首（罕用）',theme:'重要的'},
    {en:'big',pos:'adj.',cn:'大的，重大的',rare:'',theme:'大的/大量的'},
    {en:'substantial',pos:'adj.',cn:'大量的，可观的（substantial evidence 充分证据）',rare:'僻：坚固的，结实的（指物体）',theme:'大的/大量的'},
    {en:'considerable',pos:'adj.',cn:'相当大的，可观的',rare:'僻：值得考虑的，值得重视的（正式/古义）',theme:'大的/大量的'},
    {en:'significant',pos:'adj.',cn:'重要的，显著的',rare:'僻：（统计学上）显著的，非偶然的',theme:'大的/大量的'},
    {en:'ample',pos:'adj.',cn:'充足的，绰绰有余的',rare:'僻：（身材）丰满的；（空间）宽敞的（正式）',theme:'大的/大量的'},
    {en:'good',pos:'adj.',cn:'好的，有益的',rare:'',theme:'好的/有益的'},
    {en:'beneficial',pos:'adj.',cn:'有益的，有利的（be beneficial to）',rare:'僻：（法律）受益的（beneficial owner 受益所有人）',theme:'好的/有益的'},
    {en:'favourable',pos:'adj.',cn:'有利的，赞成的（favourable conditions 有利条件）',rare:'僻：（神情）赞许的，讨人喜欢的',theme:'好的/有益的'},
    {en:'advantageous',pos:'adj.',cn:'有利的，占优势的',rare:'僻：（罕用）同 favourable',theme:'好的/有益的'},
    {en:'rewarding',pos:'adj.',cn:'有回报的，有意义的（a rewarding job）',rare:'僻：（罕用）作为报酬的',theme:'好的/有益的'},
    {en:'bad',pos:'adj.',cn:'坏的，有害的',rare:'',theme:'坏的/有害的'},
    {en:'harmful',pos:'adj.',cn:'有害的（be harmful to）',rare:'',theme:'坏的/有害的'},
    {en:'detrimental',pos:'adj.',cn:'有害的，不利的（正式，常接 to）',rare:'僻：（罕用）贬损的，诋毁的',theme:'坏的/有害的'},
    {en:'adverse',pos:'adj.',cn:'不利的，有害的（adverse effects 副作用）',rare:'僻：（方向/位置）相反的，逆的',theme:'坏的/有害的'},
    {en:'damaging',pos:'adj.',cn:'造成损害的，破坏性的',rare:'僻：（罕用）诽谤的',theme:'坏的/有害的'},
    {en:'increase',pos:'v.&n.',cn:'增加，上升',rare:'',theme:'增加/上升'},
    {en:'rise',pos:'v.&n.',cn:'上升，上涨（vi.）',rare:'',theme:'增加/上升'},
    {en:'grow',pos:'v.',cn:'增长，发展',rare:'僻：种植；变得（grow tired 渐渐疲倦）',theme:'增加/上升'},
    {en:'escalate',pos:'v.',cn:'（使）逐步升级，不断恶化',rare:'僻：（罕用）乘自动扶梯上升',theme:'增加/上升'},
    {en:'surge',pos:'v.&n.',cn:'激增，涌动（a surge in demand 需求激增）',rare:'僻：n. 大浪，波涛',theme:'增加/上升'},
    {en:'soar',pos:'v.',cn:'猛增，飞涨（soaring prices 飞涨的物价）',rare:'僻：（鸟）翱翔；（情绪）高涨',theme:'增加/上升'},
    {en:'decrease',pos:'v.&n.',cn:'减少，下降',rare:'',theme:'减少/下降'},
    {en:'decline',pos:'v.&n.',cn:'下降，衰退；婉拒（decline an invitation）',rare:'僻：n.（逐渐）衰落，下坡路',theme:'减少/下降'},
    {en:'reduce',pos:'v.',cn:'减少，降低',rare:'僻：（正式）使沦为（be reduced to doing）',theme:'减少/下降'},
    {en:'diminish',pos:'v.',cn:'（使）减少，削弱（正式）',rare:'僻：使减损…的重要性或价值',theme:'减少/下降'},
    {en:'plummet',pos:'v.',cn:'暴跌，直线下降',rare:'僻：n. 铅锤，测深锤（本义）',theme:'减少/下降'},
    {en:'think',pos:'v.',cn:'认为，想',rare:'',theme:'认为/主张'},
    {en:'believe',pos:'v.',cn:'相信，认为',rare:'',theme:'认为/主张'},
    {en:'argue',pos:'v.',cn:'主张，论证（议论文高频）',rare:'僻：争吵；表明，证明',theme:'认为/主张'},
    {en:'contend',pos:'v.',cn:'主张，争辩（正式）',rare:'僻：竞争，争夺（本义）',theme:'认为/主张'},
    {en:'maintain',pos:'v.',cn:'坚持认为；维持，保持（正式）',rare:'僻：保养，维护（maintain a car）',theme:'认为/主张'},
    {en:'assume',pos:'v.',cn:'假定，假设；认为',rare:'僻：承担（责任）；掌权（assume control）',theme:'认为/主张'},
    {en:'show',pos:'v.',cn:'显示，表明',rare:'',theme:'表明/显示'},
    {en:'indicate',pos:'v.',cn:'表明，指示（正式）',rare:'僻：（车辆）打转向灯；简单提及',theme:'表明/显示'},
    {en:'demonstrate',pos:'v.',cn:'证明，演示，展示',rare:'僻：示威，游行（demonstrate against）',theme:'表明/显示'},
    {en:'reveal',pos:'v.',cn:'揭示，透露',rare:'僻：使显露；（遗迹等）被发掘出土',theme:'表明/显示'},
    {en:'illustrate',pos:'v.',cn:'（用例子/图表）说明，阐明',rare:'僻：给…加插图',theme:'表明/显示'},
    {en:'effect',pos:'n.',cn:'影响，效果（n.）',rare:'僻：v. 使发生，实现（正式，罕用）',theme:'影响'},
    {en:'impact',pos:'n.&v.',cn:'影响，冲击（have an impact on）',rare:'僻：v. 撞击；挤入，压紧',theme:'影响'},
    {en:'influence',pos:'n.&v.',cn:'影响',rare:'',theme:'影响'},
    {en:'implication',pos:'n.',cn:'可能的影响/后果；暗示',rare:'僻：卷入，牵连（正式，罕用）',theme:'影响'},
    {en:'repercussion',pos:'n.',cn:'（间接的、深远的）后果，影响（正式）',rare:'僻：回声，反弹（本义）',theme:'影响'},
    {en:'need',pos:'v.&n.',cn:'需要',rare:'',theme:'需要/要求'},
    {en:'require',pos:'v.',cn:'需要，要求',rare:'',theme:'需要/要求'},
    {en:'demand',pos:'v.&n.',cn:'要求，需求',rare:'僻：强烈要求（demand an explanation）',theme:'需要/要求'},
    {en:'necessitate',pos:'v.',cn:'使成为必需，需要（正式）',rare:'僻：（罕用）迫使',theme:'需要/要求'},
    {en:'call for',pos:'phr.',cn:'需要，呼吁（正式）',rare:'',theme:'需要/要求'},
    {en:'solve',pos:'v.',cn:'解决（问题）',rare:'',theme:'解决/应对'},
    {en:'address',pos:'v.',cn:'处理，应对（问题）（雅思高频）',rare:'常：地址；演讲 · 僻：向…讲话',theme:'解决/应对'},
    {en:'tackle',pos:'v.',cn:'处理，解决（tackle a problem）',rare:'僻：阻截，铲球（体育）',theme:'解决/应对'},
    {en:'resolve',pos:'v.',cn:'解决（争端/问题）；决心',rare:'僻：分解（化学）',theme:'解决/应对'},
    {en:'combat',pos:'v.&n.',cn:'抗击，对抗（combat climate change）',rare:'僻：战斗，搏斗（本义）',theme:'解决/应对'},
    {en:'change',pos:'v.&n.',cn:'改变，变化',rare:'',theme:'改变'},
    {en:'alter',pos:'v.',cn:'（部分地）改变，改动',rare:'',theme:'改变'},
    {en:'modify',pos:'v.',cn:'修改，调整（较正式）',rare:'僻：（语法）修饰',theme:'改变'},
    {en:'transform',pos:'v.',cn:'使彻底改观，使转化',rare:'',theme:'改变'},
    {en:'shift',pos:'v.&n.',cn:'转变，转移（shift towards）',rare:'僻：n. 轮班；换挡',theme:'改变'},
    {en:'common',pos:'adj.',cn:'常见的，普遍的',rare:'',theme:'普遍的/常见的'},
    {en:'widespread',pos:'adj.',cn:'广泛流传的，普遍的',rare:'',theme:'普遍的/常见的'},
    {en:'prevalent',pos:'adj.',cn:'盛行的，普遍存在的（正式）',rare:'僻：（罕用）优势的',theme:'普遍的/常见的'},
    {en:'ubiquitous',pos:'adj.',cn:'无处不在的（正式）',rare:'',theme:'普遍的/常见的'},
    {en:'pervasive',pos:'adj.',cn:'弥漫的，遍布的（常含负面）',rare:'僻：（罕用）有渗透力的',theme:'普遍的/常见的'},
    {en:'fast',pos:'adj.&adv.',cn:'快的，快速地',rare:'',theme:'快速的'},
    {en:'rapid',pos:'adj.',cn:'迅速的，快速的（rapid growth）',rare:'',theme:'快速的'},
    {en:'swift',pos:'adj.',cn:'迅速的，敏捷的（swift action）',rare:'僻：n. 雨燕（鸟）',theme:'快速的'},
    {en:'prompt',pos:'adj.&v.',cn:'迅速的，及时的（prompt action）',rare:'僻：v. 促使，提示；n. 提示符',theme:'快速的'},
    {en:'exponential',pos:'adj.',cn:'指数的，爆炸式增长的（exponential growth）',rare:'僻：（数学）指数的（本义）',theme:'快速的'}
  ]};

  const weakDefaults=[
    '口语流利度：填充词过多、停顿',
    '口语 Part 2 素材储备不足',
    '写作论证空泛、缺具体例证',
    '写作句式单一，缺高分句型',
    '听力 Section 3/4 正确率低',
    '阅读同义替换识别慢'
  ];

  const checklistItems=[
    '立场明确：开头段清楚表达同意/不同意',
    '每段有 topic sentence（段首句）',
    '每个论点都有具体例证（不是空泛说理）',
    '使用了至少3种高分句型（复合句/倒装/强调句）',
    '连接词多样：不是只有 firstly/secondly',
    '检查了语法错误（主谓一致、时态、单复数）',
    '字数达标（Task 2 ≥ 250词）'
  ];

  const CAMBOOKS=[4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20];
  const SKILLS=[['L','听力'],['R','阅读'],['W','写作'],['S','口语']];
  const SKILL_NAMES={L:'听力',R:'阅读',W:'写作',S:'口语'};

  function blank(){ return {days:{},streak:0,lastDone:null,essays:0,sentences:[],mocks:[],reviews:[],records:[],books:[DEFAULT_BOOK],activeBook:'default',dailyCount:20,voc2:{default:{}},camb:{},weak:null}; }

  function load(){
    let s=null;
    try{ s=JSON.parse(localStorage.getItem(KEY)); }catch(e){}
    if(!s){
      try{
        const v2=JSON.parse(localStorage.getItem('ielts_system_v2'));
        if(v2){
          s=Object.assign(blank(),v2);
          s.books=[DEFAULT_BOOK];
          s.activeBook='default';
          s.dailyCount=20;
          s.voc2={default:(v2.voc||{})};
          s.camb={};
        }
      }catch(e){}
    }
    if(!s){
      try{
        const v1=JSON.parse(localStorage.getItem('ielts_system_v1'));
        if(v1) s=Object.assign(blank(),v1);
      }catch(e){}
    }
    if(!s) s=blank();
    if(!s.books||!s.books.length) s.books=[DEFAULT_BOOK];
    if(!s.voc2) s.voc2={};
    if(!s.voc2[s.activeBook]) s.voc2[s.activeBook]={};
    if(!s.camb) s.camb={};
    if(!s.dailyCount) s.dailyCount=20;
    if(!s.weak) s.weak=null;
    return s;
  }
  let state=load();
  function save(){ localStorage.setItem(KEY,JSON.stringify(state)); }

  function ensureToday(){
    if(!state.days[today]) state.days[today]={tasks:JSON.parse(JSON.stringify(defaultTasks)),doneCount:0,totalMins:0};
    return state.days[today];
  }
  function curBook(){ return state.books.find(b=>b.id===state.activeBook)||state.books[0]; }

  function renderTasks(){
    const d=ensureToday();
    const el=document.getElementById('taskList');
    el.innerHTML='';
    d.tasks.forEach(t=>{
      const row=document.createElement('div');
      row.className='task'+(t.done?' done':'');
      row.innerHTML='<div class="checkbox">'+(t.done?'✓':'')+'</div><div class="task-name">'+t.name+'</div><div class="task-mins">'+t.mins+'分钟</div>';
      row.onclick=()=>{
        t.done=!t.done;
        if(t.done){d.doneCount++;d.totalMins+=t.mins;}else{d.doneCount--;d.totalMins-=t.mins;}
        save();renderTasks();renderDailyStats();renderSkillBars();
      };
      el.appendChild(row);
    });
    document.getElementById('todayProgress').textContent=d.doneCount+' / '+d.tasks.length;
    document.getElementById('dotToday').style.background=(d.doneCount===d.tasks.length)?'#22c55e':'#f43f5e';
  }
  window.addTask=function(){
    const name=document.getElementById('newTaskName').value.trim();
    const mins=parseInt(document.getElementById('newTaskMins').value)||30;
    if(!name)return;
    ensureToday().tasks.push({id:'c'+Date.now(),name,mins,skill:'other',done:false});
    document.getElementById('newTaskName').value='';
    document.getElementById('newTaskMins').value='';
    save();renderTasks();
  };
  window.finishDay=function(){
    const d=ensureToday();
    if(d.doneCount===0){showToast('先完成至少1个任务哦');return;}
    if(state.lastDone!==today){
      state.streak=(state.lastDone===yesterday)?state.streak+1:1;
      state.lastDone=today;
    }
    save();renderHeader();showToast('打卡成功！明天继续 🔥');
  };
  function renderHeader(){
    document.getElementById('streakNum').textContent=state.streak;
    document.getElementById('totalDone').textContent=Object.keys(state.days).filter(k=>state.days[k].doneCount>0).length;
  }

  let remain=1500,ticking=null;
  function fmt(s){return String(Math.floor(s/60)).padStart(2,'0')+':'+String(s%60).padStart(2,'0');}
  function updTimer(){document.getElementById('timerDisplay').textContent=fmt(remain);}
  window.adjustTime=function(s){remain=Math.max(60,remain+s);updTimer();};
  window.toggleTimer=function(){
    const btn=document.getElementById('startBtn');
    if(ticking){clearInterval(ticking);ticking=null;btn.textContent='▶';}
    else{
      btn.textContent='⏸';
      ticking=setInterval(()=>{
        remain--;updTimer();
        if(remain<=0){clearInterval(ticking);ticking=null;btn.textContent='▶';showToast('⏰ 时间到！休息一下');remain=1500;updTimer();}
      },1000);
    }
  };
  window.resetTimer=function(){clearInterval(ticking);ticking=null;remain=1500;updTimer();document.getElementById('startBtn').textContent='▶';};

  let lastTopic=-1,recStart=null;
  window.drawTopic=function(){
    let i;do{i=Math.floor(Math.random()*topics.length);}while(i===lastTopic);
    lastTopic=i;
    document.getElementById('topicCard').innerHTML='<div class="q">🗣 '+topics[i].q+'</div><div class="hint">'+topics[i].hint+'</div>';
    document.getElementById('recordBtn').disabled=false;
  };
  window.mockRecord=function(){
    const btn=document.getElementById('recordBtn');
    if(!recStart){
      recStart=Date.now();
      btn.textContent='⏺ 录音中... 点击停止';
      btn.style.background='#f43f5e';btn.style.color='#fff';
    }else{
      const dur=Math.round((Date.now()-recStart)/1000);
      recStart=null;
      btn.textContent='⏺ 开始录音';btn.style.background='';btn.style.color='';
      state.records.unshift({date:today,topic:topics[lastTopic].q,dur});
      if(state.records.length>10)state.records.pop();
      save();renderRecords();showToast('已记录录音 '+dur+'秒');
    }
  };
  function renderRecords(){
    const el=document.getElementById('recordList');
    if(!state.records.length){el.innerHTML='<div class="empty">还没有录音，先抽题开始吧</div>';return;}
    el.innerHTML='';
    state.records.forEach(r=>{
      const d=document.createElement('div');
      d.className='row';
      d.style.cssText='display:flex;justify-content:space-between;padding:9px 4px;border-bottom:1px solid #0f172a;font-size:13px;';
      d.innerHTML='<span style="flex:1;padding-right:8px;">'+r.topic+'</span><span style="color:#64748b;white-space:nowrap;">'+r.dur+'s · '+r.date.slice(5)+'</span>';
      el.appendChild(d);
    });
  }

  let checks=checklistItems.map(()=>false);
  function renderChecklist(){
    const el=document.getElementById('checklist');
    el.innerHTML='';
    checklistItems.forEach((c,i)=>{
      const row=document.createElement('div');
      row.className='check-item'+(checks[i]?' done':'');
      row.innerHTML='<div class="checkbox">'+(checks[i]?'✓':'')+'</div><div>'+c+'</div>';
      row.onclick=()=>{checks[i]=!checks[i];renderChecklist();};
      el.appendChild(row);
    });
  }
  window.resetChecklist=function(){checks=checklistItems.map(()=>false);renderChecklist();};
  window.logEssay=function(){
    if(!checks.every(Boolean)){showToast('清单还没全绿，逐项检查后再记录');return;}
    state.essays++;save();showToast('第 '+state.essays+' 篇作文完成 🎉');resetChecklist();renderDailyStats();
  };
  window.addSentence=function(){
    const v=document.getElementById('newSentence').value.trim();
    if(!v)return;
    state.sentences.push(v);
    document.getElementById('newSentence').value='';
    save();renderSentences();
  };
  function renderSentences(){
    document.getElementById('sentenceCount').textContent=state.sentences.length;
    const el=document.getElementById('sentenceList');
    if(!state.sentences.length){el.className='empty';el.textContent='例：I think → It is widely believed that... / From my perspective,...';return;}
    el.className='';
    el.innerHTML=state.sentences.map(s=>'<div class="check-item"><div class="checkbox" style="background:#22c55e;border-color:#22c55e;">✓</div><div>'+s+'</div></div>').join('');
  }

  function renderDailyStats(){
    let totalMins=0,totalTasks=0;
    Object.values(state.days).forEach(d=>{totalMins+=d.totalMins;totalTasks+=d.doneCount;});
    document.getElementById('statDays').textContent=Object.keys(state.days).filter(k=>state.days[k].doneCount>0).length;
    document.getElementById('statHours').textContent=(totalMins/60).toFixed(1);
    document.getElementById('statTasks').textContent=totalTasks;
    document.getElementById('statEssays').textContent=state.essays;

    const chart=document.getElementById('chart');
    chart.innerHTML='';
    const days=[];
    for(let i=13;i>=0;i--){
      const d=new Date(Date.now()-i*864e5).toISOString().slice(0,10);
      days.push({date:d,mins:state.days[d]?state.days[d].totalMins:0});
    }
    const max=Math.max.apply(null,days.map(x=>x.mins).concat([30]));
    days.forEach(x=>{
      const bar=document.createElement('div');
      bar.className='bar';
      bar.style.height=Math.max(4,(x.mins/max)*100)+'%';
      bar.title=x.date+'：'+x.mins+' 分钟';
      if(x.mins>0)bar.innerHTML='<div class="val">'+x.mins+'</div>';
      bar.innerHTML+='<div class="date">'+x.date.slice(8)+'</div>';
      chart.appendChild(bar);
    });
  }
  function renderSkillBars(){
    const skills={speak:['口语','#f472b6'],write:['写作','#fb923c'],listen:['听力','#38bdf8'],read:['阅读','#34d399'],vocab:['词汇','#a78bfa'],other:['其他','#94a3b8']};
    const acc={};let total=0;
    Object.values(state.days).forEach(d=>d.tasks.forEach(t=>{if(t.done){acc[t.skill]=(acc[t.skill]||0)+t.mins;total+=t.mins;}}));
    const el=document.getElementById('skillBars');
    el.innerHTML='';
    const sw=(acc.speak||0)+(acc.write||0);
    const pct=total?Math.round(sw/total*100):0;
    const tip=document.createElement('div');
    tip.style.cssText='font-size:12px;margin-bottom:10px;color:'+(pct>=50?'#22c55e':'#fbbf24')+';';
    tip.textContent='听说写输出占比：'+pct+'%（目标 ≥ 50%）'+(pct>=50?' ✓':' — 再多说多写一点');
    el.appendChild(tip);
    Object.keys(skills).forEach(k=>{
      const mins=acc[k]||0;
      const p=total?Math.round(mins/total*100):0;
      const row=document.createElement('div');
      row.className='skill-row';
      row.innerHTML='<div class="lbl">'+skills[k][0]+'</div><div class="skill-bar"><div class="skill-fill" style="width:'+p+'%;background:'+skills[k][1]+';"></div></div><div style="font-size:12px;color:#64748b;width:70px;text-align:right;">'+mins+'分</div>';
      el.appendChild(row);
    });
    if(!total)el.innerHTML+='<div class="empty">完成今天的任务后，这里会出现分布</div>';
  }

  function renderTrend(){
    const el=document.getElementById('trendChart');
    const ms=state.mocks.slice(0,10).reverse();
    if(ms.length<2){el.innerHTML='<div class="empty">记录2次以上模考后自动生成趋势曲线</div>';return;}
    const W=600,H=210,P=30;
    const yOf=s=>H-P-((s-4)/5)*(H-2*P);
    const xOf=i=>P+i*((W-2*P)/Math.max(ms.length-1,1));
    let svg='<svg viewBox="0 0 '+W+' '+H+'" style="width:100%;height:auto;">';
    [5,6,7,8].forEach(g=>{
      svg+='<line x1="'+P+'" y1="'+yOf(g)+'" x2="'+(W-P)+'" y2="'+yOf(g)+'" stroke="#334155" stroke-width="1" stroke-dasharray="3,3"/>';
      svg+='<text x="'+(P-6)+'" y="'+(yOf(g)+4)+'" fill="#64748b" font-size="10" text-anchor="end">'+g+'</text>';
    });
    const series=[{k:'l',c:'#38bdf8'},{k:'r',c:'#34d399'},{k:'w',c:'#fb923c'},{k:'s',c:'#f472b6'}];
    series.forEach(sr=>{
      const pts=ms.map((m,i)=>xOf(i)+','+yOf(m[sr.k])).join(' ');
      svg+='<polyline points="'+pts+'" fill="none" stroke="'+sr.c+'" stroke-width="2" stroke-linejoin="round"/>';
      ms.forEach((m,i)=>{svg+='<circle cx="'+xOf(i)+'" cy="'+yOf(m[sr.k])+'" r="3" fill="'+sr.c+'"/>';});
    });
    const avgPts=ms.map((m,i)=>xOf(i)+','+yOf((m.l+m.r+m.w+m.s)/4)).join(' ');
    svg+='<polyline points="'+avgPts+'" fill="none" stroke="#e2e8f0" stroke-width="2" stroke-dasharray="5,4"/>';
    ms.forEach((m,i)=>{svg+='<text x="'+xOf(i)+'" y="'+(H-8)+'" fill="#64748b" font-size="9" text-anchor="middle">'+(i+1)+'</text>';});
    svg+='</svg>';
    el.innerHTML=svg;
  }
  window.addMock=function(){
    const v=function(id){return parseFloat(document.getElementById(id).value);};
    const l=v('mL'),r=v('mR'),w=v('mW'),s=v('mS');
    if([l,r,w,s].some(isNaN)){showToast('四项都要填哦');return;}
    state.mocks.unshift({date:today,l,r,w,s,avg:((l+r+w+s)/4).toFixed(1)});
    ['mL','mR','mW','mS'].forEach(function(id){document.getElementById(id).value='';});
    save();renderMocks();renderTrend();showToast('模考已记录，趋势曲线已更新');
  };
  function renderMocks(){
    const el=document.getElementById('mockHistory');
    if(!state.mocks.length){el.innerHTML='<div class="empty">还没有模考记录</div>';return;}
    el.innerHTML='';
    state.mocks.slice(0,8).forEach(m=>{
      const row=document.createElement('div');
      row.className='row';
      row.innerHTML='<span style="color:#64748b;">'+m.date+'</span><span>听 '+m.l+' · 读 '+m.r+' · 写 '+m.w+' · 口 '+m.s+'</span><b style="color:#38bdf8;">均 '+m.avg+'</b>';
      el.appendChild(row);
    });
  }

  function buildCamList(){
    const el=document.getElementById('camList');
    el.innerHTML='';
    CAMBOOKS.forEach(b=>{
      const det=document.createElement('details');
      det.className='cam-book';
      const sum=document.createElement('summary');
      sum.innerHTML='剑'+b+'<span class="cam-meta" id="cmeta-'+b+'"></span>';
      det.appendChild(sum);
      for(let t=1;t<=4;t++){
        const td=document.createElement('details');
        td.className='cam-test';
        const ts=document.createElement('summary');
        ts.innerHTML='Test '+t+'<span class="cam-meta" id="cmeta-'+b+'-'+t+'"></span>';
        td.appendChild(ts);
        const body=document.createElement('div');
        body.className='cam-body';
        body.setAttribute('data-bt',b+'-'+t);
        td.appendChild(body);
        det.appendChild(td);
      }
      el.appendChild(det);
    });
    el.addEventListener('toggle',e=>{
      const t=e.target;
      if(t.classList&&t.classList.contains('cam-test')&&t.open){
        const body=t.querySelector('.cam-body');
        if(!body.innerHTML) buildCamBody(body.getAttribute('data-bt'));
      }
      if(t.classList&&t.classList.contains('cam-book')) renderCamMeta();
    },true);
  }
  function buildCamBody(bt){
    const div=document.querySelector('.cam-body[data-bt="'+bt+'"]');
    let h='';
    SKILLS.forEach(sk=>{
      const r=state.camb[bt+'-'+sk[0]]||{date:'',dur:'',score:'',note:''};
      const note=(r.note||'').replace(/"/g,'&quot;');
      h+='<div class="cam-row"><span class="sl">'+sk[1]+'</span>'
        +'<input type="date" id="cd-'+bt+'-'+sk[0]+'" value="'+(r.date||'')+'">'
        +'<input type="number" id="cm-'+bt+'-'+sk[0]+'" placeholder="时长分" value="'+(r.dur||'')+'" min="0">'
        +'<input type="number" id="cs-'+bt+'-'+sk[0]+'" placeholder="分数" value="'+(r.score||'')+'" step="0.5" min="0" max="9">'
        +'<input type="text" id="cn-'+bt+'-'+sk[0]+'" placeholder="复盘：错因 / 收获 / 改进点…" value="'+note+'">'
        +'</div>';
    });
    h+='<button class="btn" style="padding:8px 20px;font-size:13px;" onclick="saveCamb(\''+bt+'\')">💾 保存本套</button>';
    div.innerHTML=h;
  }
  window.saveCamb=function(bt){
    SKILLS.forEach(sk=>{
      const date=document.getElementById('cd-'+bt+'-'+sk[0]).value;
      const dur=document.getElementById('cm-'+bt+'-'+sk[0]).value;
      const score=document.getElementById('cs-'+bt+'-'+sk[0]).value;
      const note=document.getElementById('cn-'+bt+'-'+sk[0]).value;
      if(date||dur||score||note){
        state.camb[bt+'-'+sk[0]]={date,dur,score,note};
      }
    });
    save();renderCamMeta();renderCamSummary();showToast('剑'+bt.split('-')[0]+' Test '+bt.split('-')[1]+' 已保存');
  };
  function camRowStat(bt){
    let n=0,sum=0;
    SKILLS.forEach(sk=>{
      const r=state.camb[bt+'-'+sk[0]];
      if(r&&r.score!==''){n++;sum+=parseFloat(r.score);}
    });
    return {n,avg:n?(sum/n).toFixed(1):null};
  }
  function renderCamMeta(){
    CAMBOOKS.forEach(b=>{
      let bn=0,bsum=0;
      for(let t=1;t<=4;t++){
        const st=camRowStat(b+'-'+t);
        const span=document.getElementById('cmeta-'+b+'-'+t);
        if(span) span.textContent=st.n?('已练 '+st.n+' 项 · 均分 '+st.avg):'';
        bn+=st.n;
        if(st.avg)bsum+=parseFloat(st.avg)*st.n;
      }
      const span=document.getElementById('cmeta-'+b);
      if(span) span.textContent=bn?('共 '+bn+' 项 · 均分 '+(bsum/bn).toFixed(1)):'';
    });
  }
  function camAgg(){
    const agg={L:{n:0,s:0},R:{n:0,s:0},W:{n:0,s:0},S:{n:0,s:0}};
    let tests=0;
    CAMBOOKS.forEach(b=>{
      let any=false;
      for(let t=1;t<=4;t++){
        SKILLS.forEach(sk=>{
          const r=state.camb[b+'-'+t+'-'+sk[0]];
          if(r&&r.score!==''){agg[sk[0]].n++;agg[sk[0]].s+=parseFloat(r.score);any=true;}
        });
      }
      if(any)tests+=1;
    });
    return {agg,tests};
  }
  function renderCamSummary(){
    const el=document.getElementById('camSummary');
    const r=camAgg();
    if(!r.tests){el.innerHTML='<div class="empty">还没有剑雅练习记录</div>';return;}
    let h='<div class="cam-summary-line">覆盖 <b>'+r.tests+'</b> 本真题书 · ';
    SKILLS.forEach(sk=>{
      const a=r.agg[sk[0]];
      if(a.n){
        const avg=(a.s/a.n).toFixed(1);
        h+=sk[1]+' <b class="'+(avg<6?'warn':'')+'">'+avg+'</b>（'+a.n+'套）　';
      }else h+=sk[1]+' —　';
    });
    h+='</div>';
    h+='<div class="cam-summary-line" style="margin-top:6px;font-size:12px;color:#64748b;">';
    CAMBOOKS.forEach(b=>{
      const parts=[];
      SKILLS.forEach(sk=>{
        let n=0,ss=0;
        for(let t=1;t<=4;t++){
          const rr=state.camb[b+'-'+t+'-'+sk[0]];
          if(rr&&rr.score!==''){n++;ss+=parseFloat(rr.score);}
        }
        if(n)parts.push(sk[1]+(ss/n).toFixed(1));
      });
      if(parts.length)h+='剑'+b+'：'+parts.join(' / ')+'　';
    });
    h+='</div>';
    el.innerHTML=h;
  }

  function ensureWeak(){
    if(!state.weak) state.weak=weakDefaults.map((w,i)=>({id:'w'+i,name:w,log:[]}));
  }
  function renderWeak(){
    ensureWeak();
    const el=document.getElementById('weakList');
    el.innerHTML='';
    state.weak.forEach(wk=>{
      const latest=wk.log.length?wk.log[wk.log.length-1].status:-1;
      const dotColor=latest===0?'#f43f5e':latest===1?'#fbbf24':latest===2?'#22c55e':'#334155';
      const row=document.createElement('div');
      row.className='weak-row';
      let segs='';
      const last8=wk.log.slice(-8);
      for(let i=0;i<8;i++){
        const c=last8[i]?last8[i].status:null;
        segs+='<div class="seg'+(c===null?'':' s'+c)+'"></div>';
      }
      row.innerHTML=
        '<div class="weak-top"><div class="status-dot" style="background:'+dotColor+';"></div><div class="wt">'+wk.name+'</div></div>'+
        '<div class="weak-ctl"><select id="sel-'+wk.id+'">'+
          '<option value="0"'+(latest===0?' selected':'')+'>🔴 未改善</option>'+
          '<option value="1"'+(latest===1?' selected':'')+'>🟡 部分改善</option>'+
          '<option value="2"'+(latest===2?' selected':'')+'>🟢 已改善</option>'+
        '</select><input id="note-'+wk.id+'" placeholder="备注（可选），如：填充词明显变少"><button class="btn secondary" style="padding:7px 14px;font-size:12px;" data-wid="'+wk.id+'">记录</button></div>'+
        '<div class="weak-hist">'+segs+'<span class="hl">'+(wk.log.length?('已记录 '+wk.log.length+' 次，最近 '+wk.log[wk.log.length-1].date):'暂无记录')+'</span></div>';
      el.appendChild(row);
    });
    el.querySelectorAll('button[data-wid]').forEach(btn=>{
      btn.onclick=()=>{
        const wid=btn.getAttribute('data-wid');
        const st=parseInt(document.getElementById('sel-'+wid).value);
        const note=document.getElementById('note-'+wid).value.trim();
        const wk=state.weak.find(x=>x.id===wid);
        wk.log.push({date:today,status:st,note:note});
        save();renderWeak();showToast('弱点状态已更新');
      };
    });
  }
  window.addWeak=function(){
    ensureWeak();
    const v=document.getElementById('newWeak').value.trim();
    if(!v)return;
    state.weak.push({id:'w'+Date.now(),name:v,log:[]});
    document.getElementById('newWeak').value='';
    save();renderWeak();
  };

  function ensureVocId(bid){
    const book=state.books.find(b=>b.id===bid);
    if(!book)return;
    if(!state.voc2[bid])state.voc2[bid]={};
    book.words.forEach((w,wi)=>{
      if(!state.voc2[bid][wi])state.voc2[bid][wi]={box:0,due:today,right:0,wrong:0};
    });
  }
  function vocCounts(){
    ensureVocId(state.activeBook);
    const voc=state.voc2[state.activeBook];
    const book=curBook();
    let due=0,master=0,neu=0;
    book.words.forEach((w,wi)=>{
      const v=voc[wi];
      if(v.box>=6){master++;return;}
      if(v.due<=today){due++;if(v.box===0)neu++;}
    });
    return {total:book.words.length,due,master,neu};
  }
  function renderVocStats(){
    const c=vocCounts();
    document.getElementById('vocTotal').textContent=c.total;
    document.getElementById('vocNew').textContent=c.neu;
    document.getElementById('vocDue').textContent=c.due-c.neu;
    document.getElementById('vocMaster').textContent=c.master;
    const btn=document.getElementById('quizStartBtn');
    btn.textContent=c.due>0?('▶ 开始今日学习（'+c.due+' 词到期）'):'✨ 今日队列已清空，明天再来';
    document.getElementById('dailyCount').value=String(state.dailyCount);
    document.getElementById('bookInfo').textContent='当前词书：《'+curBook().name+'》';
  }
  window.setCount=function(v){state.dailyCount=parseInt(v);save();showToast('每日记忆量已设为 '+v+' 词');};
  function renderBookChips(){
    const el=document.getElementById('bookChips');
    el.innerHTML='';
    state.books.forEach(b=>{
      const chip=document.createElement('span');
      chip.className='book-chip'+(b.id===state.activeBook?' sel':'');
      let h='📕 '+b.name+'（'+b.words.length+'词）';
      if(b.id!=='default')h+='<span class="del" data-del="'+b.id+'">✕</span>';
      chip.innerHTML=h;
      chip.onclick=e=>{
        const del=e.target.getAttribute&&e.target.getAttribute('data-del');
        if(del){
          if(!confirm('删除词书《'+b.name+'》？其中的记忆进度将一并删除。'))return;
          state.books=state.books.filter(x=>x.id!==del);
          delete state.voc2[del];
          if(state.activeBook===del)state.activeBook='default';
          save();renderBookChips();renderVocStats();renderChips();renderBrowser();showToast('词书已删除');
          return;
        }
        state.activeBook=b.id;save();
        renderBookChips();renderVocStats();renderChips();renderBrowser();
      };
      el.appendChild(chip);
    });
  }
  window.importBook=function(){
    const name=document.getElementById('bookName').value.trim()||('导入词书 '+state.books.length);
    const text=document.getElementById('bookText').value;
    const words=[];
    text.split('\n').forEach(line=>{
      line=line.trim();
      if(!line)return;
      const t=line.split(/\s+/);
      const en=t[0];
      let pos='';
      if(t.length>1&&/^(n|v|vt|vi|adj|adv|prep|conj|pron|num|art|phr|aux|int)[\.\．]?$/i.test(t[1]))pos=t[1].replace(/[\.\．]/,'');
      const cn=t.slice(pos?2:1).join(' ');
      if(en&&cn)words.push({en,pos,cn,rare:'',theme:''});
    });
    if(!words.length){showToast('未解析到有效单词，格式：单词 词性 释义');return;}
    const id='b'+Date.now();
    state.books.push({id,name,words});
    state.voc2[id]={};
    state.activeBook=id;
    document.getElementById('bookName').value='';
    document.getElementById('bookText').value='';
    ensureVocId(id);save();
    renderBookChips();renderVocStats();filterSet=-1;renderChips();renderBrowser();
    showToast('已导入《'+name+'》 '+words.length+' 词');
  };
  function renderChips(){
    const el=document.getElementById('setChips');
    el.innerHTML='';
    const book=curBook();
    const themes=[];
    book.words.forEach(w=>{if(w.theme&&themes.indexOf(w.theme)<0)themes.push(w.theme);});
    if(!themes.length){el.style.display='none';return;}
    el.style.display='block';
    const all=document.createElement('span');
    all.className='set-chip'+(filterSet===-1?' sel':'');
    all.textContent='全部';
    all.onclick=()=>{filterSet=-1;renderChips();renderBrowser();};
    el.appendChild(all);
    themes.forEach(th=>{
      const chip=document.createElement('span');
      chip.className='set-chip'+(filterSet===th?' sel':'');
      chip.textContent=th;
      chip.onclick=()=>{filterSet=th;renderChips();renderBrowser();};
      el.appendChild(chip);
    });
  }
  let filterSet=-1;
  function renderBrowser(){
    const el=document.getElementById('wordBrowser');
    const book=curBook();
    el.innerHTML='';
    book.words.forEach(w=>{
      if(filterSet!==-1&&w.theme!==filterSet)return;
      const card=document.createElement('div');
      card.className='word-card';
      card.innerHTML='<span class="en">'+w.en+'</span><span class="pos">'+w.pos+'</span><span class="cn">'+w.cn+'</span>';
      el.appendChild(card);
    });
    if(!el.children.length)el.innerHTML='<div class="empty">该筛选条件下没有单词</div>';
  }

  let quiz={queue:[],idx:0,right:0,wrong:0,dirToggle:0};
  function shuffle(a){
    for(let i=a.length-1;i>0;i--){const j=Math.floor(Math.random()*(i+1));const t=a[i];a[i]=a[j];a[j]=t;}
    return a;
  }
  window.startQuiz=function(){
    ensureVocId(state.activeBook);
    const voc=state.voc2[state.activeBook];
    const book=curBook();
    const q=shuffle(book.words.map((w,i)=>i).filter(i=>voc[i].box<6&&voc[i].due<=today)).slice(0,state.dailyCount);
    if(!q.length){showToast('今日没有到期单词');return;}
    quiz={queue:q,idx:0,right:0,wrong:0,dirToggle:0};
    renderQuiz();
  };
  function nextDue(box){
    const d=new Date(Date.now()+IV[box]*864e5);
    return d.toISOString().slice(0,10);
  }
  function renderQuiz(){
    const area=document.getElementById('quizArea');
    const book=curBook();
    if(quiz.idx>=quiz.queue.length){
      save();renderVocStats();
      area.innerHTML='<div class="card"><div class="quiz-card"><h3 style="margin-bottom:10px;">🎉 本轮完成</h3>'+
        '<div style="font-size:15px;line-height:2;">答对 <b style="color:#22c55e;">'+quiz.right+'</b> · 答错 <b style="color:#f43f5e;">'+quiz.wrong+'</b> · 正确率 '+Math.round(quiz.right/Math.max(1,quiz.right+quiz.wrong)*100)+'%</div>'+
        '<div style="font-size:12px;color:#94a3b8;margin:8px 0 14px;">答错的词已安排明天重新出现</div>'+
        '<button class="btn" onclick="startQuiz()">再来一轮</button></div></div>';
      return;
    }
    const wi=quiz.queue[quiz.idx];
    const w=book.words[wi];
    const isCN2EN=(quiz.dirToggle++%2===0);
    const n=quiz.idx+1;
    let body='';
    if(isCN2EN){
      body='<div class="quiz-q">'+w.cn+'<small>请拼写出对应的英文单词</small></div>'+
        '<input class="quiz-input" id="quizAns" placeholder="输入英文单词" autocomplete="off">'+
        '<button class="btn" onclick="submitQuiz()">提交</button>';
    }else{
      const others=shuffle(book.words.map((x,i)=>i).filter(i=>i!==wi)).slice(0,3).map(i=>book.words[i]);
      const opts=shuffle([{t:w.cn,ok:1}].concat(others.map(o=>({t:o.cn,ok:0}))));
      body='<div class="quiz-q">'+w.en+'<small>选出正确的中文释义（'+w.pos+'）</small></div>'+
        opts.map((o,i)=>'<button class="opt" data-ok="'+o.ok+'">'+String.fromCharCode(65+i)+'. '+o.t+'</button>').join('');
    }
    area.innerHTML='<div class="card"><div class="quiz-card">'+
      '<div style="display:flex;justify-content:space-between;font-size:12px;color:#64748b;margin-bottom:8px;"><span>第 '+n+' / '+quiz.queue.length+' 题</span><span>'+(isCN2EN?'📝 中译英':'🔍 英译中')+'</span></div>'+
      '<div class="quiz-dir">'+(isCN2EN?'看中文 → 拼英文':'看英文 → 选中文')+'</div>'+
      body+
      '<div class="quiz-fb" id="quizFb"></div>'+
      '<button class="btn secondary" id="nextBtn" style="display:none;margin-top:4px;" onclick="nextQuiz()">下一题 →</button>'+
      '</div></div>';
    if(!isCN2EN){
      area.querySelectorAll('.opt').forEach(btn=>{
        btn.onclick=()=>{
          const ok=btn.getAttribute('data-ok')==='1';
          area.querySelectorAll('.opt').forEach(b=>{b.style.pointerEvents='none';if(b.getAttribute('data-ok')==='1')b.classList.add('correct');});
          if(!ok)btn.classList.add('wrongpick');
          grade(wi,ok,w);
        };
      });
    }else{
      const inp=document.getElementById('quizAns');
      inp.focus();
      inp.addEventListener('keydown',e=>{if(e.key==='Enter')submitQuiz();});
    }
  }
  window.submitQuiz=function(){
    const wi=quiz.queue[quiz.idx];
    const w=curBook().words[wi];
    const inp=document.getElementById('quizAns');
    const val=(inp.value||'').trim().toLowerCase();
    const ok=(val===w.en.toLowerCase());
    grade(wi,ok,w,val);
  };
  function grade(wi,ok,w,val){
    const voc=state.voc2[state.activeBook];
    const v=voc[wi];
    if(ok){v.right++;v.box=Math.min(6,v.box+1);v.due=nextDue(v.box);quiz.right++;}
    else{v.wrong++;v.box=Math.max(0,v.box-2);v.due=nextDue(1);quiz.wrong++;}
    save();
    const fb=document.getElementById('quizFb');
    fb.style.display='block';
    fb.innerHTML='<div class="'+(ok?'ok':'no')+'">'+(ok?'✓ 答对，已晋级到记忆盒 '+state.voc2[state.activeBook][wi].box:'✗ 答错，正确答案是 <b>'+w.en+'</b>'+(val?'（你写的：'+val+'）':''))+'</div>'+
      '<div style="margin-top:6px;"><b>常</b> '+w.cn+'</div>'+
      (w.rare?'<div class="rare" style="margin-top:4px;"><b>僻</b> '+w.rare+'</div>':'')+
      '<div style="font-size:11px;color:#64748b;margin-top:6px;">下次复习：'+state.voc2[state.activeBook][wi].due+'</div>';
    document.getElementById('nextBtn').style.display='inline-block';
    const submit=document.getElementById('quizAns');
    if(submit)submit.disabled=true;
    renderVocStats();
  }
  window.nextQuiz=function(){
    quiz.idx++;
    renderQuiz();
    document.getElementById('quizArea').scrollIntoView({behavior:'smooth',block:'center'});
  };

  let lastAnalysis=null;
  function sinceDate(){
    if(state.reviews.length) return state.reviews[0].date;
    return new Date(Date.now()-7*864e5).toISOString().slice(0,10);
  }
  window.genAnalysis=function(){
    const since=sinceDate();
    const agg={L:{n:0,s:0},R:{n:0,s:0},W:{n:0,s:0},S:{n:0,s:0}};
    let tests=new Set();
    Object.keys(state.camb).forEach(k=>{
      const r=state.camb[k];
      if(r&&r.date&&r.date>=since&&r.score!==''){
        const sk=k.slice(-1);
        agg[sk].n++;agg[sk].s+=parseFloat(r.score);
        tests.add(k.slice(0,-2));
      }
    });
    const el=document.getElementById('autoAnalysis');
    if(!tests.size){
      el.innerHTML='<div class="empty">自 '+since+' 以来还没有剑雅练习记录</div>';
      lastAnalysis=null;
      document.getElementById('btnFillRv3').disabled=true;
      document.getElementById('btnSyncWeak').disabled=true;
      return;
    }
    let html='<div class="cam-summary-line">统计区间：<b>'+since+'</b> 至今 · 完成 <b>'+tests.size+'</b> 套真题练习</div>';
    const rows=[];
    SKILLS.forEach(sk=>{
      const a=agg[sk[0]];
      if(a.n){
        const avg=(a.s/a.n).toFixed(1);
        rows.push({sk:sk[0],name:sk[1],n:a.n,avg:parseFloat(avg)});
        html+='<div class="cam-summary-line">'+sk[1]+'：<b class="'+(avg<6?'warn':'')+'">'+avg+'</b>（'+a.n+'套）'+(avg<6?' ⚠️ 低于6.0，建议加大投入':'')+'</div>';
      }else{
        html+='<div class="cam-summary-line">'+sk[1]+'：本周未练</div>';
      }
    });
    const done=rows.slice().sort((a,b)=>a.avg-b.avg)[0];
    if(done){
      const gap=(6.5-done.avg).toFixed(1);
      html+='<div class="cam-summary-line" style="margin-top:6px;">📌 最弱科目：<b class="warn">'+done.name+'</b>（'+done.avg+'，距6.5还差 '+gap+' 分）→ 下周重心建议向'+done.name+'倾斜</div>';
    }
    el.innerHTML=html;
    lastAnalysis={since,rows,tests:tests.size};
    document.getElementById('btnFillRv3').disabled=false;
    document.getElementById('btnSyncWeak').disabled=false;
  };
  window.fillRv3=function(){
    if(!lastAnalysis)return;
    let t='【系统抓取·剑雅练习数据 '+lastAnalysis.since+' 至今】\n';
    t+='共完成 '+lastAnalysis.tests+' 套真题练习。\n';
    lastAnalysis.rows.forEach(r=>{
      t+=r.name+'：'+r.n+'套，均分'+r.avg.toFixed(1)+(r.avg<6?'（低于6.0，需加强）':'')+'\n';
    });
    const weakest=lastAnalysis.rows.slice().sort((a,b)=>a.avg-b.avg)[0];
    t+='最弱科目：'+weakest.name+'，下周建议：增加'+weakest.name+'练习量，优先处理该科错题。';
    document.getElementById('rv3').value=t;
    showToast('已写入复盘第3题');
  };
  window.syncWeak=function(){
    if(!lastAnalysis)return;
    ensureWeak();
    const map={L:'听力',R:'阅读',W:'写作',S:'口语'};
    let synced=0;
    lastAnalysis.rows.forEach(r=>{
      const kw=map[r.sk];
      const wk=state.weak.find(x=>x.name.indexOf(kw)>=0);
      if(wk){
        const st=r.avg>=6.5?2:r.avg>=5.5?1:0;
        const label=st===2?'已改善':st===1?'部分改善':'未改善';
        wk.log.push({date:today,status:st,note:'系统自动同步：'+r.n+'套剑雅均分'+r.avg.toFixed(1)+'（'+label+'）'});
        synced++;
      }
    });
    save();renderWeak();
    showToast(synced?('已同步 '+synced+' 项到弱点模块'):'没有匹配到对应弱点');
  };

  let mood=null;
  document.getElementById('moodRow').addEventListener('click',e=>{
    const m=e.target.closest('.mood');if(!m)return;
    mood=m.dataset.v;
    document.querySelectorAll('.mood').forEach(x=>x.classList.toggle('sel',x===m));
  });
  window.saveReview=function(){
    const rv=['rv1','rv2','rv3','rv4'].map(id=>document.getElementById(id).value.trim());
    if(rv.every(x=>!x)&&!mood){showToast('写点什么再保存吧');return;}
    state.reviews.unshift({date:today,answers:rv,mood:mood||'🙂'});
    ['rv1','rv2','rv3','rv4'].forEach(id=>document.getElementById(id).value='');
    mood=null;document.querySelectorAll('.mood').forEach(x=>x.classList.remove('sel'));
    save();renderReviews();showToast('复盘已保存');
  };
  function renderReviews(){
    const el=document.getElementById('reviewHistory');
    if(!state.reviews.length){el.innerHTML='<div class="empty">还没有复盘记录</div>';return;}
    el.innerHTML='';
    state.reviews.slice(0,5).forEach(rv=>{
      const d=document.createElement('div');
      d.style.cssText='border-bottom:1px solid #0f172a;padding:10px 0;font-size:12px;';
      d.innerHTML='<b>'+rv.date+' '+rv.mood+'</b>'+rv.answers.filter(Boolean).map(a=>'<div style="color:#94a3b8;margin-top:5px;line-height:1.5;">· '+a+'</div>').join('');
      el.appendChild(d);
    });
  }

  document.querySelectorAll('.tab').forEach(t=>{
    t.onclick=()=>{
      document.querySelectorAll('.tab').forEach(x=>x.classList.remove('active'));
      document.querySelectorAll('.panel').forEach(x=>x.classList.remove('active'));
      t.classList.add('active');
      document.getElementById('panel-'+t.dataset.tab).classList.add('active');
    };
  });
  document.querySelectorAll('.subtab').forEach(t=>{
    t.onclick=()=>{
      document.querySelectorAll('.subtab').forEach(x=>x.classList.remove('active'));
      document.querySelectorAll('.subpanel').forEach(x=>x.classList.remove('active'));
      t.classList.add('active');
      document.getElementById('sub-'+t.dataset.sub).classList.add('active');
    };
  });
  let toastTimer=null;
  function showToast(msg){
    const t=document.getElementById('toast');
    t.textContent=msg;t.classList.add('show');
    clearTimeout(toastTimer);
    toastTimer=setTimeout(()=>t.classList.remove('show'),2200);
  }
  window.showToast=showToast;
  window.resetAll=function(){
    if(!confirm('确定要清除所有数据吗？此操作不可恢复。'))return;
    localStorage.removeItem(KEY);location.reload();
  };

  ensureVocId(state.activeBook);save();
  renderHeader();renderTasks();renderRecords();renderChecklist();renderSentences();
  renderDailyStats();renderSkillBars();renderMocks();renderTrend();
  renderWeak();buildCamList();renderCamMeta();renderCamSummary();
  renderBookChips();renderVocStats();renderChips();renderBrowser();renderReviews();
  updTimer();
})();
</script>
</body>
</http>
