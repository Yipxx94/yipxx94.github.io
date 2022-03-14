# Python的venv虚拟环境和pip包


<!--more-->


## 前言

`Python`应用程序通常会使用不在标准库内的软件包和模块。应用程序有时需要特定版本的库，因为应用程序可能需要修复特定的错误，或者可以使用库的过时版本的接口编写应用程序。

这意味着一个`Python`安装可能无法满足每个应用程序的要求。如果应用程序 A 需要特定模块的 1.0 版本但应用程序B需要 2.0 版本，则需求存在冲突，安装版本 1.0 或 2.0 将导致某一个应用程序无法运行。

这个问题的解决方案是创建一个`virtual environment`，一个目录树，其中安装有特定`Python`版本，以及许多其他包。

然后，不同的应用将可以使用不同的虚拟环境。 要解决先前需求相冲突的例子，应用程序 A 可以拥有自己的 安装了 1.0 版本的虚拟环境，而应用程序 B 则拥有安装了 2.0 版本的另一个虚拟环境。 如果应用程序 B 要求将某个库升级到 3.0 版本，也不会影响应用程序 A 的环境。

{{< admonition info >}}
**virtual environment -- 虚拟环境**

一种采用协作式隔离的运行时环境，允许 Python 用户和应用程序在安装和升级 Python 分发包时不会干扰到同一系统上运行的其他 Python 应用程序的行为。
{{< /admonition >}}

## 创建虚拟环境

用于创建和管理虚拟环境的模块称为`venv`。`venv`通常会安装你可用的最新版本的`Python`。如果您的系统上有多个版本的`Python`，可以通过运行`python3`或其他的任何版本来选择特定的`Python`版本。

**注意**：以下的命令均为`linux`下的命令。

安装`venv`

```
sudo apt-get install python3.8-venv
```

进入你需要的目录下，创建`tutorial-env`**虚拟环境**目录

```
python3 -m venv 名称（如tutorial-env）
```

激活虚拟环境

```
source tutorial-env/bin/activate
```

退出虚拟环境

```
deactivate
```

{{< admonition >}}
激活虚拟环境将改变你所用终端的提示符，以显示你正在使用的虚拟环境，并修改环境以使 python 命令所运行的将是已安装的特定 Python 版本。 例如：

```
$ source ~/envs/tutorial-env/bin/activate
(tutorial-env) $ python
```

{{< /admonition >}}

## 使用pip管理包

可以使用一个名为`pip`的程序来安装、升级和移除软件包。`pip`有许多子命令: `install`, `uninstall`, `freeze`等。

1. 可以通过指定包的名称来安装最新版本的包：

   ```
   python -m pip install novas
   ```

2. 可以通过提供包名称后跟`==`和版本号来安装特定版本的包：

   ```
   python -m pip install Django==3.2.8
   ```

其他常用命令

```
pip install --upgrade    将软件包升级到最新版本
pip uninstall            后跟一个或多个包名称将从虚拟环境中删除包
pip show                 将显示有关特定包的信息
pip list                 将显示虚拟环境中安装的所有软件包
pip freeze               将生成一个类似的已安装包列表，但输出使用pip install期望的格式
```



