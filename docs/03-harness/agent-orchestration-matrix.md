# 多 Agent 编排矩阵（PM 视角）

## 目标

让多 Agent 协作有清晰边界，不因“多开窗口”导致责任混乱。

## 角色矩阵

### PM Agent

- 任务拆解与优先级编排
- 阻塞升级与节奏控制

### Product Review Agent

- 范围与目标一致性检查
- 验收口径完整性检查

### FE Agent

- 前端实现与联调

### BE Agent

- 后端实现与契约稳定

### Review Agent

- PR 审核，输出 BLOCKER/REQUIRED/FOLLOW-UP

### Quality Agent

- 构建、测试、格式、提交规范检查

### Docs Agent

- 文档回写、模板升级、规则沉淀

## 编排规则

- 一个任务只能有一个主执行 Agent
- 审核 Agent 不能同时作为该任务执行 Agent
- 质量检查与代码审核分离，避免同源偏差

## 输出格式统一

每个 Agent 输出必须包含：

- 结论（通过/阻塞/建议）
- 证据（命令结果/日志/文件）
- 下一步动作
