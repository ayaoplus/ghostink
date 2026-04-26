# 内部命令

本目录的命令文件**不会**出现在用户的 `/ghostink` 命令清单中。它们是被主命令
(setup / write / library)调用的子流程。

## 文件清单

| 文件 | 被谁调用 | 职责 |
|---|---|---|
| `forge-soul.md` | `setup` Step 4 | 通过对话生成用户的 soul.md |
| `build-playbook.md` | `setup` Step 6 | 基于 soul 推荐 plays + 用户增删 |
| `brainstorm.md` | `write` Step 2 | 出 skeleton(角度/结构/递进/收尾) |
| `draft.md` | `write` Step 3 | 用 form 把 skeleton 写成文 |
| `form-check.md` | `write` Step 4 / `check` 命令 | 文笔合规审查(出报告) |

## 调用规范

主命令通过 `Read` 工具加载本目录文件,然后按其中描述的流程执行。
修改这些文件会同时影响所有调用它们的主命令的行为。

## 暴露给用户的 thin wrapper

`form-check` 同时支持作为独立用户命令调用:

- `/ghostink check [file]` → `commands/check.md`(thin wrapper)→ 调 `_internal/form-check.md`

> 注:`/ghostink deai` 已经是**一级独立命令**,不再走 thin wrapper / _internal
> 模式。完整 spec 直接在 `commands/deai.md`,write Step 5 直接 Read 该文件
> 调用其扫描逻辑。
