---
title: 按资源类型移动操作支持
description: 列出了可移动到新资源组或订阅的 Azure 资源类型。
ms.topic: conceptual
ms.date: 02/26/2020
ms.openlocfilehash: 8ab194ad240e4f3e0994314ef9ade3bc7159cf81
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/27/2020
ms.locfileid: "77671840"
---
# <a name="move-operation-support-for-resources"></a>支持移动操作的资源
本文列出了 Azure 资源类型是否支持移动操作。 它还提供有关移动资源时要考虑的特殊情况的信息。

跳转到资源提供程序命名空间：
> [!div class="op_single_selector"]
> - [Microsoft.AAD](#microsoftaad)
> - [microsoft.aadiam](#microsoftaadiam)
> - [Microsoft Advisor](#microsoftadvisor)
> - [Microsoft.AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft.AnalysisServices](#microsoftanalysisservices)
> - [Microsoft.ApiManagement](#microsoftapimanagement)
> - [Microsoft.AppConfiguration](#microsoftappconfiguration)
> - [AppPlatform](#microsoftappplatform)
> - [Microsoft.AppService](#microsoftappservice)
> - [Microsoft.Authorization](#microsoftauthorization)
> - [Microsoft.Automation](#microsoftautomation)
> - [Microsoft.AzureActiveDirectory](#microsoftazureactivedirectory)
> - [AzureData](#microsoftazuredata)
> - [Microsoft.AzureStack](#microsoftazurestack)
> - [Microsoft.Batch](#microsoftbatch)
> - [Microsoft.BatchAI](#microsoftbatchai)
> - [微软帐单](#microsoftbilling)
> - [Microsoft.BingMaps](#microsoftbingmaps)
> - [Microsoft.BizTalkServices](#microsoftbiztalkservices)
> - [Microsoft.Blockchain](#microsoftblockchain)
> - [Microsoft.Blueprint](#microsoftblueprint)
> - [Microsoft.BotService](#microsoftbotservice)
> - [Microsoft.Cache](#microsoftcache)
> - [Microsoft.Cdn](#microsoftcdn)
> - [Microsoft.CertificateRegistration](#microsoftcertificateregistration)
> - [Microsoft.ClassicCompute](#microsoftclassiccompute)
> - [Microsoft.ClassicNetwork](#microsoftclassicnetwork)
> - [Microsoft.ClassicStorage](#microsoftclassicstorage)
> - [Microsoft.CognitiveServices](#microsoftcognitiveservices)
> - [Microsoft.Compute](#microsoftcompute)
> - [Microsoft. 消耗](#microsoftconsumption)
> - [Microsoft.Container](#microsoftcontainer)
> - [Microsoft.ContainerInstance](#microsoftcontainerinstance)
> - [Microsoft.ContainerRegistry](#microsoftcontainerregistry)
> - [Microsoft.ContainerService](#microsoftcontainerservice)
> - [Microsoft.ContentModerator](#microsoftcontentmoderator)
> - [Microsoft.CortanaAnalytics](#microsoftcortanaanalytics)
> - [Microsoft.CostManagement](#microsoftcostmanagement)
> - [Microsoft.CustomerInsights](#microsoftcustomerinsights)
> - [CustomProviders](#microsoftcustomproviders)
> - [Microsoft.DataBox](#microsoftdatabox)
> - [Microsoft.DataBoxEdge](#microsoftdataboxedge)
> - [Microsoft.Databricks](#microsoftdatabricks)
> - [Microsoft.DataCatalog](#microsoftdatacatalog)
> - [Microsoft.DataConnect](#microsoftdataconnect)
> - [Microsoft.DataExchange](#microsoftdataexchange)
> - [Microsoft.DataFactory](#microsoftdatafactory)
> - [Microsoft.DataLake](#microsoftdatalake)
> - [Microsoft.DataLakeAnalytics](#microsoftdatalakeanalytics)
> - [Microsoft.DataLakeStore](#microsoftdatalakestore)
> - [Microsoft.DataMigration](#microsoftdatamigration)
> - [DataProtection](#microsoftdataprotection)
> - [DataShare](#microsoftdatashare)
> - [Microsoft.DBforMariaDB](#microsoftdbformariadb)
> - [Microsoft.DBforMySQL](#microsoftdbformysql)
> - [Microsoft.DBforPostgreSQL](#microsoftdbforpostgresql)
> - [Microsoft.DeploymentManager](#microsoftdeploymentmanager)
> - [Microsoft.Devices](#microsoftdevices)
> - [DevOps](#microsoftdevops)
> - [Microsoft.DevSpaces](#microsoftdevspaces)
> - [Microsoft.DevTestLab](#microsoftdevtestlab)
> - [DigitalTwins](#microsoftdigitaltwins)
> - [Microsoft.DocumentDB](#microsoftdocumentdb)
> - [Microsoft.DomainRegistration](#microsoftdomainregistration)
> - [Microsoft.EnterpriseKnowledgeGraph](#microsoftenterpriseknowledgegraph)
> - [Microsoft.EventGrid](#microsofteventgrid)
> - [Microsoft.EventHub](#microsofteventhub)
> - [Microsoft.Genomics](#microsoftgenomics)
> - [Microsoft.GuestConfiguration](#microsoftguestconfiguration)
> - [Microsoft.HanaOnAzure](#microsofthanaonazure)
> - [Microsoft.HDInsight](#microsofthdinsight)
> - [Microsoft.HealthcareApis](#microsofthealthcareapis)
> - [Microsoft.HybridCompute](#microsofthybridcompute)
> - [Microsoft.HybridData](#microsofthybriddata)
> - [Microsoft.ImportExport](#microsoftimportexport)
> - [microsoft.insights](#microsoftinsights)
> - [Microsoft.IoTCentral](#microsoftiotcentral)
> - [Microsoft.IoTSpaces](#microsoftiotspaces)
> - [Microsoft.KeyVault](#microsoftkeyvault)
> - [Kubernetes](#microsoftkubernetes)
> - [Microsoft.Kusto](#microsoftkusto)
> - [Microsoft.LabServices](#microsoftlabservices)
> - [Microsoft.LocationBasedServices](#microsoftlocationbasedservices)
> - [Microsoft.LocationServices](#microsoftlocationservices)
> - [Microsoft.Logic](#microsoftlogic)
> - [Microsoft.MachineLearning](#microsoftmachinelearning)
> - [Microsoft.MachineLearningCompute](#microsoftmachinelearningcompute)
> - [Microsoft.MachineLearningExperimentation](#microsoftmachinelearningexperimentation)
> - [Microsoft.MachineLearningModelManagement](#microsoftmachinelearningmodelmanagement)
> - [Microsoft.MachineLearningOperationalization](#microsoftmachinelearningoperationalization)
> - [Microsoft.MachineLearningServices](#microsoftmachinelearningservices)
> - [Microsoft.ManagedIdentity](#microsoftmanagedidentity)
> - [ManagedServices](#microsoftmanagedservices)
> - [Microsoft.Maps](#microsoftmaps)
> - [Microsoft.MarketplaceApps](#microsoftmarketplaceapps)
> - [Microsoft.Media](#microsoftmedia)
> - [Microservices4Spring](#microsoftmicroservices4spring)
> - [Microsoft.Migrate](#microsoftmigrate)
> - [Microsoft.NetApp](#microsoftnetapp)
> - [Microsoft.Network](#microsoftnetwork)
> - [Microsoft.NotificationHubs](#microsoftnotificationhubs)
> - [ObjectStore](#microsoftobjectstore)
> - [Microsoft.OperationalInsights](#microsoftoperationalinsights)
> - [Microsoft.OperationsManagement](#microsoftoperationsmanagement)
> - [Microsoft.Peering](#microsoftpeering)
> - [Microsoft.PolicyInsights](#microsoftpolicyinsights)
> - [Microsoft.Portal](#microsoftportal)
> - [Microsoft.PortalSdk](#microsoftportalsdk)
> - [Microsoft.PowerBI](#microsoftpowerbi)
> - [Microsoft.PowerBIDedicated](#microsoftpowerbidedicated)
> - [ProjectBabylon](#microsoftprojectbabylon)
> - [Microsoft.ProjectOxford](#microsoftprojectoxford)
> - [ProviderHub](#microsoftproviderhub)
> - [Microsoft.RecoveryServices](#microsoftrecoveryservices)
> - [Microsoft.Relay](#microsoftrelay)
> - [ResourceGraph](#microsoftresourcegraph)
> - [ResourceHealth](#microsoftresourcehealth)
> - [Microsoft.Resources](#microsoftresources)
> - [Microsoft.SaaS](#microsoftsaas)
> - [Microsoft.Search](#microsoftsearch)
> - [Microsoft.Security](#microsoftsecurity)
> - [SecurityInsights](#microsoftsecurityinsights)
> - [Microsoft.ServerManagement](#microsoftservermanagement)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft.ServiceFabric](#microsoftservicefabric)
> - [Microsoft.ServiceFabricMesh](#microsoftservicefabricmesh)
> - [Microsoft. 服务](#microsoftservices)
> - [Microsoft.SignalRService](#microsoftsignalrservice)
> - [SoftwarePlan](#microsoftsoftwareplan)
> - [Microsoft.Solutions](#microsoftsolutions)
> - [Microsoft.Sql](#microsoftsql)
> - [Microsoft.SqlVirtualMachine](#microsoftsqlvirtualmachine)
> - [Microsoft.SqlVM](#microsoftsqlvm)
> - [Microsoft.Storage](#microsoftstorage)
> - [Microsoft.StorageSync](#microsoftstoragesync)
> - [Microsoft.StorageSyncDev](#microsoftstoragesyncdev)
> - [Microsoft.StorageSyncInt](#microsoftstoragesyncint)
> - [Microsoft.StorSimple](#microsoftstorsimple)
> - [Microsoft.StreamAnalytics](#microsoftstreamanalytics)
> - [Microsoft.StreamAnalyticsExplorer](#microsoftstreamanalyticsexplorer)
> - [Microsoft. 订阅](#microsoftsubscription)
> - [microsoft.support](#microsoftsupport)
> - [Microsoft.TerraformOSS](#microsoftterraformoss)
> - [Microsoft.TimeSeriesInsights](#microsofttimeseriesinsights)
> - [Microsoft.Token](#microsofttoken)
> - [microsoft.visualstudio](#microsoftvisualstudio)
> - [Microsoft.VMwareCloudSimple](#microsoftvmwarecloudsimple)
> - [VSOnline](#microsoftvsonline)
> - [Microsoft.Web](#microsoftweb)
> - [Microsoft.WindowsIoT](#microsoftwindowsiot)
> - [WorkloadMonitor](#microsoftworkloadmonitor)

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | domainservices | 否 | 否 |

## <a name="microsoftaadiam"></a>microsoft.aadiam

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | tenants | 否 | 否 |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | 配置 | 否 | 否 |
> | 建议 | 否 | 否 |
> | 禁止显示 | 否 | 否 |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | actionrules | 是 | 是 |
> | alerts | 否 | 否 |
> | alertssummary | 否 | 否 |
> | smartdetectoralertrules | 是 | 是 |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | 服务器 | 是 | 是 |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | 服务 | 是 | 是 |

## <a name="microsoftappconfiguration"></a>Microsoft.AppConfiguration

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | configurationstores | 是 | 是 |

## <a name="microsoftappplatform"></a>AppPlatform

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | 草绿色 | 是 | 是 |

## <a name="microsoftappservice"></a>Microsoft.AppService

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | apiapps | 否 | 否 |
> | appidentities | 否 | 否 |
> | gateways | 否 | 否 |

> [!IMPORTANT]
> 请参阅[应用服务移动指南](./move-limitations/app-service-move-limitations.md)。

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | checkaccess | 否 | 否 |
> | denyassignments | 否 | 否 |
> | findorphanroleassignments | 否 | 否 |
> | 锁定 | 否 | 否 |
> | 权限 | 否 | 否 |
> | policyassignments | 否 | 否 |
> | policydefinitions | 否 | 否 |
> | policysetdefinitions | 否 | 否 |
> | roleassignments | 否 | 否 |
> | roleassignmentsusagemetrics | 否 | 否 |
> | roledefinitions | 否 | 否 |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | automationaccounts | 是 | 是 |
> | automationaccounts/配置 | 是 | 是 |
> | automationaccounts/runbook | 是 | 是 |

> [!IMPORTANT]
> Runbook 必须与自动化帐户位于同一资源组中。

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | b2cdirectories | 是 | 是 |

## <a name="microsoftazuredata"></a>AzureData

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | hybriddatamanagers | 否 | 否 |
> | postgresinstances | 否 | 否 |
> | sqlbigdataclusters | 否 | 否 |
> | sqlinstances | 否 | 否 |
> | sqlserverregistrations | 是 | 是 |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | registrations | 是 | 是 |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | batchaccounts | 是 | 是 |

## <a name="microsoftbatchai"></a>Microsoft.BatchAI

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | clusters | 否 | 否 |
> | fileservers | 否 | 否 |
> | jobs | 否 | 否 |
> | workspaces | 否 | 否 |

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | billingperiods | 否 | 否 |
> | billingpermissions | 否 | 否 |
> | billingroleassignments | 否 | 否 |
> | billingroledefinitions | 否 | 否 |
> | createbillingroleassignment | 否 | 否 |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | mapapis | 否 | 否 |

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | BizTalk | 否 | 否 |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | blockchainmembers | 否 | 否 |
> | 观看者 | 否 | 否 |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | blueprintassignments | 否 | 否 |
> | blueprints | 否 | 否 |

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | botservices | 是 | 是 |

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | redis | 是 | 是 |

> [!IMPORTANT]
> 如果为 Redis 实例的 Azure Cache 配置了虚拟网络，则无法将该实例移到不同的订阅。 请参阅[网络移动限制](./move-limitations/networking-move-limitations.md)。

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | cdnwebapplicationfirewallpolicies | 是 | 是 |
> | 配置文件 | 是 | 是 |
> | 配置文件/终结点 | 是 | 是 |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | certificateorders | 是 | 是 |

> [!IMPORTANT]
> 请参阅[应用服务移动指南](./move-limitations/app-service-move-limitations.md)。

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | domainnames | 是 | 否 |
> | virtualmachines | 是 | 否 |

> [!IMPORTANT]
> 请参阅[经典部署移动指南](./move-limitations/classic-model-move-limitations.md)。 可以使用特定于该方案的操作跨订阅移动经典部署资源。

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | networksecuritygroups | 否 | 否 |
> | reservedips | 否 | 否 |
> | virtualnetworks | 否 | 否 |

> [!IMPORTANT]
> 请参阅[经典部署移动指南](./move-limitations/classic-model-move-limitations.md)。 可以使用特定于该方案的操作跨订阅移动经典部署资源。

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | storageaccounts | 是 | 否 |

> [!IMPORTANT]
> 请参阅[经典部署移动指南](./move-limitations/classic-model-move-limitations.md)。 可以使用特定于该方案的操作跨订阅移动经典部署资源。

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | accounts | 是 | 是 |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | availabilitysets | 是 | 是 |
> | diskencryptionsets | 否 | 否 |
> | disks | 是 | 是 |
> | galleries | 否 | 否 |
> | 库/图像 | 否 | 否 |
> | 库/图像/版本 | 否 | 否 |
> | hostgroups | 否 | 否 |
> | hostgroups/主机 | 否 | 否 |
> | images | 是 | 是 |
> | proximityplacementgroups | 否 | 否 |
> | restorepointcollections | 否 | 否 |
> | sharedvmimages | 否 | 否 |
> | sharedvmimages/版本 | 否 | 否 |
> | snapshots | 是 | 是 |
> | virtualmachines | 是 | 是 |
> | virtualmachines/extensions | 是 | 是 |
> | virtualmachinescalesets | 是 | 是 |

> [!IMPORTANT]
> 请参阅[虚拟机移动指南](./move-limitations/virtual-machines-move-limitations.md)。

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | aggregatedcost | 否 | 否 |
> | 平衡 | 否 | 否 |
> | 预算 | 否 | 否 |
> | 话费 | 否 | 否 |
> | costtags | 否 | 否 |
> | credits | 否 | 否 |
> | 活动 | 否 | 否 |
> | 预测 | 否 | 否 |
> | lots | 否 | 否 |
> | 市场 | 否 | 否 |
> | operationresults | 否 | 否 |
> | operationstatus | 否 | 否 |
> | pricesheets | 否 | 否 |
> | products | 否 | 否 |
> | reservationdetails | 否 | 否 |
> | reservationrecommendations | 否 | 否 |
> | reservationsummaries | 否 | 否 |
> | reservationtransactions | 否 | 否 |
> | 标记 | 否 | 否 |
> | tenants | 否 | 否 |
> | terms | 否 | 否 |
> | usagedetails | 否 | 否 |

## <a name="microsoftcontainer"></a>Microsoft.Container

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | containergroups | 否 | 否 |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | containergroups | 否 | 否 |
> | serviceassociationlinks | 否 | 否 |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | registries | 是 | 是 |
> | 注册表/buildtasks | 是 | 是 |
> | 注册表/复制 | 是 | 是 |
> | 注册表/taskruns | 是 | 是 |
> | 注册表/任务 | 是 | 是 |
> | 注册表/webhook | 是 | 是 |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | containerservices | 否 | 否 |
> | managedclusters | 否 | 否 |
> | openshiftmanagedclusters | 否 | 否 |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | 应用程序 | 否 | 否 |

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | accounts | 否 | 否 |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | 警报 | 否 | 否 |
> | 预算 | 否 | 否 |
> | 连接器 | 是 | 是 |
> | 维数 | 否 | 否 |
> | 导出 | 否 | 否 |
> | externalsubscriptions | 否 | 否 |
> | 预测 | 否 | 否 |
> | query | 否 | 否 |
> | reportconfigs | 否 | 否 |
> | 报表 | 否 | 否 |
> | showbackrules | 否 | 否 |
> | 视图 | 否 | 否 |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | hubs | 否 | 否 |

## <a name="microsoftcustomproviders"></a>Microsoft.CustomProviders

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | 关联 | 否 | 否 |
> | resourceproviders | 是 | 是 |

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | jobs | 否 | 否 |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | databoxedgedevices | 否 | 否 |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | workspaces | 否 | 否 |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | catalogs | 是 | 是 |
> | datacatalogs | 否 | 否 |

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | connectionmanagers | 否 | 否 |

## <a name="microsoftdataexchange"></a>Microsoft.DataExchange

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | packages | 否 | 否 |
> | plans | 否 | 否 |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | datafactories | 是 | 是 |
> | factories | 是 | 是 |

## <a name="microsoftdatalake"></a>Microsoft.DataLake

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | datalakeaccounts | 否 | 否 |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | accounts | 是 | 是 |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | accounts | 是 | 是 |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | services | 否 | 否 |
> | 服务/项目 | 否 | 否 |
> | slots | 否 | 否 |

## <a name="microsoftdataprotection"></a>DataProtection

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | backupvaults | 否 | 否 |

## <a name="microsoftdatashare"></a>DataShare

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | accounts | 是 | 是 |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | 服务器 | 是 | 是 |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | 服务器 | 是 | 是 |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | servergroups | 否 | 否 |
> | 服务器 | 是 | 是 |
> | serversv2 | 是 | 是 |

## <a name="microsoftdeploymentmanager"></a>Microsoft.DeploymentManager

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | artifactsources | 是 | 是 |
> | rollouts | 是 | 是 |
> | servicetopologies | 是 | 是 |
> | servicetopologies/服务 | 是 | 是 |
> | servicetopologies/services/serviceunits | 是 | 是 |
> | steps | 是 | 是 |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | elasticpools | 否 | 否 |
> | elasticpools / iothubtenants | 否 | 否 |
> | iothubs | 是 | 是 |
> | provisioningservices | 是 | 是 |

## <a name="microsoftdevops"></a>DevOps

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | 段 | 是 | 是 |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | 控制器 | 是 | 是 |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | labcenters | 否 | 否 |
> | labs | 是 | 否 |
> | 实验室/环境 | 是 | 是 |
> | 实验室/servicerunners | 是 | 是 |
> | 实验室/virtualmachines | 是 | 否 |
> | schedules | 是 | 是 |

## <a name="microsoftdigitaltwins"></a>DigitalTwins

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | digitaltwinsinstances | 否 | 否 |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | databaseaccounts | 是 | 是 |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | domains | 是 | 是 |

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft.EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | services | 是 | 是 |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | domains | 是 | 是 |
> | eventSubscriptions | 不是-无法独立移动，但会自动移动到订阅的资源。 | 不是-无法独立移动，但会自动移动到订阅的资源。 |
> | eventsubscriptions | 不是-无法独立移动，但会自动移动到订阅的资源。 | 不是-无法独立移动，但会自动移动到订阅的资源。 |
> | extensiontopics | 否 | 否 |
> | topics | 是 | 是 |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | clusters | 是 | 是 |
> | namespaces | 是 | 是 |

## <a name="microsoftgenomics"></a>Microsoft.Genomics

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | accounts | 否 | 否 |

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | guestconfigurationassignments | 否 | 否 |
> | software | 否 | 否 |
> | softwareupdateprofile | 否 | 否 |
> | softwareupdates | 否 | 否 |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | hanainstances | 否 | 否 |
> | sapmonitors | 是 | 是 |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | clusters | 是 | 是 |

> [!IMPORTANT]
> 可以将 HDInsight 群集移到新的订阅或资源组。 但是，无法在订阅之间移动链接到 HDInsight 群集的网络资源（例如虚拟网络、NIC 或负载均衡器）。 此外，无法将连接到群集的虚拟机的 NIC 移到新的资源组。
>
> 将 HDInsight 群集移到新的订阅时，首先移动其他资源（如存储帐户）。 然后移动 HDInsight 群集本身。

## <a name="microsofthealthcareapis"></a>Microsoft.HealthcareApis

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | services | 是 | 是 |

## <a name="microsofthybridcompute"></a>Microsoft.HybridCompute

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | 造 | 是 | 是 |
> | 计算机/扩展 | 否 | 否 |

## <a name="microsofthybriddata"></a>Microsoft.HybridData

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | datamanagers | 是 | 是 |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | jobs | 是 | 是 |

## <a name="microsoftinsights"></a>microsoft.insights

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | actiongroups | 是 | 是 |
> | activitylogalerts | 否 | 否 |
> | alertrules | 是 | 是 |
> | autoscalesettings | 是 | 是 |
> | baseline | 否 | 否 |
> | calculatebaseline | 否 | 否 |
> | components | 是 | 是 |
> | diagnosticsettings | 否 | 否 |
> | diagnosticsettingscategories | 否 | 否 |
> | eventtypes | 否 | 否 |
> | extendeddiagnosticsettings | 否 | 否 |
> | logdefinitions | 否 | 否 |
> | 日志 | 否 | 否 |
> | metricalerts | 否 | 否 |
> | metricbaselines | 否 | 否 |
> | metricdefinitions | 否 | 否 |
> | metricnamespaces | 否 | 否 |
> | metrics | 否 | 否 |
> | myworkbooks | 否 | 否 |
> | privatelinkscopes | 是 | 是 |
> | scheduledqueryrules | 是 | 是 |
> | 拓扑 | 否 | 否 |
> | 事务 | 否 | 否 |
> | vminsightsonboardingstatuses | 否 | 否 |
> | webtests | 是 | 是 |
> | workbooks | 是 | 是 |
> | workbooktemplates | 是 | 是 |

> [!IMPORTANT]
> 请确保移动到新订阅不超过[订阅配额](azure-subscription-service-limits.md#azure-monitor-limits)。

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | iotapps | 是 | 是 |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | checknameavailability | 是 | 是 |
> | graph | 是 | 是 |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | vaults | 是 | 是 |

> [!IMPORTANT]
> 用于磁盘加密的密钥保管库不能移到同一订阅中的资源组或跨订阅。

## <a name="microsoftkubernetes"></a>Kubernetes

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | connectedclusters | 否 | 否 |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | clusters | 是 | 是 |

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | labaccounts | 否 | 否 |

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | accounts | 否 | 否 |

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | accounts | 否 | 否 |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | hostingenvironments | 否 | 否 |
> | integrationaccounts | 是 | 是 |
> | integrationserviceenvironments | 是 | 否 |
> | integrationserviceenvironments / managedapis | 是 | 否 |
> | isolatedenvironments | 否 | 否 |
> | workflows | 是 | 是 |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | commitmentplans | 是 | 是 |
> | webservices | 是 | 否 |
> | workspaces | 是 | 是 |

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | operationalizationclusters | 否 | 否 |

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | accounts | 否 | 否 |
> | 帐户/工作区 | 否 | 否 |
> | 帐户/工作区/项目 | 否 | 否 |
> | teamaccounts | 否 | 否 |
> | teamaccounts/工作区 | 否 | 否 |
> | teamaccounts/工作区/项目 | 否 | 否 |

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | accounts | 否 | 否 |

## <a name="microsoftmachinelearningoperationalization"></a>Microsoft.MachineLearningOperationalization

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | hostingaccounts | 否 | 否 |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | workspaces | 否 | 否 |
> | 工作区/计算 | 否 | 否 |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | 标识 | 否 | 否 |
> | userassignedidentities | 否 | 否 |

## <a name="microsoftmanagedservices"></a>ManagedServices

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | registrationassignments | 否 | 否 |
> | registrationdefinitions | 否 | 否 |

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | accounts | 是 | 是 |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | classicdevservices | 否 | 否 |

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | mediaservices | 是 | 是 |
> | windowsazure.mediaservices/liveevents | 是 | 是 |
> | windowsazure.mediaservices/streamingendpoint | 是 | 是 |

## <a name="microsoftmicroservices4spring"></a>Microservices4Spring

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | appclusters | 否 | 否 |

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | assessmentprojects | 是 | 是 |
> | migrateprojects | 是 | 是 |
> | projects | 否 | 否 |

## <a name="microsoftnetapp"></a>Microsoft.NetApp

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | netappaccounts | 否 | 否 |
> | netappaccounts / backuppolicies | 否 | 否 |
> | netappaccounts / capacitypools | 否 | 否 |
> | netappaccounts/capacitypools/卷 | 否 | 否 |
> | netappaccounts/capacitypools/卷集/mounttargets | 否 | 否 |
> | netappaccounts/capacitypools/卷/快照 | 否 | 否 |

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | applicationgateways | 否 | 否 |
> | applicationgatewaywebapplicationfirewallpolicies | 否 | 否 |
> | applicationsecuritygroups | 是 | 是 |
> | azurefirewalls | 是 | 是 |
> | bastionhosts | 否 | 否 |
> | connections | 是 | 是 |
> | ddoscustompolicies | 是 | 是 |
> | ddosprotectionplans | 否 | 否 |
> | dnszones | 是 | 是 |
> | expressroutecircuits | 否 | 否 |
> | expressroutegateways | 否 | 否 |
> | firewallpolicies | 是 | 是 |
> | frontdoors | 否 | 否 |
> | frontdoorwebapplicationfirewallpolicies | 否 | 否 |
> | ipgroups | 是 | 是 |
> | loadbalancers | 是-基本 SKU<br>无标准 SKU | 是-基本 SKU<br>无标准 SKU |
> | localnetworkgateways | 是 | 是 |
> | networkexperimentprofiles | 是 | 是 |
> | networkintentpolicies | 是 | 是 |
> | networkinterfaces | 是 | 是 |
> | networkprofiles | 否 | 否 |
> | networksecuritygroups | 是 | 是 |
> | networkwatchers | 是 | 否 |
> | networkwatchers / connectionmonitors | 是 | 否 |
> | networkwatchers / flowlogs | 是 | 否 |
> | networkwatchers/重用功能区 | 是 | 否 |
> | networkwatchers / pingmeshes | 是 | 否 |
> | p2svpngateways | 否 | 否 |
> | privatednszones | 是 | 是 |
> | privatednszones / virtualnetworklinks | 是 | 是 |
> | privateendpointredirectmaps | 否 | 否 |
> | privateendpoints | 否 | 否 |
> | privatelinkservices | 否 | 否 |
> | publicipaddresses | 是-基本 SKU<br>无标准 SKU | 是-基本 SKU<br>无标准 SKU |
> | publicipprefixes | 是 | 是 |
> | routefilters | 否 | 否 |
> | routetables | 是 | 是 |
> | serviceendpointpolicies | 是 | 是 |
> | trafficmanagerprofiles | 是 | 是 |
> | virtualhubs | 否 | 否 |
> | virtualnetworkgateways | 是 | 是 |
> | virtualnetworks | 是 | 是 |
> | virtualnetworktaps | 否 | 否 |
> | virtualrouters | 是 | 是 |
> | virtualwans | 否 | 否 |
> | vpngateways （虚拟 WAN） | 否 | 否 |
> | vpnserverconfigurations | 否 | 否 |
> | vpnsites （虚拟 WAN） | 否 | 否 |
> | webapplicationfirewallpolicies | 是 | 是 |

> [!IMPORTANT]
> 请参阅[网络移动指南](./move-limitations/networking-move-limitations.md)。

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | namespaces | 是 | 是 |
> | 命名空间/notificationhubs | 是 | 是 |

## <a name="microsoftobjectstore"></a>ObjectStore

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | osnamespaces | 是 | 是 |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | storageinsightconfigs | 否 | 否 |
> | workspaces | 是 | 是 |

> [!IMPORTANT]
> 请确保移动到新订阅不超过[订阅配额](azure-subscription-service-limits.md#azure-monitor-limits)。

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | managementassociations | 否 | 否 |
> | managementconfigurations | 是 | 是 |
> | solutions | 是 | 是 |
> | 视图 | 是 | 是 |

## <a name="microsoftpeering"></a>Microsoft.Peering

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | 对等互连 | 是 | 是 |
> | peeringservices | 否 | 否 |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | policyevents | 否 | 否 |
> | policystates | 否 | 否 |
> | policytrackedresources | 否 | 否 |
> | remediations | 否 | 否 |

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | dashboards | 是 | 是 |

## <a name="microsoftportalsdk"></a>Microsoft.PortalSdk

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | rootresources | 否 | 否 |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | workspacecollections | 是 | 是 |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | capacities | 是 | 是 |

## <a name="microsoftprojectbabylon"></a>ProjectBabylon

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | accounts | 否 | 否 |

## <a name="microsoftprojectoxford"></a>Microsoft.ProjectOxford

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | accounts | 否 | 否 |

## <a name="microsoftproviderhub"></a>ProviderHub

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | rollouts | 否 | 否 |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | backupprotecteditems | 否 | 否 |
> | replicationeligibilityresults | 否 | 否 |
> | vaults | 是 | 是 |

> [!IMPORTANT]
> 请参阅[恢复服务移动指南](../../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json)。

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | namespaces | 是 | 是 |

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | 查询 | 是 | 是 |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | availabilitystatuses | 否 | 否 |
> | childavailabilitystatuses | 否 | 否 |
> | childresources | 否 | 否 |
> | 活动 | 否 | 否 |
> | 通知 | 否 | 否 |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | deploymentscripts | 否 | 否 |
> | 链接 | 否 | 否 |
> | 标记 | 否 | 否 |

## <a name="microsoftsaas"></a>Microsoft.SaaS

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | 应用程序 | 是 | 否 |

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | searchservices | 是 | 是 |

> [!IMPORTANT]
> 不能在一个操作中将多个搜索资源移动到不同区域。 只能通过多个单独的操作移动它们。

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | adaptivenetworkhardenings | 否 | 否 |
> | advancedthreatprotectionsettings | 否 | 否 |
> | assessmentmetadata | 否 | 否 |
> | 个评估 | 否 | 否 |
> | 自动化 | 是 | 是 |
> | complianceresults | 否 | 否 |
> | 符合性要求 | 否 | 否 |
> | datacollectionagents | 否 | 否 |
> | devicesecuritygroups | 否 | 否 |
> | informationprotectionpolicies | 否 | 否 |
> | iotsecuritysolutions | 是 | 是 |
> | servervulnerabilityassessments | 否 | 否 |

## <a name="microsoftsecurityinsights"></a>SecurityInsights

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | aggregations | 否 | 否 |
> | alertrules | 否 | 否 |
> | alertruletemplates | 否 | 否 |
> | 书签 | 否 | 否 |
> | cases | 否 | 否 |
> | dataconnectors | 否 | 否 |
> | dataconnectorscheckrequirements | 否 | 否 |
> | 实体 | 否 | 否 |
> | entityqueries | 否 | 否 |
> | 发生 | 否 | 否 |
> | officeconsents | 否 | 否 |
> | 设置 | 否 | 否 |

## <a name="microsoftservermanagement"></a>Microsoft.ServerManagement

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | gateways | 否 | 否 |
> | 节点 | 否 | 否 |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | namespaces | 是 | 是 |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | 应用程序 | 否 | 否 |
> | clusters | 是 | 是 |
> | 群集/应用程序 | 否 | 否 |
> | containergroups | 否 | 否 |
> | containergroupsets | 否 | 否 |
> | edgeclusters | 否 | 否 |
> | managedclusters | 否 | 否 |
> | networks | 否 | 否 |
> | secretstores | 否 | 否 |
> | volumes | 否 | 否 |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | 应用程序 | 是 | 是 |
> | gateways | 是 | 是 |
> | networks | 是 | 是 |
> | 机密 | 是 | 是 |
> | volumes | 是 | 是 |

## <a name="microsoftservices"></a>Microsoft. 服务

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | rollouts | 否 | 否 |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | signalr | 是 | 是 |

## <a name="microsoftsoftwareplan"></a>SoftwarePlan

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | hybridusebenefits | 否 | 否 |

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | applicationdefinitions | 否 | 否 |
> | 应用程序 | 否 | 否 |
> | jitrequests | 否 | 否 |

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | instancepools | 否 | 否 |
> | managedinstances | 否 | 否 |
> | managedinstances/数据库 | 否 | 否 |
> | 服务器 | 是 | 是 |
> | 服务器/数据库 | 是 | 是 |
> | 服务器/elasticpools | 是 | 是 |
> | 服务器/jobaccounts | 是 | 是 |
> | 服务器/jobagents | 是 | 是 |
> | virtualclusters | 是 | 是 |

> [!IMPORTANT]
> 数据库和服务器必须位于同一个资源组中。 当移动 SQL 服务器时，其所有数据库也会一起移动。 此行为适用于 Azure SQL 数据库和 Azure SQL 数据仓库数据库。

## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | sqlvirtualmachinegroups | 是 | 是 |
> | sqlvirtualmachines | 是 | 是 |

## <a name="microsoftsqlvm"></a>Microsoft.SqlVM

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | dwvm | 否 | 否 |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | storageaccounts | 是 | 是 |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | storagesyncservices | 是 | 是 |

## <a name="microsoftstoragesyncdev"></a>Microsoft.StorageSyncDev

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | storagesyncservices | 否 | 否 |

## <a name="microsoftstoragesyncint"></a>Microsoft.StorageSyncInt

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | storagesyncservices | 否 | 否 |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | managers | 否 | 否 |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | streamingjobs | 是 | 是 |

> [!IMPORTANT]
> 当流分析作业处于运行状态时，无法将其移动。

## <a name="microsoftstreamanalyticsexplorer"></a>Microsoft.StreamAnalyticsExplorer

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | environments | 否 | 否 |
> | 环境/eventsources | 否 | 否 |
> | 实例 | 否 | 否 |
> | 实例/环境 | 否 | 否 |
> | 实例/环境/eventsources | 否 | 否 |

## <a name="microsoftsubscription"></a>Microsoft.Subscription

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | createsubscription | 否 | 否 |

## <a name="microsoftsupport"></a>microsoft.support

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | supporttickets | 否 | 否 |

## <a name="microsoftterraformoss"></a>Microsoft.TerraformOSS

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | providerregistrations | 否 | 否 |
> | 资源 | 否 | 否 |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | environments | 是 | 是 |
> | 环境/eventsources | 是 | 是 |
> | 环境/referencedatasets | 是 | 是 |

## <a name="microsofttoken"></a>Microsoft.Token

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | 区内 | 是 | 是 |

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | account | 否 | 否 |
> | 帐户/扩展 | 是 | 是 |
> | 帐户/项目 | 是 | 是 |

> [!IMPORTANT]
> 若要更改 Azure DevOps 的订阅，请参阅[更改用于计费的 Azure 订阅](/azure/devops/organizations/billing/change-azure-subscription?toc=/azure/azure-resource-manager/toc.json)。

## <a name="microsoftvmwarecloudsimple"></a>Microsoft.VMwareCloudSimple

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | dedicatedcloudnodes | 否 | 否 |
> | dedicatedcloudservices | 否 | 否 |
> | virtualmachines | 否 | 否 |

## <a name="microsoftvsonline"></a>VSOnline

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | accounts | 是 | 是 |
> | plans | 是 | 是 |

## <a name="microsoftweb"></a>Microsoft.Web

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | certificates | 否 | 是 |
> | connectiongateways | 是 | 是 |
> | connections | 是 | 是 |
> | customapis | 是 | 是 |
> | hostingenvironments | 否 | 否 |
> | serverfarms | 是 | 是 |
> | sites | 是 | 是 |
> | sites/premieraddons | 是 | 是 |
> | 站点/槽 | 是 | 是 |
> | staticsites | 否 | 否 |

> [!IMPORTANT]
> 请参阅[应用服务移动指南](./move-limitations/app-service-move-limitations.md)。

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | deviceservices | 否 | 否 |

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tableFixed"]
> | 资源类型 | 资源组 | 订阅 |
> | ------------- | ----------- | ---------- |
> | components | 否 | 否 |
> | monitorinstances | 否 | 否 |
> | monitors | 否 | 否 |
> | notificationsettings | 否 | 否 |

## <a name="third-party-services"></a>第三方服务

第三方服务当前不支持移动操作。

## <a name="next-steps"></a>后续步骤
有关移动资源的命令，请参阅[将资源移动至新资源组或订阅](move-resource-group-and-subscription.md)。

若要以逗号分隔值文件的形式获取同一数据，请下载 [move-support-resources.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/move-support-resources.csv)。
