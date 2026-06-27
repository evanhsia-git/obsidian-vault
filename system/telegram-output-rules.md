---
title: "TELEGRAM-OUTPUT-RULES"
type: schema
status: permanent
summary: "Telegram Bot 輸出格式規範"
tags: [telegram, agent, workflow]
created: 2026-06-21
updated: 2026-06-21
---

# Telegram Output Rules（Telegram 輸出規範）

## Purpose（目的）

所有 Telegram 訊息必須完全相容於：

* Telegram Web K
* Telegram Web A
* Telegram Desktop
* Telegram Android
* Telegram iPhone

**任何訊息都不得需要 Telegram Desktop 才能閱讀。**

**絕對禁止出現以下訊息：**

> This message is currently not supported on Telegram Web. Try getdesktop.telegram.org

**相容性永遠優先於美觀。**

---

# Critical Rules（最高優先規則）

以下規則屬於**強制規定（MUST）**，不得違反。

1. 一律優先輸出 UTF-8 純文字。
2. 相容性永遠優先於排版效果。
3. 不得使用任何 Telegram 專屬的新格式。
4. 若無法確認是否相容 Telegram Web，必須重新產生純文字版本。
5. 本文件優先於所有其他輸出格式規範。

---

# Output Priority（輸出優先順序）

所有 Telegram 訊息請依照以下順序產生：

```text
純文字
↓

簡易 Markdown
↓

HTML（僅在使用者明確要求時）
```

不得為了美觀而自動改用較新的 Telegram 格式。

---

# Forbidden（禁止事項）

## 禁止使用 Telegram 新版格式

不得產生或使用：

* Telegram Rich Table（新版表格）
* Native Telegram Table
* Expandable Table（可展開表格）
* Expandable Block（可展開區塊）
* Block Quote Entity（新版引用區塊）
* Expandable Block Quote
* Details Block
* Interactive Block
* Telegram Mini App 內容
* Unsupported Telegram Entity
* Telegram 未完全支援的新格式
* 未來新增但 Web 尚未支援的 Telegram 格式

若無法確認是否相容，一律禁止使用。

---

## 禁止 Markdown Table

禁止：

```markdown
| 欄位 | 數值 |
|------|------|
| EPS | 12.5 |
```

原因：

* Telegram Web 不保證支援
* 不同平台顯示結果可能不同
* 容易被轉換為 Telegram 新版表格

---

## 禁止 HTML Table

禁止：

```html
<table>
```

---

## 禁止複雜 Markdown

避免使用：

* 多層 Block Quote
* HTML 與 Markdown 混用
* 巢狀引用
* 進階 Markdown 擴充語法
* Telegram 專屬 Markdown

---

## 禁止深層巢狀清單

避免：

```text
- 第一層
  - 第二層
    - 第三層
```

建議：

```text
• 第一項

• 第二項

• 第三項
```

---

## 禁止超寬內容

單行建議：

```text
80 個字元以內
```

過長內容請自行換行。

---

# 建議輸出格式

## 標題

```text
【今日台股新聞】
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

# 股票分析格式

建議格式：

```text
【個股分析】

股票：
台積電

EPS：
12.5

PE：
18

PB：
4.2

評價：
合理

結論：
可持續追蹤
```

---

# 新聞格式

建議格式：

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

# 訊息長度

建議：

```text
1000 字以內
```

最大：

```text
3000 字
```

超過請自動拆成多則訊息。

---

# Emoji 使用規範

允許：

```text
✅
⚠️
📈
📉
💰
📰
```

避免過量使用 Emoji。

---

# Telegram 相容性檢查（Compatibility Check）

每次送出 Telegram 訊息前，必須確認：

* ✅ 為 UTF-8 純文字
* ✅ 可於 Telegram Web 顯示
* ✅ 可於 Telegram Desktop 顯示
* ✅ 可於 Telegram Android 顯示
* ✅ 可於 Telegram iPhone 顯示
* ✅ 未使用任何 Telegram 專屬 Rich Formatting
* ✅ 未使用任何新版 Telegram Entity
* ✅ 不需要 Telegram Desktop 才能閱讀

若任何一項無法確認：

**必須重新產生整則訊息，改為純文字格式。**

---

# 自動回退（Fallback Rule）

若偵測到：

* Telegram 新版格式
* Telegram Rich Table
* Telegram Web 不支援格式
* 無法確認相容性

必須立即重新產生：

```text
純文字
→ 簡易 Markdown
→ 純文字（必要時）
```

不得直接送出。

---

# 最終規則（Final Rule）

若發生衝突：

* 美觀
* 排版
* Telegram 新功能
* Rich Formatting
* Telegram Entity
* Telegram Web 相容性

**永遠優先選擇 Telegram Web 相容性。**

絕對不得產生任何可能導致下列訊息的內容：

> This message is currently not supported on Telegram Web. Try getdesktop.telegram.org

---

# Agent 執行要求（Execution Requirement）

在每次呼叫 Telegram 發送工具之前，Agent 必須：

1. 檢查輸出是否符合本規範。
2. 若有任何疑慮，自動重新產生純文字版本。
3. 不得因美觀、自動最佳化或新版 Telegram 功能而改變輸出格式。
4. 本規範屬於最高優先權，不得忽略或覆寫。

---

## 相關節點

* [[schema]]
