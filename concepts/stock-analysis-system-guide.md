---
title: 股市分析三層 Agent 協作系統使用說明
summary: 股市分析三層 Agent 協作系統使用說明：1. 系統運行流程
created: 2026-06-03
updated: 2026-06-03
type: concept
tags: [automation, multi-agent, finance, guide]
---

# 股市分析三層 Agent 協作系統使用說明

本系統透過 Orchestrator 調度三個專業 Skill，實現從數據擷取、分析到決策的全自動化流程。

## 1. 系統運行流程

```mermaid
graph TD
    User((使用者指令)) --> Orchestrator[股市分析 Orchestrator]
    
    subgraph "三層協作架構"
        Orchestrator -->|1. 請求數據| Gatherer[Skill 1: Data Gatherer]
        Gatherer -->|回傳標準化 JSON| Orchestrator
        
        Orchestrator -->|2. 發送數據審核| Analyzer[Skill 2: Data Analyzer]
        Analyzer -->|回傳過濾後分析因子| Orchestrator
        
        Orchestrator -->|3. 綜合評估建議| Decider[Skill 3: Decision Maker]
        Decider -->|產出 PDF/HTML 報告| User
    end

    style Orchestrator fill:#4A90E2,stroke:#fff,stroke-width:2px,color:#fff
    style Gatherer fill:#50E3C2,stroke:#333,stroke-width:1px
    style Analyzer fill:#F5A623,stroke:#333,stroke-width:1px
    style Decider fill:#D0021B,stroke:#333,stroke-width:1px,color:#fff
```

## 2. 使用方法

### 步驟 A：環境初始化
1. 確保已建立三個專屬 Skill 目錄 (`ws-data-gatherer`, `ws-data-analyzer`, `ws-decision-maker`)。
2. 配置各個 Skill 的 `SKILL.md` 定義其專屬工具集 (Toolsets)。

### 步驟 B：調用方式
透過 `hermes-wallstreet-orchestrator` 呼叫主流程。在 Python 環境中執行：

```python
from hermes_tools import delegate_task

# Orchestrator 核心調度邏輯
def run_analysis(ticker):
    # 1. 擷取 (Gatherer)
    data = delegate_task(goal=f"擷取 {ticker} 財務/新聞數據")
    
    # 2. 分析 (Analyzer)
    factors = delegate_task(goal=f"計算 {ticker} 價值因子與動能", context=data[0])
    
    # 3. 決策 (Decider)
    report = delegate_task(goal=f"產出 {ticker} 投資報告", context=factors[0])
    
    return report[0]
```

## 3. 重要注意事項
- **狀態傳遞**：由於每個 Agent 環境隔離，數據均透過 `context` 參數傳遞，務必確保數據字典格式統一 (建議使用 JSON)。
- **交付規範**：最終產出文件（PDF/HTML）一律透過 `telegram-message-file-sender` 技能發送，嚴禁使用 `MEDIA` 參數。
- **維護規範**：每次架構層級變更或邏輯更新，須同步更新本頁面的 Mermaid 流程圖。
