---
status: active
title: "全體市場公開發行公司彙總表"
summary: "全體市場公開發行公司彙總表：資料來源"
created: 2026-06-02
updated: 2026-06-02
type: entity
tags: [tw-stock, obsidian, source]
---

# 全體市場公開發行公司彙總表

## 資料來源
- **提供機關**：台灣證券交易所 (TWSE)
- **連結網站**：[TWSE 公司財務資訊揭露網站 (MOPS)](https://mops.twse.com.tw/mops/#/web/home)
- **資料類型**：整合性公司基本資料
- **涵蓋範圍**：全體市場 (上市、上櫃、興櫃、已脫市)

## 資料欄位 (推測)
| 欄位 | 說明 |
|------|------|
| 公司代號 | 證券代號 |
| 公司名稱 | 完整公司名稱 |
| 公司簡稱 | 公司別名 |
| 產業別 | 產業分類 |
| 上市日期 | 公司上市日期 |
| 市場別 | 上市場、上櫃、興櫃 |
| 狀態 | 交易中、已脫市 |

## 資料下載方式

- [[otc-company-profile-2026-06-02]]
- [[finlab]]

- **TWSE Open Data 專區**：請參考 [TWSE Open Data](https://openapi.twse.com.tw/) 瀏覽相關 API 端點。
- **MOPS 網站**：登入 MOPS 會員後，可於「資料下載」處下載相關彙總表。
- **自動化下載**：可使用 `twse-stock-data` 技能中的 OpenBB 或 TWSE OpenAPI 端點獲取數據。

## 使用說明
- 適合進行全面性的股票名單同步與清潔。
- 建議搭配 `tpex-data-fetch` 技能，補足上櫃與興櫃資料。

## 參考連結
- [TWSE OpenAPI 文件](https://openapi.twse.com.tw/v1/swagger.json)
- [MOPS 首頁](https://mops.twse.com.tw/mops/#/web/home)
- [TWSE 資料開放專區](https://data.twse.com.tw/)