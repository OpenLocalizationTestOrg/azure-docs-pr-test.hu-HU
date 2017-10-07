---
title: "T-SQL különbségek-áttelepítés – Azure SQL Database aaaResolving |} Microsoft Docs"
description: "Nem teljes mértékben támogatott Transact-SQL-utasítások az Azure SQL Database-ben"
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: c05abd9e-28a7-4c97-9bdf-bc60d08fc92e
ms.service: sql-database
ms.custom: load & move data
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 03/17/2017
ms.author: rickbyh
ms.openlocfilehash: 3852013338bfdc6c7da9d1d879c54781682bc635
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="resolving-transact-sql-differences-during-migration-toosql-database"></a>Eltérések a Transact-SQL feloldása során áttelepítési tooSQL adatbázis   
Ha [az adatbázis áttelepítése](sql-database-cloud-migrate.md) az SQL Server tooAzure SQL Server, azt tapasztalhatja, hogy az adatbázis futtatásához szükséges néhány átalakításának áttelepíthető-e az SQL Server hello előtt. Ez a témakör útmutatást tooassist hajt végre e átalakításának, mind az alapul szolgáló miért okok hello ismertetése a hello átalakításának szükség. toodetect inkompatibilitási problémák, használja a hello [adatok áttelepítési Segéd (DMA-)](https://www.microsoft.com/download/details.aspx?id=53595).

## <a name="overview"></a>Áttekintés
Microsoft SQL Server és az Azure SQL Database is teljes mértékben támogatja a legtöbb Transact-SQL funkciókat használják. Például hello alapvető SQL-összetevők, például a adattípusok, a kezelők, a karakterlánc, a számtani, logikai, és a kurzor funkciók, munkahelyi azonos az SQL Server és SQL-adatbázis. Azonban néhány T-SQL különbségek vannak DDL (adatok-definition language) és a T-SQL-utasítások és lekérdezések csak részben támogatott DML (adatok adatkezelési language) elemek (Ez a témakör későbbi részében tárgyaljuk).

Emellett néhány funkció és szintaxis nem támogatott, mivel az Azure SQL Database tervezett tooisolate függőségek hello főadatbázis és hello operációs rendszer szolgáltatásait. A legtöbb kiszolgálószintű tevékenységek, SQL-adatbázis nem megfelelőek. T-SQL-utasítások és a beállítások nem érhetők el, ha konfigurálja kiszolgálói szintű beállításokat, operációs rendszer összetevőit, vagy adja meg a rendszer-konfigurációs fájlt. Ilyen tulajdonságokkal szükség, ha megfelelő alternatív gyakran érhető el más módon SQL-adatbázis vagy egy másik Azure szolgáltatás vagy szolgáltatás. 

Például magas rendelkezésre állású be van építve a Azure-ra, így mindig a konfigurálás nem szükséges (bár a gyorsabb helyreállítás hello katasztrófa esetén érdemes lehet tooconfigure aktív georeplikáció). Igen a T-SQL-utasítások kapcsolódó tooavailability csoportok nem támogatottak az SQL-adatbázis, és a hello dinamikus felügyeleti nézetek kapcsolódó tooAlways a még nem támogatott.

Támogatott és nem támogatja az SQL-adatbázis hello szolgáltatások listáját lásd: [Azure SQL Database szolgáltatás összehasonlító](sql-database-features.md). Ezen az oldalon hello lista irányelvek és a szolgáltatások témakör kiegészíti, és Transact-SQL-utasítások összpontosít.

## <a name="transact-sql-syntax-statements-with-partial-differences"></a>Részleges eltérésekkel Transact-SQL-szintaxis utasítások
hello mag (dll) DDL-utasításokban érhetők el, de néhány DDL-utasításokban bővítmények kapcsolódó toodisk elhelyezési és nem támogatott funkciókat. 

- Hozzon létre és az ALTER DATABASE utasítás több mint három tucat lehetősége van. hello utasítások fájl elhelyezését, FILESTREAM és csak akkor jutnak érvényre tooSQL Server service broker beállításokat tartalmazza. Ez lehet, hogy nem számít, hogy adatbázisok létrehozására, mielőtt telepíti át, de ha az áttelepítés által létrehozott adatbázisok T-SQL-kódot kell összehasonlítani [CREATE DATABASE (Azure SQL Database)](https://msdn.microsoft.com/library/dn268335.aspx) hello SQL Server használatával: [létrehozása ADATBÁZIS (SQL Server Transact-SQL)](https://msdn.microsoft.com/library/ms176061.aspx) toomake meg arról, hogy minden hello-beállítások használata támogatott. Az Azure SQL Database-adatbázis létrehozása a szolgáltatási cél és rugalmas bővítést beállításokat csak tooSQL adatbázis is rendelkezik.
- hello létrehozása és az ALTER TABLE utasítás közül FileTable, amely az SQL-adatbázis nem használható, mert a FILESTREAM nem támogatott.
- A CREATE vagy ALTER login utasítás használata támogatott, de az SQL-adatbázis nem biztosít minden hello-beállítások. a több hordozható adatbázis SQL-adatbázis bátorítja használatával toomake tartalmazott adatbázis-felhasználók helyett bejelentkezéseket, amikor csak lehetséges. További információkért lásd: [CREATE/ALTER LOGIN](https://msdn.microsoft.com/library/ms189828.aspx) és [szabályozása és az adatbázis elérését](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins).

## <a name="transact-sql-syntax-not-supported-in-sql-database"></a>Nem támogatott Transact-SQL-szintaxis az SQL Database-ben   
Ezenkívül nem támogatott a tooTransact-SQL utasítás kapcsolódó toohello leírt szolgáltatások [Azure SQL Database szolgáltatás összehasonlító](sql-database-features.md), a következő utasítások és a csoportok kimutatások hello, nem támogatottak. Ha az adatbázis toobe át használ hello alábbi funkciók, újra tervezését a T-SQL tooeliminate a T-SQL funkciókat és az utasításokat.

- Rendszerobjektumok rendezése
- Kapcsolatra vonatkozó: végponti utasítások, `ORIGINAL_DB_NAME`. SQL-adatbázis nem támogatja a Windows-hitelesítést, de támogatja a hello hasonló Azure Active Directory-hitelesítés. Bizonyos hitelesítési hello SSMS legújabb verziója szükséges. További információkért lásd: [csatlakozás tooSQL adatbázis vagy az SQL Data Warehouse által használata Azure Active Directory-hitelesítéssel](sql-database-aad-authentication.md).
- Adatbázisközi lekérdezések három vagy négy résznévvel. (A csak olvasható adatbázisközi lekérdezéseket a [rugalmas adatbázis lekérdezése](sql-database-elastic-query-overview.md) funkció támogatja.)
- Tulajdonjog adatbázisközi láncolása, `TRUSTWORTHY` beállítása
- A `DATABASEPROPERTY` helyett használja a `DATABASEPROPERTYEX` utasítást.
- Az `EXECUTE AS LOGIN` helyett használja az 'EXECUTE AS USER' utasítást.
- A bővíthető kulcskezelés kivételével a titkosítás funkció támogatott
- Eseménykezelő: Események, rendszeresemény-értesítéseket, a lekérdezési értesítések
- Helye: szintaxissal kapcsolatos toodatabase helye, méretének és automatikusan kezeli a Microsoft Azure adatbázisfájlok.
- Magas rendelkezésre állású: szintaxissal kapcsolatos toohigh rendelkezésre állását, amelyen keresztül a Microsoft Azure-fiókjához. Ide tartozik a biztonsági mentéshez, visszaállításhoz, Always On funkcióhoz, adatbázis-tükrözéshez, naplóküldéshez, helyreállítási módokhoz kapcsolódó szintaxis.
- Naplófájl olvasó: hello napló olvasó, amely nem található SQL-adatbázis is Szintaxis: leküldéses replikáció, az adatváltozások rögzítése. Az SQL Database a leküldéses replikációs tétel előfizetője lehet.
- Funkciók: `fn_get_sql`, `fn_virtualfilestats`, `fn_virtualservernodes`
- Globális ideiglenes táblák
- Hardver: Szintaxissal kapcsolatos toohardware kapcsolatos kiszolgálóbeállítások: például memória, a munkavégző szál, a Processzor-affinitás, nyomkövetési jelzők. Használjon helyette szolgáltatási szinteket.
- `HAS_DBACCESS`
- `KILL STATS JOB`
- `OPENQUERY`, `OPENROWSET`, `OPENDATASOURCE`, és négyrészes neve
- .NET-keretrendszer: Az SQL Server CLR-integrációt
- Szemantikai keresés
- A kiszolgáló hitelesítő adatai: használata [adatbázishoz kötődő hitelesítő adatok](https://msdn.microsoft.com/library/mt270260.aspx) helyette.
- Kiszolgálószintű elemek: Kiszolgálói szerepkörök, `IS_SRVROLEMEMBER`, `sys.login_token`. A `GRANT`, `REVOKE`, és `DENY` kiszolgálószintű engedélyek nem érhetők el, bár van köztük olyan, amelyet adatbázisszintű engedélyek helyettesítenek. Néhány hasznos kiszolgálószintű dinamikus felügyeleti nézet rendelkezik egyenértékű adatbázisszintű dinamikus felügyeleti nézettel.
- `SET REMOTE_PROC_TRANSACTIONS`
- `SHUTDOWN`
- `sp_addmessage`
- `sp_configure` lehetőségek és `RECONFIGURE`. Egyes lehetőségek elérhetők az [ALTER DATABASE SCOPED CONFIGURATION](https://msdn.microsoft.com/library/mt629158.aspx) (adatbázis-hatókörű konfiguráció változtatása) utasítással.
- `sp_helpuser`
- `sp_migrate_user_to_contained`
- SQL Server Agent: Szintakszist hello SQL Server Agent vagy hello MSDB adatbázis alapul: riasztások, a kezelők, a központi felügyeleti kiszolgálókat. Használjon helyette parancsfájlkezelőt, mint például az Azure PowerShell.
- SQL Server-naplózás: az SQL Database naplózási helyette.
- SQL Server-nyomkövetés
- Nyomkövetési jelzők: néhány elemeket hoztak funkciókapcsoló toocompatibility módok áthelyezték.
- Transact-SQL-hibakeresés
- Eseményindítók: kiszolgálói hatókörű vagy bejelentkezési eseményindítók
- `USE`utasítás: toochange hello környezetben tooa másik adatbázist, ellenőriznie kell egy új kapcsolat toohello új adatbázist.

## <a name="full-transact-sql-reference"></a>A Transact-SQL teljes leírása
A Transact-SQL-szintaxisról és használatáról további információk és példák találhatók az SQL Server Online könyvek [A Transact-SQL leírása (adatbázismotor)](https://msdn.microsoft.com/library/bb510741.aspx) című részében. 

### <a name="about-hello-applies-to-tags"></a>Hello "Érvényes" címkékkel kapcsolatos
hello Transact-SQL referencia témakörök kapcsolódó tooSQL Server verziók 2008 toohello jelen tartalmazza. Hello témakör cím alatt létrejön egy ikon sáv, listázása hello négy SQL Server platformokon és jelző alkalmazhatósági. A rendelkezésre állási csoportok például az SQL Server 2012-ben jelentek meg. A [AVAILABILTY csoport létrehozása](https://msdn.microsoft.com/library/ff878399.aspx) a témakör azt jelzi, hogy hello utasítás érvényes **(2012-től induló) SQL Server**. hello nyilatkozat nem vonatkozik a tooSQL Server 2008, SQL Server 2008 R2, az Azure SQL Database, Azure SQL Data Warehouse vagy Parallel Data warehouse-bA.

Bizonyos esetekben hello általános a témakör tárgya egy termék használható, de termékek közötti különbségek. hello különbségek vannak megjelölt hello témakör megfelelő középpont felett. Bizonyos esetekben hello általános a témakör tárgya egy termék használható, de termékek közötti különbségek. hello különbségek vannak megjelölt hello témakör megfelelő középpont felett. Például a CREATE TRIGGER témakör hello érhető el az SQL-adatbázis. De hello **összes kiszolgáló** kiszolgálói szintű eseményindítók beállításnál azt jelzi, hogy a kiszolgálói szintű eseményindítók az SQL-adatbázis nem használható. Ehelyett használja az adatbázis-szintű eseményindítók.

## <a name="next-steps"></a>Következő lépések

Támogatott és nem támogatja az SQL-adatbázis hello szolgáltatások listáját lásd: [Azure SQL Database szolgáltatás összehasonlító](sql-database-features.md). Ezen az oldalon hello lista irányelvek és a szolgáltatások témakör kiegészíti, és Transact-SQL-utasítások összpontosít.

