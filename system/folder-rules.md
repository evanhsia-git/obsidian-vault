---
status: active
title: "Folder Structure Rules"
summary: "Folder Structure Rules - 目錄結構與讀寫權限規範（2026-07-10 實際結構重寫）"
description: "目錄結構、讀寫權限與資料夾操作規範；依實際 vault 結構重寫，權限模型以 schema.md 為準"
type: concept
tags: [architecture, flow, security]
created: 2026-06-21
updated: 2026-07-10
---

**Folder Structure Rules**

新增資料夾、刪除資料夾、調整目錄結構時必須遵守。

> 本檔於 2026-07-10 依實際 vault 結構重新檢視並重寫。權限模型以 `schema.md`（核心憲法）為唯一權威來源；本檔僅描述目錄拓撲與補充操作守則，不得與 schema.md 衝突。

---

## 目錄結構（實際狀態）

```text
/ (Root)
├── SCHEMA.md          # 規範憲法（禁止覆寫）
├── POLICY.md          # 規則路由器（禁止覆寫）
├── index.md           # 全站 MOC（禁止覆寫）
├── log.md             # 變更日誌（禁止覆寫）
├── .gitignore
├── _archive/          # [不存在] 封存區（規劃中，尚未建立）
│
├── raw/               # [唯讀] Layer 1 RAG 原始素材
│   ├── articles/
│   ├── papers/
│   ├── transcripts/
│   └── assets/
├── clippings/         # Layer 2 剪報/書摘/外部素材筆記
├── concepts/          # Layer 2 概念知識
│   ├── frameworks/
│   ├── notes/
│   ├── projects/
│   └── system/
├── entities/          # Layer 2 實體資料
│   └── cms/
├── finance/           # Layer 2 股市/金融知識
│   └── quant/         # 量化交易子領域（策略/選股規格/agent 架構）
├── notes/             # Layer 2 鬆散工作筆記（daily-news-sources、個股分析等）
├── obsidian/          # Layer 2 Obsidian/Quartz/WebDAV 設定與維運知識
├── queries/           # Layer 2 查詢報告
│   ├── market-reports/
│   └── reports/
├── reports/           # Layer 2 報告產出
 │   └── drafts/
 ├── resources/         # Layer 2 資源鏈結
├── project/           # Layer 2 專案管理
│   ├── mklab-stock.md
│   ├── mklab-stock-design.md
│   ├── mklab-stock-dev.md
│   ├── mklab-stock-prompt.md
│   ├── mklab-stock-qa-1.md
│   ├── mklab-stock-qa-2.md
│   ├── mklab-stock-resource.md
│   ├── mklab-stock-schema.md
│   ├── mklab-stock-skill.md
│   ├── mklab-stock-v1.md
│   ├── mklab-stock-v2-100個功能.md
│   ├── mklab-stock-v3.md
│   └── project-index.md
├── skills/            # Agent 技能定義
 │   ├── architecture-references/
 │   ├── blogwatcher/
 │   ├── daily-news-stock-market/
 │   ├── evolution/
 │   └── troubleshooting/
 ├── system/            # 系統架構、規則、部署（含本檔與 frontmatter-rules 等）
├── config/            # [空] 配置暫存區（目前無內容）
│
├── assets/            # 全站靜態資源（圖片、附件等）
│   ├── attachments/
│   ├── image/
│   ├── pdf/
│   └── svg/
│
├── database/          # [Git-ignored] SQLite + CSV
│   └── csv_exports/
├── copilot/           # [Git-ignored][L3 保護] Copilot 外部工具管理區
│   └── copilot-custom-prompts/
│
├── .claude/           # [L3 保護] Claude Code 管理區（禁止編輯/移動/刪除）
│   ├── agents/
│   ├── commands/
│   └── skills/
├── .claudian/         # [L3 保護] Claudian 管理區（禁止編輯/移動/刪除）
│   └── sessions/
└── ivan-notes/        # [Git-ignored][唯讀] 禁止任何 move/edit/delete/create
    ├── dayi/
    ├── hermes/
    ├── linux/
    └── templates()
```

### 已移除的過時項目（舊 folder-rules 記載但實際不存在）

以下目錄在實際 vault 中**不存在**，自本版移除：
`wiki.md`（根）、`raw.md`（根）、`_archive/`、`publish/`（含 drafts）、`util/`、`temp/`、`scripts/`（頂層 Python 腳本實際位於 Hermes skills 目錄，非 vault 內）。

---

## 讀寫權限

權限模型以 `schema.md` 為準，下表為速查：

| 目錄 | 權限 | 備註 |
|:--|:--|:--|
| `raw/` | 唯讀 | Agent 禁止修改 / 刪除 / 搬移 / 重新命名（Layer 1） |
| `ivan-notes/` | 唯讀 | Agent 禁止任何 move/edit/delete/create（Git-ignored） |
| `copilot/` | 禁止編輯/移動/刪除 | 外部工具管理區（Git-ignored，L3 保護） |
| `.claude/` `.claudian/` | 禁止編輯/移動/刪除 | 外部工具管理區（L3 保護） |
| `database/` | Git-ignored | 禁止推送；禁止刪除 |
| `assets/` | 讀寫 | 靜態資源存放區 |
| Layer 2 目錄（clippings/concepts/entities/finance/notes/obsidian/queries/reports/resources/skills/system/config） | 讀寫 | 新增/移動須取得使用者核准 |
| 根憲法檔 `SCHEMA.md` `POLICY.md` `index.md` `log.md` | 禁止覆寫 | 核心憲法 |

---

## 安全規範

禁止（詳見 schema.md Safety Rules）：

- 修改 `raw/`
- 覆寫 `SCHEMA.md` / `POLICY.md` / `index.md` / `log.md`
- 刪除 `raw/` 以外任何目錄（`database/` `skills/` `system/` `copilot/` `.claude/` `.claudian/` 等）
- 未經核准移動 Layer 2 筆記
- `rm -rf` / 批次刪除 / 批次搬移 / 批次重命名

重大變更（新增 / 刪除 / 重命名資料夾 / 修改 schema·policy·index）需取得使用者核准，流程：`方案 → 影響評估 → 使用者核准 → 執行 → 記錄 Log`。

---

## 目錄分類說明（實際用途）

| 目錄 | 分類 | 說明 |
|--|--|--|
| `raw/` | Layer 1 | RAG 原始素材，唯讀 |
| `clippings/ concepts/ entities/ finance/ notes/ obsidian/ queries/ reports/ resources/ skills/ system/` | Layer 2 | 結構化知識 / 知識圖譜 / Agent 工作區，可讀可寫 |
| `finance/quant/` | Layer 2 子域 | 量化交易專屬（選股規格、策略指南、agent 架構） |
| `obsidian/` | Layer 2 | Obsidian/Quartz/WebDAV 部署與維運知識（非 vault 設定檔本身） |
| `notes/` | Layer 2 | 鬆散工作筆記，未歸類進 concepts/finance 的臨時性內容 |
| `config/` | Layer 2 | 配置暫存區（目前空） |
| `assets/` | 資源 | 圖片/PDF/SVG 等靜態附件 |
| `database/` | 資料 | SQLite + CSV 匯出（Git-ignored） |
| `copilot/` `.claude/` `.claudian/` | 外部工具 | 第三方 AI 工具管理區，L3 保護或 Git-ignored |
| `ivan-notes/` | 使用者專屬 | 使用者私人筆記，唯讀保護 |

---

## 相關連結

- [[schema]]：核心憲法
- [[policy]]：規則路由器
- [[frontmatter-rules]]：frontmatter 欄位規範（位於 system/）
