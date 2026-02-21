---
name: committing
version: 0.1.0
description: 提交代码：自动关联当前执行的 plan，生成规范化 commit message。当用户说"提交"、"commit"时触发。也被 executing 技能内部调用。
---

# 提交代码

根据当前执行的 plan 生成规范化的 commit message。

## Commit 格式

```
<type>(<plan-name>): <description>
```

- `<plan-name>`：当前 plan 文件名去掉 `.md`，如 `20260221-phase-2-config`
- `<type>`：`feat` / `fix` / `refactor` / `test` / `docs` / `chore`
- `<description>`：简短描述本次变更

示例：
```
feat(20260221-phase-2-config): add config file parser
test(20260221-phase-2-config): add unit tests for key binding validation
fix(20260221-phase-2-config): handle missing config file gracefully
```

## 流程

1. 读取 `docs/plans/OVERVIEW.md` 确定当前正在执行的 plan
2. 如果无法确定（多个进行中或无进行中），询问用户
3. 用 `git diff --staged` 分析已暂存的变更
4. 生成符合格式的 commit message，提交前展示给用户确认
