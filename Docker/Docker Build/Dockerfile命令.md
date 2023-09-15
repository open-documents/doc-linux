
# CMD



# ENTRYPOINT

ENTRYPOINT指定容器启动时执行的命令，让容器表现为一个executable。

有2种形式：
- exec形式：`ENTRYPOINT ["executable", "param1", "param2"]`
- shell形式：`ENTRYPOINT command param1 param2`

## exec形式

exec特点：
- 命令不会在/bin/bash -c 里面进行。(1)
- 
```text
FROM ubuntu
ENTRYPOINT ["top", "-b"]
CMD ["-c"]
```
验证：
```bash
docker run -it --rm --name test top -H

# top - 08:25:00 up  7:27,  0 users,  load average: 0.00, 0.01, 0.05
# Threads:   1 total,   1 running,   0 sleeping,   0 stopped,   0 zombie
# %Cpu(s):  0.1 us,  0.1 sy,  0.0 ni, 99.7 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
# KiB Mem:   2056668 total,  1616832 used,   439836 free,    99352 buffers
# KiB Swap:  1441840 total,        0 used,  1441840 free.  1324440 cached Mem
# 
#   PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
#     1 root      20   0   19744   2336   2080 R  0.0  0.1   0:00.04 top
```


那么可能出现下面的情况：
```bash

```

最佳实践：
- 使用exec的形式来指定一个稳定的默认命令和参数。
- 使用其它方式（如CMD）来指定可能改变的参数。
```text
FROM ubuntu
ENTRYPOINT ["top", "-b"]
CMD ["-c"]
```




## shell形式

shell形式的特点：
- 不会使用CMD 和 run 提供的命令行参数。
- 作为 `/bin/bash -c` 的子命令启动。意味着可执行命令在容器中的pid不为1。
- 不能接收信号。即不能接受来自 `docker stop <container>` 提供的 SIGTERM 信号。



特点：
- `docker run <image>` 命令行参数将会附加在ENTRYPOINT之后，会覆盖 CMD命令 指定的参数。如 `docker run <image> -d `将会传递 `-d` 给entry point。
- 使用 `docker run --entrypoint` 来覆盖 ENTRYPOINT 命令。
- 所有的ENTRYPOINT只有最后一个生效。



# CMD和ENTRYPOINT组合结果

当ENTRYPOINT以shell形式出现时，CMD被忽略。
当ENTRYPOINT以exec形式出现时，CMD无论哪种形式，都会被解释为参数被附加在ENTRYPOINT指定的命令的参数之后。

|                         | ENTRYPOINT ["/bin/echo","ENTRYPOINT"] |ENTRYPOINT /bin/echo ENTRYPOINT|
| ----------------------- | ------------------------------------- | --- |
| CMD ["CMD"]             | /bin/echo ENTRYPOINT CMD(尽管此处CMD命令语法错误,也不会出现问题)|/bin/bash -c /bin/echo ENTRYPOINT|
| CMD ["/bin/echo","CMD"] | /bin/echo ENTRYPOINT /bin/echo CMD    |/bin/bash -c /bin/echo ENTRYPOINT|
| CMD CMD                 |/bin/echo ENTRYPOINT /bin/bash -c CMD(尽管此处CMD命令语法错误,也不会出现问题)|/bin/bash -c /bin/echo ENTRYPOINT|
|CMD /bin/echo CMD|/bin/echo ENTRYPOINT /bin/bash -c /bin/echo CMD|/bin/bash -c /bin/echo ENTRYPOINT|

