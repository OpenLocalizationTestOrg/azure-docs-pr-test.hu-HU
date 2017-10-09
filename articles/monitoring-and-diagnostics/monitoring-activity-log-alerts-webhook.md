---
title: "a napló tevékenységriasztásokat használt aaaUnderstand hello webhook séma |} Microsoft Docs"
description: "További információk a hello sémája hello JSON, amikor egy tevékenység napló riasztás aktiválja feladott tooa webhook URL-CÍMÉT."
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
ms.date: 03/31/2017
ms.author: johnkem
ms.openlocfilehash: 75562e0589222d3e392ea73eacfd7414a422d115
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="webhooks-for-azure-activity-log-alerts"></a>Az Azure tevékenység napló riasztásokhoz Webhookok
Hello definíciójának művelet csoport részeként webhook végpontok tooreceive tevékenység napló riasztási értesítéseket is konfigurálhat. A webhook irányíthatja a utófeldolgozási vagy egyéni műveletek értesítések tooother rendszerek. Ez a cikk bemutatja, milyen hello hasznos hello HTTP POST tooa webhook tűnik.

További információ a napló tevékenységriasztásokat: hogyan túl[Azure tevékenység létrehozása napló riasztások](monitoring-activity-log-alerts.md).

Információk a művelet csoportokon: hogyan túl[művelet csoportok létrehozása a](monitoring-action-groups.md).

## <a name="authenticate-hello-webhook"></a>Hello webhook hitelesítéséhez
hello webhook opcionálisan használhatja engedélyezési jogkivonat-alapú hitelesítésre. a webhook URI mentett jogkivonat-azonosítóval, például hello `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`.

## <a name="payload-schema"></a>Hasznos séma
hello JSON-adattartalmat hello FELADÁS egy vagy több művelet található helytől hello hasznos data.context.activityLog.eventSource mező.

###<a name="common"></a>Közös
```json
{
    "schemaId": "Microsoft.Insights/activityLogs",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "channels": "Operation",
                "correlationId": "6ac88262-43be-4adf-a11c-bd2179852898",
                "eventSource": "Administrative",
                "eventTimestamp": "2017-03-29T15:43:08.0019532+00:00",
                "eventDataId": "8195a56a-85de-4663-943e-1a2bf401ad94",
                "level": "Informational",
                "operationName": "Microsoft.Insights/actionGroups/write",
                "operationId": "6ac88262-43be-4adf-a11c-bd2179852898",
                "status": "Started",
                "subStatus": "",
                "subscriptionId": "52c65f65-0518-4d37-9719-7dbbfc68c57a",
                "submissionTimestamp": "2017-03-29T15:43:20.3863637+00:00",
                ...
            }
        },
        "properties": {}
    }
}
```
###<a name="administrative"></a>Felügyeleti
```json
{
    "schemaId": "Microsoft.Insights/activityLogs",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "authorization": {
                    "action": "Microsoft.Insights/actionGroups/write",
                    "scope": "/subscriptions/52c65f65-0518-4d37-9719-7dbbfc68c57b/resourceGroups/CONTOSO-TEST/providers/Microsoft.Insights/actionGroups/IncidentActions"
                },
                "claims": "{...}",
                "caller": "me@contoso.com",
                "description": "",
                "httpRequest": "{...}",
                "resourceId": "/subscriptions/52c65f65-0518-4d37-9719-7dbbfc68c57b/resourceGroups/CONTOSO-TEST/providers/Microsoft.Insights/actionGroups/IncidentActions",
                "resourceGroupName": "CONTOSO-TEST",
                "resourceProviderName": "Microsoft.Insights",
                "resourceType": "Microsoft.Insights/actionGroups"
            }
        },
        "properties": {}
    }
}

```
###<a name="servicehealth"></a>ServiceHealth
```json
{
    "schemaId": "unknown",
    "data": {
        "status": "Activated",
        "context": {
            "activityLog": {
                "properties": {
                    "title": "...",
                    "service": "...",
                    "region": "...",
                    "communication": "...",
                    "incidentType": "Incident",
                    "trackingId": "...",
                    "groupId": "...",
                    "impactStartTime": "3/29/2017 3:43:21 PM",
                    "impactMitigationTime": "3/29/2017 3:43:21 PM",
                    "eventCreationTime": "3/29/2017 3:43:21 PM",
                    "impactedServices": "[{...}]",
                    "defaultLanguageTitle": "...",
                    "defaultLanguageContent": "...",
                    "stage": "Active",
                    "communicationId": "...",
                    "version": "0.1"
                }
            }
        },
        "properties": {}
    }
}
```

A szolgáltatás állapotának értesítési tevékenység napló riasztások adott séma részletekért lásd: [szolgáltatás állapotával kapcsolatos értesítésekre](monitoring-service-notifications.md).

Más tevékenység napló minden riasztásról adott séma részletekért lásd: [hello Azure tevékenységnapló áttekintése](monitoring-overview-activity-logs.md).

| Elem neve | Leírás |
| --- | --- |
| status |A metrika riasztások használt. Mindig beállította túl "aktivált" tevékenység napló riasztások esetén. |
| A környezetben |Hello esemény környezetében. |
| resourceProviderName |erőforrás-szolgáltató hello hello az érintett erőforrás. |
| conditionType |Mindig "Event". |
| név |Hello riasztási szabály neve. |
| id |Erőforrás-azonosító hello riasztás. |
| leírás |Ha hello riasztást hoz létre a riasztás leírásában. |
| subscriptionId |Az Azure előfizetés-azonosító. |
| időbélyeg |Mely hello esemény lett létrehozva, amikor hello Azure szolgáltatás, amelynek a feldolgozása hello kérelem ideje. |
| resourceId |Erőforrás-azonosítója, hello érintett erőforrás. |
| erőforráscsoport-név |Hello hello erőforráscsoport nevét érintett erőforrás. |
| properties |Állítsa be a `<Key, Value>` párok (Ez azt jelenti, hogy `Dictionary<String, String>`), amely hello esemény részleteit tartalmazza. |
| Esemény |Hello eseménnyel kapcsolatos metaadatokat tartalmazó elemet. |
| Engedélyezési |Szerepköralapú hozzáférés-vezérlés tulajdonságai hello hello esemény. Ezek a Tulajdonságok általában például hello művelet hello szerepkör és hello hatókör. |
| category |Hello esemény kategóriáját. Támogatott értékek a következők: felügyeleti, figyelmeztetés, biztonsági, ServiceHealth és javaslat. |
| hívó |Hello művelet, a jogcím vagy a rendelkezésre állás alapján SPN jogcím hello felhasználó e-mail címe. Bizonyos rendszerhívások null értékű lehet. |
| correlationId |Általában egy GUID karakterlánc-formátum. A correlationId események tartozik toohello nagyobb műveletet és általában megosztani az egy correlationId. |
| eventDescription |Statikus szöveg hello esemény leírását. |
| eventDataId |Hello esemény egyedi azonosítója. |
| Eseményforrás |Neve hello Azure-szolgáltatásra vagy infrastruktúrát, hogy létrehozott hello esemény. |
| httpRequest |hello kérelem általában tartalmazza hello clientRequestId clientIpAddress és HTTP-metódust (például PUT). |
| szint |Hello a következő értékek egyikét: kritikus, hiba, figyelmeztetés, tájékoztatás és részletes. |
| OperationID azonosítójú |Általában egy GUID azonosító toosingle művelet megfelelő hello események között meg van osztva. |
| operationName |Hello művelet neve. |
| properties |Hello esemény tulajdonságai. |
| status |Karakterlánc. Hello művelet állapotát. A gyakori értékek a következők: elindítva, folyamatban lévő, sikeres, sikertelen, aktív és megoldva. |
| a részállapot |Általában tartalmazza a megfelelő REST-hívást hello hello HTTP-állapotkódot. A részállapot más karakterláncokat is tartalmazhat. Közös részállapot érték OK (HTTP-állapotkód:: 200), létrehozott (HTTP-állapotkód:: 201-es), fogadja el (HTTP-állapotkód:: 202), a tartalom nem (HTTP-állapotkód: 204), hibás kérés (HTTP-állapotkód:: 400), nem található (HTTP-állapotkód: 404-es), ütközés (HTTP-állapotkód:: 409 ), Belső kiszolgálóhiba (HTTP-állapotkód: 500), a Service nem érhető el (HTTP-állapotkód: 503-as), és az átjáró időtúllépése (HTTP-állapotkód: 504). |

## <a name="next-steps"></a>Következő lépések
* [További tudnivalók hello tevékenységnapló](monitoring-overview-activity-logs.md).
* [Hajtsa végre az Azure automation-parancsfájlok (Runbookok) Azure riasztások](http://go.microsoft.com/fwlink/?LinkId=627081).
* [Használja a logic app toosend keresztül Twilio SMS az Azure-riasztás alapján](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). Ebben a példában a metrika riasztások, de egy tevékenység napló alert módosított toowork lehet.
* [A logic app toosend Azure riasztást Slack üzenetét használja](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). Ebben a példában a metrika riasztások, de egy tevékenység napló alert módosított toowork lehet.
* [Használja a logic app toosend, egy üzenet tooan Azure feldolgozási sor az Azure-riasztás alapján](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). Ebben a példában a metrika riasztások, de egy tevékenység napló alert módosított toowork lehet.
