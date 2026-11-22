---
title: "市場資料來源總表"
description: "mklab-stock 使用的全部市場資料來源：台股（TWSE／MOPS／TPEx）、期權（TAIFEX）、集保股務（TDCC）、國際（yfinance）、ETF 持股（發行人 API／TickerInside）、美國財報（SEC EDGAR），含自動獲取方式與實測狀態"
summary: "市場資料來源總表：TWSE／MOPS／TPEx／TAIFEX／TDCC／yfinance／ETF 持股來源鏈／TickerInside／SEC EDGAR，附實測可用性與自動化現況"
type: resource
status: active
tags: [api, data, etf, finance]
created: 2026-11-03
updated: 2026-11-22
sources: ["https://openapi.twse.com.tw/v1/swagger.json", "https://mopsov.twse.com.tw/nas/t21/sii/t21sc03_115_9_0.html", "https://www.sec.gov/search-filings/edgar-application-programming-interfaces", "https://tickerinside.com/", "https://openapi.taifex.com.tw/swagger.json", "https://openapi-t.tdcc.com.tw/swagger-ui/index.html"]
related: ["resources/twse-openapi-full.md", "resources/twse-openapi-endpoints.md"]
---

# 市場資料來源總表

mklab-stock（GitHub-Native 股市儀表板）使用的全部資料來源與自動獲取方式。「實測」欄位皆為真實請求結果並標註實測日期（首次全面實測 2026-11-03，其後新增來源見各節標註）。

## 一、總覽

| 資料類型 | 來源 | 性質 | 使用現況 |
|---|---|---|---|
| 台股每日收盤／PE／殖利率 | TWSE OpenAPI、TPEx OpenAPI | 官方、免 key | 自動（`daily` job） |
| 台股官方財報（損益、資產負債） | MOPS opendata（`t187ap06_L_*`、`t187ap07_L_*`） | 官方、免 key | 自動（`official-fundamentals` job，每月 20 日） |
| 台股公司基本資料／產業別 | TWSE `t187ap03_L` | 官方 | 自動（產業別 2 位碼，24 = 半導體業） |
| 股利／股東會 | TWSE `t187ap45_L` | 官方 | 自動 |
| ETF 基金資料 | TWSE `t187ap47_L` | 官方 | 自動 |
| ROE／ROA 補充（無官方財報者） | yfinance | 第三方 | 自動（`weekly-roe` job，週六） |
| 全球指數／ETF／總經、美股前 30 大 | yfinance | 第三方 | 自動（`indices`、`us-market`、`global-history`） |
| 四大指數替代 ETF 持股與權重 | 發行人 API ＋ TickerInside ＋ stockanalysis | 混合 | 自動（`index-holdings` job，每月 2 日） |
| 全市場 ETF 每日淨值／收盤／折溢價 | TWSE MIS 反向 API `mis.twse.com.tw/stock/data/all_etf.txt` | 官方、免 key | **已實測，導入中**（見第二節末） |
| 台股 ETF 發行人官方持股（國泰系） | 國泰 `cwapi.cathaysite.com.tw` Excel API | 官方、免 key | **已實測，已導入**（見第二節末） |
| 期貨／選擇權行情與籌碼（三大法人期權、P/C 比、大額交易人、保證金） | 期交所 OpenAPI `openapi.taifex.com.tw`（135 端點） | 官方、免 key | **規格已解析；資料端點待驗證**（見第二節末） |
| 集保股務與籌碼（股權分散表、董監分戶、ETF 月分析） | 集保 OpenAPI `openapi.tdcc.com.tw`（134 端點） | 官方、免 key | **已實測，未導入**（見第二節末） |
| 主動式 ETF（A 尾）官方持股（13 家投信直出，含股數＋受益人數） | 各投信官網 API／PCF（破解地圖見 skill `taiwan-etf-holdings/references/issuer-sources.md` 附錄 A，知識來源 nctuwanglin/active-etf） | 官方、免 key | **已實測並導入**（每日工作流＋歷史累積；`fetch_etf_holdings_active.py`） |
| 美國財報（營收／淨利／EPS／資產負債／現金流／10-K／10-Q） | SEC EDGAR XBRL API | 官方、免 key | **尚未接入**（見第六節） |

## 二、台股來源

- **TWSE OpenAPI**：`https://openapi.twse.com.tw/v1/opendata/<端點>`，143 個端點、2,015 個欄位，詳見 [[resources/twse-openapi-full|TWSE OpenAPI 端點完整清單]]。
- **MOPS opendata**：`https://mopsfin.twse.com.tw/opendata/<檔名>.csv`，財報按產業拆檔（`ci` 一般業、`basi`、`bd` 銀行、`fh` 金控、`ins` 保險、`mim`），UTF-8 含 BOM（用 `utf-8-sig` 讀）。
- **TPEx OpenAPI**：上櫃資料（含標準 ETF 收盤行情），發布時差與 TWSE 不同：TPEx 為 T-0、TWSE 為 T-1／T-2 → 不可標成同一日期（已改為每筆自帶 `price_asof`）。
- **MOPS nas 月營收 HTML**：`https://mopsov.twse.com.tw/nas/t21/{sii|otc}/t21sc03_{民國年}_{月}_0.html`
  （民國 115 年 9 月 → `t21sc03_115_9_0.html`）。**2026-11-14 實測：仍可存取且為當期資料**——
  sii／otc 皆 HTTP 200（456 KB／397 KB）、出表日期 `115/11/14`、資料年月民國 115 年 9 月。
  **編碼是 Big5（CP950）**（用 UTF-8 解析會整段亂碼），回傳是 HTML 表格（含產業分組與合計列），需自行解析。
  適用時機：**回溯歷史月份**、或需要**含上櫃公司**時。
- **月營收建議走 OpenAPI（比 HTML 好抓）**：`t187ap05_L`「上市公司每月營業收入彙總表」——
  JSON、免解析 HTML、**一檔請求即全市場 1,077 筆**，欄位含當月／上月／去年同月營收、月增率、年增率、累計營收。
  **目前尚未導入**（實測有資料、列為候選）。
- **尚未導入但可用的候選**：`t187ap14_L`（各產業 EPS，1,014 筆）、`t187ap17_L`（營益分析：官方毛利率／營益率，1,000 筆）、
  `MI_MARGN`（融資融券，1,309 筆）、`holidaySchedule`（開休市日，29 筆）、`TWTAWU`（暫停交易證券）等共 36 個。
  完整對照見 [[resources/twse-openapi-endpoints|TWSE OpenAPI 端點總覽]] 的「2026-11-14 全量複測」，
  以及 repo `data/raw/twse-openapi-usage-map.md`、網頁 `mklab-stock-sources.html` 第 12 節。
- **ETF 補充資料（規模／受益人數／發行人／標的指數）**：2026-11-15 起改為**官方自動化**（每週日 18:00）：
  - TWSE：`https://wwwc.twse.com.tw/zh/ETFortune/etfExcel?...`（篩選頁的 CSV 匯出端點）
    → HTTP 200、`text/csv;charset=ms950`、約 248 列
  - TPEx：`https://info.tpex.org.tw/api/etfFilter`（**篩選頁背後的資料 API**；
    頁面上的 `blob:` 下載是瀏覽器即時產生，無法直接抓）→ JSON、118 檔
  - 以 `skills/data/fetch_etf_meta.py` 抓取（TWSE 重用既有 `parse_etf_meta` 解析器），
    失敗時依序退回人工 CSV（`data/raw/twse_etf.csv`／`tpex_etf.csv`）與上次結果；
    `meta.provenance` 會標示每個市場是 `auto`／`manual`／`previous`
  - 同一 job 也會跑 `classify_etf.py`（ETF 分類）與 `compute_indicators.py`，
    讓新上市 ETF 不會停在 `security_type=unverified`
  - ⚠️ 兩個網站的使用條款記載禁止自動化擷取（原 `parse_etf_meta.py` 檔頭的判斷）；
    Ivan 2026-11-15 已知悉並決定採官方端點、低頻（每週）自動化，並保留人工備援。
    若要停止，移除 `daily-update.yml` 的 `etf-meta` job 即可。

### ETF 淨值／折溢價與成份股（2026-11-22 實測，解析 dj8111/taiwan-etf-holdings 後驗證）

- **TWSE MIS 反向 API（全市場 ETF 淨值／折溢價）**：`https://mis.twse.com.tw/stock/data/all_etf.txt`
  - 單一請求回全市場 ETF 最新一筆：代號 `a`、名稱 `b`、資料日期 `i`（YYYYMMDD）、每單位淨值 `f`、
    收盤市價 `e`、折溢價% `g`、前日淨值 `h`。免 key，需帶 `Referer: https://mis.twse.com.tw/stock/etf_nav.jsp`。
  - **實測 2026-11-22 ✅**：一個請求回 359 檔；0050 NAV=84.68、市價=84.45、折溢價=−0.27%、日期 20261120。
  - ⚠️ 內容是「最新一筆」而非歷史下載 → 歷史要靠每日盤後排程累積，以 `etf_code + record_date` 去重
    （非交易日重跑只會拿到同一 record_date，不會重複計入）。
  - 狀態：已實測，導入中（每日工作流新步驟 + `data/etf-nav*.json`；畫面加「折溢價」欄）。
- **國泰投信官方持股 Excel API**：`https://cwapi.cathaysite.com.tw/api/ETF/DownloadETFWeightExcel?FundCode={內部碼}&SearchDate={YYYY/MM/DD}`
  - 官方直出**完整持股**（代號／名稱／股數／權重）＋基金淨資產價值、在外流通單位數、**每單位淨值**。
  - 需內部 FundCode 對照表：`00878=CN` 已驗證；其餘國泰 ETF 的內部碼可從官網 Angular bundle 的
    `api/ETF/GetETFList` 端點（cwapi.cathaysite.com.tw）建立對照。
  - **實測 2026-11-22 ✅**：00878（CN）回 56 列 xlsx，含 2026/11/20 持股權重、每單位淨值 NT$27.98。
  - 狀態：已實測，待整合進 etf-holdings 來源鏈（富邦／元大官方之後加國泰官方）。
- **MoneyDJ 持股明細頁（Basic0007a）已失效**：2026-11-22 實測該頁為 JS 空殼
  （「持股／股票名稱／資料日期」關鍵字 0 命中），純 HTTP 解析拿不到，需瀏覽器渲染。
  同站 **Basic0004 費用頁仍是 SSR** 可解析（兩頁實作不同，勿以一頁狀態推論另一頁）。
- 參考實作：GitHub `dj8111/taiwan-etf-holdings`（record_date 歷史累積、換股 diff、跨 ETF 個股追蹤的設計值得學；
  其 SQLite 36MB 直接 commit 進 git 的做法不採納，與本 repo 單檔預算原則衝突）。

### 期貨／選擇權與集保股務（2026-11-22 實測）

- **集保結算所 OpenAPI（TDCC）**：`https://openapi.tdcc.com.tw/v1/opendata/<編號>`（正式）；`openapi-t.tdcc.com.tw` 為測試環境，**同路徑同資料皆開放**。
  - 規格：UI `https://openapi.tdcc.com.tw/swagger-ui/index.html`、JSON 規格 `/tdcc-opendata-api-docs`（66 KB OpenAPI 3.0.1）。
    **134 個端點**八大類：股務資訊（1-x）、權益證券統計（2-x）、TAIBIR（2-14~2-21）、固定收益（2-x）、境外基金（3-x）、境外結構型商品（4-x）、期信基金（5-x）、股東e票通（6-x）。
  - 亮點端點（籌碼／ETF 相關）：
    - `1-5` **集保戶股權分散表**：全市場逐檔、16 級持股分級（1 張以下→1000 張以上）之人數／股數／占集保庫存比例（T-1）——散戶/大戶輪動的經典籌碼來源。
    - `1-4` 發行人董監分戶保管（董監持股）；`1-1` 證券基本資料（含股務單位、市場別、證券狀態——**單表 49.5 MB**，ETL 需 stream 篩選）。
    - `2-41` 集中保管 ETF 月分析表：逐檔 ETF 集保戶數、本／前月底保管數、月增減（受益人結構月度趨勢，資料年月 202610）。
    - `2-22`~`2-29` 個別股票異動月分析／上市櫃保管有價證券週餘額表。
  - ⚠️ 陷阱（實測）：回應 JSON **欄位名是中文**，且首欄名帶 **U+FEFF BOM 前綴**（如 `"\ufeff證券代號"`），取欄位要 strip；`1-1` 單表 49 MB 大小差異大，勿假設每端點都很小。
  - 狀態：**已實測（1-5、2-41 皆 200 + 正常 JSON），未導入**。免 key。
- **期貨交易所 OpenAPI（TAIFEX）**：`https://openapi.taifex.com.tw`
  - 規格：UI 即根路徑（Swagger UI），JSON 規格 `https://openapi.taifex.com.tw/swagger.json`（160 KB，OAS 1.0.0）。
    **135 個端點**，亮點：`DailyMarketReportFut`／`Opt`（期貨／選擇權每日行情）、三大法人系列（依日期／週別 × 總表／期貨選擇權分計／各契約／買賣權分計）、`PutCallRatio`（臺指選擇權 P/C 比）、大額交易人未沖銷部位、保證金一覽表（含股票類／**ETF 類**）、`FinalSettlementPrice` 最後結算價、可抵繳標的（含股票含 ETF）。
  - ⚠️ **實測 2026-11-22：spec 取得成功，但資料端點（`/PutCallRatio`、`/DailyMarketReportFut` 及大小寫／尾斜線變體）一律回 Swagger UI 殼（200 + text/html 1777B）**——疑似限台灣 IP 或非服務時段（本 VPS 為境外資料中心 IP；集保同 IP 卻可通，故非普遍封鎖）。待台灣網路環境再驗；spec 本身可信、端點清單已解析。
  - 狀態：**規格已解析，資料存取待驗證**。

## 三、四大指數替代 ETF 持股（`index-holdings` job）

每月更新一次（`freq=monthly`，抓到後 25 天內自動跳過），來源依序嘗試，產出於 `data/index-holdings/*.json`：

| ETF | 主要來源 | 備援（依序） | 2026-11-03 實測產出 |
|---|---|---|---|
| VOO（S&P 500） | Vanguard 官方 JSON `investor.vanguard.com/irr/funds/profile/VOO-AdditionalFundData` | 手動檔 → TickerInside | 501 檔、98.71%、基準日 2026-09-30 |
| DIA（道瓊） | SSGA 官方 XLSX（檔名必須含 `-en-`） | TickerInside → stockanalysis | 30 檔、99.95% |
| QQQ（那斯達克100） | Invesco 公開 API `dng-api.invesco.com/cache/v1/accounts/en_US/shareclasses/46090E103/holdings/fund?idType=cusip&productType=ETF` | TickerInside → stockanalysis | 101 檔、99.84%、基準日當日 |
| SOXX（費半） | 官方 iShares 下載檔（人工放入 `data/raw/official/SOXX_holdings.xls`，45 天期限） | TickerInside → stockanalysis → iShares URL（僅參考） | 30 檔、99.83%、基準日 2026-11-02 |

### 各發行人的實測狀態

- **Vanguard**：`{TICKER}-AdditionalFundData` 回傳 JSON（約 420KB），持股在 `holdingDetails.equityHoldings`，基準日 `holdingDetails.asOfDate`（每月公布）。同格式可用於 VTI／VUG／VTV／BND（尚未納入自動流程）。
- **SSGA（DIA）**：官方 XLSX 可用（`holdings-daily-us-en-dia.xlsx`，每日公布）；無 `-en-` 的舊網址為 404。
- **Invesco（QQQ）**：官方持股**頁面**對非瀏覽器一律 406、且表格由 JS 載入；同名 `action=download` 雖回 200 但 Content-Type 是 `text/html`（415KB 網頁），**不是 CSV、不能用 `pandas.read_csv()`**。真正可用的是網站背後的公開 API `dng-api.invesco.com`（免 key、免瀏覽器）。
- **iShares／BlackRock（SOXX）**：`latest-holdings.csv`、舊式 `.ajax?fileType=csv`、產品頁、BlackRock 鏡像**全部 403 Access Denied**（Akamai）。實測判斷依據是**來源 IP 屬資料中心**，不是 UA／標頭 —— 帶完整瀏覽器標頭無效，連本機的真實瀏覽器（headless Chrome）也被擋（回應含 `Reference #18.4752cd17…`）；本機 IP 為 `172.104.105.71`（Linode）。**檔案本身沒問題**（含代號、名稱、產業、市值、權重、股數），缺的是能通過 Akamai 的存取方式；GitHub Actions runner 同為資料中心 IP，因此 CI 無法繞過。
  - **替代方案（已實作）**：人工下載後放到 `data/raw/official/SOXX_holdings.xls` → 優先被使用，設 `max_age_days=45`（超過 45 天未更新自動跳過、改用自動來源；檔案不存在也只是跳過）。**2026-11-03 已由 Ivan 提供並放入 repo**。
  - **官方檔案的格式陷阱（實測）**：檔名是 `.xls`，內容其實是 **Excel 2003 XML（SpreadsheetML）** —— `pandas.read_html()` 讀不到（不是 HTML 表格）、`pandas.read_excel()` 回 `Excel file format cannot be determined`；且檔內含**未轉義的 `&`**（官方連結屬性如 `?type=ishares&style=All`），標準 XML 解析器直接 `ParseError`（Excel 自己容忍）。解法：`parse_spreadsheetml()` 用 `xml.etree` 解析並先修掉非實體的 `&`，支援 `ss:Index` 跳欄。
  - **只取股票部位**：官方檔除 30 檔股票外還有 `USD CASH`、`XTSLA`（貨幣市場基金）、`WFFUT`（現金擔保品）、`IXTZ6`（期貨）共 4 列（權重 0.17%），由 `filter_equity_only()` 依 `Asset Class` 欄濾除；`Fund Holdings as of`（Nov 02, 2026）寫入產出的 `holdings_asof`。
  - **自動備援**：TickerInside（完整 30 檔、`source_layer=issuer`、基準日 2026-11-02）在人工檔過期或不存在時接手。

## 四、共同備援：TickerInside

- **網址模板**：`https://tickerinside.com/api/v1/fund/{ticker}.json`（另有 `.csv`：`fund,holdings_as_of,source_layer,ticker,weight_pct`）。
- **格式**：`holdings` 是「代號: 權重%」的**扁平字典**（如 `{"AMD": 9.2357}`），另有 `holdings_asof`、`source_layer`、`cite_as`。
- **覆蓋**：支援大量美國 ETF；四檔指數替代 ETF 皆可用。
- **新鮮度差異大（重點）**：`source_layer=issuer`（發行人當期資料，如 SOXX／DIA）與 `source_layer=sec_nport`（SEC N-PORT 季報，如 VOO 資料日 2026-06-30、QQQ 同）**並存** → 使用時必須讀 `holdings_as_of`，**不要假設每日更新**。
- **使用條款**：要求標註來源（`cite_as` 為建議字串）；mklab-stock 會把它寫進產出檔的 `attribution` 欄位。

## 五、SEC EDGAR（美國財報，尚未接入）

官方文件：<https://www.sec.gov/search-filings/edgar-application-programming-interfaces>

可提供：公司營收（Revenues）、淨利（NetIncomeLoss）、EPS（Basic／Diluted）、資產負債表、現金流量表、季報 10-Q、年報 10-K。

| 端點 | 用途 |
|---|---|
| `https://data.sec.gov/api/xbrl/companyfacts/CIK##########.json` | 一次取回公司所有 XBRL 科目與各期間數字 |
| `https://data.sec.gov/api/xbrl/companyconcept/CIK##########/us-gaap/{tag}.json` | 單一科目歷史序列（如 `us-gaap/Revenues`） |
| `https://data.sec.gov/api/xbrl/frames/us-gaap/{tag}/USD/CY2024.json` | 跨公司同期比較 |
| `https://data.sec.gov/submissions/CIK##########.json` | 申報清單（10-K／10-Q 日期與文件連結） |
| `https://www.sec.gov/cgi-bin/browse-edgar?action=getcompany&CIK={cik}&type=10-K` | 人工查詢頁 |

- **CIK**：10 碼零填充（Apple = `0000320193`；Invesco QQQ Trust = `0001067839`）。

### 存取規範（SEC 官方要求）

| 項目 | 規定 |
|---|---|
| API key | **不需要申請** |
| User-Agent | 必須合規且能識別請求程式與聯絡方式：**程式名稱＋版本＋可聯絡的 Email**。SEC 會在服務使用異常時依此聯絡資料使用者，因此**不接受隨意編造的聯絡信箱** |
| UA 範例 | `MKLAB Stock evan.hsia@gmail.com` |
| 速率 | SEC 規定**每秒不得超過 10 次**；mklab-stock 更保守，**建議控制在每秒 2～3 次** |

```python
headers = {
    "User-Agent": "MKLAB Stock evan.hsia@gmail.com",
    "Accept-Encoding": "gzip, deflate",
    "Host": "data.sec.gov",
}
```

- **實測（2026-11-03）**：自訂 UA（未含 email）**一律 403 Forbidden** —— 與 SEC 規定一致；帶瀏覽器 UA 可通過，但正式接入必須使用合規 UA（`MKLAB Stock evan.hsia@gmail.com`）。
- **狀態**：聯絡 email、端點、格式、CIK、速率規範皆已具備，程式面尚未接入。要啟用需先決定要抓哪些美股標的與存放位置（例如 `data/us/us-fundamentals.json`）與對應 job。

## 六、登記備用（尚未納入自動流程）

- Vanguard：VTI／VUG／VTV／BND（`{TICKER}-AdditionalFundData`，同 VOO 解析器，要用時加一筆設定即可）。
- stockanalysis.com：`https://stockanalysis.com/etf/{ticker}/holdings/`，伺服器端渲染、免 key，但免費層只有前 25 大持股（權重可能僅 71%）。

## 七、維護方式（實際生效的設定在哪裡）

| 檔案 | 內容 |
|---|---|
| `data/index-proxy.json` | 四檔 ETF 的**來源順序**與**逐來源驗證門檻**（`expected_min`／`expected_max`／`min_weight_total`）—— 討論來源時以這裡為準 |
| `data/etf-source-urls.json` | 來源清單、格式、角色、實測狀態（含不可用原因）與 SEC EDGAR 端點 |
| `skills/data/fetch_index_holdings.py` | 解析器：`parse_vanguard`／`parse_invesco`／`parse_tickerinside`（JSON）、`parse_table`（HTML／CSV／XLSX） |
| `data/raw/twse-openapi-full.md` | TWSE OpenAPI 完整清單原文（repo 版） |

設計重點：同一檔 ETF 的不同來源完整度差很多（QQQ 官方 API 101 檔 vs 免費層前 25 大），因此**門檻逐來源覆寫**，避免部分資料靜默通過驗證；產出檔一律記錄 `source`、`holdings_asof`、`attribution`。

## 附錄：來源驗證原則（通用，實測得來）

1. **HTTP 200 不等於成功**：下載後必須同時檢查 `status_code`、`Content-Type` 與 body 開頭（前 100 字元），內容符合預期格式才進解析流程。
   - 實例：Invesco `action=download` 宣稱持股 CSV，HTTP 200 但 Content-Type 是 `text/html`（415KB 網頁），`pandas.read_csv()` 當然解析不了。
   - 已實作：`check_content_type()` 在來源宣告為 CSV 卻回 HTML／JSON 時直接報錯並附開頭內容；所有解析失敗訊息都帶開頭 100 字元。
2. **副檔名不可信**：iShares 的 `.xls` 實際是 Excel 2003 XML（SpreadsheetML），且含未轉義的 `&` —— 必須看內容判斷格式（`parse_spreadsheetml()`）。
3. **403 要先分辨是「標頭問題」還是「IP 問題」**：多組標頭 + 真實瀏覽器都失敗、且主機是資料中心 IP → 屬 IP 型封鎖，換 UA／cookie 都沒用。
4. **人工檔案要有期限**：人工放入的官方檔案（`manual_upload:`）一律設 `max_age_days`，過期自動跳過並讓自動來源接手，避免舊檔長期擋住自動來源。
5. **來源自帶的基準日一定要記錄**：不同來源新鮮度差很多（`issuer` 當期 vs SEC N-PORT 季報差數月），產出檔一律寫入 `holdings_asof`。
6. **部分資料要驗證完整度，而不只是「有沒有資料」**：同一檔基金的不同來源完整度可能天差地遠（QQQ 官方 101 檔 vs 免費層前 25 大），因此**門檻逐來源覆寫**，避免部分資料靜默通過。

## 相關頁面

- [[resources/twse-openapi-full|TWSE OpenAPI 端點完整清單（143 端點／2,015 欄位）]]
- [[resources/twse-openapi-endpoints|TWSE OpenAPI 端點總覽]]
- [[resources/corporate-actions|公司行為登記表]] — 分割／反分割／減資的官方查證管道（TWTBAU1 停牌區間、MOPS 公告鏡像）
- [[resources/resources-index|資源鏈結索引]]
