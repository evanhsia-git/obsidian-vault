---
status: active
title: "Karpathy-Llm-Wiki-Gist"
summary: "Karpathy LLM Wiki 模式——以相互連結 Markdown 檔案構建 LLM 知識庫。"
created: 2026-05-31
updated: 2026-06-03
type: concept
tags: [obsidian, ai]
---

## 相關頁面
- [[concepts/llm-wiki-concept|LLM Wiki 概念]]
- [[SCHEMA|Wiki 規範]]

# LLM Wiki - Andrej Karpathy

原文來源：https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f

---

我一直在使用一種簡單的模式，這使大語言模型（LLM）對我來說更有用。

核心概念：給 LLM 一個持久且不斷累積的知識庫，以相互連結的 Markdown 檔案形式存在——也就是一個 Wiki。這不是那種每次提問都從頭開始搜尋的 RAG（檢索增強生成），而是一個編譯並保持更新的 Wiki。

## 該模式
1. 建立一個 Markdown 檔案目錄。
2. 當您閱讀或寫下有趣的事物時，將其總結為一個或多個 Markdown 檔案存入 Wiki，並在檔案之間建立 `[[wikilinks]]`。
3. 由代理（Agent）維護這個 Wiki——它負責寫入、讀取並保持內容一致性。
4. 當您提問時，Agent 會閱讀 Wiki 並從編譯好的知識中綜合答案。

## 為什麼這很有效

傳統 RAG：
- 每個問題 → 從原始文件中檢索片段 → LLM 綜合結果。
- 這意味著每次查詢都是從零開始「重新發現」知識。

Wiki 模式：
- 知識已經過編譯與交叉引用，矛盾之處也已被標記。
- Wiki 反映了迄今為止所吸收的一切。
- 查詢是從已編譯的 Wiki 中回答，而不是從零開始。

## Wiki 是一個「持久且不斷累積的產物」
「交叉引用已經存在。矛盾之處已經被標記。綜合結果反映了所吸收的一切。」

## Obsidian 的類比
> 「Obsidian 是集成開發環境（IDE），LLM 是程式設計師，而 Wiki 是程式碼庫（Codebase）。」

## 基本結構
```
wiki/
├── index.md            # 分類內容目錄
├── log.md              # 動作日誌 (僅能追加)
├── raw/                # 不可變來源
│   ├── articles/
│   ├── papers/
│   └── transcripts/
├── entities/           # 實體頁面
├── concepts/           # 概念頁面
├── comparisons/        # 比較頁面
└── queries/            # 已存檔的查詢結果
```
