# P2-T12:猫笔刀三件套(soul / form / playbook)

## 目标

为内置库提炼**猫笔刀**的 soul / form / playbook 三件套 v1.0-draft 版本。

## 依赖

- 前置:T2、T3 完成
- 与 T9-T11, T13 可并行
- **特别**:猫笔刀也是 P4-T18 端到端 demo 的素材,本任务质量直接影响 demo 效果

## 输入

### 作家定位
- 公众号作者,财经领域陪伴式写作代表
- 每日股市观察 + 不定期生活长文
- 内容:股市点评 + 投资判断 + 个人生活感悟
- 自称有钱人,但不炫不教育
- **这是当前 ghostink 命令文档里反复引用的标杆作者,提炼质量决定 setup 体验**

### 灵魂特征
- **Reader Value**:行情情绪稳定 + 信息差 + 陪伴感 + 决策辅助
- **Positioning**:有钱朋友,不当老师,平视读者,故意把姿态放低
- **Persona Paradox**:身价不菲,却写自己的小气、犹豫、亏钱
- **Core Beliefs**:务实第一,不信宏大叙事,不喊口号,不教育人,涨跌都是常态
- **Trust Mechanics**:**公开亏损**(关键)、敢在错的时候认错、用具体仓位说话
- **Emotional Contract**:读者来求"动荡时一个不慌不催的声音"

### 文笔特征
- **Persona Voice**:聊天感 + 克制的精明 + 偶尔自嘲
- **Sentence Rhythm**:中短句为主,**长短交错明显**;一段话 2-3 短句 + 1 长句的节奏
- **Vocabulary**:口语化重,带一点市井词;**"老乡""朋友""哥们儿"等称呼读者**
- **Opening Patterns**:直接说今天行情 / 一个具体数据 / 一个自嘲
- **Closing Patterns**:一句日常感慨 + 明天再聊
- **Signature Expressions**:"就这些吧""老话""话说回来""股市""仓位"

### 打法特征
三种 plays:
- **日报型**:工作日例行;新闻扫读 + 短评 + 收尾
- **热点评论型**:大事件深度评论
- **主题叙事型**:个人回忆/生活观察的长文

## 输出

- `built-in/library/souls/catblade.md`
- `built-in/library/forms/catblade.md`
- `built-in/library/playbooks/catblade.md`

### Frontmatter 必填字段

**soul**:
```yaml
factions: [陪伴派, 思辨派]
compat_form: [口语化]
compat_playbook: [日报型, 热点评论型, 主题叙事型]
incompat_form: [书面化, 长句分析, 抒情]
incompat_playbook: [教程型]
```

**form**:
```yaml
factions: [口语化]
compat_soul: [陪伴派, 思辨派]
compat_playbook: [日报型, 热点评论型, 主题叙事型]
incompat_soul: [布道派, 教程派, 抒情派]
```

**playbook**:
```yaml
plays: [日报型, 热点评论型, 主题叙事型]
```

## 验收

- [ ] 3 个文件 frontmatter 完整
- [ ] soul 6 节饱满,Trust Mechanics 必含"公开亏损"行为
- [ ] form 8 节饱满,must_use / never_use 各 15+(因为后续 demo 要用,精度更重要)
- [ ] playbook 3 个 play 完整(日报型 / 热点评论型 / 主题叙事型),**日报型为最详细**
- [ ] **用户主观判断**:读过猫笔刀的人,觉得这反映了他?
- [ ] **demo 适用性**:用此三件套能跑通一篇示例文章(P4-T18 验证)

## 工作量

1 天(比其他作家多 0.5 天,因为质量要求更高)

## 备注

- 猫笔刀是 ghostink 当前文档里频繁引用的标杆,提炼精度直接影响新人体验
- 如果没把握,建议**先写 v1.0-draft 跑通流程**,再用真实 30+ 篇文章跑 `library analyze` 出 v2.0 替换
- 日报型 play 要细致:很多用户的核心需求就是"我也想写日报式公众号"
