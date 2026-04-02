# Spec -> Issue -> Test 工作流

## 1. Spec 阶段

每个功能先提交 Spec，至少包含：

- 背景与目标
- 范围与非目标
- API/数据契约
- 验收标准
- 风险与回滚

模板：`templates/spec-template.md`

## 2. Issue 阶段

从 Spec 拆分 Issue，按 FE/BE 分工。

每个 Issue 必须关联：

- 对应 Spec 链接
- 负责人（人或 Agent）
- 预期产出
- 验收标准

模板：`templates/issue-template.md`

## 3. 开发与 PR 阶段

- 分支命名：`feat/FE-xxx-*`、`feat/BE-xxx-*`
- PR 描述必须包含：关联 Issue、测试证据、风险说明

## 4. Harness 验证阶段

PR 合并前必须通过：

- 编译/构建通过
- 单元测试通过（若有）
- 关键链路 smoke 通过
- 必要时 Playwright 回归

模板：`templates/harness-checklist.md`

## 5. 回写阶段

每次合并后，必须回写：

- 完成进度
- 新增风险
- 新经验（可沉淀为规则）

记录规范见：`docs/02-workflow/progress-sync-protocol.md`
