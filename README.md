# Ghostink

> Write like yourself — at scale.

Ghostink is a [Claude Code](https://claude.ai/claude-code) skill that learns your writing style from existing articles and helps you create new content that sounds authentically *you*.

It separates **what you think** (human) from **how it's written** (AI). You provide ideas; Ghostink handles structure, voice, and formatting — all guided by a style spec extracted from your own writing.

**[中文文档 →](README_CN.md)**

## How It Works

```
Your reference articles ──→ /style-init ──→ Style Spec (your voice DNA)
                                                  ↓
Your ideas + experiences ──→ /write ──→ Article in your voice
                                                  ↓
                            /check ──→ Style audit report
                                                  ↓
              Your feedback ──→ /style-evolve ──→ Better spec over time
```

Ghostink doesn't just "write in the style of." It extracts a detailed **Style Spec** — a rulebook covering sentence rhythm, vocabulary fingerprint, emotional patterns, structural templates, and prohibitions — then enforces these rules at every stage of writing.

## Features

- **Multi-round style learning**: Iteratively analyzes 20-200+ articles to extract writing patterns with increasing precision
- **Three-layer style spec**: Voice core (portable) → Domain rules (topic-specific) → Platform formatting (channel-specific)
- **Guided creation workflow**: From rough idea to polished article through structured steps with human checkpoints
- **De-AI review**: 9-rule audit that catches the subtle patterns making AI text feel "off" — sentence pattern repetition, information density padding, generic metaphors, emotional flatlines, and more. This is Ghostink's signature feature
- **Auto illustration**: Analyze article structure, generate image prompts, and delegate to [baoyu-imagine](https://github.com/JimLiu/baoyu-skills) for image generation (9 providers supported). Works in prompt-only mode if no image tool is installed
- **Style audit**: Automated compliance checking against your spec
- **Living author profile**: Background knowledge base that grows with each writing session
- **Multi-platform output**: Format for WeChat, X/Twitter, threads, or plain markdown

## Quick Start

### 1. Install

```bash
git clone https://github.com/[your-username]/ghostink.git ~/.claude/skills/ghostink
```

### 2. Initialize your style

Put 20+ reference articles (markdown or text files) in a directory, then:

```
/ghostink style-init path/to/your/articles/
```

Ghostink will analyze your articles across multiple rounds and generate a Style Spec. This takes 10-20 minutes. Grab a coffee.

### 3. Start writing

```
/ghostink write
```

Follow the guided workflow:
1. **Define** your topic and core argument
2. **Review** the generated outline
3. **Read** the draft
4. **Check** the auto-audit report
5. **Revise** until satisfied
6. **Format** for your publishing platform

### 4. Evolve over time

After publishing, if something felt off:

```
/ghostink style-evolve "The self-deprecation was too heavy in that last piece"
```

Or pivot your style to a new domain:

```
/ghostink style-evolve "Adapt this for AI/tech content, keep the conversational tone"
```

## Commands

| Command | What it does |
|---------|-------------|
| `/ghostink write` | Full creation workflow: idea → outline → draft → de-AI → illustrate → format |
| `/ghostink check [file]` | Run style audit on any text, output compliance report |
| `/ghostink deai [file]` | Run the 9-rule de-AI review on any text |
| `/ghostink illustrate [file]` | Generate and insert illustrations for an article |
| `/ghostink style-init [dir]` | Generate a Style Spec from reference articles |
| `/ghostink style-evolve` | Update your spec based on feedback or new direction |

## Directory Structure

Ghostink reads from and writes to a `studio/` directory in your project:

```
your-project/
  └── studio/
      ├── style_spec.md              ← your style rulebook (auto-generated)
      ├── style_init_log.md          ← analysis log from style-init
      ├── author_profile/
      │   ├── identity.md            ← who you are
      │   ├── experiences.md         ← personal stories (grows over time)
      │   ├── opinions.md            ← your stated positions
      │   └── references.md          ← books, people, culture you cite
      ├── reference_articles/        ← source articles for style learning
      └── drafts/                    ← where finished articles go
```

## Style Spec Architecture

The Style Spec has three layers, allowing you to change topics or platforms without losing your voice:

| Layer | Contains | When to change |
|-------|----------|---------------|
| **Layer 1: Voice Core** | Sentence rhythm, vocabulary fingerprint, emotional patterns, prohibitions | Rarely — this IS your voice |
| **Layer 2: Domain** | Article types, terminology handling, data presentation, topic-specific rules | When changing content areas |
| **Layer 3: Platform** | Formatting, length limits, title conventions | When publishing to a new channel |

## Philosophy

Most AI writing tools try to be everything to everyone. Ghostink takes the opposite approach:

1. **Your voice is the product.** The spec is extracted from YOUR writing (or writing you admire). There's no generic "engaging writing" template.
2. **Humans provide meaning, AI provides craft.** Your ideas, experiences, and opinions are irreplaceable. Sentence structure and vocabulary consistency can be automated.
3. **Style is measurable.** Vague instructions like "write casually" don't work. Ghostink uses specific, checkable rules: word frequency tables, sentence length distributions, explicit prohibition lists.
4. **Specs evolve.** Your voice isn't static. The iterative feedback loop (write → check → evolve) ensures the spec stays aligned with how you actually want to sound.

## Requirements

- [Claude Code](https://claude.ai/claude-code) CLI or desktop app
- 20+ reference articles for style initialization

## Illustration Setup (Optional)

Ghostink's `/illustrate` command analyzes your article and generates image prompts. The actual image generation is handled by external tools. Ghostink auto-detects what's available:

| Priority | Runtime | Providers | Setup |
|----------|---------|-----------|-------|
| 1 (recommended) | [baoyu-imagine](https://github.com/JimLiu/baoyu-skills) | OpenAI, Google, Replicate, DashScope, MiniMax, Jimeng, Seedream + more | See below |
| 2 | MCP image tools | Depends on your MCP config | Configure in Claude Code settings |
| 3 (fallback) | Prompt-only | N/A | No setup needed — prompts saved for manual use |

### Setting up baoyu-imagine

1. **Install baoyu-skills** (includes baoyu-imagine and many other useful skills):

```bash
git clone https://github.com/JimLiu/baoyu-skills.git ~/.claude/skills/baoyu-skills
cd ~/.claude/skills/baoyu-skills && ./setup
```

2. **Configure your image API key** (pick one provider):

```bash
# OpenAI (DALL-E, gpt-image-1.5)
export OPENAI_API_KEY=sk-xxx

# Google (Gemini, Imagen)
export GOOGLE_API_KEY=xxx

# Replicate (NanoBanana, SDXL, etc.)
export REPLICATE_API_TOKEN=xxx

# Alibaba DashScope
export DASHSCOPE_API_KEY=xxx
```

Add the export to your `~/.zshrc` or `~/.bashrc` to persist across sessions.

3. **Test it works**:

```bash
bun ~/.claude/skills/baoyu-skills/skills/baoyu-imagine/scripts/main.ts \
  --prompt "A minimal illustration of a writing desk" \
  --image /tmp/test.png --quality 2k
```

For advanced configuration (default provider, batch limits, custom models), create `~/.baoyu-skills/baoyu-imagine/EXTEND.md` — see [baoyu-imagine docs](https://github.com/JimLiu/baoyu-skills) for the full YAML schema.

### Without baoyu-imagine

If you don't install baoyu-imagine, `/ghostink illustrate` still works — it generates prompt files at `studio/drafts/imgs/prompts/` that you can copy into any image tool (Midjourney, DALL-E web UI, ComfyUI, etc.).

## License

MIT
