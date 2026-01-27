# compoundcode-new 参考分析

## 参考项目

### Superpowers Brainstorming Skill

**文件位置**：`~/.claude/plugins/cache/claude-plugins-official/superpowers/*/skills/brainstorming/SKILL.md`

#### 核心工作流程

1. **理解阶段**
   - 检查当前项目状态（文件、文档、最近提交）
   - 逐个提问来细化想法
   - 优先使用多选题，但开放式问题也可以
   - **关键原则**：每条消息只问一个问题
   - 重点关注：目的、约束、成功标准

2. **探索方案阶段**
   - 提出 2-3 种不同的方案及其权衡
   - 以对话方式呈现选项，推荐方案优先
   - 解释推荐理由

3. **设计呈现阶段**
   - 将设计分成 200-300 字的小节
   - 每个小节后询问用户是否满意
   - 覆盖内容：架构、组件、数据流、错误处理、测试

4. **后续步骤**
   - 将验证的设计写入 `docs/plans/YYYY-MM-DD-<topic>-design.md`
   - 提交到 git
   - 询问是否准备实施
   - 使用 `superpowers:using-git-worktrees` 创建隔离工作区
   - 使用 `superpowers:writing-plans` 创建详细实施计划

#### 关键设计原则

- **一次一个问题**：不要用多个问题压倒用户
- **多选题优先**：比开放式问题更容易回答
- **YAGNI 原则**：从所有设计中无情地移除不必要的功能
- **探索替代方案**：总是在确定前提出 2-3 种方案
- **增量验证**：分节呈现设计，验证每一节
- **灵活性**：当某些内容不清楚时回头澄清

---

### Conductor NewTrack (`/conductor:newTrack`)

**文件位置**：`~/.gemini/extensions/conductor/commands/conductor/newTrack.toml`

#### 核心工作流程

1. **设置检查**
   - 验证必需文件存在：`conductor/tech-stack.md`、`conductor/workflow.md`、`conductor/product.md`
   - 如果缺失，立即停止并提示用户运行 `/conductor:setup`

2. **获取 Track 描述和类型**
   - 从参数或用户输入获取 track 描述
   - 自动推断 track 类型（Feature 或 Bug/Chore 等）
   - **不要求用户分类**

3. **交互式规格生成（spec.md）**
   - 宣布目标：引导用户通过一系列问题构建规格
   - **关键**：按顺序逐个提问，等待用户回答后再继续
   - 问题分类：
     - **Additive**：用于头脑风暴和定义范围
     - **Exclusive Choice**：用于基础、单一承诺
   - Feature 类型：问 3-5 个相关问题
   - 其他类型（Bug、Chore）：问 2-3 个相关问题
   - 生成 spec.md 草稿后，呈现给用户审核
   - 根据反馈修改，直到用户确认

4. **交互式计划生成（plan.md）**
   - 基于规格创建实施计划
   - 读取确认的 spec.md 和 `conductor/workflow.md`
   - 生成分层的 Phase、Task、Sub-task 列表
   - 包含状态标记 `[ ]`
   - 呈现给用户审核，根据反馈修改

5. **创建 Track 工件**
   - 生成唯一的 Track ID（格式：`shortname_YYYYMMDD`）
   - 创建目录和文件
   - 更新 `conductor/tracks.md`

#### 问题分类机制

**Additive（加法型）**：
- 用途：头脑风暴和定义范围（用户、目标、功能）
- 格式：开放式问题 + 选项列表 + "(Select all that apply)"
- 允许多个答案

**Exclusive Choice（排他型）**：
- 用途：基础、单一承诺（选择技术、工作流规则）
- 格式：直接问题，**不添加** "(Select all that apply)"
- 只能选择一个

#### 选项呈现规则

- 强烈推荐提供 2-3 个选项
- 最后一个选项必须是 "Type your own answer"
- 对于 Additive 问题，添加 "(Select all that apply)"

---

### Superpowers Writing-Plans Skill

**文件位置**：`~/.claude/plugins/cache/claude-plugins-official/superpowers/*/skills/writing-plans/SKILL.md`

#### 计划文档结构

```markdown
# [Feature Name] Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans

**Goal:** [One sentence describing what this builds]

**Architecture:** [2-3 sentences about approach]

**Tech Stack:** [Key technologies/libraries]
```

#### 任务粒度

每个步骤是一个动作（2-5 分钟）：
- "Write the failing test" - 一个步骤
- "Run it to make sure it fails" - 一个步骤
- "Implement the minimal code" - 一个步骤
- "Run the tests and make sure they pass" - 一个步骤
- "Commit" - 一个步骤

#### 任务结构

```markdown
### Task N: [Component Name]

**Files:**
- Create: `exact/path/to/file.py`
- Modify: `exact/path/to/existing.py:123-145`
- Test: `tests/exact/path/to/test.py`

**Step 1: Write the failing test**
[完整代码]

**Step 2: Run test to verify it fails**
Run: [exact command]
Expected: [expected output]
```

---

## CompoundCode New 设计决策

### 变更类型

支持三种类型：
- **Feature**：新功能，3-5 个问题
- **Bug**：问题修复，2-3 个问题
- **Chore**：维护/重构，2-3 个问题

### 问题设计

采用 Conductor 的问题分类：
- **Additive**：目标用户、成功标准、影响范围（可多选）
- **Exclusive Choice**：变更类型、优先级、技术选择（单选）

### 生成的文件

```
compound/changes/YYYY-MM-DD-feature-name/
├── spec.md          # 变更需求说明
├── plan.md          # 变更实施计划
└── metadata.json    # 元数据
```

### 与参考实现的对比

| 特性 | Brainstorming | NewTrack | CompoundCode New |
|------|---------------|----------|------------------|
| 问题数量 | 灵活 | 3-5 (Feature) | 3-5 (Feature) |
| 问题分类 | 无 | Additive/Exclusive | Additive/Exclusive |
| 输出格式 | design.md | spec.md + plan.md | spec.md + plan.md |
| 确认循环 | 分节确认 | 整体确认 | 整体确认 |
| 元数据 | 无 | metadata.json | metadata.json |
