---
title: Azure Migrate 中的评估
description: 了解 Azure Migrate 中的评估。
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: 0cf933dd1c8c61edfcea20ea954c5813f3848b28
ms.sourcegitcommit: b8f2fee3b93436c44f021dff7abe28921da72a6d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/18/2020
ms.locfileid: "77425691"
---
# <a name="about-assessments-in-azure-migrate"></a>关于 Azure Migrate 中的评估

本文介绍如何在[Azure Migrate：服务器评估](migrate-services-overview.md#azure-migrate-server-assessment-tool)中计算评估。 对本地计算机组运行评估，确定是否已准备好迁移到 Azure Migrate。

## <a name="how-do-i-run-an-assessment"></a>如何实现运行评估？
你可以使用 Azure Migrate：服务器评估或其他 Azure 或第三方工具来运行评估。 创建 Azure Migrate 项目后，添加所需的工具。 [了解详细信息](how-to-add-tool-first-time.md)

### <a name="collect-compute-data"></a>收集计算数据

按如下所述收集计算设置的性能数据：

1. [Azure Migrate 设备](migrate-appliance.md)将收集实时示例点：

    - **Vmware vm**：对于 vmware vm，Azure Migrate 设备将按每隔20秒的时间间隔收集实时采样点。
    - **Hyper-v vm**：对于 hyper-v vm，将每隔30秒的时间间隔收集实时示例点。
    - **物理服务器**：对于物理服务器，将每隔五分钟收集一次实时示例点。 
    
2. 设备将汇总示例点（20秒、30秒、5分钟），以便每10分钟创建一个数据点。 若要创建单一数据点，设备从所有示例中选择 "峰值" 值，然后将其发送到 Azure。
3. 服务器评估存储了上个月的所有10分钟示例点。
4. 创建评估时，服务器评估会根据*性能历史记录*和*百分比利用率*的百分位值标识用于适当调整大小的适当数据点。

    - 例如，如果将性能历史记录设置为一周，百分比利用率为95%，则服务器评估将按升序对最后一周的10分钟示例点进行排序，并选取95% 的值以进行右调整。 
    - 95% 的值可以确保忽略任何离群值，如果选择99% 的百分点值，则可能会包含这些离群值。
    - 如果要选择该时间段的高峰使用量，而不想错过任何离群值，则应选择99% 百分位以实现百分比利用率。

5. 此值与舒适系数相乘，可获取每个指标（CPU 使用率、内存使用率、磁盘 IOPS （读取和写入）、磁盘吞吐量（读取和写入）和网络吞吐量（传入和传出）的有效性能利用率数据。设备收集。

若要在服务器评估中运行评估，请在本地和 Azure 中准备评估，并设置 Azure Migrate 设备，以持续发现本地计算机。 发现计算机后，可将其收集到组中以进行评估。 若要进行更详细的、更自信的评估，可以可视化和映射计算机之间的依赖关系，以确定如何迁移它们。

- 了解如何为[VMware vm](tutorial-prepare-vmware.md)、 [hyper-v vm](tutorial-prepare-hyper-v.md)和[物理服务器](tutorial-prepare-physical.md)运行评估。
- 了解如何评估[使用 CSV 文件导入](tutorial-assess-import.md)的服务器。
- 了解如何设置[依赖项可视化](concepts-dependency-visualization.md)。

## <a name="assessments-in-server-assessment"></a>服务器评估中的评估 

使用 Azure Migrate Server 评估创建的评估是数据的时间点快照。 服务器评估工具提供两种类型的评估。

**评估类型** | **详细信息** | **数据**
--- | --- | ---
**基于性能** | 基于收集的性能数据提出建议的评估 | VM 大小建议基于 CPU 和内存使用率数据。<br/><br/> 磁盘类型建议（标准 HDD/SSD 或高级托管磁盘）基于本地磁盘的 IOPS 和吞吐量。
**按本地** | 评估不使用性能数据来提出建议。 | VM 大小建议基于本地 VM 大小<br/><br> 建议的磁盘类型基于所选的评估存储类型。

## <a name="collecting-performance-data"></a>收集性能数据

性能数据的收集方式如下：

1. [Azure Migrate 设备](migrate-appliance.md)将收集实时示例点：

    - **Vmware vm**：对于 vmware vm，Azure Migrate 设备将按每隔20秒的时间间隔收集实时采样点。
    - **Hyper-v vm**：对于 hyper-v vm，将每隔30秒的时间间隔收集实时示例点。
    - **物理服务器**：对于物理服务器，将每隔五分钟收集一次实时示例点。 
    
2. 设备将汇总示例点（20秒、30秒、5分钟），以便每10分钟创建一个数据点。 若要创建单一数据点，设备从所有示例中选择 "峰值" 值，然后将其发送到 Azure。
3. 服务器评估存储了上个月的所有10分钟示例点。
4. 创建评估时，服务器评估会根据*性能历史记录*和*百分比利用率*的百分位值标识用于适当调整大小的适当数据点。

    - 例如，如果将性能历史记录设置为一周，百分比利用率为95%，则服务器评估将按升序对最后一周的10分钟示例点进行排序，并选取95% 的值以进行右调整。 
    - 95% 的值可以确保忽略任何离群值，如果选择99% 的百分点值，则可能会包含这些离群值。
    - 如果要选择该时间段的高峰使用量，而不想错过任何离群值，则应选择99% 百分位以实现百分比利用率。

5. 此值与舒适系数相乘，可获取每个指标（CPU 使用率、内存使用率、磁盘 IOPS （读取和写入）、磁盘吞吐量（读取和写入）和网络吞吐量（传入和传出）的有效性能利用率数据。设备收集。
## <a name="whats-in-an-assessment"></a>评估内容有哪些？

Azure Migrate：服务器评估中的评估包含以下内容。

**属性** | **详细信息**
--- | ---
**目标位置** | 要迁移到的位置。服务器评估目前支持以下目标 Azure 区域：<br/><br/> 澳大利亚东部、澳大利亚东南部、巴西南部、加拿大中部、加拿大东部、印度中部、美国中部、中国东部、中国北部、东亚、美国东部、东2、德国中部、德国东北部、日本东部、日本西部、韩国中部、韩国南部、北部美国中部、北欧、美国中南部、东南亚、印度南部、英国南部、英国西部、US Gov 亚利桑那州、US Gov 德克萨斯州、US Gov 弗吉尼亚州、美国西部、西欧、印度西部、美国西部和西2。
*目标存储磁盘（按大小调整）* * | 要用于 Azure 中的存储的磁盘类型。 <br/><br/> 将目标存储磁盘指定为管理的高级托管的标准 SSD 或标准 HDD。
**目标存储磁盘（基于性能的大小调整）** | 将目标存储磁盘的类型指定为 "自动"、"高级托管"、"标准 HDD 管理" 或 "标准 SSD 托管"。<br/><br/> **自动**：磁盘建议基于磁盘的性能数据（每秒输入/输出操作数（IOPS）和吞吐量）。<br/><br/>**高级/标准**：评估建议选定存储类型中的磁盘 SKU。<br/><br/> 如果要实现99.9% 的单实例 VM SLA，请考虑使用高级托管磁盘。 这可确保将评估中的所有磁盘建议为高级托管磁盘。<br/><br/> Azure Migrate 仅支持使用托管磁盘进行迁移评估。
**预订实例（RIs）** | 指定 Azure 中的[保留实例](https://azure.microsoft.com/pricing/reserved-vm-instances/)，以便评估中的成本估计会考虑到 RI 折扣。<br/><br/> 目前仅支持 Azure Migrate 的即用即付产品/服务的 RIs。
**调整大小标准** | 用于在 Azure 中正确调整 VM 的大小。<br/><br/> 使用按原样调整大小或基于性能的大小调整。
**性能历史记录** | 用于基于性能的大小调整。 指定评估性能数据时使用的持续时间。
**百分位使用率** | 用于基于性能的大小调整。 指定要用于正确调整大小的性能样本的百分比值。 
**VM 系列** | 指定要用于调整大小的 Azure VM 系列。 例如，如果你没有需要在 Azure 中使用 A 系列 Vm 的生产环境，则可以从列表或系列中排除 A 系列。
**舒适因子** | 评估过程中使用的缓冲区。 应用于 Vm 的计算机使用率数据（CPU、内存、磁盘和网络）之上。 它用于解决季节性使用情况、简短性能历史记录等问题，并可能会在将来的使用中增大。<br/><br/> 例如，利用率为20% 的10核 VM 通常会产生两核 VM。 由于 2.0 x 的舒适因素，结果是一个四核 VM。
**产品** | 显示您注册的[Azure 产品/服务](https://azure.microsoft.com/support/legal/offer-details/)。 服务器评估会相应地评估成本。
**货币** | 帐户的计费货币。
**折扣 (%)** | 列出你在 Azure 产品/服务上收到的任何特定于订阅的折扣。 默认设置是 0%。
**VM 运行时间** | 如果 Azure Vm 每周7天，每天24小时运行一次，则可以指定它们将运行的持续时间（每月和每月的小时数）。 将相应地处理成本估算。<br/><br/> 默认值为“每月 31 天和每天 24 小时”。
**Azure 混合权益** | 指定您是否具有软件保障并且有资格使用[Azure 混合权益](https://azure.microsoft.com/pricing/hybrid-use-benefit/)。 如果设置为 "是" （默认设置），则考虑 Windows Vm 的非 Windows Azure 价格。

查看有关通过服务器评估创建评估的[最佳实践](best-practices-assessment.md)。

## <a name="how-are-assessments-calculated"></a>评估如何计算？ 

Azure Migrate 中的评估：使用收集的有关本地计算机的元数据计算服务器评估。 如果你在使用导入的计算机上运行评估。CSV 文件，为计算提供元数据。 计算分三个阶段进行：

1. **计算 Azure 就绪**：评估计算机是否适合迁移到 Azure。
2. **计算大小调整建议**：估算计算、存储和网络大小。 
2. **计算每月成本**：计算迁移后在 Azure 中运行计算机的估计每月计算和存储成本。

计算按顺序进行，并且计算机服务器仅在传递前一个阶段时才会移动到后面的阶段。 例如，如果某个服务器未能通过 Azure 就绪，则会将其标记为不适用于 Azure，并且不会对该服务器执行大小调整和成本计算。



## <a name="calculate-readiness"></a>计算准备情况

并非所有计算机都适合在 Azure 中运行。 服务器评估会评估每个本地计算机，并为其分配一个就绪类别。 
- **适用于 azure**：计算机可以按原样迁移到 azure，而无需进行任何更改。 它将在 Azure 中从完整的 Azure 支持开始。
- **Azure 有条件的就绪**：计算机可能会在 azure 中启动，但可能没有完整的 azure 支持。 例如，Azure 中不支持运行较早版本 Windows Server 的计算机。 在将这些计算机迁移到 Azure 之前，必须注意。 遵循评估中建议的修正指南来解决准备情况问题。
- **未准备好进行 azure**：计算机不会在 azure 中启动。 例如，如果本地计算机磁盘大于 64 Tb，则不能在 Azure 中托管它。 请按照更正指南进行操作，以在迁移前解决此问题。 
- **准备情况未知**：由于元数据不足，Azure Migrate 无法确定计算机的准备情况。

若要计算准备情况，服务器评估会查看下表中汇总的计算机属性和操作系统设置。 

### <a name="machine-properties"></a>计算机属性

服务器评估将查看本地 VM 的以下属性，以确定它是否可在 Azure 上运行。

**属性** | **详细信息** | **Azure 迁移就绪性状态**
--- | --- | ---
**启动类型** | Azure 支持启动类型为 BIOS 而不是 UEFI 的 Vm。 | 如果启动类型为 UEFI，则有条件地准备就绪。
**核心数** | 计算机中的内核数必须等于或小于 Azure VM 支持的最大核心数（128）。<br/><br/> 如果性能历史记录可用，Azure Migrate 会考虑已利用的内核数以进行比较。 如果在评估设置中指定了舒适因子，则将已利用的内核数乘以此舒适因子。<br/><br/> 如果没有性能历史记录，Azure Migrate 将使用已分配的内核，而不应用舒适因子。 | 如果小于或等于限制，则状态为就绪。
**内存** | 对于 Azure VM，计算机内存大小必须等于或小于 Azure M 系列上的最大内存（3892 gb Standard_M128m&nbsp;<sup>2</sup>）。 [了解详细信息](https://docs.microsoft.com/azure/virtual-machines/windows/sizes)。<br/><br/> 如果性能历史记录可用，Azure Migrate 会考虑已利用的内存以进行比较。 如果指定了舒适因子，则将已利用的内存乘以此舒适因子。<br/><br/> 如果没有历史记录，则使用分配的内存，而不应用舒适因子。<br/><br/> | 如果在限制范围内，则状态为就绪。
**存储磁盘** | 分配的磁盘大小必须为 32 TB 或更小。 尽管 Azure 支持 64 TB 磁盘超级 SSD 磁盘，Azure Migrate：服务器评估目前检查的磁盘大小限制为 32 TB，因为它尚不支持超级 SSD。 <br/><br/> 附加到计算机的磁盘数必须是65或更少，包括操作系统磁盘。 | 如果在限制范围内，则状态为就绪。
**网络** | 计算机上必须连接32个或更少的网络接口（Nic）。 | 如果在限制范围内，则状态为就绪。

### <a name="guest-operating-system"></a>来宾操作系统
服务器评估连同 VM 属性一起查看计算机的来宾操作系统，以确定它是否可在 Azure 上运行。

> [!NOTE]
> 对于 VMware Vm，服务器评估使用在 vCenter Server 中为 VM 指定的操作系统来处理来宾操作系统分析。 对于在 VMware 上运行的 Linux Vm，它当前不识别来宾操作系统的确切内核版本。

服务器评估使用以下逻辑来识别基于操作系统的 Azure 就绪情况。

**操作系统** | **详细信息** | **Azure 迁移就绪性状态**
--- | --- | ---
Windows Server 2016 和所有 SP | Azure 提供完全支持。 | 已做好 Azure 迁移准备
Windows Server 2012 R2 和所有 SP | Azure 提供完全支持。 | 已做好 Azure 迁移准备
Windows Server 2012 和所有 SP | Azure 提供完全支持。 | 已做好 Azure 迁移准备
Windows Server 2008 R2 和所有 SP | Azure 提供完全支持。| 已做好 Azure 迁移准备
Windows Server 2008（32 位和 64 位） | Azure 提供完全支持。 | 已做好 Azure 迁移准备
Windows Server 2003、2003 R2 | 这些操作系统已超过其支持的截止日期，需要[自定义支持协议（CSA）](https://aka.ms/WSosstatement)以支持 Azure 中的支持。 | Azure 有条件的就绪。 请考虑在迁移到 Azure 之前升级 OS。
Windows 2000、98、95、NT、3.1、MS-DOS | 这些操作系统已超过其支持结束日期。 计算机可能会在 Azure 中启动，但 Azure 不提供 OS 支持。 | Azure 有条件的就绪。 建议在迁移到 Azure 之前升级 OS。
Windows Client 7、8 和 10 | Azure 仅支持 [Visual Studio 订阅。](https://docs.microsoft.com/azure/virtual-machines/windows/client-images) | 已做好特定条件下的 Azure 迁移准备
Windows 10 专业版桌面 | Azure 提供了对[多租户托管权限](https://docs.microsoft.com/azure/virtual-machines/windows/windows-desktop-multitenant-hosting-deployment)的支持。 | 已做好特定条件下的 Azure 迁移准备
Windows Vista、XP Professional | 这些操作系统已超过其支持结束日期。 计算机可能会在 Azure 中启动，但 Azure 不提供 OS 支持。 | Azure 有条件的就绪。 建议在迁移到 Azure 之前升级 OS。
Linux | Azure 予以认可这些 [Linux 操作系统](../virtual-machines/linux/endorsed-distros.md)。 其他 Linux 操作系统可能会在 Azure 中启动，但建议在迁移到 Azure 之前将 OS 升级到认可的版本。 | 如果版本受到认可，则为 Azure 已就绪。<br/><br/>如果版本不受认可，则为 Azure 有条件的就绪。
其他操作系统<br/><br/> 例如，Oracle Solaris、Apple macOS 等。 | Azure 不认可这些操作系统。 计算机可能会在 Azure 中启动，但 Azure 不提供 OS 支持。 | Azure 有条件的就绪。 建议在迁移到 Azure 之前安装受支持的操作系统。  
vCenter Server 中指定为“其他”的 OS | 在此情况下，Azure Migrate 无法确认 OS。 | 就绪性未知。 确保 VM 内部运行的 OS 在 Azure 中受到支持。
32 位操作系统 | 计算机可能会在 Azure 中启动，但 Azure 可能不提供完全支持。 | Azure 有条件的就绪。 迁移到 Azure 之前，请考虑将计算机的操作系统从32位 OS 升级到64位操作系统。

## <a name="calculate-sizing-as-is-on-premises"></a>计算大小（按本地方式）

将计算机标记为准备好进行 Azure 后，服务器评估会根据规模建议确定 Azure VM 和磁盘 SKU。 如果使用 "按本地大小调整"，则服务器评估不会考虑 Vm 和磁盘的性能历史记录。

**计算规模**：根据本地分配的大小分配 AZURE VM SKU。
**存储/磁盘大小调整**：服务器评估查看评估属性（标准 HDD/SSD/premium）中指定的存储类型，并相应地推荐磁盘类型。 默认存储类型为高级磁盘。
**网络大小调整**：服务器评估考虑本地计算机上的网络适配器。


## <a name="calculate-sizing-performance-based"></a>计算大小（基于性能）

将计算机标记为 "就绪" 后，如果你使用性能基础大小调整，服务器评估会按如下所示调整大小建议：

- 服务器评估会考虑计算机的性能历史记录来识别 Azure 中的 VM 大小和磁盘类型。
- 如果使用 CSV 文件导入服务器，则使用指定的值。 此方法在以下情况下特别有用：已过度分配本地计算机，利用率实际较低，并且想要在 Azure 中将 VM 调整到适当大小以节省成本。 
- 如果不想要使用性能数据，请按照上一节中所述，将大小调整条件重置为 "按本地"。

### <a name="calculate-storage-sizing"></a>计算存储大小

对于存储大小调整，Azure Migrate 尝试将连接到计算机的每个磁盘映射到 Azure 中的磁盘，如下所示：

1. 服务器评估增加了磁盘的读取和写入 IOPS，以获取所需的总 IOPS。 同样，它会添加 "读取" 和 "写入吞吐量" 值，以获取每个磁盘的总吞吐量。
2. 如果已将存储类型指定为 "自动"，则基于 "有效 IOPS" 和 "吞吐量" 值，"服务器评估" 确定是否应将磁盘映射到 Azure 中的标准 HDD、标准 SSD 或高级磁盘。 如果存储类型设置为标准 HDD/SSD/Premium，则服务器评估会尝试在所选存储类型（标准 HDD/SSD/高级磁盘）内查找磁盘 SKU。
3. 选择磁盘的方法如下所示：
    - 如果服务器评估找不到具有所需 IOPS 和吞吐量的磁盘，则会将计算机标记为不适用于 Azure。
    - 如果服务器评估查找一组合适的磁盘，则会选择支持在评估设置中指定的位置的磁盘。
    - 如果有多个合格的磁盘，服务器评估会选择成本最低的磁盘。
    - 如果任何磁盘的性能数据不可用，则使用磁盘（磁盘大小）的配置数据在 Azure 中查找标准 SSD 磁盘。

### <a name="calculate-network-sizing"></a>计算网络大小

服务器评估尝试查找可支持连接到本地计算机的网络适配器数量和这些网络适配器所需性能的 Azure VM。
- 为了获得本地 VM 的有效网络性能，服务器评估会将每秒传输的数据（MBps）从计算机（网络输出）中聚合到所有网络适配器上，并应用舒适因子。 它使用此数字来查找可支持所需网络性能的 Azure VM。
- 与网络性能一起，服务器评估还会考虑 Azure VM 是否可支持所需的网络适配器数。
- 如果没有任何网络性能数据可用，则服务器评估只考虑 VM 大小的网络适配器计数。


### <a name="calculate-compute-sizing"></a>计算计算规模

在计算存储和网络需求后，服务器评估会考虑 CPU 和内存要求，以便在 Azure 中查找合适的 VM 大小。
- Azure Migrate 查看有效利用的核心和内存，以在 Azure 中查找合适的 VM 大小。
- 如果找不到合适的大小，计算机将标记为不适用于 Azure。
- 如果找到了合适的大小，Azure Migrate 将应用存储和网络计算。 然后，它会对最终的 VM 大小建议应用位置和定价层设置。
- 如果有多个合格的 Azure VM 大小，建议选择成本最低的那一个。


### <a name="calculate-confidence-ratings"></a>计算置信度

Azure Migrate 中的每个基于性能的评估都与一个（最低）到5星（最高）的置信度分级相关联。
- 为评估分配置信度时，会考虑到进行评估计算时所需数据点的可用性。
- 对评估的置信度分级可以用来评估 Azure Migrate 提供的大小建议的可靠性。
- 置信度评级不适用于*作为本地*评估。
- 对于基于性能的大小调整，服务器评估需要：
    - CPU 和 VM 内存的利用率数据。
    - 附加到 VM 的每个磁盘的磁盘 IOPS 和吞吐量数据。
    - 为附加到 VM 的每个网络适配器处理基于性能的大小调整的网络 i/o。

   如果在 vCenter Server 中有任何使用不足，则大小建议可能不可靠。

根据可用的数据点数百分比，评估的置信度分级如下所示。

   **数据点的可用性** | **置信度分级**
   --- | ---
   0-20% | 1 星
   21-40% | 2 星
   41-60% | 3 星
   61-80% | 4 星
   81-100% | 5 星

> [!NOTE]
> 不会将置信度评级分配到使用导入的服务器的评估。CSV 文件 Azure Migrate。 

#### <a name="low-confidence-ratings"></a>低置信度分级

下面是评估降低置信度的几个原因：

- 你未在创建评估的持续时间内分析环境。 例如，如果你创建了 "性能持续时间" 设置为一天的评估，则必须在开始发现所有数据点后等待至少一天。
- 一些 VM 在进行评估计算期间关闭。 如果任何 Vm 在某个持续时间内关闭，服务器评估将无法收集该时间段的性能数据。
- 某些 Vm 是在计算评估期间创建的。 例如，如果你为上个月的性能历史记录创建了评估，但某些 Vm 只是在一周前的环境中创建的，则新 Vm 的性能历史记录不会存在于完整的持续时间内。

> [!NOTE]
> 如果任何评估的置信度小于五星级，我们建议你等待至少一天的时间让设备对环境进行配置，然后重新计算评估。 如果不这样做，则基于性能的大小调整可能不可靠。 在这种情况下，我们建议你将评估切换为本地大小调整。

## <a name="calculate-monthly-costs"></a>计算每月成本

调整大小建议完成后，Azure Migrate 在迁移后计算计算和存储成本。

- 计算成本：使用建议的 Azure VM 大小，Azure Migrate 使用计费 API 来计算 VM 每月成本。
    - 该计算会考虑操作系统、软件保障、预留实例、VM 运行时间、位置和货币设置。
    - 它在所有计算机上聚合成本，以计算每月总计算成本。
- **存储成本**：通过聚合附加到计算机的所有磁盘的每月成本来计算计算机的每月存储成本，如下所示：
    - 服务器评估通过聚合所有计算机的存储成本来计算每月总存储成本。
    - 目前，计算不会考虑评估设置中指定的产品/服务。

成本以在评估设置中指定的币种显示。


## <a name="next-steps"></a>后续步骤

[查看](best-practices-assessment.md)创建评估的最佳实践。 
