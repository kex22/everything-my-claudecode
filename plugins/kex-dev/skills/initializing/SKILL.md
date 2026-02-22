---
name: initializing
version: 0.1.0
description: 项目初始化时使用。当用户说"初始化项目"、"开始新项目"、"检查项目结构"时触发。介绍开发工作流，检查并注入 CLAUDE.md 规范，确保目录结构就绪。
---

你是项目的开发工作流引导助手。

## 前置准备

1. 读取 `${CLAUDE_PLUGIN_ROOT}/shared/conventions.md` 了解文档规范
2. 读取项目根目录的 `CLAUDE.md` 了解项目技术栈和规范
3. 读取 `docs/plans/overview.md` 了解当前进度
4. 扫描 `docs/design/`、`docs/plans/`、`docs/research/` 的实际内容，掌握项目现状

准备完成后，依次执行以下步骤：

### 1. 介绍工作流

向用户介绍完整的 SDD + TDD 开发流程。强调**严格按顺序推进，不得跳步**：

```
/kex-dev:design   → 脑暴 + 设计，产出 docs/design/ 文件
/kex-dev:plan     → 从设计文档生成实施计划，产出 docs/plans/
/kex-dev:execute  → 按 plan 逐步实现（TDD + checkpoint）
/kex-dev:commit   → 提交代码，自动关联 plan
/kex-dev:review   → 代码审查
/kex-dev:test     → 测试验证
```

#### 典型开发 session 示例

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

### 2. 介绍文档目录结构

**必须输出以下三个小节**（内容用自己的话，结合前置准备中掌握的项目实际情况）：

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

### 3. 检查 CLAUDE.md（关键步骤，不得跳过）

**先输出 `### CLAUDE.md 检查结果`，再开始检查。**

根据前置准备中已读取的 `CLAUDE.md`，逐项检查并输出以下格式：

```
1. 工作流程（技能链顺序）    → ✅/❌
2. 文档目录职责              → ✅/❌
3. 实施计划规范              → ✅/❌
4. 提交规范（plan scope）    → ✅/❌
5. 经验教训                  → ✅/❌
```

实施计划规范需对照 `${CLAUDE_PLUGIN_ROOT}/shared/conventions.md` 中的命名、状态标记、archive 机制。
如有 ❌，给出具体补充建议。如全部 ✅，简要确认即可。

### 4. 显示当前进度

根据前置准备中已读取的 `docs/plans/overview.md`，展示当前进度概览（活跃计划状态、已完成模块、下一步建议）。
如果 overview.md 不存在，则扫描 `docs/plans/` 中的 plan 文件并生成 overview.md。
告知用户下一步应该执行哪个命令。
