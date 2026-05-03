# phase-1-usable-content-loop 能力规范

## 目的

定义 Phase 1 MVP 可体验内容闭环的阶段目标、issue 下发规则、旧 issue/PR 处置规则和最终验收口径，使 `blog-FE`、`blog-BE`、`blog-Docs` 能围绕真实用户体验完成 MVP。

## ADDED Requirements

### Requirement: MVP 必须以可体验内容闭环为完成口径

Phase 1 MVP MUST 交付一个最小可体验博客闭环：游客能浏览真实 published 文章，注册/登录用户能管理自己的文章，数据真实持久化。

#### Scenario: 游客浏览真实文章

- **GIVEN** 后端存在一篇 `published` 文章
- **WHEN** 游客访问首页、文章列表页和文章详情页
- **THEN** 前端必须展示来自真实 API 的文章数据
- **AND** 不得在核心浏览路径展示 `mockPosts`

#### Scenario: 登录用户管理自己的文章

- **GIVEN** 用户已通过邮箱验证码注册并登录
- **WHEN** 用户进入“我的文章”或等价创作入口
- **THEN** 用户必须能创建、编辑、删除、发布自己的文章
- **AND** 文章写入 PostgreSQL 后刷新页面仍可读取

#### Scenario: 草稿与发布可见性

- **GIVEN** 登录用户创建草稿文章
- **WHEN** 游客访问公开文章列表或详情
- **THEN** 游客不得看到该草稿
- **AND** 用户发布文章后游客可以看到 published 版本

### Requirement: 阶段 issue 必须基于真实进度下发

MVP 阶段 issue MUST 基于 FE/BE 当前真实实现和本地运行验证结果下发，不得假设功能从零开始，也不得忽略已经复现的断点。

#### Scenario: FE Auth 已有真实 API adapter

- **GIVEN** FE 已有 `/auth/login`、`/auth/sendcode`、`/auth/register`、`/auth/refresh` adapter
- **WHEN** 下发 FE Auth issue
- **THEN** issue 必须聚焦注册/登录契约、token/session、`/auth/me` 和 401 行为收敛
- **AND** 不得描述为“从零接入 Auth API”

#### Scenario: BE Articles 已可通过 API 写入和读取

- **GIVEN** BE 已能通过 API 创建 published 文章并让游客读取
- **WHEN** 下发 BE Articles issue
- **THEN** issue 必须聚焦权限语义、持久化测试和契约保护
- **AND** 不得描述为“从零实现文章 CRUD”

### Requirement: Issue 粒度必须是能力包

MVP 阶段 issue MUST 覆盖一个可独立验收的能力包，不得拆到单按钮/单接口，也不得大到整仓库或整条产品线。

#### Scenario: 下发 FE 我的文章任务

- **GIVEN** 普通用户当前无法通过前端进入创作入口
- **WHEN** 下发 FE 我的文章 issue
- **THEN** issue 可以覆盖普通用户创作入口、我的文章列表、新建/编辑/删除/发布入口
- **AND** 不应拆成“新增一个按钮”“新增一个路由”等过细任务

#### Scenario: 下发 Contract gap 任务

- **GIVEN** Auth 和 Articles 都存在契约差异
- **WHEN** 下发契约任务
- **THEN** Auth gap 与 Articles gap 可以分别成 issue
- **AND** 不得在本 change 中执行完整 OpenAPI 大修

### Requirement: AGENTS 不得承载阶段目标

FE/BE/Docs 的 `AGENTS.md` MUST 只承载长期 Agent 工作规则，不得写 Phase 1 目标、MVP 功能清单、当前 issue 清单或阶段状态。

#### Scenario: 对齐 FE/BE AGENTS

- **GIVEN** 需要优化 FE 或 BE 仓库 `AGENTS.md`
- **WHEN** 下发 AGENTS issue
- **THEN** issue 必须只要求写长期规则：读取控制面真源、验证命令、PR/Harness/回写规则、契约准入
- **AND** 不得把 Phase 1 Auth/Articles 目标写入 AGENTS.md

### Requirement: 旧 issue 必须按 MVP 范围处置

已有 FE/BE open issue MUST 被分类处理：Phase 1 外能力关闭或 deferred，MVP 相关但口径过旧的 issue 改写或关闭并引用新 issue。

#### Scenario: 旧 issue 属于评论/上传/搜索

- **GIVEN** 旧 issue 目标是评论、图片上传、全文搜索或复杂审核后台
- **WHEN** Phase 1 可体验闭环 issue map 发布
- **THEN** 该 issue 必须关闭或标记 `status:deferred`
- **AND** 评论说明该能力已被 `docs/control-plane/phase-roadmap.md` 排到后续阶段

#### Scenario: 旧 issue 与 MVP 部分重叠

- **GIVEN** 旧 issue 与 MVP 相关但范围过大或口径过旧
- **WHEN** 新 issue map 已发布
- **THEN** 必须编辑旧 issue 或关闭旧 issue 并引用新 issue
- **AND** 不得让两个 issue 同时追踪同一交付目标

### Requirement: Open PR 必须按阶段目标审核

FE/BE open PR MUST 在合并前审核其是否对齐 MVP 可体验闭环。

#### Scenario: PR 支持本地运行基线

- **GIVEN** PR 改善本地 5173/8080/CORS/Docker Compose/env 配置
- **WHEN** 它不引入 Phase 1 外功能
- **THEN** 它可以纳入 MVP 本地运行基线
- **AND** 必须补 issue 关联、验证证据和必要回写

#### Scenario: PR 引入搜索或排序增强

- **GIVEN** PR 目标主要是 keyword/sort_by/search 能力
- **WHEN** 该能力不阻塞 MVP 可体验闭环
- **THEN** PR 必须延期、关闭或收敛为不扩大范围的契约兼容修复
- **AND** 不得把全文搜索作为 Phase 1 完成条件

### Requirement: 最终验收必须汇总真实证据

Phase 1 最终验收 issue MUST 只做证据汇总与 runtime 回写，不得顺手修 FE/BE 代码。

#### Scenario: 汇总 MVP 验收

- **GIVEN** FE/BE MVP issue 已完成
- **WHEN** 执行最终验收
- **THEN** 必须收集注册/登录、我的文章创建/编辑/删除/发布、游客真实浏览、草稿不可见、刷新持久化的证据
- **AND** 必须回写 `phase-current.md`、`issue-dispatch.md`、`risk-register.md`

## 边界

- 范围内：MVP 可体验闭环目标、issue map、旧 issue/PR 处置、CI/smoke/AGENTS 下发目标、runtime 回写。
- 范围外：评论、图片上传、富文本、全文搜索、复杂后台统计、完整 CI/CD、全量 E2E、后端目录结构大重构。

## 参考

- Phase 1 范围：`docs/control-plane/mvp-scope.md`
- Phase Roadmap：`docs/control-plane/phase-roadmap.md`
- 当前阶段：`docs/runtime/phase-current.md`
- Issue 派发：`docs/runtime/issue-dispatch.md`
- 风险台账：`docs/runtime/risk-register.md`
- Harness：`docs/harness/phase-1-mvp-harness.md`
- Issue 模板：`templates/issue-template.md`
