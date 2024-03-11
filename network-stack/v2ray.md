# V2Ray Documetation
### sniffing
流量探测，根据指定的流量类型，重置所请求的目标。这话不太好理解，简单说这东西就是从网络流量中识别出域名。这个 sniffing 有两个用处：
1. 解决 DNS 污染
2. 对于 IP 流量可以应用后文提到的域名路由规则；
3. 识别 BT 协议，根据自己的需要拦截或者直连 BT 流量(后文有一节专门提及)。

### StreamSetting
底层传输配置, 在全局上可以等价配置 TransportObj(全局 transport 设置).

### VMess
支持 Mux（类似于 keep alive?），仅能减少握手的延迟，对网速有反作用


### KCP, mKCP
https://github.com/skywind3000/kcp

纯算法实现，并不负责底层协议（如UDP）的收发，需要使用者自己定义下层数据包的发送方式，以 callback的方式提供给 KCP。 连时钟都需要外部传递进来，内部不会有任何一次系统调用。也许你实现了一个P2P，或者某个基于 UDP的协议，而缺乏一套完善的ARQ可靠协议实现，那么简单的拷贝这两个文件到现有项目中，稍微编写两行代码，即可使用。

#### RTO 翻倍 vs 不翻倍：
TCP超时计算是RTOx2，这样连续丢三次包就变成RTOx8了，十分恐怖，而KCP启动快速模式后不x2，只是x1.5（实验证明1.5这个值相对比较好），提高了传输速度。

#### 选择性重传 vs 全部重传：
TCP丢包时会全部重传从丢的那个包开始以后的数据，KCP是选择性重传，只重传真正丢失的数据包。

#### 快速重传：
发送端发送了1,2,3,4,5几个包，然后收到远端的ACK: 1, 3, 4, 5，当收到ACK3时，KCP知道2被跳过1次，收到ACK4时，知道2被跳过了2次，此时可以认为2号丢失，不用等超时，直接重传2号包，大大改善了丢包时的传输速度。

#### 延迟ACK vs 非延迟ACK：
TCP为了充分利用带宽，延迟发送ACK（NODELAY都没用），这样超时计算会算出较大 RTT时间，延长了丢包时的判断过程。KCP的ACK是否延迟发送可以调节。

#### UNA vs ACK+UNA：
ARQ模型响应有两种，UNA（此编号前所有包已收到，如TCP）和ACK（该编号包已收到），光用UNA将导致全部重传，光用ACK则丢失成本太高，以往协议都是二选其一，而 KCP协议中，除去单独的 ACK包外，所有包都有UNA信息。

#### 非退让流控：
KCP正常模式同TCP一样使用公平，即发送窗口大小由：发送缓存大小、接收端剩余接收缓存大小、丢包退让及慢启动这四要素决定。但传送及时性要求很高的小数据时，可选择通过配置跳过后两步，仅用前两项来控制发送频率。以牺牲部分公平性及带宽利用率之代价，换取了开着BT都能流畅传输的效果。

#### mKCP
v2ray 进一步改进了 KCP

### TLS
在 server 配置证书，建立 VMess 连接时先 TLS 握手（note default udp）

- HaProxy
- TLS 分流器
- 原理都是分流 HTTP 和 非 HTTP 流量

#### Trojan

### WebSocket
- 使用 Nginx/Caddy/Apache 是因为 VPS 已经有 Nginx/Caddy/Apache 可以将 V2Ray 稍作隐藏
- 使用 WebSocket 是因为搭配 Nginx/Caddy/Apache 只能用 WebSocket，
- 使用 TLS 是因为可以流量加密，看起来更像 HTTPS。
- 此时 nginx 在 VMess 的前面接受 WebSocket 请求并 redirect 给 V2ray server

似乎会自动追踪 X-forward-for

### DNS
默认不代理本地 DNS，但内置 DNS 会覆盖 sniff 流量的 IP。可以在 route 配置，也可以详细配置 DNS server

#### Client DNS settings
仅用于[匹配 DNS](https://guide.v2fly.org/basics/dns.html#%E9%97%AE-%E5%8E%8B%E6%A0%B9%E6%B2%A1%E5%86%99-dns-%E9%85%8D%E7%BD%AE-%E5%A5%BD%E5%83%8F%E4%B9%9F%E6%98%AF%E6%AD%A3%E5%B8%B8%E7%9A%84)。

路由 routing 中 "domainStrategy" 的几种模式都跟 DNS 功能密切相关，所以在此专门说一下。
- "AsIs"，当终端请求一个域名时，进行路由里面的 domain 进行匹配，不管是否能匹配，直接按路由规则走。
- "IPIfNonMatch", 当终端请求一个域名时，进行路由里面的 domain 进行匹配，若无法匹配到结果，则对这个域名进行 DNS 查询，用结果的 IP 地址再重新进行 IP 路由匹配。
- "IPOnDemand", 当匹配时碰到任何基于 IP 的规则，将域名立即解析为 IP 进行匹配。
 
可见，AsIs是最快的，但是分路由的结果最不精确；而IPOnDemand是最精确的，但是速度是最慢的。

> DNS 服务是可以用来分流的，大致思路是，”哪些域名要去哪个 DNS 服务器解析，并希望得到属于那里的 IP 地址“。 配置的规则跟路由模式中用的是相似的，详细使用还需参考官方文档。


#### 代理 DNS 服务
[ref](https://guide.v2fly.org/basics/dns.html#%E5%AF%B9%E5%A4%96%E5%BC%80%E6%94%BE-v2ray-%E7%9A%84-dns-%E6%9C%8D%E5%8A%A1)
[ref](https://guide.v2fly.org/basics/dns.html#%E5%AF%B9%E5%A4%96%E5%BC%80%E6%94%BE-v2ray-%E7%9A%84-dns-%E6%9C%8D%E5%8A%A1)

### routing
list 第一个是 default，配置的 domain 和 ip 是 AND

### 动态端口

# Misc
#### Basic Auth in HTTP Proxy
使用 base64 enc 后放在 Proxy-Auth header 中，不安全

#### Digest Auth
- client 第一次请求受保护的资源时不会提供认证信息。
- server 检测到需要认证，返回 401 Unauthorized，并在 header WWW-Authenticate 中包含一个随机数（nonce）
- client 收到 401，使用用户名、密码、URI、nonce、自己生成的 cnonce 和请求计数器等信息，生成 response 值。与其他值一起发给服务器