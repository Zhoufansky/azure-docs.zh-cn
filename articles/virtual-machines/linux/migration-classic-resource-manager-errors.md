---
title: 从经典部署模型迁移到 Azure 资源管理器部署模型的过程中出现的常见错误
description: 本文编录了从 Azure 服务管理将 IaaS 资源迁移到 Azure 资源管理器堆栈的过程中出现的最常见错误和缓解措施。
services: virtual-machines-linux
documentationcenter: ''
author: tanmaygore
manager: vashan
editor: ''
tags: azure-resource-manager
ms.assetid: 5bc03a1b-eb1c-438c-83d9-f0e9d61f1b6a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 02/06/2020
ms.author: tagore
ms.openlocfilehash: 7dc60e6514d4f17ae7fe61bfbc043dc1f650b1d4
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/28/2020
ms.locfileid: "77914834"
---
# <a name="common-errors-during-classic-to-azure-resource-manager-migration"></a>从经典部署模型迁移到 Azure 资源管理器部署模型的过程中出现的常见错误

> [!IMPORTANT]
> 目前，大约90% 的 IaaS Vm 正在使用[Azure 资源管理器](https://azure.microsoft.com/features/resource-manager/)。 截至2020年2月28日，经典 Vm 已弃用，并将在2023年3月1日完全停用。 [了解]( https://aka.ms/classicvmretirement)有关此过时的详细信息以及[它对你有何影响](https://docs.microsoft.com/azure/virtual-machines/classic-vm-deprecation#how-does-this-affect-me)。

[!INCLUDE [virtual-machines-common-migration-errors](../../../includes/virtual-machines-common-classic-resource-manager-migration-common-errors.md)]

## <a name="next-steps"></a>后续步骤

* [平台支持的从经典部署模型到 Azure 资源管理器部署模型的 IaaS 资源迁移概述](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [有关平台支持的从经典部署模型到 Azure 资源管理器部署模型的迁移的技术深入探讨](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [规划从经典部署模型到 Azure 资源管理器的 IaaS 资源迁移](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [使用 PowerShell 将 IaaS 资源从经典部署模型迁移到 Azure 资源管理器](../windows/migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [使用 CLI 将 IaaS 资源从经典部署模型迁移到 Azure 资源管理器](migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [用于帮助将 IaaS 资源从经典部署模型迁移到 Azure 资源管理器部署模型的社区工具](../windows/migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [查看有关将 IaaS 资源从经典部署模型迁移到 Azure 资源管理器部署模型的最常见问题](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
