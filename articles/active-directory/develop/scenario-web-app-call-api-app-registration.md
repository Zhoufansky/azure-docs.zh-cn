---
title: 注册一个用于调用 web Api 的 web 应用-Microsoft 标识平台 |Microsoft
description: 了解如何注册用于调用 web Api 的 web 应用
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 5a57fcef3569734964bf6e8a41faa49800798f9b
ms.sourcegitcommit: b5d646969d7b665539beb18ed0dc6df87b7ba83d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/26/2020
ms.locfileid: "76759051"
---
# <a name="a-web-app-that-calls-web-apis-app-registration"></a>用于调用 web Api 的 web 应用：应用注册

调用 web Api 的 web 应用与登录用户的 web 应用具有相同的注册。 因此，请按照[web 应用中登录用户的说明进行操作：应用注册](scenario-web-app-sign-user-app-registration.md)。

但是，因为 web 应用现在还调用 web Api，所以它将成为机密客户端应用程序。 这就是为什么需要额外注册的原因。 应用必须与 Microsoft 标识平台共享客户端凭据或*机密*。

[!INCLUDE [Registration of client secrets](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="api-permissions"></a>API 权限

Web 应用代表已登录用户调用 Api。 为此，它们必须请求*委托的权限*。 有关详细信息，请参阅[添加访问 Web api 的权限](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis)。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [用于调用 web Api 的 web 应用：代码配置](scenario-web-app-call-api-app-configuration.md)
