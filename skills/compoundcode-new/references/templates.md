# 变更模板

本文件包含 compoundcode-new 生成的文档模板。

---

## spec.md 模板

### Feature 类型

```markdown
# {{CHANGE_NAME}}

## 概述

{{CHANGE_DESCRIPTION}}

## 类型

Feature（新功能）

## 背景

{{BACKGROUND}}

## 目标用户

{{TARGET_USERS}}

## 需求

### 功能需求

{{FUNCTIONAL_REQUIREMENTS}}

### 非功能需求

{{NON_FUNCTIONAL_REQUIREMENTS}}

## 验收标准

{{ACCEPTANCE_CRITERIA}}

## 技术考虑

{{TECHNICAL_CONSIDERATIONS}}

## 影响范围

{{AFFECTED_SCOPE}}

## 风险评估

{{RISK_ASSESSMENT}}
```

### Bug 类型

```markdown
# {{CHANGE_NAME}}

## 概述

{{CHANGE_DESCRIPTION}}

## 类型

Bug（问题修复）

## 问题描述

{{PROBLEM_DESCRIPTION}}

## 复现步骤

{{REPRODUCTION_STEPS}}

## 预期行为

{{EXPECTED_BEHAVIOR}}

## 实际行为

{{ACTUAL_BEHAVIOR}}

## 影响范围

{{AFFECTED_SCOPE}}

## 修复方案

{{FIX_APPROACH}}
```

### Chore 类型

```markdown
# {{CHANGE_NAME}}

## 概述

{{CHANGE_DESCRIPTION}}

## 类型

Chore（维护/重构）

## 改进范围

{{IMPROVEMENT_SCOPE}}

## 改进目标

{{IMPROVEMENT_GOAL}}

## 影响范围

{{AFFECTED_SCOPE}}

## 风险评估

{{RISK_ASSESSMENT}}
```

---

## plan.md 模板

```markdown
# {{CHANGE_NAME}} - 实施计划

## 概述

基于 [spec.md](./spec.md) 的实施计划。

**目标**: {{GOAL}}

**测试策略**: {{TEST_STRATEGY}}

## 任务分解

### 阶段 1: {{PHASE_1_NAME}}

**目标**: {{PHASE_1_GOAL}}

**任务**:
- [ ] 1.1 {{TASK_1_1}}
  - 文件: `{{FILE_PATH}}`
  - 步骤:
    1. {{STEP_1}}
    2. {{STEP_2}}
- [ ] 1.2 {{TASK_1_2}}

**验证**: {{PHASE_1_VERIFICATION}}

### 阶段 2: {{PHASE_2_NAME}}

**目标**: {{PHASE_2_GOAL}}

**任务**:
- [ ] 2.1 {{TASK_2_1}}
- [ ] 2.2 {{TASK_2_2}}

**验证**: {{PHASE_2_VERIFICATION}}

### 阶段 3: 测试与验收

**任务**:
- [ ] 3.1 运行单元测试
- [ ] 3.2 运行集成测试
- [ ] 3.3 代码审查
- [ ] 3.4 更新文档

**验证**: 所有测试通过，文档已更新

## 依赖

{{DEPENDENCIES}}

## 进度跟踪

| 任务 | 状态 | 完成时间 | Commit |
|------|------|----------|--------|
| 1.1 {{TASK_1_1}} | [ ] | - | - |
| 1.2 {{TASK_1_2}} | [ ] | - | - |
| 2.1 {{TASK_2_1}} | [ ] | - | - |
| 2.2 {{TASK_2_2}} | [ ] | - | - |
| 3.1 单元测试 | [ ] | - | - |
| 3.2 集成测试 | [ ] | - | - |
| 3.3 代码审查 | [ ] | - | - |
| 3.4 文档更新 | [ ] | - | - |

## 备注

{{NOTES}}
```

---

## metadata.json 模板

```json
{
  "id": "{{CHANGE_ID}}",
  "type": "{{CHANGE_TYPE}}",
  "status": "planned",
  "created_at": "{{CREATED_AT}}",
  "title": "{{CHANGE_TITLE}}",
  "description": "{{CHANGE_DESCRIPTION}}",
  "author": "{{AUTHOR}}",
  "tags": {{TAGS}},
  "priority": "{{PRIORITY}}",
  "estimated_effort": "{{ESTIMATED_EFFORT}}"
}
```

### 字段说明

| 字段 | 类型 | 说明 |
|------|------|------|
| id | string | 变更唯一标识，格式：YYYY-MM-DD-name |
| type | string | 变更类型：feature, bug, chore |
| status | string | 状态：planned, in_progress, completed, cancelled |
| created_at | string | 创建时间，ISO8601 格式 |
| title | string | 变更标题 |
| description | string | 变更描述 |
| author | string | 创建者 |
| tags | array | 标签列表 |
| priority | string | 优先级：low, medium, high, critical |
| estimated_effort | string | 预估工作量：small, medium, large |

---

## 模板变量说明

| 变量 | 说明 | 来源 |
|------|------|------|
| `{{CHANGE_NAME}}` | 变更名称 | 用户输入 |
| `{{CHANGE_DESCRIPTION}}` | 变更描述 | 用户输入 |
| `{{CHANGE_ID}}` | 变更 ID | 自动生成 |
| `{{CHANGE_TYPE}}` | 变更类型 | 用户选择 |
| `{{BACKGROUND}}` | 背景说明 | 用户输入 |
| `{{TARGET_USERS}}` | 目标用户 | 用户选择 |
| `{{FUNCTIONAL_REQUIREMENTS}}` | 功能需求 | 从问答收集 |
| `{{NON_FUNCTIONAL_REQUIREMENTS}}` | 非功能需求 | 从问答收集 |
| `{{ACCEPTANCE_CRITERIA}}` | 验收标准 | 从问答收集 |
| `{{TECHNICAL_CONSIDERATIONS}}` | 技术考虑 | 用户输入 |
| `{{AFFECTED_SCOPE}}` | 影响范围 | 分析或用户输入 |
| `{{RISK_ASSESSMENT}}` | 风险评估 | 分析或用户输入 |
| `{{GOAL}}` | 计划目标 | 从 spec 提取 |
| `{{TEST_STRATEGY}}` | 测试策略 | 用户选择 |
| `{{PHASE_N_NAME}}` | 阶段名称 | 自动生成或用户输入 |
| `{{PHASE_N_GOAL}}` | 阶段目标 | 自动生成 |
| `{{TASK_N_N}}` | 任务描述 | 自动生成 |
| `{{FILE_PATH}}` | 文件路径 | 分析或用户输入 |
| `{{DEPENDENCIES}}` | 依赖项 | 分析或用户输入 |
| `{{CREATED_AT}}` | 创建时间 | 自动生成 |
| `{{AUTHOR}}` | 作者 | 从 git config 或用户输入 |
| `{{TAGS}}` | 标签 | 用户输入 |
| `{{PRIORITY}}` | 优先级 | 用户选择 |
| `{{ESTIMATED_EFFORT}}` | 预估工作量 | 用户选择 |
