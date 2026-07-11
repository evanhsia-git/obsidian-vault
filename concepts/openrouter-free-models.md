---
status: active
title: "OpenRouter 免費模型完整列表（Free Models）"
description: "OpenRouter 免費模型列表與使用指南（含 Vision 多模態模型詳細卡片）"
summary: "OpenRouter 免費模型完整列表（Free Models）：純文字 + Vision 多模態模型一覽"
created: 2026-06-01
updated: 2026-07-09
type: concept
tags: [ai]
---

## 相關頁面
- [[concepts/nvidia-build-free-models|NVIDIA Build 免費模型]]
- Gemini API 定價

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

> 以下為支援圖片/影片/音訊輸入的免費多模態模型詳細卡片。

### 1. Google Gemma 4 31B Instruct
- **ID**: `google/gemma-4-31b-it:free`
- **參數量**: 30.7B（dense）
- **上下文**: 262K tokens
- **模態**: text + image + video → text
- **用途**: 通用多模態推理、圖片理解、影片分析、長文檔處理
- **特色**: 可配置思考/推理模式（thinking mode）；Google DeepMind 原生 dense 模型
- **推薦用途**: Agent 視覺分析、圖表理解、截圖解讀

### 2. Google Gemma 4 26B A4B Instruct
- **ID**: `google/gemma-4-26b-a4b-it:free`
- **參數量**: 25.2B（MoE，每次啟用 3.8B）
- **上下文**: 262K tokens
- **模態**: text + image + video → text
- **用途**: 高效能多模態推理，接近 31B 品質但計算成本更低
- **特色**: Mixture-of-Experts 架構，啟用參數僅 3.8B
- **推薦用途**: 資源受限場景下的視覺分析

### 3. Moonshot AI Kimi K2.6
- **ID**: `moonshotai/kimi-k2.6:free`
- **上下文**: 262K tokens
- **模態**: text + image → text
- **用途**: 長週期編碼、UI/UX 生成、多 Agent 協調
- **特色**: 擅長端到端複雜編碼任務、多模態編排
- **推薦用途**: 程式碼截圖分析、UI 設計審查、多 Agent 工作流程

### 4. NVIDIA Nemotron 3 Nano Omni 30B
- **ID**: `nvidia/nemotron-3-nano-omni-30b-a3b-reasoning:free`
- **參數量**: 30B-A3B（MoE）
- **上下文**: 256K tokens
- **模態**: text + image + audio + video → text
- **用途**: 企業級感知子代理、多模態理解
- **特色**: 支援音訊輸入，適合需要音影片綜合處理的場景
- **推薦用途**: 會議錄影分析、多媒體內容理解

### 5. NVIDIA Nemotron Nano 12B V2 VL
- **ID**: `nvidia/nemotron-nano-12b-v2-vl:free`
- **參數量**: 12B
- **上下文**: 128K tokens
- **模態**: text + image + video → text
- **用途**: 影片理解、文件智慧
- **特色**: Hybrid Transformer-Mamba 架構，輕量高效
- **推薦用途**: 文件掃描識別、短影片分析

### 6. OpenRouter Free（自動路由）
- **ID**: `openrouter/free`
- **上下文**: 200K tokens
- **模態**: text + image → text
- **用途**: 自動選擇當前可用的免費模型
- **特色**: 無需指定模型，OpenRouter 自動路由到可用免費模型
- **推薦用途**: 不挑模型時的 fallback 方案

### 7. Google Lyria 3 Pro Preview
- **ID**: `google/lyria-3-pro-preview`
- **上下文**: 1M tokens
- **模態**: text + image → text + audio（音樂生成）
- **用途**: 高品質音樂生成（48kHz）
- **特色**: 從圖片或文字描述生成完整歌曲
- **注意**: 非免費（每首 $0.08），但因支援圖片輸入而列入

### 8. Google Lyria 3 Clip Preview
- **ID**: `google/lyria-3-clip-preview`
- **上下文**: 1M tokens
- **模態**: text + image → text + audio（音樂生成）
- **用途**: 30 秒音樂片段生成
- **特色**: 快速生成短音樂片段
- **注意**: 非免費（每段 $0.04），但因支援圖片輸入而列入

### 快速對照表

| 模型 ID | 上下文 | 圖片 | 影片 | 音訊 | 推薦用途 |
|---------|--------|:----:|:----:|:----:|----------|
| gemma-4-31b-it:free | 262K | ✅ | ✅ | ❌ | 通用視覺分析 |
| gemma-4-26b-a4b-it:free | 262K | ✅ | ✅ | ❌ | 高效視覺推理 |
| kimi-k2.6:free | 262K | ✅ | ❌ | ❌ | 程式碼截圖分析 |
| nemotron-3-nano-omni:free | 256K | ✅ | ✅ | ✅ | 多媒體理解 |
| nemotron-nano-12b-v2-vl:free | 128K | ✅ | ✅ | ❌ | 文件/影片分析 |
| openrouter/free | 200K | ✅ | ❌ | ❌ | 自動 fallback |
| lyria-3-pro-preview | 1M | ✅ | ❌ | 🎵 | 音樂生成（付費） |
| lyria-3-clip-preview | 1M | ✅ | ❌ | 🎵 | 短音樂生成（付費） |

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
