# P0-T3:编写 soul / form / playbook 三个 schema 模板

## 目标

为 soul、form、playbook 三类核心资源各产出一份模板文件,定义 frontmatter 字段、章节结构、内容字段。后续:

- `setup` 命令产出用户的 soul/form/playbook 时按此模板填
- 内置库 5 个作家的三件套(P2 任务)按此模板写
- `library analyze` 命令拆解新作者时按此模板输出

## 依赖

- 前置:**T2 必须完成**(目录结构就位)
- 参考:`DESIGN.md` 第 8 节(兼容性标签)、第 9 节(数据 Schema)

## 输入

- `DESIGN.md` 第 9.1 节(soul.md 结构)
- `DESIGN.md` 第 9.2 节(form.md 结构)
- `DESIGN.md` 第 9.3 节(playbook.md 结构)
- `DESIGN.md` 第 8.2 节(派系标签词表)

## 输出

3 个模板文件 + 1 个迁移操作。

### 1. `templates/soul.template.md`

frontmatter 字段:
```yaml
---
type: soul
name: <slug,英文小写>
display_name: <中文显示名>
version: 1.0-draft       # 1.0-draft / 1.0-quick / 2.0
source: ai-distilled     # ai-distilled / user-analyzed
factions: []             # Soul 派系标签
compat_form: []
compat_playbook: []
incompat_form: []
incompat_playbook: []
description: |
  <1-2 句简介>
---
```

正文章节(对应 DESIGN 9.1):
- `## 0.1 Reader Value`(核心价值主张 + 价值类型表 + 取关测试)
- `## 0.2 Positioning`(角色 / 权力关系 / 人设悖论 / 自我定位信号)
- `## 0.3 Narrative Engine`(说服栈 / 论断模式 / 错误处理 / 故事角色)
- `## 0.4 Core Beliefs`(信念列表 + 冲突时谁赢 + 盲区)
- `## 0.5 Trust Mechanics`(行为表 / 脆弱模式 / 破信任红线)
- `## 0.6 Emotional Contract`(读者来求什么 / 永不做什么 / 锚点角色 / 情感范围)

每个字段下方用 HTML 注释 `<!-- 填什么 -->` 解释。

### 2. `templates/form.template.md`

frontmatter 字段:
```yaml
---
type: form
name: <slug>
display_name: <中文显示名>
version: 1.0-draft
source: ai-distilled
factions: []             # Form 派系标签
compat_soul: []
compat_playbook: []
incompat_soul: []
description: |
  <1-2 句简介>
---
```

正文章节(对应 DESIGN 9.2):
- `## Persona Voice`(写作人格 + 核心张力 + 脆弱维度)
- `## Sentence Rhythm`(短句比例 / 节奏型 / 长句容忍度)
- `## Vocabulary Fingerprint`
  - `### Must Use`(口语化倾向词表)
  - `### Never Use`(书面化禁用词表)
  - `### Forbidden`(0 次出现的绝对禁用词)
- `## Opening Patterns`(类型 + 频率 + 示例表)
- `## Closing Patterns`(类型 + 适用类型 + 示例表)
- `## Prohibition List`(排比 / 总结段 / 教化 / ...)
- `## Emotional Expression Toolkit`(情绪 + 表达手法 + 例句表)
- `## Signature Expressions`(招牌表达 + 使用场景 + 频率表)

### 3. `templates/playbook.template.md`

frontmatter 字段:
```yaml
---
type: playbook
name: <slug>
display_name: <中文显示名>
version: 1.0-draft
source: ai-distilled
plays: []                # 本 playbook 包含的 plays 名称数组
description: |
  <1-2 句简介>
---
```

正文示例 1 个 play 完整结构,后续可复制扩展:

```markdown
## Play: <play 名,如"日报型">

- **trigger**:<什么时候用这个 play,如"工作日例行,当天有大盘行情">
- **serves_value**:[信息差, 情感陪伴]
- **emotional_baseline**:<情感基调>
- **length**:<字数范围>
- **material_from**:[当日新闻, 即时反应]

### Structure

1. <开头部分>
2. <主体部分>
3. <收尾部分>

### Opening Patterns
[新闻扫读, 数据开场]

### Closing Patterns
[日常感慨, 明日预告]
```

### 4. 派系标签词表(写在每份模板顶部的 HTML 注释里,作为可选值参考)

```markdown
<!--
派系标签词表(factions 字段可选值):

Soul 派系:陪伴派 / 思辨派 / 教程派 / 抒情派 / 辛辣派 / 布道派
Form 派系:口语化 / 书面化 / 反讽 / 抒情 / 短句轰炸 / 长句分析
Playbook 类型:日报型 / 主题叙事型 / 热点评论型 / 教程型 / 杂文型 / 干货长论型

使用建议:每个文件 factions 字段填 1-3 个标签;
compat_X 和 incompat_X 字段填上面对应类型的标签数组。
-->
```

### 5. 旧模板迁移

- 把现有 `templates/style_spec_template.md` 移动到 `templates/_deprecated/style_spec_template.md`
- 用 `git mv` 保留历史
- 在 `templates/_deprecated/` 加一份 README.md 说明:"这里存放已弃用但保留作参考的模板"

### 6. 不动的内容

- `templates/author_profile/*` 保留原状(profile 部分不需要改 schema)

## 验收标准

- [ ] 3 个新模板文件已创建并放在 `templates/` 根目录
- [ ] 每份模板的 frontmatter 字段完整,与上面规格一致
- [ ] 每个正文字段有 HTML 注释说明填什么内容
- [ ] 派系标签词表写在每份模板顶部的 HTML 注释里
- [ ] 章节顺序与 DESIGN 9.x 节定义一致
- [ ] 旧 `style_spec_template.md` 已迁到 `_deprecated/`(用 `git mv`)
- [ ] `templates/_deprecated/README.md` 已创建说明用途
- [ ] 模板说明用中文,frontmatter 字段名英文,正文章节标题英文(与 DESIGN 一致)
- [ ] 一个新手仅看模板就能知道每个字段填什么,不必反复回看 DESIGN

## 工作量

0.5 天

## 备注

- 模板要"可填",不要"全空白":每个字段下面给 1-2 行示例(用注释或斜体)
- frontmatter 中的数组类型字段(如 `factions: []`)即使为空也保留 `[]`,不要省略
- 模板正文章节标题保持英文(与 DESIGN 9.x 一致),正文内容/说明用中文
