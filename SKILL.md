# Ghostink

> 风格驱动的写作系统:把"你思考什么"(人)和"AI 怎么写"(机器)分开。
>
> 核心理念是把作者的 **Soul(灵魂)/ Form(文笔)/ Playbook(打法)** 三层分离,
> 配合个人 **Profile(素材库)** 共同驱动写作。

## 快速开始

```
1. /ghostink setup            建立你的创作者档案(soul / form / playbook / profile)
2. /ghostink write [topic]    写一篇文章
3. /ghostink library          换文笔、加新参考、看库存
```

新人按这 3 步即可上手。完整示例见 `examples/maobidao-demo/`。

## 五个核心概念

| 概念 | 是什么 | 文件 |
|---|---|---|
| **Soul**(灵魂) | 你是谁:价值主张 / 定位 / 信念 / 信任机制 / 情感契约 | `soul.md` |
| **Form**(文笔) | 字怎么摆:句式 / 词汇 / 开头结尾 / 节奏 | `form.md` |
| **Playbook**(打法) | 不同情境下用哪种文章类型(日报/教程/评论/...) | `playbook.md` |
| **Profile**(素材库) | 你有什么:身份 / 经历 / 观点 / 参照系 | `profile/` |
| **Skeleton**(骨架) | 单篇文章的角度/结构/递进/收尾(write 中间产物) | `drafts/_brainstorm/*` |

Soul 跨主题、跨平台稳定;Form 可换;Playbook 是 Soul 在不同情境下的固化打法。Profile 是与三者并行的素材输入。

## 三个一级命令

### `/ghostink setup`
首次使用或全部重做。15-20 分钟交互向导,产出 soul / form / playbook / profile。
详见 `commands/setup.md`。

### `/ghostink write [topic]`
写一篇文章的主路径。内部依次走:选 playbook → brainstorm 出 skeleton → draft → form check → deai 报告 → 平台输出。
详见 `commands/write.md`。

### `/ghostink library`
库管理。子命令:

| 子命令 | 用途 |
|---|---|
| `library list` | 列出内置 + 用户自拆的 soul/form/playbook,标版本徽章 |
| `library info [name]` | 看某个资源的详情 |
| `library distill [author] [--quick\|--deep] [--only=soul/form/playbook]` | 从历史文章多轮提炼一个参考(默认 deep,30-60 min;`--quick` 5+ 篇 ~10 min) |
| `library pick {soul/form/playbook} [name]` | 切换当前在用的 soul/form/playbook |

详见 `commands/library.md`。

## 边角入口(可选)

| 命令 | 用途 |
|---|---|
| `/ghostink check [file]` | 任意文本审查(form 合规) |
| `/ghostink deai [file]` | 任意文本去 AI 感(出报告,不强制改) |
| `/ghostink illustrate [file]` | 配图(挂载式,调外部图像生成 CLI) |

## 派系标签

ghostink 通过派系标签管理 soul/form/playbook 之间的兼容性。完整词表见 `built-in/library/factions.md`,所有 library 文件的 `factions` / `compat_*` / `incompat_*` 字段只能从该词表选。

## Studio Discovery 规则

ghostink 在一个 **studio root** 工作——含 soul.md、profile/、drafts/ 的目录。所有路径相对 studio root。

按以下顺序解析,首匹配为准:

1. `$GHOSTINK_STUDIO` 环境变量
2. cwd 含 `soul.md` 或 `souls/` 目录 → cwd 即 studio root
3. 从 cwd 向上找(不跨 `$HOME`),首个含 `soul.md` 或 `souls/` 的祖先即 studio root
4. 否则(初始化场景)→ cwd 即 studio root,新文件创建在此

## 用户工作室目录布局

```
<studio root>/
├── soul.md                       ← 当前在用的 soul
├── form.md                       ← 当前在用的 form
├── playbook.md                   ← 你的打法集合
├── profile/                      ← 素材库
│   ├── identity.md
│   ├── experiences.md
│   ├── opinions.md
│   └── refs.md
├── library/                      ← 用户自拆的参考(可选)
│   ├── souls/
│   ├── forms/
│   └── playbooks/
├── drafts/
│   ├── YYYY-MM-DD-<slug>.md      ← 成品
│   └── _brainstorm/              ← 中间骨架
└── illustrate_config.md          ← (可选)配图设置
```

## 命令文件位置

| 命令 | 加载文件 |
|---|---|
| `/ghostink setup` | `commands/setup.md` |
| `/ghostink write` | `commands/write.md` |
| `/ghostink library` | `commands/library.md` |
| `/ghostink check` | `commands/check.md`(thin wrapper → `_internal/form-check.md`) |
| `/ghostink deai` | `commands/deai.md`(thin wrapper → `_internal/deai-pass.md`) |
| `/ghostink illustrate` | `commands/illustrate.md` |

`commands/_internal/` 下的子命令由主命令调用,不直接暴露给用户。

## 语言

ghostink 主要服务中文创作者。命令文档、库内容(soul/form/playbook 正文)使用中文,关键术语锚点(soul/form/playbook/profile/skeleton)保留英文。frontmatter 字段名(`type:`、`compat_form:` 等)必须英文。

英文版见 `README_EN.md`。

## 设计文档

完整架构与决策见 `DESIGN.md`,任务清单见 `tasks/`。
