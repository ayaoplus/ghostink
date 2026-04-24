# Ghostink

> 像你自己一样写作——批量地。

Ghostink 是一个 [Claude Code](https://claude.ai/claude-code) 技能，它从你的历史文章中学习写作风格，帮你创作听起来真正像你的新内容。

它把**你想表达什么**（人的工作）和**怎么写出来**（AI 的工作）分离。你提供想法和经历，Ghostink 负责结构、风格和排版——一切都由从你自己文章中提取的风格规格书驱动。

**[English →](README.md)**

## 工作原理

```
你的历史文章 ──→ /style-init ──→ 风格规格书（你的声纹 DNA）
                                         ↓
你的想法 + 经历 ──→ /write ──→ 你的声音写出的文章
                                         ↓
                   /check ──→ 风格审计报告
                                         ↓
         你的反馈 ──→ /style-evolve ──→ 规格书持续进化
```

Ghostink 不只是"模仿某人风格"。它提取一份详细的**风格规格书**——涵盖句式节奏、词频指纹、情绪模式、结构模板和禁忌清单——然后在写作的每个阶段强制执行这些规则。

## 功能特性

- **多轮风格学习**：迭代分析 20-200+ 篇文章，逐轮提升提取精度
- **三层风格架构**：人格内核（跨领域通用）→ 领域适配（按主题定制）→ 平台适配（按渠道定制）
- **引导式创作流程**：从模糊想法到成品文章，每一步都有人工检查点
- **去 AI 味审查**：9 条规则的深度审查——句式重复检测、信息密度校验、万能比喻识别、情绪平线告警等。这是 Ghostink 的核心差异化功能
- **自动配图**：分析文章结构、生成图片 prompt，委托 [baoyu-imagine](https://github.com/JimLiu/baoyu-skills) 生成图片（支持 9 个 provider）。未安装生图工具时自动降级为纯 prompt 输出
- **风格审计**：自动检测风格违规词汇和结构问题
- **活的作者档案**：背景知识库随每次写作自然增长
- **多平台输出**：适配公众号、X/Twitter 长文、Twitter thread、纯 Markdown

## 快速开始

### 1. 安装

```bash
git clone https://github.com/[your-username]/ghostink.git ~/.claude/skills/ghostink
```

### 2. 选定（或创建）你的 studio root

**studio root** 是 ghostink 存放风格规格书、作者档案、草稿、参考作者的目录。它**不是**技能源码目录——源码装在 `~/.claude/skills/ghostink`，你的内容放在自己的写作目录里。

两种方式告诉 ghostink 你的 studio root 在哪（详见 [配置 Studio Root](#配置-studio-root)）：

```bash
# 方式 A —— cd 过去（Claude Code / Codex 最简单）
mkdir -p ~/writing/my-blog && cd ~/writing/my-blog

# 方式 B —— 设环境变量（全局 agent 没有明确 cwd 时必须这么做）
export GHOSTINK_STUDIO=~/writing/my-blog
```

### 3. 分析参考作者（可选但推荐）

把 20+ 篇你欣赏的作者的文章（包括你自己的历史作品）放进 `analyzed_authors/<名字>/articles/`，然后：

```
/ghostink style-init <名字>
```

Ghostink 会分多轮分析并在 `analyzed_authors/<名字>/style_spec.md` 产出该作者的风格规格书。大约需要 10-20 分钟。

想学多个作者就为每个跑一次。

### 4. 打造你自己的风格规格书

```
/ghostink style-forge
```

把分析过的参考作者 + 你自己的身份融合成 studio root 下的 `style_spec.md`。这是 ghostink 真正用来写作的那份 spec。

### 5. 种下你的作者档案

```
/ghostink profile-init
```

5-10 分钟的交互式引导，填充 `author_profile/` 下的身份、代表性经历、观点、文化坐标。不做这步，ghostink 就退化成"套着你风格壳子的通用 AI"——档案是它能用你真实人生做素材的关键。

### 6. 写稿

想法模糊需要梳理：

```
/ghostink brainstorm
```

动态的苏格拉底式对话，把模糊的想法打磨成结构化大纲。产物会被 `/write` 自动识别。

想法已经清晰：

```
/ghostink write
```

引导式流程：定题 → 骨架 → 初稿 → 自动风格审计 → 去 AI 味 → 可选配图 → 平台适配。`/write` 会自动检测 brainstorm 产物并询问是否基于它。

### 7. 持续进化

发布后如果觉得哪里不对：

```
/ghostink style-evolve "上一篇的自嘲太重了，收一收"
```

## 命令一览

| 命令 | 功能 |
|------|------|
| `/ghostink brainstorm` | 交互式思维伙伴——用动态苏格拉底对话把模糊想法打磨成结构化大纲 |
| `/ghostink write` | 完整创作流程：想法 → 骨架 → 初稿 → 去AI味 → 配图 → 排版 |
| `/ghostink check [文件]` | 对任意文本做风格审计，输出合规报告 |
| `/ghostink deai [文件]` | 9 条规则的去 AI 味深度审查 |
| `/ghostink illustrate [文件]` | 为文章生成并插入配图 |
| `/ghostink style-init [名字]` | 分析参考作者的文章，产出该作者的风格规格书 |
| `/ghostink style-forge` | 融合参考作者 + 你自己的身份，锻造你自己的 spec |
| `/ghostink style-evolve` | 根据反馈或新方向更新规格书 |
| `/ghostink profile-init` | 交互式填充 `author_profile/`（身份、经历、观点、引用坐标） |

## 目录结构

Ghostink 在一个 **studio root** 目录下读写所有文件。技能源码目录（`~/.claude/skills/ghostink`）和你的 studio root 是分开的——源码只放指令，你的内容放在 studio root。

```
<studio root>/                     ← 例如 ~/writing/my-blog
  ├── style_spec.md                ← 风格规格书（自动生成）
  ├── style_init_log.md            ← 风格分析日志
  ├── author_profile/
  │   ├── identity.md              ← 你是谁
  │   ├── experiences.md           ← 个人经历（随写作自然增长）
  │   ├── opinions.md              ← 你的公开立场
  │   └── refs.md                  ← 你常引用的人物、书籍、文化符号
  ├── analyzed_authors/            ← 参考作者的文章和 spec
  └── drafts/                      ← 成品输出目录
      └── _brainstorm/             ← brainstorm 产物
```

## 配置 Studio Root

Ghostink 按下面的顺序解析 studio root，找到第一个匹配就停：

1. **`$GHOSTINK_STUDIO` 环境变量** —— 如果设置了，那个路径就是 studio root
2. **`cwd` 下有 `style_spec.md`** —— 当前目录就是 studio root
3. **`cwd` 向上查找有 `style_spec.md` 的祖先目录**（不跨越 `$HOME`）—— 那个祖先就是 studio root
4. **都没命中** —— 用 `cwd` 作为 studio root（初始化场景）

### Claude Code / Codex 这类基于 cwd 的运行时

直接 `cd` 到你的写作目录，一旦 `style_spec.md` 存在，规则 2/3 就会自动识别。

```bash
cd ~/writing/my-blog
claude  # 或 codex
# 里面跑 /ghostink write —— 读写都相对 ~/writing/my-blog
```

如果你有多个 studio（不同博客、不同声音），每个开一个独立终端：

```bash
cd ~/writing/tech-blog   # studio A
cd ~/writing/personal    # studio B
```

### 没有明确 cwd 的全局 agent（OpenClaw 等）

编排型 agent 没有稳定的 cwd。在启动 agent 时注入 `GHOSTINK_STUDIO`，或在 shell profile 里设：

```bash
# 方式 A：写入 shell profile（~/.zshrc、~/.bashrc）
export GHOSTINK_STUDIO=~/writing/my-blog

# 方式 B：单次调用
GHOSTINK_STUDIO=~/writing/my-blog <agent-command>

# 方式 C：通过 agent 自己的 config / env 注入机制
# （各个 orchestrator 方式不同，查它自己的文档）
```

一旦 `$GHOSTINK_STUDIO` 设好，不管 agent 跑在哪个目录下，所有 ghostink 命令都会在这个路径下读写。

### 跨设备同步

Studio root 就是一个普通目录，`$GHOSTINK_STUDIO` 指哪就读哪：

- **iCloud / Dropbox / OneDrive**：`export GHOSTINK_STUDIO=~/Library/Mobile\ Documents/.../my-blog`
- **一个独立的 private git repo**：clone 到 `~/writing/my-blog`，正常 commit/push
- **网络盘**：同上，指向挂载点即可

Ghostink 不管底下是什么——它只读写标准文件。

### 最小 sanity 检查

设好环境变量（或 cd 过去）之后，确认解析正确：

```bash
# 如果 $GHOSTINK_STUDIO 已设，应该匹配它
echo "${GHOSTINK_STUDIO:-$(pwd)}"
```

## 风格规格书架构

规格书分三层，让你在换话题或换平台时不丢失声音：

| 层级 | 内容 | 何时改动 |
|------|------|----------|
| **Layer 1：人格内核** | 句式节奏、词频指纹、情绪模式、禁忌清单 | 极少——这就是你的声音 |
| **Layer 2：领域适配** | 文章类型、术语处理、数据呈现、主题规则 | 换内容方向时 |
| **Layer 3：平台适配** | 排版规则、篇幅约束、标题风格 | 换发布渠道时 |

## 设计哲学

1. **你的声音就是产品。** 规格书从你的写作中提取，没有通用的"好文章"模板。
2. **人提供意义，AI 提供技艺。** 你的想法、经历和观点不可替代。句式一致性和词汇合规可以自动化。
3. **风格是可度量的。** "写得随意一点"这种指令没用。Ghostink 用具体的、可检查的规则：词频表、句长分布、禁忌词列表。
4. **规格书会进化。** 你的声音不是静态的。写作 → 审计 → 迭代的闭环确保规格书始终与你想要的声音对齐。

## 环境要求

- [Claude Code](https://claude.ai/claude-code) CLI 或桌面端
- 20+ 篇参考文章用于风格初始化

## 配图功能设置（可选）

`/ghostink illustrate` 会分析文章并生成图片 prompt。实际的图片生成交给外部工具处理，Ghostink 自动检测可用环境：

| 优先级 | 运行时 | 支持的 Provider | 配置方式 |
|--------|--------|----------------|----------|
| 1（推荐） | [baoyu-imagine](https://github.com/JimLiu/baoyu-skills) | OpenAI、Google、Replicate、DashScope、MiniMax、Jimeng、Seedream 等 | 见下方 |
| 2 | MCP 生图工具 | 取决于你的 MCP 配置 | 在 Claude Code 设置中配置 |
| 3（兜底） | 纯 prompt 模式 | 无 | 无需配置——prompt 文件保存供手动使用 |

### 安装 baoyu-imagine

1. **安装 baoyu-skills**（包含 baoyu-imagine 和其他实用技能）：

```bash
git clone https://github.com/JimLiu/baoyu-skills.git ~/.claude/skills/baoyu-skills
cd ~/.claude/skills/baoyu-skills && ./setup
```

2. **配置生图 API key**（选一个 provider 即可）：

```bash
# OpenAI（DALL-E、gpt-image-1.5）
export OPENAI_API_KEY=sk-xxx

# Google（Gemini、Imagen）
export GOOGLE_API_KEY=xxx

# Replicate（NanoBanana、SDXL 等）
export REPLICATE_API_TOKEN=xxx

# 阿里 DashScope
export DASHSCOPE_API_KEY=xxx
```

把 export 语句加到 `~/.zshrc` 或 `~/.bashrc` 中以持久化。

3. **测试是否正常**：

```bash
bun ~/.claude/skills/baoyu-skills/skills/baoyu-imagine/scripts/main.ts \
  --prompt "A minimal illustration of a writing desk" \
  --image /tmp/test.png --quality 2k
```

高级配置（默认 provider、批量并发数、自定义模型等）可以创建 `~/.baoyu-skills/baoyu-imagine/EXTEND.md`——完整 YAML 格式参见 [baoyu-imagine 文档](https://github.com/JimLiu/baoyu-skills)。

### 不安装 baoyu-imagine 也能用

`/ghostink illustrate` 在没有生图工具的情况下仍然可用——它会把 prompt 文件保存到 `drafts/imgs/prompts/`（相对 studio root），你可以把内容复制到任何生图工具中使用（Midjourney、DALL-E 网页版、ComfyUI 等）。

## 许可

MIT
