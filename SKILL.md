# Ghostink

A style-driven writing system that learns your voice and writes like you.

Ghostink separates **what you think** (human) from **how it's written** (AI). You provide ideas and experiences; Ghostink handles structure, style, and formatting — all guided by a style spec extracted from your own writing.

## Available Commands

| Command | Description |
|---------|-------------|
| `/ghostink write` | Create an article through a guided multi-step workflow |
| `/ghostink check` | Audit any text against your style spec |
| `/ghostink deai` | Remove AI-flavor — Ghostink's signature de-AI review |
| `/ghostink illustrate` | Generate article illustrations with configurable image APIs |
| `/ghostink style-init` | Initialize a style spec from reference articles |
| `/ghostink style-evolve` | Update your style spec based on feedback |

## How It Works

```
You write ideas ──→ Ghostink structures & styles ──→ Article in your voice
                         ↑                              ↓
                    style_spec.md              feedback → evolve spec
```

### Core Concepts

**Style Spec** (`style_spec.md`): A detailed rulebook extracted from your writing — sentence rhythm, vocabulary fingerprint, emotional patterns, structural templates, and a prohibition list. It has three layers:
- **Layer 1 — Voice Core**: Sentence patterns, tone, humor style, prohibitions (portable across topics)
- **Layer 2 — Domain Adaptation**: Terminology handling, article types, data presentation (topic-specific)
- **Layer 3 — Platform Adaptation**: Formatting rules per publishing channel (platform-specific)

**Author Profile** (`author_profile/`): Background knowledge about you — experiences, opinions, references. Grows naturally as you write. AI uses this to ground narratives in real details instead of fabricating.

**Article Types**: Each style spec defines 2-4 article types with distinct structural templates. The writing workflow selects the appropriate type before generating.

## Directory Convention

Ghostink looks for files in a `studio/` directory relative to your project root:

```
your-project/
  └── studio/
      ├── style_spec.md              ← your style rulebook
      ├── author_profile/
      │   ├── identity.md            ← who you are
      │   ├── experiences.md         ← life stories (numbered entries)
      │   ├── opinions.md            ← positions you've publicly taken
      │   └── references.md          ← people, books, games you often cite
      ├── reference_articles/        ← source articles for style learning
      └── drafts/                    ← output directory
```

If `studio/` doesn't exist, Ghostink will guide you through setup.

## First-Time Setup

1. Create a `studio/reference_articles/` directory and put 20+ articles that represent your desired writing style
2. Run `/ghostink style-init` to generate your style spec
3. Fill in `author_profile/identity.md` with basic info about yourself
4. Start writing with `/ghostink write`

## Command Details

Each command loads its detailed instructions from the `commands/` directory within this skill. When a command is invoked, read the corresponding file:

- `/ghostink write` → read `commands/write.md`
- `/ghostink check` → read `commands/check.md`
- `/ghostink deai` → read `commands/deai.md`
- `/ghostink illustrate` → read `commands/illustrate.md`
- `/ghostink style-init` → read `commands/style-init.md`
- `/ghostink style-evolve` → read `commands/style-evolve.md`

## Language

Ghostink works in any language. The style spec, prompts, and output language are determined by the reference articles provided during `/ghostink style-init`. If reference articles are in Chinese, the spec and output will be in Chinese. If in English, everything will be in English.

Respond to the user in the same language they use.
