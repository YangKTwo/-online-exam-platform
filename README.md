# 在线考试系统（前后端分离）

一个面向高校教学场景的前后端分离系统，支持题库管理、组卷、考试、评卷、成绩分析与 AI 助手能力，并采用网关 + 微服务架构实现权限与业务解耦。

## 核心特性

- 多角色权限体系：管理员 / 教师 / 学生，基于权限码控制菜单与页面访问。
- 覆盖考试全流程：题库、试卷、考试发布、答题、评卷、成绩与报告。
- 微服务架构：Spring Cloud Gateway + Nacos 服务发现 + 多业务服务拆分。
- AI 能力集成：AI 助手、智能组卷、评测报告生成等。
- 前端工程化完善：Vue 3 + TypeScript + Vite + Pinia + Element Plus。

## 技术栈

| 技术栈    | 前端                         | 后端                                    |
| ------ | -------------------------- | ------------------------------------- |
| 核心框架   | Vue 3                      | Spring Boot 3.1.5 + Spring Cloud 2022 |
| 开发语言   | TypeScript                 | Java 17                               |
| UI 组件库 | Element Plus               | -                                     |
| 状态管理   | Pinia                      | -                                     |
| 构建工具   | Vite 7 + vue-tsc           | Maven 多模块                             |
| 服务治理   | -                          | Nacos（服务发现）                           |
| 网关     | -                          | Spring Cloud Gateway                  |
| 数据访问   | Axios                      | MyBatis-Plus                          |
| 数据库    | -                          | MySQL 8                               |
| 缓存     | -                          | Redis                                 |
| API 文档 | -                          | SpringDoc OpenAPI / Swagger UI        |
| 其他     | ECharts, CodeMirror, KaTeX | Spring Cloud Alibaba, Feign           |

## 项目结构

> 当前代码为前后端分仓结构：  
> 前端目录：`online-eaxm-frontend`  
> 后端目录：`online-exam-sys2.0`

```text
project-root/
├── online-eaxm-frontend/              # 前端（Vue3）
│   ├── public/
│   ├── scripts/                       # 指标/脚本工具
│   ├── src/
│   │   ├── api/                       # 接口封装
│   │   ├── components/                # 公共组件
│   │   ├── config/                    # 前端配置
│   │   ├── constants/                 # 常量与权限码
│   │   ├── hooks/                     # 通用 hooks
│   │   ├── layouts/                   # 布局组件
│   │   ├── router/                    # 路由与守卫
│   │   ├── services/                  # 服务封装
│   │   ├── shared/                    # 共享组件/hooks/utils
│   │   ├── store/                     # Pinia 状态管理
│   │   ├── styles/                    # 样式与 tokens
│   │   ├── types/                     # TS 类型定义
│   │   ├── utils/                     # 工具函数（含请求封装）
│   │   └── views/                     # 页面模块（admin/teacher/student/exam...）
│   ├── .env.development
│   ├── package.json
│   └── vite.config.ts
└── online-exam-sys2.0/                # 后端（Spring Cloud 微服务）
    ├── exam-gateway/                  # 网关服务（统一 API 入口）
    ├── exam-common/                   # 公共模块
    ├── exam-user/                     # 用户与权限服务
    ├── exam-question/                 # 题目/科目/标签服务
    ├── exam-paper/                    # 试卷与组卷服务
    ├── exam-exam/                     # 考试/评卷/报告服务
    ├── exam-class/                    # 班级与成员服务
    ├── exam-ai/                       # AI 助手与 AI 能力服务
    ├── .mvn/
    └── pom.xml                        # Maven 父工程
```

## 功能模块概览

### 前端功能

- 用户认证：登录、注册、个人中心。
- 管理员：教师审核、教师管理、班级学科管理、教师分配、权限管理。
- 教师端：题库管理、标签管理、组卷、考试管理、评卷管理、成绩分析。
- 学生端：我的考试、在线答题、考试结果、成绩查询、错题本。
- AI 能力：AI 助手、部分考试/题目相关智能能力页面。

### 后端微服务职责

- `exam-gateway`：统一入口（默认 `:8080`），路由 `/api/**` 到各业务服务。
- `exam-user`：用户、权限、教师审核、文件上传。
- `exam-question`：题目、科目、标签、统计相关接口。
- `exam-paper`：试卷、试卷题目、智能组卷。
- `exam-exam`：考试发布、考试过程、答卷、评卷、统计、报告。
- `exam-class`：班级、班级成员、班级教师/学科关系。
- `exam-ai`：AI 助手、AI 相关能力接口。

## 环境要求

### 基础依赖

- Node.js：建议 `>= 18`（匹配 Vite 7 生态）。
- 包管理器：`pnpm`（仓库含 `pnpm-lock.yaml`）。
- JDK：`17`（父 `pom.xml` 指定）。
- Maven：`3.8+`（建议）。
- MySQL：`8.x`（工程中使用 MySQL 8 驱动）。
- Redis：`6.x/7.x`（默认本地 `6379`）。
- Nacos：`2.x`（默认 `127.0.0.1:8848`）。

### 默认端口（开发环境）

- 前端 Vite：`5173`（默认）
- 网关：`8080`
- question：`8081`
- paper：`8083`
- user：`8084`
- class：`8085`
- exam：`8086`
- ai：`8087`
- MySQL：`3306`
- Redis：`6379`
- Nacos：`8848`

## 安装与运行

### 1) 克隆项目

```bash
# 前端
git clone <frontend-repo-url>

# 后端
git clone <backend-repo-url>
```

### 2) 启动后端（先启动）

```bash
# 进入后端目录
cd online-exam-sys2.0

# 构建所有模块
mvn clean install -DskipTests
```

建议按以下顺序启动（可在 IDE 中逐个运行各模块 `*Application`）：

1. `exam-gateway`
2. `exam-user`
3. `exam-question`
4. `exam-paper`
5. `exam-class`
6. `exam-exam`
7. `exam-ai`

> 说明：若你依赖 Nacos 服务发现，请先启动 Nacos，再启动各服务。

### 3) 启动前端

```bash
cd online-eaxm-frontend

# 安装依赖（推荐 pnpm）
pnpm install

# 启动开发环境
pnpm dev
```

默认通过 Vite 代理将 `/api` 请求转发到 `http://localhost:8080` 网关。


| 变量名            | 说明            | 示例值                       |
| -------------- | ------------- | ------------------------- |
| `VITE_ENV`     | 运行环境标识        | `development`             |
| `VITE_API_URL` | 生产环境 API 基础地址 | `https://api.example.com` |

### 后端（建议写入配置中心或环境变量，不要硬编码）

| 变量名                     | 说明            | 示例值                           |
| ----------------------- | ------------- | ----------------------------- |
| `DB_HOST`               | MySQL 地址      | `localhost`                   |
| `DB_PORT`               | MySQL 端口      | `3306`                        |
| `DB_USER`               | MySQL 用户名     | `root`                        |
| `DB_PASSWORD`           | MySQL 密码      | `your-password`               |
| `REDIS_HOST`            | Redis 地址      | `localhost`                   |
| `REDIS_PORT`            | Redis 端口      | `6379`                        |
| `NACOS_ADDR`            | Nacos 地址      | `127.0.0.1:8848`              |
| `JWT_SECRET`            | JWT 密钥        | `replace-with-strong-secret`  |
| `OSS_ENDPOINT`          | 对象存储 endpoint | `https://oss-xx.aliyuncs.com` |
| `OSS_ACCESS_KEY_ID`     | OSS AK        | `your-ak`                     |
| `OSS_ACCESS_KEY_SECRET` | OSS SK        | `your-sk`                     |
| `OSS_BUCKET`            | OSS 存储桶       | `your-bucket`                 |
| `QWEN_API_KEY`          | AI 平台 Key     | `sk-***`                      |

## API 文档

- 项目已接入 SpringDoc OpenAPI（检测到 `swagger-ui` 与 `v3/api-docs` 配置）。
- 常用访问入口（服务启动后）：
  - 网关聚合访问：`http://localhost:8080`
  - 服务级文档示例：
    - `http://localhost:8084/swagger-ui.html`（exam-user）
    - `http://localhost:8086/swagger-ui.html`（exam-exam）
    - `http://localhost:8087/swagger-ui.html`（exam-ai）

### 认证方式

- 前端在请求头使用：`Authorization: Bearer <token>`。
- 受保护接口需携带 JWT。

## 部署指南（建议）

### 前端构建

```bash
cd online-eaxm-frontend
pnpm build
```

构建产物默认在 `dist/`，可部署到 Nginx 或静态托管平台。

### 后端构建

```bash
cd online-exam-sys2.0
mvn clean package -DskipTests
```

按模块部署各服务 Jar，并保证 MySQL/Redis/Nacos 可访问。

### Nginx 反向代理示例（前后端分离）

```nginx
server {
    listen 80;
    server_name your-domain.com;

    location / {
        root /var/www/online-eaxm-frontend/dist;
        index index.html;
        try_files $uri $uri/ /index.html;
    }

    location /api/ {
        proxy_pass http://127.0.0.1:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

## 常见问题（FAQ）

### 1. 前端请求 404 / 接口不通

- 检查网关 `8080` 是否启动。
- 检查前端 `vite.config.ts` 代理是否指向 `http://localhost:8080`。
- 检查 Nacos 是否注册到对应服务实例。

### 2. 401 / 403 权限问题

- 确认是否登录并携带 `Authorization` Token。
- 检查当前角色是否拥有对应 `permission_code`。
- 检查路由 `meta.permission` 与后端权限码是否一致。

### 3. 数据库连接失败

- 确认 MySQL 已启动、端口 `3306` 可访问。
- 检查各服务配置中的库名（`exam_user`/`exam_exam`/`exam_class`/`exam_ai` 等）。
- 检查账号密码与字符集/时区配置。

### 4. Redis 或 Nacos 连接失败

- Redis 默认 `localhost:6379`；Nacos 默认 `127.0.0.1:8848`。
- 确认本机服务已启动并且配置一致。

### 5. 端口冲突

- 修改对应模块 `application.yml` 的 `server.port`。
- 同步更新前端代理或网关路由配置。
