# Thinkpad E480 for macOS Catalina

Hackintosh your Thinkpad E480

让你的Thinkpad E480装上黑苹果

## 电脑配置

| 规格     | 详细信息                              |
| -------- | ------------------------------------|
| 电脑型号 | 联想ThinkPad 翼480（0UCD）             |
| 处理器   | 英特尔 酷睿 i5-8250U 处理器             |
| 内存     | 16GB 金士顿 DDR4 2400MHz            |
| 硬盘     | 东芝(TOSHIBA) 240GB                  |
| 集成显卡 | 英特尔 UHD 图形620                     |
| 声卡     | Conexant CX20753/4 (节点:15)          |
| 网卡     | BCM94352Z                            |

## 目前情况

* 目前支持最新版本10.15.3。
* 与时俱进，采用OC引导。
* Cpu驱动加载正常，变频正常。
* 电量显示正常。
* 睡眠唤醒正常。
* 更换BCM94352Z网卡，蓝牙和无线正常(自带Realtek 8821CE无解)。
* 有线网卡正常。
* 集成显卡驱动正常，4k正常，屏蔽独立显卡。
* 声卡注入id14，正常工作。
* USB端口正常。

## 一些说明

#### 关于安装

安装系统请勿使用本EFI，请先到[远景论坛](http://bbs.pcbeta.com/forum-561-1.html)、[黑果小兵](https://blog.daliansky.net/)等站点查看教程，采用通用配置安装成功之后再参考我的`EFI`根据你自己的配置替换。

#### 关于DSDT

~~最好不要直接使用我的`DSDT`文件，请导出你自己的`DSDT`，然后打上一下补丁:~~

~~[电量补丁](https://github.com/RehabMan/Laptop-DSDT-Patch/blob/master/battery/battery_Lenovo-X230i.txt)~~

~~[关机变重启](https://github.com/RehabMan/Laptop-DSDT-Patch/blob/master/system/system_Shutdown_restart.txt)~~

~~[睡眠秒唤醒](https://github.com/RehabMan/Laptop-DSDT-Patch/blob/master/usb/usb_prw_0x6d_xhc.txt)~~

不再使用`DSDT`文件，全部改用`SSDT` 热补丁和 `ACPI Patch` 这样`EFI`基本上可以通用了。感谢 [SukkaW](https://github.com/SukkaW)



#### 已知问题

* 在睡眠唤醒之后可能会存在插拔电源后电量不更新问题，临时解决办法是再手动将系统睡眠一下，唤醒后会恢复正常，此问题正在想办法解决，有知道怎么解决这个问题的朋友麻烦告知一下。

#### 各种驱动作用

```
1, AirportBrcmFixup.kext
修复Airport
2, AppleALC.kext
声卡驱动。
3, BrcmFirmwareData.kext和BrcmPatchRAM3.kext和BrcmBluetoothInjector.kext
Broadcom BCM94352Z 蓝牙驱动。
4, HibernationFixup.kext
修复睡眠唤醒问题，也可以不用。
5, Lilu.kext
核心依赖。
6, NoTouchID.kext
避免登录页面卡顿。
7, RealtekRTL8111.kext
有线网卡驱动。
8, SMCBatteryManager.kext和SMC*.kext
电源管理驱动和传感器驱动。
9, USBPorts.kext
USB驱动。
10, VirtualSMC.kext
用来欺骗OSX系统要安装的PC是SMC硬件。
11, VoodooPS2Controller.kext和VoodooInput.kext
触摸板驱动。
12, WhateverGreen.kext
核心依赖，显卡驱动。
```

## 鸣谢

感谢 [SukkaW](https://github.com/SukkaW) 指出AppleALC和VoodooPS2的完善建议[issues11](https://github.com/aliyoge/Hackintosh-ThinkPad-E480/issues/11)。

