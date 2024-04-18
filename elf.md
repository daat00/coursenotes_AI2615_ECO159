# 可执行文件 
- Windows: PE
- MacOS: Mach-O
- Linux: ELF

# 目标文件
- Object File 可重定位文件 `ET_REL .o`
  - 需要与其他对象链接，必须包含 section
  
- Executable File 可执行文件 `ET_EXEC`
  - 可以直接执行，必须包含 segment

- Shared Object File 共享目标文件 `.so`
  - Link eDitor: 链接 -> 目标文件
  - Dynamic Linker: `.o, .so, EF` -> process
  - 同时包含 section 和 segment
- Core Dump File 核心转储文件
  - 进程异常终止时，操作系统将进程的内存转储到文件中

## Linking View and Execution View
Linking View
```
+----------------+
| ELF Header     |
+----------------+
| PHT (Optional) |
+----------------+
| Section(s)     |
+----------------+
| Section HT     |
+----------------+
```
- Section(节): `.text, .data, .rodata, .bss, .symtab, .strtab, .shstrtab`


Execution View
```
+----------------+
| ELF Header     |
+----------------+
| Program HT     |
+----------------+
| Segment(s)     |
+----------------+
| SHT (Optional) |
+----------------+
```
- Segment(段): `.text, .data, .rodata, .bss, .symtab, .strtab, .shstrtab`

## ELF Header
`b'\x7fELF'`, endianness, 32/64, ABI, entry point, flags, machine, version
```
PHT/SHT: offset, size, entsize, num, shstrndx
```
- `e_type`: `ET_NONE, ET_REL, ET_EXEC, ET_DYN, ET_CORE, ET_LOPROC, ET_HIPROC`

## Program Header Table
Describe the segments used at run time
- `p_type`: `PT_NULL, PT_LOAD, PT_DYNAMIC, PT_INTERP, PT_NOTE, PT_SHLIB, PT_PHDR, PT_LOPROC, PT_HIPROC`
- `p_flags`: xrw
## Section Header Table


## Segment
- 告诉内核如何映射内存
- Segment 包含加载地址，文件偏移，权限，对齐
- 运行时必须信息
- segment 更大：相同权限的 section 合并到同一个 segment，例如 .text, .rodata, .data, .bss
- 但由于 4k 映射，可能会有特例

## Section
- 告诉 linker ELF 的每个部分是什么（同类合并，text、rodata、rolocate）
- Section 包含 Section 类型、文件位置、大小
- linker 依赖 Section 将不同对象文件的代码、数据合并，并修复互相引用



## Linker
`ld` GNU Linker
<br>
`ldd` List Dynamic Dependencies

#### Relocation Table
```
<Offset> <Info> <Type> <Symbol Value> <Symbol Name + Addend>
0000000a 50000002 R_386_PC32 00000000 printf - 4
```

在链接之前，`.o` 文件之间的调用处于 zero out 状态，连接时，
- 对于静态链接，合并 section，将 .text, .data 等段分别合并到一起，修复引用。
  - 直接用 ld 链接可能会缺少 libc，gcc 链接时会加入 libc 等 dependency。
- 对于动态链接，编译后指令设置为类似 `call 
- 。运行时执行 `libc.so.6 ./a.out`，动态链接器加载库后进入程序
  - 查询 GOT 表
  - 或者查询 PLT 表，然后查询 GOT 表
  
1. 静态链接也可以编译成位置无关代码
2. 系统会枚举所有的动态链接库，比对 INTERP


# GOT, PLT
https://www.yuque.com/hxfqg9/bin/ug9gx5#xfDPU
https://www.bilibili.com/video/BV1a7411p7zK

以防丢失，记录一下

linux 下的动态链接是通过 PLT, GOT 实现。我们考察 test.c, test.o, test 来说明

首先通过 `objdump -d test.o` 查看反汇编，考察 glibc 的 `printf`。在 .o 文件 call printf 的时候地址先用 `fc ff ff ff` （有符号数 -4） 代替，交由链接器处理。

`.text` 段的权限是 rx，在这里填写的地址运行时无法修改。所以，链接器需要生成一个跳转代码段（即 plt 的一项），作为中介跳转到动态库(.data)

```x86asm
.text
call printf_stub; // 调用printf的call指令

printf_stub:
    jmp  [<address stores printf>]; // 获取printf的动态库地址，并跳转

.data
printf: ; // real printf
```

上述代码是不带延迟绑定的 plt 表项，其中，[\<address stores printf>] 访问了 GOT 表，GOT 表的每一项保存一个地址（不是 jmp 指令）。plt, got 的地址在链接时确定。



#### 延迟绑定

在调用动态函数时进行地址解析和重定位工作，伪代码形如

```x86asm
// 初始化时 printf@got 处填写 lookup_printf 的地址
void printf@plt()
{
address_good:
    jmp *printf@got 
lookup_printf:
  ;// 调用重定位函数查找 printf 地址，并写到 printf@got
	jmp address_good; // 返回执行 address_good
}
```

```mermaid
graph LR
A[printf@plt] --> |*printf@got| B[printf@plt]
B --> C[common@plt]
C --> D[_dl_runtime_resolve]
```

GOT 查询并不需要跳转 pc

#### 实际 plt

查看 elf 文件中 .plt 节，.plt 节为每个函数生成了形如下面的代码
```x86asm
08048360 <setbuf@plt-0x10>: # fake symbol using the nearest one
 8048360:     push   0x804a004
 8048366:     jmp    *0x804a008
 804836c:     add    %al,(%eax)
	...

08048370 <setbuf@plt>:
 8048370:     jmp    *0x804a00c
 8048376:     push   $0x0
 804837b:     jmp    8048360 <setbuf@plt-0x10>
 ```
其中，每个 plt 表项会先试图跳转到 got 表项
- got 表项初始值为 plt 表项的下一条指令，即 jmp 回来继续执行
- 继续执行后将 push 函数标识，进入公共表项跳转到 `_dl_runtime_resolve`
- 其中，`0x0804a008` 保存 `_dl_runtime_resolve` 的地址，初始为 0，运行后重写为实际函数地址


#### 一些细节

`_dl_runtime_resolve` 通过访问 elf 的重定位符号表 `.rel.plt` 来决定应该把函数地址填回哪个 got.entry

使用 `readelf -r` 查看重定位信息，形如

```shell
Relocation section '.rel.plt' at offset 0x2b0 contains 1 entries:
 Offset     Info    Type            Sym.Value  Sym. Name
0804836c  00000107 R_386_JUMP_SLOT  00000000   printf
```

在 i386 架构下，除了每个函数占用一个 GOT 表项外，GOT 表前三项是 reserved，分别保存：
- got [0]: 本 ELF 动态段 (.dynamic 段) 的装载地址 
- got [1]：本 ELF 的 link_map 数据结构描述符地址 
- got [2]：_dl_runtime_resolve 函数的地址
  
动态链接器在加载完 ELF 之后，都会将这３地址写到 GOT 表的前３项


