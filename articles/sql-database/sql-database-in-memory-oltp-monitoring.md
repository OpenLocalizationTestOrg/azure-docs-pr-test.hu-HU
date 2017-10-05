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
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: jodebrui
ms.openlocfilehash: 5afb2209f18b1ba2aa0a916a439509b01afd80da
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-in-memory-oltp-storage"></a>Memóriabeli OLTP-tár figyelése
Használata esetén [memórián belüli online Tranzakciófeldolgozási](sql-database-in-memory.md), a memóriaoptimalizált táblák és Táblaváltozók adatainak memórián belüli online Tranzakciófeldolgozási tárolóban található. Minden prémium szolgáltatásszintet felső korlátja memórián belüli online Tranzakciófeldolgozási tárolási, amely ismerteti a [SQL Database Service Tiers cikk](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels). Ha túllépi ezt a határt, beszúrási és a frissítési műveletek kezdheti el hiba miatt sikertelenül (41823). Ezen a ponton fog kell bármelyik törli memória felszabadításához adatokat, illetve a teljesítményszintet az adatbázis frissítéséhez.

## <a name="determine-whether-data-will-fit-within-the-in-memory-storage-cap"></a>Határozza meg, hogy adatokat illeszkedni fog-e a memórián belüli tároló kap
Határozza meg a tárolási kap: tekintse át a [SQL Database Service Tiers cikk](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) számára a tárolási biztosít, amelyet a különböző Premium szolgáltatásszintek.

Memóriakövetelményei memóriaoptimalizált táblák működik, mert az SQL Server ugyanúgy nem Azure SQL Database becslése. Tekintse át a témakör a néhány percet igénybe vehet [MSDN](https://msdn.microsoft.com/library/dn282389.aspx).

Vegye figyelembe, hogy a tábla és változó táblázat sorait, valamint indexek száma felé a maximális felhasználói adatok mérete. Emellett az ALTER TABLE kell elég hely a teljes táblázat és az indexeket új verziót hoz létre.

## <a name="monitoring-and-alerting"></a>Figyelés és riasztás
Figyelheti a memórián belüli tárolóhasználat százalékában a [tárolási kap a teljesítmény szinthez](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) az Azure-ban [portal](https://portal.azure.com/): 

* Az adatbázis panelen keresse meg az Erőforrás kihasználtsága mezőbe, majd kattintson a Szerkesztés.
* Válassza ki a metrika `In-Memory OLTP Storage percentage`.
* Adja hozzá a riasztást, az erőforrás-használat párbeszédpanel megnyitásához a metrika panelen kattintson, majd kattintson a Hozzáadás riasztás.

Vagy a memóriában lévő tárhely kihasználtságát megjelenítéséhez használja a következő lekérdezést:

    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats


## <a name="correct-out-of-memory-situations---error-41823"></a>Javítsa ki a kevés memória helyzetek - 41823 hiba
Kevés a memória eredmények futó INSERT, UPDATE és LÉTREHOZÁSI műveletek 41823 hiba miatt sikertelen.

Hibaüzenet 41823 azt jelzi, hogy a memóriaoptimalizált táblák és Táblaváltozók túllépte a maximális méretet.

Ez a hiba vagy megoldása:

* A memóriaoptimalizált táblák, potenciálisan kiszervezésével a hagyományos, a lemezalapú táblák; az adatok adatok törlése vagy,
* A szolgáltatási rétegben váltson egy, az elegendő memórián belüli tároló memóriaoptimalizált táblák megtartja a szükséges adatok.

## <a name="next-steps"></a>Következő lépések
Figyelés útmutatást, lásd: [figyelése Azure SQL Database dinamikus felügyeleti nézetekkel](sql-database-monitoring-with-dmvs.md).
