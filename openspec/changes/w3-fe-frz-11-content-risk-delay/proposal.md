## Why

Week 3 的内容与风控链路不应直接并入当前候选版本。现阶段需要先把冻结边界、回滚口径和 Harness 计划前置到 OpenSpec 变更中，避免发布壳继续吸收范围外内容，导致候选版本边界漂移。

## What Changes

- 将本轮发布边界明确为只允许 FE-009 / FE-010 / FE-011 / FE-012 进入 OpenSpec 前置范围，暂不进入实现分支。
- 要求内容与风控链路相关变更先完成契约对齐、回滚口径和验证计划，再决定是否解冻进入候选版本。
- 将候选版本状态与风险台账外置到低权重同步记录，避免写入稳定规则正文。

## Capabilities

### New Capabilities

- `fe-release-freeze-boundary`: 约束 FE 冻结边界、候选版本准入与延期处理方式。
- `fe-release-harness-plan`: 约束发布前必须具备的构建、smoke 与回滚证据口径。

### Modified Capabilities

- `release-strategy`: 将候选版本前置冻结、回滚与 smoke 口径的执行要求显式化。

## Impact

- Affected repository: `blog-Docs`
- Affected downstream repositories: `blog-FE`
- Affected systems: release freeze policy, OpenSpec change workflow, harness evidence recording
- Verification impact: 需要可复核的构建/烟 smoke / 回滚记录后，才能判断是否进入候选版本

## Non-goals

- 不在本 change 中实现 FE-009 / FE-010 / FE-011 / FE-012 的业务能力。
- 不新增用户能力或修改业务契约。
- 不把阶段性状态写入高权重规则正文。

## Migration

- 现有同步记录继续承载短期状态与推进节奏。
- 稳定规则正文仅接收长期有效的边界与门禁描述。
- 如后续需要将本次冻结边界转为长期规则，再单独走新的 OpenSpec change。

## Rollback

- 如评估为不应冻结，可撤销该 change 对应的边界收束，并保持 FE-009~012 处于延后状态。
- 如候选版本验证失败，可继续保留当前冻结边界，不向实现分支追加范围外内容。
