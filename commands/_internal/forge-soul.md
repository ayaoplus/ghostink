# _internal/forge-soul

通过对话生成用户的 `soul.md`。被 `commands/setup.md` Step 4 的"对话式 forge"路径调用(分支 C/D)。

不是用户直接调用的命令。

## 输入

- `chosen_author`(可空。分支 D "纯对话生成" 时为空)
- `profile/identity.md`、`profile/experiences.md`、`profile/opinions.md`(setup Step 1+3 已生成)
- `built-in/library/factions.md`(派系标签词表,约束 factions 字段)
- 若 `chosen_author` 不空:`built-in/library/souls/<chosen_author>.md`(用作"对照菜单",不直接复制)

## 输出

`[studio]/soul.md`,严格按 `templates/soul.template.md` schema:

- frontmatter:type=soul / name(用户起的 slug)/ display_name(用户起的中文名)/ source=ai-distilled / factions / compat_* / incompat_* / description
- 6 节正文(0.1-0.6)

## 流程(6 节,每节 1-2 个 AskUserQuestion)

每节问完后立刻摘要给用户看,确认后写入对应节,再进下一节。

### 节 0.1 Reader Value

> 第一个问题:你想给读者提供什么不可替代的价值?
> 不要说"好内容"——具体到这一个你独有的东西。
>
> 候选(可参考但不限于):
> - A) 信息差(你掌握的知识/视角读者拿不到)
> - B) 情感陪伴(读者感到被理解,不孤独)
> - C) 决策支持(帮读者做现实选择)
> - D) 娱乐(纯粹好玩)
> - E) 身份认同("终于有人替我说出来了")
> - F) 自由输入

(若 chosen_author 不空,显示该作者的 0.1 作为对照,但提醒"你不一定要和他一样")

追问:取关测试 — "如果你停更,读者最会失去什么?一句话。"

### 节 0.2 Positioning

> 你跟读者的关系最接近哪种?
> - A) 朋友(平视、有时放低姿态)
> - B) 老师(传道授业,姿态略高)
> - C) 同道中人(并肩,共同探索)
> - D) 挑衅者(故意刺激读者)
> - E) 策展人(精选信息,不太输出原创判断)
> - F) 其他(自由输入)

追问:人设悖论 — "你身上有什么矛盾的特质,反而让人觉得有意思?(如'技术专家但讲话很街头')"

### 节 0.3 Narrative Engine

> 你说服读者主要靠什么?(从下面排序最重要的 3 个)
> - A) 亲身经历("我亲眼见过/做过")
> - B) 数据和具体数字
> - C) 情感共振("你也这样吧")
> - D) 权威经验("我做这行 N 年")
> - E) 逻辑推理(从 A 推到 B)

追问:错的时候怎么处理 — "如果你说的预测/判断错了,你怎么处理?"

### 节 0.4 Core Beliefs

加载 `profile/opinions.md`,把里面的观点 + 标记 differentiator 的提取出来作为初始信念候选。

> 我看你 profile 里有这些观点。哪些是你跨多篇文章会反复出现的核心信念?(选 3-5 个)
> - <opinion 1>
> - <opinion 2>
> - ...
> - F) 我想新加一条(自由输入)

每条信念追问:"shows up as...?"(它在你写作中怎么体现?)

### 节 0.5 Trust Mechanics

加载 `profile/experiences.md`,看是否有"暴露弱点"类条目。

> 你怎么让读者相信你?
> 候选行为:
> - A) 公开自己的失败(亏过的钱、做错的判断)
> - B) 主动认错并复盘
> - C) 给具体数字而非"很多""不少"
> - D) 在不确定的时候明确说"我不确定"
> - E) 自由输入

追问:破信任的红线 — "什么会让读者瞬间不再相信你?"

### 节 0.6 Emotional Contract

> 读者在什么状态下打开你的文章?他们想得到什么?
> - A) 焦虑/动荡时,想获得"不慌"的稳定感
> - B) 困惑时,想获得"清晰"
> - C) 无聊时,想娱乐
> - D) 愤怒时,想发泄(同温层)
> - E) 自由输入

追问:绝不做的事 — "压力下你也不会跨越的情感界限是什么?"

---

## 派系标签自动推断

完成 6 节后,基于内容自动推断 frontmatter 标签:

- **factions**(从 `factions.md` 的 Soul 派系选 1-3 个):
  - 0.2 是"朋友" + 0.6 "稳定感" → 陪伴派
  - 0.3 排序"逻辑推理 + 数据" → 思辨派
  - 0.5 含"教读者方法/框架" → 教程派
  - 0.6 "情感共振"为主 → 抒情派
  - 0.2 是"挑衅者" → 辛辣派
  - 0.4 含"我希望读者升级世界观" → 布道派

- **compat_form / compat_playbook**:基于 factions 推
- **incompat_form / incompat_playbook**:基于 factions 反推

显示给用户看,允许调整:

```
我推断的派系标签:

  factions:        [陪伴派, 思辨派]
  compat_form:     [口语化]
  compat_playbook: [日报型, 主题叙事型]

合理吗?要调整哪项?
```

---

## description 生成

最后基于 6 节内容生成 description(30 字内):

> 一个 [角色] 的 [价值类型] 写作,关键特征是 [独特点]

显示给用户调整。

## 写入

按 schema 写到 `[studio]/soul.md`。

显示完整内容给用户最后确认:"这就是你的 soul。Step 4 完成,进 Step 5 选 form。"

## 实现备注

- 全程对话不要走得太快——每节都让用户停下来想
- 派系标签是"软约束"——用户可以违反推断,只要他坚持
- 写入前必须显示完整 frontmatter 给用户看
- 中文叙述,frontmatter 字段名英文
