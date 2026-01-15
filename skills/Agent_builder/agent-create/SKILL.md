---
name: agent-create
description: 专业的 AI Agent 开发助手，支持 LangGraph、AgentScope、DeepAgent、AutoGen、CrewAI 等主流框架的完整开发流程
license: CC BY-NC-SA 4.0
author: Claude User
---

# Agent Create Skill

专业的 AI Agent 开发助手，帮助用户从零搭建各类 AI Agent 框架。支持 LangGraph、AgentScope、DeepAgent、AutoGen、CutoGen Studio、CrewAI 等主流框架，提供完整的开发工作流：需求分析、文档探索、结构规划、代码开发、测试验证。

## 使用场景

- **新项目启动**：需要搭建新的 Agent 项目，但不确定选择哪个框架
- **框架学习**：想学习某个 Agent 框架，但不知从何入手
- **快速原型**：需要快速构建一个可运行的 Agent 原型
- **生产级开发**：需要构建完整的、生产就绪的 Agent 系统

## 前置条件

- Claude Code CLI 环境
- 网络连接（用于搜索官方文档）
- 对应框架的运行时环境（如 Python 3.10+）
- 必要的 MCP 服务器已配置（用于文档搜索和网页访问）

## 工作流

本 Skill 采用六阶段专业开发工作流，每个阶段都有明确的产出物和用户确认环节：

### 阶段 1：需求收集与分析

**目标**：充分理解用户需求，确定最佳框架选择

**步骤**：
1. 询问使用场景和业务目标
2. 了解技术栈偏好和经验水平
3. 确认性能要求和扩展性需求
4. 讨论预算和时间限制

**产出物**：需求收集清单

**示例问题**：
- "这个 Agent 的主要用途是什么？对话、任务自动化、数据处理？"
- "您之前使用过哪些 Agent 框架？"
- "项目预计的用户规模和请求量是多少？"
- "是否需要多 Agent 协作？"

### 阶段 2：PRD 文档生成

**目标**：基于需求分析，生成结构化的产品需求文档

**内容**：
- 项目概述与目标
- 功能需求规格
- 非功能需求（性能、安全、可扩展性）
- 技术选型建议
- 风险评估与应对策略
- 里程碑规划

**产出物**：`PRD.md` - 产品需求文档

**文档结构**：
```markdown
# Agent 项目 PRD

## 1. 项目概述
## 2. 用户故事
## 3. 功能规格
## 4. 技术架构
## 5. 实施计划
## 6. 验收标准
```

### 阶段 3：文档探索（Subagent）

**目标**：使用 MCP 工具搜索官方文档，充分了解框架的实现细节和最佳实践

**触发 Subagent**：`bmad:research` 或自定义文档搜索代理

**搜索内容**：
- 官方文档和快速入门指南
- API 参考和核心概念
- 示例项目和最佳实践
- 最新版本特性和变更日志
- 常见问题和解决方案

**产出物**：`FRAMEWORK_DOCS.md` - 框架文档摘要

**处理方式**：
- Subagent 独立执行，不占用主代理上下文
- 返回结构化的框架知识库
- 提供代码示例和配置模板

### 阶段 4：结构规划（Subagent）

**目标**：基于需求和文档分析，规划完整的项目结构

**触发 Subagent**：自定义架构规划代理

**规划内容**：
- 项目目录结构设计
- 模块划分和职责定义
- 依赖配置和版本管理
- 配置文件规范
- 入口点和生命周期管理
- 测试策略和目录结构

**产出物**：
- `ARCHITECTURE.md` - 架构设计文档
- `PROJECT_STRUCTURE.md` - 项目结构说明
- 代码框架模板

**示例目录结构**：
```
agent-project/
├── src/
│   ├── agents/          # Agent 定义
│   ├── tools/           # 工具实现
│   ├── workflows/       # 工作流编排
│   ├── core/            # 核心逻辑
│   └── utils/           # 工具函数
├── tests/
│   ├── unit/            # 单元测试
│   ├── integration/     # 集成测试
│   └── e2e/             # 端到端测试
├── config/              # 配置文件
├── docs/                # 文档
└── scripts/             # 脚本
```

### 阶段 5：代码开发

**目标**：基于规划的结构，生成完整的代码实现

**开发内容**：
- Agent 核心实现
- 工具和函数定义
- 工作流编排
- 配置管理
- 错误处理和日志
- 文档字符串和注释

**产出物**：
- 完整的源代码文件
- 配置文件（pyproject.toml、requirements.txt 等）
- 环境变量示例（.env.example）
- Makefile 或脚本

**代码质量标准**：
- 遵循 PEP 8 / PEP 484
- 类型注解完整
- Docstring 规范
- 适当的错误处理
- 日志记录

### 阶段 6：测试验证（Subagent）

**目标**：使用独立 Subagent 执行测试，确保代码质量

**触发 Subagent**：自定义测试代理

**测试内容**：
- 单元测试生成和执行
- 集成测试验证
- 代码质量检查（lint、type check）
- 文档测试（如果适用）

**产出物**：
- `TEST_REPORT.md` - 测试报告
- 通过/失败状态
- 修复建议

## 输入参数

### 必需参数

| 参数名 | 类型 | 描述 |
|-------|------|------|
| `framework` | string | 目标框架（langgraph、agentscope、deepagent、autogen、crewai） |
| `project_name` | string | 项目名称 |

### 可选参数

| 参数名 | 类型 | 默认值 | 描述 |
|-------|------|--------|------|
| `language` | string | "python" | 开发语言 |
| `features` | array | [] | 所需功能特性列表 |
| `has_ui` | boolean | false | 是否需要 Web UI |
| `deployment` | string | "local" | 部署环境（local/docker/cloud） |

### 使用示例

```
/agent-create --framework=langgraph --project_name=my-chat-agent --features=memory,tools --has_ui=true
```

## 输出格式

### 目录结构

```
{project_name}/
├── src/
│   └── {project_name}/
│       ├── __init__.py
│       ├── agent.py
│       ├── config.py
│       ├── tools/
│       └── workflows/
├── tests/
├── docs/
│   ├── PRD.md
│   ├── ARCHITECTURE.md
│   └── FRAMEWORK_DOCS.md
├── config/
├── scripts/
├── .env.example
├── pyproject.toml
├── requirements.txt
├── README.md
└── Makefile
```

### 文件说明

| 文件 | 描述 |
|-----|------|
| `src/agent.py` | Agent 核心定义 |
| `src/config.py` | 配置管理 |
| `src/tools/*.py` | 自定义工具实现 |
| `src/workflows/*.py` | 工作流定义 |
| `tests/test_*.py` | 测试用例 |
| `docs/PRD.md` | 产品需求文档 |
| `docs/ARCHITECTURE.md` | 架构设计文档 |
| `docs/FRAMEWORK_DOCS.md` | 框架文档摘要 |
| `config/*.py` / `*.yaml` | 配置文件 |
| `pyproject.toml` | 项目元数据和依赖 |
| `Makefile` | 开发任务脚本 |

## 使用示例

### 示例 1：创建简单的对话 Agent

```
用户：我想创建一个简单的对话 Agent，使用 LangGraph 框架

Agent Create：
[阶段 1] 您好！我来帮您创建一个 LangGraph 对话 Agent。
为了更好地满足您的需求，请告诉我：

1. 这个 Agent 主要用于什么场景？（客服、个人助手、知识库问答等）
2. 是否需要集成外部工具？（搜索、数据库、API 等）
3. 是否需要长期记忆？
4. 预计的并发量是多少？

用户：用于客服场景，需要集成搜索工具，不需要长期记忆，预计并发 100

[生成 PRD，进行后续阶段...]
```

### 示例 2：创建多 Agent 协作系统

```
用户：用 AgentScope 创建一个多 Agent 协作系统，用于代码审查

Agent Create：
[开始需求收集...]
[生成 PRD...]
[Subagent 搜索 AgentScope 官方文档...]
[Subagent 规划项目结构...]
[生成完整代码...]
[Subagent 执行测试...]
```

## 支持的框架

| 框架 | 特点 | 适用场景 |
|-----|------|---------|
| **LangGraph** | 循环工作流、状态管理强 | 复杂对话、任务编排 |
| **AgentScope** | 分散式架构、易用性好 | 快速原型、多 Agent 协作 |
| **DeepAgent** | 深度学习集成、研究导向 | 研究实验、高级特性 |
| **AutoGen** | 多 Agent 对话、自动化强 | 企业应用、工作流自动化 |
| **CrewAI** | 角色扮演、编排简洁 | 团队模拟、角色协作 |

## 注意事项

1. **框架选择**：如果用户不确定框架，提供各框架的对比分析
2. **版本兼容性**：确保依赖版本兼容性，提供版本约束
3. **环境配置**：提供完整的运行环境配置指南
4. **文档更新**：文档探索阶段获取的信息需要验证时效性
5. **错误处理**：开发过程中遇到问题，提供备选方案

## 错误处理

| 错误类型 | 处理方式 |
|---------|---------|
| 文档搜索失败 | 尝试备用文档源，提供手动查阅链接 |
| 代码生成错误 | 提供错误日志，尝试简化实现 |
| 测试失败 | 分析失败原因，生成修复建议 |
| 依赖冲突 | 提供版本调整建议或虚拟环境配置 |

## 扩展功能

本 Skill 可以根据需求扩展以下功能：

- **模板市场**：提供预定义的 Agent 模板
- **插件系统**：支持自定义工具和中间件
- **部署集成**：一键部署到主流云平台
- **监控集成**：集成可观测性工具
- **版本升级**：框架版本升级助手

## 最佳实践

1. **渐进式开发**：从最小可行产品开始，逐步增加功能
2. **文档驱动**：先有文档再有实现
3. **测试先行**：关键逻辑先写测试
4. **代码审查**：重要变更需要代码审查
5. **持续集成**：配置 CI/CD 流水线
