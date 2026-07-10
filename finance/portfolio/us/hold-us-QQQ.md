---
title: "美股持倉 QQQ Invesco QQQ Trust"
description: "Invesco QQQ Trust (QQQ) 美股持倉記錄"
summary: "QQQ Invesco QQQ Trust — 股數與收盤價由 Agent 更新，損益自動計算（USD）"
type: resource
status: active
tags:
  - stock
  - finance
created: 2026-07-10
updated: 2026-07-10
stock_id: "QQQ"
stock_name: "Invesco QQQ Trust"
avg_cost: 445.04
current_price: 736.40
shares: 6.07609
---

# 美股持倉 QQQ Invesco QQQ Trust

Agent 協助維護的持倉頁（美股碎股，shares 為小數）。更新方式：修改 frontmatter 的 `current_price`（收盤價）與 `shares`（股數），`portfolio-dashboard.md` 的美股 Dataview 自動重算損益。

## 欄位說明

| 欄位 | 意義 | 來源 |
|------|------|------|
| `avg_cost` | 平均成本價格（USD） | 對帳單 |
| `current_price` | 收盤價（USD） | Agent 每日更新 |
| `shares` | 持有股數（碎股小數） | Agent 買賣後更新 |
| `currency` | 計價幣別 | USD |

## 相關頁面

- [[finance/portfolio/portfolio-dashboard|投資組合儀表板]]
- [[finance/finance-index]]
