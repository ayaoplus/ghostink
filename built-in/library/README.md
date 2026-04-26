# Ghostink 内置参考库

这是 ghostink 跟代码仓库一起分发的参考库,包含若干公认中文作家的 soul / form / playbook 三件套。
新人通过 `/ghostink setup` 选择内置参考起步,降低上手门槛。

## 目录

```
built-in/library/
├── souls/         <name>.md       灵魂层
├── forms/         <name>.md       文笔层
├── playbooks/     <name>.md       打法层
└── factions.md                    派系标签词表
```

每个作家在三个目录下都有同名文件(完整三件套)。用户也可以混搭:
soul 用 maobidao,form 用 wangxiaobo,playbook 用 lixiaolai——只要兼容性允许。

## 内置作家

| Slug | 中文显示名 | 灵魂派系 | 适用场景 |
|---|---|---|---|
| sanmao | 三毛 | 抒情派 + 陪伴派 | 个人散文 / 长情叙事 |
| wangxiaobo | 王小波 | 思辨派 + 辛辣派 | 杂文 / 思辨长文 |
| hanhan | 韩寒 | 辛辣派 + 思辨派 | 博客评论 / 短论 |
| maobidao | 猫笔刀 | 陪伴派 + 思辨派 | 日报型公众号 / 财经陪伴 |
| lixiaolai | 李笑来 | 布道派 + 教程派 | 概念布道 / 框架教学 |

## source 字段

每个文件 frontmatter 的 `source` 字段标产出方式:

- `built-in`:仓库自带
- `ai-distilled`:AI 训练知识 + 抽样产出(setup 流程默认)
- `user-distilled-quick`:用户跑 `/ghostink distill --quick` 拆出(5+ 篇,跳过 Phase 3 多轮迭代)
- `user-distilled-deep`:用户跑 `/ghostink distill`(默认 deep,20+ 篇,完整 7 phase + 多轮迭代)

`library list` 命令对 `built-in` / `ai-distilled` 标 ⚠(精度低于 distill 产出),
`user-distilled-deep` 推荐使用。

## 兼容性

每份 library 文件 frontmatter 含 `factions` / `compat_*` / `incompat_*`,
所有标签必须从 `factions.md` 中选。

`/ghostink library pick` 切换时基于这些标签做兼容性检查——同源安静通过,
跨源兼容安静通过,不兼容则警告但允许用户继续。

## 贡献

欢迎用户拆好新的作家三件套提交回上游。提交规范:

1. 完整三件套(soul / form / playbook 同名)
2. 每个文件 frontmatter 字段完整(`source: user-distilled-deep` 推荐)
3. `factions` 等标签必须从 factions.md 选,不发明新词
4. 基于 20+ 篇代表作走 `/ghostink distill` 完整拆出
5. PR 描述附作家简介、代表作品列表、抽样规模
