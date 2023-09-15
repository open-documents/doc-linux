

# 执行Shell脚本

以bash脚本文件名为 /home/xxx/shell.sh 为例。要执行该脚本，有下面几个方法：

1）直接指令下达： shell.sh 文件必须要具备可读与可执行 （rx） 的权限，然后：
- 绝对路径：使用 /home/xxx/shell.sh 来下达指令；
- 相对路径：假设工作目录在 /home/xxx/ ，则使用 ./shell.sh 来执行
- 变量“PATH”功能：将 shell.sh 放在 PATH 指定的目录内，例如： ~/bin/

当以这种方式执行bash脚本时，bash解释器会根据第一行（即#!所在的行）的内容来调用相应的解释器来执行bash脚本后面的内容。


2）以 bash 程序来执行：通过“ bash shell.sh ”或“ sh shell.sh ”来执行。

当以这种方式执行bash脚本时，


