---
layout: post
title: "java.lang.OutOfMemoryError: PermGen space及其解决方法"
date: 2017-08-05
description: "java,PermGen space"
tag: JAVA 
--- 

今天在把打包好的java程序部署到tomcat时运行龟速还会卡死，偶尔还会报错`java.lang.OutOfMemoryError: PermGen space`   

经过查资料得知以下原因   

```
Java.lang.OutOfMemoryError: PermGen space PermGen space的全称是Permanent Generation space,是指内存的永久保存区域, 这块内存主要是被JVM存放Class和Meta信息的,Class在被Loader时就会被放到PermGen space中, 它和存放类实例(Instance)的Heap区域不同,GC(Garbage Collection)不会在主程序运行期对 PermGen space进行清理，所以如果你的应用中有很多CLASS的话,就很可能出现PermGen space错误, 这种错误常见在web服务器对JSP进行pre compile的时候。如果你的WEB APP下都用了大量的第三方jar, 其大小超过了jvm默认的大小(4M)那么就会产生此错误信息了。
```  



### 解决方法

- 打开`TOMCAT_HOME/bin/catalina.sh`
- 在第一行加入`JAVA_OPTS="-server -XX:PermSize=64M -XX:MaxPermSize=128m `   
- 重启服务   
  `ps -ef|grep tomcat8`  
 `kill -9 pid`   
`./startup.sh `

 








  