---
layout: post
title: "nginx&tomcat 开启 https"
date: 2018-09-14
description: "nginx,tomcat,https"
tag: nginx
--- 
  
公司使用的后台程序之前一直是用 ip 访问的,近期反馈说打开之后有广告,由于没有域名,所以可能不是 DNS 劫持,而是直接页面劫持了,所以决定开启一下 https顺带给绑定个域名进行访问.

### 申请证书
- 证书选用的是腾讯云内提供申请的免费证书,虽然限制挺多的但也够用了应该.直接放个地址      [证书申请](https://console.cloud.tencent.com/ssl/)
- 审核完毕后下载证书,选然后传到服务器相应位置 如`/etc/nginx/`

### nginx 配置
- 为了不影响别的配置,选择在`/etc/nginx/conf.d/`下新建一个配置文件进行添加,内容如下 (域名及证书位置自行修改)  

``` java
	server {
        listen       443;
        server_name  inferior.wang;
        ssl                  on;
        ssl_certificate      /etc/nginx/1_inferior.wang_bundle.crt;
        ssl_certificate_key   /etc/nginx/2_inferior.wang.key;
        ssl_session_timeout  5m;
        ssl_protocols TLSv1;
        ssl_ciphers  HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers   on;
        
        location / {
              client_max_body_size    16m;
              client_body_buffer_size 128k;
              proxy_pass              http://127.0.0.1:8080;
              proxy_set_header        Host $host;
              proxy_set_header        X-Real-IP $remote_addr;
              proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
              proxy_set_header        X-Forwarded-Proto https;
              proxy_next_upstream   off;
              proxy_connect_timeout   300;
              proxy_read_timeout      300;
              proxy_send_timeout      300;
        }
  }

```

### tomcat 配置
- 修改`server.xml`,找到`<Connector port="8080" protocol="HTTP/1.1" `一行后添加`proxyPort="443"` 
- 在`HOST`节点内增加

```xml
<Valve className="org.apache.catalina.valves.RemoteIpValve"
		remoteIpHeader="x-forwarded-for"
		remoteIpProxiesHeader="x-forwarded-by"
		protocolHeader="x-forwarded-proto" />
```

### 重启 nginx&tomcat
