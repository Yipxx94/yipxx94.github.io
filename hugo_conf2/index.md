# Hugo的文件配置和博客功能增强（二）


<!--more-->


## 添加全局的`CDN`变量

对于一些静态资源，比如图片、音乐文件、第三方库等，如果有自己的cdn或者图床等，可以在站点配置文件自定义一个`cdn`变量，如下：

```
[params]
  # cdn变量，末尾不需要加/
  cdnPrefix = "http://xxxx"
```

默认的 CDN 数据文件位于`themes/LoveIt/assets/data/cdn/`目录，可以在你的项目下相同路径存放你自己的数据文件：`assets/data/cdn/`。接下来就可以在你需要的地方使用该变量，大概有如下3种用法。

### 在md文件中使用

在md文件中可以用内置的**shortcodes**来使用该变量：

```
{{</* param cdnPrefix */>}}

![avatar.jpg]({{</* param cdnPrefix */>}}/images/avatar.jpg)
```

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

在根目录的`config.toml`文件中加入如下代码：

```
[params]
...
# 以下为需添加内容 #
# busuanzi
busuanzi = true
busuanzi_site_offset = 100000
```

打开`\layouts\partials\footer.html`，添加如下代码：

```
{{- if ne .Site.Params.footer.enable false -}}
    <footer class="footer">
        <div class="footer-container">
            ...
            <!-- 以下为需添加内容 -->
			<script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js">
			</script>
            <span id="busuanzi_container_site_pv">
                本站访问量：<span id="busuanzi_value_site_pv"></span>次
            </span> | 
            <span id="busuanzi_container_site_uv">
                您是本站第 <span id="busuanzi_value_site_uv"></span> 位访问者
            </span>
            <!-- 以上为需添加内容 -->
        </div>
    </footer>
{{- end -}}
```

如果需要更多的网站统计功能，可参考[不蒜子博客](http://ibruce.info/2015/04/04/busuanzi/)。

## 主题自带的admonition样式

`LoveIt`提供了`admonition`**shortcode**，支持**12**种样式，可以在页面中插入提示的横幅。代码如下：

```
{{</* admonition */>}}
一个 **注意** 横幅
{{</* /admonition */>}}

{{</* admonition abstract */>}}
一个 **摘要** 横幅
{{</* /admonition */>}}

{{</* admonition info */>}}
一个 **信息** 横幅
{{</* /admonition */>}}

{{</* admonition tip */>}}
一个 **技巧** 横幅
{{</* /admonition */>}}

{{</* admonition success */>}}
一个 **成功** 横幅
{{</* /admonition */>}}

{{</* admonition question */>}}
一个 **问题** 横幅
{{</* /admonition */>}}

{{</* admonition warning */>}}
一个 **警告** 横幅
{{</* /admonition */>}}

{{</* admonition failure */>}}
一个 **失败** 横幅
{{</* /admonition */>}}

{{</* admonition danger */>}}
一个 **危险** 横幅
{{</* /admonition */>}}

{{</* admonition bug */>}}
一个 **Bug** 横幅
{{</* /admonition */>}}

{{</* admonition example */>}}
一个 **示例** 横幅
{{</* /admonition */>}}

{{</* admonition quote */>}}
一个 **引用** 横幅
{{</* /admonition */>}}
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

## 为图标添加动画效果

```
<div class="fa-3x">
  <i class="fas fa-spinner fa-spin"></i>
  <i class="fas fa-circle-notch fa-spin"></i>
  <i class="fas fa-sync fa-spin"></i>
  <i class="fas fa-cog fa-spin"></i>
  <i class="fas fa-spinner fa-pulse"></i>
  <i class="fas fa-stroopwafel fa-spin"></i>
</div>
```

## 添加文章打赏功能

### 配置文件添加相关变量

在`config.toml`添加如下变量：

```
  [params.reward]                           # 文章打赏
    enable = true
    wechat = "/images/wechat.png"           # 微信二维码
    alipay = "/images/alipay.png"           # 支付宝二维码
```

这里是对全局文章生效，也可以在每篇文章的文件头里添加如下变量来控制是否启用该功能：

```
reward: false
```

### 新建模板文件`reward.html`

新建模板文件`/layouts/partials/single/reward.html`，内容如下：

```
{{ if or .Params.reward (and .Site.Params.reward.enable (ne .Params.reward false)) -}}
<div class="post-reward">
  <input type="checkbox" name="reward" id="reward" hidden />
  <label class="reward-button" for="reward">{{ T "reward" }}</label>
  <div class="qr-code">
    {{ $qrCode := .Site.Params.reward }}
	{{- $cdnPrefix := .Site.Params.cdnPrefix -}}
    {{ with $qrCode.wechat -}}
      <label class="qr-code-image" for="reward">
        <img class="image" src="{{ $cdnPrefix }}{{ . }}">
        <span>{{ T "rewardWechat" }}</span>
      </label>
    {{- end }}
    {{ with $qrCode.alipay -}}
      <label class="qr-code-image" for="reward">
        <img class="image" src="{{ $cdnPrefix }}{{ . }}">
        <span>{{ T "rewardAlipay" }}</span>
      </label>
    {{- end }}
  </div>
</div>
{{- end }}
```

### 修改模板文件

将`/themes/LoveIt/layouts/posts/single.html`拷贝到`/layouts/posts/single.html`，打开拷贝后的文件，找到如下内容：

```
{{- /* Content */ -}}
<div class="content" id="content">
    {{- dict "Content" .Content "Ruby" $params.ruby "Fraction" $params.fraction "Fontawesome" $params.fontawesome | partial "function/content.html" | safeHTML -}}
</div>
```

修改成如下：

```
{{- /* Content */ -}}
<div class="content" id="content">
    {{- dict "Content" .Content "Ruby" $params.ruby "Fraction" $params.fraction "Fontawesome" $params.fontawesome | partial "function/content.html" | safeHTML -}}

	{{- /* Reward */ -}}
	{{- partial "single/reward.html" . -}}
</div>
```

### 添加CSS代码

在自定义的`_custom.scss`里添加如下样式：

```
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
```

## 菜单栏支持子菜单

### 修改模板代码

将主题的`/themes/LoveIt/layouts/partials/header.html`拷贝一份到`layouts/partials/header.html`，找到如下代码，一共有两个地方，分别对应**网页端**和**手机端**：

```
{{- range .Site.Menus.main -}}
    {{- $url := .URL | relLangURL -}}
    {{- with .Page -}}
        {{- $url = .RelPermalink -}}
    {{- end -}}
    <a class="menu-item{{ if $.IsMenuCurrent `main` . | or ($.HasMenuCurrent `main` .) | or (eq $.RelPermalink $url) }} active{{ end }}" href="{{ $url }}"{{ with .Title }} title="{{ . }}"{{ end }}{{ if (urls.Parse $url).Host }} rel="noopener noreffer" target="_blank"{{ end }}>
        {{- .Pre | safeHTML }} {{ .Name }} {{ .Post | safeHTML -}}
    </a>
{{- end -}}
```

将上述代码改为如下代码：

```
{{- range .Site.Menus.main -}}
	{{ if .HasChildren }}
		<div class="dropdown">
			<a {{ if .URL }}href="{{ .URL }}"{{ else }}href="javascript:void(0);"{{ end }} class="menu-item menu-more dropbtn" title="{{ .Title }}" {{ if eq .Post "_blank" }}target="_blank" rel="noopener"{{ end }}>
				{{- .Pre | safeHTML }} {{ .Name }} {{ if ne .Post "_blank" }}{{ .Post | safeHTML -}}{{ end }}
			</a>
			<div class="menu-more-content dropdown-content">
				{{- range .Children -}}
					{{- $url := .URL | relLangURL -}}
					{{- with .Page -}}
						{{- $url = .RelPermalink -}}
					{{- end -}}
						<a href="{{ $url }}" title="{{ .Title }}" {{ if eq .Post "_blank" }}target="_blank" rel="noopener"{{ end }}>{{- .Pre | safeHTML }} {{ .Name }} {{ if ne .Post "_blank" }}{{ .Post | safeHTML -}}{{ end }}</a>
				{{- end -}}
			</div>
		</div>
	{{ else }}
		{{- $url := .URL | relLangURL -}}
		{{- with .Page -}}
			{{- $url = .RelPermalink -}}
		{{- end -}}
		<a class="menu-item{{ if $.IsMenuCurrent `main` . | or ($.HasMenuCurrent `main` .) | or (eq $.RelPermalink $url) }} active{{ end }}" href="{{ $url }}"{{ with .Title }} title="{{ . }}"{{ end }}{{ if (urls.Parse $url).Host }} rel="noopener noreffer" target="_blank"{{ end }}>
			{{- .Pre | safeHTML }} {{ .Name }} {{ if ne .Post "_blank" }}{{ .Post | safeHTML -}}{{ end }}
		</a>
	{{- end -}}
{{- end -}}
```

### 添加CSS样式

在`_custom.scss`中添加如下代码：

```
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
```

### 配置子菜单

打开站点配置文件`config.toml`，添加子菜单到菜单栏里。子菜单其实和原本的菜单一样写法，只是多了一个`parent`属性，用来定位到对应的父菜单的`identifier`。下面是一个简单的子菜单定义方式（没有使用多语言功能）：

```
# 菜单配置
[menu]
  [[menu.main]]
    identifier = "posts"
    # 你可以在名称 (允许 HTML 格式) 之前添加其他信息, 例如图标
    pre = "<i class='fas fa-fw fa-archive'></i>"
    # 你可以在名称 (允许 HTML 格式) 之后添加其他信息, 例如图标
    post = ""
    name = "归档"
    url = "/posts/"
    title = ""
    weight = 1
  [[menu.main]]
    pre = "<i class='fas fa-fw fa-link'></i>"
    name = "友链"
    identifier = "friends"
    url = "/friends/"
    weight = 2

  # 二级菜单
  [[menu.main]]
    parent = "posts"
    pre = "<i class='fas fa-fw fa-th'></i>"
    name = "分类"
    identifier = "categories"
    url = "/categories/"
    weight = 1
  [[menu.main]]
    parent = "posts"
	identifier = "tags"
    pre = "<i class='fas fa-fw fa-tag'></i>"
    post = "_blank"
	name = "标签"
	url = "/tags/"
	title = ""
	weight = 2
```

菜单栏还允许以下配置：

* 通过给菜单配置一个`post = "_blank"`属性来将该菜单设置为在新窗口打开该链接，如果`post`属性填其他值则依然作为原本的功能使用：即给`name`添加后缀。
* 通过设置`title`来添加超链的提示文本。
* 父菜单可以通过将`url`设置为空来将其渲染为不跳转的超链：`url = ""`。

## 如何添加自定义的页面

除了发布草稿和正文，我们还可以添加自定义的页面page。page不会像文章那样被渲染，而是被渲染成一个单独的页面，类似于你的文档、标签页面。

如：

* 在站点根目录的`/content/`目录下，新建一个文件夹，比如`about`文件夹。然后在该文件夹里新建一个`index.md`文件，该文件将作为站点访问该目录的页面，你可以将其当成一篇特殊的文章。

* 在`index.md`文件里加上下面的内容，实际上这里只需要`title`就够了，`date`这个日期属性可要可不要，因为`page`页面是看不到这个日期的：

  ```
  ---
  title: "关于"
  date: 2022-01-27T23:10:00+08:00
  ---
  ```

* 接下来你就可以像写普通文章一样，在这个`index.md`文件里随便写你想要展示的内容就行了。

## 背景添加动态线条效果

在`layouts/partials/header.html`的底部`{{- end -}}`之前添加如下代码：

```
<script type="text/javascript"
	color="220,220,220" opacity='0.7' zIndex="-2" count="200" src="//cdn.bootcss.com/canvas-nest.js/1.0.0/canvas-nest.min.js">
</script>
```

其中：

* color：表示线条颜色，三个数字分别为(R,G,B)，默认：（0,0,0）；如，41,36,33（象牙黑）。
* opacity：表示线条透明度（0~1），默认：0.5。
* count：表示线条的总数量，默认：150。
* zIndex：表示背景的z-index属性，css属性用于控制所在层的位置，默认：-1。



