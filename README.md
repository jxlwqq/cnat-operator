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


### 创建 API 和控制器

使用 Operator SDK CLI 创建自定义资源定义（CRD）API 和控制器。

运行以下命令创建带有组 cnat、版本 v1alpha1 和种类 At 的 API：

```shell
operator-sdk create api \
--resource=true \
--controller=true \
--group=cnat \
--version=v1alpha1 \
--kind=At
```


定义 Memcached 自定义资源（CR）的 API。

修改 api/v1alpha1/at.go 中的 Go 类型定义，使其具有以下 spec 和 status

```go
const (
	PhasePending = "PENDING"
	PhaseRunning = "RUNNING"
	PhaseDone    = "DONE"
)

type AtSpec struct {
	Schedule string `json:"schedule,omitempty"`
	Command string `json:"command,omitempty"`
}

type AtStatus struct {
	Phase string `json:"phase,omitempty"`
}
```

为资源类型更新生成的代码：
```shell
make generate
```

运行以下命令以生成和更新 CRD 清单：
```shell
make manifests
```