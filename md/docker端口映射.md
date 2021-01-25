<!--
 * @Author: tangdaoyong
 * @Date: 2021-01-25 13:37:21
 * @LastEditors: tangdaoyong
 * @LastEditTime: 2021-01-25 13:37:56
 * @Description: 端口映射
-->
# 端口映射

docker和docker-compose 端口映射
基本遵循规则是从宿主机映射到容器，默认是tcp，如果使用udp，比如5600，要记得在运行时或者yaml文件端口处比如写：5000/udp

docker-compose映射端口的标签。
使用HOST:CONTAINER格式或者只是指定容器的端口，宿主机会随机映射端口。

复制代码
ports:
 - "3000"
 - "8000:8000"
 - "49100:22"
 - "127.0.0.1:8001:8001"
复制代码
https://blog.csdn.net/zhuchunyan_aijia/article/details/80111629

 

docker映射端口: 

宿主机映射到容器

docker run -itd -p 9201:9200 --name es_test es:latest 

docker run -itd -p 10.0.0.1:9201:9200 --name es_test es:latest 