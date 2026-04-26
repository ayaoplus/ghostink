# Ghostink

> 风格驱动的写作系统:把"你思考什么"和"AI 怎么写"分开。

Ghostink 是一个 [Claude Code](https://claude.ai/claude-code) Skill,
为高频更新的中文新媒体创作者(公众号 / X / 微博)而生。它把作者的
**Soul(灵魂)/ Form(文笔)/ Playbook(打法)**三层分离,配合个人 **Profile**
(素材库)共同驱动写作——你提供想法和经历,Ghostink 处理结构、风格、格式。

## 核心理念:为什么三层分离

传统的"写作风格"工具把灵魂和形态揉在一起。Ghostink 的关键洞察是:

- **Soul**(灵魂):你是谁,为什么读者要追你——价值主张 / 定位 / 信念 / 信任机制 / 情感契约
- **Form**(文笔):字怎么摆——句式 / 词汇 / 开头结尾 / 节奏
- **Playbook**(打法):不同情境下用哪种文章类型(日报 / 教程 / 评论 / ...)

灵魂跨主题、跨平台稳定。文笔可以为不同场景换。打法是灵魂在不同情境下的固化版本。
**Profile**(身份 / 经历 / 观点 / 参照系)是与三者并行的素材输入。

这样设计的好处:你可以"借猫笔刀的灵魂 + 用王小波的文笔 + 走李笑来的打法",
混搭成你自己的声音——只要兼容性允许。

## 快速开始

```bash
# 1. 切到一个空目录(将作为你的 studio root)
cd ~/writing/my-blog

# 2. 跑 setup,15-20 分钟交互式向导
/ghostink setup

# 3. 写第一篇
/ghostink write "你的主题"

# 4. 库管理(换文笔 / 看库存 / 加新参考)
/ghostink library
```

## 五个核心概念

| 概念 | 是什么 | 文件 |
|---|---|---|
| **Soul**(灵魂) | 你是谁 | `soul.md` |
| **Form**(文笔) | 字怎么摆 | `form.md` |
| **Playbook**(打法) | 不同情境用哪种文章类型 | `playbook.md` |
| **Profile**(素材) | 你有什么 | `profile/` |
| **Skeleton**(骨架) | 单篇文章的角度/结构 | `drafts/_brainstorm/*` |

## 三个一级命令

### `/ghostink setup`
首次/重置:建立你的创作者档案。15-20 分钟交互向导,产出
soul / form / playbook / profile。

### `/ghostink write [topic]`
写一篇文章的主路径。内部依次走:选 playbook → brainstorm 出 skeleton →
draft → form check → deai 报告 → 平台输出。

### `/ghostink distill <author>`
从一位作者的历史文章**多轮提炼**出 soul / form / playbook 三件套
(默认 deep 模式 20+ 篇,加 `--quick` 走 5+ 篇)。ghostink 的核心提炼引擎。

### `/ghostink deai [file]`
任意中文文本去 AI 感扫描 + 修订建议。**不依赖任何作家 form**,通用 50+
规则覆盖 4 大类(零容忍清单 / 句式与结构 / 词频与措辞 / 内容深度)+ 1 个
活人感终审。每条 FAIL 都带具体替换方案。融合维基"AI 写作特征"24 种 +
卡兹克禁用词 + 内部规则。

### `/ghostink library`
库管理。子命令:
- `library list` — 列出内置 + 自拆的 soul/form/playbook
- `library info [name]` — 看某个的详情
- `library pick {soul/form/playbook} [name]` — 切换在用的


## 内置作家库

为开箱即用,Ghostink 内置 5 位中文作家的完整三件套:

| Slug | 显示名 | 灵魂派系 | 适用场景 |
|---|---|---|---|
| `sanmao` | 三毛 | 抒情派 + 陪伴派 | 个人散文 / 长情叙事 |
| `wangxiaobo` | 王小波 | 思辨派 + 辛辣派 | 杂文 / 思辨长文 |
| `hanhan` | 韩寒 | 辛辣派 + 思辨派 | 博客评论 / 短论 |
| `maobidao` | 猫笔刀 | 陪伴派 + 思辨派 | 日报型公众号 / 财经陪伴 |
| `lixiaolai` | 李笑来 | 布道派 + 教程派 | 概念布道 / 框架教学 |

新人 setup 时可以直接选一套抄起步,以后自然演化。

## 边角入口

| 命令 | 用途 |
|---|---|
| `/ghostink check [file]` | 任意文本 form 合规审查(**需 studio 有 form.md**) |
| `/ghostink illustrate [file]` | 配图(挂载式,调外部图像生成 CLI) |

> `check` 跟 `deai`(一级命令)互补:**check 看像不像作家,deai 看像不像 AI**。

## 派系标签 & 兼容性

Ghostink 通过派系标签管理 soul/form/playbook 之间的兼容性。完整词表见
`built-in/library/factions.md`。

兼容性是**软警告**——同源安静通过,不兼容也允许用户继续(创作者的
"反差搭配"实验有时候是新风格的来源)。`/ghostink library pick` 切换时
基于派系标签做检查。

## 用户工作室目录布局

```
<studio root>/
├── soul.md                 ← 当前在用的 soul
├── form.md                 ← 当前在用的 form
├── playbook.md             ← 你的打法集合
├── profile/                ← 素材库
│   ├── identity.md
│   ├── experiences.md
│   ├── opinions.md
│   └── refs.md
├── library/                ← 自拆的参考(可选)
│   ├── souls/
│   ├── forms/
│   └── playbooks/
└── drafts/
    ├── YYYY-MM-DD-<slug>.md     ← 成品
    └── _brainstorm/             ← 中间骨架
```

## Studio Discovery 规则

按以下顺序解析,首匹配为准:

1. `$GHOSTINK_STUDIO` 环境变量
2. cwd 含 `soul.md` → cwd 即 studio root
3. 向上找祖先目录(不跨 `$HOME`)
4. 否则 cwd 即 studio root(初始化场景)

## 未来工作

下面这些功能不是当前优先,未来视使用情况推进:

- 多 Soul 完整支持(同时绑定不同平台,如公众号 vs X 用不同灵魂)
- `evolve / refresh` 从已发文章反向学习
- 内置作家库扩充(再加 5-10 位中文作家)

完整设计与决策记录见 `DESIGN.md`。

## 贡献

欢迎用户拆好新的作家三件套提交回上游。提交规范见 `built-in/library/README.md`。

> English version: [README_EN.md](./README_EN.md)

## License

[MIT](./LICENSE)
