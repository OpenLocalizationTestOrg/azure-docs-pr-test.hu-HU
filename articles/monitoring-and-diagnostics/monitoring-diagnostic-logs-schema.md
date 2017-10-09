---
title: "aaaAzure diagnosztikai naplók támogatott szolgáltatások és -sémákat |} Microsoft Docs"
description: "Ismerje meg hello támogatott szolgáltatások és az esemény séma Azure diagnosztikai naplókat."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: fe8887df-b0e6-46f8-b2c0-11994d28e44f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: johnkem
ms.openlocfilehash: a3cbf5267e1bd0dc257f4fb4f42c323644046a6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="supported-services-schemas-and-categories-for-azure-diagnostic-logs"></a>Támogatott szolgáltatások, sémákkal és az Azure diagnosztikai naplók kategóriák

[Azure-erőforrás diagnosztikai naplók](monitoring-overview-of-diagnostic-logs.md) naplók az Azure-erőforrások erőforrás hello műveletet leíró által kibocsátott. Ezek a naplók erőforrástípus adott. Ez a cikk azt szerkezeti hello beállítása események minden egyes szolgáltatás által kibocsátott támogatott szolgáltatások és az esemény séma. Ez a cikk is elérhető kategóriák erőforrás típusonkénti teljes listáját.

## <a name="supported-services-and-schemas-for-resource-diagnostic-logs"></a>Támogatott szolgáltatások és -sémákat erőforrás diagnosztikai naplók
az erőforrás diagnosztikai naplók hello séma hello erőforrás és a naplófájlok kategória függ.   

| Szolgáltatás | Séma & Docs |
| --- | --- |
| API Management | A séma nem érhető el. |
| Application Gateway átjárók |[Az Alkalmazásátjáró diagnosztikai naplózás](../application-gateway/application-gateway-diagnostics.md) |
| Azure Automation |[Az Azure Automation szolgáltatáshoz](../automation/automation-manage-send-joblogs-log-analytics.md) |
| Azure Batch |[Azure Batch diagnosztikai naplózás](../batch/batch-diagnostics.md) |
| Ügyfél Insights | A séma nem érhető el. |
| Content Delivery Network | A séma nem érhető el. |
| CosmosDB | A séma nem érhető el. |
| Data Lake Analytics |[Az Azure Data Lake Analytics diagnosztikai naplóinak elérése](../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
| Data Lake Store |[Diagnosztikai naplók az Azure Data Lake Store elérése](../data-lake-store/data-lake-store-diagnostic-logs.md) |
| Event Hubs |[Az Azure Event Hubs diagnosztikai naplók](../event-hubs/event-hubs-diagnostic-logs.md) |
| Key Vault |[Az Azure Key Vault naplózása](../key-vault/key-vault-logging.md) |
| Load Balancer |[Naplóelemzés az Azure Load Balancerhez](../load-balancer/load-balancer-monitor-log.md) |
| Logic Apps |[Logic Apps B2B egyéni követési séma](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md) |
| Network Security Groups (Hálózati biztonsági csoportok) |[Naplóelemzés hálózati biztonsági csoportokhoz](../virtual-network/virtual-network-nsg-manage-log.md) |
| Recovery Services | A séma nem érhető el.|
| Keresés |[Engedélyezése és a keresési forgalom Analytics használata](../search/search-traffic-analytics.md) |
| Kiszolgálófelügyelet | A séma nem érhető el. |
| Service Bus |[Az Azure Service Bus diagnosztikai naplók](../service-bus-messaging/service-bus-diagnostic-logs.md) |
| Stream Analytics |[Diagnosztikai naplók feladat](../stream-analytics/stream-analytics-job-diagnostic-logs.md) |

## <a name="supported-log-categories-per-resource-type"></a>Támogatott erőforrástípus napló kategóriát
|Erőforrás típusa|Kategória|Kategória megjelenített neve|
|---|---|---|
|Microsoft.ApiManagement/service|GatewayLogs|Naplók kapcsolódó tooApiManagement átjáró|
|Microsoft.Automation/automationAccounts|JobLogs|Feladatnaplóit|
|Microsoft.Automation/automationAccounts|JobStreams|Feladat adatfolyamok|
|Microsoft.Automation/automationAccounts|DscNodeStatus|A DSC-csomópont állapota|
|Microsoft.Batch/batchAccounts|ServiceLog|Service naplóit|
|Microsoft.Cdn/profiles/endpoints|CoreAnalytics|Lekérdezi a hello metrikák hello végpont, például a sávszélesség, a kimenő forgalom, a stb.|
|Microsoft.CustomerInsights/hubs|AuditEvents|AuditEvents|
|Microsoft.DataLakeAnalytics/accounts|Naplózás|Naplók|
|Microsoft.DataLakeAnalytics/accounts|Kérelmek|Naplók kérése|
|Microsoft.DataLakeStore/accounts|Naplózás|Naplók|
|Microsoft.DataLakeStore/accounts|Kérelmek|Naplók kérése|
|Microsoft.DocumentDB/databaseAccounts|DataPlaneRequests|DataPlaneRequests|
|Microsoft.EventHub/namespaces|ArchiveLogs|Archív naplók|
|Microsoft.EventHub/namespaces|OperationalLogs|Műveleti naplókat|
|Microsoft.EventHub/namespaces|AutoScaleLogs|Automatikus méretezési naplók|
|Microsoft.KeyVault/vaults|AuditEvent|Naplók|
|Microsoft.Logic/workflows|WorkflowRuntime|A munkafolyamat futásidejű diagnosztikai eseményei|
|Microsoft.Logic/integrationAccounts|IntegrationAccountTrackingEvents|Integráció fiók nyomon követheti az eseményeket|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|Hálózati biztonsági csoport eseménye|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|Hálózati biztonsági csoport Szabályszámlálója|
|Microsoft.Network/loadBalancers|LoadBalancerAlertEvent|Terheléselosztó értesítési eseményei|
|Microsoft.Network/loadBalancers|LoadBalancerProbeHealthStatus|Terheléselosztói mintavétel állapot betöltése|
|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|Átjáró hozzáférési napló|
|Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|Alkalmazásnapló átjáró teljesítménye|
|Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|Átjáró tűzfal alkalmazásnaplóban|
|Microsoft.RecoveryServices/Vaults|AzureBackupReport|Jelentési adatok Azure biztonsági mentés|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryJobs|Az Azure Site Recovery-feladatok|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryEvents|Az Azure Site Recovery-események|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicatedItems|Azure Site Recovery replikált elemek|
|Microsoft.Search/searchServices|OperationLogs|A műveletnaplók|
|Microsoft.ServiceBus/namespaces|OperationalLogs|Műveleti naplókat|
|Microsoft.StreamAnalytics/streamingjobs|Végrehajtás|Végrehajtás|
|Microsoft.StreamAnalytics/streamingjobs|Szerzői|Szerzői|

## <a name="next-steps"></a>Következő lépések

* [További információ a diagnosztikai naplók](monitoring-overview-of-diagnostic-logs.md)
* [Adatfolyam-erőforrás diagnosztikai naplók túl**Event Hubs**](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [Hello Azure figyelő REST API használatával erőforrás diagnosztikai beállításainak módosítása](https://msdn.microsoft.com/library/azure/dn931931.aspx)
* [A Naplóelemzési az Azure storage naplóinak elemzése](../log-analytics/log-analytics-azure-storage.md)
