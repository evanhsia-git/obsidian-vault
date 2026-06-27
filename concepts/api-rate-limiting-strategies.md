---
status: active
title: "API 流量限制應對策略"
summary: "API 流量限制應對策略：核心原則"
created: 2026-06-03
updated: 2026-06-03
type: concept
tags: [source, performance, deploy, workflow]
---

# API 流量限制應對策略

## 核心原則
在 Hermes Agent 的金融數據抓取流程中，應對 Rate Limit (429 Too Many Requests) 的黃金法則：**「先緩存、再節流、後重試」**。

## 策略矩陣

| 策略 | 適用場景 | 實作重點 |
| :--- | :--- | :--- |
| **緩存 (Caching)** | 歷史價格、財報數據 | 使用 `~/.hermes/cache/`，檢查 24 小時內檔案 |
| **節流 (Throttling)** | TWSE/FinMind 批次抓取 | 實作 `time.sleep()` 進行請求間隔控制 |
| **退避 (Backoff)** | yfinance (易觸發限流) | 實作指數退避演算法 (Exponential Backoff) |
| **隊列 (Queueing)** | 大規模標的處理 | 寫入 `stock_patch_queue.csv` 由背景 cronjob 執行 |

## 程式碼樣板 (Python)

```python
import time
from tenacity import retry, stop_after_attempt, wait_exponential

@retry(stop=stop_after_attempt(5), wait=wait_exponential(multiplier=1, min=2, max=10))
def fetch_with_retry(func, *args, **kwargs):
    return func(*args, **kwargs)

# 使用範例
def get_stock_price(ticker):
    # 此處呼叫 API
    pass

# 安全呼叫
data = fetch_with_retry(get_stock_price, "2330.TW")
time.sleep(1.0) # 強制節流
```

## 監控指標
- 每日 API 錯誤次數（記錄於 `log.md`）
- API Quota 剩餘額度 (針對 FinMind)
- 佇列處理成功率

---
[[index|回目錄]] | [[concepts/twse-api-mapping|TWSE API 對照表]]

## Corporate Collaboration

---
title: "公司化協作模型：董事長與首席分析師"
description: "公司化協作模型：董事長與首席分析師 — 概念說明頁面"
summary: "公司化協作模型：董事長與首席分析師"
type: concept
status: active
tags: [hermes]
created: 2026-06-05
updated: 2026-06-05
---

# 公司化協作模型：董事長與首席分析師

本頁面定義了 Hermes Agent 與使用者 (Ivan) 之間的協作架構，將系統關係以金融研究公司進行類比，確立責任分工與資產管理邏輯。

## 職責類比表

| 角色/資產 | 類比身份 | 功能職責 |
| :--- | :--- | :--- |
| **您 (Ivan)** | **董事長** | 戰略決策者，負責下達研究目標與審核成果。 |
| **Hermes Agent** | **首席分析師** | 戰術執行者，負責調度資源、分析數據並沉澱知識。 |
| **USER.md** | **客戶檔案** | 儲存核心方針、行事風格與準則。 |
| **MEMORY.md** | **公司備忘錄** | 儲存內部慣例、環境設定與穩定性 Facts。 |
| **SQLite** | **工作管理系統** | 記錄執行軌跡、時序狀態與任務排程。 |
| **LLM Wiki** | **研究資料庫** | 儲存分析模型、驗證過的邏輯與知識結構。 |
| **Obsidian** | **圖書館與檔案館** | 知識的實體儲存與管理，確保資產可檢索與進化。 |

## 運作邏輯
1. **指令傳遞**：董事長下達任務 → 首席分析師接收。
2. **資源調度**：利用 `USER.md` 與 `MEMORY.md` 確立標準 → 查詢 `SQLite` 脈絡 → 結合 `LLM Wiki` 進行分析。
3. **資產化沉澱**：執行結果與智慧結晶最終存入 `Obsidian` 圖書館，確保資產持續增值與可重複利用。

## 核心價值
**「您定義戰略方向，我負責執行並將所有過程與智慧沉澱進公司的圖書館，確保每份資產皆可被重複檢索與進化。」**

- [[openrouter-free-models]]
相關頁面：相關頁面：## 相關節點
- [[index]]

## Python Tool API

---
title: "python-tool-api"
description: "python-tool-api — 概念說明頁面"
summary: "python-tool-api"
type: concept
status: active
tags: [hermes]
created: 2026-06-10
updated: 2026-06-10
---

# Python Tool 與 API 驗證規範

當需要撰寫 Python 程式碼時：

1. 不得假設任何未驗證的模組存在。
2. 不得生成以下未確認模組：
   * `hermes_tools`
   * `agent_tools`
   * `ai_tools`
   * `assistant_tools`
3. 所有 import 必須符合以下其中之一：
   * Python 標準函式庫
   * `requirements.txt` 已安裝套件
   * `pip show` 可查詢套件
   * 官方文件已確認 API
4. 若無法確認 API 存在：
   * 必須明確標示「範例代碼」
   * 不得當作可直接執行程式碼輸出
5. 產生代碼前先驗證：
   `import xxx` 是否可成功執行。
6. 優先使用標準 Python、`requests`、`sqlite3`、`pathlib` 等已知套件。
- [[openrouter-free-models]]
7. 對 Hermes Agent 相關功能，不得虛構 Python SDK。
8. 不確定時先搜尋官方文件，再產生程式碼。


## 相關節點
- [[index]]

## Corporate Collaboration

## 職責類比表

| 角色/資產 | 類比身份 | 功能職責 |
| :--- | :--- | :--- |
| **您 (Ivan)** | **董事長** | 戰略決策者，負責下達研究目標與審核成果。 |
| **Hermes Agent** | **首席分析師** | 戰術執行者，負責調度資源、分析數據並沉澱知識。 |
| **USER.md** | **客戶檔案** | 儲存核心方針、行事風格與準則。 |
| **MEMORY.md** | **公司備忘錄** | 儲存內部慣例、環境設定與穩定性 Facts。 |
| **SQLite** | **工作管理系統** | 記錄執行軌跡、時序狀態與任務排程。 |
| **LLM Wiki** | **研究資料庫** | 儲存分析模型、驗證過的邏輯與知識結構。 |
| **Obsidian** | **圖書館與檔案館** | 知識的實體儲存與管理，確保資產可檢索與進化。 |

## 運作邏輯
1. **指令傳遞**：董事長下達任務 → 首席分析師接收。
2. **資源調度**：利用 `USER.md` 與 `MEMORY.md` 確立標準 → 查詢 `SQLite` 脈絡 → 結合 `LLM Wiki` 進行分析。
3. **資產化沉澱**：執行結果與智慧結晶最終存入 `Obsidian` 圖書館，確保資產持續增值與可重複利用。

## 核心價值
**「您定義戰略方向，我負責執行並將所有過程與智慧沉澱進公司的圖書館，確保每份資產皆可被重複檢索與進化。」**

- [[openrouter-free-models]]
相關頁面：相關頁面：## 相關節點
- [[index]]
## Python Tool API

1. 不得假設任何未驗證的模組存在。
2. 不得生成以下未確認模組：
   * `hermes_tools`
   * `agent_tools`
   * `ai_tools`
   * `assistant_tools`
3. 所有 import 必須符合以下其中之一：
   * Python 標準函式庫
   * `requirements.txt` 已安裝套件
   * `pip show` 可查詢套件
   * 官方文件已確認 API
4. 若無法確認 API 存在：
   * 必須明確標示「範例代碼」
   * 不得當作可直接執行程式碼輸出
5. 產生代碼前先驗證：
   `import xxx` 是否可成功執行。
6. 優先使用標準 Python、`requests`、`sqlite3`、`pathlib` 等已知套件。
- [[openrouter-free-models]]
7. 對 Hermes Agent 相關功能，不得虛構 Python SDK。
8. 不確定時先搜尋官方文件，再產生程式碼。


## 相關節點
- [[index]]