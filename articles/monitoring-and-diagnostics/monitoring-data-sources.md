---
title: "Figyelési adatokat az Azure-ból |} Microsoft Docs"
description: "Ismerje meg az összes figyelési adatforrások elérhető Azure ma."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/27/2017
ms.author: johnkem
ms.openlocfilehash: c7075c2e1a2500eca1d0aa9b3a797e8a0e903ede
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/11/2017
---
# <a name="consume-monitoring-data-from-azure"></a>Figyelési adatokat az Azure-ból

A Azure platformon azt is összegyűjtésével figyelési adatokat az Azure-megfigyelővel egyetlen helyen a következő feldolgozási sorban, de gyakorlatilag megerősíti, hogy jelenleg nem minden figyelési adatok még érhető el, hogy a folyamat. Ez a cikk azt azokat a különböző módszereket programokon keresztül elérhető megfigyelési adatok Azure-szolgáltatásokhoz.

## <a name="options-for-data-consumption"></a>Beállítások a adatok felhasználásához

| Adattípus | Kategória | Támogatott szolgáltatások | Hozzáférési módok |
| --- | --- | --- | --- |
| Az Azure figyelő platform szintű metrikák | Mérőszámok | [Itt listája](monitoring-supported-metrics.md) | <ul><li>**REST API:** [Azure figyelő metrika API](https://docs.microsoft.com/rest/api/monitor/metrics)</li><li>**Tárolási blob vagy event hub:** [diagnosztikai beállítások](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings)</li></ul> |
| Számítási vendég operációs rendszer metrikák (például) a Teljesítményfigyelő számlálók) | Mérőszámok | [Windows](../virtual-machines-dotnet-diagnostics.md) és a Linux virtuális gépek (v2) [a felhőalapú szolgáltatások](../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md), [Fabric szolgáltatás](../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md) | <ul><li>**A table Storage vagy a blob:** [Windows vagy Linux rendszerű Azure diagnostics](../cloud-services/cloud-services-dotnet-diagnostics-storage.md)</li><li>**Az Event hubs:** [Windows Azure diagnosztikai](../event-hubs/event-hubs-streaming-azure-diags-data.md)</li></ul> |
| Egyéni vagy alkalmazás metrikák | Mérőszámok | Az Application insights szolgáltatással tagolva bármely alkalmazás | <ul><li>**REST API:** [az Application Insights REST API-n](https://dev.applicationinsights.io/reference)</li></ul> |
| Storage mérőszámainak | Mérőszámok | Azure Storage | <ul><li>**Tárolási tábla:** [tárolási analitika](https://docs.microsoft.com/rest/api/storageservices/storage-analytics)</li></ul> |
| Számlázási adatokat | Mérőszámok | Az összes Azure-szolgáltatások | <ul><li>**REST API:** [az Azure erőforrás-használat és RateCard API-k](../billing/billing-usage-rate-card-overview.md)</li></ul> |
| Tevékenységnapló | Események | Az összes Azure-szolgáltatások | <ul><li>**REST API:** [Azure figyelő események API](https://docs.microsoft.com/rest/api/monitor/events)</li><li>**Tárolási blob vagy event hub:** [napló profil](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile)</li></ul> |
| A figyelő az Azure diagnosztikai naplók | Események | [Itt listája](monitoring-diagnostic-logs-schema.md) | <ul><li>**Tárolási blob vagy event hub:** [diagnosztikai beállítások](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings)</li></ul> |
| Számítási vendég operációs rendszer naplók (pl. Az IIS, az ETW, rendszerbejegyzések) | Események | [Windows](../virtual-machines-dotnet-diagnostics.md) és a Linux virtuális gépek (v2) [a felhőalapú szolgáltatások](../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md), [Fabric szolgáltatás](../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md) | <ul><li>**A table Storage vagy a blob:** [Windows vagy Linux rendszerű Azure diagnostics](../cloud-services/cloud-services-dotnet-diagnostics-storage.md)</li><li>**Az Event hubs:** [Windows Azure diagnosztikai](../event-hubs/event-hubs-streaming-azure-diags-data.md)</li></ul> |
| App Service naplóit | Események | App Services | <ul><li>**Fájl, táblázat vagy a blob storage:** [Web app diagnosztika](../app-service/web-sites-enable-diagnostic-log.md)</li></ul> |
| A tárolási naplófájljai | Események | Azure Storage | <ul><li>**Tárolási tábla:** [tárolási analitika](https://docs.microsoft.com/rest/api/storageservices/storage-analytics)</li></ul> |
| A Security Center riasztásait | Események | Azure Security Center | <ul><li>**REST API:** [biztonsági riasztások](https://msdn.microsoft.com/library/mt704050.aspx)</li></ul> |
| Active Directory-jelentéskészítés | Események | Azure Active Directory | <ul><li>**REST API:** [az Azure Active Directory graph API](../active-directory/active-directory-reporting-api-getting-started.md)</li></ul> |
| A Security Center erőforrás állapota | status | [Minden támogatott erőforrások](https://msdn.microsoft.com/library/mt704041.aspx#Anchor_1) | <ul><li>**REST API:** [biztonsági állapotok](https://msdn.microsoft.com/library/mt704041.aspx)</li></ul> |
| Resource Health | status | Támogatott szolgáltatások | <ul><li>**REST API:** [REST API erőforrás állapota](https://azure.microsoft.com/blog/reduce-troubleshooting-time-with-azure-resource-health/)</li></ul> |
| Az Azure metrika riasztások | Értesítések | [Itt listája](monitoring-supported-metrics.md) | <ul><li>**Webhook:** [Azure metrika riasztások](insights-webhooks-alerts.md)</li></ul> |
| Azure tevékenységnapló figyelő riasztások | Értesítések | Az összes Azure-szolgáltatások | <ul><li>**Webhook:** Azure tevékenységnapló riasztások</li></ul> |
| Automatikus skálázás értesítések | Értesítések | [Itt listája](monitoring-overview-autoscale.md#supported-services-for-autoscale) | <ul><li>**Webhook:** [automatikus skálázás értesítési webhook hasznos séma](insights-autoscale-to-webhook-email.md#autoscale-notification-webhook-payload-schema)</li></ul> |
| Naplófájl-keresési lekérdezés OMS-riasztások | Értesítések | OMS szolgáltatáshoz | <ul><li>**Webhook:** [Naplóelemzési riasztások](../log-analytics/log-analytics-alerts-actions.md#webhook-actions)</li></ul> |
| Application Insights-metrika riasztások | Értesítések | Application Insights | <ul><li>**Webhook:** [Application Insights-riasztások](../application-insights/app-insights-alerts.md)</li></ul> |
| Application Insights webes tesztjeinek használatát | Értesítések | Application Insights | <ul><li>**Webhook:** [Application Insights-riasztások](../application-insights/app-insights-alerts.md)</li></ul> |

## <a name="next-steps"></a>Következő lépések

- További információ [Azure figyelő metrikák](monitoring-overview-metrics.md)
- További információ [az Azure tevékenységnapló](monitoring-overview-activity-logs.md)
- További információ [Azure diagnosztikai naplók](monitoring-overview-of-diagnostic-logs.md)
