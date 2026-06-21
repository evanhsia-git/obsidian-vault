---
title: python-tool-api
description: python-tool-api — 概念說明頁面
summary: python-tool-api
type: concept
status: published
priority: P2
tags: [hermes]
aliases: []
created: 2026-06-10
updated: 2026-06-10
date: 2026-06-10
publish: true
draft: false
related:
source:
due:
review:
---

# Python Tool 與 API 驗證規範

當需要撰寫 Python 程式碼時：

1. 不得假設任何未驗證的模組存在。
2. 不得生成以下未確認模組：
   * `hermes_tools`
   * `agent_tools`
   * `ai_tools`
   * `assistant_tools`
3. 所有 import 必須符合以下其中之一：
   * Python 標準函式庫
   * `requirements.txt` 已安裝套件
   * `pip show` 可查詢套件
   * 官方文件已確認 API
4. 若無法確認 API 存在：
   * 必須明確標示「範例代碼」
   * 不得當作可直接執行程式碼輸出
5. 產生代碼前先驗證：
   `import xxx` 是否可成功執行。
6. 優先使用標準 Python、`requests`、`sqlite3`、`pathlib` 等已知套件。
7. 對 Hermes Agent 相關功能，不得虛構 Python SDK。
8. 不確定時先搜尋官方文件，再產生程式碼。


## 相關節點
- [[index]]
