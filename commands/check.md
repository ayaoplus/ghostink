# /ghostink check

审查任意文本对照当前 form 的合规性。**只出报告,不修原文件。**

本命令是 thin wrapper,实际逻辑在 `_internal/form-check.md`。

## 用法

```
/ghostink check [file-path]
/ghostink check                  # 提示用户输入或选最近草稿
```

## 流程

1. 解析参数:
   - 提供 file-path → Read 该文件
   - 不提供 → 询问用户输入文本,或扫 `[studio]/drafts/` 列最近 3 篇

2. 按 SKILL.md Studio Discovery 规则解析 studio root

3. 加载 `[studio]/form.md`、`[studio]/playbook.md`(若有)

4. **调用** `_internal/form-check.md`,输入文本 + form + playbook

5. 显示报告

6. 询问:

   > 要把 FAIL 项的建议自动应用,生成修订版吗?
   > - 是,产出 <file>_revised.md
   > - 否

   选"是" → 自动改并写到新文件(原文件不动)
   选"否" → 结束

## 实现备注

- 本文件只做参数解析 + 转调,不实现具体规则
- 不修原文件,任何修订都写新文件加 `_revised` 后缀
- 详细规则见 `_internal/form-check.md`
