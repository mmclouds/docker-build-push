 # Docker 镜像构建和推送 Action

这个 GitHub Action 工作流可以帮助你自动构建 Docker 镜像并推送到 DockerHub 或阿里云容器镜像服务。支持公有/私有仓库，可以自动创建和管理 DockerHub 私有仓库。

## 功能特点

- 支持多种仓库地址格式（HTTPS/SSH）
- 支持 DockerHub 和阿里云容器镜像服务
- 自动创建和管理 DockerHub 私有仓库
- 支持公有/私有仓库设置
- 自动添加时间戳标签，方便版本管理

## 使用前准备

### 1. 配置 Secrets

在你的 GitHub 仓库中，进入 Settings > Secrets and variables > Actions，添加以下 secrets：

对于 DockerHub：
- `DOCKERHUB_USERNAME`: DockerHub 用户名
- `DOCKERHUB_TOKEN`: DockerHub 访问令牌（从 DockerHub 网站获取）
- `GH_TOKEN`: GitHub 个人访问令牌（用于拉取代码）

对于阿里云：
- `ALIYUN_USERNAME`: 阿里云容器镜像服务登录用户名
- `ALIYUN_PASSWORD`: 阿里云容器镜像服务登录密码

### 2. 准备 Dockerfile

确保你的仓库中包含有效的 Dockerfile 文件。

## 使用方法

### 1. 手动触发工作流

1. 进入你的 GitHub 仓库
2. 点击 "Actions" 标签
3. 选择 "Docker Build and Push" 工作流
4. 点击 "Run workflow"

### 2. 填写参数

必填参数：
- `GitHub 仓库地址`: 支持以下格式
  - `owner/repo`
  - `https://github.com/owner/repo.git`
  - `git@github.com:owner/repo.git`
- `分支名称`: 要构建的分支（默认：main）
- `Dockerfile 路径`: Dockerfile 在仓库中的路径（默认：./Dockerfile）
- `镜像仓库类型`: 选择 dockerhub 或 aliyun
- `是否设置为私有`: 是否将镜像推送到私有仓库

当选择阿里云时的额外参数：
- `阿里云镜像仓库地域`: 例如 cn-hangzhou
- `阿里云镜像仓库命名空间`: 你的阿里云镜像仓库命名空间

### 3. 镜像标签说明

工作流会自动生成两个标签：
- `latest`: 始终指向最新版本
- `时间戳`: 格式为 `YYYYMMDD_HHMMSS`，用于版本追踪

### 4. 示例

#### DockerHub 公有仓库：
```
docker.io/用户名/仓库名:latest
docker.io/用户名/仓库名:20240104_153000
```

#### DockerHub 私有仓库：
```
docker.io/用户名/仓库名:latest
docker.io/用户名/仓库名:20240104_153000
```

#### 阿里云公有仓库：
```
registry-vpc.{地域}.aliyuncs.com/{命名空间}/仓库名:latest
registry-vpc.{地域}.aliyuncs.com/{命名空间}/仓库名:20240104_153000
```

#### 阿里云私有仓库：
```
registry.{地域}.aliyuncs.com/{命名空间}/仓库名:latest
registry.{地域}.aliyuncs.com/{命名空间}/仓库名:20240104_153000
```

## 注意事项

1. DockerHub Token 需要有创建和管理仓库的权限
2. 阿里云用户需要提前创建好命名空间
3. 确保 Dockerfile 路径正确
4. GitHub Token 需要有代码读取权限
5. 私有仓库功能需要相应的账号权限支持