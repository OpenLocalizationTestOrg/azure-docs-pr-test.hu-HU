---
title: "Adatok betöltése az SQL Server be Azure SQL Data Warehouse (SSIS) |} Microsoft Docs"
description: "Bemutatja, hogyan tárolt adatok mozgatása az adatforrások széles SQL-adatraktár SQL Server Integration Services (SSIS) csomag létrehozásához."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: e2c252e9-0828-47c2-a808-e3bea46c134a
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 03/30/2017
ms.author: cakarst;douglasl;barbkess
ms.openlocfilehash: 6c9cebdd715b6997d0633bc725a3945ba9e0c357
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-ssis"></a><span data-ttu-id="bbde0-103">Adatok betöltése az SQL Server be Azure SQL Data Warehouse (SSIS)</span><span class="sxs-lookup"><span data-stu-id="bbde0-103">Load data from SQL Server into Azure SQL Data Warehouse (SSIS)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="bbde0-104">SSIS</span><span class="sxs-lookup"><span data-stu-id="bbde0-104">SSIS</span></span>](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
> * [<span data-ttu-id="bbde0-105">PolyBase</span><span class="sxs-lookup"><span data-stu-id="bbde0-105">PolyBase</span></span>](sql-data-warehouse-load-from-sql-server-with-polybase.md)
> * [<span data-ttu-id="bbde0-106">bcp</span><span class="sxs-lookup"><span data-stu-id="bbde0-106">bcp</span></span>](sql-data-warehouse-load-from-sql-server-with-bcp.md)
> 
> 

<span data-ttu-id="bbde0-107">Adatok betöltése az SQL Serverről az Azure SQL Data Warehouse az SQL Server Integration Services (SSIS) csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="bbde0-107">Create a SQL Server Integration Services (SSIS) package to load data from SQL Server into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="bbde0-108">Opcionálisan átalakításának, átalakítási és nagybetűhasználatának az adatokat, ahogy keresztülhalad az SSIS-adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="bbde0-108">You can optionally restructure, transform, and cleanse the data as it passes through the SSIS data flow.</span></span>

<span data-ttu-id="bbde0-109">Az oktatóanyag során az alábbi lépéseket fogja végrehajtani:</span><span class="sxs-lookup"><span data-stu-id="bbde0-109">In this tutorial, you will:</span></span>

* <span data-ttu-id="bbde0-110">Az integrációs szolgáltatások új projekt létrehozása a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="bbde0-110">Create a new Integration Services project in Visual Studio.</span></span>
* <span data-ttu-id="bbde0-111">Kapcsolódás az adatforrásokhoz, beleértve az SQL Server (mint a forrás-) és az SQL Data Warehouse (célként).</span><span class="sxs-lookup"><span data-stu-id="bbde0-111">Connect to data sources, including SQL Server (as a source) and SQL Data Warehouse (as a destination).</span></span>
* <span data-ttu-id="bbde0-112">Tervezze meg az SSIS-csomagot, amely a forrásból származó adatokat tölt a célhelyre történő.</span><span class="sxs-lookup"><span data-stu-id="bbde0-112">Design an SSIS package that loads data from the source into the destination.</span></span>
* <span data-ttu-id="bbde0-113">Futtassa a SSIS az adatok betöltésére.</span><span class="sxs-lookup"><span data-stu-id="bbde0-113">Run the SSIS package to load the data.</span></span>

<span data-ttu-id="bbde0-114">Ez az oktatóanyag az SQL Server, az adatforrással.</span><span class="sxs-lookup"><span data-stu-id="bbde0-114">This tutorial uses SQL Server as the data source.</span></span> <span data-ttu-id="bbde0-115">SQL Server sikerült fut, a helyszíni vagy egy Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="bbde0-115">SQL Server could be running on premises or in an Azure virtual machine.</span></span>

## <a name="basic-concepts"></a><span data-ttu-id="bbde0-116">Alapfogalmak</span><span class="sxs-lookup"><span data-stu-id="bbde0-116">Basic concepts</span></span>
<span data-ttu-id="bbde0-117">A csomag nincs SSIS munkaegysége.</span><span class="sxs-lookup"><span data-stu-id="bbde0-117">The package is the unit of work in SSIS.</span></span> <span data-ttu-id="bbde0-118">Kapcsolódó csomagok projektek vannak csoportosítva.</span><span class="sxs-lookup"><span data-stu-id="bbde0-118">Related packages are grouped in projects.</span></span> <span data-ttu-id="bbde0-119">Projektek és a Tervező csomagok létrehozása a Visual Studio és az SQL Server Data Tools összetevővel.</span><span class="sxs-lookup"><span data-stu-id="bbde0-119">You create projects and design packages in Visual Studio with SQL Server Data Tools.</span></span> <span data-ttu-id="bbde0-120">A tervezési folyamat az egy visual folyamat, amelyben húzza és összetevők dobja el az eszközkészletből a Tervező felületére, csatlakoztassa őket, és állítsa be tulajdonságaikat.</span><span class="sxs-lookup"><span data-stu-id="bbde0-120">The design process is a visual process in which you drag and drop components from the Toolbox to the design surface, connect them, and set their properties.</span></span> <span data-ttu-id="bbde0-121">Ha befejezte a csomag, opcionálisan telepítése az SQL Server átfogó kezelési, figyelési és biztonsági.</span><span class="sxs-lookup"><span data-stu-id="bbde0-121">After you finish your package, you can optionally deploy it to SQL Server for comprehensive management, monitoring, and security.</span></span>

## <a name="options-for-loading-data-with-ssis"></a><span data-ttu-id="bbde0-122">Beállítások SSIS az adatok betöltése</span><span class="sxs-lookup"><span data-stu-id="bbde0-122">Options for loading data with SSIS</span></span>
<span data-ttu-id="bbde0-123">SQL Server Integration Services (SSIS), amely számos lehetőséget biztosít csatlakozik, és az adatok betöltése az SQL Data Warehouse rugalmas eszközkészlet.</span><span class="sxs-lookup"><span data-stu-id="bbde0-123">SQL Server Integration Services (SSIS) is a flexible set of tools that provides a variety of options for connecting to, and loading data into, SQL Data Warehouse.</span></span>

1. <span data-ttu-id="bbde0-124">Az ADO NET cél használatával csatlakozhat az SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="bbde0-124">Use an ADO NET Destination to connect to SQL Data Warehouse.</span></span> <span data-ttu-id="bbde0-125">Ez az oktatóanyag egy ADO NET cél használja, mert a legkevesebb konfigurációs beállításokat.</span><span class="sxs-lookup"><span data-stu-id="bbde0-125">This tutorial uses an ADO NET Destination because it has the fewest configuration options.</span></span>
2. <span data-ttu-id="bbde0-126">Az OLE DB cél használatával csatlakozhat az SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="bbde0-126">Use an OLE DB Destination to connect to SQL Data Warehouse.</span></span> <span data-ttu-id="bbde0-127">Ez a beállítás által biztosított ADO NET cél némileg jobb teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="bbde0-127">This option may provide slightly better performance than the ADO NET Destination.</span></span>
3. <span data-ttu-id="bbde0-128">Az Azure Blob feltöltése feladat segítségével az adatok az Azure Blob Storage tesztelése.</span><span class="sxs-lookup"><span data-stu-id="bbde0-128">Use the Azure Blob Upload Task to stage the data in Azure Blob Storage.</span></span> <span data-ttu-id="bbde0-129">A SSIS SQL végrehajtása a feladat segítségével indítsa el az adatok betöltése az SQL Data Warehouse Polybase parancsfájlból.</span><span class="sxs-lookup"><span data-stu-id="bbde0-129">Then use the SSIS Execute SQL task to launch a Polybase script that loads the data into SQL Data Warehouse.</span></span> <span data-ttu-id="bbde0-130">Ez a beállítás az itt felsorolt három közül a legjobb teljesítményt biztosít.</span><span class="sxs-lookup"><span data-stu-id="bbde0-130">This option provides the best performance of the three options listed here.</span></span> <span data-ttu-id="bbde0-131">Ahhoz, hogy az Azure Blob feltöltése feladat, töltse le a [Microsoft SQL Server 2016 Integration Services funkciócsomag Azure][Microsoft SQL Server 2016 Integration Services Feature Pack for Azure].</span><span class="sxs-lookup"><span data-stu-id="bbde0-131">To get the Azure Blob Upload task, download the [Microsoft SQL Server 2016 Integration Services Feature Pack for Azure][Microsoft SQL Server 2016 Integration Services Feature Pack for Azure].</span></span> <span data-ttu-id="bbde0-132">A Polybase kapcsolatos további információkért lásd: [PolyBase-útmutatóban][PolyBase Guide].</span><span class="sxs-lookup"><span data-stu-id="bbde0-132">To learn more about Polybase, see [PolyBase Guide][PolyBase Guide].</span></span>

## <a name="before-you-start"></a><span data-ttu-id="bbde0-133">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="bbde0-133">Before you start</span></span>
<span data-ttu-id="bbde0-134">Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="bbde0-134">To step through this tutorial, you need:</span></span>

1. <span data-ttu-id="bbde0-135">**Az SQL Server Integration Services (SSIS)**.</span><span class="sxs-lookup"><span data-stu-id="bbde0-135">**SQL Server Integration Services (SSIS)**.</span></span> <span data-ttu-id="bbde0-136">SSIS az SQL Server, és próbaverziójáról vagy az SQL Server licenccel rendelkező verzió szükséges.</span><span class="sxs-lookup"><span data-stu-id="bbde0-136">SSIS is a component of SQL Server and requires an evaluation version or a licensed version of SQL Server.</span></span> <span data-ttu-id="bbde0-137">Ahhoz, hogy az SQL Server 2016 Preview próbaverzióját, lásd: [SQL Server értékelések][SQL Server Evaluations].</span><span class="sxs-lookup"><span data-stu-id="bbde0-137">To get an evaluation version of SQL Server 2016 Preview, see [SQL Server Evaluations][SQL Server Evaluations].</span></span>
2. <span data-ttu-id="bbde0-138">**Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="bbde0-138">**Visual Studio**.</span></span> <span data-ttu-id="bbde0-139">Az ingyenes Visual Studio Community Edition megtekinteni [Visual Studio Community][Visual Studio Community].</span><span class="sxs-lookup"><span data-stu-id="bbde0-139">To get the free Visual Studio Community Edition, see [Visual Studio Community][Visual Studio Community].</span></span>
3. <span data-ttu-id="bbde0-140">**SQL Server Data Tools for Visual Studio (SSDT)**.</span><span class="sxs-lookup"><span data-stu-id="bbde0-140">**SQL Server Data Tools for Visual Studio (SSDT)**.</span></span> <span data-ttu-id="bbde0-141">Ahhoz, hogy az SQL Server Data Tools for Visual Studio, lásd: [letöltése SQL Server Data Tools (SSDT)][Download SQL Server Data Tools (SSDT)].</span><span class="sxs-lookup"><span data-stu-id="bbde0-141">To get SQL Server Data Tools for Visual Studio, see [Download SQL Server Data Tools (SSDT)][Download SQL Server Data Tools (SSDT)].</span></span>
4. <span data-ttu-id="bbde0-142">**Az adatokat**.</span><span class="sxs-lookup"><span data-stu-id="bbde0-142">**Sample data**.</span></span> <span data-ttu-id="bbde0-143">Ez az oktatóanyag betöltését az SQL Data Warehouse használja a mintaadatok az SQL Server a forrásadatok az AdventureWorks mintaadatbázishoz tárolja.</span><span class="sxs-lookup"><span data-stu-id="bbde0-143">This tutorial uses sample data stored in SQL Server in the AdventureWorks sample database as the source data to be loaded into SQL Data Warehouse.</span></span> <span data-ttu-id="bbde0-144">Az AdventureWorks mintaadatbázishoz megtekinteni [AdventureWorks 2014 mintaadatbázisokat][AdventureWorks 2014 Sample Databases].</span><span class="sxs-lookup"><span data-stu-id="bbde0-144">To get the AdventureWorks sample database, see [AdventureWorks 2014 Sample Databases][AdventureWorks 2014 Sample Databases].</span></span>
5. <span data-ttu-id="bbde0-145">**Egy SQL Data Warehouse-adatbázis és az engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="bbde0-145">**A SQL Data Warehouse database and permissions**.</span></span> <span data-ttu-id="bbde0-146">Ez az oktatóanyag az SQL Data Warehouse-példányhoz, és adatokat tölt be.</span><span class="sxs-lookup"><span data-stu-id="bbde0-146">This tutorial connects to a SQL Data Warehouse instance and loads data into it.</span></span> <span data-ttu-id="bbde0-147">Van engedélye a tábla létrehozásához és adatok betöltésére.</span><span class="sxs-lookup"><span data-stu-id="bbde0-147">You have to have permissions to create a table and to load data.</span></span>
6. <span data-ttu-id="bbde0-148">**Egy tűzfalszabály**.</span><span class="sxs-lookup"><span data-stu-id="bbde0-148">**A firewall rule**.</span></span> <span data-ttu-id="bbde0-149">Előtt létre szeretne hozni egy tűzfalszabályt SQL Data warehouse a helyi számítógép IP-címmel feltöltheti az adatait az SQL Data warehouse rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="bbde0-149">You have to create a firewall rule on SQL Data Warehouse with the IP address of your local computer before you can upload data to the SQL Data Warehouse.</span></span>

## <a name="step-1-create-a-new-integration-services-project"></a><span data-ttu-id="bbde0-150">1. lépés: Új Integration Services-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="bbde0-150">Step 1: Create a new Integration Services project</span></span>
1. <span data-ttu-id="bbde0-151">Indítsa el a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bbde0-151">Launch Visual Studio.</span></span>
2. <span data-ttu-id="bbde0-152">Az a **fájl** menü **új |} Projekt**.</span><span class="sxs-lookup"><span data-stu-id="bbde0-152">On the **File** menu, select **New | Project**.</span></span>
3. <span data-ttu-id="bbde0-153">Keresse meg a **telepítve |} Sablonok |} Üzleti intelligencia |} Az integrációs szolgáltatások** projekt típusok.</span><span class="sxs-lookup"><span data-stu-id="bbde0-153">Navigate to the **Installed | Templates | Business Intelligence | Integration Services** project types.</span></span>
4. <span data-ttu-id="bbde0-154">Válassza ki **Integration Services-projekt**.</span><span class="sxs-lookup"><span data-stu-id="bbde0-154">Select **Integration Services Project**.</span></span> <span data-ttu-id="bbde0-155">Adjon meg értékeket a **neve** és **hely**, majd válassza ki **OK**.</span><span class="sxs-lookup"><span data-stu-id="bbde0-155">Provide values for **Name** and **Location**, and then select **OK**.</span></span>

<span data-ttu-id="bbde0-156">A Visual Studio megnyílik, és létrehoz egy új Integration Services (SSIS) projektet.</span><span class="sxs-lookup"><span data-stu-id="bbde0-156">Visual Studio opens and creates a new Integration Services (SSIS) project.</span></span> <span data-ttu-id="bbde0-157">Majd megnyílik a Visual Studio a tervező az egyetlen új SSIS-csomag (Package.dtsx) a projektben.</span><span class="sxs-lookup"><span data-stu-id="bbde0-157">Then Visual Studio opens the designer for the single new SSIS package (Package.dtsx) in the project.</span></span> <span data-ttu-id="bbde0-158">A következő területeken képernyő jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="bbde0-158">You see the following screen areas:</span></span>

* <span data-ttu-id="bbde0-159">A bal oldali a **eszközkészlet** SSIS-összetevők.</span><span class="sxs-lookup"><span data-stu-id="bbde0-159">On the left, the **Toolbox** of SSIS components.</span></span>
* <span data-ttu-id="bbde0-160">A középső, a tervezési felülethez, több lappal.</span><span class="sxs-lookup"><span data-stu-id="bbde0-160">In the middle, the design surface, with multiple tabs.</span></span> <span data-ttu-id="bbde0-161">Általában használja legalább az **vezérlő Flow** és a **adatfolyam** lapokon.</span><span class="sxs-lookup"><span data-stu-id="bbde0-161">You typically use at least the **Control Flow** and the **Data Flow** tabs.</span></span>
* <span data-ttu-id="bbde0-162">A jobb oldalon a **Megoldáskezelőben** és a **tulajdonságok** ablaktábla.</span><span class="sxs-lookup"><span data-stu-id="bbde0-162">On the right, the **Solution Explorer** and the **Properties** panes.</span></span>
  
    ![][01]

## <a name="step-2-create-the-basic-data-flow"></a><span data-ttu-id="bbde0-163">2. lépés: Az alapszintű adatfolyam létrehozása</span><span class="sxs-lookup"><span data-stu-id="bbde0-163">Step 2: Create the basic data flow</span></span>
1. <span data-ttu-id="bbde0-164">Húzzon Data Flow feladat az eszközkészletből a Tervező felület (a a **vezérlő Flow** lap).</span><span class="sxs-lookup"><span data-stu-id="bbde0-164">Drag a Data Flow Task from the Toolbox to the center of the design surface (on the **Control Flow** tab).</span></span>
   
    ![][02]
2. <span data-ttu-id="bbde0-165">Kattintson duplán az adatok áramlását feladat váltson át az adatfolyam-lapon.</span><span class="sxs-lookup"><span data-stu-id="bbde0-165">Double-click the Data Flow Task to switch to the Data Flow tab.</span></span>
3. <span data-ttu-id="bbde0-166">Az eszközkészlet egyéb adatforrások listájából húzza az ADO.NET forrás a Tervező felületére.</span><span class="sxs-lookup"><span data-stu-id="bbde0-166">From the Other Sources list in the Toolbox, drag an ADO.NET Source to the design surface.</span></span> <span data-ttu-id="bbde0-167">A kijelölt adatforrás adapterrel, módosítsa a nevét, hogy **SQL Server-adatforrás** a a **tulajdonságok** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="bbde0-167">With the source adapter still selected, change its name to **SQL Server source** in the **Properties** pane.</span></span>
4. <span data-ttu-id="bbde0-168">Az eszközkészlet egyéb célok listájából húzza az ADO.NET cél ADO.NET forrás alatt a Tervező felületére.</span><span class="sxs-lookup"><span data-stu-id="bbde0-168">From the Other Destinations list in the Toolbox, drag an ADO.NET Destination to the design surface under the ADO.NET Source.</span></span> <span data-ttu-id="bbde0-169">A cél adapterrel kijelölését, módosítsa a nevét, hogy **SQL DW cél** a a **tulajdonságok** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="bbde0-169">With the destination adapter still selected, change its name to **SQL DW destination** in the **Properties** pane.</span></span>
   
    ![][09]

## <a name="step-3-configure-the-source-adapter"></a><span data-ttu-id="bbde0-170">3. lépés: A forrás-adapter konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="bbde0-170">Step 3: Configure the source adapter</span></span>
1. <span data-ttu-id="bbde0-171">A forrás adapter megnyitásához kattintson duplán a **ADO.NET forrás szerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="bbde0-171">Double-click the source adapter to open the **ADO.NET Source Editor**.</span></span>
   
    ![][03]
2. <span data-ttu-id="bbde0-172">A a **Csatlakozáskezelő** lapján a **ADO.NET forrás szerkesztő**, kattintson a **új** megjelenítő gombra a **ADO.NET Csatlakozáskezelő** nyissa meg a listában a **konfigurálása ADO.NET Csatlakozáskezelő** párbeszédpanel és hozza létre, amelyből ez az oktatóanyag adatokat tölt az SQL Server-adatbázis biztonságoskapcsolat-beállításainak.</span><span class="sxs-lookup"><span data-stu-id="bbde0-172">On the **Connection Manager** tab of the **ADO.NET Source Editor**, click the **New** button next to the **ADO.NET connection manager** list to open the **Configure ADO.NET Connection Manager** dialog box and create connection settings for the SQL Server database from which this tutorial loads data.</span></span>
   
    ![][04]
3. <span data-ttu-id="bbde0-173">Az a **konfigurálása ADO.NET Csatlakozáskezelő** párbeszédpanel, kattintson a **új** gombra kattintva nyissa meg a **Csatlakozáskezelő** párbeszédpanel és hozza létre az új kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="bbde0-173">In the **Configure ADO.NET Connection Manager** dialog box, click the **New** button to open the **Connection Manager** dialog box and create a new data connection.</span></span>
   
    ![][05]
4. <span data-ttu-id="bbde0-174">Az a **Csatlakozáskezelő** párbeszédpanelen tegye a következőket.</span><span class="sxs-lookup"><span data-stu-id="bbde0-174">In the **Connection Manager** dialog box, do the following things.</span></span>
   
   1. <span data-ttu-id="bbde0-175">A **szolgáltató**, válassza ki az SqlClient adatszolgáltatója.</span><span class="sxs-lookup"><span data-stu-id="bbde0-175">For **Provider**, select the SqlClient Data Provider.</span></span>
   2. <span data-ttu-id="bbde0-176">A **kiszolgálónév**, adja meg az SQL Server nevét.</span><span class="sxs-lookup"><span data-stu-id="bbde0-176">For **Server name**, enter the SQL Server name.</span></span>
   3. <span data-ttu-id="bbde0-177">Az a **jelentkezzen be a kiszolgálóra** szakaszban válassza ki vagy adja meg a hitelesítési adatokat.</span><span class="sxs-lookup"><span data-stu-id="bbde0-177">In the **Log on to the server** section, select or enter authentication information.</span></span>
   4. <span data-ttu-id="bbde0-178">Az a **csatlakozás az adatbázishoz** területen válassza ki az AdventureWorks mintaadatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="bbde0-178">In the **Connect to a database** section, select the AdventureWorks sample database.</span></span>
   5. <span data-ttu-id="bbde0-179">Kattintson a **tesztkapcsolat**.</span><span class="sxs-lookup"><span data-stu-id="bbde0-179">Click **Test Connection**.</span></span>
      
       ![][06]
   6. <span data-ttu-id="bbde0-180">Kattintson a párbeszédpanel, amely a kapcsolat vizsgálati eredményeket jelent, **OK** való visszatéréshez a **Csatlakozáskezelő** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="bbde0-180">In the dialog box that reports the results of the connection test, click **OK** to return to the **Connection Manager** dialog box.</span></span>
   7. <span data-ttu-id="bbde0-181">Az a **Csatlakozáskezelő** párbeszédpanel, kattintson a **OK** való visszatéréshez a **konfigurálása ADO.NET Csatlakozáskezelő** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bbde0-181">In the **Connection Manager** dialog box, click **OK** to return to the **Configure ADO.NET Connection Manager** dialog box.</span></span>
5. <span data-ttu-id="bbde0-182">Az a **konfigurálása ADO.NET Csatlakozáskezelő** párbeszédpanel, kattintson a **OK** való visszatéréshez a **ADO.NET adatforrás-szerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="bbde0-182">In the **Configure ADO.NET Connection Manager** dialog box, click **OK** to return to the **ADO.NET Source Editor**.</span></span>
6. <span data-ttu-id="bbde0-183">Az a **ADO.NET forrás szerkesztő**, a a **a tábla vagy a nézet neve** listáról válassza ki a **Sales.SalesOrderDetail** tábla.</span><span class="sxs-lookup"><span data-stu-id="bbde0-183">In the **ADO.NET Source Editor**, in the **Name of the table or the view** list, select the **Sales.SalesOrderDetail** table.</span></span>
   
    ![][07]
7. <span data-ttu-id="bbde0-184">Kattintson a **előzetes** az első 200 sorokat a forrás táblázatban szereplő adatok a **lekérdezés eredményének villámnézete** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="bbde0-184">Click **Preview** to see the first 200 rows of data in the source table in the **Preview Query Results** dialog box.</span></span>
   
    ![][08]
8. <span data-ttu-id="bbde0-185">Az a **lekérdezés eredményének villámnézete** párbeszédpanel, kattintson a **Bezárás** való visszatéréshez a **ADO.NET forrás szerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="bbde0-185">In the **Preview Query Results** dialog box, click **Close** to return to the **ADO.NET Source Editor**.</span></span>
9. <span data-ttu-id="bbde0-186">Az a **ADO.NET forrás szerkesztő**, kattintson a **OK** az adatforrás konfigurálásának befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="bbde0-186">In the **ADO.NET Source Editor**, click **OK** to finish configuring the data source.</span></span>

## <a name="step-4-connect-the-source-adapter-to-the-destination-adapter"></a><span data-ttu-id="bbde0-187">4. lépés: A forrás-adaptert csatlakoztatni a céladapter</span><span class="sxs-lookup"><span data-stu-id="bbde0-187">Step 4: Connect the source adapter to the destination adapter</span></span>
1. <span data-ttu-id="bbde0-188">Válassza ki a tervezési felülethez a forrás-adapteréből.</span><span class="sxs-lookup"><span data-stu-id="bbde0-188">Select the source adapter on the design surface.</span></span>
2. <span data-ttu-id="bbde0-189">Válassza ki a kék nyíl, amely kiterjeszti a forrás-adapternek, és húzza azt a cél-szerkesztő, amíg a helyen igazodik.</span><span class="sxs-lookup"><span data-stu-id="bbde0-189">Select the blue arrow that extends from the source adapter and drag it to the destination editor until it snaps into place.</span></span>
   
    ![][10]
   
    <span data-ttu-id="bbde0-190">A tipikus SSIS-csomag segítségével számos más összetevők a forrás- és a cél Between SSIS eszközkészletből átalakításának, átalakítási és nagybetűhasználatának az adatokat, ahogy keresztülhalad az SSIS-adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="bbde0-190">In a typical SSIS package, you use a number of other components from the SSIS Toolbox in between the source and the destination to restructure, transform, and cleanse your data as it passes through the SSIS data flow.</span></span> <span data-ttu-id="bbde0-191">Ez a példa lehető legegyszerűbb megtartásához azt csatlakozik a forrás közvetlenül a cél.</span><span class="sxs-lookup"><span data-stu-id="bbde0-191">To keep this example as simple as possible, we’re connecting the source directly to the destination.</span></span>

## <a name="step-5-configure-the-destination-adapter"></a><span data-ttu-id="bbde0-192">5. lépés: Konfigurálja a céladapter</span><span class="sxs-lookup"><span data-stu-id="bbde0-192">Step 5: Configure the destination adapter</span></span>
1. <span data-ttu-id="bbde0-193">A célként megadott adapter megnyitásához kattintson duplán a **ADO.NET cél szerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="bbde0-193">Double-click the destination adapter to open the **ADO.NET Destination Editor**.</span></span>
   
    ![][11]
2. <span data-ttu-id="bbde0-194">A a **Csatlakozáskezelő** lapján a **ADO.NET cél szerkesztő**, kattintson a **új** megjelenítő gombra a **Csatlakozáskezelő** nyissa meg a listában a **konfigurálása ADO.NET Csatlakozáskezelő** párbeszédpanel és hozza létre a kapcsolat beállításait az Azure SQL Data Warehouse-adatbázist, amelybe ez az oktatóanyag adatokat tölt.</span><span class="sxs-lookup"><span data-stu-id="bbde0-194">On the **Connection Manager** tab of the **ADO.NET Destination Editor**, click the **New** button next to the **Connection manager** list to open the **Configure ADO.NET Connection Manager** dialog box and create connection settings for the Azure SQL Data Warehouse database into which this tutorial loads data.</span></span>
3. <span data-ttu-id="bbde0-195">Az a **konfigurálása ADO.NET Csatlakozáskezelő** párbeszédpanel, kattintson a **új** gombra kattintva nyissa meg a **Csatlakozáskezelő** párbeszédpanel és hozza létre az új kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="bbde0-195">In the **Configure ADO.NET Connection Manager** dialog box, click the **New** button to open the **Connection Manager** dialog box and create a new data connection.</span></span>
4. <span data-ttu-id="bbde0-196">Az a **Csatlakozáskezelő** párbeszédpanelen tegye a következőket.</span><span class="sxs-lookup"><span data-stu-id="bbde0-196">In the **Connection Manager** dialog box, do the following things.</span></span>
   1. <span data-ttu-id="bbde0-197">A **szolgáltató**, válassza ki az SqlClient adatszolgáltatója.</span><span class="sxs-lookup"><span data-stu-id="bbde0-197">For **Provider**, select the SqlClient Data Provider.</span></span>
   2. <span data-ttu-id="bbde0-198">A **kiszolgálónév**, adja meg az SQL Data Warehouse nevét.</span><span class="sxs-lookup"><span data-stu-id="bbde0-198">For **Server name**, enter the SQL Data Warehouse name.</span></span>
   3. <span data-ttu-id="bbde0-199">Az a **jelentkezzen be a kiszolgálóra** szakaszban jelölje be **használja az SQL Server-hitelesítési** , és írja be a hitelesítési adatokat.</span><span class="sxs-lookup"><span data-stu-id="bbde0-199">In the **Log on to the server** section, select **Use SQL Server authentication** and enter authentication information.</span></span>
   4. <span data-ttu-id="bbde0-200">Az a **csatlakozás az adatbázishoz** területen válasszon ki egy létező SQL Data Warehouse-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="bbde0-200">In the **Connect to a database** section, select an existing SQL Data Warehouse database.</span></span>
   5. <span data-ttu-id="bbde0-201">Kattintson a **tesztkapcsolat**.</span><span class="sxs-lookup"><span data-stu-id="bbde0-201">Click **Test Connection**.</span></span>
   6. <span data-ttu-id="bbde0-202">Kattintson a párbeszédpanel, amely a kapcsolat vizsgálati eredményeket jelent, **OK** való visszatéréshez a **Csatlakozáskezelő** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="bbde0-202">In the dialog box that reports the results of the connection test, click **OK** to return to the **Connection Manager** dialog box.</span></span>
   7. <span data-ttu-id="bbde0-203">Az a **Csatlakozáskezelő** párbeszédpanel, kattintson a **OK** való visszatéréshez a **konfigurálása ADO.NET Csatlakozáskezelő** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bbde0-203">In the **Connection Manager** dialog box, click **OK** to return to the **Configure ADO.NET Connection Manager** dialog box.</span></span>
5. <span data-ttu-id="bbde0-204">Az a **konfigurálása ADO.NET Csatlakozáskezelő** párbeszédpanel, kattintson a **OK** való visszatéréshez a **ADO.NET cél szerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="bbde0-204">In the **Configure ADO.NET Connection Manager** dialog box, click **OK** to return to the **ADO.NET Destination Editor**.</span></span>
6. <span data-ttu-id="bbde0-205">Az a **ADO.NET cél szerkesztő**, kattintson a **új** mellett a **használni kívánt táblát vagy nézetet** nyissa meg a listában a **Create Table** párbeszédpanel cél új tábla létrehozása, amely megfelel a forrástábla oszloplistával együtt.</span><span class="sxs-lookup"><span data-stu-id="bbde0-205">In the **ADO.NET Destination Editor**, click **New** next to the **Use a table or view** list to open the **Create Table** dialog box to create a new destination table with a column list that matches the source table.</span></span>
   
    ![][12a]
7. <span data-ttu-id="bbde0-206">Az a **Create Table** párbeszédpanelen tegye a következőket.</span><span class="sxs-lookup"><span data-stu-id="bbde0-206">In the **Create Table** dialog box, do the following things.</span></span>
   
   1. <span data-ttu-id="bbde0-207">A célként megadott tábla nevének módosítása **értékesítési rendelési adatot**.</span><span class="sxs-lookup"><span data-stu-id="bbde0-207">Change the name of the destination table to **SalesOrderDetail**.</span></span>
   2. <span data-ttu-id="bbde0-208">Távolítsa el a **rowguid** oszlop.</span><span class="sxs-lookup"><span data-stu-id="bbde0-208">Remove the **rowguid** column.</span></span> <span data-ttu-id="bbde0-209">A **uniqueidentifier** adattípusa nem támogatott az SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="bbde0-209">The **uniqueidentifier** data type is not supported in SQL Data Warehouse.</span></span>
   3. <span data-ttu-id="bbde0-210">Módosítja az adattípust, a **LineTotal** oszlop **pénz**.</span><span class="sxs-lookup"><span data-stu-id="bbde0-210">Change the data type of the **LineTotal** column to **money**.</span></span> <span data-ttu-id="bbde0-211">A **decimális** adattípusa nem támogatott az SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="bbde0-211">The **decimal** data type is not supported in SQL Data Warehouse.</span></span> <span data-ttu-id="bbde0-212">További információt a támogatott adattípusok, lásd: [CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)][CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)].</span><span class="sxs-lookup"><span data-stu-id="bbde0-212">For info about supported data types, see [CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)][CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)].</span></span>
      
       ![][12b]
   4. <span data-ttu-id="bbde0-213">Kattintson a **OK** a tábla létrehozásához, és térjen vissza a **ADO.NET cél szerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="bbde0-213">Click **OK** to create the table and return to the **ADO.NET Destination Editor**.</span></span>
8. <span data-ttu-id="bbde0-214">Az a **ADO.NET cél szerkesztő**, jelölje be a **hozzárendelések** lapon, láthatja, hogyan oszlop szerepel a forrás oszlop szerepel a cél van leképezve.</span><span class="sxs-lookup"><span data-stu-id="bbde0-214">In the **ADO.NET Destination Editor**, select the **Mappings** tab to see how columns in the source are mapped to columns in the destination.</span></span>
   
    ![][13]
9. <span data-ttu-id="bbde0-215">Kattintson a **OK** az adatforrás konfigurálásának befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="bbde0-215">Click **OK** to finish configuring the data source.</span></span>

## <a name="step-6-run-the-package-to-load-the-data"></a><span data-ttu-id="bbde0-216">6. lépés: A csomag az adatok betöltéséhez futtassa</span><span class="sxs-lookup"><span data-stu-id="bbde0-216">Step 6: Run the package to load the data</span></span>
<span data-ttu-id="bbde0-217">Futtassa a gombra kattintva a **Start** gomb az eszköztáron vagy egy a **futtatása** a lehetőségek a **Debug** menü.</span><span class="sxs-lookup"><span data-stu-id="bbde0-217">Run the package by clicking the **Start** button on the toolbar or by selecting one of the **Run** options on the **Debug** menu.</span></span>

<span data-ttu-id="bbde0-218">A csomag kezdődik el, mivel látni sárga forgó kerekek tevékenység, valamint a feldolgozott eddig sorok számát jelzi.</span><span class="sxs-lookup"><span data-stu-id="bbde0-218">As the package begins to run, you see yellow spinning wheels to indicate activity as well as the number of rows processed so far.</span></span>

![][14]

<span data-ttu-id="bbde0-219">Ha a csomag futása befejeződött, megjelenik az zöld jelölését annak jelzésére, sikeres, valamint az adatok betöltése a forrásból történő sorainak száma.</span><span class="sxs-lookup"><span data-stu-id="bbde0-219">When the package has finished running, you see green check marks to indicate success as well as the total number of rows of data loaded from the source to the destination.</span></span>

![][15]

<span data-ttu-id="bbde0-220">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="bbde0-220">Congratulations!</span></span> <span data-ttu-id="bbde0-221">Adatok betöltése az Azure SQL Data Warehouse sikeresen használt SQL Server Integration Services.</span><span class="sxs-lookup"><span data-stu-id="bbde0-221">You’ve successfully used SQL Server Integration Services to load data into Azure SQL Data Warehouse.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bbde0-222">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bbde0-222">Next steps</span></span>
* <span data-ttu-id="bbde0-223">További tudnivalók az SSIS-adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="bbde0-223">Learn more about the SSIS data flow.</span></span> <span data-ttu-id="bbde0-224">Kezdje itt: [adatfolyam][Data Flow].</span><span class="sxs-lookup"><span data-stu-id="bbde0-224">Start here: [Data Flow][Data Flow].</span></span>
* <span data-ttu-id="bbde0-225">Útmutató a Hibakeresés és hibaelhárítás a csomagok jobbra a tervezési környezetben.</span><span class="sxs-lookup"><span data-stu-id="bbde0-225">Learn how to debug and troubleshoot your packages right in the design environment.</span></span> <span data-ttu-id="bbde0-226">Kezdje itt: [hibaelhárítási eszközök csomag fejlesztési][Troubleshooting Tools for Package Development].</span><span class="sxs-lookup"><span data-stu-id="bbde0-226">Start here: [Troubleshooting Tools for Package Development][Troubleshooting Tools for Package Development].</span></span>
* <span data-ttu-id="bbde0-227">Megtudhatja, hogyan telepítheti a SSIS-projektek és csomagok Integration Services-kiszolgáló vagy egy másik tárolóhelyre.</span><span class="sxs-lookup"><span data-stu-id="bbde0-227">Learn how to deploy your SSIS projects and packages to Integration Services Server or to another storage location.</span></span> <span data-ttu-id="bbde0-228">Kezdje itt: [telepítési a projekt és csomagok][Deployment of Projects and Packages].</span><span class="sxs-lookup"><span data-stu-id="bbde0-228">Start here: [Deployment of Projects and Packages][Deployment of Projects and Packages].</span></span>

<!-- Image references -->
[01]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-designer-01.png
[02]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-data-flow-task-02.png
[03]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-03.png
[04]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-manager-04.png
[05]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-05.png
[06]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/test-connection-06.png
[07]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-07.png
[08]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/preview-data-08.png
[09]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/source-destination-09.png
[10]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/connect-source-destination-10.png
[11]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-destination-11.png
[12a]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-before-12a.png
[12b]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-after-12b.png
[13]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/column-mapping-13.png
[14]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-running-14.png
[15]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-success-15.png

<!-- Article references -->

<!-- MSDN references -->
[PolyBase Guide]: https://msdn.microsoft.com/library/mt143171.aspx
[Download SQL Server Data Tools (SSDT)]: https://msdn.microsoft.com/library/mt204009.aspx
[CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)]: https://msdn.microsoft.com/library/mt203953.aspx
[Data Flow]: https://msdn.microsoft.com/library/ms140080.aspx
[Troubleshooting Tools for Package Development]: https://msdn.microsoft.com/library/ms137625.aspx
[Deployment of Projects and Packages]: https://msdn.microsoft.com/library/hh213290.aspx

<!--Other Web references-->
[Microsoft SQL Server 2016 Integration Services Feature Pack for Azure]: http://go.microsoft.com/fwlink/?LinkID=626967
[SQL Server Evaluations]: https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server-2016
[Visual Studio Community]: https://www.visualstudio.com/en-us/products/visual-studio-community-vs.aspx
[AdventureWorks 2014 Sample Databases]: https://msftdbprodsamples.codeplex.com/releases/view/125550
