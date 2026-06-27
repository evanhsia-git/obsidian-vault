---
title: "ai-hallucination-suppression"
description: "ai-hallucination-suppression — 概念說明頁面"
summary: "ai-hallucination-suppression"
type: concept
status: active
tags: [hermes]
created: 2026-06-10
updated: 2026-06-10
---

## AI 幻覺抑制規範 (六大關鍵)
### 強制引用 (Mandatory Citation)
* 回答需標記 [參考來源]，無法從原文找到的資訊請明確說明，嚴禁引用。

### 分步推理 (Chain-of-Thought)
* 回答前拆解任務步驟並逐步分析，最後進行總結。

### 角色邊界 (Boundaries)
* 作為專業分析師，超出專業領域問題直接說明，勿提供預測。

### 否定性約束 (Negative Constraints)
- [[openrouter-free-models]]
* 無相關資料時回覆『無法從給定文字中找到答案』，不得填入預測值。

### 置信度標記 (Uncertainty Labeling)
* 信心低於 80% 時標記『【低置信度】』。

### 後驗證提示 (Self-Check)
* 輸出前自我審查，確認內容完全依據原始資料，違背者修正。


## 相關節點
- [[index]]

## Model Error Messages
---
status: active
title: "模型回報常見訊息"
summary: "模型回報常見訊息：維護建議"
created: 2026-06-03
updated: 2026-06-03
type: concept
tags: [ai, hermes, deploy]
---

# 模型回報常見訊息 (Troubleshooting)

本頁面彙整 Hermes Agent 在執行任務時常見的系統回報訊息，包含中英文對照、發生原因與改善建議。

| 英文訊息 | 中文翻譯 | 發生原因 | 建議改善方式 |
| :--- | :--- | :--- | :--- |
| **Rate limited** | 達到速率限制 | 請求頻率超過 API 額度。 | 增加請求延遲 (Sleep)，執行隊列式處理。 |
| **Switching to fallback provider** | 切換至備用提供商 | 主模型 API 異常，啟動備援機制。 | 檢查 API Key 有效性或備份模型狀態。 |
| **Model is currently warming up** | 模型正在預熱中 | Serverless 端點冷啟動。 | 等待預熱完成，或設定延長 timeout。 |
| **Latency spike detected** | 偵測到延遲峰值 | API 回應時間異常拉長。 | 避開高峰期或優化 Query 長度。 |
| **Connection timeout reached** | 連線逾時 | 連線建立過久。 | 檢查網路連線或調整 timeout 設定。 |
| **Context window full** | 上下文視窗已滿 | 輸入長度超過模型上限。 | 精簡內容，執行摘要 (Summarization)。 |
| **Model load error / 503** | 模型加載錯誤/服務不可用 | 伺服器端暫時無法處理。 | 執行重試機制 (Retry)。 |
| **Stream interrupted** | 資料串流中斷 | 傳輸過程異常終止。 | 檢查 WebSocket 連線。 |
| **Payload too large** | 負載過大 | 傳輸內容體積過大。 | 壓縮檔案或分批發送。 |
| **Queue position: N** | 目前排隊順序：N | 伺服器端擁塞。 | 等待處理，無需操作。 |

## 維護建議
當出現上述訊息時，系統會嘗試自我修復（如：切換 Fallback Provider）。若頻繁出現，應檢查任務執行隊列是否過於集中。

---
## 相關節點
- [[hermes-workflow]]
- [[agent-driven-cronjobs]]
---
status: active
title: "模型回報常見訊息"
summary: "模型回報常見訊息：維護建議"
created: 2026-06-03
updated: 2026-06-03
type: concept
tags: [ai, hermes, deploy]
---

# 模型回報常見訊息 (Troubleshooting)

本頁面彙整 Hermes Agent 在執行任務時常見的系統回報訊息，包含中英文對照、發生原因與改善建議。

| 英文訊息 | 中文翻譯 | 發生原因 | 建議改善方式 |
| :--- | :--- | :--- | :--- |
| **Rate limited** | 達到速率限制 | 請求頻率超過 API 額度。 | 增加請求延遲 (Sleep)，執行隊列式處理。 |
| **Switching to fallback provider** | 切換至備用提供商 | 主模型 API 異常，啟動備援機制。 | 檢查 API Key 有效性或備份模型狀態。 |
| **Model is currently warming up** | 模型正在預熱中 | Serverless 端點冷啟動。 | 等待預熱完成，或設定延長 timeout。 |
| **Latency spike detected** | 偵測到延遲峰值 | API 回應時間異常拉長。 | 避開高峰期或優化 Query 長度。 |
| **Connection timeout reached** | 連線逾時 | 連線建立過久。 | 檢查網路連線或調整 timeout 設定。 |
| **Context window full** | 上下文視窗已滿 | 輸入長度超過模型上限。 | 精簡內容，執行摘要 (Summarization)。 |
| **Model load error / 503** | 模型加載錯誤/服務不可用 | 伺服器端暫時無法處理。 | 執行重試機制 (Retry)。 |
| **Stream interrupted** | 資料串流中斷 | 傳輸過程異常終止。 | 檢查 WebSocket 連線。 |
| **Payload too large** | 負載過大 | 傳輸內容體積過大。 | 壓縮檔案或分批發送。 |
| **Queue position: N** | 目前排隊順序：N | 伺服器端擁塞。 | 等待處理，無需操作。 |

## 維護建議
當出現上述訊息時，系統會嘗試自我修復（如：切換 Fallback Provider）。若頻繁出現，應檢查任務執行隊列是否過於集中。

---
## 相關節點
- [[hermes-workflow]]
- [[agent-driven-cronjobs]]
## Model Error Messages

status: active
title: "模型回報常見訊息"
summary: "模型回報常見訊息：維護建議"
created: 2026-06-03
updated: 2026-06-03
type: concept
tags: [ai, hermes, deploy]
---

# 模型回報常見訊息 (Troubleshooting)

本頁面彙整 Hermes Agent 在執行任務時常見的系統回報訊息，包含中英文對照、發生原因與改善建議。

| 英文訊息 | 中文翻譯 | 發生原因 | 建議改善方式 |
| :--- | :--- | :--- | :--- |
| **Rate limited** | 達到速率限制 | 請求頻率超過 API 額度。 | 增加請求延遲 (Sleep)，執行隊列式處理。 |
| **Switching to fallback provider** | 切換至備用提供商 | 主模型 API 異常，啟動備援機制。 | 檢查 API Key 有效性或備份模型狀態。 |
| **Model is currently warming up** | 模型正在預熱中 | Serverless 端點冷啟動。 | 等待預熱完成，或設定延長 timeout。 |
| **Latency spike detected** | 偵測到延遲峰值 | API 回應時間異常拉長。 | 避開高峰期或優化 Query 長度。 |
| **Connection timeout reached** | 連線逾時 | 連線建立過久。 | 檢查網路連線或調整 timeout 設定。 |
| **Context window full** | 上下文視窗已滿 | 輸入長度超過模型上限。 | 精簡內容，執行摘要 (Summarization)。 |
| **Model load error / 503** | 模型加載錯誤/服務不可用 | 伺服器端暫時無法處理。 | 執行重試機制 (Retry)。 |
| **Stream interrupted** | 資料串流中斷 | 傳輸過程異常終止。 | 檢查 WebSocket 連線。 |
| **Payload too large** | 負載過大 | 傳輸內容體積過大。 | 壓縮檔案或分批發送。 |
| **Queue position: N** | 目前排隊順序：N | 伺服器端擁塞。 | 等待處理，無需操作。 |

## 維護建議
當出現上述訊息時，系統會嘗試自我修復（如：切換 Fallback Provider）。若頻繁出現，應檢查任務執行隊列是否過於集中。

---
## 相關節點
- [[hermes-workflow]]
- [[agent-driven-cronjobs]]