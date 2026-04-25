# /ghostink deai

任意文本去 AI 感扫描。**只出报告 + 建议,不阈值通过,决定权在你。**

本命令是 thin wrapper,实际逻辑在 `_internal/deai-pass.md`。

## 用法

```
/ghostink deai [file-path]
/ghostink deai                  # 提示用户输入或选最近草稿
```

## 流程

1. 解析参数:
   - 提供 file-path → Read 该文件
   - 不提供 → 询问输入,或扫 `[studio]/drafts/` 列最近 3 篇

2. 按 SKILL.md Studio Discovery 规则解析 studio root

3. 加载 `[studio]/form.md`(若有,某些规则按 form factions 调整灵敏度)

4. **调用** `_internal/deai-pass.md`,输入文本 + (可选)form

5. 显示报告(9 条规则的 PASS/WARN/FAIL + 建议)

6. 询问:

   > 要应用建议生成修订版吗?
   > - 是,产出 <file>_deai.md
   > - 选择性应用(指定哪几条)
   > - 否

## 实现备注

- 本文件只做参数解析 + 转调,不实现具体规则
- 不修原文件,任何修订写新文件加 `_deai` 后缀
- 详细规则见 `_internal/deai-pass.md`
- 没有阈值自动通过逻辑,所有判断权在用户
