# xxim-server-ops
此项目为xxim服务端的运维工具，他可以帮助你快速的部署xxim服务端、对xxim服务端进行运维操作，而且可以提供你应用的运营操作。

- > 这是前后端分离的项目，xxim-server依赖此项目的服务端提供的http接口，读取服务器配置
- > 运行此项目需要一个etcd服务，用于存储配置信息，监听配置的变化进行服务端的重启
- > 此项目需要有联网权限，该项目会使用网络请求xxim提供的API接口，获取xxim-server的最新版本并下载 

# 操作流程

## 1. 依赖安装

- > 首次打开web端，需要先配置进行服务端配置，如redis、mysql、mongo、etcd、pulsar、minio等
- > 如果你没有准备，可以在依赖选项中配置安装。但是需要注意，这些依赖的安装需要联网，而且本项目只支持简单的安装，如果你需要高可用的依赖，需要自行安装

### 1.1 redis

- > redis是必须的，用做缓存、用户的token、消息的序号等
- > redis不支持集群模式 

### 1.2 mysql

- > mysql是必须的，有大量的数据需要存储

- > mysql只支持单机模式部署

### 1.3 mongo

- > mongo是必须的，用于存储用户的聊天记录

- > mongo只支持单机模式部署

### 1.4 etcd

- > etcd是必须的，用于服务的配置信息存储、服务的发现、用户长连接信息存储、用户订阅的会话等

- > etcd不支持集群模式

### 1.5 pulsar

- > pulsar不是必须的，发送消息在1000qps/s以上时，建议使用pulsar

- > 此项目不支持pulsar部署，需要自行部署pulsar

### 1.6 minio

- > minio不是必须的，用于存储用户上传的文件，你可以不选择使用minio，因为你可以选择oss/cos/kodo等第三方云存储

- > 此项目不支持minio部署，需要自行部署minio。

- > 如果你的业务没有特殊要求，建议你使用第三方云存储。在聊天数据中，我们上传的文件均支持加密，所以你不用担心数据泄露的问题。但第三方云存储的可靠性和安全性，我们无法保证，所以你需要自行选择

- > 如果你必须使用minio，请确保你的客户端可以直接访问到minio，注意：minio不只是提供给服务端访问，客户端也需要访问minio进行上传和下载。 

## 2. 服务的配置

- > 服务的配置信息存储在etcd中，当服务启动时，会从etcd中读取配置信息
- > 服务的配置信息包括：redis、mysql、mongo、etcd、pulsar、第三方推送(如极光/MobPush/腾讯推送)、日志、加密参数等

## 3. xxim服务端部署

- > 你可以在 docker/k8s 两种部署选择中选择一种

### 3.1 docker部署

- > docker部署允许你远程连接其他服务器上的docker，然后在该服务器上启动xxim-server的docker容器

- > 日志：会在当前目录下创建logs文件夹。如果产生问题，可以带上日志提交issue

### 3.2 k8s部署

- > k8s部署要求你提供一个k8s集群的yaml配置文件，然后在k8s集群中启动xxim-server的pod

- > 日志：会输出到stdout，请自行配置k8s的日志采集

## 4. 服务运行维护操作

### 4.1 服务监控

- > 打开web端，可以查看服务的运行状态是否健康。注意：服务的健康状态是通过etcd的健康状态来判断的

### 4.2 服务重启

- > 打开web端，可以重启服务，重启服务会重新读取etcd中的配置信息

### 4.3 服务升级

- > 打开web端，可以升级服务，升级服务会重新读取etcd中的配置信息，并且会下载最新的xxim-server的docker镜像

### 4.4 服务配置

- > 打开web端，可以配置服务，配置服务会将配置信息写入etcd中，并可以选择立即重启/稍后重启

### 4.5 服务日志

- > 打开web端，可以查看服务的日志

## 5. IM惺云平台

- > 如果没有接入，点击会跳转到惺云平台的接入页面。
- > 完成认证并在惺云平台创建App后，可以在此页面配置App的信息。
- > 配置完成后，可以在此页面查看App的信息。比如分享链接，二维码等。

## 6. 应用操作

### 6.0 我的

#### 6.0.1 设置

##### 6.0.1.1 个人信息

- 头像
- 昵称
- 手机号
- 邮箱

##### 6.0.1.2 安全设置

- 密码
- 二级密码

##### 6.0.1.3 通知设置

- 消息通知开关

#### 6.0.2 退出

- 退出登录

#### 6.0.3 我的消息

- 1、可以查看系统推送过来的消息，可以查看消息的详细信息，可以已读消息

### 6.1 应用统计

#### 6.1.1 数量
- 总用户数
- 活跃用户数
- 总消息数
- 总群组数
- 活跃群聊数
- 帖子数

#### 6.1.2 留存

- 一日留存
- 三日留存
- 七日留存
- 30日留存

### 6.2 后台权限

- 后台配置（IP白名单模式、消息推送）
- 菜单管理（配置后台的菜单，添加/删除/显示/隐藏）
- API管理（配置后台的API，是否记录日志/是否属于高危操作）
- 角色管理（配置后台的角色，角色绑定多个菜单和API）
- 管理员管理（配置后台的管理员，管理员需要绑定角色）
- IP白名单管理（配置后台的IP白名单）
- 操作日志 （查看后台管理员的操作日志）
- 登录日志 （查看后台管理员的登录日志）
- 系统消息（查看所有的系统消息，查看各管理员已读统计）

### 6.3 应用配置

#### 6.3.1 线路配置

- 1、配置应用的线路，你可能会使用动态CDN，所以你需要配置多个线路，当某个线路不可用时，应用会自动切换到其他线路
- 2、将线路信息保存为json格式并加密，然后上传到你指定的对象存储服务中，客户端读取该公链信息，然后解密，得到线路信息

#### 6.3.2 功能配置

- 1、安全配置：白名单模式
- 2、设备平台配置：允许在哪些平台登录；允许在哪些平台注册；允许在哪些平台创建群聊；允许在哪些平台加好友；
- 3、消息的安全配置：文本消息违禁词检测策略；图片消息违禁词检测策略；视频消息违禁词检测策略；
- 4、注册选项：是否必填头像/昵称/性别/邀请码/手机号；默认头像策略；默认昵称策略；
- 5、登录选项：密码错误上限；登录失败上限；登录失败锁定时间；
- 6、群聊选项：默认群头像策略；群人数上限；
- 7、加好友配置：
- x、自定义配置：你可以添加自定义配置，客户端可以读取该配置信息进行自定义操作；

#### 6.3.3 应用版本更新

- 1、你可以配置应用的版本信息，客户端会读取该信息，然后提示用户更新

#### 6.3.4 违禁词管理

- 1、你可以配置应用的违禁词，客户端会读取该信息，然后对消息进行违禁词检测

#### 6.3.5 公告管理

- 1、你可以配置应用的公告，客户端每次冷启动时，会读取该信息，然后展示给用户

### 6.4 用户管理

#### 6.4.1 用户权限

- 1、你可以配置 普通用户/游客/管理员 的默认权限，比如登录、注册、加好友、加群、退群、发消息、发评论、发帖子、分享应用等

#### 6.4.2 用户列表

- 1、你可以查看用户列表，可以查看用户的详细信息，可以禁用用户，可以单独修改用户的权限，可以修改用户角色，可以管理用户的好友，可以管理用户的群组

#### 6.4.3 在线用户管理

- 1、你可以查看当前在线的用户，可以踢出用户下线

#### 6.4.4 IP黑名单管理

- 1、你可以查看当前的IP黑名单，可以添加IP黑名单，可以删除IP黑名单

#### 6.4.5 IP白名单管理

- 1、你可以查看当前的IP白名单，可以添加IP白名单，可以删除IP白名单

#### 6.4.6 邀请码管理

> 邀请码可以配置用户创建，也可以后台手动创建 

- 1、你可以查看邀请码列表，可以添加邀请码，可以删除邀请码
- 2、你可以查看邀请码的使用情况，可以查看邀请码的使用记录

#### 6.4.7 登录记录管理

- 1、你可以查看用户的登录记录

#### 6.4.8 僵尸号管理

- 1、僵尸号的生成策略管理
- 2、僵尸号的列表管理

#### 6.4.9 通知号管理

> 系统默认通知号全员默认订阅，但如果用户设置不接收通知，则不会收到通知。比如天气预报。

> 通知号允许开发者注册，但需要审核，审核通过后，才能使用。凭借通知号，开发者可以给订阅者发送通知。默认无人订阅。

- 1、查看通知号列表，系统内置通知号不允许删除

### 6.5 群聊管理

#### 6.5.1 群聊列表

- 1、你可以查看群聊列表，可以查看群聊的详细信息，可以禁用群聊，可以管理群聊的成员，可以批量添加/清空僵尸号

### 6.6 UGC管理

#### 6.6.1 帖子管理

- 1、你可以查看帖子列表，可以查看帖子的详细信息，可以禁用帖子，可以管理帖子的评论

#### 6.6.2 话题管理

- 1、你可以查看话题列表，可以查看话题的详细信息，可以禁用话题，可以管理话题的帖子

### 6.7 运营管理

#### 6.7.1 系统消息

> 消息推送使用系统内置的"系统通知"通知号，全员默认订阅，但如果用户设置不接收通知，则不会收到通知。

> 消息推送的内容可以是文本、图片、视频、链接、富文本等

- 1、查看推送记录
- 2、推送/编辑消息

#### 6.7.2 推广渠道

> 推广渠道是为了统计用户的来源，比如用户是通过哪个渠道下载的APP，或者用户是通过哪个渠道注册的账号

- 1、推广渠道管理
