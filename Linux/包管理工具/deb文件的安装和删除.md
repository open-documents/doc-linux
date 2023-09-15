
# 安装

下面展示在 Ubuntu 和基于 Debian 的 Linux 发行版中安装 .deb 文件。

在命令行中安装 deb 软件包，你可以使用 `apt` 命令或者 `dpkg` 命令。实际上，`apt` 命令在底层上使用 dpkg 命令，但是 `apt` 却更流行和易于使用。

如果你想对 deb 文件使用 `apt` 命令，像这样使用它：

```text
sudo apt install path_to_deb_file
```

如果你想对将要安装的 deb 软件包使用 `dpkg` 命令，在这里是如何完成它：

```text
sudo dpkg -i path_to_deb_file
```

在这两个命令中，你应该使用你已下载的 deb 文件的路径和名称来替换 `path_to_deb_file` 。

如果你在安装 deb 软件包的过程中得到一个依赖项的错误，你可以使用下面的命令来修复依赖项的问题：

```text
sudo apt install -f
```

# 删除

移除一个 deb 软件包也不是一件什么大事。并且，你不需要用于安装程序的原始的 deb 文件。

**方法 1: 使用 apt 命令移除 deb 软件包**

你所需要的全部东西就是你所已安装程序的名称，接下来你可以使用 `apt` 或 `dpkg` 来移除这个程序。

```text
sudo apt remove program_name
```

现在，问题来了，在移除命令中，你如何找到你所需要使用的准确的程序名称？为此，`apt` 命令也有一个解决方案。

你可以使用 `apt` 命令找到所有已安装文件的列表，但是手动完成这一过程将会是一件令人头疼的事。因此，你可以使用 `grep` 命令来搜索你的软件包。

例如，在先前的部分中，我已安装 AppGrid 应用程序，但是如果我想知道准确的程序名称，我可以像这样使用一些东西：

```text
sudo apt list --installed | grep grid
```

这将给予我全部的名称中含有 “grid” 的软件包，从这里，我可以得到准确的程序名称。

```text
apt list --installed | grep grid
WARNING: apt does not have a stable CLI interface. Use with caution in scripts.
appgrid/now 0.298 all [installed,local]
```

正如你所看到的，一个名称为 “appgrid” 的软件包已经安装。现在，你可以在 `apt remove` 命令中使用这个程序名称。

**方法2: 使用 dpkg 命令移除 deb 软件包**

你可以使用 `dpkg` 来找到已安装程序的名称：

```text
dpkg -l | grep grid
```

该输出将给予所有的名称中有 “grid” 的软件包。

```text
dpkg -l | grep grid

ii appgrid 0.298 all Discover and install apps for Ubuntu
```

在上面的命令输出中的 `ii` 意味着软件包已经被正确地安装。

现在，你有了程序名称，你可以使用 `dpkg` 命令来移除它：

```text
dpkg -r program_name
```

**提示：更新 deb 软件包**

一些 deb 软件包 （像 Chrome）通过系统更新来提供其更新，但是对于大多数的其它的程序，你将不得不先移除已存在的程序，并在接下来安装更新的版本。