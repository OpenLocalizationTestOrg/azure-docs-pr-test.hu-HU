---
title: SQL Server be Azure SQL Data Warehouse (SSIS) aaaLoad adatait |} Microsoft Docs
description: "Bemutatja, hogyan toocreate egy SQL Server Integration Services (SSIS) csomag toomove adatokat az adatok különböző forrásokból tooSQL Data warehouse-bA."
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
ms.openlocfilehash: bb28a08807a5b07832b85f2f074c2acf912c1dc3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-ssis"></a><span data-ttu-id="3ea0a-103">Adatok betöltése az SQL Server be Azure SQL Data Warehouse (SSIS)</span><span class="sxs-lookup"><span data-stu-id="3ea0a-103">Load data from SQL Server into Azure SQL Data Warehouse (SSIS)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3ea0a-104">SSIS</span><span class="sxs-lookup"><span data-stu-id="3ea0a-104">SSIS</span></span>](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
> * [<span data-ttu-id="3ea0a-105">PolyBase</span><span class="sxs-lookup"><span data-stu-id="3ea0a-105">PolyBase</span></span>](sql-data-warehouse-load-from-sql-server-with-polybase.md)
> * [<span data-ttu-id="3ea0a-106">bcp</span><span class="sxs-lookup"><span data-stu-id="3ea0a-106">bcp</span></span>](sql-data-warehouse-load-from-sql-server-with-bcp.md)
> 
> 

<span data-ttu-id="3ea0a-107">Hozzon létre egy SQL Server Integration Services (SSIS) csomag tooload adatok SQL Server az Azure SQL Data Warehouse között.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-107">Create a SQL Server Integration Services (SSIS) package tooload data from SQL Server into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="3ea0a-108">Opcionálisan átalakításának, átalakítási és nagybetűhasználatának hello adatokat, ahogy keresztülhalad hello SSIS-adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-108">You can optionally restructure, transform, and cleanse hello data as it passes through hello SSIS data flow.</span></span>

<span data-ttu-id="3ea0a-109">Az oktatóanyag során az alábbi lépéseket fogja végrehajtani:</span><span class="sxs-lookup"><span data-stu-id="3ea0a-109">In this tutorial, you will:</span></span>

* <span data-ttu-id="3ea0a-110">Az integrációs szolgáltatások új projekt létrehozása a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-110">Create a new Integration Services project in Visual Studio.</span></span>
* <span data-ttu-id="3ea0a-111">Csatlakozás toodata móddal, többek között az SQL Server (mint a forrás-) és az SQL Data Warehouse (célként).</span><span class="sxs-lookup"><span data-stu-id="3ea0a-111">Connect toodata sources, including SQL Server (as a source) and SQL Data Warehouse (as a destination).</span></span>
* <span data-ttu-id="3ea0a-112">Tervezze meg az SSIS-csomagot, amely hello forrásból származó adatokat tölt be hello cél.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-112">Design an SSIS package that loads data from hello source into hello destination.</span></span>
* <span data-ttu-id="3ea0a-113">Futtassa a hello SSIS tooload hello adatait.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-113">Run hello SSIS package tooload hello data.</span></span>

<span data-ttu-id="3ea0a-114">Ez az oktatóanyag az SQL Server hello adatforrásként.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-114">This tutorial uses SQL Server as hello data source.</span></span> <span data-ttu-id="3ea0a-115">SQL Server sikerült fut, a helyszíni vagy egy Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-115">SQL Server could be running on premises or in an Azure virtual machine.</span></span>

## <a name="basic-concepts"></a><span data-ttu-id="3ea0a-116">Alapfogalmak</span><span class="sxs-lookup"><span data-stu-id="3ea0a-116">Basic concepts</span></span>
<span data-ttu-id="3ea0a-117">hello csomag hello munkaegysége SSIS.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-117">hello package is hello unit of work in SSIS.</span></span> <span data-ttu-id="3ea0a-118">Kapcsolódó csomagok projektek vannak csoportosítva.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-118">Related packages are grouped in projects.</span></span> <span data-ttu-id="3ea0a-119">Projektek és a Tervező csomagok létrehozása a Visual Studio és az SQL Server Data Tools összetevővel.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-119">You create projects and design packages in Visual Studio with SQL Server Data Tools.</span></span> <span data-ttu-id="3ea0a-120">egy visual folyamat, amelyben Ön áthúzása összetevők hello eszközkészlet toohello tervezési felületéhez hello tervezési csatlakoztassa őket, és állítsa be tulajdonságaikat.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-120">hello design process is a visual process in which you drag and drop components from hello Toolbox toohello design surface, connect them, and set their properties.</span></span> <span data-ttu-id="3ea0a-121">Ha befejezte a csomag, igény szerint telepítheti azt tooSQL kiszolgáló átfogó kezelési, figyelési és biztonsági.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-121">After you finish your package, you can optionally deploy it tooSQL Server for comprehensive management, monitoring, and security.</span></span>

## <a name="options-for-loading-data-with-ssis"></a><span data-ttu-id="3ea0a-122">Beállítások SSIS az adatok betöltése</span><span class="sxs-lookup"><span data-stu-id="3ea0a-122">Options for loading data with SSIS</span></span>
<span data-ttu-id="3ea0a-123">SQL Server Integration Services (SSIS), amely számos lehetőséget biztosít csatlakozik, és az adatok betöltése az SQL Data Warehouse rugalmas eszközkészlet.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-123">SQL Server Integration Services (SSIS) is a flexible set of tools that provides a variety of options for connecting to, and loading data into, SQL Data Warehouse.</span></span>

1. <span data-ttu-id="3ea0a-124">Használjon egy ADO NET cél tooconnect tooSQL Data warehouse-bA.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-124">Use an ADO NET Destination tooconnect tooSQL Data Warehouse.</span></span> <span data-ttu-id="3ea0a-125">Ez az oktatóanyag egy ADO NET cél használja, mert a hello legkevesebb konfigurációs beállításokat.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-125">This tutorial uses an ADO NET Destination because it has hello fewest configuration options.</span></span>
2. <span data-ttu-id="3ea0a-126">Az OLE DB cél tooconnect tooSQL adatraktár használja.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-126">Use an OLE DB Destination tooconnect tooSQL Data Warehouse.</span></span> <span data-ttu-id="3ea0a-127">Ez a beállítás által biztosított hello ADO NET cél némileg jobb teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-127">This option may provide slightly better performance than hello ADO NET Destination.</span></span>
3. <span data-ttu-id="3ea0a-128">Hello Azure Blob feltöltése feladat toostage hello adatokat használja az Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-128">Use hello Azure Blob Upload Task toostage hello data in Azure Blob Storage.</span></span> <span data-ttu-id="3ea0a-129">Ezután használja az hello SSIS SQL végrehajtása a feladat toolaunch hello adatok betöltése az SQL Data Warehouse Polybase parancsfájlból.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-129">Then use hello SSIS Execute SQL task toolaunch a Polybase script that loads hello data into SQL Data Warehouse.</span></span> <span data-ttu-id="3ea0a-130">Ez a beállítás hello három lehetőség az itt felsorolt hello legjobb teljesítményt biztosít.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-130">This option provides hello best performance of hello three options listed here.</span></span> <span data-ttu-id="3ea0a-131">tooget hello Azure Blob feltöltése feladat letöltése hello [Microsoft SQL Server 2016 Integration Services funkciócsomag Azure][Microsoft SQL Server 2016 Integration Services Feature Pack for Azure].</span><span class="sxs-lookup"><span data-stu-id="3ea0a-131">tooget hello Azure Blob Upload task, download hello [Microsoft SQL Server 2016 Integration Services Feature Pack for Azure][Microsoft SQL Server 2016 Integration Services Feature Pack for Azure].</span></span> <span data-ttu-id="3ea0a-132">toolearn Polybase kapcsolatos további információkért lásd: [PolyBase-útmutatóban][PolyBase Guide].</span><span class="sxs-lookup"><span data-stu-id="3ea0a-132">toolearn more about Polybase, see [PolyBase Guide][PolyBase Guide].</span></span>

## <a name="before-you-start"></a><span data-ttu-id="3ea0a-133">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="3ea0a-133">Before you start</span></span>
<span data-ttu-id="3ea0a-134">az oktatóanyag teljesítéséhez toostep, lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="3ea0a-134">toostep through this tutorial, you need:</span></span>

1. <span data-ttu-id="3ea0a-135">**Az SQL Server Integration Services (SSIS)**.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-135">**SQL Server Integration Services (SSIS)**.</span></span> <span data-ttu-id="3ea0a-136">SSIS az SQL Server, és próbaverziójáról vagy az SQL Server licenccel rendelkező verzió szükséges.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-136">SSIS is a component of SQL Server and requires an evaluation version or a licensed version of SQL Server.</span></span> <span data-ttu-id="3ea0a-137">SQL Server 2016 Preview próbaverzióját tooget lásd [SQL Server értékelések][SQL Server Evaluations].</span><span class="sxs-lookup"><span data-stu-id="3ea0a-137">tooget an evaluation version of SQL Server 2016 Preview, see [SQL Server Evaluations][SQL Server Evaluations].</span></span>
2. <span data-ttu-id="3ea0a-138">**Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-138">**Visual Studio**.</span></span> <span data-ttu-id="3ea0a-139">tooget ingyenes Visual Studio Community Edition hello című [Visual Studio Community][Visual Studio Community].</span><span class="sxs-lookup"><span data-stu-id="3ea0a-139">tooget hello free Visual Studio Community Edition, see [Visual Studio Community][Visual Studio Community].</span></span>
3. <span data-ttu-id="3ea0a-140">**SQL Server Data Tools for Visual Studio (SSDT)**.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-140">**SQL Server Data Tools for Visual Studio (SSDT)**.</span></span> <span data-ttu-id="3ea0a-141">SQL Server Data Tools for Visual Studio tooget lásd [letöltése SQL Server Data Tools (SSDT)][Download SQL Server Data Tools (SSDT)].</span><span class="sxs-lookup"><span data-stu-id="3ea0a-141">tooget SQL Server Data Tools for Visual Studio, see [Download SQL Server Data Tools (SSDT)][Download SQL Server Data Tools (SSDT)].</span></span>
4. <span data-ttu-id="3ea0a-142">**Az adatokat**.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-142">**Sample data**.</span></span> <span data-ttu-id="3ea0a-143">Ez az oktatóanyag tárolva az SQL Server hello AdventureWorks mintaadatbázishoz source adatok toobe SQL Data Warehouse-bA betöltött hello mintaadatokat használja.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-143">This tutorial uses sample data stored in SQL Server in hello AdventureWorks sample database as hello source data toobe loaded into SQL Data Warehouse.</span></span> <span data-ttu-id="3ea0a-144">tooget hello AdventureWorks mintaadatbázishoz, lásd: [AdventureWorks 2014 mintaadatbázisokat][AdventureWorks 2014 Sample Databases].</span><span class="sxs-lookup"><span data-stu-id="3ea0a-144">tooget hello AdventureWorks sample database, see [AdventureWorks 2014 Sample Databases][AdventureWorks 2014 Sample Databases].</span></span>
5. <span data-ttu-id="3ea0a-145">**Egy SQL Data Warehouse-adatbázis és az engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-145">**A SQL Data Warehouse database and permissions**.</span></span> <span data-ttu-id="3ea0a-146">Ez az oktatóanyag tooa SQL Data Warehouse-példányhoz csatlakozik, és adatokat tölt be.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-146">This tutorial connects tooa SQL Data Warehouse instance and loads data into it.</span></span> <span data-ttu-id="3ea0a-147">Toohave engedélyek toocreate egy tábla és tooload adatokkal rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-147">You have toohave permissions toocreate a table and tooload data.</span></span>
6. <span data-ttu-id="3ea0a-148">**Egy tűzfalszabály**.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-148">**A firewall rule**.</span></span> <span data-ttu-id="3ea0a-149">Van toocreate tűzfalszabály SQL Data Warehouse hello IP-cím a helyi számítógép adatok toohello SQL Data Warehouse feltöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-149">You have toocreate a firewall rule on SQL Data Warehouse with hello IP address of your local computer before you can upload data toohello SQL Data Warehouse.</span></span>

## <a name="step-1-create-a-new-integration-services-project"></a><span data-ttu-id="3ea0a-150">1. lépés: Új Integration Services-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="3ea0a-150">Step 1: Create a new Integration Services project</span></span>
1. <span data-ttu-id="3ea0a-151">Indítsa el a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-151">Launch Visual Studio.</span></span>
2. <span data-ttu-id="3ea0a-152">A hello **fájl** menü **új |} Projekt**.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-152">On hello **File** menu, select **New | Project**.</span></span>
3. <span data-ttu-id="3ea0a-153">Keresse meg a toohello **telepített |} Sablonok |} Üzleti intelligencia |} Az integrációs szolgáltatások** projekt típusok.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-153">Navigate toohello **Installed | Templates | Business Intelligence | Integration Services** project types.</span></span>
4. <span data-ttu-id="3ea0a-154">Válassza ki **Integration Services-projekt**.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-154">Select **Integration Services Project**.</span></span> <span data-ttu-id="3ea0a-155">Adjon meg értékeket a **neve** és **hely**, majd válassza ki **OK**.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-155">Provide values for **Name** and **Location**, and then select **OK**.</span></span>

<span data-ttu-id="3ea0a-156">A Visual Studio megnyílik, és létrehoz egy új Integration Services (SSIS) projektet.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-156">Visual Studio opens and creates a new Integration Services (SSIS) project.</span></span> <span data-ttu-id="3ea0a-157">Majd megnyílik a Visual Studio hello designer hello egyetlen új SSIS-csomag (Package.dtsx) hello projektben.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-157">Then Visual Studio opens hello designer for hello single new SSIS package (Package.dtsx) in hello project.</span></span> <span data-ttu-id="3ea0a-158">A következő képernyő területek hello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="3ea0a-158">You see hello following screen areas:</span></span>

* <span data-ttu-id="3ea0a-159">Hello bal oldali hello **eszközkészlet** SSIS-összetevők.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-159">On hello left, hello **Toolbox** of SSIS components.</span></span>
* <span data-ttu-id="3ea0a-160">Hello középső hello több lappal a tervezési felülethez.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-160">In hello middle, hello design surface, with multiple tabs.</span></span> <span data-ttu-id="3ea0a-161">Általában használja legalább hello **vezérlő Flow** és hello **adatfolyam** lapokon.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-161">You typically use at least hello **Control Flow** and hello **Data Flow** tabs.</span></span>
* <span data-ttu-id="3ea0a-162">A jobb oldali hello hello **Megoldáskezelőben** és hello **tulajdonságok** ablaktábla.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-162">On hello right, hello **Solution Explorer** and hello **Properties** panes.</span></span>
  
    ![][01]

## <a name="step-2-create-hello-basic-data-flow"></a><span data-ttu-id="3ea0a-163">2. lépés: Hello alapszintű adatfolyam létrehozása</span><span class="sxs-lookup"><span data-stu-id="3ea0a-163">Step 2: Create hello basic data flow</span></span>
1. <span data-ttu-id="3ea0a-164">Az egérrel húzzon át egy Data Flow feladat hello eszközkészlet toohello közepére hello a tervezési felülethez (a hello **vezérlő Flow** lap).</span><span class="sxs-lookup"><span data-stu-id="3ea0a-164">Drag a Data Flow Task from hello Toolbox toohello center of hello design surface (on hello **Control Flow** tab).</span></span>
   
    ![][02]
2. <span data-ttu-id="3ea0a-165">Kattintson duplán a hello Data Flow feladat tooswitch toohello adatfolyam-lapon.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-165">Double-click hello Data Flow Task tooswitch toohello Data Flow tab.</span></span>
3. <span data-ttu-id="3ea0a-166">Hello eszközkészlet hello más adatforrások listájában húzza egy ADO.NET forrás toohello a tervezési felülethez.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-166">From hello Other Sources list in hello Toolbox, drag an ADO.NET Source toohello design surface.</span></span> <span data-ttu-id="3ea0a-167">Hello forrás adapter kijelölését, megváltoztathatja a neve túl**SQL Server-adatforrás** a hello **tulajdonságok** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-167">With hello source adapter still selected, change its name too**SQL Server source** in hello **Properties** pane.</span></span>
4. <span data-ttu-id="3ea0a-168">Hello eszközkészlet hello más célhelyre listájából húzza az ADO.NET cél toohello tervezési felülethez hello ADO.NET forrás alatt.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-168">From hello Other Destinations list in hello Toolbox, drag an ADO.NET Destination toohello design surface under hello ADO.NET Source.</span></span> <span data-ttu-id="3ea0a-169">Hello cél adapter kijelölését, megváltoztathatja a neve túl**SQL DW cél** a hello **tulajdonságok** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-169">With hello destination adapter still selected, change its name too**SQL DW destination** in hello **Properties** pane.</span></span>
   
    ![][09]

## <a name="step-3-configure-hello-source-adapter"></a><span data-ttu-id="3ea0a-170">3. lépés: Hello forrás adapter konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-170">Step 3: Configure hello source adapter</span></span>
1. <span data-ttu-id="3ea0a-171">Kattintson duplán a hello forrás adapter tooopen hello **ADO.NET forrás szerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-171">Double-click hello source adapter tooopen hello **ADO.NET Source Editor**.</span></span>
   
    ![][03]
2. <span data-ttu-id="3ea0a-172">A hello **Csatlakozáskezelő** hello lapján **ADO.NET forrás szerkesztő**, hello kattintson **új** gomb következő toohello **ADO.NET Csatlakozáskezelő**lista tooopen hello **konfigurálása ADO.NET Csatlakozáskezelő** párbeszédpanel és hozza létre, amelyből ez az oktatóanyag adatokat tölt hello SQL Server adatbázis kapcsolati beállításait.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-172">On hello **Connection Manager** tab of hello **ADO.NET Source Editor**, click hello **New** button next toohello **ADO.NET connection manager** list tooopen hello **Configure ADO.NET Connection Manager** dialog box and create connection settings for hello SQL Server database from which this tutorial loads data.</span></span>
   
    ![][04]
3. <span data-ttu-id="3ea0a-173">A hello **konfigurálása ADO.NET Csatlakozáskezelő** párbeszédpanel hello kattintson **új** gomb tooopen hello **Csatlakozáskezelő** párbeszédpanel és hozza létre az új kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-173">In hello **Configure ADO.NET Connection Manager** dialog box, click hello **New** button tooopen hello **Connection Manager** dialog box and create a new data connection.</span></span>
   
    ![][05]
4. <span data-ttu-id="3ea0a-174">A hello **Csatlakozáskezelő** párbeszédpanel dolgok következő hello mezőbe.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-174">In hello **Connection Manager** dialog box, do hello following things.</span></span>
   
   1. <span data-ttu-id="3ea0a-175">A **szolgáltató**, válassza ki a hello SqlClient adatszolgáltatója.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-175">For **Provider**, select hello SqlClient Data Provider.</span></span>
   2. <span data-ttu-id="3ea0a-176">A **kiszolgálónév**, hello SQL Server nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-176">For **Server name**, enter hello SQL Server name.</span></span>
   3. <span data-ttu-id="3ea0a-177">A hello **toohello server bejelentkezés** szakaszban válassza ki vagy adja meg a hitelesítési adatokat.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-177">In hello **Log on toohello server** section, select or enter authentication information.</span></span>
   4. <span data-ttu-id="3ea0a-178">A hello **Connect tooa adatbázis** területen válassza ki az AdventureWorks mintaadatbázishoz hello.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-178">In hello **Connect tooa database** section, select hello AdventureWorks sample database.</span></span>
   5. <span data-ttu-id="3ea0a-179">Kattintson a **tesztkapcsolat**.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-179">Click **Test Connection**.</span></span>
      
       ![][06]
   6. <span data-ttu-id="3ea0a-180">A jelentések hello kapcsolat vizsgálati eredményeket hello hello párbeszédpanel, kattintson **OK** tooreturn toohello **Csatlakozáskezelő** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-180">In hello dialog box that reports hello results of hello connection test, click **OK** tooreturn toohello **Connection Manager** dialog box.</span></span>
   7. <span data-ttu-id="3ea0a-181">A hello **Csatlakozáskezelő** párbeszédpanel, kattintson a **OK** tooreturn toohello **konfigurálása ADO.NET Csatlakozáskezelő** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-181">In hello **Connection Manager** dialog box, click **OK** tooreturn toohello **Configure ADO.NET Connection Manager** dialog box.</span></span>
5. <span data-ttu-id="3ea0a-182">A hello **konfigurálása ADO.NET Csatlakozáskezelő** párbeszédpanel, kattintson a **OK** tooreturn toohello **ADO.NET forrás szerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-182">In hello **Configure ADO.NET Connection Manager** dialog box, click **OK** tooreturn toohello **ADO.NET Source Editor**.</span></span>
6. <span data-ttu-id="3ea0a-183">A hello **ADO.NET forrás szerkesztő**, a hello **hello tábla vagy nézet hello neve** listában, jelölje be hello **Sales.SalesOrderDetail** tábla.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-183">In hello **ADO.NET Source Editor**, in hello **Name of hello table or hello view** list, select hello **Sales.SalesOrderDetail** table.</span></span>
   
    ![][07]
7. <span data-ttu-id="3ea0a-184">Kattintson a **előzetes** toosee hello hello forrástáblában hello az első 200 adatsorokat **lekérdezés eredményének villámnézete** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-184">Click **Preview** toosee hello first 200 rows of data in hello source table in hello **Preview Query Results** dialog box.</span></span>
   
    ![][08]
8. <span data-ttu-id="3ea0a-185">A hello **lekérdezés eredményének villámnézete** párbeszédpanel, kattintson a **Bezárás** tooreturn toohello **ADO.NET forrás szerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-185">In hello **Preview Query Results** dialog box, click **Close** tooreturn toohello **ADO.NET Source Editor**.</span></span>
9. <span data-ttu-id="3ea0a-186">A hello **ADO.NET forrás szerkesztő**, kattintson a **OK** toofinish hello-adatforrás konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-186">In hello **ADO.NET Source Editor**, click **OK** toofinish configuring hello data source.</span></span>

## <a name="step-4-connect-hello-source-adapter-toohello-destination-adapter"></a><span data-ttu-id="3ea0a-187">4. lépés: Csatlakozás hello forrás adapter toohello céladapter</span><span class="sxs-lookup"><span data-stu-id="3ea0a-187">Step 4: Connect hello source adapter toohello destination adapter</span></span>
1. <span data-ttu-id="3ea0a-188">Válassza ki a forrás adapterének hello hello a tervezési felülethez.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-188">Select hello source adapter on hello design surface.</span></span>
2. <span data-ttu-id="3ea0a-189">Válassza ki, amely kiterjeszti hello forrás adapteréről hello kék nyíl, és húzza toohello cél szerkesztő amíg helyen igazodik.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-189">Select hello blue arrow that extends from hello source adapter and drag it toohello destination editor until it snaps into place.</span></span>
   
    ![][10]
   
    <span data-ttu-id="3ea0a-190">Egy tipikus SSIS-csomag más hello SSIS-eszközkészlet Between hello forrás és cél toorestructure hello, átalakító-összetevőt számos, és az adatok nagybetűhasználatának, ahogy keresztülhalad hello SSIS-adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-190">In a typical SSIS package, you use a number of other components from hello SSIS Toolbox in between hello source and hello destination toorestructure, transform, and cleanse your data as it passes through hello SSIS data flow.</span></span> <span data-ttu-id="3ea0a-191">tookeep ebben a példában lehető legegyszerűbb azt kapcsolat hello forrás közvetlenül toohello cél.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-191">tookeep this example as simple as possible, we’re connecting hello source directly toohello destination.</span></span>

## <a name="step-5-configure-hello-destination-adapter"></a><span data-ttu-id="3ea0a-192">5. lépés: Hello céladapter konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3ea0a-192">Step 5: Configure hello destination adapter</span></span>
1. <span data-ttu-id="3ea0a-193">Kattintson duplán a hello cél adapter tooopen hello **ADO.NET cél szerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-193">Double-click hello destination adapter tooopen hello **ADO.NET Destination Editor**.</span></span>
   
    ![][11]
2. <span data-ttu-id="3ea0a-194">A hello **Csatlakozáskezelő** hello lapján **ADO.NET cél szerkesztő**, hello kattintson **új** gomb következő toohello **Csatlakozáskezelő**lista tooopen hello **konfigurálása ADO.NET Csatlakozáskezelő** párbeszédpanel és hozza létre, amelybe ez az oktatóanyag adatokat tölt hello Azure SQL Data Warehouse-adatbázis biztonságoskapcsolat-beállításainak.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-194">On hello **Connection Manager** tab of hello **ADO.NET Destination Editor**, click hello **New** button next toohello **Connection manager** list tooopen hello **Configure ADO.NET Connection Manager** dialog box and create connection settings for hello Azure SQL Data Warehouse database into which this tutorial loads data.</span></span>
3. <span data-ttu-id="3ea0a-195">A hello **konfigurálása ADO.NET Csatlakozáskezelő** párbeszédpanel hello kattintson **új** gomb tooopen hello **Csatlakozáskezelő** párbeszédpanel és hozza létre az új kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-195">In hello **Configure ADO.NET Connection Manager** dialog box, click hello **New** button tooopen hello **Connection Manager** dialog box and create a new data connection.</span></span>
4. <span data-ttu-id="3ea0a-196">A hello **Csatlakozáskezelő** párbeszédpanel dolgok következő hello mezőbe.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-196">In hello **Connection Manager** dialog box, do hello following things.</span></span>
   1. <span data-ttu-id="3ea0a-197">A **szolgáltató**, válassza ki a hello SqlClient adatszolgáltatója.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-197">For **Provider**, select hello SqlClient Data Provider.</span></span>
   2. <span data-ttu-id="3ea0a-198">A **kiszolgálónév**, adja meg a hello SQL Data Warehouse neve.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-198">For **Server name**, enter hello SQL Data Warehouse name.</span></span>
   3. <span data-ttu-id="3ea0a-199">A hello **toohello server bejelentkezés** szakaszban jelölje be **használja az SQL Server-hitelesítési** , és írja be a hitelesítési adatokat.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-199">In hello **Log on toohello server** section, select **Use SQL Server authentication** and enter authentication information.</span></span>
   4. <span data-ttu-id="3ea0a-200">A hello **Connect tooa adatbázis** területen válasszon ki egy létező SQL Data Warehouse-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-200">In hello **Connect tooa database** section, select an existing SQL Data Warehouse database.</span></span>
   5. <span data-ttu-id="3ea0a-201">Kattintson a **tesztkapcsolat**.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-201">Click **Test Connection**.</span></span>
   6. <span data-ttu-id="3ea0a-202">A jelentések hello kapcsolat vizsgálati eredményeket hello hello párbeszédpanel, kattintson **OK** tooreturn toohello **Csatlakozáskezelő** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-202">In hello dialog box that reports hello results of hello connection test, click **OK** tooreturn toohello **Connection Manager** dialog box.</span></span>
   7. <span data-ttu-id="3ea0a-203">A hello **Csatlakozáskezelő** párbeszédpanel, kattintson a **OK** tooreturn toohello **konfigurálása ADO.NET Csatlakozáskezelő** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-203">In hello **Connection Manager** dialog box, click **OK** tooreturn toohello **Configure ADO.NET Connection Manager** dialog box.</span></span>
5. <span data-ttu-id="3ea0a-204">A hello **konfigurálása ADO.NET Csatlakozáskezelő** párbeszédpanel, kattintson a **OK** tooreturn toohello **ADO.NET cél szerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-204">In hello **Configure ADO.NET Connection Manager** dialog box, click **OK** tooreturn toohello **ADO.NET Destination Editor**.</span></span>
6. <span data-ttu-id="3ea0a-205">A hello **ADO.NET cél szerkesztő**, kattintson a **új** következő toohello **használni kívánt táblát vagy nézetet** lista tooopen hello **Create Table** párbeszédpanel toocreate utasítást egy oszloplistával hello forrástábla megfelelő új táblát.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-205">In hello **ADO.NET Destination Editor**, click **New** next toohello **Use a table or view** list tooopen hello **Create Table** dialog box toocreate a new destination table with a column list that matches hello source table.</span></span>
   
    ![][12a]
7. <span data-ttu-id="3ea0a-206">A hello **Create Table** párbeszédpanel dolgok következő hello mezőbe.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-206">In hello **Create Table** dialog box, do hello following things.</span></span>
   
   1. <span data-ttu-id="3ea0a-207">Hello céltábla hello nevének módosítása túl**értékesítési rendelési adatot**.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-207">Change hello name of hello destination table too**SalesOrderDetail**.</span></span>
   2. <span data-ttu-id="3ea0a-208">Távolítsa el a hello **rowguid** oszlop.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-208">Remove hello **rowguid** column.</span></span> <span data-ttu-id="3ea0a-209">Hello **uniqueidentifier** adattípusa nem támogatott az SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-209">hello **uniqueidentifier** data type is not supported in SQL Data Warehouse.</span></span>
   3. <span data-ttu-id="3ea0a-210">Hello hello adattípusának módosítása **LineTotal** oszlop túl**pénz**.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-210">Change hello data type of hello **LineTotal** column too**money**.</span></span> <span data-ttu-id="3ea0a-211">Hello **decimális** adattípusa nem támogatott az SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-211">hello **decimal** data type is not supported in SQL Data Warehouse.</span></span> <span data-ttu-id="3ea0a-212">További információt a támogatott adattípusok, lásd: [CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)][CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)].</span><span class="sxs-lookup"><span data-stu-id="3ea0a-212">For info about supported data types, see [CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)][CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)].</span></span>
      
       ![][12b]
   4. <span data-ttu-id="3ea0a-213">Kattintson a **OK** toocreate hello tábla, és térjen vissza toohello **ADO.NET cél szerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-213">Click **OK** toocreate hello table and return toohello **ADO.NET Destination Editor**.</span></span>
8. <span data-ttu-id="3ea0a-214">A hello **ADO.NET cél szerkesztő**, jelölje be hello **hozzárendelések** toosee lapon hogyan hello forrás oszlopai leképezve toocolumns hello cél.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-214">In hello **ADO.NET Destination Editor**, select hello **Mappings** tab toosee how columns in hello source are mapped toocolumns in hello destination.</span></span>
   
    ![][13]
9. <span data-ttu-id="3ea0a-215">Kattintson a **OK** toofinish hello-adatforrás konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-215">Click **OK** toofinish configuring hello data source.</span></span>

## <a name="step-6-run-hello-package-tooload-hello-data"></a><span data-ttu-id="3ea0a-216">6. lépés: Hello tooload hello csomagadatok futtatása</span><span class="sxs-lookup"><span data-stu-id="3ea0a-216">Step 6: Run hello package tooload hello data</span></span>
<span data-ttu-id="3ea0a-217">Futtatási hello csomag hello kattintva **Start** gombra, vagy egy hello hello eszköztár **futtatása** hello beállítások **Debug** menü.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-217">Run hello package by clicking hello **Start** button on hello toolbar or by selecting one of hello **Run** options on hello **Debug** menu.</span></span>

<span data-ttu-id="3ea0a-218">Hello csomag toorun kezdődik, mivel látni sárga forgó kerekek tooindicate tevékenység, valamint a hello eddig feldolgozott sorok száma.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-218">As hello package begins toorun, you see yellow spinning wheels tooindicate activity as well as hello number of rows processed so far.</span></span>

![][14]

<span data-ttu-id="3ea0a-219">Hello csomag futása befejeződött, amikor, tekintse meg a zöld jelölését tooindicate sikeres, valamint hello adatok betöltődnek a hello forrás toohello cél sorok maximális számát.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-219">When hello package has finished running, you see green check marks tooindicate success as well as hello total number of rows of data loaded from hello source toohello destination.</span></span>

![][15]

<span data-ttu-id="3ea0a-220">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="3ea0a-220">Congratulations!</span></span> <span data-ttu-id="3ea0a-221">Az Azure SQL Data Warehouse sikeresen használt SQL Server Integration Services tooload adatokat.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-221">You’ve successfully used SQL Server Integration Services tooload data into Azure SQL Data Warehouse.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3ea0a-222">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3ea0a-222">Next steps</span></span>
* <span data-ttu-id="3ea0a-223">További tudnivalók hello SSIS-adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-223">Learn more about hello SSIS data flow.</span></span> <span data-ttu-id="3ea0a-224">Kezdje itt: [adatfolyam][Data Flow].</span><span class="sxs-lookup"><span data-stu-id="3ea0a-224">Start here: [Data Flow][Data Flow].</span></span>
* <span data-ttu-id="3ea0a-225">Megtudhatja, hogyan toodebug és hibaelhárítása a csomagok jobb hello tervezési környezetben.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-225">Learn how toodebug and troubleshoot your packages right in hello design environment.</span></span> <span data-ttu-id="3ea0a-226">Kezdje itt: [hibaelhárítási eszközök csomag fejlesztési][Troubleshooting Tools for Package Development].</span><span class="sxs-lookup"><span data-stu-id="3ea0a-226">Start here: [Troubleshooting Tools for Package Development][Troubleshooting Tools for Package Development].</span></span>
* <span data-ttu-id="3ea0a-227">Megtudhatja, hogyan toodeploy a SSIS projektek és csomagok tooIntegration Services-kiszolgáló vagy tooanother tárolási helyét.</span><span class="sxs-lookup"><span data-stu-id="3ea0a-227">Learn how toodeploy your SSIS projects and packages tooIntegration Services Server or tooanother storage location.</span></span> <span data-ttu-id="3ea0a-228">Kezdje itt: [telepítési a projekt és csomagok][Deployment of Projects and Packages].</span><span class="sxs-lookup"><span data-stu-id="3ea0a-228">Start here: [Deployment of Projects and Packages][Deployment of Projects and Packages].</span></span>

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
