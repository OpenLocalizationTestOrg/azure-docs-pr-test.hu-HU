---
title: "aaaManage előzményadatokat az ideiglenes táblák megőrzési házirend |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse historikus megőrzési házirend tookeep előzményadatokat az ellenőrzése alatt."
services: sql-database
documentationcenter: 
author: bonova
manager: drasumic
editor: 
ms.assetid: 76cfa06a-e758-453e-942c-9f1ed6a38c2a
ms.service: sql-database
ms.custom: develop databases
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: sql-database
ms.date: 10/12/2016
ms.author: bonova
ms.openlocfilehash: a72a6111a6cd7322d734d08bf3852e95f5ffea8b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-historical-data-in-temporal-tables-with-retention-policy"></a>Előzményadatokat az ideiglenes táblák megőrzési házirend kezelése
A historikus táblák növelheti reguláris táblák nagyobb adatbázisméret, különösen akkor, ha egy hosszabb ideig a korábbi adatok megőrzése mellett. Emiatt adatmegőrzési korábbi adatok tervezési és minden historikus tábla hello kezeléséért fontos eleme. Az Azure SQL Database ideiglenes táblák megőrzési könnyen kezelhető mechanizmust, amely segít ennek a feladatnak rendelkeznek.

Hello egyedi tábla szintje, amely lehetővé teszi a felhasználóknak rugalmas korosodási toocreate házirendek historikus előzménytáblákra megőrzési is be kell állítani. Egyszerű historikus megőrzési alkalmazása: tábla létrehozása vagy séma módosítása során csak egy paraméter toobe van szükség.

Adatmegőrzési házirend meghatározása után az Azure SQL Database elindul, rendszeresen ellenőrzése, hogy van-e korábbi azon sorait, amelyek jogosultak a automatikus adatok törlése. Egyező sorok azonosítása és hello előzménytábla számított transzparens módon, fordul elő, amely ütemezett és hello rendszer futtathatja hello háttérfeladat. Hello előzmények táblázat sorait kora feltételét SYSTEM_TIME időszak végét jelölő hello oszlop alapján a rendszer ellenőrzi. Ha a megőrzési időtartam, például értéke toosix hónap, a tisztítás jogosult tábla sorainak kielégítéséhez hello feltétel a következő:

````
ValidTo < DATEADD (MONTH, -6, SYSUTCDATETIME())
````

Példa megelőző hello, azt feltételezik, hogy **ValidTo** oszlop felel meg a SYSTEM_TIME időszak vége toohello.

## <a name="how-tooconfigure-retention-policy"></a>Hogyan tooconfigure adatmegőrzési?
Adatmegőrzési historikus táblához tartozó konfigurálása előtt először ellenőrizze, hogy engedélyezett-e a historikus előzmények megőrzési *hello adatbázis szintjén*.

````
SELECT is_temporal_history_retention_enabled, name
FROM sys.databases
````

Adatbázis-jelző **is_temporal_history_retention_enabled** set tooON alapértelmezés szerint, de a felhasználók módosíthatják az ALTER DATABASE utasítással. Egyúttal automatikusan set tooOFF után [időponthoz kötött visszaállítás](sql-database-recovery-using-backups.md) műveletet. tooenable historikus előzményeinek megőrzési eltávolítása az adatbázis, a következő utasítás hello hajtható végre:

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

> [!IMPORTANT]
> Beállíthatja, hogy ideiglenes táblák még akkor is, ha a megőrzési **is_temporal_history_retention_enabled** értéke OFF, de az automatikus tisztítás elavult sorok nem indulnak el ebben az esetben.
> 
> 

Hello HISTORY_RETENTION_PERIOD paraméter megadásával tábla létrehozása során van konfigurálva adatmegőrzési szabály:

````
CREATE TABLE dbo.WebsiteUserInfo
(  
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )  
 WITH
 (
     SYSTEM_VERSIONING = ON
     (
        HISTORY_TABLE = dbo.WebsiteUserInfoHistory,
        HISTORY_RETENTION_PERIOD = 6 MONTHS
     )
 );
````

Az Azure SQL Database lehetővé teszi toospecify megőrzési időszak különböző időegységek használatával: nap, hét, hónap és év. Ha nincs megadva HISTORY_RETENTION_PERIOD, végtelen adatmegőrzési feltételezi. VÉGTELEN kulcsszó explicit módon is használható.

Bizonyos esetekben érdemes tooconfigure megőrzési tábla létrehozása után, vagy toochange korábban megadott érték. Ebben az esetben használja az ALTER TABLE utasítást:

````
ALTER TABLE dbo.WebsiteUserInfo
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 9 MONTHS));
````

> [!IMPORTANT]
> Beállítás a SYSTEM_VERSIONING tooOFF *nem őrzi meg a* megőrzési idő értékét. Beállítás nélkül HISTORY_RETENTION_PERIOD SYSTEM_VERSIONING tooON megadott explicit módon eredményez hello végtelen adatmegőrzési idővel.
> 
> 

tooreview hello megőrzési házirend aktuális állapotát, használja hello következő lekérdezés adott illesztések historikus megőrzési engedélyezése jelzőt az egyes táblák megőrzési időtartamát hello adatbázis szintjén:

````
SELECT DB.is_temporal_history_retention_enabled,
SCHEMA_NAME(T1.schema_id) AS TemporalTableSchema,
T1.name as TemporalTableName,  SCHEMA_NAME(T2.schema_id) AS HistoryTableSchema,
T2.name as HistoryTableName,T1.history_retention_period,
T1.history_retention_period_unit_desc
FROM sys.tables T1  
OUTER APPLY (select is_temporal_history_retention_enabled from sys.databases
where name = DB_NAME()) AS DB
LEFT JOIN sys.tables T2   
ON T1.history_table_id = T2.object_id WHERE T1.temporal_type = 2
````


## <a name="how-sql-database-deletes-aged-rows"></a>Hogyan SQL-adatbázis törli az elavult sorok?
hello kitakarítási folyamatot hello index elrendezését hello előzménytábla függ. Fontos toonotice, amely *csak egy fürtözött index, (B-fa vagy oszlopcentrikus) előzmények táblákban lehet konfigurált véges adatmegőrzési*. Háttérfeladat tooperform azokat az elavult adatok törlése az összes ideiglenes tábláknál véges megőrzési időszak hozza létre.
Hello sortárindex (B-fa) fürtözött index törlése programot törli az elavult sorában kisebb csoportjai (felfelé too10K) minimalizálja a terhelés adatbázis naplója és i/o-alrendszer. Habár a tisztítás logika használja szükséges a B-fa index, régebbi, mint a megőrzési időtartam nem garantálható erősen hello sorok törlések sorrendjét. Emiatt *hello tisztítás rendelés az alkalmazások bármely függőség nem hajtja végre*.

hello hello fürtözött oszlopcentrikus a karbantartási feladat eltávolítja teljes [csoportok sor](https://msdn.microsoft.com/library/gg492088.aspx) egyszerre (általában tartalmaz minden sorok 1 millió), ez nagyon hatékony, különösen akkor, ha előzményadatokat magas ütemben jön létre.

![Fürtözött oszlopcentrikus megőrzési](./media/sql-database-temporal-tables-retention-policy/cciretention.png)

Kiváló adattömörítés és hatékony megőrzési tisztítás teszi fürtözött oszlopcentrikus index forgatókönyvek tökéletes választás, ha a gyors munkaterhelése nagy mennyiségű előzményadatok. Minta része jellemzően intenzív [ideiglenes táblák használó munkaterhelések tranzakciós feldolgozást](https://msdn.microsoft.com/library/mt631669.aspx) a változások követése és naplózása, trendelemzés vagy IoT adatfeldolgozást.

## <a name="index-considerations"></a>Index kapcsolatos szempontok
hello karbantartási feladat sortárindex fürtözött indexszel rendelkező táblák hello oszlop megfelelő hello SYSTEM_TIME időszak végén az index toostart igényel. Ha ilyen index nem létezik, a véges megőrzési időtartam nem konfigurálhatja:

*Üzenet 13765, szint 16 állapot 1 <br> </br> véges megőrzési időszak beállítása sikertelen volt a rendszerverzióval ellátott historikus tábla "temporalstagetestdb.dbo.WebsiteUserInfo", mert hello előzménytábla " temporalstagetestdb.dbo.WebsiteUserInfoHistory "nem tartalmazza a szükséges fürtözött indexszel. Érdemes lehet létrehozni a fürtözött oszlopcentrikus vagy B-fa index kezdve hello oszlop SYSTEM_TIME végének megfelelő idő alatt hello előzménytáblán.*

Fontos, hogy már az Azure SQL Database által létrehozott alapértelmezett előzménytábla hello toonotice rendelkezik fürtözött index, amely megfelel az adatmegőrzési. Tooremove meg, hogy az index véges megőrzési idővel rendelkező táblán, ha sikertelen a következő hiba hello:

*Üzenet 13766, szint 16 állapot 1 <br> </br> hello fürtözött index "WebsiteUserInfoHistory.IX_WebsiteUserInfoHistory" nem dobható el, mert használatban van az automatikus tisztítás az elavult adatokat. Ha ezt az indexet kell toodrop, fontolja meg beállítás HISTORY_RETENTION_PERIOD tooINFINITE hello rendszerverzióval ellátott historikus táblán megfelelő.*

Karbantartást hello fürtözött oszlopcentrikus indexet az optimális működik, ha a korábbi sorok szúrja be a növekvő sorrendben (hello végét rendszeridőtartam-oszlop szerint rendezett), amely esetén mindig hello eset hello előzménytábla fel van töltve, kizárólagos módon hello SYSTEM_ hello VERSIONIOING mechanizmus. Hello előzmények tábla soraival nem időtartamoszlop (amely hello eset is lehet, ha meglévő előzményadatokat áttelepítette) végét szerint rendezett, ha célszerű újra létrehozni a fürtözött oszlopcentrikus index felett, hogy megfelelően van rendezve, optimális tooachieve B-fa sortárindex index teljesítmény.

Kerülje a hello előzménytáblán hello véges megőrzési időtartam fürtözött oszloptárindex újraépítését, mert a természetes hello rendszerverzió művelet által előírt hello sorcsoportok rendelési módosíthatja. Ha fürtözött oszloptárindexe toorebuild hello előzménytábla van szüksége, ehhez ismételt létrehozása megfelelő B-fa index, megőrzi az szereplő adatok rendszeres karbantartása szükséges hello rowgroups felett. ugyanezt a megközelítést kell végezni, meglévő táblába rendelkezik fürtözött oszlopindex garantált adatok rendelés nélkül hozunk létre historikus tábla hello:

````
/*Create B-tree ordered by hello end of period column*/
CREATE CLUSTERED INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory (ValidTo)
WITH (DROP_EXISTING = ON);
GO
/*Re-create clustered columnstore index*/
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON);
````

Ha hello előzménytábla hello fürtözött oszlopcentrikus indexszel rendelkező véges megőrzési időszak van konfigurálva, az adott táblához nem hozható létre a további nem fürtözött B-fa indexek:

````
CREATE NONCLUSTERED INDEX IX_WebHistNCI ON WebsiteUserInfoHistory ([UserName])
````

Egy kísérlet tooexecute fent utasítás sikertelen, és hiba a következő hello:

*Üzenet 13772, szint 16 állapot 1 <br> </br> index nem hozható létre fürtözetlen index historikus előzménytábla "WebsiteUserInfoHistory" mert véges megőrzési időtartam és a fürtözött oszlopcentrikus indexszel rendelkezik.*

## <a name="querying-tables-with-retention-policy"></a>Adatmegőrzési házirend-táblákban lekérdezése
Hello historikus tábla összes lekérdezés automatikusan kiszűrhetők korábbi sorok véges adatmegőrzési, tooavoid előre nem látható és inkonzisztens eredményeket, mert azokat az elavult sorok törölheti az hello karbantartási feladat, *időt, majd a bármikor tetszőleges sorrendben*.

hello alábbi képen látható hello lekérdezéstervet egy egyszerű lekérdezést:

````
SELECT * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME ALL;
````

terv további szűrője hello lekérdezési időszak oszlop (ValidTo) tooend hello fürtözött Index vizsgálata operátor (kiemelt) hello előzménytáblán alkalmazza. Ez a példa feltételezi, hogy az egy hónap megőrzési időszak WebsiteUserInfo táblán lett beállítva.

![Megőrzési lekérdezés szűrője](./media/sql-database-temporal-tables-retention-policy/queryexecplanwithretention.png)

Azonban ha előzménytábla közvetlenül lekérdezéséhez jelenhet meg sorokat, amelyek a régebbi, mint a megadott megőrzési időszak, de bármely garancia nélkül a ismételhető lekérdezési eredmények. hello alábbi képen látható hello lekérdezés végrehajtási lekérdezésterv hello előzménytáblán további szűrők alkalmazása nélkül:

![Előzmények megőrzési szűrő nélkül lekérdezése](./media/sql-database-temporal-tables-retention-policy/queryexecplanhistorytable.png)

Támaszkodjon az üzleti logikát olvasási előzménytábla megőrzési időtartamon túl, mivel nem várt vagy inkonzisztens eredményeket kaphat. Azt javasoljuk, hogy használ historikus lekérdezések FOR SYSTEM_TIME záradék historikus táblákban adatok elemzéséhez.

## <a name="point-in-time-restore-considerations"></a>Mutasson a idő visszaállítási szempontjai
Új adatbázis létrehozásakor [visszaállítása a meglévő adatbázis tooa adott pontot időben](sql-database-recovery-using-backups.md), historikus megőrzési hello adatbázis szintjén tiltva van. (**is_temporal_history_retention_enabled** jelző tooOFF beállítva). Ez a funkció lehetővé teszi tooexamine összes korábbi sor program a visszaállítás nem tartania során sorok elavult tooquery kap előtt távolítsa el őket. Használat túl*vizsgálja meg a konfigurált megőrzési időtartamon túl előzményadatokat*.

Tegyük fel például, hogy a historikus tábla rendelkezik-e a megadott időszak egy hónap megőrzési. Az adatbázis Premium szolgáltatási rétegben jött létre, ha lehetővé válik az adatbázis-másolat képes toocreate be újra a múltbeli hello too35 nap hello adatbázis állapotú. Amely hatékonyan lehetővé teszi tooanalyze korábbi sorokat, amelyek be too65 napos hello előzménytábla közvetlen lekérdezésével.

Ha azt szeretné, hogy tooactivate historikus megőrzési karbantartása, futtassa a következő Transact-SQL utasítás után időponthoz kötött visszaállításra hello:

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

## <a name="next-steps"></a>Következő lépések
Hogyan toouse ideiglenes táblák az alkalmazásban kivétel toolearn [Ismerkedés az Azure SQL Database az ideiglenes táblák](sql-database-temporal-tables.md).

Látogasson el Channel 9 toohear egy [valós felhasználói historikus végrehajtása sikeres szövegegység](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) és figyelési egy [historikus bemutató élő](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).

A Historikus táblák kapcsolatos részletes információkért tekintse át [az MSDN dokumentációjában tájékozódhat](https://msdn.microsoft.com/library/dn935015.aspx).

