# P1-T6:实现 library 命令(及 4 个子命令)

## 目标

实现 `/ghostink library` 主命令及 `list` / `info` / `analyze` / `pick` 4 个子命令,作为 ghostink 重构后第三个一级入口。负责库存查看、新参考拆解、文笔/灵魂切换。

## 依赖

- 前置:T1, T2, T3 完成
- 参考:`DESIGN.md` 第 5.3 节、第 8 节(兼容性机制)

## 输入

- `DESIGN.md` 第 4.2 节(library 子命令清单)
- `DESIGN.md` 第 5.3 节(library 操作流程)
- `DESIGN.md` 第 8 节(兼容性机制)
- 现有 `commands/style-init.md`(analyze 子命令复用其核心逻辑)

## 输出

新文件 `commands/library.md`,4 个子命令:

### `library list`
- 扫描 `built-in/library/` 与 studio `library/`(若存在)
- 输出表格:类型 / 名称 / 版本 / 来源 / 当前是否在用
- 版本标签显式区分:`1.0-draft` / `1.0-quick` / `2.0`(`1.0-draft` 加 ⚠ 标注)
- 标识当前在用的 soul/form/playbook(对照 studio 根目录的同名文件)

### `library info [name]`
- Read 对应的 soul/form/playbook 文件
- 输出 frontmatter 摘要 + 章节标题 + description
- 显示兼容性:跟当前 studio 的 soul/form 是否兼容

### `library analyze [author] [--path <dir>] [--only=soul|form|playbook] [--quick]`
- 沿用现有 style-init 的提取逻辑
- **完整模式(默认)**:20+ 篇文章,产出 `version: 2.0`
- **quick 模式(`--quick`)**:5+ 篇文章,产出 `version: 1.0-quick`,标记精度有限
- `--only=X` 可指定只拆其中一项
- 产出到 `[studio]/library/{souls,forms,playbooks}/[author].md`
- 完成后询问:要立即 pick 这个 form / soul 吗?

### `library pick {soul|form|playbook} [name]`
- 从 `built-in/library/` 或 studio `library/` 找对应文件
- **兼容性检查**(参见 P3-T15):
  - 同源套件(都是 catblade 的三件套):安静通过
  - 跨源但兼容:安静通过
  - 不兼容:警告并询问 A 继续 / B 取消 / C 看兼容选项
- 复制到 studio 根的 `soul.md` / `form.md` / `playbook.md`
- 备份原文件到 `.ghostink_backup/<filename>.<timestamp>.md`

## 验收

- [ ] 4 个子命令完整文档化
- [ ] `list` 区分内置 vs 用户自拆,标版本徽章
- [ ] `analyze` 完整 / quick 双模式,默认完整
- [ ] `analyze --only` 支持 soul / form / playbook 单选
- [ ] `pick` 含兼容性检查(警告但不强禁)
- [ ] `pick` 切换前自动备份,允许回退
- [ ] `info` 显示兼容性提示
- [ ] 中文叙述,frontmatter 字段名英文
- [ ] 跟现有 style-init 提取逻辑兼容(便于 analyze 直接复用)

## 工作量

1.5 天

## 备注

- analyze 子命令是 P2 内置库 v2 提炼的工具,精度直接决定 v2 库质量
- pick 切换 soul 时影响最大(灵魂换了相当于换了人),备份机制要可靠
- list 输出建议有"按当前 studio 兼容性排序"的隐性偏好(兼容的排前面)
