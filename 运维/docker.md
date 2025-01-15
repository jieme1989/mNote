# docker 常用命令

```shell
# 重启docker
service docker restart | start | stop
# 启动容器
docker start [CONTAINER ID]
# 查看详情
docker ps -a 
# 查看镜像列表
dokcer images
# 删除容器
docker rm 容器id
# 删除镜像
docker rmi 镜像id
```

# 示例 docker 安装rabbitmq
## 拉取镜像
```bash 
 docker pull rabbitmq:management
```
## 运行镜像
```shell
# 查看镜像
docker images 
# 启动镜像
docker run -d --name rabbitmq -p 15672:15672 -p 5672:5672 -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=admin [IMAGE ID]
```

```text
Docker 是一种容器。Docker 中十分重要的两个概念 Image 和 container，Image 和 container 可以看作面向对象思想中的类和对象。container 是 Image 的实例化。
Image 是只读的，分为 Base Image 和普通 Image，Base Image 是直接基于内核构造的，例如 Ubuntu Image、Centos Image 等。
```
### Image 的操作
```shell
# 列出本地所有Images
$ docker image ls
# 或者
$ docker images
# 运行某个Image
$ docker run hello-world
```
两种获取 Image 的方法：
Build from Dockerfile：
新建一个 Dockerfile，例：
```shell
FROM centos
RUN yum install -y vim
```
构建 Image：
```shell
$ docker image build -t tag/image-name .
# 或者
$ docker build -t tag/image-name .
```
Pull from Registry（类似 Github）：
```shell
$ docker pull hello-world
```
删除 Image：
```shell
$ docker rmi <IMAGE ID>
# IMAGE ID不必写全
```
Container 的操作
列出 Containers：
```shell
$ docker container ls
# 或者
$ docker ps
# 列出所有包括已经退出的Container
$ docker container ls -a
# 或者
$ docker ps -a
```
交互式运行 Container：
```shell
$ docker run -it centos
# 此centos与宿主机共享内核，使用uname -a查看内核
```
后台运行 container：
```shell
$ docker run -d flask-hello-world
# 如果不使用--name，将会随机生成一个容器名
```
重启某个 Container：
```shell
$ docker start <container_name>
```
删除一个 Container
```shell
$ docker container rm <CONTAINER ID>
# CONTAINER ID不必写全
# 或者
$ docker rm <CONTAINER ID>
```
删除所有 Container：
```shell
$ docker rm $(docker container ls -aq)
# 删除所有已退出的Container
$ docker rm $(docker container ls -f "status=exited" -q)
```
将一个经过修改后的 container 生成为一个新的 image：
```shell
$ docker container commit interesting_wilson x0c/centos-vim
sha256:9a746e51ff5f95dd4119a8bfebf6d678b93b0c4f0bc786bf2df61da495fc7f25
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
x0c/centos-vim      latest              9a746e51ff5f        7 seconds ago       330MB
centos              7                   b5b4d78bc90c        2 weeks ago         203MB
$ docker history 9a746e51ff5f
IMAGE               CREATED             CREATED BY                                      SIZE
9a746e51ff5f        43 seconds ago      /bin/bash                                       127MB
b5b4d78bc90c        2 weeks ago         /bin/sh -c #(nop)  CMD ["/bin/bash"]            0B
<missing>           2 weeks ago         /bin/sh -c #(nop)  LABEL org.label-schema.sc…   0B
<missing>           2 weeks ago         /bin/sh -c #(nop) ADD file:38e2d2a1a0cd8694b…   203MB
```

# docker 拉取不下来的问题

参考网址
```txt
https://github.com/DaoCloud/public-image-mirror
https://github.com/DaoCloud/public-image-mirror/blob/main/allows.txt
```

```shell
docker run -d -P m.daocloud.io/docker.io/library/nginx
```

| 源站                    | 替换为                        |
| ----------------------- | ----------------------------- |
| cr.l5d.io               | l5d.m.daocloud.io             |
| docker.elastic.co       | elastic.m.daocloud.io         |
| docker.io               | docker.m.daocloud.io          |
| gcr.io                  | gcr.m.daocloud.io             |
| ghcr.io                 | ghcr.m.daocloud.io            |
| k8s.gcr.io              | k8s-gcr.m.daocloud.io         |
| registry.k8s.io         | k8s.m.daocloud.io             |
| mcr.microsoft.com       | mcr.m.daocloud.io             |
| nvcr.io                 | nvcr.m.daocloud.io            |
| quay.io                 | quay.m.daocloud.io            |
| registry.jujucharms.com | jujucharms.m.daocloud.io      |
| rocks.canonical.com     | rocks-canonical.m.daocloud.io |

## minio 安装

原命令
```shell
docker pull minio/minio
```
现命令
```shell
docker pull docker.m.daocloud.io/minio/minio
```
```shell
docker run  -p 9000:9000 -p 9090:9090 --name minio \
 -d --restart=always \
 -e MINIO_ACCESS_KEY=minio \
 -e MINIO_SECRET_KEY=minio@123 \
 -v /usr/local/minio/data:/data \
 -v /usr/local/minio/config:/root/.minio \
  minio/minio server /data  --console-address ":9000" --address ":9090"
  
  docker run  -p 9000:9000 -p 9090:9090 --name minio \
 -d --restart=always \
 -e MINIO_ACCESS_KEY=minio \
 -e MINIO_SECRET_KEY=minio@123 \
 -v /usr/local/minio/data:/data \
 -v /usr/local/minio/config:/root/.minio \
  quay.io/minio/minio:latest server /data  --console-address ":9000" --address ":9090"
  
```
## rabbitmq 安装
原命令
```shell
docker pull rabbitmq:3.9.12-management

docker pull rabbitmq:management
```
现命令
```shell
docker pull docker.m.daocloud.io/library/rabbitmq:3.9.12-management

```
```shell
docker run -d -p 5672:5672 \
-p 15672:15672 \
-p 15674:15674 \
--name rabbitmq \
-v /usr/local/rabbitmq:/container/rabbitmq \
rabbitmq:3.9.12-management


docker run -d -p 5672:5672 \
-p 15672:15672 \
-p 15674:15674 \
--name rabbitmq \
-v /usr/local/rabbitmq:/container/rabbitmq \
rabbitmq:management




```
```txt
进入容器内部或者从docker中在Exec中执行下列命令
docker exec -it 容器id bash 

执行下列命令
rabbitmq-plugins enable rabbitmq_web_stomp
rabbitmq-plugins enable rabbitmq_web_stomp_examples
rabbitmq-plugins enable rabbitmq_delayed_message_exchange
```


## KafKa安装

```sh
#安装zookeeper
docker search zookeeper
docker pull wurstmeister/zookeeper
docker run -d --name zookeeper -p 2181:2181 wurstmeister/zookeeper

#安装kafka
docker search kafka
docker pull wurstmeister/kafka
docker run -d --name kafka -p 9092:9092 --link zookeeper:zookeeper \
-v /usr/kafka/config/server.properties:/config/server.properties \
--env KAFKA_ZOOKEEPER_CONNECT=zookeeper:2181 \
--env KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://localhost:9092 \
--env KAFKA_LISTENERS=PLAINTEXT://0.0.0.0:9092 \
--env KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR=1 wurstmeister/kafka 

```

```tex

--name kafka: 设置容器的名字为“kafka”。

-p 9092:9092: 将容器的9092端口映射到宿主机的9092端口。

--link zookeeper:zookeeper: 连接到名为“zookeeper”的另一个Docker容器，并且在当前的容器中可以通过zookeeper这个别名来访问它。

--env KAFKA_ZOOKEEPER_CONNECT=zookeeper:2181: 设置环境变量，指定ZooKeeper的连接字符串。

--env KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://localhost:9092: 设置环境变量，指定Kafka的advertised listeners。

--env KAFKA_LISTENERS=PLAINTEXT://0.0.0.0:9092: 设置环境变量，指定Kafka的listeners。

--env KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR=1: 设置环境变量，指定offsets topic的副本因子。

wurstmeister/kafka: 使用的Docker镜像名字。
```

```sh
#kefaka管理端 - 不成功
docker search kafka-manager
docker pull sheepkiller/kafka-manager
docker run -it --rm  -p 9001:9000 -e ZK_HOSTS="localhost:2181" -e APPLICATION_SECRET=letmein sheepkiller/kafka-manager

```

docker-compose.yml

```yml
version: '2'
services:
  kafka-manager:
    image: sheepkiller/kafka-manager # 如果要安装web管理工具可以同时安装这个，最后通过苏主机IP的9000端口进行访问，例如172.31.148.174:9000
    environment:
      ZK_HOSTS: 192.168.0.66:2181,192.168.0.66:62182,192.168.0.66:62183
      APPLICATION_SECRET: "letmein"
    ports:
      - "39000:9000"
    expose:
      - "9000"
```







## consul 安装

原命令
```shell
docker pull consul:1.15.1 
docker pull hashicorp/consul
```
现命令
```shell
docker pull docker.m.daocloud.io/library/consul:1.15.1
```
```shell
docker run -d --restart=always -p 8500:8500 -v /data/consul:/consul/data -e CONSUL_BIND_INTERFACE='eth0' --name=consul1.15.1 docker.m.daocloud.io/library/consul:1.15.1 agent -server -bootstrap -ui -client='0.0.0.0'

#默认启动
docker run -d --restart=always -p 8500:8500 -v /data/consul:/consul/data -e CONSUL_BIND_INTERFACE='eth0' --name=consul hashicorp/consul:latest agent -server -bootstrap -ui -client='0.0.0.0' 


#宿主启动
docker run -d --restart=always --network=host -v /data/consul:/consul/data -e CONSUL_BIND_INTERFACE='eth0' -e CONSUL_CLIENT_INTERFACE='eth0' --name=consul hashicorp/consul:latest agent -server -bootstrap -ui -client='0.0.0.0' 
```
## consul参数详解

- –net=host docker参数, 使得docker容器越过了net namespace的隔离，免去手动指定端口映射的步骤
- -server consul支持以server或client的模式运行, server是服务发现模块的核心, client主要用于转发请求
- -advertise 通告地址用于更改我们通告给集群中其他节点的地址。默认情况下，-bind地址是通告的。
- -retry-join 指定要加入的consul节点地址，失败后会重试, 可多次指定不同的地址
- -client Consul将绑定客户端接口的地址，包括HTTP和DNS服务器。默认情况下，这是“127.0.0.1”，只允许回送连接。
- -bind 内部集群通信绑定的地址。这是集群中所有其他节点都应该可以访问的IP地址。默认情况下，这是“0.0.0.0”，集群内的所有节点到地址必须是可达的
- -bootstrap-expect 此标志提供数据中心中预期服务器的数量。不应该提供此值，或者该值必须与群集中的其他服务器一致。指定后，Consul将等待指定数量的服务器可用，然后启动群集。允许自动选举leader，但不能与传统-bootstrap标志一起使用, 需要在server模式下运行。
- -data-dir 此标志为代理存储状态提供了一个数据目录。这对所有代理都是必需的。该目录在重新启动时应该是持久的。这对于在服务器模式下运行的代理尤其重要，因为它们必须能够保持群集状态。此外，该目录必须支持使用文件系统锁定，这意味着某些类型的已装入文件夹（例如VirtualBox共享文件夹）可能不合适
- -node 群集中此节点的名称，这在群集中必须是唯一的，默认情况下是节点的主机名。
- -config-dir 指定配置文件，当这个目录下有 .json 结尾的文件就会被加载
- -enable-script-checks 检查服务是否处于活动状态，类似开启心跳
- -datacenter 数据中心名称。如果未提供，则默认为“dc1”。Consul对多个数据中心拥有一流的支持，但它依赖于正确的配置。同一个数据中心内的节点应该位于单个局域网中。
- -ui - 启用内置的Web UI服务器和所需的HTTP路由。这消除了将Consul Web UI文件与二进制文件分开维护的需要。
- -join 指定ip, 加入到已有的集群中



## redis 安装

原命令
```shell
docker pull redis
```
现命令
```shell
docker pull docker.m.daocloud.io/library/redis
```
```shell
docker run --restart=always --log-opt max-size=100m --log-opt max-file=2 -p 6379:6379 --name redis -v /home/wangkuan/redis/redis.conf:/etc/redis/redis.conf -v /home/wangkuan/redis/data:/data -d redis redis-server /etc/redis/redis.conf 


docker run --restart=always --log-opt max-size=100m --log-opt max-file=2 -p 6379:6379 --name redis -v /home/wangkuan/redis/redis.conf:/etc/redis/redis.conf -v /home/wangkuan/redis/data:/data -d redis:latest redis-server /etc/redis/redis.conf 
```

## emqx 安装
原命令
```shell
docker pull emqx/emqx:latest
```
现命令
```shell
docker pull docker.m.daocloud.io/library/emqx
```
```shell
docker run -d --name emqx -p 1883:1883 -p 8081:8081 -p 8083:8083 -p 8084:8084 -p 8883:8883 -p 18083:18083 emqx

docker run -d --name emqx -p 1883:1883 -p 8081:8081 -p 8083:8083 -p 8084:8084 -p 8883:8883 -
p 18083:18083 emqx/emqx:latest
```

# k8s
## Dashboard

### 安装Dashboard 

```shell
# 过以下URL下载推荐的部署文件
curl -LO "https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml"
# 应用YAML文件以部署Dashboard
kubectl apply -f recommended.yaml
# 验证Dashboard部署
kubectl get pods --namespace=kubernetes-dashboard
# kubectl命令行工具启动一个代理服务器
kubectl proxy
```

### 浏览器访问

```txt
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
```
### 获取令牌

```shell
kubectl -n kubernetes-dashboard create token admin-user
```
```txt
eyJhbGciOiJSUzI1NiIsImtpZCI6IkxFeUs4anFOaGY5RDZEQ281cVJiNW9kOXlVY0V4MHZIbVhJYzF1MFNvUncifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjLmNsdXN0ZXIubG9jYWwiXSwiZXhwIjoxNzIyNTkxMTMyLCJpYXQiOjE3MjI1ODc1MzIsImlzcyI6Imh0dHBzOi8va3ViZXJuZXRlcy5kZWZhdWx0LnN2Yy5jbHVzdGVyLmxvY2FsIiwia3ViZXJuZXRlcy5pbyI6eyJuYW1lc3BhY2UiOiJrdWJlcm5ldGVzLWRhc2hib2FyZCIsInNlcnZpY2VhY2NvdW50Ijp7Im5hbWUiOiJhZG1pbi11c2VyIiwidWlkIjoiYmY4MjUwZDktZTE4MS00ZDYxLWFjMjItOGVmNDgzMDliZDgwIn19LCJuYmYiOjE3MjI1ODc1MzIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDprdWJlcm5ldGVzLWRhc2hib2FyZDphZG1pbi11c2VyIn0.vA3htnSGco-lWs-IkS2tLle-GEsclxXX7im9G7JauzAnwO6yMDylr3FV86RPMnAVmkli3VVlH0UqjQgtAnJDJc51upyB7ZTGg6TeRI2xsLFdtq-YsyoQd_51Kc6j-PjO57tVNfQPhYMDfBtStEX8vNs1DQcjIhs5Wnx1sI7tfcwN4O3MwJaLrQGLHDIeFFmCY9_CSZPParDzP0XxC1Caa2bOVoTOrsxYYWzhR5uhYYazjkLpy1Y698HoJk5B1_u5XmpyJDLL4RSABaRhUpZaVJ0bLR3oVwLoBh4ssPHLdLkqid8oXz-V6TMqWYDfnOOMLZctmPOpbeb60WDYRJ8wpw
```

# ELK

创建网络

```sh
docker network create -d bridge my_network
# 和elasticsearch使用同一个网络
```



## elasticsearch

```sh
docker pull elasticsearch:7.17.18
```

配置

```sh
# 将docker里的目录挂载到linux的/mydata目录中
# 修改/mydata就可以改掉docker里的
mkdir -p /home/wangkuan/elasticsearch/config
mkdir -p /home/wangkuan/elasticsearch/data

# es可以被远程任何机器访问
echo "http.host: 0.0.0.0" >/home/wangkuan/elasticsearch/config/elasticsearch.yml

# 递归更改权限，es需要访问
chmod -R 777 /home/wangkuan/elasticsearch/

```

启动

```sh
docker run --name elasticsearch -p 9200:9200 -p 9300:9300 \
-e  "discovery.type=single-node" \
-e ES_JAVA_OPTS="-Xms64m -Xmx512m" \
-v /home/wangkuan/elasticsearch/config/elasticsearch.yml:/usr/share/elasticsearch/config/elasticsearch.yml \
-v /home/wangkuan/elasticsearch/data:/usr/share/elasticsearch/data \
-v  /home/wangkuan/elasticsearch/plugins:/usr/share/elasticsearch/plugins \
-d elasticsearch:7.17.18 

说明：
# 9200是用户交互端口 9300是集群心跳端口
# -e指定是单阶段运行
# -e指定占用的内存大小，生产时可以设置32G

查看启动日志
docker logs elasticsearch

# 设置开机启动
docker update elasticsearch --restart=always

```

##  Logstash

拉取

```sh
docker pull logstash:7.17.18
```

启动

```sh
docker run -it \
    --name logstash \
    -p 9600:9600 \
    -p 5044:5044 \
    logstash:7.17.18
```





## kibana

拉取

```shell
docker pull kibana:7.17.18
```

配置

```tex
mkdir -p /home/wangkuan/kibana
vi /home/wangkuan/kibana/kibana.yml

# 主机地址，可以是ip,主机名
server.host: 0.0.0.0
# 提供服务的端口，监听端口
server.port: 5601
# 该 kibana 服务的名称，默认 your-hostname
server.name: "kibana"
server.shutdownTimeout: "5s"
 
#####----------elasticsearch相关----------#####
# kibana访问es服务器的URL,就可以有多个，以逗号","隔开  docker linux 的ip
elasticsearch.hosts: [ "http://172.25.149.28:9200" ]
monitoring.ui.container.elasticsearch.enabled: true
 
####----------日志相关----------#####
 
# kibana日志文件存储路径，默认stdout
logging.dest: stdout
 
# 此值为true时，禁止所有日志记录输出
# 默认false
logging.silent: false
 
# 此值为true时，禁止除错误消息之外的所有日志记录输出
# 默认false
logging.quiet: false
 
# 此值为true时，记录所有事件，包括系统使用信息和所有请求
# 默认false
logging.verbose: false
 
#####----------其他----------#####
 
# 系统和进程取样间隔，单位ms，最小值100ms
# 默认5000ms
ops.interval: 5000
# kibana web语言
# 默认en
i18n.locale: "zh-CN"

# elasticsearch.username: "kibana"
# elasticsearch.password: "123456"
```

启动

```shell
docker run -d \
--name kibana-7.17.18 \
--restart=always \
-p 5601:5601 \
-e TZ="Asia/Shanghai" \
-v /home/wangkuan/kibana/kibana.yml:/usr/share/kibana/config/kibana.yml \
kibana:7.17.18

```

重启es

```sh
docker restart es-7.17.18
```

