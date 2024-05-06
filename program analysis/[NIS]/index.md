常见的安全漏洞模型

#### 内存型
堆栈溢出

内存破坏：UAF, Double Free, Null Pointer Dereference

#### 逻辑型
认证绕过（编码或条件后门）

反序列化中加载类未检测对象安全性

#### Web 类漏洞
SQL注入，XSS，CSRF，SSRF


### Apache 

#### 堆喷射 Heap Spraying
UAF 常用 0x0c0c0c0c

#### Return-Oriented Programing
绕过 DEP(Data Execution Prevention) 

- 切换 sp 到堆区


# 侧信道
#### 幽灵 spectre
利用 cache 的读取速度快进行侧信道攻击，把数据做成地址

#### 熔断 meltdown


# 服务端漏洞
#### 攻击面
- 网络服务：HTTP, FTP, SSH, SMB, RDP, DNS 等系统自带或第三方软件提供的网络服务
- 本地应用：IE, Office, Adobe 系统自带 / 第三方本地应用
- 系统自身：攻击核心组件提权（iOS 越狱）

#### 内部机理
- 逻辑型：web 类，Java Applicaition (反序列化)
- 内存型：缓存区溢出、UAF

> Nessus, NeXpose, MSF


# IoT
常用架构 ARM, MIPS

> 与 x86 的区别，有 ra 保存返回地址。由于流水线，nop, PC + 8 是常见操作

> 与 x86 不同，缓存的非一致性
>
> Data Cache 与 Instruction Cache 分开，shellcode 写到 Data cache 之后直接读 IC 读不到

变量的栈帧位置和声明顺序是相反的，read 不会补充 0

cgi 通过环境变量传参