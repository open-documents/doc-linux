systemd 这个启动服务的机制仅有 systemctl 这个指令来处理。


# 二、systemctl观察系统所有服务

语法： **`systemctl [command] [--type=TYPE] [--all]`**

command主要有：
| command    | 描述                                                             |
| ---------- | ---------------------------------------------------------------- |
| list-units | 依据 unit 列出目前有启动的 unit。若加上 --all 才会列出没启动的。 |
| list-unit-files | 依据 /usr/lib/systemd/system/ 内的文件，将所有文件列表说明。 |

TYPE：unit的类型，即service，socket，target等。


# 四、systemctl分析各服务之间的依赖性

1）语法：**`systemctl list-dependencies [unit] [--reverse]`**
2）功能：查询unit使用到了哪些依赖项。
3）参数：
- --reverse：查看谁使用到了该unit。
4）特殊：
- systemctl list-dependencies（即不加任何内容）：查询default.target的依赖项。

# 五、systemctl关于网络服务

## 查看socket file

1）语法：**`systemctl list-sockets`**

2）结果
```shell
LISTEN                          UNIT                         ACTIVATES
/dev/log                        systemd-journald.socket      systemd-journald.service
/run/dbus/system_bus_socket     dbus.socket                  dbus.service
/run/dmeventd-client            dm-event.socket              dm-event.service
/run/dmeventd-server            dm-event.socket              dm-event.service
/run/lvm/lvmetad.socket         lvm2-lvmetad.socket          lvm2-lvmetad.service
/run/lvm/lvmpolld.socket        lvm2-lvmpolld.socket         lvm2-lvmpolld.service
/run/systemd/initctl/fifo       systemd-initctl.socket       systemd-initctl.service
/run/systemd/journal/socket     systemd-journald.socket      systemd-journald.service
/run/systemd/journal/stdout     systemd-journald.socket      systemd-journald.service
/run/systemd/shutdownd          systemd-shutdownd.socket     systemd-shutdownd.service
/run/udev/control               systemd-udevd-control.socket systemd-udevd.service
/var/run/avahi-daemon/socket    avahi-daemon.socket          avahi-daemon.service
/var/run/cups/cups.sock         cups.socket                  cups.service
/var/run/libvirt/virtlockd-sock virtlockd.socket             virtlockd.service
/var/run/libvirt/virtlogd-sock  virtlogd.socket              virtlogd.service
/var/run/rpcbind.sock           rpcbind.socket               rpcbind.service
@ISCSIADM_ABSTRACT_NAMESPACE    iscsid.socket                iscsid.service
@ISCSID_UIP_ABSTRACT_NAMESPACE  iscsiuio.socket              iscsiuio.service
kobject-uevent 1                systemd-udevd-kernel.socket  systemd-udevd.service
```

