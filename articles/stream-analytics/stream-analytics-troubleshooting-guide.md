---
title: "Azure Stream Analytics aaaTroubleshooting útmutatója |} Microsoft Docs"
description: Hogyan tootroubleshoot a Stream Analytics-feladat
keywords: "az útmutató hibaelhárítása"
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeffstok
ms.openlocfilehash: 4add48054e06a7d8eb617a08595c1be4555c59d2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-for-azure-stream-analytics"></a>Azure Stream Analytics a hibaelhárítási útmutatója

Az Azure Stream Analytics hibaelhárítási jelenhet meg egy összetett elérhető toobe első ránézésre. Után számos felhasználó dolgozik, azt létrehozott Ez az útmutató toohelp érdekében hello folyamat, és távolítsa el a bemenetek, kimenetek, lekérdezések és funkciók munka hello bizonytalanságát.

## <a name="troubleshoot-your-stream-analytics-job"></a>A Stream Analytics-feladat hibaelhárítása

A legjobb eredmény érdekében a Stream Analytics-feladat hibaelhárítása használja a következő irányelveket hello:

1.  A kapcsolat ellenőrzése:
    - Ellenőrizze a kapcsolat tooinputs és -kimenetek hello segítségével **kapcsolat tesztelése** az egyes bemeneti és kimeneti gombra.

2.  Vizsgálja meg a bemeneti adatokat:
    - tooverify, amely a bemeneti adat áramlik az Eseményközpont, használja a [Service Bus Explorer](https://code.msdn.microsoft.com/windowsapps/Service-Bus-Explorer-f2abca5a) tooconnect tooAzure Eseményközpont (ha az Event Hubs bemeneti használja).  
    - Használjon hello [ **mintaadatok** ](stream-analytics-sample-data-input.md) egyes bemeneti gombra, és töltse le a hello bemeneti mintaadatokat.
    - Hello minta adatok toounderstand hello alakzat hello adatok vizsgálata: hello séma- és hello [adattípusok](https://msdn.microsoft.com/library/azure/dn835065.aspx).

3.  A lekérdezés tesztelése:
    - A hello **lekérdezés** lapján hello használata **tesztelése** tootest hello lekérdezés gombra, és használja a letöltött hello mintaadatok túl[hello lekérdezés tesztelése](stream-analytics-test-query.md). Vizsgálja meg az esetleges hibákat, és próbálja meg toocorrect őket.
    - Ha [ **Timestamp By**](https://msdn.microsoft.com/library/azure/mt573293.aspx), ellenőrizze, hogy a hello események hello nagyobb időbélyegeket [feladat kezdési ideje](stream-analytics-out-of-order-and-late-events.md).

4.  A lekérdezés Debug:
    - Építse újra a lekérdezést hello fokozatosan egyszerű select utasítás toomore összetett összesítések lépések segítségével. toobuild hello lekérdezés logika részletes, másolatot használja [WITH](https://msdn.microsoft.com/library/azure/dn835049.aspx) záradékban.
    - Használjon [SELECT INTO](stream-analytics-select-into.md) toodebug lekérdezések lépéseit.

5.  Közös nehézségek, például a kiküszöbölése:
    - A [ **ahol** ](https://msdn.microsoft.com/library/azure/dn835048.aspx) záradék a következő hello lekérdezés kiszűri összes eseményt, és meggátolja, hogy a kimenetet létrehozása folyamatban.
    - Ablak funkciók használata esetén várakozás hello teljes ablak időtartama toosee hello lekérdezésből kimenettel.
    - hello Timestamp típusú események megelőzi hello feladat kezdési időpontja, és ezért események eldobott.

6.  Használja az esemény Rendezés:
    - Nyissa meg az összes hello működött előző lépést, ha toohello **beállítások** panelhez, és válassza [ **esemény rendelési**](stream-analytics-out-of-order-and-late-events.md). Győződjön meg arról, hogy ez a házirend beállítása adott esetben mi teljesen logikus. hello házirend *nem* hello használatakor alkalmazott **teszt** gomb tootest hello lekérdezés. Az eredménye a böngészőben hello feladat futtatásával éles és tesztelése egy különbsége.

7.  Hibakeresési metrikák használatával:
    - Ha nincs kimenet be kell szereznie, miután hello várható időtartama (hello lekérdezés alapján), próbálkozzon a hello következő:
        - Nézze meg [ **figyelési metrikákat** ](stream-analytics-monitoring.md) a hello **figyelő** fülre. Hello értékek összesítése, mert hello metrikák késnek, néhány perc múlva.
            - Ha a bemeneti események > 0, hello feladat a képes tooread bemeneti adatok. Ha a bemeneti események nem > 0, majd:
                - toosee hello adatforrás rendelkezik érvényes adatokat, hogy ellenőrizze, hogy használatával [Service Bus Explorer](https://code.msdn.microsoft.com/windowsapps/Service-Bus-Explorer-f2abca5a). Ez az ellenőrzés érvényes, ha hello feladat Eseményközpont használja bemeneti adatként.
                - Toosee ellenőrizze, hogy hello adatok szerializálási formátum és az adatok kódolást várt módon.
                - Ha hello feladat használ egy Eseményközpontot, ellenőrizze toosee-e hello üzenet törzsét hello *Null*.
            - Ha adatok konvertálási hibák > 0 és kúszó hello következő okai lehetnek:
                - hello feladat nem feltétlenül tudja toode-hello események szerializálni.
                - hello esemény séma nem egyeznek meg a definiált hello vagy a várt séma a hello lekérdezés hello események.
                - hello adattípusok néhány hello esemény hello mezők nem egyeznek meg az elvárásainak.
            - Ha futásidejű hibák > 0, az azt jelenti, hogy hello feladat hello adatokat fogadhat, de hibák előállító hello lekérdezés feldolgozása közben.
                - toofind hello hibák, nyissa meg toohello [naplók](../azure-resource-manager/resource-group-audit.md) és szűrést végezni *sikertelen* állapotát.
            - Ha InputEvents > 0 és OutputEvents = 0, az azt jelenti, hogy hello a következők egyike igaz:
                - A lekérdezés feldolgozása nulla kimeneti eseményt eredményezett.
                - Események mezők lehet vagy helytelen formátumú, ami azt eredményezi, nulla kimeneti lekérdezés feldolgozása után.
                - hello feladat lett nem toopush adatok toohello [kimeneti fogadóját](stream-analytics-select-into.md) kapcsolatot vagy a hitelesítés okokból.
        - Az összes hello azt már korábban említettük hibaeseteknél, műveletek naplóüzenetek magyarázó további részleteket (beleértve a Mi történik), kivéve ahol hello lekérdezés logika program kiszűri az összes esemény. Több esemény feldolgozását hello hibák állít elő, ha naplózza a Stream Analytics naplók hello hello azonos tooOperations 10 percen belül írja be az első három hibaüzenetek. Azután letiltja a további azonos hibát jelzett üzenetet kapja, "Hibák hogy túl gyorsan ezek van letiltva."

8. Hibakeresési naplózási és diagnosztikai naplók segítségével:
    - Használjon [naplók](../azure-resource-manager/resource-group-audit.md), és a szűrő tooidentify és hibakeresési hibákat.
    - Használjon [diagnosztikai naplók feladat](stream-analytics-job-diagnostic-logs.md) tooidentify és hibakeresési hibákat.

9. Vizsgálja meg a hello kimenetek:
    - Hello feladat állapota esetén *futtató*, attól függően, hello időtartama meghatározott hello lekérdezésben hello kimeneti hello fogadó adatforrásban megjelenik.
    - Ha nem látható, hogy tooa egyes kimeneti típus kimenetek, átirányítja őket, amelyek kevésbé összetett, például egy Azure Blob tooan kimeneti típust. A Tártallózó használatával ellenőrizheti toosee, hogy hello kimeneti láthatók. Ezenkívül ellenőrizze, hogy szabályozási szint pedig hello kimeneti vannak megakadályozza az adatok fogadása.

10. Adatfolyam elemzés használata a feladat ábra metrikák:
    - tooanalyze data flow és kapcsolatos problémákat jeleznek, használjon hello [metrikákat a feladat ábra](stream-analytics-job-diagram-with-metrics.md).

11. Nyissa meg a támogatási esetet:
    - Végül, ha minden más meghiúsul, nyissa meg a Microsoft támogatási esetet hello; előfizetés-azonosító használatával, amely tartalmazza a feladatot.

## <a name="get-help"></a>Segítségkérés

Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Következő lépések

* [A Stream Analytics bemutatása tooAzure](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)
