---
title: "aaaSQL kiszolgálón tárolt eljárási tevékenység"
description: "Ismerje meg, hogyan használhatja a hello SQL Server tárolt eljárási tevékenység tooinvoke a Data Factory-folyamat az Azure SQL Database vagy az Azure SQL Data Warehouse tárolt eljárást."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1c46ed69-4049-44ec-9b46-e90e964a4a8e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: spelluru
ms.openlocfilehash: 9116f80eefc59d95e866b2ba1de2feb1bdc4b1d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sql-server-stored-procedure-activity"></a><span data-ttu-id="fdf79-103">SQL Server tárolt eljárási tevékenység</span><span class="sxs-lookup"><span data-stu-id="fdf79-103">SQL Server Stored Procedure Activity</span></span>
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="fdf79-104">Hive-tevékenység</span><span class="sxs-lookup"><span data-stu-id="fdf79-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="fdf79-105">A Pig-tevékenység</span><span class="sxs-lookup"><span data-stu-id="fdf79-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="fdf79-106">MapReduce művelethez</span><span class="sxs-lookup"><span data-stu-id="fdf79-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="fdf79-107">Hadoop Streamelési tevékenységben</span><span class="sxs-lookup"><span data-stu-id="fdf79-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="fdf79-108">A Spark-tevékenység</span><span class="sxs-lookup"><span data-stu-id="fdf79-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="fdf79-109">Machine Learning kötegelt végrehajtási tevékenység</span><span class="sxs-lookup"><span data-stu-id="fdf79-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="fdf79-110">Machine Learning Update-erőforrástevékenység</span><span class="sxs-lookup"><span data-stu-id="fdf79-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="fdf79-111">Tárolt eljárási tevékenység</span><span class="sxs-lookup"><span data-stu-id="fdf79-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="fdf79-112">Data Lake Analytics U-SQL-tevékenység</span><span class="sxs-lookup"><span data-stu-id="fdf79-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="fdf79-113">.NET egyéni tevékenység</span><span class="sxs-lookup"><span data-stu-id="fdf79-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

## <a name="overview"></a><span data-ttu-id="fdf79-114">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="fdf79-114">Overview</span></span>
<span data-ttu-id="fdf79-115">Adatok átalakítása tevékenységek használata egy adat-előállítóban [csővezeték](data-factory-create-pipelines.md) előrejelzéseket és elemzések tootransform és a folyamat nyers adatok.</span><span class="sxs-lookup"><span data-stu-id="fdf79-115">You use data transformation activities in a Data Factory [pipeline](data-factory-create-pipelines.md) tootransform and process raw data into predictions and insights.</span></span> <span data-ttu-id="fdf79-116">hello tárolt eljárási tevékenység egyike, amely támogatja a Data Factory hello átalakítása tevékenységeket.</span><span class="sxs-lookup"><span data-stu-id="fdf79-116">hello Stored Procedure Activity is one of hello transformation activities that Data Factory supports.</span></span> <span data-ttu-id="fdf79-117">Ez a cikk épít, hello [adatok átalakítása tevékenységek](data-factory-data-transformation-activities.md) cikk, amelynek során az adatok átalakítása és hello támogatott átalakítása tevékenységek adat-előállítóban általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="fdf79-117">This article builds on hello [data transformation activities](data-factory-data-transformation-activities.md) article, which presents a general overview of data transformation and hello supported transformation activities in Data Factory.</span></span>

<span data-ttu-id="fdf79-118">Hello tárolt eljárási tevékenység tooinvoke valamelyik hello adatokat a következő tárolt eljárás tárolja a vállalati vagy egy Azure virtuális gépen (VM) is használhatja:</span><span class="sxs-lookup"><span data-stu-id="fdf79-118">You can use hello Stored Procedure Activity tooinvoke a stored procedure in one of hello following data stores in your enterprise or on an Azure virtual machine (VM):</span></span> 

- <span data-ttu-id="fdf79-119">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="fdf79-119">Azure SQL Database</span></span>
- <span data-ttu-id="fdf79-120">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="fdf79-120">Azure SQL Data Warehouse</span></span>
- <span data-ttu-id="fdf79-121">SQL Server-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="fdf79-121">SQL Server Database.</span></span>  <span data-ttu-id="fdf79-122">Ha SQL Server használ, telepítse az adatkezelési átjáró hello azonos számítógépre, hogy a gazdagépek hello adatbázis, vagy egy külön számítógépen, amely rendelkezik hozzáférési toohello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="fdf79-122">If you are using SQL Server, install Data Management Gateway on hello same machine that hosts hello database or on a separate machine that has access toohello database.</span></span> <span data-ttu-id="fdf79-123">Az adatkezelési átjáró egy összetevő, amely összeköti az adatok a helyszínen vagy a Azure VM a cloud serviceshez adatforrásokat felügyelt és biztonságos módon.</span><span class="sxs-lookup"><span data-stu-id="fdf79-123">Data Management Gateway is a component that connects data sources on-premises/on Azure VM with cloud services in a secure and managed way.</span></span> <span data-ttu-id="fdf79-124">Lásd: [az adatkezelési átjáró](data-factory-data-management-gateway.md) cikkben alább.</span><span class="sxs-lookup"><span data-stu-id="fdf79-124">See [Data Management Gateway](data-factory-data-management-gateway.md) article for details.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fdf79-125">Ha az adatok másolása az Azure SQL Database vagy az SQL Server, konfigurálhatja a hello **SqlSink** a másolási tevékenység tooinvoke hello segítségével tárolt eljárás **sqlWriterStoredProcedureName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="fdf79-125">When copying data into Azure SQL Database or SQL Server, you can configure hello **SqlSink** in copy activity tooinvoke a stored procedure by using hello **sqlWriterStoredProcedureName** property.</span></span> <span data-ttu-id="fdf79-126">További információkért lásd: [meghívása tárolt eljárás a másolási tevékenység](data-factory-invoke-stored-procedure-from-copy-activity.md).</span><span class="sxs-lookup"><span data-stu-id="fdf79-126">For more information, see [Invoke stored procedure from copy activity](data-factory-invoke-stored-procedure-from-copy-activity.md).</span></span> <span data-ttu-id="fdf79-127">További hello tulajdonságra vonatkozó további információkért lásd a következő összekötő cikkek: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="fdf79-127">For details about hello property, see following connector articles: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties).</span></span> <span data-ttu-id="fdf79-128">Adatok másolása az Azure SQL Data Warehouse a másolási tevékenység során a tárolt eljárás meghívása nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="fdf79-128">Invoking a stored procedure while copying data into an Azure SQL Data Warehouse by using a copy activity is not supported.</span></span> <span data-ttu-id="fdf79-129">De az SQL Data Warehouse hello tárolt eljárás tevékenység tooinvoke tárolt eljárást használhatja.</span><span class="sxs-lookup"><span data-stu-id="fdf79-129">But, you can use hello stored procedure activity tooinvoke a stored procedure in a SQL Data Warehouse.</span></span> 
>  
> <span data-ttu-id="fdf79-130">Ha az adatok másolása az Azure SQL Database vagy az SQL Server vagy az Azure SQL Data Warehouse, konfigurálhatja a **SqlSource** a másolási tevékenység tooinvoke egy tárolt eljárás tooread adatok forrásadatbázisból hello hello segítségével  **sqlReaderStoredProcedureName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="fdf79-130">When copying data from Azure SQL Database or SQL Server or Azure SQL Data Warehouse, you can configure **SqlSource** in copy activity tooinvoke a stored procedure tooread data from hello source database by using hello **sqlReaderStoredProcedureName** property.</span></span> <span data-ttu-id="fdf79-131">További információkért lásd: a következő összekötő cikkek hello: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)</span><span class="sxs-lookup"><span data-stu-id="fdf79-131">For more information, see hello following connector articles: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)</span></span>          


<span data-ttu-id="fdf79-132">forgatókönyv használja a következő hello egy folyamat tooinvoke Azure SQL-adatbázisban tárolt eljárás a tárolt eljárási tevékenység hello.</span><span class="sxs-lookup"><span data-stu-id="fdf79-132">hello following walkthrough uses hello Stored Procedure Activity in a pipeline tooinvoke a stored procedure in an Azure SQL database.</span></span> 

## <a name="walkthrough"></a><span data-ttu-id="fdf79-133">Útmutatás</span><span class="sxs-lookup"><span data-stu-id="fdf79-133">Walkthrough</span></span>
### <a name="sample-table-and-stored-procedure"></a><span data-ttu-id="fdf79-134">A minta-tábla és tárolt eljárás</span><span class="sxs-lookup"><span data-stu-id="fdf79-134">Sample table and stored procedure</span></span>
1. <span data-ttu-id="fdf79-135">Hozza létre a következőket hello **tábla** az Azure SQL adatbázis SQL Server Management Studio vagy bármilyen más ismeri a Feladatkezelő segítségével.</span><span class="sxs-lookup"><span data-stu-id="fdf79-135">Create hello following **table** in your Azure SQL Database using SQL Server Management Studio or any other tool you are comfortable with.</span></span> <span data-ttu-id="fdf79-136">hello datetimestamp oszlop hello dátum és idő hello megfelelő azonosító létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="fdf79-136">hello datetimestamp column is hello date and time when hello corresponding ID is generated.</span></span>

    ```SQL
    CREATE TABLE dbo.sampletable
    (
        Id uniqueidentifier,
        datetimestamp nvarchar(127)
    )
    GO

    CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable(Id);
    GO
    ```
    <span data-ttu-id="fdf79-137">Azonosító egyedi hello azonosított pedig hello datetimestamp oszlop hello dátum és idő hello megfelelő azonosító létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="fdf79-137">Id is hello unique identified and hello datetimestamp column is hello date and time when hello corresponding ID is generated.</span></span>
    
    ![Mintaadatok](./media/data-factory-stored-proc-activity/sample-data.png)

    <span data-ttu-id="fdf79-139">Ez a példa Azure SQL-adatbázisban egy hello tárolt eljárás.</span><span class="sxs-lookup"><span data-stu-id="fdf79-139">In this sample, hello stored procedure is in an Azure SQL Database.</span></span> <span data-ttu-id="fdf79-140">Hello tárolt eljárás egy Azure SQL Data warehouse-bA és az SQL Server-adatbázis, hello megközelítést akkor hasonló.</span><span class="sxs-lookup"><span data-stu-id="fdf79-140">If hello stored procedure is in an Azure SQL Data Warehouse and SQL Server Database, hello approach is similar.</span></span> <span data-ttu-id="fdf79-141">SQL Server-adatbázis, telepítenie kell egy [az adatkezelési átjáró](data-factory-data-management-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="fdf79-141">For a SQL Server database, you must install a [Data Management Gateway](data-factory-data-management-gateway.md).</span></span>
2. <span data-ttu-id="fdf79-142">Hozza létre a következőket hello **tárolt eljárás** , amely szúr be adatokat toohello **sampletable**.</span><span class="sxs-lookup"><span data-stu-id="fdf79-142">Create hello following **stored procedure** that inserts data in toohello **sampletable**.</span></span>

    ```SQL
    CREATE PROCEDURE sp_sample @DateTime nvarchar(127)
    AS

    BEGIN
        INSERT INTO [sampletable]
        VALUES (newid(), @DateTime)
    END
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="fdf79-143">**Név** és **kis-és** hello a paraméter (ebben a példában szereplő DateTime) meg kell egyeznie hello csővezeték/tevékenység JSON-ban megadott paramétert.</span><span class="sxs-lookup"><span data-stu-id="fdf79-143">**Name** and **casing** of hello parameter (DateTime in this example) must match that of parameter specified in hello pipeline/activity JSON.</span></span> <span data-ttu-id="fdf79-144">A tárolt eljárás definition hello, ügyeljen arra, hogy  **@**  hello paraméter előtagjaként szolgál.</span><span class="sxs-lookup"><span data-stu-id="fdf79-144">In hello stored procedure definition, ensure that **@** is used as a prefix for hello parameter.</span></span>

### <a name="create-a-data-factory"></a><span data-ttu-id="fdf79-145">Data factory létrehozása</span><span class="sxs-lookup"><span data-stu-id="fdf79-145">Create a data factory</span></span>
1. <span data-ttu-id="fdf79-146">Jelentkezzen be túl[Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="fdf79-146">Log in too[Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="fdf79-147">Kattintson a **új** hello bal oldali menüben kattintson **Eszközintelligencia + analitika**, és kattintson a **adat-előállító**.</span><span class="sxs-lookup"><span data-stu-id="fdf79-147">Click **NEW** on hello left menu, click **Intelligence + Analytics**, and click **Data Factory**.</span></span>

    ![Új adat-előállító](media/data-factory-stored-proc-activity/new-data-factory.png)    
3. <span data-ttu-id="fdf79-149">A hello **új adat-előállító** panelen adja meg **SProcDF** a hello nevét.</span><span class="sxs-lookup"><span data-stu-id="fdf79-149">In hello **New data factory** blade, enter **SProcDF** for hello Name.</span></span> <span data-ttu-id="fdf79-150">Az Azure Data Factory neve **globálisan egyedi**.</span><span class="sxs-lookup"><span data-stu-id="fdf79-150">Azure Data Factory names are **globally unique**.</span></span> <span data-ttu-id="fdf79-151">A névvel, tooenable hello sikeres létrehozása hello gyári hello adat-előállító tooprefix hello neve van szüksége.</span><span class="sxs-lookup"><span data-stu-id="fdf79-151">You need tooprefix hello name of hello data factory with your name, tooenable hello successful creation of hello factory.</span></span>

   ![Új adat-előállító](media/data-factory-stored-proc-activity/new-data-factory-blade.png)         
4. <span data-ttu-id="fdf79-153">Válassza ki a **Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="fdf79-153">Select your **Azure subscription**.</span></span>
5. <span data-ttu-id="fdf79-154">A **erőforráscsoport**, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="fdf79-154">For **Resource Group**, do one of hello following steps:</span></span>
   1. <span data-ttu-id="fdf79-155">Kattintson a **hozzon létre új** , és írja be a hello erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="fdf79-155">Click **Create new** and enter a name for hello resource group.</span></span>
   2. <span data-ttu-id="fdf79-156">Kattintson a **meglévő** , és válasszon ki egy meglévő erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="fdf79-156">Click **Use existing** and select an existing resource group.</span></span>  
6. <span data-ttu-id="fdf79-157">Jelölje be hello **hely** hello adat-előállító esetében.</span><span class="sxs-lookup"><span data-stu-id="fdf79-157">Select hello **location** for hello data factory.</span></span>
7. <span data-ttu-id="fdf79-158">Válassza ki **PIN-kód toodashboard** , hogy láthatóvá hello adat-előállító hello irányítópult jelentkezik be a következő alkalommal.</span><span class="sxs-lookup"><span data-stu-id="fdf79-158">Select **Pin toodashboard** so that you can see hello data factory on hello dashboard next time you log in.</span></span>
8. <span data-ttu-id="fdf79-159">Kattintson a **létrehozása** a hello **új adat-előállító** panelen.</span><span class="sxs-lookup"><span data-stu-id="fdf79-159">Click **Create** on hello **New data factory** blade.</span></span>
9. <span data-ttu-id="fdf79-160">Hello adat-előállító létrehozása hello látja **irányítópult** a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="fdf79-160">You see hello data factory being created in hello **dashboard** of hello Azure portal.</span></span> <span data-ttu-id="fdf79-161">Miután hello adat-előállító létrehozása sikerült, oldal akkor jelenik meg hello adatok gyári, amely jelzi, hogy hello hello adat-előállító tartalmát.</span><span class="sxs-lookup"><span data-stu-id="fdf79-161">After hello data factory has been created successfully, you see hello data factory page, which shows you hello contents of hello data factory.</span></span>

   ![Data Factory kezdőlap](media/data-factory-stored-proc-activity/data-factory-home-page.png)

### <a name="create-an-azure-sql-linked-service"></a><span data-ttu-id="fdf79-163">Azure SQL társított szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="fdf79-163">Create an Azure SQL linked service</span></span>
<span data-ttu-id="fdf79-164">Miután létrehozta a hello adat-előállítót, hozzon létre, amely az Azure SQL adatbázis, amely tartalmazza a hello sampletable tábla és sp_sample tárolt eljárás, tooyour adat-előállító Azure SQL társított szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="fdf79-164">After creating hello data factory, you create an Azure SQL linked service that links your Azure SQL database, which contains hello sampletable table and sp_sample stored procedure, tooyour data factory.</span></span>

1. <span data-ttu-id="fdf79-165">Kattintson a **Szerző és központi telepítése** a hello **adat-előállító** paneljén **SProcDF** toolaunch hello Data Factory Editor.</span><span class="sxs-lookup"><span data-stu-id="fdf79-165">Click **Author and deploy** on hello **Data Factory** blade for **SProcDF** toolaunch hello Data Factory Editor.</span></span>
2. <span data-ttu-id="fdf79-166">Kattintson a **az új adattároló** a hello parancssávon, és válassza a **Azure SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="fdf79-166">Click **New data store** on hello command bar and choose **Azure SQL Database**.</span></span> <span data-ttu-id="fdf79-167">Meg kell jelennie a hello JSON-parancsfájl létrehozásához egy Azure SQL társított szolgáltatásnak hello-szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="fdf79-167">You should see hello JSON script for creating an Azure SQL linked service in hello editor.</span></span>

   ![Új adattár](media/data-factory-stored-proc-activity/new-data-store.png)
3. <span data-ttu-id="fdf79-169">Hello JSON-parancsfájl ellenőrizze a következő módosításokat hello:</span><span class="sxs-lookup"><span data-stu-id="fdf79-169">In hello JSON script, make hello following changes:</span></span>

   1. <span data-ttu-id="fdf79-170">Cserélje le `<servername>` hello nevet, az Azure SQL Database-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="fdf79-170">Replace `<servername>` with hello name of your Azure SQL Database server.</span></span>
   2. <span data-ttu-id="fdf79-171">Cserélje le `<databasename>` létrehozására használt tábla hello és hello hello adatbázissal tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="fdf79-171">Replace `<databasename>` with hello database in which you created hello table and hello stored procedure.</span></span>
   3. <span data-ttu-id="fdf79-172">Cserélje le `<username@servername>` hello felhasználói fiókkal, amely rendelkezik toohello adatbázist.</span><span class="sxs-lookup"><span data-stu-id="fdf79-172">Replace `<username@servername>` with hello user account that has access toohello database.</span></span>
   4. <span data-ttu-id="fdf79-173">Cserélje le `<password>` a hello hello felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="fdf79-173">Replace `<password>` with hello password for hello user account.</span></span>

      ![Új adattár](media/data-factory-stored-proc-activity/azure-sql-linked-service.png)
4. <span data-ttu-id="fdf79-175">toodeploy hello társított szolgáltatás, kattintson a **telepítés** hello parancssávon.</span><span class="sxs-lookup"><span data-stu-id="fdf79-175">toodeploy hello linked service, click **Deploy** on hello command bar.</span></span> <span data-ttu-id="fdf79-176">Győződjön meg arról, hogy megjelenik-e hello AzureSqlLinkedService hello fában hello bal oldali megtekintése.</span><span class="sxs-lookup"><span data-stu-id="fdf79-176">Confirm that you see hello AzureSqlLinkedService in hello tree view on hello left.</span></span>

    ![fanézet, a társított szolgáltatás](media/data-factory-stored-proc-activity/tree-view.png)

### <a name="create-an-output-dataset"></a><span data-ttu-id="fdf79-178">Kimeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="fdf79-178">Create an output dataset</span></span>
<span data-ttu-id="fdf79-179">Adjon meg egy kimeneti adatkészlet egy tárolt eljárás tevékenység még akkor is, ha hello tárolt eljárás nem hozhatók létre adatokat.</span><span class="sxs-lookup"><span data-stu-id="fdf79-179">You must specify an output dataset for a stored procedure activity even if hello stored procedure does not produce any data.</span></span> <span data-ttu-id="fdf79-180">Ennek oka az, az hello kimeneti adatkészlet, amely az hello ütemezés hello tevékenység (milyen gyakran hello tevékenység fut - óránként, naponta, stb.).</span><span class="sxs-lookup"><span data-stu-id="fdf79-180">That's because it's hello output dataset that drives hello schedule of hello activity (how often hello activity is run - hourly, daily, etc.).</span></span> <span data-ttu-id="fdf79-181">hello kimeneti adatkészlet kell használnia egy **társított szolgáltatás** , amely hivatkozik tooan Azure SQL Database vagy az Azure SQL Data Warehouse vagy a használni kívánt tárolt eljárás toorun hello SQL Server-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="fdf79-181">hello output dataset must use a **linked service** that refers tooan Azure SQL Database or an Azure SQL Data Warehouse or a SQL Server Database in which you want hello stored procedure toorun.</span></span> <span data-ttu-id="fdf79-182">hello kimeneti adatkészlet ki tud szolgálni hello tárolt eljárás későbbi feldolgozásra módon toopass hello eredményként egy másik tevékenység ([tevékenységek láncolás](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="fdf79-182">hello output dataset can serve as a way toopass hello result of hello stored procedure for subsequent processing by another activity ([chaining activities](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) in hello pipeline.</span></span> <span data-ttu-id="fdf79-183">Adat-előállító azonban nem a hello kimeneti a tárolt eljárás toothis DataSet adatkészlet automatikusan írnia.</span><span class="sxs-lookup"><span data-stu-id="fdf79-183">However, Data Factory does not automatically write hello output of a stored procedure toothis dataset.</span></span> <span data-ttu-id="fdf79-184">Hello tárolt eljárás írások tooa SQL táblát, amely hello kimeneti adatkészlet mutat.</span><span class="sxs-lookup"><span data-stu-id="fdf79-184">It is hello stored procedure that writes tooa SQL table that hello output dataset points to.</span></span> <span data-ttu-id="fdf79-185">Bizonyos esetekben hello kimeneti adatkészlet lehet egy **dummy dataset** (a DataSet adatkészlet mutat, tooa tábla hello kimenete valóban nem rendelkező tárolt eljárás).</span><span class="sxs-lookup"><span data-stu-id="fdf79-185">In some cases, hello output dataset can be a **dummy dataset** (a dataset that points tooa table that does not really hold output of hello stored procedure).</span></span> <span data-ttu-id="fdf79-186">Az üres adatkészlet csak hello futtatási toospecify hello ütemezésének tárolt eljárási tevékenység szolgál.</span><span class="sxs-lookup"><span data-stu-id="fdf79-186">This dummy dataset is used only toospecify hello schedule for running hello stored procedure activity.</span></span> 

1. <span data-ttu-id="fdf79-187">Ha nem látja ezt a gombot, kattintson a három pontot  **További** hello eszköztáron kattintson **új adatkészlet**, és kattintson a **Azure SQL**.</span><span class="sxs-lookup"><span data-stu-id="fdf79-187">Click **... More** on hello toolbar, click **New dataset**, and click **Azure SQL**.</span></span> <span data-ttu-id="fdf79-188">**Új adatkészlet** hello parancs menüsoron, majd válassza a **Azure SQL**.</span><span class="sxs-lookup"><span data-stu-id="fdf79-188">**New dataset** on hello command bar and select **Azure SQL**.</span></span>

    ![fanézet, a társított szolgáltatás](media/data-factory-stored-proc-activity/new-dataset.png)
2. <span data-ttu-id="fdf79-190">Másolja és illessze be a következő JSON-parancsfájlok JSON-szerkesztőben toohello hello.</span><span class="sxs-lookup"><span data-stu-id="fdf79-190">Copy/paste hello following JSON script in toohello JSON editor.</span></span>

    ```JSON
    {                
        "name": "sprocsampleout",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "sampletable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```
3. <span data-ttu-id="fdf79-191">toodeploy hello dataset, kattintson a **telepítés** hello parancssávon.</span><span class="sxs-lookup"><span data-stu-id="fdf79-191">toodeploy hello dataset, click **Deploy** on hello command bar.</span></span> <span data-ttu-id="fdf79-192">Győződjön meg arról, hogy megjelenik-e hello dataset hello faszerkezetes nézetben.</span><span class="sxs-lookup"><span data-stu-id="fdf79-192">Confirm that you see hello dataset in hello tree view.</span></span>

    ![Fanézet, a társított szolgáltatások](media/data-factory-stored-proc-activity/tree-view-2.png)

### <a name="create-a-pipeline-with-sqlserverstoredprocedure-activity"></a><span data-ttu-id="fdf79-194">Hozzon létre egy folyamatot SqlServerStoredProcedure tevékenység</span><span class="sxs-lookup"><span data-stu-id="fdf79-194">Create a pipeline with SqlServerStoredProcedure activity</span></span>
<span data-ttu-id="fdf79-195">Most hozzon létre egy folyamatot egy tárolt eljárás tevékenységet.</span><span class="sxs-lookup"><span data-stu-id="fdf79-195">Now, let's create a pipeline with a stored procedure activity.</span></span> 

<span data-ttu-id="fdf79-196">Figyelje meg a következő tulajdonságai hello:</span><span class="sxs-lookup"><span data-stu-id="fdf79-196">Notice hello following properties:</span></span> 

- <span data-ttu-id="fdf79-197">Hello **típus** tulajdonsága túl**SqlServerStoredProcedure**.</span><span class="sxs-lookup"><span data-stu-id="fdf79-197">hello **type** property is set too**SqlServerStoredProcedure**.</span></span> 
- <span data-ttu-id="fdf79-198">Hello **storedProcedureName** típus tulajdonságainak túl van beállítva**sp_sample** (hello neve tárolt eljárás).</span><span class="sxs-lookup"><span data-stu-id="fdf79-198">hello **storedProcedureName** in type properties is set too**sp_sample** (name of hello stored procedure).</span></span>
- <span data-ttu-id="fdf79-199">Hello **storedProcedureParameters** szakasz nevű egy paraméter **/időből**.</span><span class="sxs-lookup"><span data-stu-id="fdf79-199">hello **storedProcedureParameters** section contains one parameter named **DataTime**.</span></span> <span data-ttu-id="fdf79-200">Név és a kis-és a JSON-ban hello paraméter meg kell egyeznie hello nevét és a kis-és nagybetűk hello paraméter hello tárolt eljárás definícióban.</span><span class="sxs-lookup"><span data-stu-id="fdf79-200">Name and casing of hello parameter in JSON must match hello name and casing of hello parameter in hello stored procedure definition.</span></span> <span data-ttu-id="fdf79-201">Ha az egyik paraméter null értékű kell átadni, szintaxissal hello: `"param1": null` (kisbetűket).</span><span class="sxs-lookup"><span data-stu-id="fdf79-201">If you need pass null for a parameter, use hello syntax: `"param1": null` (all lowercase).</span></span>
 
1. <span data-ttu-id="fdf79-202">Ha nem látja ezt a gombot, kattintson a három pontot  **További** a hello parancssávon, és kattintson a **új adatcsatorna**.</span><span class="sxs-lookup"><span data-stu-id="fdf79-202">Click **... More** on hello command bar and click **New pipeline**.</span></span>
2. <span data-ttu-id="fdf79-203">Másolja és illessze be a következő JSON részlet hello:</span><span class="sxs-lookup"><span data-stu-id="fdf79-203">Copy/paste hello following JSON snippet:</span></span>   

    ```JSON
    {
        "name": "SprocActivitySamplePipeline",
        "properties": {
            "activities": [
                {
                    "type": "SqlServerStoredProcedure",
                    "typeProperties": {
                        "storedProcedureName": "sp_sample",
                        "storedProcedureParameters": {
                            "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                        }
                    },
                    "outputs": [
                        {
                            "name": "sprocsampleout"
                        }
                    ],
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "SprocActivitySample"
                }
            ],
             "start": "2017-04-02T00:00:00Z",
             "end": "2017-04-02T05:00:00Z",
            "isPaused": false
        }
    }
    ```
3. <span data-ttu-id="fdf79-204">toodeploy hello sorban, kattintson a **telepítés** hello eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="fdf79-204">toodeploy hello pipeline, click **Deploy** on hello toolbar.</span></span>  

### <a name="monitor-hello-pipeline"></a><span data-ttu-id="fdf79-205">A figyelő hello folyamat</span><span class="sxs-lookup"><span data-stu-id="fdf79-205">Monitor hello pipeline</span></span>
1. <span data-ttu-id="fdf79-206">Kattintson a **X** tooclose Data Factory Editor paneleken toonavigate biztonsági toohello adat-előállító panelt, és kattintson **Diagram**.</span><span class="sxs-lookup"><span data-stu-id="fdf79-206">Click **X** tooclose Data Factory Editor blades and toonavigate back toohello Data Factory blade, and click **Diagram**.</span></span>

    ![Diagram csempe](media/data-factory-stored-proc-activity/data-factory-diagram-tile.png)
2. <span data-ttu-id="fdf79-208">A hello **diagramnézet**, az hello adatcsatornák áttekintés és adatkészletek szerepel ez az oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="fdf79-208">In hello **Diagram View**, you see an overview of hello pipelines, and datasets used in this tutorial.</span></span>

    ![Diagram csempe](media/data-factory-stored-proc-activity/data-factory-diagram-view.png)
3. <span data-ttu-id="fdf79-210">A Diagram nézet hello, kattintson duplán a hello dataset `sprocsampleout`.</span><span class="sxs-lookup"><span data-stu-id="fdf79-210">In hello Diagram View, double-click hello dataset `sprocsampleout`.</span></span> <span data-ttu-id="fdf79-211">Megjelenik a hello szeletek üzemkész állapotban.</span><span class="sxs-lookup"><span data-stu-id="fdf79-211">You see hello slices in Ready state.</span></span> <span data-ttu-id="fdf79-212">Lehetnek öt szeletek mert szelet állítanak elő minden órában hello kezdési és befejezési időpontot hello JSON között.</span><span class="sxs-lookup"><span data-stu-id="fdf79-212">There should be five slices because a slice is produced for each hour between hello start time and end time from hello JSON.</span></span>

    ![Diagram csempe](media/data-factory-stored-proc-activity/data-factory-slices.png)
4. <span data-ttu-id="fdf79-214">A szelet esetén a **készen áll a** állapot, futtassa a `select * from sampletable` hello Azure SQL adatbázis tooverify hello adatok lekérdezése toohello tábla, hello tárolt eljárás által beszúrt.</span><span class="sxs-lookup"><span data-stu-id="fdf79-214">When a slice is in **Ready** state, run a `select * from sampletable` query against hello Azure SQL database tooverify that hello data was inserted in toohello table by hello stored procedure.</span></span>

   ![kimeneti adatok](./media/data-factory-stored-proc-activity/output.png)

   <span data-ttu-id="fdf79-216">Lásd: [figyelő hello adatcsatorna](data-factory-monitor-manage-pipelines.md) Azure Data Factory folyamatok figyelésével kapcsolatos részletes információk.</span><span class="sxs-lookup"><span data-stu-id="fdf79-216">See [Monitor hello pipeline](data-factory-monitor-manage-pipelines.md) for detailed information about monitoring Azure Data Factory pipelines.</span></span>  


## <a name="specify-an-input-dataset"></a><span data-ttu-id="fdf79-217">Adjon meg egy bemeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="fdf79-217">Specify an input dataset</span></span>
<span data-ttu-id="fdf79-218">Hello forgatókönyv tárolt eljárási tevékenység nem rendelkezik a bemeneti adatkészletek.</span><span class="sxs-lookup"><span data-stu-id="fdf79-218">In hello walkthrough, stored procedure activity does not have any input datasets.</span></span> <span data-ttu-id="fdf79-219">Ha megad egy bemeneti adatkészlet, hello tárolt eljárási tevékenység befejezéséig nem fut le hello szelet bemeneti adatkészlet érhető el (üzemkész állapotban).</span><span class="sxs-lookup"><span data-stu-id="fdf79-219">If you specify an input dataset, hello stored procedure activity does not run until hello slice of input dataset is available (in Ready state).</span></span> <span data-ttu-id="fdf79-220">hello dataset lehet egy külső adatkészletet (nem egy másik tevékenysége hello által előállított azonos) vagy egy belső adatkészlet (a előtt ezt a tevékenységet futtató hello tevékenység) felsőbb szintű tevékenység által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="fdf79-220">hello dataset can be an external dataset (that is not produced by another activity in hello same pipeline) or an internal dataset that is produced by an upstream activity (hello activity that runs before this activity).</span></span> <span data-ttu-id="fdf79-221">Hello tárolt eljárás tevékenység több bemeneti adatkészletet is megadhat.</span><span class="sxs-lookup"><span data-stu-id="fdf79-221">You can specify multiple input datasets for hello stored procedure activity.</span></span> <span data-ttu-id="fdf79-222">Ha így tesz, hello tárolt eljárás tevékenység fut csak akkor, ha minden hello bemeneti adatkészlet szeletek érhetők el (üzemkész állapotban).</span><span class="sxs-lookup"><span data-stu-id="fdf79-222">If you do so, hello stored procedure activity runs only when all hello input dataset slices are available (in Ready state).</span></span> <span data-ttu-id="fdf79-223">hello bemeneti adatkészlet hello tárolt eljárás nem használható paraméterként.</span><span class="sxs-lookup"><span data-stu-id="fdf79-223">hello input dataset cannot be consumed in hello stored procedure as a parameter.</span></span> <span data-ttu-id="fdf79-224">Csak a felhasznált toocheck hello függőségi kezdési hello tárolt eljárási tevékenység előtt.</span><span class="sxs-lookup"><span data-stu-id="fdf79-224">It is only used toocheck hello dependency before starting hello stored procedure activity.</span></span>

## <a name="chaining-with-other-activities"></a><span data-ttu-id="fdf79-225">Más tevékenységek láncolás</span><span class="sxs-lookup"><span data-stu-id="fdf79-225">Chaining with other activities</span></span>
<span data-ttu-id="fdf79-226">Ha azt szeretné, hogy ehhez a tevékenységhez egy felsőbb szintű tevékenység toochain, adja meg a hello felsőbb szintű tevékenység kimenete hello tevékenység bemenetként.</span><span class="sxs-lookup"><span data-stu-id="fdf79-226">If you want toochain an upstream activity with this activity, specify hello output of hello upstream activity as an input of this activity.</span></span> <span data-ttu-id="fdf79-227">Ha így tesz, hello tárolt eljárási tevékenység befejezéséig nem fut hello felsőbb szintű tevékenység befejezése és a felsőbb szintű tevékenység hello hello kimeneti adatkészlet érhető el (a kész állapot).</span><span class="sxs-lookup"><span data-stu-id="fdf79-227">When you do so, hello stored procedure activity does not run until hello upstream activity completes and hello output dataset of hello upstream activity is available (in Ready status).</span></span> <span data-ttu-id="fdf79-228">Megadhatja a több felsőbb szintű tevékenység kimeneti adatkészletek hello tárolt eljárás tevékenység bemeneti adatkészletek.</span><span class="sxs-lookup"><span data-stu-id="fdf79-228">You can specify output datasets of multiple upstream activities as input datasets of hello stored procedure activity.</span></span> <span data-ttu-id="fdf79-229">Ha így tesz, hello tárolt eljárási tevékenység fut, csak akkor, ha minden hello bemeneti adatkészlet szeletek nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="fdf79-229">When you do so, hello stored procedure activity runs only when all hello input dataset slices are available.</span></span>  

<span data-ttu-id="fdf79-230">A következő példa hello, hello hello másolási tevékenység eredménye: OutputDataset, ami bemenete hello a tárolt eljárási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="fdf79-230">In hello following example, hello output of hello copy activity is: OutputDataset, which is an input of hello stored procedure activity.</span></span> <span data-ttu-id="fdf79-231">Ezért hello tárolt eljárási tevékenység befejezéséig nem fut le hello másolási tevékenység befejezése és hello OutputDataset szelet érhető el (üzemkész állapotban).</span><span class="sxs-lookup"><span data-stu-id="fdf79-231">Therefore, hello stored procedure activity does not run until hello copy activity completes and hello OutputDataset slice is available (in Ready state).</span></span> <span data-ttu-id="fdf79-232">Ha több bemeneti adatkészletek ad meg, hello tárolt eljárási tevékenység nem működik addig, amíg az összes hello bemeneti adatkészlet szeletek nem érhető el (üzemkész állapotban).</span><span class="sxs-lookup"><span data-stu-id="fdf79-232">If you specify multiple input datasets, hello stored procedure activity does not run until all hello input dataset slices are available (in Ready state).</span></span> <span data-ttu-id="fdf79-233">hello bemeneti adatkészletek közvetlenül paraméterek toohello tárolt eljárási tevékenység nem használható.</span><span class="sxs-lookup"><span data-stu-id="fdf79-233">hello input datasets cannot be used directly as parameters toohello stored procedure activity.</span></span> 

<span data-ttu-id="fdf79-234">A láncolás tevékenységek további információkért lásd: [több tevékenységet egy folyamaton belül](data-factory-create-pipelines.md#multiple-activities-in-a-pipeline)</span><span class="sxs-lookup"><span data-stu-id="fdf79-234">For more information on chaining activities, see [multiple activities in a pipeline](data-factory-create-pipelines.md#multiple-activities-in-a-pipeline)</span></span>

```json
{

    "name": "ADFTutorialPipeline",
    "properties": {
        "description": "Copy data from a blob tooblob",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [ { "name": "InputDataset" } ],
                "outputs": [ { "name": "OutputDataset" } ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst"
                },
                "name": "CopyFromBlobToSQL"
            },
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "SPSproc"
                },
                "inputs": [ { "name": "OutputDataset" } ],
                "outputs": [ { "name": "SQLOutputDataset" } ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "RunStoredProcedure"
            }

        ],
        "start": "2017-04-12T00:00:00Z",
        "end": "2017-04-13T00:00:00Z",
        "isPaused": false,
    }
}
```

<span data-ttu-id="fdf79-235">Ehhez hasonlóan toolink hello tároló eljárás tevékenység **alárendelt tevékenységek** (hello futtató tevékenységek hello tárolt eljárási tevékenység befejezése után), adja meg a hello kimeneti adatkészlet hello tárolt eljárás tevékenység, egy Adjon meg hello alárendelt tevékenység hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="fdf79-235">Similarly, toolink hello store procedure activity with **downstream activities** (hello activities that run after hello stored procedure activity completes), specify hello output dataset of hello stored procedure activity as an input of hello downstream activity in hello pipeline.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fdf79-236">Ha az adatok másolása az Azure SQL Database vagy az SQL Server, konfigurálhatja a hello **SqlSink** a másolási tevékenység tooinvoke hello segítségével tárolt eljárás **sqlWriterStoredProcedureName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="fdf79-236">When copying data into Azure SQL Database or SQL Server, you can configure hello **SqlSink** in copy activity tooinvoke a stored procedure by using hello **sqlWriterStoredProcedureName** property.</span></span> <span data-ttu-id="fdf79-237">További információkért lásd: [meghívása tárolt eljárás a másolási tevékenység](data-factory-invoke-stored-procedure-from-copy-activity.md).</span><span class="sxs-lookup"><span data-stu-id="fdf79-237">For more information, see [Invoke stored procedure from copy activity](data-factory-invoke-stored-procedure-from-copy-activity.md).</span></span> <span data-ttu-id="fdf79-238">Hello tulajdonságra vonatkozó további információkért lásd: a következő összekötő cikkek hello: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="fdf79-238">For details about hello property, see hello following connector articles: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties).</span></span>
>  
> <span data-ttu-id="fdf79-239">Ha az adatok másolása az Azure SQL Database vagy az SQL Server vagy az Azure SQL Data Warehouse, konfigurálhatja a **SqlSource** a másolási tevékenység tooinvoke egy tárolt eljárás tooread adatok forrásadatbázisból hello hello segítségével  **sqlReaderStoredProcedureName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="fdf79-239">When copying data from Azure SQL Database or SQL Server or Azure SQL Data Warehouse, you can configure **SqlSource** in copy activity tooinvoke a stored procedure tooread data from hello source database by using hello **sqlReaderStoredProcedureName** property.</span></span> <span data-ttu-id="fdf79-240">További információkért lásd: a következő összekötő cikkek hello: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)</span><span class="sxs-lookup"><span data-stu-id="fdf79-240">For more information, see hello following connector articles: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)</span></span>          

## <a name="json-format"></a><span data-ttu-id="fdf79-241">JSON formátumban</span><span class="sxs-lookup"><span data-stu-id="fdf79-241">JSON format</span></span>
<span data-ttu-id="fdf79-242">Íme hello JSON formátumban tárolt eljárási tevékenység meghatározásához:</span><span class="sxs-lookup"><span data-stu-id="fdf79-242">Here is hello JSON format for defining a Stored Procedure Activity:</span></span>

```JSON
{
    "name": "SQLSPROCActivity",
    "description": "description",
    "type": "SqlServerStoredProcedure",
    "inputs":  [ { "name": "inputtable"  } ],
    "outputs":  [ { "name": "outputtable" } ],
    "typeProperties":
    {
        "storedProcedureName": "<name of hello stored procedure>",
        "storedProcedureParameters":  
        {
            "param1": "param1Value"
            …
        }
    }
}
```

<span data-ttu-id="fdf79-243">hello a következő táblázat ismerteti ezeket a JSON-tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="fdf79-243">hello following table describes these JSON properties:</span></span>

| <span data-ttu-id="fdf79-244">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="fdf79-244">Property</span></span> | <span data-ttu-id="fdf79-245">Leírás</span><span class="sxs-lookup"><span data-stu-id="fdf79-245">Description</span></span> | <span data-ttu-id="fdf79-246">Szükséges</span><span class="sxs-lookup"><span data-stu-id="fdf79-246">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="fdf79-247">név</span><span class="sxs-lookup"><span data-stu-id="fdf79-247">name</span></span> | <span data-ttu-id="fdf79-248">Hello tevékenység neve.</span><span class="sxs-lookup"><span data-stu-id="fdf79-248">Name of hello activity</span></span> |<span data-ttu-id="fdf79-249">Igen</span><span class="sxs-lookup"><span data-stu-id="fdf79-249">Yes</span></span> |
| <span data-ttu-id="fdf79-250">leírás</span><span class="sxs-lookup"><span data-stu-id="fdf79-250">description</span></span> |<span data-ttu-id="fdf79-251">Milyen hello tevékenységgel a leíró szöveg</span><span class="sxs-lookup"><span data-stu-id="fdf79-251">Text describing what hello activity is used for</span></span> |<span data-ttu-id="fdf79-252">Nem</span><span class="sxs-lookup"><span data-stu-id="fdf79-252">No</span></span> |
| <span data-ttu-id="fdf79-253">type</span><span class="sxs-lookup"><span data-stu-id="fdf79-253">type</span></span> | <span data-ttu-id="fdf79-254">Értékre kell állítani: **SqlServerStoredProcedure**</span><span class="sxs-lookup"><span data-stu-id="fdf79-254">Must be set to: **SqlServerStoredProcedure**</span></span> | <span data-ttu-id="fdf79-255">Igen</span><span class="sxs-lookup"><span data-stu-id="fdf79-255">Yes</span></span> |
| <span data-ttu-id="fdf79-256">Bemenetek</span><span class="sxs-lookup"><span data-stu-id="fdf79-256">inputs</span></span> | <span data-ttu-id="fdf79-257">Választható.</span><span class="sxs-lookup"><span data-stu-id="fdf79-257">Optional.</span></span> <span data-ttu-id="fdf79-258">Ha megad egy bemeneti adatkészlet, elérhetőnek kell lennie (a "Kész" állapotú) hello a tárolt eljárás tevékenység toorun.</span><span class="sxs-lookup"><span data-stu-id="fdf79-258">If you do specify an input dataset, it must be available (in ‘Ready’ status) for hello stored procedure activity toorun.</span></span> <span data-ttu-id="fdf79-259">hello bemeneti adatkészlet hello tárolt eljárás nem használható paraméterként.</span><span class="sxs-lookup"><span data-stu-id="fdf79-259">hello input dataset cannot be consumed in hello stored procedure as a parameter.</span></span> <span data-ttu-id="fdf79-260">Csak a felhasznált toocheck hello függőségi kezdési hello tárolt eljárási tevékenység előtt.</span><span class="sxs-lookup"><span data-stu-id="fdf79-260">It is only used toocheck hello dependency before starting hello stored procedure activity.</span></span> |<span data-ttu-id="fdf79-261">Nem</span><span class="sxs-lookup"><span data-stu-id="fdf79-261">No</span></span> |
| <span data-ttu-id="fdf79-262">kimenetek</span><span class="sxs-lookup"><span data-stu-id="fdf79-262">outputs</span></span> | <span data-ttu-id="fdf79-263">Meg kell adnia egy tárolt eljárás tevékenység egy kimeneti adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="fdf79-263">You must specify an output dataset for a stored procedure activity.</span></span> <span data-ttu-id="fdf79-264">Kimeneti adatkészlet megadja hello **ütemezés** hello a tárolt eljárási tevékenység (óránként, heti, havi, stb.).</span><span class="sxs-lookup"><span data-stu-id="fdf79-264">Output dataset specifies hello **schedule** for hello stored procedure activity (hourly, weekly, monthly, etc.).</span></span> <br/><br/><span data-ttu-id="fdf79-265">hello kimeneti adatkészlet kell használnia egy **társított szolgáltatás** , amely hivatkozik tooan Azure SQL Database vagy az Azure SQL Data Warehouse vagy a használni kívánt tárolt eljárás toorun hello SQL Server-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="fdf79-265">hello output dataset must use a **linked service** that refers tooan Azure SQL Database or an Azure SQL Data Warehouse or a SQL Server Database in which you want hello stored procedure toorun.</span></span> <br/><br/><span data-ttu-id="fdf79-266">hello kimeneti adatkészlet ki tud szolgálni hello tárolt eljárás későbbi feldolgozásra módon toopass hello eredményként egy másik tevékenység ([tevékenységek láncolás](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="fdf79-266">hello output dataset can serve as a way toopass hello result of hello stored procedure for subsequent processing by another activity ([chaining activities](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) in hello pipeline.</span></span> <span data-ttu-id="fdf79-267">Adat-előállító azonban nem a hello kimeneti a tárolt eljárás toothis DataSet adatkészlet automatikusan írnia.</span><span class="sxs-lookup"><span data-stu-id="fdf79-267">However, Data Factory does not automatically write hello output of a stored procedure toothis dataset.</span></span> <span data-ttu-id="fdf79-268">Hello tárolt eljárás írások tooa SQL táblát, amely hello kimeneti adatkészlet mutat.</span><span class="sxs-lookup"><span data-stu-id="fdf79-268">It is hello stored procedure that writes tooa SQL table that hello output dataset points to.</span></span> <br/><br/><span data-ttu-id="fdf79-269">Bizonyos esetekben hello kimeneti adatkészlet lehet egy **dummy dataset**, amellyel csak hello futtatási toospecify hello ütemezésének tárolt eljárási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="fdf79-269">In some cases, hello output dataset can be a **dummy dataset**, which is used only toospecify hello schedule for running hello stored procedure activity.</span></span> |<span data-ttu-id="fdf79-270">Igen</span><span class="sxs-lookup"><span data-stu-id="fdf79-270">Yes</span></span> |
| <span data-ttu-id="fdf79-271">storedProcedureName</span><span class="sxs-lookup"><span data-stu-id="fdf79-271">storedProcedureName</span></span> |<span data-ttu-id="fdf79-272">Hello Azure SQL database vagy az Azure SQL Data Warehouse vagy SQL Server adatbázis hello kapcsolódó szolgáltatás, amely a kimeneti tábla által használt hello által képviselt hello hello tárolt eljárás nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="fdf79-272">Specify hello name of hello stored procedure in hello Azure SQL database or Azure SQL Data Warehouse or SQL Server database that is represented by hello linked service that hello output table uses.</span></span> |<span data-ttu-id="fdf79-273">Igen</span><span class="sxs-lookup"><span data-stu-id="fdf79-273">Yes</span></span> |
| <span data-ttu-id="fdf79-274">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="fdf79-274">storedProcedureParameters</span></span> |<span data-ttu-id="fdf79-275">Adja meg a tárolt eljárás paraméter értékét.</span><span class="sxs-lookup"><span data-stu-id="fdf79-275">Specify values for stored procedure parameters.</span></span> <span data-ttu-id="fdf79-276">Ha toopass null egy paraméter, szintaxissal hello: "param1": (összes kisbetű) NULL értékű.</span><span class="sxs-lookup"><span data-stu-id="fdf79-276">If you need toopass null for a parameter, use hello syntax: "param1": null (all lower case).</span></span> <span data-ttu-id="fdf79-277">Tekintse meg a következő minta toolearn e tulajdonság használatával kapcsolatos hello.</span><span class="sxs-lookup"><span data-stu-id="fdf79-277">See hello following sample toolearn about using this property.</span></span> |<span data-ttu-id="fdf79-278">Nem</span><span class="sxs-lookup"><span data-stu-id="fdf79-278">No</span></span> |

## <a name="passing-a-static-value"></a><span data-ttu-id="fdf79-279">Egy statikus értékre továbbításához</span><span class="sxs-lookup"><span data-stu-id="fdf79-279">Passing a static value</span></span>
<span data-ttu-id="fdf79-280">Most tegyük vegyen fel egy másik oszlop, "A forgatókönyv" nevű "Dokumentum minta" nevű statikus értéket tartalmazó hello táblában.</span><span class="sxs-lookup"><span data-stu-id="fdf79-280">Now, let’s consider adding another column named ‘Scenario’ in hello table containing a static value called ‘Document sample’.</span></span>

![Mintaadatokat 2](./media/data-factory-stored-proc-activity/sample-data-2.png)

<span data-ttu-id="fdf79-282">**Tábla:**</span><span class="sxs-lookup"><span data-stu-id="fdf79-282">**Table:**</span></span>

```SQL
CREATE TABLE dbo.sampletable2
(
    Id uniqueidentifier,
    datetimestamp nvarchar(127),
    scenario nvarchar(127)
)
GO

CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable2(Id);
```

<span data-ttu-id="fdf79-283">**Tárolt eljárást:**</span><span class="sxs-lookup"><span data-stu-id="fdf79-283">**Stored procedure:**</span></span>

```SQL
CREATE PROCEDURE sp_sample2 @DateTime nvarchar(127) , @Scenario nvarchar(127)

AS

BEGIN
    INSERT INTO [sampletable2]
    VALUES (newid(), @DateTime, @Scenario)
END
```

<span data-ttu-id="fdf79-284">Ezután továbbítja a hello **forgatókönyv** hello paraméter és hello értéket a tárolt eljárási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="fdf79-284">Now, pass hello **Scenario** parameter and hello value from hello stored procedure activity.</span></span> <span data-ttu-id="fdf79-285">Hello **typeProperties** című szakasza a következő kódrészletet hello minta tűnik megelőző hello:</span><span class="sxs-lookup"><span data-stu-id="fdf79-285">hello **typeProperties** section in hello preceding sample looks like hello following snippet:</span></span>

```JSON
"typeProperties":
{
    "storedProcedureName": "sp_sample",
    "storedProcedureParameters":
    {
        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
        "Scenario": "Document sample"
    }
}
```

<span data-ttu-id="fdf79-286">**Data Factory adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="fdf79-286">**Data Factory dataset:**</span></span>

```JSON
{
    "name": "sprocsampleout2",
    "properties": {
        "published": false,
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "sampletable2"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="fdf79-287">**Data Factory-folyamat**</span><span class="sxs-lookup"><span data-stu-id="fdf79-287">**Data Factory pipeline**</span></span>

```JSON
{
    "name": "SprocActivitySamplePipeline2",
    "properties": {
        "activities": [
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "sp_sample2",
                    "storedProcedureParameters": {
                        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
                        "Scenario": "Document sample"
                    }
                },
                "outputs": [
                    {
                        "name": "sprocsampleout2"
                    }
                ],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "SprocActivitySample"
            }
        ],
        "start": "2016-10-02T00:00:00Z",
        "end": "2016-10-02T05:00:00Z"
    }
}
```