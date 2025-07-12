---
layout: .post
title: 阿里云Linux服务器刷Windows servers 2012 R2
date: 2023-09-10 16:03:10
tags:
cover: https://chevereto.maimaioo.top/images/2025/04/08/0002.jpg
---

# 缘起：

上次刷腾讯云Linux服务器刷成windows的时候就预告了我可能会出有关阿里云Linux服务器刷windows server系统的文档，现在它来了。

 

# 前期准备：

一台境外阿里Linux服务器，一台能上网的电脑，还要有一颗敢于去折腾的心

 

# 老样子，先上大致流程：

1、Linux服务器系统选择Debian 11.1系统（理论上可以选择Ubuntu，因为他们都是属于debian系的）

2、往现有的Linux服务器刷入一个跳板pe，并启动到跳板pe

3、加载正式pe，并下载镜像刷机

4、往安装好的系统打上阿里云服务器的驱动（不打入阿里云的定制驱动会出各种bug），修复引导

 

# Linux端需要用到的命令：

```shell
1、sudo -i

2、apt update && apt install grub2 grub-imageboot -y && \ mkdir -p /boot/images/ && \ wget --no-check-certificate -O /boot/images/mfslinux.iso https://d.maimaioo.top/p/TXLighthousecos/CloudServer/mfslinux-FirPE-1.8.2-VirtIO.iso && \ sed -i 's/GRUB_DEFAULT=0/GRUB_DEFAULT=2/g' /etc/default/grub && \ update-grub2 && reboot
```

 

# 操作流程：

## 一、跳板pe：

### 从Linux系统启动：

1、使用网页端一键登录登录到Linux服务器中

2、在终端界面输入`sudo -i`回车切换到root之后输入 `apt update && apt install grub2 grub-imageboot -y && \ mkdir -p /boot/images/ && \ wget --no-check-certificate -O /boot/images/mfslinux.iso https://d.maimaioo.top/p/TXLighthousecos/CloudServer/mfslinux-FirPE-1.8.2-VirtIO.iso && \ sed -i 's/GRUB_DEFAULT=0/GRUB_DEFAULT=2/g' /etc/default/grub && \ update-grub2 && reboot` 等待断开连接,服务器详情页中的远程控制进入救援模式

![阿里云——详情——远程链接——紧急救援](https://chevereto.maimaioo.top/images/2025/04/03/b6d4b675b55e2fc6caf4ee7c0692003a.png)

## 二、正式pe：

### 刷入正式pe并加载进正式pe进行刷机：

1、进入到救援模式中

2、用迅雷下载ventoy解压到桌面

3、打开ventoy文件夹，打开Ventoy2Disk，在左上角的配置选项——显示所有设备——选择云服务器的系统盘——安装，等待安装完毕，重启服务器

![Ventoy界面](https://chevereto.maimaioo.top/images/2025/04/03/Ventoy.png)

4、服务器重启完后再次通过紧急救援链接服务器

5、对系统盘进行分区：打开桌面上的Diskgenius，点击服务器系统盘，快速分区/删除全部分区并自己手动分区（这里作为演示，我就选择了快速分区，C盘分随意，D盘至少15G）。

**注意：C盘为系统盘，D盘为临时存放数据的数据盘**

6、下载镜像及驱动：打开桌面上的迅雷软件，将磁力链接复制进去下载(链接我会附到文章末尾）

7、双击下载好的镜像自动挂载到虚拟光驱，使用pe里自带的WinNTSetup工具安装系统（工具需在开始菜单或者桌面找一下），里面的参数按照我配图的参数（上面我已经尽可能注明清楚）照着调就行，基本不用改动。调好后点击“开始安装“在弹出的页面里不勾选“安装成功后自动重新启动计算机”、后点确定开始安装系统。

![WinNTSetup.png](https://chevereto.maimaioo.top/images/2025/04/03/WinNTSetup.png)

8、系统安装完毕后打开桌面上的“Dism++”，选择C盘中的系统注入阿里云windows server 2012 r2的驱动

![Dism++添加驱动](https://chevereto.maimaioo.top/images/2025/04/03/Dism.png)

9、注入完驱动后修复一下引导即可重启服务器使用Windows server 2012 r2系统

10、我提供的系统镜像中登录密码默认为：“nat.ee”，进入系统后第一件事是改密码！改密码！改密码！（重要的事情说三遍）。

11、开始使用Windows server 2012 r2 。☆: .｡. o(≧▽≦)o .｡.:☆

 

# 作者有话想说：

这次阿里云Linux服务器刷windows怎么说，就十分的艰难又坎坷，从自己修改打包镜像到刷系统重复了不止10次了，再后来我们经过进一步的深入才发现，阿里云的服务器魔改了bios，且需要定制的驱动，所以先前一直内存显示不正常+突发性卡死。最后还是那句话“创作不易，请勿抄袭”，顺带感谢我的朋友@凌梦晓.的技术支持。
在这要提醒你们打驱动！打驱动！打驱动！
 

# 镜像链接和驱动下载链接：

iso_link:https://d.maimaioo.top/p/TXLighthousecos/CloudServer/Win_Server2012R2_64_Administrator_nat.ee.gz

drive_link：https://d.maimaioo.top/p/TXLighthousecos/CloudServer/AliYunDrive-2012R2.zip
