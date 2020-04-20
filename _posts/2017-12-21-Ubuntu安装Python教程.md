---
title: Ubuntu安装Python教程
layout: post
category: Python
date: 2017-12-21
---


# 安装python

SHH 登入终端，运行

```shell
python
```

可以看到是否 python2 已安装

运行

```shell
python3
```

可以看到是否 python3 已安装。我的都已安装，没安装的用 apt-get 安装即可。

运行

```shell
pip3 install virtualenv
```

安装虚拟环境管理 python 版本
如果报错说没安装，就先安装

```shell
sudo apt-get update
sudo apt-get install python3-pip
```

再运行

```shell
pip3 install virtualenv
```
报错

```
locale.Error: unsupported locale setting
```

运行

```
locale
```

发现LC_CTYPE和LC_ALL是空的。
运行
```shell
export LC_ALL="en_US.UTF-8"
export LC_CTYPE="en_US.UTF-8"
```

再安装，成功。

找一下你系统安装的 python（python2或python3）在哪里。

homebrew安装的可能在/usr/local/bin/pythonx

linux自带的可能在/usr/bin/pythonx

然后你想让虚拟环境运行 python2 ，就进入你想放虚拟环境的目录

```shell
virtualenv -p /usr/local/bin/python2.7 pythonenv
```

pythonenv 就是你虚拟环境所在目录

进入虚拟环境：

```shell
source bin/activate
```

退出虚拟环境：
通常在虚拟环境中在 shell 中输入:

```shell
deactivate
```

将会返回正常环境.

参考：[廖雪峰virtualenv](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001432712108300322c61f256c74803b43bfd65c6f8d0d0000)
