
在谈及Bash之前，首先要简单介绍一下Shell。<font color=FF00FF>Shell是一类应用程序，提供一个界面和脚本解释功能，用户通过这个界面访问操作系统内核服务</font>，Shell需要调用其他软件配合完成用户应用程序功能。

Bash是Shell中的一种。因此可以说，Shell是一种抽象的概念。

# Shell

由于安全、复杂、繁琐等原因，用户不能直接接触内核，需要另外再开发一个程序，让用户直接使用这个程序，该程序的作用就是接收用户的操作（点击图标、输入命令），并进行简单的处理，然后再传递给内核，这样用户就能间接地使用操作系统内核了。用户界面和命令行就是这个另外开发的程序，在 Linux下，这个命令行程序叫做 Shell 。

Shell 是一个应用程序，它连接了用户和 Linux 内核，让用户能够更加高效、安全、低成本地使用 Linux 内核，这就是 Shell 的本质。

Shell 本身并不是内核的一部分，它只是在内核的基础上编写的一个应用程序，它和 QQ、迅雷、Firefox 等其它软件没有什么区别。然而 Shell 也有着它的特殊性，就是开机立马启动，并呈现在用户面前；用户通过 Shell 来使用 Linux，不启动 Shell 的话，用户就没办法使用 Linux。

Shell 的另一个重要特性是它自身就是一个 解释型的程序设计语言 ，Shell 程序设计语言支持在高级语言里所能见到的绝大多数程序控制结构，比如循环，函数，变量和数组。Shell 主要用来开发一些实用的、自动化的小工具，而不是用来开发具有复杂业务逻辑的中大型软件，例如检测计算机的硬件参数、搭建 Web 运行环境、日志分析等，Shell 都非常合适。任何在提示符下能键入的命令也能放到一个可执行的 Shell程序里，这意味着用shell语言能简单地重复执行某一任务。



# Bash

Bash（GNU Bourne-Again Shell）是一个为 GNU 计划编写的 Unix shell，它是许多 Linux 平台默认使用的 shell。

查询当前运行的Shell类型：
```shell
echo $SHELL
# /bin/bash
```

# 系统中的合法shell

Linux将允许使用的Shell写入<font color=00FFFF>`/etc/shells`</font>文件。其中`/bin/bash`是Linux默认使用的Shell。

`/bin/sh` 是 `/bin/bash` 的软连接文件。

```shell
# file: /etc/shells
/bin/sh # 链接到/bin/bash
/bin/bash
/usr/bin/sh
/usr/bin/bash
```