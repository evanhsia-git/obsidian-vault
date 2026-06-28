---
title: "Hermes Telegram Dashboard 架構分析"
description: "Telegram Bot 架構分析與 Dashboard v1 設計建議"
summary: "分析 Hermes Agent Telegram 適配器的完整架構，包含 Bot 入口、Handler、Callback、State Machine 現狀，以及 Dashboard v1 最佳架構建議"
type: report
status: active
tags: [telegram, architecture, dashboard, hermes]
created: 2026-06-28
updated: 2026-06-28
---

# Hermes Telegram Dashboard 架構分析

> 生成日期：2026-06-28
> 分析範圍：`/usr/local/lib/hermes-agent/` Telegram 相關模組
> 模式：**Analysis Only**（未修改任何程式）

---

## 1. 整體架構

```
┌─────────────────────────────────────────────────────────────┐
│  Telegram Server (Bot API)                                   │
└──────────────────────┬──────────────────────────────────────┘
                       │ Polling / Webhook
                       ▼
┌──────────────────────────────────────────────────────────────┐
│  TelegramAdapter (plugins/platforms/telegram/adapter.py)     │
│  ┌─────────────────────────────────────────────────────────┐ │
│  │ Application (python-telegram-bot)                       │ │
│  │  ├─ TelegramMessageHandler(TEXT) → _handle_text_message │ │
│  │  ├─ TelegramMessageHandler(COMMAND) → _handle_command   │ │
│  │  ├─ TelegramMessageHandler(LOCATION)                    │ │
│  │  ├─ TelegramMessageHandler(PHOTO/VIDEO/AUDIO/...)      │ │
│  │  └─ CallbackQueryHandler → _handle_callback_query      │ │
│  ├─ State (in-memory dicts):                               │ │
│  │  ├─ _model_picker_state: {chat_id → dict}               │ │
│  │  ├─ _approval_state: {message_id → session_key}         │ │
│  │  ├─ _slash_confirm_state: {confirm_id → session_key}    │ │
│  │  └─ _clarify_state: {clarify_id → session_key}          │ │
│  ├─ send_model_picker() — Inline Keyboard 模型選擇器        │ │
│  └─ _is_callback_user_authorized() — callback 權限驗證     │ │
└──────────────────────┬──────────────────────────────────────┘
                       │ MessageEvent
                       ▼
┌──────────────────────────────────────────────────────────────┐
│  GatewayRunner (gateway/run.py)                              │
│  _handle_message(event)                                      │
│   ├─ pre_gateway_dispatch plugin hook                        │
│   ├─ user auth / pairing flow                                │
│   ├─ resolve_command() → GatewaySlashCommandsMixin           │
│   │   └─ /new /stop /status /restart /clear ...              │
│   └─ _handle_message_with_agent() → Agent conversation loop  │
└──────────────────────────────────────────────────────────────┘
```

---

## 2. 關鍵檔案

| 檔案 | 責任 | 行數 |
|------|------|------|
| `plugins/platforms/telegram/adapter.py` | Telegram 適配器（接收、發送、Handler 註冊） | 7798 |
| `hermes_cli/telegram_managed_bot.py` | Bot onboarding（Managed Bot 配對） | 358 |
| `plugins/platforms/telegram/telegram_ids.py` | Chat ID 正規化 | 51 |
| `plugins/platforms/telegram/telegram_network.py` | Fallback IP 傳輸（DNS 失靈時） | 259 |
| `gateway/run.py` | GatewayRunner 核心（訊息分派、agent 對話） | 18791 |
| `gateway/slash_commands.py` | Slash command 處理器（/new, /stop, /status 等） | 4118 |
| `gateway/platforms/base.py` | BasePlatformAdapter 抽象層 | 5351 |
| `gateway/config.py` | PlatformConfig（token 等設定） | 2076 |
| `hermes_cli/commands.py` | COMMAND_REGISTRY、選單、resolve_command | 2048 |
| `tools/send_message_tool.py` | 跨平台發送工具（含 MEDIA） | 1373+ |

---

## 3. 核心元件定位

### Telegram Bot 入口

- **位置**：`adapter.py:2310` — `async def connect()`
- **流程**：`Application.builder().token(self.config.token)` → 註冊 handlers → `app.start()` → polling/webhook

### Bot Token 載入

- **位置**：`adapter.py:2339` — `self.config.token`
- **來源**：`gateway.yaml → platforms.telegram.token` 或環境變數 `TELEGRAM_BOT_TOKEN`
- **型別**：`PlatformConfig.token`（`gateway/config.py:331`）

### Message Handler

- **位置**：`adapter.py:2481-2498`
- **5 個 TelegramMessageHandler**：
  - `TEXT & ~COMMAND` → `_handle_text_message`
  - `COMMAND` → `_handle_command`
  - `LOCATION | VENUE` → `_handle_location_message`
  - `PHOTO | VIDEO | AUDIO | VOICE | Document.ALL | Sticker.ALL` → `_handle_media_message`

### Command Handler

- **位置**：`adapter.py:6582` — `_handle_command()`
- **流程**：收到命令 → 轉發給 GatewayRunner → `resolve_command()` → `GatewaySlashCommandsMixin` 處理

### Callback Handler

- **位置**：`adapter.py:4456` — `_handle_callback_query()`
- **已存在的 callback 協議**：

| Prefix | 用途 | 說明 |
|--------|------|------|
| `mp:` | Model picker | 模型選擇器 |
| `mpg:` / `mm:` / `mc:` / `mb` / `mx` / `mg:` | Model picker 子操作 | 翻頁、分組等 |
| `gt:` | Gmail triage | Gmail 郵件分類 |
| `ea:` | Exec approval | 危險命令批准（once/session/always/deny） |
| `sc:` | Slash confirm | /reload-mcp 等確認提示 |
| `cl:` | Clarify | 多選題 clarify 工具 |

### Telegram API Library

- **使用**：`python-telegram-bot`
- **匯入**：`from telegram import Update, Bot, Message, InlineKeyboardButton, InlineKeyboardMarkup`
- **擴展**：`from telegram.ext import Application, CommandHandler, CallbackQueryHandler, MessageHandler, ContextTypes, filters`

### Router 架構

- **模式**：`python-telegram-bot` 的 `Application.add_handler()` 模式
- **每種 filter 一個 handler**，按註冊順序匹配
- **Callback 統一入口**：所有 inline keyboard 點擊都走 `_handle_callback_query()`，內部用前綴分流

---

## 4. 現有功能清單

| 功能 | 狀態 | 位置 |
|------|------|------|
| Inline Keyboard | ✅ 已存在 | `send_model_picker()` at line 3967 |
| Callback Query | ✅ 已存在 | `_handle_callback_query()` at line 4456 |
| Menu System | ✅ 已存在 | `telegram_menu_commands()` at line 889，最多 100 個 BotCommand |
| State Machine | ❌ 不存在 | 只有 4 個獨立 dict |
| Dashboard | ❌ 不存在 | — |
| 權限驗證 | ✅ 已存在 | `_is_callback_user_authorized()` |
| Fallback Transport | ✅ 已存在 | `TelegramFallbackTransport`（DNS 失靈時用 IP 直連） |
| Webhook 模式 | ✅ 已存在 | `TELEGRAM_WEBHOOK_URL` env 控制 |
| Forum Topic 支援 | ✅ 已存在 | `_ensure_forum_commands()` 懶加載 |
| Media 處理 | ✅ 已存在 | Photo/Video/Audio/Voice/Document/Sticker 全支援 |
| Reaction | ✅ 已存在 | `_set_reaction()` / `_clear_reactions()` |

---

## 5. Dashboard v1 最佳架構建議

### 建議檔案結構

```
plugins/platforms/telegram/
├── adapter.py              ← 現有（只加 1 行）
├── telegram_dashboard.py   ← 新增：Dashboard 核心
│   ├─ DashboardMenu       — 選單樹（Main → Sub-menus）
│   ├─ DashboardState       — State Machine（取代 dict）
│   └─ DashboardRenderer    — 訊息格式化 + InlineKeyboard 產生
├── telegram_states.py      ← 新增：State Machine + callback routing
└── telegram_menu.py        ← 新增：指令擴展（/dashboard, /cron, /sessions 視覺化）
```

### State Machine 設計

```python
class DashboardState(Enum):
    IDLE = "idle"
    MAIN_MENU = "main"
    SESSION_LIST = "session_list"
    SESSION_DETAIL = "session_detail"
    CRON_LIST = "cron_list"
    CRON_DETAIL = "cron_detail"
    CONFIG = "config"
    SKILL_LIST = "skill_list"
```

### Dashboard Callback 協議

| Prefix | 用途 | 現有 |
|--------|------|------|
| `d:` | Dashboard navigation（menu/submenu/select） | ❌ 新增 |
| `db:` | Dashboard button action（back/refresh/execute） | ❌ 新增 |
| `mp:` | Model picker | ✅ 現有 |
| `ea:` | Approval | ✅ 現有 |
| `sc:` | Slash confirm | ✅ 現有 |
| `cl:` | Clarify | ✅ 現有 |

### 需修改的現有檔案

1. **`adapter.py`** — 只加 dashboard callback 前綴判斷（在 `_handle_callback_query()` 開頭加 `if data.startswith("d:")` 或 `db:`）
2. **`hermes_cli/commands.py`** — 新增 `/dashboard` 指令，觸發 `telegram_dashboard.py` 發送主選單

### 建議新增的檔案

| 檔案 | 預估行數 | 內容 |
|------|----------|------|
| `telegram_dashboard.py` | ~300 | Dashboard 核心邏輯：選單樹、狀態轉換、資料獲取 |
| `telegram_states.py` | ~80 | State Machine 資料結構：DashboardState enum + 轉換表 |
| `telegram_menu.py` | ~150 | InlineKeyboard 選單產生器：主選單、工作階段列表、Cron 列表等 |

### 設計原則

1. **不動 adapter.py 核心** — 避免 7798 行檔案更動風險
2. **複用現有 callback 模式** — `d:` 前綴沿用 `mp:`/`ea:` 已驗證的 routing 慣例
3. **State Machine 替換散落的 dict** — 目前 4 個獨立 dict 難維護；Dashboard 需要清晰的生命週期管理
4. **與 `hermes_cli/commands.py` 整合** — `/dashboard` 指令可直接從 Telegram 觸發
5. **向後相容** — 原有 callback 協議（model picker、approval）完全不受影響

---

## 6. 相關節點

- [[telegram-output-rules]]
- [[schema]]
