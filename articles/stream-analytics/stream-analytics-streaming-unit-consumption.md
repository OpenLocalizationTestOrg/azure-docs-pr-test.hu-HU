---
title: "Az Azure Stream Analytics: A feladat toouse adatfolyam-egységek hatékonyan optimalizálása |} Microsoft Docs"
description: "Gyakorlati tanácsok a méretezés és teljesítmény az Azure Stream Analytics lekérdezési."
keywords: "streamelési egység, teljesítmény-küszöbérték"
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
ms.openlocfilehash: 5ad98b34d625190a879260f54c9eff0294e230cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-your-job-toouse-streaming-units-efficiently"></a>A feladat toouse adatfolyam-egységek hatékonyan optimalizálása

Az Azure Stream Analytics hello teljesítmény "weight" a folyamatos átviteli egységek (SUS-t) egy feladat futó összesíti. SUs hello számítási erőforrások, amelyek felhasznált tooexecute egy feladat jelölik. SUS-t adjon meg egy módon toodescribe hello relatív Eseményfeldolgozási Processzor, memória, kevert mértéke alapján kapacitás és és olvasási sebességet. Ez a kapacitás lehetővé hello lekérdezés programot, és eltávolítja, nem szükséges tooknow tárolási réteg teljesítménnyel kapcsolatos megfontolások memóriát lefoglalni a feladat manuálisan, illetve a feladat hello CPU szükséges core-száma toorun hozzávetőleges időben koncentrálhat.

## <a name="how-many-sus-are-required-for-a-job"></a>Hány SUs szükségesek egy feladatot?

Kiválasztása szükséges SUS-t egy adott projekt hello száma attól függ, hogy hello partíció konfigurációját hello a be- és hello lekérdezés, amely a hello feladat van definiálva. Hello **méretezési** panel lehetővé teszi a tooset hello SUS-t jobb száma. Érdemes a bevált gyakorlat az tooallocate több SUs, mint amennyi szükséges. hello Stream Analytics program optimalizálja a késés és a hello költségekkel további memória lefoglalása közben az átviteli sebesség.

Ajánlott eljárás hello általában toostart 6 SUS-t a lekérdezések, amelyek nem használják a *PARTITION BY*. Majd megállapítja, hogy hello étkezési hellyel, amelyben módosítása SUs hello száma után fázis reprezentatív adatmennyiséget, és vizsgálja meg a hello SU % kihasználtsági metrika próbálkozást metódus használatával.

Az Azure Stream Analytics események tartja hello "újrarendelési puffer" néven, bármely feldolgozásának megkezdése előtt ablakban. Események alapján vannak rendezve hello újrarendelési időszakban idő, és a hello ideiglenesen rendezett események utólagos műveleteket. Események idő szerinti újrarendezése biztosítja, hogy hello kezelőnek megjelenítési lehetőségeinek összes hello hello események időkeretre kötni. Azt is lehetővé teszi, hogy hajtsa végre a szükséges hello feldolgozási, majd előállít egy kimeneti hello operátor. A mechanizmus egyik mellékhatása, hogy feldolgozás késleltetett által hello újrarendelési időszak hello időtartama. hello memóriaigény hello feladat (amely hatással van a SU fogyasztás) hello méretét, a újrarendelési ablakot, és hello lévő események feladata.

> [!NOTE]
> Hello olvasók számát megváltozásakor feladat frissítéskor átmeneti kapcsolatos figyelmeztetések írása tooaudit naplókat. Stream Analytics-feladatok automatikusan helyreállni ezek átmeneti probléma.

## <a name="common-high-memory-causes-for-high-su-usage-for-running-jobs"></a>Magas SU használatát a futó feladatok nagy memória okairól

### <a name="high-cardinality-for-group-by"></a>A GROUP BY nagy számosságot

bejövő események hello számossága határozzák meg, hogy a memóriahasználat hello feladat.

Például a `SELECT count(*) from input group by clustered, tumblingwindow (minutes, 5)`, társított szám hello **fürtözött** hello lekérdezés hello számossága van.

nagy számosságot által okozott problémák toomitigate használó partíciókkal növelésével kiterjesztése hello lekérdezés **PARTITION BY**.

```
Select count(*) from input
Partition By clusterid
GROUP BY clustered tumblingwindow (minutes, 5)
```

hello száma *fürtözött* hello számossága alapján a GROUP BY itt van.

Hello lekérdezés particionálása, után azt között oszlik több csomópont. Ennek eredményeképpen minden csomópontjába érkező események hello száma csökken, amely hello hello újrarendelési puffer mérete pedig csökkenti. PartitionID is kell particionálni az event hub partíciókat.

### <a name="high-unmatched-event-count-for-join"></a>Az ILLESZTÉSI magas páratlan események száma

hello ILLESZTÉS nem egyező események száma hello memóriahasználata hello lekérdezés hatással van. Vegyük például, egy lekérdezést, amely toofind hello ad megjelenítések száma kattintással generáló keresi:

```
SELECT id from clicks INNER JOIN,
impressions on impressions.id = clicks.id AND DATEDIFF(hour, impressions, clicks) between 0 AND 10
```

Az ebben az esetben lehetséges, hogy sok ads is látható, és akkor jönnek létre, néhány kattintással is. Ilyen eredményt igényelnének hello feladat tookeep hello időszak alatt az összes hello esemény. hello felhasznált memória mérete arányos toohello ablak méretének és esemény sebessége. 

toomitigate ebben az esetben hello lekérdezés növelésével kibővítési particionálja a PARTITION BY használatával. 

Hello lekérdezés particionálása, után azt között oszlik több feldolgozási csomópont. Ennek eredményeképpen minden csomópontjába érkező események hello száma csökken, amely hello hello újrarendelési puffer mérete pedig csökkenti.

### <a name="large-number-of-out-of-order-events"></a>Üzemen kívüli események nagy száma 

Üzemen kívüli események belül egy olyan nagy időkeretet nagy számú hello "átrendezése puffer" toobe nagyobb hello méretének okoz. toomitigate ebben az esetben méretezési hello lekérdezés növelésével particionálja a PARTITION BY használatával. Hello lekérdezés particionálása, után azt között oszlik több csomópont. Ennek eredményeképpen minden csomópontjába érkező események hello száma csökken, amely hello hello újrarendelési puffer mérete pedig csökkenti. 


## <a name="get-help"></a>Segítségkérés
Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Következő lépések
* [A Stream Analytics bemutatása tooAzure](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Az Azure Stream Analytics lekérdezési nyelvi referencia](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Az Azure Stream Analytics felügyeleti REST API-referencia](https://msdn.microsoft.com/library/azure/dn835031.aspx)
