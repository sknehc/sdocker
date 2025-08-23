# sdocker

## 项目介绍

`sdocker` 是一款专为解决 DockerHub 镜像拉取慢、访问不稳定问题设计的工具。它通过 **GitHub Action + GitHub Release** 构建中间缓存仓库，借助 GitHub 文件加速下载服务，将 Docker 镜像快速加载到本地服务器。工具可无缝替换原生 `docker pull` 命令，支持同时拉取多个镜像，大幅提升镜像获取效率。
本项目依赖GitHub Release，其使用空间受限（2GB），不建议通过此脚本拉去超过2GB的镜像，建议镜像下载到本地后删除release上的文件（配置文件中ISDEL=Y，默认删除）。

## 特点

1.  **加速拉取**：依托 GitHub 全球 CDN 加速服务，规避 DockerHub 网络瓶颈，下载速度提升显著；

2.  **无缝替换**：支持脚本直接运行或安装为系统命令使用。安装为系统命令后，使用方式（`sdocker pull`）与 `docker pull` 高度一致，无需修改现有脚本或操作习惯；

3.  **多镜像支持**：支持一次性传入多个镜像地址，批量完成拉取操作；

4.  **缓存机制**：通过 GitHub Release 持久化缓存镜像，后续拉取无需重复构建，进一步节省时间；

5.  **安全可控**：基于 GitHub 账户令牌授权，可限制令牌仅对目标仓库生效，降低权限泄露风险。

## 前置依赖
*   本地服务器需安装 `bash`（版本 4.0+）、`docker`（版本 20.10+）、`git`；
*   拥有 GitHub 账户（用于 Fork 仓库、生成访问令牌）。

## 使用步骤
### 1. 点击右上角 **Fork** 按钮，将仓库复制到你的 GitHub 账户下。

### 2. 克隆仓库到本地服务器

```
git clone https://github.com/your-username/sdocker.git  # 替换为你的仓库地址
```

### 3. 进入项目目录

```
cd sdocker
```

### 4. 赋予脚本执行权限

```
chmod +x sdocker.sh
```

### 5. 查看使用帮助

执行以下命令，了解脚本的完整参数和使用示例：

```
./sdocker.sh -h
```

## 关键配置：GitHub 令牌获取与配置

脚本运行需通过 GitHub 令牌访问你的仓库，**请严格限制令牌权限，仅授予本仓库的访问权限**，操作步骤如下：

1.  登录 GitHub，进入 **Settings → Developer settings → Personal access tokens (classic)**；

2.  点击 **Generate new token**，填写以下配置：

*   **Note**：填写令牌备注（如 `sdocker-repo-access`）；

*   **Expiration**：建议设置有效期（如 30 天），降低泄露风险；

*   **Scopes**：仅勾选 `repo` 下的 `repo:status`、`repo_deployment`、`public_repo`（确保仅能访问你的公开 / 私有仓库，无额外权限）；

1.  点击 **Generate token**，复制生成的令牌（**令牌仅显示一次，请立即保存**）；

## 使用示例

### 1. 拉取单个镜像

```
./sdocker.sh pull nginx:latest  # 等效于 docker pull nginx:latest
```

### 2. 同时拉取多个镜像

```
./sdocker.sh pull ubuntu:22.04 alpine:3.18 redis:7.0  # 批量拉取多个镜像
```

## 注意事项

1.  **令牌安全**：

*   严禁将 GitHub 令牌泄露给他人，或授予超出本仓库访问范围的权限；

*   建议定期轮换令牌（如每月更新一次），降低长期使用风险。

1.  **使用范围**：

*   本脚本仅用于 **学习和技术研究**，请勿用于商业用途或违反 DockerHub、GitHub 服务条款的场景。

1.  **责任声明**：

*   因使用本工具导致的任何问题（如数据丢失、服务异常、账号风险等），均由使用者自行承担，项目作者不承担任何责任。

1.  **镜像一致性**：

*   脚本缓存的镜像版本与 DockerHub 源镜像保持一致，但建议拉取后通过 `docker inspect` 验证镜像完整性。

## 问题反馈

如遇问题，请提交 Issues，便于公开跟踪问题；