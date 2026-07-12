---
title: "quant-dashboard 專案架構"
description: "ivanhsia/quant-dashboard 股市公開儀表板專案——GitHub Actions + Pages 配合架構、雙排程、多資料源備援、ABCD 四功能規劃"
summary: "新專案架構文：Actions 為主+Hermes 備援，雙排程(08:30美股/17:00台股)，TWSE+TPEX 主源+FinMind/yfinance/OpenBB 備援，公開 Pages 展示大盤/選股/回測/測試持倉四功能"
type: resource
status: active
tags:
  - etf
  - finance
created: 2026-07-13
updated: 2026-07-13
---

# quant-dashboard 專案架構

> 狀態：**規劃中（尚未實作）**，本頁為架構決策記錄，供逐步討論。
> Repo：`ivanhsia/quant-dashboard`（GitHub Public + Pages 啟用）
> 研究基礎：[[finance/github-actions-pages-stock-analysis|GitHub Actions/Pages 股市應用研究]]

---

## 一、專案定位

| 項目 | 決策 |
| --- | --- |
| 目標 | 公開股市儀表板網站（任何人可訪問），展示台股/美股分析 |
| 運算主力 | **GitHub Actions**（定時跑腳本、生成靜態網頁） |
| 輔助角色 | **Hermes Agent**（僅開發/維護/備援，不參與日常運行） |
| 展示層 | **GitHub Pages**（靜態託管，免伺服器） |
| 雙源架構 | Actions 主力 + Hermes 備援（任一路可獨立產出） |
| 資料隱私 | 公開「測試持倉示範資料」；真實持倉留本地，絕不 push |

---

## 二、系統架構圖

```
╔══════════════════════════════════════════════════════════════════════════╗
║              GitHub Repo: ivanhsia/quant-dashboard (Public + Pages)       ║
╠══════════════════════════════════════════════════════════════════════════╣
║                                                                          ║
║  ┌── .github/workflows/ ──────────────────────────────────────────────┐ ║
║  │                                                                      │ ║
║  │  (1) us-market.yml  ■ 排程 08:30 (台灣)                              │ ║
║  │      on: schedule '30 0 * * 1-5'  # UTC 0:30 = 台灣 08:30           │ ║
║  │      → 抓前一晚美股收盤 → 建美股資料 → 更新 docs/us/                │ ║
║  │                                                                      │ ║
║  │  (2) tw-market.yml  ■ 排程 17:00 (台灣)                              │ ║
║  │      on: schedule '0 9 * * 1-5'   # UTC 9:00 = 台灣 17:00           │ ║
║  │      → 抓台股收盤 → 選股+回測 → 生成 docs/{index,picks,backtest}    │ ║
║  │                                                                      │ ║
║  │  (3) deploy-pages.yml  ■ 監聽 main push 自動部署                    │ ║
║  │      on: push {branches:[main]}  → actions/deploy-pages@v4          │ ║
║  │                                                                      │ ║
║  │  (4) hermes-backup.yml  ■ 手動 workflow_dispatch 僅備援             │ ║
║  │      (Hermes 本地跑完 push 時觸發，或 Actions 失敗手動補)            │ ║
║  └──────────────────────────────────────────────────────────────────────┘ ║
║                                  │                                       ║
║                                  ▼                                       ║
║  ┌── Repo 結構 ──────────────────────────────────────────────────────┐ ║
║  │  scripts/                                                          │ ║
║  │   ├── fetch_tw.py       台股抓取 (TWSE+TPEX 主, 備援 yfinance)    │ ║
║  │   ├── fetch_us.py       美股抓取 (yfinance/FinMind/OpenBB)        │ ║
║  │   ├── daily_stock_pick.py 選股信號                               │ ║
║  │   ├── backtest.py       回測引擎                                │ ║
║  │   ├── build_dashboard.py 生成 HTML (Plotly)                     │ ║
║  │   └── portfolio_sample.py 測試持倉產生器                        │ ║
║  │  data/    (CSV/SQLite, Actions 運行時生成, gitignore 大檔)      │ ║
║  │  docs/    (Pages 來源 = 靜態網頁, 以下為產出)                    │ ║
║  │   ├── index.html        [A] 大盤/ETF 儀表板                     │ ║
║  │   ├── picks.html         [B] 每日選股                           │ ║
║  │   ├── backtest.html      [C] 回測績效                           │ ║
║  │   ├── portfolio.html      [D] 測試持倉示範                      │ ║
║  │   └── us/index.html      [A-us] 美股儀表板                      │ ║
║  │  requirements.txt / .gitignore                                   │ ║
║  └──────────────────────────────────────────────────────────────────────┘ ║
║                                  │  git push main                       ║
║                                  ▼                                       ║
║  ☁ GitHub Pages: https://ivanhsia.github.io/quant-dashboard/             ║
║     ┌────────────────────────────────────────────────┐                 ║
║     │ [A] 台股大盤/ETF 儀表板  [A-us] 美股儀表板       │                 ║
║     │ [B] 每日選股推薦         [C] 回測績效展示         │                 ║
║     │ [D] 測試持倉示範 (標註非真實部位)                │                 ║
║     └────────────────────────────────────────────────┘                 ║
╚══════════════════════════════════════════════════════════════════════════╝
        │                                      │
        ▼                                      ▼
┌──────────────────────────┐      ┌──────────────────────────────────────┐
│  Hermes Agent (本地)       │      │  未來擴充                              │
│  ① 開發/維護 scripts+yml  │      │  • Telegram 每日推播 (新聞式)         │
│  ② 備援：Actions 掛掉時    │      │  • Quartz 知識庫交叉連結             │
│     手動跑補 push          │      │  • 前端框架升級 (React/Vue) 增強互動 │
│  ③ 本地先測腳本再推 repo   │      │  • 真實持倉接入 (需隱私評估)         │
│  ⚠️ 不設每日 cron (避衝突) │      │                                     │
└──────────────────────────┘      └──────────────────────────────────────┘
```

---

## 三、雙排程流程圖

### 排程 ①：美股數據建立（08:30 台灣）
```
台灣 08:30 ──── Actions 觸發 (us-market.yml)
    │
    ├─ 前一晚美股已收盤 (美東 16:00 = 台灣次日 04:00 夏令/05:00 冬令)
    │   → 08:30 抓資料時美股資料已完整
    ▼
fetch_us.py
    ├─ 主源: yfinance (免 token)
    ├─ 備援: FinMind API / OpenBB (依 vars 配置)
    └─ 失敗處理: 主源 timeout → 自動切備援 → 仍失敗則標記「資料缺失」不中斷
    ▼
寫入 data/us_market.csv (或 SQLite)
    ▼
build_dashboard.py → docs/us/index.html (美股儀表板)
    ▼
git push → deploy-pages.yml → Pages 更新 [A-us]
```

### 排程 ②：台股分析 + 發布（17:00 台灣）
```
台灣 17:00 ──── Actions 觸發 (tw-market.yml)
    │  (台股 13:30 收盤 + 盤後作業，17:00 資料完整)
    ▼
fetch_tw.py
    ├─ 主源: TWSE OpenAPI STOCK_DAY_ALL + TPEX OpenAPI v1
    ├─ 備援: yfinance / FinMind / OpenBB (依 vars 配置)
    └─ 驗證: 隨機抽驗 5 檔 yfinance 比對 (沿用 quant-trading 邏輯)
    ▼
┌─── 並行三路 ───────────────────────────────┐
▼                  ▼                  ▼
daily_stock_pick   backtest.py        build_dashboard.py
[B] 選股信號        [C] 回測績效        [A] 台股大盤/ETF
picks.html          backtest.html       index.html
                                          │
                                          ▼
                                  portfolio_sample.py
                                  [D] 測試持倉示範 portfolio.html
    ▼
git push → deploy-pages.yml → Pages 更新 [A][B][C][D]
```

---

## 四、各功能說明 (ABCD)

### [A] 大盤 / ETF 每日儀表板
- **內容**：台股加權指數、ETF 列表（含你關注的主動式/平衡型 T 尾碼）、技術指標（MA/RSI/KD）、漲跌家數
- **美股版 [A-us]**：S&P500 / Nasdaq / 個股
- **圖表**：Plotly K 線 + 均線、ETF 比較長條圖
- **資料**：fetch_tw.py / fetch_us.py

### [B] 每日選股推薦
- **內容**：daily_stock_pick.py 產出的多因子評分 Top N（短/中/長期）
- **展示**：表格（代號/名稱/評分/理由）+ 產業分佈圓餅圖
- **頻率**：每交易日 17:00 更新

### [C] 回測績效展示
- **內容**：backtest.py 跑策略的績效（權益曲線、夏普、最大回撤、勝率）
- **展示**：權益曲線圖 + 指標卡 + 交易明細表
- **資料**：本地回測結果 json → HTML

### [D] 測試持倉示範
- **內容**：portfolio_sample.py 產生的「示範持倉」
  - 用你真實持倉的**格式 + 標的**（如 00981A 平衡型 ETF、某台積電部位）
  - 股數/成本用**範例值**，頁面明確標註「📋 測試示範資料，非真實部位」
- **計算**：沿用 Dataview 邏輯（市值=價×股數、損益=價−成本）
- **未來**：你真實資料增減時，改 portfolio_sample.py 的來源陣列即可；真實明細留本地不 push

---

## 五、工具與技術棧

### 運算 / 部署工具
| 工具 | 用途 | 備註 |
| --- | --- | --- |
| **GitHub Actions** | 定時運算 + 自動部署 | `schedule` + `deploy-pages` |
| **GitHub Pages** | 靜態網頁託管 | 公開、免伺服器 |
| **Python 3.11** | 腳本語言 | Actions `setup-python` |
| **Plotly** | 圖表生成（烤進 HTML） | 免前端框架，最低複雜度 |
| **Hermes Agent** | 開發/維護/備援 | 本地，不日常運行 |

### 資料源（主 + 備援）
| 市場 | 主源（優先） | 備援（依 vars 切換） |
| --- | --- | --- |
| 台股 | TWSE OpenAPI `STOCK_DAY_ALL` | yfinance / FinMind / OpenBB |
| 上櫃 ETF | TPEX OpenAPI v1 | yfinance（過濾 ETF 清單） |
| 美股 | yfinance（免 token） | FinMind / OpenBB |

> **備援策略**：每個 fetch 函式實作 `try 主源 except 切備援`；主備都失敗 → 標記該市場「資料缺失」並繼續其他市場，不中斷整體流程（沿用 quant-trading 的「異常不寫入」原則）。

### Secrets / Variables（Actions 側）
- `TWSE` / `TPEX`：免 token（公開 API）
- `FINMIND_TOKEN` / `OPENBB_TOKEN`：備援用（選配）
- `YFINANCE`：免 token
- 股票清單：`environment:` 或 `vars.STOCK_LIST`（隔離配置）

---

## 六、待討論的開放問題（持續補充）

> 以下為 Agent 提出、待用戶決策的項目。逐項確認後更新本頁。

### 已決策
- ✅ 雙排程：08:30 美股 / 17:00 台股
- ✅ 資料源：TWSE+TPEX 主，FinMind/yfinance/OpenBB 備援
- ✅ 公開 + 測試持倉（非真實）
- ✅ Python+Plotly（非前端框架，MVP 階段）

### 待確認
1. **美股盤前/盤後資料**：08:30 抓的是「前一晚收盤」，是否需含盤後交易（extended hours）？還是只抓 regular hours 收盤？
2. **選股 [B] 的標的池**：只台股？還是含美股？每日選股數量（目前 quant-trading 是 15 檔）？
3. **回測 [C] 的策略來源**：用 quant-trading 現有策略？還是另寫公開版策略（避免暴露你的私密邏輯）？
4. **持倉 [D] 的標的範圍**：只用 ETF（如 00981A）還是含個股（台積電）？測試資料股數用固定範例還是隨機生成？
5. **Telegram 推播**：未來是否要在每日 Pages 更新後，發一則「今日儀表板已更新」到你的 Telegram？（Actions 可直接 call Telegram Bot API）
6. **頁面語言**：全繁體中文？還是中英雙語？
7. **資料保留**：`data/` 下的 CSV 要 git 留痕（可追溯）還是 gitignore（只留網頁產出）？
8. **Hermes 備援的觸發方式**：Actions 失敗時（如 API 全掛），你希望 Hermes 自動偵測補跑？還是手動發指令？

---

## 七、實作階段規劃（待架構定稿後）

| 階段 | 範圍 | 產出 |
| --- | --- | --- |
| Phase 0 | 建 repo + 啟用 Pages + 最小 workflow | 空站可訪問 |
| Phase 1 | [A] 台股儀表板（fetch_tw + build_dashboard） | 每日大盤頁 |
| Phase 2 | 雙排程 + 美股 [A-us] | 08:30/17:00 雙更新 |
| Phase 3 | [B] 選股 + [C] 回測 | 完整分析頁 |
| Phase 4 | [D] 測試持倉示範 | 持倉頁 |
| Phase 5 | 備援資料源接入 + Hermes 備援流程 | 韌性強化 |

> ⚠️ 本階段只討論架構，不實作。確認無誤後再進 Phase 0。

## 相關節點
- [[finance/github-actions-pages-stock-analysis|GitHub Actions/Pages 股市應用研究]]
- [[quant-python-ai-agent|量化 Python AI Agent]]
- [[finance/etf-active-stock/etf-active-stock|台灣主動式 ETF 清單]]
- [[finance/etf-code-classification|ETF 代碼分類與第六碼意義]]
