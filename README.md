# Thinkpad E480 for macOS Mojave & High Sierra

Hackintosh your Thinkpad E480

## Features

* Support 10.13.x and 10.14.
* ACPI fixes use hotpatch; related files are located in `/CLOVER/ACPI/patched`.

### CPU
* The model is `i5-8250U` , and XCPM power management is native supported. 

### Wi-Fi
* The wireless model is `Realtek 8821CE Wireless LAN 802.11ac PCI-E NIC`. Unfortunately, there's no way to enable it. 
* Using `BCM94352Z`

### USB
* USB Port Patching uses [Intel FB-Patcher](https://www.tonymacx86.com/threads/release-intel-fb-patcher-v1-4-1.254559), related file is located in `/CLOVER/kexts/Other/USBPorts.kext`.

### Ethernet
* The model name is `RTL8168/8111/8112 Gigabit Ethernet Controller`, functioning normally.

### Graphics
* The model name is `Intel UHD Graphics 620`, faked to `Intel HD Graphics 620` by injecting ig-platform-id `00001659`.
* The discrete graphics' name is `Radeon (TM) RX 550 ( 2 GB )`, disabled by `SSDT-DDGPU.aml` becuase macOS doesn't support Optimus technology.

### Audio
* The model of the sound card is `Conexant SmartAudio HD`, which is drived by `AppleALC`; injection information is located in `/CLOVER/config.plist`. 
* Headphones are not working right.
### Keyboard
* functioning normally.

### SSD
* functioning normally.

### Touchpad
* functioning normally.

### Bluetooth
* Install [BrcmFirmwareRepo.kext](https://bitbucket.org/RehabMan/os-x-brcmpatchram/downloads/) into `s/l/e`