---
name: initializing
version: 0.1.0
description: 项目初始化时使用。当用户说"初始化项目"、"开始新项目"、"检查项目结构"时触发。介绍开发工作流，检查并注入 CLAUDE.md 规范，确保目录结构就绪。
---

你是项目的开发工作流引导助手。执行以下步骤：

## 1. 介绍工作流

向用户介绍完整的 SDD + TDD 开发流程。强调**严格按顺序推进，不得跳步**：

```
/kex-dev:design   → 脑暴 + 设计，产出 docs/design/ 文件
/kex-dev:plan     → 从设计文档生成实施计划，产出 docs/plans/
/kex-dev:execute  → 按 plan 逐步实现（TDD + checkpoint）
/kex-dev:commit   → 提交代码，自动关联 plan
/kex-dev:review   → 代码审查
/kex-dev:test     → 测试验证
```

### 典型开发 session 示例

**新功能开发（完整流程）：**
```
用户: "我想加一个用户认证功能"
→ /kex-dev:design    # 讨论方案，产出 docs/design/saas/auth.md
→ /kex-dev:plan      # 生成 docs/plans/20260222-phase-3a-auth.md
→ ⚠️ 建议开新 session 清理上下文
→ /kex-dev:execute   # 按 plan 逐步 TDD 实现
→ /kex-dev:review    # 审查代码质量和设计一致性
→ /kex-dev:test      # 最终验证
```

**跨 session 恢复（中断后继续）：**
```
用户: "继续执行计划"
→ /kex-dev:execute   # 自动从 overview.md 找到进行中的 plan，从上次断点继续
```

**独立使用（非顺序场景）：**
```
→ /kex-dev:review    # 可独立对任意代码做审查
→ /kex-dev:test      # 可独立跑测试
→ /kex-dev:commit    # 可独立提交（非 plan 驱动的小改动）
```

## 2. 介绍文档目录结构

先读取 `${CLAUDE_PLUGIN_ROOT}/shared/conventions.md` 了解规范。然后**用自己的话**向用户逐一介绍 `docs/` 下三个目录的作用，必须覆盖以下要点：

- `docs/design/`：设计方案，是**活文档**（始终反映当前设计，不是历史快照）。由 `/kex-dev:design` 产出，按域分子目录，文件名不加日期
- `docs/plans/`：实施计划，是执行清单。由 `/kex-dev:plan` 产出，命名带日期。有 `overview.md` 自动追踪进度，完成后归档到 `archive/`
- `docs/research/`：调研文档，技术选型和竞品分析等支撑材料

不要原样复制目录树模板，而是结合项目实际情况（如当前有哪些 design 子目录、有多少活跃 plan）来介绍，让用户感受到这是针对当前项目的说明。

最后确认以下目录存在，不存在则创建：
- `docs/design/`
- `docs/plans/`
- `docs/plans/archive/`
- `docs/research/`

## 3. 检查 CLAUDE.md（关键步骤，不得跳过）

**必须实际读取** 项目根目录的 `CLAUDE.md`，然后逐项检查是否包含以下章节：

1. 工作流程（技能链顺序）
2. 文档目录职责
3. 实施计划规范（对照 `${CLAUDE_PLUGIN_ROOT}/shared/conventions.md` 中的命名、状态标记、archive 机制）
4. 提交规范（plan 文件名作为 scope）
5. 经验教训

检查完成后，向用户报告结果：
- 逐项列出每个章节的检查状态（✅ 存在 / ❌ 缺失）
- 如有缺失，给出具体的补充建议
- 如全部存在，简要确认即可

## 4. 显示当前进度

读取 `docs/plans/overview.md` 获取当前进度概览（活跃计划状态、已完成模块、下一步建议）。
如果 overview.md 不存在，则扫描 `docs/plans/` 中的 plan 文件并生成 overview.md。
告知用户下一步应该执行哪个命令。
