<!--
 * @Author: tangdaoyong
 * @Date: 2021-01-21 17:30:42
 * @LastEditors: tangdaoyong
 * @LastEditTime: 2021-01-25 13:39:45
 * @Description: Compose
-->
# Compose

[官方文档 Docker Compose](https://docs.docker.com/compose/)
[Docker Compose使用](https://www.cnblogs.com/lfri/p/11670159.html)

## Compose 简介
Compose 是用于定义和运行多容器 Docker 应用程序的工具。通过 Compose，您可以使用 YML 文件来配置应用程序需要的所有服务。然后，使用一个命令，就可以从 YML 文件配置中创建并启动所有服务。

如果你还不了解 YML 文件配置，可以先阅读 [YAML 入门教程](https://www.runoob.com/w3cnote/yaml-intro.html)。

Compose 使用的三个步骤：

* 使用 Dockerfile 定义应用程序的环境。
* 使用 docker-compose.yml 定义构成应用程序的服务，这样它们可以在隔离环境中一起运行。
* 最后，执行 docker-compose up 命令来启动并运行整个应用程序。

## yml

[【重要】yml文件编写详解](https://blog.csdn.net/qq_36148847/article/details/79427878)

```yml
version: "3"
services:

  redis:
    image: redis:alpine
    ports:
      - "6379"
    networks:
      - frontend
    deploy:
      replicas: 2
      update_config:
        parallelism: 2
        delay: 10s
      restart_policy:
        condition: on-failure

  db:
    image: postgres:9.4
    volumes:
      - db-data:/var/lib/postgresql/data
    networks:
      - backend
    deploy:
      placement:
        constraints: [node.role == manager]

  vote:
    image: dockersamples/examplevotingapp_vote:before
    ports:
      - 5000:80
    networks:
      - frontend
    depends_on:
      - redis
    deploy:
      replicas: 2
      update_config:
        parallelism: 2
      restart_policy:
        condition: on-failure

  result:
    image: dockersamples/examplevotingapp_result:before
    ports:
      - 5001:80
    networks:
      - backend
    depends_on:
      - db
    deploy:
      replicas: 1
      update_config:
        parallelism: 2
        delay: 10s
      restart_policy:
        condition: on-failure

  worker:
    image: dockersamples/examplevotingapp_worker
    networks:
      - frontend
      - backend
    deploy:
      mode: replicated
      replicas: 1
      labels: [APP=VOTING]
      restart_policy:
        condition: on-failure
        delay: 10s
        max_attempts: 3
        window: 120s
      placement:
        constraints: [node.role == manager]

  visualizer:
    image: dockersamples/visualizer:stable
    ports:
      - "8080:8080"
    stop_grace_period: 1m30s
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock"
    deploy:
      placement:
        constraints: [node.role == manager]

networks:
  frontend:
  backend:

volumes:
  db-data:
```

### 文件配置

compose 文件是一个定义服务、 网络和卷的 YAML 文件 。Compose 文件的默认路径是 ./docker-compose.yml

提示：可以是用 .yml 或 .yaml 作为文件扩展名

服务定义包含应用于为该服务启动的每个容器的配置，就像传递命令行参数一样 docker container create。同样，网络和卷的定义类似于 docker network create 和 docker volume create。

正如 docker container create 在 Dockerfile 指定选项，如 CMD、 EXPOSE、VOLUME、ENV，在默认情况下，你不需要再次指定它们docker-compose.yml。

可以使用 Bash 类 ${VARIABLE} 语法在配置值中使用环境变量。


下面实例中，提供 web 、worker以及db 服务，伴随着两个网络 new 和 legacy 。
```yml
version: '2'

services:
  web:
    build: ./web
    networks:
      - new

  worker:
    build: ./worker
    networks:
      - legacy

  db:
    image: mysql
    networks:
      new:
        aliases:
          - database
      legacy:
        aliases:
          - mysql

networks:
  new:
  legacy:
```
相同的服务可以在不同的网络有不同的别名

## docker-compose 通过sh命令启动nginx 容器自动退出exited with code 0

docker-compose使用的模板文件中有通过entrypoint或者command参数设置容器启动自动执行sh命令开启nginx服务，但是docker-compose up后容器自动退出了
nginx-web1 exited with code 0
nginx-web2 exited with code 0
nginx-web3 exited with code 0

原因:Docker的机制是让容器后台运行,必须至少有一个前台进程，容器运行的命令如果不是那些一直挂起的命令（比如运行top，tail），会自动退出
解决: 可以使用包含 `-g “daemon off;”` 配置项的sh命令以前台方式开启nginx服务

```
nginx -c /usr/local/nginx/conf/nginx.conf -g "daemon off;"
```
或`.yml`
```yml
version: "3.0"

services:
  web:
    # build: .
    # image: web:1
    command: [ "nginx", "-c", "/etc/nginx/nginx.conf", "-g", "daemon off;" ]
    container_name: web-app        # 指定容器的名称
    image: matias/web:1                   # 指定镜像和版本
    ports: 
      - "127.0.0.1:3000:80"
    # command: nginx -c /etc/nginx/nginx.conf #这行代码解决无法访问的问题
    tty: true # tty:true 防止启动之后直接退出，有时候这样也不行，需要再增加一个command:/bin/bash，命令不一定是这个，需要是一个不会退出的命令，然后用-d后台启动容器。
```
如果容器需要同时启动多个进程，只需要将其中一个挂起到前台即可，例如:
service php-fpm start && nginx -g "daemon off;" 
1
或

service php-fpm start && service nginx start &&  tail -f /var/log/nginx/error.log
1
部分参考:https://blog.csdn.net/u010358168/article/details/81347927
https://blog.csdn.net/meegomeego/article/details/50707532