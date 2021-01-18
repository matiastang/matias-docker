<!--
 * @Author: tangdaoyong
 * @Date: 2021-01-18 15:23:20
 * @LastEditors: tangdaoyong
 * @LastEditTime: 2021-01-18 21:37:01
 * @Description: 安装
-->
# 安装docker

[MacOS Docker安装](https://www.runoob.com/docker/macos-docker-install.html)

## MacOS

### homebrew安装

mac上有安装过homebrew可以使用 Homebrew 来安装 Docker。
Homebrew 的 Cask 已经支持 Docker for Mac，在终端输入：
```
$ brew cask install docker
```
应用程序中点击`docker`图标进行安装
在终端查看`docker`版本：
```
$ docker --version
$ docker -v
```
终端输入`docker run hello-world`，如果出现`Unable to find image 'hello-world:latest' locally`，在设置中加入国内的镜像


可以从以下网址中搜索需要的镜像：
```js
// 官方：
https://store.docker.com/
// DaoCloud:
http://hub.daocloud.io/
// AliCloud
https://dev.aliyun.com/
```