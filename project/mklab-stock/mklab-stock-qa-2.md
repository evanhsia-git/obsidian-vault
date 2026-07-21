---
title: "mklab-stock 架構審查（多角色）"
description: "以 Senior Architect / React Lead / Python Backend / DevOps / Quant 五角色視角，審查 mklab-stock 22 題決策 + 補漏 Q23~Q36 + ADR + Roadmap"
summary: "mklab-stock 架構多角色審查：Q0~Q22 每題 A/B/C/D 優缺+Recommendation、Q23~Q36 遺漏（state/TanStack/error/cache/monitor/migration/dark/a11y/seo/export/virtual/worker/security/api-version）、ADR-001~005、Phase0~7 Roadmap"
type: project
status: active
tags:
  - etf
  - finance
created: 2026-07-13
updated: 2026-07-13
---

# mklab-stock 架構審查（多角色）

> 狀態：**審查紀錄，待用戶採納**。本頁為多角色架構審查，非最終決策。
> 角色視角：Senior Software Architect / React Tech Lead / Python Backend Architect / DevOps Architect / Quant Trading System Architect。
> 對應：[[mklab-stock-v2-100個功能|架構主文]]、[[mklab-stock-qa-1|Q&A 第一批（23 題選項）]]、[[mklab-stock-prompt|實作紀錄]]。
> 每題給 A/B/C 分析 + 第四方案 D（若有）+ Recommendation（含 GitHub Pages / Hermes / React / 長期維護 評分）。

---

## 審查範圍與原則

評估維度：擴充性 / 可維護性 / 可測試性 / 效能 / 開發成本 / AI 整合 / 五年可維護。
同時對照：GitHub Pages 架構、Hermes Agent 架構、React 最佳實務、Clean Architecture、SOLID、Repository Pattern。

---

# 第一部分：Q0~Q11

## Q0. 導航選單（6.0）與 15 模組對不上

**問題**：6.0 有 Calendar/Report 但規格無；規格有多因子/Compare 但導航無；Hermes 合成一項。

- **A 導航補齊成 17 項**：優點功能全曝光；缺點擁擠、加模組改兩處；適合功能極多。
- **B 以 15 模組為準修正 15 項**：優點單一真相源；缺點需重構；✅ 適合我們。
- **C 維持 14 項收子頁**：優點簡潔；缺點隱藏功能。
- **D（⭐更優）config-driven 導航**：nav-config.json 驅動，模組增減只改 config 不動程式，符合 Open-Closed。

**Recommendation：B + D**。先 B 修正，長期走 D。
| GitHub Pages | Hermes | React | 長期 |
|---|---|---|---|
| ★★★★★ | ★★★★★ | ★★★★★ | ★★★★★ |

---

## Q1. 08:30 美股資料跨 job 保存

- **A Actions artifact 暫存**：優點跨 job 保留不進 repo；缺點有期限、需 job 依賴鏈；✅ 適合雙排程。
- **B commit 到 data/ 分支**：違反一次 push。
- **C 08:30 也 push main**：違反決策。
- **D（⭐更優）單一 workflow + `needs` 依賴**：job1 抓美股 upload-artifact → job2 抓台股 download+合併+push，無分支污染。

**Recommendation：A 或 D**（D 更乾淨；若需獨立手動重跑則 A 彈性）。

---

## Q2. 假日/休市邏輯

- **A 內建交易日曆（TW+US）**：優點專業；缺點需維護曆（用 `exchange_calendars`）。
- **B 照常跑留空**：用戶困惑。
- **C 非交易日跳過**：站停更。
- **D（⭐更優）`data/status.json` 含 `last_trading_day` + `is_stale`**：前端讀 status 決定顯示，是 Data Contract 延伸。

**Recommendation：A + D**。

---

## Q3. 備援源切換觸發

- **A 自動 verify-gate 失敗換源**：優點零人工；✅ 沿用 quant-trading。
- **B repo variables 手動切**：源掛時沒人切則停更。
- **C 主源掛整次 skip**：最糟。
- **D（⭐更優）Circuit Breaker + 健康度評分**：每源 health_score，自動選最高分；失敗計入 breaker。

**Recommendation：A 起步，D 進化**。

---

## Q4. FinMind/OpenBB token 放哪

- **A repo secrets（Actions）+ 本地 .env（Hermes）**：✅ 標準分流。
- **B 全 repo secrets**：本地難用。
- **C 明文 YAML**：🔴 安全災難。
- **D（⭐更優）Secret Manager 抽象層**：`python-dotenv` + `os.getenv`，運行環境決定來源，符合 Dependency Inversion。

**Recommendation：A + D**。

---

## Q5. 靜態站有無真 AI 內容

- **A Actions 跑 LLM 烤進 JSON**：優點靜態站有真 AI 文；缺點成本/幻覺。
- **B 只放量化分數**：零成本但站不「AI」。
- **C 預生成文 + Admin 重生成**：折衷複雜。
- **D（⭐更優）Tiered AI**：靜態烤「量化驅動 Score + 規則模板摘要」（零成本）；真 LLM 深度分析走 Phase 6 Admin。

**Recommendation：A（預算夠）或 D（平衡）**。匿名測試站不必每天燒 LLM。

---

## Q6. 新聞(6.9) 資料源

- **A FinMind 新聞 API + LLM**：結構化；✅。
- **B Yahoo RSS 爬**：免費但爬蟲維護。
- **C 暫不做**：避難。
- **D（⭐更優）多源聚合 + 去重 + LLM 摘要**：FinMind + Yahoo/TWSE 公告 → 去重 → sentiment+summary。

**Recommendation：C 起步（Phase 1 不做），A/D 在 Phase 4**。

---

## Q7. LLM 成本天花板

- **A Gemini Flash / GPT-4o-mini + 月預算上限**：✅ 便宜可控。
- **B 本地 Ollama**：零成本但 Actions 跑不了、品質低。
- **C GPT-4o**：貴。
- **D（⭐更優）Model Router + 快取**：簡單任務 Flash、複雜 4o-mini；同輸入緩存不重算。

**Recommendation：A + D**。

---

## Q8. 圖表庫

- **A ECharts（echarts-for-react）**：✅ 金融 K線/雷達/熱力最強。
- **B Plotly.js**：重。
- **C Chart.js**：金融圖弱。
- **D（⭐）ChartKit 封裝**：常用圖封裝統一調用（DRY）。

**Recommendation：A**（echarts-for-react 已是好抽象，不必另封）。

---

## Q9. 路由結構

- **A React Router + 左側 sidebar**：✅ Bloomberg 風、URL 可分享。
- **B Tab 切換**：難深鏈。
- **C Modal/Overlay**：不適多模組。
- **D（⭐更優）Nested Routes + Layout 共用 + Code Splitting**：`/dashboard/*` 共用 layout，子路由懶加載。

**Recommendation：A + D**。

---

## Q10. 語系/地區

- **A zh-TW**：✅ 符合客群。
- **B 英文**：非客群。
- **C 雙語 i18n**：一倍工作量。
- **D（⭐更優）i18n 基建預留只填 zh-TW**：`react-i18next` 架構，未來加 en 零成本。

**Recommendation：A + D 基建**。

---

## Q11. 行動版

- **A responsive 單欄堆疊**：✅ 標準。
- **B 手機專用簡版**：維護兩套。
- **C 不支援手機**：你用 iOS 不可行。
- **D（⭐更優）Container Queries + 抽屜式**：三欄桌機、平版兩欄、手機 bottom-sheet。

**Recommendation：A 起步，D 優化**。

---

# 第二部分：Q12~Q22

## Q12. 前端測試

- **A Vitest + Playwright 關鍵頁**：✅ 平衡。
- **B 只手動**：回歸地獄。
- **C 全自動 CI**：重。
- **D（⭐更優）Vitest + Testing Library + Playwright Smoke 3 路徑 + Data Contract 快照測試**：防前後端漂移。

**Recommendation：A + D 的 contract 測試**。
| React | 長期 | 可測試 |
|---|---|---|
| ★★★★★ | ★★★★★ | ★★★★★ |

---

## Q13. ETF 管理費/追蹤誤差/持股來源

- **A TWSE 基金資訊 + FinMind**：✅ 權威（上櫃 ETF 在 TPEX 另源）。
- **B 只做有資料欄位留 null**：殘缺。
- **C 手動 CSV**：累。
- **D（⭐更優）ETF 元數據獨立 Pipeline**：`etf_metadata.py` 產 `etf_meta.json`，與行情 `etf.json` 分離（頻率不同：行情日變、元數據季變 → Single Responsibility）。

**Recommendation：A + D**。

---

## Q14. 投資組合 IRR/XIRR 輸入

- **A CSV 加買入日期欄真算 XIRR**：✅ 正確。
- **B 只算成本 vs 現值留 null**：假指標。
- **C 不顯示 IRR**：避難。
- **D（⭐更優）交易明細表 lots.csv**：`date,symbol,action,qty,price,fee`；`calc_portfolio(lots)→Portfolio` 純函數、可單測、支援加減碼。

**Recommendation：D > A**（lots 模型是量化標準）。

---

## Q15. 多因子 6 因子對齊

- **A 重寫評分模型對齊 6 因子**：✅ 符合規格。
- **B 沿用三期映射**：不純。
- **C 維持三期**：偏離。
- **D（⭐更優）因子引擎插件化**：`factors/` 每因子一模組統一 `compute(universe)→Series`；新增因子=加檔案（Open-Closed）。

**Recommendation：A + D**。

---

## Q16. Heatmap 產業分類

- **A DB 現有 industry 複用缺補 fetch_industry**：✅。
- **B FinMind 抓**：重複。
- **C 暫不支援**：降功能。
- **D（⭐更優）industry 納入 Contract 必填**：源頭保證，Heatmap 只是讀 Contract 切 view（前端零邏輯）。

**Recommendation：A + D**。

---

## Q17. 美股資料範圍

- **A 三大指數 + 台股 ADR 前 50**：✅ 輕量。
- **B 全 S&P500**：重。
- **C 只指數**：無個股頁。
- **D（⭐更優）分層：指數每日必抓 + watchlist.json 用戶定義**：比硬編彈性。

**Recommendation：A 起步，D 進化**。

---

## Q18. Data Contract 同步保證

- **A CI 檢查 Pydantic vs TS 欄位**：✅ 自動防漂移。
- **B codegen（Pydantic→TS）**：徹底但綁工具鏈。
- **C 手動紀律**：必漏。
- **D（⭐更優）contract.yaml 為 Single Source**：Pydantic 與 TS 都從 yaml codegen（比 B 更徹底）。折衷：A 足矣。

**Recommendation：A**（codegen 等團隊擴大再上）。

---

## Q19. Contract 版本化

- **A 加 schema_version 欄**：✅ 舊前端可降級提示。
- **B 不版本化**：脆。
- **C git tag 當版本**：前端不知對應哪版。
- **D（⭐更優）semver + `/api/version` + 前端啟動比對**：靜態站讀 `data/meta.json` 的 `contract_version` 與打包版本比對，不符顯示「需重新部署」不崩。

**Recommendation：A + D**。

---

## Q20. 雙示警 Email 發送

- **A SMTP via repo secret**：✅ 真 Email（SendGrid 免費 100/天）。
- **B GitHub Issues 當通知**：非真郵件。
- **C 只 Telegram**：單通道（違雙通道要求）。
- **D（⭐更優）Alert 抽象層**：`alert(msg,level)` 同推 TG+Email；未來加 Slack/Discord 只加 adapter。

**Recommendation：A 或 D**。

---

## Q21. Pages 部署模式

- **A gh-pages 分支**：✅ 標準。
- **B docs/ 資料夾**：main 塞滿。
- **C 外部 CDN**：脫離 Pages。
- **D（⭐更優）Actions artifact-based deploy**：`upload-pages-artifact` + `deploy-pages`，構建物存 artifact 不進任何分支（連分支都不要）。

**Recommendation：D > A**（現代最佳實踐）。

---

## Q22. Secrets vs Settings 邊界

- **A Actions repo secrets / 本地 .env / Settings 只改本地**：✅ 清楚分隔。
- **B 全 repo secrets**：本地難用。
- **C 全本地 .env**：Actions 拿不到。
- **D（⭐更優）環境抽象 + Settings 寫 `.hermes/secrets.yaml` 經 SecretProvider**：Settings 頁只是「觸發 Hermes 寫本地」UI，**key 永不在瀏覽器**（安全紅線）。

**Recommendation：A + D**。

---

# 第三部分：遺漏架構問題（Q23~Q36）

## Q23. State Management
- **Recommendation：Zustand**（輕、非樣板）。全局只放 `theme`/`dataSourceMode`/`watchlist`；資料經 TanStack Query 管理（Server State 不進 Zustand）。UI State 與 Server State 分離。

## Q24. TanStack Query
- **Recommendation：必用**。`useQuery(['market'], () => dataClient.getMarket())` 自帶 cache/invalidate/loading。雙源切換只改 `dataClient` 實例，hook 不變。

## Q25. Error Handling
- **Recommendation：Error Boundary + `Result<T>` 模式**。DataClient 回 `Result<Contract>`，UI 統一渲染 fallback（「資料暫時無法載入」而非白屏）。

## Q26. Caching / Rate Limit
- **Recommendation**：Python 端 `tenacity` 重試 + 指數退避；前端 TanStack Query `staleTime: 1h`（日更資料一小時內不重抓）。

## Q27. Monitoring / Logging
- **Recommendation**：Actions 跑完 push `logs/run-YYYYMMDD.json`（成功/失敗/用時/筆數）；前端「關於」頁顯示最後成功運行。

## Q28. Database Migration
- **Recommendation**：`schema_version` 表 + `migrations/` 目錄，啟動時 `PRAGMA user_version` 比對自動 migrate。輕量自寫（不用 Alembic）。

## Q29. Dark Mode
- **Recommendation**：shadcn 原生 `class` 策略（`.dark`），預設深色（金融族群偏好）。

## Q30. Accessibility（a11y）
- **Recommendation**：TanStack Table 天然 a11y；圖表加 `aria-label` + 資料表備援（圖表旁附同等資料 table）。

## Q31. SEO / Search
- **Recommendation**：`react-helmet` 管理 meta；`vite-plugin-ssg` 預渲染首頁/個股給 SEO。

## Q32. CSV/PDF Export
- **Recommendation**：`papaparse`（CSV）+ `jspdf`（PDF）客戶端產，不經後端。

## Q33. Code Splitting / Virtual Table
- **Recommendation**：路由懶加載（Q9-D）+ TanStack Table `virtualized`（全市場 ~1700 檔只渲染可視列）。

## Q34. Web Worker
- **Recommendation**：重計算（Screener 即時篩、Portfolio IRR）丟 Web Worker，避免主線程卡頓。

## Q35. Security（靜態站特有）
- **Recommendation**：Admin（Phase 6）必加 Auth（VPS 前 reverse proxy + JWT），Chat/Task 端點不對外公開。

## Q36. API Versioning
- **Recommendation**：URL versioning（`/api/v1`）+ Contract 內 `schema_version`（Q19）雙軌；舊版保留 6 個月。

---

# 第四部分：Architecture Decision Record (ADR)

## ADR-001: 前端框架與解耦架構
- **Decision**：React+TS+Vite+Tailwind+shadcn/ui；前端經 DataClient 讀 Contract，不直接碰 SQLite/Python。
- **Reason**：長期可維護、雙源可切、團隊擴充容易。
- **Pros**：解耦、可測、擴充性強。
- **Cons**：初期比純 Python 儀表板慢。
- **Future**：未來換 DB/API 零前端改動。

## ADR-002: 資料契約為單一真相
- **Decision**：Stock/ETF/Portfolio/News/Strategy/Backtest/MarketSummary 定為 TS Contract + Pydantic Schema 1:1，加 `schema_version`。
- **Reason**：防漂移、未來換源不動 UI。
- **Pros**：契約穩定、CI 可驗。
- **Cons**：多一層維護。

## ADR-003: 雙源（靜態 JSON / REST API）可切
- **Decision**：展示模組雙源；Admin（Chat/Task/Settings）獨立 Phase 6 部署 Hermes VPS。
- **Reason**：靜態 Pages 不能做動態；分離清晰。
- **Pros**：公開站零成本、Admin 按需擴。
- **Cons**：兩套部署。

## ADR-004: 部署用 Actions Artifact-based Pages
- **Decision**：`upload-pages-artifact` + `deploy-pages`，不進 gh-pages 分支也不進 docs/。
- **Reason**：repo 乾淨、現代最佳實踐。
- **Pros**：無分支污染。
- **Cons**：構建物不可 git 追溯（可接受）。

## ADR-005: 狀態與資料分離
- **Decision**：Zustand（UI state）+ TanStack Query（server state）；重算丟 Web Worker。
- **Reason**：React 最佳實務、效能。
- **Pros**：可維護、流暢。
- **Cons**：學習曲線。

---

# 第五部分：Architecture Roadmap

## Phase 0: 基礎設施與契約（不部署）
- **目標**：定 Contract、建 repo、CI 骨架
- **完成**：`contracts.ts` + `schemas.py` 1:1；`ci.yml` 跑 lint+test
- **技術**：TS / Python / pytest / Vitest
- **依賴**：無
- **可部署**：否
- **預估**：3-5 天
- **風險**：低

## Phase 1: MVP 靜態展示（市場+ETF+首頁）
- **目標**：Pages 看台股大盤/ETF
- **完成**：`market.json`+`etf.json` 經 Actions 產；首頁三欄；深色模式
- **技術**：React Router + ECharts + Tailwind
- **依賴**：Phase 0
- **可部署**：✅
- **預估**：1-2 週
- **風險**：中（TPEX ETF 完整性）

## Phase 2: 計算型模組（多因子+選股+Heatmap）
- **目標**：Screener/多因子/Heatmap 互動
- **完成**：`factor_scores.json`/`screener.json`/`heatmap.json`；前端篩選+雷達+熱力；Web Worker
- **技術**：因子引擎插件化 / TanStack Table 虛擬化
- **依賴**：Phase 1
- **可部署**：✅
- **預估**：2 週
- **風險**：中（因子對齊 6 因子）

## Phase 3: 回測+策略中心
- **目標**：6.7/6.8 可跑
- **完成**：`backtest.json`；Equity Curve + 12 策略頁
- **技術**：backtest.py 改產 JSON；ECharts 曲線
- **依賴**：Phase 2
- **可部署**：✅
- **預估**：2 週
- **風險**：中（回測正確性）

## Phase 4: AI 層（分析+新聞，烤 JSON）
- **目標**：6.2/6.9 靜態可看
- **完成**：`ai_analysis.json`/`news.json` 含 LLM 摘要；Actions 跑 LLM（Router+快取）
- **技術**：LLM Router / FinMind 新聞 / Model Router
- **依賴**：Phase 1-3
- **可部署**：✅（AI 文烤進）
- **預估**：2-3 週
- **風險**：高（成本+幻覺把關）

## Phase 5: 投資組合+比較+報表
- **目標**：6.5/6.11/報表
- **完成**：`lots.csv` 驅動 Portfolio（IRR/XIRR）；Compare 雷達；CSV/PDF 匯出
- **技術**：純函數計算 / papaparse / jspdf
- **依賴**：Phase 2
- **可部署**：✅
- **預估**：1-2 週
- **風險**：低

## Phase 6: Admin 模組（Hermes VPS）
- **目標**：Chat/Task/Settings
- **完成**：FastAPI `/api/v1/*`+`/admin/*`；Auth 層；Settings 寫本地 secrets（不進瀏覽器）
- **技術**：FastAPI / JWT / Reverse Proxy
- **依賴**：Phase 1-5（前端 DataClient 切 api mode）
- **可部署**：✅（VPS 非 Pages）
- **預估**：3-4 週
- **風險**：高（安全/Auth）

## Phase 7: Release + 監控
- **目標**：正式公開
- **完成**：雙示警上線；Monitoring 頁；SEO 預渲染；a11y 通過
- **技術**：Alert 抽象層 / SSG / react-helmet
- **依賴**：Phase 1-6
- **可部署**：✅ 正式
- **預估**：1 週
- **風險**：低

---

## Blueprint 總結

**關鍵決策**：Contract-first、雙源解耦、Artifact deploy、狀態/資料分離、Admin 獨立。
**最大風險**：Phase 4（LLM 成本/幻覺）與 Phase 6（Auth 安全）。
**待用戶採納**：Q0~Q22 的 Recommendation（多推 ⭐/D 混合）是否全採；採納後整理「最終決策表」回寫架構主文。

## 相關節點
- [[mklab-stock-v2-100個功能|mklab-stock 專案架構]]
- [[mklab-stock-qa-1|mklab-stock 架構 Q&A（第一批）]]
- [[mklab-stock-prompt|mklab-stock 實作紀錄與提示詞]]
- [[mklab-stock-resource|mklab-stock 資源清單]]
