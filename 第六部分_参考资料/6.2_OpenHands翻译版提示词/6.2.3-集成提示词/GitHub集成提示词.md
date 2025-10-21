# GitHub 集成提示词

## 📎 来源文件

**原始文件位置**：
- `openhands/integrations/templates/resolver/github/issue_prompt.j2`
- `openhands/integrations/templates/resolver/github/issue_conversation_instructions.j2`
- `openhands/integrations/templates/resolver/github/pr_update_prompt.j2`
- `openhands/integrations/templates/resolver/github/pr_update_conversation_instructions.j2`

**相关文件**：
- `openhands/integrations/templates/suggested_task/open_issue_prompt.j2`

---

## Issue处理提示词

### 任务描述
{% if issue_number %}
你被要求修复仓库中的issue #{{ issue_number }}："{{ issue_title }}"。
issue上的评论已经发送给你。
{% else %}
你的任务是修复问题："{{ issue_title }}"。
{% endif %}

### Issue正文
{{ issue_body }}

### 之前的评论
{% if previous_comments %}
作为参考，以下是issue上的之前评论：

{% for comment in previous_comments %}
- @{{ comment.author }} 说：
{{ comment.body }}
{% if not loop.last %}\n\n{% endif %}
{% endfor %}
{% endif %}

### 指南

1. **仔细审查任务**
2. **对于所有实际应用代码的更改**（例如Python或Javascript），向测试目录添加适当的测试，以确保问题已得到修复
3. **运行测试**，如果通过了，你就完成了！
4. **不需要编写新测试**的情况：
   - 仅更改文档或配置文件
   - 其他非功能性更改

### 最终检查清单
重新阅读issue标题、正文和评论，确保你已成功实现所有要求。

### GitHub操作步骤
使用$GITHUB_TOKEN和GitHub API来：

1. **创建新分支**，使用`openhands/`作为前缀（例如`openhands/update-readme`）
2. **提交更改**，使用清晰的提交消息
3. **推送分支到GitHub**
4. **使用`create_pr`工具**打开新的PR
5. **PR描述应该**：
   - 遵循仓库的PR模板（如果存在，检查`.github/pull_request_template.md`）
   - 提及它"修复"或"关闭"issue编号
   - 包括模板中的所有必需部分

---

## Issue对话指令（简洁版）

{% if issue_comment %}
{{ issue_comment }}
{% else %}
请修复issue编号#{{ issue_number }}。
{% endif %}

---

## PR更新提示词

### 当前状态
你已检出到分支{{ branch_name }}，该分支有一个打开的PR #{{ pr_number }}："{{ pr_title }}"。
PR上的评论已经发送给你。

### PR描述
{{ pr_body }}

### 之前的评论
{% if comments %}
你可能会发现这些其他评论相关：
{% for comment in comments %}
- @{{ comment.author }} 在{{ comment.created_at }}说：
{{ comment.body }}
{% if not loop.last %}\n\n{% endif %}
{% endfor %}
{% endif %}

### 评论位置
{% if file_location %}
评论在文件`{{ file_location }}`的第#{{ line_number }}行
{% endif %}

### 处理评论的步骤

#### 理解PR上下文
使用$GITHUB_TOKEN和GitHub API来：
1. 检索与main的差异以了解更改
2. 获取PR正文和链接的issue以获取上下文

#### 处理评论
**如果是问题：**
1. 回答所提出的问题
2. **不要**在PR上留下任何评论

**如果请求代码更新：**
1. 在当前分支中相应地修改代码
2. 使用清晰的提交消息提交更改
3. 推送更改到GitHub以更新PR
4. **不要**在PR上留下任何评论

---

## 通用Issue提示词（适用于多平台）

### 任务
你正在处理仓库{{ repo }}中的Issue #{{ issue_number }}。你的目标是修复该issue。

### 操作步骤
使用{{ apiName }}和{{ tokenEnvVar }}环境变量来：
1. 检索issue详细信息和issue上的任何评论
2. 检出新分支并调查需要进行哪些更改
3. 进行所需的更改
4. 打开{{ requestVerb }}

**注意**：确保在{{ requestTypeShort }}描述中引用issue。

