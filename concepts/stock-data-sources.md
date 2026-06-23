---
title: 股市資料來源
summary: "台灣股市資料 API 來源與取得方式整理"
created: 2026-06-10
updated: 2026-06-23
type: concept
tags: [taiwan-stock, data-source, twse, tpex, finmind]
---

# 股市資料來源

> 比較表格、Agent 架構、資料分工詳見 [[stock-data-comparison|股市資料比較與 Agent 架構]]。

---

## API 獲取

> - [臺灣證券交易所 OpenAPI](https://openapi.twse.com.tw/)
>   - 數據：公司治理 / 證券交易 / 財務報表 / 指數權證 / 其他券商資料
> - [證券櫃檯買賣中心 OpenAPI](https://www.tpex.org.tw/openapi/)
>   - 數據：上櫃 / 指數系列 / 公司治理 / 債券 / 興櫃 / 權證 / 開放式基金 / 黃金現貨 / 創櫃 / 券商資料 / 財務報表

---

## 上市股票：網站手動獲取完整 CSV

> - [TWSE 臺灣證券交易所](https://www.twse.com.tw/zh/trading/historical/bwibbu-day.html)
>   - 數據：全台股市場上市公司股票代號、名稱、收盤價、殖利率、本益比（依日期查詢）
> - [政府資料開放平台](https://data.gov.tw/dataset/11547)（EXCEL 開啟時編碼要改 UTF-8-BOM）
>   - 數據：日期、股票代號、股票名稱、本益比、殖利率(%)、股價淨值比
> - [公開資訊觀測站](https://mops.twse.com.tw/mops/#/web/t163sb07)（手動查詢）
>   - 數據：營業收入淨額、營業成本、營業毛利、毛利率(%)
> - [公開資訊觀測站](https://mops.twse.com.tw/mops/#/web/t51sb02)（手動查詢）
>   - 數據：財務結構、償債能力、經營能力、獲利能力、現金流量
>   - 註：本報表每年 4 月 1 日更新

---

## 上櫃 / 興櫃股票：網站手動獲取完整 CSV

> - [政府資料開放平台 上市公司基本資料](https://data.gov.tw/dataset/18419)
> - [政府資料開放平台 上櫃股票本益比、殖利率、股價淨值比](https://data.gov.tw/dataset/11373)
> - [政府資料開放平台 上櫃股票行情](https://data.gov.tw/dataset/11370)
>   - 數據：資料日期、代號、名稱、收盤、漲跌、開盤、最高、最低、均價、成交股數、成交金額、成交筆數等
> - [公開資訊觀測站 ETF](https://mopsfin.twse.com.tw/opendata/t187ap47_L.csv)
> - [公開資訊觀測站 上市公司資料](https://mopsfin.twse.com.tw/opendata/t187ap03_L.csv)
> - [公開資訊觀測站 上櫃公司資料](https://mopsfin.twse.com.tw/opendata/t187ap03_O.csv)
> - [公開資訊觀測站 興櫃公司資料](https://mopsfin.twse.com.tw/opendata/t187ap03_R.csv)
> - [政府資料開放平台 興櫃股票比較_NEW](https://data.gov.tw/dataset/104192)
> - [政府資料開放平台 興櫃股票比較年報](https://data.gov.tw/dataset/130035)

---

## 其他資訊

> - [臺灣證券交易所-基本市況報導網站](https://mis.twse.com.tw/stock/index?lang=zhHant)
>   - 大盤資訊、現貨櫃股行情、期貨商品行情、借券查詢
> - [TIP 臺灣指數公司](https://taiwanindex.com.tw/)
>   - ETF、各項指數、指數比較、績效表現
> - [金融市場統計資訊系統-金管會](https://stat.fsc.gov.tw/FSCChartShow_Restore/CRPages/MS_Chart_Show.aspx)
> - [政府資料開放平台 股票市場統計](https://data.gov.tw/dataset/10804)
>   - 1987 年 5 月起股票交易與股價指數（月/年資料）
> - [政府資料開放平台 上市股票比較_NEW](https://data.gov.tw/dataset/104039)

---

## 相關頁面
- [[stock-data-comparison|股市資料比較與 Agent 架構]] — 比較表格 + Agent 架構 + 資料分工
- [[concepts/concepts-index|概念筆記索引]]
