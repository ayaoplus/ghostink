# Maobidao Demo(write 流程) — Ghostink 端到端示例

这是 ghostink 的 **write 流程**端到端示例,演示从 setup 到产出一篇文章的完整流程。
用猫笔刀的内置三件套(**v2.0-distilled**,基于 35 篇真实文章 distill 出的)作为
起点,模拟一个想"借鉴猫笔刀风格写 AI 领域内容"的用户。

> **注**:三件套已经升级到 2.0-distilled。**怎么 distill 出来的**见姐妹 demo:
> `examples/maobidao-distill-demo/`(distill 流程 demo)。
>
> 本 demo 的 transcript.md / drafts/ 还基于 1.0-draft 三件套写的(P4-T18 时期),
> 后续会用 2.0-distilled 重跑一遍——届时 brainstorm 应能利用更精准的
> Reader Cognitive Model / 7 种 Play 等新维度。

## 目录结构

```
maobidao-demo/
├── README.md          ← 本文件
├── transcript.md      ← setup + write 关键对话脚本
├── notes.md           ← 跑 demo 时的反思与改进建议
└── studio/            ← 模拟的用户工作室(setup 后的状态)
    ├── soul.md        ← maobidao soul + 用户差异化补丁(领域: AI)
    ├── form.md        ← maobidao form 直接复用
    ├── playbook.md    ← 选了日报型 + 热点评论型 + 主题叙事型
    ├── profile/
    │   ├── identity.md       ← 用户的平台 + 读者画像
    │   ├── experiences.md    ← 5 条种子经历
    │   ├── opinions.md       ← 5 条与众不同的观点
    │   └── refs.md           ← 偶尔引用的人 / 书 / 工具
    └── drafts/
        ├── _brainstorm/
        │   └── 2026-04-24-ai-junior-dev.md   ← brainstorm 产出的 skeleton
        └── 2026-04-24-ai-junior-dev.md       ← 最终成稿
```

## 如何理解这个 demo

1. **看 transcript.md** — 看 setup 和 write 真实的对话长什么样
2. **看 studio/soul.md / form.md / playbook.md** — 看 setup 完成后用户档案是什么
3. **看 drafts/_brainstorm/...** — 看 brainstorm 怎么把模糊主题变成结构化骨架
4. **看 drafts/2026-04-24-ai-junior-dev.md** — 看在猫笔刀 form 下写出的成品文章

## 如何复刻这个 demo

```bash
# 1. 切到一个空目录
cd ~/test-studio

# 2. 跑 setup,选猫笔刀整套
/ghostink setup
# → Step 2 选 A,选 maobidao
# → Step 3 用 demo profile 的内容回答(或自己的)
# → Step 5 选 maobidao form

# 3. 写一篇
/ghostink write "AI 编程对初级程序员的影响"
# → Step 1 选 热点评论型
# → Step 2 brainstorm 锐化判断到"AI 让初级程序员更值钱,不是相反"
# → Step 3-7 走完
```

## 这个 demo 的人物设定

- **写作领域**:AI 编程工具 / 软件工程
- **平台**:公众号 + X
- **读者画像**:跨界爱好者 / 开发者社群,工作日通勤场景
- **用户的差异化点**:有"带过实习生"的具体经历,理解初级开发者的真实处境

注:demo 中的 profile 是虚构的,代表一个典型的"借鉴猫笔刀写 AI 内容"用户。
真实使用时按你自己的 profile 填。

## 反思

跑这个 demo 过程中发现的流程问题、卡顿点、改进建议见 `notes.md`。
