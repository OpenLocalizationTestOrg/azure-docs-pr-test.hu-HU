---
title: "aaaDiagnose és a problémák megoldása |} Microsoft Docs"
description: "Ez az oktatóanyag ismerteti hogyan toodiagnose és idő adatsorozat Insights környezetében előforduló problémák megoldásához"
keywords: 
services: time-series-insights
documentationcenter: 
author: venkatgct
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/24/2017
ms.author: venkatja
ms.openlocfilehash: 00893d4bec497f5f8bf7093be5b96f1844446d13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="diagnose-and-solve-problems-in-your-time-series-insights-environment"></a>Hibáinak diagnosztizálásához és elhárításához idő adatsorozat Insights környezetében

## <a name="i-dont-see-my-data"></a>Az adatok nem látható
Az alábbiakban néhány ok, amiért miért nem láthatja az adatok hello környezetében [Azure idő adatsorozat Insights portálon találja meg](https://insights.timeseries.azure.com).

### <a name="your-event-source-doesnt-have-data-in-json-format"></a>A forrás nincs adatok JSON formátumban
Az Azure idő adatsorozat Insights támogat csak a JSON-adatokat. JSON-mintákat, lásd: [támogatott JSON alakzatok](time-series-insights-send-events.md#supported-json-shapes).

### <a name="when-you-registered-your-event-source-you-didnt-provide-hello-key-that-has-hello-required-permission"></a>A forrás regisztrálásakor nem adott meg, amely jogosult a szükséges hello hello kulcs
* Az IoT-központ van szüksége, amely rendelkezik tooprovide hello kulcs **service csatlakozás** engedéllyel.

   ![Az IoT-központ szolgáltatás a connect engedély](media/diagnose-and-solve-problems/iothub-serviceconnect-permissions.png)

   Ahogy az képen megelőző hello vagy hello házirendek **iothubowner** és **szolgáltatás** csatlakoztatás működik, mert mindkét **service csatlakozás** engedéllyel.
* Az eseményközpontok tooprovide hello kulcsot, amelynek kell **figyelésére** engedéllyel.

   ![Event hub figyelési engedély](media/diagnose-and-solve-problems/eventhub-listen-permissions.png)

   Ahogy az képen megelőző hello hello házirendek egyikét **olvasási** és **kezelése** csatlakoztatás működik, mert mindkét **figyelésére** engedéllyel.

### <a name="hello-provided-consumer-group-is-not-exclusive-tootime-series-insights"></a>hello megadva a felhasználói csoportban nem kizárólagos tooTime adatsorozat Insights
Az IoT-központ vagy az eseményközpont, a regisztráció során kérjük, használja a rendszer az adatok olvasása toospecify hello fogyasztói csoportot. Nem kell osztani a fogyasztói csoportot. Ha megosztott, hello alapul szolgáló eseményközpont automatikusan bontja a kapcsolatot hello olvasók egyik véletlenszerűen.

## <a name="i-see-my-data-but-theres-a-lag"></a>Az adatok látható, de a késés van
Az alábbiakban miért láthatja részleges adat hello környezetében okok [idő adatsorozat Insights portálon találja meg](https://insights.timeseries.azure.com).

### <a name="your-environment-is-getting-throttled"></a>A környezet első szabályozva van
a szabályozási korlát hello kényszerítése hello környezet SKU típusú és kapacitás alapján. Minden eseményforrások hello környezetben oszthassák meg a kapacitást. Az IoT-központ vagy az eseményközpont hello eseményforrás kényszerített hello korlátok adatok küldését, ha sávszélesség-szabályozás és a késés láthatja.

a következő diagram hello egy idő adatsorozat Insights környezet látható, amely rendelkezik egy S1 SKU és 3 kapacitása. Azt is érkező 3 millió esemény naponkénti.

![Környezet SKU aktuális kapacitás](media/diagnose-and-solve-problems/environment-sku-current-capacity.png)

Tegyük fel, hogy ebben a környezetben van választásával dolgozhat fel az eseményközpontban lévő üzenetek hello érkező sebességet hello a következő ábrán látható:

![Az eseményközpontok érkező arány – példa](media/diagnose-and-solve-problems/eventhub-ingress-rate.png)

Ahogy hello ábrán is látható, hello napi érkező ez ~ 67,000 üzeneteket. Ez az eszköz nagyjából too46 üzenetek percenként. Ha minden event hub üzenet egybesimított tooa egyetlen alkalommal adatsorozat Insights esemény, az ebben a környezetben látja, nincs. Ha minden event hub üzenet egybesimított too100 idő adatsorozat Insights eseményeket, majd 4,600 események kell fogyasztanak percenként. 3 / kapacitású S1 SKU környezet is csak 2,100 érkező jelzések percenként (1 millió esemény naponkénti 700 eseményt küld percenként 3 egység = = 2,100 eseményt küld percenként). Ezért tekintse meg a késés miatt toothrottling. 

Magas szintű megértéséhez logikai egybesimítását működését, tekintse meg a [támogatott JSON alakzatok](time-series-insights-send-events.md#supported-json-shapes).

#### <a name="recommended-steps"></a>Javasolt lépések
toofix hello lag, növelje hello Termékváltozat-kapacitás környezet. További információkért lásd: [hogyan tooscale idő adatsorozat Insights környezetét](time-series-insights-how-to-scale-your-environment.md).

### <a name="youre-pushing-historical-data-and-causing-slow-ingress"></a>Ön előzményadatokat kérdez le, és lassú érkező okozó
Ha egy meglévő eseményforrás kapcsolódik, valószínű, hogy az IoT-központ vagy az eseményközpont már tartalmaznak adatokat azt. Így hello környezetben kezdődik, húzza hello esemény forrását üzenet megőrzési időszak kezdete hello adatait. 

Ez a viselkedés hello alapértelmezett viselkedését, és nem bírálható felül. Is végezhetnek a sávszélesség-szabályozás és eltarthat egy darabig a korábbi adatok bevitele mentése toocatch.

#### <a name="recommended-steps"></a>Javasolt lépések
toofix hello lag, a következő lépéseket hajtsa végre a megfelelő hello:
1. Növelje a hello SKU kapacitás toohello megengedett maximális értéket (ebben az esetben 10). Hello kapacitás nő, miután hello érkező folyamat elindul, mentése sokkal gyorsabban alatt. Milyen gyorsan meg most elfogja keresztül a rendelkezésre állási diagram hello hello jelenítheti [idő adatsorozat Insights portálon találja meg](https://insights.timeseries.azure.com). Hello nagyobb kapacitást van szó.
2. Miután hello lag naprakész, csökkenti hello SKU kapacitás hátsó tooyour normál érkező.

## <a name="my-event-sources-timestamp-property-name-setting-doesnt-work"></a>A forrás *időbélyeg-tulajdonság neve* beállítás nem működik
Győződjön meg arról, hogy hello név-érték felel meg a következő szabályok toohello:
* hello időbélyeg tulajdonságnév _kis-és nagybetűket_.
* hello Timestamp típusú tulajdonság értéke adatforrásból származó az eseményforrás JSON karakterláncként kell hello formátum _éééé-hh-nnTóó: pp:. FFFFFFFK_. Példa ilyen karakterlánc: "2008-04-12T12:53Z".
