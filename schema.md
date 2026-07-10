---
title: "schema"
description: "Hermes Agent 核心憲法"
summary: "Obsidian Wiki、Wiki-LLM、Hermes Agent 核心規範"
type: schema
status: permanent
tags: [hermes, obsidian]
created: 2026-06-21
updated: 2026-06-29
---

# Purpose 目的

知識庫最高規範。定義核心原則、知識架構、Metadata 標準、安全規則、Agent 行為。
詳細規範由 policy.md 路由管理。

---

# Core Domains 核心領域

僅允許處理：台灣股市 / 美國股市 / Hermes Agent / Obsidian / AI·LLM
超出範圍需取得使用者確認。

---

# Required Navigation 強制導航

首次任務強制執行（同一工作階段僅一次）：

```python
read("schema.md") → read("policy.md") → read("index.md") → read("log.md", last=30)
```

完成後輸出：
```
[1/4] ✅ SCHEMA | [2/4] ✅ POLICY | [3/4] ✅ INDEX | [4/4] ✅ LOG → 導航完成
```

**禁止跳過**，除非用戶明確說「跳過導航」。任何理由（記憶中已有 / 任務簡單 / 時間緊迫）均不得作為跳過藉口。未輸出完成標記 = 未完成導航 = 禁止執行任務。

---

# Cache Strategy 快取策略

後續任務使用快取：schema.md / policy.md / index.md / 各目錄 index.md

重新載入條件：schema 或 policy 更新 / 結構變更 / 使用者要求

---

# Metadata Standards 資料標準

所有 Layer 2 頁面必填：

```yaml
title: ""        # 永遠用 double quote
description: ""  # 永遠用 double quote
summary: ""      # 永遠用 double quote
type:
tags: []         # 最多 10 個，僅從核心 48 tag 選取，禁止新增（列表見 system/frontmatter-rules.md）
status:
created:
updated:
```

欄位格式、必填項目、YAML 完整性與 48 核心 tag 完整清單，見 [[frontmatter-rules]]（唯一細節來源）。

---

# Type Pool 類型池

```text
entity | concept | project | resource | report | query | task | index | log | schema
```

完整定義與語意說明見 [[frontmatter-rules]]（唯一細節來源）。

禁止自行新增。

---

# Status Pool 狀態池

```text
draft | active | permanent | archived | deprecated
```

完整定義見 [[frontmatter-rules]]（唯一細節來源）。

預設：`status: active`

---

# Wiki-LLM Architecture 架構

## Layer 1 — `raw/`

用途：原始資料（新聞 / PDF / 財報 / 網頁）
權限：**唯讀** — 禁止修改 / 刪除 / 搬移 / 重新命名

## Layer 2

```
clippings/ concepts/ entities/ finance/ queries/ reports/ resources/ skills/ system/
```

用途：結構化知識 / 知識圖譜 / Agent 工作區
權限：可讀可寫

---

# Knowledge Principles 知識原則

1. 更新優先於建立
2. 避免重複頁面與重複知識
3. 維護圖譜完整性，禁止孤立節點
4. 建立頁面後更新 Index 與 Log

---

# Page Creation Pre-check — 新增頁面查重規範

建立任何新頁面 **之前**，必須執行以下步驟：

## 步驟 1：掃描現有內容

搜尋 vault 中是否有相同或相近主題的頁面：

```bash
# 用關鍵詞搜尋 title / description / tags / 檔案名
grep -ri "<關鍵詞>" --include="*.md" . | grep -v "raw/" | grep -v "ivan-notes/"
```

同時檢查各目錄 index 頁面確認無重複。

## 步驟 2：回報使用者

若找到相同或相近內容，必須回報：

```
⚠️ 發現相似頁面：
- existing-page-1（行數、簡述重疊處）
- existing-page-2（行數、簡述重疊處）

建議方案：
A) 合併至現有頁（推薦）
B) 編輯現有頁補充新內容
C）獨立建立新頁（說明理由）

請選擇？
```

## 步驟 3：依使用者決定執行

- 使用者選 A / B → **不建立新頁**
- 使用者選 C → 建立新頁，並在 frontmatter `description` 註明與相似頁面的差異

## 適用範圍

- 所有 Layer 2 新_page（concepts/ entities/ finance/ queries/ reports/ resources/ skills/ system/）
- obsidian/ 與 clippings/ 資料夾同樣適用
- update 現有頁面不在此限

---

# Rule Loading 規範讀取

規則唯一來源：`schema.md` / `policy.md` / `system/*`
禁止在 Skill 中重複定義規則，禁止多檔維護不同版本規則。

## system/ 規則檔現狀（2026-07-10）

| 檔案 | 職責 |
|------|------|
| `system/folder-rules.md` | 目錄結構與讀寫權限（原 folder-structure.md 更名） |
| `system/frontmatter-rules.md` | Frontmatter 唯一細節來源（schema.md 僅保留原則+索引） |
| `system/user-backup-rules.md` | 備份規範 + user-backup 編排器（合併原 backup-rules + user-backup-skill） |
| `system/skills-rules.md` | Skills 架構規範 |
| `system/database-rules.md` | 資料庫操作規範 |
| `system/quartz-rules.md` | Quartz 發布規範 |
| `system/telegram-output-rules.md` | Telegram 輸出規範 |
| `system/vps-config.md` | VPS 配置 |
| `system/system-index.md` | system 目錄索引 |

> ⚠️ 舊名 `folder-structure.md` / `backup-rules.md` / `skills/user-backup-skill.md` 已於 2026-07-10 廢除，相關連結請改用上表新名。

---

# Superpowers 流程（寫新 Skill 強制遵循）

**順序不可逆，禁止跳步，每階段結束必須停止等用戶確認：**

1. **brainstorming** → 探索需求→提問→2-3方案→用戶批准設計（HARD GATE：未批准前禁止任何實作）
2. **writing-plans** → bite-sized TDD 任務（每步含完整程式碼+驗證指令+預期結果，禁止 TBD/placeholder）
3. **executing-plans** → 逐 task RED→GREEN→REFACTOR
4. **writing-skills** → TDD for documentation + SDO（description 只寫觸發條件不寫流程）
5. **verification-before-completion** → 跑驗證指令→看輸出→才宣稱完成（證據先於宣稱）
6. **finishing-a-development-branch** → 測試全過→commit/PR

**紅線**：直接寫 code = 違規。「應該可以」/「看起來沒問題」= 違規。跳過階段 = 違規。

---

# Structural Changes 結構性變更

以下操作需使用者核准：新增 / 刪除 / 重命名資料夾 / 修改 schema·policy·index

流程：`方案 → 影響評估 → 使用者核准 → 執行 → 記錄 Log`

---

# Safety Rules 安全規則

**禁止執行**：`rm -rf` / 批次刪除 / 批次搬移 / 批次重命名

**禁止修改**：`raw/` / `SCHEMA.md` / `POLICY.md` / `index.md` / `log.md`

**禁止編輯、移動、刪除**：`ivan-notes/`（唯讀，Agent 不得修改，需經使用者同意）

**禁止刪除**：`database/` / `skills/` / `system/`

**禁止編輯、移動、刪除**：`copilot/` / `.claude/` / `.claudian/`（外部工具管理區，Agent 不得修改）

未取得核准不得執行。

---

# WebDAV 寫入權限（Post-creation 強制）

Agent 以 `root` 建立檔案，Nginx 以 `www-data` 運行 → 未修正權限會 403。

**每次建立或修改檔案後立即執行**：

```bash
sudo chown root:www-data "<檔案路徑>"
sudo chmod 664 "<檔案路徑>"
```

適用範圍：`write_file` 新建 / `mkdir` 新建目錄 / `patch` 後新建子目錄

常見陷阱：只改目錄忘改目錄內檔案 / 使用 `chmod 777`（禁止）

**批次修復指令**（同步失敗時執行）：

```bash
cd "/root/Documents/Obsidian Vault/"
# 修復擁有者
find . -name "*.md" -not -path "./raw/*" -not -path "./ivan-notes/*" -not -path "./.git/*" | while read f; do
  owner=$(stat -c '%U:%G' "$f")
  perm=$(stat -c '%a' "$f")
  if [ "$owner" != "root:www-data" ] && [ "$owner" != "www-data:www-data" ]; then
    sudo chown root:www-data "$f"
  fi
  if [ "$perm" != "664" ]; then
    sudo chmod 664 "$f"
  fi
done
```

**根因**：`write_file` 新建檔案預設為 `root:root 644` 或 `600`，www-data 無寫入權限 → WebDAV 同步失敗。

**預防**：在 `~/.bashrc` 或 agent 環境中設定 `umask 002`，確保新檔案預設為 664。

---

---

# Failure Protection 故障防護

```yaml
max_retry: 3
```

連續失敗三次輸出：
```
[STOP] Task failed 3 times. Awaiting user decision.
```

禁止：無限重試 / 無限建立·搬移·刪除·修改同一頁

---

# Page Size Limits 頁面上限

| Type                                | 建議上限                  |
| ----------------------------------- | --------------------- |
| query / concept / entity / resource | 200 行                 |
| task                                | 100 行（超過建議升格 project） |
| report / project                    | 300 行                 |
| index / schema                      | 無上限（offset 讀取）        |
| log                                 | 無硬上限（offset=-30 讀尾部）  |

超限建議拆分，提交方案後需使用者核准，禁止自行拆分。

**Log Rotation**：每 300 條封存為 `log-YYYY.md`，log.md 保留最新 300 條。

---

# Frontmatter Safety 前言安全

## 通用原則

- `title` / `description` / `summary` 永遠用 double quote 包裹
- frontmatter 內禁止 `[[wikilink]]` 及任何 Markdown 語法
- frontmatter 僅允許：string / number / boolean / array / object
- 所有 list 型欄位（`tags` / `aliases` / `sources` / `related`）必須有明確 key
- 建立或修改 frontmatter 後必須執行 `yaml.safe_load()` 驗證

完整格式規則、必填項目、禁止欄位、頁面範例與常見錯誤速查，見 [[frontmatter-rules]]（唯一細節來源）。

## copilot/ 資料夾專用規則

`copilot/` 為外部工具管理區（Level 3 保護，禁止編輯/移動/刪除）。其 frontmatter 欄位與標準不同：

**必填欄位**：
- `copilot-command-context-menu-enabled`: true
- `copilot-command-slash-enabled`: true
- `copilot-command-context-menu-order`: 0
- `copilot-command-model-key`: ""
- `copilot-command-last-used`: 0

**允許的額外 key**：以上 5 個 copilot 專用 key 只在 `copilot/` 資料夾內合法。

```yaml
# copilot/ 檔案 frontmatter 範例
---
copilot-command-context-menu-enabled: true
copilot-command-slash-enabled: true
copilot-command-context-menu-order: 0
copilot-command-model-key: ""
copilot-command-last-used: 0
---
```

---

# Constitution 憲法

1. 保護資料優先於完成任務
2. 三次失敗立即停止
3. 更新優先於建立，不建立重複知識
4. 不產生孤立節點，不修改 Layer 1 原始資料
5. 重大變更必須取得使用者核准
6. 遵循 policy 路由 / skill 詳細規範
7. 保持知識庫一致性與可維護性
