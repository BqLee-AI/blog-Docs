# 模板编写规范

## 目的

让 Issue、PR、Spec、Harness 模板能直接被人类和 AI Agent 使用，而不是只作为空泛占位。

## 适用范围

- `templates/issue-template.md`
- `templates/pr-template.md`
- `templates/spec-template.md`
- `templates/harness-checklist.md`
- 后续新增的执行模板

## 必做规则

- 模板正文默认中文。
- 路径、命令、API 字段、operationId、包名保留英文。
- 模板必须显式写出工作流参考路径。
- 模板必须包含上游真源、授权范围、验收标准和回写入口。
- 模板字段必须可填写，避免只写抽象说明。
- 实际 Issue / PR 正文不得保留 `# Issue 模板`、`# PR 模板` 等模板标题。

## Issue 模板必须包含

- 工作流入口。
- 决策上下文。
- 输入真源。
- 变更分级。
- 授权修改范围。
- 禁止修改范围。
- 依赖前置条件。
- 验收标准。
- Harness 计划。
- 回写目标。
- 关联对象。

## PR 模板必须包含

- 模板与工作流参考。
- 决策上下文。
- 变更摘要。
- 关联真源。
- 发布标签。
- 原子 PR 边界。
- 冻结边界检查。
- 内容验证。
- Harness 证据。
- 风险与回滚。
- 回写。

## Spec 模板必须包含

- Authoring references。
- Purpose。
- Requirements。
- Scenario。
- Boundaries。
- References。
- Verification。

## Harness 模板必须包含

- Evidence references。
- 通用门禁。
- 仓库或阶段检查项。
- 证据四要素：检查项、命令或人工步骤、结果、证据位置。
- 回写目标。

## 禁止事项

- 不在模板内复制 standards 全文。
- 不把模板写成不可填写的说明文。
- 不使用“按需补充”“自行判断”等没有边界的占位。
- 不把阶段状态写死到通用模板中。

## 验收检查

- 新 Agent 只看模板能否找到工作流入口？
- Reviewer 只看 PR 描述能否验收范围和证据？
- 模板是否能直接复制使用？
- 模板是否避免保留模板标题到实际正文？
- 模板是否把复杂规则引用到 `docs/standards/`，而不是全文重复？
