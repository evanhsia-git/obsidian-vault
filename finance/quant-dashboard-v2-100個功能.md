---
title: "quant-dashboard-v2(100個功能)"
description: "ivanhsia/quant-dashboard 全平台藍圖 v2.0——八位 Principal 角色重新檢視 Fork-First/GitHub-Native/Static-First 理念後，依 A-Z 分類整理全部功能構想，含 100 個創新功能表與必做/建議做/可選做/創新四級分類。"
type: project
status: archived
tags:
  - etf
  - finance
summary: "quant-dashboard v2.0 百功能藍圖備份（A-Z 100 功能），archived 不編輯。"
created: 2026-07-13
updated: 2026-07-13
---

# GitHub Native Stock Analysis Platform Blueprint v2.0

> 本文定位：**功能藍圖**（What to build），架構落地細節以 Skill 藍圖 [[quant-dashboard-skill|Skill 藍圖]] 為準（How to build）。
> 狀態：規劃中，本文為 A-Z 全量腦力激盪 + 分級，不代表全部立即實作。

## 目錄
0. [八角色理念確認](#0-八角色理念確認)
1. [P1-P8 原則 v2.0（與 v1 對照）](#1-p1-p8-原則-v20與-v1-對照)
2. [Data Source Principles / Provider Pattern 確認](#2-data-source-principles-provider-pattern-確認)
3. [Technology Stack 確認](#3-technology-stack-確認)
4. [A-V 功能分類總表](#4-a-v-功能分類總表)
5. [W. Fork Friendly vs Optional Backend 總表](#5-w-fork-friendly-vs-optional-backend-總表)
6. [X. Future Roadmap（Phase 1-4）](#6-x-future-roadmapphase-1-4)
7. [Y. Architecture Review（五年後重構點）](#7-y-architecture-review五年後重構點)
8. [Z. 100 個創新功能](#8-z-100-個創新功能)
9. [最終分級：必做 / 建議做 / 可選做 / 創新功能](#9-最終分級必做-建議做-可選做-創新功能)

---

## 0. 八角色理念確認

> 依指示，先確認理解，再談功能。以下是八個角色對本專案本質的共識，任何功能提案都要能通過這八句話的檢驗。

| 角色 | 對本專案本質的理解 |
|------|---------------------|
| Principal Software Architect | 這不是三層式應用，是「GitHub Actions = 後端運算、GitHub Pages = 前端展示」的 Static-First 系統。任何功能第一個問題永遠是：**能不能在 Build Time 做完？** |
| Principal React Architect | React 被刻意限縮為 **Render 層**，禁止商業邏輯。導覽改用 **TanStack Router**（非舊版 React Router），config-driven + code splitting。 |
| Principal Python Architect | Python 是唯一計算引擎，跑在 Actions runner（無常駐伺服器），腳本間需保持 idempotent，同一 run 內可快取但不可跨 run 留狀態（Reproducible）。 |
| GitHub Actions Expert | 平台心臟是排程與 workflow；Public repo Actions 分鐘數無限，但仍要顧 job 時長、concurrency、artifact 保存期限（90 天預設）。 |
| GitHub Pages Expert | 展示層是純靜態檔案託管，零伺服器邏輯；一切互動皆為前端 fetch 靜態 JSON，沒有「查詢」只有「篩選已產出的資料」。 |
| DevOps Architect | 部署鏈是 Actions → Artifact → Pages，必須 idempotent + reproducible + fail-safe（單一 Provider 掛掉不可擴散成整站掛）。 |
| Quant Trading Architect | 策略/回測/因子邏輯必須與資料源完全解耦（Provider Pattern），策略程式碼不可 import 任何具體 API。 |
| Open Source Maintainer | 任何新功能的第一個檢查點：**這是否讓 Fork 使用者的門檻變高？** 門檻變高就必須降級為 Optional，並在無該條件時優雅隱藏。 |

**共識結論**：本專案的競爭力不在「功能多」，而在「Fork 之後零門檻就能用的功能多」。功能規劃順序永遠是 **Build Time 優先 → 零 Key 優先 → 選配加值最後**。

---

## 1. P1-P8 原則 v2.0（與 v1 對照）

> 本次使用者提供的 P1-P8 用字與 v1 架構文件略有差異。兩者精神一致，此處做正式合併，**以下 P1-P8 v2.0 為最新標準版本，取代 v1 的 8 條**。

| # | v2.0 原則（本次採用） | 定義 | 對照 v1 舊條文 | 合併說明 |
|---|----------------------|------|----------------|---------|
| P1 | **Fork First** | Fork 即可使用 | v1 P1 Fork First | 不變 |
| P2 | **GitHub Native** | 優先用 GitHub 原生功能（Actions/Pages/Releases/Artifacts/Issues/Discussions/Wiki/Projects） | v1 P2 GitHub Native | 不變，新增 Wiki/Projects 兩項落地場景（見 4-V） |
| P3 | **Static First** | 核心功能 Build Time 完成，React 只 Render | v1 P3 Static First | 不變 |
| P4 | **Optional Backend** | Hermes/FastAPI/SQLite/AI 全部 Optional | v1 P8 Optional Backend | 條號從 P8 移到 P4，內容不變 |
| P5 | **Progressive Enhancement** | 無 Hermes/AI/Key 網站仍正常 | v1 P5 Progressive Enhancement | 不變 |
| P6 | **Build Time over Run Time** | Actions 能做就不要 React 做（Sharpe/Momentum/Ranking/Backtest/Heatmap 全 Build Time） | v1 P3 的延伸 + 新明文化 | **新增獨立條文**：v1 把這點併在 Static First 裡，v2.0 獨立成條，更精確指出「連 Ranking/Heatmap 排序運算都不可留給前端」 |
| P7 | **Open Source Friendly** | 避免付費 API / 大量 Key / 複雜安裝 | v1 P6 OSS Friendly | 條號 P6→P7，內容不變 |
| P8 | **Long-term Maintainability** | 五年後 10 萬行程式仍容易維護 | v1 P7 Reproducible 的精神延伸 | **新增**：v1 的 Reproducible（每次重跑產生相同結果）是「正確性」層次，v2.0 的 Maintainability 是「可持續演進」層次，兩者不衝突，Reproducible 併入 P8 的落實手段之一（見下） |

> v1 的「**Offline Friendly**」（無 API 也能看歷史分析）未被使用者在本次 P1-P8 中重提，但精神仍成立——它是 **P4 Optional Backend + P6 Build Time over Run Time** 的自然結果（資料已在 Build Time 產成 JSON 進 repo，本來就離線可讀）。**建議不刪除，作為 P4/P6 的落實細項保留**，不再列為獨立頂層原則，避免原則數量膨脹、失焦。

**P8 落實手段（含併入的 Reproducible）**：
- Actions 每次從源頭重抓重算，不依賴跨 run 狀態（Reproducible）
- Provider Pattern 保證新增資料源不改動 Strategy 層（見第 2 節）
- Data Contract（TS+Pydantic 1:1 + `schema_version`）防止前後端漂移
- 15+ 模組採 Registry 插件化，新增模組不動核心 router

---

## 2. Data Source Principles / Provider Pattern 確認

沿用並確認既有設計（詳見 Skill 藍圖 Data Provider Layer v2 完整程式碼），本節只做**優先序與可插拔性**的正式確認：

| 優先序 | 條件 | Provider 範例 |
|--------|------|---------------|
| 第一優先 | 官方 / 免費 / 免 API Key | TWSE、TPEx |
| 第二優先 | 免費 / 少量 API Key | Yahoo (yfinance)、Stooq（皆免 key，歸為第一優先亦可）；FinMind（需 Token） |
| 第三優先 | 商業 API | Alpha Vantage、FMP、Finnhub（Tier2 付費方案）、Polygon、Bloomberg（Tier3） |

**可插拔性確認**：`Provider(ABC)` 統一介面（`get_price/get_history/get_dividend/get_financial/get_news`）+ `markets`/`methods` 能力宣告 + `build_registry()` secret-gated 自動發現，已滿足「Provider 必須可插拔」要求——新增一個 Provider 只需新增一個檔案並在 `registry.py` 掛一行，不改 `pool.py` 路由邏輯，也不改任何 Strategy/Calc 程式碼。此設計本身即是 **P8 Long-term Maintainability** 的具體案例。

---

## 3. Technology Stack 確認

| 層 | 本次確認結果 | 與既有文件差異 |
|----|-------------|----------------|
| Frontend | React + TypeScript + Vite + TailwindCSS + shadcn/ui | 不變 |
| 路由 | **TanStack Router** | ⚠️ **變更**：既有文件用 React Router；本次使用者明確指定 TanStack Router。TanStack Router 的型別安全路由 + 內建 loader 更貼合「資料先於元件存在」的 Static-First 精神，**建議採用此變更**，並回頭同步更新架構主文 |
| 表格 | TanStack Table（虛擬化） | 不變 |
| Server State | TanStack Query（若需要） | 不變，標記「若需要」表示純靜態 JSON 場景可用 SWR-lite 手刻取代，避免過度依賴 |
| 圖表 | ECharts | 不變 |
| Backend（Optional） | Hermes Agent + Python + FastAPI + SQLite | 不變，全 Optional |
| CI/CD | GitHub Actions + GitHub Pages | 不變 |

> **待辦**：架構主文（quant-dashboard.md）第七節「技術棧」目前仍寫 React Router，需在下次同步時改為 TanStack Router，並註記變更日期，避免兩份文件路由選型不一致。

---

## 4. A-V 功能分類總表

> 說明：`類型` 欄 = Build（GitHub Actions Build Time 完成）/ React（前端純 render/互動）/ Optional（需 Hermes 等後端）。已與既有 15 模組（6.1-6.15）對照，避免重複定義；新增項目標示 **新**。

### A. Dashboard
| 功能 | 說明 | 類型 |
|------|------|------|
| Watchlist 卡片 | 自選股即時（前日收盤）報價卡 | React |
| 今日重點摘要 | 大盤漲跌/三大法人/焦點股規則摘要 | Build |
| 個人化排序 | 使用者可調整 Dashboard 卡片順序（存 localStorage 替代方案，見紅線） | React |
| 快速跳轉 | Command Palette（⌘K）跳轉任一股票/模組 | React |

### B. Market
| 功能 | 說明 | 類型 |
|------|------|------|
| 大盤總覽 | 加權指數/櫃買指數 K 線 + 成交量 | Build |
| 漲跌家數統計 | 上漲/下跌/平盤家數長條圖 | Build |
| 三大法人買賣超 | 外資/投信/自營商動向 | Build |
| 類股輪動圖 | 產業別漲跌熱力（同 6.12 Heatmap 子集） | Build |

### C. Stock Analysis
| 功能 | 說明 | 類型 |
|------|------|------|
| 個股總覽頁 | 價量/基本面/技術面三合一 | Build |
| 技術指標包 | MA/MACD/KD/RSI/布林通道 | Build |
| 基本面卡 | EPS/ROE/PB/PE/殖利率時序 | Build |
| 規則型分析* | 非 LLM 規則摘要（同 6.2） | Build（AI 摘要 Optional） |
| 個股新聞流 | 同 6.9 News，過濾至單一標的 | Build |

### D. ETF Center
| 功能 | 說明 | 類型 |
|------|------|------|
| ETF 清單/篩選 | 同 6.4 | Build |
| 持股透視 | 成分股/產業配置圓餅圖 | Build |
| 追蹤誤差/管理費比較 | 同類 ETF 比較表 | Build |
| 主動式 ETF 專區 **新** | 台灣主動式 ETF（依既有研究）獨立分類頁 | Build |

### E. Portfolio
| 功能 | 說明 | 類型 |
|------|------|------|
| 持倉輸入 | lots.csv 網頁編輯（同既有設計） | Build+React |
| 績效指標 | IRR/XIRR/Sharpe/最大回撤 | Build |
| 資產配置圖 | 產業/市場/幣別配置 | Build |
| 再平衡建議 **新** | 規則型（非 AI）依目標權重算差額 | Build |

### F. Backtest
| 功能 | 說明 | 類型 |
|------|------|------|
| 策略績效曲線 | Equity Curve + Benchmark 比較 | Build |
| 績效指標卡 | CAGR/Sharpe/Calmar/勝率 | Build |
| 交易明細下載 | CSV/JSON 匯出 | Build |
| 走勢敏感度測試 **新** | 參數掃描（如均線天數）熱力圖 | Build |

### G. Strategy Center
| 功能 | 說明 | 類型 |
|------|------|------|
| 策略清單（Registry） | 同 6.8 | Build |
| 策略說明卡 | 邏輯/適用市場/風險等級 | Build |
| 策略比較 | 多策略績效並排 | Build |
| 社群策略提案 **新** | 透過 GitHub Issue Template 提案，Actions 驗證後合併 | GitHub Native |

### H. Factor Center **新分類**
| 功能 | 說明 | 類型 |
|------|------|------|
| 六因子總覽 | Value/Growth/Momentum/Quality/Volatility/Liquidity | Build |
| 因子貢獻分解 | 個股評分的因子拆解雷達圖 | Build |
| 因子歷史穩定性 | 因子 IC（資訊係數）時序 | Build |
| 因子相關性矩陣 | 因子間相關係數熱力圖 | Build |

### I. News Center
| 功能 | 說明 | 類型 |
|------|------|------|
| 聚合新聞流 | 同 6.9 | Build |
| 情緒標記* | 規則關鍵字或 AI（Optional） | Build（AI Optional） |
| 重要度分級 | 依來源/關鍵字規則評分 | Build |
| 個股新聞去重 | 同story不同來源合併 | Build |

### J. Financial Calendar **新分類**
| 功能 | 說明 | 類型 |
|------|------|------|
| 財報行事曆 | 財報公布日期（公開資料） | Build |
| 除權息行事曆 | 除權息日/現金股利日 | Build |
| 法說會日曆 | 公開資訊觀測站抓取 | Build |
| 到期日提醒 | 期貨/選擇權結算日（若涉略衍生品） | Build |

### K. Market Replay **新分類**
| 功能 | 說明 | 類型 |
|------|------|------|
| 歷史 K 線回放 | 指定日期區間逐日播放 | React（讀歷史 JSON） |
| 事件標記回放 | 標記歷史重大事件對應股價反應 | Build+React |
| 假設情境重演 | 「若當初持有到今天」試算 | Build |

### L. Stock Encyclopedia **新分類**
| 功能 | 說明 | 類型 |
|------|------|------|
| 個股百科頁 | 產業分類/歷史沿革/主要業務 | Build（靜態資料） |
| 同業比較 | 自動歸類同產業標的比較表 | Build |
| 名詞解釋連結 | 財報名詞 hover 提示 | React |

### M. ETF Encyclopedia **新分類**
| 功能 | 說明 | 類型 |
|------|------|------|
| ETF 百科頁 | 發行商/追蹤指數/成立日 | Build |
| ETF 家族樹 | 同發行商系列 ETF 關聯圖 | Build |
| 指數方法論摘要 | 追蹤指數的選股/加權規則摘要 | Build（靜態資料） |

### N. Strategy Encyclopedia **新分類**
| 功能 | 說明 | 類型 |
|------|------|------|
| 策略原理說明 | 均值回歸/動量/因子投資等教學頁 | Build（靜態內容） |
| 經典論文對照 | 策略對應學術出處（不侵權，僅引用摘要） | Build |
| 適用市場條件 | 何種市場環境策略較有效 | Build |

### O. Market Snapshot **新分類**
| 功能 | 說明 | 類型 |
|------|------|------|
| 每日收盤快照 | 當日市場一頁式總結（可存 PDF/PNG） | Build |
| 快照歷史庫 | 過去每日快照可回顧（同 K Market Replay 資料共用） | Build |
| 分享卡片 | 產生社群分享用圖片（OG image） | Build |

### P. Ranking Center
| 功能 | 說明 | 類型 |
|------|------|------|
| 漲跌幅排行 | 日/週/月 | Build |
| 成交量排行 | 同上 | Build |
| 因子評分排行 | 依 Factor Center 分數排序 | Build |
| 法人買超排行 | 外資/投信買超前 N | Build |

### Q. Heatmap
| 功能 | 說明 | 類型 |
|------|------|------|
| 市值熱力圖 | 同 6.12 | Build |
| 產業輪動熱力圖 | 依產業分類漲跌 | Build |
| 因子熱力圖 **新** | 因子分數當日分布 | Build |

### R. Screener
| 功能 | 說明 | 類型 |
|------|------|------|
| 條件自由組合 | 同 6.10 | Build |
| 預設篩選模板 | 存股/成長股/高股息等模板 | Build |
| 篩選結果匯出 | CSV/加入 Watchlist | React |

### S. Compare
| 功能 | 說明 | 類型 |
|------|------|------|
| 多標的雷達比較 | 同 6.11，最多 5 支 | Build |
| ETF vs 個股比較 **新** | 跨類別比較（如 0050 vs 2330） | Build |
| 比較結果分享連結 | URL 帶參數重現比較畫面 | React |

### T. Report Center **新分類**
| 功能 | 說明 | 類型 |
|------|------|------|
| 週報/月報自動產生 | Actions 定期產出 Markdown/PDF | Build |
| 自選股周報 | 依 Watchlist 個人化週報 | Build（需登入態，見 W 節取捨） |
| 報表歷史庫 | 過去報表可回顧下載 | Build |

### U. Knowledge Base **新分類**
| 功能 | 說明 | 類型 |
|------|------|------|
| 財報名詞辭典 | 靜態頁面 + 全文搜尋 | Build |
| 常見問題 FAQ | GitHub Discussions 精選彙整 | GitHub Native |
| 新手教學路徑 | 循序漸進導覽（Onboarding） | React |

### V. GitHub Integration
| 功能 | 說明 | 落地方式 |
|------|------|---------|
| Releases | 每次重大版本發 Release Note | GitHub Releases |
| Wiki | 深度文件/FAQ 放 Wiki，減輕 repo README 負擔 | GitHub Wiki |
| Issues | 停滯偵測示警 + Bug 回報 + 策略提案（同 G） | GitHub Issues |
| Discussions | 社群提問/功能投票 | GitHub Discussions |
| Projects | Roadmap 看板（對應本文 X 節） | GitHub Projects |
| Actions Badge | README 顯示排程執行狀態徽章 | GitHub Actions |
| Pages | 站台本體 | GitHub Pages |
| Sponsors **新** | 若要接受贊助，走 GitHub Sponsors（不影響 Fork First） | GitHub Sponsors |

---

## 5. W. Fork Friendly vs Optional Backend 總表

| 分類 | 判準 | 涵蓋範圍 |
|------|------|---------|
| ✅ **Fork 即可用**（零 secret） | 資料源為 Tier1 免 key + 計算 Build Time 完成 | A/B/C/D/E(基本)/F/G/H/I(規則版)/J/K/L/M/N/O/P/Q/R/S(基本)/U/V 全部；共 12+ 核心模組與其延伸 |
| ⚙️ **Optional：設 Key 才增強** | Tier2/3 資料源 或 LLM 摘要 | C 的 AI 摘要、I 的情緒分析、T 的自選股週報若走 AI 潤飾 |
| 🔒 **Optional：需 Hermes/後端** | 需即時運算或持久化寫入 | 6.13 Chat、6.14 Task、6.15 Settings；E 的「登入態自選股週報」若要跨裝置同步 |

**判斷心法（W 節即 Open Source Maintainer 角色的守門邏輯）**：任何功能先問「零 secret、只有 Build Time 資料，這功能能不能跑？」——能，歸 Fork Friendly；不能但可以降級成靜態版本，仍歸 Fork Friendly + 標記 Optional 加值；完全不能降級（如即時對話），才歸 Optional Backend。

---

## 6. X. Future Roadmap（Phase 1-4）

> 與既有架構文件的 Phase 0-8（實作排程）不同層次：這裡是**產品成熟度**分期，Phase 0-8 是**工程排程**，兩者對照見表末。

| Phase | 目標 | 範圍 | 對應舊 Phase 0-8 |
|-------|------|------|-------------------|
| **Phase 1 — MVP** | 可用的公開股市儀表板 | A Dashboard / B Market / C Stock Analysis / D ETF Center / Q Heatmap（基本）/ R Screener（基本） | 舊 Phase 0-2 |
| **Phase 2 — Advanced** | 量化研究能力成形 | F Backtest / G Strategy / H Factor Center / P Ranking / S Compare | 舊 Phase 3-5 |
| **Phase 3 — Professional** | 內容深度 + 個人化 | E Portfolio（含再平衡）/ I News（情緒）/ J Calendar / K Replay / T Report / U Knowledge Base | 舊 Phase 4（AI 增強部分）+ 新增範圍 |
| **Phase 4 — Enterprise** | 生態與擴充 | 6.13-15 Optional 後端模組 / V GitHub Integration 全量 / L/M/N 百科全書系列 / 社群策略提案（G） | 舊 Phase 6-8 |

> **建議**：Phase 3、4 的百科全書系列（L/M/N）與 Report Center（T）內容量大、屬「長尾價值」而非「首屏必要」，可視社群貢獻（PR）狀況彈性延後，不佔用核心團隊工時。

---

## 7. Y. Architecture Review（五年後重構點）

> 八角色各自從專業角度指出「現在就該改」與「五年後必重構」的項目。

### 現在就應修改
| 項目 | 問題 | 建議 |
|------|------|------|
| 路由選型不一致 | 架構主文寫 React Router，本次確認為 TanStack Router | 立即同步文件（見第 3 節待辦） |
| P1-P8 版本分裂 | v1（quant-dashboard.md）與 v2.0（本文）條文用字不同 | 本文已完成合併對照（見第 1 節），下次應把 v1 文件的第一節直接替換為 v2.0 版本，避免兩份「最高原則」並存造成解讀分歧 |
| Watchlist/自選股的持久化方案未定 | 個人化功能（A/E/T）預設會想用 localStorage，但 Artifact 規範明確禁止瀏覽器儲存於此類靜態站情境類推、且匿名公開站也不該假設單一裝置 | 明確定案：**Fork 使用者的個人化一律走「自己的 repo CSV/JSON + 自己 commit」**，不引入任何形式的雲端帳號系統，維持 P1 Fork First 純度 |
| Provider Tier2/3 商業條款未盤點 | Alpha Vantage/FMP/Finnhub/Polygon/Bloomberg 的免費額度/授權條款會隨時間改變 | 建立 `providers/LICENSE_NOTES.md`，每季度人工複查一次 |

### 五年後大機率需要重構
| 項目 | 原因 | 現在的因應 |
|------|------|-----------|
| 15+ 模組膨脹成 30+ | 功能持續新增，`data/*.json` 檔案數與 `frontend/routes` 會線性成長 | 現在就用 Registry 插件化 + config-driven 導覽（已規劃），未來新增模組不改核心 router，只需注意模組間資料依賴需明確宣告 |
| 全市場 JSON 單檔過大 | ~1700 檔個股 + 因子 + 新聞全塞進去，五年後資料量翻倍 | 現在的 TanStack Table 虛擬化 + 路由懶載是必要但不夠；長期需考慮**分片 JSON**（按產業/字母分檔）而非單一大檔，此為既有「單一 JSON < 500KB」紅線的自然延伸 |
| Actions 執行時間隨模組數增加 | 目前雙排程各自獨立，未來模組一多，17:00 那條 pipeline 會變長 | 現在先用 `needs` 依賴切分 job 平行化；未來可能需要拆成多個 workflow 用 `workflow_call` 組合，而非一條龍腳本 |
| Provider Interface 的 5 個方法可能不夠 | 若未來要支援選擇權/期貨/加密貨幣等新資產類別，`get_price/history/dividend/financial/news` 五法可能不敷使用 | 現在的 `methods: set[str]` 能力宣告設計本身已預留擴充彈性（新增方法只需 provider 各自宣告支援與否），不需要現在就過度設計，但應在 ADR（架構決策記錄）中明文記下「新資產類別 = 新增 method，不是新增 Provider 基底類別」 |
| 策略 Registry 版本控管 | 策略一多，舊策略回測結果需要「凍結」不受程式碼更新影響 | 現在應在策略 Registry 設計中預留 `strategy_version` 欄位（目前規格未明確要求），避免五年後策略修改導致歷史回測結果不可信 |
| 前端 Bundle 隨模組數增加 | 15→30+ 模組，即使 code splitting，共用元件庫也會膨脹 | 現在的 Lighthouse CI 守門（JS < 250KB gzip）需要**逐模組**而非只測首頁，建議現在就把此守門規則明文寫進 CI，而非未來才補 |

---

## 8. Z. 100 個創新功能

> 分十大主題各 10 項，共 100 項。欄位：`適合` = GA（GitHub Actions Build Time）／React（前端）／Opt（Optional Backend）。`Key` = 是否需要 API Key。`Fork` = 是否 Fork 即可用（零 secret 情境下，含優雅降級）。難度/推薦為 1-5 星，以數字表示（5 為最高）。

### Z1. Dashboard & 個人化
| # | 功能名稱 | 用途 | 適合 | Key | Fork | 難度 | 推薦 |
|---|---------|------|------|-----|------|------|------|
| 1 | 主題色盤切換 | 深色/淺色/色盲友善模式 | React | 否 | 是 | 2 | 4 |
| 2 | Dashboard 拖曳排版 | 卡片自由排列，設定存自己 repo JSON | React | 否 | 是 | 3 | 3 |
| 3 | 每日一圖 | 自動選出當日最具代表性的一張圖表 | GA | 否 | 是 | 2 | 3 |
| 4 | 多語系介面 | 中/英/日切換（i18n） | React | 否 | 是 | 3 | 4 |
| 5 | 鍵盤快捷鍵地圖 | 全站快捷鍵一覽 + Command Palette | React | 否 | 是 | 2 | 3 |
| 6 | 首頁自訂 Widget | 使用者可選顯示哪些卡片（存 repo） | React | 否 | 是 | 3 | 3 |
| 7 | 效能儀表板 | 顯示本站 Lighthouse/JSON 大小等透明度指標 | GA | 否 | 是 | 2 | 4 |
| 8 | 部署狀態徽章 | README 顯示最後一次成功排程時間 | GA | 否 | 是 | 1 | 5 |
| 9 | 資料新鮮度提示條 | 全站頂部顯示「資料更新於 X」 | GA | 否 | 是 | 1 | 5 |
| 10 | 無障礙模式 | 螢幕閱讀器/字級放大優化 | React | 否 | 是 | 3 | 4 |

### Z2. Market & 指數分析
| # | 功能名稱 | 用途 | 適合 | Key | Fork | 難度 | 推薦 |
|---|---------|------|------|-----|------|------|------|
| 11 | 大盤本益比河流圖 | 加權指數 PE Band 歷史區間 | GA | 否 | 是 | 3 | 4 |
| 12 | 融資融券餘額追蹤 | 全市場信用交易水位 | GA | 否 | 是 | 3 | 3 |
| 13 | 市場寬度指標 | 站上均線家數比例（Breadth） | GA | 否 | 是 | 2 | 4 |
| 14 | 恐慌貪婪指數（台版） | 仿 CNN Fear & Greed，用台股數據自製 | GA | 否 | 是 | 3 | 5 |
| 15 | 國際指數連動比較 | 台股 vs 標普/日經/恒生同期比較 | GA | 否 | 是 | 2 | 4 |
| 16 | 匯率影響面板 | 台幣匯率對出口股影響提示 | GA | 否 | 是 | 2 | 3 |
| 17 | 期現貨價差追蹤 | 台指期 vs 現貨基差 | GA | 是（期貨源） | 否（需付費源時降級隱藏） | 3 | 3 |
| 18 | 全市場成交量能溫度計 | 量能相對近期均量比例 | GA | 否 | 是 | 2 | 4 |
| 19 | 產業指數輪動時鐘 | 仿美林時鐘台股版 | GA | 否 | 是 | 4 | 4 |
| 20 | 台股波動率指數（自製 VIX） | 用選擇權或歷史波動推算 | GA | 視資料源 | 視資料源 | 4 | 3 |

### Z3. 個股深度分析
| # | 功能名稱 | 用途 | 適合 | Key | Fork | 難度 | 推薦 |
|---|---------|------|------|-----|------|------|------|
| 21 | 股價 vs 分析師合理價 | 用多因子模型自算合理區間（非引用付費分析師） | GA | 否 | 是 | 4 | 4 |
| 22 | 財報成長趨勢燈號 | 營收/EPS/毛利率 YoY 燈號化 | GA | 否 | 是 | 2 | 5 |
| 23 | 護城河評分卡 | 規則式（市佔/毛利穩定度）評估 | GA | 否 | 是 | 3 | 3 |
| 24 | 內部人持股變化 | 董監持股增減趨勢 | GA | 否 | 是 | 2 | 4 |
| 25 | 股利政策穩定度 | 連續配息年數/配息成長率 | GA | 否 | 是 | 2 | 4 |
| 26 | 個股 Beta 值計算 | 相對大盤系統性風險 | GA | 否 | 是 | 2 | 4 |
| 27 | 同業排名雷達 | 個股在同產業各指標的百分位 | GA | 否 | 是 | 3 | 4 |
| 28 | 股價季節性分析 | 歷史同月份漲跌統計 | GA | 否 | 是 | 2 | 3 |
| 29 | 除權息填權速度追蹤 | 歷年填權天數統計 | GA | 否 | 是 | 2 | 4 |
| 30 | 個股風險警示燈 | 違約/財報異常/處置股狀態聚合 | GA | 否 | 是 | 2 | 5 |

### Z4. ETF 專區
| # | 功能名稱 | 用途 | 適合 | Key | Fork | 難度 | 推薦 |
|---|---------|------|------|-----|------|------|------|
| 31 | ETF 折溢價追蹤 | 淨值 vs 市價偏離度 | GA | 否 | 是 | 3 | 5 |
| 32 | ETF 資金流向 | 受益人數/規模變化趨勢 | GA | 否 | 是 | 2 | 4 |
| 33 | 主動式 ETF 選股透明度評分 | 依公開持股揭露頻率評分 | GA | 否 | 是 | 3 | 4 |
| 34 | ETF 重疊度分析 | 兩檔 ETF 成分股重複比例 | GA | 否 | 是 | 3 | 4 |
| 35 | 高股息 ETF 除息月曆 | 集中呈現各檔配息月份 | GA | 否 | 是 | 2 | 5 |
| 36 | ETF 費用長期侵蝕試算 | 管理費對長期報酬的複利影響 | GA | 否 | 是 | 2 | 4 |
| 37 | 槓桿/反向 ETF 風險提示 | 波動耗損（Volatility Decay）教育卡 | GA | 否 | 是 | 2 | 4 |
| 38 | ETF 新掛牌雷達 | 新上市 ETF 自動收錄提示 | GA | 否 | 是 | 2 | 3 |
| 39 | ETF 成分股調整歷史 | 定期調整前後差異對照 | GA | 否 | 是 | 3 | 3 |
| 40 | 全球 ETF 分類地圖 | 依資產類別/地區樹狀導覽 | GA | 否 | 是 | 3 | 3 |

### Z5. Portfolio & 風險
| # | 功能名稱 | 用途 | 適合 | Key | Fork | 難度 | 推薦 |
|---|---------|------|------|-----|------|------|------|
| 41 | 蒙地卡羅退休模擬 | 依歷史報酬分布模擬提領成功率 | GA | 否 | 是 | 4 | 4 |
| 42 | 資產相關性矩陣 | 持倉間相關係數，評估分散度 | GA | 否 | 是 | 3 | 4 |
| 43 | 稅務試算（證交稅/股利所得） | 台灣稅制試算器（非稅務建議） | GA | 否 | 是 | 3 | 4 |
| 44 | 目標導向配置 | 依目標報酬/風險反推配置建議 | GA | 否 | 是 | 3 | 3 |
| 45 | 壓力測試情境庫 | 套用歷史崩盤情境試算損失 | GA | 否 | 是 | 3 | 5 |
| 46 | 持倉集中度警示 | 單一標的/產業占比過高提醒 | GA | 否 | 是 | 1 | 5 |
| 47 | 定期定額試算器 | 歷史回測定期定額成效 | GA | 否 | 是 | 2 | 5 |
| 48 | 已實現/未實現損益分離 | 稅務與績效視角分開呈現 | GA | 否 | 是 | 2 | 4 |
| 49 | 多幣別持倉統一換算 | 美股/台股統一以 TWD 檢視 | GA | 否 | 是 | 2 | 4 |
| 50 | 投組 vs 大盤超額報酬 | Alpha/Beta 分解 | GA | 否 | 是 | 3 | 4 |

### Z6. Backtest & 策略
| # | 功能名稱 | 用途 | 適合 | Key | Fork | 難度 | 推薦 |
|---|---------|------|------|-----|------|------|------|
| 51 | 交易成本敏感度分析 | 手續費/滑價假設對績效影響 | GA | 否 | 是 | 2 | 4 |
| 52 | 策略組合（多策略疊加） | 多策略資金分配回測 | GA | 否 | 是 | 4 | 4 |
| 53 | Walk-Forward 分析 | 滾動窗口樣本外驗證，防止過擬合 | GA | 否 | 是 | 4 | 5 |
| 54 | 蒙地卡羅打亂測試 | 交易順序隨機打亂測穩健性 | GA | 否 | 是 | 3 | 4 |
| 55 | 策略衰退偵測 | 近期績效偏離歷史統計的警示 | GA | 否 | 是 | 3 | 4 |
| 56 | 最佳/最差年份分解 | 逐年報酬拆解找出貢獻來源 | GA | 否 | 是 | 2 | 4 |
| 57 | 策略互斥檢查 | 兩策略持股高度重疊時提示 | GA | 否 | 是 | 2 | 3 |
| 58 | 回測結果版本快照 | 策略改版時凍結舊結果（對應 Y 節建議） | GA | 否 | 是 | 3 | 5 |
| 59 | 交易信號日誌 | 逐日進出場理由可追溯 | GA | 否 | 是 | 2 | 4 |
| 60 | 策略 GitHub Action 一鍵 Fork 套用 | 提供 workflow template 讓使用者插入自訂策略 | GitHub Native | 否 | 是 | 3 | 4 |

### Z7. 因子 & 量化研究
| # | 功能名稱 | 用途 | 適合 | Key | Fork | 難度 | 推薦 |
|---|---------|------|------|-----|------|------|------|
| 61 | 因子擁擠度指標 | 該因子近期是否過度追捧（估值分散度） | GA | 否 | 是 | 4 | 3 |
| 62 | 自訂因子公式編輯器 | 使用者用 YAML/DSL 定義新因子 | GA+React | 否 | 是 | 4 | 4 |
| 63 | 因子中性化處理 | 剔除產業/市值偏誤後的純因子分數 | GA | 否 | 是 | 4 | 4 |
| 64 | 因子輪動歷史 | 各因子近 N 年報酬貢獻週期 | GA | 否 | 是 | 3 | 3 |
| 65 | 多因子組合權重優化 | 簡易均值變異數優化（非黑箱 AI） | GA | 否 | 是 | 4 | 4 |
| 66 | 因子與總經指標關聯 | 因子表現 vs 利率/通膨環境 | GA | 否 | 是 | 3 | 3 |
| 67 | 因子分數分布直方圖 | 全市場某因子分數常態性檢視 | GA | 否 | 是 | 2 | 3 |
| 68 | 極端值自動偵測 | 因子計算中的異常值標記與排除規則 | GA | 否 | 是 | 2 | 5 |
| 69 | 因子回測 vs 實際落差追蹤 | 理論因子報酬 vs 若真實下單的落差 | GA | 否 | 是 | 4 | 3 |
| 70 | 開源因子函式庫頁 | 展示所有因子計算公式（教育＋透明） | GA | 否 | 是 | 2 | 5 |

### Z8. 新聞 / 情緒 / 日曆
| # | 功能名稱 | 用途 | 適合 | Key | Fork | 難度 | 推薦 |
|---|---------|------|------|-----|------|------|------|
| 71 | 規則式情緒燈號 | 關鍵字庫（利多/利空詞）非 AI 版情緒分析 | GA | 否 | 是 | 2 | 5 |
| 72 | AI 新聞摘要* | LLM 摘要長篇公告（Optional） | GA/Opt | 是（LLM） | 降級可用 | 2 | 4 |
| 73 | 重大訊息公告聚合 | 公開資訊觀測站重訊分類 | GA | 否 | 是 | 3 | 5 |
| 74 | 法說會逐字稿摘要* | 公開逐字稿規則摘要，AI 加值 | GA/Opt | 視模式 | 降級可用 | 3 | 3 |
| 75 | 新聞來源可信度標記 | 依來源歷史準確度做提示（非審查） | GA | 否 | 是 | 3 | 3 |
| 76 | 財經日曆訂閱（.ics） | 匯出行事曆檔供 Google/Outlook 訂閱 | GA | 否 | 是 | 2 | 5 |
| 77 | 除權息倒數提醒卡 | 個股頁顯示距除權息剩餘天數 | GA | 否 | 是 | 1 | 5 |
| 78 | 新聞與股價聯動標記 | 股價異常波動日自動關聯當日新聞 | GA | 否 | 是 | 3 | 4 |
| 79 | 每週重點新聞回顧 | 週報形式彙整（同 T Report Center） | GA | 否 | 是 | 2 | 4 |
| 80 | 監理機關公告追蹤 | 金管會/證交所規則變動摘要 | GA | 否 | 是 | 2 | 3 |

### Z9. GitHub-Native 社群功能
| # | 功能名稱 | 用途 | 適合 | Key | Fork | 難度 | 推薦 |
|---|---------|------|------|-----|------|------|------|
| 81 | Issue Template：策略提案 | 標準化策略投稿格式 | GitHub Native | 否 | 是 | 1 | 5 |
| 82 | Discussions：每日盤後討論串 | Actions 自動開串 | GA+GitHub Native | 否 | 是 | 2 | 4 |
| 83 | Projects 看板：Roadmap 公開透明 | 對應本文 X 節 | GitHub Native | 否 | 是 | 1 | 5 |
| 84 | PR 自動化因子/策略驗證 | CI 跑新策略的基本合規檢查（是否直打 API 等紅線） | GA | 否 | 是 | 4 | 5 |
| 85 | Contributor 排行榜 | 依 PR/Issue 貢獻度排名（GitHub API） | GA | 否 | 是 | 2 | 3 |
| 86 | Fork 網路圖 | 視覺化多少人 Fork 並仍在跑排程（需自願回報） | GitHub Native | 否 | 是 | 3 | 2 |
| 87 | Release Note 自動生成 | 依 PR 標籤自動彙整（Changesets 模式） | GA | 否 | 是 | 2 | 4 |
| 88 | Wiki 知識庫雙向同步 | Obsidian Vault 內容選擇性同步至 Wiki | GA | 否 | 是 | 3 | 3 |
| 89 | Sponsors 資金流向透明頁 | 若接受贊助，公開資金用途（純資訊頁） | GA | 否 | 是 | 1 | 2 |
| 90 | 社群模板市集 | 使用者分享 Dashboard 排版/Screener 模板（PR 提交） | GitHub Native | 否 | 是 | 3 | 3 |

### Z10. 教育 / 遊戲化 / 創意雜項
| # | 功能名稱 | 用途 | 適合 | Key | Fork | 難度 | 推薦 |
|---|---------|------|------|-----|------|------|------|
| 91 | 模擬交易競賽（紙上） | 用歷史資料跑虛擬選股比賽，結果進 Actions 產出 | GA | 否 | 是 | 4 | 3 |
| 92 | 投資名言/行為財務學小卡 | 每日隨機顯示一則行為偏誤提醒 | GA | 否 | 是 | 1 | 3 |
| 93 | 新手模擬盤（無真金） | 純前端模擬下單練習介面 | React | 否 | 是 | 3 | 4 |
| 94 | 財報閱讀教學互動頁 | 逐步拆解一份真實財報（教育用途） | React | 否 | 是 | 3 | 4 |
| 95 | 「如果當初買了」歷史試算器 | 輸入日期/金額試算至今報酬 | GA | 否 | 是 | 2 | 5 |
| 96 | 股市冷知識產生器 | 從歷史資料自動挖掘有趣統計（如「本月最像去年幾月」） | GA | 否 | 是 | 3 | 3 |
| 97 | 個人化年度投資回顧 | 仿 Spotify Wrapped，年終產出個人交易總結圖卡 | GA | 否 | 是 | 3 | 5 |
| 98 | 錯誤決策回顧日誌 | 使用者自填交易日誌，事後統計常見誤區（存自己 repo） | React | 否 | 是 | 2 | 4 |
| 99 | 開源致敬牆 | 列出所有使用的公開資料源/開源套件與授權 | GA | 否 | 是 | 1 | 4 |
| 100 | 專案自我體檢報告 | Actions 定期自動檢查本文 P1-P8 紅線是否被違反（如意外引入付費依賴），結果發 Issue | GA | 否 | 是 | 3 | 5 |

---

## 9. 最終分級：必做 / 建議做 / 可選做 / 創新功能

> 綜合第 4-8 節，依「對 Fork First 精神的貢獻度」與「開發成本」排序。

### 必做（Phase 1-2 範圍，缺了就不成平台）
- A Dashboard 基本版、B Market、C Stock Analysis、D ETF Center、Q Heatmap、R Screener（基本）
- F Backtest、G Strategy Center、H Factor Center、P Ranking
- Z 精選：#8 部署狀態徽章、#9 資料新鮮度提示條、#22 財報成長趨勢燈號、#30 個股風險警示燈、#46 持倉集中度警示、#47 定期定額試算器、#58 回測結果版本快照、#71 規則式情緒燈號、#76/#77 財經日曆與除權息提醒、#81/#83 Issue Template 與 Projects 看板、#100 P1-P8 自我體檢報告

### 建議做（Phase 3 範圍，顯著提升深度但非上線必要）
- E Portfolio（含再平衡）、I News（情緒分析）、J Financial Calendar、S Compare
- Z 精選：#14 台版恐慌貪婪指數、#31 ETF 折溢價追蹤、#35 高股息 ETF 除息月曆、#45 壓力測試情境庫、#53 Walk-Forward 分析、#62 自訂因子公式編輯器、#70 開源因子函式庫頁、#95 「如果當初買了」試算器、#97 年度投資回顧

### 可選做（Phase 4，長尾價值，視社群貢獻彈性排程）
- K Market Replay、L/M/N 百科全書系列、T Report Center、U Knowledge Base
- Z 精選：#33 主動式 ETF 透明度評分、#61 因子擁擠度指標、#82/#85/#88/#90 社群類功能

### 創新功能（實驗性，先小規模驗證再決定是否納入正式 Roadmap）
- 6.13-15 Optional 後端模組（Chat/Task/Settings）
- Z 精選：#19 產業指數輪動時鐘、#52 策略組合疊加、#65 多因子組合權重優化、#91 模擬交易競賽、#93 新手模擬盤、#96 股市冷知識產生器

---

## 相關資源
- [[quant-dashboard-skill|Skill 藍圖（含 P1-P8 自檢清單）]]
- [[finance/quant-dashboard-prompt|實作紀錄與提示詞]]
- [[finance/quant-dashboard-qa-1|架構 Q&A（第一批）]]
- [[finance/quant-dashboard-qa-2|多角色架構審查]]
- [[finance/quant-dashboard-resource|資源清單]]
