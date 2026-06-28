---
status: active
title: "OpenRouter 免費 Vision 模型列表"
description: "OpenRouter 免費視覺模型"
summary: "OpenRouter 免費 Vision 模型列表：相關頁面"
created: 2026-06-01
updated: 2026-06-01
type: concept
tags: [ai]
---

## 相關頁面
- [[concepts/openrouter-free-models|OpenRouter 免費模型完整列表]]
- [[concepts/gemini-api-pricing|Gemini API 定價]]


# OpenRouter 免費 Vision 模型列表

> 更新時間：2026-06-01
> 來源：OpenRouter API `/v1/models`
> 條件：免費（prompt + completion 定價為 $0）且支援圖片輸入

---

## 模型列表

### 1. Google Gemma 4 31B Instruct
- **ID**: `google/gemma-4-31b-it:free`
- **參數量**: 30.7B（dense）
- **上下文**: 262K tokens
- **模態**: text + image + video → text
- **用途**: 通用多模態推理、圖片理解、影片分析、長文檔處理
- **特色**: 可配置思考/推理模式（thinking mode）；Google DeepMind 原生 dense 模型
- **推薦用途**: Agent 視覺分析、圖表理解、截圖解讀

---

### 2. Google Gemma 4 26B A4B Instruct
- **ID**: `google/gemma-4-26b-a4b-it:free`
- **參數量**: 25.2B（MoE，每次啟用 3.8B）
- **上下文**: 262K tokens
- **模態**: text + image + video → text
- **用途**: 高效能多模態推理，接近 31B 品質但計算成本更低
- **特色**: Mixture-of-Experts 架構，啟用參數僅 3.8B
- **推薦用途**: 資源受限場景下的視覺分析

---

### 3. Moonshot AI Kimi K2.6
- **ID**: `moonshotai/kimi-k2.6:free`
- **上下文**: 262K tokens
- **模態**: text + image → text
- **用途**: 長週期編碼、UI/UX 生成、多 Agent 協調
- **特色**: 擅長端到端複雜編碼任務、多模態編排
- **推薦用途**: 程式碼截圖分析、UI 設計審查、多 Agent 工作流程

---

### 4. NVIDIA Nemotron 3 Nano Omni 30B
- **ID**: `nvidia/nemotron-3-nano-omni-30b-a3b-reasoning:free`
- **參數量**: 30B-A3B（MoE）
- **上下文**: 256K tokens
- **模態**: text + image + audio + video → text
- **用途**: 企業級感知子代理、多模態理解
- **特色**: 支援音訊輸入，適合需要音影片綜合處理的場景
- **推薦用途**: 會議錄影分析、多媒體內容理解

---

### 5. NVIDIA Nemotron Nano 12B V2 VL
- **ID**: `nvidia/nemotron-nano-12b-v2-vl:free`
- **參數量**: 12B
- **上下文**: 128K tokens
- **模態**: text + image + video → text
- **用途**: 影片理解、文件智慧
- **特色**: Hybrid Transformer-Mamba 架構，輕量高效
- **推薦用途**: 文件掃描識別、短影片分析

---

### 6. OpenRouter Free（自動路由）
- **ID**: `openrouter/free`
- **上下文**: 200K tokens
- **模態**: text + image → text
- **用途**: 自動選擇當前可用的免費模型
- **特色**: 無需指定模型，OpenRouter 自動路由到可用免費模型
- **推薦用途**: 不挑模型时的fallback方案

---

### 7. Google Lyria 3 Pro Preview
- **ID**: `google/lyria-3-pro-preview`
- **上下文**: 1M tokens
- **模態**: text + image → text + audio（音樂生成）
- **用途**: 高品質音樂生成（48kHz）
- **特色**: 從圖片或文字描述生成完整歌曲
- **推薦用途**: 音樂創作、影像配樂、AI 生成音樂
- **注意**: 非免費（每首 $0.08），但因支援圖片輸入而列入

---

### 8. Google Lyria 3 Clip Preview
- **ID**: `google/lyria-3-clip-preview`
- **上下文**: 1M tokens
- **模態**: text + image → text + audio（音樂生成）
- **用途**: 30 秒音樂片段生成
- **特色**: 快速生成短音樂片段
- **推薦用途**: 短影片配樂、音效生成
- **注意**: 非免費（每段 $0.04），但因支援圖片輸入而列入

---

## 快速對照表

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

## 目前 Vision API 設定

```yaml
# ~/.hermes/config.yaml
auxiliary:
  vision:
    provider: openrouter
    model: google/gemma-4-31b-it:free  # 已從 pixtral-large-2411 修正
    timeout: 120
```

> ⚠️ 免費模型有嚴格 rate limit，若頻繁使用建議考慮付費方案或添加 OpenRouter 積分。
