# 固件 firmware, ROM
https://www.digitalcitizen.life/simple-questions-what-firmware-what-does-it-do/

对于不那么智能的设备，firmware 指代其内部 embedded 的逻辑程序： 
> a small piece of software that makes hardware work as its manufacturer intended it to.

- 如：主板 BIOS，路灯控制固件，数控笔通信固件，网卡选择协议、频率连接 wifi 的逻辑
- 注意，此时 firmware 也相当于他们的 os

按道理，应该只有被用于进行硬件和 os 间通信的小部分软件被称作固件，然而，**特别是对于智能设备**
- firmware = firmware + os = **all software**
- Custom ROM = 自定义的 os image = firmware + OS img，而非硬件 firmware 存储的 ROM

> stock ROM / stock firmware 即出厂自带的 ROM

固件的格式