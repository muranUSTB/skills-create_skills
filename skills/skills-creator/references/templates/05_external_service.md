# 外部服务集成类 Skill 模板

适用于 API 调用、第三方服务对接、云服务集成等场景。

## 模板结构

```markdown
---
name: [skill-name]
description: [一句话描述]
license: LICENSE-CC-BY-NC-SA 4.0 in LICENSE.txt
author: [作者名]
---

# Skill Name

## 简介

[详细介绍外部服务集成的功能]

## 支持的服务

### 云服务提供商
- AWS（亚马逊云）
- Azure（微软云）
- GCP（谷歌云）
- 阿里云

### 第三方 API
- 支付服务（Stripe、PayPal）
- 通讯服务（Twilio、SendGrid）
- 地图服务（Google Maps、高德）
- AI 服务（OpenAI、Anthropic）
- [其他服务]

### 数据库服务
- MySQL
- PostgreSQL
- MongoDB
- Redis

## 功能特性

### 1. 服务连接

连接管理功能：
- 连接池管理
- 连接验证
- 断线重连
- 并发控制

### 2. API 调用

API 请求功能：
- GET 请求
- POST 请求
- RESTful 调用
- GraphQL 查询

### 3. 认证授权

认证管理功能：
- API Key 认证
- OAuth 2.0
- JWT 令牌
- 证书认证

### 4. 错误处理

异常处理功能：
- 请求超时
- 速率限制
- 认证失败
- 服务不可用

### 5. 响应处理

结果处理功能：
- 数据解析
- 格式转换
- 缓存处理
- 日志记录

## 工作流

### Step 1：配置连接

配置服务连接：
- 设置认证信息
- 配置端点地址
- 设置超时参数

### Step 2：构建请求

构建 API 请求：
- 构造请求参数
- 设置请求头
- 处理请求体

### Step 3：发送请求

执行请求：
- 发送 API 请求
- 处理重试逻辑
- 记录请求日志

### Step 4：处理响应

处理返回结果：
- 解析响应数据
- 验证响应状态
- 错误处理

### Step 5：返回结果

返回处理结果：
- 结构化输出
- 错误信息返回
- 元数据添加

## 使用示例

```markdown
请调用 OpenAI API 生成一段文本。

请从数据库查询所有用户信息。

请发送一封邮件给「user@example.com」。

请获取当前天气信息。
```

## 输入参数

| 参数名 | 类型 | 必填 | 说明 |
|-------|------|------|------|
| service | string | 是 | 服务名称 |
| operation | string | 是 | 操作类型 |
| params | object | 是 | 请求参数 |
| credentials | object | 否 | 认证信息 |

## 输出格式

| 输出类型 | 格式 | 说明 |
|---------|------|------|
| 数据 | JSON | API 响应数据 |
| 文件 | 文件 | 下载的文件 |
| 状态 | object | 操作状态信息 |

## 注意事项

- 安全存储认证信息
- 遵守 API 调用限制
- 处理敏感数据
- 记录调用日志
- 实现重试机制

## 安全建议

- 使用环境变量存储密钥
- 实施最小权限原则
- 加密敏感数据
- 定期轮换密钥
```

## 配置示例

```yaml
# 配置文件示例
services:
  openai:
    api_key: ${OPENAI_API_KEY}
    base_url: https://api.openai.com/v1
    timeout: 30
    max_retries: 3

  database:
    host: localhost
    port: 5432
    username: ${DB_USER}
    password: ${DB_PASSWORD}
```
