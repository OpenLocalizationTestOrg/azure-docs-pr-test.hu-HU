---
title: "Műveletnapló esemény séma aaaAzure |} Microsoft Docs"
description: "Hello esemény séma hello tevékenységnapló ki adatok ismertetése"
author: johnkemnetz
manager: robb
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: johnkem
ms.openlocfilehash: dfece949a20a4d9b4e8a4d488c1c34842d87d586
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-activity-log-event-schema"></a>Azure tevékenységnapló esemény séma
Hello **Azure tevékenységnapló** , amely bármely történt az Azure-előfizetés szintű események betekintést biztosít a naplót. Ez a cikk ismerteti az adatok kategóriánként hello esemény séma.

## <a name="administrative"></a>Felügyeleti
Ez a kategória tartalmazza az összes hello rekordot létrehozása, update, delete és művelet műveleteket Resource Manageren keresztül. Példát mutatunk be ebbe a kategóriába események típusai hello "virtuális gép létrehozása" és "hálózati biztonsági csoport törlése" minden felhasználó által végrehajtott műveletet, vagy az alkalmazás erőforrás-kezelővel egy adott erőforrástípus művelet van modellezve. Ha hello művelet típusa beírni, a művelet, vagy törlése hello rekordokat hello kezdő és a sikeres vagy sikertelen a művelet rögzíti hello felügyeleti kategória. hello felügyeleti kategória is bármely módosítások toorole-alapú hozzáférés-vezérlés egy előfizetésben.

### <a name="sample-event"></a>Mintaesemény
```json
{
  "authorization": {
    "action": "microsoft.support/supporttickets/write",
    "role": "Subscription Admin",
    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841"
  },
  "caller": "admin@contoso.com",
  "channels": "Operation",
  "claims": {
    "aud": "https://management.core.windows.net/",
    "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
    "iat": "1421876371",
    "nbf": "1421876371",
    "exp": "1421880271",
    "ver": "1.0",
    "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
    "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
    "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
    "puid": "20030000801A118C",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
    "name": "John Smith",
    "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
    "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
    "appidacr": "2",
    "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
    "http://schemas.microsoft.com/claims/authnclassreference": "1"
  },
  "correlationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
  "description": "",
  "eventDataId": "44ade6b4-3813-45e6-ae27-7420a95fa2f8",
  "eventName": {
    "value": "EndRequest",
    "localizedValue": "End request"
  },
  "httpRequest": {
    "clientRequestId": "27003b25-91d3-418f-8eb1-29e537dcb249",
    "clientIpAddress": "192.168.35.115",
    "method": "PUT"
  },
  "id": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841/events/44ade6b4-3813-45e6-ae27-7420a95fa2f8/ticks/635574752669792776",
  "level": "Informational",
  "resourceGroupName": "MSSupportGroup",
  "resourceProviderName": {
    "value": "microsoft.support",
    "localizedValue": "microsoft.support"
  },
  "resourceUri": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
  "operationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
  "operationName": {
    "value": "microsoft.support/supporttickets/write",
    "localizedValue": "microsoft.support/supporttickets/write"
  },
  "properties": {
    "statusCode": "Created"
  },
  "status": {
    "value": "Succeeded",
    "localizedValue": "Succeeded"
  },
  "subStatus": {
    "value": "Created",
    "localizedValue": "Created (HTTP Status Code: 201)"
  },
  "eventTimestamp": "2015-01-21T22:14:26.9792776Z",
  "submissionTimestamp": "2015-01-21T22:14:39.9936304Z",
  "subscriptionId": "s1"
}
```

### <a name="property-descriptions"></a>Tulajdonság leírása
| Elem neve | Leírás |
| --- | --- |
| Engedélyezési |A BLOB RBAC tulajdonságok hello esemény. Hello "action", "szerepkör" és "hatókör" Tulajdonságok általában tartalmazza. |
| hívó |Hello művelet, jogcím vagy SPN jogcímet rendelkezésre állása alapján elvégzett hello felhasználó e-mail címe. |
| csatornák |Hello a következő értékek egyikét: "Rendszergazda", "Művelet" |
| Jogcímek |hello JWT jogkivonat Active Directory tooauthenticate hello felhasználó vagy alkalmazás tooperform használja ezt a műveletet az erőforrás-kezelőben. |
| correlationId |Általában egy GUID Azonosítót a hello karakterlánc-formátum. Az eseményeket, amelyek megosztása a correlationId tartozik toohello uber műveletet. |
| leírás |Egy esemény statikus szöveges leírása. |
| eventDataId |Az esemény egyedi azonosítója. |
| httpRequest |A BLOB leíró hello Http-kérelem. Általában tartalmazza a hello "clientRequestId", "clientIpAddress" és "method" (HTTP-metódus. For example, amelyre az). |
| szint |Hello esemény szintje. Hello a következő értékek egyikét: "Kritikus", "Error", "Figyelmeztetés", "Tájékoztató" és "Részletes" |
| erőforráscsoport-név |Hello hello erőforráscsoport nevét érintett erőforrás. |
| resourceProviderName |Hello hello erőforrás-szolgáltató neve érintett erőforrás |
| resourceId |Erőforrás-azonosítója, hello érintett erőforrás. |
| OperationID azonosítójú |GUID-azonosítónak megfelelő tooa egyetlen műveletben hello események között meg van osztva. |
| operationName |Hello művelet neve. |
| properties |Állítsa be a `<Key, Value>` hello esemény részleteinek hello leíró párok (Ez azt jelenti, hogy szótárában). |
| status |Hello művelet hello állapotát leíró karakterlánc. Néhány gyakori értékek a következők: folyamatban van, sikeres, sikertelen, aktív, a lépések megoldva. |
| a részállapot |Általában hello REST-hívást megfelelő hello HTTP-állapotkód, de más a részállapot, például a gyakori értékek leíró karakterláncok is használható: OK (HTTP-állapotkód: 200), létrehozott (HTTP-állapotkód: 201-es), elfogadott (HTTP-állapotkód: 202), nincs tartalom (HTTP Állapotkód: 204), hibás kérelem (HTTP-állapotkód: 400), nem található (HTTP-állapotkód: 404), ütközés (HTTP-állapotkód: 409), belső kiszolgálóhiba (HTTP-állapotkód: 500), a Service nem érhető el (HTTP-állapotkód: 503-as), átjáró időtúllépése (HTTP-állapotkód: 504). |
| eventTimestamp |Hello esemény lett létrehozva, amikor hello Azure szolgáltatás feldolgozása hello idejét jelző időbélyegző megfelelő hello esemény kérelmet. |
| submissionTimestamp |Hello esemény váltak elérhetővé lekérdezése idejét jelző időbélyegző. |
| subscriptionId |Az Azure előfizetés-azonosító. |

## <a name="service-health"></a>Szolgáltatás állapota
Ez a kategória történt az Azure-szolgáltatás állapotának események hello rekord tartalmazza. Ebbe a kategóriába tartozó mutatunk be hello eseménytípusra példa: "USA keleti régiója az SQL Azure tapasztal állásidő." Szolgáltatás állapotával kapcsolatos események térjen öt fajta: beavatkozás szükséges, támogatott helyreállítási, incidens, karbantartási, adatokat vagy biztonsági, és csak jelenik meg, ha egy erőforrást, amely akkor negatív hatással lehet hello esemény hello előfizetésben.

### <a name="sample-event"></a>Mintaesemény
```json
{
  "channels": "Admin",
  "correlationId": "c550176b-8f52-4380-bdc5-36c1b59d3a44",
  "description": "Active: Network Infrastructure - UK South",
  "eventDataId": "c5bc4514-6642-2be3-453e-c6a67841b073",
  "eventName": {
      "value": null
  },
  "category": {
      "value": "ServiceHealth",
      "localizedValue": "Service Health"
  },
  "eventTimestamp": "2017-07-20T23:30:14.8022297Z",
  "id": "/subscriptions/mySubscriptionID/events/c5bc4514-6642-2be3-453e-c6a67841b073/ticks/636361902148022297",
  "level": "Warning",
  "operationName": {
      "value": "Microsoft.ServiceHealth/incident/action",
      "localizedValue": "Microsoft.ServiceHealth/incident/action"
  },
  "resourceProviderName": {
      "value": null
  },
  "resourceType": {
      "value": null,
      "localizedValue": ""
  },
  "resourceId": "/subscriptions/mySubscriptionID",
  "status": {
      "value": "Active",
      "localizedValue": "Active"
  },
  "subStatus": {
      "value": null
  },
  "submissionTimestamp": "2017-07-20T23:30:34.7431946Z",
  "subscriptionId": "mySubscriptionID",
  "properties": {
    "title": "Network Infrastructure - UK South",
    "service": "Service Fabric",
    "region": "UK South",
    "communication": "Starting at approximately 21:41 UTC on 20 Jul 2017, a subset of customers in UK South may experience degraded performance, connectivity drops or timeouts when accessing their Azure resources hosted in this region. Engineers are investigating underlying Network Infrastructure issues in this region. Impacted services may include, but are not limited tooApp Services, Automation, Service Bus, Log Analytics, Key Vault, SQL Database, Service Fabric, Event Hubs, Stream Analytics, Azure Data Movement, API Management, and Azure Search. Multiple engineering teams are engaged in multiple workflows toomitigate hello impact. hello next update will be provided in 60 minutes, or as events warrant.",
    "incidentType": "Incident",
    "trackingId": "NA0F-BJG",
    "impactStartTime": "2017-07-20T21:41:00.0000000Z",
    "impactedServices": "[{\"ImpactedRegions\":[{\"RegionName\":\"UK South\"}],\"ServiceName\":\"Service Fabric\"}]",
    "defaultLanguageTitle": "Network Infrastructure - UK South",
    "defaultLanguageContent": "Starting at approximately 21:41 UTC on 20 Jul 2017, a subset of customers in UK South may experience degraded performance, connectivity drops or timeouts when accessing their Azure resources hosted in this region. Engineers are investigating underlying Network Infrastructure issues in this region. Impacted services may include, but are not limited tooApp Services, Automation, Service Bus, Log Analytics, Key Vault, SQL Database, Service Fabric, Event Hubs, Stream Analytics, Azure Data Movement, API Management, and Azure Search. Multiple engineering teams are engaged in multiple workflows toomitigate hello impact. hello next update will be provided in 60 minutes, or as events warrant.",
    "stage": "Active",
    "communicationId": "636361902146035247",
    "version": "0.1.1"
  }
}
```

### <a name="property-descriptions"></a>Tulajdonság leírása
Elem neve | Leírás
-------- | -----------
csatornák | Hello a következő értékek egyike: "Rendszergazda", "Művelet"
correlationId | Az általában egy GUID Azonosítót a hello karakterlánc-formátum. Események, amelyek azonos uber művelet általában megosztása toohello tartozik azonos correlationId hello.
leírás | Hello esemény leírása.
eventDataId | hello egyedi azonosítója egy eseményt.
EventName | hello cím hello esemény.
szint | Hello esemény szintje. Hello a következő értékek egyikét: "Kritikus", "Error", "Figyelmeztetés", "Tájékoztató" és "Részletes"
resourceProviderName | Hello hello erőforrás-szolgáltató neve érintett erőforrás. Ha nem ismeri, ez lehet null.
a resourceType| hello típusú erőforrás hello az érintett erőforrás. Ha nem ismeri, ez lehet null.
a részállapot | A szolgáltatás állapotával kapcsolatos események általában null értékű.
eventTimestamp | Hello naplóesemény jött létre, és toohello tevékenység naplója beküldött idejét jelző időbélyegző.
submissionTimestamp |   Hello esemény váltak elérhetővé az hello tevékenységnapló idejét jelző időbélyegző.
subscriptionId | Azure-előfizetés, amelyben ez az esemény naplózásának hello.
status | Hello művelet hello állapotát leíró karakterlánc. Néhány gyakori értékek a következők: aktív, a megoldott.
operationName | Hello művelet neve. Általában Microsoft.ServiceHealth/incident/action.
category | "ServiceHealth"
resourceId | Erőforrás-azonosítója, hello érintett erőforrás, ha ismert. Ellenkező esetben van megadva előfizetés-azonosító.
Properties.Title | Ez a kommunikáció hello honosított címét. Angol hello alapértelmezett nyelvét.
Properties.Communication | honosított hello részletei hello kommunikáció a HTML-kódot. Angol hello alapértelmezett beállítás.
Properties.incidentType | A lehetséges értékek: AssistedRecovery, ActionRequired, információk, incidens, karbantartási, biztonsági
Properties.trackingId | Az eseményhez kapcsolódó hello incidens azonosítja. Használja az toocorrelate hello események kapcsolódó tooan esemény.
Properties.impactedServices | Az escape-karakterrel megjelölt JSON-blob hello szolgáltatások és hello incidens által érintett területek leíró. Szolgáltatások listáját, amelyek mindegyike rendelkezik egy szolgáltatásnév és ImpactedRegions, amelyek mindegyikének a RegionName listáját.
Properties.defaultLanguageTitle | hello kommunikációs angol nyelven
Properties.defaultLanguageContent | hello kommunikációs html-kódot vagy egyszerű szöveg angol nyelven
Properties.Stage | AssistedRecovery, a ActionRequired, a információkat, az incidensek, a biztonsági a lehetséges értékek: aktív, amelyek megoldva. A karbantartás vannak: aktív, tervezett, esetbejegyzések, visszavonva, Rescheduled, megoldva, kész
Properties.communicationId | hello kommunikációs ezt az eseményt hozzá rendelve.

## <a name="alert"></a>Riasztás
Ez a kategória Azure riasztások valamennyi aktiváláshoz hello rekord tartalmazza. Ebbe a kategóriába tartozó mutatunk be hello eseménytípusra példa: "CPU % myVM lett több mint 80-as hello elmúlt 5 perc." Az Azure rendszereket rendelkezik egy riasztási koncepció--valamiféle szabály megadása, és értesítést kaphat, ha feltételek egyeznek meg, hogy a szabály. Minden alkalommal, amikor egy támogatott Azure riasztástípus "aktiválja," vagy hello feltételek fennállnak toogenerate értesítést, hello aktiválás rekord is fejlesztőre hello tevékenységnapló toothis kategóriáját.

### <a name="sample-event"></a>Mintaesemény

```json
{
  "caller": "Microsoft.Insights/alertRules",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/alertRules"
  },
  "correlationId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert/incidents/L3N1YnNjcmlwdGlvbnMvZGY2MDJjOWMtN2FhMC00MDdkLWE2ZmItZWIyMGM4YmQxMTkyL3Jlc291cmNlR3JvdXBzL0NzbUV2ZW50RE9HRk9PRC1XZXN0VVMvcHJvdmlkZXJzL21pY3Jvc29mdC5pbnNpZ2h0cy9hbGVydHJ1bGVzL215YWxlcnQwNjM2MzYyMjU4NTM1MjIxOTIw",
  "description": "'Disk read LessThan 100000 ([Count]) in hello last 5 minutes' has been resolved for CloudService: myResourceGroup/Production/Event.BackgroundJobsWorker.razzle (myResourceGroup)",
  "eventDataId": "149d4baf-53dc-4cf4-9e29-17de37405cd9",
  "eventName": {
    "value": "Alert",
    "localizedValue": "Alert"
  },
  "category": {
    "value": "Alert",
    "localizedValue": "Alert"
  },
  "id": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/Event.BackgroundJobsWorker.razzle/events/149d4baf-53dc-4cf4-9e29-17de37405cd9/ticks/636362258535221920",
  "level": "Informational",
  "resourceGroupName": "myResourceGroup",
  "resourceProviderName": {
    "value": "Microsoft.ClassicCompute",
    "localizedValue": "Microsoft.ClassicCompute"
  },
  "resourceId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/Event.BackgroundJobsWorker.razzle",
  "resourceType": {
    "value": "Microsoft.ClassicCompute/domainNames/slots/roles",
    "localizedValue": "Microsoft.ClassicCompute/domainNames/slots/roles"
  },
  "operationId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert/incidents/L3N1YnNjcmlwdGlvbnMvZGY2MDJjOWMtN2FhMC00MDdkLWE2ZmItZWIyMGM4YmQxMTkyL3Jlc291cmNlR3JvdXBzL0NzbUV2ZW50RE9HRk9PRC1XZXN0VVMvcHJvdmlkZXJzL21pY3Jvc29mdC5pbnNpZ2h0cy9hbGVydHJ1bGVzL215YWxlcnQwNjM2MzYyMjU4NTM1MjIxOTIw",
  "operationName": {
    "value": "Microsoft.Insights/AlertRules/Resolved/Action",
    "localizedValue": "Microsoft.Insights/AlertRules/Resolved/Action"
  },
  "properties": {
    "RuleUri": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/alertrules/myalert",
    "RuleName": "myalert",
    "RuleDescription": "",
    "Threshold": "100000",
    "WindowSizeInMinutes": "5",
    "Aggregation": "Average",
    "Operator": "LessThan",
    "MetricName": "Disk read",
    "MetricUnit": "Count"
  },
  "status": {
    "value": "Resolved",
    "localizedValue": "Resolved"
  },
  "subStatus": {
    "value": null
  },
  "eventTimestamp": "2017-07-21T09:24:13.522192Z",
  "submissionTimestamp": "2017-07-21T09:24:15.6578651Z",
  "subscriptionId": "mySubscriptionID"
}
```

### <a name="property-descriptions"></a>Tulajdonság leírása
| Elem neve | Leírás |
| --- | --- |
| hívó | Mindig Microsoft.Insights/alertRules |
| csatornák | Mindig "rendszergazda, a művelet" |
| Jogcímek | JSON-blob hello SPN (egyszerű szolgáltatásnév), vagy az erőforrás típusát, hello riasztási motor. |
| correlationId | A karakterlánc-formátum hello GUID. |
| leírás |Hello figyelmeztetési esemény statikus szöveges leírása. |
| eventDataId |Hello figyelmeztetési esemény egyedi azonosítója. |
| szint |Hello esemény szintje. Hello a következő értékek egyikét: "Kritikus", "Error", "Figyelmeztetés", "Tájékoztató" és "Részletes" |
| erőforráscsoport-név |Hello hello erőforráscsoport nevét érintett erőforrás, ha egy metrika riasztás. A más típusú Ez az hello figyelmeztetéshez tartalmazó erőforráscsoport hello hello nevét. |
| resourceProviderName |Hello hello erőforrás-szolgáltató neve érintett erőforrás, ha egy metrika riasztás. A más típusú Ez az hello figyelmeztetéshez hello hello erőforrás-szolgáltató nevét. |
| resourceId | Hello hello erőforrás-azonosító nevet érintett erőforrás, ha egy metrika riasztás. A más típusú Ez az erőforrás-azonosító hello hello riasztási erőforrás magát. |
| OperationID azonosítójú |GUID-azonosítónak megfelelő tooa egyetlen műveletben hello események között meg van osztva. |
| operationName |Hello művelet neve. |
| properties |Állítsa be a `<Key, Value>` hello esemény részleteinek hello leíró párok (Ez azt jelenti, hogy szótárában). |
| status |Hello művelet hello állapotát leíró karakterlánc. Néhány gyakori értékek a következők: folyamatban van, sikeres, sikertelen, aktív, a lépések megoldva. |
| a részállapot | A riasztások általában null értékű. |
| eventTimestamp |Hello esemény lett létrehozva, amikor hello Azure szolgáltatás feldolgozása hello idejét jelző időbélyegző megfelelő hello esemény kérelmet. |
| submissionTimestamp |Hello esemény váltak elérhetővé lekérdezése idejét jelző időbélyegző. |
| subscriptionId |Az Azure előfizetés-azonosító. |

### <a name="properties-field-per-alert-type"></a>Tulajdonságok mező szerepeljen riasztás típusa
hello tulajdonságok mező hello figyelmeztetési esemény hello forrása függően különböző értékeket fogja tartalmazni. Két közös riasztási eseménynek szolgáltatók a következők: tevékenységnapló riasztások és metrika riasztásokat.

#### <a name="properties-for-activity-log-alerts"></a>A műveletnapló riasztások tulajdonságai
| Elem neve | Leírás |
| --- | --- |
| properties.subscriptionId | hello előfizetés-azonosító a hello tevékenység napló okozó esemény a tevékenység napló riasztási szabály toobe aktiválva. |
| properties.eventDataId | hello adatok Eseményazonosítójú hello tevékenység napló okozó esemény a tevékenység napló riasztási szabály toobe aktiválva. |
| properties.resourceGroup | hello erőforráscsoport hello tevékenység napló okozó esemény a tevékenység napló riasztási szabály toobe aktiválva. |
| properties.resourceId | hello erőforrás-azonosító a hello tevékenység napló okozó esemény a tevékenység napló riasztási szabály toobe aktiválva. |
| properties.eventTimestamp | hello esemény időbélyegzője hello tevékenység napló okozó esemény a tevékenység napló riasztási szabály toobe aktiválva. |
| properties.operationName | a művelet neve hello hello tevékenység napló okozó esemény a tevékenység napló riasztási szabály toobe aktiválva. |
| Properties.status | hello tevékenység napló okozó esemény a tevékenység napló riasztási szabály toobe aktiválva hello állapotát.|

#### <a name="properties-for-metric-alerts"></a>A metrika riasztások tulajdonságai
| Elem neve | Leírás |
| --- | --- |
| tulajdonságok. RuleUri | Erőforrás-azonosító hello metrika riasztási szabály saját magát. |
| tulajdonságok. Szabálynév | hello hello metrika riasztási szabály neve. |
| tulajdonságok. RuleDescription | hello metrika riasztási szabály (meghatározott hello riasztási szabály) hello leírása. |
| tulajdonságok. Küszöbérték | hello küszöbérték hello metrika riasztási szabály hello kiértékelésekor. |
| tulajdonságok. WindowSizeInMinutes | hello ablakméret hello metrika riasztási szabály hello kiértékelésekor. |
| tulajdonságok. Összesítés | hello összesítéstípusa hello metrika riasztási szabályban definiált. |
| tulajdonságok. Operátor | hello feltételes operátort hello metrika riasztási szabály hello kiértékelésekor. |
| tulajdonságok. MetricName | hello hello kiértékelésekor hello metrika riasztási szabály hello metrika metrika neve. |
| tulajdonságok. MetricUnit | hello metrika egység hello mérőszám hello metrika riasztási szabály hello kiértékelésekor. |

## <a name="autoscale"></a>Automatikus méretezés
Ebbe a kategóriába bármely események kapcsolódó toohello művelet hello automatikus skálázás motor minden automatikus skálázási beállításokat adta meg az előfizetés alapján hello rekord tartalmazza. Ebbe a kategóriába tartozó mutatunk be hello eseménytípusra példa, hogy "Automatikus skálázás felskálázott művelete nem sikerült." Használja az automatikus skálázás, automatikusan horizontális felskálázás vagy méretezni a hello támogatott erőforrástípus található példányok száma alapján idő nap és/vagy terhelés (mérték) adatok az automatikus skálázási beállítás használatával. Hello feltételek teljesülése esetén tooscale felfelé vagy lefelé, kezdő hello és sikeres vagy sikertelen események rögzítése a kategória.

### <a name="sample-event"></a>Mintaesemény
```json
{
  "caller": "Microsoft.Insights/autoscaleSettings",
  "channels": "Admin, Operation",
  "claims": {
    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn": "Microsoft.Insights/autoscaleSettings"
  },
  "correlationId": "fc6a7ff5-ff68-4bb7-81b4-3629212d03d0",
  "description": "hello autoscale engine attempting tooscale resource '/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count too2 instances count.",
  "eventDataId": "a5b92075-1de9-42f1-b52e-6f3e4945a7c7",
  "eventName": {
    "value": "AutoscaleAction",
    "localizedValue": "AutoscaleAction"
  },
  "category": {
    "value": "Autoscale",
    "localizedValue": "Autoscale"
  },
  "id": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/autoscalesettings/myResourceGroup-Production-myResource-myResourceGroup/events/a5b92075-1de9-42f1-b52e-6f3e4945a7c7/ticks/636361956518681572",
  "level": "Informational",
  "resourceGroupName": "myResourceGroup",
  "resourceProviderName": {
    "value": "microsoft.insights",
    "localizedValue": "microsoft.insights"
  },
  "resourceId": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/microsoft.insights/autoscalesettings/myResourceGroup-Production-myResource-myResourceGroup",
  "resourceType": {
    "value": "microsoft.insights/autoscalesettings",
    "localizedValue": "microsoft.insights/autoscalesettings"
  },
  "operationId": "fc6a7ff5-ff68-4bb7-81b4-3629212d03d0",
  "operationName": {
    "value": "Microsoft.Insights/AutoscaleSettings/Scaledown/Action",
    "localizedValue": "Microsoft.Insights/AutoscaleSettings/Scaledown/Action"
  },
  "properties": {
    "Description": "hello autoscale engine attempting tooscale resource '/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource' from 3 instances count too2 instances count.",
    "ResourceName": "/subscriptions/mySubscriptionID/resourceGroups/myResourceGroup/providers/Microsoft.ClassicCompute/domainNames/myResourceGroup/slots/Production/roles/myResource",
    "OldInstancesCount": "3",
    "NewInstancesCount": "2",
    "LastScaleActionTime": "Fri, 21 Jul 2017 01:00:51 GMT"
  },
  "status": {
    "value": "Succeeded",
    "localizedValue": "Succeeded"
  },
  "subStatus": {
    "value": null
  },
  "eventTimestamp": "2017-07-21T01:00:51.8681572Z",
  "submissionTimestamp": "2017-07-21T01:00:52.3008754Z",
  "subscriptionId": "mySubscriptionID"
}

```

### <a name="property-descriptions"></a>Tulajdonság leírása
| Elem neve | Leírás |
| --- | --- |
| hívó | Mindig Microsoft.Insights/autoscaleSettings |
| csatornák | Mindig "rendszergazda, a művelet" |
| Jogcímek | JSON blob hello hello automatikus skálázás motor SPN (egyszerű szolgáltatásnév), vagy az erőforrás típusát. |
| correlationId | A karakterlánc-formátum hello GUID. |
| leírás |Statikus szöveg hello automatikus skálázás esemény leírását. |
| eventDataId |Hello automatikus skálázás esemény egyedi azonosítója. |
| szint |Hello esemény szintje. Hello a következő értékek egyikét: "Kritikus", "Error", "Figyelmeztetés", "Tájékoztató" és "Részletes" |
| erőforráscsoport-név |Hello automatikus skálázási beállítás hello erőforráscsoport nevét. |
| resourceProviderName |Hello automatikus skálázási beállítás hello erőforrás-szolgáltató nevét. |
| resourceId |Erőforrás-azonosító hello automatikus skálázási beállítás. |
| OperationID azonosítójú |GUID-azonosítónak megfelelő tooa egyetlen műveletben hello események között meg van osztva. |
| operationName |Hello művelet neve. |
| properties |Állítsa be a `<Key, Value>` hello esemény részleteinek hello leíró párok (Ez azt jelenti, hogy szótárában). |
| tulajdonságok. Leírás | Milyen hello automatikus skálázás motor műveletet végzett részletes leírása. |
| tulajdonságok. ResourceName | Erőforrás-azonosítója, hello érintett erőforrás (hello erőforrás mely hello a skálázási műveletek végrehajtása) |
| tulajdonságok. OldInstancesCount | hello több példányban hello automatikus skálázási művelet előtt alkalmaztak. |
| tulajdonságok. NewInstancesCount | Miután hello automatikus skálázási művelet végrehajtásának hatás példányok hello száma. |
| tulajdonságok. LastScaleActionTime | hello időbélyegzője amikor hello automatikus skálázási művelet történt. |
| status |Hello művelet hello állapotát leíró karakterlánc. Néhány gyakori értékek a következők: folyamatban van, sikeres, sikertelen, aktív, a lépések megoldva. |
| a részállapot | Az automatikus skálázás általában null értékű. |
| eventTimestamp |Hello esemény lett létrehozva, amikor hello Azure szolgáltatás feldolgozása hello idejét jelző időbélyegző megfelelő hello esemény kérelmet. |
| submissionTimestamp |Hello esemény váltak elérhetővé lekérdezése idejét jelző időbélyegző. |
| subscriptionId |Az Azure előfizetés-azonosító. |

## <a name="next-steps"></a>Következő lépések
* [További tudnivalók hello tevékenységnapló (korábbi nevén naplók)](monitoring-overview-activity-logs.md)
* [Adatfolyam-hello Azure tevékenységnapló tooEvent hubok](monitoring-stream-activity-logs-event-hubs.md)
