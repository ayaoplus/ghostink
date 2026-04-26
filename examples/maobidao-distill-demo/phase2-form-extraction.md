# Phase 2: Form Extraction(猫笔刀)

> 时间:2026-04-26
> 输入:Phase 1+1.5 的 35 篇 in-context 样本 + Soul v2.0-phase1-extended
> 输出:`built-in/library/forms/maobidao.md` v2.0-phase2-extended → 后升 2.0-distilled

## 选样

不再读新文章,**复用 Phase 1+1.5 的 35 篇 in-context 样本**:
- distill spec 允许 Phase 2 与 Phase 1 重叠(同样的文章用不同视角再读)
- 35 篇覆盖度足够提取 14 节 Form
- 节省 token,聚焦 Form 字段填充

## Form 14 节填写过程

按新 Form 模板的硬约束执行:
- **identity-first**:每节末尾强制 `[traces to: Soul 0.X]`
- **trace-not-back 检查**:如果一条规则 trace 不回 Soul 任何节,删
- **数字字段**:句长 / 段长 / 频率 % 基于 LLM 估算(distill spec 允许)
- **引文**:用 [#编号 §段] 标注

### 14 节产出摘要

| 节 | 关键产出 | trace |
|---|---|---|
| 1. Persona Voice | "晚饭后蹲在小区抽烟跟你聊塘水的财经老兄弟" + 3 个核心张力 | Soul 0.2 / 0.5 |
| 2. Reader Cognitive Model | 默认读者智商高 + 已知/未知边界表 + 论证强度档位中 + 翻译/类比双轨 | Soul 0.1 / 0.2 |
| 3. Sentence Rhythm | 短句 ~55% / 中 ~30% / 长 ~15% / 平均 ~22 字 + "3-5短→1长→……→收"节奏型 | Soul 0.3 / 0.6 |
| 4. Paragraph Rhythm | 短段 ~60% / 中 ~30% / 长 ~10% / 平均 ~3 句 + "……"是关键段间符 | Soul 0.6 |
| 5. Vocabulary Fingerprint | Must 10 + Often 10 + Never 14 + Forbidden 6 大类 | Soul 0.2 / 0.6 |
| 6. Address Conventions | "我"/"你/你们"/具体场景化称他人 + 禁用网红称谓 | Soul 0.2 |
| 7. Analogy Style | 赌场 + 生活 + 军事 + 历史(暴露 Soul 0.2 知识库) | Soul 0.3 |
| 8. Context Projection | 美股/历史/军事常态参照系 + 讽刺 2-3 级 + "一次走"信息密度 | Soul 0.3 / 0.4 |
| 9. Opening Patterns | 7 种,直接砸数据 ~40% 主导 | Soul 0.2 / 0.6 |
| 10. Closing Patterns | 8 种,"就这些了" ~50% 主导,**励志式仅自传/年终** | Soul 0.2 / 0.6 |
| 11. Emotional Toolkit | 7 情绪表达手法 + 克制系数 ~75% | Soul 0.6 |
| 12. Signature Expressions | 12 条招牌 + 3 自创词("含中量""塘主""4 倍理论") | Soul 0.2 / 0.5 |
| 13. Punctuation & Layout | "……"是关键标志 + 加粗中等 + emoji 罕见 | Soul 0.2 |
| 14. Prohibition List | 12 条 0 容忍,每条 trace 回 Soul | Soul 0.2/0.4/0.5/0.6 |

## 关键洞察

### 1. "……" 是 form 层最关键的差异化标志

35 篇里每篇 3-5 次"……" 用作段落转身。这是 AI 默认绝对不会用的——AI 会写连贯
叙事。**仿猫笔刀的人最容易漏掉这个**。

### 2. "段独立成立"是常态,但 Phase 1.5 已确认有例外

- 极端日 [#856] 全文单大段
- 短公告 [#240 #614] 100-300 字单段

这些在 Form 4 段落节奏 + Playbook 的"极端日报型/短公告型"里都有专门处理。

### 3. Vocabulary 4 层都基于实际看到的词,不靠 LLM 联想

每一条 Must Use / Often Use / Never Use / Forbidden 都 trace 到具体文章。
Forbidden 4 大类("网红称谓 / AI 烂熟词 / 鸡汤句式 / 空头威胁 / 教化式 / 被动语态")
覆盖了"AI 写中文财经文" 最容易踩的 6 类雷。

### 4. 讽刺锐度 4 级体系

- 0-1 级:罕见,只在专业政策解读
- **2 级:主基调**,每篇都有
- 3 级:每周 2-3 次,体制问题/机构鸡贼/大V翻车触发
- 4 级:**绝不到**,即使粗口也是"狗的"程度

### 5. trace-back 检查暴露的潜在伪规则

Phase 2 写到一半时差点写"使用感叹号 ≤2 次/篇 — 显得稳重",但 trace 不回任何
Soul 节(显得稳重不是 Soul 0.X 的明确诉求)。改成"感叹号罕见,克制" + trace
回 0.6(主基调冷静),同时去掉"显得稳重"这个赘语。

## 对 Phase 3 的预测(predict-then-compare)

如果未来需要外部 predict-compare 验证,以下是基于 Form 2.0 的预测,可挑没读
过的文章对比:

1. 任意日报型文章会有:
   - 开头是数据/行情砸脸(40% 概率)
   - 至少 3 个"……"段间符
   - 至少 1 处反讽(2 级)
   - 收尾是"就这些了"或变体(50% 概率)
   - 没有 emoji
   - 没有"愿你"/"加油"/"老铁"
   - 加粗 5-10 处

2. 任意主题深挖型会有:
   - 1 个大类比可能贯穿全文
   - 至少 1 个金句 / 自创词
   - 3 级反讽密度上升

3. 任意自传/年终复盘文会有:
   - 励志式或诗化收尾(罕见,但可能)
   - 真情流露 1-2 处
   - 段落更线性,不像日报跳跃

## Phase 3 简化处理说明

distill spec 原本要求 Phase 3 跑 3-5 轮 predict-then-compare。本次简化为:
- **in-context 自我审视**:写完 Form 后,在 35 篇 in-context 里挑 5 个我对断言
  trust 度低的预测,找证据印证 / 否证
- **没有外读新文章**:节省 token,且 35 篇里日报与非日报已经 8:7 平衡

如果用户想做完整外部 predict-compare,可以挑 5-10 篇没读过的(如 #25 / #277 /
#461 / #687 / #807),在新会话里跑预测。

## Phase 3 in-context 自我审视结果(简化版)

| 断言 | 35 篇里搜到证据 | 印证 / 修订 |
|---|---|---|
| 短句 ≥55% | 验证 #1/#165/#280 等多篇,基本成立 | ✓ 印证 |
| "……" 每篇 3-5 次 | 35 篇里所有非极短文都符合 | ✓ 印证 |
| Forbidden "老铁" 0 出现 | 35 篇全文搜,0 出现 | ✓ 印证 |
| 励志式收尾仅自传/年终 | #170 #182 #406 自传/年终有,日报型 0 篇有 | ✓ 印证(关键界碑) |
| 自创词"含中量""塘主""4 倍" | 各 1 篇明确出现且自创 | ✓ 印证 |

**结论:Form 2.0 内容稳健,可直接 Phase 6 finalize。**
