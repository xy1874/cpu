## 1. 导入虚拟机

&emsp;&emsp;推荐使用VirtualBox。下载完毕后，请在Virtualbox中导入：管理->导入虚拟电脑，选择对应OVF文件。

![image-20210704155051556](assets/vm-1.png)

## 2. 配置虚拟网卡

&emsp;&emsp;VirtualBox中对于虚拟网卡，需要稍微配置一下才可以使用，具体的配置如下：

![image-20210704153913729](assets/vm-2.png)

![image-20210704153926882](assets/vm-3.png)

![image-20210704153948742](assets/vm-4.png)

## 3. 使用MobaXTerm

&emsp;&emsp;为了更好的使用体验，推荐使用MobaXTerm通过ssh连接到本地的虚拟机进行进一步的操作。压缩包里提供了MobaXTerm。

&emsp;&emsp;在MobaXTerm中，点击左上角“会话”。

&emsp;&emsp;在弹出窗口中，点击”SSH“，按如下内容填写（“远程主机”地址需要自己查询）。填写完毕后，点击“好 的”。输入虚拟机密码123456。

![image-20210704155500960](assets/vm-5.png)

&emsp;&emsp;连接完毕之后，虚拟机可最小化到后台，在命令行中进行操作。左侧可以与虚拟机的文件交互（上传、 下载），右侧可以输入命令。

![image-20210704155702840](assets/vm-6.png)
