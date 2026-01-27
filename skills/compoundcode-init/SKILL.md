---
name: compoundcode-init
description: 初始化 CompoundCode 文档结构。当用户要求"初始化 CompoundCode"、"设置 compound 文档"、"创建 compound 目录"、"运行 compoundcode init"、"开始新的 CompoundCode 项目"时使用此 skill。支持 Brownfield/Greenfield 项目检测、断点续传、引导式问答生成文档。
---

# CompoundCode Init

在目标项目中初始化 CompoundCode 文档结构，创建 `compound/` 目录和 AI 可读的文档。

## 核心能力

- 检测项目类型（Brownfield/Greenfield）
- 支持中断后恢复（断点续传）
- 引导式问答生成文档
- 分析现有代码（Brownfield 项目）

## 初始化工作流

**重要：每次只问一个问题，等待用户回答后再继续。**

### 步骤 0：检查恢复状态

检查 `compound/init_state.json` 是否存在：

1. **存在且未完成**：
   - 读取 `last_successful_step`
   - 询问用户：
     - A) 从断点继续
     - B) 重新开始（删除状态）
   - 若继续，跳转到 `last_successful_step` 的下一步

2. **不存在或已完成**：开始新的初始化

### 步骤 1：项目检测

检测项目类型：

**Brownfield 指标**（满足任一）：
- 存在 `.git` 目录
- 存在依赖文件：`package.json`, `requirements.txt`, `Cargo.toml`, `go.mod`, `pyproject.toml`, `pom.xml`, `build.gradle`
- 存在源代码目录：`src/`, `lib/`, `app/`, `pkg/`
- 超过 5 个代码文件（.py, .js, .ts, .go, .rs, .java）

**Greenfield 指标**：
- 空目录
- 仅有 README.md、LICENSE 或 .gitignore

展示检测结果并确认：
- A) 确认检测结果
- B) 改为 Brownfield
- C) 改为 Greenfield

更新状态：`project_detection.completed = true`

### 步骤 2：创建目录结构

创建 `compound/` 目录和子目录：

```
compound/
├── specs/
└── changes/
```

更新状态：`directory_creation.completed = true`

### 步骤 3：生成 AGENTS.md

从模板生成 `compound/AGENTS.md`。

**Brownfield**：包含检测到的项目上下文
**Greenfield**：使用最小模板

展示给用户确认：
- A) 批准
- B) 修改（用户提供修改内容）

更新状态：`agents.completed = true`

### 步骤 4：生成 project.md（引导式）

**问题 1/3：项目名称**
- A) 使用目录名：`{detected_name}`
- B) [Brownfield] 从配置读取：`{from_config}`
- C) 自定义输入

**问题 2/3：项目描述**
- A) [Brownfield] 自动生成：`{analyzed_description}`
- B) 自定义输入
- C) 暂时跳过

**问题 3/3：目标用户**
- A) 开发者
- B) 终端用户
- C) 企业客户
- D) 自定义输入

生成 `compound/project.md` 并确认。

更新状态：`project.completed = true`

### 步骤 5：生成 techstack.md（引导式）

**问题 1/3：主要编程语言**
- A) [Brownfield] 检测到：`{detected_languages}`
- B) TypeScript/JavaScript
- C) Python
- D) Rust
- E) Go
- F) 自定义输入

**问题 2/3：框架/库**
- A) [Brownfield] 检测到：`{detected_frameworks}`
- B) 自定义输入
- C) 暂时跳过

**问题 3/3：数据库**
- A) 无
- B) PostgreSQL
- C) MySQL
- D) MongoDB
- E) SQLite
- F) 自定义输入

生成 `compound/techstack.md` 并确认。

更新状态：`techstack.completed = true`

### 步骤 6：生成 architecture.md（引导式）

**问题 1/2：架构模式**
- A) [Brownfield] 分析结果：`{detected_pattern}`
- B) 单体应用
- C) 微服务
- D) 无服务器
- E) 插件架构
- F) 自定义输入

**问题 2/2：核心模块**
- A) [Brownfield] 检测到：`{detected_modules}`
- B) 自定义输入

生成 `compound/architecture.md` 并确认。

更新状态：`architecture.completed = true`

### 步骤 7：生成 codestyle.md（引导式）

**问题 1/1：代码风格配置**
- A) [Brownfield] 检测到配置：`{detected_linters}`
- B) 使用语言默认规范
- C) 自定义输入

生成 `compound/codestyle.md` 并确认。

更新状态：`codestyle.completed = true`

### 步骤 8：生成 workflow.md

从模板生成 `compound/workflow.md`（标准 CompoundCode 工作流）。

展示并确认。

更新状态：`workflow.completed = true`

### 步骤 9：生成 specs/index.md

从模板生成 `compound/specs/index.md`。

更新状态：`specs_index.completed = true`

### 步骤 10：完成

1. 标记状态文件为已完成
2. 展示创建的文件摘要
3. 提供下一步指导

更新状态：`finalize.completed = true`

## 问答格式

```
**问题 X/Y: [主题]**

[上下文说明（如有）]

选项:
- A) [选项 A 说明]
- B) [选项 B 说明]
- C) [选项 C 说明]
- D) 自定义输入

请选择 (A/B/C/D) 或输入自定义答案:
```

## 状态文件格式

`compound/init_state.json`：

```json
{
  "version": "1.0",
  "project_type": "brownfield|greenfield",
  "last_successful_step": "techstack",
  "steps": {
    "project_detection": { "completed": true, "timestamp": "ISO8601" },
    "directory_creation": { "completed": true, "timestamp": "ISO8601" },
    "agents": { "completed": true, "timestamp": "ISO8601" },
    "project": { "completed": true, "timestamp": "ISO8601" },
    "techstack": { "completed": true, "timestamp": "ISO8601" },
    "architecture": { "completed": false },
    "codestyle": { "completed": false },
    "workflow": { "completed": false },
    "specs_index": { "completed": false },
    "finalize": { "completed": false }
  },
  "collected_data": {
    "project_name": "...",
    "description": "...",
    "languages": [],
    "frameworks": [],
    "database": "...",
    "architecture_pattern": "...",
    "modules": []
  }
}
```

## Brownfield 分析

对于 Brownfield 项目，分析代码以预填选项。详见 [references/brownfield-analysis.md](references/brownfield-analysis.md)。

## 文档模板

所有文档模板详见 [references/templates.md](references/templates.md)。

## 错误处理

- 文件创建失败：报告错误并重试
- 用户取消：保存当前状态以便恢复
- 状态文件损坏：提供重新开始选项
