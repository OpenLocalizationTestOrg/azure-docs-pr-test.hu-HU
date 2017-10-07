---
title: "API-kezelés az Azure-figyelő aaaMonitor |} Microsoft Docs"
description: "Ismerje meg, hogyan toomonitor Azure API Management service, Azure-figyelővel."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 2fa193cd-ea71-4b33-a5ca-1f55e5351e23
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 5012d8ed57ea4f94ea6bc1b7c4e1102516ec4414
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-api-management-with-azure-monitor"></a>Figyelő API-kezelés az Azure-figyelő
Az Azure figyelő az Azure-szolgáltatások, amely az összes Azure-erőforrások figyelése egyetlen helyről biztosít. Azure megfigyelővel ábrázolhatja, lekérdezése, továbbítani, archivált, és műveletek hello metrikák és az Azure erőforrások, például az API Management származó naplók. 

a következő videó bemutatja hogyan hello toomonitor figyelővel Azure API Management. Azure-figyelővel kapcsolatos további információkért lásd: [Ismerkedés az Azure-figyelő]. 


> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Monitor-API-Management-with-Azure-Monitor/player]
>
>
 
## <a name="metrics"></a>Mérőszámok
Az API Management jelenleg öt metrikák bocsát ki, és a jövőbeli hello további tooadd tervezzük. A metrikák kibocsátott percenként, felkínálva a közel valós idejű információkat hello állapot és az API-kat állapotát. Az alábbiakban látható hello metrikák összefoglalása:
* Átjáró kérelmek teljes száma: hello API-lekérdezések száma hello időszakban. 
* Átjáró sikeres kérelmek: API-kérelmek fogadott, többek között a 304, 307 és annak minden kisebb, mint (például 200) 301 sikeres HTTP válaszkódot hello száma. 
* Nem sikerült az átjáró kérelmek: hello API kérelmek száma, beleértve a 400-as és annak minden nagyobb, mint 500 hibás HTTP válaszkódot kapott.
* Jogosulatlan átjáró kérelmek:, beleértve a 401-es, a 403-as és a 429 HTTP válaszkódot kapott API-kérelmek hello száma. 
* Más átjáró kérelmek: API-kérelmek fogadott, amelyek nem tartoznak a megelőző kategóriák (például 418) hello tooany HTTP válaszkódot hello száma.

Az API Management szolgáltatásban metrikák vagy hozzáférési metrikák összes Azure-erőforrások Azure figyelőben érheti el. az API Management szolgáltatásban tooview metrikák:
1. Nyissa meg hello Azure-portálon.
2. Nyissa meg a tooyour API-kezelés szolgáltatás.
3. Kattintson a **metrikák**.

![Metrikák panel][metrics-blade]

További információ toouse metrika, lásd: [áttekintése a metrikák].

## <a name="activity-logs"></a>Tevékenységnaplók
Tevékenységi naplóit hello műveletek az API Management-szolgáltatások a végrehajtott betekintést nyújtanak. Azt korábban hívták "naplófájlok" vagy "működési logs". Tevékenység-naplók segítségével meghatározhatja hello "mi, ki, és mikor" az összes írni az API Management szolgáltatásokban végzett műveleteket (PUT, POST, Törlés). 

> [!NOTE]
> Tevékenység naplói nem tartalmazzák (GET) olvasási műveletek vagy végrehajtott műveletek hello Publisher klasszikus portál vagy az eredeti felügyeleti API-k használatával hello.

Tevékenység naplók elérhetők az API Management szolgáltatásban, és elérni az összes Azure-erőforrások Azure figyelőben naplókat. tooview tevékenység naplózza az API Management szolgáltatásban:
1. Nyissa meg hello Azure-portálon.
2. Nyissa meg a tooyour API-kezelés szolgáltatás.
3. Kattintson a **tevékenységnapló**.

![Tevékenység naplók panel][activity-logs-blade]

További információ toouse metrika, lásd: [tevékenységi naplóit – áttekintés].

## <a name="alerts"></a>Riasztások
Konfigurálhat tooreceive metrikák és tevékenység naplók alapján. Az Azure a figyelő lehetővé teszi egy riasztási toodo hello követően amikor elindítja a tooconfigure:

* E-mail értesítés küldése
* A webhook hívása
* Egy Azure logikai alkalmazás meghívása

A riasztási szabályok konfigurálhatja az API Management szolgáltatásban, vagy az Azure-figyelő. tooconfigure az API Management őket: 
1. Nyissa meg hello Azure-portálon.
2. Nyissa meg a tooyour API-kezelés szolgáltatás.
3. Kattintson a **riasztási szabályok**.

![A riasztási szabályok panel][alert-rules-blade]

Riasztások használatával kapcsolatos további információkért lásd: [áttekintése a riasztások].

## <a name="diagnostic-logs"></a>Diagnosztikai naplók
Diagnosztikai naplók gazdag információkkal kapcsolatos műveletek és a naplózás és a hibaelhárítási célból fontos hibák. Diagnosztikai naplók eltérnek a tevékenységi naplóit. Tevékenység naplók az Azure-erőforrások a végrehajtott műveletek hello betekintést. Diagnosztikai naplók Észreveheti az olyan műveletek, hogy az erőforrás végre magát.

Az API Management jelenleg biztosít diagnosztika mindegyik bejegyzés rendelkezik a következő struktúra hello igénylése kapcsolatos egyéni API naplókat (óránkénti kötegelni):

```
{
    "Tenant": "",
      "DeploymentName": "",
      "time": "",
      "resourceId": "",
      "category": "GatewayLogs",
      "operationName": "Microsoft.ApiManagement/GatewayLogs",
      "durationMs": ,
      "Level": ,
      "properties": "{
          "ApiId": "",
          "OperationId": "",
          "ProductId": "",
          "SubscriptionId": "",
          "Method": "",
          "Url": "",
          "RequestSize": ,
          "ServiceTime": "",
          "BackendMethod": "",
          "BackendUrl": "",
          "BackendResponseCode": ,
          "ResponseCode": ,
          "ResponseSize": ,
          "Cache": "",
          "UserId"
      }"
 }
```

Diagnosztikai naplók elérhetők az API Management szolgáltatásban, és hozzáférés az összes Azure-erőforrások Azure figyelőben naplók. tooview diagnosztikai naplók az API Management szolgáltatásban:
1. Nyissa meg hello Azure-portálon.
2. Nyissa meg a tooyour API-kezelés szolgáltatás.
3. Kattintson a **diagnosztikai naplófájl**.

![Diagnosztikai naplók panel][diagnostic-logs-blade]

További információ toouse metrika, lásd: [diagnosztikai naplók áttekintése].

## <a name="next-step"></a>Következő lépés

* [Ismerkedés az Azure-figyelő]
* [áttekintése a metrikák]
* [tevékenységi naplóit – áttekintés]
* [diagnosztikai naplók áttekintése]
* [áttekintése a riasztások]

[Ismerkedés az Azure-figyelő]: ../monitoring-and-diagnostics/monitoring-get-started.md
[áttekintése a metrikák]: ../monitoring-and-diagnostics/monitoring-overview-metrics.md
[tevékenységi naplóit – áttekintés]: ../monitoring-and-diagnostics/monitoring-overview-activity-logs.md
[diagnosztikai naplók áttekintése]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
[áttekintése a riasztások]: ../monitoring-and-diagnostics/insights-alerts-portal.md



[metrics-blade]: ./media/api-management-azure-monitor/api-management-metrics-blade.png
[activity-logs-blade]: ./media/api-management-azure-monitor/api-management-activity-logs-blade.png
[alert-rules-blade]: ./media/api-management-azure-monitor/api-management-alert-rules-blade.png
[diagnostic-logs-blade]: ./media/api-management-azure-monitor/api-management-diagnostic-logs-blade.png
