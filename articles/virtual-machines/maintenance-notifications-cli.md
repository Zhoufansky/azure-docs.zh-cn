---
title: 使用 CLI 获取维护通知
description: 查看 Azure 中运行的虚拟机的维护通知，并使用 Azure CLI 启动自助维护。
author: shants123
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 11/19/2019
ms.author: shants
ms.openlocfilehash: 4ad57c1c71a51f948bd405a5487a1e27e36bfff7
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2020
ms.locfileid: "77920886"
---
# <a name="handling-planned-maintenance-notifications-using-the-azure-cli"></a>使用 Azure CLI 处理计划内维护通知

**本文适用于运行 Linux 和 Windows 的虚拟机。**

你可以使用 CLI 来查看何时计划进行[维护](maintenance-notifications.md)的 vm。 可从[az vm get-help 获取](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-get-instance-view)计划内维护信息。
 
仅当有计划内维护时，才会返回维护信息。 

```azurecli-interactive
az vm get-instance-view -n myVM -g myResourceGroup --query instanceView.maintenanceRedeployStatus
```

## <a name="start-maintenance"></a>开始维护

如果 `IsCustomerInitiatedMaintenanceAllowed` 设置为 true，则以下调用将在 VM 上启动维护。

```azurecli-interactive
az vm perform-maintenance -g myResourceGroup -n myVM 
```

## <a name="classic-deployments"></a>经典部署

[!INCLUDE [classic-vm-deprecation](../../includes/classic-vm-deprecation.md)]

如果你仍有通过使用经典部署模型部署的旧 VM，则可以使用 Azure 经典 CLI 查询 VM，并启动维护。

通过键入以下内容确保在正确模式下使用经典 VM：

```
azure config mode asm
```

若要获取名为 myVM 的 VM 维护状态，请键入：

```
azure vm show myVM 
``` 

若要在名为 myVM 的经典 VM 的 myService服务和 myDeployment部署中启动维护，请键入：

```
azure compute virtual-machine initiate-maintenance --service-name myService --name myDeployment --virtual-machine-name myVM
```

## <a name="next-steps"></a>后续步骤

你还可以使用[Azure PowerShell](maintenance-notifications-powershell.md)或[门户](maintenance-notifications-portal.md)处理计划内维护。
