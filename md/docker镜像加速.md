<!--
 * @Author: tangdaoyong
 * @Date: 2021-01-18 15:30:40
 * @LastEditors: tangdaoyong
 * @LastEditTime: 2021-01-18 22:08:30
 * @Description: docker镜像加速
-->
# docker镜像加速

[docker镜像加速](https://www.runoob.com/docker/docker-mirror-acceleration.html)

国内从 DockerHub 拉取镜像有时会遇到困难，此时可以配置镜像加速器。Docker 官方和国内很多云服务商都提供了国内加速器服务，例如：

* 网易：https://hub-mirror.c.163.com/
* 阿里云：https://<你的ID>.mirror.aliyuncs.com
* ustc http://docker.mirrors.ustc.edu.cn
* 七牛云加速器：https://reg-mirror.qiniu.com
* Docker 官方中国区(貌似现在不能用) https://registry.docker-cn.com

## 阿里云镜像加速
[登录阿里云](https://account.aliyun.com/login/login.htm)
阿里云镜像获取地址：https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors，登陆后，左侧菜单选中镜像加速器就可以看到你的专属地址了。

当配置某一个加速器地址之后，若发现拉取不到镜像，请切换到另一个加速器地址。国内各大云服务商均提供了 Docker 镜像加速服务，建议根据运行 Docker 的云平台选择对应的镜像加速服务。

对于使用 `Mac OS X` 的用户，在任务栏点击 `Docker for mac` 应用图标-> `Perferences`-> Daemon-> Registrymirrors。在列表中填写加速器地址 https://reg-mirror.qiniu.com 。修改完成之后，点击 Apply&Restart 按钮，Docker 就会重启并应用配置的镜像地址了。

`Perferences`-> `Docker Engine`:
```json
{
  "features": {
    "buildkit": true
  },
  "experimental": false,
"registry-mirrors": [
"https://registry.docker-cn.com",
"https://reg-mirror.qiniu.com",
"https://hub-mirror.c.163.com"
]
}
```
**注意**registry-mirrors千万不要用https，而是用http，否则会显示No certs for egitstry.docker.com，
insecure-registries不要任何http头，否则无法通过。
[其他配置](https://docs.docker.com/engine/reference/commandline/dockerd/)

检查加速器是否生效
检查加速器是否生效配置加速器之后，如果拉取镜像仍然十分缓慢，请手动检查加速器配置是否生效，在命令行执行 `docker info`，如果从结果中看到了如下内容，说明配置成功。
```
$ docker info
Registry Mirrors:
  https://registry.docker-cn.com/
  https://reg-mirror.qiniu.com/
  https://hub-mirror.c.163.com/
```
测试，运行`docker run hello-world`则成功。
```js
$ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
0e03bdcc26d7: Pull complete
Digest: sha256:31b9c7d48790f0d8c50ab433d9c3b7e17666d6993084c002c2ff1ca09b96391d
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```