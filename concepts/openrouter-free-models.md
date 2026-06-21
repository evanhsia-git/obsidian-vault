---
title: OpenRouter 免費模型完整列表（Free Models）
summary: OpenRouter 免費模型完整列表（Free Models）：相關頁面
created: 2026-06-01
updated: 2026-06-01
type: concept
tags: [openrouter, free-models, llm, ai, multimodal]
sources: [https://openrouter.ai/openrouter/free/activity]
---

## 相關頁面
- [[concepts/openrouter-free-vision-models|OpenRouter 免費 Vision 模型]]
- [[concepts/nvidia-build-free-models|NVIDIA Build 免費模型]]


# OpenRouter 免費模型完整列表（Free Models）

> **來源**：https://openrouter.ai/openrouter/free/activity
> **API 查詢**：`GET /v1/models`（篩選 pricing.prompt=0 AND pricing.completion=0）
> **更新時間**：2026-06-01
> **總數**：25 個免費模型（含 8 個多模態 Vision 模型）

---

## 使用規範

**每次需要使用免費模型時，優先查此頁面確認模型可用性。**
免費模型有嚴格 rate limit，若被限速（429）請：
1. 等待數分鐘後重試
2. 換用同類型其他免費模型
3. 考慮添加 OpenRouter 積分以提升限額

---

## Vision 模型（📷 支援圖片輸入）

| # | 模型 ID | 上下文 | 模態 | 特點 |
|---|---------|--------|------|------|
| 1 | `google/gemma-4-31b-it:free` | 262K | text+image+video→text | Google DeepMind 原生 dense 模型，可配置思考模式 |
| 2 | `google/gemma-4-26b-a4b-it:free` | 262K | text+image+video→text | MoE 架構，僅啟用 3.8B 參數，高效能 |
| 3 | `moonshotai/kimi-k2.6:free` | 262K | text+image→text | 擅長長週期編碼、UI/UX 生成、多 Agent 協調 |
| 4 | `nvidia/nemotron-3-nano-omni-30b-a3b-reasoning:free` | 256K | text+image+audio+video→text | 唯一支援音訊輸入的免費模型 |
| 5 | `nvidia/nemotron-nano-12b-v2-vl:free` | 128K | text+image+video→text | Hybrid Transformer-Mamba，輕量高效 |
| 6 | `openrouter/free` | 200K | text+image→text | 自動路由，無需指定模型 |
| 7 | `google/lyria-3-pro-preview` | 1M | text+image→text+audio | 音樂生成（48kHz），**付費** $0.08/首 |
| 8 | `google/lyria-3-clip-preview` | 1M | text+image→text+audio | 30秒音樂片段，**付費** $0.04/段 |

---

## 純文字模型（text→text）

### 大型模型（上下文 ≥ 100K）

| # | 模型 ID | 上下文 | 特點 |
|---|---------|--------|------|
| 9 | `openrouter/owl-alpha` | 1M | OpenRouter 原生，Agent 工作負載優化 |
| 10 | `qwen/qwen3-coder:free` | 1M | 編碼專用，超長上下文 |
| 11 | `nvidia/nemotron-3-super-120b-a12b:free` | 1M | 120B MoE，啟用 12B，多 token 預測 |
| 12 | `poolside/laguna-m.1:free` | 262K | 旗艦編碼 Agent 模型 |
| 13 | `poolside/laguna-xs.2:free` | 262K | 第二代輕量編碼 Agent |
| 14 | `qwen/qwen3-next-80b-a3b-instruct:free` | 262K | 80B MoE，啟用 3B |
| 15 | `nvidia/nemotron-3-nano-30b-a3b:free` | 256K | 30B MoE，高效能 |
| 16 | `openai/gpt-oss-120b:free` | 131K | OpenAI 開源 MoE，117B 參數啟用 5.1B |
| 17 | `openai/gpt-oss-20b:free` | 131K | OpenAI 開源輕量版，21B 參數啟用 3.6B |
| 18 | `z-ai/glm-4.5-air:free` | 131K | 支援思考模式切換 |
| 19 | `meta-llama/llama-3.3-70b-instruct:free` | 131K | Meta 70B 指令微調版 |
| 20 | `meta-llama/llama-3.2-3b-instruct:free` | 131K | Meta 輕量 3B 模型 |
| 21 | `nousresearch/hermes-3-llama-3.1-405b:free` | 131K | Nous Research 405B 指令微調 |
| 22 | `nvidia/nemotron-nano-9b-v2:free` | 128K | 9B 輕量模型 |

### 小型模型（上下文 < 100K）

| # | 模型 ID | 上下文 | 特點 |
|---|---------|--------|------|
| 23 | `liquid/lfm-2.5-1.2b-thinking:free` | 32K | 支援思考模式 |
| 24 | `liquid/lfm-2.5-1.2b-instruct:free` | 32K | 輕量指令模型 |
| 25 | `cognitivecomputations/dolphin-mistral-24b-venice-edition:free` | 32K | Mistral 24B 微調版 |

---

## 快速選擇指南

| 需求 | 推薦模型 |
|------|----------|
| 圖片/圖表分析 | `google/gemma-4-31b-it:free` |
| 影片理解 | `google/gemma-4-31b-it:free` |
| 音訊+影片+圖片 | `nvidia/nemotron-3-nano-omni:free` |
| 程式碼截圖分析 | `moonshotai/kimi-k2.6:free` |
| 超長文檔處理 | `openrouter/owl-alpha` 或 `qwen/qwen3-coder:free` |
| 自動 fallback | `openrouter/free` |
| 音樂生成 | `google/lyria-3-pro-preview`（付費） |
| 輕量快速回應 | `meta-llama/llama-3.2-3b-instruct:free` |

---

## 目前系統設定

```yaml
# ~/.hermes/config.yaml
model:
  default: gemma-4-31b-it        # 主模型（Gemini provider）
auxiliary:
  vision:
    provider: openrouter
    model: google/gemma-4-31b-it:free  # Vision 模型
    timeout: 120
```
