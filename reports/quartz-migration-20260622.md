---
title: "Quartz 遷移與 YAML 修復記錄"
description: "2026-06-22 Quartz content symlink → 實體資料夾遷移、YAML frontmatter 修復、GitHub Actions 部署排查"
summary: "Quartz 遷移過程中的問題、解決方案與備份記錄"
type: report
status: active
tags: [quartz, deploy, backup]
created: 2026-06-22
updated: 2026-06-22
---

# Quartz 遷移與 YAML 修復記錄

## 背景

- **Vault**: `/root/Documents/Obsidian Vault`（Obsidian 知識庫）
- **Quartz**: `/root/Documents/Quartz`（SSG 靜態網站生成器）
- **GitHub Repo**: `evanhsia-git/quartz`（分支 `v5`）
- **GitHub Pages**: 自動部署

### 原架構問題

```
Vault (實體) ──symlink──→ Quartz/content ──git──→ GitHub ──Actions──→ Pages
                         (git 不追蹤內建檔案)
```

- `Quartz/content` 是 symlink，指向 `/root/Documents/Obsidian Vault`
- Git 只記錄 symlink 本身，**不追蹤指向目錄的檔案**
- Vault 新增/修改筆記後，`git status` 無變化，GitHub Pages 不會更新

---

## 遷移計畫

### 目標架構

```
Obsidian Vault
↓ rsync
Quartz/content（實體資料夾）
↓ git push
GitHub Actions
↓
GitHub Pages
```

### 遷移步驟

1. 備份 symlink → `content_symlink_20260622`
2. 建立實體 `content/` 資料夾
3. rsync Vault → Quartz/content（排除 `.git/`, `.obsidian/`, `publish/`, `ivan-notes/`）
4. 驗證 Markdown 檔案數量一致
5. `git add content/` + commit + push

---

## 執行過程

### Phase 1: 遷移 content symlink → 實體資料夾

```bash
# 備份 symlink
mv /root/Documents/Quartz/content /root/Documents/Quartz/content_symlink_20260622

# 建立實體資料夾
mkdir /root/Documents/Quartz/content

# rsync 同步
rsync -av --exclude='.git/' --exclude='.obsidian/' \
      --exclude='publish/' --exclude='ivan-notes/' \
      /root/Documents/Obsidian\ Vault/ /root/Documents/Quartz/content/
```

**結果**：141 個 markdown 檔案同步成功。

### Phase 2: .gitignore 更新

為避免備份 symlink 被 git 追蹤，加入 `.gitignore`：

```
content_symlink_*
```

### Phase 3: 第一次 commit + push

```
commit 00f0dc8: migrate content from symlink to tracked directory
147 files changed, 10458 insertions(+), 1 deletion(-)
```

---

## 問題與修復

### Problem 1: Wikilink 在 frontmatter 內

**錯誤訊息**：
```
Failed to process markdown `content/concepts/financial-preferences.md`: 
end of the stream or a document separator is expected (6:1)

  6 | - [[openrouter-free-models]]
  -----^
```

**原因**：`- [[openrouter-free-models]]` 在 frontmatter 內無 key，且 `sp500-components.md`

**修復方式**：
1. 將 `- [[openrouter-free-models` 從 frontmatter 移至 body 的「相關頁面」區域
2. 重寫 `summary` 為語意描述（原 `summary: Financial-Preferences：相關頁面` 無意義）

**範例**（financial-preferences.md）：

修正前：
```yaml
title: Financial-Preferences
- [[openrouter-free-models]]
summary: Financial-Preferences：相關頁面
```

修正後：
```yaml
title: Financial-Preferences
summary: 用戶金融分析偏好與品質標準規範
```

body 新增：
```markdown
## 相關頁面
- [[openrouter-free-models]]
- [[concepts/stock-automation-config|股票自動化配置]]
```

**Commit**: `2619bd7`（1 file）、`1e894e2`（3 files）

---

### Problem 2: Summary 值含特殊字元未加 quote

**錯誤訊息**：
```
Failed to process markdown `content/queries/reports/2026-06-01-daily-task-summary.md`: 
bad indentation of a mapping entry (3:79)

  3 | ... ed `hermes-system-backup` skill: completed backup of Hermes con ...
  -----------------------------------------^
```

**原因**：`summary` 值包含 `：`（full-width colon）後接空格，被 YAML parser 解讀為 key-value separator。`` ` `` 和 `:` 也會導致解析失敗。

**影響檔案**：
- `queries/reports/2026-06-01-daily-task-summary.md`

**修復方式**：用 double quote 包裹 summary 值，並簡化內容。

修正前：
```yaml
summary: 2026-06-01 Daily Task Summary：- Executed `hermes-system-backup` skill: completed backup of Hermes configuratio
```

修正後：
```yaml
summary: "2026-06-01 Daily Task Summary - Executed hermes-system-backup skill, completed backup of Hermes configuration and reports to GitHub (12 files updated)"
```

**Commit**: `d13712f`

---

### Problem 3: raw/ 檔案缺少 summary

**檔案**：`raw/articles/karpathy-llm-wiki-gist.md`

**原因**：Layer 1 原始資料未強制要求 frontmatter，但 Quartz 的 `note-properties` plugin 會解析。

**修復**：補上 `summary` 欄位。

**Commit**: `eb0568f`

---

## 本地 Build 問題

### .quartz/ 目錄不存在

**錯誤**：
```
✘ [ERROR] Could not resolve "../../.quartz/plugins"
    quartz/components/Head.tsx:7:42
```

**原因**：`.quartz/` 是 Quartz 的 build cache/plugins 目錄，由 `npm run prebuild` 生成。Clone 後本地不存在。

**本地解法**：`npm run prebuild` → `npx quartz build`

**GitHub Actions**：workflow 已包含 `npx quartz plugin install` 步驟，會自動生成 `.quartz/`。

---

## 備份記錄

| 備份 | 路徑 | 說明 |
|------|------|------|
| Symlink 備份 | `/root/Documents/Quartz/content_symlink_20260622` | 原始 symlink，指向 Vault |
| Git 歷史 | `048d7b3` | 遷移前最後 commit |
| Git 歷史 | `00f0dc8` | 遷移完成 commit |

---

## 預防措施

### Frontmatter Safety Rules（已寫入 schema.md）

1. **Wikilink 禁止在 frontmatter 內**：所有 `wikilink` 必須放在 body
2. **Summary 含特殊字元必須 double quote**：`:`, `#`, `[`, `]`, `{`, `}`, `` ` ``, `|`, `>`, `!`, `%`, `@`, `&`, `*`
3. **Frontmatter 所有項目必須有 key**：不能無 key 直接放 list item

### 遷移前檢查清單

- [ ] `find Vault -name "*.md" | wc -l` 與 `find Quartz/content -name "*.md" | wc -l` 數量一致
- [ ] `git status` 只包含 `D content` + `A content/...`
- [ ] 不含 `node_modules`, `public`, `.cache`, `dist`
- [ ] `.gitignore` 已加入 `content_symlink_*`
- [ ] 推送前已知所有 YAML frontmatter 無 wikilink

---

## 正式流程圖

```
Obsidian Vault（知識庫）
      │
      │  rsync -a --delete
      │  排除：.git/ .obsidian/ .env *.db Environment Keys.md .trash/ store
      ▼
Quartz/content（實體資料夾）
      │
      │  git add -A → commit "Robust Sync: $DATE" → push origin v5
      ▼
GitHub Repo（evanhsia-git/quartz, branch v5）
      │
      │  GitHub Actions 自動觸發
      │  ├─ Checkout repository
      │  ├─ Setup Node.js 22
      │  ├─ npm ci（安裝依賴）
      │  ├─ npx quartz plugin install（生成 .quartz/ 快取）
      │  ├─ npx quartz build（生成 public/）
      │  └─ actions/deploy-pages@v4
      ▼
GitHub Pages（https://evanhsia-git.github.io/quartz/）
```

### 腳本位置

| 腳本 | 路徑 | 觸發方式 |
|------|------|----------|
| 編排入口 | `/root/.hermes/scripts/user-backup.sh` | cron daily 05:00 UTC+8 |
| Quartz 同步 | `/root/.hermes/scripts/robust_sync_to_quartz.sh` | user-backup.sh 階段二 |
| 安全掃描 | 內建在 robust_sync_to_quartz.sh | 每次 rsync 後 |
| 部署監控 | 內建在 robust_sync_to_quartz.sh | push 後 poll Actions |

### 遷移歷史

- **2026-06-18**: Quartz 初始化，content 為實體資料夾
- **2026-06-19**: 改為 symlink 架構（git 不追蹤 Vault 內容）
- **2026-06-22**: 遷回實體資料夾架構（rsync + git push）

## 相關頁面

- [[schema]]
- [[quartz-rules]]
- quartz-v5-deployment
- [[index|主索引]]

---

## 遷移時間線

| 時間 (UTC) | 事件 | Commit |
|------------|------|--------|
| 2026-06-22 04:14 | 遷移 content symlink → 實體資料夾 | `00f0dc8` |
| 2026-06-22 05:17 | 修復 financial-preferences.md wikilink | `2619bd7` |
| 2026-06-22 05:20 | 修復 3 個檔案 wikilink | `1e894e2` |
| 2026-06-22 06:15 | 修復 summary 特殊字元 + raw summary | `d13712f`, `eb0568f` |
