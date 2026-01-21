# CompoundCode - Compound-Driven Development

## 介绍

**CompoundCode** -- **基于复利思维的上下文驱动开发方法论和工具集**。

将人类文档与AI编码结合在一起，让新员工和AI同用一个文档，都能快速的理解代码。

使用AI辅助编码，自动生成和维护需求规格说明、设计文档和代码，实现代码和文档规范复利增长。

我们相信：**未来越规范的项目和团队效率越高**，CompoundCode 致力于帮助团队实现这一目标。

## 特点

- **AI辅助编码**：利用AI技术自动生成和维护代码以及文档，提升开发效率。
- **复利增长**：采用`Context + Request -> Plan -> Execute -> Review -> Compound + Context`，在每次迭代中，将需求、设计和经验反馈到上下文中，实现代码质量和功能的复利增长。
- **先提问后设计**： 参考`superpowers`的理念，并不会立刻开始编写代码。相反，它会先退后一步，询问你真正想要做什么，就像一个经验丰富工程师一样，先需要你澄清需求和设计要求。
- **先设计后编码**： 参考`OpenSpec`、`spec-kit`等**SDD**(SPECS-Driven Development)的理念，强调在编码前进行充分的设计和规划，确保代码质量和可维护性。
- **上下文驱动开发**：参考`conductor`等**CDD**（Context-Driven Development）的理念，通过自动收集和维护上下文信息，帮助AI快速（Token less）和充分地理解项目需求和设计，提高代码生成的准确性和质量。
- **文档驱动开发**：强调通过文档驱动的开发流程，确保每个功能和变更都有清晰的设计和实现路径。
- **AI自动生成和更新文档**人类和AI共用同一套文档（上下文），确保信息一致性和可访问性。
- **模块化需求规格说明**：每个功能或变更请求都有独立的需求规格说明，便于管理和追踪。
- **变更记录**：详细记录每次变更的需求、计划和实施过程，确保项目的透明度和可追溯性。
- **易于上手**：提供简单的命令行工具，帮助团队快速启动和使用CompoundCode。
- **开源社区支持**：欢迎社区贡献和反馈，共同推动CompoundCode的发展。
- **多客户端支持**：支持 claude code、gemini cli、codex cli、copilot cli、opencode cli 等多种 AI 客户端，灵活适应不同团队的需求。（MVP 版本聚焦 Claude Code 集成）

## 使用方法

### 安装（MVP 版本）

CompoundCode MVP 版本以 Claude Code 插件的形式提供。

#### Claude Code 安装

```bash
# 1. 注册 CompoundCode 市场
/plugin marketplace add c4ys/compoundcode-marketplace

# 2. 安装插件
/plugin install compoundcode@compoundcode-marketplace

# 3. 验证安装
/help
```

### 快速开始

1. 在 Claude Code 中初始化项目：

```
/compoundcode:init
```

Claude 会引导你在项目中创建 `docs/` 目录和基础文档结构。

2. 开始一个新的功能或变更：

```
/compoundcode:new add-user-auth
```

Claude 会在 `docs/CHANGES/` 创建一个新的变更目录，包含 `spec.md` 和 `plan.md` 模板。

3. Claude 会自动读取 `docs/AGENTS.md` 了解项目上下文，并在整个开发过程中提供指导。

### 核心功能（MVP）

- **compoundcode:init**：在项目中初始化文档结构和基础文档。
- **compoundcode:new**：开启一个新的功能或变更请求，生成需求规格说明和实施计划。

### 未来功能

- **compoundcode:execute**：根据计划进行编码和实现的指导。
- **compoundcode:compound**：将反馈和新的经验整合到上下文中，为下一次迭代做好准备。
  
## 文档结构

运行 `/compoundcode:init` 后，将在你的项目中生成以下文档结构：

```plaintext
docs/
├── AGENTS.md  # 入口文件
├── PROJECT.md # 项目背景
├── TECHSTACK.md # 技术栈说明
├── ARCHITECTURE.md # 架构设计说明
├── CODESTYLE.md # 代码规范
├── WORKFLOW.md # 工作流说明
├── SPECS/ # 需求规格说明
│   ├── INDEX.md # 需求规格说明目录
│   ├── user-auth.md # 用户认证 目的、需求、设计、变更摘要
│   ├── xxx.md # 其他模块需求规格说明
├── CHANGES/ # 变更记录
│   ├── 2026-01-12-add-auth/ # 参考 conductor
│   │   ├──── spec.md # 变更需求说明
│   │   ├──── plan.md # 变更实施计划
│   │   ├──── /metadata.json
```

## 参考

- **spec-kit**: <https://github.com/github/spec-kit>
- **OpenSpec**: <https://github.com/Fission-AI/OpenSpec>
- **conductor**: <https://github.com/gemini-cli-extensions/conductor>
- **Compound Code介绍**: <https://zhuanlan.zhihu.com/p/1993009461451831150>
- **superpowers**: <https://github.com/obra/superpowers>
