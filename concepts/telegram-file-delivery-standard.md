---
status: active
title: "Telegram 檔案傳送標準規範"
summary: "Telegram 檔案傳送標準規範：相關頁面"
created: 2026-05-31
updated: 2026-06-01
type: concept
tags: [telegram, linux, obsidian, flow]
---

## 相關頁面
- [[concepts/telegram-interactive-ui|Telegram 互動式 UI]]
- [[concepts/obsidian-wiki-conventions|Obsidian Wiki 使用規範]]


# Telegram 檔案傳送標準規範 (Crucial)

## 核心禁忌
- **絕對禁止**使用 `send_message` 工具的 `MEDIA:` 參數傳送任何檔案。
- 經測試，該參數在目前環境下極不穩定，且導致多次傳送失敗 (錯誤次數 > 10次)。

## 正確執行路徑
當使用者要求「傳送給我」或「檔案傳送給我」時，必須執行以下路徑：
1. **工具選擇**: 使用 `telegram-message-file-sender` 技能。
2. **執行方式**: 直接調用 `curl` 請求 Telegram Bot API 的 `sendDocument` 或 `sendPhoto` 接口。
3. **驗證標準**: 必須確認 API 返回 `{"ok":true}` 且獲取 `message_id`。

## 觸發條件
- 關鍵字：『傳送給我』、『檔案傳送給我』。
- 情境：所有需要將 PDF, MD, CSV, PNG 等檔案交付至 Telegram 的場景。

## 更新紀錄
- 2026-05-31: 確定為最高優先級規範，記錄於記憶庫與 Obsidian 以防止 Agent 迴歸舊錯誤。
