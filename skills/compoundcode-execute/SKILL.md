---
name: compoundcode-execute
description: 执行 CompoundCode 变更计划。当用户要求"执行计划"、"实施变更"、"compoundcode execute"、"开始实现"、"执行 plan.md"时使用此 skill。遵循 TDD 工作流，跟踪任务进度，验证阶段完成，同步文档。
---

# CompoundCode Execute

执行 `compound/changes/` 中的变更计划，遵循 TDD 工作流，跟踪进度，验证完成。

## 前置条件

1. **检查 compound/ 目录**：必须存在且包含基础文档
2. **检查 changes/ 目录**：必须有待执行的变更
3. **检查 git 仓库**：建议在干净的工作目录上执行

如果前置条件不满足，提示用户先运行相应命令。

## 工作流程

### 步骤 1：环境验证

检查以下文件存在：
- `compound/AGENTS.md`
- `compound/project.md`
- `compound/workflow.md`
- `compound/changes/` 目录

检查 git 状态：
```bash
git status --porcelain
```

如果有未提交的更改，询问用户：
```
**工作目录不干净**

检测到未提交的更改。建议先提交或暂存。

选项：
- A) 继续执行（不推荐）
- B) 我先处理未提交的更改

请选择：
```

### 步骤 2：变更选择

列出所有待执行的变更：

```bash
ls compound/changes/
```

读取每个变更的 `metadata.json`，筛选 `status != "completed"` 的变更。

展示变更列表：
```
**选择要执行的变更**

找到以下待执行的变更：

1. 2026-01-27-add-auth (Feature) - 添加用户认证
2. 2026-01-28-fix-login (Bug) - 修复登录问题

选项：
- A) 执行第 1 个变更（最早的）
- B) 执行第 2 个变更
- C) 输入变更名称

请选择：
```

### 步骤 3：加载上下文

读取以下文件：
- `compound/changes/{change_id}/spec.md` - 需求规格
- `compound/changes/{change_id}/plan.md` - 实施计划
- `compound/changes/{change_id}/metadata.json` - 元数据
- `compound/workflow.md` - 工作流指导
- `compound/AGENTS.md` - 项目上下文

更新 `metadata.json` 状态为 `in_progress`。

### 步骤 4：任务执行

遍历 `plan.md` 中的任务，对每个任务：

#### 4.1 标记任务开始

在 `plan.md` 中将任务状态从 `[ ]` 改为 `[~]`（进行中）。

#### 4.2 遵循 TDD 工作流

**Red 阶段**：
1. 写一个失败的测试
2. 运行测试，确认失败
3. 失败原因必须是功能缺失，不是语法错误

**Green 阶段**：
1. 写最小代码通过测试
2. 不添加额外功能
3. 运行测试，确认通过

**Refactor 阶段**（可选）：
1. 清理代码
2. 移除重复
3. 改进命名
4. 保持测试通过

#### 4.3 提交代码

```bash
git add <changed_files>
git commit -m "<type>(<scope>): <description>"
```

提交信息格式遵循 `compound/workflow.md` 中的规范。

#### 4.4 附加 git notes（可选）

```bash
git notes add -m "Task: <task_description>
Change: <change_id>
Status: completed"
```

#### 4.5 更新 plan.md

将任务状态从 `[~]` 改为 `[x]`，记录 commit SHA：

```markdown
- [x] 1.1 实现用户模型 (a1b2c3d)
```

#### 4.6 提交 plan.md 更新

```bash
git add compound/changes/{change_id}/plan.md
git commit -m "docs: 更新任务 1.1 状态"
```

### 步骤 5：阶段验证

当一个阶段的所有任务完成后：

#### 5.1 运行自动化测试

```bash
# 根据项目类型运行测试
npm test        # JavaScript/TypeScript
pytest          # Python
cargo test      # Rust
go test ./...   # Go
```

如果测试失败：
1. 尝试自动修复（最多 2 次）
2. 仍失败则停止，报告问题

#### 5.2 生成手动验证步骤

根据 `spec.md` 中的验收标准，生成手动验证步骤：

```
**阶段 1 验证**

自动化测试：✓ 通过

手动验证步骤：
1. 启动应用：`npm start`
2. 访问 http://localhost:3000/login
3. 输入测试用户凭据
4. 预期：成功登录并跳转到首页

请完成手动验证后选择：
- A) 验证通过
- B) 发现问题（请描述）

请选择：
```

#### 5.3 创建检查点 commit

```bash
git commit --allow-empty -m "checkpoint: 阶段 1 完成"
git notes add -m "Phase 1 completed
Verification: passed
Timestamp: <ISO8601>"
```

#### 5.4 更新 plan.md 检查点

```markdown
### 阶段 1: 用户认证

[checkpoint: a1b2c3d]
```

### 步骤 6：完成处理

当所有阶段完成：

#### 6.1 更新 metadata.json

```json
{
  "status": "completed",
  "completed_at": "<ISO8601>"
}
```

#### 6.2 同步文档

分析 `spec.md` 中的变更，询问是否更新：

```
**文档同步**

这次变更可能需要更新以下文档：

- compound/specs/index.md - 添加新模块链接
- compound/project.md - 更新功能列表（可选）

选项：
- A) 自动更新所有
- B) 逐个确认
- C) 跳过文档同步

请选择：
```

#### 6.3 创建规格文档（可选）

如果是新功能，询问是否在 `compound/specs/` 中创建独立的规格文档：

```
**创建规格文档**

是否将这次变更的规格整合到 specs/ 目录？

选项：
- A) 创建 compound/specs/user-auth.md
- B) 暂不创建

请选择：
```

#### 6.4 展示完成摘要

```
**变更执行完成！**

变更：2026-01-27-add-auth
类型：Feature
状态：completed

完成的任务：
- [x] 1.1 实现用户模型 (a1b2c3d)
- [x] 1.2 添加登录 API (e4f5g6h)
- [x] 2.1 编写单元测试 (i7j8k9l)
- [x] 2.2 集成测试 (m0n1o2p)

检查点：
- 阶段 1: a1b2c3d
- 阶段 2: q3r4s5t

文档更新：
- compound/specs/index.md ✓
- compound/project.md ✓

下一步：
1. 代码审查
2. 合并到主分支
3. 运行 /compoundcode:new 开始下一个变更
```

## TDD 工作流详解

### Red-Green-Refactor 循环

```
┌─────────────────────────────────────────┐
│                RED                      │
│  1. 写一个失败的测试                    │
│  2. 运行测试，看到失败                  │
│  3. 确认失败原因是功能缺失              │
└─────────────────┬───────────────────────┘
                  │
                  ▼
┌─────────────────────────────────────────┐
│               GREEN                     │
│  1. 写最小代码通过测试                  │
│  2. 不添加额外功能                      │
│  3. 运行测试，确认通过                  │
└─────────────────┬───────────────────────┘
                  │
                  ▼
┌─────────────────────────────────────────┐
│             REFACTOR                    │
│  1. 清理代码（可选）                    │
│  2. 移除重复                            │
│  3. 保持测试通过                        │
└─────────────────────────────────────────┘
```

### TDD 铁律

**没有失败的测试，就没有生产代码。**

每个任务必须：
1. 先写测试
2. 看到测试失败
3. 再写实现
4. 看到测试通过

### 验证清单

每个任务完成前检查：
- [ ] 每个新函数都有测试
- [ ] 看到每个测试失败
- [ ] 失败原因是预期的（功能缺失）
- [ ] 写最小代码通过
- [ ] 所有测试通过
- [ ] 输出干净（无错误/警告）

## 进度跟踪

### plan.md 任务状态

| 标记 | 状态 | 说明 |
|------|------|------|
| `[ ]` | 待做 | 尚未开始 |
| `[~]` | 进行中 | 正在执行 |
| `[x]` | 完成 | 已完成并提交 |

### commit SHA 记录

```markdown
- [x] 1.1 实现用户模型 (a1b2c3d)
                        ↑
                    commit SHA 前 7 位
```

### 阶段检查点

```markdown
### 阶段 1: 用户认证

- [x] 1.1 任务 A (a1b2c3d)
- [x] 1.2 任务 B (e4f5g6h)

[checkpoint: i7j8k9l]
```

## 错误处理

### 测试失败

1. 报告失败的测试
2. 尝试自动修复（最多 2 次）
3. 仍失败则停止，等待用户指导

### 提交失败

1. 检查 git 状态
2. 报告问题
3. 等待用户处理

### 中断恢复

如果执行中断：
1. 下次运行时检测未完成的任务（`[~]` 状态）
2. 询问是否从断点继续
3. 继续执行未完成的任务

## 参考文件

- **TDD 工作流详解**：见 [references/tdd-workflow.md](references/tdd-workflow.md)
- **进度跟踪格式**：见 [references/progress-tracking.md](references/progress-tracking.md)
