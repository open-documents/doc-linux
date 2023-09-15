
yum install（reinstall）参数：
- --downloadonly：只下载rpm包不安装。（配合--downloaddir="xxx"使用）
- --downloaddir：下载的rpm包存放在本地的目录位置。

1 安装  

yum search keyword
yum install 全部安装  
yum install package1 安装指定的安装包package1  
yum groupinsall group1 安装程序组group1
yum grouplist



2 更新和升级  

yum来升级软件，，当然可以用sudo。  

yum check-update检查
yum update packagename 对某个单独包进行升级。  
yum update，这一步是必须的，yum会从服务器的header目录下载rpm的header，放在本地的缓存中



yum update 全部更新  
yum update package1 更新指定程序包package1  
yum check-update 检查可更新的程序  
yum upgrade package1 升级指定程序包package1  
yum groupupdate group1 升级程序组group1

3 查找和显示  

yum info：列出所有软件包的信息
yum info `[package]`：显示指定软件包 package 的信息
yum info updates：列出所有可更新软件包信息
yum info installed：列出所有已安装软件包信息
yum info extras：列出所有已安装但不在yum repository内的软件包信息

yum provides：列出软件包提供了哪些文件 


yum list 显示所有已经安装和可以安装的程序包
yum list updates
yum list installed

yum list package1 显示指定程序包安装情况package1  
yum groupinfo group1 显示程序组group1信息yum search string 根据关键字string查找安装包


yum list extras：列出所有已安装但不再yum repository内的软件包

4 删除程序  
yum remove &#124; erase package1 删除程序包package1  
yum groupremove group1 删除程序组group1  
yum deplist package1 查看程序package1依赖情况

5 清除缓存  
yum clean packages 清除缓存目录下的软件包  
yum clean headers 清除缓存目录下的 headers  
yum clean oldheaders 清除缓存目录下旧的 headers  
yum clean, yum clean all (= yum clean packages; yum clean oldheaders) 清除缓存目录下的软件包及旧的headers

清除YUM缓存  
yum 会把下载的软件包和header存储在cache中，而不会自动删除。如果我们觉得它们占用了磁盘空间，可以使用yum clean指令进行清除，更精确的用法是yum clean headers清除header，yum clean packages清除下载的rpm包，yum clean all 清除所有  
1.清除缓存目录(/var/cache/yum)下的软件包  
命令：yum clean packages

2.清除缓存目录(/var/cache/yum)下的 headers

命令：yum clean headers

3.清除缓存目录(/var/cache/yum)下旧的 headers

命令：yum clean oldheaders

4.清除缓存目录(/var/cache/yum)下的软件包及旧的headers

命令：yum clean, yum clean all (= yum clean packages; yum clean oldheaders)


安装最快源 yum install yum-fastestmirror