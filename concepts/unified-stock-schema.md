---
title: 統一股票資料欄位 Schema
summary: 統一股票資料欄位 Schema：1. 統一 Schema 對照表
created: 2026-06-04
updated: 2026-06-04
type: concept
tags: [data-source, stock-analysis, maintenance]
---

# 統一股票資料欄位 Schema

為確保系統資料庫 (SQLite) 與各資料來源 (TWSE, TPEx, FinMind, yfinance, OpenBB) 運作順暢，所有資料入庫前必須映射為以下統一 Schema。

## 1. 統一 Schema 對照表

| 統一欄位名稱 | 型別 | 描述 | 對應來源建議 |
| :--- | :--- | :--- | :--- |
| `stock_id` | `VARCHAR` | 股票代號 | TWSE/TPEx |
| `date` | `DATE` | 交易日期 (YYYY-MM-DD) | 所有來源 |
| `open` | `FLOAT` | 開盤價 | 所有來源 |
| `high` | `FLOAT` | 最高價 | 所有來源 |
| `low` | `FLOAT` | 最低價 | 所有來源 |
| `close` | `FLOAT` | 收盤價 | 所有來源 |
| `volume` | `BIGINT` | 成交股數 | 所有來源 |
| `amount` | `BIGINT` | 成交金額 | TWSE/TPEx |
| `pe_ratio` | `FLOAT` | 本益比 | TWSE/FinMind |
| `pb_ratio` | `FLOAT` | 股價淨值比 | TWSE/FinMind |
| `dividend_yield` | `FLOAT` | 殖利率 (%) | TWSE/FinMind |
| `eps` | `FLOAT` | 每股盈餘 (元) | FinMind |
| `roe` | `FLOAT` | 股東權益報酬率 (%) | FinMind |

## 2. 轉換規範 (Standardized Logic)
1. **命名規範**：統一使用 `snake_case`，確保與 Pandas/SQLite 欄位索引對齊。
2. **缺值處理 (Handling N/A)**：
   - 數值欄位：無資料填入 `NULL` (SQLite) 或 `None` (Python)。
   - 不建議預設填充 `-1`，以利後續統計聚合 (AVG/SUM) 時避開空值。
3. **日期格式**：所有日期強制轉換為 `YYYY-MM-DD` 字串，並在資料庫內存為 ISO 格式。
4. **編碼對齊**：
   - TWSE/TPEx：優先解析 `UTF-8`；若遇舊 CSV，強制轉換 `utf-8-sig`。
   - 資料處理：所有來源數據進入 SQLite 前需執行 `float()` 型別轉換，並過濾 `-` 或 `空字串`。

## 3. 資料獲取來源優先級 (Source Hierarchy)
- **台股市場**：官方 OpenAPI (TWSE/TPEx) → FinMind (補齊財報/法人指標)。
- **國際/美股/ETF**：OpenBB Platform → yfinance (作為穩定備援)。

## 4. 維護記錄
- 2026-06-04 | 建立統一 Schema 定義檔。

---
## 相關節點
- [[stock-data-sources]]
- [[quant-python-ai-agent]]