# Docker

1. 传统项目部署出现的问题
* 依赖关系复杂，容易出现兼容性问题
* 开发、测试、生产环境有差异

2. Docker优点
* 允许开发中将应用、依赖、函数库、配置一起打包，形成可移植镜像
* 应用运行在容器中，使用沙盒机制，相互隔离

3. Docker与虚拟机
* docker是一个系统进程；虚拟机是在操作系统中的操作系统
* docker体积小、启动速度快、性能好；虚拟机体积大、启动速度慢、性能一般

## 1. Docker

### 1.1 Docker架构
1. 镜像：将应用程序及其需要的系统函数库、环境、配置、依赖打包而成
2. 容器：镜像中的应用程序运行后形成的进程
3. Dockerhub：一个Docker镜像的托管平台(Docker Registry)
4. Docker架构（C/S架构）
* server: Docker守护进程，负责处理Docker命令，管理镜像、容器等
* client: 通过命令或RestAPI向Docker服务端发送指令

### 1.2 操作镜像命令
```docker
# 构建镜像
docker build  

# 推送镜像
docker push  

# 拉取镜像
docker pull  

# 保存镜像为一个压缩包
docker save  
eg:  docker save -o nginx.tar nginx:latest

# 加载压缩包为镜像
docker load  
eg: docker load -i nginx.tar

# 查看镜像
docker images 

# 删除镜像
docker rmi  

# 运行容器
docker run --name containerName -p 80:80 -d nginx  
# -p 将宿主机端口与容器端口映射，冒号左边是宿主机端口，右侧是容器端口
# -d 后台运行容器
```

### 1.3 容器相关命令
```docker
# 运行-->暂停
docker pause

# 暂停-->运行
docker unpause

# 运行-->停止
docker stop 

# 停止-->运行
docker start 

# 查看所有运行的容器及状态
docker ps 
eg：docker ps -a

# 查看容器运行日志
docker logs 
eg: docker logs -f mn

# 进入容器执行命令
docker exec 
eg: docker exec -it mn bash
# -it 给当前进入的容器创建一个标准输入、输出终端，允许我们与容器交互
# mn 要进入的容器的名称
# bash 进入容器后执行的命令,bash是一个linux终端交互命令

# 删除指定容器
docker rm 
```

### 1.4 数据卷相关命令

1. 数据卷：一个虚拟目录，指向宿主机文件系统中的某个目录

2. 作用：将容器与数据分离，解耦合，方便操作容器内数据，保证数据安全

3. 数据卷操作命令
> docker volume [COMMAND]
* create  创建一个vloume
* inspect  显示一个或多个volume的信息
* ls  列出所有的volume
* prune  删除未使用的volume
* rm  删除一个或多个指定的volume

4. 挂载数据卷
> 在创建容器时，可以通过-v参数来挂在一个数据卷到某个容器目录
>
> -v volume名称：容器目录
> 
> -v 宿主机文件：容器内文件
> 
> -v 宿主机目录：容器内目录

```docker
docker run \
--name mn \
-v html:/root/html \   # 把html数据卷挂载到容器内的/root/html这个目录中
-p 8080:80 \
nginx
```
5. 数据卷挂载与目录直接挂载
* 数据卷挂载耦合度低，由docker来管理目录，但是目录较深，不好找
* 目录挂载耦合度高，需要我们自己管理目录，不过目录容易寻找查看

## 2. Dockerfile自定义镜像

### 2.1 镜像结构(分层结构)
* 入口(Entrypoint)  ``镜像运行入口，一般是程序启动的脚本和参数``
* 层(layer)  ``在BaseImage基础上添加安装包、依赖、配置等，每次操作都形成新的一层``
* 基础镜像(BaseImage)  ``应用依赖的系统函数库、环境、配置、文件等``

### 2.2 自定义镜像-Dockerfile语法
Dockerfile: 文本文件，包含一个个的指令，用指令来说明要执行什么操作来构建镜像，每一个指令都会形成一层Layer

1. 指令
* FROM 指定基础镜像  ``FROM centos:6``
* ENV 设置环境变量，可在后面指令使用  ``ENV key value``
* COPY 拷贝本地文件到镜像的指定目录  ``COPY ./mysql-5.7.rpm /tmp``
* RUN 执行Linux的shell命令，一般是安装过程的命令  ``RUN yum install gcc``
* EXPOSE 指定容器运行时监听的端口，是给镜像使用者看的  ``EXPOSE 8080``
* ENTRYPOINT 镜像中应用的启动命令，容器运行时调用  ``ENTRYPOINT java -jar xx.jar``
```docker
# 基于ubuntu镜像构建一个新镜像，运行一个java项目

# 指定基础镜像
FROM ubuntu:16.04

# 配置环境变量：JDK的安装目录
ENV JAVA_DIR=/usr/local

# 拷贝jdk和java项目的包
COPY ./jdk8.tar.gz $JAVA_DIR/
COPY ./docker-demo.jar /tmp/app.jar

# 安装JDK
RUN cd $JAVA_DIR \
&& tar -xf ./jdk8.tar.gz \
&& mv ./jdk1.8.0_144 ./java8

# 配置环境变量
ENV JAVA_HOME=$JAVA_DIR/java8
ENV PATH=$PATH:$JAVA_HOME/bin

# 暴露端口
EXPOSE 8090

# 入口, java项目的启动命令
ENTRYPOINT java -jar /tmp/app.jar
```
> 构建镜像：docker build -t javaweb:1.0 .

2. 基于java:8-alpine镜像，构建java项目(已经有人把底层的构建出来了，所以只需要在这个基础之上继续构建)
```docker
# 指定基础镜像
FROM java:8-alpine

# 拷贝java项目的包
COPY ./docker-demo.jar /tmp/app.jar

# 入口, java项目的启动命令
ENTRYPOINT java -jar /tmp/app.jar
```

## 3. DockerCompose
1. DockerCompose: 基于Compose文件帮我们快速的部署分布式应用，而无需手动一个个创建和运行容器
2. Compose文件是一个文本文件，通过指令定义集群中的每个容器如何运行
```yml
version: "3.8"

service:
  mysql:
    image: mysql:5.7.25
    environment:
      MYSQL_ROOT_PASSWORD: 123
    volumes:
      - /tmp/mysql/data:/var/lib/mysql
      - /tmp/mysql/conf/hmy.cnf:/etc/mysql/conf.d/hmy.cnf
  web:
    build:
    ports:
      -8090: 8090
```
### 3.1 DockerCompose部署微服务集群
1. 编写docker-compose.yml文件
```yml
version: "3.2"

services:
  nacos:
    image: nacos/nacos-server
    environment:
      MODE: standalone
    ports:
      - "8848:8848"
  mysql:
    image: mysql:5.7.25
    environment:
      MYSQL_ROOT_PASSWORD: 123
    volumes:
      - "$PWD/mysql/data:/var/lib/mysql"
      - "$PWD/mysql/conf:/etc/mysql/conf.d/"
  userservice:
    build: ./user-service
  orderservice:
    build: ./order-service
  gateway:
    build: ./gateway
    ports:
      - "10010:10010"
```
2. 修改自己的项目，将nacos、数据库地址都命名为docker-compose中的服务名
3. 使用maven，将项目中的每个微服务打包为app.jar
4. 将打包好的app.jar拷贝到项目中的每一个对应的子目录中
5. 将微服务上传至虚拟机，利用*docker-compose up -d*来部署
> 这儿会出现错误,nacos部署有点慢，service、mysql部署完后会进行注册，nacos未部署完成会报错
> 
> 解决方案：先部署docker
> 
> 或者重新启动  docker-compose restart gateway userservice orderservice

## 4. Docker镜像仓库

### 4.1 搭建私有镜像仓库

1. 简化版镜像仓库：Docker官方的Docker Registry是一个基础版本的Docker镜像仓库，具备仓库管理的完整功能，但是没有图形化界面。

2. 带有图形化界面仓库：
* 首先*配置Docker信任地址*：私服采用的是http协议，默认不被Docker信任
```sh
# 打开要修改的文件
vi /etc/docker/daemon.json
# 添加内容：
"insecure-registries":["http://192.168.150.101:8080"]  # 这里是你自己虚拟机的位置及端口
# 重加载
systemctl daemon-reload
# 重启docker
systemctl restart docker 
```
* 使用DockerCompose部署带有图象界面的DockerRegistry
```yml
version: '3.0'
services:
  registry:
    image: registry
    volumes:
      - ./registry-data:/var/lib/registry
  ui:
    image: joxit/docker-registry-ui:static
    ports:
      - 8080:80
    environment:
      - REGISTRY_TITLE=个人私有仓库  # 可以自己定义名称
      - REGISTRY_URL=http://registry:5000
    depends_on:
      - registry 
```

### 4.2 在私有镜像仓库推送或拉取镜像
1. 推送镜像到私有镜像服务必须先tag
* 重新tag本地镜像，名称前缀为私有仓库的地址：
> docker tag nginx:latest 192.168.133.128:8080/nginx:1.0

* 推送镜像
> docker push 192.168.133.128:8080/nginx:1.0

* 拉取镜像
> docker pull 192.168.133.128:8080/nginx:1.0