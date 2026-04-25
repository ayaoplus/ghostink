# _internal/form-check

文笔合规审查。**只出报告,不强制修复。**

被以下两处调用:
- `commands/write.md` Step 4(自动审最新草稿)
- `commands/check.md`(用户用 `/ghostink check [file]` 审任意文本)

## 输入

- 待审查的文本(从 write Step 3 的草稿,或 check 命令的文件 / inline 输入)
- `[studio]/form.md`(规则源)
- `[studio]/playbook.md`(若文章类型已知,加载对应 play 的结构规则)

## 输出

结构化报告(纯文本,不写文件)。**不修改原文件。**

## 检查项

### Structure(结构)
- [ ] 开头长度(form 的 opening 长度限制内?)
- [ ] 分隔符使用("……" 或等价物存在?)
- [ ] 文章类型合规(匹配某个 play 模板?)
- [ ] 结尾风格(匹配 form 的 closing patterns?)
- [ ] 段数与顺序

### Vocabulary(词汇)
- [ ] 扫 never_use 词表 → 每条命中列出行号 + 替代建议
- [ ] 扫书面化连接词("因此""此外""综上""鉴于") → 列出 + 替代
- [ ] Signature Expressions 是否出现?(列出每条出现次数)
- [ ] 口语化密度是否在期望范围?

### Voice(声音)
- [ ] 显式个人判断数(应 ≥2)
- [ ] 自嘲或脆弱表达(form 要求时检查存在性)
- [ ] 加粗用在关键信息上,不是装饰?
- [ ] 句长节奏:短句比例是否符合 form 规定的%?

### Prohibition(禁忌)
- [ ] 无四字成语堆叠(连续 2+)
- [ ] 无排比句("我们必须 X、必须 Y、必须 Z")
- [ ] 无教科书式过渡("首先...其次...最后")
- [ ] 无教化("这告诉我们...")
- [ ] 无被动声音对冲("据悉""被认为")
- [ ] 无总结段(form 禁止时检查)

### Emotional Tone(情感基调,需 play 已知时)
- [ ] 正面事件含警示性"但"句?
- [ ] 负面事件避免空安慰?
- [ ] 整体基调匹配 play 的 emotional_baseline?

## 报告格式

```
Ghostink Form Check
====================
Target: <filename / inline>
Spec: form.md (name: <form name>)
Article Type: <play 名 / 未知>

Score: 82 / 100

STRUCTURE          [4/5]
  [PASS] 开头 3 句 ✓
  [PASS] 分隔符 2 处 ✓
  [FAIL] 结尾用了总结段 — form 禁止
  [PASS] 段落顺序符合 <play 名> 模板

VOCABULARY         [7/10]
  [FAIL] 第 12 行: "令人" → 建议改 "让人"
  [FAIL] 第 28 行: "因此" → 建议改 "所以"
  [FAIL] 第 45 行: "尽管" → 建议改 "虽然"
  [PASS] Signature Expressions: "我觉得" ×2 / "就是" ×8 ✓
  [WARN] 口语化密度略低,建议加 1-2 处口语词

VOICE              [5/5]
  [PASS] 个人判断: 4 处 ✓
  [PASS] 自嘲: 1 处 ✓
  ...

PROHIBITIONS       [9/10]
  [FAIL] 第 52 行: "综上所述" — form 禁止表达
  ...

EMOTIONAL TONE     [3/3]
  [PASS] 正面事件含警示 "但" ✓
  ...

Suggestions:
1. 把第 12 行 "令人" 改 "让人"
2. 删掉结尾的总结段
3. 在开头加一处口语化表达
```

## 报告后处理(取决于调用方)

### 被 write Step 4 调用时

报告显示后,write Step 4 询问用户:

> 你想怎么处理?
> - A) 全部修(我自动按报告改)
> - B) 选择性修(指定哪几条)
> - C) 跳过,这版我接受

A 路径 → 本命令配合 write 自动应用所有 FAIL 项的建议替换,产出修订版给用户看。
B 路径 → 等用户具体说要改哪条,再针对性改。
C 路径 → 直接进 write Step 5。

### 被 check 命令独立调用时

仅显示报告,不主动修改。最后询问:

> 要我把 FAIL 项的建议自动应用,生成修订版吗?(原文件不动,新文件加 _revised 后缀)
> - 是 / 否

## 实现备注

- **永远不强制阈值通过**——80 分以下也只是建议,用户可以选择保留
- 用 LLM 主导扫描,never_use / forbidden 词表用字符串包含 + 上下文判断(如"因此"作为词组开头才算违规,作为段中词不算)
- 报告语气:**报告事实,不评判**。"FAIL"不是骂人,是提醒
- 输出中文报告
