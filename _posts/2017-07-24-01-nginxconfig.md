---
layout: post
title: "MAC&Ubuntu nginx使用反向代理"
date: 2017-07-24
description: "MAC&Ubuntu, nginx"
tag: 常用设置和命令 
--- 
### MAC

- hosts文件修改
  
  /private/etc/hosts 添加
  
  127.0.0.1 localhost
  
  127.0.0.1 localapache
  
  192.168.2.121 localjianhua
- apache 修改端口

    /Users/wzq/ect/apache2/httpd.conf 中Listen后改成80
  
      tomcat 修改端口
    
      /Users/wzq/Library/tomcat8/conf/server.xml
    修改port 此处未测试



- nginx 安装&配置

  
  `brew install nginx`
  
  路径 /usr/local/ect/nginx
  
  配置文件 nginx.conf 
  
  apache tomcat 转发设置
  
  ```
  server {
        listen       80;
        server_name  localapache;//修改此处

        #charset koi8-r;

        #access_log  logs/host.access.log  main;
        add_header Cache-Control private;
        charset utf-8;
        fastcgi_intercept_errors on;
        location / {
            proxy_pass       http://127.0.0.1:8081;//修改此处
            proxy_set_header Host      $host;

            proxy_set_header X-Real-IP $remote_addr;

        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

        }
    }
```

  
  修改后重新载入设置 `sudo nginx -s reload`
  
  关闭  `sudo nginx -s stop`
  
  启动 `sudo nginx `



### Unbntu

- nginx 安装`sudo apt-get nginx`

   路径 /etc/nginx/sites-available/default   

   配置文件 nginx.conf    

- apache tomcat 转发设置  
- ```
server {
        listen       80;
        server_name  localapache;//修改此处

        #charset koi8-r;

        #access_log  logs/host.access.log  main;
        add_header Cache-Control private;
        charset utf-8;
        fastcgi_intercept_errors on;
        location / {
            proxy_pass       http://127.0.0.1:8081;//修改此处
            proxy_set_header Host      $host;

            proxy_set_header X-Real-IP $remote_addr;

        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

        }
    }
``` 

修改后重新载入设置 `sudo nginx -s reload  `

关闭  `sudo nginx -s stop ` 

启动 `sudo nginx `











  