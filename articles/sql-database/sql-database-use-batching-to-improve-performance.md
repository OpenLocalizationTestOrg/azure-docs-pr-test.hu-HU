---
title: "az Azure SQL Database alkalmazásteljesítmény tooimprove kötegelés aaaHow toouse"
description: "hello témakör igazolja, hogy kötegelés adatbázis-műveletek nagy mértékben imroves hello sebesség és a méretezhetőség, az Azure SQL adatbázis-alkalmazások. Bár ezek a technológiák kötegelési bármely SQL Server-adatbázis használatához hello hello a cikk célja az Azure-on."
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
ms.openlocfilehash: 124b203ee69c595f0813852ff09ef9ec6841233a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-batching-tooimprove-sql-database-application-performance"></a><span data-ttu-id="d17de-104">Hogyan tooimprove SQL-adatbázis teljesítményének kötegelés toouse</span><span class="sxs-lookup"><span data-stu-id="d17de-104">How toouse batching tooimprove SQL Database application performance</span></span>
<span data-ttu-id="d17de-105">Műveletek tooAzure SQL-adatbázis kötegelés jelentősen javítja a hello teljesítményét és méretezhetőségét, az alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="d17de-105">Batching operations tooAzure SQL Database significantly improves hello performance and scalability of your applications.</span></span> <span data-ttu-id="d17de-106">Ez a cikk első része hello rendelés toounderstand hello előnyeit, néhány minta teszteredmények összehasonlító szekvenciális és kötegelt kérelmek tooa SQL-adatbázis foglalja magában.</span><span class="sxs-lookup"><span data-stu-id="d17de-106">In order toounderstand hello benefits, hello first part of this article covers some sample test results that compare sequential and batched requests tooa SQL Database.</span></span> <span data-ttu-id="d17de-107">hello hello cikk hátralévő megjeleníti a hello technikák, forgatókönyvek és szempontok toohelp toouse, az Azure-alkalmazásokban sikeresen kötegelés.</span><span class="sxs-lookup"><span data-stu-id="d17de-107">hello remainder of hello article shows hello techniques, scenarios, and considerations toohelp you toouse batching successfully in your Azure applications.</span></span>

## <a name="why-is-batching-important-for-sql-database"></a><span data-ttu-id="d17de-108">Miért van kötegelés fontos az SQL Database?</span><span class="sxs-lookup"><span data-stu-id="d17de-108">Why is batching important for SQL Database?</span></span>
<span data-ttu-id="d17de-109">Egy jól ismert stratégia növelését a teljesítmény és méretezhetőség hívások tooa távoli szolgáltatás kötegelés.</span><span class="sxs-lookup"><span data-stu-id="d17de-109">Batching calls tooa remote service is a well-known strategy for increasing performance and scalability.</span></span> <span data-ttu-id="d17de-110">Nincs fix költségek tooany interakció, a távoli szolgáltatás, például a szerializálás, a hálózati átvitel és a deszerializálás feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="d17de-110">There are fixed processing costs tooany interactions with a remote service, such as serialization, network transfer, and deserialization.</span></span> <span data-ttu-id="d17de-111">Ezek a költségek azokat a kötegek sok külön tranzakciókat minimálisra csökkenti.</span><span class="sxs-lookup"><span data-stu-id="d17de-111">Packaging many separate transactions into a single batch minimizes these costs.</span></span>

<span data-ttu-id="d17de-112">A dokumentum azt szeretnénk tooexamine különböző SQL-adatbázis kötegelési stratégiák és forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="d17de-112">In this paper, we want tooexamine various SQL Database batching strategies and scenarios.</span></span> <span data-ttu-id="d17de-113">Bár ezek stratégiák is fontos SQL Server használó helyszíni alkalmazások esetén, több oka konzolban SQL-adatbázis kötegelés hello használata:</span><span class="sxs-lookup"><span data-stu-id="d17de-113">Although these strategies are also important for on-premises applications that use SQL Server, there are several reasons for highlighting hello use of batching for SQL Database:</span></span>

* <span data-ttu-id="d17de-114">Nincs potenciálisan nagyobb hálózati késés SQL-adatbázis eléréséhez, különösen akkor, ha az SQL-adatbázis a külső hello érnek ugyanazt a Microsoft Azure-adatközpont.</span><span class="sxs-lookup"><span data-stu-id="d17de-114">There is potentially greater network latency in accessing SQL Database, especially if you are accessing SQL Database from outside hello same Microsoft Azure datacenter.</span></span>
* <span data-ttu-id="d17de-115">SQL-adatbázis azt jelenti, hogy hello adatok hatékonyságát hello több-bérlős jellemzői hello hozzáférési réteg korrelálja toohello hello adatbázis teljes méretezhetőségét.</span><span class="sxs-lookup"><span data-stu-id="d17de-115">hello multitenant characteristics of SQL Database means that hello efficiency of hello data access layer correlates toohello overall scalability of hello database.</span></span> <span data-ttu-id="d17de-116">SQL-adatbázis az egyes bérlői felhasználók megakadályozása kell legaktívabbak adatbázis erőforrások toohello hátrányára a többi bérlő.</span><span class="sxs-lookup"><span data-stu-id="d17de-116">SQL Database must prevent any single tenant/user from monopolizing database resources toohello detriment of other tenants.</span></span> <span data-ttu-id="d17de-117">Válasz toousage, amelyek átlépik ezt az előre definiált kvótákat, az SQL-adatbázis átviteli csökkentheti vagy szabályozási kivételeket válaszolni.</span><span class="sxs-lookup"><span data-stu-id="d17de-117">In response toousage in excess of predefined quotas, SQL Database can reduce throughput or respond with throttling exceptions.</span></span> <span data-ttu-id="d17de-118">Hatékonyság, például a kötegelés, engedélyezze azt toodo munka a SQL-adatbázis a működés felső korlátjának elérése előtt.</span><span class="sxs-lookup"><span data-stu-id="d17de-118">Efficiencies, such as batching, enable you toodo more work on SQL Database before reaching these limits.</span></span> 
* <span data-ttu-id="d17de-119">Kötegelés esetében is, amelyek több adatbázist (horizontális) architektúrára való hatékony.</span><span class="sxs-lookup"><span data-stu-id="d17de-119">Batching is also effective for architectures that use multiple databases (sharding).</span></span> <span data-ttu-id="d17de-120">az adatbázis tárolóegységekhez való együttműködéshez hello hatékonyságát még mindig kulcsfontosságú tényező az általános méretezhetőséget.</span><span class="sxs-lookup"><span data-stu-id="d17de-120">hello efficiency of your interaction with each database unit is still a key factor in your overall scalability.</span></span> 

<span data-ttu-id="d17de-121">SQL-adatbázis használatának előnyei hello egyike, hogy nincs toomanage hello kiszolgálók állomás hello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="d17de-121">One of hello benefits of using SQL Database is that you don’t have toomanage hello servers that host hello database.</span></span> <span data-ttu-id="d17de-122">Azonban a felügyelt infrastruktúra is azt jelenti, hogy másképp kapcsolatos adatbázis optimalizálás toothink.</span><span class="sxs-lookup"><span data-stu-id="d17de-122">However, this managed infrastructure also means that you have toothink differently about database optimizations.</span></span> <span data-ttu-id="d17de-123">Keresse meg a tooimprove hello adatbázis hardver- vagy hálózati infrastruktúra már nem.</span><span class="sxs-lookup"><span data-stu-id="d17de-123">You can no longer look tooimprove hello database hardware or network infrastructure.</span></span> <span data-ttu-id="d17de-124">A Microsoft Azure határozza meg azokat a környezetben.</span><span class="sxs-lookup"><span data-stu-id="d17de-124">Microsoft Azure controls those environments.</span></span> <span data-ttu-id="d17de-125">hello fő területet, amely befolyásolhatja az SQL-adatbázis és az alkalmazás együttműködését.</span><span class="sxs-lookup"><span data-stu-id="d17de-125">hello main area that you can control is how your application interacts with SQL Database.</span></span> <span data-ttu-id="d17de-126">Kötegelés egyike ezek az optimalizálások.</span><span class="sxs-lookup"><span data-stu-id="d17de-126">Batching is one of these optimizations.</span></span> 

<span data-ttu-id="d17de-127">hello papír első része hello SQL-adatbázis használata a .NET-alkalmazások különböző kötegelési technikák megvizsgálja.</span><span class="sxs-lookup"><span data-stu-id="d17de-127">hello first part of hello paper examines various batching techniques for .NET applications that use SQL Database.</span></span> <span data-ttu-id="d17de-128">hello utolsó két szakaszok fedik le a kötegelési irányelvek és forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="d17de-128">hello last two sections cover batching guidelines and scenarios.</span></span>

## <a name="batching-strategies"></a><span data-ttu-id="d17de-129">Kötegelési stratégiák</span><span class="sxs-lookup"><span data-stu-id="d17de-129">Batching strategies</span></span>
### <a name="note-about-timing-results-in-this-topic"></a><span data-ttu-id="d17de-130">Megjegyzés: Ebben a témakörben időzítési eredmény</span><span class="sxs-lookup"><span data-stu-id="d17de-130">Note about timing results in this topic</span></span>
> [!NOTE]
> <span data-ttu-id="d17de-131">Eredmények nem referenciaalapokhoz képest, de célja tooshow **relatív teljesítménye**.</span><span class="sxs-lookup"><span data-stu-id="d17de-131">Results are not benchmarks but are meant tooshow **relative performance**.</span></span> <span data-ttu-id="d17de-132">Időzítés legalább 10 teszt futtatása átlagosan alapulnak.</span><span class="sxs-lookup"><span data-stu-id="d17de-132">Timings are based on an average of at least 10 test runs.</span></span> <span data-ttu-id="d17de-133">Műveletek esetében a beszúrások, a program üres táblát.</span><span class="sxs-lookup"><span data-stu-id="d17de-133">Operations are inserts into an empty table.</span></span> <span data-ttu-id="d17de-134">Ezek a tesztek mért előtti-12-es verzióra, és ezek nem feltétlenül toothroughput 12-es verziójú adatbázis hello új segítségével esetleg előforduló [szolgáltatásszintek](sql-database-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="d17de-134">These tests were measured pre-V12, and they do not necessarily correspond toothroughput that you might experience in a V12 database using hello new [service tiers](sql-database-service-tiers.md).</span></span> <span data-ttu-id="d17de-135">hello relatív előnye, hogy a technikával kötegelés hello hasonlónak kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="d17de-135">hello relative benefit of hello batching technique should be similar.</span></span>
> 
> 

### <a name="transactions"></a><span data-ttu-id="d17de-136">Tranzakciók</span><span class="sxs-lookup"><span data-stu-id="d17de-136">Transactions</span></span>
<span data-ttu-id="d17de-137">Úgy tűnik, hogy a rendellenes toobegin megvitatása tranzakciók által kötegelés áttekintése.</span><span class="sxs-lookup"><span data-stu-id="d17de-137">It seems strange toobegin a review of batching by discussing transactions.</span></span> <span data-ttu-id="d17de-138">De a tranzakciók ügyféloldali hello használata finom kiszolgálóoldali kötegelési hatással van, amely javítja a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="d17de-138">But hello use of client-side transactions has a subtle server-side batching effect that improves performance.</span></span> <span data-ttu-id="d17de-139">És a tranzakciók hozzáadása is lehetséges csak néhány sornyi kódot, így ezek biztosítanak egy gyorsan tooimprove teljesítmény egymást követő műveletek.</span><span class="sxs-lookup"><span data-stu-id="d17de-139">And transactions can be added with only a few lines of code, so they provide a fast way tooimprove performance of sequential operations.</span></span>

<span data-ttu-id="d17de-140">Vegye figyelembe a következő C#-kódban insert sorozatát tartalmazó hello és frissítési műveletek egyszerű táblán.</span><span class="sxs-lookup"><span data-stu-id="d17de-140">Consider hello following C# code that contains a sequence of insert and update operations on a simple table.</span></span>

    List<string> dbOperations = new List<string>();
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 1");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 2");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 3");
    dbOperations.Add("insert MyTable values ('new value',1)");
    dbOperations.Add("insert MyTable values ('new value',2)");
    dbOperations.Add("insert MyTable values ('new value',3)");

<span data-ttu-id="d17de-141">az ADO.NET kód egymás után következő hello ezeket a műveleteket hajt végre.</span><span class="sxs-lookup"><span data-stu-id="d17de-141">hello following ADO.NET code sequentially performs these operations.</span></span>

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();

        foreach(string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn);
            cmd.ExecuteNonQuery();                   
        }
    }

<span data-ttu-id="d17de-142">hello legjobb módja toooptimize ezt a kódot tooimplement van valamilyen ügyféloldali kötegelése hívásokat.</span><span class="sxs-lookup"><span data-stu-id="d17de-142">hello best way toooptimize this code is tooimplement some form of client-side batching of these calls.</span></span> <span data-ttu-id="d17de-143">Azonban ez a kód egy egyszerű módon tooincrease hello teljesítményének használatával egyszerűen hívások hello sorozatát szerepel egy tranzakcióban.</span><span class="sxs-lookup"><span data-stu-id="d17de-143">But there is a simple way tooincrease hello performance of this code by simply wrapping hello sequence of calls in a transaction.</span></span> <span data-ttu-id="d17de-144">Itt hello ugyanazt a kódot, amely egy tranzakció használja.</span><span class="sxs-lookup"><span data-stu-id="d17de-144">Here is hello same code that uses a transaction.</span></span>

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

<span data-ttu-id="d17de-145">Tranzakciók mindkét ezekben a példákban ténylegesen használatban van.</span><span class="sxs-lookup"><span data-stu-id="d17de-145">Transactions are actually being used in both of these examples.</span></span> <span data-ttu-id="d17de-146">Hello első példában minden egyes tekintendő, amely az implicit tranzakciókban.</span><span class="sxs-lookup"><span data-stu-id="d17de-146">In hello first example, each individual call is an implicit transaction.</span></span> <span data-ttu-id="d17de-147">Hello második példában az explicit tranzakciók becsomagolja összes hello hívások.</span><span class="sxs-lookup"><span data-stu-id="d17de-147">In hello second example, an explicit transaction wraps all of hello calls.</span></span> <span data-ttu-id="d17de-148">Hello hello dokumentációja / [írási előre tranzakciónapló](https://msdn.microsoft.com/library/ms186259.aspx), naplórekordokat esetén kiürített toohello lemez hello tranzakció véglegesítése.</span><span class="sxs-lookup"><span data-stu-id="d17de-148">Per hello documentation for hello [write-ahead transaction log](https://msdn.microsoft.com/library/ms186259.aspx), log records are flushed toohello disk when hello transaction commits.</span></span> <span data-ttu-id="d17de-149">Így több hívást együtt egy tranzakcióban, hello írási toohello tranzakciónapló tudja elhalasztani, amíg hello tranzakció.</span><span class="sxs-lookup"><span data-stu-id="d17de-149">So by including more calls in a transaction, hello write toohello transaction log can delay until hello transaction is committed.</span></span> <span data-ttu-id="d17de-150">Érvényben engedélyezi a kötegelés hello írások toohello server tranzakciós napló.</span><span class="sxs-lookup"><span data-stu-id="d17de-150">In effect, you are enabling batching for hello writes toohello server’s transaction log.</span></span>

<span data-ttu-id="d17de-151">a következő táblázat hello néhány alkalmi vizsgálati eredményeket jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="d17de-151">hello following table shows some ad-hoc testing results.</span></span> <span data-ttu-id="d17de-152">hello tesztek kerülnek végrehajtásra hello azonos szekvenciális beszúrása rendelkező és anélküli tranzakciók.</span><span class="sxs-lookup"><span data-stu-id="d17de-152">hello tests performed hello same sequential inserts with and without transactions.</span></span> <span data-ttu-id="d17de-153">További perspektívát, hello első teszteket hajtson futott távolról a Microsoft Azure-ban egy hordozható toohello adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="d17de-153">For more perspective, hello first set of tests ran remotely from a laptop toohello database in Microsoft Azure.</span></span> <span data-ttu-id="d17de-154">hello második együttesét a tesztek futtatása egy felhőalapú szolgáltatás, hogy mindkét tartózkodott hello belül azonos adatbázist a Microsoft Azure datacenter (USA nyugati régiója).</span><span class="sxs-lookup"><span data-stu-id="d17de-154">hello second set of tests ran from a cloud service and database that both resided within hello same Microsoft Azure datacenter (West US).</span></span> <span data-ttu-id="d17de-155">hello következő táblázatban hello időtartam ezredmásodpercben a szekvenciális Beszúrások rendelkező és anélküli tranzakciók.</span><span class="sxs-lookup"><span data-stu-id="d17de-155">hello following table shows hello duration in milliseconds of sequential inserts with and without transactions.</span></span>

<span data-ttu-id="d17de-156">**A helyszíni tooAzure**:</span><span class="sxs-lookup"><span data-stu-id="d17de-156">**On-Premises tooAzure**:</span></span>

| <span data-ttu-id="d17de-157">Műveletek</span><span class="sxs-lookup"><span data-stu-id="d17de-157">Operations</span></span> | <span data-ttu-id="d17de-158">Nincs tranzakció (ms)</span><span class="sxs-lookup"><span data-stu-id="d17de-158">No Transaction (ms)</span></span> | <span data-ttu-id="d17de-159">Tranzakció (ms)</span><span class="sxs-lookup"><span data-stu-id="d17de-159">Transaction (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d17de-160">1</span><span class="sxs-lookup"><span data-stu-id="d17de-160">1</span></span> |<span data-ttu-id="d17de-161">130</span><span class="sxs-lookup"><span data-stu-id="d17de-161">130</span></span> |<span data-ttu-id="d17de-162">402</span><span class="sxs-lookup"><span data-stu-id="d17de-162">402</span></span> |
| <span data-ttu-id="d17de-163">10</span><span class="sxs-lookup"><span data-stu-id="d17de-163">10</span></span> |<span data-ttu-id="d17de-164">1208</span><span class="sxs-lookup"><span data-stu-id="d17de-164">1208</span></span> |<span data-ttu-id="d17de-165">1226</span><span class="sxs-lookup"><span data-stu-id="d17de-165">1226</span></span> |
| <span data-ttu-id="d17de-166">100</span><span class="sxs-lookup"><span data-stu-id="d17de-166">100</span></span> |<span data-ttu-id="d17de-167">12662</span><span class="sxs-lookup"><span data-stu-id="d17de-167">12662</span></span> |<span data-ttu-id="d17de-168">10395</span><span class="sxs-lookup"><span data-stu-id="d17de-168">10395</span></span> |
| <span data-ttu-id="d17de-169">1000</span><span class="sxs-lookup"><span data-stu-id="d17de-169">1000</span></span> |<span data-ttu-id="d17de-170">128852</span><span class="sxs-lookup"><span data-stu-id="d17de-170">128852</span></span> |<span data-ttu-id="d17de-171">102917</span><span class="sxs-lookup"><span data-stu-id="d17de-171">102917</span></span> |

<span data-ttu-id="d17de-172">**Az Azure tooAzure (ugyanabban az adatközpontban)**:</span><span class="sxs-lookup"><span data-stu-id="d17de-172">**Azure tooAzure (same datacenter)**:</span></span>

| <span data-ttu-id="d17de-173">Műveletek</span><span class="sxs-lookup"><span data-stu-id="d17de-173">Operations</span></span> | <span data-ttu-id="d17de-174">Nincs tranzakció (ms)</span><span class="sxs-lookup"><span data-stu-id="d17de-174">No Transaction (ms)</span></span> | <span data-ttu-id="d17de-175">Tranzakció (ms)</span><span class="sxs-lookup"><span data-stu-id="d17de-175">Transaction (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d17de-176">1</span><span class="sxs-lookup"><span data-stu-id="d17de-176">1</span></span> |<span data-ttu-id="d17de-177">21</span><span class="sxs-lookup"><span data-stu-id="d17de-177">21</span></span> |<span data-ttu-id="d17de-178">26</span><span class="sxs-lookup"><span data-stu-id="d17de-178">26</span></span> |
| <span data-ttu-id="d17de-179">10</span><span class="sxs-lookup"><span data-stu-id="d17de-179">10</span></span> |<span data-ttu-id="d17de-180">220</span><span class="sxs-lookup"><span data-stu-id="d17de-180">220</span></span> |<span data-ttu-id="d17de-181">56</span><span class="sxs-lookup"><span data-stu-id="d17de-181">56</span></span> |
| <span data-ttu-id="d17de-182">100</span><span class="sxs-lookup"><span data-stu-id="d17de-182">100</span></span> |<span data-ttu-id="d17de-183">2145</span><span class="sxs-lookup"><span data-stu-id="d17de-183">2145</span></span> |<span data-ttu-id="d17de-184">341</span><span class="sxs-lookup"><span data-stu-id="d17de-184">341</span></span> |
| <span data-ttu-id="d17de-185">1000</span><span class="sxs-lookup"><span data-stu-id="d17de-185">1000</span></span> |<span data-ttu-id="d17de-186">21479</span><span class="sxs-lookup"><span data-stu-id="d17de-186">21479</span></span> |<span data-ttu-id="d17de-187">2756</span><span class="sxs-lookup"><span data-stu-id="d17de-187">2756</span></span> |

> [!NOTE]
> <span data-ttu-id="d17de-188">A eredményei nem referenciaalapokhoz képest.</span><span class="sxs-lookup"><span data-stu-id="d17de-188">Results are not benchmarks.</span></span> <span data-ttu-id="d17de-189">Lásd: hello [időzítési eredményezi, hogy ez a témakör Megjegyzés](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="d17de-189">See hello [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="d17de-190">Hello előző teszt eredményei alapján teljesítmény ténylegesen csökkenti alkalmazásburkoló egyetlen műveletben szerepel egy tranzakcióban.</span><span class="sxs-lookup"><span data-stu-id="d17de-190">Based on hello previous test results, wrapping a single operation in a transaction actually decreases performance.</span></span> <span data-ttu-id="d17de-191">De növelésével műveletek egy tranzakción belül hello száma, hello teljesítményjavulást több lesz megjelölve.</span><span class="sxs-lookup"><span data-stu-id="d17de-191">But as you increase hello number of operations within a single transaction, hello performance improvement becomes more marked.</span></span> <span data-ttu-id="d17de-192">hello teljesítménybeli különbség az akkor is jobban észlelhető, ha minden műveletnél hello Microsoft Azure adatközponton belül történik.</span><span class="sxs-lookup"><span data-stu-id="d17de-192">hello performance difference is also more noticeable when all operations occur within hello Microsoft Azure datacenter.</span></span> <span data-ttu-id="d17de-193">hello SQL-adatbázis használata a Microsoft Azure datacenter külső hello nagyobb késéseket overshadows hello jobb teljesítménye tranzakciók használatával.</span><span class="sxs-lookup"><span data-stu-id="d17de-193">hello increased latency of using SQL Database from outside hello Microsoft Azure datacenter overshadows hello performance gain of using transactions.</span></span>

<span data-ttu-id="d17de-194">Noha a tranzakciók hello használata növelheti a teljesítményt, továbbra is túl[tekintse át az ajánlott eljárások az tranzakciók és kapcsolatok](https://msdn.microsoft.com/library/ms187484.aspx).</span><span class="sxs-lookup"><span data-stu-id="d17de-194">Although hello use of transactions can increase performance, continue too[observe best practices for transactions and connections](https://msdn.microsoft.com/library/ms187484.aspx).</span></span> <span data-ttu-id="d17de-195">Tartsa hello tranzakció és a lehető legrövidebb lehetséges és Bezárás hello adatbázis-kapcsolat hello munkahelyi befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="d17de-195">Keep hello transaction as short as possible, and close hello database connection after hello work completes.</span></span> <span data-ttu-id="d17de-196">hello utasítás használatával az előző példában hello biztosítja, hogy az hello kapcsolat le van zárva hello későbbi kódblokk befejezéséről.</span><span class="sxs-lookup"><span data-stu-id="d17de-196">hello using statement in hello previous example assures that hello connection is closed when hello subsequent code block completes.</span></span>

<span data-ttu-id="d17de-197">hello előző példa bemutatja, hogy adhat hozzá egy helyi tranzakció tooany ADO.NET kódot két sort.</span><span class="sxs-lookup"><span data-stu-id="d17de-197">hello previous example demonstrates that you can add a local transaction tooany ADO.NET code with two lines.</span></span> <span data-ttu-id="d17de-198">Tranzakciók tooimprove hello teljesítményének kódot, amely lehetővé teszi a szekvenciális beszúrási, frissítési és törlési műveletek gyors lehetőséget kínál.</span><span class="sxs-lookup"><span data-stu-id="d17de-198">Transactions offer a quick way tooimprove hello performance of code that makes sequential insert, update, and delete operations.</span></span> <span data-ttu-id="d17de-199">Azonban a hello leggyorsabb teljesítmény érdekében érdemes megfontolni hello kód további tootake előnye, hogy az ügyféloldali kötegelés, például a táblázat értékű paramétereket.</span><span class="sxs-lookup"><span data-stu-id="d17de-199">However, for hello fastest performance, consider changing hello code further tootake advantage of client-side batching, such as table-valued parameters.</span></span>

<span data-ttu-id="d17de-200">Az ADO.NET tranzakciókkal kapcsolatos további információkért lásd: [ADO.NET helyi tranzakciók](https://docs.microsoft.com/dotnet/framework/data/adonet/local-transactions).</span><span class="sxs-lookup"><span data-stu-id="d17de-200">For more information about transactions in ADO.NET, see [Local Transactions in ADO.NET](https://docs.microsoft.com/dotnet/framework/data/adonet/local-transactions).</span></span>

### <a name="table-valued-parameters"></a><span data-ttu-id="d17de-201">tábla értékű paraméter</span><span class="sxs-lookup"><span data-stu-id="d17de-201">Table-valued parameters</span></span>
<span data-ttu-id="d17de-202">Tábla értékű paraméter paraméterekkel a Transact-SQL-utasítások, tárolt eljárások és függvények, felhasználó által definiált táblatípusokban támogatja.</span><span class="sxs-lookup"><span data-stu-id="d17de-202">Table-valued parameters support user-defined table types as parameters in Transact-SQL statements, stored procedures, and functions.</span></span> <span data-ttu-id="d17de-203">Ez az ügyféloldali kötegelési módszer lehetővé teszi, hogy toosend több sornyi adatot hello tábla értékű paraméter belül.</span><span class="sxs-lookup"><span data-stu-id="d17de-203">This client-side batching technique allows you toosend multiple rows of data within hello table-valued parameter.</span></span> <span data-ttu-id="d17de-204">toouse tábla értékű paraméter, először egy táblatípus határozza meg.</span><span class="sxs-lookup"><span data-stu-id="d17de-204">toouse table-valued parameters, first define a table type.</span></span> <span data-ttu-id="d17de-205">a következő Transact-SQL-utasítás hello nevű tábla típus létrehozása **MyTableType**.</span><span class="sxs-lookup"><span data-stu-id="d17de-205">hello following Transact-SQL statement creates a table type named **MyTableType**.</span></span>

    CREATE TYPE MyTableType AS TABLE 
    ( mytext TEXT,
      num INT );


<span data-ttu-id="d17de-206">A kódban, hozzon létre egy **DataTable** hello a pontos azonos nevét és típusát hello táblatípus.</span><span class="sxs-lookup"><span data-stu-id="d17de-206">In code, you create a **DataTable** with hello exact same names and types of hello table type.</span></span> <span data-ttu-id="d17de-207">Ez átadni **DataTable** egy paraméterben, szöveges lekérdezés vagy tárolt eljárás hívása.</span><span class="sxs-lookup"><span data-stu-id="d17de-207">Pass this **DataTable** in a parameter in a text query or stored procedure call.</span></span> <span data-ttu-id="d17de-208">hello következő példa bemutatja, ezzel a módszerrel:</span><span class="sxs-lookup"><span data-stu-id="d17de-208">hello following example shows this technique:</span></span>

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();

        DataTable table = new DataTable();
        // Add columns and rows. hello following is a simple example.
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

<span data-ttu-id="d17de-209">Hello előző példában hello **SqlCommand** objektum egy tábla értékű paraméter a sor beszúrása  **@TestTvp** .</span><span class="sxs-lookup"><span data-stu-id="d17de-209">In hello previous example, hello **SqlCommand** object inserts rows from a table-valued parameter, **@TestTvp**.</span></span> <span data-ttu-id="d17de-210">korábban létrehozott hello **DataTable** objektum hozzá van rendelve a hello toothis paraméterrel **SqlCommand.Parameters.Add** metódust.</span><span class="sxs-lookup"><span data-stu-id="d17de-210">hello previously created **DataTable** object is assigned toothis parameter with hello **SqlCommand.Parameters.Add** method.</span></span> <span data-ttu-id="d17de-211">Kötegelési hello beszúrása egy hívás jelentősen növeli hello teljesítmény szekvenciális Beszúrások keresztül.</span><span class="sxs-lookup"><span data-stu-id="d17de-211">Batching hello inserts in one call significantly increases hello performance over sequential inserts.</span></span>

<span data-ttu-id="d17de-212">tooimprove hello előző példa folytatásaként, használja a tárolt eljárás egy szöveges parancs helyett.</span><span class="sxs-lookup"><span data-stu-id="d17de-212">tooimprove hello previous example further, use a stored procedure instead of a text-based command.</span></span> <span data-ttu-id="d17de-213">a következő Transact-SQL-parancs hello hoz létre, amely hello tárolt eljárás **SimpleTestTableType** tábla értékű paraméter.</span><span class="sxs-lookup"><span data-stu-id="d17de-213">hello following Transact-SQL command creates a stored procedure that takes hello **SimpleTestTableType** table-valued parameter.</span></span>

    CREATE PROCEDURE [dbo].[sp_InsertRows] 
    @TestTvp as MyTableType READONLY
    AS
    BEGIN
    INSERT INTO MyTable(mytext, num) 
    SELECT mytext, num FROM @TestTvp
    END
    GO

<span data-ttu-id="d17de-214">Módosítsa a hello **SqlCommand** hello előző kód példa toohello következő deklaráció objektum.</span><span class="sxs-lookup"><span data-stu-id="d17de-214">Then change hello **SqlCommand** object declaration in hello previous code example toohello following.</span></span>

    SqlCommand cmd = new SqlCommand("sp_InsertRows", connection);
    cmd.CommandType = CommandType.StoredProcedure;

<span data-ttu-id="d17de-215">A legtöbb esetben a tábla értékű paraméter rendelkezik egyenértékű vagy jobb teljesítményt biztosít, mint más kötegelési módszerek.</span><span class="sxs-lookup"><span data-stu-id="d17de-215">In most cases, table-valued parameters have equivalent or better performance than other batching techniques.</span></span> <span data-ttu-id="d17de-216">Tábla értékű paraméterek gyakran érdemes, mivel rugalmasabb, mint más beállítások.</span><span class="sxs-lookup"><span data-stu-id="d17de-216">Table-valued parameters are often preferable, because they are more flexible than other options.</span></span> <span data-ttu-id="d17de-217">Egyéb módszerek, például SQL tömeges másolás, például csak új sorok beszúrását hello lehetővé teszik.</span><span class="sxs-lookup"><span data-stu-id="d17de-217">For example, other techniques, such as SQL bulk copy, only permit hello insertion of new rows.</span></span> <span data-ttu-id="d17de-218">De tábla értékű paraméter segítségével is logika hello tárolt eljárás toodetermine mely sorai frissítések érhetők el, és amelyek beszúrása.</span><span class="sxs-lookup"><span data-stu-id="d17de-218">But with table-valued parameters, you can use logic in hello stored procedure toodetermine which rows are updates and which are inserts.</span></span> <span data-ttu-id="d17de-219">hello táblatípus is módosított toocontain egy "Művelet" oszlopot, amely azt jelzi, hogy hello megadott sort kell beszúrni, frissíteni, vagy törölve.</span><span class="sxs-lookup"><span data-stu-id="d17de-219">hello table type can also be modified toocontain an “Operation” column that indicates whether hello specified row should be inserted, updated, or deleted.</span></span>

<span data-ttu-id="d17de-220">a következő táblázat hello alkalmi teszteredmények hello használható a tábla értékű paraméterek mutatja ezredmásodpercben.</span><span class="sxs-lookup"><span data-stu-id="d17de-220">hello following table shows ad-hoc test results for hello use of table-valued parameters in milliseconds.</span></span>

| <span data-ttu-id="d17de-221">Műveletek</span><span class="sxs-lookup"><span data-stu-id="d17de-221">Operations</span></span> | <span data-ttu-id="d17de-222">A helyszíni tooAzure (ms)</span><span class="sxs-lookup"><span data-stu-id="d17de-222">On-Premises tooAzure (ms)</span></span> | <span data-ttu-id="d17de-223">Az Azure ugyanabban az adatközpontban (ms)</span><span class="sxs-lookup"><span data-stu-id="d17de-223">Azure same datacenter (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d17de-224">1</span><span class="sxs-lookup"><span data-stu-id="d17de-224">1</span></span> |<span data-ttu-id="d17de-225">124</span><span class="sxs-lookup"><span data-stu-id="d17de-225">124</span></span> |<span data-ttu-id="d17de-226">32</span><span class="sxs-lookup"><span data-stu-id="d17de-226">32</span></span> |
| <span data-ttu-id="d17de-227">10</span><span class="sxs-lookup"><span data-stu-id="d17de-227">10</span></span> |<span data-ttu-id="d17de-228">131</span><span class="sxs-lookup"><span data-stu-id="d17de-228">131</span></span> |<span data-ttu-id="d17de-229">25</span><span class="sxs-lookup"><span data-stu-id="d17de-229">25</span></span> |
| <span data-ttu-id="d17de-230">100</span><span class="sxs-lookup"><span data-stu-id="d17de-230">100</span></span> |<span data-ttu-id="d17de-231">338</span><span class="sxs-lookup"><span data-stu-id="d17de-231">338</span></span> |<span data-ttu-id="d17de-232">51</span><span class="sxs-lookup"><span data-stu-id="d17de-232">51</span></span> |
| <span data-ttu-id="d17de-233">1000</span><span class="sxs-lookup"><span data-stu-id="d17de-233">1000</span></span> |<span data-ttu-id="d17de-234">2615</span><span class="sxs-lookup"><span data-stu-id="d17de-234">2615</span></span> |<span data-ttu-id="d17de-235">382</span><span class="sxs-lookup"><span data-stu-id="d17de-235">382</span></span> |
| <span data-ttu-id="d17de-236">10000</span><span class="sxs-lookup"><span data-stu-id="d17de-236">10000</span></span> |<span data-ttu-id="d17de-237">23830</span><span class="sxs-lookup"><span data-stu-id="d17de-237">23830</span></span> |<span data-ttu-id="d17de-238">3586</span><span class="sxs-lookup"><span data-stu-id="d17de-238">3586</span></span> |

> [!NOTE]
> <span data-ttu-id="d17de-239">A eredményei nem referenciaalapokhoz képest.</span><span class="sxs-lookup"><span data-stu-id="d17de-239">Results are not benchmarks.</span></span> <span data-ttu-id="d17de-240">Lásd: hello [időzítési eredményezi, hogy ez a témakör Megjegyzés](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="d17de-240">See hello [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="d17de-241">hello jobb teljesítménye a kötegelés azonnal kétségtelenül.</span><span class="sxs-lookup"><span data-stu-id="d17de-241">hello performance gain from batching is immediately apparent.</span></span> <span data-ttu-id="d17de-242">Hello előző szekvenciális teszt 1000 műveletek másodpercet vett igénybe 129 külső hello datacenter és hello adatközponton belül a 21 másodperc.</span><span class="sxs-lookup"><span data-stu-id="d17de-242">In hello previous sequential test, 1000 operations took 129 seconds outside hello datacenter and 21 seconds from within hello datacenter.</span></span> <span data-ttu-id="d17de-243">De a tábla értékű paraméter 1000-műveletek egy csak 2.6-os másodperc hello adatközponton kívülről és idõtartamtól hello adatközponton belül.</span><span class="sxs-lookup"><span data-stu-id="d17de-243">But with table-valued parameters, 1000 operations take only 2.6 seconds outside hello datacenter and 0.4 seconds within hello datacenter.</span></span>

<span data-ttu-id="d17de-244">További információ a tábla értékű paraméter: [Table-Valued paraméterek](https://msdn.microsoft.com/library/bb510489.aspx).</span><span class="sxs-lookup"><span data-stu-id="d17de-244">For more information on table-valued parameters, see [Table-Valued Parameters](https://msdn.microsoft.com/library/bb510489.aspx).</span></span>

### <a name="sql-bulk-copy"></a><span data-ttu-id="d17de-245">SQL tömeges másolási</span><span class="sxs-lookup"><span data-stu-id="d17de-245">SQL bulk copy</span></span>
<span data-ttu-id="d17de-246">SQL tömeges másolási egy másik módja tooinsert nagy mennyiségű adat egy cél adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="d17de-246">SQL bulk copy is another way tooinsert large amounts of data into a target database.</span></span> <span data-ttu-id="d17de-247">.NET-alkalmazásokban használható hello **SqlBulkCopy** osztály tooperform tömeges beszúrási műveletek.</span><span class="sxs-lookup"><span data-stu-id="d17de-247">.NET applications can use hello **SqlBulkCopy** class tooperform bulk insert operations.</span></span> <span data-ttu-id="d17de-248">**SqlBulkCopy** hasonló függvény toohello parancssori eszköz, a **Bcp.exe**, vagy a Transact-SQL-utasítás hello **TÖMEGES Beszúrás**.</span><span class="sxs-lookup"><span data-stu-id="d17de-248">**SqlBulkCopy** is similar in function toohello command-line tool, **Bcp.exe**, or hello Transact-SQL statement, **BULK INSERT**.</span></span> <span data-ttu-id="d17de-249">hello következő kódrészlet példa bemutatja, hogyan toobulk másolási hello hello forrás sort **DataTable**, tábla, az SQL Server, MyTable toohello céltábla.</span><span class="sxs-lookup"><span data-stu-id="d17de-249">hello following code example shows how toobulk copy hello rows in hello source **DataTable**, table, toohello destination table in SQL Server, MyTable.</span></span>

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

<span data-ttu-id="d17de-250">Néhány esetben, ha tömeges másolás előnyben részesített tábla értékű paraméter felett van.</span><span class="sxs-lookup"><span data-stu-id="d17de-250">There are some cases where bulk copy is preferred over table-valued parameters.</span></span> <span data-ttu-id="d17de-251">Tekintse meg a tábla értékű paramétert, és TÖMEGES beszúrási műveletek hello témakör hello összehasonlító táblázatában [Table-Valued paraméterek](https://msdn.microsoft.com/library/bb510489.aspx).</span><span class="sxs-lookup"><span data-stu-id="d17de-251">See hello comparison table of Table-Valued parameters versus BULK INSERT operations in hello topic [Table-Valued Parameters](https://msdn.microsoft.com/library/bb510489.aspx).</span></span>

<span data-ttu-id="d17de-252">hello következő alkalmi vizsgálati eredmények megjelenítése hello teljesítményét a kötegelés **SqlBulkCopy** ezredmásodpercben.</span><span class="sxs-lookup"><span data-stu-id="d17de-252">hello following ad-hoc test results show hello performance of batching with **SqlBulkCopy** in milliseconds.</span></span>

| <span data-ttu-id="d17de-253">Műveletek</span><span class="sxs-lookup"><span data-stu-id="d17de-253">Operations</span></span> | <span data-ttu-id="d17de-254">A helyszíni tooAzure (ms)</span><span class="sxs-lookup"><span data-stu-id="d17de-254">On-Premises tooAzure (ms)</span></span> | <span data-ttu-id="d17de-255">Az Azure ugyanabban az adatközpontban (ms)</span><span class="sxs-lookup"><span data-stu-id="d17de-255">Azure same datacenter (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d17de-256">1</span><span class="sxs-lookup"><span data-stu-id="d17de-256">1</span></span> |<span data-ttu-id="d17de-257">433</span><span class="sxs-lookup"><span data-stu-id="d17de-257">433</span></span> |<span data-ttu-id="d17de-258">57</span><span class="sxs-lookup"><span data-stu-id="d17de-258">57</span></span> |
| <span data-ttu-id="d17de-259">10</span><span class="sxs-lookup"><span data-stu-id="d17de-259">10</span></span> |<span data-ttu-id="d17de-260">441</span><span class="sxs-lookup"><span data-stu-id="d17de-260">441</span></span> |<span data-ttu-id="d17de-261">32</span><span class="sxs-lookup"><span data-stu-id="d17de-261">32</span></span> |
| <span data-ttu-id="d17de-262">100</span><span class="sxs-lookup"><span data-stu-id="d17de-262">100</span></span> |<span data-ttu-id="d17de-263">636</span><span class="sxs-lookup"><span data-stu-id="d17de-263">636</span></span> |<span data-ttu-id="d17de-264">53</span><span class="sxs-lookup"><span data-stu-id="d17de-264">53</span></span> |
| <span data-ttu-id="d17de-265">1000</span><span class="sxs-lookup"><span data-stu-id="d17de-265">1000</span></span> |<span data-ttu-id="d17de-266">2535</span><span class="sxs-lookup"><span data-stu-id="d17de-266">2535</span></span> |<span data-ttu-id="d17de-267">341</span><span class="sxs-lookup"><span data-stu-id="d17de-267">341</span></span> |
| <span data-ttu-id="d17de-268">10000</span><span class="sxs-lookup"><span data-stu-id="d17de-268">10000</span></span> |<span data-ttu-id="d17de-269">21605</span><span class="sxs-lookup"><span data-stu-id="d17de-269">21605</span></span> |<span data-ttu-id="d17de-270">2737</span><span class="sxs-lookup"><span data-stu-id="d17de-270">2737</span></span> |

> [!NOTE]
> <span data-ttu-id="d17de-271">A eredményei nem referenciaalapokhoz képest.</span><span class="sxs-lookup"><span data-stu-id="d17de-271">Results are not benchmarks.</span></span> <span data-ttu-id="d17de-272">Lásd: hello [időzítési eredményezi, hogy ez a témakör Megjegyzés](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="d17de-272">See hello [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="d17de-273">A Köteg mérete kisebb, hello használata tábla értékű paraméter outperformed hello **SqlBulkCopy** osztály.</span><span class="sxs-lookup"><span data-stu-id="d17de-273">In smaller batch sizes, hello use table-valued parameters outperformed hello **SqlBulkCopy** class.</span></span> <span data-ttu-id="d17de-274">Azonban **SqlBulkCopy** az 1000 és 10 000 sorok hello tesztek során végrehajtott 12-31 % gyorsabb, mint a tábla értékű paraméter.</span><span class="sxs-lookup"><span data-stu-id="d17de-274">However, **SqlBulkCopy** performed 12-31% faster than table-valued parameters for hello tests of 1,000 and 10,000 rows.</span></span> <span data-ttu-id="d17de-275">Tábla értékű paraméterek, például **SqlBulkCopy** van a kötegelt Beszúrás jó választás, különösen akkor, ha képest műveletek nem kötegelni toohello teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="d17de-275">Like table-valued parameters, **SqlBulkCopy** is a good option for batched inserts, especially when compared toohello performance of non-batched operations.</span></span>

<span data-ttu-id="d17de-276">A tömeges másolás az ADO.NET további információkért lásd: [az SQL Server tömeges másolási műveletek](https://msdn.microsoft.com/library/7ek5da1a.aspx).</span><span class="sxs-lookup"><span data-stu-id="d17de-276">For more information on bulk copy in ADO.NET, see [Bulk Copy Operations in SQL Server](https://msdn.microsoft.com/library/7ek5da1a.aspx).</span></span>

### <a name="multiple-row-parameterized-insert-statements"></a><span data-ttu-id="d17de-277">Több soron kívüli Beszúrás paraméteres utasításokat</span><span class="sxs-lookup"><span data-stu-id="d17de-277">Multiple-row Parameterized INSERT statements</span></span>
<span data-ttu-id="d17de-278">Egy kis kötegek esetben egy nagy tooconstruct paraméteres INSERT utasításban, amely több sor beszúrása.</span><span class="sxs-lookup"><span data-stu-id="d17de-278">One alternative for small batches is tooconstruct a large parameterized INSERT statement that inserts multiple rows.</span></span> <span data-ttu-id="d17de-279">hello a következő példakód azt mutatja be, ezzel a módszerrel.</span><span class="sxs-lookup"><span data-stu-id="d17de-279">hello following code example demonstrates this technique.</span></span>

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


<span data-ttu-id="d17de-280">Ez a példa arra szolgál, tooshow hello alapvető fogalma.</span><span class="sxs-lookup"><span data-stu-id="d17de-280">This example is meant tooshow hello basic concept.</span></span> <span data-ttu-id="d17de-281">A modell forgatókönyv volna ismétlése szükséges hello entitások tooconstruct hello lekérdezési karakterlánc és hello parancs paraméterei egyidejűleg.</span><span class="sxs-lookup"><span data-stu-id="d17de-281">A more realistic scenario would loop through hello required entities tooconstruct hello query string and hello command parameters simultaneously.</span></span> <span data-ttu-id="d17de-282">Legfeljebb 2100 lekérdezési paraméterek, ez korlátozza hello sorok maximális számát, amelyek az ilyen módon dolgozhatók tooa összesen.</span><span class="sxs-lookup"><span data-stu-id="d17de-282">You are limited tooa total of 2100 query parameters, so this limits hello total number of rows that can be processed in this manner.</span></span>

<span data-ttu-id="d17de-283">a következő alkalmi eredmények megjelenítése hello teljesítményének tesztelése az ilyen típusú insert utasítás ezredmásodpercben hello.</span><span class="sxs-lookup"><span data-stu-id="d17de-283">hello following ad-hoc test results show hello performance of this type of insert statement in milliseconds.</span></span>

| <span data-ttu-id="d17de-284">Műveletek</span><span class="sxs-lookup"><span data-stu-id="d17de-284">Operations</span></span> | <span data-ttu-id="d17de-285">Tábla értékű paraméter (ms)</span><span class="sxs-lookup"><span data-stu-id="d17de-285">Table-valued parameters (ms)</span></span> | <span data-ttu-id="d17de-286">Utasításból INSERT (ms)</span><span class="sxs-lookup"><span data-stu-id="d17de-286">Single-statement INSERT (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d17de-287">1</span><span class="sxs-lookup"><span data-stu-id="d17de-287">1</span></span> |<span data-ttu-id="d17de-288">32</span><span class="sxs-lookup"><span data-stu-id="d17de-288">32</span></span> |<span data-ttu-id="d17de-289">20</span><span class="sxs-lookup"><span data-stu-id="d17de-289">20</span></span> |
| <span data-ttu-id="d17de-290">10</span><span class="sxs-lookup"><span data-stu-id="d17de-290">10</span></span> |<span data-ttu-id="d17de-291">30</span><span class="sxs-lookup"><span data-stu-id="d17de-291">30</span></span> |<span data-ttu-id="d17de-292">25</span><span class="sxs-lookup"><span data-stu-id="d17de-292">25</span></span> |
| <span data-ttu-id="d17de-293">100</span><span class="sxs-lookup"><span data-stu-id="d17de-293">100</span></span> |<span data-ttu-id="d17de-294">33</span><span class="sxs-lookup"><span data-stu-id="d17de-294">33</span></span> |<span data-ttu-id="d17de-295">51</span><span class="sxs-lookup"><span data-stu-id="d17de-295">51</span></span> |

> [!NOTE]
> <span data-ttu-id="d17de-296">A eredményei nem referenciaalapokhoz képest.</span><span class="sxs-lookup"><span data-stu-id="d17de-296">Results are not benchmarks.</span></span> <span data-ttu-id="d17de-297">Lásd: hello [időzítési eredményezi, hogy ez a témakör Megjegyzés](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="d17de-297">See hello [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="d17de-298">Ezt a módszert használja, amelyek 100-nál kevesebb sort kötegek némileg gyorsabb lehet.</span><span class="sxs-lookup"><span data-stu-id="d17de-298">This approach can be slightly faster for batches that are less than 100 rows.</span></span> <span data-ttu-id="d17de-299">Bár hello javítása kis, ez a módszer akkor lehet, hogy kiválóan működjenek az adott alkalmazás helyzetnek lehetősége.</span><span class="sxs-lookup"><span data-stu-id="d17de-299">Although hello improvement is small, this technique is another option that might work well in your specific application scenario.</span></span>

### <a name="dataadapter"></a><span data-ttu-id="d17de-300">DataAdapter</span><span class="sxs-lookup"><span data-stu-id="d17de-300">DataAdapter</span></span>
<span data-ttu-id="d17de-301">Hello **DataAdapter** osztály toomodify lehetővé teszi egy **DataSet** objektumot, és küldje el a INSERT, UPDATE és DELETE műveletek hello változik.</span><span class="sxs-lookup"><span data-stu-id="d17de-301">hello **DataAdapter** class allows you toomodify a **DataSet** object and then submit hello changes as INSERT, UPDATE, and DELETE operations.</span></span> <span data-ttu-id="d17de-302">Hello használata **DataAdapter** ezen a módon kikapcsolja, fontos, hívások elválasztó toonote legyenek készülve az egyes különálló műveletet.</span><span class="sxs-lookup"><span data-stu-id="d17de-302">If you are using hello **DataAdapter** in this manner, it is important toonote that separate calls are made for each distinct operation.</span></span> <span data-ttu-id="d17de-303">tooimprove teljesítmény érdekében használjon hello **UpdateBatchSize** egyszerre kell lehet kötegelni műveletek tulajdonság toohello száma.</span><span class="sxs-lookup"><span data-stu-id="d17de-303">tooimprove performance, use hello **UpdateBatchSize** property toohello number of operations that should be batched at a time.</span></span> <span data-ttu-id="d17de-304">További információkért lásd: [végrehajtása kötegelt műveletek használatával DataAdapters](https://msdn.microsoft.com/library/aadf8fk2.aspx).</span><span class="sxs-lookup"><span data-stu-id="d17de-304">For more information, see [Performing Batch Operations Using DataAdapters](https://msdn.microsoft.com/library/aadf8fk2.aspx).</span></span>

### <a name="entity-framework"></a><span data-ttu-id="d17de-305">Entitás-keretrendszer</span><span class="sxs-lookup"><span data-stu-id="d17de-305">Entity framework</span></span>
<span data-ttu-id="d17de-306">Entitás-keretrendszer jelenleg nem támogatja kötegelés.</span><span class="sxs-lookup"><span data-stu-id="d17de-306">Entity Framework does not currently support batching.</span></span> <span data-ttu-id="d17de-307">Hello Közösségből származó különböző fejlesztők próbált toodemonstrate lehetséges megoldások, például a felülbírálás hello **a SaveChanges metódus** metódust.</span><span class="sxs-lookup"><span data-stu-id="d17de-307">Different developers in hello community have attempted toodemonstrate workarounds, such as override hello **SaveChanges** method.</span></span> <span data-ttu-id="d17de-308">De hello megoldások általában összetett és testre szabott toohello alkalmazás és az adatokat az adatmodellbe.</span><span class="sxs-lookup"><span data-stu-id="d17de-308">But hello solutions are typically complex and customized toohello application and data model.</span></span> <span data-ttu-id="d17de-309">hello Entity Framework codeplex-projekt jelenleg is rendelkezik az ismertető a szolgáltatás kérésre.</span><span class="sxs-lookup"><span data-stu-id="d17de-309">hello Entity Framework codeplex project currently has a discussion page on this feature request.</span></span> <span data-ttu-id="d17de-310">tooview ismertető, lásd: [tervezési értekezlet megjegyzések - 2012 augusztus 2](http://entityframework.codeplex.com/wikipage?title=Design%20Meeting%20Notes%20-%20August%202%2c%202012).</span><span class="sxs-lookup"><span data-stu-id="d17de-310">tooview this discussion, see [Design Meeting Notes - August 2, 2012](http://entityframework.codeplex.com/wikipage?title=Design%20Meeting%20Notes%20-%20August%202%2c%202012).</span></span>

### <a name="xml"></a><span data-ttu-id="d17de-311">XML</span><span class="sxs-lookup"><span data-stu-id="d17de-311">XML</span></span>
<span data-ttu-id="d17de-312">A teljesség kedvéért azt látja, hogy az XML kötegelési stratégiát, fontos tootalk.</span><span class="sxs-lookup"><span data-stu-id="d17de-312">For completeness, we feel that it is important tootalk about XML as a batching strategy.</span></span> <span data-ttu-id="d17de-313">Az XML-kód hello használata azonban más módszerekkel nem előnyöket és számos hátránya rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="d17de-313">However, hello use of XML has no advantages over other methods and several disadvantages.</span></span> <span data-ttu-id="d17de-314">hello megközelítés hasonló tootable értékű paraméterek, de egy XML-fájl vagy karakterlánc átadása tooa tárolt eljárás nem felhasználói tábla.</span><span class="sxs-lookup"><span data-stu-id="d17de-314">hello approach is similar tootable-valued parameters, but an XML file or string is passed tooa stored procedure instead of a user-defined table.</span></span> <span data-ttu-id="d17de-315">hello tárolt eljárás elemez hello parancsok hello tárolt eljárás.</span><span class="sxs-lookup"><span data-stu-id="d17de-315">hello stored procedure parses hello commands in hello stored procedure.</span></span>

<span data-ttu-id="d17de-316">Van több hátrányai toothis módszert:</span><span class="sxs-lookup"><span data-stu-id="d17de-316">There are several disadvantages toothis approach:</span></span>

* <span data-ttu-id="d17de-317">Az XML működő nehézkes lehet, és hibalehetőségeket rejt magában hiba.</span><span class="sxs-lookup"><span data-stu-id="d17de-317">Working with XML can be cumbersome and error prone.</span></span>
* <span data-ttu-id="d17de-318">Elemzési hello XML hello adatbázison processzorigényes lehet.</span><span class="sxs-lookup"><span data-stu-id="d17de-318">Parsing hello XML on hello database can be CPU-intensive.</span></span>
* <span data-ttu-id="d17de-319">A legtöbb esetben ez a módszer lassabb, mint a tábla értékű paraméter.</span><span class="sxs-lookup"><span data-stu-id="d17de-319">In most cases, this method is slower than table-valued parameters.</span></span>

<span data-ttu-id="d17de-320">Ezen okok miatt hello XML kötegelt lekérdezések használata nem ajánlott.</span><span class="sxs-lookup"><span data-stu-id="d17de-320">For these reasons, hello use of XML for batch queries is not recommended.</span></span>

## <a name="batching-considerations"></a><span data-ttu-id="d17de-321">Kötegelési kapcsolatos szempontok</span><span class="sxs-lookup"><span data-stu-id="d17de-321">Batching considerations</span></span>
<span data-ttu-id="d17de-322">a következő szakaszok hello további útmutatás nyújtása a hello használata az SQL-adatbázis alkalmazások kötegelés.</span><span class="sxs-lookup"><span data-stu-id="d17de-322">hello following sections provide more guidance for hello use of batching in SQL Database applications.</span></span>

### <a name="tradeoffs"></a><span data-ttu-id="d17de-323">Mellékhatásokkal</span><span class="sxs-lookup"><span data-stu-id="d17de-323">Tradeoffs</span></span>
<span data-ttu-id="d17de-324">Attól függően, hogy az architektúrák kötegelés magába foglaló a teljesítményt és rugalmasságot közötti kompromisszumot.</span><span class="sxs-lookup"><span data-stu-id="d17de-324">Depending on your architecture, batching can involve a tradeoff between performance and resiliency.</span></span> <span data-ttu-id="d17de-325">Vegye figyelembe például hello forgatókönyv, ahol a szerepkör váratlanul leáll.</span><span class="sxs-lookup"><span data-stu-id="d17de-325">For example, consider hello scenario where your role unexpectedly goes down.</span></span> <span data-ttu-id="d17de-326">Ha elveszíti egy adatsornak, hello szempontjából kisebb, mint egy nagy sorköteg el nem küldött elvesztése hello hatását.</span><span class="sxs-lookup"><span data-stu-id="d17de-326">If you lose one row of data, hello impact is smaller than hello impact of losing a large batch of unsubmitted rows.</span></span> <span data-ttu-id="d17de-327">Nagyobb veszélynek van, amikor sorok elegendő pufferrel, mielőtt elküldi őket a megadott időkeretnél toohello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="d17de-327">There is a greater risk when you buffer rows before sending them toohello database in a specified time window.</span></span>

<span data-ttu-id="d17de-328">Miatt ez kompromisszumot kiértékelheti, hogy Ön kötegelt hello típusú műveletek.</span><span class="sxs-lookup"><span data-stu-id="d17de-328">Because of this tradeoff, evaluate hello type of operations that you batch.</span></span> <span data-ttu-id="d17de-329">A Batch-agresszívabb (nagyobb kötegek és hosszabb idő windows) kevésbé fontos adatokkal.</span><span class="sxs-lookup"><span data-stu-id="d17de-329">Batch more aggressively (larger batches and longer time windows) with data that is less critical.</span></span>

### <a name="batch-size"></a><span data-ttu-id="d17de-330">Köteg mérete</span><span class="sxs-lookup"><span data-stu-id="d17de-330">Batch size</span></span>
<span data-ttu-id="d17de-331">A tesztelés során történt általában nem szolgál előnyökkel toobreaking nagy kötegek kisebb adattömbökbe.</span><span class="sxs-lookup"><span data-stu-id="d17de-331">In our tests, there was typically no advantage toobreaking large batches into smaller chunks.</span></span> <span data-ttu-id="d17de-332">Gyakran ez felosztása, mint egy egyetlen nagy kötegelt lassabban eredményezett.</span><span class="sxs-lookup"><span data-stu-id="d17de-332">In fact, this subdivision often resulted in slower performance than submitting a single large batch.</span></span> <span data-ttu-id="d17de-333">Példaként vegyünk egy forgatókönyvet, ahol azt szeretné, hogy tooinsert 1000 sor.</span><span class="sxs-lookup"><span data-stu-id="d17de-333">For example, consider a scenario where you want tooinsert 1000 rows.</span></span> <span data-ttu-id="d17de-334">hello következő táblázatban mennyi idő alatt toouse tábla értékű paraméter tooinsert 1000 sorok, amikor kisebb kötegekben osztva.</span><span class="sxs-lookup"><span data-stu-id="d17de-334">hello following table shows how long it takes toouse table-valued parameters tooinsert 1000 rows when divided into smaller batches.</span></span>

| <span data-ttu-id="d17de-335">Köteg mérete</span><span class="sxs-lookup"><span data-stu-id="d17de-335">Batch size</span></span> | <span data-ttu-id="d17de-336">Az ismétlés</span><span class="sxs-lookup"><span data-stu-id="d17de-336">Iterations</span></span> | <span data-ttu-id="d17de-337">Tábla értékű paraméter (ms)</span><span class="sxs-lookup"><span data-stu-id="d17de-337">Table-valued parameters (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d17de-338">1000</span><span class="sxs-lookup"><span data-stu-id="d17de-338">1000</span></span> |<span data-ttu-id="d17de-339">1</span><span class="sxs-lookup"><span data-stu-id="d17de-339">1</span></span> |<span data-ttu-id="d17de-340">347</span><span class="sxs-lookup"><span data-stu-id="d17de-340">347</span></span> |
| <span data-ttu-id="d17de-341">500</span><span class="sxs-lookup"><span data-stu-id="d17de-341">500</span></span> |<span data-ttu-id="d17de-342">2</span><span class="sxs-lookup"><span data-stu-id="d17de-342">2</span></span> |<span data-ttu-id="d17de-343">355</span><span class="sxs-lookup"><span data-stu-id="d17de-343">355</span></span> |
| <span data-ttu-id="d17de-344">100</span><span class="sxs-lookup"><span data-stu-id="d17de-344">100</span></span> |<span data-ttu-id="d17de-345">10</span><span class="sxs-lookup"><span data-stu-id="d17de-345">10</span></span> |<span data-ttu-id="d17de-346">465</span><span class="sxs-lookup"><span data-stu-id="d17de-346">465</span></span> |
| <span data-ttu-id="d17de-347">50</span><span class="sxs-lookup"><span data-stu-id="d17de-347">50</span></span> |<span data-ttu-id="d17de-348">20</span><span class="sxs-lookup"><span data-stu-id="d17de-348">20</span></span> |<span data-ttu-id="d17de-349">630</span><span class="sxs-lookup"><span data-stu-id="d17de-349">630</span></span> |

> [!NOTE]
> <span data-ttu-id="d17de-350">A eredményei nem referenciaalapokhoz képest.</span><span class="sxs-lookup"><span data-stu-id="d17de-350">Results are not benchmarks.</span></span> <span data-ttu-id="d17de-351">Lásd: hello [időzítési eredményezi, hogy ez a témakör Megjegyzés](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="d17de-351">See hello [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="d17de-352">Láthatja, hogy a legjobb teljesítményt hello 1000 sor toosubmit egyszerre őket.</span><span class="sxs-lookup"><span data-stu-id="d17de-352">You can see that hello best performance for 1000 rows is toosubmit them all at once.</span></span> <span data-ttu-id="d17de-353">Más tesztekben (itt nem látható) történt egy kis teljesítmény nyereség toobreak be két kötegek 5000 10000 sor kötegben.</span><span class="sxs-lookup"><span data-stu-id="d17de-353">In other tests (not shown here) there was a small performance gain toobreak a 10000 row batch into two batches of 5000.</span></span> <span data-ttu-id="d17de-354">Azonban ezekben a tesztekben hello táblaséma viszonylag egyszerű, így végre kell hajtania teszteli, az adatokat és a Köteg mérete tooverify ezen eredmények.</span><span class="sxs-lookup"><span data-stu-id="d17de-354">But hello table schema for these tests is relatively simple, so you should perform tests on your specific data and batch sizes tooverify these findings.</span></span>

<span data-ttu-id="d17de-355">Egy másik tényező tooconsider, hogy hello teljes kötegelt túl nagyra nő, ha SQL-adatbázis előfordulhat, hogy sávszélesség-szabályozási és toocommit hello kötegelt elutasítja.</span><span class="sxs-lookup"><span data-stu-id="d17de-355">Another factor tooconsider is that if hello total batch becomes too large, SQL Database might throttle and refuse toocommit hello batch.</span></span> <span data-ttu-id="d17de-356">Ha az épp ezért tökéletes választás a köteg méretének vizsgálat az adott forgatókönyv toodetermine hello legjobb eredmények elérése érdekében.</span><span class="sxs-lookup"><span data-stu-id="d17de-356">For hello best results, test your specific scenario toodetermine if there is an ideal batch size.</span></span> <span data-ttu-id="d17de-357">Hello köteg mérete konfigurálható tegye a futásidejű tooenable gyors beállításai alapján a teljesítmény- vagy hibákat.</span><span class="sxs-lookup"><span data-stu-id="d17de-357">Make hello batch size configurable at runtime tooenable quick adjustments based on performance or errors.</span></span>

<span data-ttu-id="d17de-358">Végezetül egyenleg hello köteg mérete hello hello kockázatot jelentő társított kötegelés.</span><span class="sxs-lookup"><span data-stu-id="d17de-358">Finally, balance hello size of hello batch with hello risks associated with batching.</span></span> <span data-ttu-id="d17de-359">Ha átmeneti hiba merül fel, vagy hello szerepkör meghiúsul, fontolja meg a hello következmények hello művelet vagy hello adatvesztés hello kötegben.</span><span class="sxs-lookup"><span data-stu-id="d17de-359">If there are transient errors or hello role fails, consider hello consequences of retrying hello operation or of losing hello data in hello batch.</span></span>

### <a name="parallel-processing"></a><span data-ttu-id="d17de-360">Párhuzamos feldolgozás</span><span class="sxs-lookup"><span data-stu-id="d17de-360">Parallel processing</span></span>
<span data-ttu-id="d17de-361">Mi történik, ha tartott hello megközelítés hello a köteg méretének csökkentését, de több szál tooexecute hello munkahelyi használt?</span><span class="sxs-lookup"><span data-stu-id="d17de-361">What if you took hello approach of reducing hello batch size but used multiple threads tooexecute hello work?</span></span> <span data-ttu-id="d17de-362">Ebben az esetben a tesztek bemutatta, hogy több kisebb többszálas kötegek általában végre a nagyobb kötegek rosszabb.</span><span class="sxs-lookup"><span data-stu-id="d17de-362">Again, our tests showed that several smaller multithreaded batches typically performed worse than a single larger batch.</span></span> <span data-ttu-id="d17de-363">hello következő teszt megpróbál egy vagy több párhuzamos kötegekben tooinsert 1000 sor.</span><span class="sxs-lookup"><span data-stu-id="d17de-363">hello following test attempts tooinsert 1000 rows in one or more parallel batches.</span></span> <span data-ttu-id="d17de-364">Ez a vizsgálat bemutatja, hogyan több egyidejű kötegek ténylegesen csökkent teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="d17de-364">This test shows how more simultaneous batches actually decreased performance.</span></span>

| <span data-ttu-id="d17de-365">A köteg méretének [ismétlési]</span><span class="sxs-lookup"><span data-stu-id="d17de-365">Batch size [Iterations]</span></span> | <span data-ttu-id="d17de-366">Két szállal (ms)</span><span class="sxs-lookup"><span data-stu-id="d17de-366">Two threads (ms)</span></span> | <span data-ttu-id="d17de-367">Négy szálak (ms)</span><span class="sxs-lookup"><span data-stu-id="d17de-367">Four threads (ms)</span></span> | <span data-ttu-id="d17de-368">Hat szálak (ms)</span><span class="sxs-lookup"><span data-stu-id="d17de-368">Six threads (ms)</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="d17de-369">1000 [1]</span><span class="sxs-lookup"><span data-stu-id="d17de-369">1000 [1]</span></span> |<span data-ttu-id="d17de-370">277</span><span class="sxs-lookup"><span data-stu-id="d17de-370">277</span></span> |<span data-ttu-id="d17de-371">315</span><span class="sxs-lookup"><span data-stu-id="d17de-371">315</span></span> |<span data-ttu-id="d17de-372">266</span><span class="sxs-lookup"><span data-stu-id="d17de-372">266</span></span> |
| <span data-ttu-id="d17de-373">500 [2]</span><span class="sxs-lookup"><span data-stu-id="d17de-373">500 [2]</span></span> |<span data-ttu-id="d17de-374">548</span><span class="sxs-lookup"><span data-stu-id="d17de-374">548</span></span> |<span data-ttu-id="d17de-375">278</span><span class="sxs-lookup"><span data-stu-id="d17de-375">278</span></span> |<span data-ttu-id="d17de-376">256</span><span class="sxs-lookup"><span data-stu-id="d17de-376">256</span></span> |
| <span data-ttu-id="d17de-377">250 [4]</span><span class="sxs-lookup"><span data-stu-id="d17de-377">250 [4]</span></span> |<span data-ttu-id="d17de-378">405</span><span class="sxs-lookup"><span data-stu-id="d17de-378">405</span></span> |<span data-ttu-id="d17de-379">329</span><span class="sxs-lookup"><span data-stu-id="d17de-379">329</span></span> |<span data-ttu-id="d17de-380">265</span><span class="sxs-lookup"><span data-stu-id="d17de-380">265</span></span> |
| <span data-ttu-id="d17de-381">100 [10]</span><span class="sxs-lookup"><span data-stu-id="d17de-381">100 [10]</span></span> |<span data-ttu-id="d17de-382">488</span><span class="sxs-lookup"><span data-stu-id="d17de-382">488</span></span> |<span data-ttu-id="d17de-383">439</span><span class="sxs-lookup"><span data-stu-id="d17de-383">439</span></span> |<span data-ttu-id="d17de-384">391</span><span class="sxs-lookup"><span data-stu-id="d17de-384">391</span></span> |

> [!NOTE]
> <span data-ttu-id="d17de-385">A eredményei nem referenciaalapokhoz képest.</span><span class="sxs-lookup"><span data-stu-id="d17de-385">Results are not benchmarks.</span></span> <span data-ttu-id="d17de-386">Lásd: hello [időzítési eredményezi, hogy ez a témakör Megjegyzés](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="d17de-386">See hello [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="d17de-387">Több lehetséges oka lehet hello akár teljesítménycsökkenés esedékes tooparallelism:</span><span class="sxs-lookup"><span data-stu-id="d17de-387">There are several potential reasons for hello degradation in performance due tooparallelism:</span></span>

* <span data-ttu-id="d17de-388">Nincsenek egy helyett több egyidejű hálózati hívást.</span><span class="sxs-lookup"><span data-stu-id="d17de-388">There are multiple simultaneous network calls instead of one.</span></span>
* <span data-ttu-id="d17de-389">Egyetlen tábla több műveleteket versengés és blokkolja a eredményezhet.</span><span class="sxs-lookup"><span data-stu-id="d17de-389">Multiple operations against a single table can result in contention and blocking.</span></span>
* <span data-ttu-id="d17de-390">Nincsenek társított terhek többszálas.</span><span class="sxs-lookup"><span data-stu-id="d17de-390">There are overheads associated with multithreading.</span></span>
* <span data-ttu-id="d17de-391">több kapcsolat megnyitása hello költsége ez fontosabb, mint a hello előnye, hogy a párhuzamos feldolgozást.</span><span class="sxs-lookup"><span data-stu-id="d17de-391">hello expense of opening multiple connections outweighs hello benefit of parallel processing.</span></span>

<span data-ttu-id="d17de-392">Különböző táblákhoz vagy adatbázisok célozhat esetén lehetséges toosee néhány teljesítmény szerezhet az ezt a stratégiát.</span><span class="sxs-lookup"><span data-stu-id="d17de-392">If you target different tables or databases, it is possible toosee some performance gain with this strategy.</span></span> <span data-ttu-id="d17de-393">Adatbázis horizontális vagy összevonási lenne a forgatókönyv ezt a módszert használja.</span><span class="sxs-lookup"><span data-stu-id="d17de-393">Database sharding or federations would be a scenario for this approach.</span></span> <span data-ttu-id="d17de-394">Horizontális több adatbázisok és útvonalak különböző az tooeach-adatbázist használja.</span><span class="sxs-lookup"><span data-stu-id="d17de-394">Sharding uses multiple databases and routes different data tooeach database.</span></span> <span data-ttu-id="d17de-395">Ha egy kis köteg tooa másik adatbázishoz, hatékonyabb lehet majd hello műveletet hajt végre párhuzamosan.</span><span class="sxs-lookup"><span data-stu-id="d17de-395">If each small batch is going tooa different database, then performing hello operations in parallel can be more efficient.</span></span> <span data-ttu-id="d17de-396">Azonban hello jobb teljesítménye nincs elég jelentős toouse egy döntési toouse adatbázis horizontális a megoldásban a hello alapjaként.</span><span class="sxs-lookup"><span data-stu-id="d17de-396">However, hello performance gain is not significant enough toouse as hello basis for a decision toouse database sharding in your solution.</span></span>

<span data-ttu-id="d17de-397">Néhány terveibe kisebb kötegekben párhuzamos végrehajtása eredményezhet továbbfejlesztett kapacitásának terhelés alatt a rendszer.</span><span class="sxs-lookup"><span data-stu-id="d17de-397">In some designs, parallel execution of smaller batches can result in improved throughput of requests in a system under load.</span></span> <span data-ttu-id="d17de-398">Ebben az esetben akkor is, ha a nagyobb kötegek gyorsabb tooprocess, párhuzamosan több kötegenként lehet hatékonyabb.</span><span class="sxs-lookup"><span data-stu-id="d17de-398">In this case, even though it is quicker tooprocess a single larger batch, processing multiple batches in parallel might be more efficient.</span></span>

<span data-ttu-id="d17de-399">Ha párhuzamos végrehajtás használja, fontolja meg az ellenőrző hello szálak maximális száma worker.</span><span class="sxs-lookup"><span data-stu-id="d17de-399">If you do use parallel execution, consider controlling hello maximum number of worker threads.</span></span> <span data-ttu-id="d17de-400">Kevesebb gyorsabb végrehajtási idő, és kevesebb a versengés eredményezheti.</span><span class="sxs-lookup"><span data-stu-id="d17de-400">A smaller number might result in less contention and a faster execution time.</span></span> <span data-ttu-id="d17de-401">Figyelembe venni, a hello egyéb terheléseket, amelyeket ez hello céladatbázis kapcsolatok és a tranzakciók egyaránt helyezi.</span><span class="sxs-lookup"><span data-stu-id="d17de-401">Also, consider hello additional load that this places on hello target database both in connections and transactions.</span></span>

### <a name="related-performance-factors"></a><span data-ttu-id="d17de-402">A teljesítmény kapcsolódó tényezők</span><span class="sxs-lookup"><span data-stu-id="d17de-402">Related performance factors</span></span>
<span data-ttu-id="d17de-403">Adatbázis teljesítménye jellemző útmutatást is kötegelés hatással van.</span><span class="sxs-lookup"><span data-stu-id="d17de-403">Typical guidance on database performance also affects batching.</span></span> <span data-ttu-id="d17de-404">Például be nagy elsődleges kulcs, vagy sok fürtözetlen indexeire rendelkező táblák csökken a teljesítmény.</span><span class="sxs-lookup"><span data-stu-id="d17de-404">For example, insert performance is reduced for tables that have a large primary key or many nonclustered indexes.</span></span>

<span data-ttu-id="d17de-405">Ha a tábla értékű paraméter tárolt eljárás használatához paranccsal hello **SET NOCOUNT ON** hello eljárás hello elején.</span><span class="sxs-lookup"><span data-stu-id="d17de-405">If table-valued parameters use a stored procedure, you can use hello command **SET NOCOUNT ON** at hello beginning of hello procedure.</span></span> <span data-ttu-id="d17de-406">A jelen nyilatkozat mellőzi hello visszatérési hello eljárás hello érintett sorok hello száma.</span><span class="sxs-lookup"><span data-stu-id="d17de-406">This statement suppresses hello return of hello count of hello affected rows in hello procedure.</span></span> <span data-ttu-id="d17de-407">Azonban a tesztelés során használatát hello **SET NOCOUNT ON** kellett nincs hatása, vagy teljesítménye csökkent.</span><span class="sxs-lookup"><span data-stu-id="d17de-407">However, in our tests, hello use of **SET NOCOUNT ON** either had no effect or decreased performance.</span></span> <span data-ttu-id="d17de-408">hello vizsgálati tárolt eljárás egyszerű volt egyetlen **BESZÚRÁSA** hello tábla értékű paraméter parancsot.</span><span class="sxs-lookup"><span data-stu-id="d17de-408">hello test stored procedure was simple with a single **INSERT** command from hello table-valued parameter.</span></span> <span data-ttu-id="d17de-409">Akkor lehet, hogy összetettebb tárolt eljárások a jelen nyilatkozat előnyös.</span><span class="sxs-lookup"><span data-stu-id="d17de-409">It is possible that more complex stored procedures would benefit from this statement.</span></span> <span data-ttu-id="d17de-410">Nem érdemes feltételezni, hogy hozzáadása, de **SET NOCOUNT ON** tooyour tárolt eljárás automatikusan javítja a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="d17de-410">But don’t assume that adding **SET NOCOUNT ON** tooyour stored procedure automatically improves performance.</span></span> <span data-ttu-id="d17de-411">toounderstand hello hatása, tesztelje a tárolt eljárás használatával és anélkül hello **SET NOCOUNT ON** utasítást.</span><span class="sxs-lookup"><span data-stu-id="d17de-411">toounderstand hello effect, test your stored procedure with and without hello **SET NOCOUNT ON** statement.</span></span>

## <a name="batching-scenarios"></a><span data-ttu-id="d17de-412">Kötegelése forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="d17de-412">Batching scenarios</span></span>
<span data-ttu-id="d17de-413">hello alábbi szakaszok azt ismertetik, hogyan toouse táblázat értékű paramétereket a három alkalmazás-forgatókönyveket.</span><span class="sxs-lookup"><span data-stu-id="d17de-413">hello following sections describe how toouse table-valued parameters in three application scenarios.</span></span> <span data-ttu-id="d17de-414">hello első forgatókönyv bemutatja, hogyan pufferelés és kötegelés hogyan tudnak együttműködni.</span><span class="sxs-lookup"><span data-stu-id="d17de-414">hello first scenario shows how buffering and batching can work together.</span></span> <span data-ttu-id="d17de-415">második forgatókönyv hello javítja a teljesítményt, egyetlen tárolt eljárás hívása a fő-részletek műveletet hajt végre.</span><span class="sxs-lookup"><span data-stu-id="d17de-415">hello second scenario improves performance by performing master-detail operations in a single stored procedure call.</span></span> <span data-ttu-id="d17de-416">végső forgatókönyvet mutat be hello hogyan toouse tábla értékű paraméter "UPSERT" műveletben.</span><span class="sxs-lookup"><span data-stu-id="d17de-416">hello final scenario shows how toouse table-valued parameters in an “UPSERT” operation.</span></span>

### <a name="buffering"></a><span data-ttu-id="d17de-417">Pufferelés</span><span class="sxs-lookup"><span data-stu-id="d17de-417">Buffering</span></span>
<span data-ttu-id="d17de-418">Habár van néhány olyan forgatókönyvet, nyilvánvaló jelölt kötegelés, ott sikerült előnyeit által késleltetett feldolgozási kötegelés több forgatókönyv áll.</span><span class="sxs-lookup"><span data-stu-id="d17de-418">Although there are some scenarios that are obvious candidate for batching, there are many scenarios that could take advantage of batching by delayed processing.</span></span> <span data-ttu-id="d17de-419">Azonban késleltetett feldolgozása is hordoz magában, ha nagyobb kockázata, hogy egy nem várt hiba hello esemény hello adatok elvesznek.</span><span class="sxs-lookup"><span data-stu-id="d17de-419">However, delayed processing also carries a greater risk that hello data is lost in hello event of an unexpected failure.</span></span> <span data-ttu-id="d17de-420">Fontos toounderstand ennek a veszélyét, és vegye figyelembe a hello következményeiről.</span><span class="sxs-lookup"><span data-stu-id="d17de-420">It is important toounderstand this risk and consider hello consequences.</span></span>

<span data-ttu-id="d17de-421">Tegyük fel, amely nyomon követi a minden felhasználó hello előzménylistáján webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d17de-421">For example, consider a web application that tracks hello navigation history of each user.</span></span> <span data-ttu-id="d17de-422">Minden lap kérésre hello alkalmazás sikerült egy adatbázis hívás toorecord hello felhasználói lap nézetben.</span><span class="sxs-lookup"><span data-stu-id="d17de-422">On each page request, hello application could make a database call toorecord hello user’s page view.</span></span> <span data-ttu-id="d17de-423">De magasabb teljesítmény és méretezhetőség pufferelés hello felhasználók navigációs tevékenységeket, és elküldi az adatokat toohello adatbázis kötegekben úgy lehet elérni.</span><span class="sxs-lookup"><span data-stu-id="d17de-423">But higher performance and scalability can be achieved by buffering hello users’ navigation activities and then sending this data toohello database in batches.</span></span> <span data-ttu-id="d17de-424">Hello adatbázis módosítása során eltelt idő és/vagy puffer mérete által aktiválhatók.</span><span class="sxs-lookup"><span data-stu-id="d17de-424">You can trigger hello database update by elapsed time and/or buffer size.</span></span> <span data-ttu-id="d17de-425">Egy szabály például megadhatja, hogy hello kötegelt fel kell dolgozni, 20 másodperc, vagy amikor hello puffer eléri-e 1000 elem után.</span><span class="sxs-lookup"><span data-stu-id="d17de-425">For example, a rule could specify that hello batch should be processed after 20 seconds or when hello buffer reaches 1000 items.</span></span>

<span data-ttu-id="d17de-426">hello alábbi példakód [reaktív bővítmények – a Rx](https://msdn.microsoft.com/data/gg577609) tooprocess pufferelt figyelési osztály által kiváltott esemény.</span><span class="sxs-lookup"><span data-stu-id="d17de-426">hello following code example uses [Reactive Extensions - Rx](https://msdn.microsoft.com/data/gg577609) tooprocess buffered events raised by a monitoring class.</span></span> <span data-ttu-id="d17de-427">Amikor a puffer kitöltés hello vagy időtúllépés, a felhasználói adatok hello kötegelt küldött toohello adatbázis egy tábla értékű paraméter.</span><span class="sxs-lookup"><span data-stu-id="d17de-427">When hello buffer fills or a timeout is reached, hello batch of user data is sent toohello database with a table-valued parameter.</span></span>

<span data-ttu-id="d17de-428">hello következő NavHistoryData osztály modellek hello felhasználói navigációs adatok.</span><span class="sxs-lookup"><span data-stu-id="d17de-428">hello following NavHistoryData class models hello user navigation details.</span></span> <span data-ttu-id="d17de-429">Például a felhasználói azonosító hello alapszintű információkat tartalmaz, hello URL-cím elérhető, hello hozzáférés időpontja.</span><span class="sxs-lookup"><span data-stu-id="d17de-429">It contains basic information such as hello user identifier, hello URL accessed, and hello access time.</span></span>

    public class NavHistoryData
    {
        public NavHistoryData(int userId, string url, DateTime accessTime)
        { UserId = userId; URL = url; AccessTime = accessTime; }
        public int UserId { get; set; }
        public string URL { get; set; }
        public DateTime AccessTime { get; set; }
    }

<span data-ttu-id="d17de-430">hello NavHistoryDataMonitor osztály hello felhasználói navigációs adatok toohello adatbázis pufferelés felelős.</span><span class="sxs-lookup"><span data-stu-id="d17de-430">hello NavHistoryDataMonitor class is responsible for buffering hello user navigation data toohello database.</span></span> <span data-ttu-id="d17de-431">Tartalmaz egy metódust, RecordUserNavigationEntry, amely válaszol-e megjelenítve jelzi egy **OnAdded** esemény.</span><span class="sxs-lookup"><span data-stu-id="d17de-431">It contains a method, RecordUserNavigationEntry, which responds by raising an **OnAdded** event.</span></span> <span data-ttu-id="d17de-432">hello következő kód bemutatja hello konstruktor logika, amely használja a Rx toocreate hello események alapján egy megfigyelhető gyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="d17de-432">hello following code shows hello constructor logic that uses Rx toocreate an observable collection based on hello event.</span></span> <span data-ttu-id="d17de-433">Majd feliratkozva toothis megfigyelhető gyűjtemény hello puffer metódussal.</span><span class="sxs-lookup"><span data-stu-id="d17de-433">It then subscribes toothis observable collection with hello Buffer method.</span></span> <span data-ttu-id="d17de-434">hello túlterhelési határozza meg, hogy hello puffer küldjön 20 másodpercenként vagy 1000 bejegyzéseket.</span><span class="sxs-lookup"><span data-stu-id="d17de-434">hello overload specifies that hello buffer should be sent every 20 seconds or 1000 entries.</span></span>

    public NavHistoryDataMonitor()
    {
        var observableData =
            Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");

        observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
    }

<span data-ttu-id="d17de-435">hello kezelő pufferelt hello elemek mindegyikét alakítja át a tábla értékű típus, és ezután továbbítja a típus tooa tárolt eljárás, hogy folyamatok hello kötegelt.</span><span class="sxs-lookup"><span data-stu-id="d17de-435">hello handler converts all of hello buffered items into a table-valued type and then passes this type tooa stored procedure that processes hello batch.</span></span> <span data-ttu-id="d17de-436">hello következő kód bemutatja hello hello NavHistoryDataEventArgs és a hello NavHistoryDataMonitor osztályok teljes definíciója.</span><span class="sxs-lookup"><span data-stu-id="d17de-436">hello following code shows hello complete definition for both hello NavHistoryDataEventArgs and hello NavHistoryDataMonitor classes.</span></span>

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

<span data-ttu-id="d17de-437">toouse az pufferelési osztály hello alkalmazás egy statikus NavHistoryDataMonitor objektumot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d17de-437">toouse this buffering class, hello application creates a static NavHistoryDataMonitor object.</span></span> <span data-ttu-id="d17de-438">Minden alkalommal, amikor egy felhasználó hozzáfér egy lap hello alkalmazás meghívja hello NavHistoryDataMonitor.RecordUserNavigationEntry metódust.</span><span class="sxs-lookup"><span data-stu-id="d17de-438">Each time a user accesses a page, hello application calls hello NavHistoryDataMonitor.RecordUserNavigationEntry method.</span></span> <span data-ttu-id="d17de-439">pufferelés logika hello kiszolgálásához küld e bejegyzések toohello adatbázis kötegekben tootake folytatódik.</span><span class="sxs-lookup"><span data-stu-id="d17de-439">hello buffering logic proceeds tootake care of sending these entries toohello database in batches.</span></span>

### <a name="master-detail"></a><span data-ttu-id="d17de-440">Fő részletei</span><span class="sxs-lookup"><span data-stu-id="d17de-440">Master detail</span></span>
<span data-ttu-id="d17de-441">Tábla értékű paraméter egyszerű INSERT forgatókönyvek hasznosak.</span><span class="sxs-lookup"><span data-stu-id="d17de-441">Table-valued parameters are useful for simple INSERT scenarios.</span></span> <span data-ttu-id="d17de-442">Lehet azonban nehezebb toobatch Beszúrások, például több mint egy olyan táblát.</span><span class="sxs-lookup"><span data-stu-id="d17de-442">However, it can be more challenging toobatch inserts that involve more than one table.</span></span> <span data-ttu-id="d17de-443">hello "kapcsolatú" például az is jó példa.</span><span class="sxs-lookup"><span data-stu-id="d17de-443">hello “master/detail” scenario is a good example.</span></span> <span data-ttu-id="d17de-444">hello fő tábla elsődleges entitás hello azonosítja.</span><span class="sxs-lookup"><span data-stu-id="d17de-444">hello master table identifies hello primary entity.</span></span> <span data-ttu-id="d17de-445">Egy vagy több részletek tábla kapcsolatos hello entitás több adatot tároljon.</span><span class="sxs-lookup"><span data-stu-id="d17de-445">One or more detail tables store more data about hello entity.</span></span> <span data-ttu-id="d17de-446">Ebben a forgatókönyvben a külső kulcsok kapcsolatai kényszerítése hello kapcsolat részletek tooa egyedi fő entitás.</span><span class="sxs-lookup"><span data-stu-id="d17de-446">In this scenario, foreign key relationships enforce hello relationship of details tooa unique master entity.</span></span> <span data-ttu-id="d17de-447">Vegye figyelembe a PurchaseOrder és a kapcsolódó OrderDetail tábla egyszerűsített verziója.</span><span class="sxs-lookup"><span data-stu-id="d17de-447">Consider a simplified version of a PurchaseOrder table and its associated OrderDetail table.</span></span> <span data-ttu-id="d17de-448">a következő Transact-SQL hello hello PurchaseOrder tábla négy oszlopot hoz létre: OrderID, orderdate oszlopra, CustomerID és állapotát.</span><span class="sxs-lookup"><span data-stu-id="d17de-448">hello following Transact-SQL creates hello PurchaseOrder table with four columns: OrderID, OrderDate, CustomerID, and Status.</span></span>

    CREATE TABLE [dbo].[PurchaseOrder](
    [OrderID] [int] IDENTITY(1,1) NOT NULL,
    [OrderDate] [datetime] NOT NULL,
    [CustomerID] [int] NOT NULL,
    [Status] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrder] 
    PRIMARY KEY CLUSTERED ( [OrderID] ASC ))

<span data-ttu-id="d17de-449">Minden egyes rendelés egy vagy több termék vásárlás tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d17de-449">Each order contains one or more product purchases.</span></span> <span data-ttu-id="d17de-450">Ezek az információk rögzítése hello PurchaseOrderDetail táblában.</span><span class="sxs-lookup"><span data-stu-id="d17de-450">This information is captured in hello PurchaseOrderDetail table.</span></span> <span data-ttu-id="d17de-451">a következő Transact-SQL hello hello PurchaseOrderDetail táblázatot hoz öt oszlopokkal: OrderID, OrderDetailID, ProductID, Egységár és OrderQty.</span><span class="sxs-lookup"><span data-stu-id="d17de-451">hello following Transact-SQL creates hello PurchaseOrderDetail table with five columns: OrderID, OrderDetailID, ProductID, UnitPrice, and OrderQty.</span></span>

    CREATE TABLE [dbo].[PurchaseOrderDetail](
    [OrderID] [int] NOT NULL,
    [OrderDetailID] [int] IDENTITY(1,1) NOT NULL,
    [ProductID] [int] NOT NULL,
    [UnitPrice] [money] NULL,
    [OrderQty] [smallint] NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrderDetail] PRIMARY KEY CLUSTERED 
    ( [OrderID] ASC, [OrderDetailID] ASC ))

<span data-ttu-id="d17de-452">hello OrderID oszlop hello PurchaseOrderDetail tábla sorrendben kell hivatkoznia, hello PurchaseOrder táblából.</span><span class="sxs-lookup"><span data-stu-id="d17de-452">hello OrderID column in hello PurchaseOrderDetail table must reference an order from hello PurchaseOrder table.</span></span> <span data-ttu-id="d17de-453">a következő idegen kulcs definícióját hello kikényszeríti ennél a határértéknél.</span><span class="sxs-lookup"><span data-stu-id="d17de-453">hello following definition of a foreign key enforces this constraint.</span></span>

    ALTER TABLE [dbo].[PurchaseOrderDetail]  WITH CHECK ADD 
    CONSTRAINT [FK_OrderID_PurchaseOrder] FOREIGN KEY([OrderID])
    REFERENCES [dbo].[PurchaseOrder] ([OrderID])

<span data-ttu-id="d17de-454">Rendelés toouse tábla értékű paraméterek rendelkeznie kell egy felhasználó által definiált táblatípus minden céloldali táblához.</span><span class="sxs-lookup"><span data-stu-id="d17de-454">In order toouse table-valued parameters, you must have one user-defined table type for each target table.</span></span>

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

<span data-ttu-id="d17de-455">Majd adja meg, amely támogatja a következő típusú táblák tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="d17de-455">Then define a stored procedure that accepts tables of these types.</span></span> <span data-ttu-id="d17de-456">Ez az eljárás lehetővé teszi, hogy egy alkalmazás toolocally kötegelt rendeléseket és a rendelés részleteit egyetlen hívásban.</span><span class="sxs-lookup"><span data-stu-id="d17de-456">This procedure allows an application toolocally batch a set of orders and order details in a single call.</span></span> <span data-ttu-id="d17de-457">hello következő Transact-SQL biztosít hello teljes tárolt eljárás deklaráció a következő példában szereplő beszerzési rendelés.</span><span class="sxs-lookup"><span data-stu-id="d17de-457">hello following Transact-SQL provides hello complete stored procedure declaration for this purchase order example.</span></span>

    CREATE PROCEDURE sp_InsertOrdersBatch (
    @orders as PurchaseOrderTableType READONLY,
    @details as PurchaseOrderDetailTableType READONLY )
    AS
    SET NOCOUNT ON;

    -- Table that connects hello order identifiers in hello @orders
    -- table with hello actual order identifiers in hello PurchaseOrder table
    DECLARE @IdentityLink AS TABLE ( 
    SubmittedKey int, 
    ActualKey int, 
    RowNumber int identity(1,1)
    );

          -- Add new orders toohello PurchaseOrder table, storing hello actual
    -- order identifiers in hello @IdentityLink table   
    INSERT INTO PurchaseOrder ([OrderDate], [CustomerID], [Status])
    OUTPUT inserted.OrderID INTO @IdentityLink (ActualKey)
    SELECT [OrderDate], [CustomerID], [Status] FROM @orders ORDER BY OrderID;

    -- Match hello passed-in order identifiers with hello actual identifiers
    -- and complete hello @IdentityLink table for use with inserting hello details
    WITH OrderedRows As (
    SELECT OrderID, ROW_NUMBER () OVER (ORDER BY OrderID) As RowNumber 
    FROM @orders
    )
    UPDATE @IdentityLink SET SubmittedKey = M.OrderID
    FROM @IdentityLink L JOIN OrderedRows M ON L.RowNumber = M.RowNumber;

    -- Insert hello order details into hello PurchaseOrderDetail table, 
          -- using hello actual order identifiers of hello master table, PurchaseOrder
    INSERT INTO PurchaseOrderDetail (
    [OrderID],
    [ProductID],
    [UnitPrice],
    [OrderQty] )
    SELECT L.ActualKey, D.ProductID, D.UnitPrice, D.OrderQty
    FROM @details D
    JOIN @IdentityLink L ON L.SubmittedKey = D.OrderID;
    GO

<span data-ttu-id="d17de-458">Ebben a példában a helyileg definiált hello @IdentityLink a táblázat sorainak újonnan beszúrt hello hello tényleges OrderID értéke.</span><span class="sxs-lookup"><span data-stu-id="d17de-458">In this example, hello locally defined @IdentityLink table stores hello actual OrderID values from hello newly inserted rows.</span></span> <span data-ttu-id="d17de-459">Ilyen rendelés azonosítókat hello hello ideiglenes OrderID értékek eltérnek @orders és @details tábla értékű paraméter.</span><span class="sxs-lookup"><span data-stu-id="d17de-459">These order identifiers are different from hello temporary OrderID values in hello @orders and @details table-valued parameters.</span></span> <span data-ttu-id="d17de-460">Emiatt hello @IdentityLink tábla majd összekapcsolja hello OrderID értékek hello @orders toohello valós OrderID paraméterértékek hello PurchaseOrder tábla hello új soraihoz.</span><span class="sxs-lookup"><span data-stu-id="d17de-460">For this reason, hello @IdentityLink table then connects hello OrderID values from hello @orders parameter toohello real OrderID values for hello new rows in hello PurchaseOrder table.</span></span> <span data-ttu-id="d17de-461">Ez a lépés után hello @IdentityLink tábla megkönnyítheti beszúrni hello rendelés részleteit a hello tényleges OrderID, amely eleget tesz a hello idegenkulcs-megkötés.</span><span class="sxs-lookup"><span data-stu-id="d17de-461">After this step, hello @IdentityLink table can facilitate inserting hello order details with hello actual OrderID that satisfies hello foreign key constraint.</span></span>

<span data-ttu-id="d17de-462">Ez a tárolt eljárás használható kód vagy más Transact-SQL-hívások.</span><span class="sxs-lookup"><span data-stu-id="d17de-462">This stored procedure can be used from code or from other Transact-SQL calls.</span></span> <span data-ttu-id="d17de-463">A dokumentum a kód például a hello tábla értékű paraméter című szakaszában talál.</span><span class="sxs-lookup"><span data-stu-id="d17de-463">See hello table-valued parameters section of this paper for a code example.</span></span> <span data-ttu-id="d17de-464">a következő Transact-SQL hello bemutatja, hogyan toocall hello sp_InsertOrdersBatch.</span><span class="sxs-lookup"><span data-stu-id="d17de-464">hello following Transact-SQL shows how toocall hello sp_InsertOrdersBatch.</span></span>

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

<span data-ttu-id="d17de-465">Ez a megoldás lehetővé teszi, hogy minden kötegelt toouse OrderID értékek: 1 kezdődő.</span><span class="sxs-lookup"><span data-stu-id="d17de-465">This solution allows each batch toouse a set of OrderID values that begin at 1.</span></span> <span data-ttu-id="d17de-466">Ideiglenes OrderID értékeiről leírják hello kapcsolatok hello kötegben, de hello tényleges OrderID értékek határozzák meg a hello beszúrási művelet hello időpontjában.</span><span class="sxs-lookup"><span data-stu-id="d17de-466">These temporary OrderID values describe hello relationships in hello batch, but hello actual OrderID values are determined at hello time of hello insert operation.</span></span> <span data-ttu-id="d17de-467">A befejezésre hello azonos utasítások hello előző példában szereplő, és egyedi rendeléseket hello adatbázis lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="d17de-467">You can run hello same statements in hello previous example repeatedly and generate unique orders in hello database.</span></span> <span data-ttu-id="d17de-468">Emiatt érdemes további kóddal vagy az adatbázis logika, amely megakadályozza a duplikált rendelések használatakor ezzel a technikával kötegelés.</span><span class="sxs-lookup"><span data-stu-id="d17de-468">For this reason, consider adding more code or database logic that prevents duplicate orders when using this batching technique.</span></span>

<span data-ttu-id="d17de-469">Ez a példa mutatja be, hogy még inkább összetett adatbázis-műveletek, például a részletezés master operations, is lehet kötegelni tábla értékű paraméterek használatával.</span><span class="sxs-lookup"><span data-stu-id="d17de-469">This example demonstrates that even more complex database operations, such as master-detail operations, can be batched using table-valued parameters.</span></span>

### <a name="upsert"></a><span data-ttu-id="d17de-470">UPSERT</span><span class="sxs-lookup"><span data-stu-id="d17de-470">UPSERT</span></span>
<span data-ttu-id="d17de-471">Egy másik kötegelési forgatókönyv magában foglalja a egyidejűleg frissíteni a létező sorok és új sort beszúrni.</span><span class="sxs-lookup"><span data-stu-id="d17de-471">Another batching scenario involves simultaneously updating existing rows and inserting new rows.</span></span> <span data-ttu-id="d17de-472">Ez a művelet néha hivatkozott tooas "UPSERT" (frissítés + insert) művelet esetében.</span><span class="sxs-lookup"><span data-stu-id="d17de-472">This operation is sometimes referred tooas an “UPSERT” (update + insert) operation.</span></span> <span data-ttu-id="d17de-473">Ahelyett, hogy külön hívások tooINSERT és a frissítés elvégzése, hello MERGE utasítás a legjobb olyan környezethez a legalkalmasabb toothis feladat.</span><span class="sxs-lookup"><span data-stu-id="d17de-473">Rather than making separate calls tooINSERT and UPDATE, hello MERGE statement is best suited toothis task.</span></span> <span data-ttu-id="d17de-474">hello MERGE utasítás végrehajtása mindkét insert és frissítési művelet egyetlen hívással.</span><span class="sxs-lookup"><span data-stu-id="d17de-474">hello MERGE statement can perform both insert and update operations in a single call.</span></span>

<span data-ttu-id="d17de-475">Tábla értékű paraméterek hello MERGE utasítás tooperform frissítéseket és a Beszúrás használhatók.</span><span class="sxs-lookup"><span data-stu-id="d17de-475">Table-valued parameters can be used with hello MERGE statement tooperform updates and inserts.</span></span> <span data-ttu-id="d17de-476">Vegyük példaként a következő oszlopok hello tartalmazó egyszerűsített alkalmazott táblázat: EmployeeID, az Utónév, a Vezetéknév, a SocialSecurityNumber:</span><span class="sxs-lookup"><span data-stu-id="d17de-476">For example, consider a simplified Employee table that contains hello following columns: EmployeeID, FirstName, LastName, SocialSecurityNumber:</span></span>

    CREATE TABLE [dbo].[Employee](
    [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
    [FirstName] [nvarchar](50) NOT NULL,
    [LastName] [nvarchar](50) NOT NULL,
    [SocialSecurityNumber] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_Employee] PRIMARY KEY CLUSTERED 
    ([EmployeeID] ASC ))

<span data-ttu-id="d17de-477">Ebben a példában a hello arra, hogy hello SocialSecurityNumber egyedi tooperform több alkalmazott EGYESÍTÉSÉVEL is használhatja.</span><span class="sxs-lookup"><span data-stu-id="d17de-477">In this example, you can use hello fact that hello SocialSecurityNumber is unique tooperform a MERGE of multiple employees.</span></span> <span data-ttu-id="d17de-478">Először hozzon létre hello felhasználó által definiált táblatípus:</span><span class="sxs-lookup"><span data-stu-id="d17de-478">First, create hello user-defined table type:</span></span>

    CREATE TYPE EmployeeTableType AS TABLE 
    ( Employee_ID INT,
      FirstName NVARCHAR(50),
      LastName NVARCHAR(50),
      SocialSecurityNumber NVARCHAR(50) );
    GO

<span data-ttu-id="d17de-479">A következő tárolt eljárás létrehozása, vagy írhat kódot, hogy használ hello MERGE utasítás tooperform hello frissítést, majd szúrja be.</span><span class="sxs-lookup"><span data-stu-id="d17de-479">Next, create a stored procedure or write code that uses hello MERGE statement tooperform hello update and insert.</span></span> <span data-ttu-id="d17de-480">hello alábbi példában hello MERGE utasítás egy tábla értékű paraméter @employees, EmployeeTableType típusú.</span><span class="sxs-lookup"><span data-stu-id="d17de-480">hello following example uses hello MERGE statement on a table-valued parameter, @employees, of type EmployeeTableType.</span></span> <span data-ttu-id="d17de-481">hello tartalmát hello @employees tábla itt nem látható.</span><span class="sxs-lookup"><span data-stu-id="d17de-481">hello contents of hello @employees table are not shown here.</span></span>

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

<span data-ttu-id="d17de-482">További információkért lásd: hello dokumentáció és példák hello MERGE utasításban.</span><span class="sxs-lookup"><span data-stu-id="d17de-482">For more information, see hello documentation and examples for hello MERGE statement.</span></span> <span data-ttu-id="d17de-483">Ugyanazzal a munkahelyi elvégezhető egy több lépésben hello tárolt eljárás hívása külön INSERT vagy UPDATE műveletekkel, hello MERGE utasítás használata sokkal hatékonyabb.</span><span class="sxs-lookup"><span data-stu-id="d17de-483">Although hello same work could be performed in a multiple-step stored procedure call with separate INSERT and UPDATE operations, hello MERGE statement is more efficient.</span></span> <span data-ttu-id="d17de-484">Adatbázis kód hogyan hozhat létre közvetlenül nélkül két adatbázis hívások az INSERT vagy UPDATE hello MERGE utasítás használó Transact-SQL-hívások is.</span><span class="sxs-lookup"><span data-stu-id="d17de-484">Database code can also construct Transact-SQL calls that use hello MERGE statement directly without requiring two database calls for INSERT and UPDATE.</span></span>

## <a name="recommendation-summary"></a><span data-ttu-id="d17de-485">A javaslat összefoglaló</span><span class="sxs-lookup"><span data-stu-id="d17de-485">Recommendation summary</span></span>
<span data-ttu-id="d17de-486">hello alábbi lista tartalmaz összefoglaló információkat a jelen témakörben bemutatott javaslatok kötegelés hello:</span><span class="sxs-lookup"><span data-stu-id="d17de-486">hello following list provides a summary of hello batching recommendations discussed in this topic:</span></span>

* <span data-ttu-id="d17de-487">Pufferelés és kötegelés tooincrease hello teljesítményét és méretezhetőségét SQL adatbázis-alkalmazások használata.</span><span class="sxs-lookup"><span data-stu-id="d17de-487">Use buffering and batching tooincrease hello performance and scalability of SQL Database applications.</span></span>
* <span data-ttu-id="d17de-488">Ismerje meg, hello mellékhatásokkal kötegelés/pufferelés és a rugalmasság között.</span><span class="sxs-lookup"><span data-stu-id="d17de-488">Understand hello tradeoffs between batching/buffering and resiliency.</span></span> <span data-ttu-id="d17de-489">Adott szerepkör meghibásodás során egy feldolgozatlan kötegelt az üzleti szempontból kritikus fontosságú adatok elvesztését kockáztatja hello hello teljesítmény előnye, hogy a kötegelés előfordulhat, hogy járó.</span><span class="sxs-lookup"><span data-stu-id="d17de-489">During a role failure, hello risk of losing an unprocessed batch of business-critical data might outweigh hello performance benefit of batching.</span></span>
* <span data-ttu-id="d17de-490">Kísérlet történt tookeep összes hívások toohello adatbázis egyetlen datacenter tooreduce késleltetése belül.</span><span class="sxs-lookup"><span data-stu-id="d17de-490">Attempt tookeep all calls toohello database within a single datacenter tooreduce latency.</span></span>
* <span data-ttu-id="d17de-491">Ha úgy dönt, hogy egyetlen kötegelési technika, tábla értékű paraméter kínálnak hello legjobb teljesítményt és rugalmasságot biztosít.</span><span class="sxs-lookup"><span data-stu-id="d17de-491">If you choose a single batching technique, table-valued parameters offer hello best performance and flexibility.</span></span>
* <span data-ttu-id="d17de-492">Hello leggyorsabb beszúrása teljesítmény, kövesse az alábbi általános irányelveket, de a forgatókönyv teszteléséhez:</span><span class="sxs-lookup"><span data-stu-id="d17de-492">For hello fastest insert performance, follow these general guidelines but test your scenario:</span></span>
  * <span data-ttu-id="d17de-493">< 100 sorai egy paraméteres INSERT parancs használata.</span><span class="sxs-lookup"><span data-stu-id="d17de-493">For < 100 rows, use a single parameterized INSERT command.</span></span>
  * <span data-ttu-id="d17de-494">A < 1000 sor használja a táblázat értékű paramétereket.</span><span class="sxs-lookup"><span data-stu-id="d17de-494">For < 1000 rows, use table-valued parameters.</span></span>
  * <span data-ttu-id="d17de-495">A > = 1000 sorok, SqlBulkCopy használja.</span><span class="sxs-lookup"><span data-stu-id="d17de-495">For >= 1000 rows, use SqlBulkCopy.</span></span>
* <span data-ttu-id="d17de-496">A és törlési műveletek frissítéséhez használja tábla értékű paraméter tárolt eljárás logikával határozza meg, hogy a helyes műveletet hello minden egyes sorára hello tábla paraméter.</span><span class="sxs-lookup"><span data-stu-id="d17de-496">For update and delete operations, use table-valued parameters with stored procedure logic that determines hello correct operation on each row in hello table parameter.</span></span>
* <span data-ttu-id="d17de-497">Köteg mérete irányelveket:</span><span class="sxs-lookup"><span data-stu-id="d17de-497">Batch size guidelines:</span></span>
  * <span data-ttu-id="d17de-498">Hello legnagyobb kötegelt méretek számára az alkalmazás- és üzleti követelmények célszerű használni.</span><span class="sxs-lookup"><span data-stu-id="d17de-498">Use hello largest batch sizes that make sense for your application and business requirements.</span></span>
  * <span data-ttu-id="d17de-499">Nagy kötegek egyenleg hello teljesítmény hello kockázatot jelentő ideiglenes vagy végzetes hibák ettől kezdve.</span><span class="sxs-lookup"><span data-stu-id="d17de-499">Balance hello performance gain of large batches with hello risks of temporary or catastrophic failures.</span></span> <span data-ttu-id="d17de-500">Mi az az újrapróbálkozások hello következménye és hello hello kötegben adatvesztés?</span><span class="sxs-lookup"><span data-stu-id="d17de-500">What is hello consequence of retries or loss of hello data in hello batch?</span></span> 
  * <span data-ttu-id="d17de-501">Hello legnagyobb köteg mérete tooverify, hogy SQL-adatbázis nem elutasítania tesztelése.</span><span class="sxs-lookup"><span data-stu-id="d17de-501">Test hello largest batch size tooverify that SQL Database does not reject it.</span></span>
  * <span data-ttu-id="d17de-502">A vezérlő kötegelés, például hello kötegméret vagy hello pufferelési időkerete konfigurációs beállítások létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d17de-502">Create configuration settings that control batching, such as hello batch size or hello buffering time window.</span></span> <span data-ttu-id="d17de-503">Ezek a beállítások rugalmasságot biztosít.</span><span class="sxs-lookup"><span data-stu-id="d17de-503">These settings provide flexibility.</span></span> <span data-ttu-id="d17de-504">Hello viselkedésnek a termelési kötegelés hello felhőalapú szolgáltatás ismételt üzembe helyezésével módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="d17de-504">You can change hello batching behavior in production without redeploying hello cloud service.</span></span>
* <span data-ttu-id="d17de-505">Kerülje a párhuzamos végrehajtás kötegek, amely több adatbázis egyetlen táblájára működik.</span><span class="sxs-lookup"><span data-stu-id="d17de-505">Avoid parallel execution of batches that operate on a single table in one database.</span></span> <span data-ttu-id="d17de-506">Ha toodivide a kötegek között több munkavégző szál, futtassa a tesztek toodetermine hello ideális szálak száma.</span><span class="sxs-lookup"><span data-stu-id="d17de-506">If you do choose toodivide a single batch across multiple worker threads, run tests toodetermine hello ideal number of threads.</span></span> <span data-ttu-id="d17de-507">Után egy nem meghatározott küszöbértéket több szál fog miatta a teljesítmény, nem pedig növeli azt.</span><span class="sxs-lookup"><span data-stu-id="d17de-507">After an unspecified threshold, more threads will decrease performance rather than increase it.</span></span>
* <span data-ttu-id="d17de-508">Vegye figyelembe a pufferelés méret és így további forgatókönyvek kötegelés végrehajtási idő.</span><span class="sxs-lookup"><span data-stu-id="d17de-508">Consider buffering on size and time as a way of implementing batching for more scenarios.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d17de-509">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d17de-509">Next steps</span></span>
<span data-ttu-id="d17de-510">Ez a cikk arra irányul, hogy hogyan adatbázis tervezési és kódolási technikák kapcsolódó toobatching javíthatja az alkalmazás teljesítményét és méretezhetőségét.</span><span class="sxs-lookup"><span data-stu-id="d17de-510">This article focused on how database design and coding techniques related toobatching can improve your application performance and scalability.</span></span> <span data-ttu-id="d17de-511">Ez azonban csak egy tényező az általános stratégiában.</span><span class="sxs-lookup"><span data-stu-id="d17de-511">But this is just one factor in your overall strategy.</span></span> <span data-ttu-id="d17de-512">További módszereket tooimprove teljesítményét és méretezhetőségét, lásd: [Azure SQL Database teljesítményét útmutatást az önálló adatbázisok](sql-database-performance-guidance.md) és [rugalmas készletek ára és teljesítménye szempontok](sql-database-elastic-pool-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="d17de-512">For more ways tooimprove performance and scalability, see [Azure SQL Database performance guidance for single databases](sql-database-performance-guidance.md) and [Price and performance considerations for an elastic pool](sql-database-elastic-pool-guidance.md).</span></span>

