---
title: "aaaImport adatok Azure Search szolgáltatásba történő hello portálon |} Microsoft Docs"
description: "Használja a hello Azure keresési adatok importálása varázslót a hello Azure Portal toocrawl az Azure data nosql-alapú Azure Cosmos DB Blob tároló, a table storage, SQL Database és SQL Server Azure virtuális gépeken."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: Azure Portal
ms.assetid: f40fe07a-0536-485d-8dfa-8226eb72e2cd
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: heidist
ms.openlocfilehash: 00b0e59594560c0cdaea748df196518e9fba3834
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="import-data-tooazure-search-using-hello-portal"></a>Importálja az adatokat tooAzure keresési hello portál használatával
hello Azure-portált biztosít egy **adatimportálás** varázsló hello Azure Search Irányítópulton az adatok betöltését az index. 

  ![Adatokat importálhat hello parancssávon][1]

Belsőleg, hello varázsló konfigurálja, és elindítja egy *indexelő*, automatizálása a folyamat indexelő hello számos lépést: 

* Csatlakozás külső adatforrás tooan hello azonos Azure-előfizetés
* Módosíthatóvá index sémát hello forrás adatszerkezet alapján létre
* JSON-dokumentumok betöltése a hello adatforrás lekért sorkészlet használatával

Az Azure Cosmos DB-adatbázisból vett mintaadatokkal kipróbálhatja ezt a munkafolyamatot. Látogasson el [bevezetés hello Azure Portal az Azure Search használatába](search-get-started-portal.md) utasításokat.

> [!NOTE]
> Indítja el a hello **adatimportálás** hello Azure Cosmos DB irányítópult toosimplify az adott adatforrás indexelő a varázslót. A bal oldali navigációs sávon, váltson túl**gyűjtemények** > **hozzáadása Azure Search** tooget elindult.

## <a name="data-sources-supported-by-hello-import-data-wizard"></a>Adatok importálása varázsló hello által támogatott adatforrások
hello adatok importálása varázsló a következő adatforrások hello támogatja: 

* Azure SQL Database
* Az SQL Server relációs adatai egy Azure virtuális gépen
* Azure Cosmos DB
* Azure Blob Storage
* Azure Table Storage

Egybesimított adatkészlet a kötelező bemenet. Az importálás kizárólag egyedi táblából, adatbázisnézetből és egyenértékű adatszerkezetből végezhető el. Ezen adatok szerkezete hello varázsló futtatása előtt kell létrehozni.

## <a name="connect-tooyour-data"></a>Csatlakozás tooyour adatok
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) és a nyitott hello szolgáltatás irányítópultját. Kattinthat **további szolgáltatások** a hello Ugrás sávon toosearch meglévő "keresési szolgáltatások" hello az aktuális előfizetésben. 
2. Kattintson a **és adatokat importálhat** a hello parancssávon tooslide nyitott hello adatokat importálhat panelen.  
3. Kattintson a **tooyour adatok csatlakozás** toospecify egy indexelőt által használt adatforrás-definíciót. Hello varázsló intra-előfizetés adatforrások, általában észleli és olvasása kapcsolat adatainak minimalizálja a teljes konfiguráció.

|  |  |
| --- | --- |
| **Meglévő adatforrás** |Ha a Search-szolgáltatásban már meg vannak adva indexelők, másik importálási folyamathoz kiválaszthat egy meglévő adatforrás-definíciót. |
| **Azure SQL Database** |Szolgáltatás nevét, egy adatbázis-felhasználó olvasási engedéllyel rendelkező hitelesítő adatait, és az adatbázis nevét hello lapon vagy az ADO.NET kapcsolati karakterlánc használatával adható meg. Válassza ki a hello kapcsolati karakterlánc beállítást tooview vagy tulajdonságainak testreszabása. <br/><br/>hello tábla vagy nézet, hello sorhalmaz biztosító hello oldalon meg kell adni. Ez a beállítás hello csatlakozás sikeres, egyúttal jogosultságot ad a legördülő listából válassza ki, így biztosíthatja, hogy a kijelölt jelenik meg. |
| **Azure virtuális gépen futó SQL Server** |Adjon meg egy teljesen minősített szolgáltatásnevet, felhasználóazonosítót és jelszót, illetve egy adatbázist kapcsolati karakterláncként. toouse ezt az adatforrást kell korábban telepített tanúsítvány hello hello kapcsolat titkosítja a helyi tárolóban levő. Útmutatásért lásd: [SQL virtuális gép kapcsolat tooAzure keresési](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md). <br/><br/>hello tábla vagy nézet, hello sorhalmaz biztosító hello oldalon meg kell adni. Ez a beállítás hello csatlakozás sikeres, egyúttal jogosultságot ad a legördülő listából válassza ki, így biztosíthatja, hogy a kijelölt jelenik meg. |
| **Azure Cosmos DB** |Követelmények a következők: hello fiók, adatbázis és gyűjtemény. Hello gyűjteményben lévő összes dokumentumot hello index fognak szerepelni. Adja meg a lekérdezés tooflatten, vagy hello sorkészlet szűrésére vagy toodetect módosítása a dokumentumok későbbi frissítési műveletek. |
| **Azure Blob Storage** |Követelmények a következők: hello tárfiók és a tároló. Másik lehetőségként blob nevének egy virtuális elnevezési konvenciója csoportosítás céljából, hello neve hello virtuális könyvtár része a tárolóban mappaként is megadhat. A további információkat [a Blob Storage indexelését](search-howto-indexing-azure-blob-storage.md) ismertető témakör tartalmazza. |
| **Azure Table Storage** |Követelmények hello storage-fiókot és egy tábla nevét tartalmazza. Szükség esetén a lekérdezés tooretrieve hello táblák egy részét is megadhat. A további információkat [a Table Storage indexelését](search-howto-indexing-azure-tables.md) ismertető témakör tartalmazza. |

## <a name="customize-target-index"></a>Célindex testreszabása
Előzetes index általában következtetni hello adatkészletből. Hozzáadása, szerkesztése vagy törlése a mezők toocomplete hello séma. Emellett attribútumainak beállítása hello mező szintű toodetermine, a következő keresési viselkedések.

1. A **testreszabás cél index**, hello nevét adja meg, és egy **kulcs** használt toouniquely azonosíthatja minden dokumentumot. hello kulcs karakterláncnak kell lennie. Ha a mezőértékek szóközökkel vagy kötőjelekkel lehet, hogy tooset további beállítások **importálhatja az adatokat** toosuppress hello ezek a karakterek érvényességi ellenőrzését.
2. Tekintse át, és vizsgálja felül a fennmaradó mezők hello. A mező neve és típusa általában már ki van töltve. Hello adattípus módosítható, amíg hello index jön létre. Ha ezt követően szeretné módosítani, újra kell építeni az indexet.
3. Állítsa be az egyes mezőkhöz tartozó indexattribútumokat:
   
   * Lekérhető hello mező a keresési eredmények között adja vissza.
   * Szűrhető lehetővé teszi, hogy hello mező toobe szűrőkifejezéseket hivatkozik.
   * Rendezhető lehetővé teszi, hogy a hello mező toobe egy rendezési szerepel.
   * Kategorizálható lehetővé teszi, hogy a jellemzőalapú navigáció hello mezője.
   * A kereshető attribútum lehetővé teszi a teljes szöveges keresést.
4. Kattintson a hello **Analyzer** lapon, ha azt szeretné, hogy a nyelvi elemzőt hello mező szinten toospecify. Ekkor csak a nyelvi elemzőket adhatja meg. Egyéni elemző vagy nem nyelvi elemző, például kulcsszó, minta és hasonló elemek használata esetén szükség van a kódra.
   
   * Kattintson a **kereshető** toodesignate teljes szöveges keresés hello mező, és engedélyezze a hello Analyzer legördülő lista.
   * Válassza ki a kívánt hello analyzer. Részletes információk: [Több nyelven elérhető dokumentumok indexének létrehozása](search-language-support.md).
5. Kattintson a hello **javaslattevő** tooenable begépelt lekérdezési javaslatok a kijelölt mezőket.

## <a name="import-your-data"></a>Adatok importálása
1. A **importálhatja az adatokat**, adja meg hello indexelő nevét. Hello termék visszahívása hello adatok importálása varázsló az indexelőt. Később Ha szeretné, hogy tooview, vagy szerkesztheti, szüksége lesz rá hello portálról, nem pedig hello varázsló ismételt futtatásával. 
2. Adja meg a hello ütemezését, hello regionális időzóna, amelyben hello szolgáltatás ki van építve alapján.
3. Speciális beállítások toospecify küszöbértékek meg e indexelő továbbra is egy dokumentum megszakad. Megadhatja, hogy **kulcs** mezők toocontain szóközöket és perjeleket tartalmazó engedélyezettek.  
4. Kattintson a **OK** toocreate hello index és hello adatok importálása.

Hello portálon indexelő figyelheti. Dokumentumok töltődnek be, mert hello dokumentum száma nő definiált hello index. Egyes esetekben hello portállapon toopick hello legújabb frissítéseket fel néhány percet vesz igénybe.

hello indexe készen tooquery, amint hello dokumentumokat be van töltve.

## <a name="query-an-index-using-search-explorer"></a>Index lekérdezése a Keresési ablak használatával

hello portál tartalmaz **keresési ablak** , hogy az index lekérdezése az anélkül, hogy toowrite összes kódot. A [Keresési ablak](search-explorer.md) bármely indexen használható.

hello keresési élményt biztosít a alapul alapértelmezett beállítások, például hello [egyszerű Szintaxishiba](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) és az alapértelmezett [searchMode lekérdezési paramétert (https://docs.microsoft.com/rest/api/searchservice/search-documents). 

A JSON-ban, az eredmények visszaadása a részletes formátumban, akkor történik, úgy, hogy a teljes dokumentum hello vizsgálhatja meg.

## <a name="edit-an-existing-indexer"></a>Meglévő indexelő szerkesztése
Amint hello adatok importálása varázsló létrehoz egy **indexelő**, amely egy önálló szerkezet hello portálon módosíthatja.

Hello szolgáltatás irányítópultján kattintson duplán a hello indexelő csempe tooslide ki az előfizetéssel létrehozott összes indexelők listájának. Kattintson duplán a hello indexelők toorun, szerkesztheti, és törölheti azt. Hello index cserélje le egy másik meglévőt, hello adatforrás módosítani, ezért vonatkozó beállítások megadása a Hibaküszöb indexelés során.

## <a name="edit-an-existing-index"></a>Meglévő index szerkesztése
hello varázsló is létrejön egy **index**. Az Azure Search strukturális frissítések tooan index szükséges egy adott index rebuild. Egy rebuild módosított sémával rendelkező hello módosításokat, és az adatok újbóli betöltése hello index újbóli létrehozása, törlése hello index terjed ki. A strukturális frissítések közé tartozik az adattípusok módosítása és a mezők átnevezése vagy törlése.

Az újjáépítést nem igénylő szerkesztési műveletek közé tartozik az új mezők hozzáadása, illetve a pontozási profilok, a javaslatok vagy a nyelvelemzők módosítása. További információ: [Update Index](https://msdn.microsoft.com/library/azure/dn800964.aspx) (Index frissítése).


## <a name="next-steps"></a>Következő lépések
Tekintse át az alábbi hivatkozások toolearn indexelők kapcsolatos további:

* [Az Azure SQL Database indexelése](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Az Azure Cosmos DB indexelése](search-howto-index-documentdb.md)
* [A Blob Storage indexelése](search-howto-indexing-azure-blob-storage.md)
* [A Table Storage indexelése](search-howto-indexing-azure-tables.md)

<!--Image references-->
[1]: ./media/search-import-data-portal/search-import-data-command.png

