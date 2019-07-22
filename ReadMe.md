# Lenovo 小新 Air 13IWL

(xlivans)

## 感谢 宪武 小兵 各位群友 大佬们 为此机型的辛苦付出

| 系统   | macOS Mojave 10.14.5                            | 18F132    |
| ------ | ----------------------------------------------- | --------- |
| CPU    | Intel(R) Core(TM) i5-8265U 1.60GHz Turbo 3.9Ghz | 4C8T      |
| 内存   | Samsung 4GB x 2  Dual Channel 1867MHz~2133MHz   | LPDDR3    |
| 硬盘   | HFS256GD9TNG-62A0A 250G Nvme SSD                | 1C5C:1527 |
| 显卡   | Intel UHD Graphics 620 Whiskey Lake-U GT2       | 8086:3EA0 |
| 声卡   | Realtek ALC236                                  | 8086:A170 |
| 网卡   | 更换Dell DW1820A BCM4350                        | 14E4:43A3 |
| 蓝牙   | 更换Dell DW1820A BCM2045A0                      | 0A5C:6412 |
| 键盘   | PS/2 标准键盘                                   | MSFT0001  |
| 触控板 | Intel I2C HID                                   | INT34BB   |
| 显示器 | AU Optronics B133HAN05.A 13.3 FHD 1920x1080     | 6AF:5A2D  |
| 摄像头 | Integrated Camera                               | 04F2:B61E |

## 

## CPU : Intel(R) Core(TM) i5-8265U 1.60GHz Turbo 3.9Ghz 4C8T  

### 驱动 : CPUFriend.kext , CPUFriendProvider.kext

### 补丁 : SSDT-XCPM.aml

1.终端执行脚本freqVectorsEdit.sh输入密码,选nano继续,记下高亮绿色的机型Mac-551B86E5744E2388.plist(根据自己实际情况选择一个合适的记下),查看即可不要继续执行,关闭终端;

2./System/Library/Extensions/IOPlatfromPluginFamily.kext/Contents/Pluglns/X86PlatformPlugin.kext/Contents/Resources拷贝Mac-551B86E5744E2388.plist到桌面，修改最低频率为800Mhz ;

3.终端执行脚本ResourceConverter.sh --kext Mac-551B86E5744E2388.plist ;

4.得到CPUFriendProvider.kext (默认输出至用户名主目录下~/) ;

5.所需脚本在 Tools/CPU/ ,变频正常, 双系统切换需要关机切换否则会有高频不降现象. 

![CPU](Pictures/CPU.png)

## 

## 内存 : Samsung 4GB x 2  Dual Channel 1867MHz~2133MHz  LPDDR3

### 驱动 : AptioMemoryFix-64.efi

内存默认频率为1867MHz , 通过修改隐藏BIOS可以超频到2133MHz ;

BIOS 9RCN22WW版本会卡内存,建议避开使用.

## 

## 硬盘 : HFS256GD9TNG-62A0A 250G Nvme SSD  1C5C:1527

### 驱动 : ApfsDriverLoader-64.efi , HFSPlus.efi

## 

## 显卡 : Intel UHD Graphics 620 Whiskey Lake-U GT2  8086:3EA0

### 驱动 : WhateverGreen.kext

### 补丁 : SSDT-PNLF.aml

采用Properties方法注入ig-platform-id,默认使用0x3e9b0000,若出现睡眠唤醒花屏尝试更换0x3EA50009 ;

终端使用 Tools/HDIDPI/hidpi-zh.sh(需要关闭SIP)一键开启HIDPI(注入EDID)可解决HDMI插拔黑屏的问题 ;

支持亮度调节,支持硬解H264,HEVC .

![UHD620](Pictures/UHD620.png)

![HDMI](Pictures/HDMI.png)

## 

## 声卡 : Realtek ALC236  8086:A170

### 驱动 : AppleALC.kext , FakePCIID.kext , FakePCIID_Intel_HDMI_Audio.kext , CodecCommander.kext

采用Properties方法注入,注入ID为99 ;

安装Tools/ALCPlugFix(需要关闭SIP)声卡守护进程(处理3.5mm接口切换);

内置音频输入输出正常,HDMI音频输出正常.

![ALC236](Pictures/ALC236.png)

## 

## 网卡 : 更换Dell DW1820A BCM4350  14E4:43A3

### 驱动 : AirportBrcmFixup.kext

添加AirportBrcmFixup.kext启动参数brcmfx-country=HK , 解决5G速率限制.

此网卡需要屏蔽5个针脚,如下图

![DW1820A](Pictures/DW1820A.jpg)

## 

## 蓝牙 : 更换Dell DW1820A BCM2045A0  0A5C:6412

### 驱动 : BrcmFirmwareData.kext , BrcmPatchRAM2.kext , BrcmBluetoothInjector.kext

蓝牙在本机目前不能完美,只能近距离使用(大概10cm),目前驱动方法为注入空壳驱动使用4096固件,使用虚拟机大法可以解决距离短的问题,双系统切换需要关机切换否则小概率出现蓝牙丢失大概率出现链接不上设备,睡眠唤醒正常,隔空、接力正常! (虚拟机大法请参考另一篇文档).

## 

## 键盘 : PS/2 标准键盘  MSFT0001

### 驱动 : VoodooPS2Controller.kext

### 补丁 : SSDT-Q11Q12.aml

找到驱动内的 VoodooPS2Controller.kext\Contents\PlugIns\VoodooPS2Mouse.kext , VoodooPS2Controller.kext\Contents\PlugIns\VoodooPS2Trackpad.kext 这两个扩展驱动直接删除,防止随机开机失败.

支持F11,F12亮度调整键,支持F6,PrtSc触控板开关键.

## 

## 触控板 : Intel I2C HID  INT34BB

### 驱动 : VoodooI2C.kext , VoodooI2CHID.kext

### 补丁 : Clover使用SSDT-XOSI.aml ; OC使用SSDT-OCOS.aml ; 共用补丁 SSDT-SBUS.aml

屏蔽苹果原装I2C驱动,重建缓存,工作正常,多手势正常

## 

## 摄像头 : Integrated Camera  04F2:B61E

免驱,注意定制USB时不要删除HS05

## 

## USB : 端口定制 8086:9DED

### 驱动 : XHCI-unsupported.kext , USBPorts.kext

### 补丁 : SSDT-EC.aml

使用Hackintool定制USB由于指纹识别不可用所以删除对应USB端口,最终保留端口为:HS01-USB2 , HS02-USB2 , HS03-TypeC+Sw , HS05-Internal-摄像头 , HS10-Internal-蓝牙 , SS01-USB3 , SS02-USB3 , SS03-TypeC+Sw其他端口都删除,导出USBPorts.kext即可.

![USB](Pictures/USB.png)

## 

## 杂项

1,添加补丁 SSDT-GPRW.aml 解决睡眠唤醒问题

2,添加驱动 NoTouchID.kext 解决输入密码卡顿问题

3.添加必备驱动 Lilu.kext , VirtualSMC.kext

4,添加电池驱动 SMCBatteryManager.kext ,

5,添加硬件各项传感器驱动 SMCProcessor.kext , SMCSuperIO.kext

## 

## 更名

### Clover更名

![Clover](Pictures/Clover.png)

### OC更名

![OC](Pictures/OC.png)

## 

## 相关软件,驱动Releases下载地址

| 文件名           | 下载地址                                                     |
| ---------------- | :----------------------------------------------------------- |
| Lilu             | https://github.com/acidanthera/Lilu/releases                 |
| VirtualSMC       | https://github.com/acidanthera/VirtualSMC/releases           |
| WhateverGreen    | https://github.com/acidanthera/WhateverGreen/releases        |
| AppleALC         | https://github.com/acidanthera/AppleALC/releases             |
| AirportBrcmFixup | https://github.com/acidanthera/AirportBrcmFixup/releases     |
| CPUFriend        | https://github.com/acidanthera/CPUFriend/releases            |
| VoodooPS2        | https://github.com/acidanthera/VoodooPS2/releases            |
| VoodooI2C        | https://github.com/alexandred/VoodooI2C/releases             |
| NoTouchID        | https://github.com/al3xtjames/NoTouchID/releases             |
| FakePCIID        | https://bitbucket.org/RehabMan/os-x-fake-pci-id/downloads    |
| CodecCommander   | https://bitbucket.org/RehabMan/os-x-eapd-codec-commander/downloads |
| BrcmPatchRAM     | https://github.com/headkaze/OS-X-BrcmPatchRAM/releases       |
| Hackintool       | http://headsoft.com.au/download/mac/Hackintool.zip           |
| PlistEdit Pro    | https://www.fatcatsoftware.com/plisteditpro/PlistEditPro.zip |

PlistEdit Pro 激活

Name : TNT team 

Serial Number :  PLEDPRO486-SF00-82GR-N8E3-50L2-0A0Q

## 