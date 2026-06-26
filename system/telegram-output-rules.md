---
title: "TELEGRAM-OUTPUT-RULES"
type: schema
status: permanent
summary: "Telegram Bot 輸出格式規範"
tags: [telegram, agent, flow, obsidian]
created: 2026-06-21
updated: 2026-06-21
---

# Purpose

確保訊息可於：

- Telegram Web
- Telegram Desktop
- Telegram Android
- Telegram iOS

正常顯示。

---

# Output Priority

優先順序：

```text
純文字
↓
簡易 Markdown
↓
HTML
```

禁止依賴複雜格式。

---

# Forbidden

禁止：

## Markdown Table

```markdown
| 欄位 | 數值 |
|------|------|
| EPS | 12.5 |
```

原因：

- Telegram 不保證支援
- Web 容易跑版
- Mobile 顯示不一致

---

## 巢狀清單

禁止：

```markdown
- 項目
  - 子項目
    - 子子項目
```

改為：

```text
• 項目

  - 子項目

    * 子子項目
```

---

## HTML Table

禁止：

```html
<table>
```

---

## 超寬內容

禁止單行超過：

```text
80字元
```

---

# Preferred Format

## 標題

```text
【每日台股新聞】
```

---

## 區塊

```text
■ 台積電

EPS：12.5
PE：18
PB：4.2
```

---

## 條列

```text
• 第一項
• 第二項
• 第三項
```

---

## 排名

```text
1. 台積電
2. 聯發科
3. 鴻海
```

---

# Stock Report Format

推薦：

```text
【個股分析】

股票：台積電

EPS：12.5
PE：18
PB：4.2

評價：

合理

結論：

可持續追蹤
```

---

# News Format

推薦：

```text
【今日重點新聞】

1. FED 維持利率不變

摘要：
市場預期降息延後。

影響：
利多金融股。

--------------------

2. 台積電擴產

摘要：
先進製程需求增加。

影響：
利多半導體。
```

---

# Length Control

單則訊息建議：

```text
1000字以內
```

最大：

```text
3000字
```

超過需拆分。

---

# Emoji Rules

允許：

```text
✅
⚠️
📈
📉
💰
📰
```

避免過量使用。

---

# Compatibility Goal

輸出必須同時適用：

- Telegram Web
- Telegram Desktop
- Telegram Android
- Telegram iPhone

不得依賴：

- Markdown Table
- HTML Table
- CSS
- 自訂格式

---
## 相關節點
- [[schema]]