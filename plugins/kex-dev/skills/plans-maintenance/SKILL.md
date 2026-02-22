---
name: plans-maintenance
version: 0.1.0
description: Plans 文件夹管家：一致性检查、归档、OVERVIEW 维护。被 planning/executing 技能调用，也可独立触发。当用户说"检查计划"、"整理plans"、"归档"时触发。
---

# Plans 文件夹维护

统一管理 `docs/plans/` 的生命周期，确保一致性。

**开始时宣布：** "我正在检查 plans 文件夹一致性。"

## 触发场景

- planning skill 创建新 plan 后调用（更新 OVERVIEW）
- executing skill 完成 plan 后调用（归档 + 更新 OVERVIEW）
- 用户手动触发（全量一致性检查）

## 一致性检查

扫描 `docs/plans/` 目录，检查：

1. **孤儿 plan**：文件状态标"已完成"但未移入 `archive/` → 自动归档
2. **OVERVIEW 漂移**：活跃计划表与实际文件不一致 → 修正
3. **缺失状态头**：plan 文件没有状态标记 → 提醒补充

## 归档流程

将已完成的 plan 移入 `docs/plans/archive/`：

1. 在文件头部追加归档信息：`> 归档时间：YYYY-MM-DD`
2. `mv` 文件到 `archive/`
3. 更新 overview.md

## overview.md 格式规范

```markdown
# 实施计划概览

## 当前阶段：<阶段名>

### 活跃计划
| 计划 | 性质 | 优先级 | 依赖 | 状态 |
（只列待执行和进行中的 plan）

### 依赖图
（如有依赖关系则画出）

### 执行建议
（下一步做什么）

### 近期归档
（只保留最近一个已完成 phase 的摘要，更早的折叠为一行）
```

关键规则：
- "近期归档"只保留上一个已完成 phase 的条目
- 更早的 phase 折叠为一行，如：`Phase 1 (6 plans) — 已归档`
- 避免列表无限增长

## 被其他 skill 调用时

- **planning 调用**：只需更新 OVERVIEW 的活跃计划表
- **executing 调用**：归档文件 + 更新 OVERVIEW（活跃→近期归档）
