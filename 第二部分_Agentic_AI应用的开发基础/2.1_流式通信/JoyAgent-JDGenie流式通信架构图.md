# JoyAgent-JDGenie 流式通信架构图

## 流式通信整体架构图

```mermaid
graph TB
    %% 用户层
    subgraph "用户交互层"
        UI["前端服务<br/>React + TypeScript<br/>端口: 3000<br/>&nbsp;"]
    end
    
    %% 核心服务层  
    subgraph "核心服务层"
        Backend["后端服务 <br/> 智能体调度中心<br/>Java Spring Boot<br/>端口: 8080<br/>&nbsp;"]
    end
    
    %% 工具服务层
    subgraph "工具服务层"
        ToolService["工具服务<br/>Python FastAPI 端口: 1601<br/>Python代码解释器 / 搜索 / 报告<br/>&nbsp;"]
        MCPClient["MCP客户端<br/>Python FastAPI 端口: 8188<br/>外部工具集成<br/>&nbsp;"]
    end
    
    %% 外部服务层
    subgraph "外部服务层"
        MCPServer["外部MCP服务<br/>标准MCP协议<br/>第三方工具<br/>&nbsp;"]
        LLM["大语言模型<br/>OpenAI / Claude / DeepSeek<br/>API调用<br/>&nbsp;"]
    end
    
    %% 连接关系 - 流式通信链路
    UI -->|" HTTP + SSE / 流式响应 "| Backend
    Backend -->|" HTTP + SSE / 工具调用 "| ToolService  
    Backend -->|" HTTP请求 / MCP工具调用 "| MCPClient
    MCPClient -->|" MCP over SSE / 标准协议 "| MCPServer
    
    %% LLM流式调用关系
    Backend -->|" HTTP + SSE / 模型调用 "| LLM
    ToolService -->|" HTTP + SSE / 模型调用 "| LLM
    
    %% 样式
    classDef frontendClass fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef backendClass fill:#f3e5f5,stroke:#4a148c,stroke-width:2px  
    classDef toolClass fill:#e8f5e8,stroke:#1b5e20,stroke-width:2px
    classDef externalClass fill:#fff3e0,stroke:#e65100,stroke-width:2px
    
    class UI frontendClass
    class Backend backendClass
    class ToolService,MCPClient toolClass
    class MCPServer,LLM externalClass


```

## 流式通信详细流程图

```mermaid
sequenceDiagram
    participant U as 用户界面<br/>(React)
    participant B as 后端服务<br/>(Java Spring)
    participant T as 工具服务<br/>(Python FastAPI)
    participant L as 大语言模型<br/>(LLM API)
    participant M as MCP客户端<br/>(Python FastAPI)
    participant E as 外部MCP服务
    
    Note over U,E: 用户发起查询请求
    
    U->>+B: POST /AutoAgent<br/>建立SSE连接
    B-->>U: SSE连接建立<br/>开始心跳
    
    Note over B,L: 智能体任务规划阶段
    B->>+L: HTTP + SSE<br/>规划思考调用<br/>(PlanningAgent.askTool)
    L-->>B: SSE流式响应<br/>规划思考内容
    B-->>U: SSE: 规划思考过程<br/>messageType: plan_thought
    L-->>-B: 工具调用决策<br/>(planning tool_call)
    B-->>U: SSE: 任务计划<br/>messageType: plan
    
    Note over B,L: 任务执行阶段
    B->>+L: HTTP + SSE<br/>工具选择调用<br/>(ExecutorAgent.askTool)
    L-->>B: SSE流式响应<br/>工具思考内容
    B-->>U: SSE: 工具思考<br/>messageType: tool_thought
    L-->>-B: 工具调用决策<br/>(tool_calls)
    
    Note over B,T: 执行代码解释器工具
    B->>+T: POST /code_interpreter<br/>SSE流式请求
    T->>+L: HTTP + SSE<br/>代码生成调用<br/>(ask_llm with litellm)
    L-->>T: SSE流式响应<br/>代码生成内容
    T-->>B: SSE: 代码生成进度<br/>实时输出
    L-->>-T: 代码生成完成
    T-->>-B: SSE: 执行完成结果<br/>包含文件信息
    B-->>U: SSE: 工具结果<br/>messageType: tool_result
    
    Note over B,M: 调用外部MCP工具
    B->>+M: POST /v1/tool/call<br/>MCP工具调用
    M->>+E: MCP over SSE<br/>建立连接
    E-->>M: MCP响应<br/>工具执行结果
    M-->>-B: HTTP响应<br/>返回结果
    B-->>U: SSE: 外部工具结果<br/>messageType: tool_result
    
    Note over B,L: 最终总结阶段
    B->>+L: HTTP + SSE<br/>总结生成调用<br/>(SummaryAgent.ask)
    L-->>B: SSE流式响应<br/>总结内容
    B-->>U: SSE: 流式总结<br/>messageType: agent_stream
    L-->>-B: 总结完成
    
    Note over B,T: 生成最终报告(可选)
    B->>+T: POST /report<br/>SSE流式生成
    T->>+L: HTTP + SSE<br/>报告生成调用<br/>(ask_llm with litellm)
    L-->>T: SSE流式响应<br/>HTML报告内容
    T-->>B: SSE: 报告生成进度<br/>逐步输出内容
    L-->>-T: 报告生成完成
    T-->>-B: SSE: 报告生成完成<br/>包含文件信息
    B-->>U: SSE: 最终结果<br/>messageType: result
    
    B-->>-U: SSE连接关闭<br/>任务完成
```

## 大语言模型调用架构

```mermaid
graph LR
    subgraph Tool["工具服务 LLM 调用"]
        T1[代码解释器<br/>code_interpreter]
        T2[报告生成<br/>report]
        T3[深度搜索<br/>deepsearch]
    end
    
    subgraph Agent["后端智能体引擎 LLM 调用"]
        A1[PlanningAgent<br/>任务规划智能体]
        A2[ExecutorAgent<br/>任务执行智能体]
        A3[ReactImplAgent<br/>ReAct智能体]
        A4[SummaryAgent<br/>总结智能体]
    end
    
    subgraph Client["LLM 调用层"]
        B1[Python LLM 工具<br/>ask_llm with litellm]
        B2[Java LLM 类<br/>getLlm.askTool]
    end
    
    subgraph LLM["大语言模型服务"]
        L1[OpenAI API]
        L2[Claude API]
        L3[DeepSeek API]
        L4[其他兼容 API]
    end
    
    T1 -->|流式调用| B1
    T2 -->|流式调用| B1
    T3 -->|流式调用| B1
    
    A1 -->|流式调用| B2
    A2 -->|流式调用| B2
    A3 -->|流式调用| B2
    A4 -->|流式调用| B2
    
    B1 -->|HTTP + SSE| L1
    B1 -->|HTTP + SSE| L2
    B1 -->|HTTP + SSE| L3
    B1 -->|HTTP + SSE| L4
    
    B2 -->|HTTP + SSE| L1
    B2 -->|HTTP + SSE| L2
    B2 -->|HTTP + SSE| L3
    B2 -->|HTTP + SSE| L4
    
    style T1 fill:#e8f5e9,stroke:#388e3c
    style T2 fill:#e8f5e9,stroke:#388e3c
    style T3 fill:#e8f5e9,stroke:#388e3c
    
    style A1 fill:#e3f2fd,stroke:#1976d2
    style A2 fill:#e3f2fd,stroke:#1976d2
    style A3 fill:#e3f2fd,stroke:#1976d2
    style A4 fill:#e3f2fd,stroke:#1976d2
    
    style B1 fill:#fff3e0,stroke:#f57c00
    style B2 fill:#fff3e0,stroke:#f57c00
    
    style L1 fill:#fce4ec,stroke:#c2185b
    style L2 fill:#fce4ec,stroke:#c2185b
    style L3 fill:#fce4ec,stroke:#c2185b
    style L4 fill:#fce4ec,stroke:#c2185b
```

## LLM 调用时序说明

```mermaid
sequenceDiagram
    participant Agent as 智能体<br/>(Planning/Executor/Summary)
    participant LLMClient as LLM客户端<br/>(Java/Python)
    participant LLMService as 大语言模型服务<br/>(OpenAI/Claude/DeepSeek)
    
    Note over Agent,LLMService: 流式调用模式
    
    Agent->>+LLMClient: 发起流式调用<br/>askTool() / ask_llm()
    Note over LLMClient: 构建请求参数<br/>messages, model, stream=true
    
    LLMClient->>+LLMService: HTTP POST<br/>建立SSE流式连接
    LLMService-->>LLMClient: 开始流式响应<br/>逐chunk返回
    
    loop 流式接收
        LLMService-->>LLMClient: SSE chunk<br/>delta content
        LLMClient-->>Agent: 实时转发chunk<br/>流式内容
        Agent-->>Agent: 处理chunk<br/>组装响应
    end
    
    LLMService-->>-LLMClient: SSE: [DONE]<br/>流式结束
    LLMClient-->>-Agent: 返回完整响应<br/>包含tool_calls
    
    Note over Agent: 解析响应<br/>执行工具或总结
```

## SSE消息协议规范

```mermaid
graph LR
    subgraph "SSE消息类型"
        A[plan_thought<br/>规划思考]
        B[plan<br/>任务计划] 
        C[task<br/>当前任务]
        D[tool_thought<br/>工具思考]
        E[tool_result<br/>工具结果]
        F[agent_stream<br/>流式输出]
        G[result<br/>最终结果]
    end
    
    subgraph "消息流向"
        H[后端智能体引擎] --> I[SSE消息格式化]
        I --> J[前端实时展示]
    end
    
    A --> H
    B --> H  
    C --> H
    D --> H
    E --> H
    F --> H
    G --> H
```

## 工具服务流式模式

```mermaid
graph TB
    subgraph "流式响应模式"
        A[General模式<br/>实时推送每个chunk]
        B[Token模式<br/>累积N个token后推送]
        C[Time模式<br/>按时间间隔批量推送]
    end
    
    subgraph "工具类型"
        D[代码解释器<br/>code_interpreter]
        E[深度搜索<br/>deep_search]
        F[报告生成<br/>report]
        G[文件操作<br/>file_tool]
    end
    
    A --> D
    B --> E
    C --> F
    A --> G
    
    D --> H[SSE: ServerSentEvent]
    E --> H
    F --> H  
    G --> H
    
    H --> I[FastAPI StreamingResponse]
    I --> J[text/event-stream]
```

## MCP协议集成架构

```mermaid
graph TB
    subgraph "JoyAgent-JDGenie核心"
        A[后端智能体引擎<br/>Java Spring Boot]
    end
    
    subgraph "MCP客户端层"  
        B[MCP客户端服务<br/>Python FastAPI<br/>Port: 8188]
    end
    
    subgraph "外部MCP生态"
        C[12306查询工具<br/>MCP Server]
        D[文件处理工具<br/>MCP Server] 
        E[数据分析工具<br/>MCP Server]
        F[其他MCP服务<br/>Standard MCP]
    end
    
    A -->|HTTP请求| B
    B -->|MCP over SSE| C
    B -->|MCP over SSE| D
    B -->|MCP over SSE| E  
    B -->|MCP over SSE| F
    
    subgraph "MCP协议栈"
        G[工具发现<br/>list_tools]
        H[工具调用<br/>call_tool]  
        I[服务器状态<br/>ping]
        J[资源管理<br/>resources]
    end
    
    B --> G
    B --> H
    B --> I
    B --> J
```

## 技术栈与端口映射

```mermaid
graph LR
    subgraph "技术栈架构"
        subgraph "前端层"
            A[React 18<br/>TypeScript<br/>Vite<br/>Port: 3000]
        end
        
        subgraph "后端层"  
            B[Java 17<br/>Spring Boot<br/>Maven<br/>Port: 8080]
        end
        
        subgraph "服务层"
            C[Python 3.11<br/>FastAPI<br/>uvicorn<br/>Port: 1601]
            D[Python 3.11<br/>FastAPI<br/>MCP Library<br/>Port: 8188]
        end
        
        subgraph "协议层"
            E[SSE<br/>text/event-stream]
            F[HTTP/HTTPS<br/>RESTful API]
            G[MCP Protocol<br/>Model Context Protocol]
        end
    end
    
    A ---|使用| E
    B ---|实现| E  
    B ---|调用| F
    C ---|提供| E
    D ---|实现| G
    D ---|提供| F
```

## 系统性能与监控

```mermaid
graph TB
    subgraph "性能优化"
        A[异步处理<br/>ThreadPool + CompletableFuture]
        B[流式响应<br/>分块传输编码]
        C[连接复用<br/>HTTP Keep-Alive]
        D[缓存机制<br/>任务结果缓存]
    end
    
    subgraph "监控体系"
        E[健康检查<br/>/health endpoints]
        F[链路追踪<br/>requestId全链路]
        G[性能监控<br/>执行时间统计]
        H[错误处理<br/>异常捕获与恢复]
    end
    
    subgraph "可观测性"
        I[结构化日志<br/>JSON格式统一输出]
        J[实时监控<br/>服务状态面板]
        K[告警机制<br/>异常情况通知]
    end
    
    A --> E
    B --> F
    C --> G  
    D --> H
    
    E --> I
    F --> J
    G --> K
    H --> I
```

## 安全架构

```mermaid
graph TB
    subgraph "安全层级"
        subgraph "传输安全"
            A[HTTPS/WSS加密<br/>TLS 1.2+]
            B[CORS跨域控制<br/>允许的源站配置]
        end
        
        subgraph "认证授权"  
            C[API Key验证<br/>请求头认证]
            D[会话隔离<br/>基于requestId]
            E[权限控制<br/>工具访问权限]
        end
        
        subgraph "数据保护"
            F[敏感词检测<br/>内容过滤机制]
            G[数据隔离<br/>用户数据严格分离]
            H[输入验证<br/>参数校验与清理]
        end
    end
    
    A --> C
    B --> D
    C --> F
    D --> G
    E --> H
```

## 部署架构

```mermaid
graph TB
    subgraph "容器化部署"
        subgraph "前端容器"
            A[nginx:alpine<br/>静态文件服务<br/>反向代理]
        end
        
        subgraph "后端容器"
            B[openjdk:17-jre<br/>Spring Boot应用<br/>智能体引擎]
        end
        
        subgraph "工具服务容器"  
            C[python:3.11-slim<br/>FastAPI应用<br/>工具执行环境]
        end
        
        subgraph "MCP客户端容器"
            D[python:3.11-slim<br/>FastAPI应用<br/>MCP协议客户端]
        end
    end
    
    subgraph "网络配置"
        E[Docker Network<br/>内部服务通信]
        F[端口映射<br/>3000:3000<br/>8080:8080<br/>1601:1601<br/>8188:8188]
    end
    
    A --> E
    B --> E
    C --> E
    D --> E
    E --> F
```

## 大语言模型调用技术细节

### 工具服务 LLM 调用 (Python)

**调用位置**：
- `code_interpreter_agent()` - 代码生成和执行
- `report()` - HTML/PPT 报告生成
- `DeepSearch.run()` - 深度搜索的推理和答案生成

**调用方法**：
```python
async for chunk in ask_llm(
    messages=messages,           # 对话历史
    model=model,                 # 模型名称
    temperature=temperature,     # 温度参数
    top_p=top_p,                # top_p 采样
    stream=True,                # 流式输出
    only_content=True,          # 只返回内容
    extra_headers=headers,      # 额外请求头
):
    yield chunk  # 流式返回
```

**底层实现**：
- 使用 `litellm` 库统一调用不同 LLM 提供商
- 支持 OpenAI、Claude、DeepSeek 等多种模型
- 自动处理敏感词过滤和替换
- 支持自定义请求头和认证信息

### 后端服务 LLM 调用 (Java)

**调用位置**：
- `PlanningAgent.think()` - 任务规划阶段的 LLM 调用
- `ExecutorAgent.think()` - 任务执行阶段的工具选择调用
- `ReactImplAgent.think()` - ReAct 模式的思考调用
- `SummaryAgent.run()` - 最终总结生成调用

**调用方法**：
```java
CompletableFuture<LLM.ToolCallResponse> future = getLlm().askTool(
    context,                           // 上下文信息
    getMemory().getMessages(),         // 历史消息
    Message.systemMessage(systemPrompt, null),  // 系统提示
    availableTools,                    // 可用工具列表
    ToolChoice.AUTO,                   // 工具选择策略
    null,                              // 额外参数
    context.getIsStream(),             // 是否流式
    300                                // 超时时间(秒)
);
```

**流式响应处理**：
- 使用 `CompletableFuture` 异步处理 LLM 响应
- 支持 SSE 流式输出,实时推送思考过程到前端
- 根据 `messageType` 区分不同阶段的输出(plan_thought, tool_thought, agent_stream)

### LLM 流式响应的数据流

```
LLM API (OpenAI/Claude)
    ↓ SSE Stream
Java LLM.askTool() / Python ask_llm()
    ↓ 实时转发
FastAPI EventSourceResponse / Spring Boot SSE
    ↓ SSE Stream
前端 EventSource / fetch
    ↓ 实时渲染
用户界面展示
```

### 不同阶段的 LLM 调用

| 阶段 | 智能体 | LLM 输入 | LLM 输出 | 流式类型 |
|------|--------|----------|----------|---------|
| 任务规划 | PlanningAgent | 用户查询 + 文件信息 | 任务计划 + planning tool_call | plan_thought |
| 工具选择 | ExecutorAgent | 当前任务 + 可用工具 | 思考过程 + tool_calls | tool_thought |
| 代码生成 | code_interpreter | 任务描述 + 文件内容 | Python 代码 | data (general) |
| 报告生成 | report | 任务描述 + 数据文件 | HTML/Markdown 报告 | data (time) |
| 深度搜索 | deepsearch | 查询 + 搜索结果 | 推理过程 + 答案 | data (token) |
| 最终总结 | SummaryAgent | 执行历史 + 结果 | 总结报告 | agent_stream |

### 流式模式说明

**General 模式** (实时推送):
- 每个 chunk 立即推送
- 适用场景: 代码生成、实时对话
- 优点: 最快的用户反馈
- 缺点: 推送频率高,网络开销大

**Token 模式** (批量推送):
- 累积 N 个 token 后推送
- 适用场景: 深度搜索、长文本生成
- 优点: 平衡响应速度和网络开销
- 缺点: 延迟略高

**Time 模式** (定时推送):
- 按时间间隔批量推送
- 适用场景: 报告生成、PPT 生成
- 优点: 稳定的推送节奏
- 缺点: 可能有明显延迟

---

**说明**：
1. 所有架构图使用Mermaid语法绘制，支持在GitHub、GitLab等平台直接渲染
2. 图表详细展示了服务间的通信协议、数据流向和技术实现
3. 每个服务的技术栈、端口配置和主要职责都有清晰标注
4. 流式通信的实现机制和消息格式规范有完整说明
5. **新增**: 完整的大语言模型调用架构和技术细节说明



```mermaid
sequenceDiagram
    participant Java as Java后端
    participant API as FastAPI层
    participant CodeInterpreter as code_interpreter_agent
    participant CIAgent as CIAgent层
    participant LLM as LLM服务(LiteLLMModel)

    Java->>+API: POST /code_interpreter
    API->>+CodeInterpreter: code_interpreter_agent()
    CodeInterpreter->>+CIAgent: create_ci_agent() + agent.run()
    CIAgent->>+LLM: model.generate_stream()
    
    loop 流式处理循环
        LLM-->>CIAgent: ChatMessageStreamDelta
        CIAgent-->>CIAgent: _step_stream() 处理
        CIAgent-->>CIAgent: 代码解析+执行
        CIAgent-->>CodeInterpreter: CodeOutput/ActionOutput
        CodeInterpreter-->>CodeInterpreter: 文件上传处理
        CodeInterpreter-->>API: 增强后的输出对象
        API-->>API: 缓冲控制+格式化
        API-->>Java: ServerSentEvent
    end
    ```
    
    
    
    