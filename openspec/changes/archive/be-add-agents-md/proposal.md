# Proposal: blog-BE 创建 AGENTS.md

## 为什么做

blog-BE 一直缺少 AGENTS.md，违反 `fe-be-adoption-gate.md` 第 3 条门禁要求（FE/BE 仓库 AGENTS.md 必须声明控制面读取顺序）。没有 AGENTS.md，AI Agent 进入 BE 仓库后无法知道该遵守什么规则、读取哪些控制面资产。

## 这次做什么

按 `agents-md-standard.md` 的 6 个必填章节，参照 `blog-Docs/AGENTS.md` 的角色定义风格，为 blog-BE 创建 AGENTS.md。

## 明确不做

- 不修改任何现有代码
- 不新增 API 端点
- 不修改配置文件

## 已定决策

- 采用角色定义 + 操作规范风格（参照 blog-Docs/AGENTS.md），不采用模板清单风格
- 分层架构描述与 develop 分支当前代码一致
- 关键约束回写到 AGENTS.md 自身，不回写到控制面

## 受影响仓库

- blog-BE（新增文件）
