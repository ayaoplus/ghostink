# /ghostink deai

Remove AI-flavor from text. This is Ghostink's signature feature — a multi-pass review that catches the subtle patterns that make AI-generated text feel "off" to human readers.

The core insight: **AI-flavor is not about tone. It's about predictability.** When a reader finishes the first paragraph and can already guess what comes next, trust is lost.

## Usage

```
/ghostink deai [file-path]
/ghostink deai                    ← will ask for input or work on current draft
```

Can run standalone on any text, or as the final step in `/ghostink write`.

## The De-AI Rulebook

### Rule 1: Punctuation Audit — The First Filter

**AI signature:** Every sentence ends with a period. Quotes appear in pairs everywhere. Dashes create artificial pauses.

**Human pattern:** Punctuation follows breath, not grammar rules.

**Check:**
- Count periods vs commas ratio. If periods ≥ commas, flag as AI-heavy
- Scan for decorative quotes (quotes NOT used for actual quotation or irony). Flag each occurrence
- Scan for dash clusters (3+ dashes in close proximity). Flag as artificial rhythm
- Sentences ending without any terminal punctuation (just flowing into the next) should exist — if zero found, add some

**Fix:**
- Convert some periods to commas where sentences flow naturally
- Remove decorative quotes — keep only actual quotations and ironic usage
- Break up dash clusters — replace some with commas or line breaks

### Rule 2: Structure Predictability — If It's "Too Right," It's Wrong

**AI signature:** Topic sentence → supporting evidence → conclusion. Every section. "Firstly... secondly... finally..." is AI's autograph.

**Check:**
- Scan for "首先/其次/然后/最后" or "First/Second/Third/Finally" patterns → flag immediately
- Check if every paragraph opens with a topic sentence followed by expansion → flag uniform pattern
- Check if the article follows 总分总 (introduction → body → summary) → flag if rigid

**Fix:**
- Restructure: let some points emerge mid-paragraph rather than leading
- Add a seeming digression that circles back to the main point
- Delete explicit transition sentences — let topics jump between paragraphs

### Rule 3: Sentence Pattern Repetition — The Most Hidden Fingerprint

**AI signature:** Repeating the same syntactic structure. "不是A，而是B" three times. "与其说X，不如说Y" twice. "真正的Z，从来都是..." everywhere.

**Check:**
- Extract sentence skeletons (strip content, keep structure)
- Flag any sentence pattern that appears 3+ times in the article
- Flag "big word" pileups: "黄金法则" "底层密码" "终极答案" "核心本质" "深层逻辑" / "golden rule" "ultimate answer" "core essence" "fundamental truth"

**Fix:**
- Third occurrence of any pattern: rewrite with a completely different structure
- Replace big words with specific, concrete descriptions
- "The golden rule of content creation" → "一条我试过确实好使的写法"

### Rule 4: Information Density — Every Paragraph Must Earn Its Place

**AI signature:** Three paragraphs, three different phrasings, one idea. Synonym stacking disguised as development.

**Check:**
- For each paragraph, extract the core claim in one sentence
- If two adjacent paragraphs have the same core claim → flag as redundant
- **Deletion test:** Remove a paragraph. If the surrounding text still reads coherently, the paragraph was padding.

**Fix:**
- Merge redundant paragraphs into one
- Each paragraph must advance the argument: new information, new angle, or new evidence
- If a paragraph only restates what was said before in different words, delete it

### Rule 5: Generic Metaphors — The Universal Comparison Problem

**AI signature:** Metaphors that fit any article: "like seeds breaking through soil," "like a lighthouse guiding the way," "like a blank canvas waiting to be painted."

**Check:**
- Extract all metaphors and similes
- **Portability test:** Could this metaphor be copy-pasted into a completely unrelated article and still work? If yes → flag as generic
- Check if metaphors have: specific time? specific action? specific sensory detail? If none → flag

**Fix:**
- Replace generic metaphors with ones that have "dirt": time, place, action, texture
- Bad: "Learning is like planting a seed"
- Good: "Learning this felt like the first time I tried to use chopsticks left-handed — everything I thought I knew was wrong, and soup went everywhere"

### Rule 6: Rhythm Flatness — The AI Metronome

**AI signature:** Every sentence is 20-30 characters. Uniform rhythm like a metronome.

**Check:**
- Calculate sentence length variance. If standard deviation < 8 characters → flag as too uniform
- Check for long-short alternation: look for sequences of 3+ sentences with similar length
- Check if any single-sentence paragraphs exist (for emphasis). If zero → flag

**Fix:**
- Inject rhythm breaks: one 5-word sentence after three long ones
- Allow run-on sentences that keep going with commas without stopping because that's how people actually think when they're excited about something
- Bold a core statement and give it its own line

### Rule 7: Emotional Flatline — One Tone Throughout

**AI signature:** Same emotional register from start to finish. No voltage change.

**Check:**
- Divide article into quartiles
- Rate the emotional intensity of each quartile (1-5 scale)
- If all quartiles are within 1 point of each other → flag as flat
- Check for emotional pivot points: does the article have at least one moment where the tone shifts?

**Fix:**
- The emotional arc for most good articles: **empathy → sting → awakening → conviction**
- Insert a shift: go from warm to sharp, or from analytical to personal
- Not every article needs drama, but it needs at least one moment of "I didn't expect this piece to go there"

### Rule 8: Opening & Closing — The Danger Zones

**AI signature openings:** "Today let's talk about..." / "Have you ever wondered..." / "In today's world..."
**AI signature closings:** "Hope this article helps!" / "In conclusion..." / "Let's embrace the future together"

**Check:**
- Does the opening start with a question? → potential AI pattern (unless it's a very specific, surprising question)
- Does the opening contain "让我们" / "let's" / "在当今" / "in today's" → flag
- Does the closing summarize the entire article? → flag
- Does the closing contain "希望" / "hope" / "祝愿" / "wish" / "总结" / "in conclusion" → flag

**Fix:**
- Opening: start with the most surprising/specific claim or a concrete scene
- Closing: stop at the most confident judgment. No wrap-up. No well-wishes. Just stop.

### Rule 9 (Ghostink Original): Self-Insertion Test

**AI signature:** God-view narration. "Many people..." / "Research shows..." / "The market indicates..."

**Check:**
- Count first-person statements with specific experiences ("I tested..." / "Last month I..." / "My experience was...")
- If zero first-person specific experiences → flag
- Count god-view generalizations ("many people" / "in general" / "research shows") → flag if > 3

**Fix:**
- Replace god-view with first-person: "Many people struggle with X" → "I struggled with X for months until..."
- Add operational details: not "there are several approaches" but "I tried three approaches, the second one worked"
- Include your actual data, decisions, and mistakes

## Scoring

After running all rules, output a De-AI Score:

```
De-AI Audit
===========
Overall: 76/100 (some AI patterns detected)

Rule 1 — Punctuation:        [8/10]  ✓ Good comma/period ratio
Rule 2 — Structure:          [6/10]  ⚠ Predictable paragraph pattern in section 2
Rule 3 — Sentence patterns:  [5/10]  ✗ "不是A而是B" appears 4 times
Rule 4 — Info density:       [9/10]  ✓ Each paragraph advances
Rule 5 — Metaphors:          [7/10]  ⚠ 1 generic metaphor in paragraph 5
Rule 6 — Rhythm:             [8/10]  ✓ Good length variance
Rule 7 — Emotional arc:      [10/10] ✓ Clear shift at paragraph 8
Rule 8 — Opening/Closing:    [9/10]  ✓ Strong opening, clean exit
Rule 9 — Self-insertion:     [7/10]  ⚠ Only 1 first-person experience

Top 3 fixes:
1. Line 15, 23, 31, 40: Rewrite "不是A而是B" — 4 occurrences, max 2 allowed
2. Paragraph 5: Replace lighthouse metaphor with specific analogy
3. Add a personal anecdote in section 3 to ground the argument
```

## Auto-Fix Mode

After presenting the report:

> Want me to auto-fix the flagged items? I'll preserve your meaning and rewrite the problematic patterns.

If yes:
1. Fix all items scored below 7
2. Show a diff for each change
3. User approves or rejects each fix individually

## Integration with /ghostink write

In the write workflow, de-AI runs as the final quality gate before formatting:

```
... → Step 5: Revise → Step 5.5: Illustrate → Step 5.6: De-AI → Step 6: Format → ...
```

After the draft passes style audit (/check), run de-AI automatically. If the de-AI score is below 70, the draft should go through another revision round before proceeding.
