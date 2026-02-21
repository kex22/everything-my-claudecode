---
name: planning
version: 0.1.0
description: 当用户说"生成计划"、"写实施计划"、"规划实现"时触发。从设计文档生成实施计划，产出到 docs/plans/，支持滚动规划和并行感知。
---

# 编写实施计划

将设计方案转化为可执行的详细实施计划。假设执行者对代码库零上下文，文档一切所需信息。

**开始时宣布：** "我正在使用 plan 技能来生成实施计划。"

## 前置检查

1. 读取 `CLAUDE.md` 了解项目技术栈和规范
2. 读取 `docs/design/` 确认有设计文档可供参考（无则提示先执行 `/kex-dev:design`）
3. 读取 `docs/plans/OVERVIEW.md` 了解已有计划和当前进度（不存在则扫描 plan 文件）

## 滚动规划 + 并行感知

- 分析设计文档中的模块依赖关系
- 无依赖的模块可同批出 plan，有依赖的等前置完成后再规划
- 与用户确认本批次要规划哪些模块

## Bite-Sized 任务粒度

每个步骤是一个独立动作：
- "编写失败的测试" — 一步
- "运行测试确认失败" — 一步
- "编写最少代码让测试通过" — 一步
- "运行测试确认通过" — 一步
- "提交" — 一步

## Plan 文件结构

每个模块一个文件，保存到 `docs/plans/YYYYMMDD-<phase>-<module>.md`（日期为创建当天）：

````markdown
# <模块名> 实施计划

> 状态：待执行
> 依赖：<前置模块列表，无则写"无">

**目标：** 一句话描述本模块要构建什么
**架构：** 2-3 句话描述实现方式
**技术栈：** 关键技术/库

---

### Task 1: [组件名]

**文件：**
- 创建: `exact/path/to/file.rs`
- 测试: `exact/path/to/tests.rs`

**Step 1: 编写失败的测试**
```rust
#[test]
fn test_specific_behavior() {
    // 完整测试代码
}
```

**Step 2: 运行测试确认失败**
运行: `cargo test test_specific_behavior`
预期: FAIL

**Step 3: 编写最少实现**
```rust
// 完整实现代码
```

**Step 4: 运行测试确认通过**
运行: `cargo test test_specific_behavior`
预期: PASS

**Step 5: 提交**
```bash
git add <files>
git commit -m "feat(<plan-name>): add specific feature"  # 由 /kex-dev:commit 技能生成
```
````

## 关键要求

- 精确文件路径，不用模糊描述
- Plan 中包含完整代码，不写"添加验证逻辑"这种占位符
- 精确命令 + 预期输出
- DRY、YAGNI、TDD、频繁提交

## 执行交接

Plan 保存后，提供执行选项：

**"Plan 已保存到 `docs/plans/<filename>.md`。两种执行方式：**
1. **当前 session 继续** — 直接调用 `/kex-dev:execute`
2. **新 session 执行（推荐）** — 清理上下文，新 session 中调用 `/kex-dev:execute`，避免讨论历史干扰

**选择哪种？"**

## 产出规范

- 文件命名：`docs/plans/YYYYMMDD-<phase>-<module>.md`（日期为创建当天）
- 头部必须有状态标记（`待执行` / `进行中 (x/y步完成)` / `已完成`）
- 完成的 plan 移入 `docs/plans/archive/`，注明归档时间和原因
- **Plan 保存后，更新 `docs/plans/OVERVIEW.md`**：在"活跃计划"表格中添加新条目
