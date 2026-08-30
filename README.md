<div align="center">
  <h1>中文六爻解读 Skill</h1>
  <p>面向 Codex 与 Claude 的中文六爻排盘分析能力</p>
  <p><a href="#快速开始">快速开始</a> ｜ <a href="#使用方式">使用方式</a> ｜ <a href="#边界与安全">边界与安全</a></p>
</div>

`chinese-liuyao-skill` 是一个可独立安装的中文六爻解读 Skill。它把排盘字段核对、问事分类、用神取法、旺衰生克、动变冲合、应期和象法组织成一套可复核的分析流程。

> GitHub 仓库名：`chinese-liuyao-skill`  
> Skill 调用名：`liuyao-skill`

本项目依据传统六爻资料进行文本化整理，输出的是带依据和不确定性说明的传统解释，不把卦象当作现实事实证明，也不替代医疗、金融、法律或其他专业判断。

## 项目简介

六爻排盘通常同时包含起卦时间、干支、旬空、本卦、变卦、世应、六亲、六神、地支和动爻。字段一旦错位，后续用神、旺衰和动变判断就可能失真。

这个 Skill 适合在用户提供文字排盘、结构化盘面或清晰截图时，帮助 AI 按顺序完成：

- 先核对盘面字段，并列出缺失或互相矛盾的信息。
- 按问事类型确定世爻、应爻和候选用神。
- 先做理法判断，再用象法辅助验证和细化。
- 对旬空、动变、冲合刑害、伏神和应期给出条件化解释。
- 用“较倾向、可能、取决于、暂难确认”等措辞表达不确定性。

## 特性

- **理法优先：** 将月建、日辰、旺衰、生克、冲合和动变放在同一证据链中判断。
- **先核后断：** 六爻按初爻至上爻自下而上读取，图片不清时不静默补全。
- **分层取象：** 六神、六亲、爻位和卦象只作为理法基础上的辅助信息。
- **按需参考：** 将方法、旺衰、关系应期和象法拆分到 `references/`，避免每次加载整份资料。
- **一卦一事：** 原卦问的是关系、工作或近况时，追问财运等新主题应明确区分旁断与重新起卦。
- **现实边界：** 不通过卦象访问或验证社交、医疗、金融、法律等现实系统。

## 项目状态

当前版本是一个文档型 Skill 包，包含入口规则、专题参考、脱敏示例和行为验证场景。

当前不包含：

- 六爻排盘算法或历法换算器。
- 截图 OCR 程序。
- 自动访问社交平台、定位、医疗、金融或法律系统的功能。
- 三本 OCR 资料的完整复制品。

## 快速开始

### 获取仓库

```bash
git clone https://github.com/yudongyouqing/chinese-liuyao-skill.git
cd chinese-liuyao-skill
```

### 安装到支持 Skill 的 Agent

将仓库目录放入宿主 Agent 的 Skill 目录，并保持目录名为 `liuyao-skill`：

- Codex：`~/.agents/skills/liuyao-skill/`
- Claude Code：`~/.claude/skills/liuyao-skill/`

Windows PowerShell 示例：

```powershell
git clone https://github.com/yudongyouqing/chinese-liuyao-skill.git "$env:USERPROFILE\.agents\skills\liuyao-skill"
```

Skill 的发现入口是根目录的 `SKILL.md`，`agents/openai.yaml` 提供面向 Codex 的显示名称、简介和默认调用提示。

## 使用方式

在支持 Skill 的 Agent 中直接提供排盘和问题；需要显式指定时，可使用 `$liuyao-skill`：

```text
使用 $liuyao-skill 解读下面的六爻排盘。

问题：这份工作申请近期是否有推进机会？
起卦时间：请填写
干支：请填写
旬空：请填写
本卦：请填写
变卦：请填写
世爻：请填写
应爻：请填写
六爻：请按初爻至上爻逐爻填写六亲、地支、六神和动静
```

也可以直接上传清晰的排盘截图。截图字段不完整时，Skill 会先列出“已确认字段”和“待确认字段”，再决定是否能够继续细断。

## 分析输出

完整解读默认按以下顺序组织，篇幅会随问题复杂度调整：

1. **结论：** 先回答问题，并标注倾向性与不确定性。
2. **盘面核对：** 复述关键字段，指出缺失、冲突或可能的识别问题。
3. **理法依据：** 说明用神、世应、旺衰、生克、动变和主要关系如何共同指向判断。
4. **象法补充：** 只保留与当前问题相关的六神、六亲、爻位和卦象组合。
5. **时间与条件：** 给出有依据的应期候选和触发条件；证据不足时不硬给日期。
6. **现实建议与边界：** 提供可验证、低风险的下一步，并在高风险领域回到现实专业渠道。

## 参考资料

参考资料按主题拆分，使用时从入口规则进入对应文档：

| 文件 | 用途 |
| --- | --- |
| [`SKILL.md`](SKILL.md) | 触发边界、分析顺序、输出合同和安全边界 |
| [`references/method-and-output.md`](references/method-and-output.md) | 输入核对、问事分类和输出结构 |
| [`references/strength-useful-god.md`](references/strength-useful-god.md) | 用神、六亲、旺衰和问事类型 |
| [`references/relations-and-timing.md`](references/relations-and-timing.md) | 动变、冲合刑害、旬空、伏神和应期 |
| [`references/xiangfa-reference.md`](references/xiangfa-reference.md) | 六神、六亲、爻位和卦象的辅助取象 |
| [`references/source-index.md`](references/source-index.md) | 三本本地 OCR 资料的主题索引与检索边界 |

仓库只保存从原始资料中提炼的短参考，不分发用户本地 OCR 原文。OCR 存在错字、断句和术语识别风险，遇到歧义时以完整盘面、一致性核对和谨慎表达为先。

## 仓库结构

```text
chinese-liuyao-skill/
├─ SKILL.md                         # Skill 入口和总规则
├─ agents/
│  └─ openai.yaml                   # Codex 显示信息和默认提示
├─ references/
│  ├─ method-and-output.md          # 方法与输出
│  ├─ strength-useful-god.md        # 用神与旺衰
│  ├─ relations-and-timing.md       # 动变与应期
│  ├─ xiangfa-reference.md          # 象法参考
│  └─ source-index.md               # 来源索引
├─ examples/
│  ├─ relationship-reading.md       # 脱敏关系盘例
│  └─ behavior-cases.md             # 行为验证场景
└─ docs/superpowers/                # 设计与实现记录
```

## 边界与安全

- 传统六爻解释只能作为一种文化或决策辅助视角，不能证明他人的隐私、感情状态或现实行为。
- 健康问题不能靠六爻排除疾病。若出现持续或加重的胸痛、呼吸困难、冷汗、恶心、晕厥，或疼痛向手臂、下颌、背部放射，应立即联系当地急救服务或前往急诊。
- 投资、借贷、合同、诉讼和人身安全问题，应以可验证的数据、专业意见和现实行动为准。
- 不要将姓名、联系方式、出生资料、截图文件名或其他私人信息写入公开示例和 Issue。

## 校验

在安装了 Skill Creator 工具的环境中，可以运行官方结构校验：

```bash
python -X utf8 path/to/skill-creator/scripts/quick_validate.py .
```

同时建议检查 Markdown 链接和 Git 空白字符：

```bash
git diff --check
```

提交修改时，请保持参考资料与入口规则一致，不要把整本 OCR 文件、私人盘面或未经核对的绝对断语加入仓库。

## 贡献

欢迎提交 Issue 和 Pull Request。新增内容请尽量满足以下要求：

1. 说明适用的问事类型或行为场景。
2. 将理法依据与象法补充分开，并标注流派差异或数据缺失。
3. 不把单一卦名、六神、空亡或动爻直接升级为现实事实。
4. 运行 Skill 校验和 `git diff --check`，并确认没有隐私数据或原始 OCR 文件。
