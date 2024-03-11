# 架构
- Master 进程
- Worker 进程（读 conf，接收请求）

# 基本 conf
```nginx
worker_processes  1; # worker 进程数
events {
    worker_connections  1024; # 每个 worker 进程最大连接数
}
http {
    # MIME TYPES
    # 服务器在 header 标注文件的类型（不同于后缀）
    # 浏览器根据 mime 解析，例如 image/jpeg 会展示，而 application/octet-stream 会下载
    include       mime.types;
    default_type  application/octet-stream; # 默认文件类型

    sendfile        on; # 不由 worker read/write，由内核直接发送，少一次拷贝

    keepalive_timeout  65;

    # 虚拟主机 vhost，该 server 本机 8080 端口接收 some.domain.com 
    # port + server_name 应该唯一
    server {
        listen       8080;
        server_name  some.domain.com, *.domain.com; # 同一 ip 下多个域名
        location / {
            root   html;
            index  index.html index.htm;
        }
    }

    server {
        listen       8080;
        server_name  ~^[a-z]+\.domain\.com$; # 正则匹配
        location / {
            root   html;
            index  index.html index.htm;
        }
    }   
}
```

## 获取 client ip
通过转发 Header 中 X-Forwarded-For, X-Real-IP 等字段实现。`$proxy_add_x_forwarded_for`, `$remote_addr`, `$http_host` 等

# 负载均衡
集群是多态提供相同服务的服务器的集合，通过负载均衡器分发请求

#### conf
反向代理一台服务器

```nginx
server {
    listen       8080;
    server_name  some.domain.com;
    location / {
        proxy_pass http://another.domain.com; # Jump to another.domain.com, do NOT support https
    }
}
```

反向代理多台服务器可以在 http 块下配置 upstream

```nginx
upstream backend0 {
    server 192.168.44.102:80 weight=8; # 轮询，按权重分配
    server 192.168.44.103 weight=1 down backup;
}
server {
    listen       8080;
    server_name  some.domain.com;
    location / {
        proxy_pass http://backend0; # Jump to backend0
    }
}
```
backup: 所有非 backup 服务器都挂掉时启用

---

下述配置现在不常用
### ip-hash
单纯的轮询无法保持会话，session 信息保存在第一台服务器，集群其他服务器没有。

ip-hash 会根据 ip 的 hash 重定向到同一台服务器，然而 ip 可能会变。

### url_hash
对于不支持 cookie 的客户端，可以在参数中加入 session id，这样，按 url hash 重定向就可以保持会话。也可以把不同资源配置在不同 cluster

### cookie hash
浏览器会自动保存并在每次请求时带上 cookie，可以根据 cookie 的 hash 重定向到同一台服务器

```nginx
upstream httpd {
    hash $cookie_jsessionid consistent;

    server 127.0.0.1;
    server 127.0.0.1;
}

```

### least_conn, fair

## token - 无状态会话保持
以往的 cookie - session 方式会把 session 信息保存在会话服务器或 redis 服务器上。对于 redis 的情况，多个会话服务器接收到不同用户带着 cookie 的请求后，需要请求同一台 redis 服务器。高并发场景可能不好用。

无状态会话不保存 session 信息，通过下发 token，把信息记录到一个 token 字符串并加密，然后下发给客户端。客户端每次请求都携带 token，服务器根据 token 识别用户。

# 动静分离
由于不论如何，静态请求都需要经过 nginx，所以可以把静态资源直接放在 nginx 服务器上。即动态请求（index.html, ...）和静态请求（jar js jpg）分离，仅将动态请求交由后端服务器处理

```nginx
server {
    listen       8080;
    server_name  some.domain.com;
    location / {
        proxy_pass http://backend0; # Jump to backend0
    }
    location ~* \.(gif|jpg|jpeg|png|css|js|ico)$ { 
        # ~ enable regex, * case insensitive, \. escape ., $ end
        root   html;
    }
    location /(images|javascript|stylesheets) {
        root   html;
    }
}
```

### 配置 ssl
    
```nginx
server {
    listen       443 ssl;
    server_name  some.domain.com;
    ssl_certificate      cert.pem; # 证书
    ssl_certificate_key  cert.key; # 私钥
    location / {
        proxy_pass http://backend0; # Jump to backend0
    }
}
```

# URL Rewrite
```nginx
location / {
    # /user/(\d+) -> /user.php?id=$1
    # break, last, redirect, permanent, rewrite
    rewrite ^/user/(\d+)$ /user.php?id=$1 last; 
    proxy_pass http://backend0; # Jump to backend0
}
location ~* /user/(\d+) {
    proxy_pass http://backend0/user/$1; # Jump to backend0/user/(\d+)
}
```

# 反盗链，拒绝直连

header 中的 referer 字段标识请求的来源，浏览器在二级加载时会自动带上 referer 字段。判等 referer 字段即可判断请求来源

```nginx
valid_referers none 192.168.0.1;
if ($invalid_referer) {
    return 403;
}
```

# 高可用
> 网络分区故障
>
> A, B 两台服务器在两个机柜，两台机柜之间的交换机过热暂时故障，导致 A, B 之间通信中断，A, B 可以连接到同机柜其他服务器但互相无法连接。并且实际上没有服务器挂掉

高可用面对这么一个问题，两台 nginx 服务器，一个是主机、一个是备机。如果备机在判断主机挂掉后拿过来 ip 地址，主机却并没有挂掉就会冲突。要怎么知道主机挂掉了没有？

## keepalived
A, B 两台机子共同管理一个虚拟 ip 地址。A, B 之间通过心跳包检测对方是否存活。如果 A 挂掉，B 会拿过来虚拟 ip 地址，继续提供服务。

keepalived 只检测自己的进程是否活着，不检测 nginx 是否活着。可以写个脚本，nginx 挂掉后 kill keepalived 进程。

# Advanced Topics
## Sticky
sticky 是一个高级的 cookie hash，stiky 自身会下发一个 cookie

## keepalive
nginx 可以分别配置对客户端的 keepalive 和对服务器的 keepalive。
- 配置客户端 keepalive 时，需要注意 send_timeout 和 keepalive_timeout 的区别
- 配置服务端 keepalive 时，注意对于 1.1 清除 close

压测 https://www.bilibili.com/video/BV1yS4y1N76R/?p=67，keepalive 提升了约 30%。*特别是一些要启动 JVM 的服务，keepalive 可以减小 overhead*

#### Charles 

## nginx 处理连接的流程
https://www.bilibili.com/video/BV1yS4y1N76R?p=70

- buffering(RAM, disk)
- 在读取的同时发送请求
- epoll，回调函数

# Gzip
把文本、字体等文件压缩后传输，browser 再解压即可

### 动态压缩
```nginx
gzip on;
gzip_comp_level 2; # 压缩级别
gzip_min_length 1k;
gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php # 避免正则
```

但是动态压缩和 sendfile 不能同时开启，因为 sendfile 是直接从内核发送，不经过 nginx

#### chunked
nginx 是响应式的，请求时不知道文件有多大、没有 content-length，于是使用 chunked 分块传输，并通过类似 \x00 的方式标识结尾。

### 静态压缩
可以预先压缩好，然后直接 sendfile html.gz  静态压缩功能需要在重新编译 nginx 时加入 gzip 模块

ngx_http_gzip_static_module, ngx_http_gunzip_module

这种方式的 header 同时有 gzip 和 content-length


# Misc
互联网项目 QPS 300 已经有不错的盈利了

https://www.bilibili.com/video/BV1yS4y1N76R?p=82 开始没看了


# rsync
