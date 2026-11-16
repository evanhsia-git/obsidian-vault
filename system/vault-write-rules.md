---
title: "Vault 頁面寫入規範"
summary: "Vault 頁面寫入規範：一律用 vault_new_page.py，自動補 frontmatter、修正權限（664/2775 setgid）、驗證 tag 白名單"
type: schema
status: active
tags: [obsidian, schema, hermes]
created: 2026-11-16
updated: 2026-11-16
---

**頁面寫入規範**：所有新增／修改 Vault 頁面一律透過工具，不要用 `write_file` 直寫。

## 用哪個工具

```bash
# 新建（自動組 frontmatter、修正權限、自我驗證）
/usr/bin/python3 /root/.hermes/scripts/vault_new_page.py --new "resources/x.md" \
    --type resource --tags etf,stock --summary "一句話摘要" --body-file /tmp/body.md

# 既有頁面補齊缺少欄位（只補不覆蓋，正文一字不動）
/usr/bin/python3 /root/.hermes/scripts/vault_new_page.py --fix "resources/x.md" --tags etf

# 編輯既有頁面後，只更新 updated
/usr/bin/python3 /root/.hermes/scripts/vault_new_page.py --touch "resources/x.md"
```

## 為什麼（兩個會反覆發生的問題）

| 問題 | 根因 | 工具的處置 |
|------|------|-----------|
| 新頁面缺 frontmatter | 只用 `write_file` 寫內容 → lint 必填 7 欄全缺，等每日 lint 才被發現 | 自動補 `title / type / tags / summary / created / updated / status`，缺任一即拒絕寫入 |
| 權限跑掉、WebDAV 同步失敗 | Agent 以 root 寫入，子目錄若無 setgid → 檔案群組變 root，www-data 只有 r-- | 寫入後 chmod 664 ＋ 群組同 Vault 根 ＋ 父目錄補 2775（setgid） |

## 硬規則

1. `type` 只能用白名單 10 種；未給時依第一層資料夾推斷（resources→resource、concepts→concept、reports→report、system→schema…），推不出就拒絕寫入。
2. `tags` 只能是核心 47 個（來源：[[system/frontmatter-rules]]）；非白名單 tag 直接拒寫。
3. 既有值一律保留——工具只補缺少的欄位，不覆蓋。`summary` 若是自動推導會提醒人工潤飾。
4. 目錄權限必須 **2775（含 setgid）**，不是 775；把 setgid 拔掉會讓問題每天復發（見 [[folder-rules]]）。

## 相關

- [[system/frontmatter-rules]]：欄位與 tag 白名單（單一來源）
- [[folder-rules]]：目錄結構與權限
