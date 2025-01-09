Distributed System
===
































[TOC]































# Lesson 1 - Intro
## What is a distributed system? 

A **set of processes/nodes**  seeking to achieve some common goal by communicating with each other

- Processes/nodes: cores, processors, physical/virtual machines, clusters
- node：只要是通信场景下不可再分的单元就是 node

#### Challenge

节点没有 global view



## Common goals

### Processes work together 

Generic common goal

- Compute a function on the global system state
- But… processes have only a **local view** of the system



### Application Examples

#### Aggregate functions

- 机器学习的 model aggregation
- sensors

#### Reliable Broadcast

保证信息要么是 received by all 要么 by none

#### Synchronization

consensus

#### Atomic Commit 原子提交

Processes reach a common decision whether to commit or abort a transaction

#### Multi-party Computation 多方计算

#### 典型应用场景
##### Inherent (genuine) distribution

- Sensor, mobile ad-hoc networks
- Peer-to-peer networks
- Online collaboration

...

##### Distribution as an engineering artifact

- Engineering choices
- Fault-tolerance, performance optimizations
- Replication (e.g., databases)
- Multicores, ...

#### Arch




![](https://notes.sjtu.edu.cn/uploads/upload_90a50921847835ecfe2e6b45ccd9c1ff.png)


通信方式

- TCP
- UDP
- 共享内存
- 物理层具体实现不重要

#### 消息传递与共享内存

互相通信的方式有两种：

- 消息传递 message passing: msg 被放在 FIFO 队列里
- 共享内存 shared massgae: 不直接通信，在公共区域写东西，再让 receiver 去读


#### 挑战

主要挑战：

failure + asynchrony + concurrency

##### failure

Processes crash

Omit to send/receive messages (e.g., buffer full)

Processes are malicious

##### asynchrony (network failure)

Network failure (e.g., of a switch) may cause messages to be dropped or delayed

Different process speeds <-> asynchrony



# Lesson 2 - Abstractions 抽象


## Processes

node 抽象了 clusters, physical/virtual machines, processor, cores

我们把有限个 process 的集合记为 $\Pi$（bitcoin 是无限个）

-  process 个数 $N = |\Pi|$
-  每个 process 被看作状态机
-  有 [0, N-1] 的编号
-  每两个节点有一对一通信连接

> bitcoin  是一种 permissionless setting


### $p_i$ 的可用操作

- send event: 把消息放入 $outbuf_i[j]$
- receive event: $inbuf_i[*]$ 读消息
- 本地计算：更新 state

> Inbuf, outbuf 并不一定实际要实现；此外，在实现算法期间，会省略加入 buf，简述为发送。省略 outbuf -> inbuf 过程。默认 buf 里只有未处理的 msg


算法是相同的，仅仅由于 msg 到达的先后差异和丢包差异导致执行过程的差异

### 全局状态

Configuration $C$ 代表当前每个节点的状态，$S^i$ 是节点 $i$ 的状态

$$
C = (S^1, S^2, \ldots, S^n)
$$

记 $s_i$ 是某个 $p_j$ 执行的一次 step，$C$ 的变化可记为
$$
C_0, s_1, C_1, s_2, C_2, \ldots
$$


> 同一个算法执行两次，$s_1$ 不同导致 $C_1$ 不同

根据对算法讨论的粒度不同，定义状态的精细度不同

## 多层算法栈模型（协议栈）

### 节点框图

一个节点会接到一个消息，处理，然后发送一个消息



<img src="https://notes.sjtu.edu.cn/uploads/upload_3ba6548ee36ef729c6a10fb7b4984c10.png" width="50%">


但分布式算法也可能是分层的，上层和通过调用、回调来使用下层的功能


<img src="https://notes.sjtu.edu.cn/uploads/upload_3a7d002bb345324554cef935fa140b92.png" width=50%>


（复习注：我的理解，是考察某一特定节点上，算法从低到高的分层实现。如每个 node 都有协议栈，然而协议是跨 node 实现的概念


右图表示分层

- 调用 invoke
- 回调 Indication

### Example: JobHandler

最简单的算法，本层的计算直接完成，只有接口。
每个节点接受处理任务的 Submit 请求。在自己身上 forward 给 process 函数，然后 trigger Confirm 函数通知其他节点。

### 通过四要素来 spec 一个算法

- Name：算法名
- Request：问题的接口
- Indication：回调
- Property：性质（活性）


## Failure Abstractions 对错误的抽象

### Process Failure Abstraction

主要有四类，分别是 Crash(stop), crash-recovery, omission, byzantine.



#### Crash(Crash-stop)

就是 crash，然后停下来


#### Crash-recovery

crash 后会 recovery，可以 crash 无穷次

#### Omission

节点 omit 忽略本应 send/receive 的 msg

#### Byzantine

可以做出任意恶意行为（包括软件出 bug），但遵从一些基本假设

- 不可以爆破私钥




### 对密码学的 abstractions

#### MAC

每对节点共享一组对称密钥。

```
MAC = authenticate(p=sender, q=receiver, message)
isTrue = verifyauth(q=receiver, sender, message, MAC)
```

- authenticate: 由 sender p 调用
- verifyauth: 由 receiver 调用验签

> example: TLS uses the Hash-Based Message Authentication Code (HMAC) algorithm


#### 数字签名 digital signature scheme

```
σ = sign(sender, msg)
isTrue = verifysig(sender, message, σ)
```

隐式调用公私钥
比 MAC 高级的地方：可以由第三方验证双方的通信


#### Hash

```
h = H(x)
```



## 网络 abstraction

课件实现了一个应用端的 Reliable Byte stream (TLS)，用重传机制来实现最终可达。


最基本的是 Fail-Loss Link 模块，类似 UDP。以 best-effort 的概率 $p$ 到达。在其之上实现了 Stubborn Link，timeout 未达则重传。

其中，从分布式 node 的角度来看，网络接口是异步的 Send 请求和 Deliver 回调。
 
Properties 参考 ppt

```tex=
Fail-Loss Link % udp
    |
Stubborn Link % udp + retransmission
    |
Perfect (Reliable) Links % 考虑 duplicated 包 = TCP
    |
Authenticated Perfect Link % 添加摘要认证步骤
```

不是 FIFO 的，可能是无序的


### Bound 每轮时间界 - Synchronous Message Passing

限制 node 的性能大体相同


- Propagation delays
    - known upper bound limit $\Delta_{delay}$ on the time a takes for a  msg to deliver
- Process Speed
    - 执行时间
- clocks
    - 时钟漂移
    - $\Delta_{drift}$

把三种时间加到一起，定义为一轮 $\Delta = \Delta_{delat} + \Delta_{proc} + \Delta_{drift}$。经过这个上界后轮次结束（不会提前结束吗）

有了 $\Delta$ 后，可以涉及 round base 分布式算法，每 $\Delta$ 时间进入下一轮（提前到则 sleep）

- 但这个假设太强了，一般不这么设计。这么设计的最出名的是 bitcoin

<img src = "https://notes.sjtu.edu.cn/uploads/upload_c9c597d0c5c30dd6cd80b2d5e12f39d1.png" width=60%>


### Asynchronous Message Passing

异步算法不需要任何假设


![](https://notes.sjtu.edu.cn/uploads/upload_2bc770b5c5e5f543d08be175fae1a212.png)


我们对网络的建模是 Eventually Synchronous:

- 大多数时间是同步的（$\Delta_{delay}, \Delta_{proc}$ 在经过 *未知时间* 后才是 bounded，或 $\Delta$ 不可提前被知道）
- $\Delta = \Delta_{delay} + \Delta_{proc}$
- 过一段时间恢复正常，与真实情况依然不是完全相同

> $Delta$ 有界那不就是同步了吗……或许，可能处于性能考虑 $\Delta$ 会有缩减的步骤，那就等于真实上界是无穷的了。


### Failure Detectors FD

考虑 Crash Stop 的情况，不考虑 recovery。

基于 timing assumptions 发送并接受心跳（$\Delta$ 在 async 的时候不可知，超时了但可能节点是好的，导致 unreliable）

若 $2\Delta$ 无响应则报告节点出错。
在半同步，异步场景下只能给出对于 failure 的 hints，是 unreliable 的模块。

对于同步和非同步场景，可以分别抽象出来**完美错误检测器** Perfect FD `P` 和**最终完美错误检测器** $\diamond P$ 两个模块。

```
FD 只有 Indication：汇报某个其他节点 p 故障了 / 恢复了
<P, Crash | p>: 初始化一个无穷循环，每 2Delta 触发一次中断回调，向所有节点发心跳，并检测哪些节点没收到上一次的心跳。（P45）
-----
<◇P, Suspect | p>
<◇P, Restore | p>
Delta 是固定常量，只是事先不知道；所以采用二进制指数退避的思路来估计 Delta。从一个小 Delta 开始，每 2Delta 收发一轮心跳，当前没心跳则 suspect，之后有心跳则 ++Delta
（上一轮的心跳在下一轮收到，仍然有效，说明 Delta 短了）
```



![](https://notes.sjtu.edu.cn/uploads/upload_fbf8ab7d2d2384462f822cb5a7e375f6.png)





性质：

- Strong Completeness: 最终，故障节点会被报告给正确节点 (P, $\diamond P$)
- Strong Accuracy：不会在 crash 之前错误怀疑任何节点。\(P\)
- Eventual Strong Accuracy：最终，不会错误怀疑 crash 节点。($\diamond P$)

> 为了方便理解，可以认为节点内部的回调函数是瞬时的、原子的；但实现的时候需要考虑。

### Leader Election

选主可以让算法看起来更简单。每个节点最终成为唯一 leader 或 follower

> 不妨假设系统的状态是静止的，也就是说，不会半道有东西 crash。

#### Why elect a leader?

- Synchronization/coordination tasks become much simpler
- We can use the leader to “centralize” the algorithm
    - Data processing, Resource allocation, Scheduling
    - Coordinating consensus

#### Why not?

- Stability?: Leader fails -> must reelect one
- Bottleneck：主节点成为 bottleneck


#### 同步场景下的 Monarchical Leader Election

P 的结果完全可信，所以啥都不用管直接换下一个主就行





![](https://notes.sjtu.edu.cn/uploads/upload_ef8ed598cd51265dff5fe7488f20bb09.png)





其中 rank 可以认为是编号。

同步算法不考虑不一致的问题，是最 vanilla 的版本。可以在此基础上考虑异步问题。


#### 半同步场景下 Eventual Leader Detector

- Eventual Accuracy: 过了一段时间后，所有正确节点相信某一个节点（不一定同一个）
- Eventual Agreement: 过了一段 unknown 时间，相信同一个节点

记 $\Omega$ 为 Eventual Leader Detector






![](https://notes.sjtu.edu.cn/uploads/upload_eeb007960d99e137783036edff54c14c.png)










Lesson 3 Broadcast 广播
====

本节主要讨论 Crash-tolerant broadcast

- reliable broadcast primitives 
- causal-order brodcast primitives

应用：shared registers 共享内存，consensus 共识，multi-party computation 多方计算

#### definition

发送者 $s$ 通过广播给 $\Pi$ 中所有人都发送广播消息 $m$。保证所有 node 都 agree on message。

Best-effort broadcast -> regular broadcast -> Uniform broadcast

## Crash Stop Cases

### Best-effort broadcast

request：<beb, Broadcast | m> 由发送者调用，向 $\Pi$ 发广播
Indication: <beb, Deliver | p, m> 包括发送者在内的所有节点在接受来自 p 的广播时触发回调


算法：request 的节点向其他节点 tcp 广播一次。其他节点如果收到了就 deliver，收不到就收不到了。

Validity: 如果正确节点广播了 m，那么最终所有正确节点 deliver m
No Duplication
No Creation: 不会凭空创造。








通信复杂度 O(N)

### (Regular) Reliable Broadcast

- RB1 - RB3 <= BEB1 - BEB3
- RB4: If a message m is delivered by some correct process, then m is eventually delivered by every correct process.

节点 crash 会打破 tcp 的可靠性

需要使用完美错误检测器

### Lazy Reliable Broadcast

基于 beb 和 fd。核心思想，假设 source 节点挂了，帮他转发一下






![](https://notes.sjtu.edu.cn/uploads/upload_286e546f04ddeaaae2a46faf29372f1d.png)






compexity:

- best case: 一轮结束，消息复杂度 O(N）
- worst case: 依次 crash，消息复杂都 O(N^2)，N 轮结束, O(N) steps


> 支持使用最终完美错误检测器（似乎是 case by case 看出来的）

### Eager Reliable Broadcast 

不论有没有 crash，一直帮他 broadcast。



<img src = https://notes.sjtu.edu.cn/uploads/upload_21e5ffa608c9649c43a539525cca8b41.png width=50%><br>






- O(N^2), O(N)


### Uniform Reliable Broadcast

rrb 只会重传最终正确节点的消息。若一个节点发了广播之后 crash 了，rrb 无法处理。

- URB1 - URB3 <= RB1 = RB3
- URB4: Uniform Agreement: Uniform agreement: If a message m is delivered by some process <u>whether correct or faulty</u>), then m is eventually delivered by every correct process.

> 不能收到消息直接 deliver，在 deliver 之前必须要判断某种条件是否满足，具体条件取决于假设和工具。


### All-Ack Uniform Reliable Broadcast

用于同步场景，针对解决 crash 场景下的 uniform

insight：如果 p 转发了我的 broadcast，相当于 p ack 了我的广播。如果所有正确节点 ack 了我的广播就可以 deliver






![](https://notes.sjtu.edu.cn/uploads/upload_831ce6cb5ef0ce76dd6b392b2e7dfee9.png)








![](https://notes.sjtu.edu.cn/uploads/upload_649b7181c8c9be6ff17fe624292cfa4c.png)






半同步场景，上述方法无效。需要引入 Quarum


#### Causal Order Broadcast

我们希望保持消息的因果性。因为网络波动，即使 m1 导致了 m2，也有可能会 m2 比 m1 先到达。

Happened before 关系：event a happened before b (a -> b) iff

- local order: a 和 b 发生在同一个节点上，a 先于 b
- communication order：存在消息 m，a 事件发送 m，b 事件接受 m
- transitivity：存在 c，a->c && c->b

若 a $\not \rightarrow$ b && $b \not \rightarrow a$, 则 a 和 b 是并发事件  concurrent events

## 考虑消息到达的顺序

### Logical Clocks (Lamport Clocks)

每个节点在本地有一个事件计数器 cnt(p)，每个 step++

p 在每个消息里携带 cnt(p)

当 q 收到 p 的消息时，cnt(q) = max(cnt(q), cnt(p)) + 1

property: If a -> b, then lc(a) < lc(b)，反过来不对。

### Vector Clocks

每个节点 $p_i$ 维护一组 counters，每个节点占一维

- 初始化：$VC_i$[j] = 0, forall j $\in$ [n]
- step 自增：$p_i$ 每经过一个 step，$VC_i$[i]++
- $p_i$ 将 $VC_i[]$ 包含在每个消息中

$p_j$ 收到 $p_i$ 的消息时，

- $VC_j[k] = \max(VC_j[k], VC_i[k]), \forall k$
- $VC_j$[j]++

### Causal Precedence 因果前序

考虑广播之间的因果序。

设 m1, m2 是两个消息，m1 是 m2 的因果前序 iff (m1 causally precedes m2 iff)

- FIFO orfer: 某个节点 pi 先广播了 m1 然后广播 m2
- Message Order：pi 首先 deliver m1，然后广播 m2
- transitivity：存在 m3, m1 causally precedes m3 && m3 causally precedes m2

因果前序不意味着 happened before，即不意味广播事件之间的关系：<crbBroadcast, m1> -> <crbBroadcast, m2>

### Happened-before v2

为了兼容广播的因果序，修改 happened before 定义：

a → b iff

- Local order: both a and b occur at the same process p, and 
event a precedes b according to p’s local clock
- Communication order: 
a is a broadcast event containing message m, whereas 
b is a delivery event for m
- Transitivity:  
there is event c, such that a → c and c → b

此时，m1 causally precedes m2 iff
< crbBroadcast, m1> → < crbBroadcast, m2>

### Causal-order (reliable) broadcast

- CRB1 - CRB4 <= RB1 - RB4
- Causal Delivery: For any message m1 that causally precedes a message m2, i.e., m1 → m2, no process delivers m2 unless it has already delivered m1. （不强调可达性


### Causal-order uniform (reliable) broadcast

是 crb 添加了 uniform 的版本。

- CURB1 - CURB 4 <= URB1 - URB4
- CURB5 <= CRB5



### No-Waiting Causal-Order Broadcast

在 broadcast msg 里携带之前的广播，deliver 时先 deliver 之前的。





![](https://notes.sjtu.edu.cn/uploads/upload_f0e2ee87fcd75b012b2db6d0eb35edf7.png)





缺点：历史太长了


### Garbage-Collection of causal past

针对同步场景，在本地通过别人的广播，记录哪个其他节点 ack 了 m

- 如果所有正确节点 ack 了 m，则把 m 从历史里删了





![](https://notes.sjtu.edu.cn/uploads/upload_2ca5fb342ee432d0ffa51cc163b2f91b.png)






### Waiting Causal-order broadcast

每条消息携带节点此时的因果序编号，其他节点在 deliver 之前等待编号到达。





<img src=https://notes.sjtu.edu.cn/uploads/upload_9a5e5b2e72c1b81929fd1423649fc3cb.png width=80%>





Performance:

- No additional communication steps compared to reliable broadcast algorithm
- O(n) message size: vector clocks


和 waiting 的区别：waiting 可以直接从 past deliver 过去的。


#### uniformed version

在这个场景下，uniform 和 causal order 是独立的两个问题，直接组合一下即可。


# lesson 4 Quorum and Async Broadcasts

## Quorum

Quorums: A collection of subsets of processes $p$

$$
QS = \{Q_1, Q_2. \ldots\}, \forall Q \in QS: Q \subset \Pi
$$

where each pair of quorums have non-empty intersection: $Q_i \cap Q_j \neq \emptyset$

本课程主要考虑对称的情况：|QS| = 2。此时要求 $N \geq 2f + 1$

- 但实际场景也有不对称的应用，例：谷歌的某个服务。

证明：设有 f 个 crash，

- liveness：我们希望剩下的 N-f 个节点能 commit decision，所以他们组成一个 Querum。 |Q| <= N -f 
- safety：保证 Q 相交：$2(N-f) > N$

> 小思考
> |QS| = 2 的时候要求多数。但是 |QS| = 3 的时候考虑 $|\Pi| = 5, QS = \{(0,1,2), (0,3,4), (2,4)\}$。这种情况似乎能支持挂 3 个？否定。

### Majority-Ack Uniform Reliable Broadcast 

考虑 crash-stop 但是网络异步的场景。Q: 这是否等价于 crash-recovery




![](https://notes.sjtu.edu.cn/uploads/upload_a20ea8f0bd0aeb911ad1e0af4ade468a.png)






Performance:

- Best case: 2 communication steps and $O(N^2)$ messages
- Worst case: N/2 + 2 communications and $O(N^2)$ msgs

### Byzantine Quorum System

- 我们仍然按消息的可达性计数求取 quorum，要求 N-f 个节点能做决定。但这次，N-f 个消息里会有错误的。为此，重新调整 N:f 的比例
- 要求最终 ack 的节点中，正确节点多于 byzantine 节点，这样哪怕全是拜占庭节点也不会脑裂。

所以 Quorum 的交应该有至少 f+1 个节点：$|Q_1\cap Q_2| \geq f+1 \Rightarrow N \geq 3f +1$

- $N-f \geq Q > 2f$, Q 中有 f 个不可信
- Q 中正确节点应过半，$Q-f> N-Q=> Q> 1/2(f+N)$（抽屉原理 2Q > f+n)


>  This is because in a 2f + 1 majority, where f processes could be faulty would intersect in at least 1 correct process - there are f + 1 correct processes in the byzantine majority and f correct proccesses outside of it. If both majorities had different f correct proccesses outside, that means that there are different f correct processes in the f + 1 correct processes in the majority. That leaves 1 process which is the same in both majorities. Therefore, if the majority was smaller than 2f + 1, say 2f, it would be possible for two majorities not to intersect and therefore partition the network.
>  https://jelenkovic.xyz/posts/distributed-algorithms/

> 也等价于单次不会有两个 quorum


### Byzantine Broadcast Primitives






![](https://notes.sjtu.edu.cn/uploads/upload_c7f96d314bfdb1a6e37f4de8abeebc95.png)






- 每个广播是独立的：每一个 instance 是一个独立的广播，不互相干扰

## Byzantine Consistent Broadcast

Name: ByzantineConsistentBroadcast, instance bcb, with sender s.


Properties:

- BCB1. Validity: If a correct process p broadcasts a message m, then every correct process eventually delivers m.
- BCB2. No duplication: Every correct process delivers at most one message.
- BCB3. Integrity: If some correct process delivers a message m with sender p and process p is correct, then m was previously broadcast by p.
- BCB4. Consistency: If some correct process delivers a message m and another correct process delivers a message m’, then m = m’. （即使广播者是坏人，deliver 的内容也要是一样的）

Assume

- Asynchronous System
- f < N/3
- No FD.（在拜占庭场景下不容易实现 FD）

解决方案：用 MAC 验证 source 的 id。采用拒绝的策略，依靠 quorum 中正确节点 reject 掉不对的 msg


### Authenticated Echo Broadcast

一个 instance 只广播一条消息：该算法只考虑传递单次消息








![](https://notes.sjtu.edu.cn/uploads/upload_f21725917c65e88b43814d28c7080eda.png)








通过 MAC 验证 echo 的发送者身份，等收到了 2/3 N 的节点时 deliver

> 讨论，fix f，若 N = 3f，则凑够 N-f = 2f 个节点时可以 commit。此时在 2f 个节点内是 f:f 的均势。拜占庭节点可以向第一组 f 个节点发送 m；给第二组 f 个节点发送 m'


Performance

- Time complexity: 2 message delays
- Mesaage complexity: O(n^2)


### Signed Echo Broadcast







![](https://notes.sjtu.edu.cn/uploads/upload_0c28b1e4b230385c03de32c4206644fb.png)







- 3 msg delays
- O(N)
- 多一轮通信，复杂度下降

对比 MAC：

- MAC 必须要两两建立一次通信来汇报 ECHO 消息，导致通信复杂度是 O(N^2)
- pubkey signature 可以先汇总到一个节点，再一并转发所有签名并验证。通信复杂度降至 O(N)，消息长度增大
- 作业 4.1 设计了另一种算法，使用的也是 MAC 但通信复杂度是 O(N)

### MAC-Vector Echo Broadcast

作业 4.1. 要求通信复杂度是 O(N)。并且 MAC 和 sinature 不同，拜占庭节点可以给节点 1 和节点 2 发送不同的 MAC


用 MAC Vector，不考虑阻断的的话与两两通信没区别。
但是较于普通两两通信，这里唯一多的额外条件是 MAC 会 fail（中间人 s）

Intuition: 本来只有 f 个节点最终不可达，所以收到 N-f 个就不等了；但 MAC fail 会导致哪怕收到了 N-f 个消息，会有更多的 fail 了。

所以，假设中心节点是正确节点。他按照 safety 的要求，收到 Q=1/2(N+f) 条 ECHO 时进入 ready 状态，于是 liveness 需要保证：

- 收到 N-f 条消息时尝试 Quorum 投票
- N-f 条消息中只有 N-2f 条消息来自正确节点能保证 MAC 正确
- 即 N-2f >= Q = (N+f)/2  ==> N > 5f+1








![](https://notes.sjtu.edu.cn/uploads/upload_7e6404aa4456843f710431d04fabc6ed.png)









- Message Complexity: O(N)
- Time Complexity: 3 msg delays

Q: N-f / (N+f)/2 +f 写哪个（4f肯定不对）




## Byzantine Reliable Broadcast

上面两个算法中，其他节点依靠 FINAL 消息来 deliver 当前轮次信息。主节点可以选择性发送 FINAL，配合第一轮 f:f:f+1 的模型，导致正确节点之间 deliver 的消息不一致。也就是说，被骗的 f 个节点没有 deliver。

于是进一步地，BRB 应当满足的性质有：

- BRB1–BRB4 <= BCB1–BCB4 in Byzantine consistent broadcast.
- BRB5. Totality: If some message is delivered by any correct process, every correct process eventually delivers a message.


### Authenticated Double-Echo Broadcast

> 场景：错误节点发起广播的时候会对两组正确节点广播 m 和 m'；也会在第二轮的时侯不给 m 发 FINAL

引入第三轮通信，等待足够多的节点（包含至少一个正确节点）ready 之后再 deliver。并且采用广播机制，可以绕过第一轮的干扰 deliver 多数节点的消息。












![](https://notes.sjtu.edu.cn/uploads/upload_2b54e610ee646d43cfc5820158498e7a.png)












![](https://notes.sjtu.edu.cn/uploads/upload_b9b48ef902321ed831fe144891db4770.png)










#### 分析有效性

##### Amplification Process

Suppose a correct process p1 somehow brb-delivers m

- p1 must have received 2f + 1 READY messages with m 
- At least f + 1 of these are from correct processes
- READY messages of these f+1 processes are also delivered by the rest f correct processes, which then send READY messages (Ampl. step)

Every correct process brbdelivers m 

- Since 2f + 1 correct processes have sent a READY message containing m.

##### Consistency

By “Echo” algorithm consistency, no correct process will send a message different from m in READY

##### Totality

- Amplification step is crucial
- Yet, cannot amplify ECHOs
- Need the third “round”

##### Benign or honest process: if a process is correct or has crashed

- i.e., faithfully following the algorithm
- Uniform totality: If some message is delivered by any benign process, every correct process eventually delivers a message.
- Bracha’s algorithm is still correct 

benign process: 一个 process 要么是正确的，要么是 crash 了。


### Eager Signed Echo brbBroadcast

用签名优化的 brb












![](https://notes.sjtu.edu.cn/uploads/upload_e21565780107426579cb537f5d3d7b2f.png)
















![](https://notes.sjtu.edu.cn/uploads/upload_0cf3d4caabe5d87f004ffda838528e45.png)






#### 复杂度

Time complexity

- Best case：2 message delays

Message complexity

- O(N^2)




Lesson 5 Shared Memory
===


类似一个变量，往里读写。接口像是多个进程读写同一块内存

## Overview

Shared memory or registers 封装了存储的读写功能，stores values。

类似 kv store

- redis (memory)
- RocksDb (disk)

### Registers

两个操作

```
Write(v){
x := v
return ACK
}
Read(): return x
```

假设 register 的初始值是空

不考虑拜占庭场景：

- 约定每个进程都不会并发地调用两个操作
- 每个 client 最多只有一个操作处于 pending 状态 (invoked, not completed)





![](https://notes.sjtu.edu.cn/uploads/upload_38e287658c066ef9cd1273353ea74291.png)




### Sequential Operations

Sequential operations op1 and op2

- If indication of op1 precedes request of op2,
- Or indication of op2 precedes request of op1






![](https://notes.sjtu.edu.cn/uploads/upload_84227f57bb27218a8af67e1b303b9c63.png)






### Concurrent Operations

Concurrent operations op1 and op2

- If neither indication of op1 precedes request of op2
- Nor indication of op2 precedes request of op1




![](https://notes.sjtu.edu.cn/uploads/upload_6333ef67883621604dd9dc6ccca849fd.png)





需要讨论在这种情况下，我们预期结果是什么

## Properties

#### Liveness

- Wait-Freedom
- Every operation invoked by a correct process must eventually complete

#### Safety

- If no concurrent write
    - 每个 read operation 返回最近写操作的值
- If concurrent writes exists
    - Safe
    - Regular
    - Atomic semantics

### Register Semantics

Safe, Regular, Atomic

- All strongly consistent in absence of r/w concurrency 没有并发的情况，三种 register 都返回最近的 w

Safe Semantics

- 多个 process，只有一个节点有权利写：Single Writer
- Read() 操作如果没有和任何 w 操作有并发操作，返回最近值
- 和某个 w 操作并发时，返回任意值

Regular Register

- Single Writer
- Read() returns:
    - the last value written if it is not concurrent with any write()
    - Otherwise, either
    - the last value written, or 
    - any value concurrently written, i.e., the input parameter of some concurrent Write()(允许读 pre-> last -> pre)


## Regular Registers

逻辑上是共享变量，实现方式是分布式多节点维护。

- 只需要在每个节点实现 Read() / Write()
- 读写返回之前要通信同步状态

具体而言

- 在 reliable p2p channel 通信
- 每个节点 $p_i$ 维护了 register 状态 $v_i$  的 copy 
    - “我认为这个变量是什么”

### (1, N) regular register Interface Spec

Interface for sync and async `onrr`, one-n regular registers. 

Request: 

- < onrr, Read >: Invokes a read operation on the register.
- < onrr, Write | v >: Invokes a write operation with value v on the register.

Indication:

- < onrr, ReadReturn | v >: Completes a read operation on the register with return value v.
- < onrr, WriteReturn >: Completes a write operation on the register.

Properties:

- ONRR1. Termination: If a correct process invokes an operation, then the operation eventually completes.
- ONRR2. Validity: A read that is not concurrent with a write returns the last value written; a read that is concurrent with a write returns the last value written or the value concurrently written.

最终反馈 + 返回值非随机

### Sync: Read-One Write-All

同步网路。每个节点本地维护他认知中的全局状态。

- 读的时候从本地读
- 写的时候广播 write all 并等待





![](https://notes.sjtu.edu.cn/uploads/upload_2aca16509dc3f7f84cda3b3e36070827.png)




一直可以读，writer 节点是 correct 时候可以写，可以容忍任意个数故障（同步）


### Async: Majority Voting Regular Register
#### A Tight Lower Bound

Theorem: any wait-free asynchronous implementation of a regular register requires a majority of correct processes, assuming at least 1 writer and 1 reader distinct from each other.

只能允许半数以下节点故障。f<n/2


#### The Majority Algorithm

一些假设

- p1 是 single writer
- 多数节点正确
- Reliable channels

#### Intuition:

- writer p1 维护一个写操作的 timestamp（以防网络波动导致乱序
- 每个节点 pi 维护
    - a local copies of the register value `val` and timestamp `ts` 
    - as well as a read timestamp `rid`
- 隐形条件：同一个节点同时只有一个 pending 的 onrr 请求。


只需要在读操作和写操作时，都访问多数节点等待 Quorum 回复，然后收集到过半数的 ack 时 return 即可。


#### Algorithm





![](https://notes.sjtu.edu.cn/uploads/upload_b6a0082ac6a59f78f3447df9798d4b4e.png)







#### Performance

- 1 RTT(2 steps)
- O(N) msgs



## Atomic Registers
### Atomicity (Linearizability)

操作是可线性化的 (Linearizability), 并且是瞬间完成的（Atomicity）

- Atomicity (Linearizability)：
    - Strong consistency even when there is read/write concurrency and failures
- Atomic (linearizable) execution is equivalent to a sequential and failure-free execution

对于每一个完成的操作来说

- 该次执行等价于在 req 和 indct 之间的某一个瞬间完成。
- appears to be executed at some instant between its request and indication events

Every failed (write) operation 
- appears to be either complete or not to have been invoked at all

典型反例
![](https://notes.sjtu.edu.cn/uploads/upload_01e779ec1fb7d9674f167ac29064b1d7.png)

典型正例
![](https://notes.sjtu.edu.cn/uploads/upload_313a53095f793f8b77be0e9d964bf7a5.png)


#### key observation for ar.

需要避免出现先读新值再读旧值的情况。

- 对于同步网络，是写结束前读一次新一次旧
- 对于异步网络，也是写结束前，已经写过的节点都不在 quorum 里

### Interface: (1, N) Atomic Register

#### Properties:

- ONAR1 – ONAR2: <= ONRR1 – ONRR2 of a (1, N) regular register. 
- ONAR3. Ordering: If a read returns a value v and a subsequent read returns a value w, then the write of w does not precede the write of v.




![](https://notes.sjtu.edu.cn/uploads/upload_163e6f82332004ac44e32b6887b39843.png)




### 同步网络的 Atomic Register

支持任意数量挂掉。

#### Motivating Counter Example






![](https://notes.sjtu.edu.cn/uploads/upload_d129a35d562e6054d86281fcb186eb81.png)





之前 onrr 在同步网络的版本是从本地读，向所有正确节点写。如果有三个节点

- p2 写 1，p1 收到并紧接着读 1.然后 p1p2 一起 crash 了
- 仅剩的 p3 还在 0 （他俩在 2Delta 内干了太多事）


#### Intuition：读完了之后对所有节点进行一次写回 writeback

- If a read succeeds, every (correct) process should be aware of it. 
- Impose the read value (or write-back) globally

（写回的实现方式是广播 ts,v。在同步网络场景下，相当于等待所有正确节点 ACK 之后再 deliver read


> 所以其实 2Delta 同步不代表所有 request 都对齐到 2Delta。可以在 deliver 前做很多事。






![](https://notes.sjtu.edu.cn/uploads/upload_47806bc6da6b02dd8df7449660eb8f38.png)





#### Performance

- 1 RTT
- O(N) msgs


### 异步网路的 Atomic Registers

#### Assumptions

- Correct majorities
- Asynchronous system
- No FD

#### Idea

- Extend regular SWMR (“majority algorithm”) with Write-back phase at readers
- Wait for majority of responses everywhere
- 写到多数，读到多数，写回到多数

> FD 和 quorum 有点可以互换





![](https://notes.sjtu.edu.cn/uploads/upload_3d030269911a347e2b1d4aaf27a3fdbe.png)






![](https://notes.sjtu.edu.cn/uploads/upload_d9b7ab3d462166ee8406e3e757643a8c.png)








#### 正确性

- read 操作 deliver 之前一定完成了多数节点的写回，保证多数节点至少见到过自己读到的 ts 
- deliver 时间在自己后面的 read 操作

#### Complexity

- 1 RTT（w) / 2 RTT \(r\)
- O(N) msg


## Multiple Writers

目前的生产系统都是 single writer，保证数据一致性

因为并发 writer 的时候，不好处理消息的聚合。常常会直接丢掉。







# Lesson 6: Consensus


(One of) the most fundamental problem(s) in distributed computing


Key to solving many other problems in distributed computing; e.g., 

- total order broadcast, 或者叫做 (state machine)　replication
- atomic commit
- terminating reliable broadcast

> 即便是 bcb 广播和因果序广播依然不会保证所有广播的顺序一致。

In the consensus problem, the processes propose values and have to agree on one among these values 


## Basic Concept

a group of n processes, among which some (usually f) may 

- crash (Crash fault), or 
- behave arbitrarily (Byzantine fault)

the problem is for correct processes to agree on a value

- 在非拜占庭场景下，可以要求 uniform

every process can propose, finally every correct process decides


## Interface for Consensus

Name: Consensus, instance c.

Request: < c, Propose | v >: Proposes value v for consensus.
Indication: < c, Decide | v >: Outputs a decided value v of consensus.

Properties:
C1. Termination: Every correct process eventually decides some value.
C2. Validity: If a process decides v, then v was proposed by some process.
C3. Integrity: No process decides twice.
C4. Agreement: No two correct processes decide differently.
C5. Uniform Agreement: No two processes decide differently.


## Industry Deployment

1. Meta-data (configuration) management
   - Google Chubby(管理数据中心元数据，闭源)
   - Apache ZooKeeper(Chubby 的开源复现)
   - etcd
2. Large-scale database systems 分布式
    - Google Spanner
    - Tencent PaxosStore
    - Ant OceanBase
    - TiDB
    - CockroachDB
    - Huawei openGauss
3. Blockchain
    - PoW, PoS, permissioned blockchains
    - (blockchain 出来之前是航空航天用的多)

## Paxoses

单个 instance 只共识一次

### 同步网络的场景

(Uniform) Consensus Algorithms using failure detectors

I: Consensus using P
II: Uniform consiensus using P


### 例 I: Hierarchical Consensus

> 同步网络，完美 P 的情形。n 轮达成一次共识。
> 只保证正确节点的共识，不满足 uniform

The processes go through rounds incrementally (1 to n): 

- in each round, the process with the id corresponding to that round is the leader of the round
- The leader of a round decides its current proposal and broadcasts it to all
- A process that is not leader in a round waits 
    (a) to deliver the proposal of the leader in that round to adopt it, or 
    (b) to suspect the leader
- 第 i 轮，主节点是 pi
- 由主节点 decide 当前轮次的 proposal
- 非主节点等待 proposal 或怀疑主节点
    





![](https://notes.sjtu.edu.cn/uploads/upload_490d0b82157ed3258aabd632a524f172.png)

    
    
    
    







![](https://notes.sjtu.edu.cn/uploads/upload_15588ecc3b7ffb84a33ab3251c9f4652.png)





> 这里 r < rank(self) 的要求，是因为只有 crash 会换主。而自己是好的。所以要么自己是主，要么主的序号更低。但是为什么要检查小于




### 例 II: Uniform Consensus

> 同步网络 P，n 轮达成一次共识。
> 但 uniform (all processes make the same decision)

C1. Termination: Every correct process eventually decides some value.
C2. Validity: If a process decides v, then v was proposed by some process.
C3. Integrity: No process decides twice.
C4. Uniform Agreement: No two processes decide differently.


#### Intuition

The processes go through rounds incrementally (1 to n): 

- `On request`, cache proposal locally
- in each round i, process pi sends its proposal to all

A process adopts any proposal it receives

- If the proposal is sent in previous round and more recent than its current proposal

Process decide on their proposal at the end of round n


> 其实和上一个是一样的，就是 decide 要等到 n






![](https://notes.sjtu.edu.cn/uploads/upload_5af8b0a8c746a314d7ff53836363d42f.png)








![](https://notes.sjtu.edu.cn/uploads/upload_69148219855183d9a5ce511a82be215d.png)





> 同步场景只需要防止 crash 导致包 deliver 了但没发出去就行。所以这个改进就是在末尾广播 deliver 了之后再 decide。


## Total Order Broadcast Agenda / StateMachine Replication

实用的共识算法需要反复进行共识。从而实现 Total Order Broadcast

#### 回顾：Reliable Broadcast 和 Causal Order

Reliable broadcast

- the processes are free to deliver messages in any order they wish
- 能到就行

Causal broadcast

- the processes need to deliver messages according to some order (causal order) 
- The order imposed by causal broadcast is however partial
- Causally unrelated messages might be delivered in different order by the processes
- causal：同一个节点内的消息先后顺序


### Total Order

每个节点 deliver 广播消息的顺序完全相同。（不一定 causal）




![](https://notes.sjtu.edu.cn/uploads/upload_25e4f717ec614efb4618810459506a34.png)




Replicated services
- State machine replication (SMR) 
- Where the replicas need to treat the requests in the same order to preserve consistency




### Interface Spec. for Total Order Broadcasts

Name: TotalOrderBroadcast, instance tob.

Request: < tob, Broadcast | m >: Broadcasts a message m to all processes.
Indication: < tob, Deliver | p, m >: Delivers a message m broadcast by process p.

Properties:

TOB1. Validity: If a correct process p broadcasts a message m, then p eventually delivers m.
TOB2. No duplication: No message is delivered more than once.
TOB3. No creation: If a process delivers a message m with sender s, then m was previously broadcast by process s.
TOB4. Agreement: If a message m is delivered by some correct process, then m is eventually delivered by every correct process.
TOB5. **Total order**: Let m1 and m2 be any two messages and suppose p and q are any two correct processes that deliver m1 and m2. If p delivers m1 before m2, then q delivers m1 before m2.



### 全序广播和共识是等价的

#### 由 consensus 构造 tob





![](https://notes.sjtu.edu.cn/uploads/upload_d4a1ba81c8c6866ea8a4fc7d2c0176e2.png)






用 rb 广播消息，rb deliver 收到后先把消息放到 unorderer 集合里。然后启动 c.1, c.2, ... 的共识实例排序。（细节：共识只保证 unordered 集合相等；然后靠固定算法排序即可。



#### 由 tob 构造 consensus

作业 6.1

```golang
upon event < c, init > do
	decided := false;
	proposal := ⊥;

upon event < c, Propose | v > such that proposal = ⊥ do
	proposal := v;
	trigger < tob, Broadcast | proposal >;

upon event < tob, Deliver | p, m > do
	 if decided = FALSE then
		decided := TRUE;
		trigger < c, Decide | m >;
```




## Paxoses: contd - 异步场景的共识

> 单个 instance 只共识一次

### Recap：半同步

Quorum = f + 1

但是如果有 f 个节点看起来挂了 (crash or network)，怎么保证共识的 uniform 呢？

- 需要实现类似同步网络中轮流发言的效果。
- 完全异步的网络下，共识不可解


#### 半同步网络

**asynchrony** 异步网络

- correct processes cannot communicate in a timely manner (within a known bound ∆_
- packet loss due to network congestion
- 完全无法给出 $\Delta$

**synchrony**

∆ is given beforehand (no network fault)

**partial synchrony**

- $\Delta$ 存在且已知，但需要先完全异步一段时间之后才进入 $\Delta$
- after some unknown time, time bound ∆ is satisfied
- 我们要把系统当作混乱的，虽然知道他最终能恢复，但要保证混乱的时候能达成 agreement

### 例 III：Paxos

我们将实现一个 uniform consensus, using:

- Quorum(majority) + $\diamond P$
- which guarantees: agreement + liveness

#### Idea

processes alternate in the role of a leader (coordinator) 

- until one of them succeeds in imposing a decision
- cluster prioritizes safety (agreement) rather than liveness (termination)


#### Intuition 算法思路

The processes go through rounds incrementally:

- in each round k, such that $k \mod n = i$, $p_i$ is the leader
- using mod because k might go beyond n

In such a round k, $p_i$ tries to decide

- If $p_i$ succeeds, consensus terminate immediately (might before n)
- $p_i$ succeeds if it is not suspected(not only if, see below)
- processes that suspect $p_i$ inform $p_i$ and move to the next round
- $p_i$ does so as well


To decide, $p_i$ does the following 

1. $p_i$ selects among a majority the latest adopted value  (latest with respect to the round in which the value is adopted – see step 2) （先找其他节点学习）
2. $p_i$ imposes that value at a majority: any process in that majority adopts that value – pi fails if it is suspected （把学习结果引入到 majority）
3. $p_i$ decides and broadcasts the decision to all （成功）

Special case, step 1

- if no value was adopted by any process in a given majority, pi imposes its own initial (proposal) value in step 2







![](https://notes.sjtu.edu.cn/uploads/upload_18041bc502ef51ef4f91b8699a0838a6.png)






#### pseudo code









![](https://notes.sjtu.edu.cn/uploads/upload_9851ed56d648c31c75f93357e9d39ee9.png)







说明：

- `estimate`, `estround` 潜在的共识结果和该结果的轮次
- `rank(self) = round` 隐式模 n
- `[READ, round]`, `[GATHER, ...]` 表示第一轮的学习
- GATHER 过半时，选出 estround 最高的学习消息，作为当次 proposal 发起 IMPOSE
- 收到 IMPOSE 时，比较 round 后更新本地变量返回 ACK 
- 主节点收到 ACK 时后，本地 deliver，并广播 DECIDE
- 过半节点更新 estimate 时，系统的 consensus 结果已经确定

对于 suspected 的主节点，广播 NACK，所有节点记录 nack 的节点(rnd)，并跳过 nack 的主节点










![](https://notes.sjtu.edu.cn/uploads/upload_e7bfd729806c87723e2da75d453db291.png)














![](https://notes.sjtu.edu.cn/uploads/upload_37d81a8ad8800c508c3c809b901f622f.png)









#### 为什么要这样做？

最简单地说，是为了把 Quorum 引入进来

1. ACK 和 READ 都完成了 Quorum 状态的更新。下一次一定会继承上一次结果。（只要 majority 完成了 IMPOSE，值就能传递
2. 如果主节点在本地 decide 后，广播 DECIDE 前挂了，其他节点的 estimate 会在后续轮次复原该值（使用最高 rnd number 保证安全性）
3. 当有主节点不被 $\diamond P$ 怀疑时，算法可以进行下去







![](https://notes.sjtu.edu.cn/uploads/upload_50b61dfd0f5338da71eedfe37efc3465.png)







#### prove for agreement

Let k be the first round in which some process $p_i$ decides some value v, i.e.,  $p_i$ is the leader of round k and $p_i$ decides v in k

- This means that, in round k,  a majority of processes adopt v
- The algorithm guarantees that no value other than v will be imposed (and hence decided) by any process in a round higher than k (Quorum)

> 事实上，这个算法并没有借助 FD，$\diamond P$ 只用来保证网络混乱结束后算法能 terminate。FD 如果既不是 P, 也不是 $\diamond P$，共识除了 termination 满足不了，agreement 和 safety 都不会破环


## 现实世界的 Paxos(Synod) 算法

之前写的 Paxos 是课堂版本的。实际生产中进行了一定程度的解耦，把保证 liveness(termination) 性质的步骤和保证 agreement 性质的步骤分隔开。

> 分离了 correct-majority 假设和最终能选出主节点的假设

Paxos 包括两个组件：

- Ω - the eventual leader detector（基于 $\diamond P$)
- (ofcons) Obstruction-free consensus (read phase + impose phase)


#### Consensus vs. OF-Consensus

OF-Consensus 可以 abort，Consensus 一定会最终达成一个 decide

### Interface Spec. for Eventual Leader Detector

Name: EventualLeaderDetector, instance Ω.

Indication: < Ω, Trust | p >: Indicates that process p is trusted to be leader.

Properties:

- ELD1. Eventual accuracy: There is a time after which every correct process trusts some correct process.
- ELD2. Eventual agreement: There is a time after which no two correct processes trust different correct processes.

### Interface Spec for Obstruction-free Consensus (ofcons)

Request: < ofcons, Propose | v > 

Indications: 两种输出可能：

- < ofcons, Decide | v >
- < ofcons, Abort >

Very similar to consensus 

- except for Termination 
- ability to abort

#### Properties

Validity: Any value decided is a value proposed 

Agreement: No two correct processes decide differently 

Obstruction-Free Termination: 

- If a correct process p proposes, every correct process eventually decides or aborts
- Moreover, if a single correct process proposes a value infinitely many times, it does not abort forever

Integrity: Every process decides at most once


### Consensus based on Eventual LD. and OF-Consensus


```golang
init: decided := false;

upon event < uc, Propose | v > do
	while not(decided) do
		if self = leader() then // leader() uses Omega to tell whether I am master
			trigger < ofcons, Propose | v >;
			wait for < ofcons, Decide | v > or < ofcons, Abort >;

upon event < ofcons, Decide | v' > do
	decided := true;
	trigger < cons, Decide | v' >;
```

### OF-Consensus

> 不依赖任何网络假设，即在完全异步的网络环境下也能运行。只依赖 correct majority 的假设

类似例 III 的 READ & IMPOSE，OFCons 也是一个两阶段算法。换主之后要先 READ











![](https://notes.sjtu.edu.cn/uploads/upload_ee566a4a8eaecb61d1af0ef3124e36b1.png)









- ballot 类似 roundnumber
- 为了节省通信 RTT，IMPOSE 成功后主节点采用广播而不是 pl








![](https://notes.sjtu.edu.cn/uploads/upload_879ac25cff83016da5a3aab9937554a7.png)







#### 生产环境的优化

1. 第一轮的时候，READ Phase 省略（因为消息都是空的
2. 目前的算法 (Synod) 只支持单次共识，不支持连续共识。Paxos 在 Synod 基础上支持连续共识。

在原始的 Paxos 版本，需要实例化多个 instance。（也叫做 multi-paxos）

- Clients initiate requests （在 cluster 外）
- Servers (processes) run consensus
- Multiple instances of consensus (Synod)
- Eg: Synod instance 25 to agree on the 25th request

这意味着共识状态是二维的。





![](https://notes.sjtu.edu.cn/uploads/upload_c8fcab25a358f043d1d2d92bbe1ad81c.png)









![](https://notes.sjtu.edu.cn/uploads/upload_1081da23c4a2f05e508fe6b1c81b7e74.png)












- 横坐标是 SeqNumber(instance[i])
- 纵坐标是 roundNum/ballot（仅在换主时++）
- 在连续共识场景下，换主后，可以一次性对所有 SeqNum 进行 READ。在这之后不用对更高的 SeqNum 进行第一轮 READ
- 换主需要学习所有历史 commit(decide) --- 优化： checkpoints(eg: 每 100 次记录 chkpt)

Both clients and processes have the (unreliable) estimate of the current leader (some process)

- Clients send requests to the leader
- The leader replies to the client

> 主节点只要不被换掉，就会在不同的 instance 发起不同的共识。

> 通常，共识消息会附带执行的命令 `c.ex`


#### 生产环境优化 cont.d：READ phase

由于主节点多数情况下不会换。所以 READ phase 可以进一步优化

- 仅在刚成为主节点的时候，查询所有 instance 的 estimate 值（切换主）
- 不换主的时候，直接 IMPOSE 即可。两次通信完成共识

> Run READ phase only when the leader changes
> - and for multiple Synod instances simultaneously

> Use the same ballot number for all future Synod instances 
> - run only IMPOSE phases in future instances
> - Each message includes ballot number (from the last READ phase) and SeqNum, e.g., SeqNum = 11 when we're trying to agree what the 11th operation should be

> When a process increments a ballot number it also READs
> - e.g., when leader changes


### NOOP

在实际工程实现上，不同 seqNum 的共识是并行进行的。所以可能后面的做完了但前面的还没有。

此时，如果换主需要学习，可能 [4] 完成了但 [3] 还没完成。需要用 NOOP 标记 [3]



> 好消息，后面全是共识

# Lesson 7 - Consensus Cont.d: FLP and Randomized Consensus

## FLP Impossibility

**Asynchronous deterministic** consensus with $n\geq 2$ processes is impossible even with one (crash) faulty process.

- 确定性共识：算法确定性地执行 N 步后结束。（广播可以）
- 即便是 01 共识(binary consensus) 也不存在确定性共识算法

> Michael J. Fischer, Nancy A. Lynch, and Michael S. Paterson for "Impossibility of  Distributed Consensus with One Faulty Process," Journal of the ACM, 32(2):374-382, April 1985.

> 不是一定不会停，而是一定存在停不下来的路径（二价到二价）。也就是说在异步场景下，步骤无上界

### Proof Sketch

对于任何非随机异步分布式算法，集群的最终状态 $C=(S_1, S_2, \ldots, S_n)$ 只取决于初始状态和每个节点接收消息的顺序。

- 用 $(p, m)$ 表示节点 p 接收到 m 的事件(event)
- def: $(p, m)$ is enabled if m is in a channel to p and p is correct

如果假设一个全局调度器(scheduler)管理接收消息的顺序，就控制了 cluster 


#### Fairness

An execution is fair if for every (p,m), if (p,m) is enabled then it eventually occurs

When (p,m) occurs?

- Eventually
- When exactly?: depends on the asynchronous scheduler


A configuration C is

- Univalent（一价状态）, if in all possible executions (that extend C), in all reachable configurations the same value is decided
	- i.e., the outcome of consensus is predictable from C
	- 0-valent, if this value is 0
	- 1-valent, if this value is 1
- Otherwise, the configuration is bivalent（二价状态）
	- i.e., the outcome of consensus is unpredictable from C

任何分布式算法都是将系统从二价状态转换到一价状态

> 在 PAXOS 例 3 中，有多数节点完成 IMPOSE 后，算法结果固定，进入一价状态

#### FLP Idea

1) Show that there is a bivalent initial configuration()
2) Starting from a bivalent configuration, show that there always exists a path that reaches another bivalent configuration


#### Bivalent Initialization Lemma (BIL)

Proof idea:

- Assume, by contradiction, all initial configurations are univalent
- Use the fact that 1 process might fail
- Show contradiction




![](https://notes.sjtu.edu.cn/uploads/upload_bd621b33989d87e2d21e98498089e503.png)





Assume 2 executions ex and ex', ex starts from C, ex' starts from C’. $p_i$ fails upon initialization in both ex and ex’, other than initial config ex and ex' are identical. Then no process other than $p_i$ (i.e., no correct process) can distinguish ex from ex’. 

- Should decide the same in both executions
- But in C they decide 0 and in C' they decide 1

#### Extension Lemma

一个 bivalent configuration 一定会再次走入另一个 bivalent configuration。






![](https://notes.sjtu.edu.cn/uploads/upload_aa790717e32bbd0ee010f4fb086f8622.png)





> 具体内容略


## 随机共识

要想设计一个可用的分布式算法，需要引入一些假设打破 FLP。

- Timing assumptions (failure detectors, synchrony)
- No possible failures
- Randomization 随机共识

在前文中，通过 FD 和随机性保证

- Failure detectors: $P, \diamond P, \Omega$
- Probabilistic guarantees: Consensus properties hold probabilistically








![](https://notes.sjtu.edu.cn/uploads/upload_e41bfd7b7c1fbc93b797666468a34612.png)









随机共识的效率也可以很高；并且不需要选主，算法是对称的

### Interface Spec. Randomized Consensus

Name: RandomizedConsensus, instance rc.

Request: < rc, Propose | v >: Proposes value v for consensus.
Indication: < rc, Decide | v >: Outputs a decided value v of consensus.

Properties:
RC1. **Probabilistic termination**: With probability 1, every correct process eventually decides some value. (趋近于 1，与 $\Delta$ 无关，不依赖网络假设）
C2. Validity: If a process decides v, then v was proposed by some process.
C3. Integrity: No process decides twice.
C4. Agreement: No two (correct) processes decide differently.


只有 termination 和普通 Consensus 有区别，变成 probabilistic


### Extension: Coin 随机数生成组件

We extend our (deterministic) model as follows

1) every process has access to a source of randomness (called a **coin**)
2) the local computation part in every step of a process may now additionally depend on the output of the coin
	- picks a random element from a finite set according to a fixed probability distribution
	- We consider random elements with uniform distribution

We say that the algorithm flips (or tosses) a coin in this case


### Interface Spec for Common Coin

Name: CommonCoin, instance coin, with domain $B$.

Request: < coin, Release >: Releases the coin.
Indication: < coin, Output | b >: Outputs the coin value $b ∈ B$.

Properties:

COIN1. Termination: Every correct process eventually outputs a coin value.

COIN2. Unpredictability: Unless at least one correct process has released the coin, no process has any information about the coin output by a correct process.

- 用于证明 termination：scheduler 也不知道随机数是什么。这样多轮之后 scheduler 不可能总恰好调度到共识失败的执行顺序

COIN3. Matching: With probability at least δ, every correct process outputs the same coin value.

- 保证算法能收敛

COIN4. No bias: In the event that all correct processes output the same coin value, the distribution of the coin is uniform over $B$ (i.e. a matching coin outputs any value in $B$ with probability $\frac{1}{\#(B)}$ ).

### Common Impl. for Common Coin

我们其实希望各个节点随机数是一样的

- Local Toss: 每个节点本地 roll 随机数（缺点：$\delta$ 太小不利于达成共识）
- Beacon ($\delta=1$)：有一个类似 Random Oracle 的节点统一分配
- Threshold signature scheme ($\delta=1$)
- Many others…


#### Local Toss

Upon releasing the coin, every process simply selects a value c at random from D according to the uniform distribution, and outputs c. 

If the domain is one bit then this realizes a $\delta = 2^{-N+1}$ matching common coin, where $N$ is number of processes

- Probability that every process selects the same $c\in \{0,1\}$ is $2^{-N}$


#### Beacon Algorithm

Beacon 是一个可信的第三方，统一管理随机数。（单节点非分布式

Small cheat: external process

- Not really distributed

An external trusted process, called the beacon, periodically:

- chooses an unpredictable random value

When an algorithm accesses a sequence of common coins 

- for the k-th coin, every process receives the k-th random value from the beacon and outputs it
- This coin matches always $(\delta=1)$.


#### Threshold Signature Scheme

门限签名。没展开



### Randomized Binary Consensus

For binary consensus, domain for proposal/decision values is {0,1}. We use Correct Majority Assumption and Randomization to build consensus. (w/o network assumption for leader election)

Round-based algorithm

- In every round, processes try to ensure that the same value is proposed by a majority of the processes 
- If there is no such value, the processes resort to the coin. Coin selects a proposal for the next round

> 例：crash 了 f 个节点，希望剩下的都提议 1 or 0

Probability that the processes agree might depend on $\delta$ 

- unless all (initial) proposals are the same
- The algorithm terminates eventually with probability 1


#### Intuition

Round-based algorithm, each round contains 2 phases

In phase 1:

- every correct process proposes a value by sending it to all processes with a best-effort broadcast 
	- Then it receives proposals from a majority of process
- On receive:
	- If all proposals from majority are the same, proposal is propagated in the 2nd phase
	- Else the process propagates ⊥ in the 2nd phase

**Invariant:** If two processes propagate v1,v2 ≠ ⊥ in the 2nd phase then v1 = v2 = v (majority)

> 进入第二轮的消息要么是 $\perp$ 要么是同一个 v

In Phase 2:

1. After a process receives a majority of phase 2 msgs, it checks if all are equal to v.
2. If yes: process decides v and reliable-broadcasts the decision
3. Elif $v \neq \perp$ exists: adopt v as the proposal in the next round
4. Else adopt the outcome of a coin as the proposal in the next round



#### Pseudo Code







![](https://notes.sjtu.edu.cn/uploads/upload_c123768130d77b890cd4ed6aafe15451.png)










其中，

- coin.round, Output 里的第一个 if 用于验证第二轮收到的 v。可以不依赖于 coin，现在的写法只是合并
- 在 N > 2f + 1 时，upon \#(val) > N/2  可以改为 \#(val) > N - f。但该函数内 $\#\{\cdots\} > N/2$ 条件不变(Quorum)。即可以接收更多的消息让共识更容易走下去
- Phase 2 里，f



![](https://notes.sjtu.edu.cn/uploads/upload_2da844a306e6c98794dec24c96e9b4b4.png)


> NOTE: 该算法不保证所有节点（甚至是多数节点）在同一个 round 会 decide。在上面的例子里，只有 0 在 round 1 decide 了。其他节点都收到了 $1, \perp, \perp$。因此，**需要坚持非空的 v**，否则后续共识可能和前面 decide 不同结果


![](https://notes.sjtu.edu.cn/uploads/upload_f55bc3a67d5a291bfa9f58fa2a2f23da.png)


#### termination

validity、agreement 略

termination：当 f 个节点坚持上一轮 decide 的时候，qi

#### Performance

Every round have 2 message delays and $O(n^2)$ messages

How many rounds:

- With probability 0, infinite
- Expected number of rounds is proportional to $1/\delta$


## Randomized Multivalued Consensus(Asynchronous Common Subset, ACS)

> 从 01 共识构造多值共识

直接让 coin 在很大的 B 上尝试投出相同的随机数并不现实，但是可以让每个节点提议一个多值，然后选择 放入/不放入 Common Subset







![](https://notes.sjtu.edu.cn/uploads/upload_a987df498f1cfb6121b7d424aeb29016.png)





- n 轮 01 共识 decide 一个多值的值
- 如果有 f 个节点 crash 了，#decidedone >= N-f 条件触发，帮 crash 的 f 个节点共识 0，最后实现计数等于 n






![](https://notes.sjtu.edu.cn/uploads/upload_a4bd72a6dda2a41c67e1b7622cb76f53.png)






# Lesson 8 - Byzantine Consensus

> 在区块链出现后走向实用。但区块链共识方法是开放网络的共识，和本课程是两个方向

## 讨论：Validity 是否接受 Byzantine 的 propose

原始的 validity 定义为：Validity: If a process decides v, then v was proposed by some process.

但如果正确节点还没 propose，只有 Byzantine 节点在 propose，则何如


#### Strong Validity

If all correct processes propose v, then no correct process decides a value other than v


#### Weak Validity

If all processes are correct and propose the same value v, then no correct process decides a value different from v; furthermore, if all processes are correct and some process decides v, then v was proposed by some process.


> 以下默认所有通信都有 signature 数字签名保护（主节点广播需要证明信息来源）；同样地，需要 3f + 1 个节点

## PBFT

应用 Paxos 思想来解决 Byzantine 问题的算法

### 挑战

拜占庭节点可能

1. 抢占好人的 round：每轮刚开始就换主
2. 正确主节点需要选 highestrank 的 READ 结果进行 IMPOSE，拜占庭节点可能选之前的
3. 不保证 estimate 能传递到下一个 round：在下一轮 READ 的时候报告别的值


### 解决方案

#### 1. When to enter a new round

> 共识本身就是抢占，paxos 需要主节点抢占

换主不依靠主节点自己切换。各个节点自主决定是否需要更新 round，然后发送 gather 给新主节点。某个节点收到足够多 gather 后节点才会变成主节点（由于 2 要附带签名，后续步骤可验证）。

#### 2. Ensure the new leader selects the highest round

在 IMPOSE 的时候，附带 READ 阶段的 2f + 1 条 gather 消息。通过其中的签名，允许节点自行计数验证 IMPOSE 的值确实是合法的。

#### 3. Add a phase to update estimate

需要两次 All-to-All

- 第一次保证了存在一个 2/3 的节点(Quorum) 认可的 estimate
- 第二次保证传播了足够多的节点，有足够多的节点在本地更新了 estimate，并携带了 2/3 节点的签名。保证 estimate 可以到下一个 round



![](https://notes.sjtu.edu.cn/uploads/upload_83aff3af36532cd5196087675fcad35d.png)


### pseudo code

> 在 PBFT 中，Paxos 的 GATHER 消息被命名为 VIEWCHAGE; IMPOSE 消息被命名为 NEWVIEW

VIEWCHANGE(leader change) - NEWVIEW(impose) - PREPARE(update estimate) - COMMIT(decide)

略。见 ppt


### Performance

Message complexity - best case: $O(n^2)$

However, communication complexity is $O(n^3)$

- The new leader broadcasts NEWVIEW message to O(n) processes
- Each NEWVIEW message contains O(n) VIEWCHANGE messages
- Each VIEWCHANGE message contains O(n) PREPARE messages


## HotStuff

HotStuff is proposed in 2019. It is still a leader-based protocol, running in **partially synchronous** model

> 作者是交大 cs 博士，在 VMware 实习的时候和 VMware research 的人做的

HotStuff has the following features

- Rotate the leader-role proactively 主动换主
- Linear view change：O(n)
- optimistic responsiveness: New leader needs to wait just for the first $N − f$ responses to guarantee that it can create a proposal that will make progress. 

> PBFT is also optimistic responsive


#### General idea

The leader takes charge of message dissemination/aggregation 
Using (n, 2f+1) threshold signature to combine 2f+1 signature shares
$O(n^2)$ -> O(n)
Using a lock to prevent forwarding VIEWCHANGE messages (in PBFT) 
Similar to Tendermint


# Lesson 9 -  Consensus Variants 共识变种

分布式场景的很多问题等价于共识。本节考虑 Crash Stop 场景。

Consensus variants

- Terminating Reliable Broadcast
- Group Membership
- Non-Blocking Atomic Commit

## Terminating (Uniform) Reliable Broadcast

带终止机制的可靠广播（Terminating Reliable Broadcast），比 URB 更强

回顾普通 URB: 广播者如果是好节点，消息最终会被所有节点接受；如果广播者 crash 了，消息可能被接受也可能被抛弃。

**Terminating**: 如果广播者 crash 了，正确节点也需要 deliver 一个消息。用 $\Delta$ 表示该 crash 消息。

> T(U)RB = Terminating + URB（一致性）

TRB 具有一个提前约定好的广播者 src，所有节点预先知道 src 是谁。并且，一个 TRB instance 一定会 deliver 一个消息


### Interface Spec for Uniform Terminating Reliable Broadcast

#### terminologies

1. Defined for a specific broadcaster process: pi = src (known by all processes)
2. Process src is supposed to broadcast a message $m\in M$ (distinct from $\Delta$, where $\Delta \notin M$ = Source Failure)
3. The other processes need to deliver a message. m if src is correct, and may deliver $\Delta$ if src crashes.


Name: UniformTerminatingReliableBroadcast, instance utrb, with sender s.

Request: < utrb, Broadcast | m >: Broadcasts a message m to all processes. Executed only by process src.
Indication: < utrb, Deliver | src, m >: Delivers a message m broadcast by process src or the symbol △.

Properties:

UTRB1. Validity: If a correct process src broadcasts a message m, then src eventually delivers m.
UTRB2. Termination: Every correct process eventually delivers exactly one message.
UTRB3. Integrity: If a correct process delivers some message m, then m was either previously broadcast by process src or it holds $m = \Delta$.
UTRB4. Uniform Agreement: If any process delivers a message m, then every correct process eventually delivers m.


### Impl. for UTRB

> 为什么会用到共识：假设正确节点断网了，该节点需要知道有没有节点 deliver 过 m，要保证所有节点一致。

> 这个算法是蚂蚁的项目

需要用共识和 $P$




![](https://notes.sjtu.edu.cn/uploads/upload_ae995e060b6a9f79d698ac449d4dd48a.png)






其中，src 如果半道 crash 了，会导致一部分节点 propose m，一部分 propose $\perp$。给共识模块 propose 不同的值，由共识达成一致返回


### UTRB = Perfect FD

作业 9.1

Prove that P is the weakest failure detector to implement utrb

Hint: Try to implement P assuming utrb (and reliable channels)
This would mean that P and utrb are equivalent in a system with reliable channels


## Group Membership

In some distributed applications, processes need to know which processes are participating in the computation and which are not（记录哪些机器参与运算的元数据）

和 FD 的区别：FD 的结果不是 **coordinated** 的, even if the failure detector is perfect。此外，实际生产环境的 GM. 不仅记录 crash，而会记录所有节点加入删除操作（新机器加入进来）

#### coordinated group membership

集群的每个节点记录集群中节点加入、退出的顺序需要是一致的。（识别到 crash / 可用新节点的顺序是一致的）


### Interface Spec for Group Membership

> view: 好节点是谁的信息

我们只关注如何一致检测 failure。我们将节点检测到 failure 称作 install views。GM. 需要

1. 准确地判断 failure 
2. The information on failures are coordinated: processes install the same sequence of views

Name: GroupMembership, instance gm.

Indication: < gm, View | V >: Installs a new view V = (id, M) with view identifier id (0, 1, 2, ...) and membership M. (M is the set of correct processes)

Properties:

GM1. Monotonicity: If a process p installs a view V = (id, M) and subsequently installs a view V' = (id', M'), then id < id' and M ⊋ M'. (M becomes smaller)
GM2. Uniform Agreement: If some process installs a view V = (id, M) and another process installs some view V' = (id, M'), then M = M'.
GM3. Completeness: If a process p crashes, then eventually every correct process installs a view (id, M) such that p ∉ M.
GM4. Accuracy: If some process installs a view (id, M) with $q \notin M$ for some process $q\in \Pi$, then q has crashed.







![](https://notes.sjtu.edu.cn/uploads/upload_a0d56da087c337e5caa42228635b8660.png)




> 维护本地 M 的副本，当本地 P 报告时更新本地，并发起共识保证装载顺序一致 

> 该实现不强依赖于完美错误检测器，用 $\diamond P$ 也能产生有意义的结果


## Non-block Atomic Commit(NBAC)

> 数据库的原子提交问题, an agreement problem


### Transactions

A **transaction** is an atomic program describing a sequence of accesses to shared and distributed information（对数据库的多次操作，组成一次原子操作）

- A transaction can be terminated either by committing or aborting


transaction example

```verilog=
begin.transaction
	Alice.withdraw(¥1000)
	Bob.deposit(¥1000)
end.transaction

begin.transaction
	EasternAirline.book(Shanghai, Beijing)
	AirChina.book(Beijing, Shanghai)
end.transaction
```

Either both instructions are executed (tx commits) or neither of them is (tx aborts)

- A transaction may abort due to Concurrency and Failures


#### ACID properties for TXs

- Atomicity: a transaction either performs entirely or none at all
- Consistency: a transaction transforms a consistent state into another consistent state（要基于数据库具体实现来定义 tx 的一致性）
- Isolation: a transaction appears to be executed in isolation
- Durability: the effects of a transaction that commits are permanent


### NBAC

NBAC 是一个 consensus 的变种，要对 tx 的 commit/abort 达成一致

- A variant of binary consensus（与原始 b.c. 不同，更偏向 0，有很多一票否决条件）
- Decision biased towards 0


> 中航信是实际负责航司信息管理的公司


### Interface Spec for NBAC

Name: NonBlockingAtomicCommit, instance nbac.

Request: < nbac, Propose | v >: Proposes value v = COMMIT or v = ABORT for
the commit.
Indication: < nbac, Decide | v >: Outputs the decided value for the commit.

Properties:

NBAC1. Termination: Every correct process eventually decides some value.
NBAC2. Abort-Validity: A process can only decide A BORT if some process proposes **ABORT** or a process **crashes**.
NBAC3. Commit-Validity: A process can only decide C OMMIT if no process proposes ABORT.
NBAC4. Integrity: No process decides twice.
NBAC5. Uniform Agreement: No two processes decide differently.

> 与正常共识的区别是 NBAC2-3

### Atomic Commit (AC)

也是在数据库中常用的方法，只是没有考虑 crash 的情况不能停。只满足 NBAC1,3,4

典型教科书方法：两阶段提交 2-phase commit(2PC)


### Impl. for Consensus-based NBAC









![](https://notes.sjtu.edu.cn/uploads/upload_83addd26c1863f1fbbf341b23f82a18b.png)





- 在 consensus 之上的应用层设计了多个 abort 判断。节点的业务代码判定成功失败后，Propose 一个结果
- commit/abort <-> 1/0

> consensus 模块的好处：开发者不用考虑各种 corner case 到底满足不满足，全都喂给 consensus 让他协商去罢

# Lesson 10 - Real World Applications

## Distributed Storage Systems

块设备、文件系统、对象存储

Google File Systems, Apache HDFS, Ceph, ...

- 首先找 Configuration Management 问存在哪台机器(Zookeeper, etcd)
- config 典型备份 5 份(f=2)；data N=3. 备份地点跨数据中心







![](https://notes.sjtu.edu.cn/uploads/upload_13523338663a919f82bb4c03a23c3a6b.png)







## Large-scale Database Systems

大规模数据库，全球数据库

Google Spanner, PingCA TiDB, WeChat PaxosStore, OpenGauss, OceanBase, ...

用 Paxos, Raft 作为 building block. 每个 tablet 都是高可用的，永不挂。






![](https://notes.sjtu.edu.cn/uploads/upload_7bfc33bf2de56f6a955d2b72f2c9b7e5.png)








## Blockchain

用 PoW 替代 Quorum

#### Throughput

- Bitcoin ~ 7txs/s
- Ethereum ~ 15txs/s
- …
- VISA ~ 10,000txs/s



#### Confirmation times:

Bitcoin/Ethereum ~ hour(s)
VISA ~ second(s)
