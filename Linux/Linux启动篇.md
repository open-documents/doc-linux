## Linux启动篇

### systemd进程

Centos7.x的systemd启动流程如下：

loacl-fs.target + swap.target

sysinit.target：主要检测硬件，加载所需要的内核模块等模块。

basic.target

multi-user.target

图形界面相关服务，如dgm.service等其他服务的加载



#### sysinit.target

将该环境的服务分为如下几类：

- 特殊文件系统设备的挂载：
- 加载额外的内核模块：通过<font color=00ffff>`/etc/modules-load.d/*.conf`</font>文件的设置，让内核额外加载管理员所需要的内核模块。
- 加载额外的内核参数设置：包括<font color=00ffff> `/etc/sysctl.conf`</font>以及`/etc/sysctl.d/*.conf`内部设置
- 设置终端字体。

该环境加载完成后，系统可独写。



#### basic.target

主要启动的服务大概如下：

- 由<font color=00ffff> `/etc/sysconfig/modules/*.modules` </font> 加载管理员指定的模块。
- 加载alsa音效驱动程序：这个alsa是个音效相关的驱动程序
- 加载firewalld防火墙
- 加载CPU微指令功能 <font color=FF0000>？</font>

该环境加载完成后，系统变为操作系统。



#### multi-user.target

可以到<font color=00ffff> `/etc/systemd/system/multi-user.target.want/ `</font>内查看默认要被启动的服务。



### 启动过程用的主要配置文件

主要用到的配置文件如下：

/etc/sysconfig/

2). `/etc/modules-load.d/*.conf`

/etc/modprobe.d/*.conf



### 内核与内核模块

位置

内核：/boot/vmlinuz或/boot/vmlinuz-version

内核解压缩所需RAM Disk：/boot/initramfs

内核模块：/lib/modules/version/kernel 或 /lib/modules/$(uname -r)/kernel

内核源代码：/usr/src/linux 或 /usr/src/kernels

如果该内核被顺利加载到系统当中，那么就会有几个信息记录下来：

内核版本：/proc/version

系统内核功能：/proc/kernel/