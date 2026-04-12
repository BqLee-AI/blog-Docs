## Why

搜索链路依赖 BE 全文检索契约，但当前候选版本不应继续吸收该范围内的实现。需要先把搜索链路的冻结边界、契约依赖、烟 smoke 口径与回滚动作前置到 OpenSpec 变更中，避免实现分支提前进入 Ready。

## What Changes

- 将搜索链路相关内容冻结为 OpenSpec 前置变更，只有在契约与 Harness 计划对齐后才允许进入后续实现。
- 明确 FE-013 当前不并入候选版本，直到搜索契约冻结并且验证口径清晰。
- 将候选版本状态、风险与回滚口径放入低权重同步记录，而不是写入稳定规则正文。

## Capabilities

### New Capabilities

- `fe-search-freeze-boundary`: 约束搜索链路的冻结边界、延期策略与候选版本准入。
- `fe-search-harness-plan`: 约束搜索链路进入候选版前必须具备的 smoke 与回滚口径。

### Modified Capabilities

- `release-strategy`: 明确搜索链路在契约未冻结前不得进入 Ready。

## Impact

- Affected repository: `blog-Docs`
- Affected downstream repositories: `blog-FE`, `blog-BE`
- Affected systems: release freeze policy, OpenSpec change workflow, harness evidence recording
- Verification impact: 需要可复核的契约对齐、smoke 与回滚记录，才能解除冻结

## Non-goals

- 不在本 change 中实现 FE-013 的业务能力。
- 不新增搜索相关用户能力。
- 不修改 BE 全文检索契约本身。

## Migration

- 当前候选版本继续保持冻结，不将搜索链路并入实现分支。
- 搜索链路的状态与风险继续通过同步记录外置。
- 若后续契约冻结完成，再通过新的 change 决定是否解冻进入实现。

## Rollback

- 如搜索契约未能冻结，则维持当前延期状态，不推进到 Ready。
- 如误入候选版本，应立即回退到上一冻结 commit / tag，并撤销搜索链路的准入判断。