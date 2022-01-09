# Nginx的安装与配置


<!--more-->


## 前言

### 概述

`Nginx`是开源、高性能、高可靠的`Web`和反向代理服务器，而且支持热部署，几乎可以做到 7 * 24 小时不间断运行，即使运行几个月也不需要重新启动，还能在不间断服务的情况下对软件版本进行热更新。性能是`Nginx`最重要的考量，其占用内存少、并发能力强、能支持高达 5w 个并发连接数，最重要的是，`Nginx`是免费的并可以商业化，配置使用也比较简单。

### `Nginx`的特点

1. 作为`Web`服务器，`Nginx`处理静态文件、索引文件，自动索引的效率非常高；
2. 作为代理服务器，`Nginx`可以实现无缓存的反向代理加速，提高网站运行速度；
3. 作为负载均衡服务器，`Nginx`既可以在内部直接支持 Rails 和 PHP ，也可以支持 HTTP 代理服务器对外进行服务，同时还支持简单的容错和利用算法进行负载均衡；
4. 在性能方面，`Nginx`是专门为性能优化而开发的，实现上非常注重效率。它采用内核 Poll 模型，可以支持更多的并发连接，最大可以支持对5万个并发连接数的响应，而且只占用很低的内存资源；
5. 在稳定性方面，`Nginx`采取了分阶段资源分配技术，使得CPU与内存的占用率非常低。`Nginx`官方表示，`Nginx`保持1万个没有活动的连接，而这些连接只占用 2.5MB 内存，因此，类似DOS这样的攻击对`Nginx`来说基本上是没有任何作用的；
6. 在高可用性方面，`Nginx`支持热部署，启动速度特别迅速，因此可以在不间断服务的情况下，对软件版本或者配置进行升级，即使运行数月也无需重新启动，几乎可以做到 7 * 24 小时不间断地运行；
7. 完全开源，生态繁荣。

### `Nginx`的作用

`Nginx`的几个重要的使用场景：

* 静态资源服务，通过本地文件系统提供服务；
* 反向代理服务，延伸出包括缓存，负载均衡等；
* `API`服务，`OpenResty`。

对于前端来说`Node.js`并不陌生，`Nginx`和`Node.js`的很多理念类似，HTTP 服务器、事件驱动、异步非阻塞等，且`Nginx`的大部分功能使用`Node.js`也可以实现，但`Nginx`和`Node.js`并不冲突，都有自己擅长的领域。`Nginx`擅长于底层服务器端资源的处理（静态资源处理转发、反向代理，负载均衡等），`Node.js`更擅长上层具体业务逻辑的处理，两者可以完美组合。

{{< admonition info >}}

**正向代理**与**反向代理**的区别

正向代理：是一个位于客户端和原始服务器(origin server)之间的服务器，为了从原始服务器取得内容，客户端向代理发送一个请求并指定目标(原始服务器)，然后代理向原始服务器转交请求并将获得的内容返回给客户端。客户端才能使用正向代理。

反向代理（Reverse Proxy）：是指以代理服务器来接受 Internet 上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给 Internet 上请求连接的客户端，此时代理服务器对外就表现为一个反向代理服务器。

区别：

1. 位置不同

   正向代理，架设在客户机和目标主机之间；

   反向代理，架设在服务器端。

2. 代理对象不同

   正向代理，代理客户端，服务端不知道实际发起请求的客户端；

   反向代理，代理服务端，客户端不知道实际提供服务的服务端。

{{< /admonition >}}

## `Nginx`的安装

### 安装环境

安装`nginx`之前，需要安装一些`nginx`所依赖的环境，下面的环境视自己的系统情况而定。

#### 安装`gcc`编译环境

```
sudo apt-get update
sudo apt-get install build-essential
gcc -v		#检验
```

#### 安装第三方的开发包

##### PCRE

`PCRE`(Perl Compatible Regular Expressions)是一个Perl库，包括 perl 兼容的正则表达式库。

`nginx`的 HTTP 模块使用pcre来解析正则表达式，所以需要在linux上安装`pcre`库。

{{< admonition >}}
`pcre`的官网有一个最新的`pcre2`，安装这个是无法编译`nginx`的，所以要安装之前版本的`pcre`。
{{< /admonition >}}

本文演示的是源码安装方式：

1. 下载源码并解压

```
wget https://sourceforge.net/projects/pcre/files/pcre/8.45/pcre-8.45.tar.gz
tar -zxvf /home/myblog/pcre-8.45.tar.gz -C /home/myblog/pcre    #解压目录根据自己的实际情况
```

2. 编译并安装

```
cd pcre/pcre-8.45
./configure
make
sudo make install
```

##### zlib

`zlib`库提供了很多种压缩和解压缩的方式，`nginx`使用`zlib`对 HTTP 包的内容进行gzip，所以需要在linux上安装`zlib`库。

本文演示的是源码安装方式：

1. 下载源码并解压

```
wget http://zlib.net/zlib-1.2.11.tar.gz
tar -zxvf /home/myblog/zlib-1.2.11.tar.gz -C /home/myblog/zlib    #解压目录根据自己的实际情况
```

2. 编译并安装

```
cd zlib/zlib-1.2.11
./configure
make
sudo make install
```

##### openssl

`OpenSSL`是一个强大的安全套接字层密码库，囊括主要的密码算法、常用的密钥和证书封装管理功能及SSL协议，并提供丰富的应用程序供测试或其它目的使用。

`nginx`不仅支持 HTTP 协议，还支持 HTTPs （即在ssl协议上传输http），所以需要在linux安装`openssl`库。

本文演示的是源码安装方式：

1. 下载源码并解压

```
wget http://www.openssl.org/source/openssl-3.0.1.tar.gz
tar -zxvf /home/myblog/openssl-3.0.1.tar.gz -C /home/myblog/openssl    #解压目录根据自己的实际情况
```

2. 编译并安装

```
cd openssl/openssl-3.0.1
./configure
make
sudo make install
```

3. 检验

```
openssl version
```

此时会出现报错：

```
openssl: error while loading shared libraries: libssl.so.3: cannot open shared object file: No such file or directory
```

解决方法：

```
su root或ctrl + d		#切换回根用户
ln -s /usr/local/lib64/libssl.so.3 /usr/lib/libssl.so.3
ln -s /usr/local/lib64/libcrypto.so.3 /usr/lib/libcrypto.so.3
```

{{< admonition >}}
第二个路径`/usr/lib/`要进入自己的`/usr`看看，是`/usr/lib`还是`/usr/lib64`，根据具体的实际情况来。
{{< /admonition >}}

### 安装`Nginx`

本文演示的是源码安装方式：

1. 下载源码并解压

```
wget http://nginx.org/download/nginx-1.20.2.tar.gz
tar -zxvf /home/myblog/nginx-1.20.2.tar.gz -C /home/myblog/nginx	#解压目录根据自己的实际情况
```

2. 编译并安装

```
cd nginx/nginx-1.20.2
./configure --prefix=/usr/local/nginx --with-http_ssl_module --with-openssl=/home/myblog/openssl/openssl-3.0.1		#这里是openssl的源码的位置
make
sudo make install
```

3. 环境变量的配置

此时，`nginx`已经成功安装，可以用命令`whereis nginx`查看安装位置，如`/usr/local/nginx`。我们要使用`nginx`命令时，必须进入`/usr/local/nginx/sbin`，为了在任何路径我们都能使用`nginx`命令来启动`nginx`，我们可以增加环境变量的配置。

* 打开文件

```
sudo vim /etc/profile
```

* 在最下面加入如下内容

```
export PATH=$PATH:/usr/local/nginx/sbin
```

* 使配置文件立即生效，之后重启系统

```
source /etc/profile
```

**验证**

```
nginx -v
输出：
nginx version: nginx/1.20.2
```

{{< admonition >}}

用这种`wget`源码的方式安装，`nginx`的配置文件在`/usr/local/nginx/conf/`下；

如果使用`apt-get`或者`yum`的方式安装，`nginx`的配置文件在`/etc/nginx/`下。

{{< /admonition >}}

### `Nginx`的常用命令

```
cd /usr/local/nginx/sbin/
./nginx -v	                #查看nginx的版本
./nginx                     #启动
./nginx -s stop             #停止
./nginx -s quit             #安全退出
./nginx -s reload           #重新加载配置文件
ps aux | grep nginx         #查看nginx进程
```

## 配置`Nginx`

**前提条件：**

* 已经购买个人域名并完成备案和解析；
* 已经开启服务器的80端口和443端口；

* 已经通过阿里云SSL证书服务完成证书签发；
* 已经下载证书到本地。

### 在`Nginx`服务器上安装证书

1. 在`Nginx`安装目录（默认为`/usr/local/nginx/conf`）下创建一个用于存放证书的目录，将其命名为`cert`：

```
cd /usr/local/nginx/conf  	#进入Nginx默认安装目录。如果你修改过默认安装目录，请根据实际配置进行调整。
mkdir cert  				#创建证书目录，命名为cert。
```

2. 将本地证书文件和私钥文件上传到`Nginx`服务器的证书目录（示例中为`/usr/local/nginx/conf/cert`）。

3. 编辑`Nginx`配置文件（`nginx.conf`），修改与证书相关的配置内容。

在配置文件中定位到HTTP协议代码片段（`http{}`），并在HTTP协议代码里面添加以下server配置（如果server配置已存在，按照以下注释内容修改相应配置即可）。

使用示例代码前，请注意替换以下内容：

* yourdomain.com：替换成证书绑定的域名。
  如果你购买的是单域名证书，需要修改为单域名（例如`www.aliyundoc.com`）；如果你购买的是通配符域名证书，则需要修改为通配符域名（例如`*.aliyundoc.com`）。

* cert-file-name.pem：替换成你在步骤3上传的证书文件的名称。
* cert-file-name.key：替换成你在步骤3上传的证书私钥文件的名称。

```
#以下属性中，以ssl开头的属性表示与证书配置有关。
server {
    listen 443 ssl;
    #配置HTTPS的默认访问端口为443。
    #如果未在此处配置HTTPS的默认访问端口，可能会造成Nginx无法启动。
    #如果你使用Nginx 1.15.0及以上版本，请使用listen 443 ssl代替listen 443和ssl on。
    server_name yourdomain.com; #需要将yourdomain.com替换成证书绑定的域名。
    root html;
    index index.html index.htm;
    ssl_certificate cert/cert-file-name.pem;  #需要将cert-file-name.pem替换成已上传的证书文件的名称。
    ssl_certificate_key cert/cert-file-name.key; #需要将cert-file-name.key替换成已上传的证书私钥文件的名称。
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    #表示使用的加密套件的类型。
    ssl_protocols TLSv1.1 TLSv1.2 TLSv1.3; #表示使用的TLS协议的类型。
    ssl_prefer_server_ciphers on;
    location / {
        root html;  #站点目录。如，root /myblog/public。
        index index.html index.htm;
    }
}
```

4. 设置HTTP请求自动跳转HTTPS。

如果你希望所有的HTTP访问自动跳转到HTTPS页面，则可以在需要跳转的HTTP站点下添加以下rewrite语句。
使用示例代码前，请注意将yourdomain.com替换成证书绑定的域名。

```
server {
    listen 80;
    server_name yourdomain.com; #需要将yourdomain.com替换成证书绑定的域名。
    rewrite ^(.*)$ https://$host$1; #将所有HTTP请求通过rewrite指令重定向到HTTPS。
    location / {
        index index.html index.htm;
    }
}
```

5. 执行以下命令，重启`Nginx`服务。

```
cd /usr/local/nginx/sbin  #进入Nginx服务的可执行目录。
./nginx -s reload  #重新载入配置文件。
```

如果重启`Nginx`服务时收到报错，可以使用以下方法进行排查：

* 收到`the "ssl" parameter requires ngx_http_ssl_module`报错：你需要重新编译`Nginx`并在编译安装的时候加上`--with-http_ssl_module`配置。
* 收到`"/cert/3970497_pic.certificatestests.com.pem":BIO_new_file() failed (SSL: error:02001002:system library:fopen:No such file or directory:fopen('/cert/3970497_pic.certificatestests.com.pem','r') error:2006D080:BIO routines:BIO_new_file:no such file)`报错：你需要去掉证书相对路径最前面的`/`。例如，你需要去掉`/cert/cert-file-name.pem`最前面的`/`，使用正确的相对路径`cert/cert-file-name.pem`。



