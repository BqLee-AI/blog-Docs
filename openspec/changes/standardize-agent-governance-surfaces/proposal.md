# 统一 Agent-facing Governance Surfaces

## 为什么做

当前 `docs/standards/` 与 `templates/` 已能提供基本入口，但还不足以稳定约束 AI Agent 的日常行为：

- `AGENTS.md` 与标准文档之间的职责边界不够清楚，Agent 容易跳过长文档或把临时状态写进固定层。
- Issue / PR / Spec / Harness 模板缺少显式工作流入口，新协作者或 Agent 不一定知道应该参考哪些路径。
- Issue、PR、commit、branch、OpenSpec change 的命名风格不统一，容易出现中英混杂、标题泛化、重复前缀等问题。
- `docs/standards/` 的部分文档偏原则化，不足以回答“这条规则应该放在哪里、什么不需要记录、如何验收”。

因此需要用一个独立 OpenSpec change 统一 Agent 可直接接触到的治理表面：`AGENTS.md`、`docs/standards/`、`templates/`、README 入口和最小验证方式。

## 变更内容

本变更将：

- 明确哪些硬规则必须固化到 `AGENTS.md`，哪些长期标准归入 `docs/standards/`，哪些运行态信息归入 `docs/runtime/`，哪些内容不应记录。
- 建立 Agent 工作规范、命名规范、文档落点规范、模板编写规范。
- 优化 Issue / PR / Spec / Harness 模板，使其显式声明模板来源、上游真源、验收入口和回写入口。
- 统一默认语言和标题风格：正文默认中文，技术标识、命令、路径、API 字段保留英文。
- 对齐 README 与 AGENTS 的入口说明，确保 Agent 不需要猜测工作流上下文。

## 影响范围

- 受影响仓库：`blog-Docs`
- 受影响文件层：
  - `AGENTS.md`
  - `README.md`
  - `docs/standards/`
  - `templates/`
  - `openspec/changes/standardize-agent-governance-surfaces/`

## 范围外

- 不修改 Phase 1 MVP scope。
- 不修改 `docs/control-plane/mvp-scope.md` 或 `docs/control-plane/phase-roadmap.md`。
- 不执行 OpenAPI 契约大修。
- 不直接同步模板到 `blog-FE` 或 `blog-BE`。
- 不改 FE/BE 业务代码。
- 不把历史聊天、临时想法、一次性命令输出沉淀为稳定规则。

## 验证影响

本变更完成后，后续 Issue / PR 应能直接引用：

- Agent 工作规范：`docs/standards/agent-working-standard.md`
- 命名规范：`docs/standards/naming-standard.md`
- 文档落点规范：`docs/standards/document-placement-standard.md`
- 模板编写规范：`docs/standards/template-authoring-standard.md`
- Issue 模板：`templates/issue-template.md`
- PR 模板：`templates/pr-template.md`
- Spec 模板：`templates/spec-template.md`
- Harness 清单：`templates/harness-checklist.md`

验收时应执行：

- `openspec validate standardize-agent-governance-surfaces`
- 人工检查模板正文是否包含工作流入口、真源、范围、验收和回写字段
- 人工检查 README / AGENTS 是否只承载高频硬规则，不重复 standards 全文
