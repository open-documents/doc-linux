

## 1、下载最新的 IDEA 2021.1 版本安装包

我们先从 IDEA 官网下载 IDEA 2021.1 版本的安装包，下载链接如下：

https://www.jetbrains.com/idea/download/

![IDEA 2021.1版本官网下载](https://www.cxybug.com/exception/20210408115710.png)

点击下载，静心等待其下载完毕即可。





## 2、先卸载老版本的 IDEA

> 注意，如果电脑上之前有安装老版本的 IDEA, 需要先卸载干净，否则可能安装失败！
>
> 注意，一定要先卸载干净掉老版本的 IDEA。

1.笔者之前安装了老版本的 IDEA, 所以要先卸载，未安装老版本 IDEA 的小伙伴直接跳过，看后面步骤就行:

![卸载老版本的IDEA](https://www.cxybug.com/exception/1616142110243.jpg)

卸载成功后，点击关闭:

![IDEA老版本卸载成功](https://www.cxybug.com/exception/161227500640133.jpg)

卸载成功后，双击刚刚下载好的 `idea` exe 格式安装包, 打开它；



## 3、开始安装 IDEA 2021.1 版本

2.安装目录默认为 `C:\Program Files\JetBrains\IntelliJ IDEA 2021.1`, 这里笔者选择的是默认路径:

![IDEA 2021.1安装第一步](https://www.cxybug.com/exception/161227508390517.jpg)

3.勾选自己想要创建的桌面快捷方式，笔者的操作系统是 64 位的，所以勾选的 64 位快捷方式：

![IDEA 2021.1安装第二步](https://www.cxybug.com/exception/161227511743444.jpg)

4.点击 `Install` ：

![IDEA 2021.1安装第三步](https://www.cxybug.com/exception/161227515963842.jpg)

5.安装完成后，勾选 `Run IntelliJ IDEA`，点击 `Finish` 运行软件:

![IDEA 2021.1安装第四步](https://www.cxybug.com/exception/161227518784184.jpg)





## 4、开始激活

6.等待 IDEA 2021.1 运行, 中间会先弹出一个注册框，我们勾选 `Evaluate for free`, 点击 `Evaluate`， 先试用30天:

![试用IDEA 30天](https://www.cxybug.com/exception/1616141581896.jpg)

**注意，如果没有弹出上面的界面，可执行重置30天试用期脚本，脚本网盘地址https://pan.baidu.com/s/1Yhq_7dP0MOayyEJ-g4_27A（密码：ute8）**

打开该文件夹后，有对应系统的执行脚本，执行即可：

```shell
windows系统：reset_jetbrains_eval_windows.vbs
linux/mac系统：reset_jetbrains_eval_mac_linux.sh
```

7.进入 IDEA 中， 先随便建个 Java 工程， 然后将网盘中最新的 IDEA 无限重置 30 天试用期补丁 `ide-eval-resetter-2.1.6.zip`拖入 IDEA 界面中，如下图所示：

![将IDEA破解补丁拖进ide中](https://www.cxybug.com/exception/160922391957819.jpg)





## 5、重启IDEA ！重启IDEA！

安装成功后，重启 IDEA. 可以通过点击 `Help` 菜单，若列表中出现 `Eval Reset`选项，则代表安装成功，可以参考下面的图示。





## 6、安装成功，如何使用？

- 一般来说，在IDE窗口切出去或切回来时（窗口失去/得到焦点）会触发事件，检测是否长达`25`天都没有重置30天试用期，到时候会给出通知让你选择。（初次安装因为无法获取上次重置时间，会直接给予提示）

- 也可以手动唤出插件的主界面：

  - 如果IDE没有打开项目，在`Welcome`界面点击菜单：`Get Help` -> `Eval Reset`
  - 如果IDE打开了项目，点击菜单：`Help` -> `Eval Reset`

  ![img](https://www.cxybug.com/exception/160922462708315.jpg)

  ![img](https://www.cxybug.com/exception/160922469596943.jpg)

- 唤出的插件主界面中包含了一些显示信息，`2`个按钮，`1`个勾选项：

  - 按钮：`Reload` 用来刷新界面上的显示信息，其中包括上一次重置30天试用期的时间。

  - 按钮：`Reset` 点击会询问是否重置试用30天并**重启IDE**。选择`Yes`则执行重置操作并**重启IDE生效**，选择`No`则什么也不做。（**此为手动重置方式**）

    ![img](https://www.cxybug.com/exception/160922546973318.jpg)

  - 勾选项：`Auto reset before per restart` 如果勾选了，则自勾选后**每次重启/退出IDE时会自动重置试用信息**，你无需做额外的事情。（此为自动重置方式）



## 7、如何查看剩余的试用期

进入 IDEA 界面后，点击 `Help` -> `Register` 查看：

![img](https://www.cxybug.com/exception/158691933254071.png)

可以看到，试用期还剩余30天：

![img](https://www.cxybug.com/exception/160922563192214.jpg)

无限重置大法好呀，这样我们就相当于永久激活了 IDEA 了，比较重要的点是，这种方法非常稳定，不会动不动就失效。



## <font color="red">8、补充: 千万不要升级 IDEA（非常重要）</font>

