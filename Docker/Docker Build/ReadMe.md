
参考文档：https://docs.docker.com/build/

Docker Build（记为Build）是Docker Engine中使用最频繁的特征。无论何时创建一个镜像，使用到的都是Build。

Docker Build使用C-S架构：
- BuildX：C端，为用户提供运行、管理Build的接口。
- BuildKit：S端，接受构建命令，真正进行构建。
<img src="D:\Obsidian\note_obsidian\运维\Docker\Docker Build\PIC\build-high-level-arch.png" alt="无法显示"/>