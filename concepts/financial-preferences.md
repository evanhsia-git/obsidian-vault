---
name: financial-preferences
description: 用戶針對金融分析的特定偏好與品質標準。
category: preferences
title: Financial-Preferences
- [[openrouter-free-models]]
summary: Financial-Preferences：相關頁面
created: 2026-05-31
updated: 2026-05-31
type: concept
tags: []
---

## 相關頁面
- - [[concepts/stock-automation-config|股票自動化配置]]

# 金融分析偏好規範 (Financial Preferences)

## 1. 報告風格
- **核心模式**：文本主導 → 數據佐證。
- **視覺要求**：
    - 僅在深度分析模式下使用局部量化圖表。
    - 必須移除所有 Emoji 符號，改用專業文本標記（如 `[高風險]`）。
    - 優先使用繁體中文，僅在專業術語 (DCF, WACC, SGE) 保留英文。
- **交付方式**：提到「傳送給我」時，優先使用 Telegram 傳送檔案而非直接顯示。

## 2. 數據與工具偏好
- **優先來源**：TWSE OpenAPI → TPEX → FinMind → OpenBB。
- **資料庫傾向**：傾向於將分析結果結構化並存入 SQLite / DuckDB，以支持後續 RAG 引用。
- **更新邏輯**：執行任務後發現新方法或 Pitfall 時，必須自動更新對應 skill。

## 3. 關鍵指標關注點
- **台股**：極其關注 ROE、淨利率、負債比。
- **美股**：關注 AI 基礎設施資本支出 (CapEx) 與 Azure/Cloud 增速。
- **宏觀**：關注聯準會利率方向與 CPI 通膨數據。