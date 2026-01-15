# Agent Create

专业的 AI Agent 开发助手，支持 LangGraph、AgentScope、DeepAgent、AutoGen、CrewAI 等主流框架的完整开发流程。

## 简介

Agent Create 是一个专业级的 Agent 开发工具，帮助用户从需求分析到代码实现，完整地搭建各类 AI Agent 框架。无论是简单的对话机器人，还是复杂的多 Agent 协作系统，都能通过本 Skill 快速构建。

## 功能特性

- **多框架支持**：LangGraph、AgentScope、DeepAgent、AutoGen、CrewAI
- **完整工作流**：需求 → PRD → 文档探索 → 结构规划 → 代码开发 → 测试验证
- **智能规划**：基于文档分析的最佳实践建议
- **Subagent 协作**：独立执行探索和测试任务，不占用主上下文
- **完整输出**：代码 + 文档 + 测试，一站式交付

## 支持的框架

| 框架 | 特点 | 适用场景 |
|-----|------|---------|
| LangGraph | 循环工作流、状态管理强 | 复杂对话、任务编排 |
| AgentScope | 分散式架构、易用性好 | 快速原型、多 Agent 协作 |
| DeepAgent | 深度学习集成、研究导向 | 研究实验、高级特性 |
| AutoGen | 多 Agent 对话、自动化强 | 企业应用、工作流自动化 |
| CrewAI | 角色扮演、编排简洁 | 团队模拟、角色协作 |

## 快速开始

```bash
# 使用 Skill
/agent-create --framework=langgraph --project_name=my-agent
```

## 详细文档

请参阅 [SKILL.md](SKILL.md) 获取完整文档，包括：

- 完整工作流说明
- 输入参数详解
- 输出文件格式
- 使用示例
- 错误处理指南

## 工作流概览

```
阶段 1: 需求收集 → 阶段 2: PRD 生成 → 阶段 3: 文档探索(Subagent)
     ↓                                      ↓
阶段 4: 结构规划(Subagent) ←───────────────/
     ↓
阶段 5: 代码开发 ←──────────────────────────/
     ↓
阶段 6: 测试验证(Subagent)
```

## 许可证

本项目采用 CC BY-NC-SA 4.0 许可证。
