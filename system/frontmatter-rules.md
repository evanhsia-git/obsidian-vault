---
title: "FRONTMATTER-RULES"
description: "Frontmatter 規範（schema.md 的唯一細節來源）"
summary: "Obsidian Frontmatter 規範 — Hermes Agent 執行版"
type: schema
status: permanent
tags: [knowledge, workflow, config]
created: 2026-06-21
updated: 2026-07-10
---

> 本檔為 `schema.md` 中 Metadata / Type / Status / Frontmatter Safety 的**唯一細節來源（Single Source of Truth）**。schema.md 僅保留原則與索引，具體清單與格式規則以本檔為準。兩者衝突時，數值以本檔為準，原則以 schema.md 為準。

# Standard Template
```yaml
---
title:
description:
summary:
type:
status:
tags:
created:
updated:
sources: []
related: []
---
```

---
# Field Rules 欄位規則

## type

僅允許：

```text
entity | concept | project | resource | report | query | task | index | log | schema
```

禁止新增。

```text
entity      實體 / 實際的人、事、物描述，例如賈伯斯、蘋果公司、IPHONE手機
concept     概念 / 抽象的想法、理論、概念、名詞解釋、公式
project     專案 / 一個有明確目標、由多個任務組成的大型計畫
resource    資源 / 參考資源別人寫的文章、網頁、書籍摘要、法律條文或代碼範例。它是你拿來參考的「別人的東西」
report      報告 / 自己整合、分析後的階段性正式輸出成果
query       查詢 / 動態查詢或看板通常裡面沒有自己寫的內容，只有一堆 Dataview 語法，用來自動抓出符合條件的筆記
task        任務 / 某個專案底下的具體執行步驟，通常是一件做完就可以勾掉的事
index       首頁 / 索引/MOC/目錄，手動整理的骨幹目錄（Map of Content），把同主題的筆記手動串在一起
log         日誌 / 發生過什麼事，帶有時間戳記的日常紀錄，如每日、每週、每月筆記
schema      規範 / 筆記結構規範、規則是什麼或是範本Template樣式
```

---
## tags

規則：
- 全 Vault 限 **48 個核心 tag**（禁止新增，與 schema.md 一致）
- 單頁最多 10 個
- 每 tag 標註 4 項：**Tag、類別、用途、關鍵詞/涵蓋內容**

### AI（10）

| Tag | 類別 | 用途 | 關鍵詞 / 涵蓋內容 |
| --- | --- | --- | --- |
| `ai` | AI | 人工智慧 | LLM、Generative AI、AI 應用 |
| `agent` | AI | AI Agent | Multi-Agent、Agent Framework、Tool Calling |
| `prompt` | AI | Prompt Engineering | System Prompt、Few-shot、Prompt Pattern |
| `model` | AI | AI 模型 | Claude、GPT、Gemini、Qwen、DeepSeek |
| `memory` | AI | Agent 記憶 | Session Memory、Long-term Memory、Context |
| `workflow` | AI | 工作流程 | Pipeline、Workflow、Flow、Automation Flow |
| `hermes` | AI | Hermes Agent | SOUL、Skills、Hermes Workflow |
| `mcp` | AI | MCP 生態 | Model Context Protocol、MCP Server、Tool Calling |
| `rag` | AI | RAG | Retrieval、Embedding、Knowledge Base |
| `vector` | AI | 向量搜尋 | Embedding、Vector Database、Semantic Search |

### PKM（6）

| Tag | 類別 | 用途 | 關鍵詞 / 涵蓋內容 |
| --- | --- | --- | --- |
| `pkm` | PKM | 知識管理 | Second Brain、Zettelkasten、PARA、Wiki |
| `obsidian` | PKM | Obsidian | Vault、Markdown、Plugin、WebDAV |
| `quartz` | PKM | Quartz | Publish、Static Site、Deploy |
| `schema` | PKM | 資料規範 | Metadata、Schema、Frontmatter、Vault Rules |
| `plugin` | PKM | 外掛 | Community Plugin、Extension |
| `template` | PKM | 範本 | Template、Snippet、Boilerplate |

### 投資（2）

| Tag | 類別 | 用途 | 關鍵詞 / 涵蓋內容 |
| --- | --- | --- | --- |
| `stock` | 投資 | 股票 | 台股、美股、ETF、個股 |
| `finance` | 投資 | 財務分析及宏觀經濟 | 財報、PE、PB、EPS、殖利率、估值、CPI、GDP、Fed、央行、利率、匯率 |

### 開發（9）

| Tag | 類別 | 用途 | 關鍵詞 / 涵蓋內容 |
| --- | --- | --- | --- |
| `python` | 開發 | Python | Script、venv、Library、Automation |
| `api` | 開發 | API | REST、GraphQL、Webhook、OAuth、SDK |
| `automation` | 開發 | 自動化 | Cron、Scheduler、Batch、Task |
| `deploy` | 開發 | 部署 | CI/CD、GitHub Actions、Release |
| `architecture` | 開發 | 系統架構 | Design Pattern、Module、Architecture |
| `config` | 開發 | 設定 | YAML、JSON、ENV、設定檔 |
| `test` | 開發 | 測試 | Unit Test、Integration Test、E2E |
| `database` | 開發 | 資料庫 | SQLite、PostgreSQL、DuckDB、MySQL |
| `github` | 開發 | GitHub | Repository、Commit、Branch、PR、Release |

### 維運（6）

| Tag | 類別 | 用途 | 關鍵詞 / 涵蓋內容 |
| --- | --- | --- | --- |
| `linux` | 維運 | Linux | Ubuntu、Shell、systemd |
| `docker` | 維運 | Docker | Container、Compose、Image |
| `vps` | 維運 | VPS | Cloud Server、Virtual Machine |
| `network` | 維運 | 網路 | DNS、Nginx、SSL、Reverse Proxy |
| `backup` | 維運 | 備份 | rsync、Git Backup、Snapshot、Recovery |
| `sync` | 維運 | 同步 | WebDAV、SSHFS、Cloud Sync |

### 管理（6）

| Tag | 類別 | 用途 | 關鍵詞 / 涵蓋內容 |
| --- | --- | --- | --- |
| `dashboard` | 管理 | 儀表板 | Dashboard、Kanban、Overview |
| `telegram` | 管理 | Telegram | Bot、推播、Notification |
| `web` | 管理 | 網頁 | Website、HTTP、Frontend、Browser |
| `report` | 管理 | 報告 | PDF、Daily Report、Summary |
| `news` | 管理 | 新聞 | Tech News、Market News、Daily News |
| `data` | 管理 | 資料 | JSON、CSV、DataFrame、Data Processing |

### 維護（4）

| Tag | 類別 | 用途 | 關鍵詞 / 涵蓋內容 |
| --- | --- | --- | --- |
| `performance` | 維護 | 效能 | Benchmark、Optimization、Cache |
| `security` | 維護 | 安全 | Authentication、Authorization、API Key、Encryption |
| `cost` | 維護 | 成本 | Token、API 費用、成本最佳化 |
| `troubleshoot` | 維護 | 問題排查 | Debug、Error、Fix、Exception |

### 其他（3）

| Tag | 類別 | 用途 | 關鍵詞 / 涵蓋內容 |
| --- | --- | --- | --- |
| `shopping` | 購物 | 商品 | 購物商品記錄 |
| `compare` | 比較 | 物件比較 | 表格比較 |
| `ivan` | 特殊 | 特殊保留 | 使用者保留 tag，自用 |

### 已淘汰並合併（供參考）

| 舊 tag | 合併到 |
|---|---|
| `auto` → | `automation` |
| `tw-stock` → | `stock` |
| `valuation` → | `finance` |
| `source` → | `data` |
| `flowershow` → | `quartz` |
| `knowledge` → | `pkm` |
| `lint` → | `troubleshoot` |
| `monitor` → | `dashboard` |
| `storage` → | `database` |
| `webdav` → | `sync` |
| `sshfs` → | `sync` |
| `nginx` → | `network` |
| `integration` → | `api` |

### 分類統計（來源：Claude 對話建議 2026-06-27）

| 類別 | 數量 |
|------|------|
| AI | 10 |
| PKM | 6 |
| 投資 | 2 |
| 開發 | 9 |
| 維運 | 6 |
| 管理 | 6 |
| 維護 | 4 |
| 購物 | 1 |
| 比較 | 1 |
| 特殊 | 1 |
| **總計** | **48 個** |

> 本 tags 體系整合自 `obsidian/obsidian-tags-optimization-recommendations.md`（2026-07-10 併入本檔為單一來源）。

---
## summary

```text
1~2 句 | 用於 RAG 檢索 / Agent 理解
```

---
## created / updated

```yaml
created: 2026-06-21   # 建立後不改
updated: 2026-06-21   # 內容修改時同步更新
```

---
## status

```text
draft | active | permanent | archived | deprecated
預設：active
```

```text
draft       # 草稿，不發布筆記 / 剛建立、內容零碎、還在收集資料或構思階段的筆記，這屬於「半成品」
active      # 使用中，正常發布筆記 / 目前正在進行、高頻率修改、具有時效性的筆記，像是進行中的工作、今年的計畫、最近在讀的書
permanent   # 長期知識 / 經過消化吸收，提煉出來的核心觀念、本質原理或長期不變的個人原則
archived    # 封存，不再維護 / 專案已經結束、事情已經做完，但未來可能需要「查閱當時紀錄」的筆記
deprecated  # 已淘汰，保留歷史 / 裡面的內容已經過時、被新方法取代、或者觀念被自己推翻了
```

---
## sources / related（選填）

```yaml
sources:
  - raw/articles/example.md

related:
  - page-name
```

> ⚠️ `related` 禁止使用 `wikilinks`，改放正文。

---
# YAML 完整性規則

```text
1. list 型欄位（tags/sources/related）必須有明確 key + 縮排列表
2. 建立/修改後用 yaml.safe_load() 驗證語法
3. lint 腳本須全域掃描所有 .md，發現錯誤立即回報
```

**最常見錯誤（tags key 缺失）：**

```yaml
# ❌ 錯誤
status: active
  - terminal
  - memory

# ✅ 正確
status: active
tags:
  - terminal
  - memory
```

---
# Required Fields 必填項目

```text
title / type / tags / summary / created / updated
```

缺少任一 → `Lint P0`，必須修復。

---
# Field Format Rules 欄位格式規則

```text
- 所有 key 必須小寫（title / tags / created，禁止 Title / Tags）
- Frontmatter 禁止 wikilink / markdown syntax / HTML tag
- 所有 [[wikilinks]] 放在 --- 之後正文區域
```

---
# Anti-Patterns 禁止欄位

```yaml
# 禁止以下欄位：
publish / subtype / confidence / priority
importance / owner / reviewer / version / category
```

---

# Portfolio 專用欄位（豁免白名單）

`ivan-notes/finance/portfolio/hold-*.md` 持倉頁允許以下自訂欄位（不在通用白名單，但為投資組合計算所需，lint 不報錯）：

```text
stock_id      # 股票代號，字串，如 "2330"
stock_name    # 股票名稱
avg_cost      # 平均成本價格（對帳單）
current_price # 參考市價 / 收盤價（Agent 每日更新）
shares        # 庫存餘額 / 股數（Agent 買賣後更新）
currency      # 計價幣別，如 "USD" / "TWD"（美股為 USD）
```

計算邏輯（Dataview）：市值 = current_price × shares；成本 = avg_cost × shares；損益 = (current_price − avg_cost) × shares；報酬率% = (current_price − avg_cost) / avg_cost × 100。

---
# Page Examples 頁面範例

```yaml
# concept
---
title: Price Earnings Ratio
type: concept
tags:
  - finance
  - stock
summary: PE Ratio 用於衡量股票估值的重要財務指標。
created: 2026-06-21
updated: 2026-06-21
status: active
---

# entity
---
title: Taiwan Semiconductor
type: entity
tags:
  - stock
summary: 台積電為全球領先晶圓代工企業。
created: 2026-06-21
updated: 2026-06-21
status: active
---

# report
---
title: Weekly Market Report
type: report
tags:
  - stock
  - finance
summary: 本週市場重點與投資觀察。
created: 2026-06-21
updated: 2026-06-21
status: active
---
```

---
# Success Criteria 通過標準

```text
✓ 所有頁面皆有 Frontmatter
✓ 所有頁面通過 yaml.safe_load 驗證
✓ 所有頁面皆有 summary / type / updated
→ Frontmatter Status: HEALTHY
```

---

## 相關節點
- [[schema]]
