# 共用股票表格 (SQLite)

**檔案路徑**：`/root/Documents/database/tw_stock_all.db`  
**資料表名稱**：`stock_overview`

這張表是 mklab‑stock 各頁面、腳本與資料處理腳本（如 `fetch_data.py`、`update_overview.py` 等）共用的「股票基本面」資料來源，所有個股的靜態基本資料都儲存於此。

## 欄位清單（共 14 個欄位）

| 欄位名稱 | 資料型別 | 說明 |
|----------|----------|------|
| **stock_id** | TEXT (PRIMARY KEY) | 股票代號（例如 `2330`、`0050`） |
| **stock_name** | TEXT | 股票中文全名（例如 `台積電`、`元大台灣50`） |
| **industry** | TEXT | 所屬產業別（依照 TWSE/TPEX 分類） |
| **isin** | TEXT | 國際證券辨識號碼（ISIN） |
| **listing_date** | TEXT | 上市／上櫃日期（格式 `YYYY-MM-DD`） |
| **cfi** | TEXT | CFI 碼（國際金融工具分類碼） |
| **roe** | REAL | Return on Equity（股東權益報酬率，%） |
| **gross_margin** | REAL | 毛利率（%） |
| **net_margin** | REAL | 淨利率（%） |
| **debt_ratio** | REAL | 負債比率（%） |
| **eps** | REAL | 每股盈餘（元） |
| **market_cap** | REAL | 市值（新台幣） |
| **shares_outstanding** | REAL | 流通在外股數（股） |
| **roa** | REAL | Return on Assets（資產報酬率，%） |

> **備註**  
> - 若某些基本面資料在來源中不可取得（例如尚未公開的財報），對應欄位會呈現 `NULL`。  
> - 此表格僅保存 **最新** 的快照資料；歷史每日價量與技術指標則另存於同資料庫裡的 `daily_prices_<YYYYMMDD>` 分表（或 `daily_prices` 主表視情況而定）。

---  

## TWSE 與 TPEx 取得的股市資料欄位對照與差異  

以下表格列出兩個主要資料來源（台灣證券交易所 **TWSE** 及 證券櫃檯買賣中心 **TPEx**）在日常抓取的 JSON 中，各欄位的命名與是否皆存在的情況。  
「✓」表示該欄位在該來源中存在，「‑」表示缺失，「≈」表示名稱不同但語意相同。

| 欄位意義 | TWSE 欄位名稱（`stocks.json`） | TPEx 欄位名稱（推斷自 API 文件） | 備註 |
|----------|------------------------------|----------------------------------|------|
| 股票代號 | `sym` | `SecuritiesCode` | ≈ |
| 股票名稱 | `name` | `SecuritiesName` | ≈ |
| 收盤價 | `price` | `ClosingPrice` | ≈ |
| 開盤價 | `open` | `OpeningPrice` | ≈ |
| 最高價 | `high` | `HighestPrice` | ≈ |
| 最低價 | `low` | `LowestPrice` | ≈ |
| 成交量（張） | `volume` | `TradingVolume` | ≈ |
| 本益比 | `pe` | `PE` | ≈ |
| 股價淨值比 | `pb` | `PB` | ≈ |
| 股利殖利率（%） | `div` | `DividendYield` | ≈ |
| 股東權益報酬率（%） | `roe` | `ROE` | ≈ |
| 資產報酬率（%） | `roa` | `ROA` | ≈ |
| 每股盈餘（元） | `eps` | `EPS` | ≈ |
| 資本額（已發行股數 × 面值） | `capital_stock` | `Capital` | ≈ |
| 市值（新台幣） | `market_cap` | `MarketValue` | ≈ |
| 產業別 | `ind` | `IndustryCategory` | ≈ |
| 是否為 ETF | `is_etf` | `IsETF` | ≈ |
| 漲跌幅（%） | `chg` | `ChangePercent` | ≈ |
| 內部排名分數 | `rank` | `Rank` | ≈ |
| 資料來源 | `source` | `DataSource` | ≈ |
| 資料品質 | `quality` | `DataQuality` | ≈ |
| 最後更新日期 | `last_updated` | `UpdateDate` | ≈ |
| 國際證券辨識號碼（ISIN） | **無** | **無** | 兩來源皆不直接提供，需透過其他表格對映 |
| 上市/上櫃日期 | **無** | **無** | 同上 |
| CFI 代碼 | **無** | **無** | 同上 |
| 毛利率、淨利率、負債比率 | **無** | **無** | 財務比率僅在基本面快照中提供，非即時報價 API 所含 |

### 主要差異總結

1. **命名慣例**：  
   - TWSE 多使用簡短的小寫英文單字或縮寫（如 `pe`, `pb`, `div`）。  
   - TPEx 傾向於使用完整的英文單字並以大寫開頭（如 `PE`, `PB`, `DividendYield`），但在實際回傳的 JSON 中常見全小寫或混合，但語意同上。

2. **額外欄位**：  
   - TWSE 的 `stocks.json` 包含 `rank`（內部評分）、`source`、`quality`、`last_updated` 等元資訊，TPEx 通常也有對應欄位但名稱不同（`DataSource`, `DataQuality`, `UpdateDate`）。  
   - 兩者皆提供 `is_etf` 旗標，方便快速區分 ETF 與普通股票。

3. **缺失的基本面財務指標**：  
   - `isin`, `listing_date`, `cfi`, `gross_margin`, `net_margin`, `debt_ratio` 等較為靜態的財務與公司基本資料，**不在即時報價 API 中**，僅會在每日基本面快照（`stocks.json`）或別的基本面資料中出現。  
   - 需要這些欄位，必須參考 `stock_overview`（SQL）或個別基本面 API  W）和 TPEX 的即時報價資料主要聚焦在 **價量與估值指標（PE/PB/DIV、EPS、ROE/ROA）**，而長期基本面（如資本股、上市日期、ISIN、CFI、毛利/淨利/負債比）則需透過其他資料來源（例如財報、公司基本資料表）補充。

---  

*此頁面自動產生於 2026-07-26，依據使用者「比對 twse.tpex. 獲取的股市資料名稱對比差異，寫入同一篇 wiki」的需求更新。*