# 交付 Phase 1 MVP 可体验内容闭环

## 为什么做

当前 Phase 1 已经明确为“邮箱验证码注册/登录 + JWT 鉴权 + 文章读写持久化 + 游客浏览”的 MVP 最小内容闭环，但真实运行验证显示，项目还没有达到“用户可体验”的完成状态：

- 后端 Auth 与 Articles 主体能力已经具备，真实 API 可登录并创建 published 文章，PostgreSQL 持久化后游客可读取。
- 前端首页已经能展示后端真实文章，但 `/articles` 页面仍展示 `mockPosts`，导致游客文章浏览体验不可信。
- 注册验证码接口后端成功返回 `message + data.retry_after_seconds`，但前端期望 `data.message`，页面报“发送验证码响应格式无效”。
- 注册成功后后端只返回 `user_id`，前端会写入 user 快照但没有 `accessToken`，形成半登录状态。
- 普通用户登录后访问 `/admin/create` 会被角色守卫挡到 `/unauthorized`，没有普通用户“我的文章/创作中心”入口。
- `/account` 调用 `/api/v1/users/profile`，后端无该接口；当前应对齐 `/auth/me` 或暂不作为 MVP 主路径。
- FE/BE 的最小 PR Gate、Playwright smoke、AGENTS.md 对齐和 CI 基线还没有统一纳入阶段目标。

因此，本变更用于一次性定义 MVP 阶段目标下发计划：明确做完后用户能体验到什么、距离目标差什么、哪些旧 issue/PR 应关闭/延期/改写/纳入，以及后续 FE/BE/Docs issue 如何下发和验收。

## 变更内容

本变更将：

- 定义 Phase 1 MVP “可体验内容闭环”的完成口径。
- 基于真实运行验证和 FE/BE 仓库现状，生成一次性 MVP 阶段 issue map。
- 明确旧 FE/BE issue 的处理策略：Phase 1 外能力关闭或标记 deferred，MVP 相关 issue 改写或关联新目标。
- 明确当前 open PR 的处理策略：
  - `blog-BE#36` 配置/CORS PR：纳入 MVP 本地运行基线，补 issue 关联和描述修正后可合并。
  - `blog-BE#37` 文章列表关键词/排序 PR：延期保留，不纳入当前 MVP 合并队列；后续作为搜索/列表增强契约任务再评审。
- 定义各 issue 必须包含的工作流入口、输入真源、授权范围、明确不做、Harness 和回写目标。
- 明确 AGENTS.md 不承载阶段目标；FE/BE AGENTS.md 只承载长期 Agent 工作规则。

## 影响范围

- 受影响仓库：`blog-Docs`
- 下游仓库：`blog-FE`、`blog-BE`
- 受影响文档层：
  - `docs/runtime/issue-dispatch.md`
  - `docs/runtime/risk-register.md`
  - `docs/runtime/phase-current.md`
  - `docs/harness/phase-1-mvp-harness.md`
  - 后续 FE/BE GitHub Issues 与 PR 描述

## 范围外

- 不把评论、回复、点赞纳入 Phase 1。
- 不把富文本编辑器、图片上传、MinIO/OSS、全文搜索纳入 Phase 1。
- 不把复杂个人资料页、复杂后台统计、完整 CI/CD、全量 E2E 纳入 Phase 1。
- 不在 AGENTS.md 写阶段目标、MVP 范围或当前 issue 清单。
- 不进行 Go 后端目录结构大重构；目录结构评估可后续单独立项。
- 不在本 change 中直接完成完整 OpenAPI 大修。

## 验证影响

后续每个 MVP 阶段 issue 必须提供对应 Harness：

- FE：`bun run test`、`bun run build`，涉及真实体验链路时提供 Playwright 或人工 smoke 证据。
- BE：`go test ./...`、`go vet ./...`、`go build ./...`，涉及 Docker/DB/Redis 时提供本地 smoke 证据。
- Docs：`openspec validate deliver-phase-1-usable-content-loop`，并回写 runtime。
- 最终验收：注册/登录、我的文章创建/编辑/删除/发布、游客真实浏览 published 文章、草稿不可见、刷新后数据仍存在。
