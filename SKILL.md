---
name: liuyao-skill
description: Use when a user provides a Chinese 六爻排盘, six-line divination chart, hexagram screenshot, or asks for 六爻的用神、旺衰、动变、应期和象法解读.
---

# 中文六爻解读

## 适用边界

- 适用于用户提供文字排盘、结构化六爻盘或排盘截图，并希望理解用神、旺衰、生克、动变、应期或象法的情况。
- 不负责排盘算法、历法换算、截图 OCR 或通过卦象访问和验证社交、医疗、金融、法律等现实系统；不把传统解释当作事实证明。
- 若胸痛持续/加重，或伴呼吸困难、冷汗、恶心、晕厥，或向手臂/下颌/背部放射，立即联系当地急救服务或前往急诊；六爻不能排除就医。

## 执行规则

1. **先核对，再判断。** 先记录占问原文和盘面字段；六爻按初爻至上爻自下而上读取。图片不清时分列“已确认字段”和“待确认字段”，不猜不清的字符。
2. 先确定问事类型、世爻、应爻和候选用神，再进行理法判断。缺失字段、多用神、流派差异或本卦/变卦与动爻矛盾时，必须显式标注并请求确认或给出条件性分支。
3. 理法先于象法。象法只能验证或细化理法已经指向的范围，不能凭卦名、六神、空亡或单一动爻替代理法，更不能直接说成现实事实。
4. 使用“较倾向、可能、取决于、暂难确认”等有边界的措辞，不作玄学绝对断语，也不引入脚本、算法或未经核对的计算。

## 参考资料路由

- 任何完整解读：先读 [method-and-output.md](references/method-and-output.md)。
- 取用神、六亲和旺衰：读 [strength-useful-god.md](references/strength-useful-god.md)。
- 动变、冲合刑害与应期：读 [relations-and-timing.md](references/relations-and-timing.md)。
- 六神、六亲、爻位和卦象的辅助取象：读 [xiangfa-reference.md](references/xiangfa-reference.md)。
- 资料来源、检索词和 OCR 使用边界：读 [source-index.md](references/source-index.md)。
- 当精炼参考不足、用户要求按原始资料核对或出现术语歧义时，先读 `source-index.md`，再按关键词从 `sources/` 选择性检索对应 OCR；不要默认一次性加载三本全文。

## 输出合同

按需要简繁，但保留以下顺序：结论；盘面核对；理法依据；象法补充；时间/条件；现实建议与边界。健康、财务、法律或人身安全问题必须保留现实专业判断，并建议以可验证数据和合适的专业渠道为准。
