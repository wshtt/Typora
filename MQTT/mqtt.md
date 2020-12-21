# MQTT



[toc]

Message Queuing Telemetry Transport 消息 队列 遥测 传输

### 一、简介 

MQTT是一个客户端服务端架构的发布/订阅模式的消息传输协议。它的设计思想是轻巧、开放、简单、规范，易于实现。这些特点使得它对很多场景来说都是很好的选择，特别是对于受限的环境如机器与机器的通信（M2M）以及物联网环境（IoT）。

MQTT协议是物联网常用的应用层协议，运行在TCP/IP中的应用层中，依赖TCP协议

- 客户端使用它连接服务端。
- 它提供有序的、可靠的、双向字节流传输。

### 二、名词解释

###### 客户端 Client
使用MQTT的程序或设备。客户端总是通过网络连接到服务端。它可以

- 发布应用消息给其它相关的客户端。
- 订阅以请求接受相关的应用消息。
- 取消订阅以移除接受应用消息的请求。
- 从服务端断开连接。

###### 服务端 Server
一个程序或设备，作为发送消息的客户端和请求订阅的客户端之间的中介。服务端
- 接受来自客户端的网络连接。
- 接受客户端发布的应用消息。
- 处理客户端的订阅和取消订阅请求。
- 转发应用消息给符合条件的已订阅客户端。

###### 会话 Session

- 客户端和服务端之间的状态交互。

###### 订阅 Subscription
- 订阅包含一个主题过滤器（Topic Filter）和一个最大的服务质量（QoS）等级。订阅与单个会话（Session）关联。会话可以包含多于一个的订阅。会话的每个订阅都有一个不同的主题过滤器。

###### 主题名 Topic Name
- 附加在应用消息上的一个标签，服务端已知且与订阅匹配。服务端发送应用消息的一个副本给每一个匹配的客户端订阅。一个主题可以有多个级别，各个级别之间用斜杠字符分隔。

###### 主题过滤器 Topic Filter
- 订阅中包含的一个表达式，用于表示相关的一个或多个主题。主题过滤器可以使用通配符。

###### 控制报文 MQTT Control Packet
- 通过网络连接发送的信息数据包。MQTT规范定义了十四种不同类型的控制报文，其中一个（PUBLISH报文）用于传输应用消息。

### 三、 报文结构

###### Fixed header 固定报头，所有控制报文都包含

>固定报头格式
>![db1ae540](F:\GitFile\Typora\MQTT\Untitled.assets\db1ae540.png)

###### Variable header 可变报头，部分控制报文包含

固定报头7-4位表示控制报文类型
>控制报文类型
>![ae6aa2fc](F:\GitFile\Typora\MQTT\Untitled.assets\ae6aa2fc.png)

3-0包含每个MQTT控制报文类型特定的标志，表中任何标记为“保留”的标志位，都是保留给以后使用的，必须设置为表格中列出的值。如果收到非法的标志，接收者必须关闭网络连接。
>![d4ff2686](F:\GitFile\Typora\MQTT\Untitled.assets\d4ff2686.png)

>从第2个字节开始。
>剩余长度（Remaining Length）表示当前报文剩余部分的字节数，包括可变报头和负载的数
>据。剩余长度不包括用于编码剩余长度字段本身的字节数。

###### Payload 有效载荷，部分控制报文包含

### 四、 报文详解
#### CONNECT – 连接服务端
>客户端链接服务端，第一次必须先发送一个connect报文确定链接。
>一次链接只能发送一次connect报文，第二次会断开链接。
##### 报文格式
###### 固定报头

>![d35c00b6](F:\GitFile\Typora\MQTT\Untitled.assets\d35c00b6.png)
###### 可变报头 10字节
>包含1. 协议名 2. 协议及别 3. 连接标志 4. 保持连接
>1. 协议名
>![52abdec4](F:\GitFile\Typora\MQTT\Untitled.assets\52abdec4.png))
>2. 协议级别
>![2ddcef28](F:\GitFile\Typora\MQTT\Untitled.assets\2ddcef28.png)
>3. 连接标志
>4. 保持连接，byte 9 和 10 ，发送报文最长时间间隔。
##### 有效载荷
>CONNECT报文的有效载荷（payload）包含一个或多个以长度为前缀的字段，可变报头中的标志决定是否包含这些字段。如果包含的话，必须按这个顺序出现：客户端标识符，遗嘱主题，遗嘱消息，用户名，密码。

#### CONNACK – 确认连接请求
服务端发送CONNACK报文响应对应客户端收到的CONNECT报文。服务端发送给客户端的第一个报文必须是CONNACK。
##### 报文格式
>

#### PUBLISH - 发布消息
>###### 固定报头
>![bb290352](F:\GitFile\Typora\MQTT\Untitled.assets\bb290352.png)
>###### QoS
>![9dd3b564](F:\GitFile\Typora\MQTT\Untitled.assets\9dd3b564.png)
>如果客户端发给服务端的PUBLISH报文的保留（RETAIN）标志被设置为1，服务端必须存储这个应用消息和它的服务质量等级（QoS），
>
>###### 可变报头
>包含主题名和报文标识符
>1. 主题名 
>UTF-8字符串，# + 通配符
>$SYS/ 这种与普通订阅不会匹配同一个通配符，需要单独指定
>2. 报文标识符
>只有当QoS等级是1或2时，报文标识符（Packet Identifier）字段才能出现在PUBLISH报文中。
>
>###### 有效载荷
#### PUBACK –发布确认
>PUBACK报文是对QoS 1等级的PUBLISH报文的响应。
>![ef615b2f](F:\GitFile\Typora\MQTT\Untitled.assets\ef615b2f.png)
>
>无有效载荷