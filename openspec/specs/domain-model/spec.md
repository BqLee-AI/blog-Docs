# domain-model 能力规范

## 目的
定义 blog 系统核心领域模型及其关系，确保前后端对数据实体有一致理解。

## 要求

### 要求：核心实体及其字段
系统必须包含以下核心实体，字段定义与 GORM 模型保持一致。

#### 场景：User 实体
- **GIVEN** 系统需要管理用户
- **WHEN** 定义 User 模型
- **THEN** 必须包含：ID、Username、Password（bcrypt 哈希）、Email（唯一）、RoleID（0=普通, 1=管理员, 2=超级管理员）

#### 场景：Article 实体
- **GIVEN** 系统需要管理文章
- **WHEN** 定义 Article 模型
- **THEN** 必须包含：ID、Title、Content、Summary、CoverImage、AuthorID（FK→User）、CategoryID（FK→Category，可空）、Tags（M:N→Tag）、Status（draft/published/archived）、ViewCount、CreatedAt、UpdatedAt、DeletedAt（软删除）

#### 场景：Category 实体
- **GIVEN** 系统需要管理分类
- **WHEN** 定义 Category 模型
- **THEN** 必须包含：ID、Name、Slug（唯一）、Description、ParentID（自引用，可空）、SortOrder

#### 场景：Tag 实体
- **GIVEN** 系统需要管理标签
- **WHEN** 定义 Tag 模型
- **THEN** 必须包含：ID、Name、Slug（唯一）、Color（#hex）、CreatedAt、UpdatedAt、DeletedAt（软删除）

### 要求：实体关系规则
实体间的关系必须遵循以下约束。

#### 场景：文章归属用户
- **WHEN** 创建文章
- **THEN** AuthorID 必须指向有效用户，不可为空

#### 场景：文章软删除
- **WHEN** 删除文章
- **THEN** 使用 GORM 软删除（设置 DeletedAt），不做物理删除

#### 场景：分类层级
- **WHEN** 创建子分类
- **THEN** ParentID 必须指向已存在的 Category 或为空（顶级分类）

## 边界

- 范围内：实体字段定义、关系约束、软删除策略
- 范围外：API 端点（见 api-contract spec）、验证逻辑、业务流程

## 参考

- 契约文件：`contracts/api/openapi.yaml`
- BE 模型：`blog-BE/src/models/`
