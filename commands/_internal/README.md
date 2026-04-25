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
| `deai-pass.md` | `write` Step 5 / `deai` 命令 | 去 AI 感扫描(出报告,不阈值通过) |

## 调用规范

主命令通过 `Read` 工具加载本目录文件,然后按其中描述的流程执行。
修改这些文件会同时影响所有调用它们的主命令的行为。

## 暴露给用户的 thin wrapper

`form-check` 和 `deai-pass` 同时支持作为独立用户命令调用:

- `/ghostink check [file]` → `commands/check.md`(thin wrapper)→ 调 `_internal/form-check.md`
- `/ghostink deai [file]` → `commands/deai.md`(thin wrapper)→ 调 `_internal/deai-pass.md`

wrapper 只做参数透传,逻辑全在 `_internal/`,维护点单一。
