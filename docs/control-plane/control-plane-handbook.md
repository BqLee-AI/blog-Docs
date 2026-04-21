# 控制面治理主手册（Control Plane Handbook）

## 1. 定位

本手册是 `blog-Docs` 的控制面治理主真源，用于冻结长期有效的治理规则与变更边界。

适用范围：

- `blog-Docs`（规则、流程、模板、闭环定义）
- `blog-FE` 与 `blog-BE`（受控于本仓库定义的范围、契约、验收与变更准入）

本手册只承载 durable rules，不承载周节奏、阶段状态、临时负责人等高时效内容。

## 2. Durable Rules（长期规则）

- 先边界，后执行：任何实现、拆解、发布都必须先对齐已冻结边界。
- 先契约，后联调：跨仓协作以冻结契约为准，不以临时沟通替代。
- 先验证，后合并：影响边界或治理口径的改动，必须有可审计证据。
- 先规范，后扩展：新增治理要求必须先进入 OpenSpec，再落入主文档。
- 稳态文档去时效化：高权重文档禁止记录短期进度、临时分工、阶段口号。
- 文档最小化：只保留“决策、边界、门禁、回滚”四类信息，过程叙事与会议纪要外置。
- 防过期优先：运行态状态、周节奏与里程碑进度必须写入同步记录，不写入高权重真源正文。

## 3. 高权重真源列表（Authority Map）

L0（治理主真源）：

- `docs/control-plane/control-plane-handbook.md`（本手册）

L1（控制面规则真源）：

- `docs/control-plane/ai-native-governance.md`
- `docs/control-plane/mvp-scope.md`
- `docs/workflow/spec-issue-test-flow.md`
- `docs/workflow/release-strategy.md`

L2（约束执行与验证真源）：

- `docs/workflow/task-release-and-sync-sop.md`
- `docs/workflow/pr-review-sop.md`
- `docs/harness/harness-closed-loop.md`

OpenSpec（治理变更准入真源）：

- `openspec/specs/control-plane-docs/spec.md`
- `openspec/specs/spec-driven-governance/spec.md`

冲突裁决顺序：`L0 > OpenSpec 已接受变更 > L1/L2 专项规则 > 运行态同步记录`。

## 4. 冻结边界（Freeze Boundaries）

### 4.1 Scope Boundary（范围边界）

冻结对象：

- 必须交付的用户链路与系统能力
- 明确排除项（本次不做）
- 验收口径中的能力边界

冻结规则：

- 未在冻结范围内的新增能力，不得直接进入实现分支。
- 对“必须做/不做”清单的改动，视为边界变更，必须走 OpenSpec。

### 4.2 Contract Boundary（契约边界）

冻结对象：

- API 输入/输出结构
- 领域数据语义与错误语义
- 跨端事件与状态约定

冻结规则：

- 影响兼容性的契约变更，不得通过“口头约定 + 直接改码”生效。
- 未给出兼容策略、迁移路径或回滚口径的契约改动，不得合并。

### 4.3 Change Boundary（变更边界）

变更分级：

- 边界内变更：不改变 scope/contract/验收门槛，仅做表述澄清或实现修复，可走常规 PR。
- 边界影响变更：触及 scope/contract/验收门槛/治理职责，必须先创建 OpenSpec 变更。
- 紧急止损变更：允许最小化止血，但在最终合并前必须补齐 OpenSpec 变更与审计链路。

## 5. OpenSpec 变更入口（唯一准入）

以下情形必须先走 OpenSpec，再更新高权重文档或执行实现：

- 调整 scope boundary
- 调整 contract boundary
- 调整 change boundary 或合并门槛
- 调整控制面真源层级、职责边界、验证闭环

最小准入包：

1. 在 `openspec/changes/<change-id>/` 建立 `proposal.md`、`tasks.md`、`specs/*/spec.md`。
2. `proposal.md` 必须写清：变更原因、影响边界、影响仓库、回滚口径。
3. `spec.md` 必须使用可验证的稳定约束与场景（避免阶段性表述）。
4. `tasks.md` 必须给出验证任务与完成证据要求。
5. OpenSpec 审核通过后，方可更新 L0/L1/L2 文档与下游仓库执行基线。

## 6. 文档冻结与解冻规则

- 冻结默认：`scope/contract/change` 三类边界默认冻结，未获准入不得改写。
- 解冻条件：仅当 OpenSpec 变更被接受，且影响面与回滚口径明确，才允许解冻改写。
- 回写要求：治理变更完成后，必须回写至对应高权重文档，保持“规范与执行基线一致”。

## 7. 执行检查清单（合并前）

- 是否触及 scope boundary？
- 是否触及 contract boundary？
- 是否触及 change boundary？
- 如触及，是否已有 OpenSpec 变更并通过审核？
- PR 是否附带验证证据与回滚口径？

任一项不满足，则不得将变更视为新的治理基线。
