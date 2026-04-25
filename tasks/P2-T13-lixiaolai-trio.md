# P2-T13:李笑来三件套(soul / form / playbook)

## 目标

为内置库提炼**李笑来**的 soul / form / playbook 三件套 v1.0-draft 版本。

## 依赖

- 前置:T2、T3 完成
- 与 T9-T12 可并行

## 输入

### 作家定位
- 中国知识布道者、原新东方教师、币圈早期人物
- 写作以"重新定义概念"为核心方法
- 代表作:《把时间当作朋友》/《七年就是一辈子》/《通往财富自由之路》/《新生:七年就是一辈子》
- 公众号 / 得到专栏 / 知识星球三个主要发文渠道

### 灵魂特征
- **Reader Value**:框架认知 + 概念重定义 + 自我提升的方法论
- **Positioning**:布道者 + 引路人,带略微高位但极力把"知识"民主化
- **Persona Paradox**:绝对自信但反复强调"我也曾经错过"
- **Core Beliefs**:概念决定行为、复利是真理、不要"小聪明"、长期主义
- **Trust Mechanics**:大量给出自己实操结果(投资数字、身价、错误)
- **Emotional Contract**:读者来求"被升级的世界观"

### 文笔特征
- **Persona Voice**:布道感 + 自我重复(关键概念反复强调)+ 平易近人
- **Sentence Rhythm**:**短句多,强调式短句穿插**;每个观点用 1-2 句陈述 + 多个例子展开
- **Vocabulary**:中性偏书面但有意识口语化;**爱用斜体/加粗强调关键词**
- **Opening Patterns**:抛一个问题 / 一个反常识断言 / 重定义一个词
- **Closing Patterns**:回到核心概念 + 一句行动指令
- **Signature Expressions**:"重新定义""你看""所谓""我们""注意"
- **结构标记**:大量用 "事实上"、"即便如此"、"再换个角度"等过渡

### 打法特征
三种 plays:
- **教程型**:一步步教会读者一个方法/思维框架
- **干货长论型**:围绕一个核心概念,层层展开,最后落到行动
- **杂文型**:对一个话题的评论,但仍带"重定义"色彩

## 输出

- `built-in/library/souls/lixiaolai.md`
- `built-in/library/forms/lixiaolai.md`
- `built-in/library/playbooks/lixiaolai.md`

### Frontmatter 必填字段

**soul**:
```yaml
factions: [布道派, 教程派]
compat_form: [短句轰炸, 书面化]
compat_playbook: [教程型, 干货长论型, 杂文型]
incompat_form: [抒情, 反讽]
incompat_playbook: [日报型, 主题叙事型]
```

**form**:
```yaml
factions: [短句轰炸, 书面化]
compat_soul: [布道派, 教程派]
compat_playbook: [教程型, 干货长论型]
incompat_soul: [抒情派, 陪伴派, 辛辣派]
```

**playbook**:
```yaml
plays: [教程型, 干货长论型, 杂文型]
```

## 验收

- [ ] 3 个文件 frontmatter 完整
- [ ] soul 6 节饱满,Core Beliefs 至少 5 条
- [ ] form 8 节饱满,**Signature Expressions 要含"重新定义"模式的用法说明**
- [ ] playbook 3 个 play 完整,**教程型为最详细**
- [ ] **用户主观判断**:读过李笑来的人觉得这是他?
- [ ] form 文件里明确"强调式短句"的使用规则(如关键概念加粗 / 单句成段)

## 工作量

0.5-1 天

## 备注

- 李笑来的辨识度集中在"重新定义"这个动作上,form 必须捕捉到
- 注意区别于其他布道型:他不是鸡汤,是"概念锤"——用一个词反复砸
- 如果你对李笑来有立场分歧,提炼时仍要客观——目标是给"想学他文笔"的用户能用,不评判其人
