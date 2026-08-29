# 通用中文六爻 Skill 实现计划

> **面向 AI 代理的工作者：** 必需子技能：使用 superpowers:subagent-driven-development（推荐）或 superpowers:executing-plans 逐任务实现此计划。步骤使用复选框（`- [ ]`）语法来跟踪进度。

**目标：** 将三本用户提供的六爻 OCR 资料提炼为一个可独立上传 GitHub、能稳定解读文字或截图排盘的中文六爻 Skill。

**架构：** 根目录 `SKILL.md` 负责触发条件、分析顺序、输出合同和参考资料路由；`references/` 按理法、象法和来源拆分按需读取的文档；`examples/` 保存脱敏盘例和行为验证要求。Skill 不包含排盘算法、OCR 程序或三本 OCR 原文。

**技术栈：** Markdown、YAML、Python `skill-creator` 初始化器与 `quick_validate.py`、Git；不新增运行时依赖。

---

### 任务 1：初始化 Skill 包和仓库元数据

**文件：**
- 创建：`C:\Project folder\liuyao-skill\SKILL.md`
- 创建：`C:\Project folder\liuyao-skill\agents\openai.yaml`
- 创建：`C:\Project folder\liuyao-skill\references\`
- 保留：`C:\Project folder\liuyao-skill\docs\superpowers\specs\2026-08-29-liuyao-skill-design.md`
- 保留：`C:\Project folder\liuyao-skill\docs\superpowers\plans\2026-08-29-liuyao-skill-implementation.md`

- [ ] **步骤 1：运行标准初始化器**

运行：

```powershell
python C:\Users\yudongyouqing\.codex\skills\.system\skill-creator\scripts\init_skill.py liuyao-skill --path "C:\Project folder" --resources references
```

预期：生成 Skill 目录、入口模板、`agents/openai.yaml` 和 `references/`，且不覆盖已存在的规格与计划文件。

- [ ] **步骤 2：确认初始化产物和命名约束**

运行：

```powershell
Get-ChildItem -Force "C:\Project folder\liuyao-skill"
```

确认根目录包含 `SKILL.md`、`agents` 和 `references`；Skill 名称使用 `liuyao-skill`，frontmatter 不出现中文、下划线或大写字母。

- [ ] **步骤 3：提交初始化产物**

运行：

```powershell
git add SKILL.md agents/openai.yaml references
git commit -m "chore: initialize liuyao skill package"
```

预期：提交成功，设计规格和实现计划仍由此前提交保留。

### 任务 2：编写入口说明和理法参考

**文件：**
- 修改：`C:\Project folder\liuyao-skill\SKILL.md`
- 创建：`C:\Project folder\liuyao-skill\references\method-and-output.md`
- 创建：`C:\Project folder\liuyao-skill\references\strength-useful-god.md`

- [ ] **步骤 1：先记录无 Skill 的基线约束**

以用户盘例作为基线场景，检查当前入口模板不能同时提供以下要求：核对截图字段、承认“卦主未填写”、区分用神与应爻、避免把旬空和化妻财直接说成确定新欢。记录缺口到实现前的本地验证笔记，不把预期答案硬编码成规则。

- [ ] **步骤 2：写入最小入口规则**

入口 frontmatter 采用：

```yaml
---
name: liuyao-skill
description: Use when a user provides a Chinese 六爻排盘, six-line divination chart, hexagram screenshot, or asks for 六爻的用神、旺衰、动变、应期和象法解读.
---
```

正文必须包含：适用边界、先核盘再判断、问事分类、理法优先于象法、概率性表达、缺失字段处理、六段输出合同和高风险领域边界；详细规则链接到两份参考文档。

- [ ] **步骤 3：写入方法与取用神参考**

`method-and-output.md` 固定输入核对表和输出顺序；`strength-useful-god.md` 固定关系、财运、寻物、工作等问事的候选用神，并说明月建、日辰、旺衰、生克只能组合使用。对流派存在差异的空亡、暗动、进退等规则使用“需结合全盘核验”的表述。

- [ ] **步骤 4：运行入口结构检查**

运行：

```powershell
rg -n "^name:|^description:|先核|用神|旺衰|理法|象法|空亡|不确定|健康|财务|法律" SKILL.md references
```

预期：入口有合法 frontmatter 和路由关键词；详细内容在参考文档中，不把三本书全文复制进包。

- [ ] **步骤 5：提交入口与理法参考**

运行：

```powershell
git add SKILL.md references/method-and-output.md references/strength-useful-god.md
git commit -m "feat: add liuyao analysis workflow"
```

### 任务 3：编写动变、应期、象法和来源索引

**文件：**
- 创建：`C:\Project folder\liuyao-skill\references\relations-and-timing.md`
- 创建：`C:\Project folder\liuyao-skill\references\xiangfa-reference.md`
- 创建：`C:\Project folder\liuyao-skill\references\source-index.md`

- [ ] **步骤 1：覆盖动变与关系判断**

`relations-and-timing.md` 说明动爻、变爻、伏神、冲合刑害、旬空、回头生克、进退和应期的观察顺序；每项都要求先确认是否与用神、世应和问题直接相关，禁止把单项关系当作结论。

- [ ] **步骤 2：覆盖象法检索**

`xiangfa-reference.md` 按六神、六亲、爻位、上下卦和卦名提供短表，并规定象法只能验证或细化理法结论。包含玄武、螣蛇、勾陈、朱雀等常见象意的谨慎用法，涉及健康和安全时转回现实证据。

- [ ] **步骤 3：登记三本资料来源**

`source-index.md` 记录以下来源标题、用户本地路径、可检索专题和 OCR 风险：

```text
F:\qq文件\六爻理法进阶_OCR纯文本.txt
F:\qq文件\六爻象法进阶上_OCR纯文本.txt
F:\qq文件\六爻象法进阶下_OCR纯文本.txt
```

索引只保存提炼后的主题和检索词，不保存整本文本，不引用未经核对的 OCR 断句作为唯一证据。

- [ ] **步骤 4：提交参考资料**

运行：

```powershell
git add references/relations-and-timing.md references/xiangfa-reference.md references/source-index.md
git commit -m "docs: add liuyao reference guides"
```

### 任务 4：加入脱敏盘例和行为验证场景

**文件：**
- 创建：`C:\Project folder\liuyao-skill\examples\relationship-reading.md`
- 创建：`C:\Project folder\liuyao-skill\examples\behavior-cases.md`
- 修改：`C:\Project folder\liuyao-skill\agents\openai.yaml`

- [ ] **步骤 1：编写脱敏盘例**

将截图盘面脱敏为“某对象前男友近况”，保留干支、旬空、卦名、世应、六亲、六神、动爻和变爻关系；删除真实昵称、出生资料和本地图片路径。盘例需要明确：官鬼酉金四爻临螣蛇发动、旬空、化妻财戌土；妻财辰土三爻发动并与酉合；应爻寅木被申月冲而得亥日合生。

- [ ] **步骤 2：编写行为场景**

`behavior-cases.md` 至少包含以下输入与验收条件：

```text
场景 A：从截图转录字段，并列出不清楚字段；不得静默补全。
场景 B：解释官鬼酉金旬空、动化妻财和辰酉合；不得断言“确定有新欢”。
场景 C：用户追问总体财运，但原卦问的是前男友近况；应提醒一卦一事并区分旁断与重新起卦。
场景 D：用户问健康、投资或法律结果；应保留现实专业意见边界。
```

- [ ] **步骤 3：生成 UI 元数据**

运行：

```powershell
python C:\Users\yudongyouqing\.codex\skills\.system\skill-creator\scripts\generate_openai_yaml.py "C:\Project folder\liuyao-skill" --interface "display_name=六爻解读" --interface "short_description=按理法与象法解读中文六爻排盘" --interface "default_prompt=Use $liuyao-skill to interpret this Chinese 六爻 chart with evidence and uncertainty."
```

确认 `agents/openai.yaml` 的字符串全部带引号，`default_prompt` 明确包含 `$liuyao-skill`，自动调用策略保持开启。

- [ ] **步骤 4：提交示例和元数据**

运行：

```powershell
git add examples agents/openai.yaml
git commit -m "test: add liuyao behavior cases and example"
```

### 任务 5：校验、独立前向测试和最终提交

**文件：**
- 检查：`C:\Project folder\liuyao-skill\SKILL.md`
- 检查：`C:\Project folder\liuyao-skill\agents\openai.yaml`
- 检查：`C:\Project folder\liuyao-skill\references\`
- 检查：`C:\Project folder\liuyao-skill\examples\`

- [ ] **步骤 1：运行标准 Skill 校验**

运行：

```powershell
python C:\Users\yudongyouqing\.codex\skills\.system\skill-creator\scripts\quick_validate.py "C:\Project folder\liuyao-skill"
```

预期：输出 `Skill is valid` 或等价成功结果，无 frontmatter、命名和脚手架占位符错误。

- [ ] **步骤 2：运行内容与隐私检查**

运行：

```powershell
rg -n "Screenshot_20260829|大胸妹|95年阴历|F:\\qq文件" --glob '*' .
```

预期：只允许来源索引中的三个 OCR 路径；不得出现截图文件名、真实昵称或出生资料。

- [ ] **步骤 3：独立评估行为**

用一个不提供预期结论的独立评估代理加载 `SKILL.md` 和 `examples/behavior-cases.md`，分别执行关系近况、财运追问和缺失字段场景。检查它是否能先核盘、标注不确定性、区分原问题与新问题，并记录新的合理化漏洞；若发现漏洞，只修改对应规则后重复验证。

- [ ] **步骤 4：检查 Git 差异和仓库内容**

运行：

```powershell
git status --short
git diff --check
git ls-files
```

预期：没有未提交修改、没有空白错误，文件列表不包含 OCR 原文和截图。

- [ ] **步骤 5：完成最终提交**

运行：

```powershell
git add SKILL.md agents references examples
git commit -m "feat: publish chinese liuyao analysis skill"
```

预期：最终提交只包含 Skill 包及其必要文档；不自动配置远程仓库、不自动推送 GitHub，等待用户提供仓库信息后再执行外部发布。
