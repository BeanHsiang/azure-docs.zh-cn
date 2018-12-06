---
title: 在 Azure 上部署 IBM Db2 pureScale
description: 了解如何部署最近用于将企业从其 z/OS 上运行的 IBM Db2 环境迁移到 Azure 上的 IBM Db2 pureScale 的[示例体系结构](ibm-db2-purescale-azure.md)。
services: virtual-machines-linux
documentationcenter: ''
author: njray
manager: edprice
editor: edprice
tags: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/09/2018
ms.author: njray
ms.openlocfilehash: 24bdc9867af869437cc0e440a80e5bf4813abbae
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/19/2018
ms.locfileid: "51977926"
---
# <a name="deploy-ibm-db2-purescale-on-azure"></a>在 Azure 上部署 IBM Db2 pureScale

本文介绍如何部署最近用于将企业从 z/OS 上运行的 IBM Db2 环境迁移到 Azure 上的 IBM Db2 pureScale 的[示例体系结构](ibm-db2-purescale-azure.md)。

要按照迁移期间使用的步骤进行操作，请参阅 GitHub 上 [Db2onAzure](http://aka.ms/db2onazure) 存储库中的安装脚本。 这些脚本基于典型的中型联机事务处理 (OLTP) 工作负载所用的体系结构。

## <a name="get-started"></a>入门

要部署此体系结构，请下载并运行 GitHub 上 [Db2onAzure](http://aka.ms/db2onazure) 存储库中的 deploy.sh 脚本。

存储库还包括可用于设置 Grafana 仪表板的脚本，该仪表板可用于查询 Promheus，即 Db2 附带的开源监视和警报系统。

> [!NOTE]
> 客户端上的 deploy.sh 脚本创建私有 SSH 密钥，并通过 HTTPS 将其传递给部署模板。 为提高安全性，建议使用 [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview) 来存储机密、密钥和密码。

## <a name="how-the-deployment-script-works"></a>部署脚本的工作原理

deploy.sh 脚本创建并配置此体系结构中使用的 Azure 资源。 该脚本会提示输入目标环境中使用的 Azure 订阅和虚拟机，然后执行以下操作：

-   在 Azure 上设置资源组、虚拟网络和子网以进行安装。

-   为环境设置 NSG 和 SSH。

-   在 GlusterFS 和 Db2 pureScale 虚拟机上设置多个 NIC。

-   创建 GlusterFS 存储虚拟机。

-   创建 jumpbox 虚拟机。

-   创建 Db2 pureScale 虚拟机。

-   创建 Db2 pureScale ping 的见证虚拟机。

-   创建用于测试的 Windows 虚拟机，但不在其上安装任何内容。

接下来，部署脚本为 Azure 上的共享存储设置 iSCSI 虚拟存储区域网络 (vSAN)。 在此示例中，iSCSI 连接到 GlusterFS。 此解决方案还提供了将 iSCSI 目标安装为单个 Windows 节点的选项。 （iSCSI 通过 TCP/IP 提供共享块存储接口，允许 Db2 pureScale 设置过程使用设备接口连接到共享存储。）有关 GlusterFS 基础知识，请参阅 GlusterFS 入门中的[体系结构：卷类型](https://docs.gluster.org/en/latest/Quick-Start-Guide/Architecture/)主题。

部署脚本执行以下常规步骤：

1.  在 Azure 上设置共享存储群集。 将 GlusterFS 用于设置共享存储群集。 该操作涉及至少两个 Linux 节点。 有关设置详细信息，请参阅 Red Hat Gluster 文档中的[在 Microsoft Azure 中设置 Red Hat Gluster 存储](https://access.redhat.com/documentation/en-us/red_hat_gluster_storage/3.1/html/deployment_guide_for_public_cloud/chap-documentation-deployment_guide_for_public_cloud-azure-setting_up_rhgs_azure)。

2.  在目标 Linux 服务器上为 GlusterFS 设置 iSCSI 直接接口。 有关设置详细信息，请参阅 GlusterFS 管理指南中的 [GlusterFS iSCSI](https://docs.gluster.org/en/latest/Administrator%20Guide/GlusterFS%20iSCSI/)。

3.  在 Linux 虚拟机上设置 iSCSI 发起程序，该虚拟机将使用 iSCSI 目标访问 GlusterFS 群集。 有关设置详细信息，请参阅 RootUsers 文档中的[如何在 Linux 中配置 iSCSI 目标和发起程序](https://www.rootusers.com/how-to-configure-an-iscsi-target-and-initiator-in-linux/)。

4.  安装 GlusterFS 作为 iSCSI 接口的存储层。

创建 iSCSI 设备后，最后一步是安装 Db2 pureScale。 在 Db2 pureScale 设置过程中，将在 GlusterFS 群集上编译并安装 [IBM Spectrum Scale](https://www.ibm.com/support/knowledgecenter/SSEPGG_11.1.0/com.ibm.db2.luw.qb.server.doc/doc/t0057167.html)（以前称为 GPFS）。 通过此群集文件系统，Db2 pureScale 可在运行 Db2 pureScale 引擎的多个虚拟机之间共享数据。 有关更多信息，请参阅 IBM 网站上的 [IBM Spectrum Scale](https://www.ibm.com/support/knowledgecenter/en/STXKQY_4.2.0/ibmspectrumscale42_welcome.html) 文档。

## <a name="db2-purescale-response-file"></a>Db2 pureScale 响应文件

GitHub 存储库包含一个响应 (.rsp) 文件 Db2server.rsp，通过该文件可为 Db2 pureScale 安装生成自动脚本。 下表列出了响应文件用于设置的 Db2 pureScale 选项。 可根据需要为安装环境自定义响应文件。

> [!NOTE]
> 示例响应文件 Db2server.rsp 包含在 GitHub 上的 [Db2onAzure](http://aka.ms/db2onazure) 存储库中。 如果要使用此文件，必须先对其进行编辑，然后才能在环境中使用。

**Db2 pureScale 响应文件选项**

| 屏幕名称               | 字段                                        | 值                                                                                                 |
|---------------------------|----------------------------------------------|-------------------------------------------------------------------------------------------------------|
| 欢迎使用                   |                                              | 新安装                                                                                           |
| 选择产品          |                                              | Db2 版本 11.1.3.3. 带有 Db2 pureScale 的服务器版本                                              |
| 配置             | Directory                                    | /data1/opt/ibm/db2/V11.1                                                                              |
|                           | 选择安装类型                 | 典型                                                                                               |
|                           | 我同意 IBM 条款                     | 已选中                                                                                               |
| 实例所有者            | 实例的现有用户、用户名        | Db2sdin1                                                                                              |
| 防护用户               | 现有用户、用户名称                     | Db2sdfe1                                                                                              |
| 群集文件系统       | 共享磁盘分区设备路径            | /dev/dm-2                                                                                             |
|                           | 装入点                                  | /Db2sd\_1804a                                                                                         |
|                           | 用于数据的共享磁盘                         | /dev/dm-1                                                                                             |
|                           | 装入点（数据）                           | /Db2fs/datafs1                                                                                        |
|                           | 用于日志的共享磁盘                          | /dev/dm-0                                                                                             |
|                           | 装入点（日志）                            | /Db2fs/logfs1                                                                                         |
|                           | Db2 群集服务断路器。 设备路径 | /dev/dm-3                                                                                             |
| 主机列表                 | d1 [eth1]、d2 [eth1]、cf1 [eth1]、cf2 [eth1] |                                                                                                       |
|                           | 首选主 CF                         | cf1                                                                                                   |
|                           | 首选次 CF                       | cf2                                                                                                   |
| 响应文件和摘要 | 第一个选项                                 | 安装带有 IBM Db2 pureScale 功能的 Db2 服务器版本，并将我的设置保存在响应文件中 |
|                           | 响应文件名称                           | /root/DB2server.rsp                                                                                   |

### <a name="notes-about-this-deployment"></a>关于此部署的说明

-   在虚拟机上设置完成并重新启动后，/dev-dm0、/dev-dm1、/dev-dm2 和/dev-dm3 的值可能会发生变化（自动脚本中为 d0）。 要查找正确的值，可先发出以下命令，然后再在要运行该设置的服务器上完成响应文件：

```
   [root\@d0 rhel]\# ls -als /dev/mapper
   total 0
   0 drwxr-xr-x 2 root root 140 May 30 11:07 .
   0 drwxr-xr-x 19 root root 4060 May 30 11:31 ..
   0 crw------- 1 root root 10, 236 May 30 11:04 control
   0 lrwxrwxrwx 1 root root 7 May 30 11:07 db2data1 -\> ../dm-1
   0 lrwxrwxrwx 1 root root 7 May 30 11:07 db2log1 -\> ../dm-0
   0 lrwxrwxrwx 1 root root 7 May 30 11:26 db2shared -\> ../dm-2
   0 lrwxrwxrwx 1 root root 7 May 30 11:08 db2tieb -\> ../dm-3
```

-   设置脚本使用 iSCSI 磁盘的别名，以便可以轻松找到实际名称。

-   在 d0 上运行设置脚本时，d1、cf0 和 cf1 上的 /dev/dm- 值可能不同**\***。 Db2 pureScale 设置无关紧要。

## <a name="troubleshooting-and-known-issues"></a>故障排除和已知问题

GitHub 存储库包括由创建者维护的知识库。 该知识库列出了可能会遇到的潜在问题以及可以尝试的解决方案。 例如，在以下情况下可能发生已知问题：

-   尝试访问网关 IP 地址。

-   正在编译 GPL。

-   主机之间的安全握手失败。

-   Db2 安装程序检测到现有文件系统。

-   手动安装 GPFS。

-   在 GPFS 已创建的情况下安装 Db2 pureScale。

-   删除 Db2 pureScale 和 GPFS。

有关这些问题和其他已知问题的详细信息，请参阅 [Db2onAzure](http://aka.ms/Db2onAzure) 存储库中的 kb.md 文件。

## <a name="next-steps"></a>后续步骤

-   [GlusterFS iSCSI](https://docs.gluster.org/en/latest/Administrator%20Guide/GlusterFS%20iSCSI/)

-   [Creating required users for a Db2 pureScale Feature installation](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.qb.server.doc/doc/t0055374.html?pos=2)（创建所需用户以安装 Db2 pureScale 功能）

-   [Db2icrt - Create instance command](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.admin.cmd.doc/doc/r0002057.html)（Db2icrt - 创建实例命令）

-   [Db2 pureScale 群集数据解决方案](http://www.ibmbigdatahub.com/blog/db2-purescale-clustered-database-solution-part-1)

-   [IBM Data Studio](https://www.ibm.com/developerworks/downloads/im/data/index.html/)

-   [平台现代化联盟：Azure 上的 IBM Db2](https://www.platformmodernization.org/pages/ibmdb2azure.aspx)

-   [Azure Virtual Data Center Lift and Shift Guide](https://azure.microsoft.com/resources/azure-virtual-datacenter-lift-and-shift-guide/)（Azure 虚拟数据中心提升和迁移指南）