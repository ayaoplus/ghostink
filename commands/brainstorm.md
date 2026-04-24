# /ghostink brainstorm

An interactive thinking partner that turns a rough idea into a structured outline — before any writing happens. Dynamic, not fixed questions. Challenges your framing. Produces a `_brainstorm` artifact that `/ghostink write` can consume directly.

## When to use

- You have a topic but not a clear argument
- You know what to write but not how to frame it differently from everyone else
- You want someone to push back on your framing before you commit to 2000 words
- You have raw experience/opinion in `author_profile/` and need help picking what to use

If you already have a sharp thesis, clear article type, and rough outline — skip this. Go straight to `/ghostink write`.

## Prerequisites

- `studio/style_spec.md` exists (required — without it, article type selection has no anchor)
- `studio/author_profile/experiences.md` exists (optional but strongly recommended — enables material suggestion in Step 5)

If `style_spec.md` is missing, tell the user to run `/ghostink style-forge` first and stop.

## Core principles (read before you start)

### How to ask dynamic questions

These rules apply to EVERY question you generate in every step below. If you catch yourself violating them, stop and rewrite the question.

**Forbidden question patterns** (never ask these):

- ✗ Open-ended meta questions: "What do you want to convey?" / "What's your main point?"
- ✗ Abstract audience probes: "Who's your target audience?"
- ✗ Fake deference: "What do you think? / How do you feel about this?"
- ✗ Anything the user could answer with "I don't know, that's why I'm here"

**Required question patterns** (every question must satisfy all four):

- ✓ Must reference a specific word or phrase the user said in a previous turn
- ✓ Must be answerable with yes/no, A-or-B, or picking from a short list
- ✓ Must come with 3-4 concrete candidate answers (use AskUserQuestion; the "Other" option is automatic)
- ✓ Must include a one-line intent: "I'm asking this to help you [X]"

### Convergence rule

**Stop when the step's output is achieved, not when N rounds have passed.** Each step below names its output. When you have it, move on. If after 3 rounds you still don't have the output, surface the problem: "We've gone three rounds and I still don't have a clear [X]. Want to skip this step, or try a different angle?"

### User can interrupt anytime

State this explicitly at the start of Step 1:

> 任何时候你可以说"跳过这步"、"直接出大纲"、"换个方向"，我会立即收敛。

Honor these signals immediately. Do not keep asking "are you sure?"

### Challenge the framing (with guardrails)

At Step 3.5 you will challenge the user's framing. This is the most valuable part of the process, and also the easiest to mess up. Guardrails:

- **Challenge must quote user's own words.** Not "I think you actually want to write about X" — but "You said [exact phrase]. That's not really about [their stated topic], it's about [alternative framing]. Agree / disagree / adjust?"
- **If there's nothing to challenge, skip.** Don't manufacture a challenge for the sake of it. Say: "你的判断已经够锋利了，不需要挑战。" and move on.
- **Offer exactly one alternative framing, not a menu.** A three-way reframe is indecisive and confusing.
- **User always wins.** If they reject the reframe, drop it immediately and stick with their original framing.

---

## Step 0 — Load context

Silently read:

- `studio/style_spec.md` — extract the Layer 0 Author DNA (section 0.x), the defined article types (section 5 or equivalent), the "never use" vocabulary
- `studio/author_profile/identity.md` (if exists)
- `studio/author_profile/experiences.md` (if exists) — just index the entry headers for now, don't dump content
- `studio/author_profile/opinions.md` (if exists)

Do not summarize any of this back to the user. They wrote it, they know what's in it.

## Step 1 — Topic input and clarity triage

Greet briefly. Then ask the user for their topic.

> 你想写什么？一句话说说就行，清楚模糊都没关系。
>
> （任何时候你可以说"跳过这步"、"直接出大纲"、"换个方向"，我会立即收敛。）

Based on their answer, branch:

**Branch A — Clear topic + clear stance** (e.g., "我想写 AI 对初级程序员是机会不是威胁，我想用我带实习生的经验作为素材")

Skip Step 2-4 entirely. Confirm in one sentence, then jump to Step 5 (material alignment) and Step 6 (outline).

**Branch B — Topic but fuzzy stance** (e.g., "我想写写最近 AI 编程工具的事")

Proceed to Step 2 normally.

**Branch C — "I don't know what to write"**

Don't ask them to think harder. Instead: scan `author_profile/experiences.md` and `opinions.md`, surface 3-4 candidate topics that pair a recent experience with a recent opinion. Ask which resonates. After they pick one, treat it as Branch B input and proceed to Step 2.

If `author_profile/` is empty, say so plainly: "你的 author_profile 里还没什么素材，我很难帮你挖话题。要不先聊聊你这周/这个月有什么让你有感而发的事？" Use their answer as the topic and proceed.

## Step 2 — Anchor the article type

**Output required:** one selected article type from the user's `style_spec.md`.

Generate a single AskUserQuestion with:

- Options = the article types defined in the user's `style_spec.md` (typically 2-4 types)
- For each option, include a one-line description of what that type is for, drawn from the spec itself
- Include a "Recommended: [type X]" prefix in the question body, with one-sentence reasoning tied to the user's topic

Example question phrasing (for a user whose spec has 日报型/热点评论型/主题叙事型):

> 你这个话题（"AI 编程工具对初级程序员的影响"）我倾向是 **热点评论型**，因为它是围绕一个行业现象的深度评论，不是日报那种当天行情的扫描，也不是纯个人回忆。你觉得呢？
>
> - A) 热点评论型（推荐）—— 围绕一个事件/现象展开，穿插个人经历佐证
> - B) 主题叙事型 —— 讲完整的个人故事，知识嵌在叙事里
> - C) 日报型 —— 当日信息扫描+点评，你这个不太合适
>
> 问这个是为了锁定结构模板，后面的大纲会按照这个类型的骨架走。

Wait for answer. If user picks a different type than recommended, accept it silently and move on.

## Step 3 — Sharpen the core judgment

**Output required:** one sentence that expresses the user's central judgment. Must be specific enough that a reader could disagree with it. "AI 很重要" is not specific enough. "AI 不会取代初级程序员，反而会让他们更值钱" is.

Ask one dynamic question per round (max 3 rounds). Each question must:

- Quote specific words from the user's earlier answers
- Offer 3-4 candidate judgments as AskUserQuestion options — range from "safe/mainstream" to "sharp/contrarian"
- State the intent: "I'm asking this to help you land on a judgment that's specific enough to be arguable"

After each round, restate what you now understand their judgment to be, and ask if it's accurate. Stop the moment they confirm.

If after 3 rounds the judgment is still vague, surface it: "我们聊了三轮，你的核心判断还是比较泛。要不跳过这步，我们从素材倒推判断？" — then move to Step 5 first, use the material to reverse-engineer a judgment, and come back.

## Step 3.5 — Challenge the framing

**Output required:** either a reframed judgment (if challenge succeeds) OR confirmation that the original judgment stands (if challenge fails or is unnecessary).

Look at everything the user has said in Steps 1-3. Ask yourself:

1. Is their judgment genuinely contrarian, or is it a mainstream take dressed up? If mainstream → there's something to challenge.
2. Did they say something in passing that contradicts or complicates their stated judgment? If yes → that's your challenge seed.
3. Did their chosen article type match the emotional weight of what they're saying? (E.g., they picked 日报型 but keep talking about a specific person — that's really 主题叙事型.)

**If there IS something to challenge, issue exactly one reframe.** Format:

> 打断一下 — 你刚才说了"[引用用户原话]"。这句话其实比你选的判断更有意思。我怀疑你真正想写的不是「[他们说的主题]」，而是「[重新框定的主题]」。
>
> - A) 你说对了，换成这个框架
> - B) 不对，保持原来的
> - C) 部分对，我来调整一下（自由输入）
>
> 问这个是因为 [为什么这个重新框定更锋利]。

**If there is NOTHING legitimate to challenge** (their judgment is already sharp, their framing is coherent, their type fits): say so plainly and skip. Do NOT fabricate a challenge.

> 你的判断已经够锋利了，不需要挑战。继续下一步。

## Step 4 — Differentiating angle

**Output required:** at least one specific angle that makes this article different from the 10 other articles on the same topic.

Ask dynamic questions in this territory:

- "你这个判断，[具体的知名作者/媒体] 最近也在讲类似的。你和他们的差别是什么？"
- "如果读者已经同意你的判断，你还想告诉他们什么他们不知道的？"
- "这个话题里最少被提到的、但你觉得关键的一点是什么？"

Each must follow the forbidden/required patterns from Core Principles.

Provide 3-4 candidate angles per question, drawn from:

- Unique experiences in `author_profile/experiences.md`
- Opinions from `author_profile/opinions.md` that relate
- Counter-examples from their Layer 0 Author DNA (e.g., if their DNA emphasizes 实用主义高于一切, a differentiating angle might be the "你不赚钱讲这些没用" take)

Max 2 rounds. Converge.

## Step 5 — Material alignment

**Output required:** a list of 1-3 concrete items from `author_profile/experiences.md` (or user-supplied in-session) that will appear in the article.

Scan `author_profile/experiences.md` for entries whose tags or keywords overlap with the topic. Surface the top 3-5 candidates as options:

> 你 author_profile 里有这些相关素材，想用哪些？
>
> - A) E012 [带实习生那次] — 你提到的"初级程序员"正好对应
> - B) E024 [自己第一份工作犯的低级错] — 可以做类比
> - C) E031 [看到一个 AI 工具上手门槛的观察] — 很贴合判断
> - D) 都不合适，我有别的素材要补（自由输入）
>
> 问这个是为了让文章有你的真实细节，避免写成"AI 分析报告"。

If `experiences.md` is empty or has no relevant entries, ask the user directly to supply 1-3 experiences/details they want woven in.

After they select, silently check: do the chosen materials actually support the sharpened judgment from Step 3/3.5? If there's a mismatch (e.g., the judgment is "AI 让初级程序员更值钱" but the chosen material is about an AI tool bug), flag it:

> 我注意到你选的 E024 讲的是低级错误，但你的判断是"AI 让初级程序员更值钱"。这两个方向有点拧。要不换素材，要不调整判断 —— 你倾向哪边？

## Step 6 — Skeleton outline

**Output required:** a section-by-section outline following the chosen article type's template (from `style_spec.md`).

Do NOT generate this as a dynamic dialogue. Generate it directly based on:

- The selected article type template (Step 2)
- The sharpened judgment (Step 3/3.5)
- The differentiating angle (Step 4)
- The chosen materials (Step 5)

Output the outline as a clean markdown section. For each section of the article type template, give:

- What goes here (1-2 sentence description)
- Which material (from Step 5) appears here, if any
- Word count estimate (optional)

Present it to the user in one shot. Ask only: "这个骨架你满意吗？要改哪里？"

Accept either "满意" (proceed to Step 7) or specific revision requests (revise in place, re-present, loop max 3 times).

## Step 7 — Write artifact and hand off

**Output required:** a markdown file at `studio/drafts/_brainstorm/YYYY-MM-DD_<slug>.md`.

Slug rules: take the topic, take first 4-6 meaningful words, convert to kebab-case. Strip punctuation. Use Chinese pinyin or keep Chinese characters — match what the user would recognize.

File format:

```markdown
---
date: YYYY-MM-DD
topic: <user's topic as stated>
article_type: <selected type>
status: brainstormed
---

## 核心判断
<one-sentence judgment from Step 3/3.5>

## 差异化切入
<angle from Step 4, 1-3 sentences>

## 可用素材
- [E012 带实习生那次] — <one line on how it's used>
- [E031 AI 工具观察] — <one line>
- [用户新增: xxx] — <one line>

## 大纲
<section-by-section outline from Step 6>

## 对话摘要
<3-5 bullet points capturing the key moves in this brainstorm session — what judgment was sharpened, what framing was challenged (if any), why this angle won over alternatives>
```

After writing, do not announce the file path unless the user asks. Immediately hand off:

> 大纲已存到 `studio/drafts/_brainstorm/YYYY-MM-DD_<slug>.md`。
>
> 现在接着写稿吗？
>
> - A) 是，现在就开始。你运行 `/ghostink write` 然后在第一步说"基于 `studio/drafts/_brainstorm/YYYY-MM-DD_<slug>.md`"，write 会跳过讨论直接按大纲写
> - B) 不，先放着。产物已经保存，随时可以回来
> - C) 骨架我再想想，重开 brainstorm

If A — stop here. The user runs `/ghostink write` manually.
If B — stop.
If C — loop back to Step 1 but load the current artifact as starting point.

---

## Few-shot examples

These are reference dialogues showing what good dynamic brainstorming looks like. They use the catblade style spec (Layer 0 DNA + 日报型/热点评论型/主题叙事型). When generating your own questions, match the specificity and intent transparency of these examples.

### Example 1 — Branch B, 热点评论型, with successful reframe

**User:** 我想写写最近 DeepSeek 开源的事

**Assistant (Step 2):**
> 这个话题我倾向 **热点评论型** —— 围绕一个行业事件展开评论，穿插你的判断和经历。你 spec 里还有日报型和主题叙事型。
>
> - A) 热点评论型（推荐）
> - B) 主题叙事型 —— 除非你想以"我用 DeepSeek 的那天"这种个人视角切入
> - C) 日报型 —— 不太合适，除非你想把它塞进当天其他新闻里一笔带过
>
> 问这个是为了锁结构模板。

**User:** A

**Assistant (Step 3):**
> OK 热点评论型。你对 DeepSeek 开源这事，一句话最想说什么？我给几个候选让你挑或改：
>
> - A) 开源是好事，证明中国 AI 能力
> - B) 开源不是重点，成本结构才是核心冲击
> - C) 美股 AI 巨头估值逻辑被打穿了，这是金融事件不是技术事件
> - D) 都不对（自由输入）
>
> 问这个是想找到一个别人没大声说的判断 —— A 太平了所有人都在说，BC 可能更像你的角度。

**User:** C 接近，但我想说的是"美股崩是果，因在于一个开源社区证明了一件本该被垄断的事"

**Assistant (Step 3.5 — reframe):**
> 打断一下 —— 你刚说"**本该被垄断的事**"。这句话比你选的 C 更有意思。我怀疑你真正想写的不是「DeepSeek 开源的冲击」，而是「垄断叙事是怎么被一个周末拆掉的」。
>
> - A) 对，换这个框架
> - B) 不对，保持原框架
> - C) 部分对，我来调整（自由输入）
>
> 问这个是因为你的判断里真正有锋芒的点是"垄断叙事的崩塌"，而不是"股价下跌"。股价只是结果。

**User:** A

**Assistant (Step 4):**
> 好，核心判断锁定："垄断叙事是怎么被一个周末拆掉的"。
>
> 现在想差异化角度。你这个判断，虎嗅/晚点/硅星人最近也在讲类似的。你和他们的差别可能在：
>
> - A) 你有真金白银在场的视角（你 spec 里的 Layer 0 DNA：一个有钱人的立场）
> - B) 你对"垄断逻辑"的本能怀疑（你 opinions.md 里提过的）
> - C) 你见过上一轮互联网垄断叙事是怎么破的（如果你 experiences 里有）
> - D) 其他（自由输入）
>
> 问这个是因为没有差异化，你的文章会被淹没在 50 篇同主题稿里。

（...后续 Step 5/6/7 省略）

### Example 2 — Branch A, 主题叙事型, no challenge needed

**User:** 我想写一篇回忆，讲我第一次在上海独自过年的经历。基调是孤独但又有一点暖。类型是主题叙事，我想用 E045 那个细节，就是大年三十晚上楼下便利店老板多送我一个茶叶蛋。

**Assistant (straight to Step 5 + confirmation):**
> 收到 —— 主题叙事型，E045 茶叶蛋那段。你的判断/基调已经清楚（孤独里的一点暖），差异化切入也明确（那个具体的茶叶蛋细节）。不需要挑战，我直接给骨架。
>
> 要不要再补 1-2 条相关素材？我看 experiences.md 里还有：
>
> - A) E012 离家前夜母亲收拾行李 —— 可以做开头钩子前的微插叙
> - B) E028 刚到上海时的租房经历 —— 可以做背景铺垫
> - C) 不用了，就用 E045，够了
>
> 问这个是因为主题叙事型通常需要 1-2 个小细节做铺垫，光一个茶叶蛋会显得单薄。

**User:** B，就用 E045 和 E028

**Assistant (Step 6):**
> （直接出大纲，按主题叙事型模板：钩子→展开→回旋）

### Example 3 — Branch C, from blank to topic

**User:** 我不知道写啥

**Assistant (Step 1 branch C):**
> 来帮你挖。翻了你 author_profile，最近可能有料的几个方向：
>
> - A) 你 opinions.md 里上周写的"AI 面试这事荒唐" + experiences.md 新加的 E067（上周那次面试经历）—— 热点评论型
> - B) experiences.md E062 提到的回老家那次 —— 主题叙事型
> - C) 你 opinions 里一直在写但没展开过的"程序员的中年危机是伪命题" —— 热点评论型
> - D) 都不对（告诉我你这周有啥让你有感而发的事）
>
> 问这个是因为从你自己的素材倒推出话题，比凭空想主题容易十倍。

**User:** A 吧，那个面试经历确实憋了一肚子话

**Assistant:** （进入 Step 2，article type 已经隐含是热点评论型，快速确认）

---

## Implementation notes for Claude running this command

- Each AskUserQuestion in Steps 2-5 is a real tool call, not prose. A markdown block labeled "Question:" is not interactive — the user clicks actual AskUserQuestion options.
- Do NOT batch questions. One AskUserQuestion per step (or per round within a step).
- Between steps, do NOT summarize what just happened unless the user asks. They saw it.
- If the user's first message already contains enough to run the whole flow (topic + type + stance + materials), do NOT mechanically go through 7 steps asking them to confirm what they already told you. Skip straight to Step 6 (outline) and Step 7 (artifact).
- Keep the session short. A typical brainstorm should be 8-15 messages total, not 40.
