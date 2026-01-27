# compoundcode-init 参考分析

## 参考项目

### Conductor Setup (`/conductor:setup`)

**文件位置**：`~/.gemini/extensions/conductor/commands/conductor/setup.toml`

#### 核心特点

1. **两阶段初始化**：交互式项目脚手架 + 初始 track 生成
2. **断点续传**：通过 `setup_state.json` 记录进度
3. **项目类型检测**：
   - Brownfield（已有项目）：检测 `.git`、依赖文件、源代码目录
   - Greenfield（新项目）：空目录或仅有 README.md
4. **顺序问答**：每次只问一个问题，提供 A/B/C/D/E 选项
5. **确认循环**：每个文档生成后需要用户审核（批准/修改）

#### 创建的目录结构

```
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

#### 断点续传实现

**状态文件结构**：
```json
{"last_successful_step": ""}
```

**恢复逻辑**：
- 检查 `conductor/setup_state.json` 是否存在
- 根据 `last_successful_step` 的值跳转到对应阶段
- 每个主要阶段完成后立即更新状态文件

#### 问答设计

**问题分类**：
- **Additive**：鼓励多个答案，添加 "(Select all that apply)"
- **Exclusive Choice**：单一选择，不添加此提示

**标准响应格式**：
```
A) [选项 A]
B) [选项 B]
C) [选项 C]
D) [输入自定义答案]
E) [自动生成并审查]
```

#### 项目类型检测

**Brownfield 指标**（满足任一）：
1. 版本控制存在: `.git`, `.svn`, `.hg`
2. 脏仓库: `git status --porcelain` 返回未提交更改
3. 依赖清单: `package.json`, `pom.xml`, `requirements.txt`, `go.mod` 等
4. 源代码目录: `src/`, `app/`, `lib/` 等包含代码文件

**Greenfield 条件**：
- 不满足任何 Brownfield 指标
- 目录为空或仅包含通用文档

---

### OpenSpec 项目

**核心特点**：
- 简洁初始化：快速创建基础结构
- 多工具集成：支持 Cursor、Continue、GitHub Copilot 等
- 三阶段工作流：Creating Changes → Implementing → Archiving

**创建的目录结构**：
```
.openspec/
├── specs/              # 规格说明
├── changes/            # 活动变更
├── archive/            # 已完成变更
├── project.md          # 项目上下文
├── AGENTS.md           # AI 助手指令
└── config.yaml         # 配置文件
```

---

## CompoundCode Init 设计决策

### 目录命名

选择 `compound/` 而非 `docs/` 或 `.compoundcode/`：
- 明确表示这是 CompoundCode 的上下文目录
- 不与现有 docs/ 目录冲突
- 不隐藏（与 .openspec/ 不同）

### 文档结构

```
compound/
├── AGENTS.md          # AI 入口文件
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

### 与 Conductor 的对比

| 特性 | Conductor | CompoundCode Init |
|------|-----------|-------------------|
| 配置格式 | TOML | Markdown (Skill) |
| 问答方式 | A/B/C/D/E 选项 | A/B/C/D 选项 + 自定义 |
| 断点续传 | setup_state.json | init_state.json |
| 目标目录 | conductor/ | compound/ |
| 生成文档 | product.md, tech-stack.md | project.md, techstack.md |
| Track 系统 | 有 | 无（使用 changes/） |
| 代码分析 | 基础检测 | 详细分析（Brownfield） |
