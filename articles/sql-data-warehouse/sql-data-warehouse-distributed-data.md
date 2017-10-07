---
title: "aaaHow Azure SQL Data Warehouse adatok működik elosztott |} Microsoft Docs"
description: "Ismerje meg, hogyan oszlik meg adatok terjesztéséhez táblák az Azure SQL Data Warehouse és Parallel Data Warehouse nagymértékben párhuzamos feldolgozási (MPP) és hello beállításait."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: bae494a6-7ac5-4c38-8ca3-ab2696c63a9f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 07/12/2017
ms.author: jrj;barbkess
ms.openlocfilehash: 9a712d8d5251e4391ede245105918283aaa4b193
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="distributed-data-and-distributed-tables-for-massively-parallel-processing-mpp"></a>Elosztott adatokon, és elosztott táblák esetében nagymértékben párhuzamos feldolgozási (MPP)
Ismerje meg, hogy a felhasználói adatok hogyan oszlik Azure SQL Data Warehouse és Parallel Data Warehouse, amelyek a Microsoft nagymértékben párhuzamos feldolgozási (MPP) rendszerek. A data warehouse toouse tervezése elosztott adatokon hatékonyan segít, tooachieve hello lekérdezésfeldolgozó hello az MPP architektúra előnyeit. Néhány adatbázis tervezési döntések ütköznek azokkal lekérdezési teljesítmény jelentős hatással lehet.  

> [!NOTE]
> Az SQL Data Warehouse és Parallel Data Warehouse használata hello azonos nagymértékben párhuzamos feldolgozási (MPP) tervezési számára, de néhány különbség miatt hello az alapul szolgáló platform. Az SQL Data Warehouse egy Platform szolgáltatásként (PaaS), amely az Azure-on futtatja. Az Analytics Platform System (AP) amely egy helyszíni készüléknek, amelyen a Windows Server párhuzamos Adatraktárhoz fut.
> 
> 

## <a name="what-is-distributed-data"></a>Mi az az elosztott adatokat?
Az SQL Data Warehouse és Parallel Data Warehouse elosztott adatokon toouser hello MPP rendszer között a különböző helyszínein a tárolt adatok hivatkozik. Egyes helyekre funkciókkal, mint egy független tárolási és feldolgozóegység lekérdezések futó hello adatok részét. Elosztott adata alapvető toorunning lekérdezések párhuzamos tooachieve magas a lekérdezések teljesítményét.

toodistribute adatok hello adatraktár minden egyes sorára egy felhasználói tábla elosztott tooone helyet rendeli hozzá.  Táblákon, amelyekhez a kivonat-elosztási módszer, valamint a ciklikus multiplexelési elosztását. elosztási módszer hello hello CREATE TABLE utasítás van megadva. 

## <a name="hash-distributed-tables"></a>Kivonatoló elosztott táblák
hello a következő ábra azt mutatja be, teljes (tábla nem osztott) lekérdezi tárolási módját kivonatoló elosztott táblázatként. Egy determinisztikus függvény minden egyes sorára toobelong tooone terjesztési rendeli hozzá. A hello tábla definíciójában hello oszlopok egyik kijelölt hello terjesztési oszlop. hello kivonatoló funkció hello terjesztési oszlop tooassign hello érték minden egyes sorára tooa terjesztési.

Teljesítménnyel kapcsolatos szempontok terjesztési oszlop, például megkülönböztethetőség adatok döntés vagy hello futtathatnak hello lekérdezéstípusok hello kiválasztásának van.

![Elosztott tábla](media/sql-data-warehouse-distributed-data/hash-distributed-table.png "elosztott tábla")  

* Minden egyes sorára tooone terjesztési tartozik.  
* Egy meghatározott kivonatoló algoritmus minden sor tooone terjesztési rendeli hozzá.  
* hello) eloszlása feladatonként (a tábla sorainak száma hello különböző méretű táblák eredményobjektumokról függően változik.

## <a name="round-robin-distributed-tables"></a>Ciklikus multiplexelés elosztott táblák
Ciklikus multiplexelés elosztott tábla hello sorok soros módon minden sor tooa terjesztési hozzárendelésével osztja el. Gyors tooload adatok egy ciklikus multiplexelés táblába, de előfordulhat, hogy a lekérdezési teljesítmény lassabb.  Általában csatlakoztatása egy ciklikus multiplexelés tábla reshuffling hello sorok tooenable hello lekérdezés tooproduce nem pontos eredményt, és sok időt igényel.

## <a name="distributed-storage-locations-are-called-distributions"></a>Elosztott tárolóhelyek terjesztéseket nevezzük.
Minden egyes elosztott helyének terjesztési neve. A lekérdezés futtatásakor párhuzamosan, minden egyes terjesztési SQL-lekérdezést végez hello adatok részét. SQL-adatraktár SQL-adatbázis toorun hello lekérdezést használja. Párhuzamos adatraktár SQL Server toorun hello lekérdezést használja. Ez osztott architektúra az alapvető tooachieving kibővített párhuzamos számítástechnikai.

### <a name="can-i-view-hello-distributions"></a>Megtekintheti a hello terjesztéseket?
Minden terjesztési terjesztési Azonosítóval rendelkezik, és látható a rendszer nézeteket, amelyek a projektjükhöz tooSQL Data warehouse-bA és párhuzamos Adatraktárhoz. Hello terjesztési azonosító tootroubleshoot lekérdezési teljesítmény és más problémákat is használhatja. Hello rendszernézetek listájáért lásd: hello [MPP rendszernézet](sql-data-warehouse-reference-tsql-statements.md).

## <a name="difference-between-a-distribution-and-a-compute-node"></a>A telepítési és a számítási csomópont közötti különbség
Egy terjesztési hello alapvető egysége, elosztott adattárolás és párhuzamos lekérdezéseket. Azokat a terjesztéseket számítási csomópontok vannak csoportosítva. Minden számítási csomópont egy vagy több felosztás követi nyomon.  

* Analytics Platform System számítási csomópontok hello architektúra és kibővített hardverképességeinek központi összetevője használja. Mindig használ egyes számítási csomópontjain nyolc azokat a terjesztéseket, kivonatoló elosztott táblák hello ábrán látható módon. hello a számítási csomópontok számát, és ezért hello terjesztéseket száma, hello hello alapplatformjaként megvásárlása számítási csomópontok száma határozza meg. Például ha megvásárolja nyolc számítási csomópontokat, kapott 64 terjesztéseket (8 számítási csomópontok x 8 terjesztéseket/csomópont). 
* Az SQL Data Warehouse 60 terjesztéseket rögzített száma és a számítási csomópontok rugalmas számos rendelkezik. az Azure számítási és tárolási erőforrásokat hello számítási csomópontok valósíthatók meg. Számítási csomópontok száma hello szolgáltatás munkaterhelés függően toohello háttér és hello számítási kapacitás (dwu-k) adja meg, ha az adatraktár hello módosíthatja. Amikor megváltoztatja a számítási csomópontok hello száma, azokat a terjesztéseket egyes számítási csomópontjain hello száma is változik. 

### <a name="can-i-view-hello-compute-nodes"></a>Megtekintheti a számítási csomópontok hello?
Minden számítási csomópont van a csomópont-Azonosítót, és látható a rendszer nézeteket, amelyek a projektjükhöz tooSQL Data warehouse-bA és párhuzamos Adatraktárhoz.  Hello számítási csomópont hello node_id oszlop sys.pdw_nodes kezdődő rendszernézetek megkeresésével tekintheti meg. Hello rendszernézetek listájáért lásd: hello [MPP rendszernézet](sql-data-warehouse-reference-tsql-statements.md).

## <a name="Replicated"></a>Replikált táblák
A replikált tábla rendelkezik egy teljes mértékben hello tábla példányát tárolt minden számítási csomóponton. A tábla replikálása eltávolítja hello kell tootransfer join vagy összesítés előtt a számítási csomópontok között. Replikált táblák valósíthatók csak kis táblákkal miatt hello – megnövelt tárhely szükséges toostore hello teljes táblázat minden számítási csomóponton.  

hello alábbi ábrán látható, hogy minden számítási csomópont replikált tábla. Az SQL Data Warehouse hello replikált tábla a teljes másolt tooa terjesztési adatbázis minden számítási csomóponton. Párhuzamos adatraktár hello replikált tábla toohello számítási csomópont hozzárendelt összes lemezeken tárolja.  A lemez stratégia fájlcsoportokat SQL Server segítségével történik.  

![Replikált tábla](media/sql-data-warehouse-distributed-data/replicated-table.png "replikált tábla") 

## <a name="next-steps"></a>Következő lépések
elosztott toouse táblák hatékonyan, lásd: [táblák az SQL Data Warehouse terjesztése](sql-data-warehouse-tables-distribute.md)  

