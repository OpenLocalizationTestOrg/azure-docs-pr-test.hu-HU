---
title: "aaaUse Log Analytics egy SQL-adatbázis több-bérlős alkalmazással |} Microsoft Docs"
description: "A telepítő és hello Azure SQL Database mintaalkalmazás Wingtip SaaS Naplóelemzés (OMS) használata"
keywords: "sql database-oktatóanyag"
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: billgib; sstein
ms.openlocfilehash: fa9085ce3462939e66853faa2a3cd71e0f6c2581
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="setup-and-use-log-analytics-oms-with-hello-wingtip-saas-app"></a><span data-ttu-id="95429-104">Beállítania és használnia Naplóelemzés (OMS) hello Wingtip SaaS-alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="95429-104">Setup and use Log Analytics (OMS) with hello Wingtip SaaS app</span></span>

<span data-ttu-id="95429-105">Ebben az oktatóanyagban beállítása és használata *Naplóelemzési ([OMS](https://www.microsoft.com/cloud-platform/operations-management-suite))* a rugalmas készletek és adatbázisokat figyeli.</span><span class="sxs-lookup"><span data-stu-id="95429-105">In this tutorial, you set up and use *Log Analytics([OMS](https://www.microsoft.com/cloud-platform/operations-management-suite))* for monitoring elastic pools and databases.</span></span> <span data-ttu-id="95429-106">-Buildekről nyújtanak a hello [Teljesítményfigyelő és a felügyeleti útmutató](sql-database-saas-tutorial-performance-monitoring.md), és bemutatja, hogyan toouse *Naplóelemzési* tooaugment hello figyelés és riasztás megadott hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="95429-106">It builds on hello [Performance Monitoring and Management tutorial](sql-database-saas-tutorial-performance-monitoring.md), and shows how toouse *Log Analytics* tooaugment hello monitoring and alerting provided in hello Azure portal.</span></span> <span data-ttu-id="95429-107">A Log Analytics különösen alkalmas kiterjedt figyelésre és riasztásra, mert több száz készlet és több százezer adatbázis használatát is támogatja.</span><span class="sxs-lookup"><span data-stu-id="95429-107">Log Analytics is particularly suitable for monitoring and alerting at scale because it supports hundreds of pools and hundreds of thousands of databases.</span></span> <span data-ttu-id="95429-108">Egyetlen figyelési megoldásként is szolgál, amely képes több Azure-előfizetésben is integrálni a különböző alkalmazások és Azure-szolgáltatások figyelését.</span><span class="sxs-lookup"><span data-stu-id="95429-108">It also provides a single monitoring solution, which can integrate monitoring of different applications and Azure services, across multiple Azure subscriptions.</span></span>

<span data-ttu-id="95429-109">Ennek az oktatóanyagnak a segítségével megtanulhatja a következőket:</span><span class="sxs-lookup"><span data-stu-id="95429-109">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="95429-110">Log Analytics (OMS) telepítése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="95429-110">Install and configure Log Analytics (OMS)</span></span>
> * <span data-ttu-id="95429-111">Naplóelemzési toomonitor készletek és adatbázisok használata</span><span class="sxs-lookup"><span data-stu-id="95429-111">Use Log Analytics toomonitor pools and databases</span></span>

<span data-ttu-id="95429-112">Ebben az oktatóanyagban a következő előfeltételek győződjön meg arról, hogy hello elvégzése toocomplete:</span><span class="sxs-lookup"><span data-stu-id="95429-112">toocomplete this tutorial, make sure hello following prerequisites are completed:</span></span>

* <span data-ttu-id="95429-113">hello Wingtip SaaS-alkalmazás telepítve van.</span><span class="sxs-lookup"><span data-stu-id="95429-113">hello Wingtip SaaS app is deployed.</span></span> <span data-ttu-id="95429-114">toodeploy öt percen belül, lásd: [központi telepítése és felfedezése hello Wingtip SaaS-alkalmazáshoz](sql-database-saas-tutorial.md)</span><span class="sxs-lookup"><span data-stu-id="95429-114">toodeploy in less than five minutes, see [Deploy and explore hello Wingtip SaaS application](sql-database-saas-tutorial.md)</span></span>
* <span data-ttu-id="95429-115">Az Azure PowerShell telepítve van.</span><span class="sxs-lookup"><span data-stu-id="95429-115">Azure PowerShell is installed.</span></span> <span data-ttu-id="95429-116">A részletekért lásd: [Ismerkedés az Azure PowerShell-lel](https://docs.microsoft.com/powershell/azure/get-started-azureps)</span><span class="sxs-lookup"><span data-stu-id="95429-116">For details, see [Getting started with Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps)</span></span>

<span data-ttu-id="95429-117">Lásd: hello [Teljesítményfigyelő és a felügyeleti útmutató](sql-database-saas-tutorial-performance-monitoring.md) hello SaaS forgatókönyveket és a minták és a figyelési megoldás hello terhelését hatásuk leírását.</span><span class="sxs-lookup"><span data-stu-id="95429-117">See hello [Performance Monitoring and Management tutorial](sql-database-saas-tutorial-performance-monitoring.md) for a discussion of hello SaaS scenarios and patterns, and how they affect hello requirements on a monitoring solution.</span></span>

## <a name="monitoring-and-managing-performance-with-log-analytics-oms"></a><span data-ttu-id="95429-118">Teljesítmény figyelése és kezelése a Log Analyticsban (OMS)</span><span class="sxs-lookup"><span data-stu-id="95429-118">Monitoring and managing performance with Log Analytics (OMS)</span></span>

<span data-ttu-id="95429-119">Az SQL Database esetében a figyelés és riasztás rendelkezésre áll az adatbázisokhoz és a készletekhez.</span><span class="sxs-lookup"><span data-stu-id="95429-119">For SQL Database, monitoring and alerting is available on databases and pools.</span></span> <span data-ttu-id="95429-120">A figyelés és riasztás beépített erőforrás-specifikus és kényelmes, a kis mennyiségű erőforrást, de kevesebb kiválóan alkalmas toomonitoring nagyobb telepítések, vagy az egységes nézetének köszönhetően különböző erőforrások és -előfizetések között.</span><span class="sxs-lookup"><span data-stu-id="95429-120">This built-in monitoring and alerting is resource-specific and convenient for small numbers of resources, but is less well suited toomonitoring large installations or for providing a unified view across different resources and subscriptions.</span></span>

<span data-ttu-id="95429-121">Nagy mennyiségű erőforrás esetén a Log Analytics használható.</span><span class="sxs-lookup"><span data-stu-id="95429-121">For high-volume scenarios Log Analytics can be used.</span></span> <span data-ttu-id="95429-122">Egy külön Azure szolgáltatás, amely analytics keresztül kibocsátott diagnosztikai naplók és a naplóelemzési munkaterület, amely számos telemetriai adatokat gyűjthet az összegyűjtött telemetrikus szolgáltatások és használt tooquery kell állíthatók be riasztások.</span><span class="sxs-lookup"><span data-stu-id="95429-122">This is a separate Azure service that provides analytics over emitted diagnostic logs and telemetry gathered in a log analytics workspace, which can collect telemetry from many services and be used tooquery and set alerts.</span></span> <span data-ttu-id="95429-123">A Log Analytics beépített lekérdezési nyelvet és adatvizualizációs eszközöket biztosít, amelyek lehetővé teszik a működési adatok elemzését és vizualizálását.</span><span class="sxs-lookup"><span data-stu-id="95429-123">Log Analytics provides a built-in query language and data visualization tools allowing operational data analytics and visualization.</span></span> <span data-ttu-id="95429-124">hello SQL elemzési megoldások számos előre definiált rugalmas készlet és figyelés és riasztás nézetek és lekérdezések adatbázis biztosít, és lehetővé teszi, hogy adja hozzá a saját ad hoc lekérdezéseket, és mentse ezeket igény szerint.</span><span class="sxs-lookup"><span data-stu-id="95429-124">hello SQL Analytics solution provides several pre-defined elastic pool and database monitoring and alerting views and queries, and lets you add your own ad-hoc queries and save these as needed.</span></span> <span data-ttu-id="95429-125">Az OMS egyéni nézettervező is biztosít.</span><span class="sxs-lookup"><span data-stu-id="95429-125">OMS also provides a custom view designer.</span></span>

<span data-ttu-id="95429-126">Napló elemzési munkaterületekkel és elemzési megoldásokat megnyitható hello Azure-portál és az OMS Szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="95429-126">Log Analytics workspaces and analytics solutions can be opened both in hello Azure portal and in OMS.</span></span> <span data-ttu-id="95429-127">hello Azure-portálon hello újabb csatlakozási pontja, de lehet mögött hello OMS-portálon az egyes területeken.</span><span class="sxs-lookup"><span data-stu-id="95429-127">hello Azure portal is hello newer access point but may be behind hello OMS portal in some areas.</span></span>

### <a name="start-hello-load-generator-toocreate-data-tooanalyze"></a><span data-ttu-id="95429-128">Indítsa el a hello terhelés generátor toocreate adatok tooanalyze</span><span class="sxs-lookup"><span data-stu-id="95429-128">Start hello load generator toocreate data tooanalyze</span></span>

1. <span data-ttu-id="95429-129">Nyissa meg **bemutató-PerformanceMonitoringAndManagement.ps1** a hello **PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="95429-129">Open **Demo-PerformanceMonitoringAndManagement.ps1** in hello **PowerShell ISE**.</span></span> <span data-ttu-id="95429-130">Ez a parancsfájl nyitva hagyja, érdemes lehet toorun hello terhelés generációs forgatókönyvek számos Ez az oktatóanyag során.</span><span class="sxs-lookup"><span data-stu-id="95429-130">Keep this script open as you may want toorun several of hello load generation scenarios during this tutorial.</span></span>
1. <span data-ttu-id="95429-131">Ha ötnél kevesebb bérlők, kiépítése egy tranzakcióköteghez bérlők tooprovide ennél is érdekesebb megoldást a figyelési környezetben:</span><span class="sxs-lookup"><span data-stu-id="95429-131">If you have less than five tenants, provision a batch of tenants tooprovide a more interesting monitoring context:</span></span>
   1. <span data-ttu-id="95429-132">Állítsa be a **$DemoScenario = 1,** **Bérlői köteg kiépítése** értéket</span><span class="sxs-lookup"><span data-stu-id="95429-132">Set **$DemoScenario = 1,** **Provision a batch of tenants**</span></span>
   1. <span data-ttu-id="95429-133">Nyomja le az **F5** toorun hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="95429-133">Press **F5** toorun hello script.</span></span>

1. <span data-ttu-id="95429-134">Állítsa be **$DemoScenario** = 2, **Normál intenzitású terhelés generálása (hozzávetőlegesen 40 DTU)** értéket.</span><span class="sxs-lookup"><span data-stu-id="95429-134">Set **$DemoScenario** = 2, **Generate normal intensity load (approx 40 DTU)**.</span></span>
1. <span data-ttu-id="95429-135">Nyomja le az **F5** toorun hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="95429-135">Press **F5** toorun hello script.</span></span>

## <a name="get-hello-wingtip-application-scripts"></a><span data-ttu-id="95429-136">Hello Wingtip alkalmazás parancsfájlok beolvasása</span><span class="sxs-lookup"><span data-stu-id="95429-136">Get hello Wingtip application scripts</span></span>

<span data-ttu-id="95429-137">hello Wingtip jegyek parancsfájlok és az alkalmazás forráskódjához találhatók hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github-tárház.</span><span class="sxs-lookup"><span data-stu-id="95429-137">hello Wingtip Tickets scripts and application source code are available in hello [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github repo.</span></span> <span data-ttu-id="95429-138">Hello található parancsfájlok [tanulási modulok mappa](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules).</span><span class="sxs-lookup"><span data-stu-id="95429-138">Script files are located in hello [Learning Modules folder](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules).</span></span> <span data-ttu-id="95429-139">Töltse le a hello **tanulási modulok** mappa tooyour helyi számítógépen, a gyökérmappa-szerkezetében karbantartása.</span><span class="sxs-lookup"><span data-stu-id="95429-139">Download hello **Learning Modules** folder tooyour local computer, maintaining its folder structure.</span></span>

## <a name="installing-and-configuring-log-analytics-and-hello-azure-sql-analytics-solution"></a><span data-ttu-id="95429-140">Telepítése és konfigurálása, Naplóelemzés és hello Azure SQL elemzési megoldások</span><span class="sxs-lookup"><span data-stu-id="95429-140">Installing and configuring Log Analytics and hello Azure SQL Analytics solution</span></span>

<span data-ttu-id="95429-141">A Naplóelemzési egy külön szolgáltatás, amelynek toobe konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="95429-141">Log Analytics is a separate service that needs toobe configured.</span></span> <span data-ttu-id="95429-142">A Log Analytics naplóadatokat, telemetriai adatok és metrikákat gyűjt egy naplóelemzési munkaterületre.</span><span class="sxs-lookup"><span data-stu-id="95429-142">Log Analytics collects log data and telemetry and metrics in a log analytics workspace.</span></span> <span data-ttu-id="95429-143">A munkaterület egy erőforrás, mint az Azure egyéb erőforrásai, és létre kell hozni.</span><span class="sxs-lookup"><span data-stu-id="95429-143">A workspace is a resource, just like other resources in Azure, and must be created.</span></span> <span data-ttu-id="95429-144">Amíg hello munkaterület hello létrehozott toobe nem kell ugyanabban az erőforráscsoportban, az általa figyelt hello alkalmazás(ok), ez gyakran az így hello legtöbb.</span><span class="sxs-lookup"><span data-stu-id="95429-144">While hello workspace doesn’t need toobe created in hello same resource group as hello application(s) it is monitoring, this often makes hello most sense.</span></span> <span data-ttu-id="95429-145">Hello esetében hello Wingtip SaaS-alkalmazás így hello munkaterület toobe hello erőforráscsoport egyszerűen törlésével hello alkalmazással könnyedén törölni.</span><span class="sxs-lookup"><span data-stu-id="95429-145">In hello case of hello Wingtip SaaS app, this enables hello workspace toobe easily deleted with hello application by simply deleting hello resource group.</span></span>

1. <span data-ttu-id="95429-146">Nyissa meg... \\Tanulási modulok\\Teljesítményfigyelő és felügyeleti\\Naplóelemzési\\*bemutató-LogAnalytics.ps1* a hello **PowerShell ISE**.</span><span class="sxs-lookup"><span data-stu-id="95429-146">Open ...\\Learning Modules\\Performance Monitoring and Management\\Log Analytics\\*Demo-LogAnalytics.ps1* in hello **PowerShell ISE**.</span></span>
1. <span data-ttu-id="95429-147">Nyomja le az **F5** toorun hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="95429-147">Press **F5** toorun hello script.</span></span>

<span data-ttu-id="95429-148">Ezen a ponton képes nyitott Naplóelemzési kell hello Azure-portál (vagy hello OMS-portálon).</span><span class="sxs-lookup"><span data-stu-id="95429-148">At this point you should be able open Log Analytics in hello Azure portal (or hello OMS portal).</span></span> <span data-ttu-id="95429-149">Eltarthat néhány percig, hello Naplóelemzési munkaterületet, és látható toobecome gyűjtött telemetriai toobe.</span><span class="sxs-lookup"><span data-stu-id="95429-149">It will take a few minutes for telemetry toobe collected in hello Log Analytics workspace and toobecome visible.</span></span> <span data-ttu-id="95429-150">hello gyűjt adatokat hello érdekesebb hello élmény hello rendszer hagyja hosszabb lesz.</span><span class="sxs-lookup"><span data-stu-id="95429-150">hello longer you leave hello system gathering data hello more interesting hello experience will be.</span></span> <span data-ttu-id="95429-151">Most már a toograb italt - csak ellenőrizze, hogy hello terhelés generátor még fut egy időben áll!</span><span class="sxs-lookup"><span data-stu-id="95429-151">Now's a good time toograb a beverage - just make sure hello load generator is still running!</span></span>


## <a name="use-log-analytics-and-hello-sql-analytics-solution-toomonitor-pools-and-databases"></a><span data-ttu-id="95429-152">A Naplóelemzési használja, és SQL elemzés megoldás toomonitor készletek és adatbázisok hello</span><span class="sxs-lookup"><span data-stu-id="95429-152">Use Log Analytics and hello SQL Analytics solution toomonitor pools and databases</span></span>


<span data-ttu-id="95429-153">Ebben a gyakorlatban Naplóelemzési és hello toolook OMS-portálon hello adatbázisok és a készletek alatt gyűjtött hello telemetriai megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="95429-153">In this exercise, open Log Analytics and hello OMS portal toolook at hello telemetry being gathered for hello databases and pools.</span></span>

1. <span data-ttu-id="95429-154">Keresse meg a toohello [Azure-portálon](https://portal.azure.com) Naplóelemzési megnyitásához kattintson további szolgáltatásokat, majd keresse meg a Naplóelemzési:</span><span class="sxs-lookup"><span data-stu-id="95429-154">Browse toohello [Azure portal](https://portal.azure.com) and open Log Analytics by clicking More services, then search for Log Analytics:</span></span>

   ![Log Analytics megnyitása](media/sql-database-saas-tutorial-log-analytics/log-analytics-open.png)

1. <span data-ttu-id="95429-156">Válassza ki a hello munkaterület nevű *wtploganalytics -&lt;felhasználói&gt;*.</span><span class="sxs-lookup"><span data-stu-id="95429-156">Select hello workspace named *wtploganalytics-&lt;USER&gt;*.</span></span>

1. <span data-ttu-id="95429-157">Válassza ki **áttekintése** tooopen hello Naplóelemzési megoldás hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="95429-157">Select **Overview** tooopen hello Log Analytics solution in hello Azure portal.</span></span>
   <span data-ttu-id="95429-158">![áttekintés-hivatkozás](media/sql-database-saas-tutorial-log-analytics/click-overview.png)</span><span class="sxs-lookup"><span data-stu-id="95429-158">![overview-link](media/sql-database-saas-tutorial-log-analytics/click-overview.png)</span></span>

    <span data-ttu-id="95429-159">**FONTOS**: eltarthat néhány percig, mielőtt hello megoldás aktív.</span><span class="sxs-lookup"><span data-stu-id="95429-159">**IMPORTANT**: It may take a couple of minutes before hello solution is active.</span></span> <span data-ttu-id="95429-160">Legyen türelemmel!</span><span class="sxs-lookup"><span data-stu-id="95429-160">Be patient!</span></span>

1. <span data-ttu-id="95429-161">Kattintson az Azure SQL elemzés csempe tooopen hello azt.</span><span class="sxs-lookup"><span data-stu-id="95429-161">Click on hello Azure SQL Analytics tile tooopen it.</span></span>

    ![Áttekintés](media/sql-database-saas-tutorial-log-analytics/overview.png)

    ![analytics](media/sql-database-saas-tutorial-log-analytics/analytics.png)

1. <span data-ttu-id="95429-164">hello hello megoldás panel görgetése oldalra, a nézet a saját görgetősáv hello alsó (frissítési hello panel szükség esetén).</span><span class="sxs-lookup"><span data-stu-id="95429-164">hello view in hello solution blade scrolls sideways, with its own scroll bar at hello bottom (refresh hello blade if needed).</span></span>

1. <span data-ttu-id="95429-165">Függőleges elképzelései hello őket, illetve az egyes erőforrások tooopen egy részletes explorer, ahol ugyanúgy használhatók hello idő-csúszkát a hello gombra kattintva különböző nézet bal felső, és kattintson az egy szűkebb időszelet toofocus a sávot.</span><span class="sxs-lookup"><span data-stu-id="95429-165">Explore hello various views by clicking on them or on individual resources tooopen a drill-down explorer, where you can use hello time-slider in hello top left or click on a vertical bar toofocus in on a narrower time slice.</span></span> <span data-ttu-id="95429-166">Az ebben a nézetben válassza ki az egyes adatbázisok vagy készletek toofocus egy adott erőforráshoz:</span><span class="sxs-lookup"><span data-stu-id="95429-166">With this view, you can select individual databases or pools toofocus on specific resources:</span></span>

    ![diagram](media/sql-database-saas-tutorial-log-analytics/chart.png)

1. <span data-ttu-id="95429-168">Vissza a hello megoldás panelen Ha toohello jobb szélén látni fogja, hogy tooopen parancsára, és megismerkedhet néhány mentett lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="95429-168">Back on hello solution blade, if you scroll toohello far right you will see some saved queries that you can click on tooopen and explore.</span></span> <span data-ttu-id="95429-169">Kísérletezhet ezek módosításával, majd mentheti a létrehozott és érdekesnek talált lekérdezéseket, melyeket aztán újra megnyitva más erőforrásokkal használhat.</span><span class="sxs-lookup"><span data-stu-id="95429-169">You can experiment with modifying these, and save any interesting queries you produce, which you can then re-open and use with other resources.</span></span>

1. <span data-ttu-id="95429-170">Vissza a hello Naplóelemzési munkaterület panelen jelölje ki az OMS-portálon tooopen hello megoldást van.</span><span class="sxs-lookup"><span data-stu-id="95429-170">Back on hello Log Analytics workspace blade, select OMS Portal tooopen hello solution there.</span></span>

    ![oms](media/sql-database-saas-tutorial-log-analytics/oms.png)

1. <span data-ttu-id="95429-172">Az OMS-portálon hello konfigurálhat.</span><span class="sxs-lookup"><span data-stu-id="95429-172">In hello OMS portal, you can configure alerts.</span></span> <span data-ttu-id="95429-173">Kattintson a hello hello adatbázis DTU nézet riasztási része.</span><span class="sxs-lookup"><span data-stu-id="95429-173">Click on hello alert portion of hello database DTU view.</span></span>

1. <span data-ttu-id="95429-174">Hello napló keresése nézetet, amely akkor jelenik meg, hogy egy oszlopdiagramot a képviselt hello mérőszámok jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="95429-174">In hello Log Search view that appears you will see a bar graph of hello metrics being represented.</span></span>

    ![naplóbeli keresés](media/sql-database-saas-tutorial-log-analytics/log-search.png)

1. <span data-ttu-id="95429-176">Ha hello eszköztáron kattintson a riasztás fogja tudni toosee hello riasztás konfigurálásában és keresztül tudja módosítani azt.</span><span class="sxs-lookup"><span data-stu-id="95429-176">If you click on Alert in hello toolbar you will be able toosee hello alert configuration and can change it.</span></span>

    ![riasztási szabály hozzáadása](media/sql-database-saas-tutorial-log-analytics/add-alert.png)

<span data-ttu-id="95429-178">figyelés és riasztás Naplóelemzés és az OMS hello hello adatok hello munkaterületen, riasztást küld minden erőforrás panel, amelyen az erőforrás-specifikus hello eltérően lekérdezéseken alapul.</span><span class="sxs-lookup"><span data-stu-id="95429-178">hello monitoring and alerting in Log Analytics and OMS is based on queries over hello data in hello workspace, unlike hello alerting on each resource blade, which is resource-specific.</span></span> <span data-ttu-id="95429-179">Tehát meghatározhat olyan riasztást, amely az összes adatbázist figyeli, ahelyett, hogy adatbázisonként meg kellene adni egyet.</span><span class="sxs-lookup"><span data-stu-id="95429-179">Thus, you can define an alert that looks over all databases, say, rather than defining one per database.</span></span> <span data-ttu-id="95429-180">Vagy írhat olyan riasztást, amely összetett lekérdezést használ több erőforrástípusban.</span><span class="sxs-lookup"><span data-stu-id="95429-180">Or write an alert that uses a composite query over multiple resource types.</span></span> <span data-ttu-id="95429-181">Lekérdezések csak korlátozott hello munkaterületen elérhető hello adatokkal.</span><span class="sxs-lookup"><span data-stu-id="95429-181">Queries are only limited by hello data available in hello workspace.</span></span>

<span data-ttu-id="95429-182">Az SQL Database szolgáltatáshoz van szó, a hello munkaterület hello adatmennyiség alapján.</span><span class="sxs-lookup"><span data-stu-id="95429-182">Log Analytics for SQL Database is charged for based on hello data volume in hello workspace.</span></span> <span data-ttu-id="95429-183">Ebben az oktatóanyagban létrehozott egy szabad munkaterület, ez a korlátozott too500MB / nap.</span><span class="sxs-lookup"><span data-stu-id="95429-183">In this tutorial, you created a Free workspace, which is limited too500MB per day.</span></span> <span data-ttu-id="95429-184">Amennyiben ezt a határértéket elérték adatok nem kerülnek toohello munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="95429-184">Once that limit is reached data is no longer added toohello workspace.</span></span>


## <a name="next-steps"></a><span data-ttu-id="95429-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="95429-185">Next steps</span></span>

<span data-ttu-id="95429-186">Ennek az oktatóanyagnak a segítségével megtanulta a következőket:</span><span class="sxs-lookup"><span data-stu-id="95429-186">In this tutorial you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="95429-187">Log Analytics (OMS) telepítése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="95429-187">Install and configure Log Analytics (OMS)</span></span>
> * <span data-ttu-id="95429-188">Naplóelemzési toomonitor készletek és adatbázisok használata</span><span class="sxs-lookup"><span data-stu-id="95429-188">Use Log Analytics toomonitor pools and databases</span></span>

[<span data-ttu-id="95429-189">Bérlői elemzések – oktatóanyag</span><span class="sxs-lookup"><span data-stu-id="95429-189">Tenant analytics tutorial</span></span>](sql-database-saas-tutorial-tenant-analytics.md)

## <a name="additional-resources"></a><span data-ttu-id="95429-190">További források</span><span class="sxs-lookup"><span data-stu-id="95429-190">Additional resources</span></span>

* [<span data-ttu-id="95429-191">További oktatóprogramot kínál, amelyek hello kezdeti Wingtip Szolgáltatottszoftver-alkalmazástelepítés épül</span><span class="sxs-lookup"><span data-stu-id="95429-191">Additional tutorials that build upon hello initial Wingtip SaaS application deployment</span></span>](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [<span data-ttu-id="95429-192">Azure Log Analytics</span><span class="sxs-lookup"><span data-stu-id="95429-192">Azure Log Analytics</span></span>](../log-analytics/log-analytics-azure-sql.md)
* [<span data-ttu-id="95429-193">OMS</span><span class="sxs-lookup"><span data-stu-id="95429-193">OMS</span></span>](https://blogs.technet.microsoft.com/msoms/2017/02/21/azure-sql-analytics-solution-public-preview/)