# P4-T19:清理过时引用

## 目标

全仓库扫描清理所有指向旧架构的字符串和路径,确保重构后没有"幽灵引用"残留。

## 依赖

- 前置:T17 完成(README 已重写)
- 推荐:T1-T16 全部完成后再做(避免重复清理)

## 输入

- 全仓库 grep 结果

## 输出

### 1. 清理目标关键词清单

执行 `git grep` 检查并清理以下字符串:

| 关键词 | 替换方向 | 备注 |
|---|---|---|
| `style-init` | `library analyze` | 命令引用 |
| `style-forge` | `setup`(对外) / `_internal/forge-soul`(对内) | 命令引用 |
| `style-evolve` | (删除) | 已砍命令 |
| `brainstorm` 命令引用 | `write` 内部 Step 2 / `_internal/brainstorm` | 看上下文 |
| `profile-init` 命令引用 | `setup` Step 1+3 | 看上下文 |
| `Layer 0` / `Layer 1` / `Layer 2` / `Layer 3` | `Soul` / `Form` / `Playbook` / `Platform` | 概念替换 |
| `Author DNA` | `Soul` | 概念替换 |
| `Voice Core` | `Form` | 概念替换 |
| `Domain Adaptation` | `Playbook` 或上下文判断 | 概念替换 |
| `Platform Adaptation` | `Platform`(独立配置) | 概念替换 |
| `style_spec.md` | `soul.md` / `form.md` / `playbook.md`(看上下文) | 文件路径 |
| `style_spec_template.md` | `soul.template.md` / `form.template.md` / `playbook.template.md` | 模板路径 |
| `analyzed_authors/` | `library/` | 路径替换 |
| `author_profile/` | `profile/` | 路径替换 |

### 2. 例外清单(允许保留)

以下位置允许保留旧术语,因为是**历史决策记录或迁移指南**:
- `DESIGN.md` 第 4.6 节(命令收敛对照表)
- `DESIGN.md` 第 12 节(决策记录)
- `tasks/` 任务文档(本次重构的工作记录)
- `templates/_deprecated/style_spec_template.md`(已弃用模板)
- `SKILL.md.v1.bak`(若存在,T1 备份)
- `git log` / commit messages(无法改)

### 3. 路径检查

确认仓库实际不存在以下路径:
- ~~`commands/style-init.md`~~
- ~~`commands/style-forge.md`~~
- ~~`commands/style-evolve.md`~~
- ~~`commands/profile-init.md`~~
- ~~`commands/brainstorm.md`~~
- ~~`templates/style_spec_template.md`~~(应在 `_deprecated/`)

### 4. SKILL.md 命令清单核对

最后扫一遍 SKILL.md,确保命令清单只有:
- `/ghostink setup`
- `/ghostink write`
- `/ghostink library`(及子命令)
- `/ghostink check`
- `/ghostink deai`
- `/ghostink illustrate`

不应出现其他命令。

### 5. 用户工作室引导

在 SKILL.md 或 README_CN.md 加一段"从旧版迁移"的简短指引(可选):

```markdown
## 从旧版迁移

如果你之前使用过 ghostink 的旧版本(含 style-init / style-forge 等命令),
重构版本的迁移路径:

1. 旧 `style_spec.md` → 拆为新版 `soul.md` / `form.md` / `playbook.md`
   - 简便方式:重新跑 `/ghostink setup`
   - 高级方式:手动按新 schema 拆分(参考 templates/)
2. 旧 `analyzed_authors/[X]/style_spec.md` → 新版 `library/{souls,forms,playbooks}/[X].md`
3. `author_profile/` → `profile/`(直接 mv)
```

## 验收

- [ ] `git grep -i "style-init"` 仅在例外清单位置出现
- [ ] `git grep -i "style-forge"` 仅在例外清单位置出现
- [ ] `git grep -i "style-evolve"` 仅在例外清单位置出现
- [ ] `git grep "Layer 0"` `"Layer 1"` `"Layer 2"` `"Layer 3"` 仅在例外位置
- [ ] `git grep "analyzed_authors"` 仅在例外位置
- [ ] `git grep "author_profile"` 仅在例外位置(profile/ 替代)
- [ ] SKILL.md 命令清单只含 6 个一级命令
- [ ] 旧命令文件全部不存在(除 _deprecated/)
- [ ] 迁移指引段落已加(SKILL.md 或 README_CN.md)

## 工作量

0.5 天

## 备注

- 这是收尾任务,做这个任务时如果发现某处文档逻辑断裂,**回到对应任务**修,不要在本任务里硬补
- grep 时注意大小写(`Layer` vs `layer`、`Style` vs `style`)
- DESIGN.md 是"历史决策记录",其中的旧术语**不要清**——那是为了未来回溯当时的判断
