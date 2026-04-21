# PR 模板

## 变更摘要

## 关联真源
- Issue:
- Spec:
- Contract/Rule:

## 发布标签（必填）
- Level Label: `level:l0-direct` / `level:l1-openspec` / `level:l2-blocker`
- Freeze Label: `freeze:locked` / `freeze:change-request`
- Status Label: `status:ready` / `status:blocked` / `status:deferred` / `status:done`
- Candidate Label（纳入候选版本时）: `release:candidate`
- Phase/Batch Label（如 week/phase，若启用）: 按 `docs/runtime/` 当前阶段文档执行

## 写法约束（防冗余 / 防过期）
- 变更摘要不超过 5 行，只写“改了什么 + 为什么”
- 证据摘要不重复日志全文，只保留结论与定位
- 不写阶段叙事、会议过程、临时分工

## 冻结边界检查
- [ ] 只修改授权范围内文件
- [ ] 未引入范围外功能
- [ ] 未破坏既定契约
- [ ] 如涉及契约变更，已先更新文档真源

## 合并门禁必填（不得留空）
- Gate Spec: docs/workflow/pr-merge-gate-spec.md
- 必需验证项状态: Build/Typecheck [PASS/FAIL/N/A] | Unit Test [PASS/FAIL/N/A] | Smoke [PASS/FAIL/N/A]
- Blocker 状态: 未解决 BLOCKER 数量 = [0/非0]

## Harness 验证证据（必填，逐项填写）
### Build/Typecheck
- Command:
- Result: PASS | FAIL | N/A（原因）
- Evidence:
- Conclusion:

### Unit Test
- Command:
- Result: PASS | FAIL | N/A（原因）
- Evidence:
- Conclusion:

### Smoke
- Command:
- Result: PASS | FAIL | N/A（原因）
- Evidence:
- Conclusion:

## 证据摘要（必填）
- 关键日志/截图结论：
- 证据链接或路径：

## 风险与回滚
- 风险:
- 回滚:

## 回写
- [ ] 已更新必要文档（规则/模板/同步记录）
