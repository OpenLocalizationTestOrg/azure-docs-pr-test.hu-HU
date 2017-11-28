---
title: "Adatok betöltése az Azure SQL Data Warehouse – adat-előállító |} Microsoft Docs"
description: "Ez az oktatóanyag az Azure SQL Data Warehouse-adatokat tölt Azure Data Factory használatával, és egy SQL Server-adatbázist használja, mint az adatforrás."
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
ms.openlocfilehash: 12a35213e07ff16bdc1c27be106792bcc032ac80
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="load-data-into-sql-data-warehouse-with-data-factory"></a><span data-ttu-id="b4bc0-103">Adatok betöltése az SQL Data Warehouse Data Factory</span><span class="sxs-lookup"><span data-stu-id="b4bc0-103">Load data into SQL Data Warehouse with Data Factory</span></span>

<span data-ttu-id="b4bc0-104">Adatok betöltése az Azure SQL Data Warehouse bármely Azure Data Factory is használhatja a [támogatott forrás adattárolókhoz](../data-factory/data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="b4bc0-104">You can use Azure Data Factory to load data into Azure SQL Data Warehouse from any of the [supported source data stores](../data-factory/data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="b4bc0-105">Például akkor lehet adatok betöltése az Azure SQL-adatbázis vagy Oracle-adatbázishoz az SQL data warehouse Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-105">For example, you can load data from an Azure SQL database or an Oracle database into a SQL data warehouse by using Data Factory.</span></span> <span data-ttu-id="b4bc0-106">Ez a cikk az oktatóanyag bemutatja, hogyan lehet adatokat betölteni a helyszíni SQL Server-adatbázist az SQL data warehouse.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-106">Tutorial in this article shows you how to load data from an on-premises SQL Server database into a SQL data warehouse.</span></span>

<span data-ttu-id="b4bc0-107">**Becsült idő**: Ez az oktatóanyag befejezéséhez körülbelül 10 – 15 perc után az Előfeltételek teljesülését.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-107">**Time estimate**: This tutorial takes about 10-15 minutes to complete once the prerequisites are met.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b4bc0-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b4bc0-108">Prerequisites</span></span>

- <span data-ttu-id="b4bc0-109">Kell egy **SQL Server-adatbázis** táblákkal, amelyek tartalmazzák az SQL data warehouse keresztül másolja az adatokat.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-109">You need a **SQL Server database** with tables that contain the data to be copied over to the SQL data warehouse.</span></span>  

- <span data-ttu-id="b4bc0-110">Az online kell **SQL Data Warehouse**.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-110">You need an online **SQL Data Warehouse**.</span></span> <span data-ttu-id="b4bc0-111">Ha még nem rendelkezik adatraktárral, megtudhatja, hogyan [hozzon létre egy Azure SQL Data Warehouse](sql-data-warehouse-get-started-provision.md).</span><span class="sxs-lookup"><span data-stu-id="b4bc0-111">If you do not already have a data warehouse, learn how to [Create an Azure SQL Data Warehouse](sql-data-warehouse-get-started-provision.md).</span></span>

- <span data-ttu-id="b4bc0-112">Kell egy **Azure Storage-fiók**.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-112">You need an **Azure Storage Account**.</span></span> <span data-ttu-id="b4bc0-113">Ha még nem rendelkezik egy tárfiókot, megtudhatja, hogyan [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="b4bc0-113">If you do not already have a storage account, learn how to [Create a storage account](../storage/common/storage-create-storage-account.md).</span></span> <span data-ttu-id="b4bc0-114">A legjobb teljesítmény érdekében keresse meg a tárfiók és az adatraktár azonos Azure-régiót.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-114">For best performance, locate the storage account and the data warehouse in the same Azure region.</span></span>

## <a name="configure-a-data-factory"></a><span data-ttu-id="b4bc0-115">Egy adat-előállító konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b4bc0-115">Configure a data factory</span></span>
1. <span data-ttu-id="b4bc0-116">Jelentkezzen be az [Azure portálra][].</span><span class="sxs-lookup"><span data-stu-id="b4bc0-116">Log in to the [Azure portal][].</span></span>
2. <span data-ttu-id="b4bc0-117">Keresse meg az adatraktár, majd kattintson a megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-117">Locate your data warehouse and click to open it.</span></span>
3. <span data-ttu-id="b4bc0-118">A fő paneljén kattintson **adatok betöltése** > **Azure Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-118">In the main blade, click **Load Data** > **Azure Data Factory**.</span></span>

    ![Adatok betöltése varázsló indítása](media/sql-data-warehouse-load-with-data-factory/launch-load-data-wizard.png)

4. <span data-ttu-id="b4bc0-120">Ha nem rendelkezik adat-előállítót az Azure-előfizetéssel, akkor tekintse meg a **új adat-előállító** párbeszédpanel a böngésző külön lapon.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-120">If you do not have a data factory in your Azure subscription, you see a **New Data Factory** dialog box in a separate tab of the browser.</span></span> <span data-ttu-id="b4bc0-121">A kért információkat, majd kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-121">Fill in the requested information, and click **Create**.</span></span> <span data-ttu-id="b4bc0-122">Az adat-előállító létrehozása után a **új adat-előállító** párbeszédpanel bezárul, és megjelenik a **válassza ki a Data Factory** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-122">After the data factory is created, the **New Data Factory** dialog box closes, and you see the **Select Data Factory** dialog box.</span></span>

    <span data-ttu-id="b4bc0-123">Ha már megtalálható az Azure-előfizetés egy vagy több adat-előállítók, megjelenik a **válasszon adat-előállító** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-123">If you have one or more data factories already in the Azure subscription, you see the **Select Data Factory** dialog box.</span></span> <span data-ttu-id="b4bc0-124">Az ezen a párbeszédpanelen válassza ki valamelyik adat-előállítót, vagy kattintson a **hozzon létre új adat-előállító** számára hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-124">In this dialog box, you can either select an existing data factory or click **Create new data factory** to create a new one.</span></span>

    ![Adat-előállító konfigurálása](media/sql-data-warehouse-load-with-data-factory/configure-data-factory.png)

5. <span data-ttu-id="b4bc0-126">Az a **válasszon adat-előállító** párbeszédpanelen a **adatok betöltése** beállítás alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-126">In the **Select Data Factory** dialog box, the **Load data** option is selected by default.</span></span> <span data-ttu-id="b4bc0-127">Kattintson a **tovább** a feladat betöltése adatok létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-127">Click **Next** to start creating a data loading task.</span></span>

## <a name="configure-the-data-factory-properties"></a><span data-ttu-id="b4bc0-128">A data factory tulajdonságainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b4bc0-128">Configure the data factory properties</span></span>
<span data-ttu-id="b4bc0-129">Most, hogy egy adat-előállító létrehozott, a következő lépésre az adatok betöltése az ütemezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-129">Now that you have created a data factory, the next step is to configure the data loading schedule.</span></span>

1. <span data-ttu-id="b4bc0-130">A **feladatnév**, adja meg **DWLoadData-fromSQLServer**.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-130">For **Task name**, enter **DWLoadData-fromSQLServer**.</span></span>
2. <span data-ttu-id="b4bc0-131">Használja az alapértelmezett **futtassa egyszer most** , majd **következő**.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-131">Use the default **Run once now** option, click **Next**.</span></span>

    ![Betöltési ütemezését úgy állítsa be](media/sql-data-warehouse-load-with-data-factory/configure-load-schedule.png)

## <a name="configure-the-source-data-store-and-gateway"></a><span data-ttu-id="b4bc0-133">A forrás-tárolót és az átjáró konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b4bc0-133">Configure the source data store and gateway</span></span>
<span data-ttu-id="b4bc0-134">Most pedig utasítani fogja a Data Factory a helyszíni SQL Server adatbázis-, amelyből el kívánja betölteni az adatokat.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-134">Now you tell Data Factory about the on-premises SQL Server database from which you want to load data.</span></span>

1. <span data-ttu-id="b4bc0-135">Válasszon **SQL Server** a támogatott forrásadatok katalógus tárolja, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-135">Choose **SQL Server** from the supported source data store catalog, and click **Next**.</span></span>

    ![Válassza ki az SQL Server-adatforrás](media/sql-data-warehouse-load-with-data-factory/choose-sql-server-source.png)

2. <span data-ttu-id="b4bc0-137">A **adja meg a helyszíni SQL Server-adatbázis** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-137">A **Specify the on-premises SQL Server database** dialog appears.</span></span> <span data-ttu-id="b4bc0-138">Az első **kapcsolatnév** mező rendszer automatikusan kitölti.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-138">The first  **Connection name** field is auto filled in.</span></span> <span data-ttu-id="b4bc0-139">A második mező nevét kéri a **átjáró**.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-139">The second field asks for the name of the **Gateway**.</span></span> <span data-ttu-id="b4bc0-140">Ha valamelyik adat-előállítót, amely már van egy átjáró használ, a legördülő listában használni az átjárót.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-140">If you are using an existing data factory that already has a gateway, you can reuse the gateway by selecting it from the drop-down list.</span></span> <span data-ttu-id="b4bc0-141">Kattintson a **átjáró létrehozása** hivatkozás létrehozásához az adatkezelési átjárót.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-141">Click the **Create Gateway** link to create a Data Management Gateway.</span></span>  

    > [!NOTE]
    > <span data-ttu-id="b4bc0-142">Ha az adatok tárolásához a helyszínen vagy egy Azure IaaS virtuális gépen, az adatkezelési átjáró szükséges.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-142">If the source data store is on-premises or in an Azure IaaS virtual machine, a Data Management Gateway is required.</span></span> <span data-ttu-id="b4bc0-143">Átjáró egy adat-előállító 1-1 kapcsolattal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-143">A gateway has a 1-1 relationship with a data factory.</span></span> <span data-ttu-id="b4bc0-144">Az adat-előállító nem használható, de több Adatbetöltési adat-előállító feladatok használható.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-144">It cannot be used from another data factory, but it can be used by multiple data loading tasks with in the same data factory.</span></span> <span data-ttu-id="b4bc0-145">Átjáró segítségével több adattárolókhoz csatlakozás Adatbetöltési feladatok futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-145">A gateway can be used to connect to multiple data stores when running data loading tasks.</span></span>
    >
    > <span data-ttu-id="b4bc0-146">Az átjáró kapcsolatos részletes információkért lásd: [az adatkezelési átjáró](../data-factory/data-factory-data-management-gateway.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-146">For detailed information about the gateway, see [Data Management Gateway](../data-factory/data-factory-data-management-gateway.md) article.</span></span>

3. <span data-ttu-id="b4bc0-147">A **átjáró létrehozása** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-147">A **Create Gateway** dialog box appears.</span></span> <span data-ttu-id="b4bc0-148">A nevét, adja meg a **GatewayForDWLoading**, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-148">For Name, enter **GatewayForDWLoading**, and click **Create**.</span></span>

4. <span data-ttu-id="b4bc0-149">A **átjáró konfigurálása** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-149">A **Configure Gateway** dialog box appears.</span></span> <span data-ttu-id="b4bc0-150">Kattintson a **indítsa el a számítógépen a gyorstelepítés** automatikus letöltése, telepítése, és az adatkezelési átjáró regisztrálása az aktuális számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-150">Click **Launch express setup on this computer** to automatically download, install, and register Data Management Gateway on your current machine.</span></span> <span data-ttu-id="b4bc0-151">A folyamat egy előugró ablak látható.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-151">The progress is shown in a pop-up window.</span></span> <span data-ttu-id="b4bc0-152">Ha a számítógép nem tud csatlakozni az adattároló, manuálisan is [töltse le és telepítse az átjáró](https://www.microsoft.com/download/details.aspx?id=39717) olyan gépen, amely csatlakozni tud-e az adatok tárolásához, és a kulcs segítségével regisztrálja.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-152">If the machine cannot connect to the data store, you can manually [download and install the gateway](https://www.microsoft.com/download/details.aspx?id=39717) on a machine that can connect to the data store, and then use the key to register.</span></span>
    > [!NOTE]
    > <span data-ttu-id="b4bc0-153">A gyorstelepítés natív módon a Microsoft Edge és az Internet Explorer működik.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-153">The express setup works natively with Microsoft Edge and Internet Explorer.</span></span> <span data-ttu-id="b4bc0-154">Ha Google Chrome használ, először telepítse a ClickOnce bővítmény Chrome webes áruházból.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-154">If you are using Google Chrome, first install the ClickOnce extension from Chrome web store.</span></span>

    ![Expressz telepítés elindítása](media/sql-data-warehouse-load-with-data-factory/launch-express-setup.png)

5. <span data-ttu-id="b4bc0-156">Várjon, amíg az átjáró-telepítés befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-156">Wait for the gateway setup to complete.</span></span> <span data-ttu-id="b4bc0-157">Miután az átjáró regisztrálása sikeres volt, és online állapotban, az előugró ablak bezárása, és az új átjáró átjáró mezőjében jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-157">Once the gateway is successfully registered and is online, the pop-up window closes and the new gateway appears in the gateway field.</span></span> <span data-ttu-id="b4bc0-158">Ezután a adja meg a többi kötelező mezők az alábbiak szerint, majd kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-158">Then fill in the rest required fields as follows, then click **Next**.</span></span>
    - <span data-ttu-id="b4bc0-159">**Kiszolgálónév**: a helyszíni SQL Server neve.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-159">**Server name**: Name of the on-premises SQL Server.</span></span>
    - <span data-ttu-id="b4bc0-160">**Az adatbázisnév**: SQL Server-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-160">**Database name**: SQL Server database.</span></span>
    - <span data-ttu-id="b4bc0-161">**Hitelesítőadat-titkosítás**: használja az alapértelmezett "által webböngésző".</span><span class="sxs-lookup"><span data-stu-id="b4bc0-161">**Credential encryption**: Use the default "By web browser".</span></span>
    - <span data-ttu-id="b4bc0-162">**Hitelesítés típusa**: válassza ki a hitelesítéshez használ.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-162">**Authentication type**: Choose the type of authentication you are using.</span></span>
    - <span data-ttu-id="b4bc0-163">**Felhasználónév** és **jelszó**: Adja meg a felhasználónevet és jelszót egy felhasználó, aki jogosult az adatok másolásához.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-163">**User name** and **password**: Enter the user name and password for a user who has permission to copy the data.</span></span>

    ![Expressz telepítés elindítása](media/sql-data-warehouse-load-with-data-factory/configure-sql-server.png)

6. <span data-ttu-id="b4bc0-165">A következő lépés, hogy válassza körültekintően a táblákat, amelynek be kell másolnia az adatokat.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-165">The next step is to choose the tables from which to copy the data.</span></span> <span data-ttu-id="b4bc0-166">Szűrheti a táblák kulcsszavak használatával.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-166">You can filter the tables by using keywords.</span></span> <span data-ttu-id="b4bc0-167">És az adatok és a tábla séma az alsó panelen megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-167">And you can preview the data and table schema in the bottom panel.</span></span> <span data-ttu-id="b4bc0-168">A kijelölés befejezése után kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-168">After you finish your selection, click **Next**.</span></span>

    ![Select tables (Táblák kiválasztása)](media/sql-data-warehouse-load-with-data-factory/select-tables.png)

## <a name="configure-the-destination-your-sql-data-warehouse"></a><span data-ttu-id="b4bc0-170">A cél, az SQL Data Warehouse konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b4bc0-170">Configure the destination, your SQL Data Warehouse</span></span>

<span data-ttu-id="b4bc0-171">Most pedig utasítani fogja a Data Factory cél információkkal kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-171">Now you tell Data Factory about the destination information.</span></span>

1. <span data-ttu-id="b4bc0-172">Az SQL Data Warehouse-kapcsolat adatainak automatikusan kitölti a rendszer.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-172">Your SQL Data Warehouse connection information is filled in automatically.</span></span> <span data-ttu-id="b4bc0-173">A felhasználó nevét adja meg a jelszót.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-173">Enter the password for the user name.</span></span> <span data-ttu-id="b4bc0-174">Kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-174">and click **Next**.</span></span>

    ![Cél konfigurálása](media/sql-data-warehouse-load-with-data-factory/configure-destination.png)

2. <span data-ttu-id="b4bc0-176">Egy intelligens táblaleképezés jelenik meg, hogy a maps forrás céltáblák táblanevek alapján.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-176">An intelligent table mapping appears that maps source to destination tables based on table names.</span></span> <span data-ttu-id="b4bc0-177">Ha a tábla nem létezik a cél, alapértelmezés szerint ADF létrehoz egy azonos nevű (Ez vonatkozik az SQL Server vagy az Azure SQL Database forrásaként).</span><span class="sxs-lookup"><span data-stu-id="b4bc0-177">If the table does not exist in the destination, by default ADF will create one with the same name (this applies to SQL Server or Azure SQL Database as source).</span></span> <span data-ttu-id="b4bc0-178">Választhatja azt is, egy meglévő táblára van leképezve.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-178">You can also choose to map to an existing table.</span></span> <span data-ttu-id="b4bc0-179">Tekintse át, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-179">Review and click **Next**.</span></span>

    ![Táblázatok hozzárendelése](media/sql-data-warehouse-load-with-data-factory/table-mapping.png)

3. <span data-ttu-id="b4bc0-181">Tekintse át a séma-hozzárendeléséhez, és keressen hiba vagy figyelmeztető üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-181">Review the schema mapping and look for error or warning messages.</span></span> <span data-ttu-id="b4bc0-182">Intelligens leképezési oszlopnév alapul.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-182">Intelligent mapping is based on column name.</span></span> <span data-ttu-id="b4bc0-183">Ha egy nem támogatott típus a forrás és cél oszlop közötti átalakítás, lásd: hibaüzenetet a megfelelő tábla mellett.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-183">If there is an unsupported data type conversion between the source and destination column, you see an error message alongside the corresponding table.</span></span> <span data-ttu-id="b4bc0-184">Lehetővé teszik a Data Factory automatikusan a táblák létrehozása mellett dönt, ha megfelelő adattípus konvertálása olyan esetben fordulhat elő, ha a kompatibilitási kijavításához forrás és a célkiszolgáló között.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-184">If you choose to let Data Factory auto create the tables, proper data type conversion may happen if needed to fix the incompatibility between source and destination stores.</span></span>

    ![Térkép séma](media/sql-data-warehouse-load-with-data-factory/schema-mapping.png)

4. <span data-ttu-id="b4bc0-186">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-186">Click **Next**.</span></span>

## <a name="configure-the-performance-settings"></a><span data-ttu-id="b4bc0-187">A Teljesítménybeállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b4bc0-187">Configure the performance settings</span></span>
<span data-ttu-id="b4bc0-188">A teljesítmény-konfigurációk esetén az adatok átmeneti megelőzően a SQL Data Warehouse performantly használt Azure storage-fiók konfigurálja [PolyBase](sql-data-warehouse-best-practices.md#use-polybase-to-load-and-export-data-quickly).</span><span class="sxs-lookup"><span data-stu-id="b4bc0-188">In the Performance configurations, you configure an Azure storage account used for staging the data before it loads into SQL Data Warehouse performantly using [PolyBase](sql-data-warehouse-best-practices.md#use-polybase-to-load-and-export-data-quickly).</span></span> <span data-ttu-id="b4bc0-189">Miután végzett a másolat, a program automatikusan kiüríti az átmeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-189">After the copy is done, the interim data in storage will be cleaned up automatically.</span></span>

<span data-ttu-id="b4bc0-190">Jelöljön ki egy meglévő Azure Storage-fiók, majd a **következő**.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-190">Select an existing Azure Storage account, and click **Next**.</span></span>

![Átmeneti blob konfigurálása](media/sql-data-warehouse-load-with-data-factory/configure-staging-blob.png)

## <a name="review-summary-information-and-deploy-the-pipeline"></a><span data-ttu-id="b4bc0-192">Tekintse át az összefoglaló információkat, és újra az adatcsatornát</span><span class="sxs-lookup"><span data-stu-id="b4bc0-192">Review summary information and deploy the pipeline</span></span>

<span data-ttu-id="b4bc0-193">Ellenőrizze a konfigurációt, és kattintson a **Befejezés** gombra kattintva az adatcsatornát.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-193">Review the configuration and click **Finish** button to deploy the pipeline.</span></span>

![Adat-előállító telepítése](media/sql-data-warehouse-load-with-data-factory/deploy-data-factory.png)

## <a name="monitor-data-loading-progress"></a><span data-ttu-id="b4bc0-195">A figyelő adatok betöltése folyamatban</span><span class="sxs-lookup"><span data-stu-id="b4bc0-195">Monitor data loading progress</span></span>

<span data-ttu-id="b4bc0-196">Megtekintheti a központi telepítésének állapotáról és az eredmények a **telepítési** lap.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-196">You can see the deployment progress and results in the **Deployment** page.</span></span>

1. <span data-ttu-id="b4bc0-197">Az üzembe helyezés, ha a hivatkozásra, amely szerint **kattintson ide a figyelő másolási folyamat** betöltése folyamatban lévő adatok figyelésére.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-197">Once the deployment is done, click the link that says **Click here to monitor copy pipeline** to monitor data loading progress.</span></span>

    ![Folyamat figyelése](media/sql-data-warehouse-load-with-data-factory/monitor-pipeline.png)

2. <span data-ttu-id="b4bc0-199">Az újonnan létrehozott **DWLoadData-fromSQLServer** adatok betöltését folyamat kiválasztva a bal oldali az automatikus **erőforrás-kezelő**.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-199">The newly created **DWLoadData-fromSQLServer** data loading pipeline is auto selected from the left-hand **Resource Explorer**.</span></span>

    ![Nézet folyamat](media/sql-data-warehouse-load-with-data-factory/view-pipeline.png)

3. <span data-ttu-id="b4bc0-201">Kattintson a folyamat minden tábla, amely egy tevékenység van leképezve a részletes állapotát tekintheti meg a középső panelen be.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-201">Click into the pipeline in the middle panel to see the detailed status for each table that maps to an Activity.</span></span>

    ![Tábla tevékenység megtekintése](media/sql-data-warehouse-load-with-data-factory/view-table-activity.png)

4. <span data-ttu-id="b4bc0-203">További kattintson be egy tevékenységet, és az adatokat, a jobb oldali panelen, mint például a adatméret vagy sorok, átviteli részletek betöltésekor látja.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-203">Further click into an activity and you see the data loading details in the right panel including data size, rows, throughput, etc.</span></span>

    ![Tábla tevékenység részleteinek megtekintése](media/sql-data-warehouse-load-with-data-factory/view-table-activity-details.png)

5. <span data-ttu-id="b4bc0-205">A figyelési nézetek későbbi, nyissa meg az SQL Data warehouse elindításához kattintson **adatok betöltése > Azure Data Factory**, válassza ki a gyári, és válassza a **meglévő betöltése feladatok figyelése**.</span><span class="sxs-lookup"><span data-stu-id="b4bc0-205">To launch this monitoring view later, go to your SQL Data Warehouse, click **Load Data > Azure Data Factory**, select your factory, and choose **Monitor existing loading tasks**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b4bc0-206">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b4bc0-206">Next steps</span></span>

<span data-ttu-id="b4bc0-207">Telepítse át az adatbázist az SQL Data Warehouse, lásd: [áttelepítése – áttekintés](sql-data-warehouse-overview-migrate.md).</span><span class="sxs-lookup"><span data-stu-id="b4bc0-207">To migrate your database to SQL Data Warehouse, see [Migration overview](sql-data-warehouse-overview-migrate.md).</span></span>

<span data-ttu-id="b4bc0-208">Ha többet szeretne megtudni az Azure Data Factory és az adatok mozgása képességeit, tekintse meg a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="b4bc0-208">To learn more about Azure Data Factory and its data movement capabilities, see the following articles:</span></span>

- [<span data-ttu-id="b4bc0-209">Az Azure Data Factory bemutatása</span><span class="sxs-lookup"><span data-stu-id="b4bc0-209">Introduction to Azure Data Factory</span></span>](../data-factory/data-factory-introduction.md)
- [<span data-ttu-id="b4bc0-210">Adatok áthelyezése a másolási tevékenység használatával</span><span class="sxs-lookup"><span data-stu-id="b4bc0-210">Move data by using Copy Activity</span></span>](../data-factory/data-factory-data-movement-activities.md)
- [<span data-ttu-id="b4bc0-211">Adatok áthelyezése az Azure SQL Data Warehouse-ba és onnan máshová az Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="b4bc0-211">Move data to and from Azure SQL Data Warehouse using Azure Data Factory</span></span>](../data-factory/data-factory-azure-sql-data-warehouse-connector.md)

<span data-ttu-id="b4bc0-212">Az adatokba az SQL Data Warehouse, olvassa el a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="b4bc0-212">To explore your data in SQL Data Warehouse, see the following articles:</span></span>

- [<span data-ttu-id="b4bc0-213">Csatlakozás az SQL Data Warehouse a Visual Studio és az SSDT</span><span class="sxs-lookup"><span data-stu-id="b4bc0-213">Connect to SQL Data Warehouse with Visual Studio and SSDT</span></span>](sql-data-warehouse-query-visual-studio.md)
- <span data-ttu-id="b4bc0-214">[Visual adataiba a Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="b4bc0-214">[Visual data with Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md).</span></span>

<!-- Azure references -->
[Azure portálra]: https://portal.azure.com
