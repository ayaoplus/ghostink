# /ghostink style-init

Analyze a reference author — extract their DNA + style into a spec. Can be run multiple times for different authors.

## Usage

```
/ghostink style-init [author-name]
/ghostink style-init catblade
/ghostink style-init --path /some/path/to/articles --name catblade
```

**Resolution order for articles:**
1. If `--path` is provided, use that directory
2. If `[author-name]` is provided, look for `analyzed_authors/[author-name]/articles/`
3. If neither, ask the user for the author name and article directory

## Output Modes

**Multi-reference mode (default):** Output goes to `analyzed_authors/[author-name]/style_spec.md`. This is for when you plan to analyze multiple authors and then forge your own spec via `/ghostink style-forge`.

**Direct mode** (`--direct`): Output goes to `style_spec.md`. This is for when you're emulating a single author directly, without forging.

## Prerequisites

- A directory containing 20+ articles in markdown or plain text format
- The more articles, the more accurate the spec (50+ is ideal, 20 is minimum)
- Articles should represent the target writing style — they can be the user's own writing or someone else's writing they want to emulate

## Process

### Phase 0: Setup

1. Determine author name and article path (see Usage above)
2. Scan the reference directory and count articles
3. If fewer than 20: warn the user that accuracy will be limited
4. If more than 200: note that you'll sample from the collection rather than reading all
5. Create directory structure if it doesn't exist:
   ```
   
   ├── references/
   │   └── [author-name]/
   │       ├── style_spec.md          ← output: this author's analyzed spec
   │       ├── style_init_log.md      ← analysis log
   │       └── articles/              ← source articles (if not elsewhere)
   ├── author_profile/                ← YOUR profile (for forge, not init)
   └── drafts/
   ```
   In `--direct` mode, output goes to `style_spec.md` instead.
6. Tell the user: "Starting analysis of [author-name]. This will take several rounds. I'll work through it automatically and show you the result when done."

### Phase 1: Author DNA Discovery (the soul — before analyzing the body)

**Why this comes first:** Style choices are downstream of identity. An author uses profanity not because of "vocabulary preference" but because they position as an unfiltered friend. Knowing the DNA first makes every subsequent style observation explainable.

1. Select 10-15 articles with MAXIMUM diversity — spread across:
   - Different time periods (early vs recent)
   - Different emotional registers (celebratory, angry, reflective, vulnerable)
   - Different topics (if applicable)
   - Different lengths (short dispatches vs long narratives)

2. Read each article, but NOT for style. Read as a **reader**, asking:
   - Why would someone come back tomorrow to read this person again?
   - What did I get from this article that I couldn't get from a news site or textbook?
   - What is the author's relationship with me (the reader)? How does it feel?
   - What does this author believe about the world? What makes them angry, sad, excited?
   - When the author makes a claim, why do I believe (or not believe) them?

3. Analyze and extract **Layer 0** dimensions:

   **0.1 Reader Value** — Identify the value stack. Common types:
   - Information asymmetry (author has access/knowledge readers don't)
   - Curation & translation (digests complexity into accessible form)
   - Emotional companionship (readers feel less alone)
   - Decision support (helps readers make real-world choices)
   - Entertainment (genuinely fun to read even without practical value)
   - Identity validation (readers feel "someone gets me")
   - Write the "unsubscribe test": if this author vanished, what's the hole?

   **0.2 Author Positioning** — Map the relationship:
   - What role does the author play? (friend / teacher / fellow sufferer / provocateur / curator / coach)
   - What's the power dynamic? (above / equal / deliberately below)
   - What's the persona paradox? (the tension that makes the persona compelling — e.g., "expert who self-deprecates," "wealthy person who shows vulnerability")
   - How does the author manage authority? (assert then undercut? build then qualify?)

   **0.3 Narrative Engine** — How arguments are built at macro level:
   - What is the primary persuasion stack? (personal experience > data > authority > emotion > logic — rank them)
   - How are stories used? (decoration on arguments, OR stories ARE the argument)
   - How does the author handle being wrong? (delete? acknowledge? post-mortem?)
   - What's the conviction pattern? (conclusion-first? evidence-then-reveal? open-ended?)

   **0.4 Core Beliefs** — The recurring worldview:
   - What convictions appear across 5+ articles, often implicitly?
   - When values conflict, which one wins? (pragmatism vs idealism, honesty vs comfort, etc.)
   - What are the author's consistent biases or blind spots?
   - What makes the author angry at a systemic level (not just personal annoyance)?

   **0.5 Trust Mechanics** — Observable credibility behaviors:
   - How does the author prove skin in the game?
   - How is uncertainty expressed? (deny? admit then give direction anyway? fully defer?)
   - How is vulnerability deployed? (strategic? genuine? both?)
   - What would instantly break reader trust?

   **0.6 Emotional Contract** — The unspoken promise:
   - What emotional experience does the reader expect per article?
   - What lines does the author never cross?
   - When do readers most need this author? (crisis? boredom? confusion? anger?)

4. Write Layer 0 of `style_spec.md` using the template
5. Log DNA findings to `style_init_log.md`

**Important:** Do NOT skip to style analysis prematurely. The DNA phase must produce a complete Layer 0 draft before proceeding. This is the foundation everything else builds on.

### Phase 2: Style Extraction (First Read — Broad Strokes)

Now, WITH the DNA as context, analyze how the author's identity manifests in craft.

1. Select 15-20 articles (can overlap with Phase 1 — you're re-reading with a different lens)
2. Read each article, this time focusing on the **surface of the writing**:
   - **Voice**: sentence length distribution, short/long ratio, rhythm patterns
   - **Vocabulary**: colloquial vs formal ratio, signature phrases, profanity usage, recurring expressions
   - **Structure**: how articles open, how they close, section patterns, separator usage
   - **Emotion**: emotional range, how different moods are expressed
   - **Prohibitions**: patterns that never appear (formal connectors, cliches, etc.)
3. For each style observation, note how it connects to Layer 0. Examples:
   - "Uses profanity" → because positioning is "unfiltered friend" (0.2)
   - "Starts with concrete scene" → because persuasion relies on personal experience (0.3)
   - "Always includes a cautionary 'but' after good news" → because emotional contract promises honesty over hype (0.6)
4. Generate Layer 1 of `style_spec.md`
5. Append findings to `style_init_log.md`

### Phase 3: Iterative Refinement (Auto-loop)

For each subsequent round:

1. Select 15-20 NEW articles (not previously read in this phase)
2. Read each article
3. For each article, "predict" its characteristics using the current spec (BOTH Layer 0 and Layer 1), then compare with reality:
   - Which rules were confirmed? (increase confidence)
   - Which rules were violated? (mark for revision)
   - What new patterns appeared that the spec doesn't cover? (mark for addition)
   - **Did any DNA assumptions prove wrong?** (e.g., thought the author was always confident, but found a cluster of deeply uncertain articles)
4. Update the spec:
   - Revise inaccurate rules in both Layer 0 and Layer 1
   - Add newly discovered patterns (tagged with `[vN.N new]`)
   - Note confidence levels where relevant
5. Append round summary to `style_init_log.md`

**Convergence check**: After each round, compare the diff between the previous and current spec. If changes are minor (only small refinements, no new sections), the analysis has converged. Stop and proceed to Phase 4.

**Maximum rounds**: 5 rounds (to avoid excessive processing). If not converged by round 5, proceed anyway.

### Phase 4: Article Type Classification

After DNA and style are extracted, analyze structural patterns to identify distinct article types:

1. Re-scan all read articles and cluster them by structural similarity
2. For each cluster, define:
   - Type name and description
   - Structural template (section order, section purposes)
   - Typical opening patterns
   - Typical closing patterns
   - Emotional baseline
   - Length range
   - **Which Layer 0 values this type primarily serves** (e.g., "daily report" serves information + emotional companionship; "narrative essay" serves entertainment + identity validation)
3. Write type templates into the spec
4. Ask the user: "I identified [N] article types: [list]. Which ones do you want to use for future creation? You can also rename them."

### Phase 5: Word Frequency Analysis

Run a statistical analysis across ALL available articles (not just the ones read in detail):

1. Extract high-frequency phrases (2-4 character Chinese phrases, or common English expressions)
2. Identify signature expressions and their frequencies
3. Build a "must use / never use" vocabulary table by comparing author's word choices against common AI defaults
4. Write the vocabulary fingerprint into the spec

### Phase 6: Finalize

1. Write the complete `style_spec.md`
2. Present a summary to the user, organized in two tiers:

   **Tier 1 — Who this author IS (Layer 0 summary):**
   - Core value proposition (1 sentence)
   - Author-reader relationship (1 sentence)
   - Top 3 core beliefs
   - The "unsubscribe test" answer
   - Trust mechanics summary

   **Tier 2 — How this author WRITES (Layer 1+ summary):**
   - Key style characteristics (5-7 bullet points)
   - Article types identified
   - Top 10 "must use" and "never use" words
   - Confidence assessment (which rules are solid vs tentative)

3. Ask: "Review the spec at `analyzed_authors/[author]/style_spec.md`. Start with Layer 0 (Author DNA) — does it capture why this author is compelling? Then check if the style rules feel right. I can adjust either layer."

4. If the user has more reference authors to analyze, suggest: "Run `/ghostink style-init [next-author]` to analyze another reference. When all references are done, run `/ghostink style-forge` to create your own spec."

### Phase 7: Author Profile Bootstrap

If the reference articles appear to be the user's own writing (check for first-person voice, personal anecdotes):

1. Extract personal details mentioned across articles → draft `author_profile/identity.md`
2. Extract life experiences → draft `author_profile/experiences.md` (numbered entries)
3. Extract stated opinions/positions → draft `author_profile/opinions.md`
4. Extract frequently mentioned people, books, games, etc. → draft `author_profile/refs.md`
5. Present to user: "I also extracted some background info from your articles. Please review `author_profile/` and correct anything inaccurate."

If the reference articles are someone else's writing:
- Skip this phase
- Tell the user to fill in `author_profile/` manually when running `/ghostink style-forge`, as it should reflect THEIR background, not the reference author's

## Output

After completion (multi-reference mode):
```

├── references/
│   └── [author-name]/
│       ├── style_spec.md       ← this author's analyzed spec (DNA + style)
│       └── style_init_log.md   ← detailed analysis log
└── ...
```

After completion (direct mode with `--direct`):
```

├── style_spec.md               ← the generated spec (for direct emulation)
├── style_init_log.md
├── author_profile/              ← bootstrapped (if applicable)
└── ...
```

## Notes

- The entire init process should take 10-20 minutes depending on article count
- The user does NOT need to intervene during Phases 1-5. Only Phase 6 and 7 require user input
- If interrupted, the process can be resumed — the log file tracks which articles have been read
- Run multiple times for different authors. Each author's spec is independent
