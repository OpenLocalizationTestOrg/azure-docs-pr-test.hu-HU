---
title: "Áttelepítése: Az adatraktár áttelepítés segédprogram |} Microsoft Docs"
description: "Telepítse át a tooSQL Data warehouse-bA."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 4d7ad981-ef31-4513-b337-50bdc4709c09
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 10/31/2016
ms.author: joeyong;barbkess
ms.openlocfilehash: c89909883fb42b0b04dd87a9973e5ee3e30d8f0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="data-warehouse-migration-utility-preview"></a>Data Warehouse Fájláttelepítő segédprogram (előzetes verzió)
> [!div class="op_single_selector"]
> * [Fájláttelepítő segédprogram letöltése][Download Migration Utility]
> 
> 

Data Warehouse áttelepítő segédprogramot hello olyan eszköz tervezett toomigrate séma- és SQL Server és az Azure SQL Database tooAzure SQL Data Warehouse adatait. Séma az áttelepítés során hello eszköz automatikusan leképezi a forrás toodestination hello megfelelő sémáját. Hello séma áttelepítése után a hello eszközök hello beállítás toomove adatokat automatikusan létrehozott parancsfájlokkal biztosít.

Ezenkívül tooschema és az adatok áttelepítési, a eszköz által biztosított, hello beállítás toogenerate kompatibilitási jelentések összefoglalója hello forrás- és példányai nem engedélyezi, hogy között inkompatibilitási problémák, amelyek zökkenőmentes áttelepítés.

## <a name="get-started"></a>Bevezetés
A telepítéshez előfeltételként szüksége lesz a hello BCP parancssori segédprogram toorun áttelepítési parancsprogramok és az Office tooview hello kompatibilitási jelentés. Miután elindította hello végrehajtható fájlt, akkor letölti lesz felszólító tooaccept szabványos EULA előtt hello eszköz települ.

Ezenkívül toorun hello áttelepítési Utiliy, akkor lesz kell hello egyik alábbi engedélyek használata, hogy csak a keresett toomigrate hello adatbázison: hozzon létre adatbázis, az ALTER ANY DATABASE vagy bármely NÉZETDEFINÍCIÓBAN.

### <a name="launching-hello-tool-and-connecting"></a>Hello eszköz megnyitása és csatlakozás
A post megjelenő hello asztali ikonra kattintva indítsa el hello eszköz telepítése. Hello eszköz megnyitásakor kérni fogja a egy kezdeti kapcsolat lapon adja meg a forrás és cél hello áttelepítési eszköz. Most támogatott SQL Server és az Azure SQL Database adatforrások és az SQL Data Warehouse célhelye. Ha bejelöli ezt követően kérni fogja tooconnect tooyour forráskiszolgáló kitölti a kiszolgálónevet és hitelesítéséhez, majd kattintson az "Csatlakozás".

Hitelesítés után hello eszköz jelennek meg, amelyek szerepelnek a hello kiszolgálót, amely kapcsolódik az adatbázisok listája. Lehet kezdeni az áttelepítést hello szeretné toomigrate, és kattintson a "Kijelölt áttelepítése" adatbázis kiválasztásával.

## <a name="migration-report"></a>Áttelepítési jelentés
Jelölje ki "Adatbázis kompatibilitásának ellenőrzése" hello eszköz hoz létre egy jelentést, minden objektum incompatibilities összefoglalójához hello adatbázis toomigrate kért. Néhány SQL Server funkcióit, amely nem jelenik meg az SQL Data Warehouse hello szélesebb körű listája megtalálható az [az áttelepítési dokumentáció][migration documentation]. Hello jelentés előállítása után fog tudni toosave és a nyitott hello jelentést az Excel programban.

Ne feledje, ha hello áttelepítési séma megállapítja, hogy "Object" módosul rendelés tooallow azonnali az áttelepítést, hogy a legtöbb probléma. Tekintse át a hello módosítások tooensure nem szeretné, hogy további módosításának toomake hello séma alkalmazása előtt.

## <a name="migrate-schema"></a>Séma áttelepítése
A csatlakozás után át séma kiválasztása hoz létre egy séma áttelepítési parancsfájl hello kijelölt táblákhoz. A parancsfájl portok hello struktúra hello tábla leképezi a nem kompatibilis adat típusok toomore kompatibilis űrlapok, és létrehozza a hitelesítő adatokat és a séma, ha ez az áttelepítési beállításokat hello hello felhasználói jelzi. Ezt a kódot is futtathatók megcélzott hello SQL Data Warehouse-példányhoz, tooa fájlt mentette, tooyour vágólapra másolt vagy beágyazott előtt további műveletek is szerkeszthető.  

Részben ismertetett beállításértékeket fent, amikor áttelepítése séma felülvizsgálati hello áttelepítési változik, hogy hello rendelés tooensure a eszköz által végrehajtott, hogy megértette őket.  

## <a name="migrate-data"></a>Adatok áttelepítése
Hello adatok áttelepítése a lehetőség, amely az adatok első tooflat a kiszolgálón található fájlok, parancsfájlok BCP hozhat létre, majd közvetlenül az SQL Data Warehouse be. Javasoljuk, hogy ez a folyamat kis mennyiségű adatokat, és az újrapróbálkozások áthelyezésére nincsenek beépített, majd a hibák akkor fordulhat elő, ha a veszteség hello hálózati kapcsolat. A rendezés toorun ez, szüksége lesz a toohave hello telepített BCP parancssori segédprogram és hello adatok hello sémája már létre kell.

Után hello paraméterek kitöltött fent, egyszerűen kell tooclick áttelepítés futtatása, és két csomagok készlete generál tooyour megadott helyre. Hello exportfájl rendelés tooexport adatok forrásból futtassa az áttelepítési egybesimított fájlokba, majd futtassa hello importfájl rendelés tooimport az adatait az SQL Data Warehouse.

## <a name="next-steps"></a>Következő lépések
Most, hogy néhány adat áttelepítése, ellenőrizze, hogyan túl[fejlesztése][develop].

<!--Image references-->

<!--Article references-->
[migration documentation]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md

<!--Other Web references--> 
[Download Migration Utility]: https://migrhoststorage.blob.core.windows.net/sqldwsample/DataWarehouseMigrationUtility.zip
