
Overlay FS 从 Linux 3.18 开始正式支持，它能够将上层目录（upperdir）和下层目录（lowerdir）进行合并，提供给用户一个统一的视图目录（merged）。其合并规则如下：
- 上下层目录同名层目录合并；
- 上下层目录中同名的文件，上层覆盖下层；

Overlay FS 相比于AUFS的优点：

- 支持页缓存（Page caching）共享，多个容器访问同一个文件，可以共享一个或多个页缓存；
- copy_up 操作快，因为 overlay 只有两层，而 AUFS 有很多层（最多不能超过127层），文件穿越多层较为耗时；
- 代码融入到 Linux 内核中，广泛支持各 Linux 发行版。

# Overlay FS与Docker

Docker 容器以镜像层（Image layer）为下层目录，容器可写层（Writable layer）为上层目录，合并到容器挂载点（根目录）。早期 Linux 内核中的 Overlay FS 不支持多下层目录，Linux 4.0 以后版本才陆续完善了多下层目录的功能。我们知道 Docker 经常是多镜像层的，这意味着具有多层的下层目录。对此，Docker 提供了两种方式接入 Overlay FS：

- Overlay Driver，下层目录依次进行硬链接，然后最上层的镜像目录跟容器可写层合并到挂载点；
-  Overlay2 Driver，只支持 Linux 4.0以上版本，下层镜像目录（最多支持500个）直接根容器可写层合并到挂载点，不存在硬链接导致的 inode 消耗过多问题。

# 环境准备

创建下面的目录结构：
```text
/home/xcxiao/tmp/overlay
       |   
       |---------lower1
       |           |------001.txt
       |           |------002.txt
       |           |------dir
       |                   |----001.txt
       |---------lower2
       |           |------001.txt
       |           |------003.txt
       |           |------dir
       |                   |----002.txt
       |---------upper
       |           |------002.txt
       |           |------dir
       |     
       |---------merged    
       |---------merged 
       
```
写入如下内容：
```bash
cd /home/xcxiao/tmp/overlay

echo lower1中的001.txt > lower1/001.txt
echo lower1中的002.txt > lower1/002.txt

echo lower2中的001.txt > lower2/001.txt
echo lower1中的003.txt > lower2/003.txt

echo upper中的002.txt > upper/002.txt

```

# 挂载

```bash
cd /home/xcxiao/tmp/overlay

mount -t overlay -o lowerdir=lower1:lower2,upperdir=upper,workdir=workdir overlay ./merged
```
结果：
```bash
cd /home/xcxiao/tmp/overlay/merged

.
│---001.txt           (内容: lower1中的001.txt)
│---002.txt           (内容: upper中的002.txt)
│---003.txt           (内容: lower2中的003.txt)
│---dir
     │---001.txt      (内容: lower1/dir中的001.txt)
     │---002.txt      (内容: lower2/dir中的002.txt)

```
结果分析图：
![[Pasted image 20230624235721.png]]