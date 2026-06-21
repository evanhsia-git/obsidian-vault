---
title: SCHEMA
description: Hermes Agent 核心憲法
summary: Obsidian Wiki、Wiki-LLM、Hermes Agent 核心規範
version: "3.2"
type: schema
status: permanent
tags: [schema, hermes, wiki, obsidian]
created: 2026-06-21
updated: 2026-06-21
---

# Purpose

本文件為知識庫最高規範。

定義：

- 核心原則
- 知識架構
- Metadata 標準
- 安全規則
- Agent 行為

詳細規範由 POLICY.md 路由管理。

---

# Core Domains

僅允許處理：

- 台灣股市
- 美國股市
- Hermes Agent
- Obsidian
- AI / LLM

超出範圍需取得使用者確認。

---

# Required Navigation

首次任務：

```python
read("SCHEMA.md")
read("POLICY.md")
read("index.md")
read("log.md", last=30)
```

完成後：

```text
[1/4] SCHEMA ✓
[2/4] POLICY ✓
[3/4] INDEX ✓
[4/4] LOG ✓
```

同一工作階段僅執行一次。

---

# Cache Strategy

後續任務：

使用快取。

重新載入條件：

- SCHEMA 更新
- POLICY 更新
- 結構變更
- 使用者要求

---

# Metadata Standards

所有 Layer 2 頁面必須包含：

```yaml
title:
type:
tags:
summary:
created:
updated:
```

---

# Type Pool

僅允許：

```text
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

禁止自行新增 Type。

---

# Status Pool

僅允許：

```text
draft
active
permanent
archived
deprecated
```

定義：

```text
draft       草稿
active      使用中
permanent   長期知識
archived    已封存
deprecated  已淘汰
```

預設：

```yaml
status: active
```

---

# Wiki-LLM Architecture

## Layer 1

```text
raw/
```

用途：

- 原始資料
- 新聞
- PDF
- 財報
- 網頁內容

權限：

```text
唯讀
```

禁止：

```text
修改
刪除
搬移
重新命名
```

---

## Layer 2

```text
concepts/
entities/
resources/
reports/
queries/
```

用途：
- 結構化知識
- 知識圖譜
- Agent 工作區

權限：
```text
可讀可寫
```
可讀可寫
```

---

# Knowledge Principles

1. 更新優先於建立
2. 避免重複頁面
3. 避免重複知識
4. 維護圖譜完整性
5. 禁止孤立節點
6. 建立頁面後更新 Index
7. 建立頁面後更新 Log

---

# Rule Loading

詳細規範由 POLICY.md 管理。

禁止：

```text
在 Skill 中重複定義規則
在多個檔案維護不同版本規則
```

規則唯一來源：

```text
SCHEMA.md
POLICY.md
system/*
```

---

# Structural Changes

以下操作需取得使用者核准：

- 新增資料夾
- 刪除資料夾
- 重新命名資料夾
- 修改 SCHEMA
- 修改 POLICY
- 修改 index

流程：

```text
方案
↓
影響評估
↓
使用者核准
↓
執行
↓
記錄 Log
```

---

# Safety Rules

禁止：

```text
rm -rf
批次刪除
批次搬移
批次重新命名
```

禁止修改：

```text
raw/
SCHEMA.md
POLICY.md
```

禁止刪除：

```text
database/
skills/
scripts/
system/
```

未取得核准不得執行。

---

# Failure Protection

最大重試：

```yaml
max_retry: 3
```

連續失敗三次：

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

# Priority

```text
P0 錯誤修復
P1 使用者任務
P2 補全連結
P3 維護建議
```

---

# Constitution

1. 保護資料優先於完成任務
2. 三次失敗立即停止
3. 更新頁面優先於建立頁面
4. 不建立重複知識
5. 不產生孤立節點
6. 不修改 Layer 1 原始資料
7. 重大變更必須取得使用者核准
8. 遵循 POLICY 路由規範
9. 遵循 system/* 詳細規範
10. 保持知識庫一致性與可維護性