# 设计说明

## 设计目标

本 change 的目标是把 Phase 1 从“技术能力大致存在”推进到“用户能体验 MVP 内容闭环”：

- 游客能浏览真实 published 文章。
- 注册/登录用户能获得可靠会话。
- 登录用户能进入自己的内容管理入口。
- 登录用户能创建、编辑、删除、发布自己的文章。
- 数据写入 PostgreSQL，刷新后仍存在。
- FE/BE PR 具备最小 CI 与 smoke 证据。
- Docs runtime 能持续跟踪 issue、风险和验收结论。

## 真实运行证据

本地验证环境：

- FE：`http://localhost:5173`
- BE：`http://localhost:8080`
- Docker Compose：Postgres、Redis、Go app

验证发现：

- `GET /api/v1/articles` 初始返回空数组。
- 首页 `/` 使用真实 article API，能显示后端创建的 published 文章。
- `/articles` 仍使用 `mockPosts`，显示 3 篇 mock 文章。
- `POST /auth/sendcode` 后端返回成功，但 FE 解包失败并显示“发送验证码响应格式无效”。
- `POST /auth/register` 返回 `user_id`，FE 形成有 user 无 token 的半登录状态。
- `POST /auth/login` 可获得 access token。
- 普通用户访问 `/admin/create` 被角色守卫拦截到 `/unauthorized`。
- `/account` 调用不存在的 `/api/v1/users/profile` 并显示账号信息加载失败。
- 直接调用后端 API 可创建 published 文章，并可由游客读取。

## Issue 粒度原则

Issue 既不能拆到单按钮/单接口，也不能大到整条仓库级任务。每个 issue 应覆盖一个可独立验收的“能力包”：

- FE Auth 会话体验。
- FE 我的文章入口。
- FE 游客真实浏览。
- FE 我的文章写作流程。
- BE Auth 契约与测试闭环。
- BE Articles 权限与持久化测试闭环。
- BE 本地运行基线。
- FE/BE CI 与 Agent 工作规则。
- Docs 契约 gap 与验收回写。

## 旧 Issue 处理策略

旧 issue 不直接删除历史，而按 Phase 1 目标归类：

- Phase 1 外能力（评论、图片上传、全文搜索、复杂审核后台）应关闭或改为 `status:deferred`，并评论说明被 Phase Roadmap 延后。
- 与 MVP 可体验闭环相关但口径过旧的 issue，应关闭并引用新 issue，或编辑为新目标。
- 治理长期跟踪类 epic 可以保留，但不能作为 Phase 1 可交付任务。
- 发布壳/候选版本类旧 issue 若不再反映当前阶段，应关闭或改为 deferred。

## Open PR 处理策略

### blog-BE#36

配置/CORS PR 对齐 MVP 本地运行基线，建议保留：

- 合并前补 issue 关联。
- 修正 PR 描述中与最终 diff 不一致的测试文件说明。
- 验收保留 `go build ./...`、`go test ./src/config/...`、`docker compose config`。
- 回写 MVP 本地基线：FE 5173、BE 8080、DB 5433、Redis host port。

### blog-BE#37

文章列表关键词/排序 PR 已完成阶段审核，建议延期保留，不纳入当前 MVP 合并队列：

- PR 实现 `keyword`、`sort_by`、`total_pages`，主要服务搜索/排序增强。
- 当前 MVP 只要求游客真实浏览 published 文章，不要求关键词搜索或排序增强。
- Issue `blog-BE#11` 应标记为 Post-MVP / FE-013 dependency。
- PR 可保持 open 或转 draft，但需要移出 MVP 合并队列。
- 后续恢复时必须补 README/OpenAPI 契约、行为测试和 smoke 证据，并明确它不是全文检索。

## AGENTS.md 边界

FE/BE AGENTS.md 需要对齐长期工作规则，但不得写阶段目标：

- 可以写：仓库职责、读 blog-Docs 真源方式、验证命令、PR/Harness/回写规则、契约变更准入。
- 不写：Phase 1 目标、MVP 功能清单、当前 issue 清单、阶段风险状态。

## 下发批次

本 change 需要一次性定义完整 MVP issue map，但执行可分批：

- P0：修复当前跑起来就断的体验链路。
- P1：补齐测试、CI、Playwright smoke、AGENTS 长期规则。
- Verify：最终验收汇总与 runtime 回写。

## 回滚

- 如果某个 FE/BE issue 被证明粒度不合适，回滚方式是关闭该 issue 并用 OpenSpec comment/Docs runtime 记录替代 issue。
- 如果某项能力被重新判定为 Phase 1 外，必须回写 `risk-register.md` 或 issue comment，并标记 deferred。
