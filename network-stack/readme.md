# Before TLS 1.3 (by 2018)
# Since TLS 1.3 (after 2018)
Domain hiding (Encrypted SNI)
https://youtu.be/TDg092qe50g
- DNS over HTTPS (DNSSEC)

ECH (Encrypted Client Hello)
https://en.wikipedia.org/wiki/Server_Name_Indication#Encrypted_Client_Hello

# 从 pixiv 免代理直连看 SNI 阻断及其绕过方法——域前置
https://www.xkww3n.cyou/2020/05/01/sni-blocking-and-domain-fronting/

# proxy 
https://en.wikipedia.org/wiki/Proxy_server#Web_proxy_servers

[http_CONNECT_method](https://en.wikipedia.org/wiki/HTTP_tunnel#HTTP_CONNECT_method) 是一个 GET POST 同级的 http 方法

系统代理只是一个 config，os 不会直接 forward packet
- Windows 应用在内部维护一个 override 的 proxy，根据情况选择 dst
- clash 的系统代理修改了 registry。以及 control panel 的设置最后也会落到 registry。env:http_proxy 应该是 linux 的

> 通过 Charles / Wireshark 抓包，windows 设置系统代理后 chrome 会自动发送 CONNECT 请求，并且不会在开发者工具中显示。clash 接受 CONNECT 包

> 对于 clash + urllib3，https 用 http 代理，四种排列组合有不同的行为 https://urllib3.readthedocs.io/en/stable/advanced-usage.html#http-and-https-proxies

