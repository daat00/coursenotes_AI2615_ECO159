# 目录结构
`local.properties`：本地 SDK 路径

`gradle.properties`：全局 gradle 配置

`app/`: src 目录

`settings.gradle`: 所有引入的模块，自动添加

### app/
`libs/`: 依赖的 jar 包, 自动添加到项目（通过在 build.gradle 中添加 dependency fileTree 配置实现）

`res/`: 资源文件，代码中可以通过 `R.string.name` 访问, 在 xml 中可以通过 `@string/name` 访问

## AndroidManifest.xml
安卓清单，注册所有四大组件。

```xml
<activity android:name=".MainActivity">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" /> <!-- 入口 -->
        <category android:name="android.intent.category.LAUNCHER" /> <!-- 入口 -->
    </intent-filter>
```

## build.gradle
有两个 `build.gradle` 文件，一个是项目的，一个是模块的。项目的 `build.gradle` 可以声明插件，模块的使用插件，配置混淆、appId

# Activity




---
# Misc
## ADB
在 pc 上运行 adb client 和 adb server；手机启动时会从 init fork 出来 adb daemon
- adb server 在 5037
- 模拟器同时占用两个端口；一个奇数端口连接 adb，一个偶数端口转发内部网络请求
- adb server 在检测到设备变动时会尝试连接 adbd。成功的设备是 ONLINE 的，失败的设备是 OFFLINE 的

## Sparse Image
### Fastboot
*ref: https://2net.co.uk/tutorial/android-sparse-image-format*

`fastboot`: A protocol implemented by `bootloaders` for loading and flashing images to internal memory

[background](http://www.slideshare.net/chrissimmonds/android-bootslides20)

A typical bootloader flash implementation has two stages
1. download image to device(typically buffered in RAM)
2. command `flash` asks device to program data into internal memory.

So image must fit into RAM, which leads to `sparse image`
- Image is split into chunks of multiple 4096-byte blocks. Any chunk marked DONT_CARE is not sent
- Image defined in `sparse_format.h` looks alike
```c
+-----------------------+
|     sparse_header     |
+-----------------------+
|     chunk_header      |
+-----------------------+
|        Raw Data       |
+-----------------------+
struct sparse_header {
    __le32 magic; // 0xed26ff3a
    ...
    __le16 file_hdr_sz; // 28
    __le16 chunk_hdr_sz; // 12
    __le32 blk_sz; // 4096
    __le32 total_blks; // total blocks in the non-sparse output image
    __le32 total_chunks; // total chunks
    ...
};
```
The saving ratio is about 2%, from 500M to 10M



### Android System Images
boot, recovery, system, userdata, cache

A typical build for an Android device produces 5 image files in `out/target/product/<product>`:
- `boot.img` - kernel + ramdisk used for normal boot
- `recovery.img` - kernel + ramdisk used to boot into recovery mode
- system.img - File system image for `/system`
- userdata.img - File system image for `/data`
- cache.img - File system image for `/cache`

`boot.img` and `recovery.img` are created by tool `mktoolimg` (the code is in `system/core/mkbootimg`)
- They contains 
  - compressed kernel
  - kernel command line
  - (optional for embedding, must for And) ramdisk in normal Linux compressed cpio format
- format defined in `bootimg.h`, details in [below](#boot-and-recovery-image-format)
### Typical Flash Memory Layout
```
+-----------------------+
|      bootloader       |
+-----------------------+
| misc(used in OTA upd.)|
+-----------------------+
| boot kernel + ramdisk |
+-----------------------+
|recovery kernel+ramdisk|
+-----------------------+
|    /system - .ro      |
+-----------------------+
|       /data - rw      |
+-----------------------+
|      /cache - rw      |
+-----------------------+
```
bootloader is responsible for
- early hardware initialization
- load and boot kernel
- initial ram file system
- system maintenance, including loading and flashing new kernel and system images.

Example: [U-Boot](http://www.denx.de/wiki/U-Boot)
- It's possible to boot Android using a normal bootloader such as U-Boot
- However, most devices include Android-specific features:
  - support normal and recovery boot modes
  - ability to load kernel + ramdisk blobs
  - Fastboot protocol 
- Example: [Little Kernel](git://codeaurora.org/kernel/lk.git)
  - Supports many Qualcomm-based devices
  - rudiementary support for BeagleBoard and PC-x86

### Boot and Recovery Image Format
on page 11
```
+-----------------------+
|         Header        |
+-----------------------+
| Kernel Image (zImage) |
+-----------------------+
|ramdisk(compressed cpio)|
+-----------------------+
```
Header format:
```c
struct boot_img_hdr {
    unsigned char magic[BOOT_MAGIC_SIZE]; // "ANDROID!"
    unsigned kernel_size;
    unsigned kernel_addr;
    unsigned ramdisk_size;
    unsigned ramdisk_addr;
    unsigned second_size;
    unsigned second_addr;
    unsigned tags_addr;
    unsigned page_size; // typical 2048
    unsigned unused[2];
    unsigned char name[BOOT_NAME_SIZE]; // "product name"
    unsigned char cmdline[BOOT_ARGS_SIZE]; // kernel command line
    unsigned id[8]; // SHA-1 hash of kernel + ramdisk + second  
    unsigned char extra_cmdline[BOOT_EXTRA_ARGS_SIZE]; // 1024
}; 
```

### Boot Sequence
Power on -> Bootloader
- Boot normal
  
  Load normal kernel + ramdisk -> run /init, read init.rc -> mount fs, start services
- Boot recovery
    
    Load recovery kernel + ramdisk

#### Reverse Engineering a Boot Image
`boot-extract`

#### Extracting & creating ramdisk
A ramdisk is just a compressed cpio archive. It's also possible to create a compressed cpio archive.

#### Create a new boot image
```
mkbootimg --kernel zImage  --ramdisk ramdisk.cpio.gz \
--base 0x10000000 --pagesize 2048 -o recovery.img
```
where zImage is also a file, and `--base` is used to calculate the kernel and ramdisk load address
```
kernel_addr = base + 0x00008000
ramdisk_addr = base + 0x01000000
```
### Fastboot
    system/core/fastboot/
`Fastboot` is an ascii-text USB protocol and a command language for various maintenanve and development tasks. It communicates `bootloader` (or `fastboot` mode) over USB. 

*(Also possible over TCP/IP)*

The *fast* is about making the development process faster

#### Booting into Bootloader
Machines can boot into bootloader by 
- holding down a key combination
- `adb reboot-bootloader`

Once booted into bootloader, use the `fastboot` command to communicate with the device. 

Basic commands: (Skip)

Flash commands:
```
erase <partition>
flash <partition> [<filename>] // Erase and program partition with <partition>.img or <filename>
flashall // erase and program all 3 system partitions of current product then reboot
```
partition if one of `boot`, `recovery`, `system`, `userdata`, `cache`. Current product is `$ANDROID_PRODUCT_OUT`

(The location and size of partitions is hard-coded in the bootloader)


#### Unlocking the Bootloader
Most devices have a locked bootloader. 
```
fastboot getvar secure // true
```

## Flash Memory Devices
- Raw NAND flash: access directly by Linux MTD.(Memory Technology Device) drivers
  - partitions are named `/dev/block/mtdblock0` and are mapped to `/proc/mtd`
- Managed flash: contains an on-chip controller
  - MMC, SD, Micro SD cards
  - eMMC (embedded MMC): package card as a chip
  - UFS (Universal Flash Storage)

### File System for Raw NAND Flash
Flash translation layer (FTL) is a software layer that maps logical block addresses (LBA) to physical blocks on the flash device.

FTL is implemented in the filesystem

NAND system requires a special filesystem, such as `yaffs2` or `jffs2`. *Note: jffs2 is incompatible with Android Runtime, since no writable mmaped files*

### File System for Managed Flash
FTL is implemented in the flash controller.

Controller chip splits flash memory into 512 Bytes sectors. 

Accessed via Linux *mmcblock* driver, partition device nodes named `/dev/block/mmcblk0p1`

#### eMMC
eMMC devices look like hard drives, use same filesystem types, e.g. ext4.(Alternative, F2FS)

#### SD Cards and other removable media
For compatibility with other operating system, they are formatted with FAT32.

Use Linux *vfat* driver


https://xdaforums.com/t/info-android-device-partitions-and-filesystems.3586565/