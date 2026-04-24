# /ghostink style-evolve

Update the style spec based on user feedback or new requirements.

## Usage

```
/ghostink style-evolve [feedback or instruction]
```

## Two Modes

### Mode A: Feedback-Driven Evolution

The user provides specific feedback about what's wrong with the current style output:

Examples:
- "I would never use that kind of transition, I prefer jumping directly"
- "The self-deprecation is overdone, dial it back"  
- "When writing about tech topics, I'm more analytical and less emotional"

Process:
1. Read current `style_spec.md`
2. Identify which rule(s) the feedback applies to
3. Propose specific changes to the spec (show diff)
4. Wait for user approval
5. Apply changes and tag with `[evolved: reason]`

### Mode B: Domain Pivot

The user wants to adapt the style for a different content domain while preserving the voice core:

Example:
- "I want to evolve this financial blogger style into an AI/tech blogger style"

Process:
1. Read current `style_spec.md`
2. Identify the three layers:
   - Layer 1 (Voice Core): preserve entirely
   - Layer 2 (Domain): needs replacement
   - Layer 3 (Platform): preserve unless user specifies otherwise
3. Ask the user for new domain context:
   - What topic area?
   - Any reference articles in the new domain? (optional — these help calibrate terminology)
   - What article types make sense for this domain?
4. Rewrite Layer 2:
   - Replace terminology handling rules
   - Redefine article types with new structural templates
   - Update data presentation patterns
5. Present the evolved spec for review

### Mode C: Spec Refinement from Article

The user provides a completed article (either their own writing or a Ghostink output) and asks for the spec to learn from it:

```
/ghostink style-evolve --from drafts/2025-04-04_article.md
```

Process:
1. Read the article
2. Compare against current spec rules
3. Identify patterns in the article that aren't captured in the spec
4. Propose additions/modifications
5. Wait for approval before applying

## Changelog

Every evolution is logged. Append to `style_spec.md` or a companion changelog file:

```markdown
## Changelog
- [2025-04-04] Evolved: reduced self-deprecation frequency per user feedback
- [2025-04-10] Domain pivot: financial → AI/tech, Layer 2 rewritten
```

## Safety

- Never delete existing rules without explicit user approval
- When modifying Layer 1 (Voice Core), always warn: "This changes your fundamental voice characteristics. Are you sure?"
- Keep a backup: before any major change, copy the current spec to `style_spec_backup_YYYYMMDD.md`
