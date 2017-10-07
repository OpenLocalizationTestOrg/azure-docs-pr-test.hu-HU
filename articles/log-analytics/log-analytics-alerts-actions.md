---
title: "az OMS szolgáltatáshoz aaaResponses tooalerts |} Microsoft Docs"
description: "Log Analytics riasztások határozza meg az OMS-adattárban lévő fontos adatokat és is proaktív értesítést küldenek, problémák vagy meg kíván hívni műveletek tooattempt toocorrect őket.  Ez a cikk ismerteti, hogyan toocreate riasztási szabály és a részletek hello különféle műveletek tarthat."
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
ms.openlocfilehash: d24bb726a96e7143985f111c0599dc4e7898b4f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-actions-tooalert-rules-in-log-analytics"></a>A Naplóelemzési műveletek tooalert szabályok hozzáadása
Ha egy [riasztást hoz létre a Naplóelemzési](log-analytics-alerts.md), lehetősége van hello a [konfigurálása hello riasztási szabály](log-analytics-alerts.md) tooperform egy vagy több művelet.  Ez a cikk ismerteti az elérhető különböző műveleteket hello és a részletek konfigurálásával összes típusához.

| Műveletek | Leírás |
|:--|:--|
| [E-mailek](#email-actions) | Hello adatokkal hello riasztási tooone vagy további címzettek e-mail küldése. |
| [Webhook](#webhook-actions) | Egy külső folyamatban egy HTTP POST kérelemben keresztül meghívni. |
| [A Runbook](#runbook-actions) | Elindít egy forgatókönyvet az Azure Automation. |


## <a name="email-actions"></a>E-mailek műveletek
E-mailek műveletek hello adatokkal hello riasztási tooone vagy további címzettek e-mail küldése.  Hello e-mail tárgya hello is megadhat, de annak tartalma Naplóelemzési által összeállított szabványos formátumban.  A hello napló keresés által visszaadott tooten rekordok, továbbá toodetails hello riasztás például hello nevét összefoglaló információkat tartalmazza.  A Naplóelemzési, visszatér a rekordok hello teljes készletét, hogy a lekérdezés hivatkozás tooa napló keresés is tartalmaz.   hello hello mail küldője *a Microsoft Operations Management Suite Team &lt; noreply@oms.microsoft.com &gt;* . 

E-mailek műveletek a következő táblázat hello hello tulajdonságok szükséges.

| Tulajdonság | Leírás |
|:--- |:--- |
| Tárgy |Tulajdonos hello e-mailben.  Üdvözlő e-mail törzsét hello nem módosítható. |
| Címzettek |Címzett e-mail címét.  Ha ad meg egynél több címet, majd külön hello címeket pontosvesszővel (;). |


## <a name="webhook-actions"></a>Webhookműveletek

Webhookműveletek lehetővé teszik a tooinvoke keresztül egy HTTP POST kérelemben egy külső folyamatban.  meghívott hello szolgáltatást kell webhookok támogatja, és határozza meg, hogyan fogja használni a tartalom kap.  Emellett a REST API-t nem támogató kifejezetten webhookokkal, amíg hello kérést a formátumban kell megadni, hogy megértette a API hello hívhatja meg.  Példák a webhook válasz tooan figyelmeztető üzenet üzenetet küld [Slackhez](http://slack.com) , vagy hozzon létre egy incidenst a [PagerDuty](http://pagerduty.com/).  A webhook toocall Slackhez a riasztási szabály létrehozásának részletes útmutatást érhető el: [Naplóelemzési riasztásokban Webhookok](log-analytics-alerts-webhooks.md).

Webhookműveletek hello tulajdonságait a következő táblázat hello igényelnek.

| Tulajdonság | Leírás |
|:--- |:--- |
| Webhook URL-CÍMÉT |hello hello webhook URL-címe |
| Egyéni JSON-adattartalmat |Egyéni adattartalom toosend a hello webhook.  További információ alább olvasható. |


Webhook URL-címet tartalmazza, és a hasznos adatok között, amelyek hello JSON formátumú, toohello külső szolgáltatás küldött.  Alapértelmezés szerint hello hasznos tartalmazza a következő táblázat hello hello értékeket.  Ez hasznos a saját egyéni egy választható tooreplace.  Ebben az esetben használhatja hello változókat hello táblázatban az egyes hello paraméterek tooinclude azok előnyeit az egyéni tartalmaz.

| Paraméter | Változó | Leírás |
|:--- |:--- |:--- |
| AlertRuleName |#alertrulename |Hello riasztási szabály neve. |
| AlertThresholdOperator |#thresholdoperator |Hello riasztási szabály operátor küszöbértéket.  *Nagyobb, mint* vagy *kisebb, mint*. |
| AlertThresholdValue |#thresholdvalue |Riasztási szabály hello tartozó küszöbérték. |
| LinkToSearchResults |#linktosearchresults |Hivatkozás tooLog Analytics napló keresési visszaadó hello rekordok hello riasztást létrehozó hello lekérdezésből. |
| Attribútumhoz resultcount számlálót. |#searchresultcount |A találatok között hello rekordok száma. |
| SearchIntervalEndtimeUtc |#searchintervalendtimeutc |Befejezési ideje UTC formátumban hello lekérdezés. |
| SearchIntervalInSeconds |#searchinterval |Riasztási szabály hello időszak. |
| SearchIntervalStartTimeUtc |#searchintervalstarttimeutc |Indítsa el a hello lekérdezések ideje UTC formátumban. |
| SearchQuery |#searchquery |Naplófájl-keresési lekérdezés hello riasztási szabály által használt. |
| SearchResults |Lásd az alábbi |JSON formátumban hello lekérdezés által visszaadott rekordok.  Korlátozott toohello első 5000 rögzíti. |
| WorkspaceID |#workspaceid |Az OMS-munkaterület azonosítója. |

Például megadhatja a következő nevű egyetlen paramétert tartalmazó egyéni adattartalom hello *szöveg*.  hello szolgáltatást, amely behívja a webhook a ennek a paraméternek, akkor rendszer.

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }

A példa hasznos oldja meg a következő esetekben hello például toosomething küldött toohello webhook.

    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }

egyéni payloadban tooinclude találatok hello json-adattartalmat a legfelső szintű tulajdonságként a sorban következő hello hozzáadása.  

    "IncludeSearchResults":true

Például egy egyéni adattartalom, amely tartalmazza az ebben az esetben a hello riasztás neve és a keresési eredmények hello toocreate, használhatja a következő hello. 

    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }


A webhook toostart egy riasztási szabály létrehozása egy külső szolgáltatás átfogó példát keresztül végigvezetheti [riasztási webhook művelet létrehozása az OMS szolgáltatáshoz toosend üzenet tooSlack](log-analytics-alerts-webhooks.md).

## <a name="runbook-actions"></a>Runbook-műveletek
Runbook műveletek az Azure Automationben runbook indítása.  A rendezés toouse a művelet típusát, akkor hello [Folyamatautomatizálási megoldása](log-analytics-add-solutions.md) telepítve, és az OMS-munkaterület konfigurálva.  Választhat hello runbookokat hello Folyamatautomatizálási megoldása során konfigurált hello automation-fiókban.

Runbook-műveletek a következő táblázat hello hello tulajdonságok szükséges.

| Tulajdonság | Leírás |
|:--- |:---|
| Forgatókönyv | A Runbook toostart szeretné, ha riasztás jön létre. |
| Futtassa a | Adja meg **Azure** toorun hello runbook hello felhőben.  Adja meg **hibridfeldolgozó** toorun hello runbook az ügynökön [hibrid forgatókönyv-feldolgozó](../automation/automation-hybrid-runbook-worker.md ) telepítve.  |

Runbook műveletek indíthatja hello runbook egy [webhook](../automation/automation-webhooks.md).  Hello riasztási szabályt hoz létre, ha az automatikusan létrehoz egy új webhook hello runbook hello nevű **OMS riasztás szervizelési** GUID követ.  

Megadja az közvetlenül nem hello runbook paramétereket feltöltéséhez, de hello [$WebhookData paraméter](../automation/automation-webhooks.md) tartalmazzák a hello hello riasztás hello eredményeiben hello napló keresése, amely létrehozta.  hello runbook kell toodefine **$WebhookData** az paraméterként tooaccess hello hello riasztás tulajdonságai.  hello riasztási adatokat érhető el egyetlen tulajdonságot, json formátumban **SearchResults** a hello **RequestBody** tulajdonsága **$WebhookData**.  Ez a következő táblázat hello hello tulajdonságokkal fog rendelkezni.

| Csomópont | Leírás |
|:--- |:--- |
| id |Elérési út és hello keresési GUID Azonosítóját. |
| __metadata |Hello riasztási többek között a következőket hello rekordok száma és a keresési eredmények hello állapotával kapcsolatos adatokat. |
| érték |A találatok között hello rekordokban külön bejegyzést.  hello bejegyzés hello részleteit hello tulajdonságok és értékek hello rekord fog egyezni. |

Például hello következő runbook volna kinyerése hello napló keresés által visszaadott hello rögzíti és rendelje hozzá mindegyik rekorddal hello típusától függően különböző tulajdonságokat.  Vegye figyelembe a hello runbook elindul átalakításával **RequestBody** JSON úgy, hogy az erőforrások az PowerShell objektumként.

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
- Megtudhatja, hogyan toowrite [az Azure Automation runbookjai](https://azure.microsoft.com/documentation/services/automation) riasztások által azonosított tooremediate problémák.
