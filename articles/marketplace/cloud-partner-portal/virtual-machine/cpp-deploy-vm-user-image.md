---
title: 从用户 VHD 中部署 Azure VM | Microsoft Docs
description: 介绍如何部署用户 VHD 映像以创建 Azure VM 实例。
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 11/29/2018
ms.author: pbutlerm
ms.openlocfilehash: 9c163ddf7859246fcdaa28edfd4b598a24a32be2
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2018
ms.locfileid: "53195334"
---
# <a name="deploy-an-azure-vm-from-a-user-vhd"></a>从用户 VHD 中部署 Azure VM

本文介绍如何使用提供的 Azure 资源管理器模板和 Azure PowerShell 脚本，部署通用 VHD 映像以创建新的 Azure VM 资源。


## <a name="vhd-deployment-template"></a>VHD 部署模板

将用于 [VHD 部署](cpp-deploy-json-template.md)的 Azure 资源管理器模板复制到名为 `VHDtoImage.json` 的本地文件。  编辑此文件以提供以下参数的值。 

|  **Parameter**             |   **说明**                                                              |
|  -------------             |   ---------------                                                              |
| ResourceGroupName          | 现有 Azure 资源组名称。  通常使用与密钥保管库相关联的同一 RG  |
| TemplateFile               | `VHDtoImage.json` 文件的完整路径                                    |
| userStorageAccountName     | 存储帐户的名称                                                    |
| sNameForPublicIP           | 公共 IP 的 DNS 名称。 必须为小写                                  |
| subscriptionId             | Azure 订阅标识符                                                  |
| 位置                   | 资源组的标准 Azure 地理位置                       |
| vmName                     | 虚拟机名称                                                    |
| vaultName                  | 密钥保管库的名称                                                          |
| vaultResourceGroup         | 密钥保管库的资源组
| certificateUrl             | 证书的 URL，包括存储在密钥保管库中的版本，例如： https://testault.vault.azure.net/secrets/testcert/b621es1db241e56a72d037479xab1r7 |
| vhdUrl                     | 虚拟硬盘的 URL                                                   |
| vmSize                     | 虚拟机实例的大小                                           |
| publicIPAddressName        | 公共 IP 地址的名称                                                  |
| virtualNetworkName         | 虚拟网络的名称                                                    |
| nicName                    | 虚拟网络的网络接口卡的名称                     |
| adminUserName              | 管理员帐户的用户名                                          |
| adminPassword              | 管理员密码                                                          |
|  |  |


## <a name="powershell-script"></a>PowerShell 脚本

复制并编辑以下脚本以提供 `$storageaccount` 和 `$vhdUrl` 变量的值。  执行它，从现有通用 VHD 创建 Azure VM 资源。

```powershell
# storage account of existing generalized VHD 
$storageaccount = "testwinrm11815"

# generalized VHD URL
$vhdUrl = "https://testwinrm11815.blob.core.windows.net/vhds/testvm1234562016651857.vhd"

echo "New-AzureRMResourceGroupDeployment -Name "dplisvvm$postfix" -ResourceGroupName "$rgName" -TemplateFile "C:\certLocation\VHDtoImage.json" -userStorageAccountName "$storageaccount" -dnsNameForPublicIP "$vmName" -subscriptionId "$mysubid" -location "$location" -vmName "$vmName" -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id  -vhdUrl "$vhdUrl" -vmSize "Standard_A2" -publicIPAddressName "myPublicIP1" -virtualNetworkName "myVNET1" -nicName "myNIC1" -adminUserName "isv" -adminPassword $pwd"

#deploying VM with existing VHD
New-AzureRMResourceGroupDeployment -Name "dplisvvm$postfix" -ResourceGroupName "$rgName" -TemplateFile "C:\certLocation\VHDtoImage.json" -userStorageAccountName "$storageaccount" -dnsNameForPublicIP "$vmName" -subscriptionId "$mysubid" -location "$location" -vmName "$vmName" -vaultName "$kvname" -vaultResourceGroup "$rgName" -certificateUrl $objAzureKeyVaultSecret.Id  -vhdUrl "$vhdUrl" -vmSize "Standard_A2" -publicIPAddressName "myPublicIP1" -virtualNetworkName "myVNET1" -nicName "myNIC1" -adminUserName "isv" -adminPassword $pwd 

```

## <a name="next-steps"></a>后续步骤

部署 VM 后，即可[认证 VM 映像](./cpp-certify-vm.md)。