# cnat-operator

### 前置条件

* 安装 Docker Desktop，并启动内置的 Kubernetes 集群
* 注册一个 [hub.docker.com](https://hub.docker.com/) 账户，需要将本地构建好的镜像推送至公开仓库中
* 安装 operator SDK CLI: `brew install operator-sdk`
* 安装 Go: `brew install go`

本示例推荐的依赖版本：

* Docker Desktop: >= 4.0.0
* Kubernetes: >= 1.21.4
* Operator-SDK: >= 1.11.0
* Go: >= 1.17

### 创建项目

使用 Operator SDK CLI 创建名为 cnat-operator 的项目。

```shell
mkdir -p $HOME/projects/cnat-operator
cd $HOME/projects/cnat-operator
go env -w GOPROXY=https://goproxy.cn,direct

operator-sdk init \
--domain=programming-kubernetes.info \
--repo=github.com/jxlwqq/cnat-operator \
--skip-go-version-check
```