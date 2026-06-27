---
status: active
title: "NVIDIA Build 免費模型列表（NIM API）"
summary: "NVIDIA Build 100+ 免費 AI 模型：LLM、視覺、嵌入、語音、安全、OCR、物理 AI 完整模型清單與 API 設定"
created: 2026-06-01
updated: 2026-06-27
type: concept
tags: [ai, integration]
---

# NVIDIA Build 免費模型列表（NIM API）

> **來源**：https://build.nvidia.com/models
> **免費 API 指南**：https://pasqualepillitteri.it/en/news/1621/nvidia-build-free-api-100-ai-models-2026
> **更新時間**：2026-06-27

---

## 平台概述

NVIDIA Build（build.nvidia.com）提供 **100+ 個免費 AI 模型**，透過 **OpenAI 相容 API** 託管於 DGX Cloud。

### 關鍵數據

| 項目 | 數值 |
|------|------|
| **總模型數** | 100+（含 46 個預覽版 NIM） |
| **免費端點** | 46 個（preview 篩選） |
| **API 相容性** | OpenAI SDK 完全相容 |
| **註冊方式** | 僅需 Email，無需信用卡 |
| **免費額度** | 初始 1,000 credits，最高可達 5,000 |
| **速率限制** | 40 req/min/模型/帳號 |
| **API 端點** | `https://integrate.api.nvidia.com/v1` |

### 設定方式

```python
from openai import OpenAI

client = OpenAI(
    base_url="https://integrate.api.nvidia.com/v1",
    api_key="nvapi-XXXXXXXXXXXXXXXXXXXXXXXXXXXX"
)

response = client.chat.completions.create(
    model="deepseek-ai/deepseek-v3.1",
    messages=[{"role": "user", "content": "..."}],
    temperature=0.6,
    max_tokens=512,
)

# 模型命名格式：provider/model-name
# 例如：meta/llama-3.1-70b-instruct
#       moonshotai/kimi-k2
#       zhipuai/glm-5.1
```

---

## 1. LLM 模型（Nemotron 系列 + 第三方）

### NVIDIA Nemotron 系列

| 模型 ID | 參數 | 上下文 | 特色 | 下載量 |
|---------|------|--------|------|--------|
| `nvidia/nemotron-3-super-120b-a12b` | 120B MoE（啟用 12B） | **1M** | Hybrid Mamba-Transformer，Agent 推理/編程/規劃/工具呼叫 | 53.53M |
| `nvidia/nemotron-3-nano-30b-a3b` | 30B MoE | 1M | 高效 MoE，編程/推理/指令遵循 | — |
| `nvidia/nvidia-nemotron-nano-9b-v2` | 9B | — | Transformer-Mamba 混合，推理/Agent 任務 | 1.05M |
| `nvidia/llama-3.3-nemotron-super-49b-v1.5` | 49B | — | 推理/工具呼叫/數學最高精度 | 2.81M |
| `nvidia/nemotron-mini-4b-instruct` | 4B | — | 裝置端推理最佳化，角色扮演/RAG/函式呼叫 | 1.62M |

### 第三方主要模型

| 模型 ID | 出版商 | 參數 | 上下文 | 特色 |
|---------|--------|------|--------|------|
| `deepseek-ai/deepseek-v3.1` | DeepSeek | — | 64K | 數學推理、程式碼生成、低成本 |
| `deepseek-ai/deepseek-v4` | DeepSeek | — | — | 最新 DeepSeek 模型 |
| `moonshotai/kimi-k2` | Moonshot | 1T MoE（啟用 32B） | **200K** | 超長上下文 |
| `moonshotai/kimi-k2.5` | Moonshot | — | — | 多模態 |
| `zhipuai/glm-5.1` | Z.ai | — | — | Agent 工作流、多語言程式碼、工具使用 |
| `zhipuai/glm-4.7` | Z.ai | — | — | Agent 工作流 |
| `meta/llama-3.1-8b-instruct` | Meta | 8B | 131K | 通用 |
| `meta/llama-3.1-70b-instruct` | Meta | 70B | 128K | 通用高精度 |
| `meta/llama-3.1-405b-instruct` | Meta | 405B | 131K | 最強 Meta 模型 |
| `meta/llama-3.2-3b-instruct` | Meta | 3B | 131K | 輕量 |
| `meta/llama-3.2-vision-instruct` | Meta | — | — | 視覺多模態 |
| `openai/gpt-oss-120b` | OpenAI | 117B MoE（啟用 5.1B） | 131K | OpenAI 開源模型 |
| `openai/gpt-oss-20b` | OpenAI | 21B MoE（啟用 3.6B） | 131K | OpenAI 輕量開源模型 |
| `google/gemma-3n-e4b-it` | Google | 4B | — | 邊緣 AI：文字/音訊/圖片 |
| `google/gemma-3n-e2b-it` | Google | 2B | — | 資源受限環境 |
| `mistralai/mistral-large-3-675b-instruct` | Mistral | 675B MoE | — | 最先進 VLM |
| `stepfun-ai/step-3.5-flash` | Stepfun | 200B MoE | — | Agent 推理引擎 |
| `stepfun-ai/step-3.7-flash` | Stepfun | — | — | 稀疏 MoE 多模態推理，企業/Agent/編碼 |
| `bytedance/seed-oss-36b-instruct` | ByteDance | 36B | — | 開源，長上下文/推理/Agent |
| `qwen/qwen3-coder-480b-a35b-instruct` | Qwen | 480B MoE（啟用 35B） | **256K** | 頂級 Agent 編碼 |

---

## 2. 視覺/多模態模型

| 模型 ID | 類型 | 特色 |
|---------|------|------|
| `nvidia/cosmos3-nano` | 影片生成 | 從文字/圖片生成物理感知影片 |
| `nvidia/cosmos3-nano-reasoner` | VLM | 影片/圖片的結構化推理，理解物理世界 |
| `nvidia/cosmos-transfer2.5-2b` | 影片生成 | 物理感知影片世界狀態生成 + 空間控制 |
| `nvidia/cosmos-transfer1-7b` | 影片生成 | 物理感知影片世界狀態生成 |
| `nvidia/cosmos-predict1-5b` | 影片預測 | 從圖片/短影片預測未來� |
| `nvidia/streampetr` | 3D 物件偵測 | 自駕車高效 3D 物件偵測 |
| `metavision/llava` | VLM | 圖片理解 |
| `nvidia/nvclip` | 嵌入 | 圖片-文字匹配多模態嵌入 |
| `nvidia/llama-3.1-nemotron-nano-vl-8b-v1` | VLM | 文件智慧、視覺問答 |

---

## 3. 嵌入/檢索模型

| 模型 ID | 類型 | 下載量 | 備註 |
|---------|------|--------|------|
| `nvidia/nv-embedqa-e5-v5` | QA 嵌入 | **27.79M** | 英文問答嵌入，最高下載量 |
| `nvidia/llama-nemotron-embed-1b-v2` | 多語言嵌入 | 38.24M | 26 語言，長文件 QA 檢索 |
| `nvidia/nv-embed-v1` | 文字嵌入 | 3.03M | 高品質文字嵌入（非商業使用） |
| `nvidia/nv-embedcode-7b-v1` | 程式碼嵌入 | — | 支援文字/程式碼/混合查詢 |
| `nvidia/llama-nemotron-reranker-vl-1b-v2` | 重排序 | — | 多模態 QA 重排序（文字+圖片） |
| `nvidia/nv-embedqa-mistral-7b-v2` | QA 嵌入 | — | 多語言 QA 嵌入 |

---

## 4. 語音/音訊模型

| 模型 ID | 類型 | 特色 | 下載量 |
|---------|------|------|--------|
| `nvidia/parakeet-1.1b-rnnt-multilingual-asr` | ASR | **25 語言**高精度轉錄 | — |
| `nvidia/parakeet-ctc-0.6b-zh-tw` | ASR | 台灣中文 ASR + 標點/時間戳 | — |
| `nvidia/parakeet-ctc-0.6b-zh-cn` | ASR | 簡體中文 ASR | — |
| `nvidia/nemotron-asr-streaming` | ASR | 英文即時語音辨識 | 9.93K |
| `nvidia/nemotron-voicechat` | 語音聊天 | 英文語音聊天 | 1.99K |
| `nvidia/magpie-tts-zeroshot` | TTS | 零樣本聲音複製 TTS | 17.69K |
| `nvidia/magpie-tts-multilingual` | TTS | 多語言 TTS | 142K |
| `nvidia/studiovoice` | 音訊增強 | 低品質麥克風轉錄音室級語音 | — |
| `nvidia/riva-translate-4b-instruct` | 翻譯 | **12 語言**翻譯 + few-shot | 274K |
| `nvidia/background-noise-removal` | 降噪 | 移除背景噪音 | — |

---

## 5. 內容安全模型

| 模型 ID | 功能 | 下載量 |
|---------|------|--------|
| `nvidia/nemotron-3-content-safety` | 多模態多語言毒性內容偵測 | 126K |
| `nvidia/nemotron-content-safety-reasoning-4b` | 推理式安全（整合 NeMo Guardrails） | 109K |
| `nvidia/llama-3.1-nemotron-safety-guard-8b-v3` | 多語言 LLM 內容審核 | 169K |
| `nvidia/llama-3.1-nemoguard-8b-topic-control` | 主題控制（防止離題） | — |
| `nvidia/nemoguard-jailbreak-detect` | **越獄偵測**（對抗性提示防護） | — |
| `nvidia/gliner-pii` | **PII 偵測**（個人識別資訊） | 257K |
| `nvidia/synthetic-video-detector` | **AI 生成影片偵測** | 91K |

---

## 6. 文件智慧/OCR 模型

| 模型 ID | 功能 | 下載量 |
|---------|------|--------|
| `nvidia/nemoretriever-ocr-v1` | OCR 文字萃取/版面分析/表格偵測 | **1.15M** |
| `nvidia/nemotron-ocr-v1` | OCR 文字萃取 | 366K |
| `nvidia/nemotron-table-structure-v1` | 表格結構偵測 | — |
| `nvidia/nemoretriever-page-elements-v3` | 圖表/表格/標題物件偵測 | — |
| `nvidia/nemotron-parse` | 圖片文字/元資料檢索 | — |
| `nvidia/nemotron-hammer-3b-v1` | 工具呼叫格式化 | — |

---

## 7. 物理 AI/自駕車模型

| 模型 ID | 功能 |
|---------|------|
| `nvidia/cosmos-predict1-5b` | 影片幀預測 |
| `nvidia/cosmos-transfer1-7b` | 世界狀態生成 |
| `nvidia/cosmos-transfer2.5-2b` | 空間控制影片生成 |
| `nvidia/streampetr` | 3D 物件偵測 |
| `nvidia/sparsedrive` | 端到端自駕（感知→預測→規劃） |
| `nvidia/bevformer` | Transformer 鳥瞰圖 3D 感知 |
| `nvidia/active-speaker-detection` | 影片中講者追蹤 |

---

## 與 OpenRouter 免費模型比較

| 維度 | NVIDIA Build | OpenRouter Free |
|------|-------------|-----------------|
| **模型數** | 100+ | 25 |
| **免費額度** | 1,000–5,000 credits | 依模型而異 |
| **速率限制** | 40 req/min | 20 req/min |
| **API 相容** | OpenAI SDK | OpenAI SDK |
| **信用卡** | ❌ 不需要 | ❌ 不需要 |
| **最大上下文** | 1M（Nemotron Super） | 1M（Owl Alpha） |
| **多模態** | ✅ 豐富（視覺/語音/影片） | ✅ 8 個 Vision 模型 |
| **內容安全** | ✅ 完整系列 | ❌ 無 |
| **OCR/文件** | ✅ 完整系列 | ❌ 無 |
| **語音/ASR/TTS** | ✅ 完整系列 | ❌ 無 |

---

## 本系統目前使用

| 使用場景 | 模型 | 來源 |
|----------|------|------|
| 主模型 | `gemma-4-31b-it` | Gemini API |
| Vision | `google/gemma-4-31b-it:free` | OpenRouter |
| **可考慮切換** | `nvidia/nemotron-3-super-120b-a12b` | NVIDIA Build（1M 上下文） |

---

## 相關頁面
- [[concepts/openrouter-free-models|OpenRouter 免費模型]]
- [[concepts/openrouter-free-vision-models|OpenRouter 免費 Vision 模型]]
- [[concepts/gemini-api-pricing|Gemini API 定價]]
