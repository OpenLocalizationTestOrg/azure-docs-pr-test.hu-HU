---
title: "aaaHandling esemény sorrendjét és az Azure Stream Analytics késedelmesség |} Microsoft Docs"
description: "Tudnivalók a Stream Analytics-soron vagy késői események adatfolyamban működéséről."
keywords: "nem megfelelő sorrendben, késői, események"
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
ms.openlocfilehash: 87c028662fbafbf4f72f57f215d017f65bb649f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-stream-analytics-event-order-handling"></a>Az Azure Stream Analytics rendelés eseménykezelésnek

A historikus adatfolyam események minden esemény rögzítése a hello idővel hello esemény érkezik. Egyes feltételek okozhat toooccasionally kap eseményfolyamokat egyes események egy másik ahhoz, mint ami lettek küldve. Egy egyszerű TCP ismét, vagy akár közötti eszköz- és fogadását az event hubs hello küld hello időbeállításainak okozhat a toooccur. "Írásjelek" események is kerülnek tooreceived eseményfolyamokat, tooadvance hello idő esemény érkezők hello hiányában. Ezek szükségesek a helyzetekben, például "Értesítés kérek, ha nincs bejelentkezések 3 perc következik be."

Bemeneti adatfolyamot, amelyek nincsenek sorrendben vagy szerepelnek:
* Rendezett (és ezért **késleltetett**).
* Módosított hello rendszer tooa felhasználó által megadott házirend szerint.


## <a name="lateness-tolerance"></a>Késedelmesség tolerancia
A Stream Analytics eltűr ilyen típusú forgatókönyvek. A Stream Analytics "soron-" és "késői" események kezelésére van. Ezek az események a következő módokon hello kezelési:

* Az események sorrendje nem érkeznek, de belül hello beállítása tolerancia **által időbélyeg átrendezésekor**.
* Legkésőbb tolerancia érkező események **eldobni vagy módosítani**.
    * **Igazítva**: beállított tooappear toohave érkező hello legújabb elfogadható idő.
    * **Eldobott**: vetve.

![Stream Analytics eseménykezelésnek](media/stream-analytics-event-handling/stream-analytics-event-handling.png)

## <a name="reduce-hello-number-of-out-of-order-events"></a>Csökkentse a hello soron események száma

Stream Analytics egy ideiglenes-átalakítás alkalmazása, akkor dolgozza fel a bejövő események (például az ablakos összesítéseket vagy időalapú illesztéseket), mert a Stream Analytics bejövő események időbélyeg sorrend szerint rendezi.

Ha hello "időbélyeg által" kulcsszóval van **nem** használt, hello Azure Event Hubs esemény sorba helyezni idő alapértelmezés szerint használt. Az Event Hubs biztosítja, hogy az egyes partíciók hello az event hubs hello Timestamp monotonicity. Emellett biztosítja azt, hogy az összes partíció események összevonva fogja tartalmazni időbélyeg sorrendben. Ezek két Event Hubs garanciák soron események nem biztosítása.

Egyes esetekben fontos az Ön toouse hello feladó időbélyegzési. Ebben az esetben a hello eseménytartalom időbélyeg van kiválasztva, a "által időbélyegző." Ezekben az esetekben egy vagy több forrás esemény misorder, előfordulhat, hogy vezette be:

* Esemény gyártók óra dönt rendelkezik. Ez a esetén közös gyártók úgy, hogy különböző órák különböző számítógépeken vannak.
* Nincs hello események toohello cél az event hubs hello forrásból származó hálózati késést.
* Óra dönt event hub partíciók között található. A Stream Analytics először rendezése az összes event hub partícióról származó események esemény sorba helyezni időpontja. Ezt követően megvizsgálja a misordered események hello adatfolyamban.

Hello konfigurációs lapján a következő alapértelmezett hello jelenik meg:

![Stream Analytics soron kezelése](media/stream-analytics-event-handling/stream-analytics-out-of-order-handling.png)

0 másodperc hello soron tűrési használja, ha vannak biztos, hogy az összes esemény sorrendben legyenek minden hello idő. Hello három eseményforrást misordered megadott, nem valószínű, hogy ez érvényét veszti. 

tooallow Stream Analytics toocorrect egy esemény misorder, egy nem nulla soron tűrési is megadhat. Stream Analytics pufferek események toothat ablakot, és úgy döntött, hogy timestamp hello használatával újrarendelések. Ezt követően végrehajtja hello historikus átalakítása. Egy 3-második ablak kezdődnie, és hangolja hello érték tooreduce hello események száma, amelyek idő igazodik. 

Egyik mellékhatása hello pufferelés az, hogy hello kimeneti **késleltetett által hello azonos idő**. Hangolja hello érték tooreduce hello soron események száma, és tartsa hello feladat késés alacsony.

## <a name="get-help"></a>Segítségkérés
Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Következő lépések
* [Bevezetés tooStream elemzés](stream-analytics-introduction.md)
* [A Stream Analytics használatába](stream-analytics-real-time-fraud-detection.md)
* [Stream Analytics-feladatok méretezése](stream-analytics-scale-jobs.md)
* [Stream Analytics lekérdezési nyelvi referencia](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [A Stream Analytics felügyeleti REST API-referencia](https://msdn.microsoft.com/library/azure/dn835031.aspx)