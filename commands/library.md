# /ghostink library

库管理。子命令:`list` / `info` / `analyze` / `pick`。

## 用法

```
/ghostink library list                        看库存
/ghostink library list --include-incompat     连不兼容也看
/ghostink library info <name>                 看某个详情
/ghostink library analyze <author> [opts]     拆一个新参考
/ghostink library pick <type> <name>          切换 soul/form/playbook
```

`<type>` 取值:`soul` / `form` / `playbook`。

---

## list 子命令

### 用途
列出所有可用的 soul / form / playbook,标版本徽章 + 当前在用标记 + 兼容性。

### 流程

1. 扫描以下两处:
   - `built-in/library/{souls,forms,playbooks}/*.md`(内置)
   - `[studio]/library/{souls,forms,playbooks}/*.md`(用户自拆,若存在)

2. Read 每个文件的 frontmatter(只读 frontmatter,不读正文,节约成本)

3. 加载当前 studio 根的 `soul.md` / `form.md` / `playbook.md` 的 frontmatter 作为对照

4. 输出表格(分三块:soul / form / playbook),每行含:
   - Name(slug)
   - Display name(中文)
   - Version(`1.0-draft` 加 ⚠,`2.0` 推荐)
   - Source(built-in / user-analyzed)
   - Factions
   - 兼容性状态(同源 ✓ / 兼容 ✓ / 中性 / 不兼容 ⚠ / 当前在用 ★)
   - Description

5. 默认隐藏不兼容项,加 `--include-incompat` 显示

6. 末尾给提示:用 `info <name>` 看详情,用 `pick` 切换

### 输出示例

```
Soul Library
============

Name        Display    Version     Source         Factions          Status        Description
catblade    猫笔刀     1.0-draft⚠ built-in       陪伴派,思辨派    ★ 在用         有钱人的陪伴式财经
sanmao      三毛       1.0-draft⚠ built-in       抒情派,陪伴派    ✓ 兼容         流浪叙事 + 浓情
wangxiaobo  王小波     1.0-draft⚠ built-in       思辨派,辛辣派    ⚠ 不兼容       反讽 + 理性
                                                                                  (用 --include-incompat 显示)

Form Library
============
...

Playbook Library
================
...
```

---

## info 子命令

### 用途
看某个 soul / form / playbook 的详情。

### 流程

1. 接受 `<name>`,在 built-in 和用户 library 中查找匹配
   - 同名优先用户 library(若存在)覆盖内置
   - 找不到 → 提示"未找到,用 list 看清单"

2. 输出:
   - frontmatter 全部字段
   - 类型标签(soul / form / playbook)
   - description
   - 章节标题列表(只标题,不读正文 — 节约成本)
   - 与当前 studio 的兼容性
   - 提示用 `pick <type> <name>` 切换

3. 询问"要看完整内容吗?" → 是则 Read 整文输出。

---

## analyze 子命令

### 用途
拆一个新参考,产出 soul / form / playbook(默认三件套)。

### 用法

```
/ghostink library analyze <author> [--path <dir>] [--only=soul|form|playbook] [--quick]
```

### 选项

- `<author>`:作家 slug(英文小写),用作输出文件名
- `--path <dir>`:文章目录路径。缺省时按以下顺序找:
  1. `[studio]/library/articles/<author>/`
  2. 询问用户输入路径
- `--only=X`:只拆其中一项,跳过其他
- `--quick`:启用 quick 模式(5+ 篇文章,版本标 `1.0-quick`)

### 流程

#### Phase 0:准备
1. 解析 path,统计文章数
2. 数量判断:
   - `--quick` + ≥5 篇 → 进入 quick 模式
   - 默认 + ≥20 篇 → 进入完整模式
   - 不足 → 警告并询问继续?

3. 创建输出目录:`[studio]/library/{souls,forms,playbooks}/`

#### Phase 1:Soul 提取(若 --only ≠ form/playbook)

1. 选 10-15 篇最大多样性的样本(quick 模式选 3-5 篇)
2. 走 6 节挖掘(对应 soul template 的 0.1-0.6)
3. 写入 `[studio]/library/souls/<author>.md`,version 按模式

#### Phase 2:Form 提取(若 --only ≠ soul/playbook)

1. 选 15-20 篇(quick 选 5+)
2. 提取声音核心(template 的 8 节)
3. 词频分析(LLM 估算,quick 模式精度更低)
4. 写入 `[studio]/library/forms/<author>.md`

#### Phase 3:Playbook 提取(若 --only ≠ soul/form)

1. 文章聚类(按结构相似度)
2. 为每类定义 play
3. 写入 `[studio]/library/playbooks/<author>.md`

#### Phase 4:兼容性标签自动填充

1. 基于 soul.factions 推 form 的 compat_soul / incompat_soul
2. 基于 soul.factions 推 playbook plays
3. 写到 frontmatter

#### Phase 5:确认与提示

```
拆解完成 ✓
  - souls/<author>.md(version: 1.0-quick / 2.0)
  - forms/<author>.md
  - playbooks/<author>.md

要立刻 pick 这个 form/soul 吗?
- A) pick 整套(soul + form + playbook 同时切换)
- B) 只 pick form
- C) 都不,以后再说
```

---

## pick 子命令

### 用途
切换当前在用的 soul / form / playbook。

### 用法

```
/ghostink library pick soul <name>
/ghostink library pick form <name>
/ghostink library pick playbook <name>
```

### 流程

1. 在 `built-in/library/<type>s/` 与 `[studio]/library/<type>s/` 中查找 `<name>.md`
   - 找不到 → 提示并停

2. Read 目标文件 frontmatter

3. Read 当前 studio 根的对应文件(如果是 pick form,读 soul.md 和 playbook.md;如果是 pick soul,读 form.md 和 playbook.md)

4. **兼容性检查**:

   a. **同源套件**(目标文件的 name 与当前 studio 中 soul/form/playbook 任一同名):
   ```
   你正在切换到同一作者的 X(<author>)。同源套件兼容性最好。
   继续吗?(默认 是)
   ```

   b. **兼容**(目标 factions ⊆ 当前 X 的 compat_*):
   - 安静通过

   c. **中性**(既不在 compat 也不在 incompat):
   - 安静通过,但提示"未明确兼容"

   d. **不兼容**(目标 factions ∩ 当前 X 的 incompat_* ≠ ∅):

   ```
   ⚠ 兼容性警告

   你想把 <type> 换成 <name>。
   当前 <other_type> 是 <factions>(来自 <other_name>)。
   <name> 标记 incompat_<other_type>: <冲突项>

   可能问题:
     <基于派系冲突的具体描述,如"陪伴派 soul + 反讽 form
     → 文章可能感觉一边温柔陪伴,一边冷笑反讽">

   继续吗?
     A) 继续(我想试试反差搭配)
     B) 取消
     C) 看看哪些 <type> 跟我的 <other_type> 兼容
   ```

   - 用户选 C → 显示兼容列表(扫所有 library 文件,筛 compat_X 含当前 factions 的)

5. **备份当前文件**(用户选继续后):
   - 复制 `[studio]/<type>.md` 到 `.ghostink_backup/<type>.<YYYYMMDD-HHMMSS>.md`

6. **复制目标到当前**:
   - 把 `built-in/library/<type>s/<name>.md` 或 `[studio]/library/<type>s/<name>.md` 内容复制到 `[studio]/<type>.md`

7. 提示成功:

   ```
   ✓ 已切换 <type> 为 <name>
   备份在 .ghostink_backup/<type>.<timestamp>.md(可恢复)
   ```

### 警告文案模板

```
⚠ 兼容性警告

你想把 form 换成 [王小波]。
当前 soul 是 [陪伴派, 思辨派](来自 catblade)。
王小波 form 标记 incompat_soul: [陪伴派, 抒情派]

可能问题:
  陪伴派 soul + 反讽 form
  → 文章可能感觉一边在温柔陪伴,一边在冷笑反讽

继续吗?
  A) 继续(我想试试反差搭配)
  B) 取消
  C) 看看哪些 form 跟我的 soul 兼容
```

### 测试用例(文档化)

| 场景 | 当前 soul | pick form | 期望 |
|---|---|---|---|
| 同源 | catblade | catblade | 安静通过(同源套件) |
| 兼容 | catblade | hanhan | 走兼容性比对,可能中性或不兼容(看 factions) |
| 不兼容 | sanmao(抒情派) | wangxiaobo(反讽) | 弹警告 A/B/C |

---

## 实现备注

- 全程用 LLM 读 frontmatter + 比对集合,不需要脚本
- 备份机制必须可靠 — 用户切错了能 1 步恢复
- 警告语气避免吓退,保留"我知道我在做什么"的快捷通道
- 切换 soul 影响最大(灵魂换了相当于换了人),备份尤其重要
- 中文叙述,frontmatter 字段名英文
