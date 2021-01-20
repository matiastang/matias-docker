<!--
 * @Author: tangdaoyong
 * @Date: 2021-01-18 22:18:15
 * @LastEditors: tangdaoyong
 * @LastEditTime: 2021-01-20 10:30:18
 * @Description: docker常用命令
-->
# docker常用命令

* docker version
检查 Docker 是否正在运行
```
Client: Docker Engine - Community
 Cloud integration: 1.0.7
 Version:           20.10.2
 API version:       1.41
 Go version:        go1.13.15
 Git commit:        2291f61
 Built:             Mon Dec 28 16:12:42 2020
 OS/Arch:           darwin/amd64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.2
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.13.15
  Git commit:       8891c58
  Built:            Mon Dec 28 16:15:28 2020
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.4.3
  GitCommit:        269548fa27e0089a8b8278fc4fc781d7f65a939b
 runc:
  Version:          1.0.0-rc92
  GitCommit:        ff819c7e9184c13b7c2607fe6c30ae19403a7aff
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```

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

5. docker image inspect
查看镜像的分层
```
$ docker image inspect swift:latest
```
```json
[
    {
        "Id": "sha256:a310922a8f500ddd08de2b5dd61978b0b980186a665a1c1062a0d9b4c2104160",
        "RepoTags": [
            "swift:latest"
        ],
        "RepoDigests": [
            "swift@sha256:9676eb643577efb73f9e094871872eacf4f7a14b41fd1110e02051d880c60c16"
        ],
        "Parent": "",
        "Comment": "",
        "Created": "2020-12-17T16:17:35.432159031Z",
        "Container": "3684498430dfad3c721d445592f2887142caf2348d3b3d976850a47f59ed35f2",
        "ContainerConfig": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "SWIFT_SIGNING_KEY=A62AE125BBBFBB96A6E042EC925CC1CCED3D1561",
                "SWIFT_PLATFORM=ubuntu18.04",
                "SWIFT_BRANCH=swift-5.3.2-release",
                "SWIFT_VERSION=swift-5.3.2-RELEASE",
                "SWIFT_WEBROOT=https://swift.org/builds/"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "swift --version"
            ],
            "Image": "sha256:ebd7dfb3a6a21134356f119f1f5bf4d841bc67f147b4612ebae62a1c9cabdd43",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "Description": "Docker Container for the Swift programming language",
                "maintainer": "Swift Infrastructure <swift-infrastructure@swift.org>"
            }
        },
        "DockerVersion": "19.03.12",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "SWIFT_SIGNING_KEY=A62AE125BBBFBB96A6E042EC925CC1CCED3D1561",
                "SWIFT_PLATFORM=ubuntu18.04",
                "SWIFT_BRANCH=swift-5.3.2-release",
                "SWIFT_VERSION=swift-5.3.2-RELEASE",
                "SWIFT_WEBROOT=https://swift.org/builds/"
            ],
            "Cmd": [
                "/bin/bash"
            ],
            "Image": "sha256:ebd7dfb3a6a21134356f119f1f5bf4d841bc67f147b4612ebae62a1c9cabdd43",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "Description": "Docker Container for the Swift programming language",
                "maintainer": "Swift Infrastructure <swift-infrastructure@swift.org>"
            }
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 1758321895,
        "VirtualSize": 1758321895,
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/7f309930db016ab9c2d61fd7ce2de3a0aa117701a61f030aaab5647513badbd3/diff:/var/lib/docker/overlay2/c31a021595fabf99fededa9b2630f3a9db204999c001fd2cfb2cdc2fe29a3845/diff:/var/lib/docker/overlay2/9c20b6dc17c8343b60e9dbad0fb3de83595722ab5c11409dcfec36bb244dc325/diff:/var/lib/docker/overlay2/dc293d5d964a3692eaa1d9a22418a7017b14df9c8f6e912bbee3b7bb5aedc53b/diff",
                "MergedDir": "/var/lib/docker/overlay2/45db413e97ea48bd7aef6435cf9a3ff02746833b1d7cfabe8863378d140dcaf5/merged",
                "UpperDir": "/var/lib/docker/overlay2/45db413e97ea48bd7aef6435cf9a3ff02746833b1d7cfabe8863378d140dcaf5/diff",
                "WorkDir": "/var/lib/docker/overlay2/45db413e97ea48bd7aef6435cf9a3ff02746833b1d7cfabe8863378d140dcaf5/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:b43408d5f11b7b2faf048ae4eb25c296536c571fb2f937b4f1c3883386e93d64",
                "sha256:23135df75b44a66efa9d8dc1a10051768c27bd95388f436eb9553e0eb17211f6",
                "sha256:fe6d8881187d429af3f636c574911690455825998c9f366e985eab646665e711",
                "sha256:3ca72e6c2d895f956246a28c8b6badc3e64db792b35d7553a0b24af931bd1f06",
                "sha256:41d25d24e132f32dd7604c2ae1ebd56b3ddb14422a826e840d5d8d31244aeb40"
            ]
        },
        "Metadata": {
            "LastTagTime": "0001-01-01T00:00:00Z"
        }
    }
]
```

6. docker history
命令显示了镜像的构建历史记录，但其并不是严格意义上的镜像分层。例如，有些 Dockerfile 中的指令并不会创建新的镜像层。比如 ENV、EXPOSE、CMD 以及 ENTRY- POINT。不过，这些命令会在镜像中添加元数据。

7. docker images --digests
每次拉取镜像，摘要都会作为 docker image pull 命令返回代码的一部分。只需要在 docker image ls 命令之后添加 --digests 参数即可在本地查看镜像摘要。

8. docker image tag
为镜像打标签命令的格式是docker image tag <current-tag> <new-tag>，其作用是为指定的镜像添加一个额外的标签，并且不需要覆盖已经存在的标签。

再次执行 docker image ls 命令，可以看到这个镜像现在有了两个标签，其中一个包含 Docker ID nigelpoulton。
$ docker image ls
REPO TAG IMAGE ID CREATED SIZE
web latest fc69fdc4c18e 10 secs ago 64.4MB
nigelpoulton/web latest fc69fdc4c18e 10 secs ago 64.4MB

### docker build
构建镜像

* 每一个 RUN 指令会新增一个镜像层。因此，通过使用 && 连接多个命令以及使用反斜杠（\）换行的方法，将多个命令包含在一个 RUN 指令中，通常来说是一种值得提倡的方式。
另一个问题是开发者通常不会在构建完成后进行清理。当使用 RUN 执行一个命令时，可能会拉取一些构建工具，这些工具会留在镜像中移交至生产环境。

有多种方式来改善这一问题——比如常见的是采用建造者模式（Builder Pattern）。但无论采用哪种方式，通常都需要额外的培训，并且会增加构建的复杂度。

建造者模式需要至少两个 Dockerfile，一个用于开发环境，一个用于生产环境。

首先需要编写 Dockerfile.dev，它基于一个大型基础镜像（Base Image），拉取所需的构建工具，并构建应用。

接下来，需要基于 Dockerfile.dev 构建一个镜像，并用这个镜像创建一个容器。

这时再编写 Dockerfile.prod，它基于一个较小的基础镜像开始构建，并从刚才创建的容器中将应用程序相关的部分复制过来。

整个过程需要编写额外的脚本才能串联起来。

这种方式是可行的，但是比较复杂。

多阶段构建（Multi-Stage Build）是一种更好的方式！

多阶段构建能够在不增加复杂性的情况下优化构建过程。

下面介绍一下多阶段构建方式。

多阶段构建方式使用一个 Dockerfile，其中包含多个 FROM 指令。每一个 FROM 指令都是一个新的构建阶段（Build Stage），并且可以方便地复制之前阶段的构件。

[docker容器化 - 多阶段构建](http://c.biancheng.net/view/3159.html)
## 容器（Container）

1. docker run
> 启动容器
`docker container run <options> <im- age>:<tag> <app>`
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
Shell 提示符已经变为 root@3027eb644874:/#。@ 之后的一长串数字就是容器唯一 ID 的前 12 个字符。

* Bash Shell 成为容器中运行的且唯一运行的进程。可以通过ps -elf命令在容器内部查看。

已经知道 docker container run 会启动一个新容器，但是这次使用 -d 参数替换了 -it。-d 表示后台模式，告知容器在后台运行。
然后为容器命名，并且指定了 -p 80:8080。-p 参数将 Docker 主机的端口映射到容器内。
本例中，将 Docker 主机的 80 端口映射到了容器内的 8080 端口。这意味着当有流量访问主机的 80 端口的时候，流量会直接映射到容器内的 8080 端口。
```
$ docker container run -d --name webserver -p 80:8080
```
使用了 -d 参数启动容器，并在后台运行。这种后台启动的方式不会将当前终端连接到容器当中。
使用 `docker container ls` 命令可以查看当前运行的容器以及端口的映射情况。端口信息按照 `host-port:container-port` 的格式显示，明确这一点很重要。

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
`docker container rm <container> -f`
容器正在运行 /bin/bash 应用。当使用 docker container rm <container> -f来销毁运行中的容器时，不会发出任何告警。
这个过程相当暴力——有点像悄悄接近容器后在脑后突施冷枪。毫无征兆地被销毁，会令容器和应用猝不及防，来不及“处理后事”。
但是，docker container stop 命令就有礼貌多了（就像用枪指着容器的脑袋然后说“你有 10s 时间说出你的遗言”）。
该命令给容器内进程发送将要停止的警告信息，给进程机会来有序处理停止前要做的事情。一旦 docker stop 命令返回后，就可以使用 docker container rm 命令删除容器了。
这背后的原理可以通过 Linux/POSIX 信号来解释。docker container stop 命令向容器内的 PID 1 进程发送了 SIGTERM 这样的信号。
就像前文提到的一样，会为进程预留一个清理并优雅停止的机会。如果 10s 内进程没有终止，那么就会收到 SIGKILL 信号。这是致命一击。但是，进程起码有 10s 的时间来“解决”自己。
docker container rm <container> -f 命令不会先友好地发送 SIGTERM，这条命令会直接发出 SIGKILL。就像刚刚所打的比方一样，该命令悄悄接近并对容器发起致命一击。

8. docker container exec
当前容器仍然在运行，并且可以通过 docker container exec 命令将终端重新连接到 Docker，理解这一点很重要。
```
docker container exec -it 3027eb644874 bash
```

### 利用重启策略进行容器的自我修复
通常建议在运行容器时配置好重启策略。这是容器的一种自我修复能力，可以在指定事件或者错误后重启来完成自我修复。

重启策略应用于每个容器，可以作为参数被强制传入 docker-container run 命令中，或者在 Compose 文件中声明（在使用 Docker Compose 以及 Docker Stacks 的情况下）。

容器支持的重启策略包括 `always`、`unless-stopped` 和 `on-failed`。

always 策略是一种简单的方式。除非容器被明确停止，比如通过 docker container stop 命令，否则该策略会一直尝试重启处于停止状态的容器。

一种简单的证明方式是启动一个新的交互式容器，并在命令后面指定 --restart always 策略，同时在命令中指定运行 Shell 进程。

当容器启动的时候，会登录到该 Shell。退出 Shell 时会杀死容器中 PID 为 1 的进程，并且杀死这个容器。

但是因为指定了 --restart always 策略，所以容器会自动重启。如果运行 docker container ls 命令，就会看到容器的启动时间小于创建时间。下面请看示例。
$ docker container run --name neversaydie -it --restart always alpine sh

//等待几秒后输入exit

/# exit

$ docker container ls
CONTAINER ID IMAGE COMMAND CREATED STATUS
0901afb84439 alpine "sh" 35 seconds ago Up 1 second

注意，容器于 35s 前被创建，但却在 1s 前才启动。这是因为在容器中输入退出命令的时候，容器被杀死，然后 Docker 又重新启动了该容器。

--restart always 策略有一个很有意思的特性，当 daemon 重启的时候，停止的容器也会被重启。

例如，新创建一个容器并指定 --restart always 策略，然后通过 docker container stop命令停止该容器。

现在容器处于 Stopped (Exited) 状态。但是，如果重启 Docker daemon，当 daemon 启动完成时，该容器也会重新启动。

always 和 unless-stopped 的最大区别，就是那些指定了 --restart unless-stopped 并处于 Stopped (Exited) 状态的容器，不会在 Docker daemon 重启的时候被重启。

下面创建两个新容器，其中“always”容器指定 --restart always 策略，另一个“unless- stopped”容器指定了 --restart unless-stopped 策略。

两个容器均通过 docker container stop 命令停止，接着重启 Docker。结果“always”容器会重启，但是“unless-stopped”容器不会。
1) 创建两个新容器
$ docker container run -d --name always \
--restart always \
alpine sleep 1d

$ docker container run -d --name unless-stopped \
--restart unless-stopped \
alpine sleep 1d

$ docker container ls
CONTAINER ID IMAGE COMMAND STATUS NAMES
3142bd91ecc4 alpine "sleep 1d" Up 2 secs unless-stopped
4f1b431ac729 alpine "sleep 1d" Up 17 secs always

现在有两个运行的容器了。一个叫作“always”，另一个叫作“unless-stopped”。
2) 停止两个容器
$ docker container stop always unless-stopped

$ docker container ls -a
CONTAINER ID IMAGE STATUS NAMES
3142bd91ecc4 alpine Exited (137) 3 seconds ago unless-stopped
4f1b431ac729 alpine Exited (137) 3 seconds ago always

3) 重启 Docker
重启 Docker 的过程在不同的操作系统上可能不同。下面介绍一下如何在 Linux 上使用 systemd 重启 Docker，在 Windows Server 2016 上可以使用 restart-service 重启。
$ systemlctl restart docker

4) 一旦 Docker 重启成功，检查两个容器的状态
$ docker container ls -a
CONTAINER CREATED STATUS NAMES
314．.cc4 2 minutes ago Exited (137) 2 minutes ago unless-stopped
4f1..729 2 minutes ago Up 9 seconds always

注意到“always”容器（启动时指定了 --restart always 策略）已经重启了，但是“unless-stopped”容器（启动时指定了 --restart unless-stopped 策略）并没有重启。

on-failure 策略会在退出容器并且返回值不是 0 的时候，重启容器。

就算容器处于 stopped 状态，在 Docker daemon 重启的时候，容器也会被重启。

如果读者使用 Docker Compose 或者 Docker Stack，可以在 service 对象中配置重启策略，示例如下。
version: "3.5"
services:
myservice:
<Snip>
restart_policy:
condition: always | unless-stopped | on-failure

### 快速清理
接下来了解一种简单且快速的清理 Docker 主机上全部运行容器的方法。

这种处理方式会强制删除所有的容器，并且不会给容器完成清理的机会。

这种操作一定不能在生产环境系统或者运行着重要容器的系统上执行。

在 Docker 主机的 Shell 中运行下面的命令，可以删除全部容器。
$()

在本例中，因为只有一个运行中的容器，所以只有一个容器被删除（6efa1838cd51）。

但是该命令的工作方式，就跟前面章节中用于删除某台 Docker 主机上全部容器的命令 rm $(docker image ls -q) 一样，docker container rm 命令会删除容器。

如果将 $(docker container ls -aq) 作为参数传递给 docker container rm 命令，等价于将系统中每个容器的 ID 传给该命令。

-f 标识表示强制执行，所以即使是处于运行状态的容器也会被删除。接下来，无论是运行中还是停止的容器，都会被删除并从系统中移除。

## 仓库（Repository）