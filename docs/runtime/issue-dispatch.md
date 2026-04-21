# 阶段 Issue 下发清单（issue-dispatch）

> 用途：记录当前阶段要下发与跟踪的 Issue。该文件属于运行态资产，可高频更新。
>
> 设计原则：本文件只保留关键摘要和 Issue 链接，不重复 Issue 详情。
> Agent 接任务时从本文件获取 Issue 链接，跳到对应仓库读取 Issue body 中的完整上下文。
> 不要把整个文档仓库喂给 Agent，只加载 Issue body 中指定的输入真源。

## 清单

| Priority | Repo | Issue | Why now | Status |
|----------|------|-------|---------|--------|
| P0 | blog-BE | [BqLee-AI/blog-BE#39](https://github.com/BqLee-AI/blog-BE/issues/39) | 违反 fe-be-adoption-gate 第 3 条门禁，AGENTS.md 缺失 | ready |
| P0 | blog-FE | # | | blocked |

字段说明：

- `Issue`：GitHub Issue 链接（Agent 从这里跳转读取完整上下文）
- `Why now`：为什么现在做（不超过 1 句）
- `Status`：ready / doing / verify / blocked / done

## Context Harness 原则

1. Agent 接任务时：读本文件 → 找到 Issue 链接 → 跳到对应仓库读 Issue body
2. Issue body 中已指定输入真源（Spec / 契约 / 实现约束），Agent 只加载这些
3. 不要预加载整个文档仓库，避免无关上下文导致跑偏
4. PM 在本文件看全局状态，不需要跳到每个仓库翻 Issue

## 种子 Issue（主线上但当前不做的关联工作）

来源：Phase 0 控制面重构过程中发现的关联事项

| 来源 | 种子 | 为什么在主线上 | 优先级 |
|------|------|---------------|--------|
| BE AGENTS.md 任务 | FE AGENTS.md 补充控制面引用段 | 同一条门禁要求 | P1 |
| 控制面对齐检查 | OpenSpec change 应补充 design.md（当前只有 proposal/specs/tasks） | 对齐《AI 原生工程》05 篇，change 四面合一 | P1 |
| 控制面对齐检查 | blog-Docs AGENTS.md 补充默认工作流顺序（讨论→OpenSpec→归档→下发→执行） | Agent 不知道先做什么后做什么 | P1 |
| mvp-scope 冻结 | openapi.yaml 评论端点未定义（评论在 MVP 范围内） | 下一个功能开发的前置 | P2 |
| mvp-scope 待定项 | 验证码存储方式决策（内存 vs Redis） | 影响部署架构 | P2 |

## 待补差距（对标《AI 原生工程》05 篇）

- [ ] OpenSpec change 增加 design.md（变更怎么做：主资产、路径、边界、失败链路）
- [ ] blog-Docs AGENTS.md 写入默认工作流顺序
- [ ] 建立种子 Issue 捕获习惯：每次 OpenSpec change 归档时，回顾是否有关联工作需要记录
