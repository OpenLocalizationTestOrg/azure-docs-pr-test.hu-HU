---
title: "aaaUse Redgate tooload tooyour az Azure data adatraktár |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Redgate adatok Platform Studio adatraktározási forgatókönyvekben."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: 670aef98-31f7-4436-86c0-cc989a39fe7f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 6082390c07c8ffa73ebd8ab272ace00ba8bb1897
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-with-redgate-data-platform-studio"></a><span data-ttu-id="e7f9a-103">Adatok betöltése a Redgate Data Platform Studióval</span><span class="sxs-lookup"><span data-stu-id="e7f9a-103">Load data with Redgate Data Platform Studio</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e7f9a-104">Redgate</span><span class="sxs-lookup"><span data-stu-id="e7f9a-104">Redgate</span></span>](sql-data-warehouse-load-with-redgate.md)
> * [<span data-ttu-id="e7f9a-105">Data Factory</span><span class="sxs-lookup"><span data-stu-id="e7f9a-105">Data Factory</span></span>](sql-data-warehouse-get-started-load-with-azure-data-factory.md)
> * [<span data-ttu-id="e7f9a-106">PolyBase</span><span class="sxs-lookup"><span data-stu-id="e7f9a-106">PolyBase</span></span>](sql-data-warehouse-get-started-load-with-polybase.md)
> * [<span data-ttu-id="e7f9a-107">BCP</span><span class="sxs-lookup"><span data-stu-id="e7f9a-107">BCP</span></span>](sql-data-warehouse-load-with-bcp.md)
> 
> 

<span data-ttu-id="e7f9a-108">Az oktatóanyag bemutatja, hogyan toouse [Redgate tartozó adatok Platform Studio](http://www.red-gate.com/products/azure-development/data-platform-studio/) egy helyi SQL Server tooAzure SQL Data Warehouse toomove adatait (terjesztési pontok).</span><span class="sxs-lookup"><span data-stu-id="e7f9a-108">This tutorial shows you how toouse [Redgate's Data Platform Studio](http://www.red-gate.com/products/azure-development/data-platform-studio/) (DPS) toomove data from an on-premises SQL Server tooAzure SQL Data Warehouse.</span></span> <span data-ttu-id="e7f9a-109">Adatok Platform Studio hello legmegfelelőbb javításai vonatkozik, és leggyorsabban hello optimalizálást, ezért úgy tooget elindítani az SQL Data Warehouse szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-109">Data Platform Studio applies hello most appropriate compatibility fixes and optimizations, so it's hello quickest way tooget started with SQL Data Warehouse.</span></span>

> [!NOTE]
> <span data-ttu-id="e7f9a-110">A [Redgate](http://www.red-gate.com) már régóta a Microsoft partnere, és különböző SQL Server-eszközöket tesz elérhetővé.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-110">[Redgate](http://www.red-gate.com) is a long-time Microsoft partner that delivers various SQL Server tools.</span></span> <span data-ttu-id="e7f9a-111">A Data Platform Studio e funkciója ingyenesen elérhető kereskedelmi és nem kereskedelmi használatra.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-111">This feature in Data Platform Studio has been made available freely for both commercial and non-commercial use.</span></span>
> 
> 

## <a name="before-you-begin"></a><span data-ttu-id="e7f9a-112">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="e7f9a-112">Before you begin</span></span>
### <a name="create-or-identify-resources"></a><span data-ttu-id="e7f9a-113">Erőforrások létrehozása és azonosítása</span><span class="sxs-lookup"><span data-stu-id="e7f9a-113">Create or identify resources</span></span>
<span data-ttu-id="e7f9a-114">Az oktatóanyag elindítása előtt kell toohave:</span><span class="sxs-lookup"><span data-stu-id="e7f9a-114">Before starting this tutorial, you need toohave:</span></span>

* <span data-ttu-id="e7f9a-115">**helyszíni SQL Server-adatbázis**: hello tooimport tooSQL adatraktár kell egy helyi SQL Server kiszolgáló toocome kívánt adatokat (2008R2 verzió vagy újabb).</span><span class="sxs-lookup"><span data-stu-id="e7f9a-115">**on-premises SQL Server Database**: hello data you want tooimport tooSQL Data Warehouse needs toocome from an on-premises SQL Server (version 2008R2 or above).</span></span> <span data-ttu-id="e7f9a-116">A Data Platform Studio nem tud közvetlenül adatokat importálni egy Azure SQL Database-ből vagy szövegfájlokból.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-116">Data Platform Studio cannot import data directly from an Azure SQL Database or from text files.</span></span>
* <span data-ttu-id="e7f9a-117">**Az Azure Storage-fiók**: adatok Platform Studio hello adatokat az Azure Blob Storage előkészíti azt az SQL Data Warehouse betöltése előtt.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-117">**Azure Storage Account**: Data Platform Studio stages hello data in Azure Blob Storage before loading it into SQL Data Warehouse.</span></span> <span data-ttu-id="e7f9a-118">hello tárfiók helyett hello "Klasszikus" üzembe helyezési modellel hello "erőforrás-kezelő" üzembe helyezési modellben (hello alapértelmezett) kell használnia.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-118">hello storage account must be using hello “Resource Manager” deployment model (hello default) rather than hello “Classic” deployment model.</span></span> <span data-ttu-id="e7f9a-119">Ha a storage-fiók nem rendelkezik, megtudhatja, hogyan tooCreate egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-119">If you don't have a storage account, learn how tooCreate a storage account.</span></span> 
* <span data-ttu-id="e7f9a-120">**Az SQL Data Warehouse**: Ez az oktatóanyag helyez hello adatokat a helyszíni SQL Server tooSQL adatraktár, így toohave kell egy online adatraktárral.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-120">**SQL Data Warehouse**: This tutorial moves hello data from on-premises SQL Server tooSQL Data Warehouse, so you need toohave a data warehouse online.</span></span> <span data-ttu-id="e7f9a-121">Ha még nem rendelkezik adatraktárral, megtudhatja, hogyan tooCreate Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-121">If you do not already have a data warehouse, learn how tooCreate an Azure SQL Data Warehouse.</span></span>

> [!NOTE]
> <span data-ttu-id="e7f9a-122">A teljesítmény akkor javul, ha hello tárfiókot és hello adatraktár jöttek létre hello azonos régióban.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-122">Performance is improved if hello storage account and hello data warehouse are created in hello same region.</span></span>
> 
> 

## <a name="step-1-sign-in-toodata-platform-studio-with-your-azure-account"></a><span data-ttu-id="e7f9a-123">1. lépés: Bejelentkezés tooData Platform Studio az Azure-fiókjába</span><span class="sxs-lookup"><span data-stu-id="e7f9a-123">Step 1: Sign in tooData Platform Studio with your Azure account</span></span>
<span data-ttu-id="e7f9a-124">Nyissa meg a webböngészőt, és keresse meg a toohello [adatok Platform Studio](https://www.dataplatformstudio.com/) webhelyet.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-124">Open your web browser and navigate toohello [Data Platform Studio](https://www.dataplatformstudio.com/) website.</span></span> <span data-ttu-id="e7f9a-125">Jelentkezzen be a hello azonos Azure-fiókot adott meg használt toocreate hello tárolási fiók és az adatraktárról.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-125">Sign in with hello same Azure account that you used toocreate hello storage account and data warehouse.</span></span> <span data-ttu-id="e7f9a-126">Ha az e-mail címét is munkahelyi vagy iskolai fiókjával és Microsoft-fiókkal társított, hogy meg arról, hogy toochoose hello fiókkal tooyour erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-126">If your email address is associated with both a work or school account and a Microsoft account, be sure toochoose hello account that has access tooyour resources.</span></span>

> [!NOTE]
> <span data-ttu-id="e7f9a-127">Ha az első alkalommal adatok Platform Studio segítségével, amelyek toogrant hello alkalmazás engedély toomanage kéri az Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-127">If this is your first time using Data Platform Studio, you are asked toogrant hello application permission toomanage your Azure resources.</span></span>
> 
> 

## <a name="step-2-start-hello-import-wizard"></a><span data-ttu-id="e7f9a-128">2. lépés: Hello importálása varázsló elindítása</span><span class="sxs-lookup"><span data-stu-id="e7f9a-128">Step 2: Start hello Import Wizard</span></span>
<span data-ttu-id="e7f9a-129">Hello terjesztési pontok fő képernyőn válassza ki hello tooAzure SQL Data Warehouse hivatkozás toostart hello importálás varázslóról.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-129">From hello DPS main screen, select hello Import tooAzure SQL Data Warehouse link toostart hello import wizard.</span></span>

![][1]

## <a name="step-3-install-hello-data-platform-studio-gateway"></a><span data-ttu-id="e7f9a-130">3. lépés: Hello adatátjáró Platform Studio telepítése</span><span class="sxs-lookup"><span data-stu-id="e7f9a-130">Step 3: Install hello Data Platform Studio Gateway</span></span>
<span data-ttu-id="e7f9a-131">tooconnect tooyour a helyszíni SQL Server-adatbázist, tooinstall hello terjesztési pontok átjáró szükséges.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-131">tooconnect tooyour on-premises SQL Server database, you need tooinstall hello DPS Gateway.</span></span> <span data-ttu-id="e7f9a-132">hello átjáró egy olyan ügyfélügynök, amelynek hozzáférési tooyour helyszíni környezetben, hello adatok kibontása és feltölti azt tooyour tárfiók.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-132">hello gateway is a client agent that provides access tooyour on-premises environment, extracts hello data, and uploads it tooyour storage account.</span></span> <span data-ttu-id="e7f9a-133">Az adatai soha nem haladnak keresztül a Redgate kiszolgálóin.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-133">Your data never passes through Redgate’s servers.</span></span> <span data-ttu-id="e7f9a-134">tooinstall hello átjáró:</span><span class="sxs-lookup"><span data-stu-id="e7f9a-134">tooinstall hello Gateway:</span></span>

1. <span data-ttu-id="e7f9a-135">Kattintson a hello **átjáró létrehozása** hivatkozás</span><span class="sxs-lookup"><span data-stu-id="e7f9a-135">Click hello **Create Gateway** link</span></span>
2. <span data-ttu-id="e7f9a-136">A letöltés és telepítés hello átjáró használatával hello megadott telepítő</span><span class="sxs-lookup"><span data-stu-id="e7f9a-136">Download and install hello Gateway using hello provided installer</span></span>

![][2]

> [!NOTE]
> <span data-ttu-id="e7f9a-137">Átjáró hello bármely gépen a hálózati hozzáférés toohello forrás SQL Server-adatbázis telepíthető.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-137">hello Gateway can be installed on any machine with network access toohello source SQL Server database.</span></span> <span data-ttu-id="e7f9a-138">Windows-hitelesítés hitelesítő adatokkal rendelkező hello hello aktuális felhasználó hello SQL Server-adatbázist használja.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-138">It accesses hello SQL Server database using Windows authentication with hello credentials of hello current user.</span></span>
> 
> 

<span data-ttu-id="e7f9a-139">A telepítést követően hello átjáró állapot módosítások tooConnected, és kiválaszthatja a Tovább gombra.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-139">Once installed, hello Gateway status changes tooConnected and you can select Next.</span></span>

## <a name="step-4-identify-hello-source-database"></a><span data-ttu-id="e7f9a-140">4. lépés: Hello forrásadatbázis azonosítása</span><span class="sxs-lookup"><span data-stu-id="e7f9a-140">Step 4: Identify hello source database</span></span>
<span data-ttu-id="e7f9a-141">A hello *kiszolgáló nevét adja meg* szövegmező, adja meg az adatbázist tároló hello kiszolgáló hello nevét, és válassza ki **következő**.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-141">In hello *Enter Server Name* textbox, enter hello name of hello server that hosts your database and select **Next**.</span></span> <span data-ttu-id="e7f9a-142">Ezt követően hello legördülő menüből, válassza ki a kívánt tooimport adatait hello adatbázist.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-142">Then, from hello drop-down menu, select hello database you want tooimport data from.</span></span>

![][3]

<span data-ttu-id="e7f9a-143">Terjesztési pontok megvizsgálja a kijelölt adatbázis hello táblák tooimport.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-143">DPS inspects hello selected database for tables tooimport.</span></span> <span data-ttu-id="e7f9a-144">Alapértelmezés szerint a terjesztési pontok hello adatbázis minden hello táblája importálja.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-144">By default, DPS imports all hello tables in hello database.</span></span> <span data-ttu-id="e7f9a-145">Válassza ki, vagy kapcsolja ki a táblák összes tábla hivatkozás hello kibontásával.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-145">You can select or deselect tables by expanding hello All Tables link.</span></span> <span data-ttu-id="e7f9a-146">Válassza ki a hello Tovább gomb toomove előre.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-146">Select hello Next button toomove forward.</span></span>

## <a name="step-5-choose-a-storage-account-toostage-hello-data"></a><span data-ttu-id="e7f9a-147">5. lépés: A tárolási fiók toostage hello adatok kiválasztásához</span><span class="sxs-lookup"><span data-stu-id="e7f9a-147">Step 5: Choose a storage account toostage hello data</span></span>
<span data-ttu-id="e7f9a-148">Terjesztési pontok egy helyen toostage hello adatokat kér.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-148">DPS prompts you for a location toostage hello data.</span></span> <span data-ttu-id="e7f9a-149">Válasszon ki egy meglévő tárfiókot az előfizetéséből, majd válassza a **Next** (Tovább) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-149">Choose an existing storage account from your subscription and select **Next**.</span></span>

> [!NOTE]
> <span data-ttu-id="e7f9a-150">Terjesztési pontok létrehoz egy új blob-tároló az kiválasztott tárfiók hello és minden importálásnak egy külön mappát használja.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-150">DPS will create a new blob container in hello chosen storage account and use a distinct folder for each import.</span></span>
> 
> 

![][4]

## <a name="step-6-select-a-data-warehouse"></a><span data-ttu-id="e7f9a-151">6. lépés: Adattárház kiválasztása</span><span class="sxs-lookup"><span data-stu-id="e7f9a-151">Step 6: Select a data warehouse</span></span>
<span data-ttu-id="e7f9a-152">Akkor válassza ki, az online [Azure SQL Data Warehouse](http://aka.ms/sqldw) adatbázis-tooimport hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-152">Next, you select an online [Azure SQL Data Warehouse](http://aka.ms/sqldw) database tooimport hello data into.</span></span> <span data-ttu-id="e7f9a-153">Miután az adatbázis kijelölt, tooenter hello hitelesítő adatok tooconnect toohello kell adatbázisról, és válassza ki **következő**.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-153">Once you've selected your database, you need tooenter hello credentials tooconnect toohello database and select **Next**.</span></span>

![][5]

> [!NOTE]
> <span data-ttu-id="e7f9a-154">Terjesztési pontok hello forrás adattáblák egyesít hello data warehouse-bA.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-154">DPS merges hello source data tables into hello data warehouse.</span></span> <span data-ttu-id="e7f9a-155">Terjesztési pontok figyelmezteti, ha hello táblanév írja elő hello adatraktár toooverwrite meglévő táblákat.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-155">DPS warns you if hello table name requires it toooverwrite existing tables in hello data warehouse.</span></span> <span data-ttu-id="e7f9a-156">Opcionálisan törölheti lévő hello adatraktár létező objektumok által Delete önkéntes importálás előtt minden meglévő objektumot.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-156">You may optionally delete any existing objects in hello data warehouse by ticking Delete all existing objects before import.</span></span>
> 
> 

## <a name="step-7-import-hello-data"></a><span data-ttu-id="e7f9a-157">7. lépés: Hello adatok importálása</span><span class="sxs-lookup"><span data-stu-id="e7f9a-157">Step 7: Import hello data</span></span>
<span data-ttu-id="e7f9a-158">Terjesztési pontok megerősíti, hogy szeretné-e tooimport hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-158">DPS confirms that you would like tooimport hello data.</span></span> <span data-ttu-id="e7f9a-159">Egyszerűen kattintson hello Start Importálás gomb toobegin hello adatok importálását.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-159">Simply click hello Start import button toobegin hello data import.</span></span>

![][6]

<span data-ttu-id="e7f9a-160">Terjesztési pontok kinyeréséhez, valamint a hello a helyszíni SQL Server és hello előrehaladását hello importálása az SQL Data Warehouse hello feltöltése hello előrehaladását mutatja a képi megjelenítés jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-160">DPS displays a visualization that shows hello progress of extracting and uploading hello data from hello on-premises SQL Server and hello progress of hello import into SQL Data Warehouse.</span></span>

![][7]

<span data-ttu-id="e7f9a-161">Hello importálás végrehajtása után az terjesztési pontok hello adatimportálás összefoglalását és hello javításai elvégzett módosítás jelentést jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="e7f9a-161">Once hello import is complete, DPS displays a summary of hello data import and a change report of hello compatibility fixes that have been performed.</span></span>

![][8]

## <a name="next-steps"></a><span data-ttu-id="e7f9a-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e7f9a-162">Next steps</span></span>
<span data-ttu-id="e7f9a-163">tooexplore SQL Data warehouse, az adatok megjelenítése elindításához:</span><span class="sxs-lookup"><span data-stu-id="e7f9a-163">tooexplore your data within SQL Data Warehouse, start by viewing:</span></span>

* <span data-ttu-id="e7f9a-164">[Az Azure SQL Data Warehouse lekérdezése (Visual Studio)][Query Azure SQL Data Warehouse (Visual Studio)]</span><span class="sxs-lookup"><span data-stu-id="e7f9a-164">[Query Azure SQL Data Warehouse (Visual Studio)][Query Azure SQL Data Warehouse (Visual Studio)]</span></span>
* <span data-ttu-id="e7f9a-165">[Adatok megjelenítése Power BI használatával][Visualize data with Power BI]</span><span class="sxs-lookup"><span data-stu-id="e7f9a-165">[Visualize data with Power BI][Visualize data with Power BI]</span></span>

<span data-ttu-id="e7f9a-166">További információk a Redgate tartozó adatok Platform Studio toolearn:</span><span class="sxs-lookup"><span data-stu-id="e7f9a-166">toolearn more about Redgate’s Data Platform Studio:</span></span>

* [<span data-ttu-id="e7f9a-167">A Microsoft hello terjesztési pontok kezdőlap</span><span class="sxs-lookup"><span data-stu-id="e7f9a-167">Visit hello DPS homepage</span></span>](http://www.dataplatformstudio.com/)
* [<span data-ttu-id="e7f9a-168">Egy bemutató megtekintése a DPS-ről a Channel9-on</span><span class="sxs-lookup"><span data-stu-id="e7f9a-168">Watch a demo of DPS on Channel9</span></span>](https://channel9.msdn.com/Blogs/cloud-with-a-silver-lining/Loading-data-into-Azure-SQL-Datawarehouse-with-Redgate-Data-Platform-Studio)

<span data-ttu-id="e7f9a-169">Más módokon toomigrate és a betöltés áttekintése: SQL-adatraktár adatai:</span><span class="sxs-lookup"><span data-stu-id="e7f9a-169">For an overview of other ways toomigrate and load your data in SQL Data Warehouse see:</span></span>

* <span data-ttu-id="e7f9a-170">[A megoldás tooSQL adatraktár áttelepítése][Migrate your solution tooSQL Data Warehouse]</span><span class="sxs-lookup"><span data-stu-id="e7f9a-170">[Migrate your solution tooSQL Data Warehouse][Migrate your solution tooSQL Data Warehouse]</span></span>
* [<span data-ttu-id="e7f9a-171">Adatok betöltése az Azure SQL Data Warehouse-ba</span><span class="sxs-lookup"><span data-stu-id="e7f9a-171">Load data into Azure SQL Data Warehouse</span></span>](sql-data-warehouse-overview-load.md)

<span data-ttu-id="e7f9a-172">További fejlesztési tippek, lásd: hello [SQL Data Warehouse fejlesztői áttekintés](sql-data-warehouse-overview-develop.md).</span><span class="sxs-lookup"><span data-stu-id="e7f9a-172">For more development tips, see hello [SQL Data Warehouse development overview](sql-data-warehouse-overview-develop.md).</span></span>

<!--Image references-->
[1]: media/sql-data-warehouse-redgate/2016-10-05_15-59-56.png
[2]: media/sql-data-warehouse-redgate/2016-10-05_11-16-07.png
[3]: media/sql-data-warehouse-redgate/2016-10-05_11-17-46.png
[4]: media/sql-data-warehouse-redgate/2016-10-05_11-20-41.png
[5]: media/sql-data-warehouse-redgate/2016-10-05_11-31-24.png
[6]: media/sql-data-warehouse-redgate/2016-10-05_11-32-20.png
[7]: media/sql-data-warehouse-redgate/2016-10-05_11-49-53.png
[8]: media/sql-data-warehouse-redgate/2016-10-05_12-57-10.png

<!--Article references-->
[Query Azure SQL Data Warehouse (Visual Studio)]: ./sql-data-warehouse-query-visual-studio.md
[Visualize data with Power BI]: ./sql-data-warehouse-get-started-visualize-with-power-bi.md
[Migrate your solution tooSQL Data Warehouse]: ./sql-data-warehouse-overview-migrate.md
[Load data into Azure SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
