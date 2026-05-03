# 风险台账（risk-register）

> 用途：记录当前阶段风险与处理进度。该文件属于运行态资产，可高频更新。

## 风险分级

- P0：阻塞关键链路，需立即处理
- P1：高优先风险，需本阶段关闭或降级
- P2：一般风险，持续观察

## 台账模板

| Level | Risk | Impact | Owner | Mitigation | Trigger | Status |
|-------|------|--------|-------|------------|---------|--------|
| P0 | Phase 1 范围漂移（把评论/上传/搜索等 Phase 2+ 能力拉回当前迭代） | 直接稀释认证与文章主链路交付，导致 Phase 1 无法按时闭环 | Docs Owner / PM | 以 OpenSpec Phase 1 口径与 `mvp-scope` 作为下发真源；超界需求转入后续 Issue 并标记 deferred | 任一 P1 Issue/PR 新增评论、上传、搜索、reCAPTCHA 等能力实现或验收项 | mitigating |
| P0 | FE Auth 会话断点阻塞创作入口 | 用户注册后可能半登录，刷新或进入创作页失败，文章闭环无法体验 | FE Owner / BE Owner | FE #58 修 sendcode/register/session；BE #41 补 Auth 契约与测试；Docs #70 登记 gap | 注册/登录/刷新任一步无法稳定获得当前用户和 token | open |
| P0 | 普通用户无创作入口 | 用户即使登录也无法进入我的文章或写作流程，MVP 用户闭环不可体验 | FE Owner | FE #60 建立普通用户“我的文章/创作”入口；FE #64 接通写作闭环 | 普通用户访问写作入口仍被 `/unauthorized` 拦截 | open |
| P0 | FE 核心链路存在 mock 残留 | 验收通过但线上不可用，回归与排障成本升高 | FE Owner | FE #59 移除 `/articles` 核心路径 mock；FE #63 smoke 只能用真实 API 作为证据 | 验收证据无法给出真实请求/响应，或发现页面关键路径仍使用本地 mock 数据 | open |
| P1 | Auth/Articles OpenAPI 差异持续累积 | FE/BE 集成反复返工，测试用例与契约基线失配 | Docs Owner / API Owner | Docs #70 先登记 MVP 必需 gap；FE #58/#64 与 BE #41/#42 分别收敛实现；完整 OpenAPI 大修后续 change | 新增/变更接口在 FE 调用、BE 返回、OpenAPI 三者中出现任一不一致且未登记 | mitigating |
| P1 | 本地 5173/8080 联调基线不稳定 | FE/BE smoke 无法复现，任务完成证据失真 | BE Owner | BE #43 收敛 Docker Compose、CORS、env；BE PR #36 纳入本地运行基线 | FE 5173 无法访问 BE 8080 Auth/Articles 或 compose 启动失败 | mitigating |
| P1 | BE PR #37 搜索/排序增强扩大 MVP 范围 | 提前合入会扩大契约、测试和验收面，拖慢 MVP 主闭环 | BE Owner / Docs Owner | PR #37 标记 deferred，BE #11 标记 Post-MVP dependency；搜索后续走新 OpenSpec | PR #37 被合并或重新标 ready 且未完成搜索 OpenSpec | accepted |

字段说明：

- `Mitigation`：缓解/回滚动作（可执行）
- `Trigger`：触发升级条件（例如超时、契约冲突）
- `Status`：open / mitigating / resolved / accepted

## 规则真源引用

- 升级规则：`docs/workflow/change-intake-and-escalation.md`
- 任务同步：`docs/workflow/task-release-and-sync-sop.md`
- Harness 门禁：`docs/harness/harness-gate-baseline.md`
