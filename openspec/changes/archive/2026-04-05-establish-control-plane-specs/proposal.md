## 为什么做

`blog-Docs` 已经承担控制面职责，但目前还没有被 OpenSpec 正式描述为一组可演进、可校验的能力规范。这导致治理规则只能散落在普通 markdown 中，难以区分稳定约束与短期状态，也不利于后续 Agent 通过 change/spec 机制持续维护。

## 变更内容

- 引入 `control-plane-docs` 能力，约束稳定文档应优先记录边界、契约、验收与闭环，而非阶段性状态
- 引入 `spec-driven-governance` 能力，规定本仓库的治理规则变更应通过 OpenSpec change 产物进行提案、拆解、验证与归档
- 将 OpenSpec 配置与文档仓库的长期约束对齐，避免在高权重规则正文中堆积时间敏感信息

## 能力

### 新增能力

- `control-plane-docs`：定义控制面文档的稳定性边界、状态外置原则与高权重文档写法
- `spec-driven-governance`：定义文档仓库如何使用 OpenSpec 管理治理变更及其验证闭环

### 修改的能力

- 无

## 影响范围

- 受影响仓库：`blog-Docs`
- 受影响系统：OpenSpec change 工作流、稳定治理文档、发版/同步 SOP 引用
- 下游消费者：读取并遵循控制面规则的 `blog-FE` 和 `blog-BE` 团队
