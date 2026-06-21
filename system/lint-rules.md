---
title: LINT-RULES
type: schema
status: permanent
summary: Obsidian Vault 健康檢查規範
tags: [lint, audit, health-check, rules]
version: "1.0"
created: 2026-06-21
updated: 2026-06-21
---

# Purpose

維護知識庫品質與圖譜完整性。

目標：

- 無孤立節點
- 無壞連結
- 無重複知識
- Frontmatter 一致
- Index 完整
- 圖譜可持續擴充

---

# P0 Critical

發現後立即修復。

## 孤立節點

條件：

- 無入站連結
- 或無出站連結

處理：

- 補充 Wikilinks
- 建立關聯頁面
- 合併重複內容

---

## 壞連結

條件：

```text
```

處理：

- 修正連結
- 建立缺失頁面
- 移除錯誤引用

---

## Frontmatter 缺失

必須存在：

```yaml
title:
type:
tags:
summary:
created:
updated:
```

處理：

- 自動補齊
- 更新格式

---

# P1 Important

## Index 收錄檢查

條件：

頁面未出現在：

```text
index.md
```

處理：

- 補入對應索引

---

## 重複頁面

條件：

主題高度重疊。

例如：

```text
pe-ratio.md
price-earnings-ratio.md
```

處理：

- 合併頁面
- 保留單一權威頁

---

## 命名規範

必須：

```text
kebab-case
```

例如：

```text
price-earnings-ratio.md
```

禁止：

```text
Price Earnings Ratio.md
price_ratio.md
價格比率.md
```

---

# P2 Maintenance

## 內容過期

條件：

```text
90 天未更新
```

建議：

- 標記 review
- 補充新資料

---

## 大型頁面

條件：

```text
超過 200 行
```

建議：

- 依主題拆分
- 建立子頁面

---

## 標籤檢查

條件：

```text
超過 10 個 Tags
```

建議：

- 精簡標籤
- 移除重複概念

---

# Graph Health

## 出站連結

每頁至少：

```text
2 個 Wikilinks
```

---

## 入站連結

每頁至少：

```text
1 個 Inbound Link
```

---

## Hub 節點

核心主題建議：

```text
5+ 關聯連結
```

例如：

- AI
- LLM
- Hermes Agent
- Obsidian
- 台灣股市
- 美國股市

---

# Raw Protection

確認：

```text
raw/
```

未被：

- 修改
- 刪除
- 搬移
- 重新命名

---

# Database Protection

確認：

```text
database/
```

不包含於：

- Quartz 發布內容
- Git 同步內容

---

# Weekly Audit

每週執行：

1. Lint Scan
2. 孤立節點檢查
3. 壞連結檢查
4. Frontmatter 檢查
5. Index 檢查
6. 過期內容檢查
7. Database 備份確認

輸出：

```text
reports/weekly-health-report.md
```

---

# Report Format

```text
[LINT REPORT]

P0
- 孤立節點：
- 壞連結：
- Frontmatter 缺失：

P1
- Index 缺漏：
- 重複頁面：
- 命名違規：

P2
- 過期頁面：
- 大型頁面：
- Tag 異常：

Graph Health
- 平均出站連結：
- 平均入站連結：

Recommendation
- 修復建議
- 整理建議
- 優化建議
```

---

# Success Criteria

符合以下條件：

```text
孤立節點 = 0
壞連結 = 0
Frontmatter 缺失 = 0
Index 缺漏 = 0
```

則：

```text
筆記庫 Vault Status: HEALTHY 正常狀態
```

---

# Priority

```text
P0 = 立即修復
P1 = 本次整理
P2 = 維護優化
```