# phase-1-mvp-control-plane-plan 能力规范

## 目的

定义 Phase 1 MVP 控制面方案，使 `blog-Docs` 能准确指导后续 Docs / FE / BE Issue 下发、验收和回写。

## ADDED Requirements

### Requirement: Phase 1 范围必须明确

Phase 1 MUST 被定义为“账号认证 + 文章读写持久化”的最小博客闭环。

#### Scenario: Agent 读取 Phase 1 范围

- **GIVEN** Agent 接到 Phase 1 任务
- **WHEN** 它读取控制面范围文档
- **THEN** 它必须知道本阶段包含邮箱验证码注册/登录、JWT 鉴权、文章创建、文章编辑、文章删除、文章发布、文章列表、文章详情和 PostgreSQL 持久化
- **AND** 它必须知道评论、富文本、图片上传、全文搜索、reCAPTCHA 不属于 Phase 1

#### Scenario: Agent 遇到范围外能力

- **GIVEN** Agent 在 Phase 1 任务中遇到评论、富文本、图片上传、全文搜索或 reCAPTCHA
- **WHEN** 该能力没有被 PM 通过 OpenSpec 重新纳入 Phase 1
- **THEN** Agent 必须停止把该能力作为本阶段实现目标
- **AND** Agent 必须把它记录为后续阶段或范围变更问题

### Requirement: 控制面文件必须各司其职

Phase 1 控制面方案 MUST 把阶段边界、当前状态、验收证据、API 契约、流程规则和接入标准分配到不同层级文件中。

#### Scenario: Agent 查找阶段边界

- **GIVEN** Agent 需要判断 Phase 1 做什么和不做什么
- **WHEN** 它读取控制面真源
- **THEN** 它必须被指向 `docs/control-plane/mvp-scope.md` 和 `docs/control-plane/phase-roadmap.md`
- **AND** 不应从 `docs/runtime/issue-dispatch.md` 推断长期范围边界

#### Scenario: Agent 查找当前阶段状态

- **GIVEN** Agent 需要判断当前 Phase 1 状态、任务入口或风险
- **WHEN** 它读取运行态真源
- **THEN** 它必须被指向 `docs/runtime/phase-current.md`、`docs/runtime/issue-dispatch.md` 和 `docs/runtime/risk-register.md`
- **AND** 不应把 `docs/control-plane/mvp-scope.md` 当作实际完成度证明

#### Scenario: Agent 查找阶段验收要求

- **GIVEN** Agent 需要判断 Phase 1 任务需要跑哪些测试、CI/check 和 smoke
- **WHEN** 它读取验收真源
- **THEN** 它必须被指向 `docs/harness/phase-1-mvp-harness.md`
- **AND** 不应把验收细则塞进 `docs/runtime/phase-current.md`

### Requirement: 后续能力必须有阶段归属

Phase 1 之外的能力 MUST 在阶段路线图中有明确归属，避免被 Agent 随机拉回当前阶段。

#### Scenario: 原始 PRD 包含评论

- **GIVEN** 原始 PRD 包含评论能力
- **WHEN** Phase 1 范围被评估
- **THEN** 评论必须在 `docs/control-plane/phase-roadmap.md` 中被标记为后续阶段能力，除非 PM 通过 OpenSpec 将其重新纳入 Phase 1

#### Scenario: 原始 PRD 包含富文本、图片上传、全文搜索和 reCAPTCHA

- **GIVEN** 原始 PRD 包含富文本、图片上传、全文搜索和 reCAPTCHA
- **WHEN** Phase 1 范围被评估
- **THEN** 这些能力必须在阶段路线图中被标记为后续阶段能力
- **AND** 不得作为 Phase 1 MVP 完成判定

### Requirement: Runtime 必须暴露当前项目真实状态

运行层 MUST 记录当前阶段状态、任务入口、风险和 auth/articles 契约差异，避免 mock 残留或契约差异被误判为完成。

#### Scenario: FE 页面仍依赖 mock 数据

- **GIVEN** FE 某个 Phase 1 核心链路仍依赖 mock 数据或 localStorage 假数据
- **WHEN** 该风险影响 Phase 1 验收
- **THEN** 它必须被记录到 `docs/runtime/risk-register.md`
- **AND** 不得作为 Phase 1 完成证据

#### Scenario: BE 能力已实现但缺少契约对齐

- **GIVEN** BE 某个 auth 或 articles 能力已有实现
- **WHEN** OpenAPI 尚未准确覆盖其请求、响应、错误码或状态语义
- **THEN** `docs/runtime/openapi-gap-register.md` 必须记录对应 gap
- **AND** `issue-dispatch.md` 必须能承接后续契约对齐任务

### Requirement: Phase 1 Harness 必须按阶段定义

Phase 1 MUST 有独立的阶段验收文档，定义本阶段必要测试、CI/check、smoke 和证据，不与其他阶段混写。

#### Scenario: FE PR 声称完成 Phase 1 认证或文章任务

- **GIVEN** FE PR 声称完成 Phase 1 认证或文章任务
- **WHEN** Reviewer 验收该 PR
- **THEN** PR 必须提供 `bun run build` 和 `bun run test` 的结果
- **AND** 必须提供与该任务相关的 smoke 证据
- **AND** 必须说明核心验收是否仍依赖 mock

#### Scenario: BE PR 声称完成 Phase 1 认证或文章任务

- **GIVEN** BE PR 声称完成 Phase 1 认证或文章任务
- **WHEN** Reviewer 验收该 PR
- **THEN** PR 必须提供 `go test ./...`、`go vet ./...`、`go build ./...` 的结果
- **AND** 必须提供与该任务相关的 smoke 或测试证据
- **AND** 涉及 API 行为时必须引用 OpenAPI 路径或记录 OpenAPI gap

### Requirement: Issue 下发必须支持 Docs / FE / BE 三仓库

运行层 Issue 清单 MUST 支持先规划任务包，再回写真实 GitHub Issue 和 PR 链接。

#### Scenario: 后续 Issue 尚未创建

- **GIVEN** Phase 1 控制面方案已识别一个后续任务包
- **WHEN** 该任务尚未创建 GitHub Issue
- **THEN** `docs/runtime/issue-dispatch.md` 可以把它记录为 `planned`
- **AND** 创建 Issue 后必须回写 URL、状态和真源引用

#### Scenario: Issue 完成后需要回写

- **GIVEN** Docs、FE 或 BE 的 Phase 1 Issue 完成
- **WHEN** PR 合并或任务被判定完成
- **THEN** 必须回写 `docs/runtime/issue-dispatch.md`
- **AND** 如影响风险，必须回写 `docs/runtime/risk-register.md`

## 边界

- 范围内：Phase 1 控制面方案、文件职责、Issue 包规划、阶段验收模型、OpenAPI 差异识别要求。
- 范围外：FE/BE 代码实现、完整 OpenAPI 大修、评论/富文本/图片上传/全文搜索/reCAPTCHA 实现。

## 参考

- 当前 MVP 范围：`docs/control-plane/mvp-scope.md`
- 当前阶段：`docs/runtime/phase-current.md`
- API 契约：`contracts/api/openapi.yaml`
- 任务发布：`docs/workflow/task-release-and-sync-sop.md`
- 进度回写：`docs/workflow/progress-sync-protocol.md`
