---
title: "evolution_log"
description: "evolution_log — 技能說明頁面"
summary: "evolution_log"
type: concept
status: active
tags: [hermes]
created: 2026-06-10
updated: 2026-06-10
---

# 自動化進化實驗紀錄 (Self-Evolution Log)

本頁面記錄 Hermes Agent 在執行自動化任務（如新聞監控、資料抓取）時，觸發的自我優化行為與效能基準紀錄。

## 1. 自動化進化目標
利用「錯誤日誌」與「執行效率」作為反饋迴路，訓練系統自動調整抓取策略，無需人類手動介入 Patch。

## 2. 實驗項目：新聞監控系統 (News Monitoring)
- **目標**: 提高新聞抓取成功率、降低執行逾時 (Timeout)。
- **監控指標 (Reward Function)**:
    - 抓取成功率 (HTTP 200 比例)
    - 任務完成耗時 (執行是否低於 180s)
    - 資料格式正確性 (是否符合 `**[標題]**：摘要：[連結]` 規範)

## 3. 進化日誌 (Evolution Log)
| 時間 | 觸發事件 | 自動修復行為 | 結果 |
| :--- | :--- | :--- | :--- |
| 2026-06-10 | CnBeta RSS DNS 解析失敗 | 觸發錯誤報告，並標記來源為「弱穩定性」，降低後續掃描優先級 | 已自動繞過該節點，系統繼續執行其他 21 個來源 |
| 2026-06-10 | 執行超時 (Timeout) | 強制中斷並執行「異常輸出」機制 | 保留已獲取的片段資訊，未導致整體流程崩潰 |
| 2026-06-11 | 使用者手動演化 4 個新聞技能 | 修正 `evolve_skill.py` 約束驗證 bug 後批次演化（MIPROv2, 3 iters, openrouter/openai/gpt-4o-mini） | 4 技能全數通過約束驗證，holdout 分數均無提升（見下表） |

## 3.1 2026-06-11 實測結果（4 新聞技能）

> 入口：`python -m evolution.skills.evolve_skill --skill <name> --iterations 3 --eval-source synthetic --optimizer-model openrouter/openai/gpt-4o-mini --eval-model openrouter/openai/gpt-4o-mini`

| 技能 | Baseline → Evolved (holdout) | 約束驗證 | 產出目錄 |
| :--- | :--- | :--- | :--- |
| daily-news-twstock | 0.300 → 0.300 (+0.000) | ✓ 全通過 | output/daily-news-twstock/20260611_075802/ |
| daily-news-usstock | 0.300 → 0.300 (+0.000) | ✓ 全通過 | output/daily-news-usstock/20260611_080037/ |
| daily-news-stock-market | 0.384 → 0.384 (+0.000) | ✓ 全通過 | output/daily-news-stock-market/20260611_080509/ |
| daily-news-technology | 通過驗證（holdout 未提升） | ✓ 全通過 | output/daily-news-technology/ |

**關鍵發現**：
1. **約束 bug 已修**：原 `evolve_skill.py` 第 189 行用 `evolved_body`（不含 frontmatter）做結構檢查，必然回報「缺 YAML frontmatter」而 FAILED。已改為驗證 `evolved_full`（含 frontmatter）+ baseline 用 `skill["raw"]` 對等比較。修正後 4 技能全部通過。
2. **GEPA 不可用**：dspy 3.2.1 無 GEPA，實際 fallback 至 **MIPROv2**。
3. **模型實況**：程式碼預設 `openai/gpt-4.1`，但環境僅有 `OPENROUTER_API_KEY`；改用 `openrouter/openai/gpt-4o-mini` 可正常執行。
4. **分數未提升**：合成資料集（synthetic）+ 3 iterations 下，holdout 分數均持平。原因可能為迭代數過少、合成評估與真實偏好脫節。建議改用 `--eval-source sessiondb` 從真實對話挖掘範例，或提高 iterations。
5. **安全特性**：未通過約束即不部署（存 `evolved_FAILED.md`），原 SKILL.md 不被覆蓋。「回滾」描述應更正為「不覆蓋」。

## 3.2 2026-06-11 sessiondb 演化（修 importer bug 後成功）

**importer 修正**：`external_importers.py` 的 `HermesSessionImporter` 原本：
- 只 glob `*.json`（實際 session 是 `*.jsonl`，每行一個訊息物件）→ 挖到 0 筆
- 整檔 `json.loads` 對 `.jsonl` 與 `request_dump_*.json`（非標準）會崩潰
- `read_text()` 未容錯，遇非 UTF-8 位元組炸 `UnicodeDecodeError`

已修正：支援 `.jsonl` 逐行解析、`.json` 分支加 `JSONDecodeError`/`isinstance` 容錯、`read_text(errors="ignore")`。修正後從 244 個 session 挖到 842 則訊息。

**daily-news-twstock 演化結果**（sessiondb，5 iters, gpt-4o-mini）：
- [[architecture-workflow]]
- 挖掘：842 訊息 → 篩 148 候選 → LLM 評分留 17 相關範例（train 8 / val 4 / holdout 5）
- 優化：MIPROv2 最佳分數 30.56 → 31.14
- **Holdout: 0.312 → 0.319（+0.007, +2.2%）— 首次正向提升**
- diff：演化版 SKILL.md 內文與 baseline **幾乎相同**（僅結尾換行）
- **結論**：提升來自 few-shot 選擇，非改寫指令。代表現有 SKILL.md 指令已接近最優，手動調整的格式規範無需再改。
- 產出：`output/daily-news-twstock/20260611_084422/`

## 4. 待進化清單 (Pending Evolutions)
- [ ] 增加對「無效/死鏈結」的自動清洗功能。
- [ ] 若某來源連續 3 天回傳 `[SILENT]`，自動暫停並通知人類審查來源有效性。
- [ ] 動態調整 `blogwatcher` 的掃描併發數 (Workers) 以適應不同網路環境。

---
*本紀錄為 `Self-Evolution` 實驗的一環，記錄 Agent 如何在處理新聞監控任務中進行自我學習與修復。*


## 相關節點
- [[index]]