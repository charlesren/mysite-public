# Asus ROG STRIX Z390 I GAMING Hakintosh

主要参考tonymacx86和果黑小兵的部落阁https://blog.daliansky.net 如下两篇文章
https://blog.daliansky.net/macOS-Mojave-10.14.2-18C54-official-version-with-Clover-4792-original-image.html#more

https://www.tonymacx86.com/threads/asus-rog-strix-z390-i-gaming-motherboard-specs.259848/

bios设置参考
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

AppleALC.kext & layout-id 1  

no  gpu  iMac18,1

If you use IGPU, use iMac18,1 system definition.

For 4K, it's best to use DisplayPort connection.



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

##### usb:
https://www.tonymacx86.com/threads/asus-rog-strix-z390-i-gaming-motherboard-specs.259848/page-22

https://www.tonymacx86.com/threads/xhc-usb-kext-creation-guideline.242999/

