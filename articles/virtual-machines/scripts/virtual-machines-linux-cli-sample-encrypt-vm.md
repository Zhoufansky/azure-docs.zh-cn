---
title: Azure CLI Script 示例 - 加密 Linux VM
description: Azure CLI Script 示例 - 加密 Linux VM
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/15/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 26d453adb4ded59de162fda0ae62322399fa4d9a
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2019
ms.locfileid: "74040276"
---
# <a name="encrypt-a-linux-virtual-machine-in-azure"></a>加密 Azure 中的 Linux 虚拟机

此脚本创建安全的 Azure Key Vault、加密密钥、Azure Active Directory 服务主体和 Linux 虚拟机 (VM)。 然后使用来自 Key Vault 和服务主体凭据的加密密钥对 VM 进行加密。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/encrypt-disks/encrypt_vm.sh "Encrypt VM disks")]

## <a name="clean-up-deployment"></a>清理部署 

运行以下命令来删除资源组、VM 和所有相关资源。

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令创建资源组、Azure Key Vault、服务主体、虚拟机和所有相关资源。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 说明 |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group) | 创建用于存储所有资源的资源组。 |
| [az keyvault create](https://docs.microsoft.com/cli/azure/keyvault) | 创建 Azure Key Vault 来存储安全的数据，例如加密密钥。 |
| [az keyvault key create](https://docs.microsoft.com/cli/azure/keyvault/key) | 在 Key Vault 中创建加密密钥。 |
| [az ad sp create-for-rbac](https://docs.microsoft.com/cli/azure/ad/sp) | 创建 Azure Active Directory 服务主体来安全地进行身份验证并控制对加密密钥的访问。 |
| [az keyvault set-policy](https://docs.microsoft.com/cli/azure/keyvault) | 设置对 Key Vault 的权限来向服务主体授予对加密密钥的访问权限。 |
| [az vm create](https://docs.microsoft.com/cli/azure/vm) | 创建虚拟机并将其连接到网卡、虚拟网络、子网和 NSG。 此命令还指定要使用的虚拟机映像和管理凭据。  |
| [az vm encryption enable](https://docs.microsoft.com/cli/azure/vm/encryption) | 使用服务主体凭据和加密密钥在 VM 上启用加密。 |
| [az vm encryption show](https://docs.microsoft.com/cli/azure/vm/encryption) | 显示 VM 加密过程的状态。 |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension) | 删除资源组，包括所有嵌套的资源。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](https://docs.microsoft.com/cli/azure)。

可以在 [Azure Linux VM 文档](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)中找到其他虚拟机 CLI 脚本示例。
