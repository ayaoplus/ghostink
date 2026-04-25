# P3-T16:setup 流程中的兼容性引导

## 目标

强化 `setup` 命令 Step 5(选 form)和 Step 6(建 playbook)的兼容性引导,基于用户已选/已生成的 soul,主动推荐兼容选项,降低误配概率。

## 依赖

- 前置:T4(setup 命令)、T14(派系词表)、T15(pick 兼容性检查)完成

## 输入

- `commands/setup.md`(T4 产出)
- `built-in/library/factions.md`(T14 产出)
- `commands/library.md` pick 兼容性检查逻辑(T15 产出)

## 输出

### 1. 修改 `commands/setup.md` Step 5(选 form)

**改动前(T4 实现的版本)**:列出兼容 form 候选

**改动后**:

```
Step 5 — 选 form

加载已生成的 soul.md,提取 factions。
扫描 built-in/library/forms/ 与 [studio]/library/forms/(若有),
按以下顺序排序输出:

1. 同源 form(name 与 Step 2 选定参考一致)→ 顶部 [推荐]
2. 兼容 form(compat_soul 包含 soul.factions 中任一项)→ 中部
3. 中性 form(既不 compat 也不 incompat)→ 下部
4. 不兼容 form(incompat_soul 含 soul.factions)→ 不显示,
   但提示"如需查看不兼容选项,使用 /ghostink library list --include-incompat"

AskUserQuestion 候选:
  A) [推荐 - 同源]: catblade form (口语化)
  B) [兼容]: hanhan form (短句轰炸 + 反讽)— 警告:辛辣度更高
  C) [兼容]: sanmao form (抒情)— 警告:情感浓度更高
  D) 看不兼容选项
  E) 跳过(暂不选 form,以后再 pick)

用户选 A:直接复制 form 文件
用户选 B/C:复制并显示一句兼容性提示
用户选 D:展开不兼容列表(应用 P3-T15 警告流程)
用户选 E:不写 form.md,提示 "运行 write 之前必须 pick 一个 form"
```

### 2. 修改 `commands/setup.md` Step 6(建 playbook)

**改动前**:基于 soul 推荐 plays

**改动后**:

```
Step 6 — 建 playbook

加载 soul.md 的 compat_playbook 字段。
扫描 built-in/library/playbooks/ 中 plays 字段含 compat_playbook 任一项的文件。

可视化推荐:

> 基于你的 soul([陪伴派 + 思辨派]),推荐的打法:

>   ☑ 日报型(catblade 的 playbook 含此 play)— 高频例行写作
>   ☑ 热点评论型(catblade)— 大事件评论
>   ☑ 主题叙事型(catblade)— 个人回忆长文
>   ☐ 教程型(lixiaolai)— ⚠ 与你的 soul 偏离较大
>   ☐ 杂文型(wangxiaobo)— ⚠ 与你的 soul 偏离较大

用户多选,确认。
所选 plays 合并写入 [studio]/playbook.md(从对应内置 playbook 文件抽取 play 段落)。
```

### 3. 加 Step 5.5 兼容性总览(可选,新增节点)

在 Step 5 完成后、Step 6 之前,显示一个三件套兼容性总览:

```
你当前的搭配:

  Soul:     [name](factions)
  Form:     [name](factions)— 兼容性:[同源/兼容/中性/不兼容]
  Playbook: 待生成

[若任一组合不兼容,给出明显标识]

确认这个搭配吗?(确认 / 重选 form / 退到 Step 4 重选 soul)
```

## 验收

- [ ] setup.md Step 5 含同源/兼容/中性/不兼容四档排序
- [ ] Step 5 不兼容选项默认隐藏,需用户主动展开
- [ ] Step 6 推荐基于 soul.compat_playbook
- [ ] Step 6 不兼容 play 显式标⚠但可强选
- [ ] (可选)Step 5.5 兼容性总览,允许回退
- [ ] 所有兼容性提示文案与 P3-T15 警告风格一致

## 工作量

0.5 天

## 备注

- 这一层是"主动引导",P3-T15 是"被动检查",两者覆盖不同时机
- 不要在 setup 流程里让用户感受到"被算法限制"——保持"建议+用户决定"的姿态
- 中性档位很重要(没有任何 compat/incompat 标签声明的情况),不要默认归类为不兼容
