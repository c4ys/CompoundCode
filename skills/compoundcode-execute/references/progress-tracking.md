# 进度跟踪格式

本文档详细说明 compoundcode-execute 中的进度跟踪机制。

## plan.md 任务状态标记

### 状态定义

| 标记 | 状态 | 说明 |
|------|------|------|
| `[ ]` | pending | 待做，尚未开始 |
| `[~]` | in_progress | 进行中，正在执行 |
| `[x]` | completed | 已完成，已提交代码 |
| `[!]` | blocked | 被阻塞，需要解决问题 |
| `[-]` | skipped | 已跳过，不再需要 |

### 状态转换

```
[ ] pending
    │
    ▼ 开始执行
[~] in_progress
    │
    ├──▶ 执行完成 ──▶ [x] completed
    │
    ├──▶ 遇到阻塞 ──▶ [!] blocked
    │                    │
    │                    ▼ 问题解决
    │                [~] in_progress
    │
    └──▶ 决定跳过 ──▶ [-] skipped
```

## Commit SHA 记录

### 格式

```markdown
- [x] 1.1 任务描述 (a1b2c3d)
                    ↑
              commit SHA 前 7 位
```

### 获取 commit SHA

```bash
# 获取最新提交的 SHA
git rev-parse --short HEAD

# 输出示例：a1b2c3d
```

### 记录位置

在任务描述后、括号内记录：

```markdown
### 阶段 1: 基础设施

- [x] 1.1 创建数据库模型 (a1b2c3d)
- [x] 1.2 实现 API 端点 (e4f5g6h)
- [~] 1.3 添加验证逻辑
- [ ] 1.4 编写文档
```

## 阶段检查点

### 检查点格式

```markdown
### 阶段 1: 基础设施

- [x] 1.1 任务 A (a1b2c3d)
- [x] 1.2 任务 B (e4f5g6h)

[checkpoint: i7j8k9l]
```

### 创建检查点

当一个阶段的所有任务完成并验证通过后：

```bash
# 创建空提交作为检查点
git commit --allow-empty -m "checkpoint: 阶段 1 完成"

# 获取检查点 SHA
git rev-parse --short HEAD
```

### 检查点 git notes

```bash
git notes add -m "Phase 1 Checkpoint
========================
Tasks completed: 2
Tests: passed
Manual verification: passed
Timestamp: 2026-01-27T10:30:00Z"
```

## metadata.json 更新

### 初始状态

```json
{
  "id": "2026-01-27-add-auth",
  "type": "feature",
  "status": "planned",
  "created_at": "2026-01-27T09:00:00Z",
  "title": "添加用户认证",
  "description": "实现用户登录和注册功能"
}
```

### 执行中状态

```json
{
  "id": "2026-01-27-add-auth",
  "type": "feature",
  "status": "in_progress",
  "created_at": "2026-01-27T09:00:00Z",
  "started_at": "2026-01-27T10:00:00Z",
  "title": "添加用户认证",
  "description": "实现用户登录和注册功能",
  "current_phase": 1,
  "current_task": "1.2"
}
```

### 完成状态

```json
{
  "id": "2026-01-27-add-auth",
  "type": "feature",
  "status": "completed",
  "created_at": "2026-01-27T09:00:00Z",
  "started_at": "2026-01-27T10:00:00Z",
  "completed_at": "2026-01-27T15:00:00Z",
  "title": "添加用户认证",
  "description": "实现用户登录和注册功能",
  "checkpoints": [
    { "phase": 1, "sha": "i7j8k9l", "timestamp": "2026-01-27T12:00:00Z" },
    { "phase": 2, "sha": "m0n1o2p", "timestamp": "2026-01-27T14:30:00Z" }
  ],
  "commits": [
    { "task": "1.1", "sha": "a1b2c3d" },
    { "task": "1.2", "sha": "e4f5g6h" },
    { "task": "2.1", "sha": "q3r4s5t" },
    { "task": "2.2", "sha": "u6v7w8x" }
  ]
}
```

## Git Notes 审计日志

### 任务完成 note

```bash
git notes add -m "Task: 1.1 创建数据库模型
Change: 2026-01-27-add-auth
Status: completed
Files changed:
  - src/models/user.py (created)
  - tests/test_user.py (created)
Tests: passed
Timestamp: 2026-01-27T10:30:00Z"
```

### 阶段检查点 note

```bash
git notes add -m "Phase 1 Checkpoint
========================
Change: 2026-01-27-add-auth
Tasks completed: 2/2
  - 1.1 创建数据库模型 (a1b2c3d)
  - 1.2 实现 API 端点 (e4f5g6h)

Automated tests: passed
Manual verification: passed
  - Login flow verified
  - Registration flow verified

Timestamp: 2026-01-27T12:00:00Z"
```

### 查看 git notes

```bash
# 查看特定提交的 note
git notes show <commit_sha>

# 查看所有 notes
git log --show-notes
```

## 进度报告

### 实时进度

在执行过程中显示：

```
**执行进度**

变更：2026-01-27-add-auth
阶段：1/2
任务：2/4

当前任务：1.2 实现 API 端点

[██████████░░░░░░░░░░] 50%
```

### 阶段完成报告

```
**阶段 1 完成**

完成的任务：
- [x] 1.1 创建数据库模型 (a1b2c3d)
- [x] 1.2 实现 API 端点 (e4f5g6h)

验证结果：
- 自动化测试：✓ 通过 (12 个测试)
- 手动验证：✓ 通过

检查点：i7j8k9l

下一阶段：阶段 2 - 测试和文档
```

### 最终完成报告

```
**变更执行完成**

变更：2026-01-27-add-auth
类型：Feature
状态：completed

时间线：
- 创建时间：2026-01-27 09:00
- 开始执行：2026-01-27 10:00
- 完成时间：2026-01-27 15:00
- 总耗时：5 小时

完成的任务：4/4
- [x] 1.1 创建数据库模型 (a1b2c3d)
- [x] 1.2 实现 API 端点 (e4f5g6h)
- [x] 2.1 编写单元测试 (q3r4s5t)
- [x] 2.2 编写集成测试 (u6v7w8x)

检查点：
- 阶段 1: i7j8k9l
- 阶段 2: y9z0a1b

测试覆盖率：85%

文档更新：
- compound/specs/index.md ✓
- compound/project.md ✓
```

## 断点续传

### 检测中断

检查 `plan.md` 中是否有 `[~]` 状态的任务：

```bash
grep -n "\[~\]" compound/changes/*/plan.md
```

### 恢复执行

如果检测到中断的执行：

```
**检测到未完成的执行**

变更：2026-01-27-add-auth
中断位置：阶段 1，任务 1.2

选项：
- A) 从断点继续
- B) 重新开始当前任务
- C) 放弃执行

请选择：
```

### 恢复点记录

在 `metadata.json` 中记录恢复点：

```json
{
  "resume_point": {
    "phase": 1,
    "task": "1.2",
    "timestamp": "2026-01-27T11:30:00Z"
  }
}
```
