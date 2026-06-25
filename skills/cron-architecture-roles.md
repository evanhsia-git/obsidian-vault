---
title: "Cron 架構角色分工：Skills、Python、no_agent"
description: "說明每日台股 cron 中 Skills、Python 腳本、no_agent 三者各自的角色與關係"
summary: "Cron 架構中 Skills、Python、no_agent 的角色釐清"
type: concept
status: active
tags: [auto, agent, linux]
related: "[[skills/skills-index]]"
created: 2026-06-21
updated: 2026-06-25
---

# Cron 架構角色分工：Skills、Python、no_agent

> 釐清每日台股自動化流程中三個關鍵要素的角色。

## 角色對照表

| 層級 | 角色 | 說明 |
|:---|:---|:---|
| **Cron Job** | 排程觸發器 | 定時啟動，決定「何時跑」 |
| **Wrapper 腳本** (`*-cron.py`) | 參數傳遞層 | 用 `subprocess.run` 呼叫主腳本，傳入模式參數 |
| **Python 腳本** (`.py`) | 執行邏輯層 | 實際抓取資料、過濾、格式化、輸出 |
| **Skills** (`.md`) | 知識文件層 | 記錄「怎麼做」的規範，**不被執行** |

## 以每日台股為例

```
cronjob (no_agent: true)
  └─ daily-news-twstock-cron.py    ← wrapper，subprocess 傳 "twstock" 參數
       └─ daily-news.py twstock    ← 主腳本，RSS 抓取 + keep_twstock() 過濾
```

### Python 腳本做什麼

- HTTP 請求（RSS、API）
- XML / JSON 解析
- 關鍵字過濾（`keep_twstock()`）
- 文字格式化與 stdout 輸出

### Skills 做什麼

- 寫在 `~/.hermes/skills/` 裡的 `.md` 檔案
- 給 **Agent（Hermes）** 看的操作手冊
- 記錄工作流程、參數、注意事項
- **不被任何腳本 import 或執行**

## 關鍵問題：no_agent 會用到 Skills 嗎？

**不會。**

`no_agent: true` 的 cron job 執行流程：

1. Scheduler 啟動 → 執行 `script`（Python 腳本）
2. 腳本 stdout 直接作為訊息交付
3. **完全跳過 Agent**，所以不會讀取任何 Skill

Skills 只在以下情況被載入：

- **Agent 對話模式**（用戶與 Hermes 聊天時）
- **`no_agent: false`** 的 cron（Agent 會讀 Skill 再執行任務）

## 相關頁面

- [[stock-automation-config|自動化配置設定]]
- [[stock-analysis-system-guide|股市分析系統使用說明]]
- [[skills/skills-index|Skills 目錄]]
