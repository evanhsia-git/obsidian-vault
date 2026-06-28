---
title: "Agent 驅動任務 (LLM-driven Cronjob)"
description: "Agent 驅動任務 (LLM-driven Cronjob) — 概念說明頁面"
summary: "Agent 驅動任務 (LLM-driven Cronjob)"
type: concept
status: active
tags: [hermes, agent, auto]
created: 2026-06-12
updated: 2026-06-12
---

# 🤖 Agent 驅動任務 (LLM-driven Cronjob)

在 Hermes Agent 系統中，自動化任務主要分為三種類型。**Agent 驅動任務** 是其中最具靈活性、但也最具成本（Token 使用量較高）的一種模式。

## 🎯 定義

**Agent 驅動任務** 是一種透過 Cronjob 排程，在指定時間啟動一個完整的 AI Agent 實例，並交由其執行「包含推理邏輯」的複雜指令。與傳統腳本不同，它不依賴預寫好的程式碼路徑，而是依賴 **Prompt (指令)** 來驅動 Agent 使用工具（如 `terminal`, `web_search`, `read_file`）來達成目標。

---

## 🔄 三種自動化模式對比

| 特性 | 傳統 Script (Python/Bash) | 既有 Skill (Modular Tool) | Agent 驅動任務 (LLM-driven) |
| :--- | :--- | :--- | :--- |
| **核心驅動力** | 寫死的程式碼邏輯 | 模組化的工具函數 | **AI 的推理與理解力** |
| **靈活性** | 極低 (只能做預定動作) | 中 (可組合不同的 Skill) | **極高 (可處理非結構化資訊)** |
| **成本 (Token)** | 極低 (幾乎為 0) | 低 (僅工具呼叫成本) | **高 (需進行完整推理與生成)** |
| **錯誤處理** | 依賴 Try-Catch 邏輯 | 依賴工具回傳值 | **具備自主糾錯與嘗試能力** |
| **最佳場景** | 資料搬移、簡單備份、固定報表 | 抓取特定 API、執行標準化分析 | **摘要分析、異常偵測、跨來源彙整** |

---

## 📖 使用說明 (How to Design)

要設計一個成功的 Agent 驅動任務，Prompt 的品質至關重要。一個高品質的任務 Prompt 應包含以下要素：

1. **明確的目標 (Goal)**：告訴 Agent 最終要產出的結果是什麼（例如：一份 Markdown 報告、一個 SQLite 紀錄）。
2. **資料來源與範圍 (Context/Scope)**：指定 Agent 需要檢閱哪些檔案或搜尋哪些範圍（例如：`read log.md`、`grep yesterday's date`）。
3. **步驟指南 (Workflow)**：雖然 Agent 會自行推理，但給予初步的步驟建議能大幅提升成功率。
4. **輸出格式規範 (Output Format)**：嚴格定義輸出格式（例如：使用特定的 Header、禁止使用某些符號、必須包含特定欄位）。

### 💡 撰寫範例 (Bad vs Good)

❌ **錯誤範例 (過於模糊)**：
> 「幫我看看昨天的日誌並做個報告。」
> *(Agent 可能會跑出冗長的對話、格式混亂、或不知道要寫到哪裡)*

✅ **正確範例 (結構化指令)**：
> 「請執行以下任務：
> 1. 讀取 `/root/Documents/Obsidian Vault/log.md`。
> 2. 使用 `grep` 篩選出包含日期 `$(date -d 'yesterday' +'%Y-%m-%d')` 的所有行。
> 3. 將結果整理成一份『每日任務執行報告』。
> 4. 報告格式要求：
>    - 第一行：`**任務報告 - [日期]**`
>    - 第二行：成功/失敗統計
>    - 第三行：各項任務的摘要說明
> 5. 最後將報告直接輸出於回應中。」

---

## 🚀 應用範例 (Use Cases)

### 1. 跨來源資訊彙整 (Intelligence Synthesis)
* **場景**：每天早上抓取技術新聞、股市新聞與內部 Log，並將三者結合，分析是否有任何外部事件影響了內部的技術變動。
* **價值**：傳統腳本無法理解「新聞內容」與「Log 變動」之間的語意關聯，但 Agent 可以。

### 2. 智慧型健康檢查 (Smart System Audit)
* **場景**：定期掃描 Obsidian Vault，不只是檢查語法錯誤，還要檢查「知識是否過時」或「兩個概念之間是否存在邏輯矛盾」。
* **價值**：能提供具備「洞察力」的維護建議，而不僅僅是錯誤清單。

### 3. 自適應回報系統 (Adaptive Reporting)
* **場景**：根據當天任務的複雜度，自動決定報告的長度與詳細程度。
* **價值**：任務簡單時給出精簡摘要，任務重大時自動擴充成深入分析。

---

## ⚠️ 設計原則與警告

* **成本意識**：若任務可以透過單純的 Python 腳本完成，**絕對不要**使用 Agent 驅動模式。
* **防範「假裝完成」**：對於高成本任務，建議在 Prompt 中加入「驗證步驟」（例如：要求 Agent 在完成後執行 `ls` 或 `wc -l` 來確認檔案已寫入）。
* **防範資訊爆炸**：確保 Agent 知道何時該「保持沉默 (Silent)」，避免在無事可做時噴出大量的冗餘資訊。

---
## 相關節點
- [[hermes-workflow]]
- [[cron-jobs]]

---

## Architecture Roles

---\ntitle: "Cron 架構角色分工：Skills、Python、no_agent"\ndescription: "說明每日台股 cron 中 Skills、Python 腳本、no_agent 三者各自的角色與關係"\nsummary: "Cron 架構中 Skills、Python、no_agent 的角色釐清"\ntype: concept\nstatus: active\ntags: [auto, agent, linux]\ncreated: 2026-06-21\nupdated: 2026-06-25\n---

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
cron job (no_agent: true)
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
- [[investment-resources|股市分析系統使用說明]]
- [[skills/skills-index|Skills 目錄]]
## Architecture Roles


## Skill Archiving SOP

在刪除目錄前，必須先建立壓縮備份：

```bash
# 建立備份目標目錄
mkdir -p ~/.hermes/backups/skills/

# 打包 Skill 目錄 (將 `[skill-name]` 替換為實際名稱)
tar -czvf ~/.hermes/backups/skills/[skill-name]_$(date +%Y%m%d).tar.gz ~/.hermes/skills/[skill-name]
```

## 2. 移除階段 (系統清理)
使用 Hermes 內建工具進行正式移除，需註冊歸檔意圖：

```python
from hermes_tools import skill_manage

# 若 Skill 已釘選 (Pinned)，請先執行 `hermes curator unpin [skill-name]`
# 執行正式刪除
skill_manage(
    action='delete', 
    name='[skill-name]', 
    absorbed_into="" # 若無合併對象，填入空字串
)
```

## 3. 檢查階段 (清理相依 Cron)
在移除前，請務必先掃描是否有排程任務依賴此 Skill：

```python
from hermes_tools import cronjob
# 執行檢查
cronjob(action='list')
```

## 4. Wiki 沉澱 (結構維護)
根據「導航驅動記憶」規範，需在 Obsidian 完成以下變更：
- **`log.md`**：紀錄歸檔時間、原因、備份路徑 (e.g., `~/.hermes/backups/skills/...`)。
- **`index.md`**：移除該 Skill 的連結索引。
- **`GitHub`**：完成上述變更後，務必將更新同步推送到 `evanhsia-git/obsidian-vault`。

---
*註：本流程嚴格遵守「導航 → 執行 → 沉澱」規範。執行前請確認已取得 Ivan 的結構性變更審核許可。*

---
## 相關節點
- [[schema]]