# 任务清单

## 1. OpenSpec

- [x] 输入：现有 `AGENTS.md`、`docs/standards/`、`templates/`、README。
- [x] 输出：`openspec/changes/standardize-agent-governance-surfaces/`。
- [x] 验收：`openspec validate standardize-agent-governance-surfaces` 通过。
- [ ] 回写：本 change 归档前更新相关 runtime 状态。

## 2. Standards

- [x] 输入：`docs/standards/agents-md-standard.md`、`fe-be-adoption-gate.md`、`spec-writing-standard.md`。
- [x] 输出：新增或更新 Agent 工作、命名、文档落点、模板编写、Spec 写作、FE/BE 接入标准。
- [x] 验收：每个标准都说明目的、适用范围、必做规则、禁止事项、验收检查。
- [x] 回写：README 目录结构包含新增标准。

## 3. Templates

- [x] 输入：`templates/issue-template.md`、`pr-template.md`、`spec-template.md`、`harness-checklist.md`。
- [x] 输出：模板显式包含工作流入口、真源、范围、验收、回写字段。
- [x] 验收：模板可直接复制到 Issue / PR 使用，不保留“模板标题”作为实际正文。
- [x] 回写：`docs/standards/template-authoring-standard.md` 描述模板字段规则。

## 4. Entrypoints

- [x] 输入：`AGENTS.md`、`README.md`。
- [x] 输出：入口说明只保留高频硬规则和路径索引。
- [x] 验收：AGENTS 不重复 standards 全文；README 目录结构与实际文件一致。
- [x] 回写：如发现新的长期硬约束，回写到 `AGENTS.md` 或对应 standard。

## 5. Validation

- [x] 执行 `openspec validate standardize-agent-governance-surfaces`。
- [x] 执行路径检查，确认新增/引用文件存在。
- [x] 检查模板默认中文、技术标识保留英文。
- [x] 检查没有修改 Phase 1 MVP scope、OpenAPI 契约或 FE/BE 仓库文件。
