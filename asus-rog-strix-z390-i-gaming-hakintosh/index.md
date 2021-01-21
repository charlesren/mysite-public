# Asus ROG STRIX Z390 I GAMING Hakintosh

#### 配置如下:
散热   海盗船h60  378.9    
电源  海盗船sf600  698.9    
机箱   追风者217e  700      
主板   华硕rog z390i    1869    
显示器  dell2419   1498.5   
硬盘      三星970evo500g   879   
硅脂   猫头鹰nt-h1  18.9   
cpu  9700k散片   2459    
无线网卡  dell  dw1560（bcm94352z） 185S   
键盘鼠标自备    
内存 同事闲置   
#### 注意事项：
- 系统安装：第一阶段通过u盘安装到硬盘。安装过程中会重启，此时需按热键选择启动设备（asus 为F8） ，选择通过u盘启动，在引导界面选择硬盘名称，完成第二阶段安装。第二阶段安装完成后使用multibeast\ kextbeast\ clover configurator \clever等工具安装clever及驱动，完成post Installation.
- clover configurator已能自动升级clover ,升级kexts
- kextBeast使用方法:把kext文件放到桌面，然后运行kentbeast程序即可.
- **升级EFI时：backup your existing, working EFI folder.** Copy/paste your Serial, Board Serial, UUID, and Memory settings to the new config.plist.
- drivers64uefi 放一些启动时的驱动，比如apfs  
- **删除kext:**delete the extra  kext from /Library/Extensions, then do a rebuild of your kextcache using this command in terminal:sudo kextcache -i /
sudo kextstat
- kext utility 解决权限问题，重建缓存

#### 工具：
- unibeast  (tonymacx86)
- multibeast (tonymacx86)
- kextbeast (tonymacx86)
- [kent utility](http://cvad-mac.narod.ru/)
- Clover Configurator (tonymacx86)
- [geekbench](http://browser.geekbench.com/）
- [hackintool](headsoft.com.au/download/mac/Hackintool.zip)  *原intel FB Patcher*

#### 文件：
- clover （tonymacx86）   
- kexts (tonymacx86 ...)    
- SSDT-UIAC.aml   （tonymacx86）    
- [FakeSMC](https://github.com/RehabMan/OS-X-FakeSMC-kozlek)  后续可能被virtualsmc取代    
- [Lilu](https://github.com/acidanthera/Lilu)    
- [WhateverGreen](https://github.com/acidanthera/WhateverGreen)    
- [AppleALC](https://github.com/acidanthera/AppleALC)   后续可能集成到whatevergreen   
- [usbinjectall](https://github.com/RehabMan/OS-X-USB-Inject-All)
- DW1560:
    [BrcmPatchRam](https://github.com/RehabMan/BrcmPatchRAM.git)   
    BrcmFirmwareRepo.kext    
    AirportBrcmFixup.kext    

#### 参考文档
主要参考如下两篇文章

[tonymacx86](https://blog.daliansky.net/macOS-Mojave-10.14.2-18C54-official-version-with-Clover-4792-original-image.html#more)

[果黑小兵的部落阁](https://www.tonymacx86.com/threads/asus-rog-strix-z390-i-gaming-motherboard-specs.259848/)
#### Kent安装及使用
https://www.tonymacx86.com/threads/guide-installing-3rd-party-kexts-el-capitan-sierra-high-sierra-mojave.268964/

#### BIOS设置参考

https://www.tonymacx86.com/threads/asus-rog-strix-z390-i-gaming-motherboard-specs.259848/page-24

https://www.tonymacx86.com/threads/pastrychefs-asus-rog-strix-z370-g-gaming-wi-fi-ac-build-w-i9-9900k-amd-vega-56.239969/

- Advanced/CPU Configuration/Intel Virtualization Technology - Enabled
- Advanced/System Agent (SA) Configuration/VT-d - Disabled
- Advanced/System Agent (SA) Configuration/Graphics Configuration/Primary Display - PCIE (This will not be available if you are only using UHD 630.)
- Advanced/System Agent (SA) Configuration/Graphics Configuration/iGPU Multi-Monitor - Enabled (This will not be available if you are only using UHD 630.)
- Advanced/System Agent (SA) Configuration/Graphics Configuration/RC6(Render Standby) - Enabled
- Advanced/System Agent (SA) Configuration/Graphics Configuration/DVMT Pre-Allocated - 192M (64M should also work)
- Advanced/USB Configuration/Legacy USB Support - Enabled
- Advanced/USB Configuration/USB Keyboard and Mouse Simulator - Disabled
- Boot/CSM (Compatibility Support Module)/ Launch CSM - Enabled (Updated January 22, 2018: I originally used Disabled because it would allow Clover to boot in to the monitor's native resolution. Since then, I have found that Enabling CSM gives better compatibility with devices such as AQC107 and multi monitor support albeit at the loss of native resolution for the Clover boot menu. Bottom line, try both and use the one that works best with your hardware.)
- Boot/Secure Boot/ OS Type - Other OS
- (Optional)Ai Tweaker/Ai Overclock Tuner - XMP


#### usb:
使用ssdt文件（SSDT-UIAC.aml）  

put it in EFI/Clover/ACPI/patched, which will limit USB ports within 15 ports.

Notice: Due to 15 ports limits, one of the Front USB Port can only read USB3.0.

![](/ASUS-ROG-STRIX-Z390-i-GAMING-Hakintosh.jpg)

**参考**

https://www.tonymacx86.com/threads/asus-rog-strix-z390-i-gaming-motherboard-specs.259848/page-22

https://www.tonymacx86.com/threads/guide-creating-a-custom-ssdt-for-usbinjectall-kext.211311/

https://www.tonymacx86.com/threads/xhc-usb-kext-creation-guideline.242999/
#### 升级clover
try to only have the efi partition of the target drive mointed

#### 挂载EFI分区步骤：
diskutil list

sudo diskutil mount ...

或者通过clover configuator 挂载

#### 安装步骤：
参考Tonymacx86 相应版本guide

#### Maintenance & Future Updates
1. Always check the forum to see if new versions of macOS break anything.

2. Check for updates to:
- Everything in /EFI/CLOVER/kexts/Other/
- Clover
- apfs.efi which is located in /EFI/CLOVER/drivers64UEFI/ (No longer needed. Superseded by ApfsDriverLoader.efi.)
Of particular interest to this build are:
- Clover EFI bootloader
- FakeSMC.kext
- USBInjectAll.kext
- XHCI-200-series-injector.kext (Use the XHCI-200-series-injector.kext because it matches our a2af Device ID.)
- Lilu.kext
- AppleALC.kext
- IntelGraphicsFixup.kext (Now integrated in to WhateverGreen 1.2.0)
- NvidiaGraphicsFixup.kext (Now integrated in to WhateverGreen 1.2.0)
- WhateverGreen.kext
- Shiki.kext (Now integrated in to WhateverGreen 1.2.0)

*Update: The latest version of Clover Configurator provides an easy means of keeping your kexts updated. Select "Kext Installer" and then "Other".*

3.Of course, after updating macOS, Nvidia web drivers will also need updating.

4. On rare occasions, we need to update the SMBIOS section because of updated firmwares on real Macs. To do this:
    - Open your config.plist with Clover Configurator.
    - Copy your working Serial Number, SmUUID, and Board Serial Number.
    - Click the little up/down button to the right of the image of an iMac.
    - Select iMac18,3.
    - Fill in the Serial Number, SmUUID, and Board Serial Number with what you copied earlier.
    - Save.

5. Be careful with motherboard BIOS updates!! They can sometimes break things.

6. If you use FileVault, make sure that AsAmiShim.efi or AptioInputFix-64.efi is still in /EFI/CLOVER/drivers64UEFI whenever you update Clover.

##### iGpu问题：

https://www.tonymacx86.com/threads/guide-intel-framebuffer-patching-using-whatevergreen.256490/

https://blog.daliansky.net/macOS-Mojave-10.14.2-18C54-official-version-with-Clover-4792-original-image.html#more

https://www.tonymacx86.com/threads/solved-uhd-630-is-not-accelerated-in-mojave.267193/

https://www.tonymacx86.com/threads/guide-general-framebuffer-patching-guide-hdmi-black-screen-problem.269149/

Augustinus said:
>I’m using i5 9700k
>according https://www.tonymacx86.com/threads/solved-uhd-630-is-not-accelerated-in-mojave.267193/
>seems the device-id is not native support (i3 is 0x3e92, i5 is 0x3e98).
>Use 3E9B0007 if you are using IGPU only.

Use 3E910003 if you have a dGPU.
 

##### audio问题：
https://www.tonymacx86.com/threads/asus-rog-strix-z390-i-gaming-motherboard-specs.259848/page-11

https://blog.daliansky.net/Common-problems-and-solutions-in-macOS-Mojave-10.14-installation.html

https://www.tonymacx86.com/threads/guide-intel-framebuffer-patching-using-whatevergreen.256490/

Proper audio support is available.    
Typo: Mojave/Desktop working audio, AppleALC.kext_v1.3.8 or newer    
Correct: Mojave/Desktop working audio, AppleALC.kext_v1.2.8 or newer    
Post #123, edited


https://www.tonymacx86.com/threads/asus-rog-strix-z390-i-gaming-motherboard-specs.259848/page-19

https://www.tonymacx86.com/threads/adding-audio-codec-applealc.264396/#post-1849408


##### shutdown/restart问题：
https://www.tonymacx86.com/threads/asus-rog-strix-z390-i-gaming-motherboard-specs.259848/page-9

https://blog.daliansky.net/Common-problems-and-solutions-in-macOS-Mojave-10.14-installation.html

AppleALC.kext & layout-id 1  

no  gpu  iMac18,1

If you use IGPU, use iMac18,1 system definition.

For 4K, it's best to use DisplayPort connection.


