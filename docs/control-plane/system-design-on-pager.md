# 系统一页设计（System Design on a Pager）

## 主资产

前后端分离的个人博客内容管理与展示系统：认证、文章 CRUD、评论、分类标签。

## 不是

多租户 SaaS、CMS 框架、高并发系统、移动端应用。

## 分层与官方路径

```
L3 展示层   blog-FE (React 19 + TypeScript + Vite + Bun)
            官方路径：src/api/ → src/store/ → src/features/ → src/pages/
            禁止：组件直连后端、散落业务逻辑

L2 接口层   API Contract (OpenAPI 3.0, /api/v1/*)
            官方路径：contracts/openapi.yaml 是唯一真源
            禁止：口头约定接口、FE/BE 各自定义

L1 服务层   blog-BE (Go + Gin + GORM + PostgreSQL)
            官方路径：handler → service → models(含DAO) → dao(连接)
            禁止：handler 直连数据库、跨层调用

L0 治理层   blog-Docs (控制面)
            官方路径：OpenSpec change → spec → issue → PR → evidence → archive
            禁止：绕过 spec 直接改 FE/BE、口头变更契约
```

## 三条关键边界

1. **跨层直连**：FE 禁止绕过 API 契约直接定义数据结构，BE 禁止绕过 handler 层在 router 中写业务逻辑
2. **控制面绕行**：影响 scope/contract/change 的变更禁止不经 OpenSpec 直接合入
3. **无证据合并**：无 Harness 验证证据的 PR 禁止合并

## 已定决策

- 后端：Go/Gin/GORM/PostgreSQL，JWT RSA256 认证，bcrypt 密码
- 前端：React 19/Zustand/ShadcnUI/Tailwind，Bun 构建
- 发版：从 develop 冻结 commit 打 tag，不走 develop→main 合并 PR
- 认证：Bearer Token，access + refresh pair
- API：RESTful，统一响应 `{ message, data, requestId, code }`

## 待定问题

- 富文本编辑器选型
- 图片存储方案
- 验证码存储（内存 vs Redis）
- 日志方案
- 配置管理
