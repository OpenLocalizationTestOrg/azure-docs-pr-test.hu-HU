---
title: "Azure metrika riasztások aaaConfigure webhookok |} Microsoft Docs"
description: "Az Azure riasztások tooother-Azure rendszerek átirányítása."
author: johnkemnetz
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 8b3ae540-1d19-4f3d-a635-376042f8a5bb
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: johnkem
ms.openlocfilehash: bc4153ccdcff41c5b9d3c081e59a1bf260d8a283
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-webhook-on-an-azure-metric-alert"></a>A webhook Azure metrika riasztás konfigurálása
Webhook lehetővé teszik az Azure tooroute riasztási értesítés tooother rendszerek utófeldolgozási vagy egyéni műveletek. A webhook egy riasztási tooroute használhatja azt, az SMS küldő tooservices hibák jelentkezzen, egy csapat Csevegés/üzenetküldési szolgáltatásokban keresztül értesíti, vagy más műveletek többféle. Ez a cikk ismerteti, hogyan tooset olyan webhook Azure metrika riasztást, és milyen hello hasznos hello HTTP POST tooa webhook tűnik. Információ a hello beállítása és a séma (riasztás események), Azure tevékenységnapló riasztás [helyette látja ezt a lapot](insights-auditlog-to-webhook-email.md).

Azure riasztások HTTP POST hello figyelmeztetés tartalma JSON formátumban séma definiált tooa webhook URI, amely hello riasztás létrehozásakor adja meg az alábbi. Ezt az URI érvényes HTTP vagy HTTPS-végpont kell lennie. Azure visszaküldés kérelmenként egy bejegyzést, egy riasztás aktiválásakor.

## <a name="configuring-webhooks-via-hello-portal"></a>Konfigurálása webhookokkal hello portálon
Hozzáadhat és hello webhook URI hello Create/Update riasztások képernyőjén hello frissítése [portal](https://portal.azure.com/).

![Riasztási szabály felvétele](./media/insights-webhooks-alerts/Alertwebhook.png)

Beállíthatja úgy is egy riasztási toopost tooa webhook URI használatával hello [Azure PowerShell-parancsmagok](insights-powershell-samples.md#create-metric-alerts), [platformfüggetlen parancssori felület](insights-cli-samples.md#work-with-alerts), vagy [Azure figyelő REST API](https://msdn.microsoft.com/library/azure/dn933805.aspx).

## <a name="authenticating-hello-webhook"></a>Hello webhook hitelesítése
hello webhook engedélyezési jogkivonat-alapú használatával képes hitelesíteni. hello webhook URI-t menti jogkivonat-azonosítóval, pl.. `https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue`

## <a name="payload-schema"></a>Hasznos séma
POST művelet hello JSON-adattartalmat és az összes metrika-alapú értesítések séma a következő hello tartalmazza.

```JSON
{
"status": "Activated",
"context": {
            "timestamp": "2015-08-14T22:26:41.9975398Z",
            "id": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.insights/alertrules/ruleName1",
            "name": "ruleName1",
            "description": "some description",
            "conditionType": "Metric",
            "condition": {
                        "metricName": "Requests",
                        "metricUnit": "Count",
                        "metricValue": "10",
                        "threshold": "10",
                        "windowSize": "15",
                        "timeAggregation": "Average",
                        "operator": "GreaterThanOrEqual"
                },
            "subscriptionId": "s1",
            "resourceGroupName": "useast",                                
            "resourceName": "mysite1",
            "resourceType": "microsoft.foo/sites",
            "resourceId": "/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1",
            "resourceRegion": "centralus",
            "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/useast/providers/microsoft.foo/sites/mysite1"
},
"properties": {
              "key1": "value1",
              "key2": "value2"
              }
}
```


| Mező | Kötelező | Rögzített értékhalmazt | Megjegyzések |
|:--- |:--- |:--- |:--- |
| status |I |"Aktív", "Megoldott" |Állított ki hello feltételek alapján hello riasztás állapota. |
| A környezetben |I | |hello riasztási környezetben. |
| időbélyeg |I | |hello idő, mely hello riasztás működésbe lépett. |
| id |I | |Minden riasztási szabály van egyedi azonosítója. |
| név |I | |hello riasztás nevét. |
| leírás |I | |Hello riasztás leírása. |
| conditionType |I |"Metrika", az "Event" |A riasztások két típusok támogatottak. Egy olyan metrika és hello más hello tevékenységnapló az esemény alapján. Az érték toocheck akkor használja, ha hello riasztás metrika vagy esemény alapján. |
| Az állapot |I | |hello adott mezők toocheck a hello conditionType alapján. |
| metricName |a metrika riasztások | |hello nevét, amely meghatározza, hogy milyen hello szabály hello mérőszám figyeli. |
| metricUnit |a metrika riasztások |"Memória", "BytesPerSecond", "Count", "CountPerSecond", "Százaléka", "Seconds" |hello egység a hello metrika engedélyezett. [Engedélyezett értékek az itt felsorolt](https://msdn.microsoft.com/library/microsoft.azure.insights.models.unit.aspx). |
| metricValue |a metrika riasztások | |hello hello metrika hello riasztást generáló aktuális értékét. |
| Küszöbérték |a metrika riasztások | |hello küszöbértéket, mely hello riasztás aktiválva van. |
| Ablakméret |a metrika riasztások | |hello időtartam alatt használt toomonitor riasztási tevékenység hello küszöbérték alapján. 5 perc és 1 nap között kell lennie. ISO 8601 időtartama formátumban. |
| timeAggregation |a metrika riasztások |"Átlagos", "Legutóbbi", "Maximális", "Minimális", "None", "teljes" |Hogyan hello adatokat gyűjtött össze kell vonni adott idő alatt. hello alapértelmezett érték átlaga. [Engedélyezett értékek az itt felsorolt](https://msdn.microsoft.com/library/microsoft.azure.insights.models.aggregationtype.aspx). |
| Operátor |a metrika riasztások | |hello operátor toocompare hello aktuális metrikaadatokat toohello beállított küszöbértéket. |
| subscriptionId |I | |Az Azure előfizetés-azonosító. |
| erőforráscsoport-név |I | |Hello hello erőforráscsoport nevét érintett erőforrás. |
| resourceName |I | |Erőforrás neve hello érintett erőforrás. |
| a resourceType |I | |Hello típusú erőforrás érintett erőforrás. |
| resourceId |I | |Erőforrás-azonosítója, hello érintett erőforrás. |
| resourceRegion |I | |Régió vagy hello helyét érintett erőforrás. |
| portalLink |I | |A közvetlen hivatkozás toohello portál erőforrás összefoglaló oldala. |
| properties |N |Optional |Állítsa be a `<Key, Value>` párok (azaz `Dictionary<String, String>`), amely hello esemény részleteit tartalmazza. hello tulajdonságok mező kitöltése nem kötelező. Egy munkafolyamatban egyéni felhasználói felületén vagy a Logic app-alapú felhasználók kulcs és-értékek, amelyek átadhatók keresztül hello hasznos adhat meg. hello más módja toopass egyéni tulajdonságok hátsó toohello webhook hello webhook URI-ját magát (lekérdezési paraméterek) keresztül van |

> [!NOTE]
> hello tulajdonságok mező csak akkor állítható hello segítségével [Azure figyelő REST API](https://msdn.microsoft.com/library/azure/dn933805.aspx).
>
>

## <a name="next-steps"></a>Következő lépések
* További tudnivalók Azure riasztások és webhookokkal hello a videó [PagerDuty integrálni Azure riasztások](http://go.microsoft.com/fwlink/?LinkId=627080)
* [Azure Automation-parancsfájlok (Runbookok) végrehajtása Azure riasztások](http://go.microsoft.com/fwlink/?LinkId=627081)
* [Használja a logikai alkalmazás toosend keresztül Twilio SMS az Azure-riasztás alapján](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
* [Használja a logikai alkalmazás toosend Slack üzenet az Azure-riasztás alapján](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
* [Használja a logikai alkalmazás toosend üzenet tooan Azure Queue az Azure-riasztás alapján](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)
