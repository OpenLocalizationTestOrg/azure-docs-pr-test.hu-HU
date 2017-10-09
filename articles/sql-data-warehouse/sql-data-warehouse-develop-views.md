---
title: "az Azure SQL Data Warehouse aaaUsing T-SQL-nézetek |} Microsoft Docs"
description: "Tippek a Transact-SQL-nézetek használata az Azure SQL Data Warehouse adattárházzal történő, megoldások."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: b5208f32-8f4a-4056-8788-2adbb253d9fd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 3990b133946621691bdfa4b09523d21867470c74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="views-in-sql-data-warehouse"></a>Az SQL Data Warehouse-nézetek
Nézetek különösen hasznosak az SQL Data Warehouse. Számos különböző módon tooimprove hello minőségének a megoldás használható.  Ez a cikk emel ki, hogyan tooenrich a megoldás a nézetek, valamint toobe igénylő hello korlátozások figyelembe veendő néhány példát.

> [!NOTE]
> A szintaxis `CREATE VIEW` nem ez a cikk tárgyalja. Tekintse meg a toohello [NÉZET létrehozása] [ CREATE VIEW] cikket az MSDN-en a referencia jellegű információihoz.
> 
> 

## <a name="architectural-abstraction"></a>Az architektúra absztrakciós
Egy nagyon gyakori alkalmazásminta toore-használatával hozzon létre tábla AS kiválasztása (CTAS) egy objektum átnevezése mintát, miközben az adatok betöltése után táblák létrehozása.

az alábbi példa hello új rekordok tooa dátum dátumdimenzió hozzáadja. Vegye figyelembe, hogyan egy új tabble DimDate_New, létrejön, és majd átnevezett tooreplace hello eredeti verzió hello tábla.

```sql
CREATE TABLE dbo.DimDate_New
WITH (DISTRIBUTION = ROUND_ROBIN
, CLUSTERED INDEX (DateKey ASC)
)
AS
SELECT *
FROM   dbo.DimDate  AS prod
UNION ALL
SELECT *
FROM   dbo.DimDate_stg AS stg
;

RENAME OBJECT DimDate tooDimDate_Old;
RENAME OBJECT DimDate_New tooDimDate;

```

Ez a módszer azonban táblák jelenik meg, és egy felhasználó, valamint a "tábla nem létezik" hibaüzenet eltűnik eredményez. Nézetek lehet használt tooprovide felhasználók egységes megjelenítési réteggel hello alapul szolgáló objektumok váltják fel. Hozzáférés biztosítása a felhasználók által toohello adatok a nézetek azt jelenti, hogy felhasználóknak nem kell toohave látható-e hello alapjául szolgáló tábla. Ennek során győződjön meg arról, hogy a hello data warehouse tervezők hello adatmodell fejlődnek, és teljesítmény maximalizálása a CTAS hello Adatbetöltési folyamat során egységes felhasználói élményt nyújt.    

## <a name="performance-optimization"></a>Teljesítményoptimalizálás
Nézetek is lehet túlterhelt tooenforce optimalizált teljesítményt illesztést a táblák között. Például egy nézet egy redundáns terjesztési kulcs való csatlakozás feltételek toominimize adatmozgás hello részeként építhessék be.  Egy másik előnye, hogy a nézet egy adott lekérdezés tooforce vagy csatlakozó mutatót lehet. Nézetek használata ezen a módon kikapcsolja garantálja, hogy összekapcsolások mindig megtörténik az optimális módon elkerülhető az a felhasználók tooremember hello megfelelő konstruktor a táblákra hello szükségességét.

## <a name="limitations"></a>Korlátozások
Az SQL Data Warehouse nézetek metaadatok csak olyan.  Ennek következtében a következő beállítások hello nem érhetők el:

* Nincs kötés séma beállítás
* Alaptáblát keresztül hello nézet nem frissíthető
* Nézetek nem hozható létre az ideiglenes táblák
* Nem támogatott hello KIBONTÁS / NOEXPAND mutató
* Nincsenek az SQL Data Warehouse nem indexelt nézetek

## <a name="next-steps"></a>Következő lépések
További fejlesztési tippek: [SQL Data Warehouse fejlesztői áttekintés][SQL Data Warehouse development overview].
A `CREATE VIEW` szintaxis tekintse meg a túl[NÉZET létrehozása][CREATE VIEW].

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[CREATE VIEW]: https://msdn.microsoft.com/en-us/library/ms187956.aspx

<!--Other Web references-->
