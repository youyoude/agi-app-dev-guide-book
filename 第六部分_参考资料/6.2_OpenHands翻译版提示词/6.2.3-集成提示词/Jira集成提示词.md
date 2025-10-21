# Jira 集成提示词

## 📎 来源文件

**原始文件位置**：
- `openhands/integrations/templates/resolver/jira/jira_new_conversation.j2`
- `openhands/integrations/templates/resolver/jira/jira_existing_conversation.j2`
- `openhands/integrations/templates/resolver/jira/jira_instructions.j2`
- `openhands/integrations/templates/resolver/jira_dc/jira_dc_new_conversation.j2`
- `openhands/integrations/templates/resolver/jira_dc/jira_dc_existing_conversation.j2`
- `openhands/integrations/templates/resolver/jira_dc/jira_dc_instructions.j2`

---

## Jira新对话提示词

### 任务描述
你正在处理Jira issue："{{ issue_title }}"。

### Issue详情
**项目**：{{ project_key }}
**Issue Key**：{{ issue_key }}
**Issue类型**：{{ issue_type }}

### Issue描述
{{ issue_description }}

### 评论历史
{% if comments %}
以下是该issue的评论历史：
{% for comment in comments %}
- @{{ comment.author }} 在{{ comment.created_at }}说：
{{ comment.body }}
{% if not loop.last %}\n\n{% endif %}
{% endfor %}
{% endif %}

### 任务目标
1. 理解issue需求
2. 在相关代码仓库中实施解决方案
3. 确保更改符合issue描述
4. 更新issue状态（如适用）

---

## Jira现有对话提示词

### 上下文
这是一个正在进行的Jira issue讨论："{{ issue_title }}"（{{ issue_key }}）。

### 最新更新
{{ latest_update }}

### 之前的工作
{% if previous_work %}
以下是之前完成的工作：
{{ previous_work }}
{% endif %}

### 待办事项
根据最新评论和讨论，请继续处理该issue。

---

## Jira指令模板

### 基本信息
- 使用Jira API进行所有操作
- 使用提供的认证凭据
- 遵循项目的工作流程状态

### 常用操作
1. **获取Issue详情**
   - 读取issue描述、状态、优先级
   - 查看评论和附件

2. **更新Issue**
   - 添加评论说明进展
   - 更新状态（例如：从"待办"到"进行中"）
   - 添加工作日志

3. **关联代码更改**
   - 在提交消息中引用Jira issue key
   - 在PR/MR描述中链接issue
   - 更新issue以引用相关的PR/MR

### 工作流程示例
1. 将issue状态更新为"进行中"
2. 在代码仓库中实施修复
3. 提交代码（提交消息包含issue key）
4. 创建PR/MR（描述中引用issue）
5. 在Jira中添加评论说明完成的工作
6. 更新issue状态为"待审查"或"完成"

---

## Jira Data Center集成

### 特殊说明
Jira Data Center版本可能有不同的API端点或认证方法。

### 认证差异
- 可能使用不同的OAuth流程
- 可能需要额外的权限配置
- 注意API版本兼容性

### 最佳实践
- 验证API端点正确性
- 测试认证是否成功
- 处理特定于DC版本的错误
- 遵循组织的Jira使用政策

