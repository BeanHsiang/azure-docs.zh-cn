---
title: 使用 Azure AD 应用程序代理为发布的应用设置自定义主页 | Microsoft 文档
description: 介绍有关 Azure AD 应用程序代理连接器的基础知识
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: celested
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3143dfafdd402bd9eeb2f201f4e728d84c4b9f09
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2019
ms.locfileid: "64691169"
---
# <a name="set-a-custom-home-page-for-published-apps-by-using-azure-ad-application-proxy"></a>使用 Azure AD 应用程序代理为发布的应用设置自定义主页

本文介绍如何配置应用以将用户引向自定义的主页上，可能会因它们是内部还是外部。 当发布具有应用程序代理的应用时，设置内部 URL，但有时候这不是用户应首先看到的页面。 设置自定义主页，以便用户在访问应用程序时获取正确的页面。 用户将看到自定义主页设置，而不考虑是否从 Azure Active Directory 访问面板或 Office 365 应用启动器访问应用。

当用户启动应用时，他们会定向到已发布的应用的根域 URL 默认情况下。 登陆页通常设置为主页 URL。 使用 Azure AD PowerShell 模块来定义自定义主页 URL 时要应用用户登录到应用程序中的特定页面。

下面的一种方案，介绍为什么您的公司将自定义主页和为什么会根据用户的类型不同：

- 由于你有其他资产 （如图像） 的应用程序代理需要访问的文件夹结构的顶层，发布应用程序与`https://ExpenseApp`作为内部 URL。
- 但是，在公司网络，用户转到`https://ExpenseApp/login/login.aspx`登录并访问你的应用。
- 默认外部 URL 是`https://ExpenseApp-contoso.msappproxy.net`，不会将外部用户到登录页。
- 你想要设置`https://ExpenseApp-contoso.msappproxy.net/login/login.aspx`作为外部主页 URL 相反，因此外部用户会看到登录页第一次。

>[!NOTE]
>向用户提供对已发布应用的访问权限时，会在 [Azure AD 访问面板](../user-help/my-apps-portal-end-user-access.md)和 [Office 365 应用启动器](https://www.microsoft.com/microsoft-365/blog/2016/09/27/introducing-the-new-office-365-app-launcher/)中显示这些应用。

## <a name="before-you-start"></a>开始之前

在设置主页 URL 之前，请注意以下要求：

- 您指定的路径必须是根域 URL 的子域路径。

  例如，如果根域 URL 为`https://apps.contoso.com/app1/`，你配置的主页 URL 必须以与`https://apps.contoso.com/app1/`。

- 如果对发布的应用做了更改，这种更改可能会重置主页 URL 的值。 将来更新应用时，应该重新检查并根据需要更新主页 URL。

你可以在外部或内部主页上通过 Azure 门户或使用 PowerShell。

## <a name="change-the-home-page-in-the-azure-portal"></a>更改 Azure 门户的主页

若要更改你的应用通过 Azure AD 门户的外部和内部主页，请按照下列步骤：

1. 登录到[Azure Active Directory 门户](https://aad.portal.azure.com/)。 在 Azure Active Directory 管理中心内的仪表板将出现。
2. 在侧栏中选择**Azure Active Directory**。 Azure AD 概述页会显示。
3. 在概述边栏中，选择**应用注册**。 显示已注册应用的列表。
4. 从列表中选择您的应用程序。 将显示一个页面，显示已注册的应用的详细信息。
5. 选择下的链接**重定向 Uri**，其中显示适用于 web 和公共客户端类型的重定向 Uri 的数量。 随即显示已注册的应用的身份验证页。
6. 中的最后一行**重定向 Uri**表中，设置**类型**列**公共客户端 （移动和桌面）**，然后在**重定向 URI**列中，键入你想要使用的内部 URL。 新的空行如下所示刚刚修改的行。
7. 在新行中，设置**类型**列与**Web**，然后在**重定向 URI**列中，键入你想要使用的外部 URL。
8. 如果你想要删除任何现有的重定向 URI 行，选择**删除**每个不需要行旁边的图标 （垃圾回收会）。
9. 选择“保存”。

## <a name="change-the-home-page-with-powershell"></a>使用 PowerShell 更改主页

若要配置的应用程序使用 PowerShell 主页上，需要：

1. 安装 Azure AD PowerShell 模块。
2. 查找应用的 ObjectId 值。
3. 更新应用程序的主页 URL 使用 PowerShell 命令。

### <a name="install-the-azure-ad-powershell-module"></a>安装 Azure AD PowerShell 模块

在使用 PowerShell 定义自定义主页 URL 之前，请安装 Azure AD PowerShell 模块。 可以从 [PowerShell 库](https://www.powershellgallery.com/packages/AzureAD/2.0.2.16)下载使用图形 API 终结点的包。

若要安装包，请执行以下步骤：

1. 打开标准 PowerShell 窗口，并运行以下命令：

   ```powershell
   Install-Module -Name AzureAD
   ```

    如果以非管理员身份运行该命令，请使用 `-scope currentuser` 选项。

2. 在安装期间，请选择“Y”安装来自 Nuget.org 的两个包。这两个包是必需的。

### <a name="find-the-objectid-of-the-app"></a>查找应用的 ObjectId

通过搜索应用程序的显示名称或主页上获取应用的 ObjectId。

1. 在同一 PowerShell 窗口中，导入 Azure AD 模块。

   ```powershell
   Import-Module AzureAD
   ```

2. 以租户管理员身份登录到 Azure AD 模块。

   ```powershell
   Connect-AzureAD
   ```

3. 查找应用。 此示例使用 PowerShell 通过使用显示名称的应用的搜索查找 ObjectId `SharePoint`。

   ```powershell
   Get-AzureADApplication | Where-Object { $_.DisplayName -eq "SharePoint" } | Format-List DisplayName, Homepage, ObjectId
   ```

   随后应会返回如下所示的结果。 复制 ObjectId GUID，以便在下一节中使用。

   ```console
   DisplayName : SharePoint
   Homepage    : https://sharepoint-iddemo.msappproxy.net/
   ObjectId    : 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
   ```

   或者，您可能只需拉取所有应用的列表、 使用特定的显示名称或主页上，应用的列表中搜索和复制应用的 ObjectId 后找到该应用。

   ```powershell
   Get-AzureADApplication | Format-List DisplayName, Homepage, ObjectId
   ```

### <a name="update-the-home-page-url"></a>更新主页 URL

创建主页 URL，并更新您的应用程序具有此值。 继续使用相同的 PowerShell 窗口中，或如果你使用新的 PowerShell 窗口，登录到 Azure AD 模块，然后再次使用`Connect-AzureAD`。 然后执行以下步骤：

1. 创建一个变量来保存上一节中复制的 ObjectId 值。 （将为你的应用的 ObjectId 值此 SharePoint 示例中使用的 ObjectId 值。）

   ```powershell
   $objguid = "8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4"
   ```

2. 确认你有正确的应用程序通过运行以下命令。 输出应该与在上一节中看到的输出完全一致 ([查找应用的 ObjectId](#find-the-objectid-of-the-app))。

   ```powershell
   Get-AzureADApplication -ObjectId $objguid | Format-List DisplayName, Homepage, ObjectId
   ```

3. 创建一个空白应用程序对象用于保存想要做出的更改。

   ```powershell
   $appnew = New-Object "Microsoft.Open.AzureAD.Model.Application"
   ```

4. 将主页 URL 设置为所需的值。 该值必须是已发布应用的子域路径。 例如，如果将主页 URL 从 `https://sharepoint-iddemo.msappproxy.net/` 更改为 `https://sharepoint-iddemo.msappproxy.net/hybrid/`，则应用用户将直接转到自定义主页。

   ```powershell
   $homepage = "https://sharepoint-iddemo.msappproxy.net/hybrid/"
   ```

5. 请在主页的更新。

   ```powershell
   Set-AzureADApplication -ObjectId $objguid -Homepage $homepage
   ```

6. 若要确认更改成功，再次请从步骤 2 中运行以下命令。

   ```powershell
   Get-AzureADApplication -ObjectId $objguid | Format-List DisplayName, Homepage, ObjectId
   ```

   对于我们的示例的输出应现在出现，如下所示：

   ```console
   DisplayName : SharePoint
   Homepage    : https://sharepoint-iddemo.msappproxy.net/hybrid/
   ObjectId    : 8af89bfa-eac6-40b0-8a13-c2c4e3ee22a4
   ```

7. 重新启动应用以确认主页上显示为第一个屏幕上，按预期方式。

>[!NOTE]
>对应用进行任何更改都可能会重置主页 URL。 如果主页网址重置，请重复执行此部分中的步骤，重置主页网址。

## <a name="next-steps"></a>后续步骤

- [使用 Azure AD 应用程序代理启用对 SharePoint 的远程访问](application-proxy-integrate-with-sharepoint-server.md)
- [教程：在 Azure Active Directory 中添加用于通过应用程序代理远程访问的本地应用程序](application-proxy-add-on-premises-application.md)