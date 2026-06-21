---
title: Hermes Configuration
summary: Hermes Configuration：相關頁面
created: 2026-05-31
updated: 2026-05-31
type: concept
tags: [maintenance]
---

## 相關頁面
- [[concepts/hermes-agent-rules|Hermes Agent 執行策略]]
- [[concepts/obsidian-wiki-conventions|Obsidian Wiki 使用規範]]


# Hermes Agent 配置體系 (Configuration)

## 概觀
Hermes Agent 採用分層配置機制，將通用設定與敏感金鑰分開儲存，並提供多種終端後端以確保執行安全。

## 核心目錄結構 (`~/.hermes/`)
- `config.yaml`: 核心行為設定（模型、終端、TTS、壓縮等）。
- `.env`: API 金鑰與秘密資訊（Secrets）。
- `auth.json`: OAuth 認證憑據。
- `SOUL.md`: Agent 身份定義（System Prompt 第一順位）。
- `memories/`: 持久化記憶 (`MEMORY.md`, `USER.md`)。
- `skills/`: 技能庫。
- `cron/`: 定時任務。

## 設定管理
### 優先級 (由高到低)
1. CLI 參數 → 2. `config.yaml` → 3. `.env` → 4. 內建預設值。

### 常用 CLI 指令
- `hermes config`: 查看當前設定。
- `hermes config edit`: 編輯 `config.yaml`。
- `hermes config set KEY VAL`: 設定值（自動分流至 `.env` 或 `config.yaml`）。
- `hermes config check/migrate`: 檢查缺失選項或進行版本遷移。

## 終端後端 (Terminal Backends)
| 後端 | 執行位置 | 隔離度 | 適用場景 |
| :--- | :--- | :--- | :--- |
| **local** | 主機 | 無 | 個人開發、快速測試 |
| **docker** | 持久化容器 | 高 | 安全沙箱、CI/CD |
| **ssh** | 遠端伺服器 | 網路邊界 | 高效能硬體、遠端開發 |
| **modal** | 雲端沙箱 | 高 (Cloud VM) | 短暫算力需求、評測 |
| **daytona** | 雲端工作區 | 高 (Container) | 託管開發環境 |
| **singularity**| 容器 | 命名空間 | HPC 集群 |

## 模型與上下文控制
### 輔助模型 (Auxiliary Models)
- 用於視覺、提取、壓縮等側邊任務。
- 設定模式：`provider` → `model` → `base_url`。
- `provider: "main"` 會自動沿用主模型設定。

### 上下文壓縮 (Compression)
- **引擎**: `compressor` (有損摘要)。
- **觸發閾值**: 預設 `0.50` (50% 視窗佔用時觸發)。
- **保護機制**: `protect_first_n` 鎖定開頭對話，避免遺忘初始目標。
- **硬限制**: `hygiene_hard_message_limit` (預設 400 則) 強制壓縮。

## 安全與資源防護
- **指令審核**: `approvals.mode` 可設為 `manual` (每次詢問)、`smart` (LLM 評估風險) 或 `off`。
- **Tirith 掃描**: 自動偵測終端指令中的危險操作。
- **資源限制**:
    - `file_read_max_chars`: 限制單次讀取字數（預設 100,000）。
    - `max_bytes` / `max_lines`: 截斷過長的工具輸出。
- **隱私**: `privacy.redact_pii` 自動遮蓋個資。