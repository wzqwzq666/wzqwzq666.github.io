---
layout: post
title: "Ubuntu安装Gogs设置自启动"
date: 2017-07-20
description: "Ubuntu,Gogs,自启动"
tag: 常用设置和命令 
---   
##  Ubutu安装Gogs后设置开机自启动

- 在`/etc/rc.local`的   `exit 0`上一行添加
`su - wzq -c "nohup /opt/gogs/gogs web > /opt/gogs/log/gogs_web.log 2>&1 &"`
-  wzq为用户名 
- `/opt/gogs`为安装路径