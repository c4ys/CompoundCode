# CompoundCode - Compound-Driven Development

## 介绍

CompoundCode -- 基于复利思维的上下文驱动开发方法论和工具集。

将人类文档与AI编码结合在一起，让新员工和AI同用一个文档，都能快速的理解代码。

使用AI辅助编码，自动生成和维护需求规格说明、设计文档和代码，实现代码和文档规范复利增长。

我们相信：**未来越规范的项目和团队效率越高**，CompoundCode 致力于帮助团队实现这一目标。

## 特点

- **统一文档**：人类和AI共用同一套文档，确保信息一致性和可访问性。
- **AI辅助编码**：利用AI技术自动生成和维护代码以及文档，提升开发效率。
- **复利增长**：采用`Context + Request -> Plan -> Execute -> Review -> Compound + Context`，在每次迭代中，将经验和设计反馈到上下文中，实现代码质量和功能的复利增长。
- **先设计后编码**： 参考**superpowers**的理念，强调在编码前进行充分的设计和规划，确保代码质量和可维护性。
- **模块化需求规格说明**：每个功能或变更请求都有独立的需求规格说明，便于管理和追踪。
- **变更记录**：详细记录每次变更的需求、计划和实施过程，确保项目的透明度和可追溯性。
- **易于上手**：提供简单的命令行工具，帮助团队快速启动和使用CompoundCode。
- **开源社区支持**：欢迎社区贡献和反馈，共同推动CompoundCode的发展。
- **文档驱动开发**：强调通过文档驱动的开发流程，确保每个功能和变更都有清晰的设计和实现路径。
- **上下文驱动开发**：通过维护丰富的上下文信息，帮助AI更好地理解项目需求和设计，提高代码生成的准确性和质量。

## 使用方法

- **compound:init**：初始化项目结构和基础文档。
- **compound:new**：开启一个新的功能或变更请求，生成相应的需求规格说明。
- **compound:plan**：基于需求规格说明，制定详细的实现计划。
- **compound:execute**：根据计划进行编码和实现。
- **compound:review**：对实现的代码和文档进行审查，确保质量和一致性。
- **compound:compound**：将审查反馈和新的经验整合到上下文中，为下一次迭代做好准备。
  
## 文档结构

```plaintext
docs/
├── AGENTS.md  # 入口文件
├── PROJECT.md # 项目背景
├── TECKSTACK.md # 技术栈说明
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
