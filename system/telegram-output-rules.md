---
title: "TELEGRAM-OUTPUT-RULES"
description: "Telegram 輸出格式規範"
type: schema
status: permanent
summary: "Telegram Bot 輸出格式規範（已解除所有限制）"
tags: [telegram, agent, workflow]
created: 2026-06-21
updated: 2026-06-28
---

# Telegram Output Rules

## 結論

Telegram 新版已全面支援標準 Markdown，**解除所有格式限制**。

允許使用：表格、標題、粗體、斜體、代碼塊、列表、巢狀列表、引用、連結、MathJax 等完整 Markdown 語法。

## 唯一保留

- 可使用 `telegram-message-file-sender` 發送檔案
- 可用 `send_message` 的 `MEDIA` 參數發送媒體

---

## 相關節點

* [[schema]]
