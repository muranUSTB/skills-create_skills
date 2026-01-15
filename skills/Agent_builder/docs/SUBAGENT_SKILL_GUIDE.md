# Claude Code Subagent 与 Skill 完整指南

> 从零创建专业级 Agent 开发工具的全过程记录

---

## 目录

1. [背景与目标](#1-背景与目标)
2. [准备工作](#2-准备工作)
3. [创建 Subagent](#3-创建-subagent)
4. [创建 Skill](#4-创建-skill)
5. [Subagent 与 Skill 的关系](#5-subagent-与-skill-的关系)
6. [如何使用](#6-如何使用)
7. [完整示例](#7-完整示例)
8. [常见问题](#8-常见问题)

---

## 1. 背景与目标

### 1.1 需求描述

用户希望创建一个专业的 **AI Agent 开发工具**，用于帮助开发者快速搭建各类 Agent 框架项目（LangGraph、AgentScope、DeepAgent、AutoGen、CrewAI 等）。

**核心要求：**
- 支持多个主流 Agent 框架
- 完整的开发工作流（需求分析 → 文档探索 → 结构规划 → 开发 → 测试）
- 利用 MCP 工具搜索官方文档
- 使用 Subagent 处理耗时任务，不占用主代理上下文
- 彩色标识区分不同功能的 Subagent

### 1.2 最终成果

```
📦 agent-create (Skill)
├── agent-doc-explorer  🔵 蓝色 - 文档探索专家
├── agent-arch-planner  🟢 绿色 - 架构规划师
└── agent-tester        🟠 橙色 - 测试专家
```

---

## 2. 准备工作

### 2.1 目录结构认知

Claude Code 的配置文件存放在用户目录下：

```bash
# Windows
C:\Users\{用户名}\.claude\

# macOS/Linux
~/.claude/
```

**核心目录：**

| 目录 | 用途 |
|------|------|
| `agents/` | 存放 Subagent 配置 |
| `skills/` | 存放 Skill 配置 |
| `commands/` | 自定义命令 |
| `config/` | 其他配置 |

### 2.2 创建目录

```bash
# 创建三个 Subagent 目录
mkdir -p C:/Users/14429/.claude/agents/agent-doc-explorer
mkdir -p C:/Users/14429/.claude/agents/agent-arch-planner
mkdir -p C:/Users/14429/.claude/agents/agent-tester

# 创建 Skill 目录
mkdir -p C:/Users/14429/.claude/Skills/agent-create
```

---

## 3. 创建 Subagent

### 3.1 Subagent 是什么？

**Subagent（子代理）** 是 Claude Code 中的独立代理单元，具有：
- 独立的上下文环境
- 单独的系统提示词
- 可被主代理调用
- 执行结果返回主代理

### 3.2 Subagent 文件结构

每个 Subagent 只需要一个 `agent.md` 文件：

```
agents/{subagent-name}/
└── agent.md          # Subagent 配置文件（必需）
```

### 3.3 创建文档探索专家（蓝色）

**路径：** `C:\Users\14429\.claude\agents\agent-doc-explorer\agent.md`

```markdown
---
name: agent-doc-explorer
description: 🔵 Agent 文档探索专家 - 负责搜索和分析官方文档
color: blue
author: Claude User
---

# Agent Doc Explorer (🔵 蓝色)

使用 MCP 工具搜索官方文档的专业 Subagent。

## 职责

- 使用 MCP 搜索官方文档
- 提取快速入门指南
- 分析 API 参考
- 收集示例代码
- 生成文档摘要

## 输入参数

| 参数 | 类型 | 描述 |
|-----|------|------|
| `framework` | string | 框架名称（langgraph, agentscope 等） |
| `focus` | string | 搜索重点（installation, api, examples） |

## 工作流程

### Step 1：搜索文档
使用 MCP 工具搜索官方文档：
```python
# 使用 context7 MCP 搜索文档
mcp__context7__query_docs(libraryId="/org/project", query="...")
```

### Step 2：提取关键信息
- 快速入门步骤
- 核心 API 用法
- 配置选项
- 最佳实践

### Step 3：生成摘要
输出结构化的框架知识库。

## 输出

- FRAMEWORK_DOCS.md
- 代码示例
- 配置模板

## 颜色标识

🔵 **蓝色** - 表示文档探索、信息收集类任务
```

### 3.4 创建架构规划师（绿色）

**路径：** `C:\Users\14429\.claude\agents\agent-arch-planner\agent.md`

```markdown
---
name: agent-arch-planner
description: 🟢 Agent 架构规划师 - 负责设计项目结构和模块划分
color: green
author: Claude User
---

# Agent Arch Planner (🟢 绿色)

设计项目架构和目录结构的专业 Subagent。

## 职责

- 设计项目目录结构
- 规划模块划分
- 配置依赖管理
- 制定编码规范
- 生成代码模板

## 输入参数

| 参数 | 类型 | 描述 |
|-----|------|------|
| `framework` | string | 框架类型 |
| `project_name` | string | 项目名称 |
| `requirements` | string | 需求描述 |

## 工作流程

### Step 1：分析需求
理解项目需求和框架特点。

### Step 2：设计结构
```
{project_name}/
├── src/
│   └── {project_name}/
│       ├── agent.py
│       ├── config.py
│       ├── tools/
│       └── workflows/
├── tests/
├── docs/
├── config/
├── scripts/
├── .env.example
├── pyproject.toml
├── requirements.txt
└── README.md
```

### Step 3：配置依赖
生成 `pyproject.toml` 和 `requirements.txt`。

### Step 4：生成代码模板
编写核心代码框架。

## 输出

- ARCHITECTURE.md
- PROJECT_STRUCTURE.md
- 代码框架模板

## 颜色标识

🟢 **绿色** - 表示架构设计、规划类任务
```

### 3.5 创建测试专家（橙色）

**路径：** `C:\Users\14429\.claude\agents\agent-tester\agent.md`

```markdown
---
name: agent-tester
description: 🟠 Agent 测试专家 - 负责代码测试和质量验证
color: orange
author: Claude User
---

# Agent Tester (🟠 橙色)

执行测试和验证代码质量的专业 Subagent。

## 职责

- 编写单元测试
- 执行集成测试
- 运行代码质量检查
- 生成测试报告
- 提供修复建议

## 输入参数

| 参数 | 类型 | 描述 |
|-----|------|------|
| `project_path` | string | 项目路径 |
| `framework` | string | 框架类型 |
| `test_level` | string | 测试级别 |

## 工作流程

### Step 1：检查项目结构
验证目录和文件是否符合规范。

### Step 2：编写测试用例
```python
# tests/test_agent.py
import pytest

class TestAgent:
    def test_initialization(self):
        # ...

    def test_response(self):
        # ...
```

### Step 3：执行测试
```bash
pytest tests/ -v --cov=src
black --check src/
ruff check src/
mypy src/
```

### Step 4：生成报告
输出 TEST_REPORT.md。

## 输出

- TEST_REPORT.md
- 测试结果摘要
- 修复建议

## 颜色标识

🟠 **橙色** - 表示测试、验证、质量保障类任务
```

### 3.6 Subagent 创建完成

```bash
$ ls -la C:/Users/14429/.claude/agents/

agents/
├── agent-doc-explorer/      # 🔵 蓝色
│   └── agent.md
├── agent-arch-planner/      # 🟢 绿色
│   └── agent.md
└── agent-tester/            # 🟠 橙色
    └── agent.md
```

---

## 4. 创建 Skill

### 4.1 Skill 是什么？

**Skill** 是 Claude Code 的功能扩展，提供特定领域的专业能力。

**与 Subagent 的区别：**

| 特性 | Subagent | Skill |
|------|----------|-------|
| 定位 | 被调用的执行单元 | 主动提供的功能 |
| 调用方式 | `Task(subagent_type="...")` | `/skill-name` |
| 上下文 | 独立，不占主代理上下文 | 共享主代理上下文 |
| 数量 | 可多个 | 可多个 |

### 4.2 Skill 文件结构

```
skills/{skill-name}/
├── SKILL.md        # 必需：Skill 主文件
├── README.md       # 可选：说明文档
└── LICENSE.txt     # 可选：许可证
```

### 4.3 创建 agent-create Skill

**路径：** `C:\Users\14429\.claude\Skills\agent-create\SKILL.md`

```markdown
---
name: agent-create
description: 专业的 AI Agent 开发助手，支持 LangGraph、AgentScope、DeepAgent、AutoGen、CrewAI 等主流框架的完整开发流程
license: CC BY-NC-SA 4.0
author: Claude User
---

# Agent Create Skill

专业的 AI Agent 开发助手，提供完整的开发工作流。

## Subagent 配置

| Subagent | 名称 | 颜色 | 职责 |
|----------|------|------|------|
| `agent-doc-explorer` | 文档探索专家 | 🔵 蓝色 | 搜索和分析官方文档 |
| `agent-arch-planner` | 架构规划师 | 🟢 绿色 | 设计项目结构和架构 |
| `agent-tester` | 测试专家 | 🟠 橙色 | 执行测试和质量验证 |

## 工作流

```
用户需求 --> [阶段1] 需求收集 --> [阶段2] PRD生成
                        |                   |
                        v                   v
         [阶段3] 🔵 Subagent: agent-doc-explorer
                  搜索官方文档 --> 生成 FRAMEWORK_DOCS.md
                                       |
                                       v
         [阶段4] 🟢 Subagent: agent-arch-planner
                  规划结构 --> 生成 ARCHITECTURE.md
                                       |
                                       v
                    [阶段5] 代码开发 --> 完整源代码
                                       |
                                       v
         [阶段6] 🟠 Subagent: agent-tester
                  执行测试 --> 生成 TEST_REPORT.md
```

## Subagent 调用示例

```python
# 阶段 3：文档探索
Task(
    subagent_type="agent-doc-explorer",
    prompt=f"搜索 {framework} 官方文档，提取快速入门和API"
)

# 阶段 4：结构规划
Task(
    subagent_type="agent-arch-planner",
    prompt=f"基于需求设计 {project_name} 的项目结构"
)

# 阶段 6：测试验证
Task(
    subagent_type="agent-tester",
    prompt=f"对项目执行完整测试，生成测试报告"
)
```

## 使用示例

```
/agent-create --framework=langgraph --project_name=my-chat-agent
```
```

### 4.4 创建附加文件

**README.md：**
```markdown
# agent-create

专业的 AI Agent 开发助手。

## 功能

- 支持多框架（LangGraph、AgentScope 等）
- 六阶段完整工作流
- 三个专业 Subagent

## 使用

```bash
/agent-create --framework=langgraph --project_name=my-agent
```
```

**LICENSE.txt：**
使用 CC BY-NC-SA 4.0 许可证（略）

### 4.5 Skill 创建完成

```bash
$ ls -la C:/Users/14429/.claude/Skills/agent-create/

Skills/agent-create/
├── SKILL.md       # 9.4 KB
├── README.md      # 2.1 KB
└── LICENSE.txt    # 16.3 KB
```

---

## 5. Subagent 与 Skill 的关系

### 5.1 层次结构

```
┌─────────────────────────────────────┐
│           用户交互层                 │
│         /agent-create (Skill)       │
└─────────────────┬───────────────────┘
                  │
                  ▼
┌─────────────────────────────────────┐
│           任务分发层                 │
│         Task(subagent_type=...)     │
└─────────────────┬───────────────────┘
                  │
        ┌─────────┼─────────┐
        ▼         ▼         ▼
┌───────────┐ ┌───────────┐ ┌───────────┐
│  🔵 Sub   │ │  🟢 Sub   │ │  🟠 Sub   │
│  agent-   │ │  agent-   │ │  agent-   │
│  doc-     │ │  arch-    │ │  tester   │
│  explorer │ │  planner  │ │           │
└───────────┘ └───────────┘ └───────────┘
```

### 5.2 调用流程

```python
# 1. 用户调用 Skill
/agent-create --framework=langgraph --project_name=my-agent

# 2. Skill 解析需求，生成 PRD

# 3. 调用文档探索 Subagent
doc_result = Task(
    subagent_type="agent-doc-explorer",
    prompt="搜索 LangGraph 官方文档..."
)

# 4. 调用架构规划 Subagent
arch_result = Task(
    subagent_type="agent-arch-planner",
    prompt="设计项目结构..."
)

# 5. 主代理生成代码

# 6. 调用测试 Subagent
test_result = Task(
    subagent_type="agent-tester",
    prompt="执行测试..."
)

# 7. 返回最终结果
✅ 项目完成！
```

### 5.3 上下文管理

**关键优势：** Subagent 独立执行，不占用主代理上下文！

| 层级 | 上下文 | 用途 |
|------|--------|------|
| 主代理 (Skill) | 用户原始需求 + PRD | 整体协调 |
| 🔵 Subagent | 框架文档查询结果 | 文档分析 |
| 🟢 Subagent | PRD + 文档摘要 | 结构设计 |
| 🟠 Subagent | 完整代码 | 测试验证 |

---

## 6. 如何使用

### 6.1 前置条件

确保 Claude Code 已安装，配置好 MCP 服务器：

```json
// ~/.claude/settings.json
{
  "mcpServers": {
    "context7": { ... },
    "exa-code": { ... }
  }
}
```

### 6.2 使用 Skill

**方式 1：命令调用**
```bash
/agent-create --framework=langgraph --project_name=my-chat-agent --features=memory,tools
```

**方式 2：对话调用**
```
用户：帮我用 LangGraph 创建一个对话 Agent

Agent Create：
好的！我来帮您创建一个 LangGraph 对话 Agent。
请告诉我：
1. 这个 Agent 用于什么场景？
2. 需要集成哪些工具？
3. 是否需要长期记忆？
```

### 6.3 查看可用 Skills

```bash
/claude code --list-skills
```

### 6.4 查看可用 Subagents

Subagent 不直接调用，通过 Skill 内部使用。

---

## 7. 完整示例

### 场景：创建 LangGraph 对话 Agent

#### Step 1：用户提出需求

```
用户：帮我创建一个 LangGraph 对话 Agent，用于客服场景
```

#### Step 2：Skill 响应

```
[agent-create]：
您好！我来帮您创建一个 LangGraph 对话 Agent。

请确认以下需求：
1. 主要功能：对话 + 搜索工具
2. 场景：客服
3. 框架：LangGraph
4. 项目名：customer-service-agent

请确认是否正确？
```

#### Step 3：用户确认

```
用户：确认，开始创建
```

#### Step 4：Skill 执行流程

```
[阶段 1] 需求收集完成 ✅
[阶段 2] 生成 PRD.md ✅

[阶段 3] 🔵 调用 agent-doc-explorer
  └─ 搜索 LangGraph 官方文档...
  └─ 提取快速入门、API、示例...
  └─ 生成 FRAMEWORK_DOCS.md ✅

[阶段 4] 🟢 调用 agent-arch-planner
  └─ 分析 PRD 和框架文档...
  └─ 设计项目结构...
  └─ 生成 ARCHITECTURE.md + 代码模板 ✅

[阶段 5] 生成完整代码
  └─ src/customer_service_agent/agent.py
  └─ src/customer_service_agent/config.py
  └─ src/customer_service_agent/tools/search.py
  └─ pyproject.toml
  └─ requirements.txt ✅

[阶段 6] 🟠 调用 agent-tester
  └─ 执行单元测试...
  └─ 执行代码质量检查...
  └─ 生成 TEST_REPORT.md ✅
```

#### Step 5：完成交付

```
✅ 项目创建完成！

📁 输出目录：customer-service-agent/
├── src/
│   └── customer_service_agent/
│       ├── __init__.py
│       ├── agent.py
│       ├── config.py
│       ├── tools/
│       │   └── search.py
│       └── workflows/
│           └── conversation.py
├── tests/
├── docs/
│   ├── PRD.md
│   ├── ARCHITECTURE.md
│   ├── FRAMEWORK_DOCS.md
│   └── TEST_REPORT.md
├── config/
├── scripts/
├── .env.example
├── .gitignore
├── pyproject.toml
├── requirements.txt
├── README.md
└── Makefile

🎉 可以开始开发了！
```

---

## 8. 常见问题

### Q1：Subagent 和 Skill 有什么区别？

| 问题 | Subagent | Skill |
|------|----------|-------|
| 是什么 | 被调用的执行单元 | 提供功能的扩展 |
| 如何调用 | `Task(subagent_type="...")` | `/skill-name` |
| 上下文 | 独立 | 共享 |
| 用途 | 复杂任务的分解 | 特定领域的专业能力 |

### Q2：Subagent 可以独立使用吗？

**可以！** Subagent 可以被任何 Skill 或主代理调用：

```python
Task(subagent_type="agent-doc-explorer", prompt="搜索 Python 官方文档")
```

### Q3：如何调试 Subagent？

1. 在 Skill 中添加日志
2. 检查 Subagent 返回的结果
3. 验证 Subagent 的 agent.md 配置

### Q4：Subagent 返回的结果如何传递给主代理？

```python
# 调用 Subagent
result = Task(subagent_type="agent-doc-explorer", prompt="搜索文档")

# result 包含 Subagent 的所有输出
docs_summary = result.summary
code_examples = result.examples
```

### Q5：可以创建多少个 Subagent？

**没有限制！** 根据需求创建任意数量的 Subagent。

**建议：**
- 每个 Subagent 专注于单一职责
- 使用颜色或前缀区分不同类型
- 保持命名清晰

### Q6：Subagent 会消耗额外配额吗？

是的。每个 Subagent 都是独立的 Claude API 调用。

**优化建议：**
- Subagent 处理耗时的独立任务
- 主代理处理需要上下文的决策

---

## 总结

### 核心概念

| 概念 | 说明 |
|------|------|
| **Subagent** | 独立的执行单元，用于处理特定任务 |
| **Skill** | 提供专业能力的扩展工具 |
| **Task()** | 调用 Subagent 的函数 |
| **颜色标识** | 区分不同职责的 Subagent |

### 创建步骤

1. ✅ 创建 Subagent 目录和 `agent.md`
2. ✅ 创建 Skill 目录和 `SKILL.md`
3. ✅ 在 Skill 中使用 `Task()` 调用 Subagent
4. ✅ 重新加载 Claude Code
5. ✅ 开始使用！

### 文件位置汇总

```
C:\Users\14429\.claude\
├── agents/
│   ├── agent-doc-explorer/agent.md    # 🔵 蓝色
│   ├── agent-arch-planner/agent.md    # 🟢 绿色
│   └── agent-tester/agent.md          # 🟠 橙色
└── Skills/
    └── agent-create/
        ├── SKILL.md
        ├── README.md
        └── LICENSE.txt
```

---

> 文档创建时间：2026-01-11
> 作者：Claude Code User
