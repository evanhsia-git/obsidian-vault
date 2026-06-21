---
title: Hermes Agent 執行策略
description: Hermes Agent 執行策略 — 概念說明頁面
summary: Hermes Agent 執行策略
type: concept
status: active
priority: P2
tags: [maintenance]
aliases: []
created: 2026-05-31
updated: 2026-05-31
date: 2026-05-31
publish: true
draft: false
related:
source:
due:
review:
---

## 相關頁面
- [[concepts/hermes-configuration|Hermes 配置]]
- [[concepts/obsidian-wiki-conventions|Obsidian Wiki 使用規範]]


# Hermes Agent 效能優化與任務執行策略

為了達到「加快執行動作」的目標，Hermes Agent 採用階層式資源存取策略，避免重複性工作與不必要的延遲。

## 資源存取階層 (Hierarchy of Access)

### 1. 記憶庫優先 (快取層)
- **觸發方式**: 系統啟動與每一則訊息自動注入。
- **存取資源**: `MEMORY.md` 與 `USER PROFILE`。
- **用途**: 處理偏好設定（如 PDF 字體、輸出格式、傳送方式）、 recurring 習慣與已建立的穩定規範。這是最快速的執行層。

### 2. 會話紀錄回溯 (歷史層)
- **觸發方式**: 當涉及過去任務流程或邏輯確認時。
- **存取工具**: `session_search`。
- **用途**: 利用 `bookend`（會話頭尾）快速重組過去任務的 goal $
ightarrow$ resolution，避免閱讀全文。

### 3. 知識庫層 (深度查閱層)
- **觸發方式**: 當任務涉及「知識檢索」、「數據篩選」或「關聯分析」時。
- **存取資源**: `Obsidian Vault` (Wiki) 與 `SQLite 資料庫`。
- **行為規範**: Agent 不會自動掃描所有筆記以避免延遲；若您下達指令時提及「查閱 wiki」、「ob 內容」、「SQLite 記錄」，Agent 將優先調用這些資源。

## 給使用者的建議：如何讓 Agent 更快？
若希望 Agent 主動調用特定知識庫資源，請在指令中使用以下關鍵詞：
- **「查閱 ob / wiki...」**: 強制 Agent 優先同步最新的知識框架。
- **「依據 SQLite 儲存的...」**: 強制 Agent 調用資料庫內的結構化數據。

---
*註: 此內容為 Agent 對於任務執行邏輯的自我檢視與說明。*

相關頁面：[[environment-keys|Environment Keys]]

相關頁面：[[news-push-log]]


## 相關節點
- [[index]]
