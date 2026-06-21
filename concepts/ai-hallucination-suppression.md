---
title: ai-hallucination-suppression
description: ai-hallucination-suppression — 概念說明頁面
summary: ai-hallucination-suppression
type: concept
status: active
priority: P2
tags: [hermes]
aliases: []
created: 2026-06-10
updated: 2026-06-10
date: 2026-06-10
publish: true
draft: false
related:
source:
due:
review:
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