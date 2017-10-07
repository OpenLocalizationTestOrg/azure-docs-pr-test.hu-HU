---
title: "az Azure SQL Data Warehouse aaaManage adatbázisok |} Microsoft Docs"
description: "SQL Data Warehouse-adatbázisokban kezelésének áttekintése. Felügyeleti eszközök, a dwu-k és kibővített teljesítmény, lekérdezési teljesítményt, a helyes biztonsági házirendek létrehozása, és egy adatbázis visszaállításához adatsérülés akár regionális kimaradás hibaelhárítási tartalmazza."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: 8576fdb3-71fe-4b3b-a4e0-5e8a7f148acf
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: caec6572c4ab395107c3b095adc69a53eae8bd88
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-databases-in-azure-sql-data-warehouse"></a>Az Azure SQL Data Warehouse adatbázisok kezelése
Az SQL Data Warehouse automatizálja az adatbázisok kezelése sok aspektusait. Például csak kell tooadjust és díj ellenében hello jobb szintű számítási erőforrásokat, és hagyja meg az SQL Data Warehouse tooscale teljesítmény munkájuk összes hello kiterjesztése és a méretezés vissza.

Kétségtelenül érdemes toomonitor a munkaterhelés tooidentify a teljesítmény szükséges, valamint a hosszan futó lekérdezések hibaelhárítása. Is szüksége lesz tooperform néhány biztonsági feladatok toomanage engedélyek egyes felhasználókhoz és bejelentkezések.

Ez az Áttekintés ezeket az jellemzőket SQL Data Warehouse felügyeletét tárgyalja.

* Felügyeleti eszközök
* Skála számítási
* Szüneteltetése és folytatása
* Teljesítmény gyakorlati tanácsok
* Lekérdezés figyelése
* Biztonság
* Biztonsági mentés és visszaállítás

## <a name="management-tools"></a>Felügyeleti eszközök
Az SQL Data Warehouse számos eszközök toomanage adatbázissal is használhatja. Adatbázisok kezelése közben, során elkészít-e az eszköz beállítások az egyes feladatok tooperform kell.

### <a name="azure-portal"></a>Azure Portal
Hello [Azure-portálon] [ Azure portal] egy webes portál, ahol létrehozása, frissítése, és törölni az adatbázisokat és adatbázis-erőforrások figyelése. Ez az eszköz kiváló akkor, ha Ön most használatának megkezdéséhez Azure, az adatraktár-adatbázisokat kis számú kezelése vagy kell tooquickly foglalkozhat.

az Azure-portálon hello lépései tooget lásd: [(Azure-portál) az SQL Data Warehouse létrehozása][Create a SQL Data Warehouse (Azure portal)].

### <a name="sql-server-data-tools-in-visual-studio"></a>A Visual Studio SQL Server Data Tools összetevővel
[SQL Server Data Tools] [ SQL Server Data Tools] (SSDT) a Visual Studio lehetővé teszi a kezelése, és az adatbázis tooconnect. Ha ismeri a Visual Studio vagy más integrált fejlesztési környezetekben (IDEs) alkalmazásfejlesztő, próbálkozzon az SSDT a Visual Studióban.

SSDT magában foglalja az SQL Server Object Explorer programban, amely lehetővé teszi toovisualize hello csatlakozás és adatbázisok az SQL Data Warehouse parancsprogramok hajtható végre. tooquickly csatlakozzon az adatraktár tooSQL, egyszerűen kattintson hello **Megnyitás Visual Studio** hello parancssávon, amikor hello adatbázis részletek megtekintése a klasszikus Azure portál hello gombjára.  

a Visual Studióban az SSDT használatába tooget lásd: [lekérdezés Azure SQL Data Warehouse a Visual Studio][Query Azure SQL Data Warehouse with Visual Studio].

### <a name="command-line-tools"></a>Parancssori eszközök
A parancssori eszközök alkalmasak a feladatok automatizálásához.  PowerShell és sqlcmd két nagyszerű lehetőséget tooautomate a folyamatok.  Azt javasoljuk, hogy ezek az eszközök nagy számú logikai kiszolgáló kezelése és központi telepítése éles környezetben az erőforrás-változása hello szükséges feladatok parancsprogrammal létrehozva, és majd automatikus.

### <a name="dynamic-management-views"></a>Dinamikus felügyeleti nézetekkel
Dinamikus felügyeleti nézetek hello be és vaj felügyelete az SQL Data Warehouse. Szinte minden olyan információt, amely felfedi a hello portálon dinamikus felügyeleti nézetek támaszkodik. toosee SQL Data Warehouse dinamikus felügyeleti nézetek, listáját lásd: [SQL Data Warehouse rendszernézetek][SQL Data Warehouse system views].

tooget indult el, lásd: [kapcsolódás és lekérdezés az Sqlcmd][Connect and query with sqlcmd], és [hozzon létre egy adatbázist (PowerShell)][Create a database (PowerShell)].

## <a name="scale-compute"></a>Skála számítási
Az SQL Data Warehouse teljesítményét, vagy vissza bővítése vagy csökkentése a számítási erőforrásokat Processzor, memória és i/o műveletek sávszélességétől gyors vertikális. tooscale teljesítmény toodo szüksége adattárházegységek (dwu-k), hogy az SQL Data Warehouse foglal le, mely tooyour adatbázis hello számának beállítása. Az SQL Data Warehouse gyorsan hello módosítása lehetővé teszi, és kezeli az összes hello alapul szolgáló módosítások toohardware vagy szoftver.

toolearn dwu-k, skálázás kapcsolatos további információkért lásd: [méretezhető teljesítmény].

## <a name="pause-and-resume"></a>Szünet és folytatás
toosave költségek, is szüneteltetése és folytatása számítási erőforrások igény. Például ha nem használ hello adatbázis hello éjszakai során, és a hétvégekre, után ilyen alkalmakkor szünetelteti, is fogják folytatni azt hello nap alatt. Ön nem számlázni dwu-k közben hello adatbázis fel van függesztve.

További információkért lásd: [számítási szüneteltetése][Pause compute], és [folytathatja a számítást][Resume compute].

## <a name="performance-best-practices"></a>Teljesítmény gyakorlati tanácsok
Ha az első lépések egy új technológiával, hello tippek és legjobb jobb hello indítás működő trükkök felderítéséhez takaríthat meg rengeteg időt.  Gyakorlati tanácsok a témakörök számos teljes találja.

toosee hello egyik legfontosabb szempontja a számítási feladathoz fejleszt sok összefoglalását lásd: [SQL Data Warehouse gyakorlati tanácsok][SQL Data Warehouse Best Practices].

## <a name="query-monitoring"></a>Lekérdezés figyelése
Néha egy lekérdezése túl hosszú, de még nem meg arról, hogy melyik hello hibát okozó van. Az SQL Data Warehouse (dinamikus felügyeleti nézetek) használható toofigure dinamikus felügyeleti nézetekkel rendelkezik, ahonnan a lekérdezés túl sokáig tart.

toofind hosszan futó lekérdezések bővebb ismertetése a [a dinamikus felügyeleti nézetek használatával számítási feladat figyeléséhez][Monitor your workload using DMVs].

## <a name="security"></a>Biztonság
toomaintain biztonságos rendszer, kell lenniük hello figyelmeztető riasztás, és bármilyen típusú jogosulatlan hozzáférés elleni védelmet. A biztonsági rendszer toomake, a tűzfalszabályok teljesülnek, csak a jogosult IP-címek képesek-e csatlakozni kell. A felhasználói hitelesítő adatok a megfelelő hitelesítés szükséges. Miután egy felhasználó kapcsolódott toohello adatbázis, hello felhasználói csak kell engedélyek tooperform műveletek minimális száma. toosecure adatokat, a titkosítási használhatja. Akkor is vizsgálati és nyomkövetési eseményeket is végig, ha a gyanús tevékenységeket, fontos toohave.

biztonsági okokból toohello keresztül head kezelésével kapcsolatos toolearn [biztonsági áttekintése][Security overview].

## <a name="backup-and-restore"></a>Biztonsági mentés és visszaállítás
Az adatok megbízható backps, akkor minden éles adatbázis nagyon fontos részét képezik. Az SQL Data Warehouse biztosítja az adatok biztonságos automatikusan biztonsági mentést kell készíteni a aktív adatbázisainak rendszeres időközönként. Ezek a biztonsági másolatok teszik lehetővé a forgatókönyvekben, ahol korábban az adatok sérült, vagy véletlenül a az adatok vagy az adatbázis eldobása toorecover.  Az adatmegőrzési hello adatok biztonsági mentés ütemezése és hogyan toorestore, adatbázis: [a pillanatkép-visszaállítás][Restore from snapshot].

## <a name="next-steps"></a>Következő lépések
Jó adatbázis tervezési alapelvek segítségével teszi egyszerűbbé toomanage az SQL Data Warehouse az adatbázisokat. További, toolearn keresztül toohello head [fejlesztői áttekintés][Development overview].

<!--Image references-->

<!--Article references-->
[Create a SQL Data Warehouse (Azure Portal)]: sql-data-warehouse-get-started-provision.md
[Create a database (PowerShell)]: sql-data-warehouse-get-started-provision-powershell.md
[connection]: sql-data-warehouse-develop-connections.md
[Query Azure SQL Data Warehouse with Visual Studio]: sql-data-warehouse-query-visual-studio.md
[Connect and query with sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md
[Development overview]: sql-data-warehouse-overview-develop.md
[Monitor your workload using DMVs]: sql-data-warehouse-manage-monitor.md
[Pause compute]: sql-data-warehouse-manage-compute-overview.md#pause-compute-bk
[Restore from snapshot]: sql-data-warehouse-restore-database-overview.md
[Resume compute]: sql-data-warehouse-manage-compute-overview.md#resume-compute-bk
[méretezhető teljesítmény]: sql-data-warehouse-manage-compute-overview.md#scale-compute
[Security overview]: sql-data-warehouse-overview-manage-security.md
[SQL Data Warehouse Best Practices]: sql-data-warehouse-best-practices.md
[SQL Data Warehouse system views]: sql-data-warehouse-reference-tsql-system-views.md

<!--MSDN references-->
[SQL Server Data Tools]: https://msdn.microsoft.com/library/mt204009.aspx

<!--Other web references-->
[Azure portal]: http://portal.azure.com/
