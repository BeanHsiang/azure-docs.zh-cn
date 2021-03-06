---
title: Azure Data Lake Storage Gen2 的已知问题 | Microsoft Docs
description: 了解 Azure Data Lake Storage Gen2 的限制和已知问题
services: storage
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 02/28/2019
ms.author: normesta
ms.openlocfilehash: 51230fe050c67253dd5b2bb3f23d03512df64ef6
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2019
ms.locfileid: "64939253"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage Gen2 的已知问题

本文包含 Data Lake Storage Gen2 的已知问题和临时限制。

## <a name="sdk-support-for-data-lake-storage-gen2-accounts"></a>针对 Data Lake Storage Gen2 帐户的 SDK 支持

没有可用于 Data Lake Storage Gen2 帐户的 SDK。

## <a name="blob-storage-apis"></a>Blob 存储 API

Data Lake Storage Gen2 帐户尚不能使用 Blob 存储 API。

这些 API 已禁用，以防止意外出现的数据访问问题，因为 Blob 存储 API 与 Azure Data Lake Gen2 API 目前不可互操作。

如果你在这些 API 被禁用之前使用了它们来加载数据，并且生产环境需要访问该数据，请联系 Microsoft 支持部门并提供以下信息：

* 订阅 ID（GUID，不是名称）

* 存储帐户名称

* 在生产环境中是否频繁受到影响，如果是，针对的是哪些存储帐户？

* 即使在生产环境中未频繁受到影响，也请告诉我们你是否出于某种原因需要将该数据复制到另一个存储帐户，如果是，为什么？

在这些情况下，我们可以在有限的时间段内恢复对 Blob API 的访问权限，以便你可以将此数据复制到未启用分层命名空间功能的存储帐户。

非托管虚拟机 (VM) 磁盘依赖于被禁用的 Blob 存储 API，因此，若要在存储帐户中启用分层命名空间，请考虑将非托管 VM 磁盘放到未启用分层命名空间功能的存储帐户中。

## <a name="api-interoperability"></a>API 互操作性

Blob 存储 API 和 Azure Data Lake Gen2 API 不能互操作。

如果有使用 Blob API 的工具、应用程序、服务或脚本，并且希望使用它们处理上传到帐户的所有内容，则在 Blob API 与 Azure Data Lake Gen2 API 实现互操作之前，请勿在 Blob 存储帐户中启用分层命名空间。 使用存储帐户没有分层命名空间，则意味着您然后不会访问数据湖存储第 2 代特定功能，例如目录和文件系统访问控制列表。

## <a name="azure-storage-explorer"></a>Azure 存储资源管理器

若要使用 Azure 存储资源管理器查看或管理 Data Lake Storage Gen2 帐户，你必须至少具有可[免费下载](https://azure.microsoft.com/features/storage-explorer/)的此工具的版本 `1.6.0`。

请注意，当前未嵌入到 Azure 门户的存储资源管理器的版本不支持查看或管理数据湖存储第 2 代帐户层次结构命名空间启用了功能。

## <a name="blob-viewing-tool"></a>Blob 查看工具

Azure 门户中的 Blob 查看工具仅为 Data Lake Storage Gen2 提供有限的支持。

## <a name="third-party-applications"></a>第三方应用程序

第三方应用程序可能不支持 Data Lake Storage Gen2。

是否支持该服务由每家第三方应用程序提供商决定。 目前，Blob 存储 API 和 Data Lake Storage Gen2 API 不可用于管理相同的内容。 由于我们正在努力实现这种互操作性，有可能许多第三方工具将自动支持 Data Lake Storage Gen2。

## <a name="azcopy-support"></a>AzCopy 支持

AzCopy 版本 8 不支持 Data Lake Storage Gen2。

请改用 AzCopy 的最新预览版 ([AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json))，因为它支持 Data Lake Storage Gen2 终结点。

## <a name="azure-event-grid"></a>Azure 事件网格

[Azure 事件网格](https://azure.microsoft.com/services/event-grid/)不会从 Azure Data Lake Gen2 帐户接收事件，因为这些帐户目前不会生成事件。  

## <a name="soft-delete-and-snapshots"></a>软删除和快照

软删除和快照不适用于 Data Lake Storage Gen2 帐户。

所有版本控制功能（包括[快照](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob)和[软删除](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete)）尚不适用于已启用分层命名空间功能的存储帐户。

## <a name="object-level-storage-tiers"></a>对象级存储层

对象级存储层（“热”、“冷”和“存档”）尚不适用于 Azure Data Lake Storage Gen 2 帐户，但适用于未启用分层命名空间功能的存储帐户。

## <a name="azure-blob-storage-lifecycle-management-policies"></a>Azure Blob 存储生命周期管理策略

Azure Blob 存储生命周期管理策略尚不可用于数据湖存储第 2 代的帐户。

这些策略适用于未启用分层命名空间功能的存储帐户。

## <a name="diagnostic-logs"></a>诊断日志

诊断日志不适用于 Data Lake Storage Gen2 帐户。
