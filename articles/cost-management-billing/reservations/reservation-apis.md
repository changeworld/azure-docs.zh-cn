---
title: 适用于 Azure 预留自动化的 API | Microsoft Docs
description: 了解 Azure API，以便通过它以编程方式获取预留信息。
author: yashesvi
ms.reviewer: yashesvi
tags: billing
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: conceptual
ms.date: 02/13/2020
ms.author: banders
ms.openlocfilehash: 773334787ec7b2706c16e517281d6a60215ad482
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2020
ms.locfileid: "96353011"
---
# <a name="apis-for-azure-reservation-automation"></a>适用于 Azure 预留自动化的 API

使用 Azure API 以编程方式为组织获取有关 Azure 服务或软件预留的信息。

## <a name="find-reservation-plans-to-buy"></a>查找要购买的预留计划

使用预留建议 API 获取根据组织的使用情况购买预留计划的建议。 有关详细信息，请参阅[获取预留建议](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation)。

也可通过“消耗 API 使用情况详细信息”分析资源使用情况。 有关详细信息，请参阅[使用情况详细信息 - 按计费帐户划分的计费期间的列表](/rest/api/consumption/usagedetails/list#billingaccountusagedetailslistforbillingperiod-legacy)。 始终使用的 Azure 资源通常是最适合预留的。

## <a name="buy-a-reservation"></a>购买预留项

可以使用 REST API 以编程方式购买 Azure 预留和软件计划。 若要了解详细信息，请参阅[预留订单 - 购买 API](/rest/api/reserved-vm-instances/reservationorder/purchase)。

以下是使用 REST API 进行购买的示例请求：

```
PUT https://management.azure.com/providers/Microsoft.Capacity/reservationOrders/<GUID>?api-version=2019-04-01
```

请求正文：

```
{
 "sku": {
    "name": "standard_D1"
  },
 "location": "westus",
 "properties": {
    "reservedResourceType": "VirtualMachines",
    "billingScopeId": "/subscriptions/ed3a1871-612d-abcd-a849-c2542a68be83",
    "term": "P1Y",
    "quantity": "1",
    "displayName": "TestReservationOrder",
    "appliedScopes": null,
    "appliedScopeType": "Shared",
    "reservedResourceProperties": {
      "instanceFlexibility": "On"
    }
  }
}
```

也可在 Azure 门户中购买预留。 有关详细信息，请参阅以下文章：

服务计划：
- [虚拟机](../../virtual-machines/prepay-reserved-vm-instances.md?toc=%2fazure%2fbilling%2fTOC.json)
-  [Cosmos DB](../../cosmos-db/cosmos-db-reserved-capacity.md?toc=/azure/billing/TOC.json)
- [SQL 数据库](../../azure-sql/database/reserved-capacity-overview.md?toc=/azure/billing/TOC.json)

软件计划：
- [SUSE Linux 软件](../../virtual-machines/linux/prepay-suse-software-charges.md?toc=/azure/billing/TOC.json)

## <a name="get-reservations"></a>获取预留

如果你是签署了企业协议的 Azure 客户（EA 客户），则可获取组织购买的预留，只需使用[获取预留实例交易费用 API](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-charges) 即可。 对于其他订阅，请获取购买的预留的列表并获取查看权限，方法是使用 API [预留订单 - 列表](/rest/api/reserved-vm-instances/reservationorder/list)。 默认情况下，购买了预订的帐户所有者或人员有权查看预留。

## <a name="see-reservation-usage"></a>查看预留使用情况

如果你是 EA 客户，则可以编程方式查看组织中的预留是如何使用的。 有关详细信息，请参阅[获取企业客户的预留实例使用情况](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage)。 对于其他订阅，请使用 API [预览摘要 - 按预留订单和预留列出](/rest/api/consumption/reservationssummaries/listbyreservationorderandreservation)。

如果发现组织的预留未充分使用，请执行以下操作：

- 确保组织创建的虚拟机数与预留的 VM 大小相匹配。
- 确保已启用实例大小灵活性。 有关详细信息，请参阅[管理预留 - 更改预留 VM 实例的优化设置](manage-reserved-vm-instance.md#change-optimize-setting-for-reserved-vm-instances)。
- 将预留范围更改为共享，使其应用得更广。 有关详细信息，请参阅[管理预留 - 更改预留的范围](manage-reserved-vm-instance.md#change-the-reservation-scope)。
- 交换未使用的数量。 有关详细信息，请参阅[管理预留](manage-reserved-vm-instance.md)。

## <a name="give-access-to-reservations"></a>授予对预留的访问权限

获取用户可以访问的所有预留的列表，方法是使用[预留 - 操作 - 列表 API](/rest/api/reserved-vm-instances/reservationorder/list)。 若要以编程方式授予对预留的访问权限，请参阅以下文章之一：

- [使用 REST API 添加或删除 Azure 角色分配](../../role-based-access-control/role-assignments-rest.md)
- [使用 Azure PowerShell 添加或删除 Azure 角色分配](../../role-based-access-control/role-assignments-powershell.md)
- [使用 Azure CLI 添加或删除 Azure 角色分配](../../role-based-access-control/role-assignments-cli.md)

## <a name="split-or-merge-reservation"></a>拆分或合并预留

购买一个预留中的多个资源实例后，可能会希望将一个预留中的实例分配给其他订阅。 可以更改预留范围，使之适用于同一计费上下文中的所有订阅。 但出于成本管理或预算目的，可能需要将范围保留为“单个订阅”，将预留实例分配给特定的订阅。

若要拆分某个预留，请使用 API [预留 - 拆分](/rest/api/reserved-vm-instances/reservation/split)。 也可使用 PowerShell 来拆分预留。 有关详细信息，请参阅[管理预留 - 将一个预留拆分成两个预留](manage-reserved-vm-instance.md#split-a-single-reservation-into-two-reservations)。

若要将两个预留合并成一个预留，请使用 API [预留 - 合并](/rest/api/reserved-vm-instances/reservation/merge)。

## <a name="change-scope-for-a-reservation"></a>更改预留范围

预留范围可以是计费上下文中的单个订阅、单个资源组或所有订阅。 如果将范围设置为单个订阅或单个资源组，预留将与所选订阅中正在运行的资源进行匹配。 如果删除或移动订阅或资源组，则不会使用预留。  如果将范围设置为共享，Azure 会将预留与计费上下文内所有订阅中运行的资源匹配。 计费上下文依赖于用于购买预留的订阅。 可以在购买时选择范围，也可以在购买后随时更改范围。 有关详细信息，请参阅[管理预留 - 更改范围](manage-reserved-vm-instance.md#change-the-reservation-scope)。

若要以编程方式更改范围，请使用 API [预留 - 更新](/rest/api/reserved-vm-instances/reservation/update)。

## <a name="learn-more"></a>了解详细信息

- [什么是适用于 Azure 的预留](save-compute-costs-reservations.md)
- [了解 VM 预留折扣的应用方式](../manage/understand-vm-reservation-charges.md)
- [了解如何应用 SUSE Linux Enterprise 软件计划折扣](understand-suse-reservation-charges.md)
- [了解如何应用其他预留折扣](understand-reservation-charges.md)
- [了解即用即付订阅的预留使用情况](understand-reserved-instance-usage.md)
- [了解企业合约的预留使用情况](understand-reserved-instance-usage-ea.md)
- [预留未包含的 Windows 软件成本](reserved-instance-windows-software-costs.md)
- [合作伙伴中心云解决方案提供商 (CSP) 计划中的 Azure 预订](/partner-center/azure-reservations)