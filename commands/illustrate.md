# /ghostink illustrate

Generate illustrations for an article. Can run as a standalone command on any article, or as part of the `/ghostink write` workflow.

## Usage

```
/ghostink illustrate [article-file]
/ghostink illustrate                   ← will ask for input
```

## Configuration

On first run, create `studio/illustrate_config.md` if it doesn't exist:

```yaml
# Image Generation Settings
provider: gpt                    # gpt | nanobanana | seedance | replicate | dashscope | custom
model: gpt-image-1               # model name (provider-specific)
api_key_env: OPENAI_API_KEY      # environment variable name holding the API key
aspect_ratio: 16:9               # default aspect ratio
quality: standard                # standard | hd | 2k
output_dir: imgs                 # subdirectory relative to article for images
watermark:
  enabled: false
  content: ""                    # @username or custom text

# Style Preferences
default_style: minimal           # minimal | warm | notion | blueprint | editorial | watercolor | screen-print
language: auto                   # zh | en | auto (match article language)
```

If baoyu-skills is installed (check for `~/.claude/skills/baoyu-skills/`), inform the user:
> "baoyu-article-illustrator is available. Want to use it instead? It has more advanced features."

If the user agrees, delegate to baoyu-article-illustrator and skip the rest of this workflow.

## Process

### Step 1: Analyze Article

Read the article and identify illustration opportunities:

1. **Find core arguments** (2-5 main points that benefit from visualization)
2. **Find abstract concepts** that are hard to grasp from text alone
3. **Find data comparisons** that could be visualized
4. **Find narrative moments** that have strong visual potential

**Skip illustrating:**
- Metaphors literally (if the article says "like cutting a watermelon," don't draw a watermelon)
- Decorative/generic scenes with no informational value
- Points that are already clear from text

### Step 2: Generate Illustration Plan

Output a plan to the user:

```
Illustration Plan
=================
Article: [title]
Images: [N] recommended

1. [Position: after paragraph 3]
   Purpose: Visualize the comparison between X and Y
   Type: comparison
   Key content: [specific data/concepts from the article]

2. [Position: after paragraph 7]
   Purpose: Show the workflow of Z
   Type: flowchart
   Key content: [specific steps from the article]

...
```

Wait for user approval. They may:
- Remove items ("skip #2")
- Add positions ("also add one after paragraph 5")
- Change types ("make #1 a scene instead")

### Step 3: Generate Prompts

For each approved illustration, generate an image prompt following these principles:

**Prompt structure:**
```
[Layout description]: [composition details]
[Content zones]: [specific elements from the article — real data, real terms, not placeholders]
[Color palette]: [2-3 colors with hex codes, semantically meaningful]
[Style]: [matching the configured default_style]
[Aspect ratio]: [from config]
```

**Prompt quality rules:**
- Use ACTUAL content from the article: real numbers, real terms, real quotes
- Describe composition spatially: "left side shows X, right side shows Y, connected by Z"
- Keep layouts clean: generous whitespace, no clutter, no complex backgrounds
- If characters appear: simplified cartoon silhouettes, NOT photorealistic
- Text in images: large, prominent, minimal, keyword-focused
- Match the article's language

Save prompts to `studio/drafts/imgs/prompts/NN-type-slug.md`

### Step 4: Generate Images

For each prompt:
1. Read the provider config from `studio/illustrate_config.md`
2. Call the appropriate image generation API:

**GPT (OpenAI):**
```bash
# Use the images generation endpoint via the API
```
Note: For OpenAI image generation, use the available MCP tools or API calls.

**NanoBanana / Replicate / Others:**
If baoyu-imagine CLI is available, use it:
```bash
baoyu-imagine --provider [provider] --model [model] --prompt-file [prompt-file] --output [output-path] --ar [aspect-ratio]
```

If baoyu-imagine is not available, inform the user:
> "To generate images, you need either an image generation MCP tool or baoyu-imagine CLI. You can also copy the prompts from `studio/drafts/imgs/prompts/` and generate images manually."

3. Save generated images to `studio/drafts/imgs/NN-type-slug.png`

### Step 5: Insert into Article

After all images are generated:
1. Insert markdown image tags at the planned positions
2. Use relative paths: `![description](imgs/NN-type-slug.png)`
3. Image description (alt text) should be a brief, meaningful description — not the full prompt

Show the user the final article with images inserted.

## Integration with /ghostink write

When running as part of the write workflow, `/ghostink illustrate` runs between Step 5 (Revise) and Step 6 (Format):

```
... → Step 5: Revise → Step 5.5: Illustrate (optional) → Step 6: Format → ...
```

After the draft is finalized, ask:
> "Want to add illustrations? (/ghostink illustrate)"

If yes, run the illustration workflow on the current draft.

## Notes

- Image generation costs money. Always show the plan and get approval before generating.
- If generation fails for one image, continue with the rest and report failures at the end.
- Prompts are always saved regardless of whether generation succeeds — user can re-generate later.
