---
title: "Adatok másolása az Azure SQL Database |} Microsoft Docs"
description: "Útmutató: Azure SQL Database szolgáltatáshoz Azure Data Factory és a-adatok másolása."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 484f735b-8464-40ba-a9fc-820e6553159e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: a64d13fa7dc5f50c259b98774be80b603dce400a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="copy-data-to-and-from-azure-sql-database-using-azure-data-factory"></a><span data-ttu-id="ce567-103">Másolja az adatokat, és az Azure SQL adatbázis Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="ce567-103">Copy data to and from Azure SQL Database using Azure Data Factory</span></span>
<span data-ttu-id="ce567-104">Ez a cikk ismerteti, hogyan a másolási tevékenység során az Azure Data Factory és az Azure SQL Database áthelyezni az adatokat.</span><span class="sxs-lookup"><span data-stu-id="ce567-104">This article explains how to use the Copy Activity in Azure Data Factory to move data to and from Azure SQL Database.</span></span> <span data-ttu-id="ce567-105">Buildekről nyújtanak a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, amelynek során adatátvitel a másolási tevékenység az általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="ce567-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>  

## <a name="supported-scenarios"></a><span data-ttu-id="ce567-106">Támogatott helyzetek</span><span class="sxs-lookup"><span data-stu-id="ce567-106">Supported scenarios</span></span>
<span data-ttu-id="ce567-107">Adatokat másolhat **az Azure SQL Database** tárolja a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="ce567-107">You can copy data **from Azure SQL Database** to the following data stores:</span></span>

[!INCLUDE [data-factory-supported-sinks](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="ce567-108">Adatok másolása a következő adatokat tárolja **az Azure SQL Database**:</span><span class="sxs-lookup"><span data-stu-id="ce567-108">You can copy data from the following data stores **to Azure SQL Database**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

## <a name="supported-authentication-type"></a><span data-ttu-id="ce567-109">Támogatott hitelesítési típushoz</span><span class="sxs-lookup"><span data-stu-id="ce567-109">Supported authentication type</span></span>
<span data-ttu-id="ce567-110">Az Azure SQL Database-összekötő az egyszerű hitelesítést támogatja.</span><span class="sxs-lookup"><span data-stu-id="ce567-110">Azure SQL Database connector supports basic authentication.</span></span>

## <a name="getting-started"></a><span data-ttu-id="ce567-111">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="ce567-111">Getting started</span></span>
<span data-ttu-id="ce567-112">A másolási tevékenység, amely helyezi át az adatokat belőle egy Azure SQL Database különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="ce567-112">You can create a pipeline with a copy activity that moves data to/from an Azure SQL Database by using different tools/APIs.</span></span>

<span data-ttu-id="ce567-113">Hozzon létre egy folyamatot a legegyszerűbb módja használatára a **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="ce567-113">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="ce567-114">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) létrehozásával egy folyamatot, az adatok másolása varázsló segítségével gyorsan útmutatást.</span><span class="sxs-lookup"><span data-stu-id="ce567-114">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="ce567-115">Az alábbi eszközöket használhatja a folyamatokat létrehozni: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager sablon**, **.NET API**, és **REST API**.</span><span class="sxs-lookup"><span data-stu-id="ce567-115">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="ce567-116">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) hozzon létre egy folyamatot a másolási tevékenység részletes útmutatóját.</span><span class="sxs-lookup"><span data-stu-id="ce567-116">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="ce567-117">Akár az eszközök vagy API-k, hajtsa végre a következő lépésekkel hozza létre egy folyamatot, amely mozgatja az adatokat a forrás-tárolóban a fogadó tárolóban:</span><span class="sxs-lookup"><span data-stu-id="ce567-117">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span> 

1. <span data-ttu-id="ce567-118">Hozzon létre egy **adat-előállító**.</span><span class="sxs-lookup"><span data-stu-id="ce567-118">Create a **data factory**.</span></span> <span data-ttu-id="ce567-119">Egy adat-előállító tartalmazhat egy vagy több folyamatok.</span><span class="sxs-lookup"><span data-stu-id="ce567-119">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="ce567-120">Hozzon létre **összekapcsolt szolgáltatások** bemeneti és kimeneti adatok csatolásához tárolja a a data factory.</span><span class="sxs-lookup"><span data-stu-id="ce567-120">Create **linked services** to link input and output data stores to your data factory.</span></span> <span data-ttu-id="ce567-121">Például ha a másolt adatok az az Azure blob storage Azure SQL-adatbázishoz, hoz létre az Azure storage-fiók és az Azure SQL adatbázis összekapcsolása a data factory két összekapcsolt szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="ce567-121">For example, if you are copying data from an Azure blob storage to an Azure SQL database, you create two linked services to link your Azure storage account and Azure SQL database to your data factory.</span></span> <span data-ttu-id="ce567-122">Az Azure SQL Database jellemző csatolt szolgáltatás tulajdonságait, lásd: [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="ce567-122">For linked service properties that are specific to Azure SQL Database, see [linked service properties](#linked-service-properties) section.</span></span> 
3. <span data-ttu-id="ce567-123">Hozzon létre **adatkészletek** a másolási művelet bemeneti és kimeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="ce567-123">Create **datasets** to represent input and output data for the copy operation.</span></span> <span data-ttu-id="ce567-124">A példában az előző lépésben említett hozzon létre egy adatkészlet adja meg a blob-tároló és a bemeneti adatokat tartalmazó mappát.</span><span class="sxs-lookup"><span data-stu-id="ce567-124">In the example mentioned in the last step, you create a dataset to specify the blob container and folder that contains the input data.</span></span> <span data-ttu-id="ce567-125">És hoz létre, ha meg szeretné adni az SQL-tábla az Azure SQL-adatbázis, amely tárolja az adatokat a blob-tároló átmásolja egy másik DataSet adatkészletben.</span><span class="sxs-lookup"><span data-stu-id="ce567-125">And, you create another dataset to specify the SQL table in the Azure SQL database  that holds the data copied from the blob storage.</span></span> <span data-ttu-id="ce567-126">Azure Data Lake Store adott adatkészlet tulajdonságai, lásd: [adatkészlet tulajdonságai](#dataset-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="ce567-126">For dataset properties that are specific to Azure Data Lake Store, see [dataset properties](#dataset-properties) section.</span></span>
4. <span data-ttu-id="ce567-127">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="ce567-127">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="ce567-128">A korábban említett példában BlobSource forrás-és SqlSink akár használhatja a fogadó a másolási tevékenységhez.</span><span class="sxs-lookup"><span data-stu-id="ce567-128">In the example mentioned earlier, you use BlobSource as a source and SqlSink as a sink for the copy activity.</span></span> <span data-ttu-id="ce567-129">Ehhez hasonlóan az Azure SQL Database az Azure Blob Storage másolása, használható SqlSource és BlobSink a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="ce567-129">Similarly, if you are copying from Azure SQL Database to Azure Blob Storage, you use SqlSource and BlobSink in the copy activity.</span></span> <span data-ttu-id="ce567-130">Az Azure SQL Database adott tevékenység Tulajdonságok másolása, lásd: [tevékenység Tulajdonságok másolása](#copy-activity-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="ce567-130">For copy activity properties that are specific to Azure SQL Database, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="ce567-131">További részletek a tárolóban használatáról a forrás vagy a fogadó a hivatkozásra a adattároló az előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="ce567-131">For details on how to use a data store as a source or a sink, click the link in the previous section for your data store.</span></span>

<span data-ttu-id="ce567-132">A varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és a feldolgozási sor) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="ce567-132">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="ce567-133">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások a JSON formátum használatával.</span><span class="sxs-lookup"><span data-stu-id="ce567-133">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="ce567-134">Adatok másolása az Azure SQL adatbázis használandó adat-előállító entitások JSON-definíciók minták, lásd: [JSON példák](#json-examples-for-copying-data-to-and-from-sql-database) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="ce567-134">For samples with JSON definitions for Data Factory entities that are used to copy data to/from an Azure SQL Database, see [JSON examples](#json-examples-for-copying-data-to-and-from-sql-database) section of this article.</span></span> 

<span data-ttu-id="ce567-135">A következő szakaszok részletesen bemutatják az Azure SQL Database Data Factory tartozó entitások meghatározásához használt JSON tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="ce567-135">The following sections provide details about JSON properties that are used to define Data Factory entities specific to Azure SQL Database:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="ce567-136">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="ce567-136">Linked service properties</span></span>
<span data-ttu-id="ce567-137">Egy Azure SQL Azure SQL-adatbázis kapcsolódó a szolgáltatás hivatkozások a data factory.</span><span class="sxs-lookup"><span data-stu-id="ce567-137">An Azure SQL linked service links an Azure SQL database to your data factory.</span></span> <span data-ttu-id="ce567-138">Az alábbi táblázatban az adott Azure SQL társított szolgáltatásnak JSON-elemek leírását.</span><span class="sxs-lookup"><span data-stu-id="ce567-138">The following table provides description for JSON elements specific to Azure SQL linked service.</span></span>

| <span data-ttu-id="ce567-139">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="ce567-139">Property</span></span> | <span data-ttu-id="ce567-140">Leírás</span><span class="sxs-lookup"><span data-stu-id="ce567-140">Description</span></span> | <span data-ttu-id="ce567-141">Szükséges</span><span class="sxs-lookup"><span data-stu-id="ce567-141">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ce567-142">type</span><span class="sxs-lookup"><span data-stu-id="ce567-142">type</span></span> |<span data-ttu-id="ce567-143">A type tulajdonságot kell beállítani: **AzureSqlDatabase**</span><span class="sxs-lookup"><span data-stu-id="ce567-143">The type property must be set to: **AzureSqlDatabase**</span></span> |<span data-ttu-id="ce567-144">Igen</span><span class="sxs-lookup"><span data-stu-id="ce567-144">Yes</span></span> |
| <span data-ttu-id="ce567-145">connectionString</span><span class="sxs-lookup"><span data-stu-id="ce567-145">connectionString</span></span> |<span data-ttu-id="ce567-146">Adja meg az Azure SQL Database-példányt a connectionString tulajdonság való kapcsolódáshoz szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="ce567-146">Specify information needed to connect to the Azure SQL Database instance for the connectionString property.</span></span> <span data-ttu-id="ce567-147">Csak az alapszintű hitelesítést is támogatja.</span><span class="sxs-lookup"><span data-stu-id="ce567-147">Only basic authentication is supported.</span></span> |<span data-ttu-id="ce567-148">Igen</span><span class="sxs-lookup"><span data-stu-id="ce567-148">Yes</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="ce567-149">Konfigurálása [Azure SQL Database-tűzfal](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) az adatbázis-kiszolgálót [a kiszolgálóhoz való hozzáféréshez Azure-szolgáltatások engedélyezése](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure).</span><span class="sxs-lookup"><span data-stu-id="ce567-149">Configure [Azure SQL Database Firewall](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) the database server to [allow Azure Services to access the server](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure).</span></span> <span data-ttu-id="ce567-150">Ezenkívül az adatokat az Azure SQL Database a külső Azure többek között a helyszíni adatforrásokból a data factory átjáróval másolása, az IP-címtartományt a gép, amely adatokat küld az Azure SQL Database konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="ce567-150">Additionally, if you are copying data to Azure SQL Database from outside Azure including from on-premises data sources with data factory gateway, configure appropriate IP address range for the machine that is sending data to Azure SQL Database.</span></span>

## <a name="dataset-properties"></a><span data-ttu-id="ce567-151">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="ce567-151">Dataset properties</span></span>
<span data-ttu-id="ce567-152">Adja meg a bemeneti vagy kimeneti adatok Azure SQL adatbázis dataset, állítsa be a adatkészlet type tulajdonsága: **AzureSqlTable**.</span><span class="sxs-lookup"><span data-stu-id="ce567-152">To specify a dataset to represent input or output data in an Azure SQL database, you set the type property of the dataset to: **AzureSqlTable**.</span></span> <span data-ttu-id="ce567-153">Állítsa be a **linkedServiceName** tulajdonsághoz a DataSet adatkészlet neve az Azure SQL társított szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="ce567-153">Set the **linkedServiceName** property of the dataset to the name of the Azure SQL linked service.</span></span>  

<span data-ttu-id="ce567-154">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: a [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="ce567-154">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="ce567-155">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="ce567-155">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="ce567-156">A typeProperties szakasz más adatkészlet egyes típusai és információkat nyújt azokról az adattárban adatok helyét.</span><span class="sxs-lookup"><span data-stu-id="ce567-156">The typeProperties section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="ce567-157">A **typeProperties** szakasz az adatkészlet típusú **AzureSqlTable** tulajdonságai a következők:</span><span class="sxs-lookup"><span data-stu-id="ce567-157">The **typeProperties** section for the dataset of type **AzureSqlTable** has the following properties:</span></span>

| <span data-ttu-id="ce567-158">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="ce567-158">Property</span></span> | <span data-ttu-id="ce567-159">Leírás</span><span class="sxs-lookup"><span data-stu-id="ce567-159">Description</span></span> | <span data-ttu-id="ce567-160">Szükséges</span><span class="sxs-lookup"><span data-stu-id="ce567-160">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ce567-161">tableName</span><span class="sxs-lookup"><span data-stu-id="ce567-161">tableName</span></span> |<span data-ttu-id="ce567-162">A tábla vagy nézet társított szolgáltatásnak Azure SQL Database-példány neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="ce567-162">Name of the table or view in the Azure SQL Database instance that linked service refers to.</span></span> |<span data-ttu-id="ce567-163">Igen</span><span class="sxs-lookup"><span data-stu-id="ce567-163">Yes</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="ce567-164">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="ce567-164">Copy activity properties</span></span>
<span data-ttu-id="ce567-165">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: a [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="ce567-165">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="ce567-166">Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="ce567-166">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

> [!NOTE]
> <span data-ttu-id="ce567-167">A másolási tevékenység során csak egy bemenettel rendelkezik, és csak egy kimenetet.</span><span class="sxs-lookup"><span data-stu-id="ce567-167">The Copy Activity takes only one input and produces only one output.</span></span>

<span data-ttu-id="ce567-168">Mivel a tulajdonságok érhetők el a **typeProperties** szakasz a tevékenység tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="ce567-168">Whereas, properties available in the **typeProperties** section of the activity vary with each activity type.</span></span> <span data-ttu-id="ce567-169">A másolási tevékenység során két érték források és mosdók típusától függően.</span><span class="sxs-lookup"><span data-stu-id="ce567-169">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="ce567-170">Ha adatokat az Azure SQL-adatbázis, a másolási tevékenység beállítása a forrástípus **SqlSource**.</span><span class="sxs-lookup"><span data-stu-id="ce567-170">If you are moving data from an Azure SQL database, you set the source type in the copy activity to **SqlSource**.</span></span> <span data-ttu-id="ce567-171">Hasonlóképpen, ha adatait Azure SQL-adatbázishoz, beállítása a fogadó típusa a másolási tevékenység **SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="ce567-171">Similarly, if you are moving data to an Azure SQL database, you set the sink type in the copy activity to **SqlSink**.</span></span> <span data-ttu-id="ce567-172">Ez a témakör SqlSource és SqlSink által támogatott tulajdonságokról.</span><span class="sxs-lookup"><span data-stu-id="ce567-172">This section provides a list of properties supported by SqlSource and SqlSink.</span></span>

### <a name="sqlsource"></a><span data-ttu-id="ce567-173">SqlSource</span><span class="sxs-lookup"><span data-stu-id="ce567-173">SqlSource</span></span>
<span data-ttu-id="ce567-174">A másolási tevékenység, ha az adatforrás típusú **SqlSource**, a következő tulajdonságok érhetők el **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="ce567-174">In copy activity, when the source is of type **SqlSource**, the following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="ce567-175">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="ce567-175">Property</span></span> | <span data-ttu-id="ce567-176">Leírás</span><span class="sxs-lookup"><span data-stu-id="ce567-176">Description</span></span> | <span data-ttu-id="ce567-177">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="ce567-177">Allowed values</span></span> | <span data-ttu-id="ce567-178">Szükséges</span><span class="sxs-lookup"><span data-stu-id="ce567-178">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="ce567-179">sqlReaderQuery</span><span class="sxs-lookup"><span data-stu-id="ce567-179">sqlReaderQuery</span></span> |<span data-ttu-id="ce567-180">Az egyéni lekérdezés segítségével adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="ce567-180">Use the custom query to read data.</span></span> |<span data-ttu-id="ce567-181">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="ce567-181">SQL query string.</span></span> <span data-ttu-id="ce567-182">Példa: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="ce567-182">Example: `select * from MyTable`.</span></span> |<span data-ttu-id="ce567-183">Nem</span><span class="sxs-lookup"><span data-stu-id="ce567-183">No</span></span> |
| <span data-ttu-id="ce567-184">sqlReaderStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="ce567-184">sqlReaderStoredProcedureName</span></span> |<span data-ttu-id="ce567-185">A tárolt eljárás, amely adatokat olvas a forrástábla neve.</span><span class="sxs-lookup"><span data-stu-id="ce567-185">Name of the stored procedure that reads data from the source table.</span></span> |<span data-ttu-id="ce567-186">A tárolt eljárás neve.</span><span class="sxs-lookup"><span data-stu-id="ce567-186">Name of the stored procedure.</span></span> <span data-ttu-id="ce567-187">Az utolsó SQL-utasítás a következő tárolt eljárást a SELECT utasítással kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ce567-187">The last SQL statement must be a SELECT statement in the stored procedure.</span></span> |<span data-ttu-id="ce567-188">Nem</span><span class="sxs-lookup"><span data-stu-id="ce567-188">No</span></span> |
| <span data-ttu-id="ce567-189">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="ce567-189">storedProcedureParameters</span></span> |<span data-ttu-id="ce567-190">A tárolt eljárás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="ce567-190">Parameters for the stored procedure.</span></span> |<span data-ttu-id="ce567-191">A név/érték párok.</span><span class="sxs-lookup"><span data-stu-id="ce567-191">Name/value pairs.</span></span> <span data-ttu-id="ce567-192">Nevét és a kis-és a paraméterek meg kell egyeznie a nevek és a kis-és nagybetűhasználat a tárolt eljárás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="ce567-192">Names and casing of parameters must match the names and casing of the stored procedure parameters.</span></span> |<span data-ttu-id="ce567-193">Nem</span><span class="sxs-lookup"><span data-stu-id="ce567-193">No</span></span> |

<span data-ttu-id="ce567-194">Ha a **sqlReaderQuery** van megadva a SqlSource, a másolási tevékenység fut ez a lekérdezés az adatok lekérdezése az Azure SQL Database forrása.</span><span class="sxs-lookup"><span data-stu-id="ce567-194">If the **sqlReaderQuery** is specified for the SqlSource, the Copy Activity runs this query against the Azure SQL Database source to get the data.</span></span> <span data-ttu-id="ce567-195">Másik lehetőségként megadhat tárolt eljárás megadásával a **sqlReaderStoredProcedureName** és **storedProcedureParameters** (Ha a tárolt eljárás paraméterek fogadja el).</span><span class="sxs-lookup"><span data-stu-id="ce567-195">Alternatively, you can specify a stored procedure by specifying the **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if the stored procedure takes parameters).</span></span>

<span data-ttu-id="ce567-196">Ha nem ad meg sqlReaderQuery vagy sqlReaderStoredProcedureName, az adatkészlet JSON struktúrában szakaszában meghatározott oszlopokat-lekérdezés összeállításához használt (`select column1, column2 from mytable`) az Azure SQL-adatbázis futtatásához.</span><span class="sxs-lookup"><span data-stu-id="ce567-196">If you do not specify either sqlReaderQuery or sqlReaderStoredProcedureName, the columns defined in the structure section of the dataset JSON are used to build a query (`select column1, column2 from mytable`) to run against the Azure SQL Database.</span></span> <span data-ttu-id="ce567-197">Az adatkészlet-definícióban nem rendelkezik a struktúra, ha minden kiválasztott oszlop. a táblából.</span><span class="sxs-lookup"><span data-stu-id="ce567-197">If the dataset definition does not have the structure, all columns are selected from the table.</span></span>

> [!NOTE]
> <span data-ttu-id="ce567-198">Amikor **sqlReaderStoredProcedureName**, továbbra is meg kell adnia egy értéket a **tableName** az adatkészlet JSON tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="ce567-198">When you use **sqlReaderStoredProcedureName**, you still need to specify a value for the **tableName** property in the dataset JSON.</span></span> <span data-ttu-id="ce567-199">Nincs érvényesítést hajt végre ezt a táblázatot, ha van.</span><span class="sxs-lookup"><span data-stu-id="ce567-199">There are no validations performed against this table though.</span></span>
>
>

### <a name="sqlsource-example"></a><span data-ttu-id="ce567-200">SqlSource – példa</span><span class="sxs-lookup"><span data-stu-id="ce567-200">SqlSource example</span></span>

```JSON
"source": {
    "type": "SqlSource",
    "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
    "storedProcedureParameters": {
        "stringData": { "value": "str3" },
        "identifier": { "value": "$$Text.Format('{0:yyyy}', SliceStart)", "type": "Int"}
    }
}
```

<span data-ttu-id="ce567-201">**A tárolt eljárás definíciója:**</span><span class="sxs-lookup"><span data-stu-id="ce567-201">**The stored procedure definition:**</span></span>

```SQL
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
     select *
     from dbo.UnitTestSrcTable
     where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="sqlsink"></a><span data-ttu-id="ce567-202">SqlSink</span><span class="sxs-lookup"><span data-stu-id="ce567-202">SqlSink</span></span>
<span data-ttu-id="ce567-203">**SqlSink** támogatja a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="ce567-203">**SqlSink** supports the following properties:</span></span>

| <span data-ttu-id="ce567-204">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="ce567-204">Property</span></span> | <span data-ttu-id="ce567-205">Leírás</span><span class="sxs-lookup"><span data-stu-id="ce567-205">Description</span></span> | <span data-ttu-id="ce567-206">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="ce567-206">Allowed values</span></span> | <span data-ttu-id="ce567-207">Szükséges</span><span class="sxs-lookup"><span data-stu-id="ce567-207">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="ce567-208">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="ce567-208">writeBatchTimeout</span></span> |<span data-ttu-id="ce567-209">Várakozási idő a kötegelt beszúrási művelet befejezését, mielőtt azt az időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="ce567-209">Wait time for the batch insert operation to complete before it times out.</span></span> |<span data-ttu-id="ce567-210">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="ce567-210">timespan</span></span><br/><br/> <span data-ttu-id="ce567-211">Példa: "00: 30:00" (30 perc).</span><span class="sxs-lookup"><span data-stu-id="ce567-211">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="ce567-212">Nem</span><span class="sxs-lookup"><span data-stu-id="ce567-212">No</span></span> |
| <span data-ttu-id="ce567-213">WriteBatchSize</span><span class="sxs-lookup"><span data-stu-id="ce567-213">writeBatchSize</span></span> |<span data-ttu-id="ce567-214">Szúr be az SQL-tábla adatokat, amikor a puffer mérete eléri writeBatchSize.</span><span class="sxs-lookup"><span data-stu-id="ce567-214">Inserts data into the SQL table when the buffer size reaches writeBatchSize.</span></span> |<span data-ttu-id="ce567-215">Egész szám (sorok száma)</span><span class="sxs-lookup"><span data-stu-id="ce567-215">Integer (number of rows)</span></span> |<span data-ttu-id="ce567-216">Nem (alapértelmezett: 10000)</span><span class="sxs-lookup"><span data-stu-id="ce567-216">No (default: 10000)</span></span> |
| <span data-ttu-id="ce567-217">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="ce567-217">sqlWriterCleanupScript</span></span> |<span data-ttu-id="ce567-218">Adja meg egy lekérdezést a másolási tevékenység végrehajtása úgy, hogy egy adott szelet adatait.</span><span class="sxs-lookup"><span data-stu-id="ce567-218">Specify a query for Copy Activity to execute such that data of a specific slice is cleaned up.</span></span> <span data-ttu-id="ce567-219">További információkért lásd: [ismételhető másolási](#repeatable-copy).</span><span class="sxs-lookup"><span data-stu-id="ce567-219">For more information, see [repeatable copy](#repeatable-copy).</span></span> |<span data-ttu-id="ce567-220">A lekérdezési utasítást.</span><span class="sxs-lookup"><span data-stu-id="ce567-220">A query statement.</span></span> |<span data-ttu-id="ce567-221">Nem</span><span class="sxs-lookup"><span data-stu-id="ce567-221">No</span></span> |
| <span data-ttu-id="ce567-222">sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="ce567-222">sliceIdentifierColumnName</span></span> |<span data-ttu-id="ce567-223">Adja meg a másolási tevékenység során automatikusan létrejön szelet azonosítóval, amely távolítja el az adatokat egy adott szelet, amikor futtassa újra a kitöltésének oszlop nevét.</span><span class="sxs-lookup"><span data-stu-id="ce567-223">Specify a column name for Copy Activity to fill with auto generated slice identifier, which is used to clean up data of a specific slice when rerun.</span></span> <span data-ttu-id="ce567-224">További információkért lásd: [ismételhető másolási](#repeatable-copy).</span><span class="sxs-lookup"><span data-stu-id="ce567-224">For more information, see [repeatable copy](#repeatable-copy).</span></span> |<span data-ttu-id="ce567-225">Egy oszlop binary(32) adattípusú oszlop neve.</span><span class="sxs-lookup"><span data-stu-id="ce567-225">Column name of a column with data type of binary(32).</span></span> |<span data-ttu-id="ce567-226">Nem</span><span class="sxs-lookup"><span data-stu-id="ce567-226">No</span></span> |
| <span data-ttu-id="ce567-227">sqlWriterStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="ce567-227">sqlWriterStoredProcedureName</span></span> |<span data-ttu-id="ce567-228">A tárolt eljárás neve a cél táblázatba upserts (frissítés/Beszúrás) adatok.</span><span class="sxs-lookup"><span data-stu-id="ce567-228">Name of the stored procedure that upserts (updates/inserts) data into the target table.</span></span> |<span data-ttu-id="ce567-229">A tárolt eljárás neve.</span><span class="sxs-lookup"><span data-stu-id="ce567-229">Name of the stored procedure.</span></span> |<span data-ttu-id="ce567-230">Nem</span><span class="sxs-lookup"><span data-stu-id="ce567-230">No</span></span> |
| <span data-ttu-id="ce567-231">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="ce567-231">storedProcedureParameters</span></span> |<span data-ttu-id="ce567-232">A tárolt eljárás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="ce567-232">Parameters for the stored procedure.</span></span> |<span data-ttu-id="ce567-233">A név/érték párok.</span><span class="sxs-lookup"><span data-stu-id="ce567-233">Name/value pairs.</span></span> <span data-ttu-id="ce567-234">Nevét és a kis-és a paraméterek meg kell egyeznie a nevek és a kis-és nagybetűhasználat a tárolt eljárás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="ce567-234">Names and casing of parameters must match the names and casing of the stored procedure parameters.</span></span> |<span data-ttu-id="ce567-235">Nem</span><span class="sxs-lookup"><span data-stu-id="ce567-235">No</span></span> |
| <span data-ttu-id="ce567-236">sqlWriterTableType</span><span class="sxs-lookup"><span data-stu-id="ce567-236">sqlWriterTableType</span></span> |<span data-ttu-id="ce567-237">Adjon meg egy tábla típus a következő tárolt eljárás használható.</span><span class="sxs-lookup"><span data-stu-id="ce567-237">Specify a table type name to be used in the stored procedure.</span></span> <span data-ttu-id="ce567-238">Másolási tevékenység elérhetővé teszi az adatok áthelyezése egy ideiglenes táblát, amely a táblatípus.</span><span class="sxs-lookup"><span data-stu-id="ce567-238">Copy activity makes the data being moved available in a temp table with this table type.</span></span> <span data-ttu-id="ce567-239">Tárolt eljárás kódot is majd egyesítheti az adatokat, a meglévő adatok másolásának.</span><span class="sxs-lookup"><span data-stu-id="ce567-239">Stored procedure code can then merge the data being copied with existing data.</span></span> |<span data-ttu-id="ce567-240">Egy tábla környezettípus nevét.</span><span class="sxs-lookup"><span data-stu-id="ce567-240">A table type name.</span></span> |<span data-ttu-id="ce567-241">Nem</span><span class="sxs-lookup"><span data-stu-id="ce567-241">No</span></span> |

#### <a name="sqlsink-example"></a><span data-ttu-id="ce567-242">SqlSink – példa</span><span class="sxs-lookup"><span data-stu-id="ce567-242">SqlSink example</span></span>

```JSON
"sink": {
    "type": "SqlSink",
    "writeBatchSize": 1000000,
    "writeBatchTimeout": "00:05:00",
    "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
    "sqlWriterTableType": "CopyTestTableType",
    "storedProcedureParameters": {
        "identifier": { "value": "1", "type": "Int" },
        "stringData": { "value": "str1" },
        "decimalData": { "value": "1", "type": "Decimal" }
    }
}
```

## <a name="json-examples-for-copying-data-to-and-from-sql-database"></a><span data-ttu-id="ce567-243">Adatok másolása és az SQL-adatbázis a JSON példák</span><span class="sxs-lookup"><span data-stu-id="ce567-243">JSON examples for copying data to and from SQL Database</span></span>
<span data-ttu-id="ce567-244">Az alábbi példák megadják minta JSON-definíciókat tartalmazzon, segítségével hozzon létre egy folyamatot [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="ce567-244">The following examples provide sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="ce567-245">Adatok másolása az Azure SQL Database és az Azure Blob Storage mutatnak.</span><span class="sxs-lookup"><span data-stu-id="ce567-245">They show how to copy data to and from Azure SQL Database and Azure Blob Storage.</span></span> <span data-ttu-id="ce567-246">Azonban az adatok átmásolhatók **közvetlenül** a forrásokban, sem a megadott nyelő [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) a másolási tevékenység során az Azure Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="ce567-246">However, data can be copied **directly** from any of sources to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>

### <a name="example-copy-data-from-azure-sql-database-to-azure-blob"></a><span data-ttu-id="ce567-247">Példa: Adatok másolása az Azure SQL Database az Azure-Blobba</span><span class="sxs-lookup"><span data-stu-id="ce567-247">Example: Copy data from Azure SQL Database to Azure Blob</span></span>
<span data-ttu-id="ce567-248">A következő adat-előállító entitások azonos meghatározása:</span><span class="sxs-lookup"><span data-stu-id="ce567-248">The same defines the following Data Factory entities:</span></span>

1. <span data-ttu-id="ce567-249">A társított szolgáltatás típusa [AzureSqlDatabase](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="ce567-249">A linked service of type [AzureSqlDatabase](#linked-service-properties).</span></span>
2. <span data-ttu-id="ce567-250">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="ce567-250">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="ce567-251">Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureSqlTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="ce567-251">An input [dataset](data-factory-create-datasets.md) of type [AzureSqlTable](#dataset-properties).</span></span>
4. <span data-ttu-id="ce567-252">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [Azure Blob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="ce567-252">An output [dataset](data-factory-create-datasets.md) of type [Azure Blob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="ce567-253">A [csővezeték](data-factory-create-pipelines.md) , a másolási tevékenység által használt [SqlSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="ce567-253">A [pipeline](data-factory-create-pipelines.md) with a Copy activity that uses [SqlSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="ce567-254">A minta idősorozat adatainak másolása (óránként, naponta, stb.) az Azure SQL-adatbázis egy táblából egy blobba óránként.</span><span class="sxs-lookup"><span data-stu-id="ce567-254">The sample copies time-series data (hourly, daily, etc.) from a table in Azure SQL database to a blob every hour.</span></span> <span data-ttu-id="ce567-255">A mintákat a következő szakaszok ismertetik ezeket a mintákat használt JSON-tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="ce567-255">The JSON properties used in these samples are described in sections following the samples.</span></span>  

<span data-ttu-id="ce567-256">**A társított szolgáltatásnak Azure SQL Database:**</span><span class="sxs-lookup"><span data-stu-id="ce567-256">**Azure SQL Database linked service:**</span></span>

```JSON
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
<span data-ttu-id="ce567-257">Tekintse meg a [Azure SQL társított szolgáltatást](#linked-service) szolgáltatásnak által támogatott tulajdonságokról szakasza listája.</span><span class="sxs-lookup"><span data-stu-id="ce567-257">See the [Azure SQL Linked Service](#linked-service) section for the list of properties supported by this linked service.</span></span>

<span data-ttu-id="ce567-258">**Az Azure Blob storage társított szolgáltatásnak:**</span><span class="sxs-lookup"><span data-stu-id="ce567-258">**Azure Blob storage linked service:**</span></span>

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
<span data-ttu-id="ce567-259">Tekintse meg a [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) cikkében listája a társított szolgáltatás által támogatott tulajdonságokról.</span><span class="sxs-lookup"><span data-stu-id="ce567-259">See the [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) article for the list of properties supported by this linked service.</span></span>


<span data-ttu-id="ce567-260">**Az Azure SQL bemeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="ce567-260">**Azure SQL input dataset:**</span></span>

<span data-ttu-id="ce567-261">A minta azt feltételezi, hogy létrehozott egy tábla "MyTable" Azure SQL, egy "timestampcolumn" nevű adatsorozat időadatok oszlopot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="ce567-261">The sample assumes you have created a table “MyTable” in Azure SQL and it contains a column called “timestampcolumn” for time series data.</span></span>

<span data-ttu-id="ce567-262">"External" beállítása: "true" arról értesíti az Azure Data Factory szolgáltatásnak, hogy az adatkészlet data factoryval való külső, és egy tevékenység adat-előállító nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="ce567-262">Setting “external”: ”true” informs the Azure Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

```JSON
{
  "name": "AzureSqlInput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
    "typeProperties": {
      "tableName": "MyTable"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```

<span data-ttu-id="ce567-263">Tekintse meg a [Azure SQL dataset típustulajdonságokat](#dataset) ehhez az adathalmaztípushoz által támogatott tulajdonságokról szakasza listája.</span><span class="sxs-lookup"><span data-stu-id="ce567-263">See the [Azure SQL dataset type properties](#dataset) section for the list of properties supported by this dataset type.</span></span>  

<span data-ttu-id="ce567-264">**Az Azure Blob kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="ce567-264">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="ce567-265">Adatot ír egy új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="ce567-265">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="ce567-266">A mappa elérési útját a BLOB a szelet által feldolgozott kezdési ideje alapján dinamikusan történik.</span><span class="sxs-lookup"><span data-stu-id="ce567-266">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="ce567-267">A mappa elérési útját használja, év, hónap, nap és a kezdési idő órában részeit.</span><span class="sxs-lookup"><span data-stu-id="ce567-267">The folder path uses year, month, day, and hours parts of the start time.</span></span>

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}/",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "\t",
        "rowDelimiter": "\n"
      }
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
<span data-ttu-id="ce567-268">Tekintse meg a [Azure-Blob adatkészletet típustulajdonságokat](data-factory-azure-blob-connector.md#dataset-properties) ehhez az adathalmaztípushoz által támogatott tulajdonságokról szakasza listája.</span><span class="sxs-lookup"><span data-stu-id="ce567-268">See the [Azure Blob dataset type properties](data-factory-azure-blob-connector.md#dataset-properties) section for the list of properties supported by this dataset type.</span></span>  

<span data-ttu-id="ce567-269">**A másolási tevékenység során az SQL-forrás és fogadó Blob egy folyamaton belül:**</span><span class="sxs-lookup"><span data-stu-id="ce567-269">**A copy activity in a pipeline with SQL source and Blob sink:**</span></span>

<span data-ttu-id="ce567-270">A feldolgozási sor tartalmazza a másolási tevékenység, amely a bemeneti és kimeneti adatkészletek használatára van konfigurálva, és óránkénti futásra nem ütemezték.</span><span class="sxs-lookup"><span data-stu-id="ce567-270">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="ce567-271">Az adatcsatorna JSON-definícióból a **forrás** típusúra **SqlSource** és **fogadó** típusúra **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="ce567-271">In the pipeline JSON definition, the **source** type is set to **SqlSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="ce567-272">A megadott SQL-lekérdezést a **SqlReaderQuery** tulajdonság kiválasztása az adatok másolása az elmúlt órában.</span><span class="sxs-lookup"><span data-stu-id="ce567-272">The SQL query specified for the **SqlReaderQuery** property selects the data in the past hour to copy.</span></span>

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "AzureSQLtoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureSQLInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
     ]
   }
}
```
<span data-ttu-id="ce567-273">A példában **sqlReaderQuery** a SqlSource van megadva.</span><span class="sxs-lookup"><span data-stu-id="ce567-273">In the example, **sqlReaderQuery** is specified for the SqlSource.</span></span> <span data-ttu-id="ce567-274">A másolási tevékenység fut ez a lekérdezés az adatok lekérdezése az Azure SQL Database forrása.</span><span class="sxs-lookup"><span data-stu-id="ce567-274">The Copy Activity runs this query against the Azure SQL Database source to get the data.</span></span> <span data-ttu-id="ce567-275">Másik lehetőségként megadhat tárolt eljárás megadásával a **sqlReaderStoredProcedureName** és **storedProcedureParameters** (Ha a tárolt eljárás paraméterek fogadja el).</span><span class="sxs-lookup"><span data-stu-id="ce567-275">Alternatively, you can specify a stored procedure by specifying the **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if the stored procedure takes parameters).</span></span>

<span data-ttu-id="ce567-276">Ha nem ad meg sqlReaderQuery vagy sqlReaderStoredProcedureName, az adatkészlet JSON struktúrában szakaszában meghatározott oszlopokat futtatni az Azure SQL adatbázis-lekérdezés összeállításához használt.</span><span class="sxs-lookup"><span data-stu-id="ce567-276">If you do not specify either sqlReaderQuery or sqlReaderStoredProcedureName, the columns defined in the structure section of the dataset JSON are used to build a query to run against the Azure SQL Database.</span></span> <span data-ttu-id="ce567-277">Például: `select column1, column2 from mytable`.</span><span class="sxs-lookup"><span data-stu-id="ce567-277">For example: `select column1, column2 from mytable`.</span></span> <span data-ttu-id="ce567-278">Az adatkészlet-definícióban nem rendelkezik a struktúra, ha minden kiválasztott oszlop. a táblából.</span><span class="sxs-lookup"><span data-stu-id="ce567-278">If the dataset definition does not have the structure, all columns are selected from the table.</span></span>

<span data-ttu-id="ce567-279">Tekintse meg a [Sql-forrás](#sqlsource) szakasz és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) SqlSource és BlobSink által támogatott tulajdonságok listája.</span><span class="sxs-lookup"><span data-stu-id="ce567-279">See the [Sql Source](#sqlsource) section and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) for the list of properties supported by SqlSource and BlobSink.</span></span>

### <a name="example-copy-data-from-azure-blob-to-azure-sql-database"></a><span data-ttu-id="ce567-280">Példa: Adatok másolása az Azure Blob az Azure SQL adatbázis</span><span class="sxs-lookup"><span data-stu-id="ce567-280">Example: Copy data from Azure Blob to Azure SQL Database</span></span>
<span data-ttu-id="ce567-281">A minta a következő adat-előállító entitások határozza meg:</span><span class="sxs-lookup"><span data-stu-id="ce567-281">The sample defines the following Data Factory entities:</span></span>  

1. <span data-ttu-id="ce567-282">A társított szolgáltatás típusa [AzureSqlDatabase](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="ce567-282">A linked service of type [AzureSqlDatabase](#linked-service-properties).</span></span>
2. <span data-ttu-id="ce567-283">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="ce567-283">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="ce567-284">Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="ce567-284">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="ce567-285">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureSqlTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="ce567-285">An output [dataset](data-factory-create-datasets.md) of type [AzureSqlTable](#dataset-properties).</span></span>
5. <span data-ttu-id="ce567-286">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) és [SqlSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="ce567-286">A [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) and [SqlSink](#copy-activity-properties).</span></span>

<span data-ttu-id="ce567-287">A minta másolatok idősorozat adatok (óránként, naponta, stb.) az Azure blob-egy táblához, az Azure SQL adatbázis-óránként.</span><span class="sxs-lookup"><span data-stu-id="ce567-287">The sample copies time-series data (hourly, daily, etc.) from Azure blob to a table in Azure SQL database every hour.</span></span> <span data-ttu-id="ce567-288">A mintákat a következő szakaszok ismertetik ezeket a mintákat használt JSON-tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="ce567-288">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="ce567-289">**Az Azure SQL társított szolgáltatásnak:**</span><span class="sxs-lookup"><span data-stu-id="ce567-289">**Azure SQL linked service:**</span></span>

```JSON
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
<span data-ttu-id="ce567-290">Tekintse meg a [Azure SQL társított szolgáltatást](#linked-service) szolgáltatásnak által támogatott tulajdonságokról szakasza listája.</span><span class="sxs-lookup"><span data-stu-id="ce567-290">See the [Azure SQL Linked Service](#linked-service) section for the list of properties supported by this linked service.</span></span>

<span data-ttu-id="ce567-291">**Az Azure Blob storage társított szolgáltatásnak:**</span><span class="sxs-lookup"><span data-stu-id="ce567-291">**Azure Blob storage linked service:**</span></span>

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
<span data-ttu-id="ce567-292">Tekintse meg a [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) cikkében listája a társított szolgáltatás által támogatott tulajdonságokról.</span><span class="sxs-lookup"><span data-stu-id="ce567-292">See the [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) article for the list of properties supported by this linked service.</span></span>


<span data-ttu-id="ce567-293">**Az Azure Blob bemeneti adatkészletet:**</span><span class="sxs-lookup"><span data-stu-id="ce567-293">**Azure Blob input dataset:**</span></span>

<span data-ttu-id="ce567-294">Adatok van felvett egy új blobból minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="ce567-294">Data is picked up from a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="ce567-295">A mappa elérési útját és nevét a BLOB dinamikusan értékeli ki a kezdési időt a szelet által feldolgozott alapján.</span><span class="sxs-lookup"><span data-stu-id="ce567-295">The folder path and file name for the blob are dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="ce567-296">A mappa elérési útját használja év, hónap és nap részét kezdési idejét, valamint fájl nevét a kezdő időpontja óra részét.</span><span class="sxs-lookup"><span data-stu-id="ce567-296">The folder path uses year, month, and day part of the start time and file name uses the hour part of the start time.</span></span> <span data-ttu-id="ce567-297">"external": "true" beállítás arról értesíti az, hogy ezt a táblázatot az adat-előállítóban külső, és egy tevékenység adat-előállító nem hozzák a Data Factory szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="ce567-297">“external”: “true” setting informs the Data Factory service that this table is external to the data factory and is not produced by an activity in the data factory.</span></span>

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/",
      "fileName": "{Hour}.csv",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": "\n"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```
<span data-ttu-id="ce567-298">Tekintse meg a [Azure-Blob adatkészletet típustulajdonságokat](data-factory-azure-blob-connector.md#dataset-properties) ehhez az adathalmaztípushoz által támogatott tulajdonságokról szakasza listája.</span><span class="sxs-lookup"><span data-stu-id="ce567-298">See the [Azure Blob dataset type properties](data-factory-azure-blob-connector.md#dataset-properties) section for the list of properties supported by this dataset type.</span></span>

<span data-ttu-id="ce567-299">**Az Azure SQL Database kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="ce567-299">**Azure SQL Database output dataset:**</span></span>

<span data-ttu-id="ce567-300">A minta másolja az adatokat az Azure SQL "MyTable" nevű tábla.</span><span class="sxs-lookup"><span data-stu-id="ce567-300">The sample copies data to a table named “MyTable” in Azure SQL.</span></span> <span data-ttu-id="ce567-301">A tábla létrehozása az azonos számú oszlopot az Azure SQL-ben a Blob CSV-fájl tartalmazza a várt módon.</span><span class="sxs-lookup"><span data-stu-id="ce567-301">Create the table in Azure SQL with the same number of columns as you expect the Blob CSV file to contain.</span></span> <span data-ttu-id="ce567-302">Új sorok hozzáadásakor a tábla minden órában.</span><span class="sxs-lookup"><span data-stu-id="ce567-302">New rows are added to the table every hour.</span></span>

```JSON
{
  "name": "AzureSqlOutput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
    "typeProperties": {
      "tableName": "MyOutputTable"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
<span data-ttu-id="ce567-303">Tekintse meg a [Azure SQL dataset típustulajdonságokat](#dataset) ehhez az adathalmaztípushoz által támogatott tulajdonságokról szakasza listája.</span><span class="sxs-lookup"><span data-stu-id="ce567-303">See the [Azure SQL dataset type properties](#dataset) section for the list of properties supported by this dataset type.</span></span>

<span data-ttu-id="ce567-304">**A másolási tevékenység során a Blob-forrás és fogadó SQL-feldolgozási folyamat:**</span><span class="sxs-lookup"><span data-stu-id="ce567-304">**A copy activity in a pipeline with Blob source and SQL sink:**</span></span>

<span data-ttu-id="ce567-305">A feldolgozási sor tartalmazza a másolási tevékenység, amely a bemeneti és kimeneti adatkészletek használatára van konfigurálva, és óránkénti futásra nem ütemezték.</span><span class="sxs-lookup"><span data-stu-id="ce567-305">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="ce567-306">Az adatcsatorna JSON-definícióból a **forrás** típusúra **BlobSource** és **fogadó** típusúra **SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="ce567-306">In the pipeline JSON definition, the **source** type is set to **BlobSource** and **sink** type is set to **SqlSink**.</span></span>

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoSQL",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource",
            "blobColumnSeparators": ","
          },
          "sink": {
            "type": "SqlSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
      ]
   }
}
```
<span data-ttu-id="ce567-307">Tekintse meg a [Sql fogadó](#sqlsink) szakasz és [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) SqlSink és BlobSource által támogatott tulajdonságok listája.</span><span class="sxs-lookup"><span data-stu-id="ce567-307">See the [Sql Sink](#sqlsink) section and [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) for the list of properties supported by SqlSink and BlobSource.</span></span>

## <a name="identity-columns-in-the-target-database"></a><span data-ttu-id="ce567-308">A céladatbázis azonosító oszlop</span><span class="sxs-lookup"><span data-stu-id="ce567-308">Identity columns in the target database</span></span>
<span data-ttu-id="ce567-309">Ez a szakasz egy példát biztosít arra az adatok másolása a forrástábla nélkül azonosító oszlop azonosító oszlopot tartalmazó táblát.</span><span class="sxs-lookup"><span data-stu-id="ce567-309">This section provides an example for copying data from a source table without an identity column to a destination table with an identity column.</span></span>

<span data-ttu-id="ce567-310">**Forrástábla:**</span><span class="sxs-lookup"><span data-stu-id="ce567-310">**Source table:**</span></span>

```SQL
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```
<span data-ttu-id="ce567-311">**Céltábla:**</span><span class="sxs-lookup"><span data-stu-id="ce567-311">**Destination table:**</span></span>

```SQL
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```
<span data-ttu-id="ce567-312">Figyelje meg, hogy a céltábla rendelkezik-e az azonosító oszlop.</span><span class="sxs-lookup"><span data-stu-id="ce567-312">Notice that the target table has an identity column.</span></span>

<span data-ttu-id="ce567-313">**Forrás adatkészlet JSON-definícióból**</span><span class="sxs-lookup"><span data-stu-id="ce567-313">**Source dataset JSON definition**</span></span>

```JSON
{
    "name": "SampleSource",
    "properties": {
        "type": " SqlServerTable",
        "linkedServiceName": "TestIdentitySQL",
        "typeProperties": {
            "tableName": "SourceTbl"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```
<span data-ttu-id="ce567-314">**Cél adatkészlet JSON-definícióból**</span><span class="sxs-lookup"><span data-stu-id="ce567-314">**Destination dataset JSON definition**</span></span>

```JSON
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
        "type": "AzureSqlTable",
        "linkedServiceName": "TestIdentitySQLSource",
        "typeProperties": {
            "tableName": "TargetTbl"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": false,
        "policy": {}
    }    
}
```

<span data-ttu-id="ce567-315">Figyelje meg, hogy a forrás és cél táblázatként különböző sémája (cél rendelkezik egy olyan további oszlop identitású).</span><span class="sxs-lookup"><span data-stu-id="ce567-315">Notice that as your source and target table have different schema (target has an additional column with identity).</span></span> <span data-ttu-id="ce567-316">Ilyen esetben meg kell adnia **struktúra** tulajdonság az a tároló adatkészlet-definícióban, amely nem tartalmazza az identitásoszlop.</span><span class="sxs-lookup"><span data-stu-id="ce567-316">In this scenario, you need to specify **structure** property in the target dataset definition, which doesn’t include the identity column.</span></span>

## <a name="invoke-stored-procedure-from-sql-sink"></a><span data-ttu-id="ce567-317">A fogadó SQL tárolt eljárás meghívása</span><span class="sxs-lookup"><span data-stu-id="ce567-317">Invoke stored procedure from SQL sink</span></span>
<span data-ttu-id="ce567-318">A folyamat a másolási tevékenység SQL fogadó egy tárolt eljárás végrehajtásával hívásának példáért lásd: [fogadó SQL tárolt eljárás meghívása a másolási tevékenység](data-factory-invoke-stored-procedure-from-copy-activity.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="ce567-318">For an example of invoking a stored procedure from SQL sink in a copy activity of a pipeline, see [Invoke stored procedure for SQL sink in copy activity](data-factory-invoke-stored-procedure-from-copy-activity.md) article.</span></span> 

## <a name="type-mapping-for-azure-sql-database"></a><span data-ttu-id="ce567-319">Írja be az Azure SQL Database leképezése</span><span class="sxs-lookup"><span data-stu-id="ce567-319">Type mapping for Azure SQL Database</span></span>
<span data-ttu-id="ce567-320">Ahogyan az a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk másolási tevékenység az eseményforrás-típusnak gyűjtése módszert használja a következő 2. lépés típusok automatikus típuskonverziók hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="ce567-320">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article Copy activity performs automatic type conversions from source types to sink types with the following 2-step approach:</span></span>

1. <span data-ttu-id="ce567-321">A natív eseményforrás-típusnak átalakítása .NET-típusa</span><span class="sxs-lookup"><span data-stu-id="ce567-321">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="ce567-322">.NET-típus konvertálása natív a fogadó típusa</span><span class="sxs-lookup"><span data-stu-id="ce567-322">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="ce567-323">Ha az adatok mozgatása az Azure SQL Database, a következő megfeleltetéseket használ az SQL-típus a .NET-típus, és ez fordítva is igaz.</span><span class="sxs-lookup"><span data-stu-id="ce567-323">When moving data to and from Azure SQL Database, the following mappings are used from SQL type to .NET type and vice versa.</span></span> <span data-ttu-id="ce567-324">Leképezése nem ugyanaz, mint az SQL Server adattípus-hozzárendelése az ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="ce567-324">The mapping is same as the SQL Server Data Type Mapping for ADO.NET.</span></span>

| <span data-ttu-id="ce567-325">SQL Server adatbázismotor típusa</span><span class="sxs-lookup"><span data-stu-id="ce567-325">SQL Server Database Engine type</span></span> | <span data-ttu-id="ce567-326">.NET-keretrendszer típusa</span><span class="sxs-lookup"><span data-stu-id="ce567-326">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="ce567-327">bigint</span><span class="sxs-lookup"><span data-stu-id="ce567-327">bigint</span></span> |<span data-ttu-id="ce567-328">Int64</span><span class="sxs-lookup"><span data-stu-id="ce567-328">Int64</span></span> |
| <span data-ttu-id="ce567-329">Bináris</span><span class="sxs-lookup"><span data-stu-id="ce567-329">binary</span></span> |<span data-ttu-id="ce567-330">Byte]</span><span class="sxs-lookup"><span data-stu-id="ce567-330">Byte[]</span></span> |
| <span data-ttu-id="ce567-331">bit</span><span class="sxs-lookup"><span data-stu-id="ce567-331">bit</span></span> |<span data-ttu-id="ce567-332">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="ce567-332">Boolean</span></span> |
| <span data-ttu-id="ce567-333">Karakter</span><span class="sxs-lookup"><span data-stu-id="ce567-333">char</span></span> |<span data-ttu-id="ce567-334">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="ce567-334">String, Char[]</span></span> |
| <span data-ttu-id="ce567-335">Dátum</span><span class="sxs-lookup"><span data-stu-id="ce567-335">date</span></span> |<span data-ttu-id="ce567-336">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="ce567-336">DateTime</span></span> |
| <span data-ttu-id="ce567-337">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="ce567-337">Datetime</span></span> |<span data-ttu-id="ce567-338">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="ce567-338">DateTime</span></span> |
| <span data-ttu-id="ce567-339">datetime2</span><span class="sxs-lookup"><span data-stu-id="ce567-339">datetime2</span></span> |<span data-ttu-id="ce567-340">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="ce567-340">DateTime</span></span> |
| <span data-ttu-id="ce567-341">datetimeoffset</span><span class="sxs-lookup"><span data-stu-id="ce567-341">Datetimeoffset</span></span> |<span data-ttu-id="ce567-342">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="ce567-342">DateTimeOffset</span></span> |
| <span data-ttu-id="ce567-343">Decimális</span><span class="sxs-lookup"><span data-stu-id="ce567-343">Decimal</span></span> |<span data-ttu-id="ce567-344">Decimális</span><span class="sxs-lookup"><span data-stu-id="ce567-344">Decimal</span></span> |
| <span data-ttu-id="ce567-345">A FILESTREAM attribútum (varbinary(max))</span><span class="sxs-lookup"><span data-stu-id="ce567-345">FILESTREAM attribute (varbinary(max))</span></span> |<span data-ttu-id="ce567-346">Byte]</span><span class="sxs-lookup"><span data-stu-id="ce567-346">Byte[]</span></span> |
| <span data-ttu-id="ce567-347">Lebegőpontos</span><span class="sxs-lookup"><span data-stu-id="ce567-347">Float</span></span> |<span data-ttu-id="ce567-348">Dupla</span><span class="sxs-lookup"><span data-stu-id="ce567-348">Double</span></span> |
| <span data-ttu-id="ce567-349">Kép</span><span class="sxs-lookup"><span data-stu-id="ce567-349">image</span></span> |<span data-ttu-id="ce567-350">Byte]</span><span class="sxs-lookup"><span data-stu-id="ce567-350">Byte[]</span></span> |
| <span data-ttu-id="ce567-351">int</span><span class="sxs-lookup"><span data-stu-id="ce567-351">int</span></span> |<span data-ttu-id="ce567-352">Int32</span><span class="sxs-lookup"><span data-stu-id="ce567-352">Int32</span></span> |
| <span data-ttu-id="ce567-353">pénz</span><span class="sxs-lookup"><span data-stu-id="ce567-353">money</span></span> |<span data-ttu-id="ce567-354">Decimális</span><span class="sxs-lookup"><span data-stu-id="ce567-354">Decimal</span></span> |
| <span data-ttu-id="ce567-355">nchar</span><span class="sxs-lookup"><span data-stu-id="ce567-355">nchar</span></span> |<span data-ttu-id="ce567-356">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="ce567-356">String, Char[]</span></span> |
| <span data-ttu-id="ce567-357">ntext</span><span class="sxs-lookup"><span data-stu-id="ce567-357">ntext</span></span> |<span data-ttu-id="ce567-358">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="ce567-358">String, Char[]</span></span> |
| <span data-ttu-id="ce567-359">Numerikus</span><span class="sxs-lookup"><span data-stu-id="ce567-359">numeric</span></span> |<span data-ttu-id="ce567-360">Decimális</span><span class="sxs-lookup"><span data-stu-id="ce567-360">Decimal</span></span> |
| <span data-ttu-id="ce567-361">nvarchar</span><span class="sxs-lookup"><span data-stu-id="ce567-361">nvarchar</span></span> |<span data-ttu-id="ce567-362">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="ce567-362">String, Char[]</span></span> |
| <span data-ttu-id="ce567-363">valós</span><span class="sxs-lookup"><span data-stu-id="ce567-363">real</span></span> |<span data-ttu-id="ce567-364">Egyetlen</span><span class="sxs-lookup"><span data-stu-id="ce567-364">Single</span></span> |
| <span data-ttu-id="ce567-365">ROWVERSION</span><span class="sxs-lookup"><span data-stu-id="ce567-365">rowversion</span></span> |<span data-ttu-id="ce567-366">Byte]</span><span class="sxs-lookup"><span data-stu-id="ce567-366">Byte[]</span></span> |
| <span data-ttu-id="ce567-367">smalldatetime</span><span class="sxs-lookup"><span data-stu-id="ce567-367">smalldatetime</span></span> |<span data-ttu-id="ce567-368">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="ce567-368">DateTime</span></span> |
| <span data-ttu-id="ce567-369">smallint</span><span class="sxs-lookup"><span data-stu-id="ce567-369">smallint</span></span> |<span data-ttu-id="ce567-370">Int16</span><span class="sxs-lookup"><span data-stu-id="ce567-370">Int16</span></span> |
| <span data-ttu-id="ce567-371">kis pénz típusú értéknél</span><span class="sxs-lookup"><span data-stu-id="ce567-371">smallmoney</span></span> |<span data-ttu-id="ce567-372">Decimális</span><span class="sxs-lookup"><span data-stu-id="ce567-372">Decimal</span></span> |
| <span data-ttu-id="ce567-373">sql_variant</span><span class="sxs-lookup"><span data-stu-id="ce567-373">sql_variant</span></span> |<span data-ttu-id="ce567-374">Objektum *</span><span class="sxs-lookup"><span data-stu-id="ce567-374">Object *</span></span> |
| <span data-ttu-id="ce567-375">Szöveg</span><span class="sxs-lookup"><span data-stu-id="ce567-375">text</span></span> |<span data-ttu-id="ce567-376">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="ce567-376">String, Char[]</span></span> |
| <span data-ttu-id="ce567-377">time</span><span class="sxs-lookup"><span data-stu-id="ce567-377">time</span></span> |<span data-ttu-id="ce567-378">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="ce567-378">TimeSpan</span></span> |
| <span data-ttu-id="ce567-379">időbélyeg</span><span class="sxs-lookup"><span data-stu-id="ce567-379">timestamp</span></span> |<span data-ttu-id="ce567-380">Byte]</span><span class="sxs-lookup"><span data-stu-id="ce567-380">Byte[]</span></span> |
| <span data-ttu-id="ce567-381">tinyint</span><span class="sxs-lookup"><span data-stu-id="ce567-381">tinyint</span></span> |<span data-ttu-id="ce567-382">Bájt</span><span class="sxs-lookup"><span data-stu-id="ce567-382">Byte</span></span> |
| <span data-ttu-id="ce567-383">egyedi azonosító</span><span class="sxs-lookup"><span data-stu-id="ce567-383">uniqueidentifier</span></span> |<span data-ttu-id="ce567-384">GUID</span><span class="sxs-lookup"><span data-stu-id="ce567-384">Guid</span></span> |
| <span data-ttu-id="ce567-385">varbinary</span><span class="sxs-lookup"><span data-stu-id="ce567-385">varbinary</span></span> |<span data-ttu-id="ce567-386">Byte]</span><span class="sxs-lookup"><span data-stu-id="ce567-386">Byte[]</span></span> |
| <span data-ttu-id="ce567-387">varchar</span><span class="sxs-lookup"><span data-stu-id="ce567-387">varchar</span></span> |<span data-ttu-id="ce567-388">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="ce567-388">String, Char[]</span></span> |
| <span data-ttu-id="ce567-389">xml</span><span class="sxs-lookup"><span data-stu-id="ce567-389">xml</span></span> |<span data-ttu-id="ce567-390">XML</span><span class="sxs-lookup"><span data-stu-id="ce567-390">Xml</span></span> |

## <a name="map-source-to-sink-columns"></a><span data-ttu-id="ce567-391">Térkép forrás oszlopok gyűjtése</span><span class="sxs-lookup"><span data-stu-id="ce567-391">Map source to sink columns</span></span>
<span data-ttu-id="ce567-392">A forrás oszlop szerepel a fogadó dataset adatkészlet leképezési oszlopok, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="ce567-392">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-copy"></a><span data-ttu-id="ce567-393">Ismételhető másolása</span><span class="sxs-lookup"><span data-stu-id="ce567-393">Repeatable copy</span></span>
<span data-ttu-id="ce567-394">Amikor adat másolása az SQL Server-adatbázis, a másolási tevékenység hozzáfűzi adatokat a fogadó tábla alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="ce567-394">When copying data to SQL Server Database, the copy activity appends data to the sink table by default.</span></span> <span data-ttu-id="ce567-395">Ehelyett egy UPSERT végrehajtásához tekintse meg [Repeatable írni SqlSink](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) cikk.</span><span class="sxs-lookup"><span data-stu-id="ce567-395">To perform an UPSERT instead,  See [Repeatable write to SqlSink](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) article.</span></span> 

<span data-ttu-id="ce567-396">Ha az adatok másolását a relációs adatokat tárol, ismételhetőség tartsa szem előtt, nem kívánt eredmények elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="ce567-396">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="ce567-397">Az Azure Data Factoryben futtathatja a szelet manuálisan.</span><span class="sxs-lookup"><span data-stu-id="ce567-397">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="ce567-398">Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="ce567-398">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="ce567-399">A szelet akkor fut újra, vagy módon, ha győződjön meg arról, hogy ugyanazokat az adatokat olvasható függetlenül attól, hogy a szelet futtatása hány alkalommal kell.</span><span class="sxs-lookup"><span data-stu-id="ce567-399">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="ce567-400">Lásd: [relációs források olvasni Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="ce567-400">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="ce567-401">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="ce567-401">Performance and Tuning</span></span>
<span data-ttu-id="ce567-402">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) tájékozódhat az kulcsfontosságú szerepet játszik adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon optimalizálhatja azt, hogy hatás teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="ce567-402">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
