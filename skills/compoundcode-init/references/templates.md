# 文档模板

本文件包含 CompoundCode 初始化时生成的所有文档模板。

模板变量使用 `{{VARIABLE_NAME}}` 格式，在生成时替换为实际值。

---

## AGENTS.md 模板

```markdown
# {{PROJECT_NAME}} - AI 工作指南

## 项目概述

{{PROJECT_DESCRIPTION}}

## 文档索引

| 文档 | 说明 |
|------|------|
| [project.md](./project.md) | 项目背景和目标 |
| [techstack.md](./techstack.md) | 技术栈说明 |
| [architecture.md](./architecture.md) | 架构设计 |
| [codestyle.md](./codestyle.md) | 代码规范 |
| [workflow.md](./workflow.md) | 工作流程 |
| [specs/](./specs/) | 需求规格说明 |
| [changes/](./changes/) | 变更记录 |

## 工作流程

### 开始新功能/变更

1. 在 `changes/` 创建新目录：`YYYY-MM-DD-feature-name/`
2. 创建 `spec.md` 描述需求
3. 创建 `plan.md` 制定实施计划
4. 按计划实施，更新进度

### 代码规范

遵循 [codestyle.md](./codestyle.md) 中的规范。

### 提交规范

- feat: 新功能
- fix: 修复
- docs: 文档
- refactor: 重构
- test: 测试

## 上下文加载

开始工作前，请阅读：
1. 本文件了解项目概况
2. `project.md` 了解项目背景
3. `techstack.md` 了解技术栈
4. 相关的 `specs/` 文档
```

---

## project.md 模板

```markdown
# {{PROJECT_NAME}}

## 项目背景

{{PROJECT_DESCRIPTION}}

## 目标用户

{{TARGET_USERS}}

## 核心价值

<!-- 描述项目为用户提供的核心价值 -->

## 项目状态

- **阶段**: {{PROJECT_PHASE}}
- **版本**: {{VERSION}}

## 关键里程碑

| 里程碑 | 目标 | 状态 |
|--------|------|------|
| MVP | - | 进行中 |

## 相关链接

- 仓库: {{REPO_URL}}
```

---

## techstack.md 模板

```markdown
# 技术栈

## 编程语言

{{LANGUAGES}}

## 框架和库

### 核心框架

{{FRAMEWORKS}}

### 主要依赖

<!-- 列出主要依赖及其用途 -->

## 数据库

{{DATABASE}}

## 开发工具

### 包管理

{{PACKAGE_MANAGER}}

### 构建工具

<!-- 列出构建工具 -->

### 代码质量

{{LINTERS}}

## 部署环境

<!-- 描述部署环境 -->
```

---

## architecture.md 模板

```markdown
# 架构设计

## 架构模式

{{ARCHITECTURE_PATTERN}}

## 目录结构

```
{{DIRECTORY_STRUCTURE}}
```

## 核心模块

{{CORE_MODULES}}

## 数据流

<!-- 描述数据在系统中的流动 -->

## 关键设计决策

<!-- 记录重要的架构决策及其原因 -->
```

---

## codestyle.md 模板

```markdown
# 代码规范

## 通用规范

- 使用有意义的变量名
- 保持函数简短（建议 < 50 行）
- 添加必要的注释
- 遵循 DRY 原则

## 语言特定规范

{{LANGUAGE_SPECIFIC_RULES}}

## 格式化工具

{{FORMATTERS}}

## Linter 配置

{{LINTER_CONFIG}}

## 命名约定

| 类型 | 约定 | 示例 |
|------|------|------|
| 变量 | camelCase | `userName` |
| 函数 | camelCase | `getUserName()` |
| 类 | PascalCase | `UserService` |
| 常量 | UPPER_SNAKE_CASE | `MAX_RETRY_COUNT` |
| 文件 | kebab-case | `user-service.ts` |

## 注释规范

- 使用中文注释
- 复杂逻辑必须添加注释
- 公共 API 需要文档注释
```

---

## workflow.md 模板

```markdown
# 工作流程

## CompoundCode 方法论

本项目采用 CompoundCode 方法论，强调：
- 先设计后编码
- 文档驱动开发
- 上下文复利增长

## 开发流程

### 1. 需求分析

1. 在 `specs/` 中查找或创建需求文档
2. 明确需求范围和验收标准

### 2. 变更规划

1. 创建变更目录：`changes/YYYY-MM-DD-feature-name/`
2. 编写 `spec.md`：详细需求说明
3. 编写 `plan.md`：实施计划

### 3. 实施

1. 按 `plan.md` 逐步实施
2. 遵循 TDD：先写测试，再写实现
3. 保持小步提交

### 4. 审核

1. 代码审查
2. 测试验证
3. 文档更新

### 5. 整合

1. 更新 `specs/` 相关文档
2. 归档变更记录
3. 更新项目文档

## 提交规范

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Type

| 类型 | 说明 |
|------|------|
| feat | 新功能 |
| fix | 修复 |
| docs | 文档 |
| style | 格式（不影响代码运行） |
| refactor | 重构 |
| test | 测试 |
| chore | 构建/工具 |

## 分支策略

| 分支 | 用途 |
|------|------|
| main | 稳定版本 |
| develop | 开发分支 |
| feature/* | 功能分支 |
| fix/* | 修复分支 |

## 代码审查清单

- [ ] 代码符合规范
- [ ] 测试覆盖充分
- [ ] 文档已更新
- [ ] 无安全漏洞
- [ ] 性能可接受
```

---

## specs/index.md 模板

```markdown
# 需求规格目录

## 概述

本目录包含项目的需求规格说明文档。

## 文档列表

| 模块 | 文档 | 状态 | 最后更新 |
|------|------|------|----------|
| - | - | - | - |

## 添加新规格

1. 创建新文件：`specs/<module-name>.md`
2. 使用下方规格模板
3. 更新本索引

## 规格模板

```markdown
# <模块名称>

## 目的

<模块的目的和价值>

## 需求

### 功能需求

- [ ] 需求 1
- [ ] 需求 2

### 非功能需求

- [ ] 性能要求
- [ ] 安全要求

## 设计

<设计概要>

## 变更历史

| 日期 | 变更 | 作者 |
|------|------|------|
```
```

---

## 变更 spec.md 模板

用于 `changes/YYYY-MM-DD-name/spec.md`：

```markdown
# {{CHANGE_NAME}}

## 概述

{{CHANGE_DESCRIPTION}}

## 背景

<!-- 为什么需要这个变更 -->

## 需求

### 功能需求

- [ ] 需求 1
- [ ] 需求 2

### 非功能需求

- [ ] 性能要求
- [ ] 安全要求

## 验收标准

- [ ] 标准 1
- [ ] 标准 2

## 影响范围

<!-- 列出受影响的模块/文件 -->

## 风险评估

<!-- 潜在风险及缓解措施 -->
```

---

## 变更 plan.md 模板

用于 `changes/YYYY-MM-DD-name/plan.md`：

```markdown
# {{CHANGE_NAME}} - 实施计划

## 概述

基于 [spec.md](./spec.md) 的实施计划。

## 任务分解

### 阶段 1: 准备

- [ ] 任务 1.1
- [ ] 任务 1.2

### 阶段 2: 实施

- [ ] 任务 2.1
- [ ] 任务 2.2

### 阶段 3: 测试

- [ ] 任务 3.1
- [ ] 任务 3.2

### 阶段 4: 完成

- [ ] 代码审查
- [ ] 文档更新
- [ ] 合并到主分支

## 依赖

<!-- 列出依赖的其他任务或资源 -->

## 进度跟踪

| 任务 | 状态 | 完成时间 | 备注 |
|------|------|----------|------|
| - | - | - | - |
```

---

## 模板变量说明

| 变量 | 说明 | 来源 |
|------|------|------|
| `{{PROJECT_NAME}}` | 项目名称 | 用户输入或目录名 |
| `{{PROJECT_DESCRIPTION}}` | 项目描述 | 用户输入或分析结果 |
| `{{TARGET_USERS}}` | 目标用户 | 用户选择 |
| `{{PROJECT_PHASE}}` | 项目阶段 | 默认"开发中" |
| `{{VERSION}}` | 版本号 | 从配置文件读取或默认"0.1.0" |
| `{{REPO_URL}}` | 仓库地址 | 从 git remote 读取 |
| `{{LANGUAGES}}` | 编程语言 | 用户选择或分析结果 |
| `{{FRAMEWORKS}}` | 框架 | 用户选择或分析结果 |
| `{{DATABASE}}` | 数据库 | 用户选择 |
| `{{PACKAGE_MANAGER}}` | 包管理器 | 分析结果 |
| `{{LINTERS}}` | Linter 工具 | 分析结果 |
| `{{ARCHITECTURE_PATTERN}}` | 架构模式 | 用户选择或分析结果 |
| `{{DIRECTORY_STRUCTURE}}` | 目录结构 | 分析结果 |
| `{{CORE_MODULES}}` | 核心模块 | 用户输入或分析结果 |
| `{{LANGUAGE_SPECIFIC_RULES}}` | 语言特定规范 | 根据语言生成 |
| `{{FORMATTERS}}` | 格式化工具 | 分析结果 |
| `{{LINTER_CONFIG}}` | Linter 配置 | 分析结果 |
| `{{CHANGE_NAME}}` | 变更名称 | 用户输入 |
| `{{CHANGE_DESCRIPTION}}` | 变更描述 | 用户输入 |
