---
title: 了解 Azure 虚拟机预留实例折扣 | Microsoft Docs
description: 了解如何将 Azure 虚拟机预留实例折扣应用于正在运行的虚拟机。
documentationcenter: ''
author: yashesvi
manager: yashar
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/13/2019
ms.author: banders
ms.openlocfilehash: b112dd881d4b2e87e617111d00bc82c6151d7750
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "60370062"
---
# <a name="how-the-azure-reservation-discount-is-applied-to-virtual-machines"></a>如何将 Azure 预订折扣应用于虚拟机

购买 Azure 虚拟机预留实例后，预留折扣将自动应用于与预留的属性和数量匹配的虚拟机。 预留涵盖虚拟机的计算成本。

预订折扣应用于从 Azure Marketplace 购买了基础 Vm。

有关 SQL 数据库保留容量，请参阅[了解 Azure 预留实例折扣](billing-understand-reservation-charges.md)。

下表说明了购买虚拟机预留实例后的虚拟机成本。 在任何情况下，均按标准费率收取存储和网络费用。

| 虚拟机类型  | 通过虚拟机预留实例收费 |
|-----------------------|--------------------------------------------|
|未安装其他软件的 Linux VM | 预留涵盖 VM 基础结构成本。|
|具有软件费用的 Linux VM（例如，Red Hat） | 预留涵盖基础结构成本。 将收取其他软件费用。|
|未安装其他软件的 Windows VM |预留涵盖基础结构成本。 将收取 Windows 软件费用。|
|安装了其他软件（例如，SQL server）的 Windows VM | 预留涵盖基础结构成本。 将收取 Windows 软件和其他软件费用。|
|具有 [Azure 混合权益](../virtual-machines/windows/hybrid-use-benefit-licensing.md)的 Windows VM | 预留涵盖基础结构成本。 Azure 混合权益涵盖 Windows 软件成本。 其他任何软件都是单独收费的。|

## <a name="how-reservation-discount-is-applied"></a>如何应用预订折扣

预订折扣是"*使用-it-或-丢失-it*"。 因此，如果任何小时内没有匹配的资源，然后您丢失预订数量的该小时。 不能执行将转发未使用保留的小时数。

在关闭资源时，将预订折扣将自动应用于指定范围内的另一个匹配的资源。 如果没有匹配找到资源在指定范围内，则保留的时间为*丢失*。

## <a name="reservation-discount-for-non-windows-vms"></a>预订折扣非 Windows Vm

 Azure 预留折扣按小时应用于正在运行的 VM 实例。 已购买的预订将与正在运行的 VM 所发送的使用情况信息进行匹配以应用预订折扣。 对于可能无法运行整个小时的 VM，将从其他未使用预订的 VM（包括同时运行的 VM）填充预订。 在一个小时结束时，将锁定该小时内的 VM 预留应用程序。 如果 VM 未运行一小时或该小时内同时运行的 VM 未填充该小时的预留，则该小时的预留未充分利用。 下图说明了如何将预订应用于应计费的 VM 使用量。 该说明基于一次预订购买和两个匹配的 VM 实例。

![一个已应用预留和两个匹配的 VM 实例的屏幕截图](./media/billing-reserved-vm-instance-application/billing-reserved-vm-instance-application.png)

1. 保留行的上方是任何使用常规的即用即付费率收费。 对于预留范围内的任何使用量不收取任何费用，因为这些使用量已在购买预留时付费。
2. 在第 1 个小时内，实例 1 运行了 0.75 小时，实例 2 运行了 0.5 小时。 第 1 个小时的总体使用情况为 1.25 小时。 将按即用即付费率收取剩余 0.25 小时的费用。
3. 在第 2 个小时和第 3 个小时内，这两个实例都各运行了 1 小时。 一个实例的费用由预订费用涵盖，按即用即付费率对另一个实例收费。
4. 在第 4 个小时内，实例 1 运行了 0.5 小时，实例 2 运行了 1 小时。 预订费用完全涵盖了实例 1 的费用，并涵盖了实例 2 的 0.5 小时费用。 将按即用即付费率收取剩余 0.5 小时的费用。

要了解 Azure 预留应用情况并在计费使用情况报告中查看该信息，请参阅[了解预留使用情况](billing-understand-reserved-instance-usage-ea.md)。

## <a name="reservation-discount-for-windows-vms"></a>预订折扣适用于 Windows Vm

正在运行 Windows VM 实例时，将应用预留以涵盖基础结构成本。 对 Windows VM 的 VM 基础结构成本应用预订与对非 Windows VM 的 VM 基础结构成本应用预订相同。 将按 vCPU 对 Windows 软件单独收费。 请参阅 [Windows 软件的预留成本](billing-reserved-Instance-windows-software-costs.md)。 可以利用 [Windows Server 的 Azure 混合权益](../virtual-machines/windows/hybrid-use-benefit-licensing.md)来涵盖 Windows 许可费用。

## <a name="discount-can-apply-to-different-sizes"></a>折扣可以将应用于不同的大小

在购买预留 VM 实例时，如果选择“优化对象：实例大小灵活性”，则折扣覆盖范围将取决于所选的 VM 大小。 预订可以应用于同一大小系列组中的虚拟机 (VM) 大小。 有关详细信息，请参阅[预订 VM 实例的虚拟机大小灵活性](../virtual-machines/windows/reserved-vm-instance-size-flexibility.md)。

## <a name="discount-applies-to-matching-servicetype-only"></a>折扣适用于仅匹配服务类型

预订折扣仅适用于虚拟机使用其中`ServiceType`中的值`AdditionalInfo`匹配购买的预订。 预订折扣应用程序将忽略为 Vm 使用的计量和仅在计算`ServiceType`。 知道哪个服务键入您购买的 VM。 你可以在交换非高级存储 VM 保留高级存储预订，或按照相反的方式。

## <a name="classic-vms-and-cloud-services"></a>经典虚拟机和云服务

保留 VM 实例会自动将应用于这两个经典 Vm 和云服务，启用实例大小灵活性。 对于云服务，预订折扣仅适用于计算成本。 预订折扣应用到云服务，使用费用将被拆分为计算费用 （Linux 计量） 和云服务费用 （云服务管理米）。 有关详细信息，请参阅[预订折扣适用于云服务](billing-reserved-instance-windows-software-costs.md#cloud-services-software-meters-not-included-in-reservation-cost)。

## <a name="need-help-contact-us"></a>需要帮助？ 联系我们

如果有疑问或需要帮助，请[创建支持请求](https://go.microsoft.com/fwlink/?linkid=2083458)。

## <a name="next-steps"></a>后续步骤

若要了解有关 Azure 预订的详细信息，请参阅以下文章：

- [什么是 Azure 的保留项？](billing-save-compute-costs-reservations.md)
- [通过 Azure 虚拟机预留实例为虚拟机预付费](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [通过 Azure SQL 数据库保留容量预付 SQL 数据库计算资源费用](../sql-database/sql-database-reserved-capacity.md)
- [管理 Azure 的保留项](billing-manage-reserved-vm-instance.md)
- [了解即用即付订阅的预留使用情况](billing-understand-reserved-instance-usage.md)
- [了解企业合约的预留使用情况](billing-understand-reserved-instance-usage-ea.md)
- [了解 CSP 订阅的预留使用情况](/partner-center/azure-reservations)
- [预留未包含的 Windows 软件成本](billing-reserved-instance-windows-software-costs.md)
