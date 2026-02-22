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

读取 `${CLAUDE_PLUGIN_ROOT}/shared/conventions.md` 了解规范，然后向用户介绍以下目录职责：

### `docs/design/` — 设计方案（活文档）

由 `/kex-dev:design` 产出。采用层级结构：

```
docs/design/
├── overview.md          ← 顶层索引（定位、架构图、技术栈）
├── roadmap.md           ← Roadmap（按 Phase 用 checkbox 追踪）
├── shared/              ← 跨域共享概念
├── <domain-a>/          ← 域 A（如 cli/、saas/）
│   ├── overview.md      ← 域索引
│   └── feature-x.md    ← 具体功能设计
└── <domain-b>/
    └── overview.md
```

关键规则：文件名不加日期，变更记录写在文件内部。设计文档是**活文档**，始终反映当前正确设计。

### `docs/plans/` — 实施计划（执行清单）

由 `/kex-dev:plan` 产出。命名格式：`YYYYMMDD-<phase>-<module>.md`

```
docs/plans/
├── overview.md                          ← 自动维护的进度概览
├── 20260222-phase-3a-dashboard-ui.md    ← 活跃 plan（待执行/进行中）
└── archive/                             ← 已完成的 plan 归档于此
    └── 20260221-phase-2-config.md
```

生命周期：待执行 → 进行中 → 已完成（移入 archive/）

### `docs/research/` — 调研文档

技术调研、竞品分析等支撑材料，供设计阶段参考。

确认以下目录存在，不存在则创建：
- `docs/design/`
- `docs/plans/`
- `docs/plans/archive/`
- `docs/research/`

## 3. 检查 CLAUDE.md

读取项目根目录的 `CLAUDE.md`，确认包含以下关键章节：
- 工作流程（技能链顺序）
- 文档目录职责
- 实施计划规范（参照 `${CLAUDE_PLUGIN_ROOT}/shared/conventions.md` 中的命名、状态标记、archive 机制）
- 提交规范（plan 文件名作为 scope，详见 `/kex-dev:commit` 技能）
- 经验教训

如果缺失任何章节，提示用户并提供补充建议。

## 4. 显示当前进度

读取 `docs/plans/overview.md` 获取当前进度概览（活跃计划状态、已完成模块、下一步建议）。
如果 overview.md 不存在，则扫描 `docs/plans/` 中的 plan 文件并生成 overview.md。
告知用户下一步应该执行哪个命令。
