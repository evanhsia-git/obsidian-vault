---
status: active
title: "LLM Wiki 核心概念"
summary: "LLM Wiki 核心概念：核心思想：從 RAG 到 Wiki"
created: 2026-05-28
updated: 2026-05-28
type: concept
tags: [obsidian, ai, knowledge]
---

# LLM Wiki 核心概念

> 來源：[Andrej Karpathy](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)（前 Tesla AI 總監、OpenAI 創始成員）

## 核心思想：從 RAG 到 Wiki

### 傳統 RAG 的缺點
- 每次提問時，從原始文件中檢索相關片段，重新拼湊答案
- 每次都要「重新發現」知識，無累積效果
- 矛盾沒有被標記，交叉引用不存在

### LLM Wiki 的優勢
- LLM 持續維護一個結構化的 Markdown 知識庫
- 知識會隨時間累積、互聯、修正
- 交叉引用已經存在，矛盾已經標記，綜合已經反映你讀過的一切

> **關鍵句**：「Wiki 是一個持久、複利的產物。」

### 類比
> Obsidian 是 IDE，LLM 是程式設計師，Wiki 是程式碼庫。

---

## 三層架構

```
wiki/
├── SCHEMA.md           # Layer 3：規範定義（Conventions, structure rules, domain config）
├── index.md            # 內容目錄（Sectioned content catalog with one-line summaries）
├── log.md              # 維護日誌（Chronological action log, append-only, rotated yearly）
│
├── raw/                # Layer 1：不可變來源素材（Immutable source material）
│   ├── articles/       # 網路文章、剪報
│   ├── papers/         # PDF、arxiv 論文
│   ├── transcripts/    # 會議記錄、訪談
│   └── assets/         # 圖片、圖表
│
├── entities/           # Layer 2：實體頁（people, orgs, products, models）
├── concepts/           # Layer 2：概念/主題頁
├── comparisons/        # Layer 2：並排分析頁
└── queries/            # Layer 2：查詢結果頁
```

### 權限分配

| 層級 | 目錄/檔案 | 擁有者 | 可修改？ |
|------|-----------|--------|----------|
| Layer 3 | `SCHEMA.md` | Agent | ✅ |
| Layer 2 | `concepts/`, `entities/`, `comparisons/`, `queries/` | Agent | ✅ |
| Layer 1 | `raw/` | 人類（原始來源） | ❌ 不可修改 |
| 核心 | `index.md`, `log.md` | Agent | ✅ |

---

## 核心操作流程

### 1. 攝取（Ingest）
當提供來源（URL、檔案、貼上的文字）時：
1. **Capture**：將原始來源存入 `raw/` 對應子目錄，附加 frontmatter（source_url, ingested, sha256）
2. **Discuss**：與用戶討論重點（自動化情境可跳過）
3. **Check**：搜尋現有頁面避免重複
4. **Write/Update**：符合門檻才建新頁（2+ 來源提及或單一來源核心）
5. **Cross-reference**：每頁至少 2 個 wikilinks
6. **Report**：列出所有新增/更新的檔案

### 2. 查詢（Query）
1. 讀 `index.md` 識別相關頁面
2. 讀取相關頁面
3. 綜合回答並引用來源頁面
4. 有價值的答案存入 `queries/`
5. 更新 `log.md`

### 3. 維護（Lint）
定期執行健康檢查：
- 孤兒頁（無入站連結）
- 損壞連結（指向不存在的頁面）
- 索引完整性（所有頁面皆在 `index.md` 中）
- Frontmatter 驗證
- 過期內容（90 天未更新）
- 矛盾頁面
- 日誌輪轉（超過 500 條時）

---

## 頁面類型

### Concept Pages（概念頁）
- 每個概念或主題一頁
- 包含：定義/解釋、知識現狀、開放問題、相關概念、來源參考

### Entity Pages（實體頁）
- 每個重要實體一頁（公司、組織、工具、模型）
- 包含：概述、關鍵事實與日期、與其他實體關係、來源參考

### Comparison Pages（比較頁）
- 並排分析（表格格式優先）
- 包含：比較標的與原因、比較維度、結論或綜合、來源

### Query Pages（查詢頁）
- 值得保留的查詢結果
- 包含：原始問題、綜合答案、引用頁面、執行日期

---

## Frontmatter 規範

```yaml
---
title: 頁面標題
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: entity | concept | comparison | query | index | log | schema
tags: [從 SCHEMA.md 標籤分類選取]
sources: [raw/articles/source-name.md]
# 選用品質標記：
confidence: high | medium | low
contested: true
contradictions: [other-page-slug]
---
```

### Raw Sources Frontmatter
```yaml
---
source_url: https://example.com/article
ingested: YYYY-MM-DD
sha256: <hex digest>
---
```

---

## 命名規範
- 小寫、連字號、無空格（例：`transformer-architecture.md`）
- 每頁至少 2 個出站 ``
- 修改頁面時，務必更新 `updated` 日期
- 新頁面必須加入 `index.md`
- 每個動作必須附加到 `log.md`

---

## Page Thresholds（頁面建立門檻）
- **建立新頁**：當實體/概念在 2+ 來源出現，或是單一來源的核心主題
- **更新現有頁**：來源提及已涵蓋的主題時，添加資訊而非新建
- **不建立頁面**：僅提及名稱、次要細節、超出領域範圍
- **拆分頁面**：當頁面超過 ~200 行
- **封存頁面**：內容完全被取代時，移至 `_archive/`

---

## Update Policy（更新政策）
1. 檢查日期 — 較新來源通常取代較舊來源
2. 若確實矛盾，記錄雙方觀點並標註日期與來源
3. 在 frontmatter 標記：`contradictions: [page-name]`
4. 在 lint 報告中標記供使用者審查

---

## 相關頁面
- [[schema]] — 本 Wiki 的規範定義
- [[index]] — 頁面目錄
- [[log]] — 維護日誌