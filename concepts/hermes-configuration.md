---
title: "Hermes Agent 配置與 Curator 技能管理"
description: "配置體系 + Curator 生命週期 + 工作流程規範"
summary: "Hermes 配置 + Curator 技能管理 + 7 步工作流程"
type: concept
status: active
tags: [hermes, deploy]
created: 2026-05-31
updated: 2026-06-27
---

# Hermes Agent 配置與 Curator 技能管理

## 配置體系

### 目錄結構 (`~/.hermes/`)

| 檔案 | 用途 |
|:---|:---|
| `config.yaml` | 核心行為（模型、終端、TTS、壓縮） |
| `.env` | API 金鑰與秘密 |
| `auth.json` | OAuth 認證憑據 |
| `SOUL.md` | Agent 身份定義 |
| `memories/` | 持久化記憶 (MEMORY.md, USER.md) |
| `skills/` | 技能庫 |
| `cron/` | 定時任務 |

### 設定優先級

CLI 參數 → `config.yaml` → `.env` → 內建預設值

### 終端後端

| 後端 | 隔離度 | 適用場景 |
|:---|:---|:---|
| local | 無 | 個人開發 |
| docker | 高 | 安全沙箱 |
| ssh | 網路邊界 | 遠端開發 |
| modal/daytona | 高 | 雲端算力 |

### 模型與上下文

- **輔助模型**：視覺/提取/壓縮，設定 `provider → model → base_url`
- **壓縮引擎**：`compressor`（有損摘要），閾值 50%，保護前 N 則，硬限制 400 則
- **安全**：`approvals.mode`（manual/smart/off）、Tirith 掃描、資源限制、PII 遮蓋

---

## Curator 技能管理員

管理 skill 生命週期：`active → stale → archived`

### 核心機制

- **觸發**：閒置檢查（非 cron），每 168h 檢查，需閒置 2h+
- **stale**：30 天未使用
- **archived**：90 天未使用 → `~/.hermes/skills/.archive/`（可恢復，永不自動刪除）
- **保護**：Hub-installed 和 User-directed skill 不管理

### 兩大階段

1. **Pruning**（確定性）：標記 stale、歸檔
2. **Consolidation**（LLM 輔助，默認關閉）：消耗 token 進行結構性整併

### CLI

```bash
hermes curator status              # 狀態查詢
hermes curator pin <skill>         # 防止自動遷移
hermes curator archive <skill>      # 手動歸檔
hermes curator restore <skill>     # 手動恢復
```

---

## 工作流程規範 v2.0

### 核心原則

**Agent 不應每次都全域搜尋。** 逐層縮小範圍：Router → Memory → Index → Schema（必要時）

### 7 步流程

| Step | 名稱 | 動作 |
|:---|:---|:---|
| 1 | ROUTER | 任務分類（READ/WRITE/PROJECT/RESEARCH/AUTOMATION） |
| 2 | MEMORY | 查 USER.md → MEMORY.md → SOUL.md → Project Memory |
| 3 | INDEX | 知識地圖定位，禁止全 Vault 搜尋 |
| 4 | SCHEMA | 僅建立/修改文件時讀取 |
| 5 | EXECUTE | 確認目標/輸出/限制/回寫需求 |
| 6 | REFLECT | 自我反思（task/result/problem/improvement/next） |
| 7 | WRITEBACK | 知識沉澱（永久/專案/摘要/臨時 4 級） |

### 知識地圖範例

```yaml
domains:
  hermes: {root: agents/hermes/}
  obsidian: {root: obsidian/}
  quartz: {root: publishing/quartz/}
  cms: {root: publishing/cms/}
  rss: {root: automation/rss/}
  stocks: {root: finance/stocks/}
  ai: {root: ai/}
  vps: {root: infrastructure/vps/}
```
