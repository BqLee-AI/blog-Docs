# be-agents-md Specification

## Purpose
定义 blog-BE AGENTS.md 必须包含的内容和格式要求，确保 AI Agent 进入仓库后能遵循正确的工作方式。

## Requirements

### Requirement: AGENTS.md 必须包含 6 个必填章节
AGENTS.md 必须按 `agents-md-standard.md` 定义包含：项目目标与非目标、技术边界、开发命令、代码规范、验证方式、文档回写要求。

#### Scenario: AI Agent 首次进入 blog-BE
- **GIVEN** AI Agent 被分配了一个 blog-BE 任务
- **WHEN** Agent 读取 AGENTS.md
- **THEN** Agent 能找到项目职责、分层架构、技术栈锁定、禁止跨越规则和变更边界

### Requirement: AGENTS.md 必须声明控制面读取顺序
按 `fe-be-adoption-gate.md` 第 3 条，AGENTS.md 必须明确接任务前先读哪些控制面资产。

#### Scenario: Agent 接到新任务
- **GIVEN** Agent 准备开始一个新任务
- **WHEN** Agent 按 AGENTS.md 读取控制面
- **THEN** Agent 先读 mvp-scope（确认范围），再读 openapi.yaml（确认契约），再读 harness-gate-baseline（确认验证标准）

### Requirement: 关键约束回写到 AGENTS.md
如果执行中发现对全局有长远影响的关键约束，必须回写到 AGENTS.md 对应章节。

#### Scenario: Agent 发现新的架构边界约束
- **GIVEN** Agent 在实现过程中发现一个容易被违反的架构规则
- **WHEN** 该约束不在当前 AGENTS.md 中
- **THEN** Agent 必须将该约束写入 AGENTS.md 对应章节

## Boundaries
- In scope: AGENTS.md 文件创建
- Out of scope: 代码变更、API 变更、配置变更

## References
- 标准：`docs/standards/agents-md-standard.md`
- 门禁：`docs/standards/fe-be-adoption-gate.md`
- 风格参考：`AGENTS.md`（blog-Docs 根目录）
- 风格参考：`blog-FE/AGENTS.md`
