# 1）连接SSH服务端

一般来说，SSH客户端可以是XShell等，SSH服务端是Linux服务器。

通过 SSH Client 我们可以连接到运行了 SSH Server 的远程机器上。SSH Client 的基本使用方法是：
```shell
ssh user@remote -p port
```
其中：
- user -- 在远程机器上的用户名，如果不指定的话默认为当前用户。
- remote -- 远程机器的地址，可以是 IP，域名，或者是后面会提到的别名。
- port -- SSH Server 监听的端口，如果不指定的话就为默认值 22。

知道了上面这三个参数，用任意的 SSH Client 都能连接上 SSH Server。


# 2）免密登陆

上面的方式，每次登录都需要输入登入用户在服务器端的密码，下面介绍实现免密登录的方法。

正常情况下，服务器端保存公钥，客户端保存私钥。<font color=44cf57>只要服务器端的用户家目录下的.ssh/authorized_keys文件中保存有公钥内容，当客户端任意用户持有对应的私钥，即可以服务器端的特定用户（即~/.ssh/authorized_keys文件中存有该私钥对应的公钥内容的用户）的身份登录SSH服务器端。</font>

## 生成公钥-密钥对（ssh-keygen）

使用 ssh-keygen 命令生成公钥-密钥对。

参数：
- -t：指定密钥类型。包括dsa、ecdsa、ed25519、rsa。
- -f：指定生成的密钥存放的路径及名称。
- -C（大写）：指定备注信息。

通过下面的方式生成公钥-密钥对。
```shell
ssh-keygen -t ed25519 -f D:\key000 -C description000
```

在-f指定的位置（D盘）下面就会生成如下两个文件：
key000：密钥。
key000.pub：公钥 。

公钥内容如下：
```text
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIF1C4b8MFjEew7WhW6PfZCvZaLN/RrxoFGyZy2aDUuTq description000
```

## 将公钥发送给服务端（ssh-copy-id）

理论上，<font color=44cf57>只要服务器端~/.ssh/authorized_keys文件存在公钥内容即可，因此除了下面的方式，使用其它方式拷贝也行。</font>

使用 ssh-copy-id 命令将公钥发送给服务端，本质上就是创建~/.ssh/authorized_keys文件并写入公钥内容。

参数：
- -i：指定公钥文件。
- -p：指定远程服务端端口号。

```shell
ssh-copy-id -i ~/.ssh/id_rsa.pub -p 22 username@192.168.xx.xx
```

## 修改文件权限

/home/username/目录的权限不能超过755，这里设为755。
```shell
sudo chmod 755 /home/username  
```
.ssh权限为700。
```shell
chmod 700 .ssh
```
authorized_keys 权限设为600。
```shell
chmod 700 .ssh/authorized_keys
```

完成上面所有的步骤后，即可通过不同方式免密登录SSH服务器。

## 使用SSH命令免密连接SSH服务端

以Windows的cmd命令行为例。

```shell
ssh -i C:\Users\XiangCXiao\.ssh\key000 xcxiao@192.168.65.132
```

需要注意的是，当以这种方式连接SSH服务器时，通过-i指定的私钥文件需要放在用户家目录的.ssh目录下，否则会出现 `Permissions for 'D:\\key000'(放在其它位置上的私钥) are too open.`

## 使用XShell免密连接SSH服务端

使用XShell按照普通方式
