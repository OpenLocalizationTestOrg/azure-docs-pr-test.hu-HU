---
title: "aaaAzure SQL adatbázis memórián belüli technológiák |} Microsoft Docs"
description: "Az Azure SQL adatbázis memórián belüli technológiák jelentősen javítja a tranzakciós hello teljesítményét és elemzés munkaterhelések. Ismerje meg, hogy ezek a technológiák tootake előnyeit."
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
ms.openlocfilehash: 1bacd7297b2f9b018853088eabf2a2ee66a9cb43
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-performance-by-using-in-memory-technologies-in-sql-database"></a><span data-ttu-id="8d50f-104">A memórián belüli technológiái az SQL-adatbázis teljesítményének optimalizálása</span><span class="sxs-lookup"><span data-stu-id="8d50f-104">Optimize performance by using In-Memory technologies in SQL Database</span></span>

<span data-ttu-id="8d50f-105">A memórián belüli technológiái az Azure SQL Database, a különböző munkaterhelések teljesítménynövekedést érhet el: tranzakciós (online tranzakciós feldolgozási (OLTP)), elemzés (online analitikus feldolgozási (OLAP)), és a vegyes (hibrid tranzakció / analitikus feldolgozási (HTAP)).</span><span class="sxs-lookup"><span data-stu-id="8d50f-105">By using In-Memory technologies in Azure SQL Database, you can achieve performance improvements with various workloads: transactional (online transactional processing (OLTP)), analytics (online analytical processing (OLAP)), and mixed (hybrid transaction/analytical processing (HTAP)).</span></span> <span data-ttu-id="8d50f-106">Mert hello hatékonyabb lekérdezés és a tranzakció-feldolgozást, a memórián belüli technológiák is segítséget tooreduce költség.</span><span class="sxs-lookup"><span data-stu-id="8d50f-106">Because of hello more efficient query and transaction processing, In-Memory technologies also help you tooreduce cost.</span></span> <span data-ttu-id="8d50f-107">Általában nem szükséges tooupgrade hello tarifacsomag hello adatbázis tooachieve teljesítménynövekedést érhet el.</span><span class="sxs-lookup"><span data-stu-id="8d50f-107">You typically don't need tooupgrade hello pricing tier of hello database tooachieve performance gains.</span></span> <span data-ttu-id="8d50f-108">Bizonyos esetekben is lehet, csökkentse az IP-címek, miközben továbbra sem szűnik meg a memórián belüli szolgáltatáshoz. a teljesítménnyel kapcsolatos fejlesztések hello.</span><span class="sxs-lookup"><span data-stu-id="8d50f-108">In some cases, you might even be able reduce hello pricing tier, while still seeing performance improvements with In-Memory technologies.</span></span>

<span data-ttu-id="8d50f-109">Az alábbiakban a két példa hogyan memórián belüli online Tranzakciófeldolgozási segített a teljesítmény javítása toosignificantly:</span><span class="sxs-lookup"><span data-stu-id="8d50f-109">Here are two examples of how In-Memory OLTP helped toosignificantly improve performance:</span></span>

- <span data-ttu-id="8d50f-110">A memórián belüli online Tranzakciófeldolgozási, [kvórum üzleti megoldások történt képes toodouble a munkaterhelés dtu-k javítása 70 %-kal](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database).</span><span class="sxs-lookup"><span data-stu-id="8d50f-110">By using In-Memory OLTP, [Quorum Business Solutions was able toodouble their workload while improving DTUs by 70%](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database).</span></span>
    - <span data-ttu-id="8d50f-111">DTU azt jelenti, hogy *adatbázis-átviteli egység*, és egy az erőforrás-felhasználás mesurement tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="8d50f-111">DTU means *database throughput unit*, and it includes a mesurement of resource consumption.</span></span>
- <span data-ttu-id="8d50f-112">hello következő videó bemutatja az erőforrás-felhasználást egy példa munkaterhelés jelentős fejlesztéseket: [memórián belüli online Tranzakciófeldolgozási az Azure SQL adatbázis Video](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB).</span><span class="sxs-lookup"><span data-stu-id="8d50f-112">hello following video demonstrates significant improvement in resource consumption with a sample workload: [In-Memory OLTP in Azure SQL Database Video](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB).</span></span>
    - <span data-ttu-id="8d50f-113">További részletekért lásd: hello blogbejegyzést: [memórián belüli online Tranzakciófeldolgozási az Azure SQL adatbázis Blog utáni](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)</span><span class="sxs-lookup"><span data-stu-id="8d50f-113">For more details, see hello blog post: [In-Memory OLTP in Azure SQL Database Blog Post](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)</span></span>

<span data-ttu-id="8d50f-114">A memóriában technológiák minden adatbázisa hello prémium csomagban, beleértve az adatbázisok prémium rugalmas készletek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="8d50f-114">In-Memory technologies are available in all databases in hello Premium tier, including databases in Premium elastic pools.</span></span>

<span data-ttu-id="8d50f-115">hello következő videó ismerteti lehetséges teljesítménynövekedést érhet el, a memórián belüli szolgáltatáshoz. az Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="8d50f-115">hello following video explains potential performance gains with In-Memory technologies in Azure SQL Database.</span></span> <span data-ttu-id="8d50f-116">Ne feledje, hogy hello jobb teljesítménye, hogy mindig számos tényezőtől függ, például hello jellege hello munkaterhelési adatok hozzáférési mintázatát hello adatbázis, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="8d50f-116">Remember that hello performance gain that you see always depends on many factors, including hello nature of hello workload and data, access pattern of hello database, and so on.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-In-Memory-Technologies/player]
>
>

<span data-ttu-id="8d50f-117">Az Azure SQL Database a következő memórián belüli technológiák hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="8d50f-117">Azure SQL Database has hello following In-Memory technologies:</span></span>

- <span data-ttu-id="8d50f-118">*A memórián belüli online Tranzakciófeldolgozási* növeli az adatátviteli sebességet, és csökkenti a késést tranzakció feldolgozásra.</span><span class="sxs-lookup"><span data-stu-id="8d50f-118">*In-Memory OLTP* increases throughput and reduces latency for transaction processing.</span></span> <span data-ttu-id="8d50f-119">A memórián belüli online Tranzakciófeldolgozási előnyeit kihasználó forgatókönyvek a következők: nagy átviteli tranzakció, például kereskedelmi és játékokban, adatfeldolgozást események vagy az IoT-eszközök, gyorsítótárazás, az adatok betöltését, és az ideiglenes tábla és a tábla változó forgatókönyvek feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="8d50f-119">Scenarios that benefit from In-Memory OLTP are: high-throughput transaction processing such as trading and gaming, data ingestion from events or IoT devices, caching, data load, and temporary table and table variable scenarios.</span></span>
- <span data-ttu-id="8d50f-120">*Fürtözött oszlopcentrikus indexek* csökkenti a tárolási erőforrásigényét (felfelé too10 alkalommal) és a jelentéskészítési és elemzési lekérdezések teljesítményének javítása.</span><span class="sxs-lookup"><span data-stu-id="8d50f-120">*Clustered columnstore indexes* reduce your storage footprint (up too10 times) and improve performance for reporting and analytics queries.</span></span> <span data-ttu-id="8d50f-121">Használhatja a ténytáblák az adatok jelenti toofit további adatokat az adatbázisban, és a teljesítmény javításához.</span><span class="sxs-lookup"><span data-stu-id="8d50f-121">You can use it with fact tables in your data marts toofit more data in your database and improve performance.</span></span> <span data-ttu-id="8d50f-122">Is használatát és az előzmények az operatív adatbázis tooarchive, és képes tooquery too10 alkalommal be kell több adatot.</span><span class="sxs-lookup"><span data-stu-id="8d50f-122">Also, you can use it with historical data in your operational database tooarchive and be able tooquery up too10 times more data.</span></span>
- <span data-ttu-id="8d50f-123">*Nem fürtözött oszloptárindex* HTAP segítségét, toogain valós idejű működési hello lekérdezése a vállalatra betekintést hello kell toorun nélkül egy drága kinyerési, átalakítási és betöltési (ETL) folyamat közvetlenül, adatbázis, és várjon, amíg a hello data warehouse toobe feltöltve.</span><span class="sxs-lookup"><span data-stu-id="8d50f-123">*Nonclustered columnstore indexes* for HTAP help you toogain real-time insights into your business through querying hello operational database directly, without hello need toorun an expensive extract, transform, and load (ETL) process and wait for hello data warehouse toobe populated.</span></span> <span data-ttu-id="8d50f-124">Nem fürtözött oszloptárindex hello OLTP adatbázis, nagyon gyorsan elemzési lekérdezések végrehajtása közben csökkenhet az hello működési munkaterhelés hello gyakorolt engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="8d50f-124">Nonclustered columnstore indexes allow very fast execution of analytics queries on hello OLTP database, while reducing hello impact on hello operational workload.</span></span>
- <span data-ttu-id="8d50f-125">A oszlopcentrikus indexszel rendelkező memóriaoptimalizált táblák hello kombinációja is lehet.</span><span class="sxs-lookup"><span data-stu-id="8d50f-125">You can also have hello combination of a memory-optimized table with a columnstore index.</span></span> <span data-ttu-id="8d50f-126">Ez a kombináció lehetővé teszi a tooperform nagyon gyorsan tranzakció-feldolgozásra, és túl*egyidejűleg* futtatási analytics lévő kérdezi le, nagyon gyorsan hello azonos adatokat.</span><span class="sxs-lookup"><span data-stu-id="8d50f-126">This combination enables you tooperform very fast transaction processing, and too*concurrently* run analytics queries very quickly on hello same data.</span></span>

<span data-ttu-id="8d50f-127">Oszlopcentrikus indexek és a memórián belüli online Tranzakciófeldolgozási óta része hello SQL Server 2012 és a 2014-re kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="8d50f-127">Both columnstore indexes and In-Memory OLTP have been part of hello SQL Server product since 2012 and 2014, respectively.</span></span> <span data-ttu-id="8d50f-128">Az Azure SQL Database és SQL Server megosztása hello memórián belüli technológiák azonos végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="8d50f-128">Azure SQL Database and SQL Server share hello same implementation of In-Memory technologies.</span></span> <span data-ttu-id="8d50f-129">Következő lépésként ezek a technológiák új lehetőségek kiadott az Azure SQL Database először előtt a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="8d50f-129">Going forward, new capabilities for these technologies are released in Azure SQL Database first, before they are released in SQL Server.</span></span>

<span data-ttu-id="8d50f-130">Ez a témakör ismerteti, amelyek adott tooAzure SQL-adatbázis a memórián belüli online Tranzakciófeldolgozási és oszlopcentrikus indexek aspektusait, és minták is tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="8d50f-130">This topic describes aspects of In-Memory OLTP and columnstore indexes that are specific tooAzure SQL Database and also includes samples:</span></span>
- <span data-ttu-id="8d50f-131">A tárolási és adatok méretkorlátait látni fogja, ezek a technológiák hello hatását.</span><span class="sxs-lookup"><span data-stu-id="8d50f-131">You'll see hello impact of these technologies on storage and data size limits.</span></span>
- <span data-ttu-id="8d50f-132">Láthatja, hogyan toomanage hello ezek a technológiák között különböző tarifacsomagjainak hello használó adatbázisok mozgása.</span><span class="sxs-lookup"><span data-stu-id="8d50f-132">You'll see how toomanage hello movement of databases that use these technologies between hello different pricing tiers.</span></span>
- <span data-ttu-id="8d50f-133">Látni fogja, amelyek a memórián belüli online Tranzakciófeldolgozási, valamint az Azure SQL Database oszlopcentrikus indexek használata hello szemléltetik két minta.</span><span class="sxs-lookup"><span data-stu-id="8d50f-133">You'll see two samples that illustrate hello use of In-Memory OLTP, as well as columnstore indexes in Azure SQL Database.</span></span>

<span data-ttu-id="8d50f-134">Hello erőforrások további információt a következő témakörben talál.</span><span class="sxs-lookup"><span data-stu-id="8d50f-134">See hello following resources for more information.</span></span>

<span data-ttu-id="8d50f-135">Hello technológiákkal kapcsolatos részletesebb információk:</span><span class="sxs-lookup"><span data-stu-id="8d50f-135">In-depth information about hello technologies:</span></span>

- <span data-ttu-id="8d50f-136">[Memórián belüli online Tranzakciófeldolgozási áttekintése és a használati forgatókönyvek](https://msdn.microsoft.com/library/mt774593.aspx) (tartalmazza a toocustomer esettanulmányok hivatkozások és információk tooget lépések)</span><span class="sxs-lookup"><span data-stu-id="8d50f-136">[In-Memory OLTP Overview and Usage Scenarios](https://msdn.microsoft.com/library/mt774593.aspx) (includes references toocustomer case studies and information tooget started)</span></span>
- [<span data-ttu-id="8d50f-137">A memórián belüli online Tranzakciófeldolgozási dokumentációja</span><span class="sxs-lookup"><span data-stu-id="8d50f-137">Documentation for In-Memory OLTP</span></span>](http://msdn.microsoft.com/library/dn133186.aspx)
- [<span data-ttu-id="8d50f-138">Oszlopcentrikus indexek útmutató</span><span class="sxs-lookup"><span data-stu-id="8d50f-138">Columnstore Indexes Guide</span></span>](https://msdn.microsoft.com/library/gg492088.aspx)
- <span data-ttu-id="8d50f-139">Hibrid tranzakciós/analitikus feldolgozási (HTAP), más néven [valós idejű működési elemzés](https://msdn.microsoft.com/library/dn817827.aspx)</span><span class="sxs-lookup"><span data-stu-id="8d50f-139">Hybrid transactional/analytical processing (HTAP), also known as [real-time operational analytics](https://msdn.microsoft.com/library/dn817827.aspx)</span></span>

<span data-ttu-id="8d50f-140">Gyors segítséget nyújthat a memórián belüli online Tranzakciófeldolgozási: [Quick Start 1: memórián belüli online Tranzakciófeldolgozási technológiák gyorsabb a T-SQL teljesítmény](http://msdn.microsoft.com/library/mt694156.aspx) (egy másik cikkben toohelp első lépések)</span><span class="sxs-lookup"><span data-stu-id="8d50f-140">A quick primer on In-Memory OLTP: [Quick Start 1: In-Memory OLTP Technologies for Faster T-SQL Performance](http://msdn.microsoft.com/library/mt694156.aspx) (another article toohelp you get started)</span></span>

<span data-ttu-id="8d50f-141">Részletes videók hello technológiákkal kapcsolatos:</span><span class="sxs-lookup"><span data-stu-id="8d50f-141">In-depth videos about hello technologies:</span></span>

- <span data-ttu-id="8d50f-142">[Memórián belüli online Tranzakciófeldolgozási az Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB) (amely tartalmazza a teljesítmény bemutató előnyökkel jár a és lépéseket tooreproduce ezekkel az eredményekkel saját kezűleg)</span><span class="sxs-lookup"><span data-stu-id="8d50f-142">[In-Memory OLTP in Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB) (which contains a demo of performance benefits and steps tooreproduce these results yourself)</span></span>
- [<span data-ttu-id="8d50f-143">Memórián belüli online Tranzakciófeldolgozási videók: Mi van, és ha/hogyan toouse azt</span><span class="sxs-lookup"><span data-stu-id="8d50f-143">In-Memory OLTP Videos: What it is and When/How toouse it</span></span>](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/10/03/in-memory-oltp-video-what-it-is-and-whenhow-to-use-it/)
- [<span data-ttu-id="8d50f-144">Az Oszloptárindex: A memórián belüli Analytics videók Ignite 2016</span><span class="sxs-lookup"><span data-stu-id="8d50f-144">Columnstore Index: In-Memory Analytics Videos from Ignite 2016</span></span>](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/10/04/columnstore-index-in-memory-analytics-i-e-columnstore-index-videos-from-ignite-2016/)

## <a name="storage-and-data-size"></a><span data-ttu-id="8d50f-145">Tárolás és az adatok mérete</span><span class="sxs-lookup"><span data-stu-id="8d50f-145">Storage and data size</span></span>

### <a name="data-size-and-storage-cap-for-in-memory-oltp"></a><span data-ttu-id="8d50f-146">A memórián belüli online Tranzakciófeldolgozási az adatok méretét és a tárolási cap</span><span class="sxs-lookup"><span data-stu-id="8d50f-146">Data size and storage cap for In-Memory OLTP</span></span>

<span data-ttu-id="8d50f-147">A memórián belüli online Tranzakciófeldolgozási memóriaoptimalizált táblákkal, felhasználói adatok tárolásához használt tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="8d50f-147">In-Memory OLTP includes memory-optimized tables, which are used for storing user data.</span></span> <span data-ttu-id="8d50f-148">Ezek a táblázatok a memóriában szükséges toofit.</span><span class="sxs-lookup"><span data-stu-id="8d50f-148">These tables are required toofit in memory.</span></span> <span data-ttu-id="8d50f-149">Kezelheti közvetlenül az SQL Database szolgáltatás hello memória, mert van felhasználói adatok egy kvótát hello fogalmát.</span><span class="sxs-lookup"><span data-stu-id="8d50f-149">Because you manage memory directly in hello SQL Database service, we have hello  concept of a quota for user data.</span></span> <span data-ttu-id="8d50f-150">Ezzel az ötlettel hivatkozott tooas *memórián belüli online Tranzakciófeldolgozási tárolási*.</span><span class="sxs-lookup"><span data-stu-id="8d50f-150">This idea is referred tooas *In-Memory OLTP storage*.</span></span>

<span data-ttu-id="8d50f-151">Minden tarifacsomag és minden rugalmas készlet árképzési szint támogatott önálló adatbázis bizonyos mennyiségű memórián belüli online Tranzakciófeldolgozási tárolási tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="8d50f-151">Each supported standalone database pricing tier and each elastic pool pricing tier includes a certain amount of In-Memory OLTP storage.</span></span> <span data-ttu-id="8d50f-152">Hello írásának időpontjában kap egy gigabájtos méretű tárhely minden 125 adatbázis-tranzakciós egységek (dtu-i) vagy a rugalmas adatbázis tranzakciós egységek (edtu-k).</span><span class="sxs-lookup"><span data-stu-id="8d50f-152">At hello time of writing, you get a gigabyte of storage for every 125 database transaction units (DTUs) or elastic database transaction units (eDTUs).</span></span>

<span data-ttu-id="8d50f-153">Hello [SQL adatbázis szolgáltatásszintjeinek](sql-database-service-tiers.md) cikk rendelkezik hello hello memórián belüli online Tranzakciófeldolgozási elérhető tároló minden támogatott önálló adatbázis és a rugalmas készlet árképzési szint hivatalos listája.</span><span class="sxs-lookup"><span data-stu-id="8d50f-153">hello [SQL Database service tiers](sql-database-service-tiers.md) article has hello official list of hello In-Memory OLTP storage that is available for each supported standalone database and elastic pool pricing tier.</span></span>

<span data-ttu-id="8d50f-154">a következő elemek száma a memórián belüli online Tranzakciófeldolgozási tárolási cap felé hello:</span><span class="sxs-lookup"><span data-stu-id="8d50f-154">hello following items count toward your In-Memory OLTP storage cap:</span></span>

- <span data-ttu-id="8d50f-155">A memóriaoptimalizált táblák és Táblaváltozók adatsorok aktív felhasználónként.</span><span class="sxs-lookup"><span data-stu-id="8d50f-155">Active user data rows in memory-optimized tables and table variables.</span></span> <span data-ttu-id="8d50f-156">Vegye figyelembe, hogy a régi sor verziók száma nem hello cap felé.</span><span class="sxs-lookup"><span data-stu-id="8d50f-156">Note that old row versions don't count toward hello cap.</span></span>
- <span data-ttu-id="8d50f-157">A memóriaoptimalizált táblák indexei.</span><span class="sxs-lookup"><span data-stu-id="8d50f-157">Indexes on memory-optimized tables.</span></span>
- <span data-ttu-id="8d50f-158">Az ALTER TABLE műveletek működési munkaterhelés.</span><span class="sxs-lookup"><span data-stu-id="8d50f-158">Operational overhead of ALTER TABLE operations.</span></span>

<span data-ttu-id="8d50f-159">Találati hello kap, ha ki a kvóta hibaüzenetet kap, és már nem képes adatok tooinsert vagy frissítés áll.</span><span class="sxs-lookup"><span data-stu-id="8d50f-159">If you hit hello cap, you receive an out-of-quota error, and you are no longer able tooinsert or update data.</span></span> <span data-ttu-id="8d50f-160">toomitigate Ez a hiba törli az adatokat, vagy növelje a hello adatbázis vagy a készlet tarifacsomagjának hello.</span><span class="sxs-lookup"><span data-stu-id="8d50f-160">toomitigate this error, delete data or increase hello pricing tier of hello database or pool.</span></span>

<span data-ttu-id="8d50f-161">További információk a tárhely kihasználtságát a memórián belüli online Tranzakciófeldolgozási figyelése és riasztások konfigurálása, ha majdnem elérte hello cap: [figyelő memórián belüli tároló](sql-database-in-memory-oltp-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="8d50f-161">For details about monitoring In-Memory OLTP storage utilization and configuring alerts when you almost hit hello cap, see [Monitor In-Memory storage](sql-database-in-memory-oltp-monitoring.md).</span></span>

#### <a name="about-elastic-pools"></a><span data-ttu-id="8d50f-162">Tudnivalók a rugalmas készletek</span><span class="sxs-lookup"><span data-stu-id="8d50f-162">About elastic pools</span></span>

<span data-ttu-id="8d50f-163">A rugalmas készletek hello memórián belüli online Tranzakciófeldolgozási tároló összes adatbázis hello készletben által megosztott.</span><span class="sxs-lookup"><span data-stu-id="8d50f-163">With elastic pools, hello In-Memory OLTP storage is shared across all databases in hello pool.</span></span> <span data-ttu-id="8d50f-164">Ezért egy adatbázis hello használata hatással lehet más adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="8d50f-164">Therefore, hello usage in one database can potentially affect other databases.</span></span> <span data-ttu-id="8d50f-165">Ez a két megoldást a következők:</span><span class="sxs-lookup"><span data-stu-id="8d50f-165">Two mitigations for this are:</span></span>

- <span data-ttu-id="8d50f-166">Állítsa a Max-edtu-k az adatbázisok, amely kisebb, mint hello adatbáziskészlet egészét hello edtu-k száma.</span><span class="sxs-lookup"><span data-stu-id="8d50f-166">Configure a Max-eDTU for databases that is lower than hello eDTU count for hello pool as a whole.</span></span> <span data-ttu-id="8d50f-167">A maximális caps hello memórián belüli online Tranzakciófeldolgozási tárhely kihasználtságát, bármely adatbázis hello készletben, toohello méretének megfelelő toohello edtu-k száma.</span><span class="sxs-lookup"><span data-stu-id="8d50f-167">This maximum caps hello In-Memory OLTP storage utilization, in any database in hello pool, toohello size that corresponds toohello eDTU count.</span></span>
- <span data-ttu-id="8d50f-168">Állítsa a Min-edtu-k, amely nagyobb, mint 0.</span><span class="sxs-lookup"><span data-stu-id="8d50f-168">Configure a Min-eDTU that is greater than 0.</span></span> <span data-ttu-id="8d50f-169">A minimális garantálja, hogy rendelkezik-e a memórián belüli online Tranzakciófeldolgozási tárolóhellyel toohello megfelelő mennyiségű hello hello készlet minden egyes adatbázis konfigurált minimális-edtu-ra.</span><span class="sxs-lookup"><span data-stu-id="8d50f-169">This minimum guarantees that each database in hello pool has hello amount of available In-Memory OLTP storage that corresponds toohello configured Min-eDTU.</span></span>

### <a name="data-size-and-storage-for-columnstore-indexes"></a><span data-ttu-id="8d50f-170">Adatok mérete és az oszlopcentrikus indexek tároló</span><span class="sxs-lookup"><span data-stu-id="8d50f-170">Data size and storage for columnstore indexes</span></span>

<span data-ttu-id="8d50f-171">Oszloptárindexek nem szükséges toofit a memóriában.</span><span class="sxs-lookup"><span data-stu-id="8d50f-171">Columnstore indexes aren't required toofit in memory.</span></span> <span data-ttu-id="8d50f-172">Ezért a csak a hello indexek hello mérete cap hello általános adatbázis maximális méretet, amelyet hello ismertetett hello [SQL adatbázis szolgáltatásszintjeinek](sql-database-service-tiers.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="8d50f-172">Therefore, hello only cap on hello size of hello indexes is hello maximum overall database size, which is documented in hello [SQL Database service tiers](sql-database-service-tiers.md) article.</span></span>

<span data-ttu-id="8d50f-173">Fürtözött oszlopcentrikus indexek használata esetén oszlopos tömörítési hello alaptábla tárolására szolgál.</span><span class="sxs-lookup"><span data-stu-id="8d50f-173">When you use clustered columnstore indexes, columnar compression is used for hello base table storage.</span></span> <span data-ttu-id="8d50f-174">Ez a fajta tömörítés jelentősen csökkenti hello tárolási kezdjen a felhasználói adatok, ami azt jelenti, hogy több adatot is elférjen hello adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="8d50f-174">This compression can significantly reduce hello storage footprint of your user data, which means that you can fit more data in hello database.</span></span> <span data-ttu-id="8d50f-175">Hello tömörítési további növelni a [oszlopos archiválási tömörítési](https://msdn.microsoft.com/library/cc280449.aspx#Using Columnstore and Columnstore Archive Compression).</span><span class="sxs-lookup"><span data-stu-id="8d50f-175">And hello compression can be further increased with [columnar archival compression](https://msdn.microsoft.com/library/cc280449.aspx#Using Columnstore and Columnstore Archive Compression).</span></span> <span data-ttu-id="8d50f-176">hello-mel elérhető tömörítési mértékét hello jellege hello adatokat, de 10-szer hello tömörítés nem ritka.</span><span class="sxs-lookup"><span data-stu-id="8d50f-176">hello amount of compression that you can achieve depends on hello nature of hello data, but 10 times hello compression is not uncommon.</span></span>

<span data-ttu-id="8d50f-177">Például ha olyan adatbázist 1 terabájtnál (TB) maximális mérettel és 10-szer hello tömörítési oszlopcentrikus indexek használatával érhetők el, is elférjen 10 TB-nyi felhasználói adatok összesen hello adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="8d50f-177">For example, if you have a database with a maximum size of 1 terabyte (TB) and you achieve 10 times hello compression by using columnstore indexes, you can fit a total of 10 TB of user data in hello database.</span></span>

<span data-ttu-id="8d50f-178">Fürtözetlen oszlopcentrikus indexek használata esetén hello alaptábla továbbra is tárolódik hello hagyományos sortárindex formátumban.</span><span class="sxs-lookup"><span data-stu-id="8d50f-178">When you use nonclustered columnstore indexes, hello base table is still stored in hello traditional rowstore format.</span></span> <span data-ttu-id="8d50f-179">Ezért hello tárhely-megtakarítást, nagy, mint a fürtözött oszlopcentrikus indexek nem.</span><span class="sxs-lookup"><span data-stu-id="8d50f-179">Therefore, hello storage savings aren't as big as with clustered columnstore indexes.</span></span> <span data-ttu-id="8d50f-180">Azonban ha egy hagyományos fürtözött indexek száma a egyetlen oszlopcentrikus indexszel rendelkező, is látható hello tárolási erőforrásigényét hello tábla egy átfogó megtakarítását.</span><span class="sxs-lookup"><span data-stu-id="8d50f-180">However, if you're replacing a number of traditional nonclustered indexes with a single columnstore index, you can still see an overall savings in hello storage footprint for hello table.</span></span>

## <a name="moving-databases-that-use-in-memory-technologies-between-pricing-tiers"></a><span data-ttu-id="8d50f-181">A memóriában technológiák közötti árképzési szinteket használó adatbázisok áthelyezése</span><span class="sxs-lookup"><span data-stu-id="8d50f-181">Moving databases that use In-Memory technologies between pricing tiers</span></span>

<span data-ttu-id="8d50f-182">Nincsenek soha nem azonosított inkompatibilitásokat vagy egyéb probléma magasabb tarifacsomagra, többek között a szabványos tooPremium tooa frissítésekor.</span><span class="sxs-lookup"><span data-stu-id="8d50f-182">There are never any incompatibilities or other problems when you upgrade tooa higher pricing tier, such as from Standard tooPremium.</span></span> <span data-ttu-id="8d50f-183">hello elérhető funkciókat és erőforrások csak növelni.</span><span class="sxs-lookup"><span data-stu-id="8d50f-183">hello available functionality and resources only increase.</span></span>

<span data-ttu-id="8d50f-184">De tarifacsomag visszaminősítés hello negatívan befolyásolhatja az adatbázis.</span><span class="sxs-lookup"><span data-stu-id="8d50f-184">But downgrading hello pricing tier can negatively impact your database.</span></span> <span data-ttu-id="8d50f-185">hello hatás különösen akkor látható, ha Ön a prémium szintű tooStandard visszaminősítését vagy alapvető akkor, ha az adatbázis memórián belüli online Tranzakciófeldolgozási objektumokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="8d50f-185">hello impact is especially apparent when you downgrade from Premium tooStandard or Basic when your database contains In-Memory OLTP objects.</span></span> <span data-ttu-id="8d50f-186">A memóriaoptimalizált táblák és oszlopcentrikus indexek esetében nem érhetők el hello alacsonyabb szintre való visszalépést után (még akkor is, ha akkor is látható maradjon,).</span><span class="sxs-lookup"><span data-stu-id="8d50f-186">Memory-optimized tables, and columnstore indexes, are unavailable after hello downgrade (even if they remain visible).</span></span> <span data-ttu-id="8d50f-187">hello ugyanazok a feltételek vonatkoznak hello rugalmas készlet tarifacsomagjának, vagy az adatbázis áthelyezése a szolgáltatáshoz. A memóriában, alapszintű vagy Standard rugalmas készletet éppen csökkentését.</span><span class="sxs-lookup"><span data-stu-id="8d50f-187">hello same considerations apply when you're lowering hello pricing tier of an elastic pool, or moving a database with In-Memory technologies, into a Standard or Basic elastic pool.</span></span>

### <a name="in-memory-oltp"></a><span data-ttu-id="8d50f-188">Memóriabeli OLTP</span><span class="sxs-lookup"><span data-stu-id="8d50f-188">In-Memory OLTP</span></span>

<span data-ttu-id="8d50f-189">*Alacsonyabb verziójúra változtatása tooBasic/Standard*: memórián belüli online Tranzakciófeldolgozási hello Standard vagy a Basic szint-adatbázisokban nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="8d50f-189">*Downgrading tooBasic/Standard*: In-Memory OLTP isn't supported in databases in hello Standard or Basic tier.</span></span> <span data-ttu-id="8d50f-190">Ezenkívül nem lehetséges toomove egy adatbázist, amelynek a memórián belüli online Tranzakciófeldolgozási objektumok toohello Standard vagy a Basic szint.</span><span class="sxs-lookup"><span data-stu-id="8d50f-190">In addition, it isn't possible toomove a database that has any In-Memory OLTP objects toohello Standard or Basic tier.</span></span>

<span data-ttu-id="8d50f-191">Mielőtt visszaminősítését hello adatbázis tooStandard/egyszerű, távolítsa el az összes memóriaoptimalizált táblák és táblatípusokban, valamint a T-SQL minden natív módon lefordított modulok.</span><span class="sxs-lookup"><span data-stu-id="8d50f-191">Before you downgrade hello database tooStandard/Basic, remove all memory-optimized tables and table types, as well as all natively compiled T-SQL modules.</span></span>

<span data-ttu-id="8d50f-192">A programozott módon toounderstand van e egy adott adatbázisnak támogatja a memórián belüli online Tranzakciófeldolgozási.</span><span class="sxs-lookup"><span data-stu-id="8d50f-192">There is a programmatic way toounderstand whether a given database supports In-Memory OLTP.</span></span> <span data-ttu-id="8d50f-193">A következő Transact-SQL-lekérdezések hello hajthat végre:</span><span class="sxs-lookup"><span data-stu-id="8d50f-193">You can execute hello following Transact-SQL query:</span></span>

```
SELECT DatabasePropertyEx(DB_NAME(), 'IsXTPSupported');
```

<span data-ttu-id="8d50f-194">Ha hello lekérdezés visszaadja az **1**, a memórián belüli online Tranzakciófeldolgozási támogatott ebben az adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="8d50f-194">If hello query returns **1**, In-Memory OLTP is supported in this database.</span></span>


<span data-ttu-id="8d50f-195">*Alacsonyabb verziójúra változtatása tooa alacsonyabb prémium csomagban*: memóriaoptimalizált táblázatok adatait, amely kapcsolódik az IP-címek hello adatbázis hello vagy nem áll rendelkezésre hello rugalmas hello memórián belüli online Tranzakciófeldolgozási tárolási kell férnie.</span><span class="sxs-lookup"><span data-stu-id="8d50f-195">*Downgrading tooa lower Premium tier*: Data in memory-optimized tables must fit within hello In-Memory OLTP storage that is associated with hello pricing tier of hello database or is available in hello elastic pool.</span></span> <span data-ttu-id="8d50f-196">Ha hello adatbázis áthelyezésének vagy IP-címek toolower hello próbálja készletbe, amely nem rendelkezik elegendő memórián belüli online Tranzakciófeldolgozási rendelkezésre álló tár, hello művelet sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="8d50f-196">If you try toolower hello pricing tier or move hello database into a pool that doesn't have enough available In-Memory OLTP storage, hello operation fails.</span></span>

### <a name="columnstore-indexes"></a><span data-ttu-id="8d50f-197">Oszlopcentrikus indexek</span><span class="sxs-lookup"><span data-stu-id="8d50f-197">Columnstore indexes</span></span>

<span data-ttu-id="8d50f-198">*Alacsonyabb verziójúra változtatása tooBasic vagy Standard*: Oszlopcentrikus indexek használata támogatott, csak a hello prémium tarifacsomag, és nem a hello Standard vagy Basic rétegek.</span><span class="sxs-lookup"><span data-stu-id="8d50f-198">*Downgrading tooBasic or Standard*: Columnstore indexes are supported only on hello Premium pricing tier, and not on hello Standard or Basic tiers.</span></span> <span data-ttu-id="8d50f-199">Ha Ön megállapításában, az adatbázis tooStandard vagy alapszintű kiadásra, az oszlopcentrikus index nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="8d50f-199">When you downgrade your database tooStandard or Basic, your columnstore index becomes unavailable.</span></span> <span data-ttu-id="8d50f-200">hello rendszer fenntartja az oszlopcentrikus index, de soha ne használja a hello index.</span><span class="sxs-lookup"><span data-stu-id="8d50f-200">hello system maintains your columnstore index, but it never leverages hello index.</span></span> <span data-ttu-id="8d50f-201">Később vissza tooPremium, az oszlopcentrikus index nem frissíthető azonnal készen toobe újra alkalmazhatók.</span><span class="sxs-lookup"><span data-stu-id="8d50f-201">If you later upgrade back tooPremium, your columnstore index is immediately ready toobe leveraged again.</span></span>

<span data-ttu-id="8d50f-202">Ha rendelkezik egy **fürtözött** oszlopcentrikus index hello egész tábla nem érhető el réteg alacsonyabb szintre való visszalépést után.</span><span class="sxs-lookup"><span data-stu-id="8d50f-202">If you have a **clustered** columnstore index, hello whole table becomes unavailable after tier downgrade.</span></span> <span data-ttu-id="8d50f-203">Ezért ajánlott minden drop *fürtözött* oszlopcentrikus indexeket, az alábbi hello prémium csomagban adatbázis visszaminősítését előtt.</span><span class="sxs-lookup"><span data-stu-id="8d50f-203">Therefore we recommend that you drop all *clustered* columnstore indexes before you downgrade your database below hello Premium tier.</span></span>

<span data-ttu-id="8d50f-204">*Alacsonyabb verziójúra változtatása tooa alacsonyabb prémium csomagban*: az alacsonyabb szintre való visszalépést sikeres lesz, ha hello teljes adatbázis megfelelő hello cél IP-címek hello adatbázis maximális méretét, vagy belül hello rendelkezésre álló tár hello a rugalmas készletben.</span><span class="sxs-lookup"><span data-stu-id="8d50f-204">*Downgrading tooa lower Premium tier*: This downgrade succeeds if hello whole database fits within hello maximum database size for hello target pricing tier, or within hello available storage in hello elastic pool.</span></span> <span data-ttu-id="8d50f-205">Nincs a hello oszlopcentrikus indexek adott hatással.</span><span class="sxs-lookup"><span data-stu-id="8d50f-205">There is no specific impact from hello columnstore indexes.</span></span>


<a id="install_oltp_manuallink" name="install_oltp_manuallink"></a>

&nbsp;

## <a name="1-install-hello-in-memory-oltp-sample"></a><span data-ttu-id="8d50f-206">1. Hello memórián belüli online Tranzakciófeldolgozási minta telepítése</span><span class="sxs-lookup"><span data-stu-id="8d50f-206">1. Install hello In-Memory OLTP sample</span></span>

<span data-ttu-id="8d50f-207">Hello AdventureWorksLT mintaadatbázis hello mindössze néhány kattintással létrehozhat [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="8d50f-207">You can create hello AdventureWorksLT sample database with a few clicks in hello [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="8d50f-208">Ezt követően hello részben ebben a szakaszban megtudhatja, hogyan a memórián belüli online Tranzakciófeldolgozási objektumok AdventureWorksLT adatbázis kiegészítése és teljesítménybeli előnyökben bemutatása.</span><span class="sxs-lookup"><span data-stu-id="8d50f-208">Then, hello steps in this section explain how you can enrich your AdventureWorksLT database with In-Memory OLTP objects and demonstrate performance benefits.</span></span>

<span data-ttu-id="8d50f-209">A több simplistic, de több tetszetős teljesítmény bemutató a memórián belüli online Tranzakciófeldolgozási lásd:</span><span class="sxs-lookup"><span data-stu-id="8d50f-209">For a more simplistic, but more visually appealing performance demo for In-Memory OLTP, see:</span></span>

- <span data-ttu-id="8d50f-210">Kiadás: [a-memória-oltp-bemutató-1.0-s verzió](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)</span><span class="sxs-lookup"><span data-stu-id="8d50f-210">Release: [in-memory-oltp-demo-v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)</span></span>
- <span data-ttu-id="8d50f-211">Forráskód: [in-memory-oltp-demo-source-code](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/in-memory/ticket-reservations)</span><span class="sxs-lookup"><span data-stu-id="8d50f-211">Source code: [in-memory-oltp-demo-source-code](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/in-memory/ticket-reservations)</span></span>

#### <a name="installation-steps"></a><span data-ttu-id="8d50f-212">Telepítés lépései</span><span class="sxs-lookup"><span data-stu-id="8d50f-212">Installation steps</span></span>

1. <span data-ttu-id="8d50f-213">A hello [Azure-portálon](https://portal.azure.com/), Premium adatbázis létrehozása a kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="8d50f-213">In hello [Azure portal](https://portal.azure.com/), create a Premium database on a server.</span></span> <span data-ttu-id="8d50f-214">Set hello **forrás** toohello AdventureWorksLT mintaadatbázis.</span><span class="sxs-lookup"><span data-stu-id="8d50f-214">Set hello **Source** toohello AdventureWorksLT sample database.</span></span> <span data-ttu-id="8d50f-215">Részletes útmutatásért lásd: [az első Azure SQL-adatbázis létrehozása](sql-database-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="8d50f-215">For detailed instructions, see [Create your first Azure SQL database](sql-database-get-started-portal.md).</span></span>

2. <span data-ttu-id="8d50f-216">Csatlakozás SQL Server Management Studio toohello adatbázis [(SSMS.exe)](http://msdn.microsoft.com/library/mt238290.aspx).</span><span class="sxs-lookup"><span data-stu-id="8d50f-216">Connect toohello database with SQL Server Management Studio [(SSMS.exe)](http://msdn.microsoft.com/library/mt238290.aspx).</span></span>

3. <span data-ttu-id="8d50f-217">Másolás hello [memórián belüli online Tranzakciófeldolgozási Transact-SQL parancsfájl](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_oltp_sample.sql) tooyour vágólapra.</span><span class="sxs-lookup"><span data-stu-id="8d50f-217">Copy hello [In-Memory OLTP Transact-SQL script](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_oltp_sample.sql) tooyour clipboard.</span></span> <span data-ttu-id="8d50f-218">hello T-SQL parancsfájlt hoz létre hello szükséges memórián belüli objektumok hello AdventureWorksLT példaadatbázisát, amelyet az 1. lépésben létrehozott.</span><span class="sxs-lookup"><span data-stu-id="8d50f-218">hello T-SQL script creates hello necessary In-Memory objects in hello AdventureWorksLT sample database that you created in step 1.</span></span>

4. <span data-ttu-id="8d50f-219">Hello T-SQL parancsfájl beillesztése SSMS és hajthat végre hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="8d50f-219">Paste hello T-SQL script into SSMS, and then execute hello script.</span></span> <span data-ttu-id="8d50f-220">Hello `MEMORY_OPTIMIZED = ON` záradék CREATE TABLE utasítás fontosságúak.</span><span class="sxs-lookup"><span data-stu-id="8d50f-220">hello `MEMORY_OPTIMIZED = ON` clause CREATE TABLE statements are crucial.</span></span> <span data-ttu-id="8d50f-221">Példa:</span><span class="sxs-lookup"><span data-stu-id="8d50f-221">For example:</span></span>


```
CREATE TABLE [SalesLT].[SalesOrderHeader_inmem](
    [SalesOrderID] int IDENTITY NOT NULL PRIMARY KEY NONCLUSTERED ...,
    ...
) WITH (MEMORY_OPTIMIZED = ON);
```


#### <a name="error-40536"></a><span data-ttu-id="8d50f-222">Hiba 40536</span><span class="sxs-lookup"><span data-stu-id="8d50f-222">Error 40536</span></span>


<span data-ttu-id="8d50f-223">Ha hiba 40536 hello T-SQL parancsfájl futtatásakor, futtassa a következő T-SQL parancsfájl tooverify, hogy hello adatbázis támogatja a memórián belüli hello:</span><span class="sxs-lookup"><span data-stu-id="8d50f-223">If you get error 40536 when you run hello T-SQL script, run hello following T-SQL script tooverify whether hello database supports In-Memory:</span></span>


```
SELECT DatabasePropertyEx(DB_Name(), 'IsXTPSupported');
```


<span data-ttu-id="8d50f-224">Eredménye **0** azt jelenti, hogy a memóriában nem támogatott, és **1** azt jelenti, hogy esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="8d50f-224">A result of **0** means that In-Memory isn't supported, and **1** means that it is supported.</span></span> <span data-ttu-id="8d50f-225">toodiagnose hello probléma, győződjön meg arról, hogy hello az adatbázis jelenleg hello prémium szolgáltatásszintet.</span><span class="sxs-lookup"><span data-stu-id="8d50f-225">toodiagnose hello problem, ensure that hello database is at hello Premium service tier.</span></span>


#### <a name="about-hello-created-memory-optimized-items"></a><span data-ttu-id="8d50f-226">Hello kapcsolatos létre memóriaoptimalizált elemek</span><span class="sxs-lookup"><span data-stu-id="8d50f-226">About hello created memory-optimized items</span></span>

<span data-ttu-id="8d50f-227">**Táblák**: hello minta tartalmaz a következő memóriaoptimalizált táblák hello:</span><span class="sxs-lookup"><span data-stu-id="8d50f-227">**Tables**: hello sample contains hello following memory-optimized tables:</span></span>

- <span data-ttu-id="8d50f-228">SalesLT.Product_inmem</span><span class="sxs-lookup"><span data-stu-id="8d50f-228">SalesLT.Product_inmem</span></span>
- <span data-ttu-id="8d50f-229">SalesLT.SalesOrderHeader_inmem</span><span class="sxs-lookup"><span data-stu-id="8d50f-229">SalesLT.SalesOrderHeader_inmem</span></span>
- <span data-ttu-id="8d50f-230">SalesLT.SalesOrderDetail_inmem</span><span class="sxs-lookup"><span data-stu-id="8d50f-230">SalesLT.SalesOrderDetail_inmem</span></span>
- <span data-ttu-id="8d50f-231">Demo.DemoSalesOrderHeaderSeed</span><span class="sxs-lookup"><span data-stu-id="8d50f-231">Demo.DemoSalesOrderHeaderSeed</span></span>
- <span data-ttu-id="8d50f-232">Demo.DemoSalesOrderDetailSeed</span><span class="sxs-lookup"><span data-stu-id="8d50f-232">Demo.DemoSalesOrderDetailSeed</span></span>


<span data-ttu-id="8d50f-233">Memóriaoptimalizált táblák keresztül hello vizsgálhatja **Object Explorer** szolgáltatáshoz az ssms.</span><span class="sxs-lookup"><span data-stu-id="8d50f-233">You can inspect memory-optimized tables through hello **Object Explorer** in SSMS.</span></span> <span data-ttu-id="8d50f-234">Kattintson a jobb gombbal **táblák** > **szűrő** > **beállítások szűrése** > **Memóriaoptimalizált**.</span><span class="sxs-lookup"><span data-stu-id="8d50f-234">Right-click **Tables** > **Filter** > **Filter Settings** > **Is Memory Optimized**.</span></span> <span data-ttu-id="8d50f-235">hello értéke 1.</span><span class="sxs-lookup"><span data-stu-id="8d50f-235">hello value equals 1.</span></span>


<span data-ttu-id="8d50f-236">Vagy hello katalógusnézetekre, például:</span><span class="sxs-lookup"><span data-stu-id="8d50f-236">Or you can query hello catalog views, such as:</span></span>


```
SELECT is_memory_optimized, name, type_desc, durability_desc
    FROM sys.tables
    WHERE is_memory_optimized = 1;
```


<span data-ttu-id="8d50f-237">**Natív módon lefordított tárolt eljárás**: SalesLT.usp_InsertSalesOrder_inmem vizsgálhatja lekérdezéssel-katalógus megtekintése:</span><span class="sxs-lookup"><span data-stu-id="8d50f-237">**Natively compiled stored procedure**: You can inspect SalesLT.usp_InsertSalesOrder_inmem through a catalog view query:</span></span>


```
SELECT uses_native_compilation, OBJECT_NAME(object_id), definition
    FROM sys.sql_modules
    WHERE uses_native_compilation = 1;
```


&nbsp;

### <a name="run-hello-sample-oltp-workload"></a><span data-ttu-id="8d50f-238">Hello minta OLTP-munkaterhelés futtatása</span><span class="sxs-lookup"><span data-stu-id="8d50f-238">Run hello sample OLTP workload</span></span>

<span data-ttu-id="8d50f-239">csak a következő két hello közötti különbség hello *tárolt eljárások* , hogy hello első eljárás hello táblák memóriaoptimalizált verzióját használja, akkor közben hello második eljárás hello lemezen tábláit használja:</span><span class="sxs-lookup"><span data-stu-id="8d50f-239">hello only difference between hello following two *stored procedures* is that hello first procedure uses memory-optimized versions of hello tables, while hello second procedure uses hello regular on-disk tables:</span></span>

- <span data-ttu-id="8d50f-240">SalesLT**.** usp_InsertSalesOrder**_inmem**</span><span class="sxs-lookup"><span data-stu-id="8d50f-240">SalesLT**.**usp_InsertSalesOrder**_inmem**</span></span>
- <span data-ttu-id="8d50f-241">SalesLT**.** usp_InsertSalesOrder**_ondisk**</span><span class="sxs-lookup"><span data-stu-id="8d50f-241">SalesLT**.**usp_InsertSalesOrder**_ondisk**</span></span>


<span data-ttu-id="8d50f-242">Ebben a szakaszban láthatja, hogyan toouse hello hasznos **ostress.exe** segédprogram tooexecute hello stressful szinten két tárolt eljárásokat.</span><span class="sxs-lookup"><span data-stu-id="8d50f-242">In this section, you see how toouse hello handy **ostress.exe** utility tooexecute hello two stored procedures at stressful levels.</span></span> <span data-ttu-id="8d50f-243">Mennyi ideig tart a hello két magas terhelés futtatása toofinish is összehasonlíthatja.</span><span class="sxs-lookup"><span data-stu-id="8d50f-243">You can compare how long it takes for hello two stress runs toofinish.</span></span>


<span data-ttu-id="8d50f-244">Ostress.exe futtatásakor azt javasoljuk, hogy mindkét hello alábbi tervezett paraméterértékek át:</span><span class="sxs-lookup"><span data-stu-id="8d50f-244">When you run ostress.exe, we recommend that you pass parameter values designed for both of hello following:</span></span>

- <span data-ttu-id="8d50f-245">Futtassa a nagyszámú egyidejű kapcsolatok segítségével - n100.</span><span class="sxs-lookup"><span data-stu-id="8d50f-245">Run a large number of concurrent connections, by using -n100.</span></span>
- <span data-ttu-id="8d50f-246">Minden kapcsolat hurok több száz időpontokban, a rendelkezik használatával - r500.</span><span class="sxs-lookup"><span data-stu-id="8d50f-246">Have each connection loop hundreds of times, by using -r500.</span></span>


<span data-ttu-id="8d50f-247">Azonban érdemes toostart sokkal kisebb értékekkel, például - n10 és - r 50 tooensure, amely minden működik.</span><span class="sxs-lookup"><span data-stu-id="8d50f-247">However, you might want toostart with much smaller values like -n10 and -r50 tooensure that everything is working.</span></span>


### <a name="script-for-ostressexe"></a><span data-ttu-id="8d50f-248">Ostress.exe parancsfájl</span><span class="sxs-lookup"><span data-stu-id="8d50f-248">Script for ostress.exe</span></span>


<span data-ttu-id="8d50f-249">Ez a szakasz a ostress.exe parancssori beágyazott hello T-SQL-parancsfájl megjeleníti.</span><span class="sxs-lookup"><span data-stu-id="8d50f-249">This section displays hello T-SQL script that is embedded in our ostress.exe command line.</span></span> <span data-ttu-id="8d50f-250">a parancsfájl hello hello korábban telepített T-SQL-parancsfájl által létrehozott elemek.</span><span class="sxs-lookup"><span data-stu-id="8d50f-250">hello script uses items that were created by hello T-SQL script that you installed earlier.</span></span>


<span data-ttu-id="8d50f-251">hello következő parancsfájl illeszt be egy minta értékesítési sorrendben öt sor elemekhez memóriaoptimalizált hello következő *táblák*:</span><span class="sxs-lookup"><span data-stu-id="8d50f-251">hello following script inserts a sample sales order with five line items into hello following memory-optimized *tables*:</span></span>

- <span data-ttu-id="8d50f-252">SalesLT.SalesOrderHeader_inmem</span><span class="sxs-lookup"><span data-stu-id="8d50f-252">SalesLT.SalesOrderHeader_inmem</span></span>
- <span data-ttu-id="8d50f-253">SalesLT.SalesOrderDetail_inmem</span><span class="sxs-lookup"><span data-stu-id="8d50f-253">SalesLT.SalesOrderDetail_inmem</span></span>


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


<span data-ttu-id="8d50f-254">toomake hello *_ondisk* verziója hello ostress.exe előző T-SQL-parancsfájlt, cserélje a hello mindkét előfordulását *_inmem* a karakterláncrészletre *_ondisk*.</span><span class="sxs-lookup"><span data-stu-id="8d50f-254">toomake hello *_ondisk* version of hello preceding T-SQL script for ostress.exe, you would replace both occurrences of hello *_inmem* substring with *_ondisk*.</span></span> <span data-ttu-id="8d50f-255">Ezek cserékhez hatással vannak a táblák és tárolt eljárások hello nevét.</span><span class="sxs-lookup"><span data-stu-id="8d50f-255">These replacements affect hello names of tables and stored procedures.</span></span>


### <a name="install-rml-utilities-and-ostress"></a><span data-ttu-id="8d50f-256">RML segédprogramok és ostress telepítése</span><span class="sxs-lookup"><span data-stu-id="8d50f-256">Install RML utilities and ostress</span></span>


<span data-ttu-id="8d50f-257">Ideális esetben egy Azure virtuális gépen (VM) toorun ostress.exe volna tervezi.</span><span class="sxs-lookup"><span data-stu-id="8d50f-257">Ideally, you would plan toorun ostress.exe on an Azure virtual machine (VM).</span></span> <span data-ttu-id="8d50f-258">Akkor kell létrehoznia egy [Azure virtuális gép](https://azure.microsoft.com/documentation/services/virtual-machines/) a hello a AdventureWorksLT adatbázis tartalmazó azonos Azure földrajzi régióban.</span><span class="sxs-lookup"><span data-stu-id="8d50f-258">You would create an [Azure VM](https://azure.microsoft.com/documentation/services/virtual-machines/) in hello same Azure geographic region where your AdventureWorksLT database resides.</span></span> <span data-ttu-id="8d50f-259">De futtathatja ostress.exe inkább a laptopján.</span><span class="sxs-lookup"><span data-stu-id="8d50f-259">But you can run ostress.exe on your laptop instead.</span></span>


<span data-ttu-id="8d50f-260">Hello VM, vagy bármilyen üzemeltetéséhez, akkor válassza, hello ismétlési Markup Language (RML) segédprogramokat telepíthet.</span><span class="sxs-lookup"><span data-stu-id="8d50f-260">On hello VM, or on whatever host you choose, install hello Replay Markup Language (RML) utilities.</span></span> <span data-ttu-id="8d50f-261">hello segédprogramok ostress.exe tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="8d50f-261">hello utilities include ostress.exe.</span></span>

<span data-ttu-id="8d50f-262">További információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="8d50f-262">For more information, see:</span></span>
- <span data-ttu-id="8d50f-263">hello ostress.exe vitafórum [memórián belüli online Tranzakciófeldolgozási adatbázist](http://msdn.microsoft.com/library/mt465764.aspx).</span><span class="sxs-lookup"><span data-stu-id="8d50f-263">hello ostress.exe discussion in [Sample Database for In-Memory OLTP](http://msdn.microsoft.com/library/mt465764.aspx).</span></span>
- <span data-ttu-id="8d50f-264">[A memórián belüli online Tranzakciófeldolgozási adatbázis minta](http://msdn.microsoft.com/library/mt465764.aspx).</span><span class="sxs-lookup"><span data-stu-id="8d50f-264">[Sample Database for In-Memory OLTP](http://msdn.microsoft.com/library/mt465764.aspx).</span></span>
- <span data-ttu-id="8d50f-265">Hello [ostress.exe telepítésének blog](http://blogs.msdn.com/b/psssql/archive/2013/10/29/cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx).</span><span class="sxs-lookup"><span data-stu-id="8d50f-265">hello [blog for installing ostress.exe](http://blogs.msdn.com/b/psssql/archive/2013/10/29/cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx).</span></span>



<!--
dn511655.aspx is for SQL 2014,
[Extensions tooAdventureWorks tooDemonstrate In-Memory OLTP]
(http://msdn.microsoft.com/library/dn511655&#x28;v=sql.120&#x29;.aspx)

whereas for SQL 2016+
[Sample Database for In-Memory OLTP]
(http://msdn.microsoft.com/library/mt465764.aspx)
-->



### <a name="run-hello-inmem-stress-workload-first"></a><span data-ttu-id="8d50f-266">Futtassa a hello *_inmem* emelje ki először munkaterhelés</span><span class="sxs-lookup"><span data-stu-id="8d50f-266">Run hello *_inmem* stress workload first</span></span>


<span data-ttu-id="8d50f-267">Használhat egy *RML Cmd Rákérdezés* ablak toorun a ostress.exe parancssor.</span><span class="sxs-lookup"><span data-stu-id="8d50f-267">You can use an *RML Cmd Prompt* window toorun our ostress.exe command line.</span></span> <span data-ttu-id="8d50f-268">hello parancssori paraméterek ostress való közvetlen:</span><span class="sxs-lookup"><span data-stu-id="8d50f-268">hello command-line parameters direct ostress to:</span></span>

- <span data-ttu-id="8d50f-269">100 kapcsolatok egyidejű futtatását (-n100).</span><span class="sxs-lookup"><span data-stu-id="8d50f-269">Run 100 connections concurrently (-n100).</span></span>
- <span data-ttu-id="8d50f-270">Minden kapcsolat hello T-SQL-parancsfájl futtatása 50 alkalommal (-r 50).</span><span class="sxs-lookup"><span data-stu-id="8d50f-270">Have each connection run hello T-SQL script 50 times (-r50).</span></span>


```
ostress.exe -n100 -r50 -S<servername>.database.windows.net -U<login> -P<password> -d<database> -q -Q"DECLARE @i int = 0, @od SalesLT.SalesOrderDetailType_inmem, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand()* 10000; INSERT INTO @od SELECT OrderQty, ProductID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*60) as int); WHILE (@i < 20) begin; EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od; set @i += 1; end"
```


<span data-ttu-id="8d50f-271">toorun hello megelőző ostress.exe parancssorban:</span><span class="sxs-lookup"><span data-stu-id="8d50f-271">toorun hello preceding ostress.exe command line:</span></span>


1. <span data-ttu-id="8d50f-272">Futtassa a következő parancs szolgáltatáshoz az ssms, toodelete hello összes hello adatok bármely korábbi futtatása által beszúrt hello adatbázis adatok tartalmának alaphelyzetbe állítása:</span><span class="sxs-lookup"><span data-stu-id="8d50f-272">Reset hello database data content by running hello following command in SSMS, toodelete all hello data that was inserted by any previous runs:</span></span>

    ``` tsql
    EXECUTE Demo.usp_DemoReset;
    ```

2. <span data-ttu-id="8d50f-273">Másolja át a megelőző ostress.exe parancssori tooyour vágólapra hello hello szövegét.</span><span class="sxs-lookup"><span data-stu-id="8d50f-273">Copy hello text of hello preceding ostress.exe command line tooyour clipboard.</span></span>

3. <span data-ttu-id="8d50f-274">Cserélje le a hello `<placeholders>` hello paraméterek -S - U -P - d a hello javítsa ki a tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="8d50f-274">Replace hello `<placeholders>` for hello parameters -S -U -P -d with hello correct real values.</span></span>

4. <span data-ttu-id="8d50f-275">A szerkesztett parancssor futtatása a egy RML Cmd ablakot.</span><span class="sxs-lookup"><span data-stu-id="8d50f-275">Run your edited command line in an RML Cmd window.</span></span>


#### <a name="result-is-a-duration"></a><span data-ttu-id="8d50f-276">Eredménye egy időtartam</span><span class="sxs-lookup"><span data-stu-id="8d50f-276">Result is a duration</span></span>


<span data-ttu-id="8d50f-277">Ostress.exe befejezésekor időtartama futtató kimeneti hello RML Cmd ablakban a végső üzletági hello ír.</span><span class="sxs-lookup"><span data-stu-id="8d50f-277">When ostress.exe finishes, it writes hello run duration as its final line of output in hello RML Cmd window.</span></span> <span data-ttu-id="8d50f-278">Például egy rövidebb vizsgálat tartott körülbelül 1,5 percet:</span><span class="sxs-lookup"><span data-stu-id="8d50f-278">For example, a shorter test run lasted about 1.5 minutes:</span></span>

`11/12/15 00:35:00.873 [0x000030A8] OSTRESS exiting normally, elapsed time: 00:01:31.867`


#### <a name="reset-edit-for-ondisk-then-rerun"></a><span data-ttu-id="8d50f-279">Alaphelyzetbe állítja, a Szerkesztés *_ondisk*, majd futtassa újra a</span><span class="sxs-lookup"><span data-stu-id="8d50f-279">Reset, edit for *_ondisk*, then rerun</span></span>


<span data-ttu-id="8d50f-280">Miután hello hello eredményt *_inmem* futnak, hajtsa végre a következő lépéseket a hello hello *_ondisk* futtatása:</span><span class="sxs-lookup"><span data-stu-id="8d50f-280">After you have hello result from hello *_inmem* run, perform hello following steps for hello *_ondisk* run:</span></span>


1. <span data-ttu-id="8d50f-281">Hello adatbázis visszaállítása a következő parancs az SSMS toodelete hello összes hello adatok hello előző futtatása által beszúrt futtatásával:</span><span class="sxs-lookup"><span data-stu-id="8d50f-281">Reset hello database by running hello following command in SSMS toodelete all hello data that was inserted by hello previous run:</span></span>
```
EXECUTE Demo.usp_DemoReset;
```

2. <span data-ttu-id="8d50f-282">Hello ostress.exe parancssori tooreplace összes szerkesztése *_inmem* rendelkező *_ondisk*.</span><span class="sxs-lookup"><span data-stu-id="8d50f-282">Edit hello ostress.exe command line tooreplace all *_inmem* with *_ondisk*.</span></span>

3. <span data-ttu-id="8d50f-283">Futtassa újra a hello ostress.exe még egyszer, és rögzítse hello időtartama eredménye.</span><span class="sxs-lookup"><span data-stu-id="8d50f-283">Rerun ostress.exe for hello second time, and capture hello duration result.</span></span>

4. <span data-ttu-id="8d50f-284">Ebben az esetben alaphelyzetbe állítása hello adatbázis (Mi lehet a Tesztadatok nagy mennyiségű osztott törlése).</span><span class="sxs-lookup"><span data-stu-id="8d50f-284">Again, reset hello database (for responsibly deleting what can be a large amount of test data).</span></span>


#### <a name="expected-comparison-results"></a><span data-ttu-id="8d50f-285">Várt összehasonlítás eredménye</span><span class="sxs-lookup"><span data-stu-id="8d50f-285">Expected comparison results</span></span>

<span data-ttu-id="8d50f-286">A memóriában tesztek kimutatták, hogy javítja a teljesítményt **kilenc alkalommal** simplistic munkaterhelés, a ostress rendelkező a egy Azure virtuális Gépen futó hello azonos Azure-régiót hello adatbázisként.</span><span class="sxs-lookup"><span data-stu-id="8d50f-286">Our In-Memory tests have shown that performance improved by **nine times** for this simplistic workload, with ostress running on an Azure VM in hello same Azure region as hello database.</span></span>

<a id="install_analytics_manuallink" name="install_analytics_manuallink"></a>

&nbsp;

## <a name="2-install-hello-in-memory-analytics-sample"></a><span data-ttu-id="8d50f-287">2. Hello memórián belüli Analytics minta telepítése</span><span class="sxs-lookup"><span data-stu-id="8d50f-287">2. Install hello In-Memory Analytics sample</span></span>


<span data-ttu-id="8d50f-288">Ebben a szakaszban összehasonlítja hello IO és a statisztika eredmények oszlopcentrikus index és egy hagyományos b-fa index használatakor.</span><span class="sxs-lookup"><span data-stu-id="8d50f-288">In this section, you compare hello IO and statistics results when you're using a columnstore index versus a traditional b-tree index.</span></span>


<span data-ttu-id="8d50f-289">Az OLTP-munkaterhelés a valós idejű elemzési esetén gyakran legjobb toouse egy fürtözetlen oszlopcentrikus indexet.</span><span class="sxs-lookup"><span data-stu-id="8d50f-289">For real-time analytics on an OLTP workload, it's often best toouse a nonclustered columnstore index.</span></span> <span data-ttu-id="8d50f-290">További információkért lásd: [Oszlopcentrikus indexek leírt](http://msdn.microsoft.com/library/gg492088.aspx).</span><span class="sxs-lookup"><span data-stu-id="8d50f-290">For details, see [Columnstore Indexes Described](http://msdn.microsoft.com/library/gg492088.aspx).</span></span>



### <a name="prepare-hello-columnstore-analytics-test"></a><span data-ttu-id="8d50f-291">Készítse elő a hello oszlopcentrikus analytics tesztelése</span><span class="sxs-lookup"><span data-stu-id="8d50f-291">Prepare hello columnstore analytics test</span></span>


1. <span data-ttu-id="8d50f-292">Hello Azure portál toocreate hello minta egy friss AdventureWorksLT adatbázisát használja.</span><span class="sxs-lookup"><span data-stu-id="8d50f-292">Use hello Azure portal toocreate a fresh AdventureWorksLT database from hello sample.</span></span>
 - <span data-ttu-id="8d50f-293">Használja ezt a teljes nevet.</span><span class="sxs-lookup"><span data-stu-id="8d50f-293">Use that exact name.</span></span>
 - <span data-ttu-id="8d50f-294">Válassza ki a prémium szolgáltatásszintet.</span><span class="sxs-lookup"><span data-stu-id="8d50f-294">Choose any Premium service tier.</span></span>

2. <span data-ttu-id="8d50f-295">Másolás hello [sql_in-memory_analytics_sample](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_analytics_sample.sql) tooyour vágólapra.</span><span class="sxs-lookup"><span data-stu-id="8d50f-295">Copy hello [sql_in-memory_analytics_sample](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_analytics_sample.sql) tooyour clipboard.</span></span>
 - <span data-ttu-id="8d50f-296">hello T-SQL parancsfájlt hoz létre hello szükséges memórián belüli objektumok hello AdventureWorksLT példaadatbázisát, amelyet az 1. lépésben létrehozott.</span><span class="sxs-lookup"><span data-stu-id="8d50f-296">hello T-SQL script creates hello necessary In-Memory objects in hello AdventureWorksLT sample database that you created in step 1.</span></span>
 - <span data-ttu-id="8d50f-297">hello parancsfájl hello dimenziótáblában és két ténytáblák hoz létre.</span><span class="sxs-lookup"><span data-stu-id="8d50f-297">hello script creates hello Dimension table and two fact tables.</span></span> <span data-ttu-id="8d50f-298">hello ténytáblák feltöltött 3.5 millió sort.</span><span class="sxs-lookup"><span data-stu-id="8d50f-298">hello fact tables are populated with 3.5 million rows each.</span></span>
 - <span data-ttu-id="8d50f-299">hello parancsfájl toocomplete 15 percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="8d50f-299">hello script might take 15 minutes toocomplete.</span></span>

3. <span data-ttu-id="8d50f-300">Hello T-SQL parancsfájl beillesztése SSMS és hajthat végre hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="8d50f-300">Paste hello T-SQL script into SSMS, and then execute hello script.</span></span> <span data-ttu-id="8d50f-301">Hello **OSZLOPCENTRIKUS** hello kulcsszót **a CREATE INDEX** utasítás elengedhetetlen, mint:</span><span class="sxs-lookup"><span data-stu-id="8d50f-301">hello **COLUMNSTORE** keyword in hello **CREATE INDEX** statement is crucial, as in:</span></span><br/>`CREATE NONCLUSTERED COLUMNSTORE INDEX ...;`

4. <span data-ttu-id="8d50f-302">AdventureWorksLT toocompatibility szint 130 beállítása:</span><span class="sxs-lookup"><span data-stu-id="8d50f-302">Set AdventureWorksLT toocompatibility level 130:</span></span><br/>`ALTER DATABASE AdventureworksLT SET compatibility_level = 130;`

    <span data-ttu-id="8d50f-303">Szint 130 nem közvetlenül kapcsolódó tooIn-memóriával kapcsolatos szolgáltatásainak.</span><span class="sxs-lookup"><span data-stu-id="8d50f-303">Level 130 is not directly related tooIn-Memory features.</span></span> <span data-ttu-id="8d50f-304">De szint 130 általában gyorsabb lekérdezési teljesítményt, mint 120.</span><span class="sxs-lookup"><span data-stu-id="8d50f-304">But level 130 generally provides faster query performance than 120.</span></span>


#### <a name="key-tables-and-columnstore-indexes"></a><span data-ttu-id="8d50f-305">Kulcs táblák és az oszlopcentrikus indexek</span><span class="sxs-lookup"><span data-stu-id="8d50f-305">Key tables and columnstore indexes</span></span>


- <span data-ttu-id="8d50f-306">dbo. FactResellerSalesXL_CCI táblának fürtözött oszlopcentrikus index, amely továbbfejlesztett tömörítés: hello *adatok* szintjét.</span><span class="sxs-lookup"><span data-stu-id="8d50f-306">dbo.FactResellerSalesXL_CCI is a table that has a clustered columnstore index, which has advanced compression at hello *data* level.</span></span>

- <span data-ttu-id="8d50f-307">dbo. FactResellerSalesXL_PageCompressed táblának egyenértékű rendszeres fürtözött indexet, amely csak a hello tömörített *lap* szintjét.</span><span class="sxs-lookup"><span data-stu-id="8d50f-307">dbo.FactResellerSalesXL_PageCompressed is a table that has an equivalent regular clustered index, which is compressed only at hello *page* level.</span></span>


#### <a name="key-queries-toocompare-hello-columnstore-index"></a><span data-ttu-id="8d50f-308">Kulcs toocompare hello oszlopcentrikus index lekérdezése</span><span class="sxs-lookup"><span data-stu-id="8d50f-308">Key queries toocompare hello columnstore index</span></span>


<span data-ttu-id="8d50f-309">Nincsenek [T-SQL lekérdezés számos különböző futtatható](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/clustered_columnstore_sample_queries.sql) toosee teljesítménynövekedést.</span><span class="sxs-lookup"><span data-stu-id="8d50f-309">There are [several T-SQL query types that you can run](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/clustered_columnstore_sample_queries.sql) toosee performance improvements.</span></span> <span data-ttu-id="8d50f-310">T-SQL parancsfájl hello a 2 nagy figyelmet toothis pár lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="8d50f-310">In step 2 in hello T-SQL script, pay attention toothis pair of queries.</span></span> <span data-ttu-id="8d50f-311">Csak egy sorban különböznek:</span><span class="sxs-lookup"><span data-stu-id="8d50f-311">They differ only on one line:</span></span>


- `FROM FactResellerSalesXL_PageCompressed a`
- `FROM FactResellerSalesXL_CCI a`


<span data-ttu-id="8d50f-312">Fürtözött oszlopcentrikus index van hello FactResellerSalesXL\_közösségi koordináló intézet tábla.</span><span class="sxs-lookup"><span data-stu-id="8d50f-312">A clustered columnstore index is in hello FactResellerSalesXL\_CCI table.</span></span>

<span data-ttu-id="8d50f-313">hello következő T-SQL parancsfájl cikkből kinyomtatja statisztika IO és hello lekérdezések minden tábla ideje.</span><span class="sxs-lookup"><span data-stu-id="8d50f-313">hello following T-SQL script excerpt prints statistics for IO and TIME for hello query of each table.</span></span>


```
/*********************************************************************
Step 2 -- Overview
-- Page Compressed BTree table v/s Columnstore table performance differences
-- Enable actual Query Plan in order toosee Plan differences when Executing
*/
-- Ensure Database is in 130 compatibility mode
ALTER DATABASE AdventureworksLT SET compatibility_level = 130
GO

-- Execute a typical query that joins hello Fact Table with dimension tables
-- Note this query will run on hello Page Compressed table, Note down hello time
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


-- This is hello same Prior query on a table with a clustered columnstore index CCI
-- hello comparison numbers are even more dramatic hello larger hello table is (this is an 11 million row table only)
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

<span data-ttu-id="8d50f-314">Hello P2 tarifacsomag adatbázisban várhatóan hamarosan kilenc alkalommal a jobb teljesítménye hello a ehhez a lekérdezéshez hello hagyományos index képest hello fürtözött oszlopcentrikus index használatával.</span><span class="sxs-lookup"><span data-stu-id="8d50f-314">In a database with hello P2 pricing tier, you can expect about nine times hello performance gain for this query by using hello clustered columnstore index compared with hello traditional index.</span></span> <span data-ttu-id="8d50f-315">A P15 körülbelül 57 alkalommal hello jobb teljesítménye számíthat hello oszlopcentrikus index használatával.</span><span class="sxs-lookup"><span data-stu-id="8d50f-315">With P15, you can expect about 57 times hello performance gain by using hello columnstore index.</span></span>



## <a name="next-steps"></a><span data-ttu-id="8d50f-316">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8d50f-316">Next steps</span></span>

- [<span data-ttu-id="8d50f-317">Gyors üzembe helyezési 1: A memórián belüli online Tranzakciófeldolgozási technológiák a T-SQL jobb teljesítmény</span><span class="sxs-lookup"><span data-stu-id="8d50f-317">Quick Start 1: In-Memory OLTP Technologies for Faster T-SQL Performance</span></span>](http://msdn.microsoft.com/library/mt694156.aspx)

- [<span data-ttu-id="8d50f-318">Használja a memórián belüli online Tranzakciófeldolgozási egy meglévő Azure SQL-alkalmazásban</span><span class="sxs-lookup"><span data-stu-id="8d50f-318">Use In-Memory OLTP in an existing Azure SQL application</span></span>](sql-database-in-memory-oltp-migration.md)

- <span data-ttu-id="8d50f-319">[A figyelő a memórián belüli online Tranzakciófeldolgozási tárolási](sql-database-in-memory-oltp-monitoring.md) a memórián belüli online Tranzakciófeldolgozási</span><span class="sxs-lookup"><span data-stu-id="8d50f-319">[Monitor In-Memory OLTP storage](sql-database-in-memory-oltp-monitoring.md) for In-Memory OLTP</span></span>


## <a name="additional-resources"></a><span data-ttu-id="8d50f-320">További források</span><span class="sxs-lookup"><span data-stu-id="8d50f-320">Additional resources</span></span>

#### <a name="deeper-information"></a><span data-ttu-id="8d50f-321">Mélyebb információk</span><span class="sxs-lookup"><span data-stu-id="8d50f-321">Deeper information</span></span>

- [<span data-ttu-id="8d50f-322">Ismerje meg, hogyan kvórum megduplázódik miközben DTU csökkenti a memórián belüli online Tranzakciófeldolgozási az SQL-adatbázis 70 %-kal kulcsának adatbázis munkaterhelés</span><span class="sxs-lookup"><span data-stu-id="8d50f-322">Learn how Quorum doubles key database’s workload while lowering DTU by 70% with In-Memory OLTP in SQL Database</span></span>](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)

- [<span data-ttu-id="8d50f-323">Memórián belüli online Tranzakciófeldolgozási Azure SQL adatbázis blogbejegyzésben</span><span class="sxs-lookup"><span data-stu-id="8d50f-323">In-Memory OLTP in Azure SQL Database Blog Post</span></span>](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)

- [<span data-ttu-id="8d50f-324">További tudnivalók a memórián belüli online Tranzakciófeldolgozási</span><span class="sxs-lookup"><span data-stu-id="8d50f-324">Learn about In-Memory OLTP</span></span>](http://msdn.microsoft.com/library/dn133186.aspx)

- [<span data-ttu-id="8d50f-325">További tudnivalók az oszlopcentrikus indexek</span><span class="sxs-lookup"><span data-stu-id="8d50f-325">Learn about columnstore indexes</span></span>](https://msdn.microsoft.com/library/gg492088.aspx)

- [<span data-ttu-id="8d50f-326">További információk a valós idejű működési elemzés</span><span class="sxs-lookup"><span data-stu-id="8d50f-326">Learn about real-time operational analytics</span></span>](http://msdn.microsoft.com/library/dn817827.aspx)

- <span data-ttu-id="8d50f-327">Lásd: [közös terhelési mintázatok és az áttelepítés szempontjai](http://msdn.microsoft.com/library/dn673538.aspx) (amely ismerteti, ahol a memórián belüli online Tranzakciófeldolgozási gyakran biztosít teljesítménynövekedéshez munkaterhelés minták)</span><span class="sxs-lookup"><span data-stu-id="8d50f-327">See [Common Workload Patterns and Migration Considerations](http://msdn.microsoft.com/library/dn673538.aspx) (which describes workload patterns where In-Memory OLTP commonly provides significant performance gains)</span></span>

#### <a name="application-design"></a><span data-ttu-id="8d50f-328">Alkalmazás tervezése</span><span class="sxs-lookup"><span data-stu-id="8d50f-328">Application design</span></span>

- [<span data-ttu-id="8d50f-329">Memórián belüli online Tranzakciófeldolgozási (a memóriában optimalizálása)</span><span class="sxs-lookup"><span data-stu-id="8d50f-329">In-Memory OLTP (In-Memory Optimization)</span></span>](http://msdn.microsoft.com/library/dn133186.aspx)

- [<span data-ttu-id="8d50f-330">Használja a memórián belüli online Tranzakciófeldolgozási egy meglévő Azure SQL-alkalmazásban</span><span class="sxs-lookup"><span data-stu-id="8d50f-330">Use In-Memory OLTP in an existing Azure SQL application</span></span>](sql-database-in-memory-oltp-migration.md)

#### <a name="tools"></a><span data-ttu-id="8d50f-331">Eszközök</span><span class="sxs-lookup"><span data-stu-id="8d50f-331">Tools</span></span>

- [<span data-ttu-id="8d50f-332">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8d50f-332">Azure portal</span></span>](https://portal.azure.com/)

- [<span data-ttu-id="8d50f-333">SQL Server Management Studio (SSMS)</span><span class="sxs-lookup"><span data-stu-id="8d50f-333">SQL Server Management Studio (SSMS)</span></span>](https://msdn.microsoft.com/library/mt238290.aspx)

- [<span data-ttu-id="8d50f-334">SQL Server Data Tools (SSDT)</span><span class="sxs-lookup"><span data-stu-id="8d50f-334">SQL Server Data Tools (SSDT)</span></span>](http://msdn.microsoft.com/library/mt204009.aspx)
