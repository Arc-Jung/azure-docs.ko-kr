---
title: Azure 서비스에서 리소스 공급자 Azure Resource Manager
description: Azure Resource Manager에 대 한 모든 리소스 공급자 네임 스페이스를 나열 하 고 해당 네임 스페이스에 대 한 Azure 서비스를 표시 합니다.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: tomfitz
ms.openlocfilehash: b500a7b2c74acbd4e035e710f7e25f1aacd86126
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70194855"
---
# <a name="resource-providers-for-azure-services"></a>Azure 서비스에 대 한 리소스 공급자

이 문서에서는 리소스 공급자 네임 스페이스를 Azure 서비스에 매핑하는 방법을 보여 줍니다.

## <a name="match-resource-provider-to-service"></a>리소스 공급자와 서비스 일치

| 리소스 공급자 네임 스페이스 | Azure 서비스 |
| --------------------------- | ------------- |
| Microsoft.AAD | [Azure Active Directory Domain Services](../active-directory-domain-services/index.yml) |
| microsoft.aadiam | [Azure Active Directory](/azure/active-directory/) |
| Microsoft.Addons | core |
| Microsoft.ADHybridHealthService | [Azure Active Directory](/azure/active-directory/) |
| Microsoft.Advisor | [Azure Advisor](../advisor/index.yml) |
| Microsoft.AlertsManagement | [Azure Monitor](../azure-monitor/index.yml) |
| Microsoft.AnalysisServices | [Azure Analysis Services](/azure/analysis-services/) |
| Microsoft.ApiManagement | [API Management](../api-management/index.yml) |
| Microsoft.AppConfiguration | core |
| Microsoft.Authorization | [Azure Resource Manager](index.yml) |
| Microsoft.Automation | [자동화](../automation/index.yml) |
| Microsoft.AzureActiveDirectory | [Azure Active Directory B2C](../active-directory-b2c/index.yml) |
| Microsoft.AzureStack | core |
| Microsoft.Batch | [일괄 처리](../batch/index.yml) |
| Microsoft.Billing | [Billing](/azure/billing/) |
| Microsoft.BingMaps | [Bing 지도](https://docs.microsoft.com/BingMaps/#pivot=main&panel=BingMapsAPI) |
| Microsoft.BizTalkServices | [BizTalk Services](../logic-apps/logic-apps-move-from-mabs.md) |
| Microsoft.Blockchain | [Azure Blockchain 서비스](/azure/blockchain/workbench/) |
| Microsoft.Blueprint | [Azure 청사진](/azure/governance/blueprints/) |
| Microsoft.BotService | [Azure Bot Service](/azure/bot-service/) |
| Microsoft.Cache | [Azure Cache for Redis](/azure/azure-cache-for-redis/) |
| Microsoft.Capacity | core |
| Microsoft.Cdn | [Content Delivery Network](../cdn/index.yml) |
| Microsoft.CertificateRegistration | [인증서 App Service](../app-service/web-sites-purchase-ssl-web-site.md) |
| Microsoft.ClassicCompute | 클래식 배포 모델 가상 컴퓨터 |
| Microsoft.ClassicInfrastructureMigrate | 클래식 배포 모델 마이그레이션 |
| Microsoft.ClassicNetwork | 클래식 배포 모델 가상 네트워크 |
| Microsoft.ClassicStorage | 클래식 배포 모델 저장소 |
| Microsoft.ClassicSubscription | 클래식 배포 모델 |
| Microsoft.CognitiveServices | [Cognitive Services](/azure/cognitive-services/) |
| Microsoft.Commerce | core |
| Microsoft.Compute | [Virtual Machines](/azure/virtual-machines/) |
| Microsoft.Consumption | [Cost Management](/azure/cost-management/) |
| Microsoft.ContainerInstance | [Container Instances](/azure/container-instances/) |
| Microsoft.ContainerRegistry | [Container Registry](/azure/container-registry/) |
| Microsoft.ContainerService | [AKS(Azure Kubernetes Service)](/azure/aks/) |
| Microsoft.ContentModerator | [Azure Content Moderator](../cognitive-services/content-moderator/index.yml) |
| Microsoft.CostManagement | [Cost Management](/azure/cost-management/) |
| Microsoft.CustomerInsights | Customer Insights |
| Microsoft.CustomerLockbox | Microsoft Azure에 대 한 고객 Lockbox |
| Microsoft.CustomProviders | [Azure 사용자 지정 공급자](../managed-applications/custom-providers-overview.md) |
| Microsoft.DataBox | [Azure Data Box](/azure/databox-family/) |
| Microsoft.DataBoxEdge | [Azure Data Box Edge](../databox-online/data-box-edge-overview.md) |
| Microsoft.Databricks | [Azure Databricks](/azure/azure-databricks/) |
| Microsoft.DataCatalog | [Data Catalog](/azure/data-catalog/) |
| Microsoft.DataFactory | [Data Factory](/azure/data-factory/) |
| Microsoft.DataLakeAnalytics | [Data Lake Analytics](/azure/data-lake-analytics/) |
| Microsoft.DataLakeStore | [Azure Data Lake Store](../storage/blobs/data-lake-storage-introduction.md) |
| Microsoft.DataMigration | [Azure Database Migration Service](/azure/dms/) |
| Microsoft.DBforMariaDB | [Azure Database for MariaDB](/azure/mariadb/) |
| Microsoft.DBforMySQL | [Azure Database for MySQL](/azure/mysql/) |
| Microsoft.DBforPostgreSQL | [Azure Database for PostgreSQL](/azure/postgresql/) |
| Microsoft.DeploymentManager | [Azure 배포 관리자](deployment-manager-overview.md) |
| Microsoft.Devices | [IoT Hub](/azure/iot-hub/)<br />[IoT Hub Device Provisioning Service](/azure/iot-dps/) |
| Microsoft.DevSpaces | [Azure Dev Spaces](/azure/dev-spaces/) |
| Microsoft.DevTestLab | [Azure Lab Services](../lab-services/index.yml) |
| Microsoft.DocumentDB | [Azure Cosmos DB](../cosmos-db/index.yml) |
| Microsoft.DomainRegistration | [App Service](/azure/app-service/) |
| Microsoft.EnterpriseKnowledgeGraph | 엔터프라이즈 지식 그래프 |
| Microsoft.EventGrid | [Event Grid](/azure/event-grid/) |
| Microsoft.EventHub | [Event Hubs](../event-hubs/index.yml) |
| Microsoft.Features | [Azure Resource Manager](index.yml) |
| Microsoft.Genomics | [Microsoft Genomics](/azure/genomics/) |
| Microsoft.GuestConfiguration | [Azure Policy](../governance/policy/index.yml) |
| Microsoft.HanaOnAzure | [Azure의 SAP HANA](../virtual-machines/workloads/sap/hana-overview-architecture.md) |
| Microsoft.HardwareSecurityModules | [Azure 전용 HSM](../dedicated-hsm/index.yml) |
| Microsoft.HDInsight | [HDInsight](../hdinsight/index.yml) |
| Microsoft.HealthcareApis | [Azure API for FHIR](../healthcare-apis/index.yml) |
| Microsoft.ImportExport | [Azure 가져오기/내보내기](../storage/common/storage-import-export-service.md) |
| microsoft.insights | [Azure Monitor](../azure-monitor/index.yml) |
| Microsoft.Intune | [Intune](/intune/) |
| Microsoft.IoTCentral | [IoT Central](/azure/iot-central/) |
| Microsoft.IoTSpaces | [Azure Digital Twins](../digital-twins/index.yml) |
| Microsoft.KeyVault | [Key Vault](../key-vault/index.yml) |
| Microsoft.Kusto | [Azure 데이터 탐색기](../data-explorer/index.yml) |
| Microsoft.LabServices | [Azure Lab Services](../lab-services/index.yml) |
| Microsoft.LocationBasedServices | [Azure Maps](../azure-maps/index.yml) |
| Microsoft.LocationServices | core |
| Microsoft.LogAnalytics | [Azure Monitor](../azure-monitor/index.yml) |
| Microsoft.Logic | [Logic Apps](../logic-apps/index.yml) |
| Microsoft.MachineLearning | [Machine Learning Studio](../machine-learning/studio/index.yml) |
| Microsoft.MachineLearningCompute | [Machine Learning 서비스](../machine-learning/service/index.yml) |
| Microsoft.MachineLearningModelManagement | [Machine Learning 서비스](../machine-learning/service/index.yml) |
| Microsoft.MachineLearningServices | [Machine Learning 서비스](../machine-learning/service/index.yml) |
| Microsoft.ManagedIdentity | [Azure 리소스에 대한 관리 ID](../active-directory/managed-identities-azure-resources/index.yml) |
| Microsoft.ManagedLab | [Azure Lab Services](../lab-services/index.yml) |
| Microsoft.Management | [관리 그룹](/azure/governance/management-groups/) |
| Microsoft.Maps | [Azure Maps](../azure-maps/index.yml) |
| Microsoft.Marketplace | core |
| Microsoft.MarketplaceApps | core |
| Microsoft.MarketplaceOrdering | core |
| Microsoft.Media | [Media Services](../media-services/index.yml) |
| Microsoft.Migrate | [Azure Migrate](../migrate/migrate-overview.md) |
| Microsoft.MixedReality | [Azure Spatial Anchors](/azure/spatial-anchors/) |
| Microsoft.NetApp | [Azure NetApp Files](../azure-netapp-files/index.yml) |
| Microsoft.Network | [Virtual Network](../virtual-network/index.yml)<br />[Load Balancer](../load-balancer/index.yml)<br />[Application Gateway](../application-gateway/index.yml)<br />[Azure DNS](../dns/index.yml)<br />[ExpressRoute](../expressroute/index.yml)<br />[VPN Gateway](../vpn-gateway/index.yml)<br />[Traffic Manager](../traffic-manager/index.yml)<br />[Network Watcher](../network-watcher/index.yml)<br />[Azure Firewall](../firewall/index.yml)<br />[Azure Front Door Service](../frontdoor/index.yml)<br />[Azure 방호](/azure/bastion/) |
| Microsoft.NotificationHubs | [Notification Hubs](../notification-hubs/index.yml) |
| Microsoft.OffAzure | [Azure Migrate](../migrate/migrate-overview.md) |
| Microsoft.OperationalInsights | [Azure Monitor](../azure-monitor/index.yml) |
| Microsoft.OperationsManagement | [Azure Monitor](../azure-monitor/index.yml) |
| Microsoft.PolicyInsights | [Azure Policy](../governance/policy/index.yml) |
| Microsoft.Portal | [Azure Portal](/azure/azure-portal/) |
| Microsoft.PowerBI | [Power BI](/power-bi/power-bi-overview) |
| Microsoft.PowerBIDedicated | [Power BI Embedded](/azure/power-bi-embedded/) |
| Microsoft.RecoveryServices | [Site Recovery](../site-recovery/index.yml) |
| Microsoft.Relay | [Azure Relay](../service-bus-relay/relay-what-is-it.md) |
| Microsoft.ResourceGraph | [Azure 리소스 그래프](/azure/governance/resource-graph/) |
| Microsoft.ResourceHealth | core |
| Microsoft.Resources | [Azure 리소스 관리자](index.yml) |
| Microsoft.SaaS | core |
| Microsoft.Scheduler | [Scheduler](/azure/scheduler/) |
| Microsoft.Search | [Azure Search](../search/index.yml) |
| Microsoft.Security | [Security Center](../security-center/index.yml) |
| Microsoft.ServiceBus | [Service Bus](/azure/service-bus/) |
| Microsoft.ServiceFabric | [Service Fabric](../service-fabric/index.yml) |
| Microsoft.ServiceFabricMesh | [Service Fabric Mesh](../service-fabric-mesh/index.yml) |
| Microsoft.SignalRService | [Azure SignalR Service](../azure-signalr/index.yml) |
| Microsoft.SiteRecovery | [Site Recovery](../site-recovery/index.yml) |
| Microsoft.Solutions | [Azure Managed Applications](../managed-applications/index.yml) |
| Microsoft.Sql | [Azure SQL Database](../sql-database/index.yml) |
| Microsoft.SqlVirtualMachine | [Azure Virtual Machines의 SQL Server](../virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md) |
| Microsoft.Storage | [저장소](../storage/index.yml) |
| Microsoft.StorageSync | [저장소](../storage/index.yml) |
| Microsoft.StorSimple | [StorSimple](/azure/storsimple/) |
| Microsoft.StreamAnalytics | [Stream Analytics](../stream-analytics/index.yml) |
| Microsoft.Subscription | core |
| microsoft.support | core |
| Microsoft.TimeSeriesInsights | [Time Series Insights](../time-series-insights/index.yml) |
| microsoft.visualstudio | [Azure DevOps](/azure/devops/?view=azure-devops) |
| Microsoft.VMwareCloudSimple | [CloudSimple의 Azure VMware 솔루션](/azure/vmware-cloudsimple/) |
| Microsoft.Web | [App Service](../app-service/index.yml)<br />[함수](../azure-functions/index.yml) |
| Microsoft.WindowsDefenderATP | [Windows Defender Advanced Threat Protection](/windows/security/threat-protection/windows-defender-atp/windows-defender-advanced-threat-protection) |
| Microsoft.WindowsIoT | [Windows 10 IoT Core Services](https://docs.microsoft.com/windows-hardware/manufacture/iot/iotcoreservicesoverview) |
| Microsoft.WorkloadMonitor | [Azure Monitor](../azure-monitor/index.yml) |

## <a name="next-steps"></a>다음 단계

리소스 공급자에 대 한 자세한 내용은 [Azure 리소스 공급자 및 형식](resource-manager-supported-services.md) 을 참조 하세요.
