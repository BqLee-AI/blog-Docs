# 工作流端到端示例：评论功能

> 用途：展示控制面工作流如何从"PM 发起讨论"到"PR 合并回写"完整跑一遍。  
> 这是一个虚构场景，用于说明流程，不作为实际业务需求。

---

## 第 1 步：PM 发起聚焦讨论

**在哪做**：GitHub Discussions（blog-Docs 仓库）

```
标题：[PM] 下一个 batch 是否做评论功能？

内容：
- MVP scope 里还没做的是：评论、分类标签、富文本
- 按用户价值优先级，评论（核心互动）应排在分类标签前面
- 技术风险：评论涉及关联查询和级联删除

问题：
1. 确认下一个 batch 是评论功能吗？
2. 一阶评论（回复文章）还是两阶（回复评论）？
3. 有没有技术约束我没想到的？
```

---

## 第 2 步：讨论收成决策

**AI Agent 帮你做的**：

```
回复：
1. 评论在 MVP scope 内，优先级 ✓
2. 建议：先做一阶，两阶放到 Phase 2
3. 技术约束：
   - Comment 需要 article_id 外键
   - 删除文章要级联处理评论
   - 需要确认"只能删自己的评论"的权限逻辑

建议决策：
- 做一阶评论，必须登录，不做敏感词过滤
```

**PM 确认**：

```
"OK。一阶评论，必须登录，先不做敏感词。帮我起草 spec。"
```

---

## 第 3 步：AI 起草 Spec 并提交 blog-Docs PR

**AI Agent 做的**：

在 blog-Docs 创建 OpenSpec change：

```
openspec/changes/comment-system/
├── proposal.md    ← "只做一阶评论，不做回复、不做敏感词"
├── design.md      ← Comment 模型 + 4 个 API 端点
├── specs/comment-system/spec.md  ← 5 个可验证场景
└── tasks.md       ← 6 个任务 + 依赖关系
```

同时更新契约：

```
contracts/openapi.yaml 新增 4 个端点：
- POST /api/v1/articles/{id}/comments
- GET /api/v1/articles/{id}/comments
- DELETE /api/v1/comments/{id}
- GET /api/v1/comments/{id}（单条详情，可选）
```

提交 PR 到 blog-Docs。

---

## 第 4 步：PM 审核 blog-Docs PR

**PM 看**：

```
proposal.md："只做一阶评论，不做回复、不做审核" → ✓
spec.md：5 个 Scenario 覆盖了创建/列表/删除/未认证/非本人删除 → ✓
openapi.yaml：4 个端点定义清晰 → ✓
tasks.md：BE 先行，FE 后行，有依赖关系 → ✓
```

合并 PR。

---

## 第 5 步：更新运行层

**AI Agent 做**（PM 触发后）：

`docs/runtime/issue-dispatch.md`：

```
| Priority | Repo | Issue | Why now | Scope | DoD | Harness | Status |
|----------|------|-------|---------|-------|-----|---------|--------|
| P0 | blog-BE | #101 | 评论是 MVP 核心互动 | 创建/列表/删除 API | spec scenario 1-5 | go test + smoke | ready |
| P0 | blog-FE | #102 | 评论是 MVP 核心互动 | 评论组件+API对接 | spec scenario 1-5 | bun build + 页面smoke | blocked(#101) |
```

`docs/runtime/phase-current.md`：

```
阶段名称：Phase 1 - 主干开发
状态：active
本阶段目标：完成评论功能 FE/BE 全链路
```

同时在 blog-BE 和 blog-FE 仓库创建对应 Issue（内容引用 spec + openapi.yaml）。

---

## 第 6 步：BE 开发者执行

**BE Agent 接到 Issue #101 后**：

```
读什么（Issue 里写明了）：
- Spec: openspec/changes/comment-system/specs/comment-system/spec.md
- 契约: contracts/openapi.yaml#postComments
- AGENTS.md: blog-BE/AGENTS.md

做什么：
1. 创建 Comment 模型（article_id 外键 + user_id）
2. 实现 handler/service/dao
3. 写测试
4. 跑 smoke

不做什么（Issue 里写明了）：
- 不做回复评论
- 不做敏感词过滤
- 不改 Article 模型（除非必须加级联删除）
```

提交 PR：

```markdown
## PR: 实现评论创建/列表/删除 API

### 关联真源
- Issue: blog-BE#101
- Spec: openspec/changes/comment-system/specs/comment-system/spec.md
- Contract: contracts/openapi.yaml#postComments

### Harness 验证证据
| 检查项 | 命令 | 结果 | 证据 | 结论 |
|--------|------|------|------|------|
| Build | go build ./... | PASS | CI #78 | 编译通过 |
| Test | go test ./... | PASS | CI #78, 12/12 | 单测覆盖主路径 |
| Smoke: 创建评论 | curl -X POST /api/v1/articles/1/comments | PASS | 201 + {id,content} | 验收达成 |
| Smoke: 未认证 | curl -X POST ... (无token) | PASS | 401 | 验收达成 |
| Smoke: 非本人删除 | curl -X DELETE ... (他人token) | PASS | 403 | 验收达成 |

### Spec 场景覆盖
- [x] 场景"创建评论" ✓
- [x] 场景"获取评论列表" ✓
- [x] 场景"删除自己的评论" ✓
- [x] 场景"未认证拒绝" ✓
- [x] 场景"非本人删除拒绝" ✓

### 风险与回滚
- 风险：新增 comment 表 migration
- 回滚：删除 migration 文件，重启服务

### 补充提醒
- 发现 openapi.yaml 的分页参数描述不够清晰，建议后续修正
```

---

## 第 7 步：PM 审核 BE PR

**PM 逐条检查**：

```
1. Issue/Spec/契约链接都有吗？ → ✓ 追溯链完整
2. Harness 证据是四要素吗？   → ✓ 命令/结果/证据/结论
3. CI 绿色吗？                → ✓ CI #78 passed
4. Spec 场景全覆盖了吗？      → ✓ 5/5
5. 没改不该改的文件吗？       → ✓ 未动 Article 模型
6. 风险回滚清楚吗？           → ✓
7. 补充提醒有吗？             → ✓ "openapi 分页参数描述需修正"
```

合并。

---

## 第 8 步：PM 触发 AI 回写

**PM 说**："BE 评论 PR 合并了，帮我回写。"

**AI Agent 做**：

1. 标记 Issue #101 为 done
2. 更新 `docs/runtime/issue-dispatch.md`（#101 → done，#102 → ready）
3. 发现 PR 里的补充提醒，提 PR 修正 `contracts/openapi.yaml` 的分页描述
4. 更新 `docs/runtime/risk-register.md`（如需）
5. 建议："FE Issue #102 已解除阻塞，建议开始。"

---

## 第 9 步：FE 开发者执行（同理）

流程与 BE 相同：读 Issue → 读 spec → 读 openapi.yaml → 实现 → 提 PR → PM 审核 → 合并 → 回写。

---

## 第 10 步：功能完成后

**AI Agent 汇总**：

```
docs/runtime/phase-current.md 更新：
  评论功能 FE/BE 全链路完成 ✓

docs/runtime/issue-dispatch.md 更新：
  #101 done / #102 done

建议下一步：
  "当前 batch 已完成。建议下一个 batch：分类标签功能。
   是否在 Discussions 开一个新的聚焦讨论？"
```

PM 确认 → 回到第 1 步，开启下一轮。

---

## 整个流程中每个文件的职责

| 文件 | 在流程中的角色 |
|------|--------------|
| `docs/control-plane/control-plane-handbook.md` | 红线：定义边界不能随便改 |
| `docs/control-plane/mvp-scope.md` | 范围冻结：评论在 MVP 内 |
| `docs/control-plane/ai-native-governance.md` | 治理总则：先 spec 后代码 |
| `docs/workflow/spec-issue-test-flow.md` | 流程：Spec→Issue→Test 五步 |
| `docs/workflow/change-intake-and-escalation.md` | 分级：L0/L1/L2 怎么判定 |
| `docs/workflow/pr-merge-gate-spec.md` | 门禁：PR 必须有什么才能合 |
| `docs/harness/harness-closed-loop.md` | 验证：Harness 四要素标准 |
| `docs/harness/harness-gate-baseline.md` | 基线：合并门禁 + 发版门禁 |
| `docs/standards/agents-md-standard.md` | 标准：AGENTS.md 怎么写 |
| `docs/runtime/phase-current.md` | **运行态**：当前在哪个阶段 |
| `docs/runtime/issue-dispatch.md` | **运行态**：本阶段发了哪些 Issue |
| `docs/runtime/risk-register.md` | **运行态**：当前风险台账 |
| `contracts/openapi.yaml` | 契约真源：FE/BE 共同消费 |
| `openspec/changes/<id>/` | 变更过程：spec 从这产出 |
| `templates/*` | 模板：Issue/PR/Spec/Checklist |
