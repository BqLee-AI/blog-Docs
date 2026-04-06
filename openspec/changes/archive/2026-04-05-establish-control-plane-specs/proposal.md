## Why

`blog-Docs` 已经承担控制面职责，但目前还没有被 OpenSpec 正式描述为一组可演进、可校验的能力规范。这样会让治理规则只能散落在普通 markdown 中，难以区分稳定约束与短期状态，也不利于后续 agent 通过 change/spec 机制持续维护。

## What Changes

- 引入 `control-plane-docs` 能力，约束稳定文档应优先记录边界、契约、验收与闭环，而不是阶段性状态。
- 引入 `spec-driven-governance` 能力，规定本仓库的治理规则变更应通过 OpenSpec change artifacts 进行提案、拆解、验证与归档。
- 将 OpenSpec 配置与文档仓库的长期约束对齐，避免在高权重规则正文中堆积时间敏感信息。

## Capabilities

### New Capabilities

- `control-plane-docs`: 定义控制面文档的稳定性边界、状态外置原则与高权重文档写法。
- `spec-driven-governance`: 定义文档仓库如何使用 OpenSpec 管理治理变更及其验证闭环。

### Modified Capabilities

- None.

## Impact

- Affected repository: `blog-Docs`
- Affected systems: OpenSpec change workflow, stable governance docs, release/sync SOP references
- Downstream consumers: `blog-FE` and `blog-BE` teams that read and follow control-plane rules
