---
title: "aaaGetting Started with Azure SQL Database az ideiglenes táblák |} Microsoft Docs"
description: "Ismerje meg, hogyan tooget ideiglenes táblák használata az Azure SQL Database használatába."
services: sql-database
documentationcenter: 
author: bonova
manager: jhubbard
editor: 
ms.assetid: c8c0f232-0751-4a7f-a36e-67a0b29fa1b8
ms.service: sql-database
ms.custom: develop databases
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: sql-database
ms.date: 01/10/2017
ms.author: bonova
ms.openlocfilehash: 54f394b51df07aa2f9bb299f207e692171d23479
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-temporal-tables-in-azure-sql-database"></a>Ismerkedés az Azure SQL Database ideiglenes táblák
A historikus táblák egy új programozhatóság Azure SQL-adatbázis, amely lehetővé teszi tootrack és hello teljes korábbi módosításait az adatok anélkül hello egyéni kódolási elemzése. Ideiglenes táblák tartsa szorosan kapcsolódó tootime adatkörnyezetet, hogy a tárolt tények értelmezhető szerint érvényes csak hello adott időszakon belül. Ez a tulajdonság a Historikus táblák hatékony időalapú elemzés és beolvasásakor információkat kaphat a adatok alakulása lehetővé teszi.

## <a name="temporal-scenario"></a>A historikus forgatókönyv
Ez a cikk hello lépéseket tooutilize ideiglenes táblák egy alkalmazás-forgatókönyvben mutatja be. Tegyük fel, amelyet az új webhely létrehozása kifejlesztő vagy egy meglévő webhely, amelyet a felhasználói tevékenység analytics tooextend tootrack felhasználói tevékenység. Egyszerűsített példában feltételezzük, hogy hello számos meglátogatott weblapot során egy adott időn belül-e azt jelzi, amelyet a rögzített és a figyelt hello webhely adatbázis, amely az Azure SQL Database toobe. hello cél hello korábbi elemzési a felhasználói tevékenység tooget bemenetek tooredesign webhely, és adjon meg jobb felhasználói élmény hello látogatóinak.

Ebben a forgatókönyvben hello adatbázismodell nagyon egyszerű – felhasználói tevékenység metrika képviselt egyetlen egész mezőt, **PageVisited**, és rögzített hello felhasználói profil az alapszintű információk mellett. Emellett az időpontokat az elemzést követően tartja minden felhasználó esetében sorok sorozatát ahol minden sor egy adott felhasználó egy adott időszakon belül meglátogatott oldalak hello számát jelenti.

![Séma](./media/sql-database-temporal-tables/AzureTemporal1.png)

Szerencsére, nem kell tooput bármely elérhető a az alkalmazás toomaintain a kapcsolatos információk. A Historikus táblák Ez a folyamat automatizált - felkínálva teljesen rugalmasan több idő toofocus hello adatelemzés magát a webhely a tervezés során. csak annyi teendője van, toodo tooensure hello, amely **WebSiteInfo** tábla van konfigurálva, [historikus rendszerverzióval ellátott](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_0). hello szövegéről tooutilize ideiglenes táblák ebben a forgatókönyvben az alábbiakban található.

## <a name="step-1-configure-tables-as-temporal"></a>1. lépés: Konfigurálja táblák historikus
Attól függően, hogy vannak indítása az új fejlesztési vagy meglévő alkalmazás frissítését fog létrehozni az ideiglenes táblák vagy módosíthatja a meglévőket historikus attribútumok hozzáadásával. Általános esetben adott esetben lehet ezek két lehetőségek egy kombinációja. Hajtsa végre ezeket a művelet használatával [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (SSMS) [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) (SSDT) vagy bármilyen más Transact-SQL fejlesztési.

> [!IMPORTANT]
> Javasoljuk, hogy mindig használjon hello Azure és az SQL-adatbázis a frissítések tooMicrosoft szinkronizálva tooremain Management Studio legújabb verzióját. [Az SQL Server Management Studio frissítése](https://msdn.microsoft.com/library/mt238290.aspx).
> 
> 

### <a name="create-new-table"></a>Új tábla létrehozása
Használjon környezetben menüpont "Új rendszerverzióval ellátott tábla" szolgáltatáshoz az SSMS Object Explorer tooopen hello Lekérdezésszerkesztő historikus tábla sablon parancsfájl, és majd a "Adjon meg értékeket a sablon paraméterek" (Ctrl + Shift + M) toopopulate hello sablont használ:

![SSMSNewTable](./media/sql-database-temporal-tables/AzureTemporal2.png)

Az SSDT "(a rendszerverzióval ellátott) Historikus tábla" sablont választ toohello adatbázis új projekt hozzáadása során. Hogy fog tábla megnyitása Tervező és engedélyezése, adja meg, hogy tooeasily hello table elrendezést:

![SSDTNewTable](./media/sql-database-temporal-tables/AzureTemporal3.png)

Is historikus tábla létrehozása hello Transact-SQL utasítás közvetlenül, megadásával a hello az alábbi példában látható módon. Ne feledje, hogy minden historikus tábla hello kötelező elemeit hello időtartam-definíciója és hello SYSTEM_VERSIONING záradékot egy tooanother felhasználói referenciatábla sor korábbi verziók tárolásához:

````
CREATE TABLE WebsiteUserInfo 
(  
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED 
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL 
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )  
 WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.WebsiteUserInfoHistory));
````

A rendszerverzióval ellátott historikus tábla létrehozásakor a rendszer automatikusan létrehoz hello kísérő előzménytábla hello alapértelmezett konfigurációval. hello alapértelmezett előzménytábla hello időtartamoszlopok (kezdő célból) egy fürtözött B-fa index tartalmaz lap tömörítést engedélyezve van. Ez a konfiguráció nincs optimális hello többségének forgatókönyvek, amelyben az ideiglenes táblák használata esetén különösen a [adatok naplózását](https://msdn.microsoft.com/library/mt631669.aspx#Anchor_0). 

Ebben az esetben a Microsoft célja tooperform időalapú trendelemzés keresztül hosszabb adatok előzményeit, és nagyobb adatkészletek esetében a így hello tárolási választás az hello előzménytábla fürtözött oszlopcentrikus index. A fürtözött oszlopcentrikus nagyon jó tömörítési elemzési lekérdezések teljesítményt nyújt. A historikus táblák biztosít rugalmasságot tooconfigure hello aktuális és a historikus táblák indexei teljesen önállóan hello. 

> [!NOTE]
> Oszlopcentrikus indexek csak találhatók hello prémium szolgáltatásszintet.
>

a következő parancsfájl hello jeleníti meg, hogyan előzménytáblán alapértelmezett indexe megváltozott toohello fürtözött oszlopcentrikus lehet:

````
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
````

A historikus táblák hello Object Explorer hello adott ikon könnyebb azonosításához jelennek meg a előzménytábla gyermekcsomópontja megjelenítése közben.

![AlterTable](./media/sql-database-temporal-tables/AzureTemporal4.png)

### <a name="alter-existing-table-tootemporal"></a>Meglévő tábla tootemporal ALTER
Most borítóján hello alternatív forgatókönyv mely hello WebsiteUserInfo tábla már létezik, de nem tervezett tookeep volt egy korábbi módosításait. Ebben az esetben egyszerűen kiterjesztheti hello meglévő tábla toobecome historikus hello a következő példa látható:

````
ALTER TABLE WebsiteUserInfo 
ADD 
    ValidFrom datetime2 (0) GENERATED ALWAYS AS ROW START HIDDEN  
        constraint DF_ValidFrom DEFAULT DATEADD(SECOND, -1, SYSUTCDATETIME())
    , ValidTo datetime2 (0)  GENERATED ALWAYS AS ROW END HIDDEN   
        constraint DF_ValidTo DEFAULT '9999.12.31 23:59:59.99'
    , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo); 

ALTER TABLE WebsiteUserInfo  
SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.WebsiteUserInfoHistory));
GO

CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
````

## <a name="step-2-run-your-workload-regularly"></a>2. lépés: A számítási feladatok rendszeresen futtatása
hello fő ideiglenes táblák előnye, hogy nem kell toochange, vagy állítsa be a webhely az bármely módon tooperform változáskövetést. Létrehozása után a Historikus táblák transzparens módon továbbra is fennáll korábbi sor minden alkalommal, amikor módosításokat végezni az adatokon. 

A sorrend tooleverage automatikus változáskövetése ebben a forgatókönyvben, most csak frissítse oszlop **PagesVisited** minden alkalommal, amikor a felhasználó megszűnik munkamenet hello webhelyen befejezése:

````
UPDATE WebsiteUserInfo  SET [PagesVisited] = 5 
WHERE [UserID] = 1;
````

Fontos, hogy a frissítés lekérdezés hello toonotice nem szükséges tooknow hello pontos idő, hello tényleges művelet történt, sem hogyan előzményadatait megőrzi további elemzés céljából. Mindkét szempont hello Azure SQL Database automatikusan kezeli. hello következő ábra bemutatja hogyan előzményadatok minden frissítési folyamatban.

![TemporalArchitecture](./media/sql-database-temporal-tables/AzureTemporal5.png)

## <a name="step-3-perform-historical-data-analysis"></a>3. lépés: Az előzményadatok elemzés végrehajtása
Most historikus rendszerverzió-engedélyezésekor a rendszer előzményadatokat elemzésre másik lapra, hogy csak egy lekérdezést. Ebben a cikkben azt fogja adja meg a címet elemzés szabhatják - toolearn néhány példa minden adatait, megismerkedhet a különböző lehetőségek hello bevezetett [FOR SYSTEM_TIME](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_3) záradékban.

toosee hello felső 10 olyan felhasználót egy órával ezelőtt történt, frissítésétől megtekintett weblapok hello száma alapján rendezve a lekérdezés futtatására:

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
SELECT TOP 10 * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME AS OF @hourAgo
ORDER BY PagesVisited DESC
````

Könnyen módosíthatja a lekérdezés egy, a következő időpontban napján érvényes meglátogat tooanalyze hello hely egy hónapja vagy korábbi hello bármely pontján kívánja.

alapvető statisztikai elemzések tooperform hello előző nap, a következő példa hello használata:

````
DECLARE @twoDaysAgo datetime2 = DATEADD(DAY, -2, SYSUTCDATETIME());
DECLARE @aDayAgo datetime2 = DATEADD(DAY, -1, SYSUTCDATETIME());

SELECT UserID, SUM (PagesVisited) as TotalVisitedPages, AVG (PagesVisited) as AverageVisitedPages,
MAX (PagesVisited) AS MaxVisitedPages, MIN (PagesVisited) AS MinVisitedPages,
STDEV (PagesVisited) as StDevViistedPages
FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME BETWEEN @twoDaysAgo AND @aDayAgo
GROUP BY UserId
````

a tevékenységek egy adott felhasználó belül egy adott időn belül, használjon hello TARTALMAZOTT IN záradék toosearch:

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
DECLARE @twoHoursAgo datetime2 = DATEADD(HOUR, -2, SYSUTCDATETIME());
SELECT * FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME CONTAINED IN (@twoHoursAgo, @hourAgo)
WHERE [UserID] = 1;
````

Grafikus képi megjelenítés lehetőség időalapú lekérdezéseknél különösen akkor hasznos, mivel Ön lehet megjeleníteni a trendek és használati szokások egy egyszerűen elsajátítható módja nagyon egyszerűen:

![TemporalGraph](./media/sql-database-temporal-tables/AzureTemporal6.png)

## <a name="evolving-table-schema"></a>Folyamatosan változó a következő tábla sémáját
Általában akkor toochange hello historikus tábla sémáját használata közben is alkalmazások fejlesztéséhez. Ehhez egyszerűen futtassa a rendszeres az ALTER TABLE utasítások, és az Azure SQL Database megfelelő továbbterjeszti majd a változtatások toohello előzménytábla. hello alábbi parancsfájl bemutatja, hogyan adhat meg további attribútum figyelését:

````
/*Add new column for tracking source IP address*/
ALTER TABLE dbo.WebsiteUserInfo 
ADD  [IPAddress] varchar(128) NOT NULL CONSTRAINT DF_Address DEFAULT 'N/A';
````

Módosíthatja hasonlóan oszlop definíciójában, amíg aktív a számítási feladatok:

````
/*Increase hello length of name column*/
ALTER TABLE dbo.WebsiteUserInfo 
    ALTER COLUMN  UserName nvarchar(256) NOT NULL;
````

Végezetül is eltávolíthat egy oszlopot, amely már nem szükséges.

````
/*Drop unnecessary column */
ALTER TABLE dbo.WebsiteUserInfo 
    DROP COLUMN TemporaryColumn; 
````

Másik megoldásként használhatja a legújabb [SSDT](https://msdn.microsoft.com/library/mt204009.aspx) toochange historikus tábla séma csatlakoztatott toohello adatbázis (online mód) vagy hello adatbázis projekt (kapcsolat nélküli módban) részeként.

## <a name="controlling-retention-of-historical-data"></a>Korábbi adatok megőrzése vezérlése
A rendszerverzióval ellátott historikus táblákon hello előzménytábla megnőhet reguláris táblák nagyobb hello adatbázis mérete. Egy nagy- és egyre előzménytábla válhat a lejárat toopure tárolási költségek, valamint a teljesítmény előíró adó historikus lekérdezése hibát. Emiatt az adatmegőrzési házirend hello tábla adatok kezelésére szolgáló fejlesztése tervezési és minden historikus tábla hello kezeléséért fontos eleme. Az Azure SQL Database a következő előzményadatokat hello historikus tábla kezelésére szolgáló módszerek hello közül választhat:

* [Táblaparticionálást](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_2)
* [Egyéni karbantartási parancsprogramot](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_3)

## <a name="next-steps"></a>Következő lépések
A Historikus táblák részletes információkért tekintse meg [az MSDN dokumentációjában tájékozódhat](https://msdn.microsoft.com/library/dn935015.aspx).
Látogasson el Channel 9 toohear egy [valós felhasználói historikus implementációs sikeres szövegegység](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) és figyelési egy [historikus bemutató élő](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).

