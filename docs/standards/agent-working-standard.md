# Agent 工作规范

## 目的

让 AI Agent 在 `blog-Docs`、`blog-FE`、`blog-BE` 协作时按同一套上下文入口、边界判断、验收和回写方式执行任务。

## 适用范围

- 文档仓库控制面任务
- 从文档仓库下发到 FE/BE 的 Issue
- 需要引用 Spec、Contract、Harness 或 runtime 回写的 PR

## 开工顺序

1. 读取当前仓库 `AGENTS.md`。
2. 读取 Issue 中列出的“工作流入口”和“输入真源”。
3. 判断变更级别：
   - L0：不扩边界、不改契约、不改变验收门槛，可直接执行。
   - L1：涉及 scope、contract、harness、gate、长期规则，先走 OpenSpec。
   - L2：边界冲突、安全风险、P0 阻塞，先停下并回报。
4. 只加载本任务需要的高权重文档，避免历史材料污染。
5. 按 Issue 授权范围修改文件。
6. 提交 PR 时提供真源、范围、验收证据、风险与回写说明。
7. 合并后回写 runtime 状态、风险或经验。

## 必做规则

- 默认使用中文写 Issue、PR、文档正文；路径、命令、API 字段、operationId、包名、分支名保留英文。
- 一个 Issue 应有明确输入真源、授权修改范围、禁止修改范围、DoD、Harness 和回写目标。
- 一个 PR 只承载一个 Issue 或一个职责边界；多个独立任务必须拆 PR。
- PR 描述必须引用 Issue、Spec 或 Contract/Rule、验收标准和回写入口。
- 验收证据必须包含检查项、命令或人工步骤、结果、证据位置。
- 发现长期有效的新约束时，优先判断是否应回写到 `AGENTS.md` 或 `docs/standards/`。

## 禁止事项

- 不得在没有 OpenSpec 的情况下修改 scope、contract、harness gate 或长期治理规则。
- 不得把临时状态、聊天过程或无复用价值的中间想法写入固定层文档。
- 不得把已由 git diff 表达的琐碎修改再写成稳定规则。
- 不得用一个大 PR 混合多个可独立评审的控制面任务。
- 不得在 PR 正文保留 `# PR 模板`、`# Issue 模板` 这类模板标题。

## 验收检查

- Issue 是否写明工作流入口和输入真源？
- PR 是否按模板写明决策上下文、变更摘要、关联真源、验证证据、风险与回写？
- 改动文件是否位于正确分层目录？
- 是否需要 OpenSpec？如果需要，是否已有已确认的 change？
- 合并后是否有 runtime 回写入口？

## 参考

- Agent 入口：`AGENTS.md`
- 文档落点：`docs/standards/document-placement-standard.md`
- 命名规范：`docs/standards/naming-standard.md`
- 模板规范：`docs/standards/template-authoring-standard.md`
- Issue 模板：`templates/issue-template.md`
- PR 模板：`templates/pr-template.md`
