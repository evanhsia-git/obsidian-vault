---
title: Lint 稽核規範
summary: Lint 稽核規範：**排除目錄（不參與任何分析）**
description: Obsidian Vault 圖譜健康檢查與自動化修復規範
type: concept
tags: [lint, audit, maintenance, rules]
created: 2026-06-21
updated: 2026-06-21
---

# Lint 稽核規範（obsidian-lint v1.4）

**排除目錄（不參與任何分析）**

`quartz/` · `node_modules/` · `quartz-site/`

**豁免目錄**

- `raw/` → 命名、Frontmatter、連結分析
- `ivan-notes/` → 命名、Frontmatter、連結分析
- `database/` `scripts/` `util/` `temp/` → 所有檢查

**稽核群組（評分 0–100）**

- A：Vault 架構 → 資料夾階層深度、歸檔流程、頁面類型比例
- B：中繼資料品質 → Frontmatter 完整性、Type/Tag 合法性、命名合規
- C：內容密度 → 單一主題原則、Chunk 長度、重複知識
- D：連結健康 → 雙向連結數、孤立頁面、失效連結
- E：知識可信度 → 無來源陳述、幻覺風險、檢索命中率
- F：系統就緒度 → Agent 記憶體、RAG 就緒評估

**P0 豁免條款**

孤立節點、損壞連結、Frontmatter 缺失 → Agent 可直接修復，無需審核，修復後立即 Append `log.md`。

**自動化修復對照**

- Frontmatter 缺失/不全 → 執行 `references/frontmatter-batch-repair.md`
- 檔名不合規 → `scripts/safe_rename_with_wikilinks.py --apply`
- 孤立頁面 → `scripts/find_orphans.py` → 掛回 MOC
- 標籤/值不合規 → `sed` 或 Python 批次替換
