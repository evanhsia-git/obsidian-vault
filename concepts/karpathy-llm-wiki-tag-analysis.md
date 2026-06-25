---
status: active
title: "Karpathy LLM Wiki 核心原則與標籤系統分析"
created: 2026-06-20
updated: 2026-06-20
type: concept
tags: [obsidian, rag, knowledge, performance]
summary: "Karpathy LLM Wiki 核心原則分析，探討標籤系統角色定位與 RAG 查詢品質提升策略"
---

# Karpathy LLM Wiki 核心原則與標籤系統分析

## Karpathy LLM Wiki 的核心原則

Karpathy 的 LLM Wiki 核心差異在於：不像 RAG 每次查詢都從原始文件重新推導答案，而是將知識預先編譯成結構化、互相連結的實體頁面，讓知識隨時間複利累積。

### 自給自足原則（Self-Contained）

Karpathy 的核心主張是：每個知識單元必須自給自足。如果 Agent 取回一個 chunk，它不需要再去抓另外三個 chunk 才能理解完整脈絡。這意味著分割文件要依語意邊界切割，而不是任意 token 數。

### 原始設計精簡

Karpathy 的原始 gist 根本沒有定義標籤系統。他的原始設計僅定義了三層架構：

| 層級 | 用途 |
| :--- | :--- |
| `raw/` | 原始素材 |
| `wiki/` | 知識頁 |
| `schema/` | 規則 |

以及三個操作：`ingest` / `query` / `lint`，刻意保持精簡。

社群中認真使用的人，最終都在自己重新發明傳統知識管理學（Zettelkasten、型別分類、引用模型等）。

---

## 標籤池的角色定位問題

### 目前標籤結構分析

| 分類 | 數量 | 觀察 |
| :--- | :--- | :--- |
| 台灣股市 | 13 | 財務指標細緻（pe-ratio, pb-ratio, roe…） |
| Hermes Agent | 9 | 工具導向（telegram, notion, obsidian…） |
| LLM/AI/RAG | 10 | 技術概念（rag, embedding, vector-db…） |
| 系統維護 | 13 | 管理行為（index, log, maintenance…） |

### 問題：標籤扮演了兩種衝突角色

**角色 A（分類/導航）**：`taiwan-stock`、`hermes`、`rag`——給人類瀏覽用的。

**角色 B（Agent 查詢過濾）**：在 Karpathy 架構下，Agent 靠的是 `` 圖譜遍歷 + 語意搜尋，不是靠標籤過濾。系統維護類標籤（index、log、maintenance）對 Agent 幾乎沒有 RAG 價值，只是 lint 用的 metadata。

---

## 標籤池擴大的優缺點

### 優點

- 更細粒度過濾（例如區分 `finmind-api` vs `finmind-data`）
- 跨域標籤更易發現意外關聯（例如 `backtesting` 同時標記在 entities + concepts）
- Hermes 可以用標籤做第一層粗篩，再用 wikilinks 做精確路徑

### 缺點

- **標籤漂移**：標籤越多，Agent 選擇時越容易打出近義但不同的標籤，導致分類混亂
- **維護成本**：每次新頁面需要決策的認知負擔增加
- **Karpathy 悖論**：他的系統根本不依賴標籤——加再多標籤，對 LLM 語意理解的幫助也不如一個好的 `summary:` 欄位

---

## 實際建議

### 1. 拆分標籤用途，分兩層管理

```yaml
# 導航標籤（人類用）→ 維持現有，嚴格管控
tags: [taiwan-stock, hermes, rag]

# Agent 行為標籤（機器用）→ 另開 category 欄位
category: entity | data-source | api | skill | concept
```

### 2. 補 10–12 個精準缺口標籤，而非無限擴張

| 缺口 | 建議新增標籤 |
| :--- | :--- |
| LLM 廠商區分 | `openai` `anthropic` `google-gemini` `deepseek` |
| 台股資料流 | `websocket` `batch-fetch` `realtime` |
| Hermes 輸出類型 | `report` `alert` `signal` |
| Agent 技術 | `mcp` `tool-use` `multi-agent` |

這樣加 10–12 個精準標籤，比把池子擴到 80 個有用。

### 3. 最重要：強化 summary: 欄位

Karpathy 架構的實踐者指出：每篇筆記的 summary 單行描述與一致的標籤，對 LLM 查詢品質的提升幅度遠超過複雜的分類體系。

目前 Frontmatter 中 `summary` 是選用欄位——對 Hermes 而言應該升格為必填，這才是真正提升 RAG 命中率的槓桿。

---

## 結論

**不用擴大標籤池，改做這三件事效益更高：**

1. 補 10–12 個精準缺口標籤
2. `summary` 升格必填
3. 系統維護類標籤（index、log、maintenance）考慮移出 tags，改為純 type 欄位控制

---
## 相關節點
- [[llm-wiki-comparison]]
- [[hermes-workflow]]