# 当前阶段（phase-current）

> 用途：记录当前阶段推进定义。该文件属于运行态资产，可高频更新。

## 阶段标识

- 阶段名称：Phase 1 - MVP 最小内容闭环
- 阶段编号（可选）：P1
- 状态：active

## 本阶段目标（1-3 条）

1. 交付“账号认证 + 我的文章 + 真实游客浏览”的最小可体验闭环（邮箱验证码注册/登录 + 普通用户创建/编辑/删除/发布自己的文章 + 游客浏览真实 published 文章）。
2. 统一控制面运行态：阶段状态、任务下发、风险处置可在 `docs/runtime/` 持续回写。
3. 对齐契约与实现：先识别并收敛 Auth/Articles MVP 必需差异，完整 OpenAPI 大修拆到后续 change。

## 本阶段非目标（明确不做）

- 评论/回复/点赞
- 富文本编辑器、图片上传、全文搜索、reCAPTCHA
- 复杂个人中心、复杂后台统计、完整 CI/CD 与 E2E 自动化

## 本阶段 DoD（完成判定）

- [ ] 认证链路可验收：验证码注册、登录、当前用户与受保护入口行为可复现。
- [ ] 文章链路可验收：普通用户可创建、编辑、删除、发布自己的文章，刷新后数据仍存在。
- [ ] 游客浏览可验收：首页、文章列表页、详情页展示真实 API 的 published 文章，草稿游客不可见。
- [ ] FE 核心验收不依赖 mock，且 Auth/Articles 契约差异已形成清单并进入跟踪。
- [ ] FE/BE 最小 PR Gate 和 MVP smoke 证据可复核。

## 本阶段 Harness 验收重点

- 工程秩序（Build/Test/Typecheck）：按 Phase 1 harness 要求执行 FE/BE 最小检查。
- 契约验证（API/数据语义）：以 `contracts/api/openapi.yaml` 为目标 API 真源；当前 MVP 必需差异先登记 gap，再按 issue 收敛。
- 关键链路 Smoke：认证与文章两条主链路必须提供可复现证据。

## 规则真源引用

- Scope/边界：`docs/control-plane/mvp-scope.md`
- 阶段路线：`docs/control-plane/phase-roadmap.md`
- 变更依据：`openspec/changes/deliver-phase-1-usable-content-loop/{proposal,design,tasks}.md`
- 流程/SOP：`docs/workflow/`
- 验证基线：`docs/harness/`
- 标准规范：`docs/standards/`
