# P1-T4:实现 setup 命令

## 目标

实现 `/ghostink setup` 命令文档,作为新人入门的主流程。把原 `profile-init` / `style-init` / `style-forge` 的核心逻辑整合成一个 15-20 分钟的交互式向导。

## 依赖

- 前置:T1, T2, T3 全部完成
- 推荐:T8(内部命令拆分)同步进行,因 setup 调用 `_internal/forge-soul.md` 和 `_internal/build-playbook.md`
- 参考:`DESIGN.md` 第 5.1 节(setup 流程详解)

## 输入

- `DESIGN.md` 第 5.1 节(7 步流程规格)
- `DESIGN.md` 第 2 节(核心概念)
- 现有 `commands/profile-init.md`(参考交互模式)
- 现有 `commands/style-forge.md`(参考 forge 思路)
- 内置库 5 作家简介(P2 完成后才有,先用 placeholder 占位)

## 输出

新文件 `commands/setup.md`,7 步流程:

### Step 0 — 状态检测
- 三种状态:全空 / 部分填 / 已富集
- 已富集时:追加 / 重写 / 跳过已有
- 中断恢复:检测已生成文件,从该步继续

### Step 1 — 平台与读者
- AskUserQuestion 平台多选(公众号 / X / 微博 / Substack / 其他)
- AskUserQuestion 读者画像三维标签(身份 / 场景 / 关系)
- 写入 `profile/identity.md`

### Step 2 — 选模仿起点
4 个分支:
- A. 内置库整套(列出 5 作家 + 一句简介)
- B. 多组合(soul/form/playbook 分别选)
- C. 自拆参考(跳到 `library analyze` 子流程,完成后回流)
- D. 纯对话生成(不模仿)

### Step 3 — 锐化 profile
- 个人经历问询(3-5 条 experiences)
- 不同观点问询(3-5 条 opinions)
- 与已选参考 soul 对比拉差异

### Step 4 — 产出 soul.md
- A 路径:复制内置 soul + profile 差异化补丁
- B/C/D:对话式 forge,逐节确认

### Step 5 — 选 form
- 默认 = Step 2 已选作者的 form
- 列出 compat_soul 含当前 factions 的 form 候选
- 对不兼容选择给警告(参见 P3-T15)

### Step 6 — 建 playbook
- 基于 soul 推荐 plays
- 用户增减
- 写入 `playbook.md`

### Step 7 — 完成
- 摘要展示已生成的 soul/form/playbook/profile 状态
- 引导:`/ghostink write`

## 验收

- [ ] 7 步流程与 DESIGN 5.1 一一对应
- [ ] 每步有"跳过 / 以后再补"选项
- [ ] AskUserQuestion 形式呈现选择题(不是裸文本提问)
- [ ] Step 0 状态检测逻辑明确(怎么判定"已富集")
- [ ] Step 2 四个分支收敛回 Step 3 的路径清楚
- [ ] Step 4 A 路径与 B/C/D 路径分别处理
- [ ] 中途中断可恢复(从已生成文件推断进度)
- [ ] 中文叙述,关键术语保留英文
- [ ] 不内嵌 forge-soul / build-playbook 的实现细节,只调用引用

## 工作量

1 天

## 备注

- ghostink 重构后用户体验最关键的命令,新人 80% 第一印象在这里
- 写完后自己当一次新人跑一遍,体感不顺就改
- setup.md 是"流程编排",不是"具体执行"——具体逻辑放 `_internal/`
