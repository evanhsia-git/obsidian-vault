---
title: 程式碼品質分析
description: 程式碼品質分析 — 找出效能瓶頸、重複程式碼、過長函式、不必要檔案存取，並提出優化方案
summary: "程式碼品質分析 skill — 找出效能瓶頸、重複程式碼、過長函式、不必要檔案存取，並提出優化方案"
created: 2026-06-24
updated: 2026-06-24
type: index
status: active
tags: [code-quality, analysis, optimization, refactoring, skill]
---

# 程式碼品質分析

> 本 skill 提供系統化的程式碼品質分析流程，找出效能瓶頸、重複程式碼、過長函式、不必要的檔案存取，並提出具體優化方案。

---

## 功能總覽

| 功能 | 說明 |
|------|------|
| 找出效能瓶頸 | 分析時間複雜度、不必要的迴圈、重複計算 |
| 找出重複程式碼 | 偵測 copy-paste 程式碼、相似函式 |
| 找出過長函式 | 超過門檻的函式、巢狀過深 |
| 找出不必要檔案存取 | 重複讀取、未快取的 I/O、可合併的讀寫 |
| 提出優化方案 | 針對每個問題提供具體改善建議 |

---

## 分析流程

### Step 1：掃描目標

```bash
# 掃描整個專案
find . -name "*.py" -o -name "*.js" -o -name "*.ts" | head -50

# 掃描特定目錄
find ./src -type f -name "*.py"
```

### Step 2：執行分析

使用 `execute_code` 執行 Python 分析腳本，檢查以下項目：

#### 2a. 過長函式

```python
import ast, pathlib

def find_long_functions(path, threshold=50):
    """找出超過 threshold 行的函式"""
    results = []
    for f in pathlib.Path(path).rglob("*.py"):
        try:
            tree = ast.parse(f.read_text())
            for node in ast.walk(tree):
                if isinstance(node, (ast.FunctionDef, ast.AsyncFunctionDef)):
                    lines = node.end_lineno - node.lineno + 1
                    if lines > threshold:
                        results.append({
                            "file": str(f),
                            "function": node.name,
                            "lines": lines,
                            "start": node.lineno
                        })
        except SyntaxError:
            pass
    return results
```

#### 2b. 重複程式碼

```python
import ast, hashlib
from collections import defaultdict

def find_duplicate_blocks(path, min_lines=6):
    """找出重複的程式碼區塊"""
    blocks = defaultdict(list)
    for f in pathlib.Path(path).rglob("*.py"):
        try:
            lines = f.read_text().splitlines()
            for i in range(len(lines) - min_lines):
                block = "\n".join(lines[i:i+min_lines]).strip()
                if block and not block.startswith("#"):
                    h = hashlib.md5(block.encode()).hexdigest()
                    blocks[h].append((str(f), i+1))
        except Exception:
            pass
    return {h: locs for h, locs in blocks.items() if len(locs) > 1}
```

#### 2c. 不必要檔案存取

```python
import ast, pathlib

def find_redundant_io(path):
    """找出重複的檔案讀取"""
    results = []
    for f in pathlib.Path(path).rglob("*.py"):
        try:
            tree = ast.parse(f.read_text())
            file_reads = []
            for node in ast.walk(tree):
                if isinstance(node, ast.Call):
                    if hasattr(node.func, 'attr') and node.func.attr in ('read', 'read_text', 'readlines', 'open'):
                        file_reads.append((node.lineno, ast.dump(node)))
            # 檢查同一檔案是否被多次讀取
            if len(file_reads) > 3:
                results.append({
                    "file": str(f),
                    "read_count": len(file_reads),
                    "lines": [r[0] for r in file_reads]
                })
        except SyntaxError:
            pass
    return results
```

#### 2d. 效能瓶頸

```python
import ast, pathlib

def find_performance_issues(path):
    """找出常見效能問題"""
    issues = []
    for f in pathlib.Path(path).rglob("*.py"):
        try:
            source = f.read_text()
            lines = source.splitlines()
            for i, line in enumerate(lines, 1):
                # 迴圈中的字串拼接
                if 'for ' in line and '+=' in line and ('"' in line or "'" in line):
                    issues.append({"file": str(f), "line": i, "issue": "迴圈中字串拼接，建議改用 join"})
                # 重複計算
                if 'len(' in line and line.count('len(') > 1:
                    issues.append({"file": str(f), "line": i, "issue": "重複計算 len()，建議快取"})
                # 巢狀迴圈
                if line.strip().startswith('for ') and i > 1:
                    prev = lines[i-2].strip() if i > 1 else ""
                    if prev.startswith('for '):
                        issues.append({"file": str(f), "line": i, "issue": "巢狀迴圈，時間複雜度 O(n²)"})
        except Exception:
            pass
    return issues
```

### Step 3：產出報告

分析完成後，產出結構化報告：

```
[程式碼品質分析報告]

📊 掃描範圍：src/（42 個檔案）

🔴 過長函式（>50 行）：3 個
   - process_data() in utils.py:120（87 行）→ 建議拆成 3 個子函式
   - handle_request() in api.py:45（62 行）→ 建議提取驗證邏輯
   - generate_report() in report.py:200（71 行）→ 建議使用模板方法

🟡 重複程式碼：2 組
   - utils.py:30-45 與 helpers.py:12-27（相似度 95%）→ 建議提取共用函式
   - api.py:80-95 與 api.py:150-165（相似度 88%）→ 建議統一處理邏輯

🟠 不必要檔案存取：1 處
   - config.py：同一個 config.json 被讀取 5 次 → 建議讀取一次後快取

🔵 效能瓶頸：4 處
   - data.py:34：迴圈中字串拼接 → 改用 join()
   - data.py:56：重複計算 len() → 快取結果
   - processor.py:78：巢狀迴圈 O(n²) → 考慮使用 dict 優化
   - cache.py:23：每次呼叫都重新讀取檔案 → 加入記憶體快取

📋 優化優先級
   P0：效能瓶頸（影響最大）
   P1：過長函式（影響可維護性）
   P2：重複程式碼（影響一致性）
   P3：不必要檔案存取（輕微影響）
```

---

## 使用方式

### 基本用法

```
分析 src/ 目錄的程式碼品質
```

### 指定門檻

```
分析 src/ 目錄，函式長度門檻設為 30 行
```

### 指定檔案類型

```
分析所有 .py 檔案的效能問題
```

---

## 分析項目詳細說明

### 過長函式

| 門檻 | 說明 |
|------|------|
| > 30 行 | 需要關注 |
| > 50 行 | 建議拆分 |
| > 100 行 | 必須拆分 |

**常見拆分策略**：
- 提取驗證邏輯
- 提取資料轉換
- 提取副作用（I/O、API 呼叫）
- 使用策略模式取代 switch/case

### 重複程式碼

| 相似度 | 處理方式 |
|--------|----------|
| > 90% | 直接提取共用函式 |
| 70-90% | 提取共用部分，保留差異 |
| < 70% | 可能是巧合，人工判斷 |

### 效能瓶頸

**常見模式**：
- 迴圈中的字串拼接 → 改用 `join()`
- 重複計算 → 快取結果
- 巢狀迴圈 → 使用 dict/set 優化
- 未使用生成器 → 改用 `yield`
- 不必要的 list → 改用 generator expression

### 檔案存取

**常見問題**：
- 同一檔案重複讀取 → 讀取一次後快取
- 可合併的讀寫 → 批次處理
- 未使用緩衝 → 加入 `buffering` 參數
- 路徑拼接未用 `pathlib` → 改用 `Path` 物件

---

## 注意事項

1. **分析前先備份**：確保程式碼已 commit
2. **門檻可調整**：根據專案規模調整行數門檻
3. **人工判斷**：分析結果僅供參考，最終決策由開發者判斷
4. **漸進改善**：不要一次修改所有問題，按優先級逐步改善

---
## 相關頁面

- [[superpowers-reference|Superpowers 功能說明]]
- [[skills/skills-index|Skills 目錄索引]]
- [[superpowers-install|Superpowers 安裝說明]]
- [[schema|SCHEMA]] — 核心憲法
- [[policy|POLICY]] — 規則路由器
