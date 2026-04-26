<!--
Playbook 模板 — 描述一个作者在不同情境下的固化打法集合(文章类型)。

设计哲学:
- 每个 Play 都是 Soul 的一个"应用场景模板",必须 trace 回 Soul 的某节
- 必须给 frequency(占比%),否则 distill 输出的 playbook 是"理想图"
  不是"实际打法"——读者看到的频率高的就是真打法
- 末尾的"总禁忌"和"写完校验"是跨所有 play 的护栏,写每篇都要过

plays 字段列出本 playbook 包含的所有 play 名称。
每个 play 名称必须从 built-in/library/factions.md 中的"Playbook 类型"词表选。

填写守则:
- 每个 Play 末尾的 ❌/✅ 是反例与正例对照
- 不确定就在条目末尾加 [LOW-CONFIDENCE]
- frequency 必须基于样本统计(数清楚每种文章的占比)
- Structure 步骤要给出"做什么"+"为什么这一步"
-->

---
type: playbook
name: <slug-英文小写>
display_name: <中文显示名>
source: ai-distilled
plays: []                    # 本 playbook 包含的 play 名称数组
description: |
  <1-2 句简介>
---

# Playbook: <作者名>

<!--
下面是 Play 模板示例。一个 playbook 通常含 2-5 个 plays。
按需复制 Play 段落并填写。
所有 Play 的 frequency 加起来应 ≈ 100%。
-->

## Play: <play 名,如"日报型">

- **frequency**:~<X>%(占该作者全部文章的比例)
- **trigger**:<什么时候用这个 play,如"工作日例行,当天有大盘行情">
- **serves_value**:[<对应 Soul 0.1 的价值类型,如:信息差, 情感陪伴>]
- **emotional_baseline**:<情感基调,如"日常陪伴,克制">
- **length**:<字数范围,如"800-1500">
- **material_from**:[<素材来源,如:当日新闻, 即时反应, 社群提问>]
- **publish_cadence**:<发布频率,如"每工作日 / 每周 N 篇 / 每月 1 次">

### Structure(段落级流程)

1. **<开头部分>** — 做什么 + 为什么(如:"直接砸数据 — 让读者 1 秒进入语境")
2. **<主体部分>** — <做什么 + 为什么>
3. **<...>** — <...>
4. **<收尾部分>** — <做什么 + 为什么>

### Core Characteristics(核心特征 — 4-6 条)

<!--
这是 Play 的"灵魂识别码"。光有 Structure 不够,因为不同作家可能
都用"开头-主体-收尾"三段式,真正的差异在这些特征里。

❌ "信息密度高、有观点、有态度"(任何 play 都能套)
✅ "每条新闻一小段,长短交错——重要的展开,不重要的两句带过"
✅ "允许穿插个人生活碎片(大崽 / 老人 / 邻居),增加人味"
✅ "篇末常以广告或闲话破节奏(像真的闲聊完了)"
-->

1. <特征 1>
2. <特征 2>
3. <特征 3>
4. <特征 4>
5. <特征 5(可选)>
6. <特征 6(可选)>

### Opening Patterns

适用于本 Play 的 Form Opening Pattern 子集:
[<如:数据砸脸开篇, 当日新闻开篇, 反问开篇>]

### Closing Patterns

适用于本 Play 的 Form Closing Pattern 子集:
[<如:省略式收尾, 广告破节奏, 明日预告>]

### Anti-Patterns(本 Play 的局部禁忌)

<!-- 本 Play **特别**禁忌的写法。
     比如日报型 Play 禁"长篇深度"(不符合发布节奏),
     深度感悟型 Play 禁"两句带过"(不够深) -->

- <禁忌 1>
- <禁忌 2>

[traces to: Soul 0.X(说明这个 Play 服务于 Soul 哪节的价值)]

---

## Play: <下一个 play 名>

- **frequency**:~<X>%
- **trigger**:
- **serves_value**:[]
- **emotional_baseline**:
- **length**:
- **material_from**:[]
- **publish_cadence**:

### Structure

1.
2.
3.

### Core Characteristics

1.
2.
3.
4.

### Opening Patterns
[]

### Closing Patterns
[]

### Anti-Patterns

-

[traces to: Soul 0.X]

---

## Cross-Play Prohibitions(总禁忌 — 所有 Play 共用)

<!--
跨所有 Play 都不能跨越的红线。这是 Soul 0.5 红线 + Soul 0.6 情感界限
在打法层面的具象化。

❌ "保持高质量"(空)
✅ "不贩卖焦虑(再不学就晚了 / 你会被淘汰)"
✅ "不吹捧技术(展示看起来很酷但没用的东西)"
✅ "不做保姆教程(一步步截图教你点哪)"
-->

1. **不<...>** — 因为违反 Soul 0.X(<具体说明>)
2. **不<...>** — 因为违反 Soul 0.X(<...>)
3. **不<...>** — 因为违反 Soul 0.X(<...>)
4. **不<...>** — 因为违反 Soul 0.X(<...>)
5. **不<...>** — 因为违反 Soul 0.X(<...>)

(至少 5 条,每条必须 trace 回 Soul)

---

## Self-Check Checklist(写完任意一篇文章后的自检)

<!--
这是写完一篇文章后,在发布前过一遍的清单。
每条都是 yes/no 问题,有任何 no 就不发。

设计原则:每条问题都要能"立刻判断真伪",不能太抽象。

❌ "是否符合作者风格?"(无法判断)
✅ "是否有自己的实践经历或作品作为支撑?"(可判断)
-->

- [ ] 是否有<具体支撑物>(如实践经历 / 数据 / 案例)?
- [ ] 是否在<做核心动作>(如:教思维而非步骤)?
- [ ] <某种语调>是否足够(如:口语化程度)?
- [ ] 是否避免了 Cross-Play Prohibitions 全部 N 条?
- [ ] 技术 / 专业概念是否给了"白话翻译"(if 适用)?
- [ ] 是否有明确的个人判断(不骑墙)?
- [ ] 整体读感是否像"<比喻>"(如:一个靠谱朋友在跟你聊他的发现)?
- [ ] 是否有"<升华动作>"(如:从具体到思维方式)?
- [ ] <情绪检查>是否在范围内(如:没有变焦虑 / 没有变说教)?

(至少 8 条,每条要能立刻 yes/no 判断)

---

<!--
完成检查表(distill 自查,不输出到最终文件):

[ ] 所有 Play 的 frequency 加起来 ≈ 100%?
[ ] 每个 Play 是否给出 ≥4 条 Core Characteristics?
[ ] 每个 Play 是否标注 [traces to: Soul 0.X]?
[ ] Cross-Play Prohibitions ≥5 条且每条 trace 回 Soul?
[ ] Self-Check Checklist ≥8 条且每条可立刻 yes/no?
[ ] 是否有 Play 看起来跟其他 Play 重复(应合并)?
[ ] 是否有 frequency < 5% 的 Play(可能不该独立成 Play)?

任何 [LOW-CONFIDENCE] 项必须在 distill 下一轮重点验证。
-->
