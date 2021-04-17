# Git



[toc]



### 一、 Git 与 Svn

      1、Git 是分布式的，SVN 不是：这是 Git 和其它非分布式的版本控制系统，例如 SVN，CVS 等，最核心的区别。
    
      2、Git 把内容按元数据方式存储，而 SVN 是按文件：所有的资源控制系统都是把文件的元信息隐藏在一个类似 .svn、.cvs 等的文件夹里。
    
      3、Git 分支和 SVN 的分支不同：分支在 SVN 中一点都不特别，其实它就是版本库中的另外一个目录。
    
      4、Git 没有一个全局的版本号，而 SVN 有：目前为止这是跟 SVN 相比 Git 缺少的最大的一个特征。
    
      5、Git 的内容完整性要优于 SVN：Git 的内容存储使用的是 SHA-1 哈希算法。这能确保代码内容的完整性，确保在遇到磁盘故障和网络问题时降低对版本库的破坏。



![img](F:\GitFile\Typora\GIT\git.assets\50e8f080.png)

### 二、 安装配置

##### 一、
>在使用Git前我们需要先安装 Git。Git 目前支持 Linux/Unix、Solaris、Mac和 Windows 平台上运行。

>Git 各平台安装包下载地址为：http://git-scm.com/downloads

##### 二、Windows 平台上安装
>国内的镜像：https://npm.taobao.org/mirrors/git-for-windows/

>完成安装之后，就可以使用命令行的 git 工具（已经自带了 ssh 客户端）了，另外还有一个图形界面的 Git 项目管理工具。

>在开始菜单里找到"Git"->"Git Bash"，会弹出 Git 命令窗口，你可以在该窗口进行 Git 操作。

![50e8f080.png](F:\GitFile\Typora\GIT\git.assets\4ed674b8-1602224326985.png)

### 三、 Git 配置

```markdown
Git 提供了一个叫做 git config 的工具，专门用来配置或读取相应的工作环境变量。
这些环境变量，决定了 Git 在各个环节的具体工作方式和行为。这些变量可以存放在以下三个不同的地方：
```

>/etc/gitconfig 文件：系统中对所有用户都普遍适用的配置。若使用 git config 时用 --system 选项，读写的就是这个文件。

>~/.gitconfig 文件：用户目录下的配置文件只适用于该用户。若使用 git config 时用 --global 选项，读写的就是这个文件。

>当前项目的 Git 目录中的配置文件（也就是工作目录中的 .git/config 文件）：这里的配置仅仅针对当前项目有效。每一个级别的配置都会覆盖上层的相同配置，所以 .git/config 里的配置会覆盖 /etc/gitconfig 中的同名变量。

###### 1. 用户信息

```shell
设置用户信息
  $ git config --global user.name "Your Name"
  $ git config --global user.email "email@example.com"

查看用户信息
  $ git config user.name
  $ git config user.email
直接找到 C:\Users\26564\.gitconfig 修改
  [user]
    name = wu
    email = 2656439669@qq.com      
```

>如果要在某个特定的项目中使用其他名字或者电邮，只要去掉 --global 选项重新配置即可，新的设定保存在当前项目的 .git/config 文件里。

###### 2. 

```shell
查看配置信息
  $ git config --list

打开配置文件内容
  $ vim ~/.gitconfig
```

#### 工作流程

一般工作流程如下：

- 克隆 Git 资源作为工作目录。
- 在克隆的资源上添加或修改文件。
- 如果其他人修改了，你可以更新资源。
- 在提交前查看修改。
- 提交修改。
- 在修改完成后，如果发现错误，可以撤回提交并再次修改并提交。

### 四、Git 使用
#### shell命令

-------------------------------------------
###### git init 初始化仓库
```shell
初始化当前目录，该命令执行完后会在当前目录生成一个 .git 目录。使用我们指定目录作为Git仓库。
git init

初始化当前目录下的指定文件夹
git init newrepo

```

----------------------------------------
###### git clone 克隆项目
```shell
克隆项目  [url] 是你要拷贝的项目
git clone [url]  

在当前目录下创建一个名为 boostnote 的目录，作为git 仓库。
$ git clone $ git clone git@github.com:wushutong/boostnote.git
指定 mygrit 文件夹为 git 仓库
$ git clone $ git clone git@github.com:wushutong/boostnote.git mygrit
```
----------------------------------
###### git add
```shell
git add 命令可将该文件添加到暂存区。

添加一个或多个文件到暂存区：
$ git add [file1] [file2] ...
$ git add a b.java c.txt ...

添加指定目录到暂存区，包括子目录：
$ git add [dir]

添加当前目录下所有untrack的文件都加入暂存区，并且会根据.gitignore做过滤
$ git add .

忽略 .gitignore 过滤文件
$ git add *
```

-----------------------------------------
###### git status
```shell
命令用于查看在你上次提交之后是否有对文件进行再次修改。
$ git status

获取简短的结果 
$ git status -s   $ git status --short

A: 你本地新增的文件（服务器上没有）
C: 文件的一个新拷贝
D: 你本地删除的文件（服务器上还在）
M: 文件的内容或者mode被修改了
R: 文件名被修改了
T: 文件的类型被修改了
U: 文件没有被合并(你需要完成合并才能进行提交)
X: 未知状态(很可能是遇到git的bug了，你可以向git提交bug report)
？：未被git进行管理，可以使用git add file1把file1添加进git能被git所进行管理
```

-------------------------------------------
###### git diff
```shell
      git diff 命令比较文件的不同，即比较文件在暂存区和工作区的差异。
      
      显示暂存区和工作区的差异:
      $ git diff [file]
      
      显示暂存区和上一次提交(commit)的差异:
      $ git diff --cached [file]
      或
      $ git diff --staged [file]
      
      显示两次提交之间的差异:
      $ git diff [first-branch]...[second-branch]

```

------------------------------------------
###### git commit
```shell
$ git commit 命令将暂存区内容添加到本地仓库中。

提交暂存区到本地仓库中: 要输入日志消息 ""
$ git commit -m [message]

提交暂存区的指定文件到仓库区：
$ git commit [file1] [file2] ... -m [message]

-a 参数设置修改文件后不需要执行 git add 命令，直接来提交 跳过add，相当于自动add
$ git commit -a
$ git commit -am 'msg' 示例
```

---------------------------------------------
###### git push

```shell
  git push 命用于从将本地的分支版本上传到远程并合并。
```

```shell

git push <远程主机名> <本地分支名>:<远程分支名>
$ git push origin master:master

如果本地分支名与远程分支名相同，则可以省略冒号：

git push <远程主机名> <本地分支名>
$ git push origin master

如果本地版本与远程版本有差异，但又要强制推送可以使用 --force 参数：

git push --force origin master
删除主机但分支可以使用 --delete 参数，以下命令表示删除 origin 主机的 master 分支：

git push origin --delete master
```

----------------------------------
###### git pull

```shell
git pull 命用于从远程获取代码并合并本地的版本。
```

```shell
git pull <远程主机名> <远程分支名>:<本地分支名>

$ git pull
$ git pull origin

将远程主机 origin 的 master 分支拉取过来，与本地的 brantest 分支合并。
$ git pull origin master:brantest

如果远程分支是与当前分支合并，则冒号后面的部分可以省略。
$ git pull origin master
```
--------------------------------
###### git remote

```shell
git remote 命令用于在远程仓库的操作。
```

```shell
列出已经存在的远程地址名称
$ git remote 

显示所有远程仓库：在每一个名字后面列出其远程url
$ git remote -v
origin  https://github.com/tianqixin/runoob-git-test (fetch)
origin  https://github.com/tianqixin/runoob-git-test (push)

origin 为远程地址的别名。

显示某个远程仓库的信息：
git remote show [remote]
$ git remote show https://github.com/tianqixin/runoob-git-test

添加远程版本库：
git remote add [shortname] [url]
$ git remote add origin git@github.com:tianqixin/runoob-git-test.git

git remote rm name  # 删除远程仓库
git remote rename old_name new_name  # 修改仓库名

```

--------------------------------------
###### git fetch  git merge
```shell
git fetch 命用于从远程获取代码库。
```
```shell

获取服务端修改的代码
$ git fetch origin 

合并代码
$ git merge origin/master
```

----------------------------------------
###### git log 查看日志
```shell
查看所有提交日志
$ git log

查看简洁的日志
$ git log --oneline

反序展示日志
$ git log --reverse

查看指定文件修改记录
$ git blame 文件名

```

------------------------------
###### git reset
```shell
git reset 命令用于回退版本，可以指定退回某一次提交的版本。
```
```shell
$ git reset HEAD^            # 回退所有内容到上一个版本  
$ git reset HEAD^ hello.php  # 回退 hello.php 文件的版本到上一个版本  
$ git  reset  052e           # 回退到指定版本


git reset --soft HEAD   回退到某个版本
git reset --hard HEAD   回退到某个版本并删除所有修改

$ git reset –hard HEAD~3  # 回退上上上一个版本  
$ git reset –hard bae128  # 回退到某个版本回退点之前的所有信息。 
$ git reset --hard origin/master    # 将本地的状态回退到和远程的一样 

清除 add
git reset HEAD 文件名
```
      HEAD 说明：
        HEAD 表示当前版本
        HEAD^ 上一个版本
        HEAD^^ 上上一个版本
        HEAD^^^ 上上上一个版本
        以此类推...
    
        可以使用 ～数字表示
        HEAD~0 表示当前版本
        HEAD~1 上一个版本
        HEAD^2 上上一个版本
        HEAD^3 上上上一个版本
        以此类推...

----------------------------------
###### git rm 删除
```shell
删除暂存区和工作区中的文件
git rm 文件名

如果删除之前修改过并放到了暂存区 -f 强制
git rm -f 文件名

仅从暂存区删除
git rm --cached 文件名

递归删除
git rm -r *

删库跑路
git rm -rf *

```

-----------------------------------
###### git mv
```shell
git mv 命令用于移动或重命名一个文件、目录或软连接。
```
```shell
改名
git mv [file] [newfile]

强制改名
git mv -f [file] [newfile]
```

------------------------------------
###### git 分支
```shell
列出本地分支 *开头代表当前分支
$ git branch

创建分支 test
$ git branch test

切换分支到 test
$ git checkout test

创建新分支并切换到新分支
$ git checkout -b test

删除分支
$ git checkout -d test

将其他分支合并到当前分支
$ git merge test
```

------------------------------
###### git tag
```shell
查看所有标签
$ git tag

对项目打标签 v1.0 -a代表可以对这个版本进行说明注释
$ git tag -a v1.0 

对历史提交进行打标签
$ git tag -a v1.0 85fd8

删除标签
$ git tag -d v1.0

查看此版本的修改
$ git show v1.0
```

------------------------------
###### 搭建 git 服务器
```shell
# 例 centOS
# 1. 安装 git
$ yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel perl-devel
$ yum install git

# 接下来我们 创建一个git用户组和用户，用来运行git服务：
$ groupadd git
$ useradd git -g git

# 2、创建证书登录
# 收集所有需要登录的用户的公钥，公钥位于id_rsa.pub文件中，把我们的公钥导入到/home/git/.ssh/authorized_keys文件里，一行一个。
# 如果没有该文件创建它：

$ cd /home/git/
$ mkdir .ssh
$ chmod 755 .ssh
$ touch .ssh/authorized_keys
$ chmod 644 .ssh/authorized_keys

# 3、初始化Git仓库
# 首先我们选定一个目录作为Git仓库，假定是/home/gitrepo/runoob.git，在/home/gitrepo目录下输入命令：

$ cd /home
$ mkdir gitrepo
$ chown git:git gitrepo/
$ cd gitrepo
$ git init --bare runoob.git

# 以上命令Git创建一个空仓库，服务器上的Git仓库通常都以.git结尾。然后，把仓库所属用户改为git：
$ chown -R git:git runoob.git

# 4、克隆仓库
$ git clone git@192.168.45.4:/home/gitrepo/runoob.git
```


```shell
显示当前 git 配置信息
$ git config --list

编辑 git 配置文件
$ git config -e    # 针对当前仓库 
$ git config -e --global   # 针对系统上所有仓库
如果去掉 --global 参数只对当前仓库有效。
```

#### 常用命令

>workspace：工作区
>staging area：暂存区/缓存区
>local repository：或本地仓库
>remote repository：远程仓库

![img](F:\GitFile\Typora\GIT\git.assets\1622372b.png)



#### github 使用

因为提交代码使用用户名和密码不太安全，所以将本地ssh key 设置到github，这样就不需要密码直接上传文件了

1.  git 生成SSH keys
2.  github 设置 Settings-> SSH keys->New SSH key



#### gitee

与github一致

#### 生成 ssh key

```shell
生成key命令
$ ssh-keygen -t rsa -C "邮箱"
```

下面的输入内容可以不写，直接回车，但是默认名称就是id_rsa.如果我们在以后也需要配置ssh，那么容易产生混淆。建议后面加上ssh为何而建。也是为了避免ssh覆盖。

Enter file in which to save the key (/c/Users/Administrator/.ssh/id_rsa): /c/Users/Administrator/.ssh/id_rsa_github/id_rsa_github 配置保存目录 

Enter passphrase (empty for no passphrase):  输入密码

Enter same passphrase again:   再次输入密码
```shell

$ ssh-keygen -t rsa -C "2656439669@qq.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/Administrator/.ssh/id_rsa): /c/Users/Administrator/.ssh/id_rsa_github/id_rsa_github
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/Administrator/.ssh/id_rsa_github/id_rsa_github
Your public key has been saved in /c/Users/Administrator/.ssh/id_rsa_github/id_rsa_github.pub
The key fingerprint is:
SHA256:rQGOWgX4bzhT3phj/wVCTXp76cIXkf5vrQqt7kk2GcA 2656439669@qq.com
The key's randomart image is:
+---[RSA 3072]----+
|   ..     .      |
|  .  . . +   .   |
|   .  o E o o    |
|    .+.o + o o   |
|    o=.+S = =    |
|   o+ O .= B o   |
|  .  = o. O = . .|
|        .o O   .o|
|         +* ...o.|
+----[SHA256]-----+

```
```shell
# 命令行复制key的命令如下：
$ clip < ~/.ssh/id_rsa.pub   #执行完毕直接去服务端粘贴即可

# 查看key命令：
$ cat < ~/.ssh/id_rsa.osc.pub    #可以查看你的key

$ cat < ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC+kHYOhxBWSk9XydyJt9FmmNTlOHPGN8RfzKAXXZjUPbCZ9ygVNVef0SgDBNZkun0xTkTuXjZFzJeZlY+durijBpaNE6SU+3/G+hvcshheRiemS0J+dPSUBrFe4OrZhut5JB9cZA1JmHOWlGDXhwcGTrKUoLDVGCk2jbGNsXNPXGsICVLk3PrmYquJW9a5RYKTg6noNX3xVoEur+DEybqxhiGiUkwIzkYlFYFxt3s6CRSNrhnRKLVXGoPZPDjT6GsUl8loo6Z4wvt4X9XpMM1BjGySSCBMXehiQl9Ic3bwE84NK5/QXwe1HTvE7daOOX+SzKKx9cOjh/uRvRQn0j/+9f3DbYcmi+mT4ID/11Nd6uKa+h5+TKe+4PF2yaDuhQMXjDSn8/g3cUgrKqfIA4wVuhRoH+f2cPBaXjaMtT5kk3+iwT7XRSJe1mi15QNR2WUS1eJP+2/R0PHWTqN1R4BUpo/h6CNsbYaPiyQfe2P8LZe/22MXp9UhmfL8qgE1R2s= 2656439669@qq.com
```




#### config 配置多个 ssh key 来配置多个远程仓库

只需要分别配置多个ssh key 即可链接多个远程仓库

```shell
# 在 .ssh 目录下创建 config
vim config

# 在config文件中编辑

#One  # 注释

Host github.com   			# 这是 ssh -T git@github.com 命令的@ 后面的部分，相当于一个名字 
Hostname ssh.github.com     # HostName就是git托管的平台url
User 2656439669@qq.com      # 用户名
IdentityFile ~/.ssh/id_rsa_github/id_rsa_github  # 指定文件 ~/.ssh/id_rsa
PreferredAuthentications publickey
Port 443

```

```properties
# 例如同时配置 github 和 gitee
Host github.com
User 2656439669@qq.com
Hostname ssh.github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_github/id_rsa_github


Host gitee.com
User 18403002031
Hostname gitee.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_gitee/id_rsa_gitee
```

**测试**

```shell
$ ssh -T git@gitee.com
Hi 18403002031! You've successfully authenticated, but GITEE.COM does not provide shell access.

$ ssh -T git@github.com
Hi wushutong! You've successfully authenticated, but GitHub does not provide shell access.

```



#### 远程库链接

```shell
  查看远程库信息：
  $ git remote -v
  origin  git@github.com/wushutong/record.git (fetch)
  origin  git@github.com/wushutong/record.git (push)

  删除关联的origin的远程库
  $ git remote rm origin

  再次链接
  $ git remote add origin git@github.com:wushutong/boostnote.git

  重复链接异常      
  fatal: remote origin already exists. 
```
### 五、问题

##### (1)`git add .`异常

```shell
$ git add .
warning: adding embedded git repository: typora
hint: You've added another git repository inside your current repository.
hint: Clones of the outer repository will not contain the contents of
hint: the embedded repository and will not know how to obtain it.
hint: If you meant to add a submodule, use:
hint:
hint:   git submodule add <url> typora
hint:
hint: If you added this path by mistake, you can remove it from the
hint: index with:
hint:
hint:   git rm --cached typora
hint:
hint: See "git help submodule" for more information.
　　　　　　　　　
```

**原因：**

即在本地初始化的仓库(使用 git init的文件夹) 中的某一个文件夹，也含有 .git 文件 。

**解决：**

删除子文件夹里的.git文件，然后重新add、commit、push

##### (2)`git push -u origin master` 异常

**pull**

```shell
$ git push -u origin master
fatal: 'git@github.com/wushutong/record.git' does not appear to be a git repository
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

**检查SSH是否能够连接成功**

```shell
$ ssh -T git@github.com
ssh: connect to host github.com port 22: Connection timed out
```

**查看是否存在 id_rsa   id_rsa.pub  known_hosts 三个文件 目的就是查看此设备有没有配置过SSH key，如果没有，就是缺少 ssh key，生成即可**

```shell
# windows 在user\.ssh 文件夹
C:\Users\Administrator\.ssh

# linux
cd ~/.ssh
```

**如果存在SSH KEY，则新建config文件输入下面内容**

```json
Host github.com						# 这是 ssh -T git@github.com 命令的@ 后面的部分，相当于一个名字 
User 2656439669@qq.com				# 用户名
Hostname ssh.github.com				# HostName就是git托管的平台url
PreferredAuthentications publickey	#
IdentityFile ~/.ssh/id_rsa			# SSH KEY 位置
Port 443							# 端口

```

**重新连接**

```shell
$ ssh -T git@github.com
The authenticity of host '[ssh.github.com]:443 ([18.140.96.234]:443)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[ssh.github.com]:443,[18.140.96.234]:443' (RSA) to the list of known hosts.
Hi wushutong! You've successfully authenticated, but GitHub does not provide shell access.

$ ssh -T git@github.com
Hi wushutong! You've successfully authenticated, but GitHub does not provide shell access.
```



##### (3)windows 凭据管理

Windows版本的Git会默认使用Windows凭据来保存Git验证身份，（使用 ssh key 则不需要使用这个）

```
控制面板==》用户账户==》凭据管理器


```

**管理是否windows 保存git 凭据**

```
Git安装根目录中的mingw64\etc\编辑gitconfig文件，删除最后helper = manager一行，然后保存。
这样再次使用git，都会提示密码
```

