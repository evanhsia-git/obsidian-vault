---
title: NVIDIA Build LLM 模型
summary: NVIDIA Build LLM 模型：NVIDIA Nemotron 系列
created: 2026-06-01
updated: 2026-06-01
type: concept
tags: [nvidia, nim, llm, free-models, ai]
sources: [https://build.nvidia.com/models]
---

# NVIDIA Build LLM 模型

> 本頁為 [[concepts/nvidia-build-free-models|NVIDIA Build 免費模型列表]] 的子頁面

---

## NVIDIA Nemotron 系列

| 模型 ID | 參數 | 上下文 | 特色 | 下載量 |
|---------|------|--------|------|--------|
| `nvidia/nemotron-3-super-120b-a12b` | 120B MoE（啟用 12B） | **1M** | Hybrid Mamba-Transformer，Agent 推理/編程/規劃/工具呼叫 | 53.53M |
| `nvidia/nemotron-3-nano-30b-a3b` | 30B MoE | 1M | 高效 MoE，編程/推理/指令遵循 | — |
| `nvidia/nvidia-nemotron-nano-9b-v2` | 9B | — | Transformer-Mamba 混合，推理/Agent 任務 | 1.05M |
| `nvidia/llama-3.3-nemotron-super-49b-v1.5` | 49B | — | 推理/工具呼叫/數學最高精度 | 2.81M |
| `nvidia/nemotron-mini-4b-instruct` | 4B | — | 裝置端推理最佳化，角色扮演/RAG/函式呼叫 | 1.62M |

---

## 第三方主要模型

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

## 相關頁面
- [[concepts/nvidia-build-free-models|NVIDIA Build 免費模型列表]]
- [[concepts/openrouter-free-models|OpenRouter 免費模型]]
