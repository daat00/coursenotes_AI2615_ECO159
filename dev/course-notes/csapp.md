# Chapter 3
### AT&T v.s. Intel syntax
### Inline Assembly 内联汇编
- GCC feature

> 例如：amd64 在每次执行算数运算或逻辑运算时，如果结果的低 8 位有偶数个 1，就会设置 parity flag `PF = 1`

插入汇编代码有两种方法
1. 放到独立的汇编代码文件中，交给汇编器
2. `asm`

### 访问寄存器的低位
`rax, eax, ax, al` 是同一个寄存器。当访问 64 bit 寄存器的低位时
- 生成 1 byte / 2 byte 的指令会保留高位
- 生成 4 byte 的指令会清除高位

### 条件码
- `CF` 进位标志
- `ZF` 零标志
- `SF` 符号标志
- `OF` 溢出标志
- `PF` 奇偶标志

### 条件数据传送
`cmov` 指令系列，通常用于代替 `if-else` 语句。代替的方式为，同时计算出两个分支的结果，然后 `cmov` 在一个周期根据条件码 mov 结果。

### switch 跳转表
跳转表 `jt` 适用于 case 的值分布密集的情况

`jt` 是一个数组，数组的索引是 case 值，元素分支代码块的起始地址。

```c
// && 表示 代码 的地址，是 GCC 扩展的语法
void *jt[] = { &&loc_A, &&loc_def, &&loc_B, ... };
```

```asm
  .section .rodata
  .align 8
.LC0:
  .quad .L2
  .quad .L3
  .quad .L4  
```

## Procedure Call 过程
function, method, subroutine, routine, subprogram, procedure

### Stack 
常见的变量被分配到栈上的情况
- 变量被 `&` 取地址
- 数组或结构体/联合

### 对齐
动机：cpu 一次读 8 bytes，如果变量不对齐，需要两次读取，影响性能

原则：任何 k 字节的基本对象的地址必须是 k 的倍数

> 特殊：强制对齐的情况
>
> SSE 指令集对 16 bytes 数据块进行操作，要求 16 bytes 对齐。不满足的情况下，会产生 `bus error`。
>
> 因此，为了保证可移植性
> - 所有内存分配函数 (malloc, calloc, realloc, memalign) 都会返回 16 bytes 对齐的地址
> - 大多数栈帧的边界都是 16 bytes 对齐的

## Buffer Overflow
### 栈随机化 ASLR
实现方法
- linux 2005: 在每次 syscall 时，分配一个 0 ~ n 字节的随机大小的空间 [ref](https://blog.csdn.net/tugouxp/article/details/122766845)
- nvidia: 直接换 sp，类似 trap

### 栈破坏检测 CANARY
在 buffer 和栈状态之间存储一个特殊的值，称为 canary。当 buffer 被破坏时，canary 也会被破坏，从而触发异常。

现代 GCC 会尝试自动插入 canary

### 页执行权限保护 NX


## 变长栈帧
### FP
编译器无法在编译时确定栈帧的大小，因此需要在运行时动态调整栈帧的大小。这种情况下，需要使用 FP 来访问局部变量。

# Chapter 4
现代 cpu 有 15 级或更多流水线。5级也只是个例子罢了

# Chapter 6
## 存储单元种类
- SRAM: 双稳态，想象倒置钟摆
- DRAM: 电容，对干扰敏感，在光照下会丢失数据（照相机感光期间也是DRAM单元阵列）
- EPROM, EEPROM
- Flash(SSD)

地址被组织成二位阵列$w^{m\times n}$. $w= 8$.每次寻址读取时，内存控制器先后两次发送行地址（RAS）和列地址（CAS）。有行缓冲区

## 总线
总线的结构变化很快，不同厂商的体系结构不同以体现产品差异化，csapp不涉及具体的总线结构。

总体上，连接关系是
```
CPU.总线接口 <-- 系统总线 --> I/O Bridge <-- 内存总线 --> 主存
```

`总线事务`可粗略地分为读事务和写事务，他会标明事务的类型，目标设备等

