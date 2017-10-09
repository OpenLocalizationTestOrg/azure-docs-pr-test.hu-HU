---
title: "az Azure SQL Database aaaQuery teljesítmény insights |} Microsoft Docs"
description: "A legtöbb CPU-felhasználása lekérdezi az Azure SQL-adatbázis hello lekérdezési teljesítmény figyeléséhez azonosítja."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: c2f580b2-3835-453f-89f5-140e02dd2ea7
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/05/2017
ms.author: sstein
ms.openlocfilehash: 01cca26f85193c679365585cd676449c9db00e1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-query-performance-insight"></a><span data-ttu-id="2dcff-103">Az Azure SQL adatbázis-lekérdezési Terheléselemző</span><span class="sxs-lookup"><span data-stu-id="2dcff-103">Azure SQL Database Query Performance Insight</span></span>
<span data-ttu-id="2dcff-104">Kezelése és a relációs adatbázisok hello teljesítményének hangolása jelentős szakértelmét és az idő befektetési igénylő nehéz feladat.</span><span class="sxs-lookup"><span data-stu-id="2dcff-104">Managing and tuning hello performance of relational databases is a challenging task that requires significant expertise and time investment.</span></span> <span data-ttu-id="2dcff-105">Lekérdezési teljesítmény elemzését teszi lehetővé toospend kevesebb időt hibaelhárítási adatbázis teljesítményét, adja meg a következő hello:</span><span class="sxs-lookup"><span data-stu-id="2dcff-105">Query Performance Insight allows you toospend less time troubleshooting database performance by providing hello following:</span></span>

* <span data-ttu-id="2dcff-106">Az adatbázisok (DTU) erőforrás-felhasználás mélyebb betekintést.</span><span class="sxs-lookup"><span data-stu-id="2dcff-106">Deeper insight into your databases resource (DTU) consumption.</span></span> 
* <span data-ttu-id="2dcff-107">hello leggyakoribb lekérdezések szerinti CPU/időtartama/végrehajtás, amely potenciálisan a jobb teljesítmény kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="2dcff-107">hello top queries by CPU/Duration/Execution count, which can potentially be tuned for improved performance.</span></span>
* <span data-ttu-id="2dcff-108">képes toodrill hello le hello részleteinek lekérdezés, és tekintse meg a szöveg- és erőforrás-használat előzményeit.</span><span class="sxs-lookup"><span data-stu-id="2dcff-108">hello ability toodrill down into hello details of a query, view its text and history of resource utilization.</span></span> 
* <span data-ttu-id="2dcff-109">Teljesítményhangolás által végrehajtott műveleteket megjelenítő jegyzetek [SQL Azure Database Advisor](sql-database-advisor.md)</span><span class="sxs-lookup"><span data-stu-id="2dcff-109">Performance tuning annotations that show actions performed by [SQL Azure Database Advisor](sql-database-advisor.md)</span></span>  



## <a name="prerequisites"></a><span data-ttu-id="2dcff-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2dcff-110">Prerequisites</span></span>
* <span data-ttu-id="2dcff-111">A lekérdezési Terheléselemző megköveteli, hogy [Lekérdezéstár](https://msdn.microsoft.com/library/dn817826.aspx) aktív az adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="2dcff-111">Query Performance Insight requires that [Query Store](https://msdn.microsoft.com/library/dn817826.aspx) is active on your database.</span></span> <span data-ttu-id="2dcff-112">A Lekérdezéstár nem fut, ha hello portal kéri tooturn azt meg.</span><span class="sxs-lookup"><span data-stu-id="2dcff-112">If Query Store is not running, hello portal prompts you tooturn it on.</span></span>

## <a name="permissions"></a><span data-ttu-id="2dcff-113">Engedélyek</span><span class="sxs-lookup"><span data-stu-id="2dcff-113">Permissions</span></span>
<span data-ttu-id="2dcff-114">hello következő [szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-what-is.md) engedélyekre szükség toouse lekérdezési Terheléselemző:</span><span class="sxs-lookup"><span data-stu-id="2dcff-114">hello following [role-based access control](../active-directory/role-based-access-control-what-is.md) permissions are required toouse Query Performance Insight:</span></span> 

* <span data-ttu-id="2dcff-115">**Olvasó**, **tulajdonos**, **közreműködő**, **SQL DB Contributor**, vagy **SQL Server közreműködői** engedélyek szükséges tooview hello felső erőforrás fel a lekérdezések és diagramokat.</span><span class="sxs-lookup"><span data-stu-id="2dcff-115">**Reader**, **Owner**, **Contributor**, **SQL DB Contributor**, or **SQL Server Contributor** permissions are required tooview hello top resource consuming queries and charts.</span></span> 
* <span data-ttu-id="2dcff-116">**Tulajdonos**, **közreműködő**, **SQL DB Contributor**, vagy **SQL Server közreműködői** engedélyekre szükség tooview lekérdezés szövegét.</span><span class="sxs-lookup"><span data-stu-id="2dcff-116">**Owner**, **Contributor**, **SQL DB Contributor**, or **SQL Server Contributor** permissions are required tooview query text.</span></span>

## <a name="using-query-performance-insight"></a><span data-ttu-id="2dcff-117">Lekérdezési Terheléselemző használatával</span><span class="sxs-lookup"><span data-stu-id="2dcff-117">Using Query Performance Insight</span></span>
<span data-ttu-id="2dcff-118">Lekérdezési Terheléselemző könnyen toouse:</span><span class="sxs-lookup"><span data-stu-id="2dcff-118">Query Performance Insight is easy toouse:</span></span>

* <span data-ttu-id="2dcff-119">Nyissa meg [Azure-portálon](https://portal.azure.com/) és a keresési adatbázis, amelyet az tooexamine.</span><span class="sxs-lookup"><span data-stu-id="2dcff-119">Open [Azure portal](https://portal.azure.com/) and find database that you want tooexamine.</span></span> 
  * <span data-ttu-id="2dcff-120">A bal oldali menüben, a támogatási és hibaelhárítási válassza a "Lekérdezési Terheléselemző".</span><span class="sxs-lookup"><span data-stu-id="2dcff-120">From left-hand side menu, under support and troubleshooting, select “Query Performance Insight”.</span></span>
* <span data-ttu-id="2dcff-121">Hello első lapján tekintse át a legfelső szintű erőforrás-igényes lekérdezések hello listáját.</span><span class="sxs-lookup"><span data-stu-id="2dcff-121">On hello first tab, review hello list of top resource-consuming queries.</span></span>
* <span data-ttu-id="2dcff-122">Válassza ki az egyes lekérdezések tooview hozzá tartozó részletek.</span><span class="sxs-lookup"><span data-stu-id="2dcff-122">Select an individual query tooview its details.</span></span>
* <span data-ttu-id="2dcff-123">Nyissa meg [SQL Azure Database Advisor](sql-database-advisor.md) és ellenőrizze, hogy e javaslatokkal érhető el.</span><span class="sxs-lookup"><span data-stu-id="2dcff-123">Open [SQL Azure Database Advisor](sql-database-advisor.md) and check if any recommendations are available.</span></span>
* <span data-ttu-id="2dcff-124">Csúszkákkal vagy időköz megfigyelt ikonok toochange nagyítás.</span><span class="sxs-lookup"><span data-stu-id="2dcff-124">Use sliders or zoom icons toochange observed interval.</span></span>
  
    ![teljesítmény irányítópult](./media/sql-database-query-performance/performance.png)

> [!NOTE]
> <span data-ttu-id="2dcff-126">Néhány órányi adatot kell a SQL Database tooprovide lekérdezési terheléselemző a Lekérdezéstár által rögzített toobe.</span><span class="sxs-lookup"><span data-stu-id="2dcff-126">A couple hours of data needs toobe captured by Query Store for SQL Database tooprovide query performance insights.</span></span> <span data-ttu-id="2dcff-127">Ha hello adatbázis nincs tevékenység vagy a Lekérdezéstár nem volt aktív egy bizonyos időn belül, hello diagramok üres lesz az adott időszak megjelenítésekor.</span><span class="sxs-lookup"><span data-stu-id="2dcff-127">If hello database has no activity or Query Store was not active during a certain time period, hello charts will be empty when displaying that time period.</span></span> <span data-ttu-id="2dcff-128">A Lekérdezéstár engedélyezheti bármikor, ha az nem futna.</span><span class="sxs-lookup"><span data-stu-id="2dcff-128">You may enable Query Store at any time if it is not running.</span></span>   
> 
> 

## <a name="review-top-cpu-consuming-queries"></a><span data-ttu-id="2dcff-129">Tekintse át a erőforrásigényes lekérdezések felső Processzor</span><span class="sxs-lookup"><span data-stu-id="2dcff-129">Review top CPU consuming queries</span></span>
<span data-ttu-id="2dcff-130">A hello [portal](http://portal.azure.com) hello a következő:</span><span class="sxs-lookup"><span data-stu-id="2dcff-130">In hello [portal](http://portal.azure.com) do hello following:</span></span>

1. <span data-ttu-id="2dcff-131">Keresse meg az SQL-adatbázis tooa, és kattintson a **összes beállítás** > **támogatási + hibaelhárítás** > **lekérdezési terheléselemzőhöz**.</span><span class="sxs-lookup"><span data-stu-id="2dcff-131">Browse tooa SQL database and click **All settings** > **Support + Troubleshooting** > **Query performance insight**.</span></span> 
   
    ![Lekérdezési terheléselemző][1]
   
    <span data-ttu-id="2dcff-133">hello leggyakoribb lekérdezések nézet megnyitása és hello felső CPU fogyasztó lekérdezések találhatók.</span><span class="sxs-lookup"><span data-stu-id="2dcff-133">hello top queries view opens and hello top CPU consuming queries are listed.</span></span>
2. <span data-ttu-id="2dcff-134">Kattintson a részletek hello diagram körül.</span><span class="sxs-lookup"><span data-stu-id="2dcff-134">Click around hello chart for details.</span></span><br><span data-ttu-id="2dcff-135">hello felső sor mutatja összesített DTU % hello adatbázishoz, amíg hello sávok megjelenítése hello kijelölt időszakban kijelölt hello lekérdezések által használt CPU % (például, ha **elmúlt hét** kijelölt minden sáv jelöli egy nap).</span><span class="sxs-lookup"><span data-stu-id="2dcff-135">hello top line shows overall DTU% for hello database, while hello bars show CPU% consumed by hello selected queries during hello selected interval (for example, if **Past week** is selected each bar represents one day).</span></span>
   
    ![Leggyakoribb lekérdezések][2]
   
    <span data-ttu-id="2dcff-137">hello alsó rács hello látható lekérdezések összesített adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="2dcff-137">hello bottom grid represents aggregated information for hello visible queries.</span></span>
   
   * <span data-ttu-id="2dcff-138">Lekérdezésazonosítóval - lekérdezés adatbázis belül egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="2dcff-138">Query ID - unique identifier of query inside database.</span></span>
   * <span data-ttu-id="2dcff-139">CPU-t (aggregátumfüggvény függ) lekérdezés megfigyelhető időköze alatt kerülne sor.</span><span class="sxs-lookup"><span data-stu-id="2dcff-139">CPU per query during observable interval (depends on aggregation function).</span></span>
   * <span data-ttu-id="2dcff-140">Lekérdezésenként időtartama (aggregátumfüggvény függ).</span><span class="sxs-lookup"><span data-stu-id="2dcff-140">Duration per query (depends on aggregation function).</span></span>
   * <span data-ttu-id="2dcff-141">Egy adott lekérdezés végrehajtások teljes száma.</span><span class="sxs-lookup"><span data-stu-id="2dcff-141">Total number of executions for a particular query.</span></span>
     
     <span data-ttu-id="2dcff-142">Válassza ki, vagy törölje az egyes lekérdezések tooinclude, illetve kizárását őket hello diagram checkboxes használata.</span><span class="sxs-lookup"><span data-stu-id="2dcff-142">Select or clear individual queries tooinclude or exclude them from hello chart using checkboxes.</span></span>
3. <span data-ttu-id="2dcff-143">Ha az adatok elavulttá válik, kattintson a hello **frissítése** gombra.</span><span class="sxs-lookup"><span data-stu-id="2dcff-143">If your data becomes stale, click hello **Refresh** button.</span></span>
4. <span data-ttu-id="2dcff-144">Vizsgálja meg a teljesítményt és használhatja a csúszkák és nagyítás gombok toochange megfigyelési időközben: ![beállítások](./media/sql-database-query-performance/zoom.png)</span><span class="sxs-lookup"><span data-stu-id="2dcff-144">You can use sliders and zoom buttons toochange observation interval and investigate spikes:  ![settings](./media/sql-database-query-performance/zoom.png)</span></span>
5. <span data-ttu-id="2dcff-145">Ha szükséges, ha azt szeretné, hogy egy másik nézetet, válassza **egyéni** lapra, és állítsa be:</span><span class="sxs-lookup"><span data-stu-id="2dcff-145">Optionally, if you want a different view, you can select **Custom** tab and set:</span></span>
   
   * <span data-ttu-id="2dcff-146">A metrika (CPU, időtartama, végrehajtási száma)</span><span class="sxs-lookup"><span data-stu-id="2dcff-146">Metric (CPU, duration, execution count)</span></span>
   * <span data-ttu-id="2dcff-147">Időtartam (utolsó 24 óra, elmúlt hét elmúlt hónap).</span><span class="sxs-lookup"><span data-stu-id="2dcff-147">Time interval (Last 24 hours, Past week, Past month).</span></span> 
   * <span data-ttu-id="2dcff-148">Lekérdezések száma.</span><span class="sxs-lookup"><span data-stu-id="2dcff-148">Number of queries.</span></span>
   * <span data-ttu-id="2dcff-149">Aggregátumfüggvény.</span><span class="sxs-lookup"><span data-stu-id="2dcff-149">Aggregation function.</span></span>
     
     ![beállítások](./media/sql-database-query-performance/custom-tab.png)

## <a name="viewing-individual-query-details"></a><span data-ttu-id="2dcff-151">Egyes lekérdezések részleteinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="2dcff-151">Viewing individual query details</span></span>
<span data-ttu-id="2dcff-152">tooview lekérdezés részletei:</span><span class="sxs-lookup"><span data-stu-id="2dcff-152">tooview query details:</span></span>

1. <span data-ttu-id="2dcff-153">Kattintson az összes lekérdezés hello lista leggyakoribb lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="2dcff-153">Click any query in hello list of top queries.</span></span>
   
    ![Részletek](./media/sql-database-query-performance/details.png)
2. <span data-ttu-id="2dcff-155">hello Részletek nézet megnyitása és hello lekérdezések fogyasztás/időtartama/végrehajtás processzorszám időbeli bontásban.</span><span class="sxs-lookup"><span data-stu-id="2dcff-155">hello details view opens and hello queries CPU consumption/Duration/Execution count is broken down over time.</span></span>
3. <span data-ttu-id="2dcff-156">Kattintson a részletek hello diagram körül.</span><span class="sxs-lookup"><span data-stu-id="2dcff-156">Click around hello chart for details.</span></span>
   
   * <span data-ttu-id="2dcff-157">Sor általános adatbázis DTU % a felső diagram ábrázolja, és hello sávok hello kijelölt lekérdezés által használt CPU %.</span><span class="sxs-lookup"><span data-stu-id="2dcff-157">Top chart shows line with overall database DTU%, and hello bars are CPU% consumed by hello selected query.</span></span>
   * <span data-ttu-id="2dcff-158">Második diagram hello kijelölt lekérdezés által teljes időtartam látható.</span><span class="sxs-lookup"><span data-stu-id="2dcff-158">Second chart shows total duration by hello selected query.</span></span>
   * <span data-ttu-id="2dcff-159">Alsó diagram hello kijelölt lekérdezés által végrehajtások összesített számát mutatja.</span><span class="sxs-lookup"><span data-stu-id="2dcff-159">Bottom chart shows total number of executions by hello selected query.</span></span>
     
     ![Lekérdezés részletei][3]
4. <span data-ttu-id="2dcff-161">Másik lehetőségként csúszkákkal, Nagyítás gomb, vagy kattintson a **beállítások** toocustomize lekérdezési adatok megjelenítési módját, vagy egy másik időszakra toopick.</span><span class="sxs-lookup"><span data-stu-id="2dcff-161">Optionally, use sliders, zoom buttons or click **Settings** toocustomize how query data is displayed, or toopick a different time period.</span></span>

## <a name="review-top-queries-per-duration"></a><span data-ttu-id="2dcff-162">Tekintse át a felső lekérdezések száma időtartama</span><span class="sxs-lookup"><span data-stu-id="2dcff-162">Review top queries per duration</span></span>
<span data-ttu-id="2dcff-163">Hello legutóbbi frissítését lekérdezési teljesítmény elemzését, azt, amelyik segíthet a potenciális szűk keresztmetszetek azonosítása két új mérőszámok bevezetett: duration és végrehajtási számát.</span><span class="sxs-lookup"><span data-stu-id="2dcff-163">In hello recent update of Query Performance Insight, we introduced two new metrics that can help you identify potential bottlenecks: duration and execution count.</span></span><br>

<span data-ttu-id="2dcff-164">Hosszan futó lekérdezések hello legnagyobb lehetséges, hogy hosszabb erőforrások zárolása, más felhasználók számára, és korlátozza a méretezhetőség rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="2dcff-164">Long-running queries have hello greatest potential for locking resources longer, blocking other users, and limiting scalability.</span></span> <span data-ttu-id="2dcff-165">Azok a-zel is hello legjobb optimalizálás.</span><span class="sxs-lookup"><span data-stu-id="2dcff-165">They are also hello best candidates for optimization.</span></span><br>

<span data-ttu-id="2dcff-166">hosszú ideig futó lekérdezések tooidentify:</span><span class="sxs-lookup"><span data-stu-id="2dcff-166">tooidentify long running queries:</span></span>

1. <span data-ttu-id="2dcff-167">Nyissa meg **egyéni** lapon lekérdezési Terheléselemző a kiválasztott adatbázishoz</span><span class="sxs-lookup"><span data-stu-id="2dcff-167">Open **Custom** tab in Query Performance Insight for selected database</span></span>
2. <span data-ttu-id="2dcff-168">Módosítsa a metrikák toobe **időtartama**</span><span class="sxs-lookup"><span data-stu-id="2dcff-168">Change metrics toobe **duration**</span></span>
3. <span data-ttu-id="2dcff-169">Válassza ki a lekérdezések és megfigyelési időközben</span><span class="sxs-lookup"><span data-stu-id="2dcff-169">Select number of queries and observation interval</span></span>
4. <span data-ttu-id="2dcff-170">Összesítő függvény kiválasztása</span><span class="sxs-lookup"><span data-stu-id="2dcff-170">Select aggregation function</span></span>
   
   * <span data-ttu-id="2dcff-171">**Sum** hozzáadja az összes lekérdezés végrehajtási idő teljes megfigyelési időközben során.</span><span class="sxs-lookup"><span data-stu-id="2dcff-171">**Sum** adds up all query execution time during whole observation interval.</span></span>
   * <span data-ttu-id="2dcff-172">**Maximális** megkeresi a lekérdezések teljes megfigyelési időközben maximális korábban mely végrehajtási idő.</span><span class="sxs-lookup"><span data-stu-id="2dcff-172">**Max** finds queries which execution time was maximum at whole observation interval.</span></span>
   * <span data-ttu-id="2dcff-173">**Átlagos** átlagos végrehajtási idő az összes lekérdezési végrehajtások, és bemutatják, hello felső ezek átlagok kívül.</span><span class="sxs-lookup"><span data-stu-id="2dcff-173">**Avg** finds average execution time of all query executions and show you hello top out of these averages.</span></span> 
     
     ![lekérdezés időtartama][4]

## <a name="review-top-queries-per-execution-count"></a><span data-ttu-id="2dcff-175">Tekintse át a felső lekérdezések száma végrehajtási száma</span><span class="sxs-lookup"><span data-stu-id="2dcff-175">Review top queries per execution count</span></span>
<span data-ttu-id="2dcff-176">Végrehajtások nagy száma lehet, hogy nem kell érintő maga adatbázis és erőforrás-használat alacsony lehet, de alkalmazás általános juthat, lassú.</span><span class="sxs-lookup"><span data-stu-id="2dcff-176">High number of executions might not be affecting database itself and resources usage can be low, but overall application might get slow.</span></span>

<span data-ttu-id="2dcff-177">Bizonyos esetekben végrehajtási nagyon nagy száma azt eredményezheti, hálózat tooincrease kiszolgálókkal való adatváltások számát.</span><span class="sxs-lookup"><span data-stu-id="2dcff-177">In some cases, very high execution count may lead tooincrease of network round trips.</span></span> <span data-ttu-id="2dcff-178">Adatváltások jelentős mértékben befolyásolhatja a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="2dcff-178">Round trips significantly affect performance.</span></span> <span data-ttu-id="2dcff-179">Tulajdonos toonetwork késleltetés és a kiszolgáló késleltetése toodownstream.</span><span class="sxs-lookup"><span data-stu-id="2dcff-179">They are subject toonetwork latency and toodownstream server latency.</span></span> 

<span data-ttu-id="2dcff-180">Például számos adatvezérelt webhely fokozottan érik el hello adatbázist minden felhasználói kérelem esetén.</span><span class="sxs-lookup"><span data-stu-id="2dcff-180">For example, many data-driven Web sites heavily access hello database for every user request.</span></span> <span data-ttu-id="2dcff-181">Kapcsolatkészlet nyújt segítséget, hello megnövekedett hálózati forgalmat, és hello adatbázis server feldolgozási terhelését is hátrányosan befolyásolhatja a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="2dcff-181">While connection pooling helps, hello increased network traffic and processing load on hello database server can adversely affect performance.</span></span>  <span data-ttu-id="2dcff-182">Általános útmutatásként round utazgatással tooan lehető legegyszerűbb tookeep.</span><span class="sxs-lookup"><span data-stu-id="2dcff-182">General advice is tookeep round trips tooan absolute minimum.</span></span>

<span data-ttu-id="2dcff-183">tooidentify gyakran hajtotta végre a lekérdezéseket ("chatty") lekérdezéseket:</span><span class="sxs-lookup"><span data-stu-id="2dcff-183">tooidentify frequently executed queries (“chatty”) queries:</span></span>

1. <span data-ttu-id="2dcff-184">Nyissa meg **egyéni** lapon lekérdezési Terheléselemző a kiválasztott adatbázishoz</span><span class="sxs-lookup"><span data-stu-id="2dcff-184">Open **Custom** tab in Query Performance Insight for selected database</span></span>
2. <span data-ttu-id="2dcff-185">Módosítsa a metrikák toobe **végrehajtási száma**</span><span class="sxs-lookup"><span data-stu-id="2dcff-185">Change metrics toobe **execution count**</span></span>
3. <span data-ttu-id="2dcff-186">Válassza ki a lekérdezések és megfigyelési időközben</span><span class="sxs-lookup"><span data-stu-id="2dcff-186">Select number of queries and observation interval</span></span>
   
    ![lekérdezés-végrehajtási száma][5]

## <a name="understanding-performance-tuning-annotations"></a><span data-ttu-id="2dcff-188">Teljesítmény hangolási jegyzetek ismertetése</span><span class="sxs-lookup"><span data-stu-id="2dcff-188">Understanding performance tuning annotations</span></span>
<span data-ttu-id="2dcff-189">Tervezi a terhelést a lekérdezési teljesítmény elemzését, miközben bizonyára észrevette, hogy a függőleges vonal fölött hello diagram ikonok.</span><span class="sxs-lookup"><span data-stu-id="2dcff-189">While exploring your workload in Query Performance Insight, you might notice icons with vertical line on top of hello chart.</span></span><br>

<span data-ttu-id="2dcff-190">Ezekkel az ikonokkal jegyzetek; érintő által végrehajtott műveletek teljesítményének képviselnek [SQL Azure Database Advisor](sql-database-advisor.md).</span><span class="sxs-lookup"><span data-stu-id="2dcff-190">These icons are annotations; they represent performance affecting actions performed by [SQL Azure Database Advisor](sql-database-advisor.md).</span></span> <span data-ttu-id="2dcff-191">Rámutató jegyzet által kapott hello művelet alapvető információkat:</span><span class="sxs-lookup"><span data-stu-id="2dcff-191">By hovering annotation, you get basic information about hello action:</span></span>

![lekérdezés Megjegyzés][6]

<span data-ttu-id="2dcff-193">Ha szeretné, hogy további tooknow, vagy az advisor ajánlás alkalmazható, kattintson a hello ikonra.</span><span class="sxs-lookup"><span data-stu-id="2dcff-193">If you want tooknow more or apply advisor recommendation, click hello icon.</span></span> <span data-ttu-id="2dcff-194">Az a művelet részleteit nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="2dcff-194">It will open details of action.</span></span> <span data-ttu-id="2dcff-195">Ha egy aktív adott alkalmazhatja azonnal paranccsal.</span><span class="sxs-lookup"><span data-stu-id="2dcff-195">If it’s an active recommendation you can apply it straight away using command.</span></span>

![lekérdezés jegyzet részletei][7]

### <a name="multiple-annotations"></a><span data-ttu-id="2dcff-197">Több megjegyzés.</span><span class="sxs-lookup"><span data-stu-id="2dcff-197">Multiple annotations.</span></span>
<span data-ttu-id="2dcff-198">Azonban lehetséges, hogy nagyítási szintjét, mert lévő más Bezárás tooeach fogja lekérni összecsukott valamelyikébe.</span><span class="sxs-lookup"><span data-stu-id="2dcff-198">It’s possible, that because of zoom level, annotations that are close tooeach other will get collapsed into one.</span></span> <span data-ttu-id="2dcff-199">Ez különleges ikon fog megjelenni, kattintson rá fog megnyitása új panel, ahol csoportosított listája a jegyzetek megjelenik.</span><span class="sxs-lookup"><span data-stu-id="2dcff-199">This will be represented by special icon, clicking it will open new blade where list of grouped annotations will be shown.</span></span>
<span data-ttu-id="2dcff-200">Adatok, lekérdezések és teljesítményének hangolása műveletek segítségével toobetter a számítási feladatok ismertetése.</span><span class="sxs-lookup"><span data-stu-id="2dcff-200">Correlating queries and performance tuning actions can help toobetter understand your workload.</span></span> 

## <a name="optimizing-hello-query-store-configuration-for-query-performance-insight"></a><span data-ttu-id="2dcff-201">Hello Lekérdezéstár konfigurációs betekintés a lekérdezési teljesítmény optimalizálása</span><span class="sxs-lookup"><span data-stu-id="2dcff-201">Optimizing hello Query Store configuration for Query Performance Insight</span></span>
<span data-ttu-id="2dcff-202">A lekérdezési Terheléselemző felhasználása során léphetnek fel a következő Lekérdezéstár üzenetek hello:</span><span class="sxs-lookup"><span data-stu-id="2dcff-202">During your use of Query Performance Insight, you might encounter hello following Query Store messages:</span></span>

* <span data-ttu-id="2dcff-203">"A Lekérdezéstár nincs megfelelően konfigurálva ezen az adatbázison.</span><span class="sxs-lookup"><span data-stu-id="2dcff-203">"Query Store is not properly configured on this database.</span></span> <span data-ttu-id="2dcff-204">Kattintson ide további toolearn."</span><span class="sxs-lookup"><span data-stu-id="2dcff-204">Click here toolearn more."</span></span>
* <span data-ttu-id="2dcff-205">"A Lekérdezéstár nincs megfelelően konfigurálva ezen az adatbázison.</span><span class="sxs-lookup"><span data-stu-id="2dcff-205">"Query Store is not properly configured on this database.</span></span> <span data-ttu-id="2dcff-206">Kattintson ide a beállítások toochange."</span><span class="sxs-lookup"><span data-stu-id="2dcff-206">Click here toochange settings."</span></span> 

<span data-ttu-id="2dcff-207">Ezek az üzenetek általában jelennek meg, ha a Lekérdezéstár nem tud toocollect új adatokat.</span><span class="sxs-lookup"><span data-stu-id="2dcff-207">These messages usually appear when Query Store is not able toocollect new data.</span></span> 

<span data-ttu-id="2dcff-208">Első esetben történik, ha a Lekérdezéstár csak olvasható állapotban van, és paraméterei optimálisan vannak beállítva.</span><span class="sxs-lookup"><span data-stu-id="2dcff-208">First case happens when Query Store is in Read-Only state and parameters are set optimally.</span></span> <span data-ttu-id="2dcff-209">A hibát a Lekérdezéstár méretének növelését, vagy a jelölés törlésével a Lekérdezéstár.</span><span class="sxs-lookup"><span data-stu-id="2dcff-209">You can fix this by increasing size of Query Store or clearing Query Store.</span></span>

![qds gomb][8]

<span data-ttu-id="2dcff-211">Második esetben történik, ha a Lekérdezéstár le van tiltva, vagy a paraméterek nincsenek beállítva optimális.</span><span class="sxs-lookup"><span data-stu-id="2dcff-211">Second case happens when Query Store is Off or parameters aren’t set optimally.</span></span> <br><span data-ttu-id="2dcff-212">A következő az alábbi parancsok futtatásával, vagy közvetlenül a portálon hello rögzítése és megőrzési házirend, és engedélyezze a Lekérdezéstár módosíthatja:</span><span class="sxs-lookup"><span data-stu-id="2dcff-212">You can change hello Retention and Capture policy and enable Query Store by executing commands below or directly from portal:</span></span>

![qds gomb][9]

### <a name="recommended-retention-and-capture-policy"></a><span data-ttu-id="2dcff-214">Ajánlott rögzítése és az adatmegőrzési házirend</span><span class="sxs-lookup"><span data-stu-id="2dcff-214">Recommended retention and capture policy</span></span>
<span data-ttu-id="2dcff-215">Az adatmegőrzési két típusa van:</span><span class="sxs-lookup"><span data-stu-id="2dcff-215">There are two types of retention policies:</span></span>

* <span data-ttu-id="2dcff-216">Mérete alapján - set tooAUTO azt megtisztítja automatikusan, ha közel maximális méretét adatok elérésekor.</span><span class="sxs-lookup"><span data-stu-id="2dcff-216">Size based - if set tooAUTO it will clean data automatically when near max size is reached.</span></span>
* <span data-ttu-id="2dcff-217">-Alapú idő azt állítja be az alapértelmezés szerint too30 nap, ami azt jelenti, ha a Lekérdezéstár futtatandó nincs elegendő lemezterület, a művelet törli a 30 napnál régebbi adatokat lekérdezése</span><span class="sxs-lookup"><span data-stu-id="2dcff-217">Time based - by default we will set it too30 days, which means, if Query Store will run out of space, it will delete query information older than 30 days</span></span>

<span data-ttu-id="2dcff-218">Rögzítése házirend beállítható:</span><span class="sxs-lookup"><span data-stu-id="2dcff-218">Capture policy could be set to:</span></span>

* <span data-ttu-id="2dcff-219">**Minden** – összes lekérdezés rögzíti.</span><span class="sxs-lookup"><span data-stu-id="2dcff-219">**All** - Captures all queries.</span></span>
* <span data-ttu-id="2dcff-220">**Automatikus** -alkalomszerű lekérdezések és lekérdezések jelentéktelen fordítási és végrehajtási időtartamú figyelmen kívül lesznek hagyva.</span><span class="sxs-lookup"><span data-stu-id="2dcff-220">**Auto** - Infrequent queries and queries with insignificant compile and execution duration are ignored.</span></span> <span data-ttu-id="2dcff-221">Végrehajtási szám, a fordítás és a futási ideje küszöbértékek belső határozza meg.</span><span class="sxs-lookup"><span data-stu-id="2dcff-221">Thresholds for execution count, compile and runtime duration are internally determined.</span></span> <span data-ttu-id="2dcff-222">Ez a lehetőség hello alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="2dcff-222">This is hello default option.</span></span>
* <span data-ttu-id="2dcff-223">**Nincs** -Lekérdezéstár leállítja az új lekérdezések rögzítését, azonban már rögzített lekérdezések futásidejű statisztikák még gyűjtik.</span><span class="sxs-lookup"><span data-stu-id="2dcff-223">**None** - Query Store stops capturing new queries, however runtime stats for already captured queries are still collected.</span></span>

<span data-ttu-id="2dcff-224">Azt javasoljuk, hogy minden házirendek tooAUTO és tiszta házirend too30 nap beállítása:</span><span class="sxs-lookup"><span data-stu-id="2dcff-224">We recommend setting all policies tooAUTO and clean policy too30 days:</span></span>

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30));

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);

<span data-ttu-id="2dcff-225">A Lekérdezéstár méretének növeléséhez.</span><span class="sxs-lookup"><span data-stu-id="2dcff-225">Increase size of Query Store.</span></span> <span data-ttu-id="2dcff-226">Ennek oka az lehet, kapcsolódó tooa adatbázis által végrehajtott és kibocsátó a következő lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="2dcff-226">This could be performed by connecting tooa database and issuing following query:</span></span>

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);

<span data-ttu-id="2dcff-227">Ezek a beállítások alkalmazásának végül ellenőrizze a Lekérdezéstár új lekérdezések gyűjtése, azonban ha nem szeretné, hogy toowait Lekérdezéstár törlése.</span><span class="sxs-lookup"><span data-stu-id="2dcff-227">Applying these settings will eventually make Query Store collecting new queries, however if you don’t want toowait you can clear Query Store.</span></span> 

> [!NOTE]
> <span data-ttu-id="2dcff-228">A következő lekérdezés végrehajtásakor a Lekérdezéstár hello összes aktuális adatot törli.</span><span class="sxs-lookup"><span data-stu-id="2dcff-228">Executing following query will delete all current information in hello Query Store.</span></span> 
> 
> 

    ALTER DATABASE [YourDB] SET QUERY_STORE CLEAR;


## <a name="summary"></a><span data-ttu-id="2dcff-229">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="2dcff-229">Summary</span></span>
<span data-ttu-id="2dcff-230">Lekérdezési Terheléselemző segítségével megismerheti, hogy a lekérdezés-munkaterhelési hello hatását és a hálózatierőforrás-fogyasztás toodatabase kapcsolódására.</span><span class="sxs-lookup"><span data-stu-id="2dcff-230">Query Performance Insight helps you understand hello impact of your query workload and how it relates toodatabase resource consumption.</span></span> <span data-ttu-id="2dcff-231">Ezzel a szolgáltatással akkor fog információ hello leginkább erőforrásigényes lekérdezések, és ők hello toofix könnyebb azonosításához, így elkerülhetők a probléma.</span><span class="sxs-lookup"><span data-stu-id="2dcff-231">With this feature, you will learn about hello top consuming queries, and easily identify hello ones toofix before they become a problem.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2dcff-232">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2dcff-232">Next steps</span></span>
<span data-ttu-id="2dcff-233">Az SQL-adatbázis hello a teljesítmény fokozása kapcsolatos további javaslatok kattintson [javaslatok](sql-database-advisor.md) a hello **lekérdezési Terheléselemző** panelen.</span><span class="sxs-lookup"><span data-stu-id="2dcff-233">For additional recommendations about improving hello performance of your SQL database, click [Recommendations](sql-database-advisor.md) on hello **Query Performance Insight** blade.</span></span>

![Teljesítmény Advisor](./media/sql-database-query-performance/ia.png)

<!--Image references-->
[1]: ./media/sql-database-query-performance/tile.png
[2]: ./media/sql-database-query-performance/top-queries.png
[3]: ./media/sql-database-query-performance/query-details.png
[4]: ./media/sql-database-query-performance/top-duration.png
[5]: ./media/sql-database-query-performance/top-execution.png
[6]: ./media/sql-database-query-performance/annotation.png
[7]: ./media/sql-database-query-performance/annotation-details.png
[8]: ./media/sql-database-query-performance/qds-off.png
[9]: ./media/sql-database-query-performance/qds-button.png

