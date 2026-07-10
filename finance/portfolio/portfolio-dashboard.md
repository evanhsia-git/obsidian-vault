---
title: "投資組合儀表板"
description: "台股 + 美股庫存持倉儀表板 — 對帳單表格 + Dataview 自動統計"
summary: "台股 8 支 + 美股 10 支：對帳單 + Agent 可更新的持倉頁自動計算"
type: index
status: active
tags:
  - stock
  - finance
created: 2026-07-10
updated: 2026-07-10
---

# 投資組合儀表板（Portfolio Dashboard）

> 資料來源：永豐金證券 2026年6月份證券電子對帳單（證券庫存表）
> 幣別：臺幣。截至 2026-06-30 庫存餘額。參考市價為收盤價，未實現損益僅供參考。

---

## 一、證券庫存（2026-06 對帳單）

| 交易別    | 證券     | 名稱        | 庫存餘額       | 平均成本價格   | 總投資成本       | 參考市價     | 參考市値          | 未實現投資損益(不含息) | 未實現報酬率%(不含息) | 累計配息          | 未實現投資損益(含息)    | 未實現報酬率%(含息) |
| ------ | ------ | --------- | ---------- | -------- | ----------- | -------- | ------------- | ------------ | ------------ | ------------- | -------------- | ----------- |
| 現股     | 006208 | 富邦台50     | 1,000      | 119.39   | 119,387     | 250.20   | 250,200       | 130,813      | 109.57%      | 3,469.00      | 134,282.00     | 112.48%     |
| 現股     | 009800 | 中信NASDAQ  | 10,000     | 9.54     | 95,400      | 13.25    | 132,500       | 37,100       | 38.89%       | 0.00          | 37,100.00      | 38.89%      |
| 現股     | 009816 | 凱基台灣TOP50 | 30,000     | 10.31    | 309,300     | 15.65    | 469,500       | 160,200      | 51.79%       | 0.00          | 160,200.00     | 51.79%      |
| 現股     | 00981A | 主動統一台股增長  | 4,048      | 17.88    | 72,388      | 31.28    | 126,621       | 54,233       | 74.92%       | 3,731.00      | 57,964.00      | 80.07%      |
| 現股     | 00988A | 主動統一全球創新  | 1,861      | 15.04    | 27,981      | 22.28    | 41,463        | 13,482       | 48.18%       | 0.00          | 13,482.00      | 48.18%      |
| 現股     | 2330   | 台積電       | 53         | 779.98   | 41,339      | 2,410.00 | 127,730       | 86,391       | 208.98%      | 2,196.00      | 88,587.00      | 214.29%     |
| 現股     | 2454   | 聯發科       | 49         | 1,018.10 | 49,887      | 4,245.00 | 208,005       | 158,118      | 316.95%      | 5,247.00      | 163,365.00     | 327.47%     |
| 現股     | 2885   | 元大金       | 2,020      | 28.11    | 56,790      | 65.70    | 132,714       | 75,924       | 133.69%      | 3,796.00      | 79,720.00      | 140.38%     |
| **小計** | —      | —         | **49,031** | —        | **772,472** | —        | **1,488,733** | **716,261**  | **92.72%**   | **18,439.00** | **734,700.00** | **95.11%**  |

---

## 一之一、美股庫存（2026-06 複委託對帳單，USD）

| 商品     | 名稱                             | 持有股數       | 總投資成本(USD) | 收盤價      | 參考市值(USD)  | 投資損益(USD)  | 報酬率%       |
| ------ | ------------------------------ | ---------- | ---------- | -------- | ---------- | ---------- | ---------- |
| AMGN   | Amgen                          | 3.22       | 801        | 362.12   | 1,165      | 363        | 45.33%     |
| BAC    | Bank of America                | 27.37      | 801        | 56.98    | 1,560      | 759        | 94.66%     |
| BRK.B  | Berkshire Hathaway C           | 4.32       | 2,100      | 500.39   | 2,160      | 60         | 2.84%      |
| CAT    | Caterpillar                    | 2.98       | 801        | 1,064.90 | 3,178      | 2,376      | 296.51%    |
| IGV    | iShares Expanded Tech-Software | 19.68      | 2,100      | 90.60    | 1,783      | -317       | -15.10%    |
| NLR    | VanEck Uranium and Nuclear     | 12.32      | 1,700      | 115.98   | 1,429      | -271       | -15.96%    |
| QQQ    | Invesco QQQ Trust              | 6.08       | 2,704      | 736.40   | 4,474      | 1,771      | 65.49%     |
| SMH    | VanEck Semiconductor           | 27.21      | 9,003      | 655.89   | 17,846     | 8,843      | 98.23%     |
| VTI    | Vanguard Total Stock Market    | 10.47      | 2,704      | 370.04   | 3,873      | 1,169      | 43.23%     |
| VUG    | Vanguard Growth ETF            | 5.89       | 500        | 86.14    | 507        | 7          | 1.47%      |
| **小計** | —                              | **120.53** | **22,010** | —        | **37,665** | **15,655** | **71.13%** |

> 美股為碎股（定期定額），股數為小數；金額欄已取整數（USD），與對帳單原始值四捨五入一致。

---

## 二、即時持倉計算（Agent 更新後自動重算）

> 各股票持倉頁：`[[finance/portfolio/tw/hold-006208]]` `[[finance/portfolio/tw/hold-009800]]` `[[finance/portfolio/tw/hold-009816]]` `[[finance/portfolio/tw/hold-00981A]]` `[[finance/portfolio/tw/hold-00988A]]` `[[finance/portfolio/tw/hold-2330]]` `[[finance/portfolio/tw/hold-2454]]` `[[finance/portfolio/tw/hold-2885]]`
> Agent 修改各頁 frontmatter 的 `current_price`（收盤價）與 `shares`（股數）後，下表自動更新。

```dataview
TABLE
  stock_name AS "名稱",
  round(shares) AS "股數",
  round(avg_cost) AS "成本價",
  round(current_price) AS "收盤價",
  round(current_price * shares) AS "市值",
  round(avg_cost * shares) AS "成本",
  round((current_price - avg_cost) * shares) AS "損益",
  (round((current_price - avg_cost) / avg_cost * 100, 2)) + "%" AS "報酬率%"
FROM "finance/portfolio/tw"
WHERE type = "resource" AND stock_id
SORT ((current_price - avg_cost) * shares) DESC
```

> 小計（台股）：總股數 49,031 股｜總市值 NT$ 1,488,733｜總成本 NT$ 772,465｜總損益 NT$ 716,268（92.72%）

---

---

## 三、美股即時持倉計算（USD，金額取整、股數保留小數）

> 各美股持倉頁：`[[finance/portfolio/us/hold-us-AMGN]]` `[[finance/portfolio/us/hold-us-BAC]]` `[[finance/portfolio/us/hold-us-BRK.B]]` `[[finance/portfolio/us/hold-us-CAT]]` `[[finance/portfolio/us/hold-us-IGV]]` `[[finance/portfolio/us/hold-us-NLR]]` `[[finance/portfolio/us/hold-us-QQQ]]` `[[finance/portfolio/us/hold-us-SMH]]` `[[finance/portfolio/us/hold-us-VTI]]` `[[finance/portfolio/us/hold-us-VUG]]`
> 美股為碎股（shares 小數），僅「市值/成本/損益」取整數，股數與報酬率% 維持小數。

```dataview
TABLE
  stock_name AS "名稱",
  round(shares, 2) AS "股數",
  round(avg_cost) AS "成本價",
  round(current_price) AS "收盤價",
  round(current_price * shares) AS "市值(USD)",
  round(avg_cost * shares) AS "成本(USD)",
  round((current_price - avg_cost) * shares) AS "損益(USD)",
  (round((current_price - avg_cost) / avg_cost * 100, 2)) + "%" AS "報酬率%"
FROM "finance/portfolio/us"
WHERE type = "resource" AND stock_id
SORT ((current_price - avg_cost) * shares) DESC
```

> 小計（美股）：總股數 120.53 股｜總市值 USD 37,665｜總成本 USD 22,010｜總損益 USD 15,655（71.13%）

---

---

## 三、總計彙總

**台股（TWD）**
- 總股數：49,031 股
- 總成本：NT$ 772,465
- 總市值：NT$ 1,488,733
- 總損益：NT$ 716,268（92.72%）

**美股（USD）**
- 總股數：120.53 股
- 總成本：USD 22,010
- 總市值：USD 37,665
- 總損益：USD 15,655（71.13%）

---

## 相關頁面

- [[finance/finance-index]]
- [[finance/investment-strategy|投資策略]]
