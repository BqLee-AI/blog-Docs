# BqLee-AI 任务统筹工作流（产品经理视角）

> 核心原则：一次对话专注一件事，在最容易跑偏的地方收住，通过文档仓库回写同步进度
> 参考：AI Native Engineering Part 2 - 定盘

---

## 一、产品经理的推进节奏

### 不是什么
- 不是 PRD 一版定死
- 不是丢整个文档让 AI 自己找边界（容易越来越偏）
- 不是在一个对话里不断补充需求（上下文压缩导致目标丢失）

### 是什么
**持续决策 + 小批量推进 + 即时回写**

```
你（PM）持续判断方向
    │
    ▼
在文档仓库里开一个"聚焦讨论"（只解决一个问题）
    │
    ▼
AI Agent 帮你把讨论收成可执行的决策
    │
    ▼
决策冻结到文档仓库（变成 spec/issue/契约）
    │
    ▼
下发到 FE/BE（issue）
    │
    ▼
开发者执行 → PR 提供证据 → 你对照 spec 验收
    │
    ▼
合并后 AI 回写文档仓库（同步进度 + 发现问题 + 规划下一步）
    │
    ▼
准备下一个聚焦讨论
```

---

## 二、具体推进方式（按场景）

### 场景 1：要开始新的一轮开发

**你的动作：**
```markdown
在 blog-Docs 的 Discussions 或新建 Issue

标题：[PM] 下一个 batch 要做什么？文章CRUD 还是评论功能？

描述：
- MVP scope 里还没做的是：评论、分类标签、富文本
- 按价值优先级，应该先做评论（核心互动）
- 技术风险：评论涉及关联查询和级联删除

问题：
1. 下一 batch 确认是评论功能吗？
2. 评论要做多深？只做一阶评论（文章下评论）还是两阶（回复评论）？
3. 有没有我没想到的技术约束？
```

**AI Agent 的响应：**
```markdown
1. 回复你的问题：
   - 评论功能是 MVP scope 里的，优先级 ✓
   - 一阶 vs 两阶：一阶足够，两阶建议放到 Phase 2
   - 技术约束：需要考虑 Comment 模型的外键、级联删除策略、权限（只能删自己的）

2. 建议：先在文档仓库里发起一个"评论域 spec"的聚焦讨论
   - 冻结：只做一阶评论
   - 冻结：必须登录
   - 冻结：不做敏感词过滤（后续）
```

**你的判断：**
```
"OK，一阶评论，必须登录，先不做敏感词。让 AI 在 blog-Docs 里起草评论域的 spec。"
```

---

### 场景 2：要补充一个遗漏的决策

**你的动作：**
```markdown
在 blog-Docs 的 Discussions

标题：[PM] 文章状态要不要加草稿？

描述：
- 当前文章只有 published/unpublished
- 但用户提需求说写文章时想保存草稿

问题：
1. 草稿算 MVP 范围吗？还是 Phase 2？
2. 如果 Phase 1 就要做，那文章状态机要改成 draft/published 吗？
3. FE 要不要有个草稿列表页？
```

**AI Agent 的响应：**
```markdown
分析：
1. mvp-scope.md 里当前是"文章CRUD"，草稿属于 CRUD 的一部分
2. 但草稿会增加状态复杂度（要考虑草稿权限、列表过滤）
3. 如果现在加，会影响 Article 模型、列表 API、前端路由

建议：
- 把"文章草稿"当成一个独立的聚焦讨论
- 讨论清楚后，如果是 MVP，就更新 mvp-scope.md + 更新 domain-model spec
- 如果不是 MVP，就写到"Phase 2 规划"里
```

**你的判断：**
```
"草稿放到 Phase 2，Phase 1 先把 published 状态的 CRUD 做稳。让 AI 把这个决策回写到 mvp-scope.md 的'本次不做'清单里。"
```

---

### 场景 3：要解决一个卡住的问题

**你的动作：**
```markdown
在 blog-Docs 的 Discussions

标题：[PM Blocker] 评论接口响应太慢，要不要加缓存？

描述：
- BE 开发者反馈 POST /api/v1/articles/{id}/comments 太慢（2秒+）
- 原因是每次都要查文章是否存在 + 查用户是否存在 + 插入评论

问题：
1. 这是 PR 门槛问题还是发版门槛问题？
2. 缓存方案：Redis 缓存文章存在性？还是改查询逻辑？
3. 如果现在解决，会不会拖慢评论功能的交付？
```

**AI Agent 的响应：**
```markdown
1. 问题分级：这是 L2 BLOCKER（影响核心链路性能），需要紧急升级
2. 临时方案：改查询逻辑（避免两次 DB 查），不需要 Redis
3. 长期方案：Phase 2 再上 Redis 缓存
4. 建议：先创建一个"评论性能优化"的 issue，挂到现有的评论功能 issue 下作为 blocker
```

**你的判断：**
```
"OK，先改查询逻辑，不引 Redis。让 AI 创建优化 issue，阻塞评论功能 issue 的合并。"
```

---

## 三、Issue 应该长什么样（接任务者看的）

**核心原则：Issue 不是功能描述，是"可收敛的不确定性"。**

### Issue 模板（来自 blog-Docs/templates/issue-template.md）

```markdown
## Issue: [标题]

### 为什么现在做（Background）
- 这个问题阻塞了什么？
- 为什么现在必须解决？

### 目标
- 这次只解决什么（可验收表述）
- 不超过 3 条

### 非目标（Not Goals）
- 明确不做什么（防止范围漂移）

### 验收标准（必须在实现之前写好）
- [ ] 条件1（可验证）
- [ ] 条件2（可验证）
- [ ] 条件3（可验证）

### Harness 验证计划
- BE: go build ./... + go test ./... + smoke 命令
- FE: bun run build + 页面 smoke

### 输入真源（Input Truth Sources）
- Spec: blog-Docs/openspec/changes/xxx/specs/yyy/spec.md#scenario
- 契约: blog-Docs/contracts/openapi.yaml#operationId
- 上游 issue: #xxx（如果有）

### 实现约束
- 允许修改: 列出目录
- 禁止修改: 列出目录
- 技术约束: 列出特定要求

### 不确定性（Uncertainty）
- 这个 issue 要收敛的不确定性是什么？
- 收敛后，决策记录在哪？
```

### Issue 示例：评论功能（BE 子任务）

```markdown
## Issue: 实现文章评论 API（BE）

### 为什么现在做
- 评论是 MVP 的核心功能
- 被 mvp-scope.md 冻结为必须交付
- 不做评论，博客就只是内容展示，没有互动

### 目标
- 实现评论的创建、列表、删除 API
- 只支持一阶评论（直接回复文章）

### 非目标
- 不做回复评论（评论的评论）
- 不做敏感词过滤
- 不做图片评论

### 验收标准
- [ ] POST /api/v1/articles/{id}/comments 返回 201 + 评论对象
- [ ] GET /api/v1/articles/{id}/comments 返回 200 + 分页列表
- [ ] DELETE /api/v1/comments/{id} 返回 204（只能删自己的）
- [ ] 未认证返回 401
- [ ] 非本人删除返回 403

### Harness 验证计划
- BE: go test ./src/handler/comment_test.go
- Smoke:
  - curl -X POST /api/v1/articles/1/comments -H "Authorization: Bearer $TOKEN"
  - curl -X GET /api/v1/articles/1/comments
  - curl -X DELETE /api/v1/comments/1 -H "Authorization: Bearer $TOKEN"

### 输入真源
- Spec: blog-Docs/openspec/changes/comment-system/specs/comment-system/spec.md
- 场景: 创建评论 / 获取评论列表 / 删除评论
- 契约: blog-Docs/contracts/openapi.yaml#postComments / getComments / deleteComment

### 实现约束
- 允许修改: src/handler/comment.go, src/service/comment.go, src/dao/comment.go, src/models/comment.go
- 禁止修改: src/handler/article.go（除非必须改），任何涉及草稿状态的代码
- 技术约束: Comment 模型必须有 article_id 外键，删除评论不能级联删除文章

### 不确定性
- 删除评论后，文章的 comment_count 怎么更新？（收敛后记录到 domain-model spec）
```

**关键点：Issue 里有明确的"输入真源"，接任务者打开 Issue 就知道去读哪些文档，不需要猜。**

---

## 四、PR 应该长什么样（你验收时看的）

### PR 模板（来自 blog-Docs/templates/pr-template.md）

```markdown
## PR: [标题]

### 关联信息
- Issue: #xxx
- Spec: blog-Docs/openspec/changes/xxx/specs/yyy/spec.md#scenario
- 契约: blog-Docs/contracts/openapi.yaml#operationId（或"无变更"）

### 变更说明
- 简要描述做了什么（2-3 行）

### 冻结边界检查
- [ ] 未修改授权范围外的文件
- [ ] 未引入 Issue 未要求的依赖
- [ ] 未实现 Issue 的"非目标"

### Harness 验证证据（四要素）

| 检查项 | 命令 | 结果 | 证据 | 结论 |
|--------|------|------|------|------|
| Build | go build ./... | PASS | CI #45 | ✓ 编译通过 |
| Test | go test ./... | PASS | CI #45, 12/12 passed | ✓ 单测覆盖主路径 |
| Smoke: 创建评论 | curl -X POST ... | PASS | 返回 201 + {id, content, created_at} | ✓ 验收标准达成 |
| Smoke: 未认证 | curl -X POST ... | PASS | 返回 401 | ✓ 验收标准达成 |

### Spec 场景覆盖
- [ ] Spec scenario "创建评论" 覆盖 ✓
- [ ] Spec scenario "获取评论列表" 覆盖 ✓
- [ ] Spec scenario "删除评论" 覆盖 ✓

### 契约一致性检查
- [ ] POST 响应体结构符合 openapi.yaml#postComments ✓
- [ ] 响应状态码符合契约 ✓

### 风险与回滚
- 风险：新增了 Comment 模型的 migration
- 回滚：删除 migration 文件，重启服务
- 风险2：删除评论时可能并发问题
- 回滚：先锁行再删（已在代码中实现）

### 补充提醒（如果有的话）
- [ ] 发现 openapi.yaml 的分页参数描述不够清晰
- [ ] 建议更新 mvp-scope.md 的"本次不做"清单（发现误将"回复评论"当成了非目标）
```

**你验收时检查什么：**
```
1. Issue/Spec/契约 链接都有吗？（追溯链完整）
2. Harness 证据是四要素格式吗？
3. CI 是绿色的吗？
4. Spec scenario 全覆盖了吗？
5. 有没有改到不该改的文件？
6. 风险与回滚清楚吗？
7. 补充提醒有没有？（这是关键，发现新边界要记录）
```

---

## 五、合并后如何回写文档仓库

**你的动作：**
```
你合并 PR 后，告诉 AI Agent：

"这个 issue 完成了，帮我：
1. 在 issue 中标记完成，附上 PR 链接
2. 如果 PR 里写了'补充提醒'，帮我更新对应的文档
3. 更新 progress-sync-protocol.md 的本周记录
4. 检查有没有需要规划下一个 issue 的，给我建议"
```

**AI Agent 的自动化动作：**
```markdown
1. 回写 Issue：
   Issue #xxx: 完成于 2026-04-19，PR #45
   
2. 更新补充提醒：
   如果 PR 里说"openapi.yaml 的分页参数描述不够清晰"，
   AI 会直接更新 openapi.yaml 的注释，并提交 PR 到 blog-Docs

3. 更新 progress-sync：
   docs/workflow/progress-sync-protocol.md
   
   ## 2026-04-19 周同步
   | Issue | 状态 | 阻塞 | 风险 |
   |-------|------|------|------|
   | #xxx 评论功能 BE | done | 无 | 无 |
   | #xxx+1 评论功能 FE | in_progress | 无 | 无 |

4. 规划下一个 issue：
   "当前评论功能的 API 已完成，FE 还在开发。
   建议下一个 issue：实现评论功能的 FE 对接（前端页面 + api client）
   或者：如果 FE 也完成了，建议进入下一个 batch：分类标签功能"
```

**为什么必须回写？**
```
1. 同步所有人的进度：你、BE Agent、FE Agent 看到同一份运行态
2. 及时发现问题：如果 PR 里写了补充提醒，AI 直接去修，不会遗落
3. 规划下一步：不是丢一整个文档让 AI 自己找，而是基于当前完成度，精准建议下一步
4. 新 agent 接手友好：打开 progress-sync 就知道当前状态，不需要翻几十个 issue
```

---

## 六、文档仓库的权限与协作方式

### 你的原则（已确认）

| 操作 | 谁可以做 | 为什么 |
|------|----------|------|
| 创建 issue | 任何人（包括 AI Agent） | 让任何人都可以提问题 |
| 提 PR 到 blog-Docs | 任何人（包括 AI Agent） | 让 AI 可以自动更新 spec/契约 |
| **合并 blog-Docs 的 PR** | **只有你（PM）** | 控制面是治理真源，不能随意改 |
| 创建讨论 | 任何人 | 让所有人可以讨论方向 |
| 回写 progress-sync | AI Agent（你触发后） | 自动化记录，但由你触发 |

### 其他人怎么参与

```
开发者 / AI Agent 想贡献到文档仓库：
    方式1：在 Discussion 里讨论方向
    方式2：提 PR（但必须由你审核合并）
    方式3：在 FE/BE 的 PR 里写"补充提醒"，由 AI 回写到 blog-Docs

你审核 blog-Docs PR 时检查：
  - 这个修改有没有偏离 mvp-scope？
  - 这个修改有没有违反已冻结的 spec？
  - 这个修改会不会导致 FE/BE 不一致？
```

---

## 七、与 AI Native Engineering Part 2 的对齐

| Part 2 概念 | 对应到你的工作流 |
|-------------|-----------------|
| **拿到 PRD 后先让 AI 暴露问题，不是暴露代码** | 你在 Discussions 里提出问题，AI 帮你把讨论收成决策，而不是直接让 BE 写代码 |
| **需求收敛的第一个正式产物是项目章程，不是施工图** | blog-Docs 的 mvp-scope.md + 全局 spec 就是项目章程，Issue 才是施工图 |
| **task 的职责不是排施工顺序，而是孵化 issue** | 你通过聚焦讨论冻结决策，AI 从决策里拆出具体 issue，每个 issue 是"可收敛的不确定性" |
| **验收口径必须在 issue 之前** | Issue 模板里有"验收标准"段，必须填写才能标记 Ready |
| **Harness 跟着项目阶段一起长** | Phase 0 只有工程秩序，Phase 1 加契约验证，Phase 2 加交付闭环 |
| **IssueDriven 和 SpecDriven 前后咬合** | Global Spec → Issue Spec → 实现，每一步都有追溯链 |
| **Spec 先行** | Issue 必须关联 Spec，PR 必须证明覆盖了 Spec scenario |
| **把判断外置成控制面资产** | mvp-scope / domain-model / api-contract 都是外置的判断，不在人脑里 |

---

## 八、一个完整的推进周期示例

### Day 1: 你发起聚焦讨论

```
你：在 blog-Docs Discussions 开讨论"下一个 batch 做什么？"
AI：分析 MVP scope + 技术风险，建议先做评论功能
你：确认"先做评论，一阶，必须登录"
AI：在 blog-Docs 起草 comment-system 的 spec（proposal/design/specs/tasks）
你：审核 blog-Docs PR，合并
```

### Day 2-3: Issue 拆分与下发

```
AI：从 tasks.md 拆出 3 个 issue
  - Issue #1: 实现 Comment 模型 + migration（BE）
  - Issue #2: 实现评论 API handler + service（BE）
  - Issue #3: 实现评论 FE 页面 + API client（FE）

每个 issue 都有：
  - 为什么现在做（背景）
  - 验收标准（从 spec scenario 来）
  - 输入真源（spec 链接 + 契约链接）
  - 实现约束（允许/禁止修改的目录）
```

### Day 4-7: 开发者执行

```
BE Agent 接到 Issue #2：
  - 读 spec 的 3 个 scenario
  - 读 openapi.yaml 的 4 个端点
  - 读 AGENTS.md 的约束
  - 实现 + 测试 + smoke
  - 提 PR（按 PR 模板填写 Harness 证据）

FE Agent 接到 Issue #3：
  - 读 spec 的场景（虽然 FE 的验收是"页面可交互"）
  - 读 openapi.yaml 的接口定义
  - 替换 mock 为真实 API 调用
  - 提 PR（按 PR 模板填写 Harness 证据）
```

### Day 8: 你验收与合并

```
你审核 BE PR：
  - Issue 链接有吗 ✓
  - Spec 链接有吗 ✓
  - 契约一致性检查有吗 ✓
  - Harness 证据四要素吗 ✓
  - Scenario 全覆盖了吗 ✓
  - 没改不该改的文件吗 ✓
  - 风险与回滚清楚吗 ✓
  合并

你审核 FE PR：
  同上，但多了"页面 smoke"检查
  合并
```

### Day 9: AI 回写文档仓库

```
你告诉 AI："这两个 issue 完成了，帮我回写"

AI 做了：
  1. Issue #2/#3 标记 done
  2. progress-sync 更新本周记录
  3. 发现 FE PR 的补充提醒："openapi.yaml 的分页参数描述不够清晰"
  4. AI 自动更新 openapi.yaml 并提 PR 到 blog-Docs
  5. AI 建议："当前评论功能已完成，建议下一个 batch：分类标签功能"
```

### Day 10: 下一轮讨论

```
你看到 AI 的建议，决定"分类标签功能"
  → 回到 Day 1，开启新一轮聚焦讨论
```

---

## 九、关键点总结

1. **一次对话专注一件事**：每个聚焦讨论只解决一个问题，不要在一个对话里补充太多
2. **在最容易跑偏的地方收住**：Issue 的"非目标"、"实现约束"就是收住点
3. **不是丢整个文档让 AI 找**：Issue 里明确写了"输入真源"，接任务者打开就知道去读哪些文档
4. **PR 必须对照 spec 验收**：不是凭感觉"看起来可以"，而是逐条检查 Scenario 覆盖
5. **合并后必须回写**：progress-sync 是运行态真源，新 agent 接手先看这个
6. **补充提醒是关键**：开发过程中发现的新边界，必须在 PR 里写清楚，由 AI 回写，不会遗落
7. **你只操心验收和边界**：怎么写代码、用什么算法，在 AGENTS.md 里约束，你不管
8. **文档仓库只让你合并**：其他人可以讨论、可以提 PR，但控制面的最终决策只有你能合并

---

## 十、开放问题（待辩论细化）

1. **聚焦讨论用哪个工具**：GitHub Discussions？还是直接在 blog-Docs 里开 Issue？
2. **Issue 是否需要 L1 OpenSpec change**：Phase 0 简化一下，直接用 PR 流程？
3. **PR 的"补充提醒"格式**：要不要强制？还是可选？
4. **progress-sync 的更新频率**：每周一次？还是每次合并就更新？
