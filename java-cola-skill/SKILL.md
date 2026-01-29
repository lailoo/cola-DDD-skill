---
name: java-cola-skill
description: Java COLA 架构指南 - 阿里巴巴 COLA 整洁分层架构框架。用于 Java/Spring Boot 项目的 DDD 分层架构设计、代码结构规范、应用架构重构。触发条件：COLA、Java DDD、Spring Boot 架构、分层架构、整洁架构、六边形架构、洋葱圈架构、代码分层、模块划分、Gateway 模式、Repository 模式、CQRS、架构重构、职责分离、Maven 多模块。创建 Java 文件时自动应用 COLA 目录结构和命名规范。
---

# Java COLA 架构

COLA = Clean Object-Oriented and Layered Architecture（整洁面向对象分层架构）

## 分层架构

```
Adapter → App → Domain ← Infrastructure
              ↘      ↙
               Client
```

| 层 | 职责 | 实现 |
|---|------|------|
| **Adapter** | 接收外部请求 | Controller, 消息监听, 定时任务 |
| **App** | 用例编排 | ServiceImpl, CmdExe, QryExe |
| **Domain** | 核心业务逻辑 | Entity, ValueObject, Gateway 接口 |
| **Infrastructure** | 技术实现 | GatewayImpl, Mapper |
| **Client** | DTO 定义 | Command, Query, CO, ServiceI |

## 目录结构

```
project-name/
├── project-adapter/          # Controller
│   └── com/company/project/web/
├── project-app/              # Service 实现
│   └── com/company/project/
│       ├── command/          # *CmdExe.java
│       │   └── query/        # *QryExe.java
│       └── service/          # *ServiceImpl.java
├── project-client/           # API 定义
│   └── com/company/project/
│       ├── api/              # *ServiceI.java
│       └── dto/
│           ├── command/      # *Cmd.java
│           ├── query/        # *Qry.java
│           └── clientobject/ # *CO.java
├── project-domain/           # 领域层
│   └── com/company/project/domain/
│       ├── {aggregate}/      # Entity, ValueObject
│       └── gateway/          # *Gateway.java
├── project-infrastructure/   # 基础设施
│   └── com/company/project/
│       ├── gatewayimpl/      # *GatewayImpl.java
│       └── convertor/        # *Convertor.java
└── start/                    # 启动模块
```

## 命名规范

| 类型 | 后缀 | 位置 |
|------|------|------|
| 命令 | `Cmd` | client/dto/command |
| 查询 | `Qry` | client/dto/query |
| 命令执行器 | `CmdExe` | app/command |
| 查询执行器 | `QryExe` | app/command/query |
| 客户端对象 | `CO` | client/dto/clientobject |
| 数据对象 | `DO` | infrastructure |
| 服务接口 | `ServiceI` | client/api |
| 服务实现 | `ServiceImpl` | app/service |
| 网关接口 | `Gateway` | domain/gateway |
| 网关实现 | `GatewayImpl` | infrastructure/gatewayimpl |
| 转换器 | `Convertor` | infrastructure/convertor |

## 快速创建项目

```bash
mvn archetype:generate \
    -DgroupId=com.company.project \
    -DartifactId=my-project \
    -Dversion=1.0.0-SNAPSHOT \
    -Dpackage=com.company.project \
    -DarchetypeArtifactId=cola-framework-archetype-web \
    -DarchetypeGroupId=com.alibaba.cola \
    -DarchetypeVersion=5.0.0
```

## COLA 组件

| 组件 | 功能 |
|------|------|
| `cola-component-dto` | Response, PageResponse 等 |
| `cola-component-exception` | BizException, SysException |
| `cola-component-catchlog-starter` | @CatchAndLog 注解 |
| `cola-component-statemachine` | 状态机 |
| `cola-component-extension-starter` | 扩展点 |

## 代码模板

详细代码示例请参考：
- **DTO 和 Response**: [references/dto.md](references/dto.md)
- **Service 和执行器**: [references/service.md](references/service.md)
- **Gateway 模式**: [references/gateway.md](references/gateway.md)
- **完整示例**: [references/example.md](references/example.md)

## 重构检查清单

1. 创建 adapter/app/client/domain/infrastructure/start 模块
2. 配置 Maven 依赖关系
3. 定义 Command、Query、CO 类
4. 创建 CmdExe/QryExe 和 ServiceImpl
5. 定义 Gateway 接口和 GatewayImpl
6. 创建 Convertor 转换器
7. 迁移 Controller 到 adapter/web
