---
title: "Az Azure SQL adatbázis memórián belüli technológiák |} Microsoft Docs"
description: "Az Azure SQL adatbázis memórián belüli technológiák jelentősen javítja a tranzakciós teljesítményét és elemzés munkaterhelések. Útmutató: ezek a technológiák előnyeit."
services: sql-database
documentationCenter: 
author: jodebrui
manager: jhubbard
editor: 
ms.assetid: 250ef341-90e5-492f-b075-b4750d237c05
ms.service: sql-database
ms.custom: develop databases
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: jodebrui
ms.openlocfilehash: 4cb45551c486263f26947e5684d54b4f2ecc7410
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="optimize-performance-by-using-in-memory-technologies-in-sql-database"></a><span data-ttu-id="35c75-104">A memórián belüli technológiái az SQL-adatbázis teljesítményének optimalizálása</span><span class="sxs-lookup"><span data-stu-id="35c75-104">Optimize performance by using In-Memory technologies in SQL Database</span></span>

<span data-ttu-id="35c75-105">A memórián belüli technológiái az Azure SQL Database, a különböző munkaterhelések teljesítménynövekedést érhet el: tranzakciós (online tranzakciós feldolgozási (OLTP)), elemzés (online analitikus feldolgozási (OLAP)), és a vegyes (hibrid tranzakció / analitikus feldolgozási (HTAP)).</span><span class="sxs-lookup"><span data-stu-id="35c75-105">By using In-Memory technologies in Azure SQL Database, you can achieve performance improvements with various workloads: transactional (online transactional processing (OLTP)), analytics (online analytical processing (OLAP)), and mixed (hybrid transaction/analytical processing (HTAP)).</span></span> <span data-ttu-id="35c75-106">Hatékonyabb lekérdezés és a tranzakció-feldolgozást memórián belüli technológiák is segítséget költségeinek csökkentése.</span><span class="sxs-lookup"><span data-stu-id="35c75-106">Because of the more efficient query and transaction processing, In-Memory technologies also help you to reduce cost.</span></span> <span data-ttu-id="35c75-107">Általában nem kell teljesítménynövekedéshez eléréséhez az adatbázis árképzési szintjének frissítése.</span><span class="sxs-lookup"><span data-stu-id="35c75-107">You typically don't need to upgrade the pricing tier of the database to achieve performance gains.</span></span> <span data-ttu-id="35c75-108">Néhány esetben is előfordulhat közben továbbra sem szűnik meg a memórián belüli szolgáltatáshoz. a teljesítménnyel kapcsolatos fejlesztések az árképzési szint csökkentése.</span><span class="sxs-lookup"><span data-stu-id="35c75-108">In some cases, you might even be able reduce the pricing tier, while still seeing performance improvements with In-Memory technologies.</span></span>

<span data-ttu-id="35c75-109">Az alábbiakban a két példa hogyan segített a memórián belüli online Tranzakciófeldolgozási jelentősen fejleszti a teljesítményt:</span><span class="sxs-lookup"><span data-stu-id="35c75-109">Here are two examples of how In-Memory OLTP helped to significantly improve performance:</span></span>

- <span data-ttu-id="35c75-110">A memórián belüli online Tranzakciófeldolgozási, [kvórum üzleti megoldások tudta 70 %-kal dtu-k fokozása mellett az alkalmazások és szolgáltatások duplán](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database).</span><span class="sxs-lookup"><span data-stu-id="35c75-110">By using In-Memory OLTP, [Quorum Business Solutions was able to double their workload while improving DTUs by 70%](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database).</span></span>
    - <span data-ttu-id="35c75-111">DTU azt jelenti, hogy *adatbázis-átviteli egység*, és egy az erőforrás-felhasználás mesurement tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="35c75-111">DTU means *database throughput unit*, and it includes a mesurement of resource consumption.</span></span>
- <span data-ttu-id="35c75-112">A következő videó bemutatja az erőforrás-felhasználást egy példa munkaterhelés jelentős fejlesztéseket: [memórián belüli online Tranzakciófeldolgozási az Azure SQL adatbázis Video](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB).</span><span class="sxs-lookup"><span data-stu-id="35c75-112">The following video demonstrates significant improvement in resource consumption with a sample workload: [In-Memory OLTP in Azure SQL Database Video](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB).</span></span>
    - <span data-ttu-id="35c75-113">További részletekért lásd a következő blogbejegyzésben: [memórián belüli online Tranzakciófeldolgozási az Azure SQL adatbázis Blog utáni](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)</span><span class="sxs-lookup"><span data-stu-id="35c75-113">For more details, see the blog post: [In-Memory OLTP in Azure SQL Database Blog Post](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)</span></span>

<span data-ttu-id="35c75-114">A memóriában technológiák minden adatbázisa prémium tarifacsomagra, beleértve az adatbázisok prémium rugalmas készletek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="35c75-114">In-Memory technologies are available in all databases in the Premium tier, including databases in Premium elastic pools.</span></span>

<span data-ttu-id="35c75-115">A következő videó ismerteti a lehetséges teljesítménynövekedést érhet el, a memórián belüli szolgáltatáshoz. az Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="35c75-115">The following video explains potential performance gains with In-Memory technologies in Azure SQL Database.</span></span> <span data-ttu-id="35c75-116">Ne feledje, hogy jobb a teljesítménye, hogy mindig számos tényezőtől függ, többek között a munkaterhelés és adatok, az adatbázis minták, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="35c75-116">Remember that the performance gain that you see always depends on many factors, including the nature of the workload and data, access pattern of the database, and so on.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-In-Memory-Technologies/player]
>
>

<span data-ttu-id="35c75-117">Az Azure SQL-adatbázis a következő memórián belüli technológiákat rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="35c75-117">Azure SQL Database has the following In-Memory technologies:</span></span>

- <span data-ttu-id="35c75-118">*A memórián belüli online Tranzakciófeldolgozási* növeli az adatátviteli sebességet, és csökkenti a késést tranzakció feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="35c75-118">*In-Memory OLTP* increases throughput and reduces latency for transaction processing.</span></span> <span data-ttu-id="35c75-119">A memórián belüli online Tranzakciófeldolgozási előnyeit kihasználó forgatókönyvek a következők: nagy átviteli tranzakció, például kereskedelmi és játékokban, adatfeldolgozást események vagy az IoT-eszközök, gyorsítótárazás, az adatok betöltését, és az ideiglenes tábla és a tábla változó forgatókönyvek feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="35c75-119">Scenarios that benefit from In-Memory OLTP are: high-throughput transaction processing such as trading and gaming, data ingestion from events or IoT devices, caching, data load, and temporary table and table variable scenarios.</span></span>
- <span data-ttu-id="35c75-120">*Fürtözött oszlopcentrikus indexek* csökkenti a tárolási erőforrásigényét (legfeljebb 10-szer) és a jelentéskészítési és elemzési lekérdezések teljesítményének javítása.</span><span class="sxs-lookup"><span data-stu-id="35c75-120">*Clustered columnstore indexes* reduce your storage footprint (up to 10 times) and improve performance for reporting and analytics queries.</span></span> <span data-ttu-id="35c75-121">Segítségével azt a ténytáblák az adatpiac jelenti az igényei több adatot az adatbázishoz, és javíthatja a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="35c75-121">You can use it with fact tables in your data marts to fit more data in your database and improve performance.</span></span> <span data-ttu-id="35c75-122">Is segítségével, és az előzmények az operatív adatbázis archivált, és akár 10-szer több adat lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="35c75-122">Also, you can use it with historical data in your operational database to archive and be able to query up to 10 times more data.</span></span>
- <span data-ttu-id="35c75-123">*Nem fürtözött oszloptárindex* HTAP segítséget kaphat a valós idejű az operatív adatbázis közvetlen lekérdezése, olcsóbbá kivonatot futtatásának szükségessége nélkül a vállalatra vonatkozó átalakító, és (ETL) folyamat betölteni, és várja meg a az adatraktár kell feltöltenie.</span><span class="sxs-lookup"><span data-stu-id="35c75-123">*Nonclustered columnstore indexes* for HTAP help you to gain real-time insights into your business through querying the operational database directly, without the need to run an expensive extract, transform, and load (ETL) process and wait for the data warehouse to be populated.</span></span> <span data-ttu-id="35c75-124">Nem fürtözött oszloptárindex OLTP adatbázis nagyon gyorsan elemzési lekérdezések végrehajtása közben csökkenti a működési munkaterhelés gyakorolt hatás engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="35c75-124">Nonclustered columnstore indexes allow very fast execution of analytics queries on the OLTP database, while reducing the impact on the operational workload.</span></span>
- <span data-ttu-id="35c75-125">A oszlopcentrikus indexszel rendelkező memóriaoptimalizált táblák kombinációja is lehet.</span><span class="sxs-lookup"><span data-stu-id="35c75-125">You can also have the combination of a memory-optimized table with a columnstore index.</span></span> <span data-ttu-id="35c75-126">Ez a kombináció lehetővé teszi a rendkívül gyors tranzakció-feldolgozást, és a *egyidejűleg* elemzési lekérdezések nagyon gyorsan futtatnak ugyanazokat az adatokat.</span><span class="sxs-lookup"><span data-stu-id="35c75-126">This combination enables you to perform very fast transaction processing, and to *concurrently* run analytics queries very quickly on the same data.</span></span>

<span data-ttu-id="35c75-127">Oszlopcentrikus indexek és a memórián belüli online Tranzakciófeldolgozási óta SQL Server termékhez 2012 és a 2014, illetve.</span><span class="sxs-lookup"><span data-stu-id="35c75-127">Both columnstore indexes and In-Memory OLTP have been part of the SQL Server product since 2012 and 2014, respectively.</span></span> <span data-ttu-id="35c75-128">Az Azure SQL Database és SQL Server megosztani a memórián belüli technológiák azonos végrehajtására.</span><span class="sxs-lookup"><span data-stu-id="35c75-128">Azure SQL Database and SQL Server share the same implementation of In-Memory technologies.</span></span> <span data-ttu-id="35c75-129">Következő lépésként ezek a technológiák új lehetőségek kiadott az Azure SQL Database először előtt a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="35c75-129">Going forward, new capabilities for these technologies are released in Azure SQL Database first, before they are released in SQL Server.</span></span>

<span data-ttu-id="35c75-130">Ez a témakör a memórián belüli online Tranzakciófeldolgozási és oszlopcentrikus indexek, az Azure SQL Database vonatkozó szempontokat ismerteti, és minták is tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="35c75-130">This topic describes aspects of In-Memory OLTP and columnstore indexes that are specific to Azure SQL Database and also includes samples:</span></span>
- <span data-ttu-id="35c75-131">A tárolási és adatok méretkorlátait látni fogja, ezek a technológiák hatását.</span><span class="sxs-lookup"><span data-stu-id="35c75-131">You'll see the impact of these technologies on storage and data size limits.</span></span>
- <span data-ttu-id="35c75-132">Láthatja, hogy ezek a technológiák között a különböző árképzési szinteket használó adatbázisok mozgása kezelése.</span><span class="sxs-lookup"><span data-stu-id="35c75-132">You'll see how to manage the movement of databases that use these technologies between the different pricing tiers.</span></span>
- <span data-ttu-id="35c75-133">Látni fogja, hogy bemutassa a memórián belüli online Tranzakciófeldolgozási, valamint az Azure SQL Database oszlopcentrikus indexek használata két minta.</span><span class="sxs-lookup"><span data-stu-id="35c75-133">You'll see two samples that illustrate the use of In-Memory OLTP, as well as columnstore indexes in Azure SQL Database.</span></span>

<span data-ttu-id="35c75-134">További információ a következő forrásanyagokban talál.</span><span class="sxs-lookup"><span data-stu-id="35c75-134">See the following resources for more information.</span></span>

<span data-ttu-id="35c75-135">A technológiákkal kapcsolatos részletesebb információk:</span><span class="sxs-lookup"><span data-stu-id="35c75-135">In-depth information about the technologies:</span></span>

- <span data-ttu-id="35c75-136">[Memórián belüli online Tranzakciófeldolgozási áttekintése és a használati forgatókönyvek](https://msdn.microsoft.com/library/mt774593.aspx) (tartalmazza a felhasználói esettanulmányok és információk első lépések)</span><span class="sxs-lookup"><span data-stu-id="35c75-136">[In-Memory OLTP Overview and Usage Scenarios](https://msdn.microsoft.com/library/mt774593.aspx) (includes references to customer case studies and information to get started)</span></span>
- [<span data-ttu-id="35c75-137">A memórián belüli online Tranzakciófeldolgozási dokumentációja</span><span class="sxs-lookup"><span data-stu-id="35c75-137">Documentation for In-Memory OLTP</span></span>](http://msdn.microsoft.com/library/dn133186.aspx)
- [<span data-ttu-id="35c75-138">Oszlopcentrikus indexek útmutató</span><span class="sxs-lookup"><span data-stu-id="35c75-138">Columnstore Indexes Guide</span></span>](https://msdn.microsoft.com/library/gg492088.aspx)
- <span data-ttu-id="35c75-139">Hibrid tranzakciós/analitikus feldolgozási (HTAP), más néven [valós idejű működési elemzés](https://msdn.microsoft.com/library/dn817827.aspx)</span><span class="sxs-lookup"><span data-stu-id="35c75-139">Hybrid transactional/analytical processing (HTAP), also known as [real-time operational analytics](https://msdn.microsoft.com/library/dn817827.aspx)</span></span>

<span data-ttu-id="35c75-140">Gyors segítséget nyújthat a memórián belüli online Tranzakciófeldolgozási: [Quick Start 1: memórián belüli online Tranzakciófeldolgozási technológiák gyorsabb a T-SQL teljesítmény](http://msdn.microsoft.com/library/mt694156.aspx) (segítségével egy másik cikkben első lépések)</span><span class="sxs-lookup"><span data-stu-id="35c75-140">A quick primer on In-Memory OLTP: [Quick Start 1: In-Memory OLTP Technologies for Faster T-SQL Performance](http://msdn.microsoft.com/library/mt694156.aspx) (another article to help you get started)</span></span>

<span data-ttu-id="35c75-141">Részletes videók technológiákkal kapcsolatos:</span><span class="sxs-lookup"><span data-stu-id="35c75-141">In-depth videos about the technologies:</span></span>

- <span data-ttu-id="35c75-142">[Memórián belüli online Tranzakciófeldolgozási az Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB) (amely tartalmazza a bemutató jobb teljesítményt nyújt, és ezekkel az eredményekkel saját kezűleg reprodukálásához szükséges lépések)</span><span class="sxs-lookup"><span data-stu-id="35c75-142">[In-Memory OLTP in Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB) (which contains a demo of performance benefits and steps to reproduce these results yourself)</span></span>
- [<span data-ttu-id="35c75-143">Memórián belüli online Tranzakciófeldolgozási videók: Mi van, és ha/hogyan a használatára</span><span class="sxs-lookup"><span data-stu-id="35c75-143">In-Memory OLTP Videos: What it is and When/How to use it</span></span>](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/10/03/in-memory-oltp-video-what-it-is-and-whenhow-to-use-it/)
- [<span data-ttu-id="35c75-144">Az Oszloptárindex: A memórián belüli Analytics videók Ignite 2016</span><span class="sxs-lookup"><span data-stu-id="35c75-144">Columnstore Index: In-Memory Analytics Videos from Ignite 2016</span></span>](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/10/04/columnstore-index-in-memory-analytics-i-e-columnstore-index-videos-from-ignite-2016/)

## <a name="storage-and-data-size"></a><span data-ttu-id="35c75-145">Tárolás és az adatok mérete</span><span class="sxs-lookup"><span data-stu-id="35c75-145">Storage and data size</span></span>

### <a name="data-size-and-storage-cap-for-in-memory-oltp"></a><span data-ttu-id="35c75-146">A memórián belüli online Tranzakciófeldolgozási az adatok méretét és a tárolási cap</span><span class="sxs-lookup"><span data-stu-id="35c75-146">Data size and storage cap for In-Memory OLTP</span></span>

<span data-ttu-id="35c75-147">A memórián belüli online Tranzakciófeldolgozási memóriaoptimalizált táblákkal, felhasználói adatok tárolásához használt tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="35c75-147">In-Memory OLTP includes memory-optimized tables, which are used for storing user data.</span></span> <span data-ttu-id="35c75-148">Ezek a táblázatok memóriában szükségesek.</span><span class="sxs-lookup"><span data-stu-id="35c75-148">These tables are required to fit in memory.</span></span> <span data-ttu-id="35c75-149">Kezelheti a memória közvetlenül az SQL Database szolgáltatásban, mert a felhasználói adatok egy kvótát fogalmát van.</span><span class="sxs-lookup"><span data-stu-id="35c75-149">Because you manage memory directly in the SQL Database service, we have the  concept of a quota for user data.</span></span> <span data-ttu-id="35c75-150">Ezzel az ötlettel nevezzük *memórián belüli online Tranzakciófeldolgozási tárolási*.</span><span class="sxs-lookup"><span data-stu-id="35c75-150">This idea is referred to as *In-Memory OLTP storage*.</span></span>

<span data-ttu-id="35c75-151">Minden tarifacsomag és minden rugalmas készlet árképzési szint támogatott önálló adatbázis bizonyos mennyiségű memórián belüli online Tranzakciófeldolgozási tárolási tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="35c75-151">Each supported standalone database pricing tier and each elastic pool pricing tier includes a certain amount of In-Memory OLTP storage.</span></span> <span data-ttu-id="35c75-152">Írásának időpontjában a tárolási gigabájt minden 125 adatbázis-tranzakciós egységek (dtu-i) vagy a rugalmas adatbázis-tranzakciós egységek (edtu-k) beolvasása.</span><span class="sxs-lookup"><span data-stu-id="35c75-152">At the time of writing, you get a gigabyte of storage for every 125 database transaction units (DTUs) or elastic database transaction units (eDTUs).</span></span>

<span data-ttu-id="35c75-153">A [SQL adatbázis szolgáltatásszintjeinek](sql-database-service-tiers.md) cikk rendelkezik a memórián belüli online Tranzakciófeldolgozási elérhető tároló minden támogatott önálló adatbázis és a rugalmas készlet árképzési szint hivatalos listája.</span><span class="sxs-lookup"><span data-stu-id="35c75-153">The [SQL Database service tiers](sql-database-service-tiers.md) article has the official list of the In-Memory OLTP storage that is available for each supported standalone database and elastic pool pricing tier.</span></span>

<span data-ttu-id="35c75-154">A következő elemek felé a memórián belüli online Tranzakciófeldolgozási tároló maximális száma:</span><span class="sxs-lookup"><span data-stu-id="35c75-154">The following items count toward your In-Memory OLTP storage cap:</span></span>

- <span data-ttu-id="35c75-155">A memóriaoptimalizált táblák és Táblaváltozók adatsorok aktív felhasználónként.</span><span class="sxs-lookup"><span data-stu-id="35c75-155">Active user data rows in memory-optimized tables and table variables.</span></span> <span data-ttu-id="35c75-156">Vegye figyelembe, hogy a régi sor verziók száma nem a kap felé.</span><span class="sxs-lookup"><span data-stu-id="35c75-156">Note that old row versions don't count toward the cap.</span></span>
- <span data-ttu-id="35c75-157">A memóriaoptimalizált táblák indexei.</span><span class="sxs-lookup"><span data-stu-id="35c75-157">Indexes on memory-optimized tables.</span></span>
- <span data-ttu-id="35c75-158">Az ALTER TABLE műveletek működési munkaterhelés.</span><span class="sxs-lookup"><span data-stu-id="35c75-158">Operational overhead of ALTER TABLE operations.</span></span>

<span data-ttu-id="35c75-159">Elérte a kap, ha ki a kvóta hibaüzenetet kap, és már nem tudunk beszúrásához vagy frissítéséhez adatokat.</span><span class="sxs-lookup"><span data-stu-id="35c75-159">If you hit the cap, you receive an out-of-quota error, and you are no longer able to insert or update data.</span></span> <span data-ttu-id="35c75-160">Ez a hiba elhárítása érdekében, törli az adatokat, vagy növelje az adatbázis vagy a készlet tarifacsomagját.</span><span class="sxs-lookup"><span data-stu-id="35c75-160">To mitigate this error, delete data or increase the pricing tier of the database or pool.</span></span>

<span data-ttu-id="35c75-161">További információk a tárhely kihasználtságát a memórián belüli online Tranzakciófeldolgozási figyelése és a riasztások konfigurálása, ha majdnem elérte a maximális: [figyelő memórián belüli tároló](sql-database-in-memory-oltp-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="35c75-161">For details about monitoring In-Memory OLTP storage utilization and configuring alerts when you almost hit the cap, see [Monitor In-Memory storage](sql-database-in-memory-oltp-monitoring.md).</span></span>

#### <a name="about-elastic-pools"></a><span data-ttu-id="35c75-162">Tudnivalók a rugalmas készletek</span><span class="sxs-lookup"><span data-stu-id="35c75-162">About elastic pools</span></span>

<span data-ttu-id="35c75-163">A rugalmas készletek a memórián belüli online Tranzakciófeldolgozási tárolási a készletben lévő összes adatbázisok által megosztott.</span><span class="sxs-lookup"><span data-stu-id="35c75-163">With elastic pools, the In-Memory OLTP storage is shared across all databases in the pool.</span></span> <span data-ttu-id="35c75-164">Ezért a használata egy adatbázis hatással lehet más adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="35c75-164">Therefore, the usage in one database can potentially affect other databases.</span></span> <span data-ttu-id="35c75-165">Ez a két megoldást a következők:</span><span class="sxs-lookup"><span data-stu-id="35c75-165">Two mitigations for this are:</span></span>

- <span data-ttu-id="35c75-166">Állítsa a Max-edtu-k az adatbázisok, amely kisebb, mint egy teljes készlet edtu-k számát.</span><span class="sxs-lookup"><span data-stu-id="35c75-166">Configure a Max-eDTU for databases that is lower than the eDTU count for the pool as a whole.</span></span> <span data-ttu-id="35c75-167">A maximális caps a memórián belüli online Tranzakciófeldolgozási tárhely kihasználtságát, a készlet méretét, amely megfelel az edtu-k száma bármely adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="35c75-167">This maximum caps the In-Memory OLTP storage utilization, in any database in the pool, to the size that corresponds to the eDTU count.</span></span>
- <span data-ttu-id="35c75-168">Állítsa a Min-edtu-k, amely nagyobb, mint 0.</span><span class="sxs-lookup"><span data-stu-id="35c75-168">Configure a Min-eDTU that is greater than 0.</span></span> <span data-ttu-id="35c75-169">Ez a minimum garantálja, hogy a készlet minden egyes adatbázis rendelkezik-e elérhető a memórián belüli online Tranzakciófeldolgozási tárolókapacitást, amely megfelel a konfigurált minimális eDTU.</span><span class="sxs-lookup"><span data-stu-id="35c75-169">This minimum guarantees that each database in the pool has the amount of available In-Memory OLTP storage that corresponds to the configured Min-eDTU.</span></span>

### <a name="data-size-and-storage-for-columnstore-indexes"></a><span data-ttu-id="35c75-170">Adatok mérete és az oszlopcentrikus indexek tároló</span><span class="sxs-lookup"><span data-stu-id="35c75-170">Data size and storage for columnstore indexes</span></span>

<span data-ttu-id="35c75-171">Oszlopcentrikus indexek nem szükséges a memóriában.</span><span class="sxs-lookup"><span data-stu-id="35c75-171">Columnstore indexes aren't required to fit in memory.</span></span> <span data-ttu-id="35c75-172">A csak kap az indexek mérete ezért a maximális általános adatbázis méretet, amelyet dokumentációja a [SQL adatbázis szolgáltatásszintjeinek](sql-database-service-tiers.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="35c75-172">Therefore, the only cap on the size of the indexes is the maximum overall database size, which is documented in the [SQL Database service tiers](sql-database-service-tiers.md) article.</span></span>

<span data-ttu-id="35c75-173">Fürtözött oszlopcentrikus indexek használata esetén a oszlopos tömörítési alaptábla tárolására szolgál.</span><span class="sxs-lookup"><span data-stu-id="35c75-173">When you use clustered columnstore indexes, columnar compression is used for the base table storage.</span></span> <span data-ttu-id="35c75-174">Ez a fajta tömörítés jelentősen csökkenti a tárolási kezdjen a felhasználói adatok, ami azt jelenti, hogy több adatot elfér az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="35c75-174">This compression can significantly reduce the storage footprint of your user data, which means that you can fit more data in the database.</span></span> <span data-ttu-id="35c75-175">A tömörítés további növelni a [oszlopos archiválási tömörítési](https://msdn.microsoft.com/library/cc280449.aspx#Using Columnstore and Columnstore Archive Compression).</span><span class="sxs-lookup"><span data-stu-id="35c75-175">And the compression can be further increased with [columnar archival compression](https://msdn.microsoft.com/library/cc280449.aspx#Using Columnstore and Columnstore Archive Compression).</span></span> <span data-ttu-id="35c75-176">Az adatok jellegétől függ érhet el tömörítés mértéke, de 10-szer a tömörítés nem ritka.</span><span class="sxs-lookup"><span data-stu-id="35c75-176">The amount of compression that you can achieve depends on the nature of the data, but 10 times the compression is not uncommon.</span></span>

<span data-ttu-id="35c75-177">Például ha egy adatbázist, mely legfeljebb 1 terabájtnál (TB) és a tömörítést 10-szer oszlopcentrikus indexek használatával érhetők el, is elférjen összesen 10 TB-nyi felhasználói adatokat az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="35c75-177">For example, if you have a database with a maximum size of 1 terabyte (TB) and you achieve 10 times the compression by using columnstore indexes, you can fit a total of 10 TB of user data in the database.</span></span>

<span data-ttu-id="35c75-178">Fürtözetlen oszlopcentrikus indexek használata esetén a következő alaptáblában továbbra is a hagyományos sortárindex formátum tárolódik.</span><span class="sxs-lookup"><span data-stu-id="35c75-178">When you use nonclustered columnstore indexes, the base table is still stored in the traditional rowstore format.</span></span> <span data-ttu-id="35c75-179">Ezért a tárhely-megtakarítást, nagy, mint a fürtözött oszlopcentrikus indexek nem.</span><span class="sxs-lookup"><span data-stu-id="35c75-179">Therefore, the storage savings aren't as big as with clustered columnstore indexes.</span></span> <span data-ttu-id="35c75-180">Azonban ha egy hagyományos fürtözött indexek száma a egyetlen oszlopcentrikus indexszel rendelkező, továbbra is láthatja a tárolási kezdjen a következő táblázatban található az általános megtakarítások.</span><span class="sxs-lookup"><span data-stu-id="35c75-180">However, if you're replacing a number of traditional nonclustered indexes with a single columnstore index, you can still see an overall savings in the storage footprint for the table.</span></span>

## <a name="moving-databases-that-use-in-memory-technologies-between-pricing-tiers"></a><span data-ttu-id="35c75-181">A memóriában technológiák közötti árképzési szinteket használó adatbázisok áthelyezése</span><span class="sxs-lookup"><span data-stu-id="35c75-181">Moving databases that use In-Memory technologies between pricing tiers</span></span>

<span data-ttu-id="35c75-182">Nincsenek soha nem azonosított inkompatibilitásokat vagy egyéb problémák történő frissítésekor egy magasabb szintű tarifacsomagban használható, többek között a Premium szabvány.</span><span class="sxs-lookup"><span data-stu-id="35c75-182">There are never any incompatibilities or other problems when you upgrade to a higher pricing tier, such as from Standard to Premium.</span></span> <span data-ttu-id="35c75-183">Az elérhető funkciókat és erőforrások csak növelni.</span><span class="sxs-lookup"><span data-stu-id="35c75-183">The available functionality and resources only increase.</span></span>

<span data-ttu-id="35c75-184">De alacsonyabb verziójúra változtatása az árképzési szint negatív hatással lehet az adatbázis.</span><span class="sxs-lookup"><span data-stu-id="35c75-184">But downgrading the pricing tier can negatively impact your database.</span></span> <span data-ttu-id="35c75-185">A hatás különösen kétségtelenül, amikor Ön visszaminősítését prémiumról alapszintű vagy Standard amikor az adatbázis memórián belüli online Tranzakciófeldolgozási objektumokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="35c75-185">The impact is especially apparent when you downgrade from Premium to Standard or Basic when your database contains In-Memory OLTP objects.</span></span> <span data-ttu-id="35c75-186">Memóriaoptimalizált táblákkal, és oszlopcentrikus indexek esetében nem érhetők el az alacsonyabb szintre való visszalépést után (még akkor is, ha akkor is látható maradjon,).</span><span class="sxs-lookup"><span data-stu-id="35c75-186">Memory-optimized tables, and columnstore indexes, are unavailable after the downgrade (even if they remain visible).</span></span> <span data-ttu-id="35c75-187">Ugyanazok a feltételek vonatkoznak, amikor egy rugalmas készlet árképzési szintjének csökkentése, vagy egy adatbázis áthelyezése a szolgáltatáshoz. A memóriában, alapszintű vagy Standard rugalmas készlet.</span><span class="sxs-lookup"><span data-stu-id="35c75-187">The same considerations apply when you're lowering the pricing tier of an elastic pool, or moving a database with In-Memory technologies, into a Standard or Basic elastic pool.</span></span>

### <a name="in-memory-oltp"></a><span data-ttu-id="35c75-188">Memóriabeli OLTP</span><span class="sxs-lookup"><span data-stu-id="35c75-188">In-Memory OLTP</span></span>

<span data-ttu-id="35c75-189">*A Basic vagy Standard alacsonyabb verziójúra változtatása*: memórián belüli online Tranzakciófeldolgozási a Standard vagy alapszintű rétegben adatbázisokban nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="35c75-189">*Downgrading to Basic/Standard*: In-Memory OLTP isn't supported in databases in the Standard or Basic tier.</span></span> <span data-ttu-id="35c75-190">Emellett nem lehet áthelyezni egy adatbázist, amelynek a Standard vagy Basic réteghez memórián belüli online Tranzakciófeldolgozási objektumokat.</span><span class="sxs-lookup"><span data-stu-id="35c75-190">In addition, it isn't possible to move a database that has any In-Memory OLTP objects to the Standard or Basic tier.</span></span>

<span data-ttu-id="35c75-191">Előtt visszaminősítését az adatbázis Standard/egyszerű, távolítsa el az összes memóriaoptimalizált táblák és táblatípusokban, valamint a T-SQL minden natív módon lefordított modulok.</span><span class="sxs-lookup"><span data-stu-id="35c75-191">Before you downgrade the database to Standard/Basic, remove all memory-optimized tables and table types, as well as all natively compiled T-SQL modules.</span></span>

<span data-ttu-id="35c75-192">Nincs olyan tudni, hogy egy adott adatbázisnak támogatja-e a memórián belüli online Tranzakciófeldolgozási programozott módon.</span><span class="sxs-lookup"><span data-stu-id="35c75-192">There is a programmatic way to understand whether a given database supports In-Memory OLTP.</span></span> <span data-ttu-id="35c75-193">A következő Transact-SQL-lekérdezés hajthat végre:</span><span class="sxs-lookup"><span data-stu-id="35c75-193">You can execute the following Transact-SQL query:</span></span>

```
SELECT DatabasePropertyEx(DB_NAME(), 'IsXTPSupported');
```

<span data-ttu-id="35c75-194">Ha a lekérdezés visszaadja az **1**, a memórián belüli online Tranzakciófeldolgozási támogatott ebben az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="35c75-194">If the query returns **1**, In-Memory OLTP is supported in this database.</span></span>


<span data-ttu-id="35c75-195">*Egy alacsonyabb prémium csomagra alacsonyabb verziójúra változtatása*: memóriaoptimalizált táblázatok adatait hozzá kell férnie a memórián belüli online Tranzakciófeldolgozási tárolóban, amely az adatbázis árképzési szintjének társított vagy érhető el a rugalmas készletben.</span><span class="sxs-lookup"><span data-stu-id="35c75-195">*Downgrading to a lower Premium tier*: Data in memory-optimized tables must fit within the In-Memory OLTP storage that is associated with the pricing tier of the database or is available in the elastic pool.</span></span> <span data-ttu-id="35c75-196">Ha kísérli meg az árképzési szint csökkentése, vagy az adatbázist áthelyezi az a készletbe, amely nem rendelkezik elegendő memórián belüli online Tranzakciófeldolgozási tárolóhellyel, a művelet sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="35c75-196">If you try to lower the pricing tier or move the database into a pool that doesn't have enough available In-Memory OLTP storage, the operation fails.</span></span>

### <a name="columnstore-indexes"></a><span data-ttu-id="35c75-197">Oszlopcentrikus indexek</span><span class="sxs-lookup"><span data-stu-id="35c75-197">Columnstore indexes</span></span>

<span data-ttu-id="35c75-198">*Basic vagy Standard visszaminősítése*: Oszlopcentrikus indexek használata támogatott, csak a prémium tarifacsomag, nem pedig a Standard vagy Basic rétegek.</span><span class="sxs-lookup"><span data-stu-id="35c75-198">*Downgrading to Basic or Standard*: Columnstore indexes are supported only on the Premium pricing tier, and not on the Standard or Basic tiers.</span></span> <span data-ttu-id="35c75-199">Amikor visszaminősítését alapszintű vagy Standard az adatbázist, az oszlopcentrikus index nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="35c75-199">When you downgrade your database to Standard or Basic, your columnstore index becomes unavailable.</span></span> <span data-ttu-id="35c75-200">A rendszer megőrzi az oszlopcentrikus index, de soha ne használja a az index.</span><span class="sxs-lookup"><span data-stu-id="35c75-200">The system maintains your columnstore index, but it never leverages the index.</span></span> <span data-ttu-id="35c75-201">Ha később frissíteni vissza Premium, az oszlopcentrikus index azonnal készen áll a újra javítható.</span><span class="sxs-lookup"><span data-stu-id="35c75-201">If you later upgrade back to Premium, your columnstore index is immediately ready to be leveraged again.</span></span>

<span data-ttu-id="35c75-202">Ha rendelkezik egy **fürtözött** oszlopcentrikus indexet, az egész tábla nem érhető el réteg alacsonyabb szintre való visszalépést után.</span><span class="sxs-lookup"><span data-stu-id="35c75-202">If you have a **clustered** columnstore index, the whole table becomes unavailable after tier downgrade.</span></span> <span data-ttu-id="35c75-203">Ezért ajánlott minden drop *fürtözött* oszlopcentrikus indexeket előtt meg megállapításában, hogy az adatbázis alatt prémium tarifacsomagra.</span><span class="sxs-lookup"><span data-stu-id="35c75-203">Therefore we recommend that you drop all *clustered* columnstore indexes before you downgrade your database below the Premium tier.</span></span>

<span data-ttu-id="35c75-204">*Egy alacsonyabb prémium csomagra alacsonyabb verziójúra változtatása*: az alacsonyabb szintre való visszalépést sikeres lesz, ha a teljes adatbázis megfelel a cél IP-címek a maximális méretét, vagy a rendelkezésre álló tár az a rugalmas készlet belül.</span><span class="sxs-lookup"><span data-stu-id="35c75-204">*Downgrading to a lower Premium tier*: This downgrade succeeds if the whole database fits within the maximum database size for the target pricing tier, or within the available storage in the elastic pool.</span></span> <span data-ttu-id="35c75-205">Nincs az oszlopcentrikus indexek az adott hatással.</span><span class="sxs-lookup"><span data-stu-id="35c75-205">There is no specific impact from the columnstore indexes.</span></span>


<a id="install_oltp_manuallink" name="install_oltp_manuallink"></a>

&nbsp;

## <a name="1-install-the-in-memory-oltp-sample"></a><span data-ttu-id="35c75-206">1. A memórián belüli online Tranzakciófeldolgozási minta telepítése</span><span class="sxs-lookup"><span data-stu-id="35c75-206">1. Install the In-Memory OLTP sample</span></span>

<span data-ttu-id="35c75-207">A AdventureWorksLT mintaadatbázis mindössze néhány kattintással hozhatja létre a [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="35c75-207">You can create the AdventureWorksLT sample database with a few clicks in the [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="35c75-208">A jelen szakaszban szereplő lépéseket, majd azt ismertetik, hogyan a memórián belüli online Tranzakciófeldolgozási objektumok AdventureWorksLT adatbázis kiegészítése, és bemutatják a teljesítménybeli előnyökben.</span><span class="sxs-lookup"><span data-stu-id="35c75-208">Then, the steps in this section explain how you can enrich your AdventureWorksLT database with In-Memory OLTP objects and demonstrate performance benefits.</span></span>

<span data-ttu-id="35c75-209">A több simplistic, de több tetszetős teljesítmény bemutató a memórián belüli online Tranzakciófeldolgozási lásd:</span><span class="sxs-lookup"><span data-stu-id="35c75-209">For a more simplistic, but more visually appealing performance demo for In-Memory OLTP, see:</span></span>

- <span data-ttu-id="35c75-210">Kiadás: [a-memória-oltp-bemutató-1.0-s verzió](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)</span><span class="sxs-lookup"><span data-stu-id="35c75-210">Release: [in-memory-oltp-demo-v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)</span></span>
- <span data-ttu-id="35c75-211">Forráskód: [in-memory-oltp-demo-source-code](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/in-memory/ticket-reservations)</span><span class="sxs-lookup"><span data-stu-id="35c75-211">Source code: [in-memory-oltp-demo-source-code](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/in-memory/ticket-reservations)</span></span>

#### <a name="installation-steps"></a><span data-ttu-id="35c75-212">Telepítés lépései</span><span class="sxs-lookup"><span data-stu-id="35c75-212">Installation steps</span></span>

1. <span data-ttu-id="35c75-213">Az a [Azure-portálon](https://portal.azure.com/), Premium adatbázis létrehozása a kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="35c75-213">In the [Azure portal](https://portal.azure.com/), create a Premium database on a server.</span></span> <span data-ttu-id="35c75-214">Állítsa be a **forrás** AdventureWorksLT minta adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="35c75-214">Set the **Source** to the AdventureWorksLT sample database.</span></span> <span data-ttu-id="35c75-215">Részletes útmutatásért lásd: [az első Azure SQL-adatbázis létrehozása](sql-database-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="35c75-215">For detailed instructions, see [Create your first Azure SQL database](sql-database-get-started-portal.md).</span></span>

2. <span data-ttu-id="35c75-216">Kapcsolódni az adatbázishoz az SQL Server Management Studio [(SSMS.exe)](http://msdn.microsoft.com/library/mt238290.aspx).</span><span class="sxs-lookup"><span data-stu-id="35c75-216">Connect to the database with SQL Server Management Studio [(SSMS.exe)](http://msdn.microsoft.com/library/mt238290.aspx).</span></span>

3. <span data-ttu-id="35c75-217">Másolás a [memórián belüli online Tranzakciófeldolgozási Transact-SQL parancsfájl](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_oltp_sample.sql) a vágólapra.</span><span class="sxs-lookup"><span data-stu-id="35c75-217">Copy the [In-Memory OLTP Transact-SQL script](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_oltp_sample.sql) to your clipboard.</span></span> <span data-ttu-id="35c75-218">A T-SQL parancsfájlt a szükséges memórián belüli objektumok a AdventureWorksLT mintaadatbázis az 1. lépésben létrehozott hoz létre.</span><span class="sxs-lookup"><span data-stu-id="35c75-218">The T-SQL script creates the necessary In-Memory objects in the AdventureWorksLT sample database that you created in step 1.</span></span>

4. <span data-ttu-id="35c75-219">Illessze be a T-SQL parancsfájl SSMS, és majd végrehajtani a parancsprogramot.</span><span class="sxs-lookup"><span data-stu-id="35c75-219">Paste the T-SQL script into SSMS, and then execute the script.</span></span> <span data-ttu-id="35c75-220">A `MEMORY_OPTIMIZED = ON` záradék CREATE TABLE utasítás fontosságúak.</span><span class="sxs-lookup"><span data-stu-id="35c75-220">The `MEMORY_OPTIMIZED = ON` clause CREATE TABLE statements are crucial.</span></span> <span data-ttu-id="35c75-221">Példa:</span><span class="sxs-lookup"><span data-stu-id="35c75-221">For example:</span></span>


```
CREATE TABLE [SalesLT].[SalesOrderHeader_inmem](
    [SalesOrderID] int IDENTITY NOT NULL PRIMARY KEY NONCLUSTERED ...,
    ...
) WITH (MEMORY_OPTIMIZED = ON);
```


#### <a name="error-40536"></a><span data-ttu-id="35c75-222">Hiba 40536</span><span class="sxs-lookup"><span data-stu-id="35c75-222">Error 40536</span></span>


<span data-ttu-id="35c75-223">Ha hiba 40536 a T-SQL parancsfájl futtatásakor, futtassa a következő T-SQL parancsfájlt, és győződjön meg arról, hogy az adatbázis támogatja-e a memóriában lévő:</span><span class="sxs-lookup"><span data-stu-id="35c75-223">If you get error 40536 when you run the T-SQL script, run the following T-SQL script to verify whether the database supports In-Memory:</span></span>


```
SELECT DatabasePropertyEx(DB_Name(), 'IsXTPSupported');
```


<span data-ttu-id="35c75-224">Eredménye **0** azt jelenti, hogy a memóriában nem támogatott, és **1** azt jelenti, hogy esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="35c75-224">A result of **0** means that In-Memory isn't supported, and **1** means that it is supported.</span></span> <span data-ttu-id="35c75-225">A probléma diagnosztizálása érdekében győződjön meg arról, hogy az adatbázis, a prémium szolgáltatásszintet.</span><span class="sxs-lookup"><span data-stu-id="35c75-225">To diagnose the problem, ensure that the database is at the Premium service tier.</span></span>


#### <a name="about-the-created-memory-optimized-items"></a><span data-ttu-id="35c75-226">A létrehozott memóriaoptimalizált elemek kapcsolatos</span><span class="sxs-lookup"><span data-stu-id="35c75-226">About the created memory-optimized items</span></span>

<span data-ttu-id="35c75-227">**Táblák**: A minta a következő memóriaoptimalizált táblákat tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="35c75-227">**Tables**: The sample contains the following memory-optimized tables:</span></span>

- <span data-ttu-id="35c75-228">SalesLT.Product_inmem</span><span class="sxs-lookup"><span data-stu-id="35c75-228">SalesLT.Product_inmem</span></span>
- <span data-ttu-id="35c75-229">SalesLT.SalesOrderHeader_inmem</span><span class="sxs-lookup"><span data-stu-id="35c75-229">SalesLT.SalesOrderHeader_inmem</span></span>
- <span data-ttu-id="35c75-230">SalesLT.SalesOrderDetail_inmem</span><span class="sxs-lookup"><span data-stu-id="35c75-230">SalesLT.SalesOrderDetail_inmem</span></span>
- <span data-ttu-id="35c75-231">Demo.DemoSalesOrderHeaderSeed</span><span class="sxs-lookup"><span data-stu-id="35c75-231">Demo.DemoSalesOrderHeaderSeed</span></span>
- <span data-ttu-id="35c75-232">Demo.DemoSalesOrderDetailSeed</span><span class="sxs-lookup"><span data-stu-id="35c75-232">Demo.DemoSalesOrderDetailSeed</span></span>


<span data-ttu-id="35c75-233">A memóriaoptimalizált táblák keresztül vizsgálhatja meg a **Object Explorer** szolgáltatáshoz az ssms.</span><span class="sxs-lookup"><span data-stu-id="35c75-233">You can inspect memory-optimized tables through the **Object Explorer** in SSMS.</span></span> <span data-ttu-id="35c75-234">Kattintson a jobb gombbal **táblák** > **szűrő** > **beállítások szűrése** > **Memóriaoptimalizált**.</span><span class="sxs-lookup"><span data-stu-id="35c75-234">Right-click **Tables** > **Filter** > **Filter Settings** > **Is Memory Optimized**.</span></span> <span data-ttu-id="35c75-235">Az érték 1.</span><span class="sxs-lookup"><span data-stu-id="35c75-235">The value equals 1.</span></span>


<span data-ttu-id="35c75-236">Vagy a katalógus nézetek, például:</span><span class="sxs-lookup"><span data-stu-id="35c75-236">Or you can query the catalog views, such as:</span></span>


```
SELECT is_memory_optimized, name, type_desc, durability_desc
    FROM sys.tables
    WHERE is_memory_optimized = 1;
```


<span data-ttu-id="35c75-237">**Natív módon lefordított tárolt eljárás**: SalesLT.usp_InsertSalesOrder_inmem vizsgálhatja lekérdezéssel-katalógus megtekintése:</span><span class="sxs-lookup"><span data-stu-id="35c75-237">**Natively compiled stored procedure**: You can inspect SalesLT.usp_InsertSalesOrder_inmem through a catalog view query:</span></span>


```
SELECT uses_native_compilation, OBJECT_NAME(object_id), definition
    FROM sys.sql_modules
    WHERE uses_native_compilation = 1;
```


&nbsp;

### <a name="run-the-sample-oltp-workload"></a><span data-ttu-id="35c75-238">A minta OLTP-munkaterhelés futtatása</span><span class="sxs-lookup"><span data-stu-id="35c75-238">Run the sample OLTP workload</span></span>

<span data-ttu-id="35c75-239">Az egyetlen különbség a következő két *tárolt eljárások* , hogy az első eljárás használja verziók memóriaoptimalizált táblák esetén a második eljárás a lemezen tábláit használja:</span><span class="sxs-lookup"><span data-stu-id="35c75-239">The only difference between the following two *stored procedures* is that the first procedure uses memory-optimized versions of the tables, while the second procedure uses the regular on-disk tables:</span></span>

- <span data-ttu-id="35c75-240">SalesLT**.** usp_InsertSalesOrder**_inmem**</span><span class="sxs-lookup"><span data-stu-id="35c75-240">SalesLT**.**usp_InsertSalesOrder**_inmem**</span></span>
- <span data-ttu-id="35c75-241">SalesLT**.** usp_InsertSalesOrder**_ondisk**</span><span class="sxs-lookup"><span data-stu-id="35c75-241">SalesLT**.**usp_InsertSalesOrder**_ondisk**</span></span>


<span data-ttu-id="35c75-242">Ez a szakasz használata a hasznos látható **ostress.exe** segédprogram stressful szinten két tárolt eljárások végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="35c75-242">In this section, you see how to use the handy **ostress.exe** utility to execute the two stored procedures at stressful levels.</span></span> <span data-ttu-id="35c75-243">Összehasonlíthatja mennyi ideig tart a két magas terhelés kísérletekhez befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="35c75-243">You can compare how long it takes for the two stress runs to finish.</span></span>


<span data-ttu-id="35c75-244">Ostress.exe futtatásakor azt javasoljuk, hogy sikeresen lezajlott-e az alábbi tervezett paraméterértékek:</span><span class="sxs-lookup"><span data-stu-id="35c75-244">When you run ostress.exe, we recommend that you pass parameter values designed for both of the following:</span></span>

- <span data-ttu-id="35c75-245">Futtassa a nagyszámú egyidejű kapcsolatok segítségével - n100.</span><span class="sxs-lookup"><span data-stu-id="35c75-245">Run a large number of concurrent connections, by using -n100.</span></span>
- <span data-ttu-id="35c75-246">Minden kapcsolat hurok több száz időpontokban, a rendelkezik használatával - r500.</span><span class="sxs-lookup"><span data-stu-id="35c75-246">Have each connection loop hundreds of times, by using -r500.</span></span>


<span data-ttu-id="35c75-247">Érdemes azonban, például - n10 és - r 50 sokkal kisebb értékekkel indítására győződjön meg arról, hogy minden működik.</span><span class="sxs-lookup"><span data-stu-id="35c75-247">However, you might want to start with much smaller values like -n10 and -r50 to ensure that everything is working.</span></span>


### <a name="script-for-ostressexe"></a><span data-ttu-id="35c75-248">Ostress.exe parancsfájl</span><span class="sxs-lookup"><span data-stu-id="35c75-248">Script for ostress.exe</span></span>


<span data-ttu-id="35c75-249">Ez a szakasz a T-SQL-parancsfájl a ostress.exe parancssori beágyazott jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="35c75-249">This section displays the T-SQL script that is embedded in our ostress.exe command line.</span></span> <span data-ttu-id="35c75-250">A parancsfájl a korábban telepített a T-SQL-parancsfájl által létrehozott elemek.</span><span class="sxs-lookup"><span data-stu-id="35c75-250">The script uses items that were created by the T-SQL script that you installed earlier.</span></span>


<span data-ttu-id="35c75-251">A következő parancsfájl egy minta értékesítési sorrendben öt sor elemekhez szúr be a következő memóriaoptimalizált *táblák*:</span><span class="sxs-lookup"><span data-stu-id="35c75-251">The following script inserts a sample sales order with five line items into the following memory-optimized *tables*:</span></span>

- <span data-ttu-id="35c75-252">SalesLT.SalesOrderHeader_inmem</span><span class="sxs-lookup"><span data-stu-id="35c75-252">SalesLT.SalesOrderHeader_inmem</span></span>
- <span data-ttu-id="35c75-253">SalesLT.SalesOrderDetail_inmem</span><span class="sxs-lookup"><span data-stu-id="35c75-253">SalesLT.SalesOrderDetail_inmem</span></span>


```
DECLARE
    @i int = 0,
    @od SalesLT.SalesOrderDetailType_inmem,
    @SalesOrderID int,
    @DueDate datetime2 = sysdatetime(),
    @CustomerID int = rand() * 8000,
    @BillToAddressID int = rand() * 10000,
    @ShipToAddressID int = rand() * 10000;

INSERT INTO @od
    SELECT OrderQty, ProductID
    FROM Demo.DemoSalesOrderDetailSeed
    WHERE OrderID= cast((rand()*60) as int);

WHILE (@i < 20)
begin;
    EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT,
        @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od;
    SET @i = @i + 1;
end
```


<span data-ttu-id="35c75-254">Annak a *_ondisk* az előző T-SQL-parancsfájlt ostress.exe verziója, cserélje a mindkét előfordulását a *_inmem* a karakterláncrészletre *_ondisk*.</span><span class="sxs-lookup"><span data-stu-id="35c75-254">To make the *_ondisk* version of the preceding T-SQL script for ostress.exe, you would replace both occurrences of the *_inmem* substring with *_ondisk*.</span></span> <span data-ttu-id="35c75-255">A csere hatással a táblák és tárolt eljárások neve.</span><span class="sxs-lookup"><span data-stu-id="35c75-255">These replacements affect the names of tables and stored procedures.</span></span>


### <a name="install-rml-utilities-and-ostress"></a><span data-ttu-id="35c75-256">RML segédprogramok és ostress telepítése</span><span class="sxs-lookup"><span data-stu-id="35c75-256">Install RML utilities and ostress</span></span>


<span data-ttu-id="35c75-257">Ideális esetben tenné szeretné futtatni ostress.exe egy Azure virtuális gépen (VM).</span><span class="sxs-lookup"><span data-stu-id="35c75-257">Ideally, you would plan to run ostress.exe on an Azure virtual machine (VM).</span></span> <span data-ttu-id="35c75-258">Akkor kell létrehoznia egy [Azure virtuális gép](https://azure.microsoft.com/documentation/services/virtual-machines/) a azonos Azure földrajzi régióban, ahol a AdventureWorksLT adatbázis található.</span><span class="sxs-lookup"><span data-stu-id="35c75-258">You would create an [Azure VM](https://azure.microsoft.com/documentation/services/virtual-machines/) in the same Azure geographic region where your AdventureWorksLT database resides.</span></span> <span data-ttu-id="35c75-259">De futtathatja ostress.exe inkább a laptopján.</span><span class="sxs-lookup"><span data-stu-id="35c75-259">But you can run ostress.exe on your laptop instead.</span></span>


<span data-ttu-id="35c75-260">A virtuális Gépet, vagy bármilyen üzemeltetéséhez, akkor válassza, a visszajátszás Markup Language (RML) segédprogramokat telepíthet.</span><span class="sxs-lookup"><span data-stu-id="35c75-260">On the VM, or on whatever host you choose, install the Replay Markup Language (RML) utilities.</span></span> <span data-ttu-id="35c75-261">A segédprogramok ostress.exe tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="35c75-261">The utilities include ostress.exe.</span></span>

<span data-ttu-id="35c75-262">További információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="35c75-262">For more information, see:</span></span>
- <span data-ttu-id="35c75-263">A ostress.exe vitafórum [memórián belüli online Tranzakciófeldolgozási adatbázist](http://msdn.microsoft.com/library/mt465764.aspx).</span><span class="sxs-lookup"><span data-stu-id="35c75-263">The ostress.exe discussion in [Sample Database for In-Memory OLTP](http://msdn.microsoft.com/library/mt465764.aspx).</span></span>
- <span data-ttu-id="35c75-264">[A memórián belüli online Tranzakciófeldolgozási adatbázis minta](http://msdn.microsoft.com/library/mt465764.aspx).</span><span class="sxs-lookup"><span data-stu-id="35c75-264">[Sample Database for In-Memory OLTP](http://msdn.microsoft.com/library/mt465764.aspx).</span></span>
- <span data-ttu-id="35c75-265">A [ostress.exe telepítésének blog](http://blogs.msdn.com/b/psssql/archive/2013/10/29/cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx).</span><span class="sxs-lookup"><span data-stu-id="35c75-265">The [blog for installing ostress.exe](http://blogs.msdn.com/b/psssql/archive/2013/10/29/cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx).</span></span>



<!--
dn511655.aspx is for SQL 2014,
[Extensions to AdventureWorks to Demonstrate In-Memory OLTP]
(http://msdn.microsoft.com/library/dn511655&#x28;v=sql.120&#x29;.aspx)

whereas for SQL 2016+
[Sample Database for In-Memory OLTP]
(http://msdn.microsoft.com/library/mt465764.aspx)
-->



### <a name="run-the-inmem-stress-workload-first"></a><span data-ttu-id="35c75-266">Futtassa a *_inmem* emelje ki először munkaterhelés</span><span class="sxs-lookup"><span data-stu-id="35c75-266">Run the *_inmem* stress workload first</span></span>


<span data-ttu-id="35c75-267">Használhat egy *RML Cmd Rákérdezés* ablakban a ostress.exe parancssort futtathat.</span><span class="sxs-lookup"><span data-stu-id="35c75-267">You can use an *RML Cmd Prompt* window to run our ostress.exe command line.</span></span> <span data-ttu-id="35c75-268">A parancssori paraméterek ostress való közvetlen:</span><span class="sxs-lookup"><span data-stu-id="35c75-268">The command-line parameters direct ostress to:</span></span>

- <span data-ttu-id="35c75-269">100 kapcsolatok egyidejű futtatását (-n100).</span><span class="sxs-lookup"><span data-stu-id="35c75-269">Run 100 connections concurrently (-n100).</span></span>
- <span data-ttu-id="35c75-270">Minden kapcsolat 50 alkalommal a T-SQL-parancsfájl futtatása (-r 50).</span><span class="sxs-lookup"><span data-stu-id="35c75-270">Have each connection run the T-SQL script 50 times (-r50).</span></span>


```
ostress.exe -n100 -r50 -S<servername>.database.windows.net -U<login> -P<password> -d<database> -q -Q"DECLARE @i int = 0, @od SalesLT.SalesOrderDetailType_inmem, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand()* 10000; INSERT INTO @od SELECT OrderQty, ProductID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*60) as int); WHILE (@i < 20) begin; EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od; set @i += 1; end"
```


<span data-ttu-id="35c75-271">A fenti ostress.exe parancssor futtatása:</span><span class="sxs-lookup"><span data-stu-id="35c75-271">To run the preceding ostress.exe command line:</span></span>


1. <span data-ttu-id="35c75-272">Alaphelyzetbe állítja az adatbázis az adatok tartalmának szolgáltatáshoz az ssms, az minden korábbi futtatása által beszúrt adatok törléséhez a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="35c75-272">Reset the database data content by running the following command in SSMS, to delete all the data that was inserted by any previous runs:</span></span>

    ``` tsql
    EXECUTE Demo.usp_DemoReset;
    ```

2. <span data-ttu-id="35c75-273">A fenti ostress.exe parancssor szövegét másolja a vágólapra.</span><span class="sxs-lookup"><span data-stu-id="35c75-273">Copy the text of the preceding ostress.exe command line to your clipboard.</span></span>

3. <span data-ttu-id="35c75-274">Cserélje le a `<placeholders>` a paraméterek -S - U -P - d a megfelelő valós értékekkel.</span><span class="sxs-lookup"><span data-stu-id="35c75-274">Replace the `<placeholders>` for the parameters -S -U -P -d with the correct real values.</span></span>

4. <span data-ttu-id="35c75-275">A szerkesztett parancssor futtatása a egy RML Cmd ablakot.</span><span class="sxs-lookup"><span data-stu-id="35c75-275">Run your edited command line in an RML Cmd window.</span></span>


#### <a name="result-is-a-duration"></a><span data-ttu-id="35c75-276">Eredménye egy időtartam</span><span class="sxs-lookup"><span data-stu-id="35c75-276">Result is a duration</span></span>


<span data-ttu-id="35c75-277">Ostress.exe befejezése után, a kimenet a végső sorként futtatási időtartama ír a RML Cmd ablakot.</span><span class="sxs-lookup"><span data-stu-id="35c75-277">When ostress.exe finishes, it writes the run duration as its final line of output in the RML Cmd window.</span></span> <span data-ttu-id="35c75-278">Például egy rövidebb vizsgálat tartott körülbelül 1,5 percet:</span><span class="sxs-lookup"><span data-stu-id="35c75-278">For example, a shorter test run lasted about 1.5 minutes:</span></span>

`11/12/15 00:35:00.873 [0x000030A8] OSTRESS exiting normally, elapsed time: 00:01:31.867`


#### <a name="reset-edit-for-ondisk-then-rerun"></a><span data-ttu-id="35c75-279">Alaphelyzetbe állítja, a Szerkesztés *_ondisk*, majd futtassa újra a</span><span class="sxs-lookup"><span data-stu-id="35c75-279">Reset, edit for *_ondisk*, then rerun</span></span>


<span data-ttu-id="35c75-280">Miután kapott eredmény az *_inmem* futnak, hajtsa végre az alábbi lépéseket a *_ondisk* futtatása:</span><span class="sxs-lookup"><span data-stu-id="35c75-280">After you have the result from the *_inmem* run, perform the following steps for the *_ondisk* run:</span></span>


1. <span data-ttu-id="35c75-281">Az adatbázis visszaállítása a szolgáltatáshoz az ssms az adatok törléséhez a korábbi futtatás által beszúrt a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="35c75-281">Reset the database by running the following command in SSMS to delete all the data that was inserted by the previous run:</span></span>
```
EXECUTE Demo.usp_DemoReset;
```

2. <span data-ttu-id="35c75-282">Szerkessze a ostress.exe parancs cseréjét *_inmem* rendelkező *_ondisk*.</span><span class="sxs-lookup"><span data-stu-id="35c75-282">Edit the ostress.exe command line to replace all *_inmem* with *_ondisk*.</span></span>

3. <span data-ttu-id="35c75-283">Futtassa újra a második ostress.exe, és rögzítse a duration eredménye.</span><span class="sxs-lookup"><span data-stu-id="35c75-283">Rerun ostress.exe for the second time, and capture the duration result.</span></span>

4. <span data-ttu-id="35c75-284">Ebben az esetben alaphelyzetbe állítása az adatbázis (az osztott törlése, mi is vizsgálati adatok nagy mennyiségű lehet).</span><span class="sxs-lookup"><span data-stu-id="35c75-284">Again, reset the database (for responsibly deleting what can be a large amount of test data).</span></span>


#### <a name="expected-comparison-results"></a><span data-ttu-id="35c75-285">Várt összehasonlítás eredménye</span><span class="sxs-lookup"><span data-stu-id="35c75-285">Expected comparison results</span></span>

<span data-ttu-id="35c75-286">A memóriában tesztek kimutatták, hogy javítja a teljesítményt **kilenc alkalommal** a simplistic munkaterhelés, a ostress ugyanabban a régióban Azure-adatbázisként egy Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="35c75-286">Our In-Memory tests have shown that performance improved by **nine times** for this simplistic workload, with ostress running on an Azure VM in the same Azure region as the database.</span></span>

<a id="install_analytics_manuallink" name="install_analytics_manuallink"></a>

&nbsp;

## <a name="2-install-the-in-memory-analytics-sample"></a><span data-ttu-id="35c75-287">2. A memórián belüli Analytics minta telepítése</span><span class="sxs-lookup"><span data-stu-id="35c75-287">2. Install the In-Memory Analytics sample</span></span>


<span data-ttu-id="35c75-288">Ebben a szakaszban összehasonlítja az IO és a statisztika eredményeket oszlopcentrikus index és egy hagyományos b-fa index használatakor.</span><span class="sxs-lookup"><span data-stu-id="35c75-288">In this section, you compare the IO and statistics results when you're using a columnstore index versus a traditional b-tree index.</span></span>


<span data-ttu-id="35c75-289">Az OLTP-munkaterhelés valós idejű elemzés célszerű gyakran egy nem fürtözött oszloptárindex használatára.</span><span class="sxs-lookup"><span data-stu-id="35c75-289">For real-time analytics on an OLTP workload, it's often best to use a nonclustered columnstore index.</span></span> <span data-ttu-id="35c75-290">További információkért lásd: [Oszlopcentrikus indexek leírt](http://msdn.microsoft.com/library/gg492088.aspx).</span><span class="sxs-lookup"><span data-stu-id="35c75-290">For details, see [Columnstore Indexes Described](http://msdn.microsoft.com/library/gg492088.aspx).</span></span>



### <a name="prepare-the-columnstore-analytics-test"></a><span data-ttu-id="35c75-291">Készítse elő a oszlopcentrikus analytics teszt</span><span class="sxs-lookup"><span data-stu-id="35c75-291">Prepare the columnstore analytics test</span></span>


1. <span data-ttu-id="35c75-292">Az Azure-portál használatával hozható létre a minta egy friss AdventureWorksLT adatbázis.</span><span class="sxs-lookup"><span data-stu-id="35c75-292">Use the Azure portal to create a fresh AdventureWorksLT database from the sample.</span></span>
 - <span data-ttu-id="35c75-293">Használja ezt a teljes nevet.</span><span class="sxs-lookup"><span data-stu-id="35c75-293">Use that exact name.</span></span>
 - <span data-ttu-id="35c75-294">Válassza ki a prémium szolgáltatásszintet.</span><span class="sxs-lookup"><span data-stu-id="35c75-294">Choose any Premium service tier.</span></span>

2. <span data-ttu-id="35c75-295">Másolás a [sql_in-memory_analytics_sample](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_analytics_sample.sql) a vágólapra.</span><span class="sxs-lookup"><span data-stu-id="35c75-295">Copy the [sql_in-memory_analytics_sample](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_analytics_sample.sql) to your clipboard.</span></span>
 - <span data-ttu-id="35c75-296">A T-SQL parancsfájlt a szükséges memórián belüli objektumok a AdventureWorksLT mintaadatbázis az 1. lépésben létrehozott hoz létre.</span><span class="sxs-lookup"><span data-stu-id="35c75-296">The T-SQL script creates the necessary In-Memory objects in the AdventureWorksLT sample database that you created in step 1.</span></span>
 - <span data-ttu-id="35c75-297">A parancsfájl a dimenziótáblában és két ténytáblák hoz létre.</span><span class="sxs-lookup"><span data-stu-id="35c75-297">The script creates the Dimension table and two fact tables.</span></span> <span data-ttu-id="35c75-298">A ténytáblák feltöltött 3.5 millió sort.</span><span class="sxs-lookup"><span data-stu-id="35c75-298">The fact tables are populated with 3.5 million rows each.</span></span>
 - <span data-ttu-id="35c75-299">A parancsfájl elvégzéséhez 15 percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="35c75-299">The script might take 15 minutes to complete.</span></span>

3. <span data-ttu-id="35c75-300">Illessze be a T-SQL parancsfájl SSMS, és majd végrehajtani a parancsprogramot.</span><span class="sxs-lookup"><span data-stu-id="35c75-300">Paste the T-SQL script into SSMS, and then execute the script.</span></span> <span data-ttu-id="35c75-301">A **OSZLOPCENTRIKUS** kulcsszót a **a CREATE INDEX** utasítás elengedhetetlen, mint:</span><span class="sxs-lookup"><span data-stu-id="35c75-301">The **COLUMNSTORE** keyword in the **CREATE INDEX** statement is crucial, as in:</span></span><br/>`CREATE NONCLUSTERED COLUMNSTORE INDEX ...;`

4. <span data-ttu-id="35c75-302">Állítsa be AdventureWorksLT 130 kompatibilitási szintje:</span><span class="sxs-lookup"><span data-stu-id="35c75-302">Set AdventureWorksLT to compatibility level 130:</span></span><br/>`ALTER DATABASE AdventureworksLT SET compatibility_level = 130;`

    <span data-ttu-id="35c75-303">Szint 130 nem közvetlenül kapcsolódik a memóriával kapcsolatos szolgáltatásainak.</span><span class="sxs-lookup"><span data-stu-id="35c75-303">Level 130 is not directly related to In-Memory features.</span></span> <span data-ttu-id="35c75-304">De szint 130 általában gyorsabb lekérdezési teljesítményt, mint 120.</span><span class="sxs-lookup"><span data-stu-id="35c75-304">But level 130 generally provides faster query performance than 120.</span></span>


#### <a name="key-tables-and-columnstore-indexes"></a><span data-ttu-id="35c75-305">Kulcs táblák és az oszlopcentrikus indexek</span><span class="sxs-lookup"><span data-stu-id="35c75-305">Key tables and columnstore indexes</span></span>


- <span data-ttu-id="35c75-306">dbo. FactResellerSalesXL_CCI fürtözött oszlopcentrikus index, amely továbbfejlesztett tömörítés a táblának a *adatok* szintjét.</span><span class="sxs-lookup"><span data-stu-id="35c75-306">dbo.FactResellerSalesXL_CCI is a table that has a clustered columnstore index, which has advanced compression at the *data* level.</span></span>

- <span data-ttu-id="35c75-307">dbo. FactResellerSalesXL_PageCompressed táblának egyenértékű rendszeres fürtözött indexet, amely csak a rendszer tömöríti a *lap* szintjét.</span><span class="sxs-lookup"><span data-stu-id="35c75-307">dbo.FactResellerSalesXL_PageCompressed is a table that has an equivalent regular clustered index, which is compressed only at the *page* level.</span></span>


#### <a name="key-queries-to-compare-the-columnstore-index"></a><span data-ttu-id="35c75-308">Az oszloptárindex összehasonlítandó kulcs lekérdezések</span><span class="sxs-lookup"><span data-stu-id="35c75-308">Key queries to compare the columnstore index</span></span>


<span data-ttu-id="35c75-309">Nincsenek [T-SQL lekérdezés számos különböző futtatható](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/clustered_columnstore_sample_queries.sql) teljesítménnyel kapcsolatos fejlesztések megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="35c75-309">There are [several T-SQL query types that you can run](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/clustered_columnstore_sample_queries.sql) to see performance improvements.</span></span> <span data-ttu-id="35c75-310">A 2 a T-SQL parancsfájl a lekérdezések pár figyelmet fordítania.</span><span class="sxs-lookup"><span data-stu-id="35c75-310">In step 2 in the T-SQL script, pay attention to this pair of queries.</span></span> <span data-ttu-id="35c75-311">Csak egy sorban különböznek:</span><span class="sxs-lookup"><span data-stu-id="35c75-311">They differ only on one line:</span></span>


- `FROM FactResellerSalesXL_PageCompressed a`
- `FROM FactResellerSalesXL_CCI a`


<span data-ttu-id="35c75-312">Fürtözött oszlopcentrikus index szerepel a FactResellerSalesXL\_közösségi koordináló intézet tábla.</span><span class="sxs-lookup"><span data-stu-id="35c75-312">A clustered columnstore index is in the FactResellerSalesXL\_CCI table.</span></span>

<span data-ttu-id="35c75-313">A következő T-SQL parancsfájl cikkből statisztika IO és a lekérdezés minden tábla idő nyomtatása.</span><span class="sxs-lookup"><span data-stu-id="35c75-313">The following T-SQL script excerpt prints statistics for IO and TIME for the query of each table.</span></span>


```
/*********************************************************************
Step 2 -- Overview
-- Page Compressed BTree table v/s Columnstore table performance differences
-- Enable actual Query Plan in order to see Plan differences when Executing
*/
-- Ensure Database is in 130 compatibility mode
ALTER DATABASE AdventureworksLT SET compatibility_level = 130
GO

-- Execute a typical query that joins the Fact Table with dimension tables
-- Note this query will run on the Page Compressed table, Note down the time
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO

SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_PageCompressed a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO
SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO


-- This is the same Prior query on a table with a clustered columnstore index CCI
-- The comparison numbers are even more dramatic the larger the table is (this is an 11 million row table only)
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO
SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_CCI a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO

SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO
```

<span data-ttu-id="35c75-314">A P2 tarifacsomag adatbázisban körülbelül kilenc alkalommal a jobb teljesítménye ehhez a lekérdezéshez számíthat a fürtözött oszlopcentrikus index összehasonlítja a hagyományos index használatával.</span><span class="sxs-lookup"><span data-stu-id="35c75-314">In a database with the P2 pricing tier, you can expect about nine times the performance gain for this query by using the clustered columnstore index compared with the traditional index.</span></span> <span data-ttu-id="35c75-315">P15, a várhatóan hamarosan 57 alkalommal a jobb teljesítménye az oszlopcentrikus index használatával.</span><span class="sxs-lookup"><span data-stu-id="35c75-315">With P15, you can expect about 57 times the performance gain by using the columnstore index.</span></span>



## <a name="next-steps"></a><span data-ttu-id="35c75-316">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="35c75-316">Next steps</span></span>

- [<span data-ttu-id="35c75-317">Gyors üzembe helyezési 1: A memórián belüli online Tranzakciófeldolgozási technológiák a T-SQL jobb teljesítmény</span><span class="sxs-lookup"><span data-stu-id="35c75-317">Quick Start 1: In-Memory OLTP Technologies for Faster T-SQL Performance</span></span>](http://msdn.microsoft.com/library/mt694156.aspx)

- [<span data-ttu-id="35c75-318">Használja a memórián belüli online Tranzakciófeldolgozási egy meglévő Azure SQL-alkalmazásban</span><span class="sxs-lookup"><span data-stu-id="35c75-318">Use In-Memory OLTP in an existing Azure SQL application</span></span>](sql-database-in-memory-oltp-migration.md)

- <span data-ttu-id="35c75-319">[A figyelő a memórián belüli online Tranzakciófeldolgozási tárolási](sql-database-in-memory-oltp-monitoring.md) a memórián belüli online Tranzakciófeldolgozási</span><span class="sxs-lookup"><span data-stu-id="35c75-319">[Monitor In-Memory OLTP storage](sql-database-in-memory-oltp-monitoring.md) for In-Memory OLTP</span></span>


## <a name="additional-resources"></a><span data-ttu-id="35c75-320">További források</span><span class="sxs-lookup"><span data-stu-id="35c75-320">Additional resources</span></span>

#### <a name="deeper-information"></a><span data-ttu-id="35c75-321">Mélyebb információk</span><span class="sxs-lookup"><span data-stu-id="35c75-321">Deeper information</span></span>

- [<span data-ttu-id="35c75-322">Ismerje meg, hogyan kvórum megduplázódik miközben DTU csökkenti a memórián belüli online Tranzakciófeldolgozási az SQL-adatbázis 70 %-kal kulcsának adatbázis munkaterhelés</span><span class="sxs-lookup"><span data-stu-id="35c75-322">Learn how Quorum doubles key database’s workload while lowering DTU by 70% with In-Memory OLTP in SQL Database</span></span>](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)

- [<span data-ttu-id="35c75-323">Memórián belüli online Tranzakciófeldolgozási Azure SQL adatbázis blogbejegyzésben</span><span class="sxs-lookup"><span data-stu-id="35c75-323">In-Memory OLTP in Azure SQL Database Blog Post</span></span>](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)

- [<span data-ttu-id="35c75-324">További tudnivalók a memórián belüli online Tranzakciófeldolgozási</span><span class="sxs-lookup"><span data-stu-id="35c75-324">Learn about In-Memory OLTP</span></span>](http://msdn.microsoft.com/library/dn133186.aspx)

- [<span data-ttu-id="35c75-325">További tudnivalók az oszlopcentrikus indexek</span><span class="sxs-lookup"><span data-stu-id="35c75-325">Learn about columnstore indexes</span></span>](https://msdn.microsoft.com/library/gg492088.aspx)

- [<span data-ttu-id="35c75-326">További információk a valós idejű működési elemzés</span><span class="sxs-lookup"><span data-stu-id="35c75-326">Learn about real-time operational analytics</span></span>](http://msdn.microsoft.com/library/dn817827.aspx)

- <span data-ttu-id="35c75-327">Lásd: [közös terhelési mintázatok és az áttelepítés szempontjai](http://msdn.microsoft.com/library/dn673538.aspx) (amely ismerteti, ahol a memórián belüli online Tranzakciófeldolgozási gyakran biztosít teljesítménynövekedéshez munkaterhelés minták)</span><span class="sxs-lookup"><span data-stu-id="35c75-327">See [Common Workload Patterns and Migration Considerations](http://msdn.microsoft.com/library/dn673538.aspx) (which describes workload patterns where In-Memory OLTP commonly provides significant performance gains)</span></span>

#### <a name="application-design"></a><span data-ttu-id="35c75-328">Alkalmazás tervezése</span><span class="sxs-lookup"><span data-stu-id="35c75-328">Application design</span></span>

- [<span data-ttu-id="35c75-329">Memórián belüli online Tranzakciófeldolgozási (a memóriában optimalizálása)</span><span class="sxs-lookup"><span data-stu-id="35c75-329">In-Memory OLTP (In-Memory Optimization)</span></span>](http://msdn.microsoft.com/library/dn133186.aspx)

- [<span data-ttu-id="35c75-330">Használja a memórián belüli online Tranzakciófeldolgozási egy meglévő Azure SQL-alkalmazásban</span><span class="sxs-lookup"><span data-stu-id="35c75-330">Use In-Memory OLTP in an existing Azure SQL application</span></span>](sql-database-in-memory-oltp-migration.md)

#### <a name="tools"></a><span data-ttu-id="35c75-331">Eszközök</span><span class="sxs-lookup"><span data-stu-id="35c75-331">Tools</span></span>

- [<span data-ttu-id="35c75-332">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="35c75-332">Azure portal</span></span>](https://portal.azure.com/)

- [<span data-ttu-id="35c75-333">SQL Server Management Studio (SSMS)</span><span class="sxs-lookup"><span data-stu-id="35c75-333">SQL Server Management Studio (SSMS)</span></span>](https://msdn.microsoft.com/library/mt238290.aspx)

- [<span data-ttu-id="35c75-334">SQL Server Data Tools (SSDT)</span><span class="sxs-lookup"><span data-stu-id="35c75-334">SQL Server Data Tools (SSDT)</span></span>](http://msdn.microsoft.com/library/mt204009.aspx)
