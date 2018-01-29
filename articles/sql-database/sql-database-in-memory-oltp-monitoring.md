---
title: "XTP memórián belüli tároló figyelése |} Microsoft Docs"
description: "Becsült és a figyelő XTP memórián belüli tároló használatához kapacitás; Hárítsa el a kapacitás hiba 41823"
services: sql-database
documentationcenter: 
author: jodebrui
manager: jhubbard
editor: 
ms.assetid: b617308e-692c-4938-8fa2-070034a3ecef
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: jodebrui
ms.openlocfilehash: 1e7088e80cc86e3c7cf8ae8ea180d797de613e71
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/18/2018
---
# <a name="monitor-in-memory-oltp-storage"></a>A figyelő a memórián belüli online Tranzakciófeldolgozási tároló
Használata esetén [memórián belüli online Tranzakciófeldolgozási](sql-database-in-memory.md), a memóriaoptimalizált táblák és Táblaváltozók adatainak memórián belüli online Tranzakciófeldolgozási tárolóban található. Minden prémium szolgáltatásszintet felső korlátja memórián belüli online Tranzakciófeldolgozási tároló, amely ismertetett [egyetlen adatbázis erőforrás korlátok](sql-database-resource-limits.md#single-database-storage-sizes-and-performance-levels) és [rugalmas készlet erőforrás korlátok](sql-database-resource-limits.md#elastic-pool-change-storage-size). Ha túllépi ezt a határt, beszúrási és a frissítési műveletek kezdheti el önálló adatbázisok 41823 hiba és a rugalmas 41840 hiba miatt sikertelenül működő. Ekkor meg kell memória felszabadításához adatok törlése, illetve a teljesítményszintet az adatbázis frissítéséhez.

## <a name="determine-whether-data-fits-within-the-in-memory-oltp-storage-cap"></a>Határozza meg, hogy adatok elférje a memórián belüli online Tranzakciófeldolgozási tárolási kap
A tárolási biztosít, amelyet a különböző Premium szolgáltatásszintek határozza meg. Lásd: [egyetlen adatbázis erőforrás korlátok](sql-database-resource-limits.md#single-database-storage-sizes-and-performance-levels) és [rugalmas készlet erőforrás korlátok](sql-database-resource-limits.md#elastic-pool-change-storage-size).

Memóriakövetelményei memóriaoptimalizált táblák működik, mert az SQL Server ugyanúgy nem Azure SQL Database becslése. Tekintse át az előző cikket a néhány percet igénybe vehet [MSDN](https://msdn.microsoft.com/library/dn282389.aspx).

Tábla és a tábla változó sorok, valamint az indexek, száma, a maximális felhasználói adatok mérete felé. Emellett az ALTER TABLE kell elég hely a teljes táblázat és az indexeket új verziót hoz létre.

## <a name="monitoring-and-alerting"></a>Figyelés és riasztás
A tárolási kap százalékában memórián belüli tároló használata a teljesítmény szinthez a figyelheti a [Azure-portálon](https://portal.azure.com/): 

1. Az adatbázis panelen keresse meg az Erőforrás kihasználtsága mezőbe, majd kattintson a Szerkesztés.
2. Válassza ki a `In-Memory OLTP Storage percentage`.
3. Adja hozzá a riasztást, az erőforrás-használat párbeszédpanel megnyitásához a metrika panelen kattintson, majd kattintson a Hozzáadás riasztás.

Vagy a memóriában lévő tárhely kihasználtságát megjelenítéséhez használja a következő lekérdezést:

    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats


## <a name="correct-out-of-in-memory-oltp-storage-situations---errors-41823-and-41840"></a>Javítsa ki a-memória OLTP tárolási helyzetek - 41823 és 41840 hibák
Elérte-e a memórián belüli online Tranzakciófeldolgozási tárolási kap a Beszúrás adatbázis eredményezi, frissítse, ALTER, hibaüzenet 41823 (az önálló adatbázisok) vagy (a rugalmas) 41840 hiba miatt sikertelenül működő műveletek létrehozása. Mindkét hibákat okozhat az aktív tranzakció megszakítását.

Hibaüzenetek 41823 és 41840 azt jelzi, hogy a memóriaoptimalizált táblák és az adatbázis vagy a készlet Táblaváltozók elérte a memórián belüli online Tranzakciófeldolgozási maximális tárolómérete.

Ez a hiba vagy megoldása:

* A memóriaoptimalizált táblák, potenciálisan kiszervezésével a hagyományos, a lemezalapú táblák; az adatok adatok törlése vagy,
* A szolgáltatási rétegben váltson egy, az elegendő memórián belüli tároló memóriaoptimalizált táblák megtartja a szükséges adatok.

> [!NOTE] 
> Ritka esetekben hibák 41823 és 41840 átmeneti, ami azt jelenti, nincs elég tárterület a memórián belüli online Tranzakciófeldolgozási, és a művelet sikeres lehet. Ezért ajánlott mindkét figyelője az a teljes elérhető memórián belüli online Tranzakciófeldolgozási tárolót, és próbálja meg, amikor először észlelt a 41823 vagy 41840 hiba. Újrapróbálkozási logika kapcsolatos további információkért lásd: [előforduló ütközések észlelésére és logika ismételje meg a memórián belüli online Tranzakciófeldolgozási](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/transactions-with-memory-optimized-tables#conflict-detection-and-retry-logic).

## <a name="next-steps"></a>További lépések
Figyelés útmutatást, lásd: [figyelése Azure SQL Database dinamikus felügyeleti nézetekkel](sql-database-monitoring-with-dmvs.md).
