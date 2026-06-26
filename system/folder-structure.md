---
status: active
title: "Folder Structure Rules"
summary: "Folder Structure Rules - 目錄結構與讀寫權限規範"
description: "目錄結構、讀寫權限與資料夾操作規範"
type: concept
tags: [architecture, flow, security]
created: 2026-06-21
updated: 2026-06-26
---

**Folder Structure Rules**

新增資料夾、刪除資料夾、調整目錄結構時必須遵守。

**目錄結構**

```
/ (Root)
├── SCHEMA.md          # 規範憲法（禁止覆蓋）
├── POLICY.md          # 規則路由器（禁止覆蓋）
├── index.md           # 全站 MOC（禁止覆蓋）
├── log.md             # 變更日誌
├── wiki.md            # 圖譜導航核心
├── raw.md             # RAG 素材快照索引
├── .gitignore
│
├── _archive/          # 封存區（過時頁面）
├── raw/               # [唯讀] Layer 1 RAG 原始素材
│   ├── articles/
│   ├── papers/
│   ├── transcripts/
│   └── assets/
├── concepts/          # Layer 2 概念知識
│   ├── frameworks/
│   ├── notes/
│   ├── projects/
│   └── system/
├── entities/          # Layer 2 實體資料
│   └── cms/
├── finance/           # Layer 2 股市/金融知識
├── resources/         # Layer 2 資源鏈結
├── reports/           # Layer 2 報告產出
│   └── drafts/
├── queries/           # Layer 2 查詢報告
│   ├── market-reports/
│   └── reports/
├── skills/            # Agent 技能定義
│   ├── architecture-references/
│   ├── blogwatcher/
│   ├── daily-news-stock-market/
│   ├── evolution/
│   └── troubleshooting/
├── system/            # 系統架構、規則、部署
├── database/          # [Git-ignored] SQLite + CSV
├── scripts/           # Python 自動化腳本
├── util/              # TypeScript 工具（Quartz 框架）
├── publish/           # 草稿 → 發佈工作流
│   └── drafts/
├── ivan-notes/        # [Git-ignored] 唯讀，禁止任何 move/edit/delete/create
└── temp/              # 暫存區
```

**讀寫權限**

| 目錄 | 權限 | 備註 |
|:--|:--|:--|
| raw/ | 唯讀 | Agent 禁止修改或移動 |
| database/ ivan-notes/ scripts/ util/ temp/ | Git-ignored | 禁止推送 |
| Layer 2 目錄 | 讀寫 | 須取得使用者核准 |
| SCHEMA.md / POLICY.md / index.md / log.md | 禁止覆蓋 | 核心憲法 |

**安全規範**

禁止：

- 修改 raw/
- 覆蓋 SCHEMA.md、POLICY.md、index.md、log.md
- 刪除 raw/ 以外任何目錄（database/、skills/、system/ 等）
- 未經核准移動 Layer 2 筆記

重大變更需取得使用者核准。

**相關連結**

- [[schema]]：核心憲法
- [[policy]]：規則路由器
