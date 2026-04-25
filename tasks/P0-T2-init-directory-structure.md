# P0-T2:建立新目录结构

## 目标

把 ghostink 仓库的物理目录结构调整为 DESIGN 第 6.1 节定义的新布局,为后续命令重写(P1)和内置库填充(P2)打底。

## 依赖

- 无前置任务(可与 T1 并行)
- 参考:`DESIGN.md` 第 6.1 节

## 输入

- `DESIGN.md` 第 6.1 节(skill 仓库目录结构)
- 当前仓库结构

## 输出

执行以下文件系统操作:

### 1. 新建目录(7 个)

```
built-in/
built-in/library/
built-in/library/souls/
built-in/library/forms/
built-in/library/playbooks/
commands/_internal/
templates/_deprecated/
```

注:`tasks/` 目录已经存在(本任务文档就在里面),无需再建。

### 2. 新建占位文件(防止空目录被 git 忽略)

- `built-in/library/souls/.gitkeep`
- `built-in/library/forms/.gitkeep`
- `built-in/library/playbooks/.gitkeep`
- `commands/_internal/.gitkeep`
- `templates/_deprecated/.gitkeep`

### 3. 新建 README 占位

`built-in/library/README.md` 内容包含:

- 这是 ghostink 内置参考库
- 目录结构(souls / forms / playbooks)
- 版本约定:
  - `1.0-draft` = AI 直接产出的初版
  - `1.0-quick` = 用 5+ 篇文章 quick 模式拆出
  - `2.0` = 用 20+ 篇文章完整模式拆出
- 每个作家完整三件套同名(如 `wangxiaobo` 在三个目录下都有同名 `.md` 文件)

### 4. 不动的内容(P0 阶段保持不动)

- `commands/*.md`(P1 阶段才改写)
- `templates/style_spec_template.md`(T3 任务会迁到 `_deprecated/`)
- `templates/author_profile/*`(profile 部分不变)
- `prompts/`、`examples/`、`LICENSE`、`README*.md`、`SKILL.md`(对应任务处理)
- 用户已有的 `analyzed_authors/`(若存在,P4 任务再处理迁移)

### 5. .gitignore 检查

- 确保 `built-in/`、`tasks/` 不被忽略
- 检查 `.gitignore` 是否需要追加规则(如 `templates/_deprecated/` 的旧文件保留追踪)

## 验收标准

- [ ] 7 个新目录已创建,通过 `ls -la` 可见
- [ ] 5 个 `.gitkeep` 文件已创建
- [ ] `built-in/library/README.md` 已创建,包含版本约定 3 项
- [ ] `git status` 显示新增目录和文件可被追踪
- [ ] 现有 `commands/*.md` 数量与重构前一致(未删未改)
- [ ] `templates/style_spec_template.md` 仍在原位(T3 才迁)
- [ ] 不要在本任务中新建 `soul.md` / `form.md` / `playbook.md`(它们是用户工作室文件,不是仓库文件)

## 工作量

0.5 天

## 备注

- 这是纯文件系统操作,不涉及内容创作
- 可以与 T1 并行做
- **T2 必须先于 T3 完成**:T3 要把旧 `style_spec_template.md` 迁到 `templates/_deprecated/`,需要本任务先建好该目录
