---
title: 利用 Azure PowerShell 加密 Azure 规模集的磁盘
description: 了解如何使用 Azure PowerShell 来加密 Windows 虚拟机规模集中的 VM 实例和附加的磁盘
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.date: 10/15/2019
ms.author: mbaldwin
ms.openlocfilehash: bd7f92c104e06896f4b3c8bb2adef45983cf5d4d
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/19/2020
ms.locfileid: "76278988"
---
# <a name="encrypt-os-and-attached-data-disks-in-a-virtual-machine-scale-set-with-azure-powershell"></a>使用 Azure PowerShell 在虚拟机规模集中加密 OS 和附加的数据磁盘

Azure PowerShell 模块用于从 PowerShell 命令行或脚本创建和管理 Azure 资源。  本文介绍如何使用 Azure PowerShell 来创建和加密虚拟机规模集。 有关将 Azure 磁盘加密应用到虚拟机规模集的详细信息，请参阅[虚拟机规模集的 Azure 磁盘加密](disk-encryption-overview.md)。

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-an-azure-key-vault-enabled-for-disk-encryption"></a>创建支持磁盘加密的 Azure Key Vault

Azure 密钥保管库可以存储能够在应用程序和服务中安全实现的密钥、机密或密码。 使用软件保护将加密密钥存储在 Azure 密钥保管库中，或者，可在已通过 FIPS 140-2 级别 2 标准认证的硬件安全模块 (HSM) 中导入或生成密钥。 这些加密密钥用于加密和解密附加到 VM 的虚拟磁盘。 可以控制这些加密密钥，以及审核对它们的使用。

使用 [New-AzKeyVault](/powershell/module/az.keyvault/new-azkeyvault) 创建一个密钥保管库。 若要将 Key Vault 用于磁盘加密，请设置 *EnabledForDiskEncryption* 参数。 以下示例还会定义资源组名称、Key Vault 名称和位置的变量。 请提供自己的唯一 Key Vault 名称：

```azurepowershell-interactive
$rgName="myResourceGroup"
$vaultName="myuniquekeyvault"
$location = "EastUS"

New-AzResourceGroup -Name $rgName -Location $location
New-AzKeyVault -VaultName $vaultName -ResourceGroupName $rgName -Location $location -EnabledForDiskEncryption
```

### <a name="use-an-existing-key-vault"></a>使用现有的密钥保管库

仅当你要将现有的 Key Vault 用于磁盘加密时，才需要执行此步骤。 如果在上一部分中创建了 Key Vault，请跳过此步骤。

可以使用 [Set-AzKeyVaultAccessPolicy](/powershell/module/az.keyvault/Set-AzKeyVaultAccessPolicy)，在规模集所在的同一订阅和区域中启用现有的 Key Vault 进行磁盘加密。 按如下所示，在 *$vaultName* 变量中定义现有 Key Vault 的名称：


```azurepowershell-interactive
$vaultName="myexistingkeyvault"
Set-AzKeyVaultAccessPolicy -VaultName $vaultName -EnabledForDiskEncryption
```

## <a name="create-a-scale-set"></a>创建规模集

首先，使用 [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential) 设置 VM 实例的管理员用户名和密码：

```azurepowershell-interactive
$cred = Get-Credential
```

现在，使用 [New-AzVmss](/powershell/module/az.compute/new-azvmss) 创建虚拟机规模集。 若要将流量分配到单独的 VM 实例，则还要创建负载均衡器。 负载均衡器包含的规则可在 TCP 端口 80 上分配流量，并允许 TCP 端口 3389 上的远程桌面流量，以及 TCP 端口 5985 上的 PowerShell 远程流量：

```azurepowershell-interactive
$vmssName="myScaleSet"

New-AzVmss `
    -ResourceGroupName $rgName `
    -VMScaleSetName $vmssName `
    -Location $location `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -PublicIpAddressName "myPublicIPAddress" `
    -LoadBalancerName "myLoadBalancer" `
    -UpgradePolicy "Automatic" `
    -Credential $cred
```

## <a name="enable-encryption"></a>启用加密功能

若要在规模集中加密 VM 实例，请先使用 [Get-AzKeyVault](/powershell/module/az.keyvault/Get-AzKeyVault) 获取有关 Key Vault URI 和资源 ID 的某些信息。 然后，可以使用 [Set-AzVmssDiskEncryptionExtension](/powershell/module/az.compute/Set-AzVmssDiskEncryptionExtension) 结合这些变量来启动加密过程：


```azurepowershell-interactive
$diskEncryptionKeyVaultUrl=(Get-AzKeyVault -ResourceGroupName $rgName -Name $vaultName).VaultUri
$keyVaultResourceId=(Get-AzKeyVault -ResourceGroupName $rgName -Name $vaultName).ResourceId

Set-AzVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $vmssName `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId -VolumeType "All"
```

出现提示时，请键入 *y*，以便继续对规模集 VM 实例执行磁盘加密过程。

### <a name="enable-encryption-using-kek-to-wrap-the-key"></a>使用 KEK 启用加密以包装密钥

加密虚拟机规模集时，还可以使用密钥加密密钥增加安全性。

```azurepowershell-interactive
$diskEncryptionKeyVaultUrl=(Get-AzKeyVault -ResourceGroupName $rgName -Name $vaultName).VaultUri
$keyVaultResourceId=(Get-AzKeyVault -ResourceGroupName $rgName -Name $vaultName).ResourceId
$keyEncryptionKeyUrl = (Get-AzKeyVaultKey -VaultName $vaultName -Name $keyEncryptionKeyName).Key.kid;

Set-AzVmssDiskEncryptionExtension -ResourceGroupName $rgName -VMScaleSetName $vmssName `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl -DiskEncryptionKeyVaultId $keyVaultResourceId `
    -KeyEncryptionKeyUrl $keyEncryptionKeyUrl -KeyEncryptionKeyVaultId $keyVaultResourceId -VolumeType "All"
```

> [!NOTE]
>  磁盘加密-keyvault 参数的值语法是完整的标识符字符串：</br>
/subscriptions/[subscription-id-guid]/resourceGroups/[resource-group-name]/providers/Microsoft.KeyVault/vaults/[keyvault-name]</br></br>
> 密钥加密-密钥参数的值语法是 KEK 的完整 URI，如下所示：</br>
https://[keyvault-name].vault.azure.net/keys/[kekname]/[kek-unique-id]

## <a name="check-encryption-progress"></a>查看加密进度

若要检查磁盘加密状态，请使用 [Get-AzVmssDiskEncryption](/powershell/module/az.compute/Get-AzVmssDiskEncryption)：


```azurepowershell-interactive
Get-AzVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $vmssName
```

加密 VM 实例后，*EncryptionSummary* 代码会报告 *ProvisioningState/succeeded*，如以下示例输出所示：

```powershell
ResourceGroupName            : myResourceGroup
VmScaleSetName               : myScaleSet
EncryptionSettings           :
  KeyVaultURL                : https://myuniquekeyvault.vault.azure.net/
  KeyEncryptionKeyURL        :
  KeyVaultResourceId         : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myuniquekeyvault
  KekVaultResourceId         :
  KeyEncryptionAlgorithm     :
  VolumeType                 : All
  EncryptionOperation        : EnableEncryption
EncryptionSummary[0]         :
  Code                       : ProvisioningState/succeeded
  Count                      : 2
EncryptionEnabled            : True
EncryptionExtensionInstalled : True
```

## <a name="disable-encryption"></a>禁用加密功能

如果不再希望使用加密的 VM 实例磁盘，可以使用 [Disable-AzVmssDiskEncryption](/powershell/module/az.compute/Disable-AzVmssDiskEncryption) 禁用加密，如下所示：


```azurepowershell-interactive
Disable-AzVmssDiskEncryption -ResourceGroupName $rgName -VMScaleSetName $vmssName
```

## <a name="next-steps"></a>后续步骤

- 在本文中，我们已使用 Azure PowerShell 加密了虚拟机规模集。 你还可以使用[Azure CLI](disk-encryption-cli.md)或[Azure 资源管理器模板](disk-encryption-azure-resource-manager.md)。
- 如果你想要在设置另一个扩展后应用 Azure 磁盘加密，则可以使用[扩展排序](virtual-machine-scale-sets-extension-sequencing.md)。
