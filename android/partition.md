[INFO] ANDROID DEVICE PARTITIONS and FILESYSTEMS
===
https://xdaforums.com/t/info-android-device-partitions-and-filesystems.3586565/

# Partition Table
手机的内部存储是 eMMC 或 UFS，可以像 HD 一样分区。分区表由分区名、分区类型、分区大小、分区起始位置、分区结束位置、分区文件系统等组成。

不是 SDCard，是 NAND

> NAND 和传统 Block Device 本质上有一些区别，为了让 NAND 用起来像 Block Device，在 NAND 中植入 microcontroller，称为 eMMC

#### MBR(Master Boot Record) 分区表
- 传统 BIOS 系统使用的分区表
- 4 个主分区，每个分区最多 2TB
- 逻辑分区，通过扩展分区实现

由第一个扇区 sector 包含分区表，最多 4 个主分区 + 扩展分区

#### GPT(GUID Partition Table) 分区表
UEFI 系统使用的分区表，不依赖 boot sector，可以有 128 个分区。CRC32 校验

#### 相关 linux 命令
- `fdisk -l` 查看分区表
- `parted` 交互式分区工具
- `gdisk` GPT 分区工具
- `gparted` 图形化分区工具



# Partitions
安卓有超过 50 个分区，每个分区有不同的功能，有些分区是只读的，有些是可写的。
#### SoC 分区
SoC = CPU + GPU + DSP + ISP + Modem...

`aboot`, `sbl`, `rpm`, `tz`, `keymaster`, `lksecapp`

#### img 分区
可以 flash 的分区
- boot
- system
- data

system 和 data 占约 90% 的空间

## Modem/Radio
也被叫做 baseband 

在旧设备里，modem 控制蓝牙，UPS，wifi， 是 cellular radio chip 的 mini os。在新设备中由 OS 控制
- cellular radio chip 有自己的 processor
- firmware file 保存在 SYSTEM，VENDOR partition
- modem firmware 是特别的，因为他有自己的 baseband processor，保存在自己的分区
- modem 与 Android 不是特供的关系，与 hardware 绑定，kernel 可以访问 hardware
- BP 跑的是 RTOS，负责 call, SMS, Internet (CPU - Application Processor - AP)

## RIL (Radio Interface Layer)
不是单独的 partition，是 Android 的一部分，负责与 modem 通信
- RIL daemon 提供移动网络数据 (old phone to smartphone)
- Android telephony framework <--> modem AT commands

# TrustZone & TEE
- TrustZone 是 ARM 的技术，用于安全处理
- TPM (Trusted Platform Module) 是 Intel 的技术

在出厂时保存私钥，使得 client 对 server 可信

# RPM (Resource Power Manager)
启动 BootRom 中 primary / primitive bootloader 的执行
- 控制 power to radio

# bootloader
- 独立 ELF（文件系统未 load）（在 RAM）。由 SoC VENDER 提供，闭源
- 多步执行启动 kernel
- SBL secondary bootloader。由 SoC 加载，向 RAM 引入 ABOOT（application bootloader，引导 kernel，recovery，fastboot）


```
product  -- extend --> system // OEM customization
ODM      -- extend --> vendor
```
