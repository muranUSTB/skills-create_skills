# Dify 工作流 DSL 节点完整参考指南

本文档基于 Dify 官方文档和大量实际案例整理，涵盖所有常用节点的 YAML 配置写法，可作为搭建任意工作流的参考手册。

---

## 目录

1. [应用基础结构](#1-应用基础结构)
2. [Start 节点 - 开始](#2-start-节点---开始)
3. [LLM 节点 - 大语言模型](#3-llm-节点---大语言模型)
4. [Answer 节点 - 直接回复](#4-answer-节点---直接回复)
5. [Knowledge Retrieval 节点 - 知识检索](#5-knowledge-retrieval-节点---知识检索)
6. [Tool/Builtin Tool 节点 - 工具调用](#6-toolbuiltin-tool-节点---工具调用)
7. [Code 节点 - 代码执行](#7-code-节点---代码执行)
8. [HTTP Request 节点 - HTTP请求](#8-http-request-节点---http请求)
9. [If-Else 节点 - 条件分支](#9-if-else-节点---条件分支)
10. [Template 节点 - 模板转换](#10-template-节点---模板转换)
11. [Assigner 节点 - 变量赋值](#11-assigner-节点---变量赋值)
12. [Variable Aggregator 节点 - 变量聚合](#12-variable-aggregator-节点---变量聚合)
13. [Iteration 节点 - 循环](#13-iteration-节点---循环)
14. [Document Extractor 节点 - 文档提取](#14-document-extractor-节点---文档提取)
15. [Agent 节点 - 智能体](#15-agent-节点---智能体)
16. [变量引用语法](#16-变量引用语法)
17. [Edges 连接写法](#17-edges-连接写法)

---

## 1. 应用基础结构

```yaml
app:
  description: '应用描述'
  icon: 🤖
  icon_background: '#FFEAD5'
  mode: advanced-chat  # 或 workflow
  name: 应用名称
  use_icon_as_answer_icon: false
kind: app
version: {{参考案例的版本号}}
workflow:
  conversation_variables: []  # 会话变量
  environment_variables: []   # 环境变量
  features:
    file_upload:
      # 文件上传配置...
    retriever_resource:
      enabled: true
    text_to_speech:
      enabled: false
  graph:
    edges: []  # 连接线
    nodes: []  # 节点
    viewport:  # 视图位置
```

### Mode 模式说明

| 模式 | 说明 | 适用场景 |
|------|------|----------|
| `chat` | 基础聊天 | 简单对话应用 |
| `advanced-chat` | 高级聊天 | 复杂对话流程 |
| `workflow` | 工作流 | 非对话式自动化任务 |
| `agent-chat` | Agent聊天 | 智能体对话 |

---

## 2. Start 节点 - 开始

### 2.1 基础无变量

```yaml
- data:
    desc: ''
    selected: false
    title: 开始
    type: start
    variables: []  # 空变量列表
  height: 52
  id: '1741011655068'
  position:
    x: -191
    y: 263
  width: 242
```

### 2.2 文本输入变量

```yaml
- data:
    variables:
    - label: 问题输入
      max_length: 148
      options: []
      required: true
      type: text-input
      variable: user_input
  height: 90
  id: '1713261835258'
  position:
    x: 30
    y: 388.5
  width: 244
```

### 2.3 文件上传变量

```yaml
- data:
    variables:
    - allowed_file_extensions: []
      allowed_file_types:
      - image
      allowed_file_upload_methods:
      - local_file
      - remote_url
      label: file
      max_length: 48
      options: []
      required: true
      type: file
      variable: file
  height: 90
  id: '1729851066338'
  position:
    x: 0
    y: 277
  width: 244
```

### 2.4 变量类型对照表

| type | 说明 | 配置字段 |
|------|------|----------|
| `text-input` | 文本输入 | label, max_length, required |
| `file` | 文件上传 | label, allowed_file_types, allowed_file_extensions |
| `paragraph` | 多行文本 | label, max_length |
| `select` | 下拉选择 | label, options (数组) |
| `number` | 数字输入 | label, max, min |
| `boolean` | 开关 | label |

---

## 3. LLM 节点 - 大语言模型

### 3.1 基础 LLM 配置

```yaml
- data:
    context:
      enabled: true
      variable_selector:
      - '1741011662463'
      - result
    desc: ''
    model:
      completion_params:
        temperature: 0.7
      mode: chat  # 或 completion
      name: internlm/internlm2_5-7b-chat
      provider: siliconflow
    prompt_template:
    - id: ee1d7987-4534-40ac-ba95-acb929d54559
      role: system
      text: 请根据文本内容{{#context#}}回答
    selected: false
    title: LLM
    type: llm
    variables: []
    vision:
      enabled: false
  height: 96
  id: llm
  position:
    x: 382
    y: 263
  width: 242
```

### 3.2 多轮对话 Prompt 模板

```yaml
prompt_template:
- id: system-prompt-id
  role: system
  text: |
    # Role: 数学老师
    # Goal: 解答数学问题
    # Constraints:
    - 只能使用中文回答
    - 步骤要详细清晰

    用户问题: {{user_question}}
    上下文: {{context}}
- id: user-prompt-id
  role: user
  text: '{{#sys.query#}}'
- id: assistant-prompt-id
  role: assistant
  text: '以下是思考过程...'
```

### 3.3 带视觉能力的 LLM

```yaml
- data:
    vision:
      configs:
        detail: high
        variable_selector:
        - '1729851066338'
        - file
      enabled: true
    # ...其他配置
  height: 98
  id: llm
```

### 3.4 对话记忆配置

```yaml
- data:
    memory:
      query_prompt_template: '{{#sys.query#}}'
      role_prefix:
        assistant: '你是AI助手'
        user: '用户'
      window:
        enabled: true
        size: 10  # 保留最近10轮对话
    # ...其他配置
```

### 3.5 Model Provider 配置

| provider | 说明 | 配置示例 |
|----------|------|----------|
| `openai` | OpenAI | `name: gpt-4o` |
| `anthropic` | Anthropic | `name: claude-3-5-sonnet` |
| `siliconflow` | 硅基流动 | `name: internlm2_5-7b-chat` |
| `tongyi` | 通义千问 | `name: qwen-max` |
| `yi` | 零一万物 | `name: yi-large-turbo` |

---

## 4. Answer 节点 - 直接回复 vs End 节点 - 结束节点

### 4.1 节点对比（重要！）

| 特性 | Answer 节点 | End 节点 |
|-----|-------------|----------|
| **适用场景** | Chatflow（对话式应用） | Workflow（工作流应用） |
| **输出方式** | 流式输出文本 | 非流式输出 |
| **使用位置** | 可在流程中间步骤 | 只能在流程末尾 |
| **数量限制** | 可多个 | 必须唯一（Workflow） |
| **主要用途** | 对话回复 | 工作流结果输出 |

**关键区别**：
- **Workflow**：必须有且仅有一个 `End` 节点
- **Chatflow**：可使用多个 `Answer` 节点（支持流式输出）

### 4.2 Answer 节点 - Chatflow 专用

**用途**：在 Chatflow 中流式输出文本内容，支持 Markdown 格式

```yaml
- data:
    answer: '{{#llm.text#}}'
    desc: ''
    selected: false
    title: 直接回复
    type: answer
    variables: []
  height: 103
  id: answer
  position:
    x: 690
    y: 263
  width: 242
```

### 4.3 Markdown 格式回复

```yaml
- data:
    answer: |
      ## 标题

      - 项目1
      - 项目2

      | 列1 | 列2 |
      |------|------|
      | 内容1 | 内容2 |
  height: 155
  id: '1735192250756'
  title: 结果展示
  type: answer
```

### 4.4 带按钮的回复

```yaml
- data:
    answer: |
      <center><font size=4>游戏开始</font></center>

      点击开始按钮：
      <button data-size="small" data-variant="primary" data-message="#开始">开始</button>
  height: 178
  id: '1735183663452'
  type: answer
```

### 4.5 End 节点 - Workflow 专用

**用途**：Workflow 应用的结束节点，输出最终结果

```yaml
- data:
    outputs:
    - value_selector:
      - '上游节点ID'
      - text
      variable: result
    selected: false
    title: 结束
    type: end                     # ✅ 使用 type: end
  height: 103
  id: end
  position:
    x: 690
    y: 263
  width: 242
```

### 4.6 Workflow vs Chatflow 对比

| 特性 | Workflow | Chatflow |
|-----|----------|----------|
| 结束节点 | End（必须唯一） | Answer（可多个） |
| 会话变量 | ❌ 不支持 | ✅ 支持 |
| 适用场景 | 批处理/自动化任务 | 对话式交互 |
| 记忆功能 | ❌ 无 | ✅ 有 |
| 流式输出 | ❌ 无 | ✅ 支持 |

---

## 5. Knowledge Retrieval 节点 - 知识检索

### 5.1 单知识库检索

```yaml
- data:
    dataset_ids:
    - 51b9628b-0be9-4096-b77f-e6f5d0b8728c  # 知识库ID
    desc: ''
    query_variable_selector:
    - '1741011655068'
    - sys.query
    retrieval_mode: single
    selected: false
    single_retrieval_config:
      model:
        completion_params: {}
        mode: chat
        name: qwen-plus-chat
        provider: tongyi
    title: 知识检索
    type: knowledge-retrieval
  height: 90
  id: '1741011662463'
  position:
    x: 91
    y: 263
  width: 242
```

### 5.2 多知识库检索（带重排序）

```yaml
- data:
    dataset_ids:
    - 知识库ID1
    - 知识库ID2
    multiple_retrieval_config:
      reranking_enable: true
      reranking_mode: reranking_model  # 或 semantic
      reranking_model:
        model: netease-youdao/bce-reranker-base_v1
        provider: siliconflow
      top_k: 4
    query_variable_selector:
    - sys
    - query
    retrieval_mode: multiple
    title: 多知识库检索
    type: knowledge-retrieval
```

### 5.3 检索模式

| 模式 | 说明 | 适用场景 |
|------|------|----------|
| `single` | 单知识库 | 单一知识源 |
| `multiple` | 多知识库 | 多知识源合并检索 |
| `hybrid` | 混合检索 | 关键词+语义混合 |

---

## 6. Tool/Builtin Tool 节点 - 工具调用

### 6.1 搜索工具（Tavily）

```yaml
- data:
    provider_id: tavily
    provider_name: tavily
    provider_type: builtin
    selected: false
    title: TavilySearch
    tool_configurations:
      exclude_domains: null
      include_answer: 0
      include_domains: null
      include_images: 0
      include_raw_content: 0
      max_results: 10
      search_depth: basic
    tool_name: tavily_search
    tool_parameters:
      query:
        type: mixed
        value: '{{#1713261835258.Question#}}'
    type: tool
  height: 246
  id: '1713262010863'
```

### 6.2 TTS 文本转语音

```yaml
- data:
    provider_id: audio
    provider_name: audio
    provider_type: builtin
    title: Text To Speech
    tool_configurations:
      model: openai_api_compatible#tts-1
      voice#openai_api_compatible#tts-1: alloy
    tool_name: tts
    tool_parameters:
      text:
        type: mixed
        value: '{{#1738911112498.text#}}'
    type: tool
  height: 297
  id: '1738916723971'
```

### 6.3 图表工具（柱状图）

```yaml
- data:
    provider_id: chart
    provider_name: chart
    provider_type: builtin
    title: 柱状图
    tool_name: bar_chart
    tool_parameters:
      data:
        type: mixed
        value: '{{#1740636026594.score#}}'
      x_axis:
        type: mixed
        value: '{{#1740636026594.x_axis_data#}}'
    type: tool
  height: 52
  id: '1740640600640'
```

### 6.4 通用工具参数格式

```yaml
tool_parameters:
  参数名:
    type: mixed  # 或 constant, variable
    value: '{{#上游节点.输出变量#}}'
```

---

## 7. Code 节点 - 代码执行

### 7.1 基础 Python 代码

```yaml
- data:
    code: |
      import re
      import time

      def main(arg1) -> dict:
          time.sleep(13)
          urls = re.findall(r'http[s]?://[^\s)]+', arg1)
          return {
              "result": urls,
          }
    code_language: python3
    desc: ''
    outputs:
      result:
        children: null
        type: array[string]
    selected: false
    title: 提取URL
    type: code
    variables:
    - value_selector:
      - '1713262010863'
      - text
      variable: arg1
  height: 54
  id: '1713262060182'
```

### 7.2 返回多种数据类型

```yaml
outputs:
  status:
    children: null
    type: string
  message:
    children: null
    type: string
  data:
    children: null
    type: array[object]
  score:
    children: null
    type: string
  x_axis_data:
    children: null
    type: string
```

### 7.3 数据类型对照表

| type | 说明 | Python 返回示例 |
|------|------|-----------------|
| `string` | 字符串 | `"output": "结果"` |
| `number` | 数字 | `"output": 123` |
| `array[string]` | 字符串数组 | `"output": ["a", "b"]` |
| `array[object]` | 对象数组 | `"output": [{"key": "value"}]` |
| `object` | 对象 | `"output": {"key": "value"}` |

### 7.4 调用外部 API

```yaml
- data:
    code: |
      import requests
      import json

      def main(student_id: int, base_url: str = 'http://127.0.0.1:9090') -> dict:
          url = f'{base_url}/db/student/{student_id}/scores'
          try:
              response = requests.get(url)
              if response.status_code == 200:
                  scores = response.json()
                  return {
                      "status": "success",
                      "data": scores
                  }
              else:
                  return {"status": "error", "message": response.text}
          except Exception as e:
              return {"status": "error", "message": str(e)}
    code_language: python3
    outputs:
      data:
        type: array[object]
      message:
        type: string
      status:
        type: string
    variables:
    - value_selector:
      - conversation
      - student_id
      variable: student_id
    - value_selector:
      - env
      - base_url
      variable: base_url
```

---

## 8. HTTP Request 节点 - HTTP请求

### 8.1 GET 请求

```yaml
- data:
    authorization:
      config: null
      type: no-auth  # no-auth, api-key, bearer, basic
    body:
      data: []
      type: none  # none, json, form-data, x-www-form-urlencoded
    desc: ''
    headers: ''
    method: get
    params: ''
    selected: false
    timeout:
      max_connect_timeout: 10
      max_read_timeout: 30
      max_write_timeout: 10
    title: 获取天气数据
    type: http-request
    url: https://weather.cma.cn/api/climate?stationid=58367
    variables: []
```

### 8.2 POST 请求（JSON）

```yaml
- data:
    authorization:
      type: bearer
      config:
        token: '{{#env.API_TOKEN#}}'
    body:
      data:
      - key: content
        type: text
        value: '{{#sys.query#}}'
      - key: max_tokens
        type: number
        value: '1000'
      type: json
    headers: |
      Content-Type: application/json
      X-Custom-Header: custom-value
    method: post
    title: API调用
    type: http-request
    url: https://api.example.com/v1/generate
```

### 8.3 认证类型

| type | 配置示例 |
|------|----------|
| `no-auth` | 无需认证 |
| `api-key` | `{"api_key": "your-key"}` |
| `bearer` | `{"token": "your-token"}` |
| `basic` | `{"username": "user", "password": "pass"}` |
| `aws-sign` | AWS签名认证 |

---

## 9. If-Else 节点 - 条件分支

### 9.1 基础条件分支

```yaml
- data:
    cases:
    - case_id: 'true'  # 条件为真时的分支ID
      conditions:
      - comparison_operator: contains
        id: c0016bf9-a7cd-4b2c-9d8f-14fc5661977c
        value: '1'
        varType: string
        variable_selector:
        - llm
        - text
      id: 'true'
      logical_operator: and  # and, or
    - case_id: '060040ea-2780-45b2-9d82-43b4a348a99d'
      conditions:
      - comparison_operator: contains
        value: '0'
        varType: string
        variable_selector:
        - llm
        - text
      id: 特殊条件分支ID
    desc: ''
    selected: false
    title: 条件分支
    type: if-else
  height: 268
  id: '1740634251160'
```

### 9.2 比较运算符

| operator | 说明 | 示例 |
|----------|------|------|
| `=` | 等于 | `value: '1'` |
| `!=` | 不等于 | `value: 'error'` |
| `contains` | 包含 | `value: '关键词'` |
| `not contains` | 不包含 | `value: '敏感词'` |
| `>` | 大于 | `value: '100'` |
| `<` | 小于 | `value: '0'` |
| `>=` | 大于等于 | `value: '60'` |
| `<=` | 小于等于 | `value: '0'` |
| `is` | 是 | `value: 'running'` |
| `is not` | 不是 | `value: 'pending'` |

### 9.3 多条件组合

```yaml
cases:
- id: 'true'
  conditions:
  - comparison_operator: '='
    value: 'running'
    varType: string
    variable_selector:
    - conversation
    - status
  - comparison_operator: '>'
    value: '0'
    varType: number
    variable_selector:
    - code_node
    - count
  logical_operator: and  # 所有条件都满足
```

---

## 10. Template 节点 - 模板转换

### 10.1 基础模板

```yaml
- data:
    desc: ''
    selected: false
    template: |
      |URL|Summary|
      |---|---|
      {% for data in arg1 -%}
      |{{data.url}}|{{data.text}}|
      {% endfor -%}
    title: 结果表格
    type: template-transform
    variables:
    - value_selector:
      - '1720759755103'
      - result
      variable: arg1
  height: 54
  id: '1720761482451'
```

### 10.2 Jinja2 模板语法

```yaml
template: |
  {# 变量引用 #}
  {{ user_name }}

  {# 嵌套属性 #}
  {{ user.profile.name }}

  {# 数组遍历 #}
  {% for item in items %}
    {{ item.title }}
  {% endfor %}

  {# 条件判断 #}
  {% if status == 'success' %}
    成功
  {% else %}
    失败
  {% endif %}

  {# 过滤器 #}
  {{ text | upper }}
  {{ items | join(", ") }}
  {{ content | length }}
```

### 10.3 常用过滤器

| 过滤器 | 说明 | 示例 |
|--------|------|------|
| `upper` | 大写 | `{{ name | upper }}` |
| `lower` | 小写 | `{{ name | lower }}` |
| `join` | 数组连接 | `{{ arr | join(", ") }}` |
| `length` | 长度 | `{{ arr | length }}` |
| `default` | 默认值 | `{{ val | default("N/A") }}` |
| `trim` | 去除空格 | `{{ text | trim }}` |

---

## 11. Assigner 节点 - 变量赋值器

### 11.1 节点功能说明

**用途**：将值写入**会话变量**或覆盖变量值

**使用场景**：
- 存储用户偏好信息
- 提取和保存记忆
- 更新会话状态

**重要**：不是用于"整合多个内容"，那是 variable-aggregator 的功能。

### 11.2 基础用法

```yaml
- data:
    desc: ''
    items:
    - input_type: constant
      operation: set
      value: start
      variable_selector:
      - conversation
      - status
      write_mode: over-write
    selected: false
    title: 启动状态
    type: assigner
    version: '2'
  height: 86
  id: '1735183411877'
```

### 11.2 复制变量值

```yaml
- data:
    items:
    - input_type: variable
      operation: over-write
      value:
      - '1735195133945'
      - status
      variable_selector:
      - conversation
      - status
      write_mode: over-write
    - input_type: variable
      operation: over-write
      value:
      - '1735195133945'
      - game_level
      variable_selector:
      - conversation
      - game_level
      write_mode: over-write
    title: 更新状态
    type: assigner
    version: '2'
  height: 170
  id: '1735201350381'
```

### 11.3 操作类型

| operation | 说明 |
|-----------|------|
| `set` | 覆盖写入 |
| `append` | 追加到数组 |
| `extend` | 扩展数组 |
| `add` | 数值相加 |
| `sub` | 数值相减 |

### 11.4 写入模式

| write_mode | 说明 |
|------------|------|
| `over-write` | 覆盖 |
| `append` | 追加 |

---

## 12. Variable Aggregator 节点 - 变量聚合器

### 12.1 节点功能说明

**用途**：将**多分支输出聚合为单路输出**，确保无论哪个分支被执行，结果都能通过统一变量引用

**使用场景**：
- IF/ELSE 条件分支后的多路聚合
- 并行结构中的多路输出聚合
- 问题分类后的多路聚合

**核心价值**：避免下游节点重复定义，简化数据流管理

**⚠️ 重要区别**：
- **assigner（变量赋值器）**：写入会话变量
- **variable-aggregator（变量聚合器）**：聚合多分支输出（不是"简单整合内容"）

### 12.2 基础配置

```yaml
- data:
    desc: 将多个分支的输出聚合为统一变量
    output_type: string           # 聚合结果的输出类型
    selected: false
    title: 变量聚合器
    type: variable-aggregator
    variables:                    # 要聚合的变量列表（二维数组）
    - - '分支节点ID1'              # 第一个变量
      - output
    - - '分支节点ID2'              # 第二个变量
      - output
    - - '分支节点ID3'              # 第三个变量
      - output
  height: 211
  id: '1736326851692'
```

### 12.3 输出类型

| output_type | 说明 |
|-------------|------|
| `string` | 字符串 |
| `array` | 数组 |
| `object` | 对象 |

### 12.4 聚合分组

开启聚合分组后，可以聚合多组变量，各组内要求同一种数据类型。

---

## 13. Iteration 节点 - 循环

⚠️ **【重要】迭代节点是 DSL 中最容易出错的部分，缺少任何一项都会导致导入失败！**

### 13.1 完整迭代结构（必须包含所有组件）

```yaml
# ============================================
# 1. iteration 节点 - 循环控制器（必须配置 start_node_id）
# ============================================
- data:
    desc: ''
    iterator_selector:           # 要遍历的数组来源
    - '1741011655068'
    - text
    output_selector:             # 循环内输出
    - '1741011662463'
    - result
    output_type: array[object]   # ⚠️ 必须格式
    selected: false
    start_node_id: 1741011600006start  # ⚠️ 必须指向 iteration-start
    title: 批量翻译
    type: iteration
  height: 377
  id: '1741011600006'
  position:
    x: 200
    y: 100

# ============================================
# 2. iteration-start 子节点（必须有！不是"内置"的）
# ============================================
- data:
    desc: ''
    selected: false
    title: 迭代开始
    type: custom-iteration-start
  height: 52
  id: 1741011600006start
  parentId: '1741011600006'    # ⚠️ 必须指向父迭代节点
  position:
    x: 200
    y: 200
  width: 242

# ============================================
# 3. 迭代内部节点（必须包含以下标记）
# ============================================
- data:
    desc: ''
    isInIteration: true         # ⚠️ 必须标记在迭代内
    iteration_id: '1741011600006'  # ⚠️ 必须标识所属迭代
    model:
      provider: siliconflow
      name: deepseek-chat
      mode: chat
    prompt_template:
    - role: user
      text: '翻译这段文本：{{#1741011600006.item#}}'
    selected: false
    title: LLM翻译
    type: llm
  height: 96
  id: '1741011662463'
  parentId: '1741011600006'    # ⚠️ 必须指向父迭代
  position:
    x: 200
    y: 300
  width: 242
```

### 13.2 迭代内 Edge 连接（必须包含 zIndex: 1002）

```yaml
# ============================================
# 迭代内连接（type 必须是 isInIteration）
# ============================================
- data:
    isInIteration: true         # ⚠️ 必须
    iteration_id: '1741011600006'
    sourceType: custom-iteration-start
    targetType: llm
  id: iter-start-llm
  source: 1741011600006start   # 指向 iteration-start
  sourceHandle: source
  target: '1741011662463'      # 指向迭代内节点
  targetHandle: target
  type: isInIteration          # ⚠️ 必须是这个类型
  zIndex: 1002                 # ⚠️ 必须的渲染层级

# ============================================
# 迭代结束后的连接（迭代外用 custom）
# ============================================
- data:
    isInIteration: false
    sourceType: iteration
    targetType: answer
  id: iter-answer
  source: '1741011600006'
  target: answer
  type: custom
```

### 13.3 ❌ 常见错误对比

| 问题 | 错误写法 | 正确写法 |
|------|----------|----------|
| 缺少 iteration-start | 无此节点 | 必须有 `custom-iteration-start` 子节点 |
| 迭代节点缺少 start_node_id | 无此配置 | `start_node_id: 1741011600006start` |
| 迭代内部节点缺少 parentId | 无此配置 | `parentId: '1741011600006'` |
| 迭代内部节点缺少 isInIteration | 无此配置 | `isInIteration: true` |
| 迭代内部节点缺少 iteration_id | 无此配置 | `iteration_id: '1741011600006'` |
| 迭代边缺少 zIndex | 无此配置 | `zIndex: 1002` |
| 迭代边 type 错误 | `type: custom` | `type: isInIteration` |
| output_type 格式错误 | `output_type: array` | `output_type: array[object]` |

### 13.4 迭代内变量引用

循环内使用 `{{#迭代节点ID.item#}}` 引用当前迭代项：

```yaml
# LLM prompt 中引用当前项
prompt_template:
- role: user
  text: '请翻译：{{#1741011600006.item#}}'
```

### 13.5 成功导入的关键检查清单

生成迭代工作流后，请检查以下 5 点：

- [ ] **iteration 节点** 有 `start_node_id` 指向 iteration-start
- [ ] **iteration-start 节点** 有 `parentId` 指向迭代节点
- [ ] **迭代内每个节点** 都有 `parentId` + `isInIteration: true` + `iteration_id`
- [ ] **迭代内每条边** 都有 `isInIteration: true` + `zIndex: 1002`
- [ ] **output_type** 格式为 `array[object]`

---

## 14. Document Extractor 节点 - 文档提取

```yaml
- data:
    desc: ''
    is_array_file: true  # 是否支持多文件数组
    selected: false
    title: 文档提取器
    type: document-extractor
    variable_selector:
    - sys
    - files
  height: 94
  id: '1729851603790'
```

---

## 15. Agent 节点 - 智能体

```yaml
- data:
    agent_parameters:
      model:
        type: constant
        value:
          completion_params: {}
          mode: chat
          model: gpt-4o-mini
          model_type: llm
          provider: langgenius/openai/openai
          type: model-selector
      query:
        type: constant
        value: '{{#sys.query#}}'
      storage_key:
        type: constant
        value: '{{#sys.conversation_id#}}'
      task_schema:
        type: constant
        value: |
          {
            "fields": [
              {"name": "destination", "question": "请问您想去哪里旅行？", "required": true},
              {"name": "duration", "question": "您计划旅行多长时间？", "required": true}
            ]
          }
    agent_strategy_label: 多轮对话
    agent_strategy_name: TOD
    agent_strategy_provider_name: afeaad50-xxx/agent/agent
    desc: ''
    output_schema: null
    plugin_unique_identifier: afeaad50-xxx/agent:0.0.1@xxx
    selected: false
    title: Agent
    type: agent
  height: 144
  id: '1740645185279'
```

---

## 16. 变量引用语法

### 16.1 基础引用

```yaml
# 节点ID + 输出变量名
{{#节点ID.输出变量#}}

# 示例
{{#llm.text#}}
{{#code_node.result#}}
{{#1735195133945.status#}}
```

### 16.2 系统变量

| 变量路径 | 说明 |
|----------|------|
| `{{#sys.query#}}` | 用户输入 |
| `{{#sys.conversation_id#}}` | 会话ID |
| `{{#sys.files#}}` | 上传文件 |

### 16.3 特殊变量来源

| 来源 | 引用格式 | 说明 |
|------|----------|------|
| Start节点 | `{{#节点ID.变量名#}}` | 用户输入变量 |
| Code节点 | `{{#节点ID.输出名#}}` | 代码输出 |
| LLM节点 | `{{#节点ID.text#}}` | LLM输出文本 |
| 会话变量 | `{{#conversation.变量名#}}` | 跨轮次持久变量 |
| 环境变量 | `{{#env.变量名#}}` | 环境配置变量 |

---

## 17. Edges 连接写法

### 17.1 基础连接

```yaml
- data:
    sourceType: start
    targetType: llm
  id: start-llm
  source: start  # 源节点ID
  sourceHandle: source
  target: llm    # 目标节点ID
  targetHandle: target
  type: custom
```

### 17.2 If-Else 分支连接

```yaml
- data:
    isInIteration: false
    sourceType: if-else
    targetType: answer
  id: if-true-answer
  source: '1740634251160'
  sourceHandle: 'true'   # 分支条件：true
  target: answer
  targetHandle: target
  type: custom

- data:
    sourceType: if-else
    targetType: code
  id: if-false-code
  source: '1740634251160'
  sourceHandle: 'false'  # 分支条件：false
  target: '1740636026594'
```

### 17.3 自定义分支 ID 连接

```yaml
- data:
    sourceType: if-else
    targetType: answer
  id: if-custom-answer
  source: '1735183233430'
  sourceHandle: 9ca767d9-54ea-407e-a0a3-a8dc3e55e118  # 自定义case_id
  target: '1735200055854'
```

### 17.4 循环内连接

⚠️ **迭代内边必须有 `isInIteration: true` 和 `zIndex: 1002`！**

```yaml
# ============================================
# 迭代内连接（从 iteration-start 到迭代内节点）
# ============================================
- data:
    isInIteration: true         # ⚠️ 必须
    iteration_id: '1741011600006'
    sourceType: custom-iteration-start
    targetType: llm
  id: iter-start-llm
  source: 1741011600006start
  sourceHandle: source
  target: '1741011662463'
  targetHandle: target
  type: isInIteration          # ⚠️ 必须是这个
  zIndex: 1002                 # ⚠️ 必须的渲染层级

# ============================================
# 迭代内连接（迭代内节点之间）
# ============================================
- data:
    isInIteration: true         # ⚠️ 必须
    iteration_id: '1741011600006'
    sourceType: llm
    targetType: code
  id: iter-llm-code
  source: '1741011662463'
  sourceHandle: source
  target: '1741011678901'
  targetHandle: target
  type: isInIteration          # ⚠️ 必须是这个
  zIndex: 1002                 # ⚠️ 必须的渲染层级
```

---

## 18. 完整工作流示例

### 18.1 知识库问答工作流

```yaml
app:
  mode: advanced-chat
  name: 知识库问答
workflow:
  graph:
    edges:
    - source: start
      target: knowledge-retrieval
      type: custom
    - source: knowledge-retrieval
      target: llm
      type: custom
    - source: llm
      target: answer
      type: custom
    nodes:
    - id: start
      position: {x: -191, y: 263}
      type: start
      data:
        type: start
        title: 开始
        variables: []

    - id: knowledge-retrieval
      position: {x: 91, y: 263}
      type: knowledge-retrieval
      data:
        type: knowledge-retrieval
        title: 知识检索
        dataset_ids: [知识库ID]
        query_variable_selector: [start, sys.query]

    - id: llm
      position: {x: 382, y: 263}
      type: llm
      data:
        type: llm
        title: LLM
        model:
          provider: siliconflow
          name: internlm2_5-7b-chat
          mode: chat
        context:
          enabled: true
          variable_selector: [knowledge-retrieval, result]
        prompt_template:
        - role: system
          text: 请根据以下内容回答问题：{{context}}

    - id: answer
      position: {x: 690, y: 263}
      type: answer
      data:
        type: answer
        title: 直接回复
        answer: '{{#llm.text#}}'
```

---

## 附录

### A. 会话变量配置

```yaml
conversation_variables:
- description: 游戏状态
  id: xxx
  name: status
  selector: [conversation, status]
  value: pending
  value_type: string
- description: 计数器
  id: xxx
  name: count
  selector: [conversation, count]
  value: '0'
  value_type: number
```

### B. 环境变量配置

```yaml
environment_variables:
- description: API基础地址
  id: xxx
  name: base_url
  selector: [env, base_url]
  value: http://127.0.0.1:9090/
  value_type: string
- description: 阈值
  id: xxx
  name: top_n
  selector: [env, top_n]
  value: '50'
  value_type: number
```

### C. 文件上传配置

```yaml
file_upload:
  allowed_file_extensions:
  - .JPG
  - .PNG
  - .PDF
  allowed_file_types:
  - image
  - document
  allowed_file_upload_methods:
  - local_file
  - remote_url
  enabled: true
  fileUploadConfig:
    audio_file_size_limit: 50
    batch_count_limit: 5
    file_size_limit: 15
    image_file_size_limit: 10
    video_file_size_limit: 100
  image:
    enabled: true
    number_limits: 3
    transfer_methods:
    - local_file
    - remote_url
  number_limits: 10
```

---

## 19. DSL 自动生成规则

### 19.1 版本号规则

```yaml
# 生成 DSL 时，版本号应与参考案例保持一致
version: {{参考案例的版本号}}    # ✅ 正确
```

**要点：** 版本号不是固定值，应跟随所选参考案例的版本。

### 19.2 节点基础字段规则

**生成任何节点时，必须包含以下完整字段：**

```yaml
- data:
    positionAbsolute: false       # ✅ 必须
    selected: false               # ✅ 必须
    title: 节点标题               # ✅ 必须
    type: 节点类型                # ✅ 必须
    # ... 其他类型特定字段
  height: 52                      # 节点高度
  id: '节点ID'                    # 唯一标识
  position:                       # 画布位置
    x: 0
    y: 0
  width: 242                      # 节点宽度
```

### 19.3 Edges 完整字段规则

**生成任何边时，必须包含以下完整字段：**

```yaml
- data:
    isInIteration: false          # ✅ 必须
    selected: false               # ✅ 必须
    sourceType: 源节点类型         # ✅ 必须
    targetType: 目标节点类型       # ✅ 必须
  id: 边ID                        # 唯一标识
  source: 源节点ID                 # ✅ 必须
  sourceHandle: source            # ✅ 必须
  target: 目标节点ID               # ✅ 必须
  targetHandle: target            # ✅ 必须
  type: custom|true|false|isInIteration  # ✅ 必须
```

### 19.4 variable-aggregator 节点生成规则

```yaml
# ✅ 正确写法 - 生成时必须遵循
- data:
    output_type: string           # 使用 output_type，不是 outputs
    type: variable-aggregator
    variables:                    # 使用数组格式，不是 formatter_template
    - - '上游节点ID1'              # 二维数组
      - text                      # 输出字段名
    - - '上游节点ID2'
      - text
    - - '上游节点ID3'
      - text
  height: 211
  id: '聚合节点ID'

# ❌ 错误写法 - 禁止使用
# outputs: [...]        # 错误！
# formatter_template: [...]  # 错误！
```

### 19.5 end 节点生成规则

```yaml
# ✅ 正确写法 - 生成时必须遵循
- data:
    outputs:                      # 使用 outputs，不是 answer
    - value_selector:
      - '上游节点ID'
      - text
      variable: output
    selected: false
    title: 结束
    type: end                     # 使用 type: end，不是 type: answer
  height: 103
  id: end
  position:
    x: 690
    y: 263
  width: 242

# ❌ 错误写法 - 禁止使用
# type: answer          # 错误！
# answer: '{{#...#}}'   # 错误！
```

### 19.6 DSL 生成后自检清单

生成 DSL 后，逐项检查以下项目，发现错误立即修正：

**【版本号检查】**
- [ ] 版本号与参考案例一致

**【节点字段检查】**
- [ ] 每个节点有 `data.positionAbsolute: false`
- [ ] 每个节点有 `data.selected: false`
- [ ] 每个节点有 `height` 和 `width`

**【边字段检查】**
- [ ] 每条边有 `data.sourceType`
- [ ] 每条边有 `data.targetType`
- [ ] 每条边有 `data.selected: false`
- [ ] 每条边有 `data.isInIteration`

**【variable-aggregator 检查】**
- [ ] 使用 `output_type`（不是 `outputs`）
- [ ] 使用 `variables` 数组格式（不是 `formatter_template`）

**【end 节点检查】**
- [ ] `type: end`（不是 `type: answer`）
- [ ] 使用 `outputs` 配置（不是 `answer`）

### 19.7 生成器伪代码

```python
def generate_dsl():
    # 1. 版本号（跟随参考案例）
    dsl["version"] = reference_case["version"]

    # 2. 生成节点
    for node in nodes:
        node["data"]["positionAbsolute"] = False
        node["data"]["selected"] = False
        # ...

    # 3. 生成边
    for edge in edges:
        edge["data"]["selected"] = False
        edge["data"]["isInIteration"] = False
        # ...

    # 4. 特殊节点处理
    if node["data"]["type"] == "variable-aggregator":
        # 使用 output_type + variables
        node["data"]["output_type"] = "string"
        node["data"]["variables"] = [[node_id, "text"], ...]

    if node["data"]["type"] == "end":
        # 使用 type: end + outputs
        node["data"]["type"] = "end"
        node["data"]["outputs"] = [...]
```

---

> 文档版本: 1.1
> 最后更新: 2026-01-03
> 基于 Dify 官方文档和 organized_dsl 案例库整理
