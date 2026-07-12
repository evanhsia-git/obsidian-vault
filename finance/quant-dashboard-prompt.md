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
