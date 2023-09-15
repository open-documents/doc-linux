
# sudo命令执行的流程

将当前用户切换到超级用户下，或切换到指定的用户下，  
然后以超级用户或其指定切换到的用户身份执行命令，执行完成后，直接退回到当前用户。  
具体工作过程如下：  
当用户执行sudo时，系统会主动寻找/etc/sudoers文件，判断该用户是否有执行sudo的权限  
–>确认用户具有可执行sudo的权限后，让用户输入用户自己的密码确认  
–>若密码输入成功，则开始执行sudo后续的命令


默认配置文件/etc/sudoers。使用visudo命令来修改该配置文件，它使用一个vi编辑器去打开该sudoers文件，并且会帮检查语法错误。
sudoers文件能在多个系统中共享（没懂）。

# 配置文件内容--别名项

下面是配置文件中别名项，别名项能在后面直接使用。
```text
## Host Aliases(主机别名)
## Groups of machines. You may prefer to use hostnames (perhaps using 
## wildcards for entire domains) or IP addresses instead.
# Host_Alias     FILESERVERS = fs1, fs2
# Host_Alias     MAILSERVERS = smtp, smtp2
## User Aliases(用户别名)
## These aren't often necessary, as you can use regular groups
## (ie, from files, LDAP, NIS, etc) in this file - just use %groupname 
## rather than USERALIAS
# User_Alias ADMINS = jsmith, mikem

## Command Aliases(命令别名)
## These are groups of related commands...

## Networking
# Cmnd_Alias NETWORKING = /sbin/route, /sbin/ifconfig, /bin/ping, /sbin/dhclient, /usr/bin/net, /sbin/iptables, /usr/bin/rfcomm, /usr/bin/wvdial, /sbin/iwconfig, /sbin/mii-tool

## Installation and management of software
# Cmnd_Alias SOFTWARE = /bin/rpm, /usr/bin/up2date, /usr/bin/yum

## Services
# Cmnd_Alias SERVICES = /sbin/service, /sbin/chkconfig, /usr/bin/systemctl start, /usr/bin/systemctl stop, /usr/bin/systemctl reload, /usr/bin/systemctl restart, /usr/bin/systemctl status, /usr/bin/systemctl enable, /usr/bin/systemctl disable

## Updating the locate database
# Cmnd_Alias LOCATE = /usr/bin/updatedb

## Storage
# Cmnd_Alias STORAGE = /sbin/fdisk, /sbin/sfdisk, /sbin/parted, /sbin/partprobe, /bin/mount, /bin/umount

## Delegating permissions
# Cmnd_Alias DELEGATING = /usr/sbin/visudo, /bin/chown, /bin/chmod, /bin/chgrp 

## Processes
# Cmnd_Alias PROCESSES = /bin/nice, /bin/kill, /usr/bin/kill, /usr/bin/killall

## Drivers
# Cmnd_Alias DRIVERS = /sbin/modprobe

# Defaults specification

#
# Refuse to run if unable to disable echo on the tty.
#
Defaults   !visiblepw

#
# Preserving HOME has security implications since many programs
# use it when searching for configuration files. Note that HOME
# is already set when the the env_reset option is enabled, so
# this option is only effective for configurations where either
# env_reset is disabled or HOME is present in the env_keep list.
#
Defaults    always_set_home
Defaults    match_group_by_gid

# Prior to version 1.8.15, groups listed in sudoers that were not
# found in the system group database were passed to the group
# plugin, if any. Starting with 1.8.15, only groups of the form
# %:group are resolved via the group plugin by default.
# We enable always_query_group_plugin to restore old behavior.
# Disable this option for new behavior.
Defaults    always_query_group_plugin

Defaults    env_reset
Defaults    env_keep =  "COLORS DISPLAY HOSTNAME HISTSIZE KDEDIR LS_COLORS"
Defaults    env_keep += "MAIL PS1 PS2 QTDIR USERNAME LANG LC_ADDRESS LC_CTYPE"
Defaults    env_keep += "LC_COLLATE LC_IDENTIFICATION LC_MEASUREMENT LC_MESSAGES"
Defaults    env_keep += "LC_MONETARY LC_NAME LC_NUMERIC LC_PAPER LC_TELEPHONE"
Defaults    env_keep += "LC_TIME LC_ALL LANGUAGE LINGUAS _XKB_CHARSET XAUTHORITY"

#
# Adding HOME to env_keep may enable a user to run unrestricted
# commands via sudo.
#
# Defaults   env_keep += "HOME"

Defaults    secure_path = /sbin:/bin:/usr/sbin:/usr/bin
```
# 配置文件内容--规则配置

规则配置：什么用户在哪台/些机器上使用什么命令。
语法：
```
## 语法1: users1 MACHINE=(users2) COMMANDS1 [NOPASSWD:COMMANDS2]
## 说明:
##   - 允许users1中列出的用户以users2中列出的用户的身份
##   - 在MACHINE机器上
##   - 需要输入相应用户密码的情况下执行COMMANDS1中列出的命令
##   - 不需要输入相应用户密码的情况下执行COMMANDS2中列出的命令
## 注意: 需要输入的密码是users1中用户的密码,而不是users2中用户的密码


## 语法2: users1 MACHINE=COMMANDS1 [NOPASSWD:COMMANDS2]
## 说明: 当语法1中的users2是ALL(即所有人)时,可以简化成这种语法
```
例子：
```text
# 说明: 允许root在任何机器上执行任何命令
root    ALL=(ALL)       ALL   

# 说明: 允许sys组的成员在任何机器上执行NETWORKING...命令
%sys ALL = NETWORKING, SOFTWARE, SERVICES, STORAGE, DELEGATING, PROCESSES, LOCATE, DRIVERS

# 说明: 允许wheel组的成员能在任何机器上执行任何命令
%wheel  ALL=(ALL)       ALL

# 说明: 允许wheel组的成员能在任何机器上执行任何命令而不需要密码
%wheel  ALL=(ALL)       NOPASSWD: ALL

# 说明: 允许users组的成员以root的身份挂载和卸载cdrom
%users  ALL=/sbin/mount /mnt/cdrom, /sbin/umount /mnt/cdrom

# 说明: 允许users组的成员关闭这个系统
# %users  localhost=/sbin/shutdown -h now
```

# 读取drop-in文件
```
## 说明: 从/etc/sudoers.d读取drop-in文件
## 注意: 此处的#不是注释

#includedir /etc/sudoers.d

```



