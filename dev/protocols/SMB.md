<!-- https://blog.csdn.net/qq_44002418/article/details/125508092 -->
# SMB
SMB(全称是Server Message Block)是一个协议名，可用于在计算机间共享文件、打印机、串口等，电脑上的网上邻居就是靠它实现的。

SMB 是一种客户机/服务器、请求/响应协议。通过 SMB 协议，客户端应用程序可以在各种网络环境下读、写服务器上的文件，以及对服务器程序提出服务请求。此外通过 SMB 协议，应用程序可以访问远程服务器端的文件、以及打印机等资源。

SMB一开始的设计是在NetBIOS协议上运行的，而NetBIOS本身则运行在NetBEUI、IPX/SPX或TCP/IP协议上。

- NetBIOS 使用下列端口：
  - UDP/137（NetBIOS 名称服务）
  - UDP/138（NetBIOS 数据报服务）
  - TCP/139（NetBIOS 会话服务）
- SMB 使用下列端口：
  - TCP/139 (NETBIOS) windows NT
  - **TCP/445** windows 2000

NetBIOS用于局域网内主机名发现

#### 建立连接
1. 1 RTT - NEGOTIATE (version)
2. 2 RTT - SESSION_SETUP (认证)
3. 1 RTT - TREE_CONNECT (请求 uri)

#### 上传文件流程
1. CREATE
2. SET_INFO
3. WRITE
4. ...
5. CLOSE

读取的时候把 SET_INFO 和 WRITE 换成 READ 就行了

#### 断开连接
1. TREE_DISCONNECT
2. LOGOFF