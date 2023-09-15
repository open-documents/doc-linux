
centos7以前查看当前运行级别命令：runlevel。

| 运行级别 | 说明                          | 对应的target      | 说明                   |
| -------- | ----------------------------- | ----------------- | ---------------------- |
| 0        | 关机                          | shutdown.target   | 关机                   |
| 1        | 单用户模式，主要用于系统修复  | multi-user.target | 非图形界面的多用户模式 |
| 2        | 不完全的命令行模式，不包含NFS | multi-user.target | 非图形界面的多用户模式 |
| 3        | 完全命令行模式，标准字符界面  | multi-user.target | 非图形界面的多用户模式 |
| 4        |系统保留| multi-user.target | 非图形界面的多用户模式 |
| 5        |图形模式|graphical.target|图形界面的多用户模式|
| 6         |重启|reboot.target|重启|