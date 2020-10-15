# 为Rpi配置CUPS

在这里我选择了openwrt系统，虽然在树莓派上它有一些文件需要自己编译，但它针对各类路由器有很好的支持，为日后的移植提供了便利。

## openwrt安装

https://downloads.openwrt.org/snapshots/targets/bcm27xx/bcm2711/

目前树莓派4B的安装包还处于snapshot状态，官网更新十分频繁，从链接中可以找到ext4和squashfs两种文件系统的版本，二者都可以。squashfs占用小，但在扩展存储时会遇到一些问题，目前还未解决。

![image-20201015154301835](C:\Users\Alfonso\AppData\Roaming\Typora\typora-user-images\image-20201015154301835.png)



然后可利用 Balena etcher 或 Win32DiskImager 将下载好的安装包烧写至sd卡 。这里也提供了我配置好CUPS的系统文件，只是在建立https连接时有些许问题。（配置页面位于192.168.1.253，密码：）

另外值得一提的是网上也有经过修改、添加功能后的openwrt系统，安装它们可以省去后面的一些配置步骤，但少数情况下预安装的包会与新安装的包产生冲突。

## 需要注意

由于snapshot版本不包含luci web管理界面，首次启动系统后需要建立ssh连接或利用板上micro hdmi进入命令行调试。

一些路由器的管理页面为192.168.1.1，与openwrt默认的地址有冲突，可以修改路由器的页面地址，或者在ssh连接时就将openwrt默认地址修改完成。

## 配置openwrt

由于需要安装各种包，需要能让树莓派上网。有关网络设置可以参考官方文档

https://openwrt.org/docs/guide-user/network/ipv4/start

在/etc/config/network中设置wan、dns、gateway等参数。



之后可通过opkg命令，opkg update，opkg install luci即可拥有网络管理页面

ttyd包可以在网络界面提供命令行终端，更易于之后的各种修改。

## 编译CUPS

这一篇教程写得非常详细

https://github.com/TheMMcOfficial/cups-for-openwrt

在打开make的设置界面后，需要首先确认好硬件架构（cortex-a72），并找到cups，设置为M（module）

![image-20201015164108452](C:\Users\Alfonso\AppData\Roaming\Typora\typora-user-images\image-20201015164108452.png)

编译好的文件在/source/bin/packages/aarch64_cortex-a72可以找到

## 安装CUPS

将ipk文件用scp命令拷贝至树莓派中，按照顺序先用opkg安装lib打头的文件，最后opkg install cups即可。

之后即可在树莓派设置的ip：631端口访问cups管理页面，接入打印机的usb线后按提示进行配置。

在这个网页可以找到大量型号打印机的驱动文件

https://github.com/syb999/openwrt-musl-cups/tree/master/cups_ppd