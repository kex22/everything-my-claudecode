# 共享规范

跨技能共享的约定，各技能通过 `读取 shared/conventions.md` 加载。

## 设计文档结构

采用层级结构（overview + 子文档），便于 AI 按需加载：

```
docs/design/
├── overview.md          ← 顶层索引（定位、架构图、技术栈）
├── roadmap.md           ← Roadmap
├── shared/              ← 跨域共享概念
├── <domain-a>/          ← 域 A 设计
│   └── overview.md      ← 域 A 索引
└── <domain-b>/          ← 域 B 设计
    └── overview.md      ← 域 B 索引
```

- 新增设计内容根据所属域保存到对应子目录
- 跨域共享概念放 `shared/`
- 每个子目录的 `overview.md` 作为该域的索引
- 文件名不加日期，变更记录写在文件内部

**放置示例：**

| 设计内容 | 保存位置 | 理由 |
|----------|----------|------|
| 纯前端的新功能 | `frontend/feature-x.md` | 只涉及前端域 |
| 纯后端的新功能 | `backend/feature-y.md` | 只涉及后端域 |
| 前后端共用的数据格式 | `shared/data-format.md` | 两端都要理解 |
| 修改现有认证流程 | 更新 `backend/auth.md` | 已有文档，追加变更记录 |

## Roadmap（`docs/design/roadmap.md`）

- 按 Phase 组织，每个条目用 checkbox（`- [ ]`/`- [x]`）追踪完成状态
- 首次设计时创建，后续设计时追加新功能到对应 Phase
- 归档 plan 时同步勾选对应条目
- 不存在则创建，已存在则追加，不重写已有条目

## 实施计划（`docs/plans/`）

**命名**：`YYYYMMDD-<module>.md`（日期为创建当天）

**状态标记**（文件头部）：`待执行` / `进行中 (x/y步完成)` / `已完成`

**生命周期**：
- `docs/plans/` 只保留待执行和进行中的 plan
- 完成后移入 `docs/plans/archive/`，头部追加 `> 归档时间：YYYY-MM-DD`

**`docs/plans/overview.md` 格式**：

```markdown
# 实施计划概览

## 当前阶段：<阶段名>

### 活跃计划
| 计划 | 性质 | 优先级 | 依赖 | 状态 |
（只列待执行和进行中的 plan）

### 执行建议
（下一步做什么）

### 近期归档
（只保留最近一个已完成 phase 的摘要，更早的折叠为一行）
```

- "近期归档"只保留上一个已完成 phase 的条目
- 更早的 phase 折叠为一行，如：`Phase 1 (6 plans) — 已归档`
