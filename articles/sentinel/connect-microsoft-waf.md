---
title: 将 Web 应用程序防火墙数据连接到 Azure Sentinel
description: 了解如何将 Microsoft web 应用程序防火墙数据连接到 Azure Sentinel。
author: yelevin
manager: rkarlin
ms.assetid: bfa2eca4-abdc-49ce-b11a-0ee229770cdd
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: a5cef16694fa2cfae036152d22cfa4473956fc72
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/25/2020
ms.locfileid: "77588172"
---
# <a name="connect-data-from-microsoft-web-application-firewall"></a>连接来自 Microsoft web 应用程序防火墙的数据



你可以通过 Azure 应用程序网关的 Microsoft web 应用程序防火墙（WAF）对日志进行流式处理。 此 WAF 保护应用程序免受常见 web 漏洞的影响，例如 SQL 注入和跨站点脚本，并允许你自定义规则以减少误报。 按照以下说明将 Microsoft Web 应用程序防火墙日志流式传输到 Azure Sentinel。


## <a name="prerequisites"></a>必备条件

- 现有的应用程序网关资源

## <a name="connect-to-microsoft-web-application-firewall"></a>连接到 Microsoft web 应用程序防火墙

如果已有 Microsoft web 应用程序防火墙，请确保已有一个网关资源。
部署 Microsoft web 应用程序防火墙并获取数据后，可以轻松地将警报数据流式传输到 Azure Sentinel。
    
1. 在 Azure Sentinel 门户中，选择 "**数据连接器**"。
1. 在 "数据连接器" 页上，选择 " **WAF** " 磁贴。
1. 请参阅[应用程序网关资源](https://ms.portal.azure.com/#blade/HubsExtension/BrowseAllResourcesBlade/resourceType/Microsoft.Network%2FapplicationGateways) 并选择你的 WAF。
    1. 选择“诊断设置”。
    1. 选择表下的 " **+ 添加诊断设置**"。
    1. 在 "**诊断设置**" 页中，键入**名称**，然后选择 "**发送到 Log Analytics**"。
    1. 在**Log Analytics 工作区**中，选择 Azure Sentinel 工作区。
    1. 选择要分析的日志类型。 建议使用： ApplicationGatewayAccessLog 和 ApplicationGatewayFirewallLog。
1. 若要在 Microsoft web 应用程序防火墙警报 Log Analytics 中使用相关架构，请搜索**AzureDiagnostics**。

## <a name="next-steps"></a>后续步骤
本文档介绍了如何将 Microsoft web 应用程序防火墙连接到 Azure Sentinel。 要详细了解 Azure Sentinel，请参阅以下文章：
- 了解如何[洞悉数据和潜在威胁](quickstart-get-visibility.md)。
- 开始[使用 Azure Sentinel 检测威胁](tutorial-detect-threats-built-in.md)。
