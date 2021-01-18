<!--
 * @Author: tangdaoyong
 * @Date: 2021-01-18 22:18:15
 * @LastEditors: tangdaoyong
 * @LastEditTime: 2021-01-18 22:42:25
 * @Description: docker常用命令
-->
# docker常用命令

## 镜像（Image）

1. docker images
> 列出本地主机上的镜像
```
$ docker images
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
ubuntu        latest    f643c72bc252   7 weeks ago     72.9MB
hello-world   latest    bf756fb1ae65   12 months ago   13.3kB
```
* REPOSITORY：表示镜像的仓库源
* TAG：镜像的标签
* IMAGE ID：镜像ID
* CREATED：镜像创建时间
* SIZE：镜像大小
同一仓库源可以有多个 TAG，代表这个仓库源的不同个版本，如 ubuntu 仓库源里，可以有 15.10、14.04 等多个不同的版本，我们使用 REPOSITORY:TAG 来定义不同的镜像。

2. docker search
> 查找镜像
可以从以下网址中搜索需要的镜像：
```js
// Docker Hub
https://hub.docker.com/
// 官方：
https://store.docker.com/
// DaoCloud:
http://hub.daocloud.io/
// AliCloud
https://dev.aliyun.com/
```
我们也可以使用 docker search 命令来搜索镜像。如`docker search ubuntu`

3. docker pull
拉取镜像`docker pull httpd`
拉取指定镜像`docker pull ubuntu:13.10`

4. docker rmi
> 删除镜像
```
$ docker rmi hello-world
```

### docker build
构建镜像

## 容器（Container）

1. docker run
> 启动容器
```
$ docker run -it ubuntu /bin/bash
```
参数说明：

-i: 交互式操作。
-t: 终端。
ubuntu: ubuntu 镜像。
/bin/bash：放在镜像名后的是命令，这里我们希望有个交互式 Shell，因此用的是 /bin/bash。
要退出终端，直接输入 exit:
```
root@ed09e4490c57:/# exit
```
2. docker ps
> 查看所有的容器：
```
$ docker ps -a
```

3. docker start 
> 启动一个已停止的容器
```
$ docker start b750bbbcfd88 
```

4. 后台运行
在大部分的场景下，我们希望 docker 的服务是在后台运行的，我们可以过 -d 指定容器的运行模式。
```
$ docker run -itd --name ubuntu-test ubuntu /bin/bash
```

5. docker stop
停止容器的命令如下：
```
$ docker stop <容器 ID>
```
6. docker restart
停止的容器可以通过 docker restart 重启：
```
$ docker restart <容器 ID>
```

7. docker rm
删除容器使用 docker rm 命令：
```
$ docker rm -f 1e560fca3906
```
## 仓库（Repository）