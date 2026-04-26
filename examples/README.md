# Ghostink Examples

两个端到端 demo,展示 ghostink 的两条核心流程。

## 两个 demo 的关系

| Demo | 角度 | 演示流程 |
|---|---|---|
| **[maobidao-distill-demo/](./maobidao-distill-demo/)** | "怎么造工具" | `/ghostink distill` — 从 727 篇真实文章提炼 Soul/Form/Playbook 三件套 |
| **[maobidao-demo/](./maobidao-demo/)** | "怎么用工具" | `/ghostink setup` + `/ghostink write` — 用提炼好的三件套写一篇文章 |

```
真实历史文章库
     ↓
  /ghostink distill maobidao --deep
     ↓
  Soul + Form + Playbook  ← maobidao-distill-demo 演示这一段
  v2.0-distilled
     ↓
  built-in/library/{souls,forms,playbooks}/maobidao.md
     ↓
  /ghostink setup → 选 maobidao 整套
     ↓
  /ghostink write [topic]
     ↓
  drafts/YYYY-MM-DD-xxx.md  ← maobidao-demo 演示这一段
```

## 推荐看法

**第一次了解 ghostink**:先看 `maobidao-demo/`(直观,看怎么用),再看
`maobidao-distill-demo/`(深一层,看背后怎么造)。

**想为自己提炼一份三件套**:重点看 `maobidao-distill-demo/`,7 个 phase 日志
逐步展示选样 → 提炼 → 多轮迭代 → finalize 的完整过程。

**想立刻用内置作家写文**:直接看 `maobidao-demo/transcript.md`,看 setup 和
write 真实对话长什么样。

## 状态

| Demo | 三件套版本 | 完成度 |
|---|---|---|
| maobidao-distill-demo | 2.0-distilled(本 demo 直接产出) | ✓ 完整 7 phase + Phase 3 外部 predict-compare |
| maobidao-demo | 2.0-distilled(全部内容已重写) | ✓ transcript / drafts / notes / README 全部对齐新 spec |
