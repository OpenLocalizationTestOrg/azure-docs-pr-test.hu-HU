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
# <a name="manage-historical-data-in-temporal-tables-with-retention-policy"></a><span data-ttu-id="d0e59-103">Előzményadatokat az ideiglenes táblák megőrzési házirend kezelése</span><span class="sxs-lookup"><span data-stu-id="d0e59-103">Manage historical data in Temporal Tables with retention policy</span></span>
<span data-ttu-id="d0e59-104">A historikus táblák növelheti reguláris táblák nagyobb adatbázisméret, különösen akkor, ha egy hosszabb ideig a korábbi adatok megőrzése mellett.</span><span class="sxs-lookup"><span data-stu-id="d0e59-104">Temporal Tables may increase database size more than regular tables, especially if you retain historical data for a longer period of time.</span></span> <span data-ttu-id="d0e59-105">Emiatt adatmegőrzési korábbi adatok tervezési és minden historikus tábla hello kezeléséért fontos eleme.</span><span class="sxs-lookup"><span data-stu-id="d0e59-105">Hence, retention policy for historical data is an important aspect of planning and managing hello lifecycle of every temporal table.</span></span> <span data-ttu-id="d0e59-106">Az Azure SQL Database ideiglenes táblák megőrzési könnyen kezelhető mechanizmust, amely segít ennek a feladatnak rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="d0e59-106">Temporal Tables in Azure SQL Database come with easy-to-use retention mechanism that helps you accomplish this task.</span></span>

<span data-ttu-id="d0e59-107">Hello egyedi tábla szintje, amely lehetővé teszi a felhasználóknak rugalmas korosodási toocreate házirendek historikus előzménytáblákra megőrzési is be kell állítani.</span><span class="sxs-lookup"><span data-stu-id="d0e59-107">Temporal history retention can be configured at hello individual table level, which allows users toocreate flexible aging polices.</span></span> <span data-ttu-id="d0e59-108">Egyszerű historikus megőrzési alkalmazása: tábla létrehozása vagy séma módosítása során csak egy paraméter toobe van szükség.</span><span class="sxs-lookup"><span data-stu-id="d0e59-108">Applying temporal retention is simple: it requires only one parameter toobe set during table creation or schema change.</span></span>

<span data-ttu-id="d0e59-109">Adatmegőrzési házirend meghatározása után az Azure SQL Database elindul, rendszeresen ellenőrzése, hogy van-e korábbi azon sorait, amelyek jogosultak a automatikus adatok törlése.</span><span class="sxs-lookup"><span data-stu-id="d0e59-109">After you define retention policy, Azure SQL Database starts checking regularly if there are historical rows that are eligible for automatic data cleanup.</span></span> <span data-ttu-id="d0e59-110">Egyező sorok azonosítása és hello előzménytábla számított transzparens módon, fordul elő, amely ütemezett és hello rendszer futtathatja hello háttérfeladat.</span><span class="sxs-lookup"><span data-stu-id="d0e59-110">Identification of matching rows and their removal from hello history table occur transparently, in hello background task that is scheduled and run by hello system.</span></span> <span data-ttu-id="d0e59-111">Hello előzmények táblázat sorait kora feltételét SYSTEM_TIME időszak végét jelölő hello oszlop alapján a rendszer ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="d0e59-111">Age condition for hello history table rows is checked based on hello column representing end of SYSTEM_TIME period.</span></span> <span data-ttu-id="d0e59-112">Ha a megőrzési időtartam, például értéke toosix hónap, a tisztítás jogosult tábla sorainak kielégítéséhez hello feltétel a következő:</span><span class="sxs-lookup"><span data-stu-id="d0e59-112">If retention period, for example, is set toosix months, table rows eligible for cleanup satisfy hello following condition:</span></span>

````
ValidTo < DATEADD (MONTH, -6, SYSUTCDATETIME())
````

<span data-ttu-id="d0e59-113">Példa megelőző hello, azt feltételezik, hogy **ValidTo** oszlop felel meg a SYSTEM_TIME időszak vége toohello.</span><span class="sxs-lookup"><span data-stu-id="d0e59-113">In hello preceding example, we assumed that **ValidTo** column corresponds toohello end of SYSTEM_TIME period.</span></span>

## <a name="how-tooconfigure-retention-policy"></a><span data-ttu-id="d0e59-114">Hogyan tooconfigure adatmegőrzési?</span><span class="sxs-lookup"><span data-stu-id="d0e59-114">How tooconfigure retention policy?</span></span>
<span data-ttu-id="d0e59-115">Adatmegőrzési historikus táblához tartozó konfigurálása előtt először ellenőrizze, hogy engedélyezett-e a historikus előzmények megőrzési *hello adatbázis szintjén*.</span><span class="sxs-lookup"><span data-stu-id="d0e59-115">Before you configure retention policy for a temporal table, check first whether temporal historical retention is enabled *at hello database level*.</span></span>

````
SELECT is_temporal_history_retention_enabled, name
FROM sys.databases
````

<span data-ttu-id="d0e59-116">Adatbázis-jelző **is_temporal_history_retention_enabled** set tooON alapértelmezés szerint, de a felhasználók módosíthatják az ALTER DATABASE utasítással.</span><span class="sxs-lookup"><span data-stu-id="d0e59-116">Database flag **is_temporal_history_retention_enabled** is set tooON by default, but users can change it with ALTER DATABASE statement.</span></span> <span data-ttu-id="d0e59-117">Egyúttal automatikusan set tooOFF után [időponthoz kötött visszaállítás](sql-database-recovery-using-backups.md) műveletet.</span><span class="sxs-lookup"><span data-stu-id="d0e59-117">It is also automatically set tooOFF after [point in time restore](sql-database-recovery-using-backups.md) operation.</span></span> <span data-ttu-id="d0e59-118">tooenable historikus előzményeinek megőrzési eltávolítása az adatbázis, a következő utasítás hello hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="d0e59-118">tooenable temporal history retention cleanup for your database, execute hello following statement:</span></span>

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

> [!IMPORTANT]
> <span data-ttu-id="d0e59-119">Beállíthatja, hogy ideiglenes táblák még akkor is, ha a megőrzési **is_temporal_history_retention_enabled** értéke OFF, de az automatikus tisztítás elavult sorok nem indulnak el ebben az esetben.</span><span class="sxs-lookup"><span data-stu-id="d0e59-119">You can configure retention for temporal tables even if **is_temporal_history_retention_enabled** is OFF, but automatic cleanup for aged rows is not triggered in that case.</span></span>
> 
> 

<span data-ttu-id="d0e59-120">Hello HISTORY_RETENTION_PERIOD paraméter megadásával tábla létrehozása során van konfigurálva adatmegőrzési szabály:</span><span class="sxs-lookup"><span data-stu-id="d0e59-120">Retention policy is configured during table creation by specifying value for hello HISTORY_RETENTION_PERIOD parameter:</span></span>

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

<span data-ttu-id="d0e59-121">Az Azure SQL Database lehetővé teszi toospecify megőrzési időszak különböző időegységek használatával: nap, hét, hónap és év.</span><span class="sxs-lookup"><span data-stu-id="d0e59-121">Azure SQL Database allows you toospecify retention period by using different time units: DAYS, WEEKS, MONTHS, and YEARS.</span></span> <span data-ttu-id="d0e59-122">Ha nincs megadva HISTORY_RETENTION_PERIOD, végtelen adatmegőrzési feltételezi.</span><span class="sxs-lookup"><span data-stu-id="d0e59-122">If HISTORY_RETENTION_PERIOD is omitted, INFINITE retention is assumed.</span></span> <span data-ttu-id="d0e59-123">VÉGTELEN kulcsszó explicit módon is használható.</span><span class="sxs-lookup"><span data-stu-id="d0e59-123">You can also use INFINITE keyword explicitly.</span></span>

<span data-ttu-id="d0e59-124">Bizonyos esetekben érdemes tooconfigure megőrzési tábla létrehozása után, vagy toochange korábban megadott érték.</span><span class="sxs-lookup"><span data-stu-id="d0e59-124">In some scenarios, you may want tooconfigure retention after table creation, or toochange previously configured value.</span></span> <span data-ttu-id="d0e59-125">Ebben az esetben használja az ALTER TABLE utasítást:</span><span class="sxs-lookup"><span data-stu-id="d0e59-125">In that case use ALTER TABLE statement:</span></span>

````
ALTER TABLE dbo.WebsiteUserInfo
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 9 MONTHS));
````

> [!IMPORTANT]
> <span data-ttu-id="d0e59-126">Beállítás a SYSTEM_VERSIONING tooOFF *nem őrzi meg a* megőrzési idő értékét.</span><span class="sxs-lookup"><span data-stu-id="d0e59-126">Setting SYSTEM_VERSIONING tooOFF *does not preserve* retention period value.</span></span> <span data-ttu-id="d0e59-127">Beállítás nélkül HISTORY_RETENTION_PERIOD SYSTEM_VERSIONING tooON megadott explicit módon eredményez hello végtelen adatmegőrzési idővel.</span><span class="sxs-lookup"><span data-stu-id="d0e59-127">Setting SYSTEM_VERSIONING tooON without HISTORY_RETENTION_PERIOD specified explicitly results in hello INFINITE retention period.</span></span>
> 
> 

<span data-ttu-id="d0e59-128">tooreview hello megőrzési házirend aktuális állapotát, használja hello következő lekérdezés adott illesztések historikus megőrzési engedélyezése jelzőt az egyes táblák megőrzési időtartamát hello adatbázis szintjén:</span><span class="sxs-lookup"><span data-stu-id="d0e59-128">tooreview current state of hello retention policy, use hello following query that joins temporal retention enablement flag at hello database level with retention periods for individual tables:</span></span>

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


## <a name="how-sql-database-deletes-aged-rows"></a><span data-ttu-id="d0e59-129">Hogyan SQL-adatbázis törli az elavult sorok?</span><span class="sxs-lookup"><span data-stu-id="d0e59-129">How SQL Database deletes aged rows?</span></span>
<span data-ttu-id="d0e59-130">hello kitakarítási folyamatot hello index elrendezését hello előzménytábla függ.</span><span class="sxs-lookup"><span data-stu-id="d0e59-130">hello cleanup process depends on hello index layout of hello history table.</span></span> <span data-ttu-id="d0e59-131">Fontos toonotice, amely *csak egy fürtözött index, (B-fa vagy oszlopcentrikus) előzmények táblákban lehet konfigurált véges adatmegőrzési*.</span><span class="sxs-lookup"><span data-stu-id="d0e59-131">It is important toonotice that *only history tables with a clustered index (B-tree or columnstore) can have finite retention policy configured*.</span></span> <span data-ttu-id="d0e59-132">Háttérfeladat tooperform azokat az elavult adatok törlése az összes ideiglenes tábláknál véges megőrzési időszak hozza létre.</span><span class="sxs-lookup"><span data-stu-id="d0e59-132">A background task is created tooperform aged data cleanup for all temporal tables with finite retention period.</span></span>
<span data-ttu-id="d0e59-133">Hello sortárindex (B-fa) fürtözött index törlése programot törli az elavult sorában kisebb csoportjai (felfelé too10K) minimalizálja a terhelés adatbázis naplója és i/o-alrendszer.</span><span class="sxs-lookup"><span data-stu-id="d0e59-133">Cleanup logic for hello rowstore (B-tree) clustered index deletes aged row in smaller chunks (up too10K) minimizing pressure on database log and I/O subsystem.</span></span> <span data-ttu-id="d0e59-134">Habár a tisztítás logika használja szükséges a B-fa index, régebbi, mint a megőrzési időtartam nem garantálható erősen hello sorok törlések sorrendjét.</span><span class="sxs-lookup"><span data-stu-id="d0e59-134">Although cleanup logic utilizes required B-tree index, order of deletions for hello rows older than retention period cannot be firmly guaranteed.</span></span> <span data-ttu-id="d0e59-135">Emiatt *hello tisztítás rendelés az alkalmazások bármely függőség nem hajtja végre*.</span><span class="sxs-lookup"><span data-stu-id="d0e59-135">Hence, *do not take any dependency on hello cleanup order in your applications*.</span></span>

<span data-ttu-id="d0e59-136">hello hello fürtözött oszlopcentrikus a karbantartási feladat eltávolítja teljes [csoportok sor](https://msdn.microsoft.com/library/gg492088.aspx) egyszerre (általában tartalmaz minden sorok 1 millió), ez nagyon hatékony, különösen akkor, ha előzményadatokat magas ütemben jön létre.</span><span class="sxs-lookup"><span data-stu-id="d0e59-136">hello cleanup task for hello clustered columnstore removes entire [row groups](https://msdn.microsoft.com/library/gg492088.aspx) at once (typically contain 1 million of rows each), which is very efficient, especially when historical data is generated at a high pace.</span></span>

![Fürtözött oszlopcentrikus megőrzési](./media/sql-database-temporal-tables-retention-policy/cciretention.png)

<span data-ttu-id="d0e59-138">Kiváló adattömörítés és hatékony megőrzési tisztítás teszi fürtözött oszlopcentrikus index forgatókönyvek tökéletes választás, ha a gyors munkaterhelése nagy mennyiségű előzményadatok.</span><span class="sxs-lookup"><span data-stu-id="d0e59-138">Excellent data compression and efficient retention cleanup makes clustered columnstore index a perfect choice for scenarios when your workload rapidly generates high amount of historical data.</span></span> <span data-ttu-id="d0e59-139">Minta része jellemzően intenzív [ideiglenes táblák használó munkaterhelések tranzakciós feldolgozást](https://msdn.microsoft.com/library/mt631669.aspx) a változások követése és naplózása, trendelemzés vagy IoT adatfeldolgozást.</span><span class="sxs-lookup"><span data-stu-id="d0e59-139">That pattern is typical for intensive [transactional processing workloads that use temporal tables](https://msdn.microsoft.com/library/mt631669.aspx) for change tracking and auditing, trend analysis, or IoT data ingestion.</span></span>

## <a name="index-considerations"></a><span data-ttu-id="d0e59-140">Index kapcsolatos szempontok</span><span class="sxs-lookup"><span data-stu-id="d0e59-140">Index considerations</span></span>
<span data-ttu-id="d0e59-141">hello karbantartási feladat sortárindex fürtözött indexszel rendelkező táblák hello oszlop megfelelő hello SYSTEM_TIME időszak végén az index toostart igényel.</span><span class="sxs-lookup"><span data-stu-id="d0e59-141">hello cleanup task for tables with rowstore clustered index requires index toostart with hello column corresponding hello end of SYSTEM_TIME period.</span></span> <span data-ttu-id="d0e59-142">Ha ilyen index nem létezik, a véges megőrzési időtartam nem konfigurálhatja:</span><span class="sxs-lookup"><span data-stu-id="d0e59-142">If such index doesn't exist, you cannot configure a finite retention period:</span></span>

<span data-ttu-id="d0e59-143">*Üzenet 13765, szint 16 állapot 1 <br> </br> véges megőrzési időszak beállítása sikertelen volt a rendszerverzióval ellátott historikus tábla "temporalstagetestdb.dbo.WebsiteUserInfo", mert hello előzménytábla " temporalstagetestdb.dbo.WebsiteUserInfoHistory "nem tartalmazza a szükséges fürtözött indexszel. Érdemes lehet létrehozni a fürtözött oszlopcentrikus vagy B-fa index kezdve hello oszlop SYSTEM_TIME végének megfelelő idő alatt hello előzménytáblán.*</span><span class="sxs-lookup"><span data-stu-id="d0e59-143">*Msg 13765, Level 16, State 1 <br></br> Setting finite retention period failed on system-versioned temporal table 'temporalstagetestdb.dbo.WebsiteUserInfo' because hello history table 'temporalstagetestdb.dbo.WebsiteUserInfoHistory' does not contain required clustered index. Consider creating a clustered columnstore or B-tree index starting with hello column that matches end of SYSTEM_TIME period, on hello history table.*</span></span>

<span data-ttu-id="d0e59-144">Fontos, hogy már az Azure SQL Database által létrehozott alapértelmezett előzménytábla hello toonotice rendelkezik fürtözött index, amely megfelel az adatmegőrzési.</span><span class="sxs-lookup"><span data-stu-id="d0e59-144">It is important toonotice that hello default history table created by Azure SQL Database already has clustered index, which is compliant for retention policy.</span></span> <span data-ttu-id="d0e59-145">Tooremove meg, hogy az index véges megőrzési idővel rendelkező táblán, ha sikertelen a következő hiba hello:</span><span class="sxs-lookup"><span data-stu-id="d0e59-145">If you try tooremove that index on a table with finite retention period, operation fails with hello following error:</span></span>

<span data-ttu-id="d0e59-146">*Üzenet 13766, szint 16 állapot 1 <br> </br> hello fürtözött index "WebsiteUserInfoHistory.IX_WebsiteUserInfoHistory" nem dobható el, mert használatban van az automatikus tisztítás az elavult adatokat. Ha ezt az indexet kell toodrop, fontolja meg beállítás HISTORY_RETENTION_PERIOD tooINFINITE hello rendszerverzióval ellátott historikus táblán megfelelő.*</span><span class="sxs-lookup"><span data-stu-id="d0e59-146">*Msg 13766, Level 16, State 1 <br></br> Cannot drop hello clustered index 'WebsiteUserInfoHistory.IX_WebsiteUserInfoHistory' because it is being used for automatic cleanup of aged data. Consider setting HISTORY_RETENTION_PERIOD tooINFINITE on hello corresponding system-versioned temporal table if you need toodrop this index.*</span></span>

<span data-ttu-id="d0e59-147">Karbantartást hello fürtözött oszlopcentrikus indexet az optimális működik, ha a korábbi sorok szúrja be a növekvő sorrendben (hello végét rendszeridőtartam-oszlop szerint rendezett), amely esetén mindig hello eset hello előzménytábla fel van töltve, kizárólagos módon hello SYSTEM_ hello VERSIONIOING mechanizmus.</span><span class="sxs-lookup"><span data-stu-id="d0e59-147">Cleanup on hello clustered columnstore index works optimally if historical rows are inserted in hello ascending order (ordered by hello end of period column), which is always hello case when hello history table is populated exclusively by hello SYSTEM_VERSIONIOING mechanism.</span></span> <span data-ttu-id="d0e59-148">Hello előzmények tábla soraival nem időtartamoszlop (amely hello eset is lehet, ha meglévő előzményadatokat áttelepítette) végét szerint rendezett, ha célszerű újra létrehozni a fürtözött oszlopcentrikus index felett, hogy megfelelően van rendezve, optimális tooachieve B-fa sortárindex index teljesítmény.</span><span class="sxs-lookup"><span data-stu-id="d0e59-148">If rows in hello history table are not ordered by end of period column (which may be hello case if you migrated existing historical data), you should re-create clustered columnstore index on top of B-tree rowstore index that is properly ordered, tooachieve optimal performance.</span></span>

<span data-ttu-id="d0e59-149">Kerülje a hello előzménytáblán hello véges megőrzési időtartam fürtözött oszloptárindex újraépítését, mert a természetes hello rendszerverzió művelet által előírt hello sorcsoportok rendelési módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="d0e59-149">Avoid rebuilding clustered columnstore index on hello history table with hello finite retention period, because it may change ordering in hello row groups naturally imposed by hello system-versioning operation.</span></span> <span data-ttu-id="d0e59-150">Ha fürtözött oszloptárindexe toorebuild hello előzménytábla van szüksége, ehhez ismételt létrehozása megfelelő B-fa index, megőrzi az szereplő adatok rendszeres karbantartása szükséges hello rowgroups felett.</span><span class="sxs-lookup"><span data-stu-id="d0e59-150">If you need toorebuild clustered columnstore index on hello history table, do that by re-creating it on top of compliant B-tree index, preserving ordering in hello rowgroups necessary for regular data cleanup.</span></span> <span data-ttu-id="d0e59-151">ugyanezt a megközelítést kell végezni, meglévő táblába rendelkezik fürtözött oszlopindex garantált adatok rendelés nélkül hozunk létre historikus tábla hello:</span><span class="sxs-lookup"><span data-stu-id="d0e59-151">hello same approach should be taken if you create temporal table with existing history table that has clustered column index without guaranteed data order:</span></span>

````
/*Create B-tree ordered by hello end of period column*/
CREATE CLUSTERED INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory (ValidTo)
WITH (DROP_EXISTING = ON);
GO
/*Re-create clustered columnstore index*/
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON);
````

<span data-ttu-id="d0e59-152">Ha hello előzménytábla hello fürtözött oszlopcentrikus indexszel rendelkező véges megőrzési időszak van konfigurálva, az adott táblához nem hozható létre a további nem fürtözött B-fa indexek:</span><span class="sxs-lookup"><span data-stu-id="d0e59-152">When finite retention period is configured for hello history table with hello clustered columnstore index, you cannot create additional non-clustered B-tree indexes on that table:</span></span>

````
CREATE NONCLUSTERED INDEX IX_WebHistNCI ON WebsiteUserInfoHistory ([UserName])
````

<span data-ttu-id="d0e59-153">Egy kísérlet tooexecute fent utasítás sikertelen, és hiba a következő hello:</span><span class="sxs-lookup"><span data-stu-id="d0e59-153">An attempt tooexecute above statement fails with hello following error:</span></span>

<span data-ttu-id="d0e59-154">*Üzenet 13772, szint 16 állapot 1 <br> </br> index nem hozható létre fürtözetlen index historikus előzménytábla "WebsiteUserInfoHistory" mert véges megőrzési időtartam és a fürtözött oszlopcentrikus indexszel rendelkezik.*</span><span class="sxs-lookup"><span data-stu-id="d0e59-154">*Msg 13772, Level 16, State 1 <br></br> Cannot create non-clustered index on a temporal history table 'WebsiteUserInfoHistory' since it has finite retention period and clustered columnstore index defined.*</span></span>

## <a name="querying-tables-with-retention-policy"></a><span data-ttu-id="d0e59-155">Adatmegőrzési házirend-táblákban lekérdezése</span><span class="sxs-lookup"><span data-stu-id="d0e59-155">Querying tables with retention policy</span></span>
<span data-ttu-id="d0e59-156">Hello historikus tábla összes lekérdezés automatikusan kiszűrhetők korábbi sorok véges adatmegőrzési, tooavoid előre nem látható és inkonzisztens eredményeket, mert azokat az elavult sorok törölheti az hello karbantartási feladat, *időt, majd a bármikor tetszőleges sorrendben*.</span><span class="sxs-lookup"><span data-stu-id="d0e59-156">All queries on hello temporal table automatically filter out historical rows matching finite retention policy, tooavoid unpredictable and inconsistent results, since aged rows can be deleted by hello cleanup task, *at any point in time and in arbitrary order*.</span></span>

<span data-ttu-id="d0e59-157">hello alábbi képen látható hello lekérdezéstervet egy egyszerű lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="d0e59-157">hello following picture shows hello query plan for a simple query:</span></span>

````
SELECT * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME ALL;
````

<span data-ttu-id="d0e59-158">terv további szűrője hello lekérdezési időszak oszlop (ValidTo) tooend hello fürtözött Index vizsgálata operátor (kiemelt) hello előzménytáblán alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="d0e59-158">hello query plan includes additional filter applied tooend of period column (ValidTo) in hello Clustered Index Scan operator on hello history table (highlighted).</span></span> <span data-ttu-id="d0e59-159">Ez a példa feltételezi, hogy az egy hónap megőrzési időszak WebsiteUserInfo táblán lett beállítva.</span><span class="sxs-lookup"><span data-stu-id="d0e59-159">This example assumes that one MONTH retention period was set on WebsiteUserInfo table.</span></span>

![Megőrzési lekérdezés szűrője](./media/sql-database-temporal-tables-retention-policy/queryexecplanwithretention.png)

<span data-ttu-id="d0e59-161">Azonban ha előzménytábla közvetlenül lekérdezéséhez jelenhet meg sorokat, amelyek a régebbi, mint a megadott megőrzési időszak, de bármely garancia nélkül a ismételhető lekérdezési eredmények.</span><span class="sxs-lookup"><span data-stu-id="d0e59-161">However, if you query history table directly, you may see rows that are older than specified retention period, but without any guarantee for repeatable query results.</span></span> <span data-ttu-id="d0e59-162">hello alábbi képen látható hello lekérdezés végrehajtási lekérdezésterv hello előzménytáblán további szűrők alkalmazása nélkül:</span><span class="sxs-lookup"><span data-stu-id="d0e59-162">hello following picture shows query execution plan for hello query on hello history table without additional filters applied:</span></span>

![Előzmények megőrzési szűrő nélkül lekérdezése](./media/sql-database-temporal-tables-retention-policy/queryexecplanhistorytable.png)

<span data-ttu-id="d0e59-164">Támaszkodjon az üzleti logikát olvasási előzménytábla megőrzési időtartamon túl, mivel nem várt vagy inkonzisztens eredményeket kaphat.</span><span class="sxs-lookup"><span data-stu-id="d0e59-164">Do not rely your business logic on reading history table beyond retention period as you may get inconsistent or unexpected results.</span></span> <span data-ttu-id="d0e59-165">Azt javasoljuk, hogy használ historikus lekérdezések FOR SYSTEM_TIME záradék historikus táblákban adatok elemzéséhez.</span><span class="sxs-lookup"><span data-stu-id="d0e59-165">We recommend that you use temporal queries with FOR SYSTEM_TIME clause for analyzing data in temporal tables.</span></span>

## <a name="point-in-time-restore-considerations"></a><span data-ttu-id="d0e59-166">Mutasson a idő visszaállítási szempontjai</span><span class="sxs-lookup"><span data-stu-id="d0e59-166">Point in time restore considerations</span></span>
<span data-ttu-id="d0e59-167">Új adatbázis létrehozásakor [visszaállítása a meglévő adatbázis tooa adott pontot időben](sql-database-recovery-using-backups.md), historikus megőrzési hello adatbázis szintjén tiltva van.</span><span class="sxs-lookup"><span data-stu-id="d0e59-167">When you create new database by [restoring existing database tooa specific point in time](sql-database-recovery-using-backups.md), it has temporal retention disabled at hello database level.</span></span> <span data-ttu-id="d0e59-168">(**is_temporal_history_retention_enabled** jelző tooOFF beállítva).</span><span class="sxs-lookup"><span data-stu-id="d0e59-168">(**is_temporal_history_retention_enabled** flag set tooOFF).</span></span> <span data-ttu-id="d0e59-169">Ez a funkció lehetővé teszi tooexamine összes korábbi sor program a visszaállítás nem tartania során sorok elavult tooquery kap előtt távolítsa el őket.</span><span class="sxs-lookup"><span data-stu-id="d0e59-169">This functionality allows you tooexamine all historical rows upon restore, without worrying that aged rows are removed before you get tooquery them.</span></span> <span data-ttu-id="d0e59-170">Használat túl*vizsgálja meg a konfigurált megőrzési időtartamon túl előzményadatokat*.</span><span class="sxs-lookup"><span data-stu-id="d0e59-170">You can use it too*inspect historical data beyond configured retention period*.</span></span>

<span data-ttu-id="d0e59-171">Tegyük fel például, hogy a historikus tábla rendelkezik-e a megadott időszak egy hónap megőrzési.</span><span class="sxs-lookup"><span data-stu-id="d0e59-171">Say that a temporal table has one MONTH retention period specified.</span></span> <span data-ttu-id="d0e59-172">Az adatbázis Premium szolgáltatási rétegben jött létre, ha lehetővé válik az adatbázis-másolat képes toocreate be újra a múltbeli hello too35 nap hello adatbázis állapotú.</span><span class="sxs-lookup"><span data-stu-id="d0e59-172">If your database was created in Premium Service tier, you would be able toocreate database copy with hello database state up too35 days back in hello past.</span></span> <span data-ttu-id="d0e59-173">Amely hatékonyan lehetővé teszi tooanalyze korábbi sorokat, amelyek be too65 napos hello előzménytábla közvetlen lekérdezésével.</span><span class="sxs-lookup"><span data-stu-id="d0e59-173">That effectively would allow you tooanalyze historical rows that are up too65 days old by querying hello history table directly.</span></span>

<span data-ttu-id="d0e59-174">Ha azt szeretné, hogy tooactivate historikus megőrzési karbantartása, futtassa a következő Transact-SQL utasítás után időponthoz kötött visszaállításra hello:</span><span class="sxs-lookup"><span data-stu-id="d0e59-174">If you want tooactivate temporal retention cleanup, run hello following Transact-SQL statement after point in time restore:</span></span>

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

## <a name="next-steps"></a><span data-ttu-id="d0e59-175">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d0e59-175">Next steps</span></span>
<span data-ttu-id="d0e59-176">Hogyan toouse ideiglenes táblák az alkalmazásban kivétel toolearn [Ismerkedés az Azure SQL Database az ideiglenes táblák](sql-database-temporal-tables.md).</span><span class="sxs-lookup"><span data-stu-id="d0e59-176">toolearn how toouse Temporal Tables in your applications, check out [Getting Started with Temporal Tables in Azure SQL Database](sql-database-temporal-tables.md).</span></span>

<span data-ttu-id="d0e59-177">Látogasson el Channel 9 toohear egy [valós felhasználói historikus végrehajtása sikeres szövegegység](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) és figyelési egy [historikus bemutató élő](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).</span><span class="sxs-lookup"><span data-stu-id="d0e59-177">Visit Channel 9 toohear a [real customer temporal implementation success story](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) and watch a [live temporal demonstration](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).</span></span>

<span data-ttu-id="d0e59-178">A Historikus táblák kapcsolatos részletes információkért tekintse át [az MSDN dokumentációjában tájékozódhat](https://msdn.microsoft.com/library/dn935015.aspx).</span><span class="sxs-lookup"><span data-stu-id="d0e59-178">For detailed information about Temporal Tables, review [MSDN documentation](https://msdn.microsoft.com/library/dn935015.aspx).</span></span>

