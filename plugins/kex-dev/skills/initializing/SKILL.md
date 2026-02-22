---
name: initializing
version: 0.3.0
description: 项目初始化时使用。当用户说"初始化项目"、"开始新项目"、"检查项目结构"时触发。介绍开发工作流，检查并注入 CLAUDE.md 规范，确保目录结构就绪。
---

项目开发工作流引导助手。

**开始时宣布：** "我正在使用 init 技能来初始化项目。"

## 前置准备

1. 读取 `${CLAUDE_PLUGIN_ROOT}/shared/conventions.md` 了解文档规范
2. 读取项目根目录的 `CLAUDE.md` 了解项目技术栈和规范
3. 读取 `docs/plans/overview.md` 了解当前进度（不存在则记录为空）
4. 列出 `docs/design/`、`docs/plans/`、`docs/research/` 下的文件名（仅列出，不读取内容）

## 创建 Task 追踪

准备完成后，使用 TaskCreate 创建以下 4 个 task：

1. "介绍开发工作流" — activeForm: "介绍开发工作流"
2. "介绍文档目录结构" — activeForm: "介绍文档目录结构"
3. "检查 CLAUDE.md 规范" — activeForm: "检查 CLAUDE.md 规范"
4. "显示当前进度" — activeForm: "显示当前进度"

然后**逐个执行**每个 task（标记 in_progress → 执行 → 标记 completed），不得跳过或合并。

### Task 1：介绍开发工作流

读取 `${CLAUDE_PLUGIN_ROOT}/skills/initializing/references/workflow.md`，将其内容完整输出给用户。

### Task 2：介绍文档目录结构

输出以下三个小节（内容结合前置准备中掌握的项目实际情况）：

**`docs/design/` — 设计方案（活文档）**
要点：始终反映当前设计而非历史快照，由 `/kex-dev:design` 产出，按域分子目录，文件名不加日期。列出当前项目实际有哪些子目录。

**`docs/plans/` — 实施计划（执行清单）**
要点：由 `/kex-dev:plan` 产出，命名带日期，`overview.md` 自动追踪进度，完成后归档到 `archive/`。列出当前有多少活跃/归档 plan。

**`docs/research/` — 调研文档**
要点：技术选型、竞品分析等支撑材料。

最后确认以下目录存在，不存在则创建：
- `docs/design/`
- `docs/plans/`
- `docs/plans/archive/`
- `docs/research/`

### Task 3：检查 CLAUDE.md 规范

根据前置准备中已读取的 `CLAUDE.md`，逐项检查并输出：

```
1. 工作流程（技能链顺序）    → ✅/❌
2. 文档目录职责              → ✅/❌
3. 实施计划规范              → ✅/❌
4. 提交规范（plan scope）    → ✅/❌
5. 经验教训                  → ✅/❌
```

对照 `${CLAUDE_PLUGIN_ROOT}/shared/conventions.md` 中的命名、状态标记、archive 机制。
如有 ❌，给出具体补充建议并写入 CLAUDE.md。如全部 ✅，简要确认。

### Task 4：显示当前进度

根据前置准备中已读取的 `docs/plans/overview.md`，展示活跃计划状态和下一步建议。
如果 overview.md 不存在，扫描 `docs/plans/` 中的 plan 文件并生成 overview.md。
告知用户下一步应该执行哪个命令。
