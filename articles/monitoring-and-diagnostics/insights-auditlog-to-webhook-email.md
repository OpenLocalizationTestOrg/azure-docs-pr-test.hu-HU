---
title: "a webhook Azure tevékenységnapló riasztások aaaCall |} Microsoft Docs"
description: "Útvonal tevékenység naplózási események tooother-szolgáltatások egyéni műveletek. Például SMS küldése, jelentkezzen hibák, vagy egy csoport Csevegés/üzenetküldési szolgáltatáson keresztül értesíti."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 64d333d1-7f37-4a00-9d16-dda6e69a113b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: johnkem
ms.openlocfilehash: 9017ff3e5165857ec7084a8f07f4123552e55f73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="call-a-webhook-on-azure-activity-log-alerts"></a>A webhook hívható meg Azure tevékenységnapló riasztások
Webhook lehetővé teszik az Azure tooroute riasztási értesítés tooother rendszerek utófeldolgozási vagy egyéni műveletek. A webhook egy riasztási tooroute használhatja azt, az SMS küldő tooservices hibák jelentkezzen, egy csapat Csevegés/üzenetküldési szolgáltatásokban keresztül értesíti, vagy más műveletek többféle. Ez a cikk ismerteti, hogyan tooset egy webhook toobe meghívva, amikor egy Azure tevékenységnapló riasztási következik be. Azt is bemutatja, milyen hello hasznos hello HTTP POST tooa webhook tűnik. Hello beállítása és a séma Azure metrika riasztás [helyette látja ezt a lapot](insights-webhooks-alerts.md). Tevékenységnapló riasztási toosend e-mailt aktiválásakor is állíthat be.

> [!NOTE]
> Ez a funkció jelenleg előzetes verzióban érhetők, és a jövőbeli hello bármikor törlődni fog.
>
>

Hello segítségével tevékenységnapló riasztást állíthat be [Azure PowerShell-parancsmagok](insights-powershell-samples.md#create-metric-alerts), [platformfüggetlen parancssori felület](insights-cli-samples.md#work-with-alerts), vagy [Azure figyelő REST API](https://msdn.microsoft.com/library/azure/dn933805.aspx). Jelenleg nem állítható be Azure-portálon hello segítségével egy.

## <a name="authenticating-hello-webhook"></a>Hello webhook hitelesítése
hello webhook képes hitelesíteni az alábbi módszerek valamelyikével:

1. **Jogkivonat-alapú engedélyezési** -webhook URI mentett jogkivonat-azonosítóval, például hello`https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`
2. **Alapszintű engedélyezési** -webhook URI mentik felhasználónévvel és jelszóval, például hello`https://userid:password@mysamplealert/webcallback?someparamater=somevalue&foo=bar`

## <a name="payload-schema"></a>Hasznos séma
POST művelet hello JSON-adattartalmat és az összes tevékenységnapló alapú értesítések séma a következő hello tartalmazza. Ebben a sémában metrika-alapú értesítések által használt egyik hasonló toohello.

```
{
        "status": "Activated",
        "context": {
                "resourceProviderName": "Microsoft.Web",
                "event": {
                        "$type": "Microsoft.WindowsAzure.Management.Monitoring.Automation.Notifications.GenericNotifications.Datacontracts.InstanceEventContext, Microsoft.WindowsAzure.Management.Mon.Automation",
                        "authorization": {
                                "action": "Microsoft.Web/sites/start/action",
                                "scope": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest"
                        },
                        "eventDataId": "327caaca-08d7-41b1-86d8-27d0a7adb92d",
                        "category": "Administrative",
                        "caller": "myname@mycompany.com",
                        "httpRequest": {
                                "clientRequestId": "f58cead8-c9ed-43af-8710-55e64def208d",
                                "clientIpAddress": "104.43.166.155",
                                "method": "POST"
                        },
                        "status": "Succeeded",
                        "subStatus": "OK",
                        "level": "Informational",
                        "correlationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "eventDescription": "",
                        "operationName": "Microsoft.Web/sites/start/action",
                        "operationId": "4a40beaa-6a63-4d92-85c4-923a25abb590",
                        "properties": {
                                "$type": "Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage",
                                "statusCode": "OK",
                                "serviceRequestId": "f7716681-496a-4f5c-8d14-d564bcf54714"
                        }
                },
                "timestamp": "Friday, March 11, 2016 9:13:23 PM",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/alertonevent2",
                "name": "alertonevent2",
                "description": "test alert on event start",
                "conditionType": "Event",
                "subscriptionId": "s1",
                "resourceId": "/subscriptions/s1/resourcegroups/rg1/providers/Microsoft.Web/sites/leoalerttest",
                "resourceGroupName": "rg1"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```

| Elem neve | Leírás |
| --- | --- |
| status |A metrika riasztások használt. Mindig beállította túl "aktivált" tevékenységnapló riasztások esetén. |
| A környezetben |Hello esemény környezetében. |
| resourceProviderName |erőforrás-szolgáltató hello hello az érintett erőforrás. |
| conditionType |Mindig "Event". |
| név |Hello riasztási szabály neve. |
| id |Erőforrás-azonosító hello riasztás. |
| leírás |Riasztás leírása az beállított a hello riasztás létrehozása során. |
| subscriptionId |Az Azure előfizetés-azonosító. |
| időbélyeg |Mely hello esemény lett létrehozva, amikor hello Azure szolgáltatás, amelynek a feldolgozása hello kérelem ideje. |
| resourceId |Erőforrás-azonosítója, hello érintett erőforrás. |
| erőforráscsoport-név |Hello hello erőforráscsoport nevét érintett erőforrás |
| properties |Állítsa be a `<Key, Value>` párok (azaz `Dictionary<String, String>`), amely hello esemény részleteit tartalmazza. |
| Esemény |Hello eseménnyel kapcsolatos metaadatokat tartalmazó elemnek. |
| Engedélyezési |hello RBAC hello esemény tulajdonságai. Ezek általában például hello "action", "szerepkör" és hello "hatókörben." |
| category |Hello esemény kategóriáját. Támogatott értékek a következők: felügyeleti, a riasztásra, biztonság, ServiceHealth, javaslat. |
| hívó |Hello művelet, a jogcím vagy a rendelkezésre állás alapján SPN jogcím hello felhasználó e-mail címe. Bizonyos rendszerhívások null értékű lehet. |
| correlationId |Általában egy GUID karakterlánc-formátum. A correlationId események tartozik toohello nagyobb műveletet és általában megosztani az egy correlationId. |
| eventDescription |Statikus szöveg hello esemény leírását. |
| eventDataId |Hello esemény egyedi azonosítója. |
| Eseményforrás |Neve hello Azure-szolgáltatásra vagy infrastruktúrát, hogy létrehozott hello esemény. |
| httpRequest |Általában hello "clientRequestId", "clientIpAddress" és "method" tartalmazza (pl. PUT HTTP-metódus). |
| szint |Hello a következő értékek egyikét: "Kritikus", "Error", "Figyelmeztetés", "Tájékoztató" és "Részletes." |
| OperationID azonosítójú |Általában egy GUID azonosító toosingle művelet megfelelő hello események között meg van osztva. |
| operationName |Hello művelet neve. |
| properties |Hello esemény tulajdonságai. |
| status |Karakterlánc. Hello művelet állapotát. Gyakori értékek a következők: "Started", "Folyamatban", "Sikeres", "Sikertelen", "Active", "Megoldott". |
| a részállapot |Általában tartalmazza a megfelelő REST-hívást hello hello HTTP-állapotkódot. A részállapot leíró más karakterláncok is tartalmazhat. Közös részállapot értékek a következők: OK (HTTP-állapotkód:: 200), létrehozott (HTTP-állapotkód:: 201-es), fogadja el (HTTP-állapotkód:: 202), nem a tartalom (HTTP-állapotkód: 204), hibás kérés (HTTP-állapotkód:: 400), nem található (HTTP-állapotkód:: 404-es), ütközés (HTTP-állapotkód:: 409), belső kiszolgálóhiba (HTTP-állapotkód:: 500), szolgáltatás nem érhető el (HTTP-állapotkód:: 503-as), átjáró időtúllépése (HTTP-állapotkód: 504) |

## <a name="next-steps"></a>Következő lépések
* [További tudnivalók hello műveletnapló](monitoring-overview-activity-logs.md)
* [Azure Automation-parancsfájlok (Runbookok) végrehajtása Azure riasztások](http://go.microsoft.com/fwlink/?LinkId=627081)
* [Használja a logikai alkalmazás toosend keresztül Twilio SMS az Azure-riasztás alapján](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app). Ebben a példában a metrika riasztások, de egy tevékenységnapló alert módosított toowork lehet.
* [Használja a logikai alkalmazás toosend Azure riasztást Slack üzenetét](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app). Ebben a példában a metrika riasztások, de egy tevékenységnapló alert módosított toowork lehet.
* [Használja az Azure-riasztás alapján üzenet tooan Azure Queue logikai alkalmazás toosend](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app). Ebben a példában a metrika riasztások, de egy tevékenységnapló alert módosított toowork lehet.
