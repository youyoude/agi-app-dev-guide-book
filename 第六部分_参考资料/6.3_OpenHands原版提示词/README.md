# OpenHands 提示词分类索引

本文件夹包含 OpenHands 项目中所有提示词的分类整理，原汁原味保留原始内容。

## 📁 文件夹结构说明

### 1-Agents（智能体提示词）
包含各类 AI 智能体的核心提示词模板：

#### CodeAct-Agent
- `system_prompt.j2` - 主系统提示词，定义角色、效率、文件系统指南、代码质量、版本控制等
- `system_prompt_interactive.j2` - 交互模式系统提示词，增加交互规则
- `system_prompt_long_horizon.j2` - 长期任务管理系统提示词，包含任务追踪
- `system_prompt_tech_philosophy.j2` - 技术哲学系统提示词，采用 Linus Torvalds 工程思维
- `user_prompt.j2` - 用户提示词模板（空文件）
- `additional_info.j2` - 附加信息模板，包含仓库信息、运行时信息等
- `microagent_info.j2` - 微代理信息模板
- `security_risk_assessment.j2` - 安全风险评估策略
- `in_context_learning_example.j2` - 上下文学习示例
- `in_context_learning_example_suffix.j2` - 上下文学习示例后缀

#### ReadOnly-Agent
- `system_prompt.j2` - 只读智能体系统提示词，定义只读能力和限制
- `user_prompt.j2` - 用户提示词模板（空文件）
- `additional_info.j2` - 附加信息模板
- `microagent_info.j2` - 微代理信息模板
- `in_context_learning_example.j2` - 只读模式的上下文学习示例
- `in_context_learning_example_suffix.j2` - 上下文学习示例后缀

---

### 2-Integrations（集成服务提示词）
包含与各种外部平台集成的提示词模板：

#### GitHub
- `issue_prompt.j2` - GitHub Issue 处理提示词
- `issue_conversation_instructions.j2` - Issue 对话指令
- `pr_update_prompt.j2` - Pull Request 更新提示词
- `pr_update_conversation_instructions.j2` - PR 更新对话指令

#### GitLab
- `issue_prompt.j2` - GitLab Issue 处理提示词
- `issue_conversation_instructions.j2` - Issue 对话指令
- `mr_update_prompt.j2` - Merge Request 更新提示词
- `mr_update_conversation_instructions.j2` - MR 更新对话指令

#### Bitbucket
- `issue_prompt.j2` - Bitbucket Issue 处理提示词
- `issue_conversation_instructions.j2` - Issue 对话指令
- `pr_update_prompt.j2` - Pull Request 更新提示词
- `pr_update_conversation_instructions.j2` - PR 更新对话指令

#### Jira
- `jira_instructions.j2` - Jira 集成指令
- `jira_new_conversation.j2` - Jira 新对话模板
- `jira_existing_conversation.j2` - Jira 已有对话模板

#### Jira-DC
- `jira_dc_instructions.j2` - Jira Data Center 集成指令
- `jira_dc_new_conversation.j2` - Jira DC 新对话模板
- `jira_dc_existing_conversation.j2` - Jira DC 已有对话模板

#### Linear
- `linear_instructions.j2` - Linear 集成指令
- `linear_new_conversation.j2` - Linear 新对话模板
- `linear_existing_conversation.j2` - Linear 已有对话模板

#### Slack
- `user_message_conversation_instructions.j2` - Slack 用户消息对话指令

#### Suggested-Tasks
- `failing_checks_prompt.j2` - 失败检查任务提示词
- `merge_conflict_prompt.j2` - 合并冲突任务提示词
- `open_issue_prompt.j2` - 未解决 Issue 任务提示词
- `unresolved_comments_prompt.j2` - 未解决评论任务提示词

#### 其他
- `summary_prompt.j2` - 总结提示词模板

---

### 3-Resolver（问题解决器提示词）
包含问题解决和成功判断相关的提示词：

#### Resolve
- `basic.jinja` - 基础解决提示词
- `basic-conversation-instructions.jinja` - 基础对话指令
- `basic-with-tests.jinja` - 带测试的基础解决提示词
- `basic-with-tests-conversation-instructions.jinja` - 带测试的对话指令
- `basic-followup.jinja` - 基础后续跟进提示词
- `basic-followup-conversation-instructions.jinja` - 后续跟进对话指令
- `pr-changes-summary.jinja` - PR 变更总结模板

#### Guess-Success
- `issue-success-check.jinja` - Issue 成功检查模板
- `pr-feedback-check.jinja` - PR 反馈检查模板
- `pr-review-check.jinja` - PR 审查检查模板
- `pr-thread-check.jinja` - PR 讨论线程检查模板

---

### 4-Benchmarks（评估基准提示词）
包含用于性能评估和测试的提示词：

#### SWE-Bench
- `swe_default.j2` - SWE-Bench 默认提示词
- `swe_gpt4.j2` - SWE-Bench GPT-4 提示词
- `swt.j2` - SWT 提示词

#### NoCode-Bench
- `nc.j2` - NoCode-Bench 提示词

---

### 5-Microagents（微代理提示词）
包含各种专门用途的微代理指令文档：

- `README.md` - 微代理说明文档
- `add_agent.md` - 添加代理指令
- `add_repo_inst.md` - 添加仓库指令
- `address_pr_comments.md` - 处理 PR 评论指令
- `agent_memory.md` - 代理记忆指令
- `bitbucket.md` - Bitbucket 集成指令
- `code-review.md` - 代码审查指令
- `codereview-roasted.md` - 严格代码审查指令
- `default-tools.md` - 默认工具指令
- `docker.md` - Docker 相关指令
- `fix_test.md` - 修复测试指令
- `fix-py-line-too-long.md` - 修复 Python 行过长指令
- `flarglebargle.md` - 示例微代理
- `github.md` - GitHub 集成指令
- `gitlab.md` - GitLab 集成指令
- `kubernetes.md` - Kubernetes 相关指令
- `npm.md` - NPM 相关指令
- `onboarding.md` - 新手入门指令
- `pdflatex.md` - PDF LaTeX 相关指令
- `security.md` - 安全相关指令
- `ssh.md` - SSH 相关指令
- `swift-linux.md` - Swift Linux 相关指令
- `update_pr_description.md` - 更新 PR 描述指令
- `update_test.md` - 更新测试指令

---

### 6-Enterprise（企业版提示词）
包含企业版专用的提示词模板：

- `summary_system_message.j2` - 总结系统消息模板
- `summary_user_message.j2` - 总结用户消息模板

---

### 7-Others（其他提示词）
包含其他辅助功能的提示词：

- `generate_remember_prompt.j2` - 生成记忆提示词模板
- `Dockerfile.j2` - Docker 文件模板（如存在）

---

## 📊 统计信息

- **总计**: 62 个 .j2 模板文件 + 11 个 .jinja 模板文件 + 24 个 .md 微代理文档
- **Agents**: 16 个文件
- **Integrations**: 23 个文件
- **Resolver**: 11 个文件
- **Benchmarks**: 4 个文件
- **Microagents**: 24 个文件
- **Enterprise**: 2 个文件
- **Others**: 2 个文件

---

## 🔍 使用说明

### 文件格式说明
- `.j2` 文件：Jinja2 模板文件，使用 Jinja2 模板引擎渲染
- `.jinja` 文件：Jinja 模板文件，同样使用 Jinja2 模板引擎
- `.md` 文件：Markdown 格式的指令文档，直接作为提示词内容使用

### 模板变量
模板中常见的变量包括：
- `repository_info` - 仓库信息
- `runtime_info` - 运行时信息
- `triggered_agents` - 触发的代理列表
- `conversation_instructions` - 对话指令
- `repository_instructions` - 仓库指令
- `cli_mode` - CLI 模式标识

---

## 📝 注意事项

1. 所有提示词均为原版内容，未经翻译或修改
2. 提示词模板使用 Jinja2 语法，包含条件判断、循环、变量替换等功能
3. 部分模板通过 `{% include %}` 引用其他模板，形成模块化结构
4. 微代理文档可以通过关键词触发，动态注入到系统提示词中
5. 不同的 Agent 有不同的系统提示词变体，适用于不同场景

---

## 🔗 相关资源

- OpenHands 项目主页: https://github.com/All-Hands-AI/OpenHands
- Jinja2 模板引擎文档: https://jinja.palletsprojects.com/

---

*整理日期: 2025-10-21*
*版本: 基于项目主分支*

