---
title: include 文件
description: include 文件
services: digital-twins
author: dsk-2015
ms.service: digital-twins
ms.topic: include
ms.date: 09/17/2018
ms.author: dkshir
ms.custom: include file
ms.openlocfilehash: e5e015c1e12e68f54ae7fd32082faed1abfebe74
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2018
ms.locfileid: "51626383"
---
1. 登录到 [Azure 门户](http://portal.azure.com)。

1. 在左窗格中，选择“创建资源”。 搜索“数字孪生”，选择“数字孪生(预览版)”。 选择“创建”，启动部署过程。

   ![用于创建新数字孪生实例的选项](./media/create-digital-twins-portal/create-digital-twins.png)

1. 在“数字孪生”窗格中，输入以下信息：
   * **资源名称**：为数字孪生实例创建一个唯一名称。
   * **订阅**：选择需要将其用于创建此数字孪生实例的订阅。 
   * **资源组**：为数字孪生实例选择或创建[资源组](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups)。
   * **位置**：选择离你的设备最近的位置。

    ![包含输入信息的“数字孪生”窗格](./media/create-digital-twins-portal/create-digital-twins-param.png)

1. 查看数字孪生信息，然后选择“创建”。 创建数字孪生实例可能需要数分钟。 可在“通知”窗格中监视进度。

1. 打开数字孪生实例的“概览”窗格。 记下“管理 API”下的链接。

   **管理 API** URL 的格式为：_https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/swagger_。 此 URL 将引导你到适用于实例的 Azure 数字孪生 REST API 文档。 阅读[如何使用 Azure 数字孪生 Swagger](../articles/digital-twins/how-to-use-swagger.md)，了解如何阅读和使用此 API 文档。

    将**管理 API** URL 修改成此格式：_https://yourDigitalTwinsName.yourLocation.azuresmartspaces.net/management/api/v1.0/_。 应用程序将使用此修改过的 URL 作为基 URL 来访问实例。 将此修改过的 URL 复制到一个临时文件。 下一部分将需要此 URL。

    ![管理 API](./media/create-digital-twins-portal/digital-twins-management-api.png)