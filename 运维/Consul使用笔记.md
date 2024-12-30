## Consul使用笔记-------杨华-----20230327-------

### 在路径上输入cmd，

####  打开 命令提示符 输入

1）开发模式。-dev 数据会基于内存，重启后注册的服务都会消失。
consul.exe agent -dev；启动consul
2）生产模式。-client -server 数据会持久化
consul.exe agent -client -server；启动consul

### 持久化启动方式：

call consul agent -server -bootstrap-expect=1 -ui -bind=127.0.0.1 -client=0.0.0.0 -data-dir=D:/appdata/Consul/data/ -config-dir=D:/appdata/Consul/dir/



call consul agent -server -bootstrap-expect=1 -ui -bind=127.0.0.1 -client=0.0.0.0 -data-dir=D:/CastcZhy.Products/1.consul/config/ -config-dir=D:/CastcZhy.Products/1.consul/config/dir/


### 访问路径：

http://127.0.0.1:8500/ui/dc1/kv/Consul/



### 查看网卡信息

ipconfig 

#-dev -bind  设定绑定网卡，直接就启动服务了
#通过agent命令，来启动consul 服务


### key/value 导出

consul kv export --http-addr=http://127.0.0.1:8500  >consul_kv.json

### key/value 导入

consul kv import --http-addr=http://127.0.0.1:8500 @consul_kv.json