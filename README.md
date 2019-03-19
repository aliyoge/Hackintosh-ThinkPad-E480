# Thinkpad E480 for macOS Mojave & High Sierra

Hackintosh your Thinkpad E480

## Features

* Support 10.13.x and 10.14.
* ACPI fixes use hotpatch; related files are located in `/CLOVER/ACPI/patched`.

#### CPU
* The model is `i5-8250U` , and XCPM power management is native supported. 

#### Battery
* The Power display is functioning normally.
* Use [RehabMan Battery Patch](https://github.com/RehabMan/Laptop-DSDT-Patch/blob/master/battery/battery_Lenovo-X230i.txt) to fix Dsdt.

#### Wi-Fi
* The wireless model is `Realtek 8821CE Wireless LAN 802.11ac PCI-E NIC`. Unfortunately, there's no way to enable it. 
* Using `BCM94352Z`

#### USB
* USB Port Patching uses [Intel FB-Patcher](https://www.tonymacx86.com/threads/release-intel-fb-patcher-v1-4-1.254559), related file is located in `/CLOVER/kexts/Other/USBPorts.kext`.

#### Ethernet
* The model name is `RTL8168/8111/8112 Gigabit Ethernet Controller`, functioning normally.

#### Graphics
* The model name is `Intel UHD Graphics 620`, faked to `Intel HD Graphics 620` by injecting ig-platform-id `00001659`.
* The discrete graphics' name is `Radeon (TM) RX 550 ( 2 GB )`, disabled by `SSDT-DDGPU.aml` becuase macOS doesn't support Optimus technology.

#### Audio
* The model of the sound card is `Conexant SmartAudio HD`, which is drived by `AppleALC`; injection information is located in `/CLOVER/config.plist`. 
* Headphones are not working right.

#### Keyboard
* functioning normally.

#### SSD
* functioning normally.

#### Touchpad
* functioning normally.

#### Bluetooth
* Install [BrcmFirmwareRepo.kext](https://bitbucket.org/RehabMan/os-x-brcmpatchram/downloads/) into `s/l/e`

## Tips

#### Disable Hibernation

Be aware that hibernation (suspend to disk or S4 sleep) is not supported on hackintosh.

You should disable it:

Code:

```bash
sudo pmset -a hibernatemode 0
sudo rm /var/vm/sleepimage
sudo mkdir /var/vm/sleepimage
```

Always check your hibernatemode after updates and disable it. System updates tend to re-enable it, although the trick above (making sleepimage a directory) tends to help.

And it may be a good idea to disable the other hibernation related options:

Code:

```bash
sudo pmset -a standby 0
sudo pmset -a autopoweroff 0
```

#### Hibernation config

```
$ pmset -g #显示当前电源状态下的设置
$ pmset -g custom #显示所有电源状态下的设置

standbydelay 10800 #standby启动的时间。单位秒
standby 1 #处于睡眠状态经过设定时间后，记忆体数据写入硬碟，关闭记忆体供电。1开启，0关闭
womp 1 #网路唤醒。1开启，0关闭
halfdim 1 #显示器亮度调低时间。单位分钟
hibernatefile /var/vm/sleepimage #睡眠档案位置
powernap 1 #PowerNAP是否开启。1开启，0关闭
gpuswitch 2 #GPU是否自动切换。1开启，0关闭，2不支持
networkoversleep 0 #睡眠时提供共享网路服务
disksleep 10 #机械硬碟停转时间。单位分钟，0关闭
sleep 0 #系统睡眠的时间。单位分钟，0关闭
autopoweroffdelay 28800 #autopoweroff启动的时间，单位为秒
hibernatemode 3 #睡眠方式设置
autopoweroff 1 #处于睡眠状态经过设定时间后，记忆体写入硬碟，关闭记忆体电源。1开启，0关闭
ttyskeepawake 1 #远程用户活动时防止睡眠。1开启，0关闭
displaysleep 10 #显示器关闭时间。单位分钟，0关闭
acwake 0 #电源状态改变时唤醒。1开启，0关闭
lidwake 1 #开盖唤醒。1开启，0关闭
```

#### Experimental option for Skylake/Kaby Lake (and later): HWP

In Skylake CPUs, Intel introduced a new power management technology: SpeedShift (aka. SST, aka. HWP).

With HWP enabled, the CPU handles pstate management by itself instead of requiring the OS to do it. The CPU itself will automatically shift to higher and lower pstates depending on CPU demand.

In order to use HWP, use an SMBIOS that is enabled for HWP... currently MacBook9,1, MacBookPro13,x (and now MacBookPro14,x, MacBookPro15,x). Also, since HWP tends to cause the xcpm_idle to be invoked, make sure the xcpm_idle patch (courtesy of PikeRAlpha) is enabled. It is default in all current plists provided by my Clover laptop guide. If you're using older than current plists, you may have to copy the patch into your config.plist/KernelAndKextPatches/KernelToPatch section.

You can also enable HWP for other SMBIOS by creating a patched resources injector for X86PlatformPlugin.kext (or by patching the kext itself). But that is a subject for another day.

Note: You still need SSDT.aml from ssdtPRgen.sh or SSDT-XCPM.aml as discussed earlier.

#### How to fully enable HWP

1、 config.plist/CPU/HWPEnable = Yes

2、 `plugin-type` must in ACPI table, use `SSDT-pr.aml` to fully enable AGPM(AppleGraphicPowerManagement) and X86PlatformPlugin.

3、 add `xcpm_idle` to `config.plist/KernalAndKextPatches/KernelToPatch`

```
<key>KernelToPatch</key>
<array>
        <dict>
                <key>Comment</key>
                <string>MSR 0xE2 _xcpm_idle instant reboot(c) Pike R. Alpha</string>
                <key>Disabled</key>
                <false/>
                <key>Find</key>
                <data>
                ILniAAAADzA=
                </data>
                <key>MatchOS</key>
                <string>10.12</string>
                <key>Replace</key>
                <data>
                ILniAAAAkJA=
                </data>
        </dict>
</array>
```

4、 config `X86PlatformPluginInjector` (optional)

#### The function of kexts

```
1, ACPIBatteryManager.kext 
电源管理，配合dsdt使用。
2, BrcmFirmwareData.kext和BrcmPatchRAM2.kext和AirportBrcmFixup.kext
Broadcom BCM94352Z 无线网卡和蓝牙驱动，/Library/Extensions也需有一份。
3, AppleALC.kext
声卡驱动。
4, AppleIGB.kext和RealtekRTL8111.kext
有线网卡驱动。
5, CPUFriend.kext和CPUFriendDataProvider.kext
动态修改X86PlatformPlugin，注入电源管理数据。
6, FakeSMC.kext和Fake_*.kext
FakeSMC.kext用来欺骗OSX系统要安装的PC是SMC硬件。
7, GenericUSBXHCI.kext
USB3.0驱动，可以删除，在10.11就没用了。
8, HibernationFixup.kext
修复睡眠唤醒问题，也可以不用。
9, Lilu.kext
核心依赖。
10, USBPorts.kext
USB驱动。
11, VoodooPS2Controller.kext
触摸板驱动。
12, WhateverGreen.kext
核心依赖
```


