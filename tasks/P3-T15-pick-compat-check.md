# P3-T15:实现 pick 兼容性检查

## 目标

在 `library pick` 命令中实现 soul/form/playbook 切换时的兼容性检查逻辑。三档结果:同源安静通过 / 兼容安静通过 / 不兼容警告。**软警告不强禁**。

## 依赖

- 前置:T6(library 命令)、T14(派系词表)完成

## 输入

- `DESIGN.md` 第 8.3 节(兼容性检查时机)
- `built-in/library/factions.md`(派系词表)
- `commands/library.md`(已含 pick 子命令骨架,本任务补完)

## 输出

### 1. 在 `commands/library.md` 的 pick 子命令补全检查逻辑

伪代码 / 文档形式描述(实际由 LLM 在运行时执行):

```
当用户执行 /ghostink library pick {type} {name}:

1. Read 目标文件 [studio]/library/{type}s/{name}.md 或
                  built-in/library/{type}s/{name}.md
   提取 frontmatter:
   - factions
   - compat_*
   - incompat_*

2. Read studio 根的当前文件:
   - 若 type=form,读 soul.md 取 factions
   - 若 type=soul,读 form.md 取 factions(及 playbook.md)
   - 若 type=playbook,读 soul.md 和 form.md

3. 计算兼容档位:
   a. 同源套件:目标文件的 name 与当前 studio 中 soul/form/playbook 任一同名
      → "你正在切换到同一作者的 X,兼容性最好,继续吗?(默认是)"

   b. 兼容:目标 factions ⊂ 当前 X 的 compat_* 集合
      → 安静通过,直接替换

   c. 不兼容:目标 factions 与当前 X 的 incompat_* 集合有交集
      → 弹警告:
        "你正在切换 [type] 到 [name]。
         检测到兼容性问题:
           你当前的 [other_type] 是 [factions]
           [name] 的 incompat_[other_type] 包含 [冲突项]
         可能后果:[基于派系冲突的具体描述]
         继续吗?
           A) 继续(我知道自己在做什么)
           B) 取消
           C) 看看哪些 [type] 跟我的 [other_type] 兼容"

   d. 中性(既不在 compat 也不在 incompat):安静通过,但 stderr 提示"未明确兼容"

4. 用户选 A 或兼容直通时:
   - 备份当前文件到 .ghostink_backup/{type}.{timestamp}.md
   - 复制目标文件到 studio 根的 {type}.md
   - 提示成功

5. 用户选 C 时:
   - 输出兼容列表(扫所有 library 文件,筛 compat_X 含当前 factions 的)
   - 用户选其中一个继续 pick
```

### 2. 警告文案模板(写到 commands/library.md)

具体的提示文本,确保中文自然、不啰嗦:

```
⚠ 兼容性警告

你想把 form 换成 [王小波]。
当前 soul 是 [陪伴派 + 思辨派](来自 catblade)。
王小波 form 标记 incompat_soul: [陪伴派, 抒情派]

可能问题:
  陪伴派 soul + 反讽 form
  → 文章可能感觉一边在温柔陪伴,一边在冷笑反讽

继续吗?
  A) 继续(我想试试反差搭配)
  B) 取消
  C) 看看哪些 form 跟我的 soul 兼容
```

### 3. 测试用例(写到 commands/library.md 末尾,作为文档示例)

3 个具体场景:

| 场景 | 当前 soul | pick form | 期望结果 |
|---|---|---|---|
| 同源 | catblade | catblade | 安静通过 |
| 兼容 | catblade(陪伴派) | sanmao(抒情)…等等不对 | 应改成"中性"或"不兼容" |
| 不兼容 | sanmao(抒情派) | wangxiaobo form(反讽) | 弹警告 |

(任务执行时根据 P2 实际三件套数据调整测试用例)

## 验收

- [ ] commands/library.md 的 pick 子命令含完整兼容性检查描述
- [ ] 3 档结果(同源 / 兼容 / 不兼容)处理路径清晰
- [ ] 警告文案模板已写入文档
- [ ] 备份机制明确(`.ghostink_backup/` 路径与命名规则)
- [ ] 不兼容时 A/B/C 三选项的行为定义
- [ ] 至少 3 个测试用例文档化

## 工作量

0.5 天

## 备注

- 这是**纯文档/prompt 任务**,实际由 LLM 在运行时按描述执行
- 检查算法用集合操作即可,不复杂——关键在文案和用户体验
- 警告不是吓退,是提醒;一定保留"我知道我在做什么"的快捷通道
