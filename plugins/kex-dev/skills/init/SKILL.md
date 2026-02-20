---
name: init
version: 0.1.0
description: 项目初始化时使用。当用户说"初始化项目"、"开始新项目"、"检查项目结构"时触发。介绍开发工作流，检查并注入 CLAUDE.md 规范，确保目录结构就绪。
---

你是项目的开发工作流引导助手。执行以下步骤：

## 1. 介绍工作流

向用户介绍项目的完整开发流程：

```
/design  → 脑暴 + 设计，产出 docs/design/ 文件
/plan    → 从设计文档生成实施计划，产出 docs/plans/
/execute → 按 plan 逐步实现（TDD + checkpoint）
/review  → 代码审查
/test    → 测试验证
```

每个环节不可跳步，必须按顺序推进。

## 2. 检查项目结构

确认以下目录存在，不存在则创建：
- `docs/design/`
- `docs/plans/`
- `docs/plans/archive/`
- `docs/research/`

## 3. 检查 CLAUDE.md

读取项目根目录的 `CLAUDE.md`，确认包含以下关键章节：
- 工作流程（技能链顺序）
- 文档目录职责
- 实施计划规范（滚动规划、状态标记、archive 机制）
- 经验教训

如果缺失任何章节，提示用户并提供补充建议。

## 4. 显示当前进度

扫描 `docs/plans/` 中的 plan 文件，显示当前进行中和待执行的计划。
扫描 `docs/plans/archive/` 显示已完成的计划数量。
告知用户下一步应该执行哪个命令。
