---
title: 管理 Azure 安全中心的安全事件 |Microsoft Docs
description: 本文档可帮助你使用 Azure 安全中心管理安全事件。
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 09/09/2019
ms.author: memildin
ms.openlocfilehash: 1a6dbaeac5355d50edb93a7f215d7f8e88231e98
ms.sourcegitcommit: f15f548aaead27b76f64d73224e8f6a1a0fc2262
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/26/2020
ms.locfileid: "77615972"
---
# <a name="manage-security-incidents-in-azure-security-center"></a>管理 Azure 安全中心的安全事件

会审和调查安全警报对于最有经验的安全分析师来说是非常耗时的，而且很多人都很难知道从何处着手。 通过使用[分析](security-center-detection-capabilities.md)来连接不同的[安全警报](security-center-managing-and-responding-alerts.md)之间的信息，安全中心可以提供攻击活动及其所有相关警报的单一视图，让你快速了解攻击者所采取的操作以及哪些资源受到了影响。

本主题介绍安全中心中的事件，以及如何使用修正其警报。

## <a name="what-is-a-security-incident"></a>什么是安全事件？

在安全中心，安全事件是对资源的所有警报汇总，与 [网络攻击链](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) 模式保持一致。 事件显示在 "[安全警报](security-center-managing-and-responding-alerts.md)" 列表中。 单击事件可查看相关警报，使你可以获取有关每个匹配项的详细信息。

## <a name="managing-security-incidents"></a>管理安全事件

1. 在安全中心仪表板上，单击 "**安全警报**" 磁贴。 将列出事件和警报。 请注意，安全事件描述相比其他警报具有不同的图标。

    ![查看安全事件](./media/security-center-managing-and-responding-alerts/security-center-manage-alerts.png)

1. 若要查看详细信息，请单击某个事件。 "**检测到安全事件**" 边栏选项卡显示更多详细信息。 "**常规信息**" 一节可以深入了解触发安全警报的内容。 如果警报仍处于活动状态，则它会显示目标资源、源 IP 地址（如果适用），以及有关如何修正的建议等信息。  

    ![响应 Azure 安全中心的安全事件](./media/security-center-managing-and-responding-alerts/security-center-alert-incident.png)

1. 若要获取有关每个警报的详细信息，请单击 "警报"。 安全中心提供的修正建议因安全警报而异。

   > [!NOTE]
   > 同一个警报可以作为事件的一部分存在，也可以作为独立的警报显示。

    ![警报详细信息](./media/security-center-incident/security-center-incident-alert.png)

1. 按照为每个警报提供的补救步骤进行操作。

有关警报的详细信息，请[管理和响应安全警报](security-center-managing-and-responding-alerts.md)。

以下主题将指导你根据资源类型来完成不同的警报：

* [IaaS Windows 计算机的警报](threat-protection.md#windows-machines)
* [IaaS Linux 计算机的警报](threat-protection.md#linux-machines)
* [Azure App Service 的警报](threat-protection.md#app-services)
* [Azure 容器警报](threat-protection.md#azure-containers)
* [SQL 数据库和 SQL 数据仓库的警报](threat-protection.md#data-sql)
* [Azure 存储的警报](threat-protection.md#azure-storage)
* [Cosmos DB 的警报](threat-protection.md#cosmos-db)

以下主题介绍安全中心如何使用它从与 Azure 基础结构集成所收集的不同遥测，以便为 Azure 上部署的资源应用其他保护层：

* [Azure 管理层警报（Azure 资源管理器）（预览版）](threat-protection.md#management-layer)
* [Azure Key Vault 的警报（预览）](threat-protection.md#azure-keyvault)
* [Azure 网络层警报](threat-protection.md#network-layer)
* [来自其他服务的警报](threat-protection.md#alerts-other)

## <a name="see-also"></a>另请参阅
在本文档中，已经学习了如何在安全中心使用安全事件功能。 若要了解有关安全中心的详细信息，请参阅以下文章：

* [Azure 安全中心的安全警报](security-center-alerts-overview.md)。
* [管理安全警报](security-center-managing-and-responding-alerts.md)
* [Azure 安全中心规划和操作指南](security-center-planning-and-operations-guide.md)
