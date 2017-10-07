---
title: "az event hubs érzékelőinek Azure Stream Analytics aaaDebug |} Microsoft Docs"
description: "A lekérdezés ajánlott eljárások az Event Hubs fogyasztói csoportok a Stream Analytics-feladatok figyelembe véve."
keywords: "Event hub határt, a fogyasztói csoportot"
services: stream-analytics
documentationcenter: 
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
ms.openlocfilehash: 89821e6273151de43b5e42d907e547c939e24824
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="debug-azure-stream-analytics-with-event-hub-receivers"></a>Azure Stream Analytics a hibakereséshez event hubs érzékelőinek száma

Használhatja az Azure Event Hubs az Azure Stream Analytics tooingest vagy a kimenetet a feladatból. Az Event Hubs használatára vonatkozó ajánlott toouse több fogyasztói csoportok, tooensure feladat méretezhetőséget. Egyik oka, hogy hello Stream Analytics-feladat egy adott bevitel az olvasók számát hello hatással lesz hello olvasók egyetlen felhasználói csoportokban. hello pontos száma fogadók hello kibővített topológia logika belső részleteket alapul. hello száma fogadók kívülről nem lesz közzétéve. hello olvasók számát hello feladat kezdési időpontban vagy feladat frissítéskor módosíthatja.

> [!NOTE]
> Hello olvasók számát egy feladat a frissítés során megváltozásakor átmeneti kapcsolatos figyelmeztetések írása tooaudit naplókat. Stream Analytics-feladatok automatikusan helyreállni ezek átmeneti probléma.

## <a name="number-of-readers-per-partition-exceeds-event-hubs-limit-of-five"></a>Partíciónként Olvasók száma meghaladja az Event Hubs legfeljebb öt

Mely hello az olvasók partíciónként száma meghaladja a hello Event Hubs legfeljebb öt forgatókönyvek hello alábbiakat foglalja magába:

* Több KIVÁLASZTÓ utasítást: Ha több KIVÁLASZTÓ utasítást túl hivatkozó**azonos** eseményközpont bemeneti, minden KIVÁLASZTÓ utasításhoz hatására a létrehozott új fogadó toobe.
* UNION: UNION használatához esetén lehetséges toohave toohello hivatkozó több bemeneti **azonos** event hub és fogyasztói csoportot.
* ÖNILLESZTÉS: Automatikus csatlakozás művelet használatához esetén lehetséges toorefer toohello **azonos** eseményközpont több alkalommal.

## <a name="solution"></a>Megoldás

hello következő bevált gyakorlatát segíthet csökkenteni a mely hello partíciónként Olvasók száma meghaladja a hello Event Hubs legfeljebb öt forgatókönyvek.

### <a name="split-your-query-into-multiple-steps-by-using-a-with-clause"></a>A lekérdezés felosztása több lépést a WITH záradék használatával

hello WITH záradékban ideiglenes elnevezett eredménykészletet, amely a FROM záradék a lekérdezés hello hivatkozhat. Hello WITH záradékkal egy SELECT utasítás hello végrehajtási hatóköre határozza meg.

Például helyett ezt a lekérdezést:

```
SELECT foo 
INTO output1
FROM inputEventHub

SELECT bar
INTO output2
FROM inputEventHub 
…
```

Használja ezt a lekérdezést:

```
WITH input (
   SELECT * FROM inputEventHub
) as data

SELECT foo
INTO output1
FROM data

SELECT bar
INTO output2
FROM data
…
```

### <a name="ensure-that-inputs-bind-toodifferent-consumer-groups"></a>Győződjön meg arról, hogy a bemeneti adatok kötési toodifferent fogyasztói csoportok

A lekérdezések, amelyekben három vagy több bemeneti adatok nem csatlakoztatott toohello ugyanazt az Event Hubs fogyasztói csoportjában külön felhasználói csoportok létrehozása. Ehhez szükséges további Stream Analytics bemenetek hello létrehozását.


## <a name="get-help"></a>Segítségkérés
Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Következő lépések
* [Bevezetés tooStream elemzés](stream-analytics-introduction.md)
* [A Stream Analytics használatába](stream-analytics-real-time-fraud-detection.md)
* [Stream Analytics-feladatok méretezése](stream-analytics-scale-jobs.md)
* [Stream Analytics lekérdezési nyelvi referencia](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [A Stream Analytics felügyeleti REST API-referencia](https://msdn.microsoft.com/library/azure/dn835031.aspx)
