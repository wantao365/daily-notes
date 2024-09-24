# 实验环境

youxi1　　192.168.5.101　　负载均衡器

youxi2　　192.168.5.102　　主机1

youxi3　　192.168.5.103　　主机2

# Nginx负载均衡策略

　　nginx的负载均衡用于upstream模板定义的后端服务器列表中选取一台服务器接收用户的请求。一个基本的upstream模块如下：

```
upstream [服务器组名称]{
　　server [IP地址]:[端口号];
　　server [IP地址]:[端口号];
　　....
}
```

　　在upstream模块配置完成后，要让指定的访问反向代理到服务器列表，格式如下：

```
location ~ .*$ {
　　index index.jsp index.html;
　　proxy_pass http://[服务器组名称];
}
```

　　这样就完成了最基本的负载均衡，但是这并不能满足实际需求。目前Nginx的upstream模块支持6种方式的负载均衡策略（算法）：轮询（默认方式）、weight（权重方式）、ip_hash（依据ip分配方式）、least_conn（最少连接方式）、fair（第三方提供的响应时间方式）、url_hash（第三方通过的依据URL分配方式）。

##  1）轮询

　　最基本的配置方法，是upstream模块默认的负载均衡策略。每个请求会按时间顺序平均分配到不同的后端服务器。有如下参数：

| fail_timeout | 与max_fails结合使用                                          |
| ------------ | ------------------------------------------------------------ |
| max_fails    | 在fail_timeout参数设置的时间内最大失败次数。如果在这个时间内，所有该服务器的请求都失败了，那么认为该服务器停机 |
| fail_time    | 服务器被认为停机的时长，默认10s（被认为停机的服务器尝试间隔？） |
| backup       | 标记该服务器为备用服务器。当主服务器停止时，请求会被发送到它这里 |
| down         | 标记服务器永久停机                                           |

> 注意：1.down标记的服务器会自动剔除；2.缺省就是轮询；3.此策略适合服务器配置无状态且短平块的服务使用

## 2）weight

　　权重方式，在轮询策略的基础上指定轮询的几率。也可以认为是在轮询的基础上新增了一个weight的参数，此参数指定轮询的几率，值为number。upstream模块配置模板如下：

```
upstream [服务器组名称]{
　　server [IP地址]:[端口号] weight=2;
　　server [IP地址]:[端口号];
　　....
}
```

　　在该例子中，没有weight参数的服务器默认为1，weight的数值与访问比例成正比，所有weight值的总和为一个循环单位，服务器自身的weight值为循环单位内的轮询次数。

> 注意：1.权重越高分配到的请求越多；2.此策略可以和least_conn策略、iphash策略结合使用；3.此策略比较适合服务器硬件配置差距较大的情况。

## 3）ip_hash

　　依据ip分配方式，指定负载均衡器按照基于客户端IP的分配方式，这个方法确保了相同的客户端请求一致发送到相同的服务器，以保证session会话。这样每个访客都固定访问一个后端服务器，可以解决session不能跨服务器的问题。upstream模块配置模板如下：

```
upstream [服务器组名称]{
　　ip_hash;
　　server [IP地址]:[端口号] weight=2;
　　server [IP地址]:[端口号];
　　....
}
```

>  注意：
>
> ​	1.nginx1.3.1之前的版本不能在ip_hash中使用权重（weight）；
>
> ​	2..ip_hash不能与backup同时使用；
>
> ​	3.此策略适合有状态服务的程序，比如session；　
>
> 　4.当有服务器需要剔除，必须手动down掉。

## 4）least_conn

　　最少连接方式，把请求发给链接数最少的后端服务器。轮询是把请求平均分配给各个后端，使它们的负载大致相同。但是，有些请求占用的时间很长，会导致其所在的后端负载较高。这种情况下，least_conn这种方式就可以达到更好的负载均衡效果。upstream模块配置模板如下：

```
upstream [服务器组名称]{
　　least_conn;
　　server [IP地址]:[端口号] weight=2;
　　server [IP地址]:[端口号];
　　....
}
```

> 注意：此策略适合请求处理时间长短不一造成的服务器过载情况。

## 5）fair

　　响应时间方式，按照服务器端的响应时间来分配请求，响应时间短的优先分配。upstream模块配置模板如下：

```
upstream [服务器组名称]{
　　server [IP地址]:[端口号] weight=2;
　　server [IP地址]:[端口号];
　　....
　　fair;
}
```

> 注意：需要安装第三方插件。

## 6）url_hash

　　url分配方式，按访问url的hash结果来分配请求，使每个url定向到同一个后端服务器，要配合缓存命中来使用。同一个资源多次请求可能会到达不同的服务器上，导致不必要的多次下载，缓存命中率不高，以及一些资源时间的浪费。而使用url_hash，可以使得同一个url（也就是同一个资源请求）会到达同一台服务器，一旦缓存住了资源，再次收到请求，就可以在缓存中读取。upstream模块配置模板如下：

```
upstream [服务器组名称]{
　　server [IP地址]:[端口号] weight=2;
　　server [IP地址]:[端口号];
　　....
　　fair;
}
```

> 注意：需要安装第三方插件。

# 实验

## 1）在负载均衡器youxi1上编译安装nginx

　　安装nginx的依赖包

```
[root@youxi1 ~]# yum -y install gcc gcc-c++ autoconf automake zlib zlib-devel openssl openssl-devel pcre pcre-devel
```

　　上传nginx源码包nginx-1.14.1.tar.gz，解压安装

```
[root@youxi1 ~]# tar xf nginx-1.14.1.tar.gz -C /usr/local/src/

[root@youxi1 ~]# cd /usr/local/src/nginx-1.14.1/

[root@youxi1 nginx-1.14.1]# ./configure --prefix=/usr/local/nginx --with-http_dav_module --with-http_stub_status_module --with-http_addition_module --with-http_sub_module --with-http_flv_module --with-http_mp4_module

[root@youxi1 nginx-1.14.1]# make && make install

[root@youxi1 nginx-1.14.1]# echo $?
0
```

> 参数说明：
>
> --with-http_dav_module，启用ngx_http_dav_module支持（增加PUT,DELETE,MKCOL：创建集合,COPY和MOVE方法）默认情况下为关闭，需编译开启；
>
> --with-http_stub_status_module，启用ngx_http_stub_status_module支持（获取nginx自上次启动以来的工作状态）；
>
> --with-http_addition_module，启用ngx_http_addition_module支持（作为一个输出过滤器，支持不完全缓冲，分部分响应请求）；
>
> --with-http_sub_module，启用ngx_http_sub_module支持（允许用一些其他文本替换nginx响应中的一些文本）；
>
> --with-http_flv_module，启用ngx_http_flv_module支持（提供寻求内存使用基于时间的偏移量文件）；
>
> --with-http_mp4_module，启用对mp4文件支持（提供寻求内存使用基于时间的偏移量文件）。

　　生成nginx用户

```
[root@youxi1 nginx-1.14.1]# useradd -M -s /sbin/nologin nginx
```

　　启动并添加开机自启

```
[root@youxi1 nginx-1.14.1]# /usr/local/nginx/sbin/nginx``[root@youxi1 nginx-1.14.1]# echo /usr/local/nginx/sbin/nginx >> /etc/rc.local``[root@youxi1 nginx-1.14.1]# chmod +x /etc/rc.d/rc.local
```

　　如果防火墙是开启的记得添加端口号

```
[root@youxi1 nginx-1.14.1]# firewall-cmd --permanent --zone=``public` `--add-port=80/tcp && firewall-cmd --reload``success``success
```

　　访问192.168.5.101查看nginx是否正常

测试完成后，修改nginx的配置文件，最后重启nginx

```
[root@youxi1 nginx-1.14.1]# cp /usr/local/nginx/conf/nginx.conf{,.bak}
[root@youxi1 nginx-1.14.1]# vim /usr/local/nginx/conf/nginx.conf
user  nginx;　　//第2行
location / {　　//第43行起
　　root html;
　　index index.html index.htm;
　　if ($request_uri ~* \.html$){
　　　　proxy_pass http://htmlservers;
　　}
　　if ($request_uri ~* \.php$){
　　　　proxy_pass http://phpservers;
　　}
　　proxy_pass http://picservers;
}
upstream htmlservers {　　//在http模块下，server模块平级处添加
　　server 192.168.5.102:80;
　　server 192.168.5.103:80;
}
upstream phpservers{
　　server 192.168.5.102:80;
　　server 192.168.5.103:80;
}
upstream picservers {
　　server 192.168.5.102:80;
　　server 192.168.5.103:80;
}
[root@youxi1 nginx-1.14.1]# /usr/local/nginx/sbin/nginx -s reload
```

## 2）在youxi2和youxi3上布置网页程序

```
[root@youxi2 ~]# yum -y install httpd
[root@youxi2 ~]# echo youxi2 > /var/www/html/index.html
[root@youxi2 ~]# echo youxi2.php > /var/www/html/index.php
[root@youxi2 ~]# echo youxi2.other > /var/www/html/index.jsp
[root@youxi2 ~]# systemctl start httpd.service
 
[root@youxi3 ~]# yum -y install httpd
[root@youxi3 ~]# echo youxi3 > /var/www/html/index.html
[root@youxi3 ~]# echo youxi3.php > /var/www/html/index.php
[root@youxi3 ~]# echo youxi3.other > /var/www/html/index.jsp
[root@youxi3 ~]# systemctl start httpd.service
```

如果防火墙是开启的，记得添加端口号

```
[root@youxi2 ~]# firewall-cmd --permanent --zone=public --add-port=80/tcp && firewall-cmd --reload
success
success
 
[root@youxi3 ~]# firewall-cmd --permanent --zone=public --add-port=80/tcp && firewall-cmd --reload
success
success
```

>  最后：
>
> 192.168.5.101、192.168.5.101/index.php、192.168.5.101/index.jsp 分别多次访问查看

# 案例

```
#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    server {
        listen       80;
        server_name  localhost;
        #charset koi8-r;
        #access_log  logs/host.access.log  main;
        location / {
            root   html;
            index  index.html index.htm;
            if ($request_uri ~* \.html$) {
                proxy_pass http://htmlservers;
            }
            if ($request_uri ~* \.png$) {
                proxy_pass http://picservers;
            }
            proxy_pass http://htmlservers;
        }
    }

    server {
        listen       9400;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root  D:/Temporary/arcgis;
            index api/4.9/dojo/dojo.js index.html index.htm;
            add_header Access-Control-Allow-Origin *;
            add_header Access-Control-Allow-Methods 'GET, POST, OPTIONS';
            add_header Access-Control-Allow-Headers 'DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Authorization';

            if ($request_method = 'OPTIONS') {
                return 204;
            }
        }
    }

    server{
        listen 9401;
        autoindex off;
        server_name localhost;
        access_log D:/Temporary/access.log combined;
        index index.html index.htm index.jsp index.php;
        #error_page 404 /404.html;
        if ( $query_string ~* ".*[\;'\<\>].*" ){
            return 404;
        }
        
        location ~ /(hcxmall_fe|hcxmall_admin_fe)/dist/view/* {
            deny all;
        }
        
        location / {
            root C:\Users\Administrator\Pictures;
            add_header Access-Control-Allow-Origin *;
        }
    }

    upstream picservers{
        server  localhost:9401;
    }

    upstream htmlservers{
        server  localhost:9400;
    }

    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}
```

