# Maobidao Distill Demo

`/ghostink distill maobidao --deep` 的完整流程实跑示例。

## 这个 demo 是什么

ghostink 的核心提炼引擎 `distill` 怎么从一位真实作者的历史文章里,提炼出
**Soul / Form / Playbook 三件套** v2.0-distilled。

本 demo 用**猫笔刀(moomoocat)**做样本——他在公众号每晚 22 点写财经日报,
2022-11 至 2025-04 共 727 篇。distill 流程从中**均匀采样 + 类型采样**共
35 篇细读,产出最终三件套刷新内置库。

## 输入(只读 iCloud,不复制进仓库)

```
/Users/erik/Library/Mobile Documents/iCloud~md~obsidian/Documents/cortex/studio/reference library/猫笔刀/
└── 727 篇 markdown 文章(命名 <编号>_<日期>_0_<标题>.md)
```

## 35 篇 in-context 样本清单

### Phase 1:15 篇均匀采样(按编号每 ~50 篇取 1)

#1 #2 #53 #107 #165 #228 #280 #343 #407 #466 #527 #588 #651 #706 #763 #827

### Phase 1.5:+20 篇类型导向采样

| 类别 | 篇数 | 文章 |
|---|---|---|
| A 个人叙事 | 4 | #170 #182 #500 #602 |
| B 主题深挖 | 3 | #480 #514 #657 |
| C 热点评论 | 3 | #150 #422 #506 |
| D 生活随笔 | 2 | #526 #730 |
| E 年度复盘 | 2 | #92 #406 |
| F 极端日报 | 6 | #240 #560 #614 #666 #707 #856 |

## 输出

### 内置库三件套(覆盖 1.0-draft → 升 2.0-distilled)

- `built-in/library/souls/maobidao.md`
- `built-in/library/forms/maobidao.md`
- `built-in/library/playbooks/maobidao.md`

### Distill 流程日志(7 个 phase)

- `phase1-soul-extraction.md` — Phase 1 Soul 提取(15 篇,身份先于风格)
- `phase1.5-soul-extension.md` — Phase 1.5 Soul 扩展(+20 篇,修订 5 处)
- `phase2-form-extraction.md` — Phase 2 Form 14 节提取(每条 trace 回 Soul)
- `phase4-playbook-clustering.md` — Phase 4 Playbook 7 种 Play 聚类
- `phase5-vocab-stats.md` — Phase 5 词频统计(LLM 估算)
- `phase6-finalize.md` — Phase 6 三件套统一升级 + 兼容性标签 + 双层摘要

(Phase 3 简化为 in-context 自我审视,记录在 phase2 日志末尾)
(Phase 7 skip — 不是用户自己的文章,Profile bootstrap 不适用)

## 跟 maobidao-demo(write demo)的关系

| Demo | 角度 | 内容 |
|---|---|---|
| **maobidao-distill-demo**(本 demo) | "怎么造的" | distill 7 phase 流程 + 真实 35 篇样本 |
| **maobidao-demo** | "怎么用" | 用本 demo 产出的 2.0-distilled 三件套写一篇 demo 文章 |

两个 demo 互补——distill 是"造工具",write 是"用工具"。

## 关键产物 highlights

### Soul Uniqueness Statement
> 他不像传统财经博主那样"分析师腔 + 看好/看空二选一",他做的是**晚饭后蹲在
> 小区里跟你抽根烟聊今天塘水**——把当天的市场新闻、政策变化、行业八卦,加上
> 自家大崽和叔叔的故事,全部用短句和省略号串起来,告诉你他怎么看,但不告诉你
> 怎么做。最反人设:"诚实 > 取悦,我不会给你酣畅淋漓的快感"

### Form 关键差异化(AI 默认仿不出来的)

1. **"……" 段间符**(每篇 3-5 次,AI 默认绝不会用)
2. **自创词**("含中量""塘主""4 倍理论"— 搜全网只有他用)
3. **讽刺 4 级体系**(主基调 2 级,绝不到 4 级粗口)
4. **励志式收尾仅自传/年终**(日报里写励志 = IP 破)

### Playbook 7 种 Play frequency

50% 日报 / 15% 周末闲聊 / 12% 极端日 / 10% 主题深挖 / 5% 自传年终 /
5% 短公告 / 3% 热点评论

## 复用这个流程

想为别的作者跑同样的提炼?

```
/ghostink distill <author> --path /path/to/articles --deep
```

详细 spec 见 `commands/distill.md`。
