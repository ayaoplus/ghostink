# P1-T7:砍掉 evolve 命令

## 目标

删除 `style-evolve` 命令文件,把其三个 mode 的功能拆解到其他位置。释放命令空间,降低用户认知负担。

## 依赖

- 前置:T6 完成(分散去向已就绪)

## 输入

- 现有 `commands/style-evolve.md`(待删)
- `DESIGN.md` 第 4.5 节(砍掉的命令说明)、第 10 节(二阶段功能列表)

## 输出

### 1. 删除文件
- `commands/style-evolve.md`

### 2. 验证三个 mode 的去向

| 原 Mode | 新去处 | 在哪体现 |
|---|---|---|
| Mode A:反馈调整 | 融入 `write` 反馈循环 | T5 的 write.md 应天然支持(用户改稿时自然发生) |
| Mode B:领域迁移 | 用 playbook 替代 | 用户加新 playbook 即可,不需新命令 |
| Mode C:从已发文章学习 | 二阶段实现(`refresh`) | DESIGN 第 10 节已记录 |

### 3. 全仓库引用清理
- grep `style-evolve` 与 `evolve`,清除所有引用
- 重点检查:`SKILL.md`(应已无,T1 已重写)、`README*.md`(P4-T17 处理)、`commands/*.md`(交叉引用)
- DESIGN.md 中保留 evolve 的说明(作为决策记录),不删

### 4. 二阶段记录
确认 `DESIGN.md` 第 10 节"二阶段功能"表中包含:
- evolve / refresh 从已发文章反向学习

(DESIGN.md 里已有此项,本任务只是核对)

## 验收

- [ ] `commands/style-evolve.md` 已删除
- [ ] `git grep -i "style-evolve"` 无结果(DESIGN.md 内的决策记录除外)
- [ ] `git grep -i "/ghostink evolve"` 无结果
- [ ] T5 的 write.md 文档中明确"写后反馈直接更新 spec",承接 Mode A 职能
- [ ] DESIGN.md 第 10 节明确包含 evolve 二阶段计划

## 工作量

0.5 天

## 备注

- 删除前确认 T6 的 library 命令已涵盖 Mode B 的等效操作
- 这是一次纯清理任务,工作量主要在 grep 与交叉验证
