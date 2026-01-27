# Skill 设计参考分析

本文档记录了 CompoundCode 各 skill 设计时参考的项目和实现分析。

## 目录

- [init-skill-reference.md](./init-skill-reference.md) - compoundcode-init 参考分析
- [new-skill-reference.md](./new-skill-reference.md) - compoundcode-new 参考分析
- [execute-skill-reference.md](./execute-skill-reference.md) - compoundcode-execute 参考分析

## 参考项目

| 项目 | 来源 | 主要参考点 |
|------|------|-----------|
| Conductor | gemini-cli-extensions | setup、newTrack、implement 命令设计 |
| Superpowers | Claude Code 官方插件 | brainstorming、executing-plans、TDD skill |
| OpenSpec | Fission-AI | 目录结构、工作流设计 |
| spec-kit | GitHub | 规格说明模板 |

## 核心设计原则

### 1. 问答设计

所有参考实现都遵循：
- **一次一个问题**：不要用多个问题压倒用户
- **多选题优先**：提供 2-3 个选项 + 自定义输入
- **等待用户回答**：不要假设或跳过
- **确认循环**：生成文档后用户审核

### 2. 问题分类

来自 Conductor 的设计：
- **Additive（加法型）**：收集多个想法，添加 "(可多选)"
- **Exclusive Choice（排他型）**：单一决策，不添加 "(可多选)"

### 3. 断点续传

所有 skill 都支持中断恢复：
- 使用状态文件记录进度
- 每步完成后立即更新
- 启动时检查并提供恢复选项

### 4. TDD 工作流

来自 Superpowers 的 Red-Green-Refactor：
- **Red**：写失败的测试
- **Green**：写最小代码通过
- **Refactor**：清理代码

### 5. 进度跟踪

结合两种方式：
- **plan.md 任务状态**：`[ ]` → `[~]` → `[x]`
- **commit SHA 记录**：审计日志
- **阶段检查点**：验证通过后创建
