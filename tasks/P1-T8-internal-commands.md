# P1-T8:内部命令拆分

## 目标

把原 9 命令拆解,把仍有用的逻辑移到 `commands/_internal/`,作为 setup 和 write 的子流程被调用。删除已被收编/砍掉的命令文件。`_internal/` 不会被 SKILL.md 列为用户入口。

## 依赖

- 前置:T4(setup 命令)、T5(write 命令)、T6(library 命令)完成
- 推荐:T7(砍 evolve)同步进行
- 参考:`DESIGN.md` 第 4.3 节(内部 primitives 列表)

## 输入

- 现有 `commands/*.md`(全 9 个)
- T4 / T5 / T6 产出的新命令文档

## 输出

### 1. 创建 `commands/_internal/` 下的 6 个文件

| 文件 | 来源 | 改动 |
|---|---|---|
| `_internal/forge-soul.md` | 从 style-forge.md 抽 Phase 1-3 的 forge 逻辑 | 改为对话式生成 soul.md,可被 setup Step 4 调用 |
| `_internal/build-playbook.md` | 从 style-forge.md 抽 Phase 3.2 的 article types 部分 | 改为基于 soul 推荐 plays,可被 setup Step 6 调用 |
| `_internal/brainstorm.md` | 从原 brainstorm.md 改写 | 改为接受 topic + soul + playbook + profile 输入,产 skeleton |
| `_internal/draft.md` | 从原 write.md 抽 Step 3 写稿逻辑 | 接受 skeleton + form,产 draft |
| `_internal/form-check.md` | 从原 check.md 改写 | 改为只出报告,不内嵌 fix 逻辑 |
| `_internal/deai-pass.md` | 从原 deai.md 改写 | **去掉自动阈值通过逻辑**,只出报告 + 建议 |

### 2. 删除已被收编的旧文件

- `commands/style-init.md`(功能进入 `library analyze`,T6 已实现)
- `commands/style-forge.md`(功能进入 setup + `_internal/forge-soul.md`)
- `commands/profile-init.md`(功能进入 setup,但单独入口保留——见步骤 3)
- `commands/brainstorm.md`(已迁到 `_internal/`)
- `commands/check.md`(已迁到 `_internal/`,但单独入口保留)
- `commands/deai.md`(已迁到 `_internal/`,但单独入口保留)
- `commands/write.md`(由 T5 重写覆盖,不算删除)

### 3. 保留独立可选入口(在 SKILL.md 边角入口区)

部分 _internal 命令同时暴露给用户作为独立入口(用于"任意文本"场景):

| 用户入口 | 内部位置 | 说明 |
|---|---|---|
| `/ghostink check [file]` | `_internal/form-check.md` | 审任意文本 |
| `/ghostink deai [file]` | `_internal/deai-pass.md` | 任意文本去 AI |
| `/ghostink illustrate [file]` | `commands/illustrate.md`(保留独立) | 配图 |

实现方式:在 `commands/check.md` / `commands/deai.md` 写一个"thin wrapper",调用 `_internal/` 的同名文件。或者:`_internal/` 文件的开头明确"既可被 write 调用也可独立调用",对外通过 SKILL.md 的命令路由。

**推荐方式**:写 thin wrapper 文件,逻辑全在 `_internal/`,wrapper 只做参数透传。这样维护点单一。

### 4. 加 `commands/_internal/README.md`

说明:
- 这是内部命令目录,不会出现在用户的命令清单
- 主命令(setup / write / library)通过 Read 调用这里的文件
- 修改这些文件会同时影响主命令的行为

## 验收

- [ ] `commands/_internal/` 下有 6 个 .md 文件 + README
- [ ] 旧 `commands/style-init.md` 已删
- [ ] 旧 `commands/style-forge.md` 已删
- [ ] 旧 `commands/profile-init.md` 已删
- [ ] 旧 `commands/brainstorm.md` 已删(内容迁到 _internal)
- [ ] `commands/check.md` 改为 thin wrapper(转调 `_internal/form-check.md`)
- [ ] `commands/deai.md` 改为 thin wrapper(转调 `_internal/deai-pass.md`)
- [ ] `commands/illustrate.md` 保留不变
- [ ] `commands/_internal/deai-pass.md` 不再含"70/85 阈值自动通过"逻辑
- [ ] T4(setup.md)与 T5(write.md)中对 `_internal/` 的引用路径都正确
- [ ] SKILL.md 命令清单不包含 `_internal/` 中的 6 个文件

## 工作量

1.5 天

## 备注

- 这是 P1 阶段的"收尾"任务,前面 T4/T5/T6 都依赖它最终成形
- 拆分时不要修改逻辑,只搬运 + 调适入口契约;逻辑改进留到后续迭代
- thin wrapper 的目的是让 SKILL.md 命令路由清晰,不要写成完整副本
