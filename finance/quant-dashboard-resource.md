---
title: "quant-dashboard 資源清單"
description: "股市 Dashboard 開發的參考資源——分析平台/Python 框架/Pages 框架/UI 庫/圖表庫/資料源/學習順序；並標註與 quant-dashboard-skill 的採用狀態。"
summary: "quant-dashboard 參考資源頁：OpenBB/TradingView 等平台、Streamlit/Dash 等 Python 框架、Quartz/Astro 等靜態框架、shadcn/Tremor UI、Plotly/ECharts 圖表、10 家 Provider 資料源；每項標註「我們狀態：採用/參考/備援/未採用」以對齊 skill。"
type: project
status: active
tags:
  - etf
  - finance
created: 2026-07-13
updated: 2026-07-14
---

# quant-dashboard 資源清單

> 參考資源彙整，供 [[quant-dashboard-v2-100個功能|quant-dashboard 專案架構]] 與 [[finance/quant-dashboard-prompt|實作紀錄]] 選型使用。
> 來源：用戶提供《Python 股票分析 Dashboard 開發指南（Hermes Edition）》。
> **對齊依據**：本頁每項標註「我們狀態」以對照可部署 Skill 正文 [[quant-dashboard-skill|quant-dashboard 可部署 Skill]] 與架構主文 [[finance/quant-dashboard|quant-dashboard 架構主文 v3.0]]。

**我們狀態圖例**：`採用` = skill/主文已納入正式技術棧；`參考` = UI/UX 或實作借鏡但不直接依賴；`備援` = Tier2/3 設 secret 才啟用；`未採用` = 替代路線，當前架構不選。

### 我們技術對照總表（skill §二 / §四 / §六）
| 類別 | 我們採用 | 來源章節 |
|------|----------|----------|
| 前端框架 | React + TS + Vite | skill §二 |
| UI | Tailwind + shadcn/ui | skill §二 / §四 |
| 圖表 | ECharts | skill §二 |
| 表格 | TanStack Table（虛擬化） | skill §二 |
| 後端腳本 | Python 3.11 純腳本（產 `data/*.json`，免 DB） | skill §二 / §五 |
| 部署 | Actions artifact-based Pages + `make deploy` | skill §二 / §九 |
| 資料源 Tier1 | TWSE / TPEx / Yahoo / Stooq（免 key） | skill §四 |
| 資料源 Tier2/3 | FinMind / Alpha Vantage / FMP / Finnhub / Polygon / Bloomberg（設 secret） | skill §四 |

---

## 一、股票分析平台（參考對象）
| 名稱 | 官方網站 | GitHub | 推薦 | 我們狀態 |
| --- | --- | --- | --- | --- |
| OpenBB | https://openbb.co | https://github.com/OpenBB-finance/OpenBB | ⭐⭐⭐⭐⭐ | 參考（UI/UX 風格） |
| TradingView | https://www.tradingview.com | - | ⭐⭐⭐⭐⭐ | 參考（UI/UX 風格；但非即時盤，見主文 §4 新手心態） |
| Finviz | https://finviz.com | - | ⭐⭐⭐⭐⭐ | 參考（Heatmap / Screener 概念） |
| Koyfin | https://www.koyfin.com | - | ⭐⭐⭐⭐ | 參考（複合視圖） |
| Simply Wall St | https://simplywall.st | - | ⭐⭐⭐⭐ | 參考（財報燈號視覺） |
| StockAnalysis | https://stockanalysis.com | - | ⭐⭐⭐⭐ | 參考 |
| MacroTrends | https://www.macrotrends.net | - | ⭐⭐⭐⭐ | 參考 |
| CompaniesMarketCap | https://companiesmarketcap.com | - | ⭐⭐⭐⭐⭐ | 參考（市值排名） |

> 我們的 Dashboard 風格參考 Bloomberg Terminal / TradingView / OpenBB / Koyfin（見 prompt 頁 6.0）。這些皆為**參考對象**，非我們的依賴或 fork 來源。

---

## 二、Python Dashboard 框架
| 框架 | 網址 | GitHub | 用途 | 我們狀態 |
| --- | --- | --- | --- | --- |
| Streamlit | https://streamlit.io | streamlit/streamlit | 快速 Dashboard | 未採用（React 替代） |
| Dash | https://dash.plotly.com | plotly/dash | 專業互動圖表 | 未採用（React 替代） |
| NiceGUI | https://nicegui.io | zauberzeug/nicegui | 全 Python Web UI | 未採用 |
| Panel | https://panel.holoviz.org | holoviz/panel | 科學分析 | 未採用 |
| Bokeh | https://bokeh.org | bokeh/bokeh | 互動圖表 | 未採用 |
| PyGWalker | https://docs.kanaries.net/pygwalker | Kanaries/pygwalker | 視覺分析 | 未採用 |
| Gradio | https://gradio.app | gradio-app/gradio | AI Demo | 未採用 |
| Reflex | https://reflex.dev | reflex-dev/reflex | Python Full Stack | 未採用（替代路線：若未來想零前端學習可重構，但當前選 React） |

> ⚠️ **與我們架構的關係**：我們已決定前端用 **React+TS+Tailwind+shadcn/ui**（非 Python 框架，見 skill §二）。上表是「全 Python 方案」替代路線——當前不採用。

---

## 三、GitHub Pages / 文件框架
| 框架 | 網址 | 我們狀態 |
| --- | --- | --- |
| Quartz | https://quartz.jzhao.xyz | 未採用（本專案）；但**我們 Obsidian Vault 已用 Quartz 發布**（另項，非 dashboard 本身） |
| Material for MkDocs | https://squidfunk.github.io/mkdocs-material/ | 未採用 |
| Docusaurus | https://docusaurus.io/showcase | 未採用 |
| Astro | https://astro.build/showcase | 未採用 |
| Hugo | https://gohugo.io/showcase/ | 未採用 |
| Jekyll | https://jekyllrb.com/showcase/ | 未採用 |
| VitePress | https://vitepress.dev | 未採用 |
| Nextra | https://nextra.site | 未採用 |

> 我們的 quant-dashboard 用 **React (Vite) 直接 build → Pages**（見 skill §二 / 主文 §10 樹），不走上述文件框架（那些是文件/部落格型，非 Dashboard 型）。Quartz 僅用於本 vault 文件站，與 dashboard 部署無關。

---

## 四、Dashboard UI 元件庫
| 庫 | 網址 | 我們狀態 |
| --- | --- | --- |
| Tremor | https://www.tremor.so | 參考（Dashboard 元件概念） |
| shadcn/ui | https://ui.shadcn.com | **採用**（skill §二 / §四） |
| Tabler | https://tabler.io | 未採用 |
| Flowbite | https://flowbite.com | 未採用 |
| Mantine | https://mantine.dev | 未採用 |
| Aceternity UI | https://ui.aceternity.com | 未採用 |

---

## 五、圖表庫
| 庫 | 網址 | 我們狀態 |
| --- | --- | --- |
| Plotly | https://plotly.com | 參考（React 側可用 `plotly.js` 畫 K線/權益曲線；非主選） |
| Apache ECharts | https://echarts.apache.org | **採用**（skill §二；React 側用 `echarts-for-react`） |
| Chart.js | https://www.chartjs.org | 未採用（原指南曾標採用，已與 skill 對齊改為 ECharts） |
| Highcharts | https://www.highcharts.com | 未採用（商用授權） |

> React 側主圖表為 **ECharts**（`echarts-for-react`），K線/雷達圖/權益曲線皆可用；Plotly 僅作備選參考。

---

## 六、資料源（Data Provider Layer）
對齊 skill §四 10 provider 模型（Tier1 免 key 主源；Tier2/3 設 secret 才實例化）。

| Provider | 網址 | Tier | 市場 | 我們狀態 |
| --- | --- | --- | --- | --- |
| TWSE | https://www.twse.com.tw | 1 | TW 上市 | **採用（主源，免 key）** |
| TPEx | https://www.tpex.org.tw | 1 | TPEX 上櫃/ETF | **採用（主源，免 key）** |
| Yahoo (yfinance) | https://github.com/ranaroussi/yfinance | 1 | TW,US,GLOBAL,指數 | **採用（主源，免 key）**；原指南 `yfinance` 即此 |
| Stooq | https://www.stooq.com | 1 | US,GLOBAL,指數 | **採用（主源，免 key）** |
| FinMind | https://finmindtrade.com | 2 | TW | 備援（設 `FINMIND_TOKEN` 才啟用） |
| Alpha Vantage | https://www.alphavantage.co | 2 | US,GLOBAL | 備援（設 `ALPHAVANTAGE_KEY`） |
| FMP | https://financialmodelingprep.com | 2 | US,GLOBAL | 備援（設 `FMP_KEY`） |
| Finnhub | https://finnhub.io | 2 | US,GLOBAL | 備援（設 `FINNHUB_KEY`） |
| Polygon | https://polygon.io | 3 | US,GLOBAL | 備援（設 `POLYGON_KEY`） |
| Bloomberg | https://www.bloomberg.com | 3 | GLOBAL | 備援（設 `BLOOMBERG_KEY`） |

> Tier1（TWSE/TPEx/Yahoo/Stooq）為 Fork 即跑主源，免 API Key；Tier2/3 為選配備援，僅當使用者於 repo 設定對應 secret 才進 Provider 池（見 skill §四 Registry / 紅線）。原指南僅列 TWSE/TPEx/FinMind/yfinance，已擴充對齊 10 provider 模型。

---

## 七、建議架構（原指南版，供對照）
```
GitHub Actions → Python → FinMind/yfinance → 策略分析 → Plotly
→ Jinja2 → HTML Dashboard → GitHub Pages → Obsidian → Hermes Agent
```
> 此為「Python 全包」路線。我們的架構（見主文 v3.0 / skill）改為 **React 前端 + 雙源解耦 + Data Contract + Provider Layer**，更適合長期擴充。狀態：`未採用（參考對照）`。

---

## 八、學習順序（原指南建議）
1. TradingView（參考 UI）
2. OpenBB（參考 UI）
3. Streamlit Gallery（未採用路線）
4. Dash Gallery（未採用路線）
5. NiceGUI（未採用路線）
6. Quartz（我們 Vault 已用）
7. Material for MkDocs（未採用）
8. shadcn/ui（**我們採用**）
9. Tremor（參考）
10. Astro（未採用）

---

## 九、TODO（原指南，對照我們專案現行 Phase）
- [ ] 建立股票 Dashboard → 對應我們 **Phase 0 骨架 + Phase 1 核心**
- [ ] GitHub Actions 自動更新 → 對應**雙排程**（daily-tw / daily-us / deploy.yml）
- [ ] GitHub Pages 發布 → 對應**部署**（Actions artifact-based Pages + `make deploy`）
- [ ] Telegram 通知 → 對應**雙示警**（GitHub Issues + Telegram/Email 設 secret）
- [ ] Obsidian 自動同步 → Quartz 已有（vault 文件站，非 dashboard）
- [ ] Hermes Agent 整合 → 對應 P4 Optional Backend（AI 摘要設 Key 才啟用，Phase2）

> 原指南寫「Phase 1~5 / Admin Phase 6」已過時；現行路線為 **Phase 0 / 1 / 2**（見 skill §九）。

---

## 十、圖表庫與實作參考（用戶提供連結，2026-07-14 收錄）
評估基準：我們採 **React + Vite + Static First**，圖表主庫為 **ECharts**（skill §二）；運算全 Build-Time、零常駐後端。

| 項目 | 網址 | 功能 | 我們狀態 / 能否用於 Pages |
|------|------|------|---------------------------|
| Lightweight Charts 文件 | https://tradingview.github.io/lightweight-charts/docs | TradingView 輕量金融圖表庫（canvas，K線/面積/基線/柱狀） | **可選補強**：K線效能勝 ECharts，但授權須標註 TradingView（NOTICE+連結）。違反單一圖表庫簡單原則（P8），僅當 Asset 個股 K線效能瓶頸時引入，否則維持 ECharts |
| Lightweight Charts 倉庫 | https://github.com/tradingview/lightweight-charts | 同上原始碼；含 AI coding skill（v5 API） | 同上；Apache-2.0，須 attribution |
| ECharts 範例庫 | https://echarts.apache.org/examples/ | 官方範例（K線/雷達/權益曲線模板） | **採用（參考）**：我們圖表主庫，此站為配置抄寫來源，不引入新依賴 |
| Chart.js 倉庫 | https://github.com/chartjs/Chart.js | 通用圖表庫（非金融專精） | **未採用**：金融 K線需外掛；我們選 ECharts |
| D3 Gallery | https://observablehq.com/@d3/gallery | D3 資料視覺化範例（自定義/force/tree/geo） | **僅參考**：學習曲線高、React 整合重；Heatmap/自定圖可借鏡，不直接依賴（P8 簡單優先） |
| github_watch | https://github.com/RohanAdwankar/github_watch | 靜態 treemap 範例：Actions 每日產 `heatmap.json` → Pages，無 DB/無伺服器 | **架構級參考（推薦）**：其「Actions 算→寫 JSON→Pages 渲染」模式 = 我們 §三/§10 骨架；可作 Market Heatmap View（漲跌家數/市值 treemap）實作參考 |
| stock-screener 線上版 | https://xang1234.github.io/stock-screener/ | 多市場選股器靜態 Demo（80+ 篩選/廣度/RRG） | **概念參考**：其 Screener/Ranking/Breadth = 我們 Asset Domain Screener/Ranking + Market 廣度；靜態版功能受限 |
| stock-screener 倉庫 | https://github.com/xang1234/stock-screener | 同上原始碼（Docker+Postgres 重棧；含靜態版） | **概念參考（不引入後端）**：借其篩選邏輯/UI 概念；資料走我們 Tier1 + Build-Time，不引入其 Docker/Postgres |

**stock-screener 可應用功能映射（對照我們 Domain）**

| 它的功能 | 我們對應 Domain | 應用方式 |
|----------|----------------|----------|
| Scan 綜合評分 + 80+ 篩選 + CSV 匯出 | Asset（Screener/Ranking） | 採相同**綜合評分模型**（Minervini/CANSLIM 通過條件→評分門檻）；Build-Time 算好寫 `asset.json` 的 `screener[]/ranking[]`，前端只渲染。最該抄的核心 |
| Market Health & Exposure 0–100 量尺 | 首頁（風險彙整）+ Market | 移植其評分公式（DMA 距離/分發日/VIX）做首頁風險燈；純 Tier1 算，Phase1 可做 |
| Breadth（StockBee 廣度） | Market（Heatmap/漲跌家數） | ±4% 動能/趨勢窗定義直接沿用；`market.json` 漲跌家數加廣度指標 |
| Groups + RRG 相對旋轉圖 | Market（類股強弱，進階） | RRG 適合「市場層級只看好壞」進階視圖；運算貴→歸 Phase2 |
| Watchlist sparkline + 多週期漲跌條 | Portfolio（watchlist）/ 首頁 | UI 直接借；擴 `user.json` watchlist 欄位 |
| Stock Detail 評分面板 | Asset 個股頁 | 評分/燈號面板結構參考 |
| Validation 回測 | Research（Phase2） | 其「確定性可重現」設計值得學；設 Key/運算貴→Phase2 |
| Themes AI / Assistant AI | P4 Optional Backend | 僅設 secret 才啟用，不進核心 |

> 不抄：Live App 後端堆疊（FastAPI/Postgres/Redis/Celery）、Operations 頁（我們用 GitHub Issues/Projects）、Material-UI/Recharts（我們用 shadcn/ui+ECharts）、12 市場擴張（我們聚焦 TW+US）。
> 其 Static Site 模式（預匯出 JSON→Pages 只讀）**驗證我們架構選擇正確**。

**收錄結論**
- 直接採用：ECharts 範例庫（#3）。
- 架構級參考（最值得學）：github_watch（#5，靜態 JSON→Pages 模式）；stock-screener（#6/#7，Screener/廣度概念）。
- 可選補強：Lightweight Charts（#1/#2，僅 K線效能需求時，注意授權標註）。
- 僅靈感 / 不採：D3（#4 借鏡）、Chart.js（#4 未採用）。

---

## 相關節點
- [[quant-dashboard-skill|quant-dashboard 可部署 Skill 正文（執行規範）]]
- [[finance/quant-dashboard|quant-dashboard 架構主文 v3.0（設計依據）]]
- [[quant-dashboard-v2-100個功能|quant-dashboard 專案架構]]
- [[finance/quant-dashboard-prompt|quant-dashboard 實作紀錄與提示詞]]
- [[finance/github-actions-pages-stock-analysis|GitHub Actions/Pages 股市應用研究]]
