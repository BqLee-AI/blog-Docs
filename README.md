# blog-Docs

BqLee-AI 项目的**控制面仓库**——指导 `blog-FE` 和 `blog-BE` 的工程标准与推进节奏。

## Phase 1 当前入口（先看这里）

- 固定层（阶段边界）：
  - `docs/control-plane/phase-roadmap.md`
  - `docs/control-plane/mvp-scope.md`
- 运行层（当前状态与任务）：
  - `docs/runtime/phase-current.md`
  - `docs/runtime/issue-dispatch.md`
  - `docs/runtime/risk-register.md`
- Harness（Phase 1 验收口径）：
  - `docs/harness/phase-1-mvp-harness.md`
- OpenAPI gap（auth/articles 差异台账）：
  - `docs/runtime/openapi-gap-register.md`

当前阶段优先完成 `blog-Docs` 控制面整理与对齐；`blog-FE` / `blog-BE` 的实现类 Issue 以后续下发为准。

## 核心工作流

```
PM 发起聚焦讨论（一次一件事）
    ↓
AI 帮忙收成决策 → 冻结到 spec / 契约
    ↓
从决策拆 Issue 下发到 FE/BE
    ↓
开发者执行 → PR 提供证据 → PM 对照 spec 验收
    ↓
合并后 AI 回写文档仓库（同步进度 + 发现问题 + 规划下一步）
    ↓
准备下一个聚焦讨论
```

完整示例见 [`docs/workflow/workflow-example.md`](docs/workflow/workflow-example.md)

## 目录结构

```text
.
├── AGENTS.md                          ← Agent 行为入口（必读）
├── docs/
│   ├── control-plane/                 ← 红线：范围/契约/变更边界（不能随便改）
│   │   ├── README.md
│   │   ├── control-plane-handbook.md  # L0 治理主真源
│   │   ├── ai-native-governance.md    # L1 三层控制面总则
│   │   ├── phase-roadmap.md           # L1 长周期阶段边界
│   │   └── mvp-scope.md               # L1 Phase 1 范围冻结
│   ├── workflow/                      ← 规则：怎么推进任务、审核 PR、发版
│   │   ├── spec-issue-test-flow.md
│   │   ├── change-intake-and-escalation.md
│   │   ├── pr-merge-gate-spec.md
│   │   ├── pr-review-sop.md
│   │   ├── release-strategy.md
│   │   ├── task-release-and-sync-sop.md
│   │   ├── progress-sync-protocol.md
│   │   ├── openspec-governance-baseline.md
│   │   └── workflow-example.md        # 端到端工作流示例
│   ├── harness/                       ← 验收：怎么验证是否合规
│   │   ├── README.md
│   │   ├── harness-closed-loop.md     # Harness 四步闭环
│   │   ├── harness-gate-baseline.md   # 合并门禁 + 发版门禁
│   │   └── phase-1-mvp-harness.md     # Phase 1 验收口径
│   ├── standards/                     ← 标准：怎么写 AGENTS / 仓库怎么接入
│   │   ├── agents-md-standard.md
│   │   └── fe-be-adoption-gate.md
│   └── runtime/                       ← 运行态：当前阶段、Issue 下发、风险台账（可高频更新）
│       ├── README.md
│       ├── phase-current.md           # 当前阶段定义
│       ├── issue-dispatch.md          # 本阶段 Issue 清单
│       ├── risk-register.md           # 风险台账
│       └── openapi-gap-register.md    # auth/articles 契约差异台账
├── openspec/                          ← OpenSpec 变更管理
│   ├── config.yaml
│   ├── specs/                         # L0 冻结 spec
│   └── changes/                       # 变更过程（进行中 + 归档）
└── templates/                         ← 可复用模板
    ├── spec-template.md
    ├── issue-template.md
    ├── pr-template.md
    └── harness-checklist.md
```

## 固定层 vs 运行层

| 层 | 目录 | 特点 | 可变性 |
|----|------|------|--------|
| 固定层 | `control-plane/` `workflow/` `harness/` `standards/` | 长期规则、红线、门禁 | 变更需充分讨论并确认影响面，重大变更走 OpenSpec |
| 运行层 | `runtime/` | 阶段推进、Issue 清单、风险台账 | 可高频更新 |

运行层文档可更新状态，但**不得改写固定层规则定义**。

## 高权重真源

- `docs/control-plane/control-plane-handbook.md`（L0）
- `docs/control-plane/mvp-scope.md`（L1）
- `docs/control-plane/ai-native-governance.md`（L1）
- `docs/workflow/release-strategy.md`（L1）
- `docs/workflow/progress-sync-protocol.md`（L2）
- `docs/workflow/task-release-and-sync-sop.md`（L2）

冲突裁决：L0 > OpenSpec 已接受变更 > L1/L2 专项规则 > 运行态同步记录

## 对前后端仓库的约束

- `blog-FE` 与 `blog-BE` 必须各自维护 `AGENTS.md`
- 每个功能开发必须关联 Spec 与 Issue
- 没有 Harness 验证记录的 PR 不允许合并
- 发布后必须回写文档（进度、风险、经验）
- 发版前必须冻结范围、记录候选版本、保留回滚口径

## 发布策略

- `develop` 是持续开发分支
- 发版不依赖 `develop -> main` 合并
- 发版以 `develop` 的冻结 commit 为准，通过 `tag` 或 `release/*` 分支完成
- 时间敏感状态不应写入高权重规则正文，应放在 `docs/runtime/`

## Phase 1 快速开始

1. 先读 `AGENTS.md`
2. 依次阅读 `docs/control-plane/phase-roadmap.md` → `docs/control-plane/mvp-scope.md`
3. 读取 `docs/runtime/phase-current.md` 与 `docs/runtime/issue-dispatch.md`
4. 需要验收口径时读取 `docs/harness/phase-1-mvp-harness.md`
5. 需要契约差异入口时读取 `docs/runtime/openapi-gap-register.md`
6. 范围变化先走 OpenSpec，再改固定层文档
