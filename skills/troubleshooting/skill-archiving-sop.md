---
title: Skill 歸檔封存標準作業程序 (SOP)
summary: Skill 歸檔封存標準作業程序 (SOP)：1. 備份階段 (存檔)
created: 2026-06-03
updated: 2026-06-03
type: task
tags: [maintenance, skill-management, hermes-agent]
---

# Skill 歸檔封存 SOP

本文件定義當某個 Skill 不再活躍使用時，應執行的封存作業流程。確保系統保持輕量化，同時保留該 Skill 的程式碼與歷史脈絡供未來查閱。

## 1. 備份階段 (存檔)
在刪除目錄前，必須先建立壓縮備份：

```bash
# 建立備份目標目錄
mkdir -p ~/.hermes/backups/skills/

# 打包 Skill 目錄 (將 `[skill-name]` 替換為實際名稱)
tar -czvf ~/.hermes/backups/skills/[skill-name]_$(date +%Y%m%d).tar.gz ~/.hermes/skills/[skill-name]
```

## 2. 移除階段 (系統清理)
使用 Hermes 內建工具進行正式移除，需註冊歸檔意圖：

```python
from hermes_tools import skill_manage

# 若 Skill 已釘選 (Pinned)，請先執行 `hermes curator unpin [skill-name]`
# 執行正式刪除
skill_manage(
    action='delete', 
    name='[skill-name]', 
    absorbed_into="" # 若無合併對象，填入空字串
)
```

## 3. 檢查階段 (清理相依 Cron)
在移除前，請務必先掃描是否有排程任務依賴此 Skill：

```python
from hermes_tools import cronjob
# 執行檢查
cronjob(action='list')
```

## 4. Wiki 沉澱 (結構維護)
根據「導航驅動記憶」規範，需在 Obsidian 完成以下變更：
- **`log.md`**：紀錄歸檔時間、原因、備份路徑 (e.g., `~/.hermes/backups/skills/...`)。
- **`index.md`**：移除該 Skill 的連結索引。
- **`GitHub`**：完成上述變更後，務必將更新同步推送到 `evanhsia-git/obsidian-vault`。

---
*註：本流程嚴格遵守「導航 → 執行 → 沉澱」規範。執行前請確認已取得 Ivan 的結構性變更審核許可。*

---
## 相關節點
- [[schema]]