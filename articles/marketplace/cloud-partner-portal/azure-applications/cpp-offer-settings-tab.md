---
title: Azure 应用程序产品/服务设置 |Azure Marketplace
description: 为 Azure 应用程序产品/服务配置产品/服务设置。
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: pabutler
ms.openlocfilehash: c5e00f77ffeec61fb4fee4b4e8eb72b7c7ed1077
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/08/2019
ms.locfileid: "73827539"
---
# <a name="azure-application-offer-settings-tab"></a>Azure 应用程序产品/服务设置选项卡

本文介绍如何配置 Azure 应用程序的产品/服务设置。

此时会打开 " **Azure 应用程序 > 新的产品/服务**" 页，并在 "**产品/服务设置**" 选项卡附加到字段名称后面的星号（*）指示它是必需的。

![套餐标识窗体](./media/azureapp-offer-settings-tab.png)

## <a name="offer-identity-settings"></a>“产品/服务标识”设置

在“产品/服务标识”下，必须为下表中描述的字段提供信息。  

|    字段         |       说明                                                            |
|  ---------       |     ---------------                                                          |
| **产品/服务 ID\***       | 产品/服务的唯一标识符（在发布者配置文件内）。 此标识符将显示在产品 URL 和见解报表中。 它的最大长度为 50 个字符，并且可以使用小写字母数字字符和短划线 (-)。 （标识符不能以短划线结尾。）**注意：** 在产品/服务推出后，不能更改此字段。 <br> 例如，如果 Contoso 发布了产品/服务 ID 为 **sample-container** 的产品/服务，则会为该产品/服务分配 Azure 市场 URL `https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sample-container?tab=Overview`。 |
| **发布服务器 ID\***     | 你的组织在 Azure 市场中的唯一标识符。 你的所有产品/服务都应当与你的发布者 ID 相关联。 保存产品/服务后，无法更改此值。 |
| **名称\***          | 产品/服务的显示名称。 此名称显示在 Azure 市场和云合作伙伴门户中。 该名称不能超过 50 个字符。 建议为你的产品使用可识别的品牌名称。 不要包含组织的名称，除非这是产品的营销方式。 如果你在其他网站和出版物中营销此产品/服务，请确保名称在所有出版物中相同。 |
|  |  |

选择“保存”以保存你的产品/服务设置。

## <a name="next-steps"></a>后续步骤

使用 [SKU](./cpp-skus-tab.md) 选项卡配置你的产品/服务的 SKU。