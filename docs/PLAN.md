# CompoundCode 开发计划

## 项目概述

CompoundCode 是一个基于复利思维的上下文驱动开发方法论和工具集。**MVP 版本以 Claude Code 插件的形式提供**，通过 skill 文件让 AI 理解项目上下文，帮助项目建立标准化的文档结构和工作流，实现代码和文档规范的复利增长。

## 核心理念

**CompoundCode 不是一个项目模板，而是一个方法论工具**：
- 它为**目标项目**生成和维护文档结构
- 它通过 Claude Code skill 文件指导 AI 如何管理文档和工作流
- MVP 版本不提供独立的 CLI 工具，而是集成到 Claude Code 中

**工作流**：
```
目标项目 → /compoundcode:init → 创建文档结构（compound/AGENTS.md, project.md 等）
         → /compoundcode:new → 创建变更请求（compound/changes/xxx/）
         → 手动或 AI 辅助 → 编写 plan.md 和实施
         → 手动整合 → 更新 specs
```

## 当前状态

- 项目处于概念阶段，仅有 README.md 文件
- 无任何 skill 文件、插件配置或文档模板
- 需要从零开始构建

---

## MVP 范围

**目标**：为 Claude Code 用户提供可用的 CompoundCode 工作流

**交付形式**：Claude Code 插件（通过 marketplace 安装）

**包含功能**：
1. `/compoundcode:init` - 在目标项目中初始化文档结构
2. `/compoundcode:new` - 创建新的变更请求
3. CompoundCode skill 文件 - 让 Claude 理解 CompoundCode 方法论和工作流

**暂不包含**：
- `/compoundcode:execute` - 用户可以手动执行
- `/compoundcode:compound` - 用户可以手动整合
- 独立的 Python CLI 工具
- 其他 AI 客户端支持（Gemini、Codex 等）

---

## 开发阶段

### 阶段 1：插件基础结构和文档模板（M1）

**目标**：建立 Claude Code 插件的基础结构，并设计所有文档模板

#### 1.1 插件配置
- [ ] 创建插件目录结构
- [ ] 创建 `plugin.json` 配置文件
- [ ] 定义插件元数据（名称、版本、描述、作者）
- [ ] 配置 marketplace 信息

#### 1.2 基础目录结构
```
CompoundCode/
├── skills/
│   ├── compoundcode-init.md      # init 命令 skill
│   ├── compoundcode-new.md       # new 命令 skill
│   └── compoundcode-workflow.md  # 工作流指导 skill
├── plugin.json                    # 插件配置
└── README.md
```

#### 1.3 文档模板设计
设计以下文档模板（将内嵌在 skill 文件中）：
- [ ] `AGENTS.md` - AI 入口文件模板
- [ ] `project.md` - 项目背景模板
- [ ] `techstack.md` - 技术栈说明模板
- [ ] `architecture.md` - 架构设计模板
- [ ] `codestyle.md` - 代码规范模板
- [ ] `workflow.md` - 工作流说明模板
- [ ] `specs/index.md` - 需求规格目录模板
- [ ] `change_spec.md` - 变更请求模板
- [ ] `change_plan.md` - 变更计划模板

#### 1.4 模板变量
- [ ] 定义模板变量（项目名称、描述等）
- [ ] 设计模板填充逻辑

---

### 阶段 2：compoundcode:init skill（M2）

**目标**：实现 `/compoundcode:init` 命令

#### 2.1 Skill 实现
- [ ] 创建 `skills/compoundcode-init.md`
- [ ] 实现初始化逻辑指导
- [ ] 检测当前目录是否为项目根目录
- [ ] 创建 `compound/` 目录结构
- [ ] 生成所有基础文档
- [ ] 处理已存在文件的情况（询问是否覆盖）

#### 2.2 生成的目录结构
```
目标项目/
└── compound/
    ├── AGENTS.md
    ├── project.md
    ├── techstack.md
    ├── architecture.md
    ├── codestyle.md
    ├── workflow.md
    ├── init_state.json
    ├── specs/
    │   └── index.md
    └── changes/
```

---

### 阶段 3：compoundcode:new skill（M3）

**目标**：实现 `/compoundcode:new` 命令（包含 plan 功能）

#### 3.1 Skill 实现
- [ ] 创建 `skills/compoundcode-new.md`
- [ ] 实现 `new <name>` 命令逻辑
- [ ] 在 `compound/changes/` 创建变更目录（格式：`YYYY-MM-DD-name/`）
- [ ] 生成 `spec.md` 文件（需求规格说明）
- [ ] 生成 `plan.md` 文件（实施计划）
- [ ] 生成 `metadata.json` 文件

#### 3.2 生成的结构
```
compound/changes/2026-01-21-add-feature/
├── spec.md          # 变更需求说明
├── plan.md          # 变更实施计划
└── metadata.json    # 元数据
```

---

### 阶段 4：工作流 skill（M4）

**目标**：提供 CompoundCode 方法论的完整工作流指导

#### 4.1 Skill 文件
- [ ] 创建 `skills/compoundcode-workflow.md`
- [ ] 定义 CompoundCode 完整工作流
- [ ] 说明如何使用 compoundcode 命令
- [ ] 提供最佳实践指导
- [ ] 集成 brainstorming、planning 等流程

---

### 阶段 5：插件打包和发布（M5）

**目标**：将插件发布到 marketplace

#### 5.1 Marketplace 配置
- [ ] 创建 compoundcode-marketplace 仓库
- [ ] 配置 marketplace.json
- [ ] 添加插件索引

#### 5.2 测试
- [ ] 测试插件安装流程
- [ ] 测试 init 命令
- [ ] 测试 new 命令
- [ ] 测试完整工作流

#### 5.3 文档
- [ ] 完善 README.md
- [ ] 添加安装说明
- [ ] 添加使用示例
- [ ] 添加贡献指南

#### 5.4 发布
- [ ] 发布到 GitHub
- [ ] 注册 marketplace
- [ ] 宣传和推广

---

## 技术栈

| 组件 | 技术选型 | 说明 |
|------|----------|------|
| 插件系统 | Claude Code Plugin | 插件框架 |
| Skill 格式 | Markdown | Skill 文件格式 |
| 配置格式 | JSON | 插件配置和元数据 |
| 文档格式 | Markdown | 所有文档使用 Markdown |
| 模板系统 | 内嵌在 Skill 中 | 无需独立模板引擎 |

---

## 里程碑

| 里程碑 | 目标 | 交付物 |
|--------|------|--------|
| M1 | 插件基础和模板 | plugin.json, 目录结构, 文档模板设计 |
| M2 | init skill | compoundcode-init.md 可用 |
| M3 | new skill | compoundcode-new.md 可用 |
| M4 | 工作流 skill | compoundcode-workflow.md |
| M5 | MVP 发布 | marketplace 发布 |

---

## 下一步行动

1. **立即开始**：创建插件基础结构和文档模板（M1）
2. **优先实现**：`compoundcode:init` skill（M2），这是核心功能
3. **持续完善**：在开发过程中使用 CompoundCode 自身的方法论（dogfooding）

---

## 未来扩展（MVP 之后）

- `/compoundcode:execute` - 执行指导和进度跟踪的 skill
- `/compoundcode:compound` - 自动整合到上下文的 skill
- 支持其他 AI 客户端（Gemini CLI、Codex CLI 等）
- 可自定义模板
- 项目类型模板（Web、CLI、库等）
- 独立的 Python CLI 工具（可选）

---

## 参考资源

- [spec-kit](https://github.com/github/spec-kit) - GitHub 的规格说明工具
- [OpenSpec](https://github.com/Fission-AI/OpenSpec) - AI 驱动的规格说明
- [conductor](https://github.com/gemini-cli-extensions/conductor) - Gemini CLI 的变更管理
- [superpowers](https://github.com/obra/superpowers) - Claude Code 的超能力技能集
