RAFT
====
https://www.bilibili.com/video/BV1nu4y167WM

https://raft.github.io/

1. 只有集群超过半数节点 commit/apply 时，操作日志才会 commit / 操作才会 apply 到 state machine。
2. 保证了只要是提交的操作一定是集群最终的操作。
3. 维护了一个单调不减的 commitIndex，保证了操作的顺序性，通过 index 来推算自己是不是慢了
4. 通过 term 判断 leader，仅 leader 可以接受客户端的请求

#### Multi-Paxos
RAFT 属于 multi-paxos。把 propose 阶段合并到上一个 accept 阶段。


# Follower, Candidate, Leader
每个节点有三种状态：Follower, Candidate, Leader。Candidate 是过渡状态


# 选举
leader 需要
- 过半数的节点投票
- 自己的 term 必须大于等于其他节点的 term
- log index 必须大于等于其他节点的 log index

其中，后几条的实现方法是其他节点拒绝投票，返回自己的 term 和 log index。

# 心跳 & 日志复制
