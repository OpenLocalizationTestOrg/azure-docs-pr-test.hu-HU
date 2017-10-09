---
title: "több Azure SQL adatbázis aaaRun elemzési lekérdezések |} Microsoft Docs"
description: "Adatok kinyerése a bérlő adatbázis kapcsolat nélküli elemzéshez analytics adatbázisba"
keywords: "sql database-oktatóanyag"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: billgib; sstein
ms.openlocfilehash: f2664e4aafd2fecc98d20d229342bca19b0b08c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="extract-data-from-tenant-databases-into-an-analytics-database-for-offline-analysis"></a><span data-ttu-id="ae2d0-104">Adatok kinyerése a bérlő adatbázis kapcsolat nélküli elemzéshez analytics adatbázisba</span><span class="sxs-lookup"><span data-stu-id="ae2d0-104">Extract data from tenant databases into an analytics database for offline analysis</span></span>

<span data-ttu-id="ae2d0-105">Ebben az oktatóanyagban egy rugalmas feladat toorun lekérdezések írásában, minden bérlői adatbázis használja.</span><span class="sxs-lookup"><span data-stu-id="ae2d0-105">In this tutorial, you use an elastic job toorun queries against each tenant database.</span></span> <span data-ttu-id="ae2d0-106">hello feladat jegy értékesítési adatait kinyeri, és betölti az elemzési adatbázis (vagy az adatraktár) elemzés céljából.</span><span class="sxs-lookup"><span data-stu-id="ae2d0-106">hello job extracts ticket sales data and loads it into an analytics database (or data warehouse) for analysis.</span></span> <span data-ttu-id="ae2d0-107">hello analytics adatbázisa majd lekérdezett tooextract információkat kaphat a napi működésiadat egyetlen bérlő számára.</span><span class="sxs-lookup"><span data-stu-id="ae2d0-107">hello analytics database is then queried tooextract insights from this day-to-day operational data of all tenants.</span></span>


<span data-ttu-id="ae2d0-108">Ezen oktatóanyag segítségével megtanulhatja a következőket:</span><span class="sxs-lookup"><span data-stu-id="ae2d0-108">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ae2d0-109">Hello bérlői analytics adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="ae2d0-109">Create hello tenant analytics database</span></span>
> * <span data-ttu-id="ae2d0-110">Hozzon létre egy ütemezett feladatot tooretrieve adatokat, és hello analytics adatbázisának feltöltése</span><span class="sxs-lookup"><span data-stu-id="ae2d0-110">Create a scheduled job tooretrieve data and populate hello analytics database</span></span>

<span data-ttu-id="ae2d0-111">Ez az oktatóanyag, győződjön meg arról, hogy hello a következő előfeltételek teljesülését toocomplete:</span><span class="sxs-lookup"><span data-stu-id="ae2d0-111">toocomplete this tutorial, make sure hello following prerequisites are met:</span></span>

* <span data-ttu-id="ae2d0-112">hello Wingtip SaaS-alkalmazás telepítve van.</span><span class="sxs-lookup"><span data-stu-id="ae2d0-112">hello Wingtip SaaS app is deployed.</span></span> <span data-ttu-id="ae2d0-113">toodeploy öt percen belül, lásd: [központi telepítése és felfedezése hello Wingtip SaaS-alkalmazáshoz](sql-database-saas-tutorial.md)</span><span class="sxs-lookup"><span data-stu-id="ae2d0-113">toodeploy in less than five minutes, see [Deploy and explore hello Wingtip SaaS application](sql-database-saas-tutorial.md)</span></span>
* <span data-ttu-id="ae2d0-114">Az Azure PowerShell telepítve van.</span><span class="sxs-lookup"><span data-stu-id="ae2d0-114">Azure PowerShell is installed.</span></span> <span data-ttu-id="ae2d0-115">A részletekért lásd: [Ismerkedés az Azure PowerShell-lel](https://docs.microsoft.com/powershell/azure/get-started-azureps)</span><span class="sxs-lookup"><span data-stu-id="ae2d0-115">For details, see [Getting started with Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)</span></span>
* <span data-ttu-id="ae2d0-116">hello legújabb verziója az SQL Server Management Studio (SSMS) van telepítve.</span><span class="sxs-lookup"><span data-stu-id="ae2d0-116">hello latest version of SQL Server Management Studio (SSMS) is installed.</span></span> [<span data-ttu-id="ae2d0-117">Az SSMS letöltése és telepítése</span><span class="sxs-lookup"><span data-stu-id="ae2d0-117">Download and Install SSMS</span></span>](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

## <a name="tenant-operational-analytics-pattern"></a><span data-ttu-id="ae2d0-118">Bérlői operatív analitikai mintázat</span><span class="sxs-lookup"><span data-stu-id="ae2d0-118">Tenant Operational Analytics pattern</span></span>

<span data-ttu-id="ae2d0-119">Hello kiváló lehetőségek SaaS-alkalmazásokhoz az egyik toouse hello gazdag bérlői tárolt adatok hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="ae2d0-119">One of hello great opportunities with SaaS applications is toouse hello rich tenant data that is stored in hello cloud.</span></span> <span data-ttu-id="ae2d0-120">Az adatok toogain betekintést hello művelet és az alkalmazás és a bérlők használatának használja.</span><span class="sxs-lookup"><span data-stu-id="ae2d0-120">Use this data toogain insights into hello operation and usage of your application, and your tenants.</span></span> <span data-ttu-id="ae2d0-121">Ezeket az adatokat a szolgáltatás fejlesztés, használhatóság fejlesztések és egyéb befektetések hello alkalmazás és a platform is ismerteti.</span><span class="sxs-lookup"><span data-stu-id="ae2d0-121">This data can guide feature development, usability improvements, and other investments in hello app and platform.</span></span> <span data-ttu-id="ae2d0-122">Ezeknek az adatoknak egyetlen több bérlős adatbázisban történő elérése könnyű, de nem olyan egyszerű, ha méretezve akár több ezer adatbázis között vannak elosztva.</span><span class="sxs-lookup"><span data-stu-id="ae2d0-122">Accessing this data when it's in a single multi-tenant database is easy, but not so easy when distributed at scale across potentially thousands of databases.</span></span> <span data-ttu-id="ae2d0-123">Több megközelítés tooaccessing el az adatok toouse rugalmas feladatok, amelyek lehetővé teszik a lekérdezés eredményeinek eredményt visszaadó feladat végrehajtási toobe egy kimeneti adatbázis és tábla rögzített.</span><span class="sxs-lookup"><span data-stu-id="ae2d0-123">One approach tooaccessing this data is toouse Elastic jobs, which enable result-returning query results from job execution toobe captured in an output database and table.</span></span>

## <a name="get-hello-wingtip-application-scripts"></a><span data-ttu-id="ae2d0-124">Hello Wingtip alkalmazás parancsfájlok beolvasása</span><span class="sxs-lookup"><span data-stu-id="ae2d0-124">Get hello Wingtip application scripts</span></span>

<span data-ttu-id="ae2d0-125">hello Wingtip Szolgáltatottszoftver-parancsfájlok és az alkalmazás forráskódjához találhatók hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github-tárház.</span><span class="sxs-lookup"><span data-stu-id="ae2d0-125">hello Wingtip SaaS scripts and application source code are available in hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github repo.</span></span> <span data-ttu-id="ae2d0-126">[Lépések toodownload hello Wingtip SaaS parancsfájlok](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).</span><span class="sxs-lookup"><span data-stu-id="ae2d0-126">[Steps toodownload hello Wingtip SaaS scripts](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).</span></span>

## <a name="deploy-a-database-for-tenant-analytics-results"></a><span data-ttu-id="ae2d0-127">A bérlői analitikai eredményeket tároló adatbázis kialakítása</span><span class="sxs-lookup"><span data-stu-id="ae2d0-127">Deploy a database for tenant analytics results</span></span>

<span data-ttu-id="ae2d0-128">Ez az oktatóanyag egy adatbázis telepített toocapture hello parancsfájlokat, amelyek eredményt visszaadó lekérdezéseket tartalmaznak feladat végrehajtásának eredménye toohave igényel.</span><span class="sxs-lookup"><span data-stu-id="ae2d0-128">This tutorial requires you toohave a database deployed toocapture hello results from job execution of scripts, which contain results-returning queries.</span></span> <span data-ttu-id="ae2d0-129">Hozzunk létre egy tenantanalytics nevű adatbázist erre a célra.</span><span class="sxs-lookup"><span data-stu-id="ae2d0-129">Let's create a database called tenantanalytics for this purpose.</span></span>

1. <span data-ttu-id="ae2d0-130">Nyissa meg... \\Tanulási modulok\\működési Analytics\\bérlői Analytics\\*bemutató-TenantAnalyticsDB.ps1* a hello *PowerShell ISE* és beállítása a következő érték hello:</span><span class="sxs-lookup"><span data-stu-id="ae2d0-130">Open …\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*Demo-TenantAnalyticsDB.ps1* in hello *PowerShell ISE* and set hello following value:</span></span>
   * <span data-ttu-id="ae2d0-131">**$DemoScenario** = **2***Működési elemzés adatbázis telepítése*</span><span class="sxs-lookup"><span data-stu-id="ae2d0-131">**$DemoScenario** = **2** *Deploy operational analytics database*</span></span>
1. <span data-ttu-id="ae2d0-132">Nyomja le az **F5** toorun hello bemutató-parancsfájl (adott hívások hello *telepítés-TenantAnalyticsDB.ps1* parancsfájl) hello bérlői analytics adatbázist hoz létre, amely.</span><span class="sxs-lookup"><span data-stu-id="ae2d0-132">Press **F5** toorun hello demo script (that calls hello *Deploy-TenantAnalyticsDB.ps1* script) which creates hello tenant analytics database.</span></span>

## <a name="create-some-data-for-hello-demo"></a><span data-ttu-id="ae2d0-133">Néhány hello bemutató adatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="ae2d0-133">Create some data for hello demo</span></span>

1. <span data-ttu-id="ae2d0-134">Nyissa meg... \\Tanulási modulok\\működési Analytics\\bérlői Analytics\\*bemutató-TenantAnalyticsDB.ps1* a hello *PowerShell ISE* és beállítása a következő érték hello:</span><span class="sxs-lookup"><span data-stu-id="ae2d0-134">Open …\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*Demo-TenantAnalyticsDB.ps1* in hello *PowerShell ISE* and set hello following value:</span></span>
   * <span data-ttu-id="ae2d0-135">**$DemoScenario** = **1***Jegyek beszerzése minden helyszínen*</span><span class="sxs-lookup"><span data-stu-id="ae2d0-135">**$DemoScenario** = **1** *Purchase tickets for events at all venues*</span></span>
1. <span data-ttu-id="ae2d0-136">Nyomja le az **F5** toorun parancsfájl hello és előzmények megvásárlásáról jegy létrehozását.</span><span class="sxs-lookup"><span data-stu-id="ae2d0-136">Press **F5** toorun hello script and create ticket purchasing history.</span></span>


## <a name="create-a-scheduled-job-tooretrieve-tenant-analytics-about-ticket-purchases"></a><span data-ttu-id="ae2d0-137">Hozzon létre egy ütemezett feladatot tooretrieve bérlői analytics jegy vásárlás kapcsolatos</span><span class="sxs-lookup"><span data-stu-id="ae2d0-137">Create a scheduled job tooretrieve tenant analytics about ticket purchases</span></span>

<span data-ttu-id="ae2d0-138">Ez a parancsfájl egy feladat tooretrieve jegy vásárlási információk egyetlen bérlő számára hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ae2d0-138">This script creates a job tooretrieve ticket purchase information from all tenants.</span></span> <span data-ttu-id="ae2d0-139">Miután összesítése egy táblát, kaphatnak a gazdag osztályon metrikák kapcsolatos jegy minták megvásárlásáról hello bérlők között.</span><span class="sxs-lookup"><span data-stu-id="ae2d0-139">Once aggregated into a single table, you can gain rich insightful metrics about ticket purchasing patterns across hello tenants.</span></span>

1. <span data-ttu-id="ae2d0-140">Nyissa meg a szolgáltatáshoz az SSMS, és csatlakozzon toohello katalógus -&lt;felhasználói&gt;. database.windows.net kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="ae2d0-140">Open SSMS and connect toohello catalog-&lt;user&gt;.database.windows.net server</span></span>
1. <span data-ttu-id="ae2d0-141">Nyissa meg a ...\\Tanulási modulok\\Működési elemzések\\Bérlői analitikák\\*TicketPurchasesfromAllTenants.sql* fájlt</span><span class="sxs-lookup"><span data-stu-id="ae2d0-141">Open ...\\Learning Modules\\Operational Analytics\\Tenant Analytics\\*TicketPurchasesfromAllTenants.sql*</span></span>
1. <span data-ttu-id="ae2d0-142">Módosítsa &lt;felhasználói&gt;, hello felhasználói név használata hello Wingtip SaaS-alkalmazás hello parancsfájl hello tetején telepítésekor használt **sp\_hozzáadása\_cél\_csoport\_tag** és **sp\_hozzáadása\_feladatlépés használja**</span><span class="sxs-lookup"><span data-stu-id="ae2d0-142">Modify &lt;User&gt;, use hello user name used when you deployed hello Wingtip SaaS app at hello top of hello script, **sp\_add\_target\_group\_member** and **sp\_add\_jobstep**</span></span>
1. <span data-ttu-id="ae2d0-143">Kattintson a jobb gombbal, válassza ki **kapcsolat**, és csatlakozzon toohello katalógus -&lt;felhasználói&gt;. database.windows.net kiszolgáló, ha még nincs csatlakoztatva</span><span class="sxs-lookup"><span data-stu-id="ae2d0-143">Right click, select **Connection**, and connect toohello catalog-&lt;User&gt;.database.windows.net server, if not already connected</span></span>
1. <span data-ttu-id="ae2d0-144">Hogy az csatlakoztatott toohello **jobaccount** adatbázis, és nyomja le az **F5** hello parancsfájl futtatásához</span><span class="sxs-lookup"><span data-stu-id="ae2d0-144">Ensure you are connected toohello **jobaccount** database and press **F5** to run hello script</span></span>

* <span data-ttu-id="ae2d0-145">**SP\_hozzáadása\_cél\_csoport** hoz létre hello célcsoport neve *TenantGroup*, igazolnia kell, hogy tooadd cél tagok most.</span><span class="sxs-lookup"><span data-stu-id="ae2d0-145">**sp\_add\_target\_group** creates hello target group name *TenantGroup*, now we need tooadd target members.</span></span>
* <span data-ttu-id="ae2d0-146">**SP\_hozzáadása\_cél\_csoport\_tag** ad hozzá egy *server* céltípust tag, mely úgy ítéli (Megjegyzés: Ez a hello customer1 - kiszolgálón található összes adatbázisok &lt;Felhasználói&gt; hello bérlői adatbázisokat tartalmazó kiszolgáló) időpontban feladat végrehajtási hello feladat kell foglalni.</span><span class="sxs-lookup"><span data-stu-id="ae2d0-146">**sp\_add\_target\_group\_member** adds a *server* target member type, which deems all databases within that server (note this is hello customer1-&lt;User&gt; server containing hello tenant databases) at time of job execution should be included in hello job.</span></span>
* <span data-ttu-id="ae2d0-147">Az **sp\_add\_job** létrehoz egy új, hetenként ütemezett feladatot „Jegyvásárlások az összes bérlőnél” néven</span><span class="sxs-lookup"><span data-stu-id="ae2d0-147">**sp\_add\_job** creates a new weekly scheduled job called “Ticket Purchases from all Tenants”</span></span>
* <span data-ttu-id="ae2d0-148">**SP\_hozzáadása\_feladatlépés használja** hello feladat lépésének T-SQL parancsot szöveg tooretrieve tartalmazó összes hello jegy vásárlási információk létrehoz minden bérlők és vissza a következő táblába eredménykészlet másolási hello  *AllTicketsPurchasesfromAllTenants*</span><span class="sxs-lookup"><span data-stu-id="ae2d0-148">**sp\_add\_jobstep** creates hello job step containing T-SQL command text tooretrieve all hello ticket purchase information from all tenants and copy hello returning result set into a table called *AllTicketsPurchasesfromAllTenants*</span></span>
* <span data-ttu-id="ae2d0-149">hello fennmaradó nézetek hello parancsfájl megjelenítése hello objektumok és a figyelő feladat végrehajtása hello megléte.</span><span class="sxs-lookup"><span data-stu-id="ae2d0-149">hello remaining views in hello script display hello existence of hello objects and monitor job execution.</span></span> <span data-ttu-id="ae2d0-150">Tekintse át a hello állapot értéket hello **életciklus** oszlop toomonitor hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="ae2d0-150">Review hello status value from hello **lifecycle** column toomonitor hello status.</span></span> <span data-ttu-id="ae2d0-151">Egyszer sikeres volt, hello feladat sikeresen befejeződött az összes bérlői adatbázisok és hello hello tartalmazó két további adatbázisok táblájára hivatkozhat.</span><span class="sxs-lookup"><span data-stu-id="ae2d0-151">Once, Succeeded, hello job has successfully finished on all tenant databases and hello two additional databases containing hello reference table.</span></span>

<span data-ttu-id="ae2d0-152">Hello parancsfájl sikeresen fut hasonló eredményeket eredményez:</span><span class="sxs-lookup"><span data-stu-id="ae2d0-152">Successfully running hello script should result in similar results:</span></span>

![eredmények](media/sql-database-saas-tutorial-tenant-analytics/ticket-purchases-job.png)

## <a name="create-a-job-tooretrieve-a-summary-count-of-ticket-purchases-from-all-tenants"></a><span data-ttu-id="ae2d0-154">Egy feladat tooretrieve jegy összesített száma a későbbi szoftvervásárlások minden bérlő létrehozása</span><span class="sxs-lookup"><span data-stu-id="ae2d0-154">Create a job tooretrieve a summary count of ticket purchases from all tenants</span></span>

<span data-ttu-id="ae2d0-155">Ezt a parancsfájlt a jegy vásárlását feladat tooretrieve összege egyetlen bérlő számára hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ae2d0-155">This script creates a job tooretrieve sum of all ticket purchases from all tenants.</span></span>

1. <span data-ttu-id="ae2d0-156">Nyissa meg a szolgáltatáshoz az SSMS és toohello csatlakozzon *katalógus -&lt;felhasználói&gt;. database.windows.net* kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="ae2d0-156">Open SSMS and connect toohello *catalog-&lt;User&gt;.database.windows.net* server</span></span>
1. <span data-ttu-id="ae2d0-157">Nyissa meg hello fájl... \\Tanulási modulok\\biztosítása és a katalógus\\működési Analytics\\Analytics bérlői\\*eredmények-TicketPurchasesfromAllTenants.sql*</span><span class="sxs-lookup"><span data-stu-id="ae2d0-157">Open hello file …\\Learning Modules\\Provision and Catalog\\Operational Analytics\\Tenant Analytics\\*Results-TicketPurchasesfromAllTenants.sql*</span></span>
1. <span data-ttu-id="ae2d0-158">Módosítsa &lt;felhasználói&gt;, hello felhasználói név használata hello Wingtip SaaS app hello parancsfájlban a hello telepítésekor használt **sp\_hozzáadása\_feladatlépés használja** tárolt eljárás</span><span class="sxs-lookup"><span data-stu-id="ae2d0-158">Modify &lt;User&gt;, use hello user name used when you deployed hello Wingtip SaaS app in hello script, in hello **sp\_add\_jobstep** stored procedure</span></span>
1. <span data-ttu-id="ae2d0-159">Kattintson a jobb gombbal, válassza ki **kapcsolat**, és csatlakozzon toohello katalógus -&lt;felhasználói&gt;. database.windows.net kiszolgáló, ha még nincs csatlakoztatva</span><span class="sxs-lookup"><span data-stu-id="ae2d0-159">Right click, select **Connection**, and connect toohello catalog-&lt;User&gt;.database.windows.net server, if not already connected</span></span>
1. <span data-ttu-id="ae2d0-160">Hogy az csatlakoztatott toohello **tenantanalytics** adatbázis, és nyomja le az **F5** hello parancsfájl futtatásához</span><span class="sxs-lookup"><span data-stu-id="ae2d0-160">Ensure you are connected toohello **tenantanalytics** database and press **F5** to run hello script</span></span>

<span data-ttu-id="ae2d0-161">Hello parancsfájl sikeresen fut hasonló eredményeket eredményez:</span><span class="sxs-lookup"><span data-stu-id="ae2d0-161">Successfully running hello script should result in similar results:</span></span>

![eredmények](media/sql-database-saas-tutorial-tenant-analytics/total-sales.png)



* <span data-ttu-id="ae2d0-163">Az **sp\_add\_job** létrehoz egy új, hetenként ütemezett feladatot „Jegyrendelési eredmények” néven</span><span class="sxs-lookup"><span data-stu-id="ae2d0-163">**sp\_add\_job** creates a new weekly scheduled job called “ResultsTicketsOrders”</span></span>

* <span data-ttu-id="ae2d0-164">**SP\_hozzáadása\_feladatlépés használja** hello feladat lépésének T-SQL parancsot szöveg tooretrieve tartalmazó összes hello jegy vásárlási információk létrehoz minden bérlők és vissza a következő táblába CountofTicketOrders eredménykészlet másolási hello</span><span class="sxs-lookup"><span data-stu-id="ae2d0-164">**sp\_add\_jobstep** creates hello job step containing T-SQL command text tooretrieve all hello ticket purchase information from all tenants and copy hello returning result set into a table called CountofTicketOrders</span></span>

* <span data-ttu-id="ae2d0-165">hello fennmaradó nézetek hello parancsfájl megjelenítése hello objektumok és a figyelő feladat végrehajtása hello megléte.</span><span class="sxs-lookup"><span data-stu-id="ae2d0-165">hello remaining views in hello script display hello existence of hello objects and monitor job execution.</span></span> <span data-ttu-id="ae2d0-166">Tekintse át a hello állapot értéket hello **életciklus** oszlop toomonitor hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="ae2d0-166">Review hello status value from hello **lifecycle** column toomonitor hello status.</span></span> <span data-ttu-id="ae2d0-167">Egyszer sikeres volt, hello feladat sikeresen befejeződött az összes bérlői adatbázisok és hello hello tartalmazó két további adatbázisok táblájára hivatkozhat.</span><span class="sxs-lookup"><span data-stu-id="ae2d0-167">Once, Succeeded, hello job has successfully finished on all tenant databases and hello two additional databases containing hello reference table.</span></span>


## <a name="next-steps"></a><span data-ttu-id="ae2d0-168">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ae2d0-168">Next steps</span></span>

<span data-ttu-id="ae2d0-169">Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="ae2d0-169">In this tutorial you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ae2d0-170">Bérlői analitikai adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="ae2d0-170">Deploy a tenant analytics database</span></span>
> * <span data-ttu-id="ae2d0-171">Hozzon létre egy ütemezett feladatot tooretrieve analitikai adatok bérlők között</span><span class="sxs-lookup"><span data-stu-id="ae2d0-171">Create a scheduled job tooretrieve analytical data across tenants</span></span>

<span data-ttu-id="ae2d0-172">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="ae2d0-172">Congratulations!</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ae2d0-173">További források</span><span class="sxs-lookup"><span data-stu-id="ae2d0-173">Additional resources</span></span>

* <span data-ttu-id="ae2d0-174">További [oktatóprogramot kínál, amelyek hello Wingtip SaaS-alkalmazás épül](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)</span><span class="sxs-lookup"><span data-stu-id="ae2d0-174">Additional [tutorials that build upon hello Wingtip SaaS application](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)</span></span>
* [<span data-ttu-id="ae2d0-175">Rugalmas feladatok</span><span class="sxs-lookup"><span data-stu-id="ae2d0-175">Elastic Jobs</span></span>](sql-database-elastic-jobs-overview.md)
