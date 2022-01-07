# Hugo搭建过程的踩坑记录


<!--more-->


## 前言

本文主要记录在整个站点搭建过程遇到的一些bug以及相应的解决方案。希望能给朋友们一些参考。

```
云服务器系统
ubuntu 20.04LTS

docker版本
docker 20.10.11

hugo版本
hugo v0.90.1-48907889+extended linux/amd64 BuildDate=2021-12-10T10:56:41Z VendorInfo=gohugoio

主题
LoveIt
```

## 添加国内软件源

在安装软件的过程中，个别的地区会出现下载速度缓慢的问题，这个时候就需要添加国内的软件源，如清华源，在`/etc/apt/sources.list`文件中的内容替换为下面的内容。

```
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
```

添加源以后，需要执行`sudo apt-get update`，此时可能会出现错误。

解决办法：

```
sudo gpg --keyserver keyserver.ubuntu.com --recv 3B4FE6ACC0B21F32 （公钥）
sudo gpg --export --armor 3B4FE6ACC0B21F32 | sudo apt-key add -
```

**注意**：公钥根据错误提示信息来填写。

## git clone主题报错

报错情况：

```
error: RPC failed; curl 56 GnuTLS recv error (-110): The TLS connection was non-properly terminated.
```

解决方法：

1. 使用下面命令安装 build-essential、fakeroot 和 dpkg-dev。

`sudo apt-get install build-essential fakeroot dpkg-dev`

2. 使用下面命令在家目录中创建一个名为 git-rectify 的目录。

`mkdir ~/git-rectify`

3. 进入 get-rectify 目录并获取 git 源文件。

```
cd ~/git-rectify
apt-get source git
```

4. 安装所有 git 依赖项。

`sudo apt-get build-dep git`

5. 安装 libcurl的依赖文件。

`sudo apt-get install libcurl4-openssl-dev`

6. 进入git目录。

`cd git-2.25.1/`

路径名后面2.*是版本号，需要看一下自己的版本。

7. 修改文件内容，需要修改两个文件。

```
vim ./debian/control     # 把libcurl4-gnutls-dev 修改为 libcurl4-openssl-dev
vim ./debian/rules        # 把TEST =test整行删除
```

8. 编译和构建安装包。

`sudo dpkg-buildpackage -rfakeroot -b`

9. 退回上一级目录，安装编译好的安装包。

```
cd ..
sudo dpkg -i git_2.25.1-1ubuntu3.2_amd64.deb
```

用git进行clone时提示“服务器验证失败”，在命令行下输入：

`export GIT_SSL_NO_VERIFY=1`

## 部署到GitHub Pages不显示页面

解决方法：

1. 在github进入仓库，选择**Settings**，找到**Pages**。

2. 选择**Change theme**。

3. 点击**Link to another page**，再随便选一个主题即可。



