
当yum安装和升级软件时，会查找yum仓库中各个源。

yum仓库以.repo文件表示，存放在`/etc/yum.repo.d` 目录下，一个repo文件中可以定义多个软件源。

# yum仓库

yum仓库可以以2种方式定义：


- yum.conf配置文件中。
- `/etc/yum.repo.d` 目录下的 .repo文件中（推荐方式）。



