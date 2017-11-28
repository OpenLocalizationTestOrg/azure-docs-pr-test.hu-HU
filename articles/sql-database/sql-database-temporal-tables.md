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
# <a name="getting-started-with-temporal-tables-in-azure-sql-database"></a><span data-ttu-id="7e173-103">Ismerkedés az Azure SQL Database ideiglenes táblák</span><span class="sxs-lookup"><span data-stu-id="7e173-103">Getting Started with Temporal Tables in Azure SQL Database</span></span>
<span data-ttu-id="7e173-104">A historikus táblák egy új programozhatóság Azure SQL-adatbázis, amely lehetővé teszi tootrack és hello teljes korábbi módosításait az adatok anélkül hello egyéni kódolási elemzése.</span><span class="sxs-lookup"><span data-stu-id="7e173-104">Temporal Tables are a new programmability feature of Azure SQL Database that allows you tootrack and analyze hello full history of changes in your data, without hello need for custom coding.</span></span> <span data-ttu-id="7e173-105">Ideiglenes táblák tartsa szorosan kapcsolódó tootime adatkörnyezetet, hogy a tárolt tények értelmezhető szerint érvényes csak hello adott időszakon belül.</span><span class="sxs-lookup"><span data-stu-id="7e173-105">Temporal Tables keep data closely related tootime context so that stored facts can be interpreted as valid only within hello specific period.</span></span> <span data-ttu-id="7e173-106">Ez a tulajdonság a Historikus táblák hatékony időalapú elemzés és beolvasásakor információkat kaphat a adatok alakulása lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="7e173-106">This property of Temporal Tables allows for efficient time-based analysis and getting insights from data evolution.</span></span>

## <a name="temporal-scenario"></a><span data-ttu-id="7e173-107">A historikus forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="7e173-107">Temporal Scenario</span></span>
<span data-ttu-id="7e173-108">Ez a cikk hello lépéseket tooutilize ideiglenes táblák egy alkalmazás-forgatókönyvben mutatja be.</span><span class="sxs-lookup"><span data-stu-id="7e173-108">This article illustrates hello steps tooutilize Temporal Tables in an application scenario.</span></span> <span data-ttu-id="7e173-109">Tegyük fel, amelyet az új webhely létrehozása kifejlesztő vagy egy meglévő webhely, amelyet a felhasználói tevékenység analytics tooextend tootrack felhasználói tevékenység.</span><span class="sxs-lookup"><span data-stu-id="7e173-109">Suppose that you want tootrack user activity on a new website that is being developed from scratch or on an existing website that you want tooextend with user activity analytics.</span></span> <span data-ttu-id="7e173-110">Egyszerűsített példában feltételezzük, hogy hello számos meglátogatott weblapot során egy adott időn belül-e azt jelzi, amelyet a rögzített és a figyelt hello webhely adatbázis, amely az Azure SQL Database toobe.</span><span class="sxs-lookup"><span data-stu-id="7e173-110">In this simplified example, we assume that hello number of visited web pages during a period of time is an indicator that needs toobe captured and monitored in hello website database that is hosted on Azure SQL Database.</span></span> <span data-ttu-id="7e173-111">hello cél hello korábbi elemzési a felhasználói tevékenység tooget bemenetek tooredesign webhely, és adjon meg jobb felhasználói élmény hello látogatóinak.</span><span class="sxs-lookup"><span data-stu-id="7e173-111">hello goal of hello historical analysis of user activity is tooget inputs tooredesign website and provide better experience for hello visitors.</span></span>

<span data-ttu-id="7e173-112">Ebben a forgatókönyvben hello adatbázismodell nagyon egyszerű – felhasználói tevékenység metrika képviselt egyetlen egész mezőt, **PageVisited**, és rögzített hello felhasználói profil az alapszintű információk mellett.</span><span class="sxs-lookup"><span data-stu-id="7e173-112">hello database model for this scenario is very simple - user activity metric is represented with a single integer field, **PageVisited**, and is captured along with basic information on hello user profile.</span></span> <span data-ttu-id="7e173-113">Emellett az időpontokat az elemzést követően tartja minden felhasználó esetében sorok sorozatát ahol minden sor egy adott felhasználó egy adott időszakon belül meglátogatott oldalak hello számát jelenti.</span><span class="sxs-lookup"><span data-stu-id="7e173-113">Additionally, for time based analysis, you would keep a series of rows for each user, where every row represents hello number of pages a particular user visited within a specific period of time.</span></span>

![Séma](./media/sql-database-temporal-tables/AzureTemporal1.png)

<span data-ttu-id="7e173-115">Szerencsére, nem kell tooput bármely elérhető a az alkalmazás toomaintain a kapcsolatos információk.</span><span class="sxs-lookup"><span data-stu-id="7e173-115">Fortunately, you do not need tooput any effort in your app toomaintain this activity information.</span></span> <span data-ttu-id="7e173-116">A Historikus táblák Ez a folyamat automatizált - felkínálva teljesen rugalmasan több idő toofocus hello adatelemzés magát a webhely a tervezés során.</span><span class="sxs-lookup"><span data-stu-id="7e173-116">With Temporal Tables, this process is automated - giving you full flexibility during website design and more time toofocus on hello data analysis itself.</span></span> <span data-ttu-id="7e173-117">csak annyi teendője van, toodo tooensure hello, amely **WebSiteInfo** tábla van konfigurálva, [historikus rendszerverzióval ellátott](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_0).</span><span class="sxs-lookup"><span data-stu-id="7e173-117">hello only thing you have toodo is tooensure that **WebSiteInfo** table is configured as [temporal system-versioned](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_0).</span></span> <span data-ttu-id="7e173-118">hello szövegéről tooutilize ideiglenes táblák ebben a forgatókönyvben az alábbiakban található.</span><span class="sxs-lookup"><span data-stu-id="7e173-118">hello exact steps tooutilize Temporal Tables in this scenario are described below.</span></span>

## <a name="step-1-configure-tables-as-temporal"></a><span data-ttu-id="7e173-119">1. lépés: Konfigurálja táblák historikus</span><span class="sxs-lookup"><span data-stu-id="7e173-119">Step 1: Configure tables as temporal</span></span>
<span data-ttu-id="7e173-120">Attól függően, hogy vannak indítása az új fejlesztési vagy meglévő alkalmazás frissítését fog létrehozni az ideiglenes táblák vagy módosíthatja a meglévőket historikus attribútumok hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="7e173-120">Depending on whether you are starting new development or upgrading existing application, you will either create temporal tables or modify existing ones by adding temporal attributes.</span></span> <span data-ttu-id="7e173-121">Általános esetben adott esetben lehet ezek két lehetőségek egy kombinációja.</span><span class="sxs-lookup"><span data-stu-id="7e173-121">In general case, your scenario can be a mix of these two options.</span></span> <span data-ttu-id="7e173-122">Hajtsa végre ezeket a művelet használatával [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (SSMS) [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) (SSDT) vagy bármilyen más Transact-SQL fejlesztési.</span><span class="sxs-lookup"><span data-stu-id="7e173-122">Perform these action using [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (SSMS), [SQL Server Data Tools](https://msdn.microsoft.com/library/mt204009.aspx) (SSDT) or any other Transact-SQL development tool.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7e173-123">Javasoljuk, hogy mindig használjon hello Azure és az SQL-adatbázis a frissítések tooMicrosoft szinkronizálva tooremain Management Studio legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="7e173-123">It is recommended that you always use hello latest version of Management Studio tooremain synchronized with updates tooMicrosoft Azure and SQL Database.</span></span> <span data-ttu-id="7e173-124">[Az SQL Server Management Studio frissítése](https://msdn.microsoft.com/library/mt238290.aspx).</span><span class="sxs-lookup"><span data-stu-id="7e173-124">[Update SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).</span></span>
> 
> 

### <a name="create-new-table"></a><span data-ttu-id="7e173-125">Új tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="7e173-125">Create new table</span></span>
<span data-ttu-id="7e173-126">Használjon környezetben menüpont "Új rendszerverzióval ellátott tábla" szolgáltatáshoz az SSMS Object Explorer tooopen hello Lekérdezésszerkesztő historikus tábla sablon parancsfájl, és majd a "Adjon meg értékeket a sablon paraméterek" (Ctrl + Shift + M) toopopulate hello sablont használ:</span><span class="sxs-lookup"><span data-stu-id="7e173-126">Use context menu item “New System-Versioned Table” in SSMS Object Explorer tooopen hello query editor with a temporal table template script and then use “Specify Values for Template Parameters” (Ctrl+Shift+M) toopopulate hello template:</span></span>

![SSMSNewTable](./media/sql-database-temporal-tables/AzureTemporal2.png)

<span data-ttu-id="7e173-128">Az SSDT "(a rendszerverzióval ellátott) Historikus tábla" sablont választ toohello adatbázis új projekt hozzáadása során.</span><span class="sxs-lookup"><span data-stu-id="7e173-128">In SSDT, chose “Temporal Table (System-Versioned)” template when adding new items toohello database project.</span></span> <span data-ttu-id="7e173-129">Hogy fog tábla megnyitása Tervező és engedélyezése, adja meg, hogy tooeasily hello table elrendezést:</span><span class="sxs-lookup"><span data-stu-id="7e173-129">That will open table designer and enable you tooeasily specify hello table layout:</span></span>

![SSDTNewTable](./media/sql-database-temporal-tables/AzureTemporal3.png)

<span data-ttu-id="7e173-131">Is historikus tábla létrehozása hello Transact-SQL utasítás közvetlenül, megadásával a hello az alábbi példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="7e173-131">You can also a create temporal table by specifying hello Transact-SQL statements directly, as shown in hello example below.</span></span> <span data-ttu-id="7e173-132">Ne feledje, hogy minden historikus tábla hello kötelező elemeit hello időtartam-definíciója és hello SYSTEM_VERSIONING záradékot egy tooanother felhasználói referenciatábla sor korábbi verziók tárolásához:</span><span class="sxs-lookup"><span data-stu-id="7e173-132">Note that hello mandatory elements of every temporal table are hello PERIOD definition and hello SYSTEM_VERSIONING clause with a reference tooanother user table that will store historical row versions:</span></span>

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

<span data-ttu-id="7e173-133">A rendszerverzióval ellátott historikus tábla létrehozásakor a rendszer automatikusan létrehoz hello kísérő előzménytábla hello alapértelmezett konfigurációval.</span><span class="sxs-lookup"><span data-stu-id="7e173-133">When you create system-versioned temporal table, hello accompanying history table with hello default configuration is automatically created.</span></span> <span data-ttu-id="7e173-134">hello alapértelmezett előzménytábla hello időtartamoszlopok (kezdő célból) egy fürtözött B-fa index tartalmaz lap tömörítést engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="7e173-134">hello default history table contains a clustered B-tree index on hello period columns (end, start) with page compression enabled.</span></span> <span data-ttu-id="7e173-135">Ez a konfiguráció nincs optimális hello többségének forgatókönyvek, amelyben az ideiglenes táblák használata esetén különösen a [adatok naplózását](https://msdn.microsoft.com/library/mt631669.aspx#Anchor_0).</span><span class="sxs-lookup"><span data-stu-id="7e173-135">This configuration is optimal for hello majority of scenarios in which temporal tables are used, especially for [data auditing](https://msdn.microsoft.com/library/mt631669.aspx#Anchor_0).</span></span> 

<span data-ttu-id="7e173-136">Ebben az esetben a Microsoft célja tooperform időalapú trendelemzés keresztül hosszabb adatok előzményeit, és nagyobb adatkészletek esetében a így hello tárolási választás az hello előzménytábla fürtözött oszlopcentrikus index.</span><span class="sxs-lookup"><span data-stu-id="7e173-136">In this particular case, we aim tooperform time-based trend analysis over a longer data history and with bigger data sets, so hello storage choice for hello history table is a clustered columnstore index.</span></span> <span data-ttu-id="7e173-137">A fürtözött oszlopcentrikus nagyon jó tömörítési elemzési lekérdezések teljesítményt nyújt.</span><span class="sxs-lookup"><span data-stu-id="7e173-137">A clustered columnstore provides very good compression and performance for analytical queries.</span></span> <span data-ttu-id="7e173-138">A historikus táblák biztosít rugalmasságot tooconfigure hello aktuális és a historikus táblák indexei teljesen önállóan hello.</span><span class="sxs-lookup"><span data-stu-id="7e173-138">Temporal Tables give you hello flexibility tooconfigure indexes on hello current and temporal tables completely independently.</span></span> 

> [!NOTE]
> <span data-ttu-id="7e173-139">Oszlopcentrikus indexek csak találhatók hello prémium szolgáltatásszintet.</span><span class="sxs-lookup"><span data-stu-id="7e173-139">Columnstore indexes are only available in hello premium service tier.</span></span>
>

<span data-ttu-id="7e173-140">a következő parancsfájl hello jeleníti meg, hogyan előzménytáblán alapértelmezett indexe megváltozott toohello fürtözött oszlopcentrikus lehet:</span><span class="sxs-lookup"><span data-stu-id="7e173-140">hello following script shows how default index on history table can be changed toohello clustered columnstore:</span></span>

````
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory
ON dbo.WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON); 
````

<span data-ttu-id="7e173-141">A historikus táblák hello Object Explorer hello adott ikon könnyebb azonosításához jelennek meg a előzménytábla gyermekcsomópontja megjelenítése közben.</span><span class="sxs-lookup"><span data-stu-id="7e173-141">Temporal Tables are represented in hello Object Explorer with hello specific icon for easier identification, while its history table is displayed as a child node.</span></span>

![AlterTable](./media/sql-database-temporal-tables/AzureTemporal4.png)

### <a name="alter-existing-table-tootemporal"></a><span data-ttu-id="7e173-143">Meglévő tábla tootemporal ALTER</span><span class="sxs-lookup"><span data-stu-id="7e173-143">Alter existing table tootemporal</span></span>
<span data-ttu-id="7e173-144">Most borítóján hello alternatív forgatókönyv mely hello WebsiteUserInfo tábla már létezik, de nem tervezett tookeep volt egy korábbi módosításait.</span><span class="sxs-lookup"><span data-stu-id="7e173-144">Let’s cover hello alternative scenario in which hello WebsiteUserInfo table already exists, but was not designed tookeep a history of changes.</span></span> <span data-ttu-id="7e173-145">Ebben az esetben egyszerűen kiterjesztheti hello meglévő tábla toobecome historikus hello a következő példa látható:</span><span class="sxs-lookup"><span data-stu-id="7e173-145">In this case, you can simply extend hello existing table toobecome temporal, as shown in hello following example:</span></span>

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

## <a name="step-2-run-your-workload-regularly"></a><span data-ttu-id="7e173-146">2. lépés: A számítási feladatok rendszeresen futtatása</span><span class="sxs-lookup"><span data-stu-id="7e173-146">Step 2: Run your workload regularly</span></span>
<span data-ttu-id="7e173-147">hello fő ideiglenes táblák előnye, hogy nem kell toochange, vagy állítsa be a webhely az bármely módon tooperform változáskövetést.</span><span class="sxs-lookup"><span data-stu-id="7e173-147">hello main advantage of Temporal Tables is that you do not need toochange or adjust your website in any way tooperform change tracking.</span></span> <span data-ttu-id="7e173-148">Létrehozása után a Historikus táblák transzparens módon továbbra is fennáll korábbi sor minden alkalommal, amikor módosításokat végezni az adatokon.</span><span class="sxs-lookup"><span data-stu-id="7e173-148">Once created, Temporal Tables transparently persist previous row versions every time you perform modifications on your data.</span></span> 

<span data-ttu-id="7e173-149">A sorrend tooleverage automatikus változáskövetése ebben a forgatókönyvben, most csak frissítse oszlop **PagesVisited** minden alkalommal, amikor a felhasználó megszűnik munkamenet hello webhelyen befejezése:</span><span class="sxs-lookup"><span data-stu-id="7e173-149">In order tooleverage automatic change tracking for this particular scenario, let’s just update column **PagesVisited** every time when user ends her/his session on hello website:</span></span>

````
UPDATE WebsiteUserInfo  SET [PagesVisited] = 5 
WHERE [UserID] = 1;
````

<span data-ttu-id="7e173-150">Fontos, hogy a frissítés lekérdezés hello toonotice nem szükséges tooknow hello pontos idő, hello tényleges művelet történt, sem hogyan előzményadatait megőrzi további elemzés céljából.</span><span class="sxs-lookup"><span data-stu-id="7e173-150">It is important toonotice that hello update query doesn’t need tooknow hello exact time when hello actual operation occurred nor how historical data will be preserved for future analysis.</span></span> <span data-ttu-id="7e173-151">Mindkét szempont hello Azure SQL Database automatikusan kezeli.</span><span class="sxs-lookup"><span data-stu-id="7e173-151">Both aspects are automatically handled by hello Azure SQL Database.</span></span> <span data-ttu-id="7e173-152">hello következő ábra bemutatja hogyan előzményadatok minden frissítési folyamatban.</span><span class="sxs-lookup"><span data-stu-id="7e173-152">hello following diagram illustrates how history data is being generated on every update.</span></span>

![TemporalArchitecture](./media/sql-database-temporal-tables/AzureTemporal5.png)

## <a name="step-3-perform-historical-data-analysis"></a><span data-ttu-id="7e173-154">3. lépés: Az előzményadatok elemzés végrehajtása</span><span class="sxs-lookup"><span data-stu-id="7e173-154">Step 3: Perform historical data analysis</span></span>
<span data-ttu-id="7e173-155">Most historikus rendszerverzió-engedélyezésekor a rendszer előzményadatokat elemzésre másik lapra, hogy csak egy lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="7e173-155">Now when temporal system-versioning is enabled, historical data analysis is just one query away from you.</span></span> <span data-ttu-id="7e173-156">Ebben a cikkben azt fogja adja meg a címet elemzés szabhatják - toolearn néhány példa minden adatait, megismerkedhet a különböző lehetőségek hello bevezetett [FOR SYSTEM_TIME](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_3) záradékban.</span><span class="sxs-lookup"><span data-stu-id="7e173-156">In this article, we will provide a few examples that address common analysis scenarios - toolearn all details, explore various options introduced with hello [FOR SYSTEM_TIME](https://msdn.microsoft.com/library/dn935015.aspx#Anchor_3) clause.</span></span>

<span data-ttu-id="7e173-157">toosee hello felső 10 olyan felhasználót egy órával ezelőtt történt, frissítésétől megtekintett weblapok hello száma alapján rendezve a lekérdezés futtatására:</span><span class="sxs-lookup"><span data-stu-id="7e173-157">toosee hello top 10 users ordered by hello number of visited web pages as of an hour ago, run this query:</span></span>

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
SELECT TOP 10 * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME AS OF @hourAgo
ORDER BY PagesVisited DESC
````

<span data-ttu-id="7e173-158">Könnyen módosíthatja a lekérdezés egy, a következő időpontban napján érvényes meglátogat tooanalyze hello hely egy hónapja vagy korábbi hello bármely pontján kívánja.</span><span class="sxs-lookup"><span data-stu-id="7e173-158">You can easily modify this query tooanalyze hello site visits as of a day ago, a month ago or at any point in hello past you wish.</span></span>

<span data-ttu-id="7e173-159">alapvető statisztikai elemzések tooperform hello előző nap, a következő példa hello használata:</span><span class="sxs-lookup"><span data-stu-id="7e173-159">tooperform basic statistical analysis for hello previous day, use hello following example:</span></span>

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

<span data-ttu-id="7e173-160">a tevékenységek egy adott felhasználó belül egy adott időn belül, használjon hello TARTALMAZOTT IN záradék toosearch:</span><span class="sxs-lookup"><span data-stu-id="7e173-160">toosearch for activities of a specific user, within a period of time, use hello CONTAINED IN clause:</span></span>

````
DECLARE @hourAgo datetime2 = DATEADD(HOUR, -1, SYSUTCDATETIME());
DECLARE @twoHoursAgo datetime2 = DATEADD(HOUR, -2, SYSUTCDATETIME());
SELECT * FROM dbo.WebsiteUserInfo 
FOR SYSTEM_TIME CONTAINED IN (@twoHoursAgo, @hourAgo)
WHERE [UserID] = 1;
````

<span data-ttu-id="7e173-161">Grafikus képi megjelenítés lehetőség időalapú lekérdezéseknél különösen akkor hasznos, mivel Ön lehet megjeleníteni a trendek és használati szokások egy egyszerűen elsajátítható módja nagyon egyszerűen:</span><span class="sxs-lookup"><span data-stu-id="7e173-161">Graphic visualization is especially convenient for temporal queries as you can show trends and usage patterns in an intuitive way very easily:</span></span>

![TemporalGraph](./media/sql-database-temporal-tables/AzureTemporal6.png)

## <a name="evolving-table-schema"></a><span data-ttu-id="7e173-163">Folyamatosan változó a következő tábla sémáját</span><span class="sxs-lookup"><span data-stu-id="7e173-163">Evolving table schema</span></span>
<span data-ttu-id="7e173-164">Általában akkor toochange hello historikus tábla sémáját használata közben is alkalmazások fejlesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="7e173-164">Typically, you will need toochange hello temporal table schema while you are doing app development.</span></span> <span data-ttu-id="7e173-165">Ehhez egyszerűen futtassa a rendszeres az ALTER TABLE utasítások, és az Azure SQL Database megfelelő továbbterjeszti majd a változtatások toohello előzménytábla.</span><span class="sxs-lookup"><span data-stu-id="7e173-165">For that, simply run regular ALTER TABLE statements and Azure SQL Database will appropriately propagate changes toohello history table.</span></span> <span data-ttu-id="7e173-166">hello alábbi parancsfájl bemutatja, hogyan adhat meg további attribútum figyelését:</span><span class="sxs-lookup"><span data-stu-id="7e173-166">hello following script shows how you can add additional attribute for tracking:</span></span>

````
/*Add new column for tracking source IP address*/
ALTER TABLE dbo.WebsiteUserInfo 
ADD  [IPAddress] varchar(128) NOT NULL CONSTRAINT DF_Address DEFAULT 'N/A';
````

<span data-ttu-id="7e173-167">Módosíthatja hasonlóan oszlop definíciójában, amíg aktív a számítási feladatok:</span><span class="sxs-lookup"><span data-stu-id="7e173-167">Similarly, you can change column definition while your workload is active:</span></span>

````
/*Increase hello length of name column*/
ALTER TABLE dbo.WebsiteUserInfo 
    ALTER COLUMN  UserName nvarchar(256) NOT NULL;
````

<span data-ttu-id="7e173-168">Végezetül is eltávolíthat egy oszlopot, amely már nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="7e173-168">Finally, you can remove a column that you do not need anymore.</span></span>

````
/*Drop unnecessary column */
ALTER TABLE dbo.WebsiteUserInfo 
    DROP COLUMN TemporaryColumn; 
````

<span data-ttu-id="7e173-169">Másik megoldásként használhatja a legújabb [SSDT](https://msdn.microsoft.com/library/mt204009.aspx) toochange historikus tábla séma csatlakoztatott toohello adatbázis (online mód) vagy hello adatbázis projekt (kapcsolat nélküli módban) részeként.</span><span class="sxs-lookup"><span data-stu-id="7e173-169">Alternatively, use latest [SSDT](https://msdn.microsoft.com/library/mt204009.aspx) toochange temporal table schema while you are connected toohello database (online mode) or as part of hello database project (offline mode).</span></span>

## <a name="controlling-retention-of-historical-data"></a><span data-ttu-id="7e173-170">Korábbi adatok megőrzése vezérlése</span><span class="sxs-lookup"><span data-stu-id="7e173-170">Controlling retention of historical data</span></span>
<span data-ttu-id="7e173-171">A rendszerverzióval ellátott historikus táblákon hello előzménytábla megnőhet reguláris táblák nagyobb hello adatbázis mérete.</span><span class="sxs-lookup"><span data-stu-id="7e173-171">With system-versioned temporal tables, hello history table may increase hello database size more than regular tables.</span></span> <span data-ttu-id="7e173-172">Egy nagy- és egyre előzménytábla válhat a lejárat toopure tárolási költségek, valamint a teljesítmény előíró adó historikus lekérdezése hibát.</span><span class="sxs-lookup"><span data-stu-id="7e173-172">A large and ever-growing history table can become an issue both due toopure storage costs as well as imposing a performance tax on temporal querying.</span></span> <span data-ttu-id="7e173-173">Emiatt az adatmegőrzési házirend hello tábla adatok kezelésére szolgáló fejlesztése tervezési és minden historikus tábla hello kezeléséért fontos eleme.</span><span class="sxs-lookup"><span data-stu-id="7e173-173">Hence, developing a data retention policy for managing data in hello history table is an important aspect of planning and managing hello lifecycle of every temporal table.</span></span> <span data-ttu-id="7e173-174">Az Azure SQL Database a következő előzményadatokat hello historikus tábla kezelésére szolgáló módszerek hello közül választhat:</span><span class="sxs-lookup"><span data-stu-id="7e173-174">With Azure SQL Database, you have hello following approaches for managing historical data in hello temporal table:</span></span>

* [<span data-ttu-id="7e173-175">Táblaparticionálást</span><span class="sxs-lookup"><span data-stu-id="7e173-175">Table Partitioning</span></span>](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_2)
* [<span data-ttu-id="7e173-176">Egyéni karbantartási parancsprogramot</span><span class="sxs-lookup"><span data-stu-id="7e173-176">Custom Cleanup Script</span></span>](https://msdn.microsoft.com/library/mt637341.aspx#Anchor_3)

## <a name="next-steps"></a><span data-ttu-id="7e173-177">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7e173-177">Next steps</span></span>
<span data-ttu-id="7e173-178">A Historikus táblák részletes információkért tekintse meg [az MSDN dokumentációjában tájékozódhat](https://msdn.microsoft.com/library/dn935015.aspx).</span><span class="sxs-lookup"><span data-stu-id="7e173-178">For detailed information on Temporal Tables, check out [MSDN documentation](https://msdn.microsoft.com/library/dn935015.aspx).</span></span>
<span data-ttu-id="7e173-179">Látogasson el Channel 9 toohear egy [valós felhasználói historikus implementációs sikeres szövegegység](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) és figyelési egy [historikus bemutató élő](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).</span><span class="sxs-lookup"><span data-stu-id="7e173-179">Visit Channel 9 toohear a [real customer temporal implemenation success story](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) and watch a [live temporal demonstration](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).</span></span>

