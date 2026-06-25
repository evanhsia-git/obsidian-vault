---
title: "schema"
description: "Hermes Agent 核心憲法"
summary: "Obsidian Wiki、Wiki-LLM、Hermes Agent 核心規範"
version: "3.2"
type: schema
status: permanent
tags: [hermes, obsidian]
created: 2026-06-21
updated: 2026-06-21
---

# Purpose

本文件為知識庫最高規範。

定義：

- 核心原則
- 知識架構
- Metadata 標準
- 安全規則
- Agent 行為

詳細規範由 policy.md 路由管理。

---

# Core Domains

僅允許處理：

- 台灣股市
- 美國股市
- Hermes Agent
- Obsidian
- AI / LLM

超出範圍需取得使用者確認。

---

# Required Navigation

首次任務：

```python
read("schema.md")
read("policy.md")
read("index.md")
read("log.md", last=30)
```

完成後：

```text
[1/4] ✅ SCHEMA | [2/4] ✅ policy | [3/4] ✅ index | [4/4] ✅ log → 導航完成
```

同一工作階段僅執行一次。

## Navigation Enforcement（強制機制）

**禁止跳過導航**，除非用戶明確說「跳過導航」。

Agent 必須在首次任務開始前完成導航序列，並在對話中明確回報導航完成標記。

**繞過防護**：
- Agent 不得因「記憶中已有導航內容」而跳過
- Agent 不得因「任務簡單」而跳過
- Agent 不得因「時間緊迫」而跳過
- 唯一例外：用戶明確說「跳過導航」

**驗證方式**：Agent 必須在對話中輸出導航完成標記，格式為：
```
[1/3] ✅ SCHEMA | [2/3] ✅ index | [3/3] ✅ log → 導航完成
```

未輸出此標記 = 未完成導航 = 不得執行任何任務。
```

同一工作階段僅執行一次。

---

# Cache Strategy

後續任務使用快取。

快取內容：
- schema.md（結構定義）
- policy.md（規則路由）
- index.md（頁面索引）
- 各目錄 index.md（局部索引）

重新載入條件：
- schema.md 更新
- policy.md 更新
- 結構變更
- 使用者要求

---

# Metadata Standards

所有 Layer 2 頁面必須包含：

```yaml
title:
type:
tags:
summary:
created:
updated:
```

**Tags 規範**：

- 整個 Vault 只能使用 **32 個核心 tag**（由 frontmatter-rules 定義），禁止新增
- 單一頁面最多 **10 個** tag
- 必須從核心列表中選取，不得自創

---

---

# Type Pool

僅允許：

```text
entity
concept
project
resource
report
query
task
index
log
schema
```

禁止自行新增 Type。

---

# Status Pool

僅允許：

```text
draft
active
permanent
archived
deprecated
```

定義：

```text
draft       草稿
active      使用中
permanent   長期知識
archived    已封存
deprecated  已淘汰
```

預設：

```yaml
status: active
```

---

# Wiki-LLM Architecture

## Layer 1

```text
raw/
```

用途：

- 原始資料
- 新聞
- PDF
- 財報
- 網頁內容

權限：

```text
唯讀
```

禁止：

```text
修改
刪除
搬移
重新命名
```

---

## Layer 2

```
concepts/
entities/
queries/
reports/
resources/
skills/
system/
```

用途：
- 結構化知識
- 知識圖譜
- Agent 工作區
權限：

```
可讀可寫
```

---

# Knowledge Principles

1. 更新優先於建立
2. 避免重複頁面
3. 避免重複知識
4. 維護圖譜完整性
5. 禁止孤立節點
6. 建立頁面後更新 Index
7. 建立頁面後更新 Log

---

# Rule Loading

詳細規範由 policy.md 管理。

禁止：

```text
在 Skill 中重複定義規則
在多個檔案維護不同版本規則
```

規則唯一來源：

```
schema.md
policy.md
system/*
```

---

# Structural Changes

以下操作需取得使用者核准：

- 新增資料夾
- 刪除資料夾
- 重新命名資料夾
- 修改 schema
- 修改 policy
- 修改 index

流程：

```text
方案
↓
影響評估
↓
使用者核准
↓
執行
↓
記錄 Log
```

---

# Safety Rules

禁止：

```text
rm -rf
批次刪除
批次搬移
批次重新命名
```

禁止修改：

```
raw/
schema.md
policy.md
```

禁止刪除：

```
database/
skills/
system/
```

未取得核准不得執行。

---

# WebDAV 寫入權限（Post-creation 強制）

Agent 以 `root` 建立檔案，Nginx 以 `www-data` 運行 WebDAV。若新檔案擁有者為 `root:root` 且權限為 644，`www-data` 無法寫入 → 403。

**規則**：在 Obsidian Vault 下建立或修改任何檔案後，必須執行權限確保：

```bash
sudo chown root:www-data "<新檔案路徑>"
sudo chmod g+rwx "<新檔案路徑>"
```

**適用範圍**：
- `write_file` 建立的新檔案
- `terminal` 中 `mkdir` 新建的目錄
- `patch` 修改後需要存檔的檔案（權限通常已正確，但新建子目錄時需檢查）

**常見陷阱**：
- ❌ 只改目錄權限，忘記改目錄內新建檔案
- ❌ `chmod 777` 不安全，用 `g+rwx` + `chown root:www-data` 即可
- ❌ 在 `concepts/`、`skills/`、`entities/` 等子目錄建立新頁面後未執行權限修正

**強制執行時機**：
1. `write_file` 後 → 立即 `chown + chmod`
2. `terminal` 中 `mkdir` 後 → 立即 `chown + chmod`
3. `obsidian-lint` 掃描發現權限時 → 立即批次修正

---

# Failure Protection

最大重試：

```yaml
max_retry: 3
```

連續失敗三次：

```text
[STOP]
Task failed 3 times.
Awaiting user decision.
```

禁止：

- 無限重試
- 無限建立檔案
- 無限搬移檔案
- 無限刪除檔案
- 無限修改同一頁

---

# Page Size Limits

**query**：> 200 行建議拆分（需使用者同意）

**task**：> 100 行建議升格為 project（需使用者同意）

**concept / entity / resource**：> 200 行建議拆分（需使用者同意）

**report / project**：> 300 行建議拆分（需使用者同意）

**index / schema**：不設上限，Agent 使用 offset 讀取

**log**：不設硬上限，Agent 使用 offset=-30 讀尾部，頁面總長度對 Agent 無影響

拆分原則：提交方案供使用者核准後執行，不得自行拆分。

## Log Rotation

log.md 每 300 條輪轉封存為 log-YYYY.md。

封存後 log.md 只保留最新 300 條，舊資料移至 log-YYYY.md（按年份）。
```

---

# Frontmatter Safety

## 問題類型

### 1. Wikilink 在 frontmatter 內

**錯誤**：
```yaml
title: My-Page
- [[openrouter-free-models]]
```

**原因**：`- [[...]]` 無 key，且 `[[` 在 YAML 中可能導致 parse failure。

**規則**：
- frontmatter 內禁止 `[[wikilink]]`
- frontmatter 內禁止 markdown 語法（`**bold**`、`` `code` ``、`[link](url)`）
- frontmatter 僅允許：string、number、boolean、array、object
- 所有 wikilink 必須放在 `---` 之後的正文區域

**修正**：將 wikilink 移至 body，補上合法的 key 或刪除。

---

### 2. Summary 值含特殊字元未加 quote

**錯誤**：
```yaml
summary: 2026-06-01 Summary：- Executed `skill-name`: completed task
```

**原因**：`：`（full-width colon）後接空格被 YAML parser 解讀為 key-value separator；`` ` `` 和 `:` 也會導致解析失敗。

**規則**：
- `summary` 值含以下字元時必須用 double quote 包裹：`:`、`#`、`[`、`]`、`{`、`}`、`` ` ``、`|`、`>`、`!`、`%`、`@`、`&`、`*`
- 最佳實踐：**所有 summary 值一律用 double quote 包裹**

**修正**：
```yaml
summary: "2026-06-01 Summary - Executed skill-name, completed task"
```

---

### 2b. Summary 值含逗號後接空格+小寫字母（Quartz YAML 解析問題）

**錯誤**：
```yaml
summary: "2026-06-01 Summary - Executed skill-name, completed backup of Hermes configuration"
```

**原因**：Quartz 使用的 YAML parser 會將 `, completed backup...` 誤判為新的 mapping entry（`key: value` 格式），導致 `bad indentation of a mapping entry` 錯誤。

**規則**：
- summary 值應避免逗號後接空格+小寫字母的結構
- 若需要逗號，改用全形逗號 `，` 或精簡內容

**修正**：
```yaml
summary: "2026-06-01 Summary - Executed skill-name, completed backup"
```

---

### 3. Frontmatter 缺少 key 的 list item

**錯誤**：
```yaml
title: My-Page
- [[page-a]]
- [[page-b]]
```

**原因**：YAML list item 必須有 key（`related: - page-a`），不能直接在頂層。

**規則**：
- frontmatter 所有項目必須有 key
- 關聯頁面使用 `related` key，放在 body 而非 frontmatter

---

### 2c. title 或 description 含冒號未加 quote（Quartz YAML 解析問題）

**錯誤**：
```yaml
---
title:Awesome DESIGN.md
description: VoltAgent 的 DESIGN.md 檔案集合，從真實網站萃取設計系統格式
summary: Awesome DESIGN.md 是 Google Stitch 設計系統格式的公司集合
---
```

**原因**：`title` 值含冒號 `Awesome DESIGN.md` 本身無冒號但 `description` 和 `summary` 中的 `：` 或 `:` 会被 YAML parser 解讀為 key-value separator，導致 `end of the stream or a document separator is expected`。

**規則**：
- `title`、**永遠用 double quote 包裹**
- `description`：**永遠用 double quote 包裹**
- `summary`：**永遠用 double quote 包裹**
- 原因：這三個欄位常見包含冒號、全形逗號、括號等特殊字元，直接引用風險極高

**修正**：
```yaml
---
title: "Awesome DESIGN.md"
description: "VoltAgent 的 DESIGN.md 檔案集合，從真實網站萃取設計系統格式"
summary: "Awesome DESIGN.md 是 Google Stitch 設計系統格式的公司集合"
---
```

---

# Constitution

1. 保護資料優先於完成任務
2. 三次失敗立即停止
3. 更新頁面優先於建立頁面
4. 不建立重複知識
5. 不產生孤立節點
6. 不修改 Layer 1 原始資料
7. 重大變更必須取得使用者核准
8. 遵循 policy 路由規範
9. 遵循 skill 詳細規範
10. 保持知識庫一致性與可維護性