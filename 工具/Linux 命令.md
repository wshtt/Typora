# Linux 命令



[toc]







## 基础

###### shell

Shell 是一个应用程序，它连接了用户和 Linux 内核，让用户能够更加高效、安全、低成本地使用 Linux 内核，这就是 Shell 的本质。shell是"为使用者提供操作界面"的软件（命令解析器）,用户通过这个软件来简单的操作系统内核。

- 图形界面shell，如windows
- 命令行shell，如linux shell

###### 命令格式

**command [options] [arguments]**   中括号代表是可选的，即有些命令不需要选项也不需要参数，但有的命令在运行时需要多个选项或参数。

- **选项options** ：选项是调整命令执行行为的开关，即，选项不同决定了命令的显示结果不同。
  - 短选项 ：-a 即（- 后接单个字母）
    - 短选项使用 `-`引导，多个短选项之间用空格隔开
    - 有的短选项可以组合，如 -lh
    - 有的短选项可以不带 -
    - 有的短选项需要带参数，如 -L 512M
  - 长选项： --help (-- 后面接单词)
    - 长选项都是完整的单词
    - 长选项一般不组合使用
    - 长选项参数格式：--size=1G






## 文件与目录操作

### 一、 编辑文件

##### 1. `vi` 与 `vim`

```shell
vi 与 vim

共同点：都是文本编辑器。

不同点：

   vi编辑器：vi是所有Unix及Linux系统下标准的编辑器，他就相当于windows系统中的记事本一样，它的强大不逊色于任何最新的文本编辑器。他是我们使用Linux系统不能缺少的工具。
   vim编辑器：vim可以当作vi的升级版本，vim最大的特点是具有程序编辑的能力，可以以字体颜色辨别语法的正确性，方便程序设计。vim里面加入了很多额外的功能，例如支持正则表达式的搜索、多文件编辑、块复制等等。
```

**`vi`的使用**

```
三种模式

①、一般模式：

	当以vi打开一个文件就直接进入一般模式了(这是默认的模式)。在这个模式中，你可以使用上下左右按键来移动光标，你可以使用删除字符或删除整行来处理文件内容，也可以使用复制、粘贴来处理你的文件数据。
    
②、编辑模式：

	在一般模式中可以进行删除、复制、粘贴等的操作，但是却无法编辑文件的内容，只有当到你按下【i, I, o,O, a, A, r, R】等任何一个字母之后才会进入编辑模式。这时候屏幕的左下方会出现【INSERT或 REPLACE】的字样，此时才可以进行编辑。而如果要回到一般模式时，则必须要按下【Esc】即可退出编辑模式。          
③、命令行模式：

	输入【 : / ? 】三个中的任何一个，就可以将光标移动到最底下那一行。在这个模式中，可以提供查找、读取、存盘、替换字符、离开vi、显示行号等的动作则是在此模式中完成的。
```

```markdown
# vi 命令
*一般模式*
  1.移动光标
        h：控制光标左移一格
        j ：控制光标下移一格
        k：控制光标上移一格
        l ：控制光标右移一格
        ctrl+b：屏幕往后移动一页
        ctrl+f ：屏幕往前移动一页
        ctrl+u：屏幕往后移动半页
        ctrl+d：屏幕往前移动半页
        0 ：移到文章的开头
        G：移动到文章的最后
        $ ：移动到光标所在行的行尾
        ^ ：移动到光标所在行的行首
        w：光标跳到下个字的开头
        e ：光标跳到下个字的字尾
        b ：光标回到上个字的开头
        #l：光标移到该行的第#个位置，如：5l,56l
   2.删除文字
        x：   每按一次，删除光标所在位置的"后面"一个字符
        #x： 例如，「6x」表示删除光标所在位置的"后面"6个字符
        X：  大写X，每按一次，删除光标所在位置的"前面"一个字符
        #X： 例如，「20X」表示删除光标所在位置的"前面"20个字符
        dd：  删除光标所在行
        #dd：从光标所在行开始删除#行
   3.复制
        yw：  将光标所在之处到字尾的字符复制到缓冲区中 
        #yw：复制#个字到缓冲区 yy：复制光标所在行到缓冲区
        #yy： 例如，「6yy」表示拷贝从光标所在的该行"往下数"6行文字 
        p：   将缓冲区内的字符贴到光标所在位置；
        注意：所有与"y"有关的复制命令都必须与"p"配合才能完成复制与粘贴功能
   4.替换
        r ：替换光标所在处的字符。
        R：替换光标所到之处的字符，直到按下「ESC」键为止 
   5.跳至指定的行
        ctrl+g：列出光标所在行的行号
        #G：例如，「15G」，表示移动光标至文章的第15行行首
   6.更改
        cw：  更改光标所在处的字到字尾处
        c#w：例如，「c3w」表示更改3个字
   7.恢复上一次操作
        u：如果您误执行一个命令，可以马上按下「u」，回到上一个操作；
        按多次"u"可以执行多次回复
*编辑模式*
        i ： 从目前光标所在处插入
        I ： 在目前所在行的第一个非空格符处开始插入
        a： 从目前光标所在的下一个字符处开始插入
        A： 从光标所在行的最后一个字符处开始插入
        o ：在目前光标所在的下一行处插入新的一行
        O：在目前光标所在处的上一行插入新的一行
        r ： 只会取代光标所在的那一个字符一次
        R：会一直取代光标所在的文字，直到按下 ESC 为止
        Esc：退出编辑模式，回到一般模式
        
*命令行模式*

指令前带 ":"
        :w ： 保存编辑的内容
        :w! ：强制写入该文件，但跟你对该文件的权限有关
        :q ：  离开vi
        :q! ： 不想保存修改强制离开
        :wq ：保存后离开
        :x ：   保存后离开
        ZZ ：若文件没有更动，则不保存离开，若文件已经被更改过，则保存后离开
        :w filename ：将编辑的数据保存成另一个文件（类似另存）
        :r filename ： 在编辑的数据中，读入另一个文件的数据。
        即将【filename】 这个文件的内容加到光标所在行后面
        :n1,n2 w filename ：将n1到n2的内容保存成filename这个文件
        :! command ：暂时离开vi 到命令行模式下执行command的显示结果；
        例如 【:! ls /home】即可在 vi 当中察看/home底下以ls输出的文件信息
        :set nu ：显示行号
        :set nonu ：与 set nu 相反，为取消行

   查找
        /xxx： 向光标向下寻找一个名称为xxx的字符串
        ?xxx：向光标向上寻找一个名称为xxx的字符串
        n：代表重复前一个查找的动作
        N：与n刚好相反，为【反向】进行行前一个查找动作
        :n1,n2s/xxx/yyy/g：n1与n2为数字，在第n1与n2行之间查找xxx这个字符串，
        并将该字符串替换为yyy
        :1,$s/xxx/yyy/g： 从第一行到最后一行查找xxx字符串，并将该字符串替换为yyy
        :1,$s/xxx/yyy/gc：从第一行到最后一行查找xxx字符串，并将该字符串替换为yyy，
        且在替换前提示用户确认是否进行替换
```



### 二、创建文件

```shell
# 创建文件夹 test
mkdir test 
# 创建两个文件夹
mkdir test1 test2
# 创建一个目录树
mkdir -p /usr/test/test1

# 创建文件
touch a.txt
```

### 三、文件目录操作

```shell
cd
   # 进入指定目录 
    cd /home/
    cd /home/data/
    # 返回上一级
    cd ..
    # 返回上一次所在目录
    cd -

    # 查看当前所在目录
    pwd

ls
    # 查看目录下文件
    ls
    # 显示隐藏文件
    ls -a
    # 显示详细信息 
    ls -l	# 简写 ll
    # 按照时间显示文件
    ls -lrt # l 详细信息、 r 倒叙、 t 时间顺序

cp
    # 复制一个test1，命名为test2
    cp test1 test2
    # -a：此选项通常在复制目录时使用，它保留链接、文件属性，并复制目录下的所有内容。其作用等于dpR参数组合。
    # -d：复制时保留链接。这里所说的链接相当于Windows系统中的快捷方式。
    # -f：覆盖已经存在的目标文件而不给出提示。
    # -i：与-f选项相反，在覆盖目标文件之前给出提示，要求用户确认是否覆盖，回答"y"时目标文件将被覆盖。
    # -p：除复制文件的内容外，还把修改时间和访问权限也复制到新文件中。
    # -r：若给出的源文件是一个目录文件，此时将复制该目录下所有的子目录和文件。
    # -l：不复制文件，只是生成链接文件。 
    cp -a dir1/ dir2
    cp -rf /usr/local/dir1/test3 /usr/local/dir2/test1

mv
    # 移动文件/剪切文件 重命名
    #  -f, --force                  覆盖前不询问
    #  -i, --interactive            覆盖前询问
    #  -n, --no-clobber             不覆盖已存在文件
    mv dir1/test1 dir2/test2
    # 移动文件夹
    mv -f dir1/ dir2

rm
    # 删除文件
    rm test1
    # -f, --force    忽略不存在的文件，从不给出提示。
    # -i, --interactive 提示是否删除
    # -r, -R, --recursive   指示rm将参数中列出的全部目录和子目录均递归地删除。
    # -v, --verbose    详细显示进行的步骤
```

### 四、查看文件

```shell
cat
	#  -A, --show-all           等于-vET
    #  -b, --number-nonblank    对非空输出行编号
    #  -e                       等于-vE
    #  -E, --show-ends          在每行结束处显示"$"
    #  -n, --number             对输出的所有行编号
    #  -s, --squeeze-blank      不输出多行空行
    #  -t                       与-vT 等价
    #  -T, --show-tabs          将跳格字符显示为^I
    #  -u                       (被忽略)
    #  -v, --show-nonprinting   使用^ 和M- 引用，除了LFD和 TAB 之外
    cat file1
    # 显示行号
    cat -n file

head
    # 显示前n 行
    head -3 file
    #  -q, --quiet, --silent 不显示包含给定文件名的文件头
    #  -v, --verbose         总是显示包含给定文件名的文件头
    head -3v file

tail
    # 显示后n 行
    tail -3 file
```

### 五、文件内搜索

```shell
grep  # 能使用正则表达式搜索文本，并把匹配的行打印出来
    # -i :  搜索时候忽略大小写
    # -v:  反转匹配，选择没有被匹配到的内容。
    # 查找 包含str 的行
    grep str /tmp/test
    grep -i pattern files
    # 查找以 str 开始的行
    grep ^str /tmp/test
    # 查找含有数字的行
    grep [0-9] /tmp/test

# 查看两个文件的不同之处
diff file1 file2
# 对比显示两个文件的不同之处
sdiff file1 file2
```

### 六、查找文件

```shell
find
    # 从根目录查找名为file1的文件和目录
    # -name 文件名
    # -iname 忽略大小写的文件名
    # -type f file; d dir; c char; b ; l link; p 管道
    # -size 大小 -n 小于; +n 大于 -100M；
    # -user 用户名
    find / -name file1
    # 查找属于user1的文件和目录
    find / -user user1
    # 查找以 .bin 结尾的文件
    find /home/user -name *.bin

```

### 七、压缩解压

```shell
tar
# -c 建立一个压缩文件的参数指令（create）--压缩
# -x 解开一个压缩文件的参数指令（extract）--解压
# -z 是否需要用gzip压缩
# -v 压缩的过程中显示文件（verbose）
# -f 使用档名，在f之后要立即接档名（file）
# 将文件解压到/data/dir/ 目录下，省略-C则为当前文件夹
tar zxvf 被解压文件 解压目录
tar zxvf file1.tar -C /data/dir/
# 
tar zcvf 压缩后文件名 被压缩文件/目录
tar zcvf /data/dir.tar /data/dir
```

### 八、进程

```shell
ps # 进程 
# 查找nginx 的所有进程
ps -ef | grep nginx
# 命令
    Basic options:        基本选择
     -A, -e               所有进程
     -a                   all with tty, except session leaders
      a                   all with tty, including other users
     -d                   all except session leaders
     -N, --deselect       negate selection
      r                   运行中进程
      T                   此终端上的所有进程
      x                   processes without controlling ttys

    Selection by list:	  列表选择
     -C <command>         command name  # 名称 -C nginx
     -G, --Group <GID>    real group id or name # -G 组id或名
     -g, --group <group>  session or effective group name 
     -p, p, --pid <PID>   process id # 进程id PID
            --ppid <PID>  parent process id # 父进程id
     -q, q, --quick-pid <PID>
                          process id (quick mode)
     -s, --sid <session>  session id
     -t, t, --tty <tty>   terminal # 终端名
     -u, U, --user <UID>  effective user id or name
     -U, --User <UID>     real user id or name

      The selection options take as their argument either:
        a comma-separated list e.g. '-u root,nobody' or
        a blank-separated list e.g. '-p 123 4567'

    Output formats:
     -F                   extra full
     -f                   full-format, including command lines
      f, --forest         ascii art process tree
     -H                   show process hierarchy
     -j                   jobs format
      j                   BSD job control format
     -l                   long format
      l                   BSD long format
     -M, Z                add security data (for SELinux)
     -O <format>          preloaded with default columns
      O <format>          as -O, with BSD personality
     -o, o, --format <format>
                          user-defined format
      s                   signal format
      u                   user-oriented format
      v                   virtual memory format
      X                   register format
     -y                   do not show flags, show rss vs. addr (used with -l)
         --context        display security context (for SELinux)
         --headers        repeat header lines, one per page
         --no-headers     do not print header at all
         --cols, --columns, --width <num>
                          set screen width
         --rows, --lines <num>
                          set screen height

    Show threads:
      H                   as if they were processes
     -L                   possibly with LWP and NLWP columns
     -m, m                after processes
     -T                   possibly with SPID column

    Miscellaneous options:
     -c                   show scheduling class with -l option
      c                   show true command name
      e                   show the environment after command
      k,    --sort        specify sort order as: [+|-]key[,[+|-]key[,...]]
      L                   show format specifiers
      n                   display numeric uid and wchan
      S,    --cumulative  include some dead child process data
     -y                   do not show flags, show rss (only with -l)
     -V, V, --version     display version information and exit
     -w, w                unlimited output width
```

### 九、rpm yum | echo

```shell
rpm #RPM软件包的管理工具
# 列出安装过的包
rpm -qa
# 获取nginx 软件包全名
rpm -q nginx
# 查看nginx 所有安装文件位置(rpm 安装的)
rpm -ql nginx
rpm -e nginx # 卸载
# 安装
rpm -ivh package.rpm


查询/验证软件包选项：
  -a, --all                        查询/验证所有软件包
  -f, --file                       查询/验证文件属于的软件包
  -g, --group                      查询/验证组中的软件包
  -p, --package                    查询/验证一个软件包
  --pkgid                          query/verify package(s) with package identifier
  --hdrid                          query/verify package(s) with header identifier
  --triggeredby                    query the package(s) triggered by the package
  --whatrequires                   query/verify the package(s) which require a dependency
  --whatprovides                   查询/验证提供相关依赖的软件包
  --nomanifest                     不把非软件包文件作为清单处理

查询选项（用 -q 或 --query）：
  -c, --configfiles                列出所有配置文件
  -d, --docfiles                   列出所有程序文档
  -L, --licensefiles               list all license files
  --dump                           转储基本文件信息
  -l, --list                       列出软件包中的文件
  --queryformat=QUERYFORMAT        使用这种格式打印信息
  -s, --state                      显示列出文件的状态

验证选项（用 -V 或 --verify）：
  --nofiledigest                   不验证文件摘要
  --nofiles                        不验证软件包中文件
  --nodeps                         不验证包依赖
  --noscript                       不执行验证脚本

安装/升级/擦除选项：
  --allfiles                       安装全部文件，包含配置文件，否则配置文件会被跳过。
  --allmatches                     移除所有符合 <package> 的软件包(如果 <package>
                                   被指定未多个软件包，常常会导致错误出现)
  --badreloc                       对不可重定位的软件包重新分配文件位置
  -e, --erase=<package>+           清除 (卸载) 软件包
  --excludedocs                    不安装程序文档
  --excludepath=<path>             略过以 <path> 开头的文件
  --force                          --replacepkgs --replacefiles 的缩写
  -F, --freshen=<packagefile>+     如果软件包已经安装，升级软件包
  -h, --hash                       软件包安装的时候列出哈希标记 (和 -v
                                   一起使用效果更好)
  --ignorearch                     不验证软件包架构
  --ignoreos                       不验证软件包操作系统
  --ignoresize                     在安装前不检查磁盘空间
  -i, --install                    安装软件包
  --justdb                         更新数据库，但不修改文件系统
  --nodeps                         不验证软件包依赖
  --nofiledigest                   不验证文件摘要
  --nocontexts                     不安装文件的安全上下文
  --noorder                        不对软件包安装重新排序以满足依赖关系
  --noscripts                      不执行软件包脚本
  --notriggers                     不执行本软件包触发的任何脚本
  --nocollections                  请不要执行任何动作集
  --oldpackage                     更新到软件包的旧版本(带 --force
                                   自动完成这一功能)
  --percent                        安装软件包时打印百分比
  --prefix=<dir>                   如果可重定位，便把软件包重定位到 <dir>
  --relocate=<old>=<new>           将文件从 <old> 重定位到 <new>
  --replacefiles                   忽略软件包之间的冲突的文件
  --replacepkgs                    如果软件包已经有了，重新安装软件包
  --test                           不真正安装，只是判断下是否能安装
  -U, --upgrade=<packagefile>+     升级软件包
  --reinstall=<packagefile>+       reinstall package(s)

所有 rpm 模式和可执行文件的通用选项：
  -D, --define=“MACRO EXPR”        定义值为 EXPR 的 MACRO
  --undefine=MACRO                 undefine MACRO
  -E, --eval=“EXPR”                打印 EXPR 的宏展开
  --macros=<FILE:…>                从文件 <FILE:...> 读取宏，不使用默认文件
  --noplugins                      don't enable any plugins
  --nodigest                       不校验软件包的摘要
  --nosignature                    不验证软件包签名
  --rcfile=<FILE:…>                从文件 <FILE:...> 读取宏，不使用默认文件
  -r, --root=ROOT                  使用 ROOT 作为顶级目录 (default: "/")
  --dbpath=DIRECTORY               使用 DIRECTORY 目录中的数据库
  --querytags                      显示已知的查询标签
  --showrc                         显示最终的 rpmrc 和宏配置
  --quiet                          提供更少的详细信息输出
  -v, --verbose                    提供更多的详细信息输出
  --version                        打印使用的 rpm 版本号

```

```shell
yum # Shell 前端软件包管理器,基于RPM包管理，能够从指定的服务器自动下载RPM包和安装，可以自动处理相对关系，并且一次安装所有依赖的软件包，无须繁琐地一次次下载，安装。
# 检查可更新的软件清单
yum check-update
# 更新所有软件
yum update
# 安装指定安装包
yum install 包名
# 更新指定软件包
yum update 包名
# 列出所有可安装软件
yum list
# 删除包
yum remove 包名
# 查找软件包
yum search 包名
# 软件包信息
yum info 包名

check          检查 RPM 数据库问题
check-update   检查是否有可用的软件包更新
clean          删除缓存数据
deplist        列出软件包的依赖关系
distribution-synchronization 已同步软件包到最新可用版本
downgrade      降级软件包
erase          从系统中移除一个或多个软件包
fs             Acts on the filesystem data of the host, mainly for removing docs/lanuages for minimal hosts.
fssnapshot     Creates filesystem snapshots, or lists/deletes current snapshots.
groups         显示或使用、组信息
help           显示用法提示
history        显示或使用事务历史
info           显示关于软件包或组的详细信息
install        向系统中安装一个或多个软件包
langavailable  Check available languages
langinfo       List languages information
langinstall    Install appropriate language packs for a language
langlist       List installed languages
langremove     Remove installed language packs for a language
list           列出一个或一组软件包
load-transaction 从文件名中加载一个已存事务
makecache      创建元数据缓存
provides       查找提供指定内容的软件包
reinstall      覆盖安装软件包
repo-pkgs      将一个源当作一个软件包组，这样我们就可以一次性安装/移除全部软件包。
repolist       显示已配置的源
search         在软件包详细信息中搜索指定字符串
shell          运行交互式的 yum shell
swap           Simple way to swap packages, instead of using shell
update         更新系统中的一个或多个软件包
update-minimal Works like upgrade, but goes to the 'newest' package match which fixes a problem that affects your system
updateinfo     Acts on repository update information
upgrade        更新软件包同时考虑软件包取代关系
version        显示机器和/或可用的源版本。

```

```shell
| #管道，将上一个命令的输出作为下一个命令的输入
cat /etc/passwd | grep root #显示 passwd 中 包含root 的。
ls /etc | grep ssh
```

```shell
echo # 输出
# 显示系统变量
echo ${JAVA_HOME}
```

```shell
free # 显示系统内存使用情况
# 显示便于理解的输出方式
free -h       #总          被使用        空闲       被共享     
              total        used        free      shared  buff/cache   available
Mem:# 内存     972M        200M        511M        7.7M        259M        625M
Swap: #交换空间 2.0G          0B        2.0G

```

```shell
ping
    # -c 指定次数后停止
    ping -c 2 www.baidu.com
```

### 十、关机 防火墙 systemctl

```shell
shutdown -h now 关闭系统
init 0 关闭系统
telinit 0 关闭系统
shutdown -h hours:minutes & 按预定时间关闭系统 
shutdown -c 取消按预定时间关闭系统 
shutdown -r now 重启
reboot 重启
logout 注销 
```

```shell
firewalld
启动： systemctl start firewalld
关闭： systemctl stop firewalld
查看状态： systemctl status firewalld 
开机禁用  ： systemctl disable firewalld
开机启用  ： systemctl enable firewalld


查看版本： firewall-cmd --version
查看帮助： firewall-cmd --help
显示状态： firewall-cmd --state
查看所有打开的端口： firewall-cmd --zone=public --list-ports
更新防火墙规则： firewall-cmd --reload
查看区域信息:  firewall-cmd --get-active-zones
查看指定接口所属区域： firewall-cmd --get-zone-of-interface=eth0
拒绝所有包：firewall-cmd --panic-on
取消拒绝状态： firewall-cmd --panic-off
查看是否拒绝： firewall-cmd --query-panic

firewall-cmd --zone=public --add-port=80/tcp --permanent #添加端口（--permanent表示永久生效，没有此参数重启后失效）
firewall-cmd --zone=public --query-port=80/tcp #查看防火墙80端口
firewall-cmd --zone=public --remove-port=80/tcp --permanent #删除防火墙80端口
```

```shell
systemctl
启动一个服务：systemctl start firewalld.service
关闭一个服务：systemctl stop firewalld.service
重启一个服务：systemctl restart firewalld.service
显示一个服务的状态：systemctl status firewalld.service
在开机时启用一个服务：systemctl enable firewalld.service
在开机时禁用一个服务：systemctl disable firewalld.service
查看服务是否开机启动：systemctl is-enabled firewalld.service
查看已启动的服务列表：systemctl list-unit-files|grep enabled
查看启动失败的服务列表：systemctl --failed
```

#### 十一、其他命令

###### unzip

1、把文件解压到当前目录下

```shell
unzip test.zip
```

2、如果要把文件解压到指定的目录下，需要用到-d参数。

```shell
unzip -d /temp test.zip
```

3、解压的时候，有时候不想覆盖已经存在的文件，那么可以加上-n参数

```shell
unzip -n test.zip
unzip -n -d /temp test.zip
```

4、只看一下zip压缩包中包含哪些文件，不进行解压缩

```shell
unzip -l test.zip
```

5、查看显示的文件列表还包含压缩比率

```shell
unzip -v test.zip
```

6、检查zip文件是否损坏

```shell
unzip -t test.zip
```

7、将压缩文件test.zip在指定目录tmp下解压缩，如果已有相同的文件存在，要求unzip命令覆盖原先的文件

```
unzip -o test.zip -d /tmp/
```

###### zip

1. 压缩目录，将/harry目录压缩为名为harry.zip压缩包. -r 递归压缩

```shell
zip -r harry.zip harry
# 可以压缩多个
zip -r harry.zip harry1 harry2 harry3
```

2. -q 安静模式输出，不显示压缩过程

```shell
zip -q -r harry.zip harry
```

3.  -x 排除指定目录下的指定文件,"./harry/ha.txt"。

```shell
zip -r harry.zip harry -x "./harry/ha.txt"
```

4. -m 压缩后删除源文件

```shell
zip -rm harry.zip harry
```

###### curl

用URL传输数据的命令行工具和库，获取url的指定数据

1. 查看所有用法

```shell
curl -help
```

2. 获取www.baidu.com

```shell
curl www.baidu.com
```

3. 获取响应头以及文本信息

```shell
curl -i www.baidu.com
```

4. 把输出写到该文件中，保留远程文件的文件名

```shell
curl -LO https://github.com/dockersamples/node-bulletin-board/archive/master.zip
```

