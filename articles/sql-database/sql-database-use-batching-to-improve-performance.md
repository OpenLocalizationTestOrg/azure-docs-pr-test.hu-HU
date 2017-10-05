---
title: "Az Azure SQL Database-alkalmazás teljesítményének javítása érdekében a kötegelés használata"
description: "A témakör igazolja, hogy kötegelési adatbázis-műveletek sebessége nagy mértékben imroves és méretezhetőséget biztosít a az Azure SQL adatbázis-alkalmazások. Habár ezek a technológiák kötegelési bármely SQL Server-adatbázis is működik, a cikk célja az Azure-on."
services: sql-database
documentationcenter: na
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 563862ca-c65a-46f6-975d-10df7ff6aa9c
ms.service: sql-database
ms.custom: develop apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/12/2016
ms.author: sstein
ms.openlocfilehash: 22cff47444306e599325ba3035d83a0266d69c72
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-batching-to-improve-sql-database-application-performance"></a><span data-ttu-id="2f2b3-104">SQL-adatbázis teljesítményének javítása érdekében a kötegelés használata</span><span class="sxs-lookup"><span data-stu-id="2f2b3-104">How to use batching to improve SQL Database application performance</span></span>
<span data-ttu-id="2f2b3-105">Az Azure SQL Database-műveletek kötegelése jelentősen javítja a teljesítményét és méretezhetőségét, az alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-105">Batching operations to Azure SQL Database significantly improves the performance and scalability of your applications.</span></span> <span data-ttu-id="2f2b3-106">Előnyeinek megismerése, hogy ez a cikk első része ismertet néhány minta vizsgálati eredmények, hasonlítsa össze az SQL-adatbázis szekvenciális és kötegelt kérelmek.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-106">In order to understand the benefits, the first part of this article covers some sample test results that compare sequential and batched requests to a SQL Database.</span></span> <span data-ttu-id="2f2b3-107">A cikk fennmaradó a technikák, a forgatókönyvek és a szempontokat tartalmaz, amelyek segítséget nyújtanak az Azure-alkalmazásokban sikeresen kötegelés használandó jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-107">The remainder of the article shows the techniques, scenarios, and considerations to help you to use batching successfully in your Azure applications.</span></span>

## <a name="why-is-batching-important-for-sql-database"></a><span data-ttu-id="2f2b3-108">Miért van kötegelés fontos az SQL Database?</span><span class="sxs-lookup"><span data-stu-id="2f2b3-108">Why is batching important for SQL Database?</span></span>
<span data-ttu-id="2f2b3-109">A távoli szolgáltatás hívásainak kötegelés növelését a teljesítmény és méretezhetőség jól ismert stratégiáját.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-109">Batching calls to a remote service is a well-known strategy for increasing performance and scalability.</span></span> <span data-ttu-id="2f2b3-110">Egy távoli szolgáltatással, például a szerializálás, a hálózati átvitel és a deszerializálás kölcsönhatások feldolgozási költségek rögzítettek.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-110">There are fixed processing costs to any interactions with a remote service, such as serialization, network transfer, and deserialization.</span></span> <span data-ttu-id="2f2b3-111">Ezek a költségek azokat a kötegek sok külön tranzakciókat minimálisra csökkenti.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-111">Packaging many separate transactions into a single batch minimizes these costs.</span></span>

<span data-ttu-id="2f2b3-112">A dokumentum azt szeretnénk vizsgálja meg a különböző SQL-adatbázis kötegelés azokat a stratégiákat és forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-112">In this paper, we want to examine various SQL Database batching strategies and scenarios.</span></span> <span data-ttu-id="2f2b3-113">Bár ezek stratégiák is fontos SQL Server használó helyszíni alkalmazások esetén, több oka konzolban kötegelés SQL-adatbázis használatát:</span><span class="sxs-lookup"><span data-stu-id="2f2b3-113">Although these strategies are also important for on-premises applications that use SQL Server, there are several reasons for highlighting the use of batching for SQL Database:</span></span>

* <span data-ttu-id="2f2b3-114">Nincs potenciálisan nagyobb hálózati késés SQL-adatbázis, különösen akkor, ha az ugyanahhoz a Microsoft Azure adatközponton kívülről SQL-adatbázis elérésére.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-114">There is potentially greater network latency in accessing SQL Database, especially if you are accessing SQL Database from outside the same Microsoft Azure datacenter.</span></span>
* <span data-ttu-id="2f2b3-115">SQL-adatbázis több-bérlős jellemzői azt jelenti, hogy az adatok hozzáférési réteg felel meg az adatbázis átfogó méretezhetősége hatékonyságát.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-115">The multitenant characteristics of SQL Database means that the efficiency of the data access layer correlates to the overall scalability of the database.</span></span> <span data-ttu-id="2f2b3-116">SQL-adatbázis az egyes bérlői felhasználók megakadályozása kell legaktívabbak más bérlők hátrányára adatbázis-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-116">SQL Database must prevent any single tenant/user from monopolizing database resources to the detriment of other tenants.</span></span> <span data-ttu-id="2f2b3-117">SQL-adatbázis használati, amelyek átlépik ezt az előre definiált kvóták válaszul, átviteli csökkentheti vagy szabályozási kivételeket válaszolni.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-117">In response to usage in excess of predefined quotas, SQL Database can reduce throughput or respond with throttling exceptions.</span></span> <span data-ttu-id="2f2b3-118">Hatékonyság, például a kötegelés, lehetővé teszik a munkájuk elvégzéséhez további SQL-adatbázis a működés felső korlátjának elérése előtt.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-118">Efficiencies, such as batching, enable you to do more work on SQL Database before reaching these limits.</span></span> 
* <span data-ttu-id="2f2b3-119">Kötegelés esetében is, amelyek több adatbázist (horizontális) architektúrára való hatékony.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-119">Batching is also effective for architectures that use multiple databases (sharding).</span></span> <span data-ttu-id="2f2b3-120">Az adatbázis tárolóegységekhez folytatott kommunikációt hatékonyságát még mindig a teljes méretezhetőség kulcsfontosságú tényező.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-120">The efficiency of your interaction with each database unit is still a key factor in your overall scalability.</span></span> 

<span data-ttu-id="2f2b3-121">Az SQL-adatbázis használatának előnyei egyike, hogy nem kell az adatbázist üzemeltető kiszolgáló kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-121">One of the benefits of using SQL Database is that you don’t have to manage the servers that host the database.</span></span> <span data-ttu-id="2f2b3-122">Azonban a felügyelt infrastruktúra is azt jelenti, hogy másképp gondolniuk adatbázis optimalizálás.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-122">However, this managed infrastructure also means that you have to think differently about database optimizations.</span></span> <span data-ttu-id="2f2b3-123">Már nem megtekintheti az adatbázis hardver- vagy hálózati infrastruktúra javítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-123">You can no longer look to improve the database hardware or network infrastructure.</span></span> <span data-ttu-id="2f2b3-124">A Microsoft Azure határozza meg azokat a környezetben.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-124">Microsoft Azure controls those environments.</span></span> <span data-ttu-id="2f2b3-125">A fő területet, amely befolyásolhatja az SQL-adatbázis és az alkalmazás együttműködését.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-125">The main area that you can control is how your application interacts with SQL Database.</span></span> <span data-ttu-id="2f2b3-126">Kötegelés egyike ezek az optimalizálások.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-126">Batching is one of these optimizations.</span></span> 

<span data-ttu-id="2f2b3-127">A dokumentum első része SQL-adatbázis használata a .NET-alkalmazások különböző kötegelési technikák megvizsgálja.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-127">The first part of the paper examines various batching techniques for .NET applications that use SQL Database.</span></span> <span data-ttu-id="2f2b3-128">Az utolsó két szakaszok fedik le a kötegelési irányelvek és forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-128">The last two sections cover batching guidelines and scenarios.</span></span>

## <a name="batching-strategies"></a><span data-ttu-id="2f2b3-129">Kötegelési stratégiák</span><span class="sxs-lookup"><span data-stu-id="2f2b3-129">Batching strategies</span></span>
### <a name="note-about-timing-results-in-this-topic"></a><span data-ttu-id="2f2b3-130">Megjegyzés: Ebben a témakörben időzítési eredmény</span><span class="sxs-lookup"><span data-stu-id="2f2b3-130">Note about timing results in this topic</span></span>
> [!NOTE]
> <span data-ttu-id="2f2b3-131">Eredmények nem referenciaalapokhoz képest, de van kialakítva, hogy megjelenítése **relatív teljesítménye**.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-131">Results are not benchmarks but are meant to show **relative performance**.</span></span> <span data-ttu-id="2f2b3-132">Időzítés legalább 10 teszt futtatása átlagosan alapulnak.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-132">Timings are based on an average of at least 10 test runs.</span></span> <span data-ttu-id="2f2b3-133">Műveletek esetében a beszúrások, a program üres táblát.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-133">Operations are inserts into an empty table.</span></span> <span data-ttu-id="2f2b3-134">Ezek a tesztek mért előtti-12-es verzióra, és ezek nem feltétlenül felelnek meg, hogy Ön is szembesülhet egy 12-es verziójú adatbázis, az új átviteli [szolgáltatásszintek](sql-database-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="2f2b3-134">These tests were measured pre-V12, and they do not necessarily correspond to throughput that you might experience in a V12 database using the new [service tiers](sql-database-service-tiers.md).</span></span> <span data-ttu-id="2f2b3-135">A relatív előnye, hogy a kötegelési technika hasonlónak kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-135">The relative benefit of the batching technique should be similar.</span></span>
> 
> 

### <a name="transactions"></a><span data-ttu-id="2f2b3-136">Tranzakciók</span><span class="sxs-lookup"><span data-stu-id="2f2b3-136">Transactions</span></span>
<span data-ttu-id="2f2b3-137">Furcsa megvitatása tranzakciók által kötegelés áttekintése megkezdéséhez tűnik.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-137">It seems strange to begin a review of batching by discussing transactions.</span></span> <span data-ttu-id="2f2b3-138">De a tranzakciók ügyféloldali használata finom kiszolgálóoldali kötegelési hatással van, amely javítja a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-138">But the use of client-side transactions has a subtle server-side batching effect that improves performance.</span></span> <span data-ttu-id="2f2b3-139">És a tranzakciók hozzáadása is lehetséges csak néhány sornyi kódot, így gyorsan egymást követő műveletek teljesítményének javításával biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-139">And transactions can be added with only a few lines of code, so they provide a fast way to improve performance of sequential operations.</span></span>

<span data-ttu-id="2f2b3-140">Vegye figyelembe a következő C#-kódban insert sorozatát tartalmazó és a frissítési műveletek egyszerű táblán.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-140">Consider the following C# code that contains a sequence of insert and update operations on a simple table.</span></span>

    List<string> dbOperations = new List<string>();
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 1");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 2");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 3");
    dbOperations.Add("insert MyTable values ('new value',1)");
    dbOperations.Add("insert MyTable values ('new value',2)");
    dbOperations.Add("insert MyTable values ('new value',3)");

<span data-ttu-id="2f2b3-141">A következő ADO.NET kód egymás után végrehajtja ezeket a műveleteket.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-141">The following ADO.NET code sequentially performs these operations.</span></span>

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();

        foreach(string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn);
            cmd.ExecuteNonQuery();                   
        }
    }

<span data-ttu-id="2f2b3-142">A legjobb módja, ez a kód optimalizálása érdekében, hogy az adott hívások kötegelés ügyféloldali valamilyen alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-142">The best way to optimize this code is to implement some form of client-side batching of these calls.</span></span> <span data-ttu-id="2f2b3-143">Azonban ez a kód a teljesítmény növelése érdekében használatával egyszerűen a hívások sorrendjét a tranzakcióban egyszerű módszert.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-143">But there is a simple way to increase the performance of this code by simply wrapping the sequence of calls in a transaction.</span></span> <span data-ttu-id="2f2b3-144">Itt található, amely egy tranzakció használja ugyanazt a kódot.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-144">Here is the same code that uses a transaction.</span></span>

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();
        SqlTransaction transaction = conn.BeginTransaction();

        foreach (string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn, transaction);
            cmd.ExecuteNonQuery();
        }

        transaction.Commit();
    }

<span data-ttu-id="2f2b3-145">Tranzakciók mindkét ezekben a példákban ténylegesen használatban van.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-145">Transactions are actually being used in both of these examples.</span></span> <span data-ttu-id="2f2b3-146">Az első példában minden egyes tekintendő, amely az implicit tranzakciókban.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-146">In the first example, each individual call is an implicit transaction.</span></span> <span data-ttu-id="2f2b3-147">A második példában az explicit tranzakciók becsomagolja a hívások mindegyikét.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-147">In the second example, an explicit transaction wraps all of the calls.</span></span> <span data-ttu-id="2f2b3-148">/ Dokumentációját a [írási előre tranzakciónapló](https://msdn.microsoft.com/library/ms186259.aspx), naplórekordokat kiürített a lemezre, ha a tranzakció véglegesítése.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-148">Per the documentation for the [write-ahead transaction log](https://msdn.microsoft.com/library/ms186259.aspx), log records are flushed to the disk when the transaction commits.</span></span> <span data-ttu-id="2f2b3-149">Így több hívást együtt egy tranzakcióban, az írás a tranzakciós napló tudja elhalasztani, amíg a tranzakció.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-149">So by including more calls in a transaction, the write to the transaction log can delay until the transaction is committed.</span></span> <span data-ttu-id="2f2b3-150">Érvényben engedélyezi az írási műveleteket ad ki a kiszolgáló tranzakciónapló a kötegelés.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-150">In effect, you are enabling batching for the writes to the server’s transaction log.</span></span>

<span data-ttu-id="2f2b3-151">Az alábbi táblázat néhány alkalmi vizsgálati eredményeket jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-151">The following table shows some ad-hoc testing results.</span></span> <span data-ttu-id="2f2b3-152">A következő tesztek kerülnek végrehajtásra, és anélkül tranzakciók azonos szekvenciális beszúrása.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-152">The tests performed the same sequential inserts with and without transactions.</span></span> <span data-ttu-id="2f2b3-153">Több szempont az első készletét tesztek futott távolról a hordozható az adatbázis a Microsoft Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-153">For more perspective, the first set of tests ran remotely from a laptop to the database in Microsoft Azure.</span></span> <span data-ttu-id="2f2b3-154">A második készlet tesztet hajt végre egy felhőalapú szolgáltatás, hogy mindkét tartózkodott belül az azonos Microsoft Azure datacenter (USA nyugati régiója) adatbázis futott.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-154">The second set of tests ran from a cloud service and database that both resided within the same Microsoft Azure datacenter (West US).</span></span> <span data-ttu-id="2f2b3-155">A következő táblázat az időtartam a szekvenciális Beszúrások rendelkező és anélküli tranzakciók ezredmásodpercben.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-155">The following table shows the duration in milliseconds of sequential inserts with and without transactions.</span></span>

<span data-ttu-id="2f2b3-156">**Az Azure-bA helyszíni**:</span><span class="sxs-lookup"><span data-stu-id="2f2b3-156">**On-Premises to Azure**:</span></span>

| <span data-ttu-id="2f2b3-157">Műveletek</span><span class="sxs-lookup"><span data-stu-id="2f2b3-157">Operations</span></span> | <span data-ttu-id="2f2b3-158">Nincs tranzakció (ms)</span><span class="sxs-lookup"><span data-stu-id="2f2b3-158">No Transaction (ms)</span></span> | <span data-ttu-id="2f2b3-159">Tranzakció (ms)</span><span class="sxs-lookup"><span data-stu-id="2f2b3-159">Transaction (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2f2b3-160">1</span><span class="sxs-lookup"><span data-stu-id="2f2b3-160">1</span></span> |<span data-ttu-id="2f2b3-161">130</span><span class="sxs-lookup"><span data-stu-id="2f2b3-161">130</span></span> |<span data-ttu-id="2f2b3-162">402</span><span class="sxs-lookup"><span data-stu-id="2f2b3-162">402</span></span> |
| <span data-ttu-id="2f2b3-163">10</span><span class="sxs-lookup"><span data-stu-id="2f2b3-163">10</span></span> |<span data-ttu-id="2f2b3-164">1208</span><span class="sxs-lookup"><span data-stu-id="2f2b3-164">1208</span></span> |<span data-ttu-id="2f2b3-165">1226</span><span class="sxs-lookup"><span data-stu-id="2f2b3-165">1226</span></span> |
| <span data-ttu-id="2f2b3-166">100</span><span class="sxs-lookup"><span data-stu-id="2f2b3-166">100</span></span> |<span data-ttu-id="2f2b3-167">12662</span><span class="sxs-lookup"><span data-stu-id="2f2b3-167">12662</span></span> |<span data-ttu-id="2f2b3-168">10395</span><span class="sxs-lookup"><span data-stu-id="2f2b3-168">10395</span></span> |
| <span data-ttu-id="2f2b3-169">1000</span><span class="sxs-lookup"><span data-stu-id="2f2b3-169">1000</span></span> |<span data-ttu-id="2f2b3-170">128852</span><span class="sxs-lookup"><span data-stu-id="2f2b3-170">128852</span></span> |<span data-ttu-id="2f2b3-171">102917</span><span class="sxs-lookup"><span data-stu-id="2f2b3-171">102917</span></span> |

<span data-ttu-id="2f2b3-172">**Azure-az Azure-ba (ugyanabban az adatközpontban)**:</span><span class="sxs-lookup"><span data-stu-id="2f2b3-172">**Azure to Azure (same datacenter)**:</span></span>

| <span data-ttu-id="2f2b3-173">Műveletek</span><span class="sxs-lookup"><span data-stu-id="2f2b3-173">Operations</span></span> | <span data-ttu-id="2f2b3-174">Nincs tranzakció (ms)</span><span class="sxs-lookup"><span data-stu-id="2f2b3-174">No Transaction (ms)</span></span> | <span data-ttu-id="2f2b3-175">Tranzakció (ms)</span><span class="sxs-lookup"><span data-stu-id="2f2b3-175">Transaction (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2f2b3-176">1</span><span class="sxs-lookup"><span data-stu-id="2f2b3-176">1</span></span> |<span data-ttu-id="2f2b3-177">21</span><span class="sxs-lookup"><span data-stu-id="2f2b3-177">21</span></span> |<span data-ttu-id="2f2b3-178">26</span><span class="sxs-lookup"><span data-stu-id="2f2b3-178">26</span></span> |
| <span data-ttu-id="2f2b3-179">10</span><span class="sxs-lookup"><span data-stu-id="2f2b3-179">10</span></span> |<span data-ttu-id="2f2b3-180">220</span><span class="sxs-lookup"><span data-stu-id="2f2b3-180">220</span></span> |<span data-ttu-id="2f2b3-181">56</span><span class="sxs-lookup"><span data-stu-id="2f2b3-181">56</span></span> |
| <span data-ttu-id="2f2b3-182">100</span><span class="sxs-lookup"><span data-stu-id="2f2b3-182">100</span></span> |<span data-ttu-id="2f2b3-183">2145</span><span class="sxs-lookup"><span data-stu-id="2f2b3-183">2145</span></span> |<span data-ttu-id="2f2b3-184">341</span><span class="sxs-lookup"><span data-stu-id="2f2b3-184">341</span></span> |
| <span data-ttu-id="2f2b3-185">1000</span><span class="sxs-lookup"><span data-stu-id="2f2b3-185">1000</span></span> |<span data-ttu-id="2f2b3-186">21479</span><span class="sxs-lookup"><span data-stu-id="2f2b3-186">21479</span></span> |<span data-ttu-id="2f2b3-187">2756</span><span class="sxs-lookup"><span data-stu-id="2f2b3-187">2756</span></span> |

> [!NOTE]
> <span data-ttu-id="2f2b3-188">A eredményei nem referenciaalapokhoz képest.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-188">Results are not benchmarks.</span></span> <span data-ttu-id="2f2b3-189">Tekintse meg a [időzítési eredményezi, hogy ez a témakör Megjegyzés](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="2f2b3-189">See the [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="2f2b3-190">Az előző teszt eredményei alapján teljesítmény ténylegesen csökkenti alkalmazásburkoló egyetlen műveletben szerepel egy tranzakcióban.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-190">Based on the previous test results, wrapping a single operation in a transaction actually decreases performance.</span></span> <span data-ttu-id="2f2b3-191">De növelésével egy tranzakción belül műveletek számát, a teljesítmény fokozása több lesz megjelölve.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-191">But as you increase the number of operations within a single transaction, the performance improvement becomes more marked.</span></span> <span data-ttu-id="2f2b3-192">A teljesítménybeli különbség az akkor is jobban észlelhető, ha minden műveletnél fordulhat elő, a Microsoft Azure adatközponton belül.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-192">The performance difference is also more noticeable when all operations occur within the Microsoft Azure datacenter.</span></span> <span data-ttu-id="2f2b3-193">Az SQL-adatbázisát használja a Microsoft Azure adatközponton kívülről nagyobb késéseket overshadows tranzakciók használatával jobb a teljesítménye.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-193">The increased latency of using SQL Database from outside the Microsoft Azure datacenter overshadows the performance gain of using transactions.</span></span>

<span data-ttu-id="2f2b3-194">Bár a tranzakciók használata növelheti a teljesítményt, továbbra is [tekintse át az ajánlott eljárások az tranzakciók és kapcsolatok](https://msdn.microsoft.com/library/ms187484.aspx).</span><span class="sxs-lookup"><span data-stu-id="2f2b3-194">Although the use of transactions can increase performance, continue to [observe best practices for transactions and connections](https://msdn.microsoft.com/library/ms187484.aspx).</span></span> <span data-ttu-id="2f2b3-195">Tartsa a lehető legrövidebb tranzakció, és a munka végeztével, zárja be az adatbázis-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-195">Keep the transaction as short as possible, and close the database connection after the work completes.</span></span> <span data-ttu-id="2f2b3-196">Az utasítás használatával az előző példában szereplő biztosítja, hogy a kapcsolat megszakad, a következő kódblokk befejezéséről.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-196">The using statement in the previous example assures that the connection is closed when the subsequent code block completes.</span></span>

<span data-ttu-id="2f2b3-197">A korábbi példa bemutatja, hogy adhat hozzá egy helyi tranzakció ADO.NET kódok esetén is tenné két sort.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-197">The previous example demonstrates that you can add a local transaction to any ADO.NET code with two lines.</span></span> <span data-ttu-id="2f2b3-198">Tranzakciók ajánlatot gyorsan kódot, amely lehetővé teszi a szekvenciális beszúrási, frissítési és törlési műveletek teljesítményének növelésében.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-198">Transactions offer a quick way to improve the performance of code that makes sequential insert, update, and delete operations.</span></span> <span data-ttu-id="2f2b3-199">Azonban a leggyorsabb teljesítmény érdekében fontolja meg a kódot használja ki az ügyféloldali kötegelés, például a táblázat értékű paramétereket tovább.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-199">However, for the fastest performance, consider changing the code further to take advantage of client-side batching, such as table-valued parameters.</span></span>

<span data-ttu-id="2f2b3-200">Az ADO.NET tranzakciókkal kapcsolatos további információkért lásd: [ADO.NET helyi tranzakciók](https://docs.microsoft.com/dotnet/framework/data/adonet/local-transactions).</span><span class="sxs-lookup"><span data-stu-id="2f2b3-200">For more information about transactions in ADO.NET, see [Local Transactions in ADO.NET](https://docs.microsoft.com/dotnet/framework/data/adonet/local-transactions).</span></span>

### <a name="table-valued-parameters"></a><span data-ttu-id="2f2b3-201">tábla értékű paraméter</span><span class="sxs-lookup"><span data-stu-id="2f2b3-201">Table-valued parameters</span></span>
<span data-ttu-id="2f2b3-202">Tábla értékű paraméter paraméterekkel a Transact-SQL-utasítások, tárolt eljárások és függvények, felhasználó által definiált táblatípusokban támogatja.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-202">Table-valued parameters support user-defined table types as parameters in Transact-SQL statements, stored procedures, and functions.</span></span> <span data-ttu-id="2f2b3-203">Ez az ügyféloldali kötegelési módszer lehetővé teszi, hogy több sornyi adatot belül a tábla értékű paraméter küldhet.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-203">This client-side batching technique allows you to send multiple rows of data within the table-valued parameter.</span></span> <span data-ttu-id="2f2b3-204">Tábla értékű paraméter használatához először meg kell határoznia egy táblatípus.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-204">To use table-valued parameters, first define a table type.</span></span> <span data-ttu-id="2f2b3-205">A következő Transact-SQL-utasítást hoz létre nevű tábla típus **MyTableType**.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-205">The following Transact-SQL statement creates a table type named **MyTableType**.</span></span>

    CREATE TYPE MyTableType AS TABLE 
    ( mytext TEXT,
      num INT );


<span data-ttu-id="2f2b3-206">A kódban, hozzon létre egy **DataTable** pontos azonos nevét és a táblatípus típusú.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-206">In code, you create a **DataTable** with the exact same names and types of the table type.</span></span> <span data-ttu-id="2f2b3-207">Ez átadni **DataTable** egy paraméterben, szöveges lekérdezés vagy tárolt eljárás hívása.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-207">Pass this **DataTable** in a parameter in a text query or stored procedure call.</span></span> <span data-ttu-id="2f2b3-208">A következő példa bemutatja, ezzel a módszerrel:</span><span class="sxs-lookup"><span data-stu-id="2f2b3-208">The following example shows this technique:</span></span>

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();

        DataTable table = new DataTable();
        // Add columns and rows. The following is a simple example.
        table.Columns.Add("mytext", typeof(string));
        table.Columns.Add("num", typeof(int));    
        for (var i = 0; i < 10; i++)
        {
            table.Rows.Add(DateTime.Now.ToString(), DateTime.Now.Millisecond);
        }

        SqlCommand cmd = new SqlCommand(
            "INSERT INTO MyTable(mytext, num) SELECT mytext, num FROM @TestTvp",
            connection);

        cmd.Parameters.Add(
            new SqlParameter()
            {
                ParameterName = "@TestTvp",
                SqlDbType = SqlDbType.Structured,
                TypeName = "MyTableType",
                Value = table,
            });

        cmd.ExecuteNonQuery();
    }

<span data-ttu-id="2f2b3-209">Az előző példában a **SqlCommand** objektum egy tábla értékű paraméter a sor beszúrása  **@TestTvp** .</span><span class="sxs-lookup"><span data-stu-id="2f2b3-209">In the previous example, the **SqlCommand** object inserts rows from a table-valued parameter, **@TestTvp**.</span></span> <span data-ttu-id="2f2b3-210">A korábban létrehozott **DataTable** objektum ezt a paramétert hozzá van rendelve a **SqlCommand.Parameters.Add** metódust.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-210">The previously created **DataTable** object is assigned to this parameter with the **SqlCommand.Parameters.Add** method.</span></span> <span data-ttu-id="2f2b3-211">A teljesítmény egy hívásban Beszúrások kötegelés jelentősen növeli a szekvenciális Beszúrások keresztül.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-211">Batching the inserts in one call significantly increases the performance over sequential inserts.</span></span>

<span data-ttu-id="2f2b3-212">Az előző példa folytatásaként javításához használja a tárolt eljárás egy szöveges parancs helyett.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-212">To improve the previous example further, use a stored procedure instead of a text-based command.</span></span> <span data-ttu-id="2f2b3-213">A következő Transact-SQL-parancs létrehoz egy tárolt eljárás, amely a **SimpleTestTableType** tábla értékű paraméter.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-213">The following Transact-SQL command creates a stored procedure that takes the **SimpleTestTableType** table-valued parameter.</span></span>

    CREATE PROCEDURE [dbo].[sp_InsertRows] 
    @TestTvp as MyTableType READONLY
    AS
    BEGIN
    INSERT INTO MyTable(mytext, num) 
    SELECT mytext, num FROM @TestTvp
    END
    GO

<span data-ttu-id="2f2b3-214">Módosítsa a **SqlCommand** objektum az előző példakódban csatlakoztatása a következő nyilatkozatot.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-214">Then change the **SqlCommand** object declaration in the previous code example to the following.</span></span>

    SqlCommand cmd = new SqlCommand("sp_InsertRows", connection);
    cmd.CommandType = CommandType.StoredProcedure;

<span data-ttu-id="2f2b3-215">A legtöbb esetben a tábla értékű paraméter rendelkezik egyenértékű vagy jobb teljesítményt biztosít, mint más kötegelési módszerek.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-215">In most cases, table-valued parameters have equivalent or better performance than other batching techniques.</span></span> <span data-ttu-id="2f2b3-216">Tábla értékű paraméterek gyakran érdemes, mivel rugalmasabb, mint más beállítások.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-216">Table-valued parameters are often preferable, because they are more flexible than other options.</span></span> <span data-ttu-id="2f2b3-217">Egyéb módszerek, például SQL tömeges másolás, például csak új sorok beszúrását lehetővé teszik.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-217">For example, other techniques, such as SQL bulk copy, only permit the insertion of new rows.</span></span> <span data-ttu-id="2f2b3-218">De tábla értékű paraméter segítségével programot a tárolt eljárás annak meghatározására, hogy mely sorai frissítések, és amelyek beszúrása.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-218">But with table-valued parameters, you can use logic in the stored procedure to determine which rows are updates and which are inserts.</span></span> <span data-ttu-id="2f2b3-219">A táblatípus is módosíthatja a tartalmaz egy "Művelet" oszlopot, amely jelzi, hogy a megadott sor kell beszúrni, frissíteni, vagy törölve.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-219">The table type can also be modified to contain an “Operation” column that indicates whether the specified row should be inserted, updated, or deleted.</span></span>

<span data-ttu-id="2f2b3-220">A következő táblázat a tábla értékű paraméterek használatával vizsgálati eredményeket ad hoc ezredmásodpercben.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-220">The following table shows ad-hoc test results for the use of table-valued parameters in milliseconds.</span></span>

| <span data-ttu-id="2f2b3-221">Műveletek</span><span class="sxs-lookup"><span data-stu-id="2f2b3-221">Operations</span></span> | <span data-ttu-id="2f2b3-222">A helyszíni Azure-ba (ms)</span><span class="sxs-lookup"><span data-stu-id="2f2b3-222">On-Premises to Azure (ms)</span></span> | <span data-ttu-id="2f2b3-223">Az Azure ugyanabban az adatközpontban (ms)</span><span class="sxs-lookup"><span data-stu-id="2f2b3-223">Azure same datacenter (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2f2b3-224">1</span><span class="sxs-lookup"><span data-stu-id="2f2b3-224">1</span></span> |<span data-ttu-id="2f2b3-225">124</span><span class="sxs-lookup"><span data-stu-id="2f2b3-225">124</span></span> |<span data-ttu-id="2f2b3-226">32</span><span class="sxs-lookup"><span data-stu-id="2f2b3-226">32</span></span> |
| <span data-ttu-id="2f2b3-227">10</span><span class="sxs-lookup"><span data-stu-id="2f2b3-227">10</span></span> |<span data-ttu-id="2f2b3-228">131</span><span class="sxs-lookup"><span data-stu-id="2f2b3-228">131</span></span> |<span data-ttu-id="2f2b3-229">25</span><span class="sxs-lookup"><span data-stu-id="2f2b3-229">25</span></span> |
| <span data-ttu-id="2f2b3-230">100</span><span class="sxs-lookup"><span data-stu-id="2f2b3-230">100</span></span> |<span data-ttu-id="2f2b3-231">338</span><span class="sxs-lookup"><span data-stu-id="2f2b3-231">338</span></span> |<span data-ttu-id="2f2b3-232">51</span><span class="sxs-lookup"><span data-stu-id="2f2b3-232">51</span></span> |
| <span data-ttu-id="2f2b3-233">1000</span><span class="sxs-lookup"><span data-stu-id="2f2b3-233">1000</span></span> |<span data-ttu-id="2f2b3-234">2615</span><span class="sxs-lookup"><span data-stu-id="2f2b3-234">2615</span></span> |<span data-ttu-id="2f2b3-235">382</span><span class="sxs-lookup"><span data-stu-id="2f2b3-235">382</span></span> |
| <span data-ttu-id="2f2b3-236">10000</span><span class="sxs-lookup"><span data-stu-id="2f2b3-236">10000</span></span> |<span data-ttu-id="2f2b3-237">23830</span><span class="sxs-lookup"><span data-stu-id="2f2b3-237">23830</span></span> |<span data-ttu-id="2f2b3-238">3586</span><span class="sxs-lookup"><span data-stu-id="2f2b3-238">3586</span></span> |

> [!NOTE]
> <span data-ttu-id="2f2b3-239">A eredményei nem referenciaalapokhoz képest.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-239">Results are not benchmarks.</span></span> <span data-ttu-id="2f2b3-240">Tekintse meg a [időzítési eredményezi, hogy ez a témakör Megjegyzés](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="2f2b3-240">See the [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="2f2b3-241">A kötegelés jobb a teljesítménye azonnal kétségtelenül.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-241">The performance gain from batching is immediately apparent.</span></span> <span data-ttu-id="2f2b3-242">Az előző szekvenciális teszt 1000 műveletek 129 másodperc az adatközponton kívülről és az adatközponton belül a 21 másodpercet vett igénybe.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-242">In the previous sequential test, 1000 operations took 129 seconds outside the datacenter and 21 seconds from within the datacenter.</span></span> <span data-ttu-id="2f2b3-243">De tábla értékű paraméter 1000 műveletek másodpercre csak 2.6-os és az adatközponton belül idõtartamtól az adatközponton kívülről.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-243">But with table-valued parameters, 1000 operations take only 2.6 seconds outside the datacenter and 0.4 seconds within the datacenter.</span></span>

<span data-ttu-id="2f2b3-244">További információ a tábla értékű paraméter: [Table-Valued paraméterek](https://msdn.microsoft.com/library/bb510489.aspx).</span><span class="sxs-lookup"><span data-stu-id="2f2b3-244">For more information on table-valued parameters, see [Table-Valued Parameters](https://msdn.microsoft.com/library/bb510489.aspx).</span></span>

### <a name="sql-bulk-copy"></a><span data-ttu-id="2f2b3-245">SQL tömeges másolási</span><span class="sxs-lookup"><span data-stu-id="2f2b3-245">SQL bulk copy</span></span>
<span data-ttu-id="2f2b3-246">SQL tömeges másolási egy másik módja a nagy mennyiségű adat elhelyezni a céladatbázis.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-246">SQL bulk copy is another way to insert large amounts of data into a target database.</span></span> <span data-ttu-id="2f2b3-247">.NET-alkalmazások használhatják a **SqlBulkCopy** osztály végrehajtására tömeges beszúrási műveletek.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-247">.NET applications can use the **SqlBulkCopy** class to perform bulk insert operations.</span></span> <span data-ttu-id="2f2b3-248">**SqlBulkCopy** a parancssori eszköz, a függvény hasonló **Bcp.exe**, vagy a Transact-SQL-utasítás **TÖMEGES Beszúrás**.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-248">**SqlBulkCopy** is similar in function to the command-line tool, **Bcp.exe**, or the Transact-SQL statement, **BULK INSERT**.</span></span> <span data-ttu-id="2f2b3-249">Az alábbi példakód bemutatja, hogyan tömegesen másolni a sorokat a forráshelyen **DataTable**, táblázatra, az SQL Server, a céltábla táblanév.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-249">The following code example shows how to bulk copy the rows in the source **DataTable**, table, to the destination table in SQL Server, MyTable.</span></span>

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();

        using (SqlBulkCopy bulkCopy = new SqlBulkCopy(connection))
        {
            bulkCopy.DestinationTableName = "MyTable";
            bulkCopy.ColumnMappings.Add("mytext", "mytext");
            bulkCopy.ColumnMappings.Add("num", "num");
            bulkCopy.WriteToServer(table);
        }
    }

<span data-ttu-id="2f2b3-250">Néhány esetben, ha tömeges másolás előnyben részesített tábla értékű paraméter felett van.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-250">There are some cases where bulk copy is preferred over table-valued parameters.</span></span> <span data-ttu-id="2f2b3-251">Tekintse meg a tábla értékű paraméter és a következő témakör TÖMEGES beszúrási műveletek összehasonlító táblázatot [Table-Valued paraméterek](https://msdn.microsoft.com/library/bb510489.aspx).</span><span class="sxs-lookup"><span data-stu-id="2f2b3-251">See the comparison table of Table-Valued parameters versus BULK INSERT operations in the topic [Table-Valued Parameters](https://msdn.microsoft.com/library/bb510489.aspx).</span></span>

<span data-ttu-id="2f2b3-252">A következő alkalmi vizsgálati eredmények megjelenítése a kötegelés teljesítményének **SqlBulkCopy** ezredmásodpercben.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-252">The following ad-hoc test results show the performance of batching with **SqlBulkCopy** in milliseconds.</span></span>

| <span data-ttu-id="2f2b3-253">Műveletek</span><span class="sxs-lookup"><span data-stu-id="2f2b3-253">Operations</span></span> | <span data-ttu-id="2f2b3-254">A helyszíni Azure-ba (ms)</span><span class="sxs-lookup"><span data-stu-id="2f2b3-254">On-Premises to Azure (ms)</span></span> | <span data-ttu-id="2f2b3-255">Az Azure ugyanabban az adatközpontban (ms)</span><span class="sxs-lookup"><span data-stu-id="2f2b3-255">Azure same datacenter (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2f2b3-256">1</span><span class="sxs-lookup"><span data-stu-id="2f2b3-256">1</span></span> |<span data-ttu-id="2f2b3-257">433</span><span class="sxs-lookup"><span data-stu-id="2f2b3-257">433</span></span> |<span data-ttu-id="2f2b3-258">57</span><span class="sxs-lookup"><span data-stu-id="2f2b3-258">57</span></span> |
| <span data-ttu-id="2f2b3-259">10</span><span class="sxs-lookup"><span data-stu-id="2f2b3-259">10</span></span> |<span data-ttu-id="2f2b3-260">441</span><span class="sxs-lookup"><span data-stu-id="2f2b3-260">441</span></span> |<span data-ttu-id="2f2b3-261">32</span><span class="sxs-lookup"><span data-stu-id="2f2b3-261">32</span></span> |
| <span data-ttu-id="2f2b3-262">100</span><span class="sxs-lookup"><span data-stu-id="2f2b3-262">100</span></span> |<span data-ttu-id="2f2b3-263">636</span><span class="sxs-lookup"><span data-stu-id="2f2b3-263">636</span></span> |<span data-ttu-id="2f2b3-264">53</span><span class="sxs-lookup"><span data-stu-id="2f2b3-264">53</span></span> |
| <span data-ttu-id="2f2b3-265">1000</span><span class="sxs-lookup"><span data-stu-id="2f2b3-265">1000</span></span> |<span data-ttu-id="2f2b3-266">2535</span><span class="sxs-lookup"><span data-stu-id="2f2b3-266">2535</span></span> |<span data-ttu-id="2f2b3-267">341</span><span class="sxs-lookup"><span data-stu-id="2f2b3-267">341</span></span> |
| <span data-ttu-id="2f2b3-268">10000</span><span class="sxs-lookup"><span data-stu-id="2f2b3-268">10000</span></span> |<span data-ttu-id="2f2b3-269">21605</span><span class="sxs-lookup"><span data-stu-id="2f2b3-269">21605</span></span> |<span data-ttu-id="2f2b3-270">2737</span><span class="sxs-lookup"><span data-stu-id="2f2b3-270">2737</span></span> |

> [!NOTE]
> <span data-ttu-id="2f2b3-271">A eredményei nem referenciaalapokhoz képest.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-271">Results are not benchmarks.</span></span> <span data-ttu-id="2f2b3-272">Tekintse meg a [időzítési eredményezi, hogy ez a témakör Megjegyzés](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="2f2b3-272">See the [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="2f2b3-273">A Köteg mérete kisebb, használja a tábla értékű paraméterek outperformed a **SqlBulkCopy** osztály.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-273">In smaller batch sizes, the use table-valued parameters outperformed the **SqlBulkCopy** class.</span></span> <span data-ttu-id="2f2b3-274">Azonban **SqlBulkCopy** gyorsabb, mint a tábla értékű paraméterek a 12-31 % elvégzi a tesztek 1000 és 10 000 sorok.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-274">However, **SqlBulkCopy** performed 12-31% faster than table-valued parameters for the tests of 1,000 and 10,000 rows.</span></span> <span data-ttu-id="2f2b3-275">Tábla értékű paraméterek, például **SqlBulkCopy** van a kötegelt Beszúrás jó választás, különösen akkor, ha nem kötegelni műveletek teljesítményének képest.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-275">Like table-valued parameters, **SqlBulkCopy** is a good option for batched inserts, especially when compared to the performance of non-batched operations.</span></span>

<span data-ttu-id="2f2b3-276">A tömeges másolás az ADO.NET további információkért lásd: [az SQL Server tömeges másolási műveletek](https://msdn.microsoft.com/library/7ek5da1a.aspx).</span><span class="sxs-lookup"><span data-stu-id="2f2b3-276">For more information on bulk copy in ADO.NET, see [Bulk Copy Operations in SQL Server](https://msdn.microsoft.com/library/7ek5da1a.aspx).</span></span>

### <a name="multiple-row-parameterized-insert-statements"></a><span data-ttu-id="2f2b3-277">Több soron kívüli Beszúrás paraméteres utasításokat</span><span class="sxs-lookup"><span data-stu-id="2f2b3-277">Multiple-row Parameterized INSERT statements</span></span>
<span data-ttu-id="2f2b3-278">Egy kis kötegek esetben nagy paraméteres utasítást, amely több sor beszúrása összeállításához.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-278">One alternative for small batches is to construct a large parameterized INSERT statement that inserts multiple rows.</span></span> <span data-ttu-id="2f2b3-279">Az alábbi példakód mutatja be, ezzel a módszerrel.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-279">The following code example demonstrates this technique.</span></span>

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();

        string insertCommand = "INSERT INTO [MyTable] ( mytext, num ) " +
            "VALUES (@p1, @p2), (@p3, @p4), (@p5, @p6), (@p7, @p8), (@p9, @p10)";

        SqlCommand cmd = new SqlCommand(insertCommand, connection);

        for (int i = 1; i <= 10; i += 2)
        {
            cmd.Parameters.Add(new SqlParameter("@p" + i.ToString(), "test"));
            cmd.Parameters.Add(new SqlParameter("@p" + (i+1).ToString(), i));
        }

        cmd.ExecuteNonQuery();
    }


<span data-ttu-id="2f2b3-280">Ez a példa arra szolgál, hogy az alapvető fogalma megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-280">This example is meant to show the basic concept.</span></span> <span data-ttu-id="2f2b3-281">A modell forgatókönyv volna ismétlése a lekérdezési karakterláncot és a parancs paraméterei egyidejűleg összeállításához szükséges entitásokat.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-281">A more realistic scenario would loop through the required entities to construct the query string and the command parameters simultaneously.</span></span> <span data-ttu-id="2f2b3-282">Azonban legfeljebb összesen 2100 lekérdezési paraméterek, ez korlátozza az ilyen módon feldolgozható sorok száma.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-282">You are limited to a total of 2100 query parameters, so this limits the total number of rows that can be processed in this manner.</span></span>

<span data-ttu-id="2f2b3-283">A következő alkalmi teszteredmények utasítást ilyen típusú teljesítményének megjelenítése ezredmásodpercben.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-283">The following ad-hoc test results show the performance of this type of insert statement in milliseconds.</span></span>

| <span data-ttu-id="2f2b3-284">Műveletek</span><span class="sxs-lookup"><span data-stu-id="2f2b3-284">Operations</span></span> | <span data-ttu-id="2f2b3-285">Tábla értékű paraméter (ms)</span><span class="sxs-lookup"><span data-stu-id="2f2b3-285">Table-valued parameters (ms)</span></span> | <span data-ttu-id="2f2b3-286">Utasításból INSERT (ms)</span><span class="sxs-lookup"><span data-stu-id="2f2b3-286">Single-statement INSERT (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2f2b3-287">1</span><span class="sxs-lookup"><span data-stu-id="2f2b3-287">1</span></span> |<span data-ttu-id="2f2b3-288">32</span><span class="sxs-lookup"><span data-stu-id="2f2b3-288">32</span></span> |<span data-ttu-id="2f2b3-289">20</span><span class="sxs-lookup"><span data-stu-id="2f2b3-289">20</span></span> |
| <span data-ttu-id="2f2b3-290">10</span><span class="sxs-lookup"><span data-stu-id="2f2b3-290">10</span></span> |<span data-ttu-id="2f2b3-291">30</span><span class="sxs-lookup"><span data-stu-id="2f2b3-291">30</span></span> |<span data-ttu-id="2f2b3-292">25</span><span class="sxs-lookup"><span data-stu-id="2f2b3-292">25</span></span> |
| <span data-ttu-id="2f2b3-293">100</span><span class="sxs-lookup"><span data-stu-id="2f2b3-293">100</span></span> |<span data-ttu-id="2f2b3-294">33</span><span class="sxs-lookup"><span data-stu-id="2f2b3-294">33</span></span> |<span data-ttu-id="2f2b3-295">51</span><span class="sxs-lookup"><span data-stu-id="2f2b3-295">51</span></span> |

> [!NOTE]
> <span data-ttu-id="2f2b3-296">A eredményei nem referenciaalapokhoz képest.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-296">Results are not benchmarks.</span></span> <span data-ttu-id="2f2b3-297">Tekintse meg a [időzítési eredményezi, hogy ez a témakör Megjegyzés](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="2f2b3-297">See the [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="2f2b3-298">Ezt a módszert használja, amelyek 100-nál kevesebb sort kötegek némileg gyorsabb lehet.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-298">This approach can be slightly faster for batches that are less than 100 rows.</span></span> <span data-ttu-id="2f2b3-299">Bár a javítása kis, ez a módszer akkor egy másik lehetőség, amely előfordulhat, hogy kiválóan működjenek az adott alkalmazás helyzetnek.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-299">Although the improvement is small, this technique is another option that might work well in your specific application scenario.</span></span>

### <a name="dataadapter"></a><span data-ttu-id="2f2b3-300">DataAdapter</span><span class="sxs-lookup"><span data-stu-id="2f2b3-300">DataAdapter</span></span>
<span data-ttu-id="2f2b3-301">A **DataAdapter** osztály lehetővé teszi, hogy módosítsa egy **DataSet** objektumot, és küldje el az INSERT, UPDATE és DELETE műveletek változik.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-301">The **DataAdapter** class allows you to modify a **DataSet** object and then submit the changes as INSERT, UPDATE, and DELETE operations.</span></span> <span data-ttu-id="2f2b3-302">Ha használja a **DataAdapter** ezen a módon kikapcsolja, fontos megjegyezni, hogy külön hívások legyenek-e készülve az egyes különálló műveletet.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-302">If you are using the **DataAdapter** in this manner, it is important to note that separate calls are made for each distinct operation.</span></span> <span data-ttu-id="2f2b3-303">A teljesítmény javítása érdekében használja a **UpdateBatchSize** tulajdonságot, amely egyszerre kell lehet kötegelni műveletek száma.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-303">To improve performance, use the **UpdateBatchSize** property to the number of operations that should be batched at a time.</span></span> <span data-ttu-id="2f2b3-304">További információkért lásd: [végrehajtása kötegelt műveletek használatával DataAdapters](https://msdn.microsoft.com/library/aadf8fk2.aspx).</span><span class="sxs-lookup"><span data-stu-id="2f2b3-304">For more information, see [Performing Batch Operations Using DataAdapters](https://msdn.microsoft.com/library/aadf8fk2.aspx).</span></span>

### <a name="entity-framework"></a><span data-ttu-id="2f2b3-305">Entitás-keretrendszer</span><span class="sxs-lookup"><span data-stu-id="2f2b3-305">Entity framework</span></span>
<span data-ttu-id="2f2b3-306">Entitás-keretrendszer jelenleg nem támogatja kötegelés.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-306">Entity Framework does not currently support batching.</span></span> <span data-ttu-id="2f2b3-307">A közösségi különböző fejlesztők próbált meg lehetséges megoldások, például a felülbírálás bemutatása a **a SaveChanges metódus** metódust.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-307">Different developers in the community have attempted to demonstrate workarounds, such as override the **SaveChanges** method.</span></span> <span data-ttu-id="2f2b3-308">De a megoldások általában összetett és testreszabott, az alkalmazás és az adatmodell.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-308">But the solutions are typically complex and customized to the application and data model.</span></span> <span data-ttu-id="2f2b3-309">Az Entity Framework codeplex-projekt jelenleg is rendelkezik az ismertető a szolgáltatás kérésre.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-309">The Entity Framework codeplex project currently has a discussion page on this feature request.</span></span> <span data-ttu-id="2f2b3-310">Az ismertető megtekintése: [tervezési értekezlet megjegyzések - 2012 augusztus 2](http://entityframework.codeplex.com/wikipage?title=Design%20Meeting%20Notes%20-%20August%202%2c%202012).</span><span class="sxs-lookup"><span data-stu-id="2f2b3-310">To view this discussion, see [Design Meeting Notes - August 2, 2012](http://entityframework.codeplex.com/wikipage?title=Design%20Meeting%20Notes%20-%20August%202%2c%202012).</span></span>

### <a name="xml"></a><span data-ttu-id="2f2b3-311">XML</span><span class="sxs-lookup"><span data-stu-id="2f2b3-311">XML</span></span>
<span data-ttu-id="2f2b3-312">A teljesség kedvéért azt látja, hogy fontos, mint egy kötegelési stratégia XML kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-312">For completeness, we feel that it is important to talk about XML as a batching strategy.</span></span> <span data-ttu-id="2f2b3-313">Az XML-kód használatát azonban más módszerekkel nem előnyöket és számos hátránya rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-313">However, the use of XML has no advantages over other methods and several disadvantages.</span></span> <span data-ttu-id="2f2b3-314">A megoldás, tábla értékű paraméter hasonló, de egy XML-fájl vagy karakterlánc objektumnak átadott helyett a felhasználó által definiált tábla tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-314">The approach is similar to table-valued parameters, but an XML file or string is passed to a stored procedure instead of a user-defined table.</span></span> <span data-ttu-id="2f2b3-315">A tárolt eljárás elemzi a parancsok a tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-315">The stored procedure parses the commands in the stored procedure.</span></span>

<span data-ttu-id="2f2b3-316">Ezt a megközelítést több hátrányai van:</span><span class="sxs-lookup"><span data-stu-id="2f2b3-316">There are several disadvantages to this approach:</span></span>

* <span data-ttu-id="2f2b3-317">Az XML működő nehézkes lehet, és hibalehetőségeket rejt magában hiba.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-317">Working with XML can be cumbersome and error prone.</span></span>
* <span data-ttu-id="2f2b3-318">Az adatbázis az XML-elemzés processzorigényes is lehet.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-318">Parsing the XML on the database can be CPU-intensive.</span></span>
* <span data-ttu-id="2f2b3-319">A legtöbb esetben ez a módszer lassabb, mint a tábla értékű paraméter.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-319">In most cases, this method is slower than table-valued parameters.</span></span>

<span data-ttu-id="2f2b3-320">Ezen okok miatt a XML kötegelt lekérdezések használata nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-320">For these reasons, the use of XML for batch queries is not recommended.</span></span>

## <a name="batching-considerations"></a><span data-ttu-id="2f2b3-321">Kötegelési kapcsolatos szempontok</span><span class="sxs-lookup"><span data-stu-id="2f2b3-321">Batching considerations</span></span>
<span data-ttu-id="2f2b3-322">Az alábbi szakaszokban további útmutatás nyújtása a kötegelés SQL-adatbázis alkalmazások használatát.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-322">The following sections provide more guidance for the use of batching in SQL Database applications.</span></span>

### <a name="tradeoffs"></a><span data-ttu-id="2f2b3-323">Mellékhatásokkal</span><span class="sxs-lookup"><span data-stu-id="2f2b3-323">Tradeoffs</span></span>
<span data-ttu-id="2f2b3-324">Attól függően, hogy az architektúrák kötegelés magába foglaló a teljesítményt és rugalmasságot közötti kompromisszumot.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-324">Depending on your architecture, batching can involve a tradeoff between performance and resiliency.</span></span> <span data-ttu-id="2f2b3-325">Vegyük példaként a forgatókönyvet, ahol a szerepkör váratlanul leáll.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-325">For example, consider the scenario where your role unexpectedly goes down.</span></span> <span data-ttu-id="2f2b3-326">Ha elveszíti egy adatsornak, szempontjából kisebb, mint egy nagy sorköteg el nem küldött elvesztése hatását.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-326">If you lose one row of data, the impact is smaller than the impact of losing a large batch of unsubmitted rows.</span></span> <span data-ttu-id="2f2b3-327">Nagyobb veszélynek van Ha sorok elegendő pufferrel, mielőtt elküldi őket az adatbázishoz megadott időkeretnél.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-327">There is a greater risk when you buffer rows before sending them to the database in a specified time window.</span></span>

<span data-ttu-id="2f2b3-328">Miatt ez kompromisszumot kiértékelheti, hogy Ön kötegelt működés.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-328">Because of this tradeoff, evaluate the type of operations that you batch.</span></span> <span data-ttu-id="2f2b3-329">A Batch-agresszívabb (nagyobb kötegek és hosszabb idő windows) kevésbé fontos adatokkal.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-329">Batch more aggressively (larger batches and longer time windows) with data that is less critical.</span></span>

### <a name="batch-size"></a><span data-ttu-id="2f2b3-330">Köteg mérete</span><span class="sxs-lookup"><span data-stu-id="2f2b3-330">Batch size</span></span>
<span data-ttu-id="2f2b3-331">A tesztelés során történt általában nagy kötegek ossza kisebb csoportjai való nem szolgál előnyökkel.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-331">In our tests, there was typically no advantage to breaking large batches into smaller chunks.</span></span> <span data-ttu-id="2f2b3-332">Gyakran ez felosztása, mint egy egyetlen nagy kötegelt lassabban eredményezett.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-332">In fact, this subdivision often resulted in slower performance than submitting a single large batch.</span></span> <span data-ttu-id="2f2b3-333">Vegyük példaként egy olyan forgatókönyvet, ahol szeretné 1000 sor beszúrása.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-333">For example, consider a scenario where you want to insert 1000 rows.</span></span> <span data-ttu-id="2f2b3-334">Az alábbi táblázatban látható, mennyi ideig tart a tábla értékű paraméter használatával 1000 sor, amikor kisebb kötegekben osztva.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-334">The following table shows how long it takes to use table-valued parameters to insert 1000 rows when divided into smaller batches.</span></span>

| <span data-ttu-id="2f2b3-335">Köteg mérete</span><span class="sxs-lookup"><span data-stu-id="2f2b3-335">Batch size</span></span> | <span data-ttu-id="2f2b3-336">Az ismétlés</span><span class="sxs-lookup"><span data-stu-id="2f2b3-336">Iterations</span></span> | <span data-ttu-id="2f2b3-337">Tábla értékű paraméter (ms)</span><span class="sxs-lookup"><span data-stu-id="2f2b3-337">Table-valued parameters (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="2f2b3-338">1000</span><span class="sxs-lookup"><span data-stu-id="2f2b3-338">1000</span></span> |<span data-ttu-id="2f2b3-339">1</span><span class="sxs-lookup"><span data-stu-id="2f2b3-339">1</span></span> |<span data-ttu-id="2f2b3-340">347</span><span class="sxs-lookup"><span data-stu-id="2f2b3-340">347</span></span> |
| <span data-ttu-id="2f2b3-341">500</span><span class="sxs-lookup"><span data-stu-id="2f2b3-341">500</span></span> |<span data-ttu-id="2f2b3-342">2</span><span class="sxs-lookup"><span data-stu-id="2f2b3-342">2</span></span> |<span data-ttu-id="2f2b3-343">355</span><span class="sxs-lookup"><span data-stu-id="2f2b3-343">355</span></span> |
| <span data-ttu-id="2f2b3-344">100</span><span class="sxs-lookup"><span data-stu-id="2f2b3-344">100</span></span> |<span data-ttu-id="2f2b3-345">10</span><span class="sxs-lookup"><span data-stu-id="2f2b3-345">10</span></span> |<span data-ttu-id="2f2b3-346">465</span><span class="sxs-lookup"><span data-stu-id="2f2b3-346">465</span></span> |
| <span data-ttu-id="2f2b3-347">50</span><span class="sxs-lookup"><span data-stu-id="2f2b3-347">50</span></span> |<span data-ttu-id="2f2b3-348">20</span><span class="sxs-lookup"><span data-stu-id="2f2b3-348">20</span></span> |<span data-ttu-id="2f2b3-349">630</span><span class="sxs-lookup"><span data-stu-id="2f2b3-349">630</span></span> |

> [!NOTE]
> <span data-ttu-id="2f2b3-350">A eredményei nem referenciaalapokhoz képest.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-350">Results are not benchmarks.</span></span> <span data-ttu-id="2f2b3-351">Tekintse meg a [időzítési eredményezi, hogy ez a témakör Megjegyzés](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="2f2b3-351">See the [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="2f2b3-352">Láthatja, hogy-e a legjobb teljesítményt 1000 sor elküldeni őket egyszerre.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-352">You can see that the best performance for 1000 rows is to submit them all at once.</span></span> <span data-ttu-id="2f2b3-353">Más tesztekben (itt nem látható) történt egy 10000 sor kötegelt felosztása két kötegek 5000 jobb a teljesítménye kis.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-353">In other tests (not shown here) there was a small performance gain to break a 10000 row batch into two batches of 5000.</span></span> <span data-ttu-id="2f2b3-354">Azonban ezekben a tesztekben a következő tábla sémáját viszonylag egyszerű, végre kell hajtania az adatokat és a Köteg mérete ezen eredmények ellenőrzése tesztek.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-354">But the table schema for these tests is relatively simple, so you should perform tests on your specific data and batch sizes to verify these findings.</span></span>

<span data-ttu-id="2f2b3-355">Egy másik szempont az, hogy, hogy a teljes kötegelt túl nagyra nő, ha SQL-adatbázis előfordulhat, hogy sávszélesség-szabályozási és elutasítja a kötegelt véglegesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-355">Another factor to consider is that if the total batch becomes too large, SQL Database might throttle and refuse to commit the batch.</span></span> <span data-ttu-id="2f2b3-356">A legjobb eredmény elérése érdekében tesztelje az adott forgatókönyv annak meghatározásához, hogy van-e az épp ezért tökéletes választás a köteg méretének.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-356">For the best results, test your specific scenario to determine if there is an ideal batch size.</span></span> <span data-ttu-id="2f2b3-357">Ellenőrizze a Köteg mérete konfigurálható teljesítmény vagy hibák alapján gyors módosításának engedélyezése a futási időben.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-357">Make the batch size configurable at runtime to enable quick adjustments based on performance or errors.</span></span>

<span data-ttu-id="2f2b3-358">Végezetül egyenleg a Köteg mérete a kötegelés kapcsolódó kockázatokat.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-358">Finally, balance the size of the batch with the risks associated with batching.</span></span> <span data-ttu-id="2f2b3-359">Ha átmeneti hiba merül fel, vagy a szerepkör nem sikerül, fontolja meg, majd próbálja megismételni a műveletet, vagy az adatvesztés a kötegben következményeit.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-359">If there are transient errors or the role fails, consider the consequences of retrying the operation or of losing the data in the batch.</span></span>

### <a name="parallel-processing"></a><span data-ttu-id="2f2b3-360">Párhuzamos feldolgozás</span><span class="sxs-lookup"><span data-stu-id="2f2b3-360">Parallel processing</span></span>
<span data-ttu-id="2f2b3-361">Mi történik, ha a köteg méretének csökkentését megközelítés tartott, de több szál hajthatók végre a munkahelyi?</span><span class="sxs-lookup"><span data-stu-id="2f2b3-361">What if you took the approach of reducing the batch size but used multiple threads to execute the work?</span></span> <span data-ttu-id="2f2b3-362">Ebben az esetben a tesztek bemutatta, hogy több kisebb többszálas kötegek általában végre a nagyobb kötegek rosszabb.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-362">Again, our tests showed that several smaller multithreaded batches typically performed worse than a single larger batch.</span></span> <span data-ttu-id="2f2b3-363">A következő teszt megkísérli 1000 sor beszúrása egy vagy több párhuzamos kötegekben.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-363">The following test attempts to insert 1000 rows in one or more parallel batches.</span></span> <span data-ttu-id="2f2b3-364">Ez a vizsgálat bemutatja, hogyan több egyidejű kötegek ténylegesen csökkent teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-364">This test shows how more simultaneous batches actually decreased performance.</span></span>

| <span data-ttu-id="2f2b3-365">A köteg méretének [ismétlési]</span><span class="sxs-lookup"><span data-stu-id="2f2b3-365">Batch size [Iterations]</span></span> | <span data-ttu-id="2f2b3-366">Két szállal (ms)</span><span class="sxs-lookup"><span data-stu-id="2f2b3-366">Two threads (ms)</span></span> | <span data-ttu-id="2f2b3-367">Négy szálak (ms)</span><span class="sxs-lookup"><span data-stu-id="2f2b3-367">Four threads (ms)</span></span> | <span data-ttu-id="2f2b3-368">Hat szálak (ms)</span><span class="sxs-lookup"><span data-stu-id="2f2b3-368">Six threads (ms)</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="2f2b3-369">1000 [1]</span><span class="sxs-lookup"><span data-stu-id="2f2b3-369">1000 [1]</span></span> |<span data-ttu-id="2f2b3-370">277</span><span class="sxs-lookup"><span data-stu-id="2f2b3-370">277</span></span> |<span data-ttu-id="2f2b3-371">315</span><span class="sxs-lookup"><span data-stu-id="2f2b3-371">315</span></span> |<span data-ttu-id="2f2b3-372">266</span><span class="sxs-lookup"><span data-stu-id="2f2b3-372">266</span></span> |
| <span data-ttu-id="2f2b3-373">500 [2]</span><span class="sxs-lookup"><span data-stu-id="2f2b3-373">500 [2]</span></span> |<span data-ttu-id="2f2b3-374">548</span><span class="sxs-lookup"><span data-stu-id="2f2b3-374">548</span></span> |<span data-ttu-id="2f2b3-375">278</span><span class="sxs-lookup"><span data-stu-id="2f2b3-375">278</span></span> |<span data-ttu-id="2f2b3-376">256</span><span class="sxs-lookup"><span data-stu-id="2f2b3-376">256</span></span> |
| <span data-ttu-id="2f2b3-377">250 [4]</span><span class="sxs-lookup"><span data-stu-id="2f2b3-377">250 [4]</span></span> |<span data-ttu-id="2f2b3-378">405</span><span class="sxs-lookup"><span data-stu-id="2f2b3-378">405</span></span> |<span data-ttu-id="2f2b3-379">329</span><span class="sxs-lookup"><span data-stu-id="2f2b3-379">329</span></span> |<span data-ttu-id="2f2b3-380">265</span><span class="sxs-lookup"><span data-stu-id="2f2b3-380">265</span></span> |
| <span data-ttu-id="2f2b3-381">100 [10]</span><span class="sxs-lookup"><span data-stu-id="2f2b3-381">100 [10]</span></span> |<span data-ttu-id="2f2b3-382">488</span><span class="sxs-lookup"><span data-stu-id="2f2b3-382">488</span></span> |<span data-ttu-id="2f2b3-383">439</span><span class="sxs-lookup"><span data-stu-id="2f2b3-383">439</span></span> |<span data-ttu-id="2f2b3-384">391</span><span class="sxs-lookup"><span data-stu-id="2f2b3-384">391</span></span> |

> [!NOTE]
> <span data-ttu-id="2f2b3-385">A eredményei nem referenciaalapokhoz képest.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-385">Results are not benchmarks.</span></span> <span data-ttu-id="2f2b3-386">Tekintse meg a [időzítési eredményezi, hogy ez a témakör Megjegyzés](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="2f2b3-386">See the [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="2f2b3-387">Van több lehetséges oka a akár teljesítménycsökkenés párhuzamosság miatt:</span><span class="sxs-lookup"><span data-stu-id="2f2b3-387">There are several potential reasons for the degradation in performance due to parallelism:</span></span>

* <span data-ttu-id="2f2b3-388">Nincsenek egy helyett több egyidejű hálózati hívást.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-388">There are multiple simultaneous network calls instead of one.</span></span>
* <span data-ttu-id="2f2b3-389">Egyetlen tábla több műveleteket versengés és blokkolja a eredményezhet.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-389">Multiple operations against a single table can result in contention and blocking.</span></span>
* <span data-ttu-id="2f2b3-390">Nincsenek társított terhek többszálas.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-390">There are overheads associated with multithreading.</span></span>
* <span data-ttu-id="2f2b3-391">Több kapcsolat megnyitásának járó költségek ez fontosabb, mint az az előnye, hogy a párhuzamos feldolgozást.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-391">The expense of opening multiple connections outweighs the benefit of parallel processing.</span></span>

<span data-ttu-id="2f2b3-392">Különböző táblákhoz vagy adatbázisok célozhat meg, akkor megállapíthatja, hogy ezt a stratégiát, hogy néhány teljesítmény.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-392">If you target different tables or databases, it is possible to see some performance gain with this strategy.</span></span> <span data-ttu-id="2f2b3-393">Adatbázis horizontális vagy összevonási lenne a forgatókönyv ezt a módszert használja.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-393">Database sharding or federations would be a scenario for this approach.</span></span> <span data-ttu-id="2f2b3-394">Horizontális több adatbázist használ, és továbbítja a különböző adatokat az egyes adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-394">Sharding uses multiple databases and routes different data to each database.</span></span> <span data-ttu-id="2f2b3-395">Ha egy kis köteg egy másik adatbázishoz, hatékonyabb lehet majd a műveletet hajt végre párhuzamosan.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-395">If each small batch is going to a different database, then performing the operations in parallel can be more efficient.</span></span> <span data-ttu-id="2f2b3-396">Jobb a teljesítménye azonban nem elég jelentős döntést alapjául használandó adatbázis horizontális használja a megoldásban.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-396">However, the performance gain is not significant enough to use as the basis for a decision to use database sharding in your solution.</span></span>

<span data-ttu-id="2f2b3-397">Néhány terveibe kisebb kötegekben párhuzamos végrehajtása eredményezhet továbbfejlesztett kapacitásának terhelés alatt a rendszer.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-397">In some designs, parallel execution of smaller batches can result in improved throughput of requests in a system under load.</span></span> <span data-ttu-id="2f2b3-398">Ebben az esetben akkor is, ha gyorsabb, nagyobb a kötegek feldolgozni, párhuzamosan több kötegenként lehet hatékonyabb.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-398">In this case, even though it is quicker to process a single larger batch, processing multiple batches in parallel might be more efficient.</span></span>

<span data-ttu-id="2f2b3-399">Ha párhuzamos végrehajtás, érdemes a munkaszálak maximális számának vezérlése.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-399">If you do use parallel execution, consider controlling the maximum number of worker threads.</span></span> <span data-ttu-id="2f2b3-400">Kevesebb gyorsabb végrehajtási idő, és kevesebb a versengés eredményezheti.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-400">A smaller number might result in less contention and a faster execution time.</span></span> <span data-ttu-id="2f2b3-401">Is fontolja meg az egyéb terheléseket, amelyeket a ez helyez el a céladatbázist, kapcsolatok és a tranzakciók egyaránt.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-401">Also, consider the additional load that this places on the target database both in connections and transactions.</span></span>

### <a name="related-performance-factors"></a><span data-ttu-id="2f2b3-402">A teljesítmény kapcsolódó tényezők</span><span class="sxs-lookup"><span data-stu-id="2f2b3-402">Related performance factors</span></span>
<span data-ttu-id="2f2b3-403">Adatbázis teljesítménye jellemző útmutatást is kötegelés hatással van.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-403">Typical guidance on database performance also affects batching.</span></span> <span data-ttu-id="2f2b3-404">Például be nagy elsődleges kulcs, vagy sok fürtözetlen indexeire rendelkező táblák csökken a teljesítmény.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-404">For example, insert performance is reduced for tables that have a large primary key or many nonclustered indexes.</span></span>

<span data-ttu-id="2f2b3-405">Ha a tábla értékű paraméter tárolt eljárás használatához a paranccsal **SET NOCOUNT ON** az eljárás elején.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-405">If table-valued parameters use a stored procedure, you can use the command **SET NOCOUNT ON** at the beginning of the procedure.</span></span> <span data-ttu-id="2f2b3-406">A jelen nyilatkozat mellőzi a visszatérési az eljárás az érintett sorok száma.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-406">This statement suppresses the return of the count of the affected rows in the procedure.</span></span> <span data-ttu-id="2f2b3-407">Azonban a tesztelés során használatát **SET NOCOUNT ON** kellett nincs hatása, vagy teljesítménye csökkent.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-407">However, in our tests, the use of **SET NOCOUNT ON** either had no effect or decreased performance.</span></span> <span data-ttu-id="2f2b3-408">A vizsgálati tárolt eljárás egyetlen egyszerű volt **BESZÚRÁSA** parancsot a tábla értékű paraméter.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-408">The test stored procedure was simple with a single **INSERT** command from the table-valued parameter.</span></span> <span data-ttu-id="2f2b3-409">Akkor lehet, hogy összetettebb tárolt eljárások a jelen nyilatkozat előnyös.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-409">It is possible that more complex stored procedures would benefit from this statement.</span></span> <span data-ttu-id="2f2b3-410">Nem érdemes feltételezni, hogy hozzáadása, de **SET NOCOUNT ON** a tárolt eljárás automatikusan javítja a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-410">But don’t assume that adding **SET NOCOUNT ON** to your stored procedure automatically improves performance.</span></span> <span data-ttu-id="2f2b3-411">Szeretné megtudni, a hatás, tesztelje a tárolt eljárás használatával és anélkül a **SET NOCOUNT ON** utasítást.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-411">To understand the effect, test your stored procedure with and without the **SET NOCOUNT ON** statement.</span></span>

## <a name="batching-scenarios"></a><span data-ttu-id="2f2b3-412">Kötegelése forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="2f2b3-412">Batching scenarios</span></span>
<span data-ttu-id="2f2b3-413">Az alábbi szakaszok ismertetik a tábla értékű paraméter három alkalmazás helyzetekben használhatja.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-413">The following sections describe how to use table-valued parameters in three application scenarios.</span></span> <span data-ttu-id="2f2b3-414">Az első forgatókönyv bemutatja, hogyan pufferelés és kötegelés hogyan tudnak együttműködni.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-414">The first scenario shows how buffering and batching can work together.</span></span> <span data-ttu-id="2f2b3-415">A második forgatókönyv javítja a teljesítményt, egyetlen tárolt eljárás hívása a fő-részletek műveletet hajt végre.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-415">The second scenario improves performance by performing master-detail operations in a single stored procedure call.</span></span> <span data-ttu-id="2f2b3-416">A végső forgatókönyv bemutatja, hogyan tábla értékű paraméterek használatához a "UPSERT" művelet.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-416">The final scenario shows how to use table-valued parameters in an “UPSERT” operation.</span></span>

### <a name="buffering"></a><span data-ttu-id="2f2b3-417">Pufferelés</span><span class="sxs-lookup"><span data-stu-id="2f2b3-417">Buffering</span></span>
<span data-ttu-id="2f2b3-418">Habár van néhány olyan forgatókönyvet, nyilvánvaló jelölt kötegelés, ott sikerült előnyeit által késleltetett feldolgozási kötegelés több forgatókönyv áll.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-418">Although there are some scenarios that are obvious candidate for batching, there are many scenarios that could take advantage of batching by delayed processing.</span></span> <span data-ttu-id="2f2b3-419">Késleltetett feldolgozási is, hogy az adatok nem vesztek el meghibásodása nagyobb veszélynek végzi.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-419">However, delayed processing also carries a greater risk that the data is lost in the event of an unexpected failure.</span></span> <span data-ttu-id="2f2b3-420">Fontos megérteni a kockázat, és vegye figyelembe a következményekkel.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-420">It is important to understand this risk and consider the consequences.</span></span>

<span data-ttu-id="2f2b3-421">Vegye figyelembe például egy webes alkalmazás, amely nyomon követi a minden felhasználó előzménylistáján.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-421">For example, consider a web application that tracks the navigation history of each user.</span></span> <span data-ttu-id="2f2b3-422">A lap lekérése az alkalmazás sikerült hívható meg a felhasználó lapmegtekintés rögzítésére adatbázis.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-422">On each page request, the application could make a database call to record the user’s page view.</span></span> <span data-ttu-id="2f2b3-423">De a nagyobb teljesítmény és méretezhetőség legyen elérhető a felhasználók navigációs tevékenységek pufferelés, és elküldi ezeket az adatokat az adatbázisba kötegekben.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-423">But higher performance and scalability can be achieved by buffering the users’ navigation activities and then sending this data to the database in batches.</span></span> <span data-ttu-id="2f2b3-424">Az adatbázis frissítést a futása közben eltelt idő és/vagy puffer mérete indíthat el.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-424">You can trigger the database update by elapsed time and/or buffer size.</span></span> <span data-ttu-id="2f2b3-425">Egy szabály például megadhatja, hogy a kötegelt 20 másodperc, vagy amikor a puffer eléri-e 1000 elemek után fel kell dolgozni.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-425">For example, a rule could specify that the batch should be processed after 20 seconds or when the buffer reaches 1000 items.</span></span>

<span data-ttu-id="2f2b3-426">Az alábbi példakód [reaktív bővítmények – a Rx](https://msdn.microsoft.com/data/gg577609) figyelési osztály által kiváltott pufferelt események feldolgozásához.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-426">The following code example uses [Reactive Extensions - Rx](https://msdn.microsoft.com/data/gg577609) to process buffered events raised by a monitoring class.</span></span> <span data-ttu-id="2f2b3-427">Amikor beírja a puffer, vagy időkorlátot, a felhasználói adatok kötegelt zajlik egy tábla értékű paraméter az adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-427">When the buffer fills or a timeout is reached, the batch of user data is sent to the database with a table-valued parameter.</span></span>

<span data-ttu-id="2f2b3-428">A következő NavHistoryData osztály modellek a felhasználó navigációs adatait.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-428">The following NavHistoryData class models the user navigation details.</span></span> <span data-ttu-id="2f2b3-429">Például a felhasználói azonosító az elért URL-cím vagy a hozzáférés idejének alapszintű információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-429">It contains basic information such as the user identifier, the URL accessed, and the access time.</span></span>

    public class NavHistoryData
    {
        public NavHistoryData(int userId, string url, DateTime accessTime)
        { UserId = userId; URL = url; AccessTime = accessTime; }
        public int UserId { get; set; }
        public string URL { get; set; }
        public DateTime AccessTime { get; set; }
    }

<span data-ttu-id="2f2b3-430">A NavHistoryDataMonitor osztály a felhasználói navigációs adatokat az adatbázisba pufferelés felelős.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-430">The NavHistoryDataMonitor class is responsible for buffering the user navigation data to the database.</span></span> <span data-ttu-id="2f2b3-431">Tartalmaz egy metódust, RecordUserNavigationEntry, amely válaszol-e megjelenítve jelzi egy **OnAdded** esemény.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-431">It contains a method, RecordUserNavigationEntry, which responds by raising an **OnAdded** event.</span></span> <span data-ttu-id="2f2b3-432">A következő kód bemutatja a konstruktor logika, amely használja a Rx hozzon létre egy megfigyelhető gyűjteményt az események alapján.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-432">The following code shows the constructor logic that uses Rx to create an observable collection based on the event.</span></span> <span data-ttu-id="2f2b3-433">Majd feliratkozva a megfigyelhető gyűjteményhez a következő puffer metódussal.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-433">It then subscribes to this observable collection with the Buffer method.</span></span> <span data-ttu-id="2f2b3-434">A túlterhelés határozza meg, hogy a memóriapuffer 20 másodpercenként vagy 1000 bejegyzések küldjön.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-434">The overload specifies that the buffer should be sent every 20 seconds or 1000 entries.</span></span>

    public NavHistoryDataMonitor()
    {
        var observableData =
            Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");

        observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
    }

<span data-ttu-id="2f2b3-435">A kezelő összes pufferelt elem alakítja át a tábla értékű típus, és majd átadja a ehhez a típushoz, amely feldolgozza a kötegelt tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-435">The handler converts all of the buffered items into a table-valued type and then passes this type to a stored procedure that processes the batch.</span></span> <span data-ttu-id="2f2b3-436">A következő kód bemutatja a NavHistoryDataEventArgs, mind a NavHistoryDataMonitor osztályok teljes definíciója.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-436">The following code shows the complete definition for both the NavHistoryDataEventArgs and the NavHistoryDataMonitor classes.</span></span>

    public class NavHistoryDataEventArgs : System.EventArgs
    {
        public NavHistoryDataEventArgs(NavHistoryData data) { Data = data; }
        public NavHistoryData Data { get; set; }
    }

    public class NavHistoryDataMonitor
    {
        public event EventHandler<NavHistoryDataEventArgs> OnAdded;

        public NavHistoryDataMonitor()
        {
            var observableData =
                Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");

            observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
        }

        public void RecordUserNavigationEntry(NavHistoryData data)
        {    
            if (OnAdded != null)
                OnAdded(this, new NavHistoryDataEventArgs(data));
        }

        protected void Handler(IList<EventPattern<NavHistoryDataEventArgs>> items)
        {
            DataTable navHistoryBatch = new DataTable("NavigationHistoryBatch");
            navHistoryBatch.Columns.Add("UserId", typeof(int));
            navHistoryBatch.Columns.Add("URL", typeof(string));
            navHistoryBatch.Columns.Add("AccessTime", typeof(DateTime));
            foreach (EventPattern<NavHistoryDataEventArgs> item in items)
            {
                NavHistoryData data = item.EventArgs.Data;
                navHistoryBatch.Rows.Add(data.UserId, data.URL, data.AccessTime);
            }

            using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
            {
                connection.Open();

                SqlCommand cmd = new SqlCommand("sp_RecordUserNavigation", connection);
                cmd.CommandType = CommandType.StoredProcedure;

                cmd.Parameters.Add(
                    new SqlParameter()
                    {
                        ParameterName = "@NavHistoryBatch",
                        SqlDbType = SqlDbType.Structured,
                        TypeName = "NavigationHistoryTableType",
                        Value = navHistoryBatch,
                    });

                cmd.ExecuteNonQuery();
            }
        }
    }

<span data-ttu-id="2f2b3-437">Ez az osztály pufferelési használatához az alkalmazás egy statikus NavHistoryDataMonitor objektumot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-437">To use this buffering class, the application creates a static NavHistoryDataMonitor object.</span></span> <span data-ttu-id="2f2b3-438">Minden alkalommal, amikor egy felhasználó egy lap fér hozzá az alkalmazás meghívja a NavHistoryDataMonitor.RecordUserNavigationEntry metódust.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-438">Each time a user accesses a page, the application calls the NavHistoryDataMonitor.RecordUserNavigationEntry method.</span></span> <span data-ttu-id="2f2b3-439">Ezek a bejegyzések küldése az adatbázis kötegekben irányuló pufferelési logika eltérő lehet.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-439">The buffering logic proceeds to take care of sending these entries to the database in batches.</span></span>

### <a name="master-detail"></a><span data-ttu-id="2f2b3-440">Fő részletei</span><span class="sxs-lookup"><span data-stu-id="2f2b3-440">Master detail</span></span>
<span data-ttu-id="2f2b3-441">Tábla értékű paraméter egyszerű INSERT forgatókönyvek hasznosak.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-441">Table-valued parameters are useful for simple INSERT scenarios.</span></span> <span data-ttu-id="2f2b3-442">Azonban lehet, például az egynél több tábla kötegelt Beszúrás további kihívást.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-442">However, it can be more challenging to batch inserts that involve more than one table.</span></span> <span data-ttu-id="2f2b3-443">A "kapcsolatú" például az is jó példa.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-443">The “master/detail” scenario is a good example.</span></span> <span data-ttu-id="2f2b3-444">A fő táblázat azonosítja az elsődleges entitás.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-444">The master table identifies the primary entity.</span></span> <span data-ttu-id="2f2b3-445">Egy vagy több részletek tábla entitás több adatot tároljon.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-445">One or more detail tables store more data about the entity.</span></span> <span data-ttu-id="2f2b3-446">Ebben a forgatókönyvben a külső kulcsok kapcsolatai kényszerítése a kapcsolat egy egyedi fő entitásra részletességi.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-446">In this scenario, foreign key relationships enforce the relationship of details to a unique master entity.</span></span> <span data-ttu-id="2f2b3-447">Vegye figyelembe a PurchaseOrder és a kapcsolódó OrderDetail tábla egyszerűsített verziója.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-447">Consider a simplified version of a PurchaseOrder table and its associated OrderDetail table.</span></span> <span data-ttu-id="2f2b3-448">A következő Transact-SQL négy oszlopot hoz létre a PurchaseOrder tábla: OrderID, orderdate oszlopra, CustomerID és állapotát.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-448">The following Transact-SQL creates the PurchaseOrder table with four columns: OrderID, OrderDate, CustomerID, and Status.</span></span>

    CREATE TABLE [dbo].[PurchaseOrder](
    [OrderID] [int] IDENTITY(1,1) NOT NULL,
    [OrderDate] [datetime] NOT NULL,
    [CustomerID] [int] NOT NULL,
    [Status] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrder] 
    PRIMARY KEY CLUSTERED ( [OrderID] ASC ))

<span data-ttu-id="2f2b3-449">Minden egyes rendelés egy vagy több termék vásárlás tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-449">Each order contains one or more product purchases.</span></span> <span data-ttu-id="2f2b3-450">Ezt az információt a PurchaseOrderDetail tábla rögzített.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-450">This information is captured in the PurchaseOrderDetail table.</span></span> <span data-ttu-id="2f2b3-451">A következő Transact-SQL hoz létre a PurchaseOrderDetail tábla öt oszlopok: OrderID, OrderDetailID, ProductID, Egységár és OrderQty.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-451">The following Transact-SQL creates the PurchaseOrderDetail table with five columns: OrderID, OrderDetailID, ProductID, UnitPrice, and OrderQty.</span></span>

    CREATE TABLE [dbo].[PurchaseOrderDetail](
    [OrderID] [int] NOT NULL,
    [OrderDetailID] [int] IDENTITY(1,1) NOT NULL,
    [ProductID] [int] NOT NULL,
    [UnitPrice] [money] NULL,
    [OrderQty] [smallint] NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrderDetail] PRIMARY KEY CLUSTERED 
    ( [OrderID] ASC, [OrderDetailID] ASC ))

<span data-ttu-id="2f2b3-452">Az OrderID oszlop a PurchaseOrderDetail tábla sorrendben kell hivatkoznia, a PurchaseOrder táblából.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-452">The OrderID column in the PurchaseOrderDetail table must reference an order from the PurchaseOrder table.</span></span> <span data-ttu-id="2f2b3-453">A következő idegen kulcs definícióját a korlátozás érvényesítése.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-453">The following definition of a foreign key enforces this constraint.</span></span>

    ALTER TABLE [dbo].[PurchaseOrderDetail]  WITH CHECK ADD 
    CONSTRAINT [FK_OrderID_PurchaseOrder] FOREIGN KEY([OrderID])
    REFERENCES [dbo].[PurchaseOrder] ([OrderID])

<span data-ttu-id="2f2b3-454">Tábla értékű paraméter használatához rendelkeznie kell egy felhasználó által definiált táblatípus minden céloldali táblához.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-454">In order to use table-valued parameters, you must have one user-defined table type for each target table.</span></span>

    CREATE TYPE PurchaseOrderTableType AS TABLE 
    ( OrderID INT,
      OrderDate DATETIME,
      CustomerID INT,
      Status NVARCHAR(50) );
    GO

    CREATE TYPE PurchaseOrderDetailTableType AS TABLE 
    ( OrderID INT,
      ProductID INT,
      UnitPrice MONEY,
      OrderQty SMALLINT );
    GO

<span data-ttu-id="2f2b3-455">Majd adja meg, amely támogatja a következő típusú táblák tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-455">Then define a stored procedure that accepts tables of these types.</span></span> <span data-ttu-id="2f2b3-456">Ez az eljárás lehetővé teszi az helyileg a batch-rendeléseket és egy hívás a rendelés részleteit.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-456">This procedure allows an application to locally batch a set of orders and order details in a single call.</span></span> <span data-ttu-id="2f2b3-457">A következő Transact-SQL beszerzési sorrendje a példa a teljes tárolt eljárás nyilatkozat biztosít.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-457">The following Transact-SQL provides the complete stored procedure declaration for this purchase order example.</span></span>

    CREATE PROCEDURE sp_InsertOrdersBatch (
    @orders as PurchaseOrderTableType READONLY,
    @details as PurchaseOrderDetailTableType READONLY )
    AS
    SET NOCOUNT ON;

    -- Table that connects the order identifiers in the @orders
    -- table with the actual order identifiers in the PurchaseOrder table
    DECLARE @IdentityLink AS TABLE ( 
    SubmittedKey int, 
    ActualKey int, 
    RowNumber int identity(1,1)
    );

          -- Add new orders to the PurchaseOrder table, storing the actual
    -- order identifiers in the @IdentityLink table   
    INSERT INTO PurchaseOrder ([OrderDate], [CustomerID], [Status])
    OUTPUT inserted.OrderID INTO @IdentityLink (ActualKey)
    SELECT [OrderDate], [CustomerID], [Status] FROM @orders ORDER BY OrderID;

    -- Match the passed-in order identifiers with the actual identifiers
    -- and complete the @IdentityLink table for use with inserting the details
    WITH OrderedRows As (
    SELECT OrderID, ROW_NUMBER () OVER (ORDER BY OrderID) As RowNumber 
    FROM @orders
    )
    UPDATE @IdentityLink SET SubmittedKey = M.OrderID
    FROM @IdentityLink L JOIN OrderedRows M ON L.RowNumber = M.RowNumber;

    -- Insert the order details into the PurchaseOrderDetail table, 
          -- using the actual order identifiers of the master table, PurchaseOrder
    INSERT INTO PurchaseOrderDetail (
    [OrderID],
    [ProductID],
    [UnitPrice],
    [OrderQty] )
    SELECT L.ActualKey, D.ProductID, D.UnitPrice, D.OrderQty
    FROM @details D
    JOIN @IdentityLink L ON L.SubmittedKey = D.OrderID;
    GO

<span data-ttu-id="2f2b3-458">Ebben a példában a helyileg definiált @IdentityLink tábla tárolja az újonnan behelyezett sorainak tényleges OrderID értéke.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-458">In this example, the locally defined @IdentityLink table stores the actual OrderID values from the newly inserted rows.</span></span> <span data-ttu-id="2f2b3-459">Ilyen rendelés azonosítókat az ideiglenes OrderID értékek eltérnek a @orders és @details tábla értékű paraméter.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-459">These order identifiers are different from the temporary OrderID values in the @orders and @details table-valued parameters.</span></span> <span data-ttu-id="2f2b3-460">Emiatt a @IdentityLink tábla csatlakoztatja az OrderID értékeit a @orders paramétert az új sort a PurchaseOrder tábla valós OrderID értékeit.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-460">For this reason, the @IdentityLink table then connects the OrderID values from the @orders parameter to the real OrderID values for the new rows in the PurchaseOrder table.</span></span> <span data-ttu-id="2f2b3-461">Ez a lépés után a @IdentityLink tábla megkönnyítheti a rendelés részleteit és a tényleges OrderID, amely eleget tesz a Külsőkulcs-korlátozást beszúrni.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-461">After this step, the @IdentityLink table can facilitate inserting the order details with the actual OrderID that satisfies the foreign key constraint.</span></span>

<span data-ttu-id="2f2b3-462">Ez a tárolt eljárás használható kód vagy más Transact-SQL-hívások.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-462">This stored procedure can be used from code or from other Transact-SQL calls.</span></span> <span data-ttu-id="2f2b3-463">A dokumentum a kód például a táblázat értékű paramétereket című szakaszában talál.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-463">See the table-valued parameters section of this paper for a code example.</span></span> <span data-ttu-id="2f2b3-464">A következő Transact-SQL bemutatja, hogyan hívhatja meg a sp_InsertOrdersBatch.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-464">The following Transact-SQL shows how to call the sp_InsertOrdersBatch.</span></span>

    declare @orders as PurchaseOrderTableType
    declare @details as PurchaseOrderDetailTableType

    INSERT @orders 
    ([OrderID], [OrderDate], [CustomerID], [Status])
    VALUES(1, '1/1/2013', 1125, 'Complete'),
    (2, '1/13/2013', 348, 'Processing'),
    (3, '1/12/2013', 2504, 'Shipped')

    INSERT @details
    ([OrderID], [ProductID], [UnitPrice], [OrderQty])
    VALUES(1, 10, $11.50, 1),
    (1, 12, $1.58, 1),
    (2, 23, $2.57, 2),
    (3, 4, $10.00, 1)

    exec sp_InsertOrdersBatch @orders, @details

<span data-ttu-id="2f2b3-465">Ez a megoldás lehetővé teszi, hogy az egyes kötegekben OrderID értékek: 1 kezdődő használandó.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-465">This solution allows each batch to use a set of OrderID values that begin at 1.</span></span> <span data-ttu-id="2f2b3-466">Ideiglenes OrderID értékeiről leírják a kötegben lévő kapcsolatok, de a tényleges OrderID értékek az insert művelet időpontjában határozza meg.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-466">These temporary OrderID values describe the relationships in the batch, but the actual OrderID values are determined at the time of the insert operation.</span></span> <span data-ttu-id="2f2b3-467">Futtassa az előző példában szereplő ismételten a azonos utasításokat, és egyedi rendeléseket lehet létrehozni az adatbázist.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-467">You can run the same statements in the previous example repeatedly and generate unique orders in the database.</span></span> <span data-ttu-id="2f2b3-468">Emiatt érdemes további kóddal vagy az adatbázis logika, amely megakadályozza a duplikált rendelések használatakor ezzel a technikával kötegelés.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-468">For this reason, consider adding more code or database logic that prevents duplicate orders when using this batching technique.</span></span>

<span data-ttu-id="2f2b3-469">Ez a példa mutatja be, hogy még inkább összetett adatbázis-műveletek, például a részletezés master operations, is lehet kötegelni tábla értékű paraméterek használatával.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-469">This example demonstrates that even more complex database operations, such as master-detail operations, can be batched using table-valued parameters.</span></span>

### <a name="upsert"></a><span data-ttu-id="2f2b3-470">UPSERT</span><span class="sxs-lookup"><span data-stu-id="2f2b3-470">UPSERT</span></span>
<span data-ttu-id="2f2b3-471">Egy másik kötegelési forgatókönyv magában foglalja a egyidejűleg frissíteni a létező sorok és új sort beszúrni.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-471">Another batching scenario involves simultaneously updating existing rows and inserting new rows.</span></span> <span data-ttu-id="2f2b3-472">Ez a művelet van más néven "UPSERT" (frissítés + insert) műveletet.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-472">This operation is sometimes referred to as an “UPSERT” (update + insert) operation.</span></span> <span data-ttu-id="2f2b3-473">Ahelyett, hogy külön hívások BESZÚRÁSA, és FRISSÍTI a MERGE utasítás bizonyul a legalkalmasabbnak a tevékenységhez.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-473">Rather than making separate calls to INSERT and UPDATE, the MERGE statement is best suited to this task.</span></span> <span data-ttu-id="2f2b3-474">A MERGE utasítás végrehajtása mindkét insert és frissítési művelet egyetlen hívással.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-474">The MERGE statement can perform both insert and update operations in a single call.</span></span>

<span data-ttu-id="2f2b3-475">Tábla értékű paraméter frissítések és a Beszúrás elvégzéséhez használható a MERGE utasítással.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-475">Table-valued parameters can be used with the MERGE statement to perform updates and inserts.</span></span> <span data-ttu-id="2f2b3-476">Vegyük példaként a következő oszlopokat tartalmazó egyszerűsített alkalmazott táblázat: EmployeeID, az Utónév, a Vezetéknév, a SocialSecurityNumber:</span><span class="sxs-lookup"><span data-stu-id="2f2b3-476">For example, consider a simplified Employee table that contains the following columns: EmployeeID, FirstName, LastName, SocialSecurityNumber:</span></span>

    CREATE TABLE [dbo].[Employee](
    [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
    [FirstName] [nvarchar](50) NOT NULL,
    [LastName] [nvarchar](50) NOT NULL,
    [SocialSecurityNumber] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_Employee] PRIMARY KEY CLUSTERED 
    ([EmployeeID] ASC ))

<span data-ttu-id="2f2b3-477">Ebben a példában használhatja arra, hogy a SocialSecurityNumber egyedi több alkalmazott EGYESÍTÉSÉVEL végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-477">In this example, you can use the fact that the SocialSecurityNumber is unique to perform a MERGE of multiple employees.</span></span> <span data-ttu-id="2f2b3-478">Először hozza létre a felhasználó által definiált táblatípus:</span><span class="sxs-lookup"><span data-stu-id="2f2b3-478">First, create the user-defined table type:</span></span>

    CREATE TYPE EmployeeTableType AS TABLE 
    ( Employee_ID INT,
      FirstName NVARCHAR(50),
      LastName NVARCHAR(50),
      SocialSecurityNumber NVARCHAR(50) );
    GO

<span data-ttu-id="2f2b3-479">A következő tárolt eljárás létrehozása, vagy kiírhatja a MERGE utasítás segítségével hajtsa végre a frissítést, majd szúrja be a kódját.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-479">Next, create a stored procedure or write code that uses the MERGE statement to perform the update and insert.</span></span> <span data-ttu-id="2f2b3-480">Az alábbi példában a MERGE utasítás egy tábla értékű paraméter @employees, EmployeeTableType típusú.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-480">The following example uses the MERGE statement on a table-valued parameter, @employees, of type EmployeeTableType.</span></span> <span data-ttu-id="2f2b3-481">A tartalmát a @employees tábla itt nem látható.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-481">The contents of the @employees table are not shown here.</span></span>

    MERGE Employee AS target
    USING (SELECT [FirstName], [LastName], [SocialSecurityNumber] FROM @employees) 
    AS source ([FirstName], [LastName], [SocialSecurityNumber])
    ON (target.[SocialSecurityNumber] = source.[SocialSecurityNumber])
    WHEN MATCHED THEN 
    UPDATE SET
    target.FirstName = source.FirstName, 
    target.LastName = source.LastName
    WHEN NOT MATCHED THEN
       INSERT ([FirstName], [LastName], [SocialSecurityNumber])
       VALUES (source.[FirstName], source.[LastName], source.[SocialSecurityNumber]);

<span data-ttu-id="2f2b3-482">További információkért lásd: a dokumentáció és példák a MERGE utasításban.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-482">For more information, see the documentation and examples for the MERGE statement.</span></span> <span data-ttu-id="2f2b3-483">Bár a többlépéses tárolt ugyanaz a munkahelyi sikerült végezhető el a eljáráshívási külön INSERT, és frissítési műveleteket, a MERGE utasítás hatékonyabban.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-483">Although the same work could be performed in a multiple-step stored procedure call with separate INSERT and UPDATE operations, the MERGE statement is more efficient.</span></span> <span data-ttu-id="2f2b3-484">Adatbázis kód hogyan hozhat létre a MERGE utasítás közvetlenül nélkül két adatbázis hívások az INSERT vagy UPDATE használó Transact-SQL-hívások is.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-484">Database code can also construct Transact-SQL calls that use the MERGE statement directly without requiring two database calls for INSERT and UPDATE.</span></span>

## <a name="recommendation-summary"></a><span data-ttu-id="2f2b3-485">A javaslat összefoglaló</span><span class="sxs-lookup"><span data-stu-id="2f2b3-485">Recommendation summary</span></span>
<span data-ttu-id="2f2b3-486">Az alábbi lista a jelen témakörben bemutatott kötegelési ajánlások összegzését tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="2f2b3-486">The following list provides a summary of the batching recommendations discussed in this topic:</span></span>

* <span data-ttu-id="2f2b3-487">Pufferelés és kötegelés segítségével növelheti a teljesítményét és méretezhetőségét SQL adatbázis-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-487">Use buffering and batching to increase the performance and scalability of SQL Database applications.</span></span>
* <span data-ttu-id="2f2b3-488">Ismerje meg a mellékhatásokkal kötegelés/pufferelés és a rugalmasság között.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-488">Understand the tradeoffs between batching/buffering and resiliency.</span></span> <span data-ttu-id="2f2b3-489">Adott szerepkör meghibásodás során a egy feldolgozatlan kötegelt az üzleti szempontból kritikus fontosságú adatok elvesztését kockáztatja a teljesítmény előnye, hogy kötegelés előfordulhat, hogy járó.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-489">During a role failure, the risk of losing an unprocessed batch of business-critical data might outweigh the performance benefit of batching.</span></span>
* <span data-ttu-id="2f2b3-490">Tartsa a késés csökkentése érdekében az adatbázis egy adatközponton belül minden hívások történt kísérlet.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-490">Attempt to keep all calls to the database within a single datacenter to reduce latency.</span></span>
* <span data-ttu-id="2f2b3-491">Ha úgy dönt, hogy egyetlen kötegelési technika, tábla értékű paraméter nyújtanak a legjobb teljesítményt és rugalmasságot biztosít.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-491">If you choose a single batching technique, table-valued parameters offer the best performance and flexibility.</span></span>
* <span data-ttu-id="2f2b3-492">A leggyorsabb insert teljesítmény érdekében kövesse az alábbi általános irányelveket, de a forgatókönyv teszteléséhez:</span><span class="sxs-lookup"><span data-stu-id="2f2b3-492">For the fastest insert performance, follow these general guidelines but test your scenario:</span></span>
  * <span data-ttu-id="2f2b3-493">< 100 sorai egy paraméteres INSERT parancs használata.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-493">For < 100 rows, use a single parameterized INSERT command.</span></span>
  * <span data-ttu-id="2f2b3-494">A < 1000 sor használja a táblázat értékű paramétereket.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-494">For < 1000 rows, use table-valued parameters.</span></span>
  * <span data-ttu-id="2f2b3-495">A > = 1000 sorok, SqlBulkCopy használja.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-495">For >= 1000 rows, use SqlBulkCopy.</span></span>
* <span data-ttu-id="2f2b3-496">A és törlési műveletek frissítéséhez használja tábla értékű paraméter tárolt eljárás logikával határozza meg, hogy a megfelelő műveletet az egyes sorokkal. a tábla paraméterben.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-496">For update and delete operations, use table-valued parameters with stored procedure logic that determines the correct operation on each row in the table parameter.</span></span>
* <span data-ttu-id="2f2b3-497">Köteg mérete irányelveket:</span><span class="sxs-lookup"><span data-stu-id="2f2b3-497">Batch size guidelines:</span></span>
  * <span data-ttu-id="2f2b3-498">A legnagyobb kötegelt méretek számára az alkalmazás- és üzleti követelmények célszerű használni.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-498">Use the largest batch sizes that make sense for your application and business requirements.</span></span>
  * <span data-ttu-id="2f2b3-499">A jobb teljesítménye nagy köteg a kockázatot jelentő ideiglenes vagy végzetes hibák elosztása.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-499">Balance the performance gain of large batches with the risks of temporary or catastrophic failures.</span></span> <span data-ttu-id="2f2b3-500">Mi az a következménye az újrapróbálkozások és az adatvesztés a kötegben?</span><span class="sxs-lookup"><span data-stu-id="2f2b3-500">What is the consequence of retries or loss of the data in the batch?</span></span> 
  * <span data-ttu-id="2f2b3-501">A legnagyobb kötegméret ellenőrzése, hogy SQL-adatbázis nem elutasítania tesztelése.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-501">Test the largest batch size to verify that SQL Database does not reject it.</span></span>
  * <span data-ttu-id="2f2b3-502">A vezérlő kötegelés, például a köteg méretének vagy a pufferelési időszak-konfigurációs beállítások létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-502">Create configuration settings that control batching, such as the batch size or the buffering time window.</span></span> <span data-ttu-id="2f2b3-503">Ezek a beállítások rugalmasságot biztosít.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-503">These settings provide flexibility.</span></span> <span data-ttu-id="2f2b3-504">Éles környezetben kötegelési viselkedést a felhőalapú szolgáltatás ismételt üzembe helyezésével módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-504">You can change the batching behavior in production without redeploying the cloud service.</span></span>
* <span data-ttu-id="2f2b3-505">Kerülje a párhuzamos végrehajtás kötegek, amely több adatbázis egyetlen táblájára működik.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-505">Avoid parallel execution of batches that operate on a single table in one database.</span></span> <span data-ttu-id="2f2b3-506">Ha a kötegek osztja szét több munkavégző szál, tesztek futtatása annak szálak ideális számának meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-506">If you do choose to divide a single batch across multiple worker threads, run tests to determine the ideal number of threads.</span></span> <span data-ttu-id="2f2b3-507">Után egy nem meghatározott küszöbértéket több szál fog miatta a teljesítmény, nem pedig növeli azt.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-507">After an unspecified threshold, more threads will decrease performance rather than increase it.</span></span>
* <span data-ttu-id="2f2b3-508">Vegye figyelembe a pufferelés méret és így további forgatókönyvek kötegelés végrehajtási idő.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-508">Consider buffering on size and time as a way of implementing batching for more scenarios.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2f2b3-509">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2f2b3-509">Next steps</span></span>
<span data-ttu-id="2f2b3-510">Ez a cikk összpontosított hogyan adatbázis tervezési és a kapcsolódó kötegelés technikák kódolási javíthatja az alkalmazás teljesítményét és méretezhetőségét.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-510">This article focused on how database design and coding techniques related to batching can improve your application performance and scalability.</span></span> <span data-ttu-id="2f2b3-511">Ez azonban csak egy tényező az általános stratégiában.</span><span class="sxs-lookup"><span data-stu-id="2f2b3-511">But this is just one factor in your overall strategy.</span></span> <span data-ttu-id="2f2b3-512">A jobb teljesítmény és méretezhetőség további részleteket lásd: [Azure SQL Database teljesítményét útmutatást az önálló adatbázisok](sql-database-performance-guidance.md) és [rugalmas készletek ára és teljesítménye szempontok](sql-database-elastic-pool-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="2f2b3-512">For more ways to improve performance and scalability, see [Azure SQL Database performance guidance for single databases](sql-database-performance-guidance.md) and [Price and performance considerations for an elastic pool](sql-database-elastic-pool-guidance.md).</span></span>

