---
name: initializing
version: 0.5.0
description: 项目初始化时使用。当用户说"初始化项目"、"开始新项目"、"检查项目结构"时触发。介绍开发工作流，检查并注入 CLAUDE.md 规范，确保目录结构就绪。
---

项目开发工作流引导助手。

**开始时宣布：** "我正在使用 init 技能来初始化项目。"

## 第一步：创建 Task 追踪

立即使用 TaskCreate 创建以下 5 个 task（不做任何文件读取）：

1. "介绍开发工作流" — activeForm: "介绍开发工作流"
2. "确认目录结构" — activeForm: "确认目录结构"
3. "检查并注入 CLAUDE.md" — activeForm: "检查 CLAUDE.md 规范"
4. "介绍文档目录用途" — activeForm: "介绍文档目录用途"
5. "显示当前进度" — activeForm: "显示当前进度"

然后**逐个执行**每个 task（标记 in_progress → 读取所需文件 → 执行 → 标记 completed）。

### Task 1：介绍开发工作流

将以下工作流文档的内容完整输出给用户：

@${CLAUDE_PLUGIN_ROOT}/skills/initializing/references/workflow.md

### Task 2：确认目录结构

确认以下目录存在，不存在则创建：
- `docs/design/`
- `docs/plans/`
- `docs/plans/archive/`
- `docs/research/`

输出创建结果或确认已存在。

### Task 3：检查并注入 CLAUDE.md

准备：读取项目根目录 `CLAUDE.md`。然后对照以下规范文档逐项检查：

@${CLAUDE_PLUGIN_ROOT}/shared/conventions.md

```
1. 工作流程（技能链顺序）    → ✅/❌
2. 文档目录职责              → ✅/❌
3. 实施计划规范              → ✅/❌
4. 提交规范（plan scope）    → ✅/❌
5. 经验教训                  → ✅/❌
```

对照 conventions.md 中的命名、状态标记、archive 机制。
如有 ❌，给出具体补充建议并写入 CLAUDE.md。如全部 ✅，简要确认。

### Task 4：介绍文档目录用途

准备：列出 `docs/design/`、`docs/plans/`、`docs/research/` 下的文件名（仅列出，不读取内容）。

然后输出以下三个小节（结合项目实际情况）：

**`docs/design/` — 设计方案（活文档）**
要点：始终反映当前设计而非历史快照，由 `/kex-dev:design` 产出，按域分子目录，文件名不加日期。列出当前项目实际有哪些子目录。

**`docs/plans/` — 实施计划（执行清单）**
要点：由 `/kex-dev:plan` 产出，命名带日期，`overview.md` 自动追踪进度，完成后归档到 `archive/`。列出当前有多少活跃/归档 plan。

**`docs/research/` — 调研文档**
要点：技术选型、竞品分析等支撑材料。

### Task 5：显示当前进度

准备：读取 `docs/plans/overview.md`（不存在则扫描 `docs/plans/` 下的 plan 文件并生成 overview.md）。

然后展示活跃计划状态和下一步建议。告知用户下一步应该执行哪个命令。
