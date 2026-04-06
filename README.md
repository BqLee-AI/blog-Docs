# blog-Docs

`blog-Docs` 是 BqLee AI 项目的**控制面仓库**：

- 管规范（Rules / AGENTS / SOP）
- 管边界（模块职责、改动范围、验收口径）
- 管闭环（Spec -> Issue -> PR -> Test -> Release -> 回写）
- 管高权重真源（scope freeze / release strategy / contract / stable SOP）

它不是知识堆放仓库，也不应长期堆积阶段性状态，而是指导 `blog-FE` 和 `blog-BE` 的稳定工程标准。

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
│   │   └── control-plane-handbook.md
│   │   └── ai-native-governance.md
│   │   └── mvp-scope.md
│   ├── 02-workflow
│   │   ├── release-strategy.md
│   │   ├── project-dashboard.md
│   │   ├── spec-issue-test-flow.md
│   │   └── progress-sync-protocol.md
│   │   ├── change-intake-and-escalation.md
│   │   ├── openspec-governance-baseline.md
│   │   ├── pr-merge-gate-spec.md
│   │   ├── pr-review-sop.md
│   │   └── task-release-and-sync-sop.md
│   ├── 03-harness
│   │   └── harness-closed-loop.md
│   │   └── harness-gate-baseline.md
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

发布策略原则：

- `develop` 是持续开发分支
- 发版不依赖 `develop -> main` 合并
- 发版以 `develop` 的冻结 commit 为准，通过 `tag` 或 `release/*` 分支完成
- 时间敏感的进度、风险、候选版状态不应写入高权重规则正文，应放在外部同步记录或临时运行面板

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
- 发版前必须冻结范围、记录候选版本、保留回滚口径

## 高权重真源

- `docs/01-control-plane/control-plane-handbook.md`
- `docs/01-control-plane/mvp-scope.md`
- `docs/01-control-plane/ai-native-governance.md`
- `docs/02-workflow/release-strategy.md`
- `docs/02-workflow/progress-sync-protocol.md`
- `docs/02-workflow/task-release-and-sync-sop.md`

## 快速开始

1. 先读 `AGENTS.md`
2. 先确认 `docs/01-control-plane/mvp-scope.md` 的范围冻结规则
3. 按 `templates/spec-template.md` 写功能 Spec
4. 按 `templates/issue-template.md` 拆 Issue
5. 开发完成后按 `templates/harness-checklist.md` 验证
6. 发版前核对 `docs/02-workflow/release-strategy.md`
7. 如需记录运行状态，按模板生成当期同步记录，不要长期改写高权重规则正文
