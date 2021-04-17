# nginx



[toc]

##### 简介：

*Nginx* (engine x) 是一个高性能的HTTP和反向代理web服务器，同时也提供了IMAP/POP3/SMTP服务。

#### 作用:

```nginx
# web 服务.
# 负载均衡 （反向代理）
# web cache（web 缓存）

静态服务器。（图片，视频服务）另一个lighttpd。并发几万，html，js，css，flv，jpg，gif等。
动态服务，nginx——fastcgi 的方式运行PHP，jsp。（PHP并发在500-1500，MySQL 并发在300-1500）。
反向代理，负载均衡。日pv2000W以下，都可以直接用nginx做代理。
缓存服务。类似 SQUID,VARNISH。
```

#### 优缺点:

```nginx
高并发。静态小文件
占用资源少。2万并发、10个线程，内存消耗几百M。
功能种类比较多。web,cache,proxy。每一个功能都不是特别强。
支持epoll模型，使得nginx可以支持高并发。
nginx 配合动态服务和Apache有区别。（FASTCGI 接口）
利用nginx可以对IP限速，可以限制连接数。
配置简单，更灵活。
```

#### linux 下安装

```shell
# centos
# 1. 先决条件
sudo yum install yum-utils

# 2. 创建 /etc/yum.repos.d/nginx.repo 文件
# 2.1 进入/etc/yum.repos.d/ 文件夹
cd /etc/yum.repos.d/
# 2.2 创建 nginx.repo文件
touch nginx.repo
# 2.3 编辑文件
vi nginx.repo
# 2.4 把下面信息复制到文件中
[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true

[nginx-mainline]
name=nginx mainline repo
baseurl=http://nginx.org/packages/mainline/centos/$releasever/$basearch/
gpgcheck=1
enabled=0
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true
# 2.5 保存并退出 esc + :wq

# 3. 默认情况下，使用稳定的nginx软件包的存储库。如果要使用主线nginx软件包，请运行以下命令
sudo yum-config-manager --enable nginx-mainline

# 4. 安装nginx
sudo yum install nginx

# 5. 当提示您接受GPG密钥时，请验证指纹是否匹配 573B FD6B 3D8F BC64 1079 A6AB ABF5 BD82 7BD9 BF62，如果是，则接受它。

# 6. 安装完成
```

```shell
# 安装过程 

[root@centOS7 ~]# sudo yum install nginx
已加载插件：fastestmirror, langpacks
Loading mirror speeds from cached hostfile
 * base: mirrors.aliyun.com
 * extras: mirrors.aliyun.com
 * updates: mirrors.bfsu.edu.cn
nginx-stable                                                                    | 2.9 kB  00:00:00     
nginx-stable/7/x86_64/primary_db                                                |  56 kB  00:00:01     
正在解决依赖关系
--> 正在检查事务
---> 软件包 nginx.x86_64.1.1.18.0-1.el7.ngx 将被 安装
--> 解决依赖关系完成

依赖关系解决

=======================================================================================================
 Package           架构               版本                              源                        大小
=======================================================================================================
正在安装:
 nginx             x86_64             1:1.18.0-1.el7.ngx                nginx-stable             772 k

事务概要
=======================================================================================================
安装  1 软件包

总下载量：772 k
安装大小：2.7 M
Is this ok [y/d/N]: y
Downloading packages:
警告：/var/cache/yum/x86_64/7/nginx-stable/packages/nginx-1.18.0-1.el7.ngx.x86_64.rpm: 头V4 RSA/SHA1 Signature, 密钥 ID 7bd9bf62: NOKEY
nginx-1.18.0-1.el7.ngx.x86_64.rpm 的公钥尚未安装
nginx-1.18.0-1.el7.ngx.x86_64.rpm                                               | 772 kB  00:00:26     
从 https://nginx.org/keys/nginx_signing.key 检索密钥
导入 GPG key 0x7BD9BF62:
 用户ID     : "nginx signing key <signing-key@nginx.com>"
 指纹       : 573b fd6b 3d8f bc64 1079 a6ab abf5 bd82 7bd9 bf62
 来自       : https://nginx.org/keys/nginx_signing.key
是否继续？[y/N]：y
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  正在安装    : 1:nginx-1.18.0-1.el7.ngx.x86_64                                                    1/1 
----------------------------------------------------------------------

Thanks for using nginx!

Please find the official documentation for nginx here:
* http://nginx.org/en/docs/

Please subscribe to nginx-announce mailing list to get
the most important news about nginx:
* http://nginx.org/en/support.html

Commercial subscriptions for nginx are available on:
* http://nginx.com/products/

----------------------------------------------------------------------
  验证中      : 1:nginx-1.18.0-1.el7.ngx.x86_64                                                    1/1 

已安装:
  nginx.x86_64 1:1.18.0-1.el7.ngx                                                                      

完毕！
```

**查看nginx 安装官网信息**

- 进官网 `http://nginx.org/`

- 点击 `download`

  ![image-20201027112553643](.\nginx.assets\image-20201027112553643.png)

- 选择稳定主线版本

  ![image-20201027112714825](.\nginx.assets\image-20201027112714825.png)

- 选择指定安装方式

  ![image-20201027112830309](.\nginx.assets\image-20201027112830309.png)

  

-  安装教程

![image-20201027112911329](.\nginx.assets\image-20201027112911329.png)



#### 一些常用命令

```shell
# 查看nginx 版本号
nginx -v
nginx version: nginx/1.18.0

# 启动
nginx    //后续重启的话，只能通过强制删除进程，才能成功
//或者
systemctl start nginx.service

# 停止
nginx -s quit    //温和停止
nginx -s stop    //强硬停止
killall nginx    //野蛮停止，杀死进程
//或者
systemctl stop nginx.service

# 重启
systemctl restart nginx.service

# 查看nginx 服务的运行状态
ps aux | grep nginx

# 使用包管理工具，查看nginx安装目录
rpm -ql nginx

# 命令集
nginx -s quit
    stop —快速关机
    quit —正常关机
    reload —重新加载配置文件
    reopen —重新打开日志文件
```

#### 配置文件

##### 总配置文件格式

```nginx
... #全局配置

event {  #events块
    ...
}

http { # http块
    ...	# http 全局配置
    server {	#server
    	...     #server 全局
        location / {  # location
            ...
        }
         location / {
            ...
        }
    }
    server {
    	...
        location / {
            ...
        }
         location / {
            ...
        }
    }
    ... # http 全局配置
}
```

- 各部分解释
  - 全局块： 配置影响nginx全局的指令。一般有运行nginx服务器的用户组，nginx进程pid存放路径，日志存放路径，配置文件引入，允许生成worker process数等。
  - events块：配置影响nginx服务器或与用户的网络连接。有每个进程的最大连接数，选取哪种事件驱动模型处理连接请求，是否允许同时接受多个网路连接，开启多个网络连接序列化等。
  - http块：可以嵌套多个server，配置代理，缓存，日志定义等绝大多数功能和第三方模块的配置。如文件引入，mime-type定义，日志自定义，是否使用sendfile传输文件，连接超时时间，单连接请求数等。
  - server块：配置虚拟主机的相关参数，一个http中可以有多个server。
  - location块：配置请求的路由，以及各种页面的处理情况。

##### 配置文件详细说明

==总配置文件位置：`/etc/nginx/nginx.conf`==

```shell
# 运行用户，默认为nginx，可以不设置
user  nginx;
# nginx 进程，一般设置为和CPU核数一样
worker_processes  1;

# 错误日志存放目录 日志级别： debug|info|notice|warn|error|crit|alert|emerg
error_log  /var/log/nginx/error.log warn;
# 进程pid 存放位置
pid        /var/run/nginx.pid;


events {
    # accept_mutex on;   #设置网路连接序列化，防止惊群现象发生，默认为on
    # multi_accept on;  #设置一个进程是否同时接受多个网络连接，默认为off
    # use epoll;      #事件驱动模型，select|poll|kqueue|epoll|resig|/dev/poll|eventport
	# 单个后台进程的最大并发数
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;  # 文件拓展名与类型映射表，mime
    default_type  application/octet-stream;	# 默认文件类型
	# access_log off; # 取消服务日志
	# 设置日志模式
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

	# nginx 访问日志存放位置
    access_log  /var/log/nginx/access.log  main;

    sendfile        on; # 开启高效传输模式
    
#    client_max_body_size 8m; # 上传文件 默认是1M
    #tcp_nopush     on; # 减少网络报文段的数量

    keepalive_timeout  65; # 连接超时时间，单位是秒
    #client_header_timeout 10;  #客户端请求头读取超时时间
    #client_body_timeout 10; #设置客户端请求主体读取超时时间
    #send_timeout 10; #响应客户端超时时间

    #gzip  on;	# 开启gzip压缩
    #gzip_min_length 1k; #最小压缩文件大小
    #gzip_buffers 4 16k; #压缩缓冲区
	#gzip_http_version 1.0; #压缩版本（默认1.1，前端如果是squid2.5请使用1.0）
    #压缩等级 1-9 等级越高，压缩效果越好，节约宽带，但CPU消耗大
    #gzip_comp_level 2;
    #压缩类型，默认就已经包含text/html，所以下面就不用再写了，写上去也不会有问题，但是会有一个warn。
    #gzip_types text/plain application/x-javascript text/css application/xml;
    #前端缓存服务器缓存经过压缩的页面
    
	# 包含的子配置项位置和文件
    include /etc/nginx/conf.d/*.conf;
}
```

##### 子配置文件server

```shell
#  /etc/nginx/conf.d/default.conf

server {
    listen       80; # 监听的端口号
    server_name  localhost;	# 域名 可以写多个

    #charset koi8-r;	# 编码格式，如果网页格式与此格式不同，将被转码
    #access_log  /var/log/nginx/host.access.log  main; # nginx 访问日志

    location / {
        root   /usr/share/nginx/html;	# 服务器默认启动页
        index  index.html index.htm;	# 默认访问文件
    }

    #error_page  404              /404.html; # 404 页面

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;	# 错误状态码显示页面
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}
```

#### 配置

##### 1. 自定义错误页面

```nginx
error_page   状态码  错误页路径;
error_page 404 /404.html;
location = /40x.html {
}
```

##### 2. 访问权限

```shell
//allow：允许ip访问
//deny：禁止ip访问
//all：所有ip
//使用all会覆盖后者控制的权限（如：deny all在前，allow xxx.xxx.xxx.xxx在后，表示禁止所有ip访问）
#根目录权限 除了允许的，其他全部禁止
location / {
    allow xxx.xxx.xxx.xxx;
    deny all;
}

#子目录权限
location =/img{
    allow all;
}
location =/admin{
    deny all;
}

#使用正则，控制.php文件访问权限
location ~\.php$ {
    deny all;
}
```

##### 3. 虚拟主机

```shell
# 虚拟主机，就是把一台物理服务器划分成多个“虚拟”的服务器，每一个虚拟主机都可以有独立的域名和独立的目录
# nginx的虚拟主机就是通过nginx.conf中server节点指定的，想要设置多个虚拟主机，配置多个server节点即可

server { 
    listen 80; 
    # 指定这个虚拟主机名为a.test.com，当用户访问a.test.com时，就有这个虚机主机进行处理
    server_name a.test.com;  

    location / { 
        index index.html; 	# 默认首页
        root /home/www/host_a/; 	# 虚拟主机的物理根目录
    } 
}

# 虚拟主机名称格式
虚拟主机名可以有4种格式：
        （1）准确的名字，例如此例中的a.test.com
        （2）*号开头的，例如 *.test.com
        （3）*号结尾的，例如 mail.*
        （4）正则表达式形式，例如 
        server_name ~^www\d+\.test\.com$; 
注意，使用正则表达式形式时，必须以'~'开头
server_name也可以同时指定多个，例如：
server_name test.com www.test.com *.test.com;
这时优先级为：
        （1）确切的名字
        （2）最长的以*起始的通配符名字
        （3）最长的以*结束的通配符名字
        （4）第一个匹配的正则表达式名字
```

##### 5. 正向代理与反向代理

```
正向代理： 代理请求的地址,代理客户端，例如科学上网工具，把我们访问不了的网页代理到可以访问的服务器上
受益者是客户端
反向代理：代理服务器的地址，代理服务端，访问一个网址访问的是nginx ，nginx转发给真正的服务。
受益者是服务器

安全性：正向代理的客户端能够在隐藏自身信息的同时访问任意网站，这个给网络安全代理了极大的威胁。因此，我们必须把服务器保护起来，使用反向代理客户端用户只能通过外来网来访问代理服务器，并且用户并不知道自己访问的真实服务器是那一台，可以很好的提供安全保护。
功能性：反向代理的主要用途是为多个服务器提供负债均衡、缓存等功能。负载均衡就是一个网站的内容被部署在若干服务器上，可以把这些机子看成一个集群，那Nginx可以将接收到的客户端请求“均匀地”分配到这个集群中所有的服务器上，从而实现服务器压力的平均分配，也叫负载均衡。
```

#### 问题

##### 1.  访问静态资源出现 403 is forbidden

```shell
error_log 日志：

#45464: *14 "/data/www/index.html" is forbidden (13: Permission denied), client: 192.168.194.1, server: localhost, request: "GET / HTTP/1.1", host: "192.168.194.130"
```

```markdown
解决:

一、nginx 用户与当前用户不一致

	ps aux | grep "nginx: worker process" | awk '{print $1}'
nobody
root
	在nginx.conf 配置文件吧user 改为当前用户
	
二、缺少index.html或者index.php文件，就是配置文件中index index.html index.htm这行中的指定的文件。

    server {  
        listen       80;  
        server_name  localhost;  
        index  index.php index.html;  
        root  /data/www/;
    }
	如果在/data/www/下面没有index.php,index.html的时候，直接文件，会报403 forbidden。

三、权限问题，如果nginx没有web目录的操作权限，也会出现403错误。

解决办法：修改web目录的读写权限，或者是把nginx的启动用户改成目录的所属用户，重启Nginx即可解决

1.    chmod -R 777 /data

2.    chmod -R 777 /data/www/


四、SELinux设置为开启状态（enabled）的原因。

        4.1、查看当前selinux的状态。

        1.    /usr/sbin/sestatus

SELinux status:                 enabled
SELinuxfs mount:                /sys/fs/selinux
SELinux root directory:         /etc/selinux
Loaded policy name:             targeted
Current mode:                   enforcing
Mode from config file:          enforcing
Policy MLS status:              enabled
Policy deny_unknown status:     allowed
Max kernel policy version:      31

        4.2、将SELINUX=enforcing 修改为 SELINUX=disabled 状态。

        1.    vi /etc/selinux/config

        2.     

        3.    #SELINUX=enforcing 注掉

        4.    SELINUX=disabled  添加

        4.3、重启生效。reboot。

        1.    reboot
```

