---
title: 通过 Azure 门户管理 IoT Central | Microsoft Docs
description: 本文介绍如何从 Azure 门户创建和管理 IoT Central 应用程序。
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 02/11/2020
ms.topic: conceptual
manager: philmea
ms.openlocfilehash: 27517c375265b552d2e1dec4d8c167d1bc86549d
ms.sourcegitcommit: b95983c3735233d2163ef2a81d19a67376bfaf15
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/11/2020
ms.locfileid: "77137643"
---
# <a name="manage-iot-central-from-the-azure-portal"></a>通过 Azure 门户管理 IoT Central

[!INCLUDE [iot-central-selector-manage](../../../includes/iot-central-selector-manage.md)]

你可以使用[Azure 门户](https://portal.azure.com)来管理你的应用程序，而不是在[Azure IoT Central 应用程序管理器](https://aka.ms/iotcentral)网站上创建和管理 IoT Central 应用程序。

## <a name="create-iot-central-applications"></a>创建 IoT Central 应用程序

若要创建应用程序，请导航到[Azure 门户](https://ms.portal.azure.com)，然后选择 "**创建资源**"。

在 **"搜索" Marketplace**栏中，键入*IoT Central*：

![管理门户：搜索](media/howto-manage-iot-central-from-portal/image0a1.png)

在搜索结果中选择 " **IoT Central 应用程序**" 磁贴：

![管理门户：搜索结果](media/howto-manage-iot-central-from-portal/image0b1.png)

现在，选择 "**创建**"：

![管理门户：IoT Central 资源](media/howto-manage-iot-central-from-portal/image0c1.png)

填写窗体中的所有字段。 此窗体类似于你填写在[Azure IoT Central 应用程序管理器](https://aka.ms/iotcentral)网站上创建应用程序的窗体。 有关详细信息，请参阅[创建 IoT Central 应用程序](quick-deploy-iot-central.md)快速入门。

![创建 IoT Central 窗体](media/howto-manage-iot-central-from-portal/image6a.png)

**位置**是要在其中创建应用程序的[地理](https://azure.microsoft.com/global-infrastructure/geographies/)位置。 通常，应选择物理上离设备最近的位置以获得最佳性能。 目前，美国、澳大利亚、亚太地区或欧洲提供 Azure IoT Central。  选择一个位置后，之后便不能将应用程序移到其他位置。


填写所有字段后，选择 "**创建**"。

## <a name="manage-existing-iot-central-applications"></a>管理现有 IoT Central 应用程序

如果已有 Azure IoT Central 应用程序，可将其删除或移动到 Azure 门户中的其他订阅或资源组。

> [!NOTE]
> 无法查看 Azure 门户中的免费定价计划创建的应用程序，因为它们不与你的订阅相关联。

若要开始，请在门户中选择 "**所有资源**"。 选择 "**显示隐藏的类型**"，并开始在 "**按名称筛选**" 中键入应用程序的名称以找到它。 然后选择要管理的 IoT Central 应用程序。

若要导航到应用程序，请选择 " **IoT Central 应用程序 URL**：

![管理门户：资源管理](media/howto-manage-iot-central-from-portal/image3.png)

若要将应用程序移动到其他资源组，请选择资源组旁边的 "**更改**"。 在 "**移动资源**" 页上，选择要将此应用程序移动到的资源组：

![管理门户：资源管理](media/howto-manage-iot-central-from-portal/image4a.png)

若要将应用程序移动到其他订阅，请选择订阅旁边的 "**更改**"。 在 "**移动资源**" 页上，选择要将此应用程序移动到的订阅：

![管理门户：资源管理](media/howto-manage-iot-central-from-portal/image5a.png)

## <a name="next-steps"></a>后续步骤

了解如何通过 Azure 门户管理 Azure IoT Central 应用程序后，建议接下来执行以下步骤：

> [!div class="nextstepaction"]
> [管理应用程序](howto-administer.md)