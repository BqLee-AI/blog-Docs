# MVP Scope

## 目标

交付一个前后端分离的个人博客系统：管理员可发布/管理文章，访客可浏览文章并评论。认证、文章 CRUD、分类标签为核心闭环。

## 必须完成

### FE

- 首页文章列表（分页、关键词搜索、排序）
- 文章详情页（正文渲染、浏览计数）
- 登录/注册流程（邮箱验证码 + 密码）
- 管理后台：文章创建/编辑/删除（仅 admin/superadmin）
- 管理后台：Dashboard 入口
- 个人中心：资料编辑、头像上传、密码修改
- 评论展示与交互（列表、发评论、回复、点赞）
- 暗色/亮色主题切换
- 响应式布局

### BE

- 认证：注册（邮箱验证码）、登录、Token 刷新、获取当前用户
- 文章：CRUD + 分页列表 + 浏览计数 + 草稿/已发布状态管理
- 分类：列表（支持层级）
- 标签：列表
- 评论：CRUD + 回复 + 点赞（待实现）
- 统一响应格式：`{ message, data, requestId, code }`
- JWT RSA256 认证 + bcrypt 密码哈希
- CORS 跨域支持

### Control Plane

- mvp-scope 冻结
- OpenAPI 契约覆盖所有已实现 API
- L0 Spec 按能力域建立（api-contract / domain-model / harness-baseline）
- blog-BE/AGENTS.md 符合 agents-md-standard

## 本次不做

- 全文搜索（Elasticsearch / Meilisearch）
- 富文本编辑器集成（当前纯文本/Markdown）
- 图片/文件上传存储（MinIO / OSS）
- Redis 验证码存储（当前内存实现）
- 多租户 / 多作者系统
- 移动端适配专项
- 国际化（i18n）
- E2E 自动化测试（Playwright）
- 监控告警体系
- 自动化 CI/CD PR Gate（延后至多人协作阶段）

## 延后项

- 分类/标签管理接口（增删改）
- 文章草稿自动保存
- 操作日志 / 审计日志
- SEO 优化
- 性能优化（缓存、CDN）
- 日志方案（结构化日志）
- 配置中心

## 完成判定

- 管理员可通过后台完整发布一篇文章（创建 → 编辑 → 发布）
- 访客可浏览文章列表、阅读文章详情、发表评论
- 前后端 API 对接完成，无 mock 数据残留
- OpenAPI 契约与实际 API 端点一致
