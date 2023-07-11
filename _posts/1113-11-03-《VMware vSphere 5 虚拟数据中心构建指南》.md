# 1113-11-03-《VMware vSphere 5 虚拟数据中心构建指南》

### 1.3 虚拟化生态系统

#### 1.3.1 服务器虚拟化

必须区分裸机虚拟化产品和主服务器上的（称为基于主机的host based）产品。服务器上的基于主机的虚拟化应用可以用于测试，但是绝不能用于生产。如果基于主机的版本投入生产，副作用是灾难性的

这类产品中著名的有：

1.  Microsoft Virtual Server 2005、Virtual PC
2.  VMware server
3.  Vmware Workstation、VMware Player、VMware ACE、VMware Fusion（Mac版本）

在裸机（bare-metal）虚拟化应用程序中，虚拟化层直接安装在硬件上

下面是主要的裸机虚拟化解决方案：

1.  VMware vSphere
2.  Microsoft Hyper-V
3.  Citrix XenServer
4.  Oracle VM
5.  Red Hat KVM

VMware在虚拟化市场上处于领先，在技术上也走在Microsoft的前面。

Hyper-V很适合于当今的SMB（中小型企业），但是无法为大公司提供VMware的所有高级功能

#### 1.3.2 桌面虚拟化

工作站虚拟化使每个用户远程登录到位于数据中心的一个虚拟机。

对于便携电脑和台式机，客户端-虚拟化管理器类型的解决方案可以直接安装在现有硬件之上，这些解决方案可以让多哥相互隔离的VM运行于单个PC上。

#### 2.1.2

vSphere5是VMware ESX的第5代产品

### 2.2 vSphere5许可证

#### 2.2.1 vSphere5版本

下面的版本面向中小型企业（Small and Medium Business，SMB）

vSphere Essentials和vSphere Essentials Plus用于小规模部署。可以管理：3个主机服务器×2个物理处理器，最多192GB的vRAM。这些版本集成了一个vCenter Server Foundation许可证。

免费的虚拟化管理器只能使用vSphere基本虚拟管理器功能。它可以透明地升级到更高级的vSphere版本。和付费版本不通，vSphere虚拟化管理器不能由vCenter Server管理，只能通过与主机直接连接的vSphere Client管理
