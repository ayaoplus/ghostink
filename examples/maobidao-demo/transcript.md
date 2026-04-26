# Demo Transcript — 用 maobidao 2.0-distilled 三件套跑 setup + write

> 这不是逐字记录,是关键交互的脚本。真实使用时对话会更长更碎,但关键节点
> 和这里一致。基于 ghostink 当前(2026-04 后)的 setup / write 流程,使用
> 猫笔刀 v2.0-distilled 三件套(本仓库 `built-in/library/{souls,forms,playbooks}/maobidao.md`)。

---

## Part 1:`/ghostink setup`

### 进入前 — 核心术语速览

**Assistant:** ghostink 把一个创作者拆成三件套 + 一个个性化补丁:

| 名字 | 一句话定义 | 例子 |
|---|---|---|
| **Soul** | 写什么 / 为什么写(价值观、关心什么) | 王小波关心人的尊严与理性 |
| **Form** | 怎么写(句式、语气、节奏、用词) | 王小波的反讽 + 长句分析 |
| **Playbook** | 什么场景写什么(常用打法、发文节奏) | 猫笔刀的日报 + 热点评论 |
| **Profile** | 你自己的真实经历、观点、素材 | 你刚转行的故事、独有的判断 |

后面每一步问问题前,会先告诉你这一步在选哪一件、不选会怎样。

### Step 1.1 — 平台

**Assistant:** 你主要在哪些平台发文?(影响默认语气与格式)
> A) 公众号 / 中文长文平台
> B) Twitter / X 长文或 thread
> C) 微博
> D) 个人博客 / Substack
> E) 其他(自由输入)

**User:** A 和 B

### Step 1.2 — 内容类型(先导)

**Assistant:** 在问读者画像之前,先大方向上分一下你写哪类内容——这决定我后面
怎么问、给你推荐哪些内置作家。

> A) 生活散文 / 个人记述
> B) 评论 / 思辨
> C) 陪伴 / 日常分享
> D) 知识 / 教程 / 工具
> E) 混合 / 其他

**User:** D 为主,但有 B 的成分(我会评论 AI 行业事件)

**Assistant:** 记下 chosen_content_type = D。Step 2 会把李笑来(D 知识型)和
猫笔刀(C 陪伴 / 思辨混合)置顶推荐。

### Step 1.3 — 读者画像(诉求 / 场景 / 关系三维)

**Assistant:** 读者来你这里主要求什么?(可多选)
> A) 情感连接 / 共鸣
> B) 信息 / 知识 — 学东西
> C) 观点 / 思辨 — 看到不一样的角度
> D) 陪伴感 — 像跟朋友聊天
> E) 娱乐放松

**User:** B + C(主要是学东西 + 看到不一样的角度)

**Assistant:** 读者主要在什么场景看你?(可多选)
> A) 通勤 / 碎片时间
> B) 睡前 / 深夜
> C) 工作 / 学习中(目的性强,通常只对知识 / 工具型有)
> D) 周末整段时间

**User:** A + C(通勤为主,工作中查询也有)

**Assistant:** 你和读者的关系是?
> A) 陌生订阅
> B) 半熟社群
> C) 朋友圈

**User:** A,基本都是陌生订阅

✓ 写到 `profile/identity.md`(诉求:信息 / 观点;场景:通勤 / 工作;关系:陌生)

### Step 2 — 选模仿起点

**Assistant:** 这一步是选你要不要从已有作家那里"借"一个起点。ghostink 把
作家拆成三件:Soul(写什么)、Form(怎么写)、Playbook(什么场景写什么)。

下面四个起步法,分别对应"三件全抄一个 / 三件混搭 / 自拆新作家 / 完全不模仿"。
新人推荐先全抄一个,跑顺了再混搭。

> A) 内置整套挂个作家 — Soul + Form + Playbook 全抄一个(推荐新人)
> B) 多作家组合 — soul / form / playbook 分别选
> C) 自拆参考 — 跑 distill 拆完再回来
> D) 纯对话生成 — 不模仿任何人

**User:** A

**Assistant:** 内置作家(基于你 Step 1.2 选的 D 知识型,猫笔刀 + 李笑来置顶):

> - **猫笔刀**(maobidao):陪伴派 + 思辨派 — **晚饭后蹲在小区抽烟跟你聊塘水
>   的财经老兄弟**,可信度来自实仓 + 关系网 + 承认无知
> - **李笑来**(lixiaolai):布道派 + 教程派 — 概念布道 / 框架教学
> - 三毛(sanmao):抒情派 + 陪伴派
> - 王小波(wangxiaobo):思辨派 + 辛辣派
> - 韩寒(hanhan):辛辣派 + 思辨派

**User:** 猫笔刀。我不喜欢李笑来那种概念锤的感觉。我要"我懂行 + 我也亏 +
我不教你"那种姿态,虽然我写的是 AI 不是财经。

**Assistant:** OK,maobidao 整套。Step 3-4 我用对话给 soul 加 AI 领域的差异化补丁。

特别说明:猫笔刀 v2.0-distilled 是基于他真实 35 篇文章 distill 出的,有几个
**关键指纹**:
- "……" 段间符(每篇 3-5 次,**AI 默认不用**,这是关键差异化)
- 自创词("含中量""塘主""4 倍理论")
- 讽刺锐度 4 级体系(主基调 2 级,绝不到 4 级粗口)
- 真人真事类比(用李佳琦 / 郭德纲 / 木头姐做对照)

你借这套写 AI 的话,我会保留这些指纹,但把财经具体词("含中量"等)替换成
AI 领域的等价物。

### Step 3 — 锐化 profile

**Assistant:** 说 3-5 件对你印象深、可能在文章里引用的真实经历。

**User:** 我带过几个实习生,他们用 AI 工具的方式让我意识到一些事...

(对话展开,产出 5 条 experiences,见 `profile/experiences.md`)

**Assistant:** 说 3-5 个你和多数人不太一样的观点。

**User:** 我觉得 AI 不会取代初级程序员,反而让他们更值钱;精准提问能力
比写代码本身更值钱;AI 工具会放大存量差距...

(对话展开,产出 5 条 opinions,2 条标 differentiator vs maobidao 的财经判断,
见 `profile/opinions.md`)

✓ 写到 `profile/{experiences,opinions}.md`

### Step 4 — 产出 soul

**Assistant:** 我把 maobidao 的 soul 复制下来,加你的差异化补丁:

- **0.4 Core Beliefs 加新一条**:"AI 让会用 AI 的初级程序员更值钱"
  (来自你 opinion O001,标记为 differentiator vs maobidao 财经判断)
- **0.5 Trust Mechanics 调整**:"公开亏损金额"换成"公开自己实操 AI 工具的
  失败案例 / 工具集成的反直觉数据"
- **0.6 Emotional Contract 调整**:情感锚点角色改为"AI 焦虑时的不慌声音"
  (替换猫笔刀的"市场暴跌时的冷静邻居")

显示完整 soul.md...

**User:** 看上去对。继续

### Step 5 — 选 form

**Assistant:** 这一步选 Form——决定怎么写(句式 / 用词 / 语气节奏 / 反讽)。
Soul 已经定了写什么,Form 定怎么写出来。

> A) [推荐 - 同源] maobidao form(口语化 + 短句轰炸 + 反讽)
> B) [兼容] hanhan form(短句轰炸 + 反讽)— 更辛辣,3 级反讽更密集
> C) 看不兼容选项

**User:** A,我就要猫笔刀那种聊天感

✓ form.md = maobidao form(2.0-distilled,14 节)

**Assistant:** 提醒一下,猫笔刀 form 有几个**关键指纹**会在 write 时强制贯彻:
- "……" 每篇 3-5 次(AI 默认不用,这是关键差异化)
- 收尾"就这些了"或变体(50% 概率)
- 段平均 3 句,视觉留白
- 讽刺主基调 2 级,**励志式仅自传/年终**(日报里禁用 — 跟鸡汤博主关键界碑)
- 真人真事类比(财经的木头姐/赖小民 → AI 领域的等价人物)

### Step 5.5 — 兼容性总览

```
你当前的搭配:
  Soul:  maobidao-fork-ai (陪伴派 + 思辨派)
  Form:  maobidao (口语化 + 短句轰炸 + 反讽)— 兼容性:同源 ✓
  Playbook: 待生成
继续 → A
```

### Step 6 — 建 playbook

**Assistant:** 这一步建 Playbook——你的"日常写作套路"。Soul + Form 决定一篇
文章长什么样,Playbook 决定**什么时候写哪种**。

基于 maobidao playbook 7 种 Play,推荐你常用的:

> ☑ **日报型**(maobidao 50%)— 每日 AI 行业新闻速串(对应你"通勤碎片"读者)
> ☑ **热点评论型**(maobidao 3%)— AI 大事件评论(GPT-5 发布、某裁员、某争议)
> ☑ **主题深挖型**(maobidao 10%)— 1 个核心议题深挖(对应你"工作/学习中"读者)
> ☑ **自传 / 年终复盘型**(maobidao 5%)— 偶发,允许情感光谱解锁
> ☐ 周末闲聊型 — 暂不,你不写跨主题闲聊
> ☐ 极端日报型 — AI 领域没有"大盘暴跌日"等价物
> ☐ 短公告型 — 暂不

**User:** 4 种全要

✓ 写到 `playbook.md`(4 个 plays + Cross-Play Prohibitions + Self-Check)

### Step 7 — 完成

```
✓ Setup 完成

  Soul:  maobidao-fork-ai (陪伴派 + 思辨派,AI 领域差异化)
  Form:  maobidao (口语化 + 短句轰炸 + 反讽,2.0-distilled 同源)
  Playbook: 日报型 / 热点评论型 / 主题深挖型 / 自传年终复盘型
  Profile:
    - identity.md (诉求:信息+观点 / 场景:通勤+工作 / 关系:陌生)
    - experiences.md (5 条)
    - opinions.md (5 条,2 条 differentiator)
    - refs.md (空,write 时会自然增加)

下一步:
  /ghostink write [topic]
```

---

## Part 2:`/ghostink write "AI 编程对初级程序员的影响"`

### Step 1 — 选 playbook

**Assistant:** 这个话题(AI 对初级程序员)我倾向 **热点评论型**,因为它围绕
一个行业现象需要明确判断。

> - A) 热点评论型(推荐)— frequency 3%,触发场景对
> - B) 日报型 — 单事件不需要日报型多话题串联
> - C) 主题深挖型 — 除非你想以"我带的那个实习生"切入,围绕一个核心议题打透
> - D) 自传 / 年终复盘型 — 不合适,这不是个人经历主题

**User:** A

### Step 2 — brainstorm

**Assistant:** 你对这事一句话最想说什么?

> - A) AI 让初级程序员更难找工作(主流观点)
> - B) AI 让初级程序员更值钱(反主流)
> - C) AI 加速初级到高级的过渡
> - D) 自由输入

**User:** B,但我想说的是"AI 让会用 AI 的初级程序员更值钱,不会用的更困难"

**Assistant:** 打断一下 — 你说"会用 AI 的更值钱,不会用的更困难"。这话比 B
更有意思,本质是**分化判断**而不是简单的"更值钱 vs 更难"。我怀疑你真正想
写的不是"AI 对初级程序员的影响",而是"AI 把初级程序员分成了两类"。

> - A) 对,换这个框架
> - B) 不对,保持原来的
> - C) 部分对,我调整

**User:** A

**Assistant:** 好,核心判断锁定:"AI 把初级程序员分成了会用 AI 和不会用的
两类,差距在拉大"。

差异化角度(从你 profile 找):
> - A) 你带过实习生,有具体观察(experiences E001 + E002)
> - B) 你 opinion O002 — "精准提问能力是新核心技能"
> - C) 自由输入

**User:** A + B

**Assistant:** 用哪些素材?
> - A) E001 实习生小张第一周怕用 AI
> - B) E002 实习生小李三天搞定一周需求
> - C) E004 面试候选人 40 分钟没让 AI 给出可用代码
> - D) E005 团队集成 AI 代码审查工具的反直觉数据

**User:** A + B + C + D 全用

**Assistant:** 提醒按 maobidao form 写时的关键约束:
- 段平均 3 句,大量"……"切话题(每篇 3-5 次)
- 真人真事类比(可以用 Linus / 业内人物)
- 自创词或夸张式串叠(如果合适)
- 讽刺 2 级主基调(只在系统性问题时升 3 级)
- 收尾"就这些吧"或变体
- 励志式禁用(日报/热点评论 Anti-Pattern)

✓ skeleton 写到 `drafts/_brainstorm/2026-04-24-ai-junior-dev.md`

### Step 3 — draft

按 skeleton 出文,加载 form.md 14 节约束。期间用户做了 2 处微调:
- 第 1 段把"我个人觉得"改成"我觉得"(更口语)
- 第 4 段加了"工具不公平"的反讽强化

最终成稿 1820 字,见 `drafts/2026-04-24-ai-junior-dev.md`

### Step 4 — form-check (新 14 节验证)

```
Form Check Report (maobidao 2.0-distilled, 14 节验证)

✓ Sentence Rhythm: 短句 58% / 中句 28% / 长句 14% (符合 ~55/30/15)
✓ Paragraph Rhythm: "……" 5 次 (符合 3-5 次)
✓ Vocabulary Must Use: "我觉得" "其实" "说白了" "你" "你们" 多次出现
✓ Vocabulary Forbidden: 0 出现 ("老铁" "家人们" "愿你" "加油" 等)
✓ Address Conventions: 自称"我",称读者"你",称他人"小张/小李/那个候选人"
✓ Analogy Style: IDE 普及做历史类比,符合
⚠ Signature Expressions: 仅出现"就这些吧",可考虑加"鸡贼"或"漏勺"之类(可选)
✓ Closing Patterns: "就这些吧 + 闲话破节奏" 符合
✓ Prohibition: 12 条 Anti-Patterns 全部 0 触发
✓ Play 纯净: 热点评论型,无励志式 / 无诗化(不踩 Anti-Patterns)

整体评分: 92/100
关键差距: Signature 词使用偏少(可加 1-2 处更"猫笔刀")
```

**User:** 看 Signature 那条提示,加一个"鸡贼"在评论"AI 取代论"那段

### Step 5 — deai-pass

```
De-AI Report (9 条规则)
- ✓ Rule 1 句式重复: 通过 (短句 + "……" 节奏自然)
- ✓ Rule 2 信息密度填充: 通过 (有具体经历 + 数据)
- ✓ Rule 3 generic 比喻: 通过 (用 IDE 普及做历史类比,具体)
- ⚠ Rule 4 情感平直: 微警告 (没有特别强的情感波动)
- ✓ Rule 5 结构对称: 通过
- ⚠ Rule 6 客观腔: 微警告 (有几处可加更主观的"我觉得")
- ✓ Rule 7 安全词: 通过
- ✓ Rule 8 万能开头: 通过
- ✓ Rule 9 总结欲: 通过 (没有 "总而言之")

整体评分: 81/100
建议: 不强制改,微警告 2 处可看情况
```

**User:** 接受当前版本

### Step 6 — illustrate (跳过)

### Step 7 — platform-output

5 个候选标题 (按风格区分,新设计):
1. 钩子型: "AI 把初级程序员分成两类"
2. 概括型: "AI 编程时代的初级程序员之路"
3. 反差型: "AI 让程序员更值钱,但不是所有人"
4. 隐喻型: "工具不公平 — AI 给会用的人 10x"
5. 直白型: "我观察了团队里 5 个初级开发者"

**User:** 1

✓ 最终成稿:`drafts/2026-04-24-ai-junior-dev.md`(标题:AI 把初级程序员分成两类)

### Step 8 — profile 自动追加

**Assistant:** 本次会话提到的新经历(团队三周 AI 代码审查实验,具体数据)
是否加到 experiences.md?

**User:** 加

✓ E005 已在,补充几个具体数据点

---

## 实际跑下来的体感(用 2.0-distilled vs 之前 1.0-draft)

详见 `notes.md`。
