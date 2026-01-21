# CompoundCode Init Skill 设计文档

## 一、参考项目研究

### 1.1 Conductor 项目 (`/conductor:setup`)

**核心特点**：
- **两阶段初始化**：交互式项目脚手架 + 初始 track 生成
- **断点续传**：通过 `setup_state.json` 记录进度，支持中断后继续
- **项目类型检测**：
  - Brownfield（已有项目）：检测 `.git`、依赖文件、源代码目录
  - Greenfield（新项目）：空目录或仅有 README.md
- **顺序问答**：每次只问一个问题，提供 A/B/C/D/E 选项
- **确认循环**：每个文档生成后需要用户审核（批准/修改）

**创建的目录结构**：
```plaintext
conductor/
├── setup_state.json          # 状态管理
├── product.md                # 产品定义
├── product-guidelines.md     # 产品指南
├── tech-stack.md            # 技术栈
├── workflow.md              # 工作流程
├── index.md                 # 项目上下文索引
├── tracks.md                # Track 注册表
├── code_styleguides/        # 代码风格指南
└── tracks/                  # Track 目录
```

### 1.2 OpenSpec 项目

**核心特点**：
- **简洁初始化**：快速创建基础结构
- **多工具集成**：支持 Cursor、Continue、GitHub Copilot 等
- **三阶段工作流**：Creating Changes → Implementing → Archiving

**创建的目录结构**：
```plaintext
.openspec/
├── specs/              # 规格说明
├── changes/            # 活动变更
├── archive/            # 已完成变更
├── project.md          # 项目上下文
├── AGENTS.md           # AI 助手指令
└── config.yaml         # 配置文件
```

---

## 二、用户需求确认

| 需求项 | 用户选择 |
|--------|----------|
| 交互模式 | **引导模式**（类似 Conductor，逐步询问项目信息） |
| 项目检测 | **智能检测** Brownfield/Greenfield |
| 断点续传 | **支持**（通过状态文件记录进度） |
| 文档目录 | **`compound/`**（而非 `docs/`） |
| 引导生成的文档 | project.md, techstack.md, architecture.md, codestyle.md |
| Brownfield 处理 | **自动分析代码**生成文档内容，用户审核确认 |
| 确认流程 | **每个文档单独确认**（批准/修改） |
| AGENTS.md | **详细模式**（包含完整工作流指导和文档索引） |

---

## 三、目标目录结构

```plaintext
目标项目/
└── compound/
    ├── AGENTS.md          # AI 入口文件（详细工作流指导）
    ├── project.md         # 项目背景（引导生成）
    ├── techstack.md       # 技术栈说明（引导生成）
    ├── architecture.md    # 架构设计（引导生成）
    ├── codestyle.md       # 代码规范（引导生成）
    ├── workflow.md        # 工作流说明（模板）
    ├── init_state.json    # 初始化状态（断点续传）
    ├── specs/             # 需求规格说明
    │   └── index.md       # 规格目录
    └── changes/           # 变更记录目录
```

---

## 四、初始化流程设计

### 4.1 流程概览

```plaintext
开始
  ↓
检查断点续传状态
  ↓ (有状态)        ↓ (无状态)
从断点继续    →    检测项目类型
                      ↓
              Brownfield / Greenfield
                      ↓
              创建 compound/ 目录
                      ↓
              生成 AGENTS.md（模板）
                      ↓
              引导生成 project.md
                      ↓ (确认)
              引导生成 techstack.md
                      ↓ (确认)
              引导生成 architecture.md
                      ↓ (确认)
              引导生成 codestyle.md
                      ↓ (确认)
              生成 workflow.md（模板）
                      ↓
              创建 specs/ 和 changes/ 目录
                      ↓
              清理状态文件
                      ↓
              完成
```

### 4.2 断点续传机制

**init_state.json 结构**：
```json
{
  "version": "1.0",
  "project_type": "brownfield",
  "last_successful_step": "techstack",
  "steps": {
    "project_detection": { "completed": true, "timestamp": "..." },
    "agents": { "completed": true, "timestamp": "..." },
    "project": { "completed": true, "timestamp": "..." },
    "techstack": { "completed": true, "timestamp": "..." },
    "architecture": { "completed": false },
    "codestyle": { "completed": false },
    "workflow": { "completed": false },
    "specs_index": { "completed": false },
    "finalize": { "completed": false }
  },
  "collected_data": {
    "project_name": "...",
    "description": "...",
    "tech_stack": {}
  }
}
```

### 4.3 项目类型检测

**Brownfield 检测条件**（满足任一）：
- 存在 `.git` 目录
- 存在依赖文件：`package.json`, `requirements.txt`, `Cargo.toml`, `go.mod`, `pom.xml` 等
- 存在源代码目录：`src/`, `lib/`, `app/` 等
- 存在超过 5 个代码文件

**Greenfield 检测条件**：
- 空目录
- 仅有 README.md 或 LICENSE 文件

---

## 五、问答设计

### 5.1 project.md 问答

**问题 1**：项目名称
- A) 使用目录名：`{directory_name}`
- B) 自定义输入
- C) [Brownfield] 从 package.json/Cargo.toml 等读取

**问题 2**：项目描述
- A) [Brownfield] 自动生成的描述
- B) 自定义输入
- C) 暂时跳过

**问题 3**：目标用户
- A) 开发者
- B) 终端用户
- C) 企业客户
- D) 自定义输入

### 5.2 techstack.md 问答

**问题 1**：主要编程语言
- A) [Brownfield] 检测到的语言
- B) TypeScript/JavaScript
- C) Python
- D) Rust
- E) 自定义输入

**问题 2**：框架/库
- A) [Brownfield] 检测到的框架
- B) 自定义输入
- C) 暂时跳过

**问题 3**：数据库
- A) 无
- B) PostgreSQL
- C) MySQL
- D) MongoDB
- E) 自定义输入

### 5.3 architecture.md 问答

**问题 1**：架构模式
- A) [Brownfield] 分析现有代码结构
- B) 单体应用
- C) 微服务
- D) 无服务器
- E) 自定义输入

**问题 2**：核心模块
- A) [Brownfield] 自动分析目录结构
- B) 自定义输入

### 5.4 codestyle.md 问答

**问题 1**：代码风格
- A) [Brownfield] 检测现有配置（ESLint, Prettier, Black 等）
- B) 使用默认模板
- C) 自定义输入

---

## 六、待设计内容

- [ ] 各文档的完整模板内容
- [ ] Skill 文件的完整实现
- [ ] 代码分析策略的具体实现
- [ ] 错误处理和边界情况

---

## 七、下一步

1. 设计各文档的模板内容
2. 编写 `compoundcode-init.md` skill 文件
3. 测试初始化流程
