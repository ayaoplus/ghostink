# P4-T17:重写 README.md 与 README_CN.md

## 目标

把项目的两份 README 与新架构对齐。中文版作为主文档(给中文创作者),英文版作为简介+索引(国际化占位)。

## 依赖

- 前置:T1-T16 全部完成(架构已固化,内容已就位)
- 参考:`DESIGN.md` 整份、`SKILL.md` 重写后版本

## 输入

- `SKILL.md`(T1 重写后版本,作为内容基础)
- `DESIGN.md` 第 2/4/5/6/7 节
- 现有 `README.md` / `README_CN.md`(用作对照,大部分要替换)

## 输出

### 1. README_CN.md(主文档,完整中文)

章节顺序:

```markdown
# Ghostink

(一句话简介:风格驱动的写作系统,把"你思考什么"和"AI 怎么写"分开)

## 核心理念

灵魂(Soul)/ 形态(Form)/ 打法(Playbook)三层分离 + 个人弹药库(Profile)

## 快速开始

3 步上手:
1. /ghostink setup    建立你的创作者档案
2. /ghostink write    写第一篇
3. /ghostink library  换风格 / 加新参考

## 5 个核心概念

(简介 Soul / Form / Playbook / Profile / Skeleton)

## 三个一级命令

(setup / write / library 详介)

## 内置作家库

(列 5 个作家 + 风格速览)

## 目录结构

(用户工作室 + 仓库结构两张图)

## 多平台支持

(说明公众号 / X / 微博 / Substack 等)

## Studio Discovery 规则

(对应 SKILL.md 的规则)

## 二阶段路线图

(指向 DESIGN 第 10 节)

## 贡献

(欢迎用户回馈拆好的作家三件套到 built-in/library/)

## 许可

LICENSE 链接
```

要求:

- 全中文,自然流畅
- 关键术语保留英文(soul/form/playbook 首次中文括注)
- 含至少一张目录结构图
- 含一段示例对话(setup 的关键问答片段),让新人看到样子
- 不超过 400 行

### 2. README.md(英文简介)

简短(80-150 行),内容:

```markdown
# Ghostink

A style-driven writing system that separates **what you think** (human)
from **how it's written** (AI).

> **For Chinese readers / 中文用户**: please refer to [README_CN.md](./README_CN.md)
> for the full documentation. The Chinese version is the source of truth.

## Quick Start

(3 commands)

## Core Concepts

(brief overview of Soul / Form / Playbook / Profile)

## Built-in Library

(list 5 Chinese authors with one-line description in English)

## Status & Roadmap

v1: Chinese-first. International support is a v2+ goal.
```

要求:

- 明确指向 README_CN.md 作为完整文档
- 不重复中文文档的所有内容
- 帮助 GitHub 上路过的国际用户快速判断"这是干什么的、需不需要中文"

### 3. 删除内容审核

确保两份 README 都不含:
- `style-init` / `style-forge` / `style-evolve` / `brainstorm` 命令名
- `Layer 0/1/2/3` 字眼
- `analyzed_authors/` 路径(替换为 `library/`)
- 9 命令清单(替换为 3 一级 + 边角)

## 验收

- [ ] README_CN.md 不超过 400 行,章节顺序与上面一致
- [ ] README_CN.md 含 5 概念、3 命令、内置库 5 作家、目录结构、Studio Discovery
- [ ] README_CN.md 含一段 setup 示例对话(10-20 行)
- [ ] README.md 不超过 150 行,明确指向 README_CN.md
- [ ] README.md 不重复 README_CN.md 全部内容
- [ ] 两份 README 都不含旧命令名 / Layer 字眼 / analyzed_authors
- [ ] 命令清单与 SKILL.md 一致(交叉校对)

## 工作量

1 天

## 备注

- README_CN 是中文用户的入口,体验直接影响项目的"GitHub stars"
- 不要把 README 写成 SKILL.md 的副本——README 是"为什么用",SKILL.md 是"怎么用"
- 示例对话从 P4-T18 的端到端 demo 摘录最自然
