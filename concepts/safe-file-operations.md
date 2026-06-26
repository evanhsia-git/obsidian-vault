---
status: active
title: "安全檔案操作規範"
description: "LLM 執行破壞性檔案操作的安全規則"
summary: "四條核心規則：失敗停止、操作需批准、無法判斷先問、安全優於完成"
type: concept
tags: [security, linux, flow]
created: 2026-06-21
updated: 2026-06-21
---

**安全檔案操作規範**

**四條核心規則**

1. **任務連續失敗 3 次 → 立即停止**，不嘗試替代方案
2. **未經批准不得刪除、移動或重新命名檔案**
3. **無法判斷時停止並請求人工確認**
4. **資料安全優先於完成**

**免確認範圍**

以下路徑內的 mv/cp 可免確認（仍需逐步驗證）：
- `/root/Documents/Obsidian Vault/`
- `/root/Documents/Quartz/`
- `/root/.hermes/`

**唯讀區域（任何情況禁止寫入/刪除/修改）**
- `ivan-notes/`
- `raw/`

**Rollback**

重要操作前若有未提交變更，先 `git add -A && git commit -m "pre-op"`。出問題時 `git checkout .` 恢復。

**教訓**

2026-06-21：LLM 失控執行 rm/mv/mkdir，未確認、未驗證，導致檔案遺失。此規範由此事故建立。

**相關頁面**

- [[schema]]：核心憲法
- [[system/folder-structure]]：目錄結構與權限
