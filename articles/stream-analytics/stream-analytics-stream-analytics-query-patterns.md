---
title: "a Stream Analytics gyakori használati minták aaaQuery példák |} Microsoft Docs"
description: "Gyakori Azure Stream Analytics lekérdezési minták"
keywords: "lekérdezés példák"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jenniehubbard
editor: cgronlun
ms.assetid: 6b9a7d00-fbcc-42f6-9cbb-8bbf0bbd3d0e
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/08/2017
ms.author: jenniehubbard
ms.openlocfilehash: c8f7a8ac661eaf0281f4140b02c42141b73040fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="query-examples-for-common-stream-analytics-usage-patterns"></a>Példa a gyakori Stream Analytics használati minták lekérdezése
## <a name="introduction"></a>Bevezetés
Lekérdezések Azure Stream Analytics lekérdezési SQL-szerű nyelven van kifejezve. Ezeket a lekérdezéseket a rendszer részletes ismertetését lásd: hello [Stream Analytics lekérdezési nyelvi referencia](https://msdn.microsoft.com/library/azure/dn834998.aspx) útmutató. Ez a cikk vázol fel megoldások tooseveral gyakori lekérdezési minták, alapján valós forgatókönyv. Folyamatban lévő és továbbra is az új mintákat folyamatosan frissített toobe.

## <a name="query-example-convert-data-types"></a>Példa: adattípusok átalakítása
**Leírás**: hello bemeneti adatfolyam vonatkozó hello típusú tulajdonságok meghatározásához.
Például hello car karakterláncként. a bemeneti adatfolyam hello hamarosan és kell konvertálni túl toobe**INT** tooperform **SUM** azt be.

**Bemeneti**:

| Ellenőrizze | Time | Súlyozás |
| --- | --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |"1000" |
| Honda |2015-01-01T00:00:02.0000000Z |"2000" |

**Kimeneti**:

| Ellenőrizze | Súlyozás |
| --- | --- |
| Honda |3000 |

**Megoldás**:

    SELECT
        Make,
        SUM(CAST(Weight AS BIGINT)) AS Weight
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**MAGYARÁZAT**: használja a **TÍPUSKONVERZIÓ** hello utasításában **súly** mezőben toospecify az adattípushoz. A támogatott adattípusokat hello tartalmazó lista [adattípusok (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835065.aspx).

## <a name="query-example-use-likenot-like-toodo-pattern-matching"></a>Példa: hasonló/nem például toodo karakterek használata
**Leírás**: Ellenőrizze, hogy egy mező értéke hello esemény megfelel-e egy bizonyos minta.
Ellenőrizze például, hogy hello eredményt adja vissza, amely az A kezdődhet és végződhet 9 licenc lemezeket.

**Bemeneti**:

| Ellenőrizze | LicensePlate | Time |
| --- | --- | --- |
| Honda |ABC – 123 |2015-01-01T00:00:01.0000000Z |
| Toyota |AAA-999 |2015-01-01T00:00:02.0000000Z |
| Nissan |ABC-369 |2015-01-01T00:00:03.0000000Z |

**Kimeneti**:

| Ellenőrizze | LicensePlate | Time |
| --- | --- | --- |
| Toyota |AAA-999 |2015-01-01T00:00:02.0000000Z |
| Nissan |ABC-369 |2015-01-01T00:00:03.0000000Z |

**Megoldás**:

    SELECT
        *
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LicensePlate LIKE 'A%9'

**MAGYARÁZAT**: használata hello **PÉLDÁUL** utasítás toocheck hello **LicensePlate** mező érték. Emellett kell egy A, indítsa el, majd tetszőleges karakterlánc nulla vagy több rendelkezik, és majd végződhet a 9. 

## <a name="query-example-specify-logic-for-different-casesvalues-case-statements"></a>Példa: megadása logikát a különböző esetekre és-értékek ("CASE" utasítás)
**Leírás**: Adjon meg egy másik számítási mező, egy adott feltétel alapján.
Például adjon meg egy karakterlánc leírást hello megegyezik, hogy hány autók átadni, egy különleges esetben az 1.

**Bemeneti**:

| Ellenőrizze | Time |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Kimeneti**:

| CarsPassed | Time |
| --- | --- | --- |
| 1 Honda |2015-01-01T00:00:10.0000000Z |
| 2 Toyotas |2015-01-01T00:00:10.0000000Z |

**Megoldás**:

    SELECT
        CASE
            WHEN COUNT(*) = 1 THEN CONCAT('1 ', Make)
            ELSE CONCAT(CAST(COUNT(*) AS NVARCHAR(MAX)), ' ', Make, 's')
        END AS CarsPassed,
        System.TimeStamp AS Time
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**MAGYARÁZAT**: hello **eset** záradék lehetővé teszi a különböző számítási tooprovide bizonyos feltételeknek megfelelő (ebben az esetben hello hello összesített ablakban hello autók száma).

## <a name="query-example-send-data-toomultiple-outputs"></a>Példa: adatok toomultiple kimenete küldése
**Leírás**: Send data toomultiple kimeneti célok egyetlen feladat.
Például egy küszöbérték-alapú riasztás adatok elemzése, és minden események tooblob archivált.

**Bemeneti**:

| Ellenőrizze | Time |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Honda |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Output1**:

| Ellenőrizze | Time |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Honda |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Output2**:

| Ellenőrizze | Time | Darabszám |
| --- | --- | --- |
| Toyota |2015-01-01T00:00:10.0000000Z |3 |

**Megoldás**:

    SELECT
        *
    INTO
        ArchiveOutput
    FROM
        Input TIMESTAMP BY Time

    SELECT
        Make,
        System.TimeStamp AS Time,
        COUNT(*) AS [Count]
    INTO
        AlertOutput
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)
    HAVING
        [Count] >= 3

**MAGYARÁZAT**: hello **INTO** záradék közli a Stream Analytics, amely hello toowrite hello adatok toofrom kiírja a jelen nyilatkozat.
hello első lekérdezés, hogy mi tooan kimeneti érkezett hello adatok csatlakoztatott **ArchiveOutput**.
hello második lekérdezés nem néhány egyszerű összesítési és szűrés, és akkor küldi hello eredmények tooa alárendelt riasztási rendszer.

Vegye figyelembe, hogy hello eredményeit hello közös táblakifejezésekben (Táblakifejezés) is felhasználhatja (például **WITH** utasítások) több kimeneti utasításokban. Ez a beállítás rendelkezik hello hozzáadott előnye, hogy a bemeneti forrás kevesebb olvasók toohello megnyitása.
Példa: 

    WITH AllRedCars AS (
        SELECT
            *
        FROM
            Input TIMESTAMP BY Time
        WHERE
            Color = 'red'
    )
    SELECT * INTO HondaOutput FROM AllRedCars WHERE Make = 'Honda'
    SELECT * INTO ToyotaOutput FROM AllRedCars WHERE Make = 'Toyota'

## <a name="query-example-count-unique-values"></a>Példa: egyedi értékek száma
**Leírás**: hello adatfolyamot egy olyan időkeretet belül egyedi mező értékeinek hello számát.
Például hogy hány egyedi lesz továbbítja a 2-második ablakban hello téren kiállítási autók?

**Bemeneti**:

| Ellenőrizze | Time |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Honda |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**A kimenetre:**

| Darabszám | Time |
| --- | --- |
| 2 |2015-01-01T00:00:02.000Z |
| 1 |2015-01-01T00:00:04.000Z |

**Megoldás:**

````
SELECT
     COUNT(DISTINCT Make) AS CountMake,
     System.TIMESTAMP AS TIME
FROM Input TIMESTAMP BY TIME
GROUP BY 
     TumblingWindow(second, 2)
````


**Magyarázat:**
**száma (különböző ellenőrizze)** értéket ad vissza a hello különböző értékek számának hello **győződjön** belül egy olyan időkeretet oszlop.

## <a name="query-example-determine-if-a-value-has-changed"></a>Példa: határozza meg, hogy módosult-e egy értéket
**Leírás**: nézze meg a korábbi érték toodetermine Ha hello aktuális értéke eltér.
Például van hello előző car hello téren közúti hello azonos tehetjük hello aktuális autó?

**Bemeneti**:

| Ellenőrizze | Time |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |

**Kimeneti**:

| Ellenőrizze | Time |
| --- | --- |
| Toyota |2015-01-01T00:00:02.0000000Z |

**Megoldás**:

    SELECT
        Make,
        Time
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(minute, 1)) <> Make

**MAGYARÁZAT**: használata **LAG** toopeek hello a bemeneti adatfolyam vissza egy eseményt, és hello beolvasása **ellenőrizze** érték. Hasonlítsa össze toohello **ellenőrizze** hello aktuális és a kimeneti hello esemény az értéket, ha ezek eltérnek.

## <a name="query-example-find-hello-first-event-in-a-window"></a>Példa: keresés hello első esemény ablakban
**Leírás**: keresés hello első autója minden 10 perces időszakban.

**Bemeneti**:

| LicensePlate | Ellenőrizze | Time |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| YZK 5704 |Ford |2015-07-27T00:02:17.0000000Z |
| RMV 8282 |Honda |2015-07-27T00:05:01.0000000Z |
| YHN 6970 |Toyota |2015-07-27T00:06:00.0000000Z |
| VFE 1616 |Toyota |2015-07-27T00:09:31.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Kimeneti**:

| LicensePlate | Ellenőrizze | Time |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |

**Megoldás**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) = 1

Most már most módosítás hello probléma és a keresés hello első autója egy adott Meggyőződünk minden 10 perces időközt a.

| LicensePlate | Ellenőrizze | Time |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| YZK 5704 |Ford |2015-07-27T00:02:17.0000000Z |
| YHN 6970 |Toyota |2015-07-27T00:06:00.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Megoldás**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) OVER (PARTITION BY Make) = 1

## <a name="query-example-find-hello-last-event-in-a-window"></a>Példa: keresés hello utolsó esemény ablakban
**Leírás**: keresés hello utolsó car minden 10 perces időszakban.

**Bemeneti**:

| LicensePlate | Ellenőrizze | Time |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| YZK 5704 |Ford |2015-07-27T00:02:17.0000000Z |
| RMV 8282 |Honda |2015-07-27T00:05:01.0000000Z |
| YHN 6970 |Toyota |2015-07-27T00:06:00.0000000Z |
| VFE 1616 |Toyota |2015-07-27T00:09:31.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Kimeneti**:

| LicensePlate | Ellenőrizze | Time |
| --- | --- | --- |
| VFE 1616 |Toyota |2015-07-27T00:09:31.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Megoldás**:

    WITH LastInWindow AS
    (
        SELECT 
            MAX(Time) AS LastEventTime
        FROM 
            Input TIMESTAMP BY Time
        GROUP BY 
            TumblingWindow(minute, 10)
    )
    SELECT 
        Input.LicensePlate,
        Input.Make,
        Input.Time
    FROM
        Input TIMESTAMP BY Time 
        INNER JOIN LastInWindow
        ON DATEDIFF(minute, Input, LastInWindow) BETWEEN 0 AND 10
        AND Input.Time = LastInWindow.LastEventTime

**MAGYARÁZAT**: hello lekérdezés két lépésből áll. hello első talál meg egy hello legújabb időbélyeg a windows 10 perc. hello második lépésben illesztések hello hello eredeti adatfolyam toofind hello események, amelyek megfelelnek a hello minden ablakban utolsó időbélyegeket hello első lekérdezés eredményeit. 

## <a name="query-example-detect-hello-absence-of-events"></a>Példa: hello hiányában esemény észlelése
**Leírás**: állítson be egy adatfolyam nincs érték, amely megfelel egy adott feltételnek.
Például 2 egymást követő autók hello megegyezik a megadott hello téren közúti belül hello utolsó 90 másodpercet?

**Bemeneti**:

| Ellenőrizze | LicensePlate | Time |
| --- | --- | --- |
| Honda |ABC – 123 |2015-01-01T00:00:01.0000000Z |
| Honda |AAA-999 |2015-01-01T00:00:02.0000000Z |
| Toyota |DEF-987 |2015-01-01T00:00:03.0000000Z |
| Honda |GHI-345 |2015-01-01T00:00:04.0000000Z |

**Kimeneti**:

| Ellenőrizze | Time | CurrentCarLicensePlate | FirstCarLicensePlate | FirstCarTime |
| --- | --- | --- | --- | --- |
| Honda |2015-01-01T00:00:02.0000000Z |AAA-999 |ABC – 123 |2015-01-01T00:00:01.0000000Z |

**Megoldás**:

    SELECT
        Make,
        Time,
        LicensePlate AS CurrentCarLicensePlate,
        LAG(LicensePlate, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarLicensePlate,
        LAG(Time, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarTime
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(second, 90)) = Make

**MAGYARÁZAT**: használata **LAG** toopeek hello a bemeneti adatfolyam vissza egy eseményt, és hello beolvasása **ellenőrizze** érték. Hasonlítsa össze azokat toohello **ELLENŐRIZZE** hello aktuális esemény, és kimeneti hello esemény, ha azok vannak hello azonos érték. Is **LAG** hello előző car tooget adatait.

## <a name="query-example-detect-hello-duration-between-events"></a>Példa: hello időtartama események között észlelése
**Leírás**: hello időtartam egy adott esemény található. Például egy webes clickstream tekintve megállapítani, hogy hello töltött be a szolgáltatást.

**Bemeneti**:  

| Felhasználó | Szolgáltatás | Esemény | Time |
| --- | --- | --- | --- |
| user@location.com |RightMenu |Indítás |2015-01-01T00:00:01.0000000Z |
| user@location.com |RightMenu |Vége |2015-01-01T00:00:08.0000000Z |

**Kimeneti**:  

| Felhasználó | Szolgáltatás | Időtartam |
| --- | --- | --- |
| user@location.com |RightMenu |7 |

**Megoldás**:

````
    SELECT
        [user], feature, DATEDIFF(second, LAST(Time) OVER (PARTITION BY [user], feature LIMIT DURATION(hour, 1) WHEN Event = 'start'), Time) as duration
    FROM input TIMESTAMP BY Time
    WHERE
        Event = 'end'
````

**MAGYARÁZAT**: használata hello **utolsó** tooretrieve hello függvény utolsó **idő** értéke, ha hello esemény típusa történt **Start**. Hello **utolsó** függvény **PARTITION BY [user]** , amelyek eredménye hello tooindicate számított egyedi felhasználónként. hello lekérdezés rendelkezik hello időeltérése 1 órás maximális küszöbértéket **Start** és **leállítása** események, de igény szerint konfigurálható **(korlát DURATION(hour, 1)**.

## <a name="query-example-detect-hello-duration-of-a-condition"></a>Példa: hello időtartama feltétel észlelése
**Leírás**: található out mennyi ideig egy állapot fordult elő.
Tegyük fel például, hogy programhiba eredményezett (fent 20 000 font) egy helytelen súlyozással rendelkező összes autók. Azt szeretnénk, ha hello hiba toocompute hello időtartama.

**Bemeneti**:

| Ellenőrizze | Time | Súlyozás |
| --- | --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |2000 |
| Toyota |2015-01-01T00:00:02.0000000Z |25000 |
| Honda |2015-01-01T00:00:03.0000000Z |26000 |
| Toyota |2015-01-01T00:00:04.0000000Z |25000 |
| Honda |2015-01-01T00:00:05.0000000Z |26000 |
| Toyota |2015-01-01T00:00:06.0000000Z |25000 |
| Honda |2015-01-01T00:00:07.0000000Z |26000 |
| Toyota |2015-01-01T00:00:08.0000000Z |2000 |

**Kimeneti**:

| StartFault | EndFault |
| --- | --- |
| 2015-01-01T00:00:02.000Z |2015-01-01T00:00:07.000Z |

**Megoldás**:

````
    WITH SelectPreviousEvent AS
    (
    SELECT
    *,
        LAG([time]) OVER (LIMIT DURATION(hour, 24)) as previousTime,
        LAG([weight]) OVER (LIMIT DURATION(hour, 24)) as previousWeight
    FROM input TIMESTAMP BY [time]
    )

    SELECT 
        LAG(time) OVER (LIMIT DURATION(hour, 24) WHEN previousWeight < 20000 ) [StartFault],
        previousTime [EndFault]
    FROM SelectPreviousEvent
    WHERE
        [weight] < 20000
        AND previousWeight > 20000
````

**MAGYARÁZAT**: használata **LAG** tooview hello bemeneti adatfolyam 24 órán át, és keresse meg a példányok where **StartFault** és **StopFault** hello által felölelt vannak súly < 20000.

## <a name="query-example-fill-missing-values"></a>Példa: Töltse ki a hiányzó értékek
**Leírás**: hello az események streamjét a hiányzó, a készít rendszeres időközönként események adatfolyam.
Például generál egy eseményt 5 másodpercentként utoljára látott hello adatpont jelentéseket.

**Bemeneti**:

| T | érték |
| --- | --- |
| "2014-01-01T06:01:00" |1 |
| "2014-01-01T06:01:05" |2 |
| "2014-01-01T06:01:10" |3 |
| "2014-01-01T06:01:15" |4 |
| "2014-01-01T06:01:30" |5 |
| "2014-01-01T06:01:35" |6 |

**Kimeneti (első 10 sor)**:

| windowend | lastevent.t | lastevent.Value |
| --- | --- | --- |
| 2014-01-01T14:01:00.000Z |2014-01-01T14:01:00.000Z |1 |
| 2014-01-01T14:01:05.000Z |2014-01-01T14:01:05.000Z |2 |
| 2014-01-01T14:01:10.000Z |2014-01-01T14:01:10.000Z |3 |
| 2014-01-01T14:01:15.000Z |2014-01-01T14:01:15.000Z |4 |
| 2014-01-01T14:01:20.000Z |2014-01-01T14:01:15.000Z |4 |
| 2014-01-01T14:01:25.000Z |2014-01-01T14:01:15.000Z |4 |
| 2014-01-01T14:01:30.000Z |2014-01-01T14:01:30.000Z |5 |
| 2014-01-01T14:01:35.000Z |2014-01-01T14:01:35.000Z |6 |
| 2014-01-01T14:01:40.000Z |2014-01-01T14:01:35.000Z |6 |
| 2014-01-01T14:01:45.000Z |2014-01-01T14:01:35.000Z |6 |

**Megoldás**:

    SELECT
        System.Timestamp AS windowEnd,
        TopOne() OVER (ORDER BY t DESC) AS lastEvent
    FROM
        input TIMESTAMP BY t
    GROUP BY HOPPINGWINDOW(second, 300, 5)


**MAGYARÁZAT**: Ez a lekérdezés eseményeket hoz létre minden 5 másodperc és kimenetek hello korábban fogadott utolsó esemény. Hello [Hopping ablak](https://msdn.microsoft.com/library/dn835041.aspx "Hopping ablak--Azure Stream Analytics") időtartamát határozza meg, milyen távolságban hátsó hello lekérdezés keres toofind hello utolsó esemény (ebben a példában 300 másodperc).

## <a name="get-help"></a>Segítségkérés
Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Következő lépések
* [A Stream Analytics bemutatása tooAzure](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)

