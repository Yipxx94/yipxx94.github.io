# Hugo-在云服务上利用docker搭建博客与远程部署


<!--more-->


## Hugo简介


### 前言

[Hugo](https://gohugo.io/) 是一个基于 Go 语言开发的静态网站生成器（[SSG](https://jamstack.org/generators/)），目前由 [@bep](https://github.com/bep) 领衔开发，Hugo 的突出特点是简洁、灵活、高效，目前有很多知名网站都在使用 Hugo：[Netlify](https://www.netlify.com/)、[Let’s Encrypt](https://letsencrypt.org/)、[IPFS](https://ipfs.io/)、[Cloudflare Developers](https://developers.cloudflare.com/)、[DigitalOcean Docs](https://docs.digitalocean.com/products/)、[1Password](https://1password.com/) 等等。与目前国内流行的 Hexo 相比，Hugo 的速度可称为飞速——在安装和使用上都是如此。下面是官网的原话：

> **The world’s fastest framework for building websites**
>
> Hugo is one of the most popular open-source static site generators. With its amazing speed and flexibility, Hugo makes building websites fun again.


### 目录结构

```
├── archetypes/     # 文章模板          scaffolds/
├── assets/         # Hugo 管道
├── config.toml     # 配置文件          _config.yml
├── content/        # 文章目录          source/_posts/
├── data/           # Hugo 数据文件     source/_data/
├── layouts/        # 布局模板
├── public/         # 生成的静态文件     public/
├── resources/      # Hugo 缓存
├── static/         # 网站的静态文件     source/
└── themes/         # 主题目录          themes/
```

其中，`assets`、`data`、`layouts`、`static`的作用都是与站点目录下的相应文件夹相同的，且站点目录下的文件可以覆盖主题目录下的相应文件——这意味着你可以在不修改主题文件的前提下方便地定制主题。在 Hugo 中，如果你想要定制主题，你只需在站点目录下新建相应的文件即可。这是非常利于主题的维护的，你只需使用 Git 的`submodule`的方式安装 Hugo 的主题，然后更新时只需直接在站点根目录下敲一条命令回车即可，非常方便！

此外，上面的`i18n`文件夹相当于 Hexo 的主题中的`languages`文件夹，如果你不喜欢主题的一些文字翻译，可以在站点目录下新建相应文件自定义。这里特别需要提醒的是，Hugo 中的`data`和`i18n`文件夹下的所有文件都是可以按「键」覆盖的，即你无需复制文件全文，只需添加你想自定义的那项即可。


### 配置文件

Hugo 中是不区分站点和主题的配置文件的，Hugo 中只有一个位于站点根目录下的`config.toml`配置文件。你可能注意到`.toml`后缀，没错，Hugo 默认使用的配置文件是[TOML](https://github.com/toml-lang/toml)格式的，它的语法是非常简单易懂的，它在语法上也没有缩进的要求。当然，在 Hugo 中你也可使用 Hexo 默认的 YAML 格式。此外，如果你想将文章中的 Front Matter 也从 YAML 转换成 TOML 的话，推荐一个 [Python 脚本](https://github.com/wd/hexo2hugo)，但特别注意：尝试前务必先备份！


### 分类方式和组织方式

在 Hugo 中你是无法将文章的 Front Matter 中的`categories`用于文章的 URL 的。为什么呢？因为 Hugo 中的`categories`与`tags`在功能上其实是完全相同的，它们的作用都是将不同的文章联系起来。其实，Front Matter 中的`categories`和`tags`在 Hugo 中都属于 Taxonomies：

> **Hugo includes support for user-defined taxonomies to help you demonstrate logical relationships between content for the end users of your website.**
>
> Hugo 支持用户定义的类别（taxonomies）来帮你为自己网站的读者展示文章之间的逻辑关系。

在 Hugo 中其实你可以自定义自己想要的任何类别，而不仅限于部类（`categories`）和标签（`tags`），比如：你可以自定义一个`series`，也就是系列/专题/专栏。

既然 Hugo 中基于 Front Matter 的部类和标签都属于类别，而类别的作用只是联系文章，部类自然不可能有树状结构，自然也不能用部类来组织文章。那么，在 Hugo 中你要怎么组织文章呢？分区（[Sections](https://gohugo.io/content-management/sections/)）。所谓分区，即站点的`content`目录下的文件夹和子文件夹，一个文件夹即一个分区。很明显，这是基于文件系统的结构的，自然也就支持树状/网状/嵌套结构，也就能够用来实现文章的树状分类。我觉得这是 Hugo 的又一个优点，直接利用起文件系统的结构来组织文章，合理且符合用户预期，Markdown 文档的存放也更有序。


## docker镜像的拉取和容器的创建

因为Hugo的运行环境需要Go语言，所以这里直接拉取一个docker官方的golang镜像。Hugo默认的调试端口是1313，在云服务器的安全组中将1313端口打开。

1. 拉取镜像
```
docker pull golang
```

2. 创建一个容器并进行端口映射
```
docker run -p 1313:1313 --name myblog_server -itd golang:latest
```

3. 进入容器
```
docker attach myblog_server
```

4. 在容器里创建一个非根用户
```
adduser xxx
```

5. 为用户分配sudo权限(没有sudo的话要先安装sudo)
```
usermod -aG sudo xxx
```

6. 挂起容器（不能ctrl + d，这样会关闭容器）
```
ctrl + p,ctrl + q
```

配置环境变量：Linux下有两个文件可以配置环境变量，其中`/etc/profile`是对所有用户生效的；`$HOME/.profile`是对当前用户生效的，根据自己的情况自行选择一个文件打开，添加如下两行代码，保存退出。

```
export GOROOT=/usr/local/go
export PATH=$PATH:$GOROOT/bin
```

修改`/etc/profile`后要重启生效，修改`$HOME/.profile`后使用source命令加载`$HOME/.profile`文件即可生效。

Tips：在服务器和容器间复制文件，要使用完整的路径。如，

```
docker cp /home/yipxx/hugo-image.png myblog_server:/home/myblog/myblog/static/images
```


## Hugo的安装

推荐使用 Hugo extended 版本，由于某些主题的一些特性需要将 SCSS 转换为 CSS，推荐使用 Hugo extended 版本来获得更好的使用体验。

```
wget https://github.com/gohugoio/hugo/releases/download/v0.90.1/hugo_extended_0.90.1_Linux-64bit.tar.gz
tar -zxvf hugo_extended_0.90.1_Linux-64bit.tar.gz
sudo mv hugo /usr/bin/
```

检查是否安装成功

```
hugo version
```


## 创建Blog站点

首先需要创建一个新的个人站点

```
hugo new site blog
```

`blog`就是你的博客站点所在的目录，也是这个站点的根目录，创建站点后目录结构如下：

```
archetypes/
content/
data/
layouts/
static/
themes/
config.toml
```

下面简单介绍下Hugo根目录下的各个文件目录的作用：

* `archetypes`存放创建文件时使用的模板，可以自定义`front matter`属性。
* `assets`存放需要被`Hugo Pipes`处理的文件，且只有使用了`.Permalink`或者`.RelPermalink`的文件才能被发布到`public`目录。 **注意，默认不会创建assets目录**。
* `config`是配置文件，可以有`JSON`、`YAML`或者`TOML`三种格式，默认使用根目录下的`config.toml`、`config.yaml`或`config.json`中的某一个。可以通过`--config`来配置读取一个或多个配置文件，如：`hugo --config a.toml,b.toml,c.toml`。**注意，默认不会创建`config`目录**。
* `content`存放的各种md文件用于部署站点，该目录下可以自行创建若干个子目录来便于对文章进行分类，这些子目录被称为`section`。
* `data`目录存放的是用于定义变量的模板文件，相当于Java里的常量类，这些文件有`JSON`、`YAML`或者`TOML`三种格式，会在生成站点时被使用到。一般用不到该功能，具体用法可以参考：[data templates](https://gohugo.io/templates/data-templates/)。
* `layouts`目录存放的模板文件用于渲染`html`页面，模板里可以定义不同页面的`html`代码。
* `static`目录存放的是静态内容：图片、CSS、JavaScript等。
* `resources`目录用于缓存某些文件来提高生成效率。**注意，默认不会创建resources目录**。


## 添加主题

为新站点添加一个主题，以我使用的LoveIt主题为例。

```
cd blog
git init
git submodule add https://github.com/dillonzq/LoveIt.git themes/LoveIt
```

然后，修改`config.toml`：

```
theme = "LoveIt"
```

这里的`LoveIt`对应`themes`目录下的主题的文件夹名字。


## 添加文章

新建一篇文章

```
hugo new posts/my-first-post.md
```

该命令会在`content/posts`目录下生成`my-first-post.md`文件，打开进行编辑：

```
---
title: "First"
date: 2021-12-12T21:57:28+08:00
draft: true
---
## First

Hello World!!!
```

两行`---`里的属性是`front matter`，用来设置当前文章的属性配置。`front matter`的内容可以使用3种不同的格式来定义，两行`---`之间对应的是`YAML`格式，两行`+++`之间对应的是`TOML`格式，`{`和`}`之间对应的是`JSON`格式。

`draft`（草稿）不会被部署；完成帖子后，更新帖子的标题以显示`draft：false`。

下面是官方文档提供的3种不同格式的`front matter`的样例，有兴趣的可以了解下。

**TOML Example**

```
+++
title = "spf13-vim 3.0 release and new website"
description = "spf13-vim is a cross platform distribution of vim plugins and resources for Vim."
tags = [ ".vimrc", "plugins", "spf13-vim", "vim" ]
date = "2012-04-06"
categories = [
    "Development",
    "VIM"
]
slug = "spf13-vim-3-0-release-and-new-website"
+++
Content of the file goes Here
```

**YAML Example**

```
---
title: "spf13-vim 3.0 release and new website"
description: "spf13-vim is a cross platform distribution of vim plugins and resources for Vim."
tags: [ ".vimrc", "plugins", "spf13-vim", "vim" ]
lastmod: 2015-12-23
date: "2012-04-06"
categories:
- "Development"
- "VIM"
slug: "spf13-vim-3-0-release-and-new-website"
---

Content of the file goes Here
```

**JSON Example**

```
{
"title": "spf13-vim 3.0 release and new website",
    "description": "spf13-vim is a cross platform distribution of vim plugins and resources for Vim.",
    "tags": [ ".vimrc", "plugins", "spf13-vim", "vim" ],
    "date": "2012-04-06",
    "categories": [
        "Development",
    "VIM"
    ],
    "slug": "spf13-vim-3-0-release-and-new-website",
}

Content of the file goes Here
```


## 启动Hugo服务

利用本地的浏览器访问云服务器上的个人站点来进行调试。

```
hugo server --bind "0.0.0.0" -b "http://ip:1313"

开启评论系统,CDN和fingerprint
hugo server --bind "0.0.0.0" -b "http://ip:1313" -e production
```

该命令仅用于本地调试，支持热修改，也就是说在启动服务时修改文章会实时生效，但是该命令不会真正生成静态文件。

此时，已经直接可以通过`http://ip:1313`来访问该站点。


## 生成静态页面

输入命令：

```
hugo -D
```

默认会在站点根目录的`public/`目录下生成对应的静态页面，可以通过在命令行指定`-d`或者`--destination`参数来改变静态页面的存放路径，也可以通过在配置文件中设置`publishDir`来指定。

该命令生成的静态页面文件是用来部署到`pages`服务的，比如`GitHub pages`或者`Coding pages`等。

另外，hugo允许对生成的静态页面设置特殊的参数，比如在文章的`front matter`里设置参数：`draft`,`publishdate`和`expirydate`。如下：

```
---
title: "First"
date: 2020-09-08T21:57:28+08:00
draft: true
publishdate: 2020-09-18T21:57:28+08:00
expirydate: 2020-09-28T21:57:28+08:00
---
```

`draft: true`表明该文章是草稿，如果在启用服务时不指定参数`-D`或`--buildDrafts`，或者在配置文件`config.toml`中配置`buildDrafts = true`，则会在生成文章时忽略草稿。如果不想指定该参数就生成文章，需要改为`draft: false`或者将其删去。

`publishdate: 2020-09-18T21:57:28+08:00`表示将来发布的时间，如果不指定参数`-F`或`--buildFuture`，或者在配置文件`config.toml`中配置`buildFuture = true`，则无法在规定的日期之前生成该文章。

`expirydate: 2020-09-28T21:57:28+08:00`表示过期时间，如果不指定参数`-E`或`--buildExpired`，或者在配置文件`config.toml`中配置`buildExpired = true`，则无法在规定的日期之后生成该文章。


## 远程部署到Github Pages服务

Hugo和Hexo一样是静态站点生成工具，不需要服务器即可进行部署运行，为了可以在网络上也访问到我们的博客，需要将静态博客部署到某些网站的pages服务上，借用人家的服务器进行托管。

### 在github上创建一个仓库

首先在GitHub上创建一个仓库，仓库的名字格式为`<username>.github.io`。

之所以这样规定命名，是因为GitHub默认会把`<username>.github.io`的`master`分支的内容部署到GitHub Pages站点上。

### SSH key的创建与配置

```
创建密钥
ssh-keygen
```

执行结束后，`~/.ssh/`目录下会多两个文件：

`id_rsa`：私钥

`id_rsa.pub`：公钥

然后将公钥配置到GitHub账号上。

### 在本地关联GitHub的站点仓库

1. 进入public
```
cd public
```

2. 创建git项目
```
git init
```

3. git全局设置
```
git config --global user.name "xxx"
git config --global user.email "xxx@xxx.com"
```

4. 将当前文件上传
```
git add .
```

5. 将当前文件提价
```
git commit -m "my first post"
```

6. 推送现有的git仓库
```
git remote add origin https://github.com/xxx/<username>.github.io.git
```

7. 推送到github
```
git branch -M master
git push -u origin master
```



