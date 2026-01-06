# 这是沐然的skills插件市场

## 已有的 Skills

### 1. Skill Creator（技能创建工具）

**目录**：`skills/skills-creator/`

专业级 Claude Code Skill 创建工具，帮助用户从零开始设计和实现自定义的 Skill。

**功能特点**：
- 多模板支持：内置 8 种常见 Skill 模板
- 引导式创建：分步骤引导用户确认所有关键信息
- 完整输出：自动生成完整的 Skill 文件
- 最佳实践：遵循 Claude Code Skill 设计规范

**使用方式**：
```
请帮我创建一个新的 Skill。
```

### 2. Dify 工作流生成器

**目录**：`skills/dify_creator/`

通过多轮对话引导用户明确需求，生成可直接导入 Dify 的工作流 DSL YAML 配置。

**功能特点**：
- 智能对话引导：多轮问答帮助用户梳理需求
- 参考学习：基于现有 Dify 案例自动匹配最佳实践
- 完整 DSL 生成：输出符合 Dify 2.0 规范的完整配置
- 多类型支持：Chatflow、Workflow、Advanced Chat 全面覆盖

**使用方式**：
```
我想创建一个 Dify 工作流，用于...
```

## 如何使用这些 Skills

### 方式一：直接使用

1. 将 skill 目录复制到你的 Claude Code Skills 目录
2. 在 Claude Code 中加载该 Skill
3. 按照 Skill 的引导开始使用

### 方式二：作为参考

这些 Skills 的代码结构清晰，可以作为创建新 Skills 的参考模板。

## Skill 目录结构规范

一个标准的 Skill 应包含以下文件：

```
skill-name/
├── SKILL.md           # 主 Skill 文件（必需）
├── README.md          # 说明文档（推荐）
├── LICENSE.txt        # 许可证文件（推荐）
└── references/        # 参考资料目录（可选）
    └── templates/     # 模板文件
```

### SKILL.md 文件格式

```markdown
---
name: skill-name
description: 一句话描述这个 Skill 的功能
license: LICENSE-CC-BY-NC-SA 4.0 in LICENSE.txt
author: 作者名
---

# Skill 名称

[详细介绍]

## 功能特点

- 功能点 1
- 功能点 2

## 工作流

[详细的工作流程]

## 使用示例

[使用示例]

## 注意事项

[注意事项]
```

## 创建新的 Skill

使用 Skill Creator 可以快速创建新的 Skill：

1. 激活 Skill Creator
2. 按照引导提供 Skill 名称、描述、模板类型
3. 确认功能需求和工作流程
4. 自动生成完整的 Skill 文件

## 项目结构

```
skill-creator/
├── README.md                    # 本文件
├── skills/
│   ├── skills-creator/          # Skill 创建工具
│   │   ├── SKILL.md
│   │   ├── README.md
│   │   ├── LICENSE.txt
│   │   └── references/
│   │       └── templates/
│   │           ├── 01_document_processing.md
│   │           ├── 02_data_analysis.md
│   │           ├── 03_browser_operation.md
│   │           ├── 04_content_creation.md
│   │           ├── 05_external_service.md
│   │           ├── 06_utility_tool.md
│   │           ├── 07_code_development.md
│   │           └── 08_custom_template.md
│   │
│   └── dify_creator/            # Dify 工作流生成器
│       ├── SKILL.md
│       ├── README.md
│       └── organized_dsl/
│           ├── INDEX.md
│           ├── Dify_DSL_节点完整参考指南.md
│           ├── 01_内容生成与创作/
│           ├── 02_图像生成与设计/
│           └── ...
```

## 许可证

本项目采用 CC BY-NC-SA 4.0 许可证。

这意味着您可以：
- **共享**：以任何媒介或格式复制和分发本材料
- **演绎**： remix、转化、依赖本材料进行创作

但须遵守以下条件：
- **署名**：您必须给予适当的信用
- **非商业**：您不得将本材料用于商业目的
- **相同方式共享**：如果您演绎本材料，必须使用相同的许可证

## 作者

**沐然** - Claude Code Skills 集合维护者

- GitHub：https://github.com/muranUSTB
- 邮箱：hmr1442953114@163.com

## 贡献

欢迎对本项目进行贡献！您可以：
- 报告问题
- 提出功能建议
- 提交新的 Skill
- 完善文档

## 更新日志

### v1.0.0 (2025-01-06)
- 初始化项目
- 添加 Skill Creator
- 添加 Dify 工作流生成器
