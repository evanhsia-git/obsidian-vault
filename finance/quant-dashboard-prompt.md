---
title: "quant-dashboard 實作紀錄與提示詞"
description: "quant-dashboard 專案的問答精華、架構決策過程、GitHub Actions/Pages 提示詞範本——供未來寫 skill 時參考"
summary: "quant-dashboard 的對話決策記錄（雙排程/多源備援/匿名/手填CSV）+ Actions workflow 提示詞範本 + 關鍵學習點，作為未來 skill 化的種子"
type: resource
status: active
tags:
  - etf
  - finance
created: 2026-07-13
updated: 2026-07-13
---

# quant-dashboard 實作紀錄與提示詞

> 用途：記錄本專案的**問答過程、決策邏輯、提示詞範本**，供未來提煉成 Hermes skill 時參考。
> 關聯架構頁：[[finance/quant-dashboard|quant-dashboard 專案架構]]
> 研究基礎：[[finance/github-actions-pages-stock-analysis|GitHub Actions/Pages 股市應用研究]]

---

## 一、專案緣起

- 用戶研究 [ZhuLinsen/daily_stock_analysis](https://github.com/ZhuLinsen/daily_stock_analysis)（56.5k stars LLM 股票分析系統）
- 發現該 repo **用 GitHub Actions 跑分析 + artifact + 多通道推播，但沒用 GitHub Pages**
- 用戶想要：**Actions + Pages 配合**，做成公開股市儀表板

---

## 二、問答決策精華（Q&A 壓縮版）

### Q1: Actions 與 Pages 的分工？
**A**：Actions = 運算引擎（抓資料/算指標/跑回測/生成靜態網頁）；Pages = 展示層（託管 HTML，免伺服器）。兩者經 Git Repo 連接（Actions push → Pages 發佈）。

### Q2: Actions 為主還是 Hermes 為主？
**A**：**混合雙源，Actions 為主功能，Hermes 為輔助**。原因：不是人人都有 Hermes，公開系統要能獨立於 Hermes 運行。Hermes 只做開發/維護/備援，不參與日常運行。

### Q3: 排程怎麼排？
**A**：**雙排程**——
- 08:30 台灣（抓前一晚美股收盤，建立美股資料）
- 17:00 台灣（台股收盤後，抓台股+選股+回測）
- 用戶要求：**每日只 push 一次**（08:30 先存 data/ 不 push；17:00 合併前日美股+當日台股後一次 push）

### Q4: 資料來源？
**A**：**TWSE + TPEX 為主**（台股/上櫃 ETF），**備援 FinMind / yfinance / OpenBB**（防獲取失敗）。美股主源 yfinance，備援 FinMind/OpenBB。每個 fetch 函式實作 `try 主源 except 切備援`。

### Q5: 即時報價？
**A**：**不使用**。全用前一日完整收盤資料作業（頻寬/隱私考量，未來再說）。

### Q6: 持倉資料怎麼填（用戶不要程式碼/外部軟體）？
**A**：**repo 內 CSV 編輯方案**——`data/portfolio-data.csv` 放範本，使用者在 GitHub 網頁點「編輯」手填（代號/股數/成本），不用任何程式碼。Actions 讀 CSV 生成持倉頁 [D]。

### Q7: 公開網站隱私？
**A**：**匿名**。不出現作者資訊；頁面標註「金融數據僅供參考，測試使用」。真實持倉留本地，公開只用「測試示範資料」（用戶格式+標的，股數用範例值）。

### Q8: 失敗怎麼知道？
**A**：**雙示警 Telegram + Email**。監控 Pages 內容日期是否停滯（Actions 失敗/API 全掛→告警）。

### Q9: 技術棧（用戶不懂前端框架 vs Python 生成）？
**A**：**最終決策：React 或 Vue 為主（hybrid 模式）**。
- 早期假設：Python+Plotly 直接生成 HTML（MVP 最低複雜度）
- 後段變更（2026-07-13）：用戶要求「以 React/Vue 開發為主」→ 改為 Python 產 `data/*.json`，前端讀 JSON 渲染互動儀表板（K線縮放/篩選/切換）
- 解耦：Python 改邏輯不影響前端；前端改 UI 不影響資料層
- 架構變複雜（需 setup-node + npm build），但互動性與擴充性強

---

## 三、關鍵架構決策（已定稿）

| 決策點 | 結論 |
| --- | --- |
| 運算主力 | GitHub Actions（定時+生成+部署） |
| 輔助 | Hermes（開發/維護/備援，不日常運行） |
| 展示 | GitHub Pages（公開靜態） |
| 排程 | 雙：08:30 美股 / 17:00 台股，**每日一次 push** |
| 資料源 | TWSE+TPEX 主，FinMind/yfinance/OpenBB 備援 |
| 即時 | 不使用，全前日收盤資料 |
| 持倉填寫 | repo CSV 網頁編輯（無程式碼） |
| 隱私 | 匿名 + 測試資料標註 + 真實留本地 |
| 示警 | Telegram + Email 雙通道 |
| 技術 | Python + Plotly（非前端框架） | **已推翻** |
| 技術 | **React 或 Vue 為主（hybrid：Python 產 JSON + 前端讀 JSON 渲染）** | 2026-07-13 後段變更 |
| Repo | `ivanhsia/quant-dashboard`（Public + Pages） |
| 功能 | ABCD 全做（大盤/ETF、選股、回測、測試持倉） |

---

## 四、GitHub Actions 提示詞範本（未來寫 skill 用）

### 4.1 每日台股 workflow（tw-market.yml）
```yaml
name: 台股每日儀表板
on:
  schedule:
    - cron: '0 9 * * 1-5'   # UTC 9:00 = 台灣 17:00
  workflow_dispatch:
concurrency:
  group: quant-dashboard
  cancel-in-progress: false
timeout-minutes: 30
jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: write
      pages: write
    steps:
      - uses: actions/checkout@v5
      - uses: actions/setup-python@v6
        with:
          python-version: '3.11'
          cache: 'pip'
      - run: pip install -r requirements.txt
      - name: 抓台股+合併美股
        env:
          FINMIND_TOKEN: ${{ secrets.FINMIND_TOKEN }}
        run: |
          python scripts/fetch_tw.py
          python scripts/fetch_us.py --use-cache
      - name: 選股+回測+匯出 JSON
        run: |
          python scripts/daily_stock_pick.py
          python scripts/backtest.py
          python scripts/portfolio_sample.py
          python scripts/export_json.py
      - name: 前端 build (React/Vue)
        working-directory: frontend
        run: |
          npm ci
          npm run build
      - name: 部署準備
        run: |
          rm -rf docs && cp -r frontend/dist docs
          git config user.name "github-actions"
          git config user.email "actions@github.com"
          git add docs/ data/
          git commit -m "daily update $(date +%F)" || echo "no change"
          git push
```

### 4.2 部署 Pages（deploy-pages.yml）
```yaml
name: Deploy Pages
on:
  push:
    branches: [main]
permissions:
  pages: write
  id-token: write
jobs:
  deploy:
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - uses: actions/checkout@v5
      - uses: actions/configure-pages@v5
      - uses: actions/upload-pages-artifact@v3
        with:
          path: docs/
      - id: deployment
        uses: actions/deploy-pages@v4
```

### 4.3 示警 workflow（stale-check.yml）
```yaml
name: 停滯檢查
on:
  schedule:
    - cron: '0 12 * * 1-5'   # 台灣 20:00 檢查當日是否更新
  workflow_dispatch:
jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v5
      - name: 比對 docs 最新日期 vs 交易日
        run: python scripts/check_stale.py
        # 若停滯 → 呼叫 Telegram Bot API + 發 Email
```

---

6. ~~Python+Plotly 勝前端框架~~（已推翻：2026-07-13 後段改 React/Vue 為主，詳見 Q9）
7. **雙示警（Telegram+Email）**：單通道可能漏，雙通道互補
8. **前端技術棧定型**：React + TypeScript + Tailwind + shadcn/ui（用戶明確指定，見第六節模組規格）

---

## 六、15 模組 Dashboard 設計規格（用戶提供）

> 用戶給出完整 15 模組規格，目標風格參考 Bloomberg Terminal / TradingView / OpenBB / Koyfin。
> 技術棧：**React + TypeScript + Tailwind + shadcn/ui**（SPA，讀 `data/*.json` 渲染）。
> ⚠️ 本節僅記錄規格與「與現有 skills 整合分析」，不實作。

### 6.0 架構導航（用戶指定）
```
🏠 Dashboard      📈 Market        🔍 Screener
🤖 AI Analysis    📊 Portfolio     📑 Backtest
🧠 Strategy Center 📦 ETF Center   📰 News
🔥 Heatmap        📅 Calendar      ⚙️ Settings
📝 Report         🤖 Hermes Agent
```

### 6.1 股票首頁 Dashboard
- **左側**：Watch List / 自選股 / ETF / 市場分類
- **中間**：K線 / 成交量 / 技術指標 / AI分析摘要
- **右側**：最新新聞 / AI重點整理 / 今日事件 / 財報提醒
- **上方**：搜尋股票 / 全市場搜尋 / ETF搜尋
- **資料來源**：`data/market.json`（TWSE+TPEX）+ `data/news.json`

### 6.2 AI 股票分析
- 輸入股票代號 → Hermes 自動分析：基本面/技術面/籌碼/ETF持股/法人買賣/市值/估值/成長性
- 輸出：AI Summary + AI Score
- **整合**：Hermes `quant-trading` 的 analyzer 邏輯（現有 daily_stock_pick 評分可擴充為 AI Score）

### 6.3 多因子評分（Multi-Factor）
- 因子：Value / Growth / Momentum / Quality / Volatility / Liquidity
- 每因子：Score / Rank / 百分位 + 雷達圖
- **整合**：`quant-trading` 的 `daily_stock_pick.py` 多因子模型直接產 `factor_scores.json`

### 6.4 ETF Dashboard
- ETF清單 / 持股 / 產業分布 / 十大持股 / 殖利率 / 管理費 / 追蹤誤差 / AI分析 / ETF比較
- **整合**：`etf-active-stock` wiki + `quant-trading` DB（ETF 清單/配息頻率）

### 6.5 投資組合（Portfolio）
- 我的持股 / 每日報酬 / 累積報酬 / 成本 / 現值 / 未實現 / 已實現 / 股息 / IRR / XIRR / Sharpe / Sortino / Beta / Alpha（全視覺化）
- **資料**：`data/portfolio-data.csv`（手填）+ `portfolio.json`（計算後）
- **整合**：Dataview 邏輯（現有持倉頁 calculus）轉 Python 計算

### 6.6 市場總覽（Market）
- 上市/上櫃/ETF / 市值排行 / 成交量排行 / 成交值排行 / 漲停 / 跌停 / 外資 / 投信 / 自營商 / AI每日重點
- **資料**：`data/market.json` + `data/institution.json`（三大法人）

### 6.7 回測（Backtest）
- 建立策略 / 開始-結束日期 / 交易成本 / 績效 / 年化 / MDD / Sharpe / 勝率 / Equity Curve
- **整合**：`quant-trading` 的 `backtest.py`（現有回測引擎）

### 6.8 策略中心（Strategy Center）
- MA / KD / RSI / MACD / Momentum / Breakout / Value / Quality / Growth / Dividend / Low Volatility / Multi-Factor
- 每策略：說明 / 參數 / 回測 / 績效 / AI分析
- **整合**：`quant-trading/strategies/` 現有策略模組

### 6.9 AI News
- Hermes 每天整理：上市新聞 / ETF新聞 / AI摘要 / 新聞分類 / 情緒分析 / 重要程度 / 影響股票
- **整合**：Hermes `daily-news-*` skills（台股/美股/科技新聞）

### 6.10 選股中心（Screener）
- 條件自由組合：市值/成交量/ROE/EPS/殖利率/PB/PE/Momentum/RSI/MACD/法人/ETF
- **整合**：`daily_stock_pick.py` 篩選邏輯 → `screener.json`

### 6.11 股票比較（Compare）
- 最多 5 支：ROE/EPS/PE/PB/殖利率/Beta/市值/技術分析/雷達圖
- **資料**：`data/compare.json`

### 6.12 Heatmap
- 切換：市值 / 成交量 / 漲跌 / 產業 / ETF
- **資料**：`data/heatmap.json`

### 6.13 Hermes AI Chat
- 自然語言問股：「今天哪些符合 Momentum？」「找 ROE>20%」「分析 2330」「分析 0050」
- **整合**：Hermes Agent API（本地 loopback，Actions 側需 self-hosted runner 才連得到）

### 6.14 任務中心（Task Center）
- Hermes 可執行：更新資料 / 更新財報 / 分析股票 / 產生報告 / 部署 GitHub / Telegram通知（看進度）
- **整合**：Hermes cron + delegate_task

### 6.15 設定中心（Settings）
- API 設定：OpenAI / Gemini / OpenRouter / FinMind / TWSE / SQLite（Web 介面）
- ⚠️ **衝突點**：Settings 頁面要改 API Key，但 Pages 是靜態不能存——解法是 Settings 改 `repo variables`/`secrets`（經 GitHub API 或本地 Hermes 編輯），非前端直接存

---

### 6.16 與現有 skills 整合對照表

| 模組 | 現有 skill / 資產 | 整合方式 |
| --- | --- | --- |
| 6.1 首頁 | `quant-trading` DB / `tw_stock_all.db` | fetch_tw.py 產 market.json |
| 6.2 AI分析 | `quant-trading` analyzer / daily_stock_pick | 擴充為 AI Score 產出 |
| 6.3 多因子 | `daily_stock_pick.py` 多因子模型 | 直接產 factor_scores.json |
| 6.4 ETF | `etf-active-stock` wiki / `quant-trading` ETF 表 | DB 查 ETF 清單/配息 |
| 6.5 投資組合 | Obsidian 持倉頁 Dataview 邏輯 | 轉 Python 計算 IRR/XIRR/Sharpe |
| 6.6 市場 | `quant-trading` market_cap | institution.json（三大法人） |
| 6.7 回測 | `backtest.py` | 直接套用 |
| 6.8 策略 | `quant-trading/strategies/` | 12 策略模組映射 |
| 6.9 News | `daily-news-twstock` / `daily-news-usstock` / `daily-news-technology` | Hermes 每日整理產 news.json |
| 6.10 Screener | `daily_stock_pick.py` | 條件篩選產 screener.json |
| 6.13 Chat | Hermes Agent API | ⚠️ Actions 側需 self-hosted runner |
| 6.14 任務 | Hermes cron / delegate_task | 本地觸發，非 Pages 功能 |
| 6.15 設定 | GitHub Variables/Secrets | 經 API 或本地編輯，非前端存 |

### 6.17 實作優先級建議（待確認）
- **Phase 1（MVP）**：6.1 首頁 + 6.6 市場 + 6.4 ETF（純展示，資料來自 TWSE/TPEX）
- **Phase 2**：6.3 多因子 + 6.10 Screener + 6.12 Heatmap（計算型）
- **Phase 3**：6.7 回測 + 6.8 策略中心（需 backtest.py 接前端）
- **Phase 4**：6.2 AI分析 + 6.9 News + 6.13 Chat（需 Hermes API 串接）
- **Phase 5**：6.5 投資組合 + 6.14 任務 + 6.15 設定（需後端/self-hosted）

> ⚠️ 6.13/6.14/6.15 涉及「動態/後端/API Key」，純靜態 Pages 無法完整實現，需 self-hosted runner 或 Hermes 本地輔助。

---

## 五、學習點（提煉 skill 時注意）

1. **Actions 與 Pages 是兩個獨立功能**：Actions 算、Pages 秀；串接靠 `deploy-pages.yml` 監聽 push
2. **每日一次 push 防覆蓋**：雙排程若各 push 會衝突；解法是 08:30 只存 data/、17:00 合併後一次 push
3. **備援資料源是生產級必要**：主源（TWSE/TPEX）偶爾延遲/維護，備援鏈（yfinance/FinMind/OpenBB）保證不斷
4. **靜態網頁不能收表單**：手填資料用「repo CSV 網頁編輯」繞過，符合「無程式碼」要求
5. **匿名公開站**：金融數據敏感，真實持倉絕不 push；用測試示範資料 + 免責聲明
6. **Python+Plotly 勝前端框架**：MVP 階段最低複雜度，未來要互動再升級
7. **雙示警（Telegram+Email）**：單通道可能漏，雙通道互補

---

## 六、未來 skill 化建議

當實作完成（Phase 0~5），可提煉為 Hermes skill `github-quant-dashboard`：
- **觸發**：用戶說「建股市儀表板」「部署 quant-dashboard」「更新公開分析站」
- **scripts/**：fetch_tw.py / fetch_us.py / build_dashboard.py / check_stale.py
- **references/**：本頁（問答精華）+ daily_stock_analysis 分析
- **SKILL.md**：雙排程架構 + 雙源備援 + 匿名原則 + CSV 手填流程

> ⚠️ 本頁僅記錄，不實作。實作待架構全數確認後進 Phase 0。

## 相關節點
- [[finance/quant-dashboard|quant-dashboard 專案架構]]
- [[finance/github-actions-pages-stock-analysis|GitHub Actions/Pages 股市應用研究]]
- [[quant-python-ai-agent|量化 Python AI Agent]]
