---
title: Azure Service Fabric 的常见安全属性
description: 用于评估 Azure Service Fabric 的常见安全属性的清单
services: service-fabric
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: service-fabric
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: f12d11cecbf682ae82f9c432804b1d611ee3e39f
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2019
ms.locfileid: "64719406"
---
# <a name="common-security-attributes-for-azure-service-fabric"></a>Azure Service Fabric 的常见安全属性

安全性已集成到 Azure 服务的各个方面。 本文介绍 Azure Service Fabric 中内置的常见安全属性。 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>预防

| 安全属性 | Yes/No | 说明 |
|---|---|--|
| 静态加密：<ul><li>服务器端加密</li><li>使用客户托管密钥的服务器端加密</li><li>其他加密功能（例如客户端、始终加密等）</ul>| 是 | 客户拥有群集和虚拟机规模集上构建群集时。 可以在虚拟机规模集上启用 azure 磁盘加密。 |
| 传输中加密：<ul><li>快速路由加密</li><li>在 VNet 加密</li><li>VNet-VNet 加密</ul>| 是 |  |
| 加密密钥处理 （CMK，BYOK，等等。）| 是 | 客户拥有群集和虚拟机规模集上构建群集时。 可以在虚拟机规模集上启用 azure 磁盘加密。 |
| 列级加密 （Azure 数据服务）| 不适用 |  |
| 加密的 API 调用| 是 | Service Fabric API 调用通过 Azure 资源管理器进行。 需要有效 JSON web 令牌 (JWT)。 |

## <a name="network-segmentation"></a>网络分段

| 安全属性 | Yes/No | 说明 |
|---|---|--|
| 服务终结点支持| 是 |  |
| VNet 注入支持| 是 |  |
| 网络隔离和防火墙支持| 是 | 使用网络安全组 (NSG)。 |
| 强制隧道支持| 是 | Azure 网络支持强制隧道。 |

## <a name="detection"></a>检测

| 安全属性 | Yes/No | 说明|
|---|---|--|
| Azure 监视支持 （日志分析、 应用程序见解等）| 是 | 使用 Azure 监视支持和第三方支持。 |

## <a name="identity-and-access-management"></a>标识和访问管理

| 安全属性 | Yes/No | 说明|
|---|---|--|
| Authentication| 是 | 身份验证通过 Azure Active Directory 来进行。 |
| 授权| 是 | 通过 SFRP 进行针对调用的标识和访问管理 (IAM)。 直接对群集终结点进行的调用支持两个角色：用户和管理员。客户可以将 API 映射到任一角色。 |


## <a name="audit-trail"></a>审核线索

| 安全属性 | Yes/No | 说明|
|---|---|--|
| 控制和管理平面日志记录和审核| 是 | 所有控制平面操作都需经过审核和审批流程。 |
| 数据平面日志记录和审核| 不适用 | 客户拥有群集。  |

## <a name="configuration-management"></a>配置管理

| 安全特性 | Yes/No | 说明|
|---|---|--|
| 配置管理支持 （版本控制的配置，等等）。| 是 | |