---
title: 模板中的输出
description: 介绍如何在 Azure 资源管理器模板中定义输出值。
ms.topic: conceptual
ms.date: 02/25/2020
ms.openlocfilehash: ec96b45cdc5ccf488d46c2d8da03caf16d002dfa
ms.sourcegitcommit: 5a71ec1a28da2d6ede03b3128126e0531ce4387d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/26/2020
ms.locfileid: "77622837"
---
# <a name="outputs-in-azure-resource-manager-template"></a>Azure 资源管理器模板中的输出

本文介绍如何在 Azure 资源管理器模板中定义输出值。 需要从部署的资源返回值时，可以使用输出。

## <a name="define-output-values"></a>定义输出值

以下示例演示如何返回公共 IP 地址的资源 ID：

```json
"outputs": {
  "resourceID": {
    "type": "string",
    "value": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_name'))]"
  }
}
```

## <a name="conditional-output"></a>条件输出

在 "输出" 部分中，您可以有条件地返回值。 通常，在有[条件地部署](conditional-resource-deployment.md)资源时，将在输出中使用条件。 下面的示例演示如何根据是否部署了新的 IP 地址，有条件地返回该 IP 地址的资源 ID：

```json
"outputs": {
  "resourceID": {
    "condition": "[equals(parameters('publicIpNewOrExisting'), 'new')]",
    "type": "string",
    "value": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_name'))]"
  }
}
```

有关条件输出的简单示例，请参阅[条件输出模板](https://github.com/bmoore-msft/AzureRM-Samples/blob/master/conditional-output/azuredeploy.json)。

## <a name="dynamic-number-of-outputs"></a>动态输出数量

在某些情况下，在创建模板时，您不知道需要返回的值的实例数。 您可以使用**copy**元素返回值的可变数量。

```json
"outputs": {
  "storageEndpoints": {
    "type": "array",
    "copy": {
      "count": "[parameters('storageCount')]",
      "input": "[reference(concat(copyIndex(), variables('baseName'))).primaryEndpoints.blob]"
    }
  }
}
```

有关详细信息，请参阅[在 Azure 资源管理器模板中输出迭代](copy-outputs.md)。

## <a name="linked-templates"></a>链接模板

若要从链接模板中检索输出值，请使用父模板中的[reference](template-functions-resource.md#reference)函数。 父模板中的语法为：

```json
"[reference('<deploymentName>').outputs.<propertyName>.value]"
```

从链接模板获取输出属性时，属性名称不能包含短划线。

下面的示例演示如何通过从链接模板中检索值来设置负载均衡器上的 IP 地址。

```json
"publicIPAddress": {
  "id": "[reference('linkedTemplate').outputs.resourceID.value]"
}
```

不能在`reference`嵌套模板[的 outputs 节中使用 ](linked-templates.md#nested-template) 函数。 若要返回嵌套模板中部署的资源的值，请将嵌套模板转换为链接模板。

## <a name="get-output-values"></a>获取输出值

部署成功后，会在部署结果中自动返回输出值。

若要从部署历史记录中获取输出值，可以使用脚本。

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
(Get-AzResourceGroupDeployment `
  -ResourceGroupName <resource-group-name> `
  -Name <deployment-name>).Outputs.resourceID.value
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az group deployment show \
  -g <resource-group-name> \
  -n <deployment-name> \
  --query properties.outputs.resourceID.value
```

---

## <a name="example-templates"></a>示例模板

下面的示例演示使用输出的方案。

|模板  |说明  |
|---------|---------|
|[复制变量](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copyvariables.json) | 创建复杂变量，并输出这些值。 不部署任何资源。 |
|[公共 IP 地址](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip.json) | 创建公共 IP 地址并输出资源 ID。 |
|[负载均衡器](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip-parentloadbalancer.json) | 链接到前面的模板。 创建负载均衡器时，请使用输出中的资源 ID。 |

## <a name="next-steps"></a>后续步骤

* 若要了解有关输出的可用属性，请参阅[了解 Azure 资源管理器模板的结构和语法](template-syntax.md)。
