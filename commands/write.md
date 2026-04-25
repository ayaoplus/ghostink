# /ghostink write

写一篇文章的主路径。从主题到可发布文稿,内部分阶段引导。

## 用法

```
/ghostink write [topic]
/ghostink write "AI 编程对初级程序员"
/ghostink write --material ./refs/         # 提供素材文档夹
/ghostink write --skeleton ./drafts/_brainstorm/2026-04-24-xxx.md   # 基于已有 skeleton
```

## 总体流程

```
Step 0  输入解析 + studio 完整性检查
Step 1  选 playbook
Step 2  brainstorm → skeleton (调 _internal/brainstorm.md)
Step 3  draft (调 _internal/draft.md)
Step 4  form-check (调 _internal/form-check.md,出报告)
Step 5  deai-pass (调 _internal/deai-pass.md,出报告)
Step 6  illustrate (可选,调 commands/illustrate.md)
Step 7  platform-output
Step 8  profile 自动追加
```

每步之间有"用户确认"检查点,避免黑盒。

---

## Step 0 — 输入解析

1. 解析命令行:
   - `topic`:主题字符串(可选,缺省走 brainstorm Branch C "我不知道写啥")
   - `--material <dir>`:素材文档夹路径(可选)
   - `--skeleton <file>`:基于已有 skeleton 直接进 Step 3(可选)

2. 按 SKILL.md Studio Discovery 规则解析 studio root

3. 完整性检查:
   - 缺 `soul.md` → 提示"先跑 /ghostink setup",停
   - 缺 `form.md` → 提示"用 /ghostink library pick form <name> 选一个,或跑 setup",停
   - 缺 `playbook.md` → 提示先建 playbook,停
   - 缺 `profile/identity.md` → 警告但继续("生成的文章可能缺少个人化细节")

4. 如果 `--skeleton` 指定:
   - Read 该文件,提取 frontmatter(topic / play / status)
   - 跳到 Step 3 直接 draft

---

## Step 1 — 选 playbook

加载 `playbook.md`,提取所有 plays。

### 1.1 主题特征预判(自动)

基于 topic 字符串特征推荐一个 play(打分排序):
- 含日期 / "今天" / "本周" → 日报型
- 含具体事件名 / 新闻关键词 → 热点评论型
- 含"如何 / 怎么做 / 教你" → 教程型
- 含个人代词 / 时间副词 → 主题叙事型
- 否则 → 杂文型 或 干货长论型(看长度暗示)

### 1.2 AskUserQuestion 让用户确认

> 这个话题(<topic>)我倾向 **<推荐 play>**,因为 <一句理由>。
>
> 你的 playbook 还有:
> - A) <推荐 play>(推荐)
> - B) <其他 play 1>
> - C) <其他 play 2>
> - D) 我想用一个新 play(走 build-playbook 添加)

用户选了 → 记下 `chosen_play`。

---

## Step 2 — brainstorm → skeleton

调用 `commands/_internal/brainstorm.md`。

输入:
- topic
- soul.md(读 0.x 节用于挑战框架)
- chosen_play(决定结构模板)
- profile/(选素材的弹药库)
- material(若有)

输出:`drafts/_brainstorm/YYYY-MM-DD-<slug>.md`,含:
- 核心判断
- 差异化切入
- 可用素材
- 大纲(按 chosen_play 模板填)
- 对话摘要

中间和用户的对话:
- 锐化判断(对照 soul 的 0.4 信念)
- 挑战框架(soul 0.2 的人设悖论作为视角武器)
- 选差异化角度(profile/opinions 中的 differentiator 优先)
- 选素材(profile/experiences 中相关条目)
- 出大纲

完成后呈现给用户:"这个骨架你满意吗?要改哪里?"
用户确认或具体修改后,进 Step 3。

**回退入口**:如果 Step 0 检测到 `--skeleton` 参数,跳过本步直接进 Step 3。

---

## Step 3 — draft

调用 `commands/_internal/draft.md`。

输入:
- skeleton(从 Step 2 或 --skeleton 来)
- form.md(写作主依据:句式、词汇、开头结尾)
- profile/identity.md(背景信息)

输出:草稿文本,**不写文件**,先给用户看。

写作约束:
- 严格按 skeleton 的大纲走
- 严格遵守 form 的 must_use / never_use / forbidden 词表
- 严格遵守 form 的 prohibition list
- 长度按 chosen_play 的 length 范围

呈现:
> 这是初稿:
>
> [全文]
>
> 接下来自动跑 form-check 和 deai-pass,我会出报告。

---

## Step 4 — form-check(自动出报告)

调用 `commands/_internal/form-check.md`。

输入:
- 当前草稿
- form.md
- chosen_play(用于结构合规)

输出:报告含:
- Score 总分(/100)
- Structure 检查(开头/分隔/类型合规/收尾)
- Vocabulary 扫描(逐行列出 never_use 命中)
- Voice 检查(个人判断数 / 自嘲存在 / 加粗使用)
- Prohibition 扫描(成语堆叠 / 排比 / 教化等)

**不强制修复**。报告呈现后:

> 你想怎么处理?
> - A) 全部修(我自动按报告修一遍)
> - B) 选择性修(指定哪些条要改)
> - C) 跳过,这版我接受

用户选 A → 自动修复并显示修订版。
选 B → 让用户具体说。
选 C → 进 Step 5。

---

## Step 5 — deai-pass(自动出报告,**不阈值通过**)

调用 `commands/_internal/deai-pass.md`。

输出报告含:
- 9 条规则各自的 PASS/FAIL/WARN
- 总分(参考用,不强制阈值)
- 具体行号 + 建议改写

**不强制修复**,处理同 Step 4:

> 你想怎么处理?
> - A) 全部修
> - B) 选择性修
> - C) 跳过,我决定保留这版

---

## Step 6 — illustrate(可选)

> 要不要给文章配图?
> - A) 要(走 illustrate 流程)
> - B) 不要,直接进发布步骤

如果 A → 调用 `commands/illustrate.md`,完成后回 Step 7。

---

## Step 7 — platform-output

> 这篇要发到哪?(应用对应平台格式)
> - A) 公众号(rich formatting,保留 ** 和 ……)
> - B) X / Twitter 长文(去 ** 标记,…… 转空行,字数控制)
> - C) X / Twitter thread(拆成 280 字推文 + 编号)
> - D) 微博(去 markdown,纯文本)
> - E) 纯 markdown(原样输出)
> - F) 其他(自由输入)

应用对应格式规则,写到 `drafts/YYYY-MM-DD-<slug>.md`。

同时生成 5 个候选标题,按 form / soul 的 emotional_contract 选标题风格。

---

## Step 8 — profile 自动追加

扫描本次会话:
- Step 2 brainstorm 中用户提到的新经历(不在 experiences.md 里的)
- Step 2 中用户表达的新观点(不在 opinions.md 里的)
- Step 5/6 反馈中用户透露的新偏好

如有 ≥1 条:

> 我注意到这次会话里出现了一些新素材:
> - <新经历 1>
> - <新观点 2>
>
> 加进 profile 吗?
> - A) 全加
> - B) 选哪些
> - C) 不加

用户选 A/B → 按 E### / O### 编号格式追加到对应 profile 文件。

---

## 实现备注

- form-check 和 deai-pass **只出报告,不阈值通过**——决定权在用户
- 中途用户说"重写""换个角度""不要这段"时,回到对应 step 的具体子步,不要清空重做整篇
- 不要每步都问 AskUserQuestion 轰炸,只在分叉点和"用户确认"检查点问
- 中文叙述,关键术语保留英文
- 写完 Step 7 之前不写最终文件,只用 stdout 呈现草稿
