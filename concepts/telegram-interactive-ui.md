---
status: active
title: "Telegram Interactive Ui"
summary: "Telegram Interactive Ui：相關頁面"
created: 2026-05-31
updated: 2026-05-31
type: concept
tags: [deploy]
---

## 相關頁面
- [[concepts/telegram-file-delivery-standard|Telegram 檔案傳送規範]]
- [[concepts/obsidian-wiki-conventions|Obsidian Wiki 使用規範]]


# Telegram 互動界面指南 (Telegram Interactive UI Guide)

## 🌟 核心概念：行內按鈕與即時更新

Telegram 提供了強大的 **Inline Keyboards (行內按鈕)** 機制，允許機器人在不發送新消息的情況下，創造出類似 App 的互動體驗。

### 1. 行內按鈕 (Inline Keyboards)
- **定義**：附著在消息下方的按鈕，每個按鈕包含 `text` (顯示文字) 和 `callback_data` (後台識別碼)。
- **運作流程**：
    1. 機器人發送帶按鈕的消息。
    2. 用戶點擊按鈕 → Telegram 伺服器發送 `CallbackQuery` 給機器人伺服器。
    3. 機器人根據 `callback_data` 執行對應邏輯。

### 2. 即時更新 (On-the-fly Updating)
這是提升用戶體驗的關鍵技術，允許機器人直接修改已發送的消息。
- **核心 API**：`editMessageText` / `editMessageReplyMarkup`。
- **應用場景**：
    - **狀態切換**：將按鈕從 `[分析中...]` → `[查看結果]`。
    - **分頁導航**：點擊 `[下一頁]` → 直接更新當前消息的文字與按鈕，而非刷屏。
    - **確認流程**：`[確定刪除]` → `[已刪除]`。

---

## 🛠️ 在 Hermes Agent 中的實現路徑

由於 Hermes Agent 採取「請求-回應」模式，要實現上述功能需要建立一個 **互動橋接系統**。

### 技術方案
- **發送層 (The Sender)**：
    - 調用 `sendMessage` API → 附加 `InlineKeyboardMarkup`。
    - 調用 `editMessageText` API → 實現即時狀態更新。
- **監聽層 (The Listener)**：
    - 在後台啟動持久化進程 (例如使用 `python-telegram-bot` 的 `Application` 類)。
    - 監聽 `callback_query_handler`。
    - 將捕捉到的按鈕點擊事件轉化為 **文字指令** → 注入回 LLM 對話上下文。

### ⚠️ 實現挑戰
- **異步同步化**：必須確保後台監聽器捕捉到的點擊事件能準確對應到目前的會話 Session。
- **資源消耗**：需要維持一個長期運行的 Python 進程來維持對 Telegram 伺服器的連接。

---

## 🚀 實踐指南：從選單到 App 化

1. **基礎級：指令快捷單**
    - 使用行內程式碼格式 \`/command\`，讓用戶通過「點擊-複製-傳送」模擬按鈕。
2. **進階級：狀態更新 UI**
    - 實現 `editMessageText` → 讓 LLM 在分析長時間任務時，能即時告知用戶進度。
3. **終極級：全互動橋接**
    - 建立完整的 `CallbackQuery` → `LLM Prompt` 閉環，實現真正的按鈕驅動型對話。

---
*Last Updated: 2026-05-28*
*Reference: https://core.telegram.org/bots#inline-keyboards-and-on-the-fly-updating*