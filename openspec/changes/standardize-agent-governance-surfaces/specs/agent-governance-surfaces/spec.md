# agent-governance-surfaces 能力规范

## 目的

定义 Agent-facing governance surfaces 的职责边界和最低内容要求，使 AI Agent 与人类协作者在接 Issue、开 PR、写 Spec、验收和回写时能从同一套入口执行任务。

## ADDED Requirements

### Requirement: Agent 入口必须短路径可执行

`AGENTS.md` MUST 只承载 Agent 开工必须立即遵守的高频硬规则，并指向更完整的 standards 文档。

#### Scenario: Agent 接到文档仓库任务

- **GIVEN** Agent 接到 `blog-Docs` 任务
- **WHEN** 它读取 `AGENTS.md`
- **THEN** 它必须能知道默认语言、读取顺序、OpenSpec 触发条件、原子 PR 规则、验收证据要求和回写要求
- **AND** 它必须能找到 `docs/standards/` 中的完整规范入口

#### Scenario: 标准规则过长

- **GIVEN** 某条规则需要较长解释、示例或判断分支
- **WHEN** 该规则不是 Agent 每次开工必须立即记住的硬约束
- **THEN** 它必须写入 `docs/standards/`
- **AND** `AGENTS.md` 只保留一句短规则和路径引用

### Requirement: 文档落点必须可判断

控制面 MUST 定义内容落点规则，说明什么写入 `AGENTS.md`、`docs/control-plane/`、`docs/runtime/`、`docs/harness/`、`docs/standards/`、`templates/`，以及什么不需要记录。

#### Scenario: Agent 准备记录一条信息

- **GIVEN** Agent 准备把一条信息写入文档仓库
- **WHEN** 它无法判断落点
- **THEN** 它必须参考 `docs/standards/document-placement-standard.md`
- **AND** 不得把临时状态写入固定层文档

#### Scenario: 信息没有复用价值

- **GIVEN** 某条信息只是临时聊天过程、一次性命令输出或已被 git diff 完整表达的琐碎说明
- **WHEN** 它没有形成决策、验收证据、风险或可复用规则
- **THEN** 它不应被写入稳定文档

### Requirement: 模板必须自带工作流入口

Issue、PR、Spec 和 Harness 模板 MUST 显式声明模板来源、上游真源、验收入口和回写入口。

#### Scenario: 新 Agent 只拿到一个 Issue

- **GIVEN** 新 Agent 没有完整工作流上下文
- **WHEN** 它读取 Issue 正文
- **THEN** 它必须能看到 Issue 模板参考路径、PR 模板参考路径、Harness 清单、Agent 工作规范和文档落点规范
- **AND** 它必须能按这些路径继续加载上下文

#### Scenario: Reviewer 只打开一个 PR

- **GIVEN** Reviewer 只打开 PR 页面
- **WHEN** 它检查 PR 描述
- **THEN** 它必须能看到关联 Issue、Spec、Contract/Rule、验收标准和回写目标
- **AND** 它必须能判断该 PR 是否符合原子化边界

### Requirement: 命名必须统一且可追踪

Issue、PR、commit、branch、OpenSpec change 的命名 MUST 遵循统一规则，正文默认中文，技术标识保留英文。

#### Scenario: Agent 创建 PR 或 commit

- **GIVEN** Agent 准备创建 PR 或 commit
- **WHEN** 它编写标题
- **THEN** 标题应使用 `type(scope): 中文结果描述` 格式
- **AND** 不应使用 `update docs`、`fix stuff`、`bootstrap workspace` 等泛化标题

#### Scenario: Agent 创建 OpenSpec change

- **GIVEN** Agent 准备创建 OpenSpec change
- **WHEN** 它命名 change id
- **THEN** change id 必须使用 kebab-case 英文短语
- **AND** proposal/design/tasks/spec 正文默认中文

### Requirement: Standards 与 Templates 不得重复膨胀

Standards MUST 定义判断规则，Templates MUST 提供可填写字段。模板不得复制 standards 全文，standards 不得伪装成执行表单。

#### Scenario: 模板需要解释规则

- **GIVEN** 模板字段背后有复杂判断
- **WHEN** 该解释超过简短提示
- **THEN** 模板必须引用对应 standard
- **AND** 不得在模板内复制长篇规则正文

#### Scenario: 标准需要执行入口

- **GIVEN** 某个 standard 规定了 Issue 或 PR 必填字段
- **WHEN** Agent 需要实际提交
- **THEN** standard 必须指向对应模板
- **AND** 模板必须能独立填写并提交

## 边界

- 范围内：Agent 工作规范、命名规范、文档落点规范、模板编写规范、Issue/PR/Spec/Harness 模板、README/AGENTS 入口。
- 范围外：Phase 1 MVP 范围、OpenAPI 契约大修、FE/BE 代码实现、FE/BE 模板同步落地。

## 参考

- Agent 入口：`AGENTS.md`
- 标准目录：`docs/standards/`
- 模板目录：`templates/`
- 控制面主真源：`docs/control-plane/control-plane-handbook.md`
- 当前模板：`templates/issue-template.md`、`templates/pr-template.md`、`templates/spec-template.md`、`templates/harness-checklist.md`
