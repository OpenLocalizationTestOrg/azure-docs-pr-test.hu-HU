---
title: "aaaManage számítási teljesítményt az Azure SQL Data Warehouse (áttekintés) |} Microsoft Docs"
description: "Az Azure SQL Data Warehouse képességek kibővítési teljesítményét. Horizontális felskálázás dwu-k módosításával vagy és sablonok felfüggesztése és folytatása a számítási erőforrások toosave költségeket."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: 
ms.assetid: e13a82b0-abfe-429f-ac3c-f2b6789a70c6
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 03/22/2017
ms.author: elbutter
ms.openlocfilehash: 1ffbe8d694ac181eaeb6f585a2cee87a570ed7d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-overview"></a>Kezelheti a számítási teljesítményt az Azure SQL Data Warehouse (áttekintés)
> [!div class="op_single_selector"]
> * [Áttekintés](sql-data-warehouse-manage-compute-overview.md)
> * [Portál](sql-data-warehouse-manage-compute-portal.md)
> * [PowerShell](sql-data-warehouse-manage-compute-powershell.md)
> * [REST](sql-data-warehouse-manage-compute-rest-api.md)
> * [TSQL](sql-data-warehouse-manage-compute-tsql.md)
>
>

az SQL Data Warehouse architektúrája hello elválasztja a tárolást és számítást, így függetlenül minden tooscale. Ennek eredményeképpen a számítási méretezett toomeet teljesítményigénye hello adatmennyiségtől függetlenül lehet. Ez az architektúra természetes következménye, hogy [számlázási] [ billed] számítási és tárolási elkülönül. 

Ez az áttekintés bemutatja hogyan terjessze ki az SQL Data Warehouse, és hogyan tooutilize hello szüneteltetése, folytatása és az SQL Data Warehouse méretezési képességeket biztosít. Tekintse át a hello [az adatraktár-(dwu)] [ data warehouse units (DWUs)] lap toolearn hogyan dwu-k és teljesítmény kapcsolódnak. 

## <a name="how-compute-management-operations-work-in-sql-data-warehouse"></a>Milyen számítási felügyeleti műveletek a vártnak az SQL Data Warehouse
hello architektúra az SQL Data Warehouse áll-e a vezérlő csomópont, a számítási csomópontok és hello tárolási réteg 60 terjesztéseket elosztva. 

Egy normál aktív munkamenet során az SQL Data Warehouse a rendszer átjárócsomópont hello metaadatok kezeli, és hello elosztott lekérdezésoptimalizáló tartalmazza. A head csomópont alatt a számítási csomópontok és a tárolási rétegből állnak. A DWU 400 a rendszer egy átjárócsomóponttal, négy számítási csomópontok és hello tárolási rétegből álló 60 terjesztéseket rendelkezik. 

A skála változni vagy művelet felfüggeszti, hello rendszer először használhatatlanná teszi az összes bejövő lekérdezés, és visszavon tranzakciók tooensure konzisztens állapotú legyen. A skálázási műveletek, a méretezés akkor történik, a tranzakciós visszaállítás befejezése után. A méretezett művelet hello rendszerre vonatkozó hello számítási csomópontok extra kívánt számát, és megkezdi a újracsatlakoztatása hello számítási csomópontok toohello tárolási réteg. Egy lefelé méretezéshez művelet hello felesleges csomópontok kiadott, majd hello fennmaradó számítási csomópontok újra csatlakoztatja, maguk toohello megfelelő számú terjesztési. A szüneteltetési művelete minden számítási csomópont feloldásáig blokkolva lesz, és a rendszer változni fog metaadat-műveletek tooleave számos a végső rendszer stabil állapotban.

| DWU  | \#a számítási csomópontok | \#az azokat a terjesztéseket, csomópontonként |
| ---- | ------------------ | ---------------------------- |
| 100  | 1                  | 60                           |
| 200  | 2                  | 30                           |
| 300  | 3                  | 20                           |
| 400  | 4                  | 15                           |
| 500  | 5                  | 12                           |
| 600  | 6                  | 10                           |
| 1000 | 10                 | 6                            |
| 1200 | 12                 | 5                            |
| 1500 | 15                 | 4                            |
| 2000 | 20                 | 3                            |
| 3000 | 30                 | 2                            |
| 6000 | 60                 | 1                            |

hello három elsődleges funkciója számítási kezeléséhez a következők:

1. Szünet
2. Folytatás
3. Méretezés

Mindkét művelet eltarthat néhány percig toocomplete. Ha skálázás/felfüggesztése vagy folytatása automatikusan, érdemes lehet tooimplement logika tooensure, hogy bizonyos műveletek elvégzése után egy másik művelet folytatása előtt. 

Különböző végpontok keresztül hello adatbázis állapotának ellenőrzése lehetővé teszi az ilyen műveletek toocorrectly megvalósítása automatizálását. hello portal befejezett egy műveletet, és hello adatbázisok aktuális állapot értesítést küldenek, de nem engedélyezi a állapot ellenőrzéséhez szoftveres. 

>  [!NOTE]
>
>  Számítási felügyeleti funkció végpontjai között nem létezik.
>
>  

|              | Felfüggesztése vagy folytatása | Méretezés | Ellenőrizze az adatbázis állapota |
| ------------ | ------------ | ----- | -------------------- |
| Azure Portal | Igen          | Igen   | **Nem**               |
| PowerShell   | Igen          | Igen   | Igen                  |
| REST API     | Igen          | Igen   | Igen                  |
| T-SQL        | **Nem**       | Igen   | Igen                  |



<a name="scale-compute-bk"></a>

## <a name="scale-compute"></a>Skála számítási

Az SQL Data Warehouse teljesítményének mérése [az adatraktár-(dwu)] [ data warehouse units (DWUs)] Ez az egy számítási erőforrásokat, például a Processzor, memória és I/O sávszélesség abstracted mértékét. A felhasználó, aki tooscale kívánja a rendszer teljesítményét ehhez különböző eszközökkel, például hello portálon, a T-SQL és a REST API-k segítségével. 

### <a name="how-do-i-scale-compute"></a>Hogyan méretezni a számítási?
Számítási teljesítményt van kezelve az SQL Data Warehouse hello DWU beállítás módosításával. Teljesítménynövekedést [lineárisan] [ linearly] további DWU bizonyos műveletek való hozzáadása során.  Kínálunk DWU ajánlatokat, amelyek biztosítják, hogy a teljesítmény változik feltétlenül felfelé vagy lefelé amikor méretezni a rendszer. 

tooadjust dwu-k, használja az alábbi egyes módszereket.

* [Skála számítási teljesítményt az Azure-portálon][Scale compute power with Azure portal]
* [Skála számítási teljesítményt a PowerShell használatával][Scale compute power with PowerShell]
* [Skála számítási teljesítményt REST API-khoz][Scale compute power with REST APIs]
* [Skála számítási teljesítményt a TSQL használatával][Scale compute power with TSQL]

### <a name="how-many-dwus-should-i-use"></a>Hány dwu-k érdemes használni?

toounderstand milyen az ideális DWU érték van, próbálja meg skálázás felfelé és lefelé, és futtasson néhány lekérdezést az adatok betöltése után. Mivel a skálázás nem időigényes, próbálja meg különböző teljesítményszintek vagy kevesebb mint egy óra alatt. 

> [!Note] 
> Az SQL Data Warehouse tervezett tooprocess nagy mennyiségű adat. toosee toouse a nagy adatkészletet, ami megközelíti vagy meghaladja az 1 TB-os kívánt méretezéshez, különösen a nagyobb dwu-k, igaz képességeit.

Javaslatok keresése a munkaterhelés számára legjobb DWU hello:

1. Az adatok a fejlesztési először egy kisebb DWU teljesítményszintet kiválasztása.  Jó kiindulópont DW400 vagy DW200.
2. Az alkalmazás teljesítményének figyelése, erőforrásigények toohello teljesítmény választott dwu-k számának hello betartásával képest.
3. Határozza meg, mennyi gyorsabb vagy alacsonyabb teljesítményt akkor tooreach hello optimális teljesítményszint szükséges a követelmények teljesítéséhez által lineáris skála feltételezve kell lennie.
4. Növeli vagy csökkenti a hello szám dwu-k a arányban toohow sokkal gyorsabb vagy alacsonyabb, amelyet a munkaterhelés tooperform. 
5. Folytatható, amíg el nem éri az optimális teljesítmény szintű üzleti igényeinek a szükséges módosításokat.

> [!NOTE]
>
> Lekérdezési teljesítmény csak további párhuzamos folyamatkezelést biztosítja a növekszik, ha hello munkahelyi oszthatók számítási csomópontjai között. Ha úgy találja, hogy skálázás nem változik a teljesítményt, adjon tekintse meg a cikkek toocheck, hogy az adatok nem egyenlően van elosztva, vagy ha bevezették az adatátvitelt jelölik a nagy mennyiségű teljesítményhangolás. 

### <a name="when-should-i-scale-dwus"></a>Mikor érdemes méretezni a dwu-k?
A következő fontos forgatókönyveit hello dwu-k skálázás módosítja:

1. A vizsgálatok, összesítések és CTAS utasítások hello rendszer teljesítményét lineárisan módosítása
2. Olvasók és a polybase-zel betöltésekor írók növekvő hello száma
3. Egyidejű lekérdezések és feldolgozási üzembe helyezési ponti maximális száma

A javaslatok tooscale dwu-k számát:

1. Mielőtt a nagy mennyiségű adat betöltenie vagy átalakítási műveletet hajt végre, növelheti dwu-k, hogy az adatok elérhető gyorsabban.
2. Csúcsidőszak munkaidőben méretezhető tooaccommodate nagy mennyiségű egyidejű lekérdezéseket. 

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Felfüggesztés számítási
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

toopause egy adatbázisban, használja az alábbi egyes módszereket.

* [Felfüggesztés számítási az Azure-portálon][Pause compute with Azure portal]
* [Felfüggesztés számítási a PowerShell használatával][Pause compute with PowerShell]
* [Felfüggesztés számítási REST API-khoz][Pause compute with REST APIs]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Folytatás számítási
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

tooresume egy adatbázisban, használja az alábbi egyes módszereket.

* [Folytatás számítási az Azure-portálon][Resume compute with Azure portal]
* [Folytatás számítási a PowerShell használatával][Resume compute with PowerShell]
* [Folytatás számítási REST API-khoz][Resume compute with REST APIs]

<a name="check-compute-bk"></a>

## <a name="check-database-state"></a>Ellenőrizze az adatbázis állapota 

tooresume egy adatbázisban, használja az alábbi egyes módszereket.

- [A T-SQL adatbázis állapotának ellenőrzése][Check database state with T-SQL]
- [Ellenőrizze az adatbázis állapotát a PowerShell használatával][Check database state with PowerShell]
- [Ellenőrizze az adatbázis állapotát a REST API-k][Check database state with REST APIs]

## <a name="permissions"></a>Engedélyek

Méretezési hello adatbázis ismertetett hello engedélyekkel kell rendelkeznie [ALTER DATABASE][ALTER DATABASE].  Szüneteltetése és folytatása szükséges hello [SQL DB Contributor] [ SQL DB Contributor] engedéllyel, akkor a kifejezetten Microsoft.Sql/servers/databases/action.

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Következő lépések
Tekintse meg a következő cikkek toohelp ismernie néhány további legfontosabb teljesítményi fogalmak toohello:

* [Munkaterhelés és feldolgozási kezelése][Workload and concurrency management]
* [Tábla kialakítás áttekintése][Table design overview]
* [Elosztása][Table distribution]
* [Tábla indexelő][Table indexing]
* [Táblaparticionálást][Table partitioning]
* [Tábla statisztikai adatainak][Table statistics]
* [Gyakorlati tanácsok][Best practices]

<!--Image reference-->

<!--Article references-->
[data warehouse units (DWUs)]: ./sql-data-warehouse-overview-what-is.md#predictable-and-scalable-performance-with-data-warehouse-units
[billed]: https://azure.microsoft.com/en-us/pricing/details/sql-data-warehouse/
[linearly]: ./sql-data-warehouse-overview-what-is.md#predictable-and-scalable-performance-with-data-warehouse-units
[Scale compute power with Azure portal]: ./sql-data-warehouse-manage-compute-portal.md#scale-compute-power
[Scale compute power with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#scale-compute-bk
[Scale compute power with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#scale-compute-bk
[Scale compute power with TSQL]: ./sql-data-warehouse-manage-compute-tsql.md#scale-compute-bk

[capacity limits]: ./sql-data-warehouse-service-capacity-limits.md

[Pause compute with Azure portal]:  ./sql-data-warehouse-manage-compute-portal.md#pause-compute-bk
[Pause compute with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#pause-compute-bk
[Pause compute with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#pause-compute-bk

[Resume compute with Azure portal]:  ./sql-data-warehouse-manage-compute-portal.md#resume-compute-bk
[Resume compute with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#resume-compute-bk
[Resume compute with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#resume-compute-bk

[Check database state with T-SQL]: ./sql-data-warehouse-manage-compute-tsql.md#check-database-state-and-operation-progress
[Check database state with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#check-database-state
[Check database state with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#check-database-state

[Workload and concurrency management]: ./sql-data-warehouse-develop-concurrency.md
[Table design overview]: ./sql-data-warehouse-tables-overview.md
[Table distribution]: ./sql-data-warehouse-tables-distribute.md
[Table indexing]: ./sql-data-warehouse-tables-index.md
[Table partitioning]: ./sql-data-warehouse-tables-partition.md
[Table statistics]: ./sql-data-warehouse-tables-statistics.md
[Best practices]: ./sql-data-warehouse-best-practices.md
[development overview]: ./sql-data-warehouse-overview-develop.md

[SQL DB Contributor]: ../active-directory/role-based-access-built-in-roles.md#sql-db-contributor

<!--MSDN references-->
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx

<!--Other Web references-->
[Azure portal]: http://portal.azure.com/
