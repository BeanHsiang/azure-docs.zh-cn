---
title: Azure 状态监视器 v2 概述 |Microsoft Docs
description: 状态监视器 v2 的概述。 监视网站性能，无需重新部署该网站。 使用托管在本地、VM 或 Azure 上的 ASP.NET Web 应用。
services: application-insights
documentationcenter: .net
author: MS-TimothyMothra
manager: alexklim
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: tilee
ms.openlocfilehash: fac14281365ccf3c191684af8cfdebda69e734e0
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2019
ms.locfileid: "64870442"
---
# <a name="status-monitor-v2"></a>状态监视器 v2

状态监视器 v2 是发布到 PowerShell 模块[PowerShellGallery](https://www.powershellgallery.com/packages/Az.ApplicationMonitor)并不取代[状态监视器](https://docs.microsoft.com/azure/azure-monitor/app/monitor-performance-live-website-now)。 此模块提供与 IIS 托管的.NET web 应用程序的无代码的检测。
遥测数据将发送到 Azure 门户，可以[监视器](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview)你的应用程序。

> [!IMPORTANT]
> 状态监视器 v2 目前处于公共预览状态。
> 此预览版在提供时没有附带服务级别协议，不建议将其用于生产工作负荷。 某些功能可能不受支持或者受限。
> 有关详细信息，请参阅[Supplemental Terms of Use 针对 Microsoft Azure 预览版](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

## <a name="powershell-gallery"></a>PowerShell 库

https://www.powershellgallery.com/packages/Az.ApplicationMonitor


## <a name="instructions"></a>说明
- 查看我们[入门说明](status-monitor-v2-get-started.md)若要立即开始使用简洁的代码示例。
- 查看我们[详细说明](status-monitor-v2-detailed-instructions.md)深入了解如何开始。

## <a name="powershell-api-reference"></a>PowerShell API 参考
- [Disable-ApplicationInsightsMonitoring](status-monitor-v2-api-disable-monitoring.md)
- [Disable-InstrumentationEngine](status-monitor-v2-api-disable-instrumentation-engine.md)
- [Enable-ApplicationInsightsMonitoring](status-monitor-v2-api-enable-monitoring.md)
- [Enable-InstrumentationEngine](status-monitor-v2-api-enable-instrumentation-engine.md)
- [Get-ApplicationInsightsMonitoringConfig](status-monitor-v2-api-get-config.md)
- [Get-ApplicationInsightsMonitoringStatus](status-monitor-v2-api-get-status.md)
- [Set-ApplicationInsightsMonitoringConfig](status-monitor-v2-api-set-config.md)

## <a name="troubleshooting"></a>故障排除
- [故障排除](status-monitor-v2-troubleshoot.md)
- [已知问题](status-monitor-v2-troubleshoot.md#known-issues)


## <a name="faq"></a>常见问题解答

- 状态监视器 v2 是否支持代理安装？

  **是**。 必须下载状态监视器 v2 的多个选项。 如果您的计算机具有 internet 访问权限，则可以上架到 PowerShell 库使用`-Proxy`参数。 您可以手动下载此模块，并可以将其安装在计算机上或直接使用的模块。 中所述的每个选项我们[的详细说明](status-monitor-v2-detailed-instructions.md)。
  
- 如何验证启用成功？

   我们没有一个 cmdlet 以验证该支持成功。 我们建议使用[实时指标](https://docs.microsoft.com/azure/azure-monitor/app/live-stream)以快速观察到是否你的应用程序向我们发送遥测数据。
