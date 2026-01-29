# COLA DDD Skill

> Claude Code Skills 集合，提供 COLA 整洁分层架构设计指南。

---

## 项目简介

COLA DDD Skill 是一个 Claude Code Skills 集合，包含 COLA（Clean Object-Oriented and Layered Architecture）架构的 Python 和 Java 版本实现指南。

本项目适用于：
- 使用 DDD 分层架构设计后端应用
- 重构现有项目到整洁架构
- 学习 COLA 架构的最佳实践
- 在 Python/Flask/FastAPI 或 Java/Spring Boot 项目中应用分层架构

## 包含的 Skills

| Skill | 描述 | 适用场景 |
|-------|------|----------|
| [python-cola-skill](./python-cola-skill/) | Python 版 COLA 架构指南 | Flask、FastAPI 项目 |
| [java-cola-skill](./java-cola-skill/) | Java 版 COLA 架构指南 | Spring Boot 项目 |

## 安装

### 方法一：通过 Git 克隆（推荐）

```bash
# 克隆整个仓库到 skills 目录
git clone https://github.com/lailoo/cola-DDD-skill.git ~/.claude/skills/cola-DDD-skill
```

或者只安装单个 skill：

```bash
# 只安装 Python COLA Skill
git clone https://github.com/lailoo/cola-DDD-skill.git /tmp/cola-skill
cp -r ~/cola-skill/python-cola-skill ~/.claude/skills/

# 只安装 Java COLA Skill
git clone https://github.com/lailoo/cola-DDD-skill.git ~/cola-skill
cp -r ~/cola-skill/java-cola-skill ~/.claude/skills/
```

### 方法二：手动安装

1. 下载本项目的 ZIP 文件或克隆到本地
2. 将需要的 skill 文件夹复制到 Claude Code 的 skills 目录：
   - **macOS/Linux**: `~/.claude/skills/`
   - **Windows**: `%USERPROFILE%\.claude\skills\`

3. 确保文件夹结构如下：
   ```
   ~/.claude/skills/
   ├── python-cola-skill/
   │   ├── SKILL.md
   │   └── references/
   │       ├── dto.md
   │       ├── service.md
   │       ├── gateway.md
   │       └── example.md
   └── java-cola-skill/
       ├── SKILL.md
       └── references/
           ├── dto.md
           ├── service.md
           ├── gateway.md
           └── example.md
   ```

### 验证安装

重启 Claude Code 或重新加载 skills 后，在对话中输入：

```
/python-cola-skill
```

或

```
/java-cola-skill
```

如果安装成功，该技能将被激活。

## 使用

### 触发条件

当你提到以下关键词时，相应的 Skill 会被自动激活：

**通用关键词：**
- COLA、DDD、分层架构、整洁架构
- 六边形架构、洋葱圈架构、端口适配器架构
- 代码分层、模块划分、架构重构
- Gateway 模式、Repository 模式、CQRS

**Python 专用：**
- Python DDD、Flask 架构、FastAPI 架构
- Python 后端架构、Python 项目结构

**Java 专用：**
- Java DDD、Spring Boot 架构
- Maven 多模块、Java 项目结构

### 使用场景示例

#### 场景 1：新建 Python 项目

```
请帮我用 COLA 架构创建一个 Flask 用户管理系统
```

#### 场景 2：重构现有 Java 项目

```
我有一个 Spring Boot 单体应用，请帮我按 COLA 架构重构
```

#### 场景 3：创建特定组件

```
请帮我创建一个符合 COLA 规范的 UserGateway 接口和实现
```

## COLA 架构简介

COLA = Clean Object-Oriented and Layered Architecture（整洁面向对象分层架构）

### 分层结构

```
                        ┌─────────────────────────────────────┐
  Driving Adapter:      │  浏览器  │  定时器  │  消息队列      │
                        └─────────────────────────────────────┘
                                         ↓
  VO ←─────────────── ┌─────────────────────────────────────┐
  (View Object)       │            Adapter 层                │
                      │  controller │ scheduler │ consumer   │
                      └─────────────────────────────────────┘
                                         ↓
  DTO ←────────────── ┌─────────────────────────────────────┐
  (Data Transfer      │              App 层                  │
   Object)            │       service  │  executor           │
                      └─────────────────────────────────────┘
                                         ↓
  Entity ←─────────── ┌─────────────────────────────────────┐
                      │            Domain 层                 │
                      │   gateway │ model │ ability          │
                      └─────────────────────────────────────┘
                                         ↑
  DO ←─────────────── ┌─────────────────────────────────────┐
  (Data Object)       │        Infrastructure 层             │
                      │  gatewayImpl │ mapper │ config       │
                      └─────────────────────────────────────┘
                                         ↓
  Driven Adapter:     │    DB    │   Search   │    RPC      │
```

### COLA 组件

```
┌─────────────────────────────────────────────────────────────┐
│                        COLA 组件（可选）                     │
├─────────────┬─────────────┬─────────────┬─────────────────┤
│ DTO 组件    │ Exception   │ Extension   │ StateMachine    │
│             │ 组件        │ 组件        │ 组件            │
├─────────────┼─────────────┼─────────────┼─────────────────┤
│ CatchLog    │ Domain      │ RuleEngine  │ Test            │
│ 组件        │ 组件        │ 组件        │ 组件            │
└─────────────┴─────────────┴─────────────┴─────────────────┘
```

### COLA Archetypes

COLA 提供两种项目脚手架：

| Archetype | 用途 | 包含模块 |
|-----------|------|----------|
| `cola-archetype-web` | Web 应用（Adapter + 后端服务一体） | adapter, app, client, domain, infrastructure, start |
| `cola-archetype-service` | 纯后端服务（无 Web 层） | app, client, domain, infrastructure, start |

```bash
# 创建 Web 应用
mvn archetype:generate -DarchetypeArtifactId=cola-framework-archetype-web ...

# 创建纯后端服务
mvn archetype:generate -DarchetypeArtifactId=cola-framework-archetype-service ...
```

### 生成的项目结构示例

**cola-archetype-web 生成结构：**

```
demo-web/
├── demo-web-adapter/                    # 适配器层
│   └── src/main/java/com/demo/
│       ├── web/                         # REST Controller
│       │   └── UserController.java
│       ├── mobile/                      # 移动端接口
│       ├── wap/                         # WAP 接口
│       └── scheduler/                   # 定时任务
│           └── DemoJob.java
│
├── demo-web-app/                        # 应用层
│   └── src/main/java/com/demo/
│       ├── command/                     # 命令执行器
│       │   ├── UserAddCmdExe.java
│       │   └── query/                   # 查询执行器
│       │       └── UserListQryExe.java
│       ├── service/                     # Service 实现
│       │   └── UserServiceImpl.java
│       └── event/                       # 事件处理
│           └── handler/
│
├── demo-web-client/                     # 客户端层（API 定义）
│   └── src/main/java/com/demo/
│       ├── api/                         # 服务接口
│       │   └── UserServiceI.java
│       └── dto/                         # 数据传输对象
│           ├── command/
│           │   └── UserAddCmd.java
│           ├── query/
│           │   └── UserListQry.java
│           └── clientobject/
│               └── UserCO.java
│
├── demo-web-domain/                     # 领域层
│   └── src/main/java/com/demo/domain/
│       ├── user/                        # 用户聚合
│       │   ├── User.java                # 实体
│       │   └── UserStatus.java          # 值对象
│       └── gateway/                     # Gateway 接口
│           └── UserGateway.java
│
├── demo-web-infrastructure/             # 基础设施层
│   └── src/main/java/com/demo/
│       ├── gatewayimpl/                 # Gateway 实现
│       │   └── UserGatewayImpl.java
│       ├── convertor/                   # 对象转换器
│       │   └── UserConvertor.java
│       ├── dataobject/                  # 数据对象
│       │   └── UserDO.java
│       ├── mapper/                      # MyBatis Mapper
│       │   └── UserMapper.java
│       └── config/                      # 配置类
│
└── start/                               # 启动模块
    └── src/main/java/com/demo/
        └── Application.java
```

**cola-archetype-service 生成结构（无 adapter 层）：**

```
demo-service/
├── demo-service-app/                    # 应用层
├── demo-service-client/                 # 客户端层（供其他服务调用）
├── demo-service-domain/                 # 领域层
├── demo-service-infrastructure/         # 基础设施层
└── start/                               # 启动模块
```

### 依赖方向

```
Adapter → App → Domain ← Infrastructure
              ↘      ↙
               Client
```

- **Domain 层不依赖任何层**（纯业务逻辑）
- **Infrastructure 实现 Domain 定义的 Gateway 接口**
- **Client 层被所有层依赖（DTO 定义）**

### 命名规范

| 类型 | Python 后缀 | Java 后缀 |
|------|-------------|-----------|
| 命令 | `_cmd` | `Cmd` |
| 查询 | `_qry` | `Qry` |
| 命令执行器 | `_cmd_exe` | `CmdExe` |
| 查询执行器 | `_qry_exe` | `QryExe` |
| 客户端对象 | `_co` | `CO` |
| 服务接口 | `_service_i` | `ServiceI` |
| 服务实现 | `_service` | `ServiceImpl` |
| 网关接口 | `_gateway` | `Gateway` |
| 网关实现 | `_gateway_impl` | `GatewayImpl` |
| 转换器 | `_convertor` | `Convertor` |

## 文件说明

```
cola-DDD-skill/
├── README.md                    # 本说明文档
├── python-cola-skill/
│   ├── SKILL.md                 # Python COLA 技能定义
│   └── references/
│       ├── dto.md               # Response、Command、Query、CO 模板
│       ├── service.md           # Service 和执行器模板
│       ├── gateway.md           # Gateway 模式模板
│       └── example.md           # 完整用户管理示例
└── java-cola-skill/
    ├── SKILL.md                 # Java COLA 技能定义
    └── references/
        ├── dto.md               # Response、Command、Query、CO 模板
        ├── service.md           # Service 和执行器模板
        ├── gateway.md           # Gateway 模式模板
        └── example.md           # 完整示例 + Maven 配置
```

## 参考资源

- [COLA GitHub](https://github.com/alibaba/COLA) - 阿里巴巴 COLA 官方仓库
- [COLA 5.0 发布说明](https://blog.csdn.net/significantfrank/article/details/110934799)
- [《程序员的底层思维》](https://item.jd.com/13652002.html) - COLA 作者新书

## 贡献

欢迎提交 Issue 或 Pull Request 来改进这些 Skills。

### 贡献指南

1. Fork 本仓库
2. 创建你的特性分支 (`git checkout -b feature/amazing-feature`)
3. 提交你的更改 (`git commit -m 'Add some amazing feature'`)
4. 推送到分支 (`git push origin feature/amazing-feature`)
5. 打开一个 Pull Request

## 许可

MIT License

---

**提示：** 这些 Skills 旨在提供架构指导，而非强制规范。根据项目实际情况灵活调整，保持代码整洁和可维护性才是最终目标。
