# Harness 阶段计划

## Phase 0：工程秩序（当前）

验证内容：每次变更至少能编译/构建通过。

| 仓库 | 命令 | 作用 |
|------|------|------|
| blog-BE | `go build ./...` | 编译通过 |
| blog-BE | `go vet ./...` | 静态分析 |
| blog-BE | `go test ./...` | 单元测试 |
| blog-FE | `bun run build` | TS 检查 + 打包 |

执行方式：本地手动执行，PR 中提供证据。

## Phase 1：契约验证

验证内容：API 实现与 OpenAPI 契约一致。

| 检查项 | 工具 | 作用 |
|--------|------|------|
| OpenAPI lint | spectral / redocly-cli | 契约格式合法 |
| 契约一致性 | 对比 handler 路由与 openapi.yaml | 端点/字段对齐 |
| Smoke 脚本 | curl / httpie | 核心链路冒烟 |

## Phase 2：交付闭环

验证内容：关键用户路径端到端可用。

| 检查项 | 工具 | 作用 |
|--------|------|------|
| E2E 测试 | Playwright | 关键路径自动化验证 |
| 发版门禁 | build + test + E2E | 发版前全量检查 |
| 部署验证 | docker compose up | 一键启动验证 |
