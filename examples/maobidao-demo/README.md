# Maobidao Demo(write 流程) — Ghostink 端到端示例

这是 ghostink 的 **write 流程**端到端示例,演示从 setup 到产出一篇文章的
完整流程。用猫笔刀的内置三件套(**v2.0-distilled**,基于 35 篇真实文章
distill 出的)作为起点,模拟一个想"借鉴猫笔刀风格写 AI 领域内容"的用户。

> **怎么造出这套三件套**见姐妹 demo:`examples/maobidao-distill-demo/`
> (distill 流程 demo,展示 7 个 phase 怎么从真实文章提炼出 Soul/Form/Playbook)

## 目录结构

```
maobidao-demo/
├── README.md          ← 本文件
├── transcript.md      ← setup + write 关键对话脚本(基于 2.0-distilled)
├── notes.md           ← 跑 demo 的反思 + 跟 v1.0 时期对比
└── studio/            ← 模拟的用户工作室(setup 后的状态)
    ├── soul.md        ← maobidao soul 2.0-distilled + AI 领域差异化补丁
    ├── form.md        ← maobidao form 2.0-distilled(14 节,跟内置库镜像)
    ├── playbook.md    ← maobidao playbook 2.0-distilled(7 种 Play,选了 4 种)
    ├── profile/
    │   ├── identity.md       ← 平台 + 内容类型 + 诉求/场景/关系新维度
    │   ├── experiences.md    ← 5 条种子经历
    │   ├── opinions.md       ← 5 条与众不同的观点(2 条 differentiator)
    │   └── refs.md           ← AI 领域的人 / 书 / 工具参照系
    └── drafts/
        ├── _brainstorm/
        │   └── 2026-04-24-ai-junior-dev.md   ← brainstorm 产出的 skeleton
        │                                        (含 Form 关键约束 + Cross-Play
        │                                        Prohibitions + Self-Check Checklist)
        └── 2026-04-24-ai-junior-dev.md       ← 最终成稿(1860 字,form-check 92)
```

## 如何理解这个 demo

1. **看 transcript.md** — 看 setup(新流程含核心术语速览 + 内容类型先导 +
   诉求/场景/关系)和 write 的真实对话
2. **看 studio/soul.md / form.md / playbook.md** — 看 setup 完成后的用户档案
   (跟 built-in/library/ 镜像 + AI 领域差异化补丁)
3. **看 drafts/_brainstorm/...** — 看 brainstorm 怎么把模糊主题锐化成
   "AI 把初级程序员分成两类"这种锋利判断,以及如何在 skeleton 阶段就引用
   Form 约束 + Cross-Play Prohibitions + Self-Check Checklist
4. **看 drafts/2026-04-24-ai-junior-dev.md** — 看在 maobidao form 2.0-distilled
   下写出的成品文章(注意 "……" 段间符 / "鸡贼" 反讽 / IDE 历史类比 /
   "就这些吧" 收尾 + 闲话破节奏)
5. **看 notes.md** — 看 v2.0-distilled 用起来的体感 + 跟 v1.0 时期对比

## 如何复刻这个 demo

```bash
# 1. 切到一个空目录
cd ~/test-studio

# 2. 跑 setup
/ghostink setup
# → Step 1.1 选公众号 + X
# → Step 1.2 选 D(知识/教程/工具)— 触发猫笔刀 + 李笑来置顶
# → Step 1.3 诉求选 B+C / 场景选 A+C / 关系选 A
# → Step 2 选 A(整套抄)→ 选 maobidao
# → Step 3 用 demo profile 内容回答(或自己的)
# → Step 5 选 maobidao form
# → Step 6 选 4 种 Play(日报/热点评论/主题深挖/自传年终复盘)

# 3. 写一篇
/ghostink write "AI 编程对初级程序员的影响"
# → Step 1 选 热点评论型(系统会推荐)
# → Step 2 brainstorm 锐化判断到"AI 把初级程序员分成两类"
# → Step 3-7 走完(form-check 14 节验证 + deai-pass + 平台输出)
```

## 这个 demo 的人物设定

- **写作领域**:AI 编程工具 / 软件工程
- **平台**:公众号 + X
- **内容类型**:D 知识 / 工具(主)+ B 评论 / 思辨(副)
- **读者画像**:诉求 = 信息 + 观点;场景 = 通勤 + 工作中;关系 = 陌生
- **用户的差异化点**:有"带过实习生"的具体经历,理解初级开发者真实处境

注:demo 中的 profile 是虚构的,代表一个典型的"借鉴猫笔刀写 AI 内容"用户。
真实使用时按你自己的 profile 填。

## 跟 v1.0-draft 时期的关键差异

| 维度 | v1.0-draft demo (P4-T18) | v2.0-distilled demo(本版) |
|---|---|---|
| transcript 的 setup 流程 | 4 步(平台/读者/起点/补丁) | 7 步 + 核心术语速览 + 内容类型先导 |
| profile/identity 维度 | 身份/场景/关系(3 维) | 内容类型 + 诉求/场景/关系(4 维) |
| Soul 模板 | 6 节,无 Uniqueness | 6 节 + Uniqueness Statement |
| Form 验证 | Sentence + Vocabulary | 14 节 + 真人类比 + 自创词 + 4 级讽刺 |
| Playbook | 3 个 Play | 7 个 Play + frequency + Self-Check |
| draft 风格 | 像"AI 模仿口语化博主" | 像"猫笔刀去写一篇 AI 文章" |

详细对比见 `notes.md`。

## 反思

跑这个 demo 过程中发现的流程问题、卡顿点、改进建议见 `notes.md`。
