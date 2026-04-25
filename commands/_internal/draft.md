# _internal/draft

把 skeleton 变成完整草稿。被 `commands/write.md` Step 3 调用。

## 输入

- skeleton(从 brainstorm 来的 `drafts/_brainstorm/YYYY-MM-DD-xxx.md`)
- `[studio]/form.md`(写作主依据)
- `[studio]/playbook.md`(找 chosen_play 的 structure / opening / closing 模式)
- `[studio]/profile/identity.md`(读者画像 + 平台暗示)

## 输出

完整草稿文本(纯文本字符串,不写文件 — write Step 3 拿到后给用户预览)。

## 写作约束

### 1. 严格按 skeleton 大纲走

skeleton 的"## 大纲"节定义了文章结构。逐节展开,不要重排顺序、不要漏节、不要加节(除非用户在 brainstorm 后明确要求)。

### 2. 严格遵守 form.md 的所有规则

逐项加载:
- **Persona Voice**:写作人格 + 张力 + 脆弱维度——这是文字背后的"角色"
- **Sentence Rhythm**:短句比例、节奏型、最长长句限制
- **Vocabulary Fingerprint**:
  - 主动用 must_use 词(每 200-300 字至少 1 次)
  - 完全避开 never_use 词
  - 绝对避开 forbidden 词
- **Opening Patterns**:从 form.md 列的几种开头模式中选一种
- **Closing Patterns**:同上
- **Prohibition List**:每条都不出现
- **Emotional Expression Toolkit**:对应情绪用对应手法
- **Signature Expressions**:在合适位置插入(每篇 2-3 处)

### 3. 严格按 chosen_play 的尺度

playbook.md 的对应 play:
- length:控制字数在范围内
- emotional_baseline:整体情感基调
- opening_patterns / closing_patterns(若与 form 冲突,以 play 为准 — play 是更具体场景)

### 4. 素材落地

skeleton "## 可用素材"列出的每条 E### / O### / 新增,在指定位置嵌入,**用具体细节而非概要**:
- 错:"我之前带过实习生,有一次..."
- 对:"E012 那次,小张第一周连 git rebase 都不敢按回车..."

不发明 profile 里没有的细节。

## 流程

1. Read skeleton,提取:topic、play、核心判断、差异化切入、可用素材、大纲、对话摘要

2. Read form.md,把 8 节规则放进上下文

3. Read playbook.md 的对应 play 段落,把 structure / 模式 放进上下文

4. Read profile/identity.md(平台 + 读者画像影响语气)

5. **逐节生成草稿**(按 skeleton 大纲):
   - 每节先生成,自检:
     - 用了 must_use 词?
     - 命中任何 never_use / forbidden?
     - 句长比例对不对?
     - 有 prohibition list 中的模式吗?
   - 检过再放下一节

6. **整体扫一遍**:
   - 开头是 form 列的某种 opening pattern?
   - 结尾是 form 列的某种 closing pattern?
   - 有 2+ Signature Expressions 出现?
   - 字数在 play 的 length 范围内?
   - 整体情感基线对吗?

7. 输出整篇文本(不写文件)

## 实现备注

- **不要内嵌"自动跑 form-check"**——那是 write Step 4 的职责。本步只生成,不审查
- 写作过程中如果发现 skeleton 与 form 有冲突(如 skeleton 大纲含"总结段"但 form prohibition 禁止),**优先 form**(因为 form 是 IP 的稳定特征,skeleton 是单篇的临时产物)。同时在输出末尾用 HTML 注释提醒用户冲突
- 不要为了"完美"修饰得过分——保留人写的瑕疵
- 输出纯文本,不要包裹任何 Code Fence
