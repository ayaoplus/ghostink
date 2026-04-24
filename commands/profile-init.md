# /ghostink profile-init

Guides first-time setup of `author_profile/*.md` through a layered conversation. Not a blank-template fill-in-the-form — uses AskUserQuestion to actively extract your identity, experiences, opinions, and cultural references.

## Why this exists

`author_profile/` is the fuel ghostink needs to write like you. Without it:
- `/brainstorm` can't suggest topics from your life, can't recommend relevant materials
- `/write` invents fake-sounding details when it needs a concrete experience to reference
- Articles contradict your established positions across time

If the profile files are empty or only contain template placeholders, ghostink degrades to "generic AI with your style spec" — you lose half the value.

## When to use

- First time setting up ghostink (after `/style-forge`, before first `/write` or `/brainstorm`)
- When you realize your profile is thin and writes are coming out generic
- NOT for fine-tuning an already-rich profile — use direct editing or let `/write`'s Step 7 grow it incrementally

## Prerequisites

- The studio root can be resolved (see SKILL.md → Studio Discovery Rules)
- Recommended: `style_spec.md` exists at the studio root (so the profile can be anchored to the voice you're building toward)

If `author_profile/` is missing at the studio root, create the directory structure first:

```
author_profile/
  ├── identity.md
  ├── experiences.md
  ├── opinions.md
  └── refs.md
```

Copy templates from the skill's `templates/author_profile/` if the target files don't exist.

## Step 0 — Detect current state

For each of the 4 files, classify state:

- **Empty or template-only**: file missing, or contains only placeholder examples (e.g., `E001 [Example: First job]` with bracketed placeholders still in place)
- **Partially filled**: has at least 1 real entry but <3 entries
- **Rich**: has 3+ real entries

Present a one-line status to the user:

> 检测到当前 profile 状态：
> - identity.md: [空 / 部分填 / 已填]
> - experiences.md: [N 条真实条目]
> - opinions.md: [N 条]
> - refs.md: [N 条]

Then ask:

> 怎么处理已有内容？
> - A) 追加 —— 保留现有条目，这次只加新的（推荐）
> - B) 重写 —— 清空后从头来过（破坏性，需二次确认）
> - C) 只跑缺失的部分 —— 已富集的文件跳过

Default to A. If B is chosen, immediately ask a confirmation: "这会清掉现有 N 条记录，确认？"

## Step 1 — Tier 1 Essential (～5 minutes)

This is the minimum needed before `/write` works well.

### 1.1 Identity basics (writes to `identity.md`)

Ask these via AskUserQuestion (one at a time, not batched):

**Q1:**
> 你主要在哪发文？这决定默认的语气和格式。
> - A) 公众号 / 中文长文平台
> - B) Twitter/X 长文或 thread
> - C) 个人博客 / Substack
> - D) 自由输入（具体说一下）

**Q2:**
> 你的读者画像最接近哪个？
> - A) 行业内同行（懂行，不用解释基础概念）
> - B) 广义爱好者（对话题感兴趣但不专业）
> - C) 熟人朋友圈（小范围，可以放飞）
> - D) 自由输入

**Q3:**
> 你写作主要覆盖什么领域？用一句话说。
> - （纯自由输入 — 没有候选）

**Q4:**
> 一句话形容你写作时的"人设底色"。参考候选：
> - A) 有经验的专业人士，但放低姿态跟读者聊天
> - B) 一个真诚的普通人，分享自己的生活和想法
> - C) 带刺的观察者，对时事有鲜明态度
> - D) 老朋友式的陪伴者，情绪价值 + 信息并重
> - E) 自由输入

Based on Q4 and the user's existing `style_spec.md` Layer 0 Author DNA (if exists), generate:
- `Personality Traits` field: 3 bullet points that triangulate from style spec + user's answer
- `Notes` field: any context the user added

Write to `identity.md` preserving any existing content (unless Step 0 mode is "重写"). If appending and a field already has content, ask before overwriting that specific field.

### 1.2 Seed experiences (writes to `experiences.md`)

Target: 3-5 entries.

Open with:

> 说 3-5 件对你来说印象深、可能会在文章里引用的真实经历。可以是职业转折、生活片段、尴尬瞬间、觉醒时刻。简短说就行，我来整理成条目。
>
> 先给我第一件。

For each experience the user describes:

1. Extract: 2-3 key details, 1 emotional note/punchline, 3-5 tags
2. Format into the existing E### schema from `experiences.md`
3. Show the formatted entry back: "我整理成这样 —— [entry]。准吗？"
4. If user confirms, append. If user adjusts, incorporate changes.
5. Ask: "还有一件吗？"
6. Loop until user says "够了" / "没了" / equivalent, OR hits 5 entries.

**Numbering rule:** scan existing `experiences.md` for highest E number. Start new entries at `E<highest + 1>`.

**If the user struggles to come up with experiences**, offer pump-priming prompts (one at a time):

- "你人生最被动的一次是什么时候？"
- "你做过最得意的事是啥？"
- "有没有一个你现在回想起来特别尴尬的场景？"
- "第一次亏大钱是什么情况？"（如果领域相关）

### 1.3 Seed opinions (writes to `opinions.md`)

Target: 3-5 entries.

Open with:

> 说 3-5 个你和多数人判断不太一样的观点。不是"AI 很重要"这种没争议的，是你说出来会有人反对的那种。
>
> 先给第一个。

For each opinion:

1. Extract: Topic, Position (one sentence), Nuance (caveats)
2. Format into existing schema
3. Confirm with user
4. Append
5. Loop until 5 or user stops

**If user can't come up with opinions**, offer prompts:

- "你看到什么事会忍不住想喷一句？"
- "有什么现在主流的做法，你觉得 10 年后会被证明是错的？"
- "你觉得哪类人被高估了？哪类被低估了？"

## Step 2 — Tier 2 Optional (refs.md)

Ask once:

> Tier 1 完成了。要不要再顺便填一下 references —— 你常引用的人/书/游戏/影视/历史事件？这个可选，但对文章里生成"带你个人味道"的比喻很有帮助。
> - A) 填一下（再花 5-10 分钟）
> - B) 跳过，以后再说

If B → skip to Step 3.

If A, run 4 mini-rounds:

**2.1 People** — "你文章里可能反复提到的 3-5 个人物是谁？可以是名人、历史人物、身边朋友（用化名）。" 每人追问一句 "你怎么引用他们？"

**2.2 Books/Articles** — "有 3-5 本对你影响深的书或关键文章吗？"

**2.3 Games & Entertainment** — "你经常拿来打比方的游戏、综艺、节目有哪些？"

**2.4 Films/Historical events** — "经常用作类比的电影或历史事件？"

Each round: accept 0-5 entries, format to schema, append to `refs.md`.

## Step 3 — Hand off

Summarize what got written:

> Profile 初始化完成：
> - identity.md: 4 个核心字段已填
> - experiences.md: 新增 N 条
> - opinions.md: 新增 N 条
> - refs.md: 新增 N 条 / 跳过
>
> Profile 不是一次性填完就完了。每次跑 /write 结束时会建议你追加新素材，它会自然长大。

Offer next step via AskUserQuestion:

> 接下来想干啥？
> - A) 跑 /ghostink brainstorm 试一篇（推荐 —— 验证 profile 效果最快的方式）
> - B) 跑 /ghostink write 直接写稿
> - C) 先让我自己看一眼 profile 文件再决定
> - D) 都不，结束

Stop after user picks. Do not auto-invoke the next command unless they picked A/B and said "现在开始".

---

## Implementation notes

- **One AskUserQuestion per sub-step.** Don't batch Q1+Q2+Q3 into one big question — interactive flow gets lost.
- **Never let the user stare at a blank prompt for more than 10 seconds worth of thinking.** If they're stuck, offer pump-priming prompts.
- **Echo back formatted entries** before appending. The user should see what you wrote in their voice, not just trust you did it right.
- **Respect existing content.** Default append mode, never destroy on accident. The "重写" mode needs explicit double-confirmation.
- **Keep session under 20 minutes total.** If the user is running long, offer to save progress and resume: "已经填了不少了，要不这批先存，下次继续剩下的？"
- **When echoing formatted entries, use the exact schema from the template files** — format drift now becomes noise for `/brainstorm` and `/write` later.
