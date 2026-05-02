# 设计说明

## 设计目标

这次变更的目标不是增加更多文档，而是把 Agent 最容易漏读、误写、误判的规则放到正确位置：

- `AGENTS.md`：短、硬、高频，Agent 开工即读。
- `docs/standards/`：可复用的长期标准，解释规则背后的判断方法。
- `templates/`：可直接复制使用的执行表单，不承载长篇解释。
- `docs/runtime/`：阶段状态、Issue 派发、风险和回写记录。
- `docs/control-plane/`：长期范围、阶段边界、治理红线。

## 分层策略

### `AGENTS.md`

只放 Agent 不读就容易犯错的硬规则：

- 默认语言和标题风格。
- 必读顺序。
- 原子 Issue / PR 规则。
- OpenSpec 触发条件。
- 验收证据和回写要求。
- 禁止把临时状态写入固定层。

### `docs/standards/`

放完整标准和判断口径：

- `agent-working-standard.md`：Agent 接任务、判断层级、执行、验收、回写。
- `naming-standard.md`：Issue / PR / commit / branch / OpenSpec change 命名。
- `document-placement-standard.md`：内容应该放到哪个目录，什么不需要记录。
- `template-authoring-standard.md`：模板字段、语言、路径引用、可填写性。
- 既有 `agents-md-standard.md`、`spec-writing-standard.md`、`fe-be-adoption-gate.md` 继续保留并对齐这些标准。

### `templates/`

模板只解决“提交时怎么填”，不复制 standards 全文。所有模板必须显式列出工作流参考路径，使外部协作者或新 Agent 能从 Issue / PR 自己找到控制面上下文。

## 命名与语言策略

- 文档正文、Issue 正文、PR 正文默认中文。
- 路径、命令、API 字段、operationId、包名、分支名保留英文。
- 标题可使用英文 Conventional Commit type/scope，但冒号后的摘要优先中文。
- commit / PR / Issue 标题应描述结果，不使用 `update docs`、`fix stuff`、`bootstrap ...` 这类泛化描述。

## 迁移与回滚

迁移方式：

- 新增 standards 文件，避免把所有规则塞进 `AGENTS.md`。
- 优化模板字段，使实际 PR 不再保留“PR 模板”这类模板标题。
- README / AGENTS 只更新入口和硬规则引用。

回滚方式：

- 如果某个模板字段过重，可单独回退该模板文件。
- 如果某条标准产生冲突，以 `docs/control-plane/control-plane-handbook.md` 与已接受 OpenSpec 为裁决依据。

## 风险

- 模板过长导致 Agent 继续跳读：通过“模板只放表单，解释放 standards”控制。
- 规则重复导致冲突：通过 `document-placement-standard.md` 定义落点，避免同一规则多处展开。
- 命名规范过度约束：保留中文摘要和技术英文标识共存，优先可读与可追踪。
