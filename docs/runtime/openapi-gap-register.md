# OpenAPI Gap Register（Phase 1 / Auth + Articles）

> 范围约束：仅记录 Phase 1 MVP（账号认证 + 文章读写）必需差异；评论、上传、搜索等能力不作为当前 MVP 完成条件。
>
> 当前策略：本 change 先登记 OpenAPI 差异和责任 issue；完整 OpenAPI 大修拆到后续 OpenSpec change。

## MVP 阻塞 Gap

| Area | Endpoint / Path | 当前差异 | 影响 | Owner Issue | Status |
|---|---|---|---|---|---|
| Auth | `POST /api/v1/auth/sendcode` | BE 成功响应为 envelope 顶层 `message` + `data.retry_after_seconds`；FE 当前期望 `data.message`。 | FE 会把成功响应误判为格式错误，注册链路首步不稳定。 | FE [#58](https://github.com/BqLee-AI/blog-FE/issues/58), BE [#41](https://github.com/BqLee-AI/blog-BE/issues/41) | open |
| Auth | `POST /api/v1/auth/register` | BE 当前返回 `user_id`，FE 形成有 `user` 但无 `accessToken` 的半登录；OpenAPI 对注册后会话语义未明确。 | 用户注册后无法稳定进入受保护创作路径。 | FE [#58](https://github.com/BqLee-AI/blog-FE/issues/58), BE [#41](https://github.com/BqLee-AI/blog-BE/issues/41) | open |
| Auth | `GET /api/v1/auth/me` / FE `/account` | FE `/account` 调 `/api/v1/users/profile`，BE 当前无该接口；MVP 当前用户恢复应对齐 `/auth/me` 或移出主路径。 | 登录刷新和账户页会话恢复不稳定。 | FE [#58](https://github.com/BqLee-AI/blog-FE/issues/58), BE [#41](https://github.com/BqLee-AI/blog-BE/issues/41) | open |
| Articles | `GET /api/v1/articles` | 首页已使用真实 API；`/articles` 仍依赖 FE `mockPosts`；OpenAPI/实现需保护 published 列表最小字段。 | 游客文章列表验收不能作为真实证据。 | FE [#59](https://github.com/BqLee-AI/blog-FE/issues/59), BE [#42](https://github.com/BqLee-AI/blog-BE/issues/42) | open |
| Articles | `POST/PUT/DELETE /api/v1/articles` | BE API 已可写读，但 FE 写作路径在 admin 路由下且 payload/状态需要与真实 API 对齐。 | 普通用户无法体验创建、编辑、删除、发布自己的文章。 | FE [#60](https://github.com/BqLee-AI/blog-FE/issues/60), FE [#64](https://github.com/BqLee-AI/blog-FE/issues/64), BE [#42](https://github.com/BqLee-AI/blog-BE/issues/42) | open |
| Articles | `GET /api/v1/articles` + `GET /api/v1/articles/{id}` | 草稿/发布可见性需要以游客/作者视角形成测试和契约说明；无权访问/未找到语义需要可复核。 | 草稿可能被误认为公开，或 FE/BE 对 404/403 语义理解不一致。 | FE [#64](https://github.com/BqLee-AI/blog-FE/issues/64), BE [#42](https://github.com/BqLee-AI/blog-BE/issues/42) | open |

## Deferred Gap

| Area | Endpoint / Path | 当前结论 | 后续入口 |
|---|---|---|---|
| Search | `GET /api/v1/articles?keyword=&sort_by=` | keyword/sort_by/total_pages 属于列表增强，不阻塞当前 MVP；BE [#11](https://github.com/BqLee-AI/blog-BE/issues/11) 与 PR [#37](https://github.com/BqLee-AI/blog-BE/pull/37) 已标记 Post-MVP / deferred。 | 搜索/列表增强 OpenSpec |
| Upload | `POST /api/v1/upload` | 图片上传不属于当前 MVP，旧 FE/BE 单体 issue 已关闭，保留 deferred 包装入口。 | FE [#38](https://github.com/BqLee-AI/blog-FE/issues/38), BE [#25](https://github.com/BqLee-AI/blog-BE/issues/25) |
| Comments | comments API | 评论不属于当前 MVP，旧 FE/BE 单体 issue 已关闭，保留 deferred 包装入口。 | FE [#38](https://github.com/BqLee-AI/blog-FE/issues/38), BE [#25](https://github.com/BqLee-AI/blog-BE/issues/25) |

## 回写规则

- FE/BE PR 若发现新的 Auth/Articles 契约差异，必须评论回写到 [Docs #70](https://github.com/BqLee-AI/blog-Docs/issues/70) 或更新本文件。
- 若差异改变 scope、contract、harness 或 gate，必须先走新的 OpenSpec change。
- 完整 `contracts/api/openapi.yaml` 大修不在本 issue 中完成。
