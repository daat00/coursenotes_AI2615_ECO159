# Web Server Basics
## HTTPD

HTTPD, or HTTP Daemon, is a program that runs in the background of a web server and waits for incoming server requests. It's a commonly used web server that you can install and configure on a network server.

```bash
# Install httpd
sudo apt-get install httpd
```

## CGI

The http-get query string passed to CGI programs by `env`, the post string is passed to CGI programs by `stdin`.


## PHP

PHP is a popular general-purpose scripting language that is especially suited to web development. It's fast, flexible and pragmatic, PHP powers everything from your blog to the most popular websites in the world.

```php
<?php
echo "Hello, World!";
?>
```

## JavaScript

JavaScript is a programming language that allows you to implement complex features on web pages. It is an interpreted, prototype-based, scripting computer programming language.

```javascript
console.log("Hello, World!");
```

## Node.js

Node.js is an open-source, cross-platform, back-end JavaScript runtime environment that runs on the V8 engine and executes JavaScript code outside a web browser.

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World\n');
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```

## Nginx

Nginx is a web server that can also be used as a reverse proxy, load balancer, mail proxy and HTTP cache.

```bash
# Install Nginx
sudo apt-get install nginx
```

## Apache

The Apache HTTP Server, colloquially called Apache, is a free and open-source cross-platform web server software.

```bash
# Install Apache
sudo apt-get install apache2
```

## IIS

Internet Information Services (IIS) is an extensible web server software created by Microsoft for use with the Windows NT family.

```powershell
# Install IIS
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## Web Server Security

Web server security is the protection of information assets that can be accessed from a Web server. Web server security is important for any organization that has a physical or virtual Web server connected to the Internet.


# Apache
至于Apache的请求对象（request_rec），在Apache HTTP服务器中，每个接收到的HTTP请求都会对应一个request_rec结构体实例。这个结构体包含了与该请求相关的各种信息，比如请求的URL、请求头、请求体等等。在Apache的模块开发中，开发者通常需要操作这个请求对象来获取请求信息、处理请求、生成响应等。因此，这段代码很可能是在某个Apache模块的开发中，用来获取并操作当前的HTTP请求对象。

## request_rec

```c
typedef struct request_rec request_rec;

#include <httpd.h>
#include <http_protocol.h>
#include <http_request.h>

/* 在 Apache 模块中处理请求的回调函数 */
static int example_handler(request_rec *r) {
    /* 检查是否为 HTTP GET 请求 */
    if (r->method_number != M_GET) {
        /* 返回 HTTP 405 Method Not Allowed 错误 */
        return HTTP_METHOD_NOT_ALLOWED;
    }

    /* 设置响应的内容类型 */
    ap_set_content_type(r, "text/html");

    /* 发送 HTTP 响应体 */
    ap_rputs("<html><head><title>Hello, World!</title></head><body><h1>Hello, World!</h1></body></html>", r);

    /* 返回 HTTP 200 OK 状态码 */
    return OK;
}

/* 在模块初始化时注册处理函数 */
static void example_register_hooks(apr_pool_t *p) {
    /* 注册处理函数 */
    ap_hook_handler(example_handler, NULL, NULL, APR_HOOK_MIDDLE);
}

/* 定义模块 */
module AP_MODULE_DECLARE_DATA example_module = {
    STANDARD20_MODULE_STUFF,
    NULL,                  /* 模块配置 */
    NULL,                  /* 目录配置 */
    NULL,                  /* 服务器配置 */
    NULL,                  /* 命令表 */
    example_register_hooks  /* 注册处理函数的钩子 */
};
```