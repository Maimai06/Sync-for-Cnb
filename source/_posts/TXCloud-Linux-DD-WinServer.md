---
layout: .post
title: 腾讯云境外Linux轻量云服务器刷Windows系统
date: 2023-03-19 00:36:48
tags: 腾讯云 境外Linux云服务器
cover: https://chevereto.maimaioo.top/images/2025/04/07/0000.jpg
---
# 2025.04.09更新
## 为啥又更新了这篇文章?
因为之前命令里面那个pe镜像的地址失效了，于是这段时间我上网冲浪找了个打过相应驱动&精简过的Firpe，但里面缺了一点需要的工具（例如：迅雷下载、浏览器、WinNTSetup），所以花了点时间添加了进去，在pe里的桌面上。还有一件事，原来iso镜像的磁力链接被修改了，我已经再文档里更新&转存了一份到了我的云盘上。至于WinNTSetup里面怎么配置，还是这张图片。![WinNTSetup配置示例](https://chevereto.maimaioo.top/images/2025/04/03/WinNTSetup.png)
新pe镜像优点：内置kvm驱动，无需手动打驱动以下载镜像。内置文档内所需工具，无需再另外下载。
![新的PE的预览图](https://chevereto.maimaioo.top/images/2025/04/18/new-pe.png)
## 更新后的命令：

```shell
sudo su
apt-get install wget grub2 grub-imageboot -y && mkdir /boot/images/ && wget --no-check-certificate -O /boot/images/mfslinux.iso https://d.maimaioo.top/p/TXLighthousecos/CloudServer/Win10PE_KVM_FIR.iso && sed -i 's/GRUB_DEFAULT=0/GRUB_DEFAULT=2/g' /etc/default/grub && update-grub2 && reboot
```
[Mirror](https://d.maimaioo.top/p/TXLighthousecos/CloudServer/Windows_Server_2012R2_DataCenter_CN_cxthhhhh.com_v4.29_with_Full_Virtualization_Drive.iso)**（右键-复制链接地址即可）**
-------------------------
# 2023.03.26更新

## 缘起：

前段时间我投了一篇用pe来刷腾讯云境外Linux服务器的专栏，当时我就想，那有没有其他的方法可以刷Windows呢。



## 前期准备工作：

一台装着Debian11的境外Linux服务器(境内的没什么意思，可以直接换成Windows)，一台正常上网的电脑，一颗敢于去折腾的心。



## Linux端用到的命令：

```shell
apt update && apt install grub2 grub-imageboot -y && \
mkdir -p /boot/images/ && \
wget --no-check-certificate -O /boot/images/mfslinux.iso https://mfsbsd.vx.sk/files/iso/mfslinux/mfslinux-0.1.10-f9c75a4.iso && \
sed -i 's/GRUB_DEFAULT=0/GRUB_DEFAULT=2/g' /etc/default/grub && \
update-grub2
reboot

```

及刷入Windows的相关命令

```shell
# Windows Server 2019
opkg update && opkg install pv && wget -O- "https://dl.lamp.sh/vhd/cn_win2019.xz" --no-check-certificate | xzcat | pv | dd of=/dev/vda
# Windows Server 2016
opkg update && opkg install pv && wget -O- "https://dl.lamp.sh/vhd/cn_win2016.xz" --no-check-certificate | xzcat | pv | dd of=/dev/vda
# Windows Server 2012
opkg update && opkg install pv && wget -O- "https://dl.lamp.sh/vhd/cn_win2012r2.xz" --no-check-certificate | xzcat | pv | dd of=/dev/vda

```



## 开始折腾：

### 安装恢复系统（系统默认用户名：root，密码：mfsroot）：

#### 1.通过ssh连接到服务器

#### 2.在终端中输入'sudo -i'回车切换到root用户

#### 3.然后输入以下这段命令，等待执行完毕重启服务器

(可以在服务器控制页面重启，也可以在终端里用'reboot'命令重启) 

```shell
apt update && apt install grub2 grub-imageboot -y && \
mkdir -p /boot/images/ && \
wget --no-check-certificate -O /boot/images/mfslinux.iso https://mfsbsd.vx.sk/files/iso/mfslinux/mfslinux-0.1.10-f9c75a4.iso && \
sed -i 's/GRUB_DEFAULT=0/GRUB_DEFAULT=2/g' /etc/default/grub && \
update-grub2
```

### 执行安装命令：

（在这里我提供三个系统的命令，大家按需选择）

Windows Server 2012 R2

```shell
opkg update && opkg install pv && wget -O- "https://dl.lamp.sh/vhd/cn_win2012r2.xz" --no-check-certificate | xzcat | pv | dd of=/dev/vda
```

Windows Server 2016

```shell
opkg update && opkg install pv && wget -O- "https://dl.lamp.sh/vhd/cn_win2016.xz" --no-check-certificate | xzcat | pv | dd of=/dev/vda
```

Windows Server 2019

```shell
opkg update && opkg install pv && wget -O- "https://dl.lamp.sh/vhd/cn_win2019.xz" --no-check-certificate | xzcat | pv | dd of=/dev/vda
```

运行完毕后重启服务器，进入到Windows系统。



## 后记：

server系统默认开了rdp（微软自家的远程桌面），用户是：Administrator，密码都是：Teddysun.com。由于是用vhd克隆，所以在装完系统后C盘会比原先小，需要到磁盘管理里”拓展卷“进行分区的扩大。这里不单单可以刷server，还有其他的win系统（详情可以去https://dl.lamp.sh/vhd查看），要注意一点：根据自己服务器引导方式选择镜像（传统bios或uefi）。



## 注：

阿里云也适用此教程（但不能根据此教程刷winserver2012），刷入其他镜像按下面格式就行（将xxx替换成刷的目录）

```shell
opkg update && opkg install pv && wget -O- "https://dl.lamp.sh/vhd/xxx" --no-check-certificate | xzcat | pv | dd of=/dev/vda
```

举个栗子：如果要刷windows11，就输入`opkg update && opkg install pv && wget -O- "https://dl.lamp.sh/vhd/zh-cn_windows11_22h2.xz" --no-check-certificate | xzcat | pv | dd of=/dev/vda`

-----------------------------------------------------------------------------------------
# 2023.03.19更新

## 前期准备：

1台境外Linux服务器，一台能上网的电脑，一颗敢于折腾的心

## 大致流程：

1、Linux系统选择安装Ubuntu 18.04.1 LTS版本
2、用ssh安装pe系统
3、进入pe系统刷写镜像

## Linux端用到的命令：

```shell
sudo su
apt-get install wget grub2 grub-imageboot -y && mkdir /boot/images/ && wget --no-check-certificate -O /boot/images/mfslinux.iso http://xcloud.yxdzltw.com/file_id/627979e99a66c32592304bbdbac0947b5b872c05/Win7PE_KVM_V2.iso && sed -i 's/GRUB_DEFAULT=0/GRUB_DEFAULT=2/g' /etc/default/grub && update-grub2 && reboot
```

## 操作流程：

1、用腾讯云的一键登录登录到要刷Windows系统的煮鸡中
2、输入命令 `sudo su` 回车切换到root之后输入命令后等待断开连接
3、使用vnc登录到服务器中会发现目前系统变成了一个win7的pe系统，此时凡事不要慌，先把驱动打上来驱动一些必要的组件（驱动网卡）打开qudong文件夹运行1.bat安装网卡驱动才能正常上网
4、分区，打开桌面上的分区工具，对服务器的系统盘进行重新分区or格式化（原配置中如果系统盘大小低于50G的建议花点钱临时组个10~20G的高性能云硬盘挂载到轻量云上临时存放数据，如果大于50G可以在系统盘中分10G左右的空间存放系统镜像及安装工具）
5、接下来我们需要下载镜像和工具，镜像我这边用的是Windows_Server_2012R2_DataCenter_CN_cxthhhhh.com_v4.29_with_Full_Virtualization_Drive.iso（一位群友提供的，磁力链接在文章结尾），打开桌面上的software文件夹中的“Thundermini”，将磁力链接复制进去下载。工具用的是WinNTSetup（网上x86的版本，下载链接在文章结尾）。
6、用pe系统中自带的Imdisk软件将iso镜像挂载到虚拟光驱，打开下载好的WinNTSetup，里面的参数按照我配图的参数（上面我已经尽可能注明清楚）照着调就行，基本不用改动。调好后点击“开始安装“在弹出的页面里勾选上“安装成功后自动重新启动计算机”（也可以不勾，不勾的话要自己手动重启，所以建议勾上）后点确定开始安装系统。
![WinNTSetup.png](https://chevereto.maimaioo.top/images/2025/04/03/WinNTSetup.png)7、开始安装后就可以想干啥干啥，因为到用户登录之前都不需要进行任何操作。
8、我提供的系统镜像中登录密码默认为：“cxthhhhh.com”，进入系统后第一件事是改密码！改密码！改密码！（重要的事情说三遍）。
9、开始使用Windows server 2012 r2 。☆*: .｡. o(≧▽≦)o .｡.:*☆。

## 作者有话想说：

这篇文档只适用于腾讯云的云服务器，阿里云的云服务器作者在折腾中，折腾成功了再写。如果刷机有报错，请自行百度解决，如果我自己遇到了的话会补充上来。其他系统也适用，前提是iso格式镜像且里面包含着virtio驱动。用ssh刷Windows(同腾讯云)的文档已经在写了✍，敬请期待。

## 镜像链接和工具下载链接：

[Windows_Server_2012R2_DataCenter_CN_cxthhhhh.com_v4.29_with_Full_Virtualization_Drive.iso by cxthhhhh.com](https://odc.cxthhhhh.com/p/SyStem-OS/Windows-ISO-WIM-ESD-Install/Windows_Server_2012R2_DataCenter_CN_cxthhhhh.com_v4.29_with_Full_Virtualization_Drive.iso?sign=XtdlhGJ-HSg35X8C08Ln_z5fY3orkasmyVNLU18YBBo=:0)
工具链接：“[WinNTSetup下载_WinNTSetup绿色汉化版5.2.6 - 系统之家 (xitongzhijia.net)](https://www.xitongzhijia.net/soft/20577.html)”在历史版本中选择v4.1.0版本下载
