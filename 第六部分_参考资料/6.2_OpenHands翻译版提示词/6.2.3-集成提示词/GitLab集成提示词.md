# GitLab 集成提示词

## 📎 来源文件

**原始文件位置**：
- `openhands/integrations/templates/resolver/gitlab/mr_update_prompt.j2`
- `openhands/integrations/templates/resolver/gitlab/mr_update_conversation_instructions.j2`
- `openhands/integrations/templates/resolver/gitlab/issue_prompt.j2`
- `openhands/integrations/templates/resolver/gitlab/issue_conversation_instructions.j2`

---

## Merge Request更新提示词

### 当前状态
你已检出到分支{{ branch_name }}，该分支有一个打开的MR（Merge Request）#{{ mr_number }}："{{ mr_title }}"。
MR上的评论已经发送给你。

### MR描述
{{ mr_body }}

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

#### 理解MR上下文
使用GitLab API和认证令牌来：
1. 检索与目标分支的差异以了解更改
2. 获取MR正文和链接的issue以获取上下文

#### 处理评论
**如果是问题：**
1. 回答所提出的问题
2. **不要**在MR上留下任何评论

**如果请求代码更新：**
1. 在当前分支中相应地修改代码
2. 使用清晰的提交消息提交更改
3. 推送更改到GitLab以更新MR
4. **不要**在MR上留下任何评论

---

## Issue处理提示词

### 任务描述
你被要求修复GitLab仓库中的issue："{{ issue_title }}"。

### 操作步骤
1. 使用GitLab API检索issue详细信息
2. 创建新分支（使用适当的命名约定）
3. 实施修复
4. 提交并推送更改
5. 创建Merge Request
6. 在MR描述中引用issue

### 最佳实践
- 使用清晰的提交消息
- 确保测试通过
- 遵循项目的贡献指南
- 在MR描述中提供足够的上下文

---

## GitLab API使用指南

### 认证
- 使用项目提供的GitLab访问令牌
- 通常通过环境变量提供

### 常用API操作
- 获取issue详情：`/api/v4/projects/:id/issues/:issue_iid`
- 获取MR详情：`/api/v4/projects/:id/merge_requests/:mr_iid`
- 获取MR变更：`/api/v4/projects/:id/merge_requests/:mr_iid/changes`
- 创建MR：`POST /api/v4/projects/:id/merge_requests`

### 注意事项
- 始终使用API而不是Web界面
- 正确处理API响应和错误
- 遵守速率限制

