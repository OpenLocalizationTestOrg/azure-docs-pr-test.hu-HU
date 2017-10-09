---
title: "aaaMigrate a meglévő Azure adatraktár toopremium tárolási |} Microsoft Docs"
description: "Áttelepítését egy meglévő adatraktár toopremium adattárolás"
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: barbkess
editor: 
ms.assetid: 04b05dea-c066-44a0-9751-0774eb84c689
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 11/29/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: 145199c2da1f6f1fb8898626cd04886c42d82204
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-data-warehouse-toopremium-storage"></a>A data warehouse toopremium tároló áttelepítése
Új Azure SQL Data Warehouse [prémium szintű storage, a nagyobb teljesítmény kiszámíthatóságot][premium storage for greater performance predictability]. Jelenleg a szabványos tárolón adatraktárakra mostantól lehet meglévő adatok toopremium tárolási át. Automatikus áttelepítése előnyeit élvezheti, vagy ha jobban szeret toocontrol során (amely tartalmaz, amely bizonyos időre leállítást) toomigrate, teheti hello áttelepítési saját maga.

Ha egynél több adatraktár, használja a hello [automatikus áttelepítése ütemezés] [ automatic migration schedule] toodetermine, ha akkor is telepíthetők át.

## <a name="determine-storage-type"></a>Tárolási típus meghatározása
Ha egy adatraktár hello következő dátumok előtt létrehozott, Standard szintű storage jelenleg használt.

| **Régió** | **Ez a dátum előtt létrehozott adatraktár** |
|:--- |:--- |
| Kelet-Ausztrália |Prémium szintű storage még nem érhető el |
| Kelet-Kína |2016. november 1. |
| Észak-Kína |2016. november 1. |
| Közép-Németország |2016. november 1. |
| Északkelet-Németország |2016. november 1. |
| Nyugat-India |Prémium szintű storage még nem érhető el |
| Nyugat-Japán |Prémium szintű storage még nem érhető el |
| USA északi középső régiója |2016. november 10. |

## <a name="automatic-migration-details"></a>Automatikus áttelepítése részletei
Alapértelmezés szerint kiforrottabb lesz az adatbázis az Ön 18:00:00 és 6:00-kor a régió helyi idő közötti során hello [automatikus áttelepítése ütemezés][automatic migration schedule]. A meglévő adatraktár használhatatlan lesz, hello az áttelepítés során. hello áttelepítési / adatraktár lépnek / terabájt tárhelyet körülbelül egy óra. Nem kell fizetnie bármely részének hello automatikus áttelepítése során.

> [!NOTE]
> Hello áttelepítés akkor fejeződött be, amikor az adatraktár lesznek ismét online állapotú, és használható.
>
>

A Microsoft hello a következő lépéseket toocomplete hello áttelepítési (ezek nem igényelnek beavatkozást bármely beavatkozás) tart. Ebben a példában képzelhető el, hogy a meglévő adatraktár szabványos tárolón jelenleg neve "MyDW."

1. Microsoft túl átnevezi "MyDW" "[időbélyeg] MyDW_DO_NOT_USE_."
2. A Microsoft leállítja "[időbélyeg] MyDW_DO_NOT_USE_." Ebben az időszakban a biztonsági másolat használatban van. Ha azt a folyamat során problémába ütközik jelenhet meg több szüneteltetése és folytatása.
3. Microsoft hoz létre egy új data warehouse "MyDW" a prémium szintű storage hello biztonsági másolatból a 2. lépésben hozott nevű. "MyDW" csak illeszti hello visszaállítás befejezése után.
4. Hello visszaállítás befejezése után "MyDW" arkusz toohello ugyanazokat az adatokat az adatraktár-egységek, és állapot (felfüggesztett aktív), de a hello áttelepítés előtt.
5. A Microsoft hello áttelepítés befejezése után törli az "MyDW_DO_NOT_USE_ [időbélyeg]".

> [!NOTE]
> hello következő beállítások nem jelenik meg az áttelepítés hello:
>
> * Naplózását hello adatbázis szintjén toobe újra engedélyezni kell.
> * Tűzfalszabályok hello adatbázis szintjén kell toobe újból hozzáadták. Hello kiszolgálószintű tűzfal-szabályokat, nem érintettek.
>
>

### <a name="automatic-migration-schedule"></a>Automatikus áttelepítése ütemezése
Automatikus áttelepítések során kimaradás ütemezés a következő hello elő 18:00:00 és 6:00-kor (helyi idő régiónként) között.

| **Régió** | **Becsült kezdő dátuma** | **Becsült befejezési dátum** |
|:--- |:--- |:--- |
| Kelet-Ausztrália |Még nem határozza meg |Még nem határozza meg |
| Kelet-Kína |2017. január 9. |2017. január 13. |
| Észak-Kína |2017. január 9. |2017. január 13. |
| Közép-Németország |2017. január 9. |2017. január 13. |
| Északkelet-Németország |2017. január 9. |2017. január 13. |
| Nyugat-India |Még nem határozza meg |Még nem határozza meg |
| Nyugat-Japán |Még nem határozza meg |Még nem határozza meg |
| USA északi középső régiója |2017. január 9. |2017. január 13. |

## <a name="self-migration-toopremium-storage"></a>Önálló áttelepítés toopremium tároló
Ha azt szeretné toocontrol, amikor megtörténik a leállás, követő lépéseket toomigrate egy meglévő adatraktár szabványos tárolási toopremium tároló hello is használhatja. Ha ezt a lehetőséget választja, el kell végeznie a hello önálló áttelepítés, az adott régióban megkezdése hello automatikus áttelepítése előtt. Ez biztosítja, hogy azt ne hello automatikus áttelepítése ütközést okoz (tekintse meg a toohello [automatikus áttelepítése ütemezés][automatic migration schedule]).

### <a name="self-migration-instructions"></a>Önálló áttelepítési utasítások
az adatok adatraktár saját magának, hello biztonságimásolat-készítő és szolgáltatások visszaállítása toomigrate. hello visszaállítási hello áttelepítési része várt tootake / / adatraktár tárolási terabájt körülbelül egy óra. Ha azt szeretné, hogy tookeep hello azonos nevet áttelepítés befejezése után, kövesse a hello [lépéseket toorename áttelepítéskor][steps toorename during migration].

1. [Felfüggesztés] [ Pause] az adatraktár. Ehhez szükséges, az automatikus biztonsági mentés.
2. [Visszaállítás] [ Restore] a legutóbbi pillanatképből.
3. Törölje a meglévő adatraktár szabványos tárolón. **Ha ezt a lépést nem toodo, akkor mindkét adatraktárak fogjuk számlázni.**

> [!NOTE]
> hello következő beállítások nem jelenik meg az áttelepítés hello:
>
> * Naplózását hello adatbázis szintjén toobe újra engedélyezni kell.
> * Tűzfalszabályok hello adatbázis szintjén kell toobe újból hozzáadták. Hello kiszolgálószintű tűzfal-szabályokat, nem érintettek.
>
>

#### <a name="rename-data-warehouse-during-migration-optional"></a>Adatraktár átnevezése (választható) az áttelepítés során
Hello azonos logikai kiszolgáló nem rendelkezik a két adatbázis hello ugyanazzal a névvel. Az SQL Data Warehouse mostantól támogatja a hello képességét toorename adatraktár.

Ebben a példában képzelhető el, hogy a meglévő adatraktár szabványos tárolón jelenleg neve "MyDW."

1. Nevezze át a "MyDW" hello következő ALTER DATABASE parancs használatával. (A példában azt kell nevezni "MyDW_BeforeMigration.")  Ez a parancs minden meglévő tranzakciókat, és hello főadatbázis toosucceed kell elvégezni.
   ```
   ALTER DATABASE CurrentDatabasename MODIFY NAME = NewDatabaseName;
   ```
2. [Felfüggesztés] [ Pause] "MyDW_BeforeMigration." Ehhez szükséges, az automatikus biztonsági mentés.
3. [Visszaállítás] [ Restore] hello nevű új adatbázis a legutóbbi pillanatképből toobe (például "MyDW") használt.
4. Törölje a "MyDW_BeforeMigration." **Ha ezt a lépést nem toodo, akkor mindkét adatraktárak fogjuk számlázni.**


## <a name="next-steps"></a>Következő lépések
A hello toopremium tárolási módosításához hello mögöttes architektúráját, valamint az adatraktár adatbázis-blob fájlok számának is rendelkezik. Ez a változás toomaximize hello által a fürtözött oszlopcentrikus indexek újraépítése hello a következő parancsfájl használatával. hello parancsfájl néhányat a meglévő toohello további a BLOB-adatobjektumok kényszeríthet működik. Ha nem tesz semmit, hello adatokat fog természetes terjesztenie adott idő alatt, több adatot tölt be a táblába.

**Előfeltételek:**

- hello adatraktár futtasson-e 1000 adattárházegységek vagy annál újabb (lásd: [méretezési számítási teljesítményt][scale compute power]).
- hello hello parancsfájlt végrehajtó felhasználó kell hello [mediumrc szerepkör] [ mediumrc role] vagy újabb verzióját. egy felhasználó toothis szerepkör tooadd hello következő hajtható végre:````EXEC sp_addrolemember 'xlargerc', 'MyUser'````

````sql
-------------------------------------------------------------------------------
-- Step 1: Create table toocontrol index rebuild
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------
create table sql_statements
WITH (distribution = round_robin)
as select
    'alter index all on ' + s.name + '.' + t.NAME + ' rebuild;' as statement,
    row_number() over (order by s.name, t.name) as sequence
from
    sys.schemas s
    inner join sys.tables t
        on s.schema_id = t.schema_id
where
    is_external = 0
;
go

--------------------------------------------------------------------------------
-- Step 2: Execute index rebuilds. If script fails, hello below can be re-run toorestart where last left off.
-- Run as user in mediumrc or higher
--------------------------------------------------------------------------------

declare @nbr_statements int = (select count(*) from sql_statements)
declare @i int = 1
while(@i <= @nbr_statements)
begin
      declare @statement nvarchar(1000)= (select statement from sql_statements where sequence = @i)
      print cast(getdate() as nvarchar(1000)) + ' Executing... ' + @statement
      exec (@statement)
      delete from sql_statements where sequence = @i
      set @i += 1
end;
go
-------------------------------------------------------------------------------
-- Step 3: Clean up table created in Step 1
--------------------------------------------------------------------------------
drop table sql_statements;
go
````

Ha problémába ütközik az adatraktárban [hozzon létre egy támogatási jegy] [ create a support ticket] és "áttelepítési toopremium tároló" hello lehetséges ok, hivatkozik.

<!--Image references-->

<!--Article references-->
[automatic migration schedule]: #automatic-migration-schedule
[self-migration tooPremium Storage]: #self-migration-to-premium-storage
[create a support ticket]: sql-data-warehouse-get-started-create-support-ticket.md
[Azure paired region]: best-practices-availability-paired-regions.md
[main documentation site]: services/sql-data-warehouse.md
[Pause]: sql-data-warehouse-manage-compute-portal.md#pause-compute
[Restore]: sql-data-warehouse-restore-database-portal.md
[steps toorename during migration]: #optional-steps-to-rename-during-migration
[scale compute power]: sql-data-warehouse-manage-compute-portal.md#scale-compute-power
[mediumrc role]: sql-data-warehouse-develop-concurrency.md

<!--MSDN references-->


<!--Other Web references-->
[Premium Storage for greater performance predictability]: https://azure.microsoft.com/en-us/blog/azure-sql-data-warehouse-introduces-premium-storage-for-greater-performance/
[Azure Portal]: https://portal.azure.com
