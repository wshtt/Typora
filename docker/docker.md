# docker



[toc]

https://www.docker.org.cn/

https://www.widuu.com/docker/installation/windows.html



#### 命令

##### 容器

###### docker version

返回当前安装的 Docker 客户端和进程的版本信息，GO 语言的版本信息( Docker的编程语言 )。

###### docker start

```shell
# 启动容器

docker start [OPTIONS] CONTAINER [CONTAINER...]
Options:
  -a, --attach                  连接STDOUT / STDERR和转发信号
      --checkpoint string       Restore from this checkpoint
      --checkpoint-dir string   Use a custom checkpoint storage directory
      --detach-keys string      覆盖分离容器的键序列
  -i, --interactive             附加容器的STDIN
```

###### docker restart

```shell
# 重启容器

docker restart [OPTIONS] CONTAINER [CONTAINER...]
Options:
  -t, --time int   关闭容器的时限，超过时限强行关闭 (默认 10s)

```

###### docker stop

```shell
# 停止运行容器

docker stop [OPTIONS] CONTAINER [CONTAINER...]
Options:
  -t, --time int   关闭容器的时限，超过时限强行关闭 (默认 10s)

```

###### docker logs

```shell
# 查看容器日志 CONTAINER container 容器

docker logs [OPTIONS] CONTAINER
Options:
      --details        显示提供给日志的额外细节
  -f, --follow         跟踪日志输出
      --since string   显示某个开始时间的所有日志（--since="2016-07-01"）
      --tail string    仅列出最新N条容器日志 (默认 "all")
  -t, --timestamps     显示时间戳
      --until string   Show logs before a timestamp (e.g. 2013-01-02T13:23:37) or relative (e.g. 42m for 42 minutes)
```

###### docker ps

```shell
# 查看运行中的docker 容器
docker ps

docker ps [OPTIONS]
Options:
  -a, --all             显示所有容器（包括未运行的）
  -f, --filter filter   根据条件过滤显示内容
      --format string   返回指定格式的模板
  -n, --last int        列出最近创建的 n个容器
  -l, --latest          显示最近创建的容器
      --no-trunc        不截断输出
  -q, --quiet           安静模式，只显示容器id
  -s, --size            显示文件大小
```

###### docker run

```shell
# 
docker run --help

# 启动容器
docker run --publish 8000:8080 --detach --name bb bulletinboard:1.0
# /bin/bash 启动bash shell
docker run -t -i ubuntu:14.04 /bin/bash

docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
Options:
  -d, --detach                         后台运行容器并返回容器id
  -i, --interactive                    以交互模式运行容器，通常与-t同时使用
  -p, --publish list                   指点端口映射，格式为：主机端口：容器端口
  -P, --publish-all                    随机端口映射，容器内部端口映射到主机端口
  -t, --tty                            为容器重新分配一个伪输入终端
      --name string                    为容器指定名称
  -m, --memory bytes                   设置容器使用内存最大值
  -v, --volume list                    绑定一个卷，如 -v /test:/soft
      --expose list                    开放一个或一组端口
```

###### docker rm

```shell
# 移除容器
docker rm [OPTIONS] CONTAINER [CONTAINER...]
Options:
  -f, --force     通过 SIGKILL 信号强制删除一个运行中的容器。
  -l, --link      移除容器间的网络连接，而非容器本身。
  -v, --volumes   删除与容器关联的卷
```

###### docker pull

```shell
# 从镜像仓库中拉取或者更新指定镜像
docker pull [OPTIONS] NAME[:TAG|@DIGEST]
Options:
  -a, --all-tags                拉取所有 tagged 镜像
      --disable-content-trust   忽略镜像的校验 (默认是开启的)
      --platform string         Set platform if server is multi-platform capable
  -q, --quiet                   Suppress verbose output

```

###### docker search

```shell
# 从 Docker Hub 查找镜像
docker search [OPTIONS] TERM
Options:
  -f, --filter filter   根基条件筛选
      --format string   以标准格式输出
      --limit int       显示最大条数 (默认 25)
      --no-trunc        完整镜像描述

```

###### docker top

```shell
# 显示容器中的进程
docker top CONTAINER [ps OPTIONS]
```

###### docker inspect

```shell
# 返回容器的 IP 地址
docker inspect -f '{{ .NetworkSettings.IPAddress }}' 67875251e910

# 获取容器/镜像的元数据
docker inspect [OPTIONS] NAME|ID [NAME|ID...]
Options:
  -f, --format string   指定返回值的模板文件。
  -s, --size            显示总的文件大小。
      --type string     返回json类型
```

###### docker port

```shell
# 返回容器端口与主机端口映射
docker port CONTAINER [PRIVATE_PORT[/PROTO]]

# 直接跟容器id
docker port 67875251e910
5000/tcp -> 0.0.0.0:8000 # 容器端口->主机端口

# 容器id + 容器端口
docker port 67875251e910 5000
0.0.0.0:8000 # 主机端口
```

###### docker attach

```shell
# 连接到正在运行中的容器。
docker attach [OPTIONS] CONTAINER

# 进入容器并且可以 CTRL+C 退出
docker attach --sig-proxy=false mynginx

Options:
      --detach-keys string   Override the key sequence for detaching a container（重写分离容器的键序列）
      --help                 输出使用说明
      --no-stdin             不连接 STDIN
      --sig-proxy            Proxy all received signals to the process (default true)（将所有接收到的信号代理给进程(默认为true)）

```

##### 镜像

###### docker images

```shell
# 查看本地所有镜像（过滤中间层镜像）
$ sudo docker images

docker images [OPTIONS] [REPOSITORY[:TAG]]
Options:
  -a, --all             列出本地所有镜像（包括中间层镜像）
      --digests         显示摘要信息
  -f, --filter filter   显示满足条件的镜像
      --format string   指定返回值的模板文件
      --no-trunc        显示完整镜像信息
  -q, --quiet           只显示镜像id
```

###### docker rmi

```shell
# 移除本机镜像
docker rmi
```

###### docker tag

```shell
# 给镜像重新命名
docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]

docker tag 5db5f8471261 ouruser/sinatra:devel
```



##### Dockerfile

制作Docker image 有两种方式：一是使用 Docker container，直接构建容器，再导出成 image 使用；二是使用 Dockerfile，将所有动作写在文件中，再 build 成 image。

###### **Dockerfile基本组成**

```SHELL
#第一行必须指令基于的基础镜像
From ubutu

#维护者信息
MAINTAINER docker_user  docker_user@mail.com

#镜像的操作指令
apt/sourcelist.list

RUN apt-get update && apt-get install -y ngnix 
RUN echo "\ndaemon off;">>/etc/ngnix/nignix.conf

#容器启动时执行指令
CMD /usr/sbin/ngnix

```

###### 创建镜像

将要使用的包和**Dockerfile**文件放在一个目录中

###### **Dockerfile**文件中指令

1. `FROM`：镜像基础信息: 指定基础镜像，在哪个镜像建立

DockerFile第一条必须为From指令。如果同一个DockerFile创建多个镜像时，可使用多个From指令（每个镜像一次）

```shell
FROM <image> 或FROM <image>:<tag>
```

2. `MAINTAINER`：镜像维护者信息

```shell
MAINTAINER <name>
```

3. `RUN`：在镜像中要执行的命令

```shell
RUN <command> 或 RUN ["executable", "param1", "param2"]

# 前者在shell终端上运行，即/bin/sh -C，后者使用exec运行。例如：RUN [“/bin/bash”, “-c”,”echo hello”]
# 每条run指令在当前基础镜像执行，并且提交新镜像。
```

4. `CMD`：启动容器时执行的命令

```shell
CMD [“executable” ,”Param1”, “param2”]使用exec执行，推荐
CMD command param1 param2，在/bin/sh上执行
CMD [“Param1”, “param2”] 提供给ENTRYPOINT做默认参数。

# 指定启动容器时执行的命令，每个 Dockerfile 只能有一条 CMD 命令。如果指定了多条命令，只有最后一条会被执行。如果用户启动容器时候指定了运行的命令，则会覆盖掉 CMD 指定的命令。
```

5. `EXPOSE`

```shell
EXPOSE [...]
EXPOSE 8080 8090 5000

# 告诉Docker服务端容器暴露的端口号，供互联系统使用。p在启动Docker时，可以通过-P,主机会自动分配一个端口号转发到指定的端口。使用-p，则可以具体指定哪个本地端口映射过来
```

6. `ENV`

```shell
 ENV <key> <value>
 ENV PATH /usr/local/nginx/sbin:$PATH
 
 # 指定一个环境变量，会被后续 RUN 指令使用，并在容器运行时保持。
```

7. `ADD`

```shell
ADD <src> <dest> # 本地路径 容器路径
			
# 相当于 COPY，但是比 COPY 功能更强大
# 该命令将复制指定的目录文件到容器中的 。 其中目录文件可以是Dockerfile所在目录的一个相对路径；也可以是一个URL；还可以是一个 tar 文件，复制进容器会自动解压。
```

8. `COPY`

```shell
COPY package.json .

# 复制本地主机的 （为Dockerfile所在目录的相对路径）到容器中的 。
# 当使用本地目录为源目录时，推荐使用 COPY 。
```

9. `ENTRYPOINT`

```shell
ENTRYPOINT [“executable”, “param1”, “param2”]
ENTRYPOINT command param1 param2 （shell中执行）。
# 配置容器启动后执行的命令，并且不可被 docker run 提供的参数覆盖。
# 每个Dockerfile中只能有一个 ENTRYPOINT ，当指定多个时，只有最后一个起效。
```

10. `VOLUME`

```shell
VOLUME [“/data”] 。

# 创建一个可以从本地主机或其他容器挂载的挂载点，一般用来存放数据库和需要保持的数据等。
```

11. `USER`

```shell
USER daemon 。

# 指定运行容器时的用户名或UID，后续的 RUN 也会使用指定用户。
# 当服务不需要管理员权限时，可以通过该命令指定运行用户。并且可以在之前创建所需要的用户，例如： RUN groupadd -r postgres && useradd -r -g postgres postgres 。要临时获取管理员权限可以使用 gosu ，而不推荐 sudo 。
```

12. `WORKDIR`

```shell
# 切换工作目录，相当于 cd

WORKDIR /path/to/workdir

# 为后续的 RUN 、 CMD 、 ENTRYPOINT 指令配置工作目录。
# 可以使用多个 WORKDIR 指令，后续命令如果参数是相对路径，则会基于之前命令指定的路径。例如
```

13. `ONBUILD`

```shell
ONBUILD [INSTRUCTION]

# 在构建本镜像时不生效，在基于此镜像构建镜像时生效
# 配置当所创建的镜像作为其它新创建镜像的基础镜像时，所执行的操作指令。
```

ENTRYPOINT 和 CMD 的区别：ENTRYPOINT 指定了该镜像启动时的入口，CMD 则指定了容器启动时的命令，当两者共用时，完整的启动命令像是 ENTRYPOINT + CMD 这样。

### 容器

###### 拉取测试案例

```shell
# 下载文件
curl -LO https://github.com/dockersamples/node-bulletin-board/archive/master.zip
# zip 解压
unzip master.zip
# 进入目录
cd node-bulletin-board-master/bulletin-board-app
```

###### 创建镜像

```shell
docker build --tag bulletinboard:1.0 .
```

###### 运行

```shell
docker run --publish 8000:8080 --detach --name bb bulletinboard:1.0
```

- `--publish` :将主机端口8000转发到容器端口8080。容器有私有的端口，如果不匹配，默认阻止所有主机端口转发
- `--detach`：要求docker 在后台运行这个容器
- `--name`：指定一个名称，后续操作可是使用此名称操作容器

直接访问主机的 `8000`端口，就可以访问此程序

###### 删除

```shell
# 删除运行中容器 --force 用来停止容器
docker rm --force bb

# 停止容器
docker stop bb
```

##### Docker Hub

```json
name: 2656439669
email: 2656439669@qq.com
password: 2656439669
```

1. 登录

```shell
$ sudo docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: 2656439669
Password: 
WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
```

2. 查看本地镜像REPOSITORY 是不是docker hub ID/远程仓库名。如果不是，则修改

```shell
docker tag 镜像id 
```



2. 将镜像名称设置为`2656439669`/`仓库名`:标签。这样才可以上传到DockerHub

```shell
# 在 /root/node-bulletin-board-master/bulletin-board-app 目录下执行
$ sudo docker tag bulletinboard:1.0 <Your Docker ID>/bulletinboard:1.0
$ sudo docker tag bulletinboard:1.0 2656439669/bulletinboard:1.0
```

3. 推送到 DockerHub

```shell
$ sudo docker push <Your Docker ID>/bulletinboard:1.0
$ sudo docker push 2656439669/bulletinboard:1.0

The push refers to repository [docker.io/2656439669/bulletinboard]
c66956eb301b: Preparing 
c3d5e8ff1b6d: Preparing 
64047ea6f28f: Preparing 
e2c94039bb1d: Preparing 
f9e20f0c67df: Preparing 
bf12098ab085: Waiting 
cd6ad3c3aa81: Waiting 
c447987a5233: Waiting 
4d3dd4268d84: Waiting 
denied: requested access to the resource is denied

```

### 安装

`https://docs.docker.com/engine/install/centos/`

###### 环境要求 

1. 对于centOS，环境必须是centOS 7 或者以上的维护版本
2. 卸载旧版本的 `docker` 或者`docker-engine`以及他们的相关依赖项

```shell
$ sudo yum remove docker                   
		docker-client
        docker-client-latest
        docker-common
        docker-latest
        docker-latest-logrotate
        docker-logrotate
        docker-engine
已加载插件：fastestmirror, langpacks
参数 docker 没有匹配
参数 docker-client 没有匹配
参数 docker-client-latest 没有匹配
参数 docker-common 没有匹配
参数 docker-latest 没有匹配
参数 docker-latest-logrotate 没有匹配
参数 docker-logrotate 没有匹配
参数 docker-engine 没有匹配
不删除任何软件包
```

3. 删除`var/lib/docker`

##### 安装方式

###### 使用储存库

方便安装升级

1. 设置储存库

```shell
# 安装 yum-utils
$ sudo yum install -y yum-utils
# 添加 repo 库 地址
$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

2. 1 直接安装最新版本

```shell
$ sudo yum install docker-ce docker-ce-cli containerd.io
```

docker 安装完毕，尚未启动。docker组被创建，没有用户添加到组中

2. 2 安装特定版本的docker

```shell
# 列出docker 各个版本
$ yum list docker-ce --showduplicates | sort -r

docker-ce.x86_64  3:18.09.1-3.el7                     docker-ce-stable
docker-ce.x86_64  3:18.09.0-3.el7                     docker-ce-stable
docker-ce.x86_64  18.06.1.ce-3.el7                    docker-ce-stable
docker-ce.x86_64  18.06.0.ce-3.el7                    docker-ce-stable

# 直接安装对应版本号，如 docker-ce-18.09.1
$ sudo yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io
# 如
$ sudo yum install docker-ce-19.03.1 docker-ce-cli-19.03.1 containerd.io

```

3. 启动docker

```shell
$ sudo systemctl start docker
```

###### 手动下载RPM包安装

完全手动管理

1. 在` https://download.docker.com/linux/centos/`选择centOS版本，`x86_64/stable/Packages/` ,下载想要的`.rpm`文件

2. 安装docker 根据指定文件

```shell
$ sudo yum install /path/to/package.rpm
```

3. 升级的时候，使用`yum -y upgrade`命令指向新文件进行升级

###### 自动化脚本安装

方便，但是不接受配置，全自动化

##### 卸载

```shell
# 卸载
$ sudo yum remove docker-ce docker-ce-cli containerd.io

# 删除残留文件 
$ sudo rm -rf /var/lib/docker
```

##### 开机启动

```shell
# 开机启动
$ sudo systemctl enable docker

# 停止开机启动
$ sudo systemctl disable docker
```

