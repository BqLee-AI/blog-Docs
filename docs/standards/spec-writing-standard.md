# Spec 编写标准

## 目的

让 OpenSpec spec 可被 AI Agent 和人类共同消费，确保每条规则可验证、可追溯、可归档。

## 适用范围

- `openspec/specs/**/spec.md`
- `openspec/changes/<change-id>/specs/**/spec.md`
- `templates/spec-template.md`

## 必填章节

1. **目的**：说明这个 spec 管什么、不管什么。
2. **Requirements**：使用 MUST/SHALL/MUST NOT 语义描述行为要求。
3. **Scenario**：每个 Requirement 至少一个可验收场景。
4. **边界**：明确范围内和范围外。
5. **参考**：上游 spec、受影响仓库、契约文件或模板路径。

## Scenario 写法

每个 Requirement 下至少一个 Scenario：

```md
#### Scenario: <场景名>

- **GIVEN** <前置条件>
- **WHEN** <触发动作>
- **THEN** <期望结果>
```

规则：

- `GIVEN` 写可观察的前置状态。
- `WHEN` 写触发动作。
- `THEN` 写可判定 pass/fail 的结果。
- 复杂场景可以追加 `AND`，但不要把多个独立行为塞进一个 Scenario。

## 写作原则

- 可验证：每条 `THEN` 必须能通过测试、检查或人工验收判定。
- 长期有效：稳定 spec 不引用具体 issue 编号、PR 编号、日期或临时负责人。
- 单一职责：一个 Requirement 只描述一个行为约束。
- 不重复：跨 spec 共享的规则引用上游 spec，不重写。
- 默认中文：正文默认中文，MUST/SHALL/MUST NOT、路径、API 字段保留英文。
- 边界清楚：写明范围外，防止 Agent 自动扩需求。

## OpenSpec change 写法

- change id 使用 kebab-case 英文短语。
- `proposal.md` 写为什么做、做什么、影响范围、范围外、验证影响。
- `design.md` 写结构设计、落点、迁移和回滚。
- `tasks.md` 写可独立验收的任务清单。
- `spec.md` 写 Requirements 和 Scenarios，不写执行流水账。

## 反例

- “系统应该有良好的错误处理”：不可验证。
- “本周完成用户模块”：包含时间锚点。
- “参考上次说的”：缺少真源路径。
- 同一条规则在三个 spec 里各写一遍：重复且容易冲突。

## 验收检查

- 是否每个 Requirement 都有 Scenario？
- 是否每个 Scenario 都可验收？
- 是否写清楚范围外？
- 是否避免临时 issue、日期、负责人进入稳定 spec？
- 是否引用了正确模板和标准？
