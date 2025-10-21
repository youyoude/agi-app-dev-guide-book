# 版本控制 Microagents

## 📎 来源文件

**原始文件位置**：
- `microagents/github.md`
- `microagents/gitlab.md`
- `microagents/bitbucket.md`

---

## GitHub Microagent

### 基本信息
- **名称**：github
- **类型**：知识型
- **适用Agent**：CodeActAgent
- **触发关键词**：github, git

### 环境变量
你可以访问环境变量`GITHUB_TOKEN`，它允许你与GitHub API交互。

### 重要规则
1. **始终使用GitHub API**而不是Web浏览器进行操作
2. **始终使用`create_pr`工具**来打开pull request
3. 可以使用`curl`配合`GITHUB_TOKEN`与GitHub API交互

### 认证问题处理
如果推送到GitHub时遇到认证问题（如密码提示或权限错误），旧令牌可能已过期。在这种情况下，更新远程URL以包含当前令牌：
```bash
git remote set-url origin https://${GITHUB_TOKEN}@github.com/username/repo.git
```

### 推送指令（仅当用户要求时）
* **绝不**直接推送到`main`或`master`分支
* Git配置（用户名和邮箱）已预设，不要修改
* 如果已在以`openhands-workspace`开头的分支上，在推送前创建一个更好命名的新分支
* 使用`create_pr`工具创建pull request（如果尚未创建）
* 创建自己的分支或pull request后，继续更新它。除非明确要求，否则**不要**创建新的
* 必要时更新PR标题和描述，但不要更改分支名称
* 除非用户另有要求，否则使用main分支作为基础分支
* 打开或更新pull request后，向用户发送带有PR链接的简短消息
* 除非用户明确说明，否则**不要**将pull request标记为准备审查
* 尽可能少的步骤完成以上所有操作

### 推送示例
```bash
git remote -v && git branch  # 查找当前组织、仓库和分支
git checkout -b create-widget && git add . && git commit -m "Create widget" && git push -u origin create-widget
```

---

## GitLab Microagent

### 基本信息
- **名称**：gitlab
- **类型**：知识型
- **适用Agent**：CodeActAgent
- **触发关键词**：gitlab, git

### 环境变量
你可以访问环境变量`GITLAB_TOKEN`，它允许你与GitLab API交互。

### 重要规则
1. **始终使用GitLab API**而不是Web浏览器进行操作
2. **始终使用`create_mr`工具**来打开merge request
3. 可以使用`curl`配合`GITLAB_TOKEN`与GitLab API交互

### 认证问题处理
如果推送到GitLab时遇到认证问题，旧令牌可能已过期。更新远程URL：
```bash
git remote set-url origin https://oauth2:${GITLAB_TOKEN}@gitlab.com/username/repo.git
```

### 推送指令（仅当用户要求时）
* **绝不**直接推送到`main`或`master`分支
* Git配置（用户名和邮箱）已预设，不要修改
* 如果已在以`openhands-workspace`开头的分支上，在推送前创建一个更好命名的新分支
* 使用`create_mr`工具创建merge request（如果尚未创建）
* 创建自己的分支或merge request后，继续更新它
* 必要时更新MR标题和描述，但不要更改分支名称
* 除非用户另有要求，否则使用main分支作为基础分支
* 打开或更新merge request后，向用户发送带有MR链接的简短消息

---

## Bitbucket Microagent

### 基本信息
- **名称**：bitbucket
- **类型**：知识型
- **适用Agent**：CodeActAgent
- **触发关键词**：bitbucket

### 环境变量
你可以访问环境变量`BITBUCKET_TOKEN`，它允许你与Bitbucket API交互。

### 重要规则
1. **始终使用Bitbucket API**而不是Web浏览器进行操作
2. **始终使用`create_bitbucket_pr`工具**来打开pull request
3. 可以使用`curl`配合`BITBUCKET_TOKEN`与Bitbucket API交互

### 认证问题处理
如果推送到Bitbucket时遇到认证问题，更新远程URL：
```bash
git remote set-url origin https://x-token-auth:${BITBUCKET_TOKEN}@bitbucket.org/username/repo.git
```

### 推送指令
与GitHub类似，但使用Bitbucket特定的工具和API。

---

## 通用版本控制最佳实践

### 分支管理
1. 永远不要直接推送到主分支
2. 创建有意义的分支名称
3. 一个任务/问题使用一个分支

### 提交管理
1. 使用清晰的提交消息
2. 提交应该是原子性的和有意义的
3. 在提交消息中引用issue/ticket编号

### PR/MR管理
1. 提供详细的描述
2. 链接相关的issue
3. 保持PR/MR更新
4. 不要创建重复的PR/MR

### API使用
1. 优先使用API而不是Web界面
2. 正确处理认证令牌
3. 处理API错误和速率限制

