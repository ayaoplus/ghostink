# /ghostink check

Audit any text against your style spec. Works on Ghostink-generated drafts or your own writing.

## Usage

```
/ghostink check [file-path]
/ghostink check                  ← will ask for text input
```

## Process

1. Load `style_spec.md`
2. Read the target text (from file or user input)
3. Run all checks below
4. Output a structured audit report

## Checks

### Structure Check
- [ ] Opening length (within spec limit?)
- [ ] Separator usage ("……" or equivalent present?)
- [ ] Article type compliance (matches a defined type template?)
- [ ] Closing style (matches spec patterns?)
- [ ] Section count and order

### Vocabulary Check
- [ ] Scan for "never use" words → list each violation with line location
- [ ] Scan for formal connectors that should be colloquial (e.g., "因此"→"所以", "此外"→"另外")
- [ ] Check for signature expressions — are they naturally present?
- [ ] Profanity/colloquial density — within expected range?

### Voice Check
- [ ] Explicit personal judgments (count — at least 2?)
- [ ] Self-deprecation or vulnerability present?
- [ ] Bold markers used on key information (not decorative)?
- [ ] Sentence rhythm — short/long alternation pattern?

### Prohibition Check
- [ ] No four-character idiom clusters (2+ consecutive)
- [ ] No parallel sentence structures ("we must X, we must Y, we must Z")
- [ ] No textbook transitions ("firstly... secondly... lastly")
- [ ] No moralizing ("this teaches us that...")
- [ ] No passive voice hedging ("据悉", "被认为")
- [ ] No summary paragraph at the end

### Emotional Tone Check (if article type provides emotional rules)
- [ ] If covering positive events: is there a cautionary "but" clause?
- [ ] If covering negative events: does it avoid empty reassurance?
- [ ] Overall tone matches the article type baseline?

## Output Format

```
Ghostink Style Audit
====================
Target: [filename or "inline text"]
Spec: style_spec.md
Article Type: [detected or unknown]

Score: 82/100

STRUCTURE          [4/5]
  [PASS] Opening: 3 sentences ✓
  [PASS] Separators: 2 found ✓
  [FAIL] Closing: ends with summary paragraph — should be brief
  [PASS] Section flow matches Type A template ✓

VOCABULARY         [7/10]
  [FAIL] Line 12: "令人" → should be "让人"
  [FAIL] Line 28: "因此" → should be "所以"  
  [FAIL] Line 45: "尽管" → should be "虽然"
  [PASS] Signature expressions: "我觉得" ×2, "就是" ×8 ✓
  ...

VOICE              [5/5]
  [PASS] Personal judgments: 4 found ✓
  [PASS] Self-deprecation: 1 instance ✓
  ...

PROHIBITIONS       [9/10]
  [FAIL] Line 52: "综上所述" — prohibited expression
  ...

EMOTIONAL TONE     [3/3]
  [PASS] Positive event with cautionary note ✓
  ...

Suggestions:
1. Replace "令人" with "让人" on line 12
2. Remove summary paragraph at the end
3. Consider adding a colloquial expression in the opening
```

## Auto-Fix Option

After presenting the report, offer:

> Want me to auto-fix the FAIL items? I'll show you the changes before applying.

If the user agrees, apply fixes and show a diff. Do not modify the original file — create a new file with `_revised` suffix.
