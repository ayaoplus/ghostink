# /ghostink library

库管理。子命令:`list` / `info` / `distill` / `pick`。

## 用法

```
/ghostink library list                           看库存
/ghostink library list --include-incompat        连不兼容也看
/ghostink library info <name>                    看某个详情
/ghostink library distill <author> [opts]        提炼一个新参考
/ghostink library pick <type> <name>             切换 soul/form/playbook
```

`<type>` 取值:`soul` / `form` / `playbook`。

> **注**:`distill` 是原 `analyze` 子命令的替代。analyze 的"一遍过快速分析"
> 在新设计里是 `distill --quick`,默认 `distill` 是 deep 模式(走多轮
> predict-then-compare 迭代,产出更经得起推敲的三件套)。

---

## list 子命令

### 用途
列出所有可用的 soul / form / playbook,标版本徽章 + 当前在用标记 + 兼容性。

### 流程

1. 扫描以下两处:
   - `built-in/library/{souls,forms,playbooks}/*.md`(内置)
   - `[studio]/library/{souls,forms,playbooks}/*.md`(用户自拆,若存在)

2. Read 每个文件的 frontmatter(只读 frontmatter,不读正文,节约成本)

3. 加载当前 studio 根的 `soul.md` / `form.md` / `playbook.md` 的 frontmatter 作为对照

4. 输出表格(分三块:soul / form / playbook),每行含:
   - Name(slug)
   - Display name(中文)
   - Version(`1.0-quick` 加 ⚠,`1.0-draft` 加 ⚠,`2.0-distilled` 推荐)
   - Source(built-in / user-distilled-quick / user-distilled-deep)
   - Factions
   - 兼容性状态(同源 ✓ / 兼容 ✓ / 中性 / 不兼容 ⚠ / 当前在用 ★)
   - Description

5. 默认隐藏不兼容项,加 `--include-incompat` 显示

6. 末尾给提示:用 `info <name>` 看详情,用 `pick` 切换

### 输出示例

```
Soul Library
============

Name        Display    Version          Source                  Factions          Status      Description
maobidao    猫笔刀     2.0-distilled    user-distilled-deep     陪伴派,思辨派    ★ 在用       有钱人的陪伴式财经
sanmao      三毛       1.0-draft⚠      built-in                抒情派,陪伴派    ✓ 兼容       流浪叙事 + 浓情
wangxiaobo  王小波     1.0-draft⚠      built-in                思辨派,辛辣派    ⚠ 不兼容     反讽 + 理性
                                                                                              (用 --include-incompat 显示)

Form Library
============
...

Playbook Library
================
...
```

---

## info 子命令

### 用途
看某个 soul / form / playbook 的详情。

### 流程

1. 接受 `<name>`,在 built-in 和用户 library 中查找匹配
   - 同名优先用户 library(若存在)覆盖内置
   - 找不到 → 提示"未找到,用 list 看清单"

2. 输出:
   - frontmatter 全部字段
   - 类型标签(soul / form / playbook)
   - description
   - 章节标题列表(只标题,不读正文 — 节约成本)
   - 与当前 studio 的兼容性
   - 提示用 `pick <type> <name>` 切换

3. 询问"要看完整内容吗?" → 是则 Read 整文输出。

---

## distill 子命令

### 用途

从一位作者的历史文章提炼出 soul / form / playbook 三件套。这是 ghostink 的
**核心提炼引擎**——决定后续 write 出来的文章能不能像这位作者。

### 设计哲学

三条不可妥协的原则,distill 全程必须遵守:

1. **identity-first** — Soul 必须先于 Form 完成。Form / Playbook 的每条规则
   必须 trace 回 Soul 某节(`[traces to: Soul 0.X]`)。trace 不回的规则就是
   "通用写作建议"在伪装成"个人风格",立刻删。
2. **多轮迭代,不要一遍过** — 一遍读完出的 spec 必然填空式应付,捕捉不到
   差异化(详见 Phase 3)。**deep 模式必须跑 ≥3 轮 predict-then-compare**,
   quick 模式可以 1 轮但要明确告诉用户精度有限。
3. **样本驱动,不要 LLM 自由发挥** — 所有数字字段(短句% / frequency% /
   词频)必须基于实际样本统计;所有引文用 `[文章 §段]` 标注,不要意译,
   不要 LLM 改写;不确定的项标 `[LOW-CONFIDENCE]`,下一轮重点验证。

### 用法

```
/ghostink library distill <author> [opts]
```

### 选项

| 选项 | 说明 |
|---|---|
| `<author>` | 作家 slug(英文小写),用作输出文件名 |
| `--path <dir>` | 文章目录路径。缺省按以下顺序找:1) `[studio]/library/articles/<author>/`,2) 询问用户 |
| `--quick` | quick 模式(5+ 篇,~10 分钟,跳过 Phase 3 多轮迭代,版本标 `1.0-quick`) |
| `--deep` | deep 模式(默认,20+ 篇,30-60 分钟,完整 7 phase + 多轮迭代,版本标 `2.0-distilled`) |
| `--only=soul\|form\|playbook` | 只提炼其中一项 |
| `--rounds=N` | 强制 Phase 3 跑 N 轮(默认按收敛自动停,封顶 5 轮) |
| `--display-name <name>` | 中文显示名(缺省询问用户) |

### 模式对比

| 维度 | `--quick` | `--deep`(默认) |
|---|---|---|
| 最少文章数 | 5 篇 | 20 篇(<20 警告但允许继续) |
| 多轮迭代 | 跳过 Phase 3 | 跑 3-5 轮 |
| 词频统计 | LLM 估算 | 跨所有文章统计 |
| 模板字段填满度 | ~60% | ≥90% |
| 输出版本 | `1.0-quick` | `2.0-distilled` |
| 用时 | ~10 min | 30-60 min |
| 适用场景 | 第一次试水 / 想快速看个大概 | 真要拿来生产文章 |

### 流程总览

```
Phase 0  Setup(扫文章 / 选模式 / 建目录 / 告知用户)
Phase 1  Soul 提取(10-15 篇最大多样性,身份先于风格)
Phase 2  Form 第一遍粗描(15-20 篇,每条 trace 回 Soul)
Phase 3  多轮 predict-then-compare 迭代(deep 模式核心,3-5 轮)
Phase 4  Playbook 聚类 + frequency 统计
Phase 5  词频统计(跨所有文章)
Phase 6  Finalize + 用户确认 + 兼容性标签自动填充
Phase 7  Profile bootstrap(若文章是用户自己写的)
```

quick 模式跳过 Phase 3,Phase 5 改 LLM 估算。

---

#### Phase 0:Setup

1. 解析 `<author>` 和 `--path`,统计文章数。
2. 数量判断:
   - `--quick` + ≥5 篇 → quick 模式 OK
   - 默认/`--deep` + ≥20 篇 → deep 模式 OK
   - 不足:warning + 询问继续(若 deep 模式 < 20 篇,提示"精度会有限,
     建议改 --quick 或补文章")
   - >200 篇 → 告知"会从中采样,不全读"
3. 创建输出目录:`[studio]/library/{souls,forms,playbooks}/`
   + 日志目录 `[studio]/library/distill_logs/<author>/`
4. 告诉用户:
   ```
   开始提炼 <author>(<deep/quick> 模式,<N> 篇文章)。
   预计 <X> 分钟。我会自动跑完所有阶段,完事再来给你看结果。
   过程日志写在 [studio]/library/distill_logs/<author>/
   ```

---

#### Phase 1:Soul 提取(身份先于风格)

> **为什么 Soul 先做**:风格选择是身份的下游产物。一个作者用脏话不是因为
> "词汇偏好",是因为定位是"不加滤镜的朋友"——知道身份后,所有风格观察
> 都能解释。**直接跳到风格分析等于做无根之木**。

##### 1.1 选样本

挑 10-15 篇**最大多样性**(quick 模式 3-5 篇):
- 不同时期(早期 vs 近期)
- 不同情绪(高兴 / 愤怒 / 反思 / 脆弱)
- 不同主题(若适用)
- 不同长度(短文 vs 长文)

##### 1.2 作为读者读,不是分析者读

每篇问 6 个问题(对应 Soul 模板 0.1-0.6):

1. 为什么有人会明天再来读这位作者?(0.1 Reader Value)
2. 我从这篇得到什么是新闻网站 / 教科书给不了的?(0.1)
3. 作者跟我(读者)的关系是什么?感觉像谁?(0.2 Positioning)
4. 作者相信什么?什么让 ta 愤怒 / 悲伤 / 兴奋?(0.4 Core Beliefs)
5. 作者下结论时,我为什么信(或不信)?(0.5 Trust Mechanics)
6. 我打开这篇文章前,期待什么样的情感体验?(0.6 Emotional Contract)

##### 1.3 写 Soul 草稿

按新模板(`templates/soul.template.md`)填:
- **Uniqueness Statement**:1-3 句锁差异化(强制反例对照"他不像 X 那样
  做 Y,他做 Z")。这是后面所有规则的导航灯,**填空式应付立刻打回重写**。
- 0.1 Reader Value(含取关测试)
- 0.2 Positioning(含权威管理方式)
- 0.3 Narrative Engine(含论证最小单元)
- 0.4 Core Beliefs(信念 ≥3 篇佐证 + 优先级链)
- 0.5 Trust Mechanics(脆弱部署给频率 + 触发场景)
- 0.6 Emotional Contract(情感锚点绑定具体情境)

不确定的项标 `[LOW-CONFIDENCE]`,下一轮重点验证。

写到 `[studio]/library/souls/<author>.md`,version 按模式
(`1.0-quick` 或 `1.0-draft`,Phase 6 后 deep 模式升 `2.0-distilled`)。

##### 1.4 自查 + 日志

按 Soul 模板末尾的"完成检查表"过一遍,任何一条 fail 重写。
日志写到 `distill_logs/<author>/phase1-soul.md`(记录读了哪些文章 + 关键发现)。

---

#### Phase 2:Form 第一遍(粗描)

> **关键约束**:Form 的每条规则**必须**带 `[traces to: Soul 0.X]` 标记,
> trace 不回的就是通用写作建议在伪装,删。

##### 2.1 选样本

15-20 篇(quick 模式 5+),可与 Phase 1 重叠(同样的文章用不同视角再读)。

##### 2.2 提取 Form 14 节

按新模板(`templates/form.template.md`)填:

| 节 | 提取要点 |
|---|---|
| 1. Persona Voice | 写作人格 / 核心张力 / 脆弱维度 |
| 2. Reader Cognitive Model | **关键** — 默认读者智商假设 + 已知/未知边界 + 论证强度档位 |
| 3. Sentence Rhythm | 句长统计(数字)+ 节奏型(周期描述) |
| 4. Paragraph Rhythm | 段长统计 + 段间转折手法 + 留白逻辑 |
| 5. Vocabulary Fingerprint | Must / Often / Never / Forbidden 4 层(共 ≥35 条) |
| 6. Address Conventions | 自称 / 称读者 / 称他人 / 禁用称谓 |
| 7. Analogy Style | 类比库来源 + 好类比标准 + 密度 |
| 8. Context Projection | 参考系 + 讽刺锐度等级 + 信息密度模式 |
| 9. Opening Patterns | ≥4 种 + 频率 + 禁用开头 |
| 10. Closing Patterns | ≥3 种 + 频率 + 禁用结尾 |
| 11. Emotional Expression Toolkit | 7 种情绪各给样本句 + 克制系数 |
| 12. Signature Expressions | ≥8 条 + ≥1 自创词(关键指纹) |
| 13. Punctuation & Layout | 标点频率 + emoji/加粗/列表习惯 |
| 14. Prohibition List | ≥10 条 + 每条 trace 回 Soul |

##### 2.3 trace 到 Soul

每节(尤其 Vocabulary / Prohibition / Punctuation)末尾必须写 `[traces to: Soul 0.X]`。
连不上的规则要么删,要么倒推 Soul 该补什么(回 Phase 1 修订)。

##### 2.4 写到文件 + 日志

写 `[studio]/library/forms/<author>.md`。日志 `phase2-form.md`。

---

#### Phase 3:多轮 predict-then-compare 迭代

> **这是 distill 最值钱的设计**。一遍读完出的 spec 是"印象派"——什么
> 看着像就写什么。多轮迭代是让 spec 跟自己赛跑收敛——用当前 spec 预测
> 一篇没读过的文章会有什么特征,跟实际对比,印证 / 修订 / 新增。
>
> **quick 模式跳过此 Phase**(用户已知精度有限)。
> **deep 模式必跑 ≥3 轮,封顶 5 轮**。

##### 3.1 每轮流程

1. 选 15-20 篇**新文章**(本 Phase 之前没读过的)
2. 对每篇文章:
   - 用当前 spec(Soul + Form)**预测**这篇会有什么特征
     - "应该是 [开头模式 X],平均句长约 N 字,会用 [Signature 词],
       情绪基调 Y"
   - 实际读这篇,跟预测对比:
     - **印证项** → 提升 confidence(`[LOW-CONFIDENCE]` 升 confirmed)
     - **违反项** → 标 `[REVISE]`,记修订建议
     - **新模式** → 标 `[NEW]`,可能要加新条目
     - **DNA 假设违反** → 严重信号(以为作者一直自信,结果一簇深度不安
       的文章)→ 回 Phase 1 修 Soul
3. 整轮跑完后,统一更新 spec(避免边读边改产生抖动)
4. 写日志 `phase3-round-N.md`(记录哪些印证 / 修订 / 新增)

##### 3.2 收敛检查

每轮结束跟上一轮 diff:
- diff 只剩"轻微措辞调整 + 频率数字微调" → **收敛,停**
- diff 含新增条目 / 删除条目 / Soul 修订 → 继续
- 跑满 5 轮还在变 → 强制停,提示用户"5 轮未收敛,可能样本不够多样
  或作者本身不一致"

##### 3.3 LOW-CONFIDENCE 处理

每轮结束扫一遍残留的 `[LOW-CONFIDENCE]`:
- 经过 ≥2 轮验证还是 LOW → 删除(证据不足,留着是噪音)
- 升级为 confirmed 的去掉标记

---

#### Phase 4:Playbook 聚类 + frequency 统计

##### 4.1 聚类

把目前为止读过的所有文章按结构相似度聚类:
- 开头模式相似 + 主体推进逻辑相似 + 长度区间相似 = 同类
- 一类至少 5 篇(<5 的不独立成 Play,可能是杂项)

##### 4.2 为每个聚类定义 Play

按新模板(`templates/playbook.template.md`)填:
- **frequency**:`~X%`(本类占全部文章的比例,所有 Play 加起来 ≈100%)
- **trigger / serves_value / emotional_baseline / length / material_from / publish_cadence**
- **Structure**(段落级流程,每步说"做什么 + 为什么")
- **Core Characteristics**(4-6 条灵魂识别码,不只是 Structure)
- **Opening / Closing / Anti-Patterns**
- `[traces to: Soul 0.X]`(本 Play 服务于 Soul 哪节)

##### 4.3 跨 Play 总禁忌 + 自检表

填 `Cross-Play Prohibitions`(≥5 条,每条 trace 回 Soul)和
`Self-Check Checklist`(≥8 条,每条可立即 yes/no)。

##### 4.4 询问用户保留哪些

```
我从 <N> 篇文章里聚出 <M> 类打法:
  - <Play 1> (~X%):<一句描述>
  - <Play 2> (~Y%):<...>
  ...
要全部保留吗?还是只挑你打算复用的?(<5% 的可以考虑合并或丢)
```

写到 `[studio]/library/playbooks/<author>.md`。

---

#### Phase 5:词频统计

跨**所有**文章(不只 Phase 1-3 细读的)做统计:

##### 5.1 高频短语提取

- 中文:2-4 字常用短语
- 英文:常见表达
- 排除停用词、通用词
- 输出按频率降序的清单

##### 5.2 Vocabulary Fingerprint 4 层校准

回到 Form 模板的 Section 5,用统计结果校准:
- **Must Use**:统计高频 + 该作者标志性的词(频率显著高于 generic AI 默认)
- **Often Use**:中频 + 有特色
- **Never Use**:全样本 0 出现的常见书面/AI 词("因此 / 此外 / 综上 /
  赋能 / 范式" 等)
- **Forbidden**:全样本 0 出现的标志性禁用词(可能含网红称谓"老铁"等)

quick 模式:LLM 估算,不做全文统计。

##### 5.3 Signature Expressions 校准

用统计找出"这个作者用得特别多但其他作家不怎么用"的招牌表达,补到
Section 12。**至少要找出 1 个该作者自创词**(其他人不用的)——找不到就
跑额外几篇文章直到找到。

##### 5.4 写日志

`phase5-vocab.md`(完整词频表 + 校准过程)。

---

#### Phase 6:Finalize

##### 6.1 兼容性标签自动填充

1. 基于 `soul.factions`,推 form 的 `compat_soul` / `incompat_soul`(同派系
   兼容,对立派系不兼容,中性的不填)
2. 基于 `soul.factions` + 各 Play 的 `serves_value`,推 playbook `plays`
3. 写到三个文件的 frontmatter

##### 6.2 deep 模式版本升级

deep 模式所有三件套版本从 `1.0-draft` 升 `2.0-distilled`。
quick 模式保持 `1.0-quick`。

##### 6.3 完整呈现给用户(分两层)

```
✓ 提炼完成:<author>(<deep/quick>,<N> 篇文章,<R> 轮迭代)

【Tier 1 — 这位作者是谁】(Soul 摘要)
- Uniqueness:<1 句>
- 核心价值主张:<1 句>
- 跟读者的关系:<1 句>
- Top 3 信念:<列>
- 取关测试:<答案>
- 信任机制摘要:<1 句>

【Tier 2 — 这位作者怎么写】(Form + Playbook 摘要)
- 关键风格特征(5-7 条):<列>
- 文章打法(<M> 种,各占比):<列>
- Top 10 must-use 词:<列>
- Top 10 never-use 词:<列>
- Confidence 评估:<X 条 confirmed,Y 条 LOW-CONFIDENCE 已删,Z 条保留>

文件位置:
  [studio]/library/souls/<author>.md
  [studio]/library/forms/<author>.md
  [studio]/library/playbooks/<author>.md

提炼日志:[studio]/library/distill_logs/<author>/

请先读 Soul(身份层),感觉对了再看 Form / Playbook。任一层不准都告诉我,
我可以调整。
```

##### 6.4 询问下一步

```
要立刻 pick 这套吗?
  A) pick 整套(soul + form + playbook 同时切换)
  B) 只 pick 其中一项
  C) 都不,先看看
  D) 不准,我要修订(说哪一节,我重做)
```

---

#### Phase 7:Profile bootstrap(条件触发)

如果文章看起来是**用户自己写的**(检查第一人称 / 个人轶事 / 主观立场密集):

1. 抽提个人细节 → 草拟 `[studio]/profile/identity.md`
2. 抽提生活经历 → 草拟 `[studio]/profile/experiences.md`(E### 编号)
3. 抽提观点立场 → 草拟 `[studio]/profile/opinions.md`
4. 抽提常引人物 / 书 / 游戏 → 草拟 `[studio]/profile/refs.md`
5. 告诉用户:"我从你的文章里也抽了一些个人背景,放在 profile/。请审,
   不准确的改一下。"

如果是别人的文章:跳过 Phase 7,提示"你提炼的是别人,你自己的 profile
要单独跑 `/ghostink setup` 来填"。

---

### 中断恢复

distill 全程的进度都记在 `distill_logs/<author>/`:
- `phase1-soul.md`(Phase 1 完成标志)
- `phase2-form.md`
- `phase3-round-N.md`(每轮一个)
- `phase4-playbook.md`
- `phase5-vocab.md`
- `meta.json`(当前 phase / 已读文章列表 / 模式)

中途中断后再跑 `distill <author>`,自动检测日志,提示"上次到 Phase X 第 Y 轮,
继续 / 重做"。

---

### 实现备注

- **identity-first 是硬约束**:Phase 1 不完成不让进 Phase 2。代码层面用
  "Soul 文件存在 + 通过自查表"作 gate
- **多轮迭代不要让用户中途确认**:每轮跑完写日志,所有 phase 跑完
  Phase 6 一次性给用户审。中途打断会破坏 LLM 的连续推理
- **trace-to-Soul 检查**是机械的,distill 自己能查:扫 form / playbook
  每节末尾有没有 `[traces to: Soul 0.X]`,没有就 fail
- **样本统计要诚实**:LLM 没法精确数字数 / 句子数,但能给"约 30%"级别的
  估算。所有数字字段都要标注是"统计"还是"估算"
- **quick 模式不是 deep 模式的子集**:quick 是阉割版,版本号要醒目
  (`1.0-quick`),让用户知道这不是终态
- 中文叙述,frontmatter 字段名英文

---

## pick 子命令

### 用途
切换当前在用的 soul / form / playbook。

### 用法

```
/ghostink library pick soul <name>
/ghostink library pick form <name>
/ghostink library pick playbook <name>
```

### 流程

1. 在 `built-in/library/<type>s/` 与 `[studio]/library/<type>s/` 中查找 `<name>.md`
   - 找不到 → 提示并停

2. Read 目标文件 frontmatter

3. Read 当前 studio 根的对应文件(如果是 pick form,读 soul.md 和 playbook.md;如果是 pick soul,读 form.md 和 playbook.md)

4. **兼容性检查**:

   a. **同源套件**(目标文件的 name 与当前 studio 中 soul/form/playbook 任一同名):
   ```
   你正在切换到同一作者的 X(<author>)。同源套件兼容性最好。
   继续吗?(默认 是)
   ```

   b. **兼容**(目标 factions ⊆ 当前 X 的 compat_*):
   - 安静通过

   c. **中性**(既不在 compat 也不在 incompat):
   - 安静通过,但提示"未明确兼容"

   d. **不兼容**(目标 factions ∩ 当前 X 的 incompat_* ≠ ∅):

   ```
   ⚠ 兼容性警告

   你想把 <type> 换成 <name>。
   当前 <other_type> 是 <factions>(来自 <other_name>)。
   <name> 标记 incompat_<other_type>: <冲突项>

   可能问题:
     <基于派系冲突的具体描述,如"陪伴派 soul + 反讽 form
     → 文章可能感觉一边温柔陪伴,一边冷笑反讽">

   继续吗?
     A) 继续(我想试试反差搭配)
     B) 取消
     C) 看看哪些 <type> 跟我的 <other_type> 兼容
   ```

   - 用户选 C → 显示兼容列表(扫所有 library 文件,筛 compat_X 含当前 factions 的)

5. **备份当前文件**(用户选继续后):
   - 复制 `[studio]/<type>.md` 到 `.ghostink_backup/<type>.<YYYYMMDD-HHMMSS>.md`

6. **复制目标到当前**:
   - 把 `built-in/library/<type>s/<name>.md` 或 `[studio]/library/<type>s/<name>.md` 内容复制到 `[studio]/<type>.md`

7. 提示成功:

   ```
   ✓ 已切换 <type> 为 <name>
   备份在 .ghostink_backup/<type>.<timestamp>.md(可恢复)
   ```

### 警告文案模板

```
⚠ 兼容性警告

你想把 form 换成 [王小波]。
当前 soul 是 [陪伴派, 思辨派](来自 maobidao)。
王小波 form 标记 incompat_soul: [陪伴派, 抒情派]

可能问题:
  陪伴派 soul + 反讽 form
  → 文章可能感觉一边在温柔陪伴,一边在冷笑反讽

继续吗?
  A) 继续(我想试试反差搭配)
  B) 取消
  C) 看看哪些 form 跟我的 soul 兼容
```

### 测试用例(文档化)

| 场景 | 当前 soul | pick form | 期望 |
|---|---|---|---|
| 同源 | maobidao | maobidao | 安静通过(同源套件) |
| 兼容 | maobidao | hanhan | 走兼容性比对,可能中性或不兼容(看 factions) |
| 不兼容 | sanmao(抒情派) | wangxiaobo(反讽) | 弹警告 A/B/C |

---

## 实现备注

- 全程用 LLM 读 frontmatter + 比对集合,不需要脚本
- 备份机制必须可靠 — 用户切错了能 1 步恢复
- 警告语气避免吓退,保留"我知道我在做什么"的快捷通道
- 切换 soul 影响最大(灵魂换了相当于换了人),备份尤其重要
- 中文叙述,frontmatter 字段名英文
