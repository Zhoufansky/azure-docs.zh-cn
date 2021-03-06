---
title: Azure VMware 解决方案（AVS）-VPN 网关
description: 了解 AVS 站点到站点 VPN 和点到站点 VPN 概念
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 73171e2c46bdf6c934db5777efe36ba51153a686
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024851"
---
# <a name="vpn-gateways-overview"></a>VPN 网关概述

VPN 网关用于在本地位置的 AVS 区域网络或通过公共 internet 的计算机之间发送加密流量。 每个区域可以有一个 VPN 网关，该网关可以支持多个连接。 与同一个 VPN 网关建立多个连接时，所有 VPN 隧道共享可用的网关带宽。

AVS 提供两种类型的 VPN 网关：

* 站点到站点 VPN 网关
* 点到站点 VPN 网关

## <a name="site-to-site-vpn-gateway"></a>站点到站点 VPN 网关

站点到站点 VPN 网关用于在 AVS 区域网络和本地数据中心之间发送加密流量。 使用此连接可以定义本地网络与 AVS 区域网络之间的网络流量的子网/CIDR 范围。

VPN 网关允许在你的 AVS 私有云上使用本地服务，以及从本地网络使用 AVS 私有云上的服务。 AVS 提供基于策略的 VPN 服务器，用于从本地网络建立连接。

站点到站点 VPN 的用例：

* 本地网络中任何工作站的 AVS 私有云 vCenter 的可访问性。
* 使用本地 Active Directory 作为 vCenter 标识源。
* 方便地将 VM 模板、Iso 和其他文件从本地资源传输到 AVS 私有云 vCenter。
* 你的本地网络中的 AVS 私有云上运行的工作负荷的可访问性。

![站点到站点 VPN 连接拓扑](media/cloudsimple-site-to-site-vpn-connection.png)

### <a name="cryptographic-parameters"></a>加密参数

站点到站点 VPN 连接使用以下默认加密参数建立安全连接。 当你从本地 VPN 设备创建连接时，请使用你的本地 VPN 网关支持的以下任何参数。

#### <a name="phase-1-proposals"></a>阶段1提议

| 参数 | 建议1 | 建议2 | 建议3 |
|-----------|------------|------------|------------|
| SDK 版本 | IKEv1 | IKEv1 | IKEv1 |
| 加密 | AES 128 | AES 256 | AES 256 |
| 哈希算法| SHA 256 | SHA 256 | SHA 1 |
| Diffie-hellman 组（DH 组） | 2 | 2 | 2 |
| 生命时间 | 28,800 秒 | 28,800 秒 | 28,800 秒 |
| 数据大小 | 4GB | 4GB | 4GB |

#### <a name="phase-2-proposals"></a>阶段2方案

| 参数 | 建议1 | 建议2 | 建议3 |
|-----------|------------|------------|------------|
| 加密 | AES 128 | AES 256 | AES 256 |
| 哈希算法| SHA 256 | SHA 256 | SHA 1 |
| 完全向前保密组（PFS 组） | 无 | 无 | 无 |
| 生命时间 | 1800秒 | 1800秒 | 1800秒 |
| 数据大小 | 4GB | 4GB | 4GB |


> [!IMPORTANT]
> 在 VPN 设备上将 TCP MSS 钳位设置为1200。 或者，如果 VPN 设备不支持 MSS 钳位，还可以改为将隧道接口上的 MTU 设置为1240字节。

## <a name="point-to-site-vpn-gateway"></a>点到站点 VPN 网关

点到站点 VPN 用于在 AVS 区域网络和客户端计算机之间发送加密的流量。 点到站点 VPN 是访问 AVS 私有云网络（包括 AVS 私有云 vCenter 和工作负荷 Vm）的最简单方法。 如果要远程连接到 AVS 私有云，请使用点到站点 VPN 连接。

## <a name="next-steps"></a>后续步骤

* [设置 VPN 网关](vpn-gateway.md)
