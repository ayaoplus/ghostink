# /ghostink illustrate

Generate illustrations for an article. Ghostink handles the creative part (analyzing where images go, writing prompts); image generation is delegated to the best available tool in the user's environment.

## Usage

```
/ghostink illustrate [article-file]
/ghostink illustrate                   ← will ask for input
```

## Image Generation: Runtime Detection

Ghostink does NOT include image generation code. On every run, detect what's available and use the best option:

### Priority 1: baoyu-imagine CLI (recommended)

Check if available:
```bash
bun ~/.claude/plugins/cache/baoyu-skills/baoyu-skills/*/skills/baoyu-imagine/scripts/main.ts --help 2>/dev/null
```

Or check if baoyu-skills is installed as a Claude skill (look for `baoyu-imagine` in available skills list).

**Why this is preferred:** 9 providers (OpenAI, Google, Replicate, DashScope, MiniMax, Jimeng, Seedream, etc.), batch mode, reference images, rate limiting — all maintained by the baoyu-skills project.

If available, generate images via:
```bash
# Single image
bun [baoyu-imagine-path]/scripts/main.ts \
  --promptfiles drafts/imgs/prompts/01-type-slug.md \
  --image drafts/imgs/01-type-slug.png \
  --provider [provider] --ar [ratio] --quality [quality]

# Batch mode (multiple images)
bun [baoyu-imagine-path]/scripts/main.ts \
  --batchfile drafts/imgs/batch.json
```

For batch mode, generate a `batch.json` file:
```json
{
  "jobs": 4,
  "tasks": [
    {
      "id": "01-infographic-concept",
      "promptFiles": ["prompts/01-infographic-concept.md"],
      "image": "01-infographic-concept.png",
      "ar": "16:9",
      "quality": "2k"
    }
  ]
}
```

**API key setup:** baoyu-imagine manages its own keys via environment variables or `~/.baoyu-skills/baoyu-imagine/EXTEND.md`. If no keys are configured, tell the user:

> To set up image generation, configure your API key:
> - OpenAI: `export OPENAI_API_KEY=sk-xxx`
> - Google: `export GOOGLE_API_KEY=xxx`
> - Replicate: `export REPLICATE_API_TOKEN=xxx`
> - See baoyu-imagine docs for all providers

### Priority 2: MCP Image Tools

Check if image generation MCP tools are available in the current session (e.g., OpenAI image generation MCP, or any tool with "image" + "generate" in its description).

If found, use them directly — Claude can call MCP tools natively.

### Priority 3: Prompt-Only Mode (fallback)

If no image generation tool is available:

1. Still run Steps 1-3 (analyze article, plan, generate prompts)
2. Save all prompts to `drafts/imgs/prompts/`
3. Tell the user:

> Image prompts saved to `drafts/imgs/prompts/`. No image generation tool detected.
>
> To generate images, either:
> - Install baoyu-skills: `git clone https://github.com/JimLiu/baoyu-skills ~/.claude/skills/baoyu-skills`
> - Or copy the prompts to your preferred image generation tool (Midjourney, DALL-E web, etc.)

---

## Process

### Step 1: Analyze Article

Read the article and identify illustration opportunities:

1. **Core arguments** (2-5 main points that benefit from visualization)
2. **Abstract concepts** that are hard to grasp from text alone
3. **Data comparisons** that could be visualized
4. **Narrative moments** with strong visual potential

**Do NOT illustrate:**
- Metaphors literally — if the article says "like cutting a watermelon," visualize the underlying concept, not a watermelon
- Decorative/generic scenes with no informational value
- Points already clear from text

### Step 2: Generate Illustration Plan

Output a plan:

```
Illustration Plan
=================
Article: [title]
Runtime: [baoyu-imagine | MCP tool | prompt-only]
Images: [N] recommended

1. [Position: after paragraph 3]
   Purpose: Visualize the comparison between X and Y
   Type: comparison | infographic | scene | flowchart | framework | timeline
   Key content: [specific data/concepts from the article]

2. [Position: after paragraph 7]
   Purpose: Show the workflow of Z
   Type: flowchart
   Key content: [specific steps]
```

Wait for user approval. They may remove, add, or change items.

### Step 3: Generate Prompts

For each approved illustration, write a prompt file to `drafts/imgs/prompts/NN-type-slug.md`:

```markdown
---
illustration_id: 01-comparison-concept
type: comparison
style: minimal
aspect_ratio: "16:9"
---

[Prompt body here]
```

**Prompt quality rules:**
- **Use ACTUAL article content**: real numbers, real terms, real quotes — not generic placeholders
- **Describe composition spatially**: "left side shows X, right side shows Y, connected by Z"
- **Clean layouts**: generous whitespace, no clutter, solid or subtle gradient backgrounds
- **Characters**: simplified cartoon silhouettes, NOT photorealistic
- **Text in images**: large, prominent, minimal keywords only
- **Language**: match the article's language

**Type-specific structure:**

| Type | Layout | Key Elements |
|------|--------|-------------|
| infographic | Grid/radial/hierarchical | Zones, labels with real data, color-coded sections |
| comparison | Side-by-side with divider | Two contrasting elements, visual separation |
| flowchart | Sequential with arrows | Steps, decision points, connections |
| scene | Single focal point | Atmosphere, mood, color temperature |
| framework | Hierarchical/matrix | Concept nodes, relationships |
| timeline | Chronological | Events with visual markers |

### Step 4: Generate Images

Based on detected runtime (see Runtime Detection above):

- **baoyu-imagine**: Build batch.json → execute batch command
- **MCP tools**: Call the tool for each prompt sequentially
- **Prompt-only**: Skip this step, notify user

Save images to `drafts/imgs/NN-type-slug.png`

If any image fails, continue with the rest. Report failures at the end.

### Step 5: Insert into Article

1. Insert markdown image tags at the planned positions
2. Use relative paths: `![description](imgs/NN-type-slug.png)`
3. Alt text should be a brief meaningful description, NOT the full prompt
4. For prompt-only mode, insert placeholder tags: `![description](imgs/NN-type-slug.png "PENDING — see prompts/")`

Show the final article with images inserted.

## Integration with /ghostink write

Runs as Step 5.6 in the write workflow (after de-AI review, before formatting):

```
... → De-AI Review → Illustrate (optional) → Format → ...
```

## Notes

- Image generation costs money. Always show the plan and get approval before generating.
- Prompts are always saved regardless of generation success — user can re-generate later.
- When using baoyu-imagine batch mode, default to 4 parallel workers.
- Respect rate limits: baoyu-imagine handles this internally; for MCP tools, add 2s delay between calls.
