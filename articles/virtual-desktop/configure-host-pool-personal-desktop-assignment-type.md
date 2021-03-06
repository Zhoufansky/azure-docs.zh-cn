---
title: Windows 虚拟桌面个人桌面分配类型-Azure
description: 如何为 Windows 虚拟桌面个人桌面主机池配置分配类型。
services: virtual-desktop
author: HeidiLohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 12/10/2019
ms.author: helohr
ms.openlocfilehash: 8065be841b18fa0f8706a3bea3739ee2cb013323
ms.sourcegitcommit: f97f086936f2c53f439e12ccace066fca53e8dc3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/15/2020
ms.locfileid: "77367588"
---
# <a name="configure-the-personal-desktop-host-pool-assignment-type"></a>配置个人桌面主机池分配类型

你可以配置你的个人桌面主机池的分配类型以调整你的 Windows 虚拟桌面环境，从而更好地满足你的需求。 在本主题中，我们将向你展示如何为用户配置自动或直接分配。

>[!NOTE]
> 本文中的说明仅适用于个人桌面主机池，而不是汇集主机池，因为未将池主机池中的用户分配给特定的会话主机。

## <a name="configure-automatic-assignment"></a>配置自动分配

自动分配是在 Windows 虚拟桌面环境中创建的新个人桌面主机池的默认分配类型。 自动分配用户不需要特定的会话主机。

若要自动分配用户，请先将其分配给个人桌面主机池，以便他们可以在源中看到桌面。 当已分配的用户在其源中启动桌面时，它们将声明一个可用的会话主机（如果尚未连接到主机池），这会完成分配过程。

在开始之前，请[下载并导入 Windows 虚拟桌面 PowerShell 模块](/powershell/windows-virtual-desktop/overview/)（如果尚未这样做）。 

> [!NOTE]
> 请确保已安装 Windows 虚拟桌面 PowerShell 模块版本1.0.1534.2001 或更高版本，然后再按照这些说明进行操作。

然后，运行以下 cmdlet 登录到你的帐户：

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

若要将主机池配置为自动将用户分配到 Vm，请运行以下 PowerShell cmdlet：

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -AssignmentType Automatic
```

若要将用户分配到个人桌面主机池，请运行以下 PowerShell cmdlet：

```powershell
Add-RdsAppGroupUser <tenantname> <hostpoolname> "Desktop Application Group" -UserPrincipalName <userupn>
```

## <a name="configure-direct-assignment"></a>配置直接分配

与自动分配不同，使用直接分配时，必须将用户分配到个人桌面主机池和特定的会话主机，然后才能连接到其个人桌面。 如果仅将用户分配到不具有会话主机分配的主机池，则这些用户将无法访问资源。

若要将主机池配置为需要直接将用户分配到会话主机，请运行以下 PowerShell cmdlet：

```powershell
Set-RdsHostPool <tenantname> <hostpoolname> -AssignmentType Direct
```

若要将用户分配到个人桌面主机池，请运行以下 PowerShell cmdlet：

```powershell
Add-RdsAppGroupUser <tenantname> <hostpoolname> "Desktop Application Group" -UserPrincipalName <userupn>
```

若要将用户分配到特定的会话主机，请运行以下 PowerShell cmdlet：

```powershell
Set-RdsSessionHost <tenantname> <hostpoolname> -Name <sessionhostname> -AssignedUser <userupn>
```

## <a name="next-steps"></a>后续步骤

配置个人桌面分配类型后，可以登录到 Windows 虚拟桌面客户端，将其作为用户会话的一部分进行测试。 接下来的两个操作方法将告诉你如何使用所选的客户端连接到会话：

- [与 Windows 桌面客户端连接](connect-windows-7-and-10.md)
- [与 web 客户端连接](connect-web.md)
