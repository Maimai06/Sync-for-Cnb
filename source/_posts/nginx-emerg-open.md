---
layout: .post
title: nginx_[emerg]_open()报错
date: 2023-09-10 16:07:44
tags: note
cover:  https://chevereto.maimaioo.top/images/2025/04/07/0000.jpg
---

# 报错：

“ERROR:检测到配置文件有错误请先排除后再操作

nginx: [emerg] open() "/www/server/nginx/conf/enable-php.conf" failed (2: No such file or directory) in /www/server/nginx/conf/nginx.conf:79
 nginx: configuration file /www/server/nginx/conf/nginx.conf test failed”

 

# 解决方案：

卸载当前版本的nginx，重装旧版本的nginx
