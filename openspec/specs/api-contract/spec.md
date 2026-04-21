# api-contract 能力规范

## 目的
定义 blog 系统所有 API 端点的契约规则，确保前后端基于同一份真源（OpenAPI YAML）开发，消除口头约定和猜测。

## 要求

### 要求：API 契约唯一真源为 OpenAPI YAML
所有 API 端点的请求参数、响应结构、状态码、认证方式必须以 `blog-Docs/contracts/api/openapi.yaml` 为唯一真源。禁止在自然语言文档中重新定义接口细节。

#### 场景：开发新端点
- **GIVEN** 需要新增一个 API 端点
- **WHEN** 开发者开始实现
- **THEN** 必须先在 openapi.yaml 中定义该端点，BE 和 FE 均基于此定义实现

#### 场景：FE 消费契约
- **GIVEN** FE 需要调用某个 API
- **WHEN** 开发者编写 API 调用代码
- **THEN** 必须从 openapi.yaml 生成类型，禁止手写与契约不一致的类型定义

### 要求：API 遵循统一响应格式
所有业务端点必须返回 `{ message, data, requestId, code }` 结构。HTTP 状态码承载主要语义，`code` 字段仅在需要细分错误时使用。

#### 场景：成功响应
- **WHEN** API 处理成功
- **THEN** 返回 2xx 状态码，`message` 描述结果，`data` 携带业务数据

#### 场景：错误响应
- **WHEN** API 处理失败
- **THEN** 返回对应 4xx/5xx 状态码，`code` 提供机器可读的错误码（如 `AUTH_FAILED`、`NOT_FOUND`）

### 要求：认证端点使用 Bearer Token
需要认证的端点必须通过 `Authorization: Bearer <access_token>` 传递 JWT（RS256 签名）。

#### 场景：已认证请求
- **WHEN** 请求携带有效 Bearer Token
- **THEN** 中间件解析 Claims 并注入上下文，handler 可获取 user_id/role_id

#### 场景：未认证访问受保护端点
- **WHEN** 请求未携带 Token 或 Token 无效
- **THEN** 返回 401 + `TOKEN_MISSING` 或 `TOKEN_INVALID`

### 要求：API 路由使用 /api/v1 前缀
所有业务端点必须挂在 `/api/v1` 前缀下。

#### 场景：新增路由
- **WHEN** 新增一个业务端点
- **THEN** 路径必须以 `/api/v1` 开头

## 边界

- 范围内：RESTful API 端点定义、请求/响应结构、认证方式、错误码
- 范围外：数据库表结构（见 domain-model spec）、部署配置、前端路由

## 参考

- 契约文件：`contracts/api/openapi.yaml`
- 受影响仓库：blog-BE、blog-FE
