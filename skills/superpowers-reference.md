---
title: "Superpowers 功能說明"
description: "Superpowers 完整功能說明 — 14 個 Agentic Skills 框架"
summary: "Superpowers (obra/superpowers) 完整功能說明：14 個 Agentic Skills 框架"
created: 2026-06-23
updated: 2026-06-23
type: index
status: active
tags: [agent, workflow]
---

# Superpowers 功能說明

> Superpowers 是 [obra](https://github.com/obra) 開發的 **Agentic Skills 框架**，本質是一組預先寫好的「技能說明書」（SKILL.md），教 AI Agent 如何有紀律地做軟體開發。

- GitHub: [obra/superpowers](https://github.com/obra/superpowers)
- 安裝位置：`~/.hermes/skills/superpowers/`
- 版本：v5.x（2026-06-23 安裝）

---

## 架構總覽

Superpowers 包含 **14 個 Skills**，分為 4 個類別：

| 類別 | Skills |
|------|--------|
| **核心流程** | using-superpowers, brainstorming, writing-plans, executing-plans |
| **開發方法** | test-driven-development, systematic-debugging, subagent-driven-development, dispatching-parallel-agents |
| **協作審查** | requesting-code-review, receiving-code-review |
| **環境管理** | using-git-worktrees, finishing-a-development-branch, verification-before-completion, writing-skills |

---

## 核心流程 Skills

### 1. using-superpowers（核心入口）

**用途**：每次對話開始時必須先載入此 skill。

**核心規則**：
- **IF A SKILL APPLIES TO YOUR TASK, YOU DO NOT HAVE A CHOICE. YOU MUST USE IT.**
- 即使只有 1% 的機率適用，也必須 invoke
- 不可用「這很簡單」、「我記得這個 skill」等理由跳過

**Skill 優先級**：
1. 用戶明確指令（最高）
2. Superpowers skills
3. 預設系統提示（最低）

**Skill 類型**：
- **Rigid**（TDD、systematic-debugging）：必須嚴格遵守
- **Flexible**（patterns）：可根據情境調整

---

### 2. brainstorming（發散思考）

**用途**：在任何創意工作前必須使用——建立功能、構建元件、新增功能、修改行為。

**流程**：
1. 探索專案上下文
2. 提出澄清問題（一次一個）
3. 提出 2-3 個方案（含權衡和推薦）
4. 分段呈現設計，每段後取得核准
5. 寫入設計文件
6. 自我審查
7. 用戶審查
8. 轉為實作計畫

**關鍵原則**：
- 禁止在未取得設計核准前開始實作
- 即使「看似簡單」的設計也必須走過流程
- 一次只問一個問題

---

### 3. writing-plans（撰寫計畫）

**用途**：將規格或需求轉為可執行的實作計畫。

**計畫結構**：
```
# Feature Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development

**Goal:** [一句話描述目標]
**Architecture:** [2-3 句說明方法]
**Tech Stack:** [關鍵技術]

## Global Constraints
[專案全域需求]

---

### Task N: [元件名稱]

**Files:**
- Create: `exact/path/to/file.py`
- Modify: `exact/path/to/existing.py:123-145`
- Test: `tests/exact/path/to/test.py`

**Interfaces:**
- Consumes: [此 task 使用的前置產出]
- Produces: [後續 task 依賴的產出]

- [ ] **Step 1: Write the failing test**
- [ ] **Step 2: Run test to verify it fails**
- [ ] **Step 3: Write minimal implementation**
- [ ] **Step 4: Run test to verify it passes**
- [ ] **Step 5: Commit**
```

**關鍵原則**：
- 每個 task 必須有獨立的測試循環
- 步驟必須是 2-5 分鐘可完成的最小單位
- 禁止 placeholder（TBD、TODO、implement later）
- 所有程式碼必須完整顯示

---

### 4. executing-plans（執行計畫）

**用途**：在獨立 session 中執行已撰寫的實作計畫。

**流程**：
1. 載入並批判性審查計畫
2. 為每個 task 建立 todo
3. 逐一執行 task
4. 所有 task 完成後，使用 finishing-a-development-branch skill

**何時停止**：
- 遇到 blocker（缺少依賴、測試失敗、指令不清）
- 計畫有嚴重缺口
- 不理解某個指令
- 驗證持續失敗

---

## 開發方法 Skills

### 5. test-driven-development（TDD）

**用途**：在任何功能或 bugfix 實作前必須使用。

**鐵律**：
```
NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST
```

**紅綠重構循環**：
1. **RED**：寫一個失敗的測試
2. **Verify RED**：確認測試確實失敗
3. **GREEN**：寫最小程式碼通過測試
4. **Verify GREEN**：確認所有測試通過
5. **REFACTOR**：重構，保持綠色
6. Repeat

**禁止行為**：
- 先寫程式再寫測試
- 測試通過後不驗證
- 「這次跳過 TDD」
- 手動測試取代自動化測試
- 保留已寫好的程式作為「參考」

**常見藉口與現實**：
| 藉口 | 現實 |
|------|------|
| 「太簡單不需要測試」 | 簡單程式也會壞，測試只需 30 秒 |
| 「之後再測」 | 立即通過的測試證明不了什麼 |
| 「已手動測試過」 | 手動 ≠ 系統化，無法重複執行 |
| 「刪除 X 小時的工作是浪費」 | 沉沒成本謬論，保留無法信任的程式是技術債 |

---

### 6. systematic-debugging（系統化除錯）

**用途**：遇到任何 bug、測試失敗、異常行為時，在提出修復前必須使用。

**鐵律**：
```
NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST
```

**四階段流程**：

**Phase 1：根因調查**
- 仔細閱讀錯誤訊息
- 重現問題（確認可穩定觸發）
- 檢查近期變更
- 在多組件系統中加入診斷工具
- 追蹤資料流

**Phase 2：模式分析**
- 找到可運作的類似程式碼
- 與參考實作比較
- 識別差異
- 理解依賴關係

**Phase 3：假設與測試**
- 形成單一假設
- 最小化測試
- 驗證後才繼續
- 不確定時說「我不理解」

**Phase 4：實作**
- 先寫失敗的測試案例
- 實作單一修復
- 驗證修復
- 如果修復無效，返回 Phase 1

**紅旗（必須停止）**：
- 「快速修復，之後再調查」
- 「試試看改 X 會不會好」
- 「跳過測試，手動驗證」
- 3+ 次修復失敗 = 質疑架構而非繼續修復

---

### 7. subagent-driven-development（子 Agent 驅動開發）

**用途**：在 session 中執行有多個獨立 task 的實作計畫。

**核心原則**：每個 task 一個新的 subagent + task review + 最終 review = 高品質、快速迭代

**流程**：
```
讀取計畫 → 建立 todos → 逐一執行：
  1. 分派 implementer subagent
  2. subagent 實作、測試、commit、自我審查
  3. 分派 task reviewer subagent
  4. 審查通過 → 標記完成
  5. 審查不通過 → 分派 fix subagent → 重新審查
所有 task 完成後 → 分派最終 code reviewer → finishing-a-development-branch
```

**模型選擇**：
- 機械式實作（1-2 檔案、清晰規格）：便宜模型
- 整合與判斷（多檔案協調、除錯）：標準模型
- 架構與設計：最強模型
- 最終 review：最強模型

**關鍵原則**：
- 每個 task 必須有 review（spec compliance + code quality）
- 不可跳過 review loop
- 不可忽略 subagent 的問題
- 不可在 main/master 分支上實作

---

### 8. dispatching-parallel-agents（平行派遣 Agent）

**用途**：面對 2+ 個獨立任務，可同時進行而無共享狀態或順序依賴。

**使用時機**：
- 3+ 個測試檔案因不同根因而失敗
- 多個子系統獨立壞掉
- 每個問題可在無其他問題上下文下理解

**模式**：
1. 識別獨立領域
2. 為每個 agent 建立聚焦的任務
3. 同時分派（同一 response 中多個 dispatch = 平行執行）
4. 審查並整合結果

**關鍵原則**：
- 每個 agent 只負責一個問題領域
- 不可給 agent 過多上下文
- 不可在相關失敗時使用（修復一個可能修復其他）

---

## 協作審查 Skills

### 9. requesting-code-review（請求 Code Review）

**用途**：完成 task、實作主要功能、或 merge 前驗證工作符合需求。

**時機**：
- **必須**：subagent-driven development 的每個 task 後、完成主要功能前、merge 到 main 前
- **可選但有價值**：卡住時、重構前、修復複雜 bug 後

**流程**：
1. 取得 git SHAs
2. 分派 code reviewer subagent
3. 根據反饋行動：
   - Critical：立即修復
   - Important：繼續前修復
   - Minor：記錄供之後處理

---

### 10. receiving-code-review（接收 Code Review）

**用途**：接收 code review 回饋時，在實作建議前必須使用。

**核心原則**：技術正確性 > 社交舒適度

**回應模式**：
1. 完整閱讀回饋
2. 用自己的話重述需求
3. 對程式碼庫現實進行驗證
4. 評估：對此程式碼庫是否技術正確？
5. 技術性回覆或有理由的反駁
6. 一次實作一個項目，逐一測試

**禁止行為**：
- 「你完全正確！」（表演性同意）
- 「好棒的問題！」/「優秀的回饋！」
- 未驗證就開始實作
- 對所有項目表示感謝

**正確回應**：
- ✅ 「已修復。[簡短描述變更]」
- ✅ 「好發現 - [具體問題]。已在 [位置] 修復。」
- ✅ 直接修復，用程式碼本身表示你聽到了

---

## 環境管理 Skills

### 11. using-git-worktrees（使用 Git Worktree）

**用途**：開始需要隔離的功能工作前，確保有獨立的工作區。

**流程**：
1. **Step 0**：偵測是否已在隔離工作區
2. **Step 1a**：優先使用平台原生 worktree 工具
3. **Step 1b**：fallback 到 `git worktree add`
4. **Step 2**：專案設定（自動偵測 package.json/Cargo.toml 等）
5. **Step 3**：驗證乾淨的測試基準線

**目錄優先級**：
1. 用戶明確指定的目錄
2. 專案本地 `.worktrees/`（偏好）或 `worktrees/`
3. 預設 `.worktrees/`

**關鍵原則**：
- 永遠先執行 Step 0 偵測
- 優先使用原生工具
- 驗證目錄已被 .gitignore
- 不可在已隔離的工作區中再建立 worktree

---

### 12. finishing-a-development-branch（完成開發分支）

**用途**：實作完成、所有測試通過後，決定如何整合工作。

**流程**：
1. 驗證測試通過
2. 偵測環境（normal repo / named worktree / detached HEAD）
3. 決定 base branch
4. 呈現選項：
   - Option 1: Merge 回 base branch
   - Option 2: Push 並建立 PR
   - Option 3: 保持 branch 不變
   - Option 4: 捨棄工作
5. 執行選擇
6. 清理工作區

**關鍵原則**：
- 必須先驗證測試才呈現選項
- Option 2（PR）不可清理 worktree（用戶需要迭代）
- Option 4 需要明確確認（輸入 "discard"）
- 不可在 merge 前刪除 branch

---

### 13. verification-before-completion（完成前驗證）

**用途**：在聲稱工作完成、修復、通過前必須使用。

**鐵律**：
```
NO COMPLETION CLAIMS WITHOUT FRESH VERIFICATION EVIDENCE
```

**閘函數**：
1. IDENTIFY：什麼命令能證明這個聲稱？
2. RUN：執行完整命令（新鮮、完整）
3. READ：完整輸出、檢查 exit code、計算失敗數
4. VERIFY：輸出是否確認聲稱？
5. ONLY THEN：才做出聲稱

**紅旗**：
- 使用「應該」、「可能」、「看起來」
- 在驗證前表達滿意（「太棒了！」、「完美！」、「完成！」）
- 信任 agent 的成功報告
- 依賴部分驗證

---

### 14. writing-skills（撰寫 Skills）

**用途**：建立新 skill、編輯現有 skill、或部署前驗證 skill 是否正常。

**核心原則**：撰寫 skill = 將 TDD 應用於流程文件

**TDD 對應**：
| TDD 概念 | Skill 建立 |
|----------|------------|
| 測試案例 | 壓力場景 + subagent |
| 生產程式碼 | Skill 文件（SKILL.md） |
| 測試失敗（RED） | 無 skill 時 agent 違反規則 |
| 測試通過（GREEN） | 有 skill 時 agent 遵守規則 |
| 重構 | 關閉漏洞同時維持合規 |

**SKILL.md 結構**：
```yaml
---
name: skill-name-with-hyphens
description: "Use when [specific triggering conditions and symptoms]"
---

# Skill Name

## Overview
這是什麼？核心原則 1-2 句。

## When to Use
使用時機（症狀、情境）
When NOT to use

## Core Pattern
Before/after 比較

## Implementation
簡單模式 inline，複雜參考連結檔案

## Common Mistakes
什麼會出錯 + 修復方式
```

**Description 品質標準（SDO）**：
- ✅ 只描述觸發條件，不總結 skill 流程
- ✅ 使用第三人稱
- ✅ 包含具體症狀和情境
- ❌ 總結 skill 的工作流程
- ❌ 使用第一人稱
- ❌ 過於抽象

---

## Superpowers Workflow 總覽

完整開發流程：

```
1. using-superpowers（每次對話開始）
   ↓
2. brainstorming（創意工作前）
   ↓
3. writing-plans（將設計轉為計畫）
   ↓
4. subagent-driven-development（執行計畫）
   ├─ 每個 task：
   │  ├─ test-driven-development（實作）
   │  ├─ systematic-debugging（除錯）
   │  └─ requesting-code-review（審查）
   └─ 最終 review
   ↓
5. finishing-a-development-branch（完成分支）
   ↓
6. verification-before-completion（最終驗證）
```

---

## 與 Hermes Agent 的整合

Superpowers 安裝於 `~/.hermes/skills/superpowers/`，可與 Hermes 既有 skills 共存。

**重疊的 skills**：
- `test-driven-development`：Hermes 版本已存在，Superpowers 版本更詳細
- `systematic-debugging`：Hermes 版本已存在
- `writing-plans`：Hermes 版本已存在
- `subagent-driven-development`：Hermes 版本已存在

**原則**：兩者共存，不互相覆蓋。需要時由 Agent 選擇適合的版本。

---
## 相關頁面

- [[superpowers-install|Superpowers 安裝說明]] — 安裝流程
- [[skills/skills-index|Skills 目錄索引]]
- [[concepts/concepts-index|概念筆記索引]]
- [[architecture-references/superpowers|Superpowers 架構參考]] — 較早建立的架構分析頁面
- [[schema|SCHEMA]] — 核心憲法
- [[policy|POLICY]] — 規則路由器
