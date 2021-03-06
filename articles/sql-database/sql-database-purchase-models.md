---
title: 购买模型
description: 了解适用于 Azure SQL 数据库的购买模型。
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 02/01/2020
ms.openlocfilehash: 20c93d214195f8fe389f4982e1d8b10998c7057d
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/29/2020
ms.locfileid: "78192381"
---
# <a name="choose-between-the-vcore-and-the-dtu-purchasing-models"></a>选择 vCore 和 DTU 购买模型

使用 Azure SQL 数据库可以轻松购买完全托管的平台即服务（PaaS）数据库引擎，从而满足性能和成本需求。 根据为 Azure SQL 数据库选择的部署模型，可以选择适用于你的购买模型：

- [基于虚拟核心（vCore）的购买模型](sql-database-service-tiers-vcore.md)（推荐）。 此购买模式在预配的计算层和无服务器计算层之间提供了选择。 利用预配的计算层，可选择始终为工作负荷预配的确切计算资源量。 使用无服务器计算层，可以通过可配置的计算范围指定计算资源的自动缩放。 利用此计算层，还可以根据工作负荷活动自动暂停和恢复数据库。 预配的计算层中，每个时间单位的 vCore 单位价格低于无服务器计算层。
- [基于数据库事务单位（DTU）的购买模型](sql-database-service-tiers-dtu.md)。 此购买模型为常见的工作负荷提供了均衡的捆绑计算和存储包。

不同的购买模型适用于不同的 Azure SQL 数据库部署模型：

- [Azure SQL 数据库](sql-database-single-databases-manage.md)中的[单一数据库](sql-database-elastic-pool.md)和[弹性池](sql-database-technical-overview.md)部署选项提供[基于 DTU 的购买模型](sql-database-service-tiers-dtu.md)和[基于 vCore 的购买模型](sql-database-service-tiers-vcore.md)。
- Azure SQL 数据库中的[托管实例](sql-database-managed-instance.md)部署选项仅提供[基于 vCore 的购买模型](sql-database-service-tiers-vcore.md)。
- [超大规模服务层](sql-database-service-tier-hyperscale.md)适用于使用[基于 vCore 的购买模型](sql-database-service-tiers-vcore.md)的单一数据库。

以下表和图表比较和对比基于 vCore 和 DTU 的购买模型：

|**购买模型**|**说明**|**最适用于**|
|---|---|---|
|基于 DTU 的模型|此模型基于计算、存储和 i/o 资源的捆绑度量。 针对单一数据库和弹性池的弹性数据库事务单位（Edtu），计算大小以 Dtu 表示。 有关 Dtu 和 Edtu 的详细信息，请参阅[什么是 dtu 和 edtu？](sql-database-purchase-models.md#dtu-based-purchasing-model)。|最适合需要简单的预配置资源选项的客户。|
|基于 vCore 的模型|此模型允许单独选择计算和存储资源。 基于 vCore 的购买模型还允许使用[适用于 SQL Server 的 Azure 混合权益](https://azure.microsoft.com/pricing/hybrid-benefit/)来节省成本。|最适合注重灵活性、控制度和透明度的客户。|
||||  

![定价模型比较](./media/sql-database-service-tiers/pricing-model.png)

## <a name="compute-costs"></a>计算成本

### <a name="provisioned-compute-costs"></a>预配计算成本

在预配的计算层中，计算成本反映了为应用程序预配的总计算能力。

在“业务关键”服务层级中，我们会自动分配至少 3 个副本。 为了反映这种额外的计算资源分配，在业务关键服务层中，基于 vCore 的购买模型中的价格大约比常规用途服务层高 2.7 x。 同样，在 "业务关键" 服务 "层中，每 GB 的存储价格越高，就会反映更高的 IO 限制和 SSD 存储的延迟较低。

对于业务关键服务层和常规用途服务层，备份存储的成本是相同的，因为这两个层都使用标准存储进行备份。

### <a name="serverless-compute-costs"></a>无服务器计算成本

有关如何定义计算容量和计算无服务器计算层开销的说明，请参阅[SQL 数据库无服务器](sql-database-serverless.md)。

## <a name="storage-costs"></a>存储成本

不同存储类型的计费方式各不相同。 对于数据存储，将根据所选的最大数据库或池大小对预配的存储进行收费。 除非您减小或增加该最大值，否则成本不会改变。 备份存储与实例的自动备份相关联，并动态分配。 增加备份保持期会增加实例使用的备份存储空间。

默认情况下，将数据库的7天自动备份复制到读取访问异地冗余存储（GRS）标准 Blob 存储帐户。 此存储由每周完整备份、每日差异备份和事务日志备份使用，每5分钟复制一次。 事务日志的大小取决于数据库的更改速率。 最小存储量等于100% 的数据库大小，无需额外付费。 超出此部分的其他备份存储空间按 GB/月收费。

有关存储价格的详细信息，请参阅[定价](https://azure.microsoft.com/pricing/details/sql-database/single/)页。

## <a name="vcore-based-purchasing-model"></a>基于 vCore 的购买模型

虚拟核心（vCore）表示逻辑 CPU，并提供在硬件的各代和硬件物理特征之间进行选择的选项（例如，内核数、内存和存储大小）。 基于 vCore 的购买模型为你提供了对单个资源消耗的灵活性、控制、透明性，并通过一种简单的方式将本地工作负荷要求转换到云。 此模型可让你根据工作负荷需求选择计算、内存和存储资源。

在基于 vCore 的购买模型中，可以在[单个数据库](sql-database-single-database-scale.md)、[弹性池](sql-database-elastic-pool.md)和[托管实例](sql-database-managed-instance.md)的[常规用途](sql-database-high-availability.md#basic-standard-and-general-purpose-service-tier-availability)和[业务关键](sql-database-high-availability.md#premium-and-business-critical-service-tier-availability)服务层之间进行选择。 对于单一数据库，还可以选择[超大规模服务层](sql-database-service-tier-hyperscale.md)。

基于 vCore 的购买模式使你可以独立选择计算和存储资源、与本地性能匹配，并优化价格。 在基于 vCore 的购买模型中，你需要支付：

- 计算资源（服务层 + Vcore 数和内存量 + 硬件的生成）。
- 数据和日志存储的类型和数量。
- 备份存储（GRS）。

> [!IMPORTANT]
> 计算资源、i/o 以及数据和日志存储按数据库或弹性池收费。 按每个数据库对备份存储收费。 有关托管实例费用的详细信息，请参阅[托管实例](sql-database-managed-instance.md)。
> **区域限制：** 有关支持的区域的最新列表，请参阅[可用产品（按区域](https://azure.microsoft.com/global-infrastructure/services/?products=sql-database&regions=all)）。 若要在当前不受支持的区域中创建托管实例，请[通过 Azure 门户发送支持请求](quota-increase-request.md)。

如果单一数据库或弹性池消耗的 Dtu 超过300，则转换为基于 vCore 的购买模型可能会降低成本。 你可以使用选择的 API 或使用 Azure 门户进行转换，而无需停机。 但是，不需要进行转换，也不会自动执行转换。 如果基于 DTU 的购买模型可以满足性能和业务要求，应继续使用它。

若要从基于 DTU 的购买模型转换为基于 vCore 的购买模型，请使用以下经验法则选择计算大小：

- 标准层中的每个 100 Dtu 要求常规用途服务层中至少有1个 vCore。
- 高级层中的每个 125 Dtu 要求业务关键服务层中至少有1个 vCore。

## <a name="dtu-based-purchasing-model"></a>基于 DTU 的购买模型

数据库事务单位（DTU）表示 CPU、内存、读取和写入的混合度量值。 基于 DTU 的购买模型提供一组预配置的计算资源套件和随附的存储，以促成不同级别的应用程序性能。 如果你更喜欢预先配置的捆绑包和每月的固定付款，则基于 DTU 的模型可能更适合你的需求。

在基于 DTU 的购买模型中，可以在 "基本"、"标准" 和 "高级" 服务层之间为[单一数据库](sql-database-single-database-scale.md)和[弹性池](sql-database-elastic-pool.md)选择。 基于 DTU 的购买模型不适用于[托管实例](sql-database-managed-instance.md)。

### <a name="database-transaction-units-dtus"></a>数据库事务单位 (DTU)

对于[服务层](sql-database-single-database-scale.md)内特定计算大小的单一数据库，Microsoft 保证该数据库（独立于 Azure 云中的任何其他数据库）的特定资源级别。 此保证可提供可预测的性能级别。 为数据库分配的资源量将计算为多个 Dtu，并是计算、存储和 i/o 资源的捆绑度量值。

这些资源之间的比率最初由专用于真实 OLTP 工作负荷的[联机事务处理（OLTP）基准负载](sql-database-benchmark-overview.md)来确定。 如果工作负荷超出了任何这些资源的数量，吞吐量会受到限制，从而导致性能下降和超时。

工作负荷使用的资源不会影响 Azure 云中其他 SQL 数据库可用的资源。 同样，其他工作负荷使用的资源不会影响可用于 SQL 数据库的资源。

![边界框](./media/sql-database-what-is-a-dtu/bounding-box.png)

Dtu 最适用于了解为不同计算大小和服务层的 Azure SQL 数据库分配的相关资源。 例如：

- 通过增加数据库的计算大小使 Dtu 翻倍相当于使该数据库可用的资源集翻倍。
- 具有 1750 Dtu 的高级服务层 P11 数据库比具有5个 Dtu 的基本服务层数据库提供350倍更多 DTU 计算能力。  

若要深入了解工作负荷的资源（DTU）消耗，请使用[查询性能见解](sql-database-query-performance.md)：

- 按照 CPU/持续时间/执行计数确定最常见的查询，这些查询可能会进行优化以改善性能。 例如，i/o 密集型查询可能会受益于[内存中优化技术](sql-database-in-memory.md)，以便更好地利用特定服务层和计算大小的可用内存。
- 向下钻取查询的详细信息，查看其文本和资源使用情况的历史记录。
- 访问性能优化建议，用于显示[SQL 数据库顾问](sql-database-advisor.md)执行的操作。

### <a name="elastic-database-transaction-units-edtus"></a>弹性数据库事务单位（Edtu）

对于始终可用的 SQL 数据库，而不是提供一组并非始终需要的专用资源（Dtu），则可以将这些数据库放入[弹性池中](sql-database-elastic-pool.md)。 弹性池中的数据库位于单个 Azure SQL 数据库服务器上，并共享资源池。

弹性池中的共享资源由弹性数据库事务单位（Edtu）来度量。 弹性池提供了一个简单且经济高效的解决方案，可用于管理具有广泛变化且不可预测的使用模式的多个数据库的性能目标。 弹性池可保证池中的一个数据库无法使用所有资源，同时确保池中的每个数据库始终具有可用的最小可用资源量。

为池提供的 eDTU 的数量和价格是固定的。 在弹性池中，每个数据库都可以在配置的边界内自动缩放。 较重负载下的数据库将会消耗更多的 Edtu 来满足需求。 较轻负载下的数据库将消耗较少的 Edtu。 无负载的数据库不消耗任何 eDTU。 因为资源是为整个池而不是基于数据库进行设置的，所以弹性池可简化管理任务并为池提供可预测的预算。

你可以将其他 Edtu 添加到现有池，无需数据库停机，也不会影响池中的数据库。 同样，如果不再需要额外的 Edtu，可随时从现有池中删除它们。 你还可以随时将数据库添加到池中或从池中减去数据库。 若要为其他数据库保留 Edtu，请在重负载下限制数据库可使用的 Edtu 数。 如果数据库持续 underuses 资源，请将其移出池，并将其配置为具有可预测的必需资源量的单一数据库。

### <a name="determine-the-number-of-dtus-needed-by-a-workload"></a>确定工作负荷所需的 DTU 数

如果要将现有的本地或 SQL Server 虚拟机工作负荷迁移到 Azure SQL 数据库，请使用[dtu 计算器](https://dtucalculator.azurewebsites.net/)估计所需的 dtu 数。 对于现有的 Azure SQL 数据库工作负荷，请使用[查询性能见解](sql-database-query-performance.md)来了解数据库资源消耗（dtu），并获得更深入的信息来优化工作负荷。 [Sys. dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)动态管理视图（DMV）允许您查看最近一小时的资源消耗。 [Sys. resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)目录视图显示过去14天的资源消耗，但以较低的保真度表示平均的5分钟。

### <a name="determine-dtu-utilization"></a>确定 DTU 利用率

若要确定 DTU/eDTU 使用率相对于数据库或弹性池的 DTU/eDTU 限制的平均百分比，请使用以下公式：

`avg_dtu_percent = MAX(avg_cpu_percent, avg_data_io_percent, avg_log_write_percent)`

此公式的输入值可从[sys.databases. dm_db_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)、 [resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)和[sys. elastic_pool_resource_stats](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-elastic-pool-resource-stats-azure-sql-database) dmv 获得。 换句话说，若要确定 DTU/eDTU 对数据库或弹性池的 DTU/edtu 限制的百分比，请从以下位置选择最大百分比值： `avg_cpu_percent`、`avg_data_io_percent`和 `avg_log_write_percent` 在给定时间点。

> [!NOTE]
> 数据库的 DTU 限制由 CPU、读取、写入和数据库的可用内存决定。 但是，因为 SQL Server 数据库引擎通常会使用所有可用内存来提高性能，所以，不管当前的数据库负载如何，`avg_memory_usage_percent` 的值通常接近100%。 因此，即使内存间接影响 DTU 限制，也不会在 DTU 利用率公式中使用它。
>

### <a name="workloads-that-benefit-from-an-elastic-pool-of-resources"></a>受益于资源弹性池的工作负荷

池非常适用于资源利用率较低且利用率相对较少的数据库。 有关详细信息，请参阅[何时应考虑使用 SQL 数据库弹性池？](sql-database-elastic-pool.md)。

### <a name="hardware-generations-in-the-dtu-based-purchasing-model"></a>基于 DTU 的购买模型中的硬件代

在基于 DTU 的购买模型中，客户无法选择用于其数据库的硬件生成。 虽然给定的数据库通常会长时间（通常为多个月）在特定的硬件上生成，但有一些事件会导致数据库移到另一个硬件生成。

例如，如果数据库将扩展或缩减到不同的服务目标，或者数据中心的当前基础结构正在接近其容量限制，或者当前使用的硬件正在运行，则可以将该数据库移到不同的硬件生成中因为其生命周期结束而停止。

如果将数据库移到不同的硬件上，工作负荷性能可能会改变。 DTU 模型保证当数据库移动到不同的硬件生成时[dtu 基准](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers-dtu#dtu-benchmark)工作负荷的吞吐量和响应时间将保持完全相同，前提是其服务目标（dtu 数）保持不变。 

但是，在 Azure SQL 数据库中运行的各种客户工作负荷中，对相同服务目标使用不同硬件的影响可能更明显。 不同的工作负荷将受益于不同的硬件配置和功能。 因此，对于 DTU 基准以外的工作负荷，如果数据库从一个硬件生成移到另一个，则可能会看到性能差异。

例如，对网络延迟敏感的应用程序可以在 Gen5 硬件与 Gen4 上看到更好的性能，因为使用 Gen5 中的加速网络，但使用大量读取 IO 的应用程序可以在 Gen4 硬件和 Gen5 上看到更好的性能，因为Gen4 上的每个核心比率更高的内存。

如果客户的工作负荷对硬件的更改或希望控制其数据库的硬件生成选择的客户，则可以使用[vCore](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers-vcore)模型在数据库创建和缩放期间选择其首选的硬件生成。 在 vCore 模型中，为[单个数据库](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-single-databases)和[弹性池](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-elastic-pools)记录每个硬件生成上每个服务目标的资源限制。 有关 vCore 模型中的硬件生成的详细信息，请参阅[硬件代](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers-vcore#hardware-generations)。

## <a name="frequently-asked-questions-faqs"></a>常见问题 (FAQ)

### <a name="do-i-need-to-take-my-application-offline-to-convert-from-a-dtu-based-service-tier-to-a-vcore-based-service-tier"></a>我是否需要使应用程序脱机，以便从基于 DTU 的服务层转换到基于 vCore 的服务层？

不是。 无需使应用程序脱机。 新服务层提供了简单的在线转换方法，这类似于将数据库从标准升级到高级服务层的现有过程，以及其他方法。 您可以使用 Azure 门户、PowerShell、Azure CLI、T-sql 或 REST API 来启动此转换。 请参阅[管理单一数据库](sql-database-single-database-scale.md)和[管理弹性池](sql-database-elastic-pool.md)。

### <a name="can-i-convert-a-database-from-a-service-tier-in-the-vcore-based-purchasing-model-to-a-service-tier-in-the-dtu-based-purchasing-model"></a>是否可以将基于 vCore 的购买模型中的服务层的数据库转换为基于 DTU 的购买模型中的服务层？

是的，可以使用 Azure 门户、PowerShell、Azure CLI、T-sql 或 REST API 轻松将数据库转换为任何受支持的性能目标。 请参阅[管理单一数据库](sql-database-single-database-scale.md)和[管理弹性池](sql-database-elastic-pool.md)。

## <a name="next-steps"></a>后续步骤

- 有关基于 vCore 的购买模型的详细信息，请参阅[基于 vCore 的购买模型](sql-database-service-tiers-vcore.md)。
- 有关基于 DTU 的购买模型的详细信息，请参阅[基于 dtu 的购买模型](sql-database-service-tiers-dtu.md)。
