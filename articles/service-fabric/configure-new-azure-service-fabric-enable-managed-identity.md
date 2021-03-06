---
title: 为新的 Service Fabric 群集配置托管标识支持
description: 下面介绍如何在新的 Azure Service Fabric 群集中启用托管标识支持
ms.topic: article
ms.date: 12/09/2019
ms.custom: sfrev
ms.openlocfilehash: 0e35d2192fdcdb294b349105f3f0158564cec86b
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2020
ms.locfileid: "76930464"
---
# <a name="configure-managed-identity-support-for-a-new-service-fabric-cluster-preview"></a>为新的 Service Fabric 群集配置托管标识支持（预览）

若要在 Service Fabric 应用程序中对[Azure 资源使用托管标识](../active-directory/managed-identities-azure-resources/overview.md)，请首先在群集上启用*托管标识令牌服务*。 此服务负责使用其托管标识对 Service Fabric 应用程序进行身份验证，并负责代表用户获取访问令牌。 启用该服务后，可以在左窗格中的 "**系统**" 部分下的 "Service Fabric Explorer" 下查看该服务，并在其他系统服务旁的 name **Fabric：/system/ManagedIdentityTokenService**下运行。

> [!NOTE]
> 启用**托管标识令牌服务**需要 Service Fabric 运行时版本6.5.658.9590 或更高版本。  

## <a name="enable-the-managed-identity-token-service"></a>启用托管标识令牌服务

若要在创建群集时启用托管标识令牌服务，请将以下代码片段添加到群集 Azure 资源管理器模板：

```json
"fabricSettings": [
    {
        "name": "ManagedIdentityTokenService",
        "parameters": [
            {
                "name": "IsEnabled",
                "value": "true"
            }
        ]
    }
]
```

## <a name="errors"></a>错误

如果部署失败并出现此消息，则表示群集未处于所需的 Service Fabric 版本（支持的最低运行时为 6.5 CU2）：


```json
{
    "code": "ParameterNotAllowed",
    "message": "Section 'ManagedIdentityTokenService' and Parameter 'IsEnabled' is not allowed."
}
```

## <a name="related-articles"></a>相关文章

* 查看 Azure 中的[托管标识支持](./concepts-managed-identity.md)Service Fabric

* [在现有 Azure Service Fabric 群集中启用托管标识支持](./configure-existing-cluster-enable-managed-identity-token-service.md)

## <a name="next-steps"></a>后续步骤

* [使用系统分配的托管标识部署 Azure Service Fabric 应用程序](./how-to-deploy-service-fabric-application-system-assigned-managed-identity.md)
* [使用用户分配的托管标识部署 Azure Service Fabric 应用程序](./how-to-deploy-service-fabric-application-user-assigned-managed-identity.md)
* [利用服务代码中 Service Fabric 应用程序的托管标识](./how-to-managed-identity-service-fabric-app-code.md)
* [向 Azure Service Fabric 应用程序授予其他 Azure 资源的访问权限](./how-to-grant-access-other-resources.md)