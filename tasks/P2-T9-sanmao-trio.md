# P2-T9:三毛三件套(soul / form / playbook)

## 目标

为内置库提炼**三毛**的 soul / form / playbook 三件套 v1.0-draft 版本,基于 AI 训练知识 + 抽样代表作。

## 依赖

- 前置:T2(目录结构)、T3(schema 模板)完成
- 与 T10-T13 可并行
- 参考:`templates/soul.template.md` / `form.template.md` / `playbook.template.md`

## 输入

### 作家定位
- 中国当代散文家,1943-1991
- 流浪叙事 + 异域生活 + 浓情抒写
- 代表作:《撒哈拉的故事》/《梦里花落知多少》/《雨季不再来》/《雨季不再来》/《温柔的夜》

### 灵魂特征(soul 提炼方向)
- **Reader Value**:情感陪伴 + 异域信息 + 身份认同(自由女性 / 漂泊者)
- **Positioning**:漂泊的女子,不是"流浪文学"标签,是"把孤独写成温度"的同行人
- **Persona Paradox**:浪迹天涯却写出最有归属感的文字
- **Core Beliefs**:自由高于稳定,真情高于体面,孤独是常态而非苦难
- **Trust Mechanics**:大量真实地名、人名、日期细节;主动暴露弱点(怕死、想家、嫉妒)
- **Emotional Contract**:读者来求"被理解的孤独"

### 文笔特征(form 提炼方向)
- **Persona Voice**:温柔但坚定;讲述时第一人称,常常停下来"对你说"
- **Sentence Rhythm**:长句多,情感堆叠;偶有突然的短句作情绪爆破
- **Vocabulary**:口语化 + 略带文气;**忌讳书面化连接词**(因此/此外/综上)
- **Opening Patterns**:常以一个具体场景或一句感慨开头
- **Closing Patterns**:留白式收尾,不总结
- **Signature Expressions**:"撒哈拉""那个时候""他啊""我啊"

### 打法特征(playbook 提炼方向)
两种 plays:
- **主题叙事型**:一段经历 + 情感弧线 + 哲理留白
- **杂文型**:即兴感悟,跳跃式联想

## 输出

3 个文件,版本 `1.0-draft`,源 `ai-distilled`:

- `built-in/library/souls/sanmao.md`
- `built-in/library/forms/sanmao.md`
- `built-in/library/playbooks/sanmao.md`

### Frontmatter 必填字段

**soul**:
```yaml
factions: [抒情派, 陪伴派]
compat_form: [抒情, 长句分析]
compat_playbook: [主题叙事型, 杂文型]
incompat_form: [短句轰炸, 反讽]
incompat_playbook: [日报型, 教程型]
```

**form**:
```yaml
factions: [抒情, 长句分析]
compat_soul: [抒情派, 陪伴派]
compat_playbook: [主题叙事型, 杂文型]
incompat_soul: [辛辣派, 教程派, 布道派]
```

**playbook**:
```yaml
plays: [主题叙事型, 杂文型]
```

## 验收

- [ ] 3 个文件已创建,frontmatter 字段完整
- [ ] soul 含完整 6 节(0.1-0.6),每节有具体内容不留空
- [ ] form 含 8 节,Vocabulary Fingerprint 至少 10 个 must_use 词 + 10 个 never_use 词
- [ ] playbook 含 2 个完整的 play(主题叙事型 / 杂文型)
- [ ] 所有内容用中文,frontmatter 字段名英文
- [ ] description 字段简洁不超 30 字
- [ ] 文件版本字段为 `1.0-draft`,source 为 `ai-distilled`
- [ ] **用户主观判断**:你读过三毛的人,觉得这三份文件能反映她吗?(过不去就重做)

## 工作量

0.5-1 天

## 备注

- v1 由 AI 直接产出,精度有限——v2 等用户准备 30+ 篇原文跑 `library analyze` 再覆盖
- 三毛的灵魂和形态高度耦合,form 单用要在 incompat 标签上严格(不要让陪伴 soul 误配)
- 写完后**自己读一遍**,如果你不觉得"对,这就是三毛",就退回重做
- 派系标签必须从 SKILL.md 词表选,不要发明新词(P3-T14 锁定词表)
