---
title: "SCHEMA"
description: "Hermes Agent 知識庫結構與執行規範"
summary: "Obsidian Vault、Wiki-LLM、Hermes Agent 統一規範"
type: schema
status: permanent
tags: [hermes, obsidian, rag]
created: 2026-05-27
updated: 2026-06-21
---

# Core Domains

僅允許建立以下領域內容：

- taiwan-stock
- us-stock
- hermes-agent
- obsidian
- ai-llm

超出範圍需使用者確認。

---

# Mandatory Navigation

任務開始前：

```python
read("SCHEMA.md")
read("index.md")
read("log.md", last=30)
```

完成後回報：

```text
[1/3] SCHEMA ✓ | [2/3] INDEX ✓ | [3/3] LOG ✓
```

同一對話僅執行一次。

---

# Agent Rules

```yaml
agent_rules:
  cache_first: true
  max_retry: 3
  stop_on_loop: true
  update_existing_first: true
  avoid_duplicate_pages: true
  require_bidirectional_links: true
  protect_system_files: true
```

---

# Failure Protection

連續失敗：

```yaml
max_retry: 3
```

立即停止。

輸出：

```text
[STOP]
Task failed 3 times.
Awaiting user decision.
```

禁止：

- 無限重試
- 無限建立檔案
- 無限搬移檔案
- 無限刪除檔案
- 無限修改同一頁

---

# Frontmatter Standard

所有 Layer2 頁面必須包含：

```yaml
---
title:
type:
status:
tags:
summary:

created:
updated:

sources: []
related: []
---
```

## Required

```yaml
title
type
tags
summary
created
updated
```

## Optional

```yaml
status
sources
related
```

---

# Type Pool

禁止新增。

```yaml
entity
concept
project
resource
report
query
task
index
log
schema
```

---

# Status Pool

禁止新增。

```yaml
draft
active
permanent
archived
deprecated
```

---

# Tag Rules

原則：

- 使用既有標籤
- 優先少量標籤
- 避免同義標籤

建議：

```yaml
ai
llm
rag
agent
memory
workflow
obsidian
wiki
hermes

taiwan-stock
us-stock
etf

database
sqlite

automation
backup

quartz
webdav
linux
vps
```

---

# Directory Structure

```text
/
├── SCHEMA.md
├── index.md
├── log.md

├── raw/
├── concepts/
├── entities/
├── resources/
├── reports/
├── queries/

├── database/
│
├── skills/
├── scripts/
├── system/

├── publish/
├── temp/
├── _archive/
```

---

# Wiki-LLM Architecture

## Layer 1

```text
raw/
```

內容：

- 原始文章
- PDF
- 新聞
- 財報
- 網頁摘要
- 訪談

規則：

唯讀。

禁止：

- 修改
- 刪除
- 搬移

---

## Layer 2

```text
concepts/
entities/
resources/
reports/
queries/
```

內容：

- 整理後知識
- RAG 節點
- 圖譜節點

Agent 主要工作區。

---

# Database Rules

資料庫集中管理：

```text
database/
```

建議結構：

```text
database/

├── tw-stock/
├── tw-etf/

├── us-stock/
├── us-etf/

├── portfolio/

├── reports/

├── backup/
```

---

# Database Protection

禁止：

```text
DROP DATABASE
rm *.db
overwrite database
```

未經確認不得執行。

---

# Backup Policy

資料庫：

每日：

```text
database/backup/daily/
```

每週：

```text
database/backup/weekly/
```

每月：

```text
database/backup/monthly/
```

保留：

```yaml
daily: 7
weekly: 4
monthly: 12
```

---

# Wiki Rules

每頁至少：

```text
2 個 wikilinks
1 個 inbound link
```

禁止：

```text
孤立節點
重複頁面
```

新增頁面後：

```text
更新 index.md
更新 log.md
```

---

# RAG Rules

流程：

```text
Raw
 ↓
Retrieve
 ↓
Summarize
 ↓
Structured Page
 ↓
WikiLinks
 ↓
Graph
```

原則：

```text
更新舊頁
優先於
建立新頁
```

避免知識分裂。

---

# Page Templates

## Concept

```text
Definition
Key Ideas
Related Concepts
Sources
```

---

## Entity

```text
Overview
Facts
Timeline
Related Entities
Sources
```

---

## Project

```text
Goal
Milestones
Progress
Status
```

---

## Resource

```text
Name
Description
Usage
Related
```

---

## Report

```text
Summary
Findings
Data
Sources
```

---

## Task

```text
Objective
Priority
Status
Due Date
```

---

# Page Thresholds

建立新頁：

```text
至少 2 個來源
```

或：

```text
單一核心主題
```

---

更新：

```text
優先更新既有頁
```

---

拆分：

```text
超過 200 行
```

---

封存：

```text
deprecated
obsolete
```

移至：

```text
_archive/
```

---

# Conflict Policy

優先順序：

```text
1. 最新資料
2. 多來源驗證
3. 保留衝突內容
4. 提醒使用者裁決
```

---

# Structural Change Policy

以下必須先核准：

- 新增資料夾
- 刪除資料夾
- 大量搬移
- 重新命名目錄
- 修改 SCHEMA
- 修改 index

流程：

```text
Plan
↓
Impact
↓
Approval
↓
Execute
↓
Log
```

---

# Safety Rules

禁止：

```text
rm -rf
批次刪除
批次搬移
批次重新命名

覆蓋 SCHEMA
覆蓋 index

刪除 database
刪除 skills
刪除 scripts
```

未經確認不得執行。

---

# Git Rules

禁止同步：

```text
.env
*.db
*.sqlite

database/
temp/

node_modules/

venv/
.venv/

__pycache__/
```

---

# Output Format

所有操作：

```text
[RESULT]
內容

[LINKS]
相關 wikilinks

[LOG]
log.md 項目
```

---

# Priority

```text
P0 = 錯誤修復
P1 = 使用者任務
P2 = 補全連結
P3 = 維護建議
```

---

# Lint Checklist

檢查：

```text
孤立節點
壞連結
缺 Frontmatter
未收錄 Index
超過 200 行
超過 90 天未更新
```

P0：

```text
孤立節點
壞連結
```

立即修復。

---

# Cache Strategy

首次載入：

```text
SCHEMA
index
log
```

快取於當前 Context。

重新讀取條件：

```text
使用者要求
Schema 更新
結構變更
```

否則使用快取。

---

# Hermes Agent Constitution

最高原則：

1. 保護資料優先於完成任務
2. 更新頁面優先於建立頁面
3. 停止失控優先於持續執行
4. 保持圖譜完整性
5. 不產生孤立節點
6. 不建立重複知識
7. 不修改 Layer1 Raw Data
8. 所有重大變更必須取得使用者核准