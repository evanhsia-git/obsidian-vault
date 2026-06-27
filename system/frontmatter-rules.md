---
title: "FRONTMATTER-RULES"
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

### AI（10）

| tag | 用途 |
|---|---|
| ai | LLM、Generative AI、AI 應用 |
| agent | Multi-Agent、Agent Framework、Tool Calling |
| prompt | System Prompt、Few-shot、Prompt Pattern |
| model | Claude、GPT、Gemini、Qwen、DeepSeek |
| memory | Session Memory、Long-term Memory、Context |
| workflow | Pipeline、Workflow、Automation Flow |
| hermes | SOUL、Skills、Hermes Workflow |
| mcp | Model Context Protocol、MCP Server |
| rag | Retrieval、Embedding、Knowledge Base |
| vector | Embedding、Vector Database、Semantic Search |

### PKM（6）

| tag | 用途 |
|---|---|
| pkm | Second Brain、Zettelkasten、PARA、Wiki |
| obsidian | Vault、Markdown、Plugin、WebDAV |
| quartz | Publish、Static Site、Deploy |
| schema | Metadata、Schema、Frontmatter、Vault Rules |
| plugin | Community Plugin、Extension |
| template | Template、Snippet、Boilerplate |

### 投資（2）

| tag | 用途 |
|---|---|
| stock | 台股、美股、ETF、個股 |
| finance | 財報、PE、PB、EPS、殖利率、估值、CPI、GDP、Fed、央行、利率、匯率 |

### 開發（9）

| tag | 用途 |
|---|---|
| python | Script、venv、Library、Automation |
| api | REST、GraphQL、Webhook、OAuth、SDK |
| automation | Cron、Scheduler、Batch、Task |
| deploy | CI/CD、GitHub Actions、Release |
| architecture | Design Pattern、Module、Architecture |
| config | YAML、JSON、ENV、設定檔 |
| test | Unit Test、Integration Test、E2E |
| database | SQLite、PostgreSQL、DuckDB、MySQL |
| github | Repository、Commit、Branch、PR、Release |

### 維運（6）

| tag | 用途 |
|---|---|
| linux | Ubuntu、Shell、systemd |
| docker | Container、Compose、Image |
| vps | Cloud Server、Virtual Machine |
| network | DNS、Nginx、SSL、Reverse Proxy |
| backup | rsync、Git Backup、Snapshot、Recovery |
| sync | WebDAV、SSHFS、Cloud Sync |

### 管理（6）

| tag | 用途 |
|---|---|
| dashboard | Dashboard、Kanban、Overview |
| telegram | Bot、推播、Notification |
| web | Website、HTTP、Frontend、Browser |
| report | PDF、Daily Report、Summary |
| news | Tech News、Market News、Daily News |
| data | JSON、CSV、DataFrame、Data Processing |

### 維護（4）

| tag | 用途 |
|---|---|
| performance | Benchmark、Optimization、Cache |
| security | Authentication、Authorization、API Key、Encryption |
| cost | Token、API 費用、成本最佳化 |
| troubleshoot | Debug、Error、Fix、Exception |

### 其他（3）

| tag | 用途 |
|---|---|
| shopping | 購物商品記錄 |
| compare | 表格比較 |
| ivan | 使用者保留 tag，自用 |

### 已淘汰並合併（供參考）

| 舊 tag | 合併到 |
|---|---|
| auto → | automation |
| tw-stock → | stock |
| valuation → | finance |
| source → | data |
| flowershow → | quartz |
| knowledge → | pkm |
| lint → | troubleshoot |
| monitor → | dashboard |
| storage → | database |
| webdav → | sync |
| sshfs → | sync |
| nginx → | network |
| integration → | api |
| clippings → | （保留，不合併） |

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
