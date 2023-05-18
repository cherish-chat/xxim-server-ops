# xxim-server-ops
xxim服务端的运维工具

> 这是前后端分离的项目，xxim-server依赖此项目的服务端提供的http接口，读取服务器配置

> 运行此项目需要一个etcd服务，用于存储配置信息，监听配置的变化进行服务端的重启

> 此项目需要有联网权限，该项目会使用网络请求xxim提供的API接口，获取xxim-server的最新版本并下载 

# 操作流程

## 1. 依赖安装

- 首次打开web端，需要先配置进行服务端配置，如redis、mysql、mongo、etcd、pulsar等

- 如果你没有准备，可以在依赖选项中配置安装。但是需要注意，这些依赖的安装需要联网，而且本项目只支持简单的安装，如果你需要高可用的依赖，需要自行安装

### 1.1 redis

> redis是必须的，用做缓存、用户的token、消息的序号等

> redis不支持集群模式 

### 1.2 mysql

> mysql是必须的，有大量的数据需要存储

> mysql只支持单机模式部署

### 1.3 mongo

> mongo是必须的，用于存储用户的聊天记录

> mongo只支持单机模式部署

### 1.4 etcd

> etcd是必须的，用于服务的配置信息存储、服务的发现、用户长连接信息存储、用户订阅的会话等

> etcd不支持集群模式

### 1.5 pulsar

> pulsar不是必须的，发送消息在1000qps/s以上时，建议使用pulsar

> 此项目不支持pulsar部署，需要自行部署pulsar

## 2. 服务的配置

- 服务的配置信息存储在etcd中，当服务启动时，会从etcd中读取配置信息
- 服务的配置信息包括：redis、mysql、mongo、etcd、pulsar、第三方推送(如极光/MobPush/腾讯推送)、日志、加密参数等

## 3. xxim服务端部署

- 你可以在 docker/k8s 两种部署选择中选择一种

### 3.1 docker部署

> docker部署允许你远程连接其他服务器上的docker，然后在该服务器上启动xxim-server的docker容器

> 日志：会在当前目录下创建logs文件夹。如果产生问题，可以带上日志提交issue

### 3.2 k8s部署

> k8s部署要求你提供一个k8s集群的yaml配置文件，然后在k8s集群中启动xxim-server的pod

> 日志：会输出到stdout，请自行配置k8s的日志采集
