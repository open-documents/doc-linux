
# 前置知识

1）查询挂载信息：
```bash
df | grep ...(此处为overlay)
```
2）查询挂载详细信息（用于查询哪些目录挂载在指定目录上）：
```bash
mount -l # 可以配合管道符
```
3）启动容器：
```bash
docker run -d -p 8080:8080 --name=tomcat1 tomcat
docker run -d -p 8081:8081 --name=tomcat2 tomcat
```

3）移除容器命令：
```bash
docker rm ...(容器名称)
```

# 第一次

启动容器前：
- 无挂载信息

启动容器：
1）ID：a36873039fc562f73229ecb443280207c03efa96fc80cc437a06cd6192a61148
2）挂载信息：
```bash
overlay  17811456  4247960  13563496  24% /var/lib/docker/overlay2/fe0968d88f5e0973d61ac31796de66569d63b3ea662f7720051052174e8e27f2/merged
```
3.1）第一个与这次容器启动相关的挂载信息：
```bash
overlay on /var/lib/docker/overlay2/fe0968d88f5e0973d61ac31796de66569d63b3ea662f7720051052174e8e27f2/merged type overlay (rw,relatime,seclabel,
lowerdir=/var/lib/docker/overlay2/l/FR2RIU6RI7YZULPQFTOOIDDRCS   # 1
	   :/var/lib/docker/overlay2/l/VLCMYB6LHX3GGNF6KPINAQTZZ7    # 2
	   :/var/lib/docker/overlay2/l/5T7M3QFPEUP3MQMLVNNATFHKDJ    # 3
	   :/var/lib/docker/overlay2/l/GTSI4ZISHK5PYYGASFZNJ2SBPQ    # 4
	   :/var/lib/docker/overlay2/l/JRYNJ5C2HGEK74GQ3JWU4GCHQL    # 5
	   :/var/lib/docker/overlay2/l/QKIZBXJVVY4GQN6WJDECGECLR4    # 6 
	   :/var/lib/docker/overlay2/l/HDJNSO6T5HYFNHR7LJO5UDX44X    # 7
	   :/var/lib/docker/overlay2/l/6HMFBX625FDPSJOACRTLFZX4EI,   # 8
upperdir=/var/lib/docker/overlay2/fe0968d88f5e0973d61ac31796de66569d63b3ea662f7720051052174e8e27f2/diff,
workdir=/var/lib/docker/overlay2/fe0968d88f5e0973d61ac31796de66569d63b3ea662f7720051052174e8e27f2/work)
```
3.2）第二个与这次容器启动相关的挂载信息：
```text
nsfs on /run/docker/netns/77671fd55732 type nsfs (rw,seclabel)
```
每个lowerdir下的内容：
```bash
# 1: dev  etc
# 2: tmp
# 3: etc  tmp  usr  var
# 4: usr
# 5: root  tmp
# 6: etc  opt  root  tmp  var
# 7: etc  tmp  usr  var
# 8: bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp      usr var
```
关键是其中第3个：
```bash
cd /var/lib/docker/overlay2/l/5T7M3QFPEUP3MQMLVNNATFHKDJ
ls # 结果: usr
cd usr
ls # 结果: bin  include  lib  local  sbin  share
cd local
ls # 结果: lib  tomcat
cd tomcat 
ls
# 结果: 普通的tomcat目录
bin  BUILDING.txt  conf  CONTRIBUTING.md  lib  LICENSE  logs  native-jni-lib  NOTICE  README.md  RELEASE-NOTES  RUNNING.txt  temp  webapps  webapps.dist  work
```
upperdir下的内容：
```bash
# tmp  usr
```
workdir下的内容：
```bash
cd /var/lib/docker/overlay2/fe0968d88f5e0973d61ac31796de66569d63b3ea662f7720051052174e8e27f2/work
ls 
# work
cd work
ls 
# 无内容
```
容器ID目录下的内容：
```bash
cd /var/lib/docker/containers/a36873039fc562f73229ecb443280207c03efa96fc80cc437a06cd6192a61148
ls
# a36873039fc562f73229ecb443280207c03efa96fc80cc437a06cd6192a61148-json.log  checkpoints  config.v2.json     
# hostconfig.json  hostname  hosts  mounts  resolv.conf  resolv.conf.hash
```
# 停止容器

- 无挂载信息。
- 无法cd到merged目录（报不存在的错误）。
- lowerdir、upperdir、workdir目录没有消失。
- 容器目录没有被移除。

# 重启容器

- 挂载信息再次出现
- 能cd到merged目录。

# 第二次启动相同容器

启动：
1）容器ID：c42d067c289da704ffa8ed7b6448d20c052a14004f79925a6e557fa3f01f983f
2）容器目录：
```bash
a36873039fc562f73229ecb443280207c03efa96fc80cc437a06cd6192a61148  # 代表第一次启动的容器
c42d067c289da704ffa8ed7b6448d20c052a14004f79925a6e557fa3f01f983f  # 代表第二次启动的容器
```
3）挂载信息：
```bash
# 第二次容器挂载信息
overlay                 17811456 4248236 13563220   24% /var/lib/docker/overlay2/fd796cd35b67df0e6aad30cf8f8d5eb10de1a2fa906d37cf1632e9b7bfbf97ab/merged
# 第一次容器挂载信息
overlay                 17811456 4248236 13563220   24% /var/lib/docker/overlay2/fe0968d88f5e0973d61ac31796de66569d63b3ea662f7720051052174e8e27f2/merged
```
4.1）第一个相关的详细挂载信息：
```bash
overlay on /var/lib/docker/overlay2/fd796cd35b67df0e6aad30cf8f8d5eb10de1a2fa906d37cf1632e9b7bfbf97ab/merged type overlay (rw,relatime,seclabel,

lowerdir=/var/lib/docker/overlay2/l/N6LQB4VPMUYWKNSABFXIWQOA4D  # 1  (发生变化)
       :/var/lib/docker/overlay2/l/VLCMYB6LHX3GGNF6KPINAQTZZ7   # 2  (没有发生变化)
       :/var/lib/docker/overlay2/l/5T7M3QFPEUP3MQMLVNNATFHKDJ   # 3  (没有发生变化)
       :/var/lib/docker/overlay2/l/GTSI4ZISHK5PYYGASFZNJ2SBPQ   # 4  (没有发生变化)
       :/var/lib/docker/overlay2/l/JRYNJ5C2HGEK74GQ3JWU4GCHQL   # 5  (没有发生变化)
       :/var/lib/docker/overlay2/l/QKIZBXJVVY4GQN6WJDECGECLR4   # 6  (没有发生变化)
       :/var/lib/docker/overlay2/l/HDJNSO6T5HYFNHR7LJO5UDX44X   # 7  (没有发生变化)
       :/var/lib/docker/overlay2/l/6HMFBX625FDPSJOACRTLFZX4EI,  # 8  (没有发生变化)
upperdir=/var/lib/docker/overlay2/fd796cd35b67df0e6aad30cf8f8d5eb10de1a2fa906d37cf1632e9b7bfbf97ab/diff,
workdir=/var/lib/docker/overlay2/fd796cd35b67df0e6aad30cf8f8d5eb10de1a2fa906d37cf1632e9b7bfbf97ab/work)
```
4.2）第二个相关的详细挂载信息：
```bash
nsfs on /run/docker/netns/856b1ebd15ba type nsfs (rw,seclabel)
```

# 移除容器

发生变化：
- 容器目录下代表该容器的目录被移除。
- 无法cd到第1个lowerdir。
- 无法cd到upperdir。
- 无法cd到workdir

# 总结

当启动容器时