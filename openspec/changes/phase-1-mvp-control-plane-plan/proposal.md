# Phase 1 MVP 控制面方案

## 为什么做

当前 `blog-Docs`、`blog-FE`、`blog-BE` 与原始 PRD 之间存在范围和进度错位：

- 原始 PRD 包含评论、富文本、图片上传、全文搜索、reCAPTCHA 等能力。
- 当前 FE/BE 实际更接近“账号认证 + 文章读写”的最小闭环。
- 当前 `docs/runtime/phase-current.md` 仍偏模板状态，不能稳定指导阶段推进。
- 当前 `docs/control-plane/mvp-scope.md` 中仍包含评论等未落地能力，容易让 Agent 抓错 Phase 1 真源。
- FE/BE 的真实完成度、mock 残留、契约差异尚未被清晰回写到控制面。

因此需要先用 OpenSpec 统筹并冻结 Phase 1 MVP 控制面方案。这个变更不直接执行所有后续修改，而是确定后续文档仓库、前端仓库、后端仓库应该按什么边界、文件职责、验收模型和回写方式继续推进。

## 变更内容

本变更确定 Phase 1 MVP 控制面方案，包括：

- Phase 1 MVP 范围与非目标。
- 控制面各层文件职责：哪些内容属于 `control-plane`、`runtime`、`harness`、`contracts`、`workflow`、`standards`。
- 后续 Docs / FE / BE Issue 包拆分方式。
- Phase 1 验收与证据模型，包括本阶段必要的测试、CI/check 项、smoke 和参考证据。
- OpenAPI 差异识别要求：先识别 auth/articles 相关差异并形成后续任务，不在本变更中膨胀为完整契约大修。

## 影响范围

- 受影响仓库：`blog-Docs`
- 下游仓库：`blog-FE`、`blog-BE`
- 受影响控制面层：
  - `docs/control-plane/`
  - `docs/runtime/`
  - `docs/harness/`
  - `contracts/api/openapi.yaml`
  - `docs/workflow/`
  - `docs/standards/`

## 范围外

- 不直接修改 FE/BE 业务代码。
- 不直接创建或执行 FE/BE Issue。
- 不在本变更中完成 OpenAPI 契约大修。
- 不直接实现评论、富文本、图片上传、全文搜索、reCAPTCHA。
- 不把所有阶段内容塞进一个文档。
- 不把运行态状态写进高权重固定层文档。

## 验证影响

本变更会确定 Phase 1 的阶段性验收模型。后续 Issue 执行时，至少应能引用：

- FE 最小检查：`bun run build`、`bun run test`
- BE 最小检查：`go test ./...`、`go vet ./...`、`go build ./...`
- 认证 smoke：验证码、注册、登录、当前用户、受保护入口
- 文章 smoke：创建、列表、详情、编辑、删除、草稿/发布可见性
- 契约证据：涉及 API 行为时引用 `contracts/api/openapi.yaml` 的对应路径或记录契约差异
