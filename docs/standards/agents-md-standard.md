# AGENTS.md 编写标准

## 目的

让每个仓库的 Agent 入口短、硬、可执行。`AGENTS.md` 不是知识库，也不是完整流程手册，它只承载 Agent 开工时必须立即遵守的规则。

## 适用范围

- `blog-Docs/AGENTS.md`
- 后续同步到 `blog-FE`、`blog-BE` 的仓库级 `AGENTS.md`

## 必填章节

1. 项目职责与非目标。
2. 必读顺序。
3. 可改与不可改边界。
4. OpenSpec 或上游控制面触发条件。
5. 命名、语言和原子 PR 硬规则。
6. 最小验证方式。
7. 文档或 runtime 回写要求。

## 写作规则

- 短：优先控制在 1 到 2 屏，复杂解释放 `docs/standards/`。
- 准：只写当前有效规则，不写历史背景。
- 硬：使用“必须/不得/先/后”等可执行表述。
- 有入口：长规则必须引用具体路径，不只写“参考文档”。
- 不重复：不复制模板全文，不复制 standards 全文。

## 必须固化的高频规则

- 正文默认中文，路径、命令、API 字段等技术标识保留英文。
- 先读 `AGENTS.md`，再读 Issue 中列出的输入真源。
- 涉及 scope、contract、harness、gate、长期规则时先走 OpenSpec。
- PR 必须按 Issue 或职责边界原子化。
- PR 必须包含验证证据和回写说明。
- 临时状态不得写进固定层文档。

## 不应写入 AGENTS.md

- 阶段任务明细。
- 某个 Issue 或 PR 的当前状态。
- 大段示例和解释。
- 模板完整字段。
- 已归档的历史讨论。

## 验收检查

- Agent 只读 AGENTS 是否能避免高频错误？
- 是否能从 AGENTS 找到 standards、templates、runtime 入口？
- 是否没有把长期标准全文塞进 AGENTS？
- 是否没有把运行态状态写进 AGENTS？

## 参考

- 仓库根入口：`AGENTS.md`
- Agent 工作规范：`docs/standards/agent-working-standard.md`
- 文档落点规范：`docs/standards/document-placement-standard.md`
- 模板编写规范：`docs/standards/template-authoring-standard.md`
