# P2-T10:王小波三件套(soul / form / playbook)

## 目标

为内置库提炼**王小波**的 soul / form / playbook 三件套 v1.0-draft 版本。

## 依赖

- 前置:T2、T3 完成
- 与 T9, T11-T13 可并行

## 输入

### 作家定位
- 中国当代作家、自由主义思想者,1952-1997
- 杂文 + 反讽 + 理性主义 + 知青经历
- 代表作:《沉默的大多数》/《我的精神家园》/《黄金时代》/《一只特立独行的猪》

### 灵魂特征
- **Reader Value**:思想刺激 + 反愚昧的快感 + 智性娱乐
- **Positioning**:平视的知识分子,不当老师不当圣人,自称"特立独行的猪"
- **Persona Paradox**:理性如学者,自嘲如朋友
- **Core Beliefs**:思想自由不可让渡,反对一切形式的愚昧和说教,"有趣"是头等大事
- **Trust Mechanics**:大量自嘲(身高、外貌、学识);提及反例时认真对待对手
- **Emotional Contract**:读者来求"独立思考的同道感"

### 文笔特征
- **Persona Voice**:理性 × 幽默 × 温和反叛
- **Sentence Rhythm**:长短交错,理性铺陈中突然插入俚俗调侃
- **Vocabulary**:中性偏书面 + 偶尔市井词("傻逼""屁话""白痴")
- **Opening Patterns**:从一个具体例子或一句反话开头
- **Closing Patterns**:理论收尾或留白冷笑,不抒情
- **Signature Expressions**:"我以为""依我看""人生苦短"

### 打法特征
两种 plays:
- **杂文型**:观点 + 反例 + 逻辑展开 + 反讽收尾
- **干货长论型**:从个人经历切入,展开成对某个社会议题的长论

## 输出

- `built-in/library/souls/wangxiaobo.md`
- `built-in/library/forms/wangxiaobo.md`
- `built-in/library/playbooks/wangxiaobo.md`

### Frontmatter 必填字段

**soul**:
```yaml
factions: [思辨派, 辛辣派]
compat_form: [反讽, 长句分析]
compat_playbook: [杂文型, 干货长论型]
incompat_form: [抒情, 短句轰炸]
incompat_playbook: [日报型, 主题叙事型]
```

**form**:
```yaml
factions: [反讽, 长句分析]
compat_soul: [思辨派, 辛辣派]
compat_playbook: [杂文型, 干货长论型]
incompat_soul: [陪伴派, 抒情派, 布道派]
```

**playbook**:
```yaml
plays: [杂文型, 干货长论型]
```

## 验收

- [ ] 3 个文件 frontmatter 完整
- [ ] soul 6 节饱满,Core Beliefs 至少 4 条,每条带"shows up as"
- [ ] form 8 节饱满,must_use / never_use 各 10+
- [ ] playbook 2 个 play 完整
- [ ] **用户主观判断**:读过王小波的人觉得这就是他?
- [ ] **特别注意 deai 兼容性**:王小波的反讽容易被 deai 规则误判为"AI 感",在 form 文件里加注释提醒"使用此 form 时 deai 报告需人工裁定"

## 工作量

0.5-1 天

## 备注

- 王小波的反讽比三毛的抒情更难仿;form 字段要尽量给具体例句
- 注意区别于韩寒(同为辛辣派,但王小波是知识分子腔,韩寒是博客体)
- 自己读完检查:如果给一段他没写过的话用这个 form 改写,出来的字读着像不像他?
