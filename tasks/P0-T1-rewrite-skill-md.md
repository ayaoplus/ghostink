# P0-T1:重写 SKILL.md

## 目标

把 ghostink 的 skill 入口文件 `SKILL.md` 从旧的 9 命令架构重写为新架构(3 个一级命令 + 5 个核心概念),作为 Claude 加载本 skill 时读到的第一份文档。

## 依赖

- 无前置任务
- 参考:`DESIGN.md`(已冻结)

## 输入

- 当前 `/Users/erik/development/ghostink/SKILL.md`(对照,内容大部分要替换)
- `DESIGN.md` 第 2 节(核心概念)
- `DESIGN.md` 第 4 节(命令体系)
- `DESIGN.md` 第 6 节(文件结构)
- `DESIGN.md` 第 12.8 节(语言规则)

## 输出

重写后的 `/Users/erik/development/ghostink/SKILL.md`,7 个章节顺序如下:

### 1. 简介(2-3 句)
ghostink 是什么 + 核心理念(灵魂/形态/打法分离 + Profile 是平行素材库)。

### 2. 5 个核心概念
每个 2-3 行简介 + 文件位置:
- Soul(IP 灵魂)
- Form(文笔)
- Playbook(打法)
- Profile(素材库)
- Skeleton(单篇骨架)

### 3. 3 个一级命令
| 命令 | 用途 | 何时用 |
|---|---|---|
| `/ghostink setup` | 首次/重置:建立创作者档案 | 新人,或想全部重做 |
| `/ghostink write [topic]` | 写一篇文章 | 日常主路径 |
| `/ghostink library` | 库管理 | 换文笔、加新参考、看库存 |

### 4. library 子命令清单
list / info / analyze / pick(soul/form/playbook)

### 5. 边角入口(可选)
- `/ghostink check [file]` — 任意文本审查
- `/ghostink deai [file]` — 任意文本去 AI 感
- `/ghostink illustrate [file]` — 配图

### 6. Studio Discovery 规则
延续当前规则,**改一处**:第 2 步从"cwd 包含 `style_spec.md`" 改为 "cwd 包含 `soul.md` 或 `souls/` 目录"。

### 7. 目录布局
按 DESIGN 6.2 节展示用户工作室结构,**不**展示 skill 仓库结构(那个放在 README)。

## 验收标准

- [ ] 新文件不超过 200 行
- [ ] 不出现 `Layer 0/1/2/3` 字眼
- [ ] 不出现 `style-init` / `style-forge` / `style-evolve` / `brainstorm` / `profile-init` 命令(它们都被收编了)
- [ ] Studio Discovery 第 2 步改为检测 `soul.md` 或 `souls/`
- [ ] 关键术语(soul / form / playbook / profile / skeleton)首次出现给中文括注,例:"Soul(灵魂层)",后续保持英文
- [ ] frontmatter 字段名(`type:`、`compat_form:` 等)在示例中保持英文
- [ ] 包含一段"快速开始"(3 步:setup → write → library)放在简介之后、概念之前
- [ ] 全文中文,自然流畅,不像翻译腔
- [ ] **快速开始段落不内嵌完整对话样本**——只列 3 个命令 + 一句说明,样本留给 examples/(P4-T18)

## 工作量

0.5 天

## 备注

- 旧 SKILL.md 不要删,改名为 `SKILL.md.v1.bak` 作为参考(P4 阶段统一清理)
- 写完后人工读一遍,确保新人扫一眼就知道"我该用哪个命令"
