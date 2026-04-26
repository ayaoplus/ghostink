# /ghostink setup

ghostink 的入门主流程。15-20 分钟交互式向导,产出可立即用于写作的
`soul.md` / `form.md` / `playbook.md` / `profile/`。

## 何时用

- 新人第一次用 ghostink
- 完全重做创作者档案
- 不要用本命令做"小修小补"——那应该直接编辑对应文件

## 用法

```
/ghostink setup
/ghostink setup --reset   # 强制重写,即使已有档案
```

## 总体流程

```
Step 0  状态检测 → 决定追加 / 重写 / 跳过已有
Step 1  平台与读者(写 profile/identity.md)
Step 2  选模仿起点(4 个分支:内置整套 / 多组合 / 自拆 / 纯对话)
Step 3  锐化 profile(对话式,产 experiences + opinions 种子)
Step 4  产出 soul.md
Step 5  选 form
Step 5.5 兼容性总览
Step 6  建 playbook
Step 7  完成
```

任何一步用户可以说"跳过这步""以后再补""换个方向",立即响应不再追问。

---

## Step 0 — 状态检测

按 SKILL.md 的 Studio Discovery 规则解析 studio root,然后:

1. 检查是否存在以下文件,统计状态:
   - `soul.md`、`form.md`、`playbook.md`(三件套)
   - `profile/identity.md`、`profile/experiences.md`、`profile/opinions.md`、`profile/refs.md`

2. 三种状态:
   - **全空**:没有任一文件 → 直接进入新建流程
   - **部分填**:有部分文件 → 询问 AskUserQuestion:
     - A) 追加(保留已有,只填缺失)— 默认推荐
     - B) 重写(清空后从头来过)— 需二次确认
     - C) 跳过已富集的文件,只补缺失
   - **已富集**:三件套齐 + profile 至少 3 条 → 询问:
     - A) 重做(覆盖,需二次确认)
     - B) 我只想补一部分 → 询问补哪部分,跳到对应步
     - C) 取消(其实你不需要 setup,试试 `/ghostink write`)

3. **中断恢复**:本流程任何步骤生成的文件都按既定路径写入。中途中断后再次跑 setup,Step 0 会基于已存在的文件推断进度,提示"上次进行到 Step X,继续 / 重做"。

---

## Step 1 — 平台与读者

写到 `profile/identity.md`。

### 1.1 平台

AskUserQuestion 多选:

> 你主要在哪些平台发文?(影响默认语气与格式)
> - A) 公众号 / 中文长文平台
> - B) Twitter / X 长文或 thread
> - C) 微博
> - D) 个人博客 / Substack
> - E) 其他(自由输入)

### 1.2 内容类型(先导)

这一步决定后续问法的默认假设(避免读者画像维度被默认成职业 / 知识 / 工具型),
也用来给 Step 2 的内置作家清单做预排序。

AskUserQuestion 单选:

> 你写的内容主要属于哪类?
> - A) 生活散文 / 个人记述 — 情感、回忆、日常片段
> - B) 评论 / 思辨 — 观点表达、社会议题、立场鲜明
> - C) 陪伴 / 日常分享 — 跟读者过日子,频率稳定
> - D) 知识 / 教程 / 工具 — 信息密度高,有明确产出
> - E) 混合 / 其他(自由输入)

记下 `chosen_content_type`。后续 Step 2 分支 A 的内置作家清单按此预排序(具体映射见
Step 2)。

### 1.3 读者画像(三维标签)

三个 AskUserQuestion(逐个问,不批量)。所有维度都按"读者来求什么 / 在哪读 / 跟你
什么关系"切,不再按读者的职业身份切——后者只对知识 / 工具型内容成立。

**诉求维度**(读者来求什么,多选):
> 读者来你这里主要求什么?(可多选)
> - A) 情感连接 / 共鸣 — 被理解、被看见
> - B) 信息 / 知识 — 学东西、解决具体问题
> - C) 观点 / 思辨 — 看到不一样的角度
> - D) 陪伴感 — 像跟朋友 / 长辈聊天
> - E) 娱乐放松 — 轻量阅读、消遣

**场景维度**(多选):
> 读者主要在什么场景看你?(可多选)
> - A) 通勤 / 碎片时间(短读为主)
> - B) 睡前 / 深夜(长读 OK,偏放松)
> - C) 工作 / 学习中(目的性强,通常只对知识 / 工具型才有)
> - D) 周末整段时间(深度阅读)

**关系维度**(单选):
> 你和读者的关系是?
> - A) 陌生订阅(他们不认识真实的你)
> - B) 半熟社群(部分读者跟你互动过)
> - C) 朋友圈(小范围,可以放飞)

### 1.4 写到 identity.md

格式:

```markdown
# Identity

## 平台
- <列表>

## 内容类型
<标签>

## 读者画像
- 诉求:<标签>
- 场景:<标签>
- 关系:<标签>

## 备注
<用户自由补充,可空>
```

---

## Step 2 — 选模仿起点

AskUserQuestion 4 个分支:

> 你想从哪里起步?
>
> - A) 从内置库选一个完整套件(适合新人,选了直接全套抄一个作家)
> - B) 多组合(soul / form / playbook 分别从不同作家选)
> - C) 自拆参考(我有想拆的作者,先去 library analyze 拆完回来)
> - D) 纯对话生成(我对自己有清晰认知,不模仿任何人)

### 分支 A:内置整套

按 Step 1.2 的 `chosen_content_type` 给清单预排序,把最贴脸的作家置顶并标 [推荐],
其余按字母顺序。映射:

- A 生活散文 → 三毛(sanmao)
- B 评论 / 思辨 → 王小波(wangxiaobo)、韩寒(hanhan)
- C 陪伴 / 日常 → 猫笔刀(catblade)
- D 知识 / 教程 → 李笑来(lixiaolai)
- E 混合 / 其他 → 不预排序,按字母顺序展示全部

显示内置作家清单(读 `built-in/library/README.md` 中的列表):

> 内置作家(读详情用 `/ghostink library info <name>`):
>
> - **三毛**(sanmao):抒情派 + 陪伴派 — 个人散文 / 长情叙事
> - **王小波**(wangxiaobo):思辨派 + 辛辣派 — 杂文 / 思辨长文
> - **韩寒**(hanhan):辛辣派 + 思辨派 — 博客评论 / 短论
> - **猫笔刀**(catblade):陪伴派 — 日报型公众号 / 财经陪伴
> - **李笑来**(lixiaolai):布道派 + 教程派 — 概念布道 / 框架教学

用户选一个 → 记下 `chosen_author` → 进入 Step 3。

### 分支 B:多组合

依次问 3 个 AskUserQuestion:

1. soul 选谁?
2. form 选谁?(基于 soul 的 compat_form 排序)
3. playbook 选谁?

每问一个,记下 `chosen_soul` / `chosen_form` / `chosen_playbook_source`。

如果用户的搭配不兼容,引用 P3-T15 的兼容性检查警告(见 `commands/library.md` pick 子命令的警告模板)。

### 分支 C:自拆参考

切到 `commands/library.md` 的 `analyze` 子命令流程:

> 你想拆谁?需要准备 5+(quick)或 20+(完整)篇代表作。

完成后回流 setup Step 3,新拆出的作家自动作为 `chosen_author`。

### 分支 D:纯对话生成

跳过 Step 2 的"选参考",直接进入 Step 3,对话式生成。Step 4 也走对话式 forge(不复制内置)。

---

## Step 3 — 锐化 profile(对话式)

写到 `profile/experiences.md` 和 `profile/opinions.md`,目标各 3-5 条种子。

### 3.1 经历种子

> 说 3-5 件对你印象深、可能在文章里引用的真实经历。
> 可以是职业转折、生活片段、尴尬瞬间、觉醒时刻。简短说就行。
>
> 任何时候说"够了""跳过"我立即停。
>
> 先给我第一件。

每条:
1. 提取 2-3 个关键细节 + 1 个情感注脚 + 3-5 个标签
2. 格式化为 E### 编号(扫已有 experiences.md 取最大编号 +1)
3. 显示给用户:"我整理成这样 — [entry]。准吗?"
4. 用户确认后追加,然后问下一件

如果用户卡住,提供启动问题:
- "你人生最被动的一次是什么时候?"
- "你做过最得意的事?"
- "有没有现在想起来特别尴尬的场景?"

### 3.2 观点种子

> 说 3-5 个你和多数人不太一样的观点。不是"AI 很重要"那种没争议的,
> 是说出来会有人反对的那种。

格式:Topic / Position(一句) / Nuance(限定条件)。

启动问题:
- "你看到什么事会忍不住想喷一句?"
- "什么主流做法,你觉得 10 年后会被证明错了?"
- "什么类型的人被高估了?谁被低估了?"

### 3.3 与已选 soul 的对比拉差异(分支 A/B/C)

如果 Step 2 选了 chosen_author,在每条 opinion 录入后悄悄对比:

> 我注意到你说的 [观点] 和 [chosen_author] 在 [话题] 上的立场不一样
> ([chosen_author] 倾向 X,你倾向 Y)。这可能是你的差异化点 — 要标记出来吗?

用户选标记 → 在 opinion 条目末尾加 `[differentiator: vs <chosen_author>]`。

---

## Step 4 — 产出 soul.md

### 路径 A:基于内置参考(分支 A 或 B)

1. Read `built-in/library/souls/<chosen_soul>.md`
2. 复制 6 节内容到 studio 根的 `soul.md`
3. **应用 profile 差异化补丁**:
   - 在 0.4 Core Beliefs 节插入 Step 3.3 标记为 `differentiator` 的 opinions
   - 在 0.5 Trust Mechanics 节,根据 experiences.md 中的"暴露弱点"类条目调整 vulnerability 描述
   - 在 0.6 Emotional Contract 节,基于 identity.md 的读者关系调整"读者来求什么"
4. 把 frontmatter 的 `name` 改为用户的 slug(询问 AskUserQuestion 让用户起个名)
5. `version: 1.0-draft`,`source: ai-distilled`
6. 完整显示给用户,逐节确认

### 路径 B:对话式 forge(分支 C 或 D)

调用 `commands/_internal/forge-soul.md`。

参数:
- `chosen_author`(可空,分支 D 时为空)
- `profile`(已生成的 identity / experiences / opinions)
- `factions_vocabulary`(从 `built-in/library/factions.md` 读)

forge-soul 内部走 6 节对话生成,产出后写入 `soul.md`。

---

## Step 5 — 选 form

### 5.1 列出候选

加载 `soul.md` 的 factions。扫描 `built-in/library/forms/` 与 `[studio]/library/forms/`(若有),按以下顺序:

1. **同源 form**(name 与 chosen_author 一致)→ 顶部标 [推荐]
2. **兼容 form**(`compat_soul` 含 soul.factions 任一项)→ 中部
3. **中性 form**(既不 compat 也不 incompat)→ 下部
4. **不兼容 form**(`incompat_soul` 含 soul.factions)→ 默认隐藏

### 5.2 AskUserQuestion 选择

> 给你的 soul 选个 form:
>
> - A) [推荐 - 同源] <chosen_author> 的 form
> - B) [兼容] <other_author> 的 form — <一句区别说明>
> - C) [兼容] <...>
> - D) 看不兼容选项(可能造成风格冲突)
> - E) 跳过(暂不选,以后用 `/ghostink library pick form` 选)

### 5.3 处理选择

- 选 A/B/C:复制对应 form 到 studio 根的 `form.md`
- 选 D:展开不兼容列表,应用 `commands/library.md` pick 子命令的警告流程
- 选 E:不写 form.md,提示"运行 write 之前必须 pick 一个 form"

---

## Step 5.5 — 兼容性总览(可选检查点)

显示当前搭配 + 询问是否继续:

```
你当前的搭配:

  Soul:  <name>(<factions>)
  Form:  <name>(<factions>)— 兼容性:[同源 / 兼容 / 中性 / 不兼容]
  Playbook: 待生成

继续吗?
  A) 确认,进 Step 6
  B) 重选 form
  C) 退到 Step 4 重选 soul
```

---

## Step 6 — 建 playbook

调用 `commands/_internal/build-playbook.md`。

逻辑摘要:

1. 加载 `soul.md` 的 `compat_playbook` 字段
2. 扫描 `built-in/library/playbooks/` 中 `plays` 字段含 compat_playbook 任一项的文件
3. 推荐显示:

```
基于你的 soul(<factions>),推荐的打法:

  ☑ 日报型(catblade 的 playbook 含此 play)— 高频例行写作
  ☑ 热点评论型(catblade)— 大事件评论
  ☐ 教程型(lixiaolai)— ⚠ 与你的 soul 偏离较大
```

4. 用户多选确认 → 从对应内置 playbook 文件抽取 play 段落,合并写入 `[studio]/playbook.md`
5. 用户也可"自定义新 play",对话式补充

---

## Step 7 — 完成

显示状态摘要:

```
✓ Setup 完成

你的创作者档案:
  Soul:     <name>(<factions>)        ← studio_root/soul.md
  Form:     <name>(<factions>)        ← studio_root/form.md
  Playbook: <plays>                   ← studio_root/playbook.md
  Profile:
    - identity.md (1 条)
    - experiences.md (<N> 条)
    - opinions.md (<N> 条)
    - refs.md (空,以后写文章时会自然增加)

下一步:
  /ghostink write [topic]   开始写第一篇
  /ghostink library list    看看库里还有什么
```

---

## 实现备注

- 全程用 AskUserQuestion 呈现选择题,不要用裸文本提问让用户自由输入(除非明确需要,如"你的领域是什么")
- 每一步都有"跳过"选项,且跳过后保持流程可恢复
- 任何写文件操作前显示给用户确认,除非用户已明确同意"自动写"
- 中文叙述,关键术语保留英文
- 不内嵌 forge-soul / build-playbook 的具体逻辑——它们在 `_internal/`
