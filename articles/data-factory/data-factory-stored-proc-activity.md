---
title: "SQL Server tárolt eljárási tevékenység"
description: "Ismerje meg, hogyan használhatja az SQL Server tárolt eljárási tevékenység meghívni a Data Factory-folyamat az az Azure SQL Database vagy az Azure SQL Data Warehouse tárolt eljárást."
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
ms.openlocfilehash: 6505d9aa2c7ae003bd928e2fa82cd923a9615394
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="sql-server-stored-procedure-activity"></a><span data-ttu-id="5d563-103">SQL Server tárolt eljárási tevékenység</span><span class="sxs-lookup"><span data-stu-id="5d563-103">SQL Server Stored Procedure Activity</span></span>
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="5d563-104">Hive-tevékenység</span><span class="sxs-lookup"><span data-stu-id="5d563-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="5d563-105">A Pig-tevékenység</span><span class="sxs-lookup"><span data-stu-id="5d563-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="5d563-106">MapReduce művelethez</span><span class="sxs-lookup"><span data-stu-id="5d563-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="5d563-107">Hadoop Streamelési tevékenységben</span><span class="sxs-lookup"><span data-stu-id="5d563-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="5d563-108">A Spark-tevékenység</span><span class="sxs-lookup"><span data-stu-id="5d563-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="5d563-109">Machine Learning kötegelt végrehajtási tevékenység</span><span class="sxs-lookup"><span data-stu-id="5d563-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="5d563-110">Machine Learning Update-erőforrástevékenység</span><span class="sxs-lookup"><span data-stu-id="5d563-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="5d563-111">Tárolt eljárási tevékenység</span><span class="sxs-lookup"><span data-stu-id="5d563-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="5d563-112">Data Lake Analytics U-SQL-tevékenység</span><span class="sxs-lookup"><span data-stu-id="5d563-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="5d563-113">.NET egyéni tevékenység</span><span class="sxs-lookup"><span data-stu-id="5d563-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

## <a name="overview"></a><span data-ttu-id="5d563-114">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="5d563-114">Overview</span></span>
<span data-ttu-id="5d563-115">Adatok átalakítása tevékenységek használata egy adat-előállítóban [csővezeték](data-factory-create-pipelines.md) átalakító és előrejelzéseket és elemzések nyers adatok feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="5d563-115">You use data transformation activities in a Data Factory [pipeline](data-factory-create-pipelines.md) to transform and process raw data into predictions and insights.</span></span> <span data-ttu-id="5d563-116">A tárolt eljárási tevékenység, amely támogatja a Data Factory átalakítása tevékenységek egyike.</span><span class="sxs-lookup"><span data-stu-id="5d563-116">The Stored Procedure Activity is one of the transformation activities that Data Factory supports.</span></span> <span data-ttu-id="5d563-117">Ez a cikk épít, a [adatok átalakítása tevékenységek](data-factory-data-transformation-activities.md) cikk, amelynek során az adatok átalakítása és a Data Factory támogatott átalakítása tevékenységek általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="5d563-117">This article builds on the [data transformation activities](data-factory-data-transformation-activities.md) article, which presents a general overview of data transformation and the supported transformation activities in Data Factory.</span></span>

<span data-ttu-id="5d563-118">A tárolt eljárási tevékenység segítségével meghívása tárolt eljárás valamelyik a következő adatokat tárolja, a vállalati vagy egy Azure virtuális gépen (VM):</span><span class="sxs-lookup"><span data-stu-id="5d563-118">You can use the Stored Procedure Activity to invoke a stored procedure in one of the following data stores in your enterprise or on an Azure virtual machine (VM):</span></span> 

- <span data-ttu-id="5d563-119">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="5d563-119">Azure SQL Database</span></span>
- <span data-ttu-id="5d563-120">Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="5d563-120">Azure SQL Data Warehouse</span></span>
- <span data-ttu-id="5d563-121">SQL Server-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="5d563-121">SQL Server Database.</span></span>  <span data-ttu-id="5d563-122">Ha SQL Server használ, telepítse az adatkezelési átjáró ugyanazon a számítógépen, amelyen az adatbázis vagy egy külön számítógépen, amely hozzáféréssel rendelkezik az adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="5d563-122">If you are using SQL Server, install Data Management Gateway on the same machine that hosts the database or on a separate machine that has access to the database.</span></span> <span data-ttu-id="5d563-123">Az adatkezelési átjáró egy összetevő, amely összeköti az adatok a helyszínen vagy a Azure VM a cloud serviceshez adatforrásokat felügyelt és biztonságos módon.</span><span class="sxs-lookup"><span data-stu-id="5d563-123">Data Management Gateway is a component that connects data sources on-premises/on Azure VM with cloud services in a secure and managed way.</span></span> <span data-ttu-id="5d563-124">Lásd: [az adatkezelési átjáró](data-factory-data-management-gateway.md) cikkben alább.</span><span class="sxs-lookup"><span data-stu-id="5d563-124">See [Data Management Gateway](data-factory-data-management-gateway.md) article for details.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5d563-125">Ha az adatok másolása az Azure SQL Database vagy az SQL Server, konfigurálhatja a **SqlSink** a másolási tevékenység tárolt eljárás használatával meghívni a **sqlWriterStoredProcedureName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="5d563-125">When copying data into Azure SQL Database or SQL Server, you can configure the **SqlSink** in copy activity to invoke a stored procedure by using the **sqlWriterStoredProcedureName** property.</span></span> <span data-ttu-id="5d563-126">További információkért lásd: [meghívása tárolt eljárás a másolási tevékenység](data-factory-invoke-stored-procedure-from-copy-activity.md).</span><span class="sxs-lookup"><span data-stu-id="5d563-126">For more information, see [Invoke stored procedure from copy activity](data-factory-invoke-stored-procedure-from-copy-activity.md).</span></span> <span data-ttu-id="5d563-127">A tulajdonság kapcsolatos tudnivalókért lásd az alábbi összekötő cikkek: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="5d563-127">For details about the property, see following connector articles: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties).</span></span> <span data-ttu-id="5d563-128">Adatok másolása az Azure SQL Data Warehouse a másolási tevékenység során a tárolt eljárás meghívása nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="5d563-128">Invoking a stored procedure while copying data into an Azure SQL Data Warehouse by using a copy activity is not supported.</span></span> <span data-ttu-id="5d563-129">De a tárolt eljárási tevékenység segítségével az SQL Data Warehouse tárolt eljárás hívása.</span><span class="sxs-lookup"><span data-stu-id="5d563-129">But, you can use the stored procedure activity to invoke a stored procedure in a SQL Data Warehouse.</span></span> 
>  
> <span data-ttu-id="5d563-130">Ha az adatok másolása az Azure SQL Database vagy az SQL Server vagy az Azure SQL Data Warehouse, konfigurálhatja a **SqlSource** a másolási tevékenység meghívni egy tárolt eljárás a forrás-adatbázis használatával adatokat olvasni az **sqlReaderStoredProcedureName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="5d563-130">When copying data from Azure SQL Database or SQL Server or Azure SQL Data Warehouse, you can configure **SqlSource** in copy activity to invoke a stored procedure to read data from the source database by using the **sqlReaderStoredProcedureName** property.</span></span> <span data-ttu-id="5d563-131">További információkért tekintse meg a következő összekötő-cikkek: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)</span><span class="sxs-lookup"><span data-stu-id="5d563-131">For more information, see the following connector articles: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)</span></span>          


<span data-ttu-id="5d563-132">A következő forgatókönyv egy folyamaton belül a tárolt eljárási tevékenység segítségével Azure SQL-adatbázisban tárolt eljárás hívása.</span><span class="sxs-lookup"><span data-stu-id="5d563-132">The following walkthrough uses the Stored Procedure Activity in a pipeline to invoke a stored procedure in an Azure SQL database.</span></span> 

## <a name="walkthrough"></a><span data-ttu-id="5d563-133">Útmutatás</span><span class="sxs-lookup"><span data-stu-id="5d563-133">Walkthrough</span></span>
### <a name="sample-table-and-stored-procedure"></a><span data-ttu-id="5d563-134">A minta-tábla és tárolt eljárás</span><span class="sxs-lookup"><span data-stu-id="5d563-134">Sample table and stored procedure</span></span>
1. <span data-ttu-id="5d563-135">Hozza létre a következő **tábla** az Azure SQL adatbázis SQL Server Management Studio vagy bármilyen más ismeri a Feladatkezelő segítségével.</span><span class="sxs-lookup"><span data-stu-id="5d563-135">Create the following **table** in your Azure SQL Database using SQL Server Management Studio or any other tool you are comfortable with.</span></span> <span data-ttu-id="5d563-136">A datetimestamp oszlop, a dátum és idő, amikor jön létre a megfelelő Azonosítóhoz.</span><span class="sxs-lookup"><span data-stu-id="5d563-136">The datetimestamp column is the date and time when the corresponding ID is generated.</span></span>

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
    <span data-ttu-id="5d563-137">Az egyedi azonosított és a datetimestamp oszlop dátumát és időpontját a megfelelő Azonosítóhoz létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="5d563-137">Id is the unique identified and the datetimestamp column is the date and time when the corresponding ID is generated.</span></span>
    
    ![Mintaadatok](./media/data-factory-stored-proc-activity/sample-data.png)

    <span data-ttu-id="5d563-139">Ez a példa a tárolt eljárás az Azure SQL adatbázis.</span><span class="sxs-lookup"><span data-stu-id="5d563-139">In this sample, the stored procedure is in an Azure SQL Database.</span></span> <span data-ttu-id="5d563-140">A tárolt eljárás egy Azure SQL Data warehouse-bA és az SQL Server-adatbázis, a megközelítést akkor hasonló.</span><span class="sxs-lookup"><span data-stu-id="5d563-140">If the stored procedure is in an Azure SQL Data Warehouse and SQL Server Database, the approach is similar.</span></span> <span data-ttu-id="5d563-141">SQL Server-adatbázis, telepítenie kell egy [az adatkezelési átjáró](data-factory-data-management-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="5d563-141">For a SQL Server database, you must install a [Data Management Gateway](data-factory-data-management-gateway.md).</span></span>
2. <span data-ttu-id="5d563-142">Hozza létre a következő **tárolt eljárás** , amely beszúrja az adatokat a **sampletable**.</span><span class="sxs-lookup"><span data-stu-id="5d563-142">Create the following **stored procedure** that inserts data in to the **sampletable**.</span></span>

    ```SQL
    CREATE PROCEDURE sp_sample @DateTime nvarchar(127)
    AS

    BEGIN
        INSERT INTO [sampletable]
        VALUES (newid(), @DateTime)
    END
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="5d563-143">**Név** és **kis-és** a paraméter (ebben a példában szereplő DateTime) meg kell egyeznie a feldolgozási sor/tevékenység JSON megadott paramétert.</span><span class="sxs-lookup"><span data-stu-id="5d563-143">**Name** and **casing** of the parameter (DateTime in this example) must match that of parameter specified in the pipeline/activity JSON.</span></span> <span data-ttu-id="5d563-144">Ügyeljen arra, hogy a tárolt eljárás definícióban  **@**  paraméter előtagjaként szolgál.</span><span class="sxs-lookup"><span data-stu-id="5d563-144">In the stored procedure definition, ensure that **@** is used as a prefix for the parameter.</span></span>

### <a name="create-a-data-factory"></a><span data-ttu-id="5d563-145">Data factory létrehozása</span><span class="sxs-lookup"><span data-stu-id="5d563-145">Create a data factory</span></span>
1. <span data-ttu-id="5d563-146">Jelentkezzen be az [Azure Portalra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="5d563-146">Log in to [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="5d563-147">Kattintson a **új** a bal oldali menüben kattintson **Eszközintelligencia + analitika**, és kattintson a **adat-előállító**.</span><span class="sxs-lookup"><span data-stu-id="5d563-147">Click **NEW** on the left menu, click **Intelligence + Analytics**, and click **Data Factory**.</span></span>

    ![Új adat-előállító](media/data-factory-stored-proc-activity/new-data-factory.png)    
3. <span data-ttu-id="5d563-149">Az a **új adat-előállító** panelen adjon meg **SProcDF** nevét.</span><span class="sxs-lookup"><span data-stu-id="5d563-149">In the **New data factory** blade, enter **SProcDF** for the Name.</span></span> <span data-ttu-id="5d563-150">Az Azure Data Factory neve **globálisan egyedi**.</span><span class="sxs-lookup"><span data-stu-id="5d563-150">Azure Data Factory names are **globally unique**.</span></span> <span data-ttu-id="5d563-151">A névvel, a gyári sikeres létrehozásának engedélyezése a data factory neve előtag van szüksége.</span><span class="sxs-lookup"><span data-stu-id="5d563-151">You need to prefix the name of the data factory with your name, to enable the successful creation of the factory.</span></span>

   ![Új adat-előállító](media/data-factory-stored-proc-activity/new-data-factory-blade.png)         
4. <span data-ttu-id="5d563-153">Válassza ki a **Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="5d563-153">Select your **Azure subscription**.</span></span>
5. <span data-ttu-id="5d563-154">A **erőforráscsoport**, hajtsa végre a következő lépésekből áll:</span><span class="sxs-lookup"><span data-stu-id="5d563-154">For **Resource Group**, do one of the following steps:</span></span>
   1. <span data-ttu-id="5d563-155">Kattintson a **hozzon létre új** , és adja meg az erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="5d563-155">Click **Create new** and enter a name for the resource group.</span></span>
   2. <span data-ttu-id="5d563-156">Kattintson a **meglévő** , és válasszon ki egy meglévő erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="5d563-156">Click **Use existing** and select an existing resource group.</span></span>  
6. <span data-ttu-id="5d563-157">Válassza ki a Data Factory **helyét**.</span><span class="sxs-lookup"><span data-stu-id="5d563-157">Select the **location** for the data factory.</span></span>
7. <span data-ttu-id="5d563-158">Válassza ki **rögzítés az irányítópulton** , hogy megjelenik a data factory az irányítópulton jelentkezik be a következő alkalommal.</span><span class="sxs-lookup"><span data-stu-id="5d563-158">Select **Pin to dashboard** so that you can see the data factory on the dashboard next time you log in.</span></span>
8. <span data-ttu-id="5d563-159">Kattintson a **Create** (Létrehozás) elemre a **New data factory** (Új data factory) panelen.</span><span class="sxs-lookup"><span data-stu-id="5d563-159">Click **Create** on the **New data factory** blade.</span></span>
9. <span data-ttu-id="5d563-160">Megjelenik a data factory létrehozása a **irányítópult** az Azure portálról.</span><span class="sxs-lookup"><span data-stu-id="5d563-160">You see the data factory being created in the **dashboard** of the Azure portal.</span></span> <span data-ttu-id="5d563-161">A data factory sikeres létrehozása után megjelenik a data factory oldal, amely megjeleníti a data factory tartalmát.</span><span class="sxs-lookup"><span data-stu-id="5d563-161">After the data factory has been created successfully, you see the data factory page, which shows you the contents of the data factory.</span></span>

   ![Data Factory kezdőlap](media/data-factory-stored-proc-activity/data-factory-home-page.png)

### <a name="create-an-azure-sql-linked-service"></a><span data-ttu-id="5d563-163">Azure SQL társított szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="5d563-163">Create an Azure SQL linked service</span></span>
<span data-ttu-id="5d563-164">Miután létrehozta a data factory, létrehozhat egy Azure SQL társított szolgáltatás, amely az Azure SQL-adatbázis, amely tartalmazza a sampletable tábla és a sp_sample tárolt eljárás, hogy a data factory.</span><span class="sxs-lookup"><span data-stu-id="5d563-164">After creating the data factory, you create an Azure SQL linked service that links your Azure SQL database, which contains the sampletable table and sp_sample stored procedure, to your data factory.</span></span>

1. <span data-ttu-id="5d563-165">Kattintson a **Szerző és központi telepítése** a a **Data Factory** paneljén **SProcDF** a Data Factory Editor elindításához.</span><span class="sxs-lookup"><span data-stu-id="5d563-165">Click **Author and deploy** on the **Data Factory** blade for **SProcDF** to launch the Data Factory Editor.</span></span>
2. <span data-ttu-id="5d563-166">Kattintson a **az új adattároló** a parancs megnyitásához, és válassza a **Azure SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="5d563-166">Click **New data store** on the command bar and choose **Azure SQL Database**.</span></span> <span data-ttu-id="5d563-167">A JSON-parancsfájl létrehozásához Azure SQL társított szolgáltatásnak a szerkesztővel kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="5d563-167">You should see the JSON script for creating an Azure SQL linked service in the editor.</span></span>

   ![Új adattár](media/data-factory-stored-proc-activity/new-data-store.png)
3. <span data-ttu-id="5d563-169">A JSON-parancsfájl a következő módosításokat:</span><span class="sxs-lookup"><span data-stu-id="5d563-169">In the JSON script, make the following changes:</span></span>

   1. <span data-ttu-id="5d563-170">Cserélje le `<servername>` nevű, az Azure SQL Database-kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="5d563-170">Replace `<servername>` with the name of your Azure SQL Database server.</span></span>
   2. <span data-ttu-id="5d563-171">Cserélje le `<databasename>` az adatbázissal, amelyben létrehozta a tábla és a tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="5d563-171">Replace `<databasename>` with the database in which you created the table and the stored procedure.</span></span>
   3. <span data-ttu-id="5d563-172">Cserélje le `<username@servername>` a felhasználói fiókkal, amely hozzáféréssel rendelkezik az adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="5d563-172">Replace `<username@servername>` with the user account that has access to the database.</span></span>
   4. <span data-ttu-id="5d563-173">Cserélje le `<password>` a felhasználói fiók jelszavával.</span><span class="sxs-lookup"><span data-stu-id="5d563-173">Replace `<password>` with the password for the user account.</span></span>

      ![Új adattár](media/data-factory-stored-proc-activity/azure-sql-linked-service.png)
4. <span data-ttu-id="5d563-175">A társított szolgáltatás telepítéséhez kattintson **telepítés** a parancssávon.</span><span class="sxs-lookup"><span data-stu-id="5d563-175">To deploy the linked service, click **Deploy** on the command bar.</span></span> <span data-ttu-id="5d563-176">Győződjön meg arról, hogy megjelenik-e a bal oldali fanézetben AzureSqlLinkedService.</span><span class="sxs-lookup"><span data-stu-id="5d563-176">Confirm that you see the AzureSqlLinkedService in the tree view on the left.</span></span>

    ![fanézet, a társított szolgáltatás](media/data-factory-stored-proc-activity/tree-view.png)

### <a name="create-an-output-dataset"></a><span data-ttu-id="5d563-178">Kimeneti adatkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="5d563-178">Create an output dataset</span></span>
<span data-ttu-id="5d563-179">Meg kell adnia egy kimeneti adatkészlet egy tárolt eljárás tevékenység, még akkor is, ha a tárolt eljárás nem hozhatók létre adatokat.</span><span class="sxs-lookup"><span data-stu-id="5d563-179">You must specify an output dataset for a stored procedure activity even if the stored procedure does not produce any data.</span></span> <span data-ttu-id="5d563-180">Amely, mert a kimeneti adatkészletet, amelyek a tevékenység (milyen gyakran a tevékenység fut - óránként, naponta, stb.) az ütemezés meghajtók van.</span><span class="sxs-lookup"><span data-stu-id="5d563-180">That's because it's the output dataset that drives the schedule of the activity (how often the activity is run - hourly, daily, etc.).</span></span> <span data-ttu-id="5d563-181">A kimeneti adatkészlet kell használnia egy **társított szolgáltatás** , amely egy Azure SQL Database vagy az Azure SQL Data Warehouse vagy a tárolt eljárás futtatásához használni kívánt SQL Server-adatbázis hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="5d563-181">The output dataset must use a **linked service** that refers to an Azure SQL Database or an Azure SQL Data Warehouse or a SQL Server Database in which you want the stored procedure to run.</span></span> <span data-ttu-id="5d563-182">A kimeneti adatkészlet felelt meg a tárolt eljárás eredményét a későbbi feldolgozásra, amelyet egy másik tevékenység módon működhetnek ([tevékenységek láncolás](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) folyamatban.</span><span class="sxs-lookup"><span data-stu-id="5d563-182">The output dataset can serve as a way to pass the result of the stored procedure for subsequent processing by another activity ([chaining activities](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) in the pipeline.</span></span> <span data-ttu-id="5d563-183">Azonban adat-előállító nem automatikusan kiírhatja a kimenetet tárolt eljárás ehhez a DataSet adatkészlethez.</span><span class="sxs-lookup"><span data-stu-id="5d563-183">However, Data Factory does not automatically write the output of a stored procedure to this dataset.</span></span> <span data-ttu-id="5d563-184">A tárolt eljárás, amely egy SQL táblázat, amely a kimeneti adatkészlet mutat ír.</span><span class="sxs-lookup"><span data-stu-id="5d563-184">It is the stored procedure that writes to a SQL table that the output dataset points to.</span></span> <span data-ttu-id="5d563-185">Bizonyos esetekben a kimeneti adatkészlet lehet egy **dummy dataset** (a DataSet adatkészlet mutat, a tárolt eljárás kimeneti valóban nem rendelkező tábla).</span><span class="sxs-lookup"><span data-stu-id="5d563-185">In some cases, the output dataset can be a **dummy dataset** (a dataset that points to a table that does not really hold output of the stored procedure).</span></span> <span data-ttu-id="5d563-186">Az üres adatkészlet csak ütemezés megadása a tárolt eljárási tevékenység fut szolgál.</span><span class="sxs-lookup"><span data-stu-id="5d563-186">This dummy dataset is used only to specify the schedule for running the stored procedure activity.</span></span> 

1. <span data-ttu-id="5d563-187">Ha nem látja ezt a gombot, kattintson a három pontot  **További** kattintson az eszköztár **új adatkészlet**, és kattintson a **Azure SQL**.</span><span class="sxs-lookup"><span data-stu-id="5d563-187">Click **... More** on the toolbar, click **New dataset**, and click **Azure SQL**.</span></span> <span data-ttu-id="5d563-188">**Új adatkészlet** a parancssávon, és válassza ki a **Azure SQL**.</span><span class="sxs-lookup"><span data-stu-id="5d563-188">**New dataset** on the command bar and select **Azure SQL**.</span></span>

    ![fanézet, a társított szolgáltatás](media/data-factory-stored-proc-activity/new-dataset.png)
2. <span data-ttu-id="5d563-190">Másolja és illessze be a következő JSON parancsfájl a JSON-szerkesztőbe.</span><span class="sxs-lookup"><span data-stu-id="5d563-190">Copy/paste the following JSON script in to the JSON editor.</span></span>

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
3. <span data-ttu-id="5d563-191">A dataset telepítéséhez kattintson **telepítés** a parancssávon.</span><span class="sxs-lookup"><span data-stu-id="5d563-191">To deploy the dataset, click **Deploy** on the command bar.</span></span> <span data-ttu-id="5d563-192">Győződjön meg arról, hogy a fanézetben a dataset látni.</span><span class="sxs-lookup"><span data-stu-id="5d563-192">Confirm that you see the dataset in the tree view.</span></span>

    ![Fanézet, a társított szolgáltatások](media/data-factory-stored-proc-activity/tree-view-2.png)

### <a name="create-a-pipeline-with-sqlserverstoredprocedure-activity"></a><span data-ttu-id="5d563-194">Hozzon létre egy folyamatot SqlServerStoredProcedure tevékenység</span><span class="sxs-lookup"><span data-stu-id="5d563-194">Create a pipeline with SqlServerStoredProcedure activity</span></span>
<span data-ttu-id="5d563-195">Most hozzon létre egy folyamatot egy tárolt eljárás tevékenységet.</span><span class="sxs-lookup"><span data-stu-id="5d563-195">Now, let's create a pipeline with a stored procedure activity.</span></span> 

<span data-ttu-id="5d563-196">Figyelje meg a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="5d563-196">Notice the following properties:</span></span> 

- <span data-ttu-id="5d563-197">A **típus** tulajdonsága **SqlServerStoredProcedure**.</span><span class="sxs-lookup"><span data-stu-id="5d563-197">The **type** property is set to **SqlServerStoredProcedure**.</span></span> 
- <span data-ttu-id="5d563-198">A **storedProcedureName** típus tulajdonságainak értéke **sp_sample** (a tárolt eljárás neve).</span><span class="sxs-lookup"><span data-stu-id="5d563-198">The **storedProcedureName** in type properties is set to **sp_sample** (name of the stored procedure).</span></span>
- <span data-ttu-id="5d563-199">A **storedProcedureParameters** szakasz nevű egy paraméter **/időből**.</span><span class="sxs-lookup"><span data-stu-id="5d563-199">The **storedProcedureParameters** section contains one parameter named **DataTime**.</span></span> <span data-ttu-id="5d563-200">Név és a kis-és a JSON-paraméter meg kell egyeznie a nevét, és a kis-és a tárolt eljárás definícióban paraméter.</span><span class="sxs-lookup"><span data-stu-id="5d563-200">Name and casing of the parameter in JSON must match the name and casing of the parameter in the stored procedure definition.</span></span> <span data-ttu-id="5d563-201">Ha az egyik paraméter null értékű kell átadni, használja a szintaxist: `"param1": null` (kisbetűket).</span><span class="sxs-lookup"><span data-stu-id="5d563-201">If you need pass null for a parameter, use the syntax: `"param1": null` (all lowercase).</span></span>
 
1. <span data-ttu-id="5d563-202">Ha nem látja ezt a gombot, kattintson a három pontot  **További** a parancssávon, majd kattintson a **új adatcsatorna**.</span><span class="sxs-lookup"><span data-stu-id="5d563-202">Click **... More** on the command bar and click **New pipeline**.</span></span>
2. <span data-ttu-id="5d563-203">Másolja és illessze be a következő JSON kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="5d563-203">Copy/paste the following JSON snippet:</span></span>   

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
3. <span data-ttu-id="5d563-204">Kattintson újra az adatcsatornát, **telepítés** az eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="5d563-204">To deploy the pipeline, click **Deploy** on the toolbar.</span></span>  

### <a name="monitor-the-pipeline"></a><span data-ttu-id="5d563-205">A folyamat figyelése</span><span class="sxs-lookup"><span data-stu-id="5d563-205">Monitor the pipeline</span></span>
1. <span data-ttu-id="5d563-206">A Data Factory Editor paneljeinek a bezárásához és a Data Factory panelre való visszatéréshez kattintson az **X**, majd a **Diagram** elemre.</span><span class="sxs-lookup"><span data-stu-id="5d563-206">Click **X** to close Data Factory Editor blades and to navigate back to the Data Factory blade, and click **Diagram**.</span></span>

    ![Diagram csempe](media/data-factory-stored-proc-activity/data-factory-diagram-tile.png)
2. <span data-ttu-id="5d563-208">A **diagramnézet** áttekintést nyújt az oktatóanyagban használt folyamatokról és adatkészletekről.</span><span class="sxs-lookup"><span data-stu-id="5d563-208">In the **Diagram View**, you see an overview of the pipelines, and datasets used in this tutorial.</span></span>

    ![Diagram csempe](media/data-factory-stored-proc-activity/data-factory-diagram-view.png)
3. <span data-ttu-id="5d563-210">A Diagram nézet megnyitásához kattintson duplán a dataset `sprocsampleout`.</span><span class="sxs-lookup"><span data-stu-id="5d563-210">In the Diagram View, double-click the dataset `sprocsampleout`.</span></span> <span data-ttu-id="5d563-211">Megjelenik a szeletek üzemkész állapotban.</span><span class="sxs-lookup"><span data-stu-id="5d563-211">You see the slices in Ready state.</span></span> <span data-ttu-id="5d563-212">Lehetnek öt szeletek mert szelet állítanak elő minden órában a kezdési és befejezési időpontja a JSON formátumból között.</span><span class="sxs-lookup"><span data-stu-id="5d563-212">There should be five slices because a slice is produced for each hour between the start time and end time from the JSON.</span></span>

    ![Diagram csempe](media/data-factory-stored-proc-activity/data-factory-slices.png)
4. <span data-ttu-id="5d563-214">A szelet esetén a **készen** állapot, futtassa a `select * from sampletable` győződjön meg arról, hogy az adatok lett szúrja be a tábla a következő tárolt eljárást az Azure SQL-adatbázis lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="5d563-214">When a slice is in **Ready** state, run a `select * from sampletable` query against the Azure SQL database to verify that the data was inserted in to the table by the stored procedure.</span></span>

   ![kimeneti adatok](./media/data-factory-stored-proc-activity/output.png)

   <span data-ttu-id="5d563-216">Lásd: [a folyamat figyelése](data-factory-monitor-manage-pipelines.md) Azure Data Factory folyamatok figyelésével kapcsolatos részletes információk.</span><span class="sxs-lookup"><span data-stu-id="5d563-216">See [Monitor the pipeline](data-factory-monitor-manage-pipelines.md) for detailed information about monitoring Azure Data Factory pipelines.</span></span>  


## <a name="specify-an-input-dataset"></a><span data-ttu-id="5d563-217">Adjon meg egy bemeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="5d563-217">Specify an input dataset</span></span>
<span data-ttu-id="5d563-218">A forgatókönyv tárolt eljárási tevékenység nem rendelkezik a bemeneti adatkészletek.</span><span class="sxs-lookup"><span data-stu-id="5d563-218">In the walkthrough, stored procedure activity does not have any input datasets.</span></span> <span data-ttu-id="5d563-219">Ha megad egy bemeneti adatkészlet, a tárolt eljárási tevékenység befejezéséig nem fut le a szelet bemeneti adatkészlet érhető el (üzemkész állapotban).</span><span class="sxs-lookup"><span data-stu-id="5d563-219">If you specify an input dataset, the stored procedure activity does not run until the slice of input dataset is available (in Ready state).</span></span> <span data-ttu-id="5d563-220">Az adatkészlet egy külső adatkészletet (nem által létrehozott azonos egy másik tevékenysége) vagy egy belső adatkészlet (a tevékenység, mielőtt ezt a tevékenységet futtató) felsőbb szintű tevékenység által létrehozott lehet.</span><span class="sxs-lookup"><span data-stu-id="5d563-220">The dataset can be an external dataset (that is not produced by another activity in the same pipeline) or an internal dataset that is produced by an upstream activity (the activity that runs before this activity).</span></span> <span data-ttu-id="5d563-221">A tárolt eljárás tevékenység több bemeneti adatkészletet is megadhat.</span><span class="sxs-lookup"><span data-stu-id="5d563-221">You can specify multiple input datasets for the stored procedure activity.</span></span> <span data-ttu-id="5d563-222">Ha így tesz, a tárolt eljárás tevékenység fut, csak akkor, ha a bemeneti adatkészlet szeleteket érhetők el (üzemkész állapotban).</span><span class="sxs-lookup"><span data-stu-id="5d563-222">If you do so, the stored procedure activity runs only when all the input dataset slices are available (in Ready state).</span></span> <span data-ttu-id="5d563-223">A bemeneti adatkészletet a tárolt eljárás nem használható paraméterként.</span><span class="sxs-lookup"><span data-stu-id="5d563-223">The input dataset cannot be consumed in the stored procedure as a parameter.</span></span> <span data-ttu-id="5d563-224">Csak a tárolt eljárási tevékenység megkezdése előtt ellenőrizze, a függőség szolgál.</span><span class="sxs-lookup"><span data-stu-id="5d563-224">It is only used to check the dependency before starting the stored procedure activity.</span></span>

## <a name="chaining-with-other-activities"></a><span data-ttu-id="5d563-225">Más tevékenységek láncolás</span><span class="sxs-lookup"><span data-stu-id="5d563-225">Chaining with other activities</span></span>
<span data-ttu-id="5d563-226">Ha azt szeretné, a tanúsítványlánc egy felsőbb szintű tevékenység ehhez a tevékenységhez, adja meg a felsőbb szintű tevékenység a tevékenység bemeneti adatokként.</span><span class="sxs-lookup"><span data-stu-id="5d563-226">If you want to chain an upstream activity with this activity, specify the output of the upstream activity as an input of this activity.</span></span> <span data-ttu-id="5d563-227">Ha így tesz, a tárolt eljárási tevékenység befejezéséig nem fut a felsőbb szintű tevékenység befejezése és a felsőbb szintű tevékenység kimeneti adatkészlet érhető el (a kész állapot).</span><span class="sxs-lookup"><span data-stu-id="5d563-227">When you do so, the stored procedure activity does not run until the upstream activity completes and the output dataset of the upstream activity is available (in Ready status).</span></span> <span data-ttu-id="5d563-228">Megadhatja a tárolt eljárás tevékenység bemeneti adatkészletek több felsőbb szintű tevékenység kimeneti adatkészletek.</span><span class="sxs-lookup"><span data-stu-id="5d563-228">You can specify output datasets of multiple upstream activities as input datasets of the stored procedure activity.</span></span> <span data-ttu-id="5d563-229">Ha így tesz, a tárolt eljárás tevékenység fut, csak akkor, ha a bemeneti adatkészlet szeleteket érhetők el.</span><span class="sxs-lookup"><span data-stu-id="5d563-229">When you do so, the stored procedure activity runs only when all the input dataset slices are available.</span></span>  

<span data-ttu-id="5d563-230">A következő példában a másolási tevékenység eredménye: OutputDataset, amely a tárolt eljárás tevékenység bemenete.</span><span class="sxs-lookup"><span data-stu-id="5d563-230">In the following example, the output of the copy activity is: OutputDataset, which is an input of the stored procedure activity.</span></span> <span data-ttu-id="5d563-231">Ezért a tárolt eljárási tevékenység befejezéséig nem fut le a másolási tevékenység befejezése és a OutputDataset szelet érhető el (üzemkész állapotban).</span><span class="sxs-lookup"><span data-stu-id="5d563-231">Therefore, the stored procedure activity does not run until the copy activity completes and the OutputDataset slice is available (in Ready state).</span></span> <span data-ttu-id="5d563-232">Több bemeneti adatkészletek ad meg, ha a tárolt eljárási tevékenység nem működik, amíg a bemeneti adatkészlet szeleteket érhetők el (üzemkész állapotban).</span><span class="sxs-lookup"><span data-stu-id="5d563-232">If you specify multiple input datasets, the stored procedure activity does not run until all the input dataset slices are available (in Ready state).</span></span> <span data-ttu-id="5d563-233">A bemeneti adatkészletek közvetlenül a tárolt eljárás tevékenység paraméterei nem használható.</span><span class="sxs-lookup"><span data-stu-id="5d563-233">The input datasets cannot be used directly as parameters to the stored procedure activity.</span></span> 

<span data-ttu-id="5d563-234">A láncolás tevékenységek további információkért lásd: [több tevékenységet egy folyamaton belül](data-factory-create-pipelines.md#multiple-activities-in-a-pipeline)</span><span class="sxs-lookup"><span data-stu-id="5d563-234">For more information on chaining activities, see [multiple activities in a pipeline](data-factory-create-pipelines.md#multiple-activities-in-a-pipeline)</span></span>

```json
{

    "name": "ADFTutorialPipeline",
    "properties": {
        "description": "Copy data from a blob to blob",
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

<span data-ttu-id="5d563-235">Hasonlóképpen a tárolási eljárás tevékenység csatolásához **alárendelt tevékenységek** (a futtató tevékenységek a tárolt eljárási tevékenység befejezése után), a folyamat az alárendelt tevékenység bemeneti adatokként adja meg a tárolt eljárás tevékenység kimeneti adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="5d563-235">Similarly, to link the store procedure activity with **downstream activities** (the activities that run after the stored procedure activity completes), specify the output dataset of the stored procedure activity as an input of the downstream activity in the pipeline.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5d563-236">Ha az adatok másolása az Azure SQL Database vagy az SQL Server, konfigurálhatja a **SqlSink** a másolási tevékenység tárolt eljárás használatával meghívni a **sqlWriterStoredProcedureName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="5d563-236">When copying data into Azure SQL Database or SQL Server, you can configure the **SqlSink** in copy activity to invoke a stored procedure by using the **sqlWriterStoredProcedureName** property.</span></span> <span data-ttu-id="5d563-237">További információkért lásd: [meghívása tárolt eljárás a másolási tevékenység](data-factory-invoke-stored-procedure-from-copy-activity.md).</span><span class="sxs-lookup"><span data-stu-id="5d563-237">For more information, see [Invoke stored procedure from copy activity](data-factory-invoke-stored-procedure-from-copy-activity.md).</span></span> <span data-ttu-id="5d563-238">A tulajdonság, lásd: a következő összekötő-cikkek: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="5d563-238">For details about the property, see the following connector articles: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties).</span></span>
>  
> <span data-ttu-id="5d563-239">Ha az adatok másolása az Azure SQL Database vagy az SQL Server vagy az Azure SQL Data Warehouse, konfigurálhatja a **SqlSource** a másolási tevékenység meghívni egy tárolt eljárás a forrás-adatbázis használatával adatokat olvasni az **sqlReaderStoredProcedureName** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="5d563-239">When copying data from Azure SQL Database or SQL Server or Azure SQL Data Warehouse, you can configure **SqlSource** in copy activity to invoke a stored procedure to read data from the source database by using the **sqlReaderStoredProcedureName** property.</span></span> <span data-ttu-id="5d563-240">További információkért tekintse meg a következő összekötő-cikkek: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)</span><span class="sxs-lookup"><span data-stu-id="5d563-240">For more information, see the following connector articles: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)</span></span>          

## <a name="json-format"></a><span data-ttu-id="5d563-241">JSON formátumban</span><span class="sxs-lookup"><span data-stu-id="5d563-241">JSON format</span></span>
<span data-ttu-id="5d563-242">A tárolt eljárási tevékenység meghatározásához a JSON formátum a következő:</span><span class="sxs-lookup"><span data-stu-id="5d563-242">Here is the JSON format for defining a Stored Procedure Activity:</span></span>

```JSON
{
    "name": "SQLSPROCActivity",
    "description": "description",
    "type": "SqlServerStoredProcedure",
    "inputs":  [ { "name": "inputtable"  } ],
    "outputs":  [ { "name": "outputtable" } ],
    "typeProperties":
    {
        "storedProcedureName": "<name of the stored procedure>",
        "storedProcedureParameters":  
        {
            "param1": "param1Value"
            …
        }
    }
}
```

<span data-ttu-id="5d563-243">A következő táblázat ismerteti ezeket a JSON-tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="5d563-243">The following table describes these JSON properties:</span></span>

| <span data-ttu-id="5d563-244">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="5d563-244">Property</span></span> | <span data-ttu-id="5d563-245">Leírás</span><span class="sxs-lookup"><span data-stu-id="5d563-245">Description</span></span> | <span data-ttu-id="5d563-246">Szükséges</span><span class="sxs-lookup"><span data-stu-id="5d563-246">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5d563-247">név</span><span class="sxs-lookup"><span data-stu-id="5d563-247">name</span></span> | <span data-ttu-id="5d563-248">A tevékenység neve.</span><span class="sxs-lookup"><span data-stu-id="5d563-248">Name of the activity</span></span> |<span data-ttu-id="5d563-249">Igen</span><span class="sxs-lookup"><span data-stu-id="5d563-249">Yes</span></span> |
| <span data-ttu-id="5d563-250">Leírás</span><span class="sxs-lookup"><span data-stu-id="5d563-250">description</span></span> |<span data-ttu-id="5d563-251">Mire használható a tevékenységet leíró szöveg</span><span class="sxs-lookup"><span data-stu-id="5d563-251">Text describing what the activity is used for</span></span> |<span data-ttu-id="5d563-252">Nem</span><span class="sxs-lookup"><span data-stu-id="5d563-252">No</span></span> |
| <span data-ttu-id="5d563-253">type</span><span class="sxs-lookup"><span data-stu-id="5d563-253">type</span></span> | <span data-ttu-id="5d563-254">Értékre kell állítani: **SqlServerStoredProcedure**</span><span class="sxs-lookup"><span data-stu-id="5d563-254">Must be set to: **SqlServerStoredProcedure**</span></span> | <span data-ttu-id="5d563-255">Igen</span><span class="sxs-lookup"><span data-stu-id="5d563-255">Yes</span></span> |
| <span data-ttu-id="5d563-256">Bemenetek</span><span class="sxs-lookup"><span data-stu-id="5d563-256">inputs</span></span> | <span data-ttu-id="5d563-257">Választható.</span><span class="sxs-lookup"><span data-stu-id="5d563-257">Optional.</span></span> <span data-ttu-id="5d563-258">Ha megad egy bemeneti adatkészlet, elérhetőnek kell lennie (a "Kész" állapotú) a tárolt eljárás tevékenység futtatásához.</span><span class="sxs-lookup"><span data-stu-id="5d563-258">If you do specify an input dataset, it must be available (in ‘Ready’ status) for the stored procedure activity to run.</span></span> <span data-ttu-id="5d563-259">A bemeneti adatkészletet a tárolt eljárás nem használható paraméterként.</span><span class="sxs-lookup"><span data-stu-id="5d563-259">The input dataset cannot be consumed in the stored procedure as a parameter.</span></span> <span data-ttu-id="5d563-260">Csak a tárolt eljárási tevékenység megkezdése előtt ellenőrizze, a függőség szolgál.</span><span class="sxs-lookup"><span data-stu-id="5d563-260">It is only used to check the dependency before starting the stored procedure activity.</span></span> |<span data-ttu-id="5d563-261">Nem</span><span class="sxs-lookup"><span data-stu-id="5d563-261">No</span></span> |
| <span data-ttu-id="5d563-262">kimenetek</span><span class="sxs-lookup"><span data-stu-id="5d563-262">outputs</span></span> | <span data-ttu-id="5d563-263">Meg kell adnia egy tárolt eljárás tevékenység egy kimeneti adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="5d563-263">You must specify an output dataset for a stored procedure activity.</span></span> <span data-ttu-id="5d563-264">Kimeneti adatkészlet határozza meg a **ütemezés** a tárolt eljárás tevékenység (óránként, heti, havi, stb.).</span><span class="sxs-lookup"><span data-stu-id="5d563-264">Output dataset specifies the **schedule** for the stored procedure activity (hourly, weekly, monthly, etc.).</span></span> <br/><br/><span data-ttu-id="5d563-265">A kimeneti adatkészlet kell használnia egy **társított szolgáltatás** , amely egy Azure SQL Database vagy az Azure SQL Data Warehouse vagy a tárolt eljárás futtatásához használni kívánt SQL Server-adatbázis hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="5d563-265">The output dataset must use a **linked service** that refers to an Azure SQL Database or an Azure SQL Data Warehouse or a SQL Server Database in which you want the stored procedure to run.</span></span> <br/><br/><span data-ttu-id="5d563-266">A kimeneti adatkészlet felelt meg a tárolt eljárás eredményét a későbbi feldolgozásra, amelyet egy másik tevékenység módon működhetnek ([tevékenységek láncolás](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) folyamatban.</span><span class="sxs-lookup"><span data-stu-id="5d563-266">The output dataset can serve as a way to pass the result of the stored procedure for subsequent processing by another activity ([chaining activities](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) in the pipeline.</span></span> <span data-ttu-id="5d563-267">Azonban adat-előállító nem automatikusan kiírhatja a kimenetet tárolt eljárás ehhez a DataSet adatkészlethez.</span><span class="sxs-lookup"><span data-stu-id="5d563-267">However, Data Factory does not automatically write the output of a stored procedure to this dataset.</span></span> <span data-ttu-id="5d563-268">A tárolt eljárás, amely egy SQL táblázat, amely a kimeneti adatkészlet mutat ír.</span><span class="sxs-lookup"><span data-stu-id="5d563-268">It is the stored procedure that writes to a SQL table that the output dataset points to.</span></span> <br/><br/><span data-ttu-id="5d563-269">Bizonyos esetekben a kimeneti adatkészlet lehet egy **dummy dataset**, amellyel csak a tárolt eljárási tevékenység fut ütemezése.</span><span class="sxs-lookup"><span data-stu-id="5d563-269">In some cases, the output dataset can be a **dummy dataset**, which is used only to specify the schedule for running the stored procedure activity.</span></span> |<span data-ttu-id="5d563-270">Igen</span><span class="sxs-lookup"><span data-stu-id="5d563-270">Yes</span></span> |
| <span data-ttu-id="5d563-271">storedProcedureName</span><span class="sxs-lookup"><span data-stu-id="5d563-271">storedProcedureName</span></span> |<span data-ttu-id="5d563-272">Adja meg a tárolt eljárás neve az Azure SQL database vagy az Azure SQL Data Warehouse vagy SQL Server adatbázis, amely a társított szolgáltatás, amely a bemeneti tábla használja.</span><span class="sxs-lookup"><span data-stu-id="5d563-272">Specify the name of the stored procedure in the Azure SQL database or Azure SQL Data Warehouse or SQL Server database that is represented by the linked service that the output table uses.</span></span> |<span data-ttu-id="5d563-273">Igen</span><span class="sxs-lookup"><span data-stu-id="5d563-273">Yes</span></span> |
| <span data-ttu-id="5d563-274">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="5d563-274">storedProcedureParameters</span></span> |<span data-ttu-id="5d563-275">Adja meg a tárolt eljárás paraméter értékét.</span><span class="sxs-lookup"><span data-stu-id="5d563-275">Specify values for stored procedure parameters.</span></span> <span data-ttu-id="5d563-276">Ha az egyik paraméter null értéket átadni van szüksége, használja a szintaxist: "param1": (összes kisbetű) NULL értékű.</span><span class="sxs-lookup"><span data-stu-id="5d563-276">If you need to pass null for a parameter, use the syntax: "param1": null (all lower case).</span></span> <span data-ttu-id="5d563-277">Tekintse meg az alábbi minta tájékozódhat az e tulajdonság használatával.</span><span class="sxs-lookup"><span data-stu-id="5d563-277">See the following sample to learn about using this property.</span></span> |<span data-ttu-id="5d563-278">Nem</span><span class="sxs-lookup"><span data-stu-id="5d563-278">No</span></span> |

## <a name="passing-a-static-value"></a><span data-ttu-id="5d563-279">Egy statikus értékre továbbításához</span><span class="sxs-lookup"><span data-stu-id="5d563-279">Passing a static value</span></span>
<span data-ttu-id="5d563-280">Most tegyük vegyen fel egy másik oszlop, "A forgatókönyv" nevű "Dokumentum minta" nevű statikus értéket tartalmazó tábla.</span><span class="sxs-lookup"><span data-stu-id="5d563-280">Now, let’s consider adding another column named ‘Scenario’ in the table containing a static value called ‘Document sample’.</span></span>

![Mintaadatokat 2](./media/data-factory-stored-proc-activity/sample-data-2.png)

<span data-ttu-id="5d563-282">**Tábla:**</span><span class="sxs-lookup"><span data-stu-id="5d563-282">**Table:**</span></span>

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

<span data-ttu-id="5d563-283">**Tárolt eljárást:**</span><span class="sxs-lookup"><span data-stu-id="5d563-283">**Stored procedure:**</span></span>

```SQL
CREATE PROCEDURE sp_sample2 @DateTime nvarchar(127) , @Scenario nvarchar(127)

AS

BEGIN
    INSERT INTO [sampletable2]
    VALUES (newid(), @DateTime, @Scenario)
END
```

<span data-ttu-id="5d563-284">Most, továbbítja a **forgatókönyv** paraméter és a tárolt eljárási tevékenység közötti értéket.</span><span class="sxs-lookup"><span data-stu-id="5d563-284">Now, pass the **Scenario** parameter and the value from the stored procedure activity.</span></span> <span data-ttu-id="5d563-285">A **typeProperties** szakasz az előző példa néz ki a következő kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="5d563-285">The **typeProperties** section in the preceding sample looks like the following snippet:</span></span>

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

<span data-ttu-id="5d563-286">**Data Factory adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="5d563-286">**Data Factory dataset:**</span></span>

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

<span data-ttu-id="5d563-287">**Data Factory-folyamat**</span><span class="sxs-lookup"><span data-stu-id="5d563-287">**Data Factory pipeline**</span></span>

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