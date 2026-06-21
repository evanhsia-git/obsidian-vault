---
title: 股市資料來源
created: 2026-06-10
updated: 2026-06-20
type: concept
tags: [taiwan-stock, data-source, twse, tpex, finmind]
summary: 台灣股市資料 API 來源與取得方式整理
---

## 目錄

## • 股市資料來源網站

**API 獲取**

> > 臺灣證券交易所 OpenAPI(網站)  
> > 數據: 公司治理 / 證券交易 / 財務報表 / 指數權證 / 其他券商資料  
> > [https://openapi.twse.com.tw/](https://openapi.twse.com.tw/)
> > 
> > 證券櫃檯買賣中心 OpenAPI  
> > 數據: 上櫃 / 指數系列 / 公司治理 / 債券 / 興櫃 / 權證 / 開放式基金 / 黃金現貨 / 創櫃 / 券商資料 / 財務報表  
> > [https://www.tpex.org.tw/openapi/](https://www.tpex.org.tw/openapi/)

**上市股票,網站手動獲取完整csv檔案,再提供給agent**

> > TWSE 臺灣證券交易所  
> > 數據: 全台股市場上市公司股票代號,名稱,收盤價,殖利率,本益比（依日期查詢）  
> > [https://www.twse.com.tw/zh/trading/historical/bwibbu-day.html](https://www.twse.com.tw/zh/trading/historical/bwibbu-day.html)
> > 
> > 政府資料開放平臺 ( EXCEL開啓時,編碼要改 UTF-8-BOM )  
> > 數據: 日期,股票代號,股票名稱,本益比,殖利率(%),股價淨值比  
> > [https://data.gov.tw/dataset/11547](https://data.gov.tw/dataset/11547)
> > 
> > 公開資訊觀測站 (手動查詢)  
> > 個別公司: 營業收入淨額,營業成本,營業毛利,毛利率(%)  
> > 數據: 毛利率彙總表 查詢條件: (年度) 市場別: (上市,上櫃,興櫃,公開發行)  
> > [https://mops.twse.com.tw/mops/#/web/t163sb07](https://mops.twse.com.tw/mops/#/web/t163sb07)
> > 
> > 公開資訊觀測站 (手動查詢)  
> > 個別公司: 營業收入淨額,營業成本,營業毛利,毛利率(%)  
> > 數據: 財務結構,償債能力,經營能力,獲利能力,現金流量  
> > 註: 本報表每年 4 月 1 日更新  
> > [https://mops.twse.com.tw/mops/#/web/t51sb02](https://mops.twse.com.tw/mops/#/web/t51sb02)

**上櫃 / 興櫃股票,網站手動獲取完整csv檔案,再提供給agent**

> > 政府資料開放平臺 上市公司基本資料  
> > 出表日期,公司代號,公司名稱,公司簡稱,外國企業註冊地國,產業別,住址,營利事業統一編號,董事長,總經理,發言人,發言人職稱,代理發言人,總機電話,成立日期,上市日期,普通股每股面額,實收資本額,私募股數,特別股,編制財務報表類型,股票過戶機構,過戶電話,過戶地址,簽證會計師事務所,簽證會計師1,簽證會計師2,英文簡稱,英文通訊地址,傳真機號碼,電子郵件信箱,網址,已發行普通股數或TDR原股發行股數  
> > [https://data.gov.tw/dataset/18419](https://data.gov.tw/dataset/18419)
> > 
> > 政府資料開放平臺 上櫃股票個股本益比、殖利率、股價淨值比  
> > 資料日期、股票代號、名稱、本益比、每股股利、殖利率、股價淨值比  
> > [https://data.gov.tw/dataset/11373](https://data.gov.tw/dataset/11373)
> > 
> > 政府資料開放平臺 上櫃股票每股市值、本益比、週轉率之比較\_NEW  
> > 年月、每股市值元、每股盈餘元、每股淨值\_元、本益比、周轉率、比率、公告日期  
> > [https://data.gov.tw/dataset/104041](https://data.gov.tw/dataset/104041)
> > 
> > 政府資料開放平臺 上櫃股票行情,提供每一檔上櫃有價證券(包含股票、ETF、ETN)每日在收盤後的成交資訊(櫃買中心)  
> > 資料日期,代號,名稱,收盤,漲跌,開盤,最高,最低,均價,成交股數,成交金額,成交筆數,最後買價,最後賣價,發行股數,次日參考價,次日漲停價,次日跌停價  
> > [https://data.gov.tw/dataset/11370](https://data.gov.tw/dataset/11370)
> > 
> > 公開資訊觀測站 上市公司資料  
> > 出表日期,公司代號,公司名稱,公司簡稱,外國企業註冊地國,產業別,住址,營利事業統一編號,董事長,總經理,發言人,發言人職稱,代理發言人,總機電話,成立日期,上市日期,普通股每股面額,實收資本額,私募股數,特別股,編制財務報表類型,股票過戶機構,過戶電話,過戶地址,簽證會計師事務所,簽證會計師1,簽證會計師2,英文簡稱,英文通訊地址,傳真機號碼,電子郵件信箱,網址,已發行普通股數或TDR原股發行股數  
> > [https://mopsfin.twse.com.tw/opendata/t187ap03\_L.csv](https://mopsfin.twse.com.tw/opendata/t187ap03_L.csv)
> > 
> > 公開資訊觀測站 ETF  

- [[quant-python-ai-agent]]
- [[stock-analysis-workflow-full]]

> > 出表日期,基金代號,基金簡稱,基金類型,基金中文名稱,基金英文名稱,標的指數/追蹤指數名稱,標的指數是否為客製化或需揭露相關資訊之指數,股票及債券投資比例說明,是否設有績效指標,績效指標中文名稱,績效指標英文名稱,是否包含國外成分股,基金統一編號,成立日期,上市日期,基金經理人,經理公司總機,經理公司地址,經理公司董事長,經理公司發言人,經理公司總經理,經理公司代理發言人,總代理人,發行單位數/轉換數,保管機構,保管機構電話,保管機構地址,備註  
> > [https://mopsfin.twse.com.tw/opendata/t187ap47\_L.csv](https://mopsfin.twse.com.tw/opendata/t187ap47_L.csv)
> > 
> > 公開資訊觀測站 上櫃公司資料  
> > 出表日期,公司代號,公司名稱,公司簡稱,外國企業註冊地國,產業別,住址,營利事業統一編號,董事長,總經理,發言人,發言人職稱,代理發言人,總機電話,成立日期,上櫃日期,普通股每股面額,實收資本額,私募股數,特別股,編制財務報表類型,股票過戶機構,過戶電話,過戶地址,簽證會計師事務所,簽證會計師1,簽證會計師2,英文簡稱,英文通訊地址,傳真機號碼,電子郵件信箱,網址,已發行普通股數或TDR原股發行股數  
> > [https://mopsfin.twse.com.tw/opendata/t187ap03\_O.csv](https://mopsfin.twse.com.tw/opendata/t187ap03_O.csv)
> > 
> > 公開資訊觀測站 興櫃公司資料  
> > 出表日期,公司代號,公司名稱,公司簡稱,外國企業註冊地國,產業別,住址,營利事業統一編號,董事長,總經理,發言人,發言人職稱,代理發言人,總機電話,成立日期,上市日期,普通股每股面額,實收資本額,私募股數,特別股,編制財務報表類型,股票過戶機構,過戶電話,過戶地址,簽證會計師事務所,簽證會計師1,簽證會計師2,英文簡稱,英文通訊地址,傳真機號碼,電子郵件信箱,網址,已發行普通股數或TDR原股發行股數  
> > [https://mopsfin.twse.com.tw/opendata/t187ap03\_R.csv](https://mopsfin.twse.com.tw/opendata/t187ap03_R.csv)
> > 
> > 政府資料開放平臺  
> > 興櫃股票每股市值、本益比、週轉率之比較\_NEW  
> > 年月、每股市值元、每股盈餘元、每股淨值\_元、本益比、周轉率、市值佔GDP、比率、公告日期  
> > [https://data.gov.tw/dataset/104192](https://data.gov.tw/dataset/104192)
> > 
> > 政府資料開放平臺  
> > 興櫃股票每股市值、本益比、週轉率之比較(年報)  
> > 年月、每股市值元、每股盈餘元、每股淨值\_元、本益比、周轉率、市值佔GDP、比率、公告日期  
> > [https://data.gov.tw/dataset/130035](https://data.gov.tw/dataset/130035)

**其他資訊**

> > 臺灣證券交易所-基本市況報導網站  
> > 大盤資訊,現貨櫃股行情,期貨商品行情,借券查詢  
> > 上櫃市場,興櫃市場,期貨市場,走趨圖,集中市場ETF,集中市場ETN  
> > [https://mis.twse.com.tw/stock/index?lang=zhHant](https://mis.twse.com.tw/stock/index?lang=zhHant)
> > 
> > TIP 臺灣指數公司  
> > ETF,各項指數,指數比較,績效表現,市值型,指數化投資報告,成分股審核,指數編製規則及相關檔案,  
> > SmartBeta投資型 槓桿/反向型 股市期權策略 多資產  
> > [https://taiwanindex.com.tw/](https://taiwanindex.com.tw/)
> > 
> > 金融市場統計資訊系統-金管會  
> > [https://stat.fsc.gov.tw/FSCChartShow\_Restore/CRPages/MS\_Chart\_Show.aspx](https://stat.fsc.gov.tw/FSCChartShow_Restore/CRPages/MS_Chart_Show.aspx)
> > 
> > 政府資料開放平臺 股票市場統計-股票交易與股價指數  
> > 1987年5月起股票交易與股價指數(月資料)、1987年起股票交易與股價指數(年資料)  
> > 數據: 2026M04,上市股票-公司家數1074,加權平均股價指數36644.34  
> > [https://data.gov.tw/dataset/10804](https://data.gov.tw/dataset/10804)
> > 
> > 政府資料開放平臺  
> > 數據: 上市股票每股市值、本益比、週轉率之比較\_NEW  
> > [https://data.gov.tw/dataset/104039](https://data.gov.tw/dataset/104039)

• 資料獲取注意事項

1. CSV資料編碼需改成 UTF-8-BOM,EXCEL開啓才不會亂碼
2. 資料延遲：三大法人資料通常在收盤後 16:00~17:00 更新
3. 假日無資料：週末及國定假日無交易，API 會回傳空資料，需告知agent獲取前一個交易日資料
4. 請求頻率：建議每次請求間隔 100ms 以上，避免被封鎖
5. CORS 問題：瀏覽器直接請求會遇到跨域問題，需透過 Proxy

## • github 網路金融工具應用

目前網路上查的到 10個案例  
taiwan-stock-market  
[https://github.com/topics/taiwan-stock-market?l=python](https://github.com/topics/taiwan-stock-market?l=python)  
  
tw\_stock100  
tw\_stocker  
Taiwan-Stocks  
fmd  
Sending-Current-Stock-Prices-With-LINE  
TW-Stock-Google-Trends-Analysis  
Tw\_stock\_crawer  
TWStock-Screener  
open-market-intelligence  
V8-Automated-Quant-Trading  
factor-investing-ml-taiwan

• 我自己的 agent skill 說明

• 資料源: **TWSE OpenAPI**  
• 適用場景: 上市股日價、財報、除權息  
• 流量管理 (Rate Limit)  
策略: **批次下載** (每日 1 次)，使用 `User-Agent` 頭。  
• 資料源: **FinMind**  
• 適用場景: 法人、融資融券、月營收  
• 流量管理 (Rate Limit) 策略:  
**限流** (免費版 ~100 req/hr，註冊TOKEN 版 ~600 req/hr)，請求間隔需 > 6s。  
• 資料源: **yfinance (OpenBB)**  
• 適用場景: 美股、ETF、海外數據  
• 流量管理 (Rate Limit)  
策略: **指數退避** (Exponential Backoff)、隨機延遲 (0.5s–2s)、模擬瀏覽器 Header。  
• 資料源: **OpenBB (核心介面)**  
• 適用場景: 統一介面查詢、即時報價  
• 流量管理 (Rate Limit) 策略: **隊列化處理** ，大規模任務須寫入 `/root/Documents/stock_patch_queue.csv` 。  
• 分批執行 cron 定時任務,分別於每日 14:00、14:30、15:00 執行，自動將 1,600+ 檔上市標的均分為三個批次進行資料庫更新。

• Agent 使用架構

> > TWSE OpenAPI  
> > ↓  
> > 本地SQLite  
> > ↑  
> > FinMind (補資料)  
> > ↑  
> > yfinance (海外市場)  
> > ↑  
> > OpenBB (統一介面)

• 股市資料獲取比較表格

| 優先 | 資料來源 | 台股 | ETF | 財報 | 月營收 | 官方資料 | 免費 | 穩定性 | 推薦度 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | [TWSE OpenAPI](https://openapi.twse.com.tw/?utm_source=chatgpt.com) | ✓ | ✓ | 部分 | ✗ | ✓ | ✓ | 極高 | ★★★★★ |
| 2 | [TPEX OpenAPI](https://www.tpex.org.tw/web/api/doc.php?l=zh-tw&utm_source=chatgpt.com) | ✓ | ✓ | 部分 | ✗ | ✓ | ✓ | 極高 | ★★★★★ |
| 3 | [FinMind](https://finmindtrade.com/analysis/?utm_source=chatgpt.com#/data/api-documentation) | ✓ | ✓ | ✓ | ✓ | 半官方 | ✓ | 高 | ★★★★★ |
| 4 | [OpenBB Platform](https://openbb.co/?utm_source=chatgpt.com) | ✓ | ✓ | ✓ | ✓ | 聚合平台 | 部分 | 高 | ★★★★★ |
| 5 | [yfinance](https://github.com/ranaroussi/yfinance?utm_source=chatgpt.com) | 部分 | ✓ | 部分 | ✗ | ✗ | ✓ | 中 | ★★★★ |
| 6 | [Apify](https://apify.com/?utm_source=chatgpt.com) | 間接 | 間接 | 間接 | 間接 | ✗ | 部分 | 中 | ★★★★ |
| 7 | [Financial Modeling Prep](https://financialmodelingprep.com/?utm_source=chatgpt.com) | ✗ | ✗ | ✓ | ✗ | ✗ | 部分 | 高 | ★★★★ |
| 8 | [Finnhub](https://finnhub.io/?utm_source=chatgpt.com) | 部分 | 部分 | ✓ | ✗ | ✗ | ✓ | 中 | ★★★ |
| 9 | [Alpha Vantage](https://www.alphavantage.co/?utm_source=chatgpt.com) | ✗ | ✗ | 部分 | ✗ | ✗ | ✓ | 中 | ★★★ |

• 資料分工

| 資料 | 來源 |
| --- | --- |
| 台股股價 | TWSE |
| OTC股價 | TPEX |
| 月營收 | FinMind |
| 財報 | FinMind |
| 股利 | FinMind |
| 法人 | FinMind |
| ETF | TWSE |
| 美股ETF | yfinance |
| 美國財報 | FMP |
| 新聞 | Finnhub |
| 宏觀經濟 | OpenBB |

## 參考網站

## 個人筆記-待閱讀網站

[Get Stock Information | obsidian檔案咖啡豆版](https://obsidian.vip/zh/plugins/get-stock-information)  
[我用 Claude Code + Obsidian + BearBull.io 搞了一個自動化的股票研究資料庫 – 這是 300 多份公司筆記的樣子](https://www.reddit.com/r/ObsidianMD/comments/1rg1q4a/i_built_an_automated_equity_research_vault_using/?tl=zh-hant)  
[StonkJournal – #1 Free Trading Journal](https://stonkjournal.com/)  
[免費！Notion股票追蹤 & 投資管理範本分享：自動更新股價！圖表化投資績效！](https://vocus.cc/article/683fda78fd897800013963d8)