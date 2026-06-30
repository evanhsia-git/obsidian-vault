---
title: "Log"
description: "Obsidian Vault 維護日誌"
summary: "所有維護、修復、新增、刪除操作的記錄，含 database 結構重整"
type: log
status: active
tags: []
created: 2026-06-21
updated: 2026-06-23
---

# Obsidian Vault 維護日誌

## [2026-06-27] update | 新增 obsidian/ 資料夾 + tags 重整
- **原因**：Obsidian 主題頁面散落各處，需統一分類管理
- **動作**：
  - 建立 `obsidian/` 資料夾
  - 搬動 7 個 Obsidian 主題頁面至 `obsidian/`（obsidian-cms, obsidian-web-editing-solutions, obsidian-website-deployment, obsidian-wiki-conventions, obsidian-webdav-sync, vps-obsidian-blog-deployment, vps-obsidian-blog-deployment-v2）
  - 已搬動頁面移除 `obsidian` tag（路徑已表達分類）
  - 留在原地的非 Obsidian 主題頁面移除 `obsidian` tag（13 個檔案）
  - 更新 schema.md Layer 2 列表、folder-structure.md、index.md、obsidian-lint skill
  - 建立 `obsidian/obsidian-index.md`
- **結果**：obsidian/ 資料夾建立，7 頁搬動完成，tags 統一

## [2026-06-27] update | flow tag 重新命名為 workflow
- **原因**：統一 tag 命名，log.md 中 35 個核心標籤已使用 `workflow`，但 frontmatter-rules 和所有頁面 frontmatter 仍用 `flow`
- **動作**：將所有頁面 frontmatter 中的 `flow` tag 替換為 `workflow`（共 21 個檔案）
- **結果**：`flow` → `workflow` 全站統一

## [2026-06-27 12:00] database 結構重整 + wiki 新增 cron-list 頁面

**觸發**：用戶要求將 stock_fundamentals 合併至 stock_overview，daily_prices 只保留最新日期

**資料庫結構變更**：
- 刪除 `stock_fundamentals` 表格（1,054 筆資料已合併至 `stock_overview`）
- `stock_overview` 新增欄位：gross_margin, net_margin, debt_ratio, eps
- `daily_prices` 只保留最新日期（2026-06-25，1,368 rows）
- 新增日期表格：daily_prices_20260618、daily_prices_20260622、daily_prices_20260623、daily_prices_20260624（最多 5 個交易日）
- 資料庫路徑確認：`/root/Documents/database/tw_stock_all.db`

**Skill/腳本同步更新**：
- `init_db.py`：移除 stock_fundamentals 表格，stock_overview 新增 4 個基本面欄位
- `update_batch_2.py`：從寫入 stock_fundamentals 改為 UPDATE stock_overview
- `update_batch_3.py`：完整性檢查改為查 stock_overview 新欄位
- `update_batch_1.py`：移除 stock_fundamentals 表格建立
- `SKILL.md`：表格結構、自動化更新目標、Pitfall 等 8 處更新
- `references/batch_update_scripts.md`：batch_2 說明更新

**obsidian-lint 修正**：
- 白名單從 22 個 key 縮減至 8 個（只保留 schema 規範的 key）
- 修正後正確檢出 468 處違規（之前白名單太寬全部放過）
- 已批次清理所有違規 key（priority/aliases/date/publish/draft/related/source/due/review/version）

**新增 wiki 頁面**：
- `skills/cron-list.md`：Cron Job 列表，記錄 8 個排程任務的狀態、類型、執行時間與備註
- 已更新 skills-index.md 加入 cron-list 連結

## [2026-06-27 12:30] finance 目錄建立 + 20 篇筆記遷移

**觸發**：用戶要求在 Obsidian Vault 根目錄新增 finance/ 資料夾，將所有 tags 含 finance/tw-stock/stock 的筆記遷入

**執行動作**：
- 新增 `finance/` 目錄（Layer 2）
- 遷移 20 篇筆記：concepts/（11 篇）、entities/（6 篇）、queries/（3 篇）、skills/（1 篇）
- 新增 `finance/finance-index.md` 索引頁面
- 更新 `schema.md` Layer 2 目錄列表加入 `finance/`
- 更新 `index.md` 加入 finance 節點（20 篇）
- 更新原目錄 index：concepts-index（移除 11 篇）、entities-index（移除 6 篇）、queries-index（移除 3 篇）、skills-index（移除 1 篇）

**教訓**：大量搬移筆記時，必須同步更新所有相關 index 檔案，否則會產生孤立節點

**教訓**：
- lint 腳本白名單必須與 schema 規範完全一致，否則違規不會被抓到
- 資料庫結構變更時，所有 skill、腳本、references 必須同步更新

## [2026-06-25 10:00] 新增文章 + skill 整併 + 頁面搬移 + lint 修正 + WebDAV 權限修正 + Tags 規範
- **原因**：閱讀 Hermes Agent Curator 官方文件後用戶要求摘要並寫入 wiki；執行 daily-news 系列整併；skill 相關頁面搬移至 skills/；修正 lint 腳本缺陷；WebDAV 403 權限修正；Tags 規範更新
- **Tags 規範**：
  - 整個 Vault 核心 tag 上限從 35 個精簡至 **32 個**，禁止新增
  - 單一頁面最多 10 個 tag
  - 已更新 schema.md + frontmatter-rules.md + obsidian-lint skill 三處
  - 批次替換 132 個頁面的非核心 tag → 35 個核心列表
- **新增**: [[hermes-curator]]（concepts/）
- **整併**:
  - `daily-news-stock-market` → 合併至 `daily-stock-news`
  - `daily-news-unified` 更新為頂層聚合器
- **搬移**（7 個頁面從 concepts/entities → skills/）:
  - `concepts/skill-usage-protocol.md` → `skills/skill-usage-protocol.md`
  - `concepts/skill-script-architecture.md` → `skills/skill-script-architecture.md`
  - `concepts/obsidian-wiki-skill.md` → `skills/obsidian-wiki-skill.md`
  - `concepts/python-in-skill-implementation.md` → `skills/python-in-skill-implementation.md`
  - `concepts/cron-architecture-roles.md` → `skills/cron-architecture-roles.md`
  - `concepts/manus-use-cases.md` → `skills/manus-use-cases.md`
  - `entities/blave-quant-skill.md` → `skills/blave-quant-skill.md`
- **lint 腳本修正**:
  - 加入 `status:` 必填檢查
  - 加入 frontmatter key 白名單檢查
  - 加入 title/description/summary double quote 檢查
  - v3.2.8：完整 frontmatter 自動修正

---

## [2026-06-26 17:30] system-fix | obsidian-lint v3.4.0 + schema v3.4

**觸發**：Quartz GitHub Actions 部署失敗，`note-properties` 插件 `transformer.ts:169` 報 YAML 語法錯誤

**根本原因**：2 個 Vault 檔案（fred-economic-data.md, sp500-components.md）的 `tags:` 鍵缺失，列表項縮排在 `description` 下方，yaml.load() 拋出 `YAMLException` 導致 build 完全失敗

**修正項目**（三處同步）：
1. **obsidian-lint.py**：新增步驟 3b — `yaml.safe_load()` 全域結構驗證（遍所有 .md 檔案）
2. **schema.md**：
   - 版本 3.2 → 3.4
   - 新增 §3b「YAML 縮排結構錯誤 — key 缺失」完整規範
   - Frontmatter Safety 加入 key 缺失錯誤模式
3. **frontmatter-rules.md**：
   - 新增「YAML 結構完整性（必驗證）」章節
   - Success Criteria 加入「所有頁面皆是合法 YAML（通過 yaml.safe_load 驗證）」
4. **obsidian-lint skill**：更新到 v3.4.1，Pitfall 20 補充觸發場景，新增 Pitfall 21

**自動修復**：2 個檔案補回 `tags:` 鍵

**教訓**：`if 'tags:' in txt` 只能確認 key 字串存在，不能驗證結構合法性。必須用 `yaml.safe_load()` 做實際解析測試。
- **WebDAV 權限修正**:
  - `skills/` 目錄 403 問題：Agent 以 root 建檔，Nginx 以 www-data 運行 WebDAV
  - 已修正 `skills/` 全部子目錄權限為 `root:www-data` + `g+rwx`
  - **規範化**：schema.md 新增「WebDAV 寫入權限」章節、obsidian skill Post-creation checklist 加入步驟 0（chown + chmod）
  - 版本升至 v3.2.8
- **自動修復**: status 75 個、frontmatter 格式 149 個
- **結果**: 6 → 5 個新聞 skill；skill 相關頁面集中管理

## [2026-06-24 10:00] 新增文章
- **原因**：閱讀 Awesome DESIGN.md GitHub repo 後用戶要求摘要並寫入 wiki
- **新增**: [[awesome-design-md]]（concepts/）

## [2026-06-24 10:00] 新增文章
- **原因**：閱讀 Awesome DESIGN.md GitHub repo 後用戶要求摘要並寫入 wiki
- **新增**: [[awesome-design-md]]（concepts/）

## [2026-06-23 14:28:29] lint | 全部通過
- **原因**：定期巡檢
- **結果**：孤立節點 0/144、大型頁面 0、壞連結 0、Frontmatter 缺失 0

## [2026-06-23 14:22:02] lint | 全部通過
- **原因**：定期巡檢
- **結果**：孤立節點 0/144、大型頁面 0、壞連結 0、Frontmatter 缺失 0

## [2026-06-23 14:03:38] lint | 3 large_pages
- **原因**：定期巡檢
- **結果**：發現 3 個大型頁面（manus-use-cases、stock-data-sources、hermes-agent-backup），已拆分處理

## [2026-06-23 02:38:48] lint | 3 large_pages
- **原因**：定期巡檢
- **結果**：同上，待處理

## [2026-06-23 02:37:36] lint | 3 large_pages | 1 unsafe_summary
- **原因**：定期巡檢
- **結果**：發現大型頁面和 unsafe summary

## [2026-06-23 01:18:11] lint | 3 large_pages
## [2026-06-23 01:17:05] lint | 3 large_pages | 1 unsafe_summary
## [2026-06-23 01:16:07] lint | 6 missing_fields | 3 large_pages | 6 unsafe_summary
## [2026-06-23 01:01:36] lint | 6 missing_fields | 3 invalid_type | 3 invalid_status | 3 large_pages | 2 weak_hubs | 9 unsafe_summary
## [2026-06-23 01:00:35] lint | 6 missing_fields | 3 invalid_type | 3 invalid_status | 3 large_pages | 2 weak_hubs | 9 unsafe_summary

## [2026-06-22 04:14] migrate | Quartz content symlink → 實體資料夾
- **原因**：Quartz/content 為 symlink 指向 Vault，git 不追蹤內建檔案，Vault 變更無法觸發 GitHub Pages 更新
- **動作**：
  - 備份 symlink → content_symlink_20260622
  - 建立實體 content/ 資料夾
  - rsync Vault → Quartz/content（排除 .git/.obsidian/publish/ivan-notes）
  - 驗證 141 個 markdown 檔案數量一致
  - .gitignore 加入 content_symlink_*
- **結果**：147 files changed, 10458 insertions → push 成功（00f0dc8）

## [2026-06-22 06:17] fix | Quartz YAML frontmatter 修復
- **原因**：GitHub Actions build 失敗，4 個檔案 frontmatter 含 wikilink、1 個檔案 summary 含特殊字元未加 quote
- **動作**：
  - financial-preferences.md、fred-economic-data.md、nvidia-build-safety-models.md、sp500-components.md：將 `- [[openrouter-free-models]]` 從 frontmatter 移至 body
  - 2026-06-01-daily-task-summary.md：用 double quote 包裹 summary 值
  - karpathy-llm-wiki-gist.md：補上缺少的 summary
- **結果**：GitHub Actions 重新觸發成功

## [2026-06-21 16:23:21] lint | 全部通過
## [2026-06-21 15:55:24] lint | 孤立頁2
## [2026-06-21 15:41:54] lint | 孤立頁2
## [2026-06-21 15:32:06] lint | 缺失欄位8, 孤立頁2
## [2026-06-21 15:30:53] lint | 缺失欄位8, 孤立頁8
## [2026-06-21 15:23:50] lint | 缺失欄位8, 無效type6, 無效status57, 孤立頁26

## [2026-06-21 11:30] create | WebDAV Sync 資源頁面
- **原因**：整理 Obsidian WebDAV Sync 插件設定資訊
- **動作**：
  - 建立 `resources/obsidian-webdav-sync.md`
  - 更新 `resources/index.md`
- **結果**：資源索引 1 篇，WebDAV 設定資訊已記錄

## [2026-06-21 11:01:43] lint | 缺失欄位4, 無效type47, 標籤違規354, 孤立頁4

## [2026-06-21] fix | 恢復 SCHEMA.md version 2.1
- **原因**：merge conflict 導致 SCHEMA.md 內容錯誤
- **動作**：從 git commit `6c76e61`（schema 2.1 alignment）回復正確版本
- **結果**：SCHEMA.md 恢復為 version 2.1，336 行完整規範

## [2026-06-21] fix | 修復 user-backup.sh backup_repo 函數
- **原因**：push 失敗時 pull rebase 會卡住，導致後續操作全部失敗
- **動作**：修改 backup_repo 函數，pull rebase 失敗時自動 abort，然後 force push 以本機覆蓋遠端
- **結果**：備份流程不再因 rebase 衝突而卡住
- **原則**：GitHub 是備份區，VPS 本機是主資料，任何同步都以本機為準

## [2026-06-21] fix | 修復損壞連結與孤立頁面
- **原因**：Lint 報告 66 處損壞連結、14 個孤立頁面
- **動作**：
  1. 移除 43 個檔案中的無效連結
  2. 修正 9 處斷裂連結
  3. 移除不存在頁面連結
  4. 將 14 個孤立頁面加入對應 index.md
  5. 修復 index.md 和 log.md 的 Git merge conflict
- **結果**：損壞連結從 66 降至 ~10，孤立頁面從 14 降至 0

## [2026-06-21] fix | 修復 concepts/index.md 重複標題與殘留行號
- **原因**：💼 金融分析標題重複 3 次，第三次區塊歸類錯誤
- **動作**：刪除重複標題、將錯誤歸類項目移入系統架構區、清除殘留行號、補回 agent-driven-cronjobs
- **結果**：金融分析僅保留正確 6 項，統計表修正並跨年級統計

## [2026-06-21] create | 新增 Cron 架構角色分工概念頁
- **原因**：釐清 Skills、Python 腳本、no_agent 三者在每日台股 cron 中的角色
- **動作**：建立 `concepts/cron-architecture-roles.md`，更新 `concepts/index.md` 與主 `index.md`
- **結果**：新增 1 概念頁

## [2026-06-21] create | system/ 子檔案建立（6 個）
- **原因**：POLICY.md 規則路由器需要對應的 system/ 規範檔案
- **動作**：
  - system/frontmatter-rules.md：Frontmatter 必填欄位、type 10 個、status、confidence
  - system/folder-structure.md：目錄結構、讀寫權限、安全規範
  - system/database-rules.md：SQLite 操作、資料源、安全規範
  - system/backup-rules.md：Git 備份、還原原則
  - system/lint-rules.md：稽核群組、P0 豁免、自動化修復
  - system/quartz-rules.md：Quartz 5 部署流程
- **結果**：POLICY.md 路由完整，6 個子檔案各司其職

## [2026-06-21] upgrade | SCHEMA v2.2 → v3.1 + POLICY.md 新增
- **原因**：使用者提供新版 SCHEMA 3.1 與 POLICY.md，取代原有 v2.2
- **動作**：
  - SCHEMA.md 從 339 行精簡至 80 行（核心憲法）
  - 新增 POLICY.md（規則路由器，80 行）
  - 移除 Type Pool / Status Pool / Tag Taxonomy / Lint 等細節，改由 system/ 子檔案按需載入
  - 新增防失控機制（max_retry: 3 + [STOP]）
  - 安全規範簡化，新增 raw/ 唯讀保護
- **結果**：SCHEMA 3.1 + POLICY.md 雙檔案架構，最小載入最高效率

## [2026-06-21] update | SCHEMA.md v2.2 P1 精簡拆分
- **原因**：SCHEMA.md 394 行過長，每次對話 context 消耗大
- **動作**：
  - Lint 稽核規範 → `system/lint-audit-rules.md`
  - Telegram Output Rules → `system/telegram-output-rules.md`
  - SCHEMA.md 保留一行引用，從 394 行降至 339 行
- **結果**：SCHEMA.md 核心規範獨立，細節在 system/ 按需載入

## [2026-06-21] update | SCHEMA.md v2.2 第二輪修正
- **原因**：P0 不一致 + P2 優化
- **動作**：
  - P0: task type 適用目錄從 queries/ 改為各目錄
  - P0: Gatekeeping 標題從 USER 改回 Gatekeeping
  - P2: resource type 獨立 resources/ 目錄
  - P2: confidence 加入判斷標準（多源=high, 單一=medium, 推測=low）
  - 目錄結構新增 resources/
- **結果**：SCHEMA v2.2 完整一致

## [2026-06-21] update | SCHEMA.md v2.2 結構修正
- **原因**：Type Pool 與目錄結構、Page Types 不一致
- **動作**：
  - Type Pool：移除 comparison 與 navigation，補回 task（維持 10 個）
  - 目錄結構移除 comparisons/，wiki.md 從 navigation 改為 task
  - resource 適用目錄從 concepts/notes/ 改為 concepts/
  - Page Types 移除 comparison 與 navigation，補回 task
  - Gatekeeping 適用範圍移除 comparisons/
  - Frontmatter type 值同步更新
  - 已移除欄位說明加註 draft 欄位與 status 值的區分
  - 版本號 2.1 → 2.2
- **結果**：Type Pool、目錄結構、Page Types 三者一致，10 個 type 符合上限

## [2026-06-21] update | SCHEMA.md 標籤分類體系更新
- **原因**：統一標籤池，移除領域分組，改為平面 35 核心標籤
- **動作**：標籤分類體系更新為 35 個核心標籤（ai/llm/rag/embedding/vector-db/agent/automation/memory/prompt-engineering/hermes/skill/workflow/integration/telegram/obsidian/wiki/knowledge-management/markdown/quartz/flowershow/taiwan-stock/etf/valuation/financial-statement/data-source/linux/docker/nginx/vps/webdav/sshfs/backup/setup/optimization/maintenance），上限 50 個禁止再新增；Frontmatter tags 欄位說明同步更新；已移除欄位列表移除 status
- **結果**：標籤分類體系定稿 35 個核心標籤

## [2026-06-21] update | SCHEMA.md Status Pool 新增
- **原因**：規範頁面生命週期狀態，統一 status 欄位值
- **動作**：新增 Status Pool 章節（draft/active/permanent/archived/deprecated），上限 5 個禁止再新增；Frontmatter 標準範本加入 status 欄位；type 值同步更新
- **結果**：Status Pool 定稿 5 個，Frontmatter 規範同步更新

## [2026-06-21] update | SCHEMA.md Type Pool 更新
- **原因**：規範 type 數量上限，確保分類體系穩定
- **動作**：Type Pool 更新為 10 個 type（entity, concept, project, resource, report, query, index, log, schema, task），移除 comparison 與 navigation，加入 project/resource/report/task，設定上限 10 個禁止再新增
- **結果**：Type Pool 定稿 10 個，Page Types 規範同步更新

## [2026-06-21] fix | Lint 全面修復
- **原因**：SCHEMA v3.1 升級後，大量舊格式 frontmatter、壞連結、孤立節點需要修復
- **動作**：
  - index.md 更新：移除 comparisons、修正 queries/reports、補上 POLICY
  - 建立 resources/index.md、reports/index.md
  - log.md 壞連結修復（移除 [[wiki]]）
  - system/ 子檔案 frontmatter 修正（補 tags/summary）
  - 3 個無 frontmatter 檔案補齊（hermes-agent-rules, user-backup-skill, rss-test-report）
  - 66 個缺 summary 檔案自動批次補齊
  - system/index.md 更新（6 子檔案入站）
  - skills/index.md 更新（10 子檔案入站）
  - 110 個壞連結批次修復（55 個檔案）
  - concepts/index.md 全面更新（48 概念重新分類）
  - 孤立節點從 29 降至 0（補上 concepts/system/index、reports/index、system/index、skills/index）
- **結果**：index 完整、壞連結從 146 降至 0、孤立節點歸零

## [2026-06-21] fix | 壞連結清零
- **原因**：log.md 和 index.md 中仍有殘留壞連結
- **動作**：
  - index.md 移除 [[raw]]（raw.md 不存在）
  - concepts/safe-file-operations.md 移除 [[skills/safe-file-operations]]（skills 下無此檔案）
- **結果**：壞連結從 3 降至 0
## [2026-06-23 14:39:04] lint | 全部通過
## [2026-06-23 14:48:41] lint | 1 large_pages
## [2026-06-23 14:49:46] lint | 2 missing_fields | 1 large_pages
## [2026-06-23 14:50:38] lint | 1 large_pages
## [2026-06-23 14:52:02] lint | 1 weak_hubs
## [2026-06-23 14:52:58] lint | 全部通過
## [2026-06-23 15:34:01] lint | 1 large_pages
## [2026-06-23 15:35:53] lint | 全部通過
## [2026-06-23 21:00:48] lint | 全部通過
## [2026-06-24 21:00:54] lint | 全部通過
## [2026-06-25 09:22:27] lint | 全部通過
## [2026-06-25 09:50:06] lint | 75 missing_fields | 39 invalid_type | 1 weak_hubs
## [2026-06-25 09:51:37] lint | 75 missing_fields | 39 invalid_type | 1 weak_hubs
## [2026-06-25 09:56:22] lint | 1 missing_fields | 1 weak_hubs
## [2026-06-25 09:57:11] lint | 全部通過
## [2026-06-25 12:31:14] lint | 全部通過
## [2026-06-25 21:00:05] lint | 全部通過
## [2026-06-26 00:34:17] lint | 全部通過
## [2026-06-26 00:55:35] lint | 全部通過
## [2026-06-26 00:56:42] lint | 全部通過
## [2026-06-26 05:06:22] lint | 全部通過
## [2026-06-26 05:34:55] lint | 全部通過
## [2026-06-26 11:26:14] lint | 全部通過
## [2026-06-26 11:34:41] lint | 468 invalid_type
## [2026-06-26 11:43:07] lint | 全部通過

## [2026-06-26 13:50] 整理
- `skill-usage-protocol` 更名為 `skills-rules`，移動至 `system/` 資料夾，type 改為 `system`
- `quartz-v5-deployment` 合併至 `quartz-rules`（部署經驗保留，重複的刪除）
- 更新 `system/system-index.md`、`skills/skills-index.md` 索引

## [2026-06-26 12:00] finance 目錄新增
- wiki 新增 `finance/` 目錄（Layer 2）
- 遷移 20 篇筆記（concepts 11 + entities 6 + queries 3 + skills 1）
- 更新 `schema.md`、`index.md`、4 個原目錄 index、`log.md`
## [2026-06-26 15:06:32] lint | 2 invalid_type | 3 weak_hubs
## [2026-06-26 21:00:12] lint | 1 invalid_type | 3 weak_hubs
## [2026-06-27 07:07:07] lint | 2 invalid_type | 4 orphans | 3 weak_hubs
## [2026-06-27 07:32:36] lint | 1 invalid_names | 4 missing_fields | 3 invalid_type | 1 orphans | 3 weak_hubs
## [2026-06-27 07:43:18] lint | 1 large_pages | 1 orphans | 3 weak_hubs
## [2026-06-27 07:55:35] lint | 1 large_pages | 1 orphans | 3 weak_hubs
## [2026-06-27 08:16:47] lint | 3 weak_hubs
## [2026-06-27 08:26:19] lint | 1 large_pages | 2 weak_hubs
## [2026-06-27 08:38:48] lint | 1 large_pages | 2 weak_hubs
## [2026-06-27 08:42:32] lint | 1 large_pages | 2 weak_hubs
## [2026-06-27 08:46:48] lint | 1 large_pages
## [2026-06-27 10:56:13] lint | 1 large_pages
## [2026-06-27 12:01:56] lint | 8 large_pages | 2 orphans | 1 weak_hubs
## [2026-06-27 13:13:23] lint | 1 orphans | 9 weak_hubs
## [2026-06-27 13:19:53] lint | 全部通過
## [2026-06-27 13:19:57] lint | 全部通過
## [2026-06-27 21:00:53] lint | 全部通過
## [2026-06-28 09:19:25] lint | 全部通過

## [2026-06-28 10:00] schema 更新 — 新增 Page Creation Pre-check 規範
- **原因**：避免重複頁面，確保新增前先檢查 vault 是否有相似內容
- **動作**：schema.md 新增「Page Creation Pre-check」章節（3 步驟：掃描→回報→依決定執行），obsidian-lint skill pitfalls 同步更新
- **結果**：新增頁面強制查重流程確立

## [2026-06-28 10:30] Telegram 表格限制解除
- **原因**：新版 Telegram 已解決表格顯示問題
- **動作**：
  - `telegram-output-rules.md`：移除 Markdown Table / HTML Table 禁令，標註表格已確認相容
  - `SOUL.md`：Telegram 輸出規範允許 pipe table，移除 25 行/5 項限制
  - `User Profile` memory：移除 "no pipe tables" 限制
- **結果**：Telegram 輸出可自由使用表格

## [2026-06-28 11:10] WebDAV 權限修復 + Schema 更新
- **根因**：write_file 新建檔案預設 root:root 644/600，www-data 無寫入權限 → 403
- **修復**：
  - 修復 25 個問題檔案（chown root:www-data + chmod 664）
  - schema.md 更新 WebDAV 章節（修正 chmod g+rwx → 664、加批次修復指令、標註 umask 002 預防）
  - ~/.bashrc 設定 `umask 002`（新檔案預設 664，根治問題）
- **結果**：69 個 md 檔案全部 root:www-data 664，WebDAV 應可正常同步

## [2026-06-28 11:20] Schema 新增 copilot/.claude/.claudian 保護區
- **原因**：外部工具（Copilot、Claude）管理的資料夾，Agent 不應修改
- **動作**：schema.md Safety Rules 新增「禁止編輯、移動、刪除 copilot/ .claude/ .claudian/」
- **結果**：外部工具區受保護

## [2026-06-28 16:00] 新增 Telegram Dashboard 架構分析報告
- **原因**：分析現有 Telegram Bot 架構，為 Dashboard v1 設計做準備
- **動作**：`reports/telegram-dashboard-architecture.md` — 完整分析 10 個關鍵檔案、5 類 Handler、6 種 callback 協議
- **結論**：建議新增 3 個檔案（telegram_dashboard.py、telegram_states.py、telegram_menu.py），adapter.py 只加前綴判斷
- **參考**：分析基於 `python-telegram-bot` library，不修改任何程式

## [2026-06-28 15:40] folder-rules 新增 assets/ 目錄
- **動作**：`folder-rules.md` 目錄樹與讀寫權限表新增 `assets/`（全站靜態資源）
- **結果**：assets/ 正式納入目錄結構

## [2026-06-28 15:30] tags 對照表升級為 4 欄格式
- **原因**：原 1 欄「適用場景」資訊不足，參考 `obsidian/Obsidian-tags-Optimization-Recommendations.md` 升級為 4 欄（Tag、類別、用途、關鍵詞/涵蓋內容）
- **動作**：
  - `frontmatter-rules.md` tags 章節全面重寫為 4 欄表格
  - `obsidian-lint` skill pitfalls 第 22 條同步
- **結果**：48 個核心 tag 都有完整 4 欄定義

## [2026-06-28 15:00] 新增 ivan-notes 資料夾全面保護
- **原因**：ivan-notes 資料夾應唯讀保護
- **動作**：`schema.md` Safety Rules 將 `ivan-notes/` 從「禁止修改」提升為「禁止編輯、移動、刪除」等級
- **結果**：ivan-notes/ 需經使用者同意才能變動

## [2026-06-28 15:00] 新增 ivan 檔案保護規則
- **原因**：使用者名稱相關檔案應受保護，避免誤修改
- **動作**：
  - `schema.md` Safety Rules 新增「ivan 檔案保護」：檔名含 `ivan` 或 `tags` 含 `ivan` 的頁面，Agent 禁止編輯/移動/刪除，變動需經使用者同意
  - `obsidian-lint` skill pitfalls 第 21 條同步
- **結果**：ivan 相關檔案受保護

## [2026-06-28 11:00] Telegram 所有限制解除
- **原因**：Telegram 新版已全面支援標準 Markdown
- **動作**：
  - `telegram-output-rules.md`：整頁重寫為 639 字，僅保留「可使用 telegram-message-file-sender」和「可用 send_message MEDIA」
  - `SOUL.md`：Telegram 規範簡化為「已全面支援標準 Markdown，無格式限制」
  - `User Profile` memory：更新為「無格式限制」
- **結果**：所有格式限制（巢狀列表、標題行數、代碼塊、MathJax 等）全部解除

## [2026-06-28 10:00] 小頁面合併 — 11 頁 → 4 頁
- **原因**：11 個 <100 行小型頁面分散，知識密度低，合併後提升可讀性
- **動作**：
  - 合併 `next-ai-draw-io` + `llm-wiki-comparison` + `superpowers` → `concepts/ai-toolkit`
  - 合併 `cloudflare-integration` + `telegram-interactive-ui` + `tech-sources-monitoring` → `concepts/external-services-integration`
  - 合併 `daily-news-technology-example` + `daily-news-twstock-example` → `entities/news-and-market-examples`
  - 合併 `etf-basic-info` + `stock-analysis-system-guide` + `investment-master-strategy` → `finance/investment-resources`
  - 修復 12 處 broken links（most-blog, daily-news-usstock-fix, agent-driven-cronjobs, daily-news-technology, hermes-agent-strategy, manus-use-cases, blogwatcher/index, code-quality-analysis, superpowers-reference）
  - 更新 `concepts/concepts-index`、`entities/entities-index`、`finance/finance-index`、`skills/skills-index`
- **結果**：11 頁減至 4 頁，總頁面數從 83 降至 76（含唯讀）/ 77 降至 70（不含唯讀）
## [2026-06-28 10:11:23] lint | 1 orphans
## [2026-06-28 10:11:41] lint | 全部通過
## [2026-06-28 10:30:35] lint | 全部通過
## [2026-06-28 10:32:21] lint | 全部通過
## [2026-06-28 10:33:20] lint | 全部通過
## [2026-06-28 10:45:12] lint | 16 invalid_names | 106 missing_fields | 78 invalid_type | 16 orphans
## [2026-06-28 14:58:35] lint | 16 invalid_names | 79 missing_fields | 75 invalid_type | 1 invalid_status | 21 orphans | 1 weak_hubs
## [2026-06-28 15:10:34] lint | 16 invalid_names | 3 missing_fields | 15 orphans
## [2026-06-28 15:13:46] lint | 16 invalid_names | 78 missing_fields | 15 orphans
## [2026-06-28 15:15:41] lint | 1 invalid_names | 3 missing_fields
## [2026-06-28 15:23:59] lint | 1 invalid_names | 3 missing_fields
## [2026-06-28 15:26:24] lint | 全部通過
## [2026-06-29 20:00] 新增 wiki 頁面 — 15 種量化策略完整指南
- **原因**：用戶詢問常見量化策略種類，需建立知識基礎供後續選股系統開發
- **動作**：
  - 建立 `finance/quant/quant-trading-strategies-guide.md` — 15 種策略分類（趨勢動量 4 + 均值回歸 3 + 因子投資 4 + 事件驅動 2 + 多因子複合 2）
  - 推薦方案：多因子選股（價值 0.4 + 動量 0.3 + 品質 0.3），從前 300 大每日選 10 檔
  - 更新 `finance/finance-index.md` + `index.md`
- **結果**：7.7 KB 知識頁面，含完整評分公式、所需欄位、輸出格式範例

## [2026-06-29 20:30] 新增 spec 文件 — Daily Stock Picker 設計規格
- **原因**：用戶要建立「每日 15 檔選股推薦」skill，使用 superpowers brainstorming 流程
- **動作**：
  - 建立 `finance/quant/daily-stock-picker-spec.md` — 完整設計規格
  - 評分模型：1-10 分 + S/A/B/C 等級，多因子百分位排名加權
  - 三期程：短期（低PB+高量+ROE）/ 中期（高ROE+高毛利+低負債）/ 長期（高殖利率+低PE+EPS穩定）
  - 路線圖：Phase 1 Skill+CLI → Phase 2 60天歷史+動量 → Phase 3 FastAPI 動態網頁
  - DB 擴充：歷史從 5 天延長至 60 天
- **結果**：6.3 KB spec 文件，待用戶確認後實作

## [2026-06-28 21:00:29] lint | 1 invalid_type

## [2026-06-29 23:30] Superpowers 流程規範寫入四處
- **原因**：用戶確認 quant-trading skill 建立時未使用 Superpowers 7 階段流程，要求未來寫新 Skill 時強制遵循
- **動作**：
  - 更新 `SOUL.md`：新增「⚡ Superpowers 流程」區塊，6 階段 + 紅線
  - 更新 `system/skills-rules.md`：新增 Superpowers 表格 + HARD GATE + SDO 標準
  - 更新 `schema.md`：新增「# Superpowers 流程」區塊（schema 是最高規範）
  - 更新 `MEMORY.md`：新增 Superpowers 流程速查卡 + quant-trading 歷史資料完成紀錄
- **結果**：三處同步（SOUL + schema + skills-rules）+ memory，未來寫 Skill 時自動遵循
## [2026-06-29 21:00:40] lint | 全部通過
## [2026-06-30 02:17:41] lint | 全部通過
## [2026-06-30 02:22:53] lint | 全部通過
## [2026-06-30 04:58:03] lint | 全部通過
