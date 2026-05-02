# FE/BE 接入门禁

## 目的

确保 `blog-FE` 与 `blog-BE` 的日常开发真实接入 `blog-Docs` 控制面，而不是只在文档仓库中保留规则。

## 适用范围

- 从 `blog-Docs` 下发到 `blog-FE`、`blog-BE` 的 Issue。
- FE/BE 仓库的 PR、CI、smoke、回写。
- FE/BE 仓库级 `AGENTS.md`。

## Issue 准入门禁

进入 Ready 前，Issue 必须包含：

- 工作流入口：Issue 模板、PR 模板、Harness 清单、Agent 工作规范。
- 输入真源：Spec、Contract/Rule、OpenSpec change（如适用）。
- 授权修改范围和禁止修改范围。
- DoD 与 Harness 计划。
- 回写目标。

没有这些字段的 Issue 不应下发给 Agent 执行。

## PR 合并门禁

合并前，PR 必须包含：

- 关联 Issue。
- 关联 Spec 或 Contract/Rule。
- 原子 PR 边界说明。
- Harness 证据。
- 风险与回滚。
- 回写说明。

涉及 API 行为、契约语义、状态码、认证方式的 PR，必须引用 `contracts/` 真源或记录 OpenAPI gap。

## AGENTS 接入门禁

FE/BE 仓库 `AGENTS.md` 必须声明：

- 本仓库职责与非目标。
- 从 `blog-Docs` 读取控制面真源的顺序。
- 本仓库最小验证命令。
- PR 证据要求。
- 文档回写要求。

## CI 与 Harness 门禁

- FE 至少提供构建检查，Phase 1 相关任务应提供页面或流程 smoke。
- BE 至少提供 build/test/vet 检查，Phase 1 相关任务应提供接口或服务 smoke。
- 自动 CI 不足时，PR 描述必须提供人工验证步骤和证据位置。
- 验证失败时，PR 不进入合并状态。

## 发版门禁

- 候选版冻结后只允许阻塞修复。
- 未通过最小 smoke 不得发布。
- 发布或回滚必须回写 runtime 状态、风险和经验。

## 最低落地动作

- 将 `templates/issue-template.md` 和 `templates/pr-template.md` 的字段映射到 FE/BE Issue/PR。
- 在 FE/BE 仓库 CI 或人工审核中启用 `templates/harness-checklist.md`。
- 将联调与发版记录回写到 `docs/runtime/`。

## 验收检查

- FE/BE Issue 是否能从正文找到控制面入口？
- FE/BE PR 是否能从正文证明范围、证据和回写？
- FE/BE AGENTS 是否足够短，并指向 `blog-Docs` 真源？
- PR 合并后是否回写到 runtime？
