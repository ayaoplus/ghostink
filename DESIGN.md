# Ghostink 设计文档 v2

> 本文档是对 ghostink 整体架构的重新设计。基于"灵魂/形态/打法"三层分离,把当前 9 个分散命令收敛为 3 个一级命令。
>
> 状态:草案,待评审 → 拆任务 → 分步实现
>
> 创建日期:2026-04-24

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
- 反对"先把所有概念学完才能开始"——9 个命令各有职责的高心智负担
- 反对"分用户类型出 fast/normal track"——单一主流程 + 可选步比分叉好

---

## 2. 核心概念

整个系统建立在 **5 个概念** 上,每个概念有清晰职责、单一所有者、明确生命周期。

| 概念 | 是什么 | 跨文章稳定性 | 谁产 | 谁用 | 文件 |
|---|---|:---:|---|---|---|
| **Soul** | 我是谁:价值/定位/信念/契约/信任机制 | 高 | setup 一次产出 | brainstorm 选切入、检查灵魂契合度 | `soul.md` |
| **Form** | 字怎么摆:句式/词汇/开头结尾/节奏 | 中(可换) | setup pick 或 analyze 拆 | write 写稿、check 审稿 | `form.md` |
| **Playbook** | 怎么打这一类:文章类型们 | 中(随领域演进) | setup 建 + 用户增加 | write 选打法 | `playbook.md` |
| **Profile** | 我有什么:身份/经历/观点/参照系 | 高(增量长大) | setup 粗建 + write 自动追加 | brainstorm 选话题选素材 | `profile/*` |
| **Skeleton** | 这一篇的骨架:角度/结构/递进/收尾 | 单篇产物 | brainstorm 阶段产 | draft 阶段消费 | `drafts/_brainstorm/*` |

### 2.1 Soul vs Form 的根本差异

```
Soul 决定:为什么读者明天还想看你
Form 决定:这一段字怎么排
```

Soul 跨主题、跨平台几乎不变。Form 可以为不同场景换(同一个 Soul 的人在公众号写陪伴长文,在 X 写辛辣短句)。

### 2.2 Soul × Form × Playbook 的关系

不是三件正交的事,**它们之间有兼容约束**:

- 一个 Soul 兼容若干种 Form,但不是任意 Form 都兼容
- 一个 Soul 倾向某些 Playbook(陪伴 Soul 不太用"硬核教程"打法)
- 同源套件(同一个作者拆出来的三件套)兼容性最强

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
│        │ deai-pass           │                               │
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
    → 内部完成 brainstorm + draft + check + deai + 输出

库管理:
  /ghostink library list                  ← 看有什么
  /ghostink library info [name]           ← 看某个详细
  /ghostink library analyze [author]      ← 拆一个新参考
  /ghostink library pick [type] [name]    ← 切换 soul/form
```

---

## 4. 命令体系

### 4.1 一级命令(用户直接调,共 3 个)

| 命令 | 用途 | 何时用 |
|---|---|---|
| `/ghostink setup` | 首次/重置:建立创作者档案 | 新人,或想全部重做时 |
| `/ghostink write [topic]` | 写一篇文章 | 日常主路径 |
| `/ghostink library` | 库管理(子命令见下) | 想换文笔、加新参考、看库存 |

### 4.2 library 子命令

| 子命令 | 用途 |
|---|---|
| `library list` | 列出内置库 + 用户自拆库的所有 soul/form/playbook |
| `library info [name]` | 看某个 soul/form/playbook 的详情 |
| `library analyze [author] [--only=soul/form/playbook]` | 拆一个新参考,默认拆三件套,可指定只拆其一 |
| `library pick form [name]` | 把库里某个 form 设为当前在用的 form |
| `library pick soul [name]` | 把库里某个 soul 设为当前 soul(多 IP 时绑定平台) |
| `library pick playbook [name]` | 加载某个 playbook 到当前 |

### 4.3 内部 primitives(不暴露给用户)

下列原本是命令,重构后被收进 setup/write 内部,不再独立暴露:

- `forge-soul` — 通过对话生成用户的 soul(setup 内部)
- `forge-profile` — 通过对话粗建 profile(setup 内部)
- `build-playbook` — 从已选 soul 推荐 + 用户增删(setup 内部)
- `brainstorm` — 出 skeleton(write 第 1 阶段)
- `draft` — 用 form 写文(write 第 2 阶段)
- `form-check` — 文笔合规审查(write 第 3 阶段)
- `deai-pass` — 去 AI 感(write 第 4 阶段)

### 4.4 保留的独立可选命令

- `/ghostink illustrate` — 配图(独立挂载,write 流程内可选调用)

### 4.5 砍掉的命令

- ~~`style-init`~~ → 改名为 `library analyze`
- ~~`style-forge`~~ → 收进 setup
- ~~`style-evolve`~~ → 全部砍,功能拆解:
  - 反馈调整 → 融入 write 反馈循环
  - 领域迁移 → 用 playbook 替代
  - 从已发文章学习 → 二阶段做(改名 `refresh`)
- ~~`profile-init`~~ → 收进 setup,但保留单独入口供"profile 不够时补"
- ~~`brainstorm`~~ → 收进 write
- ~~`check`~~ → 收进 write,用户也可单独调 `library check [file]` 做独立审稿(可选)
- ~~`deai`~~ → 收进 write,同样保留独立入口供"任意文本去 AI"

### 4.6 命令收敛对照表

| 重构前(9 个) | 重构后位置 |
|---|---|
| style-init | `library analyze` |
| style-forge | setup 内部 |
| style-evolve | 砍 / 二阶段 |
| profile-init | setup 内部(单独入口保留) |
| brainstorm | write 内部 |
| write | 扩展为总入口 |
| check | write 内部(单独入口保留) |
| deai | write 内部(单独入口保留) |
| illustrate | 保留 |

最终用户面对的一级命令:**3 个主入口 + 4 个边角入口(library 子命令 + check/deai/illustrate)**。

---

## 5. 用户主流程

### 5.1 setup 流程(首次/重置)

**目标**:15-20 分钟向导,产出可立即用于写作的 soul/form/playbook/profile。

**流程**:

```
Step 0: 检测当前 studio 状态
  - 全空 → 全新 setup
  - 已部分填 → 询问追加 / 重写 / 跳已有

Step 1: 平台与读者
  - 你主要发哪些平台?(公众号 / X / 微博 / Substack / ...)
  - 读者画像(标签化,多选):
    身份维度: 同行专业 / 初学新人 / 跨界爱好 / 决策者
    场景维度: 通勤碎片 / 睡前深度 / 工作查询 / 周末长读
    关系维度: 陌生订阅 / 半熟社群 / 朋友圈
  - → 写到 profile/identity.md(标签 + 自由备注)

Step 2: 选模仿起点
  AskUserQuestion:
    A) 从内置库选一个完整套件(适合新人,选了直接全套抄)
       → 列出 5 个内置作家 + 一句简介
    B) 从内置库选多个组合(soul 选 X,form 选 Y,playbook 选 Z)
    C) 我有自己想拆的参考,先去拆(进入 library analyze 子流程,完成后回来)
    D) 不模仿任何人,直接对话生成(适合有强烈自我认知的老手)

Step 3: 锐化 profile(对话式)
  - 你做什么的?有什么不可替代的经历?
  - 有什么和大多数人不一样的观点?
  - 一边问,一边对照已选参考的 soul,用对比拉出差异
  - → 写到 profile/identity.md, opinions.md, experiences.md(种子条目 3-5 条)

Step 4: 产出 soul.md
  - 如果走 A:复制内置 soul,加你的 profile 差异化补丁
  - 如果走 B/C/D:对话式 forge(参考 library 里的 soul 作为对照菜单)
  - 显示给用户看,确认后写入

Step 5: 选 form
  AskUserQuestion(默认 = 同 Step 2 选的作者的 form):
    A) 用 [作者X] 的 form(默认,兼容你的 soul)
    B) 换一个 form(列出兼容的 form)
    C) 用一个不兼容的 form(警告:可能感觉违和)
  → 复制到 form.md

Step 6: 建 playbook
  - 基于已选 soul,从内置 playbook 里推荐 2-3 个 plays
  - 询问用户:这些 plays 你都要吗?要加自己的吗?
  - 用户可以从内置库里选 plays 加入,或描述新 play 让系统生成
  - → 写到 playbook.md

Step 7: 完成
  - 摘要展示:你的 soul / form / playbook / profile 状态
  - 提示:你现在可以 /ghostink write 了
```

**关键设计**:
- 用户全程感受到的是**一个连续向导**,不需要知道底层 forge-soul / build-playbook 等命令存在
- 每一步都有"跳过 / 以后再补"选项
- Step 2 的 D 路径(纯对话生成)给老手留口子

### 5.2 write 流程

**目标**:从主题到可发布文稿,内部分阶段引导。

```
Step 0: 输入
  /ghostink write "AI 编程对初级程序员"
  /ghostink write --material ./refs/  ← 可选,提供素材文档夹

Step 1: 选 playbook
  - 默认按主题特征推荐一个 play
  - 用户可改选其他 play

Step 2: brainstorm → skeleton
  - 引导对话(沿用当前 brainstorm 逻辑)
  - 锐化判断 + 挑战框架(基于 soul)
  - 选素材(从 profile 找)
  - 出大纲(按 playbook 模板填)
  - 写到 drafts/_brainstorm/YYYY-MM-DD-xxx.md
  - 用户确认骨架

Step 3: draft
  - 加载 form.md
  - 按 skeleton 出文
  - 输出给用户看

Step 4: form-check (自动)
  - 扫词表 / 句式 / 开头结尾合规
  - 报告 PASS/FAIL
  - 用户可"自动修"或针对性改

Step 5: deai-pass (自动出报告,用户决定)
  - 9 条规则扫描,生成报告(含分数 + 具体建议)
  - 不强制修复,不设阈值自动通过
  - 报告呈现给用户后,询问:全部修 / 选择性修 / 跳过
  - 最终决定权在用户

Step 6: illustrate (可选)
  - 询问要不要配图

Step 7: platform-output
  - 选发布平台
  - 应用对应格式规则
  - 写到 drafts/YYYY-MM-DD-xxx.md

Step 8: profile 自动追加
  - 扫描本次会话提到的新经历/观点,询问是否加进 profile
```

**关键设计**:
- 第 2-7 步连续运行,但每步之间有"暂停-用户确认"检查点(避免黑盒)
- 用户可以从中段切入(基于已存在的 _brainstorm 文件直接 draft)
- 每一阶段都明确读哪些文件(skeleton 阶段读 soul + playbook + profile,draft 阶段读 form)

### 5.3 library 操作流程

```
查看库存:
  /ghostink library list
  → 显示内置 + 用户自拆的所有 soul/form/playbook,带兼容标签

看详情:
  /ghostink library info wangxiaobo
  → 输出王小波的 soul/form/playbook 摘要

切换文笔:
  /ghostink library pick form luxun
  → 检查兼容性 → 警告(如果不兼容) → 替换 form.md
  → 提示:下次 write 会用这个 form

加新参考:
  /ghostink library analyze 鲁迅 --path ./articles/luxun/
  → 读文章 → 拆 soul + form + playbook
  → 写到 library/souls/luxun.md, library/forms/luxun.md, library/playbooks/luxun.md
  → 询问:要立刻 pick 这个 form 吗?

只拆一项:
  /ghostink library analyze X --only=form
  → 只拆 form,跳过 soul 和 playbook
```

---

## 6. 文件结构

### 6.1 Skill 仓库(跟代码走)

```
ghostink/
├── DESIGN.md                       ← 本文档
├── README.md / README_CN.md
├── SKILL.md                        ← 给 Claude 加载用的入口
├── commands/                       ← 命令实现
│   ├── setup.md
│   ├── write.md
│   ├── library.md
│   ├── illustrate.md
│   └── _internal/                  ← 内部 primitives(不直接暴露)
│       ├── forge-soul.md
│       ├── build-playbook.md
│       ├── brainstorm.md
│       ├── draft.md
│       ├── form-check.md
│       └── deai-pass.md
├── built-in/
│   └── library/
│       ├── souls/
│       │   ├── sanmao.md           三毛
│       │   ├── wangxiaobo.md       王小波
│       │   ├── hanhan.md           韩寒
│       │   ├── maobidao.md         猫笔刀
│       │   └── lixiaolai.md        李笑来
│       ├── forms/
│       │   └── (同名 5 个)
│       └── playbooks/
│           └── (同名 5 个)
├── templates/
│   ├── soul.template.md
│   ├── form.template.md
│   ├── playbook.template.md
│   └── profile/
│       ├── identity.md
│       ├── experiences.md
│       ├── opinions.md
│       └── refs.md
└── examples/                       ← (可选)端到端示例
```

### 6.2 用户工作室(跟用户内容走)

```
<studio root>/
├── soul.md                         ← 当前在用的 soul(单 IP 时)
│   或:
├── souls/                          ← 多 IP 时
│   ├── wechat.md
│   └── x.md
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
├── drafts/
│   ├── YYYY-MM-DD-xxx.md           ← 成品
│   └── _brainstorm/                ← 中间产物 skeleton
└── illustrate_config.md            ← (可选)配图设置
```

### 6.3 Studio Discovery 规则

延续当前(SKILL.md 已有的规则,改一处):

1. `$GHOSTINK_STUDIO` 环境变量
2. cwd 包含 `soul.md` 或 `souls/`(原来是 `style_spec.md`)
3. 向上找祖先目录(不跨 $HOME)
4. 默认 cwd

---

## 7. 内置作家库

### 7.1 选定的 5 个作家

| 作家 | 类型 | 价值 | 备注 |
|---|---|---|---|
| **三毛** | 散文/抒情 | 流浪叙事 + 情感浓度 | 灵魂强,form 不易脱离 |
| **王小波** | 杂文/思辨 | 反讽 + 理性 + 自由 | 灵魂强,form 不易脱离 |
| **韩寒** | 博客/辛辣 | 短句 + 反权威 + 即时性 | form 较通用 |
| **猫笔刀** | 财经/陪伴 | 陪伴式分析 + 务实 | 灵魂级偶像 |
| **李笑来** | 干货/布道 | 框架式输出 + 概念锤 | 灵魂级偶像 |

### 7.2 每个作家提供三件套

每人 3 个文件:`souls/X.md` + `forms/X.md` + `playbooks/X.md`,每个文件有 frontmatter 标注兼容性。

### 7.3 作家提炼工作量估算

每个作家:
- 收集 30+ 篇代表作品
- 走一遍 `library analyze` 的全套提取
- 人工校对 + 补充兼容性标签
- 估计 0.5-1 天/人

**总工作量:3-5 天的内容创作工作**(不是工程)。

### 7.4 v1 → v2 提炼策略

为了不卡住整体进度,内置库走**两阶段提炼**:

- **v1(本次重构内)**:由 AI 基于训练知识 + 抽样作品**直接产出初版**,标记为 `version: 1.0-draft`。这一版能让 setup 流程跑通,但精度可能不够。
- **v2(后续迭代)**:用户准备好每个作家 30+ 篇代表作,跑 `library analyze` 走完整提取流程,产出 `version: 2.0`。v2 替换 v1。

每个 library 文件 frontmatter 标注:

```yaml
version: 1.0-draft   # 或 2.0
source: ai-distilled # 或 user-analyzed
```

UI 上(`library list` / `info`)对 `1.0-draft` 加 ⚠️ 标注,提示用户"这是初版,精度待验证"。

### 7.5 后续扩展

二阶段可能加入的候选:
- 鲁迅(经典杂文)
- 槽边往事(连岳)
- 老六(罗永浩)
- 卡兹克(AI 内容)
- 宝玉(技术博主)

---

## 8. 兼容性机制

### 8.1 标签 schema(每个 library 文件 frontmatter)

```yaml
---
type: form                          # soul / form / playbook
name: wangxiaobo
display_name: 王小波
version: 1.0
source: built-in                    # built-in / user-analyzed
compat_soul:    [思辨派, 反讽派, 杂文型]
incompat_soul:  [陪伴派, 抒情派, 教程派]
compat_form:    [...]
incompat_form:  [...]
compat_playbook: [...]
description: |
  王小波的反讽文笔。短长句交错,理性中有黑色幽默。
---
```

### 8.2 兼容性"派系"标签词表(初版)

固定一个有限词表,避免标签发散:

**Soul 派系**:陪伴派 / 思辨派 / 教程派 / 抒情派 / 辛辣派 / 布道派

**Form 派系**:口语化 / 书面化 / 反讽 / 抒情 / 短句轰炸 / 长句分析

**Playbook 类型**:日报型 / 主题叙事型 / 热点评论型 / 教程型 / 杂文型 / 干货长论型

### 8.3 兼容性检查时机

只在 `library pick` 时检查,**软警告不强禁**:

```
你正在切换 form 到 [王小波]。
检测到兼容性问题:
  - 你当前的 soul 是 [陪伴派 + 抒情派]
  - 王小波 form 的 incompat_soul 包含 [陪伴派, 抒情派]

可能的后果:文章可能感觉精神分裂——一边是温柔陪伴
                              一边是冷峻反讽。

继续吗?
  A) 继续(我知道自己在做什么)
  B) 取消
  C) 看看哪些 form 跟我的 soul 兼容
```

### 8.4 兼容性矩阵的维护

**不维护全量矩阵**,只在每个 library 文件上标自己的 compat/incompat。在 pick 时即时计算冲突。

避免组合爆炸(N 个 soul × N 个 form × N 个 playbook)。

---

## 9. 数据 Schema

### 9.1 soul.md 结构

```yaml
---
type: soul
name: maobidao
version: 1.0
source: built-in
compat_form:    [口语化, 短句轰炸]
compat_playbook: [日报型, 热点评论型]
incompat_form:  [书面化, 长句分析]
factions:       [陪伴派, 务实派]
description: 一个有钱人的陪伴式财经写作
---

# Soul: 猫笔刀

## 0.1 Reader Value
- 核心价值主张
- 价值类型表
- 取关测试

## 0.2 Positioning
- 角色 / 权力关系 / 人设悖论 / 自我定位信号

## 0.3 Narrative Engine
- 说服栈排序
- 论断模式
- 错误处理
- 故事的角色

## 0.4 Core Beliefs
- 信念 1 + 表现
- 信念 2 + 表现
- ...
- 冲突时谁赢
- 盲区

## 0.5 Trust Mechanics
- 行为表
- 脆弱模式
- 破信任的红线

## 0.6 Emotional Contract
- 读者来求什么
- 永不做什么
- 情感锚点角色
- 情感范围
```

### 9.2 form.md 结构

```yaml
---
type: form
name: wangxiaobo
version: 1.0
source: built-in
compat_soul:    [思辨派, 反讽派]
compat_playbook: [杂文型, 长论型]
factions:       [反讽, 长句分析]
description: 王小波的反讽 + 理性文笔
---

# Form: 王小波

## Persona Voice
- 写作人格 1-2 句
- 核心张力
- 脆弱维度

## Sentence Rhythm
- 短句比例
- 节奏型
- 长句容忍度

## Vocabulary Fingerprint
### Must Use (口语化倾向)
| word | usage | freq |
### Never Use (书面化禁用)
| word |
### Forbidden (绝对 0 次)
- ...

## Opening Patterns
| 类型 | 频率 | 示例 |

## Closing Patterns
| 类型 | 适用类型 | 示例 |

## Prohibition List
- 排比 / 总结段 / 教化 / ...

## Emotional Expression Toolkit
| 情绪 | 表达手法 | 例句 |

## Signature Expressions
| 招牌表达 | 使用场景 | 频率 |
```

### 9.3 playbook.md 结构

```yaml
---
type: playbook
name: maobidao
version: 1.0
source: built-in
plays: [日报型, 热点评论型, 主题叙事型]
description: 猫笔刀常用三种打法
---

# Playbook: 猫笔刀

## Play: 日报型

trigger:        工作日例行,当天有大盘行情
serves_value:   [信息差, 情感陪伴]
emotional_baseline: 日常陪伴,克制
length:         800-1500
material_from:  [当日新闻, 即时反应]

structure:
  1. 开头:扫一眼今天主要事件(2-3 件)
  2. 主体:对每件给一句态度 + 短解释
  3. 收尾:一句感慨 / 给到明天的话
opening_patterns: [新闻扫读, 数据开场]
closing_patterns: [日常感慨, 明日预告]

## Play: 热点评论型
...

## Play: 主题叙事型
...
```

### 9.4 profile/ 结构

延续当前设计(identity / experiences / opinions / refs 4 个文件),无需重做。

### 9.5 skeleton(write 中间产物)

```yaml
---
date: YYYY-MM-DD
topic: <主题>
play: <用了哪个 playbook>
status: brainstormed
---

## 核心判断
## 差异化切入
## 可用素材
## 大纲
## 对话摘要
```

---

## 10. 二阶段功能(暂不实现)

下面这些功能**有价值但不是 MVP**,推迟到积累实际使用数据后再做。

| 功能 | 推迟原因 |
|---|---|
| **多 Soul 完整支持(平台绑定)** | v1 先用单 soul 验证主流程,文件结构预留扩展空间 |
| `evolve / refresh` 从已发文章反向学习 | 需要用户先有 30+ 篇 ghostink 出的稿子 |
| 内置库 v2(用户跑 analyze 重构) | v1 先用 AI 提炼初版,v2 跟随实际使用 |
| Form 库扩充(再加 5-10 个作家) | 看用户实际拿哪个用得多 |
| 协作编辑(团队共用 IP) | 完全独立的子系统,工程量大 |
| 跨语言能力(英文/日文 form 库) | 当前先攻华语市场 |
| 自动 IP 演进追踪 | 需要长期数据 |

---

## 11. 迁移工作清单

按依赖顺序排列。每项是一个独立任务,可以拆给不同时段做。

### 11.1 P0 - 架构骨架(必须先做)

- [ ] **T1. 重写 SKILL.md**:只讲 3 个一级命令 + 5 个核心概念,删除 9 命令列表
- [ ] **T2. 建新目录结构**:`built-in/library/{souls,forms,playbooks}/`、`commands/_internal/`、`templates/{soul,form,playbook}.template.md`
- [ ] **T3. 写 schema 模板文件**:soul.template.md、form.template.md、playbook.template.md

### 11.2 P1 - 命令重组

- [ ] **T4. setup 命令**:整合 profile-init + style-init + style-forge 的核心逻辑成单一向导
- [ ] **T5. write 命令重写**:把现有 brainstorm + write + check + deai 串成内部 7 步
- [ ] **T6. library 命令**:list / info / analyze / pick 子命令(analyze 复用现 style-init 逻辑)
- [ ] **T7. 砍掉 evolve**:删除 `commands/style-evolve.md`,在 README 注明二阶段
- [ ] **T8. _internal/ 内部命令**:把 brainstorm/check/deai/forge-soul 等移到内部目录,主命令调用

### 11.3 P2 - 内置库内容(创作工作)

- [ ] **T9. 三毛三件套**:soul + form + playbook + 兼容标签
- [ ] **T10. 王小波三件套**
- [ ] **T11. 韩寒三件套**
- [ ] **T12. 猫笔刀三件套**
- [ ] **T13. 李笑来三件套**

(每人独立,可并行)

### 11.4 P3 - 兼容性机制

- [ ] **T14. 派系标签词表**:固定 6+6+6 个标签,写到 SKILL.md
- [ ] **T15. pick 时的兼容性检查逻辑**:在 library 命令里实现
- [ ] **T16. setup 流程中的兼容性引导**:Step 5 选 form 时基于 soul 推荐

### 11.5 P4 - 文档与示例

- [ ] **T17. 重写 README.md / README_CN.md**:对齐新架构
- [ ] **T18. 写一个端到端 demo**:从 setup 到产出第一篇,放到 examples/
- [ ] **T19. 删除过时的 analyzed_authors/ 目录引用**:用 library/ 替代

### 11.6 任务依赖图

```
T1, T2, T3 (P0 骨架)
    ↓
T4, T5, T6 (P1 命令)  ←  T8 (内部命令拆分)
    ↓
T9-T13 (P2 内容,可并行)
    ↓
T14, T15, T16 (P3 兼容性)
    ↓
T17, T18, T19 (P4 文档收尾)
```

### 11.7 工作量估算

| 阶段 | 工作量 | 备注 |
|---|---|---|
| P0 | 0.5 天 | 文件结构 + 模板 |
| P1 | 2-3 天 | 命令重写,核心工程量 |
| P2 | 3-5 天 | 内容创作,可并行 |
| P3 | 1 天 | 兼容性机制 |
| P4 | 1 天 | 文档 |
| **合计** | **~7-10 天** | 加上 buffer |

---

## 12. 设计决策记录

为了后续维护时不忘记当时的判断,把关键决策记下来:

### 12.1 为什么 forge 不暴露,而 init 暴露?

- `init`(改名 analyze)对**别人**做,产物在 library,行为可重复且独立
- `forge` 对**自己**做,只在 setup 流程的中间产生,作为独立命令暴露会让用户疑惑"我已经有 soul 了为什么还要 forge"

### 12.2 为什么内置三件套而不只是 form?

- form-only 的库会让用户以为可以自由组合,实际不行(灵魂和形态有兼容)
- 完整三件套让用户可以"全套抄一个作家",这是新人最低门槛的起点
- 分开提供 + 兼容性标签,给老手自由组合的空间

### 12.3 为什么 playbook 是软概念,playbook.md 是单一文件?

- playbook 数量通常 2-4 个(一个作者不会有十几种打法),装一个文件够
- 多文件会引入"哪些 play 在用"的状态管理问题

### 12.4 为什么放弃 Layer 0/1/2/3 的命名?

- 数字层级暗示了线性递进,但灵魂/形态/打法是平行关系
- "Layer 0 是 DNA"的名字对新用户不友好
- 命名应该是名词不是序号(soul/form/playbook 比 layer 0/1/2 易记)

### 12.5 为什么不分新手 fast-track 和老手 normal track?

- 用户群体差异不足以撑两条流程的维护成本
- 单流程 + 可选步骤(每步有"跳过")已经能覆盖差异

### 12.6 兼容性为什么不强禁?

- 创作者偶尔会想"反差搭配"(陪伴 soul + 反讽 form 的怪异组合可能正是新风格的来源)
- 强禁会扼杀实验
- 警告 + 用户确认是足够的护栏

### 12.7 为什么 deai-pass 不设阈值自动通过?

- AI 感识别本身有主观成分,机器分数不能替代作者判断
- 强制阈值会让用户陷入"为分数改稿"而非"为读者改稿"
- 正确职责:**系统出报告,用户决定**

### 12.8 为什么选"中文 + 关键术语英文"而非全英文/双语?

- 主要用户群是中文创作者,英文文档让他们读自己的 `soul.md` 都困难
- 双语双份是经典反模式(永远有 source-of-truth 漂移)
- 关键术语(soul/form/playbook/profile/skeleton)保留英文,避免概念锚点中文化造成的分裂(如 soul 译"灵魂"还是"灵韵"?)
- frontmatter 的字段名(`type:`、`compat_form:`)必须保持英文,这是结构化数据,不能翻译

**具体规则**:
| 内容 | 语言 |
|---|---|
| 命令文档 `commands/*.md` | 中文,保留英文术语锚点 |
| `SKILL.md` / `DESIGN.md` / `README_CN.md` | 中文 |
| `README.md`(英文版) | 简介 + 指向 README_CN.md(国际化占位) |
| Library 内容(soul/form/playbook 正文) | 中文 |
| frontmatter 字段名 | 英文 |
| 派系标签词表 | 中文(直接出现在用户警告里) |
| 内置作家名 | 中文(显示用) + 英文 slug(文件名) |

### 12.9 为什么内置库走 v1 → v2 两阶段?

- v1 由 AI 直接产出,目的是**让 setup 流程能跑通**,不卡进度
- v2 由用户准备 30+ 篇代表作走 `library analyze`,精度上来后替换 v1
- 两阶段标注 `version` 字段,UI 上显式区分

---

## 13. 已确认的决策(评审后冻结)

| # | 议题 | 决策 |
|---|---|---|
| 1 | 多 Soul v1 是否实现 | **不做**,v2 实现;v1 文件结构预留扩展(`soul.md` 单文件,后续可加 `souls/` 目录) |
| 2 | identity.md 读者画像 | **细化为标签**,见 5.1 节 Step 1 的三维标签词表 |
| 3 | library analyze 文章数下限 | **完整模式 20+,quick 模式 5+**,quick 标记为 `version: 1.0-quick` |
| 4 | deai-pass 阈值 | **不设阈值**,只出报告,用户决定改不改 |
| 5 | examples/ 端到端 demo | **要做**,用猫笔刀全流程跑一篇示例 |
| 6 | 内置作家库精度 | **v1 由 AI 直接产出**,用户后续提供文章重训 v2 |
| 7 | 文档语言 | **中文 + 关键术语英文**,见 12.8 节细则 |

---

## 13.5 实现路线:纯 prompt(v1)

新架构维持当前 ghostink 的"纯 markdown 指令"形态,**不引入任何代码/脚本/构建步骤**。所有命令都是给 Claude 的 markdown 指令,通过 Claude 的工具(Read/Write/Edit/Bash/AskUserQuestion)执行。

### 未来可能脚本化的点(v2+ 考虑,v1 不做)

| 点 | 当前方案 | 未来可优化 |
|---|---|---|
| 词频分析(library analyze 阶段 5) | LLM 估算 | Python 脚本算 n-gram |
| library 索引 | LLM 读全部 frontmatter | 维护索引文件,加速 list/pick |
| 平台格式转换(公众号/X thread) | LLM 改写 markdown | 脚本化转换更稳定 |
| form-check 词表精扫 | LLM 扫描 | grep/正则,精度+速度更高 |

判断标准:**当 LLM 行为出现稳定性问题或经济性问题时再脚本化**,不为提前优化引入维护负担。

---

## 14. 下一步

1. ✅ 设计文档冻结(本次更新后)
2. 把第 11 节的 19 个任务拆成 `tasks/` 目录下的独立执行文档(每个含目标/输入/输出/验收)
3. 从 P0 开始分步实现,每完成一个任务勾选 11 节清单

---

*本文档遵循"先对齐再动手"原则。任何对架构的实质性修改都应反映回本文档,保持文档与代码同步。*
