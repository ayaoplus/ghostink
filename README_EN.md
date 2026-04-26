# Ghostink

> Style-driven writing system: separate **what you think** from **how it's written**.

Ghostink is a [Claude Code](https://claude.ai/claude-code) skill built for
high-frequency Chinese new-media writers (WeChat / X / Weibo). It splits a
writer's **Soul** (identity), **Form** (prose style), and **Playbook**
(article-type templates) into three independent layers, plus a **Profile**
(personal material library), so you can mix and match like CSS for prose.

> **中文读者**: 完整文档见 [README.md](./README.md)，中文版为主文档。

## Quick Start

```bash
cd ~/writing/my-blog

# Core flow:
/ghostink setup            # 15-20 min interactive wizard
/ghostink write "topic"    # write an article

# Other top-level commands:
/ghostink distill <author> # multi-round distill an author's trio from articles
/ghostink deai [file]      # de-AI any Chinese text (50+ rules, no form needed)
/ghostink library          # manage references / switch styles
```

## Five Top-Level Commands

- `/ghostink setup` — interactive wizard, produce soul / form / playbook / profile
- `/ghostink write [topic]` — main writing flow (brainstorm → draft → form-check → deai → output)
- `/ghostink distill <author>` — distill an author's trio from historical articles (deep mode = 20+ articles, 7 phases including multi-round predict-then-compare)
- `/ghostink deai [file]` — de-AI scanner with 50+ universal rules, doesn't need form
- `/ghostink library` — list / info / pick across built-in + user-distilled trios

## Core Concepts

- **Soul** — who you are: value proposition, positioning, beliefs, trust mechanics
- **Form** — how you write: sentence rhythm, vocabulary, opening/closing patterns
- **Playbook** — which template for which situation (daily report / tutorial / commentary / ...)
- **Profile** — what you have: identity, experiences, opinions, references

Soul stays stable across topics. Form is swappable. Playbook is Soul applied
to specific scenarios. Profile is a parallel material input.

## Built-in Library (Chinese)

Five iconic Chinese authors come pre-distilled with complete trios:

- **三毛 (sanmao)** — lyrical narrator
- **王小波 (wangxiaobo)** — ironic essayist
- **韩寒 (hanhan)** — short-sentence blogger
- **猫笔刀 (maobidao)** — companion-style finance writer
- **李笑来 (lixiaolai)** — concept evangelist

## Status

Chinese-first, pure-prompt skill (no scripts/builds). International support
not currently a priority — see DESIGN.md for architecture details.

## Documentation

- [README.md](./README.md) — full docs (Chinese, primary)
- [DESIGN.md](./DESIGN.md) — architecture and decisions
- [SKILL.md](./SKILL.md) — Claude-loaded entry point

## License

[MIT](./LICENSE)
