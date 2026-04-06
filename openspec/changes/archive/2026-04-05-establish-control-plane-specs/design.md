## Overview

本次变更把 `blog-Docs` 的长期治理约束正式收敛为 OpenSpec 能力规范。实现方式不是复制现有文档，而是抽取两类稳定能力：

1. `control-plane-docs`
2. `spec-driven-governance`

这两类能力会作为后续治理变更的规范基线存在于 `openspec/specs/` 中。

## Design Decisions

### 1. 稳定规则与短期状态分离

- 稳定规则继续保留在 `docs/` 和 `AGENTS.md`
- 时间敏感状态继续放在同步记录、复制后的 dashboard、候选版记录等低权重载体
- OpenSpec spec 只定义长期有效的 SHALL/MUST 级要求，不记录某次迭代状态

### 2. 能力按治理关注点拆分

- `control-plane-docs` 关注文档内容边界和高权重真源写法
- `spec-driven-governance` 关注 OpenSpec 在仓库中的变更入口、产物要求和验证链路

这样可以避免把“文档写法”和“变更流程”混成一个过大的能力域。

### 3. OpenSpec 作为治理变更入口

未来凡是以下类型的变更，应优先走 OpenSpec change：

- 改治理规则
- 改文档分层或真源定义
- 改 Spec -> Issue -> PR -> Release 的流程约束
- 改 release / sync / harness 的准入要求

纯文字润色、错别字、链接修复可继续直接修改 markdown，不强制走 change。

## Migration

- 现有 `docs/` 不需要整体重写
- 后续只需在治理规则发生变化时新增 OpenSpec change
- 当前这条 change 归档后，将生成初始基线 specs，供后续 change 做增量修改

## Verification

- `openspec validate establish-control-plane-specs`
- 归档后应能在 `openspec/specs/` 看到基线能力规范
- 后续 `openspec list --specs` 应能列出稳定能力名
