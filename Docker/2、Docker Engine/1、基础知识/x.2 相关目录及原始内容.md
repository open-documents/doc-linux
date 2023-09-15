
docker安装后，第一次启动会在 `/var/lib/docker/` 下创建相关目录和文件。

原始目录结构：
```bash
buildkit  containers  engine-id(f)  image  network  overlay2  plugins  runtimes  swarm  tmp  volumes
```

1）buildkit（目录）：
```bash
buildkit/
├── cache.db
├── containerdmeta.db
├── content
│   └── ingest
├── executor
├── history.db
├── metadata_v2.db
└── snapshots.db
```
2）containers：无文件和目录
3）engine-id（文件）
4）image：
```bash
image/
└── overlay2
    ├── distribution(d)    
    ├── imagedb
    │   ├── content
    │   │   └── sha256
    │   └── metadata
    │       └── sha256
    ├── layerdb(d)
    └── repositories.json(f)
```
5）network：
```bash
network/
└── files
    └── local-kv.db(f)
```
6）overlay2：
```text
overlay2/
├── backingFsBlockDev(b)
└── l(d) (以后其中的文件会作为创建容器时mount -t overlay -o的lowerdir参数)
```
从库中下载镜像后
- `overlay2/`下会下载多个文件夹（数量记为N）。
- `overlay2/l` 下会多出相同数量（即N）个软链接文件，依次指向`overlay2/`下文件夹的diff目录。

7）plugins：
```bash
plugins/
├── storage
│   └── ingest
└── tmp
```
8）runtimes：无原始内容
9）swarm：无原始内容
10）tmp：无原始内容
11）volumes：
```bash
volumes/
├── backingFsBlockDev(b)
└── metadata.db(f)
```
