# Ghostink 设计文档

> 整体架构 + 数据模型 + 设计哲学。

---

## 1. 背景与目标

### 1.1 用户画像

主要服务高频更新的中文新媒体创作者:

- 阵地:公众号长文 + 微博/X 短内容(可能多平台并存)
- 内容:有个人特色但要输出干货
- 频率:周更或更高,需要降低单篇创作成本

### 1.2 设计目标

1. **入口清晰**:新人不需要研究文档就知道从哪开始
2. **闭环完备**:不依赖用户已有素材也能上手(开箱即用)
3. **可演进**:用户从"模仿"到"找到自己"的过程被工具支撑
4. **可重组**:模仿对象的灵魂、文笔、打法可以**分别借用**而非整体绑定

### 1.3 反对什么

- 反对"一份 spec 打天下"——把 IP 灵魂和文字技法揉在一起
- 反对"先把所有概念学完才能开始"——多命令各有职责的高心智负担
- 反对"分用户类型出 fast/normal track"——单一主流程 + 可选步比分叉好

---

## 2. 核心概念

整个系统建立在 **5 个概念** 上,每个有清晰职责、单一所有者、明确生命周期。

| 概念 | 是什么 | 跨文章稳定性 | 谁产 | 谁用 | 文件 |
|---|---|:---:|---|---|---|
| **Soul** | 我是谁:价值/定位/信念/契约/信任机制 | 高 | setup 一次产出 | brainstorm 选切入、检查灵魂契合度 | `soul.md` |
| **Form** | 字怎么摆:句式/词汇/开头结尾/节奏 | 中(可换) | setup pick 或 distill 拆 | write 写稿、check 审稿、deai 调灵敏度 | `form.md` |
| **Playbook** | 怎么打这一类:文章类型们 | 中(随领域演进) | setup 建 + 用户增加 | write 选打法 | `playbook.md` |
| **Profile** | 我有什么:身份/经历/观点/参照系 | 高(增量长大) | setup 粗建 + write 自动追加 | brainstorm 选话题选素材 | `profile/*` |
| **Skeleton** | 这一篇的骨架:角度/结构/递进/收尾 | 单篇产物 | brainstorm 阶段产 | draft 阶段消费 | `drafts/_brainstorm/*` |

### 2.1 Soul vs Form 的根本差异

```
Soul 决定:为什么读者明天还想看你
Form 决定:这一段字怎么排
```

Soul 跨主题、跨平台几乎不变。Form 可以为不同场景换(同一个 Soul 的人在
公众号写陪伴长文,在 X 写辛辣短句)。

### 2.2 Soul × Form × Playbook 的关系

不是三件正交的事,**它们之间有兼容约束**:

- 一个 Soul 兼容若干种 Form,但不是任意 Form 都兼容
- 一个 Soul 倾向某些 Playbook(陪伴 Soul 不太用"硬核教程"打法)
- 同源套件(同一作者拆出来的三件套)兼容性最强

兼容性通过 frontmatter 标签机制处理(见第 8 节),**软约束**——不强禁,只警告。

### 2.3 Profile 的位置

Profile **不是**架构的一层,是**与 Soul/Form/Playbook 平行的另一种输入**。

写一篇文章时,实际输入是:

```
主题(系统外) + Soul + Playbook + Profile (素材) + Form
                    ↓
              产出 Skeleton
                    ↓
              产出 Draft
```

Soul/Form/Playbook 描述"我"的特征,Profile 提供"我"的具体素材。两边都需要。

---

## 3. 架构总览

### 3.1 数据流

```
┌─────────────────────────────────────────────────────────────┐
│ 内置库 (built-in/library/)                                   │
│   souls/  forms/  playbooks/                                 │
│   ├── 三毛 / 王小波 / 韩寒 / 猫笔刀 / 李笑来  (各三件套)      │
└──────────────────────────┬──────────────────────────────────┘
                           │ 引用
                           ↓
┌─────────────────────────────────────────────────────────────┐
│ 用户工作室 (studio root)                                      │
│                                                               │
│   ┌─────────┐   ┌─────────┐   ┌──────────┐   ┌────────┐    │
│   │ soul.md │   │ form.md │   │playbook.md│   │profile/│    │
│   └────┬────┘   └────┬────┘   └────┬─────┘   └───┬────┘    │
│        │             │              │              │         │
│        └─────┬───────┴──────────────┴──────────────┘         │
│              ↓                                                │
│        write 命令                                             │
│        ┌─────────────────────┐                               │
│        │ brainstorm阶段       │ ← 主题(外部输入)               │
│        │ ↓                   │ ← 素材文档(可选,外部输入)      │
│        │ skeleton.md         │                               │
│        │ ↓                   │                               │
│        │ draft 阶段           │                               │
│        │ ↓                   │                               │
│        │ form-check          │                               │
│        │ ↓                   │                               │
│        │ deai                │                               │
│        │ ↓                   │                               │
│        │ illustrate (可选)    │                               │
│        │ ↓                   │                               │
│        │ platform-output     │                               │
│        └─────────┬───────────┘                               │
│                  ↓                                            │
│            drafts/YYYY-MM-DD-xxx.md                          │
└─────────────────────────────────────────────────────────────┘
```

### 3.2 命令流(用户视角)

```
新人首次:
  /ghostink setup
    → 引导 15-20 min,产出 soul/form/playbook/profile

每次写稿:
  /ghostink write [topic]
    → 内部完成 brainstorm + draft + form-check + deai + 输出

提炼新参考:
  /ghostink distill <author>
    → 多轮 predict-then-compare,从历史文章产出三件套

去 AI 感(独立扫描):
  /ghostink deai [file]
    → 50+ 通用规则,任意文本可跑,不依赖 form

库管理:
  /ghostink library list / info / pick
```

---

## 4. 命令体系

### 4.1 一级命令(用户直接调,共 5 个)

| 命令 | 用途 | 何时用 |
|---|---|---|
| `/ghostink setup` | 首次/重置:建立创作者档案 | 新人,或想全部重做时 |
| `/ghostink write [topic]` | 写一篇文章 | 日常主路径 |
| `/ghostink distill <author>` | 从历史文章多轮提炼一位作者三件套 | 想为某作者建一套高精度 spec |
| `/ghostink deai [file]` | 任意中文文本去 AI 感扫描 | 任何来源文本通用,不依赖 form |
| `/ghostink library` | 库管理(子命令见下) | 想换文笔、看库存 |

### 4.2 library 子命令

| 子命令 | 用途 |
|---|---|
| `library list` | 列出内置 + 用户自拆库的所有 soul/form/playbook |
| `library info [name]` | 看某个 soul/form/playbook 的详情 |
| `library pick form [name]` | 把库里某个 form 设为当前在用的 form |
| `library pick soul [name]` | 把库里某个 soul 设为当前 soul |
| `library pick playbook [name]` | 加载某个 playbook 到当前 |

### 4.3 内部 primitives(不暴露给用户)

被主命令调用的子流程,在 `commands/_internal/`:

- `forge-soul` — 通过对话生成用户的 soul(setup 内部)
- `build-playbook` — 从已选 soul 推荐 + 用户增删(setup 内部)
- `brainstorm` — 出 skeleton(write 第 1 阶段)
- `draft` — 用 form 写文(write 第 2 阶段)
- `form-check` — 文笔合规审查(write 第 3 阶段 / `check` 命令)

### 4.4 边角入口(可选)

- `/ghostink check [file]` — 文笔合规审查(需 studio 有 form)
- `/ghostink illustrate [file]` — 配图(独立挂载,调外部图像生成 CLI)

**check 跟 deai 互补**:check 看像不像作家,deai 看像不像 AI。

---

## 5. 用户主流程

### 5.1 setup 流程(首次/重置)

**目标**:15-20 分钟向导,产出可立即用于写作的 soul/form/playbook/profile。

**流程**:

```
Step 0: 状态检测
  - 全空 → 全新 setup
  - 已部分填 → 询问追加 / 重写 / 跳已有

Step 1: 平台 + 内容类型 + 读者画像
  - 1.1 平台(公众号/X/微博/Substack/...)
  - 1.2 内容类型(生活散文/评论思辨/陪伴日常/知识工具/混合)— 先导问题
  - 1.3 读者画像(诉求/场景/关系三维)
  - → 写到 profile/identity.md

Step 2: 选模仿起点
  AskUserQuestion 4 分支:
    A) 内置整套(三件全抄一个)— 推荐新人
    B) 多作家组合(soul/form/playbook 分别选)
    C) 自拆参考(跑 distill 拆完再回来)
    D) 纯对话生成(不模仿任何人)

Step 3: 锐化 profile(对话式)
  - experiences / opinions 各 3-5 条种子
  - 跟已选 soul 拉差异(标 differentiator)
  - → 写到 profile/{experiences,opinions}.md

Step 4: 产出 soul.md
  - 路径 A:基于内置参考(复制 + profile 差异化补丁)
  - 路径 B:对话式 forge(无参考)

Step 5: 选 form
  - 推荐排序:同源 → 兼容 → 中性 → 不兼容(默认隐藏)

Step 5.5: 兼容性总览
  - Soul + Form + Playbook 三件兼容性快照,确认继续

Step 6: 建 playbook
  - 基于 soul.compat_playbook 推荐 plays
  - 用户多选 / 自定义新 play

Step 7: 完成
  - 摘要展示 + 下一步提示
```

### 5.2 write 流程

**目标**:从主题到可发布文稿,内部分阶段引导。

```
Step 0: 输入解析(主题 + 可选素材文件夹)
Step 1: 选 playbook(从 playbook.md 的 plays 中选)
Step 2: brainstorm → skeleton(对话锐化判断 + 选素材)
Step 3: draft(用 form 把 skeleton 写成文)
Step 4: form-check(自动出报告,不阈值通过)
Step 5: deai(自动出报告 with --with-form ON)
Step 6: illustrate(可选)
Step 7: platform-output(选发布平台 + 应用格式 + 写入 drafts/)
Step 8: profile 自动追加(扫本次会话提到的新经历/观点)
```

**关键设计**:
- 第 2-7 步连续运行,但每步之间有"暂停-用户确认"检查点
- 用户可从中段切入(基于已存在 _brainstorm 文件直接 draft)
- form-check 和 deai **只出报告,不阈值通过** — 决定权在用户

### 5.3 distill 流程

7 个 phase 流水线 + 多轮 predict-then-compare 迭代。详见 `commands/distill.md`。

```
Phase 0  Setup
Phase 1  Soul 提取(身份先于风格)
Phase 2  Form 提取(15 节,每条 trace 回 Soul)+ 扫 Appendix A 修辞技巧库
Phase 3  多轮 predict-then-compare 迭代(deep 模式必跑 3-5 轮)
Phase 4  Playbook 聚类 + frequency 统计
Phase 5  词频统计
Phase 6  Finalize + 兼容性标签自动填充
Phase 7  Profile bootstrap(若文章是用户自己的)
```

两种模式:`--quick`(5+ 篇 / ~10 min / 跳 Phase 3)/ `--deep`(默认,20+ 篇 / 30-60 min / 完整流程)。

### 5.4 deai 流程

5 层扫描 + 1 个活人感终审。任意中文文本通用,不依赖 form(`--with-form` opt-in)。
详见 `commands/deai.md`。

```
层 1  零容忍清单(46 条 AI 烂熟词替换表 + 禁用句式 + 禁用标点 + 假设性例子等)
层 2  句式与结构(句式重复 / 三段式 / 否定排比 / 整齐分段 / 戏剧性分段)
层 3  词频与措辞(万能词 / 同义词循环 / 系动词回避 / -ing 肤浅分析 / 过度限定 / 模糊归因 / 虚假范围 / 全员视角 / 填充短语)
层 4  内容深度(总结收尾 / 通用积极结论 / 提纲式"挑战与未来"/ 过度强调意义 / 媒体堆叠 / 协作交流痕迹 / 谄媚 / 知识截止免责声明)
─────────
层 5  活人感终审(主观,4 维:温度感 / 独特性 / 姿态 / 心流)
```

**禁用词必配替换方案**(硬约束):每条删/换建议都给具体替换方案。

### 5.5 library 操作流程

```
查看库存:
  /ghostink library list

看详情:
  /ghostink library info wangxiaobo

切换文笔:
  /ghostink library pick form luxun
  → 检查兼容性 → 警告(如果不兼容) → 替换 form.md
```

---

## 6. 文件结构

### 6.1 Skill 仓库(跟代码走)

```
ghostink/
├── DESIGN.md                       ← 本文档
├── README.md / README_EN.md
├── SKILL.md                        ← Claude 加载的入口
├── commands/                       ← 命令实现
│   ├── setup.md
│   ├── write.md
│   ├── distill.md
│   ├── deai.md
│   ├── library.md
│   ├── check.md
│   ├── illustrate.md
│   └── _internal/                  ← 内部 primitives(不暴露)
│       ├── forge-soul.md
│       ├── build-playbook.md
│       ├── brainstorm.md
│       ├── draft.md
│       └── form-check.md
├── built-in/
│   └── library/
│       ├── souls/                  ← 各内置作家 .md
│       ├── forms/
│       ├── playbooks/
│       ├── factions.md             ← 派系标签词表
│       └── README.md
└── templates/
    ├── soul.template.md
    ├── form.template.md            ← 含 Section 15 + Appendix A 修辞技巧库
    ├── playbook.template.md
    └── profile/
        ├── identity.md
        ├── experiences.md
        ├── opinions.md
        └── refs.md
```

### 6.2 用户工作室(跟用户内容走)

```
<studio root>/
├── soul.md                         ← 当前在用的 soul
├── form.md                         ← 当前在用的 form
├── playbook.md                     ← 你的打法集合
├── profile/
│   ├── identity.md
│   ├── experiences.md
│   ├── opinions.md
│   └── refs.md
├── library/                        ← 用户自拆的(可选)
│   ├── souls/
│   ├── forms/
│   └── playbooks/
└── drafts/
    ├── YYYY-MM-DD-xxx.md           ← 成品
    └── _brainstorm/                ← 中间产物 skeleton
```

### 6.3 Studio Discovery 规则

按以下顺序解析,首匹配为准:

1. `$GHOSTINK_STUDIO` 环境变量
2. cwd 包含 `soul.md` → cwd 即 studio root
3. 向上找祖先目录(不跨 `$HOME`)
4. 默认 cwd

---

## 7. 内置作家库

5 位中文作家,每位提供完整三件套(souls/forms/playbooks 同名)。

| 作家 | Slug | 灵魂派系 | 适用场景 |
|---|---|---|---|
| **三毛** | sanmao | 抒情派 + 陪伴派 | 个人散文 / 长情叙事 |
| **王小波** | wangxiaobo | 思辨派 + 辛辣派 | 杂文 / 思辨长文 |
| **韩寒** | hanhan | 辛辣派 + 思辨派 | 博客评论 / 短论 |
| **猫笔刀** | maobidao | 陪伴派 + 思辨派 | 日报型公众号 / 财经陪伴 |
| **李笑来** | lixiaolai | 布道派 + 教程派 | 概念布道 / 框架教学 |

新人 setup 时可以选一套整体抄作起点,以后自然演化(混搭 / 自拆 / 修订)。

每位作家三件套通过 `/ghostink distill` 流程产出。猫笔刀套件目前精度最高
(基于 35 篇真实文章的完整 7 phase 提炼,含 Phase 3 外部 predict-compare 验证)。

---

## 8. 兼容性机制

### 8.1 标签 schema(每个 library 文件 frontmatter)

```yaml
---
type: form                          # soul / form / playbook
name: wangxiaobo
display_name: 王小波
source: built-in                    # built-in / ai-distilled / user-distilled-quick / user-distilled-deep
factions: [反讽, 长句分析]
compat_soul:    [思辨派, 反讽派, 杂文型]
incompat_soul:  [陪伴派, 抒情派, 教程派]
compat_form:    [...]
incompat_form:  [...]
compat_playbook: [...]
description: |
  王小波的反讽文笔。短长句交错,理性中有黑色幽默。
---
```

### 8.2 兼容性"派系"标签词表

固定有限词表,避免标签发散(完整词表见 `built-in/library/factions.md`):

- **Soul 派系**:陪伴派 / 思辨派 / 教程派 / 抒情派 / 辛辣派 / 布道派
- **Form 派系**:口语化 / 书面化 / 反讽 / 抒情 / 短句轰炸 / 长句分析
- **Playbook 类型**:日报型 / 主题叙事型 / 热点评论型 / 教程型 / 杂文型 / 干货长论型

### 8.3 兼容性检查时机

只在 `library pick` 时检查,**软警告不强禁**:

```
你正在切换 form 到 [王小波]。
检测到兼容性问题:
  - 你当前的 soul 是 [陪伴派 + 抒情派]
  - 王小波 form 的 incompat_soul 包含 [陪伴派, 抒情派]

可能后果:文章可能感觉精神分裂——一边温柔陪伴,一边冷峻反讽。

继续吗?
  A) 继续(我知道自己在做什么)
  B) 取消
  C) 看看哪些 form 跟我的 soul 兼容
```

### 8.4 兼容性矩阵的维护

**不维护全量矩阵**,只在每个 library 文件上标自己的 compat/incompat。
在 pick 时即时计算冲突。避免组合爆炸。

---

## 9. 数据 Schema

完整模板见 `templates/{soul,form,playbook}.template.md`,真实样本见
`built-in/library/{souls,forms,playbooks}/*.md`。

**identity-first 硬约束**:Form / Playbook 每节末尾必须 `[traces to: Soul 0.X]`,
trace 不回的删。

### 9.1 soul.md 结构

```yaml
---
type: soul
name: maobidao
display_name: 猫笔刀
source: user-distilled-deep
factions: [陪伴派, 思辨派]
compat_form: [口语化, 短句轰炸, 反讽]
compat_playbook: [日报型, 热点评论型, 主题叙事型]
incompat_form: [书面化, 长句分析, 抒情]
incompat_playbook: [教程型]
description: |
  晚饭后蹲在小区跟你抽烟聊塘水的财经老兄弟
---

# Soul: 猫笔刀

## Uniqueness Statement
> 1-3 句锁差异化(强制反例对照"他不像 X 那样做 Y,他做 Z")— 全文导航灯

## 0.1 Reader Value
- 核心价值主张
- 价值类型表(含信任来源 + 文中证据 [#编号 §段])
- 取关测试

## 0.2 Positioning
- 角色 / 权力关系 / 人设悖论 / 自我定位信号
- 权威管理方式

## 0.3 Narrative Engine
- 说服栈排序 / 论断模式 / 错误处理 / 故事的角色
- 论证最小单元

## 0.4 Core Beliefs
- 信念 1-7 + 表现 + 证据 [#编号 §段]
- 信念之间冲突时的优先级链(具体优先级,不是抽象排序)
- 盲区(作者自己点出过的)

## 0.5 Trust Mechanics
- 行为表 + 频率 + 文中证据
- 脆弱部署(频率 + 触发场景 + 强势话题)
- 会瞬间摧毁信任的事(红线)

## 0.6 Emotional Contract
- 读者打开文章时期待的体验
- 作者绝不做的事(情感界限)
- 情感锚点角色(情境-角色对)
- 情感光谱与限制(可分常态 / 自传 / 年终 / 极端日多模式)
```

### 9.2 form.md 结构(15 节)

```yaml
---
type: form
name: maobidao
display_name: 猫笔刀
source: user-distilled-deep
factions: [口语化, 短句轰炸, 反讽]
compat_soul: [陪伴派, 思辨派]
compat_playbook: [日报型, 热点评论型, 主题叙事型]
incompat_soul: [书面化, 长句分析]
description: |
  口语化短句 + 财经术语精确 + 冷讽 2 级 + "……"段间转身
---

# Form: 猫笔刀

## 1. Persona Voice
- 写作人格 / 核心张力 / 脆弱维度
[traces to: Soul 0.X]

## 2. Reader Cognitive Model
- 默认读者智商假设
- 已知 / 未知边界表
- 论证强度档位
- 解释深度模式(白话翻译 / 类比 / 拆解)
[traces to: Soul 0.X]

## 3. Sentence Rhythm
- 句长统计(短%/中%/长% + 平均字数)
- 节奏型 + 节奏破坏点
[traces to: Soul 0.X]

## 4. Paragraph Rhythm
- 段长分布 + 段间转折手法表
- 段间留白逻辑 + 视觉密度
[traces to: Soul 0.X]

## 5. Vocabulary Fingerprint
### Must Use (≥10 条)
### Often Use (≥10 条)
### Never Use (≥15 条 — **每条必配替换方案**,硬约束)
### Forbidden (≥10 条 — 0 出现)
[traces to: Soul 0.X]

## 6. Address Conventions
- 自称 / 称读者 / 称他人 / 禁用称谓
[traces to: Soul 0.X]

## 7. Analogy Style
- 类比库来源(含真人真事来源)
- 好类比标准 + 类比密度 + 禁用类比模式
[traces to: Soul 0.X]

## 8. Context Projection
- 参考系 / 援引来源
- 讽刺锐度等级(0/1/2/3/3.5/4 级体系)
- 信息密度模式
[traces to: Soul 0.X]

## 9. Opening Patterns (≥4 种 + 频率% + 禁用开头)
[traces to: Soul 0.X]

## 10. Closing Patterns (≥3 种 + 频率% + 禁用结尾)
[traces to: Soul 0.X]

## 11. Emotional Expression Toolkit
- 7 种情绪表达手法 + 克制系数 + 触发后处理
[traces to: Soul 0.X]

## 12. Signature Expressions
- ≥8 条招牌 + ≥1 自创词
- 夸张式修辞(可选)
[traces to: Soul 0.X]

## 13. Punctuation & Layout
- 标点偏好表 + 排版习惯 + 视觉风格
[traces to: Soul 0.X]

## 14. Prohibition List
- ≥10 条总禁忌 + 每条 trace 回 Soul 哪节

## 15. Rhetorical Techniques(可选,从 Appendix A 16 条挑相关填)
- 4-8 条该作家明显使用的修辞技巧
- 挑不出 4 条不要硬凑(本身是有效观察)

## Appendix A:通用修辞技巧库(16 条)
- 扣主线句 / 论述中的故意打破 / 私人视角连接 / 对立面理解与承认 /
  句式断裂 / 回环呼应 / 谦逊铺垫 / 读者直呼 / 疑问句节奏 / 层层剥开 /
  英雄之旅叙事弧 / 反向论证 / 案例公正性 / 人物画像法 / 文化升维 / 升番逻辑
- 跨作家通用,distill Phase 2 时挑相关
```

### 9.3 playbook.md 结构

```yaml
---
type: playbook
name: maobidao
display_name: 猫笔刀
source: user-distilled-deep
plays: [日报型, 周末闲聊型, 极端日报型, 主题深挖型, 自传年终复盘型, 短公告型, 热点评论型]
description: |
  猫笔刀 7 种打法,frequency 加起来 ≈100%
---

# Playbook: 猫笔刀

> 顶部说明:Play 是分类指引,不是硬隔离。实际写作中多种 Play 经常混合
> (自传开场 + 日报中段)。判断 Play 类型看主导。

## Play: 日报型(核心)

- frequency: ~50%
- trigger: 工作日例行
- serves_value: [信息差, 决策辅助, 认知校准, 情感陪伴]
- emotional_baseline: 冷静日常 + 偶尔冷讽
- length: 1500-2500 字
- material_from: [当日新闻, 政策, 行业内幕]
- publish_cadence: 每工作日 22:00

### Structure
1. 开头:砸数据 + 总判断
2. 主体:大事件深聊(2-4 段)
3. "……" 转身
4. 多条新闻速串(数字编号 1-7)
5. 收尾:"就这些了"

### Core Characteristics (4-6 条)
1. 信息密度高 + 节奏轻快
2. 不是转述新闻,是"翻译 + 判断"
3. 行业内幕穿插
4. 允许穿插个人生活碎片
5. "……"是关键段间符
6. 粤语词 / 网络词高频

### Opening Patterns / Closing Patterns / Anti-Patterns
[...]

[traces to: Soul 0.1, 0.2, 0.3, 0.6]

## Play: <其他 6 种 Play 同结构>
...

---

## Cross-Play Prohibitions
N 条总禁忌 + 每条 trace 回 Soul

## Self-Check Checklist
N 条 yes/no 问题,可立即判断
```

### 9.4 profile/ 结构

`identity / experiences / opinions / refs` 4 个文件。详见 `templates/profile/`。

### 9.5 skeleton(write 中间产物)

```yaml
---
date: YYYY-MM-DD
topic: <主题>
play: <用了哪个 playbook>
status: brainstormed
form: <当前 form>
soul: <当前 soul>
---

## Form 关键约束(写 draft 时强制贯彻)
## Cross-Play Prohibitions(本 Play 必须避免)
## 核心判断
## 差异化切入
## 可用素材
## 大纲
## Self-Check Checklist
## 对话摘要
```

---

## 10. 未来工作

下面这些功能**有价值但不是当前优先**,推迟到积累更多使用数据后再做。

| 功能 | 说明 |
|---|---|
| **多 Soul 完整支持(平台绑定)** | 当前用单 soul,文件结构(`souls/` 目录)预留扩展空间 |
| `evolve / refresh` 从已发文章反向学习 | 用户先有 30+ 篇 ghostink 出的稿子才有数据 |
| Form 库扩充(再加 5-10 个作家) | 看用户实际拿哪个用得多 |
| 协作编辑(团队共用 IP) | 完全独立的子系统,工程量大 |
| 跨语言能力(英文/日文 form 库) | 当前先攻华语市场 |
| 自动 IP 演进追踪 | 需要长期数据 |

---

## 11. 设计决策记录

把关键判断记下来,方便后续维护时不忘当时为什么这么定。

### 11.1 为什么 Soul / Form / Playbook 分三件而不合一?

- **Soul 跨主题稳定,Form 可换,Playbook 随领域演进** — 三者生命周期不同
- 揉在一起会让"我换平台时要重新写整个 spec"成立,而实际只需要换 Form
- 三件套 + 兼容性标签能支持"同源全抄"和"混搭借用"两种使用模式

### 11.2 为什么内置三件套而不只是 form?

- form-only 的库会让用户以为可以自由组合,实际不行(Soul 和 Form 有兼容)
- 完整三件套让用户可以"全套抄一个作家",这是新人最低门槛的起点
- 分开提供 + 兼容性标签,给老手自由组合的空间

### 11.3 为什么 playbook 是软概念,playbook.md 是单一文件?

- playbook 数量通常 2-7 个(一个作者不会有十几种打法),装一个文件够
- 多文件会引入"哪些 play 在用"的状态管理问题

### 11.4 为什么用 soul/form/playbook 命名而非 layer 0/1/2/3?

- 数字层级暗示线性递进,但灵魂/形态/打法是平行关系
- "Layer 0 是 DNA"对新用户不友好
- 命名应该是名词不是序号

### 11.5 为什么不分新手 fast-track 和老手 normal track?

- 用户群体差异不足以撑两条流程的维护成本
- 单流程 + 可选步骤(每步有"跳过")已经能覆盖差异

### 11.6 为什么兼容性不强禁?

- 创作者偶尔会想"反差搭配"(陪伴 soul + 反讽 form 的怪异组合可能正是
  新风格的来源)
- 强禁会扼杀实验
- 警告 + 用户确认是足够的护栏

### 11.7 为什么 deai 和 form-check 都不设阈值自动通过?

- AI 感识别本身有主观成分,机器分数不能替代作者判断
- 强制阈值会让用户陷入"为分数改稿"而非"为读者改稿"
- 正确职责:**系统出报告,用户决定**

### 11.8 为什么 deai 是独立一级命令而不是 write 的子流程?

- deai 的规则是**通用的**(不依赖任何作家 form),适合任意中文文本
- 独立一级让"任意文本去 AI 感"成为开箱即用功能
- write Step 5 仍然内部调它,加 `--with-form` ON 走 form-aware 模式

### 11.9 为什么 distill 必须先做 Soul 再做 Form?

- 风格选择是身份的下游产物 — 一个作者用脏话不是"词汇偏好",是"定位
  是不加滤镜的朋友"
- 知道身份后,所有风格观察都能解释 — 直接跳到风格分析等于做无根之木
- identity-first 是 distill 全程的硬约束:Form/Playbook 每条规则必须
  trace 回 Soul

### 11.10 为什么选"中文 + 关键术语英文"而非全英文/双语?

- 主要用户群是中文创作者,英文文档让他们读自己的 `soul.md` 都困难
- 双语双份是经典反模式(永远有 source-of-truth 漂移)
- 关键术语(soul/form/playbook/profile/skeleton)保留英文,避免概念锚点
  中文化造成的分裂(soul 译"灵魂"还是"灵韵"?)
- frontmatter 字段名(`type:`、`compat_form:`)必须保持英文,这是结构化数据

**具体规则**:

| 内容 | 语言 |
|---|---|
| 命令文档 `commands/*.md` | 中文,保留英文术语锚点 |
| `SKILL.md` / `DESIGN.md` / `README.md` | 中文 |
| `README_EN.md` | 简介 + 指向 README.md(国际化占位) |
| Library 内容(soul/form/playbook 正文) | 中文 |
| frontmatter 字段名 | 英文 |
| 派系标签词表 | 中文(直接出现在用户警告里) |
| 内置作家名 | 中文(显示用) + 英文 slug(文件名) |

---

## 12. 实现路线

ghostink 维持"纯 markdown 指令"形态,**不引入任何代码/脚本/构建步骤**。
所有命令都是给 Claude 的 markdown 指令,通过 Claude 的工具(Read / Write /
Edit / Bash / AskUserQuestion)执行。

### 未来可能脚本化的点

| 点 | 当前方案 | 未来可优化 |
|---|---|---|
| 词频分析(distill Phase 5) | LLM 估算 | Python 脚本算 n-gram |
| library 索引 | LLM 读全部 frontmatter | 维护索引文件,加速 list/pick |
| 平台格式转换(公众号/X thread) | LLM 改写 markdown | 脚本化转换更稳定 |
| form-check 词表精扫 | LLM 扫描 | grep/正则,精度+速度更高 |
| deai 层 1.1 替换表扫描 | LLM 扫描 | grep 替换更快 |

判断标准:**当 LLM 行为出现稳定性问题或经济性问题时再脚本化**,不为
提前优化引入维护负担。

---

*本文档随架构调整同步更新,保持与代码同步。*
