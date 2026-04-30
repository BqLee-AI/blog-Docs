# 设计

## 设计目标

本变更只冻结 Phase 1 MVP 控制面方案，后续具体文档更新、Issue 创建、FE/BE 修复都应按该方案拆分执行。

核心原则：

- 整个 `blog-Docs` 是控制面，不由单一目录或单一文档承担全部职责。
- 固定层只写长期边界和阶段归属，运行态只写当前状态，验收层按阶段定义测试和证据。
- Phase 1 面向 Agent 时必须足够明确：做什么、做到什么样、不做什么、验收看哪里、完成后回写哪里。

## Phase 1 MVP 定义

Phase 1 命名为：`MVP 最小内容闭环`。

本阶段只交付“账号认证 + 文章读写”的最小博客闭环：

- 用户可以通过邮箱验证码注册。
- 用户可以登录并获得 JWT。
- 登录用户可以创建、编辑、删除并发布文章。
- 游客可以浏览已发布文章列表和文章详情。
- 文章数据必须真实写入 PostgreSQL，并能通过前端再次读取。
- FE 核心验收不得依赖 mock 数据。
- OpenAPI、FE 调用、BE 返回必须逐步对齐。

本阶段明确不做：

- 评论、回复、点赞
- TinyMCE 富文本编辑器
- 图片上传、MinIO、OSS
- 全文搜索
- reCAPTCHA
- 复杂个人中心，如头像上传、资料编辑、密码修改
- 复杂后台 Dashboard 或统计面板
- 完整 CI/CD、监控告警、E2E 自动化测试

## 控制面文件职责

### `docs/control-plane/`

固定层，负责阶段边界和长期决策，不记录临时进度。

- `phase-roadmap.md`
  - 定义 Phase 0/1/2/3/4 的阶段目标和非目标。
  - 说明原始 PRD 中的能力如何被安排到后续阶段。
  - 防止 Agent 把后续阶段能力拉回 Phase 1。

- `mvp-scope.md`
  - 冻结 Phase 1 MVP 范围。
  - 只写当前 MVP 必须做、明确不做、完成判定。
  - 不写 Issue 状态、PR 进度、临时风险。

### `docs/runtime/`

运行层，负责当前阶段状态和回写面板，可高频更新。

- `phase-current.md`
  - 记录当前阶段是 Phase 1。
  - 只放阶段摘要、当前状态、真源引用、任务包入口、回写规则入口。
  - 不承载完整路线图、完整验收细则或 API 字段细节。

- `issue-dispatch.md`
  - 记录 Docs / FE / BE 待下发和已下发 Issue。
  - 允许先记录 `planned` 状态，创建 Issue 后必须回写 URL。

- `risk-register.md`
  - 记录当前阶段风险、影响、缓解动作、触发条件和状态。

- `openapi-gap-register.md`
  - 记录 Phase 1 auth/articles 契约差异。
  - 只作为后续契约修复任务输入，不直接改写正式 OpenAPI。

### `docs/harness/`

验收层，按阶段定义测试、CI/check 项、smoke 和证据要求。

- `phase-1-mvp-harness.md`
  - 定义 Phase 1 的 FE/BE 必跑命令。
  - 定义认证和文章链路 smoke。
  - 定义 PR 必须提交的证据形式。
  - 明确哪些能力不作为 Phase 1 验收项。

### `contracts/api/`

契约层，继续以 `contracts/api/openapi.yaml` 作为 API 唯一真源。

本变更只要求识别 auth/articles 相关差异，并把差异形成后续任务；完整契约大修应拆到后续 Issue 或后续 OpenSpec change。

### `docs/workflow/`

流程层，保留通用推进规则。

- `task-release-and-sync-sop.md` 继续负责任务发布、Ready 门槛和同步节奏。
- `progress-sync-protocol.md` 继续负责回写规则。
- `spec-issue-test-flow.md` 继续负责 Spec -> Issue -> Test 主链路。

### `docs/standards/`

标准层，保留 AGENTS 和仓库接入标准。

后续应根据本方案检查 FE/BE 的 AGENTS 接入状态，但本变更不直接修改 FE/BE AGENTS。

## 后续 Issue 包建议

### Docs Issue 包

- `DOCS-P1-001`：冻结 Phase 1 MVP 范围和阶段路线图。
- `DOCS-P1-002`：更新 Phase 1 runtime 面板。
- `DOCS-P1-003`：建立 Phase 1 harness 文档。
- `DOCS-P1-004`：识别 auth/articles OpenAPI 差异并形成后续契约任务。
- `DOCS-P1-005`：对齐文档仓库入口与引用关系。

### FE Issue 包

- `FE-P1-001`：认证链路接入验收与证据补齐。
- `FE-P1-002`：文章核心链路去 mock，统一真实 API 数据源。
- `FE-P1-003`：FE AGENTS 接入控制面真源和阶段验收要求。

### BE Issue 包

- `BE-P1-001`：认证链路验收、关键测试和契约差异记录。
- `BE-P1-002`：文章读写链路验收、权限测试和契约差异记录。
- `BE-P1-003`：最小 harness/check 脚本或 CI gate 方案确认。

## 回写要求

后续 Issue 完成后必须按职责回写：

- `docs/runtime/issue-dispatch.md`：更新 Issue / PR 链接和状态。
- `docs/runtime/risk-register.md`：更新风险状态或新增风险。
- `docs/runtime/openapi-gap-register.md`：更新 auth/articles 契约差异与后续处理入口。
- `contracts/api/openapi.yaml`：仅当 API 行为已确认并进入契约任务时更新。
- `docs/control-plane/mvp-scope.md`：只有范围边界变化时更新，且必须先走 OpenSpec。
