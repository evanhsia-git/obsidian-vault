---
status: active
title: "API 流量限制應對策略"
summary: "API 流量限制應對策略：核心原則"
created: 2026-06-03
updated: 2026-06-03
type: concept
tags: [source, performance, deploy, flow]
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
