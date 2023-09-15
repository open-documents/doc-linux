CentOS7以前，init是操作系统内核第一个调用的程序，然后由init去运行所有系统需要的服务。
所有服务启动脚本放置于 **`/etc/init.d`** 目录，基本上都是使用bash shell缩写成的脚本程序。

# init服务的启动、关闭、重启、查看状态

需要启动、关闭、重启、查看状态时，可以通过如下的方式来处理。

启动：/etc/init.d/daemon start
关闭：/etc/init.d/daemon stop
重新启动：/etc/init.d/daemon restart
查看状态：/etc/init.d/daemon status

# init服务的分类

init服务的分类中，根据服务是独立启动还是被一个总管程序管理而分为两类。

## 独立启动模式（stand alone）

服务独立启动，该服务常驻于内存中，为本机或用户提供服务，反应速度快。
## 超级守护进程（super daemon）

由特殊的xinetd或inted这两个总管程序提供socket对应或端口对应的管理。因为xinetd和inetd能够管理其他服务，因此他两又被称为超级守护进程。
当没有用户要求某socket或端口时，所需要的服务不回被启动。当有用户要求某socket或端口时，xinetd才会去唤醒对应的服务程序，当该要求结束时，此服务也会结束。
优点：可以通过super daemon来执行服务的时程、连接需求等的控制。
缺点：唤醒服务需要一点实际的延迟。

# init服务依赖性问题

init在管理自己手动处理存在依赖的服务时，是无法协助唤醒被依赖服务的。

# 运行级别

init可以根据用户自定义的运行级别（runlevel）来唤醒不同的服务，以进入不同的操作界面。Linux提供了7个运行级别，分别是0~6。其中比较重要的是下面3个运行级别：
1：单人维护模式。
3：纯命令行模式。
5：图形界面。

与各个运行级别的脚本相关的脚本都放置于 **`/etc/rc.d/rc[0-6]/SXXdaemon`** 下。
- 其中S表示启动该服务，XX是数字，表示启动顺序
- 这些SXXdaemon都链接到 **`/etc/init.d/daemon`**。

## 制定运行级别默认要启动的服务

若要建立如上提到的SXXdaemon，不需要手动建立链接文件，通过下面的命令可以来处理<font color=44cf57>默认启动</font>、<font color=44cf57>默认不启动</font>、<font color=44cf57>查看默认启动与否</font>。
默认启动：chkconfig daemon on
默认不启动：chkconfig daemon off
查看默认启动与否：chkconfig --list daemon

## 切换运行级别

如果想要从命令行界面（runlevel3）切换到图形界面（runlevel5），不需要重启，只需要运行 init x（x为运行级别，此例中为5） 。init会自动分析 **`/etc/rc.d/rc[35].d`** 这两个目录内的脚本，然后启动转换运行级别中需要的服务，完成整体的运行级别的切换。
