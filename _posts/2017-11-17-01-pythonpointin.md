---
layout: post
title: "python 确定一个点是否在多边形内"
date: 2017-10-25
description: "python,一个点是否在多边形内"
tag: centos 
---   
新安装的 centos 系统的根分区大小才50多G,根本不够用,比如装不下我需要恢复的数据库文件,于是我要把分区大小调整一下.
##### 调整 CentOS 分区
- 首先先查看一下磁盘信息`# df -h`,由于当时没有截图,就口述了,查询结果是 home 分区占了860多 G,根分区只有50多 G.
- 卸载/home 分区`umount /home`
- 调整分区大小,指定/home 大小为30G`resize2fs -p /dev/mapper/VolGroup-lv_home 30G`,提示执行`e2fsck -f /dev/mapper/VolGroup-lv_home`,按要求执行完以后再执行一次第一个命令.
- 挂载/home,`mount /home`,释放空间`lvreduce -L 30G /dev/mapper/VolGroup-lv_home`
- 查看剩余空间大小`vgdisplay`
- 把剩余空间加给根分区`lvextend -L +830G /dev/mapper/VolGroup-lv_root` `resize2fs -p /dev/mapper/VolGroup-lv_root`
- 然后就要等好久,最后我直接断开连接了,再上去`df -h`看已经分好了.