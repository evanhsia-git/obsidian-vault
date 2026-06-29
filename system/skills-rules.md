---
title: "Skills 規則"
description: "所有 Skill 的建立、修改、整併、封存規範與標準流程 + 知識沉澱架構"
summary: "Skills 管理規則：建立/修改/整併/封存 + 知識沉澱架構 + 錯誤隔離機制"
type: concept
status: active
tags: [hermes, agent, workflow]
created: 2026-06-10
updated: 2026-06-27
---

## Skills 使用規範 (Skills Rules)

**核心原則**：新建技能前必須先檢視 Obsidian Vault 中的技能清單，優先使用現有技能進行調整與修改。

**檢視流程**：執行任務前先調用 skills-list 技能檢查現有技能，確定無適用技能時才考慮新建。

**新增程序**：新建技能前必須明確詢問使用者確認，並說明新建的必要性與現有技能的不足之處。

**替代方案**：優先考慮修改現有技能而非新建，可提供多種調整策略供使用者選擇。

**記錄要求**：所有技能操作都必須在 Obsidian Vault 中留下記錄，包括技能修改原因與效果評估。

## ⚡ Superpowers 流程（寫新 Skill 強制遵循）

**順序不可逆，禁止跳步，每階段結束必須停止等用戶確認：**

| 階段 | Skill | 必須做到 | 🔴 絕對禁止 |
|------|-------|----------|------------|
| 1 | brainstorming | 探索需求→提問→2-3方案→用戶批准設計 | 未批准前寫任何 code |
| 2 | writing-plans | bite-sized TDD 任務，完整程式碼+驗證指令+預期結果 | TBD / placeholder / 「稍後實作」 |
| 3 | executing-plans | 逐 task RED→GREEN→REFACTOR | 跳步、跳過驗證 |
| 4 | writing-skills | TDD for docs + SDO（description 只寫觸發條件） | 沒測試就部署 skill |
| 5 | verification | 跑指令→看輸出→才宣稱完成 | 「應該可以」「看起來沒問題」 |
| 6 | finishing | 測試全過→commit/PR | 沒驗證就 commit |

**HARD GATE**：brainstorming 用戶批准前，禁止任何實作行為（包含 scaffold、寫檔案、建目錄）。

**SDO 標準**：Skill description 只描述「何時使用」（觸發條件），不描述「做什麼」（流程摘要）。

## Agent 表達範例 (Agent Expression Examples)

**技能檢視表達**：在執行任務前，我會先檢視 Obsidian Vault 中的現有技能清單，尋找最適用的技能進行調整。

**新建詢問表達**：經檢視現有技能後，若無適用技能，我會向您說明：「我已檢視現有技能，發現 [具體不足]，建議新建技能 [名稱] 來處理此任務，請問您是否同意？」

**替代方案表達**：「除了新建技能外，我也可以修改現有的 [技能名稱] 來滿足需求，您偏好哪種方式？」

**記錄更新表達**：「技能操作已完成，我已在 Obsidian Vault 中記錄了修改原因：[具體原因] 和效果評估：[具體評估]。」

## Skill 知識沉澱架構

所有與 Hermes 技能（Skills）相關的報告、知識庫、作業過程、修正紀錄與執行結果，必須統一沉澱至：
`/root/Documents/Obsidian Vault/skills/`

### 資料夾架構規範

每個 Skill 應建立對應的子資料夾，並遵循以下標準結構：
* `skills/<skill-name>/README.md`: Skill 知識庫首頁（含維護記錄、SCHEMA→index→log 執行規範）。
* `skills/<skill-name>/scripts/`: 關鍵作業腳本。
* `skills/<skill-name>/references/`: 參考文件、來源列表、cron 規範。
* `skills/<skill-name>/templates/`: 輸出格式範本。
* `skills/<skill-name>/log/`: 每日執行結果與執行狀態紀錄（每執行完畢自動歸檔）。

### 維護原則

- **一致性**: 確保所有技能資訊皆歸檔於此處，不得散落在其他資料夾。
- **透明度**: 修正內容必須明確記錄時間與變更目的。
- **可讀性**: 嚴格執行 Traditional Chinese (繁體中文) 輸出，確保技術文件語意精準。

## 錯誤隔離與超時處理機制 (Evolutionary Error Handling & Resilience)

1. **並發優化 (Concurrency)**: 所有外部請求應採用 `concurrent.futures.ThreadPoolExecutor` 進行並發處理，單一節點逾時不影響整體執行。
2. **限時交付 (Deadlines)**: 嚴格執行 `180s` 軟超時。當 `120s` 時，系統自動停止新任務派發，將已完成的 `Future` 結果立即輸出，確保任務不出現 `[SILENT]`。
3. **優雅降級 (Graceful Degradation)**: 抓取量級採取「分層動態適應」：
    - 第一層：關鍵資訊抓取 (限時 60s，必保)
    - 第二層：補充資訊抓取 (非同步並發，逾時自動丟棄)
    - 第三層：冗餘資訊清除 (最後階段處理)
4. **錯誤日誌記錄**: 任何失敗的 `web_extract` 需記錄至 `evolution_log.md` 以進行 GEPA 演化優化。
5. **交付條件**:
   - 若任務中止但已有部分資料，請將現有成果整理輸出。
   - 若任務因完全無資料或超時導致無結果，則回傳 `[SILENT]`。
6. **異常紀錄**: 所有執行超時或請求錯誤，必須在 `skills/<skill-name>/log/` 的當日紀錄檔中留下時間戳與錯誤代碼。

## 相關節點

- [[skills-list]]
- [[skills-rules]]
- [[index]]
