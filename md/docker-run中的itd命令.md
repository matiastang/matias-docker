<!--
 * @Author: tangdaoyong
 * @Date: 2021-01-22 17:36:38
 * @LastEditors: tangdaoyong
 * @LastEditTime: 2021-01-22 18:25:44
 * @Description: run
-->
# run 

Docker 参数 -i -t 的作用
通常的解释是: -t让docker分配一个伪终端并绑定到容器的标准输入上, -i则让容器的标准输入保持打开.

问题
所以通常都是这样的: sudo docker run -it ubuntu 进入了命令交互界面
但是如果不加呢? sudo docker run ubuntu 或sudo docker create ubuntu & sudo docker start ubuntu
这样的话, docker容器无法启动。

原因
Docker中系统镜像的缺省命令是 bash，如果不加 -ti bash 命令执行了自动会退出。这是因为如果没有衔接输入流，本身就会马上结束。加-ti 后docker命令会为容器分配一个伪终端，并接管其stdin/stdout支持交互操作，这时候bash命令不会自动退出。

官方文档：https://docs.docker.com/engine/reference/commandline/run/

选项	选项简写	说明
–detach	-d	在后台运行容器，并且打印容器id。
–interactive	-i	即使没有连接，也要保持标准输入保持打开状态，一般与 -t 连用。
–tty	-t	分配一个伪tty，一般与 -i 连用。
“-”与“–”的区别请参考：Linux编程：命令行选项单横线“-”与双横线“–”的区别

## -it 选项
使用 ubuntu:19.10 镜像创建并运行一个名称为 ubuntu1910 的容器，-i 选项指示 docker 要在容器上打开一个标准的输入接口，-t 指示 docker 要创建一个伪 tty 终端，连接容器的标准输入接口，之后用户就可以通过终端进行输入。由于 docker run [OPTIONS] IMAGE [COMMAND] [ARG...] 命令的默认 COMMAND 为 /bin/bash，因此用户的输入是基于 bash shell 执行的。

示例中，在终端上输入了 exit 13 ，回车执行该命令，退出终端。该命令被传递到 docker run 的调用方，并且被记录到容器的 metadata 中。

[root@iZ2ze6ogddtzz4dzyy00xwZ ~]# docker run --name ubuntu1910 -it ubuntu:19.10
root@cd83bc3b0d3b:/# exit 13
exit
1
2
3
通过 docker ps -a 命令查看容器，Exited (13) 35 seconds ago 就是被回写的内容。

[root@iZ2ze6ogddtzz4dzyy00xwZ ~]# docker ps -a | grep ubuntu1910
cd83bc3b0d3b        ubuntu:19.10                                             "/bin/bash"              46 seconds ago      Exited (13) 35 seconds ago                                       ubuntu1910
[root@iZ2ze6ogddtzz4dzyy00xwZ ~]# 
1
2
3
容器的 metadata 在 /var/lib/docker/containers/containerId/ 目录下，其中 containerId-json.log 文件中记录了回写的内容。

[root@iZ2ze6ogddtzz4dzyy00xwZ cd83bc3b0d3bb76fc9fc5df326235b3554c15455891d3b3aa0921fb16796322d]# cat cd83bc3b0d3bb76fc9fc5df326235b3554c15455891d3b3aa0921fb16796322d-json.log 
{"log":"\u001b]0;root@cd83bc3b0d3b: /\u0007root@cd83bc3b0d3b:/# exit 13\r\n","stream":"stdout","time":"2020-02-08T14:35:22.509224333Z"}
{"log":"exit\r\n","stream":"stdout","time":"2020-02-08T14:35:22.509286061Z"}
[root@iZ2ze6ogddtzz4dzyy00xwZ cd83bc3b0d3bb76fc9fc5df326235b3554c15455891d3b3aa0921fb16796322d]# 
## -d 选项
使用 docker run -d 在后台创建并启动名称为 ubuntu1 的容器，通过 docker ps 命令没有查找到处于运行状态的容器，通过 docker ps -a 命令查找到已经停止运行的 ubuntu1 容器。

[root@iZ2ze6ogddtzz4dzyy00xwZ ~]# docker run -d --name ubuntu1 ubuntu:19.10
315cc38afc2f06abb5a2fbb075ebca16455367b2de685cf0c5ba828ab62dd5a1
[root@iZ2ze6ogddtzz4dzyy00xwZ ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                               NAMES
c04afe750081        mysql:5.7           "docker-entrypoint.s…"   26 hours ago        Up 26 hours         0.0.0.0:3306->3306/tcp, 33060/tcp   mysql5.7
[root@iZ2ze6ogddtzz4dzyy00xwZ ~]# docker ps -a | grep ubuntu1
315cc38afc2f        ubuntu:19.10                                             "/bin/bash"              35 seconds ago      Exited (0) 34 seconds ago                                        ubuntu1
cd83bc3b0d3b        ubuntu:19.10                                             "/bin/bash"              18 minutes ago      Exited (13) 18 minutes ago                                       ubuntu1910
[root@iZ2ze6ogddtzz4dzyy00xwZ ~]# 
于是疑惑产生了， -d 是保证容器在后台运行，为什么我的容器停止运行了呢？

前面提到过， docker run [OPTIONS] IMAGE [COMMAND] [ARG...] 中有一个 COMMAND 参数，容器启动后会执行 COMMAND命令，它的默认值为 /bin/bash。也就是说容器在后台启动成功后，执行了 COMMAND 命令后直接关闭了。

docker命令请参考：https://blog.csdn.net/claram/article/details/103301942

了解到该原理后，我们可以通过在 docker run -d 后增加一个驻留在进程中长期运行的命令就可以保证容器不关闭了。

[root@iZ2ze6ogddtzz4dzyy00xwZ ~]# docker run -d --name ubuntu2 ubuntu:19.10 tail -f /dev/null 
a0d3c58fc68b139f63355594dd91c2d047b84a3d56880418eedcd8fedb6307b6
[root@iZ2ze6ogddtzz4dzyy00xwZ ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                               NAMES
a0d3c58fc68b        ubuntu:19.10        "tail -f /dev/null"      5 seconds ago       Up 4 seconds                                            ubuntu2
c04afe750081        mysql:5.7           "docker-entrypoint.s…"   26 hours ago        Up 26 hours         0.0.0.0:3306->3306/tcp, 33060/tcp   mysql5.7
[root@iZ2ze6ogddtzz4dzyy00xwZ ~]# 

### 处理-d退出

1. 通过`-i`或者`-t`为`-d`提供一个伪`tty n`
```
docker run -i -d images:tags
docker run -t -d images:tags
docker run -itd images:tags
```

2. 将 `tail -f /dev/null` 添加到命令中

通过执行此操作，即使主命令在后台运行，容器也不会停止，因为tail会在前台继续运行。

## docker-compose up启动之后直接退出

用docker-compose部署镜像后 运行docker-compose up -d 容器状态是Exited (0)

搜了 加上

stdin_open: true
tty: true
后还是没用

 

对比发现 启动正常的镜像：

"Entrypoint": [
                "/docker-entrypoint.sh"
            ]

异常的镜像：

"Entrypoint": null,

原因是容器最后一个进程在容器启动后马上退出，则容器也会退出

entrypoint配置，该配置的作用是在容器启动之前做一些初始化配置，如果没有则容器启动时没有进程，容器也会退出，所以要在docker-compose.yml 文件中加上  entrypoint: /docker-entrypoint.sh 即可
