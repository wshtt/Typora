# npm



[toc]



**简介：** npm(node package manager) node 的包管理工具，使用npm  管理多个版本的代码和代码依赖项 。npm 也是一个包的共享平台，可以在其中分享和使用js 包。



#### 命令

###### npm init

```shell
# 初始化 package.json 文件，在当前目录生成 package.json 文件
npm init

# package.json，是node.js 的包管理文件，通过它来管理复杂的依赖项
```

###### npm install xxx

```shell
# 安装指定版本的包 xxx
npm install xxx
npm i xxx  # 简写

# 安装当前目录下的 package.json 管理的依赖包
npm install

# 安装cnpm 库为淘宝npm 库
# 安装cnpm; -g 全局; registry 注册表;
npm install cnpm -g --registry=https://registry.npm.taobao.org
```

###### npm uninstall xxx

```shell
npm uninstall xxx
npm un
# 卸载指定的包
```

###### npm update xxx

```shell
# 更新指定包
npm update xxx
```











































###### npm install chromedriver

因为国内网络下载不了`chromedriver`（浏览器驱动）

`npm install chromedriver --chromedriver_cdnurl=http://cdn.npm.taobao.org/dist/chromedriver`

