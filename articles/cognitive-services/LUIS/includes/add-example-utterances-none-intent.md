---
title: include 文件
description: include 文件
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.custom: include file
ms.date: 02/14/2020
ms.subservice: language-understanding
ms.author: diberry
ms.openlocfilehash: 956aa308bf1cb3736c491031239661ec6b295ddb
ms.sourcegitcommit: 79cbd20a86cd6f516acc3912d973aef7bf8c66e4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/14/2020
ms.locfileid: "77279541"
---
客户端应用程序需要知道某个话语是否对应用程序没有意义或不合适。 在创建过程中，**None** 意向会添加到每个应用程序，以确定客户端应用程序是否不应回答某个言语。

如果 LUIS 对于某个话语返回“None”  意向，客户端应用程序可以询问用户是否要结束聊天或提供更多指示以继续聊天。

如果将“无”  意向留空，则将在现有主题域意向之一中预测应在主题域之外预测的言语。 因此，客户端应用程序（如聊天机器人）将基于错误的预测执行错误的操作。

1. 在左侧面板中选择“意向”。 

1. 选择“None”意向。  添加用户可能会输入，但与披萨订购应用无关的 3 个言语：

    |`None` 示例言语|
    |--|
    |`Barking dogs are annoying`|
    |`Penguins in the ocean`|

    这些示例不应使用预期在主题域中出现的字词，例如 `pizza`、`cheese`、`crust`、`pickup` `deliver`。