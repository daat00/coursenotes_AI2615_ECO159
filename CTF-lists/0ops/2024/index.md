# Memo1


# ptrace
当前进程被标记为 PT_PTRACED 后，在调用 exec 系列函数时，load_elf_binary 函数会给自己发送 SIGTRAP 信号

通用的 do_signal 函数在处理 PT_PTRACED 进程时，会停止 current，给父进程发送 SIGCHLD 信号，然后调用 schedule


- TRACEME 会给父进程发送SIGTRAP信号，被父进程 wait 捕获到
- GETREGS 获取寄存器信息, PEAKDATA 读取内存数据；并可以 set
- SINGLESTEP 配合 rip, PEAKDATA(rip) 可以在特定位置修改寄存器行为
- SYSCALL 分别在 syscall 的 enter 和 exit 时触发，配合 orig_rax, rax 可以判断在哪个 syscall