安装 flarum 的 tips
====
https://discuss.flarum.org.cn/d/3998

使用宝塔面板 + LNMP
可以直接用命令 bt

#### Composer
就是个 pip，不过需要单独安装（复制一个 php 脚本）

#### 用户权限
把 www 和 ubt 互相放到对方组里似乎不是很管用。把权限设置成 775，flarum(php) 似乎还是不能正常读写

暂时还是把 ubt 放到 www 组里，chown 到 www。因为看起来 vscode 能正常编辑

### 宝塔的目录控制
#### 配置控制
前端修改的配置会保存到对应的 conf file 里。

例如，flarum 的 nginx 修改 php 版本，会写到 flarum-nginx.conf 里，用 # BEGIN # END 包裹进行定位。

webserver 的 root 也是可调的，前端的值联通 conf 的 root。

#### 目录控制
宝塔面板在 server/panel 下，插件放在 server/*。相当于 panel 也是个插件。面板里的安装就相当于控制台安装，会将 /bin 的程序 symlink 到 /server/xxx/bin




MISC
===
#### WSGI
1. WSGI 会处理 HTTP 消息，把 Header 的 kv 写到环境变量 env 里面
2. 网站逻辑从 env 读取 kv 值，构造 HTTP 请求：用 start_response 发送 Header，返回值  Body
   
https://www.liaoxuefeng.com/wiki/1016959663602400/1017805733037760

demo

```python
# In: env: Dict, start_response: Callback provided by WSGI server
def application(environ, start_response):
    start_response('200 OK', [('Content-Type', 'text/html')])
    return [b'<h1>Hello, web!</h1>']
 ```

有了WSGI，我们关心的就是如何从environ这个dict对象拿到HTTP请求信息，然后构造HTML，通过start_response()发送Header，最后返回Body。

