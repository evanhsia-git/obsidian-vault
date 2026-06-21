---
title: schema
description: SCHEMA — 知識庫頁面
summary: SCHEMA
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

# Skill 管理與知識沉澱 SCHEMA

## 1. 核心規範
- [[schema]]
所有與 Hermes 技能（Skills）相關的報告、知識庫、作業過程、修正紀錄與執行結果，必須統一沉澱至：
`/root/Documents/Obsidian Vault/skills/`

## 2. 資料夾架構規範
每個 Skill 應建立對應的子資料夾，並遵循以下標準結構：
* `skills/<skill-name>/README.md`: Skill 知識庫首頁（含維護記錄、SCHEMA→index→log 執行規範）。
* `skills/<skill-name>/scripts/`: 關鍵作業腳本。
* `skills/<skill-name>/references/`: 參考文件、來源列表、cron 規範。
* `skills/<skill-name>/templates/`: 輸出格式範本。
* `skills/<skill-name>/log/`: 每日執行結果與執行狀態紀錄（每執行完畢自動歸檔）。

## 4. 錯誤隔離與超時處理機制 (Evolutionary Error Handling & Resilience)

1. **並發優化 (Concurrency)**: 所有外部請求應採用 `concurrent.futures.ThreadPoolExecutor` 進行並發處理，單一節點逾時不影響整體執行。
2. **限時交付 (Deadlines)**: 嚴格執行 `180s` 軟超時。當 `120s` 時，系統自動停止新任務派發，將已完成的 `Future` 結果立即輸出，確保任務不出現 `[SILENT]`。
3. **優雅降級 (Graceful Degradation)**: 抓取量級採取「分層動態適應」：
    - 第一層：關鍵資訊抓取 (限時 60s，必保)
    - 第二層：補充資訊抓取 (非同步並發，逾時自動丟棄)
    - 第三層：冗餘資訊清除 (最後階段處理)
4. **錯誤日誌記錄**: 任何失敗的 `web_extract` 需記錄至 `evolution_log.md` 以進行 GEPA 演化優化。
3. **交付條件**:
   - 若任務中止但已有部分資料，請將現有成果整理輸出。
   - 若任務因完全無資料或超時導致無結果，則回傳 `[SILENT]`。
4. **異常紀錄**: 所有執行超時或請求錯誤，必須在 `skills/<skill-name>/log/` 的當日紀錄檔中留下時間戳與錯誤代碼。

## 4. 維護原則
- **一致性**: 確保所有技能資訊皆歸檔於此處，不得散落在其他資料夾。
- **透明度**: 修正內容必須明確記錄時間與變更目的。
- **可讀性**: 嚴格執行 Traditional Chinese (繁體中文) 輸出，確保技術文件語意精準。