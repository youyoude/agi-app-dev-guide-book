# OpenHands 提示词库

## 📚 概述

这是OpenHands项目的完整提示词库，包含了系统中所有AI Agent和集成组件使用的提示词。这些提示词是OpenHands智能代码助手的核心，定义了AI如何理解任务、与用户交互以及执行各种操作。

## 🗂️ 目录结构

```
提示词/
├── 01-Agent核心提示词/          # AI Agent的核心行为定义
├── 02-Resolver提示词/            # 问题解决和成功判断
├── 03-集成提示词/               # 与外部平台的集成
├── 04-Microagents提示词/        # 专门的微型代理
├── 05-辅助提示词/               # 辅助功能提示词
└── README.md                    # 本文件
```

## 📖 详细目录

### 01-Agent核心提示词

这是OpenHands最核心的提示词集合，定义了AI Agent的基本行为和工作哲学。

| 文件名 | 描述 | 适用场景 |
|--------|------|----------|
| [CodeAct-Agent系统提示词.md](./01-Agent核心提示词/CodeAct-Agent系统提示词.md) | CodeAct Agent的完整系统提示词，包含角色定义、核心原则、问题解决工作流等 | 所有需要修改代码、执行命令的场景 |
| [CodeAct-Agent交互规则.md](./01-Agent核心提示词/CodeAct-Agent交互规则.md) | 处理高层次指令和用户交互的规则 | 当用户指令模糊或需要澄清时 |
| [CodeAct-Agent任务管理.md](./01-Agent核心提示词/CodeAct-Agent任务管理.md) | 使用task_tracker工具管理复杂任务的指南 | 多步骤、复杂的开发任务 |
| [CodeAct-Agent技术哲学.md](./01-Agent核心提示词/CodeAct-Agent技术哲学.md) | 采用Linus Torvalds风格的技术哲学和决策框架 | 需要深入技术判断和架构决策时 |
| [CodeAct-Agent示例对话.md](./01-Agent核心提示词/CodeAct-Agent示例对话.md) | 完整的示例对话，展示Agent如何工作 | 学习Agent交互模式 |
| [ReadOnly-Agent系统提示词.md](./01-Agent核心提示词/ReadOnly-Agent系统提示词.md) | ReadOnly Agent的系统提示词，专注于代码分析和探索 | 只读分析、代码审查、问题回答 |

**核心特点：**
- 🎯 以效率为先：最小化操作成本
- 📁 严格的文件管理：禁止创建多版本文件
- 🔍 探索优先：先理解再实施
- ✅ 质量保证：完整的测试和验证流程
- 🔒 安全意识：内置安全风险评估

### 02-Resolver提示词

用于解决GitHub/GitLab等平台上的问题，并判断解决方案是否成功。

| 文件名 | 描述 | 适用场景 |
|--------|------|----------|
| [基础问题解决提示词.md](./02-Resolver提示词/基础问题解决提示词.md) | 基础问题解决流程，包括基础版、带测试版和后续修复 | 解决仓库中的bug和feature请求 |
| [成功判断提示词.md](./02-Resolver提示词/成功判断提示词.md) | 判断问题是否成功解决和PR反馈是否被正确处理 | 自动化判断解决方案的有效性 |

**核心特点：**
- 🤖 自主工作：不依赖人类帮助
- ✅ 测试驱动：为代码更改添加测试
- 📋 完整检查：确保所有需求都被满足
- 🎯 自主判断：基于证据独立判断成功与否

### 03-集成提示词

与各种外部平台和服务的集成提示词。

| 文件名 | 描述 | 涵盖平台 |
|--------|------|----------|
| [GitHub集成提示词.md](./03-集成提示词/GitHub集成提示词.md) | GitHub Issue和PR处理的完整工作流 | GitHub Issues, Pull Requests |
| [GitLab集成提示词.md](./03-集成提示词/GitLab集成提示词.md) | GitLab Merge Request和Issue处理 | GitLab MRs, Issues |
| [Jira集成提示词.md](./03-集成提示词/Jira集成提示词.md) | Jira和Jira Data Center的任务处理 | Jira, Jira DC |
| [其他集成提示词.md](./03-集成提示词/其他集成提示词.md) | Linear、Bitbucket、Slack等其他集成 | Linear, Bitbucket, Slack, 建议任务 |
| [总结提示词.md](./03-集成提示词/总结提示词.md) | 工作完成后的总结消息生成 | 所有任务完成场景 |

**核心特点：**
- 🔗 API优先：使用API而不是Web界面
- 📝 完整流程：从Issue到PR的完整工作流
- 🔄 持续更新：更新现有PR而不是创建新的
- 📊 清晰总结：结构化的工作总结

### 04-Microagents提示词

专门的微型代理，为特定场景提供专业知识和工具。

| 文件名 | 描述 | 包含的Microagents |
|--------|------|------------------|
| [版本控制Microagents.md](./04-Microagents提示词/版本控制Microagents.md) | GitHub、GitLab、Bitbucket的版本控制知识 | github, gitlab, bitbucket |
| [代码审查Microagents.md](./04-Microagents提示词/代码审查Microagents.md) | 代码审查和PR评论处理 | code-review, address_pr_comments, update_pr_description |
| [开发工具Microagents.md](./04-Microagents提示词/开发工具Microagents.md) | Docker、NPM、Kubernetes、测试、安全等 | docker, npm, kubernetes, fix_test, security |

**核心特点：**
- 🎯 专业化：每个microagent专注于特定领域
- 🔤 关键词触发：通过关键词自动激活
- 📚 知识注入：为Agent提供专业领域知识
- 🛠️ 工具指导：如何使用特定工具和平台

### 05-辅助提示词

支持核心功能的辅助提示词。

| 文件名 | 描述 | 用途 |
|--------|------|------|
| [安全风险评估提示词.md](./05-辅助提示词/安全风险评估提示词.md) | 操作安全风险级别的评估标准 | 评估LOW/MEDIUM/HIGH风险 |
| [记忆生成提示词.md](./05-辅助提示词/记忆生成提示词.md) | 生成更新参考文件的提示词 | Agent记忆和学习系统 |
| [可解决性评估提示词.md](./05-辅助提示词/可解决性评估提示词.md) | 评估问题可解决性（Enterprise功能） | Issue优先级和难度评估 |

**核心特点：**
- 🔒 安全第一：明确的风险评估标准
- 🧠 持续学习：从经验中学习和改进
- 📊 智能评估：预测问题解决难度

## 🎯 使用指南

### 如何选择合适的提示词？

#### 场景1：我需要修改代码
→ 使用 **CodeAct-Agent系统提示词**

#### 场景2：我只需要分析代码，不修改
→ 使用 **ReadOnly-Agent系统提示词**

#### 场景3：我需要解决GitHub Issue
→ 使用 **Resolver提示词** + **GitHub集成提示词**

#### 场景4：我需要进行代码审查
→ 使用 **代码审查Microagents**

#### 场景5：我需要Docker相关帮助
→ 触发 **docker** 关键词，激活Docker Microagent

### 提示词的组合使用

OpenHands的提示词系统是模块化的，多个提示词可以组合使用：

```
基础：CodeAct-Agent系统提示词
  ↓
场景：GitHub Issue处理
  ↓
增强：github Microagent（自动触发）
  ↓
辅助：安全风险评估（在需要时）
```

## 🔑 关键概念

### 1. Agent类型

**CodeAct Agent**
- 可以修改文件
- 可以执行命令
- 可以安装依赖
- 可以提交代码
- 适用于大多数开发任务

**ReadOnly Agent**
- 只能读取和分析
- 不能修改文件
- 不能执行命令
- 适用于代码审查和分析

### 2. Microagents

Microagents是通过关键词自动激活的专业知识模块：

- **触发机制**：当对话中出现特定关键词时自动激活
- **知识注入**：为Agent提供特定领域的专业知识
- **无缝集成**：自动融入对话上下文

### 3. 安全风险级别

所有操作都有明确的风险评级：

- **LOW**：只读操作，无风险
- **MEDIUM**：容器内修改，风险可控
- **HIGH**：涉及敏感数据或系统级操作

### 4. 工作流程

典型的OpenHands工作流：

1. **探索**：理解代码库和问题
2. **规划**：使用task_tracker分解任务
3. **实施**：执行具体的代码更改
4. **测试**：验证解决方案
5. **提交**：提交代码和创建PR
6. **总结**：生成工作总结

## 📋 提示词设计原则

OpenHands的提示词遵循以下设计原则：

### 1. 清晰性
- 使用明确、具体的语言
- 避免歧义和模糊表达
- 提供具体的例子

### 2. 结构化
- 使用XML标签组织内容
- 分层次的信息组织
- 清晰的章节划分

### 3. 可操作性
- 提供具体的操作指令
- 包含示例和模板
- 明确的成功标准

### 4. 安全性
- 内置安全检查
- 明确的权限边界
- 风险评估机制

### 5. 可扩展性
- 模块化设计
- 易于组合和定制
- 支持新功能添加

## 🛠️ 提示词的技术细节

### Jinja2模板

OpenHands使用Jinja2模板引擎，支持：

- 变量插值：`{{ variable }}`
- 条件判断：`{% if condition %}`
- 循环：`{% for item in items %}`
- 模板包含：`{% include "template.j2" %}`

### XML结构标签

提示词中使用XML标签组织内容：

- `<ROLE>`：角色定义
- `<EFFICIENCY>`：效率准则
- `<SECURITY>`：安全规则
- `<TASK_MANAGEMENT>`：任务管理
- 等等...

### 上下文注入

动态上下文信息会被注入到提示词中：

- 仓库信息
- 运行时信息
- 用户自定义指令
- Microagent知识

## 📊 提示词统计

- **总分类**：5个主要类别
- **Agent类型**：2种（CodeAct、ReadOnly）
- **集成平台**：7个（GitHub、GitLab、Jira等）
- **Microagents**：15+个专业领域
- **核心文件**：20+个提示词文件

## 🔄 更新历史

本提示词库是从OpenHands项目的以下位置提取和整理的：

- `openhands/agenthub/*/prompts/` - Agent核心提示词
- `openhands/resolver/prompts/` - Resolver提示词
- `openhands/integrations/templates/` - 集成提示词
- `microagents/*.md` - Microagents定义
- `enterprise/integrations/solvability/prompts/` - Enterprise功能

## 🤝 贡献指南

如果你想改进这些提示词：

1. **理解现有设计**：先阅读相关提示词，理解其设计意图
2. **保持一致性**：遵循现有的格式和风格
3. **测试更改**：确保更改不会破坏现有功能
4. **更新文档**：同步更新本README

## 📞 相关资源

- **项目主页**：[OpenHands GitHub](https://github.com/All-Hands-AI/OpenHands)
- **文档**：查看项目docs目录
- **社区**：参与项目讨论和贡献

## 📄 许可

这些提示词是OpenHands项目的一部分，遵循项目的开源许可证。

---

**最后更新**：2025年10月21日
**维护者**：OpenHands团队

