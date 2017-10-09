---
title: "aaaSync adatok (előzetes verzió) |} Microsoft Docs"
description: "Ez az áttekintés bemutatja az Azure SQL adatszinkronizálás (előzetes verzió)."
services: sql-database
documentationcenter: 
author: douglaslms
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: load & move data
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: douglasl
ms.openlocfilehash: d5b2bbd6a502ba94dba7fb309a6583d2d95cc1d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sync-data-across-multiple-cloud-and-on-premises-databases-with-sql-data-sync"></a>Az SQL adatszinkronizálás több felhőalapú és helyszíni az adatbázisok közötti szinkronizálja az adatokat

SQL adatszinkronizálás egy olyan szolgáltatás, amely lehetővé teszi több SQL-adatbázisok és SQL Server-példányok kiválasztása kétirányúan hello adatok szinkronizálása az Azure SQL Database épül.

Adatszinkronizálás körül hello fogalma a szinkronizálási csoport alapul. A szinkronizálás csoport, amelyet az toosynchronize adatbázisok csoportja.

A szinkronizálás csoport rendelkezik hello következő tulajdonságai:

-   Hello **szinkronizálási séma** írja le, mely adatok szinkronizálása.

-   Hello **szinkronizálási irány** kétirányú lehet, vagy csak egy irányban áramolhasson. Ez azt jelenti, hogy a szinkronizálás lehet hello *Hub tooMember* vagy *tag tooHub*, vagy mindkettőt.

-   Hello **szinkronizálási intervallum** milyen gyakran szinkronizálásra kerül sor.

-   Hello **ütközés névfeloldási házirend** csoport szintű házirend, amely lehet *Hub wins* vagy *tag wins*.

Adatszinkronizálás egy küllős topológia toosynchronize adatokat használ. Határozhatja meg hello adatbázisok közül az egyik hello csoport szerint hello Hub adatbázis. hello adatbázisok hello részeinek tag adatbázisok. Szinkronizálási következik be, csak hello központ és az egyes tagok között.
-   Hello **Hub adatbázis** egy Azure SQL Database kell lennie.
-   Hello **tag adatbázisok** SQL-adatbázisok, a helyszíni SQL Server-adatbázisok vagy Azure virtuális gépeken futó SQL Server-példányokat.
-   Hello **Sync-adatbázis** hello metaadatok és a napló tartalmazza, az adatok szinkronizálása. hello Sync-adatbázis toobe van egy Azure SQL-adatbázis található, hello és hello Hub adatbázis ugyanabban a régióban. hello Sync-adatbázis létrehozott felhasználói és felhasználói tulajdonban lévő.

> [!NOTE]
> Ha egy a helyi adatbázist használ, akkor túl[egy helyi ügynök konfigurálása.](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-get-started-sql-data-sync)

![Adatbázisok között szinkronizálja az adatokat](media/sql-database-sync-data/sync-data-overview.png)

## <a name="when-toouse-data-sync"></a>Amikor toouse adatok szinkronizálása

Adatszinkronizálás hasznos olyan esetekben, amikor adatokat kell tartani toodate be több Azure SQL-adatbázisok vagy SQL Server-adatbázisok között toobe. Az alábbiakban hello fő alkalmazási helyzetei adatszinkronizálás:

-   **Hibrid adatszinkronizálás:** az adatszinkronizálás, így adatokat szinkronizálja a helyszíni adatbázisokat és az Azure SQL-adatbázisok tooenable hibrid alkalmazások között. Ez a funkció toocustomers, akik áthelyezése toohello felhő készül, és mit tooput előfordulhat, hogy konfigurálja az alkalmazásokban az Azure-ban része.

-   **Elosztott alkalmazások:** az sok esetben is előnyös tooseparate különböző terhelésekhez különböző adatbázisok közötti. Például olyan nagy éles adatbázist, de szükség jelentéskészítési toorun vagy analytics munkaterhelés ezeken az adatokon, esetén hasznos toohave egy második adatbázis használatát a terhelést. Ez a megközelítés minimalizálja hello teljesítmény a a termelési számítási feladatok. A két adatbázis szinkronizálva adatszinkronizálás tookeep is használhatja.

-   **Globálisan elosztott alkalmazások:** számos vállalat több régióban, illetve akár több ország kiterjedhetnek. Zárja be az egy régióban adatait legjobb toohave toominimize hálózati késés, tooyou. Az adatszinkronizálás hogy könnyedén képes tárolni adatbázisok szinkronizált hello világ különböző régiókban.

A következő forgatókönyvek hello adatszinkronizálás nem ajánlott:

-   Vészhelyreállítás

-   Olvassa el a skála

-   ETL (OLTP tooOLAP)

-   A helyszíni SQL Server tooAzure SQL-adatbázis áttelepítése

## <a name="how-does-data-sync-work"></a>Hogyan működik az adatszinkronizálás? 

-   **Adatok változásainak követése:** adatszinkronizálás segítségével insert módosításokat követi nyomon, frissítési és törlési eseményindító. hello módosítások tárolja, amely hello felhasználói adatbázis oldali táblához.

-   **Adatok szinkronizálása:** adatszinkronizálás célja egy küllős modellben. hello Hub szinkronizál minden tag külön-külön. Hello Hub módosítások letöltött toohello tag és majd hello tagja változtatásokat feltöltött toohello központ.

-   **Az ütközések feloldása:** adatszinkronizálás ütközések feloldása, két lehetőséget biztosít *Hub wins* vagy *tag wins*.
    -   Ha *Hub wins*, hello központban hello módosítások mindig felülírja a hello tag változásairól.
    -   Ha *tag wins*, hello hello felülírása módosításához hello központban változásairól. Ha egynél több tag, hello végső értéke függ, mely tag szinkronizálásának először.

## <a name="limitations-and-considerations"></a>Korlátozások és megfontolások

### <a name="performance-impact"></a>Teljesítményre gyakorolt hatás
Adatok szinkronizálása által beszúrási, frissítési és törlési eseményindítók tootrack módosításokat. Ügyféloldali táblák hello felhasználói adatbázis változáskövetési hozna létre. E módosítás követési tevékenységek hatással vannak az adatbázisban munkaterhelés. A szolgáltatási rétegben felmérése, és ha szükséges.

### <a name="eventual-consistency"></a>Végleges konzisztencia
Mivel adatszinkronizálás eseményindító-alapú, a tranzakciós konzisztencia nem garantált. A Microsoft biztosítja, hogy minden módosításai felé, és nem Adatszinkronizálás a adatvesztéshez vezethet.

### <a name="unsupported-data-types"></a>Nem támogatott adattípusok

-   A FileStream

-   SQL/CLR UDT

-   XmlSchemaCollection gyűjteményben (XML támogatott)

-   Kurzor esetén Timestamp, Hierarchyid

### <a name="requirements"></a>Követelmények

-   Minden tábla elsődleges kulccsal kell rendelkeznie.

-   Egy táblázat azonosító oszlopot, amely nem hello elsődleges kulcs nem lehet.

-   hello objektumneveknek (adatbázisok, táblákat és oszlopokat) nem tartalmazhat hello nyomtatható karakterek pontot (.), a szögletes zárójel ([]), bal vagy jobb szögletes zárójel (]).

### <a name="limitations-on-service-and-database-dimensions"></a>A szolgáltatás és az adatbázis-dimenziókra korlátozásai

|                                                                 |                        |                             |
|-----------------------------------------------------------------|------------------------|-----------------------------|
| **Dimenziók**                                                      | **Korlát**              | **Megkerülő megoldás**              |
| Szinkronizálási csoportok maximális száma bármely adatbázis is tartozik.       | 5                      |                             |
| Több végpont már egy szinkronizálási csoportban              | 30                     | Több szinkronizálási csoportok létrehozása |
| A helyi végpont egy szinkronizálási csoportban maximális száma. | 5                      | Több szinkronizálási csoportok létrehozása |
| Adatbázis, a tábla, a séma és az oszlop neve                       | nevenként 50 karakter hosszú lehet |                             |
| A szinkronizálás csoport táblák                                          | 500                    | Több szinkronizálási csoportok létrehozása |
| A táblázat egy szinkronizálási csoportban                              | 1000                   |                             |
| Adatok sorméret táblán                                        | 24 mb                  |                             |
| Minimális szinkronizálási időköz                                           | 5 perc              |                             |

## <a name="common-questions"></a>Gyakori kérdések

### <a name="how-frequently-can-data-sync-synchronize-my-data"></a>Milyen gyakran adatszinkronizálás adatok bármikor szinkronizálhatók a? 
hello minimális gyakoriság 5 perc értéke.

### <a name="can-i-use-data-sync-toosync-between-sql-server-on-premises-databases-only"></a>Használható adatszinkronizálás toosync csak SQL Server helyszíni adatbázisok között? 
Közvetlenül nem. Szinkronizálhatja a helyszíni SQL Server-adatbázisok közötti közvetve, azonban Hub-adatbázis létrehozása az Azure-ban, és ezután vegye fel a hello helyszíni adatbázisok toohello szinkronizálású csoport.
   
### <a name="can-i-use-data-sync-tooseed-data-from-my-production-database-tooan-empty-database-and-then-keep-them-synchronized"></a>I használhatja adatszinkronizálás tooseed adatok adatbázisból az éles adatbázis tooan üres, és majd láthatóan tartja őket szinkronizálva? 
Igen. Hello séma manuális létrehozása hello új adatbázis alkalmazott parancsprogrammal azt az eredeti hello. Miután létrehozta a hello séma, hello táblák tooa szinkronizálási csoport toocopy hello adatok hozzáadása a, és legyen szinkronizálva.

### <a name="why-do-i-see-tables-that-i-did-not-create"></a>Miért látom azt táblázatot, amely nem hozta létre?  
Adatszinkronizálás ügyféloldali táblák az adatbázisban változáskövetési hoz létre. Ne törölje őket, vagy adatszinkronizálás nem működik.
   
### <a name="i-got-an-error-message-that-said-cannot-insert-hello-value-null-into-hello-column-column-column-does-not-allow-nulls-what-does-this-mean-and-how-can-i-fix-hello-error"></a>Figyelmeztető hibaüzenet jelenik meg, az említett "hello NULL érték nem lehet beszúrni hello oszlop \<oszlop\>. Oszlop nem szerepelhet nulls érték." Ez mit jelent, és hogyan lehet megoldani a problémát hello hiba? 
Ez a hibaüzenet azt jelzi, hello két alábbi problémák egyike:
1.  Előfordulhat, hogy a tábla elsődleges kulcs nélkül. egy elsődleges kulcs tooall hello táblák toofix a probléma hozzáadása folyamatban szinkronizálás.
2.  A CREATE INDEX utasítás WHERE záradék lehet. Szinkronizálási nem kezeli ezt az állapotot. toofix probléma, távolítsa el a WHERE záradék hello, vagy manuálisan módosításokat hello tooall adatbázisok. 
 
### <a name="how-does-data-sync-handle-circular-references-that-is-when-hello-same-data-is-synced-in-multiple-sync-groups-and-keeps-changing-as-a-result"></a>Hogyan adatszinkronizálás körkörös hivatkozásokat kezelni? Ez azt jelenti, hogy mikor hello ugyanazokat az adatokat több szinkronizálási csoportban van-e szinkronizálva, és emiatt tartja módosítása?
Adatszinkronizálás. a körkörös hivatkozások nem tud kezelni. Lehet, hogy tooavoid őket. 

## <a name="next-steps"></a>Következő lépések

SQL adatszinkronizálás kapcsolatos további információkért lásd:

-   [Bevezetés az SQL adatszinkronizálás](sql-database-get-started-sql-data-sync.md)

-   Végezze el a PowerShell példák hogyan tooconfigure SQL-adatok szinkronizálása:
    -   [PowerShell toosync több Azure SQL-adatbázist használja](scripts/sql-database-sync-data-between-sql-databases.md)
    -   [Egy Azure SQL-adatbázis és a helyszíni SQL Server-adatbázisok közötti PowerShell toosync használata](scripts/sql-database-sync-data-between-azure-onprem.md)

-   [Hello teljes adatszinkronizálás SQL műszaki dokumentáció letöltése](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_full_documentation.pdf?raw=true)

-   [Hello SQL adatok szinkronizálása REST API-dokumentáció letöltése](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

SQL-adatbázis kapcsolatos további információkért lásd:

-   [SQL-adatbázis – áttekintés](sql-database-technical-overview.md)

-   [Adatbázis életciklusának kezelésére](https://msdn.microsoft.com/library/jj907294.aspx)
