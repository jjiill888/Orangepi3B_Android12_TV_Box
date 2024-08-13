# Orangepi 3B android12 

大家好，任意网络平台正在阅读此信息的每个人

这里是莎普蕾的开发者---黑灰的咸鱼

我正在为香橙派 3B 移植 android12 操作系统，目前已经成功引导并且启动 android12 镜像，这个工作起源于我的 3d 打印机项目，此前我家庭影院使用的机顶盒是基于 orangepi zero3 的 android tv 盒子（base android 12 tv），在过去的使用中出现了相当多的问题，比如有时 hdmi 切换信号源后无法继续显示内容，有时功放（yamaha v283）无法解码 hdmi 音频，我必须为它放置在有开关键的插座上（因为它主板上没有电源开关），有时休眠后睡死，有时系统输出分辨率在 4k 和 fhd 中来回切换，配置了 hdmi cec 但是无法跟其他设备联动，于是我打算把它丢给 3d 打印机作为上位机去使用。

在我的收藏里有一块香橙派 3b，我非常喜欢这块板子，我过去曾经将它作为一台 ubuntu pc 去使用，但是它的解码芯片的驱动问题一直存在问题（详情见 https://www.bilibili.com/read/mobile?id=28853443），firefox 浏览器视频总是卡顿，rk3566 22nm 的四核 a55 soc 很难支持我的开发工作，日用办公软件运行非常的卡，4g 内存无法运行太多的软件。

但是它的优点令我非常喜爱，它在提供树莓派 4b 级别的性能的同时，价格足够低廉（189rmb 或者是 39 美元），在一张银行卡大小（85 x 56mm）的前提下，拥有全尺寸 hdmi2.0，edp, spi flash， 4 个 usb a 接口，还有最重要的 pcie2.0 的 m.2 接口，我曾经想要将它作为一台 nas 去使用，利用 pcie 总线拓展出 sata 接口，利用板载 rtc 电池成为一台带 ups 的 nas。但是最终为将它放入自己的柜子里，不再去使用它，我已经有足够多的开发板了，不过这仍然是我相当推荐的 sbc（single board computer）而且它还有 m2 接口。

在一开始，我将它刷入 ubuntu 和 armbian 并且使用 kdeconnect 让我的手机作为遥控器，即便我使用兼容的解码驱动，但是性能依旧无法解码我需要播放的高码率视频，并且始终无法解决 linux hdmi 输出音频的问题，播放视频时光标移动卡顿，以及最重要的 kodi 无法被正确的驱动，opengl 驱动一直有问题，所以我在一个星期之后，为它安装了香橙派官方的 android 镜像。

问题似乎解决了？kodi 可以流畅播放视频，hdmi 可以正确的输出音频，不过 90% 的时候启动无法被 yamaha v283 识别，而连接电视时则可以，但是我的电视只有两个 hdmi 输入接口，一个支持 hdmi2.0/arc，一个支持 hdmi1.2， 所以我没法在观看 4k 视频的同时使用功放。在被识别的 50% 情况中音频无法被解码，4k 和 fhd 会来回的自动切换，kdeconnect 光标的左右键被错误的映射，一些不是沉浸式访问的应用下方会出现 adb shell 无法去除的导航条（这是 75% 我使用 app 的情况）。香橙派提供了 orangepi 3b android11 的源代码和编译文档，我想要去试图解决这些问题，但是我仅仅解决半个问题，我成功在源码中修改以取消了导航条，但是留下了对应像素的黑边，hdmi cec 的开发对我来说较为困难，特别是我还似乎需要拥有 yamaha v283 所使用的松下 hdmi 模块的技术文档。

我不得不做出为 orangepi 3b 移植 android12 的选择, 我清楚谷歌对于 android12 的更新包括 HAL 1.1（详见 https://source.android.google.cn/docs/devices/tv/hdmi-cec?hl=zh-cn），极大的优化了 hdmi 关联设备的兼容性，于是我获取 rk3566 android12 的源代码，修改了一部分代码以编译出可以烧录进 orangepi 3b 的 android 镜像，经过测试可以烧录进 emmc，android12 跑起来了。

我的 yamaha v283 功放可以接受它的视频输入，重启也可以保持上次启动的分辨率，不会因为分辨率自动切换而引起 ui 显示不全的问题，默认播放器可以解码视频，但是 kodi 解码时只有 3.5 耳机接口有声音。且完全没有音频通过 hdmi 输出，有线网卡和无线网卡没有被正确的驱动,只能使用 usb 外接网卡才能连接网络，除此之外运行顺利，这意味着接下去的几个月，我将得到一个好用的 android 机顶盒。

我很想知道有多少人在 orangepi 3b 和其他开发板上因为错误的文档，不完善的系统而沮丧不已，我希望能够提供一个开源的能被个人和社区推动的 plan B。

这是我第一次在 arm 平台做这样的工作，也是第一次接触 android 的源码，很多问题我不保证我能去解决，但是我希望任何人都可以提出建议，特别是关于 hdmi cec 模块，我能找到资料很少，我很希望能和熟悉这个模块的人一起开发，但是我身边并没有这样的人。

那么最后，二进制保佑你   Binary Bless You。

01000010 01101001 01101110 01100001 01110010 01111001 00100000 01000010 01101100

01100101 01110011 01110011 00100000 01011001 01101111 01110101

‍

编译环境

操作系统:wsl2 运行的 ubuntu20.04

内存：32g (不解答因为内存低于 16g 而引起的问题)

cpu：至少4核cpu（amd 3100x or 9100f ）
硬盘：至少 400g 容量的 ssd

‍

1.安装依赖
```console
 sudo apt-get update

 sudo apt-get install git-core gnupg flex bison gperf build-essential zip curl
 zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 lib32ncurses5-dev x11proto-
 core-dev libx11-dev lib32z-dev ccache libgl1-mesa-dev libxml2-utils xsltproc
 unzip python-pyelftools python3-pyelftools device-tree-compiler libfdt-dev
 libfdt1 libssl-dev liblz4-tool python-dev openssl
```
2.合并压缩包
```console
cat ROCKCHIP_ANDROID12.0_SDK_RELEASE_PART_* > ROCKCHIP_ANDROID12.0_SDK_RELEASE.tar
```

3.解压
```console
 tar -xvf ROCKCHIP_ANDROID12.0_SDK_RELEASE.tar -C
```
‍
4.编译
```console
 source build/envsetup.sh

 lunch rk3566_sgo-userdebug

 ./build.sh -AUCKu  -d rk3566-evb2-lp4x-v10
```

5.如何去释放 wsl2 的硬盘空间（解决 wsl2 内删除文件后磁盘空间未减少的问题）
```console
 wsl --shutdown //“磁盘已经连接” 重启电脑，执行这步

 diskpart

 select vdisk file="C:\users\…\ext4.vhdx"
 attach vdisk readonly // "所请求的操作需要以只读方式"  请执行这一步
 compact vdisk
 detach vdisk
 exit
```
‍

Hello everyone, anyone reading this message on any network platform.

This is Eric_king.

I am currently porting the Android 12 operating system to the Orange Pi 3B. I have successfully booted and launched the Android 12 image. This work originated from my 3D printer project. Previously, the set-top box used in my home theater was based on the Orange Pi Zero3 and ran an Android TV box (based on Android 12 TV). During past usage, I encountered quite a few issues, such as the HDMI signal source sometimes not displaying content after switching, the amplifier (Yamaha V283) being unable to decode HDMI audio at times, the necessity of placing it in a socket with a power button (since there is no power switch on the motherboard), the system sometimes not waking up from sleep, and the output resolution switching back and forth between 4K and FHD. I configured HDMI CEC, but it failed to interact with other devices. As a result, I decided to repurpose it for use as a host computer for my 3D printer.I am currently porting the Android 12 operating system to the Orange Pi 3B. I have successfully booted and launched the Android 12 image. This work originated from my 3D printer project. Previously, the set-top box used in my home theater was based on the Orange Pi Zero3 and ran an Android TV box (based on Android 12 TV). During past usage, I encountered quite a few issues, such as the HDMI signal source sometimes not displaying content after switching, the amplifier (Yamaha V283) being unable to decode HDMI audio at times, the necessity of placing it in a socket with a power button (since there is no power switch on the motherboard), the system sometimes not waking up from sleep, and the output resolution switching back and forth between 4K and FHD. I configured HDMI CEC, but it failed to interact with other devices. As a result, I decided to repurpose it for use as a host computer for my 3D printer.

In my collection, I have an Orange Pi 3B that I absolutely love. I’ve used it in the past as an Ubuntu PC, but I’ve always had issues with the driver for its decoding chip (for more details, https://www.bilibili.com/read/mobile?id=28853443). The Firefox browser would always stutter when playing videos, and the RK3566 22nm quad-core A55 SoC struggled to support my development work. Daily office software ran very sluggishly, and the 4GB of memory couldn’t handle running too many applications at once.

However, its advantages are what I truly appreciate. It offers performance comparable to the Raspberry Pi 4B at a significantly lower price (189 RMB or $39). Despite its compact size, equivalent to a credit card (85 x 56mm), it features a full-size HDMI 2.0, eDP, SPI flash, four USB-A interfaces, and most importantly, a PCIe 2.0 M.2 interface. I once considered using it as a NAS by expanding the PCIe bus to add a SATA interface and using the onboard RTC battery to create a NAS with UPS capabilities. But in the end, I decided to put it away in my cabinet and stop using it, as I already have enough development boards. Nevertheless, this is still a single board computer (SBC) that I highly recommend, especially because it has an M.2 interface.

At first, I flashed it with Ubuntu and Armbian and used KDE Connect to turn my phone into a remote control. Despite using compatible decoding drivers, the performance was still insufficient to decode the high-bitrate videos I needed to play, and I could never resolve the issue of Linux HDMI output audio. The cursor would lag when playing videos, and most importantly, Kodi could not be driven correctly. There were always issues with the OpenGL drivers, so after a week, I installed the official Orange Pi Android image on it.

In my collection, I have an Orange Pi 3B that I absolutely love. I’ve used it in the past as an Ubuntu PC, but I’ve always had issues with the driver for its decoding chip (for more details,https://www.bilibili.com/read/mobile?id=28853443). The Firefox browser would always stutter when playing videos, and the RK3566 22nm quad-core A55 SoC struggled to support my development work. Daily office software ran very sluggishly, and the 4GB of memory couldn’t handle running too many applications at once.

However, its advantages are what I truly appreciate. It offers performance comparable to the Raspberry Pi 4B at a significantly lower price (189 RMB or $39). Despite its compact size, equivalent to a credit card (85 x 56mm), it features a full-size HDMI 2.0, eDP, SPI flash, four USB-A interfaces, and most importantly, a PCIe 2.0 M.2 interface. I once considered using it as a NAS by expanding the PCIe bus to add a SATA interface and using the onboard RTC battery to create a NAS with UPS capabilities. But in the end, I decided to put it away in my cabinet and stop using it, as I already have enough development boards. Nevertheless, this is still a single board computer (SBC) that I highly recommend, especially because it has an M.2 interface.

At first, I flashed it with Ubuntu and Armbian and used KDE Connect to turn my phone into a remote control. Despite using compatible decoding drivers, the performance was still insufficient to decode the high-bitrate videos I needed to play, and I could never resolve the issue of Linux HDMI output audio. The cursor would lag when playing videos, and most importantly, Kodi could not be driven correctly. There were always issues with the OpenGL drivers, so after a week, I installed the official Orange Pi Android image on it.

The issues seem to have been resolved to some extent. Kodi can now play videos smoothly, and HDMI audio output is working correctly. However, 90% of the time, the system fails to be recognized by my Yamaha V283 amplifier when starting up, although it works fine when connected to a TV. Unfortunately, my TV only has two HDMI input ports, one supporting HDMI 2.0/ARC and the other HDMI 1.2, so I can’t watch 4K videos and use the amplifier at the same time. In the 50% of cases where it is recognized, the audio sometimes cannot be decoded, and the resolution switches back and forth between 4K and FHD. The left and right mouse buttons are incorrectly mapped in KDE Connect, and a navigation bar that cannot be removed with adb shell appears at the bottom of non-immersive apps (which accounts for 75% of the apps I use).

Orange Pi provides the source code and compilation documentation for the Orange Pi 3B running Android 11, and I’ve tried to address these issues. I managed to modify the source code to remove the navigation bar, but it left a black bar of corresponding pixels. Developing HDMI CEC is quite challenging for me, especially since I seem to need the technical documentation for the Panasonic HDMI module used by my Yamaha V283.

I decided to port Android 12 to the Orange Pi 3B. I’m aware that Google’s updates for Android 12 include HAL 1.1 (see  https://source.android.google.cn/docs/devices/tv/hdmi-cechttps://source.android.google.cn/docs/devices/tv/hdmi-cec), which greatly improves compatibility with HDMI-related devices. I obtained the source code for RK3566 Android 12, made some modifications, and compiled an Android image that can be flashed onto the Orange Pi 3B. After testing, it can be burned into eMMC, and Android 12 runs smoothly.

My Yamaha V283 amplifier can now accept video input from it, and it maintains the last startup resolution even after rebooting, solving the issue of the UI not displaying properly due to resolution switching. Although it can decode video smoothly, there is no audio output through HDMI. The wired and wireless network cards are not being driven correctly, so I can’t connect to the network. Apart from these issues, it runs smoothly, which means I’ll have a functional Android set-top box for the next few months.

I’m curious to know how many people have become frustrated with the Orange Pi 3B and other development boards due to incorrect documentation and incomplete systems. I hope to provide an open-source Plan B that can be driven by individuals and the community.

This is my first time working on an ARM platform and my first encounter with Android source code. I can’t guarantee that I’ll be able to solve all the issues, but I hope anyone can offer suggestions, especially regarding the HDMI CEC module. There’s very little information available, and I would love to collaborate with someone familiar with this module, but there’s no one like that around me.

Finally，Binary Bless You。

01000010 01101001 01101110 01100001 01110010 01111001 00100000 01000010 01101100

01100101 01110011 01110011 00100000 01011001 01101111 01110101
