---
layout: post
title: "Ubuntu安装Showdoc could not find driver 错误"
date: 2017-07-20
description: "Ubuntu,Showdoc,could not find driver"
tag: 常用设置和命令 
---   
   Ubuntu安装Showdoc时会检测一些php需要的组件并提示，这时候只需要按照提示安装即可

   安装后点击登录报错`could not find driver`

   了解了一下发现showdoc现在已经限定使用sqlite

   这时候只需要手动安装下php的sqlite3库即可
        
   `sudo apt-get install php5-sqlite`

