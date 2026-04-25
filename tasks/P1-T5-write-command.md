# P1-T5:重写 write 命令

## 目标

把原 `brainstorm` + `write` + `check` + `deai` 串成单一 `/ghostink write` 命令的 8 步流程。这是 ghostink 重构后另一个主入口,日常写稿的主路径。

## 依赖

- 前置:T1, T2, T3 完成
- 推荐:与 T4 并行,T8 同步进行
- 参考:`DESIGN.md` 第 5.2 节(write 流程详解)

## 输入

- `DESIGN.md` 第 5.2 节(8 步流程规格)
- 现有 `commands/brainstorm.md`(Step 2 的 skeleton 阶段沿用其逻辑)
- 现有 `commands/write.md`(参考多步引导模式)
- 现有 `commands/check.md`(form-check 阶段沿用)
- 现有 `commands/deai.md`(deai-pass 阶段沿用,但**改为出报告不强制**)

## 输出

重写后的 `commands/write.md`,8 步流程:

### Step 0 — 输入解析
- 命令行接受 `[topic]` 与 `--material <dir>`
- 检测 studio 是否完整(soul/form/playbook 都在)
- 不全则提示先跑 setup

### Step 1 — 选 playbook
- 从 `playbook.md` 列出可用 plays
- 默认按主题特征推荐一个
- AskUserQuestion 让用户确认或换

### Step 2 — brainstorm → skeleton
- 调用 `_internal/brainstorm.md`
- 沿用现有 brainstorm 的对话模式(锐化判断 + 挑战框架 + 选素材)
- 产出 `drafts/_brainstorm/YYYY-MM-DD-xxx.md`
- 用户确认骨架
- **支持"基于已有 skeleton 文件直接进入 Step 3"的回退入口**

### Step 3 — draft
- 调用 `_internal/draft.md`
- 加载 `form.md`,按 skeleton 出文
- 输出给用户预览

### Step 4 — form-check(自动出报告)
- 调用 `_internal/form-check.md`
- 词表 / 句式 / 开头结尾合规扫描
- 报告 PASS/FAIL,**不强制修**
- 询问:全部修 / 选择性修 / 跳过

### Step 5 — deai-pass(自动出报告,不设阈值)
- 调用 `_internal/deai-pass.md`
- 9 条规则扫描,出报告含分数 + 具体建议
- **不强制修,不阈值通过**
- 询问:全部修 / 选择性修 / 跳过

### Step 6 — illustrate(可选)
- 询问是否配图
- 是 → 调用 `commands/illustrate.md`

### Step 7 — platform-output
- 选发布平台(公众号 / X 长文 / X thread / markdown)
- 应用对应格式规则
- 写入 `drafts/YYYY-MM-DD-xxx.md`

### Step 8 — profile 自动追加
- 扫描本次会话提到的新经历/观点
- 询问是否加进 `profile/experiences.md` / `opinions.md`

## 验收

- [ ] 8 步流程与 DESIGN 5.2 一一对应
- [ ] 每步之间有"用户确认"检查点(避免黑盒)
- [ ] 支持从已有 `_brainstorm/*.md` 直接进 Step 3
- [ ] form-check 和 deai-pass **只出报告**,改不改由用户决定
- [ ] 不强制阈值通过,不强制修复
- [ ] Step 0 检测 studio 不全时给出明确指引(先跑 setup)
- [ ] 中文叙述
- [ ] 不内嵌 brainstorm / draft / form-check / deai-pass 的实现细节,只调用引用

## 工作量

1 天

## 备注

- 这是 ghostink 日常使用的高频路径,体验对老用户更敏感
- 沿用现有 brainstorm.md 的"动态问题"原则——见原文档"Core principles"段
- 不要让 8 步变成 8 个 AskUserQuestion 轰炸,只在分叉处问
