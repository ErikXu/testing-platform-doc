# 安装

## 系统要求

- 1 核 1G

- Linux 或 Windows WSL

- Docker

## 安装 MongoDB

``` bash
mkdir -p /opt/mongo
docker run --name mongo -v /opt/mongo:/etc/mongo -p 27017:27017 -d mongo:4.2
```

如果你有自己的 MongoDB 可以直接使用。

## 镜像安装

首次打包镜像可能会花点时间，可以关注公众号 `跬步之巅`，并发送 `测试平台` 获取镜像仓库地址。

- 运行平台

``` bash
docker run --name testing-platform -d \
-e MONGO__CONNECTIONSTRING={MongoDB 连接串，如 mongodb://192.168.1.1:27017} \
-p 80:80 -p 8080:8080 xxx/testing-platform:latest
```

- 在被观察服务器运行 Agent

``` bash
docker run --name testing-agent -d --pid=host \
-e SERVERDOMAIN="{平台地址，如 http://192.168.1.1:5000}" \
-e CLIENTADDRESS="{被测服务器可访问地址，如 192.168.1.2}" \
-e AGENTPORT=5001 \
-e MONITORPORT=5002 \
-p 5001:5001 -p 5002:5002 \
xxx/testing-agent:latest
```

## 平台源码安装

- 克隆代码

``` bash
git clone https://github.com/ErikXu/testing-platform.git
```

- 编译程序

``` bash
cd testing-platform/src
bash build.sh
```

- 打包镜像

``` bash
bash pack.sh
```

- 运行程序

``` bash
docker run --name testing-platform -d \
-e MONGO__CONNECTIONSTRING={MongoDB 连接串，如 mongodb://192.168.1.1:27017} \
-p 80:80 -p 8080:8080 testing-platform:1.0.0
```

## Agent 源码安装

可选，当需要观察被测服务器的资源使用情况时，在被测服务器进行安装。

- 克隆代码

``` bash
git clone https://github.com/ErikXu/testing-platform.git
```

- 编译程序

``` bash
cd testing-platform/src
bash build_agent.sh
```

- 打包镜像

``` bash
bash pack_dev.sh
```

- 运行 Agent

``` bash
docker run --name testing-agent -d --pid=host \
-e SERVERDOMAIN="{平台地址，如 http://192.168.1.1:5000}" \
-e CLIENTADDRESS="{被测服务器可访问地址，如 192.168.1.2}" \
-e AGENTPORT=5001 \
-e MONITORPORT=5002 \
-p 5001:5001 -p 5002:5002 \
testing-agent:1.0.0
```
