---
title: "policy"
description: "Hermes Agent 規則路由器"
summary: "根據任務類型載入對應規範"
type: schema
status: permanent
tags: [flow, hermes, obsidian]
created: 2026-06-21
updated: 2026-06-21
---
tags
# Purpose

本文件負責規則路由。

Agent 僅在需要時讀取對應規範，避免載入整個 system。

---

# Page Operations

建立頁面、修改頁面、更新頁面時：

```python
read("system/frontmatter-rules.md")
```

適用：

* concept
* entity
* project
* resource
* report
* query
* task

---

# Folder Operations

新增資料夾、刪除資料夾、調整目錄結構時：

```python
read("system/folder-structure.md")
```

---

# Database Operations


- [[schema]]
- [[system/system-index]]

資料庫相關操作前：

```python
read("system/database-rules.md")
```

適用：

* SQLite
* 台股資料庫
* 美股資料庫
* ETF 資料庫
* 庫存資料庫

---

# Backup Operations

備份、同步、還原前：

```python
read("system/backup-rules.md")
```

適用：

* rsync
* Git Backup
* Database Backup
* Snapshot

---

# Quartz Publishing

發布網站前：

```python
read("system/quartz-rules.md")
```

適用：

* Quartz
* GitHub Pages
* GitHub Actions

---

# Telegram Output

輸出 Telegram 訊息前：

```python
read("system/telegram-output-rules.md")
```

適用：

* Telegram Bot
* Daily News
* 推播訊息

---

# Multiple Rules

若任務涉及多個領域：

依序載入。

範例：

```text
更新台股分析報告
↓
frontmatter-rules

產生資料
↓
database-rules

發布 Quartz
↓
quartz-rules
```

---

# Priority

```text
SCHEMA
↓
POLICY
↓
SYSTEM RULES
↓
TASK
```

高層規則優先。

---

# Default Behavior

若無對應規範：

僅依照：

```text
SCHEMA.md
```

執行。

禁止自行推測不存在的規則。

---

# Safety

若任務可能：

* 刪除資料
* 覆蓋資料
* 搬移大量檔案
* 修改目錄結構

必須先讀取：

```python
read("system/folder-structure.md")
```

並取得使用者核准。

---

# Router Principle

只讀取需要的規範。

避免：

* 載入全部 system
* 重複讀取相同規範
* 增加不必要 Context

目標：

```text
最小載入
最高效率
最大安全性
```