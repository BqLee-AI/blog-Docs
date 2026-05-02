# 命名规范

## 目的

统一 Issue、PR、commit、branch、OpenSpec change 的命名方式，让人和 Agent 能快速判断变更类型、影响范围和结果。

## 默认语言

- 正文默认中文。
- 路径、命令、API 字段、operationId、包名、分支名、OpenSpec change id 保留英文。
- 标题允许使用英文 `type(scope):` 前缀，冒号后的摘要优先中文。

## Issue 标题

推荐格式：

```text
DOCS-GOV-001：建立 Agent-facing governance surfaces OpenSpec
FE-P1-001：打通邮箱验证码登录注册真实接口
BE-P1-001：补齐文章读写持久化 smoke
```

规则：

- 前缀表达仓库或任务域，例如 `DOCS-GOV`、`DOCS-P1`、`FE-P1`、`BE-P1`。
- 编号递增，避免复用。
- 冒号后写清结果，不写泛化动作。

## PR 标题

推荐格式：

```text
docs(standards): 统一 Agent 工作与命名规范
docs(templates): 优化 Issue 与 PR 模板入口
docs(openspec): 定义 Agent governance surfaces
```

规则：

- 使用 `type(scope): 中文结果描述`。
- `type` 常用值：`docs`、`feat`、`fix`、`chore`、`test`、`ci`。
- `scope` 应指向变更层或模块，例如 `openspec`、`standards`、`templates`、`runtime`、`harness`。
- 摘要描述结果，不描述过程。

不推荐：

```text
update docs
fix stuff
chore(openspec): bootstrap spec-driven governance workspace
docs: align phase 1 entrypoints
```

## Commit 标题

推荐与 PR 标题保持同一风格，但 commit 可以更细：

```text
docs(standards): 新增文档落点规范
docs(templates): 给 PR 模板补充回写入口
docs(agents): 固化默认中文与原子 PR 规则
```

规则：

- 一个 commit 对应一个可解释的改动意图。
- 不同职责边界拆 commit。
- 不把机械格式化和语义变更混在一个 commit，除非范围很小。

## Branch 命名

推荐格式：

```text
docs/standardize-agent-governance-surfaces
docs/update-pr-template
fix/runtime-dispatch-status
```

规则：

- 使用小写英文、数字、连字符和斜杠。
- 分支名表达任务，不塞 Issue 全标题。
- OpenSpec 类分支优先与 change id 对齐。

## OpenSpec change id

推荐格式：

```text
standardize-agent-governance-surfaces
phase-1-mvp-control-plane-plan
align-auth-article-openapi-contract
```

规则：

- 使用 kebab-case 英文短语。
- 表达治理变更或契约变更的结果。
- 正文可使用中文，保持人类验收友好。

## 验收检查

- 标题是否能看出影响层？
- 标题是否说明完成后的结果？
- 是否避免了 `update`、`fix stuff`、`misc` 等泛化词？
- Issue / PR / commit / branch 是否互相可追踪？
