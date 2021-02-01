# hexo



[toc]



##### 简介

https://hexo.io/zh-cn/docs/

Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。基于node.js 的静态网页生成器

#### 安装

###### 安装前提

- Git
- Node.js( 12.0+版本)

###### 安装

1. install

```shell
$ npm install -g hexo-cli
```

2. 对于熟悉 npm 的进阶用户，可以仅局部安装 `hexo` 包。

```shell
$ npm install hexo
```

3. 配置环境变量

````json
C:\Program Files\nodejs\node_global\node_modules\hexo\bin
````

1. `npx hexo <command>`
2. 将 Hexo 所在的目录下的 `node_modules` 添加到环境变量之中即可直接使用 `hexo <command>`：

```shell
echo 'PATH="$PATH:./node_modules/.bin"' >> ~/.profile
```

#### 建博客

##### 1、初始化

```shell
# 初始化
$ hexo init <folder>
# 进入指定文件夹
$ cd <folder>
# npm
$ npm install
```

##### 2、 hexo目录

```shell
.
├── _config.yml
├── package.json
├── package-lock.json
├── .gitignore
├── node_modules
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

- _config.yml ：配置大部分的参数
- package.json：应用程序的信息。[EJS](https://ejs.co/), [Stylus](http://learnboost.github.io/stylus/) 和 [Markdown](http://daringfireball.net/projects/markdown/) renderer 已默认安装，您可以自由移除。
- scaffolds：模版文件夹。当您新建文章时，Hexo 会根据 scaffold 来建立文件
- source：资源文件夹是存放用户资源的地方。除 `_posts` 文件夹之外，开头命名为 `_` (下划线)的文件 / 文件夹和隐藏的文件将会被忽略。Markdown 和 HTML 文件会被解析并放到 `public` 文件夹，而其他文件会被拷贝过去。
- themes：主题文件夹。Hexo 会根据主题来生成静态页面。

##### 3、编译，启动服务

```shell
$ hexo generate

$ hexo server
INFO  Validating config
INFO  Start processing
INFO  Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.
INFO  Catch you later

# http://localhost:4000

```









#### 命令

###### hexo init

```shell
$ hexo init [folder]

# 新建一个网站。如果没有设置 folder ，Hexo 默认在目前的文件夹建立网站。
# 这一步Git克隆hexo-starter 到指定的文件夹（慢死）
```

###### hexo new

```shell
$ hexo new "post title with whitespace"

$ hexo new [layout] <title>

# 新建一篇文章。如果没有设置 layout 的话，默认使用 _config.yml 中的 default_layout 参数代替。如果标题包含空格的话，请使用引号括起来。

Options:
  -p, --path     自定义新文章的路径
  -r, --replace  如果存在同名文章，将其替换
  -s, --slug     文章的 Slug，作为新文章的文件名和发布后的 URL

```

###### hexo generate

```shell
# 生成静态文件
$ hexo generate

# 简写
$ hexo g

Options:
  -b, --bail         生成过程中如果发生任何未处理的异常则抛出异常
  -c, --concurrency  最大同时生成文件的数量，默认无限制
  -d, --deploy       文件生成后立即部署网站
  -f, --force        强制重新生成文件
Hexo 引入了差分机制，如果 public 目录存在，那么 hexo g 只会重新生成改动的文件。
使用该参数的效果接近 hexo clean && hexo generate
  -w, --watch        监视文件变动

```

###### hexo publish

```shell
# 发布草稿
$ hexo publish [layout] <filename>
```

###### hexo server

```shell
# 3.0 版本之后不内置server，需要手动安装
$ npm install hexo-server --save



# 启动服务器，默认情况下，访问网址为： http://localhost:4000/
$ hexo server


Options:
  -i, --ip            Override the default server IP. Bind to all IP address by default.
  -l, --log [format]  启动日记记录，使用覆盖记录格式
  -o, --open          Immediately open the server url in your default web browser.
  -p, --port          重设端口
  -s, --static        只使用静态文件

```

###### hexo deploy

```shell
# 部署网站。
$ hexo deploy

# 简写
$ hexo d

Options:
  --setup         Setup without deployment
  -g, --generate  部署之前预先生成静态文件

```

###### hexo render

```shell
# 渲染文件。
$ hexo render <file1> [file2] ...

Description:
Render files with renderer plugins (e.g. Markdown) and save them at the specified path.

Options:
  --engine  Specify render engine
  --output  	设置输出路径
  --pretty  Prettify JSON output
```

###### hexo migrate

```shell
# 从其他博客系统 迁移内容。
$ hexo migrate <type>

```

###### hexo clean

```shell
# 清除缓存文件 (db.json) 和已生成的静态文件 (public)。
$ hexo clean
```

###### hexo list

```shell
# 列出网站资料。
$ hexo list <type>


```

#### 部署到github

###### 1. 创建github用户

###### 2. 创建仓库

```java
名称只能为：用户名.github.io

例如 wushutong.github.io

```

###### 3. 本地生成git 的 ssh key 并安装到github

###### 4. 在git Bash 执行

```shell
# 安装部署插件
npm install hexo-deployer-git --save


```

###### 5.修改改_config.yml文件

```yml
deploy:
  type: git
  repository: git@github.com:wushutong/wushutong.github.io.git
  branch: master

```

###### 部署

```shell
hexo clean（清除缓存）
hexo g（上传到仓库）
hexo d（部署）

```

#### 写博客

###### 1.摘要

```html
手动，在摘要后面添加
<!--more--> 
```

###### 2.分类

其他菜单同理

```shell
# 创建分类页面
hexo new page categories

# 打开新增的页面，新增type
---
title: 文章分类
date: 2017-05-27 13:47:40
type: "categories"
---

# 对文章增加分类就行了
---
title: jQuery
date: 2017-05-26 12:12:57
categories: 
- web前端
---

# - web前端 多个横杠不会数组的意思，第二个代表的是二级分类
```

3.标签

```shell
# 创建标签页面
hexo new page tags

# 打开新增的页面，添加type
---
title: 文章分类
date: 2017-05-27 13:47:40
type: "tags"
---

# 给文章添加标签
---
title: jQuery对表单的操作及更多应用
date: 2017-05-26 12:12:57
categories: 
- web前端
tags:
- jQuery
- 表格
- 表单验证
---

# 标签可以添加多个
```



#### 主题

[HEXO主题选择地址](https://hexo.io/themes/)

[next主题使用说明](http://theme-next.iissnan.com/getting-started.html)

[主题使用说明最新](https://theme-next.js.org/docs/theme-settings/footer)

```json
https://www.yunyoujun.cn/note/make-hexo-theme-yun/
https://github.com/Siricee/hexo-theme-Chic
https://github.com/next-theme/hexo-theme-next
```



###### 1.将主题文件放到`themes`目录下

```shell
# 方式1.  git 直接拉取next 主题方式
$ git clone https://github.com/next-theme/hexo-theme-next themes/next

# 方式2. 手动 github 下载之后放在themes/next

```

###### 2.在`hexo/_config.yml`中修改使用的主题

```yml
# hexo/_config.yml (修改使用的主题，指定为theme/next)
theme: next

```

###### 3.添加主题备用配置文件

```yml
# 将主题的配置文件hexo/themes/next/_config.yml 复制到主目录

# 更名为 hexo/_config.next.yml

# 只有当配置了 theme: next，并且 备用配置文件名为 _config.next.yml 才可以被加载

```

###### 4.执行clean清除缓存

```shell
$ hexo clean

```





##### next 主题更多配置

- 主页设置

```yml
# hexo/_config.yml
archive_dir: /		# 存档


index_generator:
  path: ''			# 索引页路径
  per_page: 10		# 每页条数
  order_by: -date	# 排序字段

```





- 配置多语言

```yml
# 主配置文件 hexo/_config.yml （配置为支持中英文）
language: 
    - zh-CN
    - en

# 主题配置文件 hexo/_config.next.yml (配置语言切换)
language_switcher: true

```

- 升级

```shell
# 进入 next 主题文件夹
$ cd themes / next 
# pull
$ git pull origin master

```

- 



##### hexo 配置_config.yml



| 参数          | 描述                                                         |
| :------------ | :----------------------------------------------------------- |
| `title`       | 网站标题                                                     |
| `subtitle`    | 网站副标题                                                   |
| `description` | 网站描述                                                     |
| `keywords`    | 网站的关键词。支援多个关键词。                               |
| `author`      | 您的名字                                                     |
| `language`    | 网站使用的语言。对于简体中文用户来说，使用不同的主题可能需要设置成不同的值，请参考你的主题的文档自行设置，常见的有 `zh-Hans`和 `zh-CN`。 |
| `timezone`    | 网站时区。Hexo 默认使用您电脑的时区。请参考 [时区列表](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) 进行设置，如 `America/New_York`, `Japan`, 和 `UTC` 。一般的，对于中国大陆地区可以使用 `Asia/Shanghai`。 |

- 网站描述信息

```yml
# hexo/_config.yml
title: '主标题'
subtitle: '副标题'
description: '网站描述'
keywords: '关键词'
author: '夜月祈'
language:  		# 这里使用了多语言配置
    - zh-CN
    - en
timezone: ''	# 默认电脑的时区

```

- 网址信息



| 参数                         | 描述                                                         | 默认值                      |
| :--------------------------- | :----------------------------------------------------------- | :-------------------------- |
| `url`                        | 网址, must starts with `http://` or `https://`               |                             |
| `root`                       | 网站根目录                                                   |                             |
| `permalink`                  | 文章的 [永久链接](https://hexo.io/zh-cn/docs/permalinks) 格式 | `:year/:month/:day/:title/` |
| `permalink_defaults`         | 永久链接中各部分的默认值                                     |                             |
| `pretty_urls`                | 改写 [`permalink`](https://hexo.io/zh-cn/docs/variables) 的值来美化 URL |                             |
| `pretty_urls.trailing_index` | 是否在永久链接中保留尾部的 `index.html`，设置为 `false` 时去除 | `true`                      |
| `pretty_urls.trailing_html`  | 是否在永久链接中保留尾部的 `.html`, 设置为 `false` 时去除 (*对尾部的 `index.html`无效*) |                             |

```yml
url: http://example.com
root: /								# 如果网站存放在子目录中，例如 http://example.com/blog，则请将您的 url 设为 http://example.com/blog 并把 root 设为 /blog/
permalink: :year/:month/:day/:title/
permalink_defaults:
pretty_urls:
  trailing_index: true # Set to false to remove trailing 'index.html' from permalinks
  trailing_html: true # Set to false to remove trailing '.html' from permalinks

```

- 目录指定



| 参数           | 描述                                                         | 默认值           |
| :------------- | :----------------------------------------------------------- | :--------------- |
| `source_dir`   | 资源文件夹，这个文件夹用来存放内容。                         | `source`         |
| `public_dir`   | 公共文件夹，这个文件夹用于存放生成的站点文件。               | `public`         |
| `tag_dir`      | 标签文件夹                                                   | `tags`           |
| `archive_dir`  | 归档文件夹                                                   | `archives`       |
| `category_dir` | 分类文件夹                                                   | `categories`     |
| `code_dir`     | Include code 文件夹，`source_dir` 下的子目录                 | `downloads/code` |
| `i18n_dir`     | 国际化（i18n）文件夹                                         | `:lang`          |
| `skip_render`  | 跳过指定文件的渲染。匹配到的文件将会被不做改动地复制到 `public` 目录中。您可使用 [glob 表达式](https://github.com/micromatch/micromatch#extended-globbing)来匹配路径。 |                  |

```yml
source_dir: source
public_dir: public
tag_dir: tags
archive_dir: archives
category_dir: categories
code_dir: downloads/code
i18n_dir: :lang
skip_render: # 直接写相对于source文件夹的文件名，将在渲染时不被主题修改

```

- 写作规则



| 参数                    | 描述                                                         | 默认值    |
| :---------------------- | :----------------------------------------------------------- | :-------- |
| `new_post_name`         | 新文章的文件名称                                             | :title.md |
| `default_layout`        | 预设布局                                                     | post      |
| `auto_spacing`          | 在中文和英文之间加入空格                                     | false     |
| `titlecase`             | 把标题转换为 首字母大写                                      | false     |
| `external_link`         | 在新标签中打开链接                                           | true      |
| `external_link.enable`  | 在新标签中打开链接                                           | `true`    |
| `external_link.field`   | 对整个网站（`site`）生效或仅对文章（`post`）生效             | `site`    |
| `external_link.exclude` | 需要排除的域名。主域名和子域名如 `www` 需分别配置            | `[]`      |
| `filename_case`         | 把文件名称转换为 (1) 小写或 (2) 大写                         | 0         |
| `render_drafts`         | 显示草稿                                                     | false     |
| `post_asset_folder`     | 启动 [Asset 文件夹](https://hexo.io/zh-cn/docs/asset-folders) | false     |
| `relative_link`         | 把链接改为与根目录的相对位址                                 | false     |
| `future`                | 显示未来的文章                                               | true      |
| `highlight`             | 代码块的设置, see [Highlight.js](https://hexo.io/docs/syntax-highlight#Highlight-js) section for usage guide |           |
| `prismjs`               | 代码块的设置, see [PrismJS](https://hexo.io/docs/syntax-highlight#PrismJS) section for usage guide |           |

```yml
new_post_name: :title.md # File name of new posts
default_layout: post
titlecase: false # Transform title into titlecase
external_link:
  enable: true # Open external links in new tab
  field: site # Apply to the whole site
  exclude: ''
filename_case: 0
render_drafts: false
post_asset_folder: false
relative_link: false
future: true
highlight:
  enable: true
  line_number: true
  auto_detect: false
  tab_replace: ''
  wrap: true
  hljs: false
prismjs:
  enable: false
  preprocess: true
  line_number: true
  tab_replace: ''

```

- 分类&标签

| 参数               | 描述     | 默认值          |
| :----------------- | :------- | :-------------- |
| `default_category` | 默认分类 | `uncategorized` |
| `category_map`     | 分类别名 |                 |
| `tag_map`          | 标签别名 |                 |

```yml
default_category: uncategorized
category_map:
tag_map:

```

- 分页

| 参数             | 描述                                | 默认值 |
| :--------------- | :---------------------------------- | :----- |
| `per_page`       | 每页显示的文章量 (0 = 关闭分页功能) | `10`   |
| `pagination_dir` | 分页目录                            | `page` |

```yml
per_page: 10
pagination_dir: page

```

- 部署位置

```yml
deploy:
  type: git		# 类型
  repository: git@github.com:wushutong/wushutong.github.io.git	# 仓库
  branch: master	# 分支

```

- 其他

| 参数             | 描述                                                         |
| :--------------- | :----------------------------------------------------------- |
| `theme`          | 指定主题名称。值为`false`时禁用主题                          |
| `theme_config`   | 主题的配置文件。在这里放置的配置会覆盖主题目录下的 `_config.yml` 中的配置 |
| `meta_generator` | [Meta generator](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/meta#属性) 标签。 值为 `false` 时 Hexo 不会在头部插入该标签 |





###### Scheme 主题方案

```yml
# hexo/_config.next.yml
# Schemes
#scheme: Muse    	# 默认，很干净
#scheme: Mist		# 单列视图
#scheme: Pisces		# 双柱
scheme: Gemini		# 带阴影

```

###### 网站图标

```yml
# 默认图标放置在 hexo/themes/next/source/images/ 下面

# hexo/_config.next.yml  （后面跟的是相对于主题主配置文件 hexo/themes/next/source 的位置）
favicon:
  small: /images/favicon-16x16-next.png
  medium: /images/favicon-32x32-next.png
  apple_touch_icon: /images/apple-touch-icon-next.png
  safari_pinned_tab: /images/logo.svg
  #android_manifest: /manifest.json
  
# 生成图标网址，需要翻墙
https://realfavicongenerator.net/

```

###### Custom Logo 徽标

```yml
# hexo/_config.next.yml (Scheme Mist 不支持)
custom_logo: #/uploads/custom-logo.jpg


```

###### 侧边栏

```yml
# hexo/_config.next.yml
sidebar:
  # Sidebar Position. 	# 位置
  position: left 		# 左侧
  #position: right  	# 右侧

  # Manual define the sidebar width. If commented, will be default for:
  # Muse | Mist: 320
  # Pisces | Gemini: 240
  #width: 300			# 宽度

  # Sidebar Display (only for Muse | Mist), available values:
  #  - post    expand on posts automatically. Default. 	具有索引的帖子显示侧边栏
  #  - always  expand for all pages automatically.		所有页面
  #  - hide    expand only when click on the sidebar toggle icon.	隐藏
  #  - remove  totally remove sidebar including sidebar toggle.		删除
  display: post

  # Sidebar padding in pixels.		内边距
  padding: 18
  # Sidebar offset from top menubar in pixels (only for Pisces | Gemini). 偏移
  offset: 12

```

###### 头像

```yml
# hexo/_config.next.yml

avatar:
  # url 放置头像图片
  # 1. 如果图片放在了hexo/source/uploads/ 中，url：/uploads/avatar.gif
  # 2. 如果图片放在了主题目录source/images/中，url: /images/avatar.gif
  # 3. 直接使用网络图片
  url: #/images/avatar.gif
  # 头像是否圆边
  rounded: false
  # 头像将在鼠标悬停时旋转。
  rotated: false
```

###### 侧边栏帖子计数

```yml
# hexo/_config.next.yml
site_state: true
```

###### 社交软件链接

```yml
# hexo/_config.next.yml
# 链接名: 链接网址 || 图标
social:
  #GitHub: https://github.com/wushutong || fab fa-github
  #E-Mail: mailto:yourname@gmail.com || fa fa-envelope
  #Weibo: https://weibo.com/yourname || fab fa-weibo
  #Google: https://plus.google.com/yourname || fab fa-google
  #Twitter: https://twitter.com/yourname || fab fa-twitter
  #FB Page: https://www.facebook.com/yourname || fab fa-facebook
  #StackOverflow: https://stackoverflow.com/yourname || fab fa-stack-overflow
  #YouTube: https://youtube.com/yourname || fab fa-youtube
  #Instagram: https://instagram.com/yourname || fab fa-instagram
  #Skype: skype:yourname?call|chat || fab fa-skype
```

###### 社交图标效果

```yml
# hexo/_config.next.yml

social_icons:
  enable: true
  icons_only: false	#只显示图标
  transition: false
```

###### 侧边栏目录

```yml
# hexo/_config.next.yml
toc:
  enable: true	# 目录可用
  # 为目录添加编号
  number: true
  # 帖子标题过长
  wrap: false
  # 是否显示所有级别目录
  expand_all: false
  # 目录深度
  max_depth: 6
```

###### 添加自己喜欢的链接

```yml
# hexo/_config.next.yml
# 名称： 链接
links:
  #Title: https://example.com

```

###### 喜欢的链接样式控制

```yml
# hexo/_config.next.yml
links_settings:
  icon: fa fa-globe		# 图标
  title: Links			# 名称
  # Available values: block | inline
  layout: block			# 

```

###### 页脚

```yml
# hexo/_config.next.yml
footer:
  # 默认显示当前年，如果有 since 2019，效果为 2019 - 2020 
  #since: 2020

  # 年和作者中间的图标.
  icon:
    # 图标名称，在后面这个网站查找选择 See: https://fontawesome.com/icons
    name: fa fa-heart
    # 图标动画.
    animated: false
    # Change the color of icon, using Hex Code.
    color: "#ff0000"

  # If not defined, `author` from Hexo `_config.yml` will be used. 版权信息，默认显示作者名
  copyright:

  # Powered by Hexo & NexT 网站平台信息，Powered by Hexo & NexT
  powered: true

  # Beian ICP and gongan information for Chinese users. See: https://beian.miit.gov.cn, http://www.beian.gov.cn 网址备案信息
  beian:
    enable: false
    icp:
    # The digit in the num of gongan beian.
    gongan_id:
    # The full num of gongan beian.
    gongan_num:
    # The icon for gongan beian. See: http://www.beian.gov.cn/portal/download
    gongan_icon_url:

```

###### 菜单页面

```yml
# hexo/_config.next.yml
menu:
# 名称：相对于hexo/source/的路径 || 图标名
  home: /index/ || fa fa-home			# 首页
  about: /about/ || fa fa-user
  tags: /tags/ || fa fa-tags
  #categories: /categories/ || fa fa-th
  archives: /archives/ || fa fa-archive
  #schedule: /schedule/ || fa fa-calendar
  #sitemap: /sitemap.xml || fa fa-sitemap
  #commonweal: /404/ || fa fa-heartbeat

```







#### 问题



##### `WARN No layout: index.html`

主题有问题，配置问题或者无主题