# 阶段 Issue 下发清单（issue-dispatch）

> 用途：记录当前阶段要下发与跟踪的 Issue。该文件属于运行态资产，可高频更新。
>
> 设计原则：本文件只保留关键摘要和 Issue 链接，不重复 Issue 详情。
> Agent 接任务时从本文件获取 Issue 链接，跳到对应仓库读取 Issue body 中的完整上下文。
> 不要把整个文档仓库喂给 Agent，只加载 Issue body 中指定的输入真源。

## 清单

| Priority | Repo | Issue | Why now | Status |
|----------|------|-------|---------|--------|
| P0 | blog-Docs | [BqLee-AI/blog-Docs#44](https://github.com/BqLee-AI/blog-Docs/issues/44) | 需先冻结 Phase Roadmap 与 MVP Scope，避免阶段边界反复变动 | done |
| P0 | blog-Docs | [BqLee-AI/blog-Docs#48](https://github.com/BqLee-AI/blog-Docs/issues/48) | 需把 Phase 1 当前运行态从模板落成可执行面板，作为并行协作入口 | done |
| P0 | blog-Docs | [BqLee-AI/blog-Docs#47](https://github.com/BqLee-AI/blog-Docs/issues/47) | 需建立 Phase 1 Harness 与验收标准，统一验证口径 | done |
| P1 | blog-Docs | [BqLee-AI/blog-Docs#46](https://github.com/BqLee-AI/blog-Docs/issues/46) | 需识别 auth/articles OpenAPI 差异，避免 FE/BE 与契约偏移积累 | done |
| P1 | blog-Docs | [BqLee-AI/blog-Docs#45](https://github.com/BqLee-AI/blog-Docs/issues/45) | 需对齐文档仓库入口与引用关系，降低 Agent 取错真源风险 | done |

字段说明：

- `Issue`：GitHub Issue 链接（Agent 从这里跳转读取完整上下文）
- `Why now`：为什么现在做（不超过 1 句）
- `Status`：ready / doing / verify / blocked / done

## Context Harness 原则

1. Agent 接任务时：读本文件 → 找到 Issue 链接 → 跳到对应仓库读 Issue body
2. Issue body 中已指定输入真源（Spec / 契约 / 实现约束），Agent 只加载这些
3. 不要预加载整个文档仓库，避免无关上下文导致跑偏
4. PM 在本文件看全局状态，不需要跳到每个仓库翻 Issue
