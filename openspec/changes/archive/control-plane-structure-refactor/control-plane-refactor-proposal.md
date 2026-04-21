# BqLee-AI 项目控制面重构方案

> 状态：已定稿
> 日期：2026-04-19（定稿 2026-04-21）
> 参与视角：产品经理 / 开发工程师 / 架构师
> 参考文献：《AI 原生工程》Part 2 · 定盘（ai.mengchen.icu）

---

## 0. 核心诊断

blog-Docs 控制面当前状态：**框架已建，有壳无魂。**

| 维度 | 现状 | 问题 |
|------|------|------|
| mvp-scope.md | 空模板，无冻结内容 | 范围未定，开发无边界 |
| API 契约 | 散落在 Project Plan 的自然语言中 | 不可被测试消费，FE 只能猜 |
| blog-BE AGENTS.md | 不存在 | 违反自身控制面的 `fe-be-adoption-gate.md` 要求 |
| CI | FE 零 CI，BE 只有 deploy 无 PR gate | PR 无门禁，合并无底线 |
| Harness | 四层定义完整，但零落地 | 定义了"无证据不合并"，实际全靠人记 |
| GitHub 模板 | 存在于 blog-Docs/templates/ 但未部署 | 创建 Issue/PR 时无规范提示 |
| 旧文档 | workspace 根目录散落 3 个文档 | 不在控制面管理内 |
| OpenSpec | 框架就位，仅跑过 1 次归档 change | 从未真正运营 |

**一句话结论**：当前最缺的不是更多治理文档，而是把已有框架填入真实决策。

---

## 1. blog-Docs 产品定位

### 1.1 主资产

| 是 | 不是 |
|----|------|
| 冻结的决策（MVP 范围、阶段边界、验收口径） | "建议结构"、"写法要求"等模板说明 |
| 可执行的契约（OpenAPI YAML，可被测试消费） | 自然语言描述"响应格式是 `{code, message, data}`" |
| 运行态的真源（当前冻结到哪条契约、哪些 issue 在飞） | 空 dashboard 模板、空 scope 模板 |

**一句话**：blog-Docs 的主资产是"判断的外置"，不是"知识的堆放"。

### 1.2 服务对象

| 消费者 | 需要什么 | blog-Docs 怎么给 |
|--------|---------|-----------------|
| 人（PM/架构师） | 冻结范围、风险台账、推进节奏 | dashboard（运行态）、scope（冻结态） |
| AI Agent（BE） | AGENTS.md 约束 + OpenAPI 契约 | OpenAPI YAML（机器可读），不是自然语言 |
| AI Agent（FE） | AGENTS.md + API 契约 + mock 约定 | 同上 |

### 1.3 最大产品理解偏差

把"定框架"等同于"已治理"。

- mvp-scope.md 是模板不是冻结内容
- 契约写在自然语言里不可被测试消费
- OpenSpec 框架建了但没有实际变更走这个流程
- Harness 定义了四层但最底层的"编译通过"都没自动化

---

## 2. 控制面整体架构

### 2.1 目录结构

```
blog-Docs/
├── AGENTS.md                        # Agent 行为编排（入口）
├── openspec/
│   ├── config.yaml                  # 全局 schema + context + rules
│   ├── specs/                       # L0 冻结 spec（长期有效）
│   │   ├── control-plane-docs/      #   治理文档稳定性（已有）
│   │   ├── spec-driven-governance/  #   OpenSpec 变更规则（已有）
│   │   ├── api-contract/            # ★ API 契约域（新增）
│   │   ├── domain-model/            # ★ 领域模型域（新增）
│   │   └── harness-baseline/        # ★ 验证基线域（新增）
│   └── changes/
│       ├── active/                  #   进行中的 change
│       └── archive/                 #   已归档的 change
├── docs/
│   ├── control-plane/
│   │   ├── control-plane-handbook.md
│   │   ├── ai-native-governance.md
│   │   ├── mvp-scope.md             # ★ 待填充真实冻结范围
│   │   └── system-design-on-pager.md # ★ 新增
│   ├── workflow/                 # （保持现有）
│   ├── harness/
│   │   ├── harness-closed-loop.md
│   │   ├── harness-gate-baseline.md
│   │   └── harness-phase-plan.md    # ★ 新增
│   └── standards/
│       ├── agents-md-standard.md
│       ├── fe-be-adoption-gate.md
│       └── spec-writing-standard.md # ★ 新增
├── contracts/                       # ★ 新增：契约真源
│   └── api/openapi.yaml             # OpenAPI 3.0 规范
├── templates/                       # （保持，需部署到 FE/BE 的 .github/）
│   ├── spec-template.md
│   ├── issue-template.md
│   ├── pr-template.md
│   ├── harness-checklist.md
│   └── system-design-on-pager.md    # ★ 新增
└── .github/workflows/
    └── spec-lint.yml                # ★ 新增：控制面 CI
```

### 2.2 模块依赖关系

```
AGENTS.md（流程编排入口）
    │
    ├──▶ 01-control-plane/（治理规则层，定义冻结边界）
    │         │
    │         ▼
    ├──▶ 02-workflow/（工作流执行层，Spec→Issue→PR）
    │         │
    │         ├──▶ templates/（执行模板）
    │         └──▶ contracts/（契约真源，FE/BE 共同消费）
    │                   │
    │                   ▼
    └──▶ 03-harness/（验证闭环层，证据→门禁）
              │
              └──▶ 04-standards/（编写标准）
```

### 2.3 与 FE/BE 的闭环机制

```
blog-Docs                         blog-FE / blog-BE
┌──────────────┐  规则下发         ┌──────────────┐
│ Spec/契约    │─────────────────▶│ AGENTS.md    │
│ 边界/门禁    │  (引用+链接)     │ 读取控制面    │
└──────┬───────┘                  │ 规范执行     │
       │                          └──────┬───────┘
       │                                 │
       │  证据回写                        │ PR + Harness 证据
       │◀─────────────────────────────────│
       │                                 │
┌──────▼───────┐  状态同步        ┌──────▼───────┐
│ progress-    │◀─────────────────│ Issue 状态    │
│ sync / dash  │                  │ 回写结论      │
└──────────────┘                  └──────────────┘

回环：Spec 下发 → 执行 → 证据回写 → Spec/契约 演进 → 再下发
```

---

## 3. 全局 Spec → Issue → 子 Spec 三层结构

### 3.1 层级定义

```
L0 Global Spec（项目级，长期冻结）
│  openspec/specs/<domain>/spec.md
│  例: api-contract/spec.md, auth-flow/spec.md, domain-model/spec.md
│
├──▶ L1 Feature Spec（功能级，随 OpenSpec change 创建）
│     openspec/changes/<change-id>/specs/<feature>/spec.md
│     例: openspec/changes/article-crud/specs/article-crud/spec.md
│
└──▶ L2 Issue Spec（任务级，嵌入 GitHub Issue body）
      Issue body 中引用 L1 的具体 scenario + 验收标准
```

### 3.2 Global Spec 模板

```markdown
# <domain> Specification

## Purpose
一段话说明这个 spec 管什么、不管什么。

## Requirements

### Requirement: <可验证的行为要求标题>
<要求描述，使用 MUST/SHALL/MUST NOT 语义>

#### Scenario: <场景名>
- **GIVEN** <前置条件>
- **WHEN** <触发动作>
- **THEN** <期望结果>

## Boundaries
- In scope: ...
- Out of scope: ...

## References
- 上游 spec:
- 受影响的仓库:
- 契约文件: contracts/openapi.yaml#<operationId>
```

### 3.3 从 Global Spec 孵化 Issue 的流程

```
1. 你决定"接下来做什么"

2. AI Agent 检查 mvp-scope.md，确认在冻结范围内
   从全局 Spec 取出对应域的 Requirements

3. 按"可收敛的不确定性"拆 Issue
   每个 Issue 满足三件事：
   - 单独拿出来可以在一轮内把边界收住
   - 一旦不先收住，后续很多实现会一起漂
   - 收住之后，能为后续一批实现提供共同前提

4. Issue 的验收口径从 Global Spec 的 Scenario 直接映射
   Spec 说"密码错误返回 401"
   → Issue 验收就是"密码错误返回 401"

5. Issue 模板（精简版）
```

```markdown
## Issue: [title]

**Spec**: links to specs/xxx.md#scenario
**Contract**: links to contracts/openapi.yaml#endpoint
**Type**: feat | fix | chore

### 验收口径（必须在实现之前写好）
- [ ] 可验证的条件 1
- [ ] 可验证的条件 2

### Harness
- BE: go build ./... + go test ./...
- FE: bun run build

### 不确定性
- 这个 issue 要收敛的不确定性是什么？
```

### 3.4 Issue 产出自己的 Spec

**什么时候需要 Issue 级 Spec？**

| 变更级别 | 定义 | 是否需要 Issue 级 Spec |
|---------|------|----------------------|
| L0 直接执行 | 在范围内，无新功能，无契约变更 | 不需要，按 Global Spec 做 |
| L1 OpenSpec | 新功能、契约变更、跨仓库依赖 | 需要，走 OpenSpec change |
| L2 BLOCKER | 边界冲突、安全风险、P0 阻塞 | 紧急升级，先解决再补 spec |

**Issue 级 Spec 的产出流程：**

```
Issue 被判定为 L1
    │
    ▼
在 blog-Docs 中创建 OpenSpec change:
  openspec/changes/<change-id>/
  ├── proposal.md    ← 为什么做，影响面（2-3 段话）
  ├── design.md      ← 怎么做：数据模型、端点、状态流转
  ├── specs/
  │   └── <feature>/spec.md  ← 行为要求（从 Global Spec 细化）
  └── tasks.md       ← 拆成具体任务 + 依赖关系

proposal 写的是决策不是施工图：
- 这轮只做 X，不做 Y
- 状态只有 A/B，不做 C
- 分页用 offset，不做 cursor

你审核通过后，开发就按这个来。
AI Agent 不能在实现中偷偷加 Y/C/cursor。
```

### 3.5 Spec 之间的追溯关系

```
追溯链：
  GitHub Issue
    → .spec 引用 → openspec/changes/<id>/specs/<f>/spec.md
      → .requirement 引用 → openspec/specs/<domain>/spec.md#Requirement
        → .contract 引用 → contracts/openapi.yaml
          → .implementation → FE/BE 代码文件
```

---

## 4. 日常运营工作流

### 4.1 你的日常：范围决策

```
你决定"接下来做什么"
    │
    ▼
AI Agent：
1. 检查 mvp-scope.md，确认在冻结范围内
2. 从 Global Spec 取出对应域的 Requirements
3. 起草 Issue + OpenSpec change（proposal/design/specs/tasks）
4. 提交 PR 到 blog-Docs
    │
    ▼
你审核 blog-Docs PR：
- 范围对不对？
- 验收口径够不够具体？
- 有没有偷偷带进不该做的东西？
    │
    ▼
合并 → Issue 可以开工了
```

### 4.2 你的日常：PR 验收

开发者（或 AI Agent）提交 PR 时需要按模板填写：

```markdown
## PR: [标题]

### 关联
- Issue: #xxx
- Spec: openspec/changes/<id>/specs/<f>/spec.md
- 契约: contracts/openapi.yaml#<operationId>

### Harness 证据
| 检查项 | 命令 | 结果 | 证据 |
|--------|------|------|------|
| Build | go build ./... | PASS | CI #45 |
| Test | go test ./... | PASS | CI #45, 8/8 passed |
| Smoke: xxx | curl -X POST ... | PASS | 返回 201 + 对象 |

### 风险与回滚
- 风险：...
- 回滚：...
```

**你验收时检查什么：**

```
1. Issue/Spec/契约 链接都有吗？（追溯链完整）
2. Harness 证据是四要素格式吗？（命令/结果/证据/结论）
3. CI 是绿色的吗？（自动化门禁）
4. 有没有改到不该改的文件？（边界检查）
5. Spec scenario 全覆盖了吗？（验收口径检查）
```

### 4.3 你的日常：经验回写

```
Issue 完成后 AI Agent 自动：
1. 在 Issue 中记录完成结论
2. 如果发现新的边界/规则，建议更新 AGENTS.md 或 spec
3. 如果契约有微调，建议更新 openapi.yaml
4. 更新 progress-sync 记录
    │
    ▼
你审核回写内容是否合理
```

### 4.4 开发者的工作流

```
1. 接到 Issue
   ├── 读 Issue 中的 Spec 引用和验收口径
   ├── 读 AGENTS.md 了解约束
   └── 读 contracts/openapi.yaml 了解接口定义

2. 开发
   ├── 从 develop 拉分支：feat/BE-001-article-crud
   ├── 按 Spec + 契约实现
   └── 不改 Spec 范围外的文件

3. 提 PR
   ├── 确保本地 CI 通过
   ├── 按 PR 模板填写（Issue/Spec/契约链接 + Harness 证据）
   └── 等待 Review

4. Review 后修改 → 合并
```

### 4.5 完整例子：评论功能

```
第 1 步：你决定做评论
  你："下一个做评论功能"
  AI："检查 mvp-scope.md... 评论在 MVP 范围内 ✓"
  AI："检查 api-contract spec... 还没有评论的 scenario，需要先补"

第 2 步：AI 起草 OpenSpec change
  blog-Docs PR:
    openspec/changes/comment-system/
    ├── proposal.md  ← 只做文章评论，不做回复、不做审核
    ├── design.md    ← Comment 模型 + 4 个 API 端点
    ├── specs/comment-system/spec.md ← 5 个 scenario
    └── tasks.md     ← 6 个任务，有依赖关系
    + contracts/openapi.yaml 更新（新增 4 个端点）

第 3 步：你审核 blog-Docs PR
  看 proposal："不做回复 OK，不做审核 OK"
  看 spec scenario："5 个场景覆盖了，验收口径够具体"
  合并 → Issue 被创建

第 4 步：BE 开发者拿到 Issue
  读 spec → 读 openapi.yaml → 实现 handler/service/dao
  提 PR + Harness 证据

第 5 步：你验收 BE PR
  CI green ✓、Scenario 覆盖 ✓、证据四要素 ✓
  合并

第 6 步：FE 开发者拿到 Issue
  读 spec → 读 openapi.yaml → 替换 mock 为真实 API
  提 PR + Harness 证据

第 7 步：你验收 FE PR
  同上 + 页面可访问 ✓
  合并

第 8 步：经验回写
  AI："评论功能完成，发现 openapi.yaml 的分页参数描述不够清晰"
  你："OK，补一下"
```

---

## 5. 人与 AI Agent 职责分工

| 职责 | 人（xxs588） | AI Agent |
|------|-------------|----------|
| 决策 | 范围冻结、架构取舍、优先级 | 分析、方案对比、风险提示 |
| Spec 创建 | 审核和批准 | 起草 proposal/design/specs/tasks |
| Issue 拆解 | 确认和分配 | 从 tasks.md 拆解到 FE/BE issue |
| 代码实现 | Review + 关键模块 | 80% 的代码编写 |
| Harness 验证 | 审核证据 | 执行 CI、跑 smoke、生成证据摘要 |
| 文档回写 | 审核 | 自动更新 progress-sync、归档 change |
| 漂移检测 | 月度审计 | 每次 PR 自动检查是否偏离 AGENTS.md |

**核心原则**：
- 人定边界，AI 执行
- 人审证据，AI 采证据
- 人管控制面，AI 管执行面

---

## 6. 分阶段工作流

### Phase 0: 定盘（当前 → 2 周）

**目标**：消除 CI 断层、补齐缺失 AGENTS.md、冻结 MVP 范围、让控制面真正转起来

| 优先级 | 任务 | 仓库 | 预估 |
|--------|------|------|------|
| P0 | 冻结 mvp-scope.md（填入：必须交付文章CRUD+认证+评论；不做全文搜索/富文本/MinIO） | blog-Docs | 1天 |
| P0 | 产出 contracts/openapi.yaml 覆盖 auth + article 端点 | blog-Docs | 1天 |
| P0 | 创建 blog-BE/AGENTS.md（按 agents-md-standard 6 个必填章节） | blog-BE | 0.5天 |
| P0 | BE PR Gate CI（go build + go test + go vet） | blog-BE | 0.5天 |
| P0 | FE PR Gate CI（bun install + bun run build） | blog-FE | 0.5天 |
| P1 | 部署 Issue/PR 模板到 FE/BE 的 .github/ | 全部 | 0.5天 |
| P1 | FE AGENTS.md 补充控制面引用段 | blog-FE | 0.5天 |
| P1 | 迁移 workspace 根目录散落文档到 docs/archive/ | blog-Docs | 0.5天 |
| P1 | 试跑一次完整 OpenSpec change（验证流程闭环） | blog-Docs | 1天 |

**Phase 0 DoD**：
- [ ] BE 有 AGENTS.md 且符合标准
- [ ] FE/BE 各有 PR Gate CI 且可跑通
- [ ] blog-Docs 有 spec lint CI
- [ ] mvp-scope.md 填入真实范围
- [ ] 至少执行过一次完整的 OpenSpec change 生命周期
- [ ] workspace 根目录散落文档已归档

### Phase 1: 主干开发（4-6 周）

**目标**：基于冻结的 MVP 范围，前后端各自推进核心功能，控制面持续拆 issue + 验收

| 维度 | blog-Docs 产出 | FE 完成 | BE 完成 | Harness |
|------|---------------|---------|---------|---------|
| Spec | 每个业务域创建 L1 Feature Spec | 按 spec 开发 | 按 spec 开发 | spec 场景可映射到测试 |
| 契约 | openapi.yaml 冻结 auth + article | 基于契约实现 API client | 基于契约实现 handler | 契约一致性检查 |
| CI | — | 增加bun run lint | 增加go vet | PR 自动跑 |
| 测试 | — | 开始补 store 单测 | 开始补 handler 单测 | — |
| Harness | Phase 1 harness 计划 | auth + 页面 smoke | auth + article CRUD smoke | 核心链路 100% smoke |

**Phase 1 DoD**：
- [ ] BE auth + article + category/tag API 全部完成
- [ ] FE 脱离 mock 对接真实 API
- [ ] contracts/openapi.yaml 覆盖所有已实现 API
- [ ] 至少完成 5 个 OpenSpec change 全生命周期
- [ ] smoke 清单覆盖 5 条核心链路

### Phase 2: 联调交付（2-3 周）

**目标**：前后端全量联调、完成 MVP 功能闭环、准备发版

| 维度 | 产出 |
|------|------|
| 联调 | 全量联调 + 冒烟矩阵 |
| 契约 | 契约冻结（不再变更） |
| Harness | Playwright E2E 关键路径 |
| 发版 | 冻结候选版本 + 发版 smoke |
| 部署 | 一键 `docker compose up` 可用 |

### Phase 3: 稳定运维（持续）

| 维度 | 产出 |
|------|------|
| 治理 | 持续演进 spec、回写经验、漂移检测 |
| 测试 | 单测覆盖率 BE >70% / FE >60% |
| E2E | Playwright 全覆盖关键路径 |
| 监控 | 健康检查 + 日志 + 告警 |

---

## 7. Harness 架构

### 7.1 三层 Harness 分阶段搭建

| 阶段 | 层级 | 具体内容 |
|------|------|---------|
| Phase 0 | Layer 1: 工程秩序 | BE: go build/test/vet; FE: bun build; Docs: spec lint |
| Phase 1 | +Layer 2: 契约验证 | OpenAPI lint + 契约一致性测试 + smoke 脚本 |
| Phase 2 | +Layer 3: 交付闭环 | Playwright E2E + 发版门禁 + 部署验证 |

### 7.2 CI Workflow 设计

**Phase 0 — blog-BE/.github/workflows/ci.yml**

```yaml
name: PR Gate
on:
  pull_request:
    branches: [develop]
jobs:
  build-and-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-go@v5
        with:
          go-version-file: go.mod
          cache: true
      - run: go build ./...
      - run: go test ./...
      - run: go vet ./...
```

**Phase 0 — blog-FE/.github/workflows/ci.yml**

```yaml
name: PR Gate
on:
  pull_request:
    branches: [develop]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: oven-sh/setup-bun@v2
      - run: bun install --frozen-lockfile
      - run: bun run build
```

**Phase 0 — blog-Docs/.github/workflows/spec-lint.yml**

```yaml
name: Spec Lint
on:
  pull_request:
    branches: [develop, main]
jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Check spec files have required sections
        run: |
          for f in openspec/specs/*/spec.md; do
            grep -q "## Purpose" "$f" || { echo "$f missing Purpose"; exit 1; }
            grep -q "## Requirements" "$f" || { echo "$f missing Requirements"; exit 1; }
            grep -q "#### Scenario:" "$f" || { echo "$f missing Scenario"; exit 1; }
          done
```

### 7.3 证据采集与回写

```
CI 运行 → 产出四要素摘要 → 自动评论到 PR → 合并时回写 Issue → 周同步归档

证据格式：
| 检查项 | 命令 | 结果 | 证据 |
|--------|------|------|------|
| Build | go build ./... | PASS | CI run #123 |
| Test | go test ./... | PASS | CI run #123, 12/12 passed |
| Smoke: Auth Login | curl -X POST /api/v1/auth/login | PASS | 返回 200 + token |
```

---

## 8. 治理机制

### 8.1 OpenSpec Change 真实运营

| 场景 | 触发 | 流程 | 产物 |
|------|------|------|------|
| 新功能开发 | 需要新增 API / 数据模型 | openspec-propose → 拆 issue | L1 spec + tasks |
| 契约变更 | 需要改 API 字段/状态码 | openspec-propose → 更新契约 → 双端同步 | 变更后 openapi.yaml |
| 流程调整 | 需要改工作流/门禁 | openspec-propose → 更新 02-workflow | 变更后 workflow 文档 |
| 边界扩展 | 超出 MVP 范围 | openspec-propose → 更新 mvp-scope | 变更后 scope |
| Bug 修复 | 不涉及边界/契约 | 直接走 issue + PR | 无需 OpenSpec |

**最小运营节奏**：
- 每个新功能必须有 L1 spec（哪怕是精简版）
- 每周至少完成 1 个 change 的 archive
- 月度回顾：审计 L0 spec 是否漂移

### 8.2 防止控制面漂移（四道防线）

**防线 1：CI 门禁（机器层）**
- PR 必须通过 CI（build + test + lint）
- 契约变更必须通过 OpenAPI lint

**防线 2：Spec 场景映射（规范层）**
- 每个 L1 spec 至少有 1 个可验证 Scenario
- 每个 Issue 必须引用 Spec Scenario

**防线 3：周同步审计（流程层）**
- Issue 是否关联 Spec？PR 是否有 Harness 证据？
- FE/BE 的 AGENTS.md 是否与 agents-md-standard 一致

**防线 4：月度 L0 审计（治理层）**
- openapi.yaml 与实际 API 端点是否一致
- mvp-scope 的"本次不做"清单有无被违反
- 审计结果回写为新的 OpenSpec change

---

## 9. 系统一页设计（System Design on a Pager）

### 主资产

- **是**：前后端分离的博客内容管理 + 展示系统（认证、文章 CRUD、评论、分类标签）
- **不是**：多租户 SaaS、CMS 框架、高并发系统、移动端应用

### 分层与官方路径

```
L3 展示层   blog-FE (React 19 + TypeScript + Vite + Bun)
            官方路径：src/api/ → src/store/ → src/features/ → src/pages/
            禁止：组件直连后端、散落业务逻辑

L2 接口层   API Contract (OpenAPI 3.0, /api/v1/*)
            官方路径：contracts/openapi.yaml 是唯一真源
            禁止：口头约定接口、FE/BE 各自定义

L1 服务层   blog-BE (Go + Gin + GORM + PostgreSQL)
            官方路径：handler → service → dao → models
            禁止：handler 直连数据库、跨域调用

L0 治理层   blog-Docs (控制面)
            官方路径：OpenSpec change → spec → issue → PR → evidence → archive
            禁止：绕过 spec 直接改 FE/BE、口头变更契约
```

### 三条关键边界

1. **跨层直连**：FE 禁止绕过 API 契约直接定义数据结构，BE 禁止绕过 handler 层在 router 中写业务逻辑
2. **控制面绕行**：影响 scope/contract/change 的变更禁止不经 OpenSpec 直接合入
3. **无证据合并**：无 Harness 验证证据的 PR 禁止合并

### 已定决策

- 后端：Go/Gin/GORM/PostgreSQL，JWT RSA256 认证，bcrypt 密码
- 前端：React 19/Zustand/ShadcnUI/Tailwind，Bun 构建
- 发版：从 develop 冻结 commit 打 tag，不走 develop→main 合并 PR
- 认证：Bearer Token，access + refresh pair
- API：RESTful，统一响应 `{ message, data, requestId, code }`

### 待定问题

- 富文本编辑器选型
- 图片存储方案
- 验证码存储（内存 vs Redis）
- 日志方案
- 配置管理

### 下一批 Issue 种子

| ID | 仓库 | 标题 | 优先级 |
|----|------|------|--------|
| D-001 | blog-Docs | 冻结 mvp-scope.md | P0 |
| D-002 | blog-BE | 创建 AGENTS.md | P0 |
| D-003 | blog-BE | 添加 PR Gate CI | P0 |
| D-004 | blog-FE | 添加 PR Gate CI | P0 |
| D-005 | blog-Docs | 添加 spec-lint CI | P1 |
| D-006 | blog-Docs | 创建 api-contract L0 spec | P0 |
| D-007 | blog-Docs | 创建 domain-model L0 spec | P1 |
| D-008 | blog-Docs | 迁移散落文档 | P1 |
| D-009 | blog-Docs | 创建 harness-phase-plan.md | P1 |
| D-010 | blog-Docs | 试跑一次完整 OpenSpec change | P0 |

---

## 附录：已确认决策

| # | 问题 | 决策 |
|---|------|------|
| 1 | Global Spec 粒度 | 按能力拆（api-contract / domain-model / harness-baseline） |
| 2 | OpenSpec vs 轻量 PR | 灵活处理，小改动直接 PR，涉及契约/边界变更的走 OpenSpec |
| 3 | Agent 反馈机制 | 灵活，视情况在 issue comment 或直接沟通 |
| 4 | contracts/ 消费方式 | openapi.yaml 自动生成（openapi-typescript / openapi-fetch） |
| 5 | Phase 0 时间线 | 不钉死，按 DoD checklist 逐项验收 |
| 6 | 旧文档归档 | 先正常归档保留，RAG 污染问题等实际出现再处理 |
| 7 | main 分支同步 | 暂不处理 |
| 8 | github-feishu-webhook-bot | 暂不从 CLAUDE.md 移除 |
