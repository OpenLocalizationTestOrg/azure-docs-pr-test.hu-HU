---
title: "aaaMigrate a séma tooSQL adatraktár |} Microsoft Docs"
description: "Tippek a séma tooAzure SQL Data Warehouse adattárházzal történő, megoldások áttelepítéséhez."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 538b60c9-a07f-49bf-9ea3-1082ed6699fb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 10/31/2016
ms.author: joeyong;barbkess
ms.openlocfilehash: 1309b743b78564575695038a4856d9d25a2b18d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-schemas-toosql-data-warehouse"></a>Telepítse át a sémák tooSQL Data warehouse-bA
Az SQL-sémák tooSQL Data warehouse-ba történő áttelepítésére vonatkozó útmutatást. 

## <a name="plan-your-schema-migration"></a>A séma áttelepítés tervezése

Áttelepítés tervezése közben, lásd: hello [tábla áttekintése] [ table overview] toobecome ismeri a táblázat kialakítási szempontok például statisztikáiról, terjesztési, particionálás, és az indexelés.  Azt is soroljuk [táblában funkciók nem támogatott] [ unsupported table features] és azok megoldását ismerteti.

## <a name="use-user-defined-schemas-tooconsolidate-databases"></a>Felhasználó által definiált sémák tooconsolidate adatbázisok használata

A meglévő alkalmazások és szolgáltatások valószínűleg tartalmazza az egynél több adatbázisból. Egy SQL Server data warehouse tartalmazhat például egy átmeneti adatbázis, adatraktár-adatbázis és néhány adat adatközpont-adatbázis. Ebben a topológiában minden adatbázisnak külön biztonsági házirendek külön feladatként fut.

Ezzel szemben a SQL Data Warehouse hello teljes adatraktározás számítási feladatáról egy adatbázison belül futtatja. Adatbázis közötti illesztések nem engedélyezettek. Ezért az SQL Data Warehouse hello data warehouse toobe hello egy adatbázisban tárolt által használt összes táblának vár.

Azt javasoljuk, felhasználó által definiált sémák tooconsolidate a meglévő alkalmazások és szolgáltatások egy adatbázisba. Tekintse meg a [felhasználói sémák](sql-data-warehouse-develop-user-defined-schemas.md)

## <a name="use-compatible-data-types"></a>Kompatibilis adattípusok használata
Módosítsa az adatok típusok toobe az SQL Data Warehouse szolgáltatással kompatibilis. Támogatott és nem támogatott adattípusú listájáért lásd: [adattípusok][data types]. Témakör biztosít a lehetséges megoldások hello nem támogatott típusú. Emellett biztosítja a lekérdezés tooidentify meglévő típusok nem támogatottak az SQL Data Warehouse.

## <a name="minimize-row-size"></a>Sor mérete minimalizálása érdekében
A legjobb teljesítmény érdekében minimalizálása érdekében a táblák hello sorhosszt. Rövidebb sor hosszának toobetter teljesítmény vezethet, mivel hello legkisebb adattípusok használatát, amelyek működnek az adatok. 

A táblázat sor szélessége a PolyBase van a 1 Megabájtos korlátot.  Ha tooload adatokat az SQL Data Warehouse PolyBase rendelkező, frissítse a táblák toohave sor maximális szélessége 1 MB-nál kevesebb. 

<!--
- For example, this table uses variable length data but hello largest possible size of hello row is still less than 1 MB. PolyBase will load data into this table.

- This table uses variable length data and hello defined row width is less than one MB. When loading rows, PolyBase allocates hello full length of hello variable-length data. hello full length of this row is greater than one MB.  PolyBase will not load data into this table.  

-->

## <a name="specify-hello-distribution-option"></a>Hello terjesztési beállítást adja meg
Az SQL Data Warehouse egy olyan elosztott adatbázis rendszer. Minden tábla elosztott vagy replikált hello számítási csomópontjai között. Nincs olyan tábla beállítás, amely lehetővé teszi, hogy adja meg, hogyan toodistribute hello adatokat. hello használhatók ciklikus multiplexelés replikálva, vagy kivonatoló elosztott. Minden egyes vannak előnyei és hátrányai. Ha nem ad meg hello terjesztő beállítást, az SQL Data Warehouse használja ciklikus multiplexelés hello alapértelmezettként.

- Ciklikus multiplexelés hello alapértelmezett beállítás. Hello legegyszerűbb toouse, és lehetséges gyorsaságának hello adatokat tölt, de illesztések adatmozgás, ami megnöveli a lekérdezési teljesítmény szükséges.
- A replikált tároló egy-egy példányát minden számítási csomóponton hello tábla. Replikált táblák nincsenek performant, mert az összekapcsolásokhoz és az összesítések nem igényelnek adatátvitelt jelölik. Ezek – megnövelt tárhely szükséges, és ezért működhet a legjobban a kisebb táblákat.
- Elosztott kivonatoló hello sorok elosztása az összes hello csomópontok keresztül kivonatoló függvényt. A kivonattáblákkal elosztott hello szív az SQL Data Warehouse, mert azok tervezett tooprovide a lekérdezési teljesítmény nagy táblákon. Ez a beállítás megköveteli néhány tervezési tooselect hello legjobb oszlop toodistribute hello adatokról. Azonban ha nem választja hello legjobb oszlop hello először, könnyen újra terjesztheti hello adatokat egy másik oszlop alapján. 

toochoose hello ajánlott terjesztési beállítás mindegyikhez, lásd: [táblák elosztott](sql-data-warehouse-tables-distribute.md).


## <a name="next-steps"></a>Következő lépések
Miután sikeresen áttelepítette az adatbázis-séma tooSQL Data warehouse-ba, folytassa a következő cikkek hello tooone:

* [Adatok áttelepítése][Migrate your data]
* [Telepítse át a kódot][Migrate your code]

Az SQL Data Warehouse gyakorlati tanácsokat kapcsolatban bővebben lásd: hello [ajánlott eljárások] [ best practices] cikk.

<!--Image references-->

<!--Article references-->
[Migrate your code]: ./sql-data-warehouse-migrate-code.md
[Migrate your data]: ./sql-data-warehouse-migrate-data.md
[best practices]: ./sql-data-warehouse-best-practices.md
[table overview]: ./sql-data-warehouse-tables-overview.md
[unsupported table features]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[data types]: ./sql-data-warehouse-tables-data-types.md
[unsupported data types]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types

<!--MSDN references-->


<!--Other Web references-->
