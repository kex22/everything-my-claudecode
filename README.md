# everything-my-claudecode

自用 [Claude Code](https://docs.anthropic.com/en/docs/claude-code) 插件集合。

## 插件列表

### kex-dev

kex 项目的 SDD + TDD 开发工作流技能链。

**命令（用户可调用）：**

| 命令 | 说明 |
|------|------|
| `/kex-dev:init` | 项目初始化，介绍工作流和目录结构 |
| `/kex-dev:design` | 脑暴 + 设计，产出 design doc |
| `/kex-dev:plan` | 从设计文档生成实施计划 |
| `/kex-dev:execute` | 按 plan 逐步 TDD 实现 |
| `/kex-dev:commit` | 提交代码，自动关联 plan |
| `/kex-dev:review` | 代码审查 |
| `/kex-dev:test` | 测试验证 |

**内部技能（命令调用，不直接暴露）：** `plans-maintenance`、`research`

## 安装

```bash
claude plugin add --source everything-my-claudecode kex-dev
```
