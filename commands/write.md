# /ghostink write

The main creation workflow. Guides the user through a multi-step process from idea to publishable article.

## Prerequisites

Before starting, verify these files exist:
- `style_spec.md` — if missing, tell the user to run `/ghostink style-init` first
- `author_profile/identity.md` — if missing, proceed but warn that narrative grounding will be limited

## Step 1: Define Topic & Type

### Step 1a: Check for brainstorm artifacts (before asking anything)

Silently scan `drafts/_brainstorm/` for `.md` files. If the directory exists and contains files:

1. List the most recent 3 files (by filename date, newest first)
2. For each, read the YAML frontmatter to extract `topic`, `article_type`, and `status`
3. Present them via AskUserQuestion:

   > 我看到你之前跑过 /brainstorm。要不要基于其中一个直接写稿？
   >
   > - A) 基于最近的：`YYYY-MM-DD_<slug>` — <topic> · <article_type>
   > - B) 基于上一个：`YYYY-MM-DD_<slug>` — <topic> · <article_type>
   > - C) 基于更早的：`YYYY-MM-DD_<slug>` — <topic> · <article_type>
   > - D) 不基于，从零开始（走完整流程）
   >
   > 选 A/B/C 会跳过主题讨论，直接进入大纲确认。

4. If user picks A/B/C (brainstorm-based path):
   - Read the selected file in full
   - Parse sections: `核心判断`、`差异化切入`、`可用素材`、`大纲`
   - Echo back in 2-3 sentences: "你在 brainstorm 里定的是 [article_type]，核心判断是 [核心判断]，基于 [素材列表]。我直接按那份大纲走。"
   - **Skip to Step 2's outline-presentation phase** — use the brainstorm file's 大纲 section as the outline, DO NOT regenerate. Ask only: "这个大纲直接用，还是有要改的？"
   - If user confirms → proceed to Step 3 (Write Draft) using article_type from frontmatter
   - If user wants changes → revise the outline in place, re-present, then Step 3

5. If user picks D (or the directory doesn't exist / is empty):
   - Continue to Step 1b below (the original flow)

### Step 1b: Ask for topic (when no brainstorm basis)

Ask the user:

> What do you want to write about? And how developed is your idea — do you have a clear outline already, or just a rough direction?
>
> （如果想法还模糊，建议先跑 /ghostink brainstorm 做一轮结构化讨论。）

### Mode A: Structured Input (user has a clear outline)

The user provides:
- Article type (prompt them to choose from the types defined in their `style_spec.md`)
- Core argument / thesis in a few sentences
- Key points or material
- (Optional) Personal experiences to weave in

After receiving input:
1. Confirm your understanding in 2-3 sentences
2. Suggest 2-3 structural improvements (e.g., "Consider adding a counter-example in section 2?" or "This anecdote would work well as the opening hook")
3. Wait for user confirmation before proceeding

### Mode B: Exploratory Input (user has a rough direction)

The user provides a vague topic, maybe a reference link or a few scattered thoughts.

Guide them through Socratic questioning (3-5 rounds max):
1. What's your attitude toward this topic? (positive / negative / complex)
2. Who is your audience?
3. Do you have any personal experience related to this?
4. What's the one thing you want readers to remember? Say it in one sentence.
5. (Adapt follow-up questions based on answers)

After convergence, summarize:
> Based on our discussion, I understand you want to express [X]. I recommend article type [Y]. The core argument is [Z]. Shall I proceed with the outline?

Wait for confirmation.

## Step 2: Generate Outline

Read the article type template from `style_spec.md` (Section 5 or equivalent).

Generate a structural outline following the template:
- List each section/block with 1-2 sentence descriptions
- Mark where "……" separators go
- Indicate which personal experiences (if any) will be woven in — cross-reference `author_profile/experiences.md` if available
- Suggest where key data points or examples should appear

Present the outline to the user. Wait for approval or revision requests.

## Step 3: Write Draft

Load these from `style_spec.md`:
- Layer 1 (Voice Core): sentence patterns, vocabulary fingerprint, prohibitions, emotional rules
- Layer 2 (Domain Adaptation): terminology handling, data presentation
- The specific article type template being used

Also load `author_profile/` files as background context (identity.md at minimum).

Write the full article following:
1. The approved outline from Step 2
2. All style rules from the spec
3. The vocabulary fingerprint — use the "must use" words, avoid the "never use" words

Output the draft to the user. Do NOT write to file yet — the user needs to review first.

## Step 4: Auto-Review

Immediately after presenting the draft, run the review checklist from the style spec (Section 10 or equivalent). For each item, report pass/fail:

```
Style Audit Report
==================
[PASS] Opening within 5 sentences
[PASS] "……" separators present
[PASS] 2+ explicit personal judgments
[FAIL] Found "令人" in paragraph 3 — should be "让人"
[PASS] Ends with appropriate closing
[WARN] No self-deprecation detected — consider adding
...
```

Also scan for:
- Any words from the "never use" list
- Consecutive formal connectors that should be colloquial (e.g., "因此" → "所以")
- Structural violations (missing separator, wrong section order)

Present the report to the user.

## Step 5: Revise

Based on the audit report and user feedback:

If the user says "fix it" or similar → automatically fix all FAIL items and present the revised draft.

If the user gives specific instructions (e.g., "paragraph 2 doesn't sound like me, more casual") → revise accordingly.

If the user approves the draft → proceed to Step 6.

Multiple revision rounds are normal. Loop Steps 4-5 until the user is satisfied.

## Step 5.5: De-AI Review

After the draft passes style review and user revisions, run the de-AI audit automatically. Load the full rulebook from `commands/deai.md` and execute all 9 rules.

If the de-AI score is **below 70**: flag the issues and offer to auto-fix before proceeding.
If the score is **70-85**: show the report, offer optional fixes, let the user decide.
If the score is **above 85**: briefly note the score and proceed.

This is a non-skippable step. Every draft must pass through de-AI review.

## Step 5.6: Illustrate (Optional)

After de-AI review, ask:

> Want to add illustrations to this article? (y/n)

If yes, run the illustration workflow from `commands/illustrate.md` on the current draft.

If `illustrate_config.md` doesn't exist, guide the user through first-time setup (choose provider, model, API key).

If the user declines, skip to Step 6.

## Step 6: Format & Output

Ask the user:

> Where will this be published? Options:
> 1. WeChat Official Account (rich formatting)
> 2. X / Twitter long-form article
> 3. X / Twitter thread
> 4. Plain markdown
> 5. Other (specify)

Apply Layer 3 (Platform Adaptation) rules from the style spec. If no Layer 3 exists for the chosen platform, apply sensible defaults:

**WeChat**: Keep `**bold**` markers, keep "……" separators, add `!` image placeholders where appropriate.

**X long-form**: Remove bold markers (X Articles don't support markdown bold), convert "……" to blank lines, keep paragraph structure intact, ensure under 4000 characters if possible.

**X thread**: Split into individual tweets. Each tweet must be independently readable. First tweet is the hook. Last tweet is the conclusion. Add numbering (1/N format). Each tweet under 280 characters.

**Plain markdown**: Output as-is.

Also generate 5 candidate titles following the title rules in the style spec (if defined) or these defaults:
- Keep under 8 characters if possible
- Colloquial, emotional, or suspenseful
- No clickbait cliches

Write the final article to `drafts/YYYY-MM-DD_title.md`.

## Step 7: Deposit New Experiences

Silently scan the user's input from Step 1 (and any material provided during the session) for personal experiences that aren't already in `author_profile/experiences.md`.

If new experiences are found:
1. Extract them into numbered entries (following the existing format in experiences.md)
2. Show the user: "I noticed some new personal details from this session. Shall I add these to your author profile?"
3. If approved, append to `author_profile/experiences.md`

This keeps the author profile growing naturally with each writing session.
