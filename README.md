# blog-Docs

BqLee-AI 项目的**控制面仓库**——指导 `blog-FE` 和 `blog-BE` 的工程标准与推进节奏。

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

完整示例见 [`docs/runtime/workflow-example.md`](docs/runtime/workflow-example.md)

## 目录结构

```text
.
├── AGENTS.md                          ← Agent 行为入口（必读）
├── docs/
│   ├── control-plane/                 ← 红线：范围/契约/变更边界（不能随便改）
│   │   ├── README.md
│   │   ├── control-plane-handbook.md  # L0 治理主真源
│   │   ├── ai-native-governance.md    # L1 三层控制面总则
│   │   └── mvp-scope.md               # L1 MVP 范围冻结
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
│   │   └── harness-gate-baseline.md   # 合并门禁 + 发版门禁
│   ├── standards/                     ← 标准：怎么写 AGENTS / 仓库怎么接入
│   │   ├── agents-md-standard.md
│   │   └── fe-be-adoption-gate.md
│   └── runtime/                       ← 运行态：当前阶段、Issue 下发、风险台账（可高频更新）
│       ├── README.md
│       ├── phase-current.md           # 当前阶段定义
│       ├── issue-dispatch.md          # 本阶段 Issue 清单
│       └── risk-register.md           # 风险台账
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

## 快速开始

1. 先读 `AGENTS.md`
2. 确认 `docs/control-plane/mvp-scope.md` 的范围冻结规则
3. 按 `templates/spec-template.md` 写功能 Spec
4. 按 `templates/issue-template.md` 拆 Issue
5. 开发完成后按 `templates/harness-checklist.md` 验证
6. PR 按 `templates/pr-template.md` 填写证据
7. 发版前核对 `docs/workflow/release-strategy.md`
8. 运行态信息写入 `docs/runtime/`，不要改写高权重规则正文
