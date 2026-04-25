# P4-T18:端到端 demo

## 目标

用猫笔刀的三件套走完整 setup → write 流程,产出一份示例文章和完整对话记录。这是新人最快理解 ghostink 价值的入口。

## 依赖

- 前置:T4(setup)、T5(write)、T9-T13(5 作家三件套,**特别是 T12 猫笔刀**)、T14-T16(兼容性)
- 参考:整个 ghostink 已重构状态

## 输入

- `built-in/library/{souls,forms,playbooks}/catblade.md`(P2-T12 产出)
- 一个示例主题(建议:近期某个真实但不敏感的话题,如"AI 编程对初级程序员的影响")

## 输出

新建目录 `examples/catblade-demo/`,内容:

### 1. `examples/catblade-demo/README.md`

说明:
- 这是 ghostink 的端到端示例
- 演示从 setup 到产出一篇文章的完整流程
- 如何复刻这个 demo
- 如何把 demo 的成果迁移到自己的 studio

### 2. `examples/catblade-demo/transcript.md`

完整对话记录(伪 transcript,模拟用户与 ghostink 的真实交互):
- setup 7 步的关键对话片段(每步抽 5-10 轮对话)
- write 8 步的关键对话片段
- 用清晰格式标注:`# Step` / `> User:` / `< Assistant:`

### 3. 模拟产出的 studio 状态

`examples/catblade-demo/studio/` 下:

```
studio/
├── soul.md          ← 从 catblade 借,加用户 profile 差异化
├── form.md          ← catblade form
├── playbook.md      ← 含 日报型 + 热点评论型
├── profile/
│   ├── identity.md
│   ├── experiences.md   (3-5 条编造但合理的经历)
│   ├── opinions.md
│   └── refs.md
├── drafts/
│   ├── _brainstorm/
│   │   └── 2026-04-24_ai-junior-dev.md   ← skeleton
│   └── 2026-04-24_ai-junior-dev.md       ← 成稿
```

### 4. 成稿质量要求

`drafts/2026-04-24_ai-junior-dev.md` 应:
- 长度 1500-2500 字
- 体现 catblade 风格(短中句、口语化、个人判断、自嘲)
- 包含 1-2 个具体素材(从 profile/experiences.md 来)
- 通过 form-check 报告(无重大违规)
- 通过 deai-pass 报告(分数合理)
- **关键**:**自己读一遍,如果不像猫笔刀写的就重做**

### 5. 反思笔记

`examples/catblade-demo/notes.md` 写下:
- 走 demo 时发现的 setup / write 流程问题(如有)
- 哪些步骤体感顺,哪些卡顿
- 下一版改进建议

## 验收

- [ ] `examples/catblade-demo/` 目录就位
- [ ] README.md 说明清楚 demo 用法
- [ ] transcript.md 完整记录 setup + write 关键对话
- [ ] studio/ 目录含完整 4 件套 + drafts
- [ ] 成稿满足风格 + 长度 + 素材要求
- [ ] notes.md 含至少 3 条流程反思
- [ ] **用户主观判断**:让没用过 ghostink 的人看完 demo,能否理解整个工具的价值?

## 工作量

1 天(含跑流程 + 收集 transcript + 反思)

## 备注

- 这是**最贵也最有用**的任务,直接决定新人对 ghostink 的第一印象
- 不要"为了 demo 而 demo"——按真实使用场景跑,卡住的地方就是产品要修的地方
- transcript 不必逐字记录,但要让读者感受到工具的对话风格
- 跑出来的成稿质量不达标 = 内置 catblade 三件套有问题(回到 P2-T12 修)
