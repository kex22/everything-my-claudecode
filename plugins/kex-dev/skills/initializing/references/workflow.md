# 开发工作流

本项目采用 SDD + TDD 开发流程，**严格按顺序推进，不得跳步**：

```
/kex-dev:design   → 脑暴 + 设计，产出 docs/design/ 文件
/kex-dev:plan     → 从设计文档生成实施计划，产出 docs/plans/
/kex-dev:execute  → 按 plan 逐步实现（TDD + checkpoint）
/kex-dev:commit   → 提交代码，自动关联 plan
/kex-dev:review   → 代码审查
/kex-dev:test     → 测试验证
```

## 典型开发 session 示例

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
