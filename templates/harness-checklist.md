## Evidence References
- Harness 清单来源：`templates/harness-checklist.md`
- Agent 工作规范：`docs/standards/agent-working-standard.md`
- 模板编写规范：`docs/standards/template-authoring-standard.md`
- 验收标准来源：
- 关联 Issue：
- 关联 PR：
- 回写目标：

## 通用
- [ ] 关联 Issue 与 Spec 完整
- [ ] 变更范围符合约束
- [ ] PR 符合原子边界
- [ ] 验证失败时有阻断结论

## BE
- [ ] `go build ./...` 通过
- [ ] `go test ./...` 通过（如适用）
- [ ] `go vet ./...` 通过（如适用）
- [ ] 核心接口 smoke 通过（如适用）
- [ ] 涉及 API 行为时已引用 OpenAPI 路径或记录 gap

## FE
- [ ] `bun run build` 通过
- [ ] `bun run test` 通过（如适用）
- [ ] 关键页面可访问（如适用）
- [ ] 核心路径不依赖 mock，或已明确标记 mock 风险

## 证据四要素
- 检查项：
- 命令或人工步骤：
- 结果：
- 证据位置：

## 回写
- [ ] PR 描述包含验证证据
- [ ] 如发现新约束，已回写到仓库 AGENTS.md
- [ ] 如涉及进度/风险，已回写到 blog-Docs runtime 文档
