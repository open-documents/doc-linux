
考虑这样一个场景：一个完整的Web应用除了本身提供Web服务外，可能还需要Redis服务、Mysql服务、Mq服务等等，同时在分布式服务中，每个服务可能同时需要多个容器实例，因此每次启动整个Web应用都需要依次启动多个容器实例，这是耗时的，Docker Compose（Compose）能够提供解决方案。

参考文档：https://docs.docker.com/compose/

Compose是用于定义和启动多容器应用的Docker工具：
- 使用一个yaml文件来配置一个应用中的所有服务。
- 使用一个简单命令来创建和启动所有配置后的服务。
