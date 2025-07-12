---
layout: .post
title: VPS通过DD脚本安装一个纯净的系统
date: 2023-11-16 19:11:56
tags: Linux云服务器
cover:  https://chevereto.maimaioo.top/images/2025/04/08/0002.jpg
---

# 起源：

近段时间看到了阿里云在做活动，一台2核2G内存的云服务器（不是轻量云服务器）不用9999，也不用999，只需要99，就能购买一年，续费也是这个价。当时我就心动了，但是心动不如行动，于是在我的不懈努力下，终于买了下来。但我有时候不是很想使用云服务器商的系统（虽然能有比较全面的监控），那怎么才能给云服务器换一个纯净的系统呢。那就需要我今天要提到的DD脚本了。

# 前期准备工作：

一台装着Linux的服务器（这里我用debian11为例），一台正常上网的电脑，一颗敢于去折腾的心。

# Linux端需要用到的命令：

```shell
apt-get install -y xz-utils openssl gawk file
```

```shell
wget --no-check-certificate -qO AutoReinstall.sh 'http://git.io/AutoReinstall.sh' && bash AutoReinstall.sh（要挂代理）
wget --no-check-certificate -O AutoReinstall.sh https://d.02es.com/AutoReinstall.sh && chmod a+x AutoReinstall.sh && bash AutoReinstall.sh（国内可用）
```



# 开始折腾：

1、安装运行脚本所需的软件：`apt-get install -y xz-utils openssl gawk file`

2、根据服务器的地理位置选择运行的脚本，我这里是深圳的云服务器，我就选了国内可用的脚本了`wget --no-check-certificate -O AutoReinstall.sh https://d.02es.com/AutoReinstall.sh && chmod a+x AutoReinstall.sh && bash AutoReinstall.sh`

3、在弹出的选择界面按数字键选择想要重装的系统并回车，等待下载完毕，后面均为傻瓜式操作。

 

# 常见问题：

如果运行脚本报错`wget: No such file or directory`,没什么大事，只是你wget没有安装而已，运行一下`sudo apt install wget -y`就行。

如果运行脚本提示网络异常，按键盘下键选择choose mirror（选择镜像源），接着往下翻翻到“china“接着选一个中意的镜像站即可。
