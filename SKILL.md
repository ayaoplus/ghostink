# Ghostink

A style-driven writing system that learns your voice and writes like you.

Ghostink separates **what you think** (human) from **how it's written** (AI). You provide ideas and experiences; Ghostink handles structure, style, and formatting — all guided by a style spec extracted from your own writing.

## Available Commands

| Command | Description |
|---------|-------------|
| `/ghostink brainstorm` | Interactive thinking partner — turn a rough idea into a structured outline through dynamic Socratic dialogue |
| `/ghostink write` | Create an article through a guided multi-step workflow |
| `/ghostink check` | Audit any text against your style spec |
| `/ghostink deai` | Remove AI-flavor — Ghostink's signature de-AI review |
| `/ghostink illustrate` | Generate article illustrations with configurable image APIs |
| `/ghostink style-init` | Analyze a reference author — extract their DNA + style into a spec |
| `/ghostink style-forge` | Forge YOUR spec from analyzed references + your own identity |
| `/ghostink style-evolve` | Update your style spec based on feedback |
| `/ghostink profile-init` | Layered interactive setup of `author_profile/` — identity, experiences, opinions, references |

## How It Works

```
Analyze references ──→ Forge YOUR spec ──→ Write in your voice
   (style-init)         (style-forge)          (write)
        ↑                     ↑                    ↓
  reference authors    your identity +      feedback → evolve spec
                       borrowed traits
```

### Core Concepts

**Style Spec** (`style_spec.md`): A detailed rulebook that captures both WHO you are and HOW you write. It has four layers:
- **Layer 0 — Author DNA**: Why readers follow, author positioning, narrative engine, core beliefs, trust mechanics, emotional contract (the soul — portable across domains)
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
      ├── style_spec.md              ← YOUR style rulebook (output of style-forge)
      ├── author_profile/
      │   ├── identity.md            ← who you are
      │   ├── experiences.md         ← life stories (numbered entries)
      │   ├── opinions.md            ← positions you've publicly taken
      │   └── references.md          ← people, books, games you often cite
      ├── references/                ← analyzed reference authors
      │   ├── catblade/
      │   │   ├── style_spec.md      ← catblade's analyzed spec
      │   │   └── articles/          ← catblade's source articles
      │   └── [another_author]/
      │       ├── style_spec.md
      │       └── articles/
      └── drafts/                    ← output directory
```

If `studio/` doesn't exist, Ghostink will guide you through setup.

## First-Time Setup

1. Create `studio/references/[author]/articles/` for each reference author, put 20+ of their articles in each
2. Run `/ghostink style-init [author]` for each reference to analyze their DNA + style
3. Run `/ghostink style-forge` to create YOUR spec from references + your own identity
4. Start writing with `/ghostink write`

For single-reference use, you can skip step 3 — `style-init` can write directly to `studio/style_spec.md` if you're emulating rather than forging.

## Command Details

Each command loads its detailed instructions from the `commands/` directory within this skill. When a command is invoked, read the corresponding file:

- `/ghostink brainstorm` → read `commands/brainstorm.md`
- `/ghostink write` → read `commands/write.md`
- `/ghostink check` → read `commands/check.md`
- `/ghostink deai` → read `commands/deai.md`
- `/ghostink illustrate` → read `commands/illustrate.md`
- `/ghostink style-init` → read `commands/style-init.md`
- `/ghostink style-forge` → read `commands/style-forge.md`
- `/ghostink style-evolve` → read `commands/style-evolve.md`
- `/ghostink profile-init` → read `commands/profile-init.md`

## Language

Ghostink works in any language. The style spec, prompts, and output language are determined by the reference articles provided during `/ghostink style-init`. If reference articles are in Chinese, the spec and output will be in Chinese. If in English, everything will be in English.

Respond to the user in the same language they use.
