---
title: Azure Database for PostgreSQL 中的性能建议
description: 本文介绍用于 PostgreSQL 的 Azure 数据库中的性能建议功能。
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/28/2018
ms.openlocfilehash: c5324618eeda90b4ef1a512385fb2f14bf391215
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "60564423"
---
# <a name="performance-recommendations-in-azure-database-for-postgresql"></a>Azure Database for PostgreSQL 中的性能建议

**适用于：** Azure Database for PostgreSQL 9.6 和 10

性能建议功能分析数据库以创建自定义的建议以提高性能。 若要生成的建议，分析探讨各种数据库特征包括架构。 启用[Query Store](concepts-query-store.md)以充分利用性能建议功能在服务器上。 实现任何性能建议之后, 您应该测试性能以评估这些更改的影响。 

## <a name="permissions"></a>权限
使用性能建议功能运行分析所需的“所有者”或“参与者”权限。

## <a name="performance-recommendations"></a>性能建议
[性能建议](concepts-performance-recommendations.md)功能跨服务器分析工作负载以标识可能会提高性能的索引。

打开**性能建议**从**智能性能**PostgreSQL 服务器在 Azure 门户页上的菜单栏区域。

![性能建议登陆页面](./media/concepts-performance-recommendations/performance-recommendations-page.png)

选择**分析**，然后选择一个数据库，它将开始分析。 根据工作负荷，th 分析可能需要几分钟才能完成。 分析完成后，门户中将出现通知。 分析执行深入检查您的数据库。 我们建议在非高峰期执行分析。 

**建议**是否发现了任何窗口将显示的建议的列表。

![性能建议新页](./media/concepts-performance-recommendations/performance-recommendations-result.png)

不会自动应用建议。 若要应用建议，请复制查询文本，并从所选的客户端运行。 请记住要测试和监视来评估该建议。 

## <a name="recommendation-types"></a>建议类型

目前，支持两种类型的建议：*创建索引*并*删除索引*。

### <a name="create-index-recommendations"></a>创建索引建议
*创建索引*建议会推荐新的索引来提高工作负荷中的最大频率耗时或运行查询速度。 此建议类型需要[Query Store](concepts-query-store.md)才可用。 查询存储收集查询的信息，并提供详细的查询运行时和频率的统计信息分析用来使该建议。

### <a name="drop-index-recommendations"></a>删除索引建议
除了检测缺少的索引，Azure Database for PostgreSQL 分析现有索引的性能。 如果索引很少使用或冗余，分析器会建议删除它。


## <a name="next-steps"></a>后续步骤
- 了解有关如何在 Azure Database for PostgreSQL 中进行[监视和优化](concepts-monitoring.md)的详细信息。

