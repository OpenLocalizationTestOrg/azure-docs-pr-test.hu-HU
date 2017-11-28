---
title: az Azure SQL Database aaaCopy adatok |} Microsoft Docs
description: "Megtudhatja, hogyan toocopy adatokat az Azure SQL adatbázis Azure Data Factory használatával."
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
ms.openlocfilehash: d2ff16191afb028da75699c5e4d0bb310538db0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tooand-from-azure-sql-database-using-azure-data-factory"></a><span data-ttu-id="958a3-103">Adatok tooand másolása az Azure SQL adatbázis Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="958a3-103">Copy data tooand from Azure SQL Database using Azure Data Factory</span></span>
<span data-ttu-id="958a3-104">Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove adatok tooand az Azure SQL Database a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="958a3-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data tooand from Azure SQL Database.</span></span> <span data-ttu-id="958a3-105">-Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.</span><span class="sxs-lookup"><span data-stu-id="958a3-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>  

## <a name="supported-scenarios"></a><span data-ttu-id="958a3-106">Támogatott helyzetek</span><span class="sxs-lookup"><span data-stu-id="958a3-106">Supported scenarios</span></span>
<span data-ttu-id="958a3-107">Adatokat másolhat **az Azure SQL Database** toohello a következő adatokat tárolja:</span><span class="sxs-lookup"><span data-stu-id="958a3-107">You can copy data **from Azure SQL Database** toohello following data stores:</span></span>

[!INCLUDE [data-factory-supported-sinks](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="958a3-108">Adatok másolása a következő adatokat tárolja hello **tooAzure SQL-adatbázis**:</span><span class="sxs-lookup"><span data-stu-id="958a3-108">You can copy data from hello following data stores **tooAzure SQL Database**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

## <a name="supported-authentication-type"></a><span data-ttu-id="958a3-109">Támogatott hitelesítési típushoz</span><span class="sxs-lookup"><span data-stu-id="958a3-109">Supported authentication type</span></span>
<span data-ttu-id="958a3-110">Az Azure SQL Database-összekötő az egyszerű hitelesítést támogatja.</span><span class="sxs-lookup"><span data-stu-id="958a3-110">Azure SQL Database connector supports basic authentication.</span></span>

## <a name="getting-started"></a><span data-ttu-id="958a3-111">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="958a3-111">Getting started</span></span>
<span data-ttu-id="958a3-112">A másolási tevékenység, amely helyezi át az adatokat belőle egy Azure SQL Database különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="958a3-112">You can create a pipeline with a copy activity that moves data to/from an Azure SQL Database by using different tools/APIs.</span></span>

<span data-ttu-id="958a3-113">hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="958a3-113">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="958a3-114">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.</span><span class="sxs-lookup"><span data-stu-id="958a3-114">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="958a3-115">Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**.</span><span class="sxs-lookup"><span data-stu-id="958a3-115">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="958a3-116">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.</span><span class="sxs-lookup"><span data-stu-id="958a3-116">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="958a3-117">Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:</span><span class="sxs-lookup"><span data-stu-id="958a3-117">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span> 

1. <span data-ttu-id="958a3-118">Hozzon létre egy **adat-előállító**.</span><span class="sxs-lookup"><span data-stu-id="958a3-118">Create a **data factory**.</span></span> <span data-ttu-id="958a3-119">Egy adat-előállító tartalmazhat egy vagy több folyamatok.</span><span class="sxs-lookup"><span data-stu-id="958a3-119">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="958a3-120">Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="958a3-120">Create **linked services** toolink input and output data stores tooyour data factory.</span></span> <span data-ttu-id="958a3-121">Adatok másolása az Azure blob storage tooan Azure SQL-adatbázis, akkor hozzon létre például két összekapcsolt szolgáltatások toolink az Azure storage-fiók és az Azure SQL adatbázis tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="958a3-121">For example, if you are copying data from an Azure blob storage tooan Azure SQL database, you create two linked services toolink your Azure storage account and Azure SQL database tooyour data factory.</span></span> <span data-ttu-id="958a3-122">Csatolt szolgáltatás tulajdonságait, amelyek adott tooAzure SQL-adatbázis, lásd: [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="958a3-122">For linked service properties that are specific tooAzure SQL Database, see [linked service properties](#linked-service-properties) section.</span></span> 
3. <span data-ttu-id="958a3-123">Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet.</span><span class="sxs-lookup"><span data-stu-id="958a3-123">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> <span data-ttu-id="958a3-124">Hello utolsó lépésében említett hello például létrehoz egy adatkészlet toospecify hello blobtárolót és hello bemeneti adatokat tartalmazó mappát.</span><span class="sxs-lookup"><span data-stu-id="958a3-124">In hello example mentioned in hello last step, you create a dataset toospecify hello blob container and folder that contains hello input data.</span></span> <span data-ttu-id="958a3-125">És egy másik dataset toospecify hello SQL táblázat hello blob-tároló átmásolva hello adatokat tartalmazó hello Azure SQL-adatbázis létrehozása.</span><span class="sxs-lookup"><span data-stu-id="958a3-125">And, you create another dataset toospecify hello SQL table in hello Azure SQL database  that holds hello data copied from hello blob storage.</span></span> <span data-ttu-id="958a3-126">Adatkészlet tulajdonságai, amelyek adott tooAzure Data Lake Store, lásd: [adatkészlet tulajdonságai](#dataset-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="958a3-126">For dataset properties that are specific tooAzure Data Lake Store, see [dataset properties](#dataset-properties) section.</span></span>
4. <span data-ttu-id="958a3-127">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="958a3-127">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="958a3-128">A korábban említett hello példában BlobSource forrás-és SqlSink akár használhatja a fogadó hello másolási tevékenységhez.</span><span class="sxs-lookup"><span data-stu-id="958a3-128">In hello example mentioned earlier, you use BlobSource as a source and SqlSink as a sink for hello copy activity.</span></span> <span data-ttu-id="958a3-129">Hasonlóképpen a Blob Storage Azure SQL Database tooAzure másolása, használható SqlSource és BlobSink hello másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="958a3-129">Similarly, if you are copying from Azure SQL Database tooAzure Blob Storage, you use SqlSource and BlobSink in hello copy activity.</span></span> <span data-ttu-id="958a3-130">A másolási tevékenység tulajdonságait, amelyek adott tooAzure SQL-adatbázis, lásd: [tevékenység Tulajdonságok másolása](#copy-activity-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="958a3-130">For copy activity properties that are specific tooAzure SQL Database, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="958a3-131">További információkért hogyan toouse egy adatok tárolót, mint a forrás- és a fogadó hivatkozásra hello az adattároló hello előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="958a3-131">For details on how toouse a data store as a source or a sink, click hello link in hello previous section for your data store.</span></span>

<span data-ttu-id="958a3-132">Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="958a3-132">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="958a3-133">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="958a3-133">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="958a3-134">Az adat-előállító entitások, amelyek az Azure SQL adatbázis használt toocopy adatok JSON-definíciók minták, lásd: [JSON példák](#json-examples-for-copying-data-to-and-from-sql-database) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="958a3-134">For samples with JSON definitions for Data Factory entities that are used toocopy data to/from an Azure SQL Database, see [JSON examples](#json-examples-for-copying-data-to-and-from-sql-database) section of this article.</span></span> 

<span data-ttu-id="958a3-135">a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooAzure SQL-adatbázis részleteit tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="958a3-135">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooAzure SQL Database:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="958a3-136">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="958a3-136">Linked service properties</span></span>
<span data-ttu-id="958a3-137">Egy Azure SQL társított szolgáltatás hivatkozások egy Azure SQL adatbázis tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="958a3-137">An Azure SQL linked service links an Azure SQL database tooyour data factory.</span></span> <span data-ttu-id="958a3-138">a következő táblázat hello biztosít leírását a megadott JSON-elemek tooAzure SQL társított szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="958a3-138">hello following table provides description for JSON elements specific tooAzure SQL linked service.</span></span>

| <span data-ttu-id="958a3-139">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="958a3-139">Property</span></span> | <span data-ttu-id="958a3-140">Leírás</span><span class="sxs-lookup"><span data-stu-id="958a3-140">Description</span></span> | <span data-ttu-id="958a3-141">Szükséges</span><span class="sxs-lookup"><span data-stu-id="958a3-141">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="958a3-142">type</span><span class="sxs-lookup"><span data-stu-id="958a3-142">type</span></span> |<span data-ttu-id="958a3-143">hello type tulajdonságot kell beállítani: **AzureSqlDatabase**</span><span class="sxs-lookup"><span data-stu-id="958a3-143">hello type property must be set to: **AzureSqlDatabase**</span></span> |<span data-ttu-id="958a3-144">Igen</span><span class="sxs-lookup"><span data-stu-id="958a3-144">Yes</span></span> |
| <span data-ttu-id="958a3-145">connectionString</span><span class="sxs-lookup"><span data-stu-id="958a3-145">connectionString</span></span> |<span data-ttu-id="958a3-146">Adjon meg információt hello connectionString tulajdonság szükséges tooconnect toohello Azure SQL Database-példányt.</span><span class="sxs-lookup"><span data-stu-id="958a3-146">Specify information needed tooconnect toohello Azure SQL Database instance for hello connectionString property.</span></span> <span data-ttu-id="958a3-147">Csak az alapszintű hitelesítést is támogatja.</span><span class="sxs-lookup"><span data-stu-id="958a3-147">Only basic authentication is supported.</span></span> |<span data-ttu-id="958a3-148">Igen</span><span class="sxs-lookup"><span data-stu-id="958a3-148">Yes</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="958a3-149">Konfigurálása [Azure SQL Database-tűzfal](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) adatbázis-kiszolgáló túl hello[engedélyezése az Azure-szolgáltatások tooaccess hello server](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure).</span><span class="sxs-lookup"><span data-stu-id="958a3-149">Configure [Azure SQL Database Firewall](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) hello database server too[allow Azure Services tooaccess hello server](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure).</span></span> <span data-ttu-id="958a3-150">Emellett külső Azure többek között a data factory átjáró a helyszíni adatforrásokból származó adatok tooAzure SQL-adatbázis másolása, beállítható, megfelelő IP-címtartomány hello gép küldő adatok tooAzure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="958a3-150">Additionally, if you are copying data tooAzure SQL Database from outside Azure including from on-premises data sources with data factory gateway, configure appropriate IP address range for hello machine that is sending data tooAzure SQL Database.</span></span>

## <a name="dataset-properties"></a><span data-ttu-id="958a3-151">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="958a3-151">Dataset properties</span></span>
<span data-ttu-id="958a3-152">a dataset toorepresent toospecify hello adatkészlet hello type tulajdonsága bemeneti vagy kimeneti adatok Azure SQL adatbázis beállítása: **AzureSqlTable**.</span><span class="sxs-lookup"><span data-stu-id="958a3-152">toospecify a dataset toorepresent input or output data in an Azure SQL database, you set hello type property of hello dataset to: **AzureSqlTable**.</span></span> <span data-ttu-id="958a3-153">Set hello **linkedServiceName** tulajdonság hello dataset toohello nevének hello Azure SQL társított szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="958a3-153">Set hello **linkedServiceName** property of hello dataset toohello name of hello Azure SQL linked service.</span></span>  

<span data-ttu-id="958a3-154">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="958a3-154">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="958a3-155">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="958a3-155">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="958a3-156">hello typeProperties szakasz más adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti.</span><span class="sxs-lookup"><span data-stu-id="958a3-156">hello typeProperties section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="958a3-157">Hello **typeProperties** típusú hello adatkészlet szakasz **AzureSqlTable** rendelkezik hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="958a3-157">hello **typeProperties** section for hello dataset of type **AzureSqlTable** has hello following properties:</span></span>

| <span data-ttu-id="958a3-158">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="958a3-158">Property</span></span> | <span data-ttu-id="958a3-159">Leírás</span><span class="sxs-lookup"><span data-stu-id="958a3-159">Description</span></span> | <span data-ttu-id="958a3-160">Szükséges</span><span class="sxs-lookup"><span data-stu-id="958a3-160">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="958a3-161">tableName</span><span class="sxs-lookup"><span data-stu-id="958a3-161">tableName</span></span> |<span data-ttu-id="958a3-162">Hello tábla vagy nézet hello Azure SQL Database-példányt, amelyre a társított szolgáltatás neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="958a3-162">Name of hello table or view in hello Azure SQL Database instance that linked service refers to.</span></span> |<span data-ttu-id="958a3-163">Igen</span><span class="sxs-lookup"><span data-stu-id="958a3-163">Yes</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="958a3-164">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="958a3-164">Copy activity properties</span></span>
<span data-ttu-id="958a3-165">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="958a3-165">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="958a3-166">Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="958a3-166">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

> [!NOTE]
> <span data-ttu-id="958a3-167">hello másolási tevékenység során csak egy bemenettel rendelkezik, és csak egy kimenetet.</span><span class="sxs-lookup"><span data-stu-id="958a3-167">hello Copy Activity takes only one input and produces only one output.</span></span>

<span data-ttu-id="958a3-168">Mivel tulajdonságok érhetők el hello **typeProperties** hello tevékenység szakasza tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="958a3-168">Whereas, properties available in hello **typeProperties** section of hello activity vary with each activity type.</span></span> <span data-ttu-id="958a3-169">A másolási tevékenység során két érték források és mosdók hello típusától függően.</span><span class="sxs-lookup"><span data-stu-id="958a3-169">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="958a3-170">Ha adatokat az Azure SQL-adatbázis, beállítása hello forrástípus hello másolási tevékenység túl**SqlSource**.</span><span class="sxs-lookup"><span data-stu-id="958a3-170">If you are moving data from an Azure SQL database, you set hello source type in hello copy activity too**SqlSource**.</span></span> <span data-ttu-id="958a3-171">Hasonlóképpen, ha az tooan Azure SQL-adatbázist, beállítása hello a fogadó típusa hello másolási tevékenység túl**SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="958a3-171">Similarly, if you are moving data tooan Azure SQL database, you set hello sink type in hello copy activity too**SqlSink**.</span></span> <span data-ttu-id="958a3-172">Ez a témakör SqlSource és SqlSink által támogatott tulajdonságokról.</span><span class="sxs-lookup"><span data-stu-id="958a3-172">This section provides a list of properties supported by SqlSource and SqlSink.</span></span>

### <a name="sqlsource"></a><span data-ttu-id="958a3-173">SqlSource</span><span class="sxs-lookup"><span data-stu-id="958a3-173">SqlSource</span></span>
<span data-ttu-id="958a3-174">A másolási tevékenység, ha hello adatforrás típusú **SqlSource**, hello a következő tulajdonságok érhetők el **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="958a3-174">In copy activity, when hello source is of type **SqlSource**, hello following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="958a3-175">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="958a3-175">Property</span></span> | <span data-ttu-id="958a3-176">Leírás</span><span class="sxs-lookup"><span data-stu-id="958a3-176">Description</span></span> | <span data-ttu-id="958a3-177">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="958a3-177">Allowed values</span></span> | <span data-ttu-id="958a3-178">Szükséges</span><span class="sxs-lookup"><span data-stu-id="958a3-178">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="958a3-179">sqlReaderQuery</span><span class="sxs-lookup"><span data-stu-id="958a3-179">sqlReaderQuery</span></span> |<span data-ttu-id="958a3-180">Hello egyéni lekérdezés tooread adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="958a3-180">Use hello custom query tooread data.</span></span> |<span data-ttu-id="958a3-181">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="958a3-181">SQL query string.</span></span> <span data-ttu-id="958a3-182">Példa: `select * from MyTable`.</span><span class="sxs-lookup"><span data-stu-id="958a3-182">Example: `select * from MyTable`.</span></span> |<span data-ttu-id="958a3-183">Nem</span><span class="sxs-lookup"><span data-stu-id="958a3-183">No</span></span> |
| <span data-ttu-id="958a3-184">sqlReaderStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="958a3-184">sqlReaderStoredProcedureName</span></span> |<span data-ttu-id="958a3-185">Hello neve tárolt eljárást, amely hello forrástábla olvassa be az adatokat.</span><span class="sxs-lookup"><span data-stu-id="958a3-185">Name of hello stored procedure that reads data from hello source table.</span></span> |<span data-ttu-id="958a3-186">Hello neve tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="958a3-186">Name of hello stored procedure.</span></span> <span data-ttu-id="958a3-187">hello utolsó SQL-utasítás hello tárolt eljárás SELECT utasítással kell lennie.</span><span class="sxs-lookup"><span data-stu-id="958a3-187">hello last SQL statement must be a SELECT statement in hello stored procedure.</span></span> |<span data-ttu-id="958a3-188">Nem</span><span class="sxs-lookup"><span data-stu-id="958a3-188">No</span></span> |
| <span data-ttu-id="958a3-189">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="958a3-189">storedProcedureParameters</span></span> |<span data-ttu-id="958a3-190">Hello paramétereinek tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="958a3-190">Parameters for hello stored procedure.</span></span> |<span data-ttu-id="958a3-191">A név/érték párok.</span><span class="sxs-lookup"><span data-stu-id="958a3-191">Name/value pairs.</span></span> <span data-ttu-id="958a3-192">Nevek és a kis-és paraméterek meg kell egyeznie hello nevét és a kis-és nagybetűhasználat hello tárolt eljárás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="958a3-192">Names and casing of parameters must match hello names and casing of hello stored procedure parameters.</span></span> |<span data-ttu-id="958a3-193">Nem</span><span class="sxs-lookup"><span data-stu-id="958a3-193">No</span></span> |

<span data-ttu-id="958a3-194">Ha hello **sqlReaderQuery** megadott hello SqlSource, hello másolási tevékenység során ez a lekérdezés futtatása hello Azure SQL Database forrás tooget hello adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="958a3-194">If hello **sqlReaderQuery** is specified for hello SqlSource, hello Copy Activity runs this query against hello Azure SQL Database source tooget hello data.</span></span> <span data-ttu-id="958a3-195">Másik lehetőségként megadhat tárolt eljárás hello megadásával **sqlReaderStoredProcedureName** és **storedProcedureParameters** (ha hello tárolt eljárás paraméterek fogadja el).</span><span class="sxs-lookup"><span data-stu-id="958a3-195">Alternatively, you can specify a stored procedure by specifying hello **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if hello stored procedure takes parameters).</span></span>

<span data-ttu-id="958a3-196">Ha nem ad meg sqlReaderQuery vagy sqlReaderStoredProcedureName, hello adatkészlet JSON hello struktúra szakaszban meghatározott hello oszlopok-e a használt toobuild lekérdezés (`select column1, column2 from mytable`) toorun hello Azure SQL Database ellen.</span><span class="sxs-lookup"><span data-stu-id="958a3-196">If you do not specify either sqlReaderQuery or sqlReaderStoredProcedureName, hello columns defined in hello structure section of hello dataset JSON are used toobuild a query (`select column1, column2 from mytable`) toorun against hello Azure SQL Database.</span></span> <span data-ttu-id="958a3-197">Hello adatkészlet definíciója nem rendelkezik hello struktúra, ha minden kiválasztott oszlop. a hello táblából.</span><span class="sxs-lookup"><span data-stu-id="958a3-197">If hello dataset definition does not have hello structure, all columns are selected from hello table.</span></span>

> [!NOTE]
> <span data-ttu-id="958a3-198">Használata esetén **sqlReaderStoredProcedureName**, továbbra is szükséges toospecify értéket hello **tableName** hello adatkészlet JSON tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="958a3-198">When you use **sqlReaderStoredProcedureName**, you still need toospecify a value for hello **tableName** property in hello dataset JSON.</span></span> <span data-ttu-id="958a3-199">Nincs érvényesítést hajt végre ezt a táblázatot, ha van.</span><span class="sxs-lookup"><span data-stu-id="958a3-199">There are no validations performed against this table though.</span></span>
>
>

### <a name="sqlsource-example"></a><span data-ttu-id="958a3-200">SqlSource – példa</span><span class="sxs-lookup"><span data-stu-id="958a3-200">SqlSource example</span></span>

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

<span data-ttu-id="958a3-201">**hello tárolt eljárás definíciója:**</span><span class="sxs-lookup"><span data-stu-id="958a3-201">**hello stored procedure definition:**</span></span>

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

### <a name="sqlsink"></a><span data-ttu-id="958a3-202">SqlSink</span><span class="sxs-lookup"><span data-stu-id="958a3-202">SqlSink</span></span>
<span data-ttu-id="958a3-203">**SqlSink** következő tulajdonságai hello támogatja:</span><span class="sxs-lookup"><span data-stu-id="958a3-203">**SqlSink** supports hello following properties:</span></span>

| <span data-ttu-id="958a3-204">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="958a3-204">Property</span></span> | <span data-ttu-id="958a3-205">Leírás</span><span class="sxs-lookup"><span data-stu-id="958a3-205">Description</span></span> | <span data-ttu-id="958a3-206">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="958a3-206">Allowed values</span></span> | <span data-ttu-id="958a3-207">Szükséges</span><span class="sxs-lookup"><span data-stu-id="958a3-207">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="958a3-208">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="958a3-208">writeBatchTimeout</span></span> |<span data-ttu-id="958a3-209">Várnia kell az hello kötegelt beszúrási művelet toocomplete előtt azt az időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="958a3-209">Wait time for hello batch insert operation toocomplete before it times out.</span></span> |<span data-ttu-id="958a3-210">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="958a3-210">timespan</span></span><br/><br/> <span data-ttu-id="958a3-211">Példa: "00: 30:00" (30 perc).</span><span class="sxs-lookup"><span data-stu-id="958a3-211">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="958a3-212">Nem</span><span class="sxs-lookup"><span data-stu-id="958a3-212">No</span></span> |
| <span data-ttu-id="958a3-213">WriteBatchSize</span><span class="sxs-lookup"><span data-stu-id="958a3-213">writeBatchSize</span></span> |<span data-ttu-id="958a3-214">Amikor hello puffer mérete eléri writeBatchSize adatok beillesztése hello SQL táblázat.</span><span class="sxs-lookup"><span data-stu-id="958a3-214">Inserts data into hello SQL table when hello buffer size reaches writeBatchSize.</span></span> |<span data-ttu-id="958a3-215">Egész szám (sorok száma)</span><span class="sxs-lookup"><span data-stu-id="958a3-215">Integer (number of rows)</span></span> |<span data-ttu-id="958a3-216">Nem (alapértelmezett: 10000)</span><span class="sxs-lookup"><span data-stu-id="958a3-216">No (default: 10000)</span></span> |
| <span data-ttu-id="958a3-217">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="958a3-217">sqlWriterCleanupScript</span></span> |<span data-ttu-id="958a3-218">Adja meg a másolási tevékenység tooexecute vonatkozó lekérdezést úgy, hogy egy adott szelet adatait.</span><span class="sxs-lookup"><span data-stu-id="958a3-218">Specify a query for Copy Activity tooexecute such that data of a specific slice is cleaned up.</span></span> <span data-ttu-id="958a3-219">További információkért lásd: [ismételhető másolási](#repeatable-copy).</span><span class="sxs-lookup"><span data-stu-id="958a3-219">For more information, see [repeatable copy](#repeatable-copy).</span></span> |<span data-ttu-id="958a3-220">A lekérdezési utasítást.</span><span class="sxs-lookup"><span data-stu-id="958a3-220">A query statement.</span></span> |<span data-ttu-id="958a3-221">Nem</span><span class="sxs-lookup"><span data-stu-id="958a3-221">No</span></span> |
| <span data-ttu-id="958a3-222">sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="958a3-222">sliceIdentifierColumnName</span></span> |<span data-ttu-id="958a3-223">Adja meg oszlop nevét, a másolási tevékenység toofill szelet azonosító automatikusan létrejön, vagyis amikor futtassa újra a megadott szelet adatainak használt tooclean.</span><span class="sxs-lookup"><span data-stu-id="958a3-223">Specify a column name for Copy Activity toofill with auto generated slice identifier, which is used tooclean up data of a specific slice when rerun.</span></span> <span data-ttu-id="958a3-224">További információkért lásd: [ismételhető másolási](#repeatable-copy).</span><span class="sxs-lookup"><span data-stu-id="958a3-224">For more information, see [repeatable copy](#repeatable-copy).</span></span> |<span data-ttu-id="958a3-225">Egy oszlop binary(32) adattípusú oszlop neve.</span><span class="sxs-lookup"><span data-stu-id="958a3-225">Column name of a column with data type of binary(32).</span></span> |<span data-ttu-id="958a3-226">Nem</span><span class="sxs-lookup"><span data-stu-id="958a3-226">No</span></span> |
| <span data-ttu-id="958a3-227">sqlWriterStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="958a3-227">sqlWriterStoredProcedureName</span></span> |<span data-ttu-id="958a3-228">Hello nevét (frissítés/Beszúrás) upserts adatok tárolt eljárás hello cél táblába.</span><span class="sxs-lookup"><span data-stu-id="958a3-228">Name of hello stored procedure that upserts (updates/inserts) data into hello target table.</span></span> |<span data-ttu-id="958a3-229">Hello neve tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="958a3-229">Name of hello stored procedure.</span></span> |<span data-ttu-id="958a3-230">Nem</span><span class="sxs-lookup"><span data-stu-id="958a3-230">No</span></span> |
| <span data-ttu-id="958a3-231">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="958a3-231">storedProcedureParameters</span></span> |<span data-ttu-id="958a3-232">Hello paramétereinek tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="958a3-232">Parameters for hello stored procedure.</span></span> |<span data-ttu-id="958a3-233">A név/érték párok.</span><span class="sxs-lookup"><span data-stu-id="958a3-233">Name/value pairs.</span></span> <span data-ttu-id="958a3-234">Nevek és a kis-és paraméterek meg kell egyeznie hello nevét és a kis-és nagybetűhasználat hello tárolt eljárás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="958a3-234">Names and casing of parameters must match hello names and casing of hello stored procedure parameters.</span></span> |<span data-ttu-id="958a3-235">Nem</span><span class="sxs-lookup"><span data-stu-id="958a3-235">No</span></span> |
| <span data-ttu-id="958a3-236">sqlWriterTableType</span><span class="sxs-lookup"><span data-stu-id="958a3-236">sqlWriterTableType</span></span> |<span data-ttu-id="958a3-237">Adjon meg egy tábla Típus neve toobe hello tárolt eljárásban használt.</span><span class="sxs-lookup"><span data-stu-id="958a3-237">Specify a table type name toobe used in hello stored procedure.</span></span> <span data-ttu-id="958a3-238">Másolási tevékenység elérhetővé teszi hello adatok éppen áthelyezik egy ideiglenes táblát, amely a táblatípus.</span><span class="sxs-lookup"><span data-stu-id="958a3-238">Copy activity makes hello data being moved available in a temp table with this table type.</span></span> <span data-ttu-id="958a3-239">Tárolt eljárás kód majd egyesítheti a meglévő adatok másolásának hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="958a3-239">Stored procedure code can then merge hello data being copied with existing data.</span></span> |<span data-ttu-id="958a3-240">Egy tábla környezettípus nevét.</span><span class="sxs-lookup"><span data-stu-id="958a3-240">A table type name.</span></span> |<span data-ttu-id="958a3-241">Nem</span><span class="sxs-lookup"><span data-stu-id="958a3-241">No</span></span> |

#### <a name="sqlsink-example"></a><span data-ttu-id="958a3-242">SqlSink – példa</span><span class="sxs-lookup"><span data-stu-id="958a3-242">SqlSink example</span></span>

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

## <a name="json-examples-for-copying-data-tooand-from-sql-database"></a><span data-ttu-id="958a3-243">JSON Példák adatok tooand SQL-adatbázis másolása</span><span class="sxs-lookup"><span data-stu-id="958a3-243">JSON examples for copying data tooand from SQL Database</span></span>
<span data-ttu-id="958a3-244">hello alábbi példák megadják minta JSON-definíciók használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="958a3-244">hello following examples provide sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="958a3-245">Azok hogyan toocopy adatok tooand az Azure SQL Database és az Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="958a3-245">They show how toocopy data tooand from Azure SQL Database and Azure Blob Storage.</span></span> <span data-ttu-id="958a3-246">Azonban az adatok átmásolhatók **közvetlenül** bármelyik megadott hello nyelő források tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.</span><span class="sxs-lookup"><span data-stu-id="958a3-246">However, data can be copied **directly** from any of sources tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>

### <a name="example-copy-data-from-azure-sql-database-tooazure-blob"></a><span data-ttu-id="958a3-247">Példa: Adatok másolása az Azure SQL Database tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="958a3-247">Example: Copy data from Azure SQL Database tooAzure Blob</span></span>
<span data-ttu-id="958a3-248">hello azonos meghatározza, hogy a következő adat-előállító entitások hello:</span><span class="sxs-lookup"><span data-stu-id="958a3-248">hello same defines hello following Data Factory entities:</span></span>

1. <span data-ttu-id="958a3-249">A társított szolgáltatás típusa [AzureSqlDatabase](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="958a3-249">A linked service of type [AzureSqlDatabase](#linked-service-properties).</span></span>
2. <span data-ttu-id="958a3-250">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="958a3-250">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="958a3-251">Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureSqlTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="958a3-251">An input [dataset](data-factory-create-datasets.md) of type [AzureSqlTable](#dataset-properties).</span></span>
4. <span data-ttu-id="958a3-252">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [Azure Blob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="958a3-252">An output [dataset](data-factory-create-datasets.md) of type [Azure Blob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="958a3-253">A [csővezeték](data-factory-create-pipelines.md) , a másolási tevékenység által használt [SqlSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="958a3-253">A [pipeline](data-factory-create-pipelines.md) with a Copy activity that uses [SqlSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="958a3-254">hello minta idősorozat adatainak másolása (óránként, naponta, stb.) az Azure SQL adatbázis tooa blob egy táblázatban minden órában.</span><span class="sxs-lookup"><span data-stu-id="958a3-254">hello sample copies time-series data (hourly, daily, etc.) from a table in Azure SQL database tooa blob every hour.</span></span> <span data-ttu-id="958a3-255">Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="958a3-255">hello JSON properties used in these samples are described in sections following hello samples.</span></span>  

<span data-ttu-id="958a3-256">**A társított szolgáltatásnak Azure SQL Database:**</span><span class="sxs-lookup"><span data-stu-id="958a3-256">**Azure SQL Database linked service:**</span></span>

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
<span data-ttu-id="958a3-257">Lásd: hello [Azure SQL társított szolgáltatást](#linked-service) hello lista szakasza a társított szolgáltatás által támogatott tulajdonságokról.</span><span class="sxs-lookup"><span data-stu-id="958a3-257">See hello [Azure SQL Linked Service](#linked-service) section for hello list of properties supported by this linked service.</span></span>

<span data-ttu-id="958a3-258">**Az Azure Blob storage társított szolgáltatásnak:**</span><span class="sxs-lookup"><span data-stu-id="958a3-258">**Azure Blob storage linked service:**</span></span>

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
<span data-ttu-id="958a3-259">Lásd: hello [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) cikkében hello listát a társított szolgáltatás által támogatott tulajdonságokról.</span><span class="sxs-lookup"><span data-stu-id="958a3-259">See hello [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) article for hello list of properties supported by this linked service.</span></span>


<span data-ttu-id="958a3-260">**Az Azure SQL bemeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="958a3-260">**Azure SQL input dataset:**</span></span>

<span data-ttu-id="958a3-261">hello minta azt feltételezi, hogy létrehozott egy tábla "MyTable" Azure SQL, egy "timestampcolumn" nevű adatsorozat időadatok oszlopot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="958a3-261">hello sample assumes you have created a table “MyTable” in Azure SQL and it contains a column called “timestampcolumn” for time series data.</span></span>

<span data-ttu-id="958a3-262">"External" beállítása: "true" tájékoztatja hello Azure Data Factory szolgáltatásnak, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="958a3-262">Setting “external”: ”true” informs hello Azure Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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

<span data-ttu-id="958a3-263">Lásd: hello [Azure SQL dataset típustulajdonságokat](#dataset) ehhez az adathalmaztípushoz által támogatott tulajdonságokról szakasza hello listáját.</span><span class="sxs-lookup"><span data-stu-id="958a3-263">See hello [Azure SQL dataset type properties](#dataset) section for hello list of properties supported by this dataset type.</span></span>  

<span data-ttu-id="958a3-264">**Az Azure Blob kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="958a3-264">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="958a3-265">Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="958a3-265">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="958a3-266">hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="958a3-266">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="958a3-267">hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.</span><span class="sxs-lookup"><span data-stu-id="958a3-267">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

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
<span data-ttu-id="958a3-268">Lásd: hello [Azure-Blob adatkészletet típus tulajdonságainak](data-factory-azure-blob-connector.md#dataset-properties) hello lista szakasza ehhez az adathalmaztípushoz által támogatott tulajdonságokról.</span><span class="sxs-lookup"><span data-stu-id="958a3-268">See hello [Azure Blob dataset type properties](data-factory-azure-blob-connector.md#dataset-properties) section for hello list of properties supported by this dataset type.</span></span>  

<span data-ttu-id="958a3-269">**A másolási tevékenység során az SQL-forrás és fogadó Blob egy folyamaton belül:**</span><span class="sxs-lookup"><span data-stu-id="958a3-269">**A copy activity in a pipeline with SQL source and Blob sink:**</span></span>

<span data-ttu-id="958a3-270">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="958a3-270">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="958a3-271">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**SqlSource** és **fogadó** típusuk értéke túl**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="958a3-271">In hello pipeline JSON definition, hello **source** type is set too**SqlSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="958a3-272">hello SQL-lekérdezésben megadott hello **SqlReaderQuery** tulajdonság jelöli ki hello adatok hello toocopy óránként túlra.</span><span class="sxs-lookup"><span data-stu-id="958a3-272">hello SQL query specified for hello **SqlReaderQuery** property selects hello data in hello past hour toocopy.</span></span>

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
<span data-ttu-id="958a3-273">Hello példában **sqlReaderQuery** hello SqlSource van megadva.</span><span class="sxs-lookup"><span data-stu-id="958a3-273">In hello example, **sqlReaderQuery** is specified for hello SqlSource.</span></span> <span data-ttu-id="958a3-274">hello másolási tevékenység fut ez a lekérdezés hello Azure SQL Database forrásadatok tooget hello.</span><span class="sxs-lookup"><span data-stu-id="958a3-274">hello Copy Activity runs this query against hello Azure SQL Database source tooget hello data.</span></span> <span data-ttu-id="958a3-275">Másik lehetőségként megadhat tárolt eljárás hello megadásával **sqlReaderStoredProcedureName** és **storedProcedureParameters** (ha hello tárolt eljárás paraméterek fogadja el).</span><span class="sxs-lookup"><span data-stu-id="958a3-275">Alternatively, you can specify a stored procedure by specifying hello **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if hello stored procedure takes parameters).</span></span>

<span data-ttu-id="958a3-276">Ha nem ad meg sqlReaderQuery vagy sqlReaderStoredProcedureName, hello adatkészlet JSON hello struktúra szakaszban meghatározott hello oszlopok használt toobuild egy lekérdezés toorun elleni hello Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="958a3-276">If you do not specify either sqlReaderQuery or sqlReaderStoredProcedureName, hello columns defined in hello structure section of hello dataset JSON are used toobuild a query toorun against hello Azure SQL Database.</span></span> <span data-ttu-id="958a3-277">Például: `select column1, column2 from mytable`.</span><span class="sxs-lookup"><span data-stu-id="958a3-277">For example: `select column1, column2 from mytable`.</span></span> <span data-ttu-id="958a3-278">Hello adatkészlet definíciója nem rendelkezik hello struktúra, ha minden kiválasztott oszlop. a hello táblából.</span><span class="sxs-lookup"><span data-stu-id="958a3-278">If hello dataset definition does not have hello structure, all columns are selected from hello table.</span></span>

<span data-ttu-id="958a3-279">Lásd: hello [Sql-forrás](#sqlsource) szakasz és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) SqlSource és BlobSink által támogatott tulajdonságokról hello listáját.</span><span class="sxs-lookup"><span data-stu-id="958a3-279">See hello [Sql Source](#sqlsource) section and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) for hello list of properties supported by SqlSource and BlobSink.</span></span>

### <a name="example-copy-data-from-azure-blob-tooazure-sql-database"></a><span data-ttu-id="958a3-280">Példa: Adatok másolása az Azure Blob tooAzure SQL-adatbázis</span><span class="sxs-lookup"><span data-stu-id="958a3-280">Example: Copy data from Azure Blob tooAzure SQL Database</span></span>
<span data-ttu-id="958a3-281">hello minta meghatározza, hogy a következő adat-előállító entitások hello:</span><span class="sxs-lookup"><span data-stu-id="958a3-281">hello sample defines hello following Data Factory entities:</span></span>  

1. <span data-ttu-id="958a3-282">A társított szolgáltatás típusa [AzureSqlDatabase](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="958a3-282">A linked service of type [AzureSqlDatabase](#linked-service-properties).</span></span>
2. <span data-ttu-id="958a3-283">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="958a3-283">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="958a3-284">Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="958a3-284">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="958a3-285">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureSqlTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="958a3-285">An output [dataset](data-factory-create-datasets.md) of type [AzureSqlTable](#dataset-properties).</span></span>
5. <span data-ttu-id="958a3-286">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) és [SqlSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="958a3-286">A [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) and [SqlSink](#copy-activity-properties).</span></span>

<span data-ttu-id="958a3-287">hello minta idősorozat adatainak másolása (óránként, naponta, stb.) az Azure SQL database az Azure blob tooa táblából óránként.</span><span class="sxs-lookup"><span data-stu-id="958a3-287">hello sample copies time-series data (hourly, daily, etc.) from Azure blob tooa table in Azure SQL database every hour.</span></span> <span data-ttu-id="958a3-288">Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="958a3-288">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="958a3-289">**Az Azure SQL társított szolgáltatásnak:**</span><span class="sxs-lookup"><span data-stu-id="958a3-289">**Azure SQL linked service:**</span></span>

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
<span data-ttu-id="958a3-290">Lásd: hello [Azure SQL társított szolgáltatást](#linked-service) hello lista szakasza a társított szolgáltatás által támogatott tulajdonságokról.</span><span class="sxs-lookup"><span data-stu-id="958a3-290">See hello [Azure SQL Linked Service](#linked-service) section for hello list of properties supported by this linked service.</span></span>

<span data-ttu-id="958a3-291">**Az Azure Blob storage társított szolgáltatásnak:**</span><span class="sxs-lookup"><span data-stu-id="958a3-291">**Azure Blob storage linked service:**</span></span>

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
<span data-ttu-id="958a3-292">Lásd: hello [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) cikkében hello listát a társított szolgáltatás által támogatott tulajdonságokról.</span><span class="sxs-lookup"><span data-stu-id="958a3-292">See hello [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) article for hello list of properties supported by this linked service.</span></span>


<span data-ttu-id="958a3-293">**Az Azure Blob bemeneti adatkészletet:**</span><span class="sxs-lookup"><span data-stu-id="958a3-293">**Azure Blob input dataset:**</span></span>

<span data-ttu-id="958a3-294">Adatok van felvett egy új blobból minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="958a3-294">Data is picked up from a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="958a3-295">hello mappa elérési útját és nevét hello blob dinamikusan értékeli ki a rendszer által feldolgozott hello szelet hello kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="958a3-295">hello folder path and file name for hello blob are dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="958a3-296">hello mappa elérési útját használja év, hónap és nap részét hello kezdési ideje, valamint fájlnév hello kezdő időpontja óra részét hello.</span><span class="sxs-lookup"><span data-stu-id="958a3-296">hello folder path uses year, month, and day part of hello start time and file name uses hello hour part of hello start time.</span></span> <span data-ttu-id="958a3-297">"external": "true" beállítás arról értesíti az, hogy ez a táblázat külső toohello adat-előállító és hello adat-előállítóban tevékenység nem hozzák hello Data Factory szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="958a3-297">“external”: “true” setting informs hello Data Factory service that this table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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
<span data-ttu-id="958a3-298">Lásd: hello [Azure-Blob adatkészletet típus tulajdonságainak](data-factory-azure-blob-connector.md#dataset-properties) hello lista szakasza ehhez az adathalmaztípushoz által támogatott tulajdonságokról.</span><span class="sxs-lookup"><span data-stu-id="958a3-298">See hello [Azure Blob dataset type properties](data-factory-azure-blob-connector.md#dataset-properties) section for hello list of properties supported by this dataset type.</span></span>

<span data-ttu-id="958a3-299">**Az Azure SQL Database kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="958a3-299">**Azure SQL Database output dataset:**</span></span>

<span data-ttu-id="958a3-300">hello minta másolja át a "MyTable" Azure SQL-nevű tooa adattábla.</span><span class="sxs-lookup"><span data-stu-id="958a3-300">hello sample copies data tooa table named “MyTable” in Azure SQL.</span></span> <span data-ttu-id="958a3-301">Hello tábla létrehozása az Azure SQL azonos számú oszlopot hello hello Blob CSV-fájl toocontain várt.</span><span class="sxs-lookup"><span data-stu-id="958a3-301">Create hello table in Azure SQL with hello same number of columns as you expect hello Blob CSV file toocontain.</span></span> <span data-ttu-id="958a3-302">Új sorok hozzáadásakor toohello tábla óránként.</span><span class="sxs-lookup"><span data-stu-id="958a3-302">New rows are added toohello table every hour.</span></span>

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
<span data-ttu-id="958a3-303">Lásd: hello [Azure SQL dataset típustulajdonságokat](#dataset) ehhez az adathalmaztípushoz által támogatott tulajdonságokról szakasza hello listáját.</span><span class="sxs-lookup"><span data-stu-id="958a3-303">See hello [Azure SQL dataset type properties](#dataset) section for hello list of properties supported by this dataset type.</span></span>

<span data-ttu-id="958a3-304">**A másolási tevékenység során a Blob-forrás és fogadó SQL-feldolgozási folyamat:**</span><span class="sxs-lookup"><span data-stu-id="958a3-304">**A copy activity in a pipeline with Blob source and SQL sink:**</span></span>

<span data-ttu-id="958a3-305">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="958a3-305">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="958a3-306">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**BlobSource** és **fogadó** típusuk értéke túl**SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="958a3-306">In hello pipeline JSON definition, hello **source** type is set too**BlobSource** and **sink** type is set too**SqlSink**.</span></span>

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
<span data-ttu-id="958a3-307">Lásd: hello [Sql fogadó](#sqlsink) szakasz és [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) SqlSink és BlobSource által támogatott tulajdonságokról hello listáját.</span><span class="sxs-lookup"><span data-stu-id="958a3-307">See hello [Sql Sink](#sqlsink) section and [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) for hello list of properties supported by SqlSink and BlobSource.</span></span>

## <a name="identity-columns-in-hello-target-database"></a><span data-ttu-id="958a3-308">Azonosító oszlop hello céladatbázis</span><span class="sxs-lookup"><span data-stu-id="958a3-308">Identity columns in hello target database</span></span>
<span data-ttu-id="958a3-309">Ez a szakasz egy példát biztosít arra az adatok másolása a forrástábla egy azonosító oszlop tooa céltábla azonosító oszlopot tartalmazó nélkül.</span><span class="sxs-lookup"><span data-stu-id="958a3-309">This section provides an example for copying data from a source table without an identity column tooa destination table with an identity column.</span></span>

<span data-ttu-id="958a3-310">**Forrástábla:**</span><span class="sxs-lookup"><span data-stu-id="958a3-310">**Source table:**</span></span>

```SQL
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```
<span data-ttu-id="958a3-311">**Céltábla:**</span><span class="sxs-lookup"><span data-stu-id="958a3-311">**Destination table:**</span></span>

```SQL
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```
<span data-ttu-id="958a3-312">Figyelje meg, hogy hello céltábla tartalmaz azonosító oszlopot.</span><span class="sxs-lookup"><span data-stu-id="958a3-312">Notice that hello target table has an identity column.</span></span>

<span data-ttu-id="958a3-313">**Forrás adatkészlet JSON-definícióból**</span><span class="sxs-lookup"><span data-stu-id="958a3-313">**Source dataset JSON definition**</span></span>

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
<span data-ttu-id="958a3-314">**Cél adatkészlet JSON-definícióból**</span><span class="sxs-lookup"><span data-stu-id="958a3-314">**Destination dataset JSON definition**</span></span>

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

<span data-ttu-id="958a3-315">Figyelje meg, hogy a forrás és cél táblázatként különböző sémája (cél rendelkezik egy olyan további oszlop identitású).</span><span class="sxs-lookup"><span data-stu-id="958a3-315">Notice that as your source and target table have different schema (target has an additional column with identity).</span></span> <span data-ttu-id="958a3-316">Ebben az esetben szüksége toospecify **struktúra** hello tároló adatkészlet-definícióban, amely nem tartalmazza a hello azonosító oszlop tulajdonsága.</span><span class="sxs-lookup"><span data-stu-id="958a3-316">In this scenario, you need toospecify **structure** property in hello target dataset definition, which doesn’t include hello identity column.</span></span>

## <a name="invoke-stored-procedure-from-sql-sink"></a><span data-ttu-id="958a3-317">A fogadó SQL tárolt eljárás meghívása</span><span class="sxs-lookup"><span data-stu-id="958a3-317">Invoke stored procedure from SQL sink</span></span>
<span data-ttu-id="958a3-318">A folyamat a másolási tevékenység SQL fogadó egy tárolt eljárás végrehajtásával hívásának példáért lásd: [fogadó SQL tárolt eljárás meghívása a másolási tevékenység](data-factory-invoke-stored-procedure-from-copy-activity.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="958a3-318">For an example of invoking a stored procedure from SQL sink in a copy activity of a pipeline, see [Invoke stored procedure for SQL sink in copy activity](data-factory-invoke-stored-procedure-from-copy-activity.md) article.</span></span> 

## <a name="type-mapping-for-azure-sql-database"></a><span data-ttu-id="958a3-319">Írja be az Azure SQL Database leképezése</span><span class="sxs-lookup"><span data-stu-id="958a3-319">Type mapping for Azure SQL Database</span></span>
<span data-ttu-id="958a3-320">A hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk másolási tevékenység az automatikus típuskonverziók származó típusok toosink típusait a 2. lépés – a módszert követve hello hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="958a3-320">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article Copy activity performs automatic type conversions from source types toosink types with hello following 2-step approach:</span></span>

1. <span data-ttu-id="958a3-321">Natív típusok too.NET forrástípus konvertálása</span><span class="sxs-lookup"><span data-stu-id="958a3-321">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="958a3-322">.NET típusú toonative a fogadó típusa konvertálása</span><span class="sxs-lookup"><span data-stu-id="958a3-322">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="958a3-323">Ha adatok tooand tér át Azure SQL Database, hello következő megfeleltetéseket használ SQL too.NET típusának, és ez fordítva is igaz.</span><span class="sxs-lookup"><span data-stu-id="958a3-323">When moving data tooand from Azure SQL Database, hello following mappings are used from SQL type too.NET type and vice versa.</span></span> <span data-ttu-id="958a3-324">hello ugyanaz, mint az SQL Server adattípus-hozzárendelése az ADO.NET hello lesz.</span><span class="sxs-lookup"><span data-stu-id="958a3-324">hello mapping is same as hello SQL Server Data Type Mapping for ADO.NET.</span></span>

| <span data-ttu-id="958a3-325">SQL Server adatbázismotor típusa</span><span class="sxs-lookup"><span data-stu-id="958a3-325">SQL Server Database Engine type</span></span> | <span data-ttu-id="958a3-326">.NET-keretrendszer típusa</span><span class="sxs-lookup"><span data-stu-id="958a3-326">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="958a3-327">bigint</span><span class="sxs-lookup"><span data-stu-id="958a3-327">bigint</span></span> |<span data-ttu-id="958a3-328">Int64</span><span class="sxs-lookup"><span data-stu-id="958a3-328">Int64</span></span> |
| <span data-ttu-id="958a3-329">Bináris</span><span class="sxs-lookup"><span data-stu-id="958a3-329">binary</span></span> |<span data-ttu-id="958a3-330">Byte]</span><span class="sxs-lookup"><span data-stu-id="958a3-330">Byte[]</span></span> |
| <span data-ttu-id="958a3-331">bit</span><span class="sxs-lookup"><span data-stu-id="958a3-331">bit</span></span> |<span data-ttu-id="958a3-332">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="958a3-332">Boolean</span></span> |
| <span data-ttu-id="958a3-333">Karakter</span><span class="sxs-lookup"><span data-stu-id="958a3-333">char</span></span> |<span data-ttu-id="958a3-334">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="958a3-334">String, Char[]</span></span> |
| <span data-ttu-id="958a3-335">Dátum</span><span class="sxs-lookup"><span data-stu-id="958a3-335">date</span></span> |<span data-ttu-id="958a3-336">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="958a3-336">DateTime</span></span> |
| <span data-ttu-id="958a3-337">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="958a3-337">Datetime</span></span> |<span data-ttu-id="958a3-338">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="958a3-338">DateTime</span></span> |
| <span data-ttu-id="958a3-339">datetime2</span><span class="sxs-lookup"><span data-stu-id="958a3-339">datetime2</span></span> |<span data-ttu-id="958a3-340">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="958a3-340">DateTime</span></span> |
| <span data-ttu-id="958a3-341">datetimeoffset</span><span class="sxs-lookup"><span data-stu-id="958a3-341">Datetimeoffset</span></span> |<span data-ttu-id="958a3-342">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="958a3-342">DateTimeOffset</span></span> |
| <span data-ttu-id="958a3-343">Decimális</span><span class="sxs-lookup"><span data-stu-id="958a3-343">Decimal</span></span> |<span data-ttu-id="958a3-344">Decimális</span><span class="sxs-lookup"><span data-stu-id="958a3-344">Decimal</span></span> |
| <span data-ttu-id="958a3-345">A FILESTREAM attribútum (varbinary(max))</span><span class="sxs-lookup"><span data-stu-id="958a3-345">FILESTREAM attribute (varbinary(max))</span></span> |<span data-ttu-id="958a3-346">Byte]</span><span class="sxs-lookup"><span data-stu-id="958a3-346">Byte[]</span></span> |
| <span data-ttu-id="958a3-347">Lebegőpontos</span><span class="sxs-lookup"><span data-stu-id="958a3-347">Float</span></span> |<span data-ttu-id="958a3-348">Dupla</span><span class="sxs-lookup"><span data-stu-id="958a3-348">Double</span></span> |
| <span data-ttu-id="958a3-349">Kép</span><span class="sxs-lookup"><span data-stu-id="958a3-349">image</span></span> |<span data-ttu-id="958a3-350">Byte]</span><span class="sxs-lookup"><span data-stu-id="958a3-350">Byte[]</span></span> |
| <span data-ttu-id="958a3-351">int</span><span class="sxs-lookup"><span data-stu-id="958a3-351">int</span></span> |<span data-ttu-id="958a3-352">Int32</span><span class="sxs-lookup"><span data-stu-id="958a3-352">Int32</span></span> |
| <span data-ttu-id="958a3-353">pénz</span><span class="sxs-lookup"><span data-stu-id="958a3-353">money</span></span> |<span data-ttu-id="958a3-354">Decimális</span><span class="sxs-lookup"><span data-stu-id="958a3-354">Decimal</span></span> |
| <span data-ttu-id="958a3-355">nchar</span><span class="sxs-lookup"><span data-stu-id="958a3-355">nchar</span></span> |<span data-ttu-id="958a3-356">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="958a3-356">String, Char[]</span></span> |
| <span data-ttu-id="958a3-357">ntext</span><span class="sxs-lookup"><span data-stu-id="958a3-357">ntext</span></span> |<span data-ttu-id="958a3-358">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="958a3-358">String, Char[]</span></span> |
| <span data-ttu-id="958a3-359">Numerikus</span><span class="sxs-lookup"><span data-stu-id="958a3-359">numeric</span></span> |<span data-ttu-id="958a3-360">Decimális</span><span class="sxs-lookup"><span data-stu-id="958a3-360">Decimal</span></span> |
| <span data-ttu-id="958a3-361">nvarchar</span><span class="sxs-lookup"><span data-stu-id="958a3-361">nvarchar</span></span> |<span data-ttu-id="958a3-362">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="958a3-362">String, Char[]</span></span> |
| <span data-ttu-id="958a3-363">valós</span><span class="sxs-lookup"><span data-stu-id="958a3-363">real</span></span> |<span data-ttu-id="958a3-364">Egyetlen</span><span class="sxs-lookup"><span data-stu-id="958a3-364">Single</span></span> |
| <span data-ttu-id="958a3-365">ROWVERSION</span><span class="sxs-lookup"><span data-stu-id="958a3-365">rowversion</span></span> |<span data-ttu-id="958a3-366">Byte]</span><span class="sxs-lookup"><span data-stu-id="958a3-366">Byte[]</span></span> |
| <span data-ttu-id="958a3-367">smalldatetime</span><span class="sxs-lookup"><span data-stu-id="958a3-367">smalldatetime</span></span> |<span data-ttu-id="958a3-368">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="958a3-368">DateTime</span></span> |
| <span data-ttu-id="958a3-369">smallint</span><span class="sxs-lookup"><span data-stu-id="958a3-369">smallint</span></span> |<span data-ttu-id="958a3-370">Int16</span><span class="sxs-lookup"><span data-stu-id="958a3-370">Int16</span></span> |
| <span data-ttu-id="958a3-371">kis pénz típusú értéknél</span><span class="sxs-lookup"><span data-stu-id="958a3-371">smallmoney</span></span> |<span data-ttu-id="958a3-372">Decimális</span><span class="sxs-lookup"><span data-stu-id="958a3-372">Decimal</span></span> |
| <span data-ttu-id="958a3-373">sql_variant</span><span class="sxs-lookup"><span data-stu-id="958a3-373">sql_variant</span></span> |<span data-ttu-id="958a3-374">Objektum *</span><span class="sxs-lookup"><span data-stu-id="958a3-374">Object *</span></span> |
| <span data-ttu-id="958a3-375">Szöveg</span><span class="sxs-lookup"><span data-stu-id="958a3-375">text</span></span> |<span data-ttu-id="958a3-376">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="958a3-376">String, Char[]</span></span> |
| <span data-ttu-id="958a3-377">time</span><span class="sxs-lookup"><span data-stu-id="958a3-377">time</span></span> |<span data-ttu-id="958a3-378">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="958a3-378">TimeSpan</span></span> |
| <span data-ttu-id="958a3-379">időbélyeg</span><span class="sxs-lookup"><span data-stu-id="958a3-379">timestamp</span></span> |<span data-ttu-id="958a3-380">Byte]</span><span class="sxs-lookup"><span data-stu-id="958a3-380">Byte[]</span></span> |
| <span data-ttu-id="958a3-381">tinyint</span><span class="sxs-lookup"><span data-stu-id="958a3-381">tinyint</span></span> |<span data-ttu-id="958a3-382">Bájt</span><span class="sxs-lookup"><span data-stu-id="958a3-382">Byte</span></span> |
| <span data-ttu-id="958a3-383">egyedi azonosító</span><span class="sxs-lookup"><span data-stu-id="958a3-383">uniqueidentifier</span></span> |<span data-ttu-id="958a3-384">GUID</span><span class="sxs-lookup"><span data-stu-id="958a3-384">Guid</span></span> |
| <span data-ttu-id="958a3-385">varbinary</span><span class="sxs-lookup"><span data-stu-id="958a3-385">varbinary</span></span> |<span data-ttu-id="958a3-386">Byte]</span><span class="sxs-lookup"><span data-stu-id="958a3-386">Byte[]</span></span> |
| <span data-ttu-id="958a3-387">varchar</span><span class="sxs-lookup"><span data-stu-id="958a3-387">varchar</span></span> |<span data-ttu-id="958a3-388">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="958a3-388">String, Char[]</span></span> |
| <span data-ttu-id="958a3-389">xml</span><span class="sxs-lookup"><span data-stu-id="958a3-389">xml</span></span> |<span data-ttu-id="958a3-390">XML</span><span class="sxs-lookup"><span data-stu-id="958a3-390">Xml</span></span> |

## <a name="map-source-toosink-columns"></a><span data-ttu-id="958a3-391">A forrásoszlopokat toosink leképezése</span><span class="sxs-lookup"><span data-stu-id="958a3-391">Map source toosink columns</span></span>
<span data-ttu-id="958a3-392">toolearn leképezési oszlopok az forrás adatkészlet toocolumns fogadó adatkészletben, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="958a3-392">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-copy"></a><span data-ttu-id="958a3-393">Ismételhető másolása</span><span class="sxs-lookup"><span data-stu-id="958a3-393">Repeatable copy</span></span>
<span data-ttu-id="958a3-394">Adatok tooSQL Server-adatbázis másolásakor hello másolási tevékenység hozzáfűzi toohello fogadó adattábla alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="958a3-394">When copying data tooSQL Server Database, hello copy activity appends data toohello sink table by default.</span></span> <span data-ttu-id="958a3-395">egy UPSERT tooperform helyett, lásd: [ismételhető írási tooSqlSink](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) cikk.</span><span class="sxs-lookup"><span data-stu-id="958a3-395">tooperform an UPSERT instead,  See [Repeatable write tooSqlSink](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) article.</span></span> 

<span data-ttu-id="958a3-396">Amikor az adatok másolása relációs adattároló, tartsa ismételhetőség szem előtt tartva tooavoid nem kívánt eredmények.</span><span class="sxs-lookup"><span data-stu-id="958a3-396">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="958a3-397">Az Azure Data Factoryben futtathatja a szelet manuálisan.</span><span class="sxs-lookup"><span data-stu-id="958a3-397">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="958a3-398">Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="958a3-398">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="958a3-399">A szelet akkor fut újra, vagy módon, ha van szüksége arról, hogy ugyanazokat az adatokat hello toomake hogyan olvasható függetlenül attól, hogy hányszor a szelet futtatása.</span><span class="sxs-lookup"><span data-stu-id="958a3-399">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="958a3-400">Lásd: [relációs források olvasni Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="958a3-400">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="958a3-401">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="958a3-401">Performance and Tuning</span></span>
<span data-ttu-id="958a3-402">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.</span><span class="sxs-lookup"><span data-stu-id="958a3-402">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
