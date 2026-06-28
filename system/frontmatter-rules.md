---
title: "FRONTMATTER-RULES"
description: "Frontfrontmatter 規範"
type: schema
status: permanent
summary: "Obsidian Frontmatter 規範 — Hermes Agent 執行版"
tags: [knowledge, workflow, config]
created: 2026-06-21
updated: 2026-06-27
---

# Standard Template

```yaml
---
title:
type:
tags:
summary:
created:
updated:
status: active
sources: []
related: []
---
```

---

# Required Fields

```text
title / type / tags / summary / created / updated
```

缺少任一 → `Lint P0`，必須修復。

---

# Field Rules

## type

```text
entity | concept | project | resource | report | query | task | index | log | schema
```

禁止新增。

---

## tags

規則：
- 全 Vault 限 48 個核心 tag（上限 50，禁止新增）
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

---

## summary

```text
1~2 句 | 用於 RAG 檢索 / Agent 理解
```

---

## status

```text
draft | active | permanent | archived | deprecated
預設：active
```

---

## created / updated

```yaml
created: 2026-06-21   # 建立後不改
updated: 2026-06-21   # 內容修改時同步更新
```

---

## sources / related（選填）

```yaml
sources:
  - raw/articles/example.md

related:
  - page-name
```

> ⚠️ `related` 禁止使用 `[[wikilinks]]`，改放正文。

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

# Field Format Rules

```text
- 所有 key 必須小寫（title / tags / created，禁止 Title / Tags）
- Frontmatter 禁止 wikilink / markdown syntax / HTML tag
- 所有 [[wikilinks]] 放在 --- 之後正文區域
```

---

# Anti-Patterns

```yaml
# 禁止以下欄位：
draft / publish / subtype / confidence / priority
importance / owner / reviewer / version / category
```

---

# Page Examples

```yaml
# concept
---
title: Price Earnings Ratio
type: concept
tags:
  - valuation
  - tw-stock
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
  - tw-stock
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
  - tw-stock
  - finance
summary: 本週市場重點與投資觀察。
created: 2026-06-21
updated: 2026-06-21
status: active
---
```

---

# Success Criteria

```text
✓ 所有頁面皆有 Frontmatter
✓ 所有頁面通過 yaml.safe_load 驗證
✓ 所有頁面皆有 summary / type / updated
→ Frontmatter Status: HEALTHY
```

---

## 相關節點
- [[schema]]
