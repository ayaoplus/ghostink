# Phase 6: Finalize(猫笔刀)

> 时间:2026-04-26
> 输出:三件套统一升级到 2.0-distilled,frontmatter 完整

## 6.1 兼容性标签自动填充

基于 Soul.factions = `[陪伴派, 思辨派]`,推导:

### Form 兼容性
- `compat_soul: [陪伴派, 思辨派]`(同源)
- `incompat_soul: [书面化, 长句分析]`(口语化短句反义派系)
- `compat_playbook: [日报型, 热点评论型, 主题叙事型]`(本作家三件套同源)

### Playbook 兼容性
- `plays: [日报型, 周末闲聊型, 极端日报型, 主题深挖型, 自传年终复盘型, 短公告型, 热点评论型]`
- 7 个 Play 全部 trace 回 Soul 某节(见 playbook 文件每个 Play 末尾的
  `[traces to: Soul 0.X]`)

### Soul 兼容性
- 已在 Phase 1.5 时填好,无需再调

## 6.2 Version 升级

| 文件 | 之前 | 现在 |
|---|---|---|
| souls/maobidao.md | 2.0-phase1-extended | **2.0-distilled** |
| forms/maobidao.md | 2.0-phase2-extended | **2.0-distilled** |
| playbooks/maobidao.md | 2.0-phase4-extended | **2.0-distilled** |

## 6.3 完整呈现给用户(双层摘要)

### Tier 1 — 这位作者是谁(Soul 摘要)

- **Uniqueness**:晚饭后蹲在小区抽烟跟你聊塘水的财经老兄弟,可信度来自实仓 +
  关系网 + 承认无知,不来自任何头衔。**最反人设**:"诚实 > 取悦,我不会给
  你酣畅淋漓的快感"

- **核心价值主张**:做读者每晚 22 点的塘水过滤器 + 心态校准器 + 主动情绪
  共鸣(极端日)

- **跟读者的关系**:平视 + 略有经验优势,40+ 岁中年男性长辈感

- **Top 3 信念**:
  1. A 股本质是"塘主允许的散户互害游戏"
  2. 避险 > 进取,中年人更要示弱认怂
  3. 数据 > 情绪,具体 > 抽象

- **取关测试**:他停更 → 读者每晚 22 点的"股市晚课"没了。再也没人能用
  "淡定 + 内幕 + 生活气息"的混合,把今天的市场说给我听

- **信任机制**:实仓金额 + 行业关系网 + 承认无知 + 主动暴露错误预测 +
  管理过失诚恳认错 + 史诗暴跌日主动情绪共鸣

### Tier 2 — 这位作者怎么写(Form + Playbook 摘要)

- **关键风格特征**:
  1. 短句 ~55% + 平均句长 ~22 字
  2. "……" 是关键段间符,每篇 3-5 次切话题
  3. 段落短(平均 ~3 句)+ 视觉留白
  4. Vocabulary 4 层(Must 10 + Often 10 + Never 14 + Forbidden 6 大类)
  5. 自创词("含中量""塘主""4 倍理论")
  6. 讽刺锐度主基调 2 级,体制问题升 3 级,绝不到 4 级
  7. 类比库主要是赌场 + 生活 + 军事 + 历史(暴露知识库)

- **文章打法 7 种**(各占比):
  1. 日报型 ~50%(主力)
  2. 周末闲聊型 ~15%
  3. 极端日报型 ~12%
  4. 主题深挖型 ~10%
  5. 自传/年终复盘型 ~5%
  6. 短公告/占位型 ~5%
  7. 热点评论型 ~3%

- **Top 10 Must Use 词**:你/你们 / 我/我个人 / 说白了 / 其实 / 哦对了 / 真就 /
  撑死 / 没准 / 反正 / 咯

- **Top 10 Never Use 词**:因此 / 此外 / 综上 / 旨在 / 不仅而且 / 莫过于 /
  务必 / 鉴于 / 倘若 / 譬如

- **Forbidden 关键**:老铁 / 家人们 / 加油 / 愿你 / 我们都 / 赋能 / 范式 /
  颠覆 / 抓手 / 闭环 / 据悉 / 被认为

- **Confidence 评估**:
  - Soul 6 节全部 confirmed(Phase 1.5 已经 35 篇验证)
  - Form 14 节全部 confirmed
  - Playbook 7 个 Play frequency 估算可能有 ±5% 误差(35 篇 vs 727 全样本)
  - 0 条 [LOW-CONFIDENCE] 残留

## 6.4 Phase 7 跳过说明

Phase 7 Profile bootstrap 是"如果文章是用户自己写的,顺手抽 profile"。
猫笔刀是别人(moomoocat),不是 ghostink 用户自己,所以 **跳过 Phase 7**。

如果未来 ghostink 用户跑 `/ghostink distill <自己的笔名>` 提炼自己的文章,
Phase 7 会触发,产出 `[studio]/profile/{identity,experiences,opinions,refs}.md`。

## 文件位置

- `built-in/library/souls/maobidao.md` — Soul v2.0-distilled
- `built-in/library/forms/maobidao.md` — Form v2.0-distilled
- `built-in/library/playbooks/maobidao.md` — Playbook v2.0-distilled

镜像同步到 examples/maobidao-demo/studio/ 给 write demo 用:
- `examples/maobidao-demo/studio/soul.md`
- `examples/maobidao-demo/studio/form.md`
- `examples/maobidao-demo/studio/playbook.md`

## 全程日志

- `phase1-soul-extraction.md` — Phase 1 Soul 提取(15 篇)
- `phase1.5-soul-extension.md` — Phase 1.5 Soul 扩展(+20 篇,共 35 篇)
- `phase2-form-extraction.md` — Phase 2 Form 提取
- `phase4-playbook-clustering.md` — Phase 4 Playbook 聚类
- `phase5-vocab-stats.md` — Phase 5 词频统计(LLM 估算)
- `phase6-finalize.md` — 本文档

(Phase 3 简化为 in-context 自我审视,记录在 phase2 日志末尾)
(Phase 7 skip — 不是用户自己的文章)
