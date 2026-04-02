# PR 审核 SOP（PM / 产评）

## 审核标签体系

- `BLOCKER`：必须修，不修不合并
- `REQUIRED`：本阶段要求内必须修
- `FOLLOW-UP`：不阻塞合并，转新 Issue

## 评论模板

### BLOCKER

```text
[BLOCKER]
问题：
影响：
修复建议：
验收方式：
```

### REQUIRED

```text
[REQUIRED][Stage-B/Stage-C]
问题：
违反规则：
修复建议：
```

### FOLLOW-UP

```text
[FOLLOW-UP]
优化点：
影响范围：
建议方向：
```

## 评审结论

- `Approve`
- `Approve with Follow-up Issues`
- `Request Changes`
- `Reject`

## 各阶段审核重点

### Stage A

- 只抓 BLOCKER
- 不做风格洁癖拦截

### Stage B

- BLOCKER + REQUIRED
- 格式、提交、测试、契约一致性

### Stage C

- BLOCKER + REQUIRED + 闭环证据
- 强制回写规范资产

## Follow-up Issue 规则

每条 FOLLOW-UP 必须创建 Issue，且包含：

- 背景
- 预期收益
- 优先级
- 所属阶段（B 或 C）
