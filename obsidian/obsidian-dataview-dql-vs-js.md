---
title: "Obsidian Dataview DQL vs JavaScript Queries"
description: "Dataview 的 DQL 與 dataviewjs 兩種查詢模式優缺點比較"
summary: "Dataview DQL（宣告式）與 JavaScript Queries（程式式）的差異、適用場景與取捨"
type: concept
status: active
tags:
  - workflow
  - pkm
created: 2026-07-10
updated: 2026-07-10
---

# Obsidian Dataview：DQL vs JavaScript Queries

> 來源：Hermes Agent 針對 portfolio-dashboard 表格渲染問題的實務比較（2026-07-10）。

Dataview 提供兩種查詢語法：
- **DQL**：`dataview` 區塊，宣告式查詢語言
- **JavaScript Queries**：`dataviewjs` 區塊，需於設定啟用 «Enable JavaScript Queries»

---

## DQL 版本（`dataview` 區塊）

### 優點
- ✅ 不需額外設定：安裝 Dataview 即可用
- ✅ 語法簡潔：宣告式，幾行就出表格，易讀易維護
- ✅ 安全性高：沙箱限制，不能執行任意程式碼
- ✅ 渲染穩定：Dataview 核心功能，跨版本相容性好
- ✅ 基本計算夠用：`round()`、`*`、`/`、`+`、`-` 都能做

### 缺點
- ❌ 不支援自訂聚合：無法在表格底部加小計列
- ❌ 跨行運算受限：不能做 `reduce` 總計、多步驟暫存變數
- ❌ 格式化弱：`toLocaleString()` 千分位、字串拼接、`if/else` 條件分支都很難或做不到
- ❌ 複雜邏輯寫不出來：例如「美股股數小數、台股整數」的差異處理在 DQL 裡只能靠 `round(shares,2)` 硬湊，無法根據 `currency` 動態決定小數位數

---

## JavaScript Queries 版本（`dataviewjs` 區塊）

### 優點
- ✅ 完全程式能力：`map` / `reduce` / `filter` / `sort` 任意組合
- ✅ 表格內建小計列：`rows.push(["小計", ...])` 直接加在表格底部
- ✅ 任意格式化：千分位、動態小數位、條件上色、自訂字串都行
- ✅ 跨幣別/多資料集處理：台股 TWD、美股 USD 可分別計算再合併
- ✅ 最適合 portfolio 場景（逐筆計算 + 小計 + 精確格式化）

### 缺點
- ❌ 需手動開設定：Obsidian → Dataview → 勾 «Enable JavaScript Queries»
- ❌ 安全性較低：能執行任意 JS（自己寫的沒風險，但來源不可信需注意）
- ❌ 除錯較難：報錯訊息不如 DQL 友善
- ❌ 程式碼冗長：比起 DQL 一行，JS 要寫 20+ 行

---

## 針對 portfolio 場景的取捨

| 需求 | DQL | JS |
|------|-----|-----|
| 表格顯示 8 台股 + 10 美股 | ✅ | ✅ |
| 加減乘除（市值/成本/損益）| ✅ | ✅ |
| 金額取整、股數小數 | ✅（round）| ✅ |
| 表格底部小計列 | ❌（只能文字註解）| ✅（原生表格行）|
| 不需改設定 | ✅ | ❌ 要開 JS |

**建議**：不想碰設定 → 維持 DQL 版（功能完整，只差小計不在表格內）；想要表格內小計列美觀效果 → 去設定勾開 JS，改用 `dataviewjs` 版。

## 相關頁面

- [[obsidian/obsidian-webdav-sync|Obsidian WebDAV Sync]]
- [[obsidian/obsidian-index|Obsidian 目錄]]
- [[finance/portfolio/portfolio-dashboard|投資組合儀表板（實際應用範例）]]
