---
title: "aaaScale Stream Analytics-feladatok tooincrease átviteli |} Microsoft Docs"
description: "Ismerje meg, hogyan tooscale Stream Analytics-feladatok bemeneti partíciók, hangolása hello lekérdezés definícióját, és a beállítás által végrehajtott feladat folyamatos átviteli egységeket."
keywords: "adatfolyam, az adatfeldolgozás streaming hangolására analytics"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 7e857ddb-71dd-4537-b7ab-4524335d7b35
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/22/2017
ms.author: jeffstok
ms.openlocfilehash: 4ba8f6b2f8bfebd52cfa07696b501b42cda21f75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scale-azure-stream-analytics-jobs-tooincrease-stream-data-processing-throughput"></a>Scale Azure Stream Analytics feladatok tooincrease adatfolyam adatfeldolgozási átviteli sebesség
Ez a cikk bemutatja, hogyan tootune a Stream Analytics lekérdezési tooincrease átviteli Streaming Analytics-feladatok. Megtudhatja, hogyan tooscale Stream Analytics feladatok bemeneti partíciók beállításával, hangolási hello analytics lekérdezésdefiníció és kiszámítása és beállítás feladat *adatfolyam-egységek* (SUS-t). 

## <a name="what-are-hello-parts-of-a-stream-analytics-job"></a>Mik azok a hello részei a Stream Analytics-feladatot?
A Stream Analytics-feladat definíciójának bemenet, a lekérdezés és a kimeneti tartalmazza. Bemenetek, ahol hello feladat beolvassa a hello adatfolyamban. hello lekérdezés használt tootransform hello bemeneti adatfolyamban, és hello kimeneti ahol hello feladat küldi hello feladat eredményét.  

Egy feladat az adatok adatfolyamként történő legalább egy bemeneti forrást igényel. hello adatforrás adatfolyam bemeneti tárolhatók az Azure event hubs vagy az Azure blob Storage tárolóban. További információkért lásd: [Stream Analytics bemutatása tooAzure](stream-analytics-introduction.md) és [Azure Stream Analytics használatának első](stream-analytics-real-time-fraud-detection.md).

## <a name="partitions-in-event-hubs-and-azure-storage"></a>Az event hubs és az Azure storage a partíciók
A Stream Analytics-feladat skálázás kihasználja a partíciók a bemeneti vagy kimeneti hello. A particionáló lehetővé teszi, hogy részekre adatok partíciós kulcs alapján. A folyamat, amely akkor hello adatokat (például egy Streaming Analytics-feladat) is használnak, és különböző partíciók írási párhuzamosan, ami növeli az adatátviteli sebességet. Streaming Analytics használata, kihasználhatja a particionálás az event hubs és a Blob Storage tárolóban. 

Partíciók kapcsolatos további információkért tekintse meg a következő cikkek hello:

* [Event Hubs szolgáltatások – áttekintés](../event-hubs/event-hubs-features.md#partitions)
* [Az adatok particionálása](https://docs.microsoft.com/azure/architecture/best-practices/data-partitioning#partitioning-azure-blob-storage)


## <a name="streaming-units-sus"></a>Adatfolyam-egységek (SUS-t)
Adatfolyam-egységek (SUs) jelentik hello erőforrások power, amelyek szükségesek a számítási rendelés és tooexecute egy Azure Stream Analytics-feladat. SUS-t adjon meg egy módon toodescribe hello relatív Eseményfeldolgozási Processzor, memória, kevert mértéke alapján kapacitás és és olvasási sebességet. Minden egyes SU megfelel tooroughly 1 MB/s átviteli. 

Hány SUs kiválasztása egy adott feladat függ hello partíció konfigurációját hello bemenetek hello feladat definiált hello lekérdezés szükség. Tooyour kvóta SUS-t a feladat másolatot választhat. Alapértelmezés szerint minden Azure-előfizetéssel rendelkezik tartozó kvóta másolatot az összes hello analytics-feladat too50 SUS-t egy adott régióban. SUs tooincrease meghaladja a kvótát, lépjen kapcsolatba az előfizetésekhez [Microsoft Support](http://support.microsoft.com). Érvényes SUs Feladatonkénti értékei 1, 3, 6, és a 6 lépésekben.

## <a name="embarrassingly-parallel-jobs"></a>Embarrassingly párhuzamos feladat
Egy *embarrassingly párhuzamos* feladat a hello legjobban méretezhető lehetőséget az Azure Stream Analytics van. Egy partíció hello bemeneti tooone példány hello lekérdezés tooone partíció hello kimeneti csatlakozik. A párhuzamos végrehajtás rendelkezik hello követelményeknek:

1. Ha ugyanaz a kulcs feldolgozott hello függ a lekérdezés logika szerint hello azonos példány lekérdezéséhez meg kell győződnie arról, hogy hello események toohello nyissa meg a bemeneti partícióra. Az event hubs számára ez azt jelenti, hogy hello eseményadatok kell hello **PartitionKey** érték beállítása. Másik lehetőségként particionált feladók is használhatja. A blob-tároló, ez azt jelenti, hogy hello események küldött toohello partíció mappában. Ha a lekérdezés logika nem követeli meg ugyanaz a kulcs feldolgozott toobe hello által hello azonos példány lekérdezéséhez, figyelmen kívül hagyhatja ezt a követelményt. A logikai erre példa lehet egy egyszerű válassza-projekt-szűrő lekérdezés.  

2. Miután hello adatok hello bemeneti oldalon elrendezését, meg kell győződnie arról, hogy a lekérdezés particionálva van. Ehhez toouse **Partition By** összes hello lépésben. Több lépést használhat, de mindegyikük hello kell particionálható ugyanazzal a kulccsal. Jelenleg particionálási kulcs hello be kell állítani túl**PartitionId** ahhoz, hogy teljesen párhuzamos hello feladat toobe.  

3. Jelenleg csak az event hubs és a blob storage támogatja a particionált kimeneti. Az event hub kimeneti, konfigurálnia kell a hello partíciós kulcs toobe **PartitionId**. A blob storage kimenet toodo semmi sincs.  

4. hello bemeneti partíciók száma hello kimeneti partíciók számának egyenlőnek kell lennie. BLOB storage-kimenet jelenleg nem támogatja a partíciókat. Azonban ez nem probléma, mert a particionálási sémát hello fölérendelt lekérdezés hello örökli. Az alábbiakban néhány lehetséges, amelyek lehetővé teszik a teljes párhuzamos feladat partíció érték:  

   * 8 event hub bemeneti partíciók és 8 eseményközpont kimeneti partíciók
   * 8 event hub bemeneti partíciókat és a blob storage kimeneti  
   * bemeneti partíciók 8 blob storage és a blob storage kimeneti  
   * 8 blob-tároló bemeneti partíciók és 8 event hub kimeneti partíciók  

hello alábbi szakaszok ismertetik, amelyek embarrassingly párhuzamos néhány példát.

### <a name="simple-query"></a>Egyszerű lekérdezés

* Bemenet: Eseményközpont 8 partíciókkal rendelkező
* Kimeneti: Eseményközpont 8 partíciókkal rendelkező

Lekérdezés:

    SELECT TollBoothId
    FROM Input1 Partition By PartitionId
    WHERE TollBoothId > 100

Ez a lekérdezés egy egyszerű szűrési. Ezért toohello eseményközpont elküldött hello bemeneti partícionálásra vonatkozó tooworry nem szükséges. Figyelje meg, hogy hello lekérdezés tartalmazza **partíció által PartitionId**, így a korábbi #2 követelmény teljesít. Hello kimeneti, el kell tooconfigure hello event hub kimenetet a hello feladat toohave hello particionáló kulcskészlet túl**PartitionId**. Egy legutóbbi ellenőrzés arra, hogy a bemeneti partíciók száma hello kimeneti partíciók száma egyenlő toohello toomake.

### <a name="query-with-a-grouping-key"></a>Csoportosítás kulccsal lekérdezése

* Bemenet: Eseményközpont 8 partíciókkal rendelkező
* A kimenetre: A Blob storage

Lekérdezés:

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Ez a lekérdezés csoportosítási kulccsal rendelkezik. Ezért hello azonos lekérdezése példányt, ami azt jelenti, hogy események küldött jogcímet kell elküldeni toohello event hubs egy particionált módon hello által feldolgozott azonos kulcsú igényeinek toobe. De mely kulcsot kell használni? **PartitionId** feladat-logika fogalom. hello kulcs ténylegesen tartjuk **TollBoothId**, így hello **PartitionKey** hello eseményadatok értékének meg kell **TollBoothId**. A Microsoft ehhez a hello lekérdezés **Partition By** túl**PartitionId**. Mivel hello kimeneti blob-tároló, tooworry konfigurálásáról a partíciós kulcs értékre, követelmény #4 nem szükséges.

### <a name="multi-step-query-with-a-grouping-key"></a>Több lekérdezés csoportosítási kulccsal
* Bemenet: Eseményközpont 8 partíciókkal rendelkező
* Kimenet: Event hub példány 8 partíciókkal rendelkező

Lekérdezés:

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Ez a lekérdezés tartalmaz egy csoportosítási kulcsot, így az azonos kulcsú igények toobe hello által feldolgozott hello lekérdezés ugyanezen példányában. Használhatunk hello azonos stratégia hello előző példában látható módon. Ebben az esetben a hello a lekérdezés több lépést tartalmaz. Az egyes lépések rendelkezik **partíció által PartitionId**? Igen, így hello lekérdezés célkitűzéseinek #3 követelményt. Hello kimeneti, el kell tooset hello partíciós kulcs túl**PartitionId**, a fentiekben taglaltak. Azt is láthatja, hogy rendelkezik-e hello azonos számú partíciót hello bemeneti adatként.

## <a name="example-scenarios-that-are-not-embarrassingly-parallel"></a>Példák, amelyek *nem* embarrassingly párhuzamos

Hello előző szakaszban néhány embarrassingly párhuzamos forgatókönyv bemutatta azt. Ebben a szakaszban arról lesz szó, amelyek nem felelnek meg az összes hello követelmények toobe embarrassingly párhuzamos forgatókönyvek. 

### <a name="mismatched-partition-count"></a>Nem egyező partíciók száma
* Bemenet: Eseményközpont 8 partíciókkal rendelkező
* Kimenet: Eseményközpont 32 partíciókkal rendelkező

Ebben az esetben nem számít, milyen hello lekérdezés. Ha hello bemeneti partíciók száma nem egyezik meg a hello kimeneti partíciók száma, hello topológia embarrassingly párhuzamos nem.

### <a name="not-using-event-hubs-or-blob-storage-as-output"></a>Az event hubs vagy a blob-tároló nem használ kimenetként
* Bemenet: Eseményközpont 8 partíciókkal rendelkező
* Kimenet: Power BI

Power bi-kimenet jelenleg nem támogatja a particionálást. Emiatt az ebben a forgatókönyvben nincs embarrassingly párhuzamos.

### <a name="multi-step-query-with-different-partition-by-values"></a>Több lekérdezés eltérő értékű Partition By
* Bemenet: Eseményközpont 8 partíciókkal rendelkező
* Kimeneti: Eseményközpont 8 partíciókkal rendelkező

Lekérdezés:

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By TollBoothId
    GROUP BY TumblingWindow(minute, 3), TollBoothId

Ahogy látja, használja a hello második lépésben **TollBoothId** particionálási kulcs hello szerint. Ezt a lépést nem van hello azonos hello első lépéseként, és ezért szükség van, egy véletlen toodo. 

hello előző példák azt szemléltetik, néhány Stream Analytics-feladatok egy embarrassingly párhuzamos topológia megfelelő túl (és nem). Felelnek meg, ha rendelkeznek hello lehetséges, hogy a maximális skálája. Feladatok, amelyek nem felelnek meg ezeket a profilokat, útmutatást skálázás egyik le lesz elérhető a jövőben frissíti. Most használja a következő részekben hello hello általános útmutatást.

## <a name="calculate-hello-maximum-streaming-units-of-a-job"></a>A feladatok streamelési egységek maximális hello kiszámítása
hello adatfolyam-továbbítási egységek száma, amelyek segítségével a Stream Analytics-feladat hello számos lépés hello lekérdezésben definiált hello feladat, és minden egyes partíciók száma hello függ.

### <a name="steps-in-a-query"></a>A lekérdezésben lépései
A lekérdezés egy vagy több lépéseket rendelkezhet. Az egyes lépések hello által meghatározott segédlekérdezésben **WITH** kulcsszó. hello kívül hello-lekérdezést **WITH** kulcsszó (csak egy lekérdezést) is számít egy lépést, például a hello **VÁLASSZA** utasítás a következő lekérdezés hello:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute,3), TollBoothId

Ez a lekérdezés két lépést tartalmaz.

> [!NOTE]
> Ez a lekérdezés tárgyalt hello cikk későbbi részében részletesebben.
>  

### <a name="partition-a-step"></a>A lépés particionálása
A következő feltételek hello particionálás egy lépés szükséges:

* hello bemeneti forrás kell particionálni. 
* Hello **válasszon** hello lekérdezési utasítás particionált bemeneti forrásból kell olvasni.
* hello lekérdezés hello lépés belül rendelkeznie kell hello **Partition By** kulcsszó.

A lekérdezés particionálva van, amikor hello bemeneti események feldolgozott és az összesített külön partíciócsoportok, és kimenetek az események az egyes hello csoportok hozhatók létre. Ha azt szeretné, hogy egy kombinált összesítést, létre kell hoznia egy második lépésben nem particionált tooaggregate.

### <a name="calculate-hello-max-streaming-units-for-a-job"></a>Adatfolyam-egységek feladat hello maximális kiszámítása
Minden nem particionált lépést együtt költenie toosix adatfolyam-egységek (SUS-t) egy Stream Analytics-feladat. kell particionálni a tooadd SUS-t, a lépést. Mindegyik partíció lehet hat SUS-t.

<table border="1">
<tr><th>Lekérdezés</th><th>Maximális SUs hello feladat</th></td>

<tr><td>
<ul>
<li>hello lekérdezés tartalmaz egy lépésben.</li>
<li>hello lépés nincs particionálva.</li>
</ul>
</td>
<td>6</td></tr>

<tr><td>
<ul>
<li>a bemeneti adatfolyam hello 3 particionálva van.</li>
<li>hello lekérdezés tartalmaz egy lépésben.</li>
<li>hello lépés particionálva van.</li>
</ul>
</td>
<td>18</td></tr>

<tr><td>
<ul>
<li>hello lekérdezés két lépést tartalmaz.</li>
<li>Hello lépések egyike sem particionálva van.</li>
</ul>
</td>
<td>6</td></tr>

<tr><td>
<ul>
<li>a bemeneti adatfolyam hello 3 particionálva van.</li>
<li>hello lekérdezés két lépést tartalmaz. hello bemeneti lépés particionálva van, és hello a második lépés.</li>
<li>Hello <strong>válasszon</strong> particionálva hello bemeneti olvassa be az utasítást.</li>
</ul>
</td>
<td>(a particionált lépéseket 18) + 6. lépéseket nem particionált 24</td></tr>
</table>

### <a name="examples-of-scaling"></a>Példák a méretezés

hello következő lekérdezés számítja ki, amely rendelkezik a három tollbooths téren állomás áthaladás három perces ablak autók hello száma. Ez a lekérdezés akár toosix SUS-t is méretezhető.

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

További toouse SUS-t a hello lekérdezés, mind a bemeneti adatfolyam hello és kell particionálni a hello lekérdezés. Mivel hello adatok adatfolyam partíció too3 van állítva, hello következő módosított lekérdezés akár is méretezhető too18 SUs:

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Amikor lekérdezést particionálva van, a hello bemeneti események feldolgozása, külön partíciócsoportok összesíteni. A kimeneti eseményekben is vonatkozóan hello csoportok jönnek létre. Particionálás okozhat, ha hello néhány váratlan eredményekhez **GROUP BY** mező nincs a bemeneti adatfolyam hello hello partíciós kulcs. Például hello **TollBoothId** hello előző lekérdezés mezője nincs hello partíciós kulcs a **Input1**. hello eredménye, hogy hello őrbódét #1 adatait a több partíciót kell terjed.

Egyes hello **Input1** partíciók dolgoz fel külön Stream Analytics. Ennek eredményeképpen hello car száma több rekordnyi hello hello azonos Átfedésmentes ablak jön létre a azonos őrbódét. Hello bemeneti partíciós kulcs nem módosítható, ha a probléma egy partíción kívüli lépés, mint például a következő hello hozzáadásával lehet meghatározni:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId

Ez a lekérdezés lehet méretezett too24 SUS-t.

> [!NOTE]
> Ha a két adatfolyam csatlakozik, győződjön meg arról, hogy hello adatfolyamok particionáltak hello partíciós kulcs hello oszlop által használt toocreate hello illesztéseket. Győződjön meg arról, hogy rendelkezik-e hello is azonos számú mindkét adatfolyamokat a partíciók száma.
> 
> 

## <a name="configure-stream-analytics-streaming-units"></a>Konfigurálja a Stream Analytics adatfolyam-egységek

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Az erőforrások listájához hello keresse meg hello Stream Analytics-feladat tooscale szeretne, és nyissa meg.
3. A hello feladat panelen a **konfigurálása**, kattintson a **méretezési**.

    ![Az Azure Stream Analytics feladat beállítása][img.stream.analytics.preview.portal.settings.scale]

4. Hello feladat hello csúszkát tooset hello SUS-t használja. Figyelje meg, hogy-e korlátozott toospecific SU beállításait.


## <a name="monitor-job-performance"></a>Feladat teljesítmény figyelése
Hello Azure-portál használatával, nyomon követheti a feladatok hello átviteli:

![Az Azure Stream Analytics feladatok figyelése][img.stream.analytics.monitor.job]

Várt hello munkaterhelések átviteli sebessége hello kiszámításához. Ha hello átviteli kisebb a vártnál, hello bemeneti partíció hangolására, hello lekérdezés hangolása és SUs tooyour feladat hozzáadása.


## <a name="visualize-stream-analytics-throughput-at-scale-hello-raspberry-pi-scenario"></a>A Stream Analytics átviteli léptékű megjelenítése: hello málna Pi forgatókönyv
megismerte a Stream Analytics-feladatok méretezése hogyan toohelp, azt végre kísérlet málna Pi eszköz visszajelzései alapján. Ehhez a kísérlethez tudassa velünk látni több adatfolyam-továbbítási egységek és partíciók átviteli hello hatását.

Ebben a forgatókönyvben a hello eszköz érzékelő adatokat (ügyfelek) tooan eseményközpont küld. Streaming Analytics hello adatokat dolgozza fel, és riasztást vagy statisztika küld kimeneti tooanother eseményközpontban. 

hello ügyfél érzékelő adatokat küld a JSON formátumban. hello adatokat tartalmazó kimenetével is JSON formátumban van. hello adatok így néz ki:

    {"devicetime":"2014-12-11T02:24:56.8850110Z","hmdt":42.7,"temp":72.6,"prss":98187.75,"lght":0.38,"dspl":"R-PI Olivier's Office"}

a következő lekérdezés hello használt toosend riasztás esetén egy világos ki kell kapcsolni:

    SELECT AVG(lght),
     "LightOff" as AlertText
    FROM input TIMESTAMP
    BY devicetime
     WHERE
        lght< 0.05 GROUP BY TumblingWindow(second, 1)

### <a name="measure-throughput"></a>Mérték átviteli sebesség

Ebben a környezetben átviteli érték hello bemeneti adatfeldolgozási Stream Analytics egy rögzített időn belül. (Jelenleg mért 10 perc.) tooachieve hello legjobb feldolgozási adatátviteli sebességét hello bemeneti adatai, mindkét hello adatfolyam bemeneti és hello lekérdezés volt particionálva. Azt a részét **COUNT()** hello lekérdezés toomeasure a bemeneti események feldolgozása megtörtént. a bemeneti események toocome nem egyszerűen várakozási toomake meg arról, hogy hello feladat, mindegyik partíciójához hello bemeneti eseményközpont által előre betöltött bemeneti adatokat körülbelül 300 MB.

hello következő táblázat azt növelni a streamelési egységek számának hello és hello megfelelő partíció található, az event hubs látott hello eredmények.  

<table border="1">
<tr><th>Bemeneti partíciók</th><th>Kimeneti partíciók</th><th>Folyamatos átviteli egységek</th><th>Fenntartható
</th></td>

<tr><td>12</td>
<td>12</td>
<td>6</td>
<td>4.06 MB/s</td>
</tr>

<tr><td>12</td>
<td>12</td>
<td>12</td>
<td>8.06 MB/s</td>
</tr>

<tr><td>48</td>
<td>48</td>
<td>48</td>
<td>38.32 MB/s</td>
</tr>

<tr><td>192</td>
<td>192</td>
<td>192</td>
<td>172.67 MB/s</td>
</tr>

<tr><td>480</td>
<td>480</td>
<td>480</td>
<td>454.27 MB/s</td>
</tr>

<tr><td>720</td>
<td>720</td>
<td>720</td>
<td>609.69 MB/s</td>
</tr>
</table>

És hello következő grafikon azt ábrázolja, SUS-t és átviteli hello kapcsolatát a képi megjelenítés.

![img.Stream.Analytics.perfgraph][img.stream.analytics.perfgraph]

## <a name="get-help"></a>Segítségkérés
Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Következő lépések
* [A Stream Analytics bemutatása tooAzure](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor-NewPortal.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings-NewPortal.png   

<!--Link references-->

[microsoft.support]: http://support.microsoft.com
[azure.management.portal]: http://manage.windowsazure.com
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301

