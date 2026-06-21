---
name: stock-automation-config
description: 股市相關定時任務 (Cron Jobs) 配置與輸出規範。
category: automation
title: Stock-Automation-Config
summary: Stock-Automation-Config：相關頁面
created: 2026-05-31
updated: 2026-05-31
type: concept
tags: []
---

## 相關頁面
- [[concepts/twse-api-mapping|TWSE API 映射]]
- [[concepts/stock-database-state|股票資料庫狀態]]


# 股市自動化配置 (2026-05-28)

## 1. 定時任務總覽 (Cron Jobs)

| 任務名稱 | job_id | 時間 (台北) | 內容 |
| :--- | :--- | :--- | :--- |
| 📊 每日股市指標 | `e62ca9f193fd` | 08:30 | VIX, CNN Fear&Greed, VOO, 加權, 0050, 日經225, KOSPI, FRED 利率/CPI/GDP, USD/TWD |
| 🇺🇸 美股新聞 | `a40f503afdf8` | 08:30 | 10 則美股新聞 |
| 🇹🇼 台股新聞 | `a43bac586a89` | 08:30 | 10 則台股新聞 |
| 🤖 AI/科技新聞 | `3f49f2990_e06` | 12:00, 18:00 | 5-8 篇 AI/科技熱門文章 |
| 📝 任務摘要Wiki | `c06af70a4011` | 08:00 | 24h 任務總結 → 本地 Wiki |
| 🔄 Update Check | `ed5d30a18e08` | 每 3 天 | Hermes Agent 版本檢查 |

## 2. 輸出規範 (Output Rules)
- **格式要求**：每條新聞附 `[連結](URL)`，連結文字固定為「連結」，不列出來源網站名稱。
- **過濾規則**：嚴禁輸出 Agent 的內部工作語言 (例如 "I have...", "Now let me...")。
- **沉默規則**：若無有效內容，直接輸出 `[SILENT]`。

## 3. 優先級路徑
- **數據抓取優先級**：TWSE OpenAPI → TPEX → FinMind → OpenBB。