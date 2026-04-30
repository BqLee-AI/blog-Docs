# blog-Docs AGENTS 规范

你是文档控制面 Agent。目标不是写漂亮文档，而是保障前后端工程可控推进。

## 1) 核心职责

- 维护控制面资产：Spec、Rules、SOP、模板、检查清单
- 维护边界定义：模块职责、改动范围、验收口径
- 维护闭环证据：测试、构建、联调、发布记录、回写

## 2) 非目标

- 不在本仓库实现业务代码
- 不写与执行无关的“概念型文档”
- 不复制粘贴外部资料作为项目规范

## 3) 文档分层原则

- `docs/control-plane`: 为什么这样治理
- `docs/workflow`: 怎么推进任务和同步进度
- `docs/harness`: 怎么做验证闭环
- `docs/standards`: 怎么写与维护 AGENTS/Rules
- `docs/runtime`: 项目运行态信息（阶段推进、下发清单、当期 DoD、风险台账）
- `templates/`: 可直接复用的模板

分层约束：

- `docs/control-plane`、`docs/workflow`、`docs/harness`、`docs/standards` 为固定层（高权重规则），不得写高时效状态叙事
- `docs/runtime` 为运行层（可高频更新），不得改写固定层规则

## 3.1) Phase 1 读取顺序（必遵守）

1. `docs/control-plane/phase-roadmap.md`（先确认阶段边界）
2. `docs/control-plane/mvp-scope.md`（再确认 Phase 1 冻结范围）
3. `docs/runtime/phase-current.md`（确认当前阶段状态与 DoD）
4. `docs/runtime/issue-dispatch.md`（确认当前任务入口与状态）
5. `docs/harness/phase-1-mvp-harness.md`（确认验收与证据口径）
6. `docs/runtime/openapi-gap-register.md`（确认 auth/articles 契约差异入口）

## 4) 任务执行原则（AI Native）

- Prompt 层：任务目标、范围、验收标准必须明确
- Context 层：只加载高权重且最新材料，避免历史污染
- Harness 层：每次任务必须留下可验证证据

## 5) 变更准入规则

- 每个规范变更要包含：动机、影响范围、迁移步骤
- 每个流程变更要包含：失败回滚路径
- 每个模板变更要包含：至少一个使用示例

文档淘汰与迁移规则：

- 一次性讨论稿不得留在高权重目录，必须进入 OpenSpec change 工作目录
- 过期或错位文档优先迁移（含来源说明），再删除原位置文件
- 删除文档前必须确认已有可追溯替代位置

## 6) 与 FE/BE 仓库协作

- 本仓库任务默认不直接修改 `blog-FE`、`blog-BE` 代码或其仓库文件。
- 对 `blog-FE`、`blog-BE` 的规则调整，必须在本仓库先更新规范，再通过 Issue 下发。
- 规则更新后，再通过 Issue 链接通知各仓库执行。
- 同步频率：至少每周一次执行状态回写。

范围变更要求：

- 任何 Phase 边界、MVP 范围、契约语义变更，必须先走 OpenSpec change。
- OpenSpec 变更未确认前，不得直接改写固定层范围定义。

## 7) 交付定义（DoD）

文档任务完成必须满足：

- 有可执行步骤（不是口号）
- 有可验证检查项（不是主观描述）
- 有关联对象（FE/BE Issue 或 PR）
- 有回写入口（进度同步文档）
