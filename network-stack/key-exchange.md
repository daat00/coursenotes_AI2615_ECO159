## Diffie-Hellman(Basic Version)
#### Setup
prime $p$, generator $\alpha$ of $\Z_p^*$

#### Protocol
- $𝐴$ chooses random $𝑥$, send $𝐵$ message $𝛼^𝑥 \mod 𝑝$,
- $𝐵$ chooses random $𝑦$, send $𝐴$ message $𝛼^𝑦 \mod 𝑝$,
- $𝐵$ receives $𝛼^𝑥$ and computes the shared key as $\bm K = (𝑎^𝑥)^𝑦 \mod 𝑝$
- $𝐴$ receives $𝛼^𝑦$ and computes the shared key as $\bm K=(𝑎^𝑦)^𝑥 \mod 𝑝$

若 middle 发送给 $B$ $\alpha^{x'}$, middle 和 $B$ 会同样计算出 $K = \alpha^{x'y}$.


## MTI/A0
假设预先知道对方的公钥

#### Setup
- Prime $p$, generator $\alpha$ of $Z_p^*$
- $A$: select random private key $a$, computes public key $z_A =\alpha^a \mod p$
- $B$: select random private key $b$, computes public key $z_B =\alpha^b \mod p$

#### Protocol Messages
- $A$ -> $B$: $\alpha^x \mod p$
- $B$ -> $A$: $\alpha^y \mod p$

#### Protocol Actions
- A computes the shared key as $K=(a^y)^a z_B^x \mod p= (a^y)^a \alpha^{bx} =\alpha^{ay+bx}$
- B computes the shared key as $K=(a^x)^b z_A^y \mod p=(a^x)^b \alpha^{ay} =\alpha^{bx+ay}$

middle 发送给 $B$ $\alpha^{x'}$, 
- $B$ 计算出 $K = \alpha^{x'y} \alpha^{ay}$
- middle 算不出来 $\alpha^{ay}$

#### Modern Auth
client 的公钥临时生成然后发过去的。仅在密码通信层进行单向认证，另一向认证在应用层进行（验证注册时密码等）

 
# Key establishment
Key establishment is a process or protocol whereby a shared secret becomes available to two or more parties, for subsequent 
cryptographic use.

#### Key transport
one party creates or otherwise obtains a secret value, and 
securely transfers it to the other(s).
- 适用于机房内两两通信，ssh 此时太麻烦（kerberos）
- Key Distribution Center (KDC) 为每一对用户分配一个会话密钥


#### Key agreement
a shared secret is derived by two (or more) parties as a 
function of information contributed by, or associated with, each of these, 
(ideally) such that no party can predetermine the resulting value.


# 服务器存储密码
#### Hash + Salt
- 防止彩虹表攻击（枚举密码组合撞库，扩大密码空间）
- 服务器存储 $H(pw||salt)$

# PKI
Who uses PKI 

- SSL, TLS
- VPN (PPTP, IPSec, L2TP...)
- Email (S/MIME, PGP, Exchange KMS)
- Files (PGP, W2K EFS, and many others)
- Web Services (WS-Security)
- Smartcards (Certificates and private key store) 
- Executables (Java applets, .NET Assemblies, Drivers, Authenticode)
- Copyright protection (DRM)

### X.509 Certificate (simplified)
- X.500 Subject
- Public Key
- X.500 issuer
- Expiration date
- Serial Number (for revocation)
- Extensions

+ CA Digital Signature

### Revocation
由 client 的浏览器发起，可以是 CRL 或者 OCSP

# TLS
## SSL Basics
SSL consists of two protocols

### Handshake protocol
- public-key cryptography 
- generate master secret


#### 协商过程 (TLS 1.2, 2008)

- ClientHello (version, random, session id, cipher suite list, compression method)
- ServerHello (version, chosen cipher suite, random)
  - [Certificate]
  - \[ServerKeyExchange\]: (Select cipher suite)
  - [CertificateRequest]
  - change cipher spec -> switch to encrypted
- ClientHello continues
  - \[Certificate\]
  - ClientKeyExchange (pre-master secret)
  - \[CertificateVerify\]

Pre-master Secret -> Master Secret -> Key Material -> {key, IV, MAC}...

> 在浏览器配置 client certificate 以发送 client: [Certificate] 应答 server: [CertificateRequest]
>
> TLS 1.2 允许 client 自主选择 key，直接交给 server


#### 协商过程 (TLS 1.3, 2018)

> 1 RTT or 0 RTT, header 是 1.2, 下属内容作为 extension 传输（兼容 1.2）
>
> 支持 QUIC(UDP) 协议，可以在 UDP 第一个报直接发 tls 1.3 的 client hello

`key exchange` 压缩到 1 个 message，身份认证（certificate）被移动到 authentication 部分，在第一轮 hello 的时候直接交换 key，然后再认证。同时，废除了 RSA key exchange，只保留 （EC）DHE 和 PSK，所以第一轮一定有 key share

Early Secret -> Handshake Secret -> Master Secret


#### Record protocol
将 application data 进行 fragment, compress, add MAC, encrypt, append SSL record header(at front)

### Session Resumption
支持 tcp 关闭链接，然后重新连接，不需要重新协商
- 使用 session id, PSK, ticket 

0-RTT: 通过 PSK 或者 ticket 直接发送 application data