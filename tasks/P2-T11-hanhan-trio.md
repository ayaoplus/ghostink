# P2-T11:韩寒三件套(soul / form / playbook)

## 目标

为内置库提炼**韩寒**的 soul / form / playbook 三件套 v1.0-draft 版本。

## 依赖

- 前置:T2、T3 完成
- 与 T9, T10, T12, T13 可并行

## 输入

### 作家定位
- 中国当代作家、博客时代代表人物,1982-
- 早期博客辛辣评论 + 80 后反权威 + 短句轰炸式文风
- 代表作:博客文集《杂的文》/《我所理解的生活》/早期长篇《三重门》/电影台词
- **本任务以"博客时代韩寒"为提炼对象**,不混入电影/小说期

### 灵魂特征
- **Reader Value**:解气 + 反权威认同 + 娱乐 + "他敢说"
- **Positioning**:不卑不亢的同龄人,不当导师不当意见领袖,但敢撕
- **Persona Paradox**:对体制反讽,对自己也反讽(不假装清醒)
- **Core Beliefs**:不信权威、不信宏大叙事、不信"集体"、相信常识
- **Trust Mechanics**:用具体事件说话,不空喊;输了愿意承认
- **Emotional Contract**:读者来求"敢说的同龄人替我说话"

### 文笔特征
- **Persona Voice**:玩世不恭 × 内里清醒
- **Sentence Rhythm**:短句多,节奏快;**短句堆叠后偶尔一长句作收**
- **Vocabulary**:口语化重,网络用语随意;**绝不用四字成语堆叠**
- **Opening Patterns**:从新闻事件 / 一句反讽 / 自嘲入手
- **Closing Patterns**:反讽收尾或荒诞类比
- **Signature Expressions**:"我说""你说""这个事情啊"

### 打法特征
两种 plays:
- **热点评论型**:针对一个事件,短段落 + 反讽 + 比喻
- **杂文型**:即兴跳跃,无固定结构

## 输出

- `built-in/library/souls/hanhan.md`
- `built-in/library/forms/hanhan.md`
- `built-in/library/playbooks/hanhan.md`

### Frontmatter 必填字段

**soul**:
```yaml
factions: [辛辣派, 思辨派]
compat_form: [短句轰炸, 反讽]
compat_playbook: [热点评论型, 杂文型]
incompat_form: [抒情, 长句分析]
incompat_playbook: [教程型, 主题叙事型]
```

**form**:
```yaml
factions: [短句轰炸, 反讽]
compat_soul: [辛辣派, 思辨派]
compat_playbook: [热点评论型, 杂文型]
incompat_soul: [抒情派, 陪伴派, 布道派]
```

**playbook**:
```yaml
plays: [热点评论型, 杂文型]
```

## 验收

- [ ] 3 个文件 frontmatter 完整
- [ ] soul 6 节饱满
- [ ] form 8 节饱满,Sentence Rhythm 量化:短句比例 ≥60%、最长句不超 N 字
- [ ] playbook 2 个 play
- [ ] **用户主观判断**:像博客时代的韩寒,不像晚期或电影台词
- [ ] form 中明确标注"博客时代",避免混入晚期作品风格

## 工作量

0.5-1 天

## 备注

- 韩寒和王小波都属辛辣派,但**辨识度差别要写出来**:
  - 王小波 = 学者腔反讽,长句分析为主
  - 韩寒 = 博客体反讽,短句堆叠为主
- form 应该提供"句长分布"具体数据(如"15 字以下句占 60%+")
- description 字段建议:"博客时代韩寒:短句轰炸 + 反讽 + 反权威"
