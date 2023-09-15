systemd 先定义所有的服务为一个服务单位 （unit），并将该 unit 归类到不同的服务类型 （type） 去。 
systemd 将服务单位 （unit） 区分为如下类型：
- service
- socket
- target
- path
- snapshot
- timer

| 扩展名   | 描述                                                                                         |
| -------- | -------------------------------------------------------------------------------------------- |
| .service | 最常见的服务类型(service unit)。主要是系统服务，包括服务器本身所需要的本机服务以及网络服务。 |
| .target  | 执行环境类型(target unit): 一组 unit 的集合。即执行 xxx.target 就是执行一组Unit。            |
| .socket  |                                                                                              |


