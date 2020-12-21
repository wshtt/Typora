# EMQ

[toc]



##### 简介

*EMQ X* (Erlang/Enterprise/Elastic MQTT Broker) 是基于 Erlang/OTP 平台开发的开源物联网 MQTT 消息服务器。

Erlang/OTP是出色的软实时 (Soft-Realtime)、低延时 (Low-Latency)、分布式 (Distributed)的语言平台。

MQTT 是轻量的 (Lightweight)、发布订阅模式 (PubSub) 的物联网消息协议。

EMQ X 设计目标是实现高可靠，并支持承载海量物联网终端的MQTT连接，支持在海量物联网设备间低延时消息路由:

1. 稳定承载大规模的 MQTT 客户端连接，单服务器节点支持50万到100万连接。
2. 分布式节点集群，快速低延时的消息路由，单集群支持1000万规模的路由。
3. 消息服务器内扩展，支持定制多种认证方式、高效存储消息到后端数据库。
4. 完整物联网协议支持，MQTT、MQTT-SN、CoAP、LwM2M、WebSocket 或私有协议支持。

EMQ X Broker 在全球物联网市场广泛应用。无论是产品原型设计、物联网创业公司、还是大规模的商业部署，EMQ X Broker 都支持开源免费使用。

[官方文档](https://docs.emqx.net/broker/latest/cn/)

### 使用

#### Centos 包管理器安装

1. 安装依赖包

```shell
[root@centOS7 ~]# yum install -y yum-utils device-mapper-persistent-data lvm2
```

2. 设置稳定版本库 centos 7

```shell
[root@centOS7 ~]# yum-config-manager --add-repo https://repos.emqx.io/emqx-ce/redhat/centos/7/emqx-ce.repo
```

3. 安装最新版本的 EMQ X Broker

```shell
yum install emqx
```

4. 安装特定版本的 EMQ X Broker

```shell
# 1.  查询可用版本
[root@centOS7 ~]# yum list emqx --showduplicates | sort -r

已加载插件：fastestmirror, langpacks
可安装的软件包
 * updates: mirrors.aliyun.com
Loading mirror speeds from cached hostfile
 * extras: mirrors.aliyun.com
emqx.x86_64                      4.2.2-1.el7                      emqx-ce-stable
emqx.x86_64                      4.2.1-1.el7                      emqx-ce-stable
emqx.x86_64                      4.2.0-1.el7                      emqx-ce-stable

# 2. 根据第二列中的版本字符串安装特定版本，例如 4.2.2
[root@centOS7 ~]# sudo yum install emqx-4.2.2

```

#### 启动

```shell
# 1. 直接启动
$ emqx start
emqx 4.0.0 is started successfully!

# 2. systemctl
$ sudo systemctl start emqx

# 3. service 启动
$ sudo service emqx start

```

#### 停止

```shell
$ emqx stop
ok
```

#### 重启

```shell
emqx restart
```

#### 卸载

```shell
$ sudo yum remove emqx
```

#### 查看状态

```shell
# 异常
$ emqx_ctl status
Node 'emqx@127.0.0.1' is started
emqx 4.0.0 is running

# 正常
$ emqx_ctl status
Node 'emqx@127.0.0.1' not responding to pings。
```

#### 使用控制台启动EMQ X

```shell
emqx console
```

#### Ping EMQ X Broker。

````shell
emqx ping
````





### 目录结构

![image-20201113143657461](F:\GitFile\Typora\MQTT\EMQ.assets\image-20201113143657461.png)

##### bin 目录

**emqx、emqx.cmd**

EMQ X 的可执行文件

**emqx_ctl、emqx_ctl.cmd**

EMQ X 管理命令的可执行文件

#####  etc 目录

![image-20201113143836670](F:\GitFile\Typora\MQTT\EMQ.assets\image-20201113143836670.png)

#####  data 目录

**configs/app.\*.config**

EMQ X 读取 `etc/emqx.conf` 和 `etc/plugins/*.conf` 中的配置后，转换为 Erlang 原生配置文件格式，并在运行时读取其中的配置。

**loaded_plugins**

`loaded_plugins` 文件记录了 EMQ X 默认启动的插件列表，可以修改此文件以增删默认启动的插件。`loaded_plugins` 中启动项格式为 `{<Plugin Name>, <Enabled>}.`，`<Enabled>` 字段为布尔类型，EMQ X 会在启动时根据 `<Enabled>` 的值判断是否需要启动该插件。

```json
{emqx_management,true}.
{emqx_recon,true}.
{emqx_retainer,true}.
{emqx_dashboard,true}.
{emqx_rule_engine,true}.
{emqx_bridge_mqtt,false}.
```

**mnesia**

Mnesia 数据库是 Erlang 内置的一个分布式 DBMS，可以直接存储 Erlang 的各种数据结构。

#####  log 目录

**emqx.log.\***

EMQ X 运行时产生的日志文件

**crash.dump**

EMQ X 的崩溃转储文件，可以通过 `etc/emqx.conf` 修改配置

**erlang.log.\***

以 `emqx start` 方式后台启动 EMQ X 时，控制台日志的副本文件。

### Dashboard

EMQ X 提供了 Dashboard 以方便用户管理设备与监控相关指标。通过 Dashboard，你可以查看服务器基本信息、负载情况和统计数据，可以查看某个客户端的连接状态等信息甚至断开其连接，也可以动态加载和卸载指定插件。除此之外，EMQ X Dashboard 还提供了规则引擎的可视化操作界面，同时集成了一个简易的 MQTT 客户端工具供用户测试使用。

EMQ X Dashboard 功能由 `emqx-dashboard` 插件实现，该插件默认处于启用状态

`http://localhost:18083/` 来查看你的 Dashboard，默认用户名是 `admin`，密码是 `public`。



