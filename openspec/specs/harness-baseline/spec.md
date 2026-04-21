# harness-baseline 能力规范

## 目的
定义 blog 系统各仓库的验证基线——每次变更必须满足的最低验证标准。

## 要求

### 要求：BE 变更必须通过编译
blog-BE 的每次代码变更必须通过 `go build ./...`。

#### 场景：提交 BE PR
- **GIVEN** 开发者修改了 blog-BE 代码
- **WHEN** 准备提交 PR
- **THEN** `go build ./...` 必须成功，无编译错误

### 要求：FE 变更必须通过构建
blog-FE 的每次代码变更必须通过 `bun run build`。

#### 场景：提交 FE PR
- **GIVEN** 开发者修改了 blog-FE 代码
- **WHEN** 准备提交 PR
- **THEN** `bun run build` 必须成功（含 TypeScript 检查 + Vite 打包）

### 要求：PR 必须包含 Harness 证据
每个 PR 描述中必须包含验证证据，格式为四要素：检查项、命令、结果、证据。

#### 场景：PR 包含验证证据
- **GIVEN** 开发者提交 PR
- **WHEN** PR 描述中填写验证部分
- **THEN** 必须包含至少 Build 检查项，格式：`| 检查项 | 命令 | 结果 | 证据 |`

### 要求：契约变更必须同步
修改 openapi.yaml 后，BE 和 FE 的实现必须与更新后的契约一致。

#### 场景：契约变更
- **GIVEN** openapi.yaml 发生变更
- **WHEN** PR 涉及 API 契约
- **THEN** BE handler 和 FE API 层必须同步更新以匹配新契约

## 边界

- 范围内：编译/构建门禁、PR 证据格式、契约一致性
- 范围外：自动化 CI 流水线（延后至多人协作阶段）、E2E 测试、性能测试

## 参考

- 验证标准：`docs/harness/harness-gate-baseline.md`
- 契约文件：`contracts/api/openapi.yaml`
