# Harness 验证清单

## 通用
- [ ] 关联 Issue 与 Spec 完整
- [ ] 变更范围符合约束
- [ ] 验证失败时有阻断结论

## BE
- [ ] `go build ./...` 通过
- [ ] `go test ./...` 通过（如适用）
- [ ] 核心接口 smoke 通过（如适用）

## FE
- [ ] `bun run build` 通过
- [ ] 关键页面可访问（如适用）

## 回写
- [ ] PR 描述包含验证证据
- [ ] 如发现新约束，已回写到仓库 AGENTS.md
- [ ] 如涉及进度/风险，已回写到 blog-Docs runtime 文档
