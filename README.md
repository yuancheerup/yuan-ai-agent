# Yuan AI Agent

基于 Spring AI 的智能恋爱咨询助手，集成 RAG（检索增强生成）技术，为用户提供专业的恋爱心理咨询服务。

## 项目简介

Yuan AI Agent 是一个基于 Spring Boot 和 Spring AI 框架开发的智能对话系统，专注于恋爱心理领域。通过集成阿里云百炼大模型和 PGVector 向量数据库，实现了智能化的恋爱咨询服务，支持多轮对话记忆和知识库检索增强。

## 核心特性

- **智能对话**：基于阿里云百炼大模型，提供专业的恋爱心理咨询服务
- **多轮对话记忆**：支持上下文记忆，实现连贯的多轮对话体验
- **RAG 检索增强**：集成 PGVector 向量数据库，通过知识库检索增强回答质量
- **查询重写**：自动优化用户查询，提升检索准确性
- **文档智能分割**：支持 Markdown 文档的智能解析和向量化存储
- **API 文档**：集成 Knife4j，提供友好的 API 文档界面

## 技术栈

| 技术 | 版本 | 说明 |
|------|------|------|
| Spring Boot | 3.4.7 | 基础框架 |
| Java | 21 | 编程语言 |
| Spring AI | 1.0.0-M6 | AI 应用开发框架 |
| Spring AI Alibaba | 1.0.0-M6.1 | 阿里云百炼集成 |
| LangChain4j | 1.0.0-beta2 | LangChain Java 实现 |
| PostgreSQL + PGVector | - | 向量数据库 |
| Knife4j | 4.4.0 | API 文档工具 |
| Hutool | 5.8.38 | Java 工具库 |
| Lombok | 1.18.36 | 代码简化工具 |


## 快速开始

### 环境要求

- JDK 21+
- Maven 3.6+
- PostgreSQL 12+（需安装 PGVector 扩展）
- 阿里云百炼 API Key

### 配置说明

1. **配置阿里云百炼 API Key**

   在 `application-local.yml` 中配置：
   ```yaml
   spring:
     ai:
       dashscope:
         api-key: your-api-key-here
   ```

2. **配置 PostgreSQL 数据库**

   在 `application-local.yml` 中配置：
   ```yaml
   spring:
     datasource:
       url: jdbc:postgresql://localhost:5432/your-database
       username: your-username
       password: your-password
   ```

3. **安装 PGVector 扩展**

   在 PostgreSQL 中执行：
   ```sql
   CREATE EXTENSION IF NOT EXISTS vector;
   ```

### 运行项目

```bash
# 克隆项目
git clone <repository-url>
cd yuan-ai-agent

# 编译项目
mvn clean install

# 运行项目
mvn spring-boot:run
```

应用将在 `http://localhost:8123/api` 启动

### API 文档访问

启动项目后，访问 Knife4j API 文档：
http://localhost:8123/api/doc.html


## 核心功能说明

### 1. 智能对话

支持多轮对话记忆，自动维护会话上下文：

```java
String response = loveApp.doChat("我想咨询恋爱问题", "chat-001");
```

### 2. RAG 知识库问答

集成向量检索，基于知识库提供精准回答：

```java
String response = loveApp.doChatWithRag("如何追求心仪的对象？", "chat-001");
```

### 3. 查询重写

自动优化用户查询，提升检索效果：

```java
String rewrittenQuery = queryRewriter.doQueryRewriter("怎么追女生");
```

### 4. 文档向量化

支持 Markdown 文档的智能解析和向量化存储：

```java
loveAppDocumentLoader.loadDocuments();
```

## RAG 架构说明

项目实现了完整的 RAG（检索增强生成）流程：

1. **文档加载**：`LoveAppDocumentLoader` 负责加载和解析知识库文档
2. **文本分割**：`MyTokenTextSplitter` 智能分割长文档
3. **向量化存储**：使用 PGVector 存储文档向量
4. **查询重写**：`QueryRewriter` 优化用户查询
5. **检索增强**：`LoveAppRagCustomAdvisorFactory` 创建检索增强顾问
6. **上下文注入**：`LoveAppContextualQueryAugmenterFactory` 增强查询上下文

## 知识库文档

项目内置三份恋爱知识库文档：

- **单身篇**：单身状态下的社交拓展、追求技巧等问题
- **恋爱篇**：恋爱中的沟通、矛盾处理等问题
- **已婚篇**：婚姻中的家庭责任、亲属关系等问题

文档支持按状态标签过滤检索。

## 开发指南

### 添加新的知识库文档

1. 将 Markdown 文档放入 `src/main/resources/document/` 目录
2. 在文档中添加状态标签元数据
3. 调用 `LoveAppDocumentLoader` 加载文档

### 自定义 RAG 策略

通过 `LoveAppRagCustomAdvisorFactory` 可以自定义：
- 相似度阈值
- 返回文档数量
- 过滤条件

```java
Advisor advisor = LoveAppRagCustomAdvisorFactory
    .createLoveAppRagCustomAdvisor(vectorStore, "恋爱");
```

## 测试

项目包含完整的单元测试：

```bash
# 运行所有测试
mvn test

# 运行特定测试类
mvn test -Dtest=LoveAppTest
```

## 许可证

本项目采用 MIT 许可证。

## 联系方式

如有问题或建议，欢迎提交 Issue 或 Pull Request。
