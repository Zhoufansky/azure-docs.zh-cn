---
title: 读取副本-Azure Database for MariaDB
description: 了解 Azure Database for MariaDB 中的读取副本：选择区域、创建副本、连接到副本、监视复制和停止复制。
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 01/16/2020
ms.openlocfilehash: 41b5d7519776ca84cf002d463048eb7a8dec2410
ms.sourcegitcommit: d29e7d0235dc9650ac2b6f2ff78a3625c491bbbf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/17/2020
ms.locfileid: "76169155"
---
# <a name="read-replicas-in-azure-database-for-mariadb"></a>Azure Database for MariaDB 中的只读副本

使用只读副本功能可将数据从 Azure Database for MariaDB 服务器复制到只读服务器。 可将主服务器中的数据复制到最多 5 个副本。 副本使用 MariaDB 引擎的二进制日志（binlog）基于文件位置的复制技术（包含全局事务 ID （GTID））以异步方式更新。 要详细了解 binlog 复制，请参阅 [binlog 复制概述](https://mariadb.com/kb/en/library/replication-overview/)。

副本是您管理的与常规 Azure Database for MariaDB 服务器相似的新服务器。 每个只读副本按照预配计算资源的 vCore 数量以及每月 GB 存储量计费。

若要了解有关 GTID 复制的详细信息，请参阅[MariaDB 复制文档](https://mariadb.com/kb/en/library/gtid/)。

## <a name="when-to-use-a-read-replica"></a>何时使用只读副本

只读副本功能可帮助改善读取密集型工作负荷的性能与规模。 读取工作负载可以与副本服务器隔离，而写入工作负载可以定向到主服务器。

常见方案是让 BI 和分析工作负载将只读副本用作报告的数据源。

由于副本是只读的，它们不能直接缓解主服务器上的写入容量负担。 此功能并非面向写入密集型工作负荷。

读取副本功能使用异步复制。 该功能不适用于同步复制方案。 主服务器与副本之间存在明显的延迟。 副本上的数据最终将与主服务器上的数据保持一致。 对于能够适应这种延迟的工作负荷，可以使用此功能。


## <a name="cross-region-replication"></a>跨区域复制
可以在主服务器所在的不同区域中创建读取副本。 跨区域复制有助于进行灾难恢复规划或使数据更接近用户的情况。

> [!NOTE]
> 跨区域复制处于预览阶段。

可以在任何[Azure Database for MariaDB 区域](https://azure.microsoft.com/global-infrastructure/services/?products=mariadb)中拥有主服务器。  主服务器可以在其配对区域或通用副本区域中拥有副本。 下图显示了哪些副本区域可用，具体取决于你的主区域。

[![读取副本区域](media/concepts-read-replica/read-replica-regions.png)](media/concepts-read-replica/read-replica-regions.png#lightbox)

### <a name="universal-replica-regions"></a>通用副本区域
您可以在以下任何区域中创建读取副本，而不管您的主服务器位于何处。 支持的通用副本区域包括：

澳大利亚东部、澳大利亚东南部、美国中部、东亚、美国东部、美国东部2、日本东部、日本西部、韩国中部、韩国南部、美国中北部、北欧、美国中南部、东南亚、英国南部、英国西部、西欧、美国西部。

\* 美国西部2暂时不可用作跨区域副本位置。


### <a name="paired-regions"></a>配对区域
除通用副本区域外，还可以在主服务器的 Azure 配对区域中创建读取副本。 如果你不知道区域对，可以从[Azure 配对区域一文](../best-practices-availability-paired-regions.md)了解详细信息。

如果你使用跨区域副本进行灾难恢复计划，则建议在配对区域中创建副本，而不是在另一个区域中创建。 配对区域避免同时进行更新，并划分物理隔离和数据驻留的优先级。  

但是，有一些限制： 

* 区域可用性：在美国西部2、法国中部、阿拉伯联合酋长国北部和德国中部提供 Azure Database for MariaDB。 但是，它们的配对区域不可用。
    
* 单向对：某些 Azure 区域仅在一个方向上配对。 这些区域包括印度西部、巴西南部和 US Gov 弗吉尼亚州。 
   这意味着印度西部的主服务器可以在印度南部创建副本。 但是，印度南部的主服务器无法在印度西部创建副本。 这是因为西部印度的次要区域是印度南部地区，而印度南部的次要区域不是西印度。


## <a name="create-a-replica"></a>创建副本

如果主服务器没有现有的副本服务器，则会先重新启动主服务器以准备复制。

启动 "创建副本" 工作流时，将创建一个空白 Azure Database for MariaDB 服务器。 新服务器中填充了主服务器上的数据。 创建时间取决于主服务器上的数据量，以及自上次每周完整备份以来所经历的时间。 具体所需时间从几分钟到几小时不等。

> [!NOTE]
> 如果尚未在服务器上设置存储警报，我们建议进行设置。 当服务器即将达到其存储限制（这会影响复制）时，警报可以向你发出通知。

了解如何[在 Azure 门户中创建只读副本](howto-read-replicas-portal.md)。

## <a name="connect-to-a-replica"></a>连接到副本

在创建时，副本将继承主服务器的防火墙规则或 VNet 服务终结点。 之后，这些规则与主服务器无关。

副本从主服务器继承其管理员帐户。 主服务器上的所有用户帐户将复制到只读副本。 只能使用主服务器上可用的用户帐户连接到只读副本。

您可以使用副本的主机名和有效用户帐户连接到副本，就像在常规 Azure Database for MariaDB 服务器上一样。 对于名为**myreplica**的服务器和管理员用户名**myadmin**，可以使用 mysql CLI 连接到副本：

```bash
mysql -h myreplica.mariadb.database.azure.com -u myadmin@myreplica -p
```

在提示符下，输入用户帐户的密码。

## <a name="monitor-replication"></a>监视复制

Azure Database for MariaDB 提供 Azure Monitor 中的**复制滞后时间（秒）** 。 此指标仅适用于副本。

此指标是使用 MariaDB 的 `SHOW SLAVE STATUS` 命令中可用的 `seconds_behind_master` 指标计算得出的。

设置警报以在复制滞后达到工作负荷不可接受的值时通知你。

## <a name="stop-replication"></a>停止复制

可以停止主服务器与副本之间的复制。 在主服务器与只读副本之间停止复制后，副本将成为独立服务器。 独立服务器中的数据是启动“停止复制”命令时副本上可用的数据。 独立服务器与主服务器不同步。

当你选择停止复制到副本时，它将丢失到其以前的主副本和其他副本的所有链接。 主副本与其副本之间没有自动故障转移。

> [!IMPORTANT]
> 独立服务器不能再次成为副本。
> 在只读副本上停止复制之前，请确保副本包含所需的全部数据。

了解如何[停止复制到副本](howto-read-replicas-portal.md)。

## <a name="considerations-and-limitations"></a>注意事项和限制

### <a name="pricing-tiers"></a>定价层

只读副本当前仅适用于“常规用途”和“内存优化”的定价层。

### <a name="master-server-restart"></a>主服务器重启

为没有现有副本的主副本创建副本时，会先重新启动主副本以准备复制的副本。 请考虑这一点，并在非高峰期执行这些操作。

### <a name="new-replicas"></a>新副本

读取副本创建为新的 Azure Database for MariaDB 服务器。 无法将现有的服务器设为副本。 无法创建另一个只读副本的副本。

### <a name="replica-configuration"></a>副本配置

副本是使用与主服务器相同的服务器配置创建的。 创建副本后，可以独立于主服务器更改多个设置：计算生成、Vcore、存储、备份保持期和 MariaDB 引擎版本。 定价层也可以独立更改，但“基本”层除外。

> [!IMPORTANT]
> 将主服务器的配置更新为新值之前，请将副本配置更新为与这些新值相等或更大的值。 此操作可确保副本与主服务器发生的任何更改保持同步。

创建副本时，防火墙规则、虚拟网络规则和参数设置将从主服务器继承到副本。 之后，副本的规则是独立的。

### <a name="stopped-replicas"></a>停止的副本

如果在主服务器和读取副本之间停止复制，则已停止的副本将成为接受读取和写入的独立服务器。 独立服务器不能再次成为副本。

### <a name="deleted-master-and-standalone-servers"></a>删除的主服务器和独立服务器

删除主服务器后，将对所有只读副本停止复制。 这些副本将自动成为独立的服务器，并且可以接受读写。 将删除主服务器本身。

### <a name="user-accounts"></a>用户帐户

主服务器上的用户将复制到只读副本。 只能使用主服务器上可用的用户帐户连接到只读副本。

### <a name="server-parameters"></a>服务器参数

为了防止数据变得不同步，以及为了避免可能发生的数据丢失或损坏情况，在使用只读副本时，某些服务器参数因为锁定而无法更新。

以下服务器参数在主服务器和副本服务器上都处于锁定状态：
- [`innodb_file_per_table`](https://mariadb.com/kb/en/library/innodb-system-variables/#innodb_file_per_table) 
- [`log_bin_trust_function_creators`](https://mariadb.com/kb/en/library/replication-and-binary-log-system-variables/#log_bin_trust_function_creators)

副本服务器上的[`event_scheduler`](https://mariadb.com/kb/en/library/server-system-variables/#event_scheduler)参数被锁定。

### <a name="other"></a>其他

- 不支持创建副本服务器的副本。
- 内存中表可能会导致副本不同步。这是 MariaDB 复制技术的限制。
- 确保主服务器表具有主键。 缺少主键可能会导致主服务器与副本服务器之间的复制延迟。

## <a name="next-steps"></a>后续步骤

- 了解如何[使用 Azure 门户创建和管理只读副本](howto-read-replicas-portal.md)
- 了解如何[使用 Azure CLI 和 REST API 创建和管理读取副本](howto-read-replicas-cli.md)
