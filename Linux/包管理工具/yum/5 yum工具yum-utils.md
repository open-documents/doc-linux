
```bash
yum install -y yum-utils
```

# yum-config-manager

yum-config-manager命令的本质是对`/etc/yum.repos.d/`（库数据的储存位置）文件夹下文件的增删查改，推荐使用yum-config-manager命令进行改动

1）添加repository
```bash
yum-config-manager --add-repo repository_url
```
# yumdownloader

功能：下载rpm包。通常使用在只下载rpm包不安装的情境下。

常用参数：
- --destdir：指定下载的软件包存放路径
- --resolve：解决依赖关系并下载所需的包

```

```



