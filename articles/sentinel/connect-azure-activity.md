---
title: 将 Azure 活动数据连接到 Azure Sentinel |Microsoft Docs
description: 了解如何将 Azure 活动数据连接到 Azure Sentinel。
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 8c25baa8-b93b-41da-9e6c-15bb7b5c5511
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: 28d5acd80deef193c7d2fea46f682f26abd756aa
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/25/2020
ms.locfileid: "77588597"
---
# <a name="connect-data-from-azure-activity-log"></a>连接 Azure 活动日志中的数据



只需要单击一次即可将日志从[Azure 活动日志](../azure-monitor/platform/platform-logs-overview.md)流式传输到 azure Sentinel。 活动日志是一种订阅日志，可用于深入了解 Azure 中发生的订阅级别事件。 这包括从 Azure 资源管理器操作数据到服务运行状况事件更新的一系列数据。 使用活动日志，可以确定针对订阅中的资源执行的任何写入操作（PUT、POST、DELETE）的 "操作内容、操作人员和操作时间"。 还可以了解该操作和其他相关属性的状态。 活动日志不包括读取（GET）操作或针对使用经典/"RDFE" 模型的资源的操作。 


## <a name="prerequisites"></a>必备条件

- 具有参与者权限的用户 Log Analytics 工作区 


## <a name="connect-to-azure-activity-log"></a>连接到 Azure 活动日志

1. 在 Azure Sentinel 中，选择 "**数据连接器**"，然后单击 " **Azure 活动日志**" 磁贴。

2. 在 "Azure 活动日志" 窗格中，选择要流式传输到 Azure Sentinel 的订阅。 

3. 单击“连接”。

4. 若要在 Azure 活动警报的 Log Analytics 中使用相关架构，请搜索**AzureActivity**。


 

## <a name="next-steps"></a>后续步骤
本文档介绍了如何将 Azure 活动日志连接到 Azure Sentinel。 要详细了解 Azure Sentinel，请参阅以下文章：
- 了解如何[洞悉数据和潜在威胁](quickstart-get-visibility.md)。
- 开始[使用 Azure Sentinel 检测威胁](tutorial-detect-threats-built-in.md)。
