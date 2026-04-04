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

### 2. 初始化你的风格

准备 20+ 篇参考文章（markdown 或纯文本），然后：

```
/ghostink style-init path/to/your/articles/
```

Ghostink 会分多轮分析你的文章并生成风格规格书。大约需要 10-20 分钟。

### 3. 开始写作

```
/ghostink write
```

跟随引导流程：
1. **定题**——描述你的想法和核心观点
2. **审骨架**——检查生成的文章结构
3. **读初稿**——通读全文
4. **看审计**——查看自动风格审计报告
5. **改稿**——根据审计结果和你的判断修改
6. **排版**——选择发布平台，自动适配格式

### 4. 持续进化

发布后如果觉得哪里不对：

```
/ghostink style-evolve "上一篇的自嘲太重了，收一收"
```

或者把风格迁移到新领域：

```
/ghostink style-evolve "把这套风格适配到 AI 科技方向，保留口语化和自嘲"
```

## 命令一览

| 命令 | 功能 |
|------|------|
| `/ghostink write` | 完整创作流程：想法 → 骨架 → 初稿 → 去AI味 → 配图 → 排版 |
| `/ghostink check [文件]` | 对任意文本做风格审计，输出合规报告 |
| `/ghostink deai [文件]` | 9 条规则的去 AI 味深度审查 |
| `/ghostink illustrate [文件]` | 为文章生成并插入配图 |
| `/ghostink style-init [目录]` | 从参考文章生成风格规格书 |
| `/ghostink style-evolve` | 根据反馈或新方向更新规格书 |

## 目录结构

Ghostink 在你项目的 `studio/` 目录下读写文件：

```
你的项目/
  └── studio/
      ├── style_spec.md              ← 风格规格书（自动生成）
      ├── style_init_log.md          ← 风格分析日志
      ├── author_profile/
      │   ├── identity.md            ← 你是谁
      │   ├── experiences.md         ← 个人经历（随写作自然增长）
      │   ├── opinions.md            ← 你的公开立场
      │   └── references.md          ← 你常引用的人物、书籍、文化符号
      ├── reference_articles/        ← 参考文章（用于风格学习）
      └── drafts/                    ← 成品输出目录
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

`/ghostink illustrate` 在没有生图工具的情况下仍然可用——它会把 prompt 文件保存到 `studio/drafts/imgs/prompts/`，你可以把内容复制到任何生图工具中使用（Midjourney、DALL-E 网页版、ComfyUI 等）。

## 许可

MIT
