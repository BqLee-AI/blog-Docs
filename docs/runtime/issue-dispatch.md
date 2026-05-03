# 阶段 Issue 下发清单（issue-dispatch）

> 用途：记录当前阶段要下发与跟踪的 Issue。该文件属于运行态资产，可高频更新。
>
> 设计原则：本文件只保留关键摘要和 Issue 链接，不重复 Issue 详情。
> Agent 接任务时从本文件获取 Issue 链接，跳到对应仓库读取 Issue body 中的完整上下文。
> 不要把整个文档仓库喂给 Agent，只加载 Issue body 中指定的输入真源。

## 当前阶段入口

- 阶段：Phase 1 MVP 可体验内容闭环
- OpenSpec：`openspec/changes/deliver-phase-1-usable-content-loop/`
- Harness：`docs/harness/phase-1-mvp-harness.md`
- Scope：`docs/control-plane/mvp-scope.md`
- Roadmap：`docs/control-plane/phase-roadmap.md`

## 当前下发清单

| Priority | Repo | Issue | Why now | Status |
|----------|------|-------|---------|--------|
| P0 | blog-Docs | [BqLee-AI/blog-Docs#69](https://github.com/BqLee-AI/blog-Docs/issues/69) | 需把本轮 MVP issue、旧 issue/PR 处置和阶段风险回写到 runtime | doing |
| P0 | blog-Docs | [BqLee-AI/blog-Docs#70](https://github.com/BqLee-AI/blog-Docs/issues/70) | Auth/Articles 契约差异已阻塞 FE/BE MVP 联调，需要先登记 gap | doing |
| P2 | blog-Docs | [BqLee-AI/blog-Docs#71](https://github.com/BqLee-AI/blog-Docs/issues/71) | 最终验收必须等 FE/BE MVP issue 完成后再汇总证据 | blocked |
| P0 | blog-FE | [BqLee-AI/blog-FE#58](https://github.com/BqLee-AI/blog-FE/issues/58) | Auth 会话不稳定会阻塞“我的文章”和写作闭环 | ready |
| P0 | blog-FE | [BqLee-AI/blog-FE#59](https://github.com/BqLee-AI/blog-FE/issues/59) | `/articles` 仍展示 mock，游客浏览不能作为 MVP 证据 | ready |
| P0 | blog-FE | [BqLee-AI/blog-FE#60](https://github.com/BqLee-AI/blog-FE/issues/60) | 普通用户当前没有可达创作入口，会被 admin 路由挡住 | ready |
| P0 | blog-FE | [BqLee-AI/blog-FE#64](https://github.com/BqLee-AI/blog-FE/issues/64) | MVP 核心体验要求用户能创建、编辑、删除、发布自己的文章 | ready |
| P1 | blog-FE | [BqLee-AI/blog-FE#63](https://github.com/BqLee-AI/blog-FE/issues/63) | 浏览器级 smoke 需要从 `/tmp` 临时脚本沉淀到仓库 | ready |
| P1 | blog-FE | [BqLee-AI/blog-FE#62](https://github.com/BqLee-AI/blog-FE/issues/62) | FE 多 PR 并行前需要最小 build/test gate | ready |
| P1 | blog-FE | [BqLee-AI/blog-FE#61](https://github.com/BqLee-AI/blog-FE/issues/61) | FE AGENTS.md 需固化长期工作流，但不得写阶段目标 | ready |
| P0 | blog-BE | [BqLee-AI/blog-BE#43](https://github.com/BqLee-AI/blog-BE/issues/43) | 本地 5173/8080 联调基线是 FE/BE MVP 验收前置 | ready |
| P0 | blog-BE | [BqLee-AI/blog-BE#41](https://github.com/BqLee-AI/blog-BE/issues/41) | Auth 已有实现但需契约与测试证据保护 FE 会话 | ready |
| P0 | blog-BE | [BqLee-AI/blog-BE#42](https://github.com/BqLee-AI/blog-BE/issues/42) | Articles 已可写读但需权限、草稿可见性、持久化测试 | ready |
| P1 | blog-BE | [BqLee-AI/blog-BE#44](https://github.com/BqLee-AI/blog-BE/issues/44) | BE 多 PR 并行前需要最小 go test/vet/build gate | ready |
| P1 | blog-BE | [BqLee-AI/blog-BE#45](https://github.com/BqLee-AI/blog-BE/issues/45) | BE AGENTS.md 需固化长期工作流，但不得写阶段目标 | ready |

字段说明：

- `Issue`：GitHub Issue 链接（Agent 从这里跳转读取完整上下文）
- `Why now`：为什么现在做（不超过 1 句）
- `Status`：ready / doing / verify / blocked / done

## 旧 Issue / PR 处置

### 已关闭

- FE：[#37](https://github.com/BqLee-AI/blog-FE/issues/37)、[#47](https://github.com/BqLee-AI/blog-FE/issues/47)、[#48](https://github.com/BqLee-AI/blog-FE/issues/48)、[#49](https://github.com/BqLee-AI/blog-FE/issues/49)、[#51](https://github.com/BqLee-AI/blog-FE/issues/51)
- BE：[#16](https://github.com/BqLee-AI/blog-BE/issues/16)、[#24](https://github.com/BqLee-AI/blog-BE/issues/24)、[#31](https://github.com/BqLee-AI/blog-BE/issues/31)、[#32](https://github.com/BqLee-AI/blog-BE/issues/32)、[#33](https://github.com/BqLee-AI/blog-BE/issues/33)、[#34](https://github.com/BqLee-AI/blog-BE/issues/34)

### 保留 Deferred

- FE：[#38](https://github.com/BqLee-AI/blog-FE/issues/38)、[#39](https://github.com/BqLee-AI/blog-FE/issues/39)
- BE：[#25](https://github.com/BqLee-AI/blog-BE/issues/25)、[#26](https://github.com/BqLee-AI/blog-BE/issues/26)

### 保留但移出当前 MVP

- BE：[#11](https://github.com/BqLee-AI/blog-BE/issues/11) 保留为 Post-MVP / FE-013 dependency。
- BE PR：[#37](https://github.com/BqLee-AI/blog-BE/pull/37) 保留但延期，不进入当前 MVP 合并队列。

### 纳入当前 MVP

- BE PR：[#36](https://github.com/BqLee-AI/blog-BE/pull/36) 纳入 [BE-P1-001 #43](https://github.com/BqLee-AI/blog-BE/issues/43) 的本地联调基线。

## Context Harness 原则

1. Agent 接任务时：读本文件 → 找到 Issue 链接 → 跳到对应仓库读 Issue body
2. Issue body 中已指定输入真源（Spec / 契约 / 实现约束），Agent 只加载这些
3. 不要预加载整个文档仓库，避免无关上下文导致跑偏
4. PM 在本文件看全局状态，不需要跳到每个仓库翻 Issue
