
参考文档：https://docs.docker.com/compose/compose-file/
Compose文件规范：https://github.com/compose-spec/compose-spec/blob/master/spec.md

忽略 Compose 文件中unknown的属性：


# version

Compose规范中定义了 version 顶级属性。

- 作用：告知信息
- 保留是为了兼容性

# name

Compose规范中定义了 name 顶级属性。

有下面2种方式指定工程名称。
1）Compose规范中的name属性。
```yml
services:
    service1:
	    name: xxx
```


```yml
services:
    service1:
	    environment:
	        - COMPOSE_PROJECT_NAME
		command: echo "I'm running ${COMPOSE_PROJECT_NAME}"
```

# 服务配置

## 服务基本属性

build：

image：

container_name：容器名称，必须匹配 `[a-zA-Z0-9][a-zA-Z0-9_.-]+`

## 服务启动顺序

depends_on：定义了该服务与其它服务之间启动和关闭的顺序。

短语法：
- 指定其它服务的名称。
- 创建于指定服务之后。短语法中，Compose只能保证在启动该服务之前，该服务所依赖的服务已经开始启动，而无法保证所依赖的服务已经启动完毕（可以通过长语法控制）。
- 关闭于指定服务之前。
例子：`redis` 和 `db` 将会在 `web` 容器服务启动前创建，`web` 容器服务将会在 `redis` 和 `db` 关闭之前关闭。
```text
services:
    web:
	    build: .
	    depends_on:
	        - db
	        - redis
    redis:
	    image: redis
    db:
	    image: postgres
```
长语法：
- restart -- Compose在更新完所依赖的服务后是否会重启该服务。这会导致容器的重启受到Compose的控制，而在                             容器"死"后，容器不会自动重启。
- condition -- 服务的依赖项需要满足的条件，有3个可选值。（V3版本中不再支持）
	- service_started -- 等价于短语法的条件。
	- service_healthy -- 服务所依赖的服务在启动前需要满足healthcheck。
	- service_completed_successfully -- 所依赖的服务完全启动后，服务才会启动。

例子：
```text
services:
    web:
	    build: .
	    depends_on:
	        db:
	            condition: service_healthy
		        restart: true
	        redis:
	            condition: service_started
    redis:
	    image: redis
    db:
	    image: postgres
```

## 服务的部署

deploy

device_cgroup_rules

## 配置IO

一、blkio_config：用于配置服务的阻塞IO。
```yml
services:
	service1:
	    blkio_config:
	        weight: 300
		    weight_device:
			    - path: /dev/sda
	            weight: 400
			device_read_bps:
	            - path: /dev/sdb
                rate: '12mb'
            device_read_iops:
                - path: /dev/sdb
                rate: 120
            device_write_bps:
                - path: /dev/sdb
                rate: '1024k'
            device_write_iops:
                - path: /dev/sdb
                rate: 30
```
1）子属性 -- weight：分配给每个设备带宽的权重。默认500，范围10~10000。
2）子属性 -- weight_device：对分配给指定设备的带宽进行权重微调。必须有2个子属性。
- path：设备路径。
- weight：分配的设备权重值。
.1）device_read_bps、evice_write_bps：设置指定设备每秒的read/write速率限制。必须有2个子属性。
- path：设备路径。
- rate：每秒 read/write 速率。要么是整数值（如400），要么是字符串（如1024k）。

```yml
services:
    service1:
		device_read_bps:
			- path: /dev/sdb
			rate: '12mb'
		device_write_bps:
			- path: /dev/sdb
			rate: '1024k'

```
.2）device_read_iops、device_write_iops：设置指定设备每秒的操作数量限制。必须有2个子属性。
- path：设备路径。
- rate：每秒 操作数量 限制。要么是整数值（如400），要么是字符串（如1024k）。
```yml
services:
    service1:
		device_read_iops:
			- path: /dev/sdb
			rate: 120
		device_write_iops:
			- path: /dev/sdb
			rate: 30
```

## 配置CPU

1）cpu_count：分配给服务容器的CPU数量。
2）cpu_percent：定义服务容器可用的CPU利用率。
3）cpu_shares：定义相对于其它容器的CPU权重。整数值。
4）cpu_quota：
5）cpu_rt_runtime：
6）cpu_rt_period：
7）cpuset：
8）cap_add：
9）cap_drop：
10）cgroup：
11）cgroup_parent：

## devices

`devices` defines a list of device mappings for created containers in the form of `HOST_PATH:CONTAINER_PATH[:CGROUP_PERMISSIONS]`.

```
devices:
  - "/dev/ttyUSB0:/dev/ttyUSB0"
  - "/dev/sda:/dev/xvda:rwm"
```
## 


## configs

configs：
- 配置每个服务能够访问的配置资源。
- 值需要来自services的configs，如果services的configs中没有定义，则会报错。

1）短语法

特点：
- 只指定配置资源的名称。
- 容器启动时将会将指定的配置资源挂载到 `/<config_name>` 下。

下面的例子：redis容器服务启动后，将能访问到my_config和my_other_config配置资源，其中my_config中的内容是./my_config.txt文件中的内容，my_other_config是外部资源。
```text
services:
	redis:
	    image: redis:latest
	    configs:
	        - my_config
	        - my_other_config
configs:
    my_config:
	    file: ./my_config.txt
    my_other_config:
	    external: true
```
2）长语法

长语法提供更细的粒度来控制如何创建服务容器内部访问资源配置。
子元素：
- source    -- 外部配置资源名称，来源于顶级元素 `</configs>` 中的项。
- target     -- 外部资源挂载的路径。默认为 `/<source>`。
- uid、gid -- 容器内部挂载的文件/目录的所有者和所属组的id。默认启动容器的用户和其所属组。
- mode      -- 容器内部挂载的文件/目录的访问权限。默认为0444，即所有人只有访问权限。
例子：redis容器服务启动后能够通过访问`/redis_config` 来访问外部配置资源，`redis_config`的权限为0440，`redis_config`的所有者id为103，所属组id为103。
```text
services:
    redis:
	    image: redis:latest
	    configs:
	        - source: my_config
	          target: /redis_config
	          uid: "103"
	          gid: "103"
	          mode: 0440
configs:
    my_config:
	    external: true
    my_other_config:
	    external: true
```
