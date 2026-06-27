---
title: "hermes-hierarchy"
description: "hermes-hierarchy — 概念說明頁面"
summary: "hermes-hierarchy"
type: concept
status: active
tags: [hermes]
created: 2026-06-10
updated: 2026-06-10
---

my boss,
                      ▼
                Hermes Agent
         （首席分析師／總經理）
                      │
              ┌────────────┼────────────┐
              │            │            │
   ▼            ▼            ▼
   USER.md    MEMORY.md    SQLite
   客戶檔案    公司備忘錄   工作系統
   │            │            │
   └──────┬─────┴─────┬──────┘
          │           │
          ▼           ▼
         LLM Wiki
         （研究資料庫）
               │
               ▼
             Obsidian
            （圖書館／檔案館）

相關頁面：[[otc-company-profile-2026-06-02]]


## 相關節點
- [[index]]

## Architecture
---
title: "Hermes Agent 記憶架構與階層圖"
description: "Hermes Agent 記憶架構與階層圖 — 概念說明頁面"
summary: "Hermes Agent 記憶架構與階層圖"
type: concept
status: active
tags: [hermes, agent]
created: 2026-06-05
updated: 2026-06-05
---

# Hermes Agent 記憶架構與階層圖

本頁面彙整了與 Ivan 討論的「三層記憶與階層式指揮」體系，確立了 Agent 的權限與儲存分工。

## 階層式架構示意

此結構定義了由「老闆」(使用者) 到 Agent 的指令傳遞，以及 Agent 對記憶資源的調度權限：

```text
       老闆 (Ivan)
            │
            ▼
      Hermes Agent
    (首席分析師／總經理)
            │
  ┌─────────┼─────────┐
  │         │         │
  ▼         ▼         ▼
USER.md   MEMORY.md  SQLite
客戶檔案  公司備忘錄  工作系統
  │         │         │
  └────┬────┴────┬────┘
       │         │
       ▼         ▼
       LLM Wiki
     (研究資料庫)
       │
       ▼
    Obsidian
  (圖書館／檔案館)
```

## 階層職責說明

| 階層 | 組件 | 職責 |
| :--- | :--- | :--- |
| **決策層** | **老闆 (Ivan)** | 指令發布者，負責確認系統執行方向與審核大規模結構變更。 |
| **執行層** | **Hermes Agent** | 首席分析師，負責調度所有記憶資源，確保任務閉環。 |
| **Fact 層** | **USER.md / MEMORY.md** | 儲存核心事實、個人化設定與靜態偏好，作為執行動作的依據。 |
| **State 層** | **SQLite** | 記錄當前時序狀態、日誌、對話紀錄，作為執行過程的基石。 |
| **Knowledge 層**| **LLM Wiki / Obsidian** | 存放深度知識圖譜、研究成果、歷史文件。這是代理人的外部大腦，負責提供執行時的脈絡與定義。 |

## 運作協同原則
1. **指令流**：指令由「老闆」下達，由「Hermes Agent」統一調度底層資源。
2. **記憶流**：
   - 變更事實與偏好 -> 寫入 `USER.md` / `MEMORY.md`。
   - 記錄工作時序與進度 -> 寫入 `SQLite`。
   - 沉澱知識與研究總結 -> 寫入 `Obsidian Wiki`。
3. **一致性**：任何結構性變更 (Wiki 目錄) 需由老闆確認，確保與 Agent 運作脈絡的一致性。


相關頁面：[[awesome-github-resources]]

相關頁面：[[model-error-messages]]

相關頁面：[[byterover-summary]]

## 相關節點
- [[index]]

## Architecture

## 階層式架構示意

此結構定義了由「老闆」(使用者) 到 Agent 的指令傳遞，以及 Agent 對記憶資源的調度權限：

```text
       老闆 (Ivan)
            │
            ▼
      Hermes Agent
    (首席分析師／總經理)
            │
  ┌─────────┼─────────┐
  │         │         │
  ▼         ▼         ▼
USER.md   MEMORY.md  SQLite
客戶檔案  公司備忘錄  工作系統
  │         │         │
  └────┬────┴────┬────┘
       │         │
       ▼         ▼
       LLM Wiki
     (研究資料庫)
       │
       ▼
    Obsidian
  (圖書館／檔案館)
```

## 階層職責說明

| 階層 | 組件 | 職責 |
| :--- | :--- | :--- |
| **決策層** | **老闆 (Ivan)** | 指令發布者，負責確認系統執行方向與審核大規模結構變更。 |
| **執行層** | **Hermes Agent** | 首席分析師，負責調度所有記憶資源，確保任務閉環。 |
| **Fact 層** | **USER.md / MEMORY.md** | 儲存核心事實、個人化設定與靜態偏好，作為執行動作的依據。 |
| **State 層** | **SQLite** | 記錄當前時序狀態、日誌、對話紀錄，作為執行過程的基石。 |
| **Knowledge 層**| **LLM Wiki / Obsidian** | 存放深度知識圖譜、研究成果、歷史文件。這是代理人的外部大腦，負責提供執行時的脈絡與定義。 |

## 運作協同原則
1. **指令流**：指令由「老闆」下達，由「Hermes Agent」統一調度底層資源。
2. **記憶流**：
   - 變更事實與偏好 -> 寫入 `USER.md` / `MEMORY.md`。
   - 記錄工作時序與進度 -> 寫入 `SQLite`。
   - 沉澱知識與研究總結 -> 寫入 `Obsidian Wiki`。
3. **一致性**：任何結構性變更 (Wiki 目錄) 需由老闆確認，確保與 Agent 運作脈絡的一致性。


相關頁面：[[awesome-github-resources]]

相關頁面：[[model-error-messages]]

相關頁面：[[byterover-summary]]


## 相關節點
- [[index]]
