# Asus Z390i 9700k Hackintosh With OpenCore

### 硬件配置
|配件|型号|价格|
|---|---|---|
|散热|   海盗船h60 | 378.9|
|电源  |海盗船sf600 | 698.9|
|机箱   |追风者217e  |700| 
|主板  | 华硕rog z390i |   1869|
|显示器 | dell2419   |1498.5|
|硬盘   |   三星970evo500g |  879|
|硅脂 |  猫头鹰nt-h1 | 18.9|
|cpu  |9700k Coffee Lake Refresh 630核显 FCLGA1151  |  2459|
|无线网卡|  dell  dw1560（bcm94352z）| 185|
|内存 |USCORSAIR 32GB(16x2) DDR4 3200 复仇者LPX系列||
|键盘| 高斯 GS87D||
|鼠标| Apple Trackpad 2||
### 参考文档
[OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/)

[Getting Started With ACPI](https://dortania.github.io/Getting-Started-With-ACPI/)

[Wireless Buyers Guide](https://dortania.github.io/Wireless-Buyers-Guide/)

[一键生成黑苹果 OpenCore EFI 文件OC.Gen-X](https://heipg.cn/tutorial/one-key-opencore-efi.html)
### 工具准备
#### 硬件
**方案一: OpenCore boot Manager 与macOS 安装U盘分离**
1. U盘1 : 16GB或以上容量,用于创建macOS安装U盘
2. U盘2 : 4GB或以上容量，用于安装OpenCore boot Manager
> OpenCore boot Manager与macOS 系统分离优点
> - 可以单独升级OpenCore boot Manager
> - 用Apple推荐的方法创建macOS启动盘(没有任何第三方bootloaders),此U盘还可以用于安装升级白苹果.
> - 安装完成后，OpenCore boot Manager U盘也会成为永久的启动U盘.(也可以把EFI文件拷贝到硬盘的EFI分区，从硬盘启动)
>
> *当选择从OpenCore boot Manager U盘启动后, Clover/OC should display one more option (Install macOS from the other USB)*

**方案二：OpenCore boot Manager与macOS 安装U盘合二为一**

需一个U盘 : 16GB或以上容量,用于创建OpenCore boot manager及macOS安装U盘
> One flash drive is always sufficient, you can format it using Disk Utility choosing Mac OS Extended (journal) with a GUID boot partition. It adds a UEFI partition you can mount, and then copy and paste EFI folder there. 
#### 软件
[OC Gen X](https://github.com/Pavo-IM/OC-Gen-X) :用于生成基础EFI文件

[ProperTree](https://github.com/corpnewt/ProperTree) :用于修改config.plist文件及加载SSDT
> ProperTree修改config.plist方法参考：[OpenCore Install Guide > USB Creation > config.plist Setup](https://dortania.github.io/OpenCore-Install-Guide/config.plist/) 章节
### 安装步骤
#### BIOS设置

**设置内容**

参考 [OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/)文档的Configs > Coffee Lake > Intel BIOS settings章节

Disable

- Fast Boot
- Secure Boot
- Serial/COM Port
- Parallel Port
- VT-d (can be enabled if you set DisableIoMapper to YES)
- CSM
- Thunderbolt(For initial install, as Thunderbolt can cause issues if not setup correctly)
- Intel SGX
- Intel Platform Trust
- CFG Lock (MSR 0xE2 write protection)
  > (This must be off, if you can't find the option then enable AppleXcpmCfgLock under Kernel -> Quirks. Your hack will not boot with CFG-Lock enabled)

Enable

- VT-x
- Above 4G decoding
  > Advanced Items > System Agent (SA) Configuration > Above 4G Decoding > Enable
- Hyper-Threading
- Execute Disable Bit
- EHCI/XHCI Hand-off
  > Advanced Items > USB Configuration > XHCI Hand Off > Enabled
- OS type: Windows 8.1/10 UEFI Mode
- DVMT Pre-Allocated(iGPU Memory): 64MB
  > Advanced Items > System Agent (SA) Graphics Configuration > DVMT Pre-Allocated > 64
- SATA Mode: AHCI

**设置方法**

```
1. Plug your monitor into your video cards DisplayPort to avoid graphics issues. You can use HDMI if it's all you have
2. Start your machine and use 1 of these methods to get onto BIOS during boot:

       a) Rapidly tap the delete key
       b) Rapidly tap the F2 key (some keyboards may need to have the Function key held down)
       c) Press and hold the Function key while rapidly tapping the Backspace key??

3. Check the BIOS version and update it if it is not the latest one
4. If you have previously set your BIOS, it would be best to do a CMOS reset by shorting the 2 pins on the front left corner of your motherboard. Please se instructions for complete details
5. In the main screen middle left, set X.M.P. to Enabled
6. Advanced Items > CPU Configuration > Intel (VMX) Virtualization Technology > Enable - Absolutely required for Parallels
7. Advanced Items > System Agent (SA) Graphics Configuration > Primary Display > PEG for DGPU, CPU for IGPU
8. Advanced Items > System Agent (SA) Graphics Configuration > IGPU Multi-Monitor > Enabled for DGPU, Disabled for IGPU
9. Save and reboot to activate the RC6 and DVMT settings
10. Advanced Items > System Agent (SA) Graphics Configuration > RC6(Render Standby) > Off - This settings disables a power saving feature that could potentially crash the system
13. Advanced Items > USB Configuration > Legacy USB Support > Disabled
15. Boot Menu > Boot Option 1 > UEFI USB installer drive (or whatever you named it, UEFI will be automatically prepended)
16. Exit > Save Changes

    There are few things you can tweak to eliminate ugly boot graphics and make BIOS menus easier to navigate:

17. Boot > Boot Configuration > Boot Logo Display > Disabled
18. Boot > Boot Configuration > Post Report > 1 Sec
19. Boot > Setup Mode > Advanced

    I love this next feature because it allows you to save your BIOS settings so you can play around without having to wipe them and start over.

20. Tools > Asus User Profile > Profile Name > OSX (or whatever you like)
21. Tools > Asus User Profile > Save to Profile > 1 (hit enter to save)
```

#### 创建macOS安装U盘
参考Apple官方文档: [How to create a bootable installer for macOS](https://support.apple.com/en-us/HT201372)

#### EFI准备
> 最终的EFI已上传到[Github](https://github.com/charlesren/Hackintosh-AsusZ390i-OpenCore)

**SSDT准备**
- 通用SSDT

  *参考[Getting Started With ACPI](https://dortania.github.io/Getting-Started-With-ACPI/)*

  直接从[OpenCorePkg](https://github.com/acidanthera/OpenCorePkg)项目下载
- 定制USB

  使用网上Z390-i定制好的[SSDT-UIAC.aml](https://www.tonymacx86.com/threads/the-everything-works-asus-z390-i-gaming-i7-8700k-sapphire-nitro-radeon-rx-vega-64-build.272572/)
**使用 OC Gen X 生成基础EFI文件**
> 需要的Firmware Drivers &  Kexts & SSDTS参考[OpenCore Install Guide > USB Creation > Gathering files](https://dortania.github.io/OpenCore-Install-Guide/ktext.html) 

配置如下：

System Type : **Coffee Lake**

Kext > Essential : **Lilu & VirtualSMC**

Kext > VirtualSMC Plugins : **SMCProcessor & SMCSuperIO**

Kext > Graphics : **WhateverGreen**

Kext > Audio : **AppleALC**

Kext > Ethernet : **InteMausi**

Kext > USB : **USBInjectAll**

Kext > WiFI and Bluetooth : **AirportBrcmFixup & BrcmPatchRAM3 & BrcmBluetoothInjector & BrcmFiremwareData**

Kext > Extra's : **Null**

Firmware Drivers > UEFI : **OpenRuntime.efi & HfsPlus.efi**
      
SMBIOS > System Model : **iMac19.1**

**添加SSDT并修改config.plist**
1. 把SSDT文件拷贝到**EFI/OC/ACPI**目录下
2. 运行ProperTree,打开EFI/OC/config.plist文件.
3. config.plist文件打开后, 按Cmd/Ctrl + Shift + R 快捷键,然后选中 EFI/OC 文件夹,用来**Clean Snapshot**
   > - This will remove all the entries from the config.plist and then adds all your SSDTs, Kexts and Firmware drivers to the config
   >
   > - Cmd/Ctrl + R is another option that will add all your files as well but will leave entries disabled if they were set like that before, useful for when you're troubleshooting but for us not needed right now
4. 完成后，会发现SSDTs, Kexts and firmware drivers 在 config.plist里出现了.
5. 保存设置并关闭ProperTree软件.

**DW1560修正**
> **问题:**
>
> 安装过程中出现**IOKit Daemon (kernelmanagerd) stall[0], (240s): 'PXSX'**字样，似乎中断240s,大概会重复三次，然后才能安装完成。完成后，系统连接不上wifi.

搜索发现有网友也出现[IOKit Daemon (kernelmanagerd) stall[0], (240s): 'PXSX'](https://www.tonymacx86.com/threads/solved-iokit-daemon-kernelmanagerd-stall-0-240s-pxsx.303324/)问题。问题初步定位在DW1560上。

根据以上线索，结合DW1560相关kext，在[AirportBrcmFixup](https://github.com/acidanthera/AirportBrcmFixup#please-pay-attention)页面，发现如下信息： 
> In 11 (Big Sur) class AirPortBrcm4360 has been completely removed. Using of injector kext with such class name and matched vendor-id:device-id blocks loading of original airport kext. 
>
> To address this issue and keep compatibility with older systems injectors for AirPortBrcm4360 and AirPortBrcmNIC were removed from main Info.plist file. Instead, the two new kext injectors are deployed in PlugIns folder: AirPortBrcm4360_Injector.kext and AirPortBrcmNIC_Injector.kext. 
>
> **You have to block (or remove) AirPortBrcm4360_Injector.kext in BigSur.** 
>
>  - In OpenCore you can specify MaxKernel 19.9.9 for AirPortBrcm4360_Injector.kext. 

**解决方案:**

Specify MaxKernel 19.9.9 for AirPortBrcm4360_Injector.kext in config.plist file. 
#### OpenCore启动盘制作
> OpenCore启动盘创建方法参考：[OpenCore Install Guide > USB Creation > Creating the USB](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/) 章节

**本次使用OenCore boot Manager 与macOS 安装U盘分离方案，所以不需要执行手册中下载macOS，载入macOS等部分**

以在macOS系统下操作为例:

- 格式化U盘2.
  > use macOS Extended (HFS+) with a GUID partition map. This will create two partitions: the main MyVolume and a second called EFI which is used as a boot partition where your firmware will check for boot files.
- 挂载EFI
- 把上面最终的EFI文件夹拷贝到EFI分区


#### 安装macOS
1. 插入U盘1,U盘2
2. 启动电脑，快速按F8键，选择从OpenCore U盘启动
3. 正常安装macOS

#### 硬盘启动配置
**本章节为可选配置，正常情况下，总是从U盘2启动;当期望从硬盘启动时，执行如下步骤**

1. 查看硬盘的EFI分区名称
```
diskutil list
```
2. 挂载硬盘的EFI分区
> 以EFI分区为disk0s1为例 
```
sudo diskutil mount disk0s1 
```
3. 把最终EFI文件夹拷贝到硬盘EFI分区














