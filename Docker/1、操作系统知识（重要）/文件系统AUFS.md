
参考文档：https://blog.csdn.net/weixin_41402069/article/details/125866279

AUFS（advanced multi-layered unification filesystem）在主机上使用多层目录存储，每一个目录在 AUFS 中都叫作分支，而在 Docker 中则称之为层（layer），但最终呈现给用户的则是一个普通单层的文件系统，我们把多层以单一层的方式呈现出来的过程叫作联合挂载。

默认整合AUFS的Linux发行版内核有：
- Ubuntu
- Debian

默认不支持AUFS的Linux发行版有：
- CentOS

# AUFS特点

AUFS有下面的特点：
- 当创建AUFS时，如果存在同名文件，只会显示挂载时出现在最前面文件夹中的内容。
- 



# 检查系统是否支持AUFS

在使用aufs之前，可以通过下面的命令确认当前系统是否支持aufs，如果不支持，请自行根据相应发行版的文档安装。
```bash
grep aufs /proc/filesystems
# 1) 如果没有输出, 则表示不支持aufs
# 2) 存在输出, 则表示支持aufs, 下面是支持aufs时该命令的输出结果:
#    nodev   aufs
# nodev表示该文件系统不需要建在设备上
```

# 安装AUFS

## CentOS7中安装AUFS

添加yum仓库：向/etc/yum.repos.d/中添加kernel-ml-aufs.repo仓库配置。
```bash
yum-config-manager --add-repo=https://yum.spaceduck.org/kernel-ml-aufs/kernel-ml-aufs.repo
```
安装。
```bash
yum install kernel-ml-aufs
```
修改内核启动。
```bash
vi /etc/default/grub

# file: /etc/default/grub
GRUB_DEFAULT=0  # 默认为saved,表示下次启动时默认启动上次的内核,修改为0表示启动时选择第一个内核
```
重新生成grub.cfg，并重启计算机。
```bash
grub2-mkconfig -o /boot/grub2/grub.cfg

reboot # 重启计算机
```
查看是否支持（下面2种方式选1种即可）。
```bash
cat /proc/filesystems         # 1
# ...
# nodev	aufs
# ...
grep aufs /proc/filesystems   # 2
# nodev	aufs
```

# 环境准备

环境准备：
```text
## 创建目录结构
/home/user/tmp
     │
     │-----------container
     │              │--container.txt
     │              
     │--------------image1
     │                 │--001.txt
     │                 │--002.txt
     │
     │--------------image2
     │                 │--002.txt
     │                 │--003.txt 
     │
     │--------------image3
     │                 │--004.txt

cd /home/user/tmp
## 写入内容
echo image1 > image1/001.txt 
echo image1 > image1/002.txt 
echo image2 > image2/002.txt 
echo image2 > image2/003.txt 
echo image3 > image3/004.txt 
```
# 只读挂载

挂载创建AUFS：
```bash
sudo mount -t aufs -o dirs=./container1:./image1=ro:./image2=ro:./image3=ro none ./mnt
## 如果出现错误执行下面的命令
sudo mount -t aufs -o dirs=./container1:./image1=ro:./image2=ro:./image3=ro,xino=/dev/shm/aufs.xino none ./mnt
```
查看结果：
```bash
pwd # /home/xcxiao/tmp
ls mnt
# 结果:

```


验证写时复制：
```bash
# 修改 mnt 目录下的 image1.txt 文件
echo Hello, Image layer1 changed! > mnt/image1.txt

# 查看下 image1/image1.txt 文件内容
cat image1/image1.txt
Hello, Image layer1!
# 可以看到,镜像层的 image1.txt 文件并未被修改

# 查看一下"容器层"对应的 image1.txt 文件内容
ls -l container1/
total 8
-rw-rw-r-- 1 ubuntu ubuntu 24 Sep  9 16:55 container1.txt
-rw-rw-r-- 1 ubuntu ubuntu 29 Sep  9 17:21 image1.txt
# 可以看到多了文件image1.txt
# 查看文件内容
/tmp/aufs$ cat container1/image1.txt
Hello, Image layer1 changed!
# AUFS 在"容器层"自动创建了 image1.txt 文件，并且内容为刚才写入的内容

```
# 读写挂载

