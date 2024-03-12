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