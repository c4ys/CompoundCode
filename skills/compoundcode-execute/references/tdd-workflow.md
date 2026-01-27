# TDD 工作流详解

本文档详细说明 compoundcode-execute 中的 TDD（测试驱动开发）工作流。

## Red-Green-Refactor 循环

### 1. RED 阶段：写失败的测试

**目标**：定义预期行为

**步骤**：
1. 理解任务需求
2. 设计测试用例
3. 编写测试代码
4. 运行测试
5. **确认测试失败**

**关键点**：
- 失败原因必须是"功能不存在"
- 不是语法错误、拼写错误或配置问题
- 测试应该清晰地描述预期行为

**示例**：
```python
# test_user.py
def test_user_can_login():
    """用户应该能够使用正确的凭据登录"""
    user = User(email="test@example.com", password="password123")
    result = user.authenticate("password123")
    assert result is True
```

运行测试：
```bash
pytest test_user.py -v
```

预期输出（RED）：
```
FAILED test_user.py::test_user_can_login - NameError: name 'User' is not defined
```

### 2. GREEN 阶段：写最小代码通过测试

**目标**：让测试通过，不多不少

**步骤**：
1. 写最简单的代码让测试通过
2. 不添加额外功能
3. 不考虑"未来可能需要"的功能
4. 运行测试
5. **确认测试通过**

**关键点**：
- YAGNI（You Ain't Gonna Need It）
- 只做让测试通过所需的最少工作
- 不要过度设计

**示例**：
```python
# user.py
class User:
    def __init__(self, email, password):
        self.email = email
        self.password = password

    def authenticate(self, password):
        return self.password == password
```

运行测试：
```bash
pytest test_user.py -v
```

预期输出（GREEN）：
```
PASSED test_user.py::test_user_can_login
```

### 3. REFACTOR 阶段：清理代码

**目标**：改进代码质量，保持测试通过

**步骤**：
1. 识别可改进的地方
2. 小步重构
3. 每次改动后运行测试
4. **确保测试始终通过**

**可重构的方面**：
- 移除重复代码
- 改进命名
- 提取辅助函数
- 简化逻辑
- 改进结构

**示例**：
```python
# user.py（重构后）
import hashlib

class User:
    def __init__(self, email: str, password: str):
        self.email = email
        self._password_hash = self._hash_password(password)

    def authenticate(self, password: str) -> bool:
        """验证用户密码"""
        return self._password_hash == self._hash_password(password)

    @staticmethod
    def _hash_password(password: str) -> str:
        """对密码进行哈希处理"""
        return hashlib.sha256(password.encode()).hexdigest()
```

运行测试：
```bash
pytest test_user.py -v
```

预期输出（仍然 GREEN）：
```
PASSED test_user.py::test_user_can_login
```

## TDD 铁律

### 规则 1：没有失败的测试，就没有生产代码

在写任何生产代码之前，必须先有一个失败的测试。

### 规则 2：只写刚好让测试失败的测试代码

不要写超出当前需求的测试。

### 规则 3：只写刚好让测试通过的生产代码

不要写超出当前测试需求的代码。

## 常见测试类型

### 单元测试

测试单个函数或方法的行为。

```python
def test_add_numbers():
    assert add(2, 3) == 5
```

### 集成测试

测试多个组件的协作。

```python
def test_user_registration_flow():
    user = register_user("test@example.com", "password")
    assert user.is_active
    assert send_welcome_email(user) is True
```

### 端到端测试

测试完整的用户流程。

```python
def test_login_and_view_dashboard():
    client = TestClient(app)
    response = client.post("/login", data={"email": "test@example.com", "password": "password"})
    assert response.status_code == 302
    response = client.get("/dashboard")
    assert "Welcome" in response.text
```

## 测试命令参考

### Python (pytest)

```bash
# 运行所有测试
pytest

# 运行特定文件
pytest test_user.py

# 运行特定测试
pytest test_user.py::test_user_can_login

# 详细输出
pytest -v

# 显示 print 输出
pytest -s

# 覆盖率报告
pytest --cov=src --cov-report=term-missing
```

### JavaScript/TypeScript (Jest)

```bash
# 运行所有测试
npm test

# 运行特定文件
npm test -- user.test.js

# 监听模式
npm test -- --watch

# 覆盖率报告
npm test -- --coverage
```

### Rust (cargo)

```bash
# 运行所有测试
cargo test

# 运行特定测试
cargo test test_user_can_login

# 显示输出
cargo test -- --nocapture
```

### Go

```bash
# 运行所有测试
go test ./...

# 运行特定包
go test ./user

# 详细输出
go test -v ./...

# 覆盖率
go test -cover ./...
```

## 测试覆盖率目标

| 级别 | 覆盖率 | 说明 |
|------|--------|------|
| 最低 | 60% | 基础保障 |
| 推荐 | 80% | 良好实践 |
| 理想 | 90%+ | 高质量代码 |

## 常见问题

### Q: 测试应该多细粒度？

**A**: 测试应该覆盖行为，而不是实现细节。一个测试应该验证一个行为。

### Q: 什么时候可以跳过 TDD？

**A**: 在以下情况可以考虑：
- 探索性代码（原型）
- 简单的配置更改
- 第三方库的简单封装

但核心业务逻辑应该始终使用 TDD。

### Q: 如何处理遗留代码？

**A**:
1. 先为现有代码写"表征测试"（characterization tests）
2. 确保测试覆盖现有行为
3. 然后再进行修改

### Q: 测试太慢怎么办？

**A**:
1. 使用模拟（mock）减少外部依赖
2. 分离单元测试和集成测试
3. 只在 CI 中运行完整测试套件
