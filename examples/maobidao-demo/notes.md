# Demo 反思与改进建议(2.0-distilled 版本)

跑这次端到端 demo 时发现的流程问题与改进点。
本次基于 **maobidao 2.0-distilled** 三件套 + 新 setup / write 流程跑。

> v1.0-draft 时期(P4-T18)的反思见本文末尾"v1 历史反思"段落。

## 顺畅的点(2.0 spec 显著改进)

### 1. setup 的"核心术语速览"减少了概念懵逼

新 setup 顶部加了 Soul / Form / Playbook / Profile 一句话定义表。
跑 demo 时用户在 Step 2 看到"Soul + Form + Playbook 全抄一个"立刻懂,
不用追问"这三个是啥"。

### 2. 内容类型先导问题(Step 1.2)给了 Step 2 推荐方向

D 知识 / 工具型 → 自动把猫笔刀 + 李笑来置顶。用户看到"猫笔刀(陪伴 + 思辨)"
和"李笑来(布道 + 教程)"这两个对比,立刻能选(选了猫笔刀因为不喜欢概念锤)。

### 3. 读者画像的"诉求维度"比旧"身份维度"更通用

旧版的"同行专业 / 决策者 / 跨界爱好者"在 AI 写作场景里别扭(决策者?投资人?
不对路)。新"诉求维度(信息 / 观点 / 陪伴 / 娱乐)"直接对得上写作目的。

### 4. Step 2 主动暴露猫笔刀的关键指纹(写在 Soul 后告知用户)

新 setup 在用户选了 maobidao 后,主动告诉:
- "……" 段间符是关键差异化
- 自创词("含中量""塘主""4 倍理论")
- 4 级讽刺锐度体系
- 真人真事类比

这让用户**知道借这套 form 会带来什么**,可以理性选择。
跑 demo 时用户说"哦那要不要换 form"——但确认这些跟 AI 写作兼容(真人类比的
"木头姐"换成"Linus"就行),最终决定保留。

### 5. write 的 form-check 用新 14 节验证

旧 form-check 只看 Sentence Rhythm + Vocabulary。新 14 节验证多了:
- Address Conventions(自称 / 称读者 / 称他人)
- Analogy Style(真人类比是否出现)
- Signature Expressions(招牌词是否够)
- Play 纯净度(有没有踩 Anti-Patterns)

跑 demo 时,form-check 92/100 给出的"可加 1-2 处更猫笔刀"非常具体——
建议加"鸡贼"形容 AI 取代论叙事,用户加了 1 处,IP 感立刻强。

### 6. Self-Check Checklist 在 brainstorm 时就显示

新 playbook 末尾的 11 条 Self-Check 在 brainstorm 时就被 skeleton 引用。
用户写 draft 时心里有数(不能写励志,不能"加油 / 愿你"等),减少返工。

## 卡顿 / 改进点(2.0 暴露的新问题)

### 1. Step 2 暴露"指纹"环节信息量大

新 setup 把猫笔刀的关键指纹("……" / 自创词 / 4 级讽刺 / 真人类比)在用户
选完后一次性抛出。**信息密度高,新人可能没反应过来**。

**改进方向**:把指纹分两层——基础(短句+"……"+收尾)立刻说,高级(真人类比+
自创词+夸张式串叠)在 Step 5 选 form 后再说。

### 2. Soul 0.4 信念优先级链的差异化补丁

新 Soul 模板要求"信念之间冲突时谁赢"给具体优先级链。猫笔刀的链是
"诚实 > 取悦 > 流量 / 守护家庭 > 个人英雄主义 / 数据 > 情绪"。

用户写 AI 的话,应该把"守护家庭"这条改成什么?demo 里我留空了,但实际应该
有一条平行的"AI 实践 > AI 焦虑 / 真实经验 > 行业大词"之类的。

**改进方向**:Step 4 差异化补丁阶段,显式问用户"猫笔刀有 5 条优先级链,
你的领域里对应是什么?"

### 3. Form 7 真人真事类比的"领域迁移"没有自动化

猫笔刀 form 列了赖小民 / 木头姐 / 郭德纲 等真人作类比库。AI 领域的等价人物是
谁?demo 里我手工映射(Linus / Andy Chen),但这个映射应该 setup 时引导用户
自己列出来 5-10 个领域内"真人参照系"。

**改进方向**:Step 4 加一步"领域真人类比库迁移"——让用户列 5-10 个 AI 领域
的知名人物 / 事件,写到 profile/refs.md。

### 4. Playbook 7 种 Play 选择时,部分 Play 在新领域不适用

猫笔刀的"极端日报型"(史诗暴跌 / 暴涨)在 AI 领域没有完全等价物——AI 有
"GPT-5 发布"这种事件,但不像股市每天都有数据驱动。

跑 demo 时用户跳过了极端日报 / 周末闲聊 / 短公告 3 个 Play。这 OK,但
**Step 6 的引导可以更主动地告诉用户哪些 Play 在新领域不适用**。

**改进方向**:Step 6 prompt 升级:"基于你的领域(AI),以下 Play 可能不适用:
极端日报型(无对应)/ 周末闲聊型(你的读者画像是工作场景,不是周末长读)。
可以勾掉,以后用 write 时不会被推荐。"

### 5. form-check 92/100 vs deai-pass 81/100 的关系不清

form-check 给 92 分(form 合规度)+ deai-pass 给 81 分(去 AI 感)。两个分数
不是简单相加,但用户感觉"两个都有分,是不是要都到 90 才算好?"

**改进方向**:在 form-check 报告末尾说明"form-check ≥85 算合格,deai-pass
是参考分,决定权在你"。

## 跟 v1.0-draft 时期相比的关键改进

| 维度 | v1.0-draft | v2.0-distilled |
|---|---|---|
| Soul | 6 节,无 Uniqueness Statement | 6 节 + Uniqueness Statement(导航灯) |
| Form | 8 节,Vocabulary 3 层 | 14 节,Vocabulary 4 层 + 真人类比 / 夸张式串叠 / 4 级讽刺 |
| Playbook | 3 个 Play,无 frequency 无 Anti-Patterns | 7 个 Play + frequency + Cross-Play Prohibitions + Self-Check Checklist |
| 引文证据 | 无 | 35 篇带 [#编号 §段] |
| trace-to-Soul | 无强制 | identity-first 硬约束,每条规则带 [traces to: Soul 0.X] |
| 数字字段 | 模糊估计 | 短句% / 段长% / frequency% 都有具体数 |

跑下来明显感觉 IP 感更强:draft 里加"鸡贼"一处,form-check 立刻分上去 92,
读起来真的更像猫笔刀。v1 写的版本读起来像"AI 模仿一个口语化博主",v2 写的
版本读起来像"猫笔刀去写了一篇 AI 文章"。

## 总体评价

**2.0-distilled 三件套显著比 1.0-draft 精度高**。核心提升来自:
1. 数字字段 vs 模糊描述(短句 ~55% 比"短句为主"更可执行)
2. trace-to-Soul 强制约束让 form 规则有理由(不再是"通用写作建议在伪装")
3. 真人真事类比 + 自创词 + 夸张式串叠 = 不可复制 IP 信号

**剩下的改进空间集中在"领域迁移"环节** — 借 maobidao 写 AI 时的差异化补丁
(Soul 优先级链 / Form 真人类比库 / Playbook 适用 Play 筛选)需要更显式的
引导。

---

## v1 历史反思(P4-T18 时期,2026-04 早些时候)

> 此段保留作为对照。v1 用的是 1.0-draft 三件套,反思见 git 历史 commit
> `d8e2cdb P4: end-to-end demo (catblade)`。当时主要反思:
> - Step 3 experiences 收集太慢
> - Step 4 差异化补丁逻辑不透明
> - form-check / deai-pass 报告太长
> - maobidao form 跨领域时 Vocabulary 词表("老乡")出戏
> - deai 报告对 form 节奏的误判
>
> 这些问题在 v2.0-distilled 三件套 + 新 setup 流程下,**前 4 个已经显著改善**
> (核心术语速览 / 差异化补丁透明 / form-check 14 节验证更精准 / Vocabulary
> 4 层 + Forbidden 列表更全)。第 5 个(deai 误判 form 节奏)还在 — 需要
> deai-pass 升级看 form 的 factions / 句长统计自动调灵敏度。
