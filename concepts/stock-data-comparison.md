---
title: 股市資料比較與 Agent 架構
summary: "股市資料獲取比較表格 + Agent 使用架構 + 資料分工 + 注意事項"
created: 2026-06-23
updated: 2026-06-23
type: concept
tags: [taiwan-stock, data-source, agent-architecture, comparison]
---

# 股市資料比較與 Agent 架構

> 本頁面為 [[stock-data-sources|股市資料來源]] 的分支，專注於資料獲取比較、Agent 使用架構與資料分工。

---

## 資料獲取注意事項

1. CSV 資料編碼需改成 UTF-8-BOM，EXCEL 開啟才不會亂碼
2. 資料延遲：三大法人資料通常在收盤後 16:00~17:00 更新
3. 假日無資料：週末及國定假日無交易，API 會回傳空資料，需告知 agent 獲取前一個交易日資料
4. 請求頻率：建議每次請求間隔 100ms 以上，避免被封鎖
5. CORS 問題：瀏覽器直接請求會遇到跨域問題，需透過 Proxy

---

## GitHub 網路金融工具應用

目前網路上查的到 10 個案例：
- [taiwan-stock-market](https://github.com/topics/taiwan-stock-market?l=python)
- tw_stock100、tw_stocker、Taiwan-Stocks、fmd
- Sending-Current-Stock-Prices-With-LINE
- TW-Stock-Google-Trends-Analysis
- Tw_stock_crawer、TWStock-Screener
- open-market-intelligence、V8-Automated-Quant-Trading
- factor-investing-ml-taiwan

---

## Agent 技能說明

- 資料源：**TWSE OpenAPI**
  - 適用場景：上市股日價、財報、除權息
  - 流量管理：**批次下載**（每日 1 次），使用 `User-Agent` 頭
- 資料源：**FinMind**
  - 適用場景：法人、融資融券、月營收
  - 流量管理：**限流**（免費版 ~100 req/hr，註冊 TOKEN 版 ~600 req/hr），請求間隔需 > 6s
- 資料源：**yfinance (OpenBB)**
  - 適用場景：美股、ETF、海外數據
  - 流量管理：**指數退避**（Exponential Backoff）、隨機延遲（0.5s–2s）、模擬瀏覽器 Header
- 資料源：**OpenBB（核心介面）**
  - 適用場景：統一介面查詢、即時報價
  - 流量管理：**隊列化處理**，大規模任務須寫入 `/root/Documents/stock_patch_queue.csv`
  - 分批執行 cron 定時任務，分別於每日 14:00、14:30、15:00 執行，自動將 1,600+ 檔上市標的均分為三個批次進行資料庫更新

---

## Agent 使用架構

```
TWSE OpenAPI
    ↓
本地 SQLite
    ↑
FinMind (補資料)
    ↑
yfinance (海外市場)
    ↑
OpenBB (統一介面)
```

---

## 股市資料獲取比較表格

| 優先 | 資料來源 | 台股 | ETF | 財報 | 月營收 | 官方資料 | 免費 | 穩定性 | 推薦度 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | [TWSE OpenAPI](https://openapi.twse.com.tw/) | ✓ | ✓ | 部分 | ✗ | ✓ | ✓ | 極高 | ★★★★★ |
| 2 | [TPEX OpenAPI](https://www.tpex.org.tw/web/api/doc.php?l=zh-tw) | ✓ | ✓ | 部分 | ✗ | ✓ | ✓ | 極高 | ★★★★★ |
| 3 | [FinMind](https://finmindtrade.com/analysis/) | ✓ | ✓ | ✓ | ✓ | 半官方 | ✓ | 高 | ★★★★★ |
| 4 | [OpenBB Platform](https://openbb.co/) | ✓ | ✓ | ✓ | ✓ | 聚合平台 | 部分 | 高 | ★★★★★ |
| 5 | [yfinance](https://github.com/ranaroussi/yfinance) | 部分 | ✓ | 部分 | ✗ | ✗ | ✓ | 中 | ★★★★ |
| 6 | [Apify](https://apify.com/) | 間接 | 間接 | 間接 | 間接 | ✗ | 部分 | 中 | ★★★★ |
| 7 | [Financial Modeling Prep](https://financialmodelingprep.com/) | ✗ | ✗ | ✓ | ✗ | ✗ | 部分 | 高 | ★★★★ |
| 8 | [Finnhub](https://finnhub.io/) | 部分 | 部分 | ✓ | ✗ | ✗ | ✓ | 中 | ★★★ |
| 9 | [Alpha Vantage](https://www.alphavantage.co/) | ✗ | ✗ | 部分 | ✗ | ✗ | ✓ | 中 | ★★★ |

---

## 資料分工

| 資料 | 來源 |
| --- | --- |
| 台股股價 | TWSE |
| OTC 股價 | TPEX |
| 月營收 | FinMind |
| 財報 | FinMind |
| 股利 | FinMind |
| 法人 | FinMind |
| ETF | TWSE |
| 美股 ETF | yfinance |
| 美國財報 | FMP |
| 新聞 | Finnhub |
| 宏觀經濟 | OpenBB |

---

## 參考網站

- [Get Stock Information | obsidian 檔案咖啡豆版](https://obsidian.vip/zh/plugins/get-stock-information)
- [我用 Claude Code + Obsidian + BearBull.io 搞了一個自動化的股票研究資料庫](https://www.reddit.com/r/ObsidianMD/comments/1rg1q4a/i_built_an_automated_equity_research_vault_using/?tl=zh-hant)
- [StonkJournal – #1 Free Trading Journal](https://stonkjournal.com/)
- [免費！Notion 股票追蹤 & 投資管理範本分享](https://vocus.cc/article/683fda78fd897800013963d8)

---

## 相關頁面
- [[stock-data-sources|股市資料來源]] — API 來源 + 手動獲取來源
- [[concepts/concepts-index|概念筆記索引]]
