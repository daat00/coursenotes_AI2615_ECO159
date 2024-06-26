# Baby-escape
## 注册 PCI 设备
PCI设备通常包含一组寄存器，cpu通过总线访问寄存器，从而与设备交互。这意味着系统需要给设备分配总线地址。分配地址的方式有 mmio 和 pmio。在mmio中，I/O设备与RAM使用同一共享空间，每个寄存器对应一个内存地址。在pmio中，内存和I/O设备有各自的地址空间，此时cpu需要通过 in、out、outw,、outl 等特殊指令实现交互。
#### MMIO
PCI 设备监听总线地址（同一地址空间下）。cpu 用 ld 内存地址的命令访问 PCI 设备的寄存器（PCI 配置空间）。
#### PMIO
PCI 设备监听 I/O 端口地址。cpu 用 in/out 指令访问 PCI 设备的寄存器。

### EDU 设备
https://notes.sjtu.edu.cn/ldsyztF8QsumK22xmC-xKg

https://github.com/qemu/qemu/blob/v2.7.0/hw/misc/edu.c#L345
https://github.com/qemu/qemu/blob/v2.7.0/docs/specs/edu.txt

edu  设备本职工作是计算阶乘，同时实现了通过 dma 的 mem-io 读写，IRQ（中断请求）。

doc 中列举了参数的合规条件，以及每个 reg 的 addr 和详细功能。

寄存器可分为四组，按照用途，有
- 读取设备信息，读取设备是否正常工作
- 阶乘计算相关寄存器（input，status）。其中 status 标识了计算完成时是否需要发起 irq
- 中断相关寄存器, raise, ack, status
- dma 相关寄存器 src, dst, cnt, cmd


#### 设备的模块实现
总体上，通过注册异步回调函数的方式实现
- irq_handler
- 定义 mmio_read/mmiowrite 
- 注册设备 class 信息
- 注册设备 instance 信息

> qemu 在 c 语言基础上实现了类似 C++ 的内存模型 Qemu Object Model。QEMU 中，设备类是一个 ObjectClass 对象，设备实例是一个 Object 对象。
> 其中 Object 对象的第一个 field 是 ObjectClass。其中，ObjectClass 定义静态方法，Object定义动态方法。
> https://bbs.kanxue.com/thread-265501.htm#msg_header_h3_1


设备会在 qemu 创建时，单独创建一个线程用于计算阶乘（cond_wait 寄存器状态），signal 时计算结果，最后根据配置触发中断

mmio_read/write 函数是一个大 switch，每个 addr 对应不同的寄存器，对应寄存器的相应逻辑。这两个函数在注册内存的时候被填写信息。



edu.c 从内存的角度来看，其实就是定义了一个类（继承于 PCIDevice）（通过回调向 qemu core 注册了一个类）。代码把类的 class 函数，instance 函数填写，然后调用 type_init 注册。

```
EduState is-a PCIDevice is-a DeviceState is-a Object. There is no class struct for EduState because it doesn't need one (it would be empty); the class struct for PCIDevice is PCIDeviceClass; the class struct for DeviceState is DeviceClass; the class struct for Object is ObjectClass.
```


#### DMA 数据搬移的实现
实现了一个 timer，周期性地搬移数据，调用了 dma_rw





# QOM
qemu 通过定义一个继承 PCIDevice 类的对象来注册一个新的 PCI 设备。

由于 C 不是面向对象的语言，该对象通过 qemu 的内存模型 QOM 来完成定义。
在 QOM 中，约定 class_init 负责初始化静态成员变量，instance_init 负责初始化实例的成员变量，类似 C++/Java 中的 static。
驱动通过回调函数向 qemu 注册初始化函数和内存读写的 handler，通过触发回调函数来模拟设备的逻辑。

#	mmio_write 函数
每次对 PCI 内存的写入会触发 mmio_write 回调，函数的原型如下所示。opaque是 State 对象的指针，记录设备当前内部状态。addr 是本次访存的起始地址，val 是写入的值，size 是写入的字节长度。
```c
sjtuctf_mmio_write(sjtuctfState *opaque, hwaddr addr, 
uint64_t val, unsigned int size)
```

设备为每个寄存器分配了一个内存地址。在实现上，mmio_write 函数内部一般会 switch(addr)，根据访存的地址，分别执行不同的逻辑，以模拟不同寄存器的行为。



#	找到hva与gpa的对应关系
为了运行 system("cat flag")，我们还需要命令字符串的 hva 地址。有两种思路
1.	将 cmdline 放置在 guest 内存空间中，然后找到 gpa = 0 对应的 hva 起始地址，计算出 cmdline 的地址
2.	将 cmdline 写入 host 的 dma_buf 中，再找到 dma_buf 的起始 hva地址。
其中，第一种思路需要泄露gpa的起始地址，第二种思路需要找到hva的堆中，dma_buf的地址。

> 问过 coc 了，docker 题的堆地址是固定的，所以可以直接写入 dma_buf，然后找到 dma_buf 的地址。

# AddressSpace
AdressSpace 表示 Guest 视角不同类型的地址空间。

在 x86 下只有两种地址空间，分别是 `address_space_memory` 与 `address_space_io`。
在 qemu 程序中，他们分别对应了两个 AddressSpace 类型的全局指针变量 address_space_memory 和 address_space_io。
 
AdressSpace结构体的成员可以参考wiki。每个AddressSpace里面关联了一个root MemoryRegion。MemoryRegion是表示Guest物理内存（GPA）的结构体。MemoryResion会组成一个树形结构，逐渐瓜分整个地址空间。

MemoryRegion有根 (root)、实例 (instance)、别名 (alias) 三种类型。其中，根类型的MR用以表示与管理由多个 sub-MR 组成的内存区域，并不实际指向一块内存区域；实体类型的和alias用来表示实际内存。MR结构体中存在MemoryRegionOps类型的字段ops，保存了MR的成员函数。MR的读写函数在ops中。子MR以链表的形式保存在subregions字段中。

从hva 的视角来看，保存MR对应的hva信息的结构体是RAMBlock。在MR结构体中，ram_block字段指向了一个RAMBlock。其中的mr字段是一个MR 的指针，host字段 记录了gva 对应的hva。
 

> 问了 coc，qemu 里面函数指针比较多