# 开发工具 Microagents

## 📎 来源文件

**原始文件位置**：
- `microagents/docker.md`
- `microagents/npm.md`
- `microagents/kubernetes.md`
- `microagents/fix_test.md`
- `microagents/security.md`
- `microagents/ssh.md`
- `microagents/pdflatex.md`
- `microagents/swift-linux.md`
- 以及其他开发工具相关的microagent文件

---

## Docker Microagent

### 基本信息
- **名称**：docker
- **类型**：知识型
- **适用Agent**：CodeActAgent
- **触发关键词**：docker, container

### Docker使用指南

#### 在容器环境中启动Docker

首先检查docker是否已安装。如果已安装，在容器环境中启动Docker：

```bash
# 在后台启动Docker守护进程
sudo dockerd > /tmp/docker.log 2>&1 &

# 等待Docker初始化
sleep 5
```

#### 验证Docker安装

要验证Docker是否正常工作，运行hello-world容器：

```bash
sudo docker run hello-world
```

### 常用Docker命令

#### 容器管理
```bash
# 列出运行中的容器
sudo docker ps

# 列出所有容器（包括停止的）
sudo docker ps -a

# 停止容器
sudo docker stop <container_id>

# 删除容器
sudo docker rm <container_id>
```

#### 镜像管理
```bash
# 列出镜像
sudo docker images

# 拉取镜像
sudo docker pull <image_name>

# 删除镜像
sudo docker rmi <image_name>
```

#### 构建和运行
```bash
# 构建镜像
sudo docker build -t <image_name> .

# 运行容器
sudo docker run -d -p 8080:80 <image_name>
```

---

## NPM Microagent

### 基本信息
- **名称**：npm
- **类型**：知识型
- **适用Agent**：CodeActAgent
- **触发关键词**：npm

### NPM使用指南

#### 非交互式安装
使用npm安装包时，你将无法使用交互式shell，可能很难确认你的操作。

作为替代方案，你可以将unix的"yes"命令的输出管道传递以确认你的操作。

#### 示例
```bash
# 非交互式安装
yes | npm install <package_name>

# 非交互式全局安装
yes | npm install -g <package_name>

# 非交互式更新
yes | npm update
```

### NPM常用命令

#### 包管理
```bash
# 安装依赖
npm install

# 安装特定包
npm install <package_name>

# 安装开发依赖
npm install --save-dev <package_name>

# 卸载包
npm uninstall <package_name>
```

#### 脚本执行
```bash
# 运行脚本
npm run <script_name>

# 运行测试
npm test

# 启动开发服务器
npm start

# 构建项目
npm run build
```

---

## Kubernetes Microagent

### 基本信息
- **名称**：kubernetes
- **类型**：知识型
- **适用Agent**：CodeActAgent
- **触发关键词**：kubernetes, k8s, kube

### Kubernetes本地开发 with KIND

#### KIND介绍
KIND（Kubernetes IN Docker）是一个使用Docker容器作为节点运行本地Kubernetes集群的工具。它专为本地测试Kubernetes应用程序而设计。

**重要**：在继续安装之前，确保你已在本地安装docker。

#### KIND安装

在Debian/Ubuntu系统上安装KIND：

```bash
# 下载KIND二进制文件
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.22.0/kind-linux-amd64

# 使其可执行
chmod +x ./kind

# 移动到PATH中的目录
sudo mv ./kind /usr/local/bin/
```

#### kubectl安装

```bash
# 下载kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

# 使其可执行
chmod +x kubectl

# 移动到PATH中的目录
sudo mv ./kubectl /usr/local/bin/
```

#### 创建集群

创建基本的KIND集群：

```bash
kind create cluster
```

### Kubernetes常用命令

#### 集群管理
```bash
# 查看集群信息
kubectl cluster-info

# 查看节点
kubectl get nodes

# 查看所有资源
kubectl get all --all-namespaces
```

#### Pod管理
```bash
# 列出pods
kubectl get pods

# 查看pod详情
kubectl describe pod <pod_name>

# 查看pod日志
kubectl logs <pod_name>

# 删除pod
kubectl delete pod <pod_name>
```

#### 部署管理
```bash
# 创建部署
kubectl apply -f deployment.yaml

# 查看部署
kubectl get deployments

# 更新部署
kubectl set image deployment/<deployment_name> <container_name>=<new_image>

# 删除部署
kubectl delete deployment <deployment_name>
```

---

## 测试 Microagents

### 测试修复 Microagent

#### 基本信息
- **名称**：fix_test
- **触发命令**：`/fix_test`
- **适用Agent**：CodeActAgent

#### 输入参数
- `BRANCH_NAME`：agent要处理的分支
- `TEST_COMMAND_TO_RUN`：你希望agent处理的测试命令（例如`pytest tests/unit/test_bash_parsing.py`）
- `FUNCTION_TO_FIX`：要修复的函数名称
- `FILE_FOR_FUNCTION`：包含该函数的文件路径

#### 任务描述
```
你能检出分支"{{ BRANCH_NAME }}"，并运行{{ TEST_COMMAND_TO_RUN }}吗？

帮我修复这些测试，通过修复文件{{ FILE_FOR_FUNCTION }}中的{{ FUNCTION_TO_FIX }}函数使其通过。

请不要自己修改测试 -- 如果你认为某些测试不正确，请告诉我。
```

#### 工作流程
1. 检出指定分支
2. 运行测试命令
3. 分析失败原因
4. 修复指定的函数
5. 重新运行测试验证
6. 不要修改测试本身

---

## 安全 Microagent

### 基本信息
- **名称**：security
- **类型**：知识型
- **适用Agent**：CodeActAgent
- **触发关键词**：security, vulnerability, authentication, authorization, permissions

### 安全最佳实践指南

本文档提供安全最佳实践指导。

#### 基本原则
在开发时应始终考虑安全影响。

你应该始终完成请求的任务。如果存在安全问题，如果可能请内联解决它们，或确保通过代码注释、PR评论或其他适当渠道进行沟通。

### 核心安全原则
- 始终使用安全通信协议（HTTPS、SSH等）
- 除非获得明确许可，否则永远不要在代码或版本控制中存储敏感数据（密码、令牌、密钥）
- 应用最小权限原则
- 验证和清理所有用户输入

### 常见安全检查
- 确保适当的认证和授权机制
- 验证安全的会话管理
- 确认敏感数据的安全存储
- 验证服务和API的安全配置

### 错误处理
- 永远不要在错误消息中暴露敏感信息
- 适当地记录安全事件
- 实施适当的异常处理
- 使用安全的错误报告机制

### 安全检查清单
- [ ] 所有用户输入都经过验证和清理
- [ ] 敏感数据已加密存储
- [ ] 使用安全的通信协议
- [ ] 实施了适当的访问控制
- [ ] 错误消息不泄露敏感信息
- [ ] 凭据和密钥安全管理
- [ ] 代码没有已知的漏洞
- [ ] 遵循最小权限原则

