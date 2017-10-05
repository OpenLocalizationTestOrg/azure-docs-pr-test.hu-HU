---
title: "Riasztások az OMS szolgáltatáshoz válaszokat |} Microsoft Docs"
description: "Naplóelemzési riasztások határozza meg az OMS-adattárban lévő fontos adatokat és is proaktív értesítést küldenek, problémák vagy meghívása műveletek kijavításának őket.  Ez a cikk ismerteti a riasztási szabály és a részletek a különböző általuk végezhető műveletek létrehozása."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 6cfd2a46-b6a2-4f79-a67b-08ce488f9a91
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/28/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b8731e1fe48b7d809b113eb5273e3962542b8f34
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="add-actions-to-alert-rules-in-log-analytics"></a>Műveletek hozzáadni a Naplóelemzési riasztási szabályok
Ha egy [riasztást hoz létre a Naplóelemzési](log-analytics-alerts.md), lehetősége van a [a riasztási szabály konfigurálása](log-analytics-alerts.md) egy vagy több műveletek elvégzéséhez.  Ez a cikk ismerteti a rendelkezésre álló különféle műveletek és a részletek konfigurálásával összes típusához.

| Műveletek | Leírás |
|:--|:--|
| [E-mailek](#email-actions) | A riasztás részleteit e-mailt küldeni egy vagy több címzett. |
| [Webhook](#webhook-actions) | Egy külső folyamatban egy HTTP POST kérelemben keresztül meghívni. |
| [A Runbook](#runbook-actions) | Elindít egy forgatókönyvet az Azure Automation. |


## <a name="email-actions"></a>E-mailek műveletek
E-mailek műveletek a riasztás részleteit e-mail küldése egy vagy több címzett.  Megadhatja, hogy az e-mail tárgya, de annak tartalma Naplóelemzési által összeállított szabványos formátumban.  Ez magában foglalja például a nevét a riasztás részleteit a napló keresés által visszaadott legfeljebb tíz rekordok mellett összegző információkat.  A Naplóelemzési, visszatér a rekordok teljes készletét, hogy a lekérdezés egy napló keresés hivatkozást is tartalmaz.   Az üzenet küldője *a Microsoft Operations Management Suite Team &lt; noreply@oms.microsoft.com &gt;* . 

E-mailek műveletek igényelnek a tulajdonságok az alábbi táblázatban.

| Tulajdonság | Leírás |
|:--- |:--- |
| Tárgy |A tulajdonos e-mailben.  Az üzenet törzse nem módosítható. |
| Címzettek |Címzett e-mail címét.  Ha több címet ad meg, majd külön a címeket pontosvesszővel (;). |


## <a name="webhook-actions"></a>Webhookműveletek

Webhookműveletek lehetővé teszi egy külső folyamatban egy HTTP POST kérelemben keresztül.  A meghívott szolgáltatás kell webhookok támogatja, és határozza meg, hogyan fogja használni a tartalom kap.  Sikerült is meghívhatja a REST API-t nem támogató kifejezetten webhookokkal, amíg a kérelmet, amely együttműködik a API formátumban.  Példák a webhook riasztást válaszul egy üzenetet küldi [Slackhez](http://slack.com) , vagy hozzon létre egy incidenst a [PagerDuty](http://pagerduty.com/).  Riasztási szabály létrehozása a Slackhez hívása olyan webhook részletes útmutatást érhető el: [Naplóelemzési riasztásokban Webhookok](log-analytics-alerts-webhooks.md).

Webhookműveletek igényelnek a tulajdonságok az alábbi táblázatban.

| Tulajdonság | Leírás |
|:--- |:--- |
| Webhook URL-CÍMÉT |A webhook URL-CÍMÉT. |
| Egyéni JSON-adattartalmat |A webhook küldendő egyéni hasznos.  További információ alább olvasható. |


Webhook URL-címet és a hasznos adatok között, amely a külső szolgáltatásnak továbbított adatok JSON formátumú, tartalmazza.  Alapértelmezés szerint a tartalom a következő táblázat tartalmazza az értékeket.  Ha szeretné, ezek a hasznos adatok kicseréli a saját egyéni egy.  Ebben az esetben használhatja a változók a táblázatban az egyes paraméterek egyéni adattartalmat értékük felvenni.

| Paraméter | Változó | Leírás |
|:--- |:--- |:--- |
| AlertRuleName |#alertrulename |A riasztási szabály neve. |
| AlertThresholdOperator |#thresholdoperator |A riasztási szabály operátor küszöbértéket.  *Nagyobb, mint* vagy *kisebb, mint*. |
| AlertThresholdValue |#thresholdvalue |A riasztási szabály tartozó küszöbérték. |
| LinkToSearchResults |#linktosearchresults |A lekérdezésből, amely a riasztás létrehozása a rekordok visszaadó Naplóelemzési napló keresési kapcsolódik. |
| Attribútumhoz resultcount számlálót. |#searchresultcount |A keresési eredmények rekordok száma. |
| SearchIntervalEndtimeUtc |#searchintervalendtimeutc |Befejezési ideje UTC formátumban a lekérdezést. |
| SearchIntervalInSeconds |#searchinterval |A riasztási szabály időszak. |
| SearchIntervalStartTimeUtc |#searchintervalstarttimeutc |Indítsa el a lekérdezések ideje UTC formátumban. |
| SearchQuery |#searchquery |Naplófájl-keresési lekérdezés a riasztási szabály által használt. |
| SearchResults |Lásd az alábbi |A lekérdezés JSON formátumban rögzíti.  Az első 5000 rekordok korlátozva. |
| WorkspaceID |#workspaceid |Az OMS-munkaterület azonosítója. |

Például megadhatja a következő egyéni payload nevű egyetlen paramétert tartalmazó *szöveg*.  A szolgáltatás, amely behívja a webhook a ennek a paraméternek, akkor rendszer.

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }

Ez a példa hasznos volna oldja fel a következőhöz, ha a webhook.

    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }

A keresési eredmények belefoglalása az egyéni adattartalom, adja hozzá a következő sort a json-adattartalmat legfelső szintű tulajdonság.  

    "IncludeSearchResults":true

Például egy egyéni adattartalom, amely tartalmazza a riasztás neve és a keresési eredmények létrehozásához használhatja a következő. 

    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }


Riasztási szabály létrehozása a webhook: egy külső szolgáltatás indítására átfogó példát keresztül végigvezetheti [riasztási webhook művelet létrehozása az OMS szolgáltatáshoz üzenetet küldeni a Slackhez](log-analytics-alerts-webhooks.md).

## <a name="runbook-actions"></a>Runbook-műveletek
Runbook műveletek az Azure Automationben runbook indítása.  Ahhoz, hogy a művelet típusát, rendelkeznie kell a [Folyamatautomatizálási megoldása](log-analytics-add-solutions.md) telepítve, és az OMS-munkaterület konfigurálva.  Az automation-fiókban az Automation-megoldás beállított runbookok közül választhat.

Runbook műveleteket igényelnek a tulajdonságok az alábbi táblázatban.

| Tulajdonság | Leírás |
|:--- |:---|
| Forgatókönyv | Ez a Runbook elindítja a riasztás létrehozásakor. |
| Futtassa a | Adja meg **Azure** a runbook futtatásához a felhőben.  Adja meg **hibridfeldolgozó** a runbook futhat az ügynök [hibrid forgatókönyv-feldolgozó](../automation/automation-hybrid-runbook-worker.md ) telepítve.  |

Runbook műveletek indíthatja a runbook egy [webhook](../automation/automation-webhooks.md).  A riasztási szabály létrehozásakor az automatikusan létrehoz egy új webhook a runbook nevű **OMS riasztás szervizelési** GUID követ.  

Nem közvetlenül tölthető fel a runbook paramétereket, de a [$WebhookData paraméter](../automation/automation-webhooks.md) a riasztást, többek között az azt létrehozó napló keresési eredmények részleteit tartalmazza.  A runbook kell megadni **$WebhookData** úgy, hogy a riasztás tulajdonságai érhetők el a paramétert.  A riasztási adatokat érhető el egyetlen tulajdonságot, json formátumban **SearchResults** a a **RequestBody** tulajdonsága **$WebhookData**.  Ez az alábbi táblázatban a tulajdonságokkal fog rendelkezni.

| Csomópont | Leírás |
|:--- |:--- |
| id |Elérési út és a keresés GUID. |
| __metadata |A riasztás rekordok száma és a keresési eredmények állapotát kapcsolatos információkat. |
| érték |A keresési eredmények rekordokban külön bejegyzést.  A bejegyzés a részleteket a tulajdonságok és értékek a rekord fog egyezni. |

Például a következő runbook Ehhez bontsa ki a napló keresés által visszaadott rekordok, és rendelje hozzá minden egyes bejegyzés típusától függően különböző tulajdonságokat.  Vegye figyelembe, hogy a runbook elindítja átalakításával **RequestBody** JSON úgy, hogy az erőforrások az PowerShell objektumként.

    param ( 
        [object]$WebhookData
    )

    $RequestBody = ConvertFrom-JSON -InputObject $WebhookData.RequestBody
    $Records     = $RequestBody.SearchResults.value

    foreach ($Record in $Records)
    {
        $Computer = $Record.Computer

        if ($Record.Type -eq 'Event')
        {
            $EventNo    = $Record.EventID
            $EventLevel = $Record.EventLevelName
            $EventData  = $Record.EventData
        }

        if ($Record.Type -eq 'Perf')
        {
            $Object    = $Record.ObjectName
            $Counter   = $Record.CounterName
            $Instance  = $Record.InstanceName
            $Value     = $Record.CounterValue
        }
    }


## <a name="next-steps"></a>Következő lépések
- A forgatókönyv a [konfigurálása egy webook](log-analytics-alerts-webhooks.md) a riasztási szabályt.  
- Ismerje meg, hogyan írhat [az Azure Automation runbookjai](https://azure.microsoft.com/documentation/services/automation) riasztások által azonosított problémák megoldásáról.