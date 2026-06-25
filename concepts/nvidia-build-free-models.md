---
status: active
title: "NVIDIA Build 免費模型列表（NIM API）"
summary: "NVIDIA Build 免費模型列表（NIM API）：平台概述"
created: 2026-06-01
updated: 2026-06-01
type: concept
tags: [ai, integration]
---

# NVIDIA Build 免費模型列表（NIM API）

> **來源**：https://build.nvidia.com/models
> **免費 API 指南**：https://pasqualepillitteri.it/en/news/1621/nvidia-build-free-api-100-ai-models-2026
> **更新時間**：2026-06-01

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

## 子頁面導覽

| 頁面 | 內容 |
|------|------|
| [[nvidia-build-llm-models|LLM 模型]] | Nemotron 系列、第三方主要模型（DeepSeek/Kimi/GLM/Llama/GPT-OSS/Qwen 等） |
| [[nvidia-build-vision-models|視覺/多模態模型]] | Cosmos 系列、LLaVA、NV-CLIP、VLM |
| [[nvidia-build-embedding-models|嵌入/檢索模型]] | NV-EmbedQA、Llama Nemotron Embed、程式碼嵌入 |
| [[nvidia-build-speech-models|語音/音訊模型]] | Parakeet ASR、Magpie TTS、Riva 翻譯、降噪 |
| [[nvidia-build-safety-models|內容安全模型]] | 毒性偵測、越獄偵測、PII 偵測、AI 生成影片偵測 |
| [[nvidia-build-document-models|文件智慧/OCR 模型]] | Nemotron OCR、表格結構偵測、頁面元素偵測 |
| [[nvidia-build-physical-ai-models|物理 AI/自駕車模型]] | Cosmos Predict/Transfer、StreamPETR、SparseDrive |

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
