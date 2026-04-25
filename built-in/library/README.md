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
soul 用 catblade,form 用 wangxiaobo,playbook 用 lixiaolai——只要兼容性允许。

## 当前内置作家(v1)

| Slug | 中文显示名 | 灵魂派系 | 适用场景 |
|---|---|---|---|
| sanmao | 三毛 | 抒情派 + 陪伴派 | 个人散文 / 长情叙事 |
| wangxiaobo | 王小波 | 思辨派 + 辛辣派 | 杂文 / 思辨长文 |
| hanhan | 韩寒 | 辛辣派 + 思辨派 | 博客评论 / 短论 |
| catblade | 猫笔刀 | 陪伴派 + 思辨派 | 日报型公众号 / 财经陪伴 |
| lixiaolai | 李笑来 | 布道派 + 教程派 | 概念布道 / 框架教学 |

## 版本约定

每个文件 frontmatter 含 `version` 字段,值约定:

| 版本 | 含义 |
|---|---|
| `1.0-draft` | AI 直接产出的初版,精度有限,UI 显示 ⚠ |
| `1.0-quick` | 用 5+ 篇文章 quick 模式拆出 |
| `2.0` | 用 20+ 篇代表作完整模式拆出,推荐使用 |

`source` 字段对应:
- `ai-distilled`:AI 训练知识 + 抽样产出
- `user-analyzed`:跑过 `library analyze` 拆出

## 兼容性

每份 library 文件 frontmatter 含 `factions` / `compat_*` / `incompat_*`,
所有标签必须从 `factions.md` 中选。

`/ghostink library pick` 切换时基于这些标签做兼容性检查——同源安静通过,
跨源兼容安静通过,不兼容则警告但允许用户继续。

## 贡献

欢迎用户拆好新的作家三件套提交回上游。提交规范:

1. 完整三件套(soul / form / playbook 同名)
2. 每个文件 frontmatter 字段完整
3. `factions` 等标签必须从 factions.md 选,不发明新词
4. version 字段为 `2.0`(基于 20+ 篇代表作完整拆出)
5. PR 描述附作家简介、代表作品列表、抽样规模
