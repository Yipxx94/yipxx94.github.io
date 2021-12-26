# Hugo的文件配置和博客功能增强


<!--more-->


## 前言

Hugo本身可以通过修改站点配置文件来完成页面的部分定制，如按钮、多语言等功能。本博客使用的是Hugo的**LoveIt**主题，本文也是基于该主题而写的，不过Hugo的美化步骤应该大同小异。


## 配置文件

Hugo默认使用根目录下的`config.toml`、`config.yaml`或`config.json`中的某一个作为站点的配置文件，可以通过`--config`来配置读取一个或多个配置文件。

```
hugo --config debugconfig.toml
hugo --config a.toml,b.toml,c.toml
```

除了[Hugo 全局配置](https://gohugo.io/getting-started/configuration/)之外，**LoveIt**主题还允许您在网站配置中定义以下参数 (这是一个示例`config.toml`，其内容为默认值)。

**`config.toml`**

```
baseURL = "http://example.org/"
# [en, zh-cn, fr, ...] 设置默认的语言
defaultContentLanguage = "zh-cn"
# 网站语言, 仅在这里 CN 大写
languageCode = "zh-CN"
# 是否包括中日韩文字
hasCJKLanguage = true
# 网站标题
title = "我的全新 Hugo 网站"

# 更改使用 Hugo 构建网站时使用的默认主题
theme = "LoveIt"

[params]
  # LoveIt 主题版本
  version = "0.2.X"

[menu]
  [[menu.main]]
    identifier = "posts"
    # 你可以在名称 (允许 HTML 格式) 之前添加其他信息, 例如图标
    pre = ""
    # 你可以在名称 (允许 HTML 格式) 之后添加其他信息, 例如图标
    post = ""
    name = "文章"
    url = "/posts/"
    # 当你将鼠标悬停在此菜单链接上时, 将显示的标题
    title = ""
    weight = 1
  [[menu.main]]
    identifier = "tags"
    pre = ""
    post = ""
    name = "标签"
    url = "/tags/"
    title = ""
    weight = 2
  [[menu.main]]
    identifier = "categories"
    pre = ""
    post = ""
    name = "分类"
    url = "/categories/"
    title = ""
    weight = 3

# Hugo 解析文档的配置
[markup]
  # 语法高亮设置 (https://gohugo.io/content-management/syntax-highlighting)
  [markup.highlight]
    # false 是必要的设置 (https://github.com/dillonzq/LoveIt/issues/158)
    noClasses = false

# [en, zh-cn, fr, pl, ...] 设置默认的语言
defaultContentLanguage = "zh-cn"

[languages]
  [languages.en]
    weight = 1
    title = "My New Hugo Site"
    languageCode = "en"
    languageName = "English"
    [[languages.en.menu.main]]
      identifier = "posts"
      pre = ""
      post = ""
      name = "Posts"
      url = "/posts/"
      title = ""
      weight = 1
    [[languages.en.menu.main]]
      identifier = "tags"
      pre = ""
      post = ""
      name = "Tags"
      url = "/tags/"
      title = ""
      weight = 2
    [[languages.en.menu.main]]
      identifier = "categories"
      pre = ""
      post = ""
      name = "Categories"
      url = "/categories/"
      title = ""
      weight = 3

  [languages.zh-cn]
    weight = 2
    title = "我的全新 Hugo 网站"
    # 网站语言, 仅在这里 CN 大写
    languageCode = "zh-CN"
    languageName = "简体中文"
    # 是否包括中日韩文字
    hasCJKLanguage = true
    [[languages.zh-cn.menu.main]]
      identifier = "posts"
      pre = ""
      post = ""
      name = "文章"
      url = "/posts/"
      title = ""
      weight = 1
    [[languages.zh-cn.menu.main]]
      identifier = "tags"
      pre = ""
      post = ""
      name = "标签"
      url = "/tags/"
      title = ""
      weight = 2
    [[languages.zh-cn.menu.main]]
      identifier = "categories"
      pre = ""
      post = ""
      name = "分类"
      url = "/categories/"
      title = ""
      weight = 3

  [languages.fr]
    weight = 3
    title = "Mon nouveau site Hugo"
    languageCode = "fr"
    languageName = "Français"
    [[languages.fr.menu.main]]
      identifier = "posts"
      pre = ""
      post = ""
      name = "Postes"
      url = "/posts/"
      title = ""
      weight = 1
    [[languages.fr.menu.main]]
      identifier = "tags"
      pre = ""
      post = ""
      name = "Balises"
      url = "/tags/"
      title = ""
      weight = 2
    [[languages.fr.menu.main]]
      identifier = "categories"
      pre = ""
      post = ""
      name = "Catégories"
      url = "/categories/"
      title = ""
      weight = 3

[params]
  #  LoveIt 主题版本
  version = "0.2.X"
  # 网站描述
  description = "这是我的全新 Hugo 网站"
  # 网站关键词
  keywords = ["Theme", "Hugo"]
  # 网站默认主题样式 ("light", "dark", "auto")
  defaultTheme = "auto"
  # 公共 git 仓库路径，仅在 enableGitInfo 设为 true 时有效
  gitRepo = ""
  #  哪种哈希函数用来 SRI, 为空时表示不使用 SRI
  # ("sha256", "sha384", "sha512", "md5")
  fingerprint = ""
  #  日期格式
  dateFormat = "2006-01-02"
  # 网站图片, 用于 Open Graph 和 Twitter Cards
  images = ["/logo.png"]

  #  应用图标配置
  [params.app]
    # 当添加到 iOS 主屏幕或者 Android 启动器时的标题, 覆盖默认标题
    title = "LoveIt"
    # 是否隐藏网站图标资源链接
    noFavicon = false
    # 更现代的 SVG 网站图标, 可替代旧的 .png 和 .ico 文件
    svgFavicon = ""
    # Android 浏览器主题色
    themeColor = "#ffffff"
    # Safari 图标颜色
    iconColor = "#5bbad5"
    # Windows v8-10磁贴颜色
    tileColor = "#da532c"

  #  搜索配置
  [params.search]
    enable = true
    # 搜索引擎的类型 ("lunr", "algolia")
    type = "lunr"
    # 文章内容最长索引长度
    contentLength = 4000
    # 搜索框的占位提示语
    placeholder = ""
    #  最大结果数目
    maxResultLength = 10
    #  结果内容片段长度
    snippetLength = 50
    #  搜索结果中高亮部分的 HTML 标签
    highlightTag = "em"
    #  是否在搜索索引中使用基于 baseURL 的绝对路径
    absoluteURL = false
    [params.search.algolia]
      index = ""
      appID = ""
      searchKey = ""

  # 页面头部导航栏配置
  [params.header]
    # 桌面端导航栏模式 ("fixed", "normal", "auto")
    desktopMode = "fixed"
    # 移动端导航栏模式 ("fixed", "normal", "auto")
    mobileMode = "auto"
    #  页面头部导航栏标题配置
    [params.header.title]
      # LOGO 的 URL
      logo = ""
      # 标题名称
      name = ""
      # 你可以在名称 (允许 HTML 格式) 之前添加其他信息, 例如图标
      pre = ""
      # 你可以在名称 (允许 HTML 格式) 之后添加其他信息, 例如图标
      post = ""
      #  是否为标题显示打字机动画
      typeit = false

  # 页面底部信息配置
  [params.footer]
    enable = true
    #  自定义内容 (支持 HTML 格式)
    custom = ''
    #  是否显示 Hugo 和主题信息
    hugo = true
    #  是否显示版权信息
    copyright = true
    #  是否显示作者
    author = true
    # 网站创立年份
    since = 2019
    # ICP 备案信息，仅在中国使用 (支持 HTML 格式)
    icp = ""
    # 许可协议信息 (支持 HTML 格式)
    license = '<a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>'

  #  Section (所有文章) 页面配置
  [params.section]
    # section 页面每页显示文章数量
    paginate = 20
    # 日期格式 (月和日)
    dateFormat = "01-02"
    # RSS 文章数目
    rss = 10

  #  List (目录或标签) 页面配置
  [params.list]
    # list 页面每页显示文章数量
    paginate = 20
    # 日期格式 (月和日)
    dateFormat = "01-02"
    # RSS 文章数目
    rss = 10

  # 主页配置
  [params.home]
    #  RSS 文章数目
    rss = 10
    # 主页个人信息
    [params.home.profile]
      enable = true
      # Gravatar 邮箱，用于优先在主页显示的头像
      gravatarEmail = ""
      # 主页显示头像的 URL
      avatarURL = "/images/avatar.png"
      #  主页显示的网站标题 (支持 HTML 格式)
      title = ""
      # 主页显示的网站副标题
      subtitle = "这是我的全新 Hugo 网站"
      # 是否为副标题显示打字机动画
      typeit = true
      # 是否显示社交账号
      social = true
      #  免责声明 (支持 HTML 格式)
      disclaimer = ""
    # 主页文章列表
    [params.home.posts]
      enable = true
      # 主页每页显示文章数量
      paginate = 6
      #  被 params.page 中的 hiddenFromHomePage 替代
      # 当你没有在文章前置参数中设置 "hiddenFromHomePage" 时的默认行为
      defaultHiddenFromHomePage = false

  # 作者的社交信息设置
  [params.social]
    GitHub = "xxxx"
    Linkedin = ""
    Twitter = "xxxx"
    Instagram = "xxxx"
    Facebook = "xxxx"
    Telegram = "xxxx"
    Medium = ""
    Gitlab = ""
    Youtubelegacy = ""
    Youtubecustom = ""
    Youtubechannel = ""
    Tumblr = ""
    Quora = ""
    Keybase = ""
    Pinterest = ""
    Reddit = ""
    Codepen = ""
    FreeCodeCamp = ""
    Bitbucket = ""
    Stackoverflow = ""
    Weibo = ""
    Odnoklassniki = ""
    VK = ""
    Flickr = ""
    Xing = ""
    Snapchat = ""
    Soundcloud = ""
    Spotify = ""
    Bandcamp = ""
    Paypal = ""
    Fivehundredpx = ""
    Mix = ""
    Goodreads = ""
    Lastfm = ""
    Foursquare = ""
    Hackernews = ""
    Kickstarter = ""
    Patreon = ""
    Steam = ""
    Twitch = ""
    Strava = ""
    Skype = ""
    Whatsapp = ""
    Zhihu = ""
    Douban = ""
    Angellist = ""
    Slidershare = ""
    Jsfiddle = ""
    Deviantart = ""
    Behance = ""
    Dribbble = ""
    Wordpress = ""
    Vine = ""
    Googlescholar = ""
    Researchgate = ""
    Mastodon = ""
    Thingiverse = ""
    Devto = ""
    Gitea = ""
    XMPP = ""
    Matrix = ""
    Bilibili = ""
    Email = "xxxx@xxxx.com"
    RSS = true # 

  #  文章页面配置
  [params.page]
    #  是否在主页隐藏一篇文章
    hiddenFromHomePage = false
    #  是否在搜索结果中隐藏一篇文章
    hiddenFromSearch = false
    #  是否使用 twemoji
    twemoji = false
    # 是否使用 lightgallery
    lightgallery = false
    #  是否使用 ruby 扩展语法
    ruby = true
    #  是否使用 fraction 扩展语法
    fraction = true
    #  是否使用 fontawesome 扩展语法
    fontawesome = true
    # 是否在文章页面显示原始 Markdown 文档链接
    linkToMarkdown = true
    #  是否在 RSS 中显示全文内容
    rssFullText = false
    #  目录配置
    [params.page.toc]
      # 是否使用目录
      enable = true
      #  是否保持使用文章前面的静态目录
      keepStatic = true
      # 是否使侧边目录自动折叠展开
      auto = true
    #  代码配置
    [params.page.code]
      # 是否显示代码块的复制按钮
      copy = true
      # 默认展开显示的代码行数
      maxShownLines = 10
    #  KaTeX 数学公式
    [params.page.math]
      enable = true
      # 默认块定界符是 $$ ... $$ 和 \\[ ... \\]
      blockLeftDelimiter = ""
      blockRightDelimiter = ""
      # 默认行内定界符是 $ ... $ 和 \\( ... \\)
      inlineLeftDelimiter = ""
      inlineRightDelimiter = ""
      # KaTeX 插件 copy_tex
      copyTex = true
      # KaTeX 插件 mhchem
      mhchem = true
    #  Mapbox GL JS 配置
    [params.page.mapbox]
      # Mapbox GL JS 的 access token
      accessToken = ""
      # 浅色主题的地图样式
      lightStyle = "mapbox://styles/mapbox/light-v9"
      # 深色主题的地图样式
      darkStyle = "mapbox://styles/mapbox/dark-v9"
      # 是否添加 NavigationControl
      navigation = true
      # 是否添加 GeolocateControl
      geolocate = true
      # 是否添加 ScaleControl
      scale = true
      # 是否添加 FullscreenControl
      fullscreen = true
    #  文章页面的分享信息设置
    [params.page.share]
      enable = true
      Twitter = true
      Facebook = true
      Linkedin = false
      Whatsapp = true
      Pinterest = false
      Tumblr = false
      HackerNews = false
      Reddit = false
      VK = false
      Buffer = false
      Xing = false
      Line = true
      Instapaper = false
      Pocket = false
      Digg = false
      Stumbleupon = false
      Flipboard = false
      Weibo = true
      Renren = false
      Myspace = true
      Blogger = true
      Baidu = false
      Odnoklassniki = false
      Evernote = true
      Skype = false
      Trello = false
      Mix = false
    #  评论系统设置
    [params.page.comment]
      enable = true
      # Disqus 评论系统设置
      [params.page.comment.disqus]
        # 
        enable = false
        # Disqus 的 shortname，用来在文章中启用 Disqus 评论系统
        shortname = ""
      # Gitalk 评论系统设置
      [params.page.comment.gitalk]
        # 
        enable = false
        owner = ""
        repo = ""
        clientId = ""
        clientSecret = ""
      # Valine 评论系统设置
      [params.page.comment.valine]
        enable = false
        appId = ""
        appKey = ""
        placeholder = ""
        avatar = "mp"
        meta= ""
        pageSize = 10
        lang = ""
        visitor = true
        recordIP = true
        highlight = true
        enableQQ = false
        serverURLs = ""
        #  emoji 数据文件名称, 默认是 "google.yml"
        # ("apple.yml", "google.yml", "facebook.yml", "twitter.yml")
        # 位于 "themes/LoveIt/assets/data/emoji/" 目录
        # 可以在你的项目下相同路径存放你自己的数据文件:
        # "assets/data/emoji/"
        emoji = ""
      # Facebook 评论系统设置
      [params.page.comment.facebook]
        enable = false
        width = "100%"
        numPosts = 10
        appId = ""
        languageCode = "zh_CN"
      #  Telegram Comments 评论系统设置
      [params.page.comment.telegram]
        enable = false
        siteID = ""
        limit = 5
        height = ""
        color = ""
        colorful = true
        dislikes = false
        outlined = false
      #  Commento 评论系统设置
      [params.page.comment.commento]
        enable = false
      #  Utterances 评论系统设置
      [params.page.comment.utterances]
        enable = false
        # owner/repo
        repo = ""
        issueTerm = "pathname"
        label = ""
        lightTheme = "github-light"
        darkTheme = "github-dark"
    #  第三方库配置
    [params.page.library]
      [params.page.library.css]
        # someCSS = "some.css"
        # 位于 "assets/"
        # 或者
        # someCSS = "https://cdn.example.com/some.css"
      [params.page.library.js]
        # someJavascript = "some.js"
        # 位于 "assets/"
        # 或者
        # someJavascript = "https://cdn.example.com/some.js"
    #  页面 SEO 配置
    [params.page.seo]
      # 图片 URL
      images = []
      # 出版者信息
      [params.page.seo.publisher]
        name = ""
        logoUrl = ""

  #  TypeIt 配置
  [params.typeit]
    # 每一步的打字速度 (单位是毫秒)
    speed = 100
    # 光标的闪烁速度 (单位是毫秒)
    cursorSpeed = 1000
    # 光标的字符 (支持 HTML 格式)
    cursorChar = "|"
    # 打字结束之后光标的持续时间 (单位是毫秒, "-1" 代表无限大)
    duration = -1

  # 网站验证代码，用于 Google/Bing/Yandex/Pinterest/Baidu
  [params.verification]
    google = ""
    bing = ""
    yandex = ""
    pinterest = ""
    baidu = ""

  #  网站 SEO 配置
  [params.seo]
    # 图片 URL
    image = ""
    # 缩略图 URL
    thumbnailUrl = ""

  #  网站分析配置
  [params.analytics]
    enable = false
    # Google Analytics
    [params.analytics.google]
      id = ""
      # 是否匿名化用户 IP
      anonymizeIP = true
    # Fathom Analytics
    [params.analytics.fathom]
      id = ""
      # 自行托管追踪器时的主机路径
      server = ""

  #  Cookie 许可配置
  [params.cookieconsent]
    enable = true
    # 用于 Cookie 许可横幅的文本字符串
    [params.cookieconsent.content]
      message = ""
      dismiss = ""
      link = ""

  #  第三方库文件的 CDN 设置
  [params.cdn]
    # CDN 数据文件名称, 默认不启用
    # ("jsdelivr.yml")
    # 位于 "themes/LoveIt/assets/data/cdn/" 目录
    # 可以在你的项目下相同路径存放你自己的数据文件:
    # "assets/data/cdn/"
    data = ""

  #  兼容性设置
  [params.compatibility]
    # 是否使用 Polyfill.io 来兼容旧式浏览器
    polyfill = false
    # 是否使用 object-fit-images 来兼容旧式浏览器
    objectFit = false

# Hugo 解析文档的配置
[markup]
  # 语法高亮设置
  [markup.highlight]
    codeFences = true
    guessSyntax = true
    lineNos = true
    lineNumbersInTable = true
    # false 是必要的设置
    # (https://github.com/dillonzq/LoveIt/issues/158)
    noClasses = false
  # Goldmark 是 Hugo 0.60 以来的默认 Markdown 解析库
  [markup.goldmark]
    [markup.goldmark.extensions]
      definitionList = true
      footnote = true
      linkify = true
      strikethrough = true
      table = true
      taskList = true
      typographer = true
    [markup.goldmark.renderer]
      # 是否在文档中直接使用 HTML 标签
      unsafe = true
  # 目录设置
  [markup.tableOfContents]
    startLevel = 2
    endLevel = 6

# 作者配置
[author]
  name = "xxxx"
  email = ""
  link = ""

# 网站地图配置
[sitemap]
  changefreq = "weekly"
  filename = "sitemap.xml"
  priority = 0.5

# Permalinks 配置
[Permalinks]
  # posts = ":year/:month/:filename"
  posts = ":filename"

# 隐私信息配置
[privacy]
  #  Google Analytics 相关隐私 (被 params.analytics.google 替代)
  [privacy.googleAnalytics]
    # ...
  [privacy.twitter]
    enableDNT = true
  [privacy.youtube]
    privacyEnhanced = true

# 用于输出 Markdown 格式文档的设置
[mediaTypes]
  [mediaTypes."text/plain"]
    suffixes = ["md"]

# 用于输出 Markdown 格式文档的设置
[outputFormats.MarkDown]
  mediaType = "text/plain"
  isPlainText = true
  isHTML = false

# 用于 Hugo 输出文档的设置
[outputs]
  # 
  home = ["HTML", "RSS", "JSON"]
  page = ["HTML", "MarkDown"]
  section = ["HTML", "RSS"]
  taxonomy = ["HTML", "RSS"]
  taxonomyTerm = ["HTML"]
```


## 添加自定义的`_custom.scss`

`LoveIt`主题提供了一个自定义的`_custom.scss`，可以在该文件里添加自定义的`css`样式。该文件目录位于`\themes\LoveIt\assets\css\_custom.scss`，不建议直接在该文件里写`css`代码。

Hugo在渲染页面时优先读取站点根目录下的同名字的目录和文件，所以可以利用这个特点来美化主题。只需要把想修改的主题模板文件拷贝到根目录下同样的目录中并进行修改，这样就可以在不改动原本的主题文件的情况下实现主题美化。

首先在站点根目录下创建一个自定义的文件：`\assets\css\_custom.scss`，这样Hugo就会最终以该文件来渲染页面的样式。

**`_custom.scss`**

```
// ==============================
// Custom style
// 自定义样式
// ==============================
/* Custom styles. */

/* 滚动条 */
::-webkit-scrollbar {
    width: 1rem;
    height: .5rem;
}

/* 右下角按钮 */
.fixed-button {
    margin-bottom: 5rem;
}

/* 图片 */
figcaption {
    display: none !important;
}

img[data-sizes="auto"] {
    display: block;
/*    width: 50%;*/
}


/* 菜单栏 */
#header-desktop .header-wrapper {
    padding: 0 2rem 0 5vh;
}
#header-desktop .header-wrapper .menu .menu-item {
    margin: 0 .3rem;
}

.menu .menu-item i {
    margin-right: 2px;
}

/* 子菜单栏 */
.dropdown {
  display: inline-block;
}

/* 子菜单的内容 (默认隐藏) */
.dropdown-content {
  display: none;
  position: absolute;
  background-color: #f9f9f9;
  box-shadow: 0px 8px 16px 0px rgba(0, 0, 0, 0.2);
  border-radius: 4px;
  line-height: 1.3rem;
}

/* 子菜单的链接 */
.dropdown-content a {
  padding: 10px 18px 10px 14px;
  text-decoration: none;
  display: block;
  & i {
    margin-right: 3px;
  }
}

/* 鼠标移上去后修改子菜单链接颜色 */
.dropdown-content a:hover {
  background-color: #f1f1f1;
  border-radius: 4px;
}

/* 在鼠标移上去后显示子菜单 */
.dropdown:hover .dropdown-content {
  display: block;
}

@media screen and (max-width: 680px) {
    .dropdown {
      display: inline;
    }
  .dropdown:hover .dropdown-content {
    display: inline;
    z-index: 1;
    margin-top: -2em;
    margin-left: 3em;
  }
  .dropdown-content a:hover {
    background-color: transparent;
  }
}


/* Github Corner */
.github-corner:hover .octo-arm {
	animation: octocat-wave 560ms ease-in-out
}

@keyframes octocat-wave {
	0%,100% {
		transform: rotate(0)
	}

	20%,60% {
		transform: rotate(-25deg)
	}

	40%,80% {
		transform: rotate(10deg)
	}
}

@media (max-width:500px) {
	.github-corner:hover .octo-arm {
		animation: none
	}

	.github-corner .octo-arm {
		animation: octocat-wave 560ms ease-in-out
	}
}

/* 头像旋转 */
.home .home-profile .home-avatar img {
    width: 5rem;

  /* 设置循环动画
  [animation: 
	(play)动画名称
	(2s)动画播放时长单位秒或微秒
	(ease-out)动画播放的速度曲线为以低速结束 
	(1s)等待1秒然后开始动画
	(1)动画播放次数(infinite为循环播放) ]*/
 
  /* 鼠标经过头像旋转360度 */
  -webkit-transition: -webkit-transform 1.0s ease-out;
  -moz-transition: -moz-transform 1.0s ease-out;
  transition: transform 1.0s ease-out;
    &:hover {
      /* 鼠标经过停止头像旋转 
      -webkit-animation-play-state:paused;
      animation-play-state:paused;*/

      /* 鼠标经过头像旋转360度 */
      -webkit-transform: rotateZ(360deg);
      -moz-transform: rotateZ(360deg);
      transform: rotateZ(360deg);
    }
}
/* Z 轴旋转动画 */
@-webkit-keyframes play {
  0% {
    -webkit-transform: rotateZ(0deg);
  }
  100% {
    -webkit-transform: rotateZ(-360deg);
  }
}
@-moz-keyframes play {
  0% {
    -moz-transform: rotateZ(0deg);
  }
  100% {
    -moz-transform: rotateZ(-360deg);
  }
}
@keyframes play {
  0% {
    transform: rotateZ(0deg);
  }
  100% {
    transform: rotateZ(-360deg);
  }
}


/* 首页头部 */
.home[posts] .home-profile {
    padding-top: 0;
}

.home-avatar {
    padding-top: 2rem !important;
}

.home-profile {
    margin-left: -1rem;
    margin-right: -1rem;
    background: white;
	opacity: .85;
}
[theme=dark] .home-profile {
    background: #3a3535;
	opacity: .8;
}

.home .home-profile .home-title {
    margin-top: .67em;
}

/* 首页文章摘要 */
.home[posts] .summary {
	margin-bottom: .1rem;
    margin-left: -1rem;
    margin-right: -1rem;
	padding-left: 1rem;
	padding-right: 1rem;
    background: white;
	opacity: .95;
}
[theme=dark] .home[posts] .summary {
    background: #3a3535;
}

/* 首页的分页页码 */
.pagination {
    margin-top: .1rem;
    margin-bottom: 0;
    margin-left: -1rem;
    margin-right: 1rem;
	padding: 1rem 2rem .5rem 0;
    background: white;
	opacity: .95;
}
[theme=dark] .pagination {
    background: #3a3535;
}

/* 首页的阅读全文按钮 */
.single.summary .post-footer a:first-child {
    position: relative;
    z-index: 1;
    &::before {
      content: '';
      position: absolute;
      z-index: -1;
      top: 0;
      bottom: 0;
      left: -0.25em;
      right: -0.25em;
      background-color: hsla(341, 97%, 59%, 0.75);
      transform-origin: center right;
      transform: scaleX(0);
      transition: transform 0.2s ease-in-out;
    }

    &:hover::before {
      transform: scaleX(1);
      transform-origin: center left;
    }
}


/* 字体 */
.home[posts] .summary .content {
    color: #161209;
}
[theme=dark] .home[posts] .summary .content {
    color: #949494;
}
[theme=dark] .single .post-meta {
    color: #7e7979;
}

.single .single-title {
	font-size: 1.8rem;
}


/* 文章正文 */
.page.single .content p {
    margin: 1rem 0;
    padding: 0 1rem;
}

/* 文章脚部 */
.page.single .post-footer {
    margin-top: 0;
    margin-bottom: 1rem;
}

/* 文章元数据meta */
.post-meta .post-meta-line:nth-child(2) i:nth-child(1) {
    margin-left: 0;
}
.post-meta .post-meta-line:nth-child(2) i {
    margin-left: 0.3rem;
}
.post-meta .post-meta-line:nth-child(2) span i {
    margin-left: 0.3rem !important;
}
.post-meta a#post-meta-vcount {
    color: #a9a9b3;
    &:hover {
        color: #2d96bd;
    }
}


/* 文章的标签 */
.post-tags a {
    position: relative;
    &::before, &::after {
      content: '';
      position: absolute;
      left: 0;
      right: 0;
      height: 2px;
      background-color: #fc2f70;
      transform: scaleX(0);
      transition: transform 0.5s ease;
    }

    &::before {
      top: 0;
      transform-origin: center right;
    }

    &:hover::before {
      transform-origin: center left;
      transform: scaleX(1);
    }

    &::after {
      bottom: 0;
      transform-origin: center left;
    }

    &:hover::after {
      transform-origin: center right;
      transform: scaleX(1);
    }

}


/* 页面脚部 */
.footer {
    display: block;
	border-top-width: 3px;
    border-top-style: solid;
    border-top-color: #a166ab;
    position: relative;
    z-index: -1;
    max-width: 800px;
    width: 60%;
    margin: .1rem auto 0 auto;
	padding-left: 1rem;
	padding-right: 1rem;
    background: white;
	opacity: .95;
}
[theme=dark] .footer {
    background: #3a3535;
}

@media only screen and (max-width: 1440px) {
    .footer {
        width:54.5%
    }
}

@media only screen and (max-width: 1200px) {
    .footer {
        width:50.5%
    }
}

@media only screen and (max-width: 960px) {
    .footer {
        width: 77%
    }
}

@media only screen and (max-width: 680px) {
    .footer {
        width: 95%
    }
}


/* 目录 */
.toc {
    background: white;
	opacity: .95;
}
[theme=dark] .toc {
    background: #3a3535;
}
[theme=dark] #toc-auto {
    border-left-color: #6f6a6a;
}

.toc .toc-content {
    font-size: .8rem;
}

.toc .toc-content code {
    border: none;
    color: #f7ab01;
    font-size: 1em;
}

nav#TableOfContents ol {
    padding-inline-start: 30px;

    & ol {
        padding-inline-start: 15px;
        font-size: .75rem;
        display: none;
    }

    & li.has-active ol {
        display: block;
    }
}

/* 分隔线 */
hr {
    border: none;
    border-bottom: 2px dashed #7a7a7a !important;
}

/* 标题 */
.page.single h2 {
    box-shadow: rgb(95, 90, 75) 0px 0px 0px 1px, rgba(10, 10, 0, 0.5) 1px 1px 6px 1px;
    color: rgb(255, 255, 255);
    font-family: 微软雅黑, 宋体, 黑体, Arial;
    font-weight: bold;
    line-height: 1.3;
    text-shadow: rgb(34, 34, 34) 2px 2px 3px;
    background: rgb(43, 102, 149);
    border-radius: 6px;
    border-width: initial;
    border-style: none;
    border-color: initial;
    border-image: initial;
    padding: 7px;
    margin: 18px 0px 18px -5px !important;
}


/* 打赏 */
article .post-reward {
    margin-top: 20px;
    padding-top: 10px;
    text-align: center;
    border-top: 1px dashed #e6e6e6
}

article .post-reward .reward-button {
    margin: 15px 0;
    padding: 3px 7px;
    display: inline-block;
    color: #c05b4d;
    border: 1px solid #c05b4d;
    border-radius: 5px;
    cursor: pointer
}

article .post-reward .reward-button:hover {
    color: #fefefe;
    background-color: #c05b4d;
    transition: .5s
}

article .post-reward #reward:checked~.qr-code {
    display: block
}

article .post-reward #reward:checked~.reward-button {
    display: none
}

article .post-reward .qr-code {
    display: none
}

article .post-reward .qr-code .qr-code-image {
    display: inline-block;
    min-width: 200px;
    width: 40%;
    margin-top: 15px
}

article .post-reward .qr-code .qr-code-image span {
    display: inline-block;
    width: 100%;
    margin: 8px 0
}

article .post-reward .qr-code .image {
    width: 200px;
    height: 200px
}


/* 评论 */
.v[data-class=v] .veditor {
    background-size: contain !important;
    background-repeat: no-repeat !important;
    background-position: right !important;
    font-weight: bold;
    opacity: 0.75;
    color: black;
}

.v[data-class=v] .vcards .vcard .vh {
    border-bottom: 1px dashed #ccb5b5 !important;
}

.single #comments {
    padding: 4rem 0 0 0;
}

/* 评论表情包 */
.v[data-class='v'] .vcontent img[src^="https://cdn.jsdelivr.net/gh/lewky/lewky.github.io@master/images/emoji"] {
  width: 45px !important;
  margin: 0.25em;
  vertical-align: bottom !important;
}
.v[data-class='v'] .vcontent .vemoji {
  width: 45px !important;
  margin: 0.25em;
  vertical-align: bottom !important;
}


/* 归档、标签、分类、特殊页面 */
.page.archive, .page.single, .page.single.special {
	padding-left: 1rem;
	padding-right: 1rem;
    padding-bottom: 1rem;
    background: white;	
	opacity: .95;
}
[theme=dark] .page.archive,
[theme=dark] .page.single,
[theme=dark] .page.single.special {
    background: #3a3535;
}

.archive-item-date2 {
    color: #a9a9b3;
}

[theme=dark] .archive .archive-item-date {
    color: #a9a9b3;
}

.page.single.special .single-title.animated.pulse.faster {
    padding-right: 1rem;
}

.archive .card-item-title a sup {
    color: #a9a9b3;
	font-weight: initial;
}

.archive .group-title sup {
    color: #a9a9b3;
	font-weight: initial;
}

.archive .single-title.animated.pulse.faster sup {
	margin-left: .4rem;
    color: #a9a9b3;
	font-weight: initial;
}

[theme=dark] .archive .tag-cloud-tags a sup {
    color: #a9a9b3;
}

/* 分类页面 */
.archive .categories-card {
    margin-top: 1rem;

    & .card-item {
        margin-top: 1rem;
    }
}

/* 行内代码块 */
code {
    margin: 0 .2rem;
    font-size: .9em;
    border: 1px solid #d6d6d6;
    border-radius: .2rem;
}

/* 预格式代码块(用tab键插入的代码块) */
pre code {
    margin: 0;
    border: none;
    font-size: .875rem;
}

/* 标题里的代码块样式 */
.page.single .content>h2 code {
    color: #f7ab01;
    background: transparent !important;
    border: none;
}
```

### 注意

只有使用的是扩展版本的Hugo，才能令`_custom.scss`文件生效！！！

请查看你所使用的Hugo版本，如果不是`hugo_extended`版本，请前往[Hugo Release](https://github.com/gohugoio/hugo/releases)页面下载你当前版本Hugo所对应的`hugo_extended`版本。


## 添加自定义的`custom.js`

LoveIt主题并没有提供一个文件来让我们自定义`JavaScript`，所以需要自己创建一个`js`文件来自定义`JavaScript`。

首先在站点根目录下创建一个自定义的`JavaScript`文件：`\static\js\custom.js`。这个文件需要在`body`的闭合标签之前引入，并且要在`theme.min.js`的引入顺序之后。这样可以防止样式被其他文件覆盖，并且不会因为`JavaScript`文件假装太久导致页面长时间的空白。

对于LoveIt主题，`custom.js`添加在`\themes\LoveIt\layouts\partials\assets.html`里。

首先把该文件拷贝到根目录下的`\layouts\partials\assets.html`，然后打开拷贝后的文件，把自定义的`JavaScript`文件添加到最末尾的`{{- partial "plugin/analytics.html" . -}}`的上一行：

```
{{- /* 自定义的js文件 */ -}}
<script type="text/javascript" src="/js/custom.js"></script>
```

如果有其他的`JavaScript`文件要引入，加在一样的地方就可以，但是要放在自定义的`custom.js`之前。

**`custom.js`**

```
/* 返回随机颜色 */
function randomColor() {
	return "rgb("+~~(255*Math.random())+","+~~(255*Math.random())+","+~~(255*Math.random())+")";
}

/* 鼠标点击文字特效 */
var a_idx = 0;
var a_click = 1;
var a = new Array("道可道", "非常道", "名可名", "非常名", "虚其心", "实其腹", "和其光" ,"同其尘", "居善地", "心善渊", "少则得", "敝则新",
    "上善若水", "知人者智", "自知者明", "道生一", "一生二", "二生三", "三生万物",
    "人法地", "地法天", "天法道", "道法自然", "大直若曲", "大巧若拙", "大辩若讷", "合抱之木",
    "生于毫末", "九层之台", "起于累土", "千里之行", "始于足下", "无为",
    "无我", "无欲", "居下", "清虚", "自然", "万物之始", "大道至简", "衍化至繁",
    "澹兮其若海", "旷兮其若谷", "致虚极", "守静笃", "善行无辙迹",
    "知其白", "守其黑", "知其荣", "守其辱", "道常无为而无不为",
    "上德无为而无以为", "万物得一以生", "清静为天下正", "圣人不病", "以其病病", "夫惟病病", "是以不病",
    "一曰慈", "二曰俭", "三曰不敢为天下先", "勇于敢则杀");
jQuery(document).ready(function($) {
    $("body").click(function(e) {
		/* 点击频率，点击几次就换文字 */
		var frequency = 2;
		if (a_click % frequency === 0) {
			
			var $i = $("<span/>").text(a[a_idx]);
			a_idx = (a_idx + 1) % a.length;
			var x = e.pageX,
			y = e.pageY;
			$i.css({
				"z-index": 9999,
				"top": y - 20,
				"left": x,
				"position": "absolute",
				"font-weight": "bold",
				"color": randomColor(),
				"-webkit-user-select": "none",
				"-moz-user-select": "none",
				"-ms-user-select": "none",
				"user-select": "none"
			});
			$("body").append($i);
			$i.animate({
				"top": y - 180,
				"opacity": 0
			},
			1500,
			function() {
				$i.remove();
			});
			
		}
	a_click ++;
		
    });
});

	/* 评论框加载背景图片 */
	$(".v[data-class=v] .veditor").attr('style', "background-image: url(" + $cdnPrefix + "/images/common/valinebg.webp) !important;");
});

function getCurrentDateString() {
	var now = new Date();
	var month = now.getMonth() + 1;
	var day = now.getDate();
	var hour = now.getHours();
	return "" + now.getFullYear() + (month < 10 ? "0" + month : month) + (day < 10 ? "0" + day : day) + (hour < 10 ? "0" + hour : hour);
}

/* 站点运行时间 */
function runtime() {
	window.setTimeout("runtime()", 1000);
	/* 请修改这里的起始时间 */
    let startTime = new Date('12/10/2021 21:00:00');
    let endTime = new Date();
    let usedTime = endTime - startTime;
    let days = Math.floor(usedTime / (24 * 3600 * 1000));
    let leavel = usedTime % (24 * 3600 * 1000);
    let hours = Math.floor(leavel / (3600 * 1000));
    let leavel2 = leavel % (3600 * 1000);
    let minutes = Math.floor(leavel2 / (60 * 1000));
    let leavel3 = leavel2 % (60 * 1000);
    let seconds = Math.floor(leavel3 / (1000));
    let runbox = document.getElementById('run-time');
    runbox.innerHTML = '本站已运行<i class="far fa-clock fa-fw"></i> '
        + ((days < 10) ? '0' : '') + days + ' 天 '
        + ((hours < 10) ? '0' : '') + hours + ' 时 '
        + ((minutes < 10) ? '0' : '') + minutes + ' 分 '
        + ((seconds < 10) ? '0' : '') + seconds + ' 秒 ';
}
runtime();
```


## 添加全局的`CDN`变量

对于一些静态资源，比如图片、音乐文件、第三方库等，如果有自己的cdn或者图床等，可以在站点配置文件自定义一个`cdn`变量，如下：

```
[params]
  # cdn变量，末尾不需要加/
  cdnPrefix = "http://xxxx"
```

默认的 CDN 数据文件位于`themes/LoveIt/assets/data/cdn/`目录，可以在你的项目下相同路径存放你自己的数据文件：`assets/data/cdn/`。接下来就可以在你需要的地方使用该变量，大概有如下3种用法。

### 在md文件中使用

在md文件中可以用内置的**shortcodes**来使用该变量。

### 在模板文件中使用

在layouts目录下有很多html文件，这些是用来渲染站点的模板文件，可以用Hugo的语法来引入该变量：

```
{{ .Site.Params.cdnPrefix }}
```

如果在一个模板文件里有多个地方使用到该变量，可以定义一个局部变量来使用：

```
{{- $cdn := .Site.Params.cdnPrefix -}}

/* 使用局部变量 */
{{ $cdn }}
```

### 在JavaScript文件中使用

由于JavaScript文件中不能使用上述的**shortcodes**或Hugo语法来引用变量，只能通过在`\layouts\partials\assets.html`中用JavaScript语法来引入该变量，如下：

```
/* 这是可以应用于JavaScript文件的全局变量 */
<script>
	/* cdn for some static resources */
	var $cdnPrefix = {{ .Site.Params.cdnPrefix }};
</script>
```

这样就可以在其他被引入的JavaScript文件中使用这个`$cdnPrefix`变量：

```
$(function () {
	$.backstretch([
		  $cdnPrefix + "/images/background/saber1.jpg"
	], { duration: 60000, fade: 1500 });
});
```

如果是想在模板文件里引入某个自定义的JavaScript文件，如下：

```
{{- /* 自定义的js文件 */ -}}
<script type="text/javascript" src="{{ .Site.Params.cdnPrefix }}/js/custom.js"></script>
```


## Valine评论功能添加

1. 把`\themes\LoveIt\layouts\partials\comment.html`拷贝到站点根目录下的`\layouts\partials\comment.html`。

2. 打开拷贝后的`comment.html`，找到`Valine`相关的代码，把`{{- if $valine.enable -}}`和`{{- end -}}`之间的代码改成如下：

   ```
   {{- if $valine.enable -}}
       <div id="valine" class="comment"></div>
       <script src="//unpkg.com/valine@latest/dist/Valine.min.js"></script>
   
   	<script type="text/javascript">
   		new Valine({
   			el: '#valine' ,
   			appId: '{{ $valine.appId }}',
   			appKey: '{{ $valine.appKey }}',
   			notify: '{{ $valine.notify }}', 
   			verify: '{{ $valine.verify }}', 
   			avatar:'{{ $valine.avatar }}', 
   			placeholder: '{{ $valine.placeholder }}',
   			visitor: '{{ $valine.visitor }}'
   		});
   	</script>
   {{- end -}}
   ```

之后在站点配置文件里启用`valine`，然后填上从`LeanCloud`的应用中得到的`appId`和`appKey`就可以用了。并且在使用了`valine`的同时，还可以顺带启用阅读次数的统计功能。

### LeanCloud的使用

1. 在`LeanCloud`官网注册账号；
2. 创建一个应用，然后进入该应用的配置，选择设置 -> 应用凭证，然后复制该应用的`AppId`和`AppKey`到站点配置文件里;

3. 选择设置 -> 安全中心，在Web安全域名中输入站点域名，或`http://localhost:1313`；
4. 选择数据存储 -> 结构化数据，创建两个新的`Class`，`Comment`（评论会存在这里）和`Counter`（阅读次数统计）。


## 添加站点运行时间

将`\themes\LoveIt\layouts\partials\footer.html`拷贝到`\layouts\partials\footer.html`，打开拷贝后的文件，在`<div class="footer-container">`的下方添加如下代码：

```
<div class="footer-line">
	<span id="run-time"></span>
</div>
```

然后在`custom.js`中添加如下代码：

```
/* 站点运行时间 */
function runtime() {
	window.setTimeout("runtime()", 1000);
	/* 请修改这里的起始时间 */
    let startTime = new Date('12/10/2021 21:00:00');
    let endTime = new Date();
    let usedTime = endTime - startTime;
    let days = Math.floor(usedTime / (24 * 3600 * 1000));
    let leavel = usedTime % (24 * 3600 * 1000);
    let hours = Math.floor(leavel / (3600 * 1000));
    let leavel2 = leavel % (3600 * 1000);
    let minutes = Math.floor(leavel2 / (60 * 1000));
    let leavel3 = leavel2 % (60 * 1000);
    let seconds = Math.floor(leavel3 / (1000));
    let runbox = document.getElementById('run-time');
    runbox.innerHTML = '本站已运行<i class="far fa-clock fa-fw"></i> '
        + ((days < 10) ? '0' : '') + days + ' 天 '
        + ((hours < 10) ? '0' : '') + hours + ' 时 '
        + ((minutes < 10) ? '0' : '') + minutes + ' 分 '
        + ((seconds < 10) ? '0' : '') + seconds + ' 秒 ';
}
runtime();
```


## 添加不蒜子网站计数

本站只在页面底部增加了网站的总访问量，打开`\layouts\partials\footer.html`，在最底部`{{- end -}}`之前添加如下代码：

```
   本站访问量
   <div>
           <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
           <i class="fa fa-user"></i>  <span id="busuanzi_value_site_pv"></span>|
           <i class="fa fa-eye"></i> <span id="busuanzi_value_site_uv"></span>人次
    </div>
```

如果需要更多的网站统计功能，可参考[不蒜子博客](http://ibruce.info/2015/04/04/busuanzi/)。


## 主题自带的admonition样式

`LoveIt`提供了`admonition`**shortcode**，支持**12**种样式，可以在页面中插入提示的横幅。代码如下：

```
{{< admonition >}}
一个 **注意** 横幅
{{< /admonition >}}

{{< admonition abstract >}}
一个 **摘要** 横幅
{{< /admonition >}}

{{< admonition info >}}
一个 **信息** 横幅
{{< /admonition >}}

{{< admonition tip >}}
一个 **技巧** 横幅
{{< /admonition >}}

{{< admonition success >}}
一个 **成功** 横幅
{{< /admonition >}}

{{< admonition question >}}
一个 **问题** 横幅
{{< /admonition >}}

{{< admonition warning >}}
一个 **警告** 横幅
{{< /admonition >}}

{{< admonition failure >}}
一个 **失败** 横幅
{{< /admonition >}}

{{< admonition danger >}}
一个 **危险** 横幅
{{< /admonition >}}

{{< admonition bug >}}
一个 **Bug** 横幅
{{< /admonition >}}

{{< admonition example >}}
一个 **示例** 横幅
{{< /admonition >}}

{{< admonition quote >}}
一个 **引用** 横幅
{{< /admonition >}}
```

效果如下：

{{< admonition >}}
一个 **注意** 横幅
{{< /admonition >}}

{{< admonition abstract >}}
一个 **摘要** 横幅
{{< /admonition >}}

{{< admonition info >}}
一个 **信息** 横幅
{{< /admonition >}}

{{< admonition tip >}}
一个 **技巧** 横幅
{{< /admonition >}}

{{< admonition success >}}
一个 **成功** 横幅
{{< /admonition >}}

{{< admonition question >}}
一个 **问题** 横幅
{{< /admonition >}}

{{< admonition warning >}}
一个 **警告** 横幅
{{< /admonition >}}

{{< admonition failure >}}
一个 **失败** 横幅
{{< /admonition >}}

{{< admonition danger >}}
一个 **危险** 横幅
{{< /admonition >}}

{{< admonition bug >}}
一个 **Bug** 横幅
{{< /admonition >}}

{{< admonition example >}}
一个 **示例** 横幅
{{< /admonition >}}

{{< admonition quote >}}
一个 **引用** 横幅
{{< /admonition >}}



