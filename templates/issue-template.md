# Issue 模板

## 类型
- [ ] Feature
- [ ] Bug
- [ ] Chore

## 变更分级（必填，三选一）
- [ ] L0 可直接执行（Direct Execute）
- [ ] L1 必须 OpenSpec
- [ ] L2 必须 BLOCKER 升级

## 发布标签（必填）
- [ ] `level:l0-direct` / `level:l1-openspec` / `level:l2-blocker`
- [ ] `freeze:locked` / `freeze:change-request`
- [ ] `status:ready` / `status:blocked` / `status:deferred` / `status:done`
- [ ] 若纳入候选版本：`release:candidate`
- [ ] 若启用阶段/批次标签（如 week/phase），按 `docs/runtime/` 当前阶段文档执行

## 写法约束（防冗余 / 防过期）
- [ ] `背景` 不超过 3 行，只写事实与约束，不写会议过程
- [ ] `目标` 不超过 3 条，使用可验收表述
- [ ] `DoD` 不超过 5 条，避免口号化描述
- [ ] 不写具体日期、临时负责人、阶段口号（放到同步记录）

## 分级判定依据（勾选触发项）
### L0 自检（仅当以下全满足才可选 L0）
- [ ] 改动仅在授权范围且符合冻结边界
- [ ] 不新增功能、不扩边界
- [ ] 不变更 API/数据/错误语义契约
- [ ] 已给出可执行 Harness 计划与证据口径

### L1 触发项（任一命中即必须 OpenSpec）
- [ ] 新增功能或扩展范围
- [ ] 变更 API/数据模型/错误语义契约
- [ ] 跨仓库/跨团队依赖调整
- [ ] 需要新增验收规则或回滚策略

### L2 触发项（任一命中即必须升级 BLOCKER）
- [ ] 边界冲突或契约冲突导致无法继续
- [ ] 存在安全、数据破坏或线上事故风险
- [ ] P0/P1 关键路径阻塞超过 4 小时
- [ ] 缺少验证前置或无法提供 Harness 证据

## 背景

## 目标

## 范围
- In Scope:
- Out of Scope:
- Frozen Boundary:
  - [ ] 不修改未授权模块
  - [ ] 不引入额外功能扩张
  - [ ] 不变更未冻结的契约

## 输入真源
- Scope/Rule:
- Contract:
- Spec:

## 实现约束
- 只允许修改目录：
- 禁止修改目录：
- 依赖前置条件：

## 验收标准（DoD）
- [ ]

## Harness 计划
- 必跑命令：
- 关键 smoke：
- 证据形式（日志/截图/摘要）：

## 关联
- Spec:
- 上游依赖:
- 相关 PR:
