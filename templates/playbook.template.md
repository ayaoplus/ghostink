<!--
Playbook 模板 — 描述一个作者在不同情境下的固化打法集合(文章类型)。

plays 字段列出本 playbook 包含的所有 play 名称。
每个 play 名称必须从 built-in/library/factions.md 中的"Playbook 类型"词表选。
-->

---
type: playbook
name: <slug-英文小写>
display_name: <中文显示名>
version: 1.0-draft
source: ai-distilled
plays: []                    # 本 playbook 包含的 play 名称数组
description: |
  <1-2 句简介>
---

# Playbook: <作者名>

<!-- 下面是 Play 模板示例。一个 playbook 通常含 2-4 个 plays。
     按需复制 Play 段落并填写。 -->

## Play: <play 名,如"日报型">

- **trigger**:<什么时候用这个 play,如"工作日例行,当天有大盘行情">
- **serves_value**:[<对应 Soul 0.1 的价值类型,如:信息差, 情感陪伴>]
- **emotional_baseline**:<情感基调,如"日常陪伴,克制">
- **length**:<字数范围,如"800-1500">
- **material_from**:[<素材来源,如:当日新闻, 即时反应>]

### Structure

1. <开头部分:做什么>
2. <主体部分:做什么>
3. <收尾部分:做什么>

### Opening Patterns
[<如:新闻扫读, 数据开场, 自嘲>]

### Closing Patterns
[<如:日常感慨, 明日预告>]

---

## Play: <下一个 play 名>

- **trigger**:
- **serves_value**:[]
- **emotional_baseline**:
- **length**:
- **material_from**:[]

### Structure

1.
2.
3.

### Opening Patterns
[]

### Closing Patterns
[]
