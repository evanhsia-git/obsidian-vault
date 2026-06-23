---
title: superpowers-install
description: Superpowers skill pack 安裝說明
summary: "Superpowers 安裝說明 — obra/superpowers 14 個 skills 匯入 Hermes Agent 的完整流程"
type: concept
status: active
tags: [hermes, skills, superpowers, installation]
created: 2026-06-23
updated: 2026-06-23
---

# Superpowers Skill Pack 安裝說明

## 什麼是 Superpowers？

Superpowers 是 [obra](https://github.com/obra) 開發的 **Agentic Skills 框架**，本質是一組預先寫好的「技能說明書」（SKILL.md），教 AI Agent 如何有紀律地做軟體開發。它不是程式庫，而是**給 Agent 讀的工作流程指南**。

- GitHub: [obra/superpowers](https://github.com/obra/superpowers)

## 包含哪些 Skills？（共 14 個）

| # | Skill | 用途 |
|---|-------|------|
| 1 | `brainstorming` | 發散思考、探索方案 |
| 2 | `dispatching-parallel-agents` | 平行派遣多個子 Agent |
| 3 | `executing-plans` | 按計畫執行任務 |
| 4 | `finishing-a-development-branch` | 完成開發分支（測試、merge、清理） |
| 5 | `receiving-code-review` | 接收並處理 code review 回饋 |
| 6 | `requesting-code-review` | 主動請求 code review |
| 7 | `subagent-driven-development` | 子 Agent 驅動開發 |
| 8 | `systematic-debugging` | 系統化除錯 |
| 9 | `test-driven-development` | TDD 紅綠重構循環 |
| 10 | `using-git-worktrees` | Git worktree 隔離工作區 |
| 11 | `using-superpowers` | 核心使用指南 |
| 12 | `verification-before-completion` | 完成前驗證 |
| 13 | `writing-plans` | 撰寫可執行的計畫 |
| 14 | `writing-skills` | 如何寫 skill |

## 安裝方式

Superpowers 原生支援 Claude Code / Codex / Cursor 等，**沒有官方 Hermes 安裝方式**。以下採用手動 clone + 複製的方式匯入：

### 步驟

```bash
# 1. Clone repo（淺克隆，只取最新一份）
cd /tmp && rm -rf superpowers-repo
git clone --depth 1 https://github.com/obra/superpowers.git superpowers-repo

# 2. 建立 Hermes skills 目標目錄
mkdir -p ~/.hermes/skills/superpowers

# 3. 複製 14 個 skill 目錄
cp -r /tmp/superpowers-repo/skills/* ~/.hermes/skills/superpowers/

# 4. 驗證安裝
hermes skills list | grep -i superpowers
```

### 安裝後結構

```
~/.hermes/skills/superpowers/
├── brainstorming/
│   └── SKILL.md
├── dispatching-parallel-agents/
│   └── SKILL.md
├── executing-plans/
│   └── SKILL.md
├── finishing-a-development-branch/
│   └── SKILL.md
├── receiving-code-review/
│   └── SKILL.md
├── requesting-code-review/
│   └── SKILL.md
├── subagent-driven-development/
│   └── SKILL.md
├── systematic-debugging/
│   └── SKILL.md
├── test-driven-development/
│   └── SKILL.md
├── using-git-worktrees/
│   └── SKILL.md
├── using-superpowers/
│   └── SKILL.md
├── verification-before-completion/
│   └── SKILL.md
├── writing-plans/
│   └── SKILL.md
└── writing-skills/
    └── SKILL.md
```

## 與既有 Skills 的關係

Hermes Agent 已部分重疊的技能（如 `test-driven-development`、`systematic-debugging`、`writing-plans`、`subagent-driven-development`）。Superpowers 的版本是針對通用 Agent 設計的，可能較新。

**原則**：兩者共存，不互相覆蓋。需要時由 Agent 選擇適合的版本。

## 注意事項

- 這些 skill 是**唯讀指南**，不會自動執行任何東西
- 它們在 Agent 處理相關任務時會被自動載入參考
- 不會與既有 skill 衝突（名稱相同但路徑不同）
- 更新方式：重新 clone repo 並覆蓋

---

## 相關節點

- [[skills/architecture-references/superpowers]] — Superpowers 架構參考（較早建立的架構分析頁面）
- [[skills/skills-index]] — Skills 目錄索引
