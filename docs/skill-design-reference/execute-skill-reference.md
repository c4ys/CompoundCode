# compoundcode-execute 参考分析

## 参考项目

### Superpowers Executing-Plans Skill

**文件位置**：`~/.claude/plugins/cache/claude-plugins-official/superpowers/*/skills/executing-plans/SKILL.md`

#### 核心工作流程

```
加载计划 → 批量执行（默认3个任务）→ 报告反馈 → 继续下一批 → 完成后调用 finishing-a-development-branch
```

#### 关键特点

1. **批量执行模式**：每批 3 个任务，中间有检查点
2. **人工审查循环**：每批完成后暂停，等待反馈
3. **子技能集成**：依赖 `test-driven-development` 和 `finishing-a-development-branch` 技能
4. **简化的任务跟踪**：使用 TodoWrite 工具（任务列表）
5. **灵活的停止机制**：遇到阻塞立即停止，不强行推进

#### 停止条件

立即停止的情况：
- 遇到阻塞（缺失依赖、测试失败、指令不清）
- 计划有关键缺陷
- 验证失败

处理方式：
```
停止 → 报告问题 → 等待用户指导
```

---

### Superpowers TDD Skill

**文件位置**：`~/.claude/plugins/cache/claude-plugins-official/superpowers/*/skills/test-driven-development/SKILL.md`

#### Red-Green-Refactor 循环

**RED 阶段**：
1. 写一个失败的测试
2. 验证测试确实失败（必须看到失败）
3. 失败原因必须是功能缺失，不是拼写错误

**GREEN 阶段**：
1. 写最小代码通过测试
2. 不添加额外功能
3. 不重构其他代码
4. 只做通过测试所需的最少工作

**REFACTOR 阶段**：
1. 移除重复
2. 改进名称
3. 提取辅助函数
4. 保持测试通过

#### TDD 铁律

**没有失败的测试，就没有生产代码。**

验证清单：
- 每个新函数都有测试
- 看到每个测试失败
- 每个测试失败的原因是预期的
- 写最小代码通过
- 所有测试通过
- 输出干净（无错误/警告）

---

### Conductor Implement (`/conductor:implement`)

**文件位置**：`~/.gemini/extensions/conductor/commands/conductor/implement.toml`

#### 核心工作流程

```
验证环境 → 选择 Track → 更新状态 → 加载上下文 → 逐个执行任务 → 同步文档 → 清理 Track
```

#### 关键特点

1. **严格的协议驱动**：每一步都有明确的检查点和验证
2. **完整的上下文管理**：
   - 读取 `plan.md`、`spec.md`、`workflow.md`
   - 遵循 `workflow.md` 中的 TDD 流程
3. **任务级别的详细跟踪**：
   - 每个任务标记为 `[ ]`（待做）、`[~]`（进行中）、`[x]`（完成）
   - 记录 commit SHA（前 7 位）
   - 附加 git notes 作为审计日志
4. **阶段完成验证**：
   - 自动化测试验证
   - 手动验证步骤
   - 用户确认
   - 创建检查点 commit
5. **文档同步**：
   - 完成后自动更新 `product.md`、`tech-stack.md`
   - 需要用户确认

#### 任务工作流

```
1. 选择任务
2. 标记为进行中 [~]
3. 写失败测试（RED）
4. 实现通过测试（GREEN）
5. 重构（可选）
6. 验证覆盖率 >80%
7. 检查技术栈偏差
8. 提交代码
9. 附加 git notes（任务总结）
10. 更新 plan.md，记录 commit SHA
11. 提交 plan.md 更新
```

#### 阶段完成验证

```
1. 宣布阶段完成
2. 确保测试覆盖
   - 获取前一阶段的 checkpoint SHA
   - 运行 git diff 获取变更文件列表
   - 为每个代码文件验证测试存在
   - 缺失测试则创建

3. 执行自动化测试
   - 宣布将运行的命令
   - 执行测试
   - 失败则调试（最多 2 次尝试）
   - 持续失败则停止并请求帮助

4. 生成手动验证计划
   - 分析 product.md、product-guidelines.md、plan.md
   - 生成分步骤的验证步骤
   - 包括具体命令和预期结果

5. 等待用户确认

6. 创建检查点 commit
   - 附加详细的验证报告作为 git notes

7. 更新 plan.md，记录 checkpoint SHA

8. 提交 plan.md 更新
```

#### 验证失败处理

```
测试失败 → 调试（最多 2 次尝试）→ 仍失败 → 停止并请求帮助
```

#### 文档同步规则

- `product.md`：如果功能显著影响产品描述
- `tech-stack.md`：如果技术栈有变化
- `product-guidelines.md`：仅在品牌/语调变化时（需谨慎）

---

### Conductor Workflow 模板

**文件位置**：`~/.gemini/extensions/conductor/templates/workflow.md`

#### 核心原则

- "The Plan is the Source of Truth"
- 测试驱动开发（TDD）
- 代码覆盖率 >80%

#### 质量门

- 测试覆盖率
- 代码风格合规
- 文档
- 类型安全
- Linting
- 移动功能
- 安全审查

---

## CompoundCode Execute 设计决策

### 核心架构

结合两个参考实现的优点：

```
┌─────────────────────────────────────────────────────────┐
│ compoundcode-execute Skill                              │
├─────────────────────────────────────────────────────────┤
│ 1. 环境验证                                             │
│    ├─ 检查 compound/ 目录结构                           │
│    ├─ 验证 AGENTS.md、project.md 等存在                │
│    └─ 检查 changes/ 目录中的变更                        │
│                                                         │
│ 2. 变更选择                                             │
│    ├─ 列出所有待执行的变更                              │
│    ├─ 自动选择最早的未完成变更                          │
│    └─ 支持用户指定变更                                  │
│                                                         │
│ 3. 上下文加载                                           │
│    ├─ 读取 spec.md（需求规格）                         │
│    ├─ 读取 plan.md（实施计划）                         │
│    ├─ 读取 workflow.md（工作流指导）                   │
│    └─ 读取 AGENTS.md（项目上下文）                     │
│                                                         │
│ 4. 任务执行                                             │
│    ├─ 遵循 TDD 流程（Red-Green-Refactor）              │
│    ├─ 更新 plan.md 中的任务状态                        │
│    ├─ 记录 commit SHA                                  │
│    └─ 附加 git notes                                   │
│                                                         │
│ 5. 阶段验证                                             │
│    ├─ 运行自动化测试                                    │
│    ├─ 生成手动验证步骤                                  │
│    ├─ 等待用户确认                                      │
│    └─ 创建检查点 commit                                │
│                                                         │
│ 6. 完成处理                                             │
│    ├─ 更新 specs/ 中的规格                             │
│    ├─ 同步 project.md 等文档                           │
│    └─ 提供清理选项                                      │
└─────────────────────────────────────────────────────────┘
```

### 关键设计决策

| 方面 | 选择 | 理由 |
|------|------|------|
| 执行模式 | 逐个任务 | 更细粒度的控制和反馈 |
| 进度跟踪 | plan.md + git | 持久化 + 审计日志 |
| 验证方式 | 自动 + 手动 | 平衡效率和质量 |
| 错误处理 | 有限重试 + 停止 | 防止无限循环 |
| 文档同步 | 自动分析 + 用户确认 | 保证准确性 |
| 批量处理 | 支持但可选 | 灵活适应不同场景 |

### 进度跟踪机制

采用 Conductor 的方式：

**plan.md 任务状态**：
| 标记 | 状态 | 说明 |
|------|------|------|
| `[ ]` | pending | 待做 |
| `[~]` | in_progress | 进行中 |
| `[x]` | completed | 已完成 |

**commit SHA 记录**：
```markdown
- [x] 1.1 任务描述 (a1b2c3d)
```

**阶段检查点**：
```markdown
### 阶段 1: 用户认证

[checkpoint: i7j8k9l]
```

### 与参考实现的对比

| 特性 | Executing-Plans | Implement | CompoundCode Execute |
|------|-----------------|-----------|---------------------|
| 执行模式 | 批量（3个） | 逐个 | 逐个 |
| 进度跟踪 | TodoWrite | plan.md + git | plan.md + git |
| TDD 集成 | 调用子技能 | 内置 | 内置 |
| 验证方式 | 简化 | 详细 | 详细 |
| 文档同步 | 无 | 自动 + 确认 | 自动 + 确认 |
| 断点续传 | 无 | 有 | 有 |
| git notes | 无 | 有 | 有 |
