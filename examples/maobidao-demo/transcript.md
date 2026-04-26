# Demo Transcript — 从 setup 到一篇文章

> 这不是逐字记录,是关键交互的脚本。真实使用时对话会更长更碎,但关键节点和这里一致。

---

## Part 1:`/ghostink setup`

### Step 1 — 平台与读者

**Assistant:** 你主要在哪些平台发文?(影响默认语气与格式)
> A) 公众号 / 中文长文平台
> B) Twitter / X 长文或 thread
> C) 微博
> D) 个人博客 / Substack
> E) 其他

**User:** A 和 B

**Assistant:** 你的读者画像最接近哪个?
> A) 同行专业(懂行,不需要解释基础概念)
> B) 初学新人
> C) 跨界爱好者
> D) 决策者

**User:** C — 主要是开发者社群,他们懂代码,但 AI 这个领域很多人在跨界

**Assistant:** 读者主要在什么场景看你?
> A) 通勤碎片
> B) 睡前深度
> C) 工作中查询
> D) 周末长读

**User:** A 和 B 都有,通勤为主

**Assistant:** 你和读者的关系是?
> A) 陌生订阅
> B) 半熟社群
> C) 朋友圈

**User:** A,基本都是陌生订阅

✓ 写到 profile/identity.md

### Step 2 — 选模仿起点

**Assistant:** 你想从哪里起步?
> A) 从内置库选一个完整套件(适合新人,直接全套抄一个作家)
> B) 多组合(soul / form / playbook 分别选)
> C) 自拆参考(我有想拆的作者)
> D) 纯对话生成

**User:** A

**Assistant:** 内置作家:
> - 三毛(sanmao):抒情 + 陪伴 — 散文 / 长情叙事
> - 王小波(wangxiaobo):思辨 + 辛辣 — 杂文 / 思辨长文
> - 韩寒(hanhan):辛辣 + 思辨 — 博客评论 / 短论
> - **猫笔刀**(maobidao):陪伴 — 日报型公众号 / 财经陪伴
> - 李笑来(lixiaolai):布道 + 教程 — 概念布道 / 框架教学

**User:** 猫笔刀。我喜欢他不催不教育的写法,虽然我写的是 AI 不是财经

**Assistant:** OK,maobidao 整套 — soul / form / playbook 三件套都从他这复制。
不过你领域不一样,Step 3-4 我会用对话给 soul 加 AI 领域的差异化补丁。

### Step 3 — 锐化 profile

**Assistant:** 说 3-5 件对你印象深、可能在文章里引用的真实经历。

**User:** 我带过几个实习生,他们用 AI 工具的方式让我意识到一些事...

(对话展开,产出 5 条 experiences)

**Assistant:** 说 3-5 个你和多数人不太一样的观点。

**User:** 我觉得 AI 不会取代初级程序员,反而让他们更值钱...

(对话展开,产出 5 条 opinions,其中 2 条标记为 differentiator)

✓ 写到 profile/experiences.md, opinions.md

### Step 4 — 产出 soul

**Assistant:** 我把 maobidao 的 soul 复制下来,加你的差异化补丁:
- 0.4 信念加入你的"AI 让初级程序员更值钱"作为新一条
- 0.5 信任机制中的"公开亏损"改成"公开自己实操 AI 工具的失败案例"
- 0.6 情感锚点改为"AI 焦虑时的不慌声音"

显示完整 soul.md...

**User:** 看上去对。继续

### Step 5 — 选 form

**Assistant:** 给你的 soul(陪伴派 + 思辨派)选个 form:
> - A) [推荐 - 同源] maobidao form(口语化)
> - B) [兼容] hanhan form(短句轰炸 + 反讽)— 更辛辣
> - C) 看不兼容选项

**User:** A,我就要猫笔刀那种聊天感

✓ form.md = maobidao form

### Step 6 — 建 playbook

**Assistant:** 基于你的 soul,推荐的打法:
> ☑ 日报型(maobidao 含)— 高频例行
> ☑ 热点评论型(maobidao)— 大事件评论
> ☑ 主题叙事型(maobidao)— 个人回忆长文

**User:** 全要

✓ playbook.md = maobidao 三个 plays

### Step 7 — 完成

```
✓ Setup 完成

  Soul:  maobidao-fork (陪伴派 + 思辨派)
  Form:  maobidao (口语化)
  Playbook: 日报型 / 热点评论型 / 主题叙事型
  Profile:
    - identity.md (1 条)
    - experiences.md (5 条)
    - opinions.md (5 条)
    - refs.md (空)

下一步:
  /ghostink write [topic]
```

---

## Part 2:`/ghostink write "AI 编程对初级程序员的影响"`

### Step 1 — 选 playbook

**Assistant:** 这个话题(AI 编程对初级程序员)我倾向 **热点评论型**,因为它围绕一个行业现象。
> - A) 热点评论型(推荐)
> - B) 日报型 — 不太合适
> - C) 主题叙事型 — 除非你想以"我带的那个实习生"切入

**User:** A

### Step 2 — brainstorm

**Assistant:** 你对这事一句话最想说什么?
> - A) AI 让初级程序员更难找工作(主流观点)
> - B) AI 让初级程序员更值钱(反主流)
> - C) AI 加速初级到高级的过渡
> - D) 自由输入

**User:** B,但我想说的是"AI 让会用 AI 的初级程序员更值钱,不会用的更困难"

**Assistant:** 打断一下 — 你说"会用 AI 的更值钱,不会用的更困难"。这话比 B 更有意思。
我怀疑你真正想写的不是"AI 对初级程序员的影响",而是"AI 把初级程序员分成了两类"。

> - A) 对,换这个框架
> - B) 不对,保持原来的
> - C) 部分对,我调整

**User:** A

**Assistant:** 好,核心判断锁定:"AI 把初级程序员分成了会用 AI 和不会用的两类,差距在拉大"。

差异化角度:
> - A) 你带过实习生,有具体观察(profile/experiences)
> - B) 你 opinions 里写的"工具决定上限"
> - C) 自由输入

**User:** A + B

**Assistant:** 用哪些素材?
> - A) E001 实习生小张第一周
> - B) E003 实习生小李用 AI 三天搞定的事
> - C) E004 自己当年初级时的对照

**User:** A + B + C 都用

✓ skeleton 写到 drafts/_brainstorm/2026-04-24-ai-junior-dev.md

### Step 3-7

(自动跑 draft → form-check → deai → 平台输出,期间用户做了 2 处微调)

最终成稿写到 `drafts/2026-04-24-ai-junior-dev.md`,长度 1820 字,通过 form-check
评分 88/100,deai-pass 评分 81/100(用户接受当前版本不再修)。

---

## 实际跑下来的体感

- **Step 2 的 4 个分支体验流畅**——A 路径(整套抄)对新人友好
- **Step 4 的差异化补丁**是 demo 的关键,**让用户感觉"这个 soul 是我的"而不是抄来的**
- write 的 brainstorm 的"挑战框架"环节抓得很准——把"AI 让初级程序员更值钱"
  锐化成"AI 把初级程序员分成两类",这是用户没想到但确实更锋利的框定
- form-check 88 分意味着大部分文笔合规,小问题用户自己读改
- deai-pass 81 不强制,符合"决定权在用户"的原则
