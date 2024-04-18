ROM Dev
===

## ROM 内模块
- kernel
- bootloader(unlocked bootloader)
- recovery(seperate bootable partition)
- radio
- framework(libs, APIs)
- apps
- core(system services, libs, essential system process)
- Android Runtime(Dalvik)

## building ROM
需要三个参数，写入 localManifest.xml
1. Device tree
2. Kernel
3. Vendor

build 的命令根据 ROM 有所不同

