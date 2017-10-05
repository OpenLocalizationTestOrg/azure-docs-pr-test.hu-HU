---
title: "Adatok másolása az Azure SQL Data Warehouse |} Microsoft Docs"
description: "Útmutató: Azure SQL Data Warehouse Azure Data Factory használatával és a-adatok másolása"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: d90fa9bd-4b79-458a-8d40-e896835cfd4a
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: 8cba89e0947646b498af07aa484511bf07bf7b0e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="copy-data-to-and-from-azure-sql-data-warehouse-using-azure-data-factory"></a><span data-ttu-id="5e177-103">Másolja az adatokat, és az Azure SQL Data Warehouse Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="5e177-103">Copy data to and from Azure SQL Data Warehouse using Azure Data Factory</span></span>
<span data-ttu-id="5e177-104">Ez a cikk azt ismerteti, hogyan használható a másolási tevékenység során az Azure Data Factory adatok áthelyezése az Azure SQL Data Warehouse és a.</span><span class="sxs-lookup"><span data-stu-id="5e177-104">This article explains how to use the Copy Activity in Azure Data Factory to move data to/from Azure SQL Data Warehouse.</span></span> <span data-ttu-id="5e177-105">Buildekről nyújtanak a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, amelynek során adatátvitel a másolási tevékenység az általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="5e177-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>  

> [!TIP]
> <span data-ttu-id="5e177-106">A legjobb teljesítmény érdekében az adatok betöltése az Azure SQL Data Warehouse polybase szolgáltatást akkor használja.</span><span class="sxs-lookup"><span data-stu-id="5e177-106">To achieve best performance, use PolyBase to load data into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="5e177-107">A [használja a PolyBase az adatok betöltése az Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) szakasz részleteket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="5e177-107">The [Use PolyBase to load data into Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) section has details.</span></span> <span data-ttu-id="5e177-108">A használati esetek bemutatóért lásd: [1 TB-os betöltése az Azure SQL Data Warehouse a 15 perc Azure Data Factory](data-factory-load-sql-data-warehouse.md).</span><span class="sxs-lookup"><span data-stu-id="5e177-108">For a walkthrough with a use case, see [Load 1 TB into Azure SQL Data Warehouse under 15 minutes with Azure Data Factory](data-factory-load-sql-data-warehouse.md).</span></span>

## <a name="supported-scenarios"></a><span data-ttu-id="5e177-109">Támogatott helyzetek</span><span class="sxs-lookup"><span data-stu-id="5e177-109">Supported scenarios</span></span>
<span data-ttu-id="5e177-110">Adatokat másolhat **az Azure SQL Data Warehouse** tárolja a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="5e177-110">You can copy data **from Azure SQL Data Warehouse** to the following data stores:</span></span>

[!INCLUDE [data-factory-supported-sinks](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="5e177-111">Adatok másolása a következő adatokat tárolja **az Azure SQL Data Warehouse**:</span><span class="sxs-lookup"><span data-stu-id="5e177-111">You can copy data from the following data stores **to Azure SQL Data Warehouse**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

> [!TIP]
> <span data-ttu-id="5e177-112">Adat másolása az SQL Server vagy az Azure SQL Database az Azure SQL Data Warehouse, ha a tábla nem létezik a tárolási célhelye, amikor Data Factory automatikusan a tábla az SQL Data Warehouse segítségével létrehozható a tábla sémája a forrás-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="5e177-112">When copying data from SQL Server or Azure SQL Database to Azure SQL Data Warehouse, if the table does not exist in the destination store, Data Factory can automatically create the table in SQL Data Warehouse by using the schema of the table in the source data store.</span></span> <span data-ttu-id="5e177-113">Lásd: [tábla létrehozásához automatikus](#auto-table-creation) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="5e177-113">See [Auto table creation](#auto-table-creation) for details.</span></span>

## <a name="supported-authentication-type"></a><span data-ttu-id="5e177-114">Támogatott hitelesítési típushoz</span><span class="sxs-lookup"><span data-stu-id="5e177-114">Supported authentication type</span></span>
<span data-ttu-id="5e177-115">Az Azure SQL Data Warehouse összekötő alapszintű hitelesítés támogatása.</span><span class="sxs-lookup"><span data-stu-id="5e177-115">Azure SQL Data Warehouse connector support basic authentication.</span></span>

## <a name="getting-started"></a><span data-ttu-id="5e177-116">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="5e177-116">Getting started</span></span>
<span data-ttu-id="5e177-117">A másolási tevékenység, amely helyezi át az adatokat az Azure SQL Data Warehouse és a különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="5e177-117">You can create a pipeline with a copy activity that moves data to/from an Azure SQL Data Warehouse by using different tools/APIs.</span></span>

<span data-ttu-id="5e177-118">A legegyszerűbben úgy, hogy hozzon létre egy folyamatot, amely másolja az adatokat és a Azure SQL Data Warehouse-hoz adatok másolása varázsló használatával.</span><span class="sxs-lookup"><span data-stu-id="5e177-118">The easiest way to create a pipeline that copies data to/from Azure SQL Data Warehouse is to use the Copy data wizard.</span></span> <span data-ttu-id="5e177-119">Lásd: [oktatóanyag: adatok betöltése az SQL Data Warehouse Data Factory](../sql-data-warehouse/sql-data-warehouse-load-with-data-factory.md) létrehozásával egy folyamatot, az adatok másolása varázsló segítségével gyorsan útmutatást.</span><span class="sxs-lookup"><span data-stu-id="5e177-119">See [Tutorial: Load data into SQL Data Warehouse with Data Factory](../sql-data-warehouse/sql-data-warehouse-load-with-data-factory.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="5e177-120">Az alábbi eszközöket használhatja a folyamatokat létrehozni: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager sablon**, **.NET API**, és **REST API**.</span><span class="sxs-lookup"><span data-stu-id="5e177-120">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="5e177-121">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) hozzon létre egy folyamatot a másolási tevékenység részletes útmutatóját.</span><span class="sxs-lookup"><span data-stu-id="5e177-121">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span>

<span data-ttu-id="5e177-122">Akár az eszközök vagy API-k, hajtsa végre a következő lépésekkel hozza létre egy folyamatot, amely mozgatja az adatokat a forrás-tárolóban a fogadó tárolóban:</span><span class="sxs-lookup"><span data-stu-id="5e177-122">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="5e177-123">Hozzon létre egy **adat-előállító**.</span><span class="sxs-lookup"><span data-stu-id="5e177-123">Create a **data factory**.</span></span> <span data-ttu-id="5e177-124">Egy adat-előállító tartalmazhat egy vagy több folyamatok.</span><span class="sxs-lookup"><span data-stu-id="5e177-124">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="5e177-125">Hozzon létre **összekapcsolt szolgáltatások** bemeneti és kimeneti adatok csatolásához tárolja a a data factory.</span><span class="sxs-lookup"><span data-stu-id="5e177-125">Create **linked services** to link input and output data stores to your data factory.</span></span> <span data-ttu-id="5e177-126">Például ha a másolt adatok az az Azure blob storage egy Azure SQL data warehouse, hoz létre a Azure storage-fiók és az Azure SQL data warehouse összekapcsolása a data factory két társított szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="5e177-126">For example, if you are copying data from an Azure blob storage to an Azure SQL data warehouse, you create two linked services to link your Azure storage account and Azure SQL data warehouse to your data factory.</span></span> <span data-ttu-id="5e177-127">Az Azure SQL Data Warehouse jellemző csatolt szolgáltatás tulajdonságait, lásd: [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="5e177-127">For linked service properties that are specific to Azure SQL Data Warehouse, see [linked service properties](#linked-service-properties) section.</span></span> 
3. <span data-ttu-id="5e177-128">Hozzon létre **adatkészletek** a másolási művelet bemeneti és kimeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="5e177-128">Create **datasets** to represent input and output data for the copy operation.</span></span> <span data-ttu-id="5e177-129">A példában az előző lépésben említett hozzon létre egy adatkészlet adja meg a blob-tároló és a bemeneti adatokat tartalmazó mappát.</span><span class="sxs-lookup"><span data-stu-id="5e177-129">In the example mentioned in the last step, you create a dataset to specify the blob container and folder that contains the input data.</span></span> <span data-ttu-id="5e177-130">És hoz létre, ha meg szeretné adni a tábla az Azure SQL data warehouse, amely tárolja az adatokat a blob-tároló átmásolja egy másik DataSet adatkészletben.</span><span class="sxs-lookup"><span data-stu-id="5e177-130">And, you create another dataset to specify the table in the Azure SQL data warehouse that holds the data copied from the blob storage.</span></span> <span data-ttu-id="5e177-131">Az Azure SQL Data Warehouse jellemző adatkészlet tulajdonságai, lásd: [adatkészlet tulajdonságai](#dataset-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="5e177-131">For dataset properties that are specific to Azure SQL Data Warehouse, see [dataset properties](#dataset-properties) section.</span></span>
4. <span data-ttu-id="5e177-132">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="5e177-132">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="5e177-133">A korábban említett példában BlobSource forrás-és SqlDWSink akár használhatja a fogadó a másolási tevékenységhez.</span><span class="sxs-lookup"><span data-stu-id="5e177-133">In the example mentioned earlier, you use BlobSource as a source and SqlDWSink as a sink for the copy activity.</span></span> <span data-ttu-id="5e177-134">Ehhez hasonlóan az Azure Blob Storage másolása az Azure SQL Data Warehouse, használható SqlDWSource és BlobSink a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="5e177-134">Similarly, if you are copying from Azure SQL Data Warehouse to Azure Blob Storage, you use SqlDWSource and BlobSink in the copy activity.</span></span> <span data-ttu-id="5e177-135">Az Azure SQL Data Warehouse adott tevékenység Tulajdonságok másolása, lásd: [tevékenység Tulajdonságok másolása](#copy-activity-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="5e177-135">For copy activity properties that are specific to Azure SQL Data Warehouse, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="5e177-136">További részletek a tárolóban használatáról a forrás vagy a fogadó a hivatkozásra a adattároló az előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="5e177-136">For details on how to use a data store as a source or a sink, click the link in the previous section for your data store.</span></span>

<span data-ttu-id="5e177-137">A varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és a feldolgozási sor) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="5e177-137">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="5e177-138">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások a JSON formátum használatával.</span><span class="sxs-lookup"><span data-stu-id="5e177-138">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="5e177-139">Adatok másolása az Azure SQL Data Warehouse használandó adat-előállító entitások JSON-definíciók minták, lásd: [JSON példák](#json-examples-for-copying-data-to-and-from-sql-data-warehouse) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="5e177-139">For samples with JSON definitions for Data Factory entities that are used to copy data to/from an Azure SQL Data Warehouse, see [JSON examples](#json-examples-for-copying-data-to-and-from-sql-data-warehouse) section of this article.</span></span>

<span data-ttu-id="5e177-140">A következő szakaszok részletesen bemutatják, amely segítségével határozza meg a Data Factory tartozó entitások az Azure SQL Data Warehouse JSON-tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="5e177-140">The following sections provide details about JSON properties that are used to define Data Factory entities specific to Azure SQL Data Warehouse:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="5e177-141">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="5e177-141">Linked service properties</span></span>
<span data-ttu-id="5e177-142">A következő táblázat a JSON-elemek szerepelnek Azure SQL Data Warehouse kapcsolódó szolgáltatásra vonatkozó leírást.</span><span class="sxs-lookup"><span data-stu-id="5e177-142">The following table provides description for JSON elements specific to Azure SQL Data Warehouse linked service.</span></span>

| <span data-ttu-id="5e177-143">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="5e177-143">Property</span></span> | <span data-ttu-id="5e177-144">Leírás</span><span class="sxs-lookup"><span data-stu-id="5e177-144">Description</span></span> | <span data-ttu-id="5e177-145">Szükséges</span><span class="sxs-lookup"><span data-stu-id="5e177-145">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e177-146">type</span><span class="sxs-lookup"><span data-stu-id="5e177-146">type</span></span> |<span data-ttu-id="5e177-147">A type tulajdonságot kell beállítani: **AzureSqlDW**</span><span class="sxs-lookup"><span data-stu-id="5e177-147">The type property must be set to: **AzureSqlDW**</span></span> |<span data-ttu-id="5e177-148">Igen</span><span class="sxs-lookup"><span data-stu-id="5e177-148">Yes</span></span> |
| <span data-ttu-id="5e177-149">connectionString</span><span class="sxs-lookup"><span data-stu-id="5e177-149">connectionString</span></span> |<span data-ttu-id="5e177-150">Adja meg a connectionString tulajdonság az Azure SQL Data Warehouse-példány való kapcsolódáshoz szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="5e177-150">Specify information needed to connect to the Azure SQL Data Warehouse instance for the connectionString property.</span></span> <span data-ttu-id="5e177-151">Csak az alapszintű hitelesítést is támogatja.</span><span class="sxs-lookup"><span data-stu-id="5e177-151">Only basic authentication is supported.</span></span> |<span data-ttu-id="5e177-152">Igen</span><span class="sxs-lookup"><span data-stu-id="5e177-152">Yes</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="5e177-153">Konfigurálása [Azure SQL Database-tűzfal](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) és az adatbázis-kiszolgálót [a kiszolgálóhoz való hozzáféréshez Azure-szolgáltatások engedélyezése](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure).</span><span class="sxs-lookup"><span data-stu-id="5e177-153">Configure [Azure SQL Database Firewall](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) and the database server to [allow Azure Services to access the server](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure).</span></span> <span data-ttu-id="5e177-154">Ezenkívül az adatokat az Azure SQL Data Warehouse a külső Azure többek között a helyszíni adatforrásokból a data factory átjáróval másolása, az IP-címtartományt a gép, amely adatokat küld az Azure SQL Data Warehouse konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="5e177-154">Additionally, if you are copying data to Azure SQL Data Warehouse from outside Azure including from on-premises data sources with data factory gateway, configure appropriate IP address range for the machine that is sending data to Azure SQL Data Warehouse.</span></span>

## <a name="dataset-properties"></a><span data-ttu-id="5e177-155">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="5e177-155">Dataset properties</span></span>
<span data-ttu-id="5e177-156">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: a [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="5e177-156">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="5e177-157">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="5e177-157">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="5e177-158">A typeProperties szakasz más adatkészlet egyes típusai és információkat nyújt azokról az adattárban adatok helyét.</span><span class="sxs-lookup"><span data-stu-id="5e177-158">The typeProperties section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="5e177-159">A **typeProperties** szakasz az adatkészlet típusú **AzureSqlDWTable** tulajdonságai a következők:</span><span class="sxs-lookup"><span data-stu-id="5e177-159">The **typeProperties** section for the dataset of type **AzureSqlDWTable** has the following properties:</span></span>

| <span data-ttu-id="5e177-160">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="5e177-160">Property</span></span> | <span data-ttu-id="5e177-161">Leírás</span><span class="sxs-lookup"><span data-stu-id="5e177-161">Description</span></span> | <span data-ttu-id="5e177-162">Szükséges</span><span class="sxs-lookup"><span data-stu-id="5e177-162">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e177-163">tableName</span><span class="sxs-lookup"><span data-stu-id="5e177-163">tableName</span></span> |<span data-ttu-id="5e177-164">A tábla vagy nézet, az Azure SQL Data Warehouse-adatbázis hivatkozik a társított szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="5e177-164">Name of the table or view in the Azure SQL Data Warehouse database that the linked service refers to.</span></span> |<span data-ttu-id="5e177-165">Igen</span><span class="sxs-lookup"><span data-stu-id="5e177-165">Yes</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="5e177-166">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="5e177-166">Copy activity properties</span></span>
<span data-ttu-id="5e177-167">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: a [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="5e177-167">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="5e177-168">Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="5e177-168">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

> [!NOTE]
> <span data-ttu-id="5e177-169">A másolási tevékenység során csak egy bemenettel rendelkezik, és csak egy kimenetet.</span><span class="sxs-lookup"><span data-stu-id="5e177-169">The Copy Activity takes only one input and produces only one output.</span></span>

<span data-ttu-id="5e177-170">Mivel a tevékenység typeProperties szakaszában elérhető tulajdonságok tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="5e177-170">Whereas, properties available in the typeProperties section of the activity vary with each activity type.</span></span> <span data-ttu-id="5e177-171">A másolási tevékenység során két érték források és mosdók típusától függően.</span><span class="sxs-lookup"><span data-stu-id="5e177-171">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

### <a name="sqldwsource"></a><span data-ttu-id="5e177-172">SqlDWSource</span><span class="sxs-lookup"><span data-stu-id="5e177-172">SqlDWSource</span></span>
<span data-ttu-id="5e177-173">Ha a forrás típusa van **SqlDWSource**, a következő tulajdonságok érhetők el **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="5e177-173">When source is of type **SqlDWSource**, the following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="5e177-174">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="5e177-174">Property</span></span> | <span data-ttu-id="5e177-175">Leírás</span><span class="sxs-lookup"><span data-stu-id="5e177-175">Description</span></span> | <span data-ttu-id="5e177-176">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="5e177-176">Allowed values</span></span> | <span data-ttu-id="5e177-177">Szükséges</span><span class="sxs-lookup"><span data-stu-id="5e177-177">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e177-178">sqlReaderQuery</span><span class="sxs-lookup"><span data-stu-id="5e177-178">sqlReaderQuery</span></span> |<span data-ttu-id="5e177-179">Az egyéni lekérdezés segítségével adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="5e177-179">Use the custom query to read data.</span></span> |<span data-ttu-id="5e177-180">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="5e177-180">SQL query string.</span></span> <span data-ttu-id="5e177-181">Például: Válasszon * from tábla.</span><span class="sxs-lookup"><span data-stu-id="5e177-181">For example: select * from MyTable.</span></span> |<span data-ttu-id="5e177-182">Nem</span><span class="sxs-lookup"><span data-stu-id="5e177-182">No</span></span> |
| <span data-ttu-id="5e177-183">sqlReaderStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="5e177-183">sqlReaderStoredProcedureName</span></span> |<span data-ttu-id="5e177-184">A tárolt eljárás, amely adatokat olvas a forrástábla neve.</span><span class="sxs-lookup"><span data-stu-id="5e177-184">Name of the stored procedure that reads data from the source table.</span></span> |<span data-ttu-id="5e177-185">A tárolt eljárás neve.</span><span class="sxs-lookup"><span data-stu-id="5e177-185">Name of the stored procedure.</span></span> <span data-ttu-id="5e177-186">Az utolsó SQL-utasítás a következő tárolt eljárást a SELECT utasítással kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5e177-186">The last SQL statement must be a SELECT statement in the stored procedure.</span></span> |<span data-ttu-id="5e177-187">Nem</span><span class="sxs-lookup"><span data-stu-id="5e177-187">No</span></span> |
| <span data-ttu-id="5e177-188">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="5e177-188">storedProcedureParameters</span></span> |<span data-ttu-id="5e177-189">A tárolt eljárás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="5e177-189">Parameters for the stored procedure.</span></span> |<span data-ttu-id="5e177-190">A név/érték párok.</span><span class="sxs-lookup"><span data-stu-id="5e177-190">Name/value pairs.</span></span> <span data-ttu-id="5e177-191">Nevét és a kis-és a paraméterek meg kell egyeznie a nevek és a kis-és nagybetűhasználat a tárolt eljárás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="5e177-191">Names and casing of parameters must match the names and casing of the stored procedure parameters.</span></span> |<span data-ttu-id="5e177-192">Nem</span><span class="sxs-lookup"><span data-stu-id="5e177-192">No</span></span> |

<span data-ttu-id="5e177-193">Ha a **sqlReaderQuery** van megadva a SqlDWSource, a másolási tevékenység fut ez a lekérdezés az adatok lekérdezése az Azure SQL Data Warehouse forrás.</span><span class="sxs-lookup"><span data-stu-id="5e177-193">If the **sqlReaderQuery** is specified for the SqlDWSource, the Copy Activity runs this query against the Azure SQL Data Warehouse source to get the data.</span></span>

<span data-ttu-id="5e177-194">Másik lehetőségként megadhat tárolt eljárás megadásával a **sqlReaderStoredProcedureName** és **storedProcedureParameters** (Ha a tárolt eljárás paraméterek fogadja el).</span><span class="sxs-lookup"><span data-stu-id="5e177-194">Alternatively, you can specify a stored procedure by specifying the **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if the stored procedure takes parameters).</span></span>

<span data-ttu-id="5e177-195">Ha nem ad meg sqlReaderQuery vagy sqlReaderStoredProcedureName, az adatkészlet JSON struktúrában szakaszában meghatározott oszlopokat futtatni az Azure SQL Data Warehouse-lekérdezés összeállításához használt.</span><span class="sxs-lookup"><span data-stu-id="5e177-195">If you do not specify either sqlReaderQuery or sqlReaderStoredProcedureName, the columns defined in the structure section of the dataset JSON are used to build a query to run against the Azure SQL Data Warehouse.</span></span> <span data-ttu-id="5e177-196">Példa: `select column1, column2 from mytable`.</span><span class="sxs-lookup"><span data-stu-id="5e177-196">Example: `select column1, column2 from mytable`.</span></span> <span data-ttu-id="5e177-197">Az adatkészlet-definícióban nem rendelkezik a struktúra, ha minden kiválasztott oszlop. a táblából.</span><span class="sxs-lookup"><span data-stu-id="5e177-197">If the dataset definition does not have the structure, all columns are selected from the table.</span></span>

#### <a name="sqldwsource-example"></a><span data-ttu-id="5e177-198">SqlDWSource – példa</span><span class="sxs-lookup"><span data-stu-id="5e177-198">SqlDWSource example</span></span>

```JSON
"source": {
    "type": "SqlDWSource",
    "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
    "storedProcedureParameters": {
        "stringData": { "value": "str3" },
        "identifier": { "value": "$$Text.Format('{0:yyyy}', SliceStart)", "type": "Int"}
    }
}
```
<span data-ttu-id="5e177-199">**A tárolt eljárás definíciója:**</span><span class="sxs-lookup"><span data-stu-id="5e177-199">**The stored procedure definition:**</span></span>

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

### <a name="sqldwsink"></a><span data-ttu-id="5e177-200">SqlDWSink</span><span class="sxs-lookup"><span data-stu-id="5e177-200">SqlDWSink</span></span>
<span data-ttu-id="5e177-201">**SqlDWSink** támogatja a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="5e177-201">**SqlDWSink** supports the following properties:</span></span>

| <span data-ttu-id="5e177-202">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="5e177-202">Property</span></span> | <span data-ttu-id="5e177-203">Leírás</span><span class="sxs-lookup"><span data-stu-id="5e177-203">Description</span></span> | <span data-ttu-id="5e177-204">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="5e177-204">Allowed values</span></span> | <span data-ttu-id="5e177-205">Szükséges</span><span class="sxs-lookup"><span data-stu-id="5e177-205">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="5e177-206">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="5e177-206">sqlWriterCleanupScript</span></span> |<span data-ttu-id="5e177-207">Adja meg egy lekérdezést a másolási tevékenység végrehajtása úgy, hogy egy adott szelet adatait.</span><span class="sxs-lookup"><span data-stu-id="5e177-207">Specify a query for Copy Activity to execute such that data of a specific slice is cleaned up.</span></span> <span data-ttu-id="5e177-208">További információkért lásd: [ismételhetőség szakasz](#repeatability-during-copy).</span><span class="sxs-lookup"><span data-stu-id="5e177-208">For details, see [repeatability section](#repeatability-during-copy).</span></span> |<span data-ttu-id="5e177-209">A lekérdezési utasítást.</span><span class="sxs-lookup"><span data-stu-id="5e177-209">A query statement.</span></span> |<span data-ttu-id="5e177-210">Nem</span><span class="sxs-lookup"><span data-stu-id="5e177-210">No</span></span> |
| <span data-ttu-id="5e177-211">allowPolyBase</span><span class="sxs-lookup"><span data-stu-id="5e177-211">allowPolyBase</span></span> |<span data-ttu-id="5e177-212">Azt jelzi, hogy (ha alkalmazható), a PolyBase használata helyett BULKINSERT mechanizmus.</span><span class="sxs-lookup"><span data-stu-id="5e177-212">Indicates whether to use PolyBase (when applicable) instead of BULKINSERT mechanism.</span></span> <br/><br/> <span data-ttu-id="5e177-213">**Az ajánlott módszer az adatok betöltése az SQL Data Warehouse PolyBase a használata.**</span><span class="sxs-lookup"><span data-stu-id="5e177-213">**Using PolyBase is the recommended way to load data into SQL Data Warehouse.**</span></span> <span data-ttu-id="5e177-214">Lásd: [használja a PolyBase az adatok betöltése az Azure SQL Data Warehouse](#use-polybase-to-load-data-into-azure-sql-data-warehouse) szakaszban a korlátozások és részleteit.</span><span class="sxs-lookup"><span data-stu-id="5e177-214">See [Use PolyBase to load data into Azure SQL Data Warehouse](#use-polybase-to-load-data-into-azure-sql-data-warehouse) section for constraints and details.</span></span> |<span data-ttu-id="5e177-215">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="5e177-215">True</span></span> <br/><span data-ttu-id="5e177-216">Hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="5e177-216">False (default)</span></span> |<span data-ttu-id="5e177-217">Nem</span><span class="sxs-lookup"><span data-stu-id="5e177-217">No</span></span> |
| <span data-ttu-id="5e177-218">kapcsolódó polyBaseSettings</span><span class="sxs-lookup"><span data-stu-id="5e177-218">polyBaseSettings</span></span> |<span data-ttu-id="5e177-219">Egy csoport, amely tulajdonságok megadott, amikor a **allowPolybase** tulajdonsága **igaz**.</span><span class="sxs-lookup"><span data-stu-id="5e177-219">A group of properties that can be specified when the **allowPolybase** property is set to **true**.</span></span> |&nbsp; |<span data-ttu-id="5e177-220">Nem</span><span class="sxs-lookup"><span data-stu-id="5e177-220">No</span></span> |
| <span data-ttu-id="5e177-221">rejectValue</span><span class="sxs-lookup"><span data-stu-id="5e177-221">rejectValue</span></span> |<span data-ttu-id="5e177-222">Megadja a szám vagy is el kell utasítani, mielőtt a lekérdezés nem sikerült sorokat százalékát.</span><span class="sxs-lookup"><span data-stu-id="5e177-222">Specifies the number or percentage of rows that can be rejected before the query fails.</span></span> <br/><br/><span data-ttu-id="5e177-223">További információ a PolyBase utasítsa el a beállítások elemre a **argumentumok** szakasza [külső tábla létrehozása (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) témakör.</span><span class="sxs-lookup"><span data-stu-id="5e177-223">Learn more about the PolyBase’s reject options in the **Arguments** section of [CREATE EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) topic.</span></span> |<span data-ttu-id="5e177-224">0 (alapértelmezés), 1, 2...</span><span class="sxs-lookup"><span data-stu-id="5e177-224">0 (default), 1, 2, …</span></span> |<span data-ttu-id="5e177-225">Nem</span><span class="sxs-lookup"><span data-stu-id="5e177-225">No</span></span> |
| <span data-ttu-id="5e177-226">rejectType</span><span class="sxs-lookup"><span data-stu-id="5e177-226">rejectType</span></span> |<span data-ttu-id="5e177-227">Határozza meg, hogy a rejectValue beállítás konstans értéket vagy százalékában van megadva.</span><span class="sxs-lookup"><span data-stu-id="5e177-227">Specifies whether the rejectValue option is specified as a literal value or a percentage.</span></span> |<span data-ttu-id="5e177-228">Érték (alapértelmezett), százalékos aránya</span><span class="sxs-lookup"><span data-stu-id="5e177-228">Value (default), Percentage</span></span> |<span data-ttu-id="5e177-229">Nem</span><span class="sxs-lookup"><span data-stu-id="5e177-229">No</span></span> |
| <span data-ttu-id="5e177-230">rejectSampleValue</span><span class="sxs-lookup"><span data-stu-id="5e177-230">rejectSampleValue</span></span> |<span data-ttu-id="5e177-231">Mielőtt a PolyBase újraszámítja a visszautasított sorok százalékát beolvasandó sorok számát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="5e177-231">Determines the number of rows to retrieve before the PolyBase recalculates the percentage of rejected rows.</span></span> |<span data-ttu-id="5e177-232">1, 2, …</span><span class="sxs-lookup"><span data-stu-id="5e177-232">1, 2, …</span></span> |<span data-ttu-id="5e177-233">Igen, ha **rejectType** van **százalékos aránya**</span><span class="sxs-lookup"><span data-stu-id="5e177-233">Yes, if **rejectType** is **percentage**</span></span> |
| <span data-ttu-id="5e177-234">useTypeDefault</span><span class="sxs-lookup"><span data-stu-id="5e177-234">useTypeDefault</span></span> |<span data-ttu-id="5e177-235">Megadja, hogyan legyen kezelve tagolt szövegfájlok a hiányzó értékeket, amikor a PolyBase kér le adatokat a szövegfájlból.</span><span class="sxs-lookup"><span data-stu-id="5e177-235">Specifies how to handle missing values in delimited text files when PolyBase retrieves data from the text file.</span></span><br/><br/><span data-ttu-id="5e177-236">Ezt a tulajdonságot, az argumentumok ismertető részben olvashat [létrehozása külső FÁJLFORMÁTUM (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx).</span><span class="sxs-lookup"><span data-stu-id="5e177-236">Learn more about this property from the Arguments section in [CREATE EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx).</span></span> |<span data-ttu-id="5e177-237">IGAZ, hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="5e177-237">True, False (default)</span></span> |<span data-ttu-id="5e177-238">Nem</span><span class="sxs-lookup"><span data-stu-id="5e177-238">No</span></span> |
| <span data-ttu-id="5e177-239">WriteBatchSize</span><span class="sxs-lookup"><span data-stu-id="5e177-239">writeBatchSize</span></span> |<span data-ttu-id="5e177-240">Adatok beszúrása a SQL táblázatba, amikor a puffer mérete eléri writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="5e177-240">Inserts data into the SQL table when the buffer size reaches writeBatchSize</span></span> |<span data-ttu-id="5e177-241">Egész szám (sorok száma)</span><span class="sxs-lookup"><span data-stu-id="5e177-241">Integer (number of rows)</span></span> |<span data-ttu-id="5e177-242">Nem (alapértelmezett: 10000)</span><span class="sxs-lookup"><span data-stu-id="5e177-242">No (default: 10000)</span></span> |
| <span data-ttu-id="5e177-243">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="5e177-243">writeBatchTimeout</span></span> |<span data-ttu-id="5e177-244">Várakozási idő a kötegelt beszúrási művelet befejezését, mielőtt azt az időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="5e177-244">Wait time for the batch insert operation to complete before it times out.</span></span> |<span data-ttu-id="5e177-245">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="5e177-245">timespan</span></span><br/><br/> <span data-ttu-id="5e177-246">Példa: "00: 30:00" (30 perc).</span><span class="sxs-lookup"><span data-stu-id="5e177-246">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="5e177-247">Nem</span><span class="sxs-lookup"><span data-stu-id="5e177-247">No</span></span> |

#### <a name="sqldwsink-example"></a><span data-ttu-id="5e177-248">SqlDWSink – példa</span><span class="sxs-lookup"><span data-stu-id="5e177-248">SqlDWSink example</span></span>

```JSON
"sink": {
    "type": "SqlDWSink",
    "allowPolyBase": true
}
```

## <a name="use-polybase-to-load-data-into-azure-sql-data-warehouse"></a><span data-ttu-id="5e177-249">Adatok betöltése az Azure SQL Data Warehouse PolyBase segítségével</span><span class="sxs-lookup"><span data-stu-id="5e177-249">Use PolyBase to load data into Azure SQL Data Warehouse</span></span>
<span data-ttu-id="5e177-250">Használatával  **[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide)**  egy hatékony módszer a nagy mennyiségű adatok betöltését az Azure SQL Data Warehouse nagy átviteli sebességgel.</span><span class="sxs-lookup"><span data-stu-id="5e177-250">Using **[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide)** is an efficient way of loading large amount of data into Azure SQL Data Warehouse with high throughput.</span></span> <span data-ttu-id="5e177-251">A teljesítmény a nagy nyereség helyett az alapértelmezett BULKINSERT mechanizmus a PolyBase használatával tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="5e177-251">You can see a large gain in the throughput by using PolyBase instead of the default BULKINSERT mechanism.</span></span> <span data-ttu-id="5e177-252">Lásd: [teljesítmény hivatkozási szám másolása](data-factory-copy-activity-performance.md#performance-reference) a részletes összehasonlítását.</span><span class="sxs-lookup"><span data-stu-id="5e177-252">See [copy performance reference number](data-factory-copy-activity-performance.md#performance-reference) with detailed comparison.</span></span> <span data-ttu-id="5e177-253">A használati esetek bemutatóért lásd: [1 TB-os betöltése az Azure SQL Data Warehouse a 15 perc Azure Data Factory](data-factory-load-sql-data-warehouse.md).</span><span class="sxs-lookup"><span data-stu-id="5e177-253">For a walkthrough with a use case, see [Load 1 TB into Azure SQL Data Warehouse under 15 minutes with Azure Data Factory](data-factory-load-sql-data-warehouse.md).</span></span>

* <span data-ttu-id="5e177-254">Ha a forrás adatok **Azure Blob vagy az Azure Data Lake Store**, és a formátuma nem kompatibilis a PolyBase, közvetlenül másolhatja az Azure SQL Data Warehouse PolyBase használatával.</span><span class="sxs-lookup"><span data-stu-id="5e177-254">If your source data is in **Azure Blob or Azure Data Lake Store**, and the format is compatible with PolyBase, you can directly copy to Azure SQL Data Warehouse using PolyBase.</span></span> <span data-ttu-id="5e177-255">Lásd:  **[közvetlen másolása a PolyBase használatával](#direct-copy-using-polybase)**  adatokkal.</span><span class="sxs-lookup"><span data-stu-id="5e177-255">See **[Direct copy using PolyBase](#direct-copy-using-polybase)** with details.</span></span>
* <span data-ttu-id="5e177-256">Ha a forrás-tárolót és formátum eredetileg nem támogatott a PolyBase által, használhatja a  **[előkészített másolása a PolyBase használatával](#staged-copy-using-polybase)**  inkább a beállítást.</span><span class="sxs-lookup"><span data-stu-id="5e177-256">If your source data store and format is not originally supported by PolyBase, you can use the **[Staged Copy using PolyBase](#staged-copy-using-polybase)** feature instead.</span></span> <span data-ttu-id="5e177-257">Is biztosít, nagyobb átviteli sebesség automatikusan adatok PolyBase-kompatibilis formátumra való konvertálása, és az adatok tárolása az Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="5e177-257">It also provides you better throughput by automatically converting the data into PolyBase-compatible format and storing the data in Azure Blob storage.</span></span> <span data-ttu-id="5e177-258">Majd betölti az SQL Data Warehouse-adatok.</span><span class="sxs-lookup"><span data-stu-id="5e177-258">It then loads data into SQL Data Warehouse.</span></span>

<span data-ttu-id="5e177-259">Állítsa be a `allowPolyBase` tulajdonságot **igaz** az Azure Data Factoryben az adatok másolása az Azure SQL Data Warehouse polybase szolgáltatást akkor használja a következő példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="5e177-259">Set the `allowPolyBase` property to **true** as shown in the following example for Azure Data Factory to use PolyBase to copy data into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="5e177-260">AllowPolyBase értéke igaz, amikor a PolyBase konkrét tulajdonságok használatával megadhatja a `polyBaseSettings` tulajdonságcsoport.</span><span class="sxs-lookup"><span data-stu-id="5e177-260">When you set allowPolyBase to true, you can specify PolyBase specific properties using the `polyBaseSettings` property group.</span></span> <span data-ttu-id="5e177-261">Tekintse meg a [SqlDWSink](#SqlDWSink) szakasz kapcsolódó polyBaseSettings használható tulajdonságokról vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="5e177-261">see the [SqlDWSink](#SqlDWSink) section for details about properties that you can use with polyBaseSettings.</span></span>

```JSON
"sink": {
    "type": "SqlDWSink",
    "allowPolyBase": true,
    "polyBaseSettings":
    {
        "rejectType": "percentage",
        "rejectValue": 10.0,
        "rejectSampleValue": 100,
        "useTypeDefault": true
    }
}
```

### <a name="direct-copy-using-polybase"></a><span data-ttu-id="5e177-262">A PolyBase használatával közvetlen másolása</span><span class="sxs-lookup"><span data-stu-id="5e177-262">Direct copy using PolyBase</span></span>
<span data-ttu-id="5e177-263">SQL Data Warehouse PolyBase közvetlenül támogatja Azure-Blob és az Azure Data Lake Store (egyszerű szolgáltatásnév) forrásként, és az adott fájl formátum követelményeinek.</span><span class="sxs-lookup"><span data-stu-id="5e177-263">SQL Data Warehouse PolyBase directly support Azure Blob and Azure Data Lake Store (using service principal) as source and with specific file format requirements.</span></span> <span data-ttu-id="5e177-264">Ha a forrásadatok megfelel a jelen szakaszban bemutatott, közvetlenül átmásolhatja forrás adattár az Azure SQL Data Warehouse PolyBase használatával.</span><span class="sxs-lookup"><span data-stu-id="5e177-264">If your source data meets the criteria described in this section, you can directly copy from source data store to Azure SQL Data Warehouse using PolyBase.</span></span> <span data-ttu-id="5e177-265">Ellenkező esetben használhatja [előkészített másolása a PolyBase használatával](#staged-copy-using-polybase).</span><span class="sxs-lookup"><span data-stu-id="5e177-265">Otherwise, you can use [Staged Copy using PolyBase](#staged-copy-using-polybase).</span></span>

> [!TIP]
> <span data-ttu-id="5e177-266">Adatok másolása Data Lake Store az SQL Data Warehouse hatékonyan, további információhoz [Azure Data Factory megkönnyíti még és kényelmes elvégzésével nyújt betekintést az adatokat, az SQL Data Warehouse szolgáltatással Data Lake Store használatakor](https://blogs.msdn.microsoft.com/azuredatalake/2017/04/08/azure-data-factory-makes-it-even-easier-and-convenient-to-uncover-insights-from-data-when-using-data-lake-store-with-sql-data-warehouse/).</span><span class="sxs-lookup"><span data-stu-id="5e177-266">To copy data from Data Lake Store to SQL Data Warehouse efficiently, learn more from [Azure Data Factory makes it even easier and convenient to uncover insights from data when using Data Lake Store with SQL Data Warehouse](https://blogs.msdn.microsoft.com/azuredatalake/2017/04/08/azure-data-factory-makes-it-even-easier-and-convenient-to-uncover-insights-from-data-when-using-data-lake-store-with-sql-data-warehouse/).</span></span>

<span data-ttu-id="5e177-267">A feltételeknek nem felel meg, ha az Azure Data Factory ellenőrzi a beállításait, és automatikusan visszaáll az adatátvitelt jelölik a BULKINSERT mechanizmus.</span><span class="sxs-lookup"><span data-stu-id="5e177-267">If the requirements are not met, Azure Data Factory checks the settings and automatically falls back to the BULKINSERT mechanism for the data movement.</span></span>

1. <span data-ttu-id="5e177-268">**Forrás társított szolgáltatás** típusa: **AzureStorage** vagy **szolgáltatás egyszerű hitelesítéssel AzureDataLakeStore**.</span><span class="sxs-lookup"><span data-stu-id="5e177-268">**Source linked service** is of type: **AzureStorage** or **AzureDataLakeStore with service principal authentication**.</span></span>  
2. <span data-ttu-id="5e177-269">A **bemeneti adatkészlet** típusa: **AzureBlob** vagy **AzureDataLakeStore**, és írja be a format `type` tulajdonságai **OrcFormat**, vagy **szöveges** , a következő beállításokat:</span><span class="sxs-lookup"><span data-stu-id="5e177-269">The **input dataset** is of type: **AzureBlob** or **AzureDataLakeStore**, and the format type under `type` properties is **OrcFormat**, or **TextFormat** with the following configurations:</span></span>

   1. <span data-ttu-id="5e177-270">`rowDelimiter`kell  **\n** .</span><span class="sxs-lookup"><span data-stu-id="5e177-270">`rowDelimiter` must be **\n**.</span></span>
   2. <span data-ttu-id="5e177-271">`nullValue`értéke **üres karakterlánc** (""), vagy `treatEmptyAsNull` értéke **igaz**.</span><span class="sxs-lookup"><span data-stu-id="5e177-271">`nullValue` is set to **empty string** (""), or `treatEmptyAsNull` is set to **true**.</span></span>
   3. <span data-ttu-id="5e177-272">`encodingName`értéke **utf-8**, amely **alapértelmezett** érték.</span><span class="sxs-lookup"><span data-stu-id="5e177-272">`encodingName` is set to **utf-8**, which is **default** value.</span></span>
   4. <span data-ttu-id="5e177-273">`escapeChar`, `quoteChar`, `firstRowAsHeader`, és `skipLineCount` nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="5e177-273">`escapeChar`, `quoteChar`, `firstRowAsHeader`, and `skipLineCount` are not specified.</span></span>
   5. <span data-ttu-id="5e177-274">`compression`lehet **tömörítés**, **GZip**, vagy **Deflate**.</span><span class="sxs-lookup"><span data-stu-id="5e177-274">`compression` can be **no compression**, **GZip**, or **Deflate**.</span></span>

    ```JSON
    "typeProperties": {
       "folderPath": "<blobpath>",
       "format": {
           "type": "TextFormat",     
           "columnDelimiter": "<any delimiter>",
           "rowDelimiter": "\n",       
           "nullValue": "",           
           "encodingName": "utf-8"    
       },
       "compression": {  
           "type": "GZip",  
           "level": "Optimal"  
       }  
    },
    ```

3. <span data-ttu-id="5e177-275">Nincs nincs `skipHeaderLineCount` beállítás alatt **BlobSource** vagy **AzureDataLakeStore** a másolási tevékenységhez, a folyamat.</span><span class="sxs-lookup"><span data-stu-id="5e177-275">There is no `skipHeaderLineCount` setting under **BlobSource** or **AzureDataLakeStore** for the Copy activity in the pipeline.</span></span>
4. <span data-ttu-id="5e177-276">Nincs nincs `sliceIdentifierColumnName` beállítás alatt **SqlDWSink** a másolási tevékenységhez, a folyamat.</span><span class="sxs-lookup"><span data-stu-id="5e177-276">There is no `sliceIdentifierColumnName` setting under **SqlDWSink** for the Copy activity in the pipeline.</span></span> <span data-ttu-id="5e177-277">(A PolyBase garantálja, hogy minden adat frissül, vagy nem frissül, az egyszeri futtatás.</span><span class="sxs-lookup"><span data-stu-id="5e177-277">(PolyBase guarantees that all data is updated or nothing is updated in a single run.</span></span> <span data-ttu-id="5e177-278">Eléréséhez **ismételhetőség**, használhat `sqlWriterCleanupScript`).</span><span class="sxs-lookup"><span data-stu-id="5e177-278">To achieve **repeatability**, you could use `sqlWriterCleanupScript`).</span></span>
5. <span data-ttu-id="5e177-279">Nincs nincs `columnMapping` használatban lévő a kapcsolódó, a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="5e177-279">There is no `columnMapping` being used in the associated in Copy activity.</span></span>

### <a name="staged-copy-using-polybase"></a><span data-ttu-id="5e177-280">A PolyBase használatával előkészített másolása</span><span class="sxs-lookup"><span data-stu-id="5e177-280">Staged Copy using PolyBase</span></span>
<span data-ttu-id="5e177-281">A forrásadatok nem felel meg az előző szakaszban bemutatott, amikor az adatok másolását az átmeneti átmeneti Azure Blob Storage (nem lehet a prémium szintű Storage) keresztül is engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="5e177-281">When your source data doesn’t meet the criteria introduced in the previous section, you can enable copying data via an interim staging Azure Blob Storage (cannot be Premium Storage).</span></span> <span data-ttu-id="5e177-282">Ebben az esetben Azure Data Factory automatikusan végez átalakítások adatok formátuma elégíteni PolyBase, akkor az SQL Data Warehouse-, illetve utolsó karbantartás az adatok betöltése a PolyBase segítségével az adatok ideiglenes adatait a blobtárolóból.</span><span class="sxs-lookup"><span data-stu-id="5e177-282">In this case, Azure Data Factory automatically performs transformations on the data to meet data format requirements of PolyBase, then use PolyBase to load data into SQL Data Warehouse, and at last clean-up your temp data from the Blob storage.</span></span> <span data-ttu-id="5e177-283">Lásd: [előkészített másolási](data-factory-copy-activity-performance.md#staged-copy) átmeneti Azure Blob keresztül az adatok másolásának működéséről, általában a részletekért.</span><span class="sxs-lookup"><span data-stu-id="5e177-283">See [Staged Copy](data-factory-copy-activity-performance.md#staged-copy) for details on how copying data via a staging Azure Blob works in general.</span></span>

> [!NOTE]
> <span data-ttu-id="5e177-284">Ha az Azure SQL Data Warehouse PolyBase használatával tárolja az adatok másolását egy helyszíni adatokból, és átmeneti, ha az adatkezelési átjáró verziója nem éri el 2.4, JRE (Java Runtime Environment) szükséges az átjáró gépen, amely használatával a forrásadatok megfelelő formátumba.</span><span class="sxs-lookup"><span data-stu-id="5e177-284">When copying data from an on-prem data store into Azure SQL Data Warehouse using PolyBase and staging, if your Data Management Gateway version is below 2.4, JRE (Java Runtime Environment) is required on your gateway machine that is used to transform your source data into proper format.</span></span> <span data-ttu-id="5e177-285">Javasoljuk, hogy az ilyen függőségi elkerülése érdekében az átjárót a legújabb verzióra frissít.</span><span class="sxs-lookup"><span data-stu-id="5e177-285">Suggest you upgrade your gateway to the latest to avoid such dependency.</span></span>
>

<span data-ttu-id="5e177-286">Ez a funkció használatához hozzon létre egy [Azure Storage társított szolgáltatásnak](data-factory-azure-blob-connector.md#azure-storage-linked-service) , amely hivatkozik, amely rendelkezik az átmeneti a blob storage Azure Storage-fiókot, majd adja meg a `enableStaging` és `stagingSettings` látható módon a másolási tevékenység tulajdonságai a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="5e177-286">To use this feature, create an [Azure Storage linked service](data-factory-azure-blob-connector.md#azure-storage-linked-service) that refers to the Azure Storage Account that has the interim blob storage, then specify the `enableStaging` and `stagingSettings` properties for the Copy Activity as shown in the following code:</span></span>

```json
"activities":[  
{
    "name": "Sample copy activity from SQL Server to SQL Data Warehouse via PolyBase",
    "type": "Copy",
    "inputs": [{ "name": "OnpremisesSQLServerInput" }],
    "outputs": [{ "name": "AzureSQLDWOutput" }],
    "typeProperties": {
        "source": {
            "type": "SqlSource",
        },
        "sink": {
            "type": "SqlDwSink",
            "allowPolyBase": true
        },
        "enableStaging": true,
        "stagingSettings": {
            "linkedServiceName": "MyStagingBlob"
        }
    }
}
]
```

## <a name="best-practices-when-using-polybase"></a><span data-ttu-id="5e177-287">A PolyBase használata esetén ajánlott eljárások</span><span class="sxs-lookup"><span data-stu-id="5e177-287">Best practices when using PolyBase</span></span>
<span data-ttu-id="5e177-288">Az alábbi szakaszokban további gyakorlati tanácsok a meglévők közül ismertetett [ajánlott eljárások az Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="5e177-288">The following sections provide additional best practices to the ones that are mentioned in [Best practices for Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md).</span></span>

### <a name="required-database-permission"></a><span data-ttu-id="5e177-289">Adatbázis szükséges engedéllyel</span><span class="sxs-lookup"><span data-stu-id="5e177-289">Required database permission</span></span>
<span data-ttu-id="5e177-290">PolyBase használatához igényel az adatok betöltése az SQL Data Warehouse használt felhasználó rendelkezik-e a ["Vezérlő" engedély](https://msdn.microsoft.com/library/ms191291.aspx) a célként megadott adatbázison.</span><span class="sxs-lookup"><span data-stu-id="5e177-290">To use PolyBase, it requires the user being used to load data into SQL Data Warehouse has the ["CONTROL" permission](https://msdn.microsoft.com/library/ms191291.aspx) on the target database.</span></span> <span data-ttu-id="5e177-291">Egyik módja, hogy, hogy a felhasználó hozzáadása "db_owner" szerepkör tagjaként.</span><span class="sxs-lookup"><span data-stu-id="5e177-291">One way to achieve that is to add that user as a member of "db_owner" role.</span></span> <span data-ttu-id="5e177-292">Útmutató a következő ehhez [ebben a szakaszban](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md#authorization).</span><span class="sxs-lookup"><span data-stu-id="5e177-292">Learn how to do that by following [this section](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md#authorization).</span></span>

### <a name="row-size-and-data-type-limitation"></a><span data-ttu-id="5e177-293">Írja be korlátozás sorméret és adatok</span><span class="sxs-lookup"><span data-stu-id="5e177-293">Row size and data type limitation</span></span>
<span data-ttu-id="5e177-294">A Polybase terhelések esetén egyre korlátozódik betöltése sort is kisebb, mint **1 MB** és VARCHR(MAX), NVARCHAR(MAX) vagy VARBINARY(MAX) nem tölthető be.</span><span class="sxs-lookup"><span data-stu-id="5e177-294">Polybase loads are limited to loading rows both smaller than **1 MB** and cannot load to VARCHR(MAX), NVARCHAR(MAX) or VARBINARY(MAX).</span></span> <span data-ttu-id="5e177-295">Tekintse meg [Itt](../sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md#loads).</span><span class="sxs-lookup"><span data-stu-id="5e177-295">Refer to [here](../sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md#loads).</span></span>

<span data-ttu-id="5e177-296">Ha 1 MB-nál nagyobb méretű sorokat tartalmazó forrásadatok, érdemes lehet a forrástáblákból függőleges felosztása több kis megfelelően, ha azok legnagyobb sor mérete nem haladja meg a határértéket.</span><span class="sxs-lookup"><span data-stu-id="5e177-296">If you have source data with rows of size greater than 1 MB, you may want to split the source tables vertically into several small ones where the largest row size of each of them does not exceed the limit.</span></span> <span data-ttu-id="5e177-297">A kisebb táblák majd lehet betölteni. a PolyBase használatával, és egyesíti az Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="5e177-297">The smaller tables can then be loaded using PolyBase and merged together in Azure SQL Data Warehouse.</span></span>

### <a name="sql-data-warehouse-resource-class"></a><span data-ttu-id="5e177-298">Az SQL Data Warehouse erőforrás osztály</span><span class="sxs-lookup"><span data-stu-id="5e177-298">SQL Data Warehouse resource class</span></span>
<span data-ttu-id="5e177-299">Lehetséges legjobb teljesítmény elérése érdekében fontolja meg a felhasználói adatok betöltése az SQL Data Warehouse polybase használt nagyobb erőforrásosztály hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="5e177-299">To achieve best possible throughput, consider to assign larger resource class to the user being used to load data into SQL Data Warehouse via PolyBase.</span></span> <span data-ttu-id="5e177-300">Útmutató a következő ehhez [módosíthatja a felhasználói erőforrás osztály példa](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).</span><span class="sxs-lookup"><span data-stu-id="5e177-300">Learn how to do that by following [Change a user resource class example](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).</span></span>

### <a name="tablename-in-azure-sql-data-warehouse"></a><span data-ttu-id="5e177-301">az Azure SQL Data Warehouse táblanév</span><span class="sxs-lookup"><span data-stu-id="5e177-301">tableName in Azure SQL Data Warehouse</span></span>
<span data-ttu-id="5e177-302">A következő táblázat példákat tartalmaz a megadása a **tableName** tulajdonságot az adatkészlet JSON-séma-és táblanevet különböző kombinációjához.</span><span class="sxs-lookup"><span data-stu-id="5e177-302">The following table provides examples on how to specify the **tableName** property in dataset JSON for various combinations of schema and table name.</span></span>

| <span data-ttu-id="5e177-303">Megadott adatbázissémát</span><span class="sxs-lookup"><span data-stu-id="5e177-303">DB Schema</span></span> | <span data-ttu-id="5e177-304">Tábla neve</span><span class="sxs-lookup"><span data-stu-id="5e177-304">Table name</span></span> | <span data-ttu-id="5e177-305">tableName JSON tulajdonsága</span><span class="sxs-lookup"><span data-stu-id="5e177-305">tableName JSON property</span></span> |
| --- | --- | --- |
| <span data-ttu-id="5e177-306">dbo</span><span class="sxs-lookup"><span data-stu-id="5e177-306">dbo</span></span> |<span data-ttu-id="5e177-307">Táblanév</span><span class="sxs-lookup"><span data-stu-id="5e177-307">MyTable</span></span> |<span data-ttu-id="5e177-308">MyTable vagy dbo. MyTable vagy [dbo]. [MyTable]</span><span class="sxs-lookup"><span data-stu-id="5e177-308">MyTable or dbo.MyTable or [dbo].[MyTable]</span></span> |
| <span data-ttu-id="5e177-309">dbo1</span><span class="sxs-lookup"><span data-stu-id="5e177-309">dbo1</span></span> |<span data-ttu-id="5e177-310">Táblanév</span><span class="sxs-lookup"><span data-stu-id="5e177-310">MyTable</span></span> |<span data-ttu-id="5e177-311">dbo1. MyTable vagy [dbo1]. [MyTable]</span><span class="sxs-lookup"><span data-stu-id="5e177-311">dbo1.MyTable or [dbo1].[MyTable]</span></span> |
| <span data-ttu-id="5e177-312">dbo</span><span class="sxs-lookup"><span data-stu-id="5e177-312">dbo</span></span> |<span data-ttu-id="5e177-313">My.Table</span><span class="sxs-lookup"><span data-stu-id="5e177-313">My.Table</span></span> |<span data-ttu-id="5e177-314">[My.Table] vagy [dbo]. [My.Table]</span><span class="sxs-lookup"><span data-stu-id="5e177-314">[My.Table] or [dbo].[My.Table]</span></span> |
| <span data-ttu-id="5e177-315">dbo1</span><span class="sxs-lookup"><span data-stu-id="5e177-315">dbo1</span></span> |<span data-ttu-id="5e177-316">My.Table</span><span class="sxs-lookup"><span data-stu-id="5e177-316">My.Table</span></span> |<span data-ttu-id="5e177-317">[dbo1]. [My.Table]</span><span class="sxs-lookup"><span data-stu-id="5e177-317">[dbo1].[My.Table]</span></span> |

<span data-ttu-id="5e177-318">A következő hibát látja, ha problémát a tableName tulajdonsághoz megadott érték lehet.</span><span class="sxs-lookup"><span data-stu-id="5e177-318">If you see the following error, it could be an issue with the value you specified for the tableName property.</span></span> <span data-ttu-id="5e177-319">Lásd a megfelelő módon adhatja meg a tableName JSON tulajdonság értékei a táblázatban.</span><span class="sxs-lookup"><span data-stu-id="5e177-319">See the table for the correct way to specify values for the tableName JSON property.</span></span>  

```
Type=System.Data.SqlClient.SqlException,Message=Invalid object name 'stg.Account_test'.,Source=.Net SqlClient Data Provider
```

### <a name="columns-with-default-values"></a><span data-ttu-id="5e177-320">Az alapértelmezett értékekkel oszlopok</span><span class="sxs-lookup"><span data-stu-id="5e177-320">Columns with default values</span></span>
<span data-ttu-id="5e177-321">A Data Factory PolyBase szolgáltatás jelenleg csak az azonos számú oszlopot, mint a céltábla fogad el.</span><span class="sxs-lookup"><span data-stu-id="5e177-321">Currently, PolyBase feature in Data Factory only accepts the same number of columns as in the target table.</span></span> <span data-ttu-id="5e177-322">Tegyük fel például, négy oszlopokkal rendelkező táblát rendelkezik, és az egyik legyen alapértelmezett értékkel van definiálva.</span><span class="sxs-lookup"><span data-stu-id="5e177-322">Say, you have a table with four columns and one of them is defined with a default value.</span></span> <span data-ttu-id="5e177-323">A bemeneti adatok továbbra is tartalmaznia kell a négy oszlopot.</span><span class="sxs-lookup"><span data-stu-id="5e177-323">The input data should still contain four columns.</span></span> <span data-ttu-id="5e177-324">A 3-oszlop a bemeneti adatkészletet biztosító eredményezné az alábbihoz hasonló hiba:</span><span class="sxs-lookup"><span data-stu-id="5e177-324">Providing a 3-column input dataset would yield an error similar to the following message:</span></span>

```
All columns of the table must be specified in the INSERT BULK statement.
```
<span data-ttu-id="5e177-325">NULL érték az alapértelmezett érték egy speciális formája, amely.</span><span class="sxs-lookup"><span data-stu-id="5e177-325">NULL value is a special form of default value.</span></span> <span data-ttu-id="5e177-326">Ha az oszlop nullázható, az adott oszlop a bemeneti adatokat (a blob) üres lehet (nem hiányzik a bemeneti adatkészlet).</span><span class="sxs-lookup"><span data-stu-id="5e177-326">If the column is nullable, the input data (in blob) for that column could be empty (cannot be missing from the input dataset).</span></span> <span data-ttu-id="5e177-327">A PolyBase NULL számukra az Azure SQL Data Warehouse szúrja be.</span><span class="sxs-lookup"><span data-stu-id="5e177-327">PolyBase inserts NULL for them in the Azure SQL Data Warehouse.</span></span>  

## <a name="auto-table-creation"></a><span data-ttu-id="5e177-328">Automatikus tábla létrehozásához</span><span class="sxs-lookup"><span data-stu-id="5e177-328">Auto table creation</span></span>
<span data-ttu-id="5e177-329">Ha másolása varázsló segítségével adatok másolása az SQL Server vagy az Azure SQL Database az Azure SQL Data warehouse-ba, és a táblázat, amely megfelel a következő forrástábla nem szerepel a rendeltetési tárolási, adat-előállító automatikusan létrehozásához a tábla az adatraktár u a forrás táblaséma folyamata.</span><span class="sxs-lookup"><span data-stu-id="5e177-329">If you are using Copy Wizard to copy data from SQL Server or Azure SQL Database to Azure SQL Data Warehouse and the table that corresponds to the source table does not exist in the destination store, Data Factory can automatically create the table in the data warehouse by using the source table schema.</span></span>

<span data-ttu-id="5e177-330">Adat-előállító hoz létre a tábla a céltár a tábla néven a forrás-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="5e177-330">Data Factory creates the table in the destination store with the same table name in the source data store.</span></span> <span data-ttu-id="5e177-331">Az adattípusok oszlopok a következő típusleképezéshez alapján választják ki.</span><span class="sxs-lookup"><span data-stu-id="5e177-331">The data types for columns are chosen based on the following type mapping.</span></span> <span data-ttu-id="5e177-332">Ha szükséges, az azonosított inkompatibilitásokat javításához a forrás- és tárolók közötti típuskonverziók hajt végre.</span><span class="sxs-lookup"><span data-stu-id="5e177-332">If needed, it performs type conversions to fix any incompatibilities between source and destination stores.</span></span> <span data-ttu-id="5e177-333">Ciklikus multiplexelés elosztása is használja.</span><span class="sxs-lookup"><span data-stu-id="5e177-333">It also uses Round Robin table distribution.</span></span>

| <span data-ttu-id="5e177-334">Forrás SQL-adatbázis oszlop típusa</span><span class="sxs-lookup"><span data-stu-id="5e177-334">Source SQL Database column type</span></span> | <span data-ttu-id="5e177-335">Cél SQL DW oszlop típusa (méretének korlátozása)</span><span class="sxs-lookup"><span data-stu-id="5e177-335">Destination SQL DW column type (size limitation)</span></span> |
| --- | --- |
| <span data-ttu-id="5e177-336">int</span><span class="sxs-lookup"><span data-stu-id="5e177-336">Int</span></span> | <span data-ttu-id="5e177-337">int</span><span class="sxs-lookup"><span data-stu-id="5e177-337">Int</span></span> |
| <span data-ttu-id="5e177-338">BigInt</span><span class="sxs-lookup"><span data-stu-id="5e177-338">BigInt</span></span> | <span data-ttu-id="5e177-339">BigInt</span><span class="sxs-lookup"><span data-stu-id="5e177-339">BigInt</span></span> |
| <span data-ttu-id="5e177-340">SmallInt</span><span class="sxs-lookup"><span data-stu-id="5e177-340">SmallInt</span></span> | <span data-ttu-id="5e177-341">SmallInt</span><span class="sxs-lookup"><span data-stu-id="5e177-341">SmallInt</span></span> |
| <span data-ttu-id="5e177-342">TinyInt</span><span class="sxs-lookup"><span data-stu-id="5e177-342">TinyInt</span></span> | <span data-ttu-id="5e177-343">TinyInt</span><span class="sxs-lookup"><span data-stu-id="5e177-343">TinyInt</span></span> |
| <span data-ttu-id="5e177-344">bit</span><span class="sxs-lookup"><span data-stu-id="5e177-344">Bit</span></span> | <span data-ttu-id="5e177-345">bit</span><span class="sxs-lookup"><span data-stu-id="5e177-345">Bit</span></span> |
| <span data-ttu-id="5e177-346">Decimális</span><span class="sxs-lookup"><span data-stu-id="5e177-346">Decimal</span></span> | <span data-ttu-id="5e177-347">Decimális</span><span class="sxs-lookup"><span data-stu-id="5e177-347">Decimal</span></span> |
| <span data-ttu-id="5e177-348">Numerikus</span><span class="sxs-lookup"><span data-stu-id="5e177-348">Numeric</span></span> | <span data-ttu-id="5e177-349">Decimális</span><span class="sxs-lookup"><span data-stu-id="5e177-349">Decimal</span></span> |
| <span data-ttu-id="5e177-350">Lebegőpontos</span><span class="sxs-lookup"><span data-stu-id="5e177-350">Float</span></span> | <span data-ttu-id="5e177-351">Lebegőpontos</span><span class="sxs-lookup"><span data-stu-id="5e177-351">Float</span></span> |
| <span data-ttu-id="5e177-352">pénz</span><span class="sxs-lookup"><span data-stu-id="5e177-352">Money</span></span> | <span data-ttu-id="5e177-353">pénz</span><span class="sxs-lookup"><span data-stu-id="5e177-353">Money</span></span> |
| <span data-ttu-id="5e177-354">Real</span><span class="sxs-lookup"><span data-stu-id="5e177-354">Real</span></span> | <span data-ttu-id="5e177-355">Real</span><span class="sxs-lookup"><span data-stu-id="5e177-355">Real</span></span> |
| <span data-ttu-id="5e177-356">Kis pénz típusú értéknél</span><span class="sxs-lookup"><span data-stu-id="5e177-356">SmallMoney</span></span> | <span data-ttu-id="5e177-357">Kis pénz típusú értéknél</span><span class="sxs-lookup"><span data-stu-id="5e177-357">SmallMoney</span></span> |
| <span data-ttu-id="5e177-358">Bináris</span><span class="sxs-lookup"><span data-stu-id="5e177-358">Binary</span></span> | <span data-ttu-id="5e177-359">Bináris</span><span class="sxs-lookup"><span data-stu-id="5e177-359">Binary</span></span> |
| <span data-ttu-id="5e177-360">varbinary</span><span class="sxs-lookup"><span data-stu-id="5e177-360">Varbinary</span></span> | <span data-ttu-id="5e177-361">Varbinary (legfeljebb 8000)</span><span class="sxs-lookup"><span data-stu-id="5e177-361">Varbinary (up to 8000)</span></span> |
| <span data-ttu-id="5e177-362">Dátum</span><span class="sxs-lookup"><span data-stu-id="5e177-362">Date</span></span> | <span data-ttu-id="5e177-363">Dátum</span><span class="sxs-lookup"><span data-stu-id="5e177-363">Date</span></span> |
| <span data-ttu-id="5e177-364">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="5e177-364">DateTime</span></span> | <span data-ttu-id="5e177-365">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="5e177-365">DateTime</span></span> |
| <span data-ttu-id="5e177-366">DateTime2</span><span class="sxs-lookup"><span data-stu-id="5e177-366">DateTime2</span></span> | <span data-ttu-id="5e177-367">DateTime2</span><span class="sxs-lookup"><span data-stu-id="5e177-367">DateTime2</span></span> |
| <span data-ttu-id="5e177-368">Time</span><span class="sxs-lookup"><span data-stu-id="5e177-368">Time</span></span> | <span data-ttu-id="5e177-369">Time</span><span class="sxs-lookup"><span data-stu-id="5e177-369">Time</span></span> |
| <span data-ttu-id="5e177-370">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="5e177-370">DateTimeOffset</span></span> | <span data-ttu-id="5e177-371">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="5e177-371">DateTimeOffset</span></span> |
| <span data-ttu-id="5e177-372">SmallDateTime</span><span class="sxs-lookup"><span data-stu-id="5e177-372">SmallDateTime</span></span> | <span data-ttu-id="5e177-373">SmallDateTime</span><span class="sxs-lookup"><span data-stu-id="5e177-373">SmallDateTime</span></span> |
| <span data-ttu-id="5e177-374">Szöveg</span><span class="sxs-lookup"><span data-stu-id="5e177-374">Text</span></span> | <span data-ttu-id="5e177-375">Varchar (legfeljebb 8000)</span><span class="sxs-lookup"><span data-stu-id="5e177-375">Varchar (up to 8000)</span></span> |
| <span data-ttu-id="5e177-376">NText</span><span class="sxs-lookup"><span data-stu-id="5e177-376">NText</span></span> | <span data-ttu-id="5e177-377">NVarChar (legfeljebb 4000)</span><span class="sxs-lookup"><span data-stu-id="5e177-377">NVarChar (up to 4000)</span></span> |
| <span data-ttu-id="5e177-378">Kép</span><span class="sxs-lookup"><span data-stu-id="5e177-378">Image</span></span> | <span data-ttu-id="5e177-379">VarBinary (legfeljebb 8000)</span><span class="sxs-lookup"><span data-stu-id="5e177-379">VarBinary (up to 8000)</span></span> |
| <span data-ttu-id="5e177-380">Egyedi azonosító</span><span class="sxs-lookup"><span data-stu-id="5e177-380">UniqueIdentifier</span></span> | <span data-ttu-id="5e177-381">Egyedi azonosító</span><span class="sxs-lookup"><span data-stu-id="5e177-381">UniqueIdentifier</span></span> |
| <span data-ttu-id="5e177-382">Karakter</span><span class="sxs-lookup"><span data-stu-id="5e177-382">Char</span></span> | <span data-ttu-id="5e177-383">Karakter</span><span class="sxs-lookup"><span data-stu-id="5e177-383">Char</span></span> |
| <span data-ttu-id="5e177-384">NChar</span><span class="sxs-lookup"><span data-stu-id="5e177-384">NChar</span></span> | <span data-ttu-id="5e177-385">NChar</span><span class="sxs-lookup"><span data-stu-id="5e177-385">NChar</span></span> |
| <span data-ttu-id="5e177-386">VarChar</span><span class="sxs-lookup"><span data-stu-id="5e177-386">VarChar</span></span> | <span data-ttu-id="5e177-387">VarChar (legfeljebb 8000)</span><span class="sxs-lookup"><span data-stu-id="5e177-387">VarChar (up to 8000)</span></span> |
| <span data-ttu-id="5e177-388">NVarChar</span><span class="sxs-lookup"><span data-stu-id="5e177-388">NVarChar</span></span> | <span data-ttu-id="5e177-389">NVarChar (legfeljebb 4000)</span><span class="sxs-lookup"><span data-stu-id="5e177-389">NVarChar (up to 4000)</span></span> |
| <span data-ttu-id="5e177-390">XML</span><span class="sxs-lookup"><span data-stu-id="5e177-390">Xml</span></span> | <span data-ttu-id="5e177-391">Varchar (legfeljebb 8000)</span><span class="sxs-lookup"><span data-stu-id="5e177-391">Varchar (up to 8000)</span></span> |

[!INCLUDE [data-factory-type-repeatability-for-sql-sources](../../includes/data-factory-type-repeatability-for-sql-sources.md)]

## <a name="type-mapping-for-azure-sql-data-warehouse"></a><span data-ttu-id="5e177-392">Írja be az Azure SQL Data Warehouse leképezése</span><span class="sxs-lookup"><span data-stu-id="5e177-392">Type mapping for Azure SQL Data Warehouse</span></span>
<span data-ttu-id="5e177-393">Ahogyan az a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, a másolási tevékenység eseményforrás-típusnak gyűjtése típusa a következő 2. lépés – a módszert használja az automatikus típuskonverziók hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="5e177-393">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types to sink types with the following 2-step approach:</span></span>

1. <span data-ttu-id="5e177-394">A natív eseményforrás-típusnak átalakítása .NET-típusa</span><span class="sxs-lookup"><span data-stu-id="5e177-394">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="5e177-395">.NET-típus konvertálása natív a fogadó típusa</span><span class="sxs-lookup"><span data-stu-id="5e177-395">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="5e177-396">Ha adatok áthelyezése az Azure SQL Data Warehouse &, a következő megfeleltetéseket használ az SQL-típus a .NET-típus, és ez fordítva is igaz.</span><span class="sxs-lookup"><span data-stu-id="5e177-396">When moving data to & from Azure SQL Data Warehouse, the following mappings are used from SQL type to .NET type and vice versa.</span></span>

<span data-ttu-id="5e177-397">Leképezése nem ugyanaz, mint a [SQL Server adattípus-hozzárendelése az ADO.NET](https://msdn.microsoft.com/library/cc716729.aspx).</span><span class="sxs-lookup"><span data-stu-id="5e177-397">The mapping is same as the [SQL Server Data Type Mapping for ADO.NET](https://msdn.microsoft.com/library/cc716729.aspx).</span></span>

| <span data-ttu-id="5e177-398">SQL Server adatbázismotor típusa</span><span class="sxs-lookup"><span data-stu-id="5e177-398">SQL Server Database Engine type</span></span> | <span data-ttu-id="5e177-399">.NET-keretrendszer típusa</span><span class="sxs-lookup"><span data-stu-id="5e177-399">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="5e177-400">bigint</span><span class="sxs-lookup"><span data-stu-id="5e177-400">bigint</span></span> |<span data-ttu-id="5e177-401">Int64</span><span class="sxs-lookup"><span data-stu-id="5e177-401">Int64</span></span> |
| <span data-ttu-id="5e177-402">Bináris</span><span class="sxs-lookup"><span data-stu-id="5e177-402">binary</span></span> |<span data-ttu-id="5e177-403">Byte]</span><span class="sxs-lookup"><span data-stu-id="5e177-403">Byte[]</span></span> |
| <span data-ttu-id="5e177-404">bit</span><span class="sxs-lookup"><span data-stu-id="5e177-404">bit</span></span> |<span data-ttu-id="5e177-405">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="5e177-405">Boolean</span></span> |
| <span data-ttu-id="5e177-406">Karakter</span><span class="sxs-lookup"><span data-stu-id="5e177-406">char</span></span> |<span data-ttu-id="5e177-407">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="5e177-407">String, Char[]</span></span> |
| <span data-ttu-id="5e177-408">Dátum</span><span class="sxs-lookup"><span data-stu-id="5e177-408">date</span></span> |<span data-ttu-id="5e177-409">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="5e177-409">DateTime</span></span> |
| <span data-ttu-id="5e177-410">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="5e177-410">Datetime</span></span> |<span data-ttu-id="5e177-411">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="5e177-411">DateTime</span></span> |
| <span data-ttu-id="5e177-412">datetime2</span><span class="sxs-lookup"><span data-stu-id="5e177-412">datetime2</span></span> |<span data-ttu-id="5e177-413">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="5e177-413">DateTime</span></span> |
| <span data-ttu-id="5e177-414">datetimeoffset</span><span class="sxs-lookup"><span data-stu-id="5e177-414">Datetimeoffset</span></span> |<span data-ttu-id="5e177-415">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="5e177-415">DateTimeOffset</span></span> |
| <span data-ttu-id="5e177-416">Decimális</span><span class="sxs-lookup"><span data-stu-id="5e177-416">Decimal</span></span> |<span data-ttu-id="5e177-417">Decimális</span><span class="sxs-lookup"><span data-stu-id="5e177-417">Decimal</span></span> |
| <span data-ttu-id="5e177-418">A FILESTREAM attribútum (varbinary(max))</span><span class="sxs-lookup"><span data-stu-id="5e177-418">FILESTREAM attribute (varbinary(max))</span></span> |<span data-ttu-id="5e177-419">Byte]</span><span class="sxs-lookup"><span data-stu-id="5e177-419">Byte[]</span></span> |
| <span data-ttu-id="5e177-420">Lebegőpontos</span><span class="sxs-lookup"><span data-stu-id="5e177-420">Float</span></span> |<span data-ttu-id="5e177-421">Dupla</span><span class="sxs-lookup"><span data-stu-id="5e177-421">Double</span></span> |
| <span data-ttu-id="5e177-422">Kép</span><span class="sxs-lookup"><span data-stu-id="5e177-422">image</span></span> |<span data-ttu-id="5e177-423">Byte]</span><span class="sxs-lookup"><span data-stu-id="5e177-423">Byte[]</span></span> |
| <span data-ttu-id="5e177-424">int</span><span class="sxs-lookup"><span data-stu-id="5e177-424">int</span></span> |<span data-ttu-id="5e177-425">Int32</span><span class="sxs-lookup"><span data-stu-id="5e177-425">Int32</span></span> |
| <span data-ttu-id="5e177-426">pénz</span><span class="sxs-lookup"><span data-stu-id="5e177-426">money</span></span> |<span data-ttu-id="5e177-427">Decimális</span><span class="sxs-lookup"><span data-stu-id="5e177-427">Decimal</span></span> |
| <span data-ttu-id="5e177-428">nchar</span><span class="sxs-lookup"><span data-stu-id="5e177-428">nchar</span></span> |<span data-ttu-id="5e177-429">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="5e177-429">String, Char[]</span></span> |
| <span data-ttu-id="5e177-430">ntext</span><span class="sxs-lookup"><span data-stu-id="5e177-430">ntext</span></span> |<span data-ttu-id="5e177-431">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="5e177-431">String, Char[]</span></span> |
| <span data-ttu-id="5e177-432">Numerikus</span><span class="sxs-lookup"><span data-stu-id="5e177-432">numeric</span></span> |<span data-ttu-id="5e177-433">Decimális</span><span class="sxs-lookup"><span data-stu-id="5e177-433">Decimal</span></span> |
| <span data-ttu-id="5e177-434">nvarchar</span><span class="sxs-lookup"><span data-stu-id="5e177-434">nvarchar</span></span> |<span data-ttu-id="5e177-435">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="5e177-435">String, Char[]</span></span> |
| <span data-ttu-id="5e177-436">valós</span><span class="sxs-lookup"><span data-stu-id="5e177-436">real</span></span> |<span data-ttu-id="5e177-437">Egyetlen</span><span class="sxs-lookup"><span data-stu-id="5e177-437">Single</span></span> |
| <span data-ttu-id="5e177-438">ROWVERSION</span><span class="sxs-lookup"><span data-stu-id="5e177-438">rowversion</span></span> |<span data-ttu-id="5e177-439">Byte]</span><span class="sxs-lookup"><span data-stu-id="5e177-439">Byte[]</span></span> |
| <span data-ttu-id="5e177-440">smalldatetime</span><span class="sxs-lookup"><span data-stu-id="5e177-440">smalldatetime</span></span> |<span data-ttu-id="5e177-441">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="5e177-441">DateTime</span></span> |
| <span data-ttu-id="5e177-442">smallint</span><span class="sxs-lookup"><span data-stu-id="5e177-442">smallint</span></span> |<span data-ttu-id="5e177-443">Int16</span><span class="sxs-lookup"><span data-stu-id="5e177-443">Int16</span></span> |
| <span data-ttu-id="5e177-444">kis pénz típusú értéknél</span><span class="sxs-lookup"><span data-stu-id="5e177-444">smallmoney</span></span> |<span data-ttu-id="5e177-445">Decimális</span><span class="sxs-lookup"><span data-stu-id="5e177-445">Decimal</span></span> |
| <span data-ttu-id="5e177-446">sql_variant</span><span class="sxs-lookup"><span data-stu-id="5e177-446">sql_variant</span></span> |<span data-ttu-id="5e177-447">Objektum *</span><span class="sxs-lookup"><span data-stu-id="5e177-447">Object *</span></span> |
| <span data-ttu-id="5e177-448">Szöveg</span><span class="sxs-lookup"><span data-stu-id="5e177-448">text</span></span> |<span data-ttu-id="5e177-449">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="5e177-449">String, Char[]</span></span> |
| <span data-ttu-id="5e177-450">time</span><span class="sxs-lookup"><span data-stu-id="5e177-450">time</span></span> |<span data-ttu-id="5e177-451">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="5e177-451">TimeSpan</span></span> |
| <span data-ttu-id="5e177-452">időbélyeg</span><span class="sxs-lookup"><span data-stu-id="5e177-452">timestamp</span></span> |<span data-ttu-id="5e177-453">Byte]</span><span class="sxs-lookup"><span data-stu-id="5e177-453">Byte[]</span></span> |
| <span data-ttu-id="5e177-454">tinyint</span><span class="sxs-lookup"><span data-stu-id="5e177-454">tinyint</span></span> |<span data-ttu-id="5e177-455">Bájt</span><span class="sxs-lookup"><span data-stu-id="5e177-455">Byte</span></span> |
| <span data-ttu-id="5e177-456">egyedi azonosító</span><span class="sxs-lookup"><span data-stu-id="5e177-456">uniqueidentifier</span></span> |<span data-ttu-id="5e177-457">GUID</span><span class="sxs-lookup"><span data-stu-id="5e177-457">Guid</span></span> |
| <span data-ttu-id="5e177-458">varbinary</span><span class="sxs-lookup"><span data-stu-id="5e177-458">varbinary</span></span> |<span data-ttu-id="5e177-459">Byte]</span><span class="sxs-lookup"><span data-stu-id="5e177-459">Byte[]</span></span> |
| <span data-ttu-id="5e177-460">varchar</span><span class="sxs-lookup"><span data-stu-id="5e177-460">varchar</span></span> |<span data-ttu-id="5e177-461">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="5e177-461">String, Char[]</span></span> |
| <span data-ttu-id="5e177-462">xml</span><span class="sxs-lookup"><span data-stu-id="5e177-462">xml</span></span> |<span data-ttu-id="5e177-463">XML</span><span class="sxs-lookup"><span data-stu-id="5e177-463">Xml</span></span> |

<span data-ttu-id="5e177-464">A másolási tevékenység definíciójának fogadó adatkészletből oszlopok forrás adatkészletből oszlopokat is leképezheti.</span><span class="sxs-lookup"><span data-stu-id="5e177-464">You can also map columns from source dataset to columns from sink dataset in the copy activity definition.</span></span> <span data-ttu-id="5e177-465">További információkért lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="5e177-465">For details, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="json-examples-for-copying-data-to-and-from-sql-data-warehouse"></a><span data-ttu-id="5e177-466">Adatok másolása és az SQL Data Warehouse JSON példák</span><span class="sxs-lookup"><span data-stu-id="5e177-466">JSON examples for copying data to and from SQL Data Warehouse</span></span>
<span data-ttu-id="5e177-467">Az alábbi példák megadják minta JSON-definíciókat tartalmazzon, segítségével hozzon létre egy folyamatot [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="5e177-467">The following examples provide sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="5e177-468">Adatok másolása az Azure SQL Data warehouse-bA és az Azure Blob Storage mutatnak.</span><span class="sxs-lookup"><span data-stu-id="5e177-468">They show how to copy data to and from Azure SQL Data Warehouse and Azure Blob Storage.</span></span> <span data-ttu-id="5e177-469">Azonban az adatok átmásolhatók **közvetlenül** a forrásokban, sem a megadott nyelő [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) a másolási tevékenység során az Azure Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="5e177-469">However, data can be copied **directly** from any of sources to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>

### <a name="example-copy-data-from-azure-sql-data-warehouse-to-azure-blob"></a><span data-ttu-id="5e177-470">Példa: Adatok másolása az Azure SQL Data Warehouse az Azure-Blobba</span><span class="sxs-lookup"><span data-stu-id="5e177-470">Example: Copy data from Azure SQL Data Warehouse to Azure Blob</span></span>
<span data-ttu-id="5e177-471">A minta a következő adat-előállító entitások határozza meg:</span><span class="sxs-lookup"><span data-stu-id="5e177-471">The sample defines the following Data Factory entities:</span></span>

1. <span data-ttu-id="5e177-472">A társított szolgáltatás típusa [AzureSqlDW](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="5e177-472">A linked service of type [AzureSqlDW](#linked-service-properties).</span></span>
2. <span data-ttu-id="5e177-473">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="5e177-473">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="5e177-474">Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureSqlDWTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="5e177-474">An input [dataset](data-factory-create-datasets.md) of type [AzureSqlDWTable](#dataset-properties).</span></span>
4. <span data-ttu-id="5e177-475">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="5e177-475">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="5e177-476">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [SqlDWSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="5e177-476">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [SqlDWSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="5e177-477">A minta idősorozat (óránkénti, napi stb) adatainak másolása az Azure SQL Data Warehouse-adatbázis egy táblából egy blobba óránként.</span><span class="sxs-lookup"><span data-stu-id="5e177-477">The sample copies time-series (hourly, daily, etc.) data from a table in Azure SQL Data Warehouse database to a blob every hour.</span></span> <span data-ttu-id="5e177-478">A mintákat a következő szakaszok ismertetik ezeket a mintákat használt JSON-tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="5e177-478">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="5e177-479">**A társított szolgáltatásnak Azure SQL Data Warehouse:**</span><span class="sxs-lookup"><span data-stu-id="5e177-479">**Azure SQL Data Warehouse linked service:**</span></span>

```JSON
{
  "name": "AzureSqlDWLinkedService",
  "properties": {
    "type": "AzureSqlDW",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
<span data-ttu-id="5e177-480">**Az Azure Blob storage társított szolgáltatásnak:**</span><span class="sxs-lookup"><span data-stu-id="5e177-480">**Azure Blob storage linked service:**</span></span>

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
<span data-ttu-id="5e177-481">**Az SQL Data Warehouse bemeneti adatkészletet:**</span><span class="sxs-lookup"><span data-stu-id="5e177-481">**Azure SQL Data Warehouse input dataset:**</span></span>

<span data-ttu-id="5e177-482">A minta azt feltételezi, hogy létrehozott egy tábla "MyTable" az Azure SQL Data Warehouse és egy "timestampcolumn" nevű adatsorozat időadatok oszlopot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="5e177-482">The sample assumes you have created a table “MyTable” in Azure SQL Data Warehouse and it contains a column called “timestampcolumn” for time series data.</span></span>

<span data-ttu-id="5e177-483">"External" beállítása: "true" arról tájékoztatja a Data Factory szolgáltatásnak, hogy az adatkészlet külső data factoryval való és adat-előállító tevékenység nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="5e177-483">Setting “external”: ”true” informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

```JSON
{
  "name": "AzureSqlDWInput",
  "properties": {
    "type": "AzureSqlDWTable",
    "linkedServiceName": "AzureSqlDWLinkedService",
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
<span data-ttu-id="5e177-484">**Az Azure Blob kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="5e177-484">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="5e177-485">Adatot ír egy új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="5e177-485">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="5e177-486">A mappa elérési útját a BLOB a szelet által feldolgozott kezdési ideje alapján dinamikusan történik.</span><span class="sxs-lookup"><span data-stu-id="5e177-486">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="5e177-487">A mappa elérési útját használja, év, hónap, nap és a kezdési idő órában részeit.</span><span class="sxs-lookup"><span data-stu-id="5e177-487">The folder path uses year, month, day, and hours parts of the start time.</span></span>

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

<span data-ttu-id="5e177-488">**Másolási tevékenység során a folyamat SqlDWSource és BlobSink:**</span><span class="sxs-lookup"><span data-stu-id="5e177-488">**Copy activity in a pipeline with SqlDWSource and BlobSink:**</span></span>

<span data-ttu-id="5e177-489">A feldolgozási sor tartalmazza a másolási tevékenység, amely a bemeneti és kimeneti adatkészletek használatára van konfigurálva, és óránkénti futásra nem ütemezték.</span><span class="sxs-lookup"><span data-stu-id="5e177-489">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="5e177-490">Az adatcsatorna JSON-definícióból a **forrás** típusúra **SqlDWSource** és **fogadó** típusúra **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="5e177-490">In the pipeline JSON definition, the **source** type is set to **SqlDWSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="5e177-491">A megadott SQL-lekérdezést a **SqlReaderQuery** tulajdonság kiválasztása az adatok másolása az elmúlt órában.</span><span class="sxs-lookup"><span data-stu-id="5e177-491">The SQL query specified for the **SqlReaderQuery** property selects the data in the past hour to copy.</span></span>

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "AzureSQLDWtoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureSqlDWInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlDWSource",
            "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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
> [!NOTE]
> <span data-ttu-id="5e177-492">A példában **sqlReaderQuery** a SqlDWSource van megadva.</span><span class="sxs-lookup"><span data-stu-id="5e177-492">In the example, **sqlReaderQuery** is specified for the SqlDWSource.</span></span> <span data-ttu-id="5e177-493">A másolási tevékenység fut ez a lekérdezés az adatok lekérdezése az Azure SQL Data Warehouse forrás.</span><span class="sxs-lookup"><span data-stu-id="5e177-493">The Copy Activity runs this query against the Azure SQL Data Warehouse source to get the data.</span></span>
>
> <span data-ttu-id="5e177-494">Másik lehetőségként megadhat tárolt eljárás megadásával a **sqlReaderStoredProcedureName** és **storedProcedureParameters** (Ha a tárolt eljárás paraméterek fogadja el).</span><span class="sxs-lookup"><span data-stu-id="5e177-494">Alternatively, you can specify a stored procedure by specifying the **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if the stored procedure takes parameters).</span></span>
>
> <span data-ttu-id="5e177-495">Ha nem ad meg sqlReaderQuery vagy sqlReaderStoredProcedureName, az adatkészlet JSON struktúrában szakaszában meghatározott oszlopokat segítségével (válassza Oszlop1, column2 from tábla)-lekérdezés összeállításához az Azure SQL Data Warehouse futtatásához.</span><span class="sxs-lookup"><span data-stu-id="5e177-495">If you do not specify either sqlReaderQuery or sqlReaderStoredProcedureName, the columns defined in the structure section of the dataset JSON are used to build a query (select column1, column2 from mytable) to run against the Azure SQL Data Warehouse.</span></span> <span data-ttu-id="5e177-496">Az adatkészlet-definícióban nem rendelkezik a struktúra, ha minden kiválasztott oszlop. a táblából.</span><span class="sxs-lookup"><span data-stu-id="5e177-496">If the dataset definition does not have the structure, all columns are selected from the table.</span></span>
>
>

### <a name="example-copy-data-from-azure-blob-to-azure-sql-data-warehouse"></a><span data-ttu-id="5e177-497">Példa: Adatok másolása az Azure Blob az Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="5e177-497">Example: Copy data from Azure Blob to Azure SQL Data Warehouse</span></span>
<span data-ttu-id="5e177-498">A minta a következő adat-előállító entitások határozza meg:</span><span class="sxs-lookup"><span data-stu-id="5e177-498">The sample defines the following Data Factory entities:</span></span>

1. <span data-ttu-id="5e177-499">A társított szolgáltatás típusa [AzureSqlDW](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="5e177-499">A linked service of type [AzureSqlDW](#linked-service-properties).</span></span>
2. <span data-ttu-id="5e177-500">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="5e177-500">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="5e177-501">Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="5e177-501">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="5e177-502">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureSqlDWTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="5e177-502">An output [dataset](data-factory-create-datasets.md) of type [AzureSqlDWTable](#dataset-properties).</span></span>
5. <span data-ttu-id="5e177-503">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) és [SqlDWSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="5e177-503">A [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) and [SqlDWSink](#copy-activity-properties).</span></span>

<span data-ttu-id="5e177-504">A minta másolatok idősorozat adatok (óránként, naponta, stb.) az Azure blob-egy táblához, az Azure SQL Data Warehouse-adatbázis használati ideje minden órában.</span><span class="sxs-lookup"><span data-stu-id="5e177-504">The sample copies time-series data (hourly, daily, etc.) from Azure blob to a table in Azure SQL Data Warehouse database every hour.</span></span> <span data-ttu-id="5e177-505">A mintákat a következő szakaszok ismertetik ezeket a mintákat használt JSON-tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="5e177-505">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="5e177-506">**A társított szolgáltatásnak Azure SQL Data Warehouse:**</span><span class="sxs-lookup"><span data-stu-id="5e177-506">**Azure SQL Data Warehouse linked service:**</span></span>

```JSON
{
  "name": "AzureSqlDWLinkedService",
  "properties": {
    "type": "AzureSqlDW",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
<span data-ttu-id="5e177-507">**Az Azure Blob storage társított szolgáltatásnak:**</span><span class="sxs-lookup"><span data-stu-id="5e177-507">**Azure Blob storage linked service:**</span></span>

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
<span data-ttu-id="5e177-508">**Az Azure Blob bemeneti adatkészletet:**</span><span class="sxs-lookup"><span data-stu-id="5e177-508">**Azure Blob input dataset:**</span></span>

<span data-ttu-id="5e177-509">Adatok van felvett egy új blobból minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="5e177-509">Data is picked up from a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="5e177-510">A mappa elérési útját és nevét a BLOB dinamikusan értékeli ki a kezdési időt a szelet által feldolgozott alapján.</span><span class="sxs-lookup"><span data-stu-id="5e177-510">The folder path and file name for the blob are dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="5e177-511">A mappa elérési útját használja év, hónap és nap részét kezdési idejét, valamint fájl nevét a kezdő időpontja óra részét.</span><span class="sxs-lookup"><span data-stu-id="5e177-511">The folder path uses year, month, and day part of the start time and file name uses the hour part of the start time.</span></span> <span data-ttu-id="5e177-512">"external": "true" beállítás arról értesíti az, hogy ezt a táblázatot az adat-előállítóban külső, és egy tevékenység adat-előállító nem hozzák a Data Factory szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="5e177-512">“external”: “true” setting informs the Data Factory service that this table is external to the data factory and is not produced by an activity in the data factory.</span></span>

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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
<span data-ttu-id="5e177-513">**Az SQL Data Warehouse kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="5e177-513">**Azure SQL Data Warehouse output dataset:**</span></span>

<span data-ttu-id="5e177-514">A minta másolja az adatokat az Azure SQL Data Warehouse "MyTable" nevű tábla.</span><span class="sxs-lookup"><span data-stu-id="5e177-514">The sample copies data to a table named “MyTable” in Azure SQL Data Warehouse.</span></span> <span data-ttu-id="5e177-515">A tábla létrehozása az Azure SQL Data Warehouse az azonos számú oszlopot tartalmaz a Blob CSV-fájl várt.</span><span class="sxs-lookup"><span data-stu-id="5e177-515">Create the table in Azure SQL Data Warehouse with the same number of columns as you expect the Blob CSV file to contain.</span></span> <span data-ttu-id="5e177-516">Új sorok hozzáadásakor a tábla minden órában.</span><span class="sxs-lookup"><span data-stu-id="5e177-516">New rows are added to the table every hour.</span></span>

```JSON
{
  "name": "AzureSqlDWOutput",
  "properties": {
    "type": "AzureSqlDWTable",
    "linkedServiceName": "AzureSqlDWLinkedService",
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
<span data-ttu-id="5e177-517">**Másolási tevékenység során a folyamat BlobSource és SqlDWSink:**</span><span class="sxs-lookup"><span data-stu-id="5e177-517">**Copy activity in a pipeline with BlobSource and SqlDWSink:**</span></span>

<span data-ttu-id="5e177-518">A feldolgozási sor tartalmazza a másolási tevékenység, amely a bemeneti és kimeneti adatkészletek használatára van konfigurálva, és óránkénti futásra nem ütemezték.</span><span class="sxs-lookup"><span data-stu-id="5e177-518">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="5e177-519">Az adatcsatorna JSON-definícióból a **forrás** típusúra **BlobSource** és **fogadó** típusúra **SqlDWSink**.</span><span class="sxs-lookup"><span data-stu-id="5e177-519">In the pipeline JSON definition, the **source** type is set to **BlobSource** and **sink** type is set to **SqlDWSink**.</span></span>

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoSQLDW",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlDWOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource",
            "blobColumnSeparators": ","
          },
          "sink": {
            "type": "SqlDWSink",
            "allowPolyBase": true
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
<span data-ttu-id="5e177-520">Útmutatást megtalálja az [1 TB-os betöltése az Azure SQL Data Warehouse a 15 perc Azure Data Factory](data-factory-load-sql-data-warehouse.md) és [adatok betöltése az Azure Data Factory](../sql-data-warehouse/sql-data-warehouse-get-started-load-with-azure-data-factory.md) cikk az Azure SQL Data Warehouse-dokumentációban.</span><span class="sxs-lookup"><span data-stu-id="5e177-520">For a walkthrough, see the see [Load 1 TB into Azure SQL Data Warehouse under 15 minutes with Azure Data Factory](data-factory-load-sql-data-warehouse.md) and [Load data with Azure Data Factory](../sql-data-warehouse/sql-data-warehouse-get-started-load-with-azure-data-factory.md) article in the Azure SQL Data Warehouse documentation.</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="5e177-521">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="5e177-521">Performance and Tuning</span></span>
<span data-ttu-id="5e177-522">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) tájékozódhat az kulcsfontosságú szerepet játszik adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon optimalizálhatja azt, hogy hatás teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="5e177-522">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
