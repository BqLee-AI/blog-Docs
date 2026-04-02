# blog-Docs

`blog-Docs` 是 BqLee AI 项目的**控制面仓库**：

- 管规范（Rules / AGENTS / SOP）
- 管边界（模块职责、改动范围、验收口径）
- 管闭环（Spec -> Issue -> PR -> Test -> Release -> 回写）

它不是知识堆放仓库，而是指导 `blog-FE` 和 `blog-BE` 的工程执行标准。

## 仓库目标

- 让前后端 Agent 在同一套规则下稳定协作
- 让任务从“能写”升级到“可验证、可追踪、可回滚”
- 让每次踩坑沉淀为可复用规范，而不是聊天记录

## 目录结构

```text
.
├── AGENTS.md
├── docs
│   ├── 01-control-plane
│   │   └── ai-native-governance.md
│   ├── 02-workflow
│   │   ├── spec-issue-test-flow.md
│   │   └── progress-sync-protocol.md
│   │   ├── pm-product-governance-playbook.md
│   │   ├── issue-dispatch-mechanism.md
│   │   └── iteration-optimization-loop.md
│   │   ├── stage-review-policy.md
│   │   ├── pr-review-sop.md
│   │   └── task-release-and-sync-sop.md
│   ├── 03-harness
│   │   └── harness-closed-loop.md
│   │   └── agent-orchestration-matrix.md
│   └── 04-standards
│       └── agents-md-standard.md
└── templates
    ├── spec-template.md
    ├── issue-template.md
    └── harness-checklist.md
```

## 采用的开发范式（AI Native Engineering）

基于你指定的方法论，落地为三层控制面：

1. `Prompt` 层：说清任务目标与边界
2. `Context` 层：给对材料，控制权重，减少污染
3. `Harness` 层：让执行可观察、可验证、可收敛

对应工程闭环：

`Spec -> Issue -> Branch -> PR -> Harness Validate -> Merge -> Doc Backwrite`

## 推荐工具组合

- Spec 管理：`OpenSpec`（或仓库内 markdown spec + 模板）
- 任务追踪：GitHub Issues / Milestones / Labels
- 执行编排：`gh` CLI + Agent CLI
- 验证闭环：单测 + 构建 + Playwright + PR 检查清单
- 规范沉淀：`AGENTS.md` + 本仓库模板

## 对前后端仓库的约束

- `blog-FE` 与 `blog-BE` 必须各自维护 `AGENTS.md`
- 每个功能开发必须关联 Spec 与 Issue
- 没有 Harness 验证记录的 PR 不允许合并
- 发布后必须回写文档（进度、风险、经验）

## 快速开始

1. 先读 `AGENTS.md`
2. 按 `templates/spec-template.md` 写功能 Spec
3. 按 `templates/issue-template.md` 拆 Issue
4. 开发完成后按 `templates/harness-checklist.md` 验证
5. 更新 `docs/02-workflow/progress-sync-protocol.md` 约定的进度记录
