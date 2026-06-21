---
title: architecture_workflow
description: architecture_workflow — 技能說明頁面
summary: architecture_workflow
type: concept
status: published
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

# Hermes Agent 進化系統架構與工作流

本文件詳細記錄 `hermes-agent-self-evolution` 框架在系統中的運作邏輯、觸發條件與標準工作流。

## 1. 運作邏輯核心
本系統採用的並非「常駐式背景服務」，而是「事件觸發式」的自我修復與優化架構。其核心價值在於透過「反饋迴路」(Feedback Loop) 自動識別技能執行中的效率瓶頸，並主動修正代碼或 Prompt。

## 2. 標準工作流 (Standard Workflow)
演化過程嚴格遵循以下四階段週期：

1. **正常執行階段 (Execution)**：執行目標技能（如 `blogwatcher`），產出執行日誌與數據。
2. **效能評估階段 (Evaluation)**：系統調用監控腳本，分析該次執行是否存在超時、報錯或輸出質量不佳（指標：執行成功率、資源消耗）。
3. **演化觸發階段 (Evolution Trigger)**：當評估指標未達標時，啟動 `run_evolution_real.py`，載入目標檔案 (SKILL.md 或腳本) 並產生多個「變異版本」(Mutation)。
4. **驗證與部署階段 (Deployment)**：於隔離環境進行 Dry-Run 評估，選擇最佳版本並建議替換。

## 3. 觸發條件 (Trigger Conditions)
演化觸發並非全自動偵測，而是基於規則 (Rule-based) 的檢核：

*   **觸發檢核點**：技能執行結束後的立即評估。
*   **觸發指標 (Fitness Function)**：
    *   **穩定性**：執行超時頻率 (Timeout frequency)。
    *   **完整性**：輸出內容的非空值檢測 (Non-empty artifact)。
    *   **品質**：標題摘要的格式準確度與結構檢查 (YAML Frontmatter Check)。

## 4. 手動操作指南
若系統發生效能下降或抓取異常，可手動執行以下指令觸發進化程序：

```bash
hermes-evolve
```
*(此指令已設定為 `/usr/local/lib/hermes-agent/venv/bin/python3 /root/hermes-agent-self-evolution/run_evolution_real.py` 的 alias)*

## 5. 擴展說明
本框架實際透過 DSPy 進行優化：**GEPA 在 dspy 3.2.1 不可用，會自動 fallback 至 MIPROv2**。模型引擎在程式碼預設為 `openai/gpt-4.1`，但本環境僅有 `OPENROUTER_API_KEY`，故實測以 `--optimizer-model openrouter/openai/gpt-4o-mini --eval-model openrouter/openai/gpt-4o-mini` 執行。

### 5.1 入口腳本對照（2026-06-11 釐清）
| 腳本 | 用途 | 風險 |
| :--- | :--- | :--- |
| `evolution.skills.evolve_skill`（CLI 模組） | **建議入口**。單一技能演化，支援 `--dry-run`、`--iterations`、`--eval-source` | 可控 |
| `run_all_evolution.py` | 寫死 `dry_run=False`，`os.walk` **全部**技能目錄逐一演化 | ⚠️ 會大量耗 API/時間，勿直接跑 |
| `run_evolution_real.py` | architecture 舊版記載的 alias `hermes-evolve` 對應腳本 | 需確認與 CLI 一致性 |

### 5.2 已修正 bug（2026-06-11）
`evolve_skill.py` 約束驗證原本傳 `evolved_body`（不含 frontmatter）給 `skill_structure` 檢查，導致**任何演化版都被誤判為缺 YAML frontmatter 而 FAILED**。已修正為驗證 `evolved_full`（含 frontmatter），baseline 改用 `skill["raw"]` 對等比較。修正後 4 個新聞技能全數通過驗證。

### 5.3 部署語意更正
框架**沒有「自動回滾」**機制。實際行為是：演化版**通過約束才輸出供審查**（存 `output/<skill>/<timestamp>/`），未通過則存 `evolved_FAILED.md`；**無論如何都不會自動覆蓋現役 SKILL.md**。採納改進需人工 review diff 後手動合併。


## 相關節點
- [[index]]
