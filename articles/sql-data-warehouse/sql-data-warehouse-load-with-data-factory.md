---
title: "az Azure SQL Data Warehouse – adat-előállító aaaLoad adatok |} Microsoft Docs"
description: "Ez az oktatóanyag adatokat tölt az Azure SQL Data Warehouse Azure Data Factory használatával, és egy SQL Server-adatbázist használja, mint hello adatforrás."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse;azure-data-factory
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: loading
ms.date: 02/08/2017
ms.author: cakarst;barbkess
ms.openlocfilehash: 471871d3ee00ab34cc84bb63fbd13a323d14c2b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-into-sql-data-warehouse-with-data-factory"></a><span data-ttu-id="dea28-103">Adatok betöltése az SQL Data Warehouse Data Factory</span><span class="sxs-lookup"><span data-stu-id="dea28-103">Load data into SQL Data Warehouse with Data Factory</span></span>

<span data-ttu-id="dea28-104">Azure Data Factory tooload adatokat használhatja az Azure SQL Data Warehouse bármelyik hello [támogatott forrás adattárolókhoz](../data-factory/data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="dea28-104">You can use Azure Data Factory tooload data into Azure SQL Data Warehouse from any of hello [supported source data stores](../data-factory/data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="dea28-105">Például akkor lehet adatok betöltése az Azure SQL-adatbázis vagy Oracle-adatbázishoz az SQL data warehouse Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="dea28-105">For example, you can load data from an Azure SQL database or an Oracle database into a SQL data warehouse by using Data Factory.</span></span> <span data-ttu-id="dea28-106">Ez a cikk az oktatóanyag bemutatja, hogyan tooload adatait egy helyi SQL Server adatbázis-e az SQL data warehouse.</span><span class="sxs-lookup"><span data-stu-id="dea28-106">Tutorial in this article shows you how tooload data from an on-premises SQL Server database into a SQL data warehouse.</span></span>

<span data-ttu-id="dea28-107">**Becsült idő**: Ez az oktatóanyag kapcsolatos toocomplete 10 – 15 percet vesz igénybe, hello előfeltételek teljesülése után.</span><span class="sxs-lookup"><span data-stu-id="dea28-107">**Time estimate**: This tutorial takes about 10-15 minutes toocomplete once hello prerequisites are met.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dea28-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="dea28-108">Prerequisites</span></span>

- <span data-ttu-id="dea28-109">Kell egy **SQL Server-adatbázis** hello adatokat tartalmazó táblák toobe felülírt toohello SQL data warehouse-bA.</span><span class="sxs-lookup"><span data-stu-id="dea28-109">You need a **SQL Server database** with tables that contain hello data toobe copied over toohello SQL data warehouse.</span></span>  

- <span data-ttu-id="dea28-110">Az online kell **SQL Data Warehouse**.</span><span class="sxs-lookup"><span data-stu-id="dea28-110">You need an online **SQL Data Warehouse**.</span></span> <span data-ttu-id="dea28-111">Ha még nem rendelkezik adatraktárral, megtudhatja, hogyan túl[hozzon létre egy Azure SQL Data Warehouse](sql-data-warehouse-get-started-provision.md).</span><span class="sxs-lookup"><span data-stu-id="dea28-111">If you do not already have a data warehouse, learn how too[Create an Azure SQL Data Warehouse](sql-data-warehouse-get-started-provision.md).</span></span>

- <span data-ttu-id="dea28-112">Kell egy **Azure Storage-fiók**.</span><span class="sxs-lookup"><span data-stu-id="dea28-112">You need an **Azure Storage Account**.</span></span> <span data-ttu-id="dea28-113">Ha még nem rendelkezik egy tárfiókot, megtudhatja, hogyan túl[hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="dea28-113">If you do not already have a storage account, learn how too[Create a storage account](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="dea28-114">A legjobb teljesítmény érdekében keresse meg a tárfiók hello és hello az adatraktár-hello az azonos Azure-régiót.</span><span class="sxs-lookup"><span data-stu-id="dea28-114">For best performance, locate hello storage account and hello data warehouse in hello same Azure region.</span></span>

## <a name="configure-a-data-factory"></a><span data-ttu-id="dea28-115">Egy adat-előállító konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dea28-115">Configure a data factory</span></span>
1. <span data-ttu-id="dea28-116">Jelentkezzen be toohello [Azure-portálon][].</span><span class="sxs-lookup"><span data-stu-id="dea28-116">Log in toohello [Azure portal][].</span></span>
2. <span data-ttu-id="dea28-117">Keresse meg az adatraktár és tooopen kattintson azt.</span><span class="sxs-lookup"><span data-stu-id="dea28-117">Locate your data warehouse and click tooopen it.</span></span>
3. <span data-ttu-id="dea28-118">Hello fő paneljén kattintson **adatok betöltése** > **Azure Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="dea28-118">In hello main blade, click **Load Data** > **Azure Data Factory**.</span></span>

    ![Adatok betöltése varázsló indítása](media/sql-data-warehouse-load-with-data-factory/launch-load-data-wizard.png)

4. <span data-ttu-id="dea28-120">Ha nem rendelkezik adat-előállítót az Azure-előfizetéssel, akkor tekintse meg a **új adat-előállító** hello böngésző külön lapon párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="dea28-120">If you do not have a data factory in your Azure subscription, you see a **New Data Factory** dialog box in a separate tab of hello browser.</span></span> <span data-ttu-id="dea28-121">Adja meg a hello a kért információkat, majd kattintson az **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="dea28-121">Fill in hello requested information, and click **Create**.</span></span> <span data-ttu-id="dea28-122">Hello adat-előállító létrehozása után hello **új adat-előállító** párbeszédpanel bezárul, és meg hello **válasszon adat-előállító** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="dea28-122">After hello data factory is created, hello **New Data Factory** dialog box closes, and you see hello **Select Data Factory** dialog box.</span></span>

    <span data-ttu-id="dea28-123">Ha már a hello Azure-előfizetés egy vagy több adat-előállítók, megjelenik a hello **válasszon adat-előállító** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="dea28-123">If you have one or more data factories already in hello Azure subscription, you see hello **Select Data Factory** dialog box.</span></span> <span data-ttu-id="dea28-124">Az ezen a párbeszédpanelen válassza ki valamelyik adat-előállítót, vagy kattintson a **hozzon létre új adat-előállító** toocreate egy újat.</span><span class="sxs-lookup"><span data-stu-id="dea28-124">In this dialog box, you can either select an existing data factory or click **Create new data factory** toocreate a new one.</span></span>

    ![Adat-előállító konfigurálása](media/sql-data-warehouse-load-with-data-factory/configure-data-factory.png)

5. <span data-ttu-id="dea28-126">A hello **válasszon adat-előállító** párbeszédpanelen hello **adatok betöltése** beállítás alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="dea28-126">In hello **Select Data Factory** dialog box, hello **Load data** option is selected by default.</span></span> <span data-ttu-id="dea28-127">Kattintson a **következő** toostart a feladat betöltése adatok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="dea28-127">Click **Next** toostart creating a data loading task.</span></span>

## <a name="configure-hello-data-factory-properties"></a><span data-ttu-id="dea28-128">Hello data factory tulajdonságainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dea28-128">Configure hello data factory properties</span></span>
<span data-ttu-id="dea28-129">Most, hogy egy adat-előállító létrehozott hello következő lépésre tooconfigure hello Adatbetöltési ütemezés.</span><span class="sxs-lookup"><span data-stu-id="dea28-129">Now that you have created a data factory, hello next step is tooconfigure hello data loading schedule.</span></span>

1. <span data-ttu-id="dea28-130">A **feladatnév**, adja meg **DWLoadData-fromSQLServer**.</span><span class="sxs-lookup"><span data-stu-id="dea28-130">For **Task name**, enter **DWLoadData-fromSQLServer**.</span></span>
2. <span data-ttu-id="dea28-131">Hello alapértelmezett **futtassa egyszer most** , majd **következő**.</span><span class="sxs-lookup"><span data-stu-id="dea28-131">Use hello default **Run once now** option, click **Next**.</span></span>

    ![Betöltési ütemezését úgy állítsa be](media/sql-data-warehouse-load-with-data-factory/configure-load-schedule.png)

## <a name="configure-hello-source-data-store-and-gateway"></a><span data-ttu-id="dea28-133">Hello forrás adattár és átjáró konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dea28-133">Configure hello source data store and gateway</span></span>
<span data-ttu-id="dea28-134">Most pedig utasítani fogja a Data Factory hello a helyszíni SQL Server adatbázis-, amelyből el kívánja tooload adatokat.</span><span class="sxs-lookup"><span data-stu-id="dea28-134">Now you tell Data Factory about hello on-premises SQL Server database from which you want tooload data.</span></span>

1. <span data-ttu-id="dea28-135">Válasszon **SQL Server** hello támogatott forrásadatok katalógus tárolja, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="dea28-135">Choose **SQL Server** from hello supported source data store catalog, and click **Next**.</span></span>

    ![Válassza ki az SQL Server-adatforrás](media/sql-data-warehouse-load-with-data-factory/choose-sql-server-source.png)

2. <span data-ttu-id="dea28-137">A **megadása hello a helyszíni SQL Server-adatbázis** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="dea28-137">A **Specify hello on-premises SQL Server database** dialog appears.</span></span> <span data-ttu-id="dea28-138">először hello **kapcsolatnév** mező rendszer automatikusan kitölti.</span><span class="sxs-lookup"><span data-stu-id="dea28-138">hello first  **Connection name** field is auto filled in.</span></span> <span data-ttu-id="dea28-139">hello második mező kér hello hello neve **átjáró**.</span><span class="sxs-lookup"><span data-stu-id="dea28-139">hello second field asks for hello name of hello **Gateway**.</span></span> <span data-ttu-id="dea28-140">Ha használ, amely már rendelkezik az átjáró valamelyik adat-előállítót, hello legördülő listában használni hello átjáró.</span><span class="sxs-lookup"><span data-stu-id="dea28-140">If you are using an existing data factory that already has a gateway, you can reuse hello gateway by selecting it from hello drop-down list.</span></span> <span data-ttu-id="dea28-141">Kattintson a hello **átjáró létrehozása** toocreate adatkezelési átjárót hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="dea28-141">Click hello **Create Gateway** link toocreate a Data Management Gateway.</span></span>  

    > [!NOTE]
    > <span data-ttu-id="dea28-142">Ha hello forrásadatok tárolja a helyszínen vagy egy Azure IaaS virtuális gépen, az adatkezelési átjáró szükséges.</span><span class="sxs-lookup"><span data-stu-id="dea28-142">If hello source data store is on-premises or in an Azure IaaS virtual machine, a Data Management Gateway is required.</span></span> <span data-ttu-id="dea28-143">Átjáró egy adat-előállító 1-1 kapcsolattal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="dea28-143">A gateway has a 1-1 relationship with a data factory.</span></span> <span data-ttu-id="dea28-144">Az adat-előállító nem használható, de több Adatbetöltési hello a feladatok használható azonos adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="dea28-144">It cannot be used from another data factory, but it can be used by multiple data loading tasks with in hello same data factory.</span></span> <span data-ttu-id="dea28-145">Átjáró lehet használt tooconnect toomultiple adattárolókhoz Adatbetöltési feladatok futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="dea28-145">A gateway can be used tooconnect toomultiple data stores when running data loading tasks.</span></span>
    >
    > <span data-ttu-id="dea28-146">Hello átjáró kapcsolatos részletes információkért lásd: [az adatkezelési átjáró](../data-factory/data-factory-data-management-gateway.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="dea28-146">For detailed information about hello gateway, see [Data Management Gateway](../data-factory/data-factory-data-management-gateway.md) article.</span></span>

3. <span data-ttu-id="dea28-147">A **átjáró létrehozása** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="dea28-147">A **Create Gateway** dialog box appears.</span></span> <span data-ttu-id="dea28-148">A nevét, adja meg a **GatewayForDWLoading**, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="dea28-148">For Name, enter **GatewayForDWLoading**, and click **Create**.</span></span>

4. <span data-ttu-id="dea28-149">A **átjáró konfigurálása** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="dea28-149">A **Configure Gateway** dialog box appears.</span></span> <span data-ttu-id="dea28-150">Kattintson a **indítsa el a számítógépen a gyorstelepítés** tooautomatically letöltése, telepítése, és az adatkezelési átjáró regisztrálása az aktuális számítógépen.</span><span class="sxs-lookup"><span data-stu-id="dea28-150">Click **Launch express setup on this computer** tooautomatically download, install, and register Data Management Gateway on your current machine.</span></span> <span data-ttu-id="dea28-151">hello folyamatban van egy előugró ablak látható.</span><span class="sxs-lookup"><span data-stu-id="dea28-151">hello progress is shown in a pop-up window.</span></span> <span data-ttu-id="dea28-152">Ha hello gép toohello adattár nem tud csatlakozni, manuálisan is [töltse le és telepítse a hello átjáró](https://www.microsoft.com/download/details.aspx?id=39717) olyan gépen, amely képes kapcsolódni a toohello adatok tárolására, és hello kulcs tooregister használja.</span><span class="sxs-lookup"><span data-stu-id="dea28-152">If hello machine cannot connect toohello data store, you can manually [download and install hello gateway](https://www.microsoft.com/download/details.aspx?id=39717) on a machine that can connect toohello data store, and then use hello key tooregister.</span></span>
    > [!NOTE]
    > <span data-ttu-id="dea28-153">a gyorstelepítés hello natív módon a Microsoft Edge és az Internet Explorer működik.</span><span class="sxs-lookup"><span data-stu-id="dea28-153">hello express setup works natively with Microsoft Edge and Internet Explorer.</span></span> <span data-ttu-id="dea28-154">Ha Google Chrome használ, először telepítse hello ClickOnce bővítmény Chrome webes store-ból.</span><span class="sxs-lookup"><span data-stu-id="dea28-154">If you are using Google Chrome, first install hello ClickOnce extension from Chrome web store.</span></span>

    ![Expressz telepítés elindítása](media/sql-data-warehouse-load-with-data-factory/launch-express-setup.png)

5. <span data-ttu-id="dea28-156">Várjon, amíg hello átjáró telepítő toocomplete.</span><span class="sxs-lookup"><span data-stu-id="dea28-156">Wait for hello gateway setup toocomplete.</span></span> <span data-ttu-id="dea28-157">Miután hello átjáró regisztrálása sikeres volt, és online állapotban, hello előugró ablak bezárása és hello új átjáró hello átjáró mezőben jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="dea28-157">Once hello gateway is successfully registered and is online, hello pop-up window closes and hello new gateway appears in hello gateway field.</span></span> <span data-ttu-id="dea28-158">Ezután a fill hello többi kötelező mezők az alábbiak szerint, majd kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="dea28-158">Then fill in hello rest required fields as follows, then click **Next**.</span></span>
    - <span data-ttu-id="dea28-159">**Kiszolgálónév**: hello nevét a helyszíni SQL Server.</span><span class="sxs-lookup"><span data-stu-id="dea28-159">**Server name**: Name of hello on-premises SQL Server.</span></span>
    - <span data-ttu-id="dea28-160">**Az adatbázisnév**: SQL Server-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="dea28-160">**Database name**: SQL Server database.</span></span>
    - <span data-ttu-id="dea28-161">**Hitelesítőadat-titkosítás**: "által webböngésző" hello alapértelmezett használja.</span><span class="sxs-lookup"><span data-stu-id="dea28-161">**Credential encryption**: Use hello default "By web browser".</span></span>
    - <span data-ttu-id="dea28-162">**Hitelesítés típusa**: Adja meg hello típusú hitelesítést használ.</span><span class="sxs-lookup"><span data-stu-id="dea28-162">**Authentication type**: Choose hello type of authentication you are using.</span></span>
    - <span data-ttu-id="dea28-163">**Felhasználónév** és **jelszó**: hello felhasználónév és jelszó megadása egy felhasználó, aki rendelkezik engedéllyel toocopy hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="dea28-163">**User name** and **password**: Enter hello user name and password for a user who has permission toocopy hello data.</span></span>

    ![Expressz telepítés elindítása](media/sql-data-warehouse-load-with-data-factory/configure-sql-server.png)

6. <span data-ttu-id="dea28-165">hello a következő lépésre toochoose hello táblák mely toocopy hello adatokból.</span><span class="sxs-lookup"><span data-stu-id="dea28-165">hello next step is toochoose hello tables from which toocopy hello data.</span></span> <span data-ttu-id="dea28-166">Hello táblák kulcsszavak használatával végezhet.</span><span class="sxs-lookup"><span data-stu-id="dea28-166">You can filter hello tables by using keywords.</span></span> <span data-ttu-id="dea28-167">És hello adatok és a tábla séma hello alsó panelen megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="dea28-167">And you can preview hello data and table schema in hello bottom panel.</span></span> <span data-ttu-id="dea28-168">A kijelölés befejezése után kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="dea28-168">After you finish your selection, click **Next**.</span></span>

    ![Select tables (Táblák kiválasztása)](media/sql-data-warehouse-load-with-data-factory/select-tables.png)

## <a name="configure-hello-destination-your-sql-data-warehouse"></a><span data-ttu-id="dea28-170">Hello cél, az SQL Data Warehouse konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dea28-170">Configure hello destination, your SQL Data Warehouse</span></span>

<span data-ttu-id="dea28-171">Most pedig utasítani fogja a Data Factory hello cél információkról.</span><span class="sxs-lookup"><span data-stu-id="dea28-171">Now you tell Data Factory about hello destination information.</span></span>

1. <span data-ttu-id="dea28-172">Az SQL Data Warehouse-kapcsolat adatainak automatikusan kitölti a rendszer.</span><span class="sxs-lookup"><span data-stu-id="dea28-172">Your SQL Data Warehouse connection information is filled in automatically.</span></span> <span data-ttu-id="dea28-173">Adjon meg felhasználónevet hello hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="dea28-173">Enter hello password for hello user name.</span></span> <span data-ttu-id="dea28-174">Kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="dea28-174">and click **Next**.</span></span>

    ![Cél konfigurálása](media/sql-data-warehouse-load-with-data-factory/configure-destination.png)

2. <span data-ttu-id="dea28-176">Egy intelligens táblaleképezés jelenik meg, amely leképezi a forrás toodestination táblák táblanevek alapján.</span><span class="sxs-lookup"><span data-stu-id="dea28-176">An intelligent table mapping appears that maps source toodestination tables based on table names.</span></span> <span data-ttu-id="dea28-177">Ha hello tábla nem létezik a célként megadott hello, alapértelmezés szerint ADF létrehoz egyet a hello ugyanazt a nevet (Ez vonatkozik a tooSQL Server vagy az Azure SQL Database forrásaként).</span><span class="sxs-lookup"><span data-stu-id="dea28-177">If hello table does not exist in hello destination, by default ADF will create one with hello same name (this applies tooSQL Server or Azure SQL Database as source).</span></span> <span data-ttu-id="dea28-178">Másik lehetőségként toomap tooan meglévő tábla.</span><span class="sxs-lookup"><span data-stu-id="dea28-178">You can also choose toomap tooan existing table.</span></span> <span data-ttu-id="dea28-179">Tekintse át, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="dea28-179">Review and click **Next**.</span></span>

    ![Táblázatok hozzárendelése](media/sql-data-warehouse-load-with-data-factory/table-mapping.png)

3. <span data-ttu-id="dea28-181">Tekintse át a hello séma-hozzárendeléséhez, és keressen hiba vagy figyelmeztető üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="dea28-181">Review hello schema mapping and look for error or warning messages.</span></span> <span data-ttu-id="dea28-182">Intelligens leképezési oszlopnév alapul.</span><span class="sxs-lookup"><span data-stu-id="dea28-182">Intelligent mapping is based on column name.</span></span> <span data-ttu-id="dea28-183">Egy nem támogatott típus hello forrás és cél oszlop közötti átalakítás esetén lásd: hello megfelelő tábla mellett hibaüzenetet.</span><span class="sxs-lookup"><span data-stu-id="dea28-183">If there is an unsupported data type conversion between hello source and destination column, you see an error message alongside hello corresponding table.</span></span> <span data-ttu-id="dea28-184">Ha úgy dönt, hogy a Data Factory automatikus toolet hello táblák létrehozása, megfelelő adattípus konvertálása toofix hello kompatibilitási forrás- és tárolók közötti szükség esetén fordulhat elő.</span><span class="sxs-lookup"><span data-stu-id="dea28-184">If you choose toolet Data Factory auto create hello tables, proper data type conversion may happen if needed toofix hello incompatibility between source and destination stores.</span></span>

    ![Térkép séma](media/sql-data-warehouse-load-with-data-factory/schema-mapping.png)

4. <span data-ttu-id="dea28-186">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="dea28-186">Click **Next**.</span></span>

## <a name="configure-hello-performance-settings"></a><span data-ttu-id="dea28-187">Hello Teljesítménybeállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dea28-187">Configure hello performance settings</span></span>
<span data-ttu-id="dea28-188">Hello teljesítmény konfigurációkban átmeneti hello adatokat, az SQL Data Warehouse performantly használatával megelőzően használt Azure storage-fiók konfigurálása [PolyBase](sql-data-warehouse-best-practices.md#use-polybase-to-load-and-export-data-quickly).</span><span class="sxs-lookup"><span data-stu-id="dea28-188">In hello Performance configurations, you configure an Azure storage account used for staging hello data before it loads into SQL Data Warehouse performantly using [PolyBase](sql-data-warehouse-best-practices.md#use-polybase-to-load-and-export-data-quickly).</span></span> <span data-ttu-id="dea28-189">Hello másolása után a program automatikusan kiüríti hello köztes adatok.</span><span class="sxs-lookup"><span data-stu-id="dea28-189">After hello copy is done, hello interim data in storage will be cleaned up automatically.</span></span>

<span data-ttu-id="dea28-190">Jelöljön ki egy meglévő Azure Storage-fiók, majd a **következő**.</span><span class="sxs-lookup"><span data-stu-id="dea28-190">Select an existing Azure Storage account, and click **Next**.</span></span>

![Átmeneti blob konfigurálása](media/sql-data-warehouse-load-with-data-factory/configure-staging-blob.png)

## <a name="review-summary-information-and-deploy-hello-pipeline"></a><span data-ttu-id="dea28-192">Tekintse át az összefoglaló információkat és hello adatcsatornát</span><span class="sxs-lookup"><span data-stu-id="dea28-192">Review summary information and deploy hello pipeline</span></span>

<span data-ttu-id="dea28-193">Olvassa el a hello konfigurációs és a **Befejezés** gomb toodeploy hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="dea28-193">Review hello configuration and click **Finish** button toodeploy hello pipeline.</span></span>

![Adat-előállító telepítése](media/sql-data-warehouse-load-with-data-factory/deploy-data-factory.png)

## <a name="monitor-data-loading-progress"></a><span data-ttu-id="dea28-195">A figyelő adatok betöltése folyamatban</span><span class="sxs-lookup"><span data-stu-id="dea28-195">Monitor data loading progress</span></span>

<span data-ttu-id="dea28-196">Hello központi telepítésének állapotáról és hello eredményez **telepítési** lap.</span><span class="sxs-lookup"><span data-stu-id="dea28-196">You can see hello deployment progress and results in hello **Deployment** page.</span></span>

1. <span data-ttu-id="dea28-197">Miután hello üzembe helyezés hivatkozásra hello, amely szerint **ide toomonitor másolási folyamat** toomonitor adatok betöltése folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="dea28-197">Once hello deployment is done, click hello link that says **Click here toomonitor copy pipeline** toomonitor data loading progress.</span></span>

    ![Folyamat figyelése](media/sql-data-warehouse-load-with-data-factory/monitor-pipeline.png)

2. <span data-ttu-id="dea28-199">az újonnan létrehozott hello **DWLoadData-fromSQLServer** adatok betöltését csővezeték kiválasztva a bal oldali hello automatikus **erőforrás-kezelő**.</span><span class="sxs-lookup"><span data-stu-id="dea28-199">hello newly created **DWLoadData-fromSQLServer** data loading pipeline is auto selected from hello left-hand **Resource Explorer**.</span></span>

    ![Nézet folyamat](media/sql-data-warehouse-load-with-data-factory/view-pipeline.png)

3. <span data-ttu-id="dea28-201">Kattintson bele hello folyamatba hello középső panel toosee hello részletes minden táblához, amely leképezhető tooan tevékenység állapota.</span><span class="sxs-lookup"><span data-stu-id="dea28-201">Click into hello pipeline in hello middle panel toosee hello detailed status for each table that maps tooan Activity.</span></span>

    ![Tábla tevékenység megtekintése](media/sql-data-warehouse-load-with-data-factory/view-table-activity.png)

4. <span data-ttu-id="dea28-203">További kattintson be egy tevékenységet, és adataira hello hello jobb oldali panelen, mint például a adatméret vagy sorok, átviteli részletek betöltésekor.</span><span class="sxs-lookup"><span data-stu-id="dea28-203">Further click into an activity and you see hello data loading details in hello right panel including data size, rows, throughput, etc.</span></span>

    ![Tábla tevékenység részleteinek megtekintése](media/sql-data-warehouse-load-with-data-factory/view-table-activity-details.png)

5. <span data-ttu-id="dea28-205">toolaunch a figyelési nézet újabb, lépjen tooyour SQL Data Warehouse, kattintson a **adatok betöltése > Azure Data Factory**, válassza ki a gyári, és válassza a **meglévő betöltése feladatok figyelése**.</span><span class="sxs-lookup"><span data-stu-id="dea28-205">toolaunch this monitoring view later, go tooyour SQL Data Warehouse, click **Load Data > Azure Data Factory**, select your factory, and choose **Monitor existing loading tasks**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dea28-206">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dea28-206">Next steps</span></span>

<span data-ttu-id="dea28-207">toomigrate az adatbázis tooSQL adatraktár, lásd: [áttelepítése – áttekintés](sql-data-warehouse-overview-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="dea28-207">toomigrate your database tooSQL Data Warehouse, see [Migration overview](sql-data-warehouse-overview-migrate.md).</span></span>

<span data-ttu-id="dea28-208">További információ az Azure Data Factory és az adatok mozgása platformképességei toolearn lásd: a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="dea28-208">toolearn more about Azure Data Factory and its data movement capabilities, see hello following articles:</span></span>

- [<span data-ttu-id="dea28-209">Data Factory bemutatása tooAzure</span><span class="sxs-lookup"><span data-stu-id="dea28-209">Introduction tooAzure Data Factory</span></span>](../data-factory/data-factory-introduction.md)
- [<span data-ttu-id="dea28-210">Adatok áthelyezése a másolási tevékenység használatával</span><span class="sxs-lookup"><span data-stu-id="dea28-210">Move data by using Copy Activity</span></span>](../data-factory/data-factory-data-movement-activities.md)
- [<span data-ttu-id="dea28-211">Adatok tooand áthelyezése az Azure SQL Data Warehouse Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="dea28-211">Move data tooand from Azure SQL Data Warehouse using Azure Data Factory</span></span>](../data-factory/data-factory-azure-sql-data-warehouse-connector.md)

<span data-ttu-id="dea28-212">tooexplore az adatokat az SQL Data Warehouse, lásd a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="dea28-212">tooexplore your data in SQL Data Warehouse, see hello following articles:</span></span>

- [<span data-ttu-id="dea28-213">Visual Studio és az SSDT tooSQL adatraktár csatlakozás</span><span class="sxs-lookup"><span data-stu-id="dea28-213">Connect tooSQL Data Warehouse with Visual Studio and SSDT</span></span>](sql-data-warehouse-query-visual-studio.md)
- <span data-ttu-id="dea28-214">[Visual adataiba a Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="dea28-214">[Visual data with Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md).</span></span>

<!-- Azure references -->
[Azure-portálon]: https://portal.azure.com
