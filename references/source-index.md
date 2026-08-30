# 本地来源索引

本索引只登记用户本地资料的位置、主题和检索词，帮助在需要时回查原始材料。三本 OCR 文件不随 Skill 上传，也不假定它们存在于 GitHub 包或当前仓库中。

## 来源登记

| 资料 | 本地路径 | 主题与检索词 | 使用边界 |
| --- | --- | --- | --- |
| 六爻理法进阶 | `F:\qq文件\六爻理法进阶_OCR纯文本.txt` | 主题：月建冲爻、日辰冲爻、日月作用、合的层次、应爻。检索词：`月建冲爻`、`日辰冲爻`、`日月作用`、`合的层次`、`应爻` | 用于核对理法术语和关系层次；仍需结合完整盘面、所选流派和问题相关性，不用单段 OCR 代替推理 |
| 六爻象法进阶上 | `F:\qq文件\六爻象法进阶上_OCR纯文本.txt` | 主题：六神、综合取象、长生十二宫。检索词：`六神`、`综合取象`、`长生十二宫` | 用于补充象法候选；先理法后取象，不把某个六神、长生状态或一段象意直接当作事实 |
| 六爻象法进阶下 | `F:\qq文件\六爻象法进阶下_OCR纯文本.txt` | 主题：三刑、伏神、旬空、反吟、进退、隔山化爻、六害、八宫卦象。检索词：`三刑`、`伏神`、`旬空`、`反吟`、`进退`、`隔山化爻`、`六害`、`八宫卦象` | 用于查找专项关系和象法线索；逐爻核对本变、地支、动静和用神相关性，不把复杂术语当成单项吉凶开关 |

## OCR 使用警告

OCR 可能有错字、断句问题，也可能误识干支、六亲、符号或专门术语。未经原图、用户转录或盘面一致性核对的 OCR 文字，不能作为唯一证据；遇到歧义时应记录“待确认”，并以可核对的排盘字段和统一的默认口径为先。

仓库只保存从资料中提炼的摘要和检索词，不保存整本 OCR，也不把用户本地文件复制进 Skill。不复制整段 OCR 原文，只保留必要的脱敏短摘要或用自己的话说明；姓名、账号、联系方式等个人信息必须先脱敏，不能进入索引、示例或冲突记录。

## Windows 检索示例

以下 PowerShell 片段可直接复制执行。每本 OCR 都先通过同一个存在性、只读严格 UTF-8 解码门槛，只有成功后才运行对应的 `rg`；命中行不会直接打印，避免把未经脱敏的 OCR 内容复制出来：

```powershell
function Invoke-CheckedOcrSearch {
    param(
        [Parameter(Mandatory)]
        [string]$Path,
        [Parameter(Mandatory)]
        [string]$Pattern
    )

    if (-not (Test-Path -LiteralPath $Path -PathType Leaf)) {
        Write-Output '本地文件不存在，无法回查'
        return
    }

    try {
        $utf8Strict = [System.Text.UTF8Encoding]::new($false, $true)
        # ReadAllText 只读文件；严格 UTF-8 解码失败会进入 catch。
        $null = [System.IO.File]::ReadAllText($Path, $utf8Strict)
    } catch {
        Write-Output '本地文件无权限、不可读或无法按严格 UTF-8 读取，无法回查'
        return
    }

    Write-Output '文件存在且已按严格 UTF-8 读取；现在才运行 rg'
    $hits = @(rg -n -S $Pattern -- $Path)
    if ($LASTEXITCODE -eq 0) {
        Write-Output ("检索命中 {0} 行；记录命中内容前先脱敏，只保留必要短摘要，不复制整段 OCR 原文。" -f $hits.Count)
    } elseif ($LASTEXITCODE -eq 1) {
        Write-Output '未找到匹配内容'
    } else {
        Write-Output '检索失败，无法回查'
    }
}

$ocrRoot = 'F:\qq文件'
Invoke-CheckedOcrSearch -Path (Join-Path $ocrRoot '六爻理法进阶_OCR纯文本.txt') -Pattern '月建冲爻|日辰冲爻|日月作用|合的层次|应爻'
Invoke-CheckedOcrSearch -Path (Join-Path $ocrRoot '六爻象法进阶上_OCR纯文本.txt') -Pattern '六神|综合取象|长生十二宫'
Invoke-CheckedOcrSearch -Path (Join-Path $ocrRoot '六爻象法进阶下_OCR纯文本.txt') -Pattern '三刑|伏神|旬空|反吟|进退|隔山化爻|六害|八宫卦象'
```

## 本地资料与脱敏门槛

每次回查前都按同一顺序完成文件存在性、可读性、UTF-8 解码和脱敏检查。上面的统一函数已对三本 OCR 分别执行这些检查；只有 `Test-Path` 确认文件存在且 `ReadAllText` 成功按严格 UTF-8 读取后，才允许运行对应的 `rg`。检查失败时函数立即返回，不运行后续检索。

如果文件不存在、路径不是文件、无权限、读取失败或无法按 UTF-8 读取，必须如实标记“无法回查”，不得声称“已读取”或“已检索到”。检查失败时不运行后续检索，也不凭检索词补造原文、盘面字段或传统依据。

记录任何命中内容前先脱敏，删除或替换姓名、账号、联系方式等个人信息；只保留与当前问题有关的必要短摘要，不复制整段 OCR 原文。短摘要仍要标明它来自本地资料、可能存在 OCR 错字或断句问题，并与已核对的盘面事实分开。

### 不可冒称已读取

只有存在性和可读性检查都成功，且实际完成了对应检索，才能描述“已回查”或“检索命中”。否则统一说明“本地文件不存在、无权限或无法按 UTF-8 读取，无法回查”（按实际情况选择具体原因）；不得用模糊措辞让用户误以为已经读取本地 OCR。

若本机路径不可用，应说明资料未能回查，不要把缺失文件默认为仓库缺陷，也不要凭 OCR 关键词补造盘面事实。
