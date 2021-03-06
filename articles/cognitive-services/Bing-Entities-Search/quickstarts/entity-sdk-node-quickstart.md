---
title: 快速入门：通过适用于 Node.js 的 SDK 发送搜索请求 - 必应实体搜索
titleSuffix: Azure Cognitive Services
description: 使用本快速入门通过适用于 Node.js 的必应实体搜索 SDK 来搜索实体。
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 01/22/2020
ms.author: aahi
ms.openlocfilehash: 6ece3d7979dc80a2c6c576b3ce279d4fb9bc9472
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2020
ms.locfileid: "76716387"
---
# <a name="quickstart-send-a-search-request-with-the-bing-entity-search-sdk-for-nodejs"></a>快速入门：通过适用于 Node.js 的必应实体搜索 SDK 来发送搜索请求

使用本快速入门开始通过适用于 Node.js 的必应实体搜索 SDK 来搜索实体。 虽然必应实体搜索具有与大多数编程语言兼容的 REST API，但该 SDK 提供了一种简单方法来将服务集成到应用程序中。 可以在 [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/entitySearch.js) 上找到此示例的源代码。

## <a name="prerequisites"></a>必备条件

* 最新版本的 [Node.js](https://nodejs.org/en/download/)。

* [适用于 Node.js 的必应实体搜索 SDK](https://www.npmjs.com/package/@azure/cognitiveservices-entitysearch)

若要安装必应实体搜索 SDK，请执行以下操作：

1. 在开发环境中运行 `npm install ms-rest-azure`。
2. 在开发环境中运行 `npm install @azure/cognitiveservices-entitysearch`。

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>创建并初始化应用程序

1. 在你喜欢使用的 IDE 或编辑器中创建新的 JavaScript 文件，然后添加以下要求。

    ```javascript
    const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
    const EntitySearchAPIClient = require('@azure/cognitiveservices-entitysearch');
    ```

2. 使用你的订阅密钥创建 `CognitiveServicesCredentials` 的实例。 然后，使用该实例创建搜索客户端的实例。

    ```javascript
    let credentials = new CognitiveServicesCredentials('YOUR-ACCESS-KEY');
    let entitySearchApiClient = new EntitySearchAPIClient(credentials);
    ```

## <a name="send-a-request-and-receive-a-response"></a>发送请求并接收响应

1. 通过 `entitiesOperations.search()` 发送实体搜索请求。 在收到响应后，输出 `queryContext`、返回的结果数以及第一个结果的说明。

    ```javascript
    entitySearchApiClient.entitiesOperations.search('seahawks').then((result) => {
        console.log(result.queryContext);
        console.log(result.entities.value);
        console.log(result.entities.value[0].description);
    }).catch((err) => {
        throw err;
    });
    ```

<!-- Removing until we can replace with a sanitized version.
![Entity results](media/entity-search-sdk-node-quickstart-results.png)
-->

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [创建单页 Web 应用](../tutorial-bing-entities-search-single-page-app.md)

* [什么是必应实体搜索 API？](../overview.md )