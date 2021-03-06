## 创建NFS服务

### 教程目的
本教程介绍一种利用容器自建NFS服务来共享cbs云盘的方法。

### NFS简介
NFS是Network File System的简写,即网络文件系统。NFS允许一个系统在网络上与他人共享目录和文件。通过使用NFS，用户和程序可以像访问本地文件一样访问远端系统上的文件。

### 容器服务创建NFS服务步骤

#### 第一步：选择镜像

![Alt text](https://mc.qcloudimg.com/static/img/6238482728fbffc531c9b029bcf78eff/image.png)

按照上图创建服务，选择 ccr.ccs.tencentyun.com/library/nfs-server tag为latest 的公有镜像

#### 第二步：在容器配置中添加数据盘

![Alt text](https://mc.qcloudimg.com/static/img/89ad7eed50c60acb52c2769f5886e809/image.png)

建议打开云硬盘的自动续费功能，防止存储在云硬盘中的数据因欠费造成不必要的损失。

#### 第三步：在容器配置中设置挂载路径

![Alt text](https://mc.qcloudimg.com/static/img/a54be48bcbe8e24410361b5a2860c43f/image.png)

请务必按照上图所示将云盘挂载到/exports中

#### 第四步：在容器配置中添加端口映射

![Alt text](https://mc.qcloudimg.com/static/img/821631db6da25f46a4446d947c66cdde/image.png)

如图，设置访问方式为集群内访问，并添加111,2049,20048三个端口映射。

#### 第五步：开启容器特权级功能
![Alt text](https://mc.qcloudimg.com/static/img/1a739ddd2e4933285af85954c4c59aea/image.png)

如图，开启容器的特权级功能。

#### 第六步：完成创建容器

服务创建完成后，可以在同一集群内创建一个挂载该nfs盘的测试服务，测试服务可以使用任意镜像，只需要在创建测试服务时选择挂载刚创建的nfs盘。若测试服务启动成功，则说明nfs服务搭建完成。挂载nfs盘的细节请参见：[挂载详情](https://www.qcloud.com/document/product/457/9112)

### 注意事项

在使用该方法创建的NFS盘服务时，请在挂载时指定挂载路径为 服务集群IP:/exports，如图所示。
![Alt text](https://mc.qcloudimg.com/static/img/c1f4835904370122094124950cb0df37/image.png)

如果出现挂载nfs盘时候挂载失败，且提示事件 mount: wrong fs type, bad option, bad superblock on ip:path, missing codepage or helper program, or other error (for several filesystems (e.g. nfs, cifs) ，可能是因为您正使用的较老集群里面不含有nfs的工具库（新建集群不会出现这种情况）。若出现这种情况，可以登录到集群节点上，按照系统的不同执行以下指令：

对于Ubuntu 16.04系统：  
```shell
apt-get install nfs-kernel-server -y
``` 
对于CentOS 7.2 系统：  
```shell
yum install nfs-utils -y
``` 
