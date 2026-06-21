---
title: Log
description: Obsidian Vault 維護日誌
summary: 所有維護、修復、新增、刪除操作的記錄
type: log
status: active
tags: [system, log]
created: 2026-06-21
updated: 2026-06-21
---

# Obsidian Vault 維護日誌

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

## [2026-06-21] create | system/ 子檔案建立（6 個）

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

## [2026-06-21] create | 新增 Cron 架構角色分工概念頁
- **原因**：釐清 Skills、Python 腳本、no_agent 三者在每日台股 cron 中的角色
- **動作**：建立 `concepts/cron-architecture-roles.md`，更新 `concepts/index.md` 與主 `index.md`
- **結果**：新增 1 概念頁

## [2026-06-21] fix | 修復 concepts/index.md 重複標題與殘留行號
- **原因**：💼 金融分析標題重複 3 次，第三次區塊歸類錯誤
- **動作**：刪除重複標題、將錯誤歸類項目移入系統架構區、清除殘留行號、補回 agent-driven-cronjobs
- **結果**：金融分析僅保留正確 6 項，統計表修正並跨年級統計

## [2026-06-21] fix | 修復損壞連結與孤立頁面
- **原因**：Lint 報告 66 處損壞連結、14 個孤立頁面
- **動作**：
  1. 移除 43 個檔案中的 `` 無效連結
  2. 修正 `` → `[[daily-news-sources-rss]]` 等 9 處斷裂連結
  3. 移除 ``、`` 等不存在頁面連結
  4. 將 14 個孤立頁面加入對應 index.md
  5. 修復 index.md 和 log.md 的 Git merge conflict
- **結果**：損壞連結從 66 降至 ~10，孤立頁面從 14 降至 0

## [2026-06-21] fix | 修復 user-backup.sh backup_repo 函數
- **原因**：push 失敗時 pull rebase 會卡住，導致後續操作全部失敗
- **動作**：修改 backup_repo 函數，pull rebase 失敗時自動 abort，然後 force push 以本機覆蓋遠端
- **結果**：備份流程不再因 rebase 衝突而卡住
- **原則**：GitHub 是備份區，VPS 本機是主資料，任何同步都以本機為準

## [2026-06-21] fix | 恢復 SCHEMA.md version 2.1
- **原因**：merge conflict 導致 SCHEMA.md 內容錯誤
- **動作**：從 git commit `6c76e61`（schema 2.1 alignment）回復正確版本
- **結果**：SCHEMA.md 恢復為 version 2.1，336 行完整規範

  標籤警告明細（354 處）:
    - POLICY.md: policy
    - POLICY.md: router
    - POLICY.md: hermes
    - POLICY.md: wiki
    - cnn-fear-and-greed-analysis.md: entity
    - daily-news-sources-rss.md: data-source
    - daily-news-sources-rss.md: obsidian
    - daily-news-sources-rss.md: maintenance
    - otc-company-profile-2026-06-02.md: otc
    - otc-company-profile-2026-06-02.md: company
    - otc-company-profile-2026-06-02.md: basic-data
    - trading-agents.md: ai
    - trading-agents.md: llm
    - trading-agents.md: agents
    - trading-agents.md: trading
    - trading-agents.md: quantitative-trading
    - awesome-github-resources.md: hermes
    - awesome-github-resources.md: concept
    - icdyct-blog.md: hermes
    - us-cpi-analysis.md: entity
    ... 還有 334 處
## [2026-06-21 11:01:43] lint | 缺失欄位4, 無效type47, 標籤違規354, 孤立頁4

## [2026-06-21 11:30] create | WebDAV Sync 資源頁面
- **原因**：整理 Obsidian WebDAV Sync 插件設定資訊
- **動作**：
  - 建立 `resources/obsidian-webdav-sync.md`
  - 更新 `resources/index.md`
- **結果**：資源索引 1 篇，WebDAV 設定資訊已記錄
