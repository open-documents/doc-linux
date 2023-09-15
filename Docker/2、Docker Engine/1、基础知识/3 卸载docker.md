
# 删除docker所在目录

```bash
rm -rf /etc/docker
rm -rf /run/docker
rm -rf /var/lib/dockershim
rm -rf /var/lib/docker
```

# 卸载相关包

查看相关包
```bash
yum list installed | grep docker
```

卸载相关包
```bash
yum remove containerd.io.x86_64
yum remove docker-ce.x86_64
yum remove docker-ce-cli.x86_64
yum remove docker-ce-rootless-extras.x86_64
yum remove docker-compose-plugin.x86_64
yum remove docker-scan-plugin.x86_64
```

# 查看卸载完成

```bash
docker version
# bash: /usr/bin/docker: 没有那个文件或目录
```