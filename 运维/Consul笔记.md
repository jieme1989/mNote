## Consul基本用法

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



## Consul开启登录ACL

### 步骤1：添加consul配置文件，开启ACL。

创建config/dir目录，将以下配置文件放在此目录下，命名为acl.json

```json
{
    "acl": {
        "enabled": true,        //是否开启ACLToken
        "default_policy": "deny",  //allow和deny两个值。allow模式下，ACL是黑名单，允许任何未明确禁止的操作。deny模式下，ACL是白名单，阻止任何未明确允许的操作。
		"enable_token_persistence": true //值为true时，API使用的令牌集合将被保存到磁盘，并且当代理重新启动时会重新加载。
    }
}
```



```sh
启动consul

启动命令：

consul.exe agent -server -bootstrap -advertise 127.0.0.1 -data-dir ./data -ui -config-dir ./config/dir/acl.json
```



### 步骤2：获取bootstrap token

```sh
consul acl bootstrap
```

```sh
AccessorID:       b561dae8-0ec5-bdc5-2ed6-28d4350ca82d
# token的内容
SecretID:         ea9f39c6-8e4c-e8ed-30fb-6accff325ad3
#描述
Description:      Bootstrap Token (Global Management)
Local:            false
Create Time:      2025-01-23 15:00:08.9862145 +0800 CST
# 使用的全局策略，权限很大，类似mysql的root
Policies:
   00000000-0000-0000-0000-000000000001 - global-management
```

### 步骤3：代码修改

#### Program.cs

```c#
if (oConsulConfigurationAppsetting != null)
{
    foreach (var itemSetting in oConsulConfigurationAppsetting.DictionaySettingFiles)
    {
        builder.Configuration.AddConsul(itemSetting.Value, op =>
        {
            op.ConsulConfigurationOptions = cco =>
            {
                cco.Address = new Uri(oConsulConfigurationAppsetting.Server);
                //添加Token
                cco.Token = "ea9f39c6-8e4c-e8ed-30fb-6accff325ad3";
            };
            op.ReloadOnChange = true;
        });
    }
}
```

### ConsulConfig.cs

```c#
ConsulClient client = new ConsulClient(c =>
{
    c.Address = new Uri(config.Address);
    c.Datacenter = config.DataCenter;
    //添加Token
    c.Token = "ea9f39c6-8e4c-e8ed-30fb-6accff325ad3";
});
```

