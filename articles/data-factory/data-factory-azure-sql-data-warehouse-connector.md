---
title: az Azure SQL Data Warehouse aaaCopy adatok |} Microsoft Docs
description: "Megtudhatja, hogyan toocopy adatokat az Azure SQL Data Warehouse Azure Data Factory használatával"
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
ms.openlocfilehash: 75bfcf3c99844fc1297ca500107da23cf875e41f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tooand-from-azure-sql-data-warehouse-using-azure-data-factory"></a><span data-ttu-id="6cd21-103">Adatok tooand másolása az Azure SQL Data Warehouse Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="6cd21-103">Copy data tooand from Azure SQL Data Warehouse using Azure Data Factory</span></span>
<span data-ttu-id="6cd21-104">Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove adatok Azure SQL Data Warehouse és a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="6cd21-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data to/from Azure SQL Data Warehouse.</span></span> <span data-ttu-id="6cd21-105">-Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.</span><span class="sxs-lookup"><span data-stu-id="6cd21-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>  

> [!TIP]
> <span data-ttu-id="6cd21-106">tooachieve a legjobb teljesítményt, az Azure SQL Data Warehouse PolyBase tooload adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="6cd21-106">tooachieve best performance, use PolyBase tooload data into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="6cd21-107">Hello [használja a PolyBase tooload adatokat az Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) szakasz részleteket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="6cd21-107">hello [Use PolyBase tooload data into Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) section has details.</span></span> <span data-ttu-id="6cd21-108">A használati esetek bemutatóért lásd: [1 TB-os betöltése az Azure SQL Data Warehouse a 15 perc Azure Data Factory](data-factory-load-sql-data-warehouse.md).</span><span class="sxs-lookup"><span data-stu-id="6cd21-108">For a walkthrough with a use case, see [Load 1 TB into Azure SQL Data Warehouse under 15 minutes with Azure Data Factory](data-factory-load-sql-data-warehouse.md).</span></span>

## <a name="supported-scenarios"></a><span data-ttu-id="6cd21-109">Támogatott helyzetek</span><span class="sxs-lookup"><span data-stu-id="6cd21-109">Supported scenarios</span></span>
<span data-ttu-id="6cd21-110">Adatokat másolhat **az Azure SQL Data Warehouse** toohello a következő adatokat tárolja:</span><span class="sxs-lookup"><span data-stu-id="6cd21-110">You can copy data **from Azure SQL Data Warehouse** toohello following data stores:</span></span>

[!INCLUDE [data-factory-supported-sinks](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="6cd21-111">Adatok másolása a következő adatokat tárolja hello **tooAzure SQL Data Warehouse**:</span><span class="sxs-lookup"><span data-stu-id="6cd21-111">You can copy data from hello following data stores **tooAzure SQL Data Warehouse**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

> [!TIP]
> <span data-ttu-id="6cd21-112">Ha az adatok másolása az SQL Server vagy az Azure SQL Database tooAzure SQL Data Warehouse, ha hello tábla nem létezik a hello céltár, adat-előállító automatikusan segítségével hozhat létre hello tábla az SQL Data Warehouse hello séma hello tábla hello forrás adattároló.</span><span class="sxs-lookup"><span data-stu-id="6cd21-112">When copying data from SQL Server or Azure SQL Database tooAzure SQL Data Warehouse, if hello table does not exist in hello destination store, Data Factory can automatically create hello table in SQL Data Warehouse by using hello schema of hello table in hello source data store.</span></span> <span data-ttu-id="6cd21-113">Lásd: [tábla létrehozásához automatikus](#auto-table-creation) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="6cd21-113">See [Auto table creation](#auto-table-creation) for details.</span></span>

## <a name="supported-authentication-type"></a><span data-ttu-id="6cd21-114">Támogatott hitelesítési típushoz</span><span class="sxs-lookup"><span data-stu-id="6cd21-114">Supported authentication type</span></span>
<span data-ttu-id="6cd21-115">Az Azure SQL Data Warehouse összekötő alapszintű hitelesítés támogatása.</span><span class="sxs-lookup"><span data-stu-id="6cd21-115">Azure SQL Data Warehouse connector support basic authentication.</span></span>

## <a name="getting-started"></a><span data-ttu-id="6cd21-116">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="6cd21-116">Getting started</span></span>
<span data-ttu-id="6cd21-117">A másolási tevékenység, amely helyezi át az adatokat az Azure SQL Data Warehouse és a különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="6cd21-117">You can create a pipeline with a copy activity that moves data to/from an Azure SQL Data Warehouse by using different tools/APIs.</span></span>

<span data-ttu-id="6cd21-118">hello legegyszerűbb módja toocreate egy folyamatot, amely másolja az adatokat az Azure SQL Data Warehouse és a rendszer toouse hello másolása varázsló.</span><span class="sxs-lookup"><span data-stu-id="6cd21-118">hello easiest way toocreate a pipeline that copies data to/from Azure SQL Data Warehouse is toouse hello Copy data wizard.</span></span> <span data-ttu-id="6cd21-119">Lásd: [oktatóanyag: adatok betöltése az SQL Data Warehouse Data Factory](../sql-data-warehouse/sql-data-warehouse-load-with-data-factory.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.</span><span class="sxs-lookup"><span data-stu-id="6cd21-119">See [Tutorial: Load data into SQL Data Warehouse with Data Factory](../sql-data-warehouse/sql-data-warehouse-load-with-data-factory.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="6cd21-120">Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**.</span><span class="sxs-lookup"><span data-stu-id="6cd21-120">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="6cd21-121">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.</span><span class="sxs-lookup"><span data-stu-id="6cd21-121">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span>

<span data-ttu-id="6cd21-122">Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:</span><span class="sxs-lookup"><span data-stu-id="6cd21-122">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="6cd21-123">Hozzon létre egy **adat-előállító**.</span><span class="sxs-lookup"><span data-stu-id="6cd21-123">Create a **data factory**.</span></span> <span data-ttu-id="6cd21-124">Egy adat-előállító tartalmazhat egy vagy több folyamatok.</span><span class="sxs-lookup"><span data-stu-id="6cd21-124">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="6cd21-125">Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="6cd21-125">Create **linked services** toolink input and output data stores tooyour data factory.</span></span> <span data-ttu-id="6cd21-126">Adatok másolása az Azure blob storage tooan Azure SQL adatraktárban, akkor hozzon létre például két összekapcsolt szolgáltatások toolink az Azure storage-fiók és az Azure SQL data warehouse tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="6cd21-126">For example, if you are copying data from an Azure blob storage tooan Azure SQL data warehouse, you create two linked services toolink your Azure storage account and Azure SQL data warehouse tooyour data factory.</span></span> <span data-ttu-id="6cd21-127">Csatolt szolgáltatás tulajdonságait, amelyek adott tooAzure SQL Data Warehouse, lásd: [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="6cd21-127">For linked service properties that are specific tooAzure SQL Data Warehouse, see [linked service properties](#linked-service-properties) section.</span></span> 
3. <span data-ttu-id="6cd21-128">Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet.</span><span class="sxs-lookup"><span data-stu-id="6cd21-128">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> <span data-ttu-id="6cd21-129">Hello utolsó lépésében említett hello például létrehoz egy adatkészlet toospecify hello blobtárolót és hello bemeneti adatokat tartalmazó mappát.</span><span class="sxs-lookup"><span data-stu-id="6cd21-129">In hello example mentioned in hello last step, you create a dataset toospecify hello blob container and folder that contains hello input data.</span></span> <span data-ttu-id="6cd21-130">És egy másik dataset toospecify hello tábla hello Azure SQL data warehouse hello blob-tároló átmásolva hello adatokat tartalmazó hozzon létre.</span><span class="sxs-lookup"><span data-stu-id="6cd21-130">And, you create another dataset toospecify hello table in hello Azure SQL data warehouse that holds hello data copied from hello blob storage.</span></span> <span data-ttu-id="6cd21-131">Adatkészlet tulajdonságai, amelyek adott tooAzure SQL Data Warehouse, lásd: [adatkészlet tulajdonságai](#dataset-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="6cd21-131">For dataset properties that are specific tooAzure SQL Data Warehouse, see [dataset properties](#dataset-properties) section.</span></span>
4. <span data-ttu-id="6cd21-132">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="6cd21-132">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="6cd21-133">A korábban említett hello példában BlobSource forrás-és SqlDWSink akár használhatja a fogadó hello másolási tevékenységhez.</span><span class="sxs-lookup"><span data-stu-id="6cd21-133">In hello example mentioned earlier, you use BlobSource as a source and SqlDWSink as a sink for hello copy activity.</span></span> <span data-ttu-id="6cd21-134">Hasonlóképpen a Blob Storage Azure SQL Data Warehouse tooAzure másolása, használható SqlDWSource és BlobSink hello másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="6cd21-134">Similarly, if you are copying from Azure SQL Data Warehouse tooAzure Blob Storage, you use SqlDWSource and BlobSink in hello copy activity.</span></span> <span data-ttu-id="6cd21-135">A másolási tevékenység tulajdonságait, amelyek adott tooAzure SQL Data Warehouse, lásd: [tevékenység Tulajdonságok másolása](#copy-activity-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="6cd21-135">For copy activity properties that are specific tooAzure SQL Data Warehouse, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="6cd21-136">További információkért hogyan toouse egy adatok tárolót, mint a forrás- és a fogadó hivatkozásra hello az adattároló hello előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="6cd21-136">For details on how toouse a data store as a source or a sink, click hello link in hello previous section for your data store.</span></span>

<span data-ttu-id="6cd21-137">Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="6cd21-137">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="6cd21-138">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="6cd21-138">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="6cd21-139">Az adat-előállító entitások, amelyek az Azure SQL Data Warehouse használt toocopy adatok JSON-definíciók minták, lásd: [JSON példák](#json-examples-for-copying-data-to-and-from-sql-data-warehouse) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="6cd21-139">For samples with JSON definitions for Data Factory entities that are used toocopy data to/from an Azure SQL Data Warehouse, see [JSON examples](#json-examples-for-copying-data-to-and-from-sql-data-warehouse) section of this article.</span></span>

<span data-ttu-id="6cd21-140">a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooAzure SQL Data Warehouse részleteit tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="6cd21-140">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooAzure SQL Data Warehouse:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="6cd21-141">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="6cd21-141">Linked service properties</span></span>
<span data-ttu-id="6cd21-142">a következő táblázat hello biztosít JSON elemek adott tooAzure SQL Data Warehouse kapcsolódó szolgáltatás leírását.</span><span class="sxs-lookup"><span data-stu-id="6cd21-142">hello following table provides description for JSON elements specific tooAzure SQL Data Warehouse linked service.</span></span>

| <span data-ttu-id="6cd21-143">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="6cd21-143">Property</span></span> | <span data-ttu-id="6cd21-144">Leírás</span><span class="sxs-lookup"><span data-stu-id="6cd21-144">Description</span></span> | <span data-ttu-id="6cd21-145">Szükséges</span><span class="sxs-lookup"><span data-stu-id="6cd21-145">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6cd21-146">type</span><span class="sxs-lookup"><span data-stu-id="6cd21-146">type</span></span> |<span data-ttu-id="6cd21-147">hello type tulajdonságot kell beállítani: **AzureSqlDW**</span><span class="sxs-lookup"><span data-stu-id="6cd21-147">hello type property must be set to: **AzureSqlDW**</span></span> |<span data-ttu-id="6cd21-148">Igen</span><span class="sxs-lookup"><span data-stu-id="6cd21-148">Yes</span></span> |
| <span data-ttu-id="6cd21-149">connectionString</span><span class="sxs-lookup"><span data-stu-id="6cd21-149">connectionString</span></span> |<span data-ttu-id="6cd21-150">Adjon meg információt hello connectionString tulajdonság szükséges tooconnect toohello Azure SQL Data Warehouse-példányhoz.</span><span class="sxs-lookup"><span data-stu-id="6cd21-150">Specify information needed tooconnect toohello Azure SQL Data Warehouse instance for hello connectionString property.</span></span> <span data-ttu-id="6cd21-151">Csak az alapszintű hitelesítést is támogatja.</span><span class="sxs-lookup"><span data-stu-id="6cd21-151">Only basic authentication is supported.</span></span> |<span data-ttu-id="6cd21-152">Igen</span><span class="sxs-lookup"><span data-stu-id="6cd21-152">Yes</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="6cd21-153">Konfigurálása [Azure SQL Database-tűzfal](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) és adatbázis-kiszolgáló túl hello[engedélyezése az Azure-szolgáltatások tooaccess hello server](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure).</span><span class="sxs-lookup"><span data-stu-id="6cd21-153">Configure [Azure SQL Database Firewall](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) and hello database server too[allow Azure Services tooaccess hello server](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure).</span></span> <span data-ttu-id="6cd21-154">Ezenkívül az adatok tooAzure SQL Data Warehouse másolása a külső Azure többek között a helyszíni adatforrásokból a data factory átjáróval, az IP-címtartományt, amely az SQL Data Warehouse adatok tooAzure küld hello gép konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="6cd21-154">Additionally, if you are copying data tooAzure SQL Data Warehouse from outside Azure including from on-premises data sources with data factory gateway, configure appropriate IP address range for hello machine that is sending data tooAzure SQL Data Warehouse.</span></span>

## <a name="dataset-properties"></a><span data-ttu-id="6cd21-155">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="6cd21-155">Dataset properties</span></span>
<span data-ttu-id="6cd21-156">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="6cd21-156">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="6cd21-157">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="6cd21-157">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="6cd21-158">hello typeProperties szakasz más adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti.</span><span class="sxs-lookup"><span data-stu-id="6cd21-158">hello typeProperties section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="6cd21-159">Hello **typeProperties** típusú hello adatkészlet szakasz **AzureSqlDWTable** rendelkezik hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="6cd21-159">hello **typeProperties** section for hello dataset of type **AzureSqlDWTable** has hello following properties:</span></span>

| <span data-ttu-id="6cd21-160">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="6cd21-160">Property</span></span> | <span data-ttu-id="6cd21-161">Leírás</span><span class="sxs-lookup"><span data-stu-id="6cd21-161">Description</span></span> | <span data-ttu-id="6cd21-162">Szükséges</span><span class="sxs-lookup"><span data-stu-id="6cd21-162">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6cd21-163">tableName</span><span class="sxs-lookup"><span data-stu-id="6cd21-163">tableName</span></span> |<span data-ttu-id="6cd21-164">Hello tábla vagy nézet hello Azure SQL Data Warehouse-adatbázis, amely a társított szolgáltatás hello nevére hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="6cd21-164">Name of hello table or view in hello Azure SQL Data Warehouse database that hello linked service refers to.</span></span> |<span data-ttu-id="6cd21-165">Igen</span><span class="sxs-lookup"><span data-stu-id="6cd21-165">Yes</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="6cd21-166">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="6cd21-166">Copy activity properties</span></span>
<span data-ttu-id="6cd21-167">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="6cd21-167">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="6cd21-168">Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="6cd21-168">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

> [!NOTE]
> <span data-ttu-id="6cd21-169">hello másolási tevékenység során csak egy bemenettel rendelkezik, és csak egy kimenetet.</span><span class="sxs-lookup"><span data-stu-id="6cd21-169">hello Copy Activity takes only one input and produces only one output.</span></span>

<span data-ttu-id="6cd21-170">Mivel a hello hello tevékenység részében typeProperties rendelkezésre álló tulajdonságok tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="6cd21-170">Whereas, properties available in hello typeProperties section of hello activity vary with each activity type.</span></span> <span data-ttu-id="6cd21-171">A másolási tevékenység során két érték források és mosdók hello típusától függően.</span><span class="sxs-lookup"><span data-stu-id="6cd21-171">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

### <a name="sqldwsource"></a><span data-ttu-id="6cd21-172">SqlDWSource</span><span class="sxs-lookup"><span data-stu-id="6cd21-172">SqlDWSource</span></span>
<span data-ttu-id="6cd21-173">Ha a forrás típusa van **SqlDWSource**, hello a következő tulajdonságok érhetők el **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="6cd21-173">When source is of type **SqlDWSource**, hello following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="6cd21-174">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="6cd21-174">Property</span></span> | <span data-ttu-id="6cd21-175">Leírás</span><span class="sxs-lookup"><span data-stu-id="6cd21-175">Description</span></span> | <span data-ttu-id="6cd21-176">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="6cd21-176">Allowed values</span></span> | <span data-ttu-id="6cd21-177">Szükséges</span><span class="sxs-lookup"><span data-stu-id="6cd21-177">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="6cd21-178">sqlReaderQuery</span><span class="sxs-lookup"><span data-stu-id="6cd21-178">sqlReaderQuery</span></span> |<span data-ttu-id="6cd21-179">Hello egyéni lekérdezés tooread adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="6cd21-179">Use hello custom query tooread data.</span></span> |<span data-ttu-id="6cd21-180">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="6cd21-180">SQL query string.</span></span> <span data-ttu-id="6cd21-181">Például: Válasszon * from tábla.</span><span class="sxs-lookup"><span data-stu-id="6cd21-181">For example: select * from MyTable.</span></span> |<span data-ttu-id="6cd21-182">Nem</span><span class="sxs-lookup"><span data-stu-id="6cd21-182">No</span></span> |
| <span data-ttu-id="6cd21-183">sqlReaderStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="6cd21-183">sqlReaderStoredProcedureName</span></span> |<span data-ttu-id="6cd21-184">Hello neve tárolt eljárást, amely hello forrástábla olvassa be az adatokat.</span><span class="sxs-lookup"><span data-stu-id="6cd21-184">Name of hello stored procedure that reads data from hello source table.</span></span> |<span data-ttu-id="6cd21-185">Hello neve tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="6cd21-185">Name of hello stored procedure.</span></span> <span data-ttu-id="6cd21-186">hello utolsó SQL-utasítás hello tárolt eljárás SELECT utasítással kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6cd21-186">hello last SQL statement must be a SELECT statement in hello stored procedure.</span></span> |<span data-ttu-id="6cd21-187">Nem</span><span class="sxs-lookup"><span data-stu-id="6cd21-187">No</span></span> |
| <span data-ttu-id="6cd21-188">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="6cd21-188">storedProcedureParameters</span></span> |<span data-ttu-id="6cd21-189">Hello paramétereinek tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="6cd21-189">Parameters for hello stored procedure.</span></span> |<span data-ttu-id="6cd21-190">A név/érték párok.</span><span class="sxs-lookup"><span data-stu-id="6cd21-190">Name/value pairs.</span></span> <span data-ttu-id="6cd21-191">Nevek és a kis-és paraméterek meg kell egyeznie hello nevét és a kis-és nagybetűhasználat hello tárolt eljárás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="6cd21-191">Names and casing of parameters must match hello names and casing of hello stored procedure parameters.</span></span> |<span data-ttu-id="6cd21-192">Nem</span><span class="sxs-lookup"><span data-stu-id="6cd21-192">No</span></span> |

<span data-ttu-id="6cd21-193">Ha hello **sqlReaderQuery** megadott hello SqlDWSource, hello másolási tevékenység fut ez a lekérdezés hello Azure SQL Data Warehouse forrásadatok tooget hello.</span><span class="sxs-lookup"><span data-stu-id="6cd21-193">If hello **sqlReaderQuery** is specified for hello SqlDWSource, hello Copy Activity runs this query against hello Azure SQL Data Warehouse source tooget hello data.</span></span>

<span data-ttu-id="6cd21-194">Másik lehetőségként megadhat tárolt eljárás hello megadásával **sqlReaderStoredProcedureName** és **storedProcedureParameters** (ha hello tárolt eljárás paraméterek fogadja el).</span><span class="sxs-lookup"><span data-stu-id="6cd21-194">Alternatively, you can specify a stored procedure by specifying hello **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if hello stored procedure takes parameters).</span></span>

<span data-ttu-id="6cd21-195">Ha nem ad meg sqlReaderQuery vagy sqlReaderStoredProcedureName, hello adatkészlet JSON hello struktúra szakaszban meghatározott hello oszlopok használt toobuild egy lekérdezés toorun elleni hello Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="6cd21-195">If you do not specify either sqlReaderQuery or sqlReaderStoredProcedureName, hello columns defined in hello structure section of hello dataset JSON are used toobuild a query toorun against hello Azure SQL Data Warehouse.</span></span> <span data-ttu-id="6cd21-196">Példa: `select column1, column2 from mytable`.</span><span class="sxs-lookup"><span data-stu-id="6cd21-196">Example: `select column1, column2 from mytable`.</span></span> <span data-ttu-id="6cd21-197">Hello adatkészlet definíciója nem rendelkezik hello struktúra, ha minden kiválasztott oszlop. a hello táblából.</span><span class="sxs-lookup"><span data-stu-id="6cd21-197">If hello dataset definition does not have hello structure, all columns are selected from hello table.</span></span>

#### <a name="sqldwsource-example"></a><span data-ttu-id="6cd21-198">SqlDWSource – példa</span><span class="sxs-lookup"><span data-stu-id="6cd21-198">SqlDWSource example</span></span>

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
<span data-ttu-id="6cd21-199">**hello tárolt eljárás definíciója:**</span><span class="sxs-lookup"><span data-stu-id="6cd21-199">**hello stored procedure definition:**</span></span>

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

### <a name="sqldwsink"></a><span data-ttu-id="6cd21-200">SqlDWSink</span><span class="sxs-lookup"><span data-stu-id="6cd21-200">SqlDWSink</span></span>
<span data-ttu-id="6cd21-201">**SqlDWSink** következő tulajdonságai hello támogatja:</span><span class="sxs-lookup"><span data-stu-id="6cd21-201">**SqlDWSink** supports hello following properties:</span></span>

| <span data-ttu-id="6cd21-202">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="6cd21-202">Property</span></span> | <span data-ttu-id="6cd21-203">Leírás</span><span class="sxs-lookup"><span data-stu-id="6cd21-203">Description</span></span> | <span data-ttu-id="6cd21-204">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="6cd21-204">Allowed values</span></span> | <span data-ttu-id="6cd21-205">Szükséges</span><span class="sxs-lookup"><span data-stu-id="6cd21-205">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="6cd21-206">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="6cd21-206">sqlWriterCleanupScript</span></span> |<span data-ttu-id="6cd21-207">Adja meg a másolási tevékenység tooexecute vonatkozó lekérdezést úgy, hogy egy adott szelet adatait.</span><span class="sxs-lookup"><span data-stu-id="6cd21-207">Specify a query for Copy Activity tooexecute such that data of a specific slice is cleaned up.</span></span> <span data-ttu-id="6cd21-208">További információkért lásd: [ismételhetőség szakasz](#repeatability-during-copy).</span><span class="sxs-lookup"><span data-stu-id="6cd21-208">For details, see [repeatability section](#repeatability-during-copy).</span></span> |<span data-ttu-id="6cd21-209">A lekérdezési utasítást.</span><span class="sxs-lookup"><span data-stu-id="6cd21-209">A query statement.</span></span> |<span data-ttu-id="6cd21-210">Nem</span><span class="sxs-lookup"><span data-stu-id="6cd21-210">No</span></span> |
| <span data-ttu-id="6cd21-211">allowPolyBase</span><span class="sxs-lookup"><span data-stu-id="6cd21-211">allowPolyBase</span></span> |<span data-ttu-id="6cd21-212">Azt jelzi, hogy (ha alkalmazható) PolyBase toouse BULKINSERT mechanizmus helyett.</span><span class="sxs-lookup"><span data-stu-id="6cd21-212">Indicates whether toouse PolyBase (when applicable) instead of BULKINSERT mechanism.</span></span> <br/><br/> <span data-ttu-id="6cd21-213">**A PolyBase használata javasolt módja tooload adatokat az SQL Data Warehouse hello.**</span><span class="sxs-lookup"><span data-stu-id="6cd21-213">**Using PolyBase is hello recommended way tooload data into SQL Data Warehouse.**</span></span> <span data-ttu-id="6cd21-214">Lásd: [használja a PolyBase tooload adatokat az Azure SQL Data Warehouse](#use-polybase-to-load-data-into-azure-sql-data-warehouse) szakaszban a korlátozások és részleteit.</span><span class="sxs-lookup"><span data-stu-id="6cd21-214">See [Use PolyBase tooload data into Azure SQL Data Warehouse](#use-polybase-to-load-data-into-azure-sql-data-warehouse) section for constraints and details.</span></span> |<span data-ttu-id="6cd21-215">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="6cd21-215">True</span></span> <br/><span data-ttu-id="6cd21-216">Hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="6cd21-216">False (default)</span></span> |<span data-ttu-id="6cd21-217">Nem</span><span class="sxs-lookup"><span data-stu-id="6cd21-217">No</span></span> |
| <span data-ttu-id="6cd21-218">kapcsolódó polyBaseSettings</span><span class="sxs-lookup"><span data-stu-id="6cd21-218">polyBaseSettings</span></span> |<span data-ttu-id="6cd21-219">Egy csoport lehet megadni, ha hello tulajdonságok **allowPolybase** tulajdonsága túl**igaz**.</span><span class="sxs-lookup"><span data-stu-id="6cd21-219">A group of properties that can be specified when hello **allowPolybase** property is set too**true**.</span></span> |&nbsp; |<span data-ttu-id="6cd21-220">Nem</span><span class="sxs-lookup"><span data-stu-id="6cd21-220">No</span></span> |
| <span data-ttu-id="6cd21-221">rejectValue</span><span class="sxs-lookup"><span data-stu-id="6cd21-221">rejectValue</span></span> |<span data-ttu-id="6cd21-222">Megadja a hello számát vagy a sorok, amelyekre el lehet utasítani, mielőtt hello lekérdezés nem sikerült százalékát.</span><span class="sxs-lookup"><span data-stu-id="6cd21-222">Specifies hello number or percentage of rows that can be rejected before hello query fails.</span></span> <br/><br/><span data-ttu-id="6cd21-223">Bővebben a hello PolyBase elutasítása hello lehetőségeit további **argumentumok** szakasza [külső tábla létrehozása (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) témakör.</span><span class="sxs-lookup"><span data-stu-id="6cd21-223">Learn more about hello PolyBase’s reject options in hello **Arguments** section of [CREATE EXTERNAL TABLE (Transact-SQL)](https://msdn.microsoft.com/library/dn935021.aspx) topic.</span></span> |<span data-ttu-id="6cd21-224">0 (alapértelmezés), 1, 2...</span><span class="sxs-lookup"><span data-stu-id="6cd21-224">0 (default), 1, 2, …</span></span> |<span data-ttu-id="6cd21-225">Nem</span><span class="sxs-lookup"><span data-stu-id="6cd21-225">No</span></span> |
| <span data-ttu-id="6cd21-226">rejectType</span><span class="sxs-lookup"><span data-stu-id="6cd21-226">rejectType</span></span> |<span data-ttu-id="6cd21-227">Meghatározza, hogy hello rejectValue beállítás konstans értéket vagy százalékában van megadva.</span><span class="sxs-lookup"><span data-stu-id="6cd21-227">Specifies whether hello rejectValue option is specified as a literal value or a percentage.</span></span> |<span data-ttu-id="6cd21-228">Érték (alapértelmezett), százalékos aránya</span><span class="sxs-lookup"><span data-stu-id="6cd21-228">Value (default), Percentage</span></span> |<span data-ttu-id="6cd21-229">Nem</span><span class="sxs-lookup"><span data-stu-id="6cd21-229">No</span></span> |
| <span data-ttu-id="6cd21-230">rejectSampleValue</span><span class="sxs-lookup"><span data-stu-id="6cd21-230">rejectSampleValue</span></span> |<span data-ttu-id="6cd21-231">Határozza meg, hogy hello sorok tooretrieve előtt hello PolyBase újraszámítja a visszautasított sorok hello százalékát.</span><span class="sxs-lookup"><span data-stu-id="6cd21-231">Determines hello number of rows tooretrieve before hello PolyBase recalculates hello percentage of rejected rows.</span></span> |<span data-ttu-id="6cd21-232">1, 2, …</span><span class="sxs-lookup"><span data-stu-id="6cd21-232">1, 2, …</span></span> |<span data-ttu-id="6cd21-233">Igen, ha **rejectType** van **százalékos aránya**</span><span class="sxs-lookup"><span data-stu-id="6cd21-233">Yes, if **rejectType** is **percentage**</span></span> |
| <span data-ttu-id="6cd21-234">useTypeDefault</span><span class="sxs-lookup"><span data-stu-id="6cd21-234">useTypeDefault</span></span> |<span data-ttu-id="6cd21-235">Itt adhatja meg, hogyan értékek a hiányzó toohandle tagolt-e szövegfájlok amikor PolyBase hello szövegfájlból kér le adatokat.</span><span class="sxs-lookup"><span data-stu-id="6cd21-235">Specifies how toohandle missing values in delimited text files when PolyBase retrieves data from hello text file.</span></span><br/><br/><span data-ttu-id="6cd21-236">Ezt a tulajdonságot hello argumentumok című szakaszában olvashat [létrehozása külső FÁJLFORMÁTUM (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx).</span><span class="sxs-lookup"><span data-stu-id="6cd21-236">Learn more about this property from hello Arguments section in [CREATE EXTERNAL FILE FORMAT (Transact-SQL)](https://msdn.microsoft.com/library/dn935026.aspx).</span></span> |<span data-ttu-id="6cd21-237">IGAZ, hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="6cd21-237">True, False (default)</span></span> |<span data-ttu-id="6cd21-238">Nem</span><span class="sxs-lookup"><span data-stu-id="6cd21-238">No</span></span> |
| <span data-ttu-id="6cd21-239">WriteBatchSize</span><span class="sxs-lookup"><span data-stu-id="6cd21-239">writeBatchSize</span></span> |<span data-ttu-id="6cd21-240">Adatok beszúrása hello SQL táblázatba, amikor hello puffer mérete eléri writeBatchSize</span><span class="sxs-lookup"><span data-stu-id="6cd21-240">Inserts data into hello SQL table when hello buffer size reaches writeBatchSize</span></span> |<span data-ttu-id="6cd21-241">Egész szám (sorok száma)</span><span class="sxs-lookup"><span data-stu-id="6cd21-241">Integer (number of rows)</span></span> |<span data-ttu-id="6cd21-242">Nem (alapértelmezett: 10000)</span><span class="sxs-lookup"><span data-stu-id="6cd21-242">No (default: 10000)</span></span> |
| <span data-ttu-id="6cd21-243">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="6cd21-243">writeBatchTimeout</span></span> |<span data-ttu-id="6cd21-244">Várnia kell az hello kötegelt beszúrási művelet toocomplete előtt azt az időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="6cd21-244">Wait time for hello batch insert operation toocomplete before it times out.</span></span> |<span data-ttu-id="6cd21-245">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="6cd21-245">timespan</span></span><br/><br/> <span data-ttu-id="6cd21-246">Példa: "00: 30:00" (30 perc).</span><span class="sxs-lookup"><span data-stu-id="6cd21-246">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="6cd21-247">Nem</span><span class="sxs-lookup"><span data-stu-id="6cd21-247">No</span></span> |

#### <a name="sqldwsink-example"></a><span data-ttu-id="6cd21-248">SqlDWSink – példa</span><span class="sxs-lookup"><span data-stu-id="6cd21-248">SqlDWSink example</span></span>

```JSON
"sink": {
    "type": "SqlDWSink",
    "allowPolyBase": true
}
```

## <a name="use-polybase-tooload-data-into-azure-sql-data-warehouse"></a><span data-ttu-id="6cd21-249">Az Azure SQL Data Warehouse PolyBase tooload adatok használata</span><span class="sxs-lookup"><span data-stu-id="6cd21-249">Use PolyBase tooload data into Azure SQL Data Warehouse</span></span>
<span data-ttu-id="6cd21-250">Használatával  **[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide)**  egy hatékony módszer a nagy mennyiségű adatok betöltését az Azure SQL Data Warehouse nagy átviteli sebességgel.</span><span class="sxs-lookup"><span data-stu-id="6cd21-250">Using **[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide)** is an efficient way of loading large amount of data into Azure SQL Data Warehouse with high throughput.</span></span> <span data-ttu-id="6cd21-251">A nagy nyereség hello átviteli sebességének hello alapértelmezett BULKINSERT mechanizmus helyett a PolyBase használatával tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="6cd21-251">You can see a large gain in hello throughput by using PolyBase instead of hello default BULKINSERT mechanism.</span></span> <span data-ttu-id="6cd21-252">Lásd: [teljesítmény hivatkozási szám másolása](data-factory-copy-activity-performance.md#performance-reference) a részletes összehasonlítását.</span><span class="sxs-lookup"><span data-stu-id="6cd21-252">See [copy performance reference number](data-factory-copy-activity-performance.md#performance-reference) with detailed comparison.</span></span> <span data-ttu-id="6cd21-253">A használati esetek bemutatóért lásd: [1 TB-os betöltése az Azure SQL Data Warehouse a 15 perc Azure Data Factory](data-factory-load-sql-data-warehouse.md).</span><span class="sxs-lookup"><span data-stu-id="6cd21-253">For a walkthrough with a use case, see [Load 1 TB into Azure SQL Data Warehouse under 15 minutes with Azure Data Factory](data-factory-load-sql-data-warehouse.md).</span></span>

* <span data-ttu-id="6cd21-254">Ha a forrás adatok **Azure Blob vagy az Azure Data Lake Store**, és hello formátum kompatibilis a PolyBase, tooAzure SQL Data Warehouse PolyBase segítségével közvetlenül másolhatja.</span><span class="sxs-lookup"><span data-stu-id="6cd21-254">If your source data is in **Azure Blob or Azure Data Lake Store**, and hello format is compatible with PolyBase, you can directly copy tooAzure SQL Data Warehouse using PolyBase.</span></span> <span data-ttu-id="6cd21-255">Lásd:  **[közvetlen másolása a PolyBase használatával](#direct-copy-using-polybase)**  adatokkal.</span><span class="sxs-lookup"><span data-stu-id="6cd21-255">See **[Direct copy using PolyBase](#direct-copy-using-polybase)** with details.</span></span>
* <span data-ttu-id="6cd21-256">Ha a forrás-tárolót és formátum eredetileg nem támogatott a PolyBase által, használhatja a hello  **[előkészített másolása a PolyBase használatával](#staged-copy-using-polybase)**  inkább a beállítást.</span><span class="sxs-lookup"><span data-stu-id="6cd21-256">If your source data store and format is not originally supported by PolyBase, you can use hello **[Staged Copy using PolyBase](#staged-copy-using-polybase)** feature instead.</span></span> <span data-ttu-id="6cd21-257">Is biztosít, nagyobb átviteli sebesség automatikusan hello adatok PolyBase-kompatibilis formátumra konvertálása és hello adattárolás az Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="6cd21-257">It also provides you better throughput by automatically converting hello data into PolyBase-compatible format and storing hello data in Azure Blob storage.</span></span> <span data-ttu-id="6cd21-258">Majd betölti az SQL Data Warehouse-adatok.</span><span class="sxs-lookup"><span data-stu-id="6cd21-258">It then loads data into SQL Data Warehouse.</span></span>

<span data-ttu-id="6cd21-259">Set hello `allowPolyBase` tulajdonság túl**igaz** a következő példa az Azure Data Factory toouse PolyBase toocopy adatokat az Azure SQL Data Warehouse hello látható módon.</span><span class="sxs-lookup"><span data-stu-id="6cd21-259">Set hello `allowPolyBase` property too**true** as shown in hello following example for Azure Data Factory toouse PolyBase toocopy data into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="6cd21-260">Ha úgy állítja be a allowPolyBase tootrue, PolyBase tulajdonságokat hello segítségével megadhatja `polyBaseSettings` tulajdonságcsoport.</span><span class="sxs-lookup"><span data-stu-id="6cd21-260">When you set allowPolyBase tootrue, you can specify PolyBase specific properties using hello `polyBaseSettings` property group.</span></span> <span data-ttu-id="6cd21-261">Lásd: hello [SqlDWSink](#SqlDWSink) szakasz kapcsolódó polyBaseSettings használható tulajdonságokról vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="6cd21-261">see hello [SqlDWSink](#SqlDWSink) section for details about properties that you can use with polyBaseSettings.</span></span>

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

### <a name="direct-copy-using-polybase"></a><span data-ttu-id="6cd21-262">A PolyBase használatával közvetlen másolása</span><span class="sxs-lookup"><span data-stu-id="6cd21-262">Direct copy using PolyBase</span></span>
<span data-ttu-id="6cd21-263">SQL Data Warehouse PolyBase közvetlenül támogatja Azure-Blob és az Azure Data Lake Store (egyszerű szolgáltatásnév) forrásként, és az adott fájl formátum követelményeinek.</span><span class="sxs-lookup"><span data-stu-id="6cd21-263">SQL Data Warehouse PolyBase directly support Azure Blob and Azure Data Lake Store (using service principal) as source and with specific file format requirements.</span></span> <span data-ttu-id="6cd21-264">Ha a forrásadatok megfelel a jelen szakaszban ismertetett hello feltételeknek, közvetlenül átmásolhatja forrás adatokat tároló tooAzure SQL Data Warehouse PolyBase használatával.</span><span class="sxs-lookup"><span data-stu-id="6cd21-264">If your source data meets hello criteria described in this section, you can directly copy from source data store tooAzure SQL Data Warehouse using PolyBase.</span></span> <span data-ttu-id="6cd21-265">Ellenkező esetben használhatja [előkészített másolása a PolyBase használatával](#staged-copy-using-polybase).</span><span class="sxs-lookup"><span data-stu-id="6cd21-265">Otherwise, you can use [Staged Copy using PolyBase](#staged-copy-using-polybase).</span></span>

> [!TIP]
> <span data-ttu-id="6cd21-266">Data Lake Store tooSQL adatraktár toocopy adatait hatékonyan, további információhoz [Azure Data Factory teszi egyszerűbbé és kényelmes toouncover információkat kaphat a adatok akkor is igaz, Data Lake Store használata az SQL Data Warehouse szolgáltatással](https://blogs.msdn.microsoft.com/azuredatalake/2017/04/08/azure-data-factory-makes-it-even-easier-and-convenient-to-uncover-insights-from-data-when-using-data-lake-store-with-sql-data-warehouse/).</span><span class="sxs-lookup"><span data-stu-id="6cd21-266">toocopy data from Data Lake Store tooSQL Data Warehouse efficiently, learn more from [Azure Data Factory makes it even easier and convenient toouncover insights from data when using Data Lake Store with SQL Data Warehouse](https://blogs.msdn.microsoft.com/azuredatalake/2017/04/08/azure-data-factory-makes-it-even-easier-and-convenient-to-uncover-insights-from-data-when-using-data-lake-store-with-sql-data-warehouse/).</span></span>

<span data-ttu-id="6cd21-267">Hello feltételeknek nem felel meg, ha az Azure Data Factory hello beállítások ellenőrzi, és automatikusan visszaáll toohello BULKINSERT mechanizmus hello adatátvitelt jelölik.</span><span class="sxs-lookup"><span data-stu-id="6cd21-267">If hello requirements are not met, Azure Data Factory checks hello settings and automatically falls back toohello BULKINSERT mechanism for hello data movement.</span></span>

1. <span data-ttu-id="6cd21-268">**Forrás társított szolgáltatás** típusa: **AzureStorage** vagy **szolgáltatás egyszerű hitelesítéssel AzureDataLakeStore**.</span><span class="sxs-lookup"><span data-stu-id="6cd21-268">**Source linked service** is of type: **AzureStorage** or **AzureDataLakeStore with service principal authentication**.</span></span>  
2. <span data-ttu-id="6cd21-269">Hello **bemeneti adatkészlet** típusa: **AzureBlob** vagy **AzureDataLakeStore**, és a formátum típusa hello `type` tulajdonságai **OrcFormat** , vagy **szöveges** a következő konfigurációk hello:</span><span class="sxs-lookup"><span data-stu-id="6cd21-269">hello **input dataset** is of type: **AzureBlob** or **AzureDataLakeStore**, and hello format type under `type` properties is **OrcFormat**, or **TextFormat** with hello following configurations:</span></span>

   1. <span data-ttu-id="6cd21-270">`rowDelimiter`kell  **\n** .</span><span class="sxs-lookup"><span data-stu-id="6cd21-270">`rowDelimiter` must be **\n**.</span></span>
   2. <span data-ttu-id="6cd21-271">`nullValue`értéke túl**üres karakterlánc** (""), vagy `treatEmptyAsNull` értéke túl**igaz**.</span><span class="sxs-lookup"><span data-stu-id="6cd21-271">`nullValue` is set too**empty string** (""), or `treatEmptyAsNull` is set too**true**.</span></span>
   3. <span data-ttu-id="6cd21-272">`encodingName`értéke túl**utf-8**, amely **alapértelmezett** érték.</span><span class="sxs-lookup"><span data-stu-id="6cd21-272">`encodingName` is set too**utf-8**, which is **default** value.</span></span>
   4. <span data-ttu-id="6cd21-273">`escapeChar`, `quoteChar`, `firstRowAsHeader`, és `skipLineCount` nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="6cd21-273">`escapeChar`, `quoteChar`, `firstRowAsHeader`, and `skipLineCount` are not specified.</span></span>
   5. <span data-ttu-id="6cd21-274">`compression`lehet **tömörítés**, **GZip**, vagy **Deflate**.</span><span class="sxs-lookup"><span data-stu-id="6cd21-274">`compression` can be **no compression**, **GZip**, or **Deflate**.</span></span>

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

3. <span data-ttu-id="6cd21-275">Nincs nincs `skipHeaderLineCount` beállítás alatt **BlobSource** vagy **AzureDataLakeStore** hello másolási tevékenység hello-feldolgozási folyamat számára.</span><span class="sxs-lookup"><span data-stu-id="6cd21-275">There is no `skipHeaderLineCount` setting under **BlobSource** or **AzureDataLakeStore** for hello Copy activity in hello pipeline.</span></span>
4. <span data-ttu-id="6cd21-276">Nincs nincs `sliceIdentifierColumnName` beállítás alatt **SqlDWSink** hello másolási tevékenység hello-feldolgozási folyamat számára.</span><span class="sxs-lookup"><span data-stu-id="6cd21-276">There is no `sliceIdentifierColumnName` setting under **SqlDWSink** for hello Copy activity in hello pipeline.</span></span> <span data-ttu-id="6cd21-277">(A PolyBase garantálja, hogy minden adat frissül, vagy nem frissül, az egyszeri futtatás.</span><span class="sxs-lookup"><span data-stu-id="6cd21-277">(PolyBase guarantees that all data is updated or nothing is updated in a single run.</span></span> <span data-ttu-id="6cd21-278">tooachieve **ismételhetőség**, használhat `sqlWriterCleanupScript`).</span><span class="sxs-lookup"><span data-stu-id="6cd21-278">tooachieve **repeatability**, you could use `sqlWriterCleanupScript`).</span></span>
5. <span data-ttu-id="6cd21-279">Nincs nincs `columnMapping` a másolási tevékenység társított hello használja.</span><span class="sxs-lookup"><span data-stu-id="6cd21-279">There is no `columnMapping` being used in hello associated in Copy activity.</span></span>

### <a name="staged-copy-using-polybase"></a><span data-ttu-id="6cd21-280">A PolyBase használatával előkészített másolása</span><span class="sxs-lookup"><span data-stu-id="6cd21-280">Staged Copy using PolyBase</span></span>
<span data-ttu-id="6cd21-281">A forrásadatok hello előző szakaszban bemutatott hello feltételeknek nem felel meg, amikor az adatok másolását az átmeneti átmeneti Azure Blob Storage (nem lehet a prémium szintű Storage) keresztül is engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="6cd21-281">When your source data doesn’t meet hello criteria introduced in hello previous section, you can enable copying data via an interim staging Azure Blob Storage (cannot be Premium Storage).</span></span> <span data-ttu-id="6cd21-282">Ebben az esetben Azure Data Factory automatikusan átalakítások végez hello toomeet adatok formátuma vonatkozó követelmények a PolyBase, majd használja a PolyBase tooload adatokat az SQL Data Warehouse, és a legutóbbi karbantartás hello blobtárolóból ideiglenes adatait.</span><span class="sxs-lookup"><span data-stu-id="6cd21-282">In this case, Azure Data Factory automatically performs transformations on hello data toomeet data format requirements of PolyBase, then use PolyBase tooload data into SQL Data Warehouse, and at last clean-up your temp data from hello Blob storage.</span></span> <span data-ttu-id="6cd21-283">Lásd: [előkészített másolási](data-factory-copy-activity-performance.md#staged-copy) átmeneti Azure Blob keresztül az adatok másolásának működéséről, általában a részletekért.</span><span class="sxs-lookup"><span data-stu-id="6cd21-283">See [Staged Copy](data-factory-copy-activity-performance.md#staged-copy) for details on how copying data via a staging Azure Blob works in general.</span></span>

> [!NOTE]
> <span data-ttu-id="6cd21-284">Ha az Azure SQL Data Warehouse PolyBase használatával tárolja az adatok másolását egy helyszíni adatokból, és átmeneti, ha az adatkezelési átjáró verziója nem éri el 2.4, JRE (Java Runtime Environment) megadása szükséges. az átjáró számítógépre, amely használt tootransform a forrásadatok van megfelelő formátumba.</span><span class="sxs-lookup"><span data-stu-id="6cd21-284">When copying data from an on-prem data store into Azure SQL Data Warehouse using PolyBase and staging, if your Data Management Gateway version is below 2.4, JRE (Java Runtime Environment) is required on your gateway machine that is used tootransform your source data into proper format.</span></span> <span data-ttu-id="6cd21-285">Javasoljuk, hogy az átjáró toohello legújabb tooavoid ilyen függőségi frissíti.</span><span class="sxs-lookup"><span data-stu-id="6cd21-285">Suggest you upgrade your gateway toohello latest tooavoid such dependency.</span></span>
>

<span data-ttu-id="6cd21-286">toouse ezt a beállítást, hozzon létre egy [Azure Storage társított szolgáltatásnak](data-factory-azure-blob-connector.md#azure-storage-linked-service) toohello Azure Storage-fiókot, amely rendelkezik hello ideiglenes blob-tároló hivatkozik, amely, majd adja meg a hello `enableStaging` és `stagingSettings` hello másolási tevékenység tulajdonságai ahogy az a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="6cd21-286">toouse this feature, create an [Azure Storage linked service](data-factory-azure-blob-connector.md#azure-storage-linked-service) that refers toohello Azure Storage Account that has hello interim blob storage, then specify hello `enableStaging` and `stagingSettings` properties for hello Copy Activity as shown in hello following code:</span></span>

```json
"activities":[  
{
    "name": "Sample copy activity from SQL Server tooSQL Data Warehouse via PolyBase",
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

## <a name="best-practices-when-using-polybase"></a><span data-ttu-id="6cd21-287">A PolyBase használata esetén ajánlott eljárások</span><span class="sxs-lookup"><span data-stu-id="6cd21-287">Best practices when using PolyBase</span></span>
<span data-ttu-id="6cd21-288">hello következő szakaszokban további ajánlott eljárások toohello azokat, a említett [ajánlott eljárások az Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="6cd21-288">hello following sections provide additional best practices toohello ones that are mentioned in [Best practices for Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md).</span></span>

### <a name="required-database-permission"></a><span data-ttu-id="6cd21-289">Adatbázis szükséges engedéllyel</span><span class="sxs-lookup"><span data-stu-id="6cd21-289">Required database permission</span></span>
<span data-ttu-id="6cd21-290">toouse PolyBase, igényel hello felhasználó éppen használt tooload adatokat az SQL Data Warehouse rendelkezik hello ["Vezérlő" engedély](https://msdn.microsoft.com/library/ms191291.aspx) hello cél adatbázison.</span><span class="sxs-lookup"><span data-stu-id="6cd21-290">toouse PolyBase, it requires hello user being used tooload data into SQL Data Warehouse has hello ["CONTROL" permission](https://msdn.microsoft.com/library/ms191291.aspx) on hello target database.</span></span> <span data-ttu-id="6cd21-291">Egyirányú tooachieve, amely tooadd "db_owner" szerepkör tagjaként felhasználó.</span><span class="sxs-lookup"><span data-stu-id="6cd21-291">One way tooachieve that is tooadd that user as a member of "db_owner" role.</span></span> <span data-ttu-id="6cd21-292">Megtudhatja, hogyan toodo, amelyek a következő [ebben a szakaszban](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md#authorization).</span><span class="sxs-lookup"><span data-stu-id="6cd21-292">Learn how toodo that by following [this section](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md#authorization).</span></span>

### <a name="row-size-and-data-type-limitation"></a><span data-ttu-id="6cd21-293">Írja be korlátozás sorméret és adatok</span><span class="sxs-lookup"><span data-stu-id="6cd21-293">Row size and data type limitation</span></span>
<span data-ttu-id="6cd21-294">A Polybase terhelések korlátozódnak tooloading sort is kisebb, mint **1 MB** és nem tölthető be a tooVARCHR(MAX), NVARCHAR(MAX) vagy VARBINARY(MAX).</span><span class="sxs-lookup"><span data-stu-id="6cd21-294">Polybase loads are limited tooloading rows both smaller than **1 MB** and cannot load tooVARCHR(MAX), NVARCHAR(MAX) or VARBINARY(MAX).</span></span> <span data-ttu-id="6cd21-295">Tekintse meg a túl[Itt](../sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md#loads).</span><span class="sxs-lookup"><span data-stu-id="6cd21-295">Refer too[here](../sql-data-warehouse/sql-data-warehouse-service-capacity-limits.md#loads).</span></span>

<span data-ttu-id="6cd21-296">Ha 1 MB-nál nagyobb méretű sorokat tartalmazó forrásadatok, előfordulhat, hogy a kívánt toosplit hello forrástáblákból függőleges több kis állók közül. Ha hello legnagyobb sorainak méretéhez azok nem haladja meg a hello korlátot.</span><span class="sxs-lookup"><span data-stu-id="6cd21-296">If you have source data with rows of size greater than 1 MB, you may want toosplit hello source tables vertically into several small ones where hello largest row size of each of them does not exceed hello limit.</span></span> <span data-ttu-id="6cd21-297">hello kisebb táblák majd tölthetők be a PolyBase használatával, és egyesíti az Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="6cd21-297">hello smaller tables can then be loaded using PolyBase and merged together in Azure SQL Data Warehouse.</span></span>

### <a name="sql-data-warehouse-resource-class"></a><span data-ttu-id="6cd21-298">Az SQL Data Warehouse erőforrás osztály</span><span class="sxs-lookup"><span data-stu-id="6cd21-298">SQL Data Warehouse resource class</span></span>
<span data-ttu-id="6cd21-299">tooachieve legjobb lehetséges átviteli, fontolja meg a tooassign nagyobb erőforrás osztály toohello felhasználó éppen használt tooload adatokat az SQL Data Warehouse polybase.</span><span class="sxs-lookup"><span data-stu-id="6cd21-299">tooachieve best possible throughput, consider tooassign larger resource class toohello user being used tooload data into SQL Data Warehouse via PolyBase.</span></span> <span data-ttu-id="6cd21-300">Megtudhatja, hogyan toodo, amelyek a következő [módosíthatja a felhasználói erőforrás osztály példa](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).</span><span class="sxs-lookup"><span data-stu-id="6cd21-300">Learn how toodo that by following [Change a user resource class example](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).</span></span>

### <a name="tablename-in-azure-sql-data-warehouse"></a><span data-ttu-id="6cd21-301">az Azure SQL Data Warehouse táblanév</span><span class="sxs-lookup"><span data-stu-id="6cd21-301">tableName in Azure SQL Data Warehouse</span></span>
<span data-ttu-id="6cd21-302">hello alábbi táblázat példákat hogyan toospecify hello **tableName** tulajdonságot az adatkészlet JSON-séma-és táblanevet különböző kombinációjához.</span><span class="sxs-lookup"><span data-stu-id="6cd21-302">hello following table provides examples on how toospecify hello **tableName** property in dataset JSON for various combinations of schema and table name.</span></span>

| <span data-ttu-id="6cd21-303">Megadott adatbázissémát</span><span class="sxs-lookup"><span data-stu-id="6cd21-303">DB Schema</span></span> | <span data-ttu-id="6cd21-304">Tábla neve</span><span class="sxs-lookup"><span data-stu-id="6cd21-304">Table name</span></span> | <span data-ttu-id="6cd21-305">tableName JSON tulajdonsága</span><span class="sxs-lookup"><span data-stu-id="6cd21-305">tableName JSON property</span></span> |
| --- | --- | --- |
| <span data-ttu-id="6cd21-306">dbo</span><span class="sxs-lookup"><span data-stu-id="6cd21-306">dbo</span></span> |<span data-ttu-id="6cd21-307">Táblanév</span><span class="sxs-lookup"><span data-stu-id="6cd21-307">MyTable</span></span> |<span data-ttu-id="6cd21-308">MyTable vagy dbo. MyTable vagy [dbo]. [MyTable]</span><span class="sxs-lookup"><span data-stu-id="6cd21-308">MyTable or dbo.MyTable or [dbo].[MyTable]</span></span> |
| <span data-ttu-id="6cd21-309">dbo1</span><span class="sxs-lookup"><span data-stu-id="6cd21-309">dbo1</span></span> |<span data-ttu-id="6cd21-310">Táblanév</span><span class="sxs-lookup"><span data-stu-id="6cd21-310">MyTable</span></span> |<span data-ttu-id="6cd21-311">dbo1. MyTable vagy [dbo1]. [MyTable]</span><span class="sxs-lookup"><span data-stu-id="6cd21-311">dbo1.MyTable or [dbo1].[MyTable]</span></span> |
| <span data-ttu-id="6cd21-312">dbo</span><span class="sxs-lookup"><span data-stu-id="6cd21-312">dbo</span></span> |<span data-ttu-id="6cd21-313">My.Table</span><span class="sxs-lookup"><span data-stu-id="6cd21-313">My.Table</span></span> |<span data-ttu-id="6cd21-314">[My.Table] vagy [dbo]. [My.Table]</span><span class="sxs-lookup"><span data-stu-id="6cd21-314">[My.Table] or [dbo].[My.Table]</span></span> |
| <span data-ttu-id="6cd21-315">dbo1</span><span class="sxs-lookup"><span data-stu-id="6cd21-315">dbo1</span></span> |<span data-ttu-id="6cd21-316">My.Table</span><span class="sxs-lookup"><span data-stu-id="6cd21-316">My.Table</span></span> |<span data-ttu-id="6cd21-317">[dbo1]. [My.Table]</span><span class="sxs-lookup"><span data-stu-id="6cd21-317">[dbo1].[My.Table]</span></span> |

<span data-ttu-id="6cd21-318">Ha megjelenik a következő hiba hello, problémát hello tableName tulajdonsága megadott hello érték lehet.</span><span class="sxs-lookup"><span data-stu-id="6cd21-318">If you see hello following error, it could be an issue with hello value you specified for hello tableName property.</span></span> <span data-ttu-id="6cd21-319">Hello megfelelő módon toospecify hello tableName JSON tulajdonság értékei a hello táblázatban találja.</span><span class="sxs-lookup"><span data-stu-id="6cd21-319">See hello table for hello correct way toospecify values for hello tableName JSON property.</span></span>  

```
Type=System.Data.SqlClient.SqlException,Message=Invalid object name 'stg.Account_test'.,Source=.Net SqlClient Data Provider
```

### <a name="columns-with-default-values"></a><span data-ttu-id="6cd21-320">Az alapértelmezett értékekkel oszlopok</span><span class="sxs-lookup"><span data-stu-id="6cd21-320">Columns with default values</span></span>
<span data-ttu-id="6cd21-321">Jelenleg a PolyBase szolgáltatás adat-előállítóban csak fogad hello azonos számú oszlopot hello céltábla hasonlóan.</span><span class="sxs-lookup"><span data-stu-id="6cd21-321">Currently, PolyBase feature in Data Factory only accepts hello same number of columns as in hello target table.</span></span> <span data-ttu-id="6cd21-322">Tegyük fel például, négy oszlopokkal rendelkező táblát rendelkezik, és az egyik legyen alapértelmezett értékkel van definiálva.</span><span class="sxs-lookup"><span data-stu-id="6cd21-322">Say, you have a table with four columns and one of them is defined with a default value.</span></span> <span data-ttu-id="6cd21-323">hello bemeneti adatok továbbra is tartalmaznia kell a négy oszlopot.</span><span class="sxs-lookup"><span data-stu-id="6cd21-323">hello input data should still contain four columns.</span></span> <span data-ttu-id="6cd21-324">A 3-oszlop a bemeneti adatkészletet biztosító eredményezné egy hasonló toohello hiba, a következő üzenetet:</span><span class="sxs-lookup"><span data-stu-id="6cd21-324">Providing a 3-column input dataset would yield an error similar toohello following message:</span></span>

```
All columns of hello table must be specified in hello INSERT BULK statement.
```
<span data-ttu-id="6cd21-325">NULL érték az alapértelmezett érték egy speciális formája, amely.</span><span class="sxs-lookup"><span data-stu-id="6cd21-325">NULL value is a special form of default value.</span></span> <span data-ttu-id="6cd21-326">Ha hello oszlop nullázható, hello bemeneti adatait (blob) az adott oszlop lehet üres (nem hiányzik hello bemeneti adatkészlet).</span><span class="sxs-lookup"><span data-stu-id="6cd21-326">If hello column is nullable, hello input data (in blob) for that column could be empty (cannot be missing from hello input dataset).</span></span> <span data-ttu-id="6cd21-327">A PolyBase számukra NULL hello Azure SQL Data Warehouse szúrja be.</span><span class="sxs-lookup"><span data-stu-id="6cd21-327">PolyBase inserts NULL for them in hello Azure SQL Data Warehouse.</span></span>  

## <a name="auto-table-creation"></a><span data-ttu-id="6cd21-328">Automatikus tábla létrehozásához</span><span class="sxs-lookup"><span data-stu-id="6cd21-328">Auto table creation</span></span>
<span data-ttu-id="6cd21-329">Ha az SQL Server adatainak másolása varázsló toocopy használ, vagy az Azure SQL Database tooAzure SQL Data Warehouse és hello tartozó tábla toohello forrástábla nem létezik a hello céltár, adat-előállító automatikusan létrehozhat hello tábla hello az adatraktár hello forrás táblaséma használatával.</span><span class="sxs-lookup"><span data-stu-id="6cd21-329">If you are using Copy Wizard toocopy data from SQL Server or Azure SQL Database tooAzure SQL Data Warehouse and hello table that corresponds toohello source table does not exist in hello destination store, Data Factory can automatically create hello table in hello data warehouse by using hello source table schema.</span></span>

<span data-ttu-id="6cd21-330">Adat-előállító hello céltár hello táblát hoz létre a hello ugyanaz a tábla neve hello forrás adattár.</span><span class="sxs-lookup"><span data-stu-id="6cd21-330">Data Factory creates hello table in hello destination store with hello same table name in hello source data store.</span></span> <span data-ttu-id="6cd21-331">hello adattípusok oszlopok a következő típusleképezéshez hello alapján választják ki.</span><span class="sxs-lookup"><span data-stu-id="6cd21-331">hello data types for columns are chosen based on hello following type mapping.</span></span> <span data-ttu-id="6cd21-332">Szükség esetén elvégez típus átalakítások toofix között a forrás és cél tárolja az azonosított inkompatibilitásokat.</span><span class="sxs-lookup"><span data-stu-id="6cd21-332">If needed, it performs type conversions toofix any incompatibilities between source and destination stores.</span></span> <span data-ttu-id="6cd21-333">Ciklikus multiplexelés elosztása is használja.</span><span class="sxs-lookup"><span data-stu-id="6cd21-333">It also uses Round Robin table distribution.</span></span>

| <span data-ttu-id="6cd21-334">Forrás SQL-adatbázis oszlop típusa</span><span class="sxs-lookup"><span data-stu-id="6cd21-334">Source SQL Database column type</span></span> | <span data-ttu-id="6cd21-335">Cél SQL DW oszlop típusa (méretének korlátozása)</span><span class="sxs-lookup"><span data-stu-id="6cd21-335">Destination SQL DW column type (size limitation)</span></span> |
| --- | --- |
| <span data-ttu-id="6cd21-336">int</span><span class="sxs-lookup"><span data-stu-id="6cd21-336">Int</span></span> | <span data-ttu-id="6cd21-337">int</span><span class="sxs-lookup"><span data-stu-id="6cd21-337">Int</span></span> |
| <span data-ttu-id="6cd21-338">BigInt</span><span class="sxs-lookup"><span data-stu-id="6cd21-338">BigInt</span></span> | <span data-ttu-id="6cd21-339">BigInt</span><span class="sxs-lookup"><span data-stu-id="6cd21-339">BigInt</span></span> |
| <span data-ttu-id="6cd21-340">SmallInt</span><span class="sxs-lookup"><span data-stu-id="6cd21-340">SmallInt</span></span> | <span data-ttu-id="6cd21-341">SmallInt</span><span class="sxs-lookup"><span data-stu-id="6cd21-341">SmallInt</span></span> |
| <span data-ttu-id="6cd21-342">TinyInt</span><span class="sxs-lookup"><span data-stu-id="6cd21-342">TinyInt</span></span> | <span data-ttu-id="6cd21-343">TinyInt</span><span class="sxs-lookup"><span data-stu-id="6cd21-343">TinyInt</span></span> |
| <span data-ttu-id="6cd21-344">bit</span><span class="sxs-lookup"><span data-stu-id="6cd21-344">Bit</span></span> | <span data-ttu-id="6cd21-345">bit</span><span class="sxs-lookup"><span data-stu-id="6cd21-345">Bit</span></span> |
| <span data-ttu-id="6cd21-346">Decimális</span><span class="sxs-lookup"><span data-stu-id="6cd21-346">Decimal</span></span> | <span data-ttu-id="6cd21-347">Decimális</span><span class="sxs-lookup"><span data-stu-id="6cd21-347">Decimal</span></span> |
| <span data-ttu-id="6cd21-348">Numerikus</span><span class="sxs-lookup"><span data-stu-id="6cd21-348">Numeric</span></span> | <span data-ttu-id="6cd21-349">Decimális</span><span class="sxs-lookup"><span data-stu-id="6cd21-349">Decimal</span></span> |
| <span data-ttu-id="6cd21-350">Lebegőpontos</span><span class="sxs-lookup"><span data-stu-id="6cd21-350">Float</span></span> | <span data-ttu-id="6cd21-351">Lebegőpontos</span><span class="sxs-lookup"><span data-stu-id="6cd21-351">Float</span></span> |
| <span data-ttu-id="6cd21-352">pénz</span><span class="sxs-lookup"><span data-stu-id="6cd21-352">Money</span></span> | <span data-ttu-id="6cd21-353">pénz</span><span class="sxs-lookup"><span data-stu-id="6cd21-353">Money</span></span> |
| <span data-ttu-id="6cd21-354">Real</span><span class="sxs-lookup"><span data-stu-id="6cd21-354">Real</span></span> | <span data-ttu-id="6cd21-355">Real</span><span class="sxs-lookup"><span data-stu-id="6cd21-355">Real</span></span> |
| <span data-ttu-id="6cd21-356">Kis pénz típusú értéknél</span><span class="sxs-lookup"><span data-stu-id="6cd21-356">SmallMoney</span></span> | <span data-ttu-id="6cd21-357">Kis pénz típusú értéknél</span><span class="sxs-lookup"><span data-stu-id="6cd21-357">SmallMoney</span></span> |
| <span data-ttu-id="6cd21-358">Bináris</span><span class="sxs-lookup"><span data-stu-id="6cd21-358">Binary</span></span> | <span data-ttu-id="6cd21-359">Bináris</span><span class="sxs-lookup"><span data-stu-id="6cd21-359">Binary</span></span> |
| <span data-ttu-id="6cd21-360">varbinary</span><span class="sxs-lookup"><span data-stu-id="6cd21-360">Varbinary</span></span> | <span data-ttu-id="6cd21-361">Varbinary (felfelé too8000)</span><span class="sxs-lookup"><span data-stu-id="6cd21-361">Varbinary (up too8000)</span></span> |
| <span data-ttu-id="6cd21-362">Dátum</span><span class="sxs-lookup"><span data-stu-id="6cd21-362">Date</span></span> | <span data-ttu-id="6cd21-363">Dátum</span><span class="sxs-lookup"><span data-stu-id="6cd21-363">Date</span></span> |
| <span data-ttu-id="6cd21-364">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="6cd21-364">DateTime</span></span> | <span data-ttu-id="6cd21-365">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="6cd21-365">DateTime</span></span> |
| <span data-ttu-id="6cd21-366">DateTime2</span><span class="sxs-lookup"><span data-stu-id="6cd21-366">DateTime2</span></span> | <span data-ttu-id="6cd21-367">DateTime2</span><span class="sxs-lookup"><span data-stu-id="6cd21-367">DateTime2</span></span> |
| <span data-ttu-id="6cd21-368">Time</span><span class="sxs-lookup"><span data-stu-id="6cd21-368">Time</span></span> | <span data-ttu-id="6cd21-369">Time</span><span class="sxs-lookup"><span data-stu-id="6cd21-369">Time</span></span> |
| <span data-ttu-id="6cd21-370">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="6cd21-370">DateTimeOffset</span></span> | <span data-ttu-id="6cd21-371">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="6cd21-371">DateTimeOffset</span></span> |
| <span data-ttu-id="6cd21-372">SmallDateTime</span><span class="sxs-lookup"><span data-stu-id="6cd21-372">SmallDateTime</span></span> | <span data-ttu-id="6cd21-373">SmallDateTime</span><span class="sxs-lookup"><span data-stu-id="6cd21-373">SmallDateTime</span></span> |
| <span data-ttu-id="6cd21-374">Szöveg</span><span class="sxs-lookup"><span data-stu-id="6cd21-374">Text</span></span> | <span data-ttu-id="6cd21-375">Varchar (felfelé too8000)</span><span class="sxs-lookup"><span data-stu-id="6cd21-375">Varchar (up too8000)</span></span> |
| <span data-ttu-id="6cd21-376">NText</span><span class="sxs-lookup"><span data-stu-id="6cd21-376">NText</span></span> | <span data-ttu-id="6cd21-377">NVarChar (felfelé too4000)</span><span class="sxs-lookup"><span data-stu-id="6cd21-377">NVarChar (up too4000)</span></span> |
| <span data-ttu-id="6cd21-378">Kép</span><span class="sxs-lookup"><span data-stu-id="6cd21-378">Image</span></span> | <span data-ttu-id="6cd21-379">VarBinary (felfelé too8000)</span><span class="sxs-lookup"><span data-stu-id="6cd21-379">VarBinary (up too8000)</span></span> |
| <span data-ttu-id="6cd21-380">Egyedi azonosító</span><span class="sxs-lookup"><span data-stu-id="6cd21-380">UniqueIdentifier</span></span> | <span data-ttu-id="6cd21-381">Egyedi azonosító</span><span class="sxs-lookup"><span data-stu-id="6cd21-381">UniqueIdentifier</span></span> |
| <span data-ttu-id="6cd21-382">Karakter</span><span class="sxs-lookup"><span data-stu-id="6cd21-382">Char</span></span> | <span data-ttu-id="6cd21-383">Karakter</span><span class="sxs-lookup"><span data-stu-id="6cd21-383">Char</span></span> |
| <span data-ttu-id="6cd21-384">NChar</span><span class="sxs-lookup"><span data-stu-id="6cd21-384">NChar</span></span> | <span data-ttu-id="6cd21-385">NChar</span><span class="sxs-lookup"><span data-stu-id="6cd21-385">NChar</span></span> |
| <span data-ttu-id="6cd21-386">VarChar</span><span class="sxs-lookup"><span data-stu-id="6cd21-386">VarChar</span></span> | <span data-ttu-id="6cd21-387">VarChar (felfelé too8000)</span><span class="sxs-lookup"><span data-stu-id="6cd21-387">VarChar (up too8000)</span></span> |
| <span data-ttu-id="6cd21-388">NVarChar</span><span class="sxs-lookup"><span data-stu-id="6cd21-388">NVarChar</span></span> | <span data-ttu-id="6cd21-389">NVarChar (felfelé too4000)</span><span class="sxs-lookup"><span data-stu-id="6cd21-389">NVarChar (up too4000)</span></span> |
| <span data-ttu-id="6cd21-390">XML</span><span class="sxs-lookup"><span data-stu-id="6cd21-390">Xml</span></span> | <span data-ttu-id="6cd21-391">Varchar (felfelé too8000)</span><span class="sxs-lookup"><span data-stu-id="6cd21-391">Varchar (up too8000)</span></span> |

[!INCLUDE [data-factory-type-repeatability-for-sql-sources](../../includes/data-factory-type-repeatability-for-sql-sources.md)]

## <a name="type-mapping-for-azure-sql-data-warehouse"></a><span data-ttu-id="6cd21-392">Írja be az Azure SQL Data Warehouse leképezése</span><span class="sxs-lookup"><span data-stu-id="6cd21-392">Type mapping for Azure SQL Data Warehouse</span></span>
<span data-ttu-id="6cd21-393">A hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk másolási tevékenység hajt végre automatikus típuskonverziók származó típusok toosink típusait a 2. lépés – a módszert követve hello:</span><span class="sxs-lookup"><span data-stu-id="6cd21-393">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types toosink types with hello following 2-step approach:</span></span>

1. <span data-ttu-id="6cd21-394">Natív típusok too.NET forrástípus konvertálása</span><span class="sxs-lookup"><span data-stu-id="6cd21-394">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="6cd21-395">.NET típusú toonative a fogadó típusa konvertálása</span><span class="sxs-lookup"><span data-stu-id="6cd21-395">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="6cd21-396">Ha túl & Azure SQL Data Warehouse az adatok áthelyezése, hello következő megfeleltetéseket használ SQL too.NET típusának, és ez fordítva is igaz.</span><span class="sxs-lookup"><span data-stu-id="6cd21-396">When moving data too& from Azure SQL Data Warehouse, hello following mappings are used from SQL type too.NET type and vice versa.</span></span>

<span data-ttu-id="6cd21-397">hello leképezési legyen, mint hello [SQL Server adattípus-hozzárendelése az ADO.NET](https://msdn.microsoft.com/library/cc716729.aspx).</span><span class="sxs-lookup"><span data-stu-id="6cd21-397">hello mapping is same as hello [SQL Server Data Type Mapping for ADO.NET](https://msdn.microsoft.com/library/cc716729.aspx).</span></span>

| <span data-ttu-id="6cd21-398">SQL Server adatbázismotor típusa</span><span class="sxs-lookup"><span data-stu-id="6cd21-398">SQL Server Database Engine type</span></span> | <span data-ttu-id="6cd21-399">.NET-keretrendszer típusa</span><span class="sxs-lookup"><span data-stu-id="6cd21-399">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="6cd21-400">bigint</span><span class="sxs-lookup"><span data-stu-id="6cd21-400">bigint</span></span> |<span data-ttu-id="6cd21-401">Int64</span><span class="sxs-lookup"><span data-stu-id="6cd21-401">Int64</span></span> |
| <span data-ttu-id="6cd21-402">Bináris</span><span class="sxs-lookup"><span data-stu-id="6cd21-402">binary</span></span> |<span data-ttu-id="6cd21-403">Byte]</span><span class="sxs-lookup"><span data-stu-id="6cd21-403">Byte[]</span></span> |
| <span data-ttu-id="6cd21-404">bit</span><span class="sxs-lookup"><span data-stu-id="6cd21-404">bit</span></span> |<span data-ttu-id="6cd21-405">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="6cd21-405">Boolean</span></span> |
| <span data-ttu-id="6cd21-406">Karakter</span><span class="sxs-lookup"><span data-stu-id="6cd21-406">char</span></span> |<span data-ttu-id="6cd21-407">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="6cd21-407">String, Char[]</span></span> |
| <span data-ttu-id="6cd21-408">Dátum</span><span class="sxs-lookup"><span data-stu-id="6cd21-408">date</span></span> |<span data-ttu-id="6cd21-409">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="6cd21-409">DateTime</span></span> |
| <span data-ttu-id="6cd21-410">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="6cd21-410">Datetime</span></span> |<span data-ttu-id="6cd21-411">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="6cd21-411">DateTime</span></span> |
| <span data-ttu-id="6cd21-412">datetime2</span><span class="sxs-lookup"><span data-stu-id="6cd21-412">datetime2</span></span> |<span data-ttu-id="6cd21-413">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="6cd21-413">DateTime</span></span> |
| <span data-ttu-id="6cd21-414">datetimeoffset</span><span class="sxs-lookup"><span data-stu-id="6cd21-414">Datetimeoffset</span></span> |<span data-ttu-id="6cd21-415">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="6cd21-415">DateTimeOffset</span></span> |
| <span data-ttu-id="6cd21-416">Decimális</span><span class="sxs-lookup"><span data-stu-id="6cd21-416">Decimal</span></span> |<span data-ttu-id="6cd21-417">Decimális</span><span class="sxs-lookup"><span data-stu-id="6cd21-417">Decimal</span></span> |
| <span data-ttu-id="6cd21-418">A FILESTREAM attribútum (varbinary(max))</span><span class="sxs-lookup"><span data-stu-id="6cd21-418">FILESTREAM attribute (varbinary(max))</span></span> |<span data-ttu-id="6cd21-419">Byte]</span><span class="sxs-lookup"><span data-stu-id="6cd21-419">Byte[]</span></span> |
| <span data-ttu-id="6cd21-420">Lebegőpontos</span><span class="sxs-lookup"><span data-stu-id="6cd21-420">Float</span></span> |<span data-ttu-id="6cd21-421">Dupla</span><span class="sxs-lookup"><span data-stu-id="6cd21-421">Double</span></span> |
| <span data-ttu-id="6cd21-422">Kép</span><span class="sxs-lookup"><span data-stu-id="6cd21-422">image</span></span> |<span data-ttu-id="6cd21-423">Byte]</span><span class="sxs-lookup"><span data-stu-id="6cd21-423">Byte[]</span></span> |
| <span data-ttu-id="6cd21-424">int</span><span class="sxs-lookup"><span data-stu-id="6cd21-424">int</span></span> |<span data-ttu-id="6cd21-425">Int32</span><span class="sxs-lookup"><span data-stu-id="6cd21-425">Int32</span></span> |
| <span data-ttu-id="6cd21-426">pénz</span><span class="sxs-lookup"><span data-stu-id="6cd21-426">money</span></span> |<span data-ttu-id="6cd21-427">Decimális</span><span class="sxs-lookup"><span data-stu-id="6cd21-427">Decimal</span></span> |
| <span data-ttu-id="6cd21-428">nchar</span><span class="sxs-lookup"><span data-stu-id="6cd21-428">nchar</span></span> |<span data-ttu-id="6cd21-429">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="6cd21-429">String, Char[]</span></span> |
| <span data-ttu-id="6cd21-430">ntext</span><span class="sxs-lookup"><span data-stu-id="6cd21-430">ntext</span></span> |<span data-ttu-id="6cd21-431">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="6cd21-431">String, Char[]</span></span> |
| <span data-ttu-id="6cd21-432">Numerikus</span><span class="sxs-lookup"><span data-stu-id="6cd21-432">numeric</span></span> |<span data-ttu-id="6cd21-433">Decimális</span><span class="sxs-lookup"><span data-stu-id="6cd21-433">Decimal</span></span> |
| <span data-ttu-id="6cd21-434">nvarchar</span><span class="sxs-lookup"><span data-stu-id="6cd21-434">nvarchar</span></span> |<span data-ttu-id="6cd21-435">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="6cd21-435">String, Char[]</span></span> |
| <span data-ttu-id="6cd21-436">valós</span><span class="sxs-lookup"><span data-stu-id="6cd21-436">real</span></span> |<span data-ttu-id="6cd21-437">Egyetlen</span><span class="sxs-lookup"><span data-stu-id="6cd21-437">Single</span></span> |
| <span data-ttu-id="6cd21-438">ROWVERSION</span><span class="sxs-lookup"><span data-stu-id="6cd21-438">rowversion</span></span> |<span data-ttu-id="6cd21-439">Byte]</span><span class="sxs-lookup"><span data-stu-id="6cd21-439">Byte[]</span></span> |
| <span data-ttu-id="6cd21-440">smalldatetime</span><span class="sxs-lookup"><span data-stu-id="6cd21-440">smalldatetime</span></span> |<span data-ttu-id="6cd21-441">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="6cd21-441">DateTime</span></span> |
| <span data-ttu-id="6cd21-442">smallint</span><span class="sxs-lookup"><span data-stu-id="6cd21-442">smallint</span></span> |<span data-ttu-id="6cd21-443">Int16</span><span class="sxs-lookup"><span data-stu-id="6cd21-443">Int16</span></span> |
| <span data-ttu-id="6cd21-444">kis pénz típusú értéknél</span><span class="sxs-lookup"><span data-stu-id="6cd21-444">smallmoney</span></span> |<span data-ttu-id="6cd21-445">Decimális</span><span class="sxs-lookup"><span data-stu-id="6cd21-445">Decimal</span></span> |
| <span data-ttu-id="6cd21-446">sql_variant</span><span class="sxs-lookup"><span data-stu-id="6cd21-446">sql_variant</span></span> |<span data-ttu-id="6cd21-447">Objektum *</span><span class="sxs-lookup"><span data-stu-id="6cd21-447">Object *</span></span> |
| <span data-ttu-id="6cd21-448">Szöveg</span><span class="sxs-lookup"><span data-stu-id="6cd21-448">text</span></span> |<span data-ttu-id="6cd21-449">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="6cd21-449">String, Char[]</span></span> |
| <span data-ttu-id="6cd21-450">time</span><span class="sxs-lookup"><span data-stu-id="6cd21-450">time</span></span> |<span data-ttu-id="6cd21-451">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="6cd21-451">TimeSpan</span></span> |
| <span data-ttu-id="6cd21-452">időbélyeg</span><span class="sxs-lookup"><span data-stu-id="6cd21-452">timestamp</span></span> |<span data-ttu-id="6cd21-453">Byte]</span><span class="sxs-lookup"><span data-stu-id="6cd21-453">Byte[]</span></span> |
| <span data-ttu-id="6cd21-454">tinyint</span><span class="sxs-lookup"><span data-stu-id="6cd21-454">tinyint</span></span> |<span data-ttu-id="6cd21-455">Bájt</span><span class="sxs-lookup"><span data-stu-id="6cd21-455">Byte</span></span> |
| <span data-ttu-id="6cd21-456">egyedi azonosító</span><span class="sxs-lookup"><span data-stu-id="6cd21-456">uniqueidentifier</span></span> |<span data-ttu-id="6cd21-457">GUID</span><span class="sxs-lookup"><span data-stu-id="6cd21-457">Guid</span></span> |
| <span data-ttu-id="6cd21-458">varbinary</span><span class="sxs-lookup"><span data-stu-id="6cd21-458">varbinary</span></span> |<span data-ttu-id="6cd21-459">Byte]</span><span class="sxs-lookup"><span data-stu-id="6cd21-459">Byte[]</span></span> |
| <span data-ttu-id="6cd21-460">varchar</span><span class="sxs-lookup"><span data-stu-id="6cd21-460">varchar</span></span> |<span data-ttu-id="6cd21-461">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="6cd21-461">String, Char[]</span></span> |
| <span data-ttu-id="6cd21-462">xml</span><span class="sxs-lookup"><span data-stu-id="6cd21-462">xml</span></span> |<span data-ttu-id="6cd21-463">XML</span><span class="sxs-lookup"><span data-stu-id="6cd21-463">Xml</span></span> |

<span data-ttu-id="6cd21-464">Forrás adatkészlet toocolumns hello másolási tevékenységdefinícióban fogadó adatkészletből oszlopokat is leképezheti.</span><span class="sxs-lookup"><span data-stu-id="6cd21-464">You can also map columns from source dataset toocolumns from sink dataset in hello copy activity definition.</span></span> <span data-ttu-id="6cd21-465">További információkért lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="6cd21-465">For details, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="json-examples-for-copying-data-tooand-from-sql-data-warehouse"></a><span data-ttu-id="6cd21-466">Az adatok tooand másolását az SQL Data Warehouse JSON példák</span><span class="sxs-lookup"><span data-stu-id="6cd21-466">JSON examples for copying data tooand from SQL Data Warehouse</span></span>
<span data-ttu-id="6cd21-467">hello alábbi példák megadják minta JSON-definíciók használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="6cd21-467">hello following examples provide sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="6cd21-468">Azok hogyan toocopy adatok tooand az Azure SQL Data Warehouse és az Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="6cd21-468">They show how toocopy data tooand from Azure SQL Data Warehouse and Azure Blob Storage.</span></span> <span data-ttu-id="6cd21-469">Azonban az adatok átmásolhatók **közvetlenül** bármelyik megadott hello nyelő források tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.</span><span class="sxs-lookup"><span data-stu-id="6cd21-469">However, data can be copied **directly** from any of sources tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>

### <a name="example-copy-data-from-azure-sql-data-warehouse-tooazure-blob"></a><span data-ttu-id="6cd21-470">Példa: Adatok másolása az Azure SQL Data Warehouse tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="6cd21-470">Example: Copy data from Azure SQL Data Warehouse tooAzure Blob</span></span>
<span data-ttu-id="6cd21-471">hello minta meghatározza, hogy a következő adat-előállító entitások hello:</span><span class="sxs-lookup"><span data-stu-id="6cd21-471">hello sample defines hello following Data Factory entities:</span></span>

1. <span data-ttu-id="6cd21-472">A társított szolgáltatás típusa [AzureSqlDW](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="6cd21-472">A linked service of type [AzureSqlDW](#linked-service-properties).</span></span>
2. <span data-ttu-id="6cd21-473">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="6cd21-473">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="6cd21-474">Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureSqlDWTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="6cd21-474">An input [dataset](data-factory-create-datasets.md) of type [AzureSqlDWTable](#dataset-properties).</span></span>
4. <span data-ttu-id="6cd21-475">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="6cd21-475">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="6cd21-476">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [SqlDWSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="6cd21-476">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [SqlDWSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="6cd21-477">hello minta idősorozat (óránkénti, napi stb) adatainak másolása az Azure SQL Data Warehouse adatbázis tooa blob egy táblázatban minden órában.</span><span class="sxs-lookup"><span data-stu-id="6cd21-477">hello sample copies time-series (hourly, daily, etc.) data from a table in Azure SQL Data Warehouse database tooa blob every hour.</span></span> <span data-ttu-id="6cd21-478">Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="6cd21-478">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="6cd21-479">**A társított szolgáltatásnak Azure SQL Data Warehouse:**</span><span class="sxs-lookup"><span data-stu-id="6cd21-479">**Azure SQL Data Warehouse linked service:**</span></span>

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
<span data-ttu-id="6cd21-480">**Az Azure Blob storage társított szolgáltatásnak:**</span><span class="sxs-lookup"><span data-stu-id="6cd21-480">**Azure Blob storage linked service:**</span></span>

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
<span data-ttu-id="6cd21-481">**Az SQL Data Warehouse bemeneti adatkészletet:**</span><span class="sxs-lookup"><span data-stu-id="6cd21-481">**Azure SQL Data Warehouse input dataset:**</span></span>

<span data-ttu-id="6cd21-482">hello minta azt feltételezi, hogy létrehozott egy tábla "MyTable" az Azure SQL Data Warehouse és egy "timestampcolumn" nevű adatsorozat időadatok oszlopot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="6cd21-482">hello sample assumes you have created a table “MyTable” in Azure SQL Data Warehouse and it contains a column called “timestampcolumn” for time series data.</span></span>

<span data-ttu-id="6cd21-483">"External" beállítása: "true" tájékoztatja hello Data Factory szolgáltatásnak, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="6cd21-483">Setting “external”: ”true” informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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
<span data-ttu-id="6cd21-484">**Az Azure Blob kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="6cd21-484">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="6cd21-485">Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="6cd21-485">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="6cd21-486">hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="6cd21-486">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="6cd21-487">hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.</span><span class="sxs-lookup"><span data-stu-id="6cd21-487">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

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

<span data-ttu-id="6cd21-488">**Másolási tevékenység során a folyamat SqlDWSource és BlobSink:**</span><span class="sxs-lookup"><span data-stu-id="6cd21-488">**Copy activity in a pipeline with SqlDWSource and BlobSink:**</span></span>

<span data-ttu-id="6cd21-489">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="6cd21-489">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="6cd21-490">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**SqlDWSource** és **fogadó** típusuk értéke túl**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="6cd21-490">In hello pipeline JSON definition, hello **source** type is set too**SqlDWSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="6cd21-491">hello SQL-lekérdezésben megadott hello **SqlReaderQuery** tulajdonság jelöli ki hello adatok hello toocopy óránként túlra.</span><span class="sxs-lookup"><span data-stu-id="6cd21-491">hello SQL query specified for hello **SqlReaderQuery** property selects hello data in hello past hour toocopy.</span></span>

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
> <span data-ttu-id="6cd21-492">Hello példában **sqlReaderQuery** hello SqlDWSource van megadva.</span><span class="sxs-lookup"><span data-stu-id="6cd21-492">In hello example, **sqlReaderQuery** is specified for hello SqlDWSource.</span></span> <span data-ttu-id="6cd21-493">hello másolási tevékenység fut ez a lekérdezés hello Azure SQL Data Warehouse forrásadatok tooget hello.</span><span class="sxs-lookup"><span data-stu-id="6cd21-493">hello Copy Activity runs this query against hello Azure SQL Data Warehouse source tooget hello data.</span></span>
>
> <span data-ttu-id="6cd21-494">Másik lehetőségként megadhat tárolt eljárás hello megadásával **sqlReaderStoredProcedureName** és **storedProcedureParameters** (ha hello tárolt eljárás paraméterek fogadja el).</span><span class="sxs-lookup"><span data-stu-id="6cd21-494">Alternatively, you can specify a stored procedure by specifying hello **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if hello stored procedure takes parameters).</span></span>
>
> <span data-ttu-id="6cd21-495">Ha nem ad meg sqlReaderQuery vagy sqlReaderStoredProcedureName, hello adatkészlet JSON hello struktúra szakaszban meghatározott hello oszlopok-e a használt toobuild (válassza Oszlop1, column2 from tábla) lekérdezés toorun hello Azure SQL Data Warehouse ellen.</span><span class="sxs-lookup"><span data-stu-id="6cd21-495">If you do not specify either sqlReaderQuery or sqlReaderStoredProcedureName, hello columns defined in hello structure section of hello dataset JSON are used toobuild a query (select column1, column2 from mytable) toorun against hello Azure SQL Data Warehouse.</span></span> <span data-ttu-id="6cd21-496">Hello adatkészlet definíciója nem rendelkezik hello struktúra, ha minden kiválasztott oszlop. a hello táblából.</span><span class="sxs-lookup"><span data-stu-id="6cd21-496">If hello dataset definition does not have hello structure, all columns are selected from hello table.</span></span>
>
>

### <a name="example-copy-data-from-azure-blob-tooazure-sql-data-warehouse"></a><span data-ttu-id="6cd21-497">Példa: Adatok másolása az Azure Blob tooAzure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="6cd21-497">Example: Copy data from Azure Blob tooAzure SQL Data Warehouse</span></span>
<span data-ttu-id="6cd21-498">hello minta meghatározza, hogy a következő adat-előállító entitások hello:</span><span class="sxs-lookup"><span data-stu-id="6cd21-498">hello sample defines hello following Data Factory entities:</span></span>

1. <span data-ttu-id="6cd21-499">A társított szolgáltatás típusa [AzureSqlDW](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="6cd21-499">A linked service of type [AzureSqlDW](#linked-service-properties).</span></span>
2. <span data-ttu-id="6cd21-500">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="6cd21-500">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="6cd21-501">Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="6cd21-501">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="6cd21-502">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureSqlDWTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="6cd21-502">An output [dataset](data-factory-create-datasets.md) of type [AzureSqlDWTable](#dataset-properties).</span></span>
5. <span data-ttu-id="6cd21-503">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) és [SqlDWSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="6cd21-503">A [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) and [SqlDWSink](#copy-activity-properties).</span></span>

<span data-ttu-id="6cd21-504">hello minta idősorozat adatainak másolása (óránként, naponta, stb.) az Azure SQL Data Warehouse-adatbázis az Azure blob tooa táblából óránként.</span><span class="sxs-lookup"><span data-stu-id="6cd21-504">hello sample copies time-series data (hourly, daily, etc.) from Azure blob tooa table in Azure SQL Data Warehouse database every hour.</span></span> <span data-ttu-id="6cd21-505">Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="6cd21-505">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="6cd21-506">**A társított szolgáltatásnak Azure SQL Data Warehouse:**</span><span class="sxs-lookup"><span data-stu-id="6cd21-506">**Azure SQL Data Warehouse linked service:**</span></span>

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
<span data-ttu-id="6cd21-507">**Az Azure Blob storage társított szolgáltatásnak:**</span><span class="sxs-lookup"><span data-stu-id="6cd21-507">**Azure Blob storage linked service:**</span></span>

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
<span data-ttu-id="6cd21-508">**Az Azure Blob bemeneti adatkészletet:**</span><span class="sxs-lookup"><span data-stu-id="6cd21-508">**Azure Blob input dataset:**</span></span>

<span data-ttu-id="6cd21-509">Adatok van felvett egy új blobból minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="6cd21-509">Data is picked up from a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="6cd21-510">hello mappa elérési útját és nevét hello blob dinamikusan értékeli ki a rendszer által feldolgozott hello szelet hello kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="6cd21-510">hello folder path and file name for hello blob are dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="6cd21-511">hello mappa elérési útját használja év, hónap és nap részét hello kezdési ideje, valamint fájlnév hello kezdő időpontja óra részét hello.</span><span class="sxs-lookup"><span data-stu-id="6cd21-511">hello folder path uses year, month, and day part of hello start time and file name uses hello hour part of hello start time.</span></span> <span data-ttu-id="6cd21-512">"external": "true" beállítás arról értesíti az, hogy ez a táblázat külső toohello adat-előállító és hello adat-előállítóban tevékenység nem hozzák hello Data Factory szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="6cd21-512">“external”: “true” setting informs hello Data Factory service that this table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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
<span data-ttu-id="6cd21-513">**Az SQL Data Warehouse kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="6cd21-513">**Azure SQL Data Warehouse output dataset:**</span></span>

<span data-ttu-id="6cd21-514">hello minta másolja át az Azure SQL Data Warehouse "MyTable" nevű tooa adattábla.</span><span class="sxs-lookup"><span data-stu-id="6cd21-514">hello sample copies data tooa table named “MyTable” in Azure SQL Data Warehouse.</span></span> <span data-ttu-id="6cd21-515">Hello tábla létrehozása az Azure SQL Data Warehouse azonos számú oszlopot hello hello Blob CSV-fájl toocontain várt.</span><span class="sxs-lookup"><span data-stu-id="6cd21-515">Create hello table in Azure SQL Data Warehouse with hello same number of columns as you expect hello Blob CSV file toocontain.</span></span> <span data-ttu-id="6cd21-516">Új sorok hozzáadásakor toohello tábla óránként.</span><span class="sxs-lookup"><span data-stu-id="6cd21-516">New rows are added toohello table every hour.</span></span>

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
<span data-ttu-id="6cd21-517">**Másolási tevékenység során a folyamat BlobSource és SqlDWSink:**</span><span class="sxs-lookup"><span data-stu-id="6cd21-517">**Copy activity in a pipeline with BlobSource and SqlDWSink:**</span></span>

<span data-ttu-id="6cd21-518">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="6cd21-518">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="6cd21-519">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**BlobSource** és **fogadó** típusuk értéke túl**SqlDWSink**.</span><span class="sxs-lookup"><span data-stu-id="6cd21-519">In hello pipeline JSON definition, hello **source** type is set too**BlobSource** and **sink** type is set too**SqlDWSink**.</span></span>

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
<span data-ttu-id="6cd21-520">Útmutatást lásd: lásd: hello [1 TB-os betöltése az Azure SQL Data Warehouse a 15 perc Azure Data Factory](data-factory-load-sql-data-warehouse.md) és [adatok betöltése az Azure Data Factory](../sql-data-warehouse/sql-data-warehouse-get-started-load-with-azure-data-factory.md) hello Azure SQL Data Warehouse cikk dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="6cd21-520">For a walkthrough, see hello see [Load 1 TB into Azure SQL Data Warehouse under 15 minutes with Azure Data Factory](data-factory-load-sql-data-warehouse.md) and [Load data with Azure Data Factory](../sql-data-warehouse/sql-data-warehouse-get-started-load-with-azure-data-factory.md) article in hello Azure SQL Data Warehouse documentation.</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="6cd21-521">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="6cd21-521">Performance and Tuning</span></span>
<span data-ttu-id="6cd21-522">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.</span><span class="sxs-lookup"><span data-stu-id="6cd21-522">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
