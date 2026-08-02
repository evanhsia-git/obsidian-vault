--- 
status: active 
title: "搜尋引擎聚合邏輯說明" 
description: "搜尋聚合邏輯實作" 
summary: "搜尋引擎聚合邏輯說明：1. 定義" 
created: 2026-06-03 
updated: 2026-06-03 
type: concept 
tags: [auto, source, rag] 
--- 

# 搜尋引擎聚合邏輯 (Search Engine Aggregation Logic)

## 1. 定義
搜尋引擎聚合邏輯是指將多個不同來源（如搜尋引擎、API、資料庫）的查詢結果進行智慧合併、去重、排序與過濾的技術方法。其目標是利用各來源的優勢，彌補單一來源的不足，產出更完整、準確且無偏見的最終結果。

## 2. 金融數據蒐集中的具體應用
在 `ws-data-gatherer` 中，此邏輯主要用於提升數據可靠度：

- **多源股價數據驗證**：同時從 TWSE OpenAPI、Yahoo Finance、FinMind 抓取同一檔股票的收盤價，取中位數或加權平均值減少單點錯誤。
- **新聞內容去重與補充**：從多個財經媒體抓取同一則新聞，自動去重後合併各來源的補充資訊。
- **基本面指標交叉驗證**：比較不同來源提供的 PE/PB/ROE 數值，當偏差超過閾值時觸發複核機制。
- **市場情緒綜合評分**：結合 PTT、新聞標題的情緒分析，產出穩定的綜合情緒指數。

## 3. 主要好處與優勢

| 優勢 | 說明 |
| :--- | :--- |
| **提升準確性** | 透過多源交叉驗證減少單點錯誤，確保數據精確度。 |
| **擴大覆蓋率** | 結合不同來源特性，彌補單一資料來源的缺失。 |
| **時效性互補** | 整合即時新聞價格與深度財報數據，平衡時效與質量。 |
| **抗風險能力** | 當某來源中斷時，系統可自動切換至其他備援來源。 |
| **去除偏見** | 綜合多家報導，平衡單一媒體的立場影響。 |

## 4. 實作建議
在 `ws-data-gatherer` 中建議採取：

1. **並行請求**：同時向多個來源發送請求。
2. **標準化轉換**：將結果轉換為統一的 JSON 格式。
3. **智慧選擇規則**：例如價格取中位數，基本面數據若偏差 < 5% 則取平均值。
4. **可靠度標註**：為每筆聚合數據標記信賴分數。

---
## 相關節點
- stock-data-sources
- unified-stock-schema

## TWSE API Mapping

---
status: active
description: "台灣證券交易所 (TWSE) OpenAPI 端點映射與抓取規範。"
title: "Twse-Api-Mapping"
summary: "Twse-Api-Mapping：相關頁面"
created: 2026-05-31
updated: 2026-05-31
type: concept
tags: []
---
## 相關頁面
- [[concepts/stock-database-state|股票資料庫狀態]]
- 股票自動化配置


# TWSE API 映射表 (2026-05-28)

## 1. 個股估值與基本面
| 端點名稱 | URL / 請求路徑 | 內容 | 編碼 | 頻率 | 來源 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **BWIBBU_ALL** | `exchangeReport/BWIBBU_ALL?response=open_data` | PE + 殖利率 + PB (1077檔上市) | UTF-8 | 每日 | data.gov.tw 11547 |
| **BWIBBU_d** | `exchangeReport/BWIBBU_d?response=csv&date=YYYYMMDD` | 收盤價 + PE + PB + 殖利率 | Big5 | 每日 | TWSE |
| **STOCK_DAY_AVG_ALL** | `exchangeReport/STOCK_DAY_AVG_ALL?response=open_data` | 收盤價 + 月平均價 (26846檔) | UTF-8 | 最新日 | data.gov.tw 11548 |

## 2. 財務報表 (ROE/EPS 核心)
- **損益表 (Income Statement)**: `t187ap06_L_ci`
- **資產負債表 (Balance Sheet)**: `t187ap07_L_ci`
- **注意**：不同產業需使用不同端點（如 _basi, _fh, _ins, _bd）。
- **核心欄位**：`基本每股盈餘（元）`。
- **計算邏輯**：ROE = 稅後淨利 / 股東權益 (需年化 4 季)。

## 3. ETF 數據
- **端點**: `https://mopsfin.twse.com.tw/opendata/t187ap47_L.csv`
- **內容**: 256 檔 ETF, 29 欄位。
- **編碼**: UTF-8 BOM (utf-8-sig)。
- **更新頻率**: 每月。

## 4. 優先級定義
當多個來源提供相同數據時，優先級如下：
**TWSE OpenAPI → TPEX → FinMind → OpenBB**

## TWSE API 映射表 (2026-05-28)

## 1. 個股估值與基本面
| 端點名稱 | URL / 請求路徑 | 內容 | 編碼 | 頻率 | 來源 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **BWIBBU_ALL** | `exchangeReport/BWIBBU_ALL?response=open_data` | PE + 殖利率 + PB (1077檔上市) | UTF-8 | 每日 | data.gov.tw 11547 |
| **BWIBBU_d** | `exchangeReport/BWIBBU_d?response=csv&date=YYYYMMDD` | 收盤價 + PE + PB + 殖利率 | Big5 | 每日 | TWSE |
| **STOCK_DAY_AVG_ALL** | `exchangeReport/STOCK_DAY_AVG_ALL?response=open_data` | 收盤價 + 月平均價 (26846檔) | UTF-8 | 最新日 | data.gov.tw 11548 |

## 2. 財務報表 (ROE/EPS 核心)
- **損益表 (Income Statement)**: `t187ap06_L_ci`
- **資產負債表 (Balance Sheet)**: `t187ap07_L_ci`
- **注意**：不同產業需使用不同端點（如 _basi, _fh, _ins, _bd）。
- **核心欄位**：`基本每股盈餘（元）`。
- **計算邏輯**：ROE = 稅後淨利 / 股東權益 (需年化 4 季)。

## 3. ETF 數據
- **端點**: `https://mopsfin.twse.com.tw/opendata/t187ap47_L.csv`
- **內容**: 256 檔 ETF, 29 欄位。
- **編碼**: UTF-8 BOM (utf-8-sig)。
- **更新頻率**: 每月。

## 4. 優先級定義
當多個來源提供相同數據時，優先級如下：
**TWSE OpenAPI → TPEX → FinMind → OpenBB**

## TWSEMCPServer 總結與改進建議

### 1. TWSEMCPServer 各資料來源說明
| 資料類別 | 主要來源 | 取得方式 | 特色 |
|----------|----------|----------|------|
| 即時股價 / 盤中報價 | MIS 即時報價 (https://mis.twse.com.tw) | 傳入股票代碼（可一次多檔） → 即時成交、買賣盤、漲跌、張數 | 單一端點可一次取多檔，適合盤面監控；需注意頻率限制（建議 ≤ 1 次/秒） |
| 日K / 月均價 / 歷史收盤 | TWSE Web API – 例如 /exchangeReport/STOCK_DAY、/exchangeReport/MI_INDEX、/exchangeReport/STOCK_MONTH_AVG | 需要 `date=YYYYMMDD`、`stockId`、`response=json`；回傳 JSON 陣列（含日期、開高低收、成交量等） | 支援任意歷史日期（只要當天有交易）；日期多為民國年，需轉西元年（YYY+1911） |
| 月營收 | MOPS Open CSV (https://mopsfin.twse.com.tw/opendata/t187ap05_L.csv) | 直接下載 CSV，使用 `utf-8-sig` 解碼、`csv.DictReader` 讀取 | 一次取得全部上市公司當月營收；建議本地快取（同一天重複呼叫直接讀檔） |
| 三大法人買賣超（個股） | TWSE T86 (https://www.twse.com.tw/rwd/zh/fund/T86) | 必要參數：`date=YYYYMMDD`、`selectType=ALL`、`response=json`；回傳含外資、投信、自營商買賣超及合計 | 已是目前主要來源；回傳日期為民國年，需轉換 |
| 三大法人彙總（市場級） | TWSE Web API – `/fund/TWT44U`、`/fund/TWT38U` 等 | 同上，回傳每日市場總買賣超、各業別外資持股比例等 | 可用來判斷大盤資金流向、產業輪動 |
| 上櫃三大法人 | TPEx OpenAPI – `/tpex_3insti_daily_trading_info`、`/tpex_3insti_monthly_trading_info` | 需要 `date=YYYYMMDD`、`response=json`；欄位命名與 TWSE 略有不同（例如 `foreign_net`、`trust_net`、`dealer_net`） | 與 TWSE 類似，但資料量較少；建議同時抓取兩者並以 `source` 欄位區分 |
| 融資融券餘額 | TWSE MI_MARGN (https://openapi.twse.com.tw/v1/exchangeReport/MI_MARGN) ＋ TPEx 同類端點 | 直接回傳 JSON 陣列，含股票代號、融資今日餘額、融券今日餘額等 | 已在目前腳本使用；回傳日期為西元年，無需額外轉換 |
| 除權除息、股利 | TWSE Web API – `/calendar/STOCK_DAY_ALR`、`/calendar/STOCK_MONTH`、`/calendar/STOCK_DAY` | 傳入日期或月份 → 回傳當天/當月所有除權除息、發放股利的紀錄 | 可直接產出「除息日前後」交易策略所需的行事曆表 |
| 財務報表（資產負債表、損益表、現金流量表） | TWSE OpenAPI – `/financial/statement` 系列（例如 `BALANCE_SHEET`、`INCOME_STATEMENT`、`CASH_FLOW`） | 需要 `companyId`、`year`、`season`、`reportType` 等參數；回傳為 XBRL 或 JSON（視 endpoint 而定） | 可依季度抽取，計算 ROE、ROA、負債率、流動比率等基本面因子 |
| 期貨／選擇權歷史 | TAIFEX 網站下載頁 (https://www.taifex.com.tw) → 網頁爬取 CSV/ZIP，或直接呼叫 TAIFEX OpenAPI（僅近一日） | 因官方 API 只給最新一天，若需要長期歷史則必須爬取下載頁面 | 若未來要納入期貨/選擇權因子，可參照此做法 |

### 2. 與目前 `fetch_chip_data.py` 實作的對比
| 功能 | 目前實作 | 建議改進 |
|------|----------|----------|
| 三大法人個股 | 呼叫 TWSE_INSTITUTIONAL_API (T86) + 僅嘗試一次 TPEX（無版本區分） | - 多版本 TPEX 嘗試 (v1/v2) <br> - 加入重試、標頭（User‑Agent、Referer） <br> - 若當天無資料（週末）自動 fallback 到最近一個交易日 <br> - 使用統一 logging（取代 print） |
| 月營收 | 直接下載 MOPS CSV、`utf‑8-sig` + `csv.DictReader`，逐筆 INSERT | - 加入本地快取（同一天只下載一次） <br> - 使用 executemany（或自訂的 `execute_many`) 一次寫入，減少 DB 鎖定次數 <br> - 加錯誤處理與 logging |
| 注意／處置股 | 呼叫 TWSE / TPEx 公告 API，做 `dict`/`list` 兩種容錯處理，只寫入 `stock_id、status、reason、source` | - 額外擷取公告日期（effective_date）與原因欄位（如「處置事由」或「注意交易資訊」） <br> - 若當天無公告，仍可寫入一筆 `status IS NULL` 的紀錄，方便後續統計「連續幾天無公告」 <br> - 使用統一的 fetch_json（帶重試） |
| 融資融券 | 呼叫 TWSE MI_MARGN + 僅嘗試一次 TPEX（無版本區分） | - 同三大法人：多版本 TPEX 嘗試、加重試與標頭 <br> - 若兩來源皆失敗，可回退到前一天的收盤值（已在腳本中保留） <br> - 計算 `margin_usage_pct` 時加入防零除保護（已有） |
| 除權息 / 配息 | 未實作 | - 新增 `fetch_dividends()`、`fetch_rights()` 函式，調用 `/calendar/STOCK_DAY_ALR` 與 `/calendar/STOCK_MONTH` <br> - 存入 `dividends`（`stock_id、ex_date、cash_dividend、stock_dividend`）與 `rights`（`stock_id、ex_date、ratio`）表 <br> - 這兩張表可直接用於「除息日前後」的選股或風險控管 |
| 日K / 月均價 | 未實作 | - 新增 `fetch_daily_price(stock_ids, days)` 使用 `/exchangeReport/STOCK_DAY`（TWSE）與 `/tpex_daily_close_quotes`（TPEx） <br> - 支援批次下載（一次請求多檔股票）或非同步（`aiohttp` + `asyncio.gather`） <br> - 存入 `daily_prices`（`stock_id、trade_date、open、high、low、close、volume`） <br> - 另外可加 `fetch_monthly_avg()` 使用 `/exchangeReport/STOCK_MONTH_AVG` |
| 基本面（財報、EPS、ROE …） | 未實作 | - 新增 `fetch_financial()` 系列，調用 `/financial/statement`（如 `BALANCE_SHEET`、`INCOME_STATEMENT`、`CASH_FLOW`） <br> - 依季度或年度抽取，計算 ROE、ROA、負債率、流動比率、EPS、每股股利等因子，存入 `fundamentals` 表 <br> - 此表可與技術面、籌碼面因子合併做多因子選股 |
| 資料庫設計 | 每個功能對應一張簡單表，多數缺少索引與時間戳 | - 為每張表加上複合主鍵或唯一索引（例如 `PRIMARY KEY (trade_date, stock_id)`） <br> - 加入 `created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP`、`updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP` 方便追蹤資料更新時間 <br> - 若資料量龐大（如日K），可考慮按月分區（`PARTITION BY RANGE (strftime('%Y-%m', trade_date))`） |
| 錯誤處理 & 日誌 | 只用 `print` 輸出簡單警告 | - 使用 Python `logging` 模組（`INFO、WARNING、ERROR`） <br> - 記錄請求 URL、狀態碼、回應前 200 字元（方便除錯） <br> - 失敗計數器：連續 N 次失敗自動發送 Telegram 警報 |
| 非同步/批次 | 逐一請求（serial） | - 對於需要大量請求的情境（日K、基本面、月均價）使用 `aiohttp` + `asyncio.gather` 顯著縮短時間 <br> - 可保留同步版本作為備用，方便在無法安裝 `aiohttp` 的環境下運行 |
| 組織結構 | 單一腳本裡塞全部函式 | - 模組化：`fetchers/`（各資料來源）、`db/`（連線、建表、批次寫入）、`utils/`（請求、重試、日期轉換、logging）、`models/`（資料類別或 ORM 定義） <br> - 這樣方便單元測試、CI/CD 以及未來功能擴充 |

### 3. 具體改進程式碼範例（可直接複製）

#### utils/request.py（帶重試、日誌、標頭）
```python
# -*- coding: utf-8 -*-
"""
統一的 HTTP 請求工具：
- 自動重試（3 次，指數退避）
- 常見 Headers（User-Agent、Referer、Accept）
- 詳細的 logging（請求 URL、狀態碼、回應前 200 字元）
- 失敗時回傳 None，不拋出例外（讓呼叫者決定是否使用 fallback）
"""
import logging
import time
from typing import Any, Dict, Optional

import requests
from requests.adapters import HTTPAdapter
from urllib3.util.retry import Retry

logger = logging.getLogger(__name__)

def _build_session() -> requests.Session:
    sess = requests.Session()
    retry = Retry(
        total=3,
        backoff_factor=1,               # 1s, 2s, 4s …
        status_forcelist=[429, 500, 502, 503, 504],
        allowed_methods=["GET", "POST"],
        raise_on_status=False,
    )
    adapter = HTTPAdapter(max_retries=retry)
    sess.mount("http://", adapter)
    sess.mount("https://", adapter)
    # 以下為多數 TWSE/TPEx 端點常需的 Header
    sess.headers.update({
        "User-Agent": "Mozilla/5.0 (compatible; HermesAgent/1.0)",
        "Accept": "application/json, text/plain, */*",
        "Referer": "https://www.twse.com.tw/",  # 或 https://www.tpex.org.tw/
    })
    return sess

SESSION = _build_session()

def fetch_json(
    url: str,
    *,
    params: Optional[Dict[str, Any]] = None,
    method: str = "GET",
    data: Optional[dict] = None,
    label: str = "",
) -> Optional[dict]:
    """
    取得 JSON 資料（失敗時回傳 None）。
    參數：
        url: 目標端點
        params: 查詢字串（dict）
        method: "GET" 或 "POST"
        data: POST 時的 body（dict）
        label: 用於 log 的標籤（例如 "TWSE 三大法人"）
    回傳值：
        解析後的 Python dict/list；若失敗則回傳 None
    """
    try:
        if method.upper() == "POST":
            resp = SESSION.post(url, params=params, json=data, timeout=15)
        else:
            resp = SESSION.get(url, params=params, timeout=15)

        if resp.status_code != 200:
            logger.warning(
                "[%s] HTTP %s %s – %s | URL: %s",
                label,
                resp.status_code,
                resp.reason,
                resp.url,
            )
            return None

        # 嘗試解析 JSON；即使 Content-Type 不是 application/json 也先試一次
        try:
            return resp.json()
        except ValueError:
            snippet = resp.text[:200].replace("\n", "\\n")
            logger.warning(
                "[%s] 非合法 JSON，前200字元：%s | URL: %s",
                label,
                snippet,
                url,
            )
            return None

    except requests.RequestException as exc:
        logger.error("[%s] 請求發生例外：%s", label, exc)
        return None
```

#### 更新後的 fetch_institutional（示範）
```python
# fetchers/institutional.py
import sqlite3
from datetime import datetime, timedelta
from .utils.request import fetch_json, roc_to_western
from .db import execute_many  # 假設你已封裝好的 executemany 包裝函式

TWSE_INST = "https://www.twse.com.tw/rwd/zh/fund/T86"
TPEX_INST = "https://www.tpex.org.tw/openapi/v1/tpex_3insti_daily_trading_info"

def _prev_trading_day(date_str: str) -> str:
    """回傳比 date_str (YYYY-MM-DD) 早一個交易日的字串（簡易版：只跳過週末）"""
    dt = datetime.strptime(date_str, "%Y-%m-%d")
    while True:
        dt -= timedelta(days=1)
        if dt.weekday() < 5:  # 0~4 為週一~五
            return dt.strftime("%Y-%m-%d")
    # 若要考慮國定假日，可在此加入假日表判斷

def fetch_institutional(conn: sqlite3.Connection, trade_date: str | None = None) -> int:
    """
    抓取三大法人個股買賣超並寫入 institutional_trading。
    回傳實際寫入的筆數（失敗則回 0）。
    """
    """
    trade_date = trade_date or datetime.now().strftime("%Y-%m-%d")
    total_inserted = 0

    # ---------- TWSE ----------
    data = fetch_json(
        TWSE_INST,
        params={"date": trade_date.replace("-", ""), "selectType": "ALL", "response": "json"},
        label="TWSE 三大法人",
    )
    if data and isinstance(data.get("data"), list):
        total_inserted += _process_rows(conn, data["data"], trade_date, source="TWSE")
    else:
        # 若當天無資料（週末或節假日），嘗試前一天作為備援
        fallback = _prev_trading_day(trade_date)
        if fallback != trade_date:
            data = fetch_json(
                TWSE_INST,
                params={"date": f"{fallback.replace('-', '')}", "selectType": "ALL", "response": "json"},
                label="TWSE 三大法人（前一天備援）",
            )
            if data and isinstance(data.get("data"), list):
                total_inserted += _process_rows(
                    conn, data["data"], fallback, source="TWSE (fallback)"
                )

    # ---------- TPEX（嘗試多個版本） ----------
    for ver in ("v1", "v2"):
        base = TPEX_INST.replace("v1", ver) if ver == "v2" else TPEX_INST
        data = fetch_json(
            base,
            params={"date": trade_date.replace("-", ""), "response": "json"},
            label=f"TPEX 三大法人 ({ver})",
        )
        if data and isinstance(data, (list, dict)):
            rows = data if isinstance(data, list) else data.get("data", [])
            total_inserted += _process_rows(conn, rows, trade_date, source=f"TPEX {ver}")
            break  # 任一版本成功即可離開

    conn.commit()
    return total_inserted


def _process_rows(
    conn: sqlite3.Connection,
    rows: list,
    trade_date: str,
    *,
    source: str,
) -> int:
    """把原始資料寫入 institutional_trading，回傳實際寫入筆數。"""
    inserted = 0
    for r in rows:
        try:
            stock_id = str(r[0]).strip()
            foreign_net = float(str(r[4]).replace(",", "")) if len(r) > 4 and r[4] not in (None, "", "-") else 0.0
            trust_net = float(str(r[7]).replace(",", "")) if len(r) > 7 and r[7] not in (None, "", "-") else 0.0
            dealer_net = float(str(r[10]).replace(",", "")) if len(r) > 10 and r[10] not in (None, "", "-") else 0.0
            total_net = float(str(r[-1]).replace(",", "")) if r and r[-1] not in (None, "", "-") else 0.0
        except (IndexError, ValueError, TypeError):
            continue

        conn.execute(
            """
            INSERT OR REPLACE INTO institutional_trading
            (trade_date, stock_id, foreign_net, trust_net, dealer_net, total_net, source)
            VALUES (?, ?, ?, ?, ?, ?, ?)
            """,
            (trade_date, stock_id, foreign_net, trust_net, dealer_net, total_net, source),
        )
        inserted += 1
    return inserted
```
```