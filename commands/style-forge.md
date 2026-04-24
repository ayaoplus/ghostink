# /ghostink style-forge

From analyzed references to YOUR voice. This is the "character creation" process — you pick traits from multiple reference authors, combine them with your own identity and domain, and forge a style spec that is uniquely yours.

## Prerequisites

- At least 1 analyzed reference author in `analyzed_authors/[author]/style_spec.md`
  - If none exist, tell the user to run `/ghostink style-init` first
- Ideally 2-4 reference authors for meaningful differentiation
- The user should have some idea of their own domain and audience (if not, Phase 2 will draw it out)

## Usage

```
/ghostink style-forge
/ghostink style-forge --references catblade,author_b
```

## Overview

```
Reference Specs ──→ Comparison Matrix ──→ User picks traits
                                              ↓
                    User's own identity ──→ Synthesis ──→ YOUR style_spec.md
                                              ↓
                                     Differentiation check
```

The key insight: a reference author's DNA has **portable** and **non-portable** elements:

| Element | Portable? | Example |
|---------|-----------|---------|
| Positioning strategy | Yes | "Friend, not teacher" can work in any domain |
| Trust mechanics | Yes | "Show losses publicly" → "Show failed experiments publicly" |
| Emotional contract model | Yes | "Emotional anchor in volatility" → "Calm voice in hype cycles" |
| Narrative engine approach | Yes | "Stories ARE the argument" works in any domain |
| Core beliefs | **No** | Must be YOUR beliefs, not borrowed |
| Domain knowledge | **No** | Finance terms don't transfer to AI |
| Specific vocabulary | **Partially** | Colloquial tone transfers, specific terms don't |
| Article types | **Partially** | Structure transfers, content templates don't |

---

## Phase 1: Reference Review & Comparison

### 1.1 Load References

Scan `analyzed_authors/` for all analyzed authors. For each, load their `style_spec.md` and extract a summary of Layer 0 dimensions.

If `--references` flag is provided, only load the specified authors.

### 1.2 Build Comparison Matrix

Present a side-by-side comparison across ALL Layer 0 dimensions. This is the user's "menu" to pick from.

```
Reference Author Comparison
============================

                    │ catblade              │ author_b              │ author_c
────────────────────┼───────────────────────┼───────────────────────┼──────────────
READER VALUE        │                       │                       │
  Primary           │ Info translation +    │ Deep analysis +       │ Trend spotting +
                    │ emotional companion   │ frameworks            │ entertainment
  Secondary         │ Entertainment         │ Career guidance       │ Community identity
────────────────────┼───────────────────────┼───────────────────────┼──────────────
POSITIONING         │                       │                       │
  Role              │ Rich friend           │ Senior mentor         │ Fellow explorer
  Power dynamic     │ Deliberately equal    │ Slightly above        │ Equal
  Paradox           │ Expert who self-      │ Authority who admits  │ Insider who stays
                    │ deprecates            │ doubt                 │ curious
────────────────────┼───────────────────────┼───────────────────────┼──────────────
NARRATIVE ENGINE    │                       │                       │
  Persuasion #1     │ Personal experience   │ Data + frameworks     │ Trend evidence
  Story role        │ Story IS the argument │ Story as illustration │ Story as hook
  Wrong handling    │ Public admission      │ Silent correction     │ Post-mortem essay
────────────────────┼───────────────────────┼───────────────────────┼──────────────
TRUST MECHANICS     │                       │                       │
  Skin in game      │ Shows real P&L        │ Shows track record    │ Builds in public
  Vulnerability     │ Deep, genuine         │ Rare, controlled      │ Frequent, casual
────────────────────┼───────────────────────┼───────────────────────┼──────────────
EMOTIONAL CONTRACT  │                       │                       │
  Reader comes for  │ Stability in chaos    │ Clarity in complexity │ Excitement in
                    │                       │                       │ discovery
  Never does        │ Empty reassurance     │ Oversimplification    │ Fear-mongering
────────────────────┼───────────────────────┼───────────────────────┼──────────────
VOICE               │                       │                       │
  Tone              │ Colloquial + profane  │ Professional + warm   │ Playful + sharp
  Rhythm            │ Short bursts          │ Long analytical       │ Mixed, energetic
  Signature         │ "就这些吧" / profanity│ Frameworks / diagrams │ Metaphors / memes
```

### 1.3 Guided Selection

Walk through each dimension and ask:

> For **[dimension]**, here's what each reference does:
> - catblade: [description]
> - author_b: [description]
>
> Which approach resonates with you? You can:
> 1. Pick one reference's approach
> 2. Combine elements from multiple
> 3. Define your own approach
> 4. Skip (we'll figure it out later)

**Do NOT rush through this.** Each dimension deserves a brief conversation. Ask follow-up questions if the user's answer is vague.

Collect selections into a "forge blueprint":

```
Forge Blueprint
===============
Positioning:   catblade-style (friend, not teacher) + own twist: [user input]
Trust:         catblade (show real results) adapted to: [user's domain]
Narrative:     mix of catblade (experience-first) and author_b (framework-support)
Emotion:       catblade (stability anchor) adapted to: [user's context]
Voice:         catblade (colloquial) but toned down on profanity
```

---

## Phase 2: Identity Definition — The Parts That Must Be Yours

After the user has picked reference traits, focus on what CANNOT be borrowed.

### 2.1 Domain & Audience

Ask:

> Now let's define what's uniquely yours.
>
> 1. **What's your domain?** (e.g., AI/tech, crypto, lifestyle, education)
> 2. **Who specifically reads you?** Not "everyone interested in AI" — WHO are they? (developers? founders? curious non-technical people? investors?)
> 3. **What does your audience struggle with that you can help?** (information overload? hype vs reality? career decisions? keeping up?)
> 4. **What's your unfair advantage?** What do you know or have access to that most people in your domain don't?

### 2.2 Core Beliefs

This is the most important part. Beliefs cannot be borrowed — they are what make YOUR voice different from someone doing the same positioning with the same reference.

Ask:

> What do you believe about [domain] that most people in your space would disagree with, or at least find surprising?
>
> Some prompts to get you started:
> - What's a popular opinion in [domain] that you think is wrong?
> - When [domain] people argue, which side are you usually on?
> - What's the one thing you wish your audience understood?
> - What makes you angry about how [domain] is discussed?
> - What's a mistake you've made that taught you something most people haven't learned yet?

Guide the user to articulate 3-5 core beliefs. These should be:
- **Specific** to their domain (not generic "honesty is important")
- **Somewhat contrarian** (if everyone agrees, it's not a belief, it's a platitude)
- **Actionable** (it should change what you write and how)

### 2.3 Personal Inventory

Ask:

> Last part — what's YOUR material? Reference authors have their stories; you need yours.
>
> 1. **What experiences give you credibility?** (jobs, projects, failures, experiments)
> 2. **What's a story you keep telling friends?** (this usually becomes your signature content)
> 3. **What's something embarrassing or uncomfortable you'd be willing to share?** (vulnerability = trust)
> 4. **Who are the people, books, tools, or ideas you keep referencing?** (your intellectual fingerprint)

Write initial entries to `author_profile/` files.

---

## Phase 3: Synthesis — Forging the Spec

Now combine everything into a unified `style_spec.md`.

### 3.1 Layer 0 Synthesis

For each dimension, merge the reference approach with the user's identity:

**0.1 Reader Value:**
- Start from the reference's value model, but adapt to the user's domain and audience
- Example: catblade's "info translation + emotional companion" → for an AI blogger might become "hype-to-reality translation + builder's companion"
- Write the "unsubscribe test" specific to the user

**0.2 Positioning:**
- Apply the borrowed positioning strategy but calibrate for the user's domain
- The persona paradox must feel natural for the user — don't force catblade's "rich but self-deprecating" if it doesn't fit
- Example: "rich friend who self-deprecates" → "technically deep but explains like a friend" or "industry insider who sides with the audience"

**0.3 Narrative Engine:**
- Adapt the persuasion stack to the user's strengths
- If the user has strong personal experience → experience-first (like catblade)
- If the user is more analytical → data-first with experience support
- Define how the user handles being wrong (must be authentic)

**0.4 Core Beliefs:**
- These come ENTIRELY from Phase 2.2 — no borrowing
- Format them with "shows up as" to make them actionable in writing

**0.5 Trust Mechanics:**
- Adapt reference trust behaviors to the user's domain
- "Show real P&L" → "Show real experiment results" or "Show real tool usage" or "Share actual metrics"
- Define the user's vulnerability pattern

**0.6 Emotional Contract:**
- Define based on the user's audience needs + reference model
- Example: catblade's "stability in market chaos" → "clarity in AI hype cycles"

### 3.2 Layer 1 Synthesis

For voice and craft, use the reference as a starting point but adapt:

**Vocabulary:**
- Keep the colloquial/formal ratio from the reference
- Replace domain-specific terms with the user's domain equivalents
- Build a new "must use / never use" list for the user's domain
- Preserve the general tone (e.g., "profanity OK" or "no profanity")

**Sentence Rhythm:**
- Can borrow directly if the user likes the reference's rhythm
- Or adjust (e.g., "catblade's rhythm but longer analytical passages for technical content")

**Structure & Article Types:**
- Define 2-4 article types for the user's domain, using reference types as structural inspiration
- Example: catblade's "日报型" → "AI 日报" (same structure: opening hook → analysis → news list → closing)
- Example: catblade's "主题叙事型" → "Builder Story" (same narrative arc, different content)

**Opening/Closing Patterns:**
- Adapt reference patterns to the user's domain
- Preserve the structural principle (e.g., "direct entry, no preamble") while changing the content

### 3.3 Layer 2 Domain Adaptation

Write fresh:
- Terminology handling for the user's domain
- Data presentation patterns
- How technical depth is managed

### 3.4 Layer 3 Platform

Ask which platforms the user publishes on and write platform rules.

---

## Phase 4: Differentiation Check

Critical final step. Compare the forged spec against each reference to ensure the user's voice is DISTINCT.

### 4.1 Similarity Scan

For each reference, check:
- Is the positioning too similar? (borrowing strategy is OK; being a clone is not)
- Are the beliefs different enough? (if your beliefs match the reference, you'll sound like a copycat)
- Is the vocabulary distinct? (different domain should naturally help)
- Are the article types differentiated? (structure can be similar; content and angle must differ)

### 4.2 Differentiation Report

```
Differentiation Report
======================

vs catblade:
  SHARED (intentional): Friend positioning, experience-first narrative, colloquial tone
  DISTINCT: Domain (AI vs finance), beliefs (your 5 vs their 6), no profanity,
            longer analytical passages, different trust signal (open-source > P&L)
  ⚠ WARNING: None — sufficiently differentiated

vs author_b:
  SHARED (intentional): Framework-supported analysis
  DISTINCT: Everything else
  ⚠ WARNING: None
```

### 4.3 Uniqueness Statement

Generate a 2-3 sentence "uniqueness statement" — what makes the user's voice impossible to confuse with any reference:

> "You are a technically deep AI practitioner who writes like a friend, not a guru.
> Unlike [catblade], your credibility comes from building, not investing.
> Unlike [author_b], you lead with stories and opinions, not frameworks.
> Your signature: translating AI hype into builder reality, with real experiments as proof."

Write this at the top of the user's `style_spec.md` as a north star.

---

## Phase 5: Output & Review

1. Write the complete `style_spec.md`
2. Present a summary:

   **Your Voice DNA:**
   - Uniqueness statement
   - Core value proposition
   - Top 3 beliefs
   - Borrowed elements and their sources

   **Borrowed vs Original:**
   | Element | Source | Adaptation |
   |---------|--------|------------|
   | Positioning | catblade | Friend → technically-literate friend |
   | Trust | catblade | Real P&L → real experiments |
   | Narrative | catblade + own | Experience-first + technical depth |
   | Beliefs | 100% original | [list] |
   | Vocabulary | adapted | Colloquial base + AI terminology |

3. Ask: "Read through `style_spec.md`. Does it feel like YOU? Or does it feel like you're wearing someone else's clothes? The beliefs and positioning should feel natural — if anything feels forced, tell me."

---

## After Forging

- The user can run `/ghostink write` immediately — the spec is ready
- Use `/ghostink style-evolve` to refine after each writing session
- Use `/ghostink style-evolve --from [article]` to feed published articles back into the spec
- Over time, the spec should drift away from references and toward the user's unique voice

## Reference Tracking

The forged spec should include a "lineage" section at the bottom:

```markdown
## Lineage

This spec was forged from the following references:
- catblade (finance blogger): positioning strategy, trust mechanics, narrative approach
- [author_b] (tech writer): analytical structure, framework integration

Elements that are 100% original:
- Core beliefs (Section 0.4)
- Domain vocabulary (Section 八-b)
- Article types content (Section 五, structure borrowed from catblade)
```

This helps future `/style-evolve` runs understand which parts came from where, so evolution stays coherent.
