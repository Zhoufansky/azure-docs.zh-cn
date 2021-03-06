---
title: Azure HDInsight 管理 IP 地址
description: 了解必须允许哪些 IP 地址的入站流量，以便通过 Azure HDInsight 为虚拟网络正确配置网络安全组和用户定义的路由。
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 03/03/2020
ms.openlocfilehash: 5e8f39b58f258742108fe323d9395efd87bc288f
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2020
ms.locfileid: "78271816"
---
# <a name="hdinsight-management-ip-addresses"></a>HDInsight 管理 IP 地址

> [!Important]
> 在大多数情况下，你现在可以为网络安全组使用[服务标记](hdinsight-service-tags.md)功能，而不是手动添加 IP 地址。 将仅为服务标记添加新区域，并且最终将不推荐使用静态 IP 地址。

如果使用网络安全组（Nsg）或用户定义的路由（Udr）来控制到 HDInsight 群集的入站流量，则必须确保群集可以与关键 Azure 运行状况和管理服务通信。  这些服务的某些 IP 地址是特定于区域的，其中某些地址适用于所有 Azure 区域。 如果不使用自定义 DNS，则可能还需要允许来自 Azure DNS 服务的流量。

以下部分讨论了必须允许的特定 IP 地址。

## <a name="azure-dns-service"></a>Azure DNS 服务

如果你使用的是 Azure 提供的 DNS 服务，则允许从端口53上的__168.63.129.16__访问。 有关详细信息，请参阅 [VM 和角色实例的名称解析](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)文档。 如果使用的是自定义 DNS，请跳过此步骤。

## <a name="health-and-management-services-all-regions"></a>运行状况和管理服务：所有区域

允许来自以下 Azure HDInsight 运行状况和管理服务的 IP 地址的流量，适用于所有 Azure 区域：

| 源 IP 地址 | 目标  | 方向 |
| ---- | ----- | ----- |
| 168.61.49.99 | \*：443 | 入站 |
| 23.99.5.239 | \*：443 | 入站 |
| 168.61.48.131 | \*：443 | 入站 |
| 138.91.141.162 | \*：443 | 入站 |

## <a name="health-and-management-services-specific-regions"></a>运行状况和管理服务：特定区域

允许 Azure HDInsight 运行状况和管理服务在资源所在的特定 Azure 区域中列出的 IP 地址的流量：

> [!IMPORTANT]  
> 如果你使用的 Azure 区域未列出，请使用网络安全组的[服务标记](hdinsight-service-tags.md)功能。

| 国家/地区 | 区域 | 允许的源 IP 地址 | 允许的目标 | 方向 |
| ---- | ---- | ---- | ---- | ----- |
| 亚洲 | 东亚 | 23.102.235.122</br>52.175.38.134 | \*：443 | 入站 |
| &nbsp; | 东南亚 | 13.76.245.160</br>13.76.136.249 | \*：443 | 入站 |
| 澳大利亚 | 澳大利亚东部 | 104.210.84.115</br>13.75.152.195 | \*：443 | 入站 |
| &nbsp; | 澳大利亚东南部 | 13.77.2.56</br>13.77.2.94 | \*：443 | 入站 |
| 巴西 | 巴西南部 | 191.235.84.104</br>191.235.87.113 | \*：443 | 入站 |
| Canada | 加拿大东部 | 52.229.127.96</br>52.229.123.172 | \*：443 | 入站 |
| &nbsp; | 加拿大中部 | 52.228.37.66</br>52.228.45.222 |\*：443 | 入站 |
| 中国 | 中国北部 | 42.159.96.170</br>139.217.2.219</br></br>42.159.198.178</br>42.159.234.157 | \*：443 | 入站 |
| &nbsp; | 中国东部 | 42.159.198.178</br>42.159.234.157</br></br>42.159.96.170</br>139.217.2.219 | \*：443 | 入站 |
| &nbsp; | 中国北部 2 | 40.73.37.141</br>40.73.38.172 | \*：443 | 入站 |
| &nbsp; | 中国东部 2 | 139.217.227.106</br>139.217.228.187 | \*：443 | 入站 |
| 欧洲 | 北欧 | 52.164.210.96</br>13.74.153.132 | \*：443 | 入站 |
| &nbsp; | 西欧| 52.166.243.90</br>52.174.36.244 | \*：443 | 入站 |
| 法国 | 法国中部| 20.188.39.64</br>40.89.157.135 | \*：443 | 入站 |
| 德国 | 德国中部 | 51.4.146.68</br>51.4.146.80 | \*：443 | 入站 |
| &nbsp; | 德国东北部 | 51.5.150.132</br>51.5.144.101 | \*：443 | 入站 |
| 印度 | 印度中部 | 52.172.153.209</br>52.172.152.49 | \*：443 | 入站 |
| &nbsp; | 印度南部 | 104.211.223.67<br/>104.211.216.210 | \*：443 | 入站 |
| 日本 | 日本东部 | 13.78.125.90</br>13.78.89.60 | \*：443 | 入站 |
| &nbsp; | 日本西部 | 40.74.125.69</br>138.91.29.150 | \*：443 | 入站 |
| 韩国 | 韩国中部 | 52.231.39.142</br>52.231.36.209 | \*：443 | 入站 |
| &nbsp; | 韩国南部 | 52.231.203.16</br>52.231.205.214 | \*：443 | 入站
| United Kingdom | 英国西部 | 51.141.13.110</br>51.141.7.20 | \*：443 | 入站 |
| &nbsp; | 英国南部 | 51.140.47.39</br>51.140.52.16 | \*：443 | 入站 |
| United States | 美国中部 | 13.89.171.122</br>13.89.171.124 | \*：443 | 入站 |
| &nbsp; | 美国东部 | 13.82.225.233</br>40.71.175.99 | \*：443 | 入站 |
| &nbsp; | 美国中北部 | 157.56.8.38</br>157.55.213.99 | \*：443 | 入站 |
| &nbsp; | 美国中西部 | 52.161.23.15</br>52.161.10.167 | \*：443 | 入站 |
| &nbsp; | 美国西部 | 13.64.254.98</br>23.101.196.19 | \*：443 | 入站 |
| &nbsp; | 美国西部 2 | 52.175.211.210</br>52.175.222.222 | \*：443 | 入站 |
| &nbsp; | 阿拉伯联合酋长国北部 | 65.52.252.96</br>65.52.252.97 | \*：443 | 入站 |

若要获取用于 Azure 政府版的 IP 地址的信息，请参阅 [Azure 政府智能 + 分析](https://docs.microsoft.com/azure/azure-government/documentation-government-services-intelligenceandanalytics)文档。

有关详细信息，请参阅[控制网络流量](hdinsight-plan-virtual-network-deployment.md#networktraffic)一节。

如果使用的是用户定义的路由（Udr），则应指定路由，并允许从虚拟网络到上述 Ip 的出站流量，并将下一个跃点设置为 "Internet"。

## <a name="next-steps"></a>后续步骤

* [为 Azure HDInsight 群集创建虚拟网络](hdinsight-create-virtual-network.md)
* [适用于 Azure HDInsight 的网络安全组（NSG）服务标记](hdinsight-service-tags.md)
