# Brownfield 项目分析策略

对于 Brownfield（已有代码）项目，分析现有代码以预填文档选项。

## 语言检测

### 文件扩展名统计

统计以下扩展名的文件数量：

| 扩展名 | 语言 |
|--------|------|
| .py | Python |
| .js, .mjs, .cjs | JavaScript |
| .ts, .tsx | TypeScript |
| .go | Go |
| .rs | Rust |
| .java | Java |
| .kt | Kotlin |
| .rb | Ruby |
| .php | PHP |
| .cs | C# |
| .cpp, .cc, .cxx, .h, .hpp | C++ |
| .c | C |
| .swift | Swift |
| .dart | Dart |

### 配置文件检测

| 文件 | 语言/运行时 |
|------|-------------|
| package.json | Node.js (JavaScript/TypeScript) |
| pyproject.toml, setup.py, requirements.txt | Python |
| Cargo.toml | Rust |
| go.mod | Go |
| pom.xml, build.gradle | Java |
| Gemfile | Ruby |
| composer.json | PHP |

## 框架检测

### JavaScript/TypeScript

检查 `package.json` 的 `dependencies` 和 `devDependencies`：

| 依赖 | 框架 |
|------|------|
| react, react-dom | React |
| vue | Vue.js |
| @angular/core | Angular |
| next | Next.js |
| nuxt | Nuxt.js |
| express | Express.js |
| @nestjs/core | NestJS |
| fastify | Fastify |
| koa | Koa |
| electron | Electron |

### Python

检查 `pyproject.toml` 或 `requirements.txt`：

| 依赖 | 框架 |
|------|------|
| django | Django |
| flask | Flask |
| fastapi | FastAPI |
| tornado | Tornado |
| pyramid | Pyramid |
| pytorch, torch | PyTorch |
| tensorflow | TensorFlow |
| pandas | Pandas |
| numpy | NumPy |

### Rust

检查 `Cargo.toml` 的 `[dependencies]`：

| 依赖 | 框架/库 |
|------|---------|
| actix-web | Actix Web |
| rocket | Rocket |
| axum | Axum |
| tokio | Tokio (异步运行时) |
| serde | Serde (序列化) |
| diesel | Diesel (ORM) |

### Go

检查 `go.mod`：

| 依赖 | 框架 |
|------|------|
| github.com/gin-gonic/gin | Gin |
| github.com/labstack/echo | Echo |
| github.com/gofiber/fiber | Fiber |
| github.com/gorilla/mux | Gorilla Mux |

## 架构分析

### 目录结构模式

| 模式 | 架构类型 |
|------|----------|
| src/, lib/, pkg/ | 标准项目结构 |
| cmd/, internal/, pkg/ | Go 项目结构 |
| app/, config/, routes/ | Web 应用 |
| services/, api/, models/ | 微服务/API |
| components/, pages/, hooks/ | React/前端应用 |
| src/main/, src/test/ | Java/Maven 结构 |

### 入口文件检测

| 文件 | 项目类型 |
|------|----------|
| main.py, app.py | Python 应用 |
| index.js, server.js, app.js | Node.js 应用 |
| main.go | Go 应用 |
| main.rs, lib.rs | Rust 应用 |
| Main.java | Java 应用 |

### 模块检测

分析顶级目录结构，识别核心模块：

1. 列出 `src/` 或项目根目录下的子目录
2. 排除常见非模块目录：`node_modules`, `__pycache__`, `.git`, `dist`, `build`, `target`
3. 将剩余目录作为潜在模块

## 代码风格检测

### Linter 配置文件

| 文件 | 工具 |
|------|------|
| .eslintrc, .eslintrc.js, .eslintrc.json | ESLint |
| .prettierrc, .prettierrc.js | Prettier |
| pyproject.toml (tool.black, tool.ruff) | Black, Ruff |
| .flake8, setup.cfg | Flake8 |
| rustfmt.toml, .rustfmt.toml | Rustfmt |
| .golangci.yml | GolangCI-Lint |
| .editorconfig | EditorConfig |

### 格式化配置

检查以下配置：

1. **缩进**：检查 `.editorconfig` 或代码文件
2. **引号风格**：检查 ESLint/Prettier 配置
3. **行长度**：检查 linter 配置

## 数据库检测

### 依赖文件检测

| 依赖 | 数据库 |
|------|--------|
| pg, postgres, psycopg2 | PostgreSQL |
| mysql, mysql2, pymysql | MySQL |
| mongodb, mongoose, pymongo | MongoDB |
| sqlite3, better-sqlite3 | SQLite |
| redis, ioredis | Redis |
| prisma | Prisma (多数据库) |
| sequelize | Sequelize (多数据库) |
| sqlalchemy | SQLAlchemy (多数据库) |
| diesel | Diesel (多数据库) |

### 配置文件检测

检查以下文件中的数据库配置：
- `.env`, `.env.example`
- `config/database.yml`
- `docker-compose.yml`

## 分析输出格式

分析完成后，生成以下结构：

```json
{
  "languages": [
    { "name": "TypeScript", "percentage": 65 },
    { "name": "JavaScript", "percentage": 35 }
  ],
  "frameworks": ["React", "Next.js", "Tailwind CSS"],
  "architecture": {
    "pattern": "前端应用",
    "modules": ["components", "pages", "hooks", "utils"]
  },
  "codestyle": {
    "linters": ["ESLint", "Prettier"],
    "config_files": [".eslintrc.js", ".prettierrc"]
  },
  "database": "PostgreSQL"
}
```
