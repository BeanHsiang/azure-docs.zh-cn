---
title: 收集 Azure Sentinel 预览版中的 Azure AD 数据 |Microsoft Docs
description: 了解如何收集 Azure Sentinel 中的 Azure Active Directory 数据。
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 0a8f4a58-e96a-4883-adf3-6b8b49208e6a
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/30/2019
ms.author: rkarlin
ms.openlocfilehash: 315b18feb74862bbeca6ff8265ee003fbad48595
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/02/2019
ms.locfileid: "57242303"
---
# <a name="collect-data-from-azure-active-directory"></a>从 Azure Active Directory 中收集数据

> [!IMPORTANT]
> Azure Sentinel 目前处于公共预览状态。
> 此预览版在提供时没有附带服务级别协议，不建议将其用于生产工作负荷。 某些功能可能不受支持或者受限。 有关详细信息，请参阅 [Microsoft Azure 预览版补充使用条款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。

Azure Sentinel，可从中收集数据[Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md)和其流式处理到 Azure Sentinel。 你可以选择流[登录日志](../active-directory/reports-monitoring/concept-sign-ins.md)并[审核日志](../active-directory/reports-monitoring/concept-audit-logs.md)。

## <a name="prerequisites"></a>必备组件

- 如果你想要从 Active Directory 导出登录数据，必须具有 Azure AD P1 或 P2 许可证。

- 具有你想要从日志流式传输的租户的全局管理员或安全管理权限的用户。


## <a name="connect-to-azure-ad"></a>连接到 Azure AD

1. 在 Azure Sentinel，选择**数据收集**，然后单击**Azure Active Directory**磁贴。

2. 你想要流式传输到 Azure Sentinel 的日志，旁边单击**Connect**。






## <a name="next-steps"></a>后续步骤
在本文档中，您学习了如何将 Azure AD 连接到 Azure Sentinel。 若要了解有关 Azure Sentinel 的详细信息，请参阅以下文章：
- 了解如何[来了解一下你的数据和潜在威胁](quickstart-get-visibility.md)。
- 开始[检测威胁 Azure Sentinel](tutorial-detect-threats.md)。