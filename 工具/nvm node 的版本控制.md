# nvm windows下 的node版本控制

[Releases · coreybutler/nvm-windows · GitHub](https://github.com/coreybutler/nvm-windows/releases)

nvm install 版本号：安装指定版本的node（例如安装8.0.0版本->nvm install 8.0.0或者nvm install v8.0.0）
nvm ls：查看本地已经安装的node所有版本
nvm use 版本号：切换到指定版本的node环境，前提要系统已经安装了node该版本。


npm rebuild node-sass

一.  

      8.11以上版本的node版本对应的npm都没法自动安装，
      需要自己到npm官网( https://npm.taobao.org/mirrors/npm/)
      下载手动安装对应的npm版本
      1、进入nvm下的指定的node版本目录发现目录下没有npm运行文件
      2、进入node_modules发现文件夹为空
      3、自己下载node对应的npm版本
      4、解压后将文件夹重命名为npm并复制到node_modules目录下
      5、最后将npm中bin目录下的npm以及npm.cmd复制到与node_modules目录同级目录下
    
      vue-cli脚手架的一个bug，原因是chromedriver的部分文件被国内网络给墙掉了，无法下载完整的chromedirver包，解决办法：npm install chromedriver --chromedriver_cdnurl=http://cdn.npm.taobao.org/dist/chromedriver