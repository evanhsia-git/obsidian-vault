---
title: "mklab-stock 專案架構"
description: "ivanhsia/mklab-stock 股市公開儀表板——Fork-First / GitHub-Native / Static-First 架構。任何人 Fork 即跑，零 VPS/零 DB/零 AI Key，12 核心模組 100% Build-Time 完成，AI/Chat/Task/Settings 為 Optional 加值（無則自動隱藏）"
summary: "新專案架構 v2：Fork 即可用、優先 GitHub Native、Static First、Offline Friendly、Progressive Enhancement、OSS Friendly、Reproducible、Optional Backend。15 模組中 12 個純 Actions+Pages 完成，3 個 Optional 後端模組 Graceful Degradation"
type: project
status: archived
tags:
  - etf
  - finance
created: 2026-07-13
updated: 2026-07-13
---



# mklab-stock 專案架構

> 狀態：**規劃中（尚未實作）**，本頁為架構決策記錄 v2。
> Repo：`ivanhsia/mklab-stock`（GitHub Public + Pages 啟用）
> 研究基礎：[[finance/github-actions-pages-stock-analysis|GitHub Actions/Pages 股市應用研究]]
> Skill 藍圖（精簡版，含相同 P1-P8 + 自檢清單）：[[mklab-stock-skill|mklab-stock Skill 藍圖]]

## 目錄
一、[最高設計原則 P1-P8](#一最高設計原則architecture-principles-v2) ‧ 二、[P1~P8 落實對照](#二p1p8-逐條對應落實) ‧ 三、[15 模組星級表](#三15-模組星級表6-維評分) ‧ 四、[Build vs Run Time](#四build-time-vs-run-time-重分類) ‧ 五、[Fork Friendly Architecture](#五fork-friendly-architecture) ‧ 六、[GitHub Native Architecture](#六github-native-architecture) ‧ 七、[技術棧](#七技術棧v2-修正) ‧ 八、[資料源](#八資料源v2公開優先) ‧ 八之二、[Data Provider Layer v2](#八之二data-provider-layer-v2能力感知路由-透明-failover) ‧ 九、[15 模組規格](#九15-模組規格對照-v2-原則) ‧ 十、[雙排程](#十雙排程保留符合-p3p7) ‧ 十一、[執行鐵律](#十一執行鐵律red-linesv2) ‧ 十二、[實作階段規劃](#十二實作階段規劃v2)

---

## 一、最高設計原則（Architecture Principles v2）

> 2026-07-13 後段用戶要求重設計：**Fork 即可使用，不依賴任何私人服務**。以下 8 條為最高指導原則，所有決策不得違背。

### P1. Fork First（Fork 即可使用）
任何人只需 Fork Repository 即可使用。**不依賴**：VPS / Docker / SQLite / FastAPI / OpenAI API / Gemini API / 任何私人服務。

### P2. GitHub Native（優先 GitHub 原生）
依序優先使用：
1. GitHub Actions
2. GitHub Pages
3. GitHub Releases
4. GitHub Artifacts
5. GitHub Issues
6. GitHub Discussions
7. GitHub Wiki
8. GitHub Projects

### P3. Static First（靜態優先）
所有主要功能**必須在 Build Time 完成**。React 只負責 Render，不含商業邏輯/計算。

### P4. Offline Friendly（離線友善）
即使沒有 API，也能瀏覽完整分析結果（JSON 進 repo / Pages，可離線看歷史）。

### P5. Progressive Enhancement（漸進增強）
需要後端的功能（AI/Chat/Task/即時）屬**加值**，不影響主要功能。無後端時**自動隱藏**，不是整站不能用。

### P6. OSS Friendly（開源友善）
不需要申請大量 API Key，不需要建立複雜雲端環境。主資料源只用**免 key 公開 API**。

### P7. Reproducible（可重現）
Actions 每次執行都從源頭重抓重算，產生相同網站。

### P8. Optional Backend（後端可選）
Hermes / FastAPI / SQLite / AI **全部 Optional**，不是 Required。沒有也能正常使用。

---

## 二、P1~P8 逐條對應落實

| 原則 | 落實方式 |
|------|---------|
| P1 Fork First | 零 secret 即可跑（TWSE / TPEX / Yahoo 公開 API，免 key） |
| P2 GitHub Native | Actions 算 → Artifact 暫存 → Pages 秀；Issues/Discussions 做示警/回饋 |
| P3 Static First | 資料/計算 Build Time 完成，React 只 render |
| P4 Offline Friendly | JSON 進 repo/Pages，無 API 也能瀏覽歷史分析 |
| P5 Progressive Enhancement | AI/Chat/Task 是加值；沒有就隱藏，不影響 12 核心模組 |
| P6 OSS Friendly | 不用 FinMind/OpenAI 等需申請 key 的源（改公開源）；key 全 Optional |
| P7 Reproducible | Actions 每次從源頭重抓重算，產相同站 |
| P8 Optional Backend | Hermes/FastAPI/SQLite/AI 全 Optional |

---

## 三、15 模組星級表（6 維評分）

> ★5=優 / ★1=差。`*` = AI 部分 Optional，無 LLM 時降級為規則/標題，模組仍可用。
> 6.13/6.14/6.15 後端不存在時**整個模組從 UI 消失**，不佔用星級。

| # | 模組 | Build/Run | GitHub Native | Fork Friendly | Offline Friendly | Static Friendly | Optional Backend | Maintainability |
|---|------|-----------|--------------|--------------|-----------------|---------------|-----------------|-----------------|
| 6.1 | Dashboard | Build | ★5 | ★5 | ★5 | ★5 | ★5 | ★5 |
| 6.2 | AI Analysis* | Build* | ★5 | ★5* | ★5 | ★5 | ★5 | ★5 |
| 6.3 | Multi-Factor | Build | ★5 | ★5 | ★5 | ★5 | ★5 | ★5 |
| 6.4 | ETF Center | Build | ★5 | ★5 | ★5 | ★5 | ★5 | ★5 |
| 6.5 | Portfolio | Build | ★5 | ★5 | ★5 | ★5 | ★5 | ★5 |
| 6.6 | Market | Build | ★5 | ★5 | ★5 | ★5 | ★5 | ★5 |
| 6.7 | Backtest | Build | ★5 | ★5 | ★5 | ★5 | ★5 | ★5 |
| 6.8 | Strategy | Build | ★5 | ★5 | ★5 | ★5 | ★5 | ★5 |
| 6.9 | AI News* | Build* | ★5 | ★5* | ★5 | ★5 | ★5 | ★5 |
| 6.10 | Screener | Build | ★5 | ★5 | ★5 | ★5 | ★5 | ★5 |
| 6.11 | Compare | Build | ★5 | ★5 | ★5 | ★5 | ★5 | ★5 |
| 6.12 | Heatmap | Build | ★5 | ★5 | ★5 | ★5 | ★5 | ★5 |
| 6.13 | Chat | Run | ★1 | ★1 | ★1 | ★1 | ★1 | ★3 |
| 6.14 | Task | Run | ★1 | ★1 | ★1 | ★1 | ★1 | ★3 |
| 6.15 | Settings | Run | ★1 | ★2 | ★2 | ★2 | ★1 | ★3 |

**網站整體**（不含 3 個 Optional 模組）：GitHub Native ★5 / Fork ★5 / Offline ★5 / Static ★5 / Optional Backend ★5 / Maintainability ★5

---

## 四、Build Time vs Run Time 重分類

### ✅ Build Time（GitHub Actions，零後端，fork 即跑）
```
fetch_tw.py        → TWSE / TPEX 公開 API（免 key）
fetch_us.py        → Yahoo Finance 公開 API（免 key）
calc_factor.py     → 多因子評分（Value/Growth/Momentum/Quality/Volatility/Liquidity）
calc_screener.py   → 條件篩選
calc_backtest.py   → 回測（純 Python，12 策略）
calc_portfolio.py  → 讀 lots.csv 算 IRR/XIRR/Sharpe
calc_heatmap.py    → 產業/漲跌熱力
calc_compare.py    → 個股比較雷達
news_aggregator.py → 公開源抓標題（AI 摘要 Optional）
analysis_rule.py   → 規則型分析（非 LLM，無 key 也能產）
export_json.py     → 全部匯出 data/*.json（含 ai_summary:null 當無 LLM）
build_status.py    → status.json（last_trading_day + is_stale）
npm run build      → React → dist/
```

### ⚙️ Run Time（Optional，僅當 Hermes 後端存在才啟用）
```
/api/v1/chat      → Hermes LLM 即時推理（無 → 隱藏）
/api/v1/task      → Hermes 觸發任務（無 → 隱藏）
/api/v1/settings  → Key 持久化（無 → 只顯示唯讀說明）
/api/v1/*         → 即時資料模式（無 → 前端只用靜態 JSON）
```

---

## 五、Fork Friendly Architecture

```
任何人 Fork ivanhsia/mklab-stock
        │
        ├─ 啟用 GitHub Actions（預設 workflow，ZERO secrets）
        │     │  抓 TWSE / TPEX / Yahoo（全公開、免 key）
        │     ▼
        │  Actions: fetch → calc → export JSON → build React → artifact
        │     ▼
        └─ GitHub Pages 託管靜態站
               │
               ▼
          使用者開站：
          ✅ 12 核心模組全可用（無 VPS / 無 DB / 無 AI / 無 key）
          ⚙️ Chat / Task / Settings：自動隱藏（graceful degradation）
          📌 若想加 AI：自行設 LLM secret → 重新跑 Actions 即啟用
```

---

## 六、GitHub Native Architecture

```
┌─ GitHub Repo: ivanhsia/mklab-stock ──────────────┐
│  frontend/   (React SPA，只 render)                   │
│  data/       (*.json，Actions 產出，進版控)            │
│  scripts/    (fetch_*.py / calc_*.py / export_json)   │
│  .github/workflows/ (market.yml / deploy.yml)         │
└──────────────────────┬───────────────────────────────┘
                         │
         ┌───────────────┼────────────────┐
         ▼               ▼                ▼
    GitHub Actions   GitHub Pages    GitHub (Optional)
    ├ fetch          ├ 靜態 SPA        ├ Issues（停滯示警）
    ├ calc            ├ 無伺服器        ├ Discussions（回饋）
    ├ export JSON     └ 讀 data/*.json  └ Releases（版本）
    ├ build React
    └ upload artifact ──── push ────▶ Pages 更新

   ⚙️ Hermes VPS（可選，非必要）
      └ /api/v1/* → 僅增強（Chat/Task/Settings/即時模式）
         前端 DataClient 預設 mode:'json'；偵測不到後端自動隱藏
```

---

## 七、技術棧（v2 修正）

| 層 | 選擇 | 備註 |
|----|------|------|
| 前端 | React + TS + Vite + Tailwind + shadcn/ui | 只 render，零商業邏輯 |
| 圖表 | ECharts（echarts-for-react） | K線/雷達/熱力 |
| 表格 | TanStack Table（虛擬化） | 全市場 ~1700 檔 |
| 狀態 | Zustand（UI） + TanStack Query（server） | 職責分離 |
| 路由 | React Router（Nested + Code Splitting） | config-driven 導航 |
| 後端腳本 | Python 3.11（純腳本，免 DB） | 直接轉 JSON，不依賴 SQLite |
| 部署 | Actions artifact-based Pages | 不進分支 |
| 資料 | TWSE / TPEX / Yahoo（全免 key） | 主源即公開 |
| AI | Optional（LLM Router + 快取） | 無 key 時 ai_summary:null |
| 示警 | GitHub Issues + Telegram/Email（選配 secret） | 停滯偵測 |

> ⚠️ **SQLite 不再是必需**：Python 腳本直接 fetch → 計算 → 匯出 JSON，不經 DB。SQLite 僅作本地開發快取（Optional）。
> ⚠️ **FinMind / OpenBB / OpenAI / Gemini 全降為 Optional 增強**：主源只用免 key 公開 API；設了 secret 才啟用備援/AI。

---

## 八、資料源（v2：公開優先）

| 市場 | 主源（免 key） | Optional 增強（設 secret 才用） |
|------|--------------|--------------------------------|
| 台股 | TWSE OpenAPI `STOCK_DAY_ALL` | yfinance（備援） |
| 上櫃 ETF | TPEX OpenAPI v1 | yfinance（過濾 ETF 清單） |
| 美股 | Yahoo Finance 公開 API | FinMind / OpenBB（備援） |
| 新聞 | 公開 RSS / 公告 | FinMind News + LLM 摘要（設 key） |

> 備援策略：每 fetch 函式 `try 主源 except 切備援`；主備皆無 → 標記「資料缺失」繼續其他市場，不中斷。

---

## 八之二、Data Provider Layer v2（能力感知路由 + 透明 Failover）

> 目標不變：**完全支援 Fork / GitHub Actions / GitHub Pages；預設零 API Key；所有 Provider 可互換；Strategy 不知資料來源。**
> 相對 v1 的關鍵修正：**v1 只按 tier 順序輪詢，但 TWSE 不能回 AAPL、TPEX 不能回美股，天真 failover 會對不支援的市場硬打**。v2 改為「能力感知路由」——先過濾（tier∩secret∩市場∩方法∩健康）再排序。

### 8.2.1 Tier 與能力矩陣

| Provider | Tier | 覆蓋市場 | 支援方法（price/history/dividend/financial/news） | Key |
|----------|------|----------|---------------------------------------------------|-----|
| TWSE | 1 | TW（上市） | price, history | 無 |
| TPEx | 1 | TPEX（上櫃/ETF） | price, history | 無 |
| Yahoo (yfinance) | 1 | TW, US, GLOBAL, 指數 | price, history, dividend, financial, news | 無 |
| Stooq | 1 | US, GLOBAL, 指數 | price, history | 無 |
| FinMind | 2 | TW | price, history, dividend, financial, news | `FINMIND_TOKEN` |
| Alpha Vantage | 2 | US, GLOBAL | price, history, dividend, financial | `ALPHAVANTAGE_KEY` |
| FMP | 2 | US, GLOBAL | price, history, dividend, financial | `FMP_KEY` |
| Finnhub | 2 | US, GLOBAL | price, history, financial, news | `FINNHUB_KEY` |
| Polygon | 3 | US, GLOBAL | price, history, dividend, financial | `POLYGON_KEY` |
| Bloomberg | 3 | GLOBAL | price, history, dividend, financial, news | `BLOOMBERG_KEY` |

> 同一市場有多個 provider 即形成真實 failover（如 `2330.TW` → TWSE + Yahoo；`AAPL` → Yahoo + Stooq）。不支援的方法（如 Stooq 無 news）在矩陣標記，pool 直接跳過該 provider。

### 8.2.2 型別契約（`providers/types.py`）

```python
from dataclasses import dataclass
from enum import Enum

class Market(str, Enum):
    TW = "tw"; TPEX = "tpex"; US = "us"; GLOBAL = "global"

class Interval(str, Enum):
    DAILY = "1d"; WEEKLY = "1wk"; MONTHLY = "1mo"

@dataclass
class Bar:
    date: str; open: float; high: float; low: float; close: float; volume: int

@dataclass
class Quote:                      # get_price 回傳：最新一筆快照
    symbol: str; date: str; price: float; currency: str
    prev_close: float | None = None

@dataclass
class Dividend:
    date: str; amount: float; currency: str = "USD"

@dataclass
class Financial:
    symbol: str
    roe: float | None = None; eps: float | None = None
    pb: float | None = None; pe: float | None = None
    dividend_yield: float | None = None

@dataclass
class NewsItem:
    source: str; title: str; url: str
    published_at: str | None = None
    sentiment: str | None = None; importance: int | None = None
```

### 8.2.3 Provider 介面（`providers/base.py`）

每個 provider 宣告 `markets` 與 `methods`（能力），並實作統一方法；不支援的方法拋 `NotImplementedError`，由 pool 跳過。

```python
from abc import ABC, abstractmethod
from .types import Bar, Quote, Dividend, Financial, NewsItem, Market, Interval

class Provider(ABC):
    name: str
    tier: int
    markets: set[Market]          # 能力：覆蓋哪些市場
    methods: set[str]             # 能力：支援哪些方法

    # ---- 統一介面（Strategy 只認這五個）----
    @abstractmethod
    def get_price(self, symbol: str) -> Quote: ...
    @abstractmethod
    def get_history(self, symbol: str, days: int = 250,
                    interval: Interval = Interval.DAILY) -> list[Bar]: ...
    @abstractmethod
    def get_dividend(self, symbol: str) -> list[Dividend]: ...
    @abstractmethod
    def get_financial(self, symbol: str) -> Financial: ...
    @abstractmethod
    def get_news(self, symbol: str) -> list[NewsItem]: ...

    # ---- 可選覆寫 ----
    def normalize(self, symbol: str) -> str:
        """canonical symbol → 該 provider 專用格式。預設原樣。"""
        return symbol

    def supports(self, market: Market, method: str) -> bool:
        return market in self.markets and method in self.methods
```

### 8.2.4 具體 Provider（能力宣告範例）

```python
# providers/twse.py
class TWSEProvider(Provider):
    name = "twse"; tier = 1
    markets = {Market.TW}
    methods = {"get_price", "get_history"}
    def normalize(self, symbol): return symbol.replace(".TW", "")   # 2330.TW → 2330
    def get_history(self, symbol, days=250, interval=Interval.DAILY):
        # TWSE OpenAPI STOCK_DAY_ALL → Bar[]（免 key）
        ...
    # get_dividend/get_financial/get_news → raise NotImplementedError（pool 跳過）

# providers/yahoo.py
class YahooProvider(Provider):
    name = "yahoo"; tier = 1
    markets = {Market.TW, Market.US, Market.GLOBAL}
    methods = {"get_price","get_history","get_dividend","get_financial","get_news"}
    def normalize(self, symbol): return symbol           # yfinance 直接用 2330.TW / AAPL
    def get_history(self, symbol, days=250, interval=Interval.DAILY):
        # yfinance 免 key → Bar[]
        ...

# providers/stooq.py
class StooqProvider(Provider):
    name = "stooq"; tier = 1
    markets = {Market.US, Market.GLOBAL}
    methods = {"get_price", "get_history"}     # 無 dividend/financial/news

# providers/finmind.py  (Tier 2，需 FINMIND_TOKEN 才實例化)
class FinMindProvider(Provider):
    name = "finmind"; tier = 2
    markets = {Market.TW}
    methods = {"get_price","get_history","get_dividend","get_financial","get_news"}
```

### 8.2.5 Registry（Fork 預設自動發現）

`build_registry(enabled_tiers)` 決定「哪些 provider 進池」：
- **Tier 1 永遠進池**（免 key）。
- **Tier 2/3 僅當對應 secret 環境變數存在才實例化**；不存在則不進池、永不呼叫。
- 回傳依 `(tier, 內部優先序)` 升冪排序的列表。

```python
# providers/registry.py
import os
from .twse import TWSEProvider
from .tpex import TPExProvider
from .yahoo import YahooProvider
from .stooq import StooqProvider
from .finmind import FinMindProvider   # 需 token
# ... 其餘 Tier2/3 provider

TIER1 = [TWSEProvider, TPExProvider, YahooProvider, StooqProvider]
TIER2 = [(FinMindProvider, "FINMIND_TOKEN"),
         (AlphaVantageProvider, "ALPHAVANTAGE_KEY"),
         (FMPProvider, "FMP_KEY"),
         (FinnhubProvider, "FINNHUB_KEY")]
TIER3 = [(PolygonProvider, "POLYGON_KEY"),
         (BloombergProvider, "BLOOMBERG_KEY")]

def build_registry(enabled_tiers=(1,2,3)):
    provs = []
    if 1 in enabled_tiers:
        provs += [P() for P in TIER1]
    if 2 in enabled_tiers:
        provs += [P() for P, env in TIER2 if os.getenv(env)]
    if 3 in enabled_tiers:
        provs += [P() for P, env in TIER3 if os.getenv(env)]
    return sorted(provs, key=lambda p: p.tier)   # tier 升冪；同 tier 依宣告順序
```

> **Fork 即用**：任何 Fork 不設任何 secret → 池中只有 TWSE/TPEx/Yahoo/Stooq，全免 key。設了 `FINMIND_TOKEN` 才多一個 TW 備援；設 `ALPHAVANTAGE_KEY` 才多一個 US 備援。未設 key 的 provider 程式碼即使存在也不會被呼叫。

### 8.2.6 DataProvider Facade + 透明 Failover（`providers/pool.py`）

這是 Strategy 唯一會碰的物件。**能力感知路由 + 斷路器 + 超時 + 同 run 快取**。

```python
# providers/pool.py
import time, threading
from .base import Provider
from .types import Market, Interval, DataUnavailable

FAIL_THRESHOLD = 3      # 連續失敗達此數 → 斷路暫停
COOLDOWN_SEC   = 300    # 暫停秒數
CALL_TIMEOUT   = 20     # 單次呼叫超時（秒），避免 Actions 卡死

class DataProvider:
    def __init__(self, providers: list[Provider]):
        self.providers = sorted(providers, key=lambda p: p.tier)
        self._cache: dict = {}          # 同 run 快取（每次 Actions run 重建）

    # ---- 對外統一介面（Strategy 只 call 這些）----
    def get_price(self, symbol):    return self._call("get_price", symbol)
    def get_history(self, symbol, days=250, interval=Interval.DAILY):
        return self._call("get_history", symbol, days, interval)
    def get_dividend(self, symbol): return self._call("get_dividend", symbol)
    def get_financial(self, symbol):return self._call("get_financial", symbol)
    def get_news(self, symbol):     return self._call("get_news", symbol)

    # ---- 能力感知路由 ----
    def _route(self, symbol: str, method: str) -> list[Provider]:
        market = resolve_market(symbol)
        return [p for p in self.providers
                if p.supports(market, method) and p.is_healthy()]

    def _call(self, method, symbol, *a, **kw):
        cache_key = (method, symbol, a, tuple(kw.items()))
        if cache_key in self._cache:
            return self._cache[cache_key]
        tried = []
        for p in self._route(symbol, method):       # 已按 tier 排序
            try:
                with _timeout(CALL_TIMEOUT):
                    result = getattr(p, method)(p.normalize(symbol), *a, **kw)
                p.mark_success()
                self._cache[cache_key] = result
                return result
            except (NotImplementedError, DataUnavailable, Exception) as e:
                p.mark_failure()                     # 斷路器 +1
                tried.append((p.name, repr(e)))
        raise DataUnavailable(symbol, method, tried)
```

- `resolve_market(symbol)`：`.TW`→TW、`.TWO`→TPEX、其餘→US（或呼叫時顯式帶 `market=`）。
- `p.is_healthy()`：連續失敗 ≥ `FAIL_THRESHOLD` 時進 `COOLDOWN_SEC` 冷卻，期間不被選入；成功一次即復活。
- `_timeout(...)`：threading 包裝，單 provider 卡死不拖垮整條 pipeline。
- 同 run 快取：Build Time 內同一 symbol 多次計算只抓一次（仍符合 Reproducible——每次 run 重抓）。

### 8.2.7 Failover 流程圖（含能力過濾）

```
Strategy.calc_momentum("AAPL")
        │
        ▼
DataProvider.get_history("AAPL")
        │  resolve_market → US
        │  _route 過濾：TWSE/TPEX 不覆蓋 US → 剔除；剩 Yahoo, Stooq (+, FinMind 無 token 不出現)
        │  依 tier 排序嘗試：
        ├─ Yahoo (tier1) ──✅ 回傳 Bar[] → Strategy 不知來源
        │
        │  （若 Yahoo 壞掉 → mark_failure + 斷路器）
        ▼
        ├─ Stooq (tier1) ──✅ 自動接手
        │
        │  （若 Stooq 也壞 → 設了 FINMIND_TOKEN 才會出現 FinMind，否則跳過）
        ▼
        └─ 全失敗 → DataUnavailable(symbol, method, tried=[...])
           → 上層標記該市場缺失，不中斷其他計算

# 台股範例：calc_momentum("2330.TW")
#   resolve_market → TW → 候選 TWSE + Yahoo（兩者皆覆蓋 TW）
#   TWSE 先試 → 壞 → Yahoo 自動接手（真實 TW failover）
```

### 8.2.8 設計紅線

1. **Strategy / Calc 只依賴 `DataProvider` facade**，禁止 `import` 具體 provider、禁止直打 `yfinance` / TWSE API / Stooq。
2. **Tier 1 永不依賴 secret**；Tier 2/3 無對應 key 時 `build_registry` 不自動加入池、永不呼叫。
3. **Failover 對上層完全透明**：Strategy 只見成功結果或 `DataUnavailable(tried=[...])`（含嘗試過的 provider 與錯誤，便於 Debug）。
4. **能力感知路由**：不對「不支援該市場/方法」的 provider 發請求，省 quota、避錯。
5. **斷路器 + 超時**：連續失敗的 provider 暫時降權；單次呼叫卡死不拖垮整條 pipeline。
6. **Reproducible**：每次 Actions run 重抓重算；同 run 快取只省重複呼叫，不跨 run 留存。

### 8.2.9 檔案結構

```
scripts/providers/
  types.py        # Bar / Quote / Dividend / Financial / NewsItem / Market / Interval
  base.py         # Provider ABC（markets / methods / normalize / supports）
  registry.py     # build_registry()：依 tier + secret 自動發現
  pool.py         # DataProvider facade：路由 + failover + 斷路器 + 快取
  exceptions.py   # DataUnavailable
  twse.py  tpex.py  yahoo.py  stooq.py
  finmind.py  alphavantage.py  fmp.py  finnhub.py
  polygon.py  bloomberg.py
```

> **Strategy 範例（零來源知識）**：
> ```python
> from providers.pool import DataProvider
> from providers.registry import build_registry
> pool = DataProvider(build_registry())          # Fork 即 Tier1
> def calc_momentum(symbol, pool, lookback=20):
>     bars = pool.get_history(symbol, days=250)  # 不知來源，可能是 TWSE 也可能是 Yahoo
>     return (bars[-1].close - bars[-lookback].close) / bars[-lookback].close
> ```

---

## 九、15 模組規格（對照 v2 原則）

> 技術棧：React + TS + Tailwind + shadcn/ui（SPA，讀 `data/*.json` 渲染）。
> 標的風格：Bloomberg / TradingView / OpenBB / Koyfin。

### 6.0 架構導航（15 項，config-driven）
```
🏠 Dashboard  📈 Market  🔍 Screener  🤖 AI Analysis  📊 Portfolio
📑 Backtest  🧠 Strategy  📦 ETF  📰 News  🔥 Heatmap
📅 Calendar  📝 Report  🤖 Chat*  📋 Task*  ⚙️ Settings*
```
（* = Optional 後端模組，無 Hermes 時自動隱藏）

### 6.1~6.12 展示模組（Build Time，100% GitHub Native）
- **6.1 Dashboard**：Watch List / K線 / 技術指標 / 新聞摘要。`data/market.json`
- **6.2 AI Analysis***：規則分析 Build 產；AI Summary 設 LLM 才補。`data/ai_analysis.json`（無 LLM → `ai_summary:null`）
- **6.3 Multi-Factor**：6 因子評分+雷達。`data/factor_scores.json`
- **6.4 ETF Center**：清單/持股/殖利率/管理費/追蹤誤差。`data/etf.json` + `data/etf_meta.json`
- **6.5 Portfolio**：讀 `lots.csv` 算 IRR/XIRR/Sharpe。`data/portfolio.json`
- **6.6 Market**：市值/成交量/漲跌/三大法人。`data/market.json` + `data/institution.json`
- **6.7 Backtest**：策略績效+Equity Curve。`data/backtest.json`
- **6.8 Strategy**：12 策略（Registry 插件化）。`data/strategies.json`
- **6.9 AI News***：公開源標題 Build 產；AI 摘要設 LLM 才補。`data/news.json`
- **6.10 Screener**：條件自由組合。`data/screener.json`
- **6.11 Compare**：最多 5 支雷達。`data/compare.json`
- **6.12 Heatmap**：市值/漲跌/產業切換。`data/heatmap.json`

### 6.13~6.15 Optional 後端模組（Run Time，Graceful Degradation）
- **6.13 Chat**：Hermes LLM 即時問股。無 Hermes → 整模組隱藏。
- **6.14 Task**：Hermes 觸發任務。無 Hermes → 隱藏。
- **6.15 Settings**：API Key 設定。無後端 → 只顯示唯讀說明，不暴露編輯。

---

## 十、雙排程（保留，符合 P3/P7）

### 08:30 台灣（抓前晚美股）
```
Actions(us-market.yml) → fetch_us.py(Yahoo 免 key) → data/us_market.json（不 push）
```

### 17:00 台灣（台股+合併→一次 push）
```
Actions(tw-market.yml)
  ├ fetch_tw.py (TWSE+TPEX)
  ├ calc_factor/screener/backtest/portfolio/heatmap/compare
  ├ news_aggregator（公開源）
  ├ export_json → data/*.json
  ├ build_status → status.json
  ├ npm run build → dist/
  └ git push 一次（含前日美股+當日台股）
```

---

## 十一、執行鐵律（Red Lines，v2）

1. **Fork 即跑**：預設 workflow 零 secret 可全功能（12 核心模組）
2. **Static First**：所有計算 Build Time 完成；React 不含商業邏輯
3. **Offline Friendly**：JSON 進 repo，無 API 也能看
4. **Graceful Degradation**：無 Hermes/AI → 相關模組隱藏，非整站掛
5. **Optional Secret**：FinMind/OpenAI 等全選配；無 key 時產 `null` 不報錯
6. **每日一次 push**：08:30 暫存不 commit，17:00 合併一次 push
7. **匿名**：真實持倉絕不 push；公開只用測試示範資料+免責聲明
8. **Reproducible**：每次 Actions 從源頭重抓重算

> 對應 P 碼：1→P1 · 2→P3 · 3→P4 · 4→P5 · 5→P6/P8 · 6→P7 · 7→P1（產品隱私規則） · 8→P7

**上線前自檢清單**（與 Skill 藍圖同步）：
- [ ] 零 secret 情境下功能仍可用或優雅隱藏？→ P1/P8
- [ ] 計算發生在 Build Time，非前端 Run Time？→ P3
- [ ] 資料已進 `data/*.json`，離線可讀？→ P4
- [ ] 缺 Hermes/LLM 時顯示 `null`/隱藏，而非報錯？→ P5
- [ ] 未新增付費或高門檻必要依賴？→ P6
- [ ] 重跑 Actions 可重現一致結果？→ P7

---

## 十二、實作階段規劃（v2）

| Phase | 目標 | 可部署 | 零 secret |
|-------|------|--------|-----------|
| 0 | Contract + repo + CI 骨架 | 否 | ✅ |
| 1 | MVP 市場+ETF+首頁（免 key） | ✅ Pages | ✅ |
| 2 | 多因子+Screener+Heatmap | ✅ | ✅ |
| 3 | 回測+策略中心 | ✅ | ✅ |
| 4 | AI 分析+News（規則版，無 LLM） | ✅ | ✅ |
| 5 | 投資組合+Compare+報表 | ✅ | ✅ |
| 6 | Optional: 設 LLM secret → AI 增強 | ✅ | 需 secret |
| 7 | Optional: Hermes VPS → Chat/Task/Settings | ✅ VPS | 需 VPS |
| 8 | Release + 監控（Issues 示警） | ✅ 正式 | ✅ |

> ⚠️ Phase 6/7 為**加值**，不影響 Phase 1~5 已部署的公開站。

---

## 相關節點
- [[finance/github-actions-pages-stock-analysis|GitHub Actions/Pages 股市應用研究]]
- [[mklab-stock-prompt|實作紀錄與提示詞]]
- [[mklab-stock-qa-1|架構 Q&A（第一批）]]
- [[mklab-stock-qa-2|多角色架構審查]]
- [[mklab-stock-resource|資源清單]]
- [[mklab-stock-skill|Skill 藍圖（標準 SKILL.md 規範，含 P1-P8 自檢清單）]]
- [[finance/etf-active-stock/etf-active-stock|台灣主動式 ETF 清單]]
- [[finance/etf-code-classification|ETF 代碼分類與第六碼意義]]
- `quant-trading`（既存 Hermes skill，提供 DB/選股/回測；與本專案整合對照表見 [[mklab-stock-prompt|實作紀錄]] 第六節）

