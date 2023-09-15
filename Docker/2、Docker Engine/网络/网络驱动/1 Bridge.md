
参考文档：https://docs.docker.com/network/drivers/bridge


Bridge（网桥）是一种网络层设备，用来转发不同网络中的流量。Bridge可以是硬件设备，也可以是运行在主机内核中软件设备。

Docker Bridge属于后者，特点：
- 属于同一个网桥网络的容器之间能够相互通信。
- 不属于同一个网桥网络的容器之间不能相互通信。
- Docker Bridge应用于相同Docker daemon主机的容器之间的通信，而不适用于不同Docker daemon主机的容器之间的通信。




# 默认的桥接网络



# 自定义桥接网络

# 默认桥接网络和自定义桥接网络的差异

1）自定义桥接网络

# 相关命令

1）创建自定义一个网桥网络。
```bash
docker network create my-net
```