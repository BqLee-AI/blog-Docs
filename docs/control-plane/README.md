# 控制面（control-plane）

## 这目录管什么

冻结的项目红线：范围边界、契约边界、变更边界、治理规则。

这里的东西**不能随便改**。小调整通过 PR + 审核即可，涉及边界/契约/门禁的重大变更需经充分讨论后走 OpenSpec。

## 包含文件

| 文件 | 职责 | 层级 |
|------|------|------|
| `control-plane-handbook.md` | 治理主真源，定义 durable rules 和冻结边界 | L0 |
| `ai-native-governance.md` | 三层控制面总则（Prompt/Context/Harness） | L1 |
| `phase-roadmap.md` | 阶段路线图（Phase 0-4 边界） | L1 |
| `mvp-scope.md` | Phase 1 MVP 范围冻结（必须做/不做/完成判定） | L1 |

## 和其他目录的关系

- 具体流程怎么走 → `docs/workflow/`
- 怎么验证是否合规 → `docs/harness/`
- 当前阶段推进到哪了 → `docs/runtime/`
