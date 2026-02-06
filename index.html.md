# index.html  
<!DOCTYPE html>  
<html lang="ja">  
<head>  
    <meta charset="UTF-8">  
    <meta name="apple-mobile-web-app-capable" content="yes">  
    <meta name="apple-mobile-web-app-status-bar-style" content="default">  
    <meta name="apple-mobile-web-app-title" content="MEMOトレ">  
    <link rel="apple-touch-icon" href="icon-192.png">  
    <link rel="manifest" href="manifest.json">  
      
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">  
    <title>【MEMOトレ】</title>  
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>  
    <style>  
        :root {  
            --bg-color: #f4f7f6;  
            --card-bg: #ffffff;  
            --primary: #2196F3;  
            --secondary: #E91E63;  
            --tertiary: #4CAF50;  
            --text-main: #333333;  
            --text-sub: #757575;  
            --border: #e0e0e0;  
            --danger: #FF5252;  
            --overlay-bg: rgba(0, 0, 0, 0.5);  
            --calendar-selected: rgba(33, 150, 243, 0.2);  
            --youtube-color: #FF0000;  
        }  
  
        body {  
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;  
            background-color: var(--bg-color);  
            color: var(--text-main);  
            margin: 0; padding: 0; padding-bottom: 90px;  
        }  
  
        header {  
            background-color: var(--card-bg);  
            padding: 12px 15px;  
            text-align: center;  
            box-shadow: 0 2px 4px rgba(0,0,0,0.05);  
            position: sticky; top: 0; z-index: 100;  
        }  
        .mode-switch { display: flex; background: #f0f0f0; border-radius: 20px; padding: 4px; gap: 4px; }  
        .mode-btn { flex: 1; background: none; border: none; color: var(--text-sub); padding: 8px 0; font-size: 0.8rem; border-radius: 16px; cursor: pointer; font-weight: bold; }  
        .mode-btn.active[data-mode="hypertrophy"] { background: #2196F3; color: white; }  
        .mode-btn.active[data-mode="diet"] { background: #E91E63; color: white; }  
        .mode-btn.active[data-mode="health"] { background: #4CAF50; color: white; }  
  
        .container { padding: 12px; max-width: 600px; margin: 0 auto; }  
        .card { background-color: var(--card-bg); border-radius: 12px; padding: 15px; margin-bottom: 15px; box-shadow: 0 2px 8px rgba(0,0,0,0.05); border: 1px solid var(--border); }  
  
        .part-selector { display: flex; overflow-x: auto; gap: 8px; margin-bottom: 15px; padding-bottom: 5px; scrollbar-width: none; }  
        .part-btn { background: #fff; color: var(--text-sub); border: 1px solid var(--border); padding: 8px 16px; border-radius: 20px; font-size: 0.85rem; white-space: nowrap; cursor: pointer; }  
  
        .control-panel { display: flex; flex-direction: column; gap: 10px; margin-bottom: 15px; }  
        .select-main { display: flex; gap: 8px; align-items: center; }  
        select { flex: 1; background-color: #f9f9f9; color: var(--text-main); padding: 12px; border: 1px solid var(--border); border-radius: 8px; font-size: 1rem; outline: none; }  
          
        .select-tools { display: flex; gap: 8px; }  
        .tool-btn { flex: 1; display: flex; align-items: center; justify-content: center; background: #fff; border: 1px solid var(--border); border-radius: 8px; cursor: pointer; height: 42px; font-size: 0.85rem; }  
        .youtube-btn { width: 50px; flex-shrink: 0; color: var(--youtube-color); font-size: 1.4rem; }  
        .recommend-btn { flex: 2; color: var(--primary); font-weight: bold; }  
        .manage-btn { flex: 1; color: var(--text-sub); font-size: 1.1rem; }  
  
        .coach-advice { background: #f9f9f9; border-left: 5px solid var(--primary); padding: 12px; border-radius: 6px; margin-bottom: 15px; font-size: 0.85rem; }  
        .prev-log-area { background: #f0f4f8; padding: 10px; border-radius: 8px; margin-bottom: 15px; font-size: 0.85rem; display: flex; justify-content: space-between; align-items: center; }  
        .copy-btn { background: #fff; border: 1px solid var(--primary); color: var(--primary); padding: 4px 12px; border-radius: 20px; font-size: 0.75rem; font-weight: bold; }  
  
        .set-header { display: grid; grid-template-columns: 28px 28px 25px 1fr 1fr 45px; gap: 6px; margin-bottom: 8px; font-size: 0.65rem; color: var(--text-sub); text-align: center; }  
        .set-row { display: grid; grid-template-columns: 28px 28px 25px 1fr 1fr 45px; gap: 6px; margin-bottom: 10px; align-items: center; }  
        .set-num { color: var(--primary); font-weight: bold; text-align: center; background: #e3f2fd; width: 20px; height: 20px; line-height: 20px; border-radius: 50%; margin: auto; font-size: 0.75rem;}  
          
        .set-copy-btn { border: 1px solid var(--border); background: #fff; border-radius: 4px; cursor: pointer; font-size: 0.6rem; height: 22px; width: 100%; color: var(--text-sub); display: flex; align-items: center; justify-content: center; padding: 0; font-weight: bold;}  
        .set-copy-btn:active { background: #eee; transform: translateY(1px); }  
  
        input[type="number"] { background: #f9f9f9; border: 1px solid var(--border); color: var(--text-main); padding: 10px 5px; border-radius: 8px; text-align: center; font-size: 1.1rem; width: 100%; box-sizing: border-box; }  
        .rm-display { font-size: 0.7rem; color: var(--text-sub); text-align: center; font-family: monospace;}  
          
        .save-btn { width: 100%; background: var(--primary); color: white; border: none; padding: 16px; border-radius: 12px; font-size: 1rem; font-weight: bold; cursor: pointer; }  
  
        .tabs { position: fixed; bottom: 0; width: 100%; background: var(--card-bg); display: flex; border-top: 1px solid var(--border); padding-bottom: env(safe-area-inset-bottom); z-index: 99; }  
        .tab-btn { flex: 1; padding: 12px; background: none; border: none; color: var(--text-sub); font-size: 0.75rem; cursor: pointer; display: flex; flex-direction: column; align-items: center; }  
        .tab-btn.active { color: var(--primary); font-weight: bold; }  
  
        .view-section { display: none; }  
        .view-section.active { display: block; }  
  
        .calendar-grid { display: grid; grid-template-columns: repeat(7, 1fr); gap: 6px; text-align: center; }  
        .cal-cell { aspect-ratio: 1; display: flex; justify-content: center; align-items: center; border-radius: 10px; font-size: 0.9rem; cursor: pointer; position: relative; background: #f9f9f9; }  
        .cal-cell.today { border: 1px solid var(--primary); color: var(--primary); }  
        .cal-cell.has-log::after { content: ''; width: 4px; height: 4px; background-color: var(--primary); border-radius: 50%; position: absolute; bottom: 4px; }  
  
        .modal-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: var(--overlay-bg); z-index: 999; display: none; justify-content: center; align-items: center; }  
        .modal-content { background: var(--card-bg); width: 85%; max-width: 350px; max-height: 80vh; border-radius: 16px; padding: 20px; overflow-y: auto; }  
        .modal-item { padding: 15px 0; border-bottom: 1px solid var(--border); display: flex; justify-content: space-between; align-items: center; cursor: pointer; }  
    </style>  
</head>  
<body>  
  
    <header>  
        <h1 style="margin-bottom:8px; font-size:1.1rem; color:#333;">【MEMOトレ】</h1>  
        <div class="mode-switch">  
            <button class="mode-btn active" data-mode="hypertrophy" onclick="changeMode('hypertrophy')">💪 筋肥大</button>  
            <button class="mode-btn" data-mode="diet" onclick="changeMode('diet')">🔥 シェイプ</button>  
            <button class="mode-btn" data-mode="health" onclick="changeMode('health')">🍀 健康維持</button>  
        </div>  
    </header>  
  
    <div id="view-record" class="view-section active container">  
        <div class="part-selector">  
            <button class="part-btn active" onclick="setPart('胸')">胸</button>  
            <button class="part-btn" onclick="setPart('背中')">背中</button>  
            <button class="part-btn" onclick="setPart('脚')">脚</button>  
            <button class="part-btn" onclick="setPart('肩')">肩</button>  
            <button class="part-btn" onclick="setPart('腕')">腕</button>  
            <button class="part-btn" onclick="setPart('腹')">腹</button>  
        </div>  
  
        <div class="card">  
            <div class="control-panel">  
                <div class="select-main">  
                    <select id="exerciseSelect" onchange="loadPreviousLog()"></select>  
                    <button class="tool-btn youtube-btn" onclick="searchYouTube()">▶️</button>  
                </div>  
                <div class="select-tools">  
                    <button class="tool-btn recommend-btn" onclick="openRecommendModal()">💡 AIおすすめ提案</button>  
                    <button class="tool-btn manage-btn" onclick="openManageModal()">⚙️ 設定</button>  
                </div>  
            </div>  
              
            <div class="coach-advice" id="coachAdvice">  
                <span id="adviceText">--</span>  
            </div>  
  
            <div class="prev-log-area" id="prevLogArea" style="display:none;">  
                <div style="font-size: 0.8rem; flex:1;">前回: <span id="prevLogContent" style="font-family:monospace;">--</span></div>  
                <button class="copy-btn" onclick="copyPreviousLog()">履歴コピー</button>  
            </div>  
  
            <div class="set-header">  
                <div>W↓</div><div>R↓</div><div>SET</div> <div>KG</div> <div>REPS</div> <div>1RM</div>  
            </div>  
            <div id="inputRows"></div>  
            <button class="save-btn" id="saveBtn" onclick="saveLog()">トレーニング完了！</button>  
        </div>  
    </div>  
  
    <div id="view-calendar" class="view-section container">  
        <div class="card">  
            <div class="calendar-header">  
                <button onclick="changeMonth(-1)" style="border:none; background:none;">◀</button>  
                <span id="calendarTitle" style="font-weight:bold;"></span>  
                <button onclick="changeMonth(1)" style="border:none; background:none;">▶</button>  
            </div>  
            <div id="calendarGridHeader" class="calendar-grid"></div>  
            <div id="calendarBody" class="calendar-grid"></div>  
        </div>  
        <div class="card">  
            <h3 id="selectedDateTitle" style="font-size:0.9rem; margin-top:0;">選択日の記録</h3>  
            <div id="dayDetailList" style="font-size:0.85rem;"></div>  
        </div>  
    </div>  
  
    <div id="view-history" class="view-section container">  
        <div class="card">  
            <h3 style="text-align:center; margin-top:0; font-size:0.9rem;">日別 合計負荷量 (kg)</h3>  
            <canvas id="chartCanvas"></canvas>  
        </div>  
        <div id="historyList"></div>  
    </div>  
  
    <div class="tabs">  
        <button class="tab-btn active" id="tab-record" onclick="switchTab('record')"><span>📝</span>記録</button>  
        <button class="tab-btn" id="tab-calendar" onclick="switchTab('calendar')"><span>📅</span>カレンダー</button>  
        <button class="tab-btn" id="tab-history" onclick="switchTab('history')"><span>📈</span>分析</button>  
    </div>  
  
    <div id="manageModal" class="modal-overlay">  
        <div class="modal-content">  
            <h3 style="margin-top:0; font-size:1.1rem;">種目リストの編集</h3>  
            <button onclick="addNewExercise()" style="width:100%; padding:10px; margin-bottom:15px; border-radius:8px; border:none; background:#f0f0f0; font-weight:bold;">＋ 新規種目を手動追加</button>  
            <ul id="manageList" style="list-style:none; padding:0; margin-bottom:20px;"></ul>  
            <button onclick="closeModal('manageModal')" style="width:100%; padding:12px; background:#333; color:#fff; border:none; border-radius:8px;">閉じる</button>  
        </div>  
    </div>  
  
    <div id="recommendModal" class="modal-overlay">  
        <div class="modal-content">  
            <h3 style="margin-top:0; font-size:1.1rem;">AIおすすめ種目</h3>  
            <ul id="recommendList" style="list-style:none; padding:0; margin-bottom:20px;"></ul>  
            <button onclick="closeModal('recommendModal')" style="width:100%; padding:12px; background:#333; color:#fff; border:none; border-radius:8px;">閉じる</button>  
        </div>  
    </div>  
  
    <script>  
        // (中略: 前回の全スクリプトをここに入れてください。sw.jsの登録コードも忘れずに)  
        if ('serviceWorker' in navigator) {  
            navigator.serviceWorker.register('sw.js')  
                .then(() => console.log('Service Worker Registered'));  
        }  
          
        // 以前のスクリプト本体をここに貼り付け  
        const DB_EX = 'workout_ex_final_pwa';  
        const DB_LOGS = 'workout_logs_final_pwa';  
        const aiKnowledgeBase = { '胸': ['ベンチプレス','ダンベルプレス','インクラインベンチ','ダンベルフライ','チェストプレス','ケーブルクロスオーバー','ディップス'], '背中': ['デッドリフト','ラットプルダウン','ベントオーバーロウ','懸垂','シーテッドロウ','フェイスプル'], '脚': ['スクワット','レッグプレス','レッグエクステンション','レッグカール','ブルガリアンスクワット','ランジ'], '肩': ['ショルダープレス','サイドレイズ','フロントレイズ','リアデルト','アップライトロウ','アーノルドプレス'], '腕': ['アームカール','ハンマーカール','トライセプスエクステンション','フレンチプレス','プレスダウン'], '腹': ['クランチ','レッグレイズ','アブローラー','プランク','バイシクルクランチ'] };  
        const coach = { hypertrophy: { name:'筋肥大', color:'#2196F3', advice:'8〜12回で限界がくる重量設定が最高に効果的です！' }, diet: { name:'シェイプ', color:'#E91E63', advice:'15〜20回でインターバルを30秒に！脂肪を燃やしましょう。' }, health: { name:'健康維持', color:'#4CAF50', advice:'10〜15回で無理なく。フォームを意識しましょう。' } };  
        let currentPart = '胸'; let currentMode = 'hypertrophy'; let currentDate = new Date(); let myChart = null;  
        document.addEventListener('DOMContentLoaded', () => { if(!localStorage.getItem(DB_EX)) { const initialUserList = {}; for(let key in aiKnowledgeBase) { initialUserList[key] = aiKnowledgeBase[key].slice(0, 3); } localStorage.setItem(DB_EX, JSON.stringify(initialUserList)); } initInputRows(); const days = ['日','月','火','水','木','金','土']; document.getElementById('calendarGridHeader').innerHTML = days.map(d=>`<div style="font-size:0.75rem; color:#888;">${d}</div>`).join(''); setPart('胸'); updateAdviceUI(); });  
        function copyWeight(idx) { const val = document.querySelector(`.w-in[data-idx="${idx-1}"]`).value; if (val) { const target = document.querySelector(`.w-in[data-idx="${idx}"]`); target.value = val; calc(target); } }  
        function copyReps(idx) { const val = document.querySelector(`.r-in[data-idx="${idx-1}"]`).value; if (val) { document.querySelector(`.r-in[data-idx="${idx}"]`).value = val; calc(document.querySelector(`.r-in[data-idx="${idx}"]`)); } }  
        function initInputRows() { let html = ''; for(let i=1; i<=5; i++) { const weightBtn = (i > 1) ? `<button class="set-copy-btn" onclick="copyWeight(${i})">W↓</button>` : '<div></div>'; const repsBtn = (i > 1) ? `<button class="set-copy-btn" onclick="copyReps(${i})">R↓</button>` : '<div></div>'; html += `<div class="set-row">${weightBtn}${repsBtn}<div class="set-num">${i}</div><input type="number" class="w-in" data-idx="${i}" oninput="calc(this)" placeholder="-"><input type="number" class="r-in" data-idx="${i}" oninput="calc(this)" placeholder="-"><div class="rm-display" id="rm-${i}">-</div></div>`; } document.getElementById('inputRows').innerHTML = html; }  
        function searchYouTube() { const exName = document.getElementById('exerciseSelect').value; if(!exName) return; const url = `https://www.youtube.com/results?search_query=${encodeURIComponent(exName + " やり方 フォーム")}`; const newWindow = window.open(url, '_blank'); if(!newWindow) window.location.href = url; }  
        function changeMode(mode) { currentMode = mode; document.querySelectorAll('.mode-btn').forEach(b => b.classList.toggle('active', b.dataset.mode === mode)); updateAdviceUI(); }  
        function updateAdviceUI() { const data = coach[currentMode]; document.documentElement.style.setProperty('--primary', data.color); document.getElementById('adviceText').innerText = data.advice; document.querySelectorAll('.part-btn.active').forEach(b => { b.style.backgroundColor = data.color; b.style.borderColor = data.color; }); const tab = document.querySelector('.tab-btn.active'); if(tab) tab.style.color = data.color; }  
        function setPart(part) { currentPart = part; document.querySelectorAll('.part-btn').forEach(b => { const isActive = b.innerText === part; b.classList.toggle('active', isActive); b.style.backgroundColor = isActive ? coach[currentMode].color : ''; b.style.borderColor = isActive ? coach[currentMode].color : ''; b.style.color = isActive ? '#fff' : ''; }); const db = JSON.parse(localStorage.getItem(DB_EX)); const select = document.getElementById('exerciseSelect'); select.innerHTML = (db[part]||[]).map(ex => `<option value="${ex}">${ex}</option>`).join(''); loadPreviousLog(); }  
        function calc(el) { const idx = el.dataset.idx; const w = document.querySelector(`.w-in[data-idx="${idx}"]`).value; const r = document.querySelector(`.r-in[data-idx="${idx}"]`).value; const disp = document.getElementById(`rm-${idx}`); if(w && r) disp.innerText = Math.round(w * (1 + r/30)) + 'k'; else disp.innerText = '-'; }  
        function saveLog() { const ex = document.getElementById('exerciseSelect').value; let sets = []; for(let i=1; i<=5; i++){ const w = document.querySelector(`.w-in[data-idx="${i}"]`).value; const r = document.querySelector(`.r-in[data-idx="${i}"]`).value; if(w && r) sets.push({w:Number(w), r:Number(r)}); } if(!sets.length) return alert('記録を入力してください'); const log = { id:Date.now(), ts:Date.now(), date:new Date().toLocaleDateString(), ex, part:currentPart, sets }; const logs = JSON.parse(localStorage.getItem(DB_LOGS)) || []; logs.unshift(log); localStorage.setItem(DB_LOGS, JSON.stringify(logs)); alert('保存しました！'); loadPreviousLog(); }  
        function loadPreviousLog() { const ex = document.getElementById('exerciseSelect').value; const logs = JSON.parse(localStorage.getItem(DB_LOGS)) || []; const prev = logs.find(l => l.ex === ex); const area = document.getElementById('prevLogArea'); if(prev) { area.style.display = 'flex'; document.getElementById('prevLogContent').innerText = prev.sets.map(s=>`${s.w}×${s.r}`).join(', '); document.getElementById('prevLogContent').dataset.json = JSON.stringify(prev.sets); } else { area.style.display = 'none'; } document.querySelectorAll('input').forEach(i => i.value = ''); document.querySelectorAll('.rm-display').forEach(d => d.innerText = '-'); }  
        function copyPreviousLog() { const json = document.getElementById('prevLogContent').dataset.json; if(!json) return; JSON.parse(json).forEach((s, i) => { if(i < 5) { const w = document.querySelector(`.w-in[data-idx="${i+1}"]`); const r = document.querySelector(`.r-in[data-idx="${i+1}"]`); w.value = s.w; r.value = s.r; calc(w); } }); }  
        function switchTab(name) { document.querySelectorAll('.view-section').forEach(v => v.classList.remove('active')); document.querySelectorAll('.tab-btn').forEach(b => { b.classList.remove('active'); b.style.color = ''; }); document.getElementById(`view-${name}`).classList.add('active'); const btn = document.getElementById(`tab-${name}`); btn.classList.add('active'); btn.style.color = coach[currentMode].color; if(name==='history') updateChart(); if(name==='calendar') renderCalendar(); }  
        function renderCalendar() { const y = currentDate.getFullYear(), m = currentDate.getMonth(); document.getElementById('calendarTitle').innerText = `${y}年 ${m+1}月`; const start = new Date(y, m, 1).getDay(); const len = new Date(y, m+1, 0).getDate(); const logs = JSON.parse(localStorage.getItem(DB_LOGS)) || []; const body = document.getElementById('calendarBody'); body.innerHTML = ''; for(let i=0; i<start; i++) body.appendChild(document.createElement('div')); for(let d=1; d<=len; d++) { const cell = document.createElement('div'); cell.className = 'cal-cell'; cell.innerText = d; const has = logs.some(l => { const dt = new Date(l.ts); return dt.getFullYear()===y && dt.getMonth()===m && dt.getDate()===d; }); if(has) cell.classList.add('has-log'); cell.onclick = () => { document.querySelectorAll('.cal-cell').forEach(c=>c.classList.remove('selected')); cell.classList.add('selected'); const target = logs.filter(l => { const dt = new Date(l.ts); return dt.getFullYear()===y && dt.getMonth()===m && dt.getDate()===d; }); document.getElementById('dayDetailList').innerHTML = target.map(l=>`<div style="padding:8px 0; border-bottom:1px solid #eee;"><b>${l.ex}</b><br><small>${l.sets.map(s=>s.w+'×'+s.r).join(', ')}</small></div>`).join('') || '記録なし'; }; body.appendChild(cell); } }  
        function changeMonth(n) { currentDate.setMonth(currentDate.getMonth()+n); renderCalendar(); }  
        function updateChart() { const ctx = document.getElementById('chartCanvas'); const logs = JSON.parse(localStorage.getItem(DB_LOGS)) || []; const dataMap = {}; logs.forEach(l => { dataMap[l.date] = (dataMap[l.date]||0) + l.sets.reduce((a,s)=>a+(s.w*s.r), 0); }); const labels = Object.keys(dataMap).sort((a,b)=>new Date(a)-new Date(b)); if(myChart) myChart.destroy(); myChart = new Chart(ctx, { type:'line', data:{ labels, datasets:[{ label:'負荷(kg)', data:labels.map(l=>dataMap[l]), borderColor:coach[currentMode].color, tension:0.3, fill:true, backgroundColor:coach[currentMode].color+'22' }] }, options:{ plugins:{legend:{display:false}}, scales:{y:{beginAtZero:true}} } }); const list = document.getElementById('historyList'); list.innerHTML = logs.map(l=>`<div style="padding:10px; border-bottom:1px solid #eee; font-size:0.85rem;">${l.date} <b>${l.ex}</b> <small>${l.sets.map(s=>s.w+'×'+s.r).join(' ')}</small></div>`).join(''); }  
        function openManageModal() { const db = JSON.parse(localStorage.getItem(DB_EX)); document.getElementById('manageList').innerHTML = db[currentPart].map(ex => `<li class="modal-item"><span>${ex}</span><button onclick="delEx('${ex}')" style="color:red; border:none; background:none; font-size:1.2rem;">🗑️</button></li>`).join(''); document.getElementById('manageModal').style.display='flex'; }  
        function addNewExercise() { const n = prompt('新しい種目名を入力:'); if(!n) return; const db = JSON.parse(localStorage.getItem(DB_EX)); db[currentPart].push(n); localStorage.setItem(DB_EX, JSON.stringify(db)); openManageModal(); setPart(currentPart); }  
        function delEx(ex) { if(!confirm('削除しますか？')) return; const db = JSON.parse(localStorage.getItem(DB_EX)); db[currentPart] = db[currentPart].filter(i=>i!==ex); localStorage.setItem(DB_EX, JSON.stringify(db)); openManageModal(); setPart(currentPart); }  
        function openRecommendModal() { const db = JSON.parse(localStorage.getItem(DB_EX)); const userExercises = db[currentPart] || []; const allKnowledge = aiKnowledgeBase[currentPart] || []; const shuffled = [...allKnowledge].sort(() => 0.5 - Math.random()); const selection = shuffled.slice(0, 5); const ul = document.getElementById('recommendList'); ul.innerHTML = selection.map(ex => { const isNew = !userExercises.includes(ex); return `<li class="modal-item" onclick="addAndSet('${ex}')"><span style="font-weight:bold;">${ex}${isNew ? '<span class="new-tag">NEW</span>' : ''}</span><span style="color:var(--primary);">＋</span></li>`; }).join(''); document.getElementById('recommendModal').style.display='flex'; }  
        function addAndSet(ex) { const db = JSON.parse(localStorage.getItem(DB_EX)); if(!db[currentPart].includes(ex)) { db[currentPart].push(ex); localStorage.setItem(DB_EX, JSON.stringify(db)); } setPart(currentPart); document.getElementById('exerciseSelect').value = ex; closeModal('recommendModal'); loadPreviousLog(); }  
        function closeModal(id) { document.getElementById(id).style.display='none'; }  
    </script>  
</body>  
</html>  
