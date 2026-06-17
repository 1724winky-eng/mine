<!DOCTYPE html>

<html lang="zh-HK">

<head>

&#x20;   <meta charset="UTF-8">

&#x20;   <meta name="viewport" content="width=device-width, initial-scale=1.0">

&#x20;   <title>智能股票持倉與風險管理系統 - 概念原型</title>

&#x20;   <script src="https://cdn.tailwindcss.com"></script>

&#x20;   <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

&#x20;   <script>

&#x20;       tailwind.config = {

&#x20;           theme: {

&#x20;               extend: {

&#x20;                   colors: {

&#x20;                       warmbg: '#F5F5F0',

&#x20;                       warmcard: '#FFFFFF',

&#x20;                       primary: '#2C5530',

&#x20;                       secondary: '#D4AF37',

&#x20;                       textmain: '#333333',

&#x20;                       textmuted: '#666666',

&#x20;                       danger: '#C62828',

&#x20;                       success: '#2E7D32'

&#x20;                   },

&#x20;                   fontFamily: {

&#x20;                       sans: \['system-ui', '-apple-system', 'BlinkMacSystemFont', 'Segoe UI', 'Roboto', 'Helvetica Neue', 'Arial', 'sans-serif'],

&#x20;                   }

&#x20;               }

&#x20;           }

&#x20;       }

&#x20;   </script>

&#x20;   <style>

&#x20;       body {

&#x20;           background-color: #F5F5F0;

&#x20;           color: #333333;

&#x20;       }

&#x20;       .chart-container {

&#x20;           position: relative;

&#x20;           width: 100%;

&#x20;           max-width: 600px;

&#x20;           margin-left: auto;

&#x20;           margin-right: auto;

&#x20;           height: 300px;

&#x20;           max-height: 400px;

&#x20;       }

&#x20;       @media (min-width: 768px) {

&#x20;           .chart-container { height: 350px; }

&#x20;       }

&#x20;       .tab-active {

&#x20;           border-bottom: 4px solid #2C5530;

&#x20;           color: #2C5530;

&#x20;           font-weight: bold;

&#x20;       }

&#x20;       .fade-in {

&#x20;           animation: fadeIn 0.3s ease-in-out;

&#x20;       }

&#x20;       @keyframes fadeIn {

&#x20;           from { opacity: 0; transform: translateY(10px); }

&#x20;           to { opacity: 1; transform: translateY(0); }

&#x20;       }

&#x20;   </style>



&#x20;   <!-- Chosen Palette: Warm Neutrals (bg-stone-50/F5F5F0), Deep Forest Green (#2C5530) for primary actions, subtle gold (#D4AF37) for highlights, standard red/green for financial metrics. -->

&#x20;   

&#x20;   <!-- Application Structure Plan: The SPA is structured as a multi-tab dashboard that acts as both an educational guide and a functional prototype of the system described in the Source Report. 

&#x20;   1. 'System Architecture': Breaks down Phase 1 \& 3 into an interactive block diagram using HTML flexbox to explain the tech stack.

&#x20;   2. 'Database Schema': Translates Phase 2 into interactive tables.

&#x20;   3. 'Portfolio Dashboard': A prototype of Phase 4 (Page 1) and Phase 3 (Module 1 \& 2), using Chart.js to visualize holdings and PnL.

&#x20;   4. 'AI Risk Manager': A prototype of Phase 3 (Module 4) \& Phase 4 (Page 2), demonstrating the Human-in-the-loop interaction with simulated LLM news analysis. 

&#x20;   This structure was chosen because it allows the user to explore the \*theory\* (architecture/database) alongside the \*practical application\* (dashboard/AI views) seamlessly in one place. -->



&#x20;   <!-- Visualization \& Content Choices: 

&#x20;   - Architecture Guide -> Goal: Organize \& Inform -> Method: HTML Grid/Flex interactive nodes -> Justification: Breaks down abstract tech stack into explorable pieces without SVG.

&#x20;   - Database Schema -> Goal: Organize -> Method: HTML Tables with Tabbed navigation -> Justification: Best way to show tabular schema data clearly.

&#x20;   - Holdings Allocation -> Goal: Proportions -> Method: Chart.js Doughnut Chart -> Justification: Standard financial visualization for portfolio weightings. (No SVG/Mermaid used).

&#x20;   - Monthly PnL -> Goal: Trend/Compare -> Method: Chart.js Bar Chart -> Justification: Clearly shows month-over-month performance changes.

&#x20;   - AI Risk Manager -> Goal: Interaction/Inform -> Method: Interactive HTML Cards with 'Accept/Reject' logic -> Justification: Demonstrates the 'Human-in-the-loop' best practice from the report directly. -->



&#x20;   <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->



</head>

<body class="antialiased min-h-screen flex flex-col">



&#x20;   <nav class="bg-white shadow-md sticky top-0 z-50">

&#x20;       <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">

&#x20;           <div class="flex justify-between h-16">

&#x20;               <div class="flex items-center">

&#x20;                   <span class="text-3xl mr-2">📈</span>

&#x20;                   <span class="font-bold text-xl text-primary tracking-wide">AI 投資組合大腦</span>

&#x20;               </div>

&#x20;           </div>

&#x20;           <div class="flex space-x-8 overflow-x-auto no-scrollbar" id="nav-tabs">

&#x20;               <button onclick="switchTab('overview')" class="tab-btn tab-active py-4 px-1 inline-flex items-center text-sm font-medium whitespace-nowrap" data-target="overview">

&#x20;                   🏗️ 系統架構與模組

&#x20;               </button>

&#x20;               <button onclick="switchTab('database')" class="tab-btn border-b-4 border-transparent text-textmuted hover:text-primary py-4 px-1 inline-flex items-center text-sm font-medium whitespace-nowrap" data-target="database">

&#x20;                   💾 資料庫設計

&#x20;               </button>

&#x20;               <button onclick="switchTab('portfolio')" class="tab-btn border-b-4 border-transparent text-textmuted hover:text-primary py-4 px-1 inline-flex items-center text-sm font-medium whitespace-nowrap" data-target="portfolio">

&#x20;                   📊 持倉總覽 (雛型)

&#x20;               </button>

&#x20;               <button onclick="switchTab('ai')" class="tab-btn border-b-4 border-transparent text-textmuted hover:text-primary py-4 px-1 inline-flex items-center text-sm font-medium whitespace-nowrap" data-target="ai">

&#x20;                   🤖 AI 風控大腦 (雛型)

&#x20;               </button>

&#x20;           </div>

&#x20;       </div>

&#x20;   </nav>



&#x20;   <main class="flex-grow max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8 w-full">



&#x20;       <section id="overview" class="tab-content fade-in">

&#x20;           <div class="bg-warmcard rounded-xl shadow-sm p-6 mb-8 border border-gray-100">

&#x20;               <h2 class="text-2xl font-bold text-primary mb-4">系統架構概覽 (Architecture)</h2>

&#x20;               <p class="text-textmain mb-6 leading-relaxed">

&#x20;                   本區塊展示了指南中規劃的系統整體架構。系統目標為達到「容易修改、方便瀏覽、自動化計算與 AI 分析」。點擊下方各層級區塊，可查看詳細的技術選型與核心模組 (Phase 1 \& Phase 3) 說明。

&#x20;               </p>



&#x20;               <div class="flex flex-col md:flex-row gap-6 mb-8">

&#x20;                   <div class="flex-1 space-y-4">

&#x20;                       <div class="p-4 border-2 border-primary bg-green-50 rounded-lg cursor-pointer hover:bg-green-100 transition-colors text-center" onclick="showArchDetail('frontend')">

&#x20;                           <span class="text-2xl block mb-2">🖥️</span>

&#x20;                           <h3 class="font-bold text-lg">前端展示 (Frontend)</h3>

&#x20;                           <p class="text-sm text-textmuted">Streamlit / Web UI</p>

&#x20;                       </div>

&#x20;                       <div class="flex justify-center text-primary text-xl">⬇️ ⬆️</div>

&#x20;                       <div class="p-4 border-2 border-blue-600 bg-blue-50 rounded-lg cursor-pointer hover:bg-blue-100 transition-colors text-center" onclick="showArchDetail('backend')">

&#x20;                           <span class="text-2xl block mb-2">⚙️</span>

&#x20;                           <h3 class="font-bold text-lg">後端與邏輯 (Backend)</h3>

&#x20;                           <p class="text-sm text-textmuted">Python 核心模組</p>

&#x20;                       </div>

&#x20;                       <div class="flex justify-center space-x-12 text-primary text-xl">

&#x20;                           <span>↙️</span><span>⬇️</span><span>↘️</span>

&#x20;                       </div>

&#x20;                       <div class="flex gap-4">

&#x20;                           <div class="flex-1 p-4 border-2 border-purple-600 bg-purple-50 rounded-lg cursor-pointer hover:bg-purple-100 transition-colors text-center" onclick="showArchDetail('ai\_engine')">

&#x20;                               <span class="text-2xl block mb-2">🧠</span>

&#x20;                               <h3 class="font-bold">AI 引擎</h3>

&#x20;                               <p class="text-xs text-textmuted">Gemini / OpenAI</p>

&#x20;                           </div>

&#x20;                           <div class="flex-1 p-4 border-2 border-orange-600 bg-orange-50 rounded-lg cursor-pointer hover:bg-orange-100 transition-colors text-center" onclick="showArchDetail('database\_layer')">

&#x20;                               <span class="text-2xl block mb-2">🗄️</span>

&#x20;                               <h3 class="font-bold">資料庫</h3>

&#x20;                               <p class="text-xs text-textmuted">PostgreSQL / SQLite</p>

&#x20;                           </div>

&#x20;                       </div>

&#x20;                   </div>



&#x20;                   <div class="flex-1 bg-gray-50 rounded-lg p-6 border border-gray-200" id="arch-details-panel">

&#x20;                       <h3 class="text-xl font-bold mb-4 text-gray-800">👈 點擊左側架構圖查看細節</h3>

&#x20;                       <p class="text-gray-600">這裡將顯示各個模組的開發細節與技術選型。</p>

&#x20;                   </div>

&#x20;               </div>

&#x20;           </div>

&#x20;           

&#x20;           <div class="bg-blue-50 rounded-xl p-6 border-l-4 border-blue-500">

&#x20;               <h3 class="font-bold text-lg mb-2">排程與自動化 (Task Scheduling)</h3>

&#x20;               <p>透過 <strong>Crontab</strong> 或 <strong>GitHub Actions</strong> 執行兩個主要自動化任務：</p>

&#x20;               <ul class="list-disc list-inside mt-2 space-y-1">

&#x20;                   <li>每日盤後 (e.g., 晚上 6 點)：執行新聞收集 (Module 3) 與 AI 分析 (Module 4)。</li>

&#x20;                   <li>每月月底 (晚上 11:59)：執行月度結算 (Module 2)，生成財報。</li>

&#x20;               </ul>

&#x20;           </div>

&#x20;       </section>



&#x20;       <section id="database" class="tab-content hidden fade-in">

&#x20;           <div class="bg-warmcard rounded-xl shadow-sm p-6 mb-8 border border-gray-100">

&#x20;               <h2 class="text-2xl font-bold text-primary mb-4">資料庫模組設計 (Phase 2)</h2>

&#x20;               <p class="text-textmain mb-6 leading-relaxed">

&#x20;                   系統核心基於三個關聯資料表。您可以點擊下方按鈕查看每個資料表的結構與模擬數據。這些資料表構成了計算成本、盈虧與持倉狀態的基礎。

&#x20;               </p>



&#x20;               <div class="flex space-x-2 mb-4">

&#x20;                   <button id="btn-transactions" onclick="showTable('transactions')" class="db-btn px-4 py-2 bg-primary text-white rounded-md text-sm font-medium">Transactions (交易紀錄)</button>

&#x20;                   <button id="btn-holdings" onclick="showTable('holdings')" class="db-btn px-4 py-2 bg-gray-200 text-gray-700 rounded-md text-sm font-medium hover:bg-gray-300">Holdings (當前持倉)</button>

&#x20;                   <button id="btn-monthly" onclick="showTable('monthly')" class="db-btn px-4 py-2 bg-gray-200 text-gray-700 rounded-md text-sm font-medium hover:bg-gray-300">Monthly\_PNL (每月盈虧)</button>

&#x20;               </div>



&#x20;               <div class="overflow-x-auto border rounded-lg">

&#x20;                   <table class="min-w-full divide-y divide-gray-200" id="db-table-content">

&#x20;                   </table>

&#x20;               </div>

&#x20;               <p id="db-table-desc" class="mt-4 text-sm text-textmuted italic"></p>

&#x20;           </div>

&#x20;       </section>



&#x20;       <section id="portfolio" class="tab-content hidden fade-in">

&#x20;           <div class="mb-6">

&#x20;               <h2 class="text-2xl font-bold text-primary mb-2">持倉總覽前端雛型 (Phase 4 - Page 1)</h2>

&#x20;               <p class="text-textmain leading-relaxed">

&#x20;                   展示如何利用前端技術 (如 Streamlit/Web) 讀取資料庫，並將枯燥的數據轉換為直觀的圖表。此處呈現了基礎持倉管理 (Module 1) 與月度結算 (Module 2) 的計算結果。

&#x20;               </p>

&#x20;           </div>



&#x20;           <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">

&#x20;               <div class="bg-warmcard p-6 rounded-xl shadow-sm border border-gray-100">

&#x20;                   <h3 class="text-sm font-semibold text-textmuted uppercase tracking-wider mb-2">總市值 (Portfolio Value)</h3>

&#x20;                   <div class="text-3xl font-bold text-gray-900">$124,500.00</div>

&#x20;                   <div class="mt-2 text-sm text-success font-medium">↑ $2,400 今日變化</div>

&#x20;               </div>

&#x20;               <div class="bg-warmcard p-6 rounded-xl shadow-sm border border-gray-100">

&#x20;                   <h3 class="text-sm font-semibold text-textmuted uppercase tracking-wider mb-2">未實現盈虧 (Unrealized PnL)</h3>

&#x20;                   <div class="text-3xl font-bold text-success">+$18,200.00</div>

&#x20;                   <div class="mt-2 text-sm text-textmuted">帳面獲利</div>

&#x20;               </div>

&#x20;               <div class="bg-warmcard p-6 rounded-xl shadow-sm border border-gray-100">

&#x20;                   <h3 class="text-sm font-semibold text-textmuted uppercase tracking-wider mb-2">已實現盈虧 (Realized PnL)</h3>

&#x20;                   <div class="text-3xl font-bold text-gray-900">+$4,500.00</div>

&#x20;                   <div class="mt-2 text-sm text-textmuted">本年度累計</div>

&#x20;               </div>

&#x20;           </div>



&#x20;           <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">

&#x20;               <div class="bg-warmcard p-6 rounded-xl shadow-sm border border-gray-100 flex flex-col">

&#x20;                   <h3 class="text-lg font-bold mb-4 text-center">資產配置佔比 (Holdings)</h3>

&#x20;                   <div class="flex-grow flex items-center justify-center">

&#x20;                       <div class="chart-container">

&#x20;                           <canvas id="allocationChart"></canvas>

&#x20;                       </div>

&#x20;                   </div>

&#x20;               </div>

&#x20;               <div class="bg-warmcard p-6 rounded-xl shadow-sm border border-gray-100 flex flex-col">

&#x20;                   <h3 class="text-lg font-bold mb-4 text-center">近半年盈虧變化 (Monthly PNL)</h3>

&#x20;                   <div class="flex-grow flex items-center justify-center">

&#x20;                       <div class="chart-container">

&#x20;                           <canvas id="pnlChart"></canvas>

&#x20;                       </div>

&#x20;                   </div>

&#x20;               </div>

&#x20;           </div>

&#x20;       </section>



&#x20;       <section id="ai" class="tab-content hidden fade-in">

&#x20;            <div class="mb-6">

&#x20;               <h2 class="text-2xl font-bold text-primary mb-2">AI 風控大腦雛型 (Phase 3 - Module 4)</h2>

&#x20;               <p class="text-textmain leading-relaxed">

&#x20;                   此為系統的核心亮點。系統每日自動抓取新聞 (Module 3)，並將新聞輸入給 LLM 判斷是否需要調整止蝕/止賺價。嚴格遵循 <strong>Human-in-the-loop (人在迴路)</strong> 原則，AI 僅提供建議，不直接觸發自動交易。

&#x20;               </p>

&#x20;           </div>



&#x20;           <div class="bg-warmcard rounded-xl shadow-sm border border-gray-200 overflow-hidden">

&#x20;               <div class="bg-gray-50 px-6 py-4 border-b border-gray-200 flex justify-between items-center">

&#x20;                   <div class="flex items-center space-x-3">

&#x20;                       <span class="text-4xl">🍎</span>

&#x20;                       <div>

&#x20;                           <h3 class="text-xl font-bold text-gray-900">AAPL (蘋果公司)</h3>

&#x20;                           <p class="text-sm text-textmuted">當前市價: $165.50 | 平均成本: $150.00</p>

&#x20;                       </div>

&#x20;                   </div>

&#x20;                   <div class="text-right">

&#x20;                       <div class="text-sm text-textmuted">當前設定</div>

&#x20;                       <div class="font-medium text-danger">止蝕: <span id="current-sl">$140.00</span></div>

&#x20;                       <div class="font-medium text-success">止賺: <span id="current-tp">$180.00</span></div>

&#x20;                   </div>

&#x20;               </div>



&#x20;               <div class="p-6 grid grid-cols-1 md:grid-cols-2 gap-8">

&#x20;                   <div>

&#x20;                       <h4 class="font-bold text-gray-700 mb-3 flex items-center"><span class="mr-2">📰</span> 過去 24 小時新聞摘要 (Module 3)</h4>

&#x20;                       <ul class="space-y-3">

&#x20;                           <li class="p-3 bg-gray-50 rounded text-sm border-l-2 border-gray-300">

&#x20;                               <span class="font-semibold block mb-1">Apple 發佈強勁 Q3 財報，iPhone 銷量超預期</span>

&#x20;                               服務收入創歷史新高，帶動整體營收成長...

&#x20;                           </li>

&#x20;                           <li class="p-3 bg-gray-50 rounded text-sm border-l-2 border-gray-300">

&#x20;                               <span class="font-semibold block mb-1">分析師上調 Apple 目標價至 $195</span>

&#x20;                               認為新一代 AI 功能將推動超級換機潮...

&#x20;                           </li>

&#x20;                       </ul>

&#x20;                   </div>



&#x20;                   <div class="bg-blue-50/50 p-5 rounded-lg border border-blue-100 relative">

&#x20;                       <div class="absolute -top-3 -right-3 text-3xl">🤖</div>

&#x20;                       <h4 class="font-bold text-blue-800 mb-3">LLM 分析與建議 (Module 4)</h4>

&#x20;                       

&#x20;                       <div class="mb-4">

&#x20;                           <span class="inline-block px-2 py-1 bg-green-100 text-green-800 text-xs font-bold rounded uppercase tracking-wide">Sentiment: Positive</span>

&#x20;                       </div>

&#x20;                       

&#x20;                       <p class="text-sm text-gray-700 mb-4 bg-white p-3 rounded border border-blue-50 shadow-sm">

&#x20;                           <strong>AI 總結:</strong> 新聞顯示基本面強勁且市場預期樂觀，有向上突破潛力。建議上調止蝕價以鎖定部分利潤，並同步上調止賺價以擴大獲利空間。

&#x20;                       </p>



&#x20;                       <div class="grid grid-cols-2 gap-4 mb-6">

&#x20;                           <div class="bg-white p-3 rounded text-center border border-gray-200">

&#x20;                               <div class="text-xs text-gray-500 mb-1">建議新止蝕價</div>

&#x20;                               <div class="text-lg font-bold text-danger">$155.00</div>

&#x20;                               <div class="text-xs text-gray-400 line-through">原: $140.00</div>

&#x20;                           </div>

&#x20;                           <div class="bg-white p-3 rounded text-center border border-gray-200">

&#x20;                               <div class="text-xs text-gray-500 mb-1">建議新止賺價</div>

&#x20;                               <div class="text-lg font-bold text-success">$190.00</div>

&#x20;                               <div class="text-xs text-gray-400 line-through">原: $180.00</div>

&#x20;                           </div>

&#x20;                       </div>



&#x20;                       <div id="action-area" class="flex flex-col sm:flex-row gap-3">

&#x20;                           <button onclick="acceptAISuggestion()" class="flex-1 bg-primary text-white py-2 rounded shadow hover:bg-green-800 transition-colors font-medium">

&#x20;                               ✅ 接受 AI 建議更新資料庫

&#x20;                           </button>

&#x20;                           <button onclick="alert('開啟手動編輯表單...')" class="flex-1 bg-white border border-gray-300 text-gray-700 py-2 rounded shadow-sm hover:bg-gray-50 transition-colors font-medium">

&#x20;                               ✏️ 手動修改

&#x20;                           </button>

&#x20;                       </div>

&#x20;                   </div>

&#x20;               </div>

&#x20;           </div>

&#x20;       </section>



&#x20;   </main>



&#x20;   <script>

&#x20;       function switchTab(tabId) {

&#x20;           document.querySelectorAll('.tab-content').forEach(el => el.classList.add('hidden'));

&#x20;           document.querySelectorAll('.tab-btn').forEach(el => {

&#x20;               el.classList.remove('tab-active', 'border-transparent', 'text-textmuted');

&#x20;               el.classList.add('border-transparent', 'text-textmuted');

&#x20;               if(el.dataset.target === tabId) {

&#x20;                   el.classList.add('tab-active');

&#x20;                   el.classList.remove('border-transparent', 'text-textmuted');

&#x20;               }

&#x20;           });

&#x20;           document.getElementById(tabId).classList.remove('hidden');

&#x20;       }



&#x20;       const archData = {

&#x20;           'frontend': {

&#x20;               title: '前端展示 (Frontend)',

&#x20;               icon: '🖥️',

&#x20;               desc: '使用 Streamlit 開發 Web 介面。包含三個核心頁面：持倉總覽、AI 建議審核、新增交易表單。優勢在於能用極少 Python 代碼建立極具質感的數據儀表板。'

&#x20;           },

&#x20;           'backend': {

&#x20;               title: '後端與邏輯 (Backend)',

&#x20;               icon: '⚙️',

&#x20;               desc: '系統核心大腦，完全使用 Python 開發。包含：<br><ul class="list-disc ml-5 mt-2"><li><b>Module 1</b>: 基礎持倉管理 (買賣紀錄、計算平均成本)</li><li><b>Module 2</b>: 月度結算模組 (每月自動計算資產)</li><li><li><b>Module 3</b>: 每日新聞收集 (串接 yfinance, Finnhub API)</li></ul>'

&#x20;           },

&#x20;           'ai\_engine': {

&#x20;               title: 'AI 引擎 (Module 4)',

&#x20;               icon: '🧠',

&#x20;               desc: '透過 Google Gemini API 或 OpenAI API，傳入精心設計的 Prompt 與當日抓取的新聞。強制 AI 以 JSON 格式輸出情緒分析 (Sentiment)、總結，以及具體的止蝕/止賺調整數字。'

&#x20;           },

&#x20;           'database\_layer': {

&#x20;               title: '資料庫 (Database)',

&#x20;               icon: '🗄️',

&#x20;               desc: 'MVP 階段可使用 SQLite 或 Google Sheets (透過 gspread)。進階可升級至 PostgreSQL 或 Airtable。核心包含三個表：Transactions (交易), Holdings (持倉), Monthly\_PNL (月報)。'

&#x20;           }

&#x20;       };



&#x20;       function showArchDetail(key) {

&#x20;           const data = archData\[key];

&#x20;           const panel = document.getElementById('arch-details-panel');

&#x20;           panel.innerHTML = `

&#x20;               <div class="flex items-center mb-3">

&#x20;                   <span class="text-3xl mr-3">${data.icon}</span>

&#x20;                   <h3 class="text-xl font-bold text-primary">${data.title}</h3>

&#x20;               </div>

&#x20;               <p class="text-gray-700 leading-relaxed">${data.desc}</p>

&#x20;           `;

&#x20;           panel.classList.add('fade-in');

&#x20;           setTimeout(() => panel.classList.remove('fade-in'), 300);

&#x20;       }



&#x20;       const dbTables = {

&#x20;           'transactions': {

&#x20;               desc: '紀錄每一次的買賣，是計算成本和盈虧的最底層基礎資料。',

&#x20;               headers: \['id', 'date', 'ticker', 'action', 'quantity', 'price', 'fees'],

&#x20;               rows: \[

&#x20;                   \['1', '2023-10-01', 'AAPL', 'Buy', '100', '150.00', '1.50'],

&#x20;                   \['2', '2023-10-15', 'TSLA', 'Buy', '50', '220.00', '2.00'],

&#x20;                   \['3', '2023-11-20', 'AAPL', 'Sell', '20', '170.00', '1.50']

&#x20;               ]

&#x20;           },

&#x20;           'holdings': {

&#x20;               desc: '可從 Transactions 動態計算，包含 AI 關注的風險控制價格。',

&#x20;               headers: \['ticker', 'total\_quantity', 'average\_cost', 'current\_price', 'take\_profit', 'stop\_loss'],

&#x20;               rows: \[

&#x20;                   \['AAPL', '80', '150.00', '165.50', '180.00', '140.00'],

&#x20;                   \['TSLA', '50', '220.00', '205.00', '280.00', '190.00'],

&#x20;                   \['0700.HK', '500', '280.00', '310.00', '350.00', '260.00']

&#x20;               ]

&#x20;           },

&#x20;           'monthly': {

&#x20;               desc: '由 Module 2 每月底自動生成的快照，用於繪製長期趨勢。',

&#x20;               headers: \['month', 'portfolio\_value', 'realized\_pnl', 'unrealized\_pnl'],

&#x20;               rows: \[

&#x20;                   \['2023-09', '115,000.00', '2,100.00', '8,500.00'],

&#x20;                   \['2023-10', '120,500.00', '2,100.00', '12,000.00'],

&#x20;                   \['2023-11', '124,500.00', '4,500.00', '18,200.00']

&#x20;               ]

&#x20;           }

&#x20;       };



&#x20;       function showTable(tableName) {

&#x20;           document.querySelectorAll('.db-btn').forEach(btn => {

&#x20;               btn.classList.remove('bg-primary', 'text-white');

&#x20;               btn.classList.add('bg-gray-200', 'text-gray-700');

&#x20;           });



&#x20;           const activeBtn = document.getElementById(`btn-${tableName}`);

&#x20;           if (activeBtn) {

&#x20;               activeBtn.classList.remove('bg-gray-200', 'text-gray-700');

&#x20;               activeBtn.classList.add('bg-primary', 'text-white');

&#x20;           }



&#x20;           const data = dbTables\[tableName];

&#x20;           document.getElementById('db-table-desc').innerText = "說明： " + data.desc;

&#x20;           

&#x20;           let html = '<thead class="bg-gray-50 text-left"><tr>';

&#x20;           data.headers.forEach(h => {

&#x20;               html += `<th class="px-6 py-3 text-xs font-medium text-gray-500 uppercase tracking-wider">${h}</th>`;

&#x20;           });

&#x20;           html += '</tr></thead><tbody class="bg-white divide-y divide-gray-200">';

&#x20;           data.rows.forEach(row => {

&#x20;               html += '<tr>';

&#x20;               row.forEach(cell => {

&#x20;                   html += `<td class="px-6 py-4 whitespace-nowrap text-sm text-gray-700">${cell}</td>`;

&#x20;               });

&#x20;               html += '</tr>';

&#x20;           });

&#x20;           html += '</tbody>';

&#x20;           document.getElementById('db-table-content').innerHTML = html;

&#x20;       }



&#x20;       let allocChart, pnlChartInst;



&#x20;       function initCharts() {

&#x20;           const ctxAlloc = document.getElementById('allocationChart').getContext('2d');

&#x20;           allocChart = new Chart(ctxAlloc, {

&#x20;               type: 'doughnut',

&#x20;               data: {

&#x20;                   labels: \['AAPL (Apple)', 'TSLA (Tesla)', '0700.HK (Tencent)', 'Cash'],

&#x20;                   datasets: \[{

&#x20;                       data: \[40, 25, 20, 15],

&#x20;                       backgroundColor: \['#2C5530', '#4A7C59', '#88B04B', '#E5E7EB'],

&#x20;                       borderWidth: 0

&#x20;                   }]

&#x20;               },

&#x20;               options: {

&#x20;                   responsive: true,

&#x20;                   maintainAspectRatio: false,

&#x20;                   plugins: {

&#x20;                       legend: { position: 'right' }

&#x20;                   },

&#x20;                   cutout: '70%'

&#x20;               }

&#x20;           });



&#x20;           const ctxPnl = document.getElementById('pnlChart').getContext('2d');

&#x20;           pnlChartInst = new Chart(ctxPnl, {

&#x20;               type: 'bar',

&#x20;               data: {

&#x20;                   labels: \['Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov'],

&#x20;                   datasets: \[

&#x20;                       {

&#x20;                           label: '未實現盈虧 (Unrealized)',

&#x20;                           data: \[5000, 6500, 4200, 8500, 12000, 18200],

&#x20;                           backgroundColor: '#88B04B'

&#x20;                       },

&#x20;                       {

&#x20;                           label: '已實現盈虧 (Realized)',

&#x20;                           data: \[1000, 1000, 2100, 2100, 2100, 4500],

&#x20;                           backgroundColor: '#2C5530'

&#x20;                       }

&#x20;                   ]

&#x20;               },

&#x20;               options: {

&#x20;                   responsive: true,

&#x20;                   maintainAspectRatio: false,

&#x20;                   scales: {

&#x20;                       x: { stacked: true, grid: { display: false } },

&#x20;                       y: { stacked: true, beginAtZero: true }

&#x20;                   }

&#x20;               }

&#x20;           });

&#x20;       }



&#x20;       function acceptAISuggestion() {

&#x20;           document.getElementById('current-sl').innerHTML = '<span class="fade-in inline-block">$155.00</span>';

&#x20;           document.getElementById('current-tp').innerHTML = '<span class="fade-in inline-block">$190.00</span>';

&#x20;           

&#x20;           const actionArea = document.getElementById('action-area');

&#x20;           actionArea.innerHTML = `

&#x20;               <div class="w-full bg-green-50 text-green-800 p-3 rounded border border-green-200 text-center font-bold flex items-center justify-center fade-in">

&#x20;                   <span class="mr-2">💾</span> 資料庫已更新 (Holdings Table)

&#x20;               </div>

&#x20;           `;

&#x20;       }



&#x20;       window.onload = () => {

&#x20;           initCharts();

&#x20;           showTable('transactions'); 

&#x20;       };

&#x20;   </script>

</body>

</html>

