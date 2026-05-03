# 任务清单

## 1. OpenSpec

- [ ] 输入：Phase 1 scope、runtime、harness、真实运行验证结果、FE/BE issue/PR 盘点。
- [ ] 输出：`openspec/changes/deliver-phase-1-usable-content-loop/`。
- [ ] 验收：`openspec validate deliver-phase-1-usable-content-loop` 通过。
- [ ] 回写：归档前更新 `docs/runtime/issue-dispatch.md`、`risk-register.md`、`phase-current.md`。

## 2. 旧 Issue / PR 处置

- [ ] 盘点 `blog-FE` open issue：搜索、图片上传、评论、审核后台、发布壳、治理 epic。
- [ ] 盘点 `blog-BE` open issue：健康检查、评论、图片上传、全文检索、发布 smoke、文章列表优化、发布壳、治理 epic。
- [ ] 对 Phase 1 外能力执行关闭或 deferred 标记，并评论引用 phase roadmap。
- [ ] 对 MVP 相关旧 issue 执行改写或关闭并引用新 issue。
- [ ] 审核 `blog-BE#36`，补 issue 关联和 PR 描述修正要求。
- [ ] 审核 `blog-BE#37`，判断延期、关闭或收敛到 Articles 契约任务。

## 3. 一次性下发 MVP 阶段 Issue

- [ ] Docs issue：定义执行计划、契约 gap、runtime 回写、最终验收。
- [ ] FE issue：Auth 会话、我的文章入口、游客真实浏览、我的文章写作、Playwright smoke、CI、AGENTS 长期规则。
- [ ] BE issue：运行基线、Auth 测试契约、Articles 权限持久化测试、CI、AGENTS 长期规则。
- [ ] 所有 issue 使用 `templates/issue-template.md`，包含工作流入口、输入真源、授权范围、明确不做、Harness、回写目标。
- [ ] 所有 issue 打稳定标签：`level:*`、`freeze:*`、`status:*`，必要时加 `release:candidate`。

## 4. Runtime 回写

- [ ] `docs/runtime/issue-dispatch.md` 记录新 issue 链接、优先级和状态。
- [ ] `docs/runtime/risk-register.md` 更新 mock、Auth 契约、普通用户创作入口、CI/smoke、旧 PR 风险。
- [ ] `docs/runtime/phase-current.md` 增补“可体验内容闭环”当前执行口径。

## 5. 验证

- [ ] `openspec validate deliver-phase-1-usable-content-loop`。
- [ ] 检查 AGENTS.md 未写阶段目标。
- [ ] 检查 issue map 未包含评论、图片上传、全文搜索、富文本等 Phase 1 外能力。
- [ ] 检查 Playwright 临时安装已被记录为后续 FE smoke/CI 任务。
