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

### 2. Pick (or create) your studio root

A **studio root** is the directory where ghostink stores your style spec, author profile, drafts, and analyzed reference authors. It is **not** the skill source directory — it's *your* writing workspace.

Two ways to tell ghostink where your studio root is (see [Configuring the Studio Root](#configuring-the-studio-root) for details):

```bash
# Option A — cd into it (simplest for Claude Code / Codex)
mkdir -p ~/writing/my-blog && cd ~/writing/my-blog

# Option B — set an environment variable (required for global agents without a meaningful cwd)
export GHOSTINK_STUDIO=~/writing/my-blog
```

### 3. Analyze a reference author (optional but recommended)

Put 20+ articles by a writer whose style you admire (including your own past writing) in `analyzed_authors/<name>/articles/`, then:

```
/ghostink style-init <name>
```

Ghostink will analyze the articles across multiple rounds and produce a spec at `analyzed_authors/<name>/style_spec.md`. This takes 10-20 minutes.

Repeat for each reference you want to learn from.

### 4. Forge your own style spec

```
/ghostink style-forge
```

Blends your analyzed references + your own identity into `style_spec.md` at the studio root. This is the spec ghostink writes against.

### 5. Seed your author profile

```
/ghostink profile-init
```

Interactive 5-10 minute walkthrough that fills `author_profile/` with your identity, a few signature experiences, your opinions, and cultural references. Without this, ghostink degrades to "generic AI with your style spec" — the profile is what lets it write with your actual life as material.

### 6. Write

For a rough idea that needs shaping:

```
/ghostink brainstorm
```

Dynamic Socratic dialogue that turns a fuzzy topic into a structured outline. Produces an artifact that `/write` can consume directly.

For an idea you already have clear:

```
/ghostink write
```

Guided workflow: define → outline → draft → auto style-check → de-AI review → optional illustration → platform formatting. `/write` also detects `/brainstorm` artifacts automatically.

### 7. Evolve over time

After publishing, if something felt off:

```
/ghostink style-evolve "The self-deprecation was too heavy in that last piece"
```

## Commands

| Command | What it does |
|---------|-------------|
| `/ghostink brainstorm` | Interactive thinking partner — turn a rough idea into a structured outline through dynamic Socratic dialogue |
| `/ghostink write` | Full creation workflow: idea → outline → draft → de-AI → illustrate → format |
| `/ghostink check [file]` | Run style audit on any text, output compliance report |
| `/ghostink deai [file]` | Run the 9-rule de-AI review on any text |
| `/ghostink illustrate [file]` | Generate and insert illustrations for an article |
| `/ghostink style-init [name]` | Analyze a reference author's articles → produce their style spec |
| `/ghostink style-forge` | Blend analyzed references + your identity into YOUR style spec |
| `/ghostink style-evolve` | Update your spec based on feedback or new direction |
| `/ghostink profile-init` | Layered interactive setup of `author_profile/` (identity, experiences, opinions, refs) |

## Directory Structure

Ghostink reads from and writes to a **studio root** — a directory that holds everything ghostink needs. The source skill directory (`~/.claude/skills/ghostink`) is separate; it holds only the instructions. Your content lives in the studio root.

```
<studio root>/                     ← e.g., ~/writing/my-blog
  ├── style_spec.md                ← your style rulebook (auto-generated)
  ├── style_init_log.md            ← analysis log from style-init
  ├── author_profile/
  │   ├── identity.md              ← who you are
  │   ├── experiences.md           ← personal stories (grows over time)
  │   ├── opinions.md              ← your stated positions
  │   └── refs.md                  ← books, people, culture you cite
  ├── analyzed_authors/            ← source articles + specs for style learning
  └── drafts/                      ← where finished articles go
      └── _brainstorm/             ← brainstorm artifacts
```

## Configuring the Studio Root

Ghostink resolves the studio root in this order, stopping at the first match:

1. **`$GHOSTINK_STUDIO` environment variable** — if set, that path is the studio root
2. **`cwd` contains `style_spec.md`** — current directory is the studio root
3. **Ancestor of `cwd` contains `style_spec.md`** (stops at `$HOME`) — that ancestor is the studio root
4. **Otherwise** — `cwd` is treated as the studio root (initialization case)

### For Claude Code / Codex and other cwd-aware runtimes

Just `cd` to your writing directory. Rules 2/3 pick it up automatically once `style_spec.md` exists.

```bash
cd ~/writing/my-blog
claude  # or: codex
# inside, run /ghostink write — it'll read/write relative to ~/writing/my-blog
```

If you keep multiple studios (different blogs, different voices), open a separate terminal for each:

```bash
cd ~/writing/tech-blog   # studio A
cd ~/writing/personal    # studio B
```

### For global agents without a meaningful cwd (OpenClaw, etc.)

Orchestrator-style agents don't have a stable cwd. Inject `GHOSTINK_STUDIO` when you spawn the agent, or set it in your shell profile:

```bash
# Option A: shell profile (~/.zshrc, ~/.bashrc)
export GHOSTINK_STUDIO=~/writing/my-blog

# Option B: per-invocation
GHOSTINK_STUDIO=~/writing/my-blog <agent-command>

# Option C: pass via the agent's config / env injection mechanism
# (varies by orchestrator — consult its docs)
```

Once `$GHOSTINK_STUDIO` is set, every ghostink command reads and writes under that path regardless of where the agent is running.

### Syncing across machines

The studio root is just a directory — point `$GHOSTINK_STUDIO` at any location:

- **iCloud / Dropbox / OneDrive**: `export GHOSTINK_STUDIO=~/Library/Mobile\ Documents/.../my-blog`
- **A private git repo**: clone to `~/writing/my-blog`, commit/push as usual
- **A network drive**: same idea, just point at the mount point

Ghostink doesn't care what's underneath — it only reads/writes standard files.

### Minimal sanity check

Set the env var (or cd somewhere), then verify resolution:

```bash
# If $GHOSTINK_STUDIO is set, this should match it
echo "${GHOSTINK_STUDIO:-$(pwd)}"
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

If you don't install baoyu-imagine, `/ghostink illustrate` still works — it generates prompt files at `drafts/imgs/prompts/` (relative to your studio root) that you can copy into any image tool (Midjourney, DALL-E web UI, ComfyUI, etc.).

## License

MIT
