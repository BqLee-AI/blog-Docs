# Tasks

## 1. 确认 Phase 1 范围

- [ ] 1.1 确认 Phase 1 命名为 `MVP 最小内容闭环`
- [ ] 1.2 确认 Phase 1 只包含账号认证、文章读写、文章持久化、游客浏览
- [ ] 1.3 确认评论、富文本、图片上传、全文搜索、reCAPTCHA 等能力延后
- [ ] 1.4 确认 Phase 1 完成判定面向真实 API 和真实数据库，不以 mock 作为核心验收证据

## 2. 确认控制面文件职责

- [ ] 2.1 确认 `docs/control-plane/phase-roadmap.md` 负责阶段路线图
- [ ] 2.2 确认 `docs/control-plane/mvp-scope.md` 负责 Phase 1 范围冻结
- [ ] 2.3 确认 `docs/runtime/phase-current.md` 负责当前阶段摘要和真源引用
- [ ] 2.4 确认 `docs/runtime/issue-dispatch.md` 负责 Docs/FE/BE Issue 下发和状态回写
- [ ] 2.5 确认 `docs/runtime/risk-register.md` 负责当前阶段风险
- [ ] 2.6 确认 `docs/runtime/openapi-gap-register.md` 负责 auth/articles 契约差异入口
- [ ] 2.7 确认 `docs/harness/phase-1-mvp-harness.md` 负责阶段性测试、CI/check、smoke 和证据要求

## 3. 确认后续 Issue 包

- [ ] 3.1 确认 Docs Issue 包：范围冻结、runtime 面板、harness、OpenAPI 差异识别、入口引用收口
- [ ] 3.2 确认 FE Issue 包：认证验收、文章去 mock、AGENTS 接入控制面
- [ ] 3.3 确认 BE Issue 包：认证验收、文章验收、最小 harness/check
- [ ] 3.4 确认每个 Issue 必须包含真源引用、验收标准、验证命令和回写目标

## 4. 确认 Phase 1 证据模型

- [ ] 4.1 确认 FE 最小检查为 `bun run build` 和 `bun run test`
- [ ] 4.2 确认 BE 最小检查为 `go test ./...`、`go vet ./...`、`go build ./...`
- [ ] 4.3 确认认证 smoke 至少覆盖验证码、注册、登录、当前用户、受保护入口
- [ ] 4.4 确认文章 smoke 至少覆盖创建、列表、详情、编辑、删除、草稿/发布可见性
- [ ] 4.5 确认 API 行为差异必须记录为 OpenAPI gap，并拆为后续任务处理

## 5. 评审

- [ ] 5.1 与 PM 评审本方案
- [ ] 5.2 根据评审意见修订 proposal / design / spec / tasks
- [ ] 5.3 方案确认后，再拆分后续 Docs / FE / BE Issue
