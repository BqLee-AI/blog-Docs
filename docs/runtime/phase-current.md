# 当前阶段（phase-current）

> 用途：记录当前阶段推进定义。该文件属于运行态资产，可高频更新。

## 阶段标识

- 阶段名称：Phase 1 - MVP 最小内容闭环
- 阶段编号（可选）：P1
- 状态：active

## 本阶段目标（1-3 条）

1. 交付“账号认证 + 文章读写”的最小可用闭环（注册/登录 + 文章创建/编辑/发布 + 游客浏览已发布内容）。
2. 统一控制面运行态：阶段状态、任务下发、风险处置可在 `docs/runtime/` 持续回写。
3. 对齐契约与实现：先识别并收敛 auth/articles OpenAPI 差异，避免验证口径漂移。

## 本阶段非目标（明确不做）

- 评论/回复/点赞
- 富文本编辑器、图片上传、全文搜索、reCAPTCHA
- 复杂个人中心、复杂后台统计、完整 CI/CD 与 E2E 自动化

## 本阶段 DoD（完成判定）

- [ ] 认证链路可验收：验证码注册、登录、当前用户与受保护入口行为可复现。
- [ ] 文章链路可验收：创建、列表、详情、编辑、删除、草稿/发布可见性可复现。
- [ ] FE 核心验收不依赖 mock，且 auth/articles 契约差异已形成清单并进入跟踪。

## 本阶段 Harness 验收重点

- 工程秩序（Build/Test/Typecheck）：按 Phase 1 harness 要求执行 FE/BE 最小检查。
- 契约验证（API/数据语义）：以 `contracts/api/openapi.yaml` 为唯一 API 真源，差异先记录后收敛。
- 关键链路 Smoke：认证与文章两条主链路必须提供可复现证据。

## 规则真源引用

- Scope/边界：`docs/control-plane/mvp-scope.md`
- 阶段路线：`docs/control-plane/phase-roadmap.md`
- 变更依据：`openspec/changes/phase-1-mvp-control-plane-plan/{proposal,design}.md`
- 流程/SOP：`docs/workflow/`
- 验证基线：`docs/harness/`
- 标准规范：`docs/standards/`
