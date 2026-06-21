---
title: Database Rules
summary: Database Rules：**Database Rules**
description: SQLite 資料庫操作與資料抓取規範
type: concept
tags: [database, sqlite, rules, data]
created: 2026-06-21
updated: 2026-06-21
---

**Database Rules**

資料庫相關操作前必須遵守。

**資料庫位置**

- 路徑：`database/`（Git-ignored）
- 格式：SQLite + CSV

**適用範圍**

- 台股資料庫
- 美股資料庫
- ETF 資料庫
- 庫存資料庫

**操作原則**

1. 查詢優先於寫入
2. 寫入前檢查資料是否存在（避免重複）
3. 大量寫入使用事務（transaction）
4. 操作完成後驗證資料完整性

**資料源**

- 台灣證券交易所（TWSE）OpenAPI
- 證券櫃檯買賣中心（TPEX）OpenAPI
- Yahoo Finance API
- FinMind API

**安全規範**

- 資料庫檔案禁止手動修改
- 備份由 [[system/backup-rules]] 處理
- 敏感資料（API Key）禁止寫入資料庫

**相關連結**

- [[schema]]：核心憲法
- [[policy]]：規則路由器
