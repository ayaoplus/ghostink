# /ghostink style-init

Initialize a style spec from reference articles through multi-round iterative analysis.

## Usage

```
/ghostink style-init [path-to-reference-articles]
```

If no path is provided, look for `studio/reference_articles/`. If that doesn't exist either, ask the user to provide a directory path.

## Prerequisites

- A directory containing 20+ articles in markdown or plain text format
- The more articles, the more accurate the spec (50+ is ideal, 20 is minimum)
- Articles should represent the target writing style — they can be the user's own writing or someone else's writing they want to emulate

## Process

### Phase 0: Setup

1. Scan the reference directory and count articles
2. If fewer than 20: warn the user that accuracy will be limited
3. If more than 200: note that you'll sample from the collection rather than reading all
4. Create `studio/` directory structure if it doesn't exist:
   ```
   studio/
   ├── style_spec.md
   ├── author_profile/
   │   ├── identity.md
   │   ├── experiences.md
   │   ├── opinions.md
   │   └── references.md
   └── drafts/
   ```
5. Tell the user: "Starting style analysis. This will take several rounds. I'll work through it automatically and show you the result when done."

### Phase 1: First Read (Broad Strokes)

1. Randomly select 15-20 articles (spread across different time periods if filenames contain dates)
2. Read each article fully
3. Analyze and extract:
   - **Voice**: sentence length distribution, short/long ratio, rhythm patterns
   - **Vocabulary**: colloquial vs formal ratio, signature phrases, profanity usage, recurring expressions
   - **Structure**: how articles open, how they close, section patterns, separator usage
   - **Emotion**: emotional range, how different moods are expressed
   - **Prohibitions**: patterns that never appear (formal connectors, cliches, etc.)
4. Generate `style_spec.md` v0.1 using the template from `templates/style_spec_template.md`
5. Log findings to `studio/style_init_log.md`

### Phase 2-N: Iterative Refinement (Auto-loop)

For each subsequent round:

1. Select 15-20 NEW articles (not previously read)
2. Read each article
3. For each article, "predict" its characteristics using the current spec, then compare with reality:
   - Which rules were confirmed? (increase confidence)
   - Which rules were violated? (mark for revision)
   - What new patterns appeared that the spec doesn't cover? (mark for addition)
4. Update the spec:
   - Revise inaccurate rules
   - Add newly discovered patterns (tagged with `[vN.N new]`)
   - Note confidence levels where relevant
5. Append round summary to `studio/style_init_log.md`

**Convergence check**: After each round, compare the diff between the previous and current spec. If changes are minor (only small refinements, no new sections), the analysis has converged. Stop and proceed to Phase 3.

**Maximum rounds**: 5 rounds (to avoid excessive processing). If not converged by round 5, proceed anyway.

### Phase 3: Article Type Classification

After the style is extracted, analyze the structural patterns to identify distinct article types:

1. Re-scan all read articles and cluster them by structural similarity
2. For each cluster, define:
   - Type name and description
   - Structural template (section order, section purposes)
   - Typical opening patterns
   - Typical closing patterns
   - Emotional baseline
   - Length range
3. Write type templates into Section 5 of the spec
4. Ask the user: "I identified [N] article types: [list]. Which ones do you want to use for future creation? You can also rename them."

### Phase 4: Word Frequency Analysis

Run a statistical analysis across ALL available articles (not just the ones read in detail):

1. Extract high-frequency phrases (2-4 character Chinese phrases, or common English expressions)
2. Identify signature expressions and their frequencies
3. Build a "must use / never use" vocabulary table by comparing author's word choices against common AI defaults
4. Write the vocabulary fingerprint into the spec

### Phase 5: Finalize

1. Write the complete `style_spec.md`
2. Present a summary to the user:
   - Key style characteristics (5-7 bullet points)
   - Article types identified
   - Top 10 "must use" and "never use" words
   - Confidence assessment (which rules are solid vs tentative)
3. Ask: "Review the spec at `studio/style_spec.md`. Anything that feels off? I can adjust."

### Phase 6: Author Profile Bootstrap

If the reference articles appear to be the user's own writing (check for first-person voice, personal anecdotes):

1. Extract personal details mentioned across articles → draft `identity.md`
2. Extract life experiences → draft `experiences.md` (numbered entries)
3. Extract stated opinions/positions → draft `opinions.md`
4. Extract frequently mentioned people, books, games, etc. → draft `references.md`
5. Present to user: "I also extracted some background info from your articles. Please review `author_profile/` and correct anything inaccurate."

If the reference articles are someone else's writing:
- Skip this phase
- Tell the user to fill in `author_profile/` manually, as it should reflect THEIR background, not the reference author's

## Output

After completion:
```
studio/
├── style_spec.md           ← the generated style rulebook
├── style_init_log.md       ← detailed analysis log (round-by-round)
├── author_profile/          ← bootstrapped (if applicable)
│   ├── identity.md
│   ├── experiences.md
│   ├── opinions.md
│   └── references.md
└── reference_articles/      ← unchanged, user's input
```

## Notes

- The entire init process should take 10-20 minutes depending on article count
- The user does NOT need to intervene during Phases 1-4. Only Phase 5 and 6 require user input
- If interrupted, the process can be resumed — the log file tracks which articles have been read
