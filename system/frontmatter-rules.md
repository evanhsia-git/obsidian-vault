---
title: FRONTMATTER-RULES
type: schema
status: permanent
summary: Obsidian Frontmatter 規範
tags: [frontmatter, rules, page-operations]
created: 2026-06-21
updated: 2026-06-21
---

# Purpose

統一 Metadata 格式。

目標：

* 提升 RAG 命中率
* 維持知識庫一致性
* 降低 Agent 維護成本
* 避免 Metadata 過度膨脹

---

# Standard Template

所有 Layer 2 頁面使用：

```yaml
---
title:
type:
tags:
summary:

created:
updated:

status: active

sources: []
related: []
---
```

---

# Required Fields

必填：

```yaml
title
type
tags
summary
created
updated
```

缺少任一欄位：

```text
Lint = P0
```

必須修復。

---

# Optional Fields

選填：

```yaml
status
sources
related
```

未使用可省略。

---

# Field Rules

## title

頁面名稱。

範例：

```yaml
title: Price Earnings Ratio
```

---

## type

只能使用：

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

禁止新增類型。

---

## tags

用途：

```text
搜尋
分類
過濾
```

規則：

```text
1~10 個
```

建議：

```yaml
tags:
  - ai
  - llm
  - rag
```

避免：

```yaml
tags:
  - ai
  - artificial-intelligence
  - machine-learning
  - deep-learning
  - technology
  - software
```

過度標記。

---

## summary

最重要欄位。

用途：

```text
RAG 檢索
搜尋結果
Agent 理解
```

規則：

```text
1~2 句
```

範例：

```yaml
summary: PE Ratio 用於衡量股票估值的核心財務指標。
```

---

## created

建立日期。

格式：

```yaml
created: 2026-06-21
```

建立後不再修改。

---

## updated

最後更新日期。

格式：

```yaml
updated: 2026-06-21
```

內容修改時同步更新。

---

## status

允許：

```text
draft
active
permanent
archived
deprecated
```

預設：

```yaml
status: active
```

---

## sources

來源追蹤。

範例：

```yaml
sources:
  - raw/articles/pe-ratio.md
```

規則：

* 可省略
* 建議研究型內容使用

---

## related

關聯頁面。

範例：

```yaml
related:
  - earnings-per-share
  - valuation
```

規則：

* 可省略
* 不取代 Wikilinks

---

# Page Examples

## Concept

```yaml
---
title: Price Earnings Ratio
type: concept
tags:
  - valuation
  - taiwan-stock

summary: PE Ratio 用於衡量股票估值的重要財務指標。

created: 2026-06-21
updated: 2026-06-21

status: active
---
```

---

## Entity

```yaml
---
title: Taiwan Semiconductor
type: entity
tags:
  - taiwan-stock

summary: 台積電為全球領先晶圓代工企業。

created: 2026-06-21
updated: 2026-06-21

status: active
---
```

---

## Report

```yaml
---
title: Weekly Market Report
type: report
tags:
  - taiwan-stock
  - market

summary: 本週市場重點與投資觀察。

created: 2026-06-21
updated: 2026-06-21

status: active
---
```

---

# Anti Patterns

禁止：

```yaml
draft: false
publish: true
subtype:
confidence:
priority:
importance:
owner:
reviewer:
version:
category:
```

原因：

```text
增加維護成本
降低一致性
對 RAG 幫助有限
```

---

# Wikilink Placement

[[Wikilinks]] 必須放在正文區域。

禁止：

```yaml
related:
  - [[page-name]]
  - [[another-page]]
```

原因：

```text
1. YAML 解析錯誤：[[ ]] 在 YAML 中可能導致 parse failure
2. 維護一致性：Frontmatter 僅存放結構化資料
3. 工具相容性：部分 SSG/Static Site Generator 不支援 frontmatter wikilinks
```

規則：

```text
Frontmatter 允許型別：string, number, boolean, array, object
Frontmatter 禁止型別：wikilink, markdown syntax, HTML tag
```

所有 [[wikilinks]] 必須放在 --- 之後的正文區域。

---

# Naming Rules

Frontmatter 不允許：

```yaml
Title:
Tags:
Created:
```

必須：

```yaml
title:
tags:
created:
```

全部小寫。

---

# Success Criteria

符合以下條件：

```text
所有頁面皆有 Frontmatter
所有頁面皆有 Summary
所有頁面皆有 Type
所有頁面皆有 Updated
```

則：

```text
Frontmatter Status: HEALTHY
```

---
## 相關節點
- [[schema]]