# docker 容器挂载宿主机目录

[TOC]



### 一、正常写法与参数解析

启动docker 容器时，使用 `-v` 参数指定要挂载的宿主机目录 

容器启动后，容器内自动创建`/soft` 目录，

```shell
docker run -it -v /test:/soft centos /bin/bash

# /test 宿主机目录， linux 以/开头的为绝对路径，相当于C:/
# /soft docker容器的目录，绝对路径，在容器的根目录/下
# centos 容器名称
# /bin/bash
```



### 二、注意

##### 1. 容器目录`/soft`只能写绝对路径

```
如果容器目录 写成soft ，没有写/ ，则会报错
```

##### 2.宿主机目录如果不存在，则会自动创建

```
如果/test 不存在宿主机根目录下，则会创建此目录
```

##### 3.宿主机目录写成相对路径，忘记写`/`，则会创建到其他地方

```
宿主机目录： /var/lib/docker/volumes/test/
```

##### 4.如果只写了一个路径，则写的为容器路径

```
docker run -it -v /test centos /bin/bash
容器路径为写的路径
宿主机路径为：  /var/lib/docker/volumes/xxxx..../ 随机命名的一个 
```

##### 5.如果修改了容器目录的属主和属组，对应的宿主机挂载目录也会更改

```
更改的属主只会根据 user 的UID 来更改
```

##### 6.销毁容器的时候，宿主机挂载目录的一切都会保留

```
容器销毁只是取消了挂载关系，不会对宿主机目录进行操作了
```

##### 7.

```
挂载宿主机已存在目录后，在容器内对其进行操作，报“Permission denied”。

一般是允许在容器内对其进行操作。如果报以上错误，可通过两种方式解决：

1> 关闭selinux。

临时关闭：# setenforce 0

永久关闭：修改/etc/sysconfig/selinux文件，将SELINUX的值设置为disabled。

2> 以特权方式启动容器 

指定--privileged参数

如：# docker run -it --privileged=true -v /test:/soft centos /bin/bash
```

**`docker inspect`命令查看宿主机的挂载目录**





```shell
docker run -d -p 8301:8301 -v /data/BOOK:/data qianji-cloud/qianji-oss-server /bin/bash --name qianjicloud20_qianji-oss_1

```

