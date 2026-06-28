---
title: "h-memory-optimizer"
description: "Hermes Agent 記憶體優化工具，管理 SOUL.md / MEMORY.md / USER.md 三檔"
summary: "hermes記憶體優化"
type: schema
status: active
tags:
  - hermes
  - skills
created: 2026-06-28
updated: 2026-06-28
---


---
name: h-memory-optimizer
description: >
  Hermes Agent 記憶體優化工具，管理 SOUL.md / MEMORY.md / USER.md 三檔。
  觸發條件：
  ① 任務前自動檢查：任一檔使用率 >70% 時強制執行
  ② 用戶明示：「清理 memory」「memory 滿了」「優化記憶體」「H記憶優化」「h記憶優化」
  執行策略：>70% 用清空重寫；≤70% 只刪少量條目。
---

# H-Memory Optimizer v2

## ▶ 執行入口（任務前必做）

```
Step 0｜讀取三檔現況
  - SOUL.md   上限：依專案設定（預設 2000 chars）
  - MEMORY.md 上限：4000 chars
  - USER.md   上限：1375 chars

Step 1｜計算使用率
  soul_pct  = len(SOUL.md)  / 2000
  mem_pct   = len(MEMORY.md) / 4000
  user_pct  = len(USER.md)   / 1375

Step 2｜決策
  任一 >70% → 執行 Phase 2（清空重寫）
  全部 ≤70% → 執行 Phase 1（診斷）→ 視需要 Phase 3（局部刪除）
  完成後繼續原任務
```

---

## Phase 1：診斷（使用率 ≤70%）

逐條掃描，標記：

| 標記 | 定義 |
|------|------|
| `[DUP]` | ≥2 條描述同一主題 |
| `[OLD]` | 過時 API、臨時檔名、已完成對話細節 |
| `[BLOAT]` | 完整句子可壓縮為關鍵字 |
| `[FINE]` | 關鍵事實，保留 |

**保留優先**：DB IDs、API retry 規則、付費確認規則、rate limit、模型設定、時區。

---

## Phase 2：清空重寫（使用率 >70%，推薦）

```
1. 依序 remove 全部 MEMORY.md entries
2. 依序 remove 全部 USER.md entries
3. 視需要精簡 SOUL.md（只刪 [OLD]/[DUP]，保留身份定義）
4. 用高密度格式重新 add：
   USER.md  → ≤3 條、合計 ≤500 chars
   MEMORY.md → ≤5 條、合計 ≤2800 chars
   SOUL.md  → 僅調整，不清空
```

**清空重寫優點**：無 residual 殘留、重新組織架構、單次完成。

---

## Phase 3：局部刪除（使用率 ≤70%）

針對 `[DUP]`：保留最新最完整版本，合併獨有資訊後刪其餘。
針對 `[OLD]`：直接刪除，不需替換。
針對 `[BLOAT]`：用 replace 壓縮為關鍵字格式。

---

## Phase 4：驗證

```
✓ MEMORY.md <70%（<2800 chars）
✓ USER.md   <50%（<688 chars）
✓ SOUL.md   <70%
✓ 關鍵事實已保留（DB IDs、API limits、模型偏好）
✓ 無 credential 洩漏
```

---

## memory.remove 操作規範

- `old_text` 使用前 20–30 字的唯一子字串，**勿貼整段**
- 連續失敗 2 次 → 改用 `replace` 覆寫，或跳過
- 無法匹配 → entry 已刪或已變更，直接跳過
- 批量刪除前可用 `search_files` 定位重複內容

---

## 三檔分工原則

| 檔案 | 存放內容 | 禁止存放 |
|------|---------|---------|
| `SOUL.md` | 身份定義、行為準則、核心使命 | 技術細節、API 資料 |
| `MEMORY.md` | API 行為、環境事實、工具技巧、DB IDs | 個人偏好、溝通風格 |
| `USER.md` | 個人偏好、暱稱、溝通風格、工作流偏好 | 技術 API、環境資訊 |

---

## 常見清理模式

**模式 A｜DB IDs 跨檔重複**
`Before` USER + MEMORY 各有 DB IDs → `After` 統一放 USER，MEMORY 只留行為規則

**模式 B｜專案細節膨脹**
`Before` 每個 script 路徑、每步操作 → `After` 只留結論（「256 檔 ETF 寫入 etf_basic_info」）

**模式 C｜技能更新記錄**
`Before` 「2026-05-23 更新了 skill X」→ `After` 刪除（skill 本身已含此資訊）

**模式 D｜套件版本號**
`Before` 「FinMind 1.9.10, TA-Lib 0.6.8」→ `After` 只保留有特殊限制的（如 TA-Lib 需 float64）

**模式 E｜重複 API 描述**
`Before` 「TWSE 月平均價 API」+ 「TWSE API 大全」→ `After` 合併為一條，刪被涵蓋的

**模式 F｜SOUL.md 行為準則膨脹**
`Before` 多條「任務前須...」規則散落各處 → `After` 合併為單一「執行檢查清單」區塊

**模式 G｜USER.md 大量 entries 堆積**
`Before` 8 條 entries 含重複主題（98% 使用率）→ `After` 1 條高密度 entry（≤15% 使用率）
目標：≤3 條、≤500 chars，以 § 分段

---

## 壓縮技巧速查

| Before（冗餘） | After（高密度） |
|--------------|--------------|
| 「寫入資料時先檢查現有頁面/block 內容，用 PATCH 更新現有 block」 | 「寫入先檢查→PATCH更新」 |
| 「config.yaml web.search_backend=searxng，.env SEARXNG_URL=http://localhost:8080」 | 「SearXNG(localhost:8080)」 |
| 「第一次失敗→等3分鐘→第二次嘗試→再失敗才通知」 | 「失敗→等3min→重試→失敗才通知」 |
| 完整 sentences | 關鍵字＋符號（§ / → / +） |

---

## 附加規範（2026-06-08）

**Skill 管理**：每次任務前 `skills_list()` 檢查；優先修改現有技能，新建前必須明示說明必要性；三層分類架構（主類→子類→技能）。

**Python 使用限制**：僅限精確數學計算、解析 CSV/Excel/JSON、格式轉換；禁用於純文字創作、概念解釋、簡單邏輯；呼叫前先確認無法用純文字完成。

---

## 安全守則

```
❌ 禁止刪除：DB IDs、API retry 規則、付費確認規則、rate limit、模型偏好、時區
✅ 可以刪除：臨時檔名、對話過程細節、重複兩次以上的相同資訊、已被 skill 封裝的工作流
```
