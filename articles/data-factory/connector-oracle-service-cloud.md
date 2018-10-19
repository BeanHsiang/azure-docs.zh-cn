---
title: 使用 Azure 数据工厂（预览版）从 Oracle 服务云复制数据 | Microsoft Docs
description: 了解如何通过在 Azure 数据工厂管道中使用复制活动，将数据从 Oracle 服务云复制到支持的接收器数据存储。
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/07/2018
ms.author: jingwang
ms.openlocfilehash: a576b94881e114a97e58bf93515e372221da3346
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/07/2018
ms.locfileid: "44095910"
---
# <a name="copy-data-from-oracle-service-cloud-using-azure-data-factory-preview"></a>使用 Azure 数据工厂（预览版）从 Oracle 服务云复制数据

本文概述了如何使用 Azure 数据工厂中的复制活动从 Oracle 服务云复制数据。 它是基于概述复制活动总体的[复制活动概述](copy-activity-overview.md)一文。

> [!IMPORTANT]
> 此连接器目前提供预览版。 可以进行试用并提供反馈。 若要在解决方案中使用预览版连接器的依赖项，请联系 [Azure 支持部门](https://azure.microsoft.com/support/)。

## <a name="supported-capabilities"></a>支持的功能

可以将数据从 Oracle 服务云复制到任何支持的接收器数据存储。 有关复制活动支持作为源/接收器的数据存储列表，请参阅[支持的数据存储](copy-activity-overview.md#supported-data-stores-and-formats)表。

Azure 数据工厂提供内置的驱动程序用于启用连接，因此无需使用此连接器手动安装任何驱动程序。

## <a name="getting-started"></a>入门

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

对于特定于 Oracle 服务云连接器的数据工厂实体，以下部分提供有关用于定义这些实体的属性详细信息。

## <a name="linked-service-properties"></a>链接服务属性

Oracle 服务云链接服务支持以下属性：

| 属性 | 说明 | 必选 |
|:--- |:--- |:--- |
| type | type 属性必须设置为：OracleServiceCloud | 是 |
| host | Oracle 服务云实例的 URL。  | 是 |
| username | 用于访问 Oracle 服务云服务器的用户名。  | 是 |
| password | 在用户名键中提供的用户名所对应的密码。 可以选择将此字段标记为 SecureString 以将其安全地存储在 ADF，或在 Azure Key Vault 中存储密码，并允许在此处 ADF 复制活动请求时执行数据复制 - 在[在 Key Vault 中存储凭据](store-credentials-in-key-vault.md)中了解详细信息。 | 是 |
| useEncryptedEndpoints | 指定是否使用 HTTPS 加密数据源终结点。 默认值为 true。  | 否 |
| useHostVerification | 指定通过 SSL 连接时是否需要服务器证书中的主机名匹配服务器的主机名。 默认值为 true。  | 否 |
| usePeerVerification | 指定通过 SSL 连接时是否要验证服务器的标识。 默认值为 true。  | 否 |

**示例：**

```json
{
    "name": "OracleServiceCloudLinkedService",
    "properties": {
        "type": "OracleServiceCloud",
        "typeProperties": {
            "host" : "<host>",
            "username" : "<username>",
            "password": {
                 "type": "SecureString",
                 "value": "<password>"
            },
            "useEncryptedEndpoints" : true,
            "useHostVerification" : true,
            "usePeerVerification" : true,
        }
    }
}

```

## <a name="dataset-properties"></a>数据集属性

有关可用于定义数据集的各部分和属性的完整列表，请参阅[数据集](concepts-datasets-linked-services.md)一文。 本部分提供 Oracle 服务云数据集支持的属性列表。

若要从/向 Oracle 服务云复制数据，请将数据集的 type 属性设置为“OracleServiceCloudObject”。 此类型的数据集中没有任何其他特定于类型的属性。

**示例**

```json
{
    "name": "OracleServiceCloudDataset",
    "properties": {
        "type": "OracleServiceCloudObject",
        "linkedServiceName": {
            "referenceName": "<OracleServiceCloud linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}

```

## <a name="copy-activity-properties"></a>复制活动属性

有关可用于定义活动的各部分和属性的完整列表，请参阅[管道](concepts-pipelines-activities.md)一文。 本部分提供 Oracle 服务云源支持的属性列表。

### <a name="oracle-service-cloud-as-source"></a>作为源的 Oracle 服务云

若要从 Oracle 服务云复制数据，请将复制活动中的源类型设置为“OracleServiceCloudSource”。 复制活动**源**部分支持以下属性：

| 属性 | 说明 | 必选 |
|:--- |:--- |:--- |
| type | 复制活动源的 type 属性必须设置为：OracleServiceCloudSource | 是 |
| query | 使用自定义 SQL 查询读取数据。 例如：`"SELECT * FROM MyTable"`。 | 是 |

**示例：**

```json
"activities":[
    {
        "name": "CopyFromOracleServiceCloud",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<OracleServiceCloud input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "OracleServiceCloudSource",
                "query": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>后续步骤
有关 Azure 数据工厂中复制活动支持作为源和接收器的数据存储的列表，请参阅[支持的数据存储](copy-activity-overview.md#supported-data-stores-and-formats)。