# _internal/build-playbook

基于已选 soul 推荐 plays + 用户增删,产出 `playbook.md`。被 `commands/setup.md` Step 6 调用。

## 输入

- `[studio]/soul.md`(取 factions 与 compat_playbook)
- `built-in/library/playbooks/*.md`(候选 play 来源)
- `[studio]/library/playbooks/*.md`(用户自拆,若有)
- `built-in/library/factions.md`(Playbook 类型词表)

## 输出

`[studio]/playbook.md`,按 `templates/playbook.template.md` schema:

- frontmatter:type=playbook / name=<用户 slug> / version=1.0-draft / source=ai-distilled / plays=[选定的 play 名] / description
- 每个选定的 play 完整结构:trigger / serves_value / emotional_baseline / length / material_from / structure / opening_patterns / closing_patterns

## 流程

### Step 1:扫描候选

读 `soul.md` 的 `compat_playbook`(如 `[日报型, 主题叙事型]`)。

扫所有 built-in 和用户 library 的 playbook 文件,提取:
- 每个文件的 plays 字段
- 每个 play 在哪个文件的哪个节(便于后续抽取)

按"该 play 是否在当前 soul 的 compat_playbook 中"分两组。

### Step 2:推荐展示

```
基于你的 soul(<factions>),推荐的打法:

兼容(推荐):
  ☑ 日报型(maobidao 的 playbook 含此 play)
     trigger: 工作日例行,当天有大盘行情
     emotional_baseline: 日常陪伴,克制
  ☑ 主题叙事型(maobidao)
     trigger: 想写一段完整的回忆/经历
  ☑ 热点评论型(maobidao)
     trigger: 大事件深度评论

不太兼容(可选):
  ☐ 教程型(lixiaolai)— ⚠ 与你的 soul 偏离较大
  ☐ 杂文型(wangxiaobo)— ⚠ 与你的 soul 偏离较大
```

### Step 3:用户多选

AskUserQuestion 让用户多选要哪些 plays(预选所有"兼容"项)。

用户也可以选"不兼容"项,系统接受不警告(setup 阶段已经做过总览,不重复警告)。

### Step 4:聚合写入

对每个选中的 play:

1. 找到来源文件
2. 提取该 play 的完整段落(从 `## Play: X` 到下一个 `## Play:` 或文件结尾)
3. 拼接到 `[studio]/playbook.md`

frontmatter:
- `plays`:用户选中的 play 名称数组
- `name`:用户的 slug(从 setup Step 4 已确定)
- `description`:基于选中的 plays 生成,如 "陪伴式作者的三种打法:日报、主题叙事、热点评论"

### Step 5:自定义新 play(可选)

> 想补充一个内置库没有的 play 吗?
> - A) 加,我描述一下
> - B) 不用了,这些够

如果 A,询问:
- play 名(从 factions.md 的 Playbook 类型选,或新创但需提示这是临时的)
- trigger
- structure(3 步够了)
- opening / closing 模式(可选)

按 template 格式追加到 playbook.md。

### Step 6:确认

显示完整 `playbook.md` 给用户看,询问"这就是你的 playbook,继续 Step 7 吗?"

## 实现备注

- 不要凭空创造 play 名字——从 factions.md 的 Playbook 类型词表选
- 用户选不兼容 play 时,在该 play 段落开头加 HTML 注释提醒"⚠ 与 soul factions 偏离较大,使用时注意"
- 保持每个 play 段落结构与 template 一致,便于后续 write 命令读取
