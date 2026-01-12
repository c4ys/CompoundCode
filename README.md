# CompoundCode - Compound-Driven Development

## 介绍

CompoundCode 将人类文档与AI编码结合在一起，让新员工和AI同用一个文档，都能快速的理解代码。
使用AI辅助编码，自动生成和维护需求规格说明、设计文档和代码，实现代码和文档规范复利增长。
我们相信：未来越规范的项目和团队效率越高，CompoundCode 致力于帮助团队实现这一目标。

## 特点

- **统一文档**：人类和AI共用同一套文档，确保信息一致性和可访问性。
- **AI辅助编码**：利用AI技术自动生成和维护代码以及文档，提升开发效率。
- **复利增长**：采用`Context + Request -> Plan -> Execute -> Review -> Compound + Context`，在每次迭代中，将经验和设计反馈到上下文中，实现代码质量和功能的复利增长。

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
├── CHANGES/ # 变更记录
│   ├── 2026-01-12-add-auth/ # 参考 conductor
│   │   ├──── spec.md # 变更需求说明
│   │   ├──── plan.md # 变更实施计划
│   │   ├──── /metadata.json
```

## 参考

- [spec-kit]<https://github.com/github/spec-kit>
- <https://github.com/Fission-AI/OpenSpec>
- <https://github.com/gemini-cli-extensions/conductor>
- <https://zhuanlan.zhihu.com/p/1993009461451831150>
- <https://github.com/obra/superpowers>
